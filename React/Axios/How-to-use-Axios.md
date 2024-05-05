### 목차
- [Axios](#axios)
  - [GET](#get)
  - [POST](#post)
  - [DELETE](#delete)
  - [PUT \& PATCH](#put--patch)

---
## Axios
`Axios`는 Promise API를 사용하는 HTTP 비동기 통신 라이브러리이다.

- 운영 환경에 따라 브라우저 간 XMLHttpReqeust 객체 또는 Node.js의 HTTP API를 사용한다.
- ES6 Promise API를 사용한다.
- HTTP Methods를 사용한다.
- Reqeust의 응답을 자동으로 JSON 형태로 만든다.
  
### GET
`GET` 방식은 서버에서 어떠한 데이터를 받아서 보여줄 때 사용한다.

이때, `Reponse`는 JSON 형태이다.

```js
const getLists = async () => {
    const { data } = await axios.get("요청할 주소");
};
```
```js
const postList = async () => {
	try {
		const { data } = await axios.get("요청할 주소");
		console.log(data);
	} catch (error) {
		console.log(error);
	}
}
```
```js
const getLists = async () => {
	axios.get("요청할 주소")
		.then(res => {
			console.log(res.data);
		}).catch(err => {
			console.log(err);
		});
}
```

### POST
`POST`는 새로운 리소스를 생성할 때 사용한다.

```js
const postList = async () ⇒ {
	const { data } = await axios.post(
		"요청할 주소",
		"보낼 값(객체)",
		{
			headers: {
				'Content-type': 'application/json',
				'Accept': 'application/json'
			}
		}).then(res => {
			console.log(res.data);
		}).catch(err => {
			console.log(err);
		}
	);
}
```
```js
const postList = async () => {
	try {
		const { data } = await axios.post("요청할 주소", {
			headers: {
				'Content-type': 'application/json',
				'Accept': 'application/json',
			}
		});
		console.log(data);
	} catch (error) {
		console.log(error);
	}
}
```

### DELETE
`DELETE`는 데이터베이스의 내용을 삭제할 때 사용한다.

```js
const deleteList = async (listId) => {
	const { data ] = await axios.delete('요청할 주소/${listId}');
}
```

### PUT & PATCH
`PUT`과 `PATCH`는 데이터베이스에 저장된 내용을 갱신하는 목적으로 사용된다.

- PUT: 보낸 내용으로 리소스를 덮어쓴다.
- PATCH: 보낸 내용으로 리소스의 일부를 변경한다.

```js
axios.put("요청할 주소", "덮어쓸 값", config);
axios.patch("요청할 주소", "일부만 바꿀 값", config);
```