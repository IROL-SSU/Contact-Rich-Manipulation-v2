# Research Trend — Nonprehensile and Contact-Feedback Manipulation

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Previous Works](./previous_works.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 2021년 이후의 대표 연구를 method family별 timeline으로 정리하고, 우리 문제에서 RL을 primary learning framework로 선택하는 이유와 그 한계를 함께 설명한다.

---

## 1. Timeline의 범위와 분류 규칙

Timeline에는 두 종류의 논문이 들어간다.

- `Direct`: pushing, pivoting, rearrangement 등 nonprehensile manipulation을 직접 해결한다.
- `Adjacent`: contact-rich manipulation을 위한 IL·VLA·multimodal feedback의 변화를 보여주지만, 우리 task와 직접 비교할 baseline은 아니다.

Method family는 논문의 모든 구성요소가 아니라 **deployment action을 생성하는 주된 mechanism**으로 판정한다.

| Family | 판정 기준 | 포함 예시 |
| --- | --- | --- |
| Planning / Control | 명시적 model, contact mode, trajectory optimization, search 또는 hand-designed feedback law가 action을 생성 | Physics-based search, tactile servoing, contact-mode optimization |
| RL | Environment interaction과 return을 통해 policy·value를 최적화 | PPO/SAC, constrained RL, model-based RL |
| IL | Demonstration action을 주된 supervision으로 policy를 학습 | Behavior cloning, diffusion/flow policy |
| VLA | Vision·language representation과 action generation을 하나의 generalist policy에 결합 | π0 계열, ForceVLA |
| Hybrid | 둘 이상의 family가 핵심적인 경우 병기 | Optimization demonstration + RL, IL base policy + residual RL |

따라서 `diffusion`은 자동으로 IL이 아니고, `world model`도 자동으로 planning이 아니다. 실제 objective와 policy update 방식을 확인하여 분류한다.

---

## 2. 2021–2026 연구 흐름

| 연도 | Planning / Control | RL | IL | VLA | 흐름에서의 의미 |
| --- | --- | --- | --- | --- | --- |
| 2021 | [Physics-Based Adaptive Motion Primitives](https://doi.org/10.1109/ICRA48506.2021.9561221) `Direct`; [Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) `Direct` | [Dexterous Manoeuvre through Touch](https://doi.org/10.1109/ICRA48506.2021.9562061) `Direct` | [Multimodal Contact-Rich Skills from Demonstrations](https://doi.org/10.1109/ICRA48506.2021.9561734) `Adjacent; LfD/RL hybrid` | — | 명시적 physics/search·tactile servoing과 함께 tactile RL 및 demonstration-derived reward/policy learning이 이미 병행 |
| 2022 | — | [Goal-Oriented Non-Prehensile Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) `Direct` | — | — | RL이 unknown dynamics와 collision/contact-maintenance objective를 포함한 closed-loop pushing으로 확장 |
| 2023 | — | [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236) `Direct`; [Generalizable Pivoting](https://doi.org/10.1109/ICRA48891.2023.10161271) `Direct` | [Diffusion Policy](https://doi.org/10.15607/RSS.2023.XIX.026) `Adjacent` | — | RL은 tactile feedback·sim-to-real·unseen-object skill을, IL은 multimodal action distribution과 action sequence modeling을 강화 |
| 2024 | [Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135) `Direct`; [Tight Convex Relaxations](https://doi.org/10.15607/RSS.2024.XX.132) `Direct` | [HACMan++](https://doi.org/10.15607/RSS.2024.XX.129) `Direct`; [Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157) `Direct` | [3D-ViTac](https://doi.org/10.48550/arXiv.2410.24091) `Adjacent` | [π0](https://doi.org/10.48550/arXiv.2410.24164) `Adjacent` | Contact-mode optimization과 geometry-grounded RL이 함께 발전하고, IL·VLA가 manipulation policy의 범용성과 표현력을 확장 |
| 2025 | — | [HyDo](https://doi.org/10.1109/LRA.2025.3564780) `Direct`; [HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154) `Direct`; [PIN-WM](https://doi.org/10.15607/RSS.2025.XXI.153) `Direct` | [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052) `Adjacent` | [ForceVLA](https://doi.org/10.52202/085713-3124) `Adjacent` | RL은 contact selection·general environment·world-model adaptation으로 확장되고, IL·VLA도 tactile/F/T를 이용한 reactive control을 다루기 시작 |
| 2026 | [Compliant Non-Prehensile Pushing](https://doi.org/10.48550/arXiv.2605.25672) `Direct` | [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) `Direct`; [DAPL](https://doi.org/10.15607/RSS.2026.XXII.149) `Direct` | [DexMove](https://openreview.net/forum?id=dT3ZciXvNX) `Direct` | — | Model·demonstration·RL의 hybridization, clutter dynamics와 tactile wrist–finger control이 직접적인 비교 대상으로 등장 |

### Timeline에서 읽어야 할 변화

1. `Planning/Control → Learning`으로 일방적으로 대체된 것이 아니다. 명시적 model과 optimization은 여전히 data efficiency, interpretability와 constraint handling에 강하다.
2. RL은 contact outcome을 정확히 모델링하기보다 반복 interaction과 task return에서 closed-loop behavior를 학습하는 방향으로 확장되었다.
3. IL은 고차원·multimodal action sequence를 안정적으로 재현하는 방향으로 발전했고, 2025년 이후 tactile/F/T를 이용한 fast reactive branch까지 포함한다.
4. VLA는 semantic instruction과 cross-task transfer를 확장했으며, ForceVLA와 같이 force를 직접 입력하는 반례도 존재한다.
5. 최신 흐름은 family 간 대체보다 `planning/optimization + RL`, `IL + online correction`, `VLA + RL` 같은 hybridization이다.

---

## 3. Method family별 장단점

| Family | 강점 | 구조적 한계 | 우리 문제에서의 역할 |
| --- | --- | --- | --- |
| Planning / Control | 물리 constraint를 명시할 수 있고 data-efficient하며 failure 원인을 해석하기 쉽다. | 정확한 geometry·friction·contact mode가 필요하고, contact sequence와 clutter가 늘면 search·modeling 비용이 커진다. | Safety/controller baseline과 privileged demonstration source |
| RL | Simulation에서 다양한 contact outcome과 recovery를 탐색하고, sensor feedback과 delayed task success를 하나의 objective로 최적화할 수 있다. | Reward·reset·randomization 설계와 많은 interaction이 필요하며 Sim-to-Real gap과 unsafe exploration 문제가 있다. | 현재 low-level policy의 primary framework |
| IL | Reward engineering 없이 자연스럽고 multimodal한 trajectory를 학습하며 diffusion/flow로 high-dimensional action을 표현하기 좋다. | Demonstration에 없는 geometry error·contact loss·failure recovery의 coverage가 약하며 tactile/force demonstration 수집 비용이 높다. | 강한 learning baseline과 향후 hybrid 후보 |
| VLA | Language-conditioned multi-task behavior, semantic prior와 broad transfer에 강하다. | 대규모 data·compute가 필요하고 low-level force/contact precision은 별도 sensor data와 architecture를 요구한다. | 상위 task interpretation 또는 high-level command source; 현재 low-level contribution의 직접 baseline은 제한적 |
| Hybrid | Model의 sample efficiency, demonstration의 안정성과 RL의 online optimization을 결합할 수 있다. | 구성요소별 기여와 data/compute budget을 분리하지 않으면 공정한 비교가 어렵다. | B81과 같은 가장 현실적인 경쟁 방향 |

이 표는 어느 family의 보편적 우위를 주장하지 않는다. 동일한 task와 정보 budget을 통제하지 않은 성공률의 단순 비교도 사용하지 않는다.

---

## 4. 우리 문제에서 RL을 선택하는 이유

RL의 장점만으로 방법 선택을 정당화하지 않는다. 먼저 기존 방법이 해결한 부분을 인정하고, **각 방법을 단독으로 적용했을 때 우리 문제의 어떤 요구가 남는지**를 확인해야 한다. 아래 한계는 해당 family가 본질적으로 해결할 수 없다는 뜻이 아니라, 현재 연구 조건에서 추가 model·data·mechanism이 필요하다는 뜻이다.

### 4.1 다른 방법만으로 충분하지 않은 이유

| 대안 | 이미 잘 해결하는 부분 | 우리 조건에서 단독 적용할 때 남는 문제 | 본 연구에서 유지할 역할 |
| --- | --- | --- | --- |
| Heuristic / motion primitive | 동작 구조가 단순하고 계산 비용과 구현 위험이 낮으며, 알려진 contact mode에는 예측 가능한 행동을 제공한다. [Physics-Based Adaptive Motion Primitives](https://doi.org/10.1109/ICRA48506.2021.9561221)처럼 물리 prior를 이용한 적응도 가능하다. | Approximate OBB의 위치·방향·크기 오차로 nominal contact와 실제 contact가 달라지면 규칙별 예외 처리가 필요하다. Approach–Rotation–Push 중 contact migration, slip 또는 separation이 발생할 때 가능한 상태 조합이 늘어나므로 고정 규칙의 확장이 어렵다. | 초기 action prior, reset behavior와 해석 가능한 baseline |
| Model-based planning / optimization / control | Geometry, collision, contact constraint와 safety condition을 명시적으로 다룰 수 있고 data-efficient하다. [Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135)은 tactile feedback을 이용한 contact-mode control도 가능함을 보여준다. | 정확한 geometry·friction·contact mode 또는 신뢰할 수 있는 online state estimate가 필요하다. 우리 문제에서는 coarse geometry와 실제 contact의 불일치 때문에 계획에 사용한 contact mode 자체가 틀릴 수 있으며, wrist–finger contact sequence를 매번 재추정·재계획하는 비용도 고려해야 한다. | OSC/Differential IK, safety constraint와 optimization-guided baseline |
| IL | Demonstration에 포함된 자연스러운 multimodal trajectory와 고차원 action distribution을 안정적으로 재현할 수 있다. [Diffusion Policy](https://doi.org/10.15607/RSS.2023.XIX.026)와 [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052)는 action sequence와 reactive correction의 강점을 보여준다. | Demonstration이 포함하지 않은 geometry perturbation, 잘못된 초기 contact, slip·contact loss와 recovery를 학습하려면 해당 실패와 복구가 포함된 coverage가 필요하다. 특히 tactile/F/T가 포함된 wrist–finger demonstration을 다양한 물체·마찰·오차 조건에서 수집하는 비용이 크며, imitation objective만으로는 최종 Rotation-to-Push 성공을 직접 최적화하지 않는다. | 동일 observation/action을 사용하는 강한 learning baseline, 향후 demonstration pretraining 후보 |
| VLA | Language instruction, semantic prior와 여러 task 간 transfer에 강하다. [π0](https://doi.org/10.48550/arXiv.2410.24164) 이후 [ForceVLA](https://doi.org/10.52202/085713-3124)처럼 force feedback을 포함하는 방향으로도 발전했다. | Tactile/F/T를 사용하지 못하는 방법이라고 일반화할 수는 없지만, contact-level correction에는 해당 sensor가 포함된 대규모 trajectory와 architecture가 별도로 필요하다. 현재 문제는 상위 instruction 해석보다 millimeter-scale contact 변화와 연속 wrist–finger correction이 핵심이므로 generalist model의 규모가 직접적인 이점을 보장하지 않는다. | 상위 task command·goal generation, 향후 generalist-policy interface |

따라서 다른 family의 약점만으로 RL이 자동으로 정답이 되는 것은 아니다. 우리 조건에서 RL을 선택하려면 다음 조건도 동시에 성립해야 한다.

1. 성공과 실패를 simulation에서 반복 생성할 수 있다.
2. Approximate-geometry error와 contact dynamics를 학습 분포로 구성할 수 있다.
3. Rotation-to-Push 성공처럼 원하는 결과를 reward와 독립 evaluation metric으로 정의할 수 있다.
4. 실물에 배치할 observation만 actor에 제공하면서 privileged simulator information은 training에만 제한할 수 있다.

### 4.2 문제 요구와 RL의 대응

| 우리 조건 | RL과의 연결 | 함께 인정해야 할 조건 |
| --- | --- | --- |
| Approximate OBB 때문에 nominal contact와 실제 contact가 다를 수 있음 | Geometry perturbation과 dynamics randomization 아래 recovery behavior를 interaction으로 학습 가능 | Perturbation distribution이 실물 error를 대표해야 함 |
| Approach→Rotation→Push의 앞 행동이 뒤 성공에 영향을 줌 | Delayed episode return과 phase-gated shaping으로 전체 결과를 최적화 가능 | Shared return만으로 downstream awareness를 입증할 수는 없음 |
| Binary tactile·wrist F/T의 의미가 상태에 따라 달라짐 | Multimodal observation을 closed-loop policy에 직접 결합 가능 | 각 modality의 역할은 sensor ablation으로 분리해야 함 |
| Wrist와 fingers를 함께 움직여야 할 가능성이 있음 | 고차원 continuous action과 contact-dependent correction을 joint policy로 학습 가능 | Wrist-only·fixed-hand baseline보다 유의한 이득이 필요함 |
| Real tactile demonstration을 대규모로 모으기 어려움 | Simulation의 privileged state·force를 critic/reward에 사용하고 actor는 deployable observation으로 제한 가능 | Contact model과 sensor transfer gap을 randomization·real validation으로 검증해야 함 |
| Task semantics보다 low-level physical execution이 중심 | Large VLA backbone 없이 task-specific policy를 집중 학습 가능 | 상위 perception/planning과의 system boundary를 명확히 해야 함 |

따라서 선택 논리는 `다른 방법은 contact-rich nonprehensile manipulation에 부적합하다`가 아니다. **현재 연구는 정확한 model보다 반복 interaction을 얻기 쉽고, demonstration coverage는 제한되며, contact feedback에 따른 recovery와 장기 task outcome을 함께 최적화해야 하므로 RL을 primary learning framework로 채택한다.** Planning/control은 low-level controller와 safety constraint로, IL은 강한 비교 대상과 pretraining 후보로, VLA는 상위 goal interface로 남는다.

발표에서 사용할 결론은 다음과 같다.

> **RL은 모든 manipulation 방법보다 우월해서가 아니라, 본 연구처럼 task goal은 명확하지만 contact outcome은 불확실하고, simulation interaction은 풍부하며 real demonstration은 제한적인 low-level nonprehensile execution에 적합한 선택이다.**

이 결론도 가설적 방법 선택이다. RL이 IL 또는 model-based baseline보다 geometry perturbation과 contact disturbance에서 유의한 이득을 보이지 않거나, Sim-to-Real gap 때문에 실물 성능이 유지되지 않으면 RL 선택의 실험적 근거는 약해진다.

---

## 5. Presentation용 Research Trend 구성

한 장에 모든 논문을 넣지 않고 다음처럼 단계적으로 보여준다.

1. **Timeline overview:** 2021–2026 축에 논문 제목과 family 색상만 표시한다.
2. **Planning/Control:** B96, B37, B92를 통해 model·primitive·contact-mode 명시의 강점과 확장 비용을 설명한다.
3. **RL:** B97, B84, B10/B85, HACMan++, HAMNET/PIN-WM, B81/DAPL을 통해 tactile feedback·generalization·sequence의 확장을 설명한다.
4. **IL/VLA:** B98, Diffusion Policy, RDP, ForceVLA, DexMove를 통해 demonstration·semantic prior와 contact feedback까지 발전했음을 인정한다.
5. **Method trade-off:** 앞 절의 장단점 표를 제시한다.
6. **Why RL here:** 우리 조건과 RL의 대응표를 제시하고 Previous Works의 closest comparison으로 이동한다.

Timeline에서는 논문 수보다 흐름이 중요하다. 각 연도에서 같은 결론을 반복하는 논문은 제외하고, 방법 또는 연구 범위를 바꾼 대표 사례만 남긴다.

---

## 6. Research Trend와 Previous Works의 경계

| 구분 | Research Trend | Previous Works |
| --- | --- | --- |
| 목적 | 분야의 방법론적 변화와 RL 선택 근거 설명 | 우리와 가까운 시스템에서 미해결 교차점 식별 |
| 포함 범위 | Direct nonprehensile + adjacent contact-rich IL/VLA | Nonprehensile execution과 tactile/F/T가 직접 관련된 연구 |
| 비교 단위 | 연도, method family, 장단점 | Environment, agent, system의 고정 category |
| 결론 | 왜 본 연구에서 RL을 우선하는가 | 어떤 조합이 아직 실험적으로 검증되지 않았는가 |
| contribution과의 관계 | 방법 선택을 정당화하지만 novelty를 만들지는 않음 | Candidate contribution과 필요한 ablation을 직접 도출 |
