---
published : true
title : MappedSuperClass 어노테이션
date : 2024-02-06 +09:00
categories : [Spring, JPA]
tags : [
  JPA,
]
---
<!-- ![](/assets/img/Spring/aaaa.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->
<!-- ![](/assets/img/alg/4-1.png){:style="width:1000px" } -->

### @MappedSuperClass
- 부모 클래스는 테이블과 매핑하지 않고 부모 클래스를 상속받는 자식 클래스에게 매핑 정보만 제공해준다
- @Entity 어노테이션을 사용하면 실제 테이블과 매핑이되지만 @MappedSuperClass는 실제 테이블에 매핑되지 않는다.
- 위의 클래스는 직접 생성해서 사용할 일이 거의 없으므로 추상클래스로 만드는 것이 좋다

1. 객체
- Member 클래스
  - Long id
  - String name
  - String email
- Seller 클래스
  - Long id
  - String name
  - String shopName

<hr>

- 위 처럼 공통 속성이 존재하면

<hr>

- BaseEntity 클래스
  - Long id
  - String name
- Member 클래스
  - String email
- Seller 클래스
  - String shopName

<hr>

- 공통 속성을 상속한다
- 하지만 BaseEntity 클래스는 DB에서 테이블로 만들 필요가 없기 때문에 테이블과 매핑을 할 수 없다

```java
@MappedSuperclass
public abstract class BaseEntity{
  @Id
  @GeneratedValue
  private Long id;

  @Column(nullable = false)
  private String name;
}

@Entity
public class Member extends BaseEntity{
  @Column
  private String email;
}

@Entity
public class Seller extends BaseEntity{
  @Column
  private String shopName;
}
```

- BaseEntity의 id, name 두 공통 속성을 부모클래스로 모으로 객체 상속 관계로 만들었다.
- 자식 엔티티에서 공통으로 사용되는 정보만 제공하면 된다
- 만약 부모로부터 물려받은 매핑 정보를 재정의 하려면 @AttributeOverrides, @AttributeOverride를 사용한다
- 만약 부모와의 연관관계를 재정의 하려면 @AssociationOverrides, @AssociationOverride를 사용한다

```java
@Entity
@AttributeOverrides({
  @AttributeOverride(name = "ID", column = @Column(name = "MEMEBER_ID"))
  @AttributeOverride(name = "NAME", column = @Column(name = "MEMEBER_NAME"))
})
public class Member extends BaseEntity{
  @Column
  private String email;
}
```

- ORM에서 이야기하는 진정한 상속 매핑은 객체 상속을 데이터베이스의 슈퍼타입 서브타입 관계와 매핑하는 것이니 헷갈리지 말자
