---
title: ASP.NET Core のセッションとアプリケーションの状態
author: rick-anderson
description: 要求間でアプリケーションとユーザー (セッション) の状態を維持する手法。
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: 1b41690fce707314f6cd0e891e4180481a2f632b
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/12/2018
---
# <a name="session-and-application-state-in-aspnet-core"></a>ASP.NET Core のセッションとアプリケーションの状態

[Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)、[Diana LaRose](https://github.com/DianaLaRose) 作

HTTP はステートレス プロトコルです。 Web サーバーは各 HTTP 要求を非依存の要求として扱い、前の要求からのユーザー値を維持しません。 この記事では、要求間でアプリケーションとセッションの状態を維持するさまざまな方法について取り上げます。

## <a name="session-state"></a>セッション状態

セッション状態は ASP.NET Core の機能で、これを使用することでユーザーが Web アプリを参照中にユーザー データを保存して格納することができます。 セッション状態は、サーバー上のディクショナリまたはハッシュ テーブルから構成され、ブラウザーからの要求間でデータを維持します。 セッション データはキャッシュによりバックアップされます。

ASP.NET Core は、セッション ID を含む Cookie をクライアントに与えることでセッションの状態を維持します。セッション ID は要求ごとにサーバーに送信されます。 サーバーはセッション ID を使用し、セッション データを取得します。 セッション Cookie はブラウザーに固有であるため、ブラウザー間でセッションを共有することはできません。 セッション Cookie は、ブラウザー セッションが終了したときに初めて削除されます。 Cookie を受け取り、セッションが期限切れになった場合、同じセッション Cookie を使用する新しいセッションが作成されます。

サーバーは、最後の要求から限られた時間だけセッションを維持します。 セッション タイムアウトを設定するか、20 分という既定値を使用してください。 セッション状態は、特定のセッションに固有であるが、永久的に維持する必要がないユーザー データの格納に最適です。 `Session.Clear` を呼び出したときか、データ ストアでセッションが期限切れになったとき、バックアップ ストアからデータが削除されます。 サーバーでは、ブラウザーが閉じられことやセッション Cookie が削除されたことが認識されません。

> [!WARNING]
> セッションでは機密データを保存しないでください。 クライアントでブラウザーが閉じられず、セッション Cookie が消去されないことがあります (ブラウザーによっては、ウィンドウ間でセッション Cookie が有効になります)。 また、セッションが 1 人のユーザーに制限されないことがあります。次のユーザーが同じセッションを続けることがあります。

メモリ内セッション プロバイダーは、ローカル サーバー上でセッション データを保存します。 サーバー ファームで Web アプリを実行する予定であれば、固定セッションを使用し、各セッションを特定のサーバーに結び付ける必要があります。 Windows Azure Web Sites プラットフォームは、初期設定で固定セッションを使用します (アプリケーション要求ルーティング処理または ARR)。 ただし、固定セッションは拡張性に影響を与え、Web アプリの更新を複雑にすることがあります。 良い選択肢は、Redis または SQL Server の分散キャッシュを使用することです。固定セッションを必要としません。 詳細については、[分散キャッシュの使用](xref:performance/caching/distributed)に関するページを参照してください。 サービス プロバイダーの設定方法については、この記事の「[セッションを構成する](#configuring-session)」を参照してください。

## <a name="tempdata"></a>TempData

ASP.NET Core MVC は[コントローラー](/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0)上で [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) プロパティを公開します。 このプロパティは、読み取られるまでデータを格納します。 `Keep` メソッドと `Peek` メソッドは、削除せずにデータを確認するために使用できます。 `TempData` は特に、複数の要求にデータが必要な場合のリダイレクトに役立ちます。 `TempData` は、Cookie やセッション状態を利用することなどで、TempData プロバイダーによって実装されます。

### <a name="tempdata-providers"></a>TempData プロバイダー

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

ASP.NET Core 2.0 以降、Cookie に TempData を保存するために Cookie ベースの TempData プロバイダーが既定で使用されます。

Cookie データは [Base64UrlTextEncoder](/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0) でエンコードされます。 Cookie はチャンク エンコードされるため、ASP.NET Core 1.x の 1 Cookie のサイズ上限は適用されません。 暗号化されているデータを圧縮すると、[CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) 攻撃や [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) 攻撃など、セキュリティ上の問題を起す可能性があるため、Cookie データは圧縮されません。 Cookie ベース TempData プロバイダーの詳細については、「[CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)」を参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

ASP.NET Core 1.0 と 1.1 では、セッション状態 TempData プロバイダーが既定です。

---

### <a name="choosing-a-tempdata-provider"></a>TempData プロバイダーを選択する

TempData プロバイダーを選択するときの考慮事項:

1. そのアプリケーションでセッション状態が他の目的のために既に使用されていないか。 使用されている場合、(データのサイズを除き) セッション状態 TempData プロバイダーがそのアプリケーションにコストを追加することはありません。
2. そのアプリケーションでは、比較的少量のデータに対して (最大 500 バイト) TempData がわずかばかり使用されているだけではないか。 該当する場合、Cookie TempData プロバイダーは TempData を送信する要求ごとに少額のコストを追加します。 該当しない場合、セッション状態 TempData プロバイダーは便利かもしれません。TempData が尽きるまで、要求のたびに大量のデータをラウンドトリップすることが回避されます。
3. そのアプリケーションは Web ファーム (複数のサーバー) で実行するのか。 その場合、Cookie TempData プロバイダーを使用するために追加の構成は必要ありません。

> [!NOTE]
> ほとんどの Web クライアント (Web ブラウザーなど) は、各 Cookie の最大サイズ、Cookie の合計数、または両方に上限を課します。 そのため、Cookie TempData プロバイダーを使用するとき、アプリでそれらの上限が超えないことを確認してください。 データの合計サイズを考慮し、暗号化やチャンク化のオーバーヘッドを計算します。

### <a name="configure-the-tempdata-provider"></a>TempData プロバイダーを構成する

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Cookie ベース TempData プロバイダーは既定で有効になります。 次の `Startup` クラス コードでは、セッション ベース TempData プロバイダーが構成されます。

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

次の `Startup` クラス コードでは、セッション ベース TempData プロバイダーが構成されます。

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

ミドルウェア コンポーネントの場合、順序が重要です。 先の例では、`UseMvcWithDefaultRoute` の後に `UseSession` が呼び出されたとき、型 `InvalidOperationException` の例外が発生します。 詳細については、[ミドルウェアの順序付け](xref:fundamentals/middleware/index#ordering)に関するページを参照してください。

> [!IMPORTANT]
> .NET Framework が対象で、セッション ベースのプロバイダーを使用するとき、[Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) NuGet パッケージをプロジェクトに追加します。

## <a name="query-strings"></a>クエリ文字列

限られた量のデータを要求間で渡すことができます。新しい要求のクエリ文字列にそれを追加します。 これは、リンクと埋め込まれた状態がメールまたはソーシャル ネットワークを通して共有されるよう、永久的に状態をキャプチャするのに役立ちます。 ただし、この理由から、機密データにはクエリ文字列で絶対に使用しないでください。 クエリ文字列にデータを含めると、共有が簡単になりますが、[クロスサイト リクエスト フォージェリ (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) 攻撃の機会を与えてしまいます。認証中、ユーザーをだまして悪意のあるサイトに誘導します。 攻撃者はアプリからユーザー データを盗んだり、ユーザーになりすまして悪意のある行為を行ったりできます。 保存されるアプリケーションまたはセッション状態は CSRF 攻撃を防ぐ必要があります。 CSRF 攻撃の詳細については、「[Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks](xref:security/anti-request-forgery)」(クロスサイト リクエスト フォージェリ (XSRF/CSRF) 攻撃の防止) を参照してください。

## <a name="post-data-and-hidden-fields"></a>データや非表示フィールドの転記

データは非表示フォーム フィールドに保存したり、次の要求で転記したりできます。 複数ページ フォームでは、これは一般的です。 ただし、クライアントがデータを改ざんする可能性があるため、サーバーは常に再検証する必要があります。

## <a name="cookies"></a>クッキー

Cookie は、web アプリケーションでユーザー固有のデータを格納する方法です。 Cookie は要求ごとに送信されるために、そのサイズは最小に抑える必要があります。 理想的には、識別子だけを Cookie に格納し、実際のデータはサーバーに保存します。 ほとんどのブラウザーで Cookie は 4096 バイトに制限されています。 また、ドメインごとに限定された数の Cookie のみを利用できます。

Cookie は改ざんされる可能性があるため、サーバーで検証する必要があります。 クライアント上の Cookie の永続性はユーザーの操作や有効期限に左右されるため、一般的に、クライアントのデータ永続性としては最も長持ちする形態となります。

Cookie は、多くの場合、パーソナル化に利用されます。既知のユーザーのためにコンテンツをカスタマイズします。 ほとんどの場合、ユーザーは識別されるだけで本人確認されないため、一般的に、Cookie にユーザー名、アカウント名、一意のユーザー ID (GUID など) を保存することで Cookie を安全にできます。 その後、Cookie を使用し、サイトのユーザー パーソナル化インフラストラクチャにアクセスできます。

## <a name="httpcontextitems"></a>HttpContext.Items

`Items` コレクションは、ある特定の要求の処理時にのみ必要なデータの格納に最適な場所です。 コレクションのコンテンツは各要求の後に破棄されます。 `Items` コレクションは、コンポーネントまたはミドルウェアが要求中に異なる時点で作動するがパラメーターを渡す直接的な方法がないとき、通信方法として最適です。 詳細については、この記事の「[HttpContext.Items を使用する](#working-with-httpcontextitems)」を参照してください。

## <a name="cache"></a>キャッシュ

キャッシュは、データを保存し、取得する効率的な方法です。 時間やその他の考慮事項に基づき、キャッシュされる項目の有効期限を制御できます。 [キャッシュの方法](../performance/caching/index.md)に関するページを参照してください。

## <a name="working-with-session-state"></a>セッション状態の使用する

### <a name="configuring-session"></a>セッションを構成する

`Microsoft.AspNetCore.Session` パッケージは、セッション状態を管理するためのミドルウェアを提供します。 セッション ミドルウェアを有効にするには、`Startup` に次が含まれている必要があります。

- いずれかの [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) メモリ キャッシュ。 `IDistributedCache` 実装はセッションのバックアップ ストアとして利用されます。
- [AddSession](/dotnet/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 呼び出し。これには NuGet パッケージ "Microsoft.AspNetCore.Session" が必要です。
- [UseSession](/dotnet/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_) 呼び出し。

次のコードでは、メモリ内セッション プロバイダーの設定方法を確認できます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

インストールと構成の後、`HttpContext` からセッションを参照できます。

`UseSession` が呼び出される前に `Session` にアクセスする場合、例外 `InvalidOperationException: Session has not been configured for this application or request` がスローされます。

`Response` ストリームへの書き込みを既に開始した後に新しい `Session` を作成しようとする場合、例外 `InvalidOperationException: The session cannot be established after the response has started` がスローされます。 例外は Web サーバー ログにあります。ブラウザーには表示されません。

### <a name="loading-session-asynchronously"></a>セッションを非同期で読み込む

ASP.NET Core の既定のセッション プロバイダーは、`TryGetValue`、`Set`、または `Remove` メソッドの前に [ISession.LoadAsync](/dotnet/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync) メソッドが明示的に呼び出された場合にのみ、基礎となる [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache) ストアから非同期的にセッション レコードを読み込みます。 `LoadAsync` が最初に呼び出されない場合、基礎となるセッション レコードが同期的に読み込まれます。これはアプリの拡張機能に影響を与える可能性があります。

アプリケーションにこのパターンを強制させるには、`TryGetValue`、`Set`、または `Remove` の前に `LoadAsync` メソッドが呼び出されない場合に例外をスローするバージョンで [DistributedSessionStore](/dotnet/api/microsoft.aspnetcore.session.distributedsessionstore) と [DistributedSession](/dotnet/api/microsoft.aspnetcore.session.distributedsession) 実装をラップします。 ラップしたバージョンをサービス コンテナーに登録します。

### <a name="implementation-details"></a>実装詳細

セッションは Cookie を利用し、1 つのブラウザーからの要求を追跡し、識別します。 既定では、この Cookie には ".AspNet.Session" という名前が付き、"/" というパスを使用します。 Cookie の既定値でドメインが指定されない場合、ページのクライアント側スクリプトで利用できません (`CookieHttpOnly` の初期設定が `true` となるため)。

セッションの既定値をオーバーライドするには、`SessionOptions` を使用します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

サーバーは `IdleTimeout` プロパティを使用し、コンテンツが破棄されるまでのセッションのアイドル時間を決定します。 このプロパティは Cookie の有効期限に依存しません。 (読み取りまたは書き込みで) 要求がセッション ミドルウェアを通過するたびにタイムアウトがリセットされます。

`Session` は*ロックなし*のため、2 つの要求がセッションのコンテンツを変更しようとすると、最後の要求が最初の要求をオーバーライドします。 `Session` は*一貫性のあるセッション*として実装されます。つまり、コンテンツは全部まとめて保管されます。 2 つの要求がセッションのさまざまなパーツ (さまざまなキー) を変更する場合、互いに影響を及ぼすことがあります。

### <a name="set-and-get-session-values"></a>セッション値の設定および取得

セッションは、`Context.Session` を使用して Razor ページまたはビューからアクセスします。

[!code-cshtml[](app-state/sample/src/WebAppSessionDotNetCore2.0App/Views/Home/About.cshtml)]

セッションは、`PageModel` クラスまたは `HttpContext.Session` を使用したコントローラーからアクセスします。 このプロパティは [ISession](/dotnet/api/microsoft.aspnetcore.http.isession) 実装です。

次の例では、int と文字列が設定され、取得されます。

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

次の拡張メソッドを追加すると、シリアル化可能なオブジェクトを Session に設定し、取得できます。

[!code-csharp[](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

次のサンプルでは、シリアル化可能なオブジェクトを設定し、取得する方法を確認できます。

[!code-csharp[](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]

## <a name="working-with-httpcontextitems"></a>HttpContext.Items を使用する

`HttpContext` 抽象化では、`Items` と呼ばれている、型 `IDictionary<object, object>` のディクショナリ コレクションがサポートされます。 このコレクションは *HttpRequest* の開始から利用できます。各要求の終わりに破棄されます。 キー付きエントリに値を割り当てるか、特定のキーの値を要求することでアクセスできます。

次のサンプルでは、[Middleware](xref:fundamentals/middleware/index) により `isVerified` が `Items` コレクションに追加されます。

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

後のパイプラインで別のミドルウェアがこれにアクセスできます。

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " +
        context.Items["isVerified"]);
});
```

1 つのアプリでのみ使用されるミドルウェアの場合、`string` キーが許容されます。 ただし、アプリケーション間で共有されるミドルウェアの場合、キーの競合を回避するために、一意のオブジェクト キーを利用してください。 複数のアプリケーションで動作させるミドルウェアを開発する場合、下の図のように、ミドルウェア クラスに定義されている一意のオブジェクト キーを使用します。

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

その他のコードは、ミドルウェア クラスで公開されるキーを利用し、`HttpContext.Items` に格納されている値にアクセスできます。

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

この手法には、コード内の複数の場所で "鍵になる文字列" を繰り返すことをなくすという利点もあります。

## <a name="application-state-data"></a>アプリケーション状態データ

[依存関係の注入](xref:fundamentals/dependency-injection)を利用し、すべてのユーザーがデータを利用できるようにします。

1. データに含まれているサービスを定義します (たとえば、`MyAppData` という名前のクラス)。

    ```csharp
    public class MyAppData
    {
        // Declare properties/methods/etc.
    } 
    ```

2. サービス クラスを `ConfigureServices` に追加します (`services.AddSingleton<MyAppData>();` など)。

3. 各コントローラーのデータ サービス クラスを使用します。

    ```csharp
    public class MyController : Controller
    {
        public MyController(MyAppData myService)
        {
            // Do something with the service (read some data from it, 
            // store it in a private field/property, etc.)
        }
    } 
    ```

## <a name="common-errors-when-working-with-session"></a>セッションの使用時の一般的なエラー

* "'Microsoft.AspNetCore.Session.DistributedSessionStore' を起動しようとしましたが、型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' のサービスを解決できません。"

  これは通常、少なくとも 1 つの `IDistributedCache` 実装で構成に失敗したことで発生します。 詳細については、[分散キャッシュの使用](xref:performance/caching/distributed)と、[メモリ内キャッシュ](xref:performance/caching/memory)に関するページを参照してください。

* セッション ミドルウェアがセッションを永続化できなかった場合 (データベースが利用できなかったなど)、例外をログに記録し、受け取られます。 要求は通常どおり続行され、まったく予想できない動作につながります。

典型的な例:

誰かがセッションに買い物カゴを保管します。 アイテムを追加しますが、コミットが失敗します。 アプリはこの失敗を認識せず、"アイテムが追加されました" というメッセージを伝えますが、それは本当ではありません。

このようなエラーを確認するために推奨される方法は、セッションへの書き込みを終えた時点でアプリ コードから `await feature.Session.CommitAsync();` を呼び出すことです。 その後、エラーを自由に処理できます。 `LoadAsync` を呼び出すとき、同様の動作が行われます。

### <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core 1.x: このドキュメントで使用されたサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x: このドキュメントで使用されたサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
