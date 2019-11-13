# 웹서버 보안관련 설정

#### 1.암호화된 세션(ssl) 쿠키의 누락된 보안설정.

\- WAS설정에서 가능 jboss-web.xml

```xml
 <session-config>
        <session-timeout>60</session-timeout>
        <cookie-config>
            <http-only>true</http-only>
            <secure>true</secure>
        </cookie-config>
        <tracking-mode>COOKIE</tracking-mode>
</session-config>
```

\- Jboss standard.xml 설정에서 가능

```xml
<servlet-container name="default" default-session-timeout="60" disable-file-watch-service="true">
    <session-cookie http-only="true" secure="true" />
    <jsp-config development="true" check-interval="300" x-powered-by="false" />
    <websockets/>
</servlet-container>
```



#### 2.Apache header 구성 http.conf

```conf
    <IfModule mod_headers.c> 모듈명
        Header set Content-Security-Policy  "script-src * 'self' 'unsafe-inline' "
        Header set X-Content-Type-Options "nosniff"
        Header set X-XSS-Protection "1;"
        Header set Strict-Transport-Security "max-age=63072000; includeSubDomains; preload;"
        Header set Cache-Control "no-store"
        Header set Pragma "no-cache"
    </IfModule>
```

