# Track B Literature Reading Guide

> [Literature Map](./README.md) · [Intro](../Intro/README.md) · [Previous Works 비교](../Intro/previous_works.md) · [핵심 B-ID 목록](./core_papers.md) · [목적별 그룹](./topic_groups.md) · [Conference Screening](./screening/README.md)

이 문서는 논문을 최신순이나 B-ID순으로 읽지 않고, **하나의 연구 질문에 필요한 전제를 차례로 확인하는 순서**를 제공한다. 각 경로의 마지막에는 독해 후 남겨야 할 결과를 명시한다.

---

## 1. Motivation을 검증하는 독해 경로

### Step 1. 상위 task category와 method trend를 먼저 구분한다

먼저 [`../Intro/research_trend.md`](../Intro/research_trend.md)를 읽고, 2021년의 [Physics-Based Adaptive Motion Primitives](https://doi.org/10.1109/ICRA48506.2021.9561221)·[Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471)에서 2025–2026년의 [HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154)·[Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052)·[ForceVLA](https://doi.org/10.52202/085713-3124)·[Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)로 이어지는 변화를 비교한다.

확인할 내용은 planning/control, RL, IL와 VLA가 서로 일방적으로 대체된 것이 아니라 physical model, demonstration prior, semantic prior와 interaction learning을 서로 다른 비율로 결합해 왔다는 점이다. RL 선택은 보편적 우위가 아니라 `명확한 low-level goal + 불확실한 contact outcome + 풍부한 simulation interaction + 제한된 real demonstration`이라는 현재 조건에서 정당화한다.

그다음 [`../Intro/previous_works.md`](../Intro/previous_works.md)의 Environment–Agent–System 표에서 closest systems만 비교한다. 여기서 contact-rich manipulation은 별도 task category가 아니라 friction, contact transition과 force transmission uncertainty가 만드는 interaction/control challenge로 해석한다.

### Step 2. Geometry uncertainty를 처리하는 대안을 비교한다

[GD2P](https://doi.org/10.48550/arXiv.2509.18455), [DyWA](https://doi.org/10.48550/arXiv.2503.16806), [PIN-WM](https://doi.org/10.15607/RSS.2025.XXI.153), [Visuotactile Estimation and Control](https://doi.org/10.48550/arXiv.2412.13157)과 [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)을 읽는다.

정확하거나 dense한 geometry를 사용하는 방법, visual history·world model로 dynamics를 추론하는 방법, tactile·F/T로 execution 중 오차를 보완하는 방법을 분리한다. 이 비교를 거쳐야 `approximate OBB + low-dimensional contact feedback`이 단순한 입력 축소인지, 성능·데이터·계산량 사이의 의미 있는 설계점인지 판단할 수 있다.

### Step 3. Shelf blocker가 상위 문제에서 어떤 역할을 하는가

[RetrDex](https://doi.org/10.48550/arXiv.2502.18423), [Occlusion-Aware Search](https://doi.org/10.1109/IROS51168.2021.9636230), [Parallel MCTS Retrieval](https://doi.org/10.1109/IROS47612.2022.9981962)과 [Unknown Object Retrieval](https://doi.org/10.1109/ICRA57147.2024.10611541)을 읽는다.

확인할 내용은 blocker 조작이 target visibility, reachability와 retrieval success에 어떻게 기여하는지, 그리고 blocker 선택·순서 결정이 상위 planner에 얼마나 남아 있는지다.

### Step 4. Direct push와 preparatory rotation의 경계를 찾는다

[Learning Contact Locations](https://doi.org/10.1109/HUMANOIDS.2013.7030011), [Learning Generalizable Pivoting](https://doi.org/10.1109/ICRA48891.2023.10161271), [Goal-Oriented Non-Prehensile Pushing](https://doi.org/10.1109/IROS47612.2022.9981873)과 [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166)을 비교한다.

여기서는 어떤 initial pose, geometry, 접근 방향과 contact condition에서 straight pushing이 실패하고 rotation이 필요한지를 추출한다.

### Step 5. 좋은 hand configuration을 어떻게 평가하는지 본다

[GD2P](https://doi.org/10.48550/arXiv.2509.18455) → [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652) → [RL-Critic Grasp Selection](https://doi.org/10.1109/ICRA55743.2025.11127792) → [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987)의 순서로 읽는다.

이 순서는 geometry-conditioned pre-contact pose, task-wrench capability, downstream critic scoring, transition feasibility로 평가 관점이 확장되는 흐름이다. 정적 pose score와 실제 Rotation→Push continuation success를 동일시하지 않는다.

이어서 [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744) → [Guided Exploration with Sub-skill Controllers](https://doi.org/10.1109/ICRA57147.2024.10611300) → [Tac2Motion](https://doi.org/10.48550/arXiv.2509.17812)을 비교한다. 첫 논문은 task-informed 초기 grasp와 작은 online adaptation의 결합을, 뒤의 두 논문은 contact switching과 firm-contact 유지가 함께 필요한 조건을 보여준다. 여기서 검증할 명제는 `Approach 후 손 자세를 고정해야 한다`가 아니라 `전체 접촉 지지는 유지하되 성공에 필요하지 않은 재구성은 줄여야 한다`이다.

### Step 6. Contact feedback이 무엇을 보완하는지 확인한다

[Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135), [DexTouch](https://doi.org/10.1109/LRA.2024.3478571), [DexMove](https://openreview.net/forum?id=dT3ZciXvNX), [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036)과 [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)을 읽는다.

목표는 `tactile/F/T가 유용하다`는 일반론이 아니라, coarse geometry와 vision만으로 관측되지 않는 contact location, contact mode, resultant wrench와 temporal state 중 무엇을 각 센서가 제공하는지 구분하는 것이다. High-resolution tactile, binary tactile와 wrist F/T를 동일한 정보로 취급하지 않는다.

### Step 7. Sequence와 transition 주장의 경계를 확인한다

[HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154), [SPIN](https://doi.org/10.48550/arXiv.2502.18015), [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987)과 [Privileged Action](https://doi.org/10.48550/arXiv.2502.15442)을 비교한다.

Shared policy, phase label 제거 또는 여러 동작을 한 episode에 넣는 것 자체는 contribution이 아니다. 우리 방법에 explicit downstream-feasibility objective가 없다면 Rotation-to-Push는 우선 분리된 evaluation metric으로만 유지한다.

### 이 경로의 산출물

`task category → uncertainty 처리 전략 → application boundary → direct-push failure → rotation 필요성 → downstream contact gap → sensing 역할`이 이어지는 비교표를 만든다. 이 연결이 성립하기 전에는 rotate-then-push, multimodal sensing과 multi-finger control의 결합 자체를 novelty로 주장하지 않는다.

---

## 2. Observation을 검증하는 독해 경로

현재 baseline은 `target position + push direction + selected-face normal + current object pose + coarse OBB + arm/hand q + current 17D binary tactile + current wrist F/T + previous action 1-step`의 66D 입력이다. 다음 순서는 각 입력이 왜 필요한지와 무엇을 제외할지를 판단하기 위한 것이다.

1. **Task command와 current state의 분리:** [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236)과 [Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471)에서 pusher/EEF-relative goal, object state와 local contact feature의 역할을 구분하고, Track B에서는 목표 quaternion 대신 target position·fixed push direction·selected-face normal을 사용하는 이유를 검토한다.
2. **Current binary tactile의 근거와 반례:** [DexTouch](https://doi.org/10.1109/LRA.2024.3478571)에서 history 없는 spatial binary tactile를, [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036)에서 finite state stack이 필요했던 조건을 비교한다.
3. **구현 가능한 interface:** [Isaac Lab](https://doi.org/10.48550/arXiv.2511.04831)에서 ContactSensor, joint wrench, 4D quaternion, `last_action`과 observation history의 실제 API 경계를 확인한다.
4. **Vision 불확실성과 F/T history:** [Visuotactile Estimation and Control](https://doi.org/10.48550/arXiv.2412.13157)에서 occlusion, latency, validity와 temporal force inference가 필요한 이유를 확인한다.
5. **고차원 tactile의 상한:** [VTDexManip](https://openreview.net/forum?id=jf7C7EGw21), [Robot Synesthesia](https://doi.org/10.1109/ICRA57147.2024.10610532), [TacSL](https://doi.org/10.1109/TRO.2025.3547267)과 [TacEx](https://doi.org/10.48550/arXiv.2411.04776)를 통해 optical/spatial tactile가 추가로 주는 정보와 구현 비용을 비교한다.
6. **History와 latent dynamics:** [RoboPack](https://doi.org/10.15607/RSS.2024.XX.130)과 [DyWA](https://doi.org/10.48550/arXiv.2503.16806)에서 action–contact history가 물성·동역학 추론에 필요한 조건을 확인한다.
7. **Orientation 표현:** [General In-Hand Re-Orientation](https://doi.org/10.48550/arXiv.2111.03043), [RotateIt](https://doi.org/10.48550/arXiv.2309.09979)과 [Continuity of Rotation Representations](https://doi.org/10.1109/CVPR.2019.00589)을 통해 task goal과 neural-network representation을 구분한다.

### 이 경로의 산출물

각 observation에 대해 `필요한 task information / sensor source / policy 표현 / sim–real 대응 / 제외 시 예상 failure / ablation`을 한 행으로 기록한다. 논문의 threshold나 history 길이는 복사하지 않고 실제 hardware rate와 calibration으로 다시 정한다.

---

## 3. Reward를 검증하는 독해 경로

세부 비교표는 [`reward_formulation.md`](./reward_formulation.md)를 기준으로 한다. 아래 순서는 term을 많이 수집하기 위한 것이 아니라, Track B reward의 각 층을 순서대로 정당화하기 위한 것이다.

1. **Task progress와 success:** [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)에서 pushing·pivoting의 공통 task progress, sparse success와 demonstration-conditioned reward를 확인한다.
2. **Gate가 필요한 이유:** [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236)에서 goal distance에 따라 orientation·position reward의 의미를 바꾼 이유를 본다.
3. **Shaping과 safety의 분리:** [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166)에서 OBB goal, surface exploration과 action rate를 collision·toppling constraint와 구분한 이유를 확인한다.
4. **Rotation goal의 종류:** [Rotating without Seeing](https://doi.org/10.15607/RSS.2023.XIX.036), [Learning Generalizable Pivoting](https://doi.org/10.1109/ICRA48891.2023.10161271)과 [RotateIt](https://doi.org/10.48550/arXiv.2309.09979)을 비교해 target orientation과 continuous rotation reward를 구분한다.
5. **Privileged force의 용도:** [Adaptive Reaching and Pushing](https://doi.org/10.3389/fnbot.2023.1271607)과 [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262)에서 force magnitude보다 direction·lever arm과 safety 결과를 사용한 이유를 본다.
6. **Downstream feasibility:** [RL-Critic Grasp Selection](https://doi.org/10.1109/ICRA55743.2025.11127792), [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987)과 [Value-Informed Skill Chaining](https://doi.org/10.1109/IROS55552.2023.10342180)을 통해 이전 상태를 후속 성공으로 평가하는 근거와 critic overestimation 위험을 확인한다.

### 이 경로의 산출물

Reward 검토 결과는 `term 이름`이 아니라 다음 형식으로 남긴다.

> `해결할 failure → 계산 신호 → actor/privileged 경계 → 활성 조건 → 예상 부작용 → 검증할 ablation`

기존 논문의 모든 항을 합치지 않는다. Final task success를 먼저 고정하고, phase shaping, safety constraint, regularization과 downstream candidate를 한 층씩 추가한다.

---

## 4. 논문 한 편을 기록하는 공통 형식

새 논문을 읽을 때 다음 순서로 요약한다.

1. 논문이 실제로 해결한 task와 environment
2. Object·scene·robot·sensor 조건
3. Observation과 전처리
4. Action과 controller
5. Reward·loss·training data
6. Contact representation과 phase transition
7. Generalization과 real validation
8. Track B에 직접 가져올 수 있는 근거
9. 그대로 이식할 수 없는 조건

이 형식으로 확인한 뒤에만 [`topic_groups.md`](./topic_groups.md)의 해석이나 [`reward_formulation.md`](./reward_formulation.md)의 설계 근거로 사용한다.
