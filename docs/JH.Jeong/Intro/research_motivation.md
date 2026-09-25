# Research Motivation — From Application Need to Research Question

> [Intro](./README.md) · [Research Trend](./research_trend.md) · [Previous Works](./previous_works.md) · [Candidate Contributions](./contributions.md)
>
> **문서 역할:** Application expansion에서 nonprehensile manipulation의 필요성과 approximate geometry 아래 contact uncertainty까지 논리를 좁힌다. 특정 method의 우열이나 contribution은 이 문서에서 결론내리지 않는다.

---

## 1. 접근 공간 확보를 위한 nonprehensile manipulation

Robot의 적용 범위가 제조 설비에서 home service, logistics와 retail shop으로 넓어지면서, 제한된 공간에서 크기와 형상이 다양한 물체를 발견하고 재배치해야 하는 상황이 증가하고 있다. Target이 다른 물체에 가려져 있거나 hand의 접근 경로가 막혀 있다면, robot은 target을 곧바로 grasp하기 전에 주변 blocker를 옮겨 **시야, hand clearance 또는 조작 방향**을 확보해야 한다.

Prehensile manipulation은 물체를 안정적으로 구속할 수 있지만 feasible grasp, 손가락 배치 공간과 lifting clearance를 요구한다. Blocker를 짧은 거리만 옮기면 되거나 물체 뒤쪽의 finger placement와 lifting이 어렵고, grasp 이전에 orientation 또는 접근 가능한 면을 바꿔야 하는 경우에는 pushing, sliding과 pivoting 같은 nonprehensile action이 더 직접적일 수 있다.

따라서 본 연구에서 `nonprehensile manipulation`은 grasping을 대체하는 보편적 해법이 아니라, target 접근을 위해 주변 물체를 재배치하는 **task category**다.

## 2. Approximate geometry 아래의 contact uncertainty

Nonprehensile action은 안정적인 grasp 없이 접촉으로 물체 운동을 만든다. 같은 nominal motion도 contact location, local geometry, friction, mass distribution과 `stick–slip–separation` 전이에 따라 다른 결과를 낳는다. 여기서 `contact-rich manipulation`은 별도의 task category가 아니라 이러한 불확실성을 다루는 **interaction/control challenge**다.

Vision과 approximate geometry는 object pose, 접근 관계와 nominal contact face 같은 전역 정보를 제공하지만, occlusion, tracking error와 shape approximation 때문에 실제 contact onset, local surface mismatch와 force transmission까지 정확히 알려주지는 못한다. 각 정보원의 역할과 한계는 다음처럼 구분한다.

| Information source | 주된 역할 | 남는 불확실성 |
| --- | --- | --- |
| Vision / approximate geometry | 전역 task, object pose, 접근 관계, nominal contact face | 실제 contact onset, local mismatch, slip, 전달 force |
| Binary tactile | Sensor region별 contact onset, loss와 migration | Contact force의 연속 크기와 전체 wrench |
| Wrist F/T | Hand–environment system의 net force/torque와 load 변화 | 개별 contact 위치와 sensor별 force 분포 |
| Proprioception | Arm/hand configuration과 command 결과 | 외부 contact state와 hidden physical properties |

따라서 vision과 geometry는 nominal access를 정하고, tactile과 wrist F/T는 실행 중 드러나는 contact state와 geometry mismatch를 보완한다. 어느 한 modality가 다른 정보를 완전히 대체한다고 가정하지 않는다.

## 3. 현재 연구 대상과 범위

현재 다루는 작업은 선반 안의 target object에 접근하기 위해 앞쪽 blocker를 이동하는 것이다. 상위 module은 blocker와 task goal을 정하고, Stage 1 low-level policy는 안정적인 grasp나 lifting 없이 다음 과정을 수행한다.

```text
Approach / Contact Formation → Rotation / Pivoting → Push / Translation
```

Approach는 후속 조작에 사용할 contact state를 형성하고, Rotation은 direct push가 불안정하거나 현재 orientation이 desired push direction에 부적합할 때만 preparatory action으로 사용하며, Push는 blocker를 목표 방향과 거리로 이동한다. 세 항목은 sub-objective이지 독립 policy나 강제된 hard sequence가 아니므로 초기 상태가 적합하면 Rotation은 작거나 생략될 수 있다.

Target retrieval, multi-blocker ordering과 perception algorithm 자체는 이 low-level policy의 직접 범위가 아니다. Blocker–shelf support-surface contact는 pushing·pivoting에 필요한 dynamics다. Intended shelf condition에는 주변 movable object가 존재한다. 주변 물체를 제거한 단순 조건을 사용할지, 수·배치·관측 범위와 auxiliary fixed structure 또는 movable object와의 접촉을 금지·허용·이용할지는 아직 정하지 않았다. 세부 경계는 [`context.md`](../context.md)의 OD-1을 따른다.

핵심 질문은 단순히 접촉하는 방법이 아니다.

> **지속적인 vision에서 얻은 approximate geometry와 contact feedback을 함께 사용해 실제 geometry/contact mismatch를 보완하고, 이후 Rotation과 Push에 유효한 hand/contact state 및 wrist–finger motion을 형성·조정할 수 있는가?**

## 4. 방법 선정을 위한 질문

이 문제 정의만으로 특정 방법이 자동으로 선택되지는 않는다. Planning과 control은 모든 방법에 필요한 공통 기능이며, task-level decision knowledge를 사람이 설계한 model·mode·rule에서 얻는 Conventional methods, demonstration·pretrained prior에서 얻는 IL/VLA, interaction return에서 얻는 RL은 서로 다른 model·data·interaction 조건을 필요로 한다. 어느 하나를 약한 대안으로 전제하지 않고 현재 조건에서 감당할 수 있는 부담을 비교해야 한다.

> **Conventional methods, IL/VLA와 RL은 contact-feedback manipulation에 필요한 decision knowledge를 어디에서 얻으며, 현재의 model·data·interaction 조건에서는 어떤 방법을 우선해야 하는가?**

이 질문은 [Research Trend](./research_trend.md)에서 다룬다. 방법을 조건부로 선택한 뒤 [Previous Works](./previous_works.md)에서 Environment의 `Object Configuration·Surrounding Objects`, Robot Agent의 `Sensory Input·Tool·Contact Configuration·Object Manipulation`, System의 `Object Geometry·Method`를 비교하고 남은 research gap을 판정한다.
