# (인사이트) 2주간 `React.js` 강의를 진행하며...

2022년 1월 10일(월)부터 2022년 1월 20일(목)까지 동아대학교 컴퓨터 공학과에서 진행하는 실증 아카데미 프로그램의 강사로 `React.js` 강의를 진행하게 되었는데, 내가 상상했던 강의의 모습과 실제 강의의 모습의 차이점을 몸소 느끼며 어떠한 점이 강의를 실패하게 만들었는지, 어떻게 하면 더 좋은 강의를 진행할 수 있는 지를 곱씹어 보았다.

## 실습의 경우 보조자의 역할이 큼

강의 전 학교 측에서 실습 보조 학생을 붙여준다는 이야기가 나왔고 실습 보조 학생에게 수업 중 필요한 일 등등을 도움 받으면 된다고 했지만, 어떤 도움을 요청할 지 와닿지 않았고 이 생각은 이론 수업을 할 때까지 변함없이 갖고 있었다. 하지만 실습이 시작되며 각각 학생들의 컴퓨터 환경, 이해도 등의 차이로 직접 코드나 환경을 손봐줘야 하는 상황이 생기자 한 두 명까지는 내가 커버할 수 있었지만 20명정도 되는 학생들을 모두 컨펌하기엔 시간이 너무 부족했다. 이때 실습 보조 학생분들께서 적극적으로 도와주신 덕에 강의를 무사히 마무리할 수 있었는데, 말 그대로 이론에서는 보조자의 역할이 중요하지 않을 수 있지만, 실습이 있는 강의라면 보조자의 역할이 크기 때문에 필수라고 생각된다.

## 강의 `PPT`는 기본이지만, 퀄리티보다는 추가적인 자료가 더 중요

강의를 진행하기에 앞서, 일반적으로 강의에 `PPT`, 프레젠테이션 파일이 포함되어 있는 것은 당연한 요소이기 때문에 나 또한 미리캔버스를 사용해 `PPT`를 제작했다. 학생 이후로 처음 제대로 만들어보는 `PPT` 였기 때문에 시간이 걸렸고, 초반의 이론 중심에 내용에 비해 후반 실습 중심의 내용은 눈에 띄도록 부실했다. 이 부실한 `PPT`를 만들면서 "이를 강의로 커버하자." 라는 생각을 했었는데, 역시나 실제로 적용하려보니 어떤 내용을 전달하려 했는지 기억이 나지 않고, 그 내용들을 메모해둔 다음 마지막 시간에 한 번에 전달해 오히려 학생들에게 혼란을 심어주지 않았나라는 결과를 발생시켰다.

일단은 나처럼 시각 자료 제작에 능력이나 경험, 지식이 부족한, `PPT`를 전문적으로 만들 수 없는 사람은 `PPT` 만드는 시간을 사용할 바에는, 잘 만들어져 있는 다른 시각 자료를 찾아 참고용으로 보여주는 편이 훨씬 수월함을 느꼈다. 실제로 잘 정리된 시각 자료를 보여드렸을 때의 이해한 반응이 내가 만든 자료 + 말로 설명함의 반응보다 좋았다.

이를 4회차. 즉, 첫 주 강의를 끝 마친 후에 이를 깨달았기에 나는 `PPT`에 시간을 쓰기보다는 추가적인 자료를 찾는 데에 더 중점을 두었고, 내가 잘 만들 수 있는 자료인 소스 코드와 그에 대한 설명을 마크다운 파일(`.md`)로 작성하여 공유했다. 결과는 긍정적이었고, "`PPT` 제작에 시간을 길들이지말고 이렇게 설명하는 파일을 처음부터 작성했더라면 강의 품질이 더 좋았을텐데"라는 후회가 남았지만, 이렇게라도 일찍 깨달았기 때문에 혹시나 추후 다시 강의의 기회가 주어졌을 때 잘 적용할 수 있지 않을까라는 생각을 하게 됐다.

## 질문은 대답을 원하는게 아닌 이목을 집중시키기 위함

이론을 진행할 때 나 혼자 떠드는 시간이 많다보니 강의가 지루해지는게 눈에 보여 중간중간 대답을 유도하는 질문을 하곤 했는데, 90%이상 무반응이었던 것 같다. 왜 그런지 생각을 해보니 대답을 받을 수 있던 질문은 어느정도 답이 정해져 있으며, 학생들이 이 질문에 대한 대답을 한다고 한들 강의 방향에 영향을 끼치지 않는 요소였고, 대답이 없던 질문은 학생의 대답에 따라 강의가 변할 수 있는 요소. 즉, 결정력이나 영향력이 발생하는 요소들이었다.

예를 들어, "어떤 순서로 강의를 진행할까요?"와 같은 대답은 반응을 얻기 어렵고 학생을 부담스럽게 하는데, "이런 순서로 진행할건데 반대하시는 의견 있으신가요?"와 같은 대답은 흘러지나가는 듯한 발언으로 긍정의 반응이 대다수였다. 답정너 질문을 하는게 무의미해보이지만, 이 질문으로 학생들에게 순간적인 이목 집중을 받아내, 그 다음 말을 효과적으로 전달할 수 있는 좋은 매개체로 작용하지 않았나 싶었다. 그만큼 강의는 기본적으로 강사가 모든 준비를 해두어야 하며, 모든 길과 전달할 내용은 정해져 있으나, 순간적인 집중도를 부여하기 위한 목적 이외로 질문의 효과를 기대해선 안 될 것 같다는 생각이 들었다. 학생과 소통하는 수업이라는 이야기로 평가가 좋은 강의도 이런 느낌이지 않을까?

## 강의 목적 의외의 내용은 후에 꼭 알아야 할 정보라도 언급하지 않아야 함

나는 `React.js`에 대한 강의를 진행했는데, 진행하다보니 이 학생들은 컴퓨터 공학과 학생이고, 추후에 `FE` 개발자가 되든, 서버 개발자가 되든, 개발자의 길을 걷게될 사람이라고 생각을 하게 되어 중간중간 나중을 고려한 말을 많이 했다. 그런데 이게 오히려 아무 것도 모르는 학생들의 입장에서는 안 그래도 외계어에 가까운 수업에 다른 외계어가 추가된 기분이 들지 않았을까 하는 생각이 들었다. 나는 어떤 내용을 설명하면서 "아, 그런데 이건 지금 알아도 나중에 쓸모 없을거 같으니 대충 있다고만 알아두세요.", "아 이거는 보통 이렇게 설명 많이 하는데, 실제론 이렇게 사용해야 합니다." 등의 말을 많이 했는데, 애초에 이 뉘앙스 자체가 애드립에 가까운 발언이며 전자든 후자든 저런 추가적인 어휘를 사용함에 따라 전달력이 감소될 것 같다는 느낌이 강하게 들었다. 차라리 할거면 애초부터 적게 설명을 하고, 실제 사용 예제만을 전달하여 방향성이나 이해 가능성을 높이는 편이 좋을 것 같다.

---

이렇게 깨달은 부분 이외에도 사소하게 깨달은 점이 많은데, 혹여나 다음에 기회가 생기게 된다면 발전된 강의를 할 수 있도록 노력해야겠다.
