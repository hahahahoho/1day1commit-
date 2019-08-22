# getParameter() 와 getAttribute() 그리고 getReader()의 차이



### 클라이언트 요청정보를 받음

```java
request.getParameter();
```

### 서버내에서 request에 값을 담거나 꺼낼때

```java
request.setAttribute();
request.getAttribute();
```

### ajax 기본 data -> json을 받으려면 여러가지 방법이 존재하는데

#### gson라이브러리를 사용하면 받기 쉽다.

```java
Map<String, String> map = new HashMap<String, String>();
BufferedReader br = req.getReader();
Gson gson = new Gson();
map = gson.fromJson(br, map.getClass());
```

