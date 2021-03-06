# (인사이트) 개발팀의 `KPI`

![dev_team](./assets/dev_team.jpg)

개발팀의 입장에서, 일반적으로 개발팀은 어떠한 한 프로젝트를 맡고, 그 소프트웨어 기획부터 런칭까지의 모든 개발 과정, 그리고 그 이후의 결과 분석 외에 기간 내 뚜렷한 성과로 잡기 까다로운 부분이 많습니다. 가령 리팩터링, 버그 수정, 피드백 반영 등의 자그마한 개발이라도 설계를 통한 개발이 필요하므로 시간이 오래 걸리기 때문입니다.

'프로그래머 KPI', '개발자 KPI'라고 검색했을 때 나오는, 개발 생산성을 정량화하는 데 사용되는 지표 중 가장 문제가 있는 것은 완성한 코드 라인 수로 생산성을 산출하는 방식입니다. 이는 지저분한 코드 베이스의 '기술적 부채' 속에 파묻도록 개발팀을 독려하는 방법입니다. 또 다른 지표료 제공된 사용자 스토리 또는 스토리 포인트의 수, 완료된 릴리즈의 수 등의 지표 또한 빈틈이 많습니다.

실제로 많은 기업들이 소프트웨어 개발 생산성을 측정하는 데에 많은 어려움을 겪고 있으며, 잘못된 평가 기준을 팀 사기를 저하할 수 있기 때문에 "오류, 전달 지연 또느 사고의 수에 따라 개발자 생산성을 측정해서는 안 된다. 이 경우 더 많은 기능을 더 빨리, 더 효과적으로 제공해야 한다는 압박에 항상 시달리는 개발팀에 불필요한 불안을 야기한다. 대신 심리적 안정감을 주고 운영 프레임워크를 지속해서 정렬하고 엔지니어링 방식을 개선해야 한다"라는 경고 또한 있습니다.

## 개발 생산성 평가 기준으로 성과를 평가하지 말 것

적절한 맥락 없이 생산성 자체를 성과와 동일시 하면 다음과 같은 바람직하지 않은 행동이 발생할 수 있습니다.

- 비즈니스 영향이 거의 없는 쉬운 기능을 더 많이 개발
- 위험, 보안, 품질, 운영 영향을 고려하지 않고 더 많은 소프트웨어 릴리즈
- 성공적인 실험을 위한 실질적인 프로덕션 경로가 없는 혁신

## 개발 생산성 지표를 사용해 비즈니스 돕기

소프트웨어 개발팀에게 생산성 지표를 적용하기 위해서는 수익을 늘리고 최종 사용자 경험을 개선하고 품질을 높이고 비용을 낮추고 혁신을 실현하는 데에 도움이 되며, 전략적 역량을 제공하고 협업을 개선하고 효율성을 유도하고 정보 접근을 간소화하거나 위험을 낮추는 것이 필요합니다.
소프트웨어 개발은 이와 같은 비즈니스 성과의 기여자인 경우가 많은데, 성과 기반 `KPI` 소프트웨어 개발 및 IT 메트릭을 위한 맥락을 제시해야 합니다.

- 사이클 시간, 팀 속도와 같은 애자일 메트릭과 평균 복구 시간과 같은 프로덕션 메트릭
- 생산성, 품질, 협업을 포착하는 완료된 코드 리뷰, 작성된 문서, 다른 사람과의 대화와 같은 행동에 초점.
- 소프트웨어 설계, 테스트, 통합과 같은 부가가치 활동을 환경 구성, 구성 문제 처리, 기존 코드가 어떻게 작동하는지 파악하기 위한 새 코드 작성과 같은 비 부가가치 활동과 비교

## 개발 생산성 지표를 사용한 생산성 개선

- 수동 배포 대신 `CI/CD`로 배포 자동화
- 코드형 인프라(`IaC`)와 컨테이너로 환경을 표준화해 수동 구성을 최소화하고 시스템의 차이에 따라 발생하는 결함 감소
- 회귀 테스트를 자동화해 개발자가 개발 과정 중 버그를 수정하고 프로덕션에서 발견된 결함을 조사 및 수정하는 데 소비하는 시간을 줄일 수 있도록 함.

## 생산성 메트릭의 초점을 비즈니스 성과에 맞춰야 함

> "IT에서 비즈니스 지표를 사용한 생산성 측정이 늘수록 경영진이 그 가치를 이해하기 쉬워진다. 나는 항상 추가된 기능 또는 해결된 비즈니스 문제 측면에서 개발 생산성을 측정하고, 이상적으로는 추가된 가치를 금액으로 환산하기 위해 노력한다. 이렇게 하면 비용으로써 IT에서 가치 제공자로써 IT로 대화의 방향을 전환할 수 있다."  
> 마틴 데이비스, 서던 컴퍼니 서비스(Southern Company Services) CIO

개발 생산성은 일반적으로 시간, 결함 또는 금액으로 산출됩니다. 그러나 아무 것도 하지 않을 때의 위험(보안, 기회) 대비 소비한 비용당 가치(수익, 기회, 운영 비용 절감)로 측정하는 것이 더 이상적이라고 합니다. 궁극적으로, 팀은 먼저 비즈니스 성과를 측정한 다음 목표로 한 행동을 나타내는 평가 기준으로 이를 정성화해야 합니다. 성과와 전달 정성화를 결합한 `KPI`는 누가, 무엇이, 왜, 어떻게 높은 품질의 소프트웨어를 전달하는 지에 대한 답을 찾는 데 도움이 됩니다.

- 비즈니스 성과로 고객 만족 메트릭 개선을 요청한 개발팀은 평균 복구 시간 문제와 결함 탈출률을 개선하는 데 초점을 맞출 수 있다.
- 개발팀은 이러한 메트릭에서 개선을 입증하면서 자동화, 문서 작성 또는 기술 부채 감소 등 생산성 개선에 투자하는 부분을 명시해야 한다.
- 개발팀은 개선을 전달하면서 고객 만족도를 측정하고 선택한 메트릭에 대해 보고해야 한다.

> 비즈니스 성과와 개발자 생산성 메트릭을 결합한 `KPI`는 '팀이 생산성을 개선하면서 우선순위가 높은 비즈니스 성과를 제공하고 있는가'라는 질문에 답하는 데 도움이 됩니다. 기업은 팀이 모든 부분을 동시에 개선할 수 없음을 고려해 생산성을 개선하면서 성과를 제공하는 현명한 의사 결정을 내리는 팀을 높이 평가해야 합니다.

---

## 디자인 팀의 `KPI`

- 정량적

- 유저의 태스크 성공률(`CVR`)
- 유저가 어느 목적을 달성할 때 걸린 시간(Time on Task)
- 검색 또는 네비게이션 이용률(Use of search vs. navigation)
- 이탈률
- 시스템 유저빌리티 스케일(`SUS`, System Usability Scale)

- 정성적

- 유저의 기대치
- 종합적 만족도

---

# 참고

- https://brunch.co.kr/@ideawriter/18
- https://brunch.co.kr/@dalgudot/108
- https://medium.com/@enamu/ux-%EB%94%94%EC%9E%90%EC%9D%B8%EC%9D%98-%ED%8F%89%EA%B0%80%EC%99%80-kpi-ac84a4f9f564
- https://www.beusable.net/blog/?p=3069
