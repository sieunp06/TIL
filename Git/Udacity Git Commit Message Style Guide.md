# Udacity Git Commit Message Style Guide
## 커밋 메시지의 구조
커밋 메시지는 크게 세 부분으로 구성됨.
```
type: Subject

body(Option)

footer
```
### Type
- `feat`: 새로운 기능 추가
- `fix`: 버그 수정
- `docs`: 문서 수정
- `style`: 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
- `refactor`: 리팩토릭
- `test`: 테스트 코드, 리팩토링 테스트 코드 수정
- `chore`: 빌드 업무 수정, 패키지 수정

### Subject
- 50자를 넘으면 안됨.
- 대문자로 시작하고, 마침표로 끝나지 않아야 함.

### Body
- 선택사항; 커밋에 설명이 부가적으로 더 필요하면 사용.
- Subject와 Body 사이에 빈 줄을 두고 72자 이내로 작성해야 함.

### Footer
- 선택사항; issue tracker ID를 참조하는 데에 사용.

## 예시
```
feat: Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequences of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```

# References
- [Udacity Git Commit Message Style Guide](http://udacity.github.io/git-styleguide/)