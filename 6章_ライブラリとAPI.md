## 6. ライブラリとAPI

GASには、便利な外部ライブラリや APIがあります。これらを使うことで、より高度で複雑な処理を簡単に実装できます。この章では、ライブラリの利用方法と外部APIの呼び出し方、OAuth認証の実装方法を説明します。

### 6.1 GASライブラリの利用方法

GASライブラリは、他のユーザーが公開している再利用可能なコードです。ライブラリを使うことで、自分で一から実装することなく、便利な機能を利用できます。

ライブラリを使うには、以下の手順を実行します。

1. スクリプトエディタの「リソース」メニューから「ライブラリ」を選択。
2. 「ライブラリを追加」ボタンをクリック。
3. ライブラリのプロジェクトIDまたはスクリプトIDを入力。
4. バージョンを選択（通常は最新版を選択）。
5. 識別子を入力（任意の名前を付けられます）。
6. 「追加」ボタンをクリック。

ライブラリを追加したら、以下のようにしてライブラリの関数を呼び出せます。

```javascript
const result = LibraryName.functionName(arguments);
```

### 6.2 外部APIの呼び出し

GASから外部のWeb APIを呼び出すことで、様々なサービスとの連携が可能になります。外部APIを呼び出すには、`UrlFetchApp`クラスを使います。

```javascript
function callAPI() {
  const url = "https://api.example.com/data";
  const params = {
    method: "get",
    headers: { "Content-Type": "application/json" },
    muteHttpExceptions: true,
  };

  const response = UrlFetchApp.fetch(url, params);
  const data = JSON.parse(response.getContentText());
  Logger.log(data);
}
```

上記の例では、指定したURLにGETリクエストを送信し、レスポンスのJSONをパースしてログに出力しています。

### 6.3 OAuth認証の実装

外部サービスによっては、APIを利用するためにOAuth認証が必要な場合があります。GASでOAuth認証を実装するには、以下の手順が必要です。

1. 外部サービスのデベロッパーコンソールでアプリを登録し、クライアントIDとクライアントシークレットを取得。
2. GASでOAuthライブラリを使って、認証フローを実装。
3. 認証後のアクセストークンを使って、APIを呼び出す。

```javascript
function getOAuthService() {
  return OAuth2.createService("ExampleService")
    .setAuthorizationBaseUrl("https://example.com/auth")
    .setTokenUrl("https://example.com/token")
    .setClientId("YOUR_CLIENT_ID")
    .setClientSecret("YOUR_CLIENT_SECRET")
    .setCallbackFunction("authCallback")
    .setPropertyStore(PropertiesService.getUserProperties())
    .setScope("https://example.com/auth/drive.readonly");
}

function authCallback(request) {
  const service = getOAuthService();
  const authorized = service.handleCallback(request);
  if (authorized) {
    return HtmlService.createHtmlOutput("Success!");
  } else {
    return HtmlService.createHtmlOutput("Denied.");
  }
}

function makeRequest() {
  const service = getOAuthService();
  if (service.hasAccess()) {
    const url = "https://api.example.com/data";
    const response = UrlFetchApp.fetch(url, {
      headers: { Authorization: "Bearer " + service.getAccessToken() },
    });
    Logger.log(response.getContentText());
  } else {
    const authorizationUrl = service.getAuthorizationUrl();
    Logger.log("Open the following URL and re-run the script: %s", authorizationUrl);
  }
}
```

上記の例では、`getOAuthService`関数でOAuthサービスを設定し、`authCallback`関数で認証のコールバックを処理しています。`makeRequest`関数では、認証済みのアクセストークンを使ってAPIを呼び出しています。

### 6.4 練習問題
1. 天気予報APIを呼び出して、指定した地域の天気予報を取得し、スプレッドシートに書き出す関数を作成してください。
2. Twitterの検索APIを使って、指定したキーワードを含むツイートを検索し、結果をスプレッドシートに書き出す関数を作成してください。その際、OAuth認証を実装してください。
3. Google Analyticsの APIを呼び出して、指定した期間のページビュー数を取得し、スプレッドシートに書き出す関数を作成してください。

(続く)