---
title: "ASP.NET Core 2.0 の概要"
author: rick-anderson
description: "ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: b5f1fb0de2776177374b8b4d5ea6041b0fc653a9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="96b9c-103">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="96b9c-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="96b9c-104">ここで説明する手順は最新バージョンの ASP.NET Core 用です。</span><span class="sxs-lookup"><span data-stu-id="96b9c-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="96b9c-105">以前のバージョンの概要については、</span><span class="sxs-lookup"><span data-stu-id="96b9c-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="96b9c-106">[このチュートリアルの 1.1 バージョン](xref:getting-started-1.1)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96b9c-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="96b9c-107">[.NET Core](https://www.microsoft.com/net/core/) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="96b9c-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="96b9c-108">新しい .NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="96b9c-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="96b9c-109">macOS と Linux では、ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="96b9c-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="96b9c-110">Windows では、コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="96b9c-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="96b9c-111">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="96b9c-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="96b9c-112">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="96b9c-112">Run the app.</span></span>

    <span data-ttu-id="96b9c-113">次のコマンドを使用してアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="96b9c-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="96b9c-114">[http://localhost:5000](http://localhost:5000) に移動します。</span><span class="sxs-lookup"><span data-stu-id="96b9c-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="96b9c-115">*Pages/About.cshtml* を開き、"Hello, world!</span><span class="sxs-lookup"><span data-stu-id="96b9c-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="96b9c-116">サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。</span><span class="sxs-lookup"><span data-stu-id="96b9c-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="96b9c-117">[http://localhost:5000/About](http://localhost:5000/About) を表示し、変更内容を確認します。</span><span class="sxs-lookup"><span data-stu-id="96b9c-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="96b9c-118">次の手順</span><span class="sxs-lookup"><span data-stu-id="96b9c-118">Next steps</span></span>

<span data-ttu-id="96b9c-119">入門のチュートリアルについては、「[ASP.NET Core Tutorials](tutorials/index.md)」(ASP.NET Core のチュートリアル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96b9c-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="96b9c-120">ASP.NET Core の概念とアーキテクチャの概要については、「[ASP.NET Core Introduction](index.md)」(ASP.NET Core の概要) と「[ASP.NET Core Fundamentals](fundamentals/index.md)」(ASP.NET Core の基礎) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96b9c-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="96b9c-121">ASP.NET Core アプリは、.NET Core または .NET Framework 基本クラス ライブラリおよびランタイムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="96b9c-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="96b9c-122">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="96b9c-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
