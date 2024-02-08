---
published : true
title : save와 saveAndFlush
date : 2024-02-08 +09:00
categories : [Spring, JPA]
tags : [
  JPA,
]
---
<!-- ![](/assets/img/Spring/aaaa.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->
<!-- ![](/assets/img/alg/4-1.png){:style="width:1000px" } -->

### save와 saveAndFlush
- 더티체킹(@Transactional)이 아닌 merger 방법이다
- merger 방법으로는 데이터를 영속하는 메서드인 save 메서드를 사용하는 것이다.
- save나 saveAndFlush를 쓰는 경우나 둘다 set을 할 경우 update가 되지 않는다
- @Transactional 어노테이션을 붙이는 경우에 save와 saveAndFlush를 비교하면 똑같이 메서드 종료 후 update가 일어난다.
- 그렇다면 여러번의 save와 saveAndFlush 호출이 있다면??

```java

// save 코드
Member member = Member.create(memberDTO);
member = memberRepository.save(member);
member.setName("test1");
member = memberRepository.save(member);
member.setName("test2");
member = memberRepository.save(member);
member.setName("test3W");

// saveAndFlush 코드
Member member = Member.create(memberDTO);
member = memberRepository.saveAndFlush(member);
member.setName("test1");
member = memberRepository.saveAndFlush(member);
member.setName("test2");
member = memberRepository.saveAndFlush(member);
member.setName("test3W");
```

- save를 사용할 경우 쿼리 출력이 insert와 마지막 update 하나만 나오게 된다
- 즉 db에는 test3만 저장이 된 것이다.
- saveAndFlush를 사용할 경우 쿼리 출력이 insert와 모든 update가 나오게 된다
- 즉 db에는 test1, test2, test3이 순서대로 update 된 것이다.
- 여기서 flush는 db에 업데이트 flush 개념 보다는 영속 컨텍스트 안에서 캐싱 공간으로 flush를 하는 과정이라고 생각하면 된다.
- saveAndFlush로 업데이트를 하면 매번 query를 캐싱 공간에 보내고 그걸 트랜잭션 종료 시점에 db 업데이트를 하고
- save를 하면 마지막에 컨텍스트에 존재하는 형태의 데이터를 query로 만들어서 트랜잭션 종료 시점에 db 업데이트를 하는 것이다.
- saveAndFlush 보다는 save가 성능적으로 좀 더 유리할 수 있다고 생각이 든다
