# getParameter() �� getAttribute() �׸��� getReader()�� ����



### Ŭ���̾�Ʈ ��û������ ����

```java
request.getParameter();
```

### ���������� request�� ���� ��ų� ������

```java
request.setAttribute();
request.getAttribute();
```

### ajax �⺻ data -> json�� �������� �������� ����� �����ϴµ�

#### gson���̺귯���� ����ϸ� �ޱ� ����.

```java
Map<String, String> map = new HashMap<String, String>();
BufferedReader br = req.getReader();
Gson gson = new Gson();
map = gson.fromJson(br, map.getClass());
```

