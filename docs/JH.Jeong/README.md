# JH.Jeong Research Documents

이 디렉터리는 Track B 연구를 **문제 정의 → 현재 연구 범위 → policy 구현 → 문헌 근거 → 결정 이력**의 순서로 읽을 수 있도록 구성한다.

## 처음 읽는 사람의 권장 순서

1. [`motivation.md`](./motivation.md) — 왜 이 문제가 필요하고 기존 연구에서 무엇이 남았는가
2. [`research_topic.md`](./research_topic.md) — 현재 연구가 무엇을 포함하고 어디까지를 다루는가
3. [`policy_learning.md`](./policy_learning.md) — actor input, action, privileged information과 reward를 어떻게 구현할 것인가
4. [`papers/README.md`](./papers/README.md) — 각 판단을 뒷받침하는 논문을 어디서 찾는가
5. [`context.md`](./context.md) — 위 결론이 어떤 논의와 변경을 거쳐 만들어졌는가

`context.md`는 순차 설명문이 아니라 과거 논의까지 보존하는 decision log다. 현재 내용을 빠르게 이해하려면 앞의 네 문서를 먼저 읽는다.

## 문서별 역할

| 문서 | 답하는 질문 | 포함하지 않는 내용 |
| --- | --- | --- |
| [`motivation.md`](./motivation.md) | 왜 연구해야 하며 기존 연구 대비 gap은 무엇인가? | 센서 수치·reward 식의 세부 구현 |
| [`research_topic.md`](./research_topic.md) | 현재 연구의 대상·단계·역할 경계와 평가 수준은 무엇인가? | 과거 제안과 상세 결정 과정 |
| [`policy_learning.md`](./policy_learning.md) | Policy 학습 환경을 어떤 observation·action·reward로 구현하는가? | 광범위한 문헌 목록과 연구 배경 서술 |
| [`papers/`](./papers/README.md) | 어떤 논문이 어떤 주장과 설계의 근거인가? | 현재 연구 명세의 중복 서술 |
| [`context.md`](./context.md) | 무엇이 왜 변경되었고 어떤 항목이 미결인가? | 처음 읽는 사람을 위한 압축된 설명 |

## 현재 연구의 핵심

Track B는 shelf blocker의 선택된 OBB 면을 목표 방향에 정렬한 뒤 미는 Approach→Rotation→Push를 다룬다. Approach는 최초 접촉이 아니라 후속 조작에 유효한 wrist–hand configuration을 만드는 과정이며, Rotation은 face alignment뿐 아니라 다음 Push에 유효한 terminal contact를 남겨야 한다. Shared policy는 continuous pose·coarse OBB, binary tactile, wrist F/T와 proprioception을 이용해 aggregate contact를 가급적 유지하면서 필요한 contact configuration 변화를 폐루프로 수행한다.

## 문서 갱신 원칙

- 연구 배경이나 핵심 주장이 바뀌면 `motivation.md`를 수정한다.
- 현재 범위와 역할 경계가 바뀌면 `research_topic.md`를 수정한다.
- Actor·critic·environment 계약이 바뀌면 `policy_learning.md`를 수정한다.
- 논문을 추가하거나 해석을 보강하면 `papers/`의 해당 페이지를 수정한다.
- 변경 이유와 대체된 판단은 `context.md`에 남긴다.
