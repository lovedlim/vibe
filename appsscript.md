안녕하세요! 앱스 스크립트(Apps Script)를 사용하여 문의 폼을 처리하는 방법을 알려드릴게요. 웹페이지의 폼 데이터를 구글 시트에 기록하고, 작성자와 관리자에게 이메일 알림을 보내는 세 가지 기능을 모두 구현할 수 있습니다.

1단계: 구글 스프레드시트 설정
먼저, 문의 내용을 기록할 구글 스프레드시트를 만드세요.

구글 드라이브에서 새 구글 스프레드시트를 생성합니다.

첫 번째 행에 다음과 같이 제목을 입력합니다. 타임스탬프, 성함 및 소속, 연락받으실 이메일, 문의 내용. 이 제목들은 웹 폼의 name 속성과 일치해야 합니다.

2단계: Apps Script 작성
이제 스프레드시트와 연동될 스크립트를 작성할 차례입니다.

스프레드시트 상단 메뉴에서 확장 프로그램 > Apps Script를 클릭합니다.

아래 코드를 복사해서 코드 편집기에 붙여넣으세요. YOUR_EMAIL_ADDRESS와 YOUR_SPREADSHEET_ID는 본인의 정보로 바꿔야 합니다. 스프레드시트 ID는 URL에서 d/와 /edit 사이에 있는 문자열입니다.

JavaScript

// 웹 폼에서 POST 요청을 받으면 실행되는 함수
function doPost(e) {
  // 1. 구글 시트에 데이터 기록
  let sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  let data = e.parameter;
  let row = [new Date(), data.name, data.email, data.inquiry];
  sheet.appendRow(row);

  // 2. 작성자에게 문의 완료 메일 발송
  let recipientEmail = data.email;
  let subjectForAuthor = "[문의 완료] 문의가 정상적으로 접수되었습니다.";
  let bodyForAuthor = `안녕하세요 ${data.name}님, \n\n문의가 정상적으로 접수되었습니다. 최대한 빠르게 답변드리겠습니다. \n\n[문의 내용]\n${data.inquiry}`;
  MailApp.sendEmail(recipientEmail, subjectForAuthor, bodyForAuthor);

  // 3. 관리자에게 알림 메일 발송
  let adminEmail = "YOUR_EMAIL_ADDRESS"; // <-- 여기에 관리자 이메일 주소를 입력하세요.
  let subjectForAdmin = `[${data.name}]의 문의가 왔습니다.`;
  let bodyForAdmin = `새로운 문의가 접수되었습니다.\n\n- 성함 및 소속: ${data.name}\n- 이메일: ${data.email}\n- 문의 내용:\n${data.inquiry}`;
  MailApp.sendEmail(adminEmail, subjectForAdmin, bodyForAdmin);

  // 성공 응답 반환
  return ContentService.createTextOutput("Success").setMimeType(ContentService.MimeType.TEXT);
}
3단계: 스크립트 배포
작성된 스크립트를 웹 애플리케이션으로 배포해야 웹 폼에서 접근할 수 있습니다.

Apps Script 편집기에서 배포 > 새 배포를 클릭합니다.

'유형 선택'에서 웹 앱을 선택합니다.

'설명'은 자유롭게 입력합니다.

'다음 계정으로 실행'은 나를 선택하고, '액세스 권한이 있는 사용자'는 누구나로 설정합니다.

배포를 클릭합니다. 처음 배포할 때 권한 승인 창이 뜨면, 안내에 따라 권한을 허용해 주세요.

배포가 완료되면 웹 앱 URL이 생성됩니다. 이 URL을 복사해 두세요.

4단계: 홈페이지 폼 코드 작성
마지막으로, HTML 폼의 action 속성에 위에서 복사한 웹 앱 URL을 붙여넣습니다.

HTML

<form id="contactForm" method="POST" action="YOUR_WEB_APP_URL">
  <label for="name">성함 및 소속</label>
  <input type="text" id="name" name="name" required>

  <label for="email">연락받으실 이메일</label>
  <input type="email" id="email" name="email" required>

  <label for="inquiry">문의 내용</label>
  <textarea id="inquiry" name="inquiry" required></textarea>

  <button type="submit">문의하기</button>
</form>
주의사항

input 태그와 textarea 태그의 name 속성(name="name", name="email", name="inquiry")이 Apps Script 코드와 정확히 일치해야 합니다.

이 방식은 서버 측 코드를 사용하지 않고 클라이언트 측에서 직접 구글 Apps Script로 데이터를 전송하는 방식입니다.

궁금한 점이 있다면 언제든지 다시 물어봐 주세요.
