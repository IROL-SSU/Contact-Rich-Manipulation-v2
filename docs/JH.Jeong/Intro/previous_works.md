# Closest Previous Works — From Method Choice to Research Gaps

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 우리와 가까운 nonprehensile manipulation system을 geometry representation, contact feedback·configuration, action generation과 manipulation organization의 method concept로 비교하고, 표에서 직접 도출되는 연구 질문을 기록한다.
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

Codebook은 저자가 사용한 표현을 그대로 옮기는 용도가 아니라, 서로 다른 논문을 같은 질문으로 판정하기 위한 규칙이다. Main table에는 3–4개의 상위 code만 쓰고, 세부 subtype은 괄호 또는 paper-level evidence에 남긴다. `NR`은 논문에서 확인되지 않았다는 뜻이다.

### 2.1 Column audit

이 표의 목적은 논문별 구현 요소를 빠짐없이 나열하는 것이 아니라, **각 method가 어떤 원리로 동작하며 Ours가 그 조합에서 어디에 위치하는지**를 보여주는 것이다. 따라서 독립적인 method concept만 core column으로 두고, sensor·tool·action의 세부 종류는 해당 concept의 괄호 안 근거로 묶는다.

| 비교 항목 | 연구 질문과의 연결 | 판정 |
| --- | --- | --- |
| Geometry-representation concept | 형상 정보를 전혀 쓰지 않는지, 영상에 암묵적으로 맡기는지, 명시적 geometry를 쓰는지 구분 | **Core** |
| Contact-feedback concept | 접촉 신호가 상태 추정에만 쓰이는지, 실제 action correction까지 닫힌 고리를 이루는지 구분 | **Core** |
| Contact-configuration concept | 접촉 구성이 고정인지, 시작 전에 한 번 선택되는지, 실행 중 갱신되는지 구분 | **Core** |
| Action-generation concept | 행동을 model/control, learned policy 또는 두 방식의 결합 중 무엇으로 생성하는지 구분 | **Core** |
| Manipulation organization | 한 동작만 수행하는지, contact formation과 실행을 잇는지, 여러 skill/stage를 하나의 system으로 다루는지 구분 | **Core** |
| Contact tool + Commanded action | 접촉을 바꿀 수 있는 물리적 authority를 설명하지만 독립 concept는 아님 | **Contact-configuration 괄호 근거로 통합** |
| Physical sensor input | feedback의 정보량을 설명하지만 sensor 보유 자체가 method는 아님 | **Contact-feedback 괄호 근거로 통합** |
| Evaluation setting + External-contact policy | 결과가 나온 물리 조건과 주변 접촉의 허용 범위를 설명 | **Context metadata** |
| Geometry-uncertainty test + Reported endpoint | method 구조가 아니라 실험 설계와 결과 보고 수준을 설명 | **Main comparison에서 제외** |

`Training source`, network size, demonstration·interaction budget과 safety threshold도 중요한 공정성 조건이지만 method concept는 아니다. 이 항목들과 geometry perturbation·endpoint 정의는 Section 4의 evidence와 별도 evaluation plan에서 관리한다.

### 2.2 Column overview

아래 다섯 축이 발표용 main table의 전부다. 각 축은 허용값을 3–4개의 상위 concept로 제한하며, 구체 sensor·tool·representation은 괄호에만 남긴다. 값의 세부 판정은 Sections 2.3–2.5를 참조한다.

| Method concept | 판정 질문 | 허용값 |
| --- | --- | --- |
| **Geometry representation** | 실행 중 object/environment 형상을 어떤 형태로 사용하는가? | `No shape representation` / `Implicit visual` / `Estimated explicit geometry` / `Exact model` |
| **Contact feedback** | 실제 접촉 정보가 action loop에서 어떤 역할을 하는가? | `None` / `State estimation` / `Closed-loop correction` / `Estimation + correction` |
| **Contact configuration** | 손·도구와 물체의 접촉 구성을 언제 바꾸는가? | `Fixed` / `Initial selection` / `Online adaptation` |
| **Action generation** | 실행 action을 만드는 주된 원리는 무엇인가? | `Model/control-based` / `Learning-based` / `Hybrid` |
| **Manipulation organization** | 접촉 형성과 manipulation skill을 어떤 단위로 조직하는가? | `Single-skill` / `Contact formation + execution` / `Multi-skill or multi-stage` |

이 구조에서 modality와 embodiment는 사라지는 것이 아니라 **concept를 구체화하는 evidence tag**가 된다. 예를 들어 `Closed-loop correction (tactile + wrist F/T)`와 `Online adaptation (dexterous wrist + finger)`처럼 쓴다. RL·IL·VLA도 모두 `Learning-based`로 묶되 subtype을 괄호에 남겨 학습 원리의 차이를 보존한다.

### 2.3 Geometry representation and context

#### Context metadata: Evaluation setting — 어디에서 평가했는가?

논문이 주장하는 일반성이 아니라 실제 experiment와 deployment evaluation에 등장한 공간·접촉 구조로 판정한다.

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `Open` | 주변 구조물이 핵심 조건이 아닌 작업 공간 | Open tabletop처럼 target과 support plane을 중심으로 평가 |
| `Cluttered` | 주변 물체가 있는 작업 공간 | 다른 물체가 접근, 경로 또는 충돌 가능성에 영향을 줌 |
| `Fixture-constrained` | 고정 구조물이 motion/contact를 제한하는 공간 | Fixture, hole, peg, slot, wall 또는 shelf를 포함 |
| `Mixed` | 둘 이상의 공간 구조를 평가 | Open·cluttered·constrained setting을 한 논문에서 함께 다룸 |

#### Context metadata: External-contact policy — Support 이외의 환경 접촉을 이용하는가?

정상적인 object–support-plane 접촉은 이 column에서 세지 않는다. Tool–target 접촉 이외의 robot/object–pillar, wall, fixture 또는 주변 물체 접촉이 task에서 어떤 역할을 하는지를 기록한다.

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `No external-contact use` | 외부 접촉을 조작에 사용하지 않음 | 외부 접촉이 없거나 collision으로 회피하는 경우를 포함 |
| `External contact exploited` | 외부 반력을 유리하게 이용함 | Wall·fixture·다른 물체를 pivot, bracing 또는 motion constraint로 활용 |
| `External contact required` | 외부 접촉 없이는 task가 성립하지 않음 | Insertion, wiping, constrained sliding처럼 environment contact가 필수 |
| `Mixed` | Task에 따라 외부 접촉의 역할이 달라짐 | 같은 system이 일부 task에서는 회피하고 다른 task에서는 이용·요구함 |

Evaluation setting과 external-contact policy는 독립적으로 판정한다. 예를 들어 fixture-constrained environment에서도 pillar contact를 회피하면 `No external-contact use`다.

##### 현재 task에서의 중요도

| Contact type | 현재 처리 | 연구상 의미 |
| --- | --- | --- |
| Blocker–shelf support contact | **필수·허용** | Pushing·pivoting의 마찰과 지지 반력을 결정하는 핵심 dynamics이지만 모든 episode에 존재하므로 external-contact code에서는 제외 |
| Hand–blocker contact | **의도적으로 형성·조정** | 본 연구의 직접 조작 contact이며 tactile/F/T feedback과 C1·C2의 핵심 |
| Robot/blocker–pillar contact | **회피·safety violation** | 현재 baseline에서는 manipulation resource가 아니라 collision/safety 항목 |
| Blocker–surrounding-object contact | **Stage 1에서는 없음 또는 회피** | H1–H4를 분리하기 위해 S0에서는 추가 movable object를 두지 않고, S1을 사용하면 forbidden-contact robustness로 평가 |
| Wall·pillar·다른 물체를 이용한 bracing/pivot | **현재 범위 밖** | 채택하면 external-contact exploitation이 독립 method 축이 되며 observation, reward와 closest baseline을 다시 정의해야 함 |

따라서 **support contact는 현재 task에 필수지만, support 이외의 주변 접촉을 이용하는 것은 현재 핵심 가설이 아니다.** `External-contact policy`는 extrinsic-contact 연구와의 조건 차이를 보여주는 secondary comparison column으로 유지한다. 현재 Ours는 `No external-contact use`이며, 실제 shelf task에서 pillar·주변 물체 접촉이 빈번하거나 이를 이용해야 성공 영역이 넓어진다는 근거가 생길 때만 별도 research question으로 승격한다.

승격 여부는 다음 순서로 판단한다.

1. S0 rollout에서 direct/rotate-then-push failure가 external contact 부재 때문에 발생하는지 확인한다.
2. S1에서 incidental pillar·surrounding-object contact의 빈도와 safety impact를 측정한다.
3. Deliberate bracing/pivot을 허용한 S2가 동일 sensing·action budget에서 성공 가능 영역을 유의하게 넓힐 때만 external-contact exploitation을 method scope에 포함한다.

#### Geometry representation — 실행 시 어떤 형상 표현을 사용하는가?

Geometry는 `표현 방식 + 정확도 수준`으로 기록한다. 이 column은 raw sensor 종류가 아니라 deployment system이 action을 결정할 때 이용할 수 있는 **형상 표현**을 나타낸다.

| 허용값 | 직관적 의미 | 포함 조건과 제외 조건 |
| --- | --- | --- |
| `No shape representation` | 형상 표현을 action 결정에 사용하지 않음 | Pose·tactile·proprioception만 사용하고 visual shape feature도 사용하지 않음 |
| `Implicit visual` | 영상 encoder가 형상 단서를 암묵적으로 사용 | RGB뿐 아니라 depth/RGB-D를 직접 인코딩하되 구조화된 geometry를 만들지 않음 |
| `Estimated explicit geometry` | 관측·추정된 구조화 geometry를 사용 | Partial point cloud, BPS, estimated OBB처럼 오차가 있는 explicit representation을 포함 |
| `Exact model` | 정확한 known model을 사용 | CAD·mesh·dimension이 pose와 정합되어 planning/control에 사용됨 |

`Implicit visual`은 “RGB만 사용한다”는 뜻이 아니다. RGB·depth·RGB-D를 영상 형태 그대로 encoder에 넣어 내부 feature로 처리하면 `Implicit visual`이다. 영상에서 point cloud나 OBB를 만들어 policy에 주면 `Estimated explicit geometry`로 기록한다. Point cloud·BPS·OBB의 세부 표현은 괄호로 병기한다. Marker나 tracker가 제공한 pose만으로는 object shape를 알 수 없으므로 pose 자체는 geometry로 세지 않는다.

### 2.4 Contact concepts and evidence tags

#### Evidence tag: Contact interface — 무엇으로 물체에 접촉하는가?

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `Rigid EEF/tool` | 관절 configuration을 바꾸지 않는 contact interface | Rigid EEF, tactile pusher와 grasped rigid object/tool을 포함 |
| `Gripper` | 개폐 또는 adaptive closure를 사용하는 gripper | Parallel/adaptive gripper를 포함하며 독립적인 다지 joint control은 없음 |
| `Dexterous hand` | 여러 finger joint를 갖는 다지 hand | Finger configuration을 접촉 형성이나 online adaptation에 사용할 수 있음 |
| `Mixed` | 둘 이상의 tool group을 비교·사용 | 동일 논문에서 rigid tool, gripper와 hand 범주가 task에 따라 달라짐 |

#### Evidence tag: Physical sensing — 어떤 deployment sensor stream을 받는가?

여러 입력은 `Vision → Tactile/Contact state → Wrist F/T → Proprioception` 순으로 기록한다. Main table의 `Contact feedback` 괄호에는 이 중 실제 접촉 판단·보정에 쓰이는 modality만 남기고, 가공된 geometry, task goal과 language instruction은 제외한다.

| 표기 | 의미 | 경계 |
| --- | --- | --- |
| `Vision` | Camera image stream | RGB/depth/RGB-D subtype을 괄호로 병기하고 point cloud·OBB·pose는 geometry column에 기록 |
| `Tactile` | 별도 tactile array, image 또는 taxel stream | Wrist force와 binary simulator contact state는 포함하지 않음 |
| `Contact state` | Contact flag 또는 discrete contact mode | Spatial tactile pattern이나 6-axis wrench와 구분 |
| `Wrist F/T` | Wrist/TCP의 force·torque 또는 wrench | Reward-only privileged force는 deployment input으로 세지 않음 |
| `Proprioception` | Joint position·velocity, gripper width 등 robot 내부 상태 | Command나 task goal은 포함하지 않음 |
| `NR` | Raw sensor source를 논문에서 확인할 수 없음 | Preprocessed geometry만 명시된 경우 설명을 괄호에 덧붙임 |

#### Evidence tag: Action authority — system이 실제로 무엇을 명령하는가?

| 표기 | 의미 |
| --- | --- |
| `EEF/TCP motion` | End-effector, TCP 또는 grasped object의 pose, delta pose, velocity·trajectory command |
| `Gripper/finger actuation` | Gripper width, closure 또는 finger-joint command |
| `Force command` | Target force 또는 force-control reference |

복수 action family는 `+`로 연결한다. Initial pose만 생성하는지 online command를 반복하는지는 다음 `Configuration adaptation`에서 구분하고, 세부 command는 paper-level evidence에 남긴다.

#### Contact configuration — contact configuration을 언제 바꾸는가?

Configuration은 접촉을 형성하는 wrist pose와 articulated tool DOF를 뜻한다. 단순 EEF trajectory tracking은 configuration adaptation으로 세지 않는다.

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `Fixed` | 별도 configuration이 없거나 시작 후 고정 | Rigid EEF/tool 또는 고정된 grasp·finger posture |
| `Initial selection` | Task에 맞춘 initial configuration만 선택 | Contact 전 wrist/tool/finger configuration을 정하고 이후에는 갱신하지 않음 |
| `Online adaptation` | 실행 중 configuration을 반복 갱신 | Feedback에 따라 gripper, wrist 또는 finger configuration을 수정 |

### 2.5 Action and execution concepts

#### Action generation — 행동을 만드는 주된 원리는 무엇인가?

| 허용값 | 직관적 의미 | 판정 기준 |
| --- | --- | --- |
| `Model/control-based` | Rule, model, optimization 또는 search가 action을 결정 | Feedback control, trajectory/contact-mode planning과 generative candidate planning을 포함 |
| `Learning-based` | 학습된 policy/model이 deployment action을 주도 | RL·IL·VLA는 subtype으로 괄호에 명시 |
| `Hybrid` | Model/control과 learned policy가 action generation에 함께 관여 | `Optimization + RL`, `VLA + force control`처럼 결합 요소를 괄호에 명시 |

Encoder 종류나 sensor 유무는 method group을 결정하지 않는다. 생성 model이 후보 pose만 만들고 planner가 최종 action을 실행하면 `Model/control-based`로 분류한다. 복수 component가 있어도 하나가 perception이나 initialization에만 쓰이면 자동으로 `Hybrid`로 분류하지 않는다.

#### Manipulation organization — contact와 motion을 어떤 단위로 조직하는가?

| 허용값 | 의미 |
| --- | --- |
| `Single-skill` | Push, pull 또는 pivot처럼 한 manipulation skill의 실행을 다룸 |
| `Contact formation + execution` | Task-conditioned contact/pose 형성과 이후 manipulation을 모두 system 범위에 포함 |
| `Multi-skill or multi-stage` | 여러 contact task를 다루거나 둘 이상의 의미 있는 stage를 한 system에서 연결 |

괄호에는 `(push)`, `(general contact tasks)`, `(Formation → Rotate → Push)`처럼 실제 범위를 적는다. 단순히 initial pose를 입력으로 받는 것과 system이 contact formation을 생성하는 것을 구분한다.

#### Contact feedback — 접촉 정보가 action loop에서 어디에 쓰이는가?

| 허용값 | 의미 |
| --- | --- |
| `None` | Tactile·contact state·F/T를 deployment action에 사용하지 않음 |
| `State estimation` | Contact 정보로 pose·contact mode·uncertainty를 추정하지만 action correction은 별도 module이 담당 |
| `Closed-loop correction` | Contact 정보가 policy/controller의 반복 action update에 직접 입력됨 |
| `Estimation + correction` | Contact-state estimation과 closed-loop action correction을 모두 명시적으로 수행 |

Sensor가 존재한다는 사실만으로 `Closed-loop correction`으로 판정하지 않는다. Reward-only force, offline label과 evaluation measurement는 deployment feedback에서 제외한다.

Tool·sensor·action의 원래 분류는 위 판정을 재현하기 위한 evidence vocabulary로만 사용한다. Main table에서는 `Contact-feedback concept`에 sensor와 역할을 함께 적고, `Contact-configuration concept`에 embodiment·action authority와 adaptation timing을 함께 적는다. Task 이름 자체도 method가 아니므로 `Manipulation organization`의 근거로만 사용한다.

Geometry perturbation, held-out split, phase/final success와 transition metric은 중요한 검증 항목이지만 **method concept 비교에는 포함하지 않는다.** 이들은 Section 4의 paper-level evidence와 별도 evaluation plan에서 관리한다. `Robustness`, `Context awareness`, `Generalizable`, `Controllability`처럼 논문마다 의미가 달라지는 포괄적 표현도 column으로 사용하지 않는다.

---

## 3. Canonical comparison matrix

Main table은 Environment–Agent–System inventory가 아니라 다섯 개의 **method concept**만 비교한다. Sensor, tool과 action subtype은 concept를 판정하는 괄호 근거이며, 실험 조건은 별도 context metadata로 분리한다. `Comparison role` 역시 표에 넣지 않고 direct baseline인지 adjacent comparator인지는 Section 4에서 설명한다.

### 3.1 Method-concept comparison

| Work | Geometry representation | Contact feedback | Contact configuration | Action generation | Manipulation organization |
| --- | --- | --- | --- | --- | --- |
| [B37 · Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) | No shape representation | Closed-loop correction (tactile) | Fixed (rigid pusher; wrist) | Model/control-based (feedback control) | Single-skill (push) |
| [B84 · Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) | Implicit visual | Closed-loop correction (contact state) | Fixed (rigid EEF; wrist) | Learning-based (RL) | Single-skill (push) |
| [B12 · Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157) | Implicit visual | Estimation + correction (wrist F/T)² | Fixed (rigid EEF; wrist) | Learning-based (RL) | Single-skill (push) |
| [B92 · Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135) | Exact model | Estimation + correction (tactile; optional F/T) | Fixed (grasped tool; wrist) | Model/control-based (optimization + control) | Multi-skill or multi-stage (general contact tasks) |
| [B90 · HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154) | Estimated explicit geometry (point cloud) | None | Fixed (gripper; wrist) | Learning-based (RL) | Multi-skill or multi-stage (general NPM) |
| [B01 · GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Estimated explicit geometry (point cloud → BPS)⁶ | None | Initial selection (dexterous wrist + finger pose)⁶ | Model/control-based (generative pose planning) | Contact formation + execution (push/pull) |
| [B22 · DexMove](https://openreview.net/forum?id=dT3ZciXvNX) | Estimated explicit geometry (point cloud)³ | Closed-loop correction (tactile) | Online adaptation (dexterous wrist + finger)³ | Learning-based (IL) | Multi-skill or multi-stage (contact formation + general NPM) |
| [B81 · Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Implicit visual | Closed-loop correction (contact state + wrist F/T) | Fixed (rigid EEF; wrist) | Hybrid (optimization + RL) | Multi-skill or multi-stage (push + pivot) |
| [B46 · ForceVLA](https://doi.org/10.52202/085713-3124) | Implicit visual (RGB)⁴ | Closed-loop correction (wrist F/T) | Online adaptation (gripper/grasped tool; wrist + gripper)⁴ | Learning-based (VLA) | Multi-skill or multi-stage (general contact tasks) |
| [B48 · Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) | Implicit visual (RGB)⁵ | Closed-loop correction (tactile) | Online adaptation (gripper; wrist + gripper + force)⁵ | Hybrid (VLA + force control) | Multi-skill or multi-stage (general contact tasks) |
| **Ours** | **Estimated explicit geometry (OBB)** | **Closed-loop correction (binary tactile + wrist F/T)** | **Online adaptation (dexterous wrist + finger)** | **Learning-based (RL)** | **Multi-skill or multi-stage (Contact formation → Rotate/pivot → Push)** |

이 표가 보여주는 Ours의 위치는 “RL을 쓴다”거나 “tactile/F/T를 쓴다”는 개별 요소가 아니다. 핵심 조합은 **approximate explicit geometry를 nominal guide로 사용하고, 실제 접촉 오차는 coarse tactile/F/T의 closed-loop feedback으로 보완하며, dexterous wrist–finger contact configuration을 실행 중 갱신해 contact formation–rotation–push를 연결한다**는 것이다.

### 3.2 Context metadata

아래 항목은 method concept가 아니라 결과가 나온 조건이다. 발표 본문에서는 생략하고, 환경 차이가 해석을 바꿀 때만 보조 표로 제시한다. 특히 주변 접촉은 현재 Ours가 의도적으로 이용하는 resource가 아니므로 core method 축으로 올리지 않는다.

| ID | Work | 평가 환경<br>(Evaluation setting) | 외부 접촉 정책<br>(External-contact policy) |
| --- | --- | --- | --- |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | Goal-Driven Robotic Pushing | Open | No external-contact use |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal-Oriented Pushing in Clutter | Cluttered | No external-contact use |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | Visuotactile Estimation under Occlusions | Open | No external-contact use |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | Tactile-Driven Contact Mode Control | Fixture-constrained | External contact exploited |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | HAMNET | Mixed | External contact exploited |
| [B01](https://doi.org/10.48550/arXiv.2509.18455) | GD2P | Open | No external-contact use |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | DexMove | Open | No external-contact use |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | Optimization-Guided Non-Prehensile RL | Mixed | Mixed |
| [B46](https://doi.org/10.52202/085713-3124) | ForceVLA | Fixture-constrained | External contact required |
| [B48](https://doi.org/10.48550/arXiv.2507.09160) | Tactile-VLA | Fixture-constrained | External contact required |
| — | **Ours** | Fixture-constrained | No external-contact use¹ |

1. `[Baseline/Open]` Blocker–shelf support contact는 필수·허용한다. Pillar 및 비표적 물체 접촉은 현재 baseline에서 회피하며, Stage 1의 S0 condition에는 추가 movable surrounding object를 두지 않는다. S1 robustness condition과 deliberate external-contact exploitation은 아직 미결이다.
2. `[Evidence]` B12가 `tactile`이라고 부르는 입력은 별도 tactile array가 아니라 end-effector force measurement다. 따라서 `Contact feedback`의 modality는 `wrist F/T`로 기록한다. History 길이는 main table이 아니라 evidence에서 관리한다.
3. `[Evidence]` DexMove는 initial hand contact pose를 object point cloud와 target object pose에 조건화한다. 따라서 `Geometry representation = Estimated explicit geometry (point cloud)`로 분류한다.
4. `[Evidence]` ForceVLA는 별도 tactile array 없이 real-time 6-axis end-effector wrench, RGB vision과 proprioception으로 TCP pose와 gripper-width action chunk를 생성한다. Insertion·pumping·wiping·peeling을 포함하므로 contact-aware VLA의 강한 정식 게재 비교군이지만, shelf NPM의 same-task baseline은 아니다.
5. `[Evidence]` Tactile-VLA는 dual high-resolution tactile의 normal/shear history로 target position과 contact force를 예측하고 hybrid position–force controller 및 선택적 CoT replanning을 사용한다. 현재 확인 가능한 출판 상태는 arXiv 2025이므로 preprint adjacent comparator로 구분한다.
6. `[Evidence]` GD2P는 object point cloud를 BPS로 표현하고 push/pull direction에 조건화된 wrist pose와 finger configuration을 생성한다. Point cloud/BPS는 explicit geometry이고 direction은 task conditioning이다. 접촉 이후에는 선택한 pose를 유지한 채 정해진 방향으로 이동하므로 `Contact configuration = Initial selection`이며 online contact-feedback policy는 아니다.

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
- 현재 표에서는 `approximate explicit geometry + coarse contact feedback + online dexterous wrist–finger adaptation + multi-stage manipulation`의 효과를 동일한 조건에서 분해한 연구가 확인되지 않았다.

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

1. B37, B84, B12, B92, B90, B22와 B81의 full text에서 각 concept 판정의 근거 문장·figure·section을 기록한다.
2. `Geometry representation`이 실제 deployment input인지 확인하고, implicit image와 explicit geometry를 구분한다.
3. `Contact feedback`은 sensor 존재 여부가 아니라 estimation과 action correction 중 실제 역할로 판정한다.
4. `Contact configuration`은 tool 이름만 보지 않고 wrist/finger authority와 adaptation timing을 함께 확인한다.
5. `Action generation`은 보조 encoder가 아니라 최종 action을 결정하는 주 mechanism을 기준으로 판정한다.
6. `Manipulation organization`은 논문이 실제 생성하는 stage만 포함하고 scripted downstream execution은 분리한다.
7. Support contact와 external contact를 분리하되, 주변 접촉을 의도적으로 이용하지 않는 한 context metadata에만 둔다.
8. 발표 표에는 검증이 끝난 row만 남기고 불확실한 concept는 `NR`로 표시한다.

GD2P, ForceVLA와 Tactile-VLA의 geometry/input, action, controller와 task는 2026-09-18 원문에서 확인했다. Tactile-VLA는 제출·발표 시점에 정식 게재 여부를 다시 확인하며, 확인 전까지 `preprint adjacent comparator`로 유지한다.

현재 가장 먼저 원문을 대조할 순서는 [B81](https://doi.org/10.1109/LRA.2026.3655262) → [B22](https://openreview.net/forum?id=dT3ZciXvNX) → [B12](https://doi.org/10.48550/arXiv.2412.13157) → [B92](https://doi.org/10.15607/RSS.2024.XX.135) → [B90](https://doi.org/10.15607/RSS.2025.XXI.154)이다.

---

## 8. Research gap에서 candidate contribution으로

Previous Works의 결론은 `우리의 조합이 새롭다`가 아니다. 문헌으로 확인된 범위 안에서 **두 개의 candidate question과 하나의 evaluation requirement**를 도출한 것이다. 다음 문서는 C1·C2를 비교 조건, metric과 지지·기각 결과가 있는 claim으로 바꾸고, factorized Rotation-to-Push analysis는 두 claim의 공통 평가로 둔다.

> **C1·C2를 어떤 matched experiment로 검증해야 하며, Rotation-to-Push 분석을 포함한 어떤 결과가 나와야 실제 contribution으로 확정할 수 있는가?**

이 질문은 [Candidate Contributions](./contributions.md)에서 다룬다.
