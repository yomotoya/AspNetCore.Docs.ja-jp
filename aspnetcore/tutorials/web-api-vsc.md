---
title: "ASP.NET Core と VS Code で Web API を作成する"
author: rick-anderson
description: "macOS、Linux、Windows で ASP.NET Core MVC と Visual Studio Code を利用して Web API を構築する"
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 44566c4014400aa2ca3d512eeaa226637b5f0b97
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="408de-103">Linux、macOS、Windows で ASP.NET Core MVC と Visual Studio Code を利用して Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="408de-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="408de-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="408de-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="408de-105">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="408de-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="408de-106">UI が構成されていません。</span><span class="sxs-lookup"><span data-stu-id="408de-106">A UI isn't constructed.</span></span>

<span data-ttu-id="408de-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="408de-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="408de-108">macOS、Linux、Windows: Visual Studio Code で Web API を作成する (本チュートリアル)</span><span class="sxs-lookup"><span data-stu-id="408de-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="408de-109">macOS: [Visual Studio for Mac で Web API を作成する](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="408de-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="408de-110">Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="408de-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="408de-111">開発環境を構える</span><span class="sxs-lookup"><span data-stu-id="408de-111">Set up your development environment</span></span>

<span data-ttu-id="408de-112">次をダウンロードし、インストールします。</span><span class="sxs-lookup"><span data-stu-id="408de-112">Download and install:</span></span>
- <span data-ttu-id="408de-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降。</span><span class="sxs-lookup"><span data-stu-id="408de-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="408de-114">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="408de-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="408de-115">Visual Studio Code [C# 拡張](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="408de-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="408de-116">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="408de-116">Create the project</span></span>

<span data-ttu-id="408de-117">コンソールから、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="408de-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="408de-118">Visual Studio Code (VS Code) で *TodoApi* フォルダーを開き、*Startup.cs* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="408de-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="408de-119">"'TodoApi' に作成とデバッグに必要な資産がありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を</span><span class="sxs-lookup"><span data-stu-id="408de-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="408de-120">選択します。</span><span class="sxs-lookup"><span data-stu-id="408de-120">Add them?"</span></span>
- <span data-ttu-id="408de-121">[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="408de-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' に作成とデバッグに必要な資産がありません。](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="408de-125">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="408de-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="408de-126">ブラウザーで、http://localhost:5000/api/values に移動します。</span><span class="sxs-lookup"><span data-stu-id="408de-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="408de-127">次が表示されます。</span><span class="sxs-lookup"><span data-stu-id="408de-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="408de-128">VS Code の使用に関するヒントが必要であれば、「[Visual Studio Code ヘルプ](#visual-studio-code-help)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="408de-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="408de-129">Entity Framework Core のサポートの追加</span><span class="sxs-lookup"><span data-stu-id="408de-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="408de-130">.NET Core 2.0 で新しいプロジェクトを作成すると、*TodoApi.csproj* ファイルに 'Microsoft.AspNetCore.All' プロバイダーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="408de-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="408de-131">[Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) データベース プロバイダーを個別にインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="408de-131">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="408de-132">このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用することが許可されます。</span><span class="sxs-lookup"><span data-stu-id="408de-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="408de-133">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="408de-133">Add a model class</span></span>

<span data-ttu-id="408de-134">モデルとは、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="408de-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="408de-135">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="408de-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="408de-136">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="408de-136">Add a folder named *Models*.</span></span> <span data-ttu-id="408de-137">モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。</span><span class="sxs-lookup"><span data-stu-id="408de-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="408de-138">次のコードで `TodoItem` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="408de-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="408de-139">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="408de-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="408de-140">データベース コンテキストの作成</span><span class="sxs-lookup"><span data-stu-id="408de-140">Create the database context</span></span>

<span data-ttu-id="408de-141">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="408de-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="408de-142">このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="408de-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="408de-143">*Models* フォルダーに `TodoContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="408de-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="408de-144">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="408de-144">Add a controller</span></span>

<span data-ttu-id="408de-145">*Controllers* フォルダーで、「`TodoController`」という名前のクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="408de-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="408de-146">次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="408de-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="408de-147">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="408de-147">Launch the app</span></span>

<span data-ttu-id="408de-148">VS Code で、F5 を押してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="408de-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="408de-149">http://localhost:5000/api/todo に移動します (先ほど作成した `Todo` コントローラー)。</span><span class="sxs-lookup"><span data-stu-id="408de-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="408de-150">Visual Studio Code ヘルプ</span><span class="sxs-lookup"><span data-stu-id="408de-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="408de-151">はじめに</span><span class="sxs-lookup"><span data-stu-id="408de-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="408de-152">デバッグ</span><span class="sxs-lookup"><span data-stu-id="408de-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="408de-153">統合ターミナル</span><span class="sxs-lookup"><span data-stu-id="408de-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="408de-154">ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="408de-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="408de-155">Mac ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="408de-155">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="408de-156">Linux ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="408de-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="408de-157">Windows ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="408de-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


