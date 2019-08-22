# Gson 라이브러리를 통한 json파싱

```java
Gson gson = new Gson();
//		List<Map<String, Object>> list = cubridTestDAO.getTest();
		// Map<String, String> map = new HashMap<String, String>();
		// List<Map<String, Map<String, String>>> lists = new ArrayList();
		Map<String, Map<String, String>> map2 = new HashMap<>();
		// map = gson.fromJson(param, map.getClass());
		// lists = gson.fromJson(data, lists.getClass());
		map2 = gson.fromJson(data, map2.getClass());
		System.out.println(map2.get("CLIENT_INFO").get("account_name"));
		System.out.println(map2.get("CLIENT_INFO").get("status"));
```

java에서 gson을 이용한 파싱방법은 매우 쉽다. json 형태에 맞춰 list또는 map객체를 생성하여 데이터를 받으면 된다.

위에 예제는 객체안에 키와 벨류값으로 다시 객체가 들어있는 형태.

