---
published : true
title : update
date : 2024-02-07 +09:00
categories : [Spring, JPA]
tags : [
  JPA,
]
---
<!-- ![](/assets/img/Spring/aaaa.png){:style="border:1px solid #eaeaea; border-radius: 7px; padding: 0px;" } -->
<!-- ![](/assets/img/alg/4-1.png){:style="width:1000px" } -->

### Update
- JPA에는 수정과 관련된 메서드가 없다
- 그렇다면 어떻게 해야하는 것일까??
- 방법은 두가지가 있다. 
- 1) 더티체킹(변경감지)
- 2) 벌크연산

#### 더티체킹(변경감지)
- Entity를 조회하여 조회된 Entity 데이터를 변경만 하면 DB에 자동으로 반영이 되도록 한 것이다.
- JAP는 영속성 콘텍스트를 생성하여 DB와 유사하게 Entity의 Life Cycle을 관리한다.
- JAP는 영속성 콘텍스트에 Entity를 보관할 때 상태를 저장한다. 이를 스탭샷이라고 한다
- 영속성 콘텍스트가 flush(영속성 콘텍스트의 변경 내용을 DB에 반영하는 것)되는 시점에 스냅샷과 현재 Entity의 상태를 비교한다
- 이후 변경된 필드들을 이용하여 쓰기 지연 SQL 저장소에 Update 쿼리를 생성하여 쌓아둔다
- 모든 작업이 끝나고 트랜잭션을 커밋으로 처리할 때, 쓰기 지연 SQL 저장소에 있는 쿼리들을 DB에 전달하여 Update한다
- 이것은 Service Layer에서 진행한다

#### 벌크연산(Querydsl)
- Repository 단에서 시행한다.
- 수정할 데이터를 update 퀄에서 선언한다.
- .set을 이용하여 Dto로 전달받은 값을 꺼낸다
- .where 절을 이용하여 조건을 건다(선택)
- QueryDsl로 해당 쿼리를 날리고 update한 다음 select 해보면 수정된 값이 반영되지 않는다.
- 이유는 영속성 컨텍스트에 값이 남아있기 때문이다
- 이때는 EntityManager를 추가해주고
- em.clear(); 와 em.flush(); 를 해주어야 한다
- 벌크연산의 경우 리턴 값을 숫자만 리턴할 수 있다
- 따라서 조회 쿼리를 한 번 더 만들어야 한다
- 지정되지 않은 값들은 null로 들어가게 된다.

#### JPA와 Querydsl의 차이점
- JPA스럽게 사용하는 방법은 더티체킹이라고 한다
- JAP(더티체킹) 특징
  - 트랜잭션 커밋 시점에 스냅샷 비교를 통한 update 쿼리가 만들어진다
  - 쓰기 지연 SQL 저장소에 있던 다른 쿼리들과 같이 DB로 전송된다. 이는 성능을 향상시킨다
  - DB에 존재하는 모든 값들이 채워지지 않아도 null로 치환되지 않는다
- Querydsl(벌크연산) 특징
  - Querydsl의 update는 벌크 연산용으로 영속선 컨텍스트에 반영되지 않는 문제점
  - 혹은 다량의 데이터를 수정하고 반환해야 할 경우 사용한다
  - DB에 존재하는 모든 값들이 채워지지 않으면 null로 치환된다
  - 후속 처리가 필요할 수 있다(추가적인 조회 쿼리)

```java
// JAP 더티체킹 (Service Layer)
@RequiredArgsConstructor
@Transactional
public class BoardService{
  private final EntityManager em;  

  public Team updateTeamInfo(TeamInfoUpdateDto teamInfoUpdateDto){
    Team team = em.find(Team.class, teamInfoUpdateDto.getTeamId());

    team.setTeamName(teamInfoUpdateDto.getChangeTeamName());
    team.setDetailIntro(teamInfoUpdate.getChangeDetailIntro());

    return team;
  }
}
```

```java
//Querydsal(벌크연산) (Repository Layer)

@RequiredArgsConstructor
public class BoardRepository{

  private final EntityManager em;

  public long updateTeamInfo(TeamInfoUpdateDto teamInfoUpdateDto){
    return queryFactory
          .update(team)
          .set(team.teamName, teamInfoUpdateDto.getTeamName())
          .where(team.id.eq(teamInfoUpdateDto.getTeamId()))
          .execute();
    
    em.clear();
    em.flush();
  }
}
```
