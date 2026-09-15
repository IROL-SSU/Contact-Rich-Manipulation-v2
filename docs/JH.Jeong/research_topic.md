# JH.Jeong Research Topic

> **연구 주제:** Shelf retrieval을 위한 goal-conditioned blocker-object contact manipulation
>
> **현재 중심 Track:** Track B
>
> **최종 갱신:** 2026-09-15

---

## 1. 문서 목적

이 문서는 연구 과정에서 **구체화되었거나 확인된 현재 내용**을 간결하게 정리한다. 논의의 흐름, 과거 제안, 작업 가설과 미결 사항의 상세한 기록은 [`context.md`](./context.md)에서 관리한다.

새로운 아이디어나 설계 후보는 합의되기 전까지 이 문서의 확정 내용으로 추가하지 않는다. 중요한 결정이 변경되면 `context.md`에 변경 배경과 의사결정 과정을 먼저 기록하고, 이 문서를 최신 결론에 맞게 갱신한다.

---

## 2. 연구 주제

선반 환경에서 target object의 관측이나 인출을 방해하는 blocker object를 재배치하여, target에 접근할 수 있는 공간 또는 경로를 확보한다.

연구의 중심은 continuous Vision으로 관측한 blocker의 현재 pose와 근사 geometry, 그리고 F/T·Tactile contact feedback을 이용하는 RL manipulation policy다. 먼저 주어진 회전·병진 목표를 접촉 불확실성 아래에서 안정적으로 실행하는 능력을 학습하고, 이후 공간 확보 목적에 필요한 조작 목표와 동작 전환을 정책이 결정하도록 담당 범위를 확장한다.

현재 조작 관점은 **다양한 초기 물체 자세에서, 목표 방향의 pushing을 위해 물체를 주어진 자세로 돌려두고 미는 것**이다. 회전 자체만이 목적이 아니라 후속 밀기를 위한 준비라는 점이 중요하다. 단, 1단계에서 어떤 물체 자세가 적합한지 선택하는 것은 상위 모듈의 역할이며, 우리 policy는 그 목표를 실행하기 위한 hand configuration과 접촉 상태를 형성·조절한다.

이를 한 문장으로 정리하면 다음과 같다.

> **지속적인 시각·근사 기하 관측과 접촉 감각을 이용해 blocker의 목표 조건부 회전·병진 조작을 학습하고, 이를 target 접근 공간 확보를 위한 조작 의사결정과 실행의 통합으로 확장한다.**

---

## 3. 응용 시나리오와 정책의 대상

- **Target object:** 사용자가 최종적으로 찾거나 꺼내려는 물체로, blocker에 의해 부분적으로 보이거나 가려질 수 있다.
- **Blocker object:** Target의 관측 또는 접근을 방해하며, manipulation policy가 재배치하는 물체다.
- **정책의 직접적인 조작 대상:** Blocker object
- **최종 응용 목적:** Blocker를 재배치하여 target의 노출량이나 접근 가능한 공간을 늘리고, 이후 인출이 가능한 상태를 만드는 것

현재 정책이 target retrieval 동작 자체를 수행하는 것은 아니다. 실제 target 인출은 blocker manipulation 이후의 별도 단계로 둔다.

---

## 4. 연구의 단계적 진행

### 4.1 1단계 — 주어진 조작 목표 실행

상위 판단기가 조작할 blocker와 목표 회전·병진 운동을 제공한다. Policy는 물체 근처에서의 접근과 접촉 형성부터 목표 물체 운동의 실행까지 담당한다.

**목표 선택과 실행의 경계:** 상위 모듈이 밀기에 적합한 목표 물체 orientation을 지정한다. Policy가 손목 pose·손가락 configuration·접촉 배치를 조절하는 것과 목표 물체 orientation 자체를 선택하는 것은 별개다. `적합한 물체 자세를 형성한다`는 표현은 1단계에서 **주어진 자세를 실제로 달성한다**는 뜻으로만 사용한다.

기본 동작 순서는 다음과 같다.

> **Approach / Contact Formation → Rotation → Push / Translation**

| 단계 | 물체 수준 목표 | Hand의 역할 |
| --- | --- | --- |
| Approach / Contact Formation | 후속 조작에 유효한 접촉 형성 | 근사 geometry를 이용해 손목 pose와 손가락 구성을 준비하고, 실제 접촉 feedback으로 보정 |
| Rotation | 요구된 blocker orientation 달성 | 필요한 회전 모멘트를 만들도록 접촉 위치와 힘 분포 조절 |
| Push / Translation | 목표 blocker position 도달 및 요구 orientation 유지 | 병진에 적합하게 접촉을 재조정하고 이동 방향·속도와 손 구성을 보정 |

Rotation과 Translation의 goal은 **blocker의 물체 상태에 대한 목표**이며, 손목의 자세 목표와 구분한다. 이 단계의 핵심 질문은 다음과 같다.

> 주어진 회전·병진 목표를 근사 형상과 실제 접촉 상태의 불일치 아래에서도 정확하고 안정적으로 실행할 수 있는가?

Approach는 준비의 시작이며, Rotation과 Push에서도 접촉 구성의 조절·전환이 필요할 수 있다. 따라서 연구의 기여 후보를 최초 Approach에만 한정하지 않는다. 회전이 끝난 물체 pose뿐 아니라 후속 pushing에 유효한 손·접촉 상태도 고려하되, 각 phase의 별도 알고리즘이나 network를 확정하지 않는다.

### 4.2 2단계 — 공간 확보를 위한 조작 의사결정 통합

1단계에서 확보한 실행 능력을 기반으로, 상위 입력을 세부 motion instruction에서 목적 수준으로 추상화한다.

- **1단계 지시:** 지정된 회전·병진 목표를 실행한다.
- **2단계 목적:** 선택된 blocker를 조작하여 target에 접근할 공간 또는 경로를 확보한다.

2단계에서 정책의 담당 범위는 다음 의사결정까지 확장한다.

- 회전이 필요한지 또는 바로 Push할 수 있는지 판단
- 후속 이동에 적합한 중간 orientation과 접촉 배치 결정
- 이동 방향과 거리 결정
- Rotation과 Push 사이의 전환 시점 및 필요한 동작 순서 결정
- 접촉 재형성·접촉 전환·추가 조작의 필요 여부 판단
- 공간 확보가 충분한지와 manipulation 완료 여부 판단

1단계의 기본 동작 순서는 2단계의 고정 제약이 아니다. 2단계에서는 목적과 장면 조건에 따라 필요한 동작과 순서를 선택하는 방향으로 확장한다.

---

## 5. 관측 정보와 센서 역할

Track B는 manipulation 전 과정에서 Vision, wrist F/T와 tactile sensing을 함께 사용한다.

각 신호는 **선행연구의 raw input·전처리·representation, Isaac Lab의 원천 데이터와 가공 가능성, 실물 대응, 좌표계·주기·noise·latency와 history**를 함께 검토한다. Simulation의 exact state·contact를 실제 sensor observation과 구분하지 않고 actor에 넣지 않는다.

현재 policy는 MLP이며 actor에 phase ID를 주지 않는다. `Approach / Contact Formation → Rotation → Push`의 진행은 phase별 gate가 reward term을 활성화하는 방식으로 학습한다. Gate가 action 자유도를 phase별로 mask한다는 뜻은 아니다.

### 5.1 Goal과 Vision·Geometry

Goal은 일반적인 final object-pose reaching이 아니다. Primary objective는 지정 방향·거리의 pushing이고, 상위가 제공하는 object orientation은 그 pushing을 위한 준비 및 Push 중 유지 조건이다.

$$
p_{O,g}^{W}=p_{O,0}^{W}+s_{\mathrm{push}}d_{\mathrm{push}}^{W}
$$

Policy에는 현재 EEF frame으로 변환한 target object position과 preparatory object orientation을 goal로 주고, current EEF–object pose를 별도로 준다. 현재 working baseline은 움직이는 EEF에서 orientation 문맥을 보존하기 위해 current와 target rotation을 각각 6D로 표현한다.

$$
g_t^E=\left[p_{O,g}^{E},\rho_6(R_{EG,t})\right]\in\mathbb{R}^{9}
$$

여기서 $\rho_6(R)$는 rotation matrix의 첫 두 column을 펼친 6D continuous representation이다. Task objective와 reward는 shelf normal 주위의 preparatory yaw에 한정하지만, EEF가 roll/pitch할 수 있으므로 observation을 EEF-frame yaw scalar로 축소하지 않는다. EEF가 shelf normal에 항상 정렬되는 제한된 조건의 `sin/cos yaw`는 representation ablation으로 남긴다. 이 working baseline의 최종 사용자 확정은 남아 있다.

Unseen object와 occlusion을 고려해 raw RGB, point cloud와 mesh 대신 3D OBB를 coarse geometry로 사용한다. 조작 전 initial OBB의 object-local axes·extent를 episode template으로 고정하고 manipulation 중에는 pose만 tracking한다. Observation에는 object-local extent만 넣으며, EEF에서의 box-axis 방향은 current object orientation으로 결정한다.

$$
d_O=[l_O,w_O,h_O]\in\mathbb{R}^{3}
$$

### 5.2 Tactile와 Wrist F/T

기본 actor 조합은 **binary tactile + wrist F/T**다.

- 실제 RH56E2는 총 17개 tactile sensor를 사용하는 것으로 파악한다. 정확한 위치·packet은 장비와 URDF에서 확인한다.
- Taxel별 collision body 17개를 만드는 방법은 simulation 연산량 때문에 기본안에서 제외한다.
- 실제 17 sensor와 simulation의 contact-bearing URDF link/pad를 동일한 `M`개 coarse region으로 pooling한다.
- `M=17`의 세밀한 binary tactile와 `M<17`의 coarse binary tactile는 성능·robustness·simulation cost를 실험적으로 비교한다.
- Actor tactile는 접촉 상대의 identity를 구분하지 않는다. Object, shelf 또는 다른 body 중 무엇과 닿았든 해당 region에 접촉이 감지되면 `contact on`으로 둔다. Simulation의 collider identity는 reward·critic용 privileged information에서만 구분한다.
- Threshold는 문헌의 `0.01 N`을 복사하지 않고 실제 sensor의 no-contact/contact 분포에서 `τ_on>τ_off` hysteresis와 debounce를 정한다.
- Simulation continuous contact force는 actor tactile가 아니라 reward·asymmetric critic용 privileged information으로 사용한다. 과부하·충격·접촉 부족을 평가하되 관측 불가능한 정확한 국소 force 분포를 강제하지 않는다.

### 5.3 현재 최소 MLP Observation Working Baseline

아래 표는 최신 논의를 반영한 구현 기준안이며 rotation 선택은 최종 사용자 확정 전이다.

| Observation | 표현 | 차원 | 시간 범위 |
| --- | --- | ---: | --- |
| Push-conditioned goal | EEF-frame target object position 3D + target orientation 6D | 9 | Current |
| Current object pose | EEF-frame estimated object position 3D + current orientation 6D | 9 | Current |
| Coarse geometry | Episode-consistent object-local OBB extent | 3 | Episode-fixed |
| Arm configuration | UR5e measured joint position | 6 | Current |
| Hand configuration | RH56E2 actuated joint position | 6 | Current |
| Binary tactile | Real 17 sensor 또는 공통 coarse region의 on/off | `K_bM` | 최근 `K_b` step |
| Wrist F/T | Bias-compensated EEF-frame force·torque | `6K_w` | 최근 `K_w` step |
| Previous action | EEF delta pose 6D + hand joint action 6D | `12K_a` | 최근 `K_a` step |

MLP에는 history를 recurrent state로 숨기지 않고 과거 sensor/action 값을 flatten하여 observation으로 넣는다.

$$
D_{\mathrm{MLP}}=33+K_bM+6K_w+12K_a
$$

모든 modality에 같은 history window를 적용할 필연성은 없다. 공통 `K`는 시간 정렬과 첫 비교가 단순하다는 장점이 있으며, 최종 `K_b,K_w,K_a`는 실제 action-to-sensor latency와 contact transient의 시간 범위로 정한다.

현재 최소안에서는 phase ID, object velocity, arm/hand joint velocity, absolute EEF pose·twist, fingertip position, vision confidence/age, raw RGB, point cloud와 mesh를 제외한다. 실제 failure 분석에서 필요성이 확인되면 추가·ablation한다. Shelf·workspace pose나 clearance도 actor observation에는 주지 않고, simulation privileged collision state로 penalty·termination을 계산한다.

Continuous Vision을 사용하더라도 실제 국소 접촉 표면, 마찰, 질량 분포 등 전체 물리 상태를 정확히 안다고 가정하지 않는다. 접촉 전에는 OBB로 hand configuration을 준비하고, 접촉 후에는 binary tactile와 wrist F/T로 실제 상호작용에 맞게 보정한다.

### 5.4 선행연구 비교 후 검토 결과

아래 판정은 5.3절의 현재 최소안을 즉시 대체하는 확정 명세가 아니라, 유사 과업에서 각 observation을 넣은 **이유**를 기준으로 한 설계 검토 결과다.

| 검토 대상 | 문헌에서 필요했던 이유 | 현재 판단 |
| --- | --- | --- |
| EEF-frame goal + current EEF–object pose | Tactile pushing 연구는 global target 정렬과 local contact regulation을 분리했다. 상대 표현은 task-relevant error를 직접 제공하고 world-frame 의존성을 줄인다. | **유지** |
| Arm joint position | 동일한 EEF pose라도 arm configuration에 따라 joint-limit·singularity와 가능한 motion이 다르다. DiffIK/OSC가 하위 제어를 담당하더라도 actor가 configuration 차이를 구별할 최소 proprioception이 된다. | **유지**: current `q_A` 6D만 사용; `dq_A`와 history는 제외 |
| Hand joint position | Binary contact만으로는 같은 접촉에서도 손 형상을 구분할 수 없다. Dexterous tactile 연구는 접촉 위치와 hand configuration을 함께 사용했다. | **유지** |
| Spatial binary tactile history | Vision이 직접 보지 못하는 접촉 발생 위치와 contact transition을 보완한다. Full-hand coverage의 이득도 보고되었다. | **유지**. 다만 `M=17`과 coarse `M<17`을 비교 |
| Wrist F/T history | Binary tactile가 잃는 접촉 강도·방향과 action–response dynamics를 보완한다. | **유지**. Multi-contact hand에서는 tactile의 공간 identity를 대체하지는 않음 |
| OBB extent | Unseen shape를 위한 point cloud 연구는 surface-level action selection에 geometry가 필요했다. OBB는 그보다 안정적인 coarse prior지만 같은 extent의 서로 다른 형상을 구분하지 못한다. | **유지하되 주장 범위 제한**: coarse size·axis prior이며 fine geometry-aware 표현은 아님 |
| Previous action history | 최근 어떤 EEF/hand 명령 뒤에 tactile·wrench와 실제 motion이 어떻게 변했는지 해석하는 데 유용하다. 현재 action은 measured state 기준의 one-step delta이며 새 command가 이전 command를 대체하므로, controller 내부 target을 복원하기 위한 입력은 아니다. | **유지 후보 / ablation**: history 유무와 `K_a` 비교 |
| Vision validity·age | Tracker가 occlusion 중 마지막 pose를 유지할 때는 fresh/stale 구분에 유용하지만, tracking 성능 개선 자체는 현재 연구 범위가 아니다. | **Core observation에서 제외 유지**. 실제 tracker가 metadata를 제공하고 dropout 대응이 필요할 때 Sim-to-Real 확장으로 검토 |
| Hand velocity 또는 hand-q history | 실제 손 motion과 command의 차이를 구별해 actuator lag를 보완한다. | **Ablation**: `dq_H`와 q-history 중 작은 쪽부터 비교 |
| Object pose history 또는 filtered twist | Occlusion 중 물체 운동이나 동적 pushing을 추정할 때 유용하다. | **Ablation**: quasi-static baseline의 failure가 근거를 제공할 때 추가 |
| Shelf·workspace state | 경계 위치에 따른 collision 가능성을 사전에 구분할 수 있지만, 현재 policy에는 별도 scene observation을 제공하지 않기로 했다. | **제외**: shelf collision은 privileged penalty·termination으로 제약 |

Action은 DiffIK 또는 OSC가 현재 measured state에서 이번 step의 delta를 해석하고, 다음 policy action이 들어오면 새 command로 교체하는 one-step interface로 둔다. 이전 command가 만족되지 않았더라도 새 delta를 이전 desired target에 누적하지 않는다. 따라서 별도의 controller-target state는 현재 observation에서 제외한다. Previous-action history는 오직 actuator lag와 contact response를 해석하는 효과를 검증하기 위한 항목이다.

Simulation에서는 정확한 object pose·OBB를 observation 원천으로 사용할 수 있지만, 이것이 실제 배치 난이도가 낮다는 뜻은 아니다. 초기 feasibility에서는 ground truth로 manipulation 자체를 분리해 확인하고, Sim-to-Real 단계에서는 tracker를 새로 개선하는 대신 다음 observation corruption을 적용한다.

- Translation·orientation noise와 episode bias
- 시간적으로 상관된 drift와 낮은 perception update rate
- Latency와 zero-order hold
- 간헐적 dropout·outlier
- OBB symmetry에 따른 axis permutation·sign ambiguity

Noise 범위는 임의로 크게 정하지 않고 실제 tracker log에서 측정한다. `valid/age`는 기본 입력이 아니라 실제 tracker가 해당 metadata를 제공하고 dropout recovery 실험이 필요할 때 추가하는 deployment 옵션이다.

Phase ID는 계속 제외한다. 단, reward gate는 actor가 보는 current observation과 그 history에서 진행 상태를 판별할 수 있어야 하며, observation으로 복원할 수 없는 hidden latch를 사용해서는 안 된다. Arm은 current `q_A`만 포함하고 `dq_A`·history는 제외한다. Absolute EEF pose, fingertip positions, raw RGB, point cloud와 mesh도 현재 action interface와 upstream perception 조건에서는 계속 제외한다.

### 5.5 Rotation Observation 표현 검토

회전을 다루는 연구의 goal 표현은 과업 의미에 따라 달라진다.

- [A System for General In-Hand Object Re-Orientation](https://proceedings.mlr.press/v164/chen22a.html)은 임의의 final SO(3) 자세에 도달해야 하므로 current–target orientation의 quaternion difference를 정책에 제공했다.
- [Rotating without Seeing](https://roboticsproceedings.org/rss19/p036.html)과 [RotateIt](https://proceedings.mlr.press/v229/qi23a.html)은 특정 final orientation이 아니라 지정 축으로 계속 회전하는 과업이므로 hand-centric rotation-axis vector를 goal로 제공했다.
- Planar tactile pushing은 pusher/sensor-local pose의 planar angle을 사용한다. 이때 full SO(3)를 제공할 이유가 없다.
- [On the Continuity of Rotation Representations in Neural Networks](https://openaccess.thecvf.com/content_CVPR_2019/html/Zhou_On_the_Continuity_of_Rotation_Representations_in_Neural_Networks_CVPR_2019_paper.html)은 full SO(3)를 MLP가 직접 처리할 때 5D/6D continuous representation이 quaternion·Euler보다 학습에 유리할 수 있음을 보였다.

Zhou et al.의 5D는 6D에서 한 성분을 임의로 삭제한 표현이 아니다. 회전행렬의 첫 두 column을 `c_1=(x_1,y_1,z_1)`, `c_2=(x_2,y_2,z_2)`라 두면 6D 표현은 `[c_1,c_2]`다. 5D 표현은 `x_1,y_1`을 남기고, unit vector `c_2`와 scalar `z_1`을 normalized stereographic projection으로 하나의 3D vector에 넣는다.

$$
P(c_2,z_1)=c_2\left(z_1+\sqrt{z_1^2+1}\right)
$$

따라서 개념적인 5D encoding은 다음과 같다.

$$
r_{5D}=\left[x_1,y_1,P(c_2,z_1)^{T}\right]
$$

복원할 때 `v=P(c_2,z_1)`에 대해 다음 inverse projection으로 `c_2,z_1`을 되찾고, 복원된 두 3D vector를 Gram–Schmidt orthogonalization에 넣어 rotation matrix를 만든다.

$$
c_2=\frac{v}{\|v\|},\qquad
z_1=\frac{\|v\|^2-1}{2\|v\|}
$$

즉 5D도 full SO(3) 표현이며 yaw·pitch·roll 중 하나를 제거한 표현이 아니다. 6D보다 한 차원 작지만 projection/unprojection이 추가되고 물리적 해석이 덜 직접적이다. 원 논문의 point-cloud rotation regression에서도 저자들은 stereographic projection의 gradient distortion을 5D가 6D보다 낮은 성능을 보인 가능한 원인으로 들었다. 따라서 5D는 representation ablation 후보로만 두고, baseline에는 6D를 우선한다.

우리의 current와 goal frame은 다음처럼 EEF 기준으로 맞추는 것이 자연스럽다.

$$
R_{EO,t}=R_{WE,t}^{T}R_{WO,t},\qquad
R_{EG,t}=R_{WE,t}^{T}R_{WG,t}
$$

표현의 현재 후보와 적용 조건은 다음과 같다.

1. **EEF가 shelf normal에 정렬된 planar 조건:** EEF의 roll/pitch가 고정되고 object도 shelf 위에서 upright하다는 조건이 보장되면 current yaw와 preparatory target yaw를 각각 `[\cos\psi,\sin\psi]`로 표현해도 충분하다.
2. **움직이는 EEF frame의 일반 조건:** EEF가 roll/pitch까지 바꾸거나 object가 접촉 중 기울 수 있다면, EEF-frame Euler yaw는 shelf-plane yaw와 같지 않다. 이 경우 `R_{EO}`와 `R_{EG}`의 첫 두 column을 펼친 6D continuous representation을 각각 제공하는 편이 정보 손실이 적다.

현재 working baseline은 **current와 target orientation은 EEF-relative 6D로 관측하되, reward의 회전 오차는 shelf/task frame의 normal 주위 yaw 성분만 평가**하는 것이다. Full 3D observation을 준다는 것이 object를 arbitrary SO(3) target으로 보내겠다는 뜻은 아니다. 관측은 움직이는 EEF와 접촉 중의 tilt까지 해석할 문맥을 보존하고, task objective는 원하는 pushing 방향에 적합한 preparatory yaw에 한정한다. Action은 작은 EEF-local increment이므로 3D rotation vector를 계속 사용할 수 있다. 이 baseline의 최종 사용자 확정은 남아 있다.

이에 따른 MLP 전체 차원은 각각 다음과 같다.

$$
D_{\mathrm{planar}}=25+K_bM+6K_w+12K_a,\qquad
D_{\mathrm{6D}}=33+K_bM+6K_w+12K_a
$$

6D representation 자체가 OBB symmetry를 해결하지는 않는다. Symmetry group을 `\mathcal{G}_O`라 하면 yaw reward와 goal 판정은 task frame에서 symmetry-equivalent target 중 가장 가까운 것을 사용한다.

$$
e_{\psi,\mathrm{sym}}=
\min_{S\in\mathcal{G}_O}
\left|\operatorname{wrap}\!\left(\psi(R_{WO})-\psi(R_{WG}S)\right)\right|
$$

여기서 $\psi(\cdot)$는 Euler decomposition의 임의 yaw가 아니라 shelf/task-frame normal에 직교하는 평면으로 object heading을 투영해 얻는 yaw다. Episode 중 OBB axes는 이전 frame과 가장 가까운 symmetry-equivalent orientation을 선택해 temporal continuity를 유지한다. Roll/pitch까지 목표로 제어하는 과업으로 확장할 때만 full geodesic SO(3) error를 별도로 사용한다.

---

## 6. 시스템 내 역할 경계

### 6.1 1단계의 역할 분담

- 상위 perception·판단 모듈이 장면을 관측하고 조작할 blocker, 밀기에 적합한 목표 물체 orientation과 병진 목표를 정한다. 목표 선택 알고리즘 자체는 현재 low-level policy의 contribution 범위가 아니다.
- VLA 또는 conventional planner가 장거리 reaching을 담당하고 로봇을 blocker 근처로 이동시킨다.
- Track B policy가 물체 근처의 Approach, contact formation, Rotation과 Push를 수행한다.
- 외부 execution layer가 안전 중단과 전체 시스템 수준의 실행을 관리할 수 있다.

### 6.2 2단계의 첫 확장 범위

- 조작할 blocker의 선택은 상위 판단기에 남긴다.
- 선택된 blocker를 어떻게 재배치할지는 정책이 목적에 맞게 결정하고 실행한다.
- 여러 blocker 중 선택 및 전체 조작 순서 결정은 후속 확장으로 둔다.
- 장거리 reaching과 실제 target retrieval 전체를 현재 2단계에 포함하지 않는다.

여기서 `end-to-end에 가까운 정책`은 target 접근 공간 확보라는 목적에서 blocker manipulation action까지 정책의 담당 범위를 넓힌다는 뜻이다. Raw sensor부터 action까지의 단일 network, 특정 VLA 구조 또는 특정 학습 알고리즘을 의미하지 않는다.

---

## 7. 평가 수준

연구 진행 단계에 따라 평가 목적을 구분한다.

| 구분 | 중심 평가 |
| --- | --- |
| 1단계 | 주어진 blocker 회전·병진 목표의 실행 정확도와 접촉 안정성 |
| 2단계 | Target 접근 공간 또는 경로 확보 효과와 전체 manipulation의 효율·안정성 |

Low-level object goal의 달성과 최종 shelf-retrieval 효과도 구분한다.

- **Skill-level:** blocker의 position/orientation 정확도, 접촉과 힘의 안정성, 실패 및 수행 시간
- **System-level:** target의 노출, 접근 가능한 공간·경로, 실제 인출 가능 여부와 필요한 조작 횟수

1단계의 회전·병진 정확도만으로 연구 전체의 공간적 유용성을 주장하지 않는다. 반대로 2단계의 공간 확보 효과를 평가할 때에도 접촉 부하, 전도·낙하와 같은 실행 안정성을 함께 고려한다.

---

## 8. 현재 범위에 관한 주의사항

- Track B를 Rotation 또는 Handling만을 수행하는 Track으로 정의하지 않는다.
- Track B는 lateral pushing에 한정되지 않으며 다양한 방향의 translation과 rotation을 다루는 방향이다.
- `Approach → Rotation → Push`는 1단계의 기본 실행 순서이며 모든 상황의 유일한 전략은 아니다.
- 1단계에서 policy가 밀기에 적합한 중간 물체 orientation을 스스로 선택한다고 주장하지 않는다. 목표 물체 자세의 선택은 상위, 그 목표를 위한 손 구성·접촉 실행은 policy의 역할이다.
- Contribution을 최초 Approach에만 한정하거나 각 phase마다 독립 novelty가 필요하다고 해석하지 않는다.
- 다양한 초기 자세·방향을 지향한다는 것이 모든 SE(3) 자세에서의 실행 가능성을 보장한다는 뜻은 아니다. 실제 학습·평가 범위는 미결이다.
- Continuous Vision과 근사 geometry의 사용을 정확한 접촉 상태의 완전 관측과 동일시하지 않는다.
- F/T와 tactile은 Track B의 관측에서 제외하거나 임의로 optional로 바꾸지 않는다.
- Actor에 phase ID를 주지 않는다는 현재 방향을 shared policy가 없거나 phase 구분이 없다는 뜻으로 해석하지 않는다. Phase별 reward gate는 별도로 존재한다.
- OBB는 exact geometry가 아니라 unseen object·occlusion에서 안정성을 우선한 coarse representation이다. Global semantic/canonical object frame을 안다고 가정하지 않는다.
- 실제 tactile sensor가 17개라는 사실과 policy가 반드시 17차원을 써야 한다는 주장을 구분한다. Coarse `M<17`과 17-channel 표현은 비교 대상이다.
- 세 가지 동작 구분을 세 개의 독립 network 또는 policy가 필요하다는 뜻으로 해석하지 않는다.
- 특정 시리얼 박스, hooking 동작, 동일한 회전각과 선반 구성은 설명용 예시이지 전체 학습 명세가 아니다.
- 현재 방향은 연구 문제와 진행 범위를 정의한 것이며, 최종 Method novelty나 논문 Contribution이 확정된 것은 아니다.

---

## 9. 아직 확정 명세로 취급하지 않는 항목

다음 항목은 추가 논의와 실험을 통해 구체화한 뒤 이 문서에 반영한다.

- 현재 최소 MLP observation의 실제 구현과 각 항목의 정규화·noise
- 사용 중인 Isaac Lab·Isaac Sim 버전과 이에 맞는 ContactSensor·joint-wrench API
- 실제 RH56E2 17 sensor의 위치·단위·noise floor·packet/update rate
- 실제 17 sensor와 URDF link/pad의 coarse grouping, 최종 `M`과 17-channel 비교 구현
- Binary contact threshold·hysteresis·filter와 접촉 상대에 무관한 any-contact aggregation
- Initial OBB template tracking, axis continuity, occlusion과 symmetry 처리
- Goal rotation representation과 EEF-frame target의 갱신 방식
- `K_b`, `K_w`, `K_a` 및 policy/control frequency
- 현재 제외한 arm/hand dq·validity/age·추가 kinematic feature를 failure 기반으로 추가할 조건
- Reward·termination·evaluation에만 사용할 simulation privileged information의 목록과 사용 범위
- EEF-frame `delta pose`의 회전 표현, 합성 방식, component별 scale·clip과 하위 EEF controller
- Hand joint action의 대상 DoF, absolute/delta/velocity 의미, coupling과 하위 joint controller
- 상위가 지정한 pushing 준비 orientation을 Push 중·종료 시 유지할 허용 오차와 별도 최종 orientation 조건 여부
- Phase 전환, 완료와 실패 조건
- Reward 수식과 학습 curriculum
- Policy 및 network의 개수와 계층 구조
- Sensor fusion과 modality별 history window
- OBB pose·extent의 perception error 분포
- Sim-to-Real 범위와 sensor calibration
- 공간 확보의 정량적 success metric과 비교 baseline
- 최종 Method novelty, Contribution과 논문화 범위

---

## 10. 현재 작업 우선순위

전체 `Approach / Contact Formation → Rotation → Push` 실행을 하나의 학습 문제로 고려하는 reward를 설계한다. 다만 reward term을 먼저 나열하지 않고 다음 정보 계약을 우선 정리한다.

1. 실제 실행 시 policy가 받을 **observation**
2. 시뮬레이션에서 reward, termination과 evaluation 계산에만 사용할 **privileged information**
3. Policy가 출력할 **action**과 하위 controller 사이의 의미
4. 위 정의에 기초한 전체 phase reward와 각 term의 근거

Action은 현재 다음 형태를 유력한 방향으로 둔다.

> **EEF frame에서 표현한 `delta pose`와 hand joint action**

이는 세 phase가 같은 action interface를 공유하도록 하기 위한 방향이다. 다만 `delta pose`의 회전 표현과 pose 합성, hand action의 control mode, 차원, scale, frequency 및 하위 controller는 아직 확정하지 않는다.

EEF delta pose는 매 policy step의 measured EEF pose에서 다음 command를 생성하며, 새 action이 이전 command를 대체한다. 이전 desired target에 delta를 누적하지 않는다. 하위 controller는 DiffIK 또는 OSC를 후보로 두고, command는 control decimation 동안만 유지한다. Hand가 joint-position delta를 사용한다면 마찬가지로 measured joint position 기준의 one-step command로 정의한다.

Reward의 각 term은 단순 shaping 편의가 아니라 다음 중 하나 이상의 근거와 연결한다.

- 과업 목표와 phase별 성공 조건
- 접촉 및 물체 운동의 물리적 의미
- 충돌, 낙하와 과도한 힘을 포함한 안전 조건
- 관찰된 학습 실패 mode
- 관련 선행 연구 또는 검증 가능한 설계 가설

Baseline 문헌 검토는 중단하지 않으며, observation·privileged information의 선택과 reward term의 근거를 마련하는 병행 작업으로 사용한다. Observation의 현재 최소 구성은 5.3절과 같으며, tactile region `M`, modality별 history, rotation encoding과 preprocessing은 아직 실험 전 명세다. Reward 수식은 확정하지 않았다.

---

## 11. 현재 Contribution 후보와 문헌 검토 방향

**2026-09-15 확인한 framing:** 다음 표현은 사용자가 합리적이라고 확인한 1단계 contribution 후보다. 역할 경계와 연구 관점에 대한 합의이며, 새로운 method나 성능 우위가 입증되었다는 뜻은 아니다.

> **주어진 물체 회전·병진 목표를 수행하기 위해, 접근부터 회전과 밀기까지 다지 손의 접촉 구성을 형성·전환하고 접촉 피드백으로 보정하는 조작 방법.**

접촉 준비의 유효성은 후속 pushing의 성공과 연결해 검증한다. 이 방향을 어떤 접촉 표현·평가·학습·제어 방법으로 실현할지는 미결이며, Push controller 자체에도 별도의 novelty가 있어야 한다고 정한 것은 아니다. 중간 목표 물체 orientation의 자율 선택은 2단계 확장으로 남긴다.

**현재 문헌 검토의 역할:** Baseline의 전체 문제 정의와 해결 방식을 살피면서 observation, privileged signal과 reward term의 근거를 수집한다. `회전 후 밀기`, 작업별 손 자세 합성, 촉각 기반 다지 손 제어, 후속 성공을 고려한 skill 연결에는 선행 연구가 있으므로, 이 요소들의 사용·결합만으로 신규성을 주장하지 않는다.

Agent가 제안한 우선 독해 후보는 다음과 같다. 사용자가 최종 실험 baseline을 선정한 것은 아니다.

- **GD2P:** 주어진 물체 상태·밀기 방향에 맞는 손 구성을 생성하고 실제 push 성공으로 검증하는 방법.
- **Hermans et al., 2013:** 안정적인 접촉 위치가 목표 밀기 방향과 정렬되도록 준비 회전을 사용하는 관점.
- **TaskDexGrasp:** 작업에 필요한 힘·모멘트를 가할 수 있는 hand configuration의 평가·합성 방법.

DexMove 등의 촉각 제어 연구와 Sequential Dexterity 등의 phase 연결 연구도 차별성 검토에 포함한다. 논문별 출처·한계, 단순 결합 baseline 등의 비교 후보와 판단 과정은 [`context.md`](./context.md)의 **5.11절·11절 Stage 8–10·15.2–15.3절**에 보존한다. 기존 reward 분석은 폐기하지 않고, 새 정보 계약에 맞춰 근거와 계산 가능성을 재검토할 설계 후보로 유지한다.
