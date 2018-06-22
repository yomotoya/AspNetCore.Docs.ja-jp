---
title: ASP.NET Core での統合テスト
author: guardrex
description: 統合テストによってデータベース、ファイル システム、ネットワークなどのインフラストラクチャ レベルで、アプリのコンポーネントがどのように正しく機能するようになるかを説明します。
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: 1895b06f1af9a9eb66c14aa5c7834497fc95d583
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277697"
---
# <a name="integration-tests-in-aspnet-core"></a>ASP.NET Core での統合テスト

によって[Luke Latham](https://github.com/guardrex)と[Steve Smith](https://ardalis.com/)

統合テストをアプリのサポートなどのインフラストラクチャ、データベース、ファイル システム、およびネットワークを含むレベルで、アプリのコンポーネントが正しく動作することを確認します。 ASP.NET Core では、テストの web ホストとメモリ内のテスト サーバーを単体テスト フレームワークを使用して統合テストをサポートします。

このトピックは、単体テストの基本的な知識を前提とします。 場合テストの概念についてよく知らないを参照してください、[単体テストを .NET Core と .NET Standard](/dotnet/core/testing/)トピックとそのリンクのコンテンツ。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

サンプル アプリは、Razor ページのアプリで Razor ページの基本的な知識を前提としています。 Razor ページに慣れていないの場合は、次のトピックを参照してください。

* [Razor ページを始める](xref:razor-pages/index)
* [Razor ページの概要](xref:tutorials/razor-pages/razor-pages-start)
* [Razor ページの単体テスト](xref:test/razor-pages-tests)

## <a name="introduction-to-integration-tests"></a>統合テストの概要

統合テストがより広範なレベルでのアプリのコンポーネントを評価[単体テスト](/dotnet/core/testing/)です。 単体テストは、個々 のクラスのメソッドなど、隔離されたソフトウェア コンポーネントをテストに使用されます。 統合テストでは、可能性のある要求を完全に処理するために必要なすべてのコンポーネントを含む、予期される結果を生成するために 2 つ以上のアプリ コンポーネントを一緒に使用することを確認します。

このようなより広範なテストは、アプリのインフラストラクチャと、次のコンポーネントを含む多くの場合、フレームワーク全体をテストに使用されます。

* データベース
* ファイル システム
* ネットワーク アプライアンス
* 要求-応答のパイプライン

単体テストの作成を使用するコンポーネントと呼ばれる*fakes*または*オブジェクトをモック*インフラストラクチャのコンポーネントの代わりにします。

単体テストとは対照的統合をテストします。

* アプリを実稼働環境で使用する実際のコンポーネントを使用します。
* 多くのコードとデータ処理が必要です。
* 実行時間長くなります。

したがって、最も重要なインフラストラクチャのシナリオに統合テストの使用を制限します。 場合は、動作をテストするには、単体テストまたは統合テストを使用して、単体テストを選択します。

> [!TIP]
> データベースおよびファイル システムでのデータとファイルのアクセスの可能な各順列の統合テストを記述しません。 アプリ間でどのくらいの配置に関係なくデータベースおよびファイル システム、フォーカスのある一連の読み取り、書き込み、更新、および削除の統合テストは、十分にテスト データベースのことし、ファイル システムのコンポーネントと対話します。 これらのコンポーネントと対話するメソッドのロジックのルーチンのテスト テスト単位を使用します。 単体テストでインフラストラクチャを使用して fakes/モック テスト実行の速さで結果。

> [!NOTE]
> 統合テストのディスカッションにテスト対象のプロジェクトは頻繁に呼び出される、*テスト対象のシステム*、または略して"SUT"です。

## <a name="aspnet-core-integration-tests"></a>ASP.NET Core 統合テスト

ASP.NET Core での統合テストには、次の必要があります。

* テスト プロジェクトを含めるし、テストを実行するのに使用します。 テスト プロジェクトと呼ばれる、テスト対象の ASP.NET Core プロジェクトへの参照では、*テスト対象のシステム*(SUT)。 _このトピック全体では、"SUT"を使用してテスト済みのアプリを参照してください。_
* テスト プロジェクトでは、SUT のテストの web ホストを作成し、テスト サーバーのクライアントを使用して要求と SUT への応答を処理します。
* テスト ランナーを使用して、テスト結果、テストとレポートを実行できます。

統合テストを含む通常のイベントのシーケンスの実行*配置*、 *Act*、および*Assert*テスト ステップ。

1. SUT の web ホストが構成されています。
1. アプリへの要求を送信するには、テスト サーバーのクライアントが作成されます。
1. *配置*テスト ステップが実行された: テスト アプリケーションが要求を準備します。
1. *Act*テスト ステップが実行された: クライアント要求を送信し、応答を受信します。
1. *Assert*テスト ステップが実行された:*実際*応答が検証された、*渡す*または*失敗*に基づいて、*が必要です*応答します。
1. プロセスは、すべてのテストが実行されるまで続行します。
1. テスト結果が報告されます。

通常、テストの web ホストではテスト用のアプリの標準的な web ホストの実行よりも構成が異なる。 たとえば、別のデータベースまたは別のアプリの設定は、テストに使用可能性があります。

テストの web ホストとメモリ内のテスト サーバーなどのインフラストラクチャ コンポーネント ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)) が提供されるかによって管理される、 [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)パッケージです。 このパッケージの使用には、テストの作成と実行が簡略化します。

`Microsoft.AspNetCore.Mvc.Testing`パッケージは、次のタスクを処理します。

* 依存関係ファイルにコピー (*\*.deps*) にテスト プロジェクトの SUT から*bin*フォルダーです。
* 静的なファイルおよびページ/ビューは、テストを実行するときに検出できるようにするには、コンテンツのルート SUT のプロジェクトのルートを設定します。
* 提供、 [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)で SUT のブートス トラップを効率化するクラス`TestServer`です。

[単体テスト](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)テスト プロジェクトとテスト ランナー、と共に名前テストにテストおよび方法に関する推奨事項を実行し、クラスをテストする方法の詳細な説明を設定する方法について説明します。

> [!NOTE]
> アプリのテスト プロジェクトを作成する場合は、別々 のプロジェクトに統合テストから単体テストを区切ります。 こうことからインフラストラクチャのテストのコンポーネントの単体テストに含まれる誤っていないことを確認します。 単位との統合のテストの分離により、テストのセットでは実行を制御します。

事実上ない違いは Razor ページのアプリのテストの構成」および「MVC アプリです。 唯一の違いは、テストの名前付け方法です。 Razor ページ アプリケーションでは、ページのエンドポイントのテストの名前は通常、ページのモデル クラス後 (たとえば、`IndexPageTests`インデックス ページのコンポーネントの統合をテストする)。 MVC アプリケーションでテスト通常コント ローラー クラス別に整理され、テスト コント ローラーにちなんだ名前 (たとえば、 `HomeControllerTests` Home コント ローラー用のコンポーネントの統合をテストする)。

## <a name="test-app-prerequisites"></a>アプリの前提条件をテストします。

テスト プロジェクトである必要があります。

* パッケージ参照を持つ[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)です。
* Web SDK を使用して、プロジェクト ファイル (`<Project Sdk="Microsoft.NET.Sdk.Web">`)。

これら prerequesities がわかるように、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)です。 検査、 *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj*ファイル。

## <a name="basic-tests-with-the-default-webapplicationfactory"></a>既定値 WebApplicationFactory の基本的なテスト

[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)作成に使用される、 [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)統合テストにします。 `TEntryPoint` SUT のエントリ ポイント クラスは、通常、`Startup`クラスです。

テスト クラスで実装する*クラス フィクスチャ*インターフェイス (`IClassFixture`) を示し、クラス テストが含まれるクラス内のテストの間で共有されたオブジェクトのインスタンスを指定します。

### <a name="basic-test-of-app-endpoints"></a>アプリのエンドポイントの基本のテスト

次のテスト クラス、`BasicTests`を使用して、 `WebApplicationFactory` SUT をブートス トラップでき、 [HttpClient](/dotnet/api/system.net.http.httpclient)テスト メソッドに`Get_EndpointsReturnSuccessAndCorrectContentType`です。 チェックするメソッドが応答状態コードが成功したかどうか (状態コード 200 299 の範囲内で) および`Content-Type`ヘッダーが`text/html; charset=utf-8`のいくつかのアプリ ページ。

[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)のインスタンスを作成`HttpClient`を自動的にリダイレクトに依存して cookie を処理します。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a>セキュリティで保護されたエンドポイントをテストします。

内の別のテスト、`BasicTests`クラスは、セキュリティで保護されたエンドポイントが、アプリのログイン ページに認証されていないユーザーをリダイレクトすることを確認します。

SUT で、`/SecurePage`ページの使用、 [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)規則を適用する、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)ページにします。 詳細については、次を参照してください。 [Razor ページの承認規則](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)です。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

`Get_SecurePageRequiresAnAuthenticatedUser`をテストする、 [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)を設定してリダイレクトを許可しないように設定されている[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)に`false`:

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

クライアントのリダイレクトに従うを禁止して、次のチェックを変更できます。

* SUT によって返されるステータス コードをチェックして、予期されたに対して[HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode)結果であるログイン ページへのリダイレクトの後に最終的な状態コードではなく[HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).
* `Location`で始まっていることを確認する応答ヘッダーのヘッダーの値がチェックされて`http://localhost/Identity/Account/Login`、いない最終的なログイン ページの応答、場所、`Location`ヘッダーが存在するはありません。

詳細については`WebApplicationFactoryClientOptions`を参照してください、[クライアント オプション](#client-options)セクションです。

## <a name="customize-webapplicationfactory"></a>WebApplicationFactory をカスタマイズします。

継承することで、テスト クラスとは無関係に web ホストの構成を作成できます`WebApplicationFactory`を 1 つまたは複数のカスタム ファクトリを作成します。

1. 継承`WebApplicationFactory`オーバーライドと[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)です。 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)サービス コレクションの構成を許可[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   データベースのシード処理で、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)äs ' í、`InitializeDbForTests`メソッドです。 メソッドに記載されて、[統合サンプルをテストする: テスト アプリ組織](#test-app-organization)セクションです。

2. ユーザー設定を使用して`CustomWebApplicationFactory`テスト クラスでします。 次の例のファクトリを使用する、`IndexPageTests`クラス。

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   防ぐために、サンプル アプリのクライアントが構成されている、`HttpClient`次のリダイレクトから。 説明に従って、[セキュリティで保護されたエンドポイントをテスト](#test-a-secure-endpoint) セクションで、これにより、アプリの最初の応答の結果を確認するテストです。 最初の応答でこれらのテストの多くでリダイレクトを行うは、`Location`ヘッダー。

3. 一般的なテストを使用して、`HttpClient`と、要求と応答を処理するヘルパー メソッド。

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

SUT に POST 要求は、アプリのによって自動的に行われた antiforgery チェックを満たす必要があります[データ保護 antiforgery システム](xref:security/data-protection/introduction)です。 テストの POST 要求の配置、するために、アプリケーションをテストする必要があります。

1. ページの要求を行います。
1. Antiforgery cookie と、応答からの要求の検証トークンを解析します。
1. インプレース antiforgery cookie と要求の検証の POST 要求トークンを作成します。

`SendAsync`ヘルパー拡張メソッド (*Helpers/HttpClientExtensions.cs*) および`GetDocumentAsync`ヘルパー メソッド (*Helpers/HtmlHelpers.cs*) で、 [のサンプルアプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)を使用して、 [AngleSharp](https://anglesharp.github.io/) antiforgery チェックを以下の方法で処理するためにパーサー。

* `GetDocumentAsync` &ndash; 受信、 [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage)を返します、`IHtmlDocument`です。 `GetDocumentAsync` 準備するファクトリを使用する、*仮想応答*元に基づいて`HttpResponseMessage`です。 詳細については、次を参照してください。、 [AngleSharp ドキュメント](https://github.com/AngleSharp/AngleSharp#documentation)です。
* `SendAsync` 拡張メソッドを`HttpClient`作成、 [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)を呼び出すと[SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) SUT に要求を送信します。 オーバー ロード`SendAsync`HTML フォームを受け入れる (`IHtmlFormElement`) および次。
  - フォームのボタンの送信 (`IHtmlElement`)
  - フォーム値のコレクション (`IEnumerable<KeyValuePair<string, string>>`)
  - 送信ボタン (`IHtmlElement`) の値を形成し、(`IEnumerable<KeyValuePair<string, string>>`)

> [!NOTE]
> [AngleSharp](https://anglesharp.github.io/)サード パーティがこのトピックおよびサンプル アプリでのデモンストレーションのために使用されるライブラリに解析します。 AngleSharp が ASP.NET Core アプリケーションの統合のテストに必要なまたはサポートされていません。 その他のパーサー使用できますが、ように、 [Html アジリティ パック (HAP)](http://html-agility-pack.net/)です。 別の方法では、antiforgery システムの検証トークンの要求と antiforgery cookie を直接処理するコードを作成します。

## <a name="customize-the-client-with-withwebhostbuilder"></a>WithWebHostBuilder でクライアントをカスタマイズします。

追加の構成が必要な場合、テスト メソッド内で[WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)新たに作成`WebApplicationFactory`で、 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)ですが、構成でさらにカスタマイズできます。

`Post_DeleteMessageHandler_ReturnsRedirectToRoot`テストのメソッド、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)の使用例を示します`WithWebHostBuilder`です。 このテストは、SUT にフォームの送信をトリガーすることによって、データベース内のレコードの削除を実行します。

別のテストであるため、`IndexPageTests`クラスは、データベース内のレコードのすべてを削除し、前に実行する操作を実行、`Post_DeleteMessageHandler_ReturnsRedirectToRoot`メソッド、データベースは、レコードが削除 SUT に存在することを確認するには、このテスト メソッドのシードです。 選択すると、`deleteBtn1`のボタン、 `messages` SUT への要求に SUT でフォームをシミュレートします。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a>クライアントのオプション

次の表は、既定値を示しています。 [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)作成するときに使用可能な`HttpClient`インスタンス。

| オプション | 説明 | 既定値 |
| ------ | ----------- | ------- |
| [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | 取得または設定するかどうか`HttpClient`インスタンスは、リダイレクト応答に自動的に従う必要があります。 | `true` |
| [BaseAddress](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | 取得または設定のベース アドレス`HttpClient`インスタンス。 | `http://localhost` |
| [HandleCookies](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | 取得または設定するかどうか`HttpClient`インスタンスは、cookie を処理する必要があります。 | `true` |
| [MaxAutomaticRedirections](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | リダイレクト応答の最大数の設定を取得または`HttpClient`インスタンスが従う必要があります。 | 7 |

作成、`WebApplicationFactoryClientOptions`クラスに渡すと、 [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)メソッド (既定値は、コード例で表示されます)。

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a>テスト インフラストラクチャがアプリのコンテンツのルート パスを推論する方法

`WebApplicationFactory`コンス トラクターを検索して、アプリのコンテンツのルート パスを推測する、 [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)と等しいキーを使用して、統合テストを含むアセンブリを`TEntryPoint`アセンブリ`System.Reflection.Assembly.FullName`. 正しいキーを持つ属性が見つからない場合に`WebApplicationFactory`ソリューション ファイルを検索するフォールバック (*\*.sln*) を追加し、`TEntryPoint`ソリューション ディレクトリにアセンブリ名。 アプリケーションのルート ディレクトリ (コンテンツのルートのパス) を使用して、ビューとコンテンツ ファイルを検出します。

ほとんどの場合、必要はありませんアプリ コンテンツのルートを明示的に設定するように検索ロジックは、実行時に正しいコンテンツのルートを検索する通常します。 コンテンツのルートが見つからないの特殊なシナリオで明示的にまたはカスタム ロジックを使用して、ルートを指定できますコンテンツ アプリで、組み込みの検索アルゴリズムを使用します。 そのようなシナリオでアプリのコンテンツのルートを設定するには、呼び出し、`UseSolutionRelativeContentRoot`から拡張メソッド、 [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost)パッケージです。 ソリューションの相対パスと省略可能なソリューション ファイルの名前または glob パターンを指定 (既定 = `*.sln`)。

呼び出す、 [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot)メソッドを使用して拡張*1*次のどの方法。

* テスト クラスを構成するときに`WebApplicationFactory`でのカスタム構成を指定して、 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* カスタムのテスト クラスを構成するときに`WebApplicationFactory`から継承`WebApplicationFactory`オーバーライドと[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a>シャドウ コピーを無効にします。

シャドウ コピーすると、出力フォルダーとは異なるフォルダーで実行するテストが発生します。 正常に動作するテストでは、シャドウ コピーする必要があります無効になります。 [サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)xUnit を使用し、シャドウ コピー xUnit などによって無効になります、 *xunit.runner.json*適切な構成設定を持つファイルです。 詳細については、次を参照してください。 [xUnit.net を構成する JSON で](https://xunit.github.io/docs/configuring-with-json.html)です。

追加、 *xunit.runner.json*次のコンテンツを含むテスト プロジェクトのルートにファイル。

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a>統合テストのサンプル

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)は 2 個のアプリで構成されます。

| アプリ | プロジェクト フォルダー | 説明 |
| --- | -------------- | ----------- |
| メッセージ アプリ (SUT) | *src/RazorPagesProject* | ユーザーを追加、1 つを削除、削除、およびメッセージを分析できます。 |
| アプリのテスト | *tests/RazorPagesProject.Tests* | 統合テスト SUT するために使用します。 |

などの IDE の組み込みのテスト機能を使用してテストを実行できる[Visual Studio](https://www.visualstudio.com/vs/)です。 使用して場合[Visual Studio Code](https://code.visualstudio.com/)またはコマンド プロンプトで次のコマンドを実行、コマンドライン、 *tests/RazorPagesProject.Tests*フォルダー。

```console
dotnet test
```

### <a name="message-app-sut-organization"></a>メッセージ アプリ (SUT) 組織

SUT は、次の特性を持つ Razor ページ メッセージ システムを示します。

* アプリのインデックス ページ (*Pages/Index.cshtml*と*Pages/Index.cshtml.cs*) UI とページを追加、削除、およびメッセージ (メッセージあたりの平均ワード) の分析を制御するモデルのメソッドを提供.
* メッセージは、`Message`クラス (*Data/Message.cs*) 2 つのプロパティを持つ: `Id` (キー) と`Text`(メッセージ)。 `Text`プロパティは必須であり 200 文字までに制限されます。
* 使用してメッセージが格納される[Entity Framework のメモリ内のデータベース](/ef/core/providers/in-memory/)&#8224;。
* アプリには、データベース コンテキスト クラスのデータ アクセス層 (DAL) が含まれています`AppDbContext`(*Data/AppDbContext.cs*)。
* データベースがアプリの起動時に空の場合、メッセージ ストアは、3 つのメッセージで初期化されます。
* アプリが含まれています、`/SecurePage`認証済みユーザーによってのみアクセスできます。

&#8224;EF トピック[InMemory を伴うテスト](/ef/core/miscellaneous/testing/in-memory)MSTest でテストにメモリ内のデータベースを使用する方法について説明します。 このトピックでは、 [xUnit](https://xunit.github.io/)テスト フレームワーク。 テストの概念と別のテスト フレームワークの間でのテストの実装は、似ているが同一でです。

アプリを使用していないが、[リポジトリ パターン](http://martinfowler.com/eaaCatalog/repository.html)の有効な例が表示されない、[作業単位 (UoW) パターン](https://martinfowler.com/eaaCatalog/unitOfWork.html)、Razor ページには、開発のこれらのパターンがサポートされています。 詳細については、次を参照してください[インフラストラクチャの永続性レイヤーをデザイン](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)、 [ASP.NET MVC アプリケーションでリポジトリおよび単位の作業パターンを実装する](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)、および[テスト コント ローラー。ロジック](/aspnet/core/mvc/controllers/testing)(このサンプルは、リポジトリ パターンを実装)。

### <a name="test-app-organization"></a>組織のアプリをテストします。

テスト アプリケーションは、コンソール アプリケーション内、 *tests/RazorPagesProject.Tests*フォルダーです。

| App フォルダーのテスト | 説明 |
| --------------- | ----------- |
| *BasicTests* | *BasicTests.cs*ルーティング、認証されていないユーザー、セキュリティで保護されたページにアクセスして取得 GitHub ユーザー プロファイル、およびプロファイルのユーザーのログインを確認用のテスト メソッドが含まれています。 |
| *IntegrationTests* | *IndexPageTests.cs*ユーザー設定を使用して、インデックス ページの統合テストを含む`WebApplicationFactory`クラスです。 |
| *ヘルパー/ユーティリティ* | <ul><li>*Utilities.cs*が含まれています、`InitializeDbForTests`メソッドでテスト データとデータベースのシードに使用します。</li><li>*HtmlHelpers.cs* 、AngleSharp を返すメソッドを提供`IHtmlDocument`テスト メソッドで使用します。</li><li>*HttpClientExtensions.cs*のオーバー ロードを用意`SendAsync`SUT に要求を送信します。</li></ul> |

テスト フレームワークが[xUnit](https://xunit.github.io/)です。 使用して統合テストが実施、 [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost)が含まれている、 [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)です。 [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)テスト ホストとテスト サーバーを構成するパッケージが使用される、`TestHost`と`TestServer`パッケージは、テスト アプリのプロジェクト ファイルで直接パッケージ参照を必要としない、またはテスト アプリケーションの開発者用の構成。

**テスト用データベースのシード**

通常、統合テストには、テストの実行前にデータベース内の小さなデータセットが必要です。 たとえば、削除は、データベースが正常に削除要求の少なくとも 1 つのレコードをいる必要がありますのでデータベース レコードの削除の呼び出しをテストします。

サンプル アプリで次の 3 つのメッセージと共にデータベースがシード処理*Utilities.cs*テストが実行したときに使用できること。

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a>その他の技術情報

* [単体テスト](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Razor ページの単体テスト](xref:test/razor-pages-tests)
* [ミドルウェア](xref:fundamentals/middleware/index)
* [テスト コントローラー](xref:mvc/controllers/testing)
