# Research Trend — Method Limits and Conditional RL Selection

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Previous Works](./previous_works.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** Nonprehensile manipulation의 구조적 난제에 대해 기존 방법이 해결한 범위와 남긴 model·data·interaction·computation 부담을 비교하고, 그 근거를 현재 연구 조건에 대입해 RL을 primary action-generation mechanism으로 선택하는 이유와 기각 조건을 정리한다. 비교 대상의 연도별 timeline과 Ours에 대한 개별 비교는 [Previous Works](./previous_works.md)에서 다룬다.

---

## 1. Method-selection question과 그룹화 원칙

[Research Motivation](./research_motivation.md)은 연구 대상을 `approximate geometry 아래의 contact-feedback nonprehensile manipulation`으로 좁혔다. 그러나 contact uncertainty나 tactile/F/T의 존재만으로 RL이 선택되지는 않는다. 이 문서의 질문은 다음과 같다.

> **Nonprehensile manipulation의 contact planning·execution·recovery를 해결해 온 방법은 각각 어떤 자원을 요구하며, 정확한 model이나 충분한 demonstration보다 simulation interaction과 task outcome을 더 쉽게 확보할 수 있는 조건에서 왜 RL이 더 합리적인가?**

세부 architecture 이름을 나열하기보다 deployment action을 만드는 주된 mechanism을 세 그룹으로 묶는다.

| Base mechanism | Action을 만드는 주된 근거 | 주로 요구하는 자원 |
| --- | --- | --- |
| **Explicit model / feedback control** | Dynamics·contact mode·constraint·feedback law와 online optimization | 적절한 model·state estimate·mode assumption과 online computation |
| **Demonstration / pretrained or generative prior** | Expert action, pretrained representation 또는 생성된 candidate | Demonstration·pretraining data, candidate 검증과 local controller |
| **Interaction-return RL** | Environment interaction에서 얻은 task return으로 policy·value 최적화 | Exploration 가능한 interaction, reward·training distribution과 offline computation |

Planning-generated demonstration과 RL, VLA와 force controller처럼 여러 mechanism이 핵심 역할을 나누면 별도의 네 번째 base family로 만들지 않고 **composition으로 표시해 어느 부담을 어느 module이 담당하는지** 기록한다. 또한 한 논문의 제한된 task scope를 해당 method family 전체의 불가능성으로 일반화하지 않는다.

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

| Base mechanism or composition | 기존 연구가 보여준 가능성 | 남은 부담과 적용 경계 | RL 선택에 주는 의미 |
| --- | --- | --- | --- |
| **Explicit model / feedback control** | [Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471)은 explicit push-interaction model 없이 tactile feedback law로 translation을 보정했다. [Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135)은 tactile state estimation과 mode-constrained optimization으로 planar sliding·pivoting을 제어했다. | 전자는 uncluttered/open/translation 범위이고, 후자는 known polygon과 planar quasi-static contact, prescribed mode trajectory 등 명시적 가정 아래 작동한다. Geometry·contact mode·wrist–finger motion의 조합이 늘면 model과 switching/optimization 설계 부담도 증가한다. | Contact feedback이나 recovery 자체는 RL만의 장점이 아니다. 다만 approximate geometry에서 많은 contact transition을 사전에 모델링하기 어려울수록 interaction으로 decision rule을 학습할 상대적 이유가 커진다. |
| **Demonstration / pretrained or generative prior** | [GD2P](https://doi.org/10.48550/arXiv.2509.18455)는 geometry-conditioned pre-contact hand pose를 생성해 simulation과 planning으로 검사한다. [DexMove](https://openreview.net/forum?id=dT3ZciXvNX)는 tactile wrist–finger IL을 보여준다. [ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169)는 [ForceVLA](https://doi.org/10.52202/085713-3124)의 force-conditioned VLA를 force target·control-mode output과 hybrid force–position control로 확장했고, [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160)는 tactile-conditioned action과 position–force control을 결합한다. | 생성된 initial pose가 post-contact feasibility와 recovery를 보장하지는 않는다. IL/VLA는 synthetic·human demonstration, synchronized force/tactile data와 embodiment-specific controller의 coverage에 의존한다. ForceVLA2와 Tactile-VLA는 direct nonprehensile보다 인접 contact-rich 근거다. | 충분한 recovery demonstration이나 검증된 planner가 있으면 이 접근이 더 효율적일 수 있다. 반대로 정답 action은 비유일하고 실패 상태를 포함한 expert coverage가 부족하면 task outcome을 supervision으로 쓰는 RL이 더 자연스럽다. |
| **Interaction-return RL** | [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873)은 goal progress·contact 유지·collision을, [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271)은 wall-assisted pivoting을, [Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157)은 state uncertainty를, [HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154)은 object·environment geometry를 interaction outcome과 연결했다. | RL도 문제 구조를 자동으로 발견하지 않는다. 각 연구는 depth-derived feature와 state/action projection, privileged simulation data, modular representation 또는 procedurally diverse training을 추가했다. Sample complexity, reward specification, unsafe exploration과 Sim-to-Real이 남는다. | RL의 장점은 무구조성이 아니라, task outcome으로 contact decision을 평가하고 반복 실행 가능한 policy로 압축할 수 있다는 점이다. Representation·action structure와 training distribution을 의도적으로 설계해야 한다. |
| **Composition: role-sharing / structured RL** | [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)은 optimization-generated demonstrations로 feasible prior를 제공하고 RL로 closed-loop policy를 학습한다. | Module 간 state·objective mismatch와 optimizer 의존성이 추가된다. 좁은 feasible region에서는 vanilla RL이 충분히 탐색하지 못할 수 있다. | 결론은 pure model-free RL의 보편적 우위가 아니다. Contact feasibility를 찾기 어려운 구간은 planning·optimization·demonstration으로 구조화하고, 불확실성 아래 반복되는 correction은 RL에 맡기는 구성이 더 합리적일 수 있다. |

Pure interaction-return RL을 선택할 때에는 부담이 다음과 같이 이동한다. Structured RL은 양쪽 부담을 일부 함께 유지한다.

> `model·mode schedule·expert action·online search`의 부담
> → `interaction data·reward/exploration·offline training`의 부담

[Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236)이 보여주듯 learned-model planning은 sample-efficient할 수 있고, 충분히 학습한 model-free policy는 큰 disturbance에서 더 강건할 수 있다. 따라서 선택 기준은 추상적인 우열이 아니라 **어떤 부담을 현재 연구에서 더 신뢰성 있게 감당할 수 있는가**이다.

---

## 4. Nonprehensile manipulation에서 RL이 더 합리적인 조건

### 4.1 정답 trajectory보다 task outcome을 정의하기 쉽다

Object goal과 success condition은 정의할 수 있어도, 어느 surface를 어떤 hand configuration으로 접촉하고 언제 slide·pivot·separate·re-contact해야 하는지에 대한 하나의 정답 trajectory는 존재하지 않는다. RL은 expert action을 그대로 모방하는 대신 interaction return으로 여러 contact decision을 비교할 수 있다. 이는 contact location과 motion parameter를 함께 학습한 [HACMan](https://doi.org/10.48550/arXiv.2305.03942), environment contact를 포함한 pivoting을 학습한 [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271)과 같은 문제에 잘 맞는다.

### 4.2 실패와 recovery를 training distribution으로 만들 수 있다

실물에서는 slip, contact loss, excessive force와 failed approach를 의도적으로 반복하기 어렵다. Simulation과 automatic reset이 가능하면 geometry·friction·sensor error와 disturbance를 반복 생성하고, recovery가 실제 return을 높이는지 학습할 수 있다. [Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157)은 privileged policies가 만든 diverse simulation interaction으로 estimator를 학습한 뒤, 그 estimator를 RL loop에 넣어 uncertainty-aware policy를 학습했다. [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271)은 wall position·F/T noise·initial pose를 randomize해 학습했고, 실물에서 첫 pivoting 실패 뒤 재접근해 성공한 정성적 recovery 사례를 보였다. 다만 randomization과 failure-state coverage가 없다면 RL도 nominal behavior만 학습하므로 robustness는 자동 결과가 아니다.

### 4.3 Contact formation과 후속 outcome을 하나의 objective로 연결할 수 있다

Pre-contact pose, pivoting과 pushing을 각각 성공시키는 것만으로는 충분하지 않다. Episode-level return을 사용하면 초기 contact formation과 environmental-contact exploitation이 최종 Rotation-to-Push 성공에 미친 영향을 함께 평가할 수 있다. [HACMan++](https://doi.org/10.15607/RSS.2024.XX.129)은 primitive type·contact location·motion parameter를 순차적으로 선택해 multi-step manipulation을 구성하지만, Rotation 종료 상태를 후속 Push 성공으로 직접 평가하는 것은 별개의 문제다. 따라서 이 coupling은 분야 수준의 일반적 사실이 아니라 본 연구가 검증할 project-level hypothesis다. 긴 horizon의 credit assignment는 어려워지지만, 단계별 controller가 제공하지 않는 downstream criterion을 명시적으로 넣을 수 있다는 장점이 있다.

### 4.4 반복되는 online decision을 policy에 amortize할 수 있다

Planning은 새로운 state마다 contact sequence와 trajectory를 다시 계산한다. RL은 많은 offline interaction 비용을 먼저 지불하고, 반복되는 task family에서 그 결정을 빠른 policy inference로 실행한다. [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)은 CITO-generated demonstrations로 RL 학습을 유도하고, 배포 시 sensor-driven closed-loop policy를 실행하는 직접 사례다. One-off task나 training distribution 밖의 목표에는 online planning이 더 적합하지만, 유사한 constrained manipulation을 반복하는 배치에서는 학습 비용을 회수할 수 있다.

이 논리는 다음 조건에서 강해진다.

| RL의 상대적 이점이 커지는 조건 | 다른 방법의 이점이 커지는 조건 |
| --- | --- |
| 정확한 contact model과 mode sequence는 부족하지만 randomized simulation interaction은 충분함 | 신뢰할 수 있는 dynamics·state estimate·constraint와 작은 contact-mode 집합이 있음 |
| 성공 metric은 명확하지만 가능한 action trajectory가 비유일함 | 충분히 다양한 expert·recovery demonstration이 이미 존재함 |
| Contact choice, environmental contact와 continuous motion을 함께 결정해야 함 | 안정적인 feedback law나 planner가 task 전체를 직접 다룰 수 있음 |
| 실패·disturbance를 안전하게 생성하고 자동 reset할 수 있음 | 실제 interaction만 가능하고 failure·reset 비용이 큼 |
| 유사한 task를 반복해 offline training cost를 회수할 수 있음 | 소수의 one-off task나 distribution 밖의 목표를 즉시 해결해야 함 |

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

> **이 연구에서 RL을 선택하는 이유는 contact feedback이나 recovery가 RL만의 기능이기 때문이 아니다. 정확한 contact model·mode schedule과 다양한 recovery demonstration은 확보하기 어렵지만, task outcome과 deployable sensor feedback을 정의하고 randomized simulation interaction을 반복 생성할 수 있다. 따라서 contact formation, environmental-contact exploitation, reorientation과 후속 pushing의 영향을 reusable closed-loop policy로 학습하는 RL이 현재 자원 구조에 더 합리적인 primary action-generation mechanism이다.**

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

다음 문서인 [Previous Works](./previous_works.md)는 B85를 포함한 비교 대상 11편만으로 구성한 timeline과 일곱 개의 공통 column을 사용해, Ours와 가장 가까운 조건 및 아직 검증되지 않은 C1·C2를 정리한다.
