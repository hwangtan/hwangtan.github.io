---
date: '2019-05-15 08:48:05'
layout: post
title: Using Google Sheets as a Database!
subtitle: ''
description: >-
 스프레드 시트를 외부에서 접근 하는 데에는 여러 가지 방법이 있는데, 오늘은 이러한 여러 가지 접근 방법에 대해서 설명해 보겠습니다.
image: /assets/img/uploads/notebook-1850613_1920.jpg
category: blog
tags:
  - Spreadsheet
  - database
author: jacob
paginate: false
---

# Using Google Sheets as a Database!

### WHY

이 방법에 관심을 가지게 된 이유는 저의 개인 프로젝트 때문 입니다.


저장 하는 데이터의 양이 많지 않고 개인 용도로 사용하는데 서비스를 이용 하는 비용을 최소화 하고 싶었습니다.

몽고 디비 기반에 mLab 이라는 서비스가 있었지만, 좀더 가볍게 데이터를 저장하거나 접근 하고 싶었고 대안으로 구글 스프레드 시트를 찾게 되었습니다.

스프레드 시트를 외부에서 접근 하는 데에는 여러 가지 방법이 있는데, 오늘은 이러한 여러 가지 접근 방법에 대해서 설명해 보겠습니다.

### ENVIRONMENT

- express (for rendering dynamic pages)
- request (for making HTTP requests)
- fs (for the Google API)
- readline (for the Google API)
- googleapis (for the Google API)
- google-spreadsheet(for the Google API)

express 제너레이터를 통해서 express 프로 젝트를 생성 하고 시작 합니다.

### V3

V3 를 사용하는 이유는 쉽기 때문입니다.

V3 를 사용하지 말아야 할 이유도 있습니다.

- v3은 결국 사용 중단 될 예정입니다.
- v3 방법을 사용하려면 스프레드 시트를 게시 해야 합니다.
- v3 방법을 사용하면 읽기만 가능하고 쓰기는 허용하지 않습니다.

게시할 스프레드 시트를 열고 파일 → 웹에 게시를 선택합니다.

게시를 완료하면 패스 중간에 해시로 이루어진 값이 있는데 이것이 스프레드 시트의 고유한 아이디 값 입니다.

그리고 다음과 같은 규칙으로 api 를 요청 하면 쉽게 데이터를 조회 할 수 있습니다.

[`https://spreadsheets.google.com/feeds/cells/SPREADSHEET_ID/TAB_NUMBER/public/values?alt=json`](https://spreadsheets.google.com/feeds/cells/SPREADSHEET_ID/TAB_NUMBER/public/values?alt=json)

### V4

**[이 문서](https://developers.google.com/sheets/api/quickstart/nodejs#step_3_set_up_the_sample)** 를 통해서 쉽게 사용 할 수 있습니다.

이 예제는 oauth2 기반의 인증 방식을 통한 스프레드 시트의 접근 방식을 설명 합니다.
```js
    var dataPull = function (auth) {
        const sheets = google.sheets({
          version: 'v4',
          auth
        });
        sheets.spreadsheets.values.get({
          spreadsheetId: '1z3HPr81WpzeTkgjtM6O4mPTBUACjAyD3CDC3MSPLgg4',
          range: 'Track!A1:A10',
        }, (err, response) => {
          if (err) return console.log('The API returned an error: ' + err);
    
          const rows = response.data.values;
          res.render('test', {
            response: rows
          })
        });
      };
```
### google-spreadsheet

이 방법은 oAuth2 를 이용한 방식보다 깔끔하며 직관적 입니다.

**[구글 개발자 콘솔](https://console.developers.google.com/apis/api/sheets.googleapis.com/overview?project=quickstart-1551938962246&folder&organizationId)**로 이동하여 library 에서 spread sheet api 를 검색하고 이를 활성화 시킵니다.

해당 api 관리에 들어가면, Credentials 탭을 클릭 하고 Credentials in APIs & Services 을 클릭 합니다.

생성 버튼을 통해서 Create service account key 를 선택 하고 Service account name 을 입력 합니다.

[Node.js Quickstart | Sheets API | Google Developers](https://developers.google.com/sheets/api/quickstart/nodejs#step_3_set_up_the_sample)

Role 은 Editor 또는 Owner 를 선택 한 뒤에 Create 를 누르면 JSON 파일이 다운로드 되는데 해당 파일이

이 라이브러리에서 사용될 파일이 됩니다.

이때 해당 파일을 열어 보면 속성에 client_email 이 있는데 해당 값을 복사하여 연결할 스프레드 시트에 공유 사용자로 추가를 해 줘야 합니다.

스프레드 시트를 열고 파일 → 공유 인풋 창에 복사한 이메일을 붙여 넣습니다.

여기까지 완료 되었다면, 해당 라이브러리를 이용해 쉽게 스프레드 시트에 접근 할 수 있습니다.
```js
    const doc = new GoogleSpreadsheet('1z3HPr81WpzeTkgjtM6O4mPTBUACjAyD3CDC3MSPLgg4');
    
      doc.useServiceAccountAuth(creds, function (err) {
        doc.getRows(1, {
          alt: 'json'
        }, callback);
        console.log(err);
    
        function callback(err, rows) {
          console.log(err, rows)
          res.render('test', {
            response: JSON.stringify(rows)
          })
        }
      });
```
### CONCLUSION

3가지 방식에 대해서 알아 보았고, 마지막 방법을 추천 합니다.

로컬 에서 테스트를 원 하는 분을 위해서 코드는 **[이 저장소](https://github.com/hwangtan/google-spread-sheet)** 에 올렸습니다.
