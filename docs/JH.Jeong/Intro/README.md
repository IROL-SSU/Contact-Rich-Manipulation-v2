# Intro — Tactile- and Force-Guided Nonprehensile Manipulation under Approximate Geometry

> **문서 역할:** Introduction 문서의 구성과 읽는 순서를 안내한다. 연구 범위와 결정 상태는 [`../context.md`](../context.md)를 기준으로 하며, 이 문서는 세부 주장이나 구현 명세를 반복하지 않는다.
>
> **현재 상태:** 연구 문제는 `[Fixed]`, method family와 actor observation은 변경 가능한 `[Baseline]`, 가설은 `[Hypothesis]`, C1·C2는 근거 확보 전 `[Candidate]`다. 세부 method·evaluation·experiment는 `[Open]`이다.
>
> **최종 갱신:** 2026-09-24

---

## 1. 연구 범위와 역할 경계

> 선반 안의 target object에 접근할 공간을 만들기 위해, vision-derived approximate geometry가 주어진 blocker를 안정적으로 파지하거나 들어 올리지 않고 조작하며, tactile과 wrist F/T feedback으로 hand configuration과 wrist–finger motion을 online 조정하는 low-level nonprehensile manipulation을 연구한다.

현재 연구 대상은 Track B Stage 1이며, Intro에서 정리한 task는 비접촉 상태에서 시작해 `Approach / Contact Formation → Rotation / Pivoting → Push / Translation`을 다룬다. 이 세 항목은 sub-objective이지 세 개의 독립 policy나 강제된 hard sequence를 뜻하지 않는다. `Nonprehensile manipulation`은 task category이고, `contact-rich`는 접촉 위치·마찰·stick–slip–separation과 힘 전달의 불확실성을 다루는 interaction/control challenge다.

상위 모듈과 Stage 1 policy의 역할은 다음과 같이 구분한다.

| 주체 | 현재 역할 |
| --- | --- |
| **상위 모듈** | 조작할 blocker를 선택하고 low-level task goal과 전역 접근 관계를 제공한다. Blocker ordering과 target grasp·retrieval은 현재 policy의 직접 범위가 아니다. |
| **Stage 1 policy** | 주어진 goal 아래 wrist와 fingers를 제어해 contact를 형성하고, 필요한 Rotation/Pivoting을 거쳐 Push/Translation을 수행한다. |

현재 goal interface는 `selected OBB face + push direction + push distance/target position`을 사용하는 `[Baseline]`이다. Rotation 목표는 selected face의 inward normal을 push direction에 정렬하는 관계에서 유도한다. 이는 연구 정의가 아니라 변경 가능한 현재 작업 기준이며, explicit desired orientation의 필요성은 [`../context.md`](../context.md)의 open decision을 따른다.

---

## 2. Introduction 문서 구성

아래 네 문서를 순서대로 읽으면 `문제 정의 → 조건부 방법 선택 → 가까운 선행연구 비교 → 검증할 contribution 후보`의 흐름이 이어진다.

| 순서 | 문서 | 다루는 내용 | 결론 또는 남는 질문 |
| ---: | --- | --- | --- |
| 1 | [Research Motivation](./research_motivation.md) | 실제 manipulation 수요에서 nonprehensile task와 approximate-geometry contact uncertainty로 연구 문제를 좁힌다. | 여러 방법을 비교해야 하는 조건 |
| 2 | [Research Trend](./research_trend.md) | Conventional methods, IL/VLA와 RL이 task-level decision knowledge를 얻는 방식과 model·data·interaction 부담을 비교한다. | Measurable outcome, safe·affordable interaction/reset과 representative simulation이 있을 때 RL을 우선하는 조건부 `[Baseline]` 판단 |
| 3 | [Previous Works](./previous_works.md) | B85를 포함한 가까운 연구 11편의 2022–2026 timeline과 `Object Configuration·Surrounding Objects / Sensory Input·Tool·Contact Configuration·Object Manipulation / Object Geometry·Method` 비교표를 정리한다. | Ours와 가까운 조건 및 남은 C1·C2 검증 질문 |
| 4 | [Candidate Contributions](./contributions.md) | C1·C2의 핵심 주장과 현재 주장할 수 없는 범위를 정리한다. | 향후 근거가 필요한 contribution 후보 |

Introduction은 최종 contribution을 미리 확정하지 않는다. C1·C2를 어떤 method, evaluation과 experiment로 판단할지는 아직 협의 중이며, 충분한 근거가 마련된 candidate만 이후 contribution으로 승격한다.

---

## 3. 해석 원칙과 기준 문서

다음 원칙은 Intro 문서를 수정할 때도 유지한다.

- RL은 현재 model·data·interaction 조건에서 선택한 **조건부 primary method family**이며, 다른 방법에 대한 보편적 우월성이나 novelty가 아니다.
- Previous Works는 **Environment=`Object Configuration·Surrounding Objects`**, **Robot Agent=`Sensory Input·Tool·Contact Configuration·Object Manipulation`**, **System=`Object Geometry·Method`**의 여덟 column만 사용한다.
- 활성 contribution 후보는 **C1과 C2뿐**이다. 필요한 근거와 현재 주장하지 않는 범위는 [Candidate Contributions](./contributions.md)에 정리한다. 실제 비교군과 평가 방식은 아직 정하지 않았다.
- Rotation 결과와 이후 Push 가능성을 구분해서 볼 필요는 있지만 구체 평가 방식은 `[Open]`이며 별도의 C3가 아니다. Selected-face goal은 현재 observation 기준이고, shared policy 여부는 아직 협의 중이다. 둘 모두 그 자체로 contribution이 아니다.
- Sensor, RL, rotate-then-push 순서 또는 feature 조합을 사용한다는 사실만으로 contribution을 선언하지 않는다.

세부 정보의 기준 문서는 다음과 같다.

| 정보 | 기준 문서 |
| --- | --- |
| 연구 범위, 상태 분류, open decision, 변경 이력 | [Context](../context.md) |
| 연구 문제와 방법 구성을 확장해 설명한 작업 문서 | [Research Topic](../research_topic.md) |
| 합의된 actor observation과 이후 policy 설계의 `[Open]` 논의 기록 | [Policy Learning](../policy_learning.md) |
| 논문 registry, screening, 근거 수준과 독서 기록 | [Papers](../papers/README.md) |
| Introduction의 문헌 비교와 contribution 후보 | [Previous Works](./previous_works.md), [Candidate Contributions](./contributions.md) |

문서 간 표현이 충돌하면 [`../context.md`](../context.md)의 최신 상태 분류를 우선한다.
