# 13장
## 13.3.2 계획 세우기
~~~
채팅 서비스를 만들어줘.
프론트엔드는 커서로 만들고, 백엔드는 n8n 웹훅을 통해 사용하려고 해.
•	n8n 웹훅과 POST 방식으로 통신
•	message와 sessionId를 함께 전송
•	Production URL: (직접 입력)
~~~


### 13.3 n8n으로 챗봇 워크플로우 만들기
~~~
# AI Agent
{{ $json.body.message }}
~~~

### 13.6 메모리 기능 추가

~~~
# Simple Memony
{{ $json.body.sessionId }}
~~~
