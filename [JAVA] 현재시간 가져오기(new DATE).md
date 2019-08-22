# 현재시간을 원하는 형태로 가져오는 방법

```java
SimpleDateFormat format1 = new SimpleDateFormat ( "yyyy-MM-dd HH:mm:ss");
SimpleDateFormat format2 = new SimpleDateFormat ( "yyyy년 MM월dd일 HH시mm분ss초");
		
Date time = new Date();
		
String time1 = format1.format(time);
String time2 = format2.format(time);
		
System.out.println(time1);
System.out.println(time2);
```

