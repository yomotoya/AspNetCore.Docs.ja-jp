---
title: "ASP.NET Core でのセッションおよびアプリケーションの状態"
author: rick-anderson
description: "要求間で維持アプリケーションとユーザー (セッション) 状態にアプローチです。"
keywords: "アプリケーションの状態、セッション状態、クエリ文字列での ASP.NET Core の投稿します。"
ms.author: riande
manager: wpickett
ms.date: 06/08/2017
ms.topic: article
ms.assetid: 18cda488-0769-4cb9-82f6-4c6685f2045d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/app-state
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b451bde1e3180d12781d55113638cc1a99182c8
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>ASP.NET Core でのセッションおよびアプリケーションの状態の概要

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](https://ardalis.com/)、および[Diana LaRose](https://github.com/DianaLaRose)

HTTP は、ステートレス プロトコルです。 Web サーバーでは、独立した要求として、各 HTTP 要求を処理し、以前の要求からユーザーの値を保持しません。 この記事では、アプリケーションと要求間でセッション状態を保持するためにさまざまな方法について説明します。 

## <a name="session-state"></a>セッションの状態

セッション状態は、保存し、ユーザーが web アプリを参照中にユーザー データの格納に使用できる ASP.NET Core の機能です。 サーバー上のディクショナリまたはハッシュ テーブルで構成される、セッション状態は、ブラウザーからの要求間でデータを保持します。 セッション データは、キャッシュによってバックアップされます。

ASP.NET Core は、クライアントには各要求を使用してサーバーに送信されたセッション ID が含まれた cookie を提供することにより、セッション状態を維持します。 サーバーは、セッション データをフェッチするのにセッション ID を使用します。 セッション cookie はブラウザーに固有であるためのブラウザーでセッションを共有することはできません。 ブラウザー セッションの終了時にのみ、セッション cookie が削除されます。 期限切れのセッションの cookie を受信すると、同じセッションの cookie を使用する新しいセッションが作成されます。 

サーバーは、最後の要求の後に限られた時間のセッションを保持します。 セッションのタイムアウトを設定するか、20 分間の既定値を使用します。 セッション状態は、特定のセッションに固有でが完全に永続化する必要はありませんユーザー データの格納に最適です。 呼び出すか、データをバッキング ストアから削除`Session.Clear`データ ストアに、セッションが期限切れにするか。 サーバーは、ブラウザーを閉じているときに、またはセッションの cookie が削除されたときに認識されません。

> [!WARNING]
> セッションでは、機密データを格納しないでください。 クライアント可能性がありますいないブラウザーを閉じて、セッションの cookie をクリア (および一部のブラウザーが windows の間でセッション cookie を維持する。)。 また、セッションできない可能性があります。 1 人のユーザーに制限次のユーザーは、同じセッションと続ける可能性があります。

メモリ内のセッション プロバイダーは、ローカル サーバー上のセッション データを格納します。 サーバー ファームで web アプリを実行する場合は、特定のサーバーには、各セッションを関連付けるにスティッキー セッションを使用する必要があります。 Windows Azure Web サイトのプラットフォームでは、スティッキー セッション アプリケーション要求ルーティング処理 (ARR) が既定値です。 ただし、スティッキー セッションはスケーラビリティに影響を与えるし、web アプリの更新プログラムが複雑になることができます。 良いオプションは、Redis を使用する、またはスティッキー セッションを必要としないが、SQL Server の分散をキャッシュします。 詳細については、次を参照してください。[分散キャッシュを使用して作業](xref:performance/caching/distributed)です。 サービス プロバイダーの設定の詳細については、「[構成セッション](#configuring-session)この記事で後述します。

このセクションの残りの部分では、ユーザー データを格納するためのオプションについて説明します。

<a name="temp"></a>
### <a name="tempdata"></a>TempData

ASP.NET Core MVC を公開、 [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData)プロパティを[コント ローラー](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)です。 このプロパティは、読み取られるまでデータを格納します。 `Keep` メソッドと `Peek` メソッドは、削除せずにデータを確認するために使用できます。 `TempData`1 つの要求より多くのデータが必要なときにこのプロパティの値はリダイレクト、特に便利です。 `TempData`セッション状態の上に作成されています。 

## <a name="cookie-based-tempdata-provider"></a>Cookie ベース TempData プロバイダー 

ASP.NET Core 1.1 以降では、cookie にユーザーの TempData を格納するのに、クッキー ベース TempData プロバイダーを使用できます。 Cookie ベースの TempData プロバイダーを有効にするには登録、`CookieTempDataProvider`サービス`ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add CookieTempDataProvider after AddMvc and include ViewFeatures.
    // using Microsoft.AspNetCore.Mvc.ViewFeatures;
    services.AddSingleton<ITempDataProvider, CookieTempDataProvider>();
}
```

Cookie のデータがでエンコードされた、 [Base64UrlTextEncoder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.authentication.base64urltextencoder)です。 Cookie が暗号化されており、チャンク、ため、1 つの cookie サイズの制限は適用されません。 Cookie のデータが圧縮されていないため、暗号化されたデータの圧縮が問題につながるセキュリティなど、 [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit))と[侵害](https://wikipedia.org/wiki/BREACH_(security_exploit))攻撃です。 Cookie ベースの TempData プロバイダーの詳細については、次を参照してください。 [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs)です。

### <a name="query-strings"></a>クエリ文字列

新しい要求のクエリ文字列に追加して 1 つの要求からで、限られた量のデータを渡すことができます。 これは、電子メールやソーシャル ネットワークを介して共有する埋め込みの状態を持つリンクを許可する永続的な方法で状態をキャプチャするために役立ちます。 ただし、このためを使用しないでクエリ文字列機密性の高いデータ。 簡単に共有されているだけでなくクエリ文字列にデータを含めることができます作成の営業案件[クロスサイト リクエスト フォージェリ (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))攻撃は、ユーザーが認証中に悪意のあるサイトにアクセスするを騙してことができます。 攻撃者は、アプリからユーザー データを盗む、またはユーザーの代理として、悪意のあるアクションを実行します。 保持されているアプリケーションまたはセッション状態は、CSRF 攻撃から保護する必要があります。 CSRF 攻撃の詳細については、次を参照してください。 [ASP.NET Core で防止サイト間で要求の偽造防止 (XSRF/CSRF) 攻撃](../security/anti-request-forgery.md)です。

### <a name="post-data-and-hidden-fields"></a>Post データと非表示フィールド

データは、隠しフォーム フィールドに保存されているし、次の要求にポストされたことができます。 これは、複数ページのフォームで共通です。 ただし、クライアントは、データを改ざんする可能性があります、ため、サーバー必要があります常に再検証します。 

### <a name="cookies"></a>クッキー

Cookie は、web アプリケーションでユーザーに固有のデータを格納する方法を提供します。 要求ごとに cookie が送信されるため、サイズを最小限に抑える必要があります。 理想的には、識別子のみは、サーバーに格納されている実際のデータを cookie に保存する必要があります。 ほとんどのブラウザーは、cookie を 4096 バイトに制限します。 さらに、限定された数の cookie のみが各ドメインで使用できます。  

クッキーは、改ざんされる可能性がありますが、ためには、サーバーで検証する必要があります。 クライアントでクッキーの持続性は、ユーザーが操作の対象と有効期限は、クライアント上のデータの持続性の最も持続性のある形式では通常。

Cookie は、多くの場合、既知のユーザーのコンテンツをカスタマイズするパーソナル化の使用します。 ユーザーを識別のみほとんどの場合で認証されていないため、cookie にユーザー名、アカウント名、または (GUID) などの一意のユーザー ID を格納することによって通常 cookie を保護できます。 サイトのユーザーのパーソナル化インフラストラクチャにアクセスするのに、cookie を使用できます。

### <a name="httpcontextitems"></a>HttpContext.Items

`Items`コレクションが格納されるデータに適した場所に必要な 1 つ特定の要求を処理中にのみです。 コレクションの内容は、各要求の後に破棄されます。 `Items`コレクションを最適な使用のコンポーネントまたはミドルウェア手段としてを通信時に、要求でさまざまなポイントで動作があり、パラメーターを渡す直接的な方法はありません。 詳細については、次を参照してください。 [HttpContext.Items 扱う](#working-with-httpcontextitems)、この記事で後述します。

### <a name="cache"></a>キャッシュ

キャッシュは、格納およびデータを取得する効率的な方法です。 時間と他の考慮事項に基づいて、キャッシュされた項目の有効期間を制御できます。 詳細については[キャッシュ](../performance/caching/index.md)です。

<a name=session></a>

## <a name="configuring-session"></a>セッションの構成

`Microsoft.AspNetCore.Session`パッケージは、セッション状態を管理するためのミドルウェアを提供します。 セッションのミドルウェアを有効にする`Startup`含める必要があります。

- いずれか、 [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)メモリ キャッシュします。 `IDistributedCache`実装は、セッションのバッキング ストアとして使用されます。
- [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_)呼び出すには、NuGet パッケージ"Microsoft.AspNetCore.Session"する必要があります。
- [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_)呼び出します。

次のコードでは、メモリ内のセッション プロバイダーを設定する方法を示します。

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

セッションを参照する`HttpContext`がインストールされ構成されているとします。

アクセスしようとする`Session`する前に`UseSession`が呼び出されると、例外`InvalidOperationException: Session has not been configured for this application or request`がスローされます。

新しいを作成しようとすると`Session`(つまり、セッションの cookie が作成されていません) への書き込みを開始した後、`Response`ストリーム、例外`InvalidOperationException: The session cannot be established after the response has started`がスローされます。 Web サーバー ログには、例外が見つかりませんブラウザーに表示されません。

### <a name="loading-session-asynchronously"></a>セッションを非同期的に読み込む 

ASP.NET Core の既定のセッション プロバイダーが、基になるからセッションのレコードを読み込みます[IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)ストア場合に非同期的にのみ、 [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync)メソッドが明示的にする前に呼び出されます `TryGetValue`、 `Set`、または`Remove`メソッドです。 場合`LoadAsync`は呼び出されません最初に、基になるセッション レコードが同期的に読み込まれると、アプリの標準化機能する可能性のある影響を与える可能性です。

このパターンを適用するアプリケーションを表示するには、ラップ、 [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore)と[DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession)例外をスローするバージョンでの実装、`LoadAsync`メソッドではありません前に呼び出される`TryGetValue`、 `Set`、または`Remove`です。 ラップされたバージョンをサービス コンテナーに登録します。

### <a name="implementation-details"></a>実装の詳細

セッションは、追跡し、1 つのブラウザーからの要求を特定する cookie を使用します。 既定では、この cookie が名前付き"です。パスを使用して AspNet.Session"、「/」です。 Cookie の既定値は、ドメインを指定しないので、利用できるがないクライアント側スクリプトを作成 ページで (ため`CookieHttpOnly`の既定値は`true`)。

セッションの既定値をオーバーライドする`SessionOptions`:

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

サーバーを使用して、`IdleTimeout`セッションできる期間アイドル状態の内容を破棄する前に決定するプロパティです。 このプロパティは、cookie の有効期限の独立しました。 セッションのミドルウェア (読み取りまたはに書き込まれます) を通過する各要求では、タイムアウト値をリセットします。

`Session`は*ロックしない*両方で、最後の 1 つのセッションの内容の変更を試みる次の 2 つの要求が 1 つをオーバーライドする場合、します。 `Session`として実装された、*一貫性のあるセッション*、つまり、すべての内容が一緒に格納されます。 セッション (異なるキー) のさまざまな部分を変更している 2 つの要求が互いに影響することがありますもします。

## <a name="setting-and-getting-session-values"></a>設定またはセッションの値を取得します。

セッションを介してへのアクセス、`Session`プロパティ`HttpContext`です。 このプロパティは、 [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession)実装します。

設定または int 型と文字列を取得する次の例を示しています。

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet1)]

次の拡張メソッドを追加する場合は、設定およびセッションにシリアル化可能なオブジェクトを取得できます。

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

次の例では、設定し、シリアル化可能なオブジェクトを取得する方法を示します。

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>HttpContext.Items の操作

`HttpContext`抽象化では、型のディクショナリ コレクションのサポート`IDictionary<object, object>`という`Items`です。 先頭からこのコレクションは使用可能な*HttpRequest*要求ごとの最後に破棄されます。 キー付きのエントリに値を割り当てるか、特定のキーの値を要求することによってアクセスできます。

以下のサンプルで[ミドルウェア](middleware.md)追加`isVerified`を`Items`コレクション。

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

パイプラインで後では、他のミドルウェアにアクセスできます。

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

1 つのアプリでのみ使用されるミドルウェアの`string`キーは許容されます。 ただし、ミドルウェア アプリケーション間で共有されるは、キーの競合の可能性を回避するのに一意のオブジェクトのキーを使用する必要があります。 複数のアプリケーション間で動作する必要があるミドルウェアを開発している場合は、次のように、ミドルウェア クラスで定義されている一意のオブジェクト キーを使用します。

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

その他のコードに格納されている値にアクセスできます`HttpContext.Items`ミドルウェア クラスによって公開キーを使用します。

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

この方法では、コード内の複数の場所の「マジック文字列」の繰り返しを排除することの利点もあります。

<a name=appstate-errors></a>

## <a name="application-state-data"></a>アプリケーション状態データ

使用して[依存性の注入](xref:fundamentals/dependency-injection)すべてのユーザーにデータを使用できるようにします。

1. データを含むサービスを定義する (たとえば、という名前のクラス`MyAppData`)。

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. サービス クラスを追加`ConfigureServices`(たとえば`services.AddSingleton<MyAppData>();`)。
3. 各コント ローラーで、データ サービス クラスを利用します。

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

### <a name="common-errors-when-working-with-session"></a>セッションを使用する場合の一般的なエラー

* 「'Microsoft.AspNetCore.Session.DistributedSessionStore' をアクティブ化しようとしました。 型 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' のサービスを解決するのにはできません。」

  これは、少なくとも 1 つの構成に失敗により発生通常`IDistributedCache`実装します。 詳細については、次を参照してください。[分散キャッシュを使用して作業](xref:performance/caching/distributed)と[メモリ キャッシュに](xref:performance/caching/memory)です。

### <a name="additional-resources"></a>その他のリソース


* [このドキュメントで使用するサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
