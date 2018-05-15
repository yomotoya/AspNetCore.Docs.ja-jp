---
title: NSwag と ASP.NET Core の概要
author: zuckerthoben
description: NSwag を使用し、ASP.NET Core Web API アプリのドキュメント ページとヘルプ ページを生成する方法について説明します。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>NSwag と ASP.NET Core の概要

作成者: [Christoph Nienaber](https://twitter.com/zuckerthoben) / [Rico Suter](https://rsuter.com)

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

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **[パッケージ マネージャー コンソール]** ウィンドウから:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* **[NuGet パッケージの管理]** ダイアログ ボックスから:
  * **[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。
  * **パッケージ ソース**を "nuget.org" に設定します。
  * 検索ボックスに「NSwag.AspNetCore」と入力します。
  * **[参照]** タブから "NSwag.AspNetCore" パッケージを選択して、**[インストール]** をクリックします。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[Solution Pad]** > **[パッケージを追加]** で [*パッケージ*] フォルダーを右クリックします。
* **[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。
* 検索ボックスに「NSwag.AspNetCore」と入力します。
* 結果ウィンドウから NSwag.AspNetCore パッケージを選択し、**[パッケージを追加]** をクリックします。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**統合端末**からから次のコマンドを実行します。

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

次のコマンドを実行します。

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Swagger ミドルウェアを追加して構成する

`Info` クラスで次の名前空間をインポートします。

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

`Startup.Configure` メソッドで、生成された Swagger 仕様と SwaggerUI 対応のミドルウェアを有効にします。

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

アプリを起動します。 `/swagger` に移動し、Swagger UI を表示します。 `/swagger/v1/swagger.json` に移動し、Swagger 仕様を表示します。

## <a name="code-generation"></a>コード生成

### <a name="via-nswagstudio"></a>NSwagStudio から

* 公式 [GitHub リポジトリ](https://github.com/RSuter/NSwag/wiki/NSwagStudio)から `NSwagStudio` をインストールします。

* NSwagStudio を起動します。 *swagger.json* の場所を入力するか、直接コピーします。

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* クライアントの出力の種類を指定します。 選択肢には、**[TypeScript Client]\(TypeScript クライアント\)**、**[CSharp Client]\(CSharp クライアント\)**、**[CSharp Web API Controller]\(CSharp Web API コントローラー\)** があります。 Web API コントローラーは基本的に逆生成です。 サービスの仕様を利用してサービスを再構築します。

* **[Generate Outputs]\(出力の生成\)** をクリックします。

* C# の TodoApi.NSwag クライアント実装の完全なサンプルは*こちらで*ご覧いただけます。

![NSwagStudio - 出力](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* ファイルをクライアント プロジェクトに入れます (例: [Xamarin.Forms](/xamarin/xamarin-forms/) アプリ)。 API の使用を開始します。

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> ベース URL または HTTP クライアントを API クライアントに挿入できます。 ベスト プラクティスは常に [HttpClient を再利用すること](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)です。

これで API をクライアント プロジェクトに簡単に実装できます。

### <a name="other-ways-to-generate-client-code"></a>クライアント コードを生成するその他の方法

自分のワークフローにより適した、他の方法でコードを生成できます。

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [コードで](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [T4 テンプレート](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>カスタマイズ

### <a name="xml-comments"></a>XML コメント

XML コメントは次の方法で有効になります。

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* **ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** を選択します。
* **[ビルド]** タブの **[出力]** セクションの下にある **[XML ドキュメント ファイル]** チェック ボックスをオンにします。

![プロジェクトのプロパティの [ビルド] タブ](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio for Mac](#tab/visual-studio-mac-xml/)
* **[プロジェクト オプション]** ダイアログ > **[ビルド]** > **[コンパイラ]** を開きます。
* **[全般オプション]** セクションの下にある **[XML ドキュメントを生成する]** チェック ボックスをオンにします。

![プロジェクトのオプションの [全般オプション] セクション](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)
次のスニペットを *.csproj* ファイルに手動で追加します。

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a>データの注釈

NSwag では、[Reflection](/dotnet/csharp/programming-guide/concepts/reflection) が使用されます。Web API アクションのベスト プラクティスは [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) を返すことです。 結果的に、NSwag では、アクションの内容とそれで返されるものを推論できません。 次に例を示します。

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

先行するアクションによって `IActionResult` が返されますが、このアクションの内部では [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) または [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) を返しています。 データ注釈を利用し、このアクションによって返される HTTP 応答をクライアントに通知します。 アクションに次の属性を追加します。

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Swagger ジェネレーターでは、このアクションを正確に表現できるようになりました。生成されたクライアントでは、エンドポイントの呼び出し時に受け取るものが認識されます。 すべてのアクションにこれらの属性を追加することを強くお勧めします。 API アクションで返される HTTP 応答のガイドラインについては、[RFC 7231 仕様](https://tools.ietf.org/html/rfc7231#section-4.3)をご覧ください。
