# Intro — Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry

> **문서 역할:** Introduction 전체의 논리와 문서 간 연결을 보여주는 진입점이다. 세부 구현은 [`../policy_learning.md`](../policy_learning.md), 문헌 registry는 [`../papers/`](../papers/README.md), 연구 범위와 결정 상태의 기준은 [`../context.md`](../context.md)를 따른다.
>
> **현재 상태:** 연구 문제는 고정했지만 method는 변경 가능한 `Baseline`, 가설은 `Hypothesis`, C1·C2는 실험 전 `Candidate`다.
>
> **최종 갱신:** 2026-09-19

---

## 1. 연구를 한 문장으로

> 선반 안의 target object에 접근할 공간을 만들기 위해, approximate geometry만 주어진 blocker를 안정적으로 파지하거나 들어 올리지 않고 조작하며, tactile과 wrist F/T feedback으로 hand configuration과 wrist–finger motion을 online 조정하는 low-level nonprehensile manipulation을 연구한다.

현재 task-level 흐름은 다음과 같다.

```text
Approach / Contact Formation
        ↓
Rotation / Pivoting
        ↓
Push / Translation
```

- **Approach**는 이후 조작에 사용할 hand–object contact state를 형성한다.
- **Rotation**은 현재 selected-face baseline에서 object face의 inward pushing normal을 목표 push direction에 맞추는 preparatory manipulation이다.
- **Push**는 정렬과 안정성을 유지하며 blocker를 목표 위치로 병진시킨다.

세 단계는 서로 다른 sub-objective이지 세 개의 독립 policy나 강제된 hard sequence를 뜻하지 않는다. 초기 상태가 이미 Push에 적합하면 Rotation은 작거나 생략될 수 있으며, Rotation 중 병진과 접촉 migration, release/re-contact 및 wrist–finger reconfiguration도 허용한다.

Shelf blocker manipulation은 broader motivation 자체가 아니라 연구 질문을 검증하는 **현재 application instance**다. Blocker 선택과 전체 작업 순서는 상위 모듈의 역할이며, target grasp·retrieval은 현재 Stage 1 policy의 직접 범위가 아니다.

이 문서에서 `nonprehensile manipulation`은 연구의 **task category**이고, `contact-rich`는 별도 task category가 아니라 접촉 위치·마찰·stick–slip–separation과 힘 전달의 불확실성을 다루는 **interaction/control challenge**다.

---

## 2. Introduction의 논증 구조

Intro는 아래 네 문서를 하나의 논증으로 읽는다. 각 문서는 앞 단계의 결론을 이어받아 다음 단계가 답해야 할 질문을 남긴다.

| 순서 | 문서 | 이 문서의 역할 | 다음 단계로 넘기는 질문 |
| ---: | --- | --- | --- |
| 1 | [Research Motivation](./research_motivation.md) | 실제 manipulation 수요에서 nonprehensile task와 approximate-geometry contact uncertainty로 문제를 좁힌다. | 이 조건에는 어떤 method family가 적합한가? |
| 2 | [Research Trend](./research_trend.md) | Heuristic/control/planning, RL, IL, VLA와 Hybrid의 발전·trade-off를 비교하고 현재 조건에서 RL을 primary baseline으로 선택한다. | 가까운 system이 이미 무엇을 해결했고 무엇이 남았는가? |
| 3 | [Previous Works](./previous_works.md) | Environment–Agent–System 기준으로 closest work를 비교해 두 candidate question과 transition-evaluation requirement를 도출한다. | 어떤 비교와 결과가 candidate를 지지하거나 기각하는가? |
| 4 | [Candidate Contributions](./contributions.md) | C1·C2를 matched comparison, metric, 지지 조건과 기각 조건이 있는 testable claim으로 바꾼다. | Method와 Experiments가 실제로 candidate를 지지하는가? |

전체 흐름은 다음과 같다.

```text
Application need
  → Nonprehensile manipulation
  → Contact uncertainty under approximate geometry
  → Conditional method selection
  → Closest-system comparison
  → Testable hypotheses and candidate contributions
  → Matched experiments and ablations
  → Evidence-supported contributions
```

Introduction 단계에서 확정하는 것은 마지막 contribution 문구가 아니라 **연구 문제, 조건부 method 선택, 반증 가능한 claim의 구조**다.

---

## 3. 현재 연구 상태

| 구분 | 현재 정리 | 의미 |
| --- | --- | --- |
| `[Fixed]` 연구 범위 | Shelf blocker를 대상으로 한 Approach–Rotation–Push low-level nonprehensile manipulation | 무엇을 연구하는지는 고정한다. |
| `[Fixed]` 정보 역할 | Vision/coarse geometry는 전역 task·nominal access를, tactile/wrist F/T는 실제 contact state와 geometry mismatch에 대한 local feedback을 제공 | Geometry와 contact sensing의 역할을 구분한다. |
| `[Baseline]` 학습 framework | Simulation interaction, 제한된 real demonstration coverage, contact-dependent recovery와 full-episode outcome을 고려해 RL을 우선 구현 | RL의 보편적 우월성이나 novelty를 뜻하지 않는다. |
| `[Baseline]` policy organization | Shared goal-conditioned policy와 공통 action space로 세 sub-objective를 수행 | Shared policy, phase-ID 제거, 현재 observation/action 수치는 비교 가능한 최초 구현안이다. |
| `[Baseline]` transition evaluation | Rotation success와 Rotation-to-Push success를 분리하고 saved-state continuation으로 후속 가능성을 측정 | 평가 protocol이며 독립 contribution이 아니다. |
| `[Candidate]` contributions | C1 — geometry-error compensation, C2 — downstream-suitable contact formation과 online wrist–finger adaptation | Matched baseline과 ablation이 지지할 때만 확정한다. |
| `[Open]` 주요 결정 | Surrounding-object 조건, geometry ladder, explicit downstream-learning mechanism | 실험 범위와 baseline 비교를 통해 결정한다. |

현재 구현 snapshot인 66D observation, 12D action, selected-face goal, shared MLP와 phase-gated reward는 연구 정의가 아니다. 이들은 가설을 검증하기 위한 변경 가능한 baseline이며 상세 내용은 [Policy Learning](../policy_learning.md)에서 관리한다.

---

## 4. 핵심 연구 질문과 가설

| 질문 | 검증 초점 | 현재 위치 |
| --- | --- | --- |
| **RQ1. Preparatory Rotation utility** | Direct push가 어려운 initial condition에서 rotate-then-push가 성공 가능한 상태 영역을 넓히는가? | Task structure의 필요성을 검증하는 H1 |
| **RQ2. Sequential/task-conditioned contact formation** | Contact onset이나 Rotation alignment만 좋은 상태가 아니라 subsequent Push까지 유효한 contact state를 형성하는가? | H2, C2의 일부 |
| **RQ3. Complementary contact sensing** | Approximate geometry에서 tactile과 wrist F/T가 각각 또는 함께 robustness를 높이는가? | H3, C1의 핵심 |
| **RQ4. Online wrist–finger adaptation** | Contact 이후 wrist와 fingers를 함께 조절하는 것이 fixed-hand 또는 wrist-only control보다 유효한가? | H4, C2의 일부 |

`전체 episode 결과를 고려한다`는 말은 현재 explicit downstream-feasibility reward나 critic이 구현되었다는 뜻이 아니다. 현재 baseline은 shared episodic return을 사용하고, 다음 단계 유효성은 continuation rollout으로 별도 평가한다. Learned feasibility value나 transition objective는 필요성이 확인될 때 추가할 `Open` 항목이다.

---

## 5. 활성 contribution 후보

### C1 — Contact feedback for approximate-geometry error compensation

> `[Candidate]` Approximate OBB의 위치·크기·방향 오차가 있는 조건에서 binary tactile과 wrist F/T feedback이 vision/proprioception-only policy보다 Rotation-to-Push 및 final-task 성능 저하를 줄인다.

필수 검증은 geometry-error sweep과 `no contact sensor / tactile only / F/T only / tactile+F/T` factorial ablation이다. 성공률뿐 아니라 degradation slope, recovery, force·collision과 safety 지표를 함께 비교한다.

### C2 — Task-conditioned contact formation and online wrist–finger adaptation

> `[Candidate]` 후속 Rotation과 Push goal을 고려한 contact formation 및 online wrist–finger adaptation이 contact-only, phase-local, fixed-hand 또는 wrist-only policy보다 downstream success와 recovery를 높인다.

Approach contact rate나 Rotation alignment만으로 C2를 지지할 수 없다. 이들을 통제한 뒤에도 Rotation-to-Push success, final success와 held-out uncertainty 성능이 개선되어야 한다.

현재 활성 contribution 후보는 C1과 C2뿐이다. Factorized Rotation-to-Push analysis는 두 후보의 효과가 다음 조작까지 이어지는지 판정하는 공통 evaluation protocol이며, 별도의 C3가 아니다. Unified Approach–Rotation–Push policy 역시 현재 method scope와 baseline이지 그 자체로 contribution이 아니다.

---

## 6. 평가에서 반드시 분리할 결과

| 평가 수준 | 핵심 질문 | 대표 지표 |
| --- | --- | --- |
| Task execution | 목표를 정확하고 안전하게 달성했는가? | Final success, axial/lateral error, alignment error, completion time |
| Phase outcome | 각 sub-objective를 달성했는가? | Contact formation, Rotation success, Push success |
| Transition quality | 앞 단계의 종료 상태가 다음 단계에도 유효한가? | Approach→Rotation, Rotation→Push success, continuation probability |
| Robustness | Geometry·pose·friction·mass perturbation에서도 유지되는가? | Success degradation, recovery, seen–held-out gap |
| Safety | Task progress와 교환할 수 없는 위험이 발생했는가? | Forbidden collision, boundary exit, topple, peak force/impulse, joint-limit violation |

특히 다음 세 결과를 혼동하지 않는다.

- **Rotation success:** Selected-face normal과 push direction이 tolerance 안에서 정렬되고 object가 안정적이다.
- **Rotation-to-Push success:** Rotation-success state에서 정해진 continuation policy/controller가 Push goal을 안전하게 달성한다.
- **Final task success:** 지정된 translation goal과 alignment·stability 조건을 만족하고 hard safety failure가 없다.

Rotation success는 subsequent Push feasibility의 충분조건이 아니다. Contact maintenance도 학습상 선호할 수 있는 soft behavior일 뿐, 모든 성공의 필요조건으로 고정하지 않는다.

---

## 7. Claim boundary

다음은 단독 contribution으로 주장하지 않는다.

- RL, tactile, wrist F/T 또는 wrist–finger control을 사용한다는 사실
- Rotate-then-push라는 동작 순서 자체
- Shared policy, phase-ID 제거, MLP 구조나 observation/action 차원
- Feature 조합이 기존 표에 정확히 없다는 사실
- Rotation과 Rotation-to-Push를 분리해 평가한다는 사실
- OBB가 mesh, point cloud 또는 implicit visual representation보다 일반적으로 우월하다는 주장
- Planning, IL 또는 VLA보다 RL이 보편적으로 우월하다는 주장

문헌 timeline은 method 선택의 맥락을 제공하고, closest-work comparison은 검증할 gap을 정의한다. Novelty와 contribution은 최종적으로 동일 sensing·geometry·action budget을 맞춘 비교 실험과 ablation 결과로만 확정한다.

---

## 8. 발표·논문 Introduction 권장 순서

1. **Application:** Home service, logistics와 retail에서의 접근·재배치 수요
2. **Task category:** Grasp만으로 처리하기 어려운 상황과 nonprehensile manipulation
3. **Interaction challenge:** Approximate geometry 아래 contact uncertainty
4. **Current instance:** Shelf blocker를 통한 접근 공간 확보
5. **Research Trend:** Heuristic/control/planning, RL, IL, VLA와 Hybrid의 발전
6. **Conditional method choice:** 현재 model·data·interaction 조건에서 RL을 우선하는 이유와 반증 조건
7. **Closest Previous Works:** Environment–Agent–System 비교와 기존 연구가 이미 해결한 범위
8. **Research questions:** Rotation utility, transition quality, sensing complementarity와 online adaptation
9. **Candidate Contributions:** C1·C2와 각각의 matched experiment 및 기각 조건
10. **Method/Experiments transition:** Factorized transition, robustness와 safety evaluation

최종 slide 문구와 contribution 표현은 논문별 full-text evidence, baseline protocol과 실험 결과가 확정된 뒤 작성한다.
