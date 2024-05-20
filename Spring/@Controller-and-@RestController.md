## @Controller와 @RestController의 차이
### @Controller
`@Controller`는 주로 View를 반환하는 용도로 사용한다.
```java
@Controller
@RequestMapping("books")
public class SimpleBookController {

    @GetMapping(value = "/users/detailView")
    public String detailView(Model model, @RequestParam String userName){
        User user = userService.findUser(userName);
        model.addAttribute("user", user);
        return "/users/detailView";
    }
}
```

하지만 `@Controller`를 사용하여 데이터를 반환해야 할 때에는 아래와 같이 `@ResponseBody` 어노테이션을 사용하면 된다.

```java
@Controller
@RequestMapping("books")
public class SimpleBookController {

    @GetMapping("/{id}", produces = "application/json")
    public @ResponseBody Book getBook(@PathVariable int id) {
        return findBookById(id);
    }

    private Book findBookById(int id) {
        // ...
    }
}
```

### @RestController
`@RestController`는 `@Controller`에 `@ResponseBody`가 추가된 어노테이션이라고 보면 된다.

때문에 `@RestController`의 주용도는 JSON 형태로 객체 데이터를 반환하는 것이다.

```java
@RestController
@RequestMapping("books-rest")
public class SimpleBookRestController {
    
    @GetMapping("/{id}", produces = "application/json")
    public Book getBook(@PathVariable int id) {
        return findBookById(id);
    }

    private Book findBookById(int id) {
        // ...
    }
}
```

위 예시와 같이 `@RestController`를 사용하면 `@RequestBody` 어노테이션을 사용하지 않아도 된다.

---
## References
- [https://www.baeldung.com/spring-controller-vs-restcontroller](https://www.baeldung.com/spring-controller-vs-restcontroller)
- [https://mangkyu.tistory.com/49](https://mangkyu.tistory.com/49)