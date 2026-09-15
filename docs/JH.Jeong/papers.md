# JH.Jeong Paper List

> **범위:** Track B 연구를 위해 현재까지 확인하거나 검토 후보로 수집한 논문
>
> **최종 갱신:** 2026-09-15

---

## 1. 문서 사용법

이 문서는 먼저 전체 논문을 ID 순서로 제시하고, 이후 연구 목적별로 관련 논문을 다시 그룹핑한다. B01–B27은 [`context.md`](./context.md)의 기존 ID와 동일하며, 이후 새로 확인한 논문은 다음 번호부터 연속해서 추가한다.

- 목록 포함은 최종 baseline 선정, 방법 채택 또는 실험 재현을 의미하지 않는다.
- 목적별 그룹은 서로 배타적이지 않다. 한 논문이 여러 목적에 활용될 수 있다.
- arXiv DOI와 정식 출판 DOI를 구분한다. 정식 DOI 미확인은 DOI가 없다는 뜻이 아니다.
- 논문별 상세 해석, 연구와의 연결점과 주의사항은 [`context.md`](./context.md)에서 관리한다.
- 후속 문헌 탐색에서 관련 논문을 새로 확인하면 이 목록과 해당 목적별 그룹을 함께 갱신한다.

---

## 2. 전체 논문 목록

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

---

## 4. 현재 우선 독해 순서

현재 Track B 1단계의 문제 정의와 baseline을 구체화하기 위한 우선순위다. 최종 실험 baseline 선정은 아니다.

1. **B01 GD2P:** 주어진 pushing direction에 적합한 다지 손 configuration 생성과 실행 검증
2. **B21 Hermans et al.:** Pushing을 위한 준비 회전과 contact-location 선택의 고전적 문제 구조
3. **B02 TaskDexGrasp:** 목표 wrench에 적합한 다지 손 contact configuration 평가
4. **B30 HACMan → B29 HyDo:** 학습 기반 contact-location 및 motion-parameter 선택의 발전 관계
5. **B22 DexMove:** Tactile wrist–finger contact control
6. **B28 Where to Touch, How to Contact:** Contact selection과 object subgoal을 결합한 최신 계층적 구조
