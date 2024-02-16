---
published : true
title : Query Methods
date : 2024-02-16 +09:00
categories : [Spring, JPA]
tags : [
  JPA,
]
---
<!-- ![](/assets/img/Spring/aaaa.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->
<!-- ![](/assets/img/alg/4-1.png){:style="width:1000px" } -->

### Query Methods
- Spring Data JPA의 쿼리를 생성하는 기능으로, 메소드명을 규칙에 맞게 작성하면 자동으로 쿼리가 생성
- 복잡한 Entity나 Join 상황에서는 활용이 어렵다

### 샘플 예제

##### User Entity

```java
@Entity
@Getter
@Setter
public class User{

  @Id
  private Long id;

  @Column(name = "FirstName")
  private String firstName;

  @Column(name = "Last_Name")
  private String lastName;

  private int age;

  private LocalDateTime startDate;

  private boolean active;
}
```

- 쿼리 메소드를 사용 시 변수명을 기준으로 작성이 된다
- 첫글자가 대문자는 불가능하고, 중간에 언더바("_") 같은 기호도 인식이 불가능하다.
- DB 테이블에서 칼럼명과 매핑 시키기 위해 @Column의 속성을 사용하면 된다. @Column(name = "Last_Name")

##### User Repository

- 대문자로 키워드 분리하여 자동으로 인식한다
- 따라서 대소문자에 유의하여 작성하여야 한다.
- 또한 매개변수명은 어떤 것을 해도 순서대로 들어오지만, 명확한 코드를 위해 칼럼명으로 작성을 하는게 좋다

```java
@Repository
public instance UserRepository extends JpaRepository<User, Long> {

  List<User> findFirst5ByLastnameAndFirstnameOrderByIdDesc(String lastName, String firstName);

  Boolean existsByStartDateLessThanEqual(LocalDateTime startDate);

  long countByFirstnameIgnoreCaseLike(String firstName);
}
```

- 첫번째 메소드는 lastname과 firstname이 입력받은 값과 일치하는 user들을 id 역순으로 정렬하여 5개 데이터만 가져온다
- 서비스에서 return 값으로 
- userRepository.findFirst5ByLastnameAndFirstnameOrderByIdDesc("KillDong", "Hong");
- 실제 수행된 쿼리

```sql
select *
from user
where last_name = ?
and first_name = ?
order by id desc limit 5
```

- 두번째 메소드는 입력받은 날짜보다 startDate가 적거나 같은(<=) 데이터가 있다면 true를 없다면 false를 리턴하는 메소드이다
- userRepository.existsByStartDateLessThanEqual(LocalDateTime.now());

- 세번째는 대소문자를 무시하고 Like 쿼리를 날려서 해당되는 데이터의 수를 가져오는 메소드이다.
- 입력변수에 "%"를 붙여 원하는 위치에 포함하여 실행한다
- userRepository.countByFirstnameIgnoreCaseLike("kim%");

### 쿼리 Subject 키워드
- 메소드 처음 시작 위치에 오는 키워드로 어떤 작업을 할 것이며, 어떤 데이터 타입을 리턴할지 결정한다.
- findBy : List<User>와 같이 Collection 타입으로 리턴되나, 조회하는 대상이 Primary Key라면 아래와 같이 단일 객체로 리턴된다
  - Optional<User> user = userRepository.findById((long)1);
- findFirst(숫자)By : findFirstBy..과 같이 숫자가 없다면 제일 첫번째 데이터 1개가 리턴되고, 숫자가 있다면 Collection 타입으로 리턴된다.
- existBy : boolean 타입으로 리턴
- countBy : int/long 등으로 해당되는 데이터 수 리턴
- deleteBy : void 타입 혹은 int/long 등으로 삭제된 데이터 수 리턴

### 메소드명 내 키워드
- 메소드명 By 뒤에 작성되는 키워드로 SQL의 Where절 내 명령어와 같은 역할이다
<table>
  <tr>
    <th>키워드</th>
    <th>샘플</th>
    <th>JPQL</th>

  <tr>
    <td>Distinct</td>
    <td>findDistinctByLastnameAndFirstname</td>
    <td>select distinct ... where x.lastname = ?1 and x.firstname = ?2</td>

  <tr>
    <td>And</td>
    <td>findByLastnameAndFirstname</td>
    <td>..where x.lastname = ?1 and x.firstname = ?2</td>

  <tr>
    <td>Or</td>
    <td>findByLastnameOrFirstname</td>
    <td>..where x.lastanem = ?1 or x.firstname = ?2</td>

  <tr>
    <td>Is, Equals</td>
    <td>findByFirstname, findByFirstnameIs, findByFirstnameEquals</td>
    <td>..where x.firstname = ?1</td>

  <tr>
    <td>Between</td>
    <td>findByStartDateBetween</td>
    <td>..where x.startDate between ?1 and ?2</td>

  <tr>
    <td>LessThan</td>
    <td>findByAgeLessThan</td>
    <td>..where x.age < ?1</td>

  <tr>
    <td>LessThanEqual</td>
    <td>findByAgeLessThanEqual</td>
    <td>..where x.age <= ?1</td>

  <tr>
    <td>GreaterThan</td>
    <td>findByAgeGreaterThan</td>
    <td>..where x.age > ?1</td>

  <tr>
    <td>GreaterThanEqual</td>
    <td>findByAgeGreaterThanEqual</td>
    <td>..where x.age >= ?1</td>

  <tr>
    <td>After</td>
    <td>findByStartDateAfter</td>
    <td>..where x.startDate > ?1</td>

  <tr>
    <td>Before</td>
    <td>findByStartDateBefore</td>
    <td>..where x.startDate < ?1</td>

  <tr>
    <td>IsNull, Null</td>
    <td>findByAge(Is)Null</td>
    <td>..where x.age is null</td>

  <tr>
    <td>IsNotNull, NotNull</td>
    <td>findByAge(Is)NotNull</td>
    <td>..where x.age is not null</td>

  <tr>
    <td>Like</td>
    <td>findByFristnameLike</td>
    <td>..where x.firstname like ?1</td>

  <tr>
    <td>NotLike</td>
    <td>findByFirstnameNotLike</td>
    <td>..where x.firstname not like ?1</td>

  <tr>
    <td>StartingWith</td>
    <td>findByFirstnameStartingWith</td>
    <td>..where x.firstname like ?1(parameter bound with appended %)</td>

  <tr>
    <td>EndingWith</td>
    <td>findByFirstnameEndingWith</td>
    <td>..where x.firstname like ?1(parameter bound with prepended %)</td>

  <tr>
    <td>Containing</td>
    <td>findByFirstnameContaining</td>
    <td>..where x.firstname like ?1 (parameter bound wrapped in %)</td>

  <tr>
    <td>OrderBy</td>
    <td>findByAgeORderByLastnameDesc</td>
    <td>..where x.age = ?1 order by x.lastname desc</td>

  <tr>
    <td>Not</td>
    <td>findByLastnameNot</td>
    <td>..where x.lastname <> ?1</td>

  <tr>
    <td>In</td>
    <td>findByAgeIn(Collection<Age>ages)</td>
    <td>..where x.age in ?1</td>
  
  <tr>
    <td>NotIn</td>
    <td>findByAgeNotIn(Collection<Age>ages)</td>
    <td>..where x.age not in ?1</td>

  <tr>
    <td>TRUE</td>
    <td>findByActiveTrue()</td>
    <td>..where x.active = true</td>

  <tr>
    <td>FALSE</td>
    <td>findByActiveFalse()</td>
    <td>..where x.active = false</td>

  <tr>
    <td>IgnoreCase</td>
    <td>findByFirstnameIgnoreCase</td>
    <td>..where UPPER(x.firstname) = upper(?1)</td>

