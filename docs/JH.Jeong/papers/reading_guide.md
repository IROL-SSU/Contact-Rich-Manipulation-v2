# 우선 독해 순서와 검토 관점

> [Paper Index](./README.md) · [핵심 B-ID 목록](./core_papers.md) · [목적별 그룹](./topic_groups.md) · [Conference Screening](./screening/README.md)

## 1. Research motivation 구체화

단순히 유사한 논문을 많이 모으는 것이 아니라 다음 네 축의 비교표를 만드는 것이 목적이다.

1. **Shelf retrieval의 실제 필요:** [B17](https://doi.org/10.48550/arXiv.2502.18423) [RetrDex](https://doi.org/10.48550/arXiv.2502.18423), [IROS21-081](https://doi.org/10.1109/IROS51168.2021.9636230) [Occlusion-Aware Search](https://doi.org/10.1109/IROS51168.2021.9636230), [IROS22-016](https://doi.org/10.1109/IROS47612.2022.9981962) [parallel MCTS retrieval](https://doi.org/10.1109/IROS47612.2022.9981962)과 [ICRA24-169](https://doi.org/10.1109/ICRA57147.2024.10611541) [Unknown Object Retrieval](https://doi.org/10.1109/ICRA57147.2024.10611541)을 통해 blocker 조작이 target visibility·reachability·retrieval에 주는 효용과 상위 planner의 범위를 확인한다.
2. **Direct push와 preparatory rotation의 경계:** [B21](https://doi.org/10.1109/HUMANOIDS.2013.7030011) [Hermans et al.](https://doi.org/10.1109/HUMANOIDS.2013.7030011), [ICRA23-103](https://doi.org/10.1109/ICRA48891.2023.10161271) [Learning Generalizable Pivoting](https://doi.org/10.1109/ICRA48891.2023.10161271), [IROS22-013](https://doi.org/10.1109/IROS47612.2022.9981873) [Goal-Oriented Non-Prehensile Pushing](https://doi.org/10.1109/IROS47612.2022.9981873)과 [ICRA25-189](https://doi.org/10.1109/ICRA55743.2025.11128166) [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166)을 비교해 어떤 초기 pose·geometry·contact 조건에서 rotation이 필요한지 정리한다.
3. **Contact configuration과 phase 연결:** [B01](https://doi.org/10.48550/arXiv.2509.18455) [GD2P](https://doi.org/10.48550/arXiv.2509.18455), [B02](https://doi.org/10.1109/IROS58592.2024.10802652) [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652), [B03](https://doi.org/10.1109/ICRA55743.2025.11127792) [critic-based grasp scoring](https://doi.org/10.1109/ICRA55743.2025.11127792), [B08](https://doi.org/10.48550/arXiv.2309.00987) [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987)와 [ICRA25-147](https://doi.org/10.1109/ICRA55743.2025.11128462) [impedance-primitive HRL](https://doi.org/10.1109/ICRA55743.2025.11128462)을 비교해 rotation terminal state가 후속 pushing 실행 가능성을 어떻게 보존해야 하는지 검토한다.
4. **Multimodal feedback의 필요:** [B09](https://doi.org/10.1109/LRA.2024.3478571) [DexTouch](https://doi.org/10.1109/LRA.2024.3478571), [IROS21-003](https://doi.org/10.1109/IROS51168.2021.9636836) [COCOI](https://doi.org/10.1109/IROS51168.2021.9636836), [B32](https://doi.org/10.15607/RSS.2023.XIX.036) [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036)와 [ICRA25-166](https://doi.org/10.1109/ICRA55743.2025.11127409) [tactile sensing 비교](https://doi.org/10.1109/ICRA55743.2025.11127409)를 통해 coarse geometry·vision만으로 남는 불확실성과 tactile/F/T가 실제로 보완하는 정보를 구분한다.

각 논문은 `문제 설정 / object·scene 조건 / observation / action·controller / contact representation / rotation–translation 관계 / downstream objective / generalization / real validation / 우리 gap에 주는 근거` 열로 정리한다. 이 비교가 끝나기 전에는 `rotate-then-push`, `multimodal`, `multi-finger` 또는 `phase-free RL`의 결합만으로 novelty를 주장하지 않는다.

## 2. Observation formulation

현재 Track B 1단계의 **coarse OBB + arm/hand q + binary tactile + wrist F/T + MLP history** observation을 구체화하기 위한 순서다. 최종 실험 baseline 선정은 아니다. Point cloud·raw RGB·optical tactile는 현재 actor 최소안이 아니라 비교 배경 또는 후속 확장으로 읽는다.

1. **[B09](https://doi.org/10.1109/LRA.2024.3478571) [DexTouch](https://doi.org/10.1109/LRA.2024.3478571) → [B32](https://doi.org/10.15607/RSS.2023.XIX.036) [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036):** Binary tactile와 spatial coverage의 이유를 [B09](https://doi.org/10.1109/LRA.2024.3478571)에서, previous controller target과 finite history의 이유를 [B32](https://doi.org/10.15607/RSS.2023.XIX.036)에서 확인
2. **[B10](https://doi.org/10.1109/LRA.2023.3295236) [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236) → [B37](https://doi.org/10.1109/TRO.2021.3104471) [Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471):** Pusher/EEF-relative goal과 local contact state를 분리하는 이유, raw tactile image와 contact-pose feature의 차이
3. **[B35](https://doi.org/10.48550/arXiv.2511.04831) [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831):** URDF link/pad ContactSensor, joint wrench와 ObservationManager history의 실제 구현 경계
4. **[B12](https://doi.org/10.48550/arXiv.2412.13157) [Visuotactile Estimation and Control](https://doi.org/10.48550/arXiv.2412.13157):** Continuous vision의 occlusion·latency와 force history, validity·uncertainty를 처리하는 이유
5. **[B27](https://openreview.net/forum?id=jf7C7EGw21) [VTDexManip](https://openreview.net/forum?id=jf7C7EGw21):** Image–ResNet과 binary tactile–MLP를 분리한 근거 및 raw image를 현재안에서 제외할 근거
6. **[B33](https://doi.org/10.1109/ICRA57147.2024.10610532) [Robot Synesthesia](https://doi.org/10.1109/ICRA57147.2024.10610532):** Spatial tactile/point-cloud 표현이 coarse binary보다 주는 정보와 추가 구현 비용의 비교 배경
7. **[B15](https://doi.org/10.15607/RSS.2024.XX.130) [RoboPack](https://doi.org/10.15607/RSS.2024.XX.130) → [B26](https://doi.org/10.48550/arXiv.2503.16806) [DyWA](https://doi.org/10.48550/arXiv.2503.16806):** Action–contact history로 관측되지 않는 물성·동역학을 추론하는 이유와 recurrent model의 역할
8. **[B22](https://openreview.net/forum?id=dT3ZciXvNX) [DexMove](https://openreview.net/forum?id=dT3ZciXvNX) → [B34](https://doi.org/10.1109/TRO.2025.3547267) [TacSL](https://doi.org/10.1109/TRO.2025.3547267) → [B36](https://doi.org/10.48550/arXiv.2411.04776) [TacEx](https://doi.org/10.48550/arXiv.2411.04776):** 고차원 tactile·optical tactile가 제공할 수 있는 상한과 현재 ContactSensor baseline의 차이
9. **[B39](https://doi.org/10.48550/arXiv.2111.03043) [General In-Hand Re-Orientation](https://doi.org/10.48550/arXiv.2111.03043) → [B40](https://doi.org/10.48550/arXiv.2309.09979) [RotateIt](https://doi.org/10.48550/arXiv.2309.09979) → [B38](https://doi.org/10.1109/CVPR.2019.00589) [Rotation Representation](https://doi.org/10.1109/CVPR.2019.00589):** Arbitrary orientation goal, continuous-axis rotation goal과 neural-network용 SO(3) 표현을 구분

현재 tactile 관련 직접 ablation은 `F/T only`, `URDF coarse M-region binary + F/T`, `17-channel binary + F/T`다. 논문의 history 길이와 force threshold는 출발 근거일 뿐 그대로 복사하지 않고 실제 policy rate·sensor latency·hardware calibration으로 정한다.

기존 [GD2P](https://doi.org/10.48550/arXiv.2509.18455)·[Hermans et al.](https://doi.org/10.1109/HUMANOIDS.2013.7030011)·[TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652) 중심의 baseline 독해는 폐기하지 않으며, observation 명세 후 reward와 접촉 구성 설계를 진행할 때 이어간다.

## 3. Reward formulation

세부 비교표는 [Reward formulation 문헌 비교](./reward_formulation.md)를 기준으로 한다. 우선순위는 단순히 최신순이 아니라 Track B에서 답해야 할 질문 순서다.

1. **[B81](https://doi.org/10.1109/LRA.2026.3655262):** 2026년 IEEE RA-L의 pushing·pivoting 공통 reward, CITO 시연의 contact·force direction을 reward로 쓰는 이유와 magnitude를 제외한 이유
2. **[B10](https://doi.org/10.1109/LRA.2023.3295236):** Goal에서 멀 때 orientation, 가까울 때 distance로 전환하고 normal contact term을 항상 유지한 이유
3. **[B82](https://doi.org/10.1109/ICRA55743.2025.11128166):** OBB keypoint pose reward, direction-only velocity, surface exploration curriculum과 collision·toppling constraint 분리
4. **[B32](https://doi.org/10.15607/RSS.2023.XIX.036):** Continuous rotation에서 simulator angular velocity 대신 finite rotation angle을 사용하고 drift·fall·work·torque를 억제한 이유
5. **[B85](https://doi.org/10.1109/ICRA48891.2023.10161271):** Target orientation error 하나로 pivoting을 학습한 최소 formulation과 그 한계
6. **[B83](https://doi.org/10.3389/fnbot.2023.1271607) → [B84](https://doi.org/10.1109/IROS47612.2022.9981873):** Privileged contact-force direction·lever arm과 contact-maintenance·collision reward의 서로 다른 역할
7. **[B40](https://doi.org/10.48550/arXiv.2309.09979) → [B86](https://doi.org/10.48550/arXiv.1703.00472):** Continuous-axis rotation과 target-angle pivoting을 혼동하지 않기 위한 경계 사례

독해 결과는 `term 목록`이 아니라 `term → 해결 failure → 필요한 privileged information → 활성 조건 → 예상 부작용 → ablation`으로 기록한다. 기존 논문의 모든 항을 합치는 대신, Track B의 Approach·Rotation·Push와 transition에서 실제로 필요한 항만 단계적으로 검증한다.

---
