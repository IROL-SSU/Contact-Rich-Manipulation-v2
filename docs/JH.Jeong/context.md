# Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry

> **연구 정의:** **Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry**
>
> **Task category:** Nonprehensile manipulation
>
> **현재 단계:** Track B Stage 1 — goal-conditioned low-level policy
>
> **문서 역할:** 연구 범위, 가설, 합의된 작업 기준과 미결 사항을 분리하고 이후 판단의 기준을 제공한다.
>
> **현재 합의 범위:** [`Intro/`](./Intro/README.md)의 문제 정의·스토리라인과 [`policy_learning.md`](./policy_learning.md)의 actor observation까지
>
> **협의 중:** Policy architecture, action/controller, reward, termination, critic·RL 세부 방법, evaluation metric·protocol과 experiment/ablation 설계. 이 항목들은 현재 계획이나 baseline으로 간주하지 않는다.
>
> **최종 갱신:** 2026-09-24

---

## 1. Project Overview

### 1.1 이 문서의 우선순위

이 문서는 현재 연구 정의와 논의 상태를 통제하기 위한 기준 문서다. [`README.md`](./README.md), [`Intro/`](./Intro/README.md), [`research_topic.md`](./research_topic.md), [`policy_learning.md`](./policy_learning.md)와 [`papers/`](./papers/README.md)의 내용이 하나의 확정안처럼 서술되어 있더라도, **현재 합의된 범위는 Intro와 actor observation까지**다. Policy architecture와 action/controller를 비롯해 그 이후의 reward·termination·critic·evaluation·experiment 관련 내용은 모두 `[Open]`인 논의 기록이며, 최신 판단은 이 문서를 따른다.

상태 표기는 다음 의미로만 사용한다.

| Status | 의미 |
| --- | --- |
| `[Fixed]` | 현재 연구가 유지되는 동안 쉽게 바꾸지 않을 문제 범위와 시스템 경계 |
| `[Hypothesis]` | 실험으로 지지되거나 기각되어야 하는 주장 |
| `[Baseline]` | 합의 범위 안에서 현재 출발점으로 삼는 작업 기준이며 변경 가능 |
| `[Open]` | 근거·실험·사용자 판단이 더 필요한 사항 |
| `[Candidate]` | 향후 충분한 근거가 마련될 때만 contribution이 될 수 있는 항목 |
| `[Rejected]` | 검토했지만 현재 사용하지 않는 방향 또는 주장 |
| `[Superseded]` | 과거에는 사용했으나 최신 정의가 대체한 내용 |

`[Fixed]`는 연구적으로 참이라는 뜻이 아니라 **연구할 문제를 고정했다**는 뜻이다. `[Hypothesis]`와 `[Candidate]`는 논문 결과나 contribution처럼 서술하지 않는다.

### 1.2 Working research definition

> **Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry**

`[Fixed]` 선반 안의 target object에 접근할 공간을 만들기 위해 상위 모듈이 지정한 blocker를 **안정적으로 파지하거나 들어 올리지 않고** 조작하는 low-level nonprehensile manipulation을 연구한다. Approximate geometry 조건에서 tactile과 wrist F/T feedback으로 hand configuration 및 wrist–finger motion을 조정하며, Approach / Contact Formation → Rotation / Pivoting → Push / Translation을 수행한다.

`[Fixed]` 이 문서에서 `nonprehensile manipulation`은 **task category**이고, `contact-rich manipulation`은 별도의 task category가 아니라 접촉 위치·마찰·stick–slip–separation과 힘 전달의 불확실성을 다루는 **interaction/control challenge**를 뜻한다.

향후 연구는 아래 흐름으로 발전할 수 있다. 다만 Method 이후의 구체 순서와 experiment 구성은 아직 합의하지 않았다.

```text
Research Motivation
  → Research Trend와 조건부 Method 선택
  → Previous Works가 해결한 범위와 남은 Research Gap
  → 반증 가능한 Candidate Contributions / Hypotheses
  → 후보를 검증하는 Method
  → 통제된 Experiments / Ablations
  → 실험으로 지지된 Contribution
```

현재 단계에서는 마지막 contribution 문장이나 PPT 구성을 확정하지 않는다.

### 1.3 Broader motivation과 좁은 연구 문제

- `[Fixed]` 물류·가정 서비스 환경에는 pushing, pulling, pivoting처럼 안정적 파지 없이 물체를 이동시키는 nonprehensile manipulation이 필요하다.
- `[Fixed]` Prehensile grasp-and-lift 또는 retrieval은 흔히 feasible grasp, finger-placement space와 lifting clearance를 요구한다. Blocker를 짧게 옮기거나 grasp 전에 orientation·accessible face만 바꾸면 되는 제한 공간에서는 pushing·sliding·pivoting이 더 직접적일 수 있다.
- `[Fixed]` 이 과업은 물체 및 환경과의 접촉을 동반하며, 접촉 위치, 국소 형상, 마찰, 질량 분포, stick–slip–separation과 힘 전달의 불확실성 때문에 contact-rich interaction/control challenge가 된다.
- `[Fixed]` 본 연구는 heuristic, optimization, RL, IL 또는 VLA 전체의 우열을 논하지 않는다. 이들이 이미 해결한 범위를 인정한 뒤, coarse geometry와 배포 가능한 접촉 sensing 아래의 blocker manipulation에 질문을 한정한다.
- `[Fixed]` tactile 또는 wrist F/T를 사용한다는 사실 자체는 novelty가 아니다. 연구 질문은 이 정보가 **어떤 불확실성을 줄이고 어떤 후속 조작 성능을 변화시키는가**이다.

---

## 2. Fixed Research Scope

### 2.1 유지할 연구 범위

| Item | Fixed definition | Status |
| --- | --- | --- |
| Application | 선반 뒤쪽 target에 접근할 공간을 만들기 위한 앞쪽 blocker manipulation | `[Fixed]` |
| Task category | 안정적 grasp/lift 없이 pushing·pulling·pivoting으로 blocker를 재배치하는 nonprehensile manipulation | `[Fixed]` |
| Interaction/control challenge | 접촉 위치·마찰·stick–slip–separation·힘 전달이 불확실한 contact-rich interaction | `[Fixed]` |
| Manipulated object | 현재 low-level policy가 직접 조작하는 대상은 blocker | `[Fixed]` |
| Planning boundary | Blocker 선택과 전역 작업 순서 결정은 상위 모듈의 역할 | `[Fixed]` |
| Current research stage | Track B Stage 1의 low-level execution policy | `[Fixed]` |
| Task structure | Approach / Contact Formation, Rotation / Pivoting, Push / Translation의 연결된 조작 | `[Fixed]` |
| Reason for rotation | 이후 Push에 적합한 object–hand relation, object orientation 또는 contact face를 형성하기 위한 preparatory manipulation | `[Fixed]` |
| Geometry regime | 완전한 국소 접촉 형상을 actor가 안다고 가정하지 않는 approximate-geometry regime | `[Fixed]` |
| Information roles | Vision/coarse geometry는 전역 task·접근 정보를 제공하고, tactile·wrist F/T는 실제 접촉 상태와 geometry approximation error를 보완하는 closed-loop feedback으로 사용 | `[Fixed]` |
| Information boundary | 실물에서 얻을 수 없는 simulator state는 actor observation에서 제외하며, 향후 사용하더라도 actor 외부의 teacher information으로만 구분 | `[Fixed]` |
| Hardware platform | UR5e arm과 Inspire Robots RH56E2 hand를 현재 구현 플랫폼으로 사용 | `[Fixed]` |

Approach–Rotation–Push는 세 개의 의미 있는 sub-objective를 나타낸다. 모든 episode에서 Rotation을 강제로 수행하거나 세 개의 독립 policy를 사용한다는 뜻은 아니다. 초기 상태가 이미 Push에 적합하면 Rotation은 작거나 생략될 수 있다.

### 2.2 현재 범위 밖

| Out-of-scope item | Boundary | Status |
| --- | --- | --- |
| Target retrieval 전체 | Target grasp·인출은 전체 application motivation과 가능한 후속 system stage이며 Stage 1 actor의 직접 동작이 아님 | `[Fixed]` |
| Multi-blocker ordering | 여러 blocker 중 선택하고 순서를 정하는 문제는 상위 planning 또는 후속 단계 | `[Fixed]` |
| Perception algorithm 자체 | Object tracking, marker detection, OBB estimation network의 성능 개선은 현재 method contribution이 아님 | `[Fixed]` |
| General-purpose VLA | Open-world language instruction과 embodiment-general policy는 현재 연구 범위가 아님 | `[Fixed]` |
| Exact contact reconstruction | 고해상도 tactile image에서 contact geometry를 복원하는 문제는 현재 범위가 아님 | `[Fixed]` |

### 2.3 과거 Track 구분의 위치

- `[Superseded]` Track A와 Track B의 비교를 현재 `context.md`의 최상위 구조로 사용하는 방식.
- `[Fixed]` 현재 구체화 대상은 Track B Stage 1이다. Track A는 별도의 연구 방향으로 남아 있지만 현재 method와 experiment의 범위에 포함하지 않는다.
- `[Rejected]` `Track A = sweeping`, `Track B = rotation`처럼 동작 종류만으로 연구축을 구분하는 해석.

---

## 3. Research Boundary and Assumptions

### 3.1 모듈별 역할

| Module | Responsibility | Not assumed | Status |
| --- | --- | --- | --- |
| Upper-level planner | Blocker와 low-level task goal을 제공 | Low-level nonprehensile execution을 직접 해결한다고 가정하지 않음 | `[Fixed]` |
| Perception | Object pose와 어떤 형태의 geometry information을 제공 | Marker가 geometry까지 제공한다고 간주하지 않음 | `[Fixed]` |
| Stage 1 policy | Wrist와 fingers를 제어하여 접촉을 형성·변경하고 회전·밀기 goal을 실행 | Blocker 선택이나 전체 retrieval 순서를 결정하지 않음 | `[Fixed]` |
| Simulator teacher | Exact pose, contact pair·force, collision, support, velocity 등을 계산 | 이 정보가 실물 actor에 직접 제공된다고 가정하지 않음 | `[Fixed]` |
| Hardware safety layer | Force, joint, collision limit를 policy 밖에서도 감시 | Learned policy만으로 모든 안전을 보장한다고 주장하지 않음 | `[Fixed]` |

### 3.2 Task goal interface

- `[Fixed]` 상위 모듈은 low-level policy가 blocker manipulation을 실행하기에 충분한 goal을 제공한다.
- `[Baseline]` 현재 goal encoding은 `selected OBB face + push direction + push distance/target position`이다. 필요한 rotation은 selected-face normal과 push direction의 관계에서 유도한다.
- `[Open]` 상위 모듈이 별도의 desired rotation/orientation을 명시적으로 제공해야 하는 조건이 있는지 검토한다. Non-box-like object, OBB symmetry와 face ambiguity가 이 판단에 영향을 줄 수 있다.
- `[Superseded]` 목표 object quaternion 하나를 모든 task goal의 확정 표현으로 간주한 이전안.

### 3.3 Pose와 geometry를 구분한다

Marker 또는 tracker가 제공하는 기본 정보는 object pose다. Geometry는 별도의 정보원과 정확도 정의가 필요하다.

| Geometry level | Definition | Example | Intended use |
| --- | --- | --- | --- |
| No shape representation | Pose 외에 shape·dimension 정보를 actor에 주지 않음 | Object origin pose only | 형상 정보가 없는 조건의 개념적 구분 |
| Implicit visual | RGB·depth·RGB-D image를 구조화된 형상으로 변환하지 않고 직접 encode | RGB/depth image encoder | 명시적 형상 표현과 구분되는 대안 |
| Estimated explicit geometry | 실제 접촉 표면과 오차가 있을 수 있는 구조화 형상 | Partial point cloud, basis point set, estimated OBB | 현재 주 연구 조건 |
| Exact model | Known CAD/mesh 또는 정확한 dimensions가 object pose와 일관되게 정합됨 | Registered mesh, calibrated dimensions | 정확한 형상을 가정하는 조건의 개념적 구분 |

- `[Fixed]` Simulation collision geometry가 정확하더라도 actor가 exact geometry를 관측하지 않으면 actor의 geometry condition은 approximate다.
- `[Baseline]` Approximate geometry의 첫 표현으로 episode-consistent OBB를 사용한다.
- `[Open]` OBB의 생성 방법과 center·orientation·extent error distribution은 아직 정하지 않았다.
- `[Rejected]` Marker pose가 있다는 이유만으로 object geometry도 exact하다고 표현하는 방식.

### 3.4 Vision/geometry와 contact sensing의 역할 분리

| Information source | Primary role | Boundary | Status |
| --- | --- | --- | --- |
| Vision / upper-level perception | Blocker pose, 전역 task context와 접근 관계 제공 | 실제 접촉의 발생·유지·미끄러짐을 직접 안다고 가정하지 않음 | `[Fixed]` |
| Coarse geometry | 접근 방향, selected face와 nominal contact/rotation 계획에 필요한 approximate shape 제공 | 실제 국소 표면과 정확히 일치한다고 가정하지 않음 | `[Fixed]` |
| Tactile | 손의 어느 sensor region에서 접촉이 발생·소실·이동했는지에 대한 국소 feedback 제공 | Binary tactile만으로 contact force나 object identity를 안다고 가정하지 않음 | `[Fixed]` |
| Wrist F/T | Hand–environment system에 전달되는 net force/torque와 부하 변화 제공 | 개별 접촉 위치나 taxel별 압력을 직접 분해한다고 가정하지 않음 | `[Fixed]` |
| Proprioception | 현재 arm/hand configuration과 command 결과 제공 | 외부 접촉 상태를 단독으로 완전히 식별한다고 가정하지 않음 | `[Fixed]` |

- `[Fixed]` Vision/coarse geometry와 contact sensing은 동일 정보를 중복 제공한다고 가정하지 않고, 전자는 전역 task·접근 정보를, 후자는 실행 중 실제 접촉과 model–reality mismatch에 관한 feedback을 담당한다.
- `[Hypothesis]` Tactile과 wrist F/T가 approximate geometry error를 실제로 보완하고 성능을 높이는지는 연구 질문으로 유지한다. 구체 검증 방식은 아직 협의 중이다.

### 3.5 Observability boundary

- `[Fixed]` Object pose와 coarse geometry가 지속적으로 관측되어도 contact state, friction, mass distribution, local surface와 force transmission은 완전히 관측되지 않는다.
- `[Rejected]` Track B를 `fully observable manipulation`으로 표현하는 방식.
- `[Fixed]` 정확한 simulator contact pair·point·normal·force, object velocity와 collision identity는 actor observation에서 제외한다. 이를 reward·constraint·termination·critic 또는 evaluation에 사용할지는 `[Open]`이다.
- `[Fixed]` Actor의 pose·OBB는 deployment에서 tracker-derived signal을 뜻한다.
- `[Open]` Training에서 ground-truth를 proxy로 사용할지, tracker noise·latency·dropout과 OBB ambiguity를 어떤 방식과 분포로 반영할지는 정하지 않았다.

### 3.6 Environment assumptions

- `[Fixed]` Blocker–shelf support-surface contact는 pushing·pivoting을 성립시키는 dynamics이므로 허용한다. 의도하지 않은 robot/blocker–shelf sidewall·pillar contact, 구조물 충돌과 경계 이탈은 별도의 안전 조건으로 관리한다.
- `[Open]` 추가 movable surrounding object를 제거한 단순 조건을 S0로 부르는 안은 있으나, 실제 evaluation이나 experiment에서 사용할지는 정하지 않았다.
- `[Fixed]` Intended shelf setting에는 active blocker 외의 target object와 movable surrounding objects가 존재한다. `[Open]`인 것은 존재 여부가 아니라 수·배치 randomization, 관측 범위와 접촉을 금지·허용·이용할지다.

### 3.7 Task start와 initial-state distribution

- `[Baseline]` Task는 hand–blocker non-contact 상태에서 시작한다.
- `[Open]` Evaluation에 사용할 initial pose sampling 분포는 아직 정하지 않았다.

---

## 4. Research Questions

| ID | Research question | Status |
| --- | --- | --- |
| RQ1 | Direct push가 어렵거나 불안정한 initial condition에서 preparatory Rotation이 최종 Push 성공 가능 영역을 넓히는가? | `[Hypothesis]` |
| RQ2 | Manipulation goal과 이후 Rotation→Push 결과를 고려한 hand/contact formation이 접촉 형성 또는 Rotation 성공만을 목표로 한 방법보다 최종 성공률을 높이는가? | `[Hypothesis]` |
| RQ3 | 동일한 approximate-geometry 조건에서 tactile과 wrist F/T가 pose·geometry·물성 오차에 대한 closed-loop robustness를 높이는가? | `[Hypothesis]` |
| RQ4 | 실행 중 wrist control과 함께 finger configuration까지 갱신하는 `Online` 방식이 `Constant` 또는 `Pre-contact` 방식보다 robust하고 안전한가? | `[Hypothesis]` |

Surrounding objects, exact geometry와 explicit downstream learning은 중요한 설계축이지만 현재 독립 research question으로 확정하지 않는다. RQ1–RQ4를 어떤 method와 evaluation으로 다룰지도 아직 정하지 않았다.

---

## 5. Research Hypotheses

다음은 Intro의 논지를 연구 질문 형태로 정리한 가설이다. **비교군, metric, protocol, ablation과 experiment를 정한 표가 아니며**, 구체 검증 방법은 모두 협의 중이다.

| Hypothesis | 의미 | 상태 |
| --- | --- | --- |
| **H1 — Preparatory Rotation utility.** | Direct push가 제한되는 initial condition에서 preparatory Rotation이 이후 Push가 가능한 상태 범위를 넓힐 수 있다. | `[Hypothesis]` |
| **H2 — Sequential/task-conditioned contact formation.** | Approach와 Rotation의 상태는 각 단계의 즉시 결과뿐 아니라 이후 Push와의 관계를 고려할 필요가 있다. | `[Hypothesis]` |
| **H3 — Complementary contact sensing.** | Approximate geometry에서 tactile과 wrist F/T가 서로 다른 contact 정보를 제공해 geometry·physics mismatch 대응에 기여할 수 있다. | `[Hypothesis]` |
| **H4 — Online wrist–finger adaptation.** | 실행 중 wrist와 finger configuration을 갱신하는 능력이 uncertainty와 contact 변화 대응에 기여할 수 있다. | `[Hypothesis]` |

H2의 `이후 단계를 고려한다`는 표현은 특정 reward, return, saved-state evaluation 또는 explicit feasibility model에 합의했다는 뜻이 아니다.

---

## 6. Method 상태

현재 합의된 policy 수준의 구체안은 6.3의 actor observation이다. 그 밖의 architecture, action/controller, reward, evaluation과 contact 처리 방식은 논의를 위해 남긴 후보이며 현재 baseline이나 구현 계획이 아니다.

### 6.1 Intro에서 정리한 method family 판단

- `[Baseline]` Intro에서 정리한 현재 low-level execution의 primary method family는 RL이다.
- `[Baseline]` RL 선택은 다른 방법보다 본질적으로 우월하다는 주장이 아니다. 현재는 **정확한 contact model·mode·switching rule과 포괄적인 recovery demonstration보다 task outcome·safety constraint와 randomized simulation interaction·reset을 더 쉽게 구성할 수 있다**는 자원 조건에 따른 선택이다.
- `[Rejected]` RL 사용 자체를 contribution 또는 novelty로 주장하는 방식.
- `[Open]` RL과 Conventional 또는 IL/VLA를 실제로 비교할지, 비교한다면 어떤 조건과 기준을 사용할지는 정하지 않았다.

Planning과 control은 Conventional methods, IL/VLA와 RL 모두에 필요한 기능이며 RL·IL·VLA와 병렬인 broad method family가 아니다. Research Trend에서는 **task-level decision rule의 주된 source**로 방법을 구분한다. `Conventional methods`는 사람이 설계한 model·mode·constraint·rule·planner를, `IL/VLA`는 demonstration 또는 pretrained action prior를, `RL`은 interaction에서 얻은 task return·constraint를 주된 decision source로 사용한다. 여러 source가 역할을 나누면 별도 `Hybrid` family를 만들기보다 composition으로 기록한다.

| Alternative family | 우리 조건에서 단독 적용할 때 남는 문제 | 본 연구에서 유지할 역할 | Status |
| --- | --- | --- | --- |
| Heuristic / motion primitive | Approximate geometry로 nominal contact가 달라지고 contact migration·slip·separation의 조합이 증가하면 예외 규칙이 확장됨 | 초기 action prior, reset behavior 또는 비교 대상으로 검토 가능 | `[Open]` |
| Explicit-model planning / optimization | 명시적 contact model·mode에 의존하는 경우 geometry·friction 또는 신뢰할 수 있는 online state estimate가 필요하며, wrist–finger contact sequence를 반복 탐색하면 online solve 부담이 증가함 | Low-level control, safety 또는 비교 대상으로 검토 가능 | `[Open]` |
| IL | Offline behavior cloning에서는 rollout error가 expert distribution 밖의 state를 만들 수 있으므로 geometry perturbation, contact loss와 recovery를 포괄하는 data·teacher 또는 interactive aggregation이 필요함 | Reactive/generative learning과 향후 pretraining의 비교 참고 후보 | `[Open]` |
| VLA | Semantic task prior에는 강하지만 contact-level correction에는 tactile/F/T가 포함된 data와 architecture가 별도로 필요하고, 현재 task의 고정된 semantics에서는 generalist 규모의 직접 이득이 불명확함 | 상위 task command와 goal-generation interface의 참고 후보 | `[Open]` |

이 한계들은 각 family가 contact-rich manipulation을 해결할 수 없다는 뜻이 아니다. [Diffusion Policy](https://doi.org/10.15607/RSS.2023.XIX.026)는 generative IL이 multimodal action distribution을 표현할 수 있음을, [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052)는 IL도 high-frequency tactile feedback에 반응할 수 있음을 보여준다. [DAgger](https://proceedings.mlr.press/v15/ross11a.html)는 learner-induced state에 expert label을 추가해 offline behavior cloning의 distribution shift를 줄이는 방법을 제시한다. Force-aware VLA도 contact-level feedback의 반례이며, 최신 흐름은 model·demonstration·RL이 역할을 나누는 composition에 가깝다. 따라서 Research Trend는 family의 우열이 아니라 **현재 정보·data·interaction budget에서 RL을 primary method family로 두는 조건부 이유**를 설명한다.

Nonprehensile manipulation 범주에서 RL의 상대적 강점은 다음 네 조건으로 정리한다.

1. Action의 비유일성만으로 RL이 정당화되지는 않는다. 다만 policy rollout의 모든 state에 corrective expert action을 붙이기보다 outcome evaluator를 구성하기 쉬운 경우, 여러 contact choice와 recovery 결과를 task return으로 비교할 수 있다.
2. Simulation은 IL/VLA의 demonstration 생성에도 사용할 수 있다. Reliable simulation teacher는 부족하지만 failure interaction을 안전하게 생성·reset하고 outcome으로 평가할 수 있을 때 RL의 상대적 이유가 생긴다.
3. Pre-contact formation, reorientation과 translation을 episode return으로 연결해 초기 contact decision이 후속 object outcome에 미친 영향을 최적화할 가능성이 있다. 구체 reward와 credit 확인 방식은 협의 중이다.
4. Policy inference를 통한 amortization은 IL/VLA에도 해당한다. 반복되는 task family에서 빠른 policy 실행이 필요하고 recovery demonstration보다 simulation interaction을 더 많이 확보할 수 있을 때 RL 학습 비용을 회수할 수 있다.

> `Specification coverage 부담 증가` + `policy-induced recovery data 부담 증가`만으로는 RL이 결론이 되지 않는다. 여기에 `measurable outcome` + `safe·affordable interaction/reset` + `representative simulation`이 있을 때 interaction-return RL이 조건부로 합리적이다.

RL은 그 대신 interaction volume, reward·credit assignment, exploration safety, simulator contact fidelity와 Sim-to-Real 부담을 진다. Randomization 설정만으로 recovery coverage가 확보됐다고 보지 않는다. 신뢰할 수 있는 model과 작은 mode 집합이 있거나, 충분한 expert/recovery data나 simulation teacher가 있거나, one-off·OOD goal을 즉시 풀어야 하면 conventional method 또는 IL/VLA가 더 적합할 수 있다. RL은 geometry representation, low-level controller와 safety supervisor를 대체하지 않으며, planning·optimization·demonstration prior와의 결합 여부도 `[Open]`이다. 또한 RL 선택은 auxiliary fixed-structure contact나 movable-object contact를 manipulation resource로 사용한다는 결정에 의존하지 않으며, 해당 contact의 적극적 활용은 Section 7.1의 `[Open]`을 따른다.

### 6.2 Policy organization — `[Open]`

- `[Open]` 하나의 shared MLP policy가 세 sub-objective를 수행하는 방안을 검토 중이다.
- `[Open]` 현재 66D observation에는 Phase ID를 넣지 않는다. 향후 observation을 바꾸어 Phase ID를 포함할지와 reward gate 또는 phase transition 방식을 어떻게 둘지는 정하지 않았다.
- `[Open]` 세 phase에서 같은 action space를 사용할지도 action/controller 설계와 함께 협의한다.
- `[Open]` Shared phase-free policy, phase ID 포함 shared policy와 phase-specific policy/controller 중 어떤 구성을 비교할지도 정하지 않았다.
- `[Rejected]` Shared policy 또는 phase-ID 제거 자체를 검증 없이 contribution으로 표현하는 방식.

### 6.3 현재 observation 구성

현재 66D 구성은 합의된 actor observation의 작업 기준이며 연구 contribution 자체는 아니다.

| Observation block | Current encoding | Dimension | Status |
| --- | --- | ---: | --- |
| Task goal and selected-face state | End-effector-frame target position, push direction, current selected-face pushing normal | 9 | `[Baseline]` |
| Current object pose | End-effector-relative position + canonical quaternion | 7 | `[Baseline]` |
| Geometry | Object-local OBB extent | 3 | `[Baseline]` |
| Arm/hand proprioception | UR5e q + RH56E2 q | 12 | `[Baseline]` |
| Tactile | Current 17D binary vector | 17 | `[Baseline]` |
| Wrist F/T | Current end-effector-frame wrench | 6 | `[Baseline]` |
| Minimal action memory | Previous action one step | 12 | `[Baseline]`; 현재 12D action 후보를 전제로 하며 action이 바뀌면 차원을 다시 확인 |
| **Total** |  | **66** | `[Baseline]` |

- `[Baseline]` Tactile은 fixed sensor identity를 유지하는 17D any-contact binary vector다.
- `[Baseline]` 하나의 threshold를 사용하고 hysteresis는 사용하지 않는다.
- `[Baseline]` Tactile과 F/T는 current sample, previous action은 one-step만 사용한다.
- `[Open]` Threshold, filtering, history, action memory와 exact sensor mapping의 세부값은 실물 log를 확인한 뒤 협의한다.
- `[Open]` Selected-face representation과 OBB consistency의 적용 범위 및 확인 방식은 별도로 협의한다.

### 6.4 Action and controller — `[Open]`

| Element | Current choice | Status |
| --- | --- | --- |
| Wrist action | End-effector-frame delta translation 3D + local rotation increment 3D | `[Open]` |
| Finger action | RH56E2 actuated joint action 6D | `[Open]` |
| Total action | 12D | `[Open]` |
| Command semantics | 매 policy step의 measured state를 기준으로 새 command 생성 | `[Open]` |
| Low-level controller | Differential IK 또는 OSC | `[Open]` |
| Scale/frequency | Translation·rotation·finger scale와 policy rate | `[Open]` |

6-DoF wrist와 finger action을 함께 쓰는 안은 contact location, force direction과 configuration을 online으로 바꿀 자유도를 제공한다. 채택 여부와 비교 방식은 아직 협의 중이다.

### 6.5 Reward and safety discussion — `[Open]`

이 절은 합의된 reward 명세가 아니다. 아래 구분은 이후 논의를 위해 보존한 후보이며, 채택 여부·수식·우선순위는 모두 협의 중이다.

| Layer | 논의할 역할 | Examples | Status |
| --- | --- | --- | --- |
| Final task objective | 목표 이동과 face-direction alignment, stable completion | Sparse success | `[Open]` |
| Phase-specific shaping | Exploration과 credit assignment 보조 | Approach distance, alignment progress, push progress | `[Open]` |
| Contact shaping | 전체 접촉 상실을 줄이되 개별 contact migration 허용 | Aggregate contact-loss event | `[Open]` |
| Safety constraint | Task progress와 교환되지 않아야 하는 위반 | Boundary exit, topple, drop, overload, joint limit와 OD-1에서 금지한 auxiliary-structure·surrounding-object contact | `[Open]` |
| Control regularization | 불필요한 진동·명령 변화 억제 | Action rate, optional hand-motion cost | `[Open]` |

- `[Open]` 각 term의 수식, weight, threshold, curriculum과 constrained-RL/termination 방식은 확정하지 않았다.
- `[Fixed]` Reward term과 evaluation metric을 같은 것으로 취급하지 않는다. Reward는 학습 신호이고 metric은 가설 검증을 위한 외부 측정이다.
- `[Open]` Contact loss를 회복 가능한 penalty event로 둘지, termination과 어떻게 구분할지 아직 정하지 않았다.
- `[Open]` Contact 유지 signal과 hand-motion cost가 실제로 필요한지, 필요하다면 어떻게 판단할지는 H4와 관련해 추후 협의한다.

### 6.6 후속 단계의 고려 범위 — `[Open]`

다음 세 수준을 구분한다.

| 구분 | 의미 | 현재 사용 | 상태 |
| --- | --- | --- | --- |
| Shared episodic return | 앞 단계 action이 뒤 phase reward와 final success return을 함께 받음 | 학습 방식 후보 | `[Open]` |
| Transition evaluation | 저장한 Approach/Rotation state에서 continuation rollout을 실행하여 다음 단계 성공률을 측정 | 평가 방식 후보 | `[Open]` |
| Explicit downstream learning | Learned feasibility value, transition reward, terminal-state critic 또는 별도 objective로 다음 phase 가능성을 직접 최적화 | 구현 여부 미정 | `[Open]` |

세 방식 중 무엇을 사용할지는 아직 결정하지 않았다. 따라서 후속 단계 고려를 현재 method나 contribution으로 확정하지 않는다.

### 6.7 Contact and reconfiguration behavior — `[Open]`

- `[Open]` 최초 접촉 이후 aggregate hand–blocker contact를 선호할지 정하지 않았다.
- `[Open]` 동일 finger, taxel 또는 contact patch의 유지 여부는 별도 contact 규칙과 함께 협의한다.
- `[Open]` Rotation과 Push 중 contact migration, release/re-contact와 wrist–finger adjustment를 어느 범위까지 허용할지 정하지 않았다.
- `[Open]` Aggregate contact를 계속 유지하는 것이 항상 유리한지, deliberate full release가 필요한 object/task가 있는지 검토한다.
- `[Rejected]` Tactile bit change 자체를 failure 또는 reconfiguration penalty로 해석하는 방식.

---

## 7. Open Decisions

### 7.1 OD-1 — Environment Contact and Surrounding Objects

Intended shelf setting에는 active blocker 외의 movable surrounding objects가 존재한다. 다만 `shelf scene`, `clutter`, environmental contact와 `forbidden collision`을 혼용하지 않도록 support-surface contact, auxiliary fixed-structure contact와 movable surrounding-object contact를 구분한다. 주변 물체의 수·배치와 접촉 처리 방식은 아직 확정되지 않았다.

| Question | Alternatives | Evidence/decision needed | Current status |
| --- | --- | --- | --- |
| 주변 물체가 존재하는가? | Intended shelf에는 존재 / 단순화 조건에서는 제거 가능 | Application 범위와 단순 조건의 구분 | 존재 `[Fixed]`; 단순 조건 사용 `[Open]` |
| 수와 위치를 randomize하는가? | Fixed layout / count·pose randomization / curriculum | 학습 안정성 및 generalization protocol | `[Open]` |
| Shelf sidewall·pillar 같은 고정 구조물의 보조 접촉을 허용·이용하는가? | Support surface만 허용 / incidental contact 허용 / pivoting resource로 명시적 이용 | 구조적 constraint와 manipulation resource의 구분, force·collision limit 및 H1–H4와의 관계 | `[Open]` |
| Blocker–surrounding-object contact를 허용하는가? | 모두 금지 / 제한적 contact 허용 / manipulation에 이용 | 실제 application에서 허용 가능한 물리적 상호작용 정의 | `[Open]` |
| 주변 물체의 연구상 역할은 무엇인가? | Safety obstacle / policy가 대응할 uncertainty / active interaction 대상 | Observation에 scene state가 필요한지와 collision-only 처리가 충분한지 검증 | `[Open]` |
| Single- vs. multi-object 조건을 분리하는가? | 단일 조건과 multi-object 조건을 분리 / 처음부터 혼합 | 연구 질문을 구분하기 쉬운 정도와 연구 범위의 차이 | `[Open]` |

논의 중인 장면 구분 후보는 다음과 같다. 아직 실험 조건으로 채택하지 않았다.

- `[Open]` S0: 추가 movable surrounding object 없이 한 blocker만 두는 단순 조건.
- `[Fixed]` S1: 1–N개의 passive surrounding objects가 존재하는 intended shelf condition. 수·pose randomization과 contact rule은 `[Open]`이다.
- `[Open]` S2: Surrounding-object contact를 허용하거나 이용하는 multi-object contact manipulation. 채택하면 observation과 research scope가 크게 바뀐다.

현재 판단에서 blocker–shelf support-surface contact는 pushing·pivoting을 성립시키는 필수 dynamics다. Shelf sidewall·pillar 같은 **auxiliary fixed-structure contact**와 **movable-object contact**를 금지·허용·이용 중 어디까지 둘지는 open decision이다. Broad nonprehensile manipulation에서 environmental contact가 유용하다는 사실만으로 이를 현재 method의 핵심 contribution으로 승격하지 않는다.

S1은 intended application을 나타내고 S0는 이를 단순화한 논의용 조건이다. 실제 evaluation이나 experiment에서 둘을 어떻게 사용할지는 아직 정하지 않았다.

### 7.2 OD-2 — Geometry Information and Perturbation

OBB를 쓴다는 사실보다 `무엇을 알고 얼마나 틀리는가`를 먼저 정의해야 한다.

| Decision | Required clarification | Current status |
| --- | --- | --- |
| Geometry source | Known dimensions, perception-estimated OBB, category template 중 무엇인지 | `[Open]` |
| Pose source | Marker/vision pose와 geometry source를 분리하고 frame registration 방법 정의 | `[Open]` |
| OBB error components | Center offset, orientation error, extent scale/asymmetry, temporal jitter, axis permutation을 분리 | `[Open]` |
| Error distribution | Uniform 임의값이 아니라 실제 perception log 또는 명시적 stress range 사용 | `[Open]` |
| Exact-geometry reference | Oracle OBB만 둘지 registered CAD/mesh upper bound까지 둘지 | `[Open]` |
| Implicit geometry 비교 조건 | Point cloud/RGB feature policy를 구현할지 문헌 비교로만 둘지 | `[Open]` |

Geometry 조건을 논의하기 위한 후보는 다음과 같다. 이 구분을 그대로 evaluation이나 experiment에 사용할지는 미정이다.

| Code | Condition | Purpose | Status |
| --- | --- | --- | --- |
| G0 | Pose only, no geometry | Geometry input의 최소 하한 | `[Open]` |
| G1 | Perturbed approximate OBB | 현재 observation 가정과 맞는 조건 | `[Open]` |
| G2 | Oracle OBB dimensions/frame | OBB estimation error의 upper bound | `[Open]` |
| G3 | Registered exact mesh/CAD | Contact geometry까지 아는 upper bound | `[Open]` |

Coarse geometry와 tactile/F/T의 관계를 어떤 조건에서 살펴볼지, G0–G3 중 무엇을 사용할지는 이후 협의한다.

### 7.3 OD-3 — Next-Phase Consideration

| Interpretation | What it demonstrates | What it does not demonstrate | Current status |
| --- | --- | --- | --- |
| Shared full-episode return | 앞 단계가 전체 성공 return의 영향을 받을 가능성 | Credit가 실제로 전달되었거나 terminal state가 개선되었다는 보장 | `[Open]` |
| Rotation과 Rotation-to-Push의 분리 평가 | Rotation success와 Rotation-to-Push success가 다른지 측정 | Policy가 그 차이를 명시적으로 학습했다는 증거 | `[Open]` |
| Explicit feasibility objective | 다음 phase 성공 가능성을 직접 학습·shaping | 별도 estimator의 calibration·OOD 안정성 | `[Open]` |

세 해석은 구분해서 논의하되, 어떤 것을 학습이나 평가에 사용할지는 정하지 않았다. 이를 실제로 합의·구현하기 전에는 후속 단계를 직접 학습하는 방법을 contribution으로 쓰지 않는다.

### 7.4 Additional open decisions

| ID | Decision | Why unresolved | Required evidence |
| --- | --- | --- | --- |
| OD-4 | Selected face+direction vs. explicit desired rotation goal | OBB symmetry와 non-box-like object에서 동일하지 않을 수 있음 | Task set과 goal ambiguity analysis |
| OD-5 | Shared/phase-free vs. phase-aware/phase-specific policy | 현재 선택의 성능·sample-efficiency 근거가 없음 | Policy organization에 대한 추가 논의 |
| OD-6 | Initial non-contact state distribution과 upper planner–policy 시작 경계 | 너무 쉬운 sampling은 Approach 가설을 약화하고 너무 넓으면 학습을 막으며, long-range reaching을 policy 범위에 포함하는지에 따라 비교 조건이 달라짐 | Planner/reachability 기반 분포, policy 시작 위치와 curriculum study |
| OD-7 | DiffIK vs. OSC, action scale·rate | Contact response와 sim-to-real behavior가 달라짐 | Controller-matched rollout statistics and hardware limits |
| OD-8 | Tactile threshold, F/T preprocessing, history | Sensor calibration과 latency를 아직 측정하지 않음 | RH56E2/F/T logs, synchronization and noise analysis |
| OD-9 | Reward gate, weight와 safety algorithm | 문헌의 수치를 직접 이식할 수 없음 | Reward formulation과 검토 방법에 대한 합의 |
| OD-10 | Exact held-out object/generalization boundary | `unseen`의 범위가 category·shape·physics 중 무엇인지 불명확 | Train/test split definition |
| OD-11 | Real-world deployment protocol | Tracker, sensor calibration과 safety supervisor interface 미정 | Hardware integration plan |

### 7.5 Contradiction and over-specification audit

| Item | Existing Statement | Problem | Required Decision/Evidence | Current Status |
| --- | --- | --- | --- | --- |
| 연구 범위와 구현 범위 | Selected-face goal, 66D observation과 shared MLP가 현재 연구 정의처럼 서술됨 | 연구 문제와 최초 구현 선택이 혼합됨 | Selected-face goal과 66D observation만 현재 기준으로 두고 policy organization은 별도 협의 | Observation `[Baseline]`; organization `[Open]` |
| Fully observable 표현 | Continuous pose와 OBB가 있으므로 상태를 충분히 안다는 암묵적 전제 | Local contact, friction, mass와 geometry error가 남음 | Partial observability와 hidden physics를 perturbation별로 정의 | `fully observable`은 `[Rejected]` |
| Vision과 privileged simulator information | Continuous vision pose와 exact simulator state가 같은 state처럼 사용될 수 있음 | 실물 perception과 simulation teacher의 가용성이 다름 | Actor tensor와 reward/constraint/evaluation tensor를 코드·표에서 분리 | 경계 `[Fixed]`, 구현 검증 `[Open]` |
| Policy input과 reward/evaluation 정보 | Exact contact force·collision을 reward에 쓰므로 policy도 이를 활용한다고 오해 가능 | Deployment information leakage 위험 | Asymmetric critic 포함 여부까지 별도 목록과 unit test 작성 | Actor/teacher 분리 `[Fixed]` |
| Surrounding objects | Intended shelf에는 다른 물체가 있지만 S0에서는 제거 | 존재는 고정됐으나 수·pose randomization, 관측 범위와 접촉 역할이 미정 | S1 distribution·contact rule과 optional S2 결정 | 존재 `[Fixed]`, 처리 OD-1 `[Open]` |
| Shared policy 사용 이유 | 세 phase를 하나의 shared policy로 학습하는 것이 자연스러운 선택처럼 서술 | Phase-specific policy 대비 장점과 실패 조건이 미검증 | Policy organization 후보와 비교 필요성을 이후 협의 | `[Open]` |
| Phase ID 제거 이유 | 이전 연구 경험상 gate만으로 가능하므로 phase ID를 제거 | Markov state sufficiency와 성능상 이점이 본 task에서 미검증 | Phase ID와 gate 방식을 이후 협의 | `[Open]` |
| Hand configuration 평가 시점 | Approach 종료 state가 후속 Rotation·Push에 유효한지 평가 | Rotation·Push 중 계속 변하는 configuration을 설명하지 못함 | 무엇을 어느 시점에 측정할지 이후 협의 | `[Open]` |
| Approach contact 유지 | 최초 접촉을 Rotation·Push까지 유지하는 것이 효율적이라는 직관 | 필요한 release/re-contact를 막을 수 있음 | Contact 유지·전환 규칙과 평가 방식을 이후 협의 | `[Open]` |
| 실행 중 contact-configuration update | Approach에서 좋은 configuration을 만들면 이후 갱신이 적어야 한다는 서술 | Configuration 고정이 조작 범위와 robustness를 제한할 가능성 | Action authority와 contact 변경 규칙을 이후 협의 | `[Open]` |
| Tactile과 F/T 역할 | 두 센서가 geometry error를 함께 보완한다고 포괄적으로 서술 | 제공 정보와 중복·상보성이 분리되지 않음 | Tactile은 spatial contact identity, F/T는 net wrench라는 해석과 검증 방식을 이후 협의 | 역할 구분 `[Hypothesis]` |
| 6-DoF wrist와 finger control | 둘을 모두 action에 포함하는 12D interface | 모든 자유도가 실제로 필요하다는 근거가 아직 없음 | Action authority와 검토 방법을 이후 협의 | `[Open]` |
| Reward term과 evaluation metric | Alignment, contact와 success가 reward와 metric 양쪽에 등장 | Reward 최적화가 곧 가설 입증이라는 순환 논증 위험 | Reward와 evaluation을 구분하되 각각의 구체 정의는 이후 협의 | 구분 원칙 `[Fixed]`; 내용 `[Open]` |
| Contribution/novelty | 후속 단계 고려, multimodal adaptation과 unified policy를 가능한 contribution으로 서술 | 평가 방법, method 범위와 contribution 후보가 혼재 | C1·C2의 검증 조건과 평가 방식을 이후 협의 | C1·C2 `[Candidate]`; 구체 method·evaluation `[Open]` |

---

## 8. Candidate Contributions

현재 활성 contribution 후보는 C1과 C2뿐이다.

| Candidate Contribution | 현재 아이디어 | 아직 필요한 근거 | 상태 |
| --- | --- | --- | --- |
| **C1. Contact feedback을 통한 approximate-geometry error 보완** | Approximate OBB와 tactile/F/T를 함께 사용해 실행 중 geometry·physics mismatch에 대응 | Contact feedback이 실제로 approximate-geometry error에 따른 성능 저하를 줄인다는 근거와 선행연구 대비 차이 | `[Candidate]` |
| **C2. 이후 Rotation·Push에 적합한 contact-state formation과 online wrist–finger adaptation** | Task goal을 고려한 contact formation과 실행 중 wrist–finger adjustment | 초기 configuration 형성과 online adaptation이 이후 Rotation·Push에 주는 추가 효과 및 선행연구 대비 차이 | `[Candidate]` |

C2의 **결합 효과**를 주장하려면 contact formation과 online adaptation 각각의 역할을 구분할 근거가 필요하다. 구체적인 evaluation과 experiment 설계는 아직 합의하지 않았다.

Tactile 또는 wrist F/T의 사용 자체는 contribution이 아니다. C1/C2는 contact feedback이 **approximate geometry의 오차를 보완하고**, 이후 Rotation과 Push에 적합한 접촉 상태의 형성 및 online adaptation에 기여한다는 근거가 마련될 때만 확정한다. 그 근거를 만드는 구체 방법은 `[Open]`이다.

- `[Open]` Rotation success와 Rotation-to-Push success를 구분하는 관점은 contribution이 아니라 향후 평가 방식의 후보다.
- `[Superseded]` 이전 C4인 `Approach–Rotation–Push goal-conditioned low-level policy`는 현재 연구 범위의 설명이며 그 자체를 contribution 후보로 세지 않는다.

다음은 contribution으로 단독 주장하지 않는다.

- `[Rejected]` Tactile 사용 자체, F/T 사용 자체, RL 사용 자체.
- `[Rejected]` Rotate-then-push라는 순서 자체.
- `[Rejected]` Task-conditioned contact formation, wrist–finger control 또는 `Contact Configuration=Online`이라는 구성 자체를 novelty로 주장하는 방식. C2의 구체 범위와 입증 조건은 협의 중이다.
- `[Rejected]` MLP, 66D observation 또는 12D action이라는 구현 수치.
- `[Rejected]` 기존 IL/VLA가 contact feedback을 사용하지 못한다는 일반화.
- `[Rejected]` OBB가 point cloud/mesh보다 일반적으로 우월하다는 주장.

---

## 9. Evaluation and Experiment — `[Open]`

현재 evaluation metric·success criterion·protocol, baseline 비교, ablation과 experiment 구성은 **합의된 적이 없다**. 앞선 정리 과정에서 부여했던 E1–E9 번호와 세부 표는 현재 계획에서 철회한다. 이력은 Section 14에만 보존한다.

향후 협의가 필요한 질문은 다음 수준에 머문다.

- Task 성공과 각 조작 단계의 결과를 무엇으로 정의할 것인가?
- 앞 단계의 상태가 다음 Rotation 또는 Push에 유효한지를 별도로 볼 것인가?
- Geometry·pose·물성 변화, 안전과 contact behavior를 어떤 지표로 다룰 것인가?
- 어떤 method 또는 component를 실제 비교 대상으로 삼을 것인가?
- Simulation과 hardware 결과를 어떤 범위에서 연결할 것인가?

구체 metric, tolerance, split, controlled factor, seed·trial budget과 통계 방법은 모두 `[Open]`이다. 합의 전에는 위 질문을 `평가 계획`, `실험 계획`, `primary metric` 또는 `baseline experiment`라고 부르지 않는다.

---

## 10. Previous Works Classification Criteria

### 10.1 Research Trend와 closest Previous Works의 분리

문헌은 서로 다른 질문에 답하는 두 부분으로 나눈다.

| 구분 | 질문 | 포함할 연구 | 정리할 내용 |
| --- | --- | --- | --- |
| **Research Trend** | Designed model·rule, demonstration·action prior와 interaction return은 nonprehensile manipulation의 decision knowledge를 어떻게 제공하며, 어떤 조건에서 RL이 더 합리적인가? | Direct nonprehensile work와 method 선택의 반례가 되는 adjacent contact-rich IL/VLA | 공통 decision burden, 각 supervision source의 해결 범위·resource burden, 조건부 RL 선택과 기각 근거 |
| **Closest Previous Works** | 우리와 유사한 연구는 시간적으로 어떤 method concept을 발전시켰으며, Environment, Robot Agent와 System의 공통 기준에서 Ours와 어떻게 다른가? | B85를 포함해 nonprehensile execution, tactile/F/T, geometry uncertainty 또는 wrist–finger control과 직접 관련된 11편 | 비교 집합만의 2022–2026 timeline, `Object Configuration·Surrounding Objects / Sensory Input·Tool·Contact Configuration·Object Manipulation / Object Geometry·Method` 비교와 남은 검증 질문 |

`Research Trend`는 method 선택을 정당화하지만 novelty를 만들지 않는다. `Previous Works` 표에서 관찰한 feature 조합도 그 자체로 contribution이 아니다. Contribution으로 주장하려면 어떤 uncertainty와 downstream outcome을 개선하는지에 대한 근거가 필요하며, 구체적인 evaluation과 experiment는 협의 중이다.

Research Trend에는 연도별 timeline을 두지 않는다. Planning과 control은 모든 family에 필요한 기능으로 보고, method는 task-level decision knowledge의 source에 따라 `Conventional methods`, `IL/VLA`, `RL`로 묶는다. Composition은 별도 family가 아니라 model·demonstration·interaction return이 맡는 역할을 표시한다. 설명 순서는 `nonprehensile manipulation의 공통 decision burden → 기존 연구가 해결한 범위와 남긴 resource burden → RL 전환이 성립하는 추가 전제 → 현재 조건에 적용 → 반증 조건`으로 고정한다. 연도별 변화는 Previous Works 비교표의 11편만 사용한 timeline에서 다루며, 각 논문은 동일한 `B-ID`로 비교표와 연결한다.

Research Trend의 `Conventional methods`는 Previous Works 표의 compact value인 `Planning/Control`에 대응한다. Trend에서는 supervision burden을 함께 논의하기 위해 IL과 VLA를 묶지만 비교표에서는 `IL`과 `VLA`를 분리하며, RL은 두 문서에서 동일한 의미로 사용한다.

RL 선택 논리는 `RL의 장점 나열`이 아니라 다음 순서를 따른다.

1. Planning/control을 공통 기능으로 두고, task-level decision rule의 source를 designed specification, demonstration/action prior와 interaction return으로 구분한다.
2. Conventional methods와 IL/VLA가 이미 contact feedback·reactive correction·multimodal action을 다룰 수 있음을 먼저 제시한다.
3. 각 group의 상대적 부담을 model·mode·rule의 specification coverage, off-nominal·recovery data coverage와 interaction-return learning cost로 비교한다.
4. `Specification 부담`이나 `demonstration 부족`만으로 RL을 결론 내리지 않고, measurable outcome, safe·affordable interaction/reset과 representative simulation을 구성할 수 있을 때에만 RL을 primary method family로 둔다.
5. Interaction volume, reward·credit assignment, unsafe exploration, simulator fidelity와 Sim-to-Real gap은 RL이 부담하는 비용으로 함께 제시한다.
6. Reliable model·small mode set, sufficient recovery data·teacher 또는 one-off·OOD task 조건에서는 conventional method나 IL/VLA가 더 적합할 수 있음을 기각 경계로 둔다.

따라서 `왜 RL인가`는 Intro의 조건부 method 선택 근거이며 contribution이 아니다. RL의 세부 algorithm과 비교 방법은 아직 정하지 않았다.

### 10.2 두 문헌 흐름과 연구의 교차점

문헌 검색은 다음 두 연구 흐름을 모두 포함한다. 실제 논문 비교는 Section 10.3의 Environment–Robot Agent–System 기준을 따른다.

| Literature stream | 포함할 세부 주제 | 각 흐름에서 확인할 질문 | 현재 연구와의 관계 |
| --- | --- | --- | --- |
| **Nonprehensile manipulation** | Pushing, pulling, pivoting, dexterous-hand contact/pose planning | 목표 물체 운동을 위해 contact pose, hand configuration과 contact-mode transition을 어떻게 정하고 실행하는가? | Task category와 Approach–Rotation–Push 구조의 직접 근거와 비교 관점을 제공 |
| **Contact-feedback-based manipulation** | Tactile feedback, wrist F/T feedback, contact-state estimation, online adaptation | 실행 중 실제 접촉·힘·동역학 오차를 어떤 sensing과 feedback mechanism으로 추정·보정하는가? | Approximate geometry 아래의 closed-loop correction과 wrist–finger adaptation 근거와 비교 관점을 제공 |
| **두 흐름의 교차점** | Approximate geometry를 이용한 전역 접근/goal 설정 + deployable contact feedback을 이용한 접촉 상태 조정 | Contact feedback이 geometry approximation error를 보완하고 이후 Rotation·Push에 적합한 상태와 robust execution을 만드는가? | H2–H4의 연구 질문과 연결하며, 결과 전에는 gap·contribution을 `[Candidate]`로 유지 |

초기 literature map은 다음과 같다. 한 논문이 두 흐름에 걸칠 수 있으며, 최종 분류는 full text에서 실제 observation, feedback loop와 action을 확인한 뒤 확정한다.

| Stream | Subtopic | Representative starting points | Reading purpose |
| --- | --- | --- | --- |
| Nonprehensile manipulation | Pushing / cluttered pushing | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166), [Goal-Oriented Non-Prehensile Pushing](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal·collision·toppling 조건과 unknown-object pushing의 비교 관점 확인 |
| Nonprehensile manipulation | Pulling and direction-conditioned hand pose | [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Pushing/pulling direction에 맞춘 dexterous pre-contact pose의 범위 확인 |
| Nonprehensile manipulation | Rotation / pivoting | [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262), [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) | Target orientation, contact/force guidance와 object generalization 비교 |
| Nonprehensile manipulation | Dexterous contact/pose planning | [Learning Contact Locations](https://doi.org/10.1109/HUMANOIDS.2013.7030011), [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652), [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Contact location·hand pose가 manipulation goal에 조건화되는 방식 확인 |
| Contact-feedback-based manipulation | Tactile feedback | [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236), [DexTouch](https://doi.org/10.1109/LRA.2024.3478571), [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052), [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) | Tactile encoding, reactive correction, force grounding과 sim-to-real 효과 확인 |
| Contact-feedback-based manipulation | Wrist F/T feedback and force-aware control | [COCOI](https://doi.org/10.1109/IROS51168.2021.9636836), [ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169), [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Net wrench, online context와 active hybrid force–position execution의 역할 확인 |
| Contact-feedback-based manipulation | Contact-state estimation | [Active Extrinsic Contact Sensing](https://doi.org/10.1109/ICRA46639.2022.9812017), [1 kHz Behavior Tree for Self-adaptable Tactile Insertion](https://doi.org/10.1109/ICRA57147.2024.10610835) | Contact onset/loss/mode 추정과 controller 전환의 역할 확인 |
| Contact-feedback-based manipulation | Online adaptation | [COCOI](https://doi.org/10.1109/IROS51168.2021.9636836), [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744), [ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169) | Hidden dynamics 또는 task execution 중 configuration 보정의 추가 가치 확인. ForceVLA2의 runtime re-grasp는 system-level `Online` 사례지만 aperture-command source와 retry가 force/contact feedback으로 trigger됐는지는 미보고 |

방법의 종류는 두 문헌 흐름을 대체하지 않으며, 각 논문이 action을 생성하고 학습하는 방식을 함께 기록하는 데만 사용한다.

### 10.3 Environment–Robot Agent–System 비교 기준

여기서 Agent는 AI agent가 아니라 **환경과 물리적으로 상호작용하는 robot agent**를 뜻한다. 비교표는 다음 여덟 column과 정해진 값만 사용한다. 상세한 판정과 논문별 표는 [`Intro/previous_works.md`](./Intro/previous_works.md#1-column-기준)를 따른다.

| 구분 | Column | 사용하는 값 | 판정 질문 |
| --- | --- | --- | --- |
| **Environment** | **물체 형상 (Object Configuration)** | `Structured` / `Unstructured` | 조작 대상이 고정·정형 형상군인가, 다양한 비정형 형상인가? |
| **Environment** | **주변 물체 (Surrounding Objects)** | `Absent` / `Present` | 조작 대상 외의 movable object가 접근·시야·경로 또는 접촉에 영향을 주는가? |
| **Robot Agent** | **감각 입력 (Sensory Input)** | `Vision` / `Contact` / `Vision+Contact` | 실행에 vision과 tactile·contact state·wrist F/T 중 무엇을 사용하는가? |
| **Robot Agent** | **도구 (Tool)** | `Pusher` / `Gripper` / `Dexterous Hand` | 물체와 직접 접촉하는 물리적 도구는 무엇인가? |
| **Robot Agent** | **접촉 구성 (Contact Configuration)** | `Constant` / `Pre-contact` / `Online` | Aperture·finger posture 같은 내부 구성을 언제 결정·갱신하는가? |
| **Robot Agent** | **물체 조작 (Object Manipulation)** | `Translation` / `Reorientation` / `Combined` | 대상 물체의 이동, 회전 또는 둘 모두를 다루는가? |
| **System** | **물체 형상 정보 (Object Geometry)** | `None` / `Estimated` / `Exact` | 방법이 명시적 물체 형상을 어떤 정확도로 사용하는가? |
| **System** | **방법 (Method)** | `Planning/Control` / `RL` / `IL` / `VLA` | 배포 시 task-level action을 정하는 주된 방법은 무엇인가? |

`Object Configuration`은 pose·배치가 아니라 **대상 물체의 물리적 형상 범위**다. 고정 물체나 제한된 canonical family는 `Structured`, heterogeneous everyday object와 irregular shape까지 다루면 `Unstructured`다. 이 값만으로 unseen-object generalization을 주장하지 않는다.

`Surrounding Objects=Present`는 non-target movable object가 같은 장면에 있고 manipulation에 실제 제약을 줄 때만 사용한다. Table·shelf·wall 같은 고정 구조물, support surface와 task-essential tool은 제외하며, 여러 물체를 한 번에 하나씩 따로 시험한 경우도 `Absent`다. `Mixed`는 두지 않고, 양쪽을 모두 보고한 method는 최대 deployed capability인 `Present`로 표시한다.

`Sensory Input`은 external sensory channel이다. Scene camera의 RGB·depth·RGB-D는 `Vision`, optical tactile image를 포함한 tactile·binary contact·wrist F/T는 `Contact`이며, common proprioception과 goal·geometry representation은 세지 않는다. `Tool`은 physical contact device다. Active internal DOF가 없는 tip은 `Pusher`, coupled aperture fingers는 `Gripper`, independently actuated multi-finger joints는 `Dexterous Hand`다. 닫힌 gripper를 pusher처럼 사용해도 `Tool=Gripper`이며, 내부 구성을 활용하는지는 `Contact Configuration`에서 별도로 읽는다.

`Contact Configuration`은 internal configuration update authority다. 같은 구성을 유지하면 `Constant`, object·task-conditioned 구성을 첫 manipulation contact 전에 선택하고 실행 중 유지하면 `Pre-contact`, autonomous execution 중 다시 갱신할 수 있으면 `Online`이다. Wrist/EEF pose, arm motion에 따른 contact-point 이동, target-force 변경과 passive compliance는 제외한다. `Online`에는 feedback 기반 연속 조정과 temporary separation 뒤의 retry·re-contact·re-grasp를 포함하며, 판정 단위는 learned action vector가 아니라 autonomous deployment stack 전체다. Update trigger나 command source가 보고되지 않으면 근거 주석으로 남긴다.

[B99 ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169)는 `Tool=Gripper`이며 Retrieve Plate의 autonomous retry/re-grasp 때문에 `Contact Configuration=Online`이다. 다만 formal learned action에는 gripper-aperture command가 없으므로 VLA·subtask script·별도 controller 중 command source와 retry trigger는 `미보고`로 남긴다. 이 low-dimensional re-grasp capability는 Ours의 continuous multi-joint finger update와 동일하지 않다.

`Object Manipulation`은 robot motion이 아니라 대상 물체의 운동이다. Push·pull·slide는 `Translation`, pivot·rotation은 `Reorientation`, 둘을 모두 보고하면 `Combined`다. `Combined`는 두 동작을 한 episode에서 연결했다는 뜻은 아니다.

`Object Geometry`는 System이 실행에 사용하는 **명시적 형상 표현**이다. RGB·depth 영상을 구조화하지 않고 직접 encode하면 `None`, sensor-estimated OBB·point cloud·shape feature는 `Estimated`, registered CAD·mesh 또는 known exact dimensions는 `Exact`다. Section 3.3의 `Implicit visual`은 이 표에서는 `None`이다. Training/simulator만 exact geometry를 사용하면 `Exact`로 세지 않는다.

`Method`는 모든 module이 아니라 deployed task-level decision family 하나를 기록한다. Scripted heuristic, explicit controller·optimizer·planner와 generative proposal을 simulation/planning으로 고르는 stack은 `Planning/Control`, interaction return policy는 `RL`, non-VLA demonstration policy는 `IL`, pretrained vision–language action policy는 `VLA`다. 여기서 `Planning/Control`은 Previous Works 표를 위한 compact conventional-stack label이며, planning과 control이 RL·IL·VLA에 필요하지 않다는 뜻이 아니다. GD2P처럼 generative proposal이 있어도 최종 task-level selection을 physics simulation과 planning이 지배하면 `Planning/Control`로 기록한다. Optimization·generative·low-level control은 함께 쓰이는 module일 수 있으므로 별도 peer label이나 `Hybrid`를 만들지 않는다.

현재 제안 시스템의 method-level 분류는 다음과 같다.

| Work | Object Configuration | Surrounding Objects | Sensory Input | Tool | Contact Configuration | Object Manipulation | Object Geometry | Method |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Ours** | **Unstructured** | **Present** | **Vision+Contact** | **Dexterous Hand** | **Online** | **Combined** | **Estimated** | **RL** |

`Unstructured`와 `Present`는 intended shelf research scope를 나타낸다. Ours 행은 Intro에서 논의하는 목표 system의 방향을 분류한 것이며, policy architecture·action/controller·reward·evaluation·experiment가 확정됐다는 뜻은 아니다. 주변 물체를 제거한 단순 조건의 사용 방식과 held-out object 범위도 `[Open]`이다.

Observability와 autonomy는 현재 비교 집합에서 구분력이 작아 제외한다. Human demonstration은 training source이지 manual execution이 아니다. Task와 Evaluation은 Sections 2–3과 9에서 관리하며 이 표에는 넣지 않는다.

세부 observation·action, training source, controller와 결과는 근거 문서에 보존하고 비교표는 위 여덟 column으로 유지한다. 향후 직접 비교를 논의한다면 여덟 조건의 차이를 먼저 확인해야 하며, 실제 비교군과 통제 방식은 아직 정하지 않았다.

### 10.4 문헌 확인과 포함 기준

- Full text에서 `Object Configuration`, `Surrounding Objects`, `Sensory Input`, `Tool`, `Contact Configuration`, `Object Manipulation`, `Object Geometry`, `Method`를 확인한 뒤 C1·C2와의 관계를 해석한다.
- `Contact Configuration`은 formal state/action 정의와 reported execution behavior를 함께 확인한다. 둘이 완전히 대응하지 않으면 system-level capability와 command provenance를 분리해 기록하며, 보고되지 않은 action channel을 임의로 learned output으로 추론하지도 `Constant`의 증거로 사용하지도 않는다.
- 타 논문은 `확인 완료 / 추가 확인 필요 / 확인되지 않음`으로 기록하고, 우리 연구의 `[Fixed] / [Baseline] / [Open] / [Candidate]` 상태와 혼용하지 않는다.
- 공식 출판 페이지와 full text를 먼저 확인한다.
- DOI는 publisher metadata와 대조한다. DOI가 없으면 `No DOI—preprint` 또는 공식 proceedings URL로 표시한다.
- Title/abstract만으로 observation, reward, online feedback 또는 surrounding-object capability를 추론하지 않는다.
- ArXiv-only work는 task directness와 연구 조직의 최신 방향을 보여주는 경우에 제한적으로 사용하고 peer-reviewed 근거와 구분한다.
- Tactile/F/T 사용 유무만으로 우열이나 novelty를 판단하지 않는다.
- Dense clutter, surrounding-object contact와 collision avoidance를 다룬 연구를 의도적으로 제외하지 않는다.
- `Planning/Control`, `RL`, `IL`, `VLA`를 균형 있게 포함하되, data·compute·sensor 조건이 다른 방법의 raw success rate를 직접 비교하지 않는다.
- 최근 연구를 우선하되 reward·contact mechanics의 직접 근거가 되는 오래된 연구는 기초 비교 연구로 별도 표시한다.

### 10.5 균형 잡힌 초기 후보군

이 표는 문헌 비교표를 채울 출발점일 뿐, 실제 baseline이나 비교군을 선정한 표가 아니다.

| 비교 역할 | 세부 접근 | 대표 연구 | 포함 이유 | 상태 |
| --- | --- | --- | --- | --- |
| Simple heuristic reference | Heuristic | Nearest feasible OBB face 선택 + scripted normal alignment + straight push | 학습 방식과 구분되는 단순 접근의 참고 후보; 실제 비교군 채택 여부는 미정 | `[Open]` |
| Geometry/contact selection | Learning-based contact selection | [Learning Contact Locations for Pushing and Orienting Unknown Objects](https://doi.org/10.1109/HUMANOIDS.2013.7030011) | Straight push와 orienting contact의 관계를 다룬 기초 비교 연구 | `[Open]` |
| Dexterous pose planning | Optimization | [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652) | Task wrench에 조건화된 hand pose 평가 | `[Open]` |
| Dexterous pose planning | Generative geometry-conditioned | [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | ICRA 2026; direction-conditioned dexterous pre-contact pose | `[Open]` |
| Tactile pushing | Goal-conditioned RL | [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236) | Vision 없이 tactile feedback으로 pushing을 제어한 직접 사례 | `[Open]` |
| Object pushing | Constrained RL | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166) | Unknown-object pushing과 task/safety 분리 | `[Open]` |
| Cluttered pushing | RL | [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) | Surrounding-object collision을 포함한 pushing | `[Open]` |
| Reactive feedback | IL | [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052) | IL도 high-frequency tactile/force feedback에 반응할 수 있다는 강한 반례 | `[Open]` |
| Force feedback and runtime re-grasp | VLA + hybrid control | [ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169) | Force target·control mode를 생성해 active force regulation에 연결하고 Retrieve Plate에서 autonomous retry/re-grasp를 수행한다는 반례. Formal action에는 gripper aperture가 없어 learned policy·subtask script·별도 controller 중 command source는 미보고 | `[Open]` |
| Tactile grounding and adaptation | VLA + control | [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) | Tactile history를 target-force action, hybrid position–force control과 failure reasoning에 연결할 수 있다는 반례; preprint 상태를 명시 | `[Open]` |
| 관련 VLA 연구 | VLA | [OpenVLA-OFT](https://doi.org/10.15607/RSS.2025.XXI.017) | VLA latency와 adaptation 개선을 확인하기 위한 참고 논문 | `[Open]` |
| Online configuration adaptation | Residual policy | [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744) | Task-informed initial grasp와 residual joint adaptation의 결합 | `[Open]` |

[`Intro/previous_works.md`](./Intro/previous_works.md)의 현재 비교표는 확인한 full text를 바탕으로 계속 갱신한다. 위 문헌 후보나 비교표만으로 실제 비교군 또는 novelty를 확정하지 않는다.

---

## 11. 연구 주장과 남은 질문

선행연구의 결과와 그 결과를 바탕으로 Intro에서 남은 질문을 구분한다. 이 표는 method·evaluation·experiment 계획을 정하지 않는다.

| Intro의 주장 또는 질문 | 선행연구에서 확인한 내용 | 아직 남은 질문 | 상태 |
| --- | --- | --- | --- |
| Direct push만으로 처리하기 어려운 상태에는 preparatory orientation/contact change가 필요할 수 있다. | [Learning Contact Locations](https://doi.org/10.1109/HUMANOIDS.2013.7030011)은 pushing과 orienting을 위한 contact location을 구분하고, [Learning Generalizable Pivoting](https://doi.org/10.1109/ICRA48891.2023.10161271)은 target-orientation pivoting을 학습한다. | Shelf blocker에서 rotation이 실제 final Push 가능 영역을 넓히는 조건은 무엇인가? | `[Hypothesis]` |
| 좋은 contact formation은 접촉 여부보다 task goal에 대한 실행 가능성과 연결되어야 한다. | [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652)은 task wrench에 맞는 hand pose를, [RL-Critic Grasp Selection](https://doi.org/10.1109/ICRA55743.2025.11127792)은 downstream manipulation critic으로 grasp를 평가한다. | Static/task-local score와 Approach→Rotation→Push의 관계를 어떻게 정의할 것인가? | `[Hypothesis]` |
| Vision/coarse geometry와 contact sensing은 서로 다른 정보를 제공한다. | [Coarse-to-Fine Pushing](https://doi.org/10.1109/LRA.2024.3511378)은 vision과 touch/proprioception의 역할을 나누고, [DexTouch](https://doi.org/10.1109/LRA.2024.3478571)은 tactile feedback의 sim-to-real utility를 보였다. | Coarse OBB와 binary tactile+wrist F/T가 geometry error 아래에서 어떤 역할을 하는가? | `[Hypothesis]` |
| Tactile와 F/T의 추가가 자동으로 상보성을 뜻하지는 않는다. | [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052), [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160)와 [ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169)는 IL/VLA도 tactile/force feedback과 runtime correction을 사용할 수 있음을 보인다. | 저차원 binary tactile와 wrist wrench의 독립·결합 효과를 어떤 방식으로 판단할 것인가? | `[Hypothesis]` |
| Task-conditioned initial configuration과 online adaptation은 구분할 필요가 있다. | [GD2P](https://doi.org/10.48550/arXiv.2509.18455)는 direction-conditioned pre-contact pose를, [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744)는 initial grasp와 residual adaptation을 결합한다. | Nonprehensile Rotation→Push에서 online wrist–finger adaptation의 추가 가치는 무엇인가? | `[Hypothesis]` |
| Rotation success와 subsequent Push feasibility는 동일하지 않을 수 있다. | [Value-Informed Skill Chaining](https://doi.org/10.1109/IROS55552.2023.10342180)과 [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987)는 transition feasibility의 중요성을 보여준다. | 두 결과를 구분할지, 구분한다면 어떤 정의와 방법을 사용할 것인가? | `[Open]` |
| Safety와 task objective의 관계를 정해야 한다. | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166)은 task reward와 collision·toppling constraint를 분리한다. | Shelf structure, multi-finger force와 boundary condition을 어떤 규칙으로 다룰 것인가? | `[Open]` |
| Intended shelf setting에는 주변 물체가 존재한다. | [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873)은 clutter collision을 포함한 goal pushing을 다룬다. | 주변 물체의 수·배치·관측 범위와 접촉 역할을 어떻게 정할 것인가? | `[Open]` |
| Explicit downstream learning이 필요한지는 확인되지 않았다. | 기존 skill-chaining·critic 연구는 explicit transition/value modeling의 가능성을 제공한다. | Shared return, 별도 objective와 평가 방법 중 무엇이 필요한가? | `[Open]` |

연결되지 않은 요소에 대한 현재 판단은 다음과 같다.

- Previous action, exact quaternion encoding과 tactile threshold는 observation 구현 세부다. Reward gate는 별도의 `[Open]` 항목이며 같은 합의 상태로 묶지 않는다.
- 주변 물체의 존재는 intended scope다. 단순화한 S0를 실제 비교 조건으로 사용할지와 surrounding-object robustness·접촉 활용을 어떤 방식으로 판단할지는 협의 전이므로 contribution에 포함하지 않는다.
- `후속 단계를 고려한다`고 쓸 때는 shared return, transition evaluation 또는 별도 learning objective 중 무엇을 뜻하는지 명시한다.

---

## 12. 용어와 주장 범위

### 12.1 주요 용어

| 용어 | 이 문서에서의 의미 |
| --- | --- |
| Nonprehensile manipulation | 안정적인 grasp/lift 없이 pushing, pulling 또는 pivoting 등의 접촉으로 물체를 이동·재배치하는 본 연구의 task category |
| Contact-rich interaction/control challenge | 접촉 위치·마찰·stick–slip–separation과 힘 전달의 불확실성을 sensing과 feedback control로 다루는 실행상의 challenge; 본 문서의 task category가 아님 |
| Blocker manipulation | Target access를 방해하는 물체를 재배치하는 Stage 1의 직접 과업 |
| Approach / Contact Formation | 다음 조작을 시작할 object–hand relation과 contact state를 형성하는 구간; 단순 거리 감소와 동의어가 아님 |
| Rotation / Pivoting | Push에 필요한 object/contact orientation을 형성하는 preparatory manipulation |
| Push / Translation | 정렬을 유지·보정하며 지정 방향·거리로 blocker를 이동하는 구간 |
| Task-conditioned hand configuration | Contact 수가 아니라 selected manipulation goal에 조건화된 wrist–finger/contact state |
| Approximate geometry | 실제 국소 접촉 표면과 오차를 허용하는 명시적 coarse representation |
| Exact geometry | Pose와 정확히 정합된 CAD/mesh 또는 calibrated dimensions |
| Vision/coarse-geometry information | Blocker pose, nominal shape, task goal과 접근 관계를 제공하는 전역 정보; 실제 contact state를 직접 보장하지 않음 |
| Contact feedback | Tactile과 wrist F/T를 통해 실제 contact event/location 및 net wrench 변화를 관측하여 geometry·physics mismatch에 대응하는 실행 feedback |
| Online adaptation | Contact 이후 sensory feedback에 따라 wrist 또는 finger action을 계속 변경하는 넓은 개념 |
| Tool | 물체와 직접 접촉하는 장치. 내부 접촉 자유도가 없는 `Pusher`, coupled aperture의 `Gripper`, 독립 finger joint의 `Dexterous Hand`로 구분 |
| Contact Configuration | Aperture·finger posture 같은 tool 내부 구성을 언제 결정·갱신하는지 나타내는 비교 축. Wrist/EEF pose와 passive compliance는 제외 |
| Contact Configuration=`Online` | 배포 실행 중 내부 구성을 다시 갱신할 수 있는 경우. Feedback 기반 continuous multi-joint update와 re-open/close·retry·re-contact·re-grasp를 포함하며, update trigger나 command source가 불명확하면 근거 주석으로 남김 |
| Aggregate contact maintenance | 동일 contact set을 고정하지 않고 적어도 하나의 hand–object support contact를 선호하는 것 |
| Transition evaluation | 앞 phase 종료 state에서 다음 phase rollout 성공을 별도로 측정하는 것 |
| Explicit downstream learning | 다음 phase feasibility를 별도 value/reward/objective로 직접 최적화하는 것 |
| Ours | Previous Works 비교표에서 현재 제안 연구 시스템을 지칭하는 행 이름; 개별 설계가 확정되었거나 contribution이 검증되었다는 뜻은 아님 |
| Baseline | 합의 범위 안에서 현재 출발점으로 둔 변경 가능한 안. 현재 비교군·평가·실험 baseline이 정해졌다는 뜻은 아님 |

### 12.2 Actor observation과 이후 설계의 경계

- **Actor observation:** 실제 deployment에서 같은 의미로 획득 가능한 policy input.
- **Privileged-information candidate:** Simulator에서만 얻을 수 있어 actor observation에서 제외하는 exact state. Reward, constraint, termination 또는 critic에 실제로 사용할지는 `[Open]`이다.
- **Evaluation candidate:** 향후 reward와 독립적으로 정의할 수 있는 측정값. 구체 metric과 protocol은 `[Open]`이다.
- `[Rejected]` Reward에 들어갔다는 이유로 해당 정보가 actor observation이라고 표현하는 방식.
- `[Rejected]` Privileged contact force로 학습했다는 이유로 실물 tactile의 continuous force를 관측한다고 표현하는 방식.

### 12.3 주장 범위

| Previous/tempting statement | Updated boundary | Status |
| --- | --- | --- |
| 연구의 task category는 contact-rich manipulation이다. | Task category는 `nonprehensile manipulation`이고, contact-rich는 접촉 불확실성을 다루는 interaction/control challenge다. | `[Superseded]` |
| Continuous vision과 OBB가 있으므로 fully observable하다. | Pose·coarse geometry만 관측되며 contact/physics uncertainty가 남는다. | `[Superseded]` |
| Marker가 pose와 exact geometry를 모두 제공한다. | Marker는 pose source이며 geometry source·accuracy는 별도 정의한다. | `[Rejected]` |
| Selected-face goal이 연구의 확정 인터페이스다. | 현재 actor observation의 변경 가능한 작업 기준이며 explicit rotation goal의 필요성은 열어 둔다. | `[Baseline]` |
| 66D observation과 12D action이 모두 합의된 연구 설계다. | 66D actor observation은 현재 합의안이다. 12D action/controller는 협의 중이다. | Observation `[Baseline]`; action `[Open]` |
| Shared MLP와 phase-ID 제거가 method contribution이다. | Policy organization과 phase 정보 사용 여부는 아직 협의 중이다. | `[Open]` |
| Full-episode reward를 공유하므로 explicit downstream feasibility를 학습한다. | Shared return, transition evaluation과 별도 learning objective를 구분한다. | `[Superseded]` |
| Rotation alignment success가 다음 Push 가능성을 의미한다. | 두 개념은 구분할 필요가 있으나 구체 평가 방식은 협의 중이다. | `[Rejected]` |
| Approach contact를 이후에도 동일 taxel/finger로 유지해야 한다. | Contact 유지·전환 규칙은 아직 협의 중이며 특정 contact pattern을 합의하지 않았다. | `[Rejected]` |
| Tactile/F/T를 사용한다는 점이 novelty다. | 센서의 상보적 효과와 robustness gain이 H3에서 입증될 때만 method claim의 일부가 된다. | `[Rejected]` |
| 후속 단계를 고려한 contact formation은 현재 contribution이다. | 어떤 학습·평가 방법을 뜻하는지 밝히고 H2에서 downstream success 향상이 입증될 때만 contribution 후보를 확정한다. | `[Rejected]` |
| IL은 reactive하지 않고 VLA는 force를 사용하지 못한다. | 최신 reactive IL과 force/tactile VLA가 반례다. Track B의 좁은 execution gap만 비교한다. | `[Rejected]` |
| RL은 다른 manipulation 방법보다 일반적으로 우월하므로 선택한다. | Intro에서는 현재 자원 조건에서 RL을 우선 검토할 조건부 이유만 제시한다. 세부 algorithm, 학습 방식과 비교·검증 방법은 협의 중이다. | Method family `[Baseline]`; 세부 설계 `[Open]` |
| Previous Works 표의 제안 연구 행을 `Our baseline`이라 부른다. | 제안 연구 전체는 `Ours`로 부른다. 현재 `baseline`은 합의 범위 안의 변경 가능한 observation 기준에만 사용하며, 실제 비교 baseline은 아직 정하지 않았다. | `[Superseded]` |
| OBB는 mesh/point cloud보다 현실적으로 항상 우월하다. | OBB는 현재 observation 표현일 뿐 다른 geometry 표현과의 비교 방법은 정하지 않았다. | `[Rejected]` |
| 최초 접촉 후 hand reconfiguration은 최소여야 한다. | Hand reconfiguration의 허용 범위와 효율 판단 방법은 협의 중이다. | `[Open]` |
| Task-conditioned contact formation, wrist–finger control 또는 `Contact Configuration=Online` 자체가 novelty다. | GD2P·DexMove와 인접 연구가 관련 구성을 이미 보여준다. C2의 구체 method와 입증 조건은 아직 협의 중이다. | `[Rejected]` |
| Formal learned-action 식에 gripper command가 없으면 deployed contact configuration은 `Constant`다. | Reported autonomous execution 중 re-open/close·retry·re-contact·re-grasp가 확인되면 system-level capability는 `Online`으로 판정하고, learned policy·script·별도 controller 중 command provenance가 불명확하다는 사실은 별도 주석으로 남긴다. | `[Superseded]` |
| Surrounding objects는 모두 forbidden collision이다. | Intended setting에는 주변 물체가 존재하지만 수·배치·관측 범위, 접촉 규칙과 단순화 조건의 사용 방식은 협의 중이다. | `[Superseded]` |

### 12.4 근거 확보 전 사용할 표현

사용 가능한 표현:

- `본 연구는 …이라는 질문을 다룬다.`
- `현재 합의된 actor observation은 …으로 구성한다.`
- `기존 연구는 자신의 설정에서 …을 보였으며, 본 연구에는 …이 남는다.`
- `충분한 근거가 마련될 경우 contribution 후보는 …이다.`

사용하지 않는 표현:

- `본 방법은 기존 VLA/IL/RL의 한계를 극복한다.`
- `Tactile과 F/T를 사용하므로 robust하다.`
- `Shared policy이므로 phase transition을 학습한다.`
- `Unseen object에 일반화한다.` — unseen split이 정의되고 결과가 나오기 전에는 금지.

---

## 13. 현재 작업 경계와 이후 협의

### 13.1 합의된 범위

- Intro의 문제 정의, 연구 흐름과 previous-works 비교 관점
- Track B actor observation의 구성과 정보 경계

### 13.2 협의 중인 범위

- Policy architecture와 phase 처리
- Action과 low-level controller
- Reward, safety constraint, termination, critic과 RL 세부 algorithm
- Evaluation metric·success criterion·protocol
- Baseline comparison, ablation과 experiment 구성
- Contribution 후보를 최종 주장으로 확정하는 조건

이 목록은 작업 순서를 정한 것이 아니다. 다음 논의에서는 필요한 항목을 하나씩 합의하고, 합의 전의 아이디어를 구현안이나 실험 계획으로 승격하지 않는다.

---

## 14. Change Log

### 14.1 Current restructuring

같은 날짜의 변경은 위에 있을수록 최신이다. 아래 기록의 중간안이 현재 정의와 충돌하면 가장 위의 최신 정의를 따른다.

| Date | Previous Definition | Updated Definition | Reason | Affected Sections |
| --- | --- | --- | --- | --- |
| 2026-09-24 | Reward·evaluation 후보와 E1–E9가 합의된 baseline 및 experiment plan처럼 본문에 기록됨 | 현재 합의 범위를 Intro와 actor observation까지로 복원. Policy architecture, action/controller, reward·termination·critic·evaluation·experiment는 모두 `[Open]`으로 두고 E1–E9 번호와 당시의 확정형 실험표를 현행 본문에서 철회함. Reward·evaluation 관련 아이디어는 채택안이 아닌 `[Open]` 논의 기록으로만 보존 | 사용자가 실험 계획을 세운 적이 없고 observation 이후는 아직 협의 중이라고 명확히 정정함 | 1, 3, 5–14, `README.md`, `research_topic.md`, `policy_learning.md`, `Intro/` 전체, `papers/README.md`, `papers/core_papers.md`, `papers/reading_guide.md`, `papers/reward_formulation.md`, `papers/topic_groups.md`, `papers/screening/` |
| 2026-09-24 | `policy_learning.md`와 reward 문서가 pillar·주변 물체 접촉을 모두 금지하고, selected-face goal·phase-free shared policy·sensor complementarity를 확정안처럼 서술. Hand 비교도 `fixed/free adaptation`을 사용 | S0와 intended S1을 구분하고 support contact는 항상 허용하며 auxiliary fixed structure·movable-object contact는 OD-1의 open decision으로 복원. Selected-face goal, shared phase-free policy, 66D observation과 RL을 `[Baseline]`으로 명시하고 E3/E5/E6/E8/E9 검증에 연결. Hand 비교는 `Constant / Pre-contact / Online`으로 통일 | Canonical status와 구현·reward 명세의 충돌을 제거하고, 미검증 설계와 contribution을 구분하기 위해 | 3.2, 3.6, 6–9, 12–14, `README.md`, `research_topic.md`, `policy_learning.md`, `Intro/research_trend.md`, `papers/reward_formulation.md`, `papers/topic_groups.md` |
| 2026-09-24 | 일부 Intro·reading 문서가 이전 7-column 비교표와 `End-effector Reconfiguration` 용어를 사용하고, 주변 물체의 존재 자체를 open으로 서술. C1의 geometry–sensing interaction과 RL 선택의 실험적 타당성도 E1–E7에 직접 연결되지 않음 | 모든 현재 문서를 8-column `Object Configuration·Surrounding Objects / Sensory Input·Tool·Contact Configuration·Object Manipulation / Object Geometry·Method`로 동기화. Intended S1의 주변 물체 존재와 S0 removal ablation을 구분하고, C2를 `Contact-formation objective × Contact Configuration`으로 통일. E5를 geometry×sensor 교차 실험으로 명시하고 matched method-family E8과 Sim-to-Real E9를 추가 | 최신 기준 문서와 파생 문서의 충돌을 제거하고 C1·C2 및 조건부 RL 선택을 실제 검증 계획에 연결하기 위해 | 7.1, 8–9, 10.3, 13–14, `research_topic.md`, `Intro/README.md`, `Intro/research_motivation.md`, `Intro/research_trend.md`, `Intro/contributions.md`, `papers/reading_guide.md`, `papers/topic_groups.md` |
| 2026-09-24 | Conventional methods와 IL/VLA의 한계를 열거한 뒤 simulation과 recovery를 RL의 장점으로 바로 연결하고, broad trend에서도 planning/control을 RL·IL·VLA와 병렬인 family처럼 표현 | Planning/control은 공통 기능으로 두고 task-level decision source에 따라 `Conventional / IL·VLA / RL`을 비교. Specification·recovery-data 부담만으로 RL을 결론 내리지 않고 `measurable outcome + safe·affordable reset + representative simulation`을 추가 전제로 명시하며, interaction·reward/credit·exploration·Sim-to-Real 부담과 반증 조건을 함께 기록 | 기존 방법의 약점에서 RL로 건너뛰는 논리 비약을 제거하고, generative/reactive IL과 simulation teacher의 반례를 포함한 조건부 method selection으로 만들기 위해 | 6.1, 10.1, 10.3, 13, 14.1, `Intro/research_trend.md` |
| 2026-09-21 | Scene·Workspace와 Sensing 등 넓거나 중첩된 항목을 사용하고, tool 내부 구성은 `End-effector Reconfiguration=Fixed / Pre-contact / Online`, Method는 Control·Optimization·Generative를 각각 분리 | 비교표를 여덟 축으로 통일: Environment의 `Object Configuration·Surrounding Objects`, Robot Agent의 `Sensory Input·Tool·Contact Configuration·Object Manipulation`, System의 `Object Geometry·Method`. `Contact Configuration=Constant / Pre-contact / Online`, `Method=Planning/Control / RL / IL / VLA`를 사용하며 full shelf는 `Surrounding Objects=Present`, S0는 removal ablation으로 고정 | 짧은 용어로 물체 형상, 주변 방해 물체, 접촉 장치와 그 제어 권한, 대상 물체 운동 및 핵심 방법 concept을 중복 없이 비교하기 위해 | 3.6, 7.1, 10.1, 10.3–10.5, 11–13, `Intro/previous_works.md` |
| 2026-09-20 | B99 ForceVLA2를 formal learned-action 식에 gripper-aperture command가 없다는 이유로 `End-effector Reconfiguration=Fixed`로 해석 | Retrieve Plate의 autonomous retry/re-grasp를 근거로 deployed-system capability는 `Online`으로 정정. Learned policy, subtask script 또는 별도 controller 중 aperture-command source는 `미보고`로 주석하며, low-dimensional re-grasp와 Ours의 continuous multi-joint reconfiguration은 구분 | 비교 축은 hardware 이름이나 action 식의 보고 범위가 아니라 실행 중 실제 internal configuration update capability이며, reporting omission은 `Fixed`의 증거가 아니기 때문 | 10.2–10.5, 11, 12.1, 12.3, 14.1 |
| 2026-09-20 | Intro에 정리된 broad RL 선택 논리, 환경 접촉의 역할 구분과 C2의 결합 검증 원칙 일부가 `context.md`에는 압축되어 있었음 | 비유일한 trajectory의 outcome optimization, failure/recovery simulation, sequential outcome coupling과 inference amortization을 RL의 조건부 강점으로 명시. Support-surface·auxiliary fixed-structure·movable-object contact를 구분하고, C2를 `contact objective × online action authority` matched factorial comparison으로 정의 | Introduction의 최신 method-selection 논리와 canonical decision record를 일치시키되 RL·online reconfiguration 자체를 novelty로 과장하지 않기 위해 | 1.3, 3.6, 6.1, 7.1, 8–9, 12.3, 14.1 |
| 2026-09-20 | Physical tool을 `Pusher / Gripper / Dexterous Hand`로 구분하는 별도 column을 검토 | Robot Agent에 `End-effector Reconfiguration = Fixed / Pre-contact / Online`을 추가. Wrist pose를 제외한 gripper aperture·finger posture를 deployed method가 언제 결정·갱신하는지 판정하며, fixed-configuration gripper와 rigid pusher는 모두 `Fixed`로 분류 | Hardware 이름보다 contact-interface의 internal action authority가 method concept과 C2의 initial selection–online adaptation 차이를 더 직접적으로 보여주며, `Gripper`와 `Dexterous Hand`의 용어 중첩도 피하기 위해 | 10.1, 10.3–10.4, 14.1, `Intro/README.md`, `Intro/research_motivation.md`, `Intro/research_trend.md`, `Intro/previous_works.md`, `Intro/contributions.md`, `papers/reading_guide.md` |
| 2026-09-20 | Method를 `Non-learning / Learning / Hybrid`로 묶어 RL·IL·VLA와 action 생성 구조의 차이가 가려짐 | 배포 시 주된 action-generation family를 기준으로 `Control / Optimization / Generative / RL / IL / VLA`로 분류. 보조 estimator·demonstration·low-level controller는 별도 Method 값을 만들지 않음 | 논문의 method concept을 비교하면서도 `VLA+IL`, `RL+Optimization`처럼 중복 category가 늘어나는 것을 막기 위해 | 10.3, 14.1, `Intro/previous_works.md` |
| 2026-09-20 | Scene을 `Single / Cluttered`로 분류해 target 수와 주변 clutter capability가 혼재 | Scene을 `Uncluttered / Cluttered`의 이진 capability로 변경. 주변 movable clutter를 명시적으로 다루는 method는 `Cluttered`, 그렇지 않으면 `Uncluttered`로 판정하며 `Mixed`는 두지 않음 | Cluttered capability가 uncluttered scene을 대체로 포함하고, 이 표는 평가 구성보다 method가 다루는 최대 scene complexity를 비교하기 위해 | 10.3, 14.1, `Intro/previous_works.md`, `Intro/research_trend.md` |
| 2026-09-20 | Previous Works의 force-aware adjacent representative로 B46 ForceVLA를 사용 | ForceVLA를 직접 antecedent와 baseline으로 두고 force target·control-mode output까지 확장한 CVPR 2026 B99 ForceVLA2로 교체. B46은 연구 계보를 위해 core corpus에 유지 | 현재 force-aware VLA의 method concept이 passive force conditioning에서 active hybrid force–position regulation으로 발전한 범위를 반영하기 위해 | 10.2, 10.5, 11, 14.1, `Intro/previous_works.md`, `Intro/research_trend.md`, `papers/README.md`, `papers/core_papers.md`, `papers/topic_groups.md` |
| 2026-09-20 | Research Trend가 broad 논문과 Previous Works 10편을 함께 넣은 2021–2026 timeline을 보유 | B85를 closest comparison에 추가해 11편으로 만들고, 이 11편만의 2022–2026 timeline을 Previous Works로 이동. Research Trend는 기존 방법의 해결 범위와 한계를 근거로 `왜 RL인가`를 설명 | Chronology와 closest-work 비교를 한 문서에 연결하고, Research Trend는 method-selection argument에 집중하기 위해 | 10.1, 10.3, 14.1, `Intro/README.md`, `Intro/research_trend.md`, `Intro/previous_works.md`, `papers/README.md`, `papers/reading_guide.md` |
| 2026-09-20 | Previous Works 비교표 10편 중 B12와 B48이 Research Trend timeline에서 누락되고, 나머지 논문도 두 문서 간 식별자가 연결되지 않음 | B12와 B48을 해당 연도에 추가하고 Previous Works 10편 모두에 동일한 `B-ID`를 표시. Conference edition과 proceedings 연도가 다른 B12는 `CoRL 2024, PMLR 2025`를 함께 표기 | 가까운 비교 연구가 broader trend에서 차지하는 시간적·방법론적 위치를 빠짐없이 추적하기 위해 | 10.1, 14.1, `Intro/research_trend.md`, `Intro/previous_works.md` |
| 2026-09-20 | Research Trend의 2021–2026 연도별 timeline을 네 개의 방법론적 변화 표로 대체하고, RL 선택 이유를 현재 task 조건 중심으로 압축 | 연도별 timeline을 복원하고 `nonprehensile manipulation의 구조적 난제 → 방법별 자원과 부담 → interaction outcome을 이용한 RL의 조건부 강점 → 현재 task 적용`으로 재구성. 각 주장에 planning/control, RL, IL/VLA와 hybrid의 직접 근거·반례를 연결 | 분야의 시간적 변화와 broad method-selection 논리를 함께 보여주고, tactile feedback·recovery가 RL만의 기능이라는 과장을 피하기 위해 | 10.1, 14.1, `Intro/research_trend.md` |
| 2026-09-20 | Geometry 관련 column 이름으로 `Geometry Handling`과 `Object Generalization`을 검토 | Robot Agent가 실행 중 받는 명시적 형상 정보의 유무와 정확도를 나타내는 `Object Geometry = None / Estimated / Exact`로 확정 | 처리 능력과 unseen-object 일반화를 섞지 않고 하나의 기준에서 직관적으로 비교하기 위해 | 10.3–10.4, `Intro/previous_works.md`, `Intro/README.md`, `Intro/research_motivation.md`, `Intro/research_trend.md`, `Intro/contributions.md`, `papers/reading_guide.md` |
| 2026-09-20 | Environment, Robot Agent와 System을 상세한 open-ended column 및 별도의 Task·Evaluation 표로 비교 | 비교표를 Environment의 `Scene·Workspace`, Robot Agent의 `Sensing·Object Geometry·Manipulation`, System의 `Method`로 제한하고 각 column은 2–3개의 공통 값만 사용. `Object Geometry`는 `None / Estimated / Exact`로 판정 | 논문마다 서로 다른 세부 설명을 나열하지 않고 한눈에 비교하며, blocker observability와 autonomy처럼 현재 구분력이 없는 항목을 제외하기 위해 | 10.1–10.4, 14.1, `Intro/` 전체, `papers/reading_guide.md` |
| 2026-09-20 | Environment에 task·pose·shape·평가 변화를, System에 goal·단계·endpoint를 함께 넣어 세 구분의 의미가 넓어짐 | 당시 중간안에서는 Environment를 workspace·scene·environment contact, Robot Agent를 robot body·input modality·action, System을 method로 한정하고 Task와 Evaluation을 별도 표로 관리했다. 이 구성은 이후 당시 여섯-column 비교표로 대체됐다. | 사용자가 Agent는 환경과 물리적으로 상호작용하는 robot agent이고 System은 method를 뜻한다고 명확히 했으므로, 서로 다른 종류의 정보를 같은 column group에 섞지 않기 위해 | 3.6–3.7, 10.1–10.4, 14.1, `research_topic.md`, `Intro/` 전체, `papers/reading_guide.md`, `papers/screening/task_and_retrieval.md` |
| 2026-09-20 | Environment–Agent–System을 복원했지만 최종 비교를 geometry·feedback·configuration·action generation·stage organization의 다섯 항목으로 다시 축약해 pose와 shape, tactile과 wrist F/T, initial selection과 online action, stage responsibility와 endpoint가 합쳐졌고, policy 시작 위치와 상위 planner의 경계도 드러나지 않음 | 당시 중간안에서는 Environment에 task·시작 상태·pose·shape·평가 변화까지, Agent에 contact interface·실행 입력·action을, System에 goal·단계·feedback·action/controller를 기록했다. 이 분류는 이후의 중간안을 거쳐 당시 여섯-column 비교표로 대체됐다. | 비교 기준이 method 설명에 종속되면서 이전에 합의한 문제 조건, agent capability와 결과 수준이 사라지는 문제를 바로잡기 위해 | 7.4, 10.1–10.4, 14.1, `Intro/README.md`, `Intro/research_trend.md`, `Intro/previous_works.md`, `Intro/contributions.md` |
| 2026-09-20 | GD2P를 preprint로 표시한 문서와 ICRA 2026으로 표시한 논문 목록이 함께 존재 | [공식 project page](https://geodex2p.github.io/)를 기준으로 ICRA 2026 게재로 통일 | 문서 간 publication-status 충돌을 제거하기 위해 | 10.5, 14.1, `Intro/research_trend.md`, `Intro/previous_works.md`, `papers/core_papers.md` |
| 2026-09-20 | Intro 구조를 설명하는 별도 표현과 임의의 약자가 늘고, 합의되지 않은 C1→C2 검증 순서가 문서에 포함됨 | Environment–Agent–System은 줄이지 않고 그대로 쓰며, 문서의 질문과 결론을 평이한 문장으로 정리하고 C1·C2의 검증 순서는 미정으로 유지 | 연구 내용보다 문서 관리 표현이 앞서는 문제를 막고 실제 결정 상태를 정확히 보존하기 위해 | 10.1–10.5, 14.1, `Intro/` 전체 |
| 2026-09-19 | Intro 문서마다 연구 정의, 방법 비교, C1·C2, 평가와 작업 목록을 반복해 한 수정이 여러 파일에 중복 반영됨 | `README`는 문서 구성, `research_motivation`은 문제 정의, `research_trend`는 조건부 방법 선택, `previous_works`는 선행연구 비교와 남은 질문, `contributions`는 C1·C2의 검증 조건을 다루도록 정리하고 반복 내용을 줄임. Environment–Agent–System은 약자로 줄이지 않고 문서 구조는 일반 문장으로 설명 | 잦은 수정으로 문서가 서로 달라지는 문제를 줄이고 Introduction을 `문제 → 방법 선택 → 근거 → 검증할 주장`의 흐름으로 읽히게 하기 위해 | 14.1, `Intro/README.md`, `Intro/research_motivation.md`, `Intro/research_trend.md`, `Intro/previous_works.md`, `Intro/contributions.md` |
| 2026-09-19 | 다섯 항목으로 줄이는 과정에서 Environment–Agent–System 구분과 sensor·tool·action의 세부 정보가 빠짐 | 논문별 조건은 Environment–Agent–System으로 기록하고, C1·C2와 관련된 차이는 다섯 비교 항목으로 별도 정리 | 다섯 항목만으로는 environment, embodiment, sensing과 action-authority 차이가 가려지고, Environment–Agent–System만으로는 부품 조합을 contribution으로 오해할 수 있기 때문 | 10.3–10.4, `Intro/README.md`, `Intro/research_trend.md`, `Intro/previous_works.md`, `Intro/contributions.md` |
| 2026-09-19 | Previous Works 표가 sensor·tool·action과 평가 수준을 각각 독립 열로 나열해 방법의 핵심 차이가 흐려짐 | Geometry representation, contact feedback, contact configuration, action generation, manipulation organization의 다섯 항목으로 정리하고, modality·embodiment·action은 필요한 곳에 덧붙임. 평가 환경과 주변 접촉은 Environment 표에 유지 | Ours가 기존 연구와 다른 지점을 구현 부품 수가 아니라 `approximate explicit geometry + closed-loop contact correction + online wrist–finger adaptation + multi-stage execution`의 구조로 설명하기 위해 | 7.1, 10.3, `Intro/previous_works.md` |
| 2026-09-18 | Research Trend의 방법별 한계를 approximate OBB, 현재 sensor와 sequential task에 필요한 추가 요소로 직접 설명 | Nonprehensile manipulation의 공통 난제인 underconstrained motion, hybrid contact-mode transition, model/data coverage, closed-loop recovery와 safety를 기준으로 방법별 한계를 먼저 비교하고 현재 조건의 RL 선택은 별도 절로 분리 | Method 자체의 한계와 현재 구현 조건에서 부족한 부분을 혼동하지 않고 공정한 trend comparison을 만들기 위해 | 10.1, `Intro/research_trend.md` |
| 2026-09-18 | Rotation success와 Rotation-to-Push success를 나누는 평가를 C3 contribution 후보로 분류하고 unified shared policy도 C4 후보로 병기 | 활성 contribution 후보는 C1·C2만 유지하고, Rotation과 Rotation-to-Push의 분리 측정은 공통 평가로, shared policy는 method baseline으로 재분류 | 평가 설계와 contribution을 혼동하지 않기 위해 | 7.3, 8–9, 11, `Intro/README.md`, `Intro/previous_works.md`, `Intro/contributions.md` |
| 2026-09-18 | Motivation, Trend, Previous Works와 Contributions가 각자의 역할은 가졌지만 문서 전환 시 앞 단계의 결론과 다음 질문이 명시되지 않아 독립 보고서처럼 읽힘 | 각 문서를 `앞 단계의 결론 → 현재 질문 → 현재 결론 → 다음 질문`으로 연결하고, 발표 구성은 `Intro/README.md`에 집중 | 네 문서를 `문제 정의 → 조건부 방법 선택 → 남은 연구 질문 → 검증할 contribution 후보`의 흐름으로 읽히게 하기 위해 | 1.2, `Intro/README.md`, `Intro/research_motivation.md`, `Intro/research_trend.md`, `Intro/previous_works.md`, `Intro/contributions.md` |
| 2026-09-18 | Research Trend의 연도별 표, 방법별 장단점과 RL 선택 절에서 유사한 설명을 반복하고 `Direct/Adjacent`를 함께 표시; Hybrid 연구를 여러 열에 중복 배치 | 대표 연구를 다섯 방법으로 정리하고 Hybrid를 별도로 표시; GD2P를 generative/planning 흐름에 추가; `해결된 부분–추가 요구–RL 선택–반증 조건`의 순서로 설명 | 발표에서 방법론 변화와 hybridization을 함께 보이고 Previous Works의 비교와 역할이 겹치지 않게 하기 위해 | 10.1, `Intro/research_trend.md` |
| 2026-09-18 | `Comparison Role`을 main column으로 두고 geometry form/fidelity를 분리했으며, Agent에 processed geometry·goal까지 포함한 `Policy Input Modalities`를 사용; System에 task·training source·coverage·correction·endpoint를 함께 배치 | `Comparison Role` 제거; geometry는 `Geometry Information = 표현 (정확도)`로 통합; Agent는 `Tool`, raw-sensor 기준 `Input Modality`, `Action Output`, `Tool-Configuration Update`로 정리; System은 `Method`만 유지 | 표의 각 group 경계를 명확히 하고 sensor modality와 processed representation/conditioning을 혼동하지 않기 위해 | 10.3, `Intro/previous_works.md` |
| 2026-09-18 | Previous Works의 Agent와 System을 한 표에 두고 `Workspace`, `Geometry Input`, `Online Correction` 등 넓은 의미의 column을 사용; GD2P는 상세 비교표에서 누락 | Environment–Agent–System을 세 표로 분리하고, geometry form/fidelity·contact-stage coverage·reported endpoint 등 단일 판정 질문으로 교체; sensory input은 `Policy Input Modalities` 한 column으로 통합하고 GD2P를 직접 비교할 nonprehensile 연구로 추가 | 발표 가독성을 높이면서 initial hand-configuration selection, online adaptation과 sequential outcome의 차이를 모호하지 않게 비교하기 위해 | 10.3–10.5, `Intro/previous_works.md` |
| 2026-09-18 | Closest Previous Works를 주로 같은 nonprehensile task와 직접 비교 가능한 system으로 구성 | ForceVLA와 Tactile-VLA도 포함하되 같은 task의 baseline과 구분 | Task는 다르지만 force/tactile feedback을 VLA action generation과 online physical adaptation에 직접 연결한 강한 반례이므로, 기존 VLA를 과도하게 단순화하지 않기 위해 | 10–12, `Intro/previous_works.md` |
| 2026-09-17 | `왜 RL인가`를 주로 RL의 장점과 문제 조건의 대응으로 설명 | 다른 family가 해결한 범위와 단독 적용 시 남는 문제를 먼저 제시하고, simulation interaction·제한된 demonstration coverage·contact-dependent recovery 조건에서 RL을 선택하는 논리로 변경 | 다른 방법을 약한 비교 대상으로 만들지 않고 RL을 조건부 baseline 선택으로 정당화하기 위해 | 6.1, 10.1, 12.3 |
| 2026-09-17 | Previous Works 비교표의 제안 연구 행을 `Our baseline`으로 표기 | 제안 연구 행은 `Ours`로, 변경 가능한 최초 구현안과 비교 대상은 `baseline`으로 구분 | 제안 연구 전체와 provisional implementation의 의미 혼동을 방지하기 위해 | 10, 12.1–12.3 |
| 2026-09-17 | Intro 관련 논리가 root의 `motivation.md`와 `papers/`의 세부 문서에 분산 | `Intro/`를 만들고 `README.md` → `research_motivation.md` → `research_trend.md` → `previous_works.md` → `contributions.md` 순서로 재배치 | 발표 및 논문 Introduction의 독해 순서를 고정하고, 서론 논리·문헌 registry·policy 설계의 역할을 분리하기 위해 | 문서 구성, 10–13 |
| 2026-09-17 | Motivation과 Previous Works 안에서 method-family 흐름과 closest-system 비교가 혼재 | `Research Motivation → Research Trend → Closest Previous Works → Candidate Contributions`로 분리하고, trend는 RL 선택 근거, closest comparison은 contribution 검증 근거로 한정 | 발표가 broad application에서 method 선택과 좁은 research gap으로 단계적으로 수렴하도록 하기 위해 | 10–13 |
| 2026-09-17 | Shelf blocker 문제를 `contact-rich manipulation`이라는 task category로 표현 | 연구 정의를 **Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry**로 통일하고, `nonprehensile manipulation`은 task category, `contact-rich`는 interaction/control challenge로 분리 | 과업의 종류와 접촉 불확실성이라는 해결 과제를 혼동하지 않기 위해 | 1–3, 12 |
| 2026-09-17 | Previous Works를 방법 종류와 개별 sensor 중심으로 배열 | Nonprehensile manipulation과 contact-feedback-based manipulation의 두 흐름 및 교차점으로 재구성하고 각 논문의 학습 방법은 부가 정보로 기록 | Research gap이 어느 두 연구축 사이에 있는지 명확히 하기 위해 | 10–11 |
| 2026-09-17 | Coarse-geometry contact formation과 tactile/F/T adaptation이 넓은 candidate contribution으로 제시 | Geometry-error 보완, downstream contact-state quality와 online adaptation 효과가 실험으로 확인될 때만 C1/C2를 확정하도록 조건 강화 | 센서 사용과 sequential framing 자체를 novelty로 오해하지 않기 위해 | 5, 8, 11–12 |
| 2026-09-17 | Fixed scope, 가설, 66D implementation과 contribution 후보가 하나의 `최신 합의`로 혼재 | `[Fixed] / [Hypothesis] / [Baseline] / [Open] / [Candidate]`로 재분류 | 연구 정의와 현재 구현안을 분리하기 위해 | 1–13 전체 |
| 2026-09-17 | `research_topic.md`와 `policy_learning.md`의 현재안이 사실상 확정 명세처럼 읽힘 | 기존 문서는 참고용 작업 기록으로 두고, 이 문서를 분류 기준으로 지정 | 다른 문서를 수정하지 않고 판단 기준을 안정화하기 위해 | 1.1 |
| 2026-09-17 | Selected face+direction, 66D observation, shared MLP와 phase-ID 제거가 현재 연구 정의에 포함 | 모두 provisional baseline으로 이동 | 실험 결과와 hardware interface에 따라 바뀔 수 있기 때문 | 3.2, 6, 12.3 |
| 2026-09-17 | 후속 단계를 고려한다는 표현이 shared return, explicit reward와 transition metric을 혼용 | Shared return, transition evaluation과 explicit downstream learning을 구분 | 현재 explicit mechanism이 없는데 contribution처럼 보이는 문제를 제거 | 6.6, 7.3, 11, 12 |
| 2026-09-17 | Surrounding objects가 존재하는 듯 서술하면서 모두 forbidden collision로 처리 | S0/S1/S2를 open environment decision으로 분리 | 단순 obstacle인지 research variable인지 불명확했기 때문 | 3.6, 7.1, 9 |
| 2026-09-17 | Marker pose, OBB와 exact geometry의 관계가 명확하지 않음 | No/Approximate/Implicit/Exact geometry 분류와 G0–G3 비교 조건 추가 | Geometry claim과 perturbation 실험을 정의하기 위해 | 3.3, 7.2, 9, 10 |
| 2026-09-17 | Contact 유지와 fixed contact configuration이 혼동될 수 있음 | Aggregate contact preference와 contact migration/re-contact를 분리 | Online adaptation 가설과 모순을 제거 | 6.7, H4, 12 |
| 2026-09-17 | Candidate contribution이 method description과 섞임 | C1–C4를 필요한 method·실험·확정 조건과 함께 조건부로 정리 | 결과 이전의 novelty 확정을 방지 | 8 |
| 2026-09-17 | Reward term과 metric이 동일한 성공 정의처럼 사용될 수 있음 | Training signal과 independent evaluation을 명시적으로 분리 | Reward hacking과 순환 논증을 방지 | 6.5, 9 |

### 14.2 Preserved history from the previous context

기존 2170-line decision log를 조용히 폐기한 것이 아니다. 앞으로의 기준으로 필요한 변경 관계를 아래에 압축 보존하며, 세부 원문은 repository history에서 추적할 수 있다.

| Period | Preserved decision/history | Current interpretation |
| --- | --- | --- |
| 2026-09-13 | Vision-based sweeping에서 시작해 Track A/B를 구분 | Track A/B는 동작 이름이 아니라 다른 연구 방향이었으며 현재는 Track B Stage 1만 구체화 |
| 2026-09-14 | Low-level execution을 먼저 학습한 뒤 상위 의사결정으로 확장 | Stage 1 범위와 이후 확장 경계로 유지 `[Fixed]` |
| 2026-09-14–15 | GD2P, TaskDexGrasp, contact selection과 downstream state quality 검토 | H2와 C2의 문헌 출발점으로 유지; novelty는 미확정 |
| 2026-09-15 | Actor/privileged information 분리, end-effector delta pose+hand action, binary tactile와 F/T 검토 | Actor observation과 simulator-only 정보의 분리 원칙 및 observation encoding은 유지. Action과 privileged 정보의 구체 용도는 `[Open]` |
| 2026-09-15 | Rotation representation 5D/6D/quaternion, history와 action memory 논의 | Quaternion/current-only/one-step은 현재 baseline; representation 우위는 미확정 |
| 2026-09-16 | Motivation, paper index, topic groups와 reward references를 별도 문서로 분리 | 문헌과 세부 근거의 참고 자료로 유지; 현재 분류 권한은 이 문서 |
| 2026-09-16 | 최신 IL/VLA도 tactile/force와 fast feedback을 다룬다는 반례 확인 | 기존 방법을 과도하게 단순화하지 않도록 주장 범위에 반영 |
| 2026-09-17 | 64D goal-quaternion 안을 66D selected-face/direction 안으로 대체 | 64D는 `[Superseded]`; 66D actor observation은 현재 합의된 `[Baseline]` |
| 2026-09-17 | Single-threshold 17D tactile, current wrench와 one-step previous action | Single-threshold·current-only·one-step 구성은 observation `[Baseline]`; threshold 수치, sensor mapping·전처리와 향후 history 추가 여부는 `[Open]` |
| 2026-09-17 | Rotation 중 translation, recoverable alignment/contact loss와 contact migration 허용 | 과거 제안으로 보존하며 action·contact·safety 규칙은 현재 `[Open]` |

### 14.3 과거 구현안

| 과거 안 | 내용 | 상태 |
| --- | --- | --- |
| Early observation drafts | Point cloud/mesh, phase ID, 다수 kinematic feature와 modality별 history를 넓게 포함 | `[Superseded]` |
| Observation v0.1 | 6D rotation과 variable tactile/F/T/action history | `[Superseded]` |
| Observation v0.2 | Target/current quaternion을 포함한 64D input | `[Superseded]` |
| Observation v0.3 | Selected-face/direction 기반 66D input | `[Baseline]` |
| Reward v0.1–v0.2 | Target yaw/quaternion 중심의 phase reward | `[Superseded]` |
| Reward v0.3 | Face-direction alignment, translation progress, aggregate-contact loss와 safety 분리 | 과거 논의안; 현재 `[Open]` |

이후 변경은 기존 정의를 삭제하는 대신 Section 14.1 표에 `Previous Definition → Updated Definition → Reason`을 추가한다.
