# Conference Screening Index

> [Paper Index](../README.md) · [ICRA 연도별](./icra.md) · [IROS 연도별](./iros.md)

이 디렉터리는 핵심 논문을 설명하는 본문이 아니라 **새 후보를 빠짐없이 찾기 위한 screening appendix**다. 먼저 아래 선정 범위와 근거 수준을 확인한 뒤, 연구 질문에 맞는 주제 페이지로 이동한다. 연도별 목록은 특정 conference paper를 찾을 때만 사용한다.

## 검토 범위와 선정 기준

`docs/ICRA&IROS`의 10개 CSV에 수록된 **2,050편 전부**를 제목·초록·Author Keywords·IEEE Terms 기준으로 확인했다. 모든 논문이 reinforcement learning 검색 결과에 포함되었더라도, 아래 목록에는 Track B의 현재 문제 또는 문서화된 설계 미결 항목에 구체적인 근거를 주는 논문만 남겼다.

| 학회 | 2021 | 2022 | 2023 | 2024 | 2025 | 검토 합계 | 선정 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| ICRA | 186 | 149 | 190 | 263 | 238 | 1,026 | 80 |
| IROS | 155 | 169 | 181 | 231 | 288 | 1,024 | 90 |
| **합계** | **341** | **318** | **371** | **494** | **526** | **2,050** | **170** |

선정된 170편 중 168편은 이 문서에 새로 추가한 논문이고, 2편은 기존 [B03](https://doi.org/10.1109/ICRA55743.2025.11127792)·[B33](https://doi.org/10.1109/ICRA57147.2024.10610532)과의 교차참조다. 일반 navigation·locomotion·driving·multi-agent RL 논문은 task 명칭이 비슷하다는 이유만으로 넣지 않았으며, 다음 중 하나 이상에 명확히 연결될 때만 포함했다.

- Blocker pushing, non-prehensile/dexterous contact manipulation, clutter clearing, target visibility·retrieval
- Vision·tactile·wrist F/T·proprioception의 observation, history, sensor fusion과 partial observability
- Actor–critic privileged-information 분리, contact state·force를 이용한 reward·evaluation
- Contact·tactile·vision·actuator simulation, scene generation, system identification, domain randomization과 Sim-to-Real
- EEF/action representation, force·impedance controller, reward, curriculum, phase transition과 skill chaining
- Collision·과부하·전도·failure recovery, safety supervisor와 장기 공간 확보 의사결정

등급은 `A = 현재 과업 또는 핵심 설계에 직접 대응`, `B = 학습환경·센서·제어·Sim-to-Real 등에 강한 방법론적 근거`, `C = 후속 Track B 2단계 또는 확장 설계 참고`다. 관점 태그는 `TASK`, `CONTACT`, `SENSE`, `OBS`, `SIM`, `ENV`, `SYSID`, `ACTION`, `GOAL`, `REWARD`, `LEARN`, `SAFE`, `SYSTEM`을 사용한다. `CSV ID`의 마지막 숫자는 해당 연도 CSV에서 header를 제외한 1-based row 번호이므로 원본 record를 바로 추적할 수 있다.

이 목록은 **초록 수준의 전수 relevance screening**이다. `A` 등급도 최종 baseline 선정이나 논문의 세부 claim 검증을 의미하지 않으며, 실제 설계에 인용하거나 구현하기 전에는 원문·공개 코드·센서 및 실험 조건을 다시 확인한다.


## 주제별 screening

아래 표는 5.2–5.3절의 170편을 현재 연구의 설계 질문에 따라 다시 배치한 상세 재분류다. 같은 논문이 여러 설계 질문에 답할 수 있으므로 중복을 허용한다. 각 행에는 논문 제목과 해당 그룹에 포함한 구체적인 근거를 함께 기록했다. ICRA와 IROS는 그룹마다 별도 표로 구분했으며, 모든 170편이 최소 한 그룹에 포함되어 있다.

| # | 그룹 | 편수 | 주 활용 지점 |
|---:|---|---:|---|
| 1 | Low-level Contact Manipulation | 76 | 1단계 method·baseline |
| 2 | Blocker·Clutter·Retrieval | 35 | 2단계 system 확장 |
| 3 | Tactile·Wrist F/T·Multimodal Sensing | 74 | Observation 설계 |
| 4 | Observation·History·POMDP·Privileged Information | 59 | Observation/critic 계약 |
| 5 | Simulation Environment·Sensor Virtualization·Dataset/Platform | 63 | 학습환경 구현 |
| 6 | Sim-to-Real·Domain Randomization·System Identification | 47 | Sim-to-Real 설계 |
| 7 | Action Space·Controller·Force/Compliance Control | 53 | Action contract |
| 8 | Reward·Curriculum·Exploration·Replay | 70 | Reward formulation |
| 9 | Phase Transition·Skill Chaining·Hierarchical Policy | 71 | 1→2단계 구조 |
| 10 | Safety·Constraint·Termination·Failure Recovery | 21 | 안전·evaluation |
| 11 | Geometry·Affordance·Contact Configuration Representation | 25 | Geometry/contact 표현 |


| 주제 묶음 | 페이지 |
| --- | --- |
| Low-level contact manipulation; blocker·clutter·retrieval | [Task and Retrieval](./task_and_retrieval.md) |
| Tactile·F/T; observation·history; simulation sensor·dataset | [Sensing and State](./sensing_and_state.md) |
| Sim-to-Real; action·controller; reward·curriculum | [Sim-to-Real and Learning](./sim2real_and_learning.md) |
| Phase transition; safety·termination; geometry·affordance | [Transitions, Safety and Geometry](./transitions_safety_geometry.md) |
