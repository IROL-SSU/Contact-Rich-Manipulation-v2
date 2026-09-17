# JH.Jeong Research Topic

> **연구 주제:** Shelf retrieval을 위한 goal-conditioned blocker-object contact manipulation
>
> **현재 중심:** Track B의 1단계 low-level policy
>
> **문서 역할:** 현재 확인된 연구 범위와 시스템 경계를 설명한다. 구현 수치와 결정 이력은 다른 문서에서 관리한다.
>
> **최종 갱신:** 2026-09-17

---

## 1. 연구를 한 문장으로 정의하면

선반 안의 target object에 접근하기 위해 blocker를 재배치해야 하는 상황에서, **지속적인 물체 pose·근사 형상 관측과 tactile·wrist F/T feedback을 이용해 선택된 OBB 면을 목표 방향에 정렬하고 그 방향으로 미는 goal-conditioned RL policy**를 연구한다.

현재 연구의 핵심은 물체를 단순히 돌린 뒤 미는 동작의 나열이 아니다. Approach에서 만든 wrist–hand configuration이 Rotation을 가능하게 하고, Rotation이 끝났을 때의 접촉 상태가 다음 Push의 실행 가능성을 결정한다. 따라서 각 단계의 성공은 다음 단계까지 이어지는지를 포함해 평가해야 한다.

연구 배경과 기존 연구 대비 gap은 [`Intro/`](./Intro/README.md), actor·reward의 구현 기준은 [`policy_learning.md`](./policy_learning.md), 문헌은 [`papers/README.md`](./papers/README.md), 결정 과정은 [`context.md`](./context.md)를 따른다.

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

| 단계 | 물체 수준의 목적 | Hand가 만들어야 하는 상태 | 단계만 따로 평가할 때 생기는 오류 |
| --- | --- | --- | --- |
| Approach / Contact Formation | Face alignment를 시작할 수 있는 관계 형성 | 선택 면·push direction에 조건화된 wrist–hand configuration과 contact state | 물체에 닿기만 했지만 회전 모멘트나 다음 push를 만들 수 없는 상태를 성공으로 판단 |
| Rotation / Pivoting | 선택 면의 pushing normal을 push direction에 정렬 | Alignment와 Push로 이어질 수 있는 aggregate contact | 면 정렬은 맞지만 Push에 불리한 hand/contact state를 성공으로 판단 |
| Push / Translation | 정렬을 유지하며 지정 방향·거리로 이동 | 안정적인 병진, 유지되는 hand–object contact와 허용 가능한 접촉력 | 이동량만 크고 lateral drift·면 회전·전도·과부하가 있는 행동을 성공으로 판단 |

Approach는 거리 감소나 최초 접촉으로 끝나지 않는다. **후속 Rotation→Push의 성공 가능성을 높이는 hand configuration을 만드는 과정**이다. 최초 접촉 이후에는 적어도 하나의 hand–object contact가 유지되는 방향을 선호하지만, 같은 finger·taxel이 계속 접촉할 필요는 없다. 필요한 contact migration·release·re-contact와 작은 hand reconfiguration을 허용하되, 불필요한 재구성을 줄일 수 있는지는 별도 가설로 검증한다.

Rotation 중 병진은 허용한다. Alignment가 일시적으로 무너지는 것도 즉시 실패가 아니라 다시 정렬할 수 있는 recoverable constraint다. 반면 blocker의 support footprint가 shelf usable boundary를 벗어나는 것, robot/object가 pillar·다른 물체와 금지된 접촉을 만드는 것, 전도·낙하·과부하는 safety violation이다. Object와 shelf support plane의 정상적인 지지 접촉은 허용한다.

### 2.3 현재의 핵심 연구 질문

> 근사 geometry와 실제 접촉·물성 사이에 차이가 있는 unseen blocker에 대해, Approach의 hand configuration과 Rotation의 terminal contact가 최종 Push까지 실행 가능하도록 shared policy를 학습할 수 있는가?

이 질문은 다음 세 부분으로 나뉜다.

1. Direct push가 어려운 조건에서 preparatory rotation이 실제 성공 영역을 넓히는가?
2. 각 단계의 즉시 성공보다 downstream feasibility를 고려하는 것이 전체 성공률을 높이는가?
3. Coarse OBB, binary tactile와 wrist F/T만으로 접촉 불확실성을 충분히 보정할 수 있는가?

---

## 3. 연구는 두 단계로 확장한다

### 3.1 현재 범위: 주어진 조작 목표의 실행

현재 우선순위는 상위 모듈이 제공한 blocker, selected OBB face, pushing direction과 distance를 실행하는 low-level policy다.

- 하나의 shared MLP policy가 Approach→Rotation→Push를 수행한다.
- Actor에는 phase ID를 주지 않는다.
- Reward gate는 현재 접촉과 task progress에서 계산하며 action 자유도를 phase별로 막지 않는다.
- Continuous Vision은 current object pose와 coarse OBB를 제공한다.
- Tactile과 wrist F/T는 접촉 후 geometry 예측과 실제 상호작용의 차이를 보정한다.

이 단계에서 **어느 OBB 면을 사용할지 선택하는 일은 상위 모듈의 역할**이다. 필요한 물체 orientation은 선택 면의 inward pushing normal과 push direction의 정렬로 유도되며 별도의 목표 quaternion으로 주지 않는다. Policy는 이 관계를 달성·유지하기 위한 wrist·finger motion과 접촉 상태를 형성·전환한다.

### 3.2 이후 확장: 공간 확보를 위한 조작 의사결정

1단계의 실행 능력이 확보되면 입력을 세부 motion goal에서 `target에 접근할 공간을 확보하라`는 목적 수준으로 확장한다. 이때 policy 또는 상위 학습 모듈이 다음 판단 일부를 맡을 수 있다.

- Preparatory rotation이 필요한지
- 어떤 intermediate orientation과 translation이 유리한지
- Rotation에서 Push로 언제 전환할지
- 공간 확보가 충분한지

첫 확장에서도 blocker 선택은 상위 시스템에 남긴다. Multi-blocker 순서 결정과 target 인출 전체를 하나의 policy로 통합하는 것은 더 먼 범위다.

### 3.3 두 단계가 공유하는 원칙

1단계와 2단계 모두 perception network 자체의 개선보다 **조작 의사결정과 contact execution**에 초점을 둔다. 1단계는 주어진 goal의 실행 성능을, 2단계는 실제 접근 공간 확보 효과를 평가한다. 두 평가 수준을 섞지 않는다.

---

## 4. 현재 시스템 계약

### 4.1 역할 분담

| 구성 요소 | 현재 역할 |
| --- | --- |
| 상위 planner | Blocker, OBB lateral face, pushing direction·distance 제공 |
| Perception | Continuous object pose와 episode-consistent OBB frame·extent 제공 |
| Track B actor | EEF와 fingers를 함께 움직여 접촉을 형성·전환하고 회전·밀기 goal 실행 |
| Simulation teacher | Exact pose·contact·force·collision 정보를 reward·termination·evaluation에만 제공 |
| Safety layer | 실제 robot의 force·collision·joint limit를 policy 밖에서도 감시 |

### 4.2 Actor가 사용하는 정보

현재 actor observation은 총 66D다.

| 정보 묶음 | 표현 | 차원 |
| --- | --- | ---: |
| Task command·selected-face state | Current EEF frame의 target position 3D + push direction 3D + selected-face pushing normal 3D | 9 |
| Current object pose | EEF-relative position 3D + canonical quaternion 4D | 7 |
| Coarse geometry | Object-local OBB extent | 3 |
| Robot configuration | UR5e joint position + RH56E2 joint position | 12 |
| Contact sensing | Current 17D binary tactile + current wrist wrench | 23 |
| Minimal action memory | Previous action 1-step | 12 |

목표 quaternion은 actor goal에서 제거한다. Target position은 최종 도달을, 고정 push direction은 경로축을, selected-face normal은 현재 alignment를 나타낸다. 현재 object quaternion은 3D pose·tilt와 OBB frame tracking을 위해 유지한다. Selected OBB face는 정확한 mesh contact patch가 아니라 물체의 접근 측면과 pushing normal을 나타내는 coarse task label이다.

Tactile은 무엇과 접촉했는지를 구분하지 않는 any-contact 신호다. 각 채널은 하나의 threshold로 이진화하며 hysteresis는 사용하지 않는다. Threshold 수치는 아직 미결이다. 정확한 collider identity와 local contact force는 actor가 아니라 simulation privileged information으로만 사용한다. 세부 좌표계, quaternion convention, sensor 전처리와 ablation은 [`policy_learning.md`](./policy_learning.md#3-observation-v03)를 따른다.

### 4.3 Action과 controller

Action은 12D로 구성한다.

$$
\mathbf a_t=
[\Delta\mathbf p_t^E,\;\Delta\boldsymbol\phi_t^E,\;\mathbf a_t^H]
\in\mathbb R^{12}.
$$

- EEF-frame delta translation: 3D
- EEF-frame local rotation increment: 3D
- RH56E2 hand joint action: 6D

EEF delta는 매 policy step의 measured state를 기준으로 DiffIK 또는 OSC command를 새로 만든다. 이전 desired target에 새 action을 누적하지 않는다. 세 phase가 같은 action space를 사용하며 Rotation 중 병진과 Push 중 orientation correction을 허용한다.

### 4.4 Policy가 직접 보지 않는 정보

Phase ID, shelf geometry, exact contact pair·force, object velocity, raw RGB·point cloud·mesh는 현재 actor 입력에서 제외한다. Selected face normal은 task command와 tracked object pose에서 계산한 배포 가능한 값이며 exact contact normal과 다르다. Shelf support footprint·boundary, forbidden collision, toppling, overload와 joint limit는 simulation의 privileged state로 cost·termination을 계산하고 실제 system에서는 별도 safety supervisor로 제한한다. Shelf geometry를 보지 않는 초기 baseline에서는 fixed shelf layout과 collision-free swept-volume margin을 만족하는 task만 sampling한다.

---

## 5. 학습 목표와 평가

### 5.1 학습 목표의 계층

Reward는 다음 네 역할을 섞지 않는다.

1. **Final task success:** 선택 면의 alignment와 물체 안정성을 유지하며 목표 위치 도달
2. **Phase-specific shaping:** Approach 탐색, face alignment progress, Push progress와 aggregate contact 유지를 지원
3. **Safety constraint:** Shelf collision, toppling, overload와 joint-limit 위반 제한
4. **Control regularization:** 불필요한 action oscillation 억제

OBB까지의 접근 거리는 Approach의 보조 shaping일 뿐 좋은 hand configuration의 정의가 아니다. Approach action은 shared policy의 미래 Rotation·Push reward와 final success return을 통해 학습한다. 장기 credit assignment가 부족하다고 확인될 때만 learned downstream-feasibility shaping을 비교한다.

### 5.2 단계 연결을 평가하는 지표

Approach readiness state에서 continuation rollout을 시작해 두 확률을 구분한다.

- $F_R$: 목표 Rotation에 성공할 확률—Approach가 Rotation에 적합했는지 보는 진단 지표
- $F_{R\rightarrow P}$: 최종 Push까지 성공할 확률—Approach의 primary downstream metric

Rotation terminal state에서는 face-alignment success와 subsequent Push success를 별도로 기록한다. 이를 통해 `면 정렬은 성공했지만 Push에는 나쁜 contact state`를 구분한다.

### 5.3 최종 평가 수준

| 평가 수준 | 핵심 질문 | 주요 지표 |
| --- | --- | --- |
| Skill execution | 선택 면 정렬과 병진 goal을 정확하고 안전하게 실행하는가 | Alignment/Push success, path error, force, collision, topple, time |
| Phase transition | 앞 단계의 종료 상태가 다음 단계를 가능하게 하는가 | $F_R$, $F_{R\rightarrow P}$, immediate-push success, aggregate contact loss |
| Contact efficiency | 전체 접촉을 유지하면서 필요한 만큼만 재구성하는가 | Contact-loss event, contact switch, first-contact 이후 hand joint travel |
| Generalization | 관측·물성이 달라져도 성능을 유지하는가 | Held-out geometry·friction·mass success와 seen–unseen gap |
| System utility | Blocker 조작이 target 접근에 실제 도움이 되는가 | Visibility·reachability·clearance와 target retrieval success |

현재 1단계의 primary 평가는 앞의 세 수준이며, system utility는 2단계 확장에서 본격적으로 다룬다.

---

## 6. 현재 범위와 미결 사항

### 6.1 현재 확정·working baseline

- 연구 중심은 Track B다.
- Continuous Vision, OBB, binary tactile와 wrist F/T를 함께 사용한다.
- Actor는 phase-ID-free shared MLP이며 observation은 66D다.
- Action은 measured-state-referenced EEF delta pose와 hand joint action이다.
- Approach는 downstream-ready hand configuration을 만드는 과정이다.
- Rotation 중 병진을 허용하며 face-alignment violation은 recoverable하다.
- 최초 접촉 후 aggregate hand–object contact 유지를 선호하되 contact configuration 변화는 허용한다.
- Shelf support boundary 이탈과 pillar·다른 물체·구조물 접촉은 safety violation이다.
- Reward·safety·regularization의 역할을 분리한다.

### 6.2 아직 확정하지 않은 항목

- DiffIK와 OSC 중 최종 controller 및 action scale·frequency
- Reward weight, gate threshold와 safety-learning algorithm
- Approach readiness snapshot의 정확한 수집 조건
- OBB lateral-face 후보와 episode-consistent face tracking 규칙
- Initial non-contact pose와 feasible swept-volume sampling 규칙
- Aggregate contact-loss penalty와 hand-reconfiguration mild-cost ablation
- 실제 RH56E2 tactile mapping, single-threshold 수치와 wrist F/T 전처리 수치
- Policy·sensor rate, latency와 interval aggregation
- 1단계 이후 목표 선택·전환 판단을 통합하는 구체 architecture
- 최종 method novelty와 논문 contribution 문구

미결 항목의 이유와 과거 제안은 [`context.md`](./context.md)의 backlog에서 관리한다.

---

## 7. 다음에 읽을 문서

1. 문제의 필요성과 문헌상 gap: [`Intro/`](./Intro/README.md)
2. Actor·reward의 구현 명세: [`policy_learning.md`](./policy_learning.md)
3. 목적별 논문과 독해 순서: [`papers/README.md`](./papers/README.md)
4. 결정 근거와 변경 이력: [`context.md`](./context.md)
