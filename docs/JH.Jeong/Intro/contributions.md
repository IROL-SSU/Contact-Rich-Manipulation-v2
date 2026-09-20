# Candidate Contributions

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Previous Works](./previous_works.md)
>
> **문서 역할:** C1·C2에서 검증할 주장, 비교 조건, 지표와 지지·기각 조건을 정리한다. 실험 결과가 나오기 전에는 확정 contribution 문장으로 사용하지 않는다.

---

## 1. 현재 contribution 후보

[Previous Works](./previous_works.md)에서 Environment, Robot Agent와 System의 공통 분류값으로 가까운 연구를 비교한 결과, 두 가지를 검증해야 한다.

- **C1:** Approximate-geometry error에 대한 contact-feedback compensation
- **C2:** Task-conditioned contact formation과 online wrist–finger adaptation의 추가 효과

[`context.md`](../context.md)의 H1–H4와의 관계도 분리한다. H1은 preparatory Rotation이 direct push보다 유효한 조건을 확인하는 가설이며 그 자체가 contribution은 아니다. H3의 sensing complementarity가 C1에, H2의 후속 Rotation·Push에 유효한 contact formation과 H4의 online adaptation이 C2에 대응한다.

Rotation success와 Rotation-to-Push success를 나누어 평가해 C1·C2의 효과가 후속 Push까지 이어지는지 확인한다. 이 평가 자체는 별도의 C3가 아니다.

---

## 2. C1 — Contact feedback for approximate-geometry error compensation

> `[Candidate]` **G1 perturbed approximate OBB** 조건에서 binary tactile과 wrist F/T의 closed-loop feedback은 contact feedback이 없는 policy보다 geometry error 증가에 따른 Rotation-to-Push와 final-task 성능 저하를 줄인다.

### Geometry conditions

| Code | Policy에 제공하는 geometry | C1에서의 역할 |
| --- | --- | --- |
| **G0** | Object pose는 사용하지만 explicit shape representation은 사용하지 않음 | Shape 정보를 제공하지 않는 비교 조건 |
| **G1** | Episode-consistent error가 포함된 approximate OBB | 핵심 deployment condition이자 C1 claim의 중심 |
| **G2** | Ground-truth pose·extent·orientation의 oracle OBB | OBB 추정 오차가 없는 상한 비교 조건 |

G0/G1/G2는 policy가 받는 geometry information을 구분한다. Exact mesh/CAD를 사용하는 G3는 현재 핵심 C1 비교에 포함하지 않으며, 필요 여부는 [`context.md`](../context.md)의 open decision을 따른다. Simulator의 exact contact, force, collision이나 object dynamics truth는 policy input으로 사용하지 않는다.

| 판정 요소 | 내용 |
| --- | --- |
| **Required comparison** | `G0 / G1 / G2 × no contact feedback / tactile only / F/T only / tactile + F/T`; G1에서는 OBB 위치·크기·방향 error sweep 수행 |
| **Main metrics** | Rotation-to-Push success, final-task success, geometry-error별 degradation slope, final translation·alignment error, recovery와 safety violation |
| **지지 결과** | G1 error가 증가할 때 tactile 및/또는 F/T 조건이 no-contact-feedback baseline보다 degradation을 유의하게 완화하고, 그 효과가 Rotation 성공에만 머물지 않고 Rotation-to-Push와 final success에 나타남 |
| **기각·축소 결과** | Contact feedback의 이득이 없거나 geometry error와 무관함; 이득이 Rotation alignment에만 있고 subsequent Push로 이어지지 않음; `both`가 single modality보다 낫지 않으면 sensor complementarity 주장을 modality-specific 효과로 축소 |

C1은 OBB가 다른 representation보다 우월하다는 주장이 아니다. G2는 perception error가 제거된 경우를, G0는 explicit shape information이 없는 경우를 제공하며, G1의 controlled corruption이 contact feedback의 compensation 효과를 판정하는 중심 조건이다.

---

## 3. C2 — Task-conditioned contact formation and online wrist–finger adaptation

> `[Candidate]` **후속 Rotation과 Push goal을 반영한 contact formation**과 **online wrist–finger adaptation**은 같은 sensing·geometry·training·safety 조건에서 contact-only 또는 phase-local objective와 fixed-hand 또는 wrist-only control보다 Rotation-to-Push success와 recovery를 높인다.

C2는 두 요인을 분리해 검증한다.

| 요인 | 비교 조건 | 분리하려는 효과 |
| --- | --- | --- |
| **Contact-formation objective** | `Task-conditioned full-episode / Contact-only / Phase-local` | 단순 contact onset·maintenance나 현재 phase outcome이 아니라 후속 Rotation·Push에 유효한 contact state를 형성하는가? |
| **Online action authority** | `Fixed hand / Wrist only / Wrist + fingers` | Contact 이후 finger configuration의 online update가 wrist motion만으로 얻을 수 없는 추가 이득을 주는가? |

| 판정 요소 | 내용 |
| --- | --- |
| **Required comparison** | 위 두 요인의 matched factorial comparison. Initial state, task goal, geometry·sensor input, controller·training budget과 safety rule을 동일하게 유지 |
| **Main metrics** | Rotation-to-Push success, final-task success, continuation probability, recovery success, contact loss, unnecessary reconfiguration, peak wrench와 safety violation. Contact formation rate와 Rotation success는 진단 지표로 별도 보고 |
| **지지 결과** | Contact formation rate나 Rotation alignment를 통제한 뒤에도 task-conditioned condition의 Rotation-to-Push·final success가 향상되고, wrist + fingers가 fixed-hand·wrist-only보다 recovery를 높이거나 unsafe wrench·contact loss를 줄임 |
| **기각·축소 결과** | 향상이 contact onset 또는 Rotation success에만 있고 Rotation-to-Push로 이어지지 않음; finger action이 추가 task 이득 없이 motion·force·reconfiguration만 증가함; task conditioning이 contact-only·phase-local보다 유의한 이득을 보이지 않음 |

GD2P는 task-conditioned pre-contact wrist·finger configuration을, DexMove는 point-cloud-conditioned initial contact와 tactile wrist–finger policy를 이미 제안했다. 따라서 `task-conditioned contact formation`이나 `wrist–finger control` 자체를 novelty로 주장하지 않는다. C2의 검증 대상은 **approximate geometry와 coarse sensing 아래에서 downstream goal conditioning과 contact 중 online adaptation이 제공하는 추가 효과**다.

---

## 4. 공통 평가와 주장 범위

다음 세 결과를 구분한다.

| 결과 | 판정 의미 |
| --- | --- |
| **Rotation success** | Selected-face normal과 push direction이 tolerance 안에서 정렬되고 object가 안정적임 |
| **Rotation-to-Push success** | Rotation-success state에서 정해진 continuation policy/controller가 Push goal을 안전하게 달성함 |
| **Final-task success** | 지정된 translation goal, alignment·stability와 hard-safety 조건을 모두 만족함 |

Rotation success는 subsequent Push feasibility의 충분조건이 아니다. 따라서 C1·C2는 Rotation 지표만으로 지지할 수 없고, 최소한 Rotation-to-Push와 final-task 결과를 함께 보여야 한다. 두 결과를 나누어 평가하는 것은 주장을 검증하기 위한 방법이지 contribution 자체가 아니다.

단독 contribution으로 주장하지 않는 항목은 세 범주로 묶는다.

| 범주 | 현재 주장하지 않는 내용 |
| --- | --- |
| **Robot Agent 구성** | Tactile/F/T, multimodal sensing, wrist–finger action, `End-effector Reconfiguration=Online`, phase-ID 제거 또는 특정 observation/action 차원을 사용한다는 사실 |
| **System 구성** | RL, task-conditioned formation, shared policy 또는 특정 network architecture를 사용한다는 사실 |
| **일반적 우월성** | OBB가 mesh·point cloud·implicit visual보다 우월하다거나 RL이 planning·control·IL·VLA보다 보편적으로 우월하다는 주장 |
| **조합·평가** | Approach–Rotation–Push 또는 기존 표에 없는 feature 조합 자체, Rotation과 Rotation-to-Push를 나누어 평가한다는 사실 자체, shared episodic return만으로 후속 단계를 고려하는 별도 학습 방법을 구현했다는 주장 |

`Approach–Rotation–Push shared policy`는 현재 구현 범위이자 변경 가능한 baseline이다. Phase-specific policy나 scripted composition보다 일관된 이점이 확인되고 원인이 설명될 때만 별도 candidate로 재검토한다.

---

## 5. Method와 Experiments에서 검증할 내용

C1·C2는 동일 sensing·geometry·action·safety budget을 맞춘 비교와 ablation이 위 지지 조건을 충족할 때만 최종 contribution으로 승격한다. 구현·reward·evaluation 정의는 [Policy Learning](../policy_learning.md), 결정 상태와 실험 범위는 [`context.md`](../context.md)를 따른다.
