# JH.Jeong Research Topic

> **연구 주제:** Shelf retrieval을 위한 goal-conditioned blocker-object contact manipulation
>
> **현재 중심 Track:** Track B
>
> **최종 갱신:** 2026-09-16

---

## 1. 문서 목적

이 문서는 연구 과정에서 **구체화되었거나 확인된 현재 내용**을 간결하게 정리한다. 논의의 흐름, 과거 제안, 작업 가설과 미결 사항의 상세한 기록은 [`context.md`](./context.md)에서 관리한다.

Research motivation의 문헌 비교, gap, 가설과 논문용 서술 초안은 [`motivation.md`](./motivation.md)에서 별도로 발전시킨다. 이 문서에는 그중 연구 범위와 역할 경계에 영향을 주는 현재 결론만 유지한다.

새로운 아이디어나 설계 후보는 합의되기 전까지 이 문서의 확정 내용으로 추가하지 않는다. 중요한 결정이 변경되면 `context.md`에 변경 배경과 의사결정 과정을 먼저 기록하고, 이 문서를 최신 결론에 맞게 갱신한다.

---

## 2. 연구 주제

선반 환경에서 target object의 관측이나 인출을 방해하는 blocker object를 재배치하여, target에 접근할 수 있는 공간 또는 경로를 확보한다.

연구의 중심은 continuous Vision으로 관측한 blocker의 현재 pose와 근사 geometry, 그리고 F/T·Tactile contact feedback을 이용하는 RL manipulation policy다. 먼저 주어진 회전·병진 목표를 접촉 불확실성 아래에서 안정적으로 실행하는 능력을 학습하고, 이후 공간 확보 목적에 필요한 조작 목표와 동작 전환을 정책이 결정하도록 담당 범위를 확장한다.

현재 조작 관점은 **다양한 초기 물체 자세에서, 목표 방향의 pushing을 위해 물체를 주어진 자세로 돌려두고 미는 것**이다. 회전 자체만이 목적이 아니라 후속 밀기를 위한 준비라는 점이 중요하다. 단, 1단계에서 어떤 물체 자세가 적합한지 선택하는 것은 상위 모듈의 역할이며, 우리 policy는 그 목표를 실행하기 위한 hand configuration과 접촉 상태를 형성·조절한다.

이를 한 문장으로 정리하면 다음과 같다.

> **지속적인 시각·근사 기하 관측과 접촉 감각을 이용해 blocker의 목표 조건부 회전·병진 조작을 학습하고, 이를 target 접근 공간 확보를 위한 조작 의사결정과 실행의 통합으로 확장한다.**

### 2.1 Motivation과의 연결

Preparatory rotation은 독립적인 자세 맞춤이 아니라 후속 pushing에 유효한 object–hand 관계와 접촉 배치를 만드는 수단으로 본다. 따라서 Rotation의 terminal contact와 Push의 initial contact를 연결해 평가하며, coarse OBB로 알 수 없는 접촉·물성 차이는 binary tactile와 wrist F/T feedback으로 보정하는지를 검증한다.

이 문제 정의의 배경, research gap, MH1–MH4와 claim–evidence 구조는 [`motivation.md`](./motivation.md), 이를 뒷받침하는 논문과 baseline 후보는 [`papers/README.md`](./papers/README.md), 이 결론에 도달한 과정은 [`context.md`](./context.md)를 따른다.

---

## 3. 응용 시나리오와 정책의 대상

- **Target object:** 사용자가 최종적으로 찾거나 꺼내려는 물체로, blocker에 의해 부분적으로 보이거나 가려질 수 있다.
- **Blocker object:** Target의 관측 또는 접근을 방해하며, manipulation policy가 재배치하는 물체다.
- **정책의 직접적인 조작 대상:** Blocker object
- **최종 응용 목적:** Blocker를 재배치하여 target의 노출량이나 접근 가능한 공간을 늘리고, 이후 인출이 가능한 상태를 만드는 것

현재 정책이 target retrieval 동작 자체를 수행하는 것은 아니다. 실제 target 인출은 blocker manipulation 이후의 별도 단계로 둔다.

---

## 4. 연구의 단계적 진행

### 4.1 1단계 — 주어진 조작 목표 실행

상위 판단기가 조작할 blocker와 목표 회전·병진 운동을 제공한다. Policy는 물체 근처에서의 접근과 접촉 형성부터 목표 물체 운동의 실행까지 담당한다.

**목표 선택과 실행의 경계:** 상위 모듈이 밀기에 적합한 목표 물체 orientation을 지정한다. Policy가 손목 pose·손가락 configuration·접촉 배치를 조절하는 것과 목표 물체 orientation 자체를 선택하는 것은 별개다. `적합한 물체 자세를 형성한다`는 표현은 1단계에서 **주어진 자세를 실제로 달성한다**는 뜻으로만 사용한다.

기본 동작 순서는 다음과 같다.

> **Approach / Contact Formation → Rotation → Push / Translation**

| 단계 | 물체 수준 목표 | Hand의 역할 |
| --- | --- | --- |
| Approach / Contact Formation | 후속 조작에 유효한 접촉 형성 | 근사 geometry를 이용해 손목 pose와 손가락 구성을 준비하고, 실제 접촉 feedback으로 보정 |
| Rotation | 요구된 blocker orientation 달성 | 필요한 회전 모멘트를 만들도록 접촉 위치와 힘 분포 조절 |
| Push / Translation | 목표 blocker position 도달 및 요구 orientation 유지 | 병진에 적합하게 접촉을 재조정하고 이동 방향·속도와 손 구성을 보정 |

Rotation과 Translation의 goal은 **blocker의 물체 상태에 대한 목표**이며, 손목의 자세 목표와 구분한다. 이 단계의 핵심 질문은 다음과 같다.

> 주어진 회전·병진 목표를 근사 형상과 실제 접촉 상태의 불일치 아래에서도 정확하고 안정적으로 실행할 수 있는가?

Approach는 준비의 시작이며, Rotation과 Push에서도 접촉 구성의 조절·전환이 필요할 수 있다. 따라서 연구의 기여 후보를 최초 Approach에만 한정하지 않는다. 회전이 끝난 물체 pose뿐 아니라 후속 pushing에 유효한 손·접촉 상태도 고려하되, 각 phase의 별도 알고리즘이나 network를 확정하지 않는다.

### 4.2 2단계 — 공간 확보를 위한 조작 의사결정 통합

1단계에서 확보한 실행 능력을 기반으로, 상위 입력을 세부 motion instruction에서 목적 수준으로 추상화한다.

- **1단계 지시:** 지정된 회전·병진 목표를 실행한다.
- **2단계 목적:** 선택된 blocker를 조작하여 target에 접근할 공간 또는 경로를 확보한다.

2단계에서 정책의 담당 범위는 다음 의사결정까지 확장한다.

- 회전이 필요한지 또는 바로 Push할 수 있는지 판단
- 후속 이동에 적합한 중간 orientation과 접촉 배치 결정
- 이동 방향과 거리 결정
- Rotation과 Push 사이의 전환 시점 및 필요한 동작 순서 결정
- 접촉 재형성·접촉 전환·추가 조작의 필요 여부 판단
- 공간 확보가 충분한지와 manipulation 완료 여부 판단

1단계의 기본 동작 순서는 2단계의 고정 제약이 아니다. 2단계에서는 목적과 장면 조건에 따라 필요한 동작과 순서를 선택하는 방향으로 확장한다.

---

## 5. 관측 정보와 센서 역할

Track B는 manipulation 전 과정에서 Vision, wrist F/T와 tactile sensing을 함께 사용한다.

각 신호는 **선행연구의 raw input·전처리·representation, [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831)의 원천 데이터와 가공 가능성, 실물 대응, 좌표계·주기·noise·latency와 history**를 함께 검토한다. Simulation의 exact state·contact를 실제 sensor observation과 구분하지 않고 actor에 넣지 않는다.

현재 policy는 MLP이며 actor에 phase ID를 주지 않는다. `Approach / Contact Formation → Rotation → Push`의 진행은 phase별 gate가 reward term을 활성화하는 방식으로 학습한다. Gate가 action 자유도를 phase별로 mask한다는 뜻은 아니다.

### 5.1 Goal과 Vision·Geometry

Goal은 일반적인 final object-pose reaching이 아니다. Primary objective는 지정 방향·거리의 pushing이고, 상위가 제공하는 object orientation은 그 pushing을 위한 준비 및 Push 중 유지 조건이다.

$$
p_{O,g}^{W}=p_{O,0}^{W}+s_{\mathrm{push}}d_{\mathrm{push}}^{W}
$$

Policy에는 현재 EEF frame으로 변환한 target object position과 preparatory object orientation을 goal로 주고, current EEF–object pose를 별도로 준다. 현재 working baseline은 움직이는 EEF에서 orientation 문맥을 보존하기 위해 current와 target rotation을 각각 6D로 표현한다.

$$
g_t^E=\left[p_{O,g}^{E},\rho_6(R_{EG,t})\right]\in\mathbb{R}^{9}
$$

여기서 $\rho_6(R)$는 rotation matrix의 첫 두 column을 펼친 6D continuous representation이다. Task objective와 reward는 shelf normal 주위의 preparatory yaw에 한정하지만, EEF가 roll/pitch할 수 있으므로 observation을 EEF-frame yaw scalar로 축소하지 않는다. EEF가 shelf normal에 항상 정렬되는 제한된 조건의 `sin/cos yaw`는 representation ablation으로 남긴다. 이 working baseline의 최종 사용자 확정은 남아 있다.

Unseen object와 occlusion을 고려해 raw RGB, point cloud와 mesh 대신 3D OBB를 coarse geometry로 사용한다. 조작 전 initial OBB의 object-local axes·extent를 episode template으로 고정하고 manipulation 중에는 pose만 tracking한다. Observation에는 object-local extent만 넣으며, EEF에서의 box-axis 방향은 current object orientation으로 결정한다.

$$
d_O=[l_O,w_O,h_O]\in\mathbb{R}^{3}
$$

### 5.2 Tactile와 Wrist F/T

기본 actor 조합은 **binary tactile + wrist F/T**다.

- 실제 RH56E2는 총 17개 tactile sensor를 사용하는 것으로 파악한다. 정확한 위치·packet은 장비와 URDF에서 확인한다.
- Taxel별 collision body 17개를 만드는 방법은 simulation 연산량 때문에 기본안에서 제외한다.
- 실제 17 sensor와 simulation의 contact-bearing URDF link/pad를 동일한 `M`개 coarse region으로 pooling한다.
- `M=17`의 세밀한 binary tactile와 `M<17`의 coarse binary tactile는 성능·robustness·simulation cost를 실험적으로 비교한다.
- Actor tactile는 접촉 상대의 identity를 구분하지 않는다. Object, shelf 또는 다른 body 중 무엇과 닿았든 해당 region에 접촉이 감지되면 `contact on`으로 둔다. Simulation의 collider identity는 reward·critic용 privileged information에서만 구분한다.
- Threshold는 문헌의 `0.01 N`을 복사하지 않고 실제 sensor의 no-contact/contact 분포에서 `τ_on>τ_off` hysteresis와 debounce를 정한다.
- Simulation continuous contact force는 actor tactile가 아니라 reward·asymmetric critic용 privileged information으로 사용한다. 과부하·충격·접촉 부족을 평가하되 관측 불가능한 정확한 국소 force 분포를 강제하지 않는다.

### 5.3 현재 최소 MLP Observation Working Baseline

아래 표는 최신 논의를 반영한 구현 기준안이며 rotation 선택은 최종 사용자 확정 전이다.

| Observation | 표현 | 차원 | 시간 범위 |
| --- | --- | ---: | --- |
| Push-conditioned goal | EEF-frame target object position 3D + target orientation 6D | 9 | Current |
| Current object pose | EEF-frame estimated object position 3D + current orientation 6D | 9 | Current |
| Coarse geometry | Episode-consistent object-local OBB extent | 3 | Episode-fixed |
| Arm configuration | UR5e measured joint position | 6 | Current |
| Hand configuration | RH56E2 actuated joint position | 6 | Current |
| Binary tactile | Real 17 sensor 또는 공통 coarse region의 on/off | `K_bM` | 최근 `K_b` step |
| Wrist F/T | Bias-compensated EEF-frame force·torque | `6K_w` | 최근 `K_w` step |
| Previous action | EEF delta pose 6D + hand joint action 6D | `12K_a` | 최근 `K_a` step |

MLP에는 history를 recurrent state로 숨기지 않고 과거 sensor/action 값을 flatten하여 observation으로 넣는다.

$$
D_{\mathrm{MLP}}=33+K_bM+6K_w+12K_a
$$

모든 modality에 같은 history window를 적용할 필연성은 없다. 공통 `K`는 시간 정렬과 첫 비교가 단순하다는 장점이 있으며, 최종 `K_b,K_w,K_a`는 실제 action-to-sensor latency와 contact transient의 시간 범위로 정한다.

현재 최소안에서는 phase ID, object velocity, arm/hand joint velocity, absolute EEF pose·twist, fingertip position, vision confidence/age, raw RGB, point cloud와 mesh를 제외한다. 실제 failure 분석에서 필요성이 확인되면 추가·ablation한다. Shelf·workspace pose나 clearance도 actor observation에는 주지 않고, simulation privileged collision state로 penalty·termination을 계산한다.

Continuous Vision을 사용하더라도 실제 국소 접촉 표면, 마찰, 질량 분포 등 전체 물리 상태를 정확히 안다고 가정하지 않는다. 접촉 전에는 OBB로 hand configuration을 준비하고, 접촉 후에는 binary tactile와 wrist F/T로 실제 상호작용에 맞게 보정한다.

### 5.4 Observation 결정 근거의 위치

Observation의 **현재 채택 결과**는 5.1–5.3절을 따른다. 각 항목을 유지·제외하거나 ablation으로 남긴 이유는 [`context.md`](./context.md)의 5.6.1.7절, 관련 논문과 [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831) 구현 검토는 [`papers/topic_groups.md`](./papers/topic_groups.md)의 7절과 [`papers/reading_guide.md`](./papers/reading_guide.md)의 2절에서 관리한다.

### 5.5 Rotation Observation 현재 명세

현재 baseline은 full SO(3) 문맥을 보존하는 6D continuous representation을 사용한다. Planar `sin/cos yaw`와 5D representation은 ablation으로만 남긴다. 표현 선택의 문헌 근거는 [`papers/topic_groups.md`](./papers/topic_groups.md)의 7절, 5D의 유도와 결정 과정은 [`context.md`](./context.md)의 Stage 17–18에 기록한다.

우리의 current와 goal frame은 다음처럼 EEF 기준으로 맞추는 것이 자연스럽다.

$$
R_{EO,t}=R_{WE,t}^{T}R_{WO,t},\qquad
R_{EG,t}=R_{WE,t}^{T}R_{WG,t}
$$

표현의 현재 후보와 적용 조건은 다음과 같다.

1. **EEF가 shelf normal에 정렬된 planar 조건:** EEF의 roll/pitch가 고정되고 object도 shelf 위에서 upright하다는 조건이 보장되면 current yaw와 preparatory target yaw를 각각 `[\cos\psi,\sin\psi]`로 표현해도 충분하다.
2. **움직이는 EEF frame의 일반 조건:** EEF가 roll/pitch까지 바꾸거나 object가 접촉 중 기울 수 있다면, EEF-frame Euler yaw는 shelf-plane yaw와 같지 않다. 이 경우 `R_{EO}`와 `R_{EG}`의 첫 두 column을 펼친 6D continuous representation을 각각 제공하는 편이 정보 손실이 적다.

현재 working baseline은 **current와 target orientation은 EEF-relative 6D로 관측하되, reward의 회전 오차는 shelf/task frame의 normal 주위 yaw 성분만 평가**하는 것이다. Full 3D observation을 준다는 것이 object를 arbitrary SO(3) target으로 보내겠다는 뜻은 아니다. 관측은 움직이는 EEF와 접촉 중의 tilt까지 해석할 문맥을 보존하고, task objective는 원하는 pushing 방향에 적합한 preparatory yaw에 한정한다. Action은 작은 EEF-local increment이므로 3D rotation vector를 계속 사용할 수 있다. 이 baseline의 최종 사용자 확정은 남아 있다.

이에 따른 MLP 전체 차원은 각각 다음과 같다.

$$
D_{\mathrm{planar}}=25+K_bM+6K_w+12K_a,\qquad
D_{\mathrm{6D}}=33+K_bM+6K_w+12K_a
$$

6D representation 자체가 OBB symmetry를 해결하지는 않는다. Symmetry group을 `\mathcal{G}_O`라 하면 yaw reward와 goal 판정은 task frame에서 symmetry-equivalent target 중 가장 가까운 것을 사용한다.

$$
e_{\psi,\mathrm{sym}}=
\min_{S\in\mathcal{G}_O}
\left|\operatorname{wrap}\!\left(\psi(R_{WO})-\psi(R_{WG}S)\right)\right|
$$

여기서 $\psi(\cdot)$는 Euler decomposition의 임의 yaw가 아니라 shelf/task-frame normal에 직교하는 평면으로 object heading을 투영해 얻는 yaw다. Episode 중 OBB axes는 이전 frame과 가장 가까운 symmetry-equivalent orientation을 선택해 temporal continuity를 유지한다. Roll/pitch까지 목표로 제어하는 과업으로 확장할 때만 full geodesic SO(3) error를 별도로 사용한다.

---

## 6. 시스템 내 역할 경계

### 6.1 1단계의 역할 분담

- 상위 perception·판단 모듈이 장면을 관측하고 조작할 blocker, 밀기에 적합한 목표 물체 orientation과 병진 목표를 정한다. 목표 선택 알고리즘 자체는 현재 low-level policy의 contribution 범위가 아니다.
- VLA 또는 conventional planner가 장거리 reaching을 담당하고 로봇을 blocker 근처로 이동시킨다.
- Track B policy가 물체 근처의 Approach, contact formation, Rotation과 Push를 수행한다.
- 외부 execution layer가 안전 중단과 전체 시스템 수준의 실행을 관리할 수 있다.

### 6.2 2단계의 첫 확장 범위

- 조작할 blocker의 선택은 상위 판단기에 남긴다.
- 선택된 blocker를 어떻게 재배치할지는 정책이 목적에 맞게 결정하고 실행한다.
- 여러 blocker 중 선택 및 전체 조작 순서 결정은 후속 확장으로 둔다.
- 장거리 reaching과 실제 target retrieval 전체를 현재 2단계에 포함하지 않는다.

여기서 `end-to-end에 가까운 정책`은 target 접근 공간 확보라는 목적에서 blocker manipulation action까지 정책의 담당 범위를 넓힌다는 뜻이다. Raw sensor부터 action까지의 단일 network, 특정 VLA 구조 또는 특정 학습 알고리즘을 의미하지 않는다.

---

## 7. 평가 수준

연구 진행 단계에 따라 평가 목적을 구분한다.

| 구분 | 중심 평가 |
| --- | --- |
| 1단계 | 주어진 blocker 회전·병진 목표의 실행 정확도와 접촉 안정성 |
| 2단계 | Target 접근 공간 또는 경로 확보 효과와 전체 manipulation의 효율·안정성 |

Low-level object goal의 달성과 최종 shelf-retrieval 효과도 구분한다.

- **Skill-level:** blocker의 position/orientation 정확도, 접촉과 힘의 안정성, 실패 및 수행 시간
- **System-level:** target의 노출, 접근 가능한 공간·경로, 실제 인출 가능 여부와 필요한 조작 횟수

1단계의 회전·병진 정확도만으로 연구 전체의 공간적 유용성을 주장하지 않는다. 반대로 2단계의 공간 확보 효과를 평가할 때에도 접촉 부하, 전도·낙하와 같은 실행 안정성을 함께 고려한다.

---

## 8. 현재 범위에 관한 주의사항

- Track B를 Rotation 또는 Handling만을 수행하는 Track으로 정의하지 않는다.
- Track B는 lateral pushing에 한정되지 않으며 다양한 방향의 translation과 rotation을 다루는 방향이다.
- `Approach → Rotation → Push`는 1단계의 기본 실행 순서이며 모든 상황의 유일한 전략은 아니다.
- 1단계에서 policy가 밀기에 적합한 중간 물체 orientation을 스스로 선택한다고 주장하지 않는다. 목표 물체 자세의 선택은 상위, 그 목표를 위한 손 구성·접촉 실행은 policy의 역할이다.
- Contribution을 최초 Approach에만 한정하거나 각 phase마다 독립 novelty가 필요하다고 해석하지 않는다.
- 다양한 초기 자세·방향을 지향한다는 것이 모든 SE(3) 자세에서의 실행 가능성을 보장한다는 뜻은 아니다. 실제 학습·평가 범위는 미결이다.
- Continuous Vision과 근사 geometry의 사용을 정확한 접촉 상태의 완전 관측과 동일시하지 않는다.
- F/T와 tactile은 Track B의 관측에서 제외하거나 임의로 optional로 바꾸지 않는다.
- Actor에 phase ID를 주지 않는다는 현재 방향을 shared policy가 없거나 phase 구분이 없다는 뜻으로 해석하지 않는다. Phase별 reward gate는 별도로 존재한다.
- OBB는 exact geometry가 아니라 unseen object·occlusion에서 안정성을 우선한 coarse representation이다. Global semantic/canonical object frame을 안다고 가정하지 않는다.
- 실제 tactile sensor가 17개라는 사실과 policy가 반드시 17차원을 써야 한다는 주장을 구분한다. Coarse `M<17`과 17-channel 표현은 비교 대상이다.
- 세 가지 동작 구분을 세 개의 독립 network 또는 policy가 필요하다는 뜻으로 해석하지 않는다.
- 특정 시리얼 박스, hooking 동작, 동일한 회전각과 선반 구성은 설명용 예시이지 전체 학습 명세가 아니다.
- 현재 방향은 연구 문제와 진행 범위를 정의한 것이며, 최종 Method novelty나 논문 Contribution이 확정된 것은 아니다.

---

## 9. 아직 확정 명세로 취급하지 않는 항목

다음 항목은 추가 논의와 실험을 통해 구체화한 뒤 이 문서에 반영한다.

- 현재 최소 MLP observation의 실제 구현과 각 항목의 정규화·noise
- 사용 중인 [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831)·Isaac Sim 버전과 이에 맞는 ContactSensor·joint-wrench API
- 실제 RH56E2 17 sensor의 위치·단위·noise floor·packet/update rate
- 실제 17 sensor와 URDF link/pad의 coarse grouping, 최종 `M`과 17-channel 비교 구현
- Binary contact threshold·hysteresis·filter와 접촉 상대에 무관한 any-contact aggregation
- Initial OBB template tracking, axis continuity, occlusion과 symmetry 처리
- Goal rotation representation과 EEF-frame target의 갱신 방식
- `K_b`, `K_w`, `K_a` 및 policy/control frequency
- 현재 제외한 arm/hand dq·validity/age·추가 kinematic feature를 failure 기반으로 추가할 조건
- Reward·termination·evaluation에만 사용할 simulation privileged information의 목록과 사용 범위
- EEF-frame `delta pose`의 회전 표현, 합성 방식, component별 scale·clip과 하위 EEF controller
- Hand joint action의 대상 DoF, absolute/delta/velocity 의미, coupling과 하위 joint controller
- 상위가 지정한 pushing 준비 orientation을 Push 중·종료 시 유지할 허용 오차와 별도 최종 orientation 조건 여부
- Phase 전환, 완료와 실패 조건
- Reward 수식과 학습 curriculum
- Policy 및 network의 개수와 계층 구조
- Sensor fusion과 modality별 history window
- OBB pose·extent의 perception error 분포
- Sim-to-Real 범위와 sensor calibration
- 공간 확보의 정량적 success metric과 비교 baseline
- 최종 Method novelty, Contribution과 논문화 범위

---

## 10. 관련 문서

| 필요한 정보 | 기준 문서 |
| --- | --- |
| 정리된 research motivation·gap·가설·contribution 후보 | [`motivation.md`](./motivation.md) |
| 참고 논문·서지정보·선별 평가·baseline 후보 | [`papers/README.md`](./papers/README.md) |
| 결정 근거·변경 이력·미결 backlog·작업 우선순위 | [`context.md`](./context.md) |
