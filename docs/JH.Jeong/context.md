# JH.Jeong Research Context

> **주제:** Shelf retrieval을 위한 blocker-object contact manipulation
>
> **문서 성격:** 현재 합의, 작업 가설, 미결정 사항, 그리고 의사결정 과정을 함께 보존하는 living hand-off document
>
> **최종 갱신:** 2026-09-15

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

1. `1. 현재 결론 요약` — 특히 1.5절의 정보·Action 명세 우선순위와 1.6절의 1단계 역할 경계
2. `5. Track B` — 특히 5.3절의 실행 범위, **5.6.1.6절의 최신 observation**, 5.7절의 contribution 후보와 5.11절의 baseline 비교 관점
3. `15.2 기존 참고 논문 목록과 DOI`와 `15.3 후속 baseline 탐색 자료` — 후보별 연결점·한계; 5.10절의 reward 분석은 이전 검토 이력
4. `3. 전체 시스템에서 두 Track의 위치`와 `6. Track A와 Track B의 관계`
5. `11. 의사결정 과정`과 `13. 현재 결정 레지스터와 미결 Backlog`
6. `14. 후속 Agent 작업 지침`
7. `4. Track A` — 별도 연구축의 배경이 필요할 때

최신 내용과 과거 기록이 충돌할 경우 다음 순서로 판단한다.

1. 사용자가 후속 대화에서 직접 수정한 최신 내용
2. 이 문서의 **현재 결론**과 **가장 최근 날짜의 결정 기록**
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

### 1.5 현재 우선 작업 — 정보·Action 명세 후 전체 Phase Reward formulation

**[2026-09-15 최신 합의]** 사용자는 Track B를 본인의 연구주제로 삼는 방향이며, 현재는 **1단계 low-level policy 학습**을 우선 구체화한다. `Approach / Contact Formation → Rotation → Push` 전체 phase를 고려한 reward formulation을 진행하되, reward 항을 정하기 전에 다음 정보·제어 계약을 먼저 정리한다.

1. 실제 실행 시 policy가 사용할 **observation**
2. Simulation 학습에서 reward·termination·evaluation에만 사용할 **privileged information**
3. EEF frame의 **delta pose**와 **hand joint action**으로 구성할 action space

각 reward term에는 task objective, 물리 원리, 안전 제약, 관찰된 failure mode 또는 선행연구 중 어떤 근거를 갖는지 명시한다. Baseline의 전체 접근 방식 검토는 폐기하지 않고 reward term과 비교 실험의 근거를 마련하는 병행 작업으로 둔다. Observation·privileged information의 정확한 항목, action encoding·scale·control interface와 reward 수식은 아직 미결이다.

**[2026-09-15 현재 진행 중]** 첫 번째 항목인 observation을 우선 구체화한다. 모든 후보 신호에 대해 `선행연구의 실제 입력·가공 → Isaac Lab 원천 데이터와 API → policy에 넣을 표현 → 실물에서의 대응 신호 → noise·latency·한계`를 함께 기록한다. 시뮬레이터가 제공한다는 이유만으로 실물에서 얻을 수 없는 exact state·contact를 actor observation에 포함하지 않는다.

**[2026-09-15 사용자 검토 후 현재안]** MLP actor에는 phase ID를 주지 않고 reward gate로 sequence를 학습한다. 현재 최소 입력은 EEF-frame push-conditioned goal, current EEF–object pose, episode-consistent OBB extent, hand q와 binary tactile·wrist F/T·previous-action history다. 실제 17 tactile sensor는 URDF link/pad 기준 `M`개 coarse region으로 pooling하는 안을 기본으로 두고 17-channel 표현과 비교한다. History는 MLP observation에 flatten하며 modality별 길이를 같게 강제하지 않는다. 세부는 5.6.1.6과 Stage 13을 따른다.

**[현재 합의: 연구 관점]** 단순히 회전과 밀기라는 서로 다른 동작을 나열하는 것이 아니라, 다양한 초기 물체 자세에서 **목표 방향의 pushing을 위해 물체를 주어진 자세로 돌려두고, 이에 필요한 hand configuration과 접촉 상태를 형성·조절하는 문제**로 이해한다. Contribution의 범위를 최초 Approach에만 한정하지 않는다.

GD2P의 energy와 Approach reward 분석은 폐기하지 않고 **5.10절의 이전 검토 이력·설계 후보**로 보존한다. 최신 baseline 비교 관점은 **5.11절**, 기존 논문 20편은 **15.2절**, 후속 탐색 논문은 **15.3절**에 기록한다. GD2P·Hermans et al.·TaskDexGrasp 우선 독해는 Agent의 추천이며, 최종 실험 baseline의 선정은 미결이다.

### 1.6 1단계에서 확인된 역할 경계와 contribution 후보

**[2026-09-15 현재 합의]** 1단계에서 **어떤 물체 자세가 밀기에 적합한지 판단하고 목표 orientation을 선택하는 것은 상위 모듈의 역할**이다. 상위 모듈은 blocker와 목표 회전·병진을 제공하고, 우리 policy는 손목·손가락 구성과 접촉 상태를 형성·조절하여 이를 실행한다.

- `적합한 물체 자세를 형성한다`는 1단계에서 **주어진 목표 자세를 실제로 달성한다**는 뜻이지, policy가 최적의 물체 자세를 선택한다는 뜻이 아니다.
- Approach는 접촉 준비의 시작이며, Rotation과 Push에서도 접촉 조절·재구성이 필요할 수 있다. 각 phase마다 별도의 novelty나 독립 network가 있어야 한다는 뜻은 아니다.
- 밀기에 적합한 중간 물체 orientation 자체를 policy가 선택하는 것은 2단계의 후속 확장으로 남긴다.

사용자가 합리적이라고 확인한 **1단계 contribution 후보의 표현**은 다음과 같다. 역할 경계와 framing에 대한 합의이며, 신규성·성능을 입증한 결과는 아니다.

> **주어진 물체 회전·병진 목표를 수행하기 위해, 접근부터 회전과 밀기까지 다지 손의 접촉 구성을 형성·전환하고 접촉 피드백으로 보정하는 조작 방법.**

**[작업 가설 / 미결]** 이 방향에서 기존 방법 대비 어떤 접촉 표현·평가·학습·제어 메커니즘을 새롭게 제안할지와 실제 성능 우위는 아직 정하지 않았다. `회전 후 밀기`, `다지 손 사용`, `촉각 사용`, `RL 사용` 또는 이들의 결합만으로 contribution이 증명되었다고 쓰지 않는다.

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
| 근사 geometry | 접근 방향과 초기 hand configuration 준비 | Episode-consistent object-local OBB extent를 현재 actor 표현으로 사용 |
| Tactile | 실제 접촉 위치·분포를 참고해 손 구성과 접촉 배치 조정 | 실제 17 sensor를 URDF 공통 `M`개 region의 binary로 pooling; granularity는 ablation |
| Wrist F/T | 손에 전달되는 합력·모멘트를 참고해 전체 접촉 부하 조절 | Bias-compensated EEF-frame 6D wrench와 history 사용 방향 |
| Robot/hand state | 현재 hand configuration 확인 | RH56E2 actuated `q_H` 6D를 현재 최소안에 포함; arm q·velocity·추가 kinematics는 초기 제외 |

접촉 전에는 해당 물체의 tactile 신호가 없으므로 **근사 geometry로 준비하고, 접촉 후 실제 feedback으로 보정**한다. 국소 법선·전단력·미끄러짐 등을 센서가 직접 제공한다고 임의로 가정하지 않는다.

근사 형상과 실제 접촉 표면, perception 오차 및 미지 물성의 불확실성이 남으므로, continuous Vision을 사용한다는 이유만으로 전체 문제를 fully observable이라고 표현하지 않는다. OBB tracking·noise, tactile pooling `M`, F/T preprocessing과 modality별 history 길이는 미결이다.

### 5.3 1단계 — 주어진 조작 목표를 실행하는 Low-level policy

**[현재 합의]** 상위 판단기가 blocker와 목표 회전·이동을 제공하고, 정책은 물체 근처의 접근부터 접촉 조절과 조작 실행을 담당한다. 기본 순서는 `Approach / Contact Formation → Rotation → Push`다.

| 단계 | 물체에 대한 목표 | Hand의 역할 |
| --- | --- | --- |
| Approach / Contact Formation | 후속 조작을 수행할 접촉 형성 | 근사 geometry로 손목 pose·손가락 구성을 준비하고 실제 접촉으로 보정 |
| Rotation | 요구된 orientation 달성 | 필요한 회전 모멘트를 만들도록 접촉 위치와 힘 분포 조절 |
| Push / Translation | 목표 위치 도달 및 요구 orientation 유지 | 병진에 적합하게 접촉을 재조정하고 방향·속도·손 구성을 보정 |

상위 판단기는 **왜 이 blocker를 어느 방향으로 움직일지** 결정하고, 정책은 **접촉 불확실성 아래에서 그 움직임을 어떻게 수행할지** 학습한다. Rotation/Translation의 목표는 물체 상태를 뜻하며, 손목 자세 목표와 구분한다.

**[2026-09-15 재확인]** 여기에는 **밀기에 적합한 목표 물체 orientation의 선택도 상위 판단기가 담당한다**는 경계가 포함된다. Policy가 결정·조절하는 손목 pose·손가락 configuration·접촉 배치와, 상위에서 주어지는 목표 물체 pose를 혼동하지 않는다. 상위 판단기의 목표 선택 알고리즘은 현재 low-level policy의 contribution이 아니다.

**회전 후 병진의 동기:** 원하는 이동 방향으로 힘을 전달하기 유리한 물체 자세와 접촉 배치를 먼저 확보한다. 접촉면이 이동 방향에 수직인 자세는 유용한 후보지만, 접촉 위치·마찰·힘의 작용선도 물체 운동에 영향을 주므로 면의 정렬만으로 회전 없는 병진을 보장하지 않는다. 손의 접근 위치 변경만으로 충분한지, 선반·로봇 제약 때문에 물체 회전이 필요한지는 task scenario에서 검증할 사항이다.

**[작업 가설 / 설계안]** 회전이 불필요한 명령은 회전량 0으로 처리할 수 있도록 한다. 회전에 유효한 접촉이 병진에도 유효하다고 고정하지 않고, 손목·손가락 자세와 접촉 위치를 재조정할 여지를 둔다.

**상위 명령 인터페이스 설계안**

- 회전각은 `Δθ`로 표기하고 각속도 `ω`와 구분한다. 회전축 방향과 회전각의 기준 좌표계를 명시한다.
- 최종 orientation만 요구할지, 특정 pivot 축 주위의 궤적까지 요구할지 구분한다. 고정 pivot을 요구한다면 축 방향 외에 위치도 필요하다.
- 이동 방향·거리 또는 최종 position을 입력하는 방식은 미결이다. 지정 위치 도달이 목적이면 명령 시점 위치 `p₀`, 방향 단위벡터 `d̂`, 거리 `s`를 이용해 `p_g = p₀ + s d̂`로 목표를 고정하는 안이 있다.
- 위 안에서는 회전 중 물체가 이동하더라도 `p_g`를 유지하고, Push 단계에서 현재 위치부터 남은 변위를 계산한다.
- **2026-09-15 구체화:** 회전의 연구 동기는 후속 pushing에 유리한 물체 자세를 준비하는 것이다. 그 목표 orientation은 1단계에서 상위가 지정한다. Push 중·종료 시 같은 orientation을 어느 오차 범위까지 유지할지, 별도의 최종 배치 orientation을 둘지는 미결이다.

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

**[2026-09-15 최신 우선순위]** 특정 reward 항을 먼저 채택하지 않고, 전체 phase에서 사용할 정보와 action의 경계를 먼저 확정한다.

- **Policy observation:** 실제 배포 시 얻을 수 있는 Vision·근사 geometry·F/T·Tactile·robot/hand state·goal 정보의 범위
- **Privileged information:** Simulation의 정확한 물체 상태·접촉·충돌 등 policy 입력에는 넣지 않고 reward·termination·evaluation에만 사용할 정보의 범위
- **Action 방향:** EEF frame에서 표현한 delta pose와 hand joint action. Delta rotation 표현, joint action의 position/delta/velocity 방식, scaling·clipping·control frequency와 controller interface는 미결이다.

Privileged information은 학습 목표와 안전 조건을 정확히 평가하기 위한 용도로 구분한다. 실제 정책이 사용할 수 없는 정보를 observation에 섞거나, privileged state에만 의존하는 phase 전환 규칙을 실물 실행 규칙으로 간주하지 않는다.

#### 5.6.1 Policy observation 정보 계약 초안

5.6.1.1–5.6.1.5는 문헌·Isaac Lab 구현 조사 직후의 **초기 Agent 제안과 검토 이력**이다. 이후 사용자와 항목별로 논의하며 phase·goal·geometry·tactile·history 구성이 바뀌었다. 현재안은 **5.6.1.6**을 우선하며, 앞 절의 point cloud 우선 확장, phase 입력, 다수 kinematic feature와 모든 modality의 동일 history 등의 제안을 확정 명세로 사용하지 않는다.

##### 5.6.1.1 선택 원칙

1. **Deployment-equivalent:** Actor가 받는 각 신호는 실물에서도 같은 의미·차원·좌표계·주기로 만들 수 있어야 한다. Simulation ground truth를 이용할 때에는 실제 perception·sensor의 출력처럼 noise, bias, update rate, latency와 dropout을 거친 proxy를 만든다.
2. **Raw와 representation을 분리:** Sensor가 만든 원천 데이터와 network 입력을 구분한다. RGB·촉각 이미지는 CNN/ResNet 후보지만, 소수의 contact on/off 값은 binary vector 또는 작은 MLP가 자연스럽다. Point cloud는 PointNet 계열이나 BPS가 후보이며 ResNet 입력으로 임의 변환하지 않는다.
3. **Task-relative frame:** Shelf/world absolute 값만 주기보다 object goal error, EEF–object 관계와 sensor-local signal을 사용한다. 단, 선반 경계와 경로 유지처럼 고정 환경 좌표가 중요한 정보는 shelf/task frame에 남긴다.
4. **가용 정보와 정답 정보의 분리:** Exact object pose, exact contact point·normal·slip과 물성은 reward·critic·evaluation용 privileged information일 수 있지만, actor가 실제로 얻는 신호로 위장하지 않는다.
5. **시간 의미 명시:** 모든 신호에 sample time, 유효성, hold 방식과 history를 정의한다. 서로 다른 rate의 마지막 값만 단순 결합해 현재 동시 측정값처럼 취급하지 않는다.

##### 5.6.1.2 선행연구에서 확인한 observation 구성

| 연구 | 실제 policy 입력·가공 | 이 연구에 주는 근거와 한계 |
| --- | --- | --- |
| [DexTouch, RA-L 2024](https://arxiv.org/html/2401.12496v2) (B09) | Arm·hand `q/dq`, palm pose·velocity, palm-relative fingertip position, task prior와 16개 FSR의 binary contact. Simulation에서는 sensor별 net contact-force norm을 `0.01 N`에서 threshold하고 실물 전압은 low-pass 후 threshold. 10 Hz policy와 asymmetric critic 사용 | Binary tactile를 ContactSensor로 근사하는 직접 근거다. `0.01 N`은 해당 simulator·sensor의 설정이며 우리 hardware threshold로 복사하지 않는다. Wrist F/T보다 tactile가 좋았다는 해당 과업의 ablation을 모든 조작에 일반화하지 않는다. |
| [Rotating without Seeing, RSS 2023](https://roboticsproceedings.org/rss19/p036.html) (B32) | Hand joint position 16D, binary tactile 16D, 이전 joint target 16D와 rotation axis 3D. 현재와 과거 3개 state를 stack하고, contact-force norm을 `0.01 N`에서 이진화. Tactile dropout·delay randomization과 10 Hz policy 사용 | `proprioception + binary contact + previous command + short history`로 구성한 최소 tactile policy의 근거다. In-hand rotation 전용이고 vision·arm motion·wrist F/T가 없다는 한계가 있다. |
| [VTDexManip, ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/file/e19b6f65791e350347bcff8a3955cb5b-Paper-Conference.pdf) (B27) | Egocentric RGB `224×224`를 ResNet18 계열로, 20개 force sensor를 `0.01 N`에서 이진화해 MLP로, hand joint angle·velocity를 proprioception branch로 처리한 뒤 feature 결합 | ResNet은 image branch에, binary tactile는 MLP에 사용하는 구분의 근거다. 우리 기본 perception 출력이 pose·근사 geometry라면 raw RGB branch를 반드시 재현할 이유는 없다. |
| [Robot Synesthesia, ICRA 2024](https://arxiv.org/abs/2312.01853) (B33) | Depth point cloud, proprioception으로 복원한 robot-mesh points, 활성 binary tactile sensor surface의 points를 palm frame으로 변환하고 modality tag를 붙여 PointNet으로 결합 | Binary 접촉의 공간적 위치를 보존하는 확장안의 근거다. Simulator exact contact point가 아니라 **활성 sensor 영역의 위치**를 사용하므로 실물 대응이 가능하다. |
| [Visuotactile Estimation and Control, CoRL 2024](https://proceedings.mlr.press/v270/ferrandis25a.html) (B12) | Occlusion·noise가 있는 object pose와 EEF pose·wrench history를 recurrent estimator가 처리하고, 추정 pose와 uncertainty를 control policy에 전달 | Continuous vision에도 validity·age·uncertainty와 history가 필요하다는 근거다. 해당 pusher·camera·wrench 모델을 그대로 사용한다는 뜻은 아니다. |
| [DexMove, ICLR 2026](https://peilin-666.github.io/projects/DexMove/) (B22) | 여러 frame의 hand joint, wrist pose, object pose, finger별 contact position·force와 marker-level normal/shear tactile field를 transformer가 처리 | 고해상도 tactile와 긴 history의 성능 상한 후보지만, vanilla ContactSensor와 현재 실제 hand로 직접 재현할 수 없는 정보를 기본 observation으로 넣으면 안 된다. |

##### 5.6.1.3 신호별 Isaac Lab–실물 대응과 가공안

| 그룹 | 원천 정보 | Isaac Lab 구현 경로 | Policy 입력 가공안 | 실물 대응·주의 | 우선 판단 |
| --- | --- | --- | --- | --- | --- |
| Task command | 상위 모듈의 목표 blocker pose 또는 회전·병진 명령 | Command term 또는 environment state에 episode별 goal 저장 | Shelf/task-frame 목표와 현재 추정 pose로 position/orientation error 계산. 회전은 quaternion 원소보다 log-map 3D 또는 planar yaw의 `sin/cos` 후보 | 실제 상위 모듈이 보내는 command와 동일해야 함. 최종 goal과 phase subgoal을 혼동하지 않음 | 필수 |
| Phase context | Approach/Rotation/Push 상태 또는 현재 subgoal | Manager/command term에서 one-hot·remaining subgoal 제공 가능 | 실물 FSM이 동일한 관측 조건으로 전환할 때만 phase one-hot 또는 subgoal을 포함 | Privileged exact phase predicate로만 만든 ID를 actor에 주면 sim-to-real 불일치 | 조건부 |
| Current object pose | Vision이 추정한 blocker pose | 초기 구현은 rigid-object GT를 perception rate로 sample한 뒤 noise·bias·latency·dropout을 적용하는 proxy; 이후 Camera 기반 estimator로 교체 | Shelf-frame pose 또는 `EEF→object` relative pose, goal error, valid flag·confidence·age. Twist는 finite difference+filter를 검증한 뒤 선택 | GT pose를 매 physics step 그대로 주지 않음. Occlusion 중 last-value hold와 age 증가를 명시 | 필수; 표현 미확정 |
| Approximate geometry | Upstream vision의 근사 외형·점유 | GT mesh를 직접 actor에 주지 않고 OBB/partial depth proxy 생성. Camera depth는 intrinsics로 unproject하고 crop·downsample 가능 | **최소안:** OBB extent/scale. **확장안:** 256/512-point partial cloud를 EEF/palm/object frame으로 변환해 PointNet/BPS encode. 크기가 task에 중요하므로 unit-sphere 정규화는 피함 | 실제 perception이 내는 geometry 형식과 동일해야 함. Exact collision mesh·SDF는 privileged로 분리 | 사용은 필수; 표현 미확정 |
| Shelf/scene constraint | 선반 경계, 주변 점유·여유 공간 | Fixed shelf geometry는 상수/relative distances; variable clutter는 depth/segmentation의 local point cloud·occupancy | 1단계가 고정 shelf라면 EEF/object의 shelf-relative pose와 경계 거리로 시작. 장면이 변하면 local occupancy/point set 추가 | 전체 simulator scene graph나 exact non-target pose를 자동 노출하지 않음 | 환경 다양화 시 필수 |
| Arm proprioception | Arm encoder `q_A`, `dq_A` | Articulation data의 joint position/velocity | Joint limit 중심으로 `[-1,1]` 정규화하고 velocity scale·clip. 6D EEF action이어도 singularity·joint-limit 상태 때문에 arm state 유지 | UR5e encoder와 동일한 joint ordering·sign·zero를 맞춤 | 필수 |
| Hand proprioception | Hand encoder `q_H`, `dq_H` | Articulation joint state | 실제 actuator 기준의 position/velocity를 limit·속도 기준으로 정규화. Simulator의 종속 관절을 별도 독립 관측처럼 추가하지 않음 | RH56E2는 공식 사양상 6 actuated DoF다. 실제 API가 제공하는 좌표와 coupling을 확인해야 함 | 필수 |
| EEF·finger kinematics | EEF pose/twist, fingertip/pad center와 surface normal | [FrameTransformer](https://isaac-sim.github.io/IsaacLab/develop/source/overview/core-concepts/sensors/frame_transformer.html)와 FK | Shelf/object/EEF-relative pose, EEF-frame twist, 각 tactile region 중심·법선을 고정 순서로 표현 | Encoder와 calibrated kinematics로 실물에서도 계산 가능. 계산값이 실제 contact point라는 뜻은 아님 | EEF 필수; pad feature는 확장 |
| Wrist F/T | 6D force·torque | Isaac Lab 3.x의 JointWrenchSensor; 2.x 계열은 articulation의 incoming joint wrench 등 version별 경로 확인 | Sensor/EEF frame으로 변환하고 zero-bias·tool gravity/inertia 보정, low-pass, clip·scale. 현재값과 짧은 history 우선; noisy derivative는 후순위 | Joint reaction wrench는 외부 접촉만이 아니라 hand dynamics·gravity도 포함할 수 있음. Hardware sensor frame·sign·range·bias와 맞춤 | 필수 |
| Tactile/contact | Finger/palm의 센서 영역별 접촉 신호 | 각 pad/link에 [ContactSensor](https://isaac-sim.github.io/IsaacLab/develop/source/overview/core-concepts/sensors/contact_sensor.html)를 두고 blocker와의 force norm을 명시적으로 threshold | **최소안:** binary vector `b∈{0,1}^m`, direct 또는 작은 MLP. Hysteresis·debounce 적용. **공간 확장:** 활성 sensor center/normal을 EEF/palm frame point set 또는 fixed masked vector로 결합 | RH56E2 공식 제품군은 T1 17개, T2 5개 tactile sensor 구성이므로 실제 variant를 확인. Simulator exact contact point·normal·shear·slip을 actor에 추가하지 않음 | Binary 최소안 우선 추천 |
| Previous command | 직전 policy action과 controller target | Action manager buffer와 desired EEF/hand target | `a_{t-1}` 및 필요 시 current desired pose/joint target의 actual-state residual | Delta action·actuator lag·action smoothing 때문에 현재 state만으로 내부 command 상태를 알 수 없는 문제를 줄임 | 필수 후보 |
| Timing/validity | Sensor timestamp, valid mask, dropout state | Observation term에서 age·mask 상태 관리 | Vision age/confidence, tactile sensor-valid mask, 선택적으로 modality별 time-since-update | Dropout을 0값과 구분. 실물 timestamp 기준으로 생성 | 필수 후보 |

[Isaac Lab ObservationManager](https://isaac-sim.github.io/IsaacLab/main/source/api/lab/isaaclab.managers.html)는 observation term별 `noise`, `clip`, `scale`과 history 설정을 지원하므로 위 계약을 term 단위로 구현할 수 있다. 다만 API와 wrench 경로는 Isaac Lab/Isaac Sim 버전에 따라 달라지므로 구현 전에 버전을 고정해야 한다.

##### 5.6.1.4 Modality별 구체 처리

**A. Goal·Vision·Geometry**

현재 blocker를 `B`, EEF를 `E`, shelf/world를 `W`라고 하면 EEF-frame의 relative position은 다음처럼 계산할 수 있다.

$$
p_{EB}^{E}=R_{WE}^{T}(p_{WB}^{W}-p_{WE}^{W}), \qquad
e_{p}^{E}=R_{WE}^{T}(p_{Wg}^{W}-p_{WB}^{W})
$$

Orientation goal error는 `Log(R_{WB}^{T}R_{Wg})`의 3D rotation vector를 후보로 두고, 과업을 planar yaw로 제한한다면 `sin(Δψ), cos(Δψ)`로 축소할 수 있다. 현재 pose, goal pose와 error를 모두 중복 입력하는 것은 자동으로 정답이 아니며 shelf constraint와 EEF action에 필요한 frame만 남기고 ablation한다.

Vision 입력은 처음부터 raw RGB로 고정하지 않는다.

- **Vector baseline:** Upstream perception이 제공할 estimated pose + confidence/validity/age + OBB extent. 가장 저렴하고 현재 연구 가정과 일치한다.
- **Geometry-aware extension:** Segmented depth를 metric-scale partial point cloud로 만들어 EEF/palm frame에서 PointNet 또는 BPS encoder를 사용한다. [Isaac Lab Camera](https://isaac-sim.github.io/IsaacLab/main/source/overview/core-concepts/sensors/camera.html)와 depth unprojection으로 구현 가능하지만, segmentation·crop·sampling과 실물 카메라 noise를 함께 맞춰야 한다.
- **Raw RGB ablation:** VTDexManip처럼 ResNet encoder를 둘 수 있으나, 많은 parallel environment에서의 rendering·encoder 비용과 sim-to-real appearance gap이 커진다. 현재 상위 perception이 pose·geometry를 제공한다는 연구 경계에서는 기본안이 아니다.

실제 pose estimator를 붙이기 전 simulator GT로 학습하더라도 `perception proxy`를 둔다. Proxy는 GT를 정해진 vision rate에서만 sample하고, episode bias·measurement noise·latency·dropout을 적용한 뒤 update 사이에는 zero-order hold한다. `valid/confidence/age`를 함께 제공하여 occlusion을 정확한 0 pose와 구분한다.

**B. Tactile**

표준 Isaac Lab ContactSensor는 optical/visuotactile image가 아니라 rigid-body contact report에 기반한 force 정보를 제공한다. 따라서 첫 구현은 각 실제 tactile region 또는 대응 pad link의 contact-force magnitude `f_i`로 binary contact를 만든다.

$$
b_{i,t}=\begin{cases}
1 & \|f_{i,t}\|>\tau_{\mathrm{on}}\\
0 & \|f_{i,t}\|<\tau_{\mathrm{off}}\\
b_{i,t-1} & \text{otherwise}
\end{cases}, \qquad \tau_{\mathrm{off}}<\tau_{\mathrm{on}}
$$

- `force_threshold` 설정에만 암묵적으로 의존하지 않고 observation용 binary를 명시적으로 계산한다.
- 짧은 low-pass/median filter, 최소 on/off 지속시간 또는 debounce를 사용해 physics substep의 순간 접촉과 chatter를 줄인다.
- DexTouch·Rotating without Seeing의 `0.01 N`은 문헌 재현값일 뿐이다. [RH56E2 공식 사양](https://en.inspire-robots.com/product/rh56e2/)의 sensor variant·분해능과 실제 no-contact/contact 분포를 측정해 `τ_on/off`를 정한다.
- Domain randomization에는 threshold, bias, delay, false positive/negative, sensor dropout·dead mask를 포함한다. 모든 sensor가 동시에 정상이라는 가정을 피한다.
- 기본 binary vector에는 작은 MLP면 충분하다. ResNet은 tactile image가 있을 때만 고려한다.
- ContactSensor의 contact filtering은 PhysX의 one-to-many 제약을 받으므로 여러 pad와 여러 상대 물체를 동시에 구분할 때에는 pad/body별 sensor configuration이 필요할 수 있다. Pinned version에서 collision/contact reporting과 tensor shape를 확인한다. `net force`를 taxel-level force field, 정확한 contact point, shear 또는 slip으로 해석하지 않는다.

Binary가 접촉의 공간적 의미를 너무 잃는다면 다음 확장안을 비교한다.

$$
o^{\mathrm{tac}}_i=[b_i,\; b_i p_i^E,\; b_i n_i^E]
$$

여기서 `p_i^E,n_i^E`는 exact contact point/normal이 아니라 kinematics로 계산한 **sensor 영역 중심과 명목 표면 법선**이다. 또는 활성 sensor surface에서 소수 points를 뽑아 Robot Synesthesia와 같은 tactile point cloud로 만들 수 있다. 연속 force magnitude `log(1+||f_i||/f_0)`는 binary 대비 ablation 후보지만, real sensor와 force scale을 보정하기 전에는 기본안으로 삼지 않는다.

[TacSL](https://isaac-sim.github.io/IsaacLab/develop/source/experimental-features/bleeding-edge.html)은 Isaac Lab contrib의 실험적 visuotactile 경로이며 RGB와 per-taxel force field를 제공할 수 있다. 그러나 별도 sensor model·object SDF/asset 준비와 버전 의존성이 있으므로 재현 가능한 최소 baseline과 분리한다. Optical tactile image가 핵심 연구 질문으로 바뀔 때 별도 extension으로 검토한다.

**C. Wrist F/T**

Wrench는 원 sensor frame `S`에서 EEF frame `E`로 회전시키며, 두 frame의 원점이 다르면 moment arm도 반영한다.

$$
f_E=R_{ES}f_S, \qquad
\tau_E=R_{ES}\tau_S+r_{E\rightarrow S}^{E}\times f_E
$$

Episode/reset 시의 zero bias, tool·hand의 gravity와 가능한 inertial component를 보정한 뒤 low-pass, symmetric clip과 정규화를 적용한다. Force·torque는 서로 단위와 범위가 다르므로 별도 scale을 둔다. Isaac Lab joint reaction wrench를 사용할 경우 접촉 외의 관성·중력·controller 반력이 섞일 수 있으므로 real F/T와 동일한 값이라고 가정하지 않고 bias·scale·latency randomization 및 무접촉/정적/동적 검증을 수행한다.

**D. Proprioception·kinematics·previous action**

- Arm과 hand `q/dq`는 실제 encoder가 제공하는 순서·zero·sign으로 정렬하고 joint/velocity limit로 정규화한다.
- RH56E2의 hand observation과 action은 우선 실제 6 actuator coordinates를 기준으로 한다. Simulator의 12개 물리 joint가 6개 actuator에서 종속되는 경우, 실물 API로 동일하게 복원되지 않는 독립 joint 값을 actor에 추가하지 않는다.
- EEF pose/twist와 fingertip/pad center는 `q`에서 계산 가능하지만, task geometry와 action frame의 관계를 쉽게 학습하도록 명시적으로 제공할 가치가 있다. 중복 효과는 ablation한다.
- Delta-pose·delta-joint action을 쓰면 직전 action과 누적 desired target을 알아야 actuator lag·smoothing 아래의 상태 모호성을 줄일 수 있다. DexTouch와 Rotating without Seeing의 previous target 사용이 근거다.

**E. History·sensor synchronization**

- DexTouch와 Rotating without Seeing는 10 Hz policy를 사용했고, [RH56E2 공식 FAQ](https://en.inspire-robots.com/faq/)는 sensor refresh와 ROS interface rate에 제약이 있음을 명시한다. 따라서 10 Hz는 첫 통합 후보일 뿐이며, 실제 vision·hand·F/T 측정 rate와 low-level controller rate를 측정한 뒤 확정한다.
- Policy interval 동안 tactile는 `last/max contact`, wrench는 filtered last 또는 mean/max를 비교하고, vision은 새 측정이 없으면 hold하되 age를 증가시킨다.
- 첫 vector baseline은 현재+과거 3개, 즉 4-step stack을 유력 후보로 둔다. Raw image/point cloud 전체를 반복 stack하기보다 frame encoder 후 latent history 또는 recurrent module을 사용한다.
- Noise는 raw sensor level 또는 물리적으로 의미 있는 normalized level에 적용한다. 모든 modality에 동일한 독립 Gaussian noise만 넣는 방식은 피하고 bias, latency, correlated noise와 dropout을 구분한다.

##### 5.6.1.5 우선 구현할 최소안과 확장안

**[Agent 추천 / 사용자 확인 전] 최소 vector baseline**

$$
o_t^{\mathrm{vec}}=[
o_t^{\mathrm{goal}},
\hat{o}_t^{\mathrm{object}},
o_t^{\mathrm{geom(OBB)}},
o_t^{\mathrm{arm}},
o_t^{\mathrm{hand}},
o_t^{\mathrm{EEF/finger}},
\tilde{w}_t^{E},
b_t,
a_{t-1},
o_t^{\mathrm{valid/time}},
o_t^{\mathrm{phase}}?]
$$

- `goal`: position/orientation error와 명령 중 실제 상위 interface에 필요한 항목
- `object`: noise·latency·dropout을 거친 estimated pose, confidence·age
- `geom(OBB)`: approximate size/extent의 저차원 시작점
- `arm/hand`: normalized actuated joint position·velocity
- `EEF/finger`: relative kinematics와 twist 중 확정 항목
- `w`: bias-compensated, filtered, clipped wrist wrench
- `b`: 실제 sensor layout에 맞춘 binary tactile vector
- `previous/time`: 이전 action·controller target 후보와 sensor validity/age
- `phase`: 실제 runtime FSM 또는 observable subgoal이 존재할 때만 포함

이 vector를 작은 modality별 encoder 후 결합하고 4-step stack 또는 recurrent encoder로 처리하는 안이 가장 구현 위험이 낮다. 정확한 차원은 arm/hand actuator 수, tactile variant, pad·fingertip feature 수와 goal 표현을 확인한 뒤 확정한다.

**확장·ablation 순서**

1. OBB를 partial point cloud + PointNet/BPS로 교체
2. Binary tactile에 active sensor position/normal을 결합하거나 tactile point cloud 사용
3. Binary와 calibrated continuous contact magnitude 비교
4. Vector pose 입력과 raw RGB–ResNet 입력 비교
5. Fixed history와 GRU/Transformer latent history 비교
6. 마지막으로 TacSL/TacEx 계열 optical tactile 경로를 별도 실험

기본안과 확장안 모두 actor observation, asymmetric critic input과 reward ground truth의 tensor를 명시적으로 분리한다. Phase-conditioned reward를 사용하더라도 actor가 phase를 받지 않는다면 관측만으로 phase를 추론할 수 있는지 검증하고, privileged phase를 이용한 숨은 action 규칙을 만들지 않는다.

##### 5.6.1.6 항목별 논의 후 최신 Observation 방향 — 2026-09-15

아래는 초기 초안 이후 사용자와 논의해 구체화한 **현재 방향**이다. Observation 자체, history 길이와 tactile granularity는 구현·ablation 전 최종 확정값이 아니다.

**Phase와 task goal**

- Actor에 Approach/Rotation/Push phase ID를 주지 않는다. 이전 연구에서 사용한 것처럼 phase별 gate 조건으로 reward term을 활성화하여 고정 순서의 long-horizon task를 학습하는 방향이다.
- Gate는 reward 의미를 전환하며 EEF·hand action 자유도를 phase별로 mask하지 않는다. Gate가 과거 통과 여부를 latch한다면 MLP observation history로 진행 상태를 추론할 수 있어야 한다.
- Goal은 임의의 object-pose reaching이 아니다. Primary objective는 **지정 방향·거리의 pushing**이고, orientation은 그 pushing에 적합하도록 상위가 제공하는 **준비 및 Push 중 유지 조건**이다.
- 상위 명령의 pushing direction `d_push^W`와 distance `s_push`로 목표 position을 고정한다.

$$
p_{O,g}^{W}=p_{O,0}^{W}+s_{\mathrm{push}}d_{\mathrm{push}}^{W}
$$

- Policy에는 object–goal error를 직접 넣기보다, 현재 EEF frame으로 변환한 `target object position + preparatory object orientation`을 goal로 주고 current EEF–object pose를 별도로 준다.

$$
g_t^E=[p_{O,g}^{E},\;\phi_{O,\mathrm{prep}}^{E}]\in\mathbb{R}^{6}
$$

Position component는 pushing 목표를, rotation component는 preparatory/maintenance constraint를 나타낸다. Rotation gate 전에는 후자가 중심이고, gate 후에는 전자의 progress가 중심이며 orientation을 유지한다.

**Unseen object와 coarse OBB**

- Point cloud·mesh를 actor에 제공하지 않는 방향이다. 실제 unseen object에서 occlusion 때문에 dense geometry가 불안정할 수 있으므로, 비교적 안정적인 3D OBB를 의도적인 coarse geometry로 사용한다.
- 매 vision frame에서 OBB를 다시 fitting하지 않는다. 조작 전 비교적 좋은 관측에서 object-local OBB template을 만들고, 중심·축 순서·extent를 고정한 뒤 manipulation 중에는 template의 SE(3) pose만 tracking하는 episode-local 방식이 우선안이다.
- Axis permutation·sign flip은 이전 orientation과 가장 가까운 후보를 선택해 억제한다. 대칭 물체에서는 하나의 global canonical frame을 강제하지 않고 initial tracked frame 또는 symmetry-equivalent orientation을 사용한다.
- Observation에는 여덟 corner나 EEF-oriented edge vector를 중복 제공하지 않고 object-local extent만 넣는다.

$$
d_O=[l_O,w_O,h_O]\in\mathbb{R}^{3}
$$

각 local axis가 EEF에서 향하는 방향은 current object orientation `R_EO`로 이미 정해진다. 예를 들어 `v_x^E=R_EO[l_O,0,0]^T`이므로 9D edge vector는 현재 최소안에서 제외한다.

**Binary tactile와 URDF 기반 coarse pooling**

- 사용자 시스템의 RH56E2는 총 17개 tactile sensor를 사용하는 것으로 파악한다. 정확한 위치·packet은 실제 장비/URDF에서 확인한다.
- Taxel별 collision patch 17개를 별도 rigid/contact body로 만드는 방법은 parallel simulation 연산량과 contact 안정성 문제 때문에 현재 우선안에서 제외한다.
- 실제 17 taxel과 simulation contact-bearing URDF link/pad를 공통 `M`개 region으로 pooling하는 방식이 현재 가장 현실적이고 안정적인 기본안이다. 동일 URDF link 체계를 사용하므로 양쪽 region의 위치 의미를 맞출 수 있다.

$$
b_{j,t}^{\mathrm{real}}=\max_{i\in G_j}b_{i,t}^{\mathrm{real}},\qquad
b_{j,t}^{\mathrm{sim}}=\mathbb{1}\!\left[\max_{\ell\in L_j}\|f_{\ell,t}\|>\tau_j\right]
$$

`G_j`는 real taxel index 집합이고 `L_j`는 대응 URDF collision link/pad 집합이다. Actor는 양쪽 모두 같은 `b_t\in\{0,1\}^{M}`을 받는다. 하나의 URDF link 내부를 다시 여러 region으로 나누려면 contact-point binning이 필요하지만, link/pad 수준 coarse pooling에서는 이를 피할 수 있다.

- `M=17`의 세밀한 binary tactile와 `M<17`의 coarse pooling은 **실험적으로 비교할 observation granularity**다. 비교할 때 tactile encoder output과 나머지 policy 크기를 같게 유지한다.
- 기본 조합은 `binary tactile + wrist F/T`다. Tactile는 접촉 위치/영역을, wrist wrench는 전체 force·moment 크기를 보완한다.
- 논문의 `0.01 N`을 복사하지 않는다. 실제 sensor의 no-contact drift·접촉 분포·power cycle을 측정해 `τ_on>τ_off` hysteresis와 debounce를 정하고, 그 범위를 simulation threshold·delay·dropout randomization에 사용한다.
- Simulation의 continuous per-region contact force는 actor tactile로 쓰지 않고 reward·asymmetric critic용 privileged information으로 활용하는 방향이다. 과부하·충격·접촉 부족을 평가하되 actor가 binary tactile와 하나의 wrist wrench로 구분할 수 없는 정확한 per-taxel force 분포를 목표로 강제하지 않는다.

**현재 MLP observation 표**

넣지 않기로 한 항목은 아래 표에서 제외했다. Rotation은 3D rotation vector 후보를 사용하며 정확한 representation은 action contract와 함께 최종 확인한다.

| Observation | 표현 | 차원 | 시간 범위 | 역할 |
| --- | --- | ---: | --- | --- |
| Push-conditioned goal | EEF-frame target object position 3D + preparatory object rotation 3D | 6 | Current | Pushing 목표와 준비/유지 orientation 제공 |
| Current object pose | EEF-frame estimated object position 3D + rotation 3D | 6 | Current | 현재 hand–object 관계 제공 |
| Coarse geometry | Episode-consistent object-local OBB extent `(l,w,h)` | 3 | Episode-fixed | Unseen object의 안정적인 근사 크기 제공 |
| Hand configuration | RH56E2 actuated joint position | 6 | Current | 현재 손가락 구성 제공 |
| Binary tactile | Real 17 taxel 또는 URDF 공통 coarse region의 on/off | `K_b M` | 최근 `K_b` step | 접촉 영역과 접촉 전이 제공 |
| Wrist F/T | Bias-compensated EEF-frame force 3D + torque 3D | `6K_w` | 최근 `K_w` step | 전체 접촉 부하와 변화 추세 제공 |
| Previous action | EEF delta pose 6D + hand joint action 6D | `12K_a` | 최근 `K_a` step | 명령과 이후 contact response의 관계 제공 |

단일 현재값만 사용할 때 `K_b=K_w=K_a=1`이며 차원은 `39+M`이다. History를 사용하는 MLP input은 다음과 같다.

$$
o_t^{\mathrm{MLP}}=[g_t^E,T_{EO,t},d_O,q_{H,t},b_{t-K_b+1:t},w_{t-K_w+1:t},a_{t-K_a:t-1}]
$$

$$
D_{\mathrm{MLP}}=21+K_bM+6K_w+12K_a
$$

History는 별도의 sensor가 아니라 **과거 sensor/action 값을 flatten하여 현재 actor observation에 추가하는 방식**이다. 현재 policy가 MLP이므로 recurrent hidden state에 맡기지 않는다. 모든 history에 같은 길이를 써야 할 이유는 없다.

- 같은 `K`는 `[tactile_t,wrench_t,action_{t-1}]`을 정렬하기 쉽고 첫 baseline·ablation이 단순하다는 장점만 있다.
- 최종 `K_b,K_w,K_a`는 step 수보다 실제 시간 범위로 정한다. Action-to-sensor latency, 접촉 transient와 policy frequency를 측정한다.
- 첫 비교는 `K=1` 대 공통 `K=4`가 단순하며, 이후 불필요한 action history 등을 줄여 modality-specific window를 정한다.

**현재 최소안에서 제외한 항목**

Phase ID, object velocity, arm/hand joint velocity, absolute EEF pose와 twist, fingertip/pad position, vision confidence/age, arm joint position, raw RGB, point cloud와 mesh는 현재 최소 observation에서 제외한다. 이 중 arm joint position, tracker validity/age와 일부 kinematic feature는 실제 failure 분석에서 필요성이 확인될 때 추가·ablation한다. 제외는 정보가 물리적으로 무의미하다는 뜻이 아니라, 현재 MLP input의 중복과 sim-to-real 부담을 줄인다는 뜻이다.

**Tactile granularity ablation**

| 비교군 | Tactile observation | 확인할 질문 |
| --- | --- | --- |
| F/T only | Tactile 없음 | Wrist F/T만으로 가능한 수준 |
| Coarse binary + F/T | URDF link/pad 단위 `M<17` | 가장 안정적인 sim-to-real baseline |
| 17-channel binary + F/T | 실제 17 sensor에 대응하는 binary | 세밀한 접촉 위치 정보의 추가 이득과 구현 비용 |

성공률뿐 아니라 Rotation/Push 단계별 성공, contact loss·재접촉, 과도한 force, sample efficiency, sensor noise/dropout robustness, simulation throughput과 sim-to-real gap을 함께 비교한다.

#### 5.6.2 Reward·termination·evaluation용 privileged information 초안

| 그룹 | Simulation ground truth 후보 | 우선 용도 | 주의점 |
| --- | --- | --- | --- |
| Object state | 정확한 blocker pose·twist와 초기·목표 pose | Goal progress, success, path/orientation deviation, evaluation | Vision 추정값을 policy 입력으로 쓰는 조건과 분리 |
| Exact geometry | Collision mesh, surface point와 signed distance | 충돌·침투, 근접도와 접촉 위치의 평가 | 근사 geometry만 보는 policy에 보이지 않는 단 하나의 정답 접촉을 강제하지 않음 |
| Contact state | Contact pair·position·normal, continuous region/contact force, normal/tangential impulse, relative slip velocity | 접촉 형성·유지·전환, 과부하·충격·접촉 부족 reward와 asymmetric critic | Binary tactile+global wrist wrench로 구분할 수 없는 정밀 per-taxel force 분포를 목표로 강제하지 않음 |
| Object safety state | CoM, 높이, roll/pitch, support relation, workspace 이탈 | 전도·낙하·비정상 들림 failure와 penalty | 실제 안전 중단은 관측 가능한 별도 supervisor 필요 |
| Collision state | Robot–shelf, robot–비대상 물체, self-collision과 collision impulse | Safety penalty와 termination | 필요한 blocker 접촉과 금지 접촉을 pair별로 구분 |
| Robot/controller state | Joint torque, limit margin, velocity, tracking error, saturation | 과부하·limit·불안정한 command 평가 | q·dq처럼 실물에서 얻는 값은 observation과 중복 가능 |
| Dynamics parameter | 실제 mass, CoM, inertia, friction | Domain randomization label, stratified evaluation, asymmetric critic 후보 | 보통 reward target 자체로 사용하지 않음 |
| Episode/phase truth | Curriculum phase, exact phase-success predicate와 failure cause | Training gate, termination, 진단 지표 | 실제 phase 전환 정책에 그대로 사용할 수 있다고 간주하지 않음 |

Privileged information은 세 용도를 구분해 기록한다.

1. **Reward ground truth:** 목표 달성도와 물리적·안전 조건을 정확히 계산
2. **Termination/evaluation ground truth:** 성공·실패 판정과 원인 분석
3. **Critic/randomization/diagnostics only:** Actor가 보지 않는 동역학 변수나 상세 상태

Reward privileged state는 policy가 알 수 없는 정답 행동을 지시하기보다 **결과와 안전을 평가**해야 한다. 예를 들어 정확한 접촉 위치로 접촉 품질을 평가하는 것은 후보가 될 수 있지만, 근사 geometry에서 식별할 수 없는 한 점만 정답으로 강제하면 observation–reward 불일치가 된다.

현재 tactile 논의에서는 simulation continuous contact force를 privileged signal로 두는 방향이다. Actor는 binary tactile, wrist F/T, sensor/action history를 통해 접촉 영역과 전체 부하 변화를 관측한다. Reward는 contact force를 무조건 크게 만드는 대신 필요한 접촉의 존재, 안전 상한 초과, 충격과 비정상적 누름을 평가한다. Exact force는 실물 실행 시 reward를 계산하기 위한 입력이 아니라 학습·critic·evaluation용 정보이며, 실제 안전 중단은 wrist F/T 등 배포 가능한 신호를 사용해야 한다.

#### 5.6.3 Action contract 초안

정규화된 action을 다음과 같이 두는 안을 우선 검토한다.

$$
a_t = [a_t^{p}, a_t^{R}, a_t^{H}] \in [-1,1]^{6+n_H}
$$

- `a_t^p`: EEF frame의 3D translation increment
- `a_t^R`: EEF frame의 3D rotation increment. Euler angle보다 rotation vector / exponential-coordinate 후보를 우선 검토
- `a_t^H`: 제어할 hand joint의 action

현재 desired EEF pose를 `T^d_{WE,t}=(R^d_{WE,t},p^d_{WE,t})`라 할 때 local increment를 다음처럼 합성하는 안이 명확하다.

$$
p^d_{WE,t+1}=p^d_{WE,t}+R^d_{WE,t}\Delta p_t^E,\qquad
R^d_{WE,t+1}=R^d_{WE,t}\operatorname{Exp}([\Delta\phi_t^E]_\times)
$$

Hand는 우선 **bounded joint-position increment + 하위 impedance/PD controller**를 후보로 둔다.

$$
q^d_{H,t+1}=\operatorname{clip}(q^d_{H,t}+s_H\odot a_t^H, q_{H,\min}, q_{H,\max})
$$

Position increment는 phase 사이에서 연속적으로 손 구성을 바꾸기 쉽고, 실제 joint limit를 명시하기 용이하다는 장점이 있다. Absolute target·velocity·torque action과의 비교 및 실제 hand의 coupling·underactuation은 확인해야 한다. Translation, rotation과 hand component의 scale, clip, smoothing, policy/control frequency, OSC/IK/impedance interface를 함께 고정해야 action 정의가 완성된다.

세 phase가 이 action space를 공유하되 **phase별 action masking은 기본안으로 두지 않는다.** Rotation 중 필요한 병진과 Push 중의 orientation correction을 허용해야 하기 때문이다. Phase는 task subgoal과 reward·success 의미를 구분하는 것이며, 물리적으로 결합된 운동 자유도를 강제로 분리하는 뜻이 아니다.

#### 5.6.4 Reward formulation으로 넘어가기 전 체크 형식

각 signal은 `source → 실물 가용성 → policy observation 여부 → privileged 용도 → frame/unit/rate → noise·normalization → history` 순서로 표를 확정한다. 이후 각 reward term을 다음 형식으로 추적한다.

> `term → 해결할 task/failure → 계산 신호 → privileged 여부 → 활성 조건/phase → 근거 → 예상 부작용 → ablation`

다음은 전체 phase를 포괄하는 **[작업 가설 / 설계안]**이며, 확정된 수식이나 구현이 아니다.

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

**[2026-09-15 현재 framing]** 1단계의 contribution 후보를 다음처럼 표현한다.

> **주어진 물체 회전·병진 목표를 수행하기 위해, 접근부터 회전과 밀기까지 다지 손의 접촉 구성을 형성·전환하고 접촉 피드백으로 보정하는 조작 방법.**

사용자가 확인한 것은 이 표현의 합리성과 **목표 물체 자세 선택은 상위, 접촉 실행은 policy**라는 역할 구분이다. 특정 method의 신규성이나 성능을 확정한 것이 아니다. 이전 표현인 `low-level 실행 → 공간 확보 의사결정 통합`은 연구 로드맵으로 계속 유효하지만, 그 자체로 1단계의 구체적인 방법적 contribution을 설명하지는 않는다.

Contribution을 Approach에만 한정하지 않는다. 접근 때 손을 준비하는 것뿐 아니라, 주어진 orientation으로 회전하는 중의 접촉 조절과 후속 Push에 필요한 재배치가 포함된다. 반대로 각 phase에 모두 새로운 알고리즘이 있어야 한다는 뜻도 아니다. Pushing은 준비의 유효성을 검증하는 후속 과업이 될 수 있으며, Push controller 자체에 별도 novelty가 필수인지는 아직 정하지 않았다.

검증할 **[작업 가설]**는 다음과 같다.

- 1단계: 같은 목표 물체 orientation에 도달해도 서로 다른 손목·손가락·접촉 상태가 후속 pushing 성공에 차이를 만드는가? 그 차이를 고려하는 표현·평가·학습 방식이 단순한 phase 연결보다 유효한가?
- 1단계: 근사 형상으로 준비한 접촉을 F/T·Tactile로 보정하면, 형상·마찰 등 불확실성 아래에서 접촉 형성·전환 실패를 줄이고 주어진 조작 목표의 정확도·안정성을 개선하는가? 센서 추가의 효과뿐 아니라 어떤 접촉 행동과 실패가 달라졌는지 검증한다.
- 2단계: 학습된 실행 능력을 기반으로 중간 목표와 동작 전환을 결정하면, 공간 확보 성공과 조작 효율을 개선할 수 있는가?

단순히 센서를 함께 쓰거나 세 동작을 순서대로 학습한다는 사실만으로 novelty를 확정하지 않는다. 준비 회전은 B21, 작업별 손 구성은 B01·B02, 촉각 기반 손목–손가락 제어는 B22, 후속 성공을 고려한 연결은 B08에 선행 사례가 있다. 이들 전체에 대한 우위나 `최초`를 현재 문헌 검토만으로 주장하지 않는다. 비교 방법과 최종 Contribution, 각 단계의 논문화 범위는 미결이다.

### 5.8 현재 설명용 조작 예시

UR5e + Inspire RH56E2 hand가 앞쪽 시리얼 박스를 조작해 뒤쪽 target의 인출 경로를 확보하는 예시를 사용한다. Approach에서는 모서리에 손가락을 거는 hooking을 표현하고, 회전 후 좁은 측면의 법선 방향으로 Push한다. 손바닥과 손가락 안쪽 패드가 물체 측면을 향해야 한다.

Top view, 양옆이 개방된 선반, 주변 물체 배치는 **설명용 그림의 구성**이다. 모든 학습 episode를 시리얼 박스·hooking·동일한 회전각·동일한 선반 구조로 제한한 합의는 아니다. 뒤쪽 target 인출은 최종 목적이며, 그림 속 policy가 직접 수행하는 동작은 blocker의 재배치다.

### 5.9 Track B에서 나중에 구체화할 사항

진행 방향은 정했지만 전체 세부 명세를 한 번에 확정하지 않는다. 핵심 미결 사항은 13.2절의 Track B backlog에서 관리한다. 특히 1단계 goal·action·전환 기준과 2단계 공간 확보 목적의 표현·평가 기준을 구분해 구체화한다.

### 5.10 1단계 우선 연구 질문과 문헌 기반 설계 후보 — 2026-09-14

**[이전 검토 이력; 2026-09-15 두 차례 우선순위 변경]** 아래 내용은 Approach reward를 먼저 분석하던 시점의 기록이다. 이후 baseline 전체 방법의 독해를 우선했으며, 최신 순서는 observation·privileged information·action contract를 정리한 뒤 전체 phase reward를 설계하는 것이다. 기술 분석과 설계 후보는 유지하되 contribution을 Approach에만 한정하지 않는다. 최신 해석은 1.5·1.6·5.6·5.7·5.11절을 따른다.

#### 5.10.1 Approach의 목표를 후속 동작과 연결

**[2026-09-14의 연구 초점]** Approach에서 적절한 hand configuration을 형성하는 문제를 먼저 구체화했다. 단순히 물체에 가까워지거나 접촉 수를 늘리는 것보다, 주어진 명령 아래에서 **후속 Rotation과 Push를 수행하기 유리한 hand–object 상태를 만드는가**가 중요하다는 문제의식은 유지한다.

**[작업 가설]** 이 상태의 적합성은 손목 pose·손가락 구성·실제 접촉·물체 상태·팔의 실행 여유를 함께 고려해 평가할 수 있다. Approach 종료 상태와 후속 정책의 성공 관계를 검증해야 하며, 지금 특정 평가함수로 확정한 것은 아니다.

Rotation에 적합한 접촉과 Push에 적합한 접촉이 다를 수 있다. 초기 configuration을 전체 동작 내내 고정하거나, Approach부터 최종 Push용 palm alignment를 무조건 강제하지 않는다. 손목·손가락·접촉 위치를 재구성할 여지를 둔다.

#### 5.10.2 GD2P에서 확인한 사실과 정정

기준 원문은 *Learning Geometry-Aware Nonprehensile Pushing and Pulling with Dexterous Hands*, arXiv:2509.18455v4, ICRA 2026이다. 참고문헌 B01.

- **RL 논문이 아니다.** 물체 geometry 표현으로 조건화한 diffusion model이 pre-contact hand pose를 생성한다.
- Diffusion의 생성·denoising 대상은 **hand pose**다. Point cloud를 diffusion으로 augmentation하는 연구로 설명하지 않는다.
- Hand pose는 손목 위치·자세와 손가락 관절 구성으로 이루어진다. Point cloud의 BPS 표현은 생성 조건으로 사용한다.
- Grasp synthesis의 최적화 도구에서 출발하지만, 이 연구의 데이터는 **nonprehensile push/pull 성공 pose**를 생성·검증해 구축한다. 일반적인 파지 dataset만으로 push를 수행했다고 단순화하지 않는다.
- 핵심 절차는 접촉 후보 기반 pose 최적화 → physics rollout으로 실제 동작 성공 판별 → 성공 pose 기반 생성 모델 학습 → 후보 생성·선택·실행이다.
- Tactile·F/T closed-loop 적응 또는 정밀 Rotation→Push 정책 학습을 검증한 연구는 아니다. 주된 실제 실행은 open-loop이며, 다단계 예시의 재계획과 고주기 contact feedback을 구분한다.
- 우리에게 유용한 점은 **기하학적 configuration 평가와 실제 동작 성공 검증을 연결하는 구조**다. GD2P energy를 그대로 RL reward로 전환한 효과는 별도로 검증해야 한다.

[GD2P 원문 v4](https://arxiv.org/html/2509.18455v4)

#### 5.10.3 GD2P energy 항별 의미와 reward로의 연결

논문의 목적함수:

$$
E(H)=E_{\mathrm{fc}}+w_{\mathrm{dis}}E_{\mathrm{dis}}
+w_jE_j+w_{\mathrm{pen}}E_{\mathrm{pen}}
+w_{\mathrm{dir}}E_{\mathrm{dir}}+w_{\mathrm{arm}}E_{\mathrm{arm}}.
$$

| 항 | 논문 및 공개 구현의 역할 | 우리 연구의 reward 후보와 주의점 |
| --- | --- | --- |
| $E_{\mathrm{fc}}$ | 접촉 위치·법선으로 계산하는 force-closure 관련 surrogate | 안정 파지와 목표 힘·토크 전달 능력을 구분. Rotation/Push에는 task-specific wrench capability를 검토 |
| $E_{\mathrm{dis}}$ | 선택된 hand contact candidate와 물체 표면의 거리 조절 | 손끝·손가락 링크·손바닥의 task-relevant surface proximity를 접근 shaping으로 활용 |
| $E_j$ | Hand joint limit 위반 억제 | RH56E2의 실제 제어 자유도·관절 결합·가동 범위에 맞게 설계. Limit 만족 자체는 좋은 manipulability 보장이 아님 |
| $E_{\mathrm{pen}}$ | Hand–object 침투 억제. 구현에는 self/table penetration 항도 별도 존재 | Intended contact와 금지 충돌을 구분하고 과도한 침투를 억제 |
| $E_{\mathrm{dir}}$ | 논문에서는 palm normal과 moving direction의 정렬 유도 | 현재 phase의 요구 동작으로 조건화. Palm normal 정렬과 물체의 좁은 면 normal 정렬은 별개 |
| $E_{\mathrm{arm}}$ | 논문에서는 palm의 위쪽 방향 성분을 억제하는 휴리스틱 | 실제 arm IK·충돌·Jacobian manipulability를 계산하는 항으로 오해하지 않음 |

**공개 코드 분석 기록:** 검토 기준 commit은 `ff91191165f232ad0914e3c6e9f65f8bc1ef89fd`다. 다음은 공개 구현 관찰이며 논문의 모든 실험 설정을 재현했다는 뜻은 아니다.

- `energy.py`의 force-closure 항은 $\|Gn\|^2=\|\sum_i n_i\|^2+\|\sum_i p_i\times n_i\|^2$ 형태다. 실제 F/T 측정값이 아니며, 마찰원뿔 안의 접촉력 크기를 최적화하는 exact force-feasibility 계산도 아니다. 작은 surrogate만으로 모든 방향에 대한 force closure를 보장하지 않는다.
- Pushing에서 손이 물체에 가하는 wrench를 항상 0으로 만드는 것이 목적은 아니다. 준정적 물체의 전체 wrench는 지지면 마찰 등의 반력과 균형을 이룰 수 있다.
- Object SDF는 내부 양수·외부 음수이며, distance 구현은 표면 접촉점 대신 바깥쪽 여유 거리 $-\delta$를 목표로 하는 SmoothL1 형태다. Helper의 기본 offset은 5 mm, generation script의 기본값은 15 mm였으며 실제 논문 실험값으로 단정하지 않는다.
- Joint-limit penalty는 허용 범위 내부에서 0일 수 있어 관절 경계까지의 여유나 좋은 손 자세를 자동으로 보장하지 않는다.
- Direction 항은 논문에서 음의 cosine이지만 공개 구현은 양의 cosine과 양의 weight를 사용한다. Arm 관련 항은 direction 항과 서로 다른 local axis를 사용한다. URDF·좌표계·이동 방향을 함께 추적하기 전에 단순한 부호 오류라고 확정하지 않는다.
- Weight의 크기만으로 항의 중요도를 비교하지 않는다. 단위·정규화·샘플 개수에 따라 scale이 다르다.

[고정 commit의 energy.py](https://github.com/Li-Yunshuang/GD2P/blob/ff91191165f232ad0914e3c6e9f65f8bc1ef89fd/gd2p/dataset_generation/utils/energy.py), [generation script](https://github.com/Li-Yunshuang/GD2P/blob/ff91191165f232ad0914e3c6e9f65f8bc1ef89fd/gd2p/dataset_generation/scripts/generate_hand_config_dicts.py)

#### 5.10.4 Approach reward의 역할 구분

아래는 **[작업 가설 / 설계 후보]**이며 확정 reward가 아니다.

| 역할 | 후보 신호 | 주의점 |
| --- | --- | --- |
| 접근 진전 | 상대 pose, task-relevant surface proximity | 근사 geometry에서 얻은 자세 하나를 정답으로 강제하지 않음 |
| 실제 접촉 형성 | Tactile의 접촉 영역·변화, F/T의 전체 부하 | 모든 접촉 수·압력·면적을 최대화하지 않음 |
| 후속 동작 적합성 | 요구 wrench 생성 능력, 후속 정책의 value 또는 rollout 성공 | 정적 물리 surrogate와 실제 실행 성공을 구분 |
| 실행 제약 | 관절 한계, 팔·손·선반 충돌, 과부하, 전도·낙하 | 필요한 접촉 전환이나 회전 토크를 일괄 억제하지 않음 |

Tactile은 observation만으로도 기여할 수 있다. Reward에 반드시 센서값을 직접 넣어야 하는 것은 아니다. Simulation reward/evaluation용 privileged state와 실제 policy observation을 구분한다. 접촉 이력이나 recurrent state 사용 여부는 미결이다.

#### 5.10.5 후속 동작 적합성을 평가하는 두 가지 후보

| 후보 | 참고 문헌 | 평가하는 내용 | 남은 문제 |
| --- | --- | --- | --- |
| Task-specific wrench capability | B02 TaskDexGrasp | 요구 힘·토크 방향과 현재 접촉이 생성할 수 있는 wrench의 관계 | Friction·surface normal·지지면 접촉의 불확실성, force/torque scale 정규화, task prior 설정 |
| Downstream execution value / success | B03 Critic 기반 선택, B08 Sequential Dexterity, B06 HANDFUL | 실제 후속 정책 관점에서 Approach 종료 상태의 유용성 | 학습 분포 밖 critic 신뢰도, reward로 이용할 때의 과대평가, rollout 검증 |

TaskDexGrasp의 Task Wrench Space는 사전 지정하는 방향 집합이다. 목표 이동 거리·회전각만으로 필요한 wrench가 유일하게 결정되는 것은 아니다. 접촉·마찰·질량·지지면 조건이 함께 영향을 준다.

B03은 후속 RL critic으로 초기 grasp **후보를 선택**한 연구다. 이를 Approach reward로 직접 사용하는 것은 우리의 확장안이다. 할인된 value를 보정된 성공확률로 설명하지 않는다. 물체를 손 안에서 파지한 상태와 선반에 지지된 nonprehensile 상태의 차이도 검증해야 한다.

#### 5.10.6 당시의 우선 확인 사항 — Reward 검토 재개 시 참고

1. Approach 종료 상태와 Rotation/Push의 실행 성공 기준을 정의한다.
2. 접촉 가능한 기하 조건과 후속 조작 적합성을 구분한다.
3. 여러 유효 configuration을 허용하고 phase 사이의 접촉 재구성을 검토한다.
4. 같은 근사 bounding volume이지만 **실제 외부 접촉 형상**이 다른 물체군으로 geometry mismatch를 평가하는 안을 유지한다. 동일 외형·다른 질량 분포는 별도의 물성 variation이다.
5. Geometry 정확도, F/T·tactile 입력, 고정/적응 hand configuration, 후속 적합성 항의 비교 실험을 검토한다. 최종 ablation 구성은 미결이다.
6. 우선 읽기 순서: **B02 TaskDexGrasp → B03 RL critic → B04 GraspXL → B05 UniDexFPM → B06 HANDFUL**. GD2P는 이미 정독·energy 분석을 시작한 기준 논문이다.

문헌의 모든 항을 한꺼번에 합치지 않는다. 각 항이 해결하는 실패 원인을 정의하고, 실제 후속 성공 개선 여부를 확인해 채택한다.

### 5.11 Baseline을 다시 고르는 관점 — 2026-09-15

**[유지되는 탐색 기준 / 우선순위는 1.5절에서 변경됨]** 완전히 같은 과업이나 동일한 로봇·센서·RL 알고리즘만 찾지 않는다. **목표 방향으로 밀기 위한 물체 자세와 hand configuration의 준비**에 어떤 해결 방식을 제시하는지 본다. 이 비교 관점이 1단계 policy에 목표 물체 자세 선택 권한을 추가하는 것은 아니다. Baseline 독해는 더 이상 reward formulation 이전의 단독 최우선 작업이 아니며, observation·privileged information·action 정리와 reward term의 근거 수집에 병행한다.

**[Agent 추천; 최종 baseline 미선정]** 우선 독해 후보의 역할은 다음과 같다.

| 후보 | 먼저 확인할 내용 | 우리 범위와 구분할 점 |
| --- | --- | --- |
| B01 GD2P | 주어진 물체 상태·밀기 방향에서 손목·손가락 pose를 생성하고 실제 push 성공으로 검증하는 전체 pipeline | 준비 회전 목표의 선택·실행이나 tactile closed-loop 정책을 그대로 제공하는 방법은 아님 |
| B21 Hermans et al. | 형상에서 안정 밀기·회전 접촉점을 예측하고, 안정 접촉이 목표 방향과 정렬되도록 물체를 돌린 뒤 미는 관점 | 다지 손 configuration 학습이 아님; 정량 실험의 중심은 두 동작 각각의 접촉점 예측 |
| B02 TaskDexGrasp | 작업에 필요한 힘·모멘트를 가할 수 있는 손 자세의 생성·평가 | 정적 pose 합성이며 지지면을 포함한 실제 Rotation→Push 성공과 동일하지 않음 |
| B22 DexMove | 촉각 기반 손목–손가락 공동 제어와 접촉 데이터 학습 | 시연 기반 flow policy; RL이나 우리 shelf 목표와 동일한 방법은 아님 |
| B23 Task-oriented contact optimization | 주어진 물체 운동을 적은 접촉력으로 실행할 접촉 배치 최적화 | 다중 이동로봇 설정; 독립 접촉점을 다지 손 관절로 그대로 치환하지 않음 |
| B24 Tactile-based negotiation | 회전 후 밀어 경로 확보, 방향별 접촉 위치 선택과 감각 기반 손 정렬 | 큰 장애물·mobile manipulation·계획/순응 제어; 다지 손 학습이 아님 |

ExDex(B07)·UniDexFPM(B05)은 arm–hand 제어와 구현 구조, Sequential Dexterity(B08)는 phase 연결의 비교 후보로 유지한다. GraspXL(B04)·critic 기반 선택(B03) 등은 후속 구성 요소 검토에 활용한다. CORN(B25)·DyWA(B26)·VTDexManip(B27)은 각각 일반 비파지 목표 제어·물성 적응·센서 표현의 보조 후보이며, RetrDex(B17)는 주로 2단계 공간 확보 목적의 참고다.

**[작업 가설: 공정한 비교]** GD2P 단독과만 비교하면 우리에게 추가된 회전 동작의 효과와 새로운 접촉 방법의 효과가 섞일 수 있다. `별도 회전 정책 + GD2P` 같은 단순 결합 방법, 접촉 구성 고정/적응 방식, 접촉 feedback 유무 등을 비교하는 안이 제안되었다. 이는 실험 설계 후보이지 구현·채택한 baseline이 아니다. 동일 목표·초기 물체 상태·사용 가능 관측 및 안전 조건을 맞추고, 원래 방법에 추가한 모듈을 명시해야 한다.

다음 독해는 **문제·성공 조건 → 목표/관측/행동 → 접촉 구성 생성·선택 → 학습/제어·전환 방식 → 실패와 비교 실험 → reward** 순으로 진행하는 것을 권한다. 문헌의 코드 공개 여부는 재현 완료와 다르며, 현재 대화에서 학습·실험을 재현한 것은 아니다.

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
- **Track B:** continuous Vision으로 blocker의 current pose를 tracking하고, 조작 전 정한 episode-consistent object-local OBB extent를 **근사 geometry**로 제공한다. Unseen object·occlusion 안정성을 이유로 point cloud·mesh는 현재 actor 최소안에서 제외한다. OBB가 실제 접촉 표면까지 정확히 나타낸다고 해석하지 않는다.

따라서 `Geometry가 두 Track 모두에서 미지다` 또는 `Geometry 입력은 두 Track 모두 사용하지 않는다`는 이전 표현은 현재 유효하지 않다.

### 9.3 근사 형상과 실제 형상의 불일치

**[현재 합의]** Perception이 제공한 근사 geometry만으로 정확한 접촉 자세를 결정하기 어려운 조건에서 F/T·Tactile로 조작을 보정하는 방향을 다룬다.

**[작업 가설 / 학습 구성안]** 유사한 bounding box를 갖지만 실제 외부 접촉 표면이 다른 물체군을 사용해 근사 형상과 실제 접촉 형상의 불일치를 검증한다. 외형이 동일하고 내부 구조·질량 분포만 다른 물체군은 질량·무게중심·관성 변화에 대한 적응 실험으로 구분한다. 두 종류의 불확실성을 같은 문제로 취급하지 않는다.

### 9.4 여전히 남아 있는 미결 사항

- Initial OBB template의 fitting, axis permutation·sign continuity, symmetry-equivalent orientation과 occlusion tracking
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

### Stage 7 — Track B 1단계 우선 구체화와 후속 동작을 고려한 Approach

- **날짜:** 2026-09-14, Stage 6 이후 문헌 논의.
- **논의 배경:** 사용자는 Track B를 본인의 연구주제로 삼는 방향을 제시하고 1단계 관련 문헌 조사를 요청했다. GD2P를 먼저 읽으며 energy 항별 의미와 Approach reward로의 연결을 논의했다.
- **정정한 해석:** GD2P는 point-cloud diffusion augmentation이나 RL policy 학습이 아니라, geometry-conditioned hand pose 생성과 nonprehensile execution 검증을 결합한 연구다.
- **현재 초점:** 좋은 Approach configuration을 후속 Rotation과 Push의 수행 가능성에 연결한다. 기하 거리·접촉 안정성만으로 충분한지 검토한다.
- **검토 후보:** Wrench-space 적합성, 후속 RL critic 기반 평가, 목표 조건부 접촉 보상, 다목표 reward 결합, 후속 손가락 사용 여유.
- **미결:** 실제 reward 수식, policy 구성, tactile 표현, phase 전환 및 contribution. 2단계 확장은 backlog로 유지한다.

### Stage 8 — Reward 항보다 baseline의 해결 방식을 먼저 검토

- **기록 날짜:** 2026-09-15. Stage 7 이후 대화의 순서를 보존한다.
- **사용자 요청:** Reward formulation을 고민하던 중, baseline 논문의 접근 방식과 해결 방식을 먼저 살펴보는 것이 우선이라고 설명하고 논문 탐색을 요청했다.
- **Agent의 초기 추천:** ExDex·DexMove·UniDexFPM·Sequential Dexterity·GD2P를 주요 후보로, CORN·DyWA·VTDexManip을 보조 후보로 제시했다. 과업·로봇·학습·센서·phase 연결의 유사성을 기준으로 삼았다.
- **새 결론 [현재 합의]:** 특정 reward 항을 채택하기 전에 baseline의 전체 문제 정의와 해결 pipeline을 이해한다. 기존 GD2P energy 분석은 보존하지만 즉시 reward로 구현하기로 한 것은 아니다.
- **남은 질문:** 최종 baseline 선정, 공개 코드 재현 가능성, 우리의 손·센서·목표 조건으로 이식할 범위.

### Stage 9 — Pushing을 위한 물체 자세와 hand configuration으로 비교 관점 이동

- **기록 날짜:** 2026-09-15. Stage 8 이후 사용자 설명과 문헌 탐색을 기록한다.
- **사용자 설명:** 물체가 다양한 초기 자세에 있더라도 밀기에 적합한 자세로 돌려두고 미는 관점이 중요하다. 완전히 일치하는 연구를 찾기보다 특정 방향으로 밀기 위한 물체 자세와 hand configuration에 집중해야 한다. 사용자는 GD2P가 어느 정도 부합한다고 평가했다.
- **Agent의 후속 추천:** GD2P에 더해 Hermans et al.(B21)의 준비 회전·안정 접촉점 선택, TaskDexGrasp(B02)의 작업별 wrench 기반 손 자세 합성을 우선 후보로 제안했다. Task-oriented contact optimization(B23)과 tactile-based negotiation(B24)을 추가했다. 이는 사용자가 최종 baseline을 확정한 기록이 아니다.
- **Contribution 논의:** 준비 회전, 다지 손의 pushing pose, tactile wrist–finger 제어, downstream skill 연결 각각에 선행 연구가 있음을 확인했다. 따라서 이 요소들을 사용한다는 사실만으로 novelty를 주장할 수 없고, 접촉 구성의 형성·전환·보정에 구체적인 방법과 검증이 필요하다고 설명했다.
- **작업 가설:** 후속 pushing에 유효한 접촉 상태를 준비하고 실제 geometry/contact mismatch를 보정하는 방향. `별도 회전 정책 + GD2P`와 같은 단순 결합 비교도 Agent가 제안했지만 확정하지 않았다.
- **영향:** 문헌 탐색의 기준을 generic dexterous RL/skill chaining에서 pushing을 위한 준비 조작으로 좁혔다. 물체 목표를 선택하는 주체는 다음 Stage 10에서 명확히 재확인했다.

### Stage 10 — Approach만의 contribution이라는 해석과 목표 자세 선택 권한을 정정

- **날짜:** 2026-09-15.
- **논의 배경:** 사용자가 “결국 처음 Approach 단계에 contribution이 있는 것인가?”라고 물었다. Agent는 준비 조작이 Approach뿐 아니라 Rotation 및 Push로 넘어가는 접촉 조절에도 걸친다고 설명했다.
- **모호했던 Agent 표현 [대체됨]:** “다양한 초기 물체 자세에서, 목표 방향의 pushing에 적합한 물체 자세와 hand configuration을 형성하는 조작 방법.” 이 문장은 물체 자세를 **선택**하는 것과 주어진 자세를 **달성**하는 것을 분명히 구분하지 못했다.
- **사용자 재확인:** “적합한 물체 자세는 1단계에서 우리의 policy가 판단할 내용이 아닌 것 아닌가?”라고 지적했다.
- **정정 [현재 합의]:** 상위 모듈이 밀기에 적합한 목표 물체 orientation과 병진 목표를 제공한다. Policy는 그 목표를 실행하기 위한 손목·손가락 구성과 접촉 상태를 형성·조절한다. 적합한 중간 물체 orientation 자체를 선택하는 것은 후속 확장이다.
- **정정 후 contribution 후보:** “주어진 물체 회전·병진 목표를 수행하기 위해, 접근부터 회전과 밀기까지 다지 손의 접촉 구성을 형성·전환하고 접촉 피드백으로 보정하는 조작 방법.”
- **사용자의 확인:** 위 정정이 더 합리적이라고 동의하고, 여기까지의 내용을 `context.md`와 `research_topic.md`에 반영하도록 요청했다.
- **합의의 범위:** 역할 경계와 연구 framing에 대한 확인이다. 최종 method novelty, reward, policy 수, 성능 우위가 확정된 것은 아니다. Approach만의 novelty로 축소하거나 각 phase마다 별도 novelty가 있다고 확대하지 않는다.
- **영향:** 최신 요약, Track B의 목표 해석·contribution, baseline 검토, 대체 관계, 결정 레지스터, backlog, 후속 Agent 지침과 research topic 요약.
- **남은 질문:** 실제 접촉 표현·평가·학습 방법, 회전–밀기 전환 조건, 목표 orientation 유지 허용 범위, 공정한 baseline과 검증 방법.

### Stage 11 — 전체 Phase Reward formulation을 위한 정보·Action 명세 우선

- **날짜:** 2026-09-15.
- **논의 배경:** 사용자는 baseline 독해를 단독 최우선으로 두기보다 `Approach / Contact Formation → Rotation → Push` 전체 phase의 reward formulation을 진행하기로 했다. Reward term마다 근거가 있어야 하며, 그 전에 정책 입력과 학습 전용 정보를 분리할 필요를 제기했다.
- **이전 우선순위:** Reward 항을 정하기 전에 baseline 후보의 전체 문제 정의와 해결 pipeline을 먼저 검토한다.
- **새 결론 [현재 합의]:** 전체 phase reward를 설계하기 전에 policy observation, reward·termination·evaluation용 privileged information, action contract를 순서대로 정리한다. Baseline과 문헌은 각 reward term의 근거와 비교 방법을 마련하는 병행 자료로 사용한다.
- **Action 방향:** EEF frame의 delta pose와 hand joint action을 사용한다. 정확한 rotation·joint 명령 표현, 차원, scale, clipping과 controller interface는 미결이다.
- **Reward 원칙:** 각 term이 해결하는 task objective·물리 조건·안전 제약·failure mode와 문헌 근거를 추적한다. Tactile·F/T를 observation으로 쓰는 것과 reward에 직접 쓰는 것을 동일시하지 않는다.
- **영향:** 최신 요약, Track B reward 설계, baseline 우선순위, 결정 레지스터, backlog, 후속 Agent 지침과 `research_topic.md`.
- **남은 질문:** Observation과 privileged information의 정확한 항목·표현·좌표계·history, action semantics, phase 전환·termination, term별 수식·정규화·weight·ablation.

### Stage 12 — 문헌과 Isaac Lab 구현 범위를 연결한 Observation 구체화

- **날짜:** 2026-09-15.
- **사용자 지시:** Observation부터 정리하며, 각 정보에 대해 선행연구의 구성뿐 아니라 Isaac Lab에서 얻고 가공할 수 있는 범위까지 검토한다. Tactile은 raw/learned image feature를 쓰는 연구와 달리 현재 simulator 제약에 따라 ContactSensor 기반 on/off가 필요할 수 있음을 명시했다.
- **조사 결과:** DexTouch와 Rotating without Seeing는 simulator contact-force norm을 threshold한 binary tactile를 사용하고, VTDexManip은 RGB를 ResNet 계열로 처리하는 반면 binary tactile는 MLP로 처리한다. Robot Synesthesia는 활성 binary sensor의 공간 위치를 point cloud에 넣는다. Visuotactile Estimation and Control은 pose uncertainty와 wrench history의 필요성을 보여준다.
- **Isaac Lab 해석:** Core ContactSensor는 optical tactile image가 아니라 rigid-body contact report를 제공하므로 binary contact baseline을 구현할 수 있다. Camera/depth unprojection, FrameTransformer, articulation/joint-wrench 정보와 ObservationManager를 이용해 vision proxy, kinematics, F/T와 history를 구성할 수 있다. TacSL은 contrib의 experimental path이므로 최소 baseline과 분리한다. 정확한 API는 version pinning이 필요하다.
- **Agent 우선안 / 미확정:** Structured goal·noisy pose·OBB geometry, arm/hand proprioception, EEF/finger kinematics, filtered wrist wrench, sensor-region binary tactile, previous action과 validity/age를 vector baseline으로 둔다. Geometry point cloud·spatial tactile·continuous force·raw RGB·optical tactile는 순차 extension/ablation으로 둔다.
- **중요한 제한:** 논문의 `0.01 N` tactile threshold를 그대로 복사하지 않는다. 실제 RH56E2 variant·센서 배치·noise floor를 확인해 hysteresis threshold를 calibration한다. Exact contact point·normal·shear·slip은 actor 관측으로 올리지 않는다.
- **영향:** 5.6.1 observation contract, 결정 레지스터, backlog, 후속 Agent 지침, `research_topic.md`의 현재 작업 원칙과 `papers.md`의 B32–B36·observation 그룹.
- **남은 질문:** 사용 중인 Isaac Lab/Isaac Sim 버전, RH56E2 T1/T2 및 실제 sensor packet, vision/hand/F/T update rate, upstream geometry 형식, phase ID 제공 여부, 최종 vector 항목·차원.

### Stage 13 — Observation 항목별 사용자 검토와 최소 MLP 입력 구체화

- **날짜:** 2026-09-15.
- **논의 방식:** 문서 업데이트만 하지 않고 Agent의 분석·제안을 먼저 제시한 뒤 사용자가 항목별로 반론·의견을 남기고, 그 결과를 현재안에 반영했다.
- **Phase 결정:** Actor에 phase ID를 주지 않는다. 이전 연구 경험을 근거로 phase별 gate가 reward term을 활성화하여 long-horizon sequence를 학습한다. History가 필요한 경우 MLP observation에 과거 값을 직접 포함한다.
- **Goal 정정:** 단일 object–goal error를 goal로 부르지 않는다. 목표는 지정 방향의 pushing이며, 상위가 제공하는 preparatory object orientation은 이를 위한 중간·유지 조건이다. EEF-frame의 target object position과 preparatory orientation을 current EEF–object pose와 별도로 제공한다.
- **Geometry 결정:** Dense point cloud·mesh보다 occlusion에 안정적인 OBB를 사용한다. Unseen object의 global canonical frame을 요구하지 않고 initial OBB axes·extent를 episode-local template으로 고정한 뒤 pose만 tracking한다.
- **Tactile 결정:** 기본은 binary tactile + wrist F/T다. Taxel별 collision body는 연산량 때문에 우선 제외하고, 실제 17 sensor와 simulation URDF link/pad를 같은 `M`개 coarse region으로 pooling한다. `M=17`과 `M<17`은 실험 변수다.
- **Privileged contact force:** Simulation continuous contact force는 과부하·충격·접촉 부족 reward와 asymmetric critic에 사용할 수 있다. Actor가 관측할 수 없는 정확한 국소 force 분포를 강제하지 않는다.
- **최소 MLP 입력:** Push-conditioned goal 6D, current EEF–object pose 6D, OBB extent 3D, hand joint position 6D, tactile history `K_bM`, wrench history `6K_w`, action history `12K_a`. 차원은 `21+K_bM+6K_w+12K_a`다.
- **History 정정:** History는 observation 외부의 별도 개념이 아니라 MLP에 flatten해 넣는 actor observation이다. 모든 modality에 같은 window를 적용할 필연성은 없으며, 공통 `K`는 첫 비교의 단순한 기준선이다.
- **초기 제외:** Phase ID, object velocity, joint velocity, EEF twist, fingertip 위치, vision confidence/age, arm q, raw RGB, point cloud와 mesh. 실제 failure가 근거를 제공하면 다시 추가한다.
- **남은 질문:** 실제 17 sensor의 URDF/packet mapping과 coarse group `M`, `K_b/K_w/K_a`, rotation representation, OBB tracker와 symmetry 처리, F/T preprocessing, exact Isaac Lab version.

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
| GD2P가 point cloud를 diffusion으로 augmentation | Geometry를 조건으로 hand pose를 생성 | **[대체됨]** |
| GD2P가 RL 또는 일반 grasp dataset만을 활용한 push 연구 | Nonprehensile push/pull pose의 최적화·physics 검증·생성 모델 학습 | **[대체됨]** |
| GD2P energy가 검증된 RL reward | 우리의 reward에 적용하는 것은 설계 후보이며 별도 검증 필요 | **[해석 제한]** |
| Approach에서 접촉을 많이 만들면 후속 동작에도 유리 | Task-specific 접촉 능력과 실제 후속 성공을 함께 평가 | **[작업 가설]** |
| Critic value를 그대로 성공확률로 사용 | 학습된 return 또는 ranking proxy이며 보정·검증 필요 | **[해석 제한]** |
| Approach reward의 수식 구체화를 최우선 작업으로 둠 | Baseline의 문제 정의와 전체 해결 방식을 먼저 검토 | **[우선순위 변경 · 2026-09-15]** 기존 분석은 5.10절에 보존 |
| Contribution이 처음 Approach 단계에만 있음 | 접근·회전·밀기 과정의 접촉 형성·전환·보정이 후보 범위 | **[대체됨]** 각 phase의 독립 novelty를 주장하는 것도 아님 |
| 1단계 policy가 밀기에 적합한 물체 자세를 스스로 선택 | 목표 물체 orientation·병진은 상위가 지정; policy는 손 구성·접촉을 조절하며 실행 | **[명시적 정정 · 2026-09-15]** 물체 자세의 선택과 달성을 구분 |
| 요구 orientation이 왜 필요한지 자체가 미결 | 현재 동기는 후속 pushing 준비; 목표는 상위가 지정 | **[구체화됨]** Push 중·최종 orientation 제약의 수치·표현은 미결 |
| 새 contribution 후보 문장에 동의 = 신규성과 성능이 입증됨 | 연구 framing·역할 경계에 동의; 구체 method와 실험 검증은 남음 | **[해석 제한]** |
| Baseline의 전체 해결 방식 검토를 reward 이전의 단독 최우선으로 둠 | Observation·privileged information·action을 먼저 정리한 뒤 전체 phase reward를 근거와 함께 설계; baseline은 근거 수집으로 병행 | **[우선순위 변경 · 2026-09-15]** Stage 8–10의 분석은 보존 |
| Tactile 입력의 raw/feature 형태를 먼저 정하지 않음 | Core Isaac Lab에서는 ContactSensor-derived binary tactile를 최소안으로 검토하고, spatial binary와 optical tactile를 확장으로 분리 | **[작업 가설 · 2026-09-15]** 실제 hand variant·calibration과 ablation 후 확정 |
| Simulation object/contact state를 편의상 actor가 직접 사용 | 실제 perception/sensor와 동일한 rate·noise·latency·dropout을 거친 proxy만 actor에 제공하고 exact state는 privileged로 분리 | **[구체화된 원칙 · 2026-09-15]** |
| Shared policy이면 phase one-hot을 주는 것이 우선 | Actor에는 phase를 주지 않고 observable progress·history 아래에서 reward gate로 phase별 term을 활성화 | **[대체됨 · 2026-09-15]** 이전 연구 경험을 반영 |
| OBB는 개발용이고 point cloud가 최종 geometry 후보 | Unseen object·occlusion에서의 안정성을 위해 episode-consistent OBB를 현재 actor geometry로 사용 | **[대체됨 · 2026-09-15]** point cloud·mesh는 현재 최소안에서 제외 |
| Goal을 estimated object–goal error로 표현 | EEF-frame target position과 preparatory object orientation을 current EEF–object pose와 별도로 제공 | **[대체됨 · 2026-09-15]** primary pushing objective와 준비 rotation을 구분 |
| 모든 17 taxel을 simulation에서 별도 collision patch로 재현 | Taxel별 body는 우선 제외하고 실제 17 sensor와 URDF link/pad를 공통 `M`개 region으로 pooling | **[대체됨 · 2026-09-15]** `M=17`과 coarse `M<17`은 ablation |
| History를 별도 observation 종류 또는 모든 modality에 동일한 `K`로 처리 | MLP actor observation에 필요한 modality의 과거값을 flatten하며 `K_b/K_w/K_a`를 별도로 선택 가능 | **[명확화 · 2026-09-15]** 공통 `K`는 첫 baseline일 뿐 |
| Simulation continuous tactile force를 actor에도 사용할 수 있음 | Actor는 binary tactile+global wrist F/T를 사용하고 continuous contact force는 reward·critic privileged signal로 우선 사용 | **[구체화 · 2026-09-15]** exact force distribution을 행동 정답으로 강제하지 않음 |

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
| D-017 | 현재 합의 · 2026-09-14 후속 논의 | Track B를 사용자의 연구주제로 삼는 방향이며, 현재는 1단계 low-level policy와 관련 문헌 검토를 우선한다. |
| D-018 | 이전 우선순위 · 2026-09-15 변경 | Approach에서 이후 Rotation과 Push를 고려한 hand configuration reward를 먼저 검토했다. 분석은 보존하며 최신 우선순위는 D-022를 따른다. |
| D-019 | 문헌 확인 | GD2P는 geometry-conditioned pre-contact hand pose 생성이며 RL 또는 point-cloud diffusion augmentation이 아니다. |
| D-020 | 작업 가설 | Task-specific wrench capability와 후속 정책의 execution value를 Approach 적합성의 평가 후보로 검토한다. |
| D-021 | 문헌 해석 원칙 | 원 논문, 공개 구현, 우리의 reward 확장안을 구분하고 DOI 유형·출판 상태를 명시한다. |
| D-022 | 이전 우선순위 · 2026-09-15 변경 | Reward formulation보다 baseline의 문제 정의와 전체 접근·해결 방식을 먼저 살펴보려 했다. 분석은 보존하며 최신 우선순위는 D-027을 따른다. |
| D-023 | 현재 합의 · 2026-09-15 | 연구 관점은 목표 방향의 pushing을 위해 물체를 주어진 자세로 돌려두고 손 구성·접촉을 준비하는 것이다. 최초 Approach에만 contribution을 한정하지 않는다. |
| D-024 | 현재 합의 · 2026-09-15 | 1단계의 적합한 목표 물체 orientation 선택은 상위 모듈의 역할이다. Policy는 주어진 물체 회전·병진 목표를 접촉 조작으로 달성한다. |
| D-025 | 합의된 framing / 검증 전 후보 · 2026-09-15 | Contribution 후보는 주어진 물체 목표를 위한 다지 손 접촉 구성의 형성·전환·피드백 보정 방법이다. 구체 method와 신규성·성능은 미결이다. |
| D-026 | Agent 추천 / 미선정 | GD2P·Hermans et al.·TaskDexGrasp를 관점별 우선 독해 후보로 두고, 단순 회전 정책+GD2P 등의 비교를 검토한다. 사용자가 실험 baseline을 확정한 것은 아니다. |
| D-027 | 현재 합의 · 2026-09-15 | 전체 phase reward formulation 전에 policy observation과 reward·termination·evaluation용 privileged information을 구분해 정리한다. |
| D-028 | 현재 action 방향 · 2026-09-15 | Action은 EEF frame의 delta pose와 hand joint action으로 구성한다. 세부 표현·scale·controller interface는 미결이다. |
| D-029 | 현재 합의 · 2026-09-15 | Approach·Rotation·Push 전체를 고려해 reward를 설계하고, 각 term의 목적·물리·안전·failure 또는 문헌 근거를 명시한다. |
| D-030 | 현재 합의 · 2026-09-15 | 각 observation 신호는 선행연구의 입력·가공, Isaac Lab 원천 데이터·구현, policy 표현, 실물 대응, noise·latency·한계를 함께 검토한다. |
| D-031 | 이전 Agent 추천 · D-037–D-039로 구체화 | ContactSensor-derived binary tactile 최소안을 제안했다. 이후 binary tactile+wrist F/T, URDF coarse pooling과 privileged continuous force로 구체화했다. |
| D-032 | 이전 Agent 추천 · D-036으로 대체 | Noisy pose·OBB에서 시작해 point cloud를 확장하려 했으나, 현재는 occlusion 안정성을 이유로 OBB를 actor geometry로 유지하고 point cloud를 최소안에서 제외한다. |
| D-033 | 구현 전 확인 필요 | Isaac Lab/Isaac Sim version, 실제 17 tactile sensor의 위치·packet·update rate를 확인하기 전에는 ContactSensor/wrench API와 `M`, history 차원을 확정하지 않는다. |
| D-034 | 현재 방향 · 2026-09-15 | 1단계 MLP actor에 phase ID를 주지 않고 phase별 gate로 reward term을 활성화한다. Gate는 phase별 action masking을 의미하지 않는다. |
| D-035 | 현재 방향 · 2026-09-15 | Goal은 임의의 object pose가 아니라 지정 방향 pushing과 이를 위한 preparatory orientation이다. EEF-frame target object position·preparatory orientation과 current EEF–object pose를 별도로 관측한다. |
| D-036 | 현재 방향 · 2026-09-15 | Unseen object·occlusion을 고려해 point cloud/mesh 대신 episode-consistent object-local OBB extent를 actor의 coarse geometry로 사용한다. Initial template의 axes·extent를 고정하고 pose만 tracking한다. |
| D-037 | 현재 방향 · 2026-09-15 | Tactile 기본 조합은 binary tactile + wrist F/T다. 실제 17 sensor와 simulation URDF link/pad를 공통 `M`개 region으로 pooling한다. |
| D-038 | 실험 설계 · 2026-09-15 | F/T only, coarse `M<17` binary+F/T와 17-channel binary+F/T를 비교해 tactile granularity의 이득·비용·sim-to-real robustness를 검증한다. |
| D-039 | 현재 방향 · 2026-09-15 | Simulation continuous contact force는 reward·asymmetric critic의 privileged information으로 사용하되, actor가 구분할 수 없는 정밀 국소 force 분포를 목표로 강제하지 않는다. |
| D-040 | 현재 방향 · 2026-09-15 | MLP history는 tactile·wrist F/T·previous action의 과거값을 actor observation에 flatten한 것이다. `K_b/K_w/K_a`는 같을 필요가 없고 실제 latency·transient로 정한다. |
| D-041 | 현재 최소안 · 2026-09-15 | Actor input은 goal 6D, current EEF–object pose 6D, OBB extent 3D, hand q 6D와 tactile/wrench/action history다. 차원은 `21+K_bM+6K_w+12K_a`다. |

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

- 현재 최소 MLP observation `21+K_bM+6K_w+12K_a`의 구현·학습 검증
- 사용 중인 Isaac Lab·Isaac Sim 정확한 version과 ContactSensor·JointWrench API 경로
- 실제 RH56E2 17 tactile sensor의 위치·단위·noise floor·packet rate와 dead/invalid 표시
- 실제 17 sensor→URDF link/pad의 grouping `G_j/L_j`와 coarse region 수 `M`
- F/T only / coarse `M<17` binary+F/T / 17-channel binary+F/T의 granularity ablation
- Tactile `τ_on/τ_off`, filter·debounce와 blocker-only contact filtering
- Wrist F/T의 frame·sign·range·bias·gravity/inertia compensation과 simulation joint reaction wrench 대응
- Policy/control frequency, modality별 aggregation·zero-order hold·latency와 `K_b/K_w/K_a`
- Phase ID 없는 reward gate의 latch/history 조건과 전환·완료 기준
- Initial OBB template fitting, axis permutation·sign continuity, occlusion과 symmetry 처리
- EEF-frame target position·preparatory orientation의 정확한 rotation 표현과 갱신 방식
- 초기 제외한 arm q, validity/age, fingertip feature가 실제 failure에서 필요한지 여부
- Policy observation과 privileged information의 항목별 구분, 사용 목적과 실물 가용성
- 각 정보의 표현, 좌표계, 단위, update rate, noise, normalization과 history 범위
- EEF-frame delta pose의 translation·rotation 표현과 합성 방식, scale·clipping·control frequency
- Hand joint action의 target/delta/velocity 방식, 실제 제어 자유도·관절 coupling과 controller interface
- Observation으로 계산할 phase 전환 신호와 privileged state를 사용할 training/evaluation 기준의 분리
- 전체 phase reward term별 목적·근거·수식·정규화·weight·활성 구간 및 ablation

- Baseline 전체 pipeline의 비교: 목표/관측/행동, 접촉 구성 생성·선택, 학습·제어, 실패 해결 방식과 재현·이식 범위
- 현재 contribution framing을 실현할 새로운 방법과 기존 방법 대비 검증 가능한 차이
- Approach 종료 상태의 정의와 후속 Rotation/Push 성공률의 관계
- Rotation 종료 시 물체 목표 달성과 hand/contact 상태의 후속 Push 적합성을 구분하는 평가
- Task-specific wrench prior의 표현·물리 가정과 downstream critic 평가의 신뢰도
- Geometry 기반 접근 shaping과 실제 tactile 접촉 평가의 역할 분담

- Goal interface: 상위의 pushing direction·distance와 preparatory orientation을 EEF-frame goal로 변환하는 기준 시점·좌표계
- 상위가 지정한 pushing 준비 orientation을 Push 중·종료 시 유지할 허용 오차 및 별도 최종 orientation 조건 여부; 목표 orientation 선택 주체는 상위로 확정
- 평면 병진·yaw부터 시작할지와 feasible direction/rotation 범위
- 물체 근처 시작 pose 분포, contact formation 범위, 손목·손가락 action 범위
- OBB tracking과 hand q, F/T·binary tactile·action history의 정규화·fusion 표현
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

13. 현재 우선 작업은 Track B 1단계의 policy observation, privileged information과 `EEF-frame delta pose + hand joint action`의 action contract를 정리한 뒤 전체 phase reward를 근거와 함께 설계하는 것이다. Baseline 독해는 term의 근거와 비교 설계를 위한 병행 작업으로 둔다.
14. 논문에 명시된 방법·공개 코드 관찰·우리 연구의 확장 후보를 구분한다. DOI 미확인을 DOI 부재로 단정하지 않는다.
15. 1단계에서는 상위가 적합한 목표 물체 orientation과 병진 목표를 제공한다. `물체 자세 형성`을 policy의 목표 자세 선택으로 해석하지 않는다. 손목·손가락 구성의 조절은 물체 목표의 선택과 별개다.
16. Contribution을 Approach에만 한정하거나, 각 phase마다 신규성이 입증되었다고 쓰지 않는다. 사용자가 동의한 framing과 검증된 논문 contribution을 구분한다.
17. 추천 논문·단순 결합 baseline·sensor ablation은 후보이며 확정 실험이 아니다. Baseline 독해는 정보·Action 명세와 term별 근거 수집에 병행하며, 문헌의 reward 항을 한꺼번에 조합하지 않는다.
18. Policy observation과 simulation privileged information을 명시적으로 분리한다. Privileged state만으로 계산한 전환·종료 조건을 실제 실행에서도 사용할 수 있다고 간주하지 않는다.
19. Reward term마다 해결하려는 failure와 근거를 기록한다. 센서값을 observation에 포함한다는 이유만으로 동일 값을 reward에 직접 넣지 않는다.
20. Observation을 추가할 때 sensor raw data, 전처리 결과와 learned feature를 구분하고, 선행연구·Isaac Lab API·실물 대응을 함께 기록한다.
21. ContactSensor의 force를 optical tactile image, exact contact point·normal·shear 또는 slip으로 확대 해석하지 않는다. Binary threshold는 문헌값 복사가 아니라 실제 sensor calibration과 domain randomization으로 정한다.
22. Isaac Lab/Isaac Sim 버전과 실제 RH56E2 17 sensor의 위치·packet이 확인되기 전에는 wrench API, coarse region `M`과 최종 observation dimension을 확정하지 않는다.
23. 최신 observation은 5.6.1.6을 따른다. 이전 5.6.1.1–5.6.1.5의 phase·point-cloud·다수 kinematic feature 제안을 현재안으로 되돌리지 않는다.
24. Actor에 phase ID를 자동으로 추가하지 않는다. Phase별 reward gate와 action masking을 구분하고, latch가 있다면 MLP history로 추론 가능한지 확인한다.
25. Goal을 일반적인 final object pose reaching으로 단순화하지 않는다. Target position은 pushing direction·distance에서 나오며 preparatory orientation은 후속 Push를 위한 중간·유지 조건이다.
26. Unseen object의 OBB를 매 frame 재추정하지 않는다. Episode-local template의 axes·extent 일관성과 symmetry를 관리하고 pose만 tracking하는 방향을 유지한다.
27. Real 17 taxel과 simulation URDF link/pad는 공통 `M`개 region으로 mapping한다. Taxel별 collision body 구현을 기본안으로 두지 않으며 17-channel 대 coarse tactile는 ablation으로 구분한다.
28. MLP에서 history는 actor observation tensor의 일부다. 모든 modality에 같은 window를 강제하지 않고 `K_b/K_w/K_a`와 실제 시간 범위를 기록한다.

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

### 15.1 연구 논의의 출처

- 2026-09-15 정리한 Stage 8–10의 후속 사용자 대화: reward보다 baseline 접근 방식 검토를 우선하도록 변경했고, pushing을 위한 물체 자세·hand configuration으로 관점을 좁혔다. 이어 Approach에만 contribution이 있는지 논의하고, 1단계 목표 물체 자세 선택은 상위 모듈의 역할임을 재확인했다. 사용자는 정정된 contribution 후보 표현이 합리적이라고 확인하고 두 문서의 업데이트를 요청했다.
- 2026-09-14 사용자 제안 및 후속 확인: low-level action 학습을 우선 수행하고 이후 상위 추론기의 일부 역할을 정책으로 통합하는 방향을 제안했다. 단계별 정리에 동의하고 `context.md` 업데이트를 요청했다.
- 본 대화의 Track B 구체화: 근사 geometry, Approach / Contact Formation → Rotation → Push, F/T·Tactile 기반 접촉 보정, 뒤쪽 target 인출을 위한 blocker 재배치 목적과 설명 그림을 논의했다. 설계 후보와 그림 조건은 확정된 architecture·학습 명세와 구분했다.
- [`docs/README.md`](../README.md): 전체 프로젝트의 기존 hand-off. Vision-based Sweeping의 한계, 두 Track으로의 decoupling, initial pose randomization, sequential manipulation, sensor/geometry 문제 등이 자세히 기록되어 있다.
- 과거 대화 초안: `Sweeping vs Handling/Pivoting`, 두 Skill의 Vision 조건, geometry 문제, 역할 중복 및 비교 시나리오를 검토했다. 해당 첨부 원문은 다른 컴퓨터에서 접근할 수 없을 수 있으므로, 후속 작업에 필요한 결론과 사고 과정은 이 문서의 7절·9절·11절·12절에 내재화했다.
- 2026-09-12 사용자 설명: Track A의 initial-Vision/contact-feedback 조건, Track B의 continuous-Vision/current-pose-and-geometry 조건, 그리고 두 Track이 다른 동작이 아닌 다른 연구 문제라는 점을 최신 결론으로 반영했다.

### 15.2 기존 참고 논문 목록과 DOI — 2026-09-14 확인

주요 관련 논문 **17편(B01–B17)**과 GD2P의 energy·데이터 생성 배경 논문 **3편(B18–B20)**을 수록한다. DOI는 앞선 문헌 조사에서 확인한 정보를 반영했다. **arXiv DOI는 정식 출판 DOI와 구분**한다. 정식 DOI 미확인은 DOI가 없다는 단정이 아니다.

**이전 읽기 순서(2026-09-14): B02 → B03 → B04 → B05 → B06.** Reward/Approach 중심의 당시 순서로 보존한다. **2026-09-15 현재 Agent 추천은 B01·B21·B02의 전체 방법을 우선 비교**하는 것이다(5.11절). B07–B16은 실행·센서·phase 연결, B17은 retrieval 목적과 후속 확장, B18–B20은 energy·데이터 평가의 배경으로 계속 활용한다. 후속 탐색 논문 B21–B27은 15.3절에 수록한다.

| ID | 약칭 | 방법 / 주요 활용 |
| --- | --- | --- |
| B01 | GD2P | 기하 조건부 diffusion hand pose 생성; RL 아님 |
| B02 | TaskDexGrasp | Task-oriented hand pose의 미분 가능한 최적화; RL 아님 |
| B03 | RL critic으로 초기 grasp 선택 | 학습된 in-hand RL critic으로 초기 grasp 후보 선택 |
| B04 | GraspXL | 목표 조건부 RL grasp-motion synthesis |
| B05 | UniDexFPM | RL expert 학습과 diffusion policy distillation |
| B06 | HANDFUL | 손가락별 접촉 보상과 순차 RL curriculum |
| B07 | ExDex | RL 기반 arm–hand nonprehensile manipulation |
| B08 | Sequential Dexterity | RL skill chaining |
| B09 | DexTouch | Tactile RL와 sim-to-real |
| B10 | Tactile Pushing | 촉각 기반 goal-conditioned model-based / model-free RL |
| B11 | Bi-Touch | 양팔 tactile RL |
| B12 | Visuotactile Estimation and Control | Visuotactile estimator + uncertainty-aware RL |
| B13 | Coarse-to-Fine Pushing | Vision·touch·proprioception 기반 coarse-to-fine pushing |
| B14 | Force Push | Force-feedback / admittance controller; RL 아님 |
| B15 | RoboPack | Visuotactile recurrent dynamics model + MPC; RL 정책 학습 아님 |
| B16 | Nonprehensile Pregrasp | Graph search·optimal control·학습된 graspability; RL 아님 |
| B17 | RetrDex | Clutter clearing / retrieval RL |
| B18 | Differentiable Force Closure Estimator | 미분 가능한 force-closure surrogate 기반 grasp 합성 |
| B19 | DexGraspNet | Grasp pose 최적화·시뮬레이션 검증·대규모 데이터 생성 |
| B20 | Get a Grip | 성공·실패 grasp 데이터 기반 evaluator 학습 |

#### B01. GD2P

- **정식 제목:** Learning Geometry-Aware Nonprehensile Pushing and Pulling with Dexterous Hands
- **발표 정보:** ICRA 2026; arXiv 최초 공개 2025
- **DOI:** [10.48550/arXiv.2509.18455](https://doi.org/10.48550/arXiv.2509.18455) — arXiv DOI; 정식 출판 DOI 미확인
- **원문 / 공식 자료:** [GD2P](https://arxiv.org/html/2509.18455v4)
- **방법:** 기하 조건부 diffusion hand pose 생성; RL 아님
- **우리 연구와의 연결:** 손 전체의 접촉 후보, 기하 최적화와 physics rollout의 실제 동작 성공 검증
- **적용 시 구분할 점:** Diffusion 대상은 hand pose. 주된 실제 실행은 open-loop이며 tactile/FT 적응 정책이나 정밀 rotation 학습이 아니다.

#### B02. TaskDexGrasp

- **정식 제목:** Task-Oriented Dexterous Hand Pose Synthesis Using Differentiable Grasp Wrench Boundary Estimator
- **발표 정보:** IROS 2024; arXiv 최초 공개 2023
- **DOI:** [10.1109/IROS58592.2024.10802652](https://doi.org/10.1109/IROS58592.2024.10802652) — 정식 출판 DOI
- **원문 / 공식 자료:** [TaskDexGrasp](https://arxiv.org/html/2309.13586v3)
- **방법:** Task-oriented hand pose의 미분 가능한 최적화; RL 아님
- **우리 연구와의 연결:** Task Wrench Space와 Grasp Wrench Space의 관계로 목표 힘·토크 방향의 생성 능력 평가
- **적용 시 구분할 점:** TWS는 사전 지정하는 task prior다. 회전각·거리만으로 필요한 wrench가 유일하게 결정되지 않으며, 정적 접촉 능력과 실제 동작 성공을 구분한다.

#### B03. RL critic으로 초기 grasp 선택

- **정식 제목:** Composing Dextrous Grasping and In-hand Manipulation via Scoring with a Reinforcement Learning Critic
- **발표 정보:** ICRA 2025
- **DOI:** [10.1109/ICRA55743.2025.11127792](https://doi.org/10.1109/ICRA55743.2025.11127792) — 정식 출판 DOI
- **원문 / 공식 자료:** [RL critic으로 초기 grasp 선택](https://arxiv.org/abs/2505.13253)
- **방법:** 학습된 in-hand RL critic으로 초기 grasp 후보 선택
- **우리 연구와의 연결:** Approach 종료 상태를 후속 Rotation 또는 Rotation→Push 정책의 수행 가능성으로 평가하는 발상
- **적용 시 구분할 점:** 원 논문은 후보 선택 연구다. Approach reward로 사용하는 것은 우리의 확장이다. Sparse-success value도 일반적으로 보정된 성공확률이 아니며, 선반 지지 nonprehensile 상황으로의 전이는 검증이 필요하다.

#### B04. GraspXL

- **정식 제목:** GraspXL: Generating Grasping Motions for Diverse Objects at Scale
- **발표 정보:** ECCV 2024; Springer 인용 연도 2025, 온라인 공개 2024
- **DOI:** [10.1007/978-3-031-73347-5_22](https://doi.org/10.1007/978-3-031-73347-5_22) — 정식 출판 DOI
- **원문 / 공식 자료:** [GraspXL](https://arxiv.org/html/2403.19649v2)
- **방법:** 목표 조건부 RL grasp-motion synthesis
- **우리 연구와의 연결:** 접근 방향, wrist rotation, hand position, graspable region을 접촉·안정성 목표와 결합
- **적용 시 구분할 점:** 목표가 reward와 wrist control guidance에 모두 반영된다. 모든 효과를 reward만의 결과로 해석하지 않는다.

#### B05. UniDexFPM

- **정식 제목:** Dexterous Functional Pre-Grasp Manipulation with Diffusion Policy
- **발표 정보:** arXiv 2024; 이전 제목에 UniDexFPM 사용
- **DOI:** [10.48550/arXiv.2403.12421](https://doi.org/10.48550/arXiv.2403.12421) — arXiv DOI; 정식 출판 DOI 미확인
- **원문 / 공식 자료:** [UniDexFPM](https://arxiv.org/html/2403.12421v2)
- **방법:** RL expert 학습과 diffusion policy distillation
- **우리 연구와의 연결:** 위치·방향·손가락 구성의 목표 불균형을 완화하는 mutual reward
- **적용 시 구분할 점:** 목표 functional grasp 자체는 주어진다. Contact distance로 표현한 항은 finger joint angle 오차이며 tactile 측정값이 아니다. 원문의 10.1109/LRA.2024.xxxxxx는 placeholder다. 순차 phase의 모든 목표를 동시에 충족시키도록 무조건 min gate를 적용하지 않는다.

#### B06. HANDFUL

- **정식 제목:** HANDFUL: Sequential Grasp-Conditioned Dexterous Manipulation with Resource Awareness
- **발표 정보:** arXiv 2026
- **DOI:** [10.48550/arXiv.2604.25126](https://doi.org/10.48550/arXiv.2604.25126) — arXiv DOI
- **원문 / 공식 자료:** [HANDFUL](https://arxiv.org/html/2604.25126)
- **방법:** 손가락별 접촉 보상과 순차 RL curriculum
- **우리 연구와의 연결:** 현재 접촉만 최대화하지 않고 후속 동작에 사용할 손가락·운동 여유 보존
- **적용 시 구분할 점:** 첫 물체를 잡은 채 다른 물체를 조작하는 설정이다. 손가락 집합을 지정·비교하며, 실제 시연은 성공 trajectory의 open-loop 재생이다.

#### B07. ExDex

- **정식 제목:** Dexterous Non-Prehensile Manipulation for Ungraspable Object via Extrinsic Dexterity
- **발표 정보:** arXiv 2025
- **DOI:** [10.48550/arXiv.2503.23120](https://doi.org/10.48550/arXiv.2503.23120) — arXiv DOI; 정식 출판 DOI 미확인
- **원문 / 공식 자료:** [ExDex](https://arxiv.org/html/2503.23120v1)
- **방법:** RL 기반 arm–hand nonprehensile manipulation
- **우리 연구와의 연결:** Push 후 edge/wall을 활용한 grasp, phase 연결과 종료 상태 분포 재사용
- **적용 시 구분할 점:** Tactile 기반 연구로 분류하지 않는다. 관측 표기의 F는 fingertip pose이며 force sensor를 뜻하지 않는다.

#### B08. Sequential Dexterity

- **정식 제목:** Sequential Dexterity: Chaining Dexterous Policies for Long-Horizon Manipulation
- **발표 정보:** CoRL 2023
- **DOI:** [10.48550/arXiv.2309.00987](https://doi.org/10.48550/arXiv.2309.00987) — arXiv DOI; 정식 proceedings DOI 미확인
- **원문 / 공식 자료:** [Sequential Dexterity](https://sequential-dexterity.github.io/)
- **방법:** RL skill chaining
- **우리 연구와의 연결:** 이전 정책의 종료 상태와 다음 정책의 실행 가능성을 연결하는 전환 적합성
- **적용 시 구분할 점:** 우리 접촉 상태·상위 명령 인터페이스에 맞는 transition criterion을 별도로 정의해야 한다.

#### B09. DexTouch

- **정식 제목:** DexTouch: Learning to Seek and Manipulate Objects with Tactile Dexterity
- **발표 정보:** IEEE RA-L, 2024 온라인 출판
- **DOI:** [10.1109/LRA.2024.3478571](https://doi.org/10.1109/LRA.2024.3478571) — 정식 출판 DOI
- **원문 / 공식 자료:** [DexTouch](https://arxiv.org/html/2401.12496v2)
- **방법:** Tactile RL와 sim-to-real
- **우리 연구와의 연결:** 접촉 탐색·조작, tactile observation과 sensor ablation, UR5e 기반 실물 시스템
- **적용 시 구분할 점:** Allegro hand의 FSR 이진 접촉 입력을 사용한다. 고해상도 압력 분포나 continuous Vision을 사용하는 우리 설정과 구분한다.

#### B10. Tactile Pushing

- **정식 제목:** Sim-to-Real Model-Based and Model-Free Deep Reinforcement Learning for Tactile Pushing
- **발표 정보:** IEEE RA-L 2023
- **DOI:** [10.1109/LRA.2023.3295236](https://doi.org/10.1109/LRA.2023.3295236) — 정식 출판 DOI
- **원문 / 공식 자료:** [Tactile Pushing](https://arxiv.org/abs/2307.14272)
- **방법:** 촉각 기반 goal-conditioned model-based / model-free RL
- **우리 연구와의 연결:** Push 실행 보상, 외란 대응, tactile 기반 sim-to-real 비교
- **적용 시 구분할 점:** 단일 tactile pusher이며 다지 hand configuration 결정 문제와 다르다.

#### B11. Bi-Touch

- **정식 제목:** Bi-Touch: Bimanual Tactile Manipulation with Sim-to-Real Deep Reinforcement Learning
- **발표 정보:** IEEE RA-L 2023
- **DOI:** [10.1109/LRA.2023.3295991](https://doi.org/10.1109/LRA.2023.3295991) — 정식 출판 DOI
- **원문 / 공식 자료:** [Bi-Touch](https://sites.google.com/view/bi-touch/)
- **방법:** 양팔 tactile RL
- **우리 연구와의 연결:** Pushing·reorientation의 접촉 유지와 목표 갱신, sim-to-real reward 문제
- **적용 시 구분할 점:** 두 tactile pusher를 사용하는 bimanual 시스템이다. 단일 다지 hand와 같은 실험 조건으로 취급하지 않는다.

#### B12. Visuotactile Estimation and Control

- **정식 제목:** Learning Visuotactile Estimation and Control for Non-prehensile Manipulation under Occlusions
- **발표 정보:** CoRL 2024; PMLR 2025
- **DOI:** [10.48550/arXiv.2412.13157](https://doi.org/10.48550/arXiv.2412.13157) — arXiv DOI; 정식 proceedings DOI 미확인
- **원문 / 공식 자료:** [Visuotactile Estimation and Control](https://proceedings.mlr.press/v270/ferrandis25a.html)
- **방법:** Visuotactile estimator + uncertainty-aware RL
- **우리 연구와의 연결:** Pose 추정 불확실성과 contact feedback을 정책 학습에 연결
- **적용 시 구분할 점:** 접촉 force 기반 추정과 다지 손가락의 tactile 배열을 동일한 sensing 설정으로 취급하지 않는다.

#### B13. Coarse-to-Fine Pushing

- **정식 제목:** Coarse-to-Fine Robotic Pushing Using Touch, Vision and Proprioception
- **발표 정보:** IEEE RA-L 2025; 온라인 공개 2024
- **DOI:** [10.1109/LRA.2024.3511378](https://doi.org/10.1109/LRA.2024.3511378) — 정식 출판 DOI
- **원문 / 공식 자료:** [Coarse-to-Fine Pushing](https://research-information.bris.ac.uk/en/publications/coarse-to-fine-robotic-pushing-using-touch-vision-and-propriocept/)
- **방법:** Vision·touch·proprioception 기반 coarse-to-fine pushing
- **우리 연구와의 연결:** 시각의 전역 위치 정보와 촉각의 국소 조절 역할 분담
- **적용 시 구분할 점:** RL reward 논문으로 단정하지 않는다. 구체 학습·제어 구조는 추가 정독 대상이다.

#### B14. Force Push

- **정식 제목:** Force Push: Robust Single-Point Pushing with Force Feedback
- **발표 정보:** IEEE RA-L 2024
- **DOI:** [10.1109/LRA.2024.3414180](https://doi.org/10.1109/LRA.2024.3414180) — 정식 출판 DOI
- **원문 / 공식 자료:** [Force Push](https://arxiv.org/abs/2401.17517)
- **방법:** Force-feedback / admittance controller; RL 아님
- **우리 연구와의 연결:** 물성·pose 불확실성 아래 force로 pushing 방향·속도를 보정하는 control baseline
- **적용 시 구분할 점:** Quasistatic planar single-point pushing의 가정을 확인하고 다지 hand와의 조건 차이를 반영해야 한다.

#### B15. RoboPack

- **정식 제목:** RoboPack: Learning Tactile-Informed Dynamics Models for Dense Packing
- **발표 정보:** RSS 2024
- **DOI:** [10.15607/RSS.2024.XX.130](https://doi.org/10.15607/RSS.2024.XX.130) — 정식 출판 DOI
- **원문 / 공식 자료:** [RoboPack](https://www.roboticsproceedings.org/rss20/p130.html)
- **방법:** Visuotactile recurrent dynamics model + MPC; RL 정책 학습 아님
- **우리 연구와의 연결:** 접촉 이력으로 latent physics를 추정하고 미래 물체 운동을 예측
- **적용 시 구분할 점:** Geometry 관측 가능성과 물성 관측 가능성이 다르다는 점, observation/history 설계의 참고다.

#### B16. Nonprehensile Pregrasp

- **정식 제목:** Synthesize Dexterous Nonprehensile Pregrasp for Ungraspable Objects
- **발표 정보:** ACM SIGGRAPH Conference Proceedings 2023
- **DOI:** [10.1145/3588432.3591528](https://doi.org/10.1145/3588432.3591528) — 정식 출판 DOI
- **원문 / 공식 자료:** [Nonprehensile Pregrasp](https://arxiv.org/abs/2305.04654)
- **방법:** Graph search·optimal control·학습된 graspability; RL 아님
- **우리 연구와의 연결:** 후속 grasp를 가능하게 만드는 사전 조작과 환경 제약 고려
- **적용 시 구분할 점:** 뒤쪽 target의 clearance 확보와 조작 중인 물체 자체의 graspability 향상은 구분한다.

#### B17. RetrDex

- **정식 제목:** RetrDex: Efficient Object Retrieval in Cluttered Scenes with a Dexterous Hand
- **발표 정보:** IROS 2026 accepted; arXiv 최초 공개 2025
- **DOI:** [10.48550/arXiv.2502.18423](https://doi.org/10.48550/arXiv.2502.18423) — arXiv DOI; 정식 출판 DOI 미확인
- **원문 / 공식 자료:** [RetrDex](https://arxiv.org/abs/2502.18423)
- **방법:** Clutter clearing / retrieval RL
- **우리 연구와의 연결:** Blocker 조작으로 target 접근성을 높이는 시스템 목적과 2단계 평가
- **적용 시 구분할 점:** 이전 RetrievalDexterity 제목과 중복 등재하지 않는다. Tactile 활용 연구로 단정하지 않는다.

#### B18. Differentiable Force Closure Estimator

- **정식 제목:** Synthesizing Diverse and Physically Stable Grasps with Arbitrary Hand Structures using Differentiable Force Closure Estimator
- **발표 정보:** IEEE RA-L 2022; 온라인 공개 2021
- **DOI:** [10.1109/LRA.2021.3129138](https://doi.org/10.1109/LRA.2021.3129138) — 정식 출판 DOI
- **원문 / 공식 자료:** [Differentiable Force Closure Estimator](https://arxiv.org/abs/2104.09194)
- **방법:** 미분 가능한 force-closure surrogate 기반 grasp 합성
- **우리 연구와의 연결:** GD2P의 E_fc 배경과 surrogate 가정 파악
- **적용 시 구분할 점:** 작은 surrogate와 정확한 frictional force closure의 관계를 확인한다. [저자 erratum](https://yzhu.io/publication/grasp2021ral/erratum.pdf)도 함께 참고한다.

#### B19. DexGraspNet

- **정식 제목:** DexGraspNet: A Large-Scale Robotic Dexterous Grasp Dataset for General Objects Based on Simulation
- **발표 정보:** ICRA 2023; arXiv 최초 공개 2022
- **DOI:** [10.48550/arXiv.2210.02697](https://doi.org/10.48550/arXiv.2210.02697) — arXiv DOI; 앞선 검증에서 정식 출판 DOI를 1차 출처로 확정하지 못함
- **원문 / 공식 자료:** [DexGraspNet](https://pku-epic.github.io/DexGraspNet/)
- **방법:** Grasp pose 최적화·시뮬레이션 검증·대규모 데이터 생성
- **우리 연구와의 연결:** Distance·joint-limit·penetration 항과 physics validation의 배경
- **적용 시 구분할 점:** 주 대상은 grasp synthesis다. Stable grasp와 Rotation/Push용 접촉 적합성을 동일시하지 않는다.

#### B20. Get a Grip

- **정식 제목:** Get a Grip: Multi-Finger Grasp Evaluation at Scale Enables Robust Sim-to-Real Transfer
- **발표 정보:** CoRL 2024
- **DOI:** [10.48550/arXiv.2410.23701](https://doi.org/10.48550/arXiv.2410.23701) — arXiv DOI; 정식 proceedings DOI 미확인
- **원문 / 공식 자료:** [Get a Grip](https://arxiv.org/abs/2410.23701)
- **방법:** 성공·실패 grasp 데이터 기반 evaluator 학습
- **우리 연구와의 연결:** 기하 surrogate와 실제 성공 평가를 분리하고 후보를 선택하는 설계
- **적용 시 구분할 점:** Grasp 성공 evaluator를 우리의 후속 조작 성공 evaluator로 그대로 대체하지 않는다.

---

### 15.3 후속 baseline 탐색 자료 — 2026-09-15 정리

아래는 Stage 8–10의 대화에서 검토한 추가 자료다. 적합성 평가는 Agent의 해석이며, baseline 채택·학습 재현·성능 검증을 뜻하지 않는다. 기존 B01–B20의 ID는 유지한다. 출판사 DOI를 새로 확인하지 않은 항목은 원문·공식 프로젝트 링크만 남기며 DOI 부재로 단정하지 않는다.

#### B21. Learning Contact Locations for Pushing and Orienting Unknown Objects

- **발표 정보:** Tucker Hermans, Fuxin Li, James M. Rehg, Aaron F. Bobick; Humanoids 2013
- **원문:** [저자 공개 논문](https://users.cs.utah.edu/~thermans/papers/hermans-ichr2013.pdf)
- **방법:** 국소·전체 형상 특징에서 직선 밀기 안정성과 회전 효과를 각각 회귀 모델로 예측하고 접촉 위치를 선택한다.
- **직접 연결점:** 서론에서 안정적인 pushing 접촉점이 목표까지의 직선 방향과 정렬되도록 물체를 먼저 회전시키고 이후 미는 전략을 명시한다. `밀기를 위한 준비 회전` 자체를 우리의 최초 기여로 주장하지 않는다.
- **한계:** 고정 gripper 동작과 접촉 위치 선택 중심이며 다지 손 configuration 학습은 아니다. 정량 실험은 직선·회전 접촉 위치 예측 각각이 중심이므로 전체 준비→밀기 성능이 같은 수준으로 검증되었다고 확대하지 않는다.

#### B22. DexMove

- **정식 제목 / 발표:** DexMove: Learning Tactile-Guided Non-Prehensile Manipulation with Dexterous Hands; ICLR 2026
- **원문 / 공식 자료:** [프로젝트](https://peilin-666.github.io/projects/DexMove/), [OpenReview](https://openreview.net/forum?id=dT3ZciXvNX)
- **방법:** 물리적으로 검증한 손목–손가락 궤적과 인간 촉각 시연을 활용하여 flow-matching 기반 비파지 조작 정책을 학습한다.
- **연결점 / 주의:** 촉각 기반 wrist–finger 공동 제어에 가까운 선행 연구다. 촉각을 추가한다는 사실만으로 novelty를 주장하지 않는다. RL·우리 sensor 구성·shelf 과업과 동일하지 않다. 대화에서는 공식 프로젝트와 공개 논문 설명을 검토했으며 정책 학습 코드의 공개·재현은 확인하지 못했다.

#### B23. Task-Oriented Contact Optimization for Pushing Manipulation with Mobile Robots

- **발표 정보:** Filippo Bertoncelli, Mario Selvaggio, Fabio Ruggiero, Lorenzo Sabattini; IROS 2022
- **자료:** [소속기관 공개 논문 정보](https://iris.unimore.it/handle/11380/1295974)
- **방법:** 주어진 평면 궤적을 수행하는 접촉력 크기를 줄이도록 접촉 위치를 최적화하고, 계산한 힘과 위치 feedback으로 추종한다.
- **연결점 / 주의:** 목표 운동에 따른 접촉 배치의 적합성 비교에 유용하다. 다중 mobile robot의 독립 접촉점을 손 관절의 운동학·충돌 제약이 있는 다지 손으로 그대로 치환할 수 없다. 공개 초록·논문 설명 기준의 검토이며 상세 최적화·재현은 후속 독해 대상이다.

#### B24. Tactile-Based Negotiation of Unknown Objects during Navigation in Unstructured Environments with Movable Obstacles

- **발표 정보:** Simon Armleder et al.; Advanced Intelligent Systems 2024
- **DOI:** [10.1002/aisy.202300621](https://doi.org/10.1002/aisy.202300621)
- **원문:** [소속기관 공개 전문](https://research.chalmers.se/publication/539508/file/539508_Fulltext.pdf)
- **방법:** 장애물을 먼저 회전시킨 뒤 밀어 경로를 확보한다. 방향별 접촉 위치를 선택하고 robot skin의 근접·힘 신호로 표면 정렬과 순응 제어를 수행한다.
- **연결점 / 주의:** 준비 회전·접촉 위치 선택·접촉 중 손 정렬의 사례다. 큰 장애물의 mobile manipulation이며 다지 손 policy 학습이 아니다. 물체 운동·질량 분포 등에 단순화된 가정이 있고, 센서 구성을 우리의 wrist F/T·tactile과 동일시하지 않는다.

#### B25. CORN

- **정식 제목 / 발표:** CORN: Contact-based Object Representation for Nonprehensile Manipulation of General Unseen Objects; ICLR 2024
- **원문 / 코드:** [논문](https://arxiv.org/html/2403.10760), [저장소](https://github.com/iMSquared/corn)
- **방법 / 연결점:** 접촉 예측으로 사전학습한 기하 표현을 goal-conditioned 비파지 RL과 teacher–student 학습에 사용한다.
- **주의:** Gripper 설정이며 `contact-based`가 실제 tactile sensor 입력을 뜻하지 않는다. 물체 수준 목표 제어의 보조 후보이지 다지 손 configuration의 직접 baseline은 아니다.

#### B26. DyWA

- **정식 제목 / 발표:** DyWA: Dynamics-adaptive World Action Model for Generalizable Non-prehensile Manipulation; ICCV 2025
- **원문 / 코드:** [논문](https://arxiv.org/html/2503.16806), [저장소](https://github.com/jiangranlv/DyWA)
- **방법 / 연결점:** 관측·행동 이력의 적응 표현과 상태 예측을 이용한 비파지 조작. 물성 변화 대응의 보조 후보다.
- **주의:** Gripper 설정이며 다지 손·촉각 방법으로 분류하지 않는다. 목표 물체 자세의 준비와 손 접촉 구성을 함께 다루는 직접 baseline으로 확정하지 않는다.

#### B27. VTDexManip

- **정식 제목 / 발표:** VTDexManip: A Dataset and Benchmark for Visual-tactile Pretraining and Dexterous Manipulation with Reinforcement Learning; ICLR 2025
- **공식 자료 / 코드:** [프로젝트](https://lqts.github.io/VTDexManip/), [저장소](https://github.com/LQTS/VTDexManip)
- **방법 / 연결점:** Visual–tactile 표현 사전학습과 RL을 결합하며 tabletop reorientation 등의 과업을 포함한다.
- **주의:** Sensor 표현·융합 방식의 참고이며, 우리 Rotation→Push 전체 과업이나 wrist F/T 설정과 동일하지 않다.

---

## 16. Change Log

### 2026-09-15 — 사용자 검토를 반영한 최소 MLP Observation 구체화

- 문서 갱신보다 분석·논의를 먼저 진행하고, 사용자 annotation별 검토 결과를 Stage 13과 5.6.1.6에 통합했다.
- Phase ID를 actor에 주지 않고 reward gate로 long-horizon sequence를 학습하는 방향을 반영했다.
- Goal을 EEF-frame target position과 preparatory object orientation으로 분리해 primary pushing 목적과 준비 회전의 의미를 명확히 했다.
- Unseen object·occlusion을 고려해 point cloud/mesh 대신 episode-consistent OBB extent를 actor geometry로 사용하도록 수정했다.
- 실제 17 tactile sensor와 URDF link/pad를 공통 `M`개 region으로 pooling하고 17-channel 대 coarse binary를 ablation하도록 정리했다. Taxel별 collision body는 연산량 때문에 기본안에서 제외했다.
- Binary tactile+wrist F/T를 actor 기본 조합으로, continuous simulation contact force를 reward·critic privileged signal로 구분했다.
- MLP history를 tactile·wrench·action의 observation block으로 명시하고 modality별 `K_b/K_w/K_a`와 차원식을 추가했다.
- Phase, velocity, 다수 중복 kinematic feature와 raw/dense perception을 현재 최소 observation에서 제외했다.

### 2026-09-15 — 선행연구와 Isaac Lab 구현 범위를 연결한 Observation 초안

- 각 observation 후보를 문헌의 실제 입력·가공, Isaac Lab 원천 데이터, policy 표현, 실물 대응과 한계로 연결하는 형식을 도입했다.
- DexTouch, Rotating without Seeing, VTDexManip, Robot Synesthesia, Visuotactile Estimation and Control과 DexMove의 observation 구성을 비교했다.
- Structured goal·pose·OBB, proprioception·kinematics, filtered wrist wrench, binary tactile, previous action과 validity/age로 이루어진 최소 vector baseline을 Agent 추천안으로 정리했다.
- ContactSensor binary의 hysteresis·debounce·randomization, vision perception proxy, F/T frame·bias 보정과 multi-rate history 처리 방안을 기록했다.
- Geometry point cloud, spatial tactile, continuous force, raw RGB와 TacSL/TacEx optical tactile를 확장·ablation으로 분리했다.
- Isaac Lab/Isaac Sim 버전 및 RH56E2 tactile variant·실제 packet 확인을 구현 전 blocker로 추가했다.

### 2026-09-15 — 전체 Phase Reward 설계를 위한 정보·Action 명세 우선

- Baseline 독해를 단독 최우선으로 두던 순서를 변경하고, policy observation과 privileged information을 먼저 구분한 뒤 전체 phase reward를 설계하기로 했다.
- Action은 EEF frame의 delta pose와 hand joint action으로 구성하는 방향을 기록했다. 세부 action semantics와 controller interface는 미결로 남겼다.
- 각 reward term에 task·물리·안전·failure mode 또는 문헌 근거를 연결하는 원칙을 추가했다.
- 기존 baseline·GD2P·Approach reward 분석은 폐기하지 않고 reward 근거와 비교 실험을 위한 병행 자료로 보존했다.

### 2026-09-15 — Baseline 우선 검토와 1단계 contribution·목표 선택 경계 명확화

- Reward formulation보다 baseline의 전체 해결 방식을 먼저 검토하도록 현재 우선순위를 수정했다. 이전 GD2P energy·Approach reward 분석과 당시 읽기 순서는 이력으로 보존했다.
- 목표 방향의 pushing을 위한 준비 조작이라는 사용자 관점을 반영하고, contribution을 Approach에만 한정하는 해석을 정정했다.
- 1단계에서 적합한 목표 물체 자세를 선택하는 주체는 상위 모듈이며, policy는 주어진 회전·병진 목표에 맞춰 손 구성과 접촉 상태를 형성·전환·보정한다는 경계를 재확인했다.
- 합의된 contribution 후보 표현과 아직 검증되지 않은 method novelty·성능·비교 실험을 분리했다.
- Stage 8–10에 baseline 추천의 변경 과정, Agent의 모호한 표현, 사용자의 지적과 최종 확인을 기록했다.
- 현재 요약·대체 관계·결정 레지스터·backlog·후속 Agent 지침을 함께 갱신하고 B21–B27 및 후보별 역할·한계를 추가했다.
- `research_topic.md`에도 현재 역할 경계와 후보 framing을 반영했다. Track A, Track B 센서 조건, 기존 2단계 확장과 미결 설계는 유지했다.

### 2026-09-14 — Track B 1단계 초점·GD2P 분석·DOI 참고문헌 보강

- Track B의 1단계 low-level policy와 Approach hand configuration reward를 현재 우선 작업으로 명시했다.
- GD2P의 생성 대상·학습 방식·실행 방식에 대한 정정, energy 6항의 의미와 공개 코드 차이를 기록했다.
- Wrench-space 평가와 downstream execution value를 후속 동작 적합성의 작업 가설로 구분했다.
- 관련 논문 17편과 배경 논문 3편의 DOI·출판 상태·방법·활용점·한계를 추가했다.
- 현재 요약·우선 읽기 순서·Stage 7·대체 관계·결정 레지스터·미결 목록을 함께 갱신했다.
- 문서와 후속 사용자 수정이 충돌하면 사용자의 최신 설명을 우선하도록 문서 해석 순서를 바로잡았다.
- 기존 Track A, Track B의 2단계 확장 방향, 이전 의사결정 기록은 보존했다.

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
