---
title: "Mac、Linux、Windows の ASP.NET Core MVC の概要"
author: rick-anderson
description: "Mac、Linux、Windows の ASP.NET Core MVC と Visual Studio Code の概要"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: dae1b0f7c8700bbd99080752fb4bb37f93441c9a
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="3057e-103">Mac、Linux、Windows の ASP.NET Core MVC の概要</span><span class="sxs-lookup"><span data-stu-id="3057e-103">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="3057e-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3057e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3057e-105">このチュートリアルでは、[Visual Studio Code](https://code.visualstudio.com) (VS Code) を使用した、ASP.NET Core MVC Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="3057e-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="3057e-106">このチュートリアルは VS Code の知識があることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="3057e-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="3057e-107">詳細については、[VS Code の概要](https://code.visualstudio.com/docs)、および [Visual Studio Code のヘルプ](#visual-studio-code-help)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3057e-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="3057e-108">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="3057e-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="3057e-109">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3057e-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="3057e-110">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3057e-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="3057e-111">macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="3057e-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="3057e-112">VS Code と .NET Core のインストール</span><span class="sxs-lookup"><span data-stu-id="3057e-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="3057e-113">このチュートリアルには、[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="3057e-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="3057e-114">ASP.NET Core 1.1 バージョンについては、[この PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3057e-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="3057e-115">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="3057e-115">Install the following:</span></span>

* <span data-ttu-id="3057e-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降。</span><span class="sxs-lookup"><span data-stu-id="3057e-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="3057e-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3057e-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="3057e-118">VS Code [C# 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="3057e-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="3057e-119">dotnet での Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="3057e-119">Create a web app with dotnet</span></span>

<span data-ttu-id="3057e-120">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="3057e-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="3057e-121">Visual Studio Code (VS Code) で *MvcMovie* フォルダーを開き、*Startup.cs* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="3057e-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="3057e-122">"'MvcMovie' に作成とデバッグに必要な資産がありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を</span><span class="sxs-lookup"><span data-stu-id="3057e-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="3057e-123">選択します。</span><span class="sxs-lookup"><span data-stu-id="3057e-123">Add them?"</span></span>
- <span data-ttu-id="3057e-124">[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3057e-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

!["'MvcMovie' に作成とデバッグに必要な資産がありません。](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="3057e-128">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="3057e-128">Press **Debug** (F5) to build and run the program.</span></span>

![実行中のアプリ](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="3057e-130">VS Code で [Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが起動し、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="3057e-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="3057e-131">アドレス バーには、`example.com` などではなく、`localhost:5000` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3057e-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="3057e-132">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="3057e-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="3057e-133">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="3057e-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="3057e-134">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="3057e-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="3057e-135">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="3057e-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="3057e-137">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="3057e-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="3057e-138">Visual Studio Code ヘルプ</span><span class="sxs-lookup"><span data-stu-id="3057e-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="3057e-139">はじめに</span><span class="sxs-lookup"><span data-stu-id="3057e-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="3057e-140">デバッグ</span><span class="sxs-lookup"><span data-stu-id="3057e-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="3057e-141">統合ターミナル</span><span class="sxs-lookup"><span data-stu-id="3057e-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="3057e-142">ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="3057e-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="3057e-143">Mac ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="3057e-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="3057e-144">Linux ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="3057e-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="3057e-145">Windows ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="3057e-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="3057e-146">次 - コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="3057e-146">Next - Add a controller</span></span>](adding-controller.md)
