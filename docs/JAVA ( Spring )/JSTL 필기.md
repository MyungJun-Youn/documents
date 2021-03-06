### &lt;c:out&gt;

```html
<c:out value="출력할 값" default="기본값"/>  
<c:out value="출력할 값">기본값</c:out>
```

출력할 값이 null일 경우 기본값을 출력한다.


### &lt;c:set&gt;

```html
<c:set var="변수명" value="값" scope="page|request|session|application"/>  
<c:set var="변수명" scope="page|request|session|application">값</c:set>  
```

context 변수의 값을 변경한다. scope가 없을 경우 default는 page다.

```html
<c:set target="객체명" property="프로퍼티명" value="값" />
```

객체의 프로퍼티의 값을 변경 할 수도 있다.

### &lt;c:remove&gt;

```html
<c:remove var="변수명" scope="page|request|session|application" />
```

context 변수를 삭제한다. scope의 기본값은 page이다.

### &lt;c:if&gt;

```html
<c:if test="조건" var="변수명" scope="page|request|session|application">  
    콘텐츠  
</c:if>
```

조건이 true일 경우 변수에 true값이 들어가고 콘텐츠가 화면에 출력된다.  
반대로 false일 경우 변수에 false값이 들어가고 콘텐츠가 출력되지 않는다.

### &lt;c:choose&gt;

```html
<c:choose>  
    <c:when test="조건식1">콘텐츠1</c:when>  
    <c:when test="조건식2">콘텐츠2</c:when>  
    ...  
    <c:otherwise>콘텐츠n</c:otherwise>  
</c:choose>
```

조건식에 맞는 콘텐츠가 출력된다.  
&lt;c:when&gt; 태그는 1개 이상 존재해야 한다. &lt;c:otherwise&gt; 태그는 1개 이하 존재해야 한다.

### &lt;c:forEach&gt;

```html
<c:forEach var="변수명" items="목록데이터" begin="시작인덱스" end="종료인덱스">  
    콘텐츠  
</c:forEach>
```

목록데이터의 내용을 하나씩 변수명에 저장하여 콘텐츠를 반복 실행한다.  
목록데이터의 속성은 다음이 올 수 있다.  

- 배열
- java.util.Collection 구현체(ArrayList, LinkedList, Vector, EnumSet 등)
- java.util.Iterator 구현체
- java.util.Enumeration 구현체
- java.util.Map 구현체
- 콤마(,) 구분자로 나열된 문자열

시작인덱스와 종료인덱스로 몇 번째 인덱스에서 시작하고 몇 번째 인덱스에서 종료할 것인지를 저장한다.  
만약 10회 반복하고 싶다면 **종료인덱스-시작인덱스+1** 의 값이 10이 되게 설정하면된다.  

### &lt;c:forTokens&gt;

```html
<c:forTokens var="변수명" items="문자열" delims="구분자">  
   콘텐츠  
</c:forTokens>
```

문자열을 특정 구분자로 분리하여 반복문을 돌릴 수 있다.  
구분자로 구분된 문자열은 순서대로 변수에 저장된다.

### &lt;c:url&gt;

```html
<c:url var="변수명" value="주소">  
   <c:param name="이름1" value="값1" />  
   <c:param name="이름2" value="값2" />  
</c:url>  
<a href="${변수명}" />
```

매개변수를 포함한 URL을 손쉽게 만들 수 있다.

&lt;a href="주소?이름1=값1&이름2=값2"/&gt;

### &lt;c:import&gt;

```html
<c:import url="주소"/>
```

주소에 있는 html을 가져와 출력한다.

### &lt;c:redirect&gt;

```html
<c:redirect url="주소"/>
```

`HttpServletResponse`의 `sendRedirect()`를 호출한다. 주소로 이동한다.

### &lt;fmt:parseDate&gt;

```html
<fmt:parseDate var="변수명" value="2016-01-10" pattern="yyyy-MM-dd" />
```

날짜 형식으로 된 문자열을 분석하여 java.util.Date 객체를 생성한다.  
그리고 지정된 보관소(scope)에 저장한다. 기본 값은 page이다.

### &lt;fmt:formatDate&gt;

```html
<fmt:formatDate value="${변수명}" pattern="MM/dd/yy" />
```

value의 값을 pattern에 맞게 출력한다.

	출처 : 열열강의 자바 웹 개발 워크북(프리렉 엄지영) 요약
