# JH.Jeong Paper Index

> **범위:** Track B 연구를 위해 수집·검토한 논문과 baseline 후보의 탐색 포털
>
> **최종 갱신:** 2026-09-16

---

## 빠른 탐색

| 찾으려는 정보 | 이동할 페이지 |
| --- | --- |
| [B01](https://doi.org/10.48550/arXiv.2509.18455)–[B86](https://doi.org/10.48550/arXiv.1703.00472)의 제목·게재 정보·DOI·공식 자료 | [핵심 B-ID 논문 목록](./core_papers.md) |
| 연구 목적별 관련 논문과 Track B 연결점 | [목적별 논문 그룹](./topic_groups.md) |
| Motivation·observation을 위한 우선 독해 순서 | [우선 독해 가이드](./reading_guide.md) |
| Pushing·rotation/pivoting RL의 reward 항과 설계 이유 | [Reward formulation 문헌 비교](./reward_formulation.md) |
| ICRA·IROS 2021–2025 전수 screening | [Conference Screening Index](./screening/README.md) |

## 질문별 추천 경로

| 현재 질문 | 먼저 볼 페이지 | 함께 볼 페이지 |
| --- | --- | --- |
| 최신 VLA·IL·RL과 Track B의 gap은 무엇인가? | [목적별 그룹 — 최신 VLA·IL motivation](./topic_groups.md#8-최신-vlail-기반-research-motivation) | [핵심 B-ID 목록](./core_papers.md) |
| Rotation–Push transition과 contact configuration을 어떻게 비교할 것인가? | [목적별 그룹 — Rotation–Push 연결](./topic_groups.md#2-rotationpush-연결과-phasecontact-전환) | [우선 독해 가이드](./reading_guide.md) |
| Pushing과 rotation/pivoting reward를 어떤 근거로 구성할 것인가? | [Reward formulation 문헌 비교](./reward_formulation.md) | [목적별 그룹 — Reward formulation](./topic_groups.md#10-reward-formulation--pushing과-rotationpivoting) |
| Observation·history·tactile·F/T의 근거는 무엇인가? | [목적별 그룹 — Policy Observation](./topic_groups.md#7-policy-observation-표현과-isaac-lab-구현-근거) | [Observation 독해 가이드](./reading_guide.md#2-observation-formulation) |
| 특정 ICRA/IROS 논문을 연도별로 찾고 싶다. | [ICRA 목록](./screening/icra.md) 또는 [IROS 목록](./screening/iros.md) | [Screening Index](./screening/README.md) |
| Broad screening 결과를 연구 주제별로 찾고 싶다. | [Screening Index의 주제별 링크](./screening/README.md#주제별-screening) | 해당 주제 페이지 |

## 문서 체계

- `[B01](https://doi.org/10.48550/arXiv.2509.18455)–[B86](https://doi.org/10.48550/arXiv.1703.00472)`은 핵심 corpus의 고정 ID다. 새 핵심 논문은 검토 후 `B87`부터 부여한다.
- `ICRAyy-NNN`과 `IROSyy-NNN`은 conference screening용 ID이며 핵심 B-ID와 구분한다.
- 목록 포함은 baseline 선정, 방법 채택 또는 재현 완료를 뜻하지 않는다.
- 모든 문서에서 논문 제목·약칭·B-ID는 해당 논문의 DOI URL에 직접 연결한다. 정식 출판 DOI를 우선하고, 정식 DOI가 없지만 arXiv 원문이 있으면 arXiv DOI(`10.48550/arXiv...`)를 사용한다.
- DOI와 arXiv 원문이 모두 없는 논문은 예외적으로 공식 학회 원문에 연결하고 `DOI 없음`을 명시한다. 현재 이 예외는 [B22](https://openreview.net/forum?id=dT3ZciXvNX)와 [B27](https://openreview.net/forum?id=jf7C7EGw21)이다.
- 논문이 연구 결정을 바꾼 이유는 [`../context.md`](../context.md), 정리된 gap은 [`../motivation.md`](../motivation.md), 현재 명세는 [`../research_topic.md`](../research_topic.md)를 따른다.

## 갱신 원칙

1. 새 핵심 논문은 [핵심 B-ID 목록](./core_papers.md)에 먼저 추가한다.
2. 관련된 모든 목적 그룹에 같은 B-ID를 연결한다.
3. 읽기 우선순위가 달라질 때만 [우선 독해 가이드](./reading_guide.md)를 수정한다.
4. Conference screening에서 승격한 논문은 기존 screening ID를 유지하면서 새 B-ID와 교차참조한다.
5. 문헌 때문에 연구 명세·motivation이 바뀌면 변경 이유는 `context.md`에 기록한다.
6. 논문을 다른 문서에서 새로 언급할 때는 plain text로 두지 않고 위 DOI 우선순위에 따라 제목 또는 약칭 자체에 링크한다.
