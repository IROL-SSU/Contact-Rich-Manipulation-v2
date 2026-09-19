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

Codebook은 저자가 사용한 표현을 그대로 옮기는 용도가 아니라, 서로 다른 논문을 같은 질문으로 판정하기 위한 규칙이다. Main table에는 아래의 짧은 label을 쓰고, 이 절에서 각 label의 의미와 제외 조건을 고정한다. `NR`은 논문에서 확인되지 않았다는 뜻이고, `Undecided`는 **우리 system에서 아직 결정하지 않은 항목**에만 사용한다.

### 2.1 Environment

#### Evaluation setting — 어디에서 평가했는가?

논문이 주장하는 일반성이 아니라 실제 experiment와 deployment evaluation에 등장한 공간·접촉 구조로 판정한다.

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `Open tabletop` | 주변 구조물이 거의 없는 일반 테이블 | Target과 support plane 이외의 fixture·clutter가 평가의 핵심 조건이 아님 |
| `Cluttered tabletop` | 다른 물체가 함께 있는 테이블 | 주변 물체가 접근, 경로 또는 충돌 가능성에 영향을 줌 |
| `Fixture-constrained` | 고정 구조물이 접촉과 motion을 제한하는 환경 | Hole, peg, slot, board, wall과 같은 fixture가 task를 구성함 |
| `Multiple settings` | 구조적으로 다른 둘 이상의 환경 | 단순 object 교체가 아니라 tabletop·fixture 등 평가 setting 자체가 달라짐 |
| `Open tabletop + fixture` | Open condition과 fixture condition을 모두 평가 | 한 논문에서 두 조건을 명시적으로 분리해 평가함 |
| `Shelf` | Shelf deck·pillar·경계가 있는 선반 환경 | 선반 구조가 workspace와 safety constraint를 결정함 |

#### Surrounding-contact role — Target 이외의 접촉을 어떻게 다루는가?

정상적인 object–support-plane 접촉은 이 column에서 세지 않는다. Tool–target 접촉 이외의 robot/object–fixture 또는 주변 물체 접촉이 task에서 어떤 역할을 하는지를 기록한다.

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `Target contact only` | Target과의 접촉만 평가 | 별도 주변 물체·fixture 접촉이 task에 없거나 사용되지 않음 |
| `Avoid surrounding contact` | 주변 접촉을 실패·비용으로 취급 | Collision penalty, constraint 또는 termination으로 회피함 |
| `Use surrounding contact` | 주변 접촉을 조작에 활용 | Wall, fixture 또는 다른 물체의 반력을 의도적으로 이용함 |
| `Environmental contact required` | 환경 접촉 없이는 task가 성립하지 않음 | Insertion, wiping, constrained sliding처럼 fixture contact가 필수임 |
| `Depends on task` | 같은 system에서도 task별 역할이 다름 | 일부 task에서는 회피하고 다른 task에서는 이용함 |
| `Undecided` | 우리 system에서 아직 미결 | 주변 물체의 존재·허용 여부가 확정되지 않은 경우에만 사용 |

Evaluation setting과 surrounding-contact role은 독립적으로 판정한다. 예를 들어 fixture 환경에서도 fixture 접촉을 회피할 수 있고, open tabletop에서도 주변 물체를 이용할 수 있다.

#### Geometry available at deployment — 실행 시 어떤 형상 표현을 받는가?

Geometry는 `표현 방식 + 정확도 수준`으로 기록한다. 이 column은 raw sensor 종류가 아니라 deployment system이 action을 결정할 때 이용할 수 있는 **형상 표현**을 나타낸다.

| 허용값 | 직관적 의미 | 포함 조건과 제외 조건 |
| --- | --- | --- |
| `No geometry input` | 명시적·암묵적 형상 입력이 없음 | Pose, tactile 또는 proprioception만 사용하며 visual shape feature도 사용하지 않음 |
| `Image features (implicit geometry)` | 영상 feature 안에 형상이 암묵적으로 포함됨 | RGB 또는 depth/RGB-D image를 encoder에 직접 입력하지만 point cloud·OBB·mesh 같은 구조화된 형상은 만들지 않음 |
| `Observed point cloud` | 관측으로 얻은 3D 점 집합을 사용 | RGB-D·depth sensor 등으로 얻은 partial/noisy point cloud를 사용하며 exact CAD로 간주하지 않음 |
| `Observed point cloud → BPS` | 관측 point cloud를 BPS로 변환 | Point cloud를 Basis Point Set distance 등 고정 길이 표현으로 가공함 |
| `Approximate OBB` | 거친 box 형상과 pose를 사용 | 추정된 center, orientation과 extent를 사용하며 실제 local surface/contact geometry는 알지 못함 |
| `Exact known model` | 정확한 object/fixture model을 알고 사용 | Known CAD, mesh 또는 정확한 dimension이 pose와 정합되어 planning/control에 사용됨 |

`Image features (implicit geometry)`는 단순히 vision을 썼다는 뜻이 아니다. 영상을 통해 추정한 OBB를 policy에 주면 `Approximate OBB`, 영상으로 만든 point cloud를 주면 `Observed point cloud`로 기록한다. 반대로 RGB-D를 사용해도 depth image를 image encoder에 직접 넣고 구조화된 geometry를 만들지 않으면 `Image features (implicit geometry)`다. Marker나 tracker가 제공한 pose만으로는 object shape를 알 수 없으므로 pose 자체는 geometry로 세지 않는다.

### 2.2 Agent

#### Contact tool — 무엇으로 물체에 접촉하는가?

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `Rigid end effector` | 관절이 없는 단단한 tip·rod·plate | Robot 전체가 아니라 실제 contact interface가 rigid한 경우 |
| `Tactile pusher` | Tactile sensor가 부착된 pushing tool | Tool 자체가 contact sensing surface 역할을 함 |
| `Parallel gripper` | 두 jaw의 개폐를 주로 사용하는 gripper | Gripper width를 명령하지만 개별 finger joint를 독립 제어하지 않음 |
| `Adaptive gripper` | 접촉에 맞춰 finger가 수동·능동 적응하는 gripper | Dexterous-hand 수준의 독립 finger command와 구분 |
| `Grasped object/tool` | Robot이 쥔 물체나 도구로 접촉 | Gripper가 아니라 grasped item이 실제 target/environment에 힘을 전달함 |
| `Dexterous hand` | 여러 finger joint를 갖는 다지 hand | Finger configuration을 접촉 형성이나 online adaptation에 사용할 수 있음 |
| `Mixed tools` | 둘 이상의 contact tool을 비교·사용 | 동일 논문에서 tool 종류가 평가 조건에 따라 달라짐 |

#### Physical sensor input — 실행 중 어떤 물리 sensor stream을 받는가?

여러 입력은 `Vision → tactile/contact state → wrist/TCP F/T → proprioception` 순으로 기록한다. 가공된 geometry, task goal과 language instruction은 여기서 제외한다.

| 표기 | 의미 | 경계 |
| --- | --- | --- |
| `Vision` | Camera가 제공하는 raw 또는 minimally processed image stream | 확인되면 `Vision (RGB)`, `Vision (depth)`, `Vision (RGB-D)`처럼 세부 종류를 병기하고, point cloud·OBB·estimated pose는 geometry column에 기록 |
| `Tactile` | 별도 tactile array·image·taxel stream | Wrist force만 사용하면 tactile로 세지 않음 |
| `Contact flag/state` | Contact 여부나 discrete contact state | Tactile spatial pattern이나 6-axis wrench와 구분 |
| `Wrist/TCP F/T` | End-effector의 force/torque 또는 wrench | Reward-only privileged force는 deployment input으로 세지 않음 |
| `Proprioception` | Joint position·velocity, gripper width 등 robot 내부 상태 | Command나 task goal은 포함하지 않음 |
| `NR` | Raw sensor source를 논문에서 확인할 수 없음 | Preprocessed geometry만 명시된 경우 설명을 괄호에 덧붙임 |

#### Commanded action — system이 실제로 무엇을 명령하는가?

| 반복 표기 | 의미 |
| --- | --- |
| `EEF/TCP motion` | End-effector의 pose, delta pose, velocity 또는 trajectory command |
| `Object motion` | Grasped object/tool의 목표 motion을 직접 최적화·명령 |
| `Gripper width` | Gripper opening/closing command |
| `Finger joints` | 개별 또는 actuated finger joint command |
| `Contact force` | Target force 또는 force-control reference |
| `Initial wrist pose/configuration` | 접촉 전 initial wrist pose나 finger configuration만 생성 |

초기 pose/configuration 생성과 contact 중 반복 command를 구분해 기록한다. 논문 고유 action이 위 표에 맞지 않으면 의미를 보존하는 짧은 표현을 추가한다.

#### Configuration adaptation — contact configuration을 언제 바꾸는가?

Configuration은 접촉을 형성하는 wrist pose와 articulated tool DOF를 뜻한다. 단순 EEF trajectory tracking은 configuration adaptation으로 세지 않는다.

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `Not applicable` | 별도 tool/contact configuration이 없음 | Rigid EEF motion만 제어하는 경우 |
| `Fixed after setup` | 시작 후 configuration을 고정 | Grasp나 finger posture를 정한 뒤 online으로 바꾸지 않음 |
| `Task-conditioned initial only` | Goal에 맞춰 initial configuration만 선택 | Contact 전 wrist/finger pose를 생성하지만 이후에는 고정 |
| `Online gripper adjustment` | 실행 중 gripper opening/closure를 수정 | Feedback에 따라 gripper width나 closure command를 갱신 |
| `Online wrist–finger adjustment` | 실행 중 wrist와 finger configuration을 함께 수정 | Contact feedback이나 task state에 따라 wrist pose와 finger joints를 반복 갱신 |

### 2.3 System

#### Action-generation method — 행동을 만드는 주된 방법은 무엇인가?

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `Rule/feedback control` | 사람이 정의한 rule, primitive 또는 feedback law | Learned policy나 online optimizer가 주된 action generator가 아님 |
| `Optimization/planning` | Model·constraint·search로 action을 계산 | Trajectory optimization, MPC, contact-mode planning 등을 포함 |
| `Generative pose/trajectory planning` | 생성 model이 pose·trajectory 후보를 만들고 planner가 선택·실행 | Demonstration action을 직접 모방하는 IL과 구분 |
| `Reinforcement learning (RL)` | Environment return으로 policy/value를 학습 | Online deployment action을 learned RL policy가 생성 |
| `Imitation learning (IL)` | Demonstration action을 주 supervision으로 학습 | Behavior cloning, diffusion/flow policy 등을 포함 |
| `Vision-language-action (VLA)` | Vision-language representation과 action generation을 결합 | Semantic instruction만 처리하고 별도 controller가 전부 실행하면 역할을 구분해 기록 |
| `Hybrid` | 둘 이상의 방식이 action generation에 필수적으로 관여 | `Optimization + RL`, `VLA + control`처럼 실제 결합 요소를 괄호에 명시 |

Encoder 종류나 sensor 유무는 method family를 결정하지 않는다. 복수 component가 있어도 하나가 perception이나 initialization에만 쓰이면 자동으로 `Hybrid`로 분류하지 않는다.

`Evaluated task`, `contact-stage coverage`, `closed-loop correction`, `reported endpoint`는 중요한 근거이지만 Environment–Agent–System의 단일 범주로 깔끔하게 귀속되지 않는다. 따라서 main classification table에 억지로 넣지 않고 Section 4의 paper-level evidence에서 논문별로 검증한다. `Robustness`, `Context awareness`, `Generalizable`, `Controllability`처럼 논문마다 의미가 달라지는 포괄적 표현도 column으로 사용하지 않는다.

---

## 3. Canonical comparison matrix

Environment, Agent와 System은 서로 다른 질문에 답하므로 같은 row 순서를 유지한 세 표로 분할한다. `Comparison role`은 표에 넣지 않고, direct baseline인지 adjacent comparator인지는 본문과 paper-level evidence에서 설명한다.

### 3.1 Environment

| ID | Work | Evaluation setting | Surrounding-contact role | Geometry available at deployment |
| --- | --- | --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | Goal-Driven Robotic Pushing | Open tabletop | Target contact only | No geometry input |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal-Oriented Pushing in Clutter | Cluttered tabletop | Avoid surrounding contact | Image features (implicit geometry) |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Visuotactile Estimation under Occlusions | Open tabletop | Target contact only | Image features (implicit geometry) |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | Tactile-Driven Contact Mode Control | Fixture-constrained | Use surrounding contact | Exact known model |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | HAMNET | Multiple settings | Use surrounding contact | Observed point cloud |
| [B01](https://doi.org/10.48550/arXiv.2509.18455) | GD2P | Open tabletop | Target contact only | Observed point cloud → BPS⁶ |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | DexMove | Open tabletop | Target contact only | Observed point cloud³ |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization-Guided Non-Prehensile RL | Open tabletop + fixture | Depends on task | Image features (implicit geometry) |
| [B46](https://doi.org/10.52202/085713-3124) | ForceVLA | Multiple settings | Environmental contact required | Image features (implicit geometry)⁴ |
| [B48](https://doi.org/10.48550/arXiv.2507.09160) | Tactile-VLA | Multiple settings | Environmental contact required | Image features (implicit geometry)⁵ |
| — | **Ours** | Shelf | Avoid surrounding contact¹ | Approximate OBB |

### 3.2 Agent

| ID | Work | Contact tool | Physical sensor input | Commanded action | Configuration adaptation |
| --- | --- | --- | --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | Goal-Driven Robotic Pushing | Tactile pusher | Tactile + proprioception | EEF motion | Not applicable |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal-Oriented Pushing in Clutter | Rigid end effector | Vision + contact flag + proprioception | EEF motion | Not applicable |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Visuotactile Estimation under Occlusions | Rigid end effector | Vision + wrist F/T + proprioception² | EEF motion | Not applicable |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | Tactile-Driven Contact Mode Control | Grasped object/tool | Tactile + optional wrist F/T + proprioception | EEF/object motion | Fixed after setup |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | HAMNET | Parallel gripper | Vision + proprioception | EEF motion | Fixed after setup |
| [B01](https://doi.org/10.48550/arXiv.2509.18455) | GD2P | Dexterous hand | NR (preprocessed geometry only)⁶ | Initial wrist pose + finger joints; then EEF translation | Task-conditioned initial only⁶ |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | DexMove | Dexterous hand | Vision + tactile + proprioception | EEF motion + finger joints | Online wrist–finger adjustment³ |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization-Guided Non-Prehensile RL | Rigid end effector | Vision + contact state + wrist F/T + proprioception | EEF motion | Not applicable |
| [B46](https://doi.org/10.52202/085713-3124) | ForceVLA | Mixed tools (adaptive gripper / grasped tool) | Vision (RGB) + wrist/TCP F/T + proprioception | TCP pose + gripper-width action chunk | Online gripper adjustment⁴ |
| [B48](https://doi.org/10.48550/arXiv.2507.09160) | Tactile-VLA | Parallel gripper | Vision (RGB) + tactile + proprioception | Target pose + target force + gripper width | Online gripper adjustment⁵ |
| — | **Ours** | Dexterous hand | Vision + tactile + wrist F/T + proprioception | EEF delta pose + finger joints | Online wrist–finger adjustment |

### 3.3 System

| ID | Work | Action-generation method |
| --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | Goal-Driven Robotic Pushing | Rule/feedback control |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal-Oriented Pushing in Clutter | Reinforcement learning (RL) |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Visuotactile Estimation under Occlusions | Reinforcement learning (RL) |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | Tactile-Driven Contact Mode Control | Hybrid (Optimization + Control) |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | HAMNET | Reinforcement learning (RL) |
| [B01](https://doi.org/10.48550/arXiv.2509.18455) | GD2P | Generative pose/trajectory planning |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | DexMove | Imitation learning (IL) |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization-Guided Non-Prehensile RL | Hybrid (Optimization + RL) |
| [B46](https://doi.org/10.52202/085713-3124) | ForceVLA | Vision-language-action (VLA) |
| [B48](https://doi.org/10.48550/arXiv.2507.09160) | Tactile-VLA | Hybrid (VLA + Control) |
| — | **Ours** | Reinforcement learning (RL) |

1. `[Open]` Shelf와 비표적 물체의 contact는 피한다. 주변 물체를 항상 둘지, 위치·수를 randomize할지, single-object와 multi-object를 분리할지는 아직 결정되지 않았다.
2. `[Evidence]` B12가 `tactile`이라고 부르는 입력은 별도 tactile array가 아니라 end-effector force measurement다. 따라서 `Physical sensor input`에는 별도 tactile을 쓰지 않고 `wrist F/T`로 기록한다. History 길이는 main table이 아니라 evidence에서 관리한다.
3. `[Evidence]` DexMove는 initial hand contact pose를 object point cloud와 target object pose에 조건화한다. 따라서 `Geometry available at deployment = Observed point cloud`로 분류한다.
4. `[Evidence]` ForceVLA는 별도 tactile array 없이 real-time 6-axis end-effector wrench, RGB vision과 proprioception으로 TCP pose와 gripper-width action chunk를 생성한다. Insertion·pumping·wiping·peeling을 포함하므로 contact-aware VLA의 강한 정식 게재 비교군이지만, shelf NPM의 same-task baseline은 아니다.
5. `[Evidence]` Tactile-VLA는 dual high-resolution tactile의 normal/shear history로 target position과 contact force를 예측하고 hybrid position–force controller 및 선택적 CoT replanning을 사용한다. 현재 확인 가능한 출판 상태는 arXiv 2025이므로 preprint adjacent comparator로 구분한다.
6. `[Evidence]` GD2P는 object point cloud를 BPS로 표현하고 push/pull direction에 조건화된 wrist pose와 finger configuration을 생성한다. Point cloud/BPS와 direction은 각각 processed geometry와 task conditioning이므로 raw-sensor 기준의 `Physical sensor input`에서는 제외한다. 접촉 이후에는 선택한 pose를 유지한 채 정해진 방향으로 이동하므로 `Task-conditioned initial only`이며 online contact-feedback policy는 아니다.

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
2. `Geometry available at deployment`의 representation과 fidelity 표기가 실제 deployment input과 일치하는지 다시 검증한다.
3. 각 `Physical sensor input`이 실제 sensor stream인지, processed geometry·goal·privileged information인지 구분한다.
4. Wrist F/T가 actor/control input인지, reward·estimation·evaluation에서만 사용되는지 구분한다.
5. Surrounding-object 조건을 `Target contact only / Avoid surrounding contact / Use surrounding contact / Environmental contact required / Depends on task / Undecided` 중 하나로 일관되게 판정한다.
6. 발표 표에는 검증이 끝난 row만 남기고 불확실한 cell은 `NR`로 표시한다.

GD2P, ForceVLA와 Tactile-VLA의 geometry/input, action, controller와 task는 2026-09-18 원문에서 확인했다. Tactile-VLA는 제출·발표 시점에 정식 게재 여부를 다시 확인하며, 확인 전까지 `preprint adjacent comparator`로 유지한다.

현재 가장 먼저 원문을 대조할 순서는 [B81](https://doi.org/10.1109/LRA.2026.3655262) → [B22](https://openreview.net/forum?id=dT3ZciXvNX) → [B12](https://doi.org/10.48550/arXiv.2412.13157) → [B92](https://doi.org/10.15607/RSS.2024.XX.135) → [B90](https://doi.org/10.15607/RSS.2025.XXI.154)이다.

---

## 8. Research gap에서 candidate contribution으로

Previous Works의 결론은 `우리의 조합이 새롭다`가 아니다. 문헌으로 확인된 범위 안에서 **두 개의 candidate question과 하나의 evaluation requirement**를 도출한 것이다. 다음 문서는 C1·C2를 비교 조건, metric과 지지·기각 결과가 있는 claim으로 바꾸고, factorized Rotation-to-Push analysis는 두 claim의 공통 평가로 둔다.

> **C1·C2를 어떤 matched experiment로 검증해야 하며, Rotation-to-Push 분석을 포함한 어떤 결과가 나와야 실제 contribution으로 확정할 수 있는가?**

이 질문은 [Candidate Contributions](./contributions.md)에서 다룬다.
