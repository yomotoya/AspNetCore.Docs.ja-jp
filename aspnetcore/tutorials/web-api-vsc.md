---
title: ASP.NET Core と Visual Studio Code で Web API を作成する
author: rick-anderson
description: macOS、Linux、Windows で ASP.NET Core MVC と Visual Studio Code を利用して Web API を構築する
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 740110908358a382f20bc1e54e98056296278acf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089665"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>ASP.NET Core と Visual Studio Code で Web API を作成する

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)

このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。 UI が構成されていません。

このチュートリアルには 3 つのバージョンがあります。

* macOS、Linux、Windows: Visual Studio Code で Web API を作成する (本チュートリアル)
* macOS: [Visual Studio for Mac で Web API を作成する](xref:tutorials/first-web-api-mac)
* Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

VS Code の使用に関するヒントが必要であれば、「[Visual Studio Code ヘルプ](#visual-studio-code-help)」を参照してください。

## <a name="create-the-project"></a>プロジェクトの作成

コンソールから、次のコマンドを実行します。

```console
dotnet new webapi -o TodoApi
code TodoApi
```

Visual Studio Code (VS Code) で *TodoApi* フォルダーが開きます。 *Startup.cs* ファイルを選択します。

* "'TodoApi' に作成とデバッグに必要な資産がありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を 選択します。
* [There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' に作成とデバッグに必要な資産がありません。 追加しますか?" という警告メッセージが表示された VS Code 今後このメッセージを表示しない, 後で, はい](web-api-vsc/_static/vsc_restore.png)

**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。 ブラウザーで、 http://localhost:5000/api/values に移動します。 次のような出力が表示されます。

```json
["value1","value2"]
```



## <a name="add-support-for-entity-framework-core"></a>Entity Framework Core のサポートの追加

:::moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 以降で新しいプロジェクトを作成すると、プロジェクト ファイルに [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)が追加されます。

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

ASP.NET Core 2.0 で新しいプロジェクトを作成すると、プロジェクト ファイルに [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)が追加されます。

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

[Entity Framework Core InMemory](/ef/core/providers/in-memory/) データベース プロバイダーを個別にインストールする必要はありません。 このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用することが許可されます。

## <a name="add-a-model-class"></a>モデル クラスの追加

モデルは、アプリのデータを表すオブジェクトです。 ここでは、唯一のモデルが to-do 項目です。

*Models* という名前のフォルダーを追加します。 モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。

次のコードで `TodoItem` クラスを追加します。

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

`TodoItem` が作成されるとき、データベースは `Id` を生成します。

## <a name="create-the-database-context"></a>データベース コンテキストの作成

*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。 このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。

*Models* フォルダーに `TodoContext` クラスを追加します。

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>コントローラーの追加

*Controllers* フォルダーで、「`TodoController`」という名前のクラスを作成します。 その内容を次のコードに置き換えます。

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>アプリの起動

VS Code で、F5 を押してアプリを起動します。 http://localhost:5000/api/todo (作成した `Todo` コントローラー) に移動します。

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Visual Studio Code ヘルプ

* [はじめに](https://code.visualstudio.com/docs)
* [デバッグ](https://code.visualstudio.com/docs/editor/debugging)
* [統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [ショートカット キー](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [macOS ショートカット キー](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Linux ショートカット キー](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Windows ショートカット キー](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
