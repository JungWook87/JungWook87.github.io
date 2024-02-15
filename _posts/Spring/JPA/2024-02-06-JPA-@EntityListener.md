---
published : true
title : EntityListener 어노테이션
date : 2024-02-06 +09:00
categories : [Spring, JPA]
tags : [
  JPA,
]
---
<!-- ![](/assets/img/Spring/aaaa.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->
<!-- ![](/assets/img/alg/4-1.png){:style="width:1000px" } -->

### @EntityListener
- EntityListener는 엔티티의 변화를 감지하고 테이블의 데이터를 조작하는 일을 한다
- Column 값이 수정되는 것에 대해서 반복적인 코드를 일일이 작성하면 실수가 발생할 수 있기 때문에 EntityListener를 사용하면 좋다

- JPA에서는 아래의 7가지 Event를 제공한다
  - @PrePersist : Persist(insert) 메서드가 호출되기 전에 실행되는 메서드
  - @PreUpdate : merge 메서드가 호출되기 전에 실행되는 메서드
  - @PreRemove : Delete 메서드가 호출되기 전에 실행되는 메서드
  - @PostPersist : Persist 메서드가 호출된 이후에 실행되는 메서드
  - @PostUpdate : merge 메서드가 호출된 이후에 실행되는 메서드
  - @PostRemove : Delete 메서드가 호출된 이후에 실행되는 메서드
  - @PostLoad : Select 조회가 일어난 직후에 실행되는 메서드

<hr>

- 사용 예제

```java
@PrePersist
public void prePersist(){
  this.createdAt = LocalDateTime.now();
  this.updatedAt = LocalDateTime.now();
}

@PreUpdate
public void preUpdate(){
  this.updatedAt = LocalDateTime.now();
}
```

- 위와 같이 데이터가 업데이트, 생성되는 시간을 기록하는 속성의 경우는 거의 모든 Entity table에 대해서 적용될 것이다.
- 이러한 메소드를 매번 반복되게 추가하는 것을 비효율적이다.
- @EntityListeners를 사용한 클래스를 만들고 반복되는 메소드들을 모아놓자
- 그 후 Listener class를 호출하여 사용하면 코드의 가독성이 증가하고 코드의 중복도 크게 줄일 수 있다

```java
public class MyEntityListener {
  // 객체 내에서 만드는 Event가 아니므로 객체애 대한 정보가 없다.
  // Object를 parameter로 받고 Auditable instance인지 확인해야 한다

  // Auditable은 EventListener를 구현하기 위해 반복되어 사용될 메소드들을 묶어 직접 생성한 interface 이다
  @PrePersist
  public void prePersist(Object o){
    if(o instanceof Auditable){
      ((Auditable) o).setCreatedAt(LocalDateTime.now());
      ((Auditable) o).setUpdatedAt(LocalDateTime.now());
    }
  }

  @PreUpdate
  public void prePersist(Object o){
    if(o instanceof Auditable){
      ((Auditable) o).setUpdatedAt(LocalDateTime.now());
    }
  }
}
```

- 해당 MyEntityListener를 Entity 객체에 적용하는 방법은 class 위에 @EntityListeners(value = MyEntityListener.class) 를 추가해주면 된다.

### AuditingEntityListener.class
- 위의 예제로 다뤘던 데이터가 업데이트, 생성됨에 따라 createdAt, updatedAt를 업데이트 하는 것은 대부분의 DB에서 사용하게 된다.
- 그래서 JPA는 이러한 기능을 기본적으로 제공하고 있다.
- 이것이 AuditingEntityListener.class 이다

<hr>

- 사용법
1. AuditingEntityListener.class를 사용하기 위해서는 ~Application.java 파일에 @EnableJpaAuditing 를 추가해주어야 한다
2. Entity로 사용할 객체의 @EntityListeners 의 value 값에 AuditingEntityListener.class를 넣어준다. @EntityListeners(AuditingEntityListener.class)
3. Auditing에서 데이터가 생성될 때 업데이트 해주길 바라는 변수에 @CreatedDate를 붙인다
4. Auditing에서 데이터가 업데이트 될 때, 업데이트 해주길 바라는 변수에 @LastModifiedDate를 붙인다
5. 그 외에 @CreatedBy, @LastModifiedBy도 존재한다.
