# JH.Jeong Research Topic

> **연구 주제:** Shelf retrieval을 위한 goal-conditioned blocker-object contact manipulation
>
> **현재 중심:** Track B의 1단계 low-level policy
>
> **문서 역할:** Intro에서 합의한 연구 문제와 actor observation을 요약하고, 아직 협의 중인 항목과 구분한다.
>
> **현재 합의 범위:** Intro와 actor observation까지
>
> **협의 중:** Policy architecture, action/controller, reward, termination, critic·RL 세부 방법, evaluation, experiment
>
> **최종 갱신:** 2026-09-24

---

## 1. 연구를 한 문장으로 정의하면

선반 안의 target object에 접근하기 위해 blocker를 재배치해야 하는 상황에서, **지속적인 물체 pose·근사 형상 관측과 tactile·wrist F/T feedback을 이용해 선택된 OBB 면을 목표 방향에 정렬하고 그 방향으로 미는 goal-conditioned low-level policy**를 연구한다. Intro에서 RL은 조건부 primary method family로, selected-face goal과 66D actor observation은 현재 작업 기준으로 정리했다. Policy architecture와 action/controller 이후의 설계는 아직 협의 중이며, 이 선택들 자체를 contribution으로 전제하지 않는다.

현재 연구의 핵심은 물체를 단순히 돌린 뒤 미는 동작의 나열이 아니다. Approach에서 만든 wrist–hand configuration은 Rotation에, Rotation이 끝났을 때의 접촉 상태는 다음 Push의 실행 가능성에 영향을 줄 수 있다. 이 단계 간 의존성은 문제 정의에 포함하지만, 이를 측정할 지표와 평가 절차는 아직 협의 중이다.

연구 배경과 기존 연구 대비 gap은 [`Intro/`](./Intro/README.md), actor observation 명세와 이후 설계 논의는 [`policy_learning.md`](./policy_learning.md), 문헌은 [`papers/README.md`](./papers/README.md), 결정 과정은 [`context.md`](./context.md)를 따른다.

---

## 2. 어떤 문제를 다루는가

### 2.1 응용 시나리오

선반 안의 target object가 다른 물체에 가려지거나 접근 경로가 blocker에 막혀 있다고 가정한다. 상위 시스템은 조작할 blocker, 사용할 OBB lateral face, push direction과 distance를 정한다. Track B policy는 hand–object non-contact 상태에서 시작해 다음 과정을 수행한다.

```text
Approach / Contact Formation
        ↓
Rotation / Pivoting
        ↓
Push / Translation
        ↓
Target을 위한 접근 공간 확보
```

여기서 policy가 직접 조작하는 대상은 blocker다. Target의 인출은 전체 시스템의 목적이지만 현재 1단계 policy가 직접 수행하는 동작은 아니다.

### 2.2 왜 세 단계를 하나의 연속 문제로 보는가

| 단계 | 물체 수준의 목적 | Hand가 만들어야 하는 상태 | 단계만 따로 볼 때 놓치는 점 |
| --- | --- | --- | --- |
| Approach / Contact Formation | Face alignment를 시작할 수 있는 관계 형성 | 선택 면·push direction에 조건화된 wrist–hand configuration과 contact state | 물체에 닿기만 했지만 회전 모멘트나 다음 push를 만들 수 없는 상태를 성공으로 판단 |
| Rotation / Pivoting | 선택 면의 pushing normal을 push direction에 정렬 | 이후 Push로 이어질 수 있는 hand/contact state | 면 정렬은 맞지만 Push에 불리한 hand/contact state를 성공으로 판단 |
| Push / Translation | 정렬을 이용해 지정 방향·거리로 이동 | 목표 병진을 만들 수 있는 hand/contact state | 이동량만 크고 lateral drift·면 회전·전도·과부하가 있는 행동을 성공으로 판단 |

Approach는 거리 감소나 최초 접촉으로 끝나지 않는다. **후속 Rotation→Push의 성공 가능성을 높이는 hand configuration을 만드는 과정**이다. 최초 접촉 이후의 contact migration·release·re-contact와 hand reconfiguration을 어느 범위까지 허용할지는 연구 질문에 포함되며, 구체적인 학습 및 검증 방식은 아직 협의 중이다.

Rotation 중 병진과 alignment의 일시적 이탈을 허용할 가능성을 열어 둔다. Blocker의 boundary 이탈, 전도·낙하·과부하는 피해야 할 상태지만, 이를 reward, cost 또는 termination 중 무엇으로 처리할지는 아직 협의 중이다. Object와 shelf support plane의 정상적인 지지 접촉은 허용한다. Shelf sidewall·pillar 같은 auxiliary fixed structure와 주변 movable object의 접촉을 금지·허용·이용 중 어디까지 둘지도 아직 정하지 않았다.

### 2.3 현재의 핵심 연구 질문

> 근사 geometry와 실제 접촉·물성 사이에 차이가 있는 다양한 blocker에 대해, Approach의 hand configuration과 Rotation의 terminal contact가 최종 Push까지 실행 가능하도록 goal-conditioned low-level policy를 학습할 수 있는가?

이 질문은 다음 네 부분으로 나뉜다.

1. Direct push가 어려운 조건에서 preparatory rotation이 실제 성공 영역을 넓히는가?
2. 각 단계의 즉시 성공보다 downstream feasibility를 고려하는 것이 전체 성공률을 높이는가?
3. Coarse OBB, binary tactile와 wrist F/T만으로 접촉 불확실성을 충분히 보정할 수 있는가?
4. 같은 wrist controller에서 finger configuration을 contact 전에만 정하는 방식보다 실행 중 갱신하는 방식이 uncertainty와 recovery에 추가 이득을 주는가?

---

## 3. 현재 단계와 가능한 후속 확장

### 3.1 현재 범위: 주어진 조작 목표의 실행

현재 다루는 1단계는 상위 모듈이 제공한 blocker, selected OBB face, pushing direction과 distance를 실행하는 low-level policy다.

- Actor observation에는 phase ID를 주지 않는다.
- Continuous Vision은 current object pose와 coarse OBB를 제공한다.
- Tactile과 wrist F/T는 접촉 후 geometry 예측과 실제 상호작용의 차이를 보정하기 위한 feedback으로 사용한다.
- `[Open]` Shared MLP 여부, phase별 policy 분리 여부와 phase 전환 방식은 아직 협의 중이다.
- `[Open]` Reward gate와 action 자유도 제한 여부도 아직 협의 중이다.

현재 goal baseline에서는 **어느 OBB 면을 사용할지 선택하는 일은 상위 모듈의 역할**이다. 필요한 물체 orientation은 선택 면의 inward pushing normal과 push direction의 정렬로 유도하며 별도의 목표 quaternion을 주지 않는다. Policy는 이 관계를 달성·유지하기 위한 wrist·finger motion과 접촉 상태를 형성·전환한다. Explicit desired rotation/orientation이 필요한 조건은 open decision이다.

### 3.2 가능한 후속 확장: 공간 확보를 위한 조작 의사결정

1단계 이후에는 입력을 세부 motion goal에서 `target에 접근할 공간을 확보하라`는 목적 수준으로 넓히는 방향을 검토할 수 있다. 이는 현재 합의된 roadmap이 아니며, 이때 policy 또는 상위 학습 모듈이 다음 판단 일부를 맡을 가능성이 있다.

- Preparatory rotation이 필요한지
- 어떤 intermediate orientation과 translation이 유리한지
- Rotation에서 Push로 언제 전환할지
- 공간 확보가 충분한지

첫 확장에서도 blocker 선택은 상위 시스템에 남긴다. Multi-blocker 순서 결정과 target 인출 전체를 하나의 policy로 통합하는 것은 더 먼 범위다.

### 3.3 두 단계가 공유하는 원칙

1단계와 2단계 모두 perception network 자체의 개선보다 **조작 의사결정과 contact execution**에 초점을 둔다. 1단계는 주어진 goal의 실행을, 2단계는 실제 접근 공간 확보를 다룬다는 범위 차이가 있다. 구체적인 평가 수준과 지표는 아직 정하지 않았다.

---

## 4. 현재 시스템 경계와 actor observation

### 4.1 역할 분담

| 구성 요소 | 현재 역할 | 상태 |
| --- | --- | --- |
| 상위 planner | Blocker, OBB lateral face, pushing direction·distance 제공 | 합의 범위 |
| Perception | Continuous object pose와 episode-consistent OBB frame·extent 제공 | 합의 범위 |
| Track B actor | 관측을 바탕으로 blocker의 회전·밀기 goal을 실행 | 합의 범위 |
| Simulation의 정확한 상태 | Actor observation에서는 제외 | 합의 범위 |
| Simulation의 정확한 상태를 reward·termination·evaluation에 사용하는 방식 | 용도와 범위를 추후 결정 | `[Open]` |
| Safety layer | 실제 robot의 force·collision·joint limit 감시 방식 | `[Open]` |

### 4.2 Actor가 사용하는 정보

현재 actor observation은 총 66D다.

| 정보 묶음 | 표현 | 차원 |
| --- | --- | ---: |
| Task command·selected-face state | Current EEF frame의 target position 3D + push direction 3D + selected-face pushing normal 3D | 9 |
| Current object pose | EEF-relative position 3D + canonical quaternion 4D | 7 |
| Coarse geometry | Object-local OBB extent | 3 |
| Robot configuration | UR5e joint position + RH56E2 joint position | 12 |
| Contact sensing | Current 17D binary tactile + current wrist wrench | 23 |
| Minimal action memory | Previous action 1-step; 현재 12D action 후보를 전제로 하며 action 변경 시 차원 재확인 | 12 |

목표 quaternion은 actor goal에서 제거한다. Target position은 최종 도달을, 고정 push direction은 경로축을, selected-face normal은 현재 alignment를 나타낸다. 현재 object quaternion은 3D pose·tilt와 OBB frame tracking을 위해 유지한다. Selected OBB face는 정확한 mesh contact patch가 아니라 물체의 접근 측면과 pushing normal을 나타내는 coarse task label이다.

Tactile은 무엇과 접촉했는지를 구분하지 않는 any-contact 신호다. 각 채널은 하나의 threshold로 이진화하며 hysteresis는 사용하지 않는다. Threshold 수치는 아직 미결이다. 정확한 collider identity와 local contact force는 actor observation에서 제외한다. 세부 좌표계, quaternion convention, sensor 전처리와 검토 중인 대안은 [`policy_learning.md`](./policy_learning.md#3-observation-v03)를 따른다.

### 4.3 Action과 controller — `[Open]`

아래 12D action interface는 66D observation에 포함된 previous action의 현재 전제다. 최종 action 표현, controller, scale과 frequency는 아직 합의하지 않았다.

$$
\mathbf a_t=
[\Delta\mathbf p_t^E,\;\Delta\boldsymbol\phi_t^E,\;\mathbf a_t^H]
\in\mathbb R^{12}.
$$

- EEF-frame delta translation: 3D
- EEF-frame local rotation increment: 3D
- RH56E2 hand joint action: 6D

Measured-state-referenced EEF delta, DiffIK 또는 OSC, 세 단계의 action space 공유 여부와 Rotation 중 병진·Push 중 orientation correction 허용 범위는 모두 협의 중이다. Action interface가 바뀌면 previous-action observation의 차원도 함께 다시 확인해야 한다.

### 4.4 Policy가 직접 보지 않는 정보

Phase ID, shelf geometry, exact contact pair·force, object velocity, raw RGB·point cloud·mesh는 현재 actor 입력에서 제외한다. Selected face normal은 task command와 tracked object pose에서 계산한 배포 가능한 값이며 exact contact normal과 다르다. Shelf support footprint·boundary, forbidden collision, toppling, overload와 joint limit도 actor observation에는 포함하지 않는다. 이 정보를 reward·termination·evaluation 또는 별도 safety supervisor에서 어떻게 사용할지, S0와 S1을 어떤 순서와 조건으로 다룰지는 모두 협의 중이다.

---

## 5. 학습 목표와 평가 — `[Open]`

Reward, termination, evaluation metric과 experiment protocol은 아직 합의하지 않았다. 아래 내용은 앞서 나온 아이디어를 잃지 않기 위한 **논의 후보**이며, 현재 baseline이나 실험 계획을 뜻하지 않는다.

### 5.1 Reward에서 구분할 수 있는 역할

Reward를 구체화할 때 다음 역할을 구분할 필요가 있는지 검토한다.

1. **Final task success:** 선택 면의 alignment와 물체 안정성을 유지하며 목표 위치 도달
2. **Phase-specific shaping:** Approach 탐색, face alignment progress, Push progress와 aggregate contact 유지를 지원
3. **Safety constraint:** Boundary 이탈, toppling·낙하, overload, joint-limit과 OD-1에서 금지한 auxiliary-structure·surrounding-object contact 제한
4. **Control regularization:** 불필요한 action oscillation 억제

OBB까지의 접근 거리, 미래 Rotation·Push 결과, 별도의 downstream-feasibility 신호 중 무엇을 실제 학습 신호로 사용할지는 아직 정하지 않았다.

### 5.2 단계 연결을 보는 지표 후보

단계 간 의존성을 살피는 한 가지 아이디어로, Approach 이후 상태에서 continuation rollout을 수행해 다음 두 값을 구분할 수 있다.

- $F_R$: 목표 Rotation에 성공할 확률 후보
- $F_{R\rightarrow P}$: 최종 Push까지 성공할 확률 후보

이 구분은 `면 정렬은 가능하지만 Push에는 불리한 contact state`를 살펴보기 위한 아이디어다. 두 값의 채택 여부, 상태 수집 조건과 추정 방법은 모두 협의 중이다.

### 5.3 평가에서 논의할 질문

| 검토 관점 | 핵심 질문 | 지표 예시 | 상태 |
| --- | --- | --- | --- |
| Task execution | 주어진 goal을 실행하는가 | Final success, 이동·정렬 오차 | `[Open]` |
| Phase outcome | 각 과정에서 무엇을 달성했는가 | Contact formation, Rotation, Push 결과 | `[Open]` |
| Transition quality | 앞 과정의 결과가 다음 과정을 가능하게 하는가 | $F_R$, $F_{R\rightarrow P}$ 등의 continuation 지표 | `[Open]` |
| Robustness | 관측·물성 변화에서도 동작하는가 | 조건 변화에 따른 성능 차이 | `[Open]` |
| Safety | 환경·물체·robot에 위험을 만들지 않는가 | 충돌, boundary 이탈, 전도, 과도한 힘 | `[Open]` |
| Contact/control efficiency | 불필요한 접촉·hand motion을 줄이는가 | Contact loss·switch, hand motion | `[Open]` |
| Target 접근 효과 | Blocker 조작이 target 접근에 도움이 되는가 | Visibility, reachability, clearance | `[Open]` |

어느 관점을 우선할지, 어떤 metric과 비교 조건을 사용할지, 실험을 어떤 순서로 수행할지는 아직 정하지 않았다.

---

## 6. 현재 범위와 미결 사항

### 6.1 현재 합의된 범위

- 연구 중심은 Track B다.
- 상위 모듈은 blocker, selected OBB face, pushing direction과 distance를 제공한다.
- Continuous Vision의 current object pose·coarse OBB와 binary tactile·wrist F/T를 함께 사용한다.
- Actor observation은 4.2절의 66D 구성이며 phase ID는 포함하지 않는다.
- Approach는 downstream-ready hand configuration을 만드는 과정이다.
- 어느 OBB 면을 사용할지는 1단계 policy가 아니라 상위 모듈이 정한다.

### 6.2 `[Open]` 아직 합의하지 않은 항목

- Shared MLP, phase별 policy 등 구체적인 policy architecture와 phase 전환 방식
- 최종 action 표현, DiffIK·OSC 중 controller, action scale·frequency
- Rotation 중 병진, Push 중 orientation correction과 contact reconfiguration의 허용 범위
- Reward 항, weight, gate, termination과 safety-learning 방식
- Evaluation metric, 비교 조건, data split과 experiment protocol 전체
- S0·S1의 역할과 검증 순서
- Approach readiness snapshot의 정확한 수집 조건
- OBB lateral-face 후보와 episode-consistent face tracking 규칙
- Initial non-contact pose와 feasible swept-volume sampling 규칙
- S1의 surrounding-object 수·배치·관측 범위와 접촉 규칙
- Held-out object의 category·shape·physics 범위
- Aggregate contact-loss와 hand reconfiguration을 학습·평가에서 다루는 방식
- 실제 RH56E2 tactile mapping, single-threshold 수치와 wrist F/T 전처리 수치
- Policy·sensor rate, latency와 interval aggregation
- 1단계 이후 목표 선택·전환 판단을 통합하는 구체 architecture
- 최종 method novelty와 논문 contribution 문구

미결 항목의 이유와 과거 제안은 [`context.md`](./context.md)의 backlog에서 관리한다.

---

## 7. 다음에 읽을 문서

1. 문제의 필요성과 문헌상 gap: [`Intro/`](./Intro/README.md)
2. Actor observation 명세와 이후 설계 논의: [`policy_learning.md`](./policy_learning.md)
3. 목적별 논문과 독해 순서: [`papers/README.md`](./papers/README.md)
4. 결정 근거와 변경 이력: [`context.md`](./context.md)
