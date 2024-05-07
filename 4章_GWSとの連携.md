## 4. Google Workspaceサービスとの連携

GASの大きな特徴は、Google Workspaceサービスとの連携の容易さです。この章では、代表的なサービスであるGoogle Spreadsheet、Google Docs、Google Formsとの連携方法を説明します。

### 4.1 Google SpreadsheetとGAS
Google Spreadsheetは、GASと最も相性の良いサービスの1つです。スプレッドシートのデータを読み書きしたり、書式を設定したり、チャートを作成したりできます。

スプレッドシートのIDを取得する方法について。

スプレッドシートのIDを取得するには、以下の手順を実行します。

1. Googleスプレッドシートを開く
2. ブラウザのアドレスバーを確認する
3. アドレスバーのURLは以下のような形式になっています
   ```
   https://docs.google.com/spreadsheets/d/スプレッドシートのID/edit#gid=シートのID
   ```
4. `/d/` と `/edit` の間の文字列がスプレッドシートのIDです

例えば、以下のようなURLの場合：
```
https://docs.google.com/spreadsheets/d/1ABCdefGHIjklMNOpqrSTUvwxYZabcdefGHIjklMNO/edit#gid=0
```

スプレッドシートのIDは `1ABCdefGHIjklMNOpqrSTUvwxYZabcdefGHIjklMNO` になります。

このIDをコードの `const spreadsheetId = "スプレッドシートのIDを入力";` の部分に入力してください。

例：
```javascript
const spreadsheetId = "1ABCdefGHIjklMNOpqrSTUvwxYZabcdefGHIjklMNO";
```

これでスプレッドシートのIDを指定して、GASからスプレッドシートを操作できるようになります。

また、スプレッドシートのIDは、スプレッドシートを他のユーザーと共有する際にも使用されます。IDを知っている人は誰でもスプレッドシートにアクセスできるため、機密情報を含むスプレッドシートのIDは安易に共有しないように注意が必要です。

以上、スプレッドシートのIDの取得方法についての詳細説明でした。これを資料に追加することで、読者がスムーズにコードを理解し、実行できるようになるはずです。

#### シートの読み書き
```javascript
function readWriteSheet() {
  const spreadsheetId = "スプレッドシートのIDを入力";
  const spreadsheet = SpreadsheetApp.openById(spreadsheetId);
  const sheet = spreadsheet.getSheetByName('シート1');
  const value = sheet.getRange('A1').getValue();
  console.log(value);
  sheet.getRange("B1").setValue("Hello, GAS!");
}
```

#### 条件付き書式の設定
```javascript
function setConditionalFormatting(spreadsheetId) {
  const spreadsheetId = "スプレッドシートのIDを入力";
  const spreadsheet = SpreadsheetApp.openById(spreadsheetId);
  const sheet = spreadsheet.getSheetByName('シート1');
  const range = sheet.getRange("A1:B10");
  const rule = SpreadsheetApp.newConditionalFormatRule()
    .whenNumberGreaterThan(100)
    .setBackground("#FF0000")
    .setFontColor("#FFFFFF")
    .build();
  range.setConditionalFormatRules([rule]);
}
```

#### チャートの作成
```javascript
function createChart(spreadsheetId) {
  const spreadsheetId = "スプレッドシートのIDを入力";
  const spreadsheet = SpreadsheetApp.openById(spreadsheetId);
  const sheet = spreadsheet.getSheetByName('シート1');
  const range = sheet.getRange("A1:B10");
  const chart = sheet.newChart()
    .setChartType(Charts.ChartType.BAR)
    .addRange(range)
    .setPosition(1, 1, 0, 0)
    .build();
  sheet.insertChart(chart);
}
```

### 4.2 Google DocsとGAS
Google DocsもGASと連携して、ドキュメントの作成や編集、差し込み印刷などを行うことができます。

#### ドキュメントの作成と編集
```javascript
function createAndEditDoc() {
  const doc = DocumentApp.create("新しいドキュメント");
  const body = doc.getBody();
  body.appendParagraph("これは新しいドキュメントです。");
  body.appendHorizontalRule();
  body.appendParagraph("GASから作成しました。");
}
```

#### 差し込み印刷
```javascript
function mailMerge() {
  const template = DocumentApp.openById("テンプレートのドキュメントID");
  const data = SpreadsheetApp.getActiveSheet().getDataRange().getValues();
  data.forEach((row, index) => {
    if (index === 0) return;
    const copy = template.copy(`${row[0]}宛の文書`);
    const body = copy.getBody();
    body.replaceText("{{名前}}", row[0]);
    body.replaceText("{{住所}}", row[1]);
  });
}
```

### 4.3 Google FormsとGAS
Google Formsは、フォームの作成と回答データの収集に使われます。GASを使って、フォームの作成や回答データの処理を自動化できます。

#### フォームの作成と回答の取得
```javascript
function createFormAndGetResponses() {
  const form = FormApp.create("新しいフォーム");
  const item = form.addTextItem();
  item.setTitle("名前");
  item.setRequired(true);
  
  const responses = form.getResponses();
  responses.forEach(response => {
    const itemResponse = response.getItemResponses();
    itemResponse.forEach(r => {
      console.log(`${r.getItem().getTitle()}: ${r.getResponse()}`);
    });
  });
}
```

#### 回答データの処理
```javascript
function processFormResponses() {
  const form = FormApp.openById("フォームのID");
  const responses = form.getResponses();
  const sheet = SpreadsheetApp.create("フォームの回答").getActiveSheet();
  const header = ["タイムスタンプ", "名前", "メールアドレス"];
  sheet.appendRow(header);
  
  responses.forEach(response => {
    const timestamp = response.getTimestamp();
    const itemResponses = response.getItemResponses();
    const name = itemResponses[0].getResponse();
    const email = itemResponses[1].getResponse();
    const row = [timestamp, name, email];
    sheet.appendRow(row);
  });
}
```

### 4.4 練習問題
1. スプレッドシートのA1:B10の範囲に、ランダムな数値（1から100の間）を入力する関数を作成してください。
2. Google Docsのテンプレートを使って、スプレッドシートのデータを差し込んだ文書を生成する関数を作成してください。
3. Google Formsの回答データを、回答者のメールアドレスごとにフィルタリングして、スプレッドシートに出力する関数を作成してください。

(続く)