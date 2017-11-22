---
title: "ASP.NET Core と Visual Studio for Windows で Web API を作成する"
author: rick-anderson
description: "ASP.NET Core MVC と Visual Studio for Windows で Web API を構築する"
keywords: "ASP.NET Core、WebAPI、Web API、REST、HTTP、Service、HTTP サービス"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 3ef6fb26eab123c9f6f8275ee1d979b090db0413
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>ASP.NET Core と Visual Studio for Windows で Web API を作成する

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)

このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。 ユーザー インターフェイス (UI) は作成しません。

このチュートリアルには 3 つのバージョンがあります。

* Windows: Visual Studio for Windows で Web API を作成する (このチュートリアル)
* macOS: [Visual Studio for Mac で Web API を作成する](xref:tutorials/first-web-api-mac)
* macOS、Linux、Windows: [Visual Studio Code で Web API を作成する](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE[install 2.0](../includes/install2.0.md)]

ASP.NET Core 1.1 バージョンについては、[この PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) をご覧ください。

## <a name="create-the-project"></a>プロジェクトの作成

Visual Studio で、**[ファイル]** メニュー、**[新規]** > **[プロジェクト]** の順に選択します。

**[ASP.NET Core Web アプリケーション (.NET Core)]** プロジェクト テンプレートを選択します。 プロジェクトに `TodoApi` という名前を付け、**[OK]** をクリックします。

![[新しいプロジェクト] ダイアログ](first-web-api/_static/new-project.png)

**[New ASP.NET Core Web Application - TodoApi]\(新しい ASP.NET Core Web アプリケーション - TodoApi\)** ダイアログで、**[Web API]** テンプレートを選択します。 **[OK]** を選択します。 **[Enable Docker Support]\(Docker サポートを有効にする\)** は**選択しないで**ください。

![[新しい ASP.NET Core Web アプリケーション] ダイアログ。ASP.NET Core テンプレートから Web API プロジェクト テンプレートが選択されています。](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>アプリの起動

Visual Studio で、CTRL を押しながら F5 を押し、アプリを起動します。 Visual Studio でブラウザーが起動し、`http://localhost:port/api/values` にアクセスします。*ポート*はランダムに選択されたポート番号になります。 Chrome、Microsoft Edge、Firefox では次の出力が表示されます。

```
["value1","value2"]
```

### <a name="add-a-model-class"></a>モデル クラスの追加

モデルは、アプリのデータを表すオブジェクトです。 ここでは、唯一のモデルが to-do 項目です。

"Models" という名前のフォルダーを追加します。 ソリューション エクスプローラーで、プロジェクトを右クリックします。 **[追加]** > **[新しいフォルダー]** の順に選択します。 フォルダーに *Models* という名前を付けます。

注: モデル クラスはプロジェクト内のどこでも使用できます。 通例として、モデル クラス用に *Models* フォルダーを使用しています。

`TodoItem` クラスを追加します。 *Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順にクリックします。 クラスに `TodoItem` という名前を付け、**[追加]** を選択します。

`TodoItem` クラスを次のコードで更新します。

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

`TodoItem` が作成されるとき、データベースは `Id` を生成します。

### <a name="create-the-database-context"></a>データベース コンテキストの作成

*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。 このクラスは `Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。

`TodoContext` クラスを追加します。 *Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順にクリックします。 クラスに `TodoContext` という名前を付け、**[追加]** を選択します。

このクラスを次のコードで置き換えます。

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>コントローラーの追加

ソリューション エクスプローラーで、*Controllers* フォルダーを右クリックします。 **[追加]** > **[新しい項目]** の順に選択します。 **[新しい項目の追加]** ダイアログで、**[Web API コントローラー クラス]** テンプレートを選択します。 クラスに `TodoController` という名前を付けます。

![[新しい項目の追加] ダイアログ。検索ボックスに「controller」と入力されています。Web API コントローラーが選択されています。](first-web-api/_static/new_controller.png)

このクラスを次のコードで置き換えます。

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>アプリの起動

Visual Studio で、CTRL を押しながら F5 を押し、アプリを起動します。 Visual Studio でブラウザーが起動し、`http://localhost:port/api/values` にアクセスします。*ポート*はランダムに選択されたポート番号になります。 `http://localhost:port/api/todo` の `Todo` コントローラーに移動します。

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

