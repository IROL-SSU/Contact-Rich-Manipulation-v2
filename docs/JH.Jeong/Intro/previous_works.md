# Closest Previous Works — From Method Choice to Research Gaps

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 우리와 가까운 nonprehensile manipulation system을 동일한 Environment–Agent–System 기준으로 비교하고, 표에서 직접 도출되는 연구 질문과 필요한 실험을 기록한다.
>
> **주의:** 표의 빈칸이나 feature 조합만으로 novelty를 선언하지 않는다. `Candidate contribution`은 matched experiment가 지지할 때만 확정한다.

---

## 1. Research Trend에서 넘어온 질문

앞선 [Research Trend](./research_trend.md)은 현재 model·data·interaction 조건에서 RL을 primary framework로 우선한다는 결론을 냈다. 그러나 RL, tactile/F/T 또는 online correction은 이미 여러 연구에서 사용되므로 이 선택만으로 contribution이 생기지는 않는다. 이제 비교 단위를 method family에서 **실제 system condition**으로 좁혀야 한다.

| 앞 단계의 결론 | 이 문서에서 확인할 내용 | 이 문서의 출력 |
| --- | --- | --- |
| RL은 현재 조건에 적합한 `[Baseline]` 선택임 | 가까운 연구가 어떤 environment, geometry와 contact condition에서 평가되었는가? | 직접 비교 가능한 baseline 범위 |
| Contact feedback은 RL·IL·VLA·control에 이미 존재함 | 어떤 raw modality를 어떤 tool/action과 online update에 연결했는가? | 기존 연구가 이미 해결한 기능과 claim boundary |
| Initial contact formation과 contact 이후 adaptation은 다른 문제임 | 두 기능이 각각 어디까지 검증되었으며 approximate geometry 아래 무엇이 남는가? | C1·C2 후보와 transition-evaluation requirement |

따라서 이 문서는 `어떤 family가 더 좋은가`가 아니라 다음 질문에 답한다.

> **우리와 가까운 system이 이미 해결한 조건을 인정한 뒤에도, approximate geometry와 coarse contact feedback 아래에서 무엇을 추가로 검증해야 하는가?**

IL·VLA 논문이라도 semantic generalization만 다루고 nonprehensile/contact execution과 직접 연결되지 않으면 상세 표에는 넣지 않는다. 다만 ForceVLA와 Tactile-VLA처럼 **force/tactile feedback이 deployment action과 online physical correction에 직접 관여하는 연구**는 same-task baseline이 아니더라도 contact-aware learning이 이미 해결한 범위를 확인하기 위해 포함한다.

---

## 2. Comparison codebook

### 2.1 Environment

| Column | 판정 질문 | 허용값 | 경계 |
| --- | --- | --- | --- |
| Evaluation environment | 실제 실험에서 어떤 공간·접촉 구조를 평가하는가? | `Open tabletop` / `Cluttered tabletop` / `Fixture-based` / `Multiple environments` / `Open + fixture` / `Shelf` | 논문이 주장하는 일반성이 아니라 실제 평가 장면으로 판정 |
| Non-target/environment contact | Tool–target 이외의 물체·환경 접촉을 어떻게 취급하는가? | `Target only` / `Avoid` / `Exploit` / `Required` / `Task-dependent` / `Open` | Environment의 형태와 접촉의 역할은 독립적이다. 같은 fixture 환경에서도 접촉을 회피하거나 이용할 수 있으므로 두 column을 합치지 않는다. |
| Geometry information | Deployment system에 어떤 geometry 표현과 정확도 수준이 주어지는가? | `None` / `Implicit visual (not explicit)` / `Point cloud (observed)` / `Point cloud/BPS (observed)` / `OBB (approximate)` / `Known model (exact)` | 한 셀을 `representation (fidelity)`로 기록한다. Point cloud라고 자동으로 exact가 아니며, known CAD/dimensions가 pose와 정합된 경우만 `exact`다. Marker pose는 geometry 자체가 아니다. |

### 2.2 Agent

| Column | 판정 질문 | 기록 규칙 | 경계 |
| --- | --- | --- | --- |
| Tool | 실제로 target 또는 environment와 접촉하는 robot-side mechanism은 무엇인가? | `Rigid EEF`, `Tactile pusher`, `Parallel/adaptive gripper`, `Grasped object/tool`, `Dexterous hand`, `Mixed` | Robot 전체나 controller가 아니라 물체에 접촉을 전달하는 장치로 판정 |
| Input modality | Deployment 시 이용하는 물리적 sensor stream은 무엇인가? | `Vision (RGB/depth/RGB-D) → tactile → wrist/TCP F/T → proprioception` 순으로 기록하고, source를 확인할 수 없으면 `NR` | Point cloud·BPS·OBB·estimated pose는 `Geometry information`, task direction·goal·language instruction은 conditioning 정보이므로 이 column에서 제외한다. Tactile encoding과 history 같은 가공 방식도 main table에서는 제외한다. |
| Action output | Learned module 또는 controller가 무엇을 명령하는가? | EEF/TCP motion, gripper width, finger joints, contact force 등 실제 command를 기록 | 초기 pose 생성과 접촉 중 반복 command를 구분 |
| Tool-configuration update | Task에 맞춘 tool/contact configuration을 언제 변경하는가? | `N/A` / `Fixed` / `Task-conditioned initial` / `Online—gripper` / `Online—wrist/fingers` | Configuration은 접촉을 형성하는 wrist pose와 articulated tool DOF를 뜻한다. 단순 trajectory tracking과 접촉 configuration의 적응을 구분한다. |

### 2.3 System

| Column | 판정 질문 | 허용값 | 경계 |
| --- | --- | --- | --- |
| Method | 행동을 생성하는 주된 방법론은 무엇인가? | `Heuristic/Control` / `Optimization` / `Generative/Planning` / `RL` / `IL` / `VLA` / `Hybrid` | Encoder 종류나 sensor 유무가 아니라 action generation과 학습·계획 방식으로 판정한다. 복수 방식이 실제 행동 생성에 함께 관여할 때만 `Hybrid`로 기록한다. |

`Evaluated task`, `contact-stage coverage`, `closed-loop correction`, `reported endpoint`는 중요한 근거이지만 Environment–Agent–System의 단일 범주로 깔끔하게 귀속되지 않는다. 따라서 main classification table에 억지로 넣지 않고 Section 4의 paper-level evidence에서 논문별로 검증한다. `Robustness`, `Context awareness`, `Generalizable`, `Controllability`처럼 논문마다 의미가 달라지는 포괄적 표현도 column으로 사용하지 않는다.

---

## 3. Canonical comparison matrix

Environment, Agent와 System은 서로 다른 질문에 답하므로 같은 row 순서를 유지한 세 표로 분할한다. `Comparison role`은 표에 넣지 않고, direct baseline인지 adjacent comparator인지는 본문과 paper-level evidence에서 설명한다.

### 3.1 Environment

| ID | Work | Evaluation environment | Non-target/environment contact | Geometry information |
| --- | --- | --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | Goal-Driven Robotic Pushing | Open tabletop | Target only | None |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal-Oriented Pushing in Clutter | Cluttered tabletop | Avoid | Implicit visual (not explicit) |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Visuotactile Estimation under Occlusions | Open tabletop | Target only | Implicit visual (not explicit) |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | Tactile-Driven Contact Mode Control | Fixture-based | Exploit | Known model (exact) |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | HAMNET | Multiple environments | Exploit | Point cloud (observed) |
| [B01](https://doi.org/10.48550/arXiv.2509.18455) | GD2P | Open tabletop | Target only | Point cloud/BPS (observed)⁶ |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | DexMove | Open tabletop | Target only | Point cloud (observed)³ |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization-Guided Non-Prehensile RL | Open + fixture | Task-dependent | Implicit visual (not explicit) |
| [B46](https://doi.org/10.52202/085713-3124) | ForceVLA | Multiple fixture-based tasks | Required | Implicit visual (not explicit)⁴ |
| [B48](https://doi.org/10.48550/arXiv.2507.09160) | Tactile-VLA | Multiple fixture-based tasks | Required | Implicit visual (not explicit)⁵ |
| — | **Ours** | Shelf | Avoid / Open¹ | OBB (approximate) |

### 3.2 Agent

| ID | Work | Tool | Input modality | Action output | Tool-configuration update |
| --- | --- | --- | --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | Goal-Driven Robotic Pushing | Tactile pusher | Tactile + proprioception | EEF motion | N/A |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal-Oriented Pushing in Clutter | Rigid EEF | Vision + contact sensing + proprioception | EEF motion | N/A |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Visuotactile Estimation under Occlusions | Rigid EEF | Vision + wrist F/T + proprioception² | EEF motion | N/A |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | Tactile-Driven Contact Mode Control | Grasped object/tool | Tactile + optional wrist F/T + proprioception | EEF/object motion | Fixed |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | HAMNET | Parallel gripper | Vision + proprioception | EEF motion | Fixed |
| [B01](https://doi.org/10.48550/arXiv.2509.18455) | GD2P | Dexterous hand | NR (preprocessed geometry only)⁶ | Initial wrist pose + finger joints; then EEF translation | Task-conditioned initial⁶ |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | DexMove | Dexterous hand | Vision + tactile + proprioception | EEF motion + finger joints | Online—wrist/fingers³ |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization-Guided Non-Prehensile RL | Rigid EEF | Vision + contact sensing + wrist F/T + proprioception | EEF motion | N/A |
| [B46](https://doi.org/10.52202/085713-3124) | ForceVLA | Adaptive gripper / grasped tool | Vision (RGB) + wrist/TCP F/T + proprioception | TCP pose + gripper-width action chunk | Online—gripper⁴ |
| [B48](https://doi.org/10.48550/arXiv.2507.09160) | Tactile-VLA | Parallel gripper | Vision (RGB) + tactile + proprioception | Target pose + target force + gripper width | Online—gripper⁵ |
| — | **Ours** | Dexterous hand | Vision + tactile + wrist F/T + proprioception | EEF delta pose + finger joints | Online—wrist/fingers |

### 3.3 System

| ID | Work | Method |
| --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | Goal-Driven Robotic Pushing | Heuristic/Control |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal-Oriented Pushing in Clutter | RL |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Visuotactile Estimation under Occlusions | RL |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | Tactile-Driven Contact Mode Control | Hybrid (Optimization + Control) |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | HAMNET | RL |
| [B01](https://doi.org/10.48550/arXiv.2509.18455) | GD2P | Generative/Planning |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | DexMove | IL |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization-Guided Non-Prehensile RL | Hybrid (Optimization + RL) |
| [B46](https://doi.org/10.52202/085713-3124) | ForceVLA | VLA |
| [B48](https://doi.org/10.48550/arXiv.2507.09160) | Tactile-VLA | Hybrid (VLA + Control) |
| — | **Ours** | RL |

1. `[Open]` Shelf와 비표적 물체의 contact는 피한다. 주변 물체를 항상 둘지, 위치·수를 randomize할지, single-object와 multi-object를 분리할지는 아직 결정되지 않았다.
2. `[Evidence]` B12가 `tactile`이라고 부르는 입력은 별도 tactile array가 아니라 end-effector force measurement다. 따라서 `Input modality`에는 별도 tactile을 쓰지 않고 `wrist F/T`로 기록한다. History 길이는 main table이 아니라 evidence에서 관리한다.
3. `[Evidence]` DexMove는 initial hand contact pose를 object point cloud와 target object pose에 조건화한다. 따라서 `Geometry information = Point cloud (observed)`로 분류한다.
4. `[Evidence]` ForceVLA는 별도 tactile array 없이 real-time 6-axis end-effector wrench, RGB vision과 proprioception으로 TCP pose와 gripper-width action chunk를 생성한다. Insertion·pumping·wiping·peeling을 포함하므로 contact-aware VLA의 강한 정식 게재 비교군이지만, shelf NPM의 same-task baseline은 아니다.
5. `[Evidence]` Tactile-VLA는 dual high-resolution tactile의 normal/shear history로 target position과 contact force를 예측하고 hybrid position–force controller 및 선택적 CoT replanning을 사용한다. 현재 확인 가능한 출판 상태는 arXiv 2025이므로 preprint adjacent comparator로 구분한다.
6. `[Evidence]` GD2P는 object point cloud를 BPS로 표현하고 push/pull direction에 조건화된 wrist pose와 finger configuration을 생성한다. Point cloud/BPS와 direction은 각각 processed geometry와 task conditioning이므로 raw-sensor 기준의 `Input modality`에서는 제외한다. 접촉 이후에는 선택한 pose를 유지한 채 정해진 방향으로 이동하므로 `Task-conditioned initial`이며 online contact-feedback policy는 아니다.

---

## 4. Paper-level evidence and interpretation

| Work | 확인된 contribution | 우리와 직접 다른 조건 | 비교에서의 역할 |
| --- | --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | High-resolution tactile로 pusher–object relative pose를 안정화하고 proprioception으로 goal을 추종 | Rich tactile, push-only, finger action 없음 | Tactile servoing의 강한 control baseline; binary tactile의 정보 손실 경계 |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Clutter에서 goal progress, contact maintenance와 collision avoidance를 RL로 학습 | Push-only, contact flag, wrist/finger joint adaptation 없음 | Clutter-aware RL과 contact-maintenance reward baseline |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Occlusion 아래 vision과 force history로 object pose와 uncertainty를 추정하고 policy에 제공 | Fixed object class/shape 설정, EEF-only pushing, explicit OBB perturbation 아님 | F/T가 perception uncertainty를 보완하는 직접 근거 |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | High-resolution tactile로 extrinsic contact state를 추정하고 sticking/sliding mode를 최적화 | Known geometry와 명시적 contact-mode model, grasped object/tool | Contact-state estimation과 optimization/control upper baseline |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | Object·environment geometry module을 조합해 여러 NPM behavior와 unseen environment를 다룸 | Dense point-cloud representation, tactile/F/T 없음, finger action 없음 | Geometry-rich general-environment RL baseline |
| [B01](https://doi.org/10.48550/arXiv.2509.18455) | Object geometry와 push/pull direction에 조건화된 dexterous pre-contact pose를 대규모로 생성·선택하고 실제 manipulation outcome으로 평가 | Dense point-cloud/BPS geometry, initial configuration 이후 tactile/F/T 기반 online correction 없음, single push/pull | Approach의 task-conditioned hand configuration을 직접 비교하는 핵심 baseline; initial selection과 online adaptation의 효과 분리 |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | Object point cloud와 target pose로 initial contact를 정하고, synthetic trajectory와 human tactile demonstration으로 dexterous wrist–finger NPM을 학습 | Dense geometry, high-dimensional tactile와 demonstration | 가장 가까운 task-conditioned dexterous tactile baseline |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization demonstration과 RL을 결합하고 vision·wrist force feedback으로 pushing/pivoting 수행 | Reference trajectory prior, EEF-only action, tactile array 없음 | 가장 가까운 force-guided execution baseline |
| [B46](https://doi.org/10.52202/085713-3124) | 6축 end-effector wrench를 force-aware MoE로 vision·language와 융합해 five-task contact-rich manipulation과 perturbation generalization을 개선 | Large π0 backbone, task-specific demonstrations, parallel/adaptive gripper, fixed shelf NPM 아님 | F/T를 first-class VLA modality로 사용한 정식 게재 상한 비교군; `VLA는 force를 사용하지 못한다`는 주장에 대한 반례 |
| [B48](https://doi.org/10.48550/arXiv.2507.09160) | High-resolution tactile history, target force action, hybrid position–force control과 tactile-grounded reasoning으로 cross-task·OOD contact adaptation을 평가 | Preprint, rich tactile, language-conditioned demonstrations, gripper task, fixed shelf NPM 아님 | Tactile-aware VLA의 physical grounding·failure recovery 비교군; coarse binary tactile RL과의 정보·data budget 차이를 드러냄 |

위 차이는 `기존 연구가 부족하다`는 판정이 아니다. 서로 다른 problem setting과 sensing/data budget의 차이다. 정량 우열은 동일한 object, geometry error, sensing과 action budget을 맞춘 재구현·ablation에서만 주장한다.

---

## 5. What the comparison supports

### 5.1 Supported statements

- Tactile 또는 wrist F/T 기반 nonprehensile feedback은 이미 충분한 선행 근거가 있다.
- RL, optimization/control과 IL 모두 contact uncertainty를 다루며, method family만으로 novelty를 정의할 수 없다.
- GD2P는 task direction에 조건화된 dexterous initial hand configuration이 push/pull outcome을 개선할 수 있음을 직접 비교하므로, contact formation 가설에서 제외할 수 없는 baseline이다.
- ForceVLA와 Tactile-VLA는 VLA도 force/tactile feedback을 action generation, force control 또는 failure recovery에 직접 사용할 수 있음을 보여준다.
- Dense geometry 기반 general nonprehensile RL과 point-cloud-conditioned contact formation·high-dimensional tactile를 사용하는 dexterous IL은 이미 강한 비교 대상이다.
- 현재 표에서는 `approximate explicit geometry + coarse tactile/F/T + EEF/finger action + Approach–Rotation–Push`의 효과를 동일한 조건에서 분해한 연구가 확인되지 않았다.

### 5.2 Unsupported statements

- `Tactile/F/T를 처음으로 사용한다.`
- `IL·VLA는 contact feedback 또는 online correction을 수행할 수 없다.`
- `OBB가 dense geometry보다 우월하다.`
- `여러 phase를 한 policy로 연결했기 때문에 novel하다.`
- `우리 row의 조합이 유일하므로 contribution이다.`

마지막 두 문장을 피하는 것이 중요하다. Combination novelty가 아니라 **geometry error에 대한 robustness와 downstream manipulation outcome**이 contribution의 중심이어야 한다.

---

## 6. Comparison에서 남은 두 candidate와 하나의 evaluation requirement

Supported·unsupported statement를 구분하면 두 개의 candidate claim과 이를 해석하기 위한 하나의 evaluation requirement가 남는다.

| Previous-work evidence | 남은 질문 | 다음 단계의 분류 |
| --- | --- | --- |
| Contact feedback 자체와 tactile/F/T 기반 correction은 이미 존재함 | Coarse contact feedback이 approximate-geometry error에 따른 성능 저하를 실제로 완화하는가? | C1 — Geometry-error compensation |
| GD2P는 task-conditioned initial pose를, DexMove는 tactile wrist–finger policy를 제시함 | Initial contact selection 이후의 online wrist–finger adaptation이 coarse geometry 조건에서 추가 이득을 주는가? | C2 — Contact formation and adaptation |
| 기존 연구는 각 task의 final success를 평가하지만 현재 비교만으로 Rotation alignment와 subsequent Push feasibility의 관계는 확정되지 않음 | C1·C2의 효과가 Rotation 정렬에만 머무는지 subsequent Push까지 이어지는지 어떻게 판정할 것인가? | Evaluation — Factorized Rotation-to-Push analysis |

---

## 7. Immediate validation tasks

표를 발표용 최종본으로 바꾸기 전에 다음을 수행한다.

1. B37, B84, B12, B92, B90, B22와 B81의 full text에서 각 categorical value의 근거 문장·figure·section을 기록한다.
2. `Geometry information`의 representation과 fidelity 표기가 deployment input과 일치하는지 다시 검증한다.
3. 각 `Input modality`가 실제 physical sensor stream인지, processed geometry·goal·privileged information인지 구분한다.
4. Wrist F/T가 actor/control input인지, reward·estimation·evaluation에서만 사용되는지 구분한다.
5. Surrounding-object 조건을 `Target only / Avoid / Exploit / Required / Task-dependent / Open` 중 하나로 일관되게 판정한다.
6. 발표 표에는 검증이 끝난 row만 남기고 불확실한 cell은 `NR`로 표시한다.

GD2P, ForceVLA와 Tactile-VLA의 geometry/input, action, controller와 task는 2026-09-18 원문에서 확인했다. Tactile-VLA는 제출·발표 시점에 정식 게재 여부를 다시 확인하며, 확인 전까지 `preprint adjacent comparator`로 유지한다.

현재 가장 먼저 원문을 대조할 순서는 [B81](https://doi.org/10.1109/LRA.2026.3655262) → [B22](https://openreview.net/forum?id=dT3ZciXvNX) → [B12](https://doi.org/10.48550/arXiv.2412.13157) → [B92](https://doi.org/10.15607/RSS.2024.XX.135) → [B90](https://doi.org/10.15607/RSS.2025.XXI.154)이다.

---

## 8. Research gap에서 candidate contribution으로

Previous Works의 결론은 `우리의 조합이 새롭다`가 아니다. 문헌으로 확인된 범위 안에서 **두 개의 candidate question과 하나의 evaluation requirement**를 도출한 것이다. 다음 문서는 C1·C2를 비교 조건, metric과 지지·기각 결과가 있는 claim으로 바꾸고, factorized Rotation-to-Push analysis는 두 claim의 공통 평가로 둔다.

> **C1·C2를 어떤 matched experiment로 검증해야 하며, Rotation-to-Push 분석을 포함한 어떤 결과가 나와야 실제 contribution으로 확정할 수 있는가?**

이 질문은 [Candidate Contributions](./contributions.md)에서 다룬다.
