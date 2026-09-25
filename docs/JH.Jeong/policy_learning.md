# Track B Policy Learning Specification

> **문서 역할**: Track B actor observation의 합의된 명세와 이후 policy 설계의 논의 기록을 구분한다.
>
> **현재 합의 범위**: Intro와 actor Observation v0.3까지
>
> **협의 중**: Policy architecture, action/controller, reward, termination, critic·RL 세부 방법, evaluation과 experiment/ablation
>
> **최종 갱신**: 2026-09-24

---

## 0. 이 문서를 읽는 방법

- [`README.md`](README.md): 전체 문서의 권장 독해 순서
- [`research_topic.md`](research_topic.md): 현재까지 구체화된 연구 주제와 전체 시스템 정의
- [`Intro/`](Intro/README.md): 연구 필요성, research trend, previous works와 candidate contribution
- [`papers/`](papers/README.md): 관련 논문 목록과 근거 검토
- [`context.md`](context.md): 논의 흐름, 결정 변경, 결정 근거의 기록
- **이 문서**: actor observation 명세와 아직 합의되지 않은 후속 설계 메모

현재 합의된 구체 범위는 actor observation이다. Observation을 설명하기 위해 task와 action 후보를 함께 기록하지만, **policy architecture, action/controller, reward·termination·critic·evaluation·experiment는 채택된 명세나 계획이 아니다.** 9–11절은 이후 협의를 위해 보존한 `[Open]` 메모다.

이 문서는 다음 순서로 읽는다.

1. **1절:** policy가 해결할 task와 action의 의미
2. **2–4절:** actor가 무엇을 보고 각 신호를 어떻게 표현하는지
3. **5–6절:** observation을 실물과 맞추기 위한 전처리와 정보 경계
4. **7절:** observation에 관해 검토할 수 있는 대안 — 비교 방식 미정
5. **8절:** observation interface의 구현 확인 항목
6. **9–11절:** privileged information과 reward 등에 관한 협의 전 메모

### 0.1 현재 학습 명세 요약

| 항목 | 현재 정리 | 상태 |
| --- | --- | --- |
| Policy | Shared/phase-specific 구조와 phase 정보 사용 여부 | `[Open]` |
| Actor observation | EEF-relative target position·push direction·selected-face normal, current object pose, OBB, arm·hand q, current 17D tactile, current wrist F/T, previous action | `[Baseline]`, 66D |
| Action | EEF-frame delta translation·rotation + hand action을 후보로 기록 | `[Open]` |
| Task sequence | Approach / Contact Formation → Rotation / Pivoting → Push | Intro의 문제 구조; policy organization은 `[Open]` |
| Approach objective | 후속 Rotation→Push에 적합한 wrist–hand configuration과 contact state 형성 | Intro의 연구 질문; 학습 방식은 `[Open]` |
| Environment | 주변 물체가 있는 intended setting과 단순화 조건 | 수·배치·관측·접촉 규칙 및 사용 방식 `[Open]` |
| Privileged information | Exact object/contact/collision/support/effort state 후보 | Actor observation에서 제외; 구체 용도 `[Open]` |
| Reward | 문헌에서 검토한 task·contact·safety signal 후보 | `[Open]` |

이 문서에서 `[Baseline]`은 현재 합의된 observation 안에만 사용한다. 9–11절의 후보는 모두 `[Open]`이며, RL algorithm·critic 입력·reward·evaluation·experiment는 아직 합의하지 않았다.

---

## 1. 현재 학습 문제의 전제

Actor observation을 정의하려면 policy가 다룰 task와 action의 의미를 함께 적어야 한다. Task 구조는 Intro를 따르며, action과 policy organization의 세부안은 observation 차원을 해석하기 위한 현재 전제일 뿐 아직 합의된 method가 아니다.

### 1.1 Policy 구조 — `[Open]`

- 하나의 shared MLP, phase ID가 있는 shared policy와 phase-specific policy/controller가 논의 후보로 남아 있다.
- 현재 66D observation에는 Phase ID를 제공하지 않는다. 향후 observation을 바꾸어 Phase ID를 포함할지, reward gate와 hard state machine을 사용할지는 정하지 않았다.
- 따라서 shared phase-free 구성을 현재 baseline이나 contribution으로 간주하지 않는다.

### 1.2 Task goal의 의미

현재 observation에서 표현하는 goal은 물체를 임의의 목표 자세에 놓는 것이 아니라, **선택된 OBB 면과 지정 push direction·target position의 관계**다. Rotation/Pivoting은 선택 면의 pushing normal을 지정 방향과 정렬하는 준비 과정으로 설명한다. 목표 orientation quaternion은 현재 observation에 넣지 않으며, 별도 desired rotation/orientation의 필요 여부와 판단 방식은 협의 중이다.

Approach / Contact Formation의 목적도 단순히 물체에 가까워지거나 최초 접촉을 만드는 것이 아니다. **후속 Rotation과 Push를 연속적으로 수행할 수 있는 wrist–hand configuration과 hand–object contact state를 형성하는 것**이 Intro에서 정리한 task-level 질문이다. 최초 접촉 이후 aggregate contact를 선호할지, contact migration·release·re-contact와 hand reconfiguration을 어느 범위까지 허용할지는 아직 합의하지 않았다.

목표 물체 위치는 task/world frame에서 다음과 같이 정의한다.

$$
\mathbf{p}_{O,g}^{W}
=
\mathbf{p}_{O,0}^{W}
+
s_{\mathrm{push}}\mathbf{d}_{\mathrm{push}}^{W}
$$

현재 observation 전제에서 상위 모듈은 blocker, 목표 push direction·distance와 사용할 OBB lateral face를 제공한다. Object-local outward normal을 $\mathbf n_{f,\mathrm{out}}^O$라 할 때, 선택 면에서 물체 안쪽을 향하는 inward pushing normal은 $\mathbf n_f^O=-\mathbf n_{f,\mathrm{out}}^O$로 정의한다. 정책은 목표 위치, push direction과 현재 선택 면의 pushing normal을 EEF frame으로 받는다. Push direction은 목표 병진 방향이고, pushing normal은 coarse geometry의 방향이며, 실제 접촉력 방향은 접촉과 마찰에 의해 결정되는 별도 물리량이다.

Task는 hand–object non-contact 상태에서 시작한다. 초기 wrist–hand pose와 blocker pose의 sampling 규칙, Rotation 중 병진 허용 범위, safety 처리와 주변 물체 조건은 아직 협의 중이다. Object 바닥과 shelf support plane의 정상적인 지지 접촉이 task dynamics에 필요하다는 점만 유지하며, auxiliary fixed structure와 movable object의 접촉 규칙은 정하지 않았다.

현재 observation에서는 선택 면 identity가 episode 동안 유지된다고 가정하고, 초기 OBB frame과 object pose tracking으로 같은 object-local face label의 방향을 갱신한다. Face 선택은 상위 task generator의 역할로 두며, 세부 tracking 규칙은 협의 중이다.

![5-finger hand가 blocker 후면 모서리에 hook contact를 형성해 회전시키고, 선택 면 normal을 CoM에서 표시한 목표 push direction에 정렬한 뒤 손바닥 접촉으로 밀어 target 접근 공간을 여는 예시](assets/approach-rotation-push.svg)

**Figure — Hook contact에서 palm pushing으로 이어지는 동작 예시.** 세 패널에서 target, surrounding objects와 shelf pillar의 위치는 같고, 조작 대상인 blocker와 robot hand만 움직인다. Approach는 비접촉 시작 이후 후면 모서리에 손가락을 걸어 접촉을 형성한 시점을, Rotation은 hook contact로 물체를 돌려 선택 면을 push direction에 정렬한 시점을 보여준다. Push에서는 넓은 손바닥 접촉으로 전환해 정렬을 유지하며 blocker를 이동시킨다. 여기서 후면은 목표 push direction의 반대쪽 측면을 뜻한다. 초록색 영역은 blocker 이동으로 열린 접근 공간이며, target retrieval 자체는 현재 low-level policy의 수행 범위에 포함하지 않는다.

회색 평면은 측벽 없는 shelf deck이며 네 모서리의 원형 부품은 pillar다. 파란 손목과 흰색 하우징·검은 손가락은 UR5e–RH56E2의 개념도다. [RH56E2 공식 제품 자료](https://en.inspire-robots.com/product/rh56e2/)를 참고해 네 손가락의 링크·관절과 별도의 엄지를 구분했으며, 손은 접촉 관계를 설명하기 위한 도식적 투영으로 표현했다. 실제 CAD/URDF 렌더링이나 관절각의 정확한 투영은 아니다. Hook-to-palm 순서는 접촉 변화의 예시이며, 정확한 hand pose의 실행 가능성과 효과는 아직 확인되지 않았다. 모든 손가락이 물체에 닿는 배치를 의미하지 않으며, 특정 hook이나 다섯 손가락의 동시 접촉을 reward의 필수 조건으로 확정하지 않는다. Object–support-plane 접촉은 항상 허용한다. 그림의 pillar와 surrounding objects는 intended S1을 설명하지만, auxiliary fixed structure와 movable object의 접촉 규칙 및 수·배치 randomization은 아직 확정하지 않았다.

주황색 점선은 approximate OBB, 굵은 주황색 선은 episode 동안 동일하게 추적하는 selected face다. 주황색 화살표는 이 면의 **inward pushing normal**, 파란색 화살표는 **commanded push direction**이다. 그림에서 파란 화살표는 물체의 **CoM에서 시작**해 목표 병진 방향을 나타낸다. 방향 자체는 상위 task goal로 정해지며 CoM 위치만으로 결정되지 않는다. 그림의 균일한 상자에서는 설명을 위해 CoM과 기하 중심을 일치시켰으나, 일반 물체의 CoM은 OBB 중심과 다를 수 있다. 실제 CoM은 actor observation에 추가하지 않고 기존 privileged-information 구분을 유지한다.

두 화살표는 실제 contact-force 측정값을 뜻하지 않으며 selected face도 정확한 mesh contact patch가 아닌 coarse task label이다. 선택 면의 outward normal 반대 방향을 $\mathbf n_{f,t}^{E}$로 정의하면 Rotation의 정렬 기준은

$$
c_{\mathrm{align},t}
=
(\mathbf n_{f,t}^{E})^{\mathsf T}\mathbf d_{\mathrm{push},t}^{E}
=
\cos e_{\mathrm{align},t}
\rightarrow 1
$$

이다. 이는 $e_{\mathrm{align},t}\rightarrow0$과 같으며, 두 unit vector가 **같은 방향**을 향하도록 정렬한다는 뜻이다. Figure의 평면상 화살표는 공통 shelf/task frame으로 그렸고, actor에서는 두 벡터를 매 step 현재 EEF frame으로 변환한다. 같은 회전 변환을 적용하므로 두 벡터의 내적과 정렬각은 변하지 않는다. Push는 이 정렬 관계를 이용해 목표 위치로 병진하는 과정이며, aggregate contact를 필수로 유지할지와 contact 전환 규칙은 `[Open]`이다.

그림은 이해를 위한 세 snapshot이며 **CoM을 중심으로 회전하도록 요구하지 않는다.** CoM은 목표 병진 방향 화살표를 표시하는 기준점일 뿐 회전축이 아니다. Rotation의 목적은 selected-face normal과 push direction의 정렬이며, CoM 고정이나 지정 pivot 경로 추종을 전제하지 않는다. 접촉 상태에 따라 회전 중심이 달라지거나 이동할 수 있다. 이를 나타내기 위해 Rotation 패널은 초기 점선 OBB와 비교해 orientation과 중심 위치가 모두 변한 snapshot으로 그렸다. 특정 고정 pivot이나 고정 hand pose를 지정한 궤적은 아니다. Initial alignment가 이미 적합하면 Rotation을 생략할 수 있다. 세 단계는 task-level progress 순서이며 phase ID 제공이나 hard action switching을 의미하지 않는다. Rotation 중 병진·contact migration의 허용 범위와 alignment success criterion은 협의 중이다.

면 정렬은 병진을 위한 기하학적 조건이며, 순수 병진을 보장하는 충분조건은 아니다. 접촉력 $\mathbf f_i$가 CoM에 만드는 모멘트는 $(\mathbf p_i-\mathbf p_{\mathrm{CoM}})\times\mathbf f_i$이므로, 여러 finger·palm 접촉의 힘 분배와 shelf의 지지·마찰 반력까지 함께 고려해야 한다. [Adaptive Reaching and Pushing](https://doi.org/10.3389/fnbot.2023.1271607)의 §3.3은 목표 방향에 대한 force alignment와 CoM에 대한 force-line lever arm 감소를 별도로 보상한다. 이 관점을 Track B의 force shaping이나 evaluation에 사용할지는 `[Open]`이다.

### 1.3 Action 후보와 previous action observation

66D observation의 `previous action` 12차원을 해석하기 위해 아래 action 후보를 전제로 기록한다. 최종 action과 controller가 합의된 것은 아니다.

$$
\mathbf{a}_t
=
[\Delta\mathbf{p}^{E}_t,\;\Delta\boldsymbol{\phi}^{E}_t,\;\mathbf{a}^{H}_t]
\in \mathbb{R}^{12}
$$

- EEF-frame delta translation: 3D
- EEF-frame local rotation increment: 3D
- RH56E2 hand joint action: 6D

현재 action 후보에서는 매 policy step의 **측정된 현재 EEF 상태**를 기준으로 DiffIK 또는 OSC의 새 목표를 만드는 방식을 고려한다. 이전 desired target에 delta를 누적할지와 controller target state가 추가로 필요한지는 action/controller 논의에서 정한다.

---

## 2. Observation 설계 원칙

위 task를 수행하려면 actor가 목표, 현재 물체–손 관계, robot configuration과 실제 접촉 반응을 구분해서 보아야 한다. 동시에 simulation에서만 얻을 수 있는 정확한 정보에 의존해서는 안 된다. 이 두 요구를 다음 원칙으로 구체화한다.

1. **실물에서 얻을 수 있는 actor input만 사용한다.** 시뮬레이터의 정확한 contact force, collider identity, 물체 동역학 파라미터 등은 actor에게 노출하지 않는다.
2. **대부분의 task-space 정보를 EEF frame으로 통일한다.** 로봇과 물체가 함께 이동할 때 불필요한 world-coordinate 의존성을 줄인다.
3. **Task command와 current state를 구분한다.** 목표 위치·push direction은 command이고, 선택 면 normal·현재 object pose는 그 명령에 대한 현재 상태다.
4. **현재 actor observation에 필요한 시간 정보는 vector에 명시적으로 펼쳐 넣는다.** 특정 network memory가 있다고 가정하지 않는다.
5. **현재값으로 충분한 센서에는 history를 관성적으로 추가하지 않는다.** Binary tactile과 wrist F/T는 current observation으로 시작하고, 직전 action만 1-step 제공한다.
6. **필요성이 확인되지 않은 파생 정보는 먼저 제외한다.** 추가 필요성이 논의될 때 다시 검토한다.
7. **actor observation과 reward용 privileged information의 경계를 유지한다.** reward가 더 정확한 시뮬레이션 정보를 사용하더라도 actor가 그것에 의존하지 않도록 한다.

---

## 3. Observation v0.3

2절의 원칙을 실제 66D tensor로 고정한 결과가 아래 표다. 이 절은 **무엇을 넣는지**를 보여주고, 4절은 **각 값을 어떻게 계산하는지**를 설명한다.

### 3.1 전체 구성표

| ID | Observation | 실물 정보원 | Frame·표현 | 시간 범위 | 차원 | 상태 및 포함 이유 |
|---|---|---|---|---:|---:|---|
| O1 | 목표 물체 position $\mathbf{p}_{O,g,t}^{E}$ | 상위 task command + EEF pose | EEF frame, Cartesian | 현재 | 3 | **[Baseline]**. 최종 이동 위치와 lateral deviation 판단 |
| O2 | 목표 push direction $\mathbf{d}_{\mathrm{push},t}^{E}$ | 상위 task command + EEF pose | EEF frame, unit vector | 현재 | 3 | **[Baseline]**. Task/world frame에서 고정한 목표 이동 방향·면 정렬 기준을 매 step EEF frame으로 변환 |
| O3 | 선택 면의 현재 pushing normal $\mathbf{n}_{f,t}^{E}$ | 선택 face + object pose + EEF pose | EEF frame, inward unit normal | 현재 | 3 | **[Baseline]**. Rotation alignment와 Push 중 정렬 유지 판단 |
| O4 | 현재 object position $\mathbf{p}_{EO,t}^{E}$ | vision tracker | EEF frame, Cartesian | 현재 | 3 | **[Baseline]**. 현재 object–EEF 관계 |
| O5 | 현재 object orientation $\overline{\mathbf{q}}_{EO,t}$ | vision tracker | EEF frame, canonical unit quaternion | 현재 | 4 | **[Baseline]**. 현재 3D 자세·tilt와 OBB 축 방향 표현 |
| O6 | Object OBB extent $\mathbf{d}_{O}$ | 초기 perception/template | object-local $(l,w,h)$ | episode 고정 | 3 | **[Baseline]**. point cloud 없이 물체 크기 차이를 제공 |
| O7 | UR5e joint position $\mathbf{q}^{A}_t$ | joint encoder | joint coordinates | 현재 | 6 | **[Baseline]**. arm configuration과 관절 한계·특이점 관련 상태 |
| O8 | RH56E2 actuated joint position $\mathbf{q}^{H}_t$ | joint encoder | joint coordinates | 현재 | 6 | **[Baseline]**. hand configuration과 접촉 형상 상태 |
| O9 | Binary tactile $\mathbf{b}_t$ | RH56E2 tactile sensor | sensor index가 고정된 17D on/off vector | 현재 | 17 | **[Baseline]**. 실물과 simulation의 접촉 표현 차이를 줄이면서 접촉 위치 identity를 보존 |
| O10 | Wrist wrench $\widetilde{\mathbf{w}}_t$ | wrist F/T sensor | EEF frame, force 3 + torque 3 | 현재 | 6 | **[Baseline]**. 현재 하중 방향·크기·moment 제공 |
| O11 | Previous action $\mathbf{a}_{t-1}$ | policy output log | 1.3절과 같은 정규화 표현 | 직전 1 step | 12 | **[Baseline]**. 현재 12D action 후보를 전제로 하며, 최종 action이 바뀌면 차원을 함께 다시 확인 |

Tactile 17개 channel의 순서는 RH56E2 sensor index와 simulation의 대응 sensor region 사이에서 고정한다. Coarse pooling은 현재 observation에 포함하지 않고 대안으로만 남긴다.

### 3.2 Actor 입력 순서와 차원

배치 내부의 observation 순서는 다음과 같이 고정한다.

$$
\begin{aligned}
\mathbf{o}_t = [&\mathbf{p}_{O,g,t}^{E},
\mathbf{d}_{\mathrm{push},t}^{E},
\mathbf{n}_{f,t}^{E},
\mathbf{p}_{EO,t}^{E},
\overline{\mathbf{q}}_{EO,t},
\mathbf{d}_{O},\\
&\mathbf{q}^{A}_t,
\mathbf{q}^{H}_t,
\mathbf{b}_t,
\widetilde{\mathbf{w}}_t,
\mathbf{a}_{t-1}]
\end{aligned}
$$

전체 입력 차원은 다음과 같다.

$$
D_{\mathrm{obs}}
=
31 + 17 + 6 + 12
=66
$$

여기서 31D는 task command·selected-face state 9D, current object pose 7D, OBB extent 3D, arm q 6D와 hand q 6D의 합이다. Tactile·wrench history는 observation 차원을 늘리지 않으며 previous action은 직전 1-step만 사용한다.

---

## 4. Observation 항목별 구현 명세

### 4.1 Task command, selected-face state와 current object pose

World/task frame에서 고정된 목표 위치와 push direction을 **매 policy step 현재 EEF frame으로 다시 표현**한다.

$$
\mathbf{p}_{O,g,t}^{E}
=
\mathbf{R}_{WE,t}^{\mathsf{T}}
(\mathbf{p}_{O,g}^{W}-\mathbf{p}_{WE,t}^{W})
$$

$$
\mathbf{d}_{\mathrm{push},t}^{E}
=
\mathbf{R}_{WE,t}^{\mathsf{T}}\mathbf{d}_{\mathrm{push}}^{W}
$$

현재 object pose와 선택 면의 pushing normal은 다음과 같이 계산한다.

$$
\mathbf{p}_{EO,t}^{E}
=
\mathbf{R}_{WE,t}^{\mathsf{T}}
(\mathbf{p}_{WO,t}^{W}-\mathbf{p}_{WE,t}^{W}),
\qquad
\mathbf{q}_{EO,t}
=
(\mathbf{q}_{WE,t})^{-1}\otimes\mathbf{q}_{WO,t}
$$

$$
\mathbf{n}_{f,t}^{E}
=
\mathbf{R}_{WE,t}^{\mathsf{T}}
\mathbf{R}_{WO,t}\mathbf{n}_{f}^{O},
\qquad
\mathbf{n}_{f}^{O}=-\mathbf{n}_{f,\mathrm{out}}^{O}.
$$

EEF-relative command를 episode 시작 시 한 번 계산한 뒤 고정하면 EEF 이동과 함께 world상의 목표가 변하는 잘못된 문제가 생긴다. 고정되는 것은 $\mathbf{p}_{O,g}^{W}$, $\mathbf{d}_{\mathrm{push}}^{W}$와 object-local face identity이며, EEF 표현은 계속 갱신한다.

목표 position과 push direction을 모두 주는 이유는 역할이 다르기 때문이다. 목표 position은 최종 도달과 lateral correction을 정의하고, push direction은 물체가 경로에서 벗어나도 유지해야 하는 원래 경로축과 면 정렬 기준을 보존한다. 선택 면 normal은 quaternion과 extent만으로는 알 수 없는 `어느 면을 사용할 것인가`를 actor에 직접 전달한다.

현재 observation은 목표 quaternion 대신 선택 면의 pushing normal과 push direction을 제공한다. EEF-frame quaternion은 current object pose 표현이며, task가 임의의 full SO(3) 목표 자세 제어를 요구한다는 뜻은 아니다. 이 관계를 reward나 evaluation에 어떻게 사용할지는 협의 중이다.

Quaternion은 $\mathbf{q}$와 $-\mathbf{q}$가 같은 회전을 나타내므로 raw tracker output을 그대로 넣지 않는다. Unit normalization 후 scalar component가 음수이면 부호를 뒤집는 canonicalization을 적용한다.

$$
\overline{\mathbf{q}}
=
\begin{cases}
\mathbf{q}/\|\mathbf{q}\|, & q_s\ge0\\
-\mathbf{q}/\|\mathbf{q}\|, & q_s<0
\end{cases}
$$

현재 [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831) math API의 `quat_unique`도 같은 방식으로 quaternion의 real part를 non-negative로 표준화한다. 다만 Isaac Lab version에 따라 serialized component order가 `(w,x,y,z)` 또는 `(x,y,z,w)`일 수 있으므로, policy tensor의 순서는 설치 version을 확인한 뒤 sim–real adapter 양쪽에서 명시적으로 고정한다. Quaternion noise는 네 component에 독립적인 additive noise를 넣기보다 작은 rotation quaternion을 곱하는 방식으로 적용한다.

[Isaac Lab Gear Assembly sim-to-real example](https://isaac-sim.github.io/IsaacLab/v3.0.0-beta2/source/policy_deployment/02_gear_assembly/gear_assembly_policy.html)은 vision에서 얻은 shaft orientation을 4D quaternion으로 policy에 직접 제공한다. 따라서 4D quaternion을 현재 observation 표현으로 사용한다. $180^\circ$ 부근의 canonical-sign 경계가 문제가 되면 [Zhou et al.의 rotation 6D](https://doi.org/10.1109/CVPR.2019.00589)를 대안으로 검토할 수 있다. Quaternion의 raw Euclidean 차이를 reward에 사용할지는 이 observation 결정과 별개의 문제다.

### 4.2 Object geometry: OBB extent

- actor에는 raw RGB, point cloud, mesh를 넣지 않는다.
- OBB extent $(l,w,h)$는 object-local canonical axes 기준으로 정렬한다.
- episode 시작 시 선택한 template의 축과 extent를 고정하고, 이후에는 pose만 tracking한다.
- 현재 object orientation O5가 object-local 축의 EEF-frame 방향을 제공하더라도 선택 face identity는 알 수 없으므로, 선택 면의 current pushing normal O3는 task-conditioned state로 명시한다. 다른 OBB axis vector나 corner는 추가하지 않는다.
- 선택 face는 초기 OBB의 lateral face 중 하나로 제한하고 episode 동안 같은 object-local face label을 추적한다. 매 frame OBB fitting 결과의 axis permutation·sign으로 face identity를 다시 정하지 않는다.
- OBB face는 실제 mesh surface patch가 아니라 접근할 물체 측면과 pushing normal을 지정하는 coarse task label이다. Actor는 실제 접촉과의 차이를 tactile·wrist F/T로 관측하며, 정확한 접촉 정보를 별도 evaluation에 사용할지는 `[Open]`이다.
- 대칭 물체는 symmetry-equivalent orientation 중 이전 추정과 가장 연속적인 해를 택한다. Quaternion canonicalization 자체가 OBB axis swap 문제를 해결하지는 않는다.

Point cloud 기반 정책인 [CORN](https://doi.org/10.48550/arXiv.2403.10760)과 [HACMan](https://doi.org/10.48550/arXiv.2305.03942)은 geometry와 contact/action 위치의 결합이 핵심이다. 본 연구는 가려짐이 큰 shelf 환경과 실물 추정 안정성을 우선하여 OBB까지만 사용하며, surface-level geometry awareness를 주장하지 않는다.

### 4.3 Arm·hand proprioception

- UR5e의 measured joint position 6D와 RH56E2의 actuated joint position 6D를 포함한다.
- joint velocity는 초기 observation에서 제외한다.
- absolute EEF pose/twist와 fingertip·pad center position은 joint state 및 EEF-relative task state와 중복될 수 있어 제외한다.
- Hand joint history 또는 velocity는 현재 observation에서 제외하며, 필요성은 이후 별도로 검토한다.

### 4.4 Binary tactile

Actor의 tactile은 **접촉 상대를 구분하지 않는 any-contact 신호**이다. 물체, 선반 또는 다른 collider와 접촉해도 같은 region의 contact는 1이다. 접촉 상대의 identity는 actor observation에서 제외하며, 이후 reward/termination 등에 사용할지는 협의 중이다.

각 sensor region의 contact-force magnitude $f_{i,t}$는 하나의 activation threshold $\tau_{\mathrm{tac}}$로 이진화한다.

$$
b_{i,t}=\mathbb{1}\!\left[f_{i,t}\ge \tau_{\mathrm{tac}}\right]
$$

- On/off에 서로 다른 threshold를 두거나 직전 binary state를 유지하는 hysteresis는 구현하지 않는다.
- $\tau_{\mathrm{tac}}$의 수치는 아직 확정하지 않는다. [DexTouch](https://doi.org/10.1109/LRA.2024.3478571), [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036)과 [VTDexManip](https://openreview.net/forum?id=jf7C7EGw21)의 simulation 설정인 $0.01\,\mathrm{N}$은 초기 참고값이며, 최종값은 [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831) contact 출력과 실제 RH56E2 신호 분포를 확인한 뒤 정한다.
- 실물 sensor가 내부적으로 filtering된 신호를 제공하는지 확인하되, policy용 binary 변환 자체에는 시간 상태를 갖는 hysteresis를 추가하지 않는다.
- Taxel마다 별도의 rigid collision body를 만드는 방식은 현재 observation 구현에서 제외한다.
- URDF의 접촉 가능 link/pad 또는 고정된 sensor region을 실물 17채널과 일대일 대응시켜 $\mathbf{b}_t\in\{0,1\}^{17}$을 만든다.
- **17채널 current vector를 현재 observation**으로 사용하고, coarse pooling은 검토 가능한 대안으로 남긴다.
- Sensor index 순서는 episode나 contact 상대에 따라 바뀌지 않는다.

Binary contact와 force sensing을 정책 입력으로 사용하는 선행 사례는 [DexTouch](https://doi.org/10.1109/LRA.2024.3478571), [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036), [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236)을 참고한다. [DexTouch](https://doi.org/10.1109/LRA.2024.3478571)는 16D current binary tactile를 history 없이 사용했고, binary signal을 sim–real 차이를 줄이기 위한 표현으로 채택해 zero-shot real transfer를 보였다. 실물 평가에서 tactile를 비활성화한 정책보다 full tactile policy의 성공률이 크게 높았지만, 이는 **binary가 continuous tactile보다 우월하다는 단독 인과 증거**가 아니라 tactile feedback과 sim–real-compatible representation의 결합 효과로 해석한다. 반대로 [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036)은 4-step state stack을 사용했으므로 tactile history가 항상 불필요하다고 일반화하지 않는다.

### 4.5 Wrist F/T

Wrist wrench는 bias를 제거하고 EEF frame으로 표현한다.

$$
\widetilde{\mathbf{w}}_t^E
=
[\widetilde{\mathbf{f}}_t^E,\;\widetilde{\boldsymbol{\tau}}_t^E]
\in\mathbb{R}^{6}
$$

Binary tactile은 접촉 위치를 알려주지만 접촉의 강도와 방향을 잃는다. Wrist F/T는 전체 하중의 방향·크기·moment를 제공하지만 어느 tactile region에 힘이 작용했는지는 알려주지 못한다. 두 modality가 서로 다른 정보를 제공한다는 것이 현재 observation에 둘 다 포함한 이유다. 실제 성능상 상보성과 그 확인 방식은 아직 협의 중이다.

정확한 gravity/inertia compensation, clipping, filtering, normalization은 실제 sensor log와 control rate를 확인하면서 학습 환경 구현 단계에서 정한다. Wrist force feedback의 역할은 [Force Push](https://doi.org/10.1109/LRA.2024.3414180)와 [RoboPack](https://doi.org/10.15607/RSS.2024.XX.130)을 참고하되, 이들이 사용한 temporal inference가 우리 current-wrench baseline에 자동으로 필요하다고 간주하지 않는다.

### 4.6 Current sensor input과 1-step previous action

현재 actor observation에는 tactile과 wrench의 과거 frame을 concatenate하지 않는다.

- tactile: 현재 $\mathbf{b}_t$ 17D
- wrist F/T: 현재 $\widetilde{\mathbf{w}}_t$ 6D
- previous action: 직전 $\mathbf{a}_{t-1}$ 12D

직전 action은 [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831)의 `last_action` observation term과 같은 의미다. $\mathbf{a}_{t-1}$은 현재 measured state와 접촉 반응이 어떤 직전 명령 뒤에 발생했는지를 actor가 구분하도록 돕는다. 현재 observation에서는 두 step 이상의 action stack을 사용하지 않고 reset 직후 값을 0으로 초기화한다. Action 누적 방식과 controller target state는 1.3절의 `[Open]` 논의를 따른다.

Current-only sensor 입력을 채택하는 이유는 다음과 같다.

1. 17D tactile 자체가 sensor별 contact identity를 제공하고, continuous vision과 current robot configuration도 함께 관측한다.
2. 입력 차원과 시간 정렬 부담을 줄여 sim–real interface를 단순하게 유지한다.
3. [DexTouch](https://doi.org/10.1109/LRA.2024.3478571)처럼 current binary tactile만으로 실물 전이가 가능했던 직접 사례가 있다.

Sensor history를 제거하는 것과 실제 장치가 제공하는 내부 filtering은 구분한다. Policy용 binary tactile은 매 시점의 sensor 값에 단일 threshold를 적용하며 hysteresis를 사용하지 않는다. Short tactile/wrench history는 현재 observation에 포함하지 않으며, 추가 여부는 이후 협의한다.

---

## 5. Observation 전처리와 Sim-to-Real 경계

4절의 값은 simulation에서 바로 읽을 수 있지만, actor가 실물에서도 같은 의미의 신호를 받아야 한다. 이 절은 합의된 observation semantics와 아직 수치·방법을 정하지 않은 전처리 후보를 구분한다.

### 5.1 정규화 원칙

| 항목 | Working rule | 수치 결정 시점 |
|---|---|---|
| EEF-relative position | 학습 workspace bound로 scale·clip | workspace 설계 시 |
| Push direction·face normal | Unit normalization; sim–real axis convention 고정 | task interface 검증 시 |
| Quaternion | unit normalization + canonical sign; component order 고정 | Isaac Lab version 확인 시 |
| OBB extent | 학습 물체 크기 범위로 scale | object set 확정 시 |
| Arm·hand joint | joint limit 기준 정규화 | URDF 검증 시 |
| Binary tactile | 단일 threshold로 이진화한 0/1 유지; hysteresis 없음 | threshold 수치는 sensor 검증 시 |
| Wrist F/T | bias 보정 후 robust bound로 clip·scale | 실물 sensor log 수집 시 |
| Previous action | policy action space와 동일한 normalized value | action scaling 확정 시 |

### 5.2 Vision/OBB 입력의 현실화 — `[Open]`

Vision tracking 자체의 개선은 본 연구 범위가 아니다. Actor가 받는 pose·OBB는 deployment에서 tracker-derived signal을 뜻한다. Training에서 ground-truth object pose를 proxy로 사용할지와 다음 corruption을 실제 tracker 통계에 맞춰 적용할지는 아직 합의하지 않았다.

- position·orientation noise와 bias
- latency와 낮은 update rate
- 짧은 hold/dropout
- 드문 outlier
- OBB symmetry에 따른 axis ambiguity

Corruption을 사용한다면 목표 command와 선택 face를 만든 초기 object estimate, current pose와 face normal 사이의 일관성을 보존해야 한다. 구체 noise model, update rule과 metadata 사용 여부는 `[Open]`이다. Vision confidence와 마지막 update 이후 시간은 현재 66D observation에 넣지 않는다. Visuotactile state estimation의 역할 구분은 [Visuotactile Estimation and Control](https://doi.org/10.48550/arXiv.2412.13157)을 참고한다.

### 5.3 Sensor rate가 policy rate보다 빠를 때 — `[Open]`

현재 observation은 tactile과 wrist wrench의 **current value**를 사용하며 과거 frame을 actor에 stack하지 않는다. Policy tick에 어떤 sample을 current value로 대응시킬지는 아직 정하지 않았다. Latest sample 외에 검토할 수 있는 interval aggregation 후보는 다음과 같다.

- tactile: policy interval 내부의 contact OR/latch
- F/T: interval의 mean, peak 또는 impulse summary

이러한 summary의 채택 여부와 판단 방식은 `[Open]`이다. 채택한다면 sim과 real에서 동일한 시간 window와 연산을 사용해야 한다.

---

## 6. Observation 범위의 경계

Baseline을 작고 배포 가능하게 유지하려면 포함 항목뿐 아니라 제외 항목도 명시해야 한다. 아래 정보는 중복되거나 실물에서 안정적으로 얻기 어렵기 때문에 현재 actor에서 제외하며, 실패 근거가 생길 때만 다시 검토한다.

| 제외 항목 | 현재 판단 | 필요 시 검토할 조건 |
|---|---|---|
| Phase ID | 현재 observation에는 제공하지 않음 | Policy organization 논의에서 재검토 |
| Shelf/workspace state | S0의 fixed layout에서는 제공하지 않음 | S1에서 주변 물체·clearance ambiguity가 주된 실패 원인일 때 |
| Object linear/angular velocity | noisy finite difference를 피하기 위해 제외 | pose history가 실제로 필요하다는 실패 근거가 있을 때 |
| Arm·hand joint velocity | current joint state로 시작 | action-response mismatch가 구분되지 않을 때 |
| Absolute EEF pose/twist | EEF-relative task state와 중복 가능 | 관절 state만으로 제어 제약을 설명하지 못할 때 |
| Fingertip/pad Cartesian position | hand joint와 tactile에 중복 가능 | geometry/contact attribution 실패가 확인될 때 |
| Vision confidence/age | tracker 개선이 연구 범위가 아님 | deployment recovery를 명시적으로 다룰 때 |
| Raw image, point cloud, mesh | occlusion과 sim-to-real 안정성 우선 | geometry-aware variant를 별도 비교할 때 |
| Controller desired target | 현재 66D observation에서 제외 | 최종 action/controller interface를 정할 때 재검토 |
| 정확한 contact force·normal·pair | privileged information으로만 사용 | actor에 넣지 않음 |
| Tactile·wrench history | current sensor input으로 시작 | contact transition·slip·delay를 구분하지 못하는 실패가 확인될 때 |

현재 66D actor에는 shelf geometry와 surrounding-object state가 포함되지 않는다. 따라서 이 observation이 적용되는 scene 범위, surrounding-object 수·배치·접촉 규칙과 별도 safety layer의 구체 역할은 이후 협의해야 한다. Object–support-plane 접촉이 task dynamics에 필요하다는 점과 actor tensor에 privileged collision·support state를 섞지 않는 경계만 유지한다.

---

## 7. Observation 대안 — `[Open]`

아래 표는 현재 observation과 검토 가능한 대안을 기록한다. **비교 protocol이나 experiment를 정한 표가 아니며**, 무엇을 실제로 비교할지도 아직 합의하지 않았다.

| 질문 | 현재 observation | 검토 후보 | 논의 이유 |
|---|---|---|---|
| Current orientation 표현은 충분한가? | EEF-relative quaternion 4D | rotation 6D | 저차원 구현과 sign-boundary의 trade-off |
| 선택 face 표현이 필요한가? | Current EEF-frame pushing normal 3D | object-local face one-hot + quaternion; face normal 제거 | Actor에 task-relevant alignment를 직접 제공하는 의미 검토 |
| Tactile 공간 해상도는 필요한가? | full 17-channel | coarse pooling; F/T only | 접촉 위치 정보와 sim–real 구현 비용의 trade-off |
| Sensor history가 필요한가? | tactile·wrench current-only | modality별 short stack | partial observability 개선이 추가 차원을 정당화하는지 확인 |
| Previous action이 필요한가? | 1-step $\mathbf{a}_{t-1}$ | 제거; 2-step 이상 stack | action–response 추론에 대한 최소 action memory의 기여 |
| Binary tactile와 F/T가 상보적인가? | tactile+F/T | no contact feedback; tactile only; F/T only | 각 modality가 제공하는 정보와 중복 여부 검토 |
| Geometry 정보와 contact feedback이 상호작용하는가? | Approximate OBB + tactile+F/T | Geometry와 contact modality의 대안 구성 | Approximate-geometry error 보완이라는 연구 질문과의 관계 검토 |
| Selected-face goal이 충분한가? | selected face + push direction | 별도 desired rotation/orientation goal | OD-4의 OBB symmetry·non-box-like face ambiguity 확인 |
| Joint velocity/history가 필요한가? | 제외 | $\dot{\mathbf{q}}$ 또는 short $\mathbf{q}$ history | 접촉 중 stuck/compliance 상태 판별 기여 |

Rotation 6D와 explicit desired rotation/orientation goal은 각각 orientation 표현과 goal interface의 대안이다. 채택 또는 비교 여부는 협의 중이다.

---

## 8. Observation 구현 검증 항목

Observation 설계는 차원만 맞는다고 완료되지 않는다. Sim과 real에서 각 항목의 물리적 의미, 좌표계와 timestamp가 같아야 하므로 다음 항목을 학습 전 자동·수동 검증한다.

- [ ] Sim과 real에서 각 observation의 source, 단위, axis convention이 대응한다.
- [ ] Position과 orientation transform을 hand calculation과 단위 test로 검증한다.
- [ ] World/task target position·push direction과 object-local face identity는 고정되고 EEF-relative 표현만 매 step 변한다.
- [ ] Quaternion의 component order, multiplication convention, unit norm과 canonical sign이 sim/real에서 같다.
- [ ] OBB extent, 축과 selected-face identity가 episode 중 swap되지 않는다.
- [ ] Selected-face pushing normal과 push direction이 정렬될 때 alignment error가 0인지 검증한다.
- [ ] Any-contact semantics가 collider 종류와 무관하게 동일하다.
- [ ] Actor tensor에 contact pair, 정확한 force, boundary·OD-1 contact-rule flag 등 privileged 정보가 섞이지 않는다.
- [ ] Current tactile·wrench의 sensor timestamp와 policy timestamp가 일치하고, $\mathbf{a}_{t-1}$이 정확히 직전 policy action이다.
- [ ] Batch observation 순서와 $D_{\mathrm{obs}}$를 자동 test한다.

---

## 9. Privileged information 후보 — `[Open]`

앞 절까지가 현재 합의된 actor observation 범위다. **이 절부터 11절까지는 reward·termination·critic·evaluation·experiment에 관한 협의 전 메모이며, 채택된 policy 명세나 구현 계획이 아니다.** 아래 정보는 simulation에서만 얻을 수 있어 actor tensor에서 제외해야 하는 후보를 기록한다.

Simulation exact state를 reward, constraint·termination, evaluation 또는 critic에 실제로 사용할지는 아직 정하지 않았다. `Privileged`는 여기서 **actor observation에 포함하지 않는 정보**라는 뜻일 뿐, 특정 용도를 승인했다는 뜻이 아니다.

| ID | Privileged information | Simulation source | 가능한 용도 — 미합의 | Actor와의 경계 |
|---|---|---|---|---|
| P1 | Ground-truth object pose | rigid-body state | face alignment·translation progress, success | Actor는 tracker-derived pose를 사용하며, training corruption 적용 여부는 `[Open]` |
| P2 | Ground-truth object linear/angular velocity | rigid-body state | drift·toppling·stability 판정 | Actor baseline에서는 velocity 제외 |
| P3 | Hand–object contact pair·point·normal | contact report | desired contact, contact loss, force 작용선 | Actor tactile은 상대를 구분하지 않는 17D any-contact |
| P4 | Hand–object contact force·impulse | contact report | force direction, overload·impact | Actor에는 binary tactile와 measured wrist F/T만 제공 |
| P5 | Robot/object–auxiliary structure·surrounding object와 self-contact pair, force·impulse | contact report | OD-1 접촉 규칙에 따른 허용 접촉 metric 또는 forbidden collision cost·termination | Structure·scene geometry와 collision flag는 actor baseline에서 제외 |
| P6 | Object–support-plane contact, support footprint와 shelf usable boundary | contact report + rigid-body state | support 유지·shelf 이탈·fall 판정 | 정확한 support polygon·boundary는 actor에 제공하지 않음 |
| P7 | Object CoM·roll/pitch·height | asset property + rigid-body state | lever arm, balance·fall 판정 | CoM과 정확한 inertial property는 actor에서 제외 |
| P8 | Joint position·velocity·applied effort와 limits | articulation state | joint·velocity·effort constraint | Actor에는 current joint position만 제공 |
| P9 | Applied action과 직전 action | action manager | action-rate regularization | 직전 action 1-step은 actor에도 제공 |

이 후보를 논의할 때 다음 경계를 유지한다.

1. Ground-truth 정보를 reward·constraint·critic 등에 채택한다면 **deployment 입력이 아닌 학습용 정보**로만 구분한다.
2. Reward gate를 채택한다면 actor가 복원할 수 없는 hidden state를 만드는 문제를 검토한다.
3. Exact per-contact force를 사용할 경우 실물 sensing과의 의미 차이를 검토한다.
4. Exact pose와 tracker output을 reward·evaluation에 어떻게 구분해 사용할지는 별도로 협의한다.

---

## 10. Reward formulation 논의 기록 — `[Open]`

이 절의 식과 구분은 선행연구를 바탕으로 검토한 **후보**다. Reward의 구성, gate, success definition, downstream feasibility, safety 처리와 regularization 중 어느 것도 합의되지 않았다. 상세 문헌 근거는 [`papers/reward_formulation.md`](papers/reward_formulation.md)를 따른다.

### 10.1 두 분류가 아니라 네 층으로 분리한다

`phase별 reward + 그 외 penalty`보다 역할을 나누어 볼 수 있다는 문헌상 관점은 다음과 같다. Track B에 이 네 층을 채택할지는 `[Open]`이다.

| 층 | 역할 | 학습 신호에서의 처리 |
|---|---|---|
| Final task objective | 선택 면을 push direction에 정렬·유지하며 목표 위치까지 이동 | Sparse success 또는 다른 task objective 후보 |
| Phase-specific shaping | Approach, Rotation, Push의 탐색 난이도를 낮춤 | Gate가 있는 dense reward 후보 |
| Safety constraint | 충돌, 전도, 과부하, 관절 한계 등 상쇄되어서는 안 되는 조건 | Cost, termination 또는 별도 supervisor 후보 |
| Control regularization | 진동·불필요한 명령을 줄이되 task보다 우선하지 않음 | 작은 penalty 후보 |

[Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166)은 object-pose·reach·motion-direction·action-rate를 reward로 두면서 collision, toppling과 actuator limit를 constraint로 분리했다. [Stage-Wise Reward Shaping](https://doi.org/10.1109/ICRA55743.2025.11128552)은 sequential task의 stage별 reward와 cost를 constrained multi-objective formulation으로 분리했다. 이는 Track B의 reward를 협의할 때 참고할 문헌상 구분이다.

과거에 검토한 수식 후보는 다음과 같다. 현재 채택안이 아니다.

$$
r_t
=r_t^{\mathrm{succ}}
+g_A(s_t)r_t^A
+g_R(s_t)r_t^R
+g_P(s_t)r_t^P
+r_t^{\mathrm{reg}},
$$

$$
c_{j,t}\le 0\quad(j\in\mathcal C),
\qquad
\text{terminate if a hard-failure predicate is true.}
$$

여기서 $g_A,g_R,g_P$는 phase별 weight 후보이고 $c_j$는 safety cost 후보다. 이 구조, shared return과 termination 방식은 모두 `[Open]`이다.

### 10.2 Phase gate 후보

다음 기호를 사용한다.

- $c_{O,t}\in[0,1]$: 하나 이상의 hand link가 object와 접촉하는 aggregate-contact indicator 또는 smooth score
- $e_{\mathrm{align},t}=\arccos(\operatorname{clip}(\mathbf n_{f,t}^{W\mathsf T}\mathbf d_{\mathrm{push}}^W,-1,1))$: 선택 면 pushing normal과 push direction의 angle error
- $h_{\mathrm{align},t}=\exp[-(e_{\mathrm{align},t}/\sigma_{\mathrm{align}})^2]$: push-ready alignment score

과거에 제안된 gate 예시는 다음과 같다.

$$
g_A=1-c_O,
\qquad
g_R=c_O(1-h_{\mathrm{align}}),
\qquad
g_P=c_Oh_{\mathrm{align}}.
$$

이 후보가 의도한 동작은 다음과 같다.

- 접촉이 없으면 Approach shaping이 우세하다.
- 접촉했고 face–direction alignment error가 크면 Rotation shaping이 우세하다.
- 선택 면이 push direction에 가까워지면 Push shaping이 연속적으로 커진다.
- 접촉을 잃으면 별도 phase latch 없이 다시 Approach가 활성화된다.
- 초기 face alignment가 이미 적합하면 Rotation을 억지로 수행하지 않고 바로 Push할 수 있다.

이 후보에서 $g_A$는 `Approach phase 완료 판정`이 아니라 비접촉 거리 shaping의 활성도다. $c_O=1$은 접촉이 생겼다는 뜻일 뿐, hand configuration이 후속 조작에 적합하다는 뜻은 아니다. 이러한 gate를 사용할지, contact onset·phase transition을 어떻게 다룰지는 협의 중이다.

[Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236)은 goal까지의 거리에 따라 orientation shaping과 position shaping을 전환했고, [Adaptive Reaching and Pushing](https://doi.org/10.3389/fnbot.2023.1271607)과 [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873)는 접촉 여부에 따라 approach와 push 신호의 의미를 바꿨다. 위 gate는 이 사례를 바탕으로 만든 논의안이며, 식의 채택 여부와 검토 방법은 정하지 않았다.

Privileged contact pair를 gate에 쓰는 안은 actor observation과 hidden reward state가 어긋날 수 있다는 문제를 함께 검토해야 한다.

### 10.3 Final success와 task-level metric 후보

Push direction 단위벡터를 $\mathbf d$, 초기 object position을 $\mathbf p_{O,0}$라 할 때 검토한 progress 표현은 다음과 같다.

$$
s_t=\mathbf d^{\mathsf T}(\mathbf p_{O,t}-\mathbf p_{O,0}),
\qquad
\boldsymbol\ell_t=(\mathbf I-\mathbf d\mathbf d^{\mathsf T})(\mathbf p_{O,t}-\mathbf p_{O,0}).
$$

Final success를 정의하는 한 가지 후보는 다음 조건의 conjunction이다.

$$
\mathbb I_{\mathrm{succ},t}
=\mathbb I[
|s_t-s_{\mathrm{goal}}|\le\epsilon_s,
\ \|\boldsymbol\ell_t\|\le\epsilon_\ell,
\ e_{\mathrm{align},t}\le\epsilon_{\mathrm{align}},
\ \text{stable},
\ \neg\text{failure}
].
$$

$$
r_t^{\mathrm{succ}}=w_{\mathrm{succ}}\mathbb I_{\mathrm{succ},t}.
$$

손–물체 접촉을 success condition, shaping 또는 별도 metric 중 어디에 둘지는 협의 중이다.

[Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)은 pushing·pivoting 모두 task progress와 별도의 sparse success를 사용하고, [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166)은 허용 오차 안에 실제로 들어가도록 success 시 task reward를 높였다. Track B에서 sparse bonus, 종료, alignment와 stability를 어떻게 결합할지는 정하지 않았다.

`Stable`의 정의, 허용 범위와 dwell time도 evaluation 논의에 포함되는 `[Open]` 항목이다.

### 10.4 Phase별 objective와 term 후보

아래 항목은 모두 `[Open]`인 문헌 기반 후보다. 우선순위, 조합, 채택 여부와 검토 방법은 합의하지 않았다.

#### 10.4.1 Approach와 downstream-ready configuration에 관한 후보

Approach readiness를 평가할 후보 상태를 다음과 같이 둔다. 목표 회전·병진과 물체 크기에 따라 같은 손 자세의 유용성이 달라지므로 task command와 OBB extent도 상태에 포함한다.

$$
x_t^{HOC}
=
(g_t,\,d_O,\,T_{EO,t},\,q_t^A,\,q_t^H,\,b_t,\,\widetilde w_t).
$$

여기서 $g_t$는 target position, push direction과 selected-face pushing normal로 구성한 task-conditioned state다. 이 정의는 특정 hand pose를 정답으로 두지 않고, **같은 configuration도 수행할 task, 선택 면과 object geometry에 따라 다르게 평가**하기 위한 조건부 상태다.

이 상태의 품질을 손 모양 유사도보다 이후 Rotation→Push 가능성과 연결하는 관점을 검토할 수 있다. 구체 정의와 측정 방식은 `[Open]`이다.

과거 논의에서 구분한 continuation metric 후보는 다음과 같다.

$$
F_R(x)=\mathbb E_{\xi}
\left[\mathbb I_{\mathrm{rotation\ success}}\mid x,\pi,\xi\right],
\qquad
F_{R\rightarrow P}(x)=\mathbb E_{\xi}
\left[\mathbb I_{\mathrm{final\ success}}\mid x,\pi,\xi\right].
$$

$F_R$와 $F_{R\rightarrow P}$는 각각 Rotation과 최종 Push까지의 가능성을 구분하기 위한 아이디어다. 둘을 실제 metric으로 채택할지, 어떻게 추정할지는 아직 합의하지 않았다.

[RL-Critic Grasp Selection](https://doi.org/10.1109/ICRA55743.2025.11127792)은 후속 in-hand manipulation critic으로 초기 grasp 후보를 평가해 성공률을 높였고, [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987)는 transition feasibility function으로 앞 policy의 종료 상태를 뒤 policy가 실행 가능한 분포로 바꾸었다. 두 연구는 우리와 다른 grasp/in-hand·multi-policy 설정이지만, **현재 configuration의 품질을 후속 실행 가능성으로 평가해야 한다**는 근거를 제공한다.

Shared episodic return, saved-state rollout과 learned feasibility estimator는 서로 다른 후보이며 어느 것도 현재 baseline으로 채택하지 않았다. Saved-state evaluation을 사용할지와 readiness snapshot 정의도 `[Open]`이다.

별도 feasibility estimator를 사용할 경우 검토할 수 있는 potential-difference shaping 예시는 다음과 같다.

$$
r_{A,F}
=
\gamma\widehat F_{R\rightarrow P}(x_{t+1}^{HOC})
-
\widehat F_{R\rightarrow P}(x_t^{HOC}).
$$

이 estimator의 필요성, label, calibration, policy update와의 관계는 모두 협의 중이다.

#### 10.4.2 Term별 문헌 기반 후보

| Phase | Term | 예비 정의 | 상태 | 근거와 이식 이유 |
|---|---|---|---|---|
| Approach | Downstream Rotation→Push feasibility | Future return 또는 $F_R(x^{HOC})$, $F_{R\rightarrow P}(x^{HOC})$ 후보 | **[Open]** | [RL-Critic Grasp Selection](https://doi.org/10.1109/ICRA55743.2025.11127792)과 [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987)는 초기/이전 hand state를 후속 manipulation feasibility로 평가한다. Track B에 shared return, saved-state continuation 또는 별도 estimator 중 무엇을 사용할지는 정하지 않았다. |
| Approach | Hand–OBB approach progress | $r_{A,d}=(d_{H\text{-}OBB,t}-d_{H\text{-}OBB,t+1})/d_{\mathrm{scale}}$ | **[Open]** | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166)은 sampled surface reach target, [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873)은 EEF–object distance를 접촉 전 탐색에 사용했다. Track B에서는 exploration signal 후보일 뿐이다. |
| Approach | Contact-state progress | $r_{A,c}=c_{O,t+1}-c_{O,t}$ | **[Open]** | [Adaptive Reaching and Pushing](https://doi.org/10.3389/fnbot.2023.1271607)과 [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873)을 참고한 후보 signal이다. |
| Approach | Task-wrench capability | Rotation wrench와 Push force direction에 대한 achievable-wrench margin | **[Open]** | [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652)를 참고한 proxy 후보다. |
| Approach | Learned downstream feasibility shaping | $r_{A,F}=\gamma\widehat F(x_{t+1})-\widehat F(x_t)$ | **[Open]** | Downstream scoring 관점을 shared-policy shaping에 적용할 수 있는지 검토하는 후보다. |
| Rotation | Face–direction alignment progress | $r_{R,\mathrm{align}}=(e_{\mathrm{align},t}-e_{\mathrm{align},t+1})/\pi$ | **[Open]** | Target-orientation pivoting의 angle-error를 selected-face normal alignment에 적용할 수 있는지 검토하기 위한 후보다. |
| Rotation·Push | Aggregate contact-loss event | $r_{C,\mathrm{loss}}=-\mathbb I[c_{O,t}>0\land c_{O,t+1}=0]$ | **[Open]** | [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236)과 [Tac2Motion](https://doi.org/10.48550/arXiv.2509.17812)을 참고한 contact signal 후보다. |
| Rotation | Unwanted translation drift | $r_{R,\perp}=-\|\mathbf v_{O,\perp}\|^2/v_{\mathrm{scale}}^2$ | **[Open]** | Rotation 중 허용할 병진과 해로운 lateral drift를 구분할지 검토하는 후보다. |
| Push | Axial goal progress | $r_{P,s}=(\lvert s_{\mathrm{goal}}-s_t\rvert-\lvert s_{\mathrm{goal}}-s_{t+1}\rvert)/s_{\mathrm{goal}}$ | **[Open]** | Goal 방향 progress에 관한 문헌을 Track B에 적용할 수 있는지 검토하는 후보다. |
| Push | Lateral deviation | $r_{P,\ell}=-\|\boldsymbol\ell_t\|^2/\ell_{\mathrm{scale}}^2$ | **[Open]** | 지정 방향에서 벗어나는 motion을 별도로 다룰지 검토하는 후보다. |
| Push | Face–direction alignment 유지 | $r_{P,\mathrm{align}}=-e_{\mathrm{align},t}^2/e_{\mathrm{scale}}^2$ | **[Open]** | Position과 selected-face alignment를 분리할지 검토하는 후보다. |
| Push | Force direction·lever arm | force–goal alignment와 CoM–force-line distance | **[Open]** | Exact contact-force shaping의 simulator 의존성을 포함해 검토할 후보다. |
| Rotation·Push | Hand reconfiguration efficiency | First contact 이후 누적 $\|\Delta\mathbf q_t^H\|$, contact switch 수·duration | **[Open]** | Reconfiguration을 reward 또는 metric으로 다룰지 검토하는 후보다. |

과거 논의에서 조합한 즉시 reward 예시는 다음과 같다. 현재 채택안이 아니며 term과 weight는 모두 `[Open]`이다.

$$
\begin{aligned}
r_t^A &= w_{A,d}r_{A,d},\\
r_t^R &= w_{R,\mathrm{align}}r_{R,\mathrm{align}}+w_{C,\mathrm{loss}}r_{C,\mathrm{loss}},\\
r_t^P &= w_{P,s}r_{P,s}+w_{P,\ell}r_{P,\ell}+w_{P,\mathrm{align}}r_{P,\mathrm{align}}+w_{C,\mathrm{loss}}r_{C,\mathrm{loss}}.
\end{aligned}
$$

Transition reward, shared future return 또는 downstream value 중 무엇을 사용할지는 결정하지 않았다. [Value-Informed Skill Chaining](https://doi.org/10.1109/IROS55552.2023.10342180)은 이 논의를 위한 참고 사례다.

### 10.5 Safety cost·termination 후보

| 조건 | 처리 원칙 | 근거 |
|---|---|---|
| Auxiliary-structure·surrounding-object contact 또는 self-collision | Contact rule, cost와 termination의 관계를 정해야 함 | [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873), [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166) |
| Object toppling·support loss·낙하·boundary 이탈 | Cost, termination과 recoverable condition을 구분해야 함 | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166), [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036) |
| Joint position·velocity·effort limit | Learned cost와 external safety stop의 역할을 정해야 함 | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166), [Isaac Lab reward API](https://isaac-sim.github.io/IsaacLab/v2.0.0/_modules/isaaclab/envs/mdp/rewards.html) |
| Excessive wrist/contact force와 impact | Threshold, cost, termination과 hardware supervisor의 역할을 정해야 함 | [Safe Self-Supervised Visuo-Tactile Learning](https://doi.org/10.1109/ICRA48891.2023.10160763) |

Threshold와 safety 처리 방식은 hardware 정보 및 calibration 자료를 확인한 뒤 협의한다.

### 10.6 Control regularization 후보

Action-rate는 검토한 regularizer 후보 중 하나다.

$$
r_t^{\mathrm{reg}}
=-w_{\Delta a}
\left\|
\mathbf S_a(\mathbf a_t-\mathbf a_{t-1})
\right\|_2^2,
$$

여기서 $\mathbf S_a$는 translation, rotation과 hand joint action의 단위·scale 차이를 맞춘 block-diagonal normalization이다. [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)과 [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166)은 action smoothness/rate를 사용했고, [Isaac Lab real-robot reach 예제](https://isaac-sim.github.io/IsaacLab/develop/source/policy_deployment/04_reach/reach_policy.html)도 `action_rate_l2`를 작은 penalty로 둔다.

Action magnitude, joint velocity, torque/work, time penalty와 hand-joint-travel cost 중 무엇을 사용할지는 정하지 않았다. [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036)과 [RotateIt](https://doi.org/10.48550/arXiv.2309.09979)은 관련 참고 사례다.

### 10.7 Weight·scale 논의 시 확인할 사항

Reward weight를 논의할 때 문헌의 숫자를 그대로 복사할 수 없다는 점은 유지한다. 아래 항목을 실제 절차로 채택할지는 아직 정하지 않았다.

1. Position, angle, force와 action term의 scale을 어떤 기준으로 맞출 것인가?
2. Isaac Lab `RewardManager`의 step-time 처리와 policy frequency를 어떻게 반영할 것인가?
3. 각 unweighted term의 분포와 상관관계를 사전에 확인할 것인가?
4. Sparse success와 dense shaping의 상대적 scale을 어떻게 정할 것인가?
5. 어떤 term을 어떤 순서로 검토할지와 실제 hardware에서 허용할 비교 범위를 협의한다.
6. 어떤 결과를 보고할지 evaluation 논의와 함께 정한다.

---

## 11. 이후 협의 항목 — `[Open]`

아래 목록은 reward 구현 순서가 아니라 이후 사용자와 협의해야 할 질문을 보존한다. 수식, algorithm, metric, protocol 또는 구현 순서를 정한 것으로 해석하지 않는다.

### 11.1 Reward 논의 질문

1. $d_{H\text{-}OBB}$에 포함할 RH56E2 link/pad 집합과 OBB signed-distance 계산 방식
2. Desired hand–object contact $c_O$의 binary/smooth 정의와 최소 force threshold
3. Push distance·lateral·face-alignment success tolerance $\epsilon_s,\epsilon_\ell,\epsilon_{\mathrm{align}}$
4. OBB lateral-face 후보, outward/inward normal convention과 episode-consistent face tracking
5. Shelf usable boundary·support-footprint 판정, topple, overload와 OD-1에서 금지한 접촉의 soft/hard threshold
6. `constraint PPO/CMORL`, stochastic termination 또는 일반 PPO+hard termination 중 safety 학습 방식
7. Shared return, transition signal 또는 별도 feasibility objective 중 무엇이 필요한가?
8. Continuation 관점을 사용할 경우 state와 추정 방식을 어떻게 정의할 것인가?
9. Success estimator가 필요한가? 필요하다면 label과 calibration을 어떻게 정할 것인가?
10. Aggregate contact loss와 hand reconfiguration을 reward, constraint 또는 evaluation 중 어디에 둘 것인가?

### 11.2 Observation·interface에서 병행할 항목

1. **Quaternion interface**: 설치할 Isaac Lab version과 policy tensor의 `(w,x,y,z)`/`(x,y,z,w)` 순서
2. **Tactile mapping**: RH56E2 17 sensor index와 simulation link/pad 또는 sensor region의 일대일 대응
3. **Sensor synchronization**: tactile·F/T sampling rate, policy tick의 latest sample과 interval aggregation 방식
4. **Wrist F/T 처리**: bias, gravity/inertia compensation, filter, clip bound
5. **Vision corruption**: 실제 tracker에서 측정할 noise·latency·dropout 통계
6. **입력 정규화 수치**: workspace, object set, joint/action limit 확정 후 결정
7. **S1 scene interface**: surrounding-object 수·배치·관측 범위와 접촉 규칙을 정한 뒤 현재 66D observation으로 충분한지 판단

### 11.3 구현 순서 — `[Open]`

구현 순서는 아직 합의하지 않았다. Observation interface의 unit test 범위와 action/controller, reward, safety, evaluation 항목의 구현 여부·순서는 각각 별도 논의가 필요하다.
