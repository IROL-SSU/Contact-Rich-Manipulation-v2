# Research Trend — Supervision Burden and Conditional RL Selection

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Previous Works](./previous_works.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** Nonprehensile manipulation의 구조적 난제에 대해 Conventional methods, IL/VLA와 RL이 task-level decision knowledge를 어디에서 얻고 어떤 model·data·interaction 부담을 지는지 비교한다. 그 근거를 현재 연구 조건에 대입해 RL을 primary policy-learning method로 선택하는 이유와 기각 조건을 정리한다. 비교 대상의 연도별 timeline과 Ours에 대한 개별 비교는 [Previous Works](./previous_works.md)에서 다룬다.

---

## 1. Method-selection question과 그룹화 원칙

[Research Motivation](./research_motivation.md)은 연구 대상을 `approximate geometry 아래의 contact-feedback nonprehensile manipulation`으로 좁혔다. 그러나 contact uncertainty나 tactile/F/T의 존재만으로 RL이 선택되지는 않는다. Planning과 control은 어느 family에도 필요한 기능이므로 RL·IL·VLA와 병렬인 방법론 이름으로 사용하지 않는다. 여기서는 **task-level decision rule을 무엇으로 지정·학습하는가**를 기준으로 그룹을 나눈다.

> **Nonprehensile manipulation의 contact planning·execution·recovery에 필요한 decision knowledge를 model·규칙, demonstration·pretraining 또는 interaction return 중 어디에서 얻는 것이 합리적인가?**

세부 architecture 이름을 나열하기보다 task-level decision rule의 주된 source를 세 그룹으로 묶는다.

| Method group | Task-level decision knowledge의 source | 주로 요구하는 자원 |
| --- | --- | --- |
| **Conventional methods** | 사람이 설계한 heuristic, contact model·mode, constraint·cost, planner·optimizer와 feedback law | Model·mode coverage, state estimate, exception handling과 online computation |
| **IL / VLA** | Demonstration의 state–action relation 또는 pretrained vision–language–action prior | Nominal뿐 아니라 off-nominal·recovery demonstration, sensor synchronization, embodiment adaptation과 local controller |
| **RL** | Environment interaction에서 얻은 task return·constraint로 policy·value 최적화 | Exploration 가능한 interaction, reward·credit assignment, training distribution, simulator fidelity와 offline computation |

`Conventional`은 시스템에 learning module이 전혀 없다는 뜻이 아니라, 배포 시 task-level decision structure의 중심이 설계된 planner·optimizer·controller라는 뜻이다. GD2P의 generative proposal+physics/planning, optimization demonstration+RL, VLA+force controller처럼 여러 source가 핵심 역할을 나누면 **composition으로 표시해 어느 부담을 어느 module이 담당하는지** 기록한다. 범주는 상호 배타적인 architecture 이름이 아니라 dominant adaptation signal이며, 한 논문의 제한된 task scope를 해당 family 전체의 불가능성으로 일반화하지 않는다.

---

## 2. Nonprehensile manipulation이 만드는 공통 decision burden

Nonprehensile manipulation에서는 물체가 안정적인 grasp로 robot에 구속되지 않는다. Robot action은 robot–object–environment contact를 거쳐 물체 운동으로 전달되므로, 다음 부담이 어느 방법에서든 발생한다.

1. **간접적이고 비유일한 object control.** 같은 object goal에도 여러 contact location과 motion이 가능하며, 같은 robot motion도 접촉 위치·마찰·support reaction에 따라 다른 결과를 만든다. [HACMan](https://doi.org/10.48550/arXiv.2305.03942)은 이를 `where to contact`와 `how to move`의 공동 결정으로 구성한다.
2. **Hybrid하고 non-smooth한 dynamics.** Sticking, sliding, pivoting, separation과 re-contact는 continuous motion과 discrete contact mode를 함께 만든다. [Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135)은 tactile로 grasped-object pose와 extrinsic contact location을 추정하고, 주어진 contact mode 안에서 optimization과 feedback control을 수행한다.
3. **Approximate geometry와 partial observation.** 작은 pose·shape·friction 오차도 slip, contact loss와 잘못된 object motion으로 누적된다. [Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157)은 estimator uncertainty를 RL control에 전달해 이 문제를 다룬다.
4. **Local contact quality와 final task outcome의 차이.** 당장의 안정적인 접촉이나 reorientation 성공이 후속 translation의 feasibility를 보장하지 않는다. Contact formation–reorientation–translation을 독립적으로 최적화하면 앞 단계의 종료 상태가 다음 단계에 부적합할 수 있다.
5. **Environmental contact의 양면성.** Wall·table·shelf contact는 feasible motion을 늘리는 manipulation resource가 될 수 있지만 excessive force, jamming과 collision도 만든다. [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271)은 wall contact를 pivoting에 활용하고, [HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154)은 object와 environment geometry를 함께 표현한다.

따라서 핵심은 `contact를 쓰는가`가 아니라 **어떤 contact를 언제 형성·유지·전환하고, 그 결과가 final object goal에 도움이 되는지를 어떻게 결정하는가**이다.

---

## 3. 기존 연구가 보여준 가능성과 남긴 부담

아래 비교는 다른 방법이 nonprehensile manipulation을 해결할 수 없다는 주장이 아니다. 기존 연구가 실제로 해결한 범위와, 그 해결을 위해 어디에 설계·data·computation 부담을 두었는지를 함께 읽는다.

| Method group or composition | 기존 연구가 보여준 가능성 | 남은 부담과 적용 경계 | RL 선택에 주는 의미 |
| --- | --- | --- | --- |
| **Conventional methods** | [Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471)은 analytical push-interaction model 없이 설계된 tactile feedback law로 translation을 보정했다. [Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135)은 tactile state estimation과 mode-constrained optimization으로 planar sliding·pivoting을 제어했다. | 전자는 제한된 translation setting, 후자는 known polygon, planar quasi-static contact와 prescribed mode trajectory 아래에서 강점을 보인다. Object·environment geometry, contact mode와 wrist–finger action의 조합이 늘수록 model·constraint·switching rule의 coverage 부담이 커지고, optimization을 사용하는 경우 online solve 부담도 증가한다. | 정확한 가정과 작은 mode 집합에서는 data-efficient하고 해석 가능한 우선 선택이다. RL의 상대적 이유는 conventional method가 contact를 다룰 수 없어서가 아니라, 필요한 specification coverage를 신뢰성 있게 작성하기 어려워질 때 생긴다. |
| **IL / VLA** | [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052)는 slow action chunk와 high-frequency tactile loop를 결합한 reactive IL을, [DexMove](https://openreview.net/forum?id=dT3ZciXvNX)는 tactile-guided wrist–finger policy를 보여준다. [ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169)와 [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160)는 force/tactile input을 VLA action과 force control에 연결한다. | 문제는 contact feedback의 가능 여부가 아니라 **coverage의 출처**다. Offline behavior cloning에서는 rollout error가 expert distribution 밖의 state를 만들 수 있다. Off-nominal·recovery state를 안정적으로 다루려면 이를 포괄하는 data·teacher 또는 interactive aggregation이 필요하며, [DAgger](https://proceedings.mlr.press/v15/ross11a.html)는 learner가 유도한 state에 expert label을 추가한다. DexMove는 simulation trajectory와 wearable tactile demonstration을, ForceVLA2는 force signal을 포함한 task dataset과 controller를 구축했다. | 충분한 recovery demonstration과 sensor·embodiment-aligned data가 있으면 IL/VLA가 더 직접적일 수 있다. 이것이 부족하더라도 RL이 자동으로 우월한 것은 아니며, measurable outcome과 affordable simulation/reset으로 policy-induced failure state를 더 싸게 생성·평가할 수 있을 때만 RL로의 전환이 정당화된다. |
| **RL** | [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873)은 goal progress·contact 유지·collision을, [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271)은 wall-assisted pivoting을, [Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157)은 state uncertainty를, [HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154)은 object·environment geometry를 interaction outcome과 연결했다. | RL도 문제 구조를 자동으로 발견하지 않는다. 각 연구는 structured action/representation, privileged simulation data, procedural diversity나 state/action projection을 추가했다. Sample complexity, reward·credit specification, unsafe exploration, simulator contact fidelity와 Sim-to-Real이 남는다. | 핵심은 simulation 자체가 아니라 **interaction에서 수집된 state–action–outcome이 policy update에 반영되는 것**이다. Task return으로 여러 유효 contact strategy와 failure/recovery를 비교하고, 반복 실행할 closed-loop policy로 amortize할 수 있다. |
| **Composition / structured learning** | [GD2P](https://doi.org/10.48550/arXiv.2509.18455)는 generative pre-contact proposal을 physics simulation과 planning으로 검사한다. [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)은 CITO의 kinematics·dynamics-aware demonstration과 contact-force trajectory로 exploration을 유도하고 RL policy를 학습한다. | Module 간 state·objective mismatch와 planner·optimizer 의존성이 추가되지만, 좁은 feasible region에서 vanilla RL이 겪는 exploration 부담을 줄일 수 있다. 후자의 검증 범위는 rigid-object SE(2) task이며 estimator error accumulation도 남는다. | 결론은 pure model-free RL의 보편적 우위가 아니다. Feasible contact 발견은 planning·optimization·demonstration으로 구조화하고, 불확실성 아래 반복되는 correction과 downstream outcome optimization은 RL에 맡기는 구성이 더 합리적일 수 있다. |

Pure interaction-return RL을 선택할 때에는 부담이 제거되는 것이 아니라 다음과 같이 이동한다. Structured RL은 양쪽 부담을 일부 함께 유지한다.

> `model·mode specification·recovery expert label·online search`의 부담
> → `interaction volume·reward/credit·exploration·simulator coverage·transfer`의 부담

[Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236)의 실험 설정에서 learned-model planning은 sample-efficient했고, 충분히 학습한 model-free policy는 큰 disturbance에서 더 강건했다. 따라서 선택 기준은 추상적인 우열이 아니라 **어떤 부담을 현재 연구에서 더 신뢰성 있게 감당할 수 있는가**이다.

---

## 4. Conventional·IL/VLA에서 RL로 이어지는 조건부 논리

방법론 선택은 `Conventional → IL/VLA → RL`이라는 성능의 서열이나 역사적 대체 관계가 아니다. 세 그룹은 같은 contact decision burden을 서로 다른 supervision과 resource로 해결한다. 따라서 conventional method의 specification 부담이나 IL/VLA의 data 부담만으로 RL이 결론이 되지는 않는다. **현재 연구가 그 부담을 interaction-return 학습의 부담보다 감당하기 어려운지**를 함께 확인해야 한다.

| 문제 조건 | Conventional methods에서 필요한 것 | IL/VLA에서 필요한 것 | RL 전환이 성립하는 추가 전제 |
| --- | --- | --- | --- |
| Geometry·friction·contact mode의 불확실성이 큼 | Model·constraint·mode와 switching·exception rule의 coverage | 관련 variation과 correction을 포괄하는 demonstration과 sensor alignment | 관련 변화를 simulation distribution에 포함하고 outcome으로 전략을 비교할 수 있음 |
| Policy action이 slip·contact loss·failed approach를 유발함 | Recovery rule, re-planning 또는 robust feedback 설계 | Policy-induced off-nominal state를 포괄하는 corrective data 또는 teacher | 실패를 안전하게 생성·reset하고 reward·constraint로 recovery를 평가할 수 있음 |
| 초기 contact가 reorientation과 후속 translation을 함께 좌우함 | 단계 간 cost·terminal constraint와 mode schedule 설계 | Downstream trade-off가 포함된 multi-stage demonstration | Episode return이 final outcome을 반영하고 credit assignment가 가능함 |
| 같은 task family를 반복 수행함 | 반복되는 online search·optimization 또는 충분히 포괄적인 controller | 충분한 demonstration 수집 후 policy inference | 많은 offline interaction 비용을 반복 실행에서 회수할 수 있음 |

> `Specification coverage 부담 ↑` + `policy-induced recovery label 부담 ↑`만으로는 RL이 결론이 되지 않는다.
>
> 여기에 `measurable outcome` + `safe·affordable interaction/reset` + `representative simulation`이 있을 때, interaction-return RL이 **조건부로 합리적**이 된다.

### 4.1 Outcome supervision을 더 쉽게 확보할 수 있는 경우

같은 object goal에도 여러 contact location·hand configuration·motion sequence가 가능하므로, 모든 유효 state에서 하나의 expert action을 정답으로 정하기는 어렵다. 그러나 action의 비유일성만으로 RL이 필요한 것은 아니다. [Diffusion Policy](https://doi.org/10.15607/RSS.2023.XIX.026)처럼 생성형 IL은 multimodal action distribution을 표현할 수 있고, [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052)는 action chunk 실행 중 high-frequency tactile feedback에도 반응한다. RL의 구별점은 **policy rollout에서 방문한 각 state에 expert action label을 요구하는 대신**, measurable task outcome으로 여러 전략과 recovery 결과를 비교할 수 있다는 데 있다. 이 장점은 outcome이 충분한 학습 신호를 제공하고 exploration이 feasible contact를 실제로 찾을 때에만 성립한다. [HACMan](https://doi.org/10.48550/arXiv.2305.03942)과 [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271)은 각각 contact location–motion과 environment-assisted pivoting을 interaction outcome으로 학습한 사례다.

### 4.2 실패·복구 interaction을 안전하게 생성할 수 있는 경우

실물에서는 slip, contact loss, excessive force와 failed approach를 의도적으로 반복하기 어렵다. Simulation과 automatic reset이 가능하면 geometry·friction·sensor error와 disturbance를 반복 생성할 수 있다. Simulation 자체는 RL만의 자원이 아니며 demonstration 생성에도 사용할 수 있다. 차이는 teacher가 충분한 corrective action을 제공할 수 있는지다. [DAgger](https://proceedings.mlr.press/v15/ross11a.html)처럼 IL은 learner가 유도한 state distribution에 expert label을 추가하는 방식으로 이 문제를 줄일 수 있다. 반면 interaction-based RL은 현재 policy가 만든 실패·복구 rollout을 task return과 constraint로 직접 비교할 수 있으므로, reliable teacher보다 outcome evaluator를 만들기 쉬운 경우에 유리하다.

[Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157)은 한 privileged policy의 여러 training checkpoint를 rollout해 무작위·하위 최적·최적 interaction을 포함한 estimator data를 만들고, 그 estimator를 RL loop에 넣어 uncertainty-aware policy를 학습했다. 이는 diverse policy-induced state 생성의 근거이지 recovery 우월성의 직접 비교는 아니다. [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271)은 wall position·F/T noise·initial pose를 randomize해 학습했고, hardware에서 failure 후의 정성적 recovery behavior를 제시했다. 다만 failure-state coverage, safety constraint와 representative randomization이 없다면 RL도 nominal behavior나 simulator exploit을 학습할 수 있으므로 robustness는 자동 결과가 아니다. Simulation teacher가 정확한 recovery demonstration을 저렴하게 생성할 수 있다면 IL이 더 단순한 선택일 수도 있다.

### 4.3 초기 contact와 downstream outcome을 함께 평가해야 하는 경우

Multi-stage manipulation에서는 각 primitive의 local success가 final object goal을 보장하지 않는다. Conventional method는 단계 간 cost·terminal constraint를, IL/VLA는 downstream trade-off가 포함된 demonstration을 설계해야 한다. RL은 episode-level return으로 초기 contact와 environment use가 downstream success에 미친 영향을 함께 평가할 수 있지만, 긴 horizon의 credit assignment가 새로운 부담이 된다. [HACMan++](https://doi.org/10.15607/RSS.2024.XX.129)은 primitive type·contact location·motion parameter를 순차적으로 선택해 multi-step manipulation을 구성한 사례다. 본 연구의 `Rotation 종료 상태가 후속 Push에 유리한가`는 이 일반 논리의 전제가 아니라 Section 5에서 검증할 project-level hypothesis다.

### 4.4 반복 task에서 policy 학습 비용을 회수할 수 있는 경우

모든 planner나 controller가 느린 것은 아니지만, contact mode와 trajectory를 매 state에서 반복 탐색·최적화해야 하는 구성에서는 online computation이 커질 수 있다. Learned policy는 큰 offline 비용을 먼저 지불하고 반복되는 task family에서 결정을 inference로 실행한다. 이 amortization은 IL/VLA에도 해당하므로 그 자체가 RL만의 근거는 아니다. **빠른 policy 실행이 필요하면서 recovery demonstration은 부족하고 simulation interaction은 충분한 경우**에 비로소 RL 선택 근거가 된다. [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)은 CITO-generated demonstrations로 feasible exploration을 유도하고, temporal vision·force·proprioception을 쓰는 estimator와 closed-loop RL policy stack을 학습하는 composition 사례다. One-off task나 training distribution 밖의 목표에는 online planning이 더 적합할 수 있다.

이 논리는 다음 조건에서 강해진다.

| RL의 상대적 이점이 커지는 조건 | 다른 방법의 이점이 커지는 조건 |
| --- | --- |
| 정확한 contact model과 mode sequence는 부족하지만 randomized simulation interaction은 충분함 | 신뢰할 수 있는 dynamics·state estimate·constraint와 작은 contact-mode 집합이 있음 |
| Outcome·safety constraint는 측정 가능하지만 policy-induced state의 expert action은 부족함 | 충분히 다양한 expert·recovery demonstration이나 reliable simulation teacher가 있음 |
| Contact choice, environmental contact와 continuous motion을 함께 결정해야 함 | 안정적인 feedback law나 planner가 task 전체를 직접 다룰 수 있음 |
| 실패·disturbance를 안전하게 생성하고 자동 reset할 수 있음 | 실제 interaction만 가능하고 failure·reset 비용이 큼 |
| 유사한 task를 반복해 offline training cost를 회수할 수 있음 | 소수의 one-off task나 distribution 밖의 목표를 즉시 해결해야 함 |

### 4.5 발표 슬라이드용 핵심 구조

슬라이드 제목은 **`Why RL for Nonprehensile Manipulation? — A Supervision and Resource Trade-off`**처럼 잡는 편이 적합하다. 세 column은 다음 정도로 압축할 수 있다.

| Conventional methods | IL / VLA | Reinforcement learning |
| --- | --- | --- |
| **Decision source:** model·mode·constraint·designed rule | **Decision source:** demonstration·pretrained action prior | **Decision source:** interaction return·constraint |
| **Strength:** 정확한 가정과 작은 mode 집합에서 data-efficient하고 해석 가능 | **Strength:** 고차원 sensor–action relation과 reactive behavior를 직접 학습 | **Strength:** policy가 만든 성공·실패를 outcome으로 비교하며 closed-loop strategy를 개선 |
| **Main burden:** contact 변화에 따른 model·mode·rule 설계 | **Main burden:** failure·recovery coverage와 sensor·embodiment 의존성 | **Main burden:** exploration·reward·interaction과 Sim-to-Real |

> **Transition:** 정확한 model·mode와 recovery label은 부족하지만, outcome·simulation·reset은 확보할 수 있다. 따라서 task-return RL이 조건부로 합리적이다. Exploration이 병목이면 planning·demonstration prior를 결합한다.

---

## 5. 현재 연구에서 RL을 primary method로 선택하는 이유

현재 연구의 자원과 문제 조건을 앞의 판단 기준에 대입하면 다음과 같다.

| 현재 조건 | RL이 더 합리적인 이유 | 반드시 확인할 경계 |
| --- | --- | --- |
| Estimated OBB와 실제 contact surface·dynamics 사이에 오차가 있음 | Geometry·friction·sensor perturbation 아래의 실패와 recovery를 simulation interaction으로 반복 생성할 수 있음 | 실제 error를 대표하는 perturbation 정의와 matched model/control baseline |
| Contact formation과 reorientation이 후속 Push feasibility를 결정함 | Episode return으로 initial contact와 final Rotation-to-Push outcome을 연결할 수 있음 | Rotation-only success와 saved-state Push success를 분리해 credit 전달 확인 |
| Shelf contact가 constraint이면서 manipulation resource일 수 있음 | 정확한 shelf-contact model과 mode schedule은 없지만 final Rotation-to-Push outcome은 측정 가능하므로, return에서 도움이 되는 접촉과 해로운 접촉의 조건을 학습할 수 있음 | `contact allowed / penalized / unavailable` 비교와 force·collision violation 측정. 신뢰할 수 있는 model·mode scheduler가 확보되면 MPC/control과 직접 비교 |
| Tactile·wrist F/T에 따라 wrist와 finger를 함께 조정할 가능성이 있음 | 포괄적인 wrist–finger recovery demonstration과 결합 dynamics model은 없지만 interaction outcome은 얻을 수 있으므로, multimodal feedback에 따른 coupled correction을 학습할 수 있음 | Fixed-hand, wrist-only, wrist+finger 및 sensor factorial ablation. 충분한 model이나 recovery demonstration이 확보되면 MPC·IL과 직접 비교 |
| Real recovery demonstration은 제한적이지만 simulation interaction은 많이 확보할 수 있음 | Privileged simulator information은 reward·constraint에만 사용하고 actor는 deployable observation으로 학습할 수 있음 | Sensor corruption·dynamics randomization과 real transfer 평가 |

> **이 연구에서 RL을 선택하는 이유는 contact feedback이나 recovery가 RL만의 기능이기 때문이 아니다. 정확한 contact model·mode schedule과 다양한 recovery demonstration은 확보하기 어렵지만, task outcome과 deployable sensor feedback을 정의하고 randomized simulation interaction을 반복 생성할 수 있다. 따라서 contact formation, environmental-contact exploitation, reorientation과 후속 pushing의 영향을 reusable closed-loop policy로 학습하는 RL이 현재 자원 구조에 더 합리적인 primary task-level policy-learning method다.**

여기서 RL은 low-level controller, safety supervisor와 geometry representation을 대체하지 않는다. Feasible contact의 발견이 병목이면 GD2P나 Optimization-Guided RL처럼 planning·optimization prior를 결합한 structured RL로 확장한다.

---

## 6. 선택의 경계와 다음 문서

RL 선택은 `[Baseline]` 방법 판단이며 research gap이나 contribution이 아니다. 다음 결과가 나오면 RL을 primary method로 선택한 근거는 약해지거나 수정되어야 한다.

- 동일한 sensing·action budget의 model/control baseline이 estimated geometry에서도 더 적은 data와 computation으로 같은 robustness를 냄
- Pose generation과 단순 feedback만으로 perturbation 아래 Rotation-to-Push를 안정적으로 해결함
- Reactive IL이 더 적은 data와 safety violation으로 같은 recovery 및 final-task 성능을 냄
- Environmental contact 허용이 final-task success를 높이지 않고 force·collision risk만 증가시킴
- Simulation에서 얻은 이점이 실제 tactile/F/T 조건으로 전이되지 않음
- Vanilla RL이 좁은 feasible contact region을 반복적으로 찾지 못함. 이 경우 pure RL을 유지하지 않고 planning·demonstration-guided RL과 비교함

다음 문서인 [Previous Works](./previous_works.md)는 B85를 포함한 비교 대상 11편만으로 구성한 timeline과 여덟 개의 공통 column을 사용해, Ours와 가장 가까운 조건 및 아직 검증되지 않은 C1·C2를 정리한다.
