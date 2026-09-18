# Candidate Contributions — From Research Gaps to Testable Claims

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Previous Works](./previous_works.md)
>
> **문서 역할:** Previous Works 비교에서 도출된 검증 대상을 contribution 후보로 정리한다. 실험 결과가 나오기 전에는 확정 contribution 문장으로 사용하지 않는다.

---

## 1. Previous Works에서 넘어온 검증 질문

앞선 [Previous Works](./previous_works.md)는 문헌으로 확인된 사실에서 두 개의 candidate claim과 이를 검증하기 위한 transition-evaluation requirement를 도출했다. 여기서는 같은 문헌 비교를 반복하지 않고 C1과 C2를 실험 가능한 claim으로 바꾼다.

Candidate로 유지하려면 각 항목에 다음 네 요소가 있어야 한다.

| 필수 요소 | 역할 |
| --- | --- |
| Required comparison | 어떤 요인을 바꿔 인과 효과를 분리할지 정의 |
| Main metrics | 어느 outcome으로 효과를 평가할지 정의 |
| 지지 결과 | 어떤 관찰이 candidate claim을 지지하는지 정의 |
| 기각·축소 결과 | 어떤 관찰에서 claim을 버리거나 범위를 줄일지 정의 |

C1과 C2는 이 조건을 만족하도록 작성하지만 아직 확정 contribution은 아니다. Rotation과 Rotation-to-Push를 분리하는 분석은 두 candidate를 검증하는 evaluation protocol이며 contribution으로 세지 않는다.

---

## 2. C1 — Contact feedback for approximate-geometry error compensation

> `[Candidate]` Approximate OBB의 위치·크기·방향 오차가 있는 조건에서 binary tactile과 wrist F/T가 vision/proprioception-only policy보다 Rotation-to-Push 성능 저하를 줄인다.

| 판정 요소 | 내용 |
| --- | --- |
| Required comparison | `oracle / perturbed / no geometry × no contact feedback / tactile / F/T / both` |
| Main metrics | Rotation-to-Push success, final translation error, safety violation, geometry-error별 degradation slope |
| 지지 결과 | Geometry error가 증가할 때 contact feedback이 baseline의 성능 저하를 유의하게 완화 |
| 기각·축소 결과 | Contact feedback의 이득이 없거나 geometry perturbation과 무관함 |

---

## 3. C2 — Task-conditioned contact formation and online wrist–finger adaptation

> `[Candidate]` 이후 Rotation과 Push goal을 반영한 contact formation 및 online wrist–finger adaptation이 contact-only, fixed-hand 또는 wrist-only policy보다 전체 성공률과 recovery를 높인다.

| 판정 요소 | 내용 |
| --- | --- |
| Required comparison | `task-conditioned / task-agnostic × fixed hand / wrist only / wrist + fingers` |
| Main metrics | Whole-task success, unnecessary reconfiguration, contact loss, peak wrench |
| 지지 결과 | Downstream success와 recovery가 증가하고 불필요한 reconfiguration 또는 unsafe wrench가 감소 |
| 기각·축소 결과 | Finger action이 추가 이득 없이 motion·force만 증가하거나 initial configuration의 효과가 없음 |

DexMove가 이미 point-cloud-conditioned initial contact와 wrist–finger tactile policy를 제안했으므로, `task-conditioned contact formation` 또는 `wrist–finger control` 자체를 novelty로 주장하지 않는다. 핵심 비교는 **approximate geometry와 coarse sensing 조건에서의 추가 효과**다.

---

## 4. Contribution boundary

현재 다음은 contribution으로 주장하지 않는다.

- Tactile, wrist F/T, RL 또는 multimodal sensing을 사용한다는 사실 자체
- Approach–Rotation–Push를 한 episode에 포함했다는 사실 자체
- Shared episode return만으로 explicit downstream-aware learning을 구현했다는 주장
- OBB가 mesh, point cloud 또는 implicit visual representation보다 일반적으로 우월하다는 주장
- RL이 planning, IL 또는 VLA보다 보편적으로 우월하다는 주장
- Rotation success와 Rotation-to-Push success를 분리해 측정한다는 사실 자체

Factorized Rotation-to-Push analysis의 protocol과 metric은 [Policy Learning](../policy_learning.md)의 evaluation definition과 [`context.md`](../context.md)의 Evaluation and Ablation Plan에서 관리한다. 이는 C1·C2의 효과가 subsequent Push까지 이어지는지 판정하는 도구이지 별도 contribution이 아니다.

`Approach–Rotation–Push shared policy`는 현재 method scope이자 baseline이다. Phase-specific policy 또는 scripted composition보다 일관된 이점이 확인되고 그 원인이 설명될 때만 별도 contribution 후보로 승격한다.

---

## 5. Current priority

현재 contribution 후보의 우선순위는 다음과 같다.

1. **Primary:** C1 — approximate-geometry error에 대한 contact-feedback compensation
2. **Secondary:** C2 — task-conditioned contact formation과 online wrist–finger adaptation의 추가 효과

C1과 C2를 해석할 때 Rotation success와 Rotation-to-Push feasibility를 분리하지만, 이는 우선순위를 갖는 contribution 항목이 아니라 공통 evaluation protocol이다.

문헌 검증과 실험 준비의 순서는 다음과 같다.

1. [Previous Works](./previous_works.md)의 각 categorical cell을 full text의 section·figure·equation과 연결한다.
2. C1과 C2별로 sensing·geometry·action budget이 맞는 baseline을 확정한다.
3. [Policy Learning](../policy_learning.md)의 baseline method와 reward/evaluation definition을 필요한 실험에 맞춰 수정한다.
4. 실험 결과가 지지한 candidate만 최종 contribution 문장으로 승격한다.

---

## 6. Intro 결론과 Method/Experiments로의 전환

네 문서의 논리는 다음과 같이 닫힌다.

```text
Research Motivation
  → approximate geometry 아래의 contact-feedback NPM 문제 정의
Research Trend
  → 현재 조건에서 RL을 primary baseline으로 선택
Previous Works
  → 기존 성과를 인정하고 두 candidate와 transition-evaluation requirement 도출
Candidate Contributions
  → C1·C2를 matched experiment와 지지·기각 조건으로 정의
```

따라서 Introduction 단계에서 확정되는 것은 연구 문제, 조건부 method 선택과 testable claim의 구조다. Factorized Rotation-to-Push analysis는 이 claim을 판정하는 평가 도구다. 최종 contribution은 이후 Method와 Experiments가 C1·C2를 지지할 때만 확정한다.
