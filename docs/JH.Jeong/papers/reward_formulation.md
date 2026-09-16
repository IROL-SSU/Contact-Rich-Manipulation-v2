# Reward Formulation References — Pushing and Rotation/Pivoting

> [Paper Index](./README.md) · [핵심 B-ID 목록](./core_papers.md) · [목적별 그룹](./topic_groups.md) · [우선 독해](./reading_guide.md)
>
> **범위:** Track B 1단계의 `Approach / Contact Formation → Rotation → Push` reward를 설계하기 위한 RL 원문 비교
>
> **검토 상태:** 아래 reward 항과 설계 이유는 원문 full text로 확인했다. `Track B 해석`은 논문의 주장과 구분한 연구팀의 분석이다.

---

## 1. 먼저 보이는 결론

1. **Pushing reward는 단순한 object–goal distance만으로 끝나지 않는다.** 목표 방향 정렬, 접촉 유지, 힘의 작용선, 충돌·전도 제약과 action-rate regularization을 추가한 이유는 각각 local optimum, 접촉 이탈, 원치 않는 회전, 공격적인 밀기와 불안정한 제어를 막기 위해서다.
2. **Rotation/pivoting에는 서로 다른 두 목표가 섞여 있다.** `목표 자세에 도달`하는 pivoting은 orientation error를 쓰지만, `계속 회전`하는 in-hand rotation은 매 step 회전량 또는 목표 축 angular velocity를 쓴다. Track B의 준비 회전은 전자에 가깝다.
3. **Contact force magnitude를 크게 만드는 reward는 공통 해법이 아니다.** 최근 연구는 방향만 사용하거나, magnitude mismatch를 이유로 아예 맞추지 않으며, 큰 힘·토크·일은 penalty 또는 constraint로 둔다.
4. **기존 reward 중 Rotation 종료 접촉이 후속 Push에 좋은지를 직접 평가하는 항은 찾지 못했다.** 자세 도달과 연속 회전 안정성은 다루지만 downstream pushing feasibility는 별도의 연구 공백으로 남는다.

## 2. Pushing RL 비교

| ID | 과업·학습 | 실제 reward 구성 | 그렇게 설정한 이유 | Track B에서의 활용 | 그대로 쓰기 어려운 점 |
| --- | --- | --- | --- | --- | --- |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | [Pivoting·pushing, demonstration-guided RL](https://doi.org/10.1109/LRA.2026.3655262) | 공통으로 task progress, sparse success, action smoothness를 사용한다. Pushing은 object goal-pose error를 평가하고, dynamics-conditioned variant는 CITO 시연의 EEF pose·contact-force **방향**·extrinsic contact state를 추가한다. | Sparse reward만으로 찾기 어려운 동적으로 가능한 contact trajectory를 시연으로 안내한다. Force magnitude는 model–simulator mismatch가 커서 맞추지 않고 방향만 사용한다. | Push progress, smoothness, desired contact-pair reward와 privileged force-direction shaping의 강한 최신 근거 | CITO reference와 object pose·물성·extrinsic contact의 privileged state를 요구한다. 우리 방법이 같은 시연 pipeline을 쓰지 않으면 직접 baseline과 설계 근거를 구분해야 한다. |
| [B82](https://doi.org/10.1109/ICRA55743.2025.11128166) | [Mobile manipulator의 unknown-object goal pushing, constrained RL](https://doi.org/10.1109/ICRA55743.2025.11128166) | OBB 8개 vertex의 object–goal error, EEF–surface reach target, goal 방향과 일치하는 object velocity direction, action-rate reward를 사용한다. Collision, joint·torque limit, toppling은 reward 합이 아니라 constraint로 분리한다. | OBB vertex error 하나로 position과 yaw를 함께 평가한다. 속도 magnitude를 빼 공격적인 밀기를 막고, 초기에는 물체 표면 여러 위치를 탐색하도록 reach term을 크게 준 뒤 약화한다. | Object progress와 접근 shaping의 역할 분리, direction-only progress, shelf collision·toppling의 constraint/termination 처리 | 최종 goal pose를 맞추는 과업이며, Track B의 primary goal인 `주어진 방향으로 밀기`와 완전히 같지 않다. OBB keypoint reward는 symmetry 처리도 필요하다. |
| [B10](https://doi.org/10.1109/LRA.2023.3295236) | [Tactile goal-conditioned pushing, model-free·model-based RL](https://doi.org/10.1109/LRA.2023.3295236) | Goal에서 멀 때는 contact surface를 object–goal bearing에 맞추는 orientation error를, goal 근처에서는 object–goal Euclidean distance를 사용한다. 두 구간 모두 pusher가 contact surface normal과 정렬되도록 보상한다. | 거리만 줄이면 먼저 물체를 돌려야 하는 상황에서 local optimum이 생긴다. Goal 근처에서는 bearing angle이 불안정하므로 거리로 전환한다. Normal push는 접촉 유지와 center-of-friction을 통한 안정적 pushing에 중요했다. | Phase/gate에 따라 reward 의미를 바꾸는 직접 사례, push 방향 정렬과 stable-contact shaping 근거 | 단일 tactile pusher가 이미 물체와 접촉한 2D 과업이다. 다지 손의 접근·회전·접촉 전환과 shelf collision은 다루지 않는다. |
| [B83](https://doi.org/10.3389/fnbot.2023.1271607) | [Unseen-object reaching and pushing, SAC](https://doi.org/10.3389/fnbot.2023.1271607) | 비접촉 시 EEF–object contact 형성을 유도하고, 접촉 후에는 contact-force 방향을 object–goal 방향과 정렬하며 force 작용선과 CoM 사이 lever arm을 줄인다. Force magnitude는 사용하지 않는다. | 목표 방향의 순수 병진을 만들고 불필요한 rotational torque를 줄이기 위해서다. 상태 차이만 보는 reward보다 접촉 순간의 기하를 직접 평가한다. | Simulation contact force와 contact point·CoM을 privileged reward에 쓰는 근거. Push gate에서 force direction과 torque tendency를 평가할 후보 | 목표가 **회전을 억제하는 직선 pushing**이다. Preparatory rotation 구간에 적용하면 필요한 회전을 방해하므로 Push 전용 후보여야 한다. |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | [Cluttered tabletop goal pushing, TQC](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal 성공 bonus, object–goal global-path distance, EEF–object distance, clutter collision·workspace 이탈 penalty를 합한다. Desired object contact가 생기면 EEF–object distance penalty를 상쇄해 지속 접촉을 유도한다. | 접촉 전에는 object에 접근하고, 접촉 후에는 거리 penalty 때문에 떨어지지 않게 하면서 장면 collision을 줄이기 위해서다. 서로 다른 start–goal 거리는 initial distance로 정규화한다. | Approach와 contact-maintenance를 gate로 분리하고 collision을 privileged pair identity로 평가하는 근거 | Contact bonus는 접촉의 질·힘·후속 유용성을 구분하지 않는다. Actor tactile가 any-contact인 우리 설정에서는 reward만 desired object pair를 구분해야 한다. |

### Pushing 논문에서 반복되는 설계 패턴

| 반복 항 | 해결하려는 failure | 근거 논문 | Track B에서의 현재 해석 |
| --- | --- | --- | --- |
| Object task progress | 목표와 무관한 접촉·정지 | [B10](https://doi.org/10.1109/LRA.2023.3295236), [B81](https://doi.org/10.1109/LRA.2026.3655262), [B82](https://doi.org/10.1109/ICRA55743.2025.11128166), [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | 최종 pose distance보다 `주어진 push direction으로의 signed progress`가 primary candidate다. |
| Contact formation | 물체까지 도달하지 못함 | [B82](https://doi.org/10.1109/ICRA55743.2025.11128166), [B83](https://doi.org/10.3389/fnbot.2023.1271607), [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Contact 전 구간에만 활성화하고 접촉 후에는 약화·종료해야 한다. |
| Contact maintenance·normality | 접촉 이탈, 불안정한 미끄러짐 | [B10](https://doi.org/10.1109/LRA.2023.3295236), [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Any-contact 수를 최대화하지 말고 desired object contact와 안정성 결과를 평가해야 한다. |
| Force direction·line of action | 횡력, 원치 않는 회전, 비효율적 push | [B81](https://doi.org/10.1109/LRA.2026.3655262), [B83](https://doi.org/10.3389/fnbot.2023.1271607) | Privileged contact force의 유망한 용도다. 다만 rotation correction을 허용할 때는 항상 zero torque를 강제할 수 없다. |
| Direction-only velocity | 과격한 속도 증가 | [B82](https://doi.org/10.1109/ICRA55743.2025.11128166) | Push progress에는 방향을 쓰고 속도·힘 magnitude는 별도 안전 상한으로 두는 근거가 된다. |
| Collision·toppling 분리 | Reward trade-off로 안전 위반을 상쇄 | [B82](https://doi.org/10.1109/ICRA55743.2025.11128166), [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | Shelf collision과 topple은 작은 weighted penalty보다 constraint·termination 후보가 강하다. |
| Action smoothness | 진동·고주파 명령 | [B81](https://doi.org/10.1109/LRA.2026.3655262), [B82](https://doi.org/10.1109/ICRA55743.2025.11128166) | EEF delta와 hand joint action 각각의 rate penalty를 검토하되 controller dynamics와 중복 여부를 확인한다. |

## 3. Rotation·Pivoting RL 비교

| ID | 회전 목표 | 실제 reward 구성 | 그렇게 설정한 이유 | Track B에서의 활용 | 그대로 쓰기 어려운 점 |
| --- | --- | --- | --- | --- | --- |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | [지정된 pivoting goal orientation](https://doi.org/10.1109/LRA.2026.3655262) | Orientation error에 대한 linear·quadratic progress, sparse success, action smoothness를 사용한다. Dynamics-conditioned reward는 reference EEF pose·force direction·extrinsic contact state를 더한다. 저자들은 object-size domain randomization에서 quadratic term이 필요했다고 보고한다. | 단순 orientation progress가 다양한 object size에서 부족하고, 동적으로 가능한 contact·force trajectory를 안내해야 하기 때문이다. | 가장 최신의 직접 pivoting reward 근거. 목표 자세와 contact dynamics를 분리해 ablation한 구조가 유용하다. | Demonstration reference에 종속되며, terminal contact가 다음 Push에 적합한지는 평가하지 않는다. |
| [B85](https://doi.org/10.1109/ICRA48891.2023.10161271) | [물체를 table에 수직인 stand-up orientation으로 pivot](https://doi.org/10.1109/ICRA48891.2023.10161271) | Current–goal rotation matrix의 SO(3) geodesic distance 하나를 dense reward로 사용한다. | Object 종류가 바뀌어도 공통인 task objective를 간단히 정의하고, policy와 state/action projection에 generalization 부담을 맡긴다. | Target-relative orientation error만으로 pivoting이 가능한 최소 baseline | Contact 유지·힘·translation drift·후속 pushing readiness가 reward에 없다. Depth-derived object feature와 privileged object pose도 필요하다. |
| [B86](https://doi.org/10.48550/arXiv.1703.00472) | [Tool의 지정 target angle 도달](https://doi.org/10.48550/arXiv.1703.00472) | 허용 angle range로 정규화한 absolute target-angle error의 음수만 사용한다. | Pivoting의 최종 목적을 가장 직접적인 scalar로 표현한 초기 baseline이다. | Yaw-error-only baseline과 sparse success 추가 전의 최저 복잡도 비교군 | 2017년의 단일 tool·단일 pivot setting이며 최신 sensor·generalization·downstream transition을 다루지 않는다. 역사적 comparator로만 사용한다. |
| [B32](https://doi.org/10.15607/RSS.2023.XIX.036) | [지정 축 주위로 계속 in-hand rotation](https://doi.org/10.15607/RSS.2023.XIX.036) | 한 step의 signed rotation angle, object linear-velocity penalty, fall penalty, controller work·torque penalty, fingertip–object distance reward를 사용한다. Position·major-axis deviation이 크면 reset한다. | Simulator angular velocity가 noisy해 vibration을 유발했으므로 projected vector의 finite rotation angle을 사용했다. Linear drift·낙하·과도한 actuation을 억제해 smooth Sim-to-Real rotation을 유도한다. | 회전 진행량 계산, translation drift·fall·work·torque regularization과 failure termination의 근거 | 목표 자세에서 멈추는 과업이 아니라 계속 회전하는 과업이다. Rotation progress를 그대로 쓰면 target을 지나 계속 돌 수 있다. |
| [B40](https://doi.org/10.48550/arXiv.2309.09979) | [Hand-centric target axis 주위로 계속 in-hand rotation](https://doi.org/10.48550/arXiv.2309.09979) | Target axis angular velocity reward, undesired-axis angular velocity penalty, hand pose deviation, object linear velocity, work와 torque penalty를 사용한다. | 목표 축 회전을 빠르게 만들면서 다른 축 회전, 물체 병진, 비효율적 actuation과 불안정한 hand pose를 억제한다. | Desired/undesired rotation 분해와 효율·안정성 regularizer의 근거 | 역시 terminal orientation이 없는 continuous rotation이다. Preparatory yaw goal에는 angle-to-goal 또는 stopping criterion이 별도로 필요하다. |

### 목표 자세형과 연속 회전형을 구분해야 하는 이유

| 유형 | 대표 | 최적 행동 | Track B와의 관계 |
| --- | --- | --- | --- |
| Target-orientation pivoting | [B81](https://doi.org/10.1109/LRA.2026.3655262), [B85](https://doi.org/10.1109/ICRA48891.2023.10161271), [B86](https://doi.org/10.48550/arXiv.1703.00472) | 목표 각도 오차를 줄이고 허용 범위에서 멈춤 | Preparatory rotation의 1차 objective와 직접 대응 |
| Continuous-axis rotation | [B32](https://doi.org/10.15607/RSS.2023.XIX.036), [B40](https://doi.org/10.48550/arXiv.2309.09979) | 매 step 원하는 축으로 계속 회전 | 회전 안정성과 regularizer는 참고 가능하지만 task reward를 그대로 가져오면 안 됨 |

## 4. Track B reward 설계에 주는 잠정적 시사점

아래는 **채택 결정이 아니라 문헌에서 도출한 설계 가설**이다.

### 4.1 Task progress와 contact shaping을 분리한다

- Task progress는 object-level 결과를 평가한다: preparatory yaw error 감소, 이후 push direction의 signed translation progress.
- Contact shaping은 결과를 달성할 수 있게 돕는 보조항이다: contact formation, desired contact 유지, force-direction alignment.
- Contact 수·force magnitude를 무조건 키우는 항은 reward hacking과 sensor–simulation mismatch 위험이 크다.

### 4.2 Phase gate는 문헌에 있지만 우리 gate 조건은 별도로 검증한다

- [B10](https://doi.org/10.1109/LRA.2023.3295236)은 goal 근접 여부에 따라 orientation shaping에서 position shaping으로 전환한다.
- [B83](https://doi.org/10.3389/fnbot.2023.1271607)과 [B84](https://doi.org/10.1109/IROS47612.2022.9981873)는 contact 유무에 따라 approach와 push reward의 의미가 달라진다.
- 따라서 phase ID 없이 observable state로 term을 gate하는 방향은 근거가 있다. 다만 Track B에서는 gate가 hidden latch가 아니라 actor observation/history에서 복원 가능한 current condition이어야 한다.

### 4.3 Privileged contact force는 magnitude target보다 결과·안전 평가에 우선 사용한다

- [B81](https://doi.org/10.1109/LRA.2026.3655262)과 [B83](https://doi.org/10.3389/fnbot.2023.1271607)은 force **방향**을 사용하고 magnitude를 목표로 맞추지 않는다.
- Track B에서는 desired force direction, excessive normal force, impact impulse, slip과 shelf contact를 privileged reward·constraint 후보로 둘 수 있다.
- Actor가 binary tactile와 wrist F/T만 보는 상황에서 식별할 수 없는 per-contact force distribution을 정답 행동으로 강제하지 않는다.

### 4.4 Rotation reward에는 downstream Push 항이 추가로 필요하다

Target yaw 도달만 평가하면 같은 orientation success 안에서도 다음 상태가 달라질 수 있다.

- 손이 pushing 반대쪽에 남아 있는가
- 유효한 object contact가 유지되는가
- hand·arm configuration에 다음 push 방향의 motion margin이 있는가
- shelf와 과도한 force contact가 없는가
- 짧은 push rollout 또는 downstream critic에서 실제 progress가 가능한가

기존 pivoting reward의 orientation error는 유지할 수 있지만, 위 조건을 평가하는 `transition/downstream feasibility` 항 또는 별도 terminal metric이 Track B의 핵심 비교 대상이다.

## 5. 우선 독해 순서

1. **[B81](https://doi.org/10.1109/LRA.2026.3655262)** — 2026년의 pushing·pivoting 공통 formulation, demonstration-conditioned contact·force reward
2. **[B10](https://doi.org/10.1109/LRA.2023.3295236)** — goal-conditioned pushing에서 reward gate를 둔 이유와 stable normal contact
3. **[B82](https://doi.org/10.1109/ICRA55743.2025.11128166)** — pose progress·surface exploration·direction-only velocity와 safety constraint 분리
4. **[B32](https://doi.org/10.15607/RSS.2023.XIX.036)** — continuous rotation의 progress 계산과 안정성 regularizer
5. **[B85](https://doi.org/10.1109/ICRA48891.2023.10161271)** — target-orientation pivoting의 최소 reward와 unseen-object generalization
6. **[B83](https://doi.org/10.3389/fnbot.2023.1271607)** — privileged contact-force direction과 lever-arm shaping
7. **[B84](https://doi.org/10.1109/IROS47612.2022.9981873)** — clutter에서 contact 유지·collision·distance normalization
8. **[B40](https://doi.org/10.48550/arXiv.2309.09979) → [B86](https://doi.org/10.48550/arXiv.1703.00472)** — continuous-axis reward와 역사적 angle-error-only baseline의 경계 확인

## 6. 공식 자료

| ID | 공식 자료 |
| --- | --- |
| [B81](https://doi.org/10.1109/LRA.2026.3655262) | [MERL publication](https://www.merl.com/publications/TR2026-011), [Publication DOI](https://doi.org/10.1109/LRA.2026.3655262) |
| [B82](https://doi.org/10.1109/ICRA55743.2025.11128166) | [arXiv](https://arxiv.org/abs/2502.01546), [Publication DOI](https://doi.org/10.1109/ICRA55743.2025.11128166) |
| [B10](https://doi.org/10.1109/LRA.2023.3295236) | [arXiv](https://arxiv.org/abs/2307.14272), [Publication DOI](https://doi.org/10.1109/LRA.2023.3295236) |
| [B83](https://doi.org/10.3389/fnbot.2023.1271607) | [Frontiers](https://www.frontiersin.org/journals/neurorobotics/articles/10.3389/fnbot.2023.1271607/full), [Publication DOI](https://doi.org/10.3389/fnbot.2023.1271607) |
| [B84](https://doi.org/10.1109/IROS47612.2022.9981873) | [arXiv](https://arxiv.org/abs/2203.02389), [Publication DOI](https://doi.org/10.1109/IROS47612.2022.9981873) |
| [B85](https://doi.org/10.1109/ICRA48891.2023.10161271) | [arXiv](https://arxiv.org/abs/2305.02554), [Publication DOI](https://doi.org/10.1109/ICRA48891.2023.10161271) |
| [B86](https://doi.org/10.48550/arXiv.1703.00472) | [arXiv](https://arxiv.org/abs/1703.00472), [arXiv DOI](https://doi.org/10.48550/arXiv.1703.00472) |
| [B32](https://doi.org/10.15607/RSS.2023.XIX.036) | [RSS](https://roboticsproceedings.org/rss19/p036.html), [Publication DOI](https://doi.org/10.15607/RSS.2023.XIX.036) |
| [B40](https://doi.org/10.48550/arXiv.2309.09979) | [PMLR](https://proceedings.mlr.press/v229/qi23a.html), [arXiv](https://arxiv.org/abs/2309.09979) |

---

