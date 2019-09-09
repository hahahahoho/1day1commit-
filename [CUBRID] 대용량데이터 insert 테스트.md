# 대용량 데이터 INSERT TEST CASE

최근 프로젝트에서 로그를 db에 저장하는 과정에서 대용량 데이터를 넣어야 하는 상황이 생겼습니다.

##### 2개의 방법으로 테스트를 진행. 

*4천건의 데이터를 한번에 insert하는것을 기준.

#### 결과 : 1번 방법인 value값에 INSERT INTO table VALUES ('값'), ('값'), ('값'); 으로 넣는 방법이 훨씬 빠르다.

1.array형식으로 insert하는 방법

```java
@Test
	public void contextLoads() {
		ArrayList<ArrayList<String>> arr = new ArrayList<>();
		try {
			Class.forName("cubrid.jdbc.driver.CUBRIDDriver");
			Connection conn = DriverManager.getConnection("jdbc:CUBRID:175.207.13.165:30000:whitedefense:::?charSet=utf8", "dba", "");
			long beforeTime = System.currentTimeMillis();
			for (int i = 1; i <= 4000; i++) {
				ArrayList<String> arr2 = new ArrayList<>();
				for (int j = 0; j < 10; j++) {
					arr2.add("'test" + i + "'");
				}
				arr2.add("DEFAULT");
				arr.add(arr2);
			}
			String sql = "INSERT INTO tbl_insert_test VALUES ";
			String arr_str = arr.toString();
			String sql_str = arr_str.replace("[", "(").replace("]", ")").substring(1, arr_str.length() - 1);
			sql += sql_str;
			// 결과 매우느림
//			for (int i = 0; i < arr.size(); i++) {
//				String result = arr.get(i).toString().replace("[", "(").replace("]", ")");
//				sql += result;
//				if (i != arr.size() - 1) {
//					sql += ",";
//				}
//			}
			PreparedStatement psmt = conn.prepareStatement(sql);
			psmt.executeUpdate();
			long afterTime = System.currentTimeMillis();
			Double secDiffTime = (afterTime - beforeTime) / 1000.0;
			System.out.println("시간차이 array(s) : " + secDiffTime);
			// System.out.println(arr.toString());
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
//////////걸린시간 array(s) : 2.109
```

2.addbatch를 통해 insert하는 방법

```java
@Test
	public void contextLoads2() {
		ArrayList<ArrayList<String>> arr = new ArrayList<>();
		try {
			Class.forName("cubrid.jdbc.driver.CUBRIDDriver");
			Connection conn = DriverManager.getConnection("jdbc:CUBRID:175.207.13.165:30000:whitedefense:::?charSet=utf8", "dba", "");
			String sql = "INSERT INTO tbl_insert_test VALUES (?,?,?,?,?,?,?,?,?,?,DEFAULT)";
			PreparedStatement psmt = conn.prepareStatement(sql);
			long beforeTime = System.currentTimeMillis();
			for (int i = 1; i <= 4000; i++) {
				psmt.setString(1, "test" + i);
				psmt.setString(2, "test" + i);
				psmt.setString(3, "test" + i);
				psmt.setString(4, "test" + i);
				psmt.setString(5, "test" + i);
				psmt.setString(6, "test" + i);
				psmt.setString(7, "test" + i);
				psmt.setString(8, "test" + i);
				psmt.setString(9, "test" + i);
				psmt.setString(10, "test" + i);
				psmt.addBatch();
				psmt.clearParameters();
			}
			psmt.executeBatch();
			psmt.clearBatch();
			conn.commit();
			long afterTime = System.currentTimeMillis();
			Double secDiffTime = (afterTime - beforeTime) / 1000.0;
			System.out.println("시간차이 batch(s) : " + secDiffTime);
			// System.out.println(arr.toString());
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

///////////걸린시간 batch(s) : 10.83
```

