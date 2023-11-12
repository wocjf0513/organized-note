

> 예외를 트리거하는 타이밍의 차이가 있습니다.

- 가능하면 **@NotNull**을 쓰는것을 추천합니다.
- `nullable = false` 옵션의 경우, DB에 Insert **쿼리를 쏘는 상황**에서야 예외를 터트리게 됩니다.
- 하지만 `@NotNull`의 경우 엔티티를 만드는것 자체는 가능하나, Repository에 잘못된 엔티티를 저장할때, [ConstraintViolationException](https://docs.oracle.com/database/121/JAXML/javax/jcr/nodetype/ConstraintViolationException.html)를 터뜨리므로 더 빠른 순간에 문제를 잡아낼 수 있습니다.