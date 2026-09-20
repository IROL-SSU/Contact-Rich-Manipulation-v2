# 목적별 논문 그룹

> [Paper Index](./README.md) · [Intro](../Intro/README.md) · [Previous Works 비교](../Intro/previous_works.md) · [핵심 B-ID 목록](./core_papers.md) · [우선 독해](./reading_guide.md) · [Conference Screening](./screening/README.md)

이 문서는 핵심 논문을 연구 질문별로 다시 묶은 **lookup-oriented synthesis**다. 처음부터 모든 절을 순서대로 읽기보다 아래 경로에서 현재 질문에 맞는 절로 이동한다. 동일 논문이 여러 질문에 답하면 중복 배치한다.

## 이 문서를 사용하는 순서

| 현재 목적 | 먼저 읽을 절 | 다음에 읽을 절 |
| --- | --- | --- |
| 연구 motivation과 최신 baseline 파악 | [`../Intro/README.md`](../Intro/README.md) | [`../Intro/previous_works.md`](../Intro/previous_works.md) → 8. 최신 VLA·IL 기반 motivation |
| Approach hand configuration 검토 | 1. Hand·Contact Configuration | 6. Hand-pose·접촉 품질 → 2. Phase 전환 |
| Tactile·F/T observation 검토 | 3. Closed-loop contact 보정 | 7. Observation 표현과 구현 근거 |
| Goal-conditioned manipulation 범위 검토 | 4. Nonprehensile manipulation | 5. Retrieval·2단계 확장 |
| Reward formulation 검토 | 10. Reward formulation | [`reward_formulation.md`](./reward_formulation.md) |

각 절의 표는 `관련 논문 → 이미 해결한 부분 → Track B에서 남는 질문`의 순서로 읽는다.

---

## 1. Track B 1단계 핵심 — 주어진 방향의 Pushing을 위한 Hand·Contact Configuration

| 우선 | ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- | --- |
| ★ | [B01](https://doi.org/10.48550/arXiv.2509.18455) | [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | 물체 geometry와 pushing direction으로 다지 손 pre-contact pose를 생성·선택하고 실제 push 성공으로 검증하는 방법 |
| ★ | [B02](https://doi.org/10.1109/IROS58592.2024.10802652) | [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652) | Task wrench에 적합한 다지 손 contact configuration을 평가·합성하는 방법 |
| ★ | [B88](https://doi.org/10.1109/LRA.2026.3677744) | [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744) | Task-informed 초기 grasp와 작은 residual joint correction을 결합해 동적 외력 아래 slip을 줄이는 방법 |
| ★ | [B21](https://doi.org/10.1109/HUMANOIDS.2013.7030011) | [Learning Contact Locations for Pushing and Orienting Unknown Objects](https://doi.org/10.1109/HUMANOIDS.2013.7030011) | 안정적인 직선 밀기와 준비 회전을 위한 contact location을 물체 형상에서 선택하는 고전적 관점 |
|  | [B23](https://doi.org/10.1109/IROS47612.2022.9982177) | [Task-Oriented Contact Optimization](https://doi.org/10.1109/IROS47612.2022.9982177) | 주어진 물체 궤적을 적은 접촉력으로 수행하기 위한 contact placement 최적화 |
|  | [B30](https://doi.org/10.48550/arXiv.2305.03942) | [HACMan](https://doi.org/10.48550/arXiv.2305.03942) | Point cloud에서 contact location과 접촉 후 motion parameter를 함께 학습하는 action representation |
|  | [B29](https://doi.org/10.1109/LRA.2025.3564780) | [HyDo](https://doi.org/10.1109/LRA.2025.3564780) | Contact-point selection과 continuous motion parameter를 hybrid RL로 탐색하는 방법 |

## 2. Rotation–Push 연결과 Phase·Contact 전환

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| [B21](https://doi.org/10.1109/HUMANOIDS.2013.7030011) | [Learning Contact Locations for Pushing and Orienting Unknown Objects](https://doi.org/10.1109/HUMANOIDS.2013.7030011) | 물체를 먼저 회전시켜 안정적인 straight-push contact를 목표 방향과 정렬하는 구조 |
| [B24](https://doi.org/10.1002/aisy.202300621) | [Tactile-Based Negotiation](https://doi.org/10.1002/aisy.202300621) | 장애물의 준비 회전 후 병진, 접촉 위치 선택과 감각 기반 정렬 |
| [B08](https://doi.org/10.48550/arXiv.2309.00987) | [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987) | 이전 policy의 종료 분포와 다음 policy의 실행 가능성을 연결하는 skill chaining |
| [B06](https://doi.org/10.48550/arXiv.2604.25126) | [HANDFUL](https://doi.org/10.48550/arXiv.2604.25126) | 순차 manipulation에서 현재 접촉과 후속 사용 자원을 함께 고려하는 curriculum |
| [B07](https://doi.org/10.48550/arXiv.2503.23120) | [ExDex](https://doi.org/10.48550/arXiv.2503.23120) | Arm–hand nonprehensile phase 연결과 종료 상태 활용 |
| [B16](https://doi.org/10.1145/3588432.3591528) | [Nonprehensile Pregrasp](https://doi.org/10.1145/3588432.3591528) | 후속 grasp를 가능하게 하는 사전 nonprehensile manipulation planning |
| [B28](https://doi.org/10.48550/arXiv.2601.10930) | [Where to Touch, How to Contact](https://doi.org/10.48550/arXiv.2601.10930) | Contact location과 post-contact object subgoal을 연결하는 계층적 interface |
| [B31](https://doi.org/10.15607/RSS.2024.XX.129) | [HACMan++](https://doi.org/10.15607/RSS.2024.XX.129) | Primitive type·location·parameter를 선택하고 여러 primitive를 순서대로 연결하는 방법 |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | [HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154) | Object·environment geometry module을 조합해 일반 환경에서 reach·reorient·translate 계열 behavior를 연결하는 방식 |
| [B93](https://doi.org/10.48550/arXiv.2502.18015) | [SPIN](https://doi.org/10.48550/arXiv.2502.18015) | Skill applicability·intermediate pose planning과 disturbance-minimizing connector로 phase transition을 명시적으로 다루는 방식 |
| [B39](https://doi.org/10.48550/arXiv.2111.03043) | [General In-Hand Object Re-Orientation](https://doi.org/10.48550/arXiv.2111.03043) | 임의의 SO(3) goal 도달에서 current–goal quaternion difference와 symmetry-aware success를 사용하는 방식 |
| [B40](https://doi.org/10.48550/arXiv.2309.09979) | [RotateIt](https://doi.org/10.48550/arXiv.2309.09979) | 최종 자세가 아니라 hand-centric rotation-axis를 목표로 연속 회전을 학습하는 방식 |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Pushing과 pivoting에 공통 progress·success·smoothness를 두고, 동적으로 가능한 EEF pose·force direction·extrinsic contact를 시연 기반 reward로 추가하는 최신 방식 |
| [B82](https://doi.org/10.1109/ICRA55743.2025.11128166) | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166) | OBB keypoint error로 position·yaw를 함께 평가하고 collision·toppling을 constraint로 분리하는 방식 |
| [B85](https://doi.org/10.1109/ICRA48891.2023.10161271) | [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) | 목표 orientation error만으로 pivoting policy를 학습하고 object feature 기반 state/action projection으로 unseen object에 전이하는 방식 |
| [B86](https://doi.org/10.48550/arXiv.1703.00472) | [Reinforcement Learning for Pivoting Task](https://doi.org/10.48550/arXiv.1703.00472) | 정규화한 target-angle error만 사용하는 역사적 최소 pivoting reward baseline |
| [B74](https://doi.org/10.48550/arXiv.2502.15442) | [Privileged Action](https://doi.org/10.48550/arXiv.2502.15442) | Virtual force·constraint relaxation curriculum으로 phase label 없이 push·pivot·grasp를 포함한 long-horizon behavior를 탐색하는 방식 |
| [B75](https://doi.org/10.48550/arXiv.2603.15789) | [Emergent Dexterity / OmniReset](https://doi.org/10.48550/arXiv.2603.15789) | Diverse reset state가 multi-phase contact behavior와 recovery 탐색을 단순화하는 방식 |
| [B76](https://doi.org/10.48550/arXiv.2510.11019) | [Refinery](https://doi.org/10.48550/arXiv.2510.11019) | 개별 contact policy의 취약한 initial-state 영역을 fine-tuning하고 policy chaining 성공률을 높이는 방식 |
| [B87](https://doi.org/10.48550/arXiv.2509.17812) | [Tac2Motion](https://doi.org/10.48550/arXiv.2509.17812) | Firm contact와 선택적 contact release를 함께 보상하여 grasp 유지와 smooth finger gaiting을 동시에 학습하는 방식 |
| [B88](https://doi.org/10.1109/LRA.2026.3677744) | [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744) | 좋은 초기 grasp를 유지하되 residual joint correction으로 slip과 외력 변화에 적응하는 방식 |
| [B89](https://doi.org/10.1109/ICRA57147.2024.10611300) | [Guided Exploration with Sub-skill Controllers](https://doi.org/10.1109/ICRA57147.2024.10611300) | 나머지 손가락의 지지를 유지하면서 한 손가락의 접촉을 해제·재형성하는 contact switching이 큰 조작 범위에 필요한 사례 |

## 3. Tactile·F/T 기반 Closed-Loop Contact 형성·보정

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| [B09](https://doi.org/10.1109/LRA.2024.3478571) | [DexTouch](https://doi.org/10.1109/LRA.2024.3478571) | Tactile observation을 이용한 접촉 탐색·조작과 sim-to-real sensor ablation |
| [B10](https://doi.org/10.1109/LRA.2023.3295236) | [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236) | Goal-conditioned pushing에서 tactile 기반 model-based/model-free RL 비교 |
| [B11](https://doi.org/10.1109/LRA.2023.3295991) | [Bi-Touch](https://doi.org/10.1109/LRA.2023.3295991) | Tactile 접촉 유지와 물체 reorientation을 위한 bimanual RL |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | [Visuotactile Estimation and Control](https://doi.org/10.48550/arXiv.2412.13157) | Occlusion 아래 pose 추정 불확실성과 contact feedback을 연결하는 방법 |
| [B13](https://doi.org/10.1109/LRA.2024.3511378) | [Coarse-to-Fine Pushing](https://doi.org/10.1109/LRA.2024.3511378) | Vision의 전역 정보와 touch·proprioception의 국소 보정 역할 분담 |
| [B14](https://doi.org/10.1109/LRA.2024.3414180) | [Force Push](https://doi.org/10.1109/LRA.2024.3414180) | Pose·물성 불확실성 아래 force feedback으로 pushing 방향과 속도를 보정하는 제어 baseline |
| [B15](https://doi.org/10.15607/RSS.2024.XX.130) | [RoboPack](https://doi.org/10.15607/RSS.2024.XX.130) | 접촉 이력으로 latent dynamics를 추정하고 MPC에 활용하는 방법 |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | [DexMove](https://openreview.net/forum?id=dT3ZciXvNX) | Tactile 기반 wrist–finger 공동 nonprehensile control |
| [B24](https://doi.org/10.1002/aisy.202300621) | [Tactile-Based Negotiation](https://doi.org/10.1002/aisy.202300621) | Contact location과 force/proximity feedback을 이용한 표면 정렬·순응 제어 |
| [B27](https://openreview.net/forum?id=jf7C7EGw21) | [VTDexManip](https://openreview.net/forum?id=jf7C7EGw21) | Visual–tactile representation pretraining과 dexterous RL sensor 표현 |
| [B32](https://doi.org/10.15607/RSS.2023.XIX.036) | [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036) | Sensor-link contact-force norm을 threshold한 16D binary tactile와 4-step state stack |
| [B33](https://doi.org/10.1109/ICRA57147.2024.10610532) | [Robot Synesthesia](https://doi.org/10.1109/ICRA57147.2024.10610532) | Binary active sensor의 위치를 palm-frame tactile point cloud로 바꾸어 visual point cloud와 결합 |
| [B34](https://doi.org/10.1109/TRO.2025.3547267) | [TacSL](https://doi.org/10.1109/TRO.2025.3547267) | Visuotactile RGB와 per-taxel force field를 GPU에서 생성하는 고충실도 tactile simulation 후보 |
| [B36](https://doi.org/10.48550/arXiv.2411.04776) | [TacEx](https://doi.org/10.48550/arXiv.2411.04776) | Isaac Sim에서 GelSight deformation·RGB observation을 생성하는 외부 tactile simulation 후보 |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | [Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) | Tactile로 pusher–object 상대 pose를 안정화하고 proprioception으로 target bearing·distance를 정렬하는 역할 분담 |
| [B56](https://doi.org/10.15607/RSS.2025.XXI.052) | [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052) | Slow trajectory policy와 fast tactile/force feedback branch를 분리한 reactive IL 상한 |
| [B57](https://doi.org/10.1109/LRA.2025.3560871) | [FoAR](https://doi.org/10.1109/LRA.2025.3560871) | Future-contact predictor로 vision과 high-frequency wrist F/T의 중요도를 조절하는 IL baseline |
| [B73](https://doi.org/10.1109/LRA.2025.3551637) | [FORGE](https://doi.org/10.1109/LRA.2025.3551637) | Force threshold·dynamics randomization으로 pose uncertainty 아래 contact policy를 안전하게 전이하는 RL baseline |
| [B77](https://doi.org/10.1109/LRA.2025.3596487) | [DP-RRL](https://doi.org/10.1109/LRA.2025.3596487) | Demonstration diffusion policy의 motion·force trajectory를 residual RL이 online 보정하는 hybrid baseline |
| [B78](https://doi.org/10.1109/LRA.2026.3681156) | [MSDP](https://doi.org/10.1109/LRA.2026.3681156) | Vision·force·proprioception의 masked dynamic pretraining과 asymmetric actor–critic sensor fusion |
| [B80](https://doi.org/10.48550/arXiv.2405.10315) | [TRANSIC](https://doi.org/10.48550/arXiv.2405.10315) | Simulation base policy의 real-world failure를 human correction 기반 residual policy로 보완하는 Sim-to-Real baseline |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Wrist force와 extrinsic contact를 deployment observation과 privileged training signal로 분리하고 force magnitude가 아닌 방향을 reference와 맞추는 방식 |
| [B92](https://doi.org/10.15607/RSS.2024.XX.135) | [Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135) | High-resolution tactile로 extrinsic contact state를 추정하고 sticking·sliding contact mode를 optimization으로 제어하는 방식 |
| [B97](https://doi.org/10.1109/ICRA48506.2021.9562061) | [Dexterous Manoeuvre through Touch](https://doi.org/10.1109/ICRA48506.2021.9562061) | Clutter interaction에서 tactile representation과 RL을 결합한 2021년 direct nonprehensile 사례 |
| [B83](https://doi.org/10.3389/fnbot.2023.1271607) | [Adaptive Reaching and Pushing](https://doi.org/10.3389/fnbot.2023.1271607) | Contact-force 방향과 lever arm을 privileged reward로 사용해 goal-directed translation을 유도하는 방식 |
| [B87](https://doi.org/10.48550/arXiv.2509.17812) | [Tac2Motion](https://doi.org/10.48550/arXiv.2509.17812) | Tactile observation과 firm-contact·contact-release reward를 결합하며, release만 장려해서는 유의미한 finger gaiting이 형성되지 않았다는 ablation |

## 4. Goal-Conditioned Nonprehensile Manipulation과 Contact 선택

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| [B25](https://doi.org/10.48550/arXiv.2403.10760) | [CORN](https://doi.org/10.48550/arXiv.2403.10760) | Contact prediction으로 사전학습한 geometry representation과 goal-conditioned RL |
| [B26](https://doi.org/10.48550/arXiv.2503.16806) | [DyWA](https://doi.org/10.48550/arXiv.2503.16806) | 물성 변화에 적응하는 history-conditioned representation과 dynamics prediction |
| [B29](https://doi.org/10.1109/LRA.2025.3564780) | [HyDo](https://doi.org/10.1109/LRA.2025.3564780) | Discrete contact point와 continuous motion을 함께 다루는 hybrid action learning |
| [B30](https://doi.org/10.48550/arXiv.2305.03942) | [HACMan](https://doi.org/10.48550/arXiv.2305.03942) | 6D object goal을 위한 spatially grounded contact-point/motion action |
| [B01](https://doi.org/10.48550/arXiv.2509.18455) | [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | 주어진 object state와 pushing direction에 대한 geometry-conditioned hand pose |
| [B23](https://doi.org/10.1109/IROS47612.2022.9982177) | [Task-Oriented Contact Optimization](https://doi.org/10.1109/IROS47612.2022.9982177) | 주어진 planar trajectory에 적합한 contact 배치 결정 |
| [B28](https://doi.org/10.48550/arXiv.2601.10930) | [Where to Touch, How to Contact](https://doi.org/10.48550/arXiv.2601.10930) | Surface contact와 object subgoal을 함께 표현하는 contact intention |
| [B74](https://doi.org/10.48550/arXiv.2502.15442) | [Privileged Action](https://doi.org/10.48550/arXiv.2502.15442) | Virtual force와 constraint relaxation을 이용해 sparse-reward long-horizon nonprehensile behavior를 발견하는 방식 |
| [B75](https://doi.org/10.48550/arXiv.2603.15789) | [Emergent Dexterity / OmniReset](https://doi.org/10.48550/arXiv.2603.15789) | Reward·curriculum 변경보다 reset-state coverage를 확장해 contact-rich exploration을 개선하는 방식 |
| [B79](https://doi.org/10.15607/RSS.2025.XXI.019) | [ConRFT](https://doi.org/10.15607/RSS.2025.XXI.019) | 소수 demonstration에서 BC+Q learning으로 초기화한 뒤 online RL로 실제 policy를 개선하는 VLA–RL 결합 |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Object task progress와 CITO의 dynamically feasible contact reference를 결합하는 방식 |
| [B82](https://doi.org/10.1109/ICRA55743.2025.11128166) | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166) | Object-goal pose reward와 표면 reach target·motion direction을 분리하고 safety를 constraint로 다루는 방식 |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) | Object-goal·EEF-object distance, desired contact와 clutter collision을 결합한 방식 |
| [B85](https://doi.org/10.1109/ICRA48891.2023.10161271) | [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) | Full object pose·wrist F/T를 state로 사용한 target-orientation pivoting의 최소 reward 사례 |
| [B90](https://doi.org/10.15607/RSS.2025.XXI.154) | [HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154) | Object와 environment geometry를 함께 표현해 unseen general environment에 전이하는 modular RL baseline |
| [B91](https://doi.org/10.15607/RSS.2025.XXI.153) | [PIN-WM](https://doi.org/10.15607/RSS.2025.XXI.153) | Few-shot visual interaction으로 rigid-body dynamics를 식별하고 model-based RL에 사용하는 geometry·physics uncertainty 대안 |
| [B94](https://doi.org/10.15607/RSS.2026.XXII.149) | [DAPL](https://doi.org/10.15607/RSS.2026.XXII.149) | Clutter의 contact-induced multi-object dynamics representation으로 unseen scene에 대응하는 baseline |
| [B95](https://doi.org/10.48550/arXiv.2605.25672) | [Compliant Non-Prehensile Pushing](https://doi.org/10.48550/arXiv.2605.25672) | Impedance control·MPC contact-point adaptation·passivity filter를 결합한 model-based pushing safety baseline |

## 5. Track B 2단계 — 조작 의사결정, 공간 확보와 Retrieval

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| [B17](https://doi.org/10.48550/arXiv.2502.18423) | [RetrDex](https://doi.org/10.48550/arXiv.2502.18423) | Clutter clearing을 통해 target retrieval 가능성을 높이는 정책과 시스템 평가 |
| [B24](https://doi.org/10.1002/aisy.202300621) | [Tactile-Based Negotiation](https://doi.org/10.1002/aisy.202300621) | Movable obstacle를 회전·이동하여 navigation path를 확보하는 목적 설계 |
| [B28](https://doi.org/10.48550/arXiv.2601.10930) | [Where to Touch, How to Contact](https://doi.org/10.48550/arXiv.2601.10930) | High-level RL의 contact/subgoal 선택과 low-level contact dynamics 실행 분리 |
| [B31](https://doi.org/10.15607/RSS.2024.XX.129) | [HACMan++](https://doi.org/10.15607/RSS.2024.XX.129) | 목적에 따라 motion primitive의 종류·위치·parameter와 순서를 결정하는 구조 |
| [B08](https://doi.org/10.48550/arXiv.2309.00987) | [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987) | 장기 과업을 위한 policy chaining과 phase transition |
| [B16](https://doi.org/10.1145/3588432.3591528) | [Nonprehensile Pregrasp](https://doi.org/10.1145/3588432.3591528) | 후속 manipulation 성공을 목적으로 하는 사전 물체 재배치 |
| [B25](https://doi.org/10.48550/arXiv.2403.10760) | [CORN](https://doi.org/10.48550/arXiv.2403.10760) | Unseen geometry에 대한 goal-conditioned object manipulation |

## 6. Hand-Pose 생성·접촉 품질·후속 성공 평가의 배경

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| [B02](https://doi.org/10.1109/IROS58592.2024.10802652) | [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652) | Task Wrench Space와 Grasp Wrench Space를 이용한 task-specific contact quality |
| [B03](https://doi.org/10.1109/ICRA55743.2025.11127792) | [RL Critic Grasp Selection](https://doi.org/10.1109/ICRA55743.2025.11127792) | 후속 manipulation critic으로 초기 hand configuration 후보를 평가·선택하는 발상 |
| [B04](https://doi.org/10.1007/978-3-031-73347-5_22) | [GraspXL](https://doi.org/10.1007/978-3-031-73347-5_22) | Goal·접촉·안정성을 결합한 RL grasp-motion synthesis |
| [B05](https://doi.org/10.48550/arXiv.2403.12421) | [UniDexFPM](https://doi.org/10.48550/arXiv.2403.12421) | Functional pre-grasp goal과 RL expert·diffusion policy의 연결 |
| [B18](https://doi.org/10.1109/LRA.2021.3129138) | [Differentiable Force Closure Estimator](https://doi.org/10.1109/LRA.2021.3129138) | Differentiable force-closure surrogate의 물리 가정과 한계 |
| [B19](https://doi.org/10.48550/arXiv.2210.02697) | [DexGraspNet](https://doi.org/10.48550/arXiv.2210.02697) | Distance·joint-limit·penetration energy와 physics validation 기반 pose generation |
| [B20](https://doi.org/10.48550/arXiv.2410.23701) | [Get a Grip](https://doi.org/10.48550/arXiv.2410.23701) | 정적 geometry surrogate와 실제 rollout 성공 evaluator를 분리하는 방식 |
| [B88](https://doi.org/10.1109/LRA.2026.3677744) | [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744) | 초기 task-informed grasp를 고정 정답으로 보지 않고, 동적 작업 중 residual finger adaptation으로 안정성을 유지하는 방식 |
| [B89](https://doi.org/10.1109/ICRA57147.2024.10611300) | [Guided Exploration with Sub-skill Controllers](https://doi.org/10.1109/ICRA57147.2024.10611300) | Contact switching을 별도 조작 sub-skill로 보고 exploration을 유도한 사례; reconfiguration 자체를 금지하면 조작 범위를 제한할 수 있다는 반례 |

## 7. Policy Observation 표현과 [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831) 구현 근거

| ID | 논문 | 이 목적에서 확인할 내용 |
| --- | --- | --- |
| [B09](https://doi.org/10.1109/LRA.2024.3478571) | [DexTouch](https://doi.org/10.1109/LRA.2024.3478571) | History 없는 current 16D binary tactile, full-hand spatial coverage와 zero-shot sim-to-real 결과; wrist F/T·sensor deactivation ablation |
| [B12](https://doi.org/10.48550/arXiv.2412.13157) | [Visuotactile Estimation and Control](https://doi.org/10.48550/arXiv.2412.13157) | Object-pose observation의 occlusion·noise, EEF pose·wrench history, recurrent pose estimator와 uncertainty-conditioned policy |
| [B22](https://openreview.net/forum?id=dT3ZciXvNX) | [DexMove](https://openreview.net/forum?id=dT3ZciXvNX) | Wrist·finger·object·contact state의 5-frame history와 marker-level normal/shear tactile field를 쓰는 고정보량 상한선 |
| [B27](https://openreview.net/forum?id=jf7C7EGw21) | [VTDexManip](https://openreview.net/forum?id=jf7C7EGw21) | 224×224 RGB–ResNet 계열과 force-threshold binary tactile–MLP를 proprioception과 결합하는 구조 |
| [B32](https://doi.org/10.15607/RSS.2023.XIX.036) | [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036) | Binary tactile, hand q, previous joint target, rotation axis를 4-step stack하여 MLP에 입력하는 최소 tactile policy |
| [B33](https://doi.org/10.1109/ICRA57147.2024.10610532) | [Robot Synesthesia](https://doi.org/10.1109/ICRA57147.2024.10610532) | Depth-camera point cloud, robot mesh point cloud와 active tactile-sensor point cloud를 palm frame에서 융합하는 표현 |
| [B34](https://doi.org/10.1109/TRO.2025.3547267) | [TacSL](https://doi.org/10.1109/TRO.2025.3547267) | 고충실도 visuotactile image·force field를 사용할 때 필요한 별도 sensor simulation 경로와 비용 |
| [B35](https://doi.org/10.48550/arXiv.2511.04831) | [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831) | Camera, ContactSensor, FrameTransformer, joint wrench, quaternion과 `last_action` observation term을 통한 구현 가능 범위 |
| [B36](https://doi.org/10.48550/arXiv.2411.04776) | [TacEx](https://doi.org/10.48550/arXiv.2411.04776) | Vanilla ContactSensor를 넘어 GelSight 영상까지 모사할 때의 외부 Isaac Sim 확장 후보 |
| [B37](https://doi.org/10.1109/TRO.2021.3104471) | [Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) | Local tactile pose와 global proprioceptive goal을 분리한 이유; binary tactile로 그대로 재현할 수 없는 정보의 경계 |
| [B38](https://doi.org/10.1109/CVPR.2019.00589) | [Continuity of Rotation Representations](https://doi.org/10.1109/CVPR.2019.00589) | Quaternion의 antipodal discontinuity와 5D/6D continuous representation의 근거. 현재는 Isaac Lab interface와 차원 단순성을 우선해 canonical quaternion 4D를 baseline으로 선택하며, 6D는 sign-boundary failure를 확인할 representation ablation으로 사용 |
| [B39](https://doi.org/10.48550/arXiv.2111.03043) | [General In-Hand Object Re-Orientation](https://doi.org/10.48550/arXiv.2111.03043) | 임의 goal orientation에는 quaternion difference를, symmetric-object vision 평가에는 shape-equivalent criterion을 둔 사례 |
| [B40](https://doi.org/10.48550/arXiv.2309.09979) | [RotateIt](https://doi.org/10.48550/arXiv.2309.09979) | 연속 회전 task에서 hand-centric 3D rotation-axis vector를 observation에 추가한 이유 |

## 8. 최신 VLA·IL 기반 Research Motivation

이 그룹은 VLA·IL을 약한 비교 대상으로 만드는 것이 아니라, **각 계열이 이미 해결한 문제와 Track B가 추가로 검증할 문제의 경계**를 확인하기 위한 것이다. 종합된 Intro 논리는 [`../Intro/README.md`](../Intro/README.md)에서 관리한다.

### 8.1 최신 핵심 연구 선별 기준

`최신 연구`는 [π0](https://doi.org/10.48550/arXiv.2410.24164)가 공개된 **2024-10-31 이후 정식 게재된 논문**으로 정의한다. 최초 preprint가 더 이르더라도 정식 학회·저널 게재가 이후라면 포함하며, 2025–2026년 arXiv 문서라도 정식 게재가 확인되지 않으면 핵심 baseline이 아니라 watch list로 구분한다.

1. **정식 게재 수준:** ICLR·NeurIPS·ICCV·RSS·CoRL·ICRA 또는 IEEE RA-L을 우선한다.
2. **Track B 직접성:** Contact-rich, nonprehensile, dexterous hand, tactile/F/T와 long-horizon transition의 중첩 정도를 본다.
3. **영향력:** 인용 수와 후속 baseline 채택을 보조 지표로 사용하되, 최신 논문의 시간상 불이익을 감안한다.
4. **비교 가능성:** Observation·action·sensor·data budget과 공개 code·checkpoint·dataset을 확인한다.
5. **주장 대응성:** 각 baseline이 검증할 Track B의 주장 또는 구성요소를 명시한다.

아래 인용 수는 **2026-09-16 Semantic Scholar snapshot**이며 데이터베이스와 시점에 따라 바뀐다. 연구의 질이나 직접성을 대신하는 지표로 사용하지 않는다.

### 8.2 영향력과 Track B 비교 역할

| 계열 | 연구·정식 게재 | 영향력 snapshot | 이 연구가 이미 보여준 것 | Track B에서 남는 비교 질문 | 현재 역할 |
| --- | --- | ---: | --- | --- | --- |
| Generalist VLA | [B43](https://doi.org/10.48550/arXiv.2504.16054) [π0.5](https://doi.org/10.48550/arXiv.2504.16054), CoRL 2025 | 1,764 citations | 이종 robot·web data와 semantic subtask prediction으로 새로운 가정의 장기 household task까지 일반화 | Semantic generalization이 제한된 shelf의 contact feasibility와 force safety도 보장하는가 | Literature upper reference |
| Efficient VLA | [B44](https://doi.org/10.15607/RSS.2025.XXI.017) [OpenVLA-OFT](https://doi.org/10.15607/RSS.2025.XXI.017), RSS 2025 | 850 citations | Parallel decoding·action chunking·continuous action으로 VLA adaptation의 속도와 성공률을 개선 | 낮은 latency가 contact observability와 Rotation→Push terminal-contact quality도 해결하는가 | VLA adaptation reference |
| Contact-aware VLA | [B99](https://doi.org/10.48550/arXiv.2603.15169) [ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169), CVPR 2026 | 신규·집계 미성숙 | Multi-view vision과 6축 F/T를 force-aware reasoning에 연결하고 force target·control mode를 출력해 closed-loop hybrid force–position regulation을 수행 | 대규모 backbone·task-specific multimodal demonstration 없이 coarse tactile+F/T로 같은 물리적 보정이 가능한가 | 핵심 adjacent VLA reference |
| Tactile-aware VLA | [B48](https://doi.org/10.48550/arXiv.2507.09160) [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160), arXiv 2025 | Preprint·집계 미성숙 | High-resolution tactile history를 target-force action, hybrid position–force control과 failure reasoning에 연결해 cross-task·OOD adaptation을 평가 | Rich tactile와 task demonstrations 없이 binary tactile+F/T RL이 geometry error와 Rotation→Push transition을 보완할 수 있는가 | Adjacent VLA reference; 정식 게재 전 핵심 baseline으로 격상하지 않음 |
| Reactive IL | [B56](https://doi.org/10.15607/RSS.2025.XXI.052) [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052), RSS 2025 | 204 citations | Slow diffusion와 fast tactile/force branch로 action chunk 중 폐루프 반응을 구현 | 고주파 반응이 Approach configuration과 Rotation terminal state를 최종 Push success에 맞게 최적화하는가 | 강한 matched IL 후보 |
| Force-aware IL | [B57](https://doi.org/10.1109/LRA.2025.3560871) [FoAR](https://doi.org/10.1109/LRA.2025.3560871), IEEE RA-L 2025 | 68 citations | Vision과 high-frequency wrist F/T를 future-contact prediction으로 융합 | 유사한 F/T 조건에서 IL과 simulation RL의 failure coverage·비용은 어떻게 다른가 | 현실적인 matched IL 후보 |
| Visuotactile RL | [B27](https://openreview.net/forum?id=jf7C7EGw21) [VTDexManip](https://openreview.net/forum?id=jf7C7EGw21), ICLR 2025 | 재확인 필요 | Sparse binary tactile가 policy 성능과 noise robustness에 기여 | Track B에서도 binary tactile가 충분하며 17-channel과 coarse pooling 중 무엇이 필요한가 | Sensor·representation baseline |
| Sim-to-Real RL | [B73](https://doi.org/10.1109/LRA.2025.3551637) [FORGE](https://doi.org/10.1109/LRA.2025.3551637), IEEE RA-L 2025 | 45 citations | Force threshold와 dynamics randomization으로 pose uncertainty 아래 real transfer | Threshold-conditioned safety와 privileged-force reward 중 무엇이 강건한가 | Force-safety·Sim-to-Real baseline |
| Long-horizon RL | [B74](https://doi.org/10.48550/arXiv.2502.15442) [Privileged Action](https://doi.org/10.48550/arXiv.2502.15442), CoRL 2025 Spotlight | 3 citations | Virtual force·constraint relaxation curriculum으로 multi-stage contact behavior를 학습 | Reward gate와 privileged critic만으로 같은 exploration hurdle을 넘을 수 있는가 | 직접 formulation comparator |
| Scalable RL | [B75](https://doi.org/10.48550/arXiv.2603.15789) [OmniReset](https://doi.org/10.48550/arXiv.2603.15789), ICLR 2026 | 11 citations | Diverse resets로 multi-phase behavior와 retry를 학습 | Phase gate의 이득이 reset coverage를 통제한 뒤에도 남는가 | 강한 reset/curriculum comparator |
| Nonprehensile world model | [B26](https://doi.org/10.48550/arXiv.2503.16806) [DyWA](https://doi.org/10.48550/arXiv.2503.16806), ICCV 2025 | 27 citations | History로 dynamics variation을 추정해 unseen condition에 일반화 | Dense geometry/world model 없이 OBB+tactile/F/T history로 충분한가 | Task·generalization baseline |
| Dexterous tactile IL | [B22](https://openreview.net/forum?id=dT3ZciXvNX) [DexMove](https://openreview.net/forum?id=dT3ZciXvNX), ICLR 2026 | 신규·집계 미성숙 | Simulation trajectory와 human tactile demonstration을 결합한 wrist–finger nonprehensile control | Dense visuotactile·hybrid demonstration 없이 coarse tactile RL이 어느 수준까지 가능한가 | 가장 가까운 task-level reference |
| Geometry-conditioned execution | [B01](https://doi.org/10.48550/arXiv.2509.18455) [GD2P](https://doi.org/10.48550/arXiv.2509.18455), ICRA 2026 | 5 citations | Pushing/pulling direction에 맞는 dexterous pre-contact pose를 대규모로 생성·실험 | 정적 pre-contact selection과 contact 중 feedback adaptation 중 무엇이 필요한가 | Contact-configuration baseline |

높은 인용 수와 task 직접성은 다르다. [π0.5](https://doi.org/10.48550/arXiv.2504.16054)·[OpenVLA-OFT](https://doi.org/10.15607/RSS.2025.XXI.017)는 영향력이 큰 상한 reference이고, [DexMove](https://openreview.net/forum?id=dT3ZciXvNX)·[DyWA](https://doi.org/10.48550/arXiv.2503.16806)·[RDP](https://doi.org/10.15607/RSS.2025.XXI.052)·[FoAR](https://doi.org/10.1109/LRA.2025.3560871)와 long-horizon contact-rich RL이 Track B의 실험 질문에는 더 가깝다.

### 8.3 Baseline 계층과 비교 원칙

| 계층 | 포함할 연구·방법 | 비교 목적 | 구현 원칙 |
| --- | --- | --- | --- |
| A. Matched experimental baseline | Same-task PPO/asymmetric critic, [FoAR](https://doi.org/10.1109/LRA.2025.3560871)-style vision+F/T IL, 가능한 경우 [RDP](https://doi.org/10.15607/RSS.2025.XXI.052)-style reactive IL | Reward·privileged learning·closed-loop transition의 실제 이득 검증 | Task distribution, actor observation, action, controller와 real trial budget을 맞추고 pretraining·demonstration 비용도 기록 |
| B. Component baseline | [B27](https://openreview.net/forum?id=jf7C7EGw21) tactile encoding, [B73](https://doi.org/10.1109/LRA.2025.3551637) force threshold·randomization, [B74](https://doi.org/10.48550/arXiv.2502.15442) privileged curriculum, [B75](https://doi.org/10.48550/arXiv.2603.15789) diverse resets | Sensor, force safety, exploration과 phase-free learning의 효과 분리 | 전체 architecture 대신 논문이 검증한 핵심 구성요소를 ablation으로 재현 |
| C. Closest-task system baseline | [B26](https://doi.org/10.48550/arXiv.2503.16806) [DyWA](https://doi.org/10.48550/arXiv.2503.16806), [B22](https://openreview.net/forum?id=dT3ZciXvNX) [DexMove](https://openreview.net/forum?id=dT3ZciXvNX), [B01](https://doi.org/10.48550/arXiv.2509.18455) [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Nonprehensile generalization, wrist–finger contact와 hand-pose selection의 현재 상한 확인 | Sensor·geometry·data가 다르면 성공률을 단순 대조하지 않고 공통 조건에서만 정량 비교 |
| D. High-impact / adjacent VLA reference | [B43](https://doi.org/10.48550/arXiv.2504.16054) [π0.5](https://doi.org/10.48550/arXiv.2504.16054), [B44](https://doi.org/10.15607/RSS.2025.XXI.017) [OpenVLA-OFT](https://doi.org/10.15607/RSS.2025.XXI.017), [B99](https://doi.org/10.48550/arXiv.2603.15169) [ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169), [B48](https://doi.org/10.48550/arXiv.2507.09160) [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) | Generalist semantics, fast adaptation과 force/tactile-aware VLA가 해결한 범위를 인정 | Compute·data·task 조건이 다르면 literature comparison과 제한된 fine-tuning을 구분하고 전체 우열은 주장하지 않음 |

### 8.4 계열별 해결 범위와 남는 질문

| 계열 | 핵심 논문 | 확인된 장점 | Track B에서 남는 질문 |
| --- | --- | --- | --- |
| Generalist VLA | [B41](https://doi.org/10.48550/arXiv.2406.09246) [OpenVLA](https://doi.org/10.48550/arXiv.2406.09246), [B42](https://doi.org/10.48550/arXiv.2410.24164) [π0](https://doi.org/10.48550/arXiv.2410.24164), [B43](https://doi.org/10.48550/arXiv.2504.16054) [π0.5](https://doi.org/10.48550/arXiv.2504.16054), [B45](https://doi.org/10.48550/arXiv.2410.07864) [RDT-1B](https://doi.org/10.48550/arXiv.2410.07864) | 대규모 이종 robot data와 semantic prior를 이용한 다과업·다환경 transfer | Shelf 내부 unseen blocker의 국소 contact configuration과 Rotation→Push 전환을 제한된 실물 센서로 얼마나 정밀하게 다루는가 |
| Fast·efficient VLA | [B44](https://doi.org/10.15607/RSS.2025.XXI.017) [OpenVLA-OFT](https://doi.org/10.15607/RSS.2025.XXI.017), [B59](https://doi.org/10.52202/085713-3276) [Fast-in-Slow](https://doi.org/10.52202/085713-3276), [B61](https://doi.org/10.52202/085713-5484) [VLA-Cache](https://doi.org/10.52202/085713-5484), [B64](https://doi.org/10.52202/085713-1122) [RTC](https://doi.org/10.52202/085713-1122) | 병렬 decoding, fast action module, caching과 asynchronous chunk execution으로 latency·control frequency를 개선 | 빠른 실행만으로 접촉 관측 가능성, force safety와 downstream contact feasibility까지 해결되는가 |
| Force·tactile VLA | [B46](https://doi.org/10.52202/085713-3124) [ForceVLA](https://doi.org/10.52202/085713-3124), [B99](https://doi.org/10.48550/arXiv.2603.15169) [ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169), [B47](https://doi.org/10.48550/arXiv.2503.08548) [TLA](https://doi.org/10.48550/arXiv.2503.08548), [B48](https://doi.org/10.48550/arXiv.2507.09160) [Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160), [B49](https://doi.org/10.48550/arXiv.2505.09577) [VTLA](https://doi.org/10.48550/arXiv.2505.09577), [B50](https://doi.org/10.48550/arXiv.2512.23864) [DreamTacVLA](https://doi.org/10.48550/arXiv.2512.23864), [B51](https://doi.org/10.48550/arXiv.2601.20321) [TaF-VLA](https://doi.org/10.48550/arXiv.2601.20321), [B52](https://doi.org/10.48550/arXiv.2603.15257) [HapticVLA](https://doi.org/10.48550/arXiv.2603.15257) | Force/tactile을 VLA의 명시적 modality 또는 학습 supervision으로 도입하고, ForceVLA2는 이를 active hybrid force–position action으로 확장 | Specialized sensor와 task-specific multimodal demonstration 비용 없이 coarse binary tactile+F/T로 preparatory rotation과 pushing을 연결할 수 있는가 |
| Demonstration IL | [B54](https://doi.org/10.48550/arXiv.2304.13705) [ACT](https://doi.org/10.48550/arXiv.2304.13705), [B55](https://doi.org/10.15607/RSS.2023.XIX.026) [Diffusion Policy](https://doi.org/10.15607/RSS.2023.XIX.026), [B45](https://doi.org/10.48550/arXiv.2410.07864) [RDT-1B](https://doi.org/10.48550/arXiv.2410.07864) | 자연스러운 multimodal trajectory, action chunk와 expressive action distribution을 reward engineering 없이 학습 | Demonstration 밖의 접촉 이탈·물성 변화·실패 상태를 어떻게 탐색하고 회복할 것인가 |
| Reactive multimodal IL | [B56](https://doi.org/10.15607/RSS.2025.XXI.052) [RDP](https://doi.org/10.15607/RSS.2025.XXI.052), [B57](https://doi.org/10.1109/LRA.2025.3560871) [FoAR](https://doi.org/10.1109/LRA.2025.3560871), [B58](https://doi.org/10.48550/arXiv.2410.24091) [3D-ViTac](https://doi.org/10.48550/arXiv.2410.24091) | 고주파 tactile/F/T feedback과 vision을 결합해 action chunk 중 반응성과 정밀 접촉을 개선 | 별도 tactile teleoperation·task demonstrations가 필요한 조건과, Approach·Rotation 상태를 후속 Push success에 맞게 직접 학습하는지는 별개인가 |
| Contact-rich simulation RL | [B73](https://doi.org/10.1109/LRA.2025.3551637) [FORGE](https://doi.org/10.1109/LRA.2025.3551637), [B74](https://doi.org/10.48550/arXiv.2502.15442) [Privileged Action](https://doi.org/10.48550/arXiv.2502.15442), [B75](https://doi.org/10.48550/arXiv.2603.15789) [OmniReset](https://doi.org/10.48550/arXiv.2603.15789), [B76](https://doi.org/10.48550/arXiv.2510.11019) [Refinery](https://doi.org/10.48550/arXiv.2510.11019), [B78](https://doi.org/10.1109/LRA.2026.3681156) [MSDP](https://doi.org/10.1109/LRA.2026.3681156) | Force limit·randomization, privileged curriculum, reset coverage, active fine-tuning과 multisensory pretraining으로 탐색·Sim-to-Real·sensor fusion을 개선 | Phase-gated reward의 이득이 reset/curriculum·randomization·representation을 통제한 뒤에도 남는가 |
| IL–RL·Sim–Real hybrid | [B77](https://doi.org/10.1109/LRA.2025.3596487) [DP-RRL](https://doi.org/10.1109/LRA.2025.3596487), [B80](https://doi.org/10.48550/arXiv.2405.10315) [TRANSIC](https://doi.org/10.48550/arXiv.2405.10315) | Demonstration 또는 simulation base policy에 residual RL·human correction을 더해 real-world contact error를 보정 | 순수 simulation RL의 data·engineering 비용과 소량의 real correction을 쓰는 hybrid method 중 무엇이 더 효율적인가 |
| RL-refined VLA | [B53](https://doi.org/10.48550/arXiv.2606.09337) [TORL-VLA](https://doi.org/10.48550/arXiv.2606.09337), [B60](https://doi.org/10.52202/085713-5128) [SafeVLA](https://doi.org/10.52202/085713-5128), [B79](https://doi.org/10.15607/RSS.2025.XXI.019) [ConRFT](https://doi.org/10.15607/RSS.2025.XXI.019) | Offline VLA에 online RL 적응, Q learning 또는 명시적 safety constraint를 결합 | VLA+RL이라는 조합 자체는 gap이 아니며, shelf blocker의 어떤 state transition과 sensor·reward contract를 새로 검증하는가 |
| Closest task-specific systems | [B01](https://doi.org/10.48550/arXiv.2509.18455) [GD2P](https://doi.org/10.48550/arXiv.2509.18455), [B22](https://openreview.net/forum?id=dT3ZciXvNX) [DexMove](https://openreview.net/forum?id=dT3ZciXvNX), [B26](https://doi.org/10.48550/arXiv.2503.16806) [DyWA](https://doi.org/10.48550/arXiv.2503.16806) | Dexterous pushing/pulling contact pose, tactile wrist–finger nonprehensile control과 dynamics-adaptive manipulation을 각각 해결 | Preparatory Rotation의 종료 접촉을 downstream Push 초기 접촉으로 직접 최적화하는가, 그리고 coarse deployable sensing으로 가능한가 |
| Physical prerequisite 평가 | [B62](https://doi.org/10.52202/085713-4561) [VLA-OS](https://doi.org/10.52202/085713-4561), [B63](https://doi.org/10.52202/085713-2137) [BridgeVLA](https://doi.org/10.52202/085713-2137), [B65](https://doi.org/10.52202/085713-3671) [PAC Bench](https://doi.org/10.52202/085713-3671) | Hierarchical planning, 3D alignment와 물리적 실행 전제의 평가 필요성을 체계화 | Semantic plan과 coarse geometry가 실제 contact feasibility·stability·force constraint를 충분히 표현하는가 |

현재 가장 방어 가능한 gap은 `VLA는 힘을 모른다` 또는 `IL은 반응하지 못한다`가 아니다. 최신 반례가 이미 존재한다. 현재 문헌에서 직접 평가가 부족한 조합은 다음과 같이 더 좁게 정의한다.

> **Unknown shelf blocker의 선택된 OBB 면을 목표 방향에 정렬하는 preparatory rotation을 수행하면서, Approach의 hand configuration과 Rotation의 terminal contact가 최종 pushing까지 실행 가능하도록 유지·전환하고, 이를 coarse OBB·binary tactile·wrist F/T라는 배포 가능한 저차원 입력과 simulation privileged supervision으로 학습할 수 있는가?**

우리 방법의 우위 주장은 matched baseline, sensor·history ablation, unseen geometry·friction·mass 평가와 데이터·계산·센서 비용 비교가 완료된 뒤에만 사용한다. 그 전에는 `극복한다`가 아니라 `이 공백을 겨냥한다` 또는 `검증한다`로 서술한다.

## 9. 연구 배경 — Conventional Method와 Learning Method의 Trade-off

| ID | 연구 | Motivation에서의 역할 |
| --- | --- | --- |
| [B67](https://doi.org/10.15607/RSS.2024.XX.132) | [Tight Convex Relaxations](https://doi.org/10.15607/RSS.2024.XX.132) | Contact mode와 quasi-static dynamics를 명시하면 global contact planning이 강력할 수 있으므로 conventional planning을 무능한 baseline으로 서술하지 않을 근거 |
| [B68](https://doi.org/10.15607/RSS.2026.XXII.061) | [Distributionally Robust Control](https://doi.org/10.15607/RSS.2026.XXII.061) | Model-based method의 효율·신뢰성과 contact uncertainty 표현 한계 사이의 trade-off를 최신 사례로 설명 |
| [B69](https://doi.org/10.15607/RSS.2026.XXII.190) | [Certifiable Gradient-Based Contact-Rich Manipulation](https://doi.org/10.15607/RSS.2026.XXII.190) | Hybrid contact dynamics의 불연속 gradient와 smoothing-induced model mismatch가 현재도 핵심 문제임을 보여주는 사례 |
| [B66](https://doi.org/10.15607/RSS.2023.XIX.039) | [IndustReal](https://doi.org/10.15607/RSS.2023.XIX.039) | Contact-rich RL의 simulation 학습과 real transfer 가능성, 동시에 simulation-aware update·reward·curriculum·deployment 보정이 필요한 사례 |
| [B70](https://doi.org/10.48550/arXiv.2310.16917) | [MimicTouch](https://doi.org/10.48550/arXiv.2310.16917) | Tactile IL의 장점과 human–robot embodiment·demonstration sensing mismatch를 residual RL로 보완한 사례 |
| [B71](https://doi.org/10.15607/RSS.2026.XXII.004) | [Semantic Contact Fields](https://doi.org/10.15607/RSS.2026.XXII.004) | VLA의 semantic knowledge와 고충실도 physical grounding 사이의 간극, real tactile scale과 sim-to-real 문제를 직접 지적한 최신 사례 |
| [B72](https://doi.org/10.48550/arXiv.2412.09743) | [Planner-generated Contact-Rich Policy Learning](https://doi.org/10.48550/arXiv.2412.09743) | Human teleoperation이 어려운 multi-contact task에서 planning data를 활용할 수 있지만 demonstration consistency가 별도 병목임을 보여주는 사례 |
| [B96](https://doi.org/10.1109/ICRA48506.2021.9561221) | [Physics-Based Adaptive Motion Primitives](https://doi.org/10.1109/ICRA48506.2021.9561221) | Physics simulation과 제한된 motion primitive를 multi-heuristic search에 결합하면 clutter planning을 효율화할 수 있지만, 사전에 정한 primitive와 simulator query에 의존하는 대표 사례 |
| [B97](https://doi.org/10.1109/ICRA48506.2021.9562061) | [Dexterous Manoeuvre through Touch](https://doi.org/10.1109/ICRA48506.2021.9562061) | Tactile representation과 RL이 2021년부터 cluttered nonprehensile manipulation에 사용되었음을 보여주는 direct 사례 |
| [B98](https://doi.org/10.1109/ICRA48506.2021.9561734) | [Multimodal Contact-Rich Skills from Demonstrations](https://doi.org/10.1109/ICRA48506.2021.9561734) | Multimodal sensing과 learning from demonstration이 reward-engineering 부담의 대안으로 2021년부터 제안되었음을 보여주는 adjacent IL 사례 |

## 10. Reward Formulation — Pushing과 Rotation/Pivoting

논문별 reward 항, 그 항이 해결하려는 failure, Track B에 가져올 수 있는 부분과 그대로 복사하면 안 되는 조건은 [Reward formulation 문헌 비교](./reward_formulation.md)에 정리했다.

| 목적 | 우선 논문 | 핵심 비교 질문 |
| --- | --- | --- |
| 최신 pushing·pivoting 공통 formulation | [B81](https://doi.org/10.1109/LRA.2026.3655262) | Task progress·success·smoothness만 둔 RL과 kinematic/dynamic demonstration reward를 더한 RL의 차이는 무엇인가? |
| Goal-conditioned pushing | [B10](https://doi.org/10.1109/LRA.2023.3295236), [B82](https://doi.org/10.1109/ICRA55743.2025.11128166), [B83](https://doi.org/10.3389/fnbot.2023.1271607), [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | 거리, 방향, 접촉, 힘의 작용선, collision·toppling을 왜 reward 또는 constraint로 구분했는가? |
| Target-orientation pivoting | [B81](https://doi.org/10.1109/LRA.2026.3655262), [B85](https://doi.org/10.1109/ICRA48891.2023.10161271), [B86](https://doi.org/10.48550/arXiv.1703.00472) | 목표 각도 오차만으로 충분한가, contact dynamics와 action smoothness가 언제 필요한가? |
| Continuous rotation | [B32](https://doi.org/10.15607/RSS.2023.XIX.036), [B40](https://doi.org/10.48550/arXiv.2309.09979) | 회전량·angular velocity reward와 terminal orientation reward를 어떻게 구분해야 하는가? |
| Contact 유지와 configuration 전환 | [B87](https://doi.org/10.48550/arXiv.2509.17812), [B88](https://doi.org/10.1109/LRA.2026.3677744), [B89](https://doi.org/10.1109/ICRA57147.2024.10611300) | 전체 hand–object 지지는 유지하면서 개별 접촉의 release·re-contact와 작은 joint adaptation을 어느 정도 허용해야 하는가? |
| Track B transition | 위 전체 | Approach contact success와 Rotation face-alignment success를 최종 Push feasibility에 잇는 reward·terminal metric이 기존 연구에 실제로 존재하는가? |

---
