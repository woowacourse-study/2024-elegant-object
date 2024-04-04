> 인자의 값으로 NULL을 절대 허용하지 마세요. (p. 118~)

**저자의 논리에는 동의한다. 그런데 현실적으로 Null에 대한 처리를 아예 안 하는 게 가능할까?**  
우리가 만약 우리가 짠 코드로만 프로그램을 만든다면, 그리고 모든 함수에 return Null을 하나도 포함하지 않았다면 저자의 이상대로 실천이 가능할 것 같다. 그런데 현실은 그럴 수 없지 않겠는가. 외부의 함수들을 이용하게 될 텐데 거기에서 Null을 반환할 여지가 있다면 그것을 어떻게 처리해야 할까? Null에 대한 처리가 어딘가에는 존재해야 하는 거 아닌가.

-> 별도의 처리를 하지 않고 Null이 예외를 발생시키도록 방치한다.  
(JVM 표준 방식대로) NullPointerException으로 처리되도록 냅둔다는 것이 정확히 어떤 의미인가? Null을 참조하는 순간 예외가 발생하니, 그 예외만 잡아주고 세부 로직에서는 Null을 고려하지 않겠다는 의미? Null로 인한 오류가 앱을 터뜨리도록 냅두라는 건 아닐테고, 전체 단에서 그 에러를 처리해준다는 의미? 그러면 디버깅이 안 되지 않나?

**답변:** 자바나 코틀린에서 null은 즉시 오류를 발생시킨다. null의 존재 자체가 오류가 발생했음을 의미한다. 그러니 null이 발생하면 고쳐야 하는 것이다. 이런 맥락에서 보면 null을 별도 처리하지 않고 발생하도록 방치하라는 것이 이해가 된다. 오류(null)가 발생하도록 놔두면 그 오류를 만든 개발자가 알아서 고칠 것 아닌가? 아무튼 저자의 요지는 null을 최대한 다루지 말라는 것이다. null을 다루다보면 ```if (null)```과 같은 형태의 분기 처리로 객체와 직접 소통하지 않게 되기 때문이다(이것이 객체에게 인종차별을 하는 것이라고 표현한다). null을 인자로 받지도 말고(고려하지 말고), return하지도 마라(無를 return 할일이 있으면 빈 객체를 별도로 만들어라). 한편, JS의 경우 querySelector, localStorage 등의 외부 api를 다루게 되면 null과 마주칠 수밖에 없는 상황이 발생한다. 심지어 js에서는 null이 버젓이 돌아다니는 게 가능하다. 이를 해결하려면 어떻게 해야 할까? 래핑하자. 래핑해서 그 null을 고립시켜라. 혹은 최대한 빠르게 null을 처리하라(옵셔널 체이닝도 null을 처리하는 행위의 일종).

> 상수 객체가 설계하고, 유지보수하고, 이해하기가 더 쉽기 때문에 불변 객체보다는 상수 객체를 사용하는 것이 더 좋습니다. (p. 133, 밑에서 6번째 줄)

**상수 객체와 불변 객체는 애초에 용도가 다른 거 아닌가?**  
물론 상수 객체가 관리하는 게 용이하기야 하겠지만, 불변 객체를 사용할 수밖에 없으니 사용하는 거 아닌가? 메모리, 웹 페이지와 같은 정보를 상수 객체 형태로 다룰 수 없지 않은가? 어떻게 불변 객체를 상수 객체로 만들 수 있을까?

답변: 그런 것 같긴 하다. 외부에 의존하고 있는 경우에 그것을 상수 객체로 만드는 게 불가능하기 때문이다. 상수 객체가 "상수 객체가 아닌 불변 객체"보다 다루기 더 쉽고 명료하니, 가능하다면 상수 객체를 활용하자는 의미로 받아들이자.