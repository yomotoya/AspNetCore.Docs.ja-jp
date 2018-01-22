---
title: "Swagger を使用する ASP.NET Core Web API のヘルプ ページ"
author: spboyer
description: "このチュートリアルでは、Swagger を追加して、Web API アプリケーションのドキュメントとヘルプ ページを生成する手順を説明します。"
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: d044c820057dba762d3a0f621855a8f4e298ab23
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a>Swagger を使用する ASP.NET Core Web API のヘルプ ページ

<a name="web-api-help-pages-using-swagger"></a>

[Shayne Boyer](https://twitter.com/spboyer) および [Scott Addie](https://twitter.com/Scott_Addie) 著

開発者が、コンシューマー向けアプリケーションを作成するときには、API のさまざまな使用方法を理解するのが困難な場合があります。

[Swagger](https://swagger.io/) と .NET Core の実装 [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) を使用して Web API 向けのドキュメントとヘルプ ページを生成する作業は、いくつかの NuGet パッケージを追加して *Startup.cs* を変更するのと同じくらい簡単です。

* [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) は、ASP.NET Core Web API の Swagger ドキュメントを生成するためのオープン ソース プロジェクトです。

* [Swagger](https://swagger.io/) は、対話型のドキュメント、クライアント SDK の生成、および探索可能性のサポートを有効にする RESTful API のコンピューターが読み取り可能な表現です。

このチュートリアルでは、「[Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api)」(ASP.NET Core MVC と Visual Studio を使用した最初の Web API の構築) のサンプルを基にして作成します。 サンプルの手順に従う場合は、[https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) からサンプルをダウンロードしてください。

## <a name="getting-started"></a>作業の開始

Swashbuckle には 3 つの主要なコンポーネントがあります。

* `Swashbuckle.AspNetCore.Swagger`: `SwaggerDocument` オブジェクトを JSON エンドポイントとして公開するための Swagger オブジェクト モデルとミドルウェア。

* `Swashbuckle.AspNetCore.SwaggerGen`: ルート、コント ローラー、およびモデルから直接 `SwaggerDocument` オブジェクトを構築する Swagger ジェネレーター。 通常、Swagger エンドポイント ミドルウェアと組み合わせて Swagger JSON を自動的に公開します。

* `Swashbuckle.AspNetCore.SwaggerUI`: 埋め込みバージョンの Swagger UI ツールであり、Swagger JSON を解釈して、Web API の機能を説明するカスタマイズ可能な機能豊富なエクスペリエンスを構築します。 これには、パブリック メソッド用の組み込みのテスト ハーネスが含まれます。

## <a name="nuget-packages"></a>NuGet パッケージ

Swashbuckle は、次の方法で追加できます。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **[パッケージ マネージャー コンソール]** ウィンドウから:

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* **[NuGet パッケージの管理]** ダイアログ ボックスから:

     * **[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。
     * **パッケージ ソース**を "nuget.org" に設定します。
     * 検索ボックスに「Swashbuckle.AspNetCore」と入力します。
     * **[参照]** タブから "Swashbuckle.AspNetCore"パッケージを選択して、**[インストール]** をクリックします。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[Solution Pad]** > **[パッケージを追加]** で [*パッケージ*] フォルダーを右クリックします。
* **[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。
* 検索ボックスに「Swashbuckle.AspNetCore」と入力します。
* 結果ウィンドウから Swashbuckle.AspNetCore パッケージを選択し、**[パッケージを追加]** をクリックします。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**統合端末**からから次のコマンドを実行します。

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

次のコマンドを実行します。

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a>Swagger をミドルウェアに追加して構成します。

*Startup.cs* の `ConfigureServices` メソッドで Swagger ジェネレーターをサービス コレクションに追加します。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

次の `Info` クラスの using ステートメントを追加します。

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

*Startup.cs* の `Configure` メソッドで、生成された JSON ドキュメントと SwaggerUI 対応のミドルウェアを有効にします。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

アプリを起動し、`http://localhost:<random_port>/swagger/v1/swagger.json` に移動します。 エンドポイントを説明する生成されたドキュメントが表示されます。

**注:** Microsoft Edge、Google Chrome、および Firefox は JSON ドキュメントをネイティブで表示します。 読みやすくするためのドキュメントの書式設定する Chrome の拡張機能があります。 *簡略化のための次の例は短縮されています。*

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

このドキュメントでは、`http://localhost:<random_port>/swagger` ドライブに移動して表示できる Swagger UI をお勧めしています。

![Swagger UI](web-api-help-pages-using-swagger/_static/swagger-ui.png)

`TodoController` 内の各パブリック アクション メソッドを UI からテストすることができます。 メソッドの名前をクリックし、セクションを展開します。 任意の必要なパラメーターを追加し、[Try it out!] をクリックします。

![Swagger GET テストの例](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a>カスタマイズと拡張機能

Swagger は、テーマに合わせてオブジェクト モデルを文書化して、UI をカスタマイズするオプションを提供します。

### <a name="api-info-and-description"></a>API 情報と説明

`AddSwaggerGen` メソッドに渡された構成アクションを使用して、作成者、ライセンス、説明などの情報を追加することができます。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

次の図は、バージョン情報を表示する Swagger UI を示しています。

![バージョン情報を含む Swagger UI: 説明、作成者、およびその他のリンクの表示](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a>XML コメント

XML コメントは、次の方法で有効にすることができます。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[プロパティ]** を選択します。
* **[ビルド]** タブの **[出力]** セクションの下にある **[XML ドキュメント ファイル]** チェック ボックスをオンにします。

![プロジェクトのプロパティの [ビルド] タブ](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[プロジェクト オプション]** ダイアログ > **[ビルド]** > **[コンパイラ]** を開きます。
* **[全般オプション]** セクションの下にある **[XML ドキュメントを生成する]** チェック ボックスをオンにします。

![プロジェクトのオプションの [全般オプション] セクション](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

次のスニペットを *.csproj* ファイルに手動で追加します。

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

「Visual Studio Code」を参照してください。

---

XML コメントを有効にすると、ドキュメントに未記載のパブリック型とメンバーのデバッグ情報を提供することができます。 ドキュメントに未記載の型およびメンバーは、"*公開されている型またはメンバーの XML コメントがありません*" という警告メッセージで示されます。

生成された XML ファイルを使用するように Swagger を構成します。 Linux または Windows 以外のオペレーティング システムでは、ファイル名やパスで大文字小文字が区別されます。 たとえば、*ToDoApi.XML* ファイルは Windows では見つかりますが、CentOS では見つかりません。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

上記のコードで、`ApplicationBasePath` はアプリの基本パスを取得します。 基本パスは XML のコメント ファイルの検索に使用します。 生成された XML の名前はアプリケーション名に基づいているので、*TodoApi.xml* は、この例でのみ機能します。

メソッドにトリプル スラッシュのコメントを追加すると、セクション ヘッダーの説明が追加されて Swagger UI が強化されます。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![DELETE メソッドの XML コメント 'Deletes a specific TodoItem.' を 表示している Swagger UI](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

UI は、これらのコメントが含まれる生成された JSON ファイルにより強化されます。

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

[<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) タグを `Create` アクション メソッドのドキュメントに追加します。 これは、`<summary>` タグで指定された情報を補足し、より堅牢な Swagger UI を提供します。 `<remarks>` タグの内容は、テキスト、JSON、または XML で構成されます。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

これらの追加のコメントによる UI の強化に注意してください。

![追加のコメントが表示された Swagger UI](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a>データの注釈

`System.ComponentModel.DataAnnotations` にみられるモデルを属性で装飾し、Swagger UI コンポーネントを強化します。

`[Required]` 属性を `TodoItem` クラスの `Name` プロパティに追加します。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

この属性の有無は、UI 動作を変更し、基になる JSON スキーマを変更します。

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

API コントローラーに `[Produces("application/json")]` 属性を追加します。 その目的は、コント ローラーのアクションが *application/json* のコンテンツ タイプの戻りをサポートすることを宣言することです。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

**[応答コンテンツ タイプ]** ドロップダウンがコント ローラーの GET 操作の既定値としてこのコンテンツ タイプを選択します。

![既定の応答のコンテンツ タイプを持つ Swagger UI](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

Web API のデータの注釈の使用量が多いほど、UI と API のヘルプ ページはわかりやすく便利になります。

### <a name="describing-response-types"></a>応答の種類の説明

開発者は、何が返されるか &mdash; 具体的には、応答の種類とエラー コード (標準以外の場合) を最も考慮します。 これらは、XML のコメントおよびデータ注釈で処理されます。

`Create` アクションは、成功した場合は `201 Created` を返し、ポスト要求の本文が null の場合は `400 Bad Request` を返します。 Swagger UI に適切なドキュメントがないと、利用者にはこれらの予期される結果の知識がありません。 その問題を解決するために、次の例では強調表示された行を追加します。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

Swagger UI は、予期される HTTP 応答コードを明確に記述するようになりました。

![ステータスコードの POST 応答クラスの説明 'Returns the newly created Todo item' and '400 - If the item is null' および応答メッセージの下に理由を示す Swagger UI](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a>UI のカスタマイズ

stock UI は、機能し、表示可能ですが、API のドキュメント ページを作成するときには、ブランドまたはテーマを表する必要があります。 Swashbuckle コンポーネントでそのタスクを実行するには、静的ファイルを提供するためのリソースを追加して、それらのファイルをホストするフォルダー構造を構築する必要があります。

.NET Framework を対象にする場合は、`Microsoft.AspNetCore.StaticFiles` NuGet パッケージをプロジェクトに追加します。

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

静的ファイル ミドルウェアを有効にします。

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

[Swagger UI GitHub リポジトリ](https://github.com/swagger-api/swagger-ui/tree/2.x/dist)から *dist* フォルダーの内容を取得します。 このフォルダーには、Swagger UI ページに必要なアセットが含まれています。

*wwwroot/swagger/ui* フォルダーを作成し、それに *dist* フォルダーのコンテンツをコピーします。

次の CSS を使用して *wwwroot/swagger/ui/css/custom.css* ファイルを作成し、ページ ヘッダーをカスタマイズします。

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

*index.html* ファイルの *custom.css* を参照します。

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

`http://localhost:<random_port>/swagger/ui/index.html` で *index.html* ページを参照します。 ヘッダーのテキスト ボックスに「`http://localhost:<random_port>/swagger/v1/swagger.json`」を入力し、**[探索]** ボタンをクリックします。 結果のページは次のようになります。

![カスタム ヘッダーのタイトルを含む Swagger UI](web-api-help-pages-using-swagger/_static/custom-header.png)

このページでできることはたくさんあります。 [Swagger UI GitHub リポジトリ](https://github.com/swagger-api/swagger-ui)で UI リソースの完全な機能を参照してください。
