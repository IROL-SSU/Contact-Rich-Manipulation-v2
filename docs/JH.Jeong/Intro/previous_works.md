# Closest Previous Works — Nonprehensile Manipulation with Contact Feedback

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 우리와 가까운 nonprehensile manipulation system을 동일한 Environment–Agent–System 기준으로 비교하고, 표에서 직접 도출되는 연구 질문과 필요한 실험을 기록한다.
>
> **주의:** 표의 빈칸이나 feature 조합만으로 novelty를 선언하지 않는다. `Candidate contribution`은 matched experiment가 지지할 때만 확정한다.

---

## 1. Research Trend와의 경계

| 구분 | Research Trend | 이 문서의 Previous Works |
| --- | --- | --- |
| 질문 | 왜 여러 method family 가운데 RL을 우선하는가? | 어떤 가까운 system이 무엇을 이미 해결했고 무엇을 비교해야 하는가? |
| 범위 | 2021년 이후 direct NPM + adjacent IL/VLA | NPM execution, tactile/F/T, geometry uncertainty 또는 wrist–finger control과 직접 관련된 연구 |
| 비교 단위 | 연도, method family, 장단점 | Environment, agent, system의 고정 column |
| 결론 | Method 선택의 조건부 근거 | Candidate contribution과 falsification experiment |

IL·VLA 논문이라도 semantic generalization만 다루고 nonprehensile/contact execution과 직접 연결되지 않으면 이 표에는 넣지 않는다. 그런 연구는 [`research_trend.md`](./research_trend.md)에서만 다룬다.

---

## 2. Comparison codebook

### 2.1 Environment

| Column | 판정 질문 | 허용값 | 경계 |
| --- | --- | --- | --- |
| Workspace | 어떤 공간에서 실행하는가? | `Open tabletop` / `Cluttered tabletop` / `Fixture-constrained` / `Mixed` / `General environment` / `Shelf-constrained` | `Mixed`는 동일 논문이 open condition과 fixture contact condition을 모두 평가할 때만 사용 |
| Non-target contact | 비표적 물체·환경 접촉의 역할은 무엇인가? | `Absent` / `Avoided` / `Used` / `Open` | `Used`는 extrinsic contact 또는 주변 물체 상호작용이 method의 일부일 때만 사용 |
| Geometry input | Deployment policy/control이 어떤 geometry를 받는가? | `None` / `Implicit visual` / `Dense explicit` / `Approximate explicit` / `Exact` | Marker가 제공하는 pose는 geometry가 아니며, exact CAD/dimension과 구분 |

### 2.2 Agent

| Column | 판정 질문 | 허용값 | 경계 |
| --- | --- | --- | --- |
| End effector | 무엇으로 물체와 접촉하는가? | `Rigid EEF` / `Tactile pusher` / `Parallel gripper` / `Grasped object/tool` / `Dexterous hand` | Robot 전체가 아니라 실제 contact interface로 판정 |
| Tactile | 별도 tactile array의 정보량은 어느 수준인가? | `None` / `Contact flag` / `Binary array` / `High-dimensional` | Wrist force를 tactile로 부른 논문은 별도 array가 없으면 `None` |
| Wrist F/T | Wrist wrench를 policy/control에 사용하는가? | `No` / `Yes` / `Optional` | Reward-only privileged force는 deployment feedback으로 세지 않음 |
| Action authority | Online action이 무엇을 움직이는가? | `EEF` / `EEF + fingers` | Initial hand pose만 선택하고 finger가 고정되면 `EEF` |
| Configuration conditioning | Hand/contact configuration을 task goal에 맞추며 언제 조정하는가? | `N/A` / `Fixed` / `Goal-conditioned initial` / `Goal-conditioned online` | Dexterous contact pose를 별도로 다루지 않으면 `N/A` |

### 2.3 System

| Column | 판정 질문 | 허용값 | 경계 |
| --- | --- | --- | --- |
| Task scope | 어떤 nonprehensile behavior를 수행·연결하는가? | `Push` / `Pivot–slide` / `Push / pivot` / `Repositioning` / `Multi-mode` / `Approach–Rotation–Push` | 논문의 평가 task 기준 |
| Method | Deployment action을 생성하는 주된 mechanism은 무엇인가? | `Control` / `Optimization` / `RL` / `IL` / `Hybrid` | Encoder나 estimator의 학습 여부가 아니라 action generation과 training objective로 판정 |
| Online correction | 실행 중 무엇을 주로 수정하는가? | `Contact relation` / `Object motion` / `State estimate` / `Contact mode` / `Object + contact motion` | 여러 요소가 있으면 논문의 핵심 feedback target을 기록 |

`Robustness`, `Context awareness`, `Generalizable`은 단일 column으로 쓰지 않는다. 어떤 perturbation, held-out object 또는 environment에서 무엇이 개선되었는지를 별도 evidence로 기록한다.

---

## 3. Canonical comparison matrix

표가 지나치게 넓어지는 것을 막기 위해 같은 row 순서를 유지한 두 표로 분할한다. 발표에서는 두 장으로 나누거나 grouped header를 가진 하나의 표로 재구성할 수 있다.

### 3.1 Environment

| ID | Work | Workspace | Non-target contact | Geometry input |
| --- | --- | --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | Goal-Driven Robotic Pushing | Open tabletop | Absent | None |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal-Oriented Pushing in Clutter | Cluttered tabletop | Avoided | Implicit visual |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Visuotactile Estimation under Occlusions | Open tabletop | Absent | Implicit visual |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | Tactile-Driven Contact Mode Control | Fixture-constrained | Used | Exact |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | HAMNET | General environment | Used | Dense explicit |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | DexMove | Open tabletop | Absent | Dense explicit³ |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization-Guided Non-Prehensile RL | Mixed | Used | Implicit visual |
| — | **Ours** | Shelf-constrained | Avoided / Open¹ | Approximate explicit OBB |

### 3.2 Agent and system

| ID | Work | End effector | Tactile | Wrist F/T | Action authority | Configuration conditioning | Task scope | Method | Online correction |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | Goal-Driven Robotic Pushing | Tactile pusher | High-dimensional | No | EEF | N/A | Push | Control | Contact relation |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal-Oriented Pushing in Clutter | Rigid EEF | Contact flag | No | EEF | N/A | Push | RL | Object motion |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Visuotactile Estimation under Occlusions | Rigid EEF | None² | Yes | EEF | N/A | Push | RL + estimator | State estimate |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | Tactile-Driven Contact Mode Control | Grasped object/tool | High-dimensional | Optional | EEF | Fixed | Pivot–slide | Optimization | Contact mode |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | HAMNET | Parallel gripper | None | No | EEF | N/A | Multi-mode | RL | Object + contact motion |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | DexMove | Dexterous hand | High-dimensional | No | EEF + fingers | Goal-conditioned online | Repositioning | IL | Object + contact motion |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization-Guided Non-Prehensile RL | Rigid EEF | Contact flag | Yes | EEF | N/A | Push / pivot | Hybrid | Object motion |
| — | **Ours** | Dexterous hand | Binary array | Yes | EEF + fingers | Goal-conditioned online | Approach–Rotation–Push | RL | Object + contact motion |

1. `[Open]` Shelf와 비표적 물체의 contact는 피한다. 주변 물체를 항상 둘지, 위치·수를 randomize할지, single-object와 multi-object를 분리할지는 아직 결정되지 않았다.
2. `[Evidence]` B12가 `tactile`이라고 부르는 입력은 별도 tactile array가 아니라 end-effector force measurement다. 따라서 여기서는 `Tactile = None`, `Wrist F/T = Yes`로 분류한다.
3. `[Evidence]` DexMove는 initial hand contact pose를 object point cloud와 target object pose에 조건화한다. 실행 policy와 initial-contact module을 포함한 system-level geometry input은 `Dense explicit`로 분류한다.

---

## 4. Paper-level evidence and interpretation

| Work | 확인된 contribution | 우리와 직접 다른 조건 | 비교에서의 역할 |
| --- | --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | High-resolution tactile로 pusher–object relative pose를 안정화하고 proprioception으로 goal을 추종 | Rich tactile, push-only, finger action 없음 | Tactile servoing의 강한 control baseline; binary tactile의 정보 손실 경계 |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Clutter에서 goal progress, contact maintenance와 collision avoidance를 RL로 학습 | Push-only, contact flag, wrist/finger joint adaptation 없음 | Clutter-aware RL과 contact-maintenance reward baseline |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Occlusion 아래 vision과 force history로 object pose와 uncertainty를 추정하고 policy에 제공 | Fixed object class/shape 설정, EEF-only pushing, explicit OBB perturbation 아님 | F/T가 perception uncertainty를 보완하는 직접 근거 |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | High-resolution tactile로 extrinsic contact state를 추정하고 sticking/sliding mode를 최적화 | Known geometry와 명시적 contact-mode model, grasped object/tool | Contact-state estimation과 optimization/control upper baseline |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | Object·environment geometry module을 조합해 여러 NPM behavior와 unseen environment를 다룸 | Dense point-cloud representation, tactile/F/T 없음, finger action 없음 | Geometry-rich general-environment RL baseline |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | Object point cloud와 target pose로 initial contact를 정하고, synthetic trajectory와 human tactile demonstration으로 dexterous wrist–finger NPM을 학습 | Dense geometry, high-dimensional tactile와 demonstration | 가장 가까운 task-conditioned dexterous tactile baseline |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization demonstration과 RL을 결합하고 vision·wrist force feedback으로 pushing/pivoting 수행 | Reference trajectory prior, EEF-only action, tactile array 없음 | 가장 가까운 force-guided execution baseline |

위 차이는 `기존 연구가 부족하다`는 판정이 아니다. 서로 다른 problem setting과 sensing/data budget의 차이다. 정량 우열은 동일한 object, geometry error, sensing과 action budget을 맞춘 재구현·ablation에서만 주장한다.

---

## 5. What the comparison supports

### 5.1 Supported statements

- Tactile 또는 wrist F/T 기반 nonprehensile feedback은 이미 충분한 선행 근거가 있다.
- RL, optimization/control과 IL 모두 contact uncertainty를 다루며, method family만으로 novelty를 정의할 수 없다.
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

## 6. Contribution으로의 연결

이 비교에서 도출된 C1–C3와 각각의 지지·기각 조건은 [Candidate Contributions](./contributions.md)에서 관리한다. 이 문서는 논문별 비교 근거까지만 담당하며 contribution 문장을 중복 관리하지 않는다.

---

## 7. Immediate validation tasks

표를 발표용 최종본으로 바꾸기 전에 다음을 수행한다.

1. B37, B84, B12, B92, B90, B22와 B81의 full text에서 각 categorical value의 근거 문장·figure·section을 기록한다.
2. `Exact`, `Dense explicit`, `Implicit visual`, `Approximate explicit`의 판정을 deployment input 기준으로 다시 검증한다.
3. Wrist F/T가 actor/control input인지, reward·estimation·evaluation에서만 사용되는지 구분한다.
4. Surrounding-object 조건을 `Absent / Avoided / Used` 중 하나로 일관되게 판정한다.
5. 발표 표에는 검증이 끝난 row만 남기고 불확실한 cell은 `NR`로 표시한다.

현재 가장 먼저 원문을 대조할 순서는 [B81](https://doi.org/10.1109/LRA.2026.3655262) → [B22](https://openreview.net/forum?id=dT3ZciXvNX) → [B12](https://doi.org/10.48550/arXiv.2412.13157) → [B92](https://doi.org/10.15607/RSS.2024.XX.135) → [B90](https://doi.org/10.15607/RSS.2025.XXI.154)이다.
