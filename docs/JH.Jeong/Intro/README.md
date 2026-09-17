# Intro — Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry

> **Directory role:** 연구 발표와 논문의 Introduction을 구성하는 논리를 관리한다. 구현 세부사항은 [`../policy_learning.md`](../policy_learning.md), 전체 문헌 목록은 [`../papers/`](../papers/README.md), 결정 상태와 변경 근거는 [`../context.md`](../context.md)에서 관리한다.
>
> **상태:** Contribution은 closest-work comparison과 실험으로 지지되기 전까지 모두 `Candidate`다.
>
> **최종 갱신:** 2026-09-17

---

## 1. 문서 구성

Intro는 위에서 아래로 다음 순서로 읽는다.

| 순서 | 문서 | 답하는 질문 |
| ---: | --- | --- |
| 1 | [Research Motivation](./research_motivation.md) | 왜 nonprehensile manipulation과 contact uncertainty를 연구해야 하는가? |
| 2 | [Research Trend](./research_trend.md) | 2021년 이후 heuristic/planning·RL·IL·VLA는 어떻게 발전했고, 왜 현재 문제에 RL을 우선하는가? |
| 3 | [Previous Works](./previous_works.md) | 우리와 가까운 system은 무엇을 해결했으며 어떤 교차점이 남는가? |
| 4 | [Candidate Contributions](./contributions.md) | 남은 교차점을 어떤 실험으로 검증하고 언제 contribution으로 확정할 수 있는가? |

---

## 2. Intro storyline

```text
Home service · logistics · retail의 robot deployment 확대
        ↓
접근·재배치 과정에서 grasp만으로 처리하기 어려운 물체가 존재
        ↓
Pushing · pulling · sliding · pivoting과 같은 nonprehensile skill 필요
        ↓
Contact location · friction · stick–slip–separation의 불확실성
        ↓
Model-based planning/control과 task-specific heuristic만으로 확장하기 어려운 조건
        ↓
RL · IL · VLA를 포함한 learning-based manipulation의 발전
        ↓
현재 문제 조건에는 simulation interaction과 task return을 활용하는 RL이 적합
        ↓
Approximate geometry와 coarse contact feedback의 교차점에서 남은 질문 식별
        ↓
Candidate contribution과 falsification experiment 도출
```

Shelf blocker는 broader motivation의 출발점이 아니라 이를 구체화한 **application instance**다.

---

## 3. Unified research definition

> **Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry**

연구의 핵심은 approximate geometry 조건에서 tactile과 wrist F/T feedback을 이용해 hand configuration과 wrist–finger motion을 조정하며 다음 동작을 수행하는 것이다.

```text
Approach / Contact Formation
        ↓
Rotation / Pivoting
        ↓
Push / Translation
```

- `Nonprehensile manipulation`: 연구의 task category
- `Contact-rich`: 접촉 위치·마찰·stick–slip–separation 불확실성을 다루는 interaction/control challenge
- Vision/coarse geometry: 전역 task, pose와 nominal access information
- Tactile/wrist F/T: 실제 contact state와 geometry mismatch에 대한 local feedback
- Shelf blocker manipulation: 위 연구 질문을 검증할 현재 application setting

---

## 4. Intro에서 유지할 claim boundary

- RL은 planning, IL 또는 VLA보다 보편적으로 우월하다고 주장하지 않는다.
- Tactile/F/T 또는 wrist–finger control을 사용한다는 사실 자체를 contribution으로 주장하지 않는다.
- Method-family timeline은 RL 선택을 설명하지만 novelty의 근거로 사용하지 않는다.
- Previous Works의 feature 조합 차이는 research question을 도출하지만 novelty를 자동으로 증명하지 않는다.
- `Sequential` 또는 `downstream-aware`는 shared return, transition evaluation과 explicit learning mechanism을 구분한다.
- Contribution은 matched baseline과 ablation 결과가 지지한 항목만 확정한다.

---

## 5. Presentation sequence

1. **Applications:** Home service, logistics와 retail의 접근·재배치 문제
2. **Task category:** Grasp만으로 어려운 상황과 nonprehensile manipulation의 역할
3. **Interaction challenge:** Approximate geometry 아래 contact uncertainty
4. **Research Trend:** 2021–2026 representative timeline
5. **Method analysis:** Heuristic/planning, RL, IL와 VLA의 강점·한계
6. **Why RL here:** 현재 low-level problem과 RL의 조건부 적합성
7. **Previous Works:** Environment–Agent–System codebook과 closest-work comparison
8. **Remaining intersection:** Approximate geometry × contact feedback × wrist–finger execution
9. **Candidate Contributions:** C1–C3와 각각의 falsification experiment

최종 slide 문구는 논문별 full-text evidence와 실험 설계가 확정된 뒤 작성한다.
