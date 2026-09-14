# JH.Jeong Research Context

> **주제:** Shelf retrieval을 위한 blocker-object contact manipulation
>
> **문서 성격:** 현재 합의, 작업 가설, 미결정 사항, 그리고 의사결정 과정을 함께 보존하는 living hand-off document
>
> **최종 갱신:** 2026-09-14

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
- **Track B:** 조작 중 Vision으로 현재 blocker pose와 근사 geometry를 지속적으로 제공받고, Vision·F/T·Tactile을 함께 사용한다. 우선 주어진 조작 목표를 실행하는 low-level 정책을 학습한 뒤, target 접근 공간 확보에 필요한 조작 목표 설정과 동작 전환까지 정책의 역할을 확장하는 RL 문제

이를 가장 짧게 표현하면 다음과 같다.

> **Track A는 제한된 시각 관측 아래의 contact adaptation을, Track B는 지속적인 시각·근사 기하 관측 아래의 목표 조건부 조작과 그 의사결정 통합을 연구한다.**

### 1.3 아직 연구 제목이나 Contribution을 확정한 것은 아님

**[현재 합의]** 위의 문제 정의는 연구 주제를 설명하기 위한 현재의 framing이다. Vision을 어떻게 사용할지, 어떤 한계와 장점을 주장할지, 두 Track의 최종 Contribution을 어떻게 표현할지는 앞으로 계속 고민하고 실험으로 구체화해야 한다.

따라서 현재 문장을 곧바로 확정된 논문 claim으로 사용하면 안 된다. 특히 아래 주장은 아직 입증되지 않았다.

- Vision을 사용하면 반드시 느리거나 보수적인 정책이 학습된다는 주장
- F/T·Tactile을 사용하면 반드시 더 빠르거나 더 안전하다는 주장
- 모든 방향과 회전을 하나의 범용 정책이 이미 처리할 수 있다는 주장
- 현재 가정한 pose·geometry 정보가 실제 환경에서도 오차 없이 얻어진다는 주장

### 1.4 최신 진행 방향 — Low-level 실행에서 조작 의사결정 통합으로

**[2026-09-14 현재 합의]** 먼저 **주어진 조작 목표를 안정적으로 실행하는 능력**을 학습하고, 이후 **목적 달성에 필요한 조작 목표와 순서를 결정하는 능력**까지 정책의 역할을 확장한다. 현재 구체화 대상은 Track B다.

1. **1단계:** 상위 판단기가 blocker와 목표 회전·이동을 제공한다. 정책은 `Approach / Contact Formation → Rotation → Push`를 기본 순서로 접촉을 형성·조절하며 명령을 실행한다.
2. **2단계:** 상위 입력을 blocker와 확보해야 할 접근 공간 등의 목적 수준으로 추상화한다. 정책이 회전 필요성, 중간 자세, 이동 방향·거리, 동작 전환과 완료 판단 등을 수행하는 방향으로 확장한다.

2단계의 첫 확장에서는 blocker 선택을 상위 판단기에 남긴다. 여러 blocker 중 선택·조작 순서를 결정하는 문제와 실제 target 인출 동작의 통합은 추가 확장으로 둔다.

여기서 `end-to-end에 가까운 정책`은 **공간 확보 목적에서 조작 행동까지 연결하는 범위를 넓힌다**는 뜻이다. Raw sensor부터 robot action까지의 완전한 end-to-end architecture를 확정한 것이 아니다. 두 단계는 학습·연구 범위의 구분이며, 두 개의 논문 또는 특정 수의 독립 정책으로 나눈다는 뜻도 아니다.

**Track A/B는 서로 다른 연구축이고, 1단계/2단계는 Track B를 중심으로 구체화한 진행 단계다. `Track A = 1단계`, `Track B = 2단계`로 해석하지 않는다.**

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
| **Approximated geometry** | Perception module이 제공하는 근사 외형·점유 정보. 실제 국소 접촉 표면의 위치·법선·곡률까지 정확히 아는 것은 아니다. |
| **Hand configuration** | 손목의 위치·자세와 손가락 관절 구성. 손목 6자유도와 손가락 제어 자유도는 별개다. |
| **1단계 / 2단계** | 주어진 조작 목표 실행을 우선 학습한 뒤, 공간 확보를 위한 조작 의사결정까지 통합하는 연구 진행 단계. Track A/B와 구분한다. |
| **End-to-end에 가까운 정책** | 세분화된 상위 지시를 줄이고 목적에서 행동까지 정책의 담당 범위를 확장한다는 현재 표현. Raw sensor 입력 또는 단일 neural network를 필수로 뜻하지 않는다. |

`partially observable`도 두 층위로 구분해야 한다.

1. **시스템 수준:** 최종 target object가 blocker에 의해 부분적으로 보이거나 가려져 있다.
2. **Track A 조작 수준:** manipulation 도중 blocker 자체의 visual pose tracking도 로봇·Hand·주변 물체의 가림으로 제한될 수 있다.

두 의미를 섞어 쓰지 않는다. 또한 Track B의 continuous Vision은 정확한 접촉 형상·마찰·질량 분포까지 모두 관측된다는 뜻이 아니다. **Pose와 근사 geometry의 관측 가능성**과 **전체 물리 상태의 완전 관측 가능성**을 동일시하지 않는다.

---

## 3. 전체 시스템에서 두 Track의 위치

### 3.1 개념적 실행 흐름

다음은 **1단계의 기본 역할 분담**이다. Track B의 2단계에서는 2번의 조작 목표 설정 및 조작 내부의 동작 전환·완료 판단 일부를 정책으로 옮긴다.

1. Perception/VLM 등이 선반 장면을 관측한다.
2. 상위 판단기가 target object를 찾거나 꺼내기 위해 이동해야 할 blocker와 조작 목표를 정한다.
3. VLA 또는 conventional motion planner가 로봇/Tool을 blocker 조작 시작 위치로 이동시킨다.
4. Track A 또는 Track B의 Low-level RL manipulation policy가 blocker를 조작한다.
5. 장면을 다시 관측하고 target object의 노출 또는 접근 가능성을 판단한다.
6. 필요하면 다른 blocker 조작 또는 target retrieval 단계를 수행한다.

### 3.2 우선 수행 범위와 이후 확장 범위

**[현재 합의]** 우선 수행할 1단계의 핵심은 4번의 blocker manipulation policy다. Track B의 기본 동작에는 물체 근처의 Approach와 contact formation을 포함하되, 상위 VLM planning, 전체 탐색 순서 결정, 장거리 reaching, target retrieval 전체를 처음부터 하나의 RL policy로 해결하지 않는다. Track A의 시작 시 접촉 여부와 contact formation 범위는 4절의 미결 상태를 유지한다.

**[2026-09-14 현재 합의]** 이 경계를 연구의 영구적인 범위로 고정하지 않는다. Track B의 2단계에서는 공간 확보 목적에 필요한 회전·병진 목표 설정, 동작 선택·전환 및 작업 완료 판단 등을 정책의 역할로 통합한다. 최종 target 선택, 장거리 접근, 외부 안전 중단, 실제 인출 실행까지 모두 정책으로 이전하기로 한 것은 아니다.

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
- 외부 안전 중단과 로봇 정지/명령 유지 등은 별도 execution layer의 책임으로 둘 수 있다. 조작 내부의 다음 동작 선택과 작업 완료 판단은 Track B 2단계에서 정책으로 통합할 대상이며, 외부 안전 중단과 구분한다.
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

## 5. Track B — Goal-Conditioned Manipulation에서 조작 의사결정 통합으로

### 5.1 현재 문제 정의와 단계적 진행

**[현재 합의]** 선반에서 target object를 꺼낼 수 있도록 blocker object를 재배치한다. 조작 중 continuous Vision으로 현재 pose와 근사 geometry를 제공받고, F/T·Tactile feedback으로 실제 접촉에 맞게 손 구성과 동작을 조정한다.

**[2026-09-14 현재 합의]** 연구를 다음 순서로 진행한다.

> **주어진 조작 목표의 실행 능력 확보 → 공간 확보 목적에 필요한 조작 목표 설정과 실행을 통합**

| 구분 | 1단계: Low-level 조작 학습 | 2단계: 조작 의사결정까지 통합 |
| --- | --- | --- |
| 핵심 질문 | 주어진 회전·병진 목표를 안정적으로 실행할 수 있는가? | 공간 확보에 필요한 조작 목표와 순서를 스스로 결정할 수 있는가? |
| 상위 입력 | 조작할 blocker, 목표 회전, 이동 방향·거리 | 조작할 blocker와 확보해야 할 접근 공간 또는 경로 |
| 정책 역할 | 접촉 형성·조절, 요구된 물체 운동 실행 | 회전 필요성, 중간 자세, 이동 방향·거리, 동작 전환 등을 결정하고 실행 |
| 동작 순서 | Approach → Rotation → Push를 기본 순서로 사용 | 필요한 동작과 순서를 상황에 따라 선택하는 방향 |
| 주된 평가 수준 | 조작 정확도와 접촉 안정성 | 접근 공간 확보 효과와 조작 효율·안정성 |

두 단계의 차이는 행동 종류의 추가보다 **상위 판단기가 제공하던 조작 지시를 정책이 얼마나 스스로 결정하는가**에 있다. Lateral·depth·diagonal translation과 회전을 다루는 기존 방향은 유지하며, 정확한 goal 범위와 curriculum은 미결이다. 학습 알고리즘, network 수, policy 호출 구조 및 최종 Contribution은 아직 확정하지 않았다.

### 5.2 관측 조건과 센서 역할

**[현재 합의]** Vision은 manipulation 전 과정에서 사용하며, F/T와 tactile도 함께 사용한다. Geometry는 정확한 전체 shape가 아니라 **근사 형상·점유 정보**로 구체화한다.

| 정보 | 역할 | 상태 |
| --- | --- | --- |
| 현재 blocker pose | 회전·병진 목표 오차와 진행 상태 확인 | 현재 합의 |
| 근사 geometry | 접근 방향과 초기 hand configuration 준비 | 현재 합의; 표현 형식은 미결 |
| Tactile | 실제 접촉 위치·분포를 참고해 손 구성과 접촉 배치 조정 | 사용은 현재 합의; 실제 센서에서 얻을 물리량은 미결 |
| Wrist F/T | 손에 전달되는 합력·모멘트를 참고해 전체 접촉 부하 조절 | 현재 합의 |
| Robot/hand state | EEF pose와 관절 구성·운동 상태 확인 | 기본 관측 후보; 정확한 항목은 미결 |

접촉 전에는 해당 물체의 tactile 신호가 없으므로 **근사 geometry로 준비하고, 접촉 후 실제 feedback으로 보정**한다. 국소 법선·전단력·미끄러짐 등을 센서가 직접 제공한다고 임의로 가정하지 않는다.

근사 형상과 실제 접촉 표면, perception 오차 및 미지 물성의 불확실성이 남으므로, continuous Vision을 사용한다는 이유만으로 전체 문제를 fully observable이라고 표현하지 않는다. Pose update rate, noise, geometry representation, sensor fusion 및 history 사용 여부는 미결이다.

### 5.3 1단계 — 주어진 조작 목표를 실행하는 Low-level policy

**[현재 합의]** 상위 판단기가 blocker와 목표 회전·이동을 제공하고, 정책은 물체 근처의 접근부터 접촉 조절과 조작 실행을 담당한다. 기본 순서는 `Approach / Contact Formation → Rotation → Push`다.

| 단계 | 물체에 대한 목표 | Hand의 역할 |
| --- | --- | --- |
| Approach / Contact Formation | 후속 조작을 수행할 접촉 형성 | 근사 geometry로 손목 pose·손가락 구성을 준비하고 실제 접촉으로 보정 |
| Rotation | 요구된 orientation 달성 | 필요한 회전 모멘트를 만들도록 접촉 위치와 힘 분포 조절 |
| Push / Translation | 목표 위치 도달 및 요구 orientation 유지 | 병진에 적합하게 접촉을 재조정하고 방향·속도·손 구성을 보정 |

상위 판단기는 **왜 이 blocker를 어느 방향으로 움직일지** 결정하고, 정책은 **접촉 불확실성 아래에서 그 움직임을 어떻게 수행할지** 학습한다. Rotation/Translation의 목표는 물체 상태를 뜻하며, 손목 자세 목표와 구분한다.

**회전 후 병진의 동기:** 원하는 이동 방향으로 힘을 전달하기 유리한 물체 자세와 접촉 배치를 먼저 확보한다. 접촉면이 이동 방향에 수직인 자세는 유용한 후보지만, 접촉 위치·마찰·힘의 작용선도 물체 운동에 영향을 주므로 면의 정렬만으로 회전 없는 병진을 보장하지 않는다. 손의 접근 위치 변경만으로 충분한지, 선반·로봇 제약 때문에 물체 회전이 필요한지는 task scenario에서 검증할 사항이다.

**[작업 가설 / 설계안]** 회전이 불필요한 명령은 회전량 0으로 처리할 수 있도록 한다. 회전에 유효한 접촉이 병진에도 유효하다고 고정하지 않고, 손목·손가락 자세와 접촉 위치를 재조정할 여지를 둔다.

**상위 명령 인터페이스 설계안**

- 회전각은 `Δθ`로 표기하고 각속도 `ω`와 구분한다. 회전축 방향과 회전각의 기준 좌표계를 명시한다.
- 최종 orientation만 요구할지, 특정 pivot 축 주위의 궤적까지 요구할지 구분한다. 고정 pivot을 요구한다면 축 방향 외에 위치도 필요하다.
- 이동 방향·거리 또는 최종 position을 입력하는 방식은 미결이다. 지정 위치 도달이 목적이면 명령 시점 위치 `p₀`, 방향 단위벡터 `d̂`, 거리 `s`를 이용해 `p_g = p₀ + s d̂`로 목표를 고정하는 안이 있다.
- 위 안에서는 회전 중 물체가 이동하더라도 `p_g`를 유지하고, Push 단계에서 현재 위치부터 남은 변위를 계산한다.
- 요구 orientation이 최종 배치 조건인지, 병진을 돕기 위한 중간 자세인지는 미결이다.

### 5.4 2단계 — 공간 확보 목적에서 조작 의사결정을 통합

**[현재 합의]** 1단계 조작 능력을 확보한 뒤, 상위 판단기의 일부 역할을 정책으로 옮긴다. 상위 입력을 세부 motion instruction에서 목적 수준으로 추상화한다.

> 1단계: “이 물체를 지정 각도만큼 회전시키고, 지정 방향으로 지정 거리만큼 이동시켜라.”
>
> 2단계: “이 물체를 조작해 target으로 접근할 공간을 확보하라.”

정책으로 통합할 의사결정의 범위는 다음과 같다. 정확한 통합 순서와 구현은 미결이다.

- 회전 필요 여부와 바로 Push할 수 있는지 판단
- 후속 이동에 적합한 중간 orientation과 접촉 배치 결정
- 이동 방향·거리 결정
- 회전에서 Push로 전환할 시점과 필요한 동작 순서 결정
- 접촉 재형성, 접촉 전환, 추가 조작 필요 여부 판단
- 공간 확보가 충분한지와 작업 완료 여부 판단

1단계에서 상위 입력이었던 회전량·회전축·이동 목표는 2단계에서는 정책의 의사결정 대상이 될 수 있다. 이를 명시적인 subgoal로 출력할지, 행동 생성 과정에 암묵적으로 통합할지는 미결이다.

**역할 분담의 경계:** 첫 확장에서는 조작할 blocker 선택을 상위 판단기에 남긴다. 여러 blocker 중 선택과 조작 순서까지 학습하는 문제는 후속 확장으로 둔다. 실제 target 인출 동작과 장거리 reaching 전체를 이번 2단계에 포함하기로 한 것은 아니다. 정책의 작업 완료 판단과 외부 execution layer의 안전 중단도 구분한다.

**필요한 관측 확장:** Blocker pose와 geometry만으로는 어느 재배치가 target 접근에 유효한지 판단하기 어렵다. Target 위치, 확보할 접근 영역, 주변 물체와 선반의 점유·공간 제약 등 **목적과 공간적 유용성을 판단할 정보**가 필요하다. 실제로 어떤 정보와 표현을 제공할지는 미결이며, 시뮬레이션의 전체 정답 장면을 자동으로 policy observation에 넣지 않는다.

**End-to-end의 현재 의미:** 목적에서 조작 행동까지 정책의 역할을 넓힌다는 방향이다. 외부 perception이 추정한 pose·geometry를 입력받는 정책을 raw sensor부터 action까지의 완전한 end-to-end 학습이라고 부르지 않는다. 단일 network나 VLA 학습을 필수 구조로 확정하지 않는다.

### 5.5 두 단계를 연결하는 설계 원칙

**[현재 합의]** 1단계에서 얻은 조작 능력을 후속 의사결정 통합의 기반으로 활용한다.

- 특정 고정 명령만 실행하도록 제한하지 않고 회전량·이동 방향·거리 등의 goal conditioning을 고려한다.
- Hand configuration을 Approach에서 고정하지 않고 전체 조작 중 조정할 수 있도록 설계한다.
- 동작 완료 여부, 실행 상태 및 필요한 접촉 상태를 후속 의사결정과 연결할 수 있게 한다. 정확한 interface는 미결이다.
- 1단계의 기본 동작 순서를 2단계에서도 필수 순서로 고정하지 않는다.
- 손목 6자유도와 손가락 제어 자유도를 구분한다. Tactile 사용 자체가 손목 6자유도 개방을 필수로 만들지는 않는다. Joint 관측과 joint action도 별개다.

**[작업 가설 / 구조 후보]** 다음 두 경로를 열어 둔다.

1. 학습된 low-level 정책을 호출하는 상위 정책을 추가한다.
2. 기존 정책을 초기값으로 사용해 조작 의사결정과 실행을 함께 학습한다.

Policy 개수, parameter freeze 여부, 공동 fine-tuning 방식, memory 구조 및 학습 알고리즘은 미결이다. 연구의 두 단계와 세 가지 동작 구분을 network 수 또는 논문 수와 동일시하지 않는다.

### 5.6 Reward 설계 방향

다음은 **[작업 가설 / 설계안]**이며, 확정된 수식이나 구현이 아니다.

| 범위 | 보상 목표 후보 | 주의점 |
| --- | --- | --- |
| 1단계 Approach | 접근 진전, 후속 조작에 유효한 접촉 형성 | 근사 geometry에서 만든 특정 손 자세를 실제 정답처럼 강제하지 않음 |
| 1단계 Rotation | 목표 orientation 오차 감소 | 필요한 회전 모멘트까지 일괄 억제하지 않음; 허용 위치 이탈은 미결 |
| 1단계 Push | 목표 position 오차 감소, orientation 및 경로 유지 | 단순 이동량 증가만 보상하지 않음 |
| 공통 접촉 안정성 | 과도한 부하·충격, 전도·낙하, 불필요한 동작 억제 | 필요한 재접촉과 접촉 전환을 방해하지 않음 |
| 2단계 목적 달성 | 접근 공간 확보, 불필요한 조작 감소, 안정적인 실행 | 회전·병진 정확도만으로 공간적 유용성을 대체하지 않음 |

Tactile은 관측으로 활용할 수 있으며 반드시 reward에 직접 들어가야 하는 것은 아니다. 접촉 면적·압력·활성 센서 수를 무조건 최대화하지 않는다. Manipulability는 후속 동작에 필요한 로봇 운동 여유를 확보하는 보조항으로 검토하되, Jacobian 기반 운동학적 manipulability와 물체 접촉의 조작 가능성을 구분한다.

단계별 reward 활성화, 동작 전환 gate, 완료·실패 threshold와 접근 공간의 정량적 정의는 미결이다. 평가 기준은 10.2절에서 단계별로 구분한다.

### 5.7 Contribution 후보와 주장 범위

현재 연구 방향을 한 문장으로 쓰면 다음과 같다.

> **접촉 기반 low-level 조작 정책을 먼저 학습하고, 이후 target 접근 공간 확보에 필요한 조작 목표 설정과 동작 전환까지 통합하는 목표 조건부 정책으로 확장한다.**

검증할 **[작업 가설]**는 다음과 같다.

- 1단계: 근사 형상으로 준비한 손 구성과 접촉 배치를 F/T·Tactile로 수정하면, 실제 접촉 형상 불일치 아래에서 목표 조작의 정확도·안정성이 개선되는가?
- 2단계: 학습된 실행 능력을 기반으로 중간 목표와 동작 전환을 결정하면, 공간 확보 성공과 조작 효율을 개선할 수 있는가?

단순히 센서를 함께 쓰거나 세 동작을 순서대로 학습한다는 사실만으로 novelty를 확정하지 않는다. 역할을 정책으로 통합하면 반드시 성능이 개선된다는 주장도 하지 않는다. 비교 방법과 최종 Contribution, 각 단계의 논문화 범위는 미결이다.

### 5.8 현재 설명용 조작 예시

UR5e + Inspire RH56E2 hand가 앞쪽 시리얼 박스를 조작해 뒤쪽 target의 인출 경로를 확보하는 예시를 사용한다. Approach에서는 모서리에 손가락을 거는 hooking을 표현하고, 회전 후 좁은 측면의 법선 방향으로 Push한다. 손바닥과 손가락 안쪽 패드가 물체 측면을 향해야 한다.

Top view, 양옆이 개방된 선반, 주변 물체 배치는 **설명용 그림의 구성**이다. 모든 학습 episode를 시리얼 박스·hooking·동일한 회전각·동일한 선반 구조로 제한한 합의는 아니다. 뒤쪽 target 인출은 최종 목적이며, 그림 속 policy가 직접 수행하는 동작은 blocker의 재배치다.

### 5.9 Track B에서 나중에 구체화할 사항

진행 방향은 정했지만 전체 세부 명세를 한 번에 확정하지 않는다. 핵심 미결 사항은 13.2절의 Track B backlog에서 관리한다. 특히 1단계 goal·action·전환 기준과 2단계 공간 확보 목적의 표현·평가 기준을 구분해 구체화한다.

---

## 6. Track A와 Track B의 관계

### 6.1 비교표

| 구분 | Track A | Track B |
| --- | --- | --- |
| 응용 시나리오 | 가려진 target을 위해 blocker 조작 | 가려진 target을 위해 blocker 조작 |
| Track 구분 기준 | 제한된 시각 조건에서 contact sensing으로 적응 | 지속적인 pose·근사 geometry 관측 아래 조작 실행 및 의사결정 통합 |
| 초기 Vision | 사용 | 사용 |
| 조작 중 Vision | 지속적 blocker-pose tracking에 의존하지 않음 | 지속적으로 사용 |
| 조작 중 pose | 직접적인 visual update 없음 | 지속적으로 알고 있다고 가정 |
| Geometry | 사용 여부 미결 | 근사 geometry를 지속적으로 제공받음; 정확한 국소 접촉 형상은 불확실 |
| F/T·Tactile | 주된 closed-loop feedback | 사용하며, Vision에 대한 보조 정보가 될 수 있음 |
| 현재 중심 문제 | Partial observability와 contact adaptation | 주어진 조작 목표 실행 → 공간 확보를 위한 목표 설정·동작 전환 통합 |
| 동작 종류 | 특정 동작 하나로 Track을 정의하지 않음 | 특정 동작 하나로 Track을 정의하지 않음 |
| 구체화 수준 | 문제와 초기 contribution 방향이 비교적 명확 | 2단계 진행 방향과 역할 분담 구체화; architecture·goal·reward·최종 Contribution은 미결 |

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

**2026-09-14에 합의한 2단계 확장은 Track B의 실행 능력에서 조작 의사결정으로 역할을 넓히는 것이며, Track A/B의 통합이나 Vision-Free 전환을 뜻하지 않는다.**

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
- **Track B:** continuous Vision을 통해 blocker의 current pose와 **근사 geometry**를 지속적으로 제공받는다. Geometry availability는 유지하되 실제 접촉 표면까지 정확히 아는 조건으로 해석하지 않는다.

따라서 `Geometry가 두 Track 모두에서 미지다` 또는 `Geometry 입력은 두 Track 모두 사용하지 않는다`는 이전 표현은 현재 유효하지 않다.

### 9.3 근사 형상과 실제 형상의 불일치

**[현재 합의]** Perception이 제공한 근사 geometry만으로 정확한 접촉 자세를 결정하기 어려운 조건에서 F/T·Tactile로 조작을 보정하는 방향을 다룬다.

**[작업 가설 / 학습 구성안]** 유사한 bounding box를 갖지만 실제 외부 접촉 표면이 다른 물체군을 사용해 근사 형상과 실제 접촉 형상의 불일치를 검증한다. 외형이 동일하고 내부 구조·질량 분포만 다른 물체군은 질량·무게중심·관성 변화에 대한 적응 실험으로 구분한다. 두 종류의 불확실성을 같은 문제로 취급하지 않는다.

### 9.4 여전히 남아 있는 미결 사항

- Bounding box, dimension/shape class, point cloud, mesh 또는 feature 중 어떤 근사 표현을 제공할지
- 실제 표면 형상 차이, perception bias/noise, occlusion을 어떤 분포로 구성할지
- 초기 feasibility에서 ground truth pose를 사용할 범위와 perception 오차 도입 시점
- 2단계에 필요한 target·주변 물체·선반 점유 정보를 어떤 표현으로 제공할지
- Geometry conditioning 및 contact feedback의 효과를 분리할 실험 구성

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

### 10.2 Track B의 단계별 후보 평가축

**1단계 — 요구된 조작 목표의 실행 능력**

- 최종 position/orientation error, 목표 회전·병진 성공률
- 접근·접촉 형성 성공, 접촉 손실·복구와 단계 전환 실패
- Peak/mean force, 병진 중 불필요한 회전, 전도·낙하와 수행 시간
- 다양한 방향·거리·회전 목표 및 근사/실제 형상 불일치에 대한 robustness
- 공통 Vision·robot-state 조건에서 F/T-only, tactile-only, combined contact sensing의 추가 효과
- 고정 hand configuration과 조작 중 조정하는 구성 비교

**2단계 — 공간 확보 목적에 맞는 의사결정과 실행**

- 확보된 접근 공간, 접근 경로의 유효성, target 인출 가능 조건 충족
- 필요한 조작 횟수, 불필요한 회전·접촉 전환, 전체 수행 시간
- 주변 배치와 공간 제약이 달라졌을 때의 목표 설정·동작 선택 효과
- 위 공간 확보 성과와 함께 측정한 접촉 부하·전도·낙하 등 실행 안정성

**[미결]** Clearance/retrievability의 정량적 정의, 비교 baseline, threshold, 평가 protocol은 아직 확정하지 않았다. 접근 공간 추정 지표와 실제 target 인출 실행 성공률은 구분해서 보고한다.

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

### Stage 5 — Track B의 근사 형상과 세 단계 조작 구체화

- **기록 날짜:** 2026-09-14
- **논의 배경:** Continuous Vision 조건에서 Hand Configuration 준비, Rotation, Translation을 연결하는 구체적인 조작 문제를 정리했다.
- **이전 해석:** Track B는 pose·geometry를 안다는 availability 조건과 다양한 방향·회전 목표만 정의되어 있었고, 동작 구성과 geometry 정밀도는 열려 있었다.
- **새 결론:** 근사 geometry와 실제 접촉 형상의 차이를 F/T·Tactile로 보완하며 `Approach / Contact Formation → Rotation → Push`를 기본 동작으로 다룬다. 목적은 앞쪽 blocker를 재배치해 뒤쪽 target을 꺼낼 공간을 확보하는 것이다.
- **변경 이유:** 손 구성 준비와 접촉 후 보정의 역할을 구분하고, 회전·병진이 최종 공간 확보에 기여하는 관계를 구체화하기 위함이다.
- **영향:** Track B 문제 정의·관측·reward 후보·설명 그림. Track A의 관측 조건과 미결 상태는 변경하지 않는다.
- **남은 질문:** 목표 orientation의 의미, 정확한 action·geometry 표현, contact formation·전환·종료 기준. Hooking과 좁은 면 Push는 현재 설명용 예시이며 모든 물체의 유일한 전략으로 고정하지 않는다.

### Stage 6 — Low-level 실행을 우선 학습한 뒤 조작 의사결정 통합으로 확장

- **날짜:** 2026-09-14
- **논의 배경:** 사용자가 low-level action 학습을 먼저 진행하고, 이후 상위 추론기의 역할 일부를 정책으로 옮기는 end-to-end에 가까운 확장을 제안했다. 정리된 방향에 동의하고 본 문서 업데이트를 요청했다.
- **이전 해석:** 상위 판단기는 조작 목표를 정하고 low-level policy는 이를 실행한다는 범위가 연구 전체의 고정된 경계처럼 읽힐 수 있었다.
- **새 결론 [현재 합의]:** 1단계에서 주어진 회전·병진 목표의 접촉 실행 능력을 확보한다. 2단계에서는 공간 확보 목적을 입력받아 회전 필요성, 중간 자세, 이동 방향·거리, 동작 선택·전환과 완료 판단 등을 정책이 담당하는 방향으로 확장한다.
- **변경 이유:** 실행 능력의 feasibility를 먼저 확보하고 이를 바탕으로 조작 목적에 맞는 의사결정을 학습하기 위함이다. 조작 제어와 의사결정의 모든 난점을 처음부터 동시에 해결하지 않는다.
- **역할 경계:** 2단계의 첫 확장에서도 blocker 선택은 상위 판단기에 남긴다. Multi-blocker 선택·순서, 장거리 reaching과 실제 target 인출 전체의 통합은 추가 확장이다.
- **영향:** 1절 요약, 3절 역할 분담, 5절 Track B, 9절 geometry, 10절 단계별 평가, 12절 대체 관계, 13절 결정·backlog, 14절 후속 작업 지침.
- **남은 질문:** 1단계 goal/action/전환 규격, 2단계 목적·장면 표현과 success metric, hierarchical 호출 또는 공동 학습 구조, parameter 재사용·freeze·fine-tuning, novelty와 논문화 범위.
- **해석상 주의:** 두 단계는 Track A/B와 대응하지 않는다. `End-to-end에 가까움`은 정책 역할의 확장을 뜻하며 raw sensor→action 단일 network를 확정한 것이 아니다. 성능 개선은 검증할 가설이다.

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
| Track B는 geometry를 안다는 availability만 정의 | 근사 geometry를 제공받고 정확한 접촉 표면의 불확실성은 남음 | **[구체화됨]** continuous Vision 조건은 유지 |
| Vision이 있으므로 전체 상태가 fully observable | Pose·근사 geometry 관측과 접촉·물성의 완전 관측은 별개 | **[대체됨]** |
| 상위 목표 설정과 low-level 실행의 경계를 연구 전체에 고정 | 1단계는 실행을 우선 학습하고 2단계는 목표 설정·동작 전환 일부까지 통합 | **[대체됨]** 우선 범위와 확장 범위를 분리 |
| Approach → Rotation → Push를 모든 단계의 필수 순서로 해석 | 1단계 기본 순서이며 2단계에서는 필요 동작과 순서를 결정하는 방향 | **[범위 한정]** |
| 피해야 할 오해: End-to-end에 가까운 확장 = raw sensor→action 단일 network | 목적에서 행동까지 역할 통합; perception·network·학습 구조는 미결 | **[해석 제한]** |
| 회전·병진 정확도로 연구 전체의 성공 평가 | 1단계는 실행 성능, 2단계는 공간 확보 효과와 실행 안정성을 평가 | **[확장]** |
| 피해야 할 오해: Track A/B = 1단계/2단계 | 연구축과 진행 단계는 별개; 이번 구체화는 Track B 중심 | **[해석 제한]** |

---

## 13. 현재 결정 레지스터와 미결 Backlog

### 13.1 현재 결정 레지스터

| ID | 상태 | 결정 |
| --- | --- | --- |
| D-001 | 현재 합의 | 공통 응용은 partially observable target을 위해 blocker object를 조작하는 선반 시나리오다. |
| D-002 | 현재 합의 | Track A/B는 서로 다른 동작이 아니라 유사한 시나리오의 서로 다른 연구 문제다. |
| D-003 | 현재 합의 | Track A는 initial blocker pose에 Vision을 사용하고 manipulation 중 지속적인 visual pose update에 의존하지 않는다. |
| D-004 | 현재 합의 | Track A의 manipulation feedback은 F/T와 tactile이 중심이며 RL policy를 학습한다. |
| D-005 | 현재 합의 · 2026-09-14 구체화 | Track B는 continuous Vision으로 current blocker pose와 근사 geometry를 제공받는다. 정확한 국소 접촉 형상까지 안다는 뜻은 아니다. |
| D-006 | 현재 합의 | Track B는 Vision, F/T, tactile을 모두 사용하며 Vision이 main source가 될 수 있다. |
| D-007 | 현재 합의 | Track B는 rotation과 다양한 방향의 translation을 지향하며 lateral pushing에만 한정하지 않는다. |
| D-008 | 현재 합의 | Vision 조건과 Contribution 서술은 계속 구체화할 연구 framing이며 절대 전제로 고정하지 않는다. |
| D-009 | 현재 합의 | 모든 세부 설계를 지금 한 번에 확정하지 않고 필요한 순서대로 구체화한다. |
| D-010 | 현재 합의 · 2026-09-14 | Low-level 조작 실행 능력을 우선 학습한 뒤, 조작 목표 설정과 동작 전환 등 상위 판단기의 일부 역할을 정책으로 통합한다. 현재 구체화 대상은 Track B다. |
| D-011 | 현재 합의 · 2026-09-14 | 1단계는 상위에서 blocker·목표 회전·이동을 제공하고 Approach / Contact Formation → Rotation → Push를 기본 순서로 수행한다. |
| D-012 | 현재 합의 · 2026-09-14 | 2단계는 접근 공간 확보 목적에서 회전 필요성·중간 자세·이동 목표·동작 전환·완료 판단 등을 결정하는 방향이며, 정확한 통합 순서는 미결이다. |
| D-013 | 현재 합의 · 2026-09-14 | 2단계의 첫 확장에서도 blocker 선택은 상위 판단기에 남긴다. Multi-blocker 선택·순서와 실제 target 인출 전체의 통합은 추가 확장이다. |
| D-014 | 현재 합의 · 2026-09-14 | 1단계는 목표 조작 실행 성능을, 2단계는 target 접근 공간 확보 효과와 실행 안정성을 구분해 평가한다. |
| D-015 | 현재 합의 · 2026-09-14 | End-to-end에 가까운 확장은 목적→행동의 역할 통합을 의미하며 raw sensor 입력, 단일 network, VLA 또는 특정 학습 알고리즘을 확정하지 않는다. |
| D-016 | 현재 합의 · 2026-09-14 | Track A/B와 1단계/2단계는 별개다. 이번 변경이 Track A의 관측 조건 변경이나 두 Track의 Vision-Free 통합을 뜻하지 않는다. |

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

#### Track B — 1단계 우선 명세

- Goal 표현: 회전각·축 / 목표 orientation, 방향·거리 / 목표 position, 기준 좌표계·시점
- Orientation이 최종 배치 조건인지 병진을 위한 중간 자세인지
- 평면 병진·yaw부터 시작할지와 feasible direction/rotation 범위
- 물체 근처 시작 pose 분포, contact formation 범위, 손목·손가락 action 범위
- 근사 geometry와 robot state, F/T·Tactile observation 및 history/fusion 표현
- 단계 전환·완료·실패 기준과 reward 구성
- 형상 불일치 및 물성 변화의 학습 분포, sensor·hand-configuration ablation

#### Track B — 2단계 확장 명세

- 확보할 접근 공간 또는 경로의 goal representation과 success metric
- Target·주변 물체·선반 점유 정보를 제공할 범위와 observation 표현
- 회전 필요성·중간 목표·동작 순서·완료 판단을 통합할 순서와 curriculum
- 중간 목표를 명시적으로 출력할지, 행동 생성에 암묵적으로 통합할지
- 상위 정책 + low-level 호출 또는 공동 학습 구조, 가중치 재사용·freeze·fine-tuning
- Fixed-sequence / 상위 판단기 지시 방식 등 비교 baseline과 공간 확보 효과의 검증
- Multi-blocker 선택·순서, 실제 target 인출까지 확장할 시점과 범위

#### Track B — 공통 미결

- Pose/geometry noise, occlusion, sensor calibration 및 Sim-to-Real 범위
- 최종 Method novelty, Contribution 및 각 단계의 논문·졸업연구 구성

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
9. 1단계 low-level 실행의 우선 범위와 2단계 조작 의사결정 통합의 확장 범위를 구분한다. 고정 동작 순서를 후속 단계의 필수 제약으로 만들지 않는다.
10. Track A/B를 1단계/2단계와 대응시키지 않는다. End-to-end에 가까운 방향을 raw sensor→action 단일 network나 두 Track 통합으로 바꾸지 않는다.
11. Continuous Vision·근사 geometry의 availability와 정확한 접촉 상태의 완전 관측을 구분한다. 손목 자유도와 손가락 자유도, joint 관측과 action도 구분한다.
12. 단계별 실행 성능과 공간 확보·실제 인출 성능을 혼동하지 않는다. 그림 속 시리얼 박스·hooking·선반 구성은 설명용 예시이며 자동으로 학습 명세가 되지 않는다.

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

- 2026-09-14 사용자 제안 및 후속 확인: low-level action 학습을 우선 수행하고 이후 상위 추론기의 일부 역할을 정책으로 통합하는 방향을 제안했다. 단계별 정리에 동의하고 `context.md` 업데이트를 요청했다.
- 본 대화의 Track B 구체화: 근사 geometry, Approach / Contact Formation → Rotation → Push, F/T·Tactile 기반 접촉 보정, 뒤쪽 target 인출을 위한 blocker 재배치 목적과 설명 그림을 논의했다. 설계 후보와 그림 조건은 확정된 architecture·학습 명세와 구분했다.
- [`docs/README.md`](../README.md): 전체 프로젝트의 기존 hand-off. Vision-based Sweeping의 한계, 두 Track으로의 decoupling, initial pose randomization, sequential manipulation, sensor/geometry 문제 등이 자세히 기록되어 있다.
- 과거 대화 초안: `Sweeping vs Handling/Pivoting`, 두 Skill의 Vision 조건, geometry 문제, 역할 중복 및 비교 시나리오를 검토했다. 해당 첨부 원문은 다른 컴퓨터에서 접근할 수 없을 수 있으므로, 후속 작업에 필요한 결론과 사고 과정은 이 문서의 7절·9절·11절·12절에 내재화했다.
- 2026-09-12 사용자 설명: Track A의 initial-Vision/contact-feedback 조건, Track B의 continuous-Vision/current-pose-and-geometry 조건, 그리고 두 Track이 다른 동작이 아닌 다른 연구 문제라는 점을 최신 결론으로 반영했다.

---

## 16. Change Log

### 2026-09-14 — Track B의 Low-level 우선 학습과 조작 의사결정 통합 방향 반영

- 사용자가 제안하고 확인한 **low-level 실행 능력 우선 확보 → 상위 조작 의사결정 일부를 정책으로 통합**하는 진행 방향을 기록했다.
- 1단계의 상위 입력·정책 역할과 2단계의 목적 수준 입력·역할 확장을 구분했다. 첫 확장에서는 blocker 선택을 상위에 남기도록 정리했다.
- Approach / Contact Formation → Rotation → Push의 기본 동작, 근사 geometry와 실제 접촉의 불확실성, 목표 표현과 reward의 설계 후보를 반영했다.
- Execution 성능과 공간 확보 효과의 평가를 구분하고, phase별 goal·관측·action·학습 구조 backlog를 갱신했다.
- `End-to-end에 가까움`의 의미, Track A/B와 진행 단계의 구분, 설명 그림과 학습 명세의 차이를 명시했다.
- 현재 요약·역할 분담·Track B·Geometry·평가·결정 레지스터를 일관되게 갱신하고 Stage 5·6 및 대체 관계를 추가했다. 기존 Track A 내용과 과거 의사결정 기록은 보존했다.

### 2026-09-13 — 문서 최초 작성

- 비어 있던 `docs/JH.Jeong/context.md`를 self-contained research hand-off로 작성했다.
- 현재 Track A/B 정의와 공통 blocker-handling 시나리오를 기록했다.
- 과거 Sweeping/Handling 구분에서 현재 연구축으로 바뀐 이유를 보존했다.
- Track B가 continuous Vision으로 current pose와 geometry를 안다는 최신 수정을 반영했다.
- 확정 사항, 작업 가설, 미결 사항, 대체된 해석을 분리했다.
- 후속 Agent가 연구 방향을 덮어쓰지 않고 지속적으로 갱신할 수 있는 문서 운영 규칙을 추가했다.
