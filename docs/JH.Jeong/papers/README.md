# Track B Literature Map

> **문서 역할:** 논문을 나열하는 목록이 아니라, 현재 연구 질문에서 필요한 근거로 이동하는 진입점이다.
>
> **최종 갱신:** 2026-09-20

---

## 1. 처음 들어왔다면

문헌 전체를 ID 순서로 읽지 않는다. 현재 목적에 따라 다음 경로 중 하나를 선택한다.

### 연구의 필요성과 gap을 검토할 때

1. [`../Intro/README.md`](../Intro/README.md)에서 Intro의 전체 논리와 research question을 확인한다.
2. [`../Intro/research_trend.md`](../Intro/research_trend.md)에서 기존 method가 해결한 범위와 남긴 부담, 조건부 RL 선택 근거를 확인한다.
3. [`../Intro/previous_works.md`](../Intro/previous_works.md)에서 11편의 comparison-set timeline, closest-work 비교와 contribution 경계를 확인한다.
4. [`topic_groups.md`](./topic_groups.md#8-최신-vlail-기반-research-motivation)에서 최신 VLA·IL·RL이 이미 해결한 부분과 남은 질문을 비교한다.
5. [`reading_guide.md`](./reading_guide.md#1-motivation을-검증하는-독해-경로)의 순서대로 핵심 원문을 읽는다.
6. 정확한 서지정보와 공식 원문은 [`core_papers.md`](./core_papers.md)에서 찾는다.

### Policy observation을 설계할 때

1. [`../policy_learning.md`](../policy_learning.md#3-observation-v03)에서 현재 66D baseline을 확인한다.
2. [`reading_guide.md`](./reading_guide.md#2-observation을-검증하는-독해-경로)에서 각 입력을 사용한 이유를 따라간다.
3. 더 넓은 후보가 필요하면 [`topic_groups.md`](./topic_groups.md#7-policy-observation-표현과-isaac-lab-구현-근거)와 [Sensing and State screening](./screening/sensing_and_state.md)을 검색한다.

### Reward를 설계할 때

1. [`reward_formulation.md`](./reward_formulation.md)에서 pushing·pivoting reward를 failure별로 비교한다.
2. [`reading_guide.md`](./reading_guide.md#3-reward를-검증하는-독해-경로)의 순서로 원문을 확인한다.
3. 넓은 reward·safety·transition 후보는 [Sim-to-Real and Learning](./screening/sim2real_and_learning.md)과 [Transitions, Safety and Geometry](./screening/transitions_safety_geometry.md)에서 찾는다.

---

## 2. 문서별 역할

| 문서 | 용도 | 읽는 방식 |
| --- | --- | --- |
| [`core_papers.md`](./core_papers.md) | B-ID, 제목, venue, DOI와 공식 자료의 기준 목록 | 특정 ID나 제목을 찾는 lookup table |
| [`../Intro/research_trend.md`](../Intro/research_trend.md) | Method-family별 해결 범위·남는 부담과 조건부 RL 선택 | 발표의 Research Trend 및 RL 선택 근거 |
| [`../Intro/previous_works.md`](../Intro/previous_works.md) | 11편의 2022–2026 timeline과 environment·agent·system 비교 | Chronology와 closest-work gap을 함께 판정하는 비교문 |
| [`topic_groups.md`](./topic_groups.md) | 논문을 Track B의 연구 질문별로 묶고 활용점·한계를 비교 | 필요한 질문의 절만 읽는 synthesis |
| [`reading_guide.md`](./reading_guide.md) | Motivation, observation, reward별 우선 독해 순서 | 위에서 아래로 읽는 작업 순서 |
| [`reward_formulation.md`](./reward_formulation.md) | Reward term, 해결 failure, 이식 가능성과 위험 분석 | 결론→pushing→rotation→Track B 합성 순으로 읽는 분석문 |
| [`screening/`](./screening/README.md) | ICRA·IROS 2021–2025의 넓은 후보군 | 핵심 corpus 밖의 후보를 검색하는 appendix |

`core_papers.md`와 `screening/`은 순차 서술문이 아니다. 반대로 `reading_guide.md`와 `reward_formulation.md`는 논리가 이어지도록 처음부터 읽을 수 있게 구성한다.

---

## 3. ID와 근거 수준

- `B01–B98`는 상세 검토 대상으로 승격한 핵심 corpus의 고정 ID다. 새 논문은 검토 후 `B99`부터 부여한다.
- `ICRAyy-NNN`, `IROSyy-NNN`은 conference screening ID이며 핵심 B-ID와 구분한다.
- 목록에 포함됐다는 사실은 baseline 채택, 방법 재현 또는 논문 claim의 검증을 의미하지 않는다.
- `screening`은 초록 수준, `core_papers`는 서지 확인, `topic_groups`와 `reward_formulation`은 연구 질문에 연결한 분석이라는 근거 수준 차이가 있다.

논문 링크는 정식 출판 DOI를 우선하고, 정식 DOI가 없으면 arXiv DOI를 사용한다. 두 DOI가 모두 없을 때만 공식 학회 원문을 사용하며 `DOI 없음`을 표시한다. 현재 명시적 예외는 [B22](https://openreview.net/forum?id=dT3ZciXvNX)와 [B27](https://openreview.net/forum?id=jf7C7EGw21)이다.

---

## 4. 갱신 절차

1. 새 논문의 venue, DOI와 공식 원문을 확인한다.
2. 핵심 corpus에 포함할 가치가 있으면 [`core_papers.md`](./core_papers.md)에 B-ID를 부여한다.
3. 논문이 답하는 연구 질문에 따라 [`topic_groups.md`](./topic_groups.md)의 관련 절에 연결한다.
4. 독해 우선순위를 바꿀 정도로 중요할 때만 [`reading_guide.md`](./reading_guide.md)를 수정한다.
5. Reward term의 근거가 되면 [`reward_formulation.md`](./reward_formulation.md)에 `항 → 해결 failure → Track B 이식 조건`을 기록한다.
6. 연구 명세나 motivation이 바뀌면 해당 기준 문서를 갱신하고 변경 이유는 [`../context.md`](../context.md)에 남긴다.
