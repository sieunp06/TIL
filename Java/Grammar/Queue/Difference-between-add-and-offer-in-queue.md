# Queue에서 add와 offer의 차이
### 📌offer
> Inserts the specified element into this queue if it is possible to do so immediately without violating capacity restrictions.

`offer`는 queue의 맨 뒤에 값을 넣는다. 

`offer` 같은 경우, queue에 값을 넣는 것을 성공했을 때 `true`를 반환하고 실패했을 때 `false`를 반환한다.

### 📌add
> Inserts the specified element into this queue if it is possible to do so immediately without violating capacity restrictions, returning true upon success and throwing an IllegalStateException if no space is currently available.

`add`는 `offer`과 동일하게 queue 맨 뒤에 값을 넣는다.

`offer`과 동일하게 queue에 값을 넣는 것을 성공했을 때 `true`를 반환하지만, 실패했을 경우 `IllegalStateException`를 발생시킨다.

용량 제한이 있는 queue를 사용할 때에는 `offer`보다 `add`를 사용하는 것이 더 추천된다.

-----
## 💎 References
- [Java API Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)