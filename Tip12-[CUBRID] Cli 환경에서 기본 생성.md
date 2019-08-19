# 큐브리드 db생성 및 계정생성

```shell
cubrid createdb 디비명 ko_KR.utf8 --file-path=databases경로 
cubrid server start 디비명
ps -ef |grep cub >> 서버 체크
csql --CS-mode 디비명 --user=dba
create user 유저명;
alter user 유저명 password '패스워드';
drop user dba;
drop user public;
select * from db_user; >> 유저추가 확인
exit;
csql -i /경로/schema.sql 디비명     
csql -i /경로/index.sql 디비명       
csql -i /경로/데이터.sql 디비명     >>하나하나 차례대로 넣음 됩니다.
```

