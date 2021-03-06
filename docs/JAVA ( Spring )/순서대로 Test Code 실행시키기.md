## @FixMethodOrder

 - Junit 4.11부터 지원
 - TC 실행 순서를 정할 수 있다.

|속성|설명|
|---|---|
|MethodSorters.DEFAULT|HashCode를 기반으로 순서가 결정. 사용자가 예측하기 힘들다.|
|MethodSorters.JVM|JVM에서 리턴되는 순으로 실행. 때에 따라서 실행시 변경된다.|
|MethodSorters.NAME_ASCENDING|메소드 명을 오름차순으로 정렬한 순서대로 실행|

위에 두 속성은 순서가 바뀔수 있기 때문에 맨 아래에 있는 속성을 사용하여 오른 차름수로 순서가 고정되게 테스트 코드를 작성 할 수 있다.

```java
@FixMethodOrder(MethodSorters.NAME_ASCENDING)
public class ServiceTest() {
...
```

위와 같이 작성하고 이게 맞는 방법인지 모르겠지만 저같은 경우 test method에 a,b,c ... 를 붙혀서 순서를 임의로 지정하고 있습니다.


```java
@Test
public void a_simpleTest() {

}

@Test
public void b_serviceTest() {

}

...
```
