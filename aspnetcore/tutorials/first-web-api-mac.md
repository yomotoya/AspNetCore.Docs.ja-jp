---
title: ASP.NET Core と Visual Studio for Mac で Web API を作成する
author: rick-anderson
description: ASP.NET Core MVC と Visual Studio for Mac で Web API を作成する
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 46050f4bbd6ae821c03d92c8750e839d491328cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>ASP.NET Core と Visual Studio for Mac で Web API を作成する

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。 UI が構成されていません。

このチュートリアルには 3 つのバージョンがあります。

* macOS: Visual Studio for Mac で Web API を作成する (このチュートリアル)
* Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)
* macOS、Linux、Windows: [Visual Studio Code で Web API を作成する](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

永続的なデータベースを使用する例については、「[Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index)」 (macOS または Linux での ASP.NET Core MVC の概要) を参照してください。

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>プロジェクトの作成

Visual Studio で **[ファイル]**、**[新しいソリューション]** の順に選択します。

![macOS の新しいソリューション](first-web-api-mac/_static/sln.png)

**[.NET Core アプリ]**、**[ASP.NET Core Web API]**、**[次へ]** の順に選択します。

![macOS の [新しいプロジェクト] ダイアログ](first-web-api-mac/_static/1.png)

**[プロジェクト名]** に「*TodoApi*」と入力し、**[作成]** をクリックします。

![構成ダイアログ](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>アプリの起動

Visual Studio で、**[実行]**、**[デバッグありで開始]** の順に選択してアプリを起動します。 Visual Studio によってブラウザーが起動され、`http://localhost:5000` に移動されます。 HTTP 404 (Not Found) エラーが表示されます。 URL を `http://localhost:<port>/api/values` に変更します。 `ValuesController` のデータが表示されます。

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Entity Framework Core のサポートの追加

[Entity Framework Core InMemory](/ef/core/providers/in-memory/) データベース プロバイダーをインストールします。 このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用すことが許可されます。

* **[プロジェクト]** メニューの **[Add NuGet Packages]\(NuGet パッケージの追加\)** を選択します。

  * または、**[依存関係]** を右クリックし、**[パッケージを追加]** を選択します。

* 検索ボックスに、`EntityFrameworkCore.InMemory` と入力します。
* `Microsoft.EntityFrameworkCore.InMemory` を選択し、**[Add Package]\(パッケージの追加\)** を選択します。

### <a name="add-a-model-class"></a>モデル クラスの追加

モデルは、アプリのデータを表すオブジェクトです。 ここでは、唯一のモデルが to-do 項目です。

ソリューション エクスプローラーで、プロジェクトを右クリックします。 **[追加]** > **[新しいフォルダー]** の順に選択します。 フォルダーに *Models* という名前を付けます。

![新しいフォルダー](first-web-api-mac/_static/folder.png)

> [!NOTE]
> モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。

*Models* フォルダーを右クリックし、**[追加]**、**[新しいファイル]**、**[全般]**、**[空のクラス]** の順に選択します。 クラスに「*TodoItem*」という名前を付け、**[新規]** をクリックします。

生成されたコードを次と置き換えます。

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

`TodoItem` が作成されるとき、データベースは `Id` を生成します。

### <a name="create-the-database-context"></a>データベース コンテキストの作成

*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。 このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。

*Models* フォルダーに `TodoContext` クラスを追加します。

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>コントローラーの追加

ソリューション エクスプローラーの *Controllers* フォルダーに、`TodoController` クラスを追加します。

生成されたコードを次のコードに置き換えます。

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>アプリの起動

Visual Studio で、**[実行]**、**[デバッグありで開始]** の順に選択してアプリを起動します。 Visual Studio でブラウザーが起動し、`http://localhost:<port>` にアクセスします。ここで、`<port>` はランダムに選択されたポート番号になります。 HTTP 404 (Not Found) エラーが表示されます。 URL を `http://localhost:<port>/api/values` に変更します。 `ValuesController` のデータが表示されます。

```json
["value1","value2"]
```

`http://localhost:<port>/api/todo` の `Todo` コントローラーに移動します。

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>その他の CRUD 操作の実装

コントローラーに、`Create`、`Update`、および `Delete` メソッドを追加します。 これらのメソッドはテーマのバリエーションなので、単にコードを示し、主な違いを強調します。 プロジェクトは、コードの追加または変更後にビルドします。

### <a name="create"></a>作成

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

前述のメソッドは、[[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 属性で示されている HTTP POST に対応します。 MVC は、[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

前述のメソッドは、[[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 属性で示されている HTTP POST に対応します。 MVC は、HTTP 要求の本文から to-do 項目の値を取得します。
::: moniker-end

`CreatedAtRoute` メソッドにより、201 の応答があります。 これは、サーバーに新しいリソースを作成する HTTP POST メソッドに対する標準の応答です。 `CreatedAtRoute` では、応答に場所ヘッダーも追加されます。 場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。 「[10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。

### <a name="use-postman-to-send-a-create-request"></a>Postman を使用した作成要求の送信

* アプリを起動します (**[実行]**、**[デバッグありで開始]**)。
* Postman を開きます。

![Postman コンソール](first-web-api/_static/pmc.png)

* localhost URL のポート番号を更新します。
* HTTP メソッドを *POST* に設定します。
* **[Body]** タブをクリックします。
* **[raw]** ラジオ ボタンを選択します。
* 型を *[JSON (application/json)]* に設定します。
* 次の JSON のような、to-do 項目を含む要求本文を入力します。

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* **[Send]** ボタンをクリックします。

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> **[Send]** をクリックしても応答が表示されない場合、**[SSL certification verification]** オプションを無効にします。 これは、**[File]**、**[Settings]** の下にあります。 設定を無効にした後にもう一度 **[Send]** ボタンをクリックします。
::: moniker-end

次のように、**[Response]** ウィンドウの **[Headers]** タブをクリックして、**[Location]** ヘッダー値をコピーします。

![Postman コンソールの [Headers] タブ](first-web-api/_static/pmc2.png)

[Location] ヘッダーの URI を使用して、作成したリソースにアクセスできます。 `Create` メソッドにより [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_) が返されます。 `CreatedAtRoute` に渡された最初のパラメーターは、URL の生成に使用される名前のルートを表します。 `GetById` メソッドによって、`"GetTodo"` という名前のルートが作成されたことを思い出してください。

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>更新

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` は `Create` と似ていますが、HTTP PUT を使用します。 応答は [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。 HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。 部分的な更新をサポートするには、HTTP PATCH を使用します。

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmcput.png)

### <a name="delete"></a>削除

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

応答は [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
