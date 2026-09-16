# Track B Research Motivation

> **연구 주제:** Shelf retrieval을 위한 goal-conditioned blocker-object contact manipulation
>
> **문서 상태:** Working synthesis — 핵심 문제, research gap과 검증 가설을 구조화한 문서
>
> **최종 갱신:** 2026-09-16

---

## 0. 한눈에 보는 핵심

| 항목 | 핵심 내용 |
| --- | --- |
| 실제 상황 | Shelf 안의 target을 꺼내려면 앞의 blocker를 재배치해야 한다. |
| 핵심 failure | 제한된 접근 방향과 blocker의 초기 자세 때문에 direct push가 불가능하거나 불안정할 수 있다. |
| 중요한 전환 | Preparatory Rotation은 target yaw에 도달하는 것으로 끝나지 않고, 후속 Push가 사용할 수 있는 접촉 상태를 남겨야 한다. |
| Research gap | 최신 VLA·IL·RL은 범용성, tactile/force feedback, 반응성과 contact-rich learning을 발전시켰지만, 제한된 shelf에서 **Rotation terminal contact를 Push initial contact로 최적화하는 문제**는 직접적인 평가 대상이 아니었다. |
| Track B의 접근 | Coarse pose·OBB, binary tactile, wrist F/T와 proprioception을 사용하는 shared policy가 Approach–Rotation–Push의 접촉 전환을 폐루프로 실행하도록 학습한다. |
| 입증해야 할 것 | Direct push 대비 preparatory rotation의 필요성, orientation-only 대비 downstream-aware transition의 이점, tactile/F/T의 보완 효과와 unseen condition 강건성을 분리 검증한다. |

> **핵심 주장 후보:** 제한된 shelf에서 preparatory rotation의 성공은 목표 orientation 도달만으로 정의할 수 없으며, 후속 pushing에 실행 가능한 contact configuration을 형성했는지까지 포함해야 한다.

```text
Shelf retrieval을 막는 blocker
        ↓
Direct push의 실행 가능 영역이 제한됨
        ↓
Preparatory Rotation 필요
        ↓
Rotation terminal contact가 Push initial contact를 결정
        ↓
Downstream-aware contact transition을 학습·평가
```

현재 명세는 [`research_topic.md`](./research_topic.md), 논문과 baseline 근거는 [`papers.md`](./papers.md), 결정 과정은 [`context.md`](./context.md)를 따른다.

---

## 1. 왜 이 문제가 필요한가

### 1.1 Shelf retrieval은 collision-free reaching만으로 끝나지 않는다

물류 선반과 생활환경에서는 target이 다른 물체에 가려지거나 접근 경로가 막힐 수 있다. 이 경우 로봇은 target을 바로 grasp하기 전에 blocker를 밀거나 돌려 시야와 접근 공간을 확보해야 한다. 즉 환경과의 접촉을 피하는 것이 아니라, 접촉을 의도적으로 만들고 그 결과에 따라 행동을 수정해야 한다.

Contact-rich manipulation에서는 stick·slip·pivot·충돌에 따라 dynamics와 constraint가 바뀐다. 또한 국소 형상, 마찰, 질량 분포와 pose의 작은 오차가 다른 물체 운동과 접촉력으로 이어진다. 정확한 모델이 있으면 model-based planning과 control이 강력하지만, unseen object와 occlusion이 있는 shelf에서는 contact model·mode sequence를 매번 정확히 구성하는 부담이 커진다.

### 1.2 Direct push가 실패하는 이유

| 원인 | 대표적인 failure |
| --- | --- |
| 접근 제약 | 원하는 pushing direction과 가능한 EEF 접근 방향이 맞지 않거나 hand·arm이 shelf와 충돌함 |
| 초기 물체 자세 | 안정적인 접촉 surface를 사용할 수 없고, off-center contact가 slip이나 unintended rotation을 유발함 |
| 물리적 불확실성 | Coarse geometry가 실제 국소 형상·마찰·질량 분포를 충분히 설명하지 못함 |
| 접촉 전환 실패 | Rotation orientation은 맞았지만 손목·손가락 배치가 Push에 부적합하여 re-contact, contact loss 또는 과도한 힘이 발생함 |

앞의 세 원인은 preparatory rotation의 필요성을 만들고, 마지막 원인은 본 연구의 중심 질문을 만든다.

### 1.3 Preparatory Rotation의 목적

Preparatory rotation은 물체를 특정 자세로 만드는 독립 과업이 아니다. 목표 방향으로 pushing wrench를 전달할 surface와 object–hand 관계를 만들고, shelf constraint 안에서 후속 Push를 수행할 손목·손가락 configuration을 준비하는 수단이다.

따라서 다음 두 상태는 구분해야 한다.

- **Orientation-success state:** 물체가 목표 orientation에 도달했지만 후속 Push를 위해 접촉을 다시 만들어야 하는 상태
- **Transition-success state:** 목표 orientation과 함께 후속 Push를 즉시 또는 안정적으로 시작할 수 있는 접촉 상태

Track B는 두 번째 상태를 학습 목표와 평가 대상으로 삼는다.

---

## 2. 기존 연구가 해결한 것과 남은 Gap

구체 논문의 서지정보·게재 상태·영향력과 baseline 역할은 [`papers.md`](./papers.md)의 3.8절에서 관리한다. 여기서는 motivation에 필요한 결론만 사용한다.

### 2.1 최신 연구가 이미 제거한 단순한 비판

| 연구 흐름과 대표 사례 | 이미 확인된 발전 | Track B에서 별도로 남는 질문 |
| --- | --- | --- |
| Generalist·efficient VLA — π0.5, OpenVLA-OFT | 장기 household task 일반화와 빠른 VLA adaptation | Semantic generalization과 낮은 latency가 shelf 내부의 contact feasibility·force safety까지 보장하는가 |
| Force·tactile-aware policy — ForceVLA, Reactive Diffusion Policy, FoAR | Force/tactile을 이용한 contact grounding과 고주파 반응 | 반응성이 Rotation 종료 접촉을 downstream Push 성공에 맞게 최적화하는가 |
| Contact-rich·long-horizon RL — FORGE, Privileged Action, OmniReset | Sim-to-Real randomization, exploration curriculum과 reset coverage 개선 | 동일 exploration 조건에서도 downstream-aware objective와 phase-free transition의 이점이 남는가 |
| 인접 nonprehensile·dexterous 연구 — DyWA, DexMove, GD2P | Dynamics adaptation, tactile wrist–finger control과 geometry-conditioned contact pose | Coarse deployable sensing으로 Rotation–Push contact transition 전체를 폐루프로 실행할 수 있는가 |

따라서 다음은 research gap으로 사용하지 않는다.

- VLA가 force나 tactile을 전혀 사용하지 못한다는 주장
- IL이 contact 변화에 반응할 수 없다는 주장
- RL, tactile, 다지 손 또는 Rotation–Push를 결합했다는 사실 자체
- VLA와 RL을 결합하면 그 자체로 새롭다는 주장

### 2.2 남아 있는 구체적 Gap

| Gap | 기존 연구와 구분되는 검증 대상 |
| --- | --- |
| **Downstream objective gap** | Rotation의 성공을 orientation error가 아니라 후속 Push feasibility까지 포함해 정의 |
| **Contact-transition gap** | Approach–Rotation–Push를 개별 성공으로 평가하지 않고 이전 phase의 terminal contact와 다음 phase의 initial contact를 연결 |
| **Deployable-sensing gap** | Dense mesh·optical tactile 대신 coarse OBB·binary tactile·wrist F/T만으로 contact mismatch를 보정할 수 있는지 검증 |
| **Fair-evaluation gap** | Direct push, orientation-only transition, sensor 제거와 fixed contact를 분리 비교하여 어떤 요소가 실제 이득을 만드는지 확인 |

> **Research question:** Unknown shelf blocker를 목표 방향으로 밀기 위한 preparatory rotation에서, terminal contact가 downstream Push의 실행 가능성·접촉 안정성·힘 안전성을 보존하도록 학습할 수 있는가?

이 질문의 두 번째 층위는 그러한 학습이 고해상도 tactile image나 대규모 real multimodal demonstration 없이 가능한가이다. 이는 real data 부담을 줄일 가능성에 관한 가설이지, simulation interaction·reward engineering·domain randomization 비용까지 제거한다는 뜻은 아니다.

---

## 3. Track B가 제안하는 대응

### 3.1 문제 formulation

```text
Approach / Contact Formation
          ↓
Rotation: object goal + Push-feasible contact 형성
          ↓
Contact를 유지·전환
          ↓
Push: target direction·distance 달성
```

Policy에는 phase ID를 주지 않는다. Shared policy가 pose, proprioception, tactile·F/T와 action history에서 현재 접촉 상태와 필요한 전환을 추론한다. Phase별 reward gate는 학습 신호의 활성 조건이며 action을 phase별로 고정하는 장치가 아니다.

### 3.2 Method contract와 motivation의 연결

| 구성 | 현재 방향 | 해결하려는 문제 |
| --- | --- | --- |
| Goal | 상위 모듈이 제공한 pushing target position과 preparatory object orientation | 목표 선택과 low-level contact execution을 분리 |
| Geometry | Continuous object pose와 episode-consistent coarse OBB | Unseen object·occlusion에서 안정적인 저차원 geometric prior 제공 |
| Contact observation | Binary any-contact tactile, wrist 6D F/T와 modality별 history | Geometry만으로 알 수 없는 접촉 위치·강도·action response 보완 |
| Proprioception | Current arm·hand joint state와 previous action history | 현재 hand configuration과 actuator/contact response 구분 |
| Action | Measured state 기준 EEF-frame delta pose + hand joint action | Wrist와 fingers를 함께 조절해 접촉을 형성·유지·전환 |
| Training-only information | Exact contact force, collision pair와 task state를 reward·termination·critic에 사용 | 실제 actor에 불가능한 정보를 노출하지 않으면서 학습 신호를 정밀화 |
| Actor structure | Phase-ID-free shared MLP policy | 관측 가능한 상태만으로 long-horizon contact transition을 수행하는지 검증 |

핵심 메커니즘은 다음과 같다.

> **Coarse vision은 어디에 어떻게 접근할지 제시하고, tactile·F/T는 실제 접촉이 예상과 어떻게 다른지 보정하며, downstream-aware objective는 그 보정이 다음 Push에 유효한 방향으로 이루어지도록 제약한다.**

---

## 4. 검증 가설과 필요한 증거

아래 MH1–MH4는 아직 결과가 아닌 working hypotheses다.

| ID | 검증 가설 | 핵심 비교 | 주요 지표 | 지지될 때 가능한 주장 |
| --- | --- | --- | --- | --- |
| MH1 | 접근 제약과 초기 orientation 때문에 direct push가 어려운 영역에서 preparatory rotation이 성공 영역을 넓힌다. | Direct push vs. rotate-then-push | 전체 성공률, 성공 가능한 initial-state volume, collision·slip | Preparatory rotation이 필요한 조건과 효과를 규명 |
| MH2 | Orientation-only Rotation보다 downstream Push feasibility를 고려한 transition objective가 전체 성공률을 높인다. | Orientation-only vs. downstream-aware transition | Rotation success를 통제한 Push success, 즉시 Push 가능률, re-contact, peak force | Terminal contact quality의 독립적 가치 입증 |
| MH3 | Binary tactile와 wrist F/T가 coarse pose·OBB의 contact uncertainty를 보완한다. | Vision/OBB only, F/T only, tactile only, tactile+F/T; coarse vs. 17-channel | Contact loss, force, recovery, disturbance robustness | 배포 가능한 contact feedback의 효과와 필요한 tactile granularity 규명 |
| MH4 | Goal-conditioned wrist–finger adaptation이 fixed contact보다 unseen condition에 강건하다. | Fixed hand/contact vs. adaptive policy | Held-out geometry·friction·mass 성공률과 seen–unseen gap | 정의한 randomization 범위에서 adaptive contact control의 이점 입증 |

결과가 가설을 지지하지 않으면 표현도 제한한다.

- MH1 미지지: `preparatory rotation이 필요하다`가 아니라 `필요 조건을 분석했다`고 서술
- MH2 미지지: downstream-aware transition을 contribution으로 주장하지 않음
- MH3 미지지: coarse sensing을 장점으로 주장하지 않고 sensor limitation으로 보고
- MH4 미지지: unseen-object robustness 대신 학습 분포 내부의 task execution으로 범위를 축소

---

## 5. 예상 Contribution과 주장 경계

### 5.1 실험이 지지할 경우의 Contribution 후보

1. **Downstream-aware contact-transition formulation:** Preparatory Rotation의 terminal state를 후속 Push feasibility와 연결하는 문제 정의와 objective
2. **Deployable multimodal contact policy:** Coarse OBB·binary tactile·wrist F/T로 wrist–finger contact configuration을 폐루프 조절하는 phase-ID-free policy
3. **Privileged-to-deployable learning and evaluation:** Simulation의 정확한 contact 정보를 학습에만 사용하고 direct push·orientation-only·sensor·contact adaptation baseline으로 가설을 분리 검증하는 체계

### 5.2 주장하지 않는 범위

- Open-world language instruction과 generalist household-task 성능
- 여러 robot embodiment 사이의 범용 transfer
- Vision tracker 자체의 개선
- 고해상도 tactile reconstruction이나 정확한 contact localization
- Real-world online adaptation 전반
- Reward engineering·simulation data·Sim-to-Real 비용의 제거
- Domain-randomization 범위를 넘는 임의의 물체·물성 일반화

### 5.3 가장 방어 가능한 Positioning

> Track B는 VLA·IL·RL 전체를 대체하는 방법이 아니라, generalist reasoning이나 고정보량 tactile sensing과 구분되는 **제한된 shelf의 downstream-aware physical execution layer**를 연구한다.

`기존 방법을 극복한다`는 표현은 observation·action·budget을 합리적으로 맞춘 강한 baseline에서 task success, force safety, robustness와 비용의 일관된 이점이 확인된 뒤에만 사용한다.

---

## 6. 논문 Introduction용 압축 초안

로봇의 적용 범위가 구조화된 산업 환경에서 물류 선반과 가정으로 확대되면서, 목표 물체에 접근하기 위해 주변 물체를 밀거나 돌리는 contact-rich manipulation의 중요성이 커지고 있다. 이러한 환경에서는 접촉 mode가 계속 바뀌고 국소 형상·마찰·pose 오차가 물체 운동과 힘에 큰 영향을 주기 때문에, unseen object마다 정확한 contact model과 mode sequence를 구성하는 부담이 크다.

최근 VLA·IL·RL은 장기 task 일반화, 빠른 action generation, force/tactile grounding과 contact-rich Sim-to-Real을 빠르게 발전시켰다. 따라서 기존 policy가 contact를 보지 못하거나 반응하지 못한다는 일반적 비판은 더 이상 충분하지 않다. 그러나 제한된 shelf에서 direct push가 어려운 blocker를 먼저 회전할 때, 목표 orientation뿐 아니라 후속 Push가 즉시 사용할 수 있는 terminal contact를 형성하는 문제는 별도의 검증 대상으로 남는다.

본 연구는 continuous object pose와 coarse OBB, binary tactile, wrist F/T와 proprioception을 사용하는 goal-conditioned RL policy로 Approach–Rotation–Push의 contact transition을 폐루프로 조절한다. Simulation의 정확한 contact·force 정보는 reward와 critic에만 사용하며, 실제 actor는 배포 가능한 저차원 sensing에 제한한다. 이를 통해 preparatory rotation의 필요 조건, downstream-aware transition, contact sensing과 adaptive hand configuration이 전체 pushing 성공과 force safety에 미치는 효과를 분리 검증한다.

> **One-sentence version:** We study goal-conditioned manipulation of unseen shelf blockers, where a preparatory rotation must terminate not only at a target orientation but also in a contact configuration that remains feasible for subsequent pushing.
