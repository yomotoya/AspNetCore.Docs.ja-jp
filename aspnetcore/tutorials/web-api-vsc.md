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
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="2afa7-103">ASP.NET Core と Visual Studio Code で Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="2afa7-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="2afa7-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="2afa7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="2afa7-105">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="2afa7-106">UI が構成されていません。</span><span class="sxs-lookup"><span data-stu-id="2afa7-106">A UI isn't constructed.</span></span>

<span data-ttu-id="2afa7-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="2afa7-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="2afa7-108">macOS、Linux、Windows: Visual Studio Code で Web API を作成する (本チュートリアル)</span><span class="sxs-lookup"><span data-stu-id="2afa7-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="2afa7-109">macOS: [Visual Studio for Mac で Web API を作成する](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="2afa7-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="2afa7-110">Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="2afa7-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="2afa7-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="2afa7-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

<span data-ttu-id="2afa7-112">VS Code の使用に関するヒントが必要であれば、「[Visual Studio Code ヘルプ](#visual-studio-code-help)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2afa7-112">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="2afa7-113">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="2afa7-113">Create the project</span></span>

<span data-ttu-id="2afa7-114">コンソールから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-114">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="2afa7-115">Visual Studio Code (VS Code) で *TodoApi* フォルダーが開きます。</span><span class="sxs-lookup"><span data-stu-id="2afa7-115">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="2afa7-116">*Startup.cs* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-116">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="2afa7-117">"'TodoApi' に作成とデバッグに必要な資産がありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を</span><span class="sxs-lookup"><span data-stu-id="2afa7-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="2afa7-118">選択します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-118">Add them?"</span></span>
* <span data-ttu-id="2afa7-119">[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' に作成とデバッグに必要な資産がありません。](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="2afa7-123">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-123">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="2afa7-124">ブラウザーで、 http://localhost:5000/api/values に移動します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-124">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="2afa7-125">次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2afa7-125">The following output is displayed:</span></span>

```json
["value1","value2"]
```



## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="2afa7-126">Entity Framework Core のサポートの追加</span><span class="sxs-lookup"><span data-stu-id="2afa7-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2afa7-127">ASP.NET Core 2.1 以降で新しいプロジェクトを作成すると、プロジェクト ファイルに [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)が追加されます。</span><span class="sxs-lookup"><span data-stu-id="2afa7-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) to the project file:</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="2afa7-128">ASP.NET Core 2.0 で新しいプロジェクトを作成すると、プロジェクト ファイルに [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)が追加されます。</span><span class="sxs-lookup"><span data-stu-id="2afa7-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) to the project file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="2afa7-129">[Entity Framework Core InMemory](/ef/core/providers/in-memory/) データベース プロバイダーを個別にインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2afa7-129">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="2afa7-130">このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用することが許可されます。</span><span class="sxs-lookup"><span data-stu-id="2afa7-130">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="2afa7-131">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="2afa7-131">Add a model class</span></span>

<span data-ttu-id="2afa7-132">モデルは、アプリのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="2afa7-132">A model is an object representing the data in your app.</span></span> <span data-ttu-id="2afa7-133">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="2afa7-133">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="2afa7-134">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-134">Add a folder named *Models*.</span></span> <span data-ttu-id="2afa7-135">モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。</span><span class="sxs-lookup"><span data-stu-id="2afa7-135">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="2afa7-136">次のコードで `TodoItem` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-136">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="2afa7-137">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-137">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="2afa7-138">データベース コンテキストの作成</span><span class="sxs-lookup"><span data-stu-id="2afa7-138">Create the database context</span></span>

<span data-ttu-id="2afa7-139">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="2afa7-139">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="2afa7-140">このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-140">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="2afa7-141">*Models* フォルダーに `TodoContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-141">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="2afa7-142">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="2afa7-142">Add a controller</span></span>

<span data-ttu-id="2afa7-143">*Controllers* フォルダーで、「`TodoController`」という名前のクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-143">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="2afa7-144">その内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2afa7-144">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="2afa7-145">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="2afa7-145">Launch the app</span></span>

<span data-ttu-id="2afa7-146">VS Code で、F5 を押してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-146">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="2afa7-147">http://localhost:5000/api/todo (作成した `Todo` コントローラー) に移動します。</span><span class="sxs-lookup"><span data-stu-id="2afa7-147">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="2afa7-148">Visual Studio Code ヘルプ</span><span class="sxs-lookup"><span data-stu-id="2afa7-148">Visual Studio Code help</span></span>

* [<span data-ttu-id="2afa7-149">はじめに</span><span class="sxs-lookup"><span data-stu-id="2afa7-149">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="2afa7-150">デバッグ</span><span class="sxs-lookup"><span data-stu-id="2afa7-150">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="2afa7-151">統合ターミナル</span><span class="sxs-lookup"><span data-stu-id="2afa7-151">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="2afa7-152">ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="2afa7-152">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="2afa7-153">macOS ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="2afa7-153">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="2afa7-154">Linux ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="2afa7-154">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="2afa7-155">Windows ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="2afa7-155">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
