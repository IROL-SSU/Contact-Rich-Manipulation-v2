# Research Motivation — From Application Need to Research Question

> [Intro](./README.md) · [Research Trend](./research_trend.md) · [Previous Works](./previous_works.md) · [Candidate Contributions](./contributions.md)
>
> **문서 역할:** Application expansion에서 nonprehensile manipulation의 필요성과 approximate geometry 아래 contact uncertainty까지 논리를 좁힌다. 특정 method의 우열이나 contribution은 이 문서에서 결론내리지 않는다.

---

## 1. Application expansion

Robot은 제조 설비를 넘어 home service, logistics와 retail shop으로 적용 범위를 넓히고 있다. 이 환경의 작업은 사전에 정확히 정렬된 한 종류의 물체를 반복해서 다루는 문제와 다르다. Robot은 크기와 형상이 다양한 물체를 제한된 공간에서 발견하고, 접근하고, 재배치해야 하며, 다른 물체나 환경 구조가 목표 동작을 방해할 수 있다.

따라서 manipulation은 보이는 target을 곧바로 grasp하는 동작만으로 끝나지 않는다. Target이 다른 물체에 가려져 있거나 hand의 접근 경로가 막혀 있다면, robot은 먼저 주변 물체를 옮겨 **시야, hand clearance 또는 조작 방향**을 확보해야 한다.

## 2. Why nonprehensile manipulation?

Prehensile manipulation은 물체를 안정적으로 구속할 수 있지만 feasible grasp, 손가락 배치 공간과 lifting clearance를 요구한다. Full grasp가 어렵거나 불필요한 재배치에서는 pushing, pulling, sliding과 pivoting 같은 nonprehensile action이 더 직접적일 수 있다.

본 연구에서 `nonprehensile manipulation`은 task category다. 이는 grasping을 대체하는 보편적 해법이 아니라, 다음 상황을 보완하는 manipulation capability다.

- Target에 접근하기 전에 blocker를 짧은 거리만 재배치하면 되는 경우
- 물체 뒤쪽으로 손가락을 배치할 공간이 부족한 경우
- Stable grasp나 lifting이 주변 구조물과의 충돌 위험을 높이는 경우
- Grasp를 형성하기 전에 물체 orientation이나 접근 가능한 면을 바꿔야 하는 경우

## 3. Why is contact the central challenge?

Nonprehensile action은 안정적인 grasp 없이 접촉을 통해 물체 운동을 만든다. 따라서 같은 nominal motion이라도 contact location, local geometry, friction, mass distribution과 `stick–slip–separation` 전이에 따라 결과가 달라진다.

여기서 `contact-rich manipulation`은 별도의 task category가 아니라 nonprehensile manipulation을 어렵게 만드는 **interaction/control challenge**다.

Vision과 coarse geometry는 object pose, 접근 방향과 nominal contact face 같은 전역 정보를 제공할 수 있다. 그러나 occlusion, tracking error와 shape approximation 때문에 실제 contact onset, local surface mismatch와 force transmission까지 정확히 알려주지는 못한다.

| Information source | 주된 역할 | 단독으로 알기 어려운 정보 |
| --- | --- | --- |
| Vision / approximate geometry | 전역 task, object pose, 접근 관계, nominal contact face | 실제 contact onset, local mismatch, slip, 전달 force |
| Binary tactile | Sensor region별 contact onset, loss와 migration | Contact force의 연속 크기와 전체 wrench |
| Wrist F/T | Hand–environment system의 net force/torque와 load 변화 | 개별 contact 위치와 sensor별 force 분포 |
| Proprioception | Arm/hand configuration과 command 결과 | 외부 contact state와 hidden physical properties |

## 4. Broad problem에서 현재 연구 대상으로 좁히기

본 연구는 위 broader problem 가운데 다음 instance를 다룬다.

- 선반 안의 target object에 접근하기 위해 앞쪽 blocker를 이동한다.
- Blocker 선택과 전역 작업 순서는 상위 module이 제공한다.
- Low-level policy는 `Approach / Contact Formation → Rotation / Pivoting → Push / Translation`을 수행한다.
- Rotation은 항상 필요한 고정 동작이 아니다. Direct push가 불안정하거나 desired push direction에 부적합한 contact/object orientation일 때 preparatory action으로 사용한다.
- Shelf와 비표적 물체의 접촉은 기본적으로 피하되, 주변 물체의 존재와 randomization 범위는 아직 open decision이다.

핵심 질문은 단순히 접촉하는 방법이 아니다.

> **Approximate geometry만 주어진 조건에서 contact feedback을 이용해 실제 geometry/contact mismatch를 보완하고, 이후 Rotation과 Push에 유효한 hand/contact state 및 wrist–finger motion을 형성·조정할 수 있는가?**

## 5. Method 선택을 검토해야 하는 이유

이제 문제는 정의되었지만 해결 방법은 아직 정해지지 않았다. Conventional approach를 하나의 약한 방법으로 묶어 배제해서는 안 된다.

- **Task-specific heuristic**은 계산이 빠르고 구현 의도가 명확하지만, object shape·friction·contact sequence가 바뀔 때 규칙과 threshold를 다시 설계해야 할 수 있다.
- **Model-based planning/control**은 physical constraint를 명시하고 failure 원인을 해석하기 좋지만, geometry·friction·contact-mode model의 정확도에 민감하며 contact sequence와 clutter가 늘수록 search와 modeling 비용이 커질 수 있다.
- 두 접근 모두 잘 정의된 model과 범위에서는 강력한 baseline이며 safety layer 또는 demonstration generator로 활용할 수 있다.

[Physics-Based Adaptive Motion Primitives](https://doi.org/10.1109/ICRA48506.2021.9561221)처럼 physics simulation과 search를 결합한 방법과 [Tactile-Driven Contact Mode Control](https://doi.org/10.15607/RSS.2024.XX.135)처럼 contact mode를 명시적으로 최적화한 방법은 conventional method의 강점을 보여준다. 문제는 이러한 방법이 무효라는 것이 아니라, **approximate geometry 아래에서 다양한 contact outcome과 recovery를 모두 사전에 열거하고 모델링하기 어렵다**는 점이다.

Learning-based method도 자동으로 해답이 되는 것은 아니다. RL은 reward·simulation transfer 문제를, IL은 demonstration coverage 문제를, VLA는 data·compute budget과 low-level contact precision 문제를 각각 가진다. 따라서 Motivation 단계의 결론은 특정 method의 선택이 아니라 다음 질문이다.

> **Heuristic/control, planning, RL, IL와 VLA는 contact-feedback manipulation을 어떻게 확장해 왔으며, 현재의 model·data·interaction 조건에서는 어떤 framework를 우선해야 하는가?**

이 질문은 다음 문서인 [Research Trend](./research_trend.md)에서 다룬다. Method가 선택된 뒤에야 [Previous Works](./previous_works.md)에서 가까운 system과 남은 research gap을 비교한다.
