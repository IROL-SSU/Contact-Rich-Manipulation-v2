# Closest Previous Works — Timeline and Environment–Robot Agent–System Comparison

> [Intro](./README.md) · [Research Motivation](./research_motivation.md) · [Research Trend](./research_trend.md) · [Candidate Contributions](./contributions.md) · [Paper Index](../papers/README.md)
>
> **문서 역할:** 가까운 nonprehensile manipulation 연구를 소수의 공통 기준으로 분류하고, 동일한 비교 집합의 timeline을 통해 method concept의 변화를 확인한 뒤 Ours와의 차이 및 C1·C2에 남는 질문을 정리한다.
>
> **주의:** 여기서 Agent는 AI agent가 아니라 **환경과 물리적으로 상호작용하는 robot agent**를 뜻한다. 표의 분류가 다르다는 사실만으로 research gap이나 contribution을 선언하지 않는다.

---

## 1. Column 기준

비교표에는 Environment, Robot Agent와 System에 해당하는 다음 여덟 column만 사용한다. 논문마다 서로 다른 설명을 넣기보다 각 column의 정해진 값 중 하나로 분류한다.

| 구분 | Column | 사용하는 값 | 판정 기준 |
| --- | --- | --- | --- |
| **Environment** | **물체 형상 (Object Configuration)** | `Structured` / `Unstructured` | 조작 대상 물체가 고정·정형 형상군에 한정되는가, 다양한 비정형 형상을 다루는가? |
| **Environment** | **주변 물체 (Surrounding Objects)** | `Absent` / `Present` | 조작 대상 외의 movable object가 함께 존재해 접근·시야·이동 경로 또는 접촉에 영향을 주는가? |
| **Robot Agent** | **감각 입력 (Sensory Input)** | `Vision` / `Contact` / `Vision+Contact` | 실행에 vision과 tactile·contact state·wrist F/T 중 무엇을 사용하는가? |
| **Robot Agent** | **도구 (Tool)** | `Pusher` / `Gripper` / `Dexterous Hand` | 물체와 직접 접촉하는 물리적 도구는 무엇인가? |
| **Robot Agent** | **접촉 구성 (Contact Configuration)** | `Constant` / `Pre-contact` / `Online` | 도구의 aperture·finger posture 같은 내부 구성을 언제 결정·갱신하는가? |
| **Robot Agent** | **물체 조작 (Object Manipulation)** | `Translation` / `Reorientation` / `Combined` | 대상 물체의 이동, 회전 또는 둘 모두를 다루는가? |
| **System** | **물체 형상 정보 (Object Geometry)** | `None` / `Estimated` / `Exact` | 방법이 명시적 물체 형상을 어떤 정확도로 사용하는가? |
| **System** | **방법 (Method)** | `Planning/Control` / `RL` / `IL` / `VLA` | 배포 시 task-level action을 정하는 주된 방법은 무엇인가? |

`Object Configuration`은 물체의 pose나 배치가 아니라 **조작 대상의 물리적 형상 범위**다. 고정 물체나 box·cylinder·정해진 polygon처럼 제한된 형상군이면 `Structured`, 서로 다른 일상 물체와 irregular shape까지 명시적으로 다루면 `Unstructured`다. 이는 형상 범위의 분류이며 unseen-object generalization이 검증됐다는 뜻은 아니다.

`Surrounding Objects=Present`는 조작 대상이 아닌 movable object가 같은 장면에 있고 manipulation에 실제 제약을 줄 때만 사용한다. Table·shelf·wall 같은 지지면과 고정 구조물, gripper가 쥔 task-essential tool, 여러 물체를 한 번에 하나씩 따로 시험한 경우는 세지 않는다. `Mixed`는 두지 않으며, 두 조건을 모두 보고한 논문은 주변 물체가 있는 최대 deployed capability를 기준으로 `Present`로 표시한다.

`Sensory Input`은 외부 환경을 읽는 감각 channel만 기록한다. Scene camera의 RGB·depth·RGB-D는 `Vision`, optical tactile image를 포함한 tactile·binary contact·wrist F/T는 `Contact`에 포함하고, 대부분 공통인 proprioception과 goal·geometry representation은 생략한다.

`Tool`은 실제 접촉 장치를 나타낸다. 능동 내부 자유도가 없는 접촉 tip은 `Pusher`, 주로 하나의 aperture로 연동되는 fingers는 `Gripper`, 여러 finger joint를 독립적으로 구동하면 `Dexterous Hand`다. 닫힌 gripper를 pusher처럼 사용해도 물리적 도구는 `Gripper`이며, 내부 자유도를 실제로 활용하는지는 `Contact Configuration`에서 별도로 읽는다.

`Contact Configuration`은 deployed method가 내부 contact configuration을 갱신하는 **시점과 권한**을 나타낸다. Task·object와 무관하게 같은 구성을 실행 내내 유지하면 `Constant`, 첫 manipulation contact 전에 task·object별 구성을 선택한 뒤 실행 중 유지하면 `Pre-contact`, autonomous execution 중 다시 갱신할 수 있으면 `Online`이다. `Online`에는 feedback 기반 연속 조정과 일시적 contact separation 뒤의 retry·re-contact·re-grasp가 모두 포함된다. Wrist/EEF pose, arm motion으로 인한 contact-point 이동, force target 변경과 passive compliance는 포함하지 않는다. 판정 단위는 learned action vector 하나가 아니라 autonomous deployment stack 전체이며, update trigger나 command source가 보고되지 않으면 근거 주석으로 남긴다.

`Object Manipulation`은 robot motion이 아니라 **대상 물체의 운동**을 분류한다. Push·pull·slide만 다루면 `Translation`, pivot·rotation이 중심이면 `Reorientation`, 둘을 모두 보고하면 `Combined`다. Planar pushing 중 작은 yaw error를 보정하는 것만으로는 `Reorientation`으로 세지 않으며, `Combined`도 두 동작을 하나의 episode objective로 연결했다는 뜻은 아니다.

`Object Geometry`는 System이 실행에 사용하는 **명시적 형상 표현의 정확도**다. RGB·depth 영상을 구조화된 형상으로 만들지 않고 직접 encode하면 `None`, sensor로 추정한 OBB·point cloud·shape feature는 `Estimated`, 등록된 CAD·mesh 또는 알려진 정확한 dimension은 `Exact`다. Simulator나 training에서만 exact geometry를 사용하고 deployed system이 쓰지 않으면 `Exact`로 세지 않는다.

`Method`는 보조 module을 나열하지 않고 deployed task-level decision을 맡는 family 하나를 기록한다. Scripted heuristic, 명시적 controller·optimizer·planner 또는 generative proposal을 simulation/planning으로 선택하는 stack은 `Planning/Control`, interaction return으로 policy를 최적화하면 `RL`, non-VLA policy가 demonstration trajectory를 직접 학습하면 `IL`, pretrained vision–language backbone 기반 action policy이면 `VLA`다. 따라서 B01의 diffusion proposal+simulation/planning은 `Planning/Control`, optimization demonstration으로 policy를 유도한 B81은 `RL`, low-level controller를 함께 쓰는 B48·B99는 `VLA`다.

현재 Stage 1은 manipulation 대상 blocker의 pose와 geometry를 지속적으로 관측할 수 있다고 가정하므로 observability는 비교 column으로 두지 않는다. 현재 가까운 연구도 주어진 goal 이후에는 자율 실행하므로 autonomy 역시 비교 column에서 제외한다. Human demonstration은 training source이지 실행 중 manual control을 뜻하지 않는다.

---

## 2. 비교표

| Work | Object Configuration | Surrounding Objects | Sensory Input | Tool | Contact Configuration | Object Manipulation | Object Geometry | Method |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [B37 · Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) | Unstructured | Absent | Contact | Pusher | Constant | Translation | None | Planning/Control |
| [B84 · Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) | Unstructured | Present | Vision+Contact | Gripper | Constant | Translation | Exact | RL |
| [B85 · Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) | Unstructured | Absent | Vision+Contact | Gripper | Constant | Reorientation | Estimated | RL |
| [B12 · Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157) | Structured | Absent | Vision+Contact | Pusher | Constant | Translation | None | RL |
| [B92 · Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135) | Structured | Absent | Contact | Gripper | Constant | Combined | Exact | Planning/Control |
| [B90 · HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154) | Unstructured | Absent | Vision | Gripper | Constant | Combined | Exact | RL |
| [B48 · Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) | Unstructured | Absent | Vision+Contact | Gripper | Online | Translation | None | VLA |
| [B01 · GD2P](https://doi.org/10.48550/arXiv.2509.18455) | Unstructured | Present | Vision | Dexterous Hand | Pre-contact | Translation | Estimated | Planning/Control |
| [B22 · DexMove](https://openreview.net/forum?id=dT3ZciXvNX) | Unstructured | Present | Vision+Contact | Dexterous Hand | Online | Combined | Estimated | IL |
| [B81 · Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) | Unstructured | Absent | Vision+Contact | Pusher | Constant | Combined | Estimated | RL |
| [B99 · ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169) | Unstructured | Present | Vision+Contact | Gripper | Online | Combined | None | VLA |
| **Ours** | **Unstructured** | **Present** | **Vision+Contact** | **Dexterous Hand** | **Online** | **Combined** | **Estimated** | **RL** |

B84는 system-level path planner가 알려진 target diameter를 사용하므로 `Object Geometry=Exact`, B90은 등록 mesh에서 sampling한 object point cloud를 사용하므로 `Exact`로 판정했다. 반면 B37은 tactile image만 사용하고 B48·B99는 scene RGB·depth를 구조화된 형상으로 만들지 않고 직접 encode하므로 `None`이다. B22의 `Present`는 object sorting·desktop tidying까지 포함한 reported task-suite capability를 기준으로 하며, 모든 rollout에서 주변 물체가 방해물이라는 뜻은 아니다. B48은 gripper motion이 아니라 대상 물체 운동만 판정하면 별도 reorientation task가 확인되지 않아 `Translation`이다.

B99의 `Present`는 five-task suite 중 Retrieve Plate가 movable foam-ball clutter를 포함하기 때문이다. Method는 force target과 subtask transition을 만드는 주된 action architecture를 기준으로 `VLA`다. Retrieve Plate에서 autonomous retry/re-grasp가 보고되므로 deployed-system capability는 `Contact Configuration=Online`으로 판정한다. 다만 formal learned action에는 gripper-aperture command가 없으므로, 해당 command가 VLA·subtask script·별도 controller 중 어디에서 생성되는지는 `미보고`로 남긴다.

B99는 B46의 단순 개정판이 아니라 새 dataset과 architecture를 사용한 별도 논문이다. 다만 공통 저자, ForceVLA의 한계를 출발점으로 한 문제 설정과 직접 baseline 비교를 근거로 **같은 계보의 method successor**로 판정해 이 비교 집합에서는 B46을 대체했다. B46은 [Paper Index](../papers/core_papers.md)의 선행 계보 항목으로 유지한다.

### Ours를 읽는 기준

- `Unstructured`: intended shelf task는 다양한 blocker 형상을 대상으로 한다. 단, 정확한 train/test object 범위와 non-box-like generalization은 아직 OD-10에서 확정할 사항이다.
- `Present`: 실제 shelf setting에는 target 외의 movable object가 있다. 주변 물체를 제거한 단순 조건은 `Absent`로 분류할 수 있지만, 이를 실제 experiment에 사용할지는 아직 정하지 않았다.
- `Vision+Contact`: continuous vision-derived pose·OBB와 tactile·wrist F/T를 함께 사용한다.
- `Dexterous Hand`: RH56E2의 여러 finger joint를 독립적으로 구동한다.
- `Online`: C2가 다루는 목표 system은 실행 중 feedback에 따라 finger-joint configuration을 갱신할 수 있는 방향으로 분류한다. Wrist motion은 이 판정에 포함하지 않으며, 구체 action/controller와 추가 이득의 판단 방식은 아직 협의 중이다.
- `Combined`: preparatory reorientation/pivoting과 이후 translation을 모두 다룬다.
- `Estimated`: System은 exact mesh가 아니라 perception-derived OBB를 사용한다.
- `RL`: Intro의 조건부 method-family 판단에 따라 Stage 1의 주된 학습 방식을 RL로 분류한다. 실제 비교군은 아직 정하지 않았다.

---

## 3. Comparison-set Timeline

아래 timeline은 nonprehensile manipulation 전체의 역사가 아니라, 2절 비교표에 선정된 **11편만** 연도순으로 재배열한 것이다. `Ours`는 제안 시스템이므로 제외한다. 연도는 conference edition 또는 journal issue를 기준으로 하며, online publication이나 proceedings 수록 연도가 다르면 함께 표시한다.

| 연도 | 비교 연구와 method concept | 비교 집합에서 읽히는 변화 |
| --- | --- | --- |
| **2022** | [B37 · Goal-Driven Robotic Pushing](https://doi.org/10.1109/TRO.2021.3104471) `[Planning/Control: tactile feedback; online 2021]`<br>[B84 · Goal-Oriented Pushing in Clutter](https://doi.org/10.1109/IROS47612.2022.9981873) `[RL]` | Translation에서 tactile feedback law로 접촉 오차를 직접 보정하는 방식과, goal progress·contact 유지·collision avoidance를 interaction return으로 학습하는 방식이 병렬적으로 나타났다. |
| **2023** | [B85 · Learning Generalizable Pivoting Skills](https://doi.org/10.1109/ICRA48891.2023.10161271) `[Geometry-conditioned RL]` | RL의 범위가 translation에서 environment-contact pivoting으로 확장되고, depth-derived object feature와 state/action projection으로 unseen-object transfer를 다뤘다. |
| **2024** | [B12 · Visuotactile Estimation under Occlusions](https://doi.org/10.48550/arXiv.2412.13157) `[Visuotactile estimation + RL; CoRL 2024, PMLR 2025]`<br>[B92 · Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135) `[Planning/Control: prescribed-mode optimization + feedback]` | Contact uncertainty를 estimator와 controller의 결합 문제로 명시했다. 한쪽은 occlusion 아래 state uncertainty를 learned policy에 전달하고, 다른 쪽은 tactile로 grasped-object pose와 extrinsic contact location을 추정해 주어진 contact mode 안에서 optimization과 feedback control을 수행한다. |
| **2025** | [B90 · HAMNET](https://doi.org/10.15607/RSS.2025.XXI.154) `[Modular RL]`<br>[B48 · Tactile-VLA](https://doi.org/10.48550/arXiv.2507.09160) `[Tactile VLA + position–force control; Adjacent]` | Learning은 object·environment geometry를 활용하는 modular policy와 tactile-conditioned VLA로 확장됐다. 동시에 learned action generation과 embodiment-specific position–force control의 결합이 필요함을 보여준다. |
| **2026** | [B01 · GD2P](https://doi.org/10.48550/arXiv.2509.18455) `[Planning/Control: generative proposal + simulation/planning]`<br>[B22 · DexMove](https://openreview.net/forum?id=dT3ZciXvNX) `[Tactile flow-based IL]`<br>[B81 · Optimization-Guided Non-Prehensile RL](https://doi.org/10.1109/LRA.2026.3655262) `[Optimization demonstrations + RL]`<br>[B99 · ForceVLA2](https://doi.org/10.48550/arXiv.2603.15169) `[Force-aware VLA + hybrid force–position control; Adjacent]` | 역할 분화가 더 명시적이 됐다. Planning/Control의 generative·optimization module은 initial configuration과 feasible prior를 제공하고, IL/RL/VLA stack은 closed-loop execution으로 확장된다. ForceVLA2는 force target과 control mode를 action으로 생성해 active hybrid force–position regulation에 연결한다. |

이 timeline은 `Planning/Control이 learning으로 대체됐다`는 흐름을 뜻하지 않는다. Contact feedback과 online correction은 `Planning/Control`, `RL`, `IL`, `VLA` 모두에서 나타나며, 최근 변화는 각 family가 initial configuration, state estimation, feasibility와 feedback execution의 역할을 나누는 방향에 가깝다.

---

## 4. 비교에서 보이는 범위

현재 표에서 Ours와 모든 column이 같은 연구는 없다.

- B22는 Ours와 동일하게 `Unstructured`, `Present`, `Vision+Contact`, `Dexterous Hand`, `Online`, `Combined`, `Estimated`에 해당하지만 주된 method가 `IL`이다. 가장 가까운 구성이라도 Ours의 RL 선택이나 C1·C2를 자동으로 정당화하지는 않는다.
- B01은 `Dexterous Hand`와 `Estimated` geometry를 사용하지만, task-conditioned hand pose를 실행 전에 고르는 `Pre-contact`이며 generative proposal을 simulation/planning으로 선택하는 `Planning/Control`이다.
- B85는 estimated geometry와 vision·contact feedback을 사용하는 `RL`이지만 `Gripper+Constant`로 wall-assisted reorientation만 다룬다. 후속 translation과 online finger adaptation은 포함하지 않는다.
- B81은 estimated geometry와 vision·contact feedback으로 translation과 reorientation을 모두 다루지만 `Pusher+Constant`이며, 주변 movable object가 없는 조건에서 optimization-generated demonstration으로 RL을 유도한다.
- B92는 known geometry와 prescribed contact mode를 사용하는 `Planning/Control`이며, `Gripper+Constant`로 translation과 reorientation을 다룬다.
- B48·B99는 `Gripper+Online`과 vision·contact-conditioned VLA의 강한 인접 반례다. B48의 `Online`은 gripper-width force regulation이고, B99의 `Online`은 reported retry/re-grasp capability다. 둘 다 Ours의 continuous multi-joint dexterous-hand update와 같은 action authority를 뜻하지 않는다.

이 차이는 연구 질문을 좁히는 근거이지 novelty의 증거는 아니다. 특히 `Vision+Contact`, `Estimated` geometry 또는 `Combined` manipulation이라는 조합만으로 contribution을 주장하지 않는다.

---

## 5. 남은 질문

1. **C1:** Estimated geometry에 오차가 있을 때 contact feedback이 Rotation-to-Push와 final-task 성능 저하를 줄이는가?
2. **C2:** Task-conditioned pre-contact formation 이후 contact 중 online wrist–finger adaptation이 추가 이득을 주는가?

향후 수치 비교를 논의한다면 `Object Configuration`, `Surrounding Objects`, `Sensory Input`, `Tool`과 `Object Geometry`의 차이를 먼저 확인해야 한다. `Contact Configuration`을 어떤 조건에서 비교할지, 실제 비교군과 통제 방식은 아직 정하지 않았다. 조건이 다른 논문의 success rate를 Ours와 그대로 대조하지 않는다는 해석 원칙만 유지한다.

각 cell은 full text에서 확인한다. 현재 근거가 부족한 분류는 후속 원문 검토에서 수정하며, 세부 observation·action·training source와 결과는 [Paper Index](../papers/README.md)와 각 독서 문서에 남긴다.
