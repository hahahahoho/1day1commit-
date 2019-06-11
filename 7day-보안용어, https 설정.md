# 보안관련 잡지식

1. EDR(Endpoint detection & response) : 네트워크에 최종적으로 연결된 it장치 내부에서 발생하는 여러가지 상황을 탐지하여 악성코드나 해킹의 위협으로부터 보호

2. DLP(Data Loss Prevention) : 데이터 유실 방지로 데이터의 흐름을 감시하여 문서유출이나 파일의 이력을 통해 보안강화 (ex : cctv)

3. DRM(Digital Rights Management) : 데이터권리관리, 암호화를 통해 데이터를 직접적으로 보안

4. Ransom : 몸값, 공작 - 랜섬웨어란 랜섬+소프트웨어의 합성어로 악성프로그램을 통해 문서를 암호화 시키고 복호화를 시킬 수 있는 키값에 대한 돈을 요구하는 것을 의미

5. EDR의 선두주자인 회사 : 카본블랙, 파이어아이 등

6. APT(Advanced persistent Treat) : 특정 대상을 목표로 다양한 해킹 기술을 이용하여 단기, 장기적으로 은밀하고 지속적으로 공격하는 것

7. DNS 서버 변경을 통해 https를 도메인네임을 통해 접근을 막겠다는 정부의 제한을 쉽게 우회가능

8. TLS(Transport Layer Security) : 예전에는 ssl로 알려졌으나 3버전부터 TLS v1로 바뀌었음. TLS자체는 독립적인 보안 프로토콜로서 http위에 TLS를 올린 것이 https

    -> openssl(웹브라우저와 서버간의 통신을 암호화하는 오픈소스 라이브러리)을 통해 개인키 공개키를 생성하고 CSR - certificate signing request를 생성하거나 또는 이를 통해 인증서(CA)를 생성 또는, 요청할 수 있음

   ​      but, 공인인증기관을 통해 인증서를 받으려면 추가적인 정보를 더 보내야함

    ->  인증서 생성 및 설치를 다 하면 apache나 nginx에서 설정을 함으로써 https 적용 가능

    -> 호환버전 => TLS v3 : openssl 1.1.1, nginx 1.13.0, apache 2.4.37 이상





#### https 설정을 위한 좋은 설명 사이트

1. <https://webactually.com/2018/11/http%EC%97%90%EC%84%9C-https%EB%A1%9C-%EC%A0%84%ED%99%98%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C/>

2. <https://namjackson.tistory.com/24>