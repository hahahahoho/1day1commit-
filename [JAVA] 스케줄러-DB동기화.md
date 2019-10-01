# Timer를 통한 스케줄 어플리케이션

타이머클래스를 통한 db연동 어플리케이션 구현, quartz 라이브러리를 통해서도 구현가능하지만 java에서 기본으로 제공하는 Timer클래스와 TimerTask클래스를 통해서도 쉽게 구현이 가능하다. 예제 코드를 통해 자신의 상황에 맞게 수정하여 사용하면 될 것 같다.

#### 1. 메인

```java
	public static void main(String[] args) {
		TimerExample te1=new TimerExample("Task2");
		Timer t=new Timer();
		Calendar date = Calendar.getInstance();
	 	date.set(Calendar.DAY_OF_WEEK, Calendar.SATURDAY);
	 	date.set(Calendar.AM_PM, Calendar.PM);
		date.set(Calendar.HOUR, 11);
		date.set(Calendar.MINUTE, 29);
		date.set(Calendar.SECOND, 0);
		date.set(Calendar.MILLISECOND, 0);
		//하루에 한번 매주 토요일 11시 29분 실행하는 설정
		//t.scheduleAtFixedRate(te1, date.getTime(), 1000 * 60 * 60 * 24);
		//1분에 한번씩 실행
		t.schedule
    }
```

#### 2. 실제동작 CLASS

```java
public class TimerExample extends TimerTask{
	private String name ;
	private Connection conn = null;
	private Connection conn2 = null;
	private PreparedStatement pstmt = null;
	private PreparedStatement pstmt2 = null;
	private ResultSet rs = null;
	
	public TimerExample(String n){
		this.name=n;
		try {
			Class.forName ("cubrid.jdbc.driver.CUBRIDDriver");
			conn = DriverManager.getConnection("jdbc:CUBRID:ip:port:dbName:::?charSet=utf8", "user", "pw");
			conn2 = DriverManager.getConnection("jdbc:CUBRID:ip:port:dbName:::?charSet=utf8", "user", "pw");
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	@Override
	public void run() {
		 try {
			Date today = new Date();
			String sel_sql = "SELECT field1, field2, field3 FROM table WHERE ROWNUM BETWEEN 1 AND 100";
			String ins_sql = "INSERT INTO viw_test_1(field1, field2, field3) VALUES(?,?,?)";
			pstmt = conn.prepareStatement(sel_sql);
			pstmt2 = conn2.prepareStatement(ins_sql);
			rs = pstmt.executeQuery();
			while(rs.next()) {
				pstmt2.setString(1, rs.getString(1));
				pstmt2.setString(2, rs.getString(2));
				pstmt2.setString(3, rs.getString(3));
				pstmt2.addBatch();
				pstmt2.clearParameters();
			}
			int [] result = pstmt2.executeBatch();
			pstmt2.clearBatch();
			conn2.commit();
			System.out.println(result.length);
			System.out.println("insert 완료");
			SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMdd HH:mm:ss");
			System.out.println(sdf.format(today));
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

