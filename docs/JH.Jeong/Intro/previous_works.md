# Closest Previous Works — Environment, Robot Agent와 System 비교

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 가까운 nonprehensile manipulation 연구를 소수의 공통 기준으로 분류해 Ours와의 차이를 확인하고, C1·C2에 남는 질문을 정리한다.
>
> **주의:** 여기서 Agent는 AI agent가 아니라 **환경과 물리적으로 상호작용하는 robot agent**를 뜻한다. 표의 분류가 다르다는 사실만으로 research gap이나 contribution을 선언하지 않는다.

---

## 1. Column 기준

비교표에는 Environment, Robot Agent와 System에 해당하는 다음 여섯 column만 사용한다. 논문마다 서로 다른 설명을 넣기보다 각 column의 정해진 값 중 하나로 분류한다.

| 구분 | Column | 사용하는 값 | 판정 기준 |
| --- | --- | --- | --- |
| **Environment** | **Scene** | `Single` / `Cluttered` | Manipulation 대상 외의 movable object가 없으면 `Single`, 주변 movable object가 함께 있으면 `Cluttered` |
| **Environment** | **Workspace** | `Open` / `Constrained` / `Mixed` | 주변 구조물이 동작을 제한하지 않으면 `Open`, shelf·wall·fixture가 제한하면 `Constrained`, 두 조건을 모두 다루면 `Mixed` |
| **Robot Agent** | **Sensing** | `Vision` / `Contact` / `Vision+Contact` | `Contact`는 tactile, contact state와 wrist F/T를 포함한다. 대부분 공통인 proprioception은 분류에서 생략한다. |
| **Robot Agent** | **Object Geometry** | `None` / `Estimated` / `Exact` | 명시적 shape 표현이 없으면 `None`, OBB·estimated point cloud이면 `Estimated`, 정확한 CAD·mesh·dimension이면 `Exact` |
| **Robot Agent** | **Manipulation** | `Translation` / `Reorientation` / `Combined` | Push·pull만 다루면 `Translation`, rotation·pivot이 중심이면 `Reorientation`, 둘을 모두 다루면 `Combined` |
| **System** | **Method** | `Non-learning` / `Learning` / `Hybrid` | Heuristic·control·optimization/planning만 사용하면 `Non-learning`, RL·IL·VLA가 주된 action 생성 방법이면 `Learning`, learning과 planning/control이 모두 핵심이면 `Hybrid` |

`Object Geometry`는 robot agent가 실행 중 받는 **명시적 형상 정보**를 기준으로 한다. RGB나 pose만 사용해 명시적인 shape를 입력하지 않는 경우는 `None`이며, simulator가 exact geometry를 갖더라도 robot agent에 주어지지 않으면 `Exact`로 분류하지 않는다.

현재 Stage 1은 manipulation 대상 blocker의 pose와 geometry를 지속적으로 관측할 수 있다고 가정하므로 observability는 비교 column으로 두지 않는다. 현재 가까운 연구도 주어진 goal 이후에는 자율 실행하므로 autonomy 역시 비교 column에서 제외한다. Human demonstration은 training source이지 실행 중 manual control을 뜻하지 않는다.

---

## 2. 비교표

| Work | Scene | Workspace | Sensing | Object Geometry | Manipulation | Method |
| --- | --- | --- | --- | --- | --- | --- |
| [B37 · Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) | Single | Open | Contact | None | Translation | Non-learning |
| [B84 · Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) | Cluttered | Open | Vision+Contact | None | Translation | Learning |
| [B12 · Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157) | Single | Open | Vision+Contact | None | Translation | Learning |
| [B92 · Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135) | Single | Constrained | Contact | Exact | Combined | Non-learning |
| [B90 · HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154) | Single | Mixed | Vision | Estimated | Combined | Learning |
| [B01 · GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Single | Open | Vision | Estimated | Translation | Hybrid |
| [B22 · DexMove](https://openreview.net/forum?id=dT3ZciXvNX) | Single | Open | Vision+Contact | Estimated | Combined | Learning |
| [B81 · Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Single | Mixed | Vision+Contact | None | Combined | Hybrid |
| [B46 · ForceVLA](https://doi.org/10.52202/085713-3124) | Single | Mixed | Vision+Contact | None | Combined | Learning |
| [B48 · Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) | Single | Mixed | Vision+Contact | None | Combined | Hybrid |
| **Ours** | **Single** | **Constrained** | **Vision+Contact** | **Estimated** | **Combined** | **Learning** |

B01의 raw sensing source와 geometry 정확도, B22의 manipulation 범위, B46·B48의 전체 task suite 범위는 full text에서 우선 재확인한다. 표의 값은 현재 파악한 범위에서 가장 가까운 공통 분류다.

### Ours를 읽는 기준

- `Single`: 현재 S0에는 manipulation 대상 blocker 외의 추가 movable object가 없다.
- `Constrained`: shelf가 접근·회전·병진 공간을 제한한다.
- `Vision+Contact`: continuous vision-derived pose와 OBB, tactile, wrist F/T를 함께 사용한다.
- `Estimated`: robot agent는 exact mesh가 아니라 estimated OBB를 받는다.
- `Combined`: 필요한 reorientation/pivoting과 이후 translation을 모두 다룬다.
- `Learning`: Stage 1의 주된 action 생성 방법은 RL이다. Scripted heuristic은 효과를 비교하기 위한 별도 baseline이다.

---

## 3. 비교에서 보이는 범위

현재 표에서 Ours와 모든 column이 같은 연구는 없다.

- B01과 B22는 `Estimated` object geometry를 사용하는 가까운 비교 연구다. B01은 pre-contact configuration과 planner execution을, B22는 tactile wrist–finger execution을 보여준다.
- B81은 translation과 reorientation을 모두 다루고 vision·contact feedback을 사용하지만, optimization과 RL을 결합한 `Hybrid`이며 workspace 범위도 다르다.
- B92는 constrained workspace와 combined manipulation을 다루지만 exact model과 non-learning method를 사용한다.
- B46과 B48은 vision과 contact sensing을 learning method에 결합할 수 있음을 보여주지만, 여러 contact-rich task를 다루는 인접 연구다.

이 차이는 연구 질문을 좁히는 근거이지 novelty의 증거는 아니다. 특히 `Vision+Contact`, `Estimated` geometry 또는 `Combined` manipulation이라는 조합만으로 contribution을 주장하지 않는다.

---

## 4. 남은 질문

1. **C1:** Estimated geometry에 오차가 있을 때 contact feedback이 Rotation-to-Push와 final-task 성능 저하를 줄이는가?
2. **C2:** Task-conditioned pre-contact formation 이후 contact 중 online wrist–finger adaptation이 추가 이득을 주는가?

직접 수치 비교에서는 같은 Scene, Workspace, Sensing과 Object Geometry 조건을 맞춘다. 조건이 다른 논문은 특정 요소의 근거나 인접 연구의 반례로 사용하고, 원 논문의 success rate를 Ours와 그대로 대조하지 않는다.

각 cell은 full text에서 확인한다. 현재 근거가 부족한 분류는 후속 원문 검토에서 수정하며, 세부 observation·action·training source와 결과는 [Paper Index](../papers/README.md)와 각 독서 문서에 남긴다.
