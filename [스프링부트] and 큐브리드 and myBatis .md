스프링부트와 큐브리드 그리고 마이바티스를 이용하여 프로젝트를 구성하게 되었습니다. 음.. 그런데 mysql과 스프링부트 그리고 마이바티스는 연동이 잘 되는 것을 테스트 했는데.... why!!! cubrid는 연동이 안되더군요. 혹시 다른 방법이 있을 까 구글님에게 물어보며... 2일 동안 삽질을 하였지만... 결국 해결하지 못하였습니다. ㅜㅜ

그리고 안되는 원인은....!

 최근 버전 스프링부트에서 jdbcTemplate 객체를 통해 큐브리드를 연동하면 에러가 발생하는데 아직 큐브리드에서 이를 지원하지 않는다고 하네요.. 

.... 제 잘못은 아니였습니다. 저 글을 사실 미리 봤었는데... 포기하고 싶지 않더군요. 이번 경험을 통해 빠른 포기도 필요할 때가 있겠구나 뼈저리게 느낀 것 같습니다.

저는 이번에 10.1.2버전을 사용해보았고 추후 다음버전이 나와야 스프링부트 적용가능성을 검토할 수 있을 것 같습니다.

# 1.Spring boot + mabatis + cubrid : 결론 안된다. 아직!

# 2. Spring 구조 이해

###### 	  - 구성 : 스프링부트 + 큐브리드(DriverManager 를 통한) 순수 연결

######   - 로직 : spring boot @restController를 통한 restAPI구현 및 @controller를 통한 페이지 호출

-> jsp를 사용하지 않고 html사용. 모든 데이터 처리는 서버(java)에서 이루어지며, javascript ajax를 통하여 데이터처리 요청 및 호출.

