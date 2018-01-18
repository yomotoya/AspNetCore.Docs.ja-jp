---
title: "ASP.NET Core と VS Code で Web API を作成する"
description: "macOS、Linux、Windows で ASP.NET Core MVC と Visual Studio Code を利用して Web API を構築する"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, サービス, HTTP サービス, VS Code"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: 40f9259101e5d006378562a27e97948641e29450
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="00c51-104">Linux、macOS、Windows で ASP.NET Core MVC と Visual Studio Code を利用して Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="00c51-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="00c51-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="00c51-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="00c51-106">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="00c51-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="00c51-107">UI は構築しません。</span><span class="sxs-lookup"><span data-stu-id="00c51-107">You won’t build a UI.</span></span>

<span data-ttu-id="00c51-108">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="00c51-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="00c51-109">macOS、Linux、Windows: Visual Studio Code で Web API を作成する (本チュートリアル)</span><span class="sxs-lookup"><span data-stu-id="00c51-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="00c51-110">macOS: [Visual Studio for Mac で Web API を作成する](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="00c51-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="00c51-111">Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="00c51-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="00c51-112">開発環境を構える</span><span class="sxs-lookup"><span data-stu-id="00c51-112">Set up your development environment</span></span>

<span data-ttu-id="00c51-113">次をダウンロードし、インストールします。</span><span class="sxs-lookup"><span data-stu-id="00c51-113">Download and install:</span></span>
- <span data-ttu-id="00c51-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降。</span><span class="sxs-lookup"><span data-stu-id="00c51-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="00c51-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="00c51-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="00c51-116">Visual Studio Code [C# 拡張](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="00c51-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="00c51-117">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="00c51-117">Create the project</span></span>

<span data-ttu-id="00c51-118">コンソールから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="00c51-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="00c51-119">Visual Studio Code (VS Code) で *TodoApi* フォルダーを開き、*Startup.cs* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="00c51-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="00c51-120">"'TodoApi' に作成とデバッグに必要な資産がありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を</span><span class="sxs-lookup"><span data-stu-id="00c51-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="00c51-121">選択します。</span><span class="sxs-lookup"><span data-stu-id="00c51-121">Add them?"</span></span>
- <span data-ttu-id="00c51-122">[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="00c51-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' に作成とデバッグに必要な資産がありません。](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="00c51-126">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="00c51-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="00c51-127">ブラウザーで、http://localhost:5000/api/values に移動します。</span><span class="sxs-lookup"><span data-stu-id="00c51-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="00c51-128">次が表示されます。</span><span class="sxs-lookup"><span data-stu-id="00c51-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="00c51-129">VS Code の使用に関するヒントが必要であれば、「[Visual Studio Code ヘルプ](#visual-studio-code-help)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="00c51-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="00c51-130">Entity Framework Core のサポートの追加</span><span class="sxs-lookup"><span data-stu-id="00c51-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="00c51-131">.NET Core 2.0 で新しいプロジェクトを作成すると、*TodoApi.csproj* ファイルに 'Microsoft.AspNetCore.All' プロバイダーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="00c51-131">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="00c51-132">[Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) データベース プロバイダーを個別にインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="00c51-132">There is no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="00c51-133">このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用することが許可されます。</span><span class="sxs-lookup"><span data-stu-id="00c51-133">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="00c51-134">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="00c51-134">Add a model class</span></span>

<span data-ttu-id="00c51-135">モデルとは、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="00c51-135">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="00c51-136">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="00c51-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="00c51-137">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="00c51-137">Add a folder named *Models*.</span></span> <span data-ttu-id="00c51-138">モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。</span><span class="sxs-lookup"><span data-stu-id="00c51-138">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="00c51-139">次のコードで `TodoItem` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="00c51-139">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="00c51-140">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="00c51-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="00c51-141">データベース コンテキストの作成</span><span class="sxs-lookup"><span data-stu-id="00c51-141">Create the database context</span></span>

<span data-ttu-id="00c51-142">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="00c51-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="00c51-143">このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="00c51-143">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="00c51-144">*Models* フォルダーに `TodoContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="00c51-144">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="00c51-145">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="00c51-145">Add a controller</span></span>

<span data-ttu-id="00c51-146">*Controllers* フォルダーで、「`TodoController`」という名前のクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="00c51-146">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="00c51-147">次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="00c51-147">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="00c51-148">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="00c51-148">Launch the app</span></span>

<span data-ttu-id="00c51-149">VS Code で、F5 を押してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="00c51-149">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="00c51-150">http://localhost:5000/api/todo に移動します (先ほど作成した `Todo` コントローラー)。</span><span class="sxs-lookup"><span data-stu-id="00c51-150">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="00c51-151">Visual Studio Code ヘルプ</span><span class="sxs-lookup"><span data-stu-id="00c51-151">Visual Studio Code help</span></span>

- [<span data-ttu-id="00c51-152">はじめに</span><span class="sxs-lookup"><span data-stu-id="00c51-152">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="00c51-153">デバッグ</span><span class="sxs-lookup"><span data-stu-id="00c51-153">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="00c51-154">統合ターミナル</span><span class="sxs-lookup"><span data-stu-id="00c51-154">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="00c51-155">ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="00c51-155">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="00c51-156">Mac ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="00c51-156">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="00c51-157">Linux ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="00c51-157">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="00c51-158">Windows ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="00c51-158">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


