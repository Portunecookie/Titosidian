
### 논의사항
---
Users 테이블과 UserProfile 테이블을 두개로 나눴는데 성능적인면으로 봤을때 테이블을 분리하는게 나을까? 아니면 두 테이블을 User 테이블로 합치는게 나을까?

CREATE TABLE `Users` (
	`userId`	NOTNULL	NOT NULL,
	`userName`	NOTNULL	NULL,
	`userNickName`	NOT NULL	NULL,
	`userEmail`	NULLABLE	NULL,
	`password`	NOT NULL	NULL,
	`createdAt`	NOT NULL	NULL,
	`updatedAt`	NOT NULL	NULL
);

CREATE TABLE `UserProfile` (
	`profileId`	NOTNULL	NOT NULL,
	`userId`	NOTNULL	NOT NULL,
	`profilePicture`	NULLABLE	NULL,
	`winningRate`	NOTNULL	NOT NULL,
	`debateTotalCount`	NOT NULL	NULL,
	`debateVictoryCount`	NOT NULL	NULL,
	`debateDefeatCount`	NOT NULL	NULL,
	`createdAt`	NOT NULL	NULL,
	`updatedAt`	NOT NULL	NULL
);


### 논의 과정
---
두 테이블을 분리할지 합칠지 결정하는 데에는 다양한 요소를 고려해야 합니다. 성능적인 면과 데이터베이스 설계의 원칙을 함께 고려해보겠습니다.

### 분리된 테이블의 장점

1. **정규화**:
    
    - 데이터의 중복을 줄이고, 데이터 무결성을 유지할 수 있습니다.
    - 사용자 정보와 사용자 프로필 정보를 별도로 관리함으로써, 서로 다른 목적의 데이터를 더 효율적으로 관리할 수 있습니다.
2. **성능**:
    
    - 프로필 정보를 자주 업데이트해야 하는 경우(예: 토론 승률 등), 메인 사용자 테이블에 영향을 주지 않고 해당 테이블만 업데이트할 수 있어 성능 향상에 도움이 됩니다.
    - 조회 시 필요한 컬럼만을 선택적으로 가져올 수 있어, 불필요한 데이터를 읽지 않음으로써 I/O 비용을 절감할 수 있습니다.
3. **유지보수**:
    
    - 테이블이 작으면 인덱스 관리와 테이블 스캔이 더 효율적입니다.
    - 각 테이블의 역할이 명확하여 코드의 가독성과 유지보수성이 향상됩니다.

### 단일 테이블의 장점

1. **조인 비용 감소**:
    
    - 하나의 테이블에 모든 데이터를 저장하면 조인 연산을 피할 수 있어, 단순 조회 시 성능이 더 좋을 수 있습니다.
    - 복잡한 쿼리를 작성할 필요 없이 단일 쿼리로 모든 데이터를 조회할 수 있습니다.
2. **단순성**:
    
    - 데이터베이스 구조가 단순해져 이해하고 관리하기 쉬워집니다.

### 성능 및 설계 측면에서의 고려사항

- **조회 패턴**: 자주 조회하는 데이터와 그렇지 않은 데이터를 분리하여 I/O 비용을 줄일 수 있는지 고려합니다.
- **업데이트 빈도**: 프로필 정보가 자주 변경되는 경우, 이를 별도 테이블로 관리하여 메인 테이블의 잠금을 최소화할 수 있습니다.
- **데이터 크기**: 한 테이블에 너무 많은 컬럼이 있으면 테이블이 비대해지고 성능 저하를 초래할 수 있습니다.

### 결론

일반적인 상황에서는 다음과 같은 결론을 도출할 수 있습니다:

- **테이블 분리**가 적절한 경우:
    
    - 데이터 정규화를 통해 데이터 무결성을 유지하고 중복을 최소화하려는 경우.
    - 특정 컬럼(예: 프로필 정보) 업데이트가 빈번하고, 이로 인해 메인 사용자 정보에 영향을 미치지 않도록 하려는 경우.
    - 조회 패턴이 명확히 분리되어 있을 때(예: 사용자 정보와 프로필 정보를 별도로 조회하는 경우가 많을 때).
- **단일 테이블**이 적절한 경우:
    
    - 조인이 잦고, 단일 쿼리로 데이터를 조회하는 것이 더 효율적일 때.
    - 데이터 크기가 작고, 업데이트가 드물어 테이블 잠금이 큰 문제가 되지 않을 때.

따라서, **대부분의 경우 테이블을 분리하는 것이 더 나은 설계**가 될 가능성이 높습니다. 특히 사용자 프로필 정보와 같은 데이터는 업데이트가 빈번할 수 있기 때문에, 이를 별도의 테이블로 관리함으로써 메인 사용자 정보 테이블의 성능과 무결성을 유지하는 것이 좋습니다.




### 결론
---
User와 UserProfile 을 하나로 합치면 구조는 단순해져서 이해하고 관리하긴 쉬워질 것 같아욥 

그러나 ! 프로필정보를 자주 업데이트하는 경우(ex. 토론 승률) 메인 회원 정보에 대한 테이블에는 영향을 주지 않고 프로필 테이블만 업데이트하면 되니 성능 향상에 도움이 될것같고 .. (토론 승률이 순식간에 몰아서 오르는게 아니라서 큰의미없을라나?) 

어쨋든 조회시 필요한 컬럼만 선택적으로 가져올 수 있으니 I/O 비용은 절감할 수있을듯해보임. 

결론 -> Users와 UserProfile 테이블 두개로 가져갈게요




  
---
  
| @eunsoA, 2024.07.12.