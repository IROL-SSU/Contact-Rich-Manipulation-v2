# Candidate Contributions

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Previous Works](./previous_works.md)
>
> **문서 역할:** Previous Works 비교에서 도출된 검증 대상을 contribution 후보로 정리한다. 실험 결과가 나오기 전에는 확정 contribution 문장으로 사용하지 않는다.

---

## 1. Contribution을 도출하는 기준

현재 비교표에서 확인할 수 있는 사실은 다음과 같다.

- Tactile 또는 wrist F/T를 이용한 nonprehensile correction은 이미 존재한다.
- Dense geometry를 이용한 multi-mode RL과 point-cloud-conditioned contact formation·high-dimensional tactile를 사용하는 dexterous IL도 존재한다.
- Vision·force feedback과 optimization demonstration을 결합한 pushing/pivoting RL도 직접적인 baseline이다.
- 현재 조사 범위에서는 `approximate explicit geometry + coarse tactile/F/T + EEF/finger action + Approach–Rotation–Push`의 효과를 동일 조건에서 분해한 비교가 확인되지 않았다.

마지막 항목은 그 자체로 novelty를 증명하지 않는다. 각 요소가 **어떤 uncertainty를 줄이고 어떤 downstream outcome을 개선하는지** matched experiment로 보여야 한다.

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

> `[Candidate]` 이후 Rotation과 Push goal을 반영한 contact formation 및 online finger correction이 contact-only, fixed-hand 또는 wrist-only policy보다 전체 성공률과 recovery를 높인다.

| 판정 요소 | 내용 |
| --- | --- |
| Required comparison | `task-conditioned / task-agnostic × fixed hand / wrist only / wrist + fingers` |
| Main metrics | Whole-task success, unnecessary reconfiguration, contact loss, peak wrench |
| 지지 결과 | Downstream success와 recovery가 증가하고 불필요한 reconfiguration 또는 unsafe wrench가 감소 |
| 기각·축소 결과 | Finger action이 추가 이득 없이 motion·force만 증가하거나 initial configuration의 효과가 없음 |

DexMove가 이미 point-cloud-conditioned initial contact와 wrist–finger tactile policy를 제안했으므로, `task-conditioned contact formation` 또는 `wrist–finger control` 자체를 novelty로 주장하지 않는다. 핵심 비교는 **approximate geometry와 coarse sensing 조건에서의 추가 효과**다.

---

## 4. C3 — Factorized Rotation-to-Push evaluation

> `[Candidate]` Rotation alignment success와 conditional Push success를 분리하면 최종 Push failure를 설명하는 추가 정보를 제공한다.

| 판정 요소 | 내용 |
| --- | --- |
| Required comparison | Rotation terminal state를 저장한 뒤 동일 protocol로 Push continuation 평가 |
| Main metrics | Rotation success, `P(Push success \| Rotation success)`, contact loss, reconfiguration time |
| 지지 결과 | Conditional Push success가 Rotation success만으로 설명되지 않는 failure와 method 차이를 나타냄 |
| 기각·축소 결과 | 두 metric이 사실상 중복되거나 continuation protocol에 따라 결론이 불안정함 |

C3는 우선 evaluation contribution 후보다. Explicit downstream-feasibility reward, critic 또는 transition model이 추가되기 전에는 algorithmic contribution으로 표현하지 않는다.

---

## 5. Contribution boundary

현재 다음은 contribution으로 주장하지 않는다.

- Tactile, wrist F/T, RL 또는 multimodal sensing을 사용한다는 사실 자체
- Approach–Rotation–Push를 한 episode에 포함했다는 사실 자체
- Shared episode return만으로 explicit downstream-aware learning을 구현했다는 주장
- OBB가 mesh, point cloud 또는 implicit visual representation보다 일반적으로 우월하다는 주장
- RL이 planning, IL 또는 VLA보다 보편적으로 우월하다는 주장

`Approach–Rotation–Push shared policy`는 현재 method scope이자 baseline이다. Phase-specific policy 또는 scripted composition보다 일관된 이점이 확인되고 그 원인이 설명될 때만 별도 contribution 후보로 승격한다.

---

## 6. Current priority

현재 contribution 후보의 우선순위는 다음과 같다.

1. **Primary:** C1 — approximate-geometry error에 대한 contact-feedback compensation
2. **Secondary:** C2 — task-conditioned contact formation과 online wrist–finger adaptation의 추가 효과
3. **Evaluation:** C3 — Rotation과 Rotation-to-Push feasibility의 factorized evaluation

다음 단계는 [Previous Works](./previous_works.md)의 각 categorical cell을 full text의 section·figure·equation과 연결한 뒤, C1–C3의 matched baseline을 확정하는 것이다.
