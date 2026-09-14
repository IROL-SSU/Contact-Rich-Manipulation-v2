# JH.Jeong Research Topic

> **연구 주제:** Shelf retrieval을 위한 goal-conditioned blocker-object contact manipulation
>
> **현재 중심 Track:** Track B
>
> **최종 갱신:** 2026-09-15

---

## 1. 문서 목적

이 문서는 연구 과정에서 **구체화되었거나 확인된 현재 내용**을 간결하게 정리한다. 논의의 흐름, 과거 제안, 작업 가설과 미결 사항의 상세한 기록은 [`context.md`](./context.md)에서 관리한다.

새로운 아이디어나 설계 후보는 합의되기 전까지 이 문서의 확정 내용으로 추가하지 않는다. 중요한 결정이 변경되면 `context.md`에 변경 배경과 의사결정 과정을 먼저 기록하고, 이 문서를 최신 결론에 맞게 갱신한다.

---

## 2. 연구 주제

선반 환경에서 target object의 관측이나 인출을 방해하는 blocker object를 재배치하여, target에 접근할 수 있는 공간 또는 경로를 확보한다.

연구의 중심은 continuous Vision으로 관측한 blocker의 현재 pose와 근사 geometry, 그리고 F/T·Tactile contact feedback을 이용하는 RL manipulation policy다. 먼저 주어진 회전·병진 목표를 접촉 불확실성 아래에서 안정적으로 실행하는 능력을 학습하고, 이후 공간 확보 목적에 필요한 조작 목표와 동작 전환을 정책이 결정하도록 담당 범위를 확장한다.

현재 조작 관점은 **다양한 초기 물체 자세에서, 목표 방향의 pushing을 위해 물체를 주어진 자세로 돌려두고 미는 것**이다. 회전 자체만이 목적이 아니라 후속 밀기를 위한 준비라는 점이 중요하다. 단, 1단계에서 어떤 물체 자세가 적합한지 선택하는 것은 상위 모듈의 역할이며, 우리 policy는 그 목표를 실행하기 위한 hand configuration과 접촉 상태를 형성·조절한다.

이를 한 문장으로 정리하면 다음과 같다.

> **지속적인 시각·근사 기하 관측과 접촉 감각을 이용해 blocker의 목표 조건부 회전·병진 조작을 학습하고, 이를 target 접근 공간 확보를 위한 조작 의사결정과 실행의 통합으로 확장한다.**

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

| 정보 | 현재 역할 |
| --- | --- |
| Current blocker pose | 회전·병진 goal error와 조작 진행 상태 확인 |
| Approximated geometry | 접근 방향과 초기 hand configuration 준비에 사용할 근사 외형·점유 정보 제공 |
| Tactile feedback | 실제 접촉 위치와 분포를 참고하여 손 구성과 접촉 배치 보정 |
| Wrist F/T feedback | 손에 전달되는 합력과 모멘트를 참고하여 전체 접촉 부하 조절 |
| Robot/hand state | 기본 관측 후보이며 정확한 관측 항목은 아직 확정하지 않음 |

Continuous Vision을 사용하더라도 실제 국소 접촉 표면, 마찰, 질량 분포 등 전체 물리 상태를 정확히 안다고 가정하지 않는다. 접촉 전에는 근사 geometry로 hand configuration을 준비하고, 접촉 후에는 F/T·Tactile feedback으로 실제 상호작용에 맞게 보정한다.

손목의 6자유도와 손가락의 제어 자유도는 별개의 대상으로 구분한다. 또한 joint state를 관측하는 것과 해당 joint를 policy action으로 직접 제어하는 것도 구분한다.

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
- 세 가지 동작 구분을 세 개의 독립 network 또는 policy가 필요하다는 뜻으로 해석하지 않는다.
- 특정 시리얼 박스, hooking 동작, 동일한 회전각과 선반 구성은 설명용 예시이지 전체 학습 명세가 아니다.
- 현재 방향은 연구 문제와 진행 범위를 정의한 것이며, 최종 Method novelty나 논문 Contribution이 확정된 것은 아니다.

---

## 9. 아직 확정 명세로 취급하지 않는 항목

다음 항목은 추가 논의와 실험을 통해 구체화한 뒤 이 문서에 반영한다.

- Goal, observation과 action의 정확한 표현
- 상위가 지정한 pushing 준비 orientation을 Push 중·종료 시 유지할 허용 오차와 별도 최종 orientation 조건 여부
- Phase 전환, 완료와 실패 조건
- Reward 수식과 학습 curriculum
- Policy 및 network의 개수와 계층 구조
- Sensor fusion, history와 memory 구조
- Geometry와 perception error의 표현 및 분포
- Sim-to-Real 범위와 sensor calibration
- 공간 확보의 정량적 success metric과 비교 baseline
- 최종 Method novelty, Contribution과 논문화 범위

---

## 10. 현재 Contribution 후보와 문헌 검토 우선순위

**2026-09-15 확인한 framing:** 다음 표현은 사용자가 합리적이라고 확인한 1단계 contribution 후보다. 역할 경계와 연구 관점에 대한 합의이며, 새로운 method나 성능 우위가 입증되었다는 뜻은 아니다.

> **주어진 물체 회전·병진 목표를 수행하기 위해, 접근부터 회전과 밀기까지 다지 손의 접촉 구성을 형성·전환하고 접촉 피드백으로 보정하는 조작 방법.**

접촉 준비의 유효성은 후속 pushing의 성공과 연결해 검증한다. 이 방향을 어떤 접촉 표현·평가·학습·제어 방법으로 실현할지는 미결이며, Push controller 자체에도 별도의 novelty가 있어야 한다고 정한 것은 아니다. 중간 목표 물체 orientation의 자율 선택은 2단계 확장으로 남긴다.

**현재 작업 순서:** Reward formulation을 먼저 정하기보다 baseline의 전체 문제 정의와 해결 방식을 먼저 살펴본다. `회전 후 밀기`, 작업별 손 자세 합성, 촉각 기반 다지 손 제어, 후속 성공을 고려한 skill 연결에는 선행 연구가 있으므로, 이 요소들의 사용·결합만으로 신규성을 주장하지 않는다.

Agent가 제안한 우선 독해 후보는 다음과 같다. 사용자가 최종 실험 baseline을 선정한 것은 아니다.

- **GD2P:** 주어진 물체 상태·밀기 방향에 맞는 손 구성을 생성하고 실제 push 성공으로 검증하는 방법.
- **Hermans et al., 2013:** 안정적인 접촉 위치가 목표 밀기 방향과 정렬되도록 준비 회전을 사용하는 관점.
- **TaskDexGrasp:** 작업에 필요한 힘·모멘트를 가할 수 있는 hand configuration의 평가·합성 방법.

DexMove 등의 촉각 제어 연구와 Sequential Dexterity 등의 phase 연결 연구도 차별성 검토에 포함한다. 논문별 출처·한계, 단순 결합 baseline 등의 비교 후보와 판단 과정은 [`context.md`](./context.md)의 **5.11절·11절 Stage 8–10·15.2–15.3절**에 보존한다. 기존 reward 분석은 폐기하지 않고 후속 설계 후보로 유지한다.
