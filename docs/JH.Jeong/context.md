# JH.Jeong Research Context

> **주제:** Shelf retrieval을 위한 blocker-object contact manipulation
>
> **문서 성격:** 현재 합의, 작업 가설, 미결정 사항, 그리고 의사결정 과정을 함께 보존하는 living hand-off document
>
> **최종 갱신:** 2026-09-13

---

## 0. 이 문서를 읽는 방법

이 문서는 다른 컴퓨터의 Codex, Claude 또는 사람이 기존 대화 기록 없이도 연구를 이어갈 수 있도록 작성한다. 단순한 최종 요약문이 아니라, **현재 무엇이 합의되었는지와 왜 이전 안이 수정되었는지를 함께 기록하는 문서**다.

이 문서에서는 다음 상태 표기를 사용한다.

| 표기 | 의미 |
| --- | --- |
| **[현재 합의]** | 현재 대화에서 명시적으로 확인된 내용. 후속 작업의 기본 전제로 사용한다. |
| **[작업 가설]** | 현재 유력하지만 실험이나 추가 논의를 통해 바뀔 수 있는 설명·설계·기여 후보다. |
| **[미결]** | 아직 결정하지 않은 사항이다. 임의로 확정해서 구현하거나 논문 주장으로 쓰지 않는다. |
| **[과거 제안]** | 결론에 도달하는 과정에서 검토한 안이다. 현재안과 다를 수 있지만 사고의 흐름을 보존하기 위해 남긴다. |
| **[대체됨]** | 이후 논의에서 명시적으로 수정된 해석이다. 현재 전제로 사용하지 않는다. |

### 후속 작업자가 우선 읽을 부분

1. `1. 현재 결론 요약`
2. `3. 전체 시스템에서 두 Track의 위치`
3. `4. Track A`
4. `5. Track B`
5. `6. Track A와 Track B의 관계`
6. `11. 의사결정 과정`
7. `14. 후속 Agent 작업 지침`

최신 내용과 과거 기록이 충돌할 경우 다음 순서로 판단한다.

1. 이 문서의 **현재 결론**과 **가장 최근 날짜의 결정 기록**
2. 사용자가 후속 대화에서 직접 수정한 내용
3. 이 문서의 과거 제안
4. [`docs/README.md`](../README.md)의 이전 hand-off 내용

`docs/README.md`는 프로젝트 전체의 중요한 배경 자료지만, Track B의 Vision 및 Geometry 조건 등 일부 내용은 이 문서에 기록된 최신 설명으로 대체되었다.

---

## 1. 현재 결론 요약

### 1.1 공통 연구 맥락

**[현재 합의]** 궁극적인 응용 시나리오는 선반 환경에서 **부분적으로만 보이거나 가려진 target object를 드러내거나 꺼내기 위해 blocker object를 조작하는 것**이다.

Track A와 Track B는 모두 이 blocker-handling 시나리오를 다룬다. 두 Track은 서로 다른 동작을 하나씩 담당하는 구분이 아니다. 예를 들어 `Track A = Sweeping`, `Track B = Rotation/Handling`으로 정의하지 않는다.

**[현재 합의]** 두 Track의 관계는 다음과 같다.

> 유사한 blocker-object manipulation 시나리오에서, 서로 다른 정보 조건과 서로 다른 연구 질문을 다룬다.

### 1.2 현재의 핵심 구분

- **Track A:** 조작 시작 전에 Vision으로 파악한 초기 blocker pose를 이용하되, 조작 중에는 지속적인 시각 추적에 의존하지 않고 F/T·Tactile feedback으로 접촉을 조절하는 RL 문제
- **Track B:** 조작 중 Vision을 지속적으로 사용해 blocker의 현재 pose와 geometry를 알고, Vision·F/T·Tactile을 함께 이용해 회전과 다양한 방향의 이동을 수행하는 RL 문제

이를 가장 짧게 표현하면 다음과 같다.

> **Track A는 제한된 시각 관측 아래의 contact adaptation을, Track B는 지속적인 시각·기하 관측 아래의 goal-conditioned multidirectional manipulation을 연구한다.**

### 1.3 아직 연구 제목이나 Contribution을 확정한 것은 아님

**[현재 합의]** 위의 문제 정의는 연구 주제를 설명하기 위한 현재의 framing이다. Vision을 어떻게 사용할지, 어떤 한계와 장점을 주장할지, 두 Track의 최종 Contribution을 어떻게 표현할지는 앞으로 계속 고민하고 실험으로 구체화해야 한다.

따라서 현재 문장을 곧바로 확정된 논문 claim으로 사용하면 안 된다. 특히 아래 주장은 아직 입증되지 않았다.

- Vision을 사용하면 반드시 느리거나 보수적인 정책이 학습된다는 주장
- F/T·Tactile을 사용하면 반드시 더 빠르거나 더 안전하다는 주장
- 모든 방향과 회전을 하나의 범용 정책이 이미 처리할 수 있다는 주장
- 현재 가정한 pose·geometry 정보가 실제 환경에서도 오차 없이 얻어진다는 주장

---

## 2. 용어와 대상 구분

이전 논의에서 `handling`이 두 의미로 사용되어 혼동이 있었다. 이후 문서와 대화에서는 아래처럼 구분한다.

| 용어 | 이 문서에서의 의미 |
| --- | --- |
| **Target object** | 사용자가 최종적으로 찾거나 꺼내고자 하는 물체. 초기에는 blocker 때문에 부분적으로만 보이거나 가려질 수 있다. |
| **Blocker object** | Target object의 관측 또는 접근을 방해하여, Low-level policy가 이동·회전시킬 물체다. |
| **Blocker handling** | Blocker를 밀기, 이동하기, 회전하기 등을 모두 포함하는 광의의 조작 시나리오다. 두 Track 모두 이에 속한다. |
| **Handling/Pivoting mode** | 과거에 Sweeping과 구분하기 위해 제안한, 회전을 주된 물체 운동으로 사용하는 특정 task mode다. 현재 Track B 자체와 동의어가 아니다. |
| **Sweeping mode** | Blocker의 병진 이동을 주된 목표로 하는 task mode다. 현재 Track A 자체와 동의어가 아니다. |
| **Vision-Free during manipulation** | 전체 시스템이 Vision을 전혀 쓰지 않는다는 뜻이 아니다. 조작 전 초기 인식에는 Vision을 쓰지만, 조작 중 지속적인 object-pose update에 의존하지 않는다는 뜻이다. |
| **Omnidirectional** | Track B의 방향성을 설명하기 위한 잠정 용어다. 평면상의 전 방향인지, 로봇과 선반 제약을 반영한 방향 집합인지, SE(2) pose goal 전체인지 아직 정의되지 않았다. |

`partially observable`도 두 층위로 구분해야 한다.

1. **시스템 수준:** 최종 target object가 blocker에 의해 부분적으로 보이거나 가려져 있다.
2. **Track A 조작 수준:** manipulation 도중 blocker 자체의 visual pose tracking도 로봇·Hand·주변 물체의 가림으로 제한될 수 있다.

두 의미를 섞어 쓰지 않는다.

---

## 3. 전체 시스템에서 두 Track의 위치

### 3.1 개념적 실행 흐름

1. Perception/VLM 등이 선반 장면을 관측한다.
2. 상위 판단기가 target object를 찾거나 꺼내기 위해 이동해야 할 blocker와 조작 목표를 정한다.
3. VLA 또는 conventional motion planner가 로봇/Tool을 blocker 조작 시작 위치로 이동시킨다.
4. Track A 또는 Track B의 Low-level RL manipulation policy가 blocker를 조작한다.
5. 장면을 다시 관측하고 target object의 노출 또는 접근 가능성을 판단한다.
6. 필요하면 다른 blocker 조작 또는 target retrieval 단계를 수행한다.

### 3.2 현재 Low-level 연구의 담당 범위

**[현재 합의]** 핵심 연구 대상은 4번의 blocker manipulation policy다. 상위 VLM planning, 전체 탐색 순서 결정, 장거리 reaching, target retrieval 전체를 하나의 RL policy로 해결하려는 연구가 아니다.

다만 Low-level policy와 외부 시스템 사이의 인터페이스는 명확해야 한다.

- 누가 blocker와 goal을 선택하는가?
- 어느 위치까지 planner가 접근시키는가?
- Low-level policy가 contact formation을 어디까지 담당하는가?
- Policy 완료 후 누가 재관측·다음 Skill 호출·안전 정지를 담당하는가?

이 수치적 경계는 아직 모두 확정하지 않았지만, 연구 문제를 설명할 때 상위 시스템과 Low-level policy의 역할을 혼동해서는 안 된다.

### 3.3 이전 문서에서 유지할 범위 원칙

다음은 이전 초안과 [`docs/README.md`](../README.md)에서 도출되어 현재도 유효한 **작업상 원칙**이다. 세부 수치는 미결이다.

- 장거리 reaching은 주로 별도 VLA 또는 conventional planner가 담당한다.
- Manipulation policy는 blocker 조작에 적합한 tool/EEF 위치에서 시작한다.
- 넓은 workspace를 무작위로 시작시키는 것보다, 실제 planner 도착 오차를 반영한 initial pose distribution이 바람직하다.
- 초기 실험은 선반 지지면 위의 단일 blocker object로 단순화할 수 있다.
- 주변 물체와의 multi-object contact 및 clutter interaction은 후속 확장으로 둘 수 있다.
- Ground-truth object state는 simulation reward와 evaluation에 사용할 수 있으나, 각 Track에서 정의한 policy observation 조건을 위반해서는 안 된다.
- 외부 중단, 다음 Skill 전환, 로봇 정지/명령 유지 등은 별도 execution layer의 책임으로 둘 수 있다.
- 이동 Base는 전체 플랫폼의 일부일 수 있지만, 초기 실험에서 고정 운용할 수 있다.

주의: Track A의 시작 상태가 이미 접촉한 상태인지, near-contact/non-contact 상태인지, contact formation을 얼마나 포함하는지는 **아직 미결**이다. 최신 설명에서 확정된 것은 planner가 `물체를 옆으로 밀어내기 위한 tool 위치`까지 접근시킨 후 policy가 시작된다는 수준이다.

---

## 4. Track A — Initial Vision, Contact-Sensing Control

### 4.1 현재 문제 정의

**[현재 합의]** Track A의 현재 시나리오는 다음과 같다.

1. Vision을 통해 blocker object의 초기 pose를 파악한다.
2. 다른 VLA 또는 conventional motion planner를 이용해, blocker를 옆으로 밀기 적합한 tool 위치까지 로봇을 이동시킨다.
3. 이 위치에서 RL manipulation policy를 시작한다.
4. Manipulation 중에는 로봇 팔, Hand, blocker 및 주변 구조로 인해 Vision이 제한될 수 있다.
5. Vision update 주기는 Low-level control 주기보다 느릴 수 있어, 빠르게 변하는 접촉 상태에 대한 즉각적인 feedback source로는 한계가 있을 수 있다.
6. 따라서 조작 중에는 blocker pose를 Vision으로 계속 갱신하지 않고, F/T sensor와 tactile sensor에 의존해 접촉을 형성·유지·조절하며 물체를 밀어낸다.
7. 이 policy를 강화학습으로 학습한다.

현재 문제를 한 문장으로 쓰면 다음과 같다.

> **초기 시각 pose만 주어지고 manipulation 중 신뢰할 수 있는 시각 추적을 사용할 수 없는 조건에서, F/T와 tactile feedback에 기반한 RL policy가 blocker와의 접촉을 조절하며 원하는 pushing을 수행할 수 있는가?**

### 4.2 관측 조건

#### 조작 전

- **[현재 합의]** Vision 기반 초기 blocker pose
- **[현재 합의]** Planner가 생성한 조작 시작 tool/robot 상태

#### 조작 중

- **[현재 합의]** Wrist F/T sensor
- **[현재 합의]** Hand tactile sensor
- **[작업 가설]** Robot proprioception은 기본 관측 후보다.
- **[작업 가설]** F/T·Tactile history 또는 recurrent state가 필요할 수 있다.
- **[현재 합의]** 지속적으로 갱신된 visual blocker pose는 사용하지 않는다.

Track A에서 `Vision-Free`라는 표현을 사용할 때에는 반드시 `during manipulation` 또는 `without continuous visual pose tracking`이라는 범위를 함께 명시한다.

### 4.3 Method 방향 — 아직 구체화 중

현재 Method의 최소 골격은 다음과 같다.

- RL 기반 Low-level contact manipulation policy
- 초기 visual pose와 manipulation 시작 상태를 context로 사용
- 조작 중 F/T·Tactile feedback을 closed-loop observation으로 사용
- 접촉 변화에 따라 pushing action을 조절
- 기존 Cartesian/EEF-space action 및 OSC 기반 환경을 재사용할 가능성

그러나 아래는 아직 Method로 확정되지 않았다.

- Sensor fusion 구조
- History encoder 또는 RNN 사용 여부
- Contact-state estimation을 명시적으로 둘지 여부
- Action space와 Hand joint 제어 범위
- Reward 구성
- Policy termination 방식
- Sim-to-Real 및 실제 sensor calibration 방법
- 물체 geometry를 초기 context로 줄지 여부

후속 Agent는 위 후보를 이미 결정된 architecture로 서술하거나 구현하면 안 된다.

### 4.4 Contribution 초안 — 확정 아님

사용자가 현재 제시한 Contribution 방향은 다음과 같다.

> **Vision이 제한되는 manipulation 상황에서 F/T sensor와 tactile sensor를 활용하여 blocker object를 밀어내는 동작을 학습한다.**

이를 연구 질문 중심으로 조금 더 풀면 다음과 같다.

> 지속적인 visual pose tracking이 불안정하거나 사용할 수 없는 contact-rich pushing에서, 접촉 감각에 기반한 RL policy가 blocker manipulation을 수행하고 manipulation quality를 개선할 수 있는지를 검증한다.

여기서 `manipulation quality`의 정확한 정의와 novelty는 아직 미결이다. 후보는 다음과 같지만, 지금 모두를 Contribution으로 주장하는 것은 아니다.

- Contact 유지와 contact loss 감소
- Excessive force 또는 peak force 억제
- 의도하지 않은 회전·전도·불안정 감소
- 목표 변위 성공률 및 정확도
- 물체 질량·마찰·접촉 조건 변화에 대한 적응
- F/T와 tactile의 상호보완성
- 센서 history가 부분 관측 문제에 미치는 효과

### 4.5 Track A의 논리적 주의점

다음과 같이 주장하지 않는다.

> Vision은 느리므로 Vision을 사용한 policy는 반드시 느리고 보수적이다.

Vision과 Low-level contact sensing은 서로 다른 주기로 병렬 사용할 수도 있으므로 위 인과관계는 일반적으로 성립하지 않는다. 보다 방어 가능한 동기는 다음과 같다.

> 조작 중 self-occlusion 등으로 안정적인 visual pose tracking을 보장하기 어려운 조건에서, 고주기 F/T·Tactile feedback에 기반한 정책이 지속적인 시각 추적 없이도 manipulation을 수행할 수 있는지 검증한다.

더 빠른 반응, 더 높은 안정성, 더 나은 적응성은 **가정이 아니라 비교 실험으로 입증해야 할 결과**다.

### 4.6 Track A에서 나중에 구체화할 사항

아래 질문은 backlog이며, 지금 한 번에 답하거나 확정하기 위한 목록이 아니다.

- Pushing goal이 고정 lateral target인지, 방향·거리 conditioning을 포함하는지
- 완료 여부를 policy 또는 외부 execution layer 중 누가 판단하는지
- Policy 시작 시 contact가 이미 형성되어 있는지
- 잔여 approach와 contact formation의 담당 범위
- 초기 pose 오차를 어떤 분포로 모델링할지
- Track A에서 blocker geometry를 사용할지
- Robot/Hand action space와 hand configuration
- F/T-only, tactile-only, combined 등 필요한 ablation
- Track A의 핵심 성능 지표와 최종 Method novelty
- Simulation 학습, 실제 로봇 학습, Sim-to-Real의 범위

---

## 5. Track B — Continuous Vision, Goal-Conditioned Multidirectional Manipulation

### 5.1 현재 문제 정의

**[현재 합의]** Track B에서는 Vision을 manipulation 전 과정에서 지속적으로 사용한다. 따라서 연구 문제의 abstraction에서는 blocker object의 **현재 pose와 geometry를 지속적으로 알고 있다고 가정**한다.

Track B는 다음 능력을 목표로 한다.

- Blocker를 회전시키기
- 단순 lateral axis에 한정하지 않고 다양한 방향으로 밀거나 이동시키기
- 필요하면 회전과 translation을 결합하기
- Vision, F/T sensor, tactile sensor를 모두 사용하기

이때 Vision이 main information source이고, F/T와 tactile은 접촉 상태와 force interaction을 보완하는 sub information source가 될 수 있다. 정확한 역할 분담과 fusion 방식은 아직 확정하지 않았다.

현재 문제를 한 문장으로 쓰면 다음과 같다.

> **지속적으로 관측되는 blocker의 pose와 geometry, 그리고 contact feedback을 이용해, RL policy가 blocker를 회전시키고 다양한 방향의 manipulation goal을 수행할 수 있는가?**

### 5.2 관측 조건

- **[현재 합의]** 조작 중 지속적인 Vision
- **[현재 합의]** 현재 blocker pose
- **[현재 합의]** Blocker geometry
- **[현재 합의]** F/T sensor
- **[현재 합의]** Tactile sensor
- **[작업 가설]** Robot proprioception

중요한 최신 수정은 다음과 같다.

> Track B는 초기 pose와 geometry만 받은 뒤 open-loop로 조작하는 문제가 아니다. Vision을 계속 사용하여 pose와 geometry를 지속적으로 안다는 조건이다.

`안다`는 현재의 연구 abstraction을 뜻한다. 실제 perception noise, occlusion, update rate, geometry representation을 어느 수준으로 모델링할지는 별도 미결 사항이다.

### 5.3 목표 동작의 범위

Track B는 `물체를 먼저 회전시킨 뒤 다시 lateral 방향으로만 미는 policy`에 머물러서는 안 된다. 회전을 이용하더라도 최종적으로 다양한 방향의 이동을 수행할 수 있어야 한다.

현재 고려 가능한 manipulation mode는 다음과 같다.

- Lateral translation
- Depth-direction translation
- Diagonal translation
- Yaw rotation
- Rotation followed by translation
- Translation and rotation이 결합된 평면 조작

다만 이 목록 전체를 하나의 policy가 즉시 모두 수행해야 한다는 결론은 아직 없다. `Omnidirectional`의 정확한 범위와 단계적 curriculum은 추후 결정한다.

### 5.4 Method 방향 — Track A보다 덜 구체화됨

현재 Method의 상위 방향은 다음과 같다.

- Goal-conditioned RL manipulation
- 지속적인 visual pose/geometry feedback 사용
- Vision을 주된 상태 정보로 활용
- F/T·Tactile을 contact interaction 보조 정보로 활용
- 다양한 translation direction 및 rotation goal을 다루는 방향으로 확장

그러나 무엇이 핵심 Method novelty가 될지는 아직 결정하지 않았다. 다음은 후보일 뿐이다.

- Goal representation
- Pose/geometry-conditioned policy
- Vision–F/T–Tactile multimodal fusion
- Rotation과 translation을 연결하는 strategy learning
- Goal distribution 또는 curriculum 설계
- 물체 geometry에 따른 contact strategy 변화

### 5.5 Contribution 방향 — 추가 구체화 필요

현재 가능한 Contribution 서술의 골격은 다음과 같다.

> **지속적인 pose·geometry 관측과 multimodal contact feedback을 이용하여, blocker를 회전시키고 여러 방향으로 이동시키는 goal-conditioned manipulation policy를 학습한다.**

하지만 이 문장은 아직 연구 결과가 아니라 방향 설명이다. 다음 중 무엇을 중심 기여로 삼을지는 추가 논의와 feasibility 결과가 필요하다.

- 하나의 policy가 다양한 방향·거리·회전 goal에 대응하는 능력
- Geometry에 따라 서로 다른 접촉 또는 회전 전략을 선택하는 능력
- Vision을 주 정보로 하고 F/T·Tactile을 보조 정보로 사용하는 multimodal policy
- 제한된 선반 공간에서 단순 lateral sweeping보다 다양한 blocker 재배치를 가능하게 하는 능력

### 5.6 Track B에서 나중에 구체화할 사항

아래 질문 역시 backlog이며 즉시 모두 결정할 필요는 없다.

- Goal을 direction/distance로 표현할지, target pose로 표현할지
- `Omnidirectional`을 평면 전 방향으로 볼지, 제한된 feasible direction set으로 볼지
- Rotation goal과 translation goal을 별도 mode로 둘지 통합할지
- 회전 후 translation 순서를 지정할지, policy가 전략을 선택하게 할지
- RGB/RGB-D, estimated pose, geometric feature 중 어떤 Vision representation을 사용할지
- Geometry를 dimension, shape class, bounding box, point cloud 등 어떤 형태로 제공할지
- Pose·geometry estimation error를 어느 단계부터 반영할지
- F/T·Tactile이 구체적으로 어떤 실패를 보완해야 하는지
- Sensor modality ablation을 어떻게 설계할지
- Track B의 최종 Method novelty와 Contribution

---

## 6. Track A와 Track B의 관계

### 6.1 비교표

| 구분 | Track A | Track B |
| --- | --- | --- |
| 응용 시나리오 | 가려진 target을 위해 blocker 조작 | 가려진 target을 위해 blocker 조작 |
| Track 구분 기준 | 제한된 시각 조건에서 contact sensing으로 적응 | 알려진 visual/geometric state에서 다양한 goal 수행 |
| 초기 Vision | 사용 | 사용 |
| 조작 중 Vision | 지속적 blocker-pose tracking에 의존하지 않음 | 지속적으로 사용 |
| 조작 중 pose | 직접적인 visual update 없음 | 지속적으로 알고 있다고 가정 |
| Geometry | 사용 여부 미결 | 지속적으로 알고 있다고 가정 |
| F/T·Tactile | 주된 closed-loop feedback | 사용하며, Vision에 대한 보조 정보가 될 수 있음 |
| 현재 중심 문제 | Partial observability와 contact adaptation | Goal-conditioned multidirectional/rotational manipulation |
| 동작 종류 | 특정 동작 하나로 Track을 정의하지 않음 | 특정 동작 하나로 Track을 정의하지 않음 |
| 구체화 수준 | 문제와 초기 contribution 방향이 비교적 명확 | 문제·Method·Contribution 추가 구체화 필요 |

### 6.2 반드시 피해야 할 오해

- `Track A = Sweeping`, `Track B = Handling/Pivoting`으로 정의하지 않는다.
- Track B를 Vision-Free policy로 설명하지 않는다.
- Track B가 초기 pose와 geometry만 사용한다고 설명하지 않는다.
- Track B에서 F/T·Tactile이 불필요하다고 확정하지 않는다.
- Track A를 전체 perception/planning 단계까지 Vision-Free인 시스템으로 설명하지 않는다.
- `Handling`에 translation이 포함되므로 Sweeping의 완전한 상위 호환이라고 단정하지 않는다.
- 두 개의 task가 존재한다는 사실만으로 두 개의 독립적인 Method contribution이 생긴다고 가정하지 않는다.

### 6.3 장기 통합에 대한 현재 상태

[`docs/README.md`](../README.md)에서는 두 Track에서 insight를 확보한 뒤 `Vision-Free Goal-conditioned Contact Manipulation`으로 통합하는 가능성을 제시했다.

그러나 최신 설명에서 Track B는 continuous Vision을 명확한 조건으로 갖는다. 따라서 완전한 Vision-Free 통합은 **현재 확정된 최종 endpoint가 아니라 과거에 제안된 장기 가능성**으로 남긴다. 향후 두 Track을 어떤 수준에서 통합할지는 다시 논의해야 한다.

---

## 7. Sweeping과 Handling/Pivoting 논의의 현재 위치

### 7.1 과거에 검토한 두 Skill 정의

**[과거 제안]** 연구를 다음 두 Skill로 나누는 안을 검토했다.

| 과거 Skill | 당시 정의 |
| --- | --- |
| Rotation-Constrained Sweeping | Blocker를 목표 방향으로 병진시키면서 의도하지 않은 yaw rotation과 횡방향 이탈을 억제 |
| Rotation-Centered Handling/Pivoting | Blocker의 yaw rotation을 주된 운동으로 사용하고 소량의 translation을 허용 |

이 구분에는 실제 공간적 동기가 있었다.

- 이동 통로는 있지만 회전 여유가 작다면 방향을 유지하는 sweeping이 유리할 수 있다.
- 짧은 병진만으로 target 주변의 틈을 만들기 어렵다면, 길쭉하거나 비대칭인 blocker를 회전시켜 국소적인 관측·접근 공간을 확보할 수 있다.
- 엄격한 고정 pivot이나 pure rotation을 강제하면 실제 공간 확보 목적과 맞지 않을 수 있으므로 small translation을 허용하는 안이 검토되었다.

### 7.2 이 구분에서 발생한 문제

1. 두 Skill이 동일한 sensor와 policy 구조를 쓴다면 task가 둘이어도 Method contribution은 하나일 수 있다.
2. Handling이 translation을 허용할 때 Sweeping과 역할이 중복되어 보일 수 있다.
3. 중복을 피하려고 pure rotation을 강제하면 실제 task의 유용성이 약해질 수 있다.
4. 한 Skill만 Vision을 쓰거나 두 Skill 모두 Vision-Free로 만드는 논리가 전체 연구 질문보다 sensor 조건에 끌려갈 수 있다.
5. 회전 및 공간 점유를 판단하려면 geometry가 필요하지만, clutter/occlusion 환경에서는 geometry 추정이 불완전할 수 있다.

### 7.3 현재의 해석

**[현재 합의]** Sweeping과 Handling은 연구 전체를 나누는 최상위 기준이 아니다. 두 Track에서 사용할 수 있는 task 또는 manipulation mode로 남긴다.

- Track A에서 lateral pushing은 접촉 감각 기반 제어를 연구하기 위한 현재의 구체적 시나리오가 될 수 있다.
- Track B에서는 translation, rotation, rotation-constrained translation, translation+rotation 등이 goal-conditioned capability를 검증하는 task mode가 될 수 있다.

두 mode의 우열을 일반적으로 주장하지 않는다. 필요하면 동일한 target visibility 또는 접근 공간 확보 조건에서 공간적 유용성과 실행 비용을 별도로 비교한다.

---

## 8. Vision 관련 Storytelling 원칙

Vision의 활용 여부는 두 Track을 설명하는 중요한 축이지만, 연구 결론을 미리 정당화하기 위한 절대 명제로 사용하지 않는다.

### 8.1 현재 사용할 수 있는 사실·동기

- Manipulation 중 로봇 팔과 Hand가 blocker를 가려 visual tracking이 불안정해질 수 있다.
- Vision processing/update 주기와 Low-level control 주기는 다를 수 있다.
- F/T와 tactile은 접촉 변화에 대한 직접적이고 고주기인 feedback source가 될 수 있다.
- Track A는 이러한 조건에서 continuous visual pose tracking 없이 manipulation 가능한지를 묻는다.
- Track B는 continuous Vision을 적극적으로 활용하고 contact sensor를 함께 사용한다.

### 8.2 아직 실험으로 보여야 하는 결과

- Contact sensing이 vision-only 조건보다 빠르게 반응하는지
- 더 빠른 실행이 가능한지
- Contact 유지와 안정성이 좋아지는지
- Vision과 contact sensing의 결합이 어느 실패 유형을 줄이는지
- Sensor modality별 정보가 실제 policy behavior에 어떤 차이를 만드는지

### 8.3 논문 서술 시 피할 인과관계

`Vision 사용 → 낮은 판단 주기 → 보수적 policy`를 필연적 결과로 쓰지 않는다. 비동기식 vision과 고주기 tactile controller를 결합할 수 있기 때문이다.

대신 Track A에서는 `지속적인 visual tracking을 보장하기 어려운 조건에서도 contact feedback으로 조작 가능한가`를 중심 질문으로 두고, 속도·안정성·적응성은 평가 결과로 다룬다.

Track B에서는 Vision을 약점으로 취급하지 않는다. Pose와 geometry를 계속 제공하는 주된 정보원으로 활용하고, contact sensor가 이를 어떻게 보완하는지를 연구할 수 있다.

---

## 9. Geometry 논의

### 9.1 과거 문제의식

Sweeping 중 회전을 억제하거나 Handling에서 적절한 접촉 위치를 선택하려면 orientation, width, depth, shape 등 geometry 정보가 중요할 수 있다는 의견이 있었다.

한편 실제 선반의 부분 관측에서는 다음 오차가 발생할 수 있다.

- Dimension error
- Systematic bias
- Shape misclassification
- Partial observation
- Self-occlusion

이 때문에 simulation ground truth geometry에 단순 random noise를 넣는 것만으로 실제 perception error를 충분히 재현할 수 있는지 의문이 제기되었다. 또한 geometry를 명시적으로 주는 것이 zero-shot/generalization 목표와 어떤 관계인지도 논의되었다.

### 9.2 최신 Track별 상태

- **Track A:** 초기 blocker pose는 Vision으로 안다. Geometry를 policy에 제공할지는 최신 논의에서 확정하지 않았다.
- **Track B:** blocker의 current pose와 geometry를 continuous Vision을 통해 지속적으로 안다고 가정한다.

따라서 `Geometry가 두 Track 모두에서 미지다` 또는 `Geometry 입력은 두 Track 모두 사용하지 않는다`는 이전 표현은 현재 유효하지 않다.

### 9.3 여전히 남아 있는 미결 사항

Track B에서 geometry의 **availability assumption**은 정해졌지만 아래 사항은 미결이다.

- Shape class와 dimensions로 줄지
- Bounding box, point cloud, mesh 또는 learned feature를 사용할지
- 정확한 ground truth에 가까운 값으로 시작할지
- Perception error와 occlusion을 어느 단계에서 반영할지
- Geometry conditioning이 실제 성능과 strategy에 어떤 영향을 주는지

---

## 10. 실험과 평가를 구체화할 때의 기준

이 절은 확정된 실험 계획이 아니라, 문제 정의–Method–Contribution을 발전시킬 때 잊지 말아야 할 평가 관점이다.

### 10.1 Track A 후보 평가축

- Goal displacement success 및 translation error
- Contact 유지율과 contact loss 횟수
- Peak/mean force
- 의도하지 않은 yaw rotation
- 전도, 낙하, 들림 등 instability
- Manipulation time
- 질량·마찰·초기 pose 오차에 대한 robustness
- Proprioception-only / F/T-only / tactile-only / combined sensor ablation
- History length 또는 memory 구조에 따른 차이

### 10.2 Track B 후보 평가축

- Direction 및 distance goal success
- Position/orientation error
- 학습 가능한 direction range
- Translation에서 rotation 또는 combined SE(2) goal로의 확장성
- Object geometry 변화에 대한 generalization
- Vision-only와 Vision+contact sensing 비교
- Goal과 geometry에 따라 나타나는 contact/rotation strategy
- 제한된 선반 공간에서 target visibility 또는 접근 가능 영역의 변화

### 10.3 Skill metric과 시스템 metric의 분리

Low-level object-goal 달성과 최종 target retrieval 효과는 다른 수준의 지표다.

- **Skill-level:** blocker translation/rotation accuracy, force, contact, stability, time
- **System-level:** target object 노출량, 접근 가능한 틈, retrieval 가능 여부, 필요한 조작 횟수

초기 단일-object feasibility에서 system-level 성능까지 모두 입증했다고 주장하지 않는다. 반대로 후속 clutter 실험에서는 목표 각도나 거리 성공률만으로 blocker handling의 공간적 유용성을 대신하지 않는다.

---

## 11. 의사결정 과정

이 절은 과거 내용을 지우지 않고 연구 방향이 어떻게 바뀌었는지를 보존한다.

### Stage 0 — 기존 Vision-based Sweeping에서 출발

연구는 기존 Vision-based Sweeping 환경에서 시작했다. 이후 Cartesian/EEF-space action, OSC, relative observation, initial pose randomization 등 Low-level 기반을 발전시켰다.

여기서 다음 문제의식이 생겼다.

- 조작 중 robot/hand에 의한 blocker occlusion
- Visual update와 contact-control 주기 차이 가능성
- 지속적인 pose tracking이 끊겼을 때의 policy robustness
- 좌우 sweeping에 가까운 기존 goal distribution의 제한

이 단계에서 Vision-Free contact manipulation과 다양한 방향의 goal-conditioned manipulation이라는 두 방향의 씨앗이 생겼다.

### Stage 1 — Sweeping과 Handling/Pivoting의 두 Skill 구분

**[과거 제안]** 처음에는 연구를 운동 특성으로 나누려 했다.

- Sweeping: translation을 수행하면서 rotation 억제
- Handling/Pivoting: rotation을 주된 운동으로 사용하면서 small translation 허용

두 Skill 모두 초기 pose와 접촉 감각을 이용하고, manipulation 중 Vision을 사용하지 않는 안도 검토했다. 이 안은 각 Skill이 유리한 공간 조건을 설명하는 데에는 도움이 되었지만, 다음 문제가 남았다.

- 두 Skill의 Method contribution 중복
- Handling이 Sweeping을 포함하는 것처럼 보이는 역할 중복
- 두 Skill의 Vision 조건을 억지로 통일할 위험
- Pure translation/pure rotation 제약의 실제 유용성
- Geometry 입력과 부분 관측 전제의 정합성

### Stage 2 — 연구축을 Sensing과 Goal Conditioning으로 Decouple

최신 미팅 이후 최상위 구분을 운동 종류가 아닌 연구 질문으로 바꾸었다.

- Track A: Sensing / Contact adaptation
- Track B: Goal-conditioned manipulability

Sweeping과 Handling은 폐기하지 않고 Track 내부의 task/mode 후보로 내렸다. 센서·Sim-to-Real 난점이 큰 Track A와 simulation에서 goal range를 빠르게 확장할 수 있는 Track B를 처음부터 하나의 policy로 묶지 않고, 별도로 insight를 얻자는 방향이었다.

이 단계의 [`docs/README.md`](../README.md)에서는 Track B의 Vision과 sensor 사용 조건을 열어 두었고, geometry 필요성도 open issue로 남겼다.

### Stage 3 — Track A와 Track B의 현재 관측 조건 구체화

**[2026-09-12 현재 합의]** 후속 설명을 통해 다음이 명확해졌다.

1. 두 Track 모두 partially observable target을 위해 blocker를 handling하는 유사한 시나리오를 다룬다.
2. 두 Track은 서로 다른 동작을 하나씩 맡는 것이 아니라 서로 다른 연구 문제를 다룬다.
3. Track A는 initial Vision으로 blocker pose를 얻고 planner가 tool pose로 접근한 후, manipulation 중 F/T·Tactile에 의존한다.
4. Track B는 Vision을 지속적으로 사용하므로 current blocker pose와 geometry를 계속 안다고 가정한다.
5. Track B도 F/T·Tactile을 사용하며, Vision이 main이고 contact sensing이 sub가 될 수 있다.
6. Track B는 rotation 후 lateral pushing으로 한정되지 않고 다양한 방향의 manipulation을 지향한다.
7. Vision 조건을 포함한 현재 문제 정의는 연구 주제를 설명하기 위한 framing이며, 최종 Storytelling과 Contribution은 계속 구체화해야 한다.

### Stage 4 — 세부 명세를 한 번에 확정하지 않기로 한 확인

초기 hand-off 과정에서 goal encoding, policy 종료, action space, sensor ablation 등 많은 구체화 질문이 한꺼번에 제기되었다. 사용자는 현재 목적이 모든 세부 사항을 즉시 확정하는 것이 아니라, **두 Track의 의도와 최신 방향을 먼저 정확히 공유하는 것**임을 명확히 했다.

따라서 후속 Agent는 전체 연구 명세를 한 번에 완성하려 하지 말고, 현재 진행 중인 논의나 구현에 필요한 질문만 단계적으로 다룬다.

---

## 12. 이전 자료와 최신 결론의 충돌 정리

| 이전 기록 | 최신 상태 | 처리 |
| --- | --- | --- |
| Sweeping과 Handling을 두 개의 최상위 연구 주제로 둠 | 두 Track은 Contact Adaptation과 Goal-conditioned Manipulation이라는 연구 문제로 구분 | **[대체됨]** 과거 사고 과정으로만 보존 |
| 두 Skill 모두 manipulation 중 Vision-Free로 구성 | Track A만 continuous visual pose tracking을 쓰지 않으며, Track B는 continuous Vision 사용 | **[대체됨]** |
| Track B의 Vision 조건은 아직 열려 있음 | Track B는 Vision을 지속적으로 사용 | **[대체됨]** |
| Track B에서 F/T·Tactile은 필수 아님 | Track B는 Vision, F/T, Tactile을 모두 사용 | **[대체됨]** |
| Track B는 초기 pose만 받거나 geometry 필요성이 open | Track B는 current pose와 geometry를 지속적으로 안다고 가정 | Availability 조건은 **[대체됨]**, representation/noise는 **[미결]** |
| Geometry는 두 Skill 모두 명시적으로 주지 않음 | Track B에서는 geometry를 안다고 가정; Track A는 미결 | **[대체됨]** |
| 장기적으로 반드시 Vision-Free goal-conditioned policy로 통합 | 이전 가능성일 뿐 최신 endpoint로 재확인되지 않음 | **[과거 제안]** |
| Vision-Free이면 더 빠르고 적응적임 | 실험으로 검증할 가설 | **[작업 가설]** |
| 넓은 workspace에서 EEF를 uniform randomize | 실제 planner 접근 오차 분포를 반영하는 방향 | 이전 randomization 해석은 **[대체됨]** |

---

## 13. 현재 결정 레지스터와 미결 Backlog

### 13.1 현재 결정 레지스터

| ID | 상태 | 결정 |
| --- | --- | --- |
| D-001 | 현재 합의 | 공통 응용은 partially observable target을 위해 blocker object를 조작하는 선반 시나리오다. |
| D-002 | 현재 합의 | Track A/B는 서로 다른 동작이 아니라 유사한 시나리오의 서로 다른 연구 문제다. |
| D-003 | 현재 합의 | Track A는 initial blocker pose에 Vision을 사용하고 manipulation 중 지속적인 visual pose update에 의존하지 않는다. |
| D-004 | 현재 합의 | Track A의 manipulation feedback은 F/T와 tactile이 중심이며 RL policy를 학습한다. |
| D-005 | 현재 합의 | Track B는 continuous Vision을 사용하며 current blocker pose와 geometry를 지속적으로 안다고 가정한다. |
| D-006 | 현재 합의 | Track B는 Vision, F/T, tactile을 모두 사용하며 Vision이 main source가 될 수 있다. |
| D-007 | 현재 합의 | Track B는 rotation과 다양한 방향의 translation을 지향하며 lateral pushing에만 한정하지 않는다. |
| D-008 | 현재 합의 | Vision 조건과 Contribution 서술은 계속 구체화할 연구 framing이며 절대 전제로 고정하지 않는다. |
| D-009 | 현재 합의 | 모든 세부 설계를 지금 한 번에 확정하지 않고 필요한 순서대로 구체화한다. |

### 13.2 미결 Backlog

이 목록은 향후 논의의 기억 장치이지, 다음 대화에서 전부 답해야 하는 질문지가 아니다.

#### 공통

- 각 Track이 별도 논문/졸업연구인지, 하나의 연구 안의 병렬 Track인지
- 상위 Planner가 제공할 정확한 manipulation goal과 interface
- 초기 단일 blocker에서 clutter/multi-object 환경으로 확장하는 시점
- Target visibility/retrievability를 Low-level evaluation에 포함할 수준
- 실제 planner approach-error distribution의 측정 및 반영 방식
- 각 Track의 최종 학습·실물·Sim-to-Real 범위

#### Track A

- 최종 problem statement와 핵심 novelty
- Lateral pushing goal의 구체적 형태
- 시작 시 contact 여부와 contact formation 범위
- Geometry 및 proprioceptive history의 사용 여부
- 종료 판단과 success definition
- F/T·Tactile representation 및 fusion 방식
- 핵심 sensor ablation과 평가 metric

#### Track B

- `Omnidirectional`의 정확한 정의
- Goal representation과 curriculum
- Rotation/translation을 하나의 policy에 통합할 범위
- Continuous Vision과 geometry의 representation
- Pose/geometry noise 및 occlusion을 모델링할 범위
- F/T·Tactile이 Vision을 보완하는 구체적 역할
- 최종 Method novelty와 Contribution

---

## 14. 후속 Agent 작업 지침

다른 컴퓨터의 Agent는 다음 원칙에 따라 작업한다.

### 14.1 연구 내용을 해석할 때

1. 이 문서의 현재 결론을 먼저 사용한다.
2. Sweeping/Handling을 Track A/B 정의로 되돌리지 않는다.
3. Track A의 `Vision-Free` 범위를 manipulation 단계로 한정한다.
4. Track B의 continuous Vision, current pose, geometry availability를 누락하지 않는다.
5. Track B에서 F/T·Tactile을 optional이라고 임의로 바꾸지 않는다.
6. 미결인 architecture, goal, metric을 확정된 Method처럼 꾸미지 않는다.
7. 연구 동기와 실험으로 입증된 결과를 구분한다.
8. 세부 질문을 한 번에 모두 요구하지 말고, 현재 작업에 필요한 것부터 단계적으로 확인한다.

### 14.2 문서를 갱신할 때

중요한 판단이 바뀌면 과거 문장을 단순 삭제하지 않는다.

1. `1. 현재 결론 요약`과 해당 Track 절을 최신 상태로 갱신한다.
2. `11. 의사결정 과정`에 새 Stage 또는 날짜가 있는 항목을 추가한다.
3. `12. 이전 자료와 최신 결론의 충돌 정리`에 대체 관계를 기록한다.
4. `13. 현재 결정 레지스터`의 상태를 갱신한다.
5. 문서 마지막 Change Log에 날짜와 변경 이유를 추가한다.

새 결정 기록은 가능하면 아래 형식을 따른다.

```text
날짜:
논의 배경:
이전 해석:
새 결론:
변경 이유:
영향받는 Track/실험/문서:
아직 남은 질문:
```

### 14.3 코드 작업으로 이어갈 때

이 문서는 연구 맥락의 source of truth이지, 현재 코드 구현 상태를 보장하는 명세서가 아니다. 코드 변경 전에는 반드시 실제 repository 구조, branch 상태, config, observation/action space와 기존 실험 결과를 별도로 확인한다.

[`docs/README.md`](../README.md)에 Cartesian/OSC, relative observation, randomization 등에 대한 과거 구현 상태가 기록되어 있지만, 코드와 다시 대조한 뒤 사용한다.

---

## 15. 관련 자료와 Provenance

- [`docs/README.md`](../README.md): 전체 프로젝트의 기존 hand-off. Vision-based Sweeping의 한계, 두 Track으로의 decoupling, initial pose randomization, sequential manipulation, sensor/geometry 문제 등이 자세히 기록되어 있다.
- 과거 대화 초안: `Sweeping vs Handling/Pivoting`, 두 Skill의 Vision 조건, geometry 문제, 역할 중복 및 비교 시나리오를 검토했다. 해당 첨부 원문은 다른 컴퓨터에서 접근할 수 없을 수 있으므로, 후속 작업에 필요한 결론과 사고 과정은 이 문서의 7절·9절·11절·12절에 내재화했다.
- 2026-09-12 사용자 설명: Track A의 initial-Vision/contact-feedback 조건, Track B의 continuous-Vision/current-pose-and-geometry 조건, 그리고 두 Track이 다른 동작이 아닌 다른 연구 문제라는 점을 최신 결론으로 반영했다.

---

## 16. Change Log

### 2026-09-13 — 문서 최초 작성

- 비어 있던 `docs/JH.Jeong/context.md`를 self-contained research hand-off로 작성했다.
- 현재 Track A/B 정의와 공통 blocker-handling 시나리오를 기록했다.
- 과거 Sweeping/Handling 구분에서 현재 연구축으로 바뀐 이유를 보존했다.
- Track B가 continuous Vision으로 current pose와 geometry를 안다는 최신 수정을 반영했다.
- 확정 사항, 작업 가설, 미결 사항, 대체된 해석을 분리했다.
- 후속 Agent가 연구 방향을 덮어쓰지 않고 지속적으로 갱신할 수 있는 문서 운영 규칙을 추가했다.
