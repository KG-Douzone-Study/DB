# MyBatis_2

## resultMap 속성

- 검색 결과를 특정 자바 객체에 매핑하여 리턴하기 위해서 resultType 속성을 사용한다.
  그러나 검색 결과를 resultType 속성으로 매핑할 수 없는 몇몇 사례가 있다.

- 예를 들어 검색 결과가 단순 테이블 조회가 아닌 JOIN 구문을 포함할 때는 검색 결과를
  정확하게 나타내는 하나의 자바 객체로 매핑할 수 없다.

- 또는 검색된 테이블의 컬럼 이름과 매핑에 사용될 자바 객체의 변수 이름이 다를 때는
  검색 결과가 정확하게 자바 객체로 매핑되지 않는다.

- 위와 같은 경우에 < resultMap > 엘리먼트를 사용하여 매핑 규칙을 지정해야 한다.

![스크린샷 2023-05-24 오후 3 50 27](https://github.com/whochucompany/ByteClone-BE/assets/96435200/37790c29-9192-4a5c-b67d-ea60c978dc6d)

위 설정에서는 PK에 해당하는 SEQ 컬럼만 < id > 엘리먼트를 사용했고 나머지는 < result > 엘리먼트를
이용하여 검색 결과로 얻어낸 컬럼의 값과 BoardVO 객체의 변수를 매핑하고 있다.

---

## CDATA Section 사용

- SQL 구문 내에서 "<" 기호를 사용한다면 에러가 발생한다.
  이는 XML Parser가 XML 파일을 처리할 때 "<" 기호를 "작다" 라는 의미의 연산자가 아니라
  또 다른 태그의 시작으로 처리하기 때문이다.

- 이러한 에러를 방지하기 위하여 CDATA Section으로 SQL 구문을 감싸주면 된다.

- CDATA Section은 MyBatis와 상관없는 XML 고유 문법으로서 CDATA 영역에 작성된 데이터는
  단순한 문자 데이터이므로 XML 파서가 해석하지 않도로 한다.
  결국 CDATA Section 안에서 작성된 데이터는 XML 파서가 처리하지 않고
  그대로 데이터베이스에 전달하므로 문제가 발생하지 않는다.

```XML
<select id="getBoard" resultType="myboard">
	<![CDATA[
		SELECT * FROM 테이블명 WHERE seq <= #{seq}
	]]>
</select>
```

- SQL 대문자로 설정하기
  Mapper 파일에 등록되는 SQL 구문은 일반적으로 대문자로 작성한다.
  사실 SQL 구문은 대문자 소문자를 구별하지 않는다.
  하지만 일반적으로 가독성을 위하여 SQL 구문을 대문자로 표현하는 것을 선호하고 있다.
