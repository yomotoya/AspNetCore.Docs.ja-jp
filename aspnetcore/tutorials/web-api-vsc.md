---
title: "ASP.NET Core と VS Code で Web API を作成する"
author: rick-anderson
description: "macOS、Linux、Windows で ASP.NET Core MVC と Visual Studio Code を利用して Web API を構築する"
keywords: "ASP.NET Core、WebAPI、Web API、REST、Mac、Linux、HTTP、サービス、HTTP サービス、VS Code"
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-vsc
ms.openlocfilehash: abe088f2c9df94135209ce71540e6b345186ee70
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="ce7e8-104">Linux、macOS、Windows で ASP.NET Core MVC と Visual Studio Code を利用して Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="ce7e8-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="ce7e8-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="ce7e8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="ce7e8-106">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="ce7e8-107">UI は構築しません。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-107">You won’t build a UI.</span></span>

<span data-ttu-id="ce7e8-108">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ce7e8-109">macOS、Linux、Windows: Visual Studio Code で Web API を作成する (本チュートリアル)</span><span class="sxs-lookup"><span data-stu-id="ce7e8-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="ce7e8-110">macOS: [Visual Studio for Mac で Web API を作成する](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="ce7e8-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="ce7e8-111">Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="ce7e8-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="ce7e8-112">開発環境を構える</span><span class="sxs-lookup"><span data-stu-id="ce7e8-112">Set up your development environment</span></span>

<span data-ttu-id="ce7e8-113">次をダウンロードし、インストールします。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-113">Download and install:</span></span>
- [<span data-ttu-id="ce7e8-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce7e8-114">.NET Core</span></span>](https://microsoft.com/net/core)
- [<span data-ttu-id="ce7e8-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ce7e8-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="ce7e8-116">Visual Studio Code [C# 拡張](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="ce7e8-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="ce7e8-117">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="ce7e8-117">Create the project</span></span>

<span data-ttu-id="ce7e8-118">コンソールから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="ce7e8-119">Visual Studio Code (VS Code) で *TodoApi* フォルダーを開き、*Startup.cs* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="ce7e8-120">"'TodoApi' に作成とデバッグに必要な資産がありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を</span><span class="sxs-lookup"><span data-stu-id="ce7e8-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="ce7e8-121">選択します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-121">Add them?"</span></span>
- <span data-ttu-id="ce7e8-122">[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' に作成とデバッグに必要な資産がありません。](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="ce7e8-126">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="ce7e8-127">ブラウザーで、http://localhost:5000/api/values に移動します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="ce7e8-128">次が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="ce7e8-129">VS Code の使用に関するヒントが必要であれば、「[Visual Studio Code ヘルプ](#visual-studio-code-help)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="ce7e8-130">Entity Framework Core のサポートの追加</span><span class="sxs-lookup"><span data-stu-id="ce7e8-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="ce7e8-131">*TodoApi.csproj* ファイルを編集し、[Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) データベース プロバイダーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="ce7e8-132">このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用することが許可されます。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

<span data-ttu-id="ce7e8-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="ce7e8-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span></span>

<span data-ttu-id="ce7e8-134">`dotnet restore` を実行し、EF Core InMemory DB プロバイダーをダウンロードしてインストールします。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-134">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="ce7e8-135">ターミナルから `dotnet restore` を実行するか、VS Code で `⌘⇧P` (macOS) または `Ctrl+Shift+P` (Linux) を入力し、「**.NET**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-135">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="ce7e8-136">**[.NET: パッケージの復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-136">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="ce7e8-137">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="ce7e8-137">Add a model class</span></span>

<span data-ttu-id="ce7e8-138">モデルとは、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="ce7e8-139">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="ce7e8-140">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-140">Add a folder named *Models*.</span></span> <span data-ttu-id="ce7e8-141">モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-141">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="ce7e8-142">次のコードで `TodoItem` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-142">Add a `TodoItem` class with the following code:</span></span>

<span data-ttu-id="ce7e8-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="ce7e8-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="ce7e8-144">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="ce7e8-145">データベース コンテキストを作成する</span><span class="sxs-lookup"><span data-stu-id="ce7e8-145">Create the database context</span></span>

<span data-ttu-id="ce7e8-146">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="ce7e8-147">このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="ce7e8-148">*Models* フォルダーに `TodoContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-148">Add a `TodoContext` class in the *Models* folder:</span></span>

<span data-ttu-id="ce7e8-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="ce7e8-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="ce7e8-150">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="ce7e8-150">Add a controller</span></span>

<span data-ttu-id="ce7e8-151">*Controllers* フォルダーで、「`TodoController`」という名前のクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-151">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="ce7e8-152">次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-152">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="ce7e8-153">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="ce7e8-153">Launch the app</span></span>

<span data-ttu-id="ce7e8-154">VS Code で、F5 を押してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-154">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="ce7e8-155">http://localhost:5000/api/todo に移動します (先ほど作成した `Todo` コントローラー)。</span><span class="sxs-lookup"><span data-stu-id="ce7e8-155">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="ce7e8-156">Visual Studio Code ヘルプ</span><span class="sxs-lookup"><span data-stu-id="ce7e8-156">Visual Studio Code help</span></span>

- [<span data-ttu-id="ce7e8-157">はじめに</span><span class="sxs-lookup"><span data-stu-id="ce7e8-157">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="ce7e8-158">デバッグ</span><span class="sxs-lookup"><span data-stu-id="ce7e8-158">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="ce7e8-159">統合ターミナル</span><span class="sxs-lookup"><span data-stu-id="ce7e8-159">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="ce7e8-160">ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="ce7e8-160">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="ce7e8-161">Mac ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="ce7e8-161">Mac keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832143)
  - [<span data-ttu-id="ce7e8-162">Linux ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="ce7e8-162">Linux keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832144)
  - [<span data-ttu-id="ce7e8-163">Windows ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="ce7e8-163">Windows keyboard shortcuts</span></span>](https://go.microsoft.com/fwlink/?linkid=832145)

[!INCLUDE[next steps](../includes/webApi/next.md)]


