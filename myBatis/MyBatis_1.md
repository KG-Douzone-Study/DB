# MyBatis_1

## MyBatis 프레임워크의 특징

- MyBatis 프레임워크의 특징을 보면 첫째는 한두 줄의 자바 코드로 DB 연동을 처리한다는 점이며
  둘째는 SQL 명령어를 자바 코드에서 분리하여 XML 파일로 따로 관리한다는 것이다.
  
- 사실 유지보수 관점에서 보면 DB 연동에서 사용된 복잡한 자바 코드는 더 이상 중요하지 않다.
  개발자는 실행되는 SQL만 관리하면 되며 MyBatis는 개발자가 이 SQL 관리에만 집중할 수 있도록
  도와준다.

- 만약 SQL 명령어가 DAO 같은 자바 클래스에 저장되면 SQL 명령어만 수정하는 상황에서도 자바 클래스를
  다시 컴파일 해야한다. 그리고 SQL 명령어들을 한 곳에 모아서 관리하기도 쉽지 않다.
  결국 SQL 명령어에 대한 통합 관리를 위해서라도 자바 소스코드에서 SQL을 분리하는 것은 매우 중요하다.

---

- MyBatis 프로젝트 (STS 기준)
```txt
STS -> [File] -> [New] -> [Maven] -> [Maven Project]
프로젝트 생성 후
[Project] -> [Properties] -> [Project Facets] -> [Java 버전 맞춰주기]
```

- 기본
```txt
- mybatis-config.xml
  mybatis에서 사용될 DB를 연동하기 위한 설정값들과 mapper.xml을 등록하기 위한 xml

- mybatis-mapper.xml
  mybatis에서 사용될 SQL 구문을 담고 있는 xml
```

- 초기 설정
```txt
1. xml 환경 설정
   
- mybatis-config.xml -> config.dtd
  Location: http://mybatis.org/dtd/mybatis-3-config.dtd
  Key: //mybatis.org//DTD Config 3.0//EN

- mybatis-config.xml -> mapper.dtd
  Location: http://mybatis.org/dtd/mybatis-3-mapper.dtd
  Key: //mybatis.org//DTD Mapper 3.0//EN
```

- Mybatis Source Folder 생성 및 xml 파일 생성
```txt
1. STS 프로젝트에 resource folder 가 없다면 프로젝트를 우클릭한 후 [New] -> [Source Folder]
2. src/main/resources 폴더 생성 -> 폴더 안에 mybatis-config.xml 생성
3. resources 폴더 안에 새로 mappings 폴더 생성 -> mybatis-mapping.xml 생성
```

---

- 파일 중에서 가장 중요한 SQL 명령어를 담고 있는 mybatis-mapping.xml 이다.

![스크린샷 2023-05-24 오전 11 38 44](https://github.com/whochucompany/ByteClone-BE/assets/96435200/5bbc29f8-ccc8-4cd9-9ff0-176a04a4ad55)

- mybatis-mapping.xml
```txt
mapper 태그의 namespace 속성 설정

sql 문을 담은 태그 섫정

- id : 속성 설정

- parameterType : 속성 설정
  패키지 포함 클래스 풀네임으로 설정
  단, mybatis-config.xml 에서 alias 별칭을 정해주었다면 별칭으로 사용 가능
  자바 타입의 내장된 별칭 사용 가능(대소문자 구분)
  String -> string, Integer -> integer, Map -> map, HashMap -> hashmap
  Collection -> collection, ArrayList -> arrayList

- 가져오는 파라미터 값 설정 : ${} 형태와 #{} 형태를 이용해 값을 가져온다.
  ${ } : 데이터에 따른 '' 문자열 처리를 해주지 않는다 (Statement)
  #{ } : 자동으로 데이터를 입력처리 해준다 (PreparedStatement), 단일 데이터 : #{}, 다중 데이터
  : map 이나 vo 활용

- 추후 DAO에서 (namespace.id, [parameterType]) 를 이용하여 연결한다.
```

**SQL Mapper 파일은 < mapper > 를 루트 엘리먼트로 사용한다.
그리고 < insert >, < update >, < delete >, < select > 엘리먼트를 이용하여 필요한 SQL문을 등록한다.**

---

- Mybatis 환경 설정 파일

```txt
MyBatisProject에 src/main/reousrces 폴더를 선택한 후 마우스 우 클릭하여
[New] -> [Other] -> [General] -> [File]을 선택하여 db.properties 파일을 만든다.
```

```txt
db.properties 파일 수정

jdbc.driverClassName=oracle.jdbc.driver.OracleDriver jdbc.url=jdbc:oracle:thin:@localhost:1521/XEPDB1
jdbc.username=자신의 oracle 아이디
jdbc.password=자신의 oracle 비밀번호
```

```txt
MyBatisProject에 src/main/reousrces 폴더를 선택한 후 마우스 우 클릭하여
[New] -> [Other] -> [XMl] -> [XML File]을 선택하여 mybatis-config.xml 파일을 만든다.
```

![스크린샷 2023-05-24 오후 1 49 05](https://github.com/whochucompany/ByteClone-BE/assets/96435200/5b8664d5-df7c-4492-9c04-7c3d64ab3761)

---

- SqlSession 객체 생성하기

MyBatis를 이용하여 DAO를 구현하려면 SqlSession 객체가 필요하다.
그런데 이 SqlSession 객체를 얻으려면 SqlSessionFactory 객체가 필요하다.

- SqlSessionFactoryBean 클래스 작성

![스크린샷 2023-05-24 오후 2 04 22](https://github.com/whochucompany/ByteClone-BE/assets/96435200/44cc76d7-6d30-4c7d-b4a4-3b3a707e88df)

- DAO 클래스 작성

![스크린샷 2023-05-24 오후 2 13 15](https://github.com/whochucompany/ByteClone-BE/assets/96435200/d76481fa-b286-4c4f-bfd2-84e28ba000f4)

위 코드에서 구현된 각 메서드를 보면 두 개의 정보가 인자로 전달되고 있는데, 

첫 번째 인자는 실행될 SQL의 id 정보이다.
이때 SQL Mapper에 선언된 네임스페이스와 아이디를 조합하여 아이디를 지정해야 한다.

두 번째 인자는 parameterType 속성으로 지정된 파라미터 객체이다.
등록, 수정, 삭제는 각각 insert(), update(), delete() 메서드로 처리하며
단 한건의 조회는 selectOne()
목록 조회는 selectList() 메서드로 처리한다.

---

- 테스트 클라이언트 작성

![스크린샷 2023-05-24 오후 2 38 39](https://github.com/whochucompany/ByteClone-BE/assets/96435200/298cab04-25c8-45c6-b995-aabcca7f30a4)

---
