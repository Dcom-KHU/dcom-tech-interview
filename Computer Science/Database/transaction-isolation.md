## 트렌젝션 고립레벨은 뭐고, 이걸 왜 나눠 놓은거죠?
일단 **고립레벨 (Isolation level)** 에 대해서 설명 드리겠습니다. **고립레벨**은 트랜젝션에서 일관성 없는 데이터를 허용하도록 하는 수준을 의미합니다. 윗 글에서 **ACID**에 대해서 이야기 했습니다. 그래서 우리는 각 트랜젝션이 독립적인 수행이 가능하도록, 특정 수준의 **Locking** 이 필요 한 것 입니다.

하지만, 모든 요청에 대해서 **Locking**을 걸게 되면, 많이 난감해 지겠죠? 각 요청이 다른 요청이 끝날 때 까지 기다리도록, 순서대로 실행 되게 하면 데이터베이스의 성능은... 뭐 당연히 좋아지지 않을 것 입니다.

그래서 효율적인 **Locking**을 걸기 위해서, **트렌젝션 고립레벨**을 나눠 놓은 것 이지요.

### Isolation level 종류
#### 1. Read Uncommitted (레벨 0)
해당 격리수준에서는 어떤 트랜잭션의 변경내용이 **Commit**이나 **Rollback**과 상관없이 다른 트랜잭션에서 보여집니다. 사실상 **트랜잭션 고립이 이루어지지 않은 상태**로, **Dirty Read**가 일어날 가능성이 높습니다! 당연히 이 단계에서는 데이터베이스의 일관성을 유지하는 것이 불가능합니다.

**Dirty Read**는 다음을 말합니다. 커밋되지 않은 **수정중인 데이터**를 다른 트랜잭션에서 읽을 수 있도록 허용할 때 발생하는 현상입니다. 예를 들어보면 다음과 같습니다.

- A 트랜잭션에서 ID 1을 가진 유저의 별명을 **AAA**에서 **BBB**로 바꾸었습니다.
- B 트랜잭션에서 ID 1을 가진 유저의 별명을 조회 했습니다.
    - 여기서 A 트랜잭션에서 **Rollback**을 했다면? B 트랜잭션은 유저의 별명을 **BBB로 알고 있기 때문에**, 그래도 로직을 계속 수행합니다. 그러면 데이터가 서로 모순 없이 일관되게 일치해야 함을 의미하는, **데이터 정합성에 문제**가 생기는 것 이지요.

#### 2. Read Committed (레벨 1)
트랜잭션이 수행되는 동안 해당 데이터에 **Shared Lock**이 걸려, 다른 트랜잭션이 접근 할 수 없어 **대기**하게 됩니다. SQL 서버가 기본적으로 사용 합니다. **Commit이 이루어진 트랜잭션만 조회 할 수 있게**되는 것이지요. 단, 이도 **Non-Repeatable Read** 문제를 초래 합니다. 예를 들어 보죠.

- B 트랜잭션이 실행 되고, 값 10을 읽었습니다.
- A 트랜잭션이 실행 되고, 값 10을 11로 바꾼 후, **Commit** 되었습니다.
- 아직 끝나지 않은 트랜잭션 B가 또 값을 읽었는데 값이 11이 되어 있었습니다.

이는 **하나의 트랜잭션내에서 똑같은 SELECT 쿼리를 실행했을 때 항상 같은 결과**를 가져와야 하는 **Repeatable Read**의 정합성에서 어긋나게 됩니다.

#### 3. Repeatable Read (레벨 2)
트랜잭션이 완료될 때까지 **SELECT 문장이 사용하는 모든 데이터에 Shared Lock**이 걸립니다. 트랜잭션이 범위 내에서 조회한 데이터 내용이 항상 **동일함**을 보장하며, 다른 사용자는 **트랜잭션 영역에 해당되는 데이터에 대한 수정이 불가능**합니다. 단, 이 마저도 **Phantom Read** 현상이 일어 나는데, 수정을 막았지, **추가를 막지는 않았습니다.** 그래서, 또 다른 레코드가 읽히는 거죠.

#### 4. Serializable (레벨 3)
이건 그냥 "수정, 추가 모두 막는 것" 입니다. 가장 단순하고, 가장 엄격하고, 가장 느립니다.