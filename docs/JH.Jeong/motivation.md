# Track B Research Motivation

> **핵심 질문:** Shelf blocker의 선택 면을 목표 방향에 맞춘 뒤 밀어야 할 때, Approach의 hand configuration과 Rotation의 terminal contact를 최종 Push 성공에 유리하도록 학습할 수 있는가?
>
> **문서 역할:** 실제 문제에서 research gap, Track B의 대응과 검증 가설까지 하나의 논리로 연결한다.
>
> **최종 갱신:** 2026-09-17

---

## 1. 실제 문제에서 출발한다

물류 선반과 생활환경에서는 target object가 다른 물체에 가려지거나 접근 경로가 blocker에 막힐 수 있다. 이때 로봇은 target을 바로 grasp하기 전에 주변 물체를 밀거나 돌려 시야와 접근 공간을 확보해야 한다. 따라서 필요한 능력은 환경과의 접촉을 피하는 collision-free motion만이 아니라, **의도적인 접촉으로 물체를 재배치하고 접촉 결과에 맞춰 행동을 수정하는 것**이다.

그러나 contact-rich manipulation에서는 stick, slip, pivot과 충돌에 따라 dynamics와 constraint가 바뀐다. 국소 형상, 마찰, 질량 분포와 작은 pose 오차도 서로 다른 물체 운동과 접촉력으로 이어진다. 정확한 모델과 contact mode를 알면 model-based planning과 control이 강력하지만, unseen object와 occlusion이 있는 shelf마다 이를 정확히 구성하는 데에는 큰 부담이 따른다.

이 어려움 때문에 최근 RL, IL과 VLA가 contact-rich manipulation에 적극적으로 도입되고 있다. 문제는 학습 방법을 사용한다는 사실 자체가 아니라, **무엇을 성공으로 정의하고 어떤 접촉 상태까지 학습해야 하는가**이다.

---

## 2. Direct push만으로는 충분하지 않다

Blocker를 원하는 방향으로 바로 밀 수 있다면 문제는 단순해진다. 하지만 다음 조건에서는 direct push가 불가능하거나 불안정하다.

| 원인 | 나타나는 failure | 필요한 대응 |
| --- | --- | --- |
| 제한된 접근 방향 | 원하는 pushing direction에 손이 접근하지 못하거나 shelf와 충돌 | 다른 면을 사용할 수 있도록 물체와 손의 관계 변경 |
| 불리한 OBB face 방향 | 선택 면의 pushing normal과 목표 방향이 어긋나 off-center push가 slip·unintended rotation 유발 | Preparatory rotation으로 선택 면을 push direction에 정렬 |
| Coarse geometry의 오차 | OBB가 실제 국소 표면·마찰·질량 분포를 설명하지 못함 | Tactile·F/T feedback으로 접촉 후 보정 |
| 단계 사이의 불량한 접촉 | 물체에는 닿았지만 Rotation이 어렵거나, face alignment는 맞았지만 Push에 부적합 | 다음 단계의 실행 가능성을 고려한 hand/contact state 형성 |

앞의 두 조건은 preparatory rotation이 필요한 이유를 설명한다. 뒤의 두 조건은 rotation을 수행하는 것만으로는 문제가 끝나지 않는 이유를 설명한다.

```text
Direct push의 실행 가능 영역이 제한됨
        ↓
Approach에서 Rotation-ready hand configuration 필요
        ↓
Preparatory Rotation 수행
        ↓
Rotation terminal contact가 Push initial condition을 결정
        ↓
전체 Rotation→Push 성공으로 각 상태를 평가해야 함
```

따라서 Approach의 성공을 최초 접촉으로, Rotation의 성공을 선택 면 정렬만으로 정의하면 부족하다.

---

## 3. 기존 연구가 해결한 것과 남은 문제

### 3.1 이미 해결되고 있는 부분

최신 연구를 `VLA는 힘을 모른다`, `IL은 접촉 변화에 반응하지 못한다`와 같이 일반화해서 비판할 수는 없다.

| 연구 흐름과 대표 사례 | 이미 보여준 발전 | Track B에서 별도로 확인할 질문 |
| --- | --- | --- |
| Generalist·efficient VLA — [π0.5](https://doi.org/10.48550/arXiv.2504.16054), [OpenVLA-OFT](https://doi.org/10.15607/RSS.2025.XXI.017) | 장기 household task 일반화와 빠른 VLA adaptation | Semantic generalization과 낮은 latency가 shelf 내부의 contact feasibility·force safety까지 보장하는가 |
| Force·tactile-aware VLA/IL — [ForceVLA](https://doi.org/10.52202/085713-3124), [Reactive Diffusion Policy](https://doi.org/10.15607/RSS.2025.XXI.052), [FoAR](https://doi.org/10.1109/LRA.2025.3560871) | Force·tactile grounding과 action 실행 중의 빠른 반응 | 반응성이 각 phase의 contact state를 최종 Push 성공에 맞게 최적화하는가 |
| Contact-rich·long-horizon RL — [FORGE](https://doi.org/10.1109/LRA.2025.3551637), [Privileged Action](https://doi.org/10.48550/arXiv.2502.15442), [OmniReset](https://doi.org/10.48550/arXiv.2603.15789) | Sim-to-Real randomization, exploration curriculum과 reset coverage 개선 | 같은 exploration 조건에서도 downstream-aware objective의 이점이 남는가 |
| 인접 dexterous·nonprehensile 연구 — [DyWA](https://doi.org/10.48550/arXiv.2503.16806), [DexMove](https://openreview.net/forum?id=dT3ZciXvNX), [GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Dynamics adaptation, tactile wrist–finger control과 geometry-conditioned contact pose | Coarse deployable sensing으로 Approach→Rotation→Push 전체를 폐루프로 연결할 수 있는가 |

이 연구들은 범용성, 반응성, 접촉 sensing과 탐색이라는 중요한 문제를 해결한다. 따라서 Track B의 gap은 이 계열 전체가 contact-rich manipulation을 못한다는 주장이 아니다.

### 3.2 남아 있는 구체적 gap

기존 연구와의 차이는 네 질문으로 좁힌다.

| Gap | Track B에서 검증할 내용 |
| --- | --- |
| **Downstream objective** | Approach를 최초 contact가 아니라 Rotation→Push feasibility로, Rotation을 face-alignment error뿐 아니라 subsequent Push feasibility까지 포함해 평가 |
| **Contact transition** | Approach·Rotation·Push를 독립 성공으로 보지 않고 앞 단계의 terminal contact와 다음 단계의 initial condition을 연결 |
| **Deployable sensing** | Dense mesh·optical tactile 없이 coarse OBB·binary tactile·wrist F/T로 geometry–contact mismatch를 보정할 수 있는지 확인 |
| **Fair attribution** | Direct push, face-alignment-only objective, sensor 제거와 fixed-hand baseline을 분리 비교해 실제 이득의 원인을 확인 |

이로부터 현재의 research question이 나온다.

> **Unknown shelf blocker의 선택된 OBB 면을 목표 방향에 정렬한 뒤 밀 때, Approach의 wrist–hand configuration과 Rotation의 terminal contact를 최종 Rotation→Push의 실행 가능성·접촉 안정성·힘 안전성을 보존하도록 학습할 수 있는가?**

두 번째 질문은 이를 고해상도 tactile image나 대규모 real multimodal demonstration 없이 달성할 수 있는가이다. 이는 real data 부담을 줄일 가능성에 관한 가설이며, simulation interaction, reward engineering과 domain randomization 비용까지 사라진다는 뜻은 아니다.

---

## 4. Track B는 이 문제에 어떻게 대응하는가

Track B는 continuous object pose와 coarse OBB, binary tactile, wrist F/T와 proprioception을 사용하는 phase-ID-free shared policy를 학습한다.

```text
Coarse pose·OBB
  → 접근 위치와 초기 hand configuration 제안

Binary tactile·wrist F/T
  → 실제 접촉이 geometry 예측과 어떻게 다른지 보정

Downstream-aware objective
  → 보정된 hand/contact state가 Rotation과 Push로 이어지도록 학습
```

| 구성 | 현재 선택 | 이 선택이 답하려는 문제 |
| --- | --- | --- |
| Goal | 상위가 제공한 target position, push direction과 selected OBB face | 목표 선택과 low-level contact execution 분리; 목표 quaternion 제거 |
| Geometry | Continuous pose + episode-consistent OBB | Occlusion 아래에서 안정적인 저차원 prior 제공 |
| Contact feedback | Current 17D binary tactile + current wrist 6D F/T | 접촉 위치와 전체 force·moment를 상보적으로 관측 |
| Proprioception | Current arm·hand q + previous action 1-step | 현재 configuration과 직전 명령 이후 반응 구분 |
| Action | EEF-frame delta pose + hand joint action | Wrist와 fingers를 함께 조절해 contact 형성·전환 |
| Training supervision | Exact contact·force·collision은 reward·termination·evaluation에만 사용 | 실물 actor가 얻을 수 없는 정보를 deployment input에서 분리 |

Approach에서는 특정 hand pose를 정답으로 imitation하지 않는다. Shared policy의 실제 future return이 좋은 configuration을 학습하게 하고, saved-state continuation으로 face-alignment 성공률과 최종 Rotation→Push 성공률을 따로 측정한다. 최초 접촉 이후에는 aggregate hand–object contact 유지를 선호하지만 개별 contact migration은 허용한다. [Grasp to Act](https://doi.org/10.1109/LRA.2026.3677744)는 task-informed 초기 grasp와 작은 online adaptation의 결합을 지지하고, [Guided Exploration with Sub-skill Controllers](https://doi.org/10.1109/ICRA57147.2024.10611300)와 [Tac2Motion](https://doi.org/10.48550/arXiv.2509.17812)은 필요한 contact switching까지 금지하면 조작 범위를 제한할 수 있음을 보여준다. 따라서 Track B는 `minimum necessary reconfiguration`을 검증 가설로 두며 고정 contact set을 정답으로 강제하지 않는다. 세부 구현은 [`policy_learning.md`](./policy_learning.md)를 따른다.

---

## 5. 무엇으로 가설을 검증할 것인가

아래 MH1–MH4는 연구 결과가 아니라 현재 검증할 working hypotheses다.

| ID | 가설 | 핵심 비교 | 주요 지표 |
| --- | --- | --- | --- |
| MH1 | Direct push가 어려운 영역에서 preparatory rotation이 성공 영역을 넓힌다. | Direct push vs. rotate-then-push | 전체 성공률, 성공 가능한 initial-state volume, collision·slip |
| MH2 | Face-alignment-only objective보다 downstream-aware contact transition이 전체 성공률을 높인다. | Alignment-only vs. downstream-aware | Alignment 성공을 통제한 Push success, $F_{R\rightarrow P}$, contact loss, peak force |
| MH3 | Binary tactile와 wrist F/T가 coarse pose·OBB의 contact uncertainty를 보완한다. | Vision/OBB only, F/T only, tactile only, tactile+F/T; coarse vs. 17D | Contact loss, force, recovery와 disturbance robustness |
| MH4 | Task-conditioned 초기 configuration과 필요한 만큼의 wrist–finger adaptation이 fixed hand 또는 unconstrained reconfiguration보다 효율적·강건하다. | Fixed hand vs. free adaptation vs. mild reconfiguration cost | Held-out success, contact loss·switch, hand joint travel와 seen–unseen gap |

가설이 지지되지 않으면 주장도 다음과 같이 줄인다.

- MH1 미지지: Preparatory rotation의 보편적 필요성이 아니라 필요 조건 분석으로 한정
- MH2 미지지: Downstream-aware transition을 contribution에서 제외
- MH3 미지지: Coarse sensing을 장점이 아닌 limitation으로 보고
- MH4 미지지: Unseen-object robustness 대신 학습 분포 내부의 실행 문제로 한정

---

## 6. 가능한 contribution과 주장 경계

### 6.1 실험이 지지할 때 가능한 contribution

1. **Downstream-aware contact-transition formulation:** Approach와 Rotation의 상태를 최종 Push feasibility에 연결하는 문제 정의와 objective
2. **Deployable multimodal contact policy:** Coarse OBB·binary tactile·wrist F/T로 wrist–finger configuration을 폐루프 조절하는 shared policy
3. **분리 가능한 평가 체계:** Direct push, face-alignment-only, sensor와 contact-adaptation baseline으로 각 가설의 원인을 구분하는 평가

### 6.2 현재 주장하지 않는 범위

- Open-world language instruction과 generalist household-task 성능
- 여러 robot embodiment 사이의 범용 transfer
- Vision tracker 또는 고해상도 tactile reconstruction의 개선
- Real-world online adaptation 전반
- Reward engineering·simulation data·Sim-to-Real 비용의 제거
- Domain-randomization 범위를 넘는 임의의 물체·물성 일반화

가장 방어 가능한 positioning은 다음과 같다.

> Track B는 VLA·IL·RL 전체를 대체하는 방법이 아니라, 제한된 shelf에서 **다음 조작까지 실행 가능한 hand/contact state를 형성하는 physical execution layer**를 연구한다.

`기존 방법을 극복한다`는 표현은 observation, action, data·compute와 실물 trial budget을 합리적으로 맞춘 baseline에서 일관된 이점이 확인된 뒤에만 사용한다.

---

## 7. 논문 Introduction용 압축 초안

로봇의 적용 범위가 구조화된 산업 환경에서 물류 선반과 가정으로 확대되면서, 목표 물체에 접근하기 위해 주변 물체를 밀거나 돌리는 contact-rich manipulation의 중요성이 커지고 있다. 이러한 환경에서는 접촉 mode가 바뀌고 국소 형상·마찰·pose 오차가 물체 운동과 힘에 큰 영향을 주기 때문에, unseen object마다 정확한 contact model과 mode sequence를 구성하는 부담이 크다.

최근 VLA·IL·RL은 장기 task 일반화, 빠른 action generation, force/tactile grounding과 contact-rich Sim-to-Real을 발전시켰다. 따라서 기존 policy가 contact를 보지 못하거나 반응하지 못한다는 일반적 비판은 충분하지 않다. 그러나 direct push가 어려운 shelf blocker의 선택 면을 목표 방향에 먼저 정렬해야 할 때, Approach의 hand configuration과 Rotation의 terminal contact가 최종 Push까지 실행 가능한지를 명시적으로 학습·평가하는 문제는 별도로 남는다.

본 연구는 continuous object pose와 coarse OBB, binary tactile, wrist F/T와 proprioception을 사용하는 goal-conditioned RL policy로 Approach→Rotation→Push의 contact transition을 폐루프로 조절한다. Simulation의 정확한 contact·force 정보는 학습 신호에만 사용하고 실제 actor는 배포 가능한 저차원 sensing으로 제한한다. 이를 통해 preparatory rotation의 필요 조건, downstream-aware transition, contact sensing과 adaptive hand configuration이 전체 pushing success와 force safety에 미치는 효과를 분리 검증한다.

> **One-sentence version:** We study goal-conditioned manipulation of unseen shelf blockers, where a selected object face is aligned with the desired push direction while the approach configuration and rotation-terminal contact remain feasible for subsequent pushing.

---

## 8. 근거를 더 확인하려면

- 최신 VLA·IL·RL과 Track B 비교: [`papers/topic_groups.md`](./papers/topic_groups.md#8-최신-vlail-기반-research-motivation)
- 핵심 논문의 서지정보와 공식 링크: [`papers/core_papers.md`](./papers/core_papers.md)
- Reward와 transition 근거: [`papers/reward_formulation.md`](./papers/reward_formulation.md)
- 현재 연구 범위: [`research_topic.md`](./research_topic.md)
