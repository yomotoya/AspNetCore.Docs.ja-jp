---
title: "ASP.NET Core と Visual Studio for Mac で Web API を作成する"
author: rick-anderson
description: "ASP.NET Core MVC と Visual Studio for Mac で Web API を作成する"
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: b0e1a331fe3229119f4669fa336b6af4822785bf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>ASP.NET Core MVC と Visual Studio for Mac で Web API を作成する

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)

このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。 UI が構成されていません。

このチュートリアルには 3 つのバージョンがあります。

* macOS: Visual Studio for Mac で Web API を作成する (このチュートリアル)
* Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)
* macOS、Linux、Windows: [Visual Studio Code で Web API を作成する](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* 永続的なデータベースを使用する例については、「[Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index)」 (Mac または Linux での ASP.NET Core MVC の概要) を参照してください。

## <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。

- [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降
- [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a>プロジェクトの作成

Visual Studio で **[ファイル]、[新しいソリューション]** の順に選択します。

![macOS の新しいソリューション](first-web-api-mac/_static/sln.png)

**[.NET Core アプリ]、[ASP.NET Core Web API]、[次へ]** の順に選択します。

![macOS の [新しいプロジェクト] ダイアログ](first-web-api-mac/_static/1.png)

**[プロジェクト名]** に「**TodoApi**」と入力し、[作成] を選択します。

![構成ダイアログ](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>アプリの起動

Visual Studio で、**[実行]、[デバッグありで開始]** の順に選択してアプリを起動します。 Visual Studio によってブラウザーが起動され、`http://localhost:5000` に移動されます。 HTTP 404 (Not Found) エラーが表示されます。  URL を `http://localhost:port/api/values` に変更します。 `ValuesController` データが表示されます。

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Entity Framework Core のサポートの追加

[Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) データベース プロバイダーをインストールします。 このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用すことが許可されます。

* **[プロジェクト]** メニューの **[Add NuGet Packages]\(NuGet パッケージの追加\)** を選択します。 

  *  または、**[依存関係]** を右クリックし、**[Add Packages]\(パッケージの追加\)** を選択します。

* 検索ボックスに、`EntityFrameworkCore.InMemory` と入力します。
* `Microsoft.EntityFrameworkCore.InMemory` を選択し、**[Add Package]\(パッケージの追加\)** を選択します。

### <a name="add-a-model-class"></a>モデル クラスの追加

モデルとは、アプリケーションのデータを表すオブジェクトです。 ここでは、唯一のモデルが to-do 項目です。

*Models* という名前のフォルダーを追加します。 ソリューション エクスプローラーで、プロジェクトを右クリックします。 **[追加]** > **[新しいフォルダー]** の順に選択します。 フォルダーに *Models* という名前を付けます。

![新しいフォルダー](first-web-api-mac/_static/folder.png)

注: モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。

`TodoItem` クラスを追加します。 *Models* フォルダーを右クリックし、**[追加]、[新しいファイル]、[全般]、[Empty Class]\(空のクラス)\** の順に選択します。 クラスに `TodoItem` という名前を付け、**[新規]** を選択します。

生成されたコードを次と置き換えます。

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

`TodoItem` が作成されるとき、データベースは `Id` を生成します。

### <a name="create-the-database-context"></a>データベース コンテキストの作成

*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。 このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。

*Models* フォルダーに `TodoContext` クラスを追加します。

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>コントローラーの追加

ソリューション エクスプローラーの *Controllers* フォルダーに、`TodoController` クラスを追加します。

生成されたコードを次に置き換えます (閉じかっこを追加します)。

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>アプリの起動

Visual Studio で、**[実行]、[デバッグありで開始]** の順に選択してアプリを起動します。 Visual Studio でブラウザーが起動し、`http://localhost:port` にアクセスします。*ポート*はランダムに選択されたポート番号になります。 HTTP 404 (Not Found) エラーが表示されます。  URL を `http://localhost:port/api/values` に変更します。 `ValuesController` データが表示されます。

```
["value1","value2"]
```

`http://localhost:port/api/todo` の `Todo` コントローラーに移動します。

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>その他の CRUD 操作の実装

コントローラーに、`Create`、`Update`、および `Delete` メソッドを追加します。 これらはテーマのバリエーションなので、単にコードを示し、主な違いを強調します。 プロジェクトは、コードの追加または変更後にビルドします。

### <a name="create"></a>作成

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

これは、HTTP POST メソッドです。[`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 属性を使用します。 MVC は、[`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。

`CreatedAtRoute` メソッドは、サーバーに新しいリソースを作成する HTTP POST メソッドの標準の応答である 201 の応答を返します。 `CreatedAtRoute` では、応答に場所ヘッダーも追加されます。 場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。 「[10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。

### <a name="use-postman-to-send-a-create-request"></a>Postman を使用した作成要求の送信

* アプリを起動します (**[実行]、[デバッグありで開始]**)。
* Postman を起動します。

![Postman のコンソール](first-web-api/_static/pmc.png)

* HTTP メソッド名を `POST` に設定します。
* **[Body]** ラジオ ボタンを選択します。
* **[raw]** ラジオ ボタンを選択します。
* 型を JSON に設定します。
* キー/値エディターで次のような To do 項目を追加します。

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* **[Send]** を選択します。

* 次のように、下のウィンドウの [Headers] タブを選択して、**[Location]** ヘッダーをコピーします。

![Postman コンソールの [Headers] タブ](first-web-api/_static/pmget.png)

[Location] ヘッダーの URI を使用して、作成したリソースにアクセスできます。 `GetById` メソッドによって、`"GetTodo"` という名前のルートが作成されたことを思い出してください。

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a>更新

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update` は `Create` と似ていますが、HTTP PUT を使用します。 応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。 HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。 部分的な更新をサポートするには、HTTP PATCH を使用します。

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmcput.png)

### <a name="delete"></a>削除

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmd.png)

## <a name="next-steps"></a>次の手順

* [コントローラー アクションへのルーティング](xref:mvc/controllers/routing)
* API の配置については、[ホストおよび配置](xref:host-and-deploy/index)に関するページを参照してください。
* [サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。
* [Postman](https://www.getpostman.com/)
* [Fiddler](https://www.telerik.com/download/fiddler)
