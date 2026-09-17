# Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry

> **연구 정의:** **Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry**
>
> **Task category:** Nonprehensile manipulation
>
> **현재 단계:** Track B Stage 1 — goal-conditioned low-level policy
>
> **문서 역할:** 연구 범위, 가설, 임시 baseline과 미결 사항을 분리하고 이후 판단의 기준을 제공한다.
>
> **최종 갱신:** 2026-09-17

---

## 1. Project Overview

### 1.1 이 문서의 우선순위

이 문서는 현재 연구 정의를 통제하기 위한 기준 문서다. 기존의 [`README.md`](./README.md), [`motivation.md`](./motivation.md), [`research_topic.md`](./research_topic.md), [`policy_learning.md`](./policy_learning.md)와 [`papers/`](./papers/README.md)는 이번 재검토의 근거로 읽었지만 수정하지 않았다. 해당 문서에서 하나의 확정안처럼 서술된 내용도 이 문서에서 `[Baseline]`, `[Open]` 또는 `[Candidate]`로 재분류되었다면 최신 판단은 이 문서를 따른다.

상태 표기는 다음 의미로만 사용한다.

| Status | 의미 |
| --- | --- |
| `[Fixed]` | 현재 연구가 유지되는 동안 쉽게 바꾸지 않을 문제 범위와 시스템 경계 |
| `[Hypothesis]` | 실험으로 지지되거나 기각되어야 하는 주장 |
| `[Baseline]` | 가설 검증을 위해 먼저 구현하는 방법이며 변경 가능 |
| `[Open]` | 근거·실험·사용자 판단이 더 필요한 사항 |
| `[Candidate]` | 실험과 문헌 비교가 지지할 때만 contribution이 될 수 있는 항목 |
| `[Rejected]` | 검토했지만 현재 사용하지 않는 방향 또는 주장 |
| `[Superseded]` | 과거에는 사용했으나 최신 정의가 대체한 내용 |

`[Fixed]`는 연구적으로 참이라는 뜻이 아니라 **연구할 문제를 고정했다**는 뜻이다. `[Hypothesis]`와 `[Candidate]`는 논문 결과나 contribution처럼 서술하지 않는다.

### 1.2 Working research definition

> **Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry**

`[Fixed]` 선반 안의 target object에 접근할 공간을 만들기 위해 상위 모듈이 지정한 blocker를 **안정적으로 파지하거나 들어 올리지 않고** 조작하는 low-level nonprehensile manipulation을 연구한다. Approximate geometry 조건에서 tactile과 wrist F/T feedback으로 hand configuration 및 wrist–finger motion을 조정하며, Approach / Contact Formation → Rotation / Pivoting → Push / Translation을 수행한다.

`[Fixed]` 이 문서에서 `nonprehensile manipulation`은 **task category**이고, `contact-rich manipulation`은 별도의 task category가 아니라 접촉 위치·마찰·stick–slip–separation과 힘 전달의 불확실성을 다루는 **interaction/control challenge**를 뜻한다.

현재 연구는 다음 흐름으로 발전시킨다.

```text
Research Motivation
  → Previous Works가 해결한 범위
  → 남아 있는 Research Gap
  → 반증 가능한 Research Questions / Hypotheses
  → 가설을 검증하는 Method
  → 통제된 Experiments / Ablations
  → 실험으로 지지된 Contribution
```

현재 단계에서는 마지막 contribution 문장이나 PPT 구성을 확정하지 않는다.

### 1.3 Broader motivation과 좁은 연구 문제

- `[Fixed]` 물류·가정 서비스 환경에는 pushing, pulling, pivoting처럼 안정적 파지 없이 물체를 이동시키는 nonprehensile manipulation이 필요하다.
- `[Fixed]` 이 과업은 물체 및 환경과의 접촉을 동반하며, 접촉 위치, 국소 형상, 마찰, 질량 분포, stick–slip–separation과 힘 전달의 불확실성 때문에 contact-rich interaction/control challenge가 된다.
- `[Fixed]` 본 연구는 heuristic, optimization, RL, IL 또는 VLA 전체의 우열을 논하지 않는다. 이들이 이미 해결한 범위를 인정한 뒤, coarse geometry와 배포 가능한 접촉 sensing 아래의 blocker manipulation에 질문을 한정한다.
- `[Fixed]` tactile 또는 wrist F/T를 사용한다는 사실 자체는 novelty가 아니다. 연구 질문은 이 정보가 **어떤 불확실성을 줄이고 어떤 후속 조작 성능을 변화시키는가**이다.

---

## 2. Fixed Research Scope

### 2.1 유지할 연구 범위

| Item | Fixed definition | Status |
| --- | --- | --- |
| Application | 선반 뒤쪽 target에 접근할 공간을 만들기 위한 앞쪽 blocker manipulation | `[Fixed]` |
| Task category | 안정적 grasp/lift 없이 pushing·pulling·pivoting으로 blocker를 재배치하는 nonprehensile manipulation | `[Fixed]` |
| Interaction/control challenge | 접촉 위치·마찰·stick–slip–separation·힘 전달이 불확실한 contact-rich interaction | `[Fixed]` |
| Manipulated object | 현재 low-level policy가 직접 조작하는 대상은 blocker | `[Fixed]` |
| Planning boundary | Blocker 선택과 전역 작업 순서 결정은 상위 모듈의 역할 | `[Fixed]` |
| Current research stage | Track B Stage 1의 low-level execution policy | `[Fixed]` |
| Task structure | Approach / Contact Formation, Rotation / Pivoting, Push / Translation의 연결된 조작 | `[Fixed]` |
| Reason for rotation | 이후 Push에 적합한 object–hand relation, object orientation 또는 contact face를 형성하기 위한 preparatory manipulation | `[Fixed]` |
| Geometry regime | 완전한 국소 접촉 형상을 actor가 안다고 가정하지 않는 approximate-geometry regime | `[Fixed]` |
| Information roles | Vision/coarse geometry는 전역 task·접근 정보를 제공하고, tactile·wrist F/T는 실제 접촉 상태와 geometry approximation error를 보완하는 closed-loop feedback으로 사용 | `[Fixed]` |
| Training boundary | 실물에서 얻을 수 없는 simulator state는 actor observation이 아니라 reward·constraint·evaluation용 teacher information으로 분리 | `[Fixed]` |
| Hardware platform | UR5e arm과 Inspire Robots RH56E2 hand를 현재 구현 플랫폼으로 사용 | `[Fixed]` |

Approach–Rotation–Push는 세 개의 의미 있는 sub-objective를 나타낸다. 모든 episode에서 Rotation을 강제로 수행하거나 세 개의 독립 policy를 사용한다는 뜻은 아니다. 초기 상태가 이미 Push에 적합하면 Rotation은 작거나 생략될 수 있다.

### 2.2 현재 범위 밖

| Out-of-scope item | Boundary | Status |
| --- | --- | --- |
| Target retrieval 전체 | Target grasp·인출은 system-level motivation과 후속 평가 대상이지 Stage 1 actor의 직접 동작이 아님 | `[Fixed]` |
| Multi-blocker ordering | 여러 blocker 중 선택하고 순서를 정하는 문제는 상위 planning 또는 후속 단계 | `[Fixed]` |
| Perception algorithm 자체 | Object tracking, marker detection, OBB estimation network의 성능 개선은 현재 method contribution이 아님 | `[Fixed]` |
| General-purpose VLA | Open-world language instruction과 embodiment-general policy는 현재 연구 범위가 아님 | `[Fixed]` |
| Exact contact reconstruction | 고해상도 tactile image에서 contact geometry를 복원하는 문제는 현재 범위가 아님 | `[Fixed]` |

### 2.3 과거 Track 구분의 위치

- `[Superseded]` Track A와 Track B의 비교를 현재 `context.md`의 최상위 구조로 사용하는 방식.
- `[Fixed]` 현재 구체화 대상은 Track B Stage 1이다. Track A는 별도의 연구 방향으로 남아 있지만 현재 method와 experiment의 범위에 포함하지 않는다.
- `[Rejected]` `Track A = sweeping`, `Track B = rotation`처럼 동작 종류만으로 연구축을 구분하는 해석.

---

## 3. System Boundary and Assumptions

### 3.1 모듈별 역할

| Module | Responsibility | Not assumed | Status |
| --- | --- | --- | --- |
| Upper-level planner | Blocker와 low-level task goal을 제공 | Low-level nonprehensile execution을 직접 해결한다고 가정하지 않음 | `[Fixed]` |
| Perception | Object pose와 어떤 형태의 geometry information을 제공 | Marker가 geometry까지 제공한다고 간주하지 않음 | `[Fixed]` |
| Stage 1 policy | Wrist와 fingers를 제어하여 접촉을 형성·변경하고 회전·밀기 goal을 실행 | Blocker 선택이나 전체 retrieval 순서를 결정하지 않음 | `[Fixed]` |
| Simulator teacher | Exact pose, contact pair·force, collision, support, velocity 등을 계산 | 이 정보가 실물 actor에 직접 제공된다고 가정하지 않음 | `[Fixed]` |
| Hardware safety layer | Force, joint, collision limit를 policy 밖에서도 감시 | Learned policy만으로 모든 안전을 보장한다고 주장하지 않음 | `[Fixed]` |

### 3.2 Task goal interface

- `[Fixed]` 상위 모듈은 low-level policy가 blocker manipulation을 실행하기에 충분한 goal을 제공한다.
- `[Baseline]` 현재 goal encoding은 `selected OBB face + push direction + push distance/target position`이다. 필요한 rotation은 selected-face normal과 push direction의 관계에서 유도한다.
- `[Open]` 상위 모듈이 별도의 desired rotation/orientation을 명시적으로 제공해야 하는 조건이 있는지 검토한다. Non-box-like object, OBB symmetry와 face ambiguity가 이 판단에 영향을 줄 수 있다.
- `[Superseded]` 목표 object quaternion 하나를 모든 task goal의 확정 표현으로 간주한 이전안.

### 3.3 Pose와 geometry를 구분한다

Marker 또는 tracker가 제공하는 기본 정보는 object pose다. Geometry는 별도의 정보원과 정확도 정의가 필요하다.

| Geometry level | Definition | Example | Intended use |
| --- | --- | --- | --- |
| No geometry | Pose 외에 shape·dimension 정보를 actor에 주지 않음 | Object origin pose only | Geometry contribution의 하한 ablation |
| Approximate geometry | 실제 접촉 표면과 오차가 있을 수 있는 명시적 저차원 형상 | Estimated OBB, noisy dimensions | 현재 주 연구 조건 |
| Implicit visual representation | 명시적 mesh 대신 image/point-cloud feature에 형상을 암묵적으로 포함 | RGB/depth encoder, point-cloud embedding | 비교 또는 후속 확장 |
| Exact geometry | Known CAD/mesh 또는 정확한 dimensions가 object pose와 일관되게 정합됨 | Registered mesh, calibrated dimensions | Oracle/upper-bound baseline |

- `[Fixed]` Simulation collision geometry가 정확하더라도 actor가 exact geometry를 관측하지 않으면 actor의 geometry condition은 approximate다.
- `[Baseline]` Approximate geometry의 첫 표현으로 episode-consistent OBB를 사용한다.
- `[Open]` OBB의 생성 방법과 center·orientation·extent error distribution은 아직 정하지 않았다.
- `[Rejected]` Marker pose가 있다는 이유만으로 object geometry도 exact하다고 표현하는 방식.

### 3.4 Vision/geometry와 contact sensing의 역할 분리

| Information source | Primary role | Boundary | Status |
| --- | --- | --- | --- |
| Vision / upper-level perception | Blocker pose, 전역 task context와 접근 관계 제공 | 실제 접촉의 발생·유지·미끄러짐을 직접 안다고 가정하지 않음 | `[Fixed]` |
| Coarse geometry | 접근 방향, selected face와 nominal contact/rotation 계획에 필요한 approximate shape 제공 | 실제 국소 표면과 정확히 일치한다고 가정하지 않음 | `[Fixed]` |
| Tactile | 손의 어느 sensor region에서 접촉이 발생·소실·이동했는지에 대한 국소 feedback 제공 | Binary tactile만으로 contact force나 object identity를 안다고 가정하지 않음 | `[Fixed]` |
| Wrist F/T | Hand–environment system에 전달되는 net force/torque와 부하 변화 제공 | 개별 접촉 위치나 taxel별 압력을 직접 분해한다고 가정하지 않음 | `[Fixed]` |
| Proprioception | 현재 arm/hand configuration과 command 결과 제공 | 외부 접촉 상태를 단독으로 완전히 식별한다고 가정하지 않음 | `[Fixed]` |

- `[Fixed]` Vision/coarse geometry와 contact sensing은 동일 정보를 중복 제공한다고 가정하지 않고, 전자는 전역 task·접근 정보를, 후자는 실행 중 실제 접촉과 model–reality mismatch에 관한 feedback을 담당한다.
- `[Hypothesis]` Tactile과 wrist F/T가 approximate geometry error를 실제로 보완하고 성능을 높이는지는 H3와 E3/E5로 검증한다.

### 3.5 Observability boundary

- `[Fixed]` Object pose와 coarse geometry가 지속적으로 관측되어도 contact state, friction, mass distribution, local surface와 force transmission은 완전히 관측되지 않는다.
- `[Rejected]` Track B를 `fully observable manipulation`으로 표현하는 방식.
- `[Fixed]` 정확한 simulator contact pair·point·normal·force, object velocity와 collision identity는 actor가 아니라 privileged training/evaluation information이다.
- `[Baseline]` Realistic deployment를 모사할 때 actor pose·OBB에는 tracker에서 측정한 noise, latency, dropout과 OBB ambiguity를 적용한다.

### 3.6 Environment assumptions

- `[Fixed]` Shelf support surface와 구조물의 충돌·이탈은 안전 조건으로 관리한다.
- `[Baseline]` 최초 구현은 고정 shelf layout, 한 개의 조작 blocker와 추가 movable surrounding object가 없는 조건에서 핵심 가설을 분리한다.
- `[Open]` Target object를 물리적으로 scene에 둘지, surrounding object를 추가할지와 이들의 접촉 허용 범위는 Section 7에서 결정한다.
- `[Baseline]` Episode는 hand–blocker non-contact 상태에서 시작한다. 초기 pose sampling 분포는 미결이다.

---

## 4. Research Questions

| ID | Research question | Status |
| --- | --- | --- |
| RQ1 | Direct push가 어렵거나 불안정한 initial condition에서 preparatory Rotation이 최종 Push 성공 가능 영역을 넓히는가? | `[Hypothesis]` |
| RQ2 | Manipulation goal과 이후 Rotation→Push 결과를 고려한 hand/contact formation이 접촉 형성 또는 Rotation 성공만을 목표로 한 방법보다 최종 성공률을 높이는가? | `[Hypothesis]` |
| RQ3 | 동일한 approximate-geometry 조건에서 tactile과 wrist F/T가 pose·geometry·물성 오차에 대한 closed-loop robustness를 높이는가? | `[Hypothesis]` |
| RQ4 | 접촉 후 wrist와 fingers를 함께 조절하는 online adaptation이 fixed-hand 또는 제한된 제어보다 robust하고 안전한가? | `[Hypothesis]` |

Surrounding objects, exact geometry와 explicit downstream learning은 중요한 설계축이지만 현재 독립 research question으로 확정하지 않는다. 먼저 RQ1–RQ4의 조건과 비교군을 정의하는 데 사용한다.

---

## 5. Testable Hypotheses

가설은 최대 네 개로 유지한다. 통계 검정, practical significance threshold와 trial 수는 experiment protocol에서 정한다.

| Hypothesis | 비교 조건 또는 Ablation | 주요 평가 지표 | 가설을 지지하는 결과 | 가설이 기각되는 결과 |
| --- | --- | --- | --- | --- |
| **H1 — Preparatory Rotation utility.** Direct push가 제한되는 initial condition에서 Rotation 후 Push가 direct push보다 최종 성공 가능한 상태 영역을 넓힌다. | 동일 object·goal·sensor·controller에서 `direct push` vs. `rotate-then-push`; initial face-direction error와 접근 가능성을 층화 | Final task success, success 가능한 initial-state volume, lateral error, collision/topple, completion time | 어려운 alignment 구간에서 최종 성공률 또는 feasible-region coverage가 유의하게 증가하고 안전 비용이 허용 범위 내임 | 이득이 없거나 direct push보다 실패·시간·안전 비용이 커 practical advantage가 없음 |
| **H2 — Sequential/task-conditioned contact formation.** Goal과 전체 episode 결과에 조건화된 Approach/Rotation state가 contact-only 또는 phase-local objective보다 Rotation-to-Push 성공률을 높인다. | Full-episode goal-conditioned training vs. contact-onset/stability-only Approach vs. face-alignment-only Rotation; 동일 observation·action budget | Approach contact rate, Rotation success, Rotation-to-Push success, final success, saved-state continuation probability | Contact rate나 Rotation success를 통제한 뒤에도 downstream Push success가 높음 | 향상이 없거나 단순 phase-local baseline과 동일하며 추가 복잡도만 증가 |
| **H3 — Complementary contact sensing.** Approximate geometry에서 tactile과 wrist F/T를 함께 사용하는 policy가 vision/proprioception-only보다 geometry perturbation과 접촉 불확실성에 강하다. | `no contact sensor`, `tactile only`, `F/T only`, `tactile+F/T`의 factorial ablation; 동일 OBB error·randomization | Rotation-to-Push/final success, contact-loss recovery, peak force/impulse, collision, OBB-error sensitivity | 결합 입력이 held-out error에서 성공률을 높이고 force/collision을 악화시키지 않으며 각 단일 modality와 상보적 차이를 보임 | 결합 이득이 없거나 한 modality만으로 동일 성능, 또는 센서 추가가 noise에 더 취약함 |
| **H4 — Online wrist–finger adaptation.** 접촉 후 wrist와 finger를 함께 조절하는 policy가 fixed-hand 또는 wrist-only control보다 uncertainty 아래에서 성공률을 높인다. | Fixed hand, wrist-only, wrist+finger free adaptation, wrist+finger with mild motion cost | Final success, contact loss·switch, hand joint travel, peak force, recovery, seen–held-out gap | Joint adaptation이 held-out pose·friction·geometry에서 성공과 recovery를 높이고 과도한 force·motion을 만들지 않음 | Fixed/wrist-only와 차이가 없거나 finger motion이 불안정·과도하고 안전 지표가 악화됨 |

H2에서 `전체 episode 결과를 고려한다`는 말은 현재 explicit feasibility reward가 존재한다는 뜻이 아니다. Baseline은 full-episode return을 공유하며, 별도 explicit mechanism의 필요성은 Section 7에서 결정한다. 따라서 `sequential` 또는 `downstream-aware contact formation`은 현재 `[Hypothesis]`이며, H2와 transition analysis가 지지하기 전에는 contribution으로 표현하지 않는다.

---

## 6. Provisional Baseline Method

이 절은 contribution이 아니라 **가설을 검증하기 위한 최초 구현안**이다.

### 6.1 Policy organization

- `[Baseline]` 하나의 shared MLP policy가 세 sub-objective를 수행한다.
- `[Baseline]` Actor에 phase ID를 주지 않고 current task/contact state에 따라 reward term을 gate한다.
- `[Baseline]` 동일한 action space를 모든 phase에서 사용하여 Rotation 중 translation과 Push 중 orientation correction을 허용한다.
- `[Open]` Shared phase-free policy의 이점은 아직 입증되지 않았다. `phase ID 포함 shared policy`와 `phase-specific policies/controller`를 비교해야 한다.
- `[Rejected]` Shared policy 또는 phase-ID 제거 자체를 검증 없이 contribution으로 표현하는 방식.

### 6.2 Current observation snapshot

현재 66D 구성은 구현 가능한 첫 baseline일 뿐 연구 범위가 아니다.

| Observation block | Current encoding | Dimension | Status |
| --- | --- | ---: | --- |
| Task goal and selected-face state | EEF-frame target position, push direction, current selected-face pushing normal | 9 | `[Baseline]` |
| Current object pose | EEF-relative position + canonical quaternion | 7 | `[Baseline]` |
| Geometry | Object-local OBB extent | 3 | `[Baseline]` |
| Arm/hand proprioception | UR5e q + RH56E2 q | 12 | `[Baseline]` |
| Tactile | Current 17D binary vector | 17 | `[Baseline]` |
| Wrist F/T | Current EEF-frame wrench | 6 | `[Baseline]` |
| Minimal action memory | Previous action one step | 12 | `[Baseline]` |
| **Total** |  | **66** | `[Baseline]` |

- `[Baseline]` Tactile은 fixed sensor identity를 유지하는 17D any-contact binary vector다.
- `[Baseline]` 하나의 threshold를 사용하고 hysteresis는 사용하지 않는다.
- `[Baseline]` Tactile과 F/T는 current sample, previous action은 one-step만 사용한다.
- `[Open]` Threshold, filtering, history, action memory와 exact sensor mapping은 실물 log와 ablation으로 결정한다.
- `[Open]` Selected-face representation과 OBB consistency가 unseen/non-box-like objects에서 충분한지 검증한다.

### 6.3 Action and controller

| Element | Current choice | Status |
| --- | --- | --- |
| Wrist action | EEF-frame delta translation 3D + local rotation increment 3D | `[Baseline]` |
| Finger action | RH56E2 actuated joint action 6D | `[Baseline]` |
| Total action | 12D | `[Baseline]` |
| Command semantics | 매 policy step의 measured state를 기준으로 새 command 생성 | `[Baseline]` |
| Low-level controller | Differential IK 또는 OSC | `[Open]` |
| Scale/frequency | Translation·rotation·finger scale와 policy rate | `[Open]` |

6-DoF wrist와 finger action을 함께 쓰는 이유는 contact location, force direction과 configuration을 online으로 바꿀 자유도를 제공하기 위해서다. 이것이 모두 필요하다는 주장은 H4의 control ablation으로 검증한다.

### 6.4 Reward and safety structure

| Layer | Baseline role | Examples | Status |
| --- | --- | --- | --- |
| Final task objective | 목표 이동과 face-direction alignment, stable completion | Sparse success | `[Baseline]` |
| Phase-specific shaping | Exploration과 credit assignment 보조 | Approach distance, alignment progress, push progress | `[Baseline]` |
| Contact shaping | 전체 접촉 상실을 줄이되 개별 contact migration 허용 | Aggregate contact-loss event | `[Baseline]` |
| Safety constraint | Task progress와 교환되지 않아야 하는 위반 | Shelf/pillar collision, boundary exit, topple, overload, joint limit | `[Baseline]` |
| Control regularization | 불필요한 진동·명령 변화 억제 | Action rate, optional hand-motion cost | `[Baseline]` |

- `[Open]` 각 term의 수식, weight, threshold, curriculum과 constrained-RL/termination 방식은 확정하지 않았다.
- `[Fixed]` Reward term과 evaluation metric을 같은 것으로 취급하지 않는다. Reward는 학습 신호이고 metric은 가설 검증을 위한 외부 측정이다.
- `[Baseline]` Contact loss는 회복 가능한 penalty event이며 자동 failure termination이 아니다.
- `[Open]` Contact 유지 penalty와 hand-motion cost가 실제로 필요한지 H4에서 비교한다.

### 6.5 Current meaning of next-phase consideration

다음 세 수준을 구분한다.

| Level | Meaning | Current use | Status |
| --- | --- | --- | --- |
| N1 — Shared episodic return | 앞 단계 action이 뒤 phase reward와 final success return을 함께 받음 | 현재 학습 baseline | `[Baseline]` |
| N2 — Transition evaluation | 저장한 Approach/Rotation state에서 continuation rollout을 실행하여 다음 단계 성공률을 측정 | 반드시 유지할 evaluation | `[Baseline]` |
| N3 — Explicit downstream learning | Learned feasibility value, transition reward, terminal-state critic 또는 별도 objective로 다음 phase 가능성을 직접 최적화 | 구현 여부 미정 | `[Open]` |

현재 method에 명확히 존재하는 것은 N1과 N2다. N3가 없는 상태에서 `explicit downstream-feasibility learning method`를 contribution으로 확정하지 않는다.

### 6.6 Contact and reconfiguration behavior

- `[Baseline]` 최초 접촉 이후 적어도 하나의 hand–blocker contact를 가급적 유지한다.
- `[Baseline]` 동일 finger, taxel 또는 contact patch를 계속 유지하도록 강제하지 않는다.
- `[Baseline]` Rotation과 Push 중 contact migration, release/re-contact와 wrist–finger adjustment를 허용한다.
- `[Open]` Aggregate contact를 계속 유지하는 것이 항상 유리한지, deliberate full release가 필요한 object/task가 있는지 검토한다.
- `[Rejected]` Tactile bit change 자체를 failure 또는 reconfiguration penalty로 해석하는 방식.

---

## 7. Open Decisions

### 7.1 OD-1 — Surrounding Objects

현재 문서는 `shelf scene`, `clutter`와 `forbidden collision`을 혼용해 왔지만 surrounding-object condition은 확정되지 않았다.

| Question | Alternatives | Evidence/decision needed | Current status |
| --- | --- | --- | --- |
| 주변 물체가 항상 존재하는가? | 없음 / target만 존재 / 추가 passive objects 존재 | Stage 1의 최소 task와 실제 shelf relevance 비교 | `[Open]` |
| 수와 위치를 randomize하는가? | Fixed layout / count·pose randomization / curriculum | 학습 안정성 및 generalization protocol | `[Open]` |
| Blocker–surrounding-object contact를 허용하는가? | 모두 금지 / 제한적 contact 허용 / manipulation에 이용 | 실제 application에서 허용 가능한 물리적 상호작용 정의 | `[Open]` |
| 주변 물체의 연구상 역할은 무엇인가? | Safety obstacle / policy가 대응할 uncertainty / active interaction 대상 | Observation에 scene state가 필요한지와 collision-only 처리가 충분한지 검증 | `[Open]` |
| Single- vs. multi-object 실험을 분리하는가? | 단일 핵심 실험 후 multi-object stress test / 처음부터 혼합 | H1–H4 attribution과 연구 범위 간 trade-off | `[Open]` |

현재 권장 분리는 다음과 같다.

- `[Baseline]` S0: 추가 movable surrounding object 없이 한 blocker만 사용하여 H1–H4를 식별한다.
- `[Candidate]` S1: 1–N개의 passive surrounding objects를 추가하고 contact는 forbidden collision로 처리하는 robustness test.
- `[Open]` S2: Surrounding-object contact를 허용하거나 이용하는 multi-object contact manipulation. 채택하면 observation과 research scope가 크게 바뀐다.

사용자는 S0만을 논문의 주 조건으로 둘지, S1을 필수 evaluation으로 둘지 우선 결정해야 한다.

### 7.2 OD-2 — Geometry Information and Perturbation

OBB를 쓴다는 사실보다 `무엇을 알고 얼마나 틀리는가`를 먼저 정의해야 한다.

| Decision | Required clarification | Current status |
| --- | --- | --- |
| Geometry source | Known dimensions, perception-estimated OBB, category template 중 무엇인지 | `[Open]` |
| Pose source | Marker/vision pose와 geometry source를 분리하고 frame registration 방법 정의 | `[Open]` |
| OBB error components | Center offset, orientation error, extent scale/asymmetry, temporal jitter, axis permutation을 분리 | `[Open]` |
| Error distribution | Uniform 임의값이 아니라 실제 perception log 또는 명시적 stress range 사용 | `[Open]` |
| Exact baseline | Oracle OBB만 둘지 registered CAD/mesh upper bound까지 둘지 | `[Open]` |
| Implicit geometry comparator | Point cloud/RGB feature policy를 구현할지 문헌 비교로만 둘지 | `[Open]` |

Core geometry ladder는 다음과 같이 제안한다.

| Code | Condition | Purpose | Status |
| --- | --- | --- | --- |
| G0 | Pose only, no geometry | Geometry input의 최소 하한 | `[Candidate]` |
| G1 | Perturbed approximate OBB | 주 연구 조건 | `[Baseline]` |
| G2 | Oracle OBB dimensions/frame | OBB estimation error의 upper bound | `[Candidate]` |
| G3 | Registered exact mesh/CAD | Contact geometry까지 아는 upper bound | `[Open]` |

Coarse geometry와 tactile/F/T의 보완 관계를 주장하려면 최소한 G0–G2가 필요하다. G3는 exact local geometry까지의 gap을 해석하는 데 강하지만 구현 비용과 baseline fairness를 검토해야 한다.

### 7.3 OD-3 — Next-Phase Consideration

| Interpretation | What it demonstrates | What it does not demonstrate | Current status |
| --- | --- | --- | --- |
| Shared full-episode return | 앞 단계가 전체 성공 return의 영향을 받을 가능성 | Credit가 실제로 전달되었거나 terminal state가 개선되었다는 보장 | `[Baseline]` |
| Factorized transition metric | Rotation success와 Rotation-to-Push success가 다른지 측정 | Policy가 그 차이를 명시적으로 학습했다는 증거 | `[Baseline]` |
| Explicit feasibility objective | 다음 phase 성공 가능성을 직접 학습·shaping | 별도 estimator의 calibration·OOD 안정성 | `[Open]` |

우선 N1+N2로 H2를 검증한다. Phase-local baseline보다 transition metric이 개선되지 않거나 long-horizon credit failure가 나타날 때 N3를 추가한다. N3를 추가하기 전에는 `downstream-aware learning algorithm`을 확정 contribution으로 쓰지 않는다.

### 7.4 Additional open decisions

| ID | Decision | Why unresolved | Required evidence |
| --- | --- | --- | --- |
| OD-4 | Selected face+direction vs. explicit desired rotation goal | OBB symmetry와 non-box-like object에서 동일하지 않을 수 있음 | Task set과 goal ambiguity analysis |
| OD-5 | Shared/phase-free vs. phase-aware/phase-specific policy | 현재 선택의 성능·sample-efficiency 근거가 없음 | Matched policy-organization ablation |
| OD-6 | Initial non-contact state distribution | 너무 쉬운 sampling은 Approach 가설을 약화하고 너무 넓으면 학습을 막음 | Planner/reachability 기반 분포와 curriculum study |
| OD-7 | DiffIK vs. OSC, action scale·rate | Contact response와 sim-to-real behavior가 달라짐 | Controller-matched rollout statistics and hardware limits |
| OD-8 | Tactile threshold, F/T preprocessing, history | Sensor calibration과 latency를 아직 측정하지 않음 | RH56E2/F/T logs, synchronization and noise analysis |
| OD-9 | Reward gate, weight와 safety algorithm | 문헌의 수치를 직접 이식할 수 없음 | Unweighted-term distribution, failure analysis, ablation |
| OD-10 | Exact held-out object/generalization boundary | `unseen`의 범위가 category·shape·physics 중 무엇인지 불명확 | Train/test split definition |
| OD-11 | Real-world deployment protocol | Tracker, sensor calibration과 safety supervisor interface 미정 | Hardware integration plan |

### 7.5 Contradiction and over-specification audit

| Item | Existing Statement | Problem | Required Decision/Evidence | Current Status |
| --- | --- | --- | --- | --- |
| 연구 범위와 구현 범위 | Selected-face goal, 66D observation과 shared MLP가 현재 연구 정의처럼 서술됨 | 연구 문제와 최초 구현 선택이 혼합됨 | H1–H4에 필요한 최소 interface를 확인하고 구현안은 ablation으로 변경 가능하게 유지 | `[Baseline]`으로 재분류 |
| Fully observable 표현 | Continuous pose와 OBB가 있으므로 상태를 충분히 안다는 암묵적 전제 | Local contact, friction, mass와 geometry error가 남음 | Partial observability와 hidden physics를 perturbation별로 정의 | `fully observable`은 `[Rejected]` |
| Vision과 privileged simulator information | Continuous vision pose와 exact simulator state가 같은 state처럼 사용될 수 있음 | 실물 perception과 simulation teacher의 가용성이 다름 | Actor tensor와 reward/constraint/evaluation tensor를 코드·표에서 분리 | 경계 `[Fixed]`, 구현 검증 `[Open]` |
| Policy input과 reward/evaluation 정보 | Exact contact force·collision을 reward에 쓰므로 policy도 이를 활용한다고 오해 가능 | Deployment information leakage 위험 | Asymmetric critic 포함 여부까지 별도 목록과 unit test 작성 | Actor/teacher 분리 `[Fixed]` |
| Surrounding objects | Shelf/clutter를 말하면서 다른 물체는 모두 forbidden collision로 처리 | 존재 여부, randomization과 연구상 역할이 정의되지 않음 | S0/S1/S2 중 main condition과 evaluation condition 결정 | OD-1 `[Open]` |
| Shared policy 사용 이유 | 세 phase를 하나의 shared policy로 학습하는 것이 자연스러운 선택처럼 서술 | Phase-specific policy 대비 장점과 실패 조건이 미검증 | 동일 capacity·budget의 shared vs. phase-specific E6 | Shared MLP `[Baseline]` |
| Phase ID 제거 이유 | 이전 연구 경험상 gate만으로 가능하므로 phase ID를 제거 | Markov state sufficiency와 성능상 이점이 본 task에서 미검증 | No-ID vs. phase-ID shared policy 및 hidden-gate ambiguity 분석 | No-ID `[Baseline]`, 우위 `[Open]` |
| Hand configuration 평가 시점 | Approach 종료 state를 downstream-ready configuration으로 평가 | Rotation·Push 중 계속 변하는 configuration을 설명하지 못함 | Approach terminal continuation과 전체 trajectory의 contact/joint metrics를 함께 기록 | Terminal+trajectory 평가 `[Baseline]` |
| Approach contact 유지 | 최초 접촉을 Rotation·Push까지 유지하는 것이 효율적이라는 직관 | 필요한 release/re-contact를 막을 수 있음 | Aggregate contact-loss와 contact-switch/recovery를 분리 측정 | Soft preference `[Baseline]`, 강도 `[Open]` |
| Rotation 이후 reconfiguration | Approach에서 좋은 configuration을 만들면 이후 재구성이 적어야 한다는 서술 | Fixed hand가 조작 범위와 robustness를 제한할 가능성 | H4 fixed/free/mild-cost ablation | Minimum-necessary reconfiguration `[Hypothesis]` |
| Tactile과 F/T 역할 | 두 센서가 geometry error를 함께 보완한다고 포괄적으로 서술 | 제공 정보와 중복·상보성이 분리되지 않음 | Tactile은 spatial contact identity, F/T는 net wrench라는 가설을 E3 factorial ablation으로 검증 | 역할 구분 `[Hypothesis]` |
| 6-DoF wrist와 finger control | 둘을 모두 action에 포함하는 12D interface | 모든 자유도가 실제로 필요하다는 근거가 아직 없음 | Wrist-only, fixed-finger, joint adaptation comparison 및 controllability/failure analysis | 12D `[Baseline]`, 필요성 H4 |
| Reward term과 evaluation metric | Alignment, contact와 success가 reward와 metric 양쪽에 등장 | Reward 최적화가 곧 가설 입증이라는 순환 논증 위험 | 독립 metric 계산, held-out perturbation과 continuation rollout 사용 | 분리 원칙 `[Fixed]` |
| Contribution/novelty | Downstream-aware transition, multimodal adaptation과 unified policy가 가능한 contribution으로 서술 | Explicit mechanism과 matched experimental evidence가 아직 없음 | C1–C4 각각의 required experiment와 closest-work comparison 완료 | 모두 `[Candidate]` |

---

## 8. Candidate Contributions

현재 항목은 모두 `[Candidate]`다.

| Candidate Contribution | 필요한 Method 요소 | 필요한 비교 실험 | 현재 근거 수준 | 확정 조건 |
| --- | --- | --- | --- | --- |
| **C1. Contact feedback을 통한 approximate-geometry error 보완** | Approximate OBB, deployable tactile/F/T feedback과 closed-loop correction | G0/G1/G2 geometry ladder, tactile × F/T factorial ablation, geometry-error sweep | 센서별 선행 근거와 H3 설계만 존재; 본 task의 보완 효과는 미검증 | Geometry error가 증가할 때 contact feedback이 vision/proprioception-only 대비 Rotation-to-Push와 final success의 저하를 유의하게 줄임 |
| **C2. 이후 Rotation·Push에 적합한 contact-state formation과 online wrist–finger adaptation** | Goal-conditioned Approach/Rotation, continuation evaluation, wrist+finger action | Contact-only/phase-local objective, fixed hand, wrist-only, free adaptation 비교 | H2/H4와 관련 문헌만 존재; sequential 이점과 adaptation 효과 모두 미검증 | Contact/Rotation 성공을 통제한 뒤에도 downstream success가 높고, held-out uncertainty에서 online adaptation의 추가 이점이 확인됨 |
| **C3. Rotation success와 Rotation-to-Push success의 factorized evaluation** | Saved-state continuation protocol과 transition metrics | Face-alignment success를 통제한 Push continuation 분석 | 현재 corpus에서 직접 같은 protocol은 확인되지 않았으나 skill-transition 관련 근거가 있음 | 두 metric의 차이가 실제 failure를 설명하고 method 비교의 결론을 바꿈 |
| **C4. Approach–Rotation–Push goal-conditioned low-level policy** | 세 sub-objective를 연결하는 policy와 안전·contact handling | Direct push, simple policy composition, phase-specific와 shared policy | 통합 system framing 수준 | 단순 결합 baseline보다 일관된 이점과 원인 ablation이 있고 단순 engineering integration을 넘는 요소가 확인됨 |

Tactile 또는 wrist F/T의 사용 자체는 contribution이 아니다. C1/C2는 contact feedback이 **approximate geometry의 오차를 보완하고**, 이후 Rotation과 Push에 적합한 접촉 상태의 형성 및 online adaptation에 기여한다는 결과가 matched baseline과 ablation에서 확인될 때만 확정한다. `Sequential` 또는 `downstream-aware contact formation`도 같은 이유로 H2가 지지되기 전에는 `[Candidate]`를 유지한다.

다음은 contribution으로 단독 주장하지 않는다.

- `[Rejected]` Tactile 사용 자체, F/T 사용 자체, RL 사용 자체.
- `[Rejected]` Rotate-then-push라는 순서 자체.
- `[Rejected]` MLP, 66D observation 또는 12D action이라는 구현 수치.
- `[Rejected]` 기존 IL/VLA가 contact feedback을 사용하지 못한다는 일반화.
- `[Rejected]` OBB가 point cloud/mesh보다 일반적으로 우월하다는 주장.

---

## 9. Evaluation and Ablation Plan

### 9.1 Evaluation layers

| Layer | Question | Primary metrics | Status |
| --- | --- | --- | --- |
| Task execution | Goal을 정확하고 안전하게 실행하는가? | Final success, axial/lateral error, alignment error, completion time | `[Baseline]` |
| Phase outcome | 각 sub-objective는 달성되는가? | Contact formation, Rotation success, Push success | `[Baseline]` |
| Transition quality | 앞 단계 종료 상태가 다음 단계에 유효한가? | Approach→Rotation success, Rotation→Push success, continuation probability | `[Baseline]` |
| Robustness | Geometry·pose·friction·mass perturbation에서 유지되는가? | Success degradation curve, recovery rate, seen–held-out gap | `[Baseline]` |
| Safety | 환경·물체·robot에 위험을 만들지 않는가? | Forbidden collision, boundary exit, topple, peak force/impulse, joint-limit violation | `[Baseline]` |
| Contact/control efficiency | 필요 이상으로 접촉과 hand motion을 바꾸지 않는가? | Contact loss/switch, hand joint travel, action variation, energy proxy | `[Candidate]` |
| System utility | Blocker 조작이 target access를 실제로 개선하는가? | Visibility, reachability, clearance, retrieval success | `[Open]` |

### 9.2 Factorized success definitions

- **Rotation success:** Selected face와 desired push direction이 tolerance 안에서 정렬되고 object가 안정적이다.
- **Rotation-to-Push success:** Rotation-success state에서 정해진 continuation controller/policy가 Push goal을 안전하게 달성한다.
- **Final task success:** 지정된 translation goal과 alignment/stability tolerance를 만족하고 hard safety failure가 없다.
- **Contact maintenance:** 학습상 선호되는 soft behavior이며 final success의 필요조건으로 자동 고정하지 않는다.
- `[Open]` 모든 tolerance, dwell time과 continuation horizon은 task scale과 rollout 분포를 보고 정한다.

### 9.3 Core experiment matrix

| Experiment | Main variable | Controlled factors | Answers |
| --- | --- | --- | --- |
| E1 | Direct push vs. rotate-then-push | Goal, object, sensing, controller, trial budget | H1 |
| E2 | Contact-only/phase-local vs. full-episode goal-conditioned objective | Observation, action, policy capacity | H2 |
| E3 | Tactile × F/T factorial ablation | Approximate OBB error, network, training budget | H3 |
| E4 | Fixed hand vs. wrist-only vs. wrist+finger | Sensor inputs, object split, controller rate | H4 |
| E5 | G0/G1/G2 geometry ladder | Sensors and policy capacity | Geometry–sensing interaction |
| E6 | Shared phase-free vs. phase-ID vs. phase-specific | Reward and total capacity as closely as possible | Baseline architecture decision |
| E7 | S0 vs. selected S1 surrounding-object condition | Object/goal split and sensor setup | Environment scope decision |

### 9.4 Fair comparison rules

1. 같은 initial-state distribution, object split, goal, observation availability, action/controller limit와 safety rule을 사용한다.
2. Network parameter count, training interaction, demonstration amount, pretraining과 real trial budget을 보고한다.
3. 기존 method를 확장한 baseline은 원 논문의 기능과 추가한 component를 분리해 기록한다.
4. Literature-only reference와 실제 재현·재학습한 experimental baseline을 구분한다.
5. Success rate만이 아니라 transition, safety, contact와 efficiency metric을 함께 보고한다.
6. 여러 seed와 confidence interval을 사용하고, 유의성뿐 아니라 practical effect size를 정한다.
7. Reward에 사용한 신호로만 성공을 평가하지 않고 독립 evaluation 계산을 유지한다.

---

## 10. Previous Works Classification Criteria

### 10.1 두 문헌 흐름과 연구의 교차점

Previous Works의 주 구조는 method family의 단순 나열이 아니라 다음 두 흐름으로 구성한다.

| Literature stream | 포함할 세부 주제 | 각 흐름에서 확인할 질문 | 현재 연구와의 관계 |
| --- | --- | --- | --- |
| **A. Nonprehensile manipulation** | Pushing, pulling, pivoting, dexterous-hand contact/pose planning | 목표 물체 운동을 위해 contact pose, hand configuration과 contact-mode transition을 어떻게 정하고 실행하는가? | Task category와 Approach–Rotation–Push 구조의 직접 근거·baseline을 제공 |
| **B. Contact-feedback-based manipulation** | Tactile feedback, wrist F/T feedback, contact-state estimation, online adaptation | 실행 중 실제 접촉·힘·동역학 오차를 어떤 sensing과 feedback mechanism으로 추정·보정하는가? | Approximate geometry 아래의 closed-loop correction과 wrist–finger adaptation 근거·baseline을 제공 |
| **A ∩ B. 본 연구가 검증할 교차점** | Approximate geometry를 이용한 전역 접근/goal 설정 + deployable contact feedback을 이용한 접촉 상태 조정 | Contact feedback이 geometry approximation error를 보완하고 이후 Rotation·Push에 적합한 상태와 robust execution을 만드는가? | H2–H4와 E2–E5로 검증하며, 결과 전에는 gap·contribution을 `[Candidate]`로 유지 |

초기 literature map은 다음과 같다. 한 논문이 두 흐름에 걸칠 수 있으며, 최종 분류는 full text에서 실제 observation, feedback loop와 action을 확인한 뒤 확정한다.

| Stream | Subtopic | Representative starting points | Reading purpose |
| --- | --- | --- | --- |
| A | Pushing / cluttered pushing | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166), [Goal-Oriented Non-Prehensile Pushing](https://doi.org/10.1109/IROS47612.2022.9981873) | Goal·collision·toppling 조건과 unknown-object pushing baseline 확인 |
| A | Pulling and direction-conditioned hand pose | [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Pushing/pulling direction에 맞춘 dexterous pre-contact pose의 범위 확인 |
| A | Rotation / pivoting | [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262), [Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) | Target orientation, contact/force guidance와 object generalization 비교 |
| A | Dexterous contact/pose planning | [Learning Contact Locations](https://doi.org/10.1109/HUMANOIDS.2013.7030011), [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652), [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Contact location·hand pose가 manipulation goal에 조건화되는 방식 확인 |
| B | Tactile feedback | [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236), [DexTouch](https://doi.org/10.1109/LRA.2024.3478571), [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052) | Tactile encoding, reactive correction과 sim-to-real 효과 확인 |
| B | Wrist F/T feedback and force-aware control | [COCOI](https://doi.org/10.1109/IROS51168.2021.9636836), [ForceVLA](https://doi.org/10.52202/085713-3124), [Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Net wrench, online context와 force-guided execution의 역할 확인 |
| B | Contact-state estimation | [Active Extrinsic Contact Sensing](https://doi.org/10.1109/ICRA46639.2022.9812017), [1 kHz Behavior Tree for Self-adaptable Tactile Insertion](https://doi.org/10.1109/ICRA57147.2024.10610835) | Contact onset/loss/mode 추정과 controller 전환의 역할 확인 |
| B | Online adaptation | [COCOI](https://doi.org/10.1109/IROS51168.2021.9636836), [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744) | Hidden dynamics 또는 task execution 중 configuration 보정의 추가 가치 확인 |

`Method Family`는 두 흐름을 대체하는 상위 분류가 아니라, 각 논문의 action generation과 학습 방법을 기록하는 **보조 metadata**로 사용한다.

### 10.2 Main comparison table의 column

각 column은 하나의 질문만 답한다.

| Group | Column | Single question | Allowed coding |
| --- | --- | --- | --- |
| Environment | Surrounding Objects | 실행 중 조작 대상 외 물체가 존재하는가? | Absent / Present; 개수·이동성·접촉 허용은 별도 environment-detail 표에 기록 |
| Environment | Geometry Information | Deployment policy가 받는 explicit/implicit shape 정보는 무엇인가? | None / Approximate explicit / Exact explicit / Implicit visual |
| Agent | Task-Conditioned Hand Configuration | Hand/contact configuration이 manipulation goal에 조건화되는가? | No / Pre-contact only / Online |
| Agent | Tactile Feedback | Tactile이 실행 중 policy feedback으로 사용되는가? | No / Binary-discrete / Continuous-spatial |
| Agent | Wrist F/T Feedback | Wrist force/torque가 실행 중 policy feedback으로 사용되는가? | No / Force only / 6-axis wrench |
| Agent | Online Wrist–Finger Adaptation | 접촉 후 wrist와 finger를 모두 online으로 변경하는가? | None / Wrist only / Finger only / Both |
| System | Method Family | 핵심 action-generation/training family는 무엇인가? | Heuristic / Optimization or Generative / RL / IL / VLA |

`Subsequent-Action Transition`은 현재 main table column으로 넣지 않는다. 논문이 실제로 terminal-state feasibility, skill chaining 또는 continuation success를 학습·평가하는 경우에만 별도 transition-analysis table에서 비교한다.

### 10.3 Evidence and inclusion rules

- 공식 출판 페이지와 full text를 먼저 확인한다.
- DOI는 publisher metadata와 대조한다. DOI가 없으면 `No DOI—preprint` 또는 공식 proceedings URL로 표시한다.
- Title/abstract만으로 observation, reward, online feedback 또는 surrounding-object capability를 추론하지 않는다.
- ArXiv-only work는 task directness와 연구 조직의 최신 방향을 보여주는 경우에 제한적으로 사용하고 peer-reviewed 근거와 구분한다.
- Tactile/F/T 사용 유무만으로 우열이나 novelty를 판단하지 않는다.
- Dense clutter, surrounding-object contact와 collision avoidance를 다룬 연구를 의도적으로 제외하지 않는다.
- Heuristic, optimization/generative, RL, IL와 VLA를 균형 있게 포함하되, data·compute·sensor 조건이 다른 방법의 raw success rate를 직접 비교하지 않는다.
- 최근 연구를 우선하되 reward·contact mechanics의 직접 근거가 되는 오래된 연구는 `foundational comparator`로 별도 표시한다.

### 10.4 균형 잡힌 초기 후보군

이 표는 최종 baseline 선정이 아니라 비교표를 채울 출발점이다.

| Literature role | Method family | Representative work | Why it must be considered | Status |
| --- | --- | --- | --- | --- |
| A — implementation baseline | Heuristic | Nearest feasible OBB face 선택 + scripted normal alignment + straight push | Learned contact formation과 Rotation sequence의 이득을 분리하는 최소 비교군; 특정 논문의 방법으로 잘못 귀속하지 않음 | `[Candidate]` |
| A — geometry/contact selection | Learning-based contact selection | [Learning Contact Locations for Pushing and Orienting Unknown Objects](https://doi.org/10.1109/HUMANOIDS.2013.7030011) | Straight push와 orienting contact의 관계를 다룬 foundational comparator | `[Candidate]` |
| A — dexterous pose planning | Optimization | [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652) | Task wrench에 조건화된 hand pose 평가 | `[Candidate]` |
| A — dexterous pose planning | Generative geometry-conditioned | [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Direction-conditioned dexterous pre-contact pose; preprint status를 명시 | `[Candidate]` |
| A ∩ B — tactile pushing | Goal-conditioned RL | [Tactile Pushing](https://doi.org/10.1109/LRA.2023.3295236) | Vision 없이 tactile feedback으로 pushing을 제어한 직접 사례 | `[Candidate]` |
| A — object pushing | Constrained RL | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166) | Unknown-object pushing과 task/safety 분리 | `[Candidate]` |
| A — cluttered pushing | RL | [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) | Surrounding-object collision을 포함한 pushing | `[Candidate]` |
| B — reactive feedback | IL | [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052) | IL도 high-frequency tactile/force feedback에 반응할 수 있다는 강한 반례 | `[Candidate]` |
| B — force feedback | VLA | [ForceVLA](https://doi.org/10.52202/085713-3124) | VLA가 force modality를 다루지 못한다는 단순 비판을 방지 | `[Candidate]` |
| System boundary comparator | VLA | [OpenVLA-OFT](https://doi.org/10.15607/RSS.2025.XXI.017) | VLA latency와 adaptation 개선을 인정하기 위한 high-impact reference | `[Candidate]` |
| B — online configuration adaptation | Residual policy | [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744) | Task-informed initial grasp와 residual joint adaptation의 결합 | `[Candidate]` |

최종 previous-works table은 논문별 full-text 검증 후 작성한다. 이 후보군만으로 novelty를 결론내리지 않는다.

---

## 11. Claim–Evidence–Method Matrix

`Evidence`는 선행연구가 자신의 설정에서 보여준 결과이고, `Remaining Gap`은 그 결과를 바탕으로 한 현재 연구팀의 판단이다.

| Research Claim | Evidence from Previous Works | Remaining Gap | Proposed Method Element | Required Experiment | Status |
| --- | --- | --- | --- | --- | --- |
| Direct push만으로 처리하기 어려운 상태에는 preparatory orientation/contact change가 필요할 수 있다. | [Learning Contact Locations](https://doi.org/10.1109/HUMANOIDS.2013.7030011)은 pushing과 orienting을 위한 contact location을 구분하고, [Learning Generalizable Pivoting](https://doi.org/10.1109/ICRA48891.2023.10161271)은 target-orientation pivoting을 학습한다. | Shelf blocker에서 rotation이 실제 final Push 성공 영역을 넓히는 조건은 아직 본 task에서 검증되지 않음 | Rotation을 포함한 goal-conditioned execution | E1 initial-state-stratified comparison | `[Hypothesis]` |
| 좋은 contact formation은 접촉 여부가 아니라 task goal에 대한 실행 가능성으로 평가해야 한다. | [TaskDexGrasp](https://doi.org/10.1109/IROS58592.2024.10802652)은 task wrench에 맞는 hand pose를, [RL-Critic Grasp Selection](https://doi.org/10.1109/ICRA55743.2025.11127792)은 downstream manipulation critic으로 grasp를 평가한다. | Static/task-local score가 Approach→Rotation→Push continuation을 예측하는지 불명확 | Goal-conditioned Approach와 saved-state continuation evaluation | E2와 transition calibration | `[Hypothesis]` |
| Vision/coarse geometry는 전역 task·접근 정보를, contact sensing은 실행 중 실제 접촉과 geometry mismatch에 대한 feedback을 제공한다. | [Coarse-to-Fine Pushing](https://doi.org/10.1109/LRA.2024.3511378)은 vision과 touch/proprioception의 역할을 나누고, [DexTouch](https://doi.org/10.1109/LRA.2024.3478571)은 tactile feedback의 sim-to-real utility를 보였다. | Coarse OBB와 binary tactile+wrist F/T 조합이 selected-face rotation-to-push에서 실제 geometry error를 얼마나 보완하는지 불명확 | Approximate OBB + tactile + wrist F/T observation and closed-loop wrist–finger correction | E3 sensor ablation과 E5 geometry-error interaction | `[Hypothesis]` |
| Tactile와 F/T의 추가가 자동으로 상보성을 의미하지는 않는다. | [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052)과 [ForceVLA](https://doi.org/10.52202/085713-3124)는 IL/VLA도 tactile/force feedback을 사용할 수 있음을 보인다. | 저차원 binary tactile와 wrist wrench의 독립·결합 효과가 분리되지 않음 | Factorial sensor ablation | E3 | `[Hypothesis]` |
| Task-conditioned initial configuration과 online adaptation은 구분해서 평가해야 한다. | [GD2P](https://doi.org/10.48550/arXiv.2509.18455)는 direction-conditioned pre-contact pose를, [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744)는 initial grasp와 residual adaptation을 결합한다. | Nonprehensile Rotation→Push에서 wrist–finger adaptation의 추가 가치가 불명확 | Joint wrist–finger action and adaptation | E4 | `[Hypothesis]` |
| Rotation success는 subsequent Push feasibility와 동일하지 않을 수 있다. | [Value-Informed Skill Chaining](https://doi.org/10.1109/IROS55552.2023.10342180)과 [Sequential Dexterity](https://doi.org/10.48550/arXiv.2309.00987)는 skill transition/feasibility의 중요성을 보여준다. | 현재 검토 corpus에서 face alignment와 immediate Push continuation을 분리한 동일 protocol은 확인되지 않음 | Rotation vs. Rotation-to-Push factorized metrics | Saved-state continuation and E2 | `[Candidate]` |
| Safety는 task reward와 분리해 해석해야 한다. | [Dynamic Object Goal Pushing](https://doi.org/10.1109/ICRA55743.2025.11128166)은 task reward와 collision·toppling constraint를 분리한다. | Shelf structure, multi-finger force와 boundary condition에 맞는 constraint 정의가 필요 | Privileged safety cost/termination and hardware supervisor | Safety ablation은 simulation에 한정; threshold validation | `[Baseline]` |
| Surrounding objects는 단순 collision constraint와 policy가 대응할 contact-interaction variable 중 하나로 정의되어야 한다. | [Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873)은 clutter collision을 포함한 goal pushing을 다룬다. | 현재 연구가 주변 물체 접촉을 금지할지 이용할지 정하지 않음 | S0/S1/S2 environment tiers | E7 after OD-1 decision | `[Open]` |
| Explicit downstream learning이 필요한지는 아직 확인되지 않았다. | 기존 skill-chaining·critic 연구는 explicit transition/value modeling의 가능성을 제공한다. | Shared full-episode return만으로 H2가 학습되는지 미확인 | N1+N2 baseline; N3 only if needed | E2, credit/failure diagnostics | `[Open]` |

연결되지 않은 요소에 대한 현재 판단은 다음과 같다.

- Previous action, exact quaternion encoding, tactile threshold와 reward gate 식은 implementation diagnostics에는 필요할 수 있지만 독립 research claim과 직접 연결되지 않는다. 따라서 `[Baseline]` 또는 `[Open]`으로 유지한다.
- Surrounding-object robustness는 중요한 gap이지만 main scope로 채택하기 전에는 이를 검증할 observation/method가 없다. OD-1 결정 전에는 contribution에 포함하지 않는다.
- `downstream-aware`라는 표현은 N1, N2, N3 중 무엇을 뜻하는지 항상 명시한다.

---

## 12. Terminology and Claim Boundaries

### 12.1 Core terminology

| Term | Controlled meaning |
| --- | --- |
| Nonprehensile manipulation | 안정적인 grasp/lift 없이 pushing, pulling 또는 pivoting 등의 접촉으로 물체를 이동·재배치하는 본 연구의 task category |
| Contact-rich interaction/control challenge | 접촉 위치·마찰·stick–slip–separation과 힘 전달의 불확실성을 sensing과 feedback control로 다루는 실행상의 challenge; 본 문서의 task category가 아님 |
| Blocker manipulation | Target access를 방해하는 물체를 재배치하는 Stage 1의 직접 과업 |
| Approach / Contact Formation | 다음 조작을 시작할 object–hand relation과 contact state를 형성하는 구간; 단순 거리 감소와 동의어가 아님 |
| Rotation / Pivoting | Push에 필요한 object/contact orientation을 형성하는 preparatory manipulation |
| Push / Translation | 정렬을 유지·보정하며 지정 방향·거리로 blocker를 이동하는 구간 |
| Task-conditioned hand configuration | Contact 수가 아니라 selected manipulation goal에 조건화된 wrist–finger/contact state |
| Approximate geometry | 실제 국소 접촉 표면과 오차를 허용하는 명시적 coarse representation |
| Exact geometry | Pose와 정확히 정합된 CAD/mesh 또는 calibrated dimensions |
| Vision/coarse-geometry information | Blocker pose, nominal shape, task goal과 접근 관계를 제공하는 전역 정보; 실제 contact state를 직접 보장하지 않음 |
| Contact feedback | Tactile과 wrist F/T를 통해 실제 contact event/location 및 net wrench 변화를 관측하여 geometry·physics mismatch에 대응하는 실행 feedback |
| Online adaptation | Contact 이후 sensory feedback에 따라 wrist 또는 finger action을 계속 변경하는 것 |
| Aggregate contact maintenance | 동일 contact set을 고정하지 않고 적어도 하나의 hand–object support contact를 선호하는 것 |
| Transition evaluation | 앞 phase 종료 state에서 다음 phase rollout 성공을 별도로 측정하는 것 |
| Explicit downstream learning | 다음 phase feasibility를 별도 value/reward/objective로 직접 최적화하는 것 |

### 12.2 Actor, privileged information and metric

- **Actor observation:** 실제 deployment에서 같은 의미로 획득 가능한 policy input.
- **Privileged training information:** Simulator에서 reward, constraint, termination 또는 critic teacher로만 쓰는 exact state.
- **Evaluation metric:** 학습 reward와 독립적으로 가설을 판정하는 측정값.
- `[Rejected]` Reward에 들어갔다는 이유로 해당 정보가 actor observation이라고 표현하는 방식.
- `[Rejected]` Privileged contact force로 학습했다는 이유로 실물 tactile의 continuous force를 관측한다고 표현하는 방식.

### 12.3 Claim boundary register

| Previous/tempting statement | Updated boundary | Status |
| --- | --- | --- |
| 연구의 task category는 contact-rich manipulation이다. | Task category는 `nonprehensile manipulation`이고, contact-rich는 접촉 불확실성을 다루는 interaction/control challenge다. | `[Superseded]` |
| Continuous vision과 OBB가 있으므로 fully observable하다. | Pose·coarse geometry만 관측되며 contact/physics uncertainty가 남는다. | `[Superseded]` |
| Marker가 pose와 exact geometry를 모두 제공한다. | Marker는 pose source이며 geometry source·accuracy는 별도 정의한다. | `[Rejected]` |
| Selected-face goal이 연구의 확정 인터페이스다. | 유력한 baseline이며 explicit rotation goal과 비교 가능성을 열어 둔다. | `[Baseline]` |
| 66D observation과 12D action이 확정 연구 설계다. | 현재 구현 snapshot이며 실험과 hardware interface에 따라 변경 가능하다. | `[Baseline]` |
| Shared MLP와 phase-ID 제거가 method contribution이다. | 비교가 필요한 baseline organization이다. | `[Baseline]` |
| Full-episode reward를 공유하므로 explicit downstream feasibility를 학습한다. | Shared return은 N1일 뿐이며 N2 metric과 N3 mechanism을 구분한다. | `[Superseded]` |
| Rotation alignment success가 다음 Push 가능성을 의미한다. | Rotation success와 Rotation-to-Push success를 별도 측정한다. | `[Rejected]` |
| Approach contact를 이후에도 동일 taxel/finger로 유지해야 한다. | Aggregate support를 선호하되 contact migration과 re-contact를 허용한다. | `[Rejected]` |
| Tactile/F/T를 사용한다는 점이 novelty다. | 센서의 상보적 효과와 robustness gain이 H3에서 입증될 때만 method claim의 일부가 된다. | `[Rejected]` |
| Sequential/downstream-aware contact formation은 현재 contribution이다. | N1/N2/N3의 의미를 구분하고 H2에서 downstream success 향상이 입증될 때만 contribution 후보를 확정한다. | `[Rejected]` |
| IL은 reactive하지 않고 VLA는 force를 사용하지 못한다. | 최신 reactive IL과 force/tactile VLA가 반례다. Track B의 좁은 execution gap만 비교한다. | `[Rejected]` |
| OBB는 mesh/point cloud보다 현실적으로 항상 우월하다. | OBB는 deployability를 위한 baseline이며 정보 손실과 error를 geometry ladder로 평가한다. | `[Rejected]` |
| 최초 접촉 후 hand reconfiguration은 최소여야 한다. | Minimum-necessary reconfiguration은 H4에서 검증할 효율 가설이다. | `[Hypothesis]` |
| Surrounding objects는 모두 forbidden collision이다. | S0 baseline에서는 부재하며 S1/S2의 존재·접촉 규칙은 OD-1에서 결정한다. | `[Open]` |

### 12.4 Safe wording before experiments

사용 가능한 표현:

- `본 연구는 …을 검증한다.`
- `현재 baseline은 …으로 구성한다.`
- `기존 연구는 자신의 설정에서 …을 보였으며, 본 연구에는 …이 남는다.`
- `실험이 지지할 경우 contribution 후보는 …이다.`

사용하지 않는 표현:

- `본 방법은 기존 VLA/IL/RL의 한계를 극복한다.`
- `Tactile과 F/T를 사용하므로 robust하다.`
- `Shared policy이므로 phase transition을 학습한다.`
- `Unseen object에 일반화한다.` — unseen split이 정의되고 결과가 나오기 전에는 금지.

---

## 13. Work Roadmap

각 단계는 앞 단계의 산출물이 있어야 진행한다.

| Order | Work | Required output | Gate to next step | Status |
| ---: | --- | --- | --- | --- |
| 1 | 연구 범위와 open decisions 검토 | OD-1 surrounding, OD-2 geometry, OD-3 next-phase에 대한 사용자 결정 | 핵심 environment와 information condition이 고정됨 | `[Open]` |
| 2 | 핵심 가설 3–4개 확정 | H1–H4 문구, primary metric과 rejection criterion 승인 | 각 가설이 실제로 반증 가능함 | `[Open]` |
| 3 | 가설별 previous works 조사 | 가설마다 closest work, strong counterexample와 foundational work | Full text에서 비교 column의 사실을 확인 | `[Open]` |
| 4 | 논문별 근거 검증과 비교표 작성 | DOI-verified balanced comparison table + transition analysis table | 과장·누락 없이 gap이 남음 | `[Open]` |
| 5 | Baseline method 수정 | Goal, geometry, policy organization, observation/action/reward v1.0 | 모든 method 요소가 가설 또는 안전 요구와 연결됨 | `[Open]` |
| 6 | Experiment/ablation 설계 | E1–E7 protocol, split, metric, seed·trial budget | 각 contribution 후보의 판정 조건이 존재 | `[Open]` |
| 7 | Contribution 문장 확정 | 지지된 C1–C4만 남긴 claim set | 결과와 matched comparison이 확보됨 | `[Open]` |
| 8 | Motivation–Previous Works–Method storyline 작성 | Introduction/related-work narrative | Claim–Evidence–Method 연결이 완성됨 | `[Open]` |
| 9 | PPT 구성 | 발표용 slide hierarchy와 figures | 앞 단계의 확정 내용을 시각화 | `[Open]` |

### 13.1 Immediate next action

가장 먼저 다음 세 결정을 사용자와 확정한다.

1. **Surrounding objects:** S0를 main condition으로 하고 S1을 robustness evaluation으로 둘 것인가?
2. **Geometry ladder:** G0–G2를 필수로 하고 G3 exact mesh를 포함할 것인가?
3. **Next-phase mechanism:** N1+N2를 우선 검증하고 실패 시 N3를 추가할 것인가, 처음부터 N3를 method 핵심으로 설계할 것인가?

이 세 항목이 정리된 뒤 H1–H4와 previous-works 조사 범위를 확정한다.

---

## 14. Change Log

### 14.1 Current restructuring

| Date | Previous Definition | Updated Definition | Reason | Affected Sections |
| --- | --- | --- | --- | --- |
| 2026-09-17 | Shelf blocker 문제를 `contact-rich manipulation`이라는 task category로 표현 | 연구 정의를 **Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry**로 통일하고, `nonprehensile manipulation`은 task category, `contact-rich`는 interaction/control challenge로 분리 | 과업의 종류와 접촉 불확실성이라는 해결 과제를 혼동하지 않기 위해 | 1–3, 12 |
| 2026-09-17 | Previous Works를 method family와 개별 sensor 중심으로 배열 | Nonprehensile manipulation과 contact-feedback-based manipulation의 두 흐름 및 교차점으로 재구성하고 method family는 보조 metadata로 이동 | Research gap이 어느 두 연구축 사이에 있는지 명확히 하기 위해 | 10–11 |
| 2026-09-17 | Coarse-geometry contact formation과 tactile/F/T adaptation이 넓은 candidate contribution으로 제시 | Geometry-error 보완, downstream contact-state quality와 online adaptation 효과가 실험으로 확인될 때만 C1/C2를 확정하도록 조건 강화 | 센서 사용과 sequential framing 자체를 novelty로 오해하지 않기 위해 | 5, 8, 11–12 |
| 2026-09-17 | Fixed scope, 가설, 66D implementation과 contribution 후보가 하나의 `최신 합의`로 혼재 | `[Fixed] / [Hypothesis] / [Baseline] / [Open] / [Candidate]`로 재분류 | 연구 정의와 구현 snapshot을 분리하기 위해 | 1–13 전체 |
| 2026-09-17 | `research_topic.md`와 `policy_learning.md`의 현재안이 사실상 확정 명세처럼 읽힘 | 기존 문서는 참고 snapshot, 이 문서를 분류 기준으로 지정 | 다른 문서를 수정하지 않고 판단 기준을 안정화하기 위해 | 1.1 |
| 2026-09-17 | Selected face+direction, 66D observation, shared MLP와 phase-ID 제거가 현재 연구 정의에 포함 | 모두 provisional baseline으로 이동 | 실험 결과와 hardware interface에 따라 바뀔 수 있기 때문 | 3.2, 6, 12.3 |
| 2026-09-17 | `Downstream-aware`가 shared return, explicit reward와 transition metric을 혼용 | N1 shared return, N2 transition evaluation, N3 explicit learning으로 분리 | 현재 explicit mechanism이 없는데 contribution처럼 보이는 문제를 제거 | 6.5, 7.3, 11, 12 |
| 2026-09-17 | Surrounding objects가 존재하는 듯 서술하면서 모두 forbidden collision로 처리 | S0/S1/S2를 open environment decision으로 분리 | 단순 obstacle인지 research variable인지 불명확했기 때문 | 3.6, 7.1, 9 |
| 2026-09-17 | Marker pose, OBB와 exact geometry의 관계가 명확하지 않음 | No/Approximate/Implicit/Exact geometry taxonomy와 G0–G3 ladder 추가 | Geometry claim과 perturbation 실험을 정의하기 위해 | 3.3, 7.2, 9, 10 |
| 2026-09-17 | Contact 유지와 fixed contact configuration이 혼동될 수 있음 | Aggregate contact preference와 contact migration/re-contact를 분리 | Online adaptation 가설과 모순을 제거 | 6.6, H4, 12 |
| 2026-09-17 | Candidate contribution이 method description과 섞임 | C1–C4를 필요한 method·실험·확정 조건과 함께 조건부로 정리 | 결과 이전의 novelty 확정을 방지 | 8 |
| 2026-09-17 | Reward term과 metric이 동일한 성공 정의처럼 사용될 수 있음 | Training signal과 independent evaluation을 명시적으로 분리 | Reward hacking과 순환 논증을 방지 | 6.4, 9 |

### 14.2 Preserved history from the previous context

기존 2170-line decision log를 조용히 폐기한 것이 아니다. 앞으로의 기준으로 필요한 변경 관계를 아래에 압축 보존하며, 세부 원문은 repository history에서 추적할 수 있다.

| Period | Preserved decision/history | Current interpretation |
| --- | --- | --- |
| 2026-09-13 | Vision-based sweeping에서 시작해 Track A/B를 구분 | Track A/B는 동작 이름이 아니라 다른 연구 방향이었으며 현재는 Track B Stage 1만 구체화 |
| 2026-09-14 | Low-level execution을 먼저 학습한 뒤 상위 의사결정으로 확장 | Stage 1 범위와 이후 확장 경계로 유지 `[Fixed]` |
| 2026-09-14–15 | GD2P, TaskDexGrasp, contact selection과 downstream state quality 검토 | H2와 C1의 문헌 출발점으로 유지; novelty는 미확정 |
| 2026-09-15 | Actor/privileged information 분리, EEF delta pose+hand action, binary tactile와 F/T 검토 | 원칙은 유지하되 구체 차원·encoding은 `[Baseline]` |
| 2026-09-15 | Rotation representation 5D/6D/quaternion, history와 action memory 논의 | Quaternion/current-only/one-step은 현재 baseline; representation 우위는 미확정 |
| 2026-09-16 | Motivation, paper index, topic groups와 reward references를 별도 문서로 분리 | 문헌과 세부 근거의 참고 자료로 유지; 현재 분류 권한은 이 문서 |
| 2026-09-16 | 최신 IL/VLA도 tactile/force와 fast feedback을 다룬다는 반례 확인 | 기존 방법을 strawman으로 만들지 않는 claim boundary로 유지 |
| 2026-09-17 | 64D goal-quaternion 안을 66D selected-face/direction 안으로 대체 | 64D는 `[Superseded]`; 66D는 확정 scope가 아니라 `[Baseline]` |
| 2026-09-17 | Single-threshold 17D tactile, current wrench와 one-step previous action | 현재 baseline이며 threshold/history는 hardware evidence가 필요한 `[Open]` |
| 2026-09-17 | Rotation 중 translation, recoverable alignment/contact loss와 contact migration 허용 | 현재 baseline behavior; 효과는 H4 및 safety evaluation에서 검증 |

### 14.3 Superseded implementation snapshots

| Snapshot | Content | Status |
| --- | --- | --- |
| Early observation drafts | Point cloud/mesh, phase ID, 다수 kinematic feature와 modality별 history를 넓게 포함 | `[Superseded]` |
| Observation v0.1 | 6D rotation과 variable tactile/F/T/action history | `[Superseded]` |
| Observation v0.2 | Target/current quaternion을 포함한 64D input | `[Superseded]` |
| Observation v0.3 | Selected-face/direction 기반 66D input | `[Baseline]` |
| Reward v0.1–v0.2 | Target yaw/quaternion 중심의 phase reward | `[Superseded]` |
| Reward v0.3 | Face-direction alignment, translation progress, aggregate-contact loss와 safety 분리 | `[Baseline]` |

이후 변경은 기존 정의를 삭제하는 대신 Section 14.1 표에 `Previous Definition → Updated Definition → Reason`을 추가한다.
