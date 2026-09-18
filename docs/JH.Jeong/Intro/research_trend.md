# Research Trend — From Method Evolution to Conditional RL Selection

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Previous Works](./previous_works.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 2021년 이후 nonprehensile 및 contact-feedback manipulation의 방법론적 변화를 보여주고, 각 method family가 nonprehensile manipulation 자체를 해결할 때 갖는 강점과 구조적 한계를 구분한 뒤 현재 연구에서 RL을 우선 구현하는 이유를 도출한다. 개별 논문의 세부 비교와 연구 gap 판정은 [Previous Works](./previous_works.md)에서 수행한다.

---

## 1. Motivation에서 넘어온 질문

앞선 [Research Motivation](./research_motivation.md)은 연구 대상을 `approximate geometry 아래의 contact-feedback nonprehensile manipulation`으로 좁혔다. 그러나 특정 연구 조건에 바로 방법을 대응시키면 각 family의 일반적인 한계와 현재 구현상의 부족을 혼동하게 된다. 따라서 이 문서는 두 질문을 순서대로 다룬다.

> **첫째, heuristic/control, planning, RL, IL, VLA와 hybrid method는 nonprehensile manipulation의 접촉 계획·실행·복구 문제를 어떻게 해결하며, task 자체의 어떤 어려움 때문에 한계를 갖는가?**
>
> **둘째, 이러한 일반적 trade-off를 고려할 때 현재의 approximate-geometry, contact-feedback, low-level sequential manipulation에는 어떤 framework를 우선할 것인가?**

Research Trend는 이 질문에 곧바로 `RL이 가장 우월하다`고 답하지 않는다. 대신 다음 순서로 판단한다.

1. Heuristic/control, optimization/planning, RL, IL과 VLA가 무엇을 해결해 왔는지 확인한다.
2. Contact feedback과 online correction은 특정 family만의 기능이 아니라는 점을 확인한다.
3. 각 family의 한계를 현재 연구 설정이 아니라 nonprehensile manipulation의 공통 난제에서 도출한다.
4. 그 trade-off를 현재 data·model·interaction 조건에 대입해 RL을 **조건부 baseline choice**로 선택하고, 이 선택을 기각할 조건도 함께 정의한다.

### 1.1 Method 분류 기준

Method는 sensor 종류가 아니라 **deployment action을 생성하는 주된 방식**으로 분류한다. 이 기준은 [Previous Works의 System 표](./previous_works.md#33-system)와 동일하다.

| Method family | 판정 기준 | 대표 형태 |
| --- | --- | --- |
| Heuristic / Control | 사람이 정의한 rule, motion primitive 또는 feedback law가 action을 결정 | Scripted primitive, tactile servoing, impedance control |
| Optimization / Planning | 명시적 model·constraint·search 또는 trajectory optimization으로 action을 계산 | Contact-mode optimization, MPC, search |
| Generative / Planning | 후보 contact pose나 trajectory를 생성한 뒤 feasibility·planning으로 선택 | Diffusion-based contact-pose generation + motion planning |
| RL | Environment interaction에서 얻은 return으로 policy 또는 value를 최적화 | PPO/SAC, model-based RL, constrained RL |
| IL | Demonstration action을 주된 supervision으로 policy를 학습 | Behavior cloning, diffusion/flow policy |
| VLA | Vision-language representation과 action generation을 generalist policy로 결합 | π0 계열, force/tactile-aware VLA |
| Hybrid | 둘 이상의 방식이 최종 action 생성에 필수적으로 함께 관여 | Optimization + RL, VLA + force controller |

`Diffusion`이라는 architecture만으로 IL 또는 generative planning을 판정하지 않는다. Demonstration action을 모방하면 IL이고, contact-pose 후보를 생성해 planner가 선택·실행하면 generative/planning으로 구분한다.

---

## 2. 2021–2026 대표 연구 Timeline

이 timeline은 exhaustive survey가 아니라 **방법 또는 연구 범위를 바꾼 대표 사례**만 남긴다. Timeline 안에서는 별도의 `Direct/Adjacent` label을 쓰지 않는다. 우리 task와의 직접성은 [Previous Works](./previous_works.md)의 논문별 evidence에서 판단한다.

| 연도 | Heuristic / Control / Planning | RL | IL | VLA | Hybrid |
| --- | --- | --- | --- | --- | --- |
| 2021 | [Physics-Based Adaptive Motion Primitives](https://doi.org/10.1109/ICRA48506.2021.9561221); [Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) | [Dexterous Manoeuvre through Touch](https://doi.org/10.1109/ICRA48506.2021.9562061) | — | — | [Multimodal Contact-Rich Skills from Demonstrations](https://doi.org/10.1109/ICRA48506.2021.9561734) `(LfD + RL)` |
| 2022 | — | [Goal-Oriented Non-Prehensile Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) | — | — | — |
| 2023 | — | [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236); [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) | [Diffusion Policy](https://doi.org/10.15607/RSS.2023.XIX.026) | — | — |
| 2024 | [Tight Convex Relaxations for Contact-Rich Motion Planning](https://doi.org/10.15607/RSS.2024.XX.132) | [HACMan++](https://doi.org/10.15607/RSS.2024.XX.129) | — | [π0](https://doi.org/10.48550/arXiv.2410.24164) | [Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135) `(Optimization + Control)` |
| 2025 | [GD2P](https://doi.org/10.48550/arXiv.2509.18455) `preprint` | [HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154); [PIN-WM](https://doi.org/10.15607/RSS.2025.XXI.153) | [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052) | [ForceVLA](https://doi.org/10.52202/085713-3124) | [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) `(VLA + Control; preprint)` |
| 2026 | [Compliant Non-Prehensile Pushing](https://doi.org/10.48550/arXiv.2605.25672) `preprint` | [DAPL](https://doi.org/10.15607/RSS.2026.XXII.149) | [DexMove](https://openreview.net/forum?id=dT3ZciXvNX) | — | [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) `(Optimization + RL)` |

Hybrid column에는 두 방식이 최종 action generation에 모두 필요한 연구만 넣는다. 단순히 learned encoder를 control pipeline에 포함했다는 이유만으로 Hybrid로 분류하지 않으며, 한 논문을 단일-family column과 Hybrid column에 중복 기재하지 않는다.

### Timeline에서 읽어야 할 네 가지 변화

1. **Model-based에서 learning-based로 단순 대체된 것이 아니다.** Planning/control은 constraint와 contact mode를 명시하는 강점을 유지하고, learning method는 불확실한 outcome과 넓은 상태 분포를 다루는 방향으로 확장되었다.
2. **Contact feedback은 RL만의 특징이 아니다.** Tactile servoing, reactive IL, ForceVLA와 Tactile-VLA 모두 실행 중 tactile/F/T를 action 생성이나 force regulation에 사용할 수 있다.
3. **Contact 형성과 contact 이후 적응이 분리되기 시작했다.** GD2P는 task direction에 맞는 initial dexterous contact pose를 생성하고, DexMove는 접촉 이후 wrist–finger motion을 feedback으로 갱신한다. 두 기능을 같은 것으로 취급해서는 안 된다.
4. **최신 흐름은 family 간 hybridization이다.** Optimization prior + RL, slow visual policy + fast tactile branch, VLA + force controller처럼 model·demonstration·interaction learning의 역할을 나누는 방식이 증가하고 있다.

---

## 3. Nonprehensile manipulation에서 방법별 강점과 구조적 한계

여기서 `한계`는 현재 연구가 특정 sensor나 geometry representation을 사용하기 때문에 생기는 구현상 부족을 뜻하지 않는다. Nonprehensile manipulation이 공통으로 갖는 다음 어려움에 대해 각 방법이 부담해야 하는 model·data·interaction·computation의 경계를 뜻한다.

1. 물체가 grasp로 구속되지 않아 접촉 위치와 마찰에 따라 motion outcome이 크게 달라진다.
2. Stick, slip, pivot, separation과 re-contact가 이어지는 hybrid contact-mode transition을 다뤄야 한다.
3. 작은 state·geometry·dynamics 오차가 장기 trajectory에서 누적되므로 closed-loop correction과 recovery가 필요하다.
4. Robot·object·environment contact를 이용하면서도 collision, toppling과 excessive force를 제한해야 한다.

| Method | Nonprehensile manipulation에서의 핵심 강점 | Task 자체를 해결할 때의 구조적 한계 | 한계가 두드러지는 조건 |
| --- | --- | --- | --- |
| Heuristic / Control | 설계된 contact regime에서는 낮은 latency로 해석 가능한 feedback behavior를 제공하며, force·motion limit을 명시하기 쉽다. | 가능한 contact mode와 recovery를 사람이 rule·state transition·threshold로 표현해야 한다. Shape, friction과 접촉 순서가 다양해질수록 rule interaction과 예외 case가 조합적으로 증가한다. | Unseen object, contact migration, 반복되는 slip–separation–re-contact, clutter interaction |
| Optimization / Planning | Geometry, collision, friction cone, force와 contact-mode constraint를 하나의 명시적 문제로 구성하고 물리적 feasibility를 직접 다룰 수 있다. | Contact dynamics는 non-smooth·hybrid하며 다중 접촉에서는 mode 수와 비선형성이 증가한다. Model·state가 부정확하면 계산된 trajectory의 feasibility가 약해지고, online replanning은 계산 시간과 수렴성 제약을 받는다. | Multi-contact dexterous manipulation, uncertain friction/contact point, fast disturbance |
| Generative / Planning | 다봉적인 contact pose·trajectory 후보를 생성하여 하나의 nominal solution에 고정되는 문제를 줄이고, geometry와 task 조건에 따른 다양한 초기 해를 제안할 수 있다. | 생성된 후보의 기하학적 plausibility가 실제 동역학적 실행 가능성이나 disturbance recovery를 보장하지 않는다. Feasibility scorer, planner와 feedback controller의 품질에 최종 성능이 의존한다. | Unseen geometry, sparse feasible contact set, 접촉 후 예상 밖의 mode transition |
| RL | Contact mode를 모두 명시하지 않고 interaction outcome으로 closed-loop strategy와 recovery를 학습할 수 있으며, 긴 horizon의 최종 task outcome을 최적화할 수 있다. | Sample complexity, reward specification과 temporal credit assignment 부담이 크다. Unsafe exploration, simulator exploitation과 Sim-to-Real gap이 생길 수 있고 constraint 만족과 unseen-condition generalization을 자동으로 보장하지 않는다. | High-dimensional wrist–finger action, sparse success, safety-critical real deployment |
| IL | Expert가 사용하는 smooth contact transition과 고차원 multimodal action을 직접 학습하며, reward를 완전히 설계하지 않고도 복잡한 skill distribution을 표현할 수 있다. | Policy가 방문할 상태의 coverage가 demonstration support에 제한되며 작은 실행 오차가 covariate shift로 누적될 수 있다. 실패·복구와 희귀 contact transition을 포함한 demonstration은 수집하기 어렵다. | Disturbance recovery, unseen initial contact, long horizon, synchronized tactile/F/T demonstration |
| VLA | 대규모 vision–language prior를 이용해 object·instruction·task 수준의 generalization과 skill selection을 제공할 수 있다. ForceVLA와 Tactile-VLA는 force/tactile grounding도 가능한 방향임을 보여준다. | Semantic task 이해가 접촉의 동역학적 feasibility를 보장하지 않는다. High-rate·low-latency contact correction, embodiment/action-space 차이와 force/tactile data scarcity는 별도의 architecture·data·controller를 요구한다. | 정밀 force regulation, 빠른 stick–slip 변화, embodiment-specific dexterous control |
| Hybrid | Model의 constraint, demonstration의 prior와 interaction learning의 recovery를 역할별로 결합해 단일 family의 약점을 보완할 수 있다. | Module 간 state·time-scale·objective mismatch와 error propagation이 생길 수 있다. Pipeline 복잡도와 계산량이 증가하고, 성능 향상의 원인을 특정 구성요소에 귀속하기 어렵다. | Planner–policy 전환, slow–fast controller 결합, 복수 sensor·objective 통합 |

이 표는 어느 family도 nonprehensile manipulation을 해결할 수 없다는 주장이 아니다. 오히려 각각이 **어떤 자원을 사용해 접촉 불확실성을 처리하고 어디에 해결 부담을 남기는지**를 분리한다. 최신 흐름이 hybridization으로 이동하는 이유도 한 family가 절대적으로 우월해서가 아니라, 명시적 constraint·demonstration prior·interaction-based recovery가 서로 다른 문제를 해결하기 때문이다.

---

## 4. 현재 문제에서 RL을 우선하는 판단

### 4.1 연구 조건과 방법 선택의 연결

3절의 일반적 비교만으로 RL이 선택되는 것은 아니다. 아래 표는 그 trade-off를 현재 연구의 model·data·interaction 조건에 대입한 결과다.

| 현재 연구 조건 | RL을 우선할 수 있는 이유 | RL만으로 해결되지 않는 사항 | 필요한 검증 |
| --- | --- | --- | --- |
| Approximate OBB와 실제 접촉 표면 사이에 오차가 있음 | Geometry·dynamics perturbation 아래의 실패와 recovery를 simulation interaction으로 반복 생성할 수 있음 | 학습된 perturbation 범위 밖의 perception·geometry error까지 자동으로 보완하지는 않음 | Geometry-error level별 success와 tactile/F/T ablation |
| Approach→Rotation→Push의 초기 행동이 최종 성공에 영향을 줌 | Full-episode return과 phase-gated shaping으로 앞선 action과 최종 outcome을 함께 최적화할 수 있음 | 긴 horizon의 credit assignment가 downstream-ready contact formation을 실제로 만들지는 미확인 | Rotation success와 Rotation-to-Push success를 분리한 continuation evaluation |
| Tactile·wrist F/T에 따라 wrist와 fingers를 조정해야 할 가능성이 있음 | Multimodal feedback에서 EEF–finger action을 하나의 closed-loop policy로 학습할 수 있음 | Policy가 sensor를 무시하거나 spurious simulation correlation에 의존할 수 있음 | Vision/proprioception-only, tactile-only addition, F/T-only addition과 combined ablation |
| Real tactile/F/T demonstration을 대규모로 수집하기 어려움 | Privileged simulator information을 reward·critic에만 사용하고 actor는 deployable observation으로 많은 interaction을 학습할 수 있음 | Simulator contact와 실제 sensor response 사이의 gap은 RL이 자동으로 제거하지 않음 | Sensor corruption·dynamics randomization과 real transfer 평가 |
| Shelf 및 주변 구조물 접촉을 제한해야 함 | Collision cost·termination 또는 constrained RL을 rollout에 포함할 수 있음 | Penalty 기반 학습은 hard safety guarantee가 아니며 unsafe exploration과 reward hacking 가능성이 남음 | 독립 safety metric, violation rate와 hardware safety supervisor |

### 4.2 선택 결론

> **RL은 다른 manipulation 방법보다 보편적으로 우월해서 선택하는 것이 아니다. 현재 문제는 task goal은 명확하지만 geometry와 contact outcome이 불확실하고, real demonstration보다 simulation interaction을 더 많이 확보할 수 있으며, contact feedback에 따른 recovery와 최종 Rotation-to-Push outcome을 함께 최적화해야 하므로 RL을 primary learning framework로 우선한다.**

이 결론의 상태는 `[Baseline]`이다. 다음 결과가 나오면 RL 선택의 근거가 약해지거나 기각된다.

- 동일한 sensing·action budget에서 heuristic/model-based 또는 IL baseline과 차이가 없음
- Geometry perturbation이나 contact disturbance가 커질수록 RL의 상대 성능이 개선되지 않음
- Simulation에서는 성공하지만 real tactile/F/T 조건에서 성능이 유지되지 않음
- Wrist–finger joint policy가 더 단순한 fixed-hand 또는 wrist-only policy보다 이득을 보이지 않음

RL 자체, phase-gated reward 또는 multimodal input의 사용은 contribution이 아니다. Contribution은 matched experiment에서 approximate-geometry error에 대한 보완 효과와 Rotation-to-Push outcome의 개선이 입증된 뒤에만 확정한다.

---

## 5. Method 선택에서 research-gap 분석으로

이 문서가 확정하는 것은 `RL을 먼저 구현한다`는 `[Baseline]` 판단뿐이다. Timeline에서 확인했듯 planning/control, IL와 VLA도 contact feedback과 online correction을 다룰 수 있으므로, RL을 선택했다는 사실은 research gap이나 contribution이 아니다.

| 이 문서에서 얻은 결론 | 아직 답하지 못한 질문 | 다음 문서의 역할 |
| --- | --- | --- |
| Simulation interaction과 task return을 활용할 수 있는 현재 조건에서는 RL을 primary framework로 우선함 | 같은 task·geometry·sensor·action 조건과 가까운 연구가 이미 어디까지 해결했는가? | Closest systems를 Environment–Agent–System 기준으로 비교 |
| Contact feedback과 online adaptation은 여러 method family에 이미 존재함 | Approximate geometry, coarse sensing과 sequential outcome의 어떤 교차점이 실제로 남는가? | 기존 성과와 미검증 질문을 분리 |
| RL 선택에는 Sim-to-Real, reward와 safety 위험이 따름 | 어떤 baseline과 metric으로 이 선택과 제안 요소를 반증할 수 있는가? | Candidate contribution과 필요한 ablation 도출 |

따라서 다음 질문은 `왜 RL인가?`에서 `RL을 포함한 가까운 연구와 비교했을 때 정확히 무엇이 아직 검증되지 않았는가?`로 바뀐다.

> **우리와 가까운 nonprehensile/contact-feedback system은 geometry, sensing, tool action과 online adaptation의 측면에서 무엇을 이미 해결했으며, 어떤 질문을 matched experiment로 검증해야 하는가?**

이 질문은 다음 문서인 [Previous Works](./previous_works.md)에서 다룬다.
