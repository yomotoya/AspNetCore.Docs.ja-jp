---
title: macOS、Linux、Windows の ASP.NET Core MVC の概要
author: rick-anderson
description: macOS、Linux、Windows の ASP.NET Core MVC と Visual Studio Code の使用を開始する方法について説明します。
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2018
ms.locfileid: "36275276"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="a34d2-103">macOS、Linux、Windows の ASP.NET Core MVC の概要</span><span class="sxs-lookup"><span data-stu-id="a34d2-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="a34d2-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a34d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a34d2-105">このチュートリアルでは、[Visual Studio Code](https://code.visualstudio.com) (VS Code) を使用した、ASP.NET Core MVC Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="a34d2-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="a34d2-106">このチュートリアルは VS Code の知識があることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="a34d2-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="a34d2-107">詳細については、[VS Code の概要](https://code.visualstudio.com/docs)、および [Visual Studio Code のヘルプ](#visual-studio-code-help)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a34d2-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="a34d2-108">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="a34d2-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a34d2-109">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a34d2-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="a34d2-110">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a34d2-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="a34d2-111">macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a34d2-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a34d2-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a34d2-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="a34d2-113">dotnet での Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="a34d2-113">Create a web app with dotnet</span></span>

<span data-ttu-id="a34d2-114">端末から、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a34d2-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="a34d2-115">Visual Studio Code (VS Code) で *MvcMovie* フォルダーを開き、*Startup.cs* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="a34d2-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="a34d2-116">"'MvcMovie' に作成とデバッグに必要な資産がありません。追加しますか?" という内容の**警告**メッセージが表示されたら、**[はい]** を</span><span class="sxs-lookup"><span data-stu-id="a34d2-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="a34d2-117">選択します。</span><span class="sxs-lookup"><span data-stu-id="a34d2-117">Add them?"</span></span>
- <span data-ttu-id="a34d2-118">[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージが表示されたら、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a34d2-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

!["'MvcMovie' に作成とデバッグに必要な資産がありません。](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="a34d2-122">**[デバッグ]** (F5) を押してプログラムをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="a34d2-122">Press **Debug** (F5) to build and run the program.</span></span>

![実行中のアプリ](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="a34d2-124">VS Code で [Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが起動し、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="a34d2-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="a34d2-125">アドレス バーには、`example.com` などではなく、`localhost:5000` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a34d2-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="a34d2-126">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="a34d2-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="a34d2-127">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="a34d2-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="a34d2-128">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="a34d2-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="a34d2-129">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="a34d2-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="a34d2-131">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="a34d2-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="a34d2-132">Visual Studio Code ヘルプ</span><span class="sxs-lookup"><span data-stu-id="a34d2-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="a34d2-133">はじめに</span><span class="sxs-lookup"><span data-stu-id="a34d2-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="a34d2-134">デバッグ</span><span class="sxs-lookup"><span data-stu-id="a34d2-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="a34d2-135">統合ターミナル</span><span class="sxs-lookup"><span data-stu-id="a34d2-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="a34d2-136">ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="a34d2-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="a34d2-137">macOS ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="a34d2-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="a34d2-138">Linux ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="a34d2-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="a34d2-139">Windows ショートカット キー</span><span class="sxs-lookup"><span data-stu-id="a34d2-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="a34d2-140">次 - コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="a34d2-140">Next - Add a controller</span></span>](adding-controller.md)
