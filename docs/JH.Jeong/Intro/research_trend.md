# Research Trend — From Method Evolution to Conditional RL Selection

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Previous Works](./previous_works.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 2021년 이후 nonprehensile 및 contact-feedback manipulation의 방법론적 변화를 보여주고, 각 방법이 nonprehensile manipulation을 해결할 때 갖는 강점과 구조적 한계를 구분한 뒤 현재 연구에서 RL을 우선 구현하는 이유를 도출한다. 개별 논문의 세부 비교와 연구 gap 판정은 [Previous Works](./previous_works.md)에서 수행한다.

---

## 1. 비교 관점과 분류 원칙

[Research Motivation](./research_motivation.md)은 연구 대상을 `vision-derived approximate geometry와 contact feedback을 사용하는 nonprehensile manipulation`으로 좁혔다. 이 문서는 각 방법이 접촉 불확실성을 어떻게 다루며 어떤 한계를 갖는지 살핀 뒤, 그 차이를 현재의 model·data·interaction 조건에 대입한다.

> **Nonprehensile manipulation의 contact uncertainty를 다뤄 온 방법론은 어떻게 변해 왔으며, 현재 조건에서는 어떤 방법을 우선 구현해야 하는가?**

방법은 sensor 종류나 network architecture가 아니라 **실행할 action을 생성하는 주된 방식**으로 구분한다. 사람이 정의한 rule이나 feedback law는 heuristic/control, 명시적 model·constraint·search는 optimization/planning, 생성 model이 contact pose나 trajectory 후보를 제안하고 feasibility test 또는 planner가 선택·실행하면 generative/planning이다. 상호작용에서 얻은 task return으로 policy를 최적화하면 RL, demonstration action이 주 supervision이면 IL, vision–language representation과 action generation을 generalist policy로 결합하면 VLA로 본다. 둘 이상의 방식이 최종 action 생성에 필수적으로 관여할 때만 hybrid로 분류한다.

따라서 `Diffusion`이라는 architecture만으로 IL과 generative/planning을 판정하지 않는다. Demonstration action을 모방하면 IL이고, 생성된 후보를 별도의 feasibility test나 planner가 선택하면 generative/planning이다. 논문별 분류는 [Previous Works의 비교표](./previous_works.md#2-비교표)에 기록한다.

---

## 2. 2021–2026: 네 가지 방법론적 변화

아래 표는 모든 논문을 연도순으로 나열하지 않고, 방법의 역할 또는 연구 범위를 바꾼 대표 논문을 네 흐름으로 묶은 것이다. 논문별 검토 상태와 우리 task에 대한 직접성은 [Previous Works](./previous_works.md)와 [Paper Index](../papers/README.md)에서 별도로 관리한다.

| 변화 | 대표 논문 | 읽어야 할 의미 |
| --- | --- | --- |
| **Explicit model과 feedback control의 지속** | [Physics-Based Adaptive Motion Primitives](https://doi.org/10.1109/ICRA48506.2021.9561221); [Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471); [Tight Convex Relaxations for Contact-Rich Motion Planning](https://doi.org/10.15607/RSS.2024.XX.132); [Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135); [Compliant Non-Prehensile Pushing](https://doi.org/10.48550/arXiv.2605.25672) `preprint` | Model-based method가 learning으로 단순 대체된 것은 아니다. Constraint, contact mode와 low-latency feedback은 여전히 중요한 역할을 맡는다. |
| **Interaction learning의 contact skill·recovery 확장** | [Dexterous Manoeuvre through Touch](https://doi.org/10.1109/ICRA48506.2021.9562061); [Goal-Oriented Non-Prehensile Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873); [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236); [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271); [HACMan++](https://doi.org/10.15607/RSS.2024.XX.129); [HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154); [PIN-WM](https://doi.org/10.15607/RSS.2025.XXI.153); [DAPL](https://doi.org/10.15607/RSS.2026.XXII.149) | Learning은 모든 contact mode를 사전에 열거하기보다 interaction outcome에서 closed-loop behavior를 얻는 방향으로 범위를 넓혔다. |
| **Demonstration·generalist policy의 reactive multimodal 확장** | [Multimodal Contact-Rich Skills from Demonstrations](https://doi.org/10.1109/ICRA48506.2021.9561734); [Diffusion Policy](https://doi.org/10.15607/RSS.2023.XIX.026); [π0](https://doi.org/10.48550/arXiv.2410.24164); [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052); [ForceVLA](https://doi.org/10.52202/085713-3124); [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) `preprint` | Contact feedback과 online correction은 RL만의 기능이 아니다. IL과 VLA도 tactile/F/T를 action generation 또는 force regulation에 결합한다. |
| **Contact formation·post-contact adaptation의 분리와 hybridization** | [GD2P](https://doi.org/10.48550/arXiv.2509.18455), ICRA 2026; [DexMove](https://openreview.net/forum?id=dT3ZciXvNX); [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Task direction에 맞는 pre-contact configuration을 만드는 문제와 접촉 이후 motion을 feedback으로 갱신하는 문제는 구분된다. 동시에 model prior와 interaction-based recovery의 역할을 나누는 hybrid가 등장한다. |

따라서 tactile/F/T, online correction 또는 hybrid 구성의 존재만으로 research gap을 주장할 수 없다. 중요한 것은 Scene·Workspace, Sensing·Object Geometry·Manipulation과 Method의 조합에서 기존 연구가 무엇을 해결했는지다.

---

## 3. 방법별 강점과 남는 한계

Nonprehensile manipulation에서는 hybrid contact transition, 누적되는 geometry·dynamics 오차와 safety constraint를 함께 다뤄야 한다. 아래 표는 특정 방법의 우열이 아니라 **어떤 정보와 수단으로 불확실성을 처리하며 무엇이 아직 어려운지**를 보여준다.

| 접근 방식 | 관련 method | 얻는 이점 | 남는 한계 |
| --- | --- | --- | --- |
| 사람이 설계한 rule과 feedback law | Heuristic / Control | 정의된 contact regime에서 낮은 latency와 해석 가능한 behavior를 제공하고 force·motion limit을 명시하기 쉽다. | Shape, friction, contact migration과 recovery가 다양해질수록 rule·threshold와 예외 case가 증가한다. |
| Explicit model·constraint·search 및 생성된 후보 | Optimization / Planning, Generative / Planning | 물리적 feasibility를 직접 다루거나 다봉적인 contact pose·trajectory 후보를 제안할 수 있다. | Non-smooth contact dynamics와 model error가 계산 결과를 약화시킬 수 있으며, 후보의 기하학적 plausibility가 실행 가능성과 recovery를 보장하지 않는다. |
| Demonstration과 pretrained representation | IL, VLA | 복잡한 multimodal action을 모방하거나 object·instruction 수준의 prior를 활용할 수 있다. | Demonstration coverage와 covariate shift가 recovery를 제한할 수 있고, semantic 이해가 high-rate contact feasibility를 보장하지 않는다. |
| 시행착오 상호작용과 task return | RL | Contact mode를 모두 열거하지 않고 closed-loop strategy와 recovery를 학습하며 장기 task outcome을 최적화할 수 있다. | Sample complexity, reward와 credit assignment, unsafe exploration, simulator exploitation 및 Sim-to-Real 부담이 남는다. |

Hybrid method는 explicit constraint, demonstration prior와 interaction-based recovery를 역할별로 결합할 수 있지만, module 간 state·time-scale·objective mismatch와 error propagation이 생기며 성능 향상의 원인을 특정 구성요소에 귀속하기 어려워진다. 어느 방법도 자동으로 충분하거나 배제되는 것은 아니다.

---

## 4. 현재 문제에서 RL을 우선하는 조건부 판단

앞의 일반적 비교만으로 RL이 선택되는 것은 아니다. 다음 세 조건이 현재 baseline 선택을 지지한다.

| 현재 연구 조건 | RL을 우선하는 이유 | 반드시 확인할 경계 |
| --- | --- | --- |
| Approximate OBB와 실제 contact surface·dynamics 사이에 오차가 있음 | Geometry·dynamics perturbation 아래의 실패와 recovery를 simulation interaction으로 반복 생성할 수 있다. | Geometry-error sweep, matched heuristic/model-based baseline과 tactile×F/T factorial ablation |
| Contact formation과 필요한 Rotation/Pivoting이 후속 Push outcome에 영향을 주고, multimodal wrist–finger action이 요구될 수 있음 | Full-episode return으로 앞선 contact formation과 최종 Rotation-to-Push outcome을 함께 최적화할 수 있다. | Rotation과 Rotation-to-Push를 분리한 continuation evaluation, sensor·action ablation |
| Real tactile/F/T demonstration은 제한적이지만 simulation interaction은 많이 확보할 수 있음 | Privileged simulator information은 reward·constraint에만 사용하고 actor는 deployable observation으로 학습할 수 있다. | Sensor corruption·dynamics randomization과 real transfer 평가 |

Safety는 RL을 선택하는 근거가 아니라 독립 constraint다. Collision cost·termination 또는 constrained RL을 사용하더라도 hard guarantee가 생기는 것은 아니므로 violation metric과 hardware safety supervisor를 별도로 둔다.

> **RL은 다른 manipulation 방법보다 보편적으로 우월해서 선택하는 것이 아니다. 현재 문제는 task goal은 명확하지만 geometry와 contact outcome이 불확실하고, real demonstration보다 simulation interaction을 더 많이 확보할 수 있으며, contact feedback에 따른 recovery와 최종 Rotation-to-Push outcome을 함께 최적화해야 하므로 RL을 먼저 구현한다.**

이 결론의 상태는 `[Baseline]`이다. 다음 중 하나가 확인되면 RL 선택의 근거가 약해지거나 기각된다.

- 동일한 sensing·action budget에서 heuristic/model-based 또는 IL baseline과 차이가 없음
- Geometry perturbation이나 contact disturbance가 커질수록 RL의 상대 성능이 개선되지 않음
- Simulation에서는 성공하지만 real tactile/F/T 조건에서 성능이 유지되지 않음

RL 자체, phase-gated reward 또는 multimodal input의 사용은 contribution이 아니다. Contribution은 matched experiment에서 approximate-geometry error에 대한 보완 효과와 Rotation-to-Push outcome의 개선이 입증된 뒤에만 확정한다.

---

## 5. 가까운 선행연구와 남은 질문

이 문서가 확정하는 것은 현재 조건에서 `RL을 먼저 구현한다`는 `[Baseline]` 판단뿐이다. Planning/control, IL와 VLA도 contact feedback과 online correction을 다루므로 RL 선택 자체는 research gap이나 contribution이 아니다. 이제 질문은 `왜 RL인가?`가 아니라, 동일한 task·geometry·sensor·action 조건과 가까운 연구가 무엇을 이미 해결했는가로 바뀐다.

> **우리와 가까운 nonprehensile/contact-feedback 연구는 geometry, sensing, tool action과 online adaptation의 측면에서 무엇을 이미 해결했으며, 어떤 질문을 matched experiment로 검증해야 하는가?**

이 질문은 [Previous Works](./previous_works.md)에서 Environment, Robot Agent와 System의 여섯 공통 column을 비교해 다룬다.
