---
title: NSwag と ASP.NET Core の概要
author: zuckerthoben
description: NSwag を使用し、ASP.NET Core Web API のドキュメント ページとヘルプ ページを生成する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: f4cc9ec1f32ef2bd0056ba8d0cbbbe9228834d85
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279205"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>NSwag と ASP.NET Core の概要

作成者: [Christoph Nienaber](https://twitter.com/zuckerthoben) および [Rico Suter](https://rsuter.com)

::: moniker range="<= aspnetcore-2.0"
[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。
::: moniker-end

[NSwag](https://github.com/RSuter/NSwag) と共に ASP.NET Core ミドルウェアを使用するには、[NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet パッケージが必要です。 このパッケージは、Swagger ジェネレーター、Swagger UI (v2 と v3)、[ReDoc UI](https://github.com/Rebilly/ReDoc) で構成されています。

NSwag のコード生成機能を利用することを強くお勧めします。 コード生成に次のいずれかのオプションを選択します。

* [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio) を使用します。これは API のクライアント コードを C# と TypeScript で生成するための Windows デスクトップ アプリです。
* [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) または [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet パッケージを使用し、プロジェクト内でコードを生成します。
* [コマンド ライン](https://github.com/NSwag/NSwag/wiki/CommandLine)から NSwag を使用します。
* [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet パッケージを使用します。

## <a name="features"></a>フィーチャー

NSwag を使用する主な理由は、Swagger UI と Swagger ジェネレーターを導入できることのみならず、柔軟なコード生成機能を利用できることにあります。 既存の API は必要ありません&mdash;Swagger が組み込まれているサードパーティ製の API を利用し、NSwag にクライアント実装を生成させることができます。 いずれの方法でも、開発周期が早まり、API 変更に合わせた調整が簡単になります。

## <a name="package-installation"></a>パッケージ インストール

NSwag NuGet パッケージは、次の方法で追加できます。

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **[パッケージ マネージャー コンソール]** ウィンドウから:
  * **[ビュー]** > **[Other Windows]** \(その他の Windows\) > **[パッケージ マネージャー コンソール]** に移動します。
  * *TodoApi.csproj* ファイルが存在するディレクトリに移動します。
  * 次のコマンドを実行します。

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* **[NuGet パッケージの管理]** ダイアログ ボックスから:
  * **[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。
  * **パッケージ ソース**を "nuget.org" に設定します。
  * 検索ボックスに「NSwag.AspNetCore」と入力します。
  * **[参照]** タブから "NSwag.AspNetCore" パッケージを選択して、**[インストール]** をクリックします。

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[Solution Pad]** > **[パッケージを追加]** で [*パッケージ*] フォルダーを右クリックします。
* **[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。
* 検索ボックスに「NSwag.AspNetCore」と入力します。
* 結果ウィンドウから NSwag.AspNetCore パッケージを選択し、**[パッケージを追加]** をクリックします。

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**統合端末**からから次のコマンドを実行します。

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

次のコマンドを実行します。

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Swagger ミドルウェアを追加して構成する

`Startup` クラスで次の名前空間をインポートします。

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

`Startup.Configure` メソッドで、生成された Swagger 仕様と SwaggerUI 対応のミドルウェアを有効にします。

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

アプリを起動します。 `http://localhost:<port>/swagger` に移動し、Swagger UI を表示します。 `http://localhost:<port>/swagger/v1/swagger.json` に移動し、Swagger 仕様を表示します。

## <a name="code-generation"></a>コード生成

### <a name="via-nswagstudio"></a>NSwagStudio から

* 公式 [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio) リポジトリから NSwagStudio をインストールします。
* NSwagStudio を起動します。 **[Swagger Specification URL]** \(Swagger 仕様 URL\) テキストボックスに *swagger.json* ファイルの URL を入力し、**[Create local Copy]** \(ローカル コピーの作成\) ボタンをクリックします。
* **[CSharp Client]** \(CSharp クライアント\) クライアント出力の種類を選択します。 他の選択肢には、**[TypeScript Client]\(TypeScript クライアント\)**、**[CSharp Web API Controller]\(CSharp Web API コントローラー\)** があります。 Web API コントローラーは基本的に逆生成です。 サービスの仕様を利用してサービスを再構築します。
* **[Generate Outputs]** \(出力の生成\) ボタンをクリックします。 *TodoApi.NSwag* プロジェクトの完全な C# クライアントの実装が作成されます。 **[出力]** セクションの **[CSharp Client]** \(CSharp クライアント\) タブをクリックし、生成されたクライアント コードを表示します。

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> C# クライアント コードは、**[CSharp Client]** \(CSharp クライアント\) タブの **[設定]** タブに定義された設定に基づき生成されます。この設定を変更して、既定の名前空間の名前変更や同期メソッドの生成などのタスクを実行します。

* 生成された C# コードは、クライアント プロジェクトのファイルに入れます (例: [Xamarin.Forms](/xamarin/xamarin-forms/) アプリ)。
* Web API の使用を開始します。

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> ベース URL または HTTP クライアントを API クライアントに挿入できます。 ベスト プラクティスは常に [HttpClient を再利用すること](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)です。

### <a name="other-ways-to-generate-client-code"></a>クライアント コードを生成するその他の方法

自分のワークフローにより適した、他の方法でクライアント コードを生成できます。

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [コードで](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [T4 テンプレート](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a>カスタマイズ

Swagger には、Web API の消費を容易にする、オブジェクト モデルをドキュメント化するオプションがあります。

### <a name="api-info-and-description"></a>API 情報と説明

`Startup.Configure`メソッドでは、`UseSwagger` メソッドに渡された構成アクションによって、作成者、ライセンス、説明などの情報が追加されます。

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

Swagger UI には、バージョンの情報が表示されます。

![バージョン情報を含む Swagger UI](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a>XML コメント

XML コメントは次の方法で有効になります。

# <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)

* **ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** を選択します。
* **[ビルド]** タブの **[出力]** セクションの下にある **[XML ドキュメント ファイル]** チェック ボックスをオンにします。

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml/)

* **[プロジェクト オプション]** ダイアログ > **[ビルド]** > **[コンパイラ]** を開きます。
* **[全般オプション]** セクションの下にある **[XML ドキュメントを生成する]** チェック ボックスをオンにします。

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)

次のスニペットを *.csproj* ファイルに手動で追加します。

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a>データの注釈

::: moniker range="<= aspnetcore-2.0"
NSwag では、[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) が使用されます。Web API アクションに推奨される戻り値の型は [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) です。 結果的に、NSwag では、アクションの内容とそれで返されるものを推論できません。 次に例を示します。

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

先行するアクションによって `IActionResult` が返されますが、このアクションの内部では [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) または [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) を返しています。 データ注釈は、このアクションによって返される既知の HTTP 応答コードをクライアントに通知します。 アクションに次の属性を追加します。

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
NSwag では、[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) が使用されます。Web API アクションに推奨される戻り値の型は [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) です。 その結果、NSwag では `T` によって定義された戻り値のみ推論できます。 アクションのその他の可能な戻り値は推論できません。 次に例を示します。

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]

先行するアクションによって `ActionResult<T>` が返されますが、このアクションの内部では [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) を返しています。 コントローラーは、[[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 属性で修飾されるため、応答として [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) も可能です。 詳細については、「[自動的な HTTP 400 応答](xref:web-api/index#automatic-http-400-responses)」を参照してください。 データ注釈は、このアクションによって返される既知の HTTP 応答コードをクライアントに通知します。 アクションに次の属性を追加します。

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]
::: moniker-end

Swagger ジェネレーターでは、このアクションを正確に表現できるようになりました。生成されたクライアントでは、エンドポイントの呼び出し時に受け取るものが認識されます。 すべてのアクションにこれらの属性を追加することを強くお勧めします。 API アクションで返される HTTP 応答のガイドラインについては、[RFC 7231 仕様](https://tools.ietf.org/html/rfc7231#section-4.3)をご覧ください。
