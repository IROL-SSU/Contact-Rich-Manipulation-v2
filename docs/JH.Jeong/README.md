# JH.Jeong Research Documents

이 디렉터리는 Track B 연구를 **문제 정의 → 현재 합의 범위 → 협의 중인 설계 → 문헌 근거 → 결정 이력**의 순서로 읽을 수 있도록 구성한다.

현재 합의된 범위는 **Intro와 actor observation까지**다. Policy architecture, action/controller, reward, termination, critic·RL 세부 방법, evaluation과 experiment는 모두 `[Open]`이며, 문서에 남은 관련 내용은 확정안이나 실험 계획이 아니라 논의 후보다.

## 처음 읽는 사람의 권장 순서

1. [`Intro/`](./Intro/README.md) — Research Motivation → Research Trend → Previous Works → Candidate Contributions
2. [`research_topic.md`](./research_topic.md) — 현재 연구가 무엇을 포함하고 어디까지를 다루는가
3. [`policy_learning.md`](./policy_learning.md) — 합의된 actor observation과 이후 `[Open]` 설계 후보
4. [`papers/README.md`](./papers/README.md) — 각 판단을 뒷받침하는 논문을 어디서 찾는가
5. [`context.md`](./context.md) — 위 결론이 어떤 논의와 변경을 거쳐 만들어졌는가

`context.md`는 순차 설명문이 아니라 과거 논의까지 보존하는 decision log다. 현재 내용을 빠르게 이해하려면 앞의 네 문서를 먼저 읽는다.

## 문서별 역할

| 문서 | 답하는 질문 | 포함하지 않는 내용 |
| --- | --- | --- |
| [`Intro/`](./Intro/README.md) | 왜 연구해야 하며 기존 연구 대비 gap과 contribution 후보는 무엇인가? | 센서 수치·reward 식의 세부 구현 |
| [`research_topic.md`](./research_topic.md) | 현재 연구의 대상·단계·역할 경계와 합의 범위는 무엇인가? | 과거 제안과 상세 결정 과정 |
| [`policy_learning.md`](./policy_learning.md) | 합의된 actor observation은 무엇이며 이후 어떤 항목이 협의 중인가? | 광범위한 문헌 목록과 연구 배경 서술 |
| [`papers/`](./papers/README.md) | 어떤 논문이 어떤 주장과 설계의 근거인가? | 현재 연구 명세의 중복 서술 |
| [`context.md`](./context.md) | 무엇이 왜 변경되었고 어떤 항목이 미결인가? | 처음 읽는 사람을 위한 압축된 설명 |

## 현재 연구의 핵심

Track B는 shelf blocker를 필요한 경우 후속 Push에 적합한 상태로 회전한 뒤 지정 방향으로 미는 Approach→Rotation→Push를 다룬다. 초기 상태가 이미 Push에 적합하면 Rotation은 작거나 생략될 수 있다. Approach는 최초 접촉이 아니라 후속 조작에 유효한 wrist–hand configuration을 만드는 과정이다.

현재 합의된 actor observation은 continuous object pose·coarse OBB, binary tactile, wrist F/T, robot configuration과 task command를 함께 사용한다. Selected OBB face는 상위 모듈이 제공하며 phase ID는 observation에 포함하지 않는다. Shared policy 여부, action/controller, 접촉 전환 방식, S0·S1의 검증 순서, reward, evaluation과 experiment는 아직 협의 중이다.

## 문서 갱신 원칙

- 연구 배경, research trend, previous works 또는 contribution 후보가 바뀌면 `Intro/`의 해당 문서를 수정한다.
- 현재 범위와 역할 경계가 바뀌면 `research_topic.md`를 수정한다.
- Actor observation 또는 그 이후의 `[Open]` 설계 논의가 바뀌면 `policy_learning.md`를 수정한다.
- 논문을 추가하거나 해석을 보강하면 `papers/`의 해당 페이지를 수정한다.
- 변경 이유와 대체된 판단은 `context.md`에 남긴다.
