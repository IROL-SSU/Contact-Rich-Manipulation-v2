# Candidate Contributions

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Previous Works](./previous_works.md)
>
> **문서 역할:** Intro에서 제시할 C1·C2의 핵심 주장과 현재 주장할 수 없는 범위를 정리한다.
>
> **범위 경계:** 구체적인 method, reward, evaluation metric과 experiment/ablation은 아직 합의하지 않았다. 이 문서는 실험 계획이 아니다.

---

## 1. 현재 contribution 후보

[Previous Works](./previous_works.md)와의 비교에서 남는 contribution 후보는 두 가지다.

- **C1:** Approximate-geometry error에 대한 contact-feedback compensation
- **C2:** Task-conditioned contact formation과 online wrist–finger adaptation의 추가 효과

[`context.md`](../context.md)의 H1–H4와 contribution을 구분한다. Preparatory Rotation의 필요성은 문제 가설이며 그 자체가 contribution은 아니다. Tactile·wrist F/T, wrist–finger control, RL 또는 Approach–Rotation–Push의 연결을 사용한다는 사실만으로도 contribution이 되지 않는다.

---

## 2. C1 — Contact feedback for approximate-geometry error compensation

> `[Candidate]` Approximate geometry가 실제 contact surface와 어긋날 때, tactile과 wrist F/T feedback이 그 오차에 따른 조작 성능 저하를 보완한다.

C1의 핵심은 센서를 추가했다는 사실이 아니라 다음 연결이다.

```text
Approximate geometry의 오차
  → nominal contact와 실제 contact의 차이
  → tactile·wrist F/T를 이용한 closed-loop correction
  → Rotation과 Push 실행 가능성의 유지
```

현재 actor observation은 estimated OBB, binary tactile와 wrist F/T를 포함한다. 그러나 이 구성이 실제로 geometry error를 보완한다는 근거는 아직 없으며, tactile과 F/T가 각각 어떤 역할을 하는지도 확정하지 않았다. OBB가 mesh, point cloud 또는 implicit visual representation보다 일반적으로 우월하다는 주장도 하지 않는다.

C1을 최종 contribution으로 쓰려면 다음 두 질문에 답할 근거가 필요하다.

- 성능 변화가 단순한 sensor 추가 효과가 아니라 approximate-geometry error의 보완과 연결되는가?
- 그 효과가 접촉이나 Rotation의 즉시 결과에만 머물지 않고 이후 Push까지 이어지는가?

이 근거를 어떤 조건과 metric으로 확인할지는 `[Open]`이다.

---

## 3. C2 — Task-conditioned contact formation and online wrist–finger adaptation

> `[Candidate]` Approach에서 이후 Rotation·Push를 고려한 contact state를 형성하고, 실행 중 tactile·wrist F/T에 따라 wrist와 finger configuration을 조정하는 것이 후속 조작에 기여한다.

C2에는 구분해야 할 두 가지 아이디어가 있다.

| 아이디어 | 남는 질문 |
| --- | --- |
| Task-conditioned contact formation | 단순 contact onset이나 현재 단계의 성공이 아니라 이후 Rotation·Push에 유효한 state를 형성하는가? |
| Online wrist–finger adaptation | Pre-contact configuration만 정하는 것에 비해 contact 이후의 조정이 어떤 추가 역할을 하는가? |

GD2P는 task-conditioned pre-contact wrist·finger configuration을, DexMove는 point-cloud-conditioned initial contact와 tactile wrist–finger policy를 이미 제안했다. 따라서 `task-conditioned contact formation`이나 `wrist–finger control` 자체를 novelty로 주장하지 않는다.

C2를 최종 contribution으로 쓰려면 initial configuration의 역할과 contact 이후 adaptation의 역할을 구분하고, 그 효과가 이후 Rotation과 Push에 연결된다는 근거가 필요하다. 구체적인 policy organization, action/controller, learning objective, comparison과 evaluation 방식은 모두 `[Open]`이다.

---

## 4. 주장 범위

다음은 contribution으로 단독 주장하지 않는다.

| 범주 | 현재 주장하지 않는 내용 |
| --- | --- |
| **Robot Agent 구성** | Tactile/F/T, multimodal sensing, wrist–finger action, online contact configuration 또는 특정 observation/action 차원을 사용한다는 사실 |
| **System 구성** | RL, task-conditioned formation, shared policy 또는 특정 network architecture를 사용한다는 사실 |
| **동작 구성** | Approach–Rotation–Push 또는 rotate-then-push라는 순서 자체 |
| **일반적 우월성** | OBB가 다른 geometry representation보다 우월하거나 RL이 planning·control·IL·VLA보다 보편적으로 우월하다는 주장 |
| **평가 관점** | Rotation 결과와 이후 Push 가능성을 구분해서 보아야 한다는 관점 자체 |

현재 단계에서는 C1·C2를 `[Candidate]`로만 유지한다. Reward, evaluation과 experiment가 합의되고 충분한 근거가 마련된 뒤에만 최종 contribution 문장으로 확정한다.
