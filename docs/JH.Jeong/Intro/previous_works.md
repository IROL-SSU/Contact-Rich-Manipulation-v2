# Closest Previous Works — Timeline and Environment–Robot Agent–System Comparison

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 가까운 nonprehensile manipulation 연구를 소수의 공통 기준으로 분류하고, 동일한 비교 집합의 timeline을 통해 method concept의 변화를 확인한 뒤 Ours와의 차이 및 C1·C2에 남는 질문을 정리한다.
>
> **주의:** 여기서 Agent는 AI agent가 아니라 **환경과 물리적으로 상호작용하는 robot agent**를 뜻한다. 표의 분류가 다르다는 사실만으로 research gap이나 contribution을 선언하지 않는다.

---

## 1. Column 기준

비교표에는 Environment, Robot Agent와 System에 해당하는 다음 일곱 column만 사용한다. 논문마다 서로 다른 설명을 넣기보다 각 column의 정해진 값 중 하나로 분류한다.

| 구분 | Column | 사용하는 값 | 판정 기준 |
| --- | --- | --- | --- |
| **Environment** | **Scene** | `Uncluttered` / `Cluttered` | 주변 movable object가 manipulation에 영향을 주는 scene을 method가 명시적으로 다루면 `Cluttered`, 그렇지 않으면 `Uncluttered` |
| **Environment** | **Workspace** | `Open` / `Constrained` / `Mixed` | 주변 구조물이 동작을 제한하지 않으면 `Open`, shelf·wall·fixture가 제한하면 `Constrained`, 두 조건을 모두 다루면 `Mixed` |
| **Robot Agent** | **Sensing** | `Vision` / `Contact` / `Vision+Contact` | `Contact`는 tactile, contact state와 wrist F/T를 포함한다. 대부분 공통인 proprioception은 분류에서 생략한다. |
| **Robot Agent** | **Object Geometry** | `None` / `Estimated` / `Exact` | 명시적 shape 표현이 없으면 `None`, OBB·estimated point cloud·depth-derived shape/size feature처럼 sensor에서 추정한 표현이면 `Estimated`, 정확한 CAD·mesh·dimension이면 `Exact` |
| **Robot Agent** | **End-effector Reconfiguration** | `Fixed` / `Pre-contact` / `Online` | Wrist pose와 별개로 gripper aperture나 finger posture 같은 내부 contact-interface configuration을 언제 결정·갱신하는가? |
| **Robot Agent** | **Manipulation** | `Translation` / `Reorientation` / `Combined` | Push·pull만 다루면 `Translation`, rotation·pivot이 중심이면 `Reorientation`, 둘을 모두 다루면 `Combined` |
| **System** | **Method** | `Control` / `Optimization` / `Generative` / `RL` / `IL` / `VLA` | 배포 시 task-level action 또는 실행할 pose 후보를 만드는 주된 mechanism은 무엇인가? |

`Object Geometry`는 robot agent가 실행 중 받는 **명시적 형상 정보**를 기준으로 한다. RGB나 pose만 사용해 명시적인 shape를 입력하지 않는 경우는 `None`이며, simulator가 exact geometry를 갖더라도 robot agent에 주어지지 않으면 `Exact`로 분류하지 않는다.

`End-effector Reconfiguration`은 physical tool의 종류가 아니라 deployed method가 **내부 contact-interface configuration을 언제 능동적으로 결정·갱신하는지**를 나타낸다. Method가 task·object에 맞춘 내부 configuration을 생성하지 않고 preset을 유지하면 `Fixed`, task·object에 맞춰 접촉 전에 configuration을 선택한 뒤 각 contact episode에서는 유지하면 `Pre-contact`, 접촉 이후에도 gripper width나 finger joints를 갱신하면 `Online`이다. Wrist/EEF pose 변화, arm motion에 따른 contact-point 이동, target-force 변경과 passive compliance는 reconfiguration으로 세지 않는다. 따라서 닫힌 gripper의 preset을 유지하며 미는 방식과 rigid pusher는 이 column에서 모두 `Fixed`다. `Online`은 action authority를 뜻하며 feedback adaptation의 효과가 검증됐다는 의미는 아니다.

`Manipulation=Combined`는 한 논문이 translation과 reorientation을 모두 포함한다는 **paper-level coverage**를 뜻한다. 두 동작을 하나의 sequential task로 연결하거나 동일한 episode objective로 최적화했다는 뜻은 아니다.

`Translation`과 `Reorientation`은 goal pose의 좌표 수가 아니라 논문이 다루는 manipulation primitive를 기준으로 판정한다. 예를 들어 planar pushing이 orientation error까지 제어하더라도 별도의 pivot·rotation skill을 다루지 않으면 `Translation`이다.

`Method`는 논문에 포함된 모든 module을 나열하지 않고 **주된 action-generation family** 하나를 기록한다. Designed feedback law가 action을 직접 정하면 `Control`, explicit objective와 constraint를 푸는 online solver가 motion을 정하면 `Optimization`, interaction return으로 policy를 최적화하면 `RL`, non-VLA policy가 demonstration trajectory를 직접 학습하면 `IL`, learned model이 pose/action 후보를 생성하고 simulation이나 planner가 선택·실행하면 `Generative`, pretrained vision–language model 기반 action policy이면 `VLA`다. 따라서 optimization demonstration으로 RL을 유도한 B81은 `RL`, demonstration과 low-level controller를 함께 사용하는 B48·B99는 `VLA`로 분류하며, 보조 module 때문에 별도의 `Hybrid` 값을 만들지 않는다.

현재 Stage 1은 manipulation 대상 blocker의 pose와 geometry를 지속적으로 관측할 수 있다고 가정하므로 observability는 비교 column으로 두지 않는다. 현재 가까운 연구도 주어진 goal 이후에는 자율 실행하므로 autonomy 역시 비교 column에서 제외한다. Human demonstration은 training source이지 실행 중 manual control을 뜻하지 않는다.

---

## 2. 비교표

| Work | Scene | Workspace | Sensing | Object Geometry | End-effector Reconfiguration | Manipulation | Method |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [B37 · Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) | Uncluttered | Open | Contact | None | Fixed | Translation | Control |
| [B84 · Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) | Cluttered | Open | Vision+Contact | None | Fixed | Translation | RL |
| [B85 · Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) | Uncluttered | Constrained | Vision+Contact | Estimated | Fixed | Reorientation | RL |
| [B12 · Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157) | Uncluttered | Open | Vision+Contact | None | Fixed | Translation | RL |
| [B92 · Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135) | Uncluttered | Open | Contact | Exact | Fixed | Combined | Optimization |
| [B90 · HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154) | Uncluttered | Mixed | Vision | Estimated | Fixed | Combined | RL |
| [B48 · Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) | Uncluttered | Mixed | Vision+Contact | None | Online | Combined | VLA |
| [B01 · GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Uncluttered | Open | Vision | Estimated | Pre-contact | Translation | Generative |
| [B22 · DexMove](https://openreview.net/forum?id=dT3ZciXvNX) | Uncluttered | Open | Vision+Contact | Estimated | Online | Combined | IL |
| [B81 · Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Uncluttered | Mixed | Vision+Contact | Estimated | Fixed | Combined | RL |
| [B99 · ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169) | Cluttered | Mixed | Vision+Contact | None | Fixed | Combined | VLA |
| **Ours** | **Uncluttered** | **Constrained** | **Vision+Contact** | **Estimated** | **Online** | **Combined** | **RL** |

B01의 raw sensing source와 geometry 정확도, B22의 manipulation 범위, B48의 전체 task suite 범위는 full text에서 우선 재확인한다. 표의 값은 현재 파악한 범위에서 가장 가까운 공통 분류다.

B99의 `Cluttered`는 five-task suite 중 Retrieve Plate가 movable foam-ball clutter를 포함한다는 capability-level 판정이며, 다른 task는 대부분 주변 movable clutter가 없는 scene이다. Method는 end-effector pose뿐 아니라 force target과 subtask transition을 생성하는 주된 action architecture를 기준으로 `VLA`로 분류한다. Hybrid force–position controller는 그 출력을 실행하는 핵심 layer지만 별도의 Method 값으로 세지 않는다. AG-95 adaptive gripper를 장착했지만 보고된 learned action에는 gripper-width나 finger-joint command가 확인되지 않으므로 `End-effector Reconfiguration=Fixed`로 판정한다. 후속 원문에서 내부 configuration command가 확인되면 이 cell을 다시 검토한다.

B99는 B46의 단순 개정판이 아니라 새 dataset과 architecture를 사용한 별도 논문이다. 다만 공통 저자, ForceVLA의 한계를 출발점으로 한 문제 설정과 직접 baseline 비교를 근거로 **같은 계보의 method successor**로 판정해 이 비교 집합에서는 B46을 대체했다. B46은 [Paper Index](../papers/core_papers.md)의 선행 계보 항목으로 유지한다.

### Ours를 읽는 기준

- `Uncluttered`: 현재 S0에는 manipulation 대상 blocker 외에 동작에 영향을 주는 movable object가 없다.
- `Constrained`: shelf가 접근·회전·병진 공간을 제한한다.
- `Vision+Contact`: continuous vision-derived pose와 OBB, tactile, wrist F/T를 함께 사용한다.
- `Estimated`: robot agent는 exact mesh가 아니라 estimated OBB를 받는다.
- `Online`: policy가 접촉 이후에도 wrist action과 함께 finger-joint configuration을 갱신한다. 그 추가 이득은 C2에서 검증할 대상이다.
- `Combined`: 필요한 reorientation/pivoting과 이후 translation을 모두 다룬다.
- `RL`: Stage 1 policy는 interaction return으로 최적화한다. Scripted heuristic은 효과를 비교하기 위한 별도 baseline이다.

---

## 3. Comparison-set Timeline

아래 timeline은 nonprehensile manipulation 전체의 역사가 아니라, 2절 비교표에 선정된 **11편만** 연도순으로 재배열한 것이다. `Ours`는 제안 시스템이므로 제외한다. 연도는 conference edition 또는 journal issue를 기준으로 하며, online publication이나 proceedings 수록 연도가 다르면 함께 표시한다.

| 연도 | 비교 연구와 method concept | 비교 집합에서 읽히는 변화 |
| --- | --- | --- |
| **2022** | [B37 · Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) `[Tactile feedback control; online 2021]`<br>[B84 · Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) `[RL]` | Translation에서 tactile feedback law로 접촉 오차를 직접 보정하는 방식과, goal progress·contact 유지·collision avoidance를 interaction return으로 학습하는 방식이 병렬적으로 나타났다. |
| **2023** | [B85 · Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) `[Geometry-conditioned RL]` | RL의 범위가 translation에서 environment-contact pivoting으로 확장되고, depth-derived object feature와 state/action projection으로 unseen-object transfer를 다뤘다. |
| **2024** | [B12 · Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157) `[Visuotactile estimation + RL; CoRL 2024, PMLR 2025]`<br>[B92 · Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135) `[Tactile object/contact-state estimation + prescribed-mode optimization/control]` | Contact uncertainty를 estimator와 controller의 결합 문제로 명시했다. 한쪽은 occlusion 아래 state uncertainty를 learned policy에 전달하고, 다른 쪽은 tactile로 grasped-object pose와 extrinsic contact location을 추정해 주어진 contact mode 안에서 optimization과 feedback control을 수행한다. |
| **2025** | [B90 · HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154) `[Modular RL]`<br>[B48 · Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) `[Tactile VLA + position–force control; Adjacent]` | Learning은 object·environment geometry를 활용하는 modular policy와 tactile-conditioned VLA로 확장됐다. 동시에 learned action generation과 embodiment-specific position–force control의 결합이 필요함을 보여준다. |
| **2026** | [B01 · GD2P](https://doi.org/10.48550/arXiv.2509.18455) `[Generative hand-pose model + simulation/planning]`<br>[B22 · DexMove](https://openreview.net/forum?id=dT3ZciXvNX) `[Tactile flow-based IL]`<br>[B81 · Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) `[Optimization demonstrations + RL]`<br>[B99 · ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169) `[Force-aware VLA + hybrid force–position control; Adjacent]` | 역할 분화가 더 명시적이 됐다. 생성·optimization은 initial configuration과 feasible prior를, IL/RL은 closed-loop execution을 담당한다. ForceVLA2는 force를 입력 cue로만 쓰는 수준에서 나아가 force target과 control mode를 action으로 생성해 active hybrid force–position regulation에 연결한다. |

이 timeline은 `model-based method가 learning으로 대체됐다`는 흐름을 뜻하지 않는다. Contact feedback과 online correction은 non-learning control, RL, IL와 VLA 모두에서 나타나며, 최근 변화는 각 family가 initial configuration, state estimation, feasibility와 feedback execution의 역할을 나누는 방향에 가깝다.

---

## 4. 비교에서 보이는 범위

현재 표에서 Ours와 모든 column이 같은 연구는 없다.

- B01과 B22는 `Estimated` object geometry를 사용하는 가까운 비교 연구다. B01은 task-conditioned configuration을 접촉 전에 선택하는 `Pre-contact`, B22는 tactile wrist–finger configuration을 조작 중 갱신하는 `Online`에 해당한다.
- B85는 Ours와 Scene, Workspace, Sensing, Object Geometry와 Method가 같지만 `Manipulation`과 `End-effector Reconfiguration`이 다르다. Wall-assisted pivoting과 unseen-object transfer를 보여주지만 reorientation만 다루며, 후속 translation과 online finger adaptation은 포함하지 않는다.
- B81은 estimated geometry와 vision·contact feedback을 사용해 translation과 reorientation을 모두 다루지만, optimization-generated demonstration으로 유도한 `RL`이며 workspace 범위도 다르다.
- B92는 open planar workspace에서 translation과 reorientation을 모두 다루지만, known object geometry와 prescribed contact mode를 가정한 `Optimization` method를 사용한다.
- B99와 B48은 vision·contact-conditioned VLA를 hybrid force–position control과 결합할 수 있음을 보여주지만, 여러 contact-rich task를 다루는 인접 연구다. B48의 `Online`은 gripper width를 통한 internal grasp-force 조절이고, Ours의 multi-finger contact-interface reconfiguration과 동일한 action authority를 뜻하지 않는다. B99는 force target과 subtask transition까지 action output에 포함하지만 보고된 gripper configuration command가 없어 `Fixed`다.

이 차이는 연구 질문을 좁히는 근거이지 novelty의 증거는 아니다. 특히 `Vision+Contact`, `Estimated` geometry 또는 `Combined` manipulation이라는 조합만으로 contribution을 주장하지 않는다.

---

## 5. 남은 질문

1. **C1:** Estimated geometry에 오차가 있을 때 contact feedback이 Rotation-to-Push와 final-task 성능 저하를 줄이는가?
2. **C2:** Task-conditioned pre-contact formation 이후 contact 중 online wrist–finger adaptation이 추가 이득을 주는가?

직접 수치 비교에서는 같은 Scene, Workspace, Sensing과 Object Geometry 조건을 우선 맞춘다. End-effector Reconfiguration 자체가 비교 요인인 C2에서는 hardware·task·controller를 고정한 채 `Fixed / Pre-contact / Online`만 바꾸는 matched ablation을 사용한다. 조건이 다른 논문은 특정 요소의 근거나 인접 연구의 반례로 사용하고, 원 논문의 success rate를 Ours와 그대로 대조하지 않는다.

각 cell은 full text에서 확인한다. 현재 근거가 부족한 분류는 후속 원문 검토에서 수정하며, 세부 observation·action·training source와 결과는 [Paper Index](../papers/README.md)와 각 독서 문서에 남긴다.
