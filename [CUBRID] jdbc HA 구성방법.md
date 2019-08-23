# 큐브리드 HA구성 설정방법

```java
1, JDBC 예제
--connection URL string when a property(altHosts) specified for HA
URL=jdbc:CUBRID:192.168.0.1:33000:DB_name:DB_user::?altHosts=192.168.0.2:33000,192.168.0.3:33000

--connection URL string when properties(altHosts,rcTime, connectTimeout) specified for HA
URL=jdbc:CUBRID:192.168.0.1:33000:DB_name:DB_user::?altHosts=192.168.0.2:33000,192.168.0.3:33000&rcTime=600&connectTimeout=5

--connection URL string when properties(altHosts,rcTime, charSet) specified for HA
URL=jdbc:CUBRID:192.168.0.1:33000:DB_name:DB_user::?altHosts=192.168.0.2:33000,192.168.0.3:33000&rcTime=600&charSet=utf-8

2, C-CCI 예제
--connection URL string when a property(altHosts) is specified for HA 
URL=cci:CUBRID:192.168.0.1:33000:DB_name:DB_user::?altHosts=192.168.0.2:33000,192.168.0.3:33000

--connection URL string when properties(altHosts,rcTime) is specified for HA
URL=cci:CUBRID:192.168.0.1:33000:DB_name:DB_user::?altHosts=192.168.0.2:33000,192.168.0.3:33000&rcTime=600

--connection URL string when properties(logSlowQueries,slowQueryThresholdMills, logTraceApi, logTraceNetwork) are specified for interface debugging
URL = "cci:cubrid:192.168.0.1:33000:DB_name: DB_user::?logSlowQueries=true&slowQueryThresholdMillis=1000&logTraceApi=true&logTraceNetwork=true"
```

