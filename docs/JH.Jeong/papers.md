# JH.Jeong Paper List

> **범위:** Track B 연구를 위해 현재까지 확인하거나 검토 후보로 수집한 논문
>
> **최종 갱신:** 2026-09-15

---

## 1. 문서 사용법

이 문서는 먼저 기존 핵심 논문을 ID 순서로 제시하고, 이후 연구 목적별로 관련 논문을 다시 그룹핑한다. B01–B40은 [`context.md`](./context.md)와 같은 ID 체계를 사용한다. `docs/ICRA&IROS`의 전수 screening 결과는 5절에서 학회별로 분리하고 `ICRAyy-NNN`/`IROSyy-NNN` ID를 사용한다. 이 corpus에서 핵심 목록으로 승격할 논문은 후속 검토 후 B41부터 번호를 부여한다.

- 목록 포함은 최종 baseline 선정, 방법 채택 또는 실험 재현을 의미하지 않는다.
- 목적별 그룹은 서로 배타적이지 않다. 한 논문이 여러 목적에 활용될 수 있다.
- arXiv DOI와 정식 출판 DOI를 구분한다. 정식 DOI 미확인은 DOI가 없다는 뜻이 아니다.
- 논문별 상세 해석, 연구와의 연결점과 주의사항은 [`context.md`](./context.md)에서 관리한다.
- 후속 문헌 탐색에서 관련 논문을 새로 확인하면 이 목록과 해당 목적별 그룹을 함께 갱신한다.
- 5절의 항목은 CSV의 제목·초록·Author Keywords·IEEE Terms를 모두 확인한 **전수 1차 screening 결과**다. 원문 전체를 독해하거나 방법을 재현했다는 뜻은 아니며, 논문 claim·baseline 채택 전에는 원문을 다시 확인한다.

---

## 2. 기존 B-ID 논문 목록

| ID | 논문 | 발표 | 핵심 분류 | DOI | 원문·공식 자료 |
| --- | --- | --- | --- | --- | --- |
| B01 | Learning Geometry-Aware Nonprehensile Pushing and Pulling with Dexterous Hands (GD2P) | ICRA 2026; arXiv 2025 | Geometry-conditioned dexterous hand pose; pushing/pulling | [arXiv DOI](https://doi.org/10.48550/arXiv.2509.18455) | [Paper](https://arxiv.org/html/2509.18455v4) |
| B02 | Task-Oriented Dexterous Hand Pose Synthesis Using Differentiable Grasp Wrench Boundary Estimator (TaskDexGrasp) | IROS 2024; arXiv 2023 | Task-wrench-aware hand-pose optimization | [Publication DOI](https://doi.org/10.1109/IROS58592.2024.10802652) | [Paper](https://arxiv.org/html/2309.13586v3) |
| B03 | Composing Dextrous Grasping and In-hand Manipulation via Scoring with a Reinforcement Learning Critic | ICRA 2025 | RL critic 기반 초기 grasp 후보 선택 | [Publication DOI](https://doi.org/10.1109/ICRA55743.2025.11127792) | [Paper](https://arxiv.org/abs/2505.13253) |
| B04 | GraspXL: Generating Grasping Motions for Diverse Objects at Scale | ECCV 2024 | Goal-conditioned RL grasp-motion synthesis | [Publication DOI](https://doi.org/10.1007/978-3-031-73347-5_22) | [Paper](https://arxiv.org/html/2403.19649v2) |
| B05 | Dexterous Functional Pre-Grasp Manipulation with Diffusion Policy (UniDexFPM) | arXiv 2024 | RL expert와 diffusion policy 기반 functional pre-grasp | [arXiv DOI](https://doi.org/10.48550/arXiv.2403.12421) | [Paper](https://arxiv.org/html/2403.12421v2) |
| B06 | HANDFUL: Sequential Grasp-Conditioned Dexterous Manipulation with Resource Awareness | arXiv 2026 | Finger-wise contact reward; sequential RL curriculum | [arXiv DOI](https://doi.org/10.48550/arXiv.2604.25126) | [Paper](https://arxiv.org/html/2604.25126) |
| B07 | Dexterous Non-Prehensile Manipulation for Ungraspable Object via Extrinsic Dexterity (ExDex) | arXiv 2025 | RL 기반 arm–hand nonprehensile manipulation | [arXiv DOI](https://doi.org/10.48550/arXiv.2503.23120) | [Paper](https://arxiv.org/html/2503.23120v1) |
| B08 | Sequential Dexterity: Chaining Dexterous Policies for Long-Horizon Manipulation | CoRL 2023 | RL skill chaining | [arXiv DOI](https://doi.org/10.48550/arXiv.2309.00987) | [Project](https://sequential-dexterity.github.io/) |
| B09 | DexTouch: Learning to Seek and Manipulate Objects with Tactile Dexterity | IEEE RA-L 2024 | Tactile RL; sim-to-real | [Publication DOI](https://doi.org/10.1109/LRA.2024.3478571) | [Paper](https://arxiv.org/html/2401.12496v2) |
| B10 | Sim-to-Real Model-Based and Model-Free Deep Reinforcement Learning for Tactile Pushing | IEEE RA-L 2023 | Tactile goal-conditioned pushing RL | [Publication DOI](https://doi.org/10.1109/LRA.2023.3295236) | [Paper](https://arxiv.org/abs/2307.14272) |
| B11 | Bi-Touch: Bimanual Tactile Manipulation with Sim-to-Real Deep Reinforcement Learning | IEEE RA-L 2023 | Bimanual tactile RL | [Publication DOI](https://doi.org/10.1109/LRA.2023.3295991) | [Project](https://sites.google.com/view/bi-touch/) |
| B12 | Learning Visuotactile Estimation and Control for Non-prehensile Manipulation under Occlusions | CoRL 2024; PMLR 2025 | Visuotactile estimation; uncertainty-aware RL | [arXiv DOI](https://doi.org/10.48550/arXiv.2412.13157) | [Paper](https://proceedings.mlr.press/v270/ferrandis25a.html) |
| B13 | Coarse-to-Fine Robotic Pushing Using Touch, Vision and Proprioception | IEEE RA-L 2025; online 2024 | Multimodal coarse-to-fine pushing | [Publication DOI](https://doi.org/10.1109/LRA.2024.3511378) | [Publication](https://research-information.bris.ac.uk/en/publications/coarse-to-fine-robotic-pushing-using-touch-vision-and-propriocept/) |
| B14 | Force Push: Robust Single-Point Pushing with Force Feedback | IEEE RA-L 2024 | Force-feedback/admittance pushing control | [Publication DOI](https://doi.org/10.1109/LRA.2024.3414180) | [Paper](https://arxiv.org/abs/2401.17517) |
| B15 | RoboPack: Learning Tactile-Informed Dynamics Models for Dense Packing | RSS 2024 | Visuotactile recurrent dynamics model; MPC | [Publication DOI](https://doi.org/10.15607/RSS.2024.XX.130) | [Paper](https://www.roboticsproceedings.org/rss20/p130.html) |
| B16 | Synthesize Dexterous Nonprehensile Pregrasp for Ungraspable Objects | ACM SIGGRAPH Conference Proceedings 2023 | Graph search; optimal control; learned graspability | [Publication DOI](https://doi.org/10.1145/3588432.3591528) | [Paper](https://arxiv.org/abs/2305.04654) |
| B17 | RetrDex: Efficient Object Retrieval in Cluttered Scenes with a Dexterous Hand | IROS 2026 accepted; arXiv 2025 | Clutter clearing; retrieval RL | [arXiv DOI](https://doi.org/10.48550/arXiv.2502.18423) | [Paper](https://arxiv.org/abs/2502.18423) |
| B18 | Synthesizing Diverse and Physically Stable Grasps with Arbitrary Hand Structures using Differentiable Force Closure Estimator | IEEE RA-L 2022; online 2021 | Differentiable force-closure grasp synthesis | [Publication DOI](https://doi.org/10.1109/LRA.2021.3129138) | [Paper](https://arxiv.org/abs/2104.09194), [Erratum](https://yzhu.io/publication/grasp2021ral/erratum.pdf) |
| B19 | DexGraspNet: A Large-Scale Robotic Dexterous Grasp Dataset for General Objects Based on Simulation | ICRA 2023; arXiv 2022 | Grasp optimization; simulation validation; dataset | [arXiv DOI](https://doi.org/10.48550/arXiv.2210.02697) | [Project](https://pku-epic.github.io/DexGraspNet/) |
| B20 | Get a Grip: Multi-Finger Grasp Evaluation at Scale Enables Robust Sim-to-Real Transfer | CoRL 2024 | Learned grasp-success evaluator | [arXiv DOI](https://doi.org/10.48550/arXiv.2410.23701) | [Paper](https://arxiv.org/abs/2410.23701) |
| B21 | Learning Contact Locations for Pushing and Orienting Unknown Objects | Humanoids 2013 | Shape-based straight-push·orienting contact prediction | [Publication DOI](https://doi.org/10.1109/HUMANOIDS.2013.7030011) | [Author PDF](https://users.cs.utah.edu/~thermans/papers/hermans-ichr2013.pdf) |
| B22 | DexMove: Learning Tactile-Guided Non-Prehensile Manipulation with Dexterous Hands | ICLR 2026 | Tactile-guided wrist–finger trajectory learning | 미확인 | [Project](https://peilin-666.github.io/projects/DexMove/), [OpenReview](https://openreview.net/forum?id=dT3ZciXvNX) |
| B23 | Task-Oriented Contact Optimization for Pushing Manipulation with Mobile Robots | IROS 2022 | Contact-placement optimization for planar trajectory | 미확인 | [Publication](https://iris.unimore.it/handle/11380/1295974) |
| B24 | Tactile-Based Negotiation of Unknown Objects during Navigation in Unstructured Environments with Movable Obstacles | Advanced Intelligent Systems 2024 | 준비 회전; contact selection; tactile compliance | [Publication DOI](https://doi.org/10.1002/aisy.202300621) | [Paper](https://research.chalmers.se/publication/539508/file/539508_Fulltext.pdf) |
| B25 | CORN: Contact-based Object Representation for Nonprehensile Manipulation of General Unseen Objects | ICLR 2024 | Contact-aware representation; goal-conditioned RL | 미확인 | [Paper](https://arxiv.org/html/2403.10760), [Code](https://github.com/iMSquared/corn) |
| B26 | DyWA: Dynamics-adaptive World Action Model for Generalizable Non-prehensile Manipulation | ICCV 2025 | History-conditioned dynamics adaptation | 미확인 | [Paper](https://arxiv.org/html/2503.16806), [Code](https://github.com/jiangranlv/DyWA) |
| B27 | VTDexManip: A Dataset and Benchmark for Visual-tactile Pretraining and Dexterous Manipulation with Reinforcement Learning | ICLR 2025 | Visual–tactile pretraining; dexterous RL benchmark | 미확인 | [Project](https://lqts.github.io/VTDexManip/), [Code](https://github.com/LQTS/VTDexManip) |
| B28 | Where to Touch, How to Contact: A Hierarchical RL–MPC Framework for Geometry-Aware Sim-to-Real Manipulation | arXiv 2026 | Contact intention; high-level RL; contact-implicit MPC | [arXiv DOI](https://doi.org/10.48550/arXiv.2601.10930) | [Paper](https://arxiv.org/abs/2601.10930), [Project](https://zhi-xian-xie.github.io/contact_intention_website/) |
| B29 | Enhancing Exploration with Diffusion Policies in Hybrid Off-Policy RL: Application to Non-Prehensile Manipulation (HyDo) | IEEE RA-L accepted; arXiv 2024 | Hybrid contact-point/motion-parameter diffusion RL | [arXiv DOI](https://doi.org/10.48550/arXiv.2411.14913) | [Paper](https://arxiv.org/abs/2411.14913) |
| B30 | HACMan: Learning Hybrid Actor-Critic Maps for 6D Non-Prehensile Manipulation | CoRL 2023 | Point-cloud contact selection; motion parameters; RL | 미확인 | [Paper](https://proceedings.mlr.press/v229/zhou23a/zhou23a.pdf), [Code](https://github.com/HACMan-2023/HACMan) |
| B31 | HACMan++: Spatially-Grounded Motion Primitives for Manipulation | RSS 2024 | Primitive type·location·parameter selection; RL chaining | 미확인 | [Paper/Project](https://sgmp-rss2024.github.io/), [Code](https://github.com/JiangBowen0008/HACManPP) |
| B32 | Rotating without Seeing: Towards In-hand Dexterity through Touch | RSS 2023 | Binary tactile·proprioception history; tactile RL | [Publication DOI](https://doi.org/10.15607/RSS.2023.XIX.036) | [Paper](https://roboticsproceedings.org/rss19/p036.html), [Project](https://touchdexterity.github.io/) |
| B33 | Robot Synesthesia: In-Hand Manipulation with Visuotactile Sensing | ICRA 2024 | Camera·robot·tactile point-cloud fusion; teacher–student | [Publication DOI](https://doi.org/10.1109/ICRA57147.2024.10610532) | [Paper](https://arxiv.org/abs/2312.01853), [Project](https://yingyuan0414.github.io/visuotactile/) |
| B34 | TacSL: A Library for Visuotactile Sensor Simulation and Learning | IEEE T-RO 2025; arXiv 2024 | GPU visuotactile image·force-field simulation; policy distillation | [Publication DOI](https://doi.org/10.1109/TRO.2025.3547267) | [Paper](https://arxiv.org/abs/2408.06506), [Project](https://iakinola23.github.io/tacsl/) |
| B35 | Isaac Lab: A GPU-Accelerated Simulation Framework for Multi-Modal Robot Learning | arXiv 2025 | Multi-frequency sensors; observations; domain randomization | [arXiv DOI](https://doi.org/10.48550/arXiv.2511.04831) | [Paper](https://arxiv.org/abs/2511.04831), [Code](https://github.com/isaac-sim/IsaacLab) |
| B36 | TacEx: GelSight Tactile Simulation in Isaac Sim — Combining Soft-Body and Visuotactile Simulators | arXiv 2024 | Isaac Sim GelSight image·deformation simulation; RL environments | [arXiv DOI](https://doi.org/10.48550/arXiv.2411.04776) | [Paper](https://arxiv.org/abs/2411.04776), [Project](https://sites.google.com/view/tacex) |
| B37 | Goal-Driven Robotic Pushing Using Tactile and Proprioceptive Feedback | IEEE T-RO 2022; online 2021 | Tactile servoing; pusher–object relative pose; target alignment | [Publication DOI](https://doi.org/10.1109/TRO.2021.3104471) | [Paper](https://arxiv.org/abs/2012.01859), [Publication](https://research-information.bris.ac.uk/en/publications/goal-driven-robotic-pushing-using-tactile-and-proprioceptive-feed/) |
| B38 | On the Continuity of Rotation Representations in Neural Networks | CVPR 2019 | 6D Gram–Schmidt representation; 5D normalized stereographic-projection representation | [Publication DOI](https://doi.org/10.1109/CVPR.2019.00589) | [Paper](https://openaccess.thecvf.com/content_CVPR_2019/html/Zhou_On_the_Continuity_of_Rotation_Representations_in_Neural_Networks_CVPR_2019_paper.html), [Project](https://zhouyisjtu.github.io/project_rotation/rotation.html) |
| B39 | A System for General In-Hand Object Re-Orientation | CoRL 2021; PMLR 2022 | Arbitrary goal orientation; quaternion-difference observation; symmetry | 미확인 | [Paper](https://proceedings.mlr.press/v164/chen22a.html) |
| B40 | General In-Hand Object Rotation with Vision and Touch (RotateIt) | CoRL 2023 | Hand-centric rotation-axis goal; visuotactile rotation | [arXiv DOI](https://doi.org/10.48550/arXiv.2309.09979) | [Paper](https://proceedings.mlr.press/v229/qi23a.html) |

---

## 3. 목적별 논문 그룹

아래 그룹은 논문을 읽고 비교할 목적에 따른 분류다. 동일 논문을 여러 그룹에 중복해 배치한다.

### 3.1 Track B 1단계 핵심 — 주어진 방향의 Pushing을 위한 Hand·Contact Configuration

| 우선 | ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- | --- |
| ★ | B01 | GD2P | 물체 geometry와 pushing direction으로 다지 손 pre-contact pose를 생성·선택하고 실제 push 성공으로 검증하는 방법 |
| ★ | B02 | TaskDexGrasp | Task wrench에 적합한 다지 손 contact configuration을 평가·합성하는 방법 |
| ★ | B21 | Learning Contact Locations for Pushing and Orienting Unknown Objects | 안정적인 직선 밀기와 준비 회전을 위한 contact location을 물체 형상에서 선택하는 고전적 관점 |
|  | B23 | Task-Oriented Contact Optimization | 주어진 물체 궤적을 적은 접촉력으로 수행하기 위한 contact placement 최적화 |
|  | B30 | HACMan | Point cloud에서 contact location과 접촉 후 motion parameter를 함께 학습하는 action representation |
|  | B29 | HyDo | Contact-point selection과 continuous motion parameter를 hybrid RL로 탐색하는 방법 |

### 3.2 Rotation–Push 연결과 Phase·Contact 전환

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| B21 | Learning Contact Locations for Pushing and Orienting Unknown Objects | 물체를 먼저 회전시켜 안정적인 straight-push contact를 목표 방향과 정렬하는 구조 |
| B24 | Tactile-Based Negotiation | 장애물의 준비 회전 후 병진, 접촉 위치 선택과 감각 기반 정렬 |
| B08 | Sequential Dexterity | 이전 policy의 종료 분포와 다음 policy의 실행 가능성을 연결하는 skill chaining |
| B06 | HANDFUL | 순차 manipulation에서 현재 접촉과 후속 사용 자원을 함께 고려하는 curriculum |
| B07 | ExDex | Arm–hand nonprehensile phase 연결과 종료 상태 활용 |
| B16 | Nonprehensile Pregrasp | 후속 grasp를 가능하게 하는 사전 nonprehensile manipulation planning |
| B28 | Where to Touch, How to Contact | Contact location과 post-contact object subgoal을 연결하는 계층적 interface |
| B31 | HACMan++ | Primitive type·location·parameter를 선택하고 여러 primitive를 순서대로 연결하는 방법 |
| B39 | General In-Hand Object Re-Orientation | 임의의 SO(3) goal 도달에서 current–goal quaternion difference와 symmetry-aware success를 사용하는 방식 |
| B40 | RotateIt | 최종 자세가 아니라 hand-centric rotation-axis를 목표로 연속 회전을 학습하는 방식 |

### 3.3 Tactile·F/T 기반 Closed-Loop Contact 형성·보정

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| B09 | DexTouch | Tactile observation을 이용한 접촉 탐색·조작과 sim-to-real sensor ablation |
| B10 | Tactile Pushing | Goal-conditioned pushing에서 tactile 기반 model-based/model-free RL 비교 |
| B11 | Bi-Touch | Tactile 접촉 유지와 물체 reorientation을 위한 bimanual RL |
| B12 | Visuotactile Estimation and Control | Occlusion 아래 pose 추정 불확실성과 contact feedback을 연결하는 방법 |
| B13 | Coarse-to-Fine Pushing | Vision의 전역 정보와 touch·proprioception의 국소 보정 역할 분담 |
| B14 | Force Push | Pose·물성 불확실성 아래 force feedback으로 pushing 방향과 속도를 보정하는 제어 baseline |
| B15 | RoboPack | 접촉 이력으로 latent dynamics를 추정하고 MPC에 활용하는 방법 |
| B22 | DexMove | Tactile 기반 wrist–finger 공동 nonprehensile control |
| B24 | Tactile-Based Negotiation | Contact location과 force/proximity feedback을 이용한 표면 정렬·순응 제어 |
| B27 | VTDexManip | Visual–tactile representation pretraining과 dexterous RL sensor 표현 |
| B32 | Rotating without Seeing | Sensor-link contact-force norm을 threshold한 16D binary tactile와 4-step state stack |
| B33 | Robot Synesthesia | Binary active sensor의 위치를 palm-frame tactile point cloud로 바꾸어 visual point cloud와 결합 |
| B34 | TacSL | Visuotactile RGB와 per-taxel force field를 GPU에서 생성하는 고충실도 tactile simulation 후보 |
| B36 | TacEx | Isaac Sim에서 GelSight deformation·RGB observation을 생성하는 외부 tactile simulation 후보 |
| B37 | Goal-Driven Robotic Pushing | Tactile로 pusher–object 상대 pose를 안정화하고 proprioception으로 target bearing·distance를 정렬하는 역할 분담 |

### 3.4 Goal-Conditioned Nonprehensile Manipulation과 Contact 선택

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| B25 | CORN | Contact prediction으로 사전학습한 geometry representation과 goal-conditioned RL |
| B26 | DyWA | 물성 변화에 적응하는 history-conditioned representation과 dynamics prediction |
| B29 | HyDo | Discrete contact point와 continuous motion을 함께 다루는 hybrid action learning |
| B30 | HACMan | 6D object goal을 위한 spatially grounded contact-point/motion action |
| B01 | GD2P | 주어진 object state와 pushing direction에 대한 geometry-conditioned hand pose |
| B23 | Task-Oriented Contact Optimization | 주어진 planar trajectory에 적합한 contact 배치 결정 |
| B28 | Where to Touch, How to Contact | Surface contact와 object subgoal을 함께 표현하는 contact intention |

### 3.5 Track B 2단계 — 조작 의사결정, 공간 확보와 Retrieval

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| B17 | RetrDex | Clutter clearing을 통해 target retrieval 가능성을 높이는 정책과 시스템 평가 |
| B24 | Tactile-Based Negotiation | Movable obstacle를 회전·이동하여 navigation path를 확보하는 목적 설계 |
| B28 | Where to Touch, How to Contact | High-level RL의 contact/subgoal 선택과 low-level contact dynamics 실행 분리 |
| B31 | HACMan++ | 목적에 따라 motion primitive의 종류·위치·parameter와 순서를 결정하는 구조 |
| B08 | Sequential Dexterity | 장기 과업을 위한 policy chaining과 phase transition |
| B16 | Nonprehensile Pregrasp | 후속 manipulation 성공을 목적으로 하는 사전 물체 재배치 |
| B25 | CORN | Unseen geometry에 대한 goal-conditioned object manipulation |

### 3.6 Hand-Pose 생성·접촉 품질·후속 성공 평가의 배경

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| B02 | TaskDexGrasp | Task Wrench Space와 Grasp Wrench Space를 이용한 task-specific contact quality |
| B03 | RL Critic Grasp Selection | 후속 manipulation critic으로 초기 hand configuration 후보를 평가·선택하는 발상 |
| B04 | GraspXL | Goal·접촉·안정성을 결합한 RL grasp-motion synthesis |
| B05 | UniDexFPM | Functional pre-grasp goal과 RL expert·diffusion policy의 연결 |
| B18 | Differentiable Force Closure Estimator | Differentiable force-closure surrogate의 물리 가정과 한계 |
| B19 | DexGraspNet | Distance·joint-limit·penetration energy와 physics validation 기반 pose generation |
| B20 | Get a Grip | 정적 geometry surrogate와 실제 rollout 성공 evaluator를 분리하는 방식 |

### 3.7 Policy Observation 표현과 Isaac Lab 구현 근거

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| B09 | DexTouch | Arm·hand q/dq, palm pose·velocity, fingertip relative positions, task prior와 16D binary tactile의 구성; wrist F/T ablation |
| B12 | Visuotactile Estimation and Control | Object-pose observation의 occlusion·noise, EEF pose·wrench history, recurrent pose estimator와 uncertainty-conditioned policy |
| B22 | DexMove | Wrist·finger·object·contact state의 5-frame history와 marker-level normal/shear tactile field를 쓰는 고정보량 상한선 |
| B27 | VTDexManip | 224×224 RGB–ResNet 계열과 force-threshold binary tactile–MLP를 proprioception과 결합하는 구조 |
| B32 | Rotating without Seeing | Binary tactile, hand q, previous joint target, rotation axis를 4-step stack하여 MLP에 입력하는 최소 tactile policy |
| B33 | Robot Synesthesia | Depth-camera point cloud, robot mesh point cloud와 active tactile-sensor point cloud를 palm frame에서 융합하는 표현 |
| B34 | TacSL | 고충실도 visuotactile image·force field를 사용할 때 필요한 별도 sensor simulation 경로와 비용 |
| B35 | Isaac Lab | Camera, ContactSensor, FrameTransformer, joint wrench와 ObservationManager를 통한 구현 가능 범위 |
| B36 | TacEx | Vanilla ContactSensor를 넘어 GelSight 영상까지 모사할 때의 외부 Isaac Sim 확장 후보 |
| B37 | Goal-Driven Robotic Pushing | Local tactile pose와 global proprioceptive goal을 분리한 이유; binary tactile로 그대로 재현할 수 없는 정보의 경계 |
| B38 | Continuity of Rotation Representations | 5D는 6D의 마지막 네 성분을 normalized stereographic projection으로 3D에 압축한 full-SO(3) 표현이며, 6D는 두 3D vector를 Gram–Schmidt로 복원하는 더 직접적인 표현이다. 원 논문은 point-cloud regression에서 5D의 gradient distortion 가능성을 지적하므로 현재 baseline은 6D, 5D는 ablation 근거로 사용 |
| B39 | General In-Hand Object Re-Orientation | 임의 goal orientation에는 quaternion difference를, symmetric-object vision 평가에는 shape-equivalent criterion을 둔 사례 |
| B40 | RotateIt | 연속 회전 task에서 hand-centric 3D rotation-axis vector를 observation에 추가한 이유 |

---

## 4. 현재 우선 독해 순서 — Observation formulation

현재 Track B 1단계의 **coarse OBB + arm/hand q + binary tactile + wrist F/T + MLP history** observation을 구체화하기 위한 순서다. 최종 실험 baseline 선정은 아니다. Point cloud·raw RGB·optical tactile는 현재 actor 최소안이 아니라 비교 배경 또는 후속 확장으로 읽는다.

1. **B09 DexTouch → B32 Rotating without Seeing:** Binary tactile와 spatial coverage의 이유를 B09에서, previous controller target과 finite history의 이유를 B32에서 확인
2. **B10 Tactile Pushing → B37 Goal-Driven Robotic Pushing:** Pusher/EEF-relative goal과 local contact state를 분리하는 이유, raw tactile image와 contact-pose feature의 차이
3. **B35 Isaac Lab:** URDF link/pad ContactSensor, joint wrench와 ObservationManager history의 실제 구현 경계
4. **B12 Visuotactile Estimation and Control:** Continuous vision의 occlusion·latency와 force history, validity·uncertainty를 처리하는 이유
5. **B27 VTDexManip:** Image–ResNet과 binary tactile–MLP를 분리한 근거 및 raw image를 현재안에서 제외할 근거
6. **B33 Robot Synesthesia:** Spatial tactile/point-cloud 표현이 coarse binary보다 주는 정보와 추가 구현 비용의 비교 배경
7. **B15 RoboPack → B26 DyWA:** Action–contact history로 관측되지 않는 물성·동역학을 추론하는 이유와 recurrent model의 역할
8. **B22 DexMove → B34 TacSL → B36 TacEx:** 고차원 tactile·optical tactile가 제공할 수 있는 상한과 현재 ContactSensor baseline의 차이
9. **B39 General In-Hand Re-Orientation → B40 RotateIt → B38 Rotation Representation:** Arbitrary orientation goal, continuous-axis rotation goal과 neural-network용 SO(3) 표현을 구분

현재 tactile 관련 직접 ablation은 `F/T only`, `URDF coarse M-region binary + F/T`, `17-channel binary + F/T`다. 논문의 history 길이와 force threshold는 출발 근거일 뿐 그대로 복사하지 않고 실제 policy rate·sensor latency·hardware calibration으로 정한다.

기존 GD2P·Hermans et al.·TaskDexGrasp 중심의 baseline 독해는 폐기하지 않으며, observation 명세 후 reward와 접촉 구성 설계를 진행할 때 이어간다.

---

## 5. ICRA·IROS 2021–2025 RL 검색 결과 전수 Screening — 학회별 별도 목록

### 5.1 검토 범위와 선정 기준

`docs/ICRA&IROS`의 10개 CSV에 수록된 **2,050편 전부**를 제목·초록·Author Keywords·IEEE Terms 기준으로 확인했다. 모든 논문이 reinforcement learning 검색 결과에 포함되었더라도, 아래 목록에는 Track B의 현재 문제 또는 문서화된 설계 미결 항목에 구체적인 근거를 주는 논문만 남겼다.

| 학회 | 2021 | 2022 | 2023 | 2024 | 2025 | 검토 합계 | 선정 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| ICRA | 186 | 149 | 190 | 263 | 238 | 1,026 | 80 |
| IROS | 155 | 169 | 181 | 231 | 288 | 1,024 | 90 |
| **합계** | **341** | **318** | **371** | **494** | **526** | **2,050** | **170** |

선정된 170편 중 168편은 이 문서에 새로 추가한 논문이고, 2편은 기존 B03·B33과의 교차참조다. 일반 navigation·locomotion·driving·multi-agent RL 논문은 task 명칭이 비슷하다는 이유만으로 넣지 않았으며, 다음 중 하나 이상에 명확히 연결될 때만 포함했다.

- Blocker pushing, non-prehensile/dexterous contact manipulation, clutter clearing, target visibility·retrieval
- Vision·tactile·wrist F/T·proprioception의 observation, history, sensor fusion과 partial observability
- Actor–critic privileged-information 분리, contact state·force를 이용한 reward·evaluation
- Contact·tactile·vision·actuator simulation, scene generation, system identification, domain randomization과 Sim-to-Real
- EEF/action representation, force·impedance controller, reward, curriculum, phase transition과 skill chaining
- Collision·과부하·전도·failure recovery, safety supervisor와 장기 공간 확보 의사결정

등급은 `A = 현재 과업 또는 핵심 설계에 직접 대응`, `B = 학습환경·센서·제어·Sim-to-Real 등에 강한 방법론적 근거`, `C = 후속 Track B 2단계 또는 확장 설계 참고`다. 관점 태그는 `TASK`, `CONTACT`, `SENSE`, `OBS`, `SIM`, `ENV`, `SYSID`, `ACTION`, `GOAL`, `REWARD`, `LEARN`, `SAFE`, `SYSTEM`을 사용한다. `CSV ID`의 마지막 숫자는 해당 연도 CSV에서 header를 제외한 1-based row 번호이므로 원본 record를 바로 추적할 수 있다.

이 목록은 **초록 수준의 전수 relevance screening**이다. `A` 등급도 최종 baseline 선정이나 논문의 세부 claim 검증을 의미하지 않으며, 실제 설계에 인용하거나 구현하기 전에는 원문·공개 코드·센서 및 실험 조건을 다시 확인한다.

### 5.2 ICRA 선정 논문

#### ICRA 2021

| CSV ID | 등급 | 논문 | 관점 | 현재 연구에서 확인할 점 |
| --- | --- | --- | --- | --- |
| ICRA21-003 | A | [Dexterous Manoeuvre through Touch in a Cluttered Scene](https://doi.org/10.1109/ICRA48506.2021.9562061) | TASK, CONTACT, SENSE, OBS, LEARN | BioTac 신호를 autoencoder로 압축하고 RL로 clutter 내 motion sequence를 생성해 실제 iiwa에서 검증했으므로, tactile history 기반 shelf-contact 폐루프 action 선택의 직접 기준이다. |
| ICRA21-011 | B | [Observation Space Matters: Benchmark and Optimization Algorithm](https://doi.org/10.1109/ICRA48506.2021.9561019) | OBS, SENSE, LEARN | Cartesian transform, binary contact flag, short history, global position을 비교하고 불필요 채널을 제거했으므로, 현재 observation contract와 tactile/history ablation의 직접 근거다. |
| ICRA21-022 | B | [Multi-Modal Mutual Information (MuMMI) Training for Robust Self-Supervised Deep Reinforcement Learning](https://doi.org/10.1109/ICRA48506.2021.9561187) | SENSE, OBS, LEARN | modality 간 latent consistency를 학습해 missing observation에 강한 world model을 만들었으므로, vision dropout과 tactile·F/T 결손에 대한 강건성 설계에 참고한다. |
| ICRA21-027 | A | [Learning Visual Affordances with Target-Orientated Deep Q-Network to Grasp Objects by Harnessing Environmental Fixtures](https://doi.org/10.1109/ICRA48506.2021.9561737) | TASK, CONTACT, SENSE, SIM, ACTION | 벽 같은 fixture를 이용한 Slide-to-Wall 행동을 visual Q-map으로 학습해 sim-to-real 검증했으므로, shelf를 extrinsic contact로 활용하는 회전·pre-contact 전략과 직접 연결된다. |
| ICRA21-068 | A | [Learning Collaborative Pushing and Grasping Policies in Dense Clutter](https://doi.org/10.1109/ICRA48506.2021.9561828) | TASK, CONTACT, SENSE, ACTION, LEARN | 3D observation에서 planar push와 6-DoF grasp를 self-supervised Q-learning으로 공동 학습했으므로, blocker push가 후속 target graspability를 높이는지를 평가할 baseline이다. |
| ICRA21-072 | C | [Learning Multimodal Contact-Rich Skills from Demonstrations Without Reward Engineering](https://doi.org/10.1109/ICRA48506.2021.9561734) | CONTACT, SENSE, OBS, LEARN | contact-rich demonstration을 위한 multimodal sensor representation을 실제 Sawyer에서 검증했으므로, RL 외부의 sensor-fusion 및 reward-free contact-skill 비교군으로 유용하다. |
| ICRA21-076 | B | [Detect, Reject, Correct: Crossmodal Compensation of Corrupted Sensors](https://doi.org/10.1109/ICRA48506.2021.9561847) | SENSE, OBS, SAFE, LEARN | self-supervised reconstruction으로 손상된 modality를 탐지·제외하고 나머지 센서로 보상했으므로, vision latency/dropout과 tactile·F/T corruption 처리에 적용 가능하다. |
| ICRA21-084 | A | [Learning Dense Rewards for Contact-Rich Manipulation Tasks](https://doi.org/10.1109/ICRA48506.2021.9561891) | CONTACT, SENSE, REWARD, LEARN | image와 tactile feedback에서 self-supervised task progress와 dense reward를 만들었으므로, Approach–Rotation–Push progress reward 및 phase gate와 비교할 수 있다. |
| ICRA21-089 | B | [SimGAN: Hybrid Simulator Identification for Domain Adaptation via Adversarial Reinforcement Learning](https://doi.org/10.1109/ICRA48506.2021.9561731) | SIM, SYSID, LEARN | neural component와 physics simulator를 결합해 실제 trajectory에 맞는 hybrid simulator를 식별했으므로, friction·contact dynamics를 real rollout으로 보정하는 SYSID 후보다. |
| ICRA21-104 | A | [Tactile-RL for Insertion: Generalization to Objects of Unknown Geometry](https://doi.org/10.1109/ICRA48506.2021.9561646) | CONTACT, SENSE, OBS, REWARD, LEARN | attempt–pose-correction 정책에서 RL/지도학습, curriculum, F/T/tactile, RGB/flow를 비교했으므로, 현재 F/T-only 대 tactile+F/T 및 geometry-generalization ablation의 직접 근거다. |
| ICRA21-133 | A | [Proactive Action Visual Residual Reinforcement Learning for Contact-Rich Tasks Using a Torque-Controlled Robot](https://doi.org/10.1109/ICRA48506.2021.9561162) | CONTACT, SENSE, OBS, ACTION, LEARN | operational-space visual·haptic 정보와 POMDP 완화용 proactive action을 residual RL에 결합했으므로, pose vision과 F/T를 이용한 능동 contact formation 설계에 참고한다. |

#### ICRA 2022

| CSV ID | 등급 | 논문 | 관점 | 현재 연구에서 확인할 점 |
| --- | --- | --- | --- | --- |
| ICRA22-009 | A | [Interleaving Monte Carlo Tree Search and Self-Supervised Learning for Object Retrieval in Clutter](https://doi.org/10.1109/ICRA46639.2022.9812132) | TASK, ENV, ACTION, LEARN | DNN이 MCTS를 모방하고 다시 탐색을 안내하는 clutter retrieval pipeline을 제안했으므로, blocker 순서 결정과 learned low-level manipulation을 결합하는 Track B 2단계 baseline이다. |
| ICRA22-011 | C | [Learning to Pick by Digging: Data-Driven Dig-Grasping for Bin Picking from Clutter](https://doi.org/10.1109/ICRA46639.2022.9811736) | TASK, CONTACT, SENSE, ACTION, SIM | visual FCN이 clutter를 파고드는 interactive primitive별 picking 성공률을 self-supervised로 학습했으므로, 의도적 blocker contact를 통한 target singulation 사례로 참고한다. |
| ICRA22-023 | A | [Push-to-See: Learning Non-Prehensile Manipulation to Enhance Instance Segmentation via Deep Q-Learning](https://doi.org/10.1109/ICRA46639.2022.9811645) | TASK, CONTACT, SENSE, REWARD, LEARN | depth Mask R-CNN의 segmentation 개선량으로 pushing Q-policy를 학습했으므로, blocker 재배치의 목표를 target visibility 향상으로 정의할 직접 근거다. |
| ICRA22-030 | A | [Hierarchical Policy Learning for Mechanical Search](https://doi.org/10.1109/ICRA46639.2022.9811572) | TASK, ENV, ACTION, LEARN | clutter retrieval을 hierarchical POMDP로 만들고 push 및 action-selection sub-policy를 학습했으므로, 상위 blocker/action 선택과 하위 push 실행을 분리하는 구조와 매우 가깝다. |
| ICRA22-042 | C | [Discovering Synergies for Robot Manipulation with Multi-Task Reinforcement Learning](https://doi.org/10.1109/ICRA46639.2022.9812170) | ACTION, LEARN | low-dimensional hand synergy와 multi-task policy를 end-to-end로 함께 학습했으므로, RH56E2 6D action을 그대로 쓸지 synergy action으로 축소할지 판단할 근거다. |
| ICRA22-043 | A | [Learning Purely Tactile In-Hand Manipulation with a Torque-Controlled Hand](https://doi.org/10.1109/ICRA46639.2022.9812093) | CONTACT, SENSE, SIM, SYSID, REWARD, LEARN | position·torque sensing만으로 cube rotation을 identified simulation과 domain-adapted curriculum에서 학습해 실제 손에 전이했으므로, contact 유지 reward와 sim-to-real setup의 핵심 사례다. |
| ICRA22-044 | A | [On the Feasibility of Learning Finger-gaiting In-hand Manipulation with Intrinsic Sensing](https://doi.org/10.1109/ICRA46639.2022.9812212) | CONTACT, SENSE, OBS, ENV, LEARN | proprioceptive·tactile sensing만으로 축 회전 finger-gaiting을 학습하고 initial-state distribution으로 탐색을 개선했으므로, rotation contact switching과 초기 접촉 curriculum에 활용한다. |
| ICRA22-087 | A | [A Hybrid Approach for Learning to Shift and Grasp with Elaborate Motion Primitives](https://doi.org/10.1109/ICRA46639.2022.9811735) | TASK, CONTACT, ACTION, LEARN | hybrid discrete-continuous SAC와 parameterized pushing/grasping primitive로 clutter의 shift-and-grasp를 학습했으므로, primitive 종류와 연속 이동 parameter를 함께 고르는 action formulation에 직접 대응한다. |
| ICRA22-103 | B | [Graph-based Cluttered Scene Generation and Interactive Exploration using Deep Reinforcement Learning](https://doi.org/10.1109/ICRA46639.2022.9811874) | ENV, SIM, TASK, LEARN | scene grammar와 GNN-RL generator로 물리적으로 안정적인 shelf형 clutter를 만들었으므로, stable blocker arrangement·hidden-target difficulty·curriculum 자동 생성에 바로 활용할 수 있다. |
| ICRA22-104 | B | [Validate on Sim, Detect on Real - Model Selection for Domain Randomization](https://doi.org/10.1109/ICRA46639.2022.9811621) | SIM, SENSE, LEARN | simulation 평가와 고정 real-data OOD score를 결합해 DR policy를 실제 rollout 없이 순위화했으므로, sensor/contact randomization 설정과 checkpoint 선택 절차에 참고한다. |
| ICRA22-113 | B | [Learning Multi-step Robotic Manipulation Policies from Visual Observation of Scene and Q-value Predictions of Previous Action](https://doi.org/10.1109/ICRA46639.2022.9812251) | OBS, ACTION, REWARD, LEARN | visual scene과 previous-action Q prediction을 조건으로 쓰고 Task-Progress Gaussian reward를 설계했으므로, previous-action history와 phase-progress reward의 직접 비교 근거다. |
| ICRA22-115 | A | [Visuotactile-RL: Learning Multimodal Manipulation Policies with Deep Reinforcement Learning](https://doi.org/10.1109/ICRA46639.2022.9812019) | CONTACT, SENSE, OBS, SIM, LEARN | pixel vision·touch를 tactile gating, tactile augmentation, visual degradation과 함께 학습했으므로, continuous vision–tactile fusion과 modality corruption randomization의 핵심 사례다. |
| ICRA22-130 | A | [Semi-Autonomous Teleoperation via Learning Non-Prehensile Manipulation Skills](https://doi.org/10.1109/ICRA46639.2022.9811823) | TASK, CONTACT, SENSE, SIM, ACTION, REWARD | depth와 goal-object location으로 clutter 재배치용 nonprehensile trajectory option을 학습해 real에 전이했으므로, target 접근을 위한 blocker relocation formulation과 거의 동일하다. |
| ICRA22-133 | B | [Learning Insertion Primitives with Discrete-Continuous Hybrid Action Space for Robotic Assembly Tasks](https://doi.org/10.1109/ICRA46639.2022.9811973) | CONTACT, ACTION, REWARD, LEARN, SIM | exit condition을 가진 primitive type과 연속 parameter를 hybrid action으로 학습했으므로, Approach/Rotation/Push phase와 연속 EEF delta를 결합하는 대안 action contract다. |
| ICRA22-134 | B | [Active Extrinsic Contact Sensing: Application to General Peg-in-Hole Insertion](https://doi.org/10.1109/ICRA46639.2022.9812017) | CONTACT, SENSE, OBS, SIM, LEARN | active tactile controller로 contact mode를 유지하며 external contact line을 추정하고 contact-space policy를 simulation에서 학습했으므로, privileged contact state를 deployable coarse observation으로 축약하는 설계에 참고한다. |

#### ICRA 2023

| CSV ID | 등급 | 논문 | 관점 | 현재 연구에서 확인할 점 |
| --- | --- | --- | --- | --- |
| ICRA23-030 | A | [Learning Pre-Grasp Manipulation of Flat Objects in Cluttered Environments using Sliding Primitives](https://doi.org/10.1109/ICRA48891.2023.10160869) | TASK, CONTACT, ACTION, ENV, REWARD, LEARN | sliding parameter와 조작할 물체를 각각 policy/Q-network로 선택하고 curriculum으로 clutter를 확장했으므로, blocker selection·slide parameter·scene difficulty를 함께 설계할 수 있다. |
| ICRA23-032 | A | [Dextrous Tactile In-Hand Manipulation Using a Modular Reinforcement Learning Architecture](https://doi.org/10.1109/ICRA48891.2023.10160756) | TASK, CONTACT, SENSE, OBS, SIM, LEARN | 0.5초 observation window의 policy와 particle-filter object-state estimator를 분리해 goal orientation을 zero-shot 전이했으므로, tactile history·pose estimator·goal observation 설계의 직접 사례다. |
| ICRA23-061 | C | [A Multi-Agent Approach for Adaptive Finger Cooperation in Learning-based In-Hand Manipulation](https://doi.org/10.1109/ICRA48891.2023.10160909) | CONTACT, SENSE, OBS, LEARN | finger별 local-observation actor와 global-observation critic을 사용했으므로, 전체 object/contact state를 privileged critic에만 주는 asymmetric actor–critic 구조의 참고 사례다. |
| ICRA23-064 | A | [Toward Fine Contact Interactions: Learning to Control Normal Contact Force with Limited Information](https://doi.org/10.1109/ICRA48891.2023.10161224) | CONTACT, SENSE, ACTION, SAFE, LEARN | 저가의 정보량이 적은 tactile sensor로 normal-contact-force controller를 학습해 motion controller와 결합했으므로, coarse tactile/F/T로 접촉력을 유지할 수 있는지 보는 low-level baseline이다. |
| ICRA23-069 | A | [Demonstration-guided Optimal Control for Long-term Non-prehensile Planar Manipulation](https://doi.org/10.1109/ICRA48891.2023.10161496) | TASK, CONTACT, ACTION, SYSTEM | contact point·mode·separation·face switching을 포함한 pusher-slider TAMP를 demonstration으로 warm-start했으므로, Rotation–Push 사이 이산 contact-mode 전환의 model-based 비교 대상이다. |
| ICRA23-102 | A | [Seq2Seq Imitation Learning for Tactile Feedback-based Manipulation](https://doi.org/10.1109/ICRA48891.2023.10161145) | CONTACT, SENSE, OBS, ACTION, LEARN | interaction sequence로 partially observable state를 추정한 뒤 control sequence를 생성했으므로, tactile/F/T finite history와 recurrent estimator 필요성을 평가할 비-RL baseline이다. |
| ICRA23-103 | A | [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) | TASK, CONTACT, SENSE, SIM, ACTION, LEARN | unit object의 pivoting policy를 depth-derived object feature로 state/action projection해 novel object에 zero-shot 전이했으므로, geometry-conditioned blocker rotation의 직접 baseline이다. |
| ICRA23-104 | A | [RLAfford: End-to-End Affordance Learning for Robotic Manipulation](https://doi.org/10.1109/ICRA48891.2023.10161571) | CONTACT, SENSE, OBS, LEARN | RL rollout의 contact information으로 task 공통 contact map을 학습했으므로, privileged contact pair·force를 critic/representation 학습에만 활용하는 실험과 연결된다. |
| ICRA23-108 | A | [Dexterous Manipulation from Images: Autonomous Real-World RL via Substep Guidance](https://doi.org/10.1109/ICRA48891.2023.10161493) | TASK, SENSE, REWARD, LEARN, SYSTEM | final task와 중간 subtask를 image example로 정의해 다지 손이 실제 환경에서 reward engineering 없이 자율 연습했으므로, Approach–Rotation–Push substep supervision 및 curriculum 사례다. |
| ICRA23-111 | A | [DeXtreme: Transfer of Agile In-hand Manipulation from Simulation to Reality](https://doi.org/10.1109/ICRA48891.2023.10160216) | CONTACT, SENSE, OBS, SIM, ENV, SYSTEM | Isaac Gym에서 넓은 randomization으로 Allegro policy와 real-time vision pose estimator를 개발했으므로, dexterous sim-to-real과 privileged-state 대 vision-policy 비교의 핵심 자료다. |
| ICRA23-134 | B | [STAP: Sequencing Task-Agnostic Policies](https://doi.org/10.1109/ICRA48891.2023.10160220) | ACTION, REWARD, LEARN, SYSTEM | skill Q-value를 feasibility로 해석하고 sequence 전체 Q-value의 곱을 최대화했으므로, Rotation 종료 상태가 Push policy에 적합한지 평가하는 transition critic 후보다. |
| ICRA23-135 | A | [SDF-Based Graph Convolutional Q-Networks for Rearrangement of Multiple Objects](https://doi.org/10.1109/ICRA48891.2023.10161394) | TASK, CONTACT, OBS, ACTION, LEARN, ENV | 물체별 SDF로 구성한 permutation-invariant scene graph를 state-goal representation으로 사용했으므로, multi-blocker geometry와 상위 rearrangement Q-function의 강한 비교 대상이다. |
| ICRA23-141 | C | [NeRF2Real: Sim2real Transfer of Vision-guided Bipedal Motion Skills using Neural Radiance Fields](https://doi.org/10.1109/ICRA48891.2023.10161544) | SENSE, SIM, ENV, SYSTEM | 휴대전화 영상에서 NeRF appearance와 static contact geometry를 복원해 physics simulator에 합성했으므로, 실제 shelf visual appearance와 collision geometry를 함께 가상화하는 방법으로 참고한다. |
| ICRA23-168 | A | [Safe Self-Supervised Learning in Real of Visuo-Tactile Feedback Policies for Industrial Insertion](https://doi.org/10.1109/ICRA48891.2023.10160763) | CONTACT, SENSE, OBS, SAFE, LEARN, SYSTEM | tactile align과 vision insert의 두 phase 및 F/T 기반 안전 data collection을 사용했으므로, phase별 sensor 역할·F/T threshold·collision-limited training setup의 직접 근거다. |

#### ICRA 2024

| CSV ID | 등급 | 논문 | 관점 | 현재 연구에서 확인할 점 |
| --- | --- | --- | --- | --- |
| ICRA24-042 | A | [Dual-Critic Deep Reinforcement Learning for Push-Grasping Synergy in Cluttered Environment](https://doi.org/10.1109/ICRA57147.2024.10610121) | TASK, ACTION, REWARD | 시각 상태의 action-selection critic과 state-action 성공 critic, push용 double-step reward를 제안한다. Blocker push의 유효성 평가와 불필요한 push 억제에 참고한다. |
| ICRA24-073 | A | [Learning Extrinsic Dexterity with Parameterized Manipulation Primitives](https://doi.org/10.1109/ICRA57147.2024.10611431) | TASK, CONTACT, ACTION, LEARN | 환경 접촉을 이용하는 parameterized primitive들을 hierarchical RL로 연결해 grasp 불가능 물체를 재배치한다. Shelf 접촉, 준비 회전-병진 연결과 후속 retrieval 조건 형성에 참고한다. |
| ICRA24-075 | B | [Contact Energy Based Hindsight Experience Prioritization](https://doi.org/10.1109/ICRA57147.2024.10610910) | CONTACT, SENSE, REWARD, LEARN | Tactile contact와 object displacement의 contact energy로 sparse multi-goal HER sample을 우선 추출한다. Informative contact rollout 우선학습과 rotation/push sparse goal 학습에 참고한다. |
| ICRA24-080 | B | [Augmenting Tactile Simulators with Real-like and Zero-Shot Capabilities](https://doi.org/10.1109/ICRA57147.2024.10610442) | SENSE, SIM | SightGAN이 simulated/real tactile difference image를 양방향 변환하면서 contact 위치와 force 정보를 보존한다. 고해상도 tactile 가상화와 sim-to-real sensor 변환에 참고한다. |
| ICRA24-087 | A | [Curriculum-based Sensing Reduction in Simulation to Real-World Transfer for In-hand Manipulation](https://doi.org/10.1109/ICRA57147.2024.10610328) | OBS, SIM, LEARN | Actor가 critic과 같은 풍부한 simulation feature에서 시작해 배포 불가능한 feature를 단계적으로 제거한다. Privileged critic-deployable actor 정보 경계와 sensor-removal curriculum에 참고한다. |
| ICRA24-088 | B | [Geometric Fabrics: a Safe Guiding Medium for Policy Learning](https://doi.org/10.1109/ICRA57147.2024.10610235) | ACTION, SAFE, LEARN | OSC나 joint-PD target 대신 nonlinear behavioral dynamics를 RL action interface로 사용해 안전성과 sequencing을 개선한다. DiffIK/OSC 대안과 low-level action safety에 참고한다. |
| ICRA24-089 | A | [Dexterous In-hand Manipulation by Guiding Exploration with Simple Sub-skill Controllers](https://doi.org/10.1109/ICRA57147.2024.10611300) | CONTACT, ACTION, LEARN | 단순 sub-skill controller로 RL exploration을 관련 state space에 유도해 finger-gaiting 학습을 가속한다. Approach-contact-rotation-push의 phase별 controller prior에 참고한다. |
| ICRA24-090 | 기존 | [Robot Synesthesia: In-Hand Manipulation with Visuotactile Sensing](https://doi.org/10.1109/ICRA57147.2024.10610532) | SENSE, OBS, SIM | B33 교차참조. Visual/tactile point cloud 융합과 sensor ablation을 제시하므로 spatial tactile 표현과 coarse binary tactile 비교에 참고한다. |
| ICRA24-098 | B | [Universal Visual Decomposer: Long-Horizon Manipulation Made Easy](https://doi.org/10.1109/ICRA57147.2024.10611125) | OBS, REWARD, LEARN | Visual demonstration embedding의 phase shift에서 subgoal을 추출해 goal-conditioned policy와 reward shaping에 쓴다. Phase ID 없이 intermediate goal과 reward gate를 구성하는 데 참고한다. |
| ICRA24-110 | A | [Touch-Based Manipulation with Multi-Fingered Robot using Off-policy RL and Temporal Contrastive Learning](https://doi.org/10.1109/ICRA57147.2024.10610239) | CONTACT, SENSE, OBS, LEARN | Contact/non-contact 전환의 partial observability를 observation/action history의 temporal contrastive latent와 off-policy RL로 처리한다. Binary tactile, F/T와 previous-action history 설계에 참고한다. |
| ICRA24-112 | B | [MoDem-V2: Visuo-Motor World Models for Real-World Robot Manipulation](https://doi.org/10.1109/ICRA57147.2024.10611121) | CONTACT, ENV, LEARN, SAFE | Demonstration bootstrapping, exploration centering, agency handover와 actor-critic ensemble로 contact-rich visual MBRL을 실물에서 학습한다. 안전한 exploration과 real-world refinement 절차에 참고한다. |
| ICRA24-123 | A | [TEXterity: Tactile Extrinsic deXterity](https://doi.org/10.1109/ICRA57147.2024.10610622) | CONTACT, SENSE, OBS, ACTION | Kinematics와 image tactile로 coarse object pose를 추적하고 continuous estimator-controller와 receding-horizon planning으로 자세를 제어한다. 가림 시 tactile pose 보정과 rotation closed loop에 참고한다. |
| ICRA24-135 | B | [TWIST: Teacher-Student World Model Distillation for Efficient Sim-to-Real Transfer](https://doi.org/10.1109/ICRA57147.2024.10610450) | OBS, SIM, LEARN | Privileged state teacher world model의 latent dynamics를 domain-randomized image student에 distill한다. Simulation GT를 teacher/critic에만 주고 deployable vision actor로 이전하는 데 참고한다. |
| ICRA24-140 | A | [Generalize by Touching: Tactile Ensemble Skill Transfer for Robotic Furniture Assembly](https://doi.org/10.1109/ICRA57147.2024.10610567) | CONTACT, SENSE, ACTION, LEARN | Offline RL로 tactile ensemble intra-skill policy와 skill-transition model을 학습한다. Tactile 기반 phase 종료 판정과 rotation-push 전환에 참고한다. |
| ICRA24-141 | A | [Sim2Real Manipulation on Unknown Objects with Tactile-based Reinforcement Learning](https://doi.org/10.1109/ICRA57147.2024.10611113) | SENSE, OBS, SIM | 다양한 물체의 visual-tactile simulation RL을 학습하고 tactile representation별 real transfer를 비교한다. F/T-only, coarse binary와 full tactile ablation에 참고한다. |
| ICRA24-144 | B | [Symmetry-aware Reinforcement Learning for Robotic Assembly under Partial Observability with a Soft Wrist](https://doi.org/10.1109/ICRA57147.2024.10610103) | CONTACT, SENSE, OBS, SAFE | Soft wrist에서 haptic/proprioception memory policy에 symmetry augmentation과 auxiliary loss를 적용한다. F/T history, partial observability와 symmetry-aware orientation 평가에 참고한다. |
| ICRA24-169 | A | [Unknown Object Retrieval in Confined Space through Reinforcement Learning with Tactile Exploration](https://doi.org/10.1109/ICRA57147.2024.10611541) | TASK, CONTACT, SENSE, LEARN | Multi-contact tactile tool, hybrid-action RL, representative-shape training과 terminal-goal curriculum으로 좁은 공간의 미지 물체를 인출한다. Shelf retrieval, tactile exploration과 goal curriculum에 직접 참고한다. |
| ICRA24-195 | A | [See to Touch: Learning Tactile Dexterity through Visual Incentives](https://doi.org/10.1109/ICRA57147.2024.10611407) | SENSE, OBS, REWARD, LEARN | 한 human demonstration에서 visual optimal-transport reward를 만들고 tactile actor를 online RL로 최적화한다. Vision을 privileged reward로 쓰는 설계와 tactile actor ablation에 참고한다. |
| ICRA24-196 | A | [Self-supervised Learning for Joint Pushing and Grasping Policies in Highly Cluttered Environments](https://doi.org/10.1109/ICRA57147.2024.10611650) | TASK, CONTACT, ACTION, LEARN | Target grasp를 방해하는 물체를 밀도록 pushing과 grasping의 dual RL policy를 학습한다. Blocker 재배치로 target 접근 공간을 만드는 task와 clutter 평가에 직접 참고한다. |
| ICRA24-197 | A | [Harnessing the Synergy between Pushing, Grasping, and Throwing to Enhance Object Manipulation in Cluttered Scenarios](https://doi.org/10.1109/ICRA57147.2024.10610548) | TASK, CONTACT, ACTION, SYSTEM | 매 step target pose에서 target을 고립시키는 push configuration과 후속 grasp/throw parameter를 학습한다. 공간 확보용 contact/action 선택과 retrieval 연결에 참고한다. |
| ICRA24-198 | B | [Masked Visual-Tactile Pre-training for Robot Manipulation](https://doi.org/10.1109/ICRA57147.2024.10610933) | SENSE, OBS, LEARN | 20개 sparse binary tactile signal과 visual token을 attention/masking으로 융합해 pretrain한다. Binary tactile encoding, modality fusion과 channel masking ablation에 참고한다. |
| ICRA24-220 | A | [Learning Force Control for Legged Manipulation](https://doi.org/10.1109/ICRA57147.2024.10611066) | CONTACT, SENSE, ACTION, REWARD | Desired contact-force level을 명시적으로 추종하는 RL task specification과 compliant controller를 제안한다. Wrist F/T observation, force-tracking reward와 overload penalty에 참고한다. |
| ICRA24-228 | A | [1 kHz Behavior Tree for Self-adaptable Tactile Insertion](https://doi.org/10.1109/ICRA57147.2024.10610835) | CONTACT, SENSE, ACTION, SYSTEM | 고주파 tactile contact-state estimation에 따라 force-domain primitive를 behavior tree로 전환한다. Contact onset/loss, phase switching과 sensor-policy-control 주기 분리에 참고한다. |
| ICRA24-246 | B | [SERL: A Software Suite for Sample-Efficient Robotic Reinforcement Learning](https://doi.org/10.1109/ICRA57147.2024.10610040) | ENV, ACTION, REWARD, SYSTEM | Off-policy RL, reward 계산, environment reset, robot controller와 contact-rich example task를 하나의 실물 RL suite로 제공한다. Environment contract, reset/success와 controller integration에 참고한다. |

#### ICRA 2025

| CSV ID | 등급 | 논문 | 관점 | 현재 연구에서 확인할 점 |
| --- | --- | --- | --- | --- |
| ICRA25-001 | B | [RL-GSBridge: 3D Gaussian Splatting Based Real2Sim2Real Method for Robotic Manipulation Learning](https://doi.org/10.1109/ICRA55743.2025.11128103) | SENSE, SIM, ENV | Mesh-bound 3D Gaussian Splatting과 physics-simulator 상태를 동기화해 vision RL용 real-to-sim-to-real scene을 만든다. 실제 shelf의 visual sensor 가상화와 geometry-rendering 동기화에 참고한다. |
| ICRA25-010 | A | [Hierarchical Visual Policy Learning for Long-Horizon Robot Manipulation in Densely Cluttered Scenes](https://doi.org/10.1109/ICRA55743.2025.11128752) | TASK, ACTION, LEARN | High-level policy가 push/pick/place primitive를 선택하고 push option과 상위 정책을 HRL로 학습한다. Blocker 재배치와 retrieval primitive의 계층 분리에 참고한다. |
| ICRA25-027 | A | [Integrating Model-Based Control and RL for Sim2Real Transfer of Tight Insertion Policies](https://doi.org/10.1109/ICRA55743.2025.11128860) | CONTACT, ACTION, SIM, LEARN | Full-state potential-field controller와 sparse-reward residual RL을 결합하고 observation noise와 action magnitude curriculum을 사용한다. DiffIK/OSC nominal controller, pose noise와 action-scale curriculum에 참고한다. |
| ICRA25-052 | B | [Privileged-Dreamer: Explicit Imagination of Privileged Information for Rapid Adaptation of Learned Policies](https://doi.org/10.1109/ICRA55743.2025.11127672) | OBS, SYSID, LEARN | Dual recurrent model이 짧은 history에서 hidden physical parameter를 추정하고 model, actor와 critic을 그 값에 condition한다. Mass/friction/contact dynamics를 sensor-action history에서 추론하는 데 참고한다. |
| ICRA25-081 | A | [Learning In-Hand Translation Using Tactile Skin with Shear and Normal Force Sensing](https://doi.org/10.1109/ICRA55743.2025.11127974) | CONTACT, SENSE, SIM | Ternary shear와 binary normal force를 생성하는 tactile-skin sensor model로 sliding-contact RL의 zero-shot transfer를 구현한다. Shear 방향 가상화와 tactile channel ablation에 참고한다. |
| ICRA25-094 | A | [DemoStart: Demonstration-Led Auto-Curriculum Applied to Sim-to-Real with Multi-Fingered Robots](https://doi.org/10.1109/ICRA55743.2025.11127813) | OBS, SIM, REWARD, LEARN | 소수 simulation demonstration과 sparse reward로 auto-curriculum을 만들고 domain randomization으로 pixel/proprioception 정책을 이전한다. Initial-state curriculum, sparse success reward와 vision randomization에 참고한다. |
| ICRA25-130 | B | [High-Performance Reinforcement Learning on Spot: Optimizing Simulation Parameters with Distributional Measures](https://doi.org/10.1109/ICRA55743.2025.11128575) | SIM, SYSID, SYSTEM | Hardware/simulation trajectory 분포 차이를 Wasserstein distance와 MMD로 측정하고 CMA-ES로 simulation parameter를 최적화한다. Real q/action/F/T log와 simulator를 비교해 물리 parameter를 보정하는 데 참고한다. |
| ICRA25-135 | B | [Stage-Wise Reward Shaping for Acrobatic Robots: A Constrained Multi-Objective Reinforcement Learning Approach](https://doi.org/10.1109/ICRA55743.2025.11128552) | REWARD, LEARN, SAFE | Sequential task를 stage로 나누고 stage별 reward와 cost를 constrained multi-objective RL로 최적화한다. Phase ID 없이 approach-contact-rotation-push reward gate와 safety cost를 분리하는 데 참고한다. |
| ICRA25-147 | A | [Impedance Primitive-Augmented Hierarchical Reinforcement Learning for Sequential Tasks](https://doi.org/10.1109/ICRA55743.2025.11128462) | CONTACT, ACTION, LEARN | Variable-stiffness action, adaptive stiffness controller와 affordance coupling을 갖춘 HRL로 pushing을 포함한 sequential contact task를 수행한다. Rotation-push primitive 전환과 impedance action interface에 참고한다. |
| ICRA25-165 | 기존 | [Composing Dextrous Grasping and In-Hand Manipulation via Scoring with a Reinforcement Learning Critic](https://doi.org/10.1109/ICRA55743.2025.11127792) | CONTACT, ACTION, LEARN | B03 교차참조. In-hand manipulation critic으로 초기 grasp 후보를 평가하므로 후속 rotation/push 성공 기반 contact configuration 선택에 참고한다. |
| ICRA25-166 | A | [The Role of Tactile Sensing for Learning Reach and Grasp](https://doi.org/10.1109/ICRA55743.2025.11127409) | SENSE, OBS, LEARN | Imperfect vision 아래 여러 force-tactile feature와 setup을 비교하고 복잡한 tactile 입력이 학습을 어렵게 할 수 있음을 보인다. F/T-only, coarse binary와 full tactile ablation에 참고한다. |
| ICRA25-170 | A | [LEMMo-Plan: LLM-Enhanced Learning from Multi-Modal Demonstration for Planning Sequential Contact-Rich Manipulation Tasks](https://doi.org/10.1109/ICRA55743.2025.11127842) | CONTACT, SENSE, ACTION, SYSTEM | Visual demonstration에 tactile과 F/T를 순차 통합해 새로운 구성의 contact-rich task plan을 생성한다. Tactile/F/T 기반 phase와 force condition을 Track B 2단계 계획에 전달하는 데 참고한다. |
| ICRA25-175 | B | [Extended Friction Models for the Physics Simulation of Servo Actuators](https://doi.org/10.1109/ICRA55743.2025.11128640) | SIM, SYSID, ACTION | 표준 Coulomb-viscous 모델보다 정확한 servo friction model과 trajectory 기반 parameter identification을 제안한다. UR5e/RH56E2 joint dynamics와 action sim-to-real mismatch 모델링에 참고한다. |
| ICRA25-189 | A | [Dynamic Object Goal Pushing with Mobile Manipulators Through Model-Free Constrained Reinforcement Learning](https://doi.org/10.1109/ICRA55743.2025.11128166) | TASK, CONTACT, OBS, SAFE | Object pose만 관측하며 미지 물체를 목표 position과 yaw로 옮기는 constrained RL을 학습하고 toppling avoidance를 검증한다. Goal-conditioned rotation-translation, pose-only vision과 toppling constraint에 직접 참고한다. |
| ICRA25-199 | A | [DROP: Dexterous Reorientation via Online Planning](https://doi.org/10.1109/ICRA55743.2025.11128433) | CONTACT, ACTION, SIM | Vision pose estimator와 parallel simulation 기반 sampling predictive controller로 contact-rich action을 online search한다. Learned rotation policy와 online planning baseline 비교에 참고한다. |
| ICRA25-201 | A | [Reinforcement Learning with Lie Group Orientations for Robotics](https://doi.org/10.1109/ICRA55743.2025.11128743) | OBS, ACTION, LEARN | Orientation의 Lie-group structure를 보존하도록 network input/output을 수정해 EEF orientation control 성능을 높인다. EEF-relative orientation과 rotation action representation ablation에 참고한다. |

### 5.3 IROS 선정 논문

> 등급: **A** = Direct/Core, **B** = Strong method/setup. 관점 태그는 Track B의 검토 축으로 제한했다.

| CSV ID | 등급 | 논문 | 관점 | 현재 연구에서 확인할 점 |
|---|:---:|---|---|---|
| **IROS 2021 (10편)** |  |  |  |  |
| IROS21-003 | A | [COCOI: Contact-aware Online Context Inference for Generalizable Non-planar Pushing](https://doi.org/10.1109/IROS51168.2021.9636836) | `TASK` `CONTACT` `SENSE` `SYSID` `SIM` | 단안 영상과 손목 F/T를 쓰고 접촉 상호작용에서 동역학 컨텍스트를 온라인 추론해 비평면 밀기의 sim-to-real을 보였다; Blocker 회전·밀기에서 F/T history와 마찰·질량 변화 적응 입력을 확인한다. |
| IROS21-036 | B | [A Learning Approach to Robot-Agnostic Force-Guided High Precision Assembly](https://doi.org/10.1109/IROS51168.2021.9636328) | `CONTACT` `SENSE` `OBS` `ENV` `LEARN` | task-space F/T만 관측하는 robotless 환경과 recurrent distributed DDPG로 하나의 정책을 세 로봇에 적용했다; 로봇 독립형 EEF 환경, F/T 시계열 관측, 정책 이식 구조를 참고한다. |
| IROS21-040 | B | [Residual Feedback Learning for Contact-Rich Manipulation Tasks with Uncertainty](https://doi.org/10.1109/IROS51168.2021.9636176) | `CONTACT` `ACTION` `LEARN` `SYSTEM` | RL residual이 Cartesian impedance controller의 출력뿐 아니라 feedback 신호도 수정하고 위치·자세 불확실성을 키우는 adaptive curriculum을 썼다; 저수준 제어기와 action contract, 접촉 난이도 curriculum의 대안을 확인한다. |
| IROS21-066 | B | [Trajectory-based Split Hindsight Reverse Curriculum Learning](https://doi.org/10.1109/IROS51168.2021.9636842) | `TASK` `ENV` `REWARD` `LEARN` | 목표 근처에서 시작해 상태공간을 넓히고 궤적을 현재 subspace 크기로 분할해 hindsight 성공 경험으로 바꾸어 sparse-reward goal grasping을 학습했다; Approach→Rotation→Push의 reset 분포와 단계별 reverse curriculum/HER 적용 가능성을 확인한다. |
| IROS21-081 | A | [Occlusion-Aware Search for Object Retrieval in Clutter](https://doi.org/10.1109/IROS51168.2021.9636230) | `TASK` `OBS` `SIM` `LEARN` `SYSTEM` | 가려진 표적의 위치 분포와 RL heuristic을 결합한 closed-loop hybrid planner가 cluttered shelf에서 sim-only 학습 후 실제 검색·회수를 수행했다; 최종 shelf retrieval 평가, occlusion belief, blocker action 선택 기준의 직접 비교군이다. |
| IROS21-093 | B | [Learning When to Switch: Composing Controllers to Traverse a Sequence of Terrain Artifacts](https://doi.org/10.1109/IROS51168.2021.9636233) | `TASK` `ACTION` `LEARN` `SYSTEM` | 개별 정책 사이의 겹치는 상태를 curriculum으로 만들고 목적 정책으로 전환 성공 가능성을 예측하는 네트워크를 학습했다; contact formation·rotation·push 정책을 나눌 경우 전환 가능 상태와 phase 연결 평가에 참고한다. |
| IROS21-102 | B | [Memory-based Deep Reinforcement Learning for POMDPs](https://doi.org/10.1109/IROS51168.2021.9636140) | `SENSE` `OBS` `LEARN` | LSTM-TD3가 누락되거나 noisy한 관측의 POMDP에서 memory 없는 DRL보다 유리함을 보였다; binary tactile와 wrist F/T의 history 길이·누락·노이즈 ablation 근거로 삼는다. |
| IROS21-110 | B | [Learning Contact-Rich Assembly Skills Using Residual Admittance Policy](https://doi.org/10.1109/IROS51168.2021.9636547) | `CONTACT` `SENSE` `ACTION` `SIM` `SYSTEM` | haptic feedback에 따라 baseline DMP를 보정하되 RL action을 admittance parameter로 제한해 불확실성 일반화와 zero-shot sim-to-real을 보였다; 접촉 단계의 compliant controller 및 residual action parametrization을 비교한다. |
| IROS21-120 | A | [Sim-to-Real Transfer for Robotic Manipulation with Tactile Sensory](https://doi.org/10.1109/IROS51168.2021.9636259) | `CONTACT` `SENSE` `OBS` `SIM` | gripper tactile array를 시뮬레이션하고 domain randomization으로 zero-shot 전이해 문 열기와 grasp 안정성을 개선했다; tactile region, 가상 센서 출력, 노이즈·DR 및 실센서 매핑 설계를 확인한다. |
| IROS21-121 | A | [Policy Learning for Visually Conditioned Tactile Manipulation](https://doi.org/10.1109/IROS51168.2021.9636866) | `SENSE` `OBS` `LEARN` `SIM` | 환경 영상에 tactile 관측을 조건화한 learned Bayes filter로 gripper belief를 추정하고 RL 정책을 실제 로봇에 전이했다; 시야 가림 때 vision–touch 융합과 belief/history 관측을 단순 concatenation·RNN과 비교한다. |
| **IROS 2022 (14편)** |  |  |  |  |
| IROS22-013 | A | [Learning Goal-Oriented Non-Prehensile Pushing in Cluttered Scenes](https://doi.org/10.1109/IROS47612.2022.9981873) | `TASK` `CONTACT` `OBS` `SAFE` `LEARN` | depth latent, EEF–object 접촉, goal distance로 주변 물체 충돌을 피하면서 목표까지 연속 접촉 밀기를 학습했다; Blocker goal pose, 접촉 유지 보상, 이웃 물체·shelf 충돌 지표의 직접 기준이다. |
| IROS22-016 | A | [Parallel Monte Carlo Tree Search with Batched Rigid-body Simulations for Speeding up Long-Horizon Episodic Robot Planning](https://doi.org/10.1109/IROS47612.2022.9981962) | `TASK` `SIM` `ENV` `SYSTEM` | GPU rigid-body simulation을 batch로 실행하는 parallel MCTS가 clutter object retrieval에서 serial MCTS보다 30배 이상 빨랐고 실제 로봇에도 적용됐다; 장기 retrieval planner, 병렬 contact rollout, 학습 정책 대비 계획 baseline을 확인한다. |
| IROS22-019 | B | [Learn from Interaction: Learning to Pick via Reinforcement Learning in Challenging Clutter](https://doi.org/10.1109/IROS47612.2022.9981530) | `TASK` `OBS` `SIM` `LEARN` | object-interaction pose를 받는 네트워크와 depth·proprioception을 받는 네트워크에 asymmetric state input을 주어 clutter picking을 sim-to-real 일반화했다; privileged pose teacher/critic과 deployable actor 관측 분리 설계에 참고한다. |
| IROS22-032 | B | [A Contact-Safe Reinforcement Learning Framework for Contact-Rich Robot Manipulation](https://doi.org/10.1109/IROS47612.2022.9981185) | `CONTACT` `ACTION` `SAFE` `SYSTEM` | 예상 밖 arm–환경 충돌을 즉시 감지해 task/joint space 접촉력을 작게 유지하면서 EEF 접촉 작업은 compliant하게 수행했다; 허용 접촉과 금지 충돌의 분리, force limit, safety layer·termination을 확인한다. |
| IROS22-040 | A | [Fixture-Aware DDQN for Generalized Environment-Enabled Grasping](https://doi.org/10.1109/IROS47612.2022.9982182) | `TASK` `CONTACT` `OBS` `LEARN` | POMDP에서 reference image로 표적을 찾고 상호작용으로 fixture를 식별해 Slide-to-Wall grasp용 visual affordance map을 만들었다; shelf·이웃 물체를 활용하는 환경 접촉 affordance와 contact probing을 참고한다. |
| IROS22-043 | B | [Robust Sim2Real Transfer with the da Vinci Research Kit: A Study On Camera, Lighting, and Physics Domain Randomization](https://doi.org/10.1109/IROS47612.2022.9981573) | `OBS` `SIM` `ENV` `SYSID` | vision 기반 cube pushing에서 camera·lighting·physics randomization을 각각 분석했고 세 축 모두 sim-to-real에 중요함을 보였다; pose/vision 센서와 마찰·동역학 DR 범위 및 요인별 ablation을 설계할 때 확인한다. |
| IROS22-047 | B | [Visual-Tactile Multimodality for Following Deformable Linear Objects Using Reinforcement Learning](https://doi.org/10.1109/IROS47612.2022.9982218) | `SENSE` `OBS` `SIM` `ENV` `LEARN` | vision·tactile·proprioception의 distilled pose를 정책 입력으로 쓰는 시뮬레이션 benchmark를 만들고 perception과 control을 분리해 실제 전이를 겨냥했다; 센서 가상화 인터페이스와 modality 조합·dropout ablation을 참고한다. |
| IROS22-063 | B | [Vision-Guided Quadrupedal Locomotion in the Wild with Multi-Modal Delay Randomization](https://doi.org/10.1109/IROS47612.2022.9981072) | `SENSE` `OBS` `SIM` `SYSID` | proprioception과 vision을 서로 다른 과거 시점에서 무작위 선택하는 MMDR로 실제 비동기 센서 지연을 모사해 zero-shot 전이를 개선했다; vision·tactile·F/T의 rate, latency, history index randomization에 직접 참고한다. |
| IROS22-090 | A | [The Role of Tactile Sensing in Learning and Deploying Grasp Refinement Algorithms](https://doi.org/10.1109/IROS47612.2022.9981915) | `CONTACT` `SENSE` `OBS` `REWARD` `SIM` | 접촉 위치·법선·힘을 조합한 simulated tactile reward가 가장 좋았고, 그렇게 학습하면 policy state의 tactile 정보는 크게 줄여도 성능 저하가 작았다; 연속 contact force/identity는 reward·critic에만 두고 actor에는 binary tactile을 주는 비대칭 설계의 핵심 근거다. |
| IROS22-118 | A | [Active Exploration for Robotic Manipulation](https://doi.org/10.1109/IROS47612.2022.9982061) | `TASK` `CONTACT` `REWARD` `LEARN` `SYSTEM` | probabilistic dynamics ensemble의 information gain과 expected reward를 MPC로 함께 최적화해 sparse-reward continuous-contact ball pushing을 실제 로봇에서 scratch 학습했다; 미지 마찰·동역학을 식별하는 probing action과 exploration bonus를 검토한다. |
| IROS22-125 | B | [Analysis of Randomization Effects on Sim2Real Transfer in Reinforcement Learning for Robotic Manipulation Tasks](https://doi.org/10.1109/IROS47612.2022.9981951) | `SIM` `ENV` `SYSID` `LEARN` | 재현 가능한 조작 benchmark에서 네 randomization 전략과 세 parameter를 비교해 randomization 확대가 전이를 돕지만 simulation 내 정책 학습은 해칠 수 있음을 보였다; 단계별 DR 범위와 nominal/fully randomized/fine-tuned 대조군을 설계한다. |
| IROS22-127 | B | [Flexible and Precision Snap-Fit Peg-in-Hole Assembly Based on Multiple Sensations and Damping Identification](https://doi.org/10.1109/IROS47612.2022.9981639) | `CONTACT` `SENSE` `OBS` `ACTION` `LEARN` | fingertip tactile·vision·force raw data를 융합해 continuous action을 내고 별도 predictor가 buckle 접촉의 damping zone을 판별해 삽입력을 조절했다; tactile/F/T 기반 contact-phase 판정, sensor fusion, 힘 조절 action을 확인한다. |
| IROS22-140 | B | [Non-blocking Asynchronous Training for Reinforcement Learning in Real-World Environments](https://doi.org/10.1109/IROS47612.2022.9981333) | `SENSE` `OBS` `ACTION` `ENV` `SYSTEM` | action·observation·transition 수집을 독립 주기로 streaming하고 측정된 delay를 학습 loop에 전달하는 non-blocking DRL 구조를 제안했다; 제어 주기와 vision/tactile/F/T 갱신률이 다른 실제 배포의 timestamp·history 정렬을 참고한다. |
| IROS22-150 | A | [Transferring Dexterous Manipulation from GPU Simulation to a Remote Real-World TriFinger](https://doi.org/10.1109/IROS47612.2022.9981458) | `TASK` `OBS` `SIM` `REWARD` `LEARN` | object-pose keypoint를 observation과 reward에 함께 쓰고 대규모 GPU 학습·DR로 임의 6-DoF 목표 pose 정책을 실제 TriFinger에 전이했다; OBB/keypoint goal representation, pose reward, 대규모 병렬 환경 구성을 확인한다. |
| **IROS 2023 (14편)** |  |  |  |  |
| IROS23-005 | B | [Contact Reduction with Bounded Stiffness for Robust Sim-to-Real Transfer of Robot Assembly](https://doi.org/10.1109/IROS55552.2023.10341866) | `CONTACT` `SIM` `ENV` `SYSTEM` | 복잡 형상의 과도한 contact point가 만드는 느린·불안정한 simulation을 bounded-stiffness contact reduction으로 개선해 tight-clearance insertion 전이를 가능하게 했다; shelf/object contact pair 수, solver 안정성, fidelity–throughput trade-off를 확인한다. |
| IROS23-018 | A | [Viewpoint Push Planning for Mapping of Unknown Confined Spaces](https://doi.org/10.1109/IROS55552.2023.10341809) | `TASK` `OBS` `ACTION` `LEARN` `SYSTEM` | shelf의 2.5D occupancy height map에서 새 viewpoint와 최소 침습 push 중 하나를 선택해 map entropy와 occlusion을 줄였다; blocker 이동을 통한 가시성 확보, push 후보 제약, retrieval 전 perception gain 보상을 참고한다. |
| IROS23-043 | A | [Learning Bifunctional Push-Grasping Synergistic Strategy for Goal-Agnostic and Goal-Oriented Tasks](https://doi.org/10.1109/IROS55552.2023.10342533) | `TASK` `OBS` `ACTION` `REWARD` `LEARN` | 영상에서 push/grasp primitive의 dense pixel-wise Q-map을 내고 hierarchical RL과 two-stage training으로 goal-oriented/agnostic clutter 작업을 통합했다; Blocker push와 target retrieval의 상·하위 정책, action 선택, 단계 학습을 비교한다. |
| IROS23-069 | A | [Learning Contact-Based State Estimation for Assembly Tasks](https://doi.org/10.1109/IROS55552.2023.10342219) | `CONTACT` `SENSE` `OBS` `SYSID` `LEARN` | contact detection과 정밀 FK를 결합하고 상태 불확실성을 줄이는 RL exploration으로 고정 물체와 in-hand pose를 추정했다; binary tactile/F/T 접촉을 blocker·shelf 상대 pose 보정과 active probing에 쓸 수 있는지 확인한다. |
| IROS23-087 | A | [Nonprehensile Planar Manipulation through Reinforcement Learning with Multimodal Categorical Exploration](https://doi.org/10.1109/IROS55552.2023.10341629) | `TASK` `CONTACT` `ACTION` `SIM` `LEARN` | sticking·sliding·separation 같은 접촉 mode를 포착하는 categorical multimodal exploration으로 임의 위치·회전 목표 pushing을 학습하고 실제 전이를 보였다; Rotation→Push의 hybrid contact mode, exploration/action 분포, pose 정확도 기준의 직접 비교군이다. |
| IROS23-089 | B | [Domains as Objectives: Multi-Domain Reinforcement Learning with Convex-Coverage Set Learning for Domain Uncertainty Awareness](https://doi.org/10.1109/IROS55552.2023.10342236) | `SIM` `SYSID` `LEARN` | convex coverage set의 universal policy가 stochastic online system identifier의 불확실성에 맞춰 sub-domain별 행동을 선택하도록 했다; 마찰·질량·센서 parameter DR가 과도하게 보수적인 정책을 만들 때 uncertainty-aware adaptation을 검토한다. |
| IROS23-119 | B | [Comparing Quadrotor Control Policies for Zero-Shot Reinforcement Learning under Uncertainty and Partial Observability](https://doi.org/10.1109/IROS55552.2023.10341941) | `SENSE` `OBS` `SIM` `SYSID` `LEARN` | state estimate 대신 raw onboard sensor를 쓸 때와 latency 등 system parameter가 불확실할 때 recurrent policy가 feed-forward보다 zero-shot 전이에 유리했다; 현재 MLP와 history encoder를 관측 노이즈·지연 조건에서 비교할 근거다. |
| IROS23-142 | B | [Value-Informed Skill Chaining for Policy Learning of Long-Horizon Tasks with Surgical Robot](https://doi.org/10.1109/IROS55552.2023.10342180) | `TASK` `REWARD` `LEARN` `SYSTEM` | 전체 후속 task의 성공 확률을 추정하는 value로 앞 subtask가 종료할 연결 상태를 고르는 skill chaining을 제안했다; contact formation 종료 상태를 Rotation·Push 성공 가능성으로 평가하는 phase transition reward를 참고한다. |
| IROS23-146 | B | [Hybrid Learning- and Model-Based Planning and Control of In-Hand Manipulation](https://doi.org/10.1109/IROS55552.2023.10342153) | `CONTACT` `ACTION` `LEARN` `SYSTEM` | 학습 정책이 grasp/contact sequence를 온라인 선택하고 model-based planner/controller가 궤적 추종과 contact force를 담당하는 계층 구조를 제안했다; 고수준 접촉 결정과 저수준 impedance/force controller의 책임 경계를 설계할 때 확인한다. |
| IROS23-158 | B | [Grasp Stability Assessment Through Attention-Guided Cross-Modality Fusion and Transfer Learning](https://doi.org/10.1109/IROS55552.2023.10342411) | `CONTACT` `SENSE` `OBS` `SIM` `LEARN` | physics simulation으로 visual–tactile dataset을 만들고 self/cross-attention fusion과 DR·domain adaptation으로 실제 grasp stability 예측에 전이했다; 가상 sensor dataset, modality attention, sim-to-real perception 보정 절차를 참고한다. |
| IROS23-170 | A | [Pre-and Post-Contact Policy Decomposition for Non-Prehensile Manipulation with Zero-Shot Sim-To-Real Transfer](https://doi.org/10.1109/IROS55552.2023.10341657) | `TASK` `CONTACT` `ACTION` `SIM` `LEARN` | 다수의 contact-mode transition과 환경 접촉이 필요한 비파지 조작을 pre/post-contact로 분해하고 action-space·curriculum을 설계해 zero-shot 전이를 보였다; 단일 phase-hidden MLP와 명시적 접촉 전후 분해의 ablation 및 curriculum을 직접 비교한다. |
| IROS23-172 | A | [Attention for Robot Touch: Tactile Saliency Prediction for Robust Sim-to-Real Tactile Control](https://doi.org/10.1109/IROS55552.2023.10341888) | `CONTACT` `SENSE` `OBS` `SIM` `LEARN` | contact-depth map, tactile saliency map, synthetic tactile-noise generator를 함께 학습해 distractor가 있는 실제 tactile control을 견고하게 했다; tactile region mask, 접촉 임계값, noise injection과 sensor virtualization 설계에 참고한다. |
| IROS23-178 | A | [Navigation Among Movable Obstacles Using Machine Learning Based Total Time Cost Optimization](https://doi.org/10.1109/IROS55552.2023.10341355) | `TASK` `ACTION` `REWARD` `LEARN` `SYSTEM` | 우회와 장애물 이동의 총 도달 시간을 비교하고, 이동하기로 한 장애물의 목표 pose는 RL generator가 정하는 NAMO pipeline을 제안했다; 어떤 blocker를 얼마나 움직일지와 downstream retrieval 비용을 포함한 목적함수를 참고한다. |
| IROS23-181 | B | [Generating Scenarios from High-Level Specifications for Object Rearrangement Tasks](https://doi.org/10.1109/IROS55552.2023.10341369) | `TASK` `ENV` `REWARD` `LEARN` | spatial-logic specification과 원하는 난이도에 조건화한 generative model로 다양하고 difficulty-controlled한 rearrangement scenario를 만들었다; shelf 배치 제약을 만족하는 reset 생성과 easy-to-hard curriculum의 자동화를 참고한다. |

#### IROS 2024

| CSV ID | 등급 | 논문 | 관점 | 현재 연구에서 확인할 점 |
| --- | --- | --- | --- | --- |
| IROS24-007 | B | [PolyFit: A Peg-in-hole Assembly Framework for Unseen Polygon Shapes via Sim-to-real Adaptation](https://doi.org/10.1109/IROS58592.2024.10802554) | SENSE·SIM | F/T 기반 접촉 pose 추정용 simulation dataset과 paired sim–real adaptation을 wrench 가상화·보정 설계 사례로 확인한다. |
| IROS24-016 | B | [Benchmarking Smoothness and Reducing High-Frequency Oscillations in Continuous Control Policies](https://doi.org/10.1109/IROS58592.2024.10802057) | ACTION·SAFE | 연속제어 policy의 고주파 진동 지표와 loss/architecture 완화법을 delta-pose·hand-action smoothness ablation에 활용한다. |
| IROS24-027 | A | [Zero-Shot Transfer of a Tactile-based Continuous Force Control Policy from Simulation to Robot](https://doi.org/10.1109/IROS58592.2024.10802386) | SENSE·SIM | 정상 접촉력을 생성하는 tactile simulation, inductive bias와 domain randomization의 zero-shot force-control 이전을 직접 참고한다. |
| IROS24-047 | A | [RTTF: Rapid Tactile Transfer Framework for Contact-Rich Manipulation Tasks](https://doi.org/10.1109/IROS58592.2024.10801764) | SENSE·SIM·LEARN | Optical-tactile sim-to-real latent alignment와 proprioception 결합 privileged learning을 센서 가상화 및 teacher–student 후보로 참고한다. |
| IROS24-049 | A | [Local Path Planning among Pushable Objects based on Reinforcement Learning](https://doi.org/10.1109/IROS58592.2024.10802257) | TASK·SYSTEM | 비축정렬 방향까지 장애물을 미는 physics-simulation RL과 실물 이전을 Track B 2단계 공간 확보 baseline으로 검토한다. |
| IROS24-067 | B | [Continual Domain Randomization](https://doi.org/10.1109/IROS58592.2024.10802060) | SIM·LEARN | Randomization 요소를 순차적으로 늘리는 continual DR을 pose·마찰·센서 corruption curriculum 후보로 검토한다. |
| IROS24-068 | A | [Domain Randomization-free Sim-to-Real : An Attention-Augmented Memory Approach for Robotic Tasks](https://doi.org/10.1109/IROS58592.2024.10801944) | SIM·OBS | Valve rotation에서 current/history perception을 분리한 attention-memory 정책을 짧은 sensor/action history 비교 근거로 사용한다. |
| IROS24-069 | A | [MPGNet: Learning Move-Push-Grasping Synergy for Target-Oriented Grasping in Occluded Scenes](https://doi.org/10.1109/IROS58592.2024.10802784) | TASK·SYSTEM | 가려진 target을 위해 move/push/grasp 세 정책과 multi-stage training을 연결하므로 상위 재배치 의사결정 비교에 직접 관련된다. |
| IROS24-072 | B | [Efficient Tactile Sensing-based Learning from Limited Real-world Demonstrations for Dual-arm Fine Pinch-Grasp Skills](https://doi.org/10.1109/IROS58592.2024.10802651) | SENSE·CONTACT | 고차원 tactile encoding과 multimodal fusion의 contact search·reactive regrasp 효과를 coarse binary tactile의 상한 비교에 활용한다. |
| IROS24-077 | B | [Is a Simulation better than Teleoperation for Acquiring Human Manipulation Skill Data?](https://doi.org/10.1109/IROS58592.2024.10801865) | SIM·SENSE | Simulation과 teleoperation의 force-interaction demonstration 품질 비교를 접촉 데이터 가상화의 장단점 근거로 참고한다. |
| IROS24-080 | B | [Robotic valve turning: axial misalignment estimation from reaction torques](https://doi.org/10.1109/IROS58592.2024.10801385) | SENSE·CONTACT | Reaction torque pattern으로 회전축 misalignment를 추정하므로 wrist-torque history의 물리적 해석과 진단 feature에 참고한다. |
| IROS24-111 | A | [Reinforcement Learning for Active Search and Grasp in Clutter](https://doi.org/10.1109/IROS58592.2024.10801366) | TASK·SYSTEM | Camera 이동과 occluder 제거 비용을 함께 평가하는 active-search RL을 target 노출·조작 횟수의 system metric에 활용한다. |
| IROS24-128 | C | [Mastering Scene Rearrangement with Expert-Assisted Curriculum Learning and Adaptive Trade-Off Tree-Search](https://doi.org/10.1109/IROS58592.2024.10802526) | SYSTEM·LEARN | Fine-grained rearrangement action, expert-assisted curriculum과 tree search를 Track B 2단계 scene-rearrangement 비교 관점으로 참고한다. |
| IROS24-131 | B | [FF-SRL: High Performance GPU-Based Surgical Simulation For Robot Learning](https://doi.org/10.1109/IROS58592.2024.10801658) | SIM·ENV | Physics와 RL을 단일 GPU에 둔 scalable simulation 설계를 Isaac Lab 병렬환경 profiling·병목 점검 사례로 활용한다. |
| IROS24-148 | A | [The Power of the Senses: Generalizable Manipulation from Vision and Touch through Masked Multimodal Learning](https://doi.org/10.1109/IROS58592.2024.10802719) | SENSE·LEARN | Masked visual–tactile representation과 modality ablation을 Vision+binary tactile+F/T fusion의 확장 baseline으로 검토한다. |
| IROS24-166 | B | [Bridging the Sim-to-Real Gap with Bayesian Inference](https://doi.org/10.1109/IROS58592.2024.10801505) | SIM·SYSID | 저충실도 simulator prior와 real data를 결합한 Bayesian dynamics·uncertainty 추정을 dynamics calibration 후보로 참고한다. |
| IROS24-168 | A | [Tactile Active Inference Reinforcement Learning for Efficient Robotic Manipulation Skill Acquisition](https://doi.org/10.1109/IROS58592.2024.10802750) | SENSE·LEARN | 정적·동적 optical-tactile feature와 active-inference RL의 sparse-reward pushing을 tactile representation·exploration 비교에 활용한다. |
| IROS24-179 | B | [NFPDE: Normalizing Flow-based Parameter Distribution Estimation for Offline Adaptive Domain Randomization](https://doi.org/10.1109/IROS58592.2024.10801378) | SIM·SYSID | 사전 real dataset에서 DR parameter distribution을 추정하므로 tracker·마찰 log 기반 corruption calibration에 연결한다. |
| IROS24-196 | B | [Safe multi-agent reinforcement learning for bimanual dexterous manipulation](https://doi.org/10.1109/IROS58592.2024.10801490) | CONTACT·SAFE | 다지 손 협조제어의 reward와 개별·공동 safety constraint 분리를 접촉 부하·충돌 constraint 설계에 참고한다. |
| IROS24-198 | B | [Learning Variable Compliance Control From a Few Demonstrations for Bimanual Robot with Haptic Feedback Teleoperation System](https://doi.org/10.1109/IROS58592.2024.10801731) | CONTACT·ACTION | Contact-rich task의 variable compliance 학습을 DiffIK/OSC·impedance interface 비교에 활용한다. |
| IROS24-204 | A | [Learning a Shape-Conditioned Agent for Purely Tactile In-Hand Manipulation of Various Objects](https://doi.org/10.1109/IROS58592.2024.10802864) | CONTACT·OBS | Shape-conditioned tactile estimator와 multi-object reorientation을 OBB geometry+tactile 정책의 표현력·일반화 비교에 활용한다. |
| IROS24-205 | A | [Fine Manipulation Using a Tactile Skin: Learning in Simulation and Sim-to-Real Transfer](https://doi.org/10.1109/IROS58592.2024.10801397) | SENSE·SIM | Soft fingertip contact spread를 rigid-body simulator taxel 출력으로 모델링·실물 calibration하므로 tactile 가상화의 직접 근거다. |
| IROS24-209 | B | [CaT: Constraints as Terminations for Legged Locomotion Reinforcement Learning](https://doi.org/10.1109/IROS58592.2024.10802334) | REWARD·SAFE | Constraint 위반을 stochastic termination으로 변환하는 방법을 collision·overload termination과 reward 분리 후보로 검토한다. |
| IROS24-220 | B | [Exploiting Hybrid Policy in Reinforcement Learning for Interpretable Temporal Logic Manipulation](https://doi.org/10.1109/IROS58592.2024.10802202) | SYSTEM·LEARN | Temporal-logic task/waypoint/primitive/parameter 계층을 long-horizon phase gate·전환의 해석 가능한 비교안으로 참고한다. |

#### IROS 2025

| CSV ID | 등급 | 논문 | 관점 | 현재 연구에서 확인할 점 |
| --- | --- | --- | --- | --- |
| IROS25-001 | A | [Interactive Navigation for Legged Manipulators with Learned Arm-Pushing Controller](https://doi.org/10.1109/IROS60139.2025.11246770) | TASK·CONTACT·REWARD | 접촉 가능한 pushing zone 접근 뒤 안정 접촉·전도 방지 push를 학습하는 two-stage reward가 현재 Rotation→Push reward 설계와 직접 대응한다. |
| IROS25-004 | A | [Manipulate-To-Navigate: Reinforcement Learning with Visual Affordances and Manipulability Priors](https://doi.org/10.1109/IROS60139.2025.11247222) | TASK·SYSTEM | Affordance와 manipulability prior로 장애물을 옮긴 뒤 경로를 확보하므로 Track B 2단계 목적·평가의 직접 비교 후보다. |
| IROS25-010 | A | [ACGD: Visual Multitask Policy Learning with Asymmetric Critic Guided Distillation](https://doi.org/10.1109/IROS60139.2025.11247025) | OBS·LEARN | Privileged-state RL expert/critic에서 camera+proprioception student로 distill하는 구조를 asymmetric-critic 정보 계약과 비교한다. |
| IROS25-011 | B | [RT-HCP: Dealing with Inference Delays and Sample Efficiency to Learn Directly on Robotic Platforms](https://doi.org/10.1109/IROS60139.2025.11247277) | ACTION·SYSTEM | 느린 policy inference가 제어주기를 끊지 않도록 action sequence를 공급하므로 policy/control frequency·latency 계약에 참고한다. |
| IROS25-024 | A | [Dexterous Manipulation Based on Prior Dexterous Grasp Pose Knowledge](https://doi.org/10.1109/IROS60139.2025.11247095) | CONTACT·LEARN | Functional grasp-pose prior와 후속 RL을 분리해 초기 hand-configuration 탐색비용을 줄이므로 contact-formation 초기화와 비교한다. |
| IROS25-025 | A | [DexPour: Effective and Efficient High-DoF Robotic Hand Liquid Pouring via Hierarchical Reward with Approximated Proxy Abstraction](https://doi.org/10.1109/IROS60139.2025.11247170) | REWARD·SIM | Approach–grasp–transport–pour reward와 저비용 proxy physics를 phase gate 및 근사 시뮬레이션 설계 사례로 참고한다. |
| IROS25-026 | A | [Peg-in-hole assembly method based on visual reinforcement learning and tactile pose estimation](https://doi.org/10.1109/IROS60139.2025.11246004) | SENSE·CONTACT | Vision-RL pre-assembly 뒤 GelSight pose로 collision·jam을 보정하는 coarse-to-fine sensor 역할 분담을 직접 비교한다. |
| IROS25-045 | B | [KARL: Kalman-Filter Assisted Reinforcement Learner for Dynamic Object Tracking and Grasping](https://doi.org/10.1109/IROS60139.2025.11245879) | OBS·SYSTEM | Occlusion 중 6D pose filter, multi-stage curriculum과 retry를 tracker corruption·recovery evaluation에 활용한다. |
| IROS25-049 | A | [VTAO-BiManip: Masked Visual-Tactile-Action Pre-training with Object Understanding for Bimanual Dexterous Manipulation](https://doi.org/10.1109/IROS60139.2025.11246950) | SENSE·LEARN | Masked visual–tactile–action pretraining과 two-stage curriculum을 multimodal history·phase 학습의 확장안으로 검토한다. |
| IROS25-058 | B | [World Models for Anomaly Detection during Model-Based Reinforcement Learning Inference](https://doi.org/10.1109/IROS60139.2025.11245876) | SAFE·SYSTEM | World-model prediction error로 unseen dynamics·외력을 감지해 정지시키는 방식을 actor 밖 safety supervisor 후보로 참고한다. |
| IROS25-078 | B | [Sampling-Based Model Predictive Control for Dexterous Manipulation on a Biomimetic Tendon-Driven Hand](https://doi.org/10.1109/IROS60139.2025.11246473) | CONTACT·ACTION | Physics-simulator sampling MPC와 objective adaptation의 실물 tendon-hand 결과를 RL 외 contact-rich control baseline으로 검토한다. |
| IROS25-100 | A | [Learning Goal-Directed Object Pushing in Cluttered Scenes With Location-Based Attention](https://doi.org/10.1109/IROS60139.2025.11246809) | TASK·CONTACT | Contact switching·uncertainty·clutter·target orientation을 함께 다루는 goal-directed pushing으로 1·2단계의 직접 baseline 후보다. |
| IROS25-106 | A | [SimLauncher: Launching Sample-Efficient Real-World Robotic Reinforcement Learning via Simulation Pre-Training](https://doi.org/10.1109/IROS60139.2025.11246668) | SIM·LEARN | Digital-twin pretraining을 real-RL target bootstrap과 action proposal에 쓰는 contact-rich dexterous pipeline을 Sim-to-Real 확장에 참고한다. |
| IROS25-118 | A | [Hierarchical Reinforcement Learning for Articulated Tool Manipulation with Multifingered Hand](https://doi.org/10.1109/IROS60139.2025.11246691) | SYSTEM·LEARN | Goal-conditioned low-level hand policy와 high-level tool-goal/arm policy, privileged replay seeding을 1→2단계 계층화와 비교한다. |
| IROS25-137 | A | [RecoveryChaining: Learning Local Recovery Policies for Robust Manipulation](https://doi.org/10.1109/IROS60139.2025.11245856) | SYSTEM·SAFE | Sensor-detected failure에서 nominal controller의 실행 가능 상태로 복구하는 hybrid policy를 contact-loss·실패 복구 후보로 참고한다. |
| IROS25-159 | B | [Context-Based Meta Reinforcement Learning for Robust and Adaptable Peg-in-Hole Assembly Tasks](https://doi.org/10.1109/IROS60139.2025.11247014) | SENSE·SIM | Vision/kinematics meta-RL을 실물 F/T로 빠르게 적응시키는 절차를 sensor 추가·OOD 물성 적응 ablation에 활용한다. |
| IROS25-161 | C | [Refined Policy Distillation: From VLA Generalists to RL Experts](https://doi.org/10.1109/IROS60139.2025.11246761) | SYSTEM·LEARN | VLA action guidance로 RL expert를 distill/refine하므로 상위 VLA와 Track B low-level policy 연결의 후속 후보다. |
| IROS25-168 | A | [TwinTac: A Wide-Range, Highly Sensitive Tactile Sensor with Real-To-Sim Digital Twin Sensor Model](https://doi.org/10.1109/IROS60139.2025.11247002) | SENSE·SIM | 실물 tactile 출력과 FEM을 paired 학습한 real-to-sim digital twin이 sensor-data 가상화 요구에 가장 직접적으로 대응한다. |
| IROS25-185 | B | [Heterogeneous Multi-Agent Learning in Isaac Lab: Scalable Simulation for Robotic Collaboration](https://doi.org/10.1109/IROS60139.2025.11247098) | SIM·ENV | Isaac Lab의 heterogeneous-agent 환경·GPU 병렬학습 확장을 다중 controller/policy 실험의 환경 구성 사례로 참고한다. |
| IROS25-210 | B | [Reward Training Wheels: Adaptive Auxiliary Rewards for Robotics Reinforcement Learning](https://doi.org/10.1109/IROS60139.2025.11247039) | REWARD·LEARN | 학습 진행에 따라 auxiliary-reward weight를 조절하므로 phase shaping weight와 curriculum 민감도 비교에 활용한다. |
| IROS25-228 | B | [Impact of Static Friction on Sim2Real in Robotic Reinforcement Learning](https://doi.org/10.1109/IROS60139.2025.11246554) | SIM·SYSID | Static-friction 식별과 friction-aware DR 효과를 object/support·joint friction randomization 범위 설정에 직접 반영한다. |
| IROS25-230 | A | [Efficient Navigation Among Movable Obstacles using a Mobile Manipulator via Hierarchical Policy Learning](https://doi.org/10.1109/IROS60139.2025.11245985) | TASK·SYSTEM | Obstacle-property estimation, high-level pushing command와 low-level whole-body 실행을 분리한 NAMO 구조를 2단계 계층 baseline으로 검토한다. |
| IROS25-267 | B | [Failure Forecasting Boosts Robustness of Sim2Real Rhythmic Insertion Policies](https://doi.org/10.1109/IROS60139.2025.11247676) | OBS·SAFE | Task-frame pose와 failure-forecaster 기반 retry를 rotation/push 실패 진단·recovery 및 좌표계 선택에 참고한다. |
| IROS25-276 | B | [SAVR: Scooping Adaptation for Variable food properties via Reinforcement Learning](https://doi.org/10.1109/IROS60139.2025.11247626) | SENSE·SIM | Segmentation과 F/T의 물성 변화 Sim-to-Real ablation을 Vision–force fusion·물성 randomization 근거로 활용한다. |
| IROS25-278 | A | [Safe and Efficient Target Singulation with Multi-Fingered Gripper using Collision-Free Push-Stack Synergy](https://doi.org/10.1109/IROS60139.2025.11246677) | TASK·SYSTEM | 밀집·경계 제약에서 collision-free push와 stack을 결합해 target 공간을 만드는 singulation을 system-level 비교에 활용한다. |
| IROS25-284 | B | [Complex Robotic Manipulation via Hindsight Goal Diffusion and Graph-based Experience Replay](https://doi.org/10.1109/IROS60139.2025.11247131) | GOAL·LEARN | Obstacle-graph distance를 goal generation과 replay reward에 쓰므로 sparse goal-conditioned 조작·공간 확보 exploration에 참고한다. |
| IROS25-286 | B | [CageCoOpt: Enhancing Manipulation Robustness through Caging-Guided Morphology and Policy Co-Optimization](https://doi.org/10.1109/IROS60139.2025.11246485) | CONTACT·LEARN | Caging metric을 morphology와 RL policy에 넣어 geometry/contact uncertainty를 줄이는 접촉 품질 후보로 검토한다. |
| IROS25-287 | B | [Automatic Real-to-Sim-to-Real System through Iterative Interactions for Robust Robot Manipulation Policy Learning with Unseen Objects](https://doi.org/10.1109/IROS60139.2025.11247488) | SIM·ENV | 자율 상호작용으로 object reconstruction을 개선하고 복제 simulation에서 학습하는 pipeline을 환경 구축 비교로 참고한다. |
