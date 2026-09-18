# Research Trend — From Method Evolution to Conditional RL Selection

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Previous Works](./previous_works.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 2021년 이후 nonprehensile 및 contact-feedback manipulation의 방법론적 변화를 보여주고, 현재 연구 조건에서 RL을 우선 구현하는 이유를 도출한다. 개별 논문의 세부 비교와 연구 gap 판정은 [Previous Works](./previous_works.md)에서 수행한다.

---

## 1. Motivation에서 넘어온 질문

앞선 [Research Motivation](./research_motivation.md)은 연구 대상을 `approximate geometry 아래의 contact-feedback nonprehensile manipulation`으로 좁혔다. 그러나 문제 정의만으로 RL, IL, VLA 또는 model-based method 중 무엇을 선택할지는 결정되지 않는다. 따라서 이 문서는 다음 질문에서 시작한다.

> Approximate geometry만 주어진 상황에서 실제 접촉 상태를 feedback으로 보정하며, task에 적합한 접촉을 형성하고 Rotation과 Push를 연속적으로 수행하려면 어떤 학습·제어 framework가 적합한가?

Research Trend는 이 질문에 곧바로 `RL이 가장 우월하다`고 답하지 않는다. 대신 다음 순서로 판단한다.

1. Heuristic/control, optimization/planning, RL, IL과 VLA가 무엇을 해결해 왔는지 확인한다.
2. Contact feedback과 online correction은 특정 family만의 기능이 아니라는 점을 확인한다.
3. 각 family를 현재 data·model·interaction 조건에 적용할 때 추가로 필요한 것을 비교한다.
4. 그 결과로 RL을 **조건부 baseline choice**로 선택하고, 이 선택을 기각할 조건도 함께 정의한다.

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

## 3. Method별로 해결된 부분과 남는 문제

| Method | 이미 해결한 부분 | 현재 연구 조건에서 추가로 필요한 부분 | 본 연구에서의 위치 |
| --- | --- | --- | --- |
| Heuristic / Control | 단순한 동작을 낮은 계산 비용으로 실행하며, 알려진 contact condition에서 예측 가능한 feedback behavior를 구성할 수 있음 | Approximate OBB 때문에 nominal contact가 달라지고 contact migration·slip·separation 조합이 증가하면 rule과 recovery case가 빠르게 늘어남 | Scripted minimum baseline, reset behavior, low-level controller |
| Optimization / Planning | Geometry, collision, force와 contact-mode constraint를 명시할 수 있고 data-efficient하며 failure 원인을 해석하기 쉬움 | Geometry·friction·contact state가 부정확하면 최적화 문제의 전제 자체가 틀릴 수 있으며, dexterous wrist–finger contact를 online으로 반복 추정·재계획해야 함 | Safety constraint, model-based baseline, demonstration/reference source |
| Generative / Planning | Object geometry와 task direction에 맞는 다양한 pre-contact candidate를 생성하고 feasibility로 선택할 수 있음 | 선택한 initial configuration이 실제 접촉 오차나 Rotation→Push 중의 contact change에 적응하는지는 별도 feedback mechanism이 필요함 | Task-conditioned contact-formation baseline |
| RL | Simulation에서 perturbation과 recovery를 반복 경험하고, contact feedback과 delayed task outcome을 하나의 return으로 최적화할 수 있음 | Reward·reset·randomization에 민감하고 많은 interaction이 필요하며, simulator contact와 real sensor 사이의 gap이 남음 | 현재 low-level policy의 primary framework |
| IL | Demonstration에 포함된 자연스러운 multimodal trajectory와 high-dimensional action distribution을 안정적으로 학습할 수 있음 | Geometry error, 잘못된 초기 contact, slip·contact loss와 recovery를 학습하려면 해당 사례가 demonstration에 포함되어야 하며 tactile/F/T wrist–finger data 수집 비용이 큼 | 동일 observation/action 조건의 learning baseline, 향후 pretraining 후보 |
| VLA | Language instruction, semantic prior와 cross-task transfer를 제공하며, 최신 연구는 force/tactile도 action generation에 통합함 | Contact-level correction을 위해서는 해당 sensor가 포함된 task data와 architecture가 필요하며, 고정된 low-level task에서 large generalist model의 추가 이득은 별도 검증이 필요함 | 상위 task/goal interface와 contact-aware generalist reference |
| Hybrid | Model의 constraint, demonstration의 안정성과 interaction learning의 recovery 능력을 결합할 수 있음 | 구성요소별 기여와 data·compute·sensor budget을 통제하지 않으면 공정한 비교가 어려움 | RL 단독안 이후 검토할 경쟁 방법 |

이 표의 한계는 `어떤 family도 문제를 해결하지 못한다`는 뜻이 아니다. 각 방법이 현재 task에 필요한 기능을 제공하려면 **어떤 추가 model, data 또는 feedback mechanism이 필요한가**를 구분한 것이다.

---

## 4. 현재 문제에서 RL을 우선하는 판단

### 4.1 연구 조건과 방법 선택의 연결

| 현재 조건 | 다른 방법만 사용할 때 추가로 필요한 것 | RL을 우선할 수 있는 이유 | 반드시 검증할 위험 |
| --- | --- | --- | --- |
| Approximate OBB와 실제 접촉 표면 사이에 오차가 있음 | Planning/control에는 정확한 contact state 추정과 online replanning이 필요하고, IL에는 다양한 geometry-error demonstration이 필요 | Geometry·dynamics perturbation 아래에서 접촉 실패와 recovery를 simulation interaction으로 생성 가능 | 학습 perturbation이 실제 perception·geometry error를 대표하는가? |
| Approach→Rotation→Push의 초기 행동이 최종 성공에 영향을 줌 | 단계별 독립 rule이나 demonstration만으로는 final continuation objective를 별도로 설계해야 함 | Full episode return과 phase-gated shaping으로 연속 결과를 직접 최적화 가능 | Shared return만으로 downstream-aware contact formation이 생기는가? |
| Tactile·wrist F/T에 따라 wrist와 fingers를 계속 조정해야 할 가능성이 있음 | Fixed contact pose에는 별도 online correction module이 필요함 | Multimodal feedback에서 continuous EEF–finger action을 joint policy로 학습 가능 | Fixed-hand 또는 wrist-only policy보다 실제 이득이 있는가? |
| Real tactile/F/T demonstration을 대규모로 수집하기 어려움 | IL/VLA는 실패·복구와 sensor variation을 포함한 data collection이 필요함 | Privileged simulator information을 reward/critic에만 사용하고 actor는 deployable observation으로 학습 가능 | Contact model과 sensor representation의 Sim-to-Real gap을 넘는가? |
| Shelf 및 주변 구조물 접촉을 제한해야 함 | Pure trial-and-error에는 safety mechanism이 부족함 | Collision cost·termination과 constrained exploration을 학습에 포함할 수 있음 | Reward hacking 없이 constraint violation이 실제로 감소하는가? |

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
