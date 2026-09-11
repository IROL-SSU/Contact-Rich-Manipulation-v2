```markdown
# Research Hand-Off — Shelf Manipulation RL

## 0. 현재 상태 한 줄 요약

현재 연구는 기존 **Vision-based Sweeping**을 출발점으로 하되, 최신 논의에서는 연구를 처음부터 Sweeping/Handling이라는 두 동작으로 고정하기보다 다음 두 축으로 **Decouple하여 병렬 연구한 뒤 최종적으로 통합**하는 방향으로 재정리되고 있다.

1. **Track A — Vision-Free Force/Torque·Tactile 기반 Contact-rich Manipulation**
2. **Track B — Goal-conditioned Omnidirectional Object Manipulation**

기존에 진행한 Cartesian/OSC sweeping 환경은 최종 연구 그 자체라기보다, 두 연구를 진행하기 위한 **Low-level 기반 및 Warm-up/Feasibility 환경**으로 본다.


---

# 1. 전체 프로젝트 맥락

궁극적인 시스템 목표는 선반 환경에서 **주변 물체를 조작하여 가려진 영역을 드러내고, 목표 물체를 탐색하는 것**이다.

전체 구조는 대략 다음과 같다.

- Perception / VLM 등으로 장면 관측
- 상위 Planner가 조작 대상과 목표 결정
- Conventional Motion Planning을 통해 해당 물체 근처까지 이동
- Low-level RL Manipulation Skill 실행
- 물체 이동 후 다시 관측
- 필요한 경우 다음 Manipulation Skill 실행

여기서 **상위 Multi-step Planning 자체는 현재 Low-level 졸업연구의 핵심 Method가 아니다.**

상위 시스템은 주로 다음을 설명하기 위한 배경이다.

- 왜 이러한 Manipulation Skill이 필요한가?
- 왜 Vision이 가려지는 상황이 발생하는가?
- 왜 목표 방향·거리 등에 따라 다양한 조작이 필요한가?

즉, 전체 시스템을 모두 완성해야 Low-level 연구가 성립하는 구조로 보지 않는다.


---

# 2. 출발점: 기존 Vision-based Sweeping의 한계

기존 김태호 연구는 Vision-based Sweeping을 기반으로 한다.

현재 문제의식은 다음과 같다.

## 2.1 Occlusion

조작이 시작되면

- Robot Hand
- Manipulator
- 주변 물체

등에 의해 대상 물체가 가려질 수 있다.

따라서 **조작 중 지속적인 물체 Pose 관측을 항상 보장하기 어렵다.**

이 문제에서 Vision-Free manipulation의 가장 강한 Motivation을 가져간다.


## 2.2 Vision과 반응 속도

기존 논의에서는 Vision perception 속도에 의해 정책 판단 주기가 제한되고, 이에 따라 접촉 변화에 대한 빠른 대응이 어려울 수 있다는 문제를 제기했다.

다만 다음을 연구의 전제로 단정하면 안 된다.

> Vision을 사용하면 반드시 느리고 보수적인 정책이 학습된다.

보다 안전한 주장은 다음이다.

> 조작 중 안정적인 Vision tracking을 보장하기 어려운 상황에서, 고주기의 힘·접촉 관측을 이용한 정책이 시각 추적 없이도 안정적인 manipulation을 수행할 수 있는지 검증한다.

**빠른 실행이나 높은 적응성은 가정이 아니라 실험을 통해 보여줘야 할 결과이다.**


---

# 3. 이전까지의 Task 분리안

최신 미팅 이전에는 두 Skill을 다음과 같이 구분하는 안을 고려했다.

| Skill | 정의 |
| --- | --- |
| Rotation-Constrained Sweeping | 물체를 병진 이동시키되 의도하지 않은 Rotation을 억제 |
| Handling / Pivoting | Rotation을 주된 운동으로 사용하며 일부 Translation 허용 |

이 구분의 목적은 두 Skill의 역할 중복을 줄이는 것이었다.

### Sweeping이 필요한 상황

- 병진 방향으로 이동 공간은 존재함
- 물체가 회전하면 모서리나 돌출부가 주변 영역과 간섭함
- 따라서 방향을 유지하면서 병진시키는 것이 중요함

### Handling이 필요한 상황

- 짧은 병진만으로는 원하는 공간을 확보하기 어려움
- 물체의 방향을 변경하면 특정 영역을 더 효과적으로 열 수 있음
- 엄밀한 고정 Pivot은 요구하지 않고 Rotation + Small Translation 허용


---

# 4. 최신 미팅 이후 변경된 연구 구조

## 중요

**위의 Sweeping / Handling 2분법을 현재 연구의 최상위 구분으로 고정하지 않는다.**

최신 미팅에서 교수님은 연구를 다른 기준으로 분리할 것을 제안했다.

핵심은

> **Force/Torque·Tactile을 사용하는 문제와, 다양한 Goal에 따라 물체를 조작하는 문제를 일단 Decouple하자.**

는 것이다.


---

# 5. Track A — Vision-Free Force/Torque·Tactile Contact Manipulation

## 5.1 핵심 질문

> 조작 중 물체의 Vision tracking 없이, 초기 정보와 F/T·Tactile·Proprioception을 이용하여 Contact-rich manipulation의 성능을 향상시킬 수 있는가?


## 5.2 중요한 점

단순히

> Force/Torque가 들어가면 물체를 밀 수 있다.

에서 끝나면 연구가 약하다.

연구할 내용은 **Contact sensing을 사용했을 때 manipulation quality가 어떻게 변하는가**이다.

예:

- Contact 유지
- Contact loss 감소
- Excessive force 억제
- 전도/불안정 감소
- 더 빠른 조작 가능성
- 물체 특성 변화에 대한 적응
- Force/Torque history의 효과
- Tactile history의 효과
- Sensor combination에 따른 차이


## 5.3 예상 Observation

후보:

- Wrist 6-axis F/T
- Hand/Finger tactile
- Finger force
- Joint position / velocity
- EEF state
- Contact history

물체의 Ground Truth Pose는 학습 Reward 및 Evaluation에는 사용할 수 있으나, Vision-Free 조건에서는 실행 Policy의 Observation에서 제외하는 방향이 기본이다.


## 5.4 연구해야 할 것

- F/T Observation 구성
- Tactile Observation 구성
- History length
- RNN 필요성
- Reward 구성
- Contact 유지 조건
- Controller parameter
- Sensor ablation
- Sim-to-Real
- Sensor placement
- Hand configuration


---

# 6. Track B — Goal-conditioned Omnidirectional Manipulation

## 6.1 핵심 질문

> 상위 Planner가 주는 Manipulation Goal을 기반으로, 하나의 정책이 다양한 방향·거리 또는 회전에 해당하는 물체 조작을 수행할 수 있는가?


## 6.2 기존 정책과의 관계

현재 기존 Sweeping Policy도 실제로는 단순한

- Left
- Right

명령을 입력받는 구조가 아니라 **Target coordinate를 목표로 이동하는 형태**이다.

다만 기존 학습 환경에서 Depth 방향 variation이 작기 때문에 결과적으로

- 왼쪽 Sweep
- 오른쪽 Sweep

정도의 동작만 나타나는 상태이다.

따라서 기존 환경을 활용하여 Goal distribution을 확장하는 Feasibility를 먼저 확인할 수 있다.


## 6.3 단계적 확장안

### Step 1 — Direction

우선 다양한 방향의 목표를 준다.

예:

- 0°
- 15°
- 30°
- 45°

등.


### Step 2 — Direction + Distance

단순 방향이 아니라

- Goal direction
- Goal displacement

를 함께 조건으로 준다.


### Step 3 — 더 일반적인 Manipulation

Translation만 고정하지 않고 다음도 검토할 수 있다.

- Lateral sweeping
- Inward pushing
- Diagonal pushing
- Object rotation
- 특정 Corner를 이용한 회전
- 길쭉한 물체의 orientation 변경


## 6.4 연구 관점

이를 단순히

> 기존 Sweeping의 방향을 늘린다.

정도로 보지 않는다.

보다 큰 문제는

> **Goal-conditioned contact manipulation에서 어떤 조작 전략이 학습되는가?**

이다.

따라서 기존의 Handling/Pivoting 아이디어도 이 Track 안에서 **Rotation Goal 또는 조작 전략의 한 종류**로 흡수될 수 있다.


## 6.5 Open Problem 형태 허용

처음부터

> 반드시 모든 방향과 회전을 성공시키는 범용 정책을 완성한다.

고 고정할 필요는 없다.

다음과 같은 탐색 자체가 연구가 될 수 있다.

- Goal representation을 어떻게 해야 하는가?
- 어느 방향 범위까지 학습 가능한가?
- Direction → Distance 확장이 가능한가?
- Translation에서 Rotation으로 확장 가능한가?
- 어떤 Reward에서 어떤 전략이 나타나는가?
- Hand configuration은 어떻게 바뀌는가?


---

# 7. Track A와 Track B를 분리하는 이유

두 문제를 처음부터 하나의 Policy에 전부 넣으면 연구가 지나치게 복잡해진다.

특히 Track A는 실제 Sensor를 사용하면서 다음 문제가 발생한다.

- Sensor noise
- Contact simulation discrepancy
- Calibration
- Tactile modeling
- F/T Sim-to-Real
- 실제 Hand integration

따라서 Sim-to-Real에 상당한 시간이 필요할 가능성이 있다.

반면 Track B는 F/T·Tactile이 완성되지 않아도 Simulation에서 상당한 실험을 바로 진행할 수 있다.

따라서 현재 전략은

- Track A: **Sensing / Contact adaptation 문제**
- Track B: **Goal-conditioned Manipulability 문제**

를 별도로 연구한 뒤, 충분한 Insight가 확보되면 최종적으로 합치는 것이다.

최종적으로는

**Goal-conditioned + Vision-Free Contact Manipulation**

형태로 통합할 가능성을 열어둔다.


---

# 8. Vision 사용 조건에 대한 현재 해석

이전 정리에서는

> Sweeping과 Handling 모두 Vision-Free여야 한다.

는 논리로 정리했었다.

최신 미팅 이후에는 이를 그대로 고정하지 않는다.

### Track A

명확하게 **Vision-Free during manipulation**을 핵심 조건으로 가져간다.

- 초기 Perception은 가능
- 물체 근처까지 접근하는 데 Vision 사용 가능
- Manipulation 시작 이후 지속적인 Object Pose tracking에 의존하지 않음


### Track B

현재 핵심은 Vision-Free 여부보다 **Goal-conditioned manipulation capability 자체**이다.

따라서 초기 연구에서는 F/T·Tactile을 반드시 포함하지 않아도 된다.

상위단에서 Vision으로 대상과 Goal을 결정한 뒤 해당 Goal을 수행하는 Policy로 연구할 수 있다.

향후 Track A와 통합하면서 Vision dependency를 줄이는 방향을 검토할 수 있다.

**따라서 Track B까지 억지로 처음부터 Vision-Free로 만들어야 한다고 현재 확정하면 안 된다.**


---

# 9. Geometry 정보 문제

기존 Sweeping / Handling 논의 과정에서 중요한 미해결 문제가 존재한다.

## 9.1 왜 Geometry가 필요한가?

### Rotation-Constrained Sweeping

물체가 얼마나 회전하면 주변 공간을 침범하는지 판단하려면

- Orientation
- Width
- Depth
- Shape

등이 중요하다.

### Rotation / Handling

어디를 접촉해야 원하는 회전을 만들 수 있는지 판단하려면 물체의 형상 정보가 도움이 된다.


## 9.2 검토한 Geometry 입력

High-Level perception에서 대략적인 Geometry를 추정하여 전달하는 방안을 고려했다.

예:

- Shape Type: Box / Cylinder / Capsule ...
- Dimension: `d1, d2, d3`
- Initial Pose


## 9.3 문제점

실제 선반에서는 Occlusion 때문에 Geometry가 정확하지 않을 수 있다.

발생 가능한 오류:

- Dimension error
- Systematic bias
- Shape misclassification
- Partial observation
- Self-occlusion


## 9.4 아직 해결되지 않은 질문

> Simulation의 Ground Truth Geometry에 단순 Random Noise를 추가하는 것이 실제 Perception Error를 충분히 표현할 수 있는가?

아직 답이 없다.

또한 최신 연구 구조에서는 **Geometry를 반드시 Policy Observation으로 줘야 하는지부터 다시 검토할 필요가 있다.**

특히 Track B에서 다양한 Goal을 수행하게 만들 경우 Geometry의 필요성이 더 커질 수도 있다.

따라서 Geometry는 현재 **확정된 입력이 아니라 Open Issue**로 유지한다.


---

# 10. 현재 구현된 Low-Level 기반

현재 환경은 기존 Sweeping 연구를 기반으로 다음 변경이 이루어졌다.

- Joint-space Action → Cartesian / EEF-space Action
- OSC 적용
- Relative Observation 적용
- Initial Robot Pose Randomization 확대
- Object position variation 확대
- 다양한 Joint configuration에서 Sweeping 가능성 확인

즉, 기존 정책을 그대로 사용하는 상태는 아니다.

다만 아직 다음은 구현되지 않았다.

- Vision-Free F/T policy
- Tactile-based policy
- 실제 Sensor Sim-to-Real
- Goal-conditioned Omnidirectional Manipulation
- 실제 Approach Error를 반영한 Initial Pose Distribution


---

# 11. Initial Pose Randomization — 중요 수정 사항

현재 Initial Pose Randomization은 교수님이 원래 의도한 구조와 완전히 일치하지 않는다.

## 현재까지 한 것

넓은 공간에서 다양한 EEF / Joint State로 시작하도록 Randomization하고, 그 상태에서 물체를 밀 수 있는지 확인했다.

## 실제 필요한 구조

RL이 장거리 Reaching까지 모두 담당하는 것이 아니다.

전체 흐름은 다음과 같아야 한다.

1. Perception에서 Target Object 인식
2. Conventional Motion Planner로 Target 근처까지 이동
3. 실제 도착 Pose에는 Perception / Calibration / Control Error 존재
4. 해당 오차가 포함된 Pose에서 RL Manipulation 시작

즉, RL의 Initial Pose Distribution은 단순히 넓은 Workspace를 Uniform하게 Randomize하는 것이 아니라,

> **실제 Motion Planner가 Target 옆으로 접근했을 때 발생하는 Relative Pose Uncertainty**

를 반영해야 한다.


## 필요한 실험

실제 Robot에서 Approach를 반복하여

- Target Object ↔ EEF Relative Position Error
- Relative Orientation Error

를 수집한다.

이를 Distribution 또는 Ellipsoid 형태로 모델링한 뒤 Simulation의 Initial Pose Randomization에 적용한다.


---

# 12. Reaching의 담당 범위

현재 중요하게 정리해야 할 사항이다.

**장거리 Reaching은 Manipulation RL의 핵심 연구 대상이 아니다.**

Conventional Motion Planner 또는 별도 접근 정책이 대상 근처까지 로봇을 이동시킨다.

RL Manipulation Policy는 그 근처에서 시작한다.

다만 실제 Approach 오차가 있으므로

- Object 바로 옆의 고정된 정확한 Pose
- 이미 Contact가 형성된 Pose

에서만 시작하도록 만들면 안 된다.

즉,

> **Target 근처의 Non-contact Pose + 현실적인 접근 오차**

가 기본 Initial Condition이 된다.


---

# 13. Sequential Manipulation Feasibility

현재 Random Initial Pose 학습이 실제로 의미가 있는지 검증하기 위해 다음 실험이 필요하다.

예:

1. Object A를 오른쪽으로 Sweep
2. Home Pose로 복귀하지 않음
3. 다음 Object 근처로 이동
4. Object B를 왼쪽 또는 다른 방향으로 Manipulation

확인할 것:

- Homing 없이 다음 Skill 실행 가능 여부
- 이전 Manipulation에서 달라진 Joint Configuration에 대한 Robustness
- 서로 다른 Target 위치에서 동일 Policy 재사용 가능 여부

이 실험은 현재 Low-level framework의 유효성을 검증하는 Feasibility Test이다.


---

# 14. Tactile Sensor 현재 문제

현재 테스트한 Piezoresistive 계열 Tactile Sensor는 예상보다 Contact Detection이 둔한 문제가 있다.

관찰된 문제:

- 가벼운 물체에서 신호가 매우 작음
- 넓은 접촉 면적에서는 압력이 분산됨
- 무거운 물체로 바꿔도 충분하지 않은 경우 존재
- 좁은 면적에 힘이 집중될 때 상대적으로 검출이 쉬움

현재 연구의 목적은 Sensor 자체를 개발하는 것이 아니므로, 이 문제에 과도하게 매몰되지 않는다.


## 검토할 대안

- Wrist F/T 사용
- UR5e Joint Torque 변화 사용 가능성 확인
- 더 무거운 물체 사용
- 물체 내부에 분동 삽입
- Shelf/Object 마찰 증가
- Contact area 조정
- 다른 Tactile sensor 검토


## 추가적으로 중요한 연구 변수

Hand 기반 Manipulation에서는 접촉 위치가 고정되지 않을 수 있다.

예:

- Palm
- Back of hand
- Finger
- Hand inner surface
- Hand outer surface

따라서 **Tactile Sensor Placement 자체도 연구 변수**가 될 수 있다.


---

# 15. 실험 방식에 대한 방향

최종적으로 가장 잘된 결과 하나만 보여주는 방향보다는, 다양한 설계를 체계적으로 바꾸면서 어떤 현상이 발생하는지 분석하는 방향이 권장된다.

예:

- Reward 변경
- Controller parameter 변경
- Direction range 변경
- Goal representation 변경
- Hand posture 변경
- Sensor configuration 변경
- Object geometry 변경

기록할 것:

- 성공률
- 실패 패턴
- Contact 유지
- Force peak
- Object rotation
- Translation error
- Manipulation time
- 학습된 행동 전략

**실패한 실험도 버리지 말고 왜 그런 행동이 발생했는지 남긴다.**


---

# 16. Goal-conditioned Track에서 바로 가능한 실험

현재 Sensor가 없어도 Simulation에서 바로 시작할 수 있다.

우선순위:

### 1. 기존 Target Position 범위 확장

기존 Left/Right 중심 Target에서 Depth variation을 추가한다.

예:

- 0°
- 15°
- 30°
- 45°

에서 Policy가 성공하는지 확인.


### 2. Direction Curriculum

작은 Direction range부터 시작해 단계적으로 확대한다.

예:

0–15° → 0–30° → 0–45° → Wider Range


### 3. Distance Conditioning

Direction만 성공하면 Target distance variation 추가.


### 4. Rotation Feasibility

길쭉한 Box 등에 대해

- 한쪽을 Push하여 Yaw Rotation
- Corner Contact
- Rotation + Small Translation

등이 학습 가능한지 확인.


### 5. Reward Study

Goal achievement와 Contact behavior 사이에서 Reward 구성에 따라 어떤 전략이 나오는지 분석한다.


---

# 17. 기존 Sweeping vs Handling 논의의 현재 위치

기존의

- Rotation-Constrained Sweeping
- Rotation-centered Handling

구분은 폐기된 것은 아니다.

다만 현재는 이것을 **연구 전체의 최상위 두 주제**로 바로 고정하지 않는다.

오히려 Track B의 Goal-conditioned manipulation을 연구하는 과정에서

- Translation Goal
- Rotation-constrained Translation
- Rotation Goal
- Translation + Rotation

등의 **Task / Manipulation Mode**로 재해석할 수 있다.

즉, 기존 Sweeping/Handling 논의에서 도출한

- Rotation 억제의 필요성
- Rotation을 이용한 공간 확보
- Geometry dependency
- 두 조작의 공간적 유용성

은 이후 Track B를 구체화할 때 다시 사용할 수 있다.


---

# 18. 현재 확정되지 않은 사항

다음은 아직 Decision이 필요하다.

## 연구 구성

- Track A와 Track B를 각각 누가 담당할지
- 최종적으로 논문을 어느 수준까지 통합할지
- Track B의 Vision 조건
- Track B에서 Rotation을 어디까지 포함할지


## Observation

- Geometry를 명시적으로 줄지
- Shape Type을 사용할지
- Dimension을 사용할지
- Geometry Noise를 어떻게 모델링할지
- F/T History 길이
- Tactile representation


## Task

- Goal representation
- Direction 범위
- Distance 범위
- Rotation goal 표현
- Object 종류
- Object mass/friction distribution
- Hand initial configuration


## Evaluation

- Contact quality
- Speed
- Force
- Rotation / Translation accuracy
- Stability
- Generalization
- Sim-to-Real metric


---

# 19. 교수님 최신 지시사항 — 요점만

1. **Vision-Free F/T·Tactile 연구와 Goal-conditioned manipulation 연구를 일단 분리해서 병렬로 검토할 것.**
2. 두 연구를 처음부터 하나로 억지로 합치지 말고, 각각 Insight를 확보한 뒤 향후 통합할 것.
3. Goal-conditioned 쪽은 Sensor를 기다리지 말고 Simulation에서 지금부터 다양한 Direction / Distance / Rotation을 실험할 것.
4. F/T·Tactile 쪽은 단순 성공 여부가 아니라 **Contact sensing으로 성능을 얼마나 향상시킬 수 있는가**를 연구할 것.
5. Reward, Parameter, Goal range 등을 여러 형태로 바꾸어 보고 결과와 실패 패턴을 모두 기록할 것.
6. 기존에 진행하던 Low-level 기반 작업은 버리지 말고 병렬로 계속 진행할 것.
7. RL 시작 Pose는 넓은 Workspace 랜덤화가 아니라 **실제 Planner가 물체 근처에 접근했을 때의 Pose uncertainty**를 반영하는 방향으로 수정할 것.
8. Homing 없이 여러 물체를 연속적으로 Manipulation할 수 있는지 Feasibility를 확인할 것.


---

# 20. Immediate TODO

## P0 — 바로 확인

- [ ] 기존 Policy의 Depth-direction Target variation 확대
- [ ] 45° 등 Diagonal manipulation 가능 여부 확인
- [ ] Homing 없는 Sequential Sweeping Test
- [ ] 현재 Initial Pose Randomization 구조 재검토


## P1 — Initial Pose Framework

- [ ] Conventional Planner의 실제 Target approach 실험 설계
- [ ] EEF–Object Relative Pose Error 측정
- [ ] Position / Orientation Distribution 추정
- [ ] Ellipsoid 또는 이에 준하는 Randomization 정의
- [ ] 해당 범위에서 Policy 재학습


## P1 — Track A

- [ ] F/T Observation 정의
- [ ] Tactile 사용 가능성 재평가
- [ ] Contact sensing으로 개선하고 싶은 Metric 정의
- [ ] F/T-only / Tactile-only / Combined Ablation 구상
- [ ] Sensor history 구조 검토


## P1 — Track B

- [ ] Goal representation 정의
- [ ] Direction-conditioned Feasibility
- [ ] Distance-conditioned 확장
- [ ] Direction Curriculum
- [ ] Rotation Goal Feasibility
- [ ] Reward Study


## P2 — Open Issues

- [ ] Geometry 정보가 실제로 필요한지 검증
- [ ] 필요하다면 최소 Geometry representation 정의
- [ ] 실제 Geometry estimation error 조사
- [ ] Sim Geometry Noise 설계
- [ ] Hand tactile placement 검토


---

# 21. 이 연구를 이어갈 때 주의할 점

### 1. “Sweeping vs Handling”을 현재 최종 연구 구조라고 가정하지 말 것

최신 방향은

- Vision-Free Contact Adaptation
- Goal-conditioned Manipulability

의 분리이다.


### 2. Track B까지 무조건 Vision-Free라고 가정하지 말 것

Track A의 핵심은 Vision-Free이며, Track B는 현재 Manipulation capability 자체를 먼저 연구하는 방향이다.


### 3. Random Initial Pose와 현실적인 Approach Error를 혼동하지 말 것

Workspace 전체의 랜덤 시작과 실제 Planner 도착 오차를 반영한 Randomization은 다른 문제이다.


### 4. Reaching을 RL에 다시 크게 넣지 말 것

Main manipulation policy는 Target 근처에서 시작하는 구조를 기본으로 한다.


### 5. 기존 구현을 폐기하지 말 것

Cartesian / OSC / Relative Observation / Initial Pose Randomization 작업은 이후 두 Track을 위한 기반으로 계속 활용한다.


### 6. Geometry 입력은 확정 사항이 아니다

기존 논의에서는 필요성이 제기되었지만, 최신 구조에서는 실제 필요 수준부터 다시 검증해야 한다.


---

# 22. 현재 연구 방향을 가장 짧게 표현하면

> **선반 환경의 Low-level contact manipulation을 대상으로, 조작 중 Vision tracking 없이 Force/Tactile feedback으로 접촉 변화에 적응하는 문제와, 상위 Planner가 주는 다양한 Goal에 따라 방향·거리·회전을 수행하는 Goal-conditioned manipulation 문제를 분리하여 연구한다. 기존 Cartesian/OSC sweeping framework를 공통 기반으로 사용하며, 실제 Planner의 접근 오차를 반영한 Initial Pose distribution과 연속 Skill 실행 가능성을 먼저 검증한다. 두 연구에서 충분한 Insight를 얻은 뒤 향후 Vision-Free Goal-conditioned Contact Manipulation으로 통합하는 것을 장기 방향으로 둔다.**
```
