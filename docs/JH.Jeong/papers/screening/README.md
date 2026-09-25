# Conference Screening Index

> [Paper Index](../README.md) · [ICRA 연도별](./icra.md) · [IROS 연도별](./iros.md)

이 디렉터리는 핵심 논문을 설명하는 본문이 아니라 **새 후보를 빠짐없이 찾기 위한 screening appendix**다. 먼저 아래 선정 범위와 근거 수준을 확인한 뒤, 연구 질문에 맞는 주제 페이지로 이동한다. 연도별 목록은 특정 conference paper를 찾을 때만 사용한다.

> **현재 범위:** 합의된 범위는 Intro와 actor observation까지다. 이 디렉터리에서 다루는 baseline, 비교, ablation, action/controller, reward, safety와 evaluation은 외부 논문이 보고한 내용이거나 이후 논의를 위한 문헌 후보다. 우리 연구에서 채택한 방법·비교군·실험 계획을 뜻하지 않는다.

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

등급은 `A = 현재 연구 질문과 가까운 논문`, `B = 학습환경·센서·제어·Sim-to-Real 등의 방법을 검토할 논문`, `C = 범위 확장 시 참고할 논문`을 뜻한다. 등급은 활용 우선순위나 채택 여부가 아니다. 관점 태그는 `TASK`, `CONTACT`, `SENSE`, `OBS`, `SIM`, `ENV`, `SYSID`, `ACTION`, `GOAL`, `REWARD`, `LEARN`, `SAFE`, `SYSTEM`을 사용한다. `CSV ID`의 마지막 숫자는 해당 연도 CSV에서 header를 제외한 1-based row 번호이므로 원본 record를 바로 추적할 수 있다.

이 목록은 **초록 수준의 전수 relevance screening**이다. `A` 등급도 최종 baseline 선정이나 논문의 세부 claim 검증을 의미하지 않으며, 실제 설계에 인용하거나 구현하기 전에는 원문·공개 코드·센서 및 실험 조건을 다시 확인한다.


## 주제별 screening

아래 표는 5.2–5.3절의 170편을 현재 연구의 문헌 검토 질문에 따라 다시 배치한 상세 재분류다. 같은 논문이 여러 질문과 관련될 수 있으므로 중복을 허용한다. 각 행에는 논문 제목과 해당 그룹에 포함한 구체적인 근거를 함께 기록했다. ICRA와 IROS는 그룹마다 별도 표로 구분했으며, 모든 170편이 최소 한 그룹에 포함되어 있다.

| # | 그룹 | 편수 | 문헌 검토 관점 |
|---:|---|---:|---|
| 1 | Low-level Contact Manipulation | 76 | 접촉 조작 방법 후보 |
| 2 | Blocker·Clutter·Retrieval | 35 | Blocker handling과 retrieval 문제 |
| 3 | Tactile·Wrist F/T·Multimodal Sensing | 74 | 센서 구성과 융합 방식 |
| 4 | Observation·History·POMDP·Privileged Information | 59 | Observation과 정보 분리 방식 |
| 5 | Simulation Environment·Sensor Virtualization·Dataset/Platform | 63 | 시뮬레이션 환경 사례 |
| 6 | Sim-to-Real·Domain Randomization·System Identification | 47 | Sim-to-Real 방법 후보 |
| 7 | Action Space·Controller·Force/Compliance Control | 53 | Action과 controller 대안 |
| 8 | Reward·Curriculum·Exploration·Replay | 70 | Reward와 학습 방법 후보 |
| 9 | Phase Transition·Skill Chaining·Hierarchical Policy | 71 | 단계 전환과 계층 구조 후보 |
| 10 | Safety·Constraint·Termination·Failure Recovery | 21 | 안전·종료·복구 방법 후보 |
| 11 | Geometry·Affordance·Contact Configuration Representation | 25 | Geometry와 contact 표현 방식 |


| 주제 묶음 | 페이지 |
| --- | --- |
| Low-level contact manipulation; blocker·clutter·retrieval | [Task and Retrieval](./task_and_retrieval.md) |
| Tactile·F/T; observation·history; simulation sensor·dataset | [Sensing and State](./sensing_and_state.md) |
| Sim-to-Real; action·controller; reward·curriculum | [Sim-to-Real and Learning](./sim2real_and_learning.md) |
| Phase transition; safety·termination; geometry·affordance | [Transitions, Safety and Geometry](./transitions_safety_geometry.md) |
