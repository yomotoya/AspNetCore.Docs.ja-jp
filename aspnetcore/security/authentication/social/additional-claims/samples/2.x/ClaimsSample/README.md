# <a name="additional-claims-sample-app"></a>その他の要求のサンプル アプリ

サンプル アプリについて説明する方法。

* ユーザーの指定した名前と姓を Google から取得し、Google から提供される値を持つ名前クレームを格納します。
* ユーザーの Google アクセス トークンは保存`AuthenticationProperties`します。

サンプル アプリを使用します。

1. アプリを登録し、有効なクライアント ID と Google 認証用のクライアント シークレットを取得します。 詳細については、次を参照してください。 [Google 外部ログイン セットアップ](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins)します。
1. クライアント ID とクライアント シークレットをアプリに提供、 [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions)の`Startup.ConfigureServices`します。
1. アプリを実行し、My 要求ページを要求します。 ユーザーが署名されていないときに、アプリを Google にリダイレクトします。 Google でサインインします。 Google は、アプリにユーザーをリダイレクト (`/MyClaims`)。 ユーザーが認証されると、および My 要求ページが読み込まれます。 指定された名前と姓のクレームが存在**ユーザーの信頼性情報**Google から提供される値を使用します。 下に表示されるアクセス トークン**認証プロパティ**します。
