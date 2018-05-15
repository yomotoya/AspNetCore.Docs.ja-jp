---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="b42ab-103">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="b42ab-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="b42ab-104">ここで説明する手順は最新バージョンの ASP.NET Core 用です。</span><span class="sxs-lookup"><span data-stu-id="b42ab-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="b42ab-105">このドキュメントの 1.1 バージョンについては、「[ASP.NET Core 1.1 の概要](xref:getting-started-1.1)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b42ab-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="b42ab-106">[!INCLUDE [](~/includes/net-core-sdk-download-link.md)] をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b42ab-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="b42ab-107">新しい .NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b42ab-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="b42ab-108">macOS と Linux では、ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="b42ab-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="b42ab-109">Windows では、コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="b42ab-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="b42ab-110">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="b42ab-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="b42ab-111">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b42ab-111">Run the app.</span></span>

    <span data-ttu-id="b42ab-112">次のコマンドを使用してアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b42ab-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="b42ab-113">[http://localhost:5000](http://localhost:5000) を参照します。</span><span class="sxs-lookup"><span data-stu-id="b42ab-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="b42ab-114"><em>Pages/About.cshtml</em> を開き、"Hello, world!</span><span class="sxs-lookup"><span data-stu-id="b42ab-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="b42ab-115">サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。</span><span class="sxs-lookup"><span data-stu-id="b42ab-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="b42ab-116">[http://localhost:5000/About](http://localhost:5000/About) を参照して、変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="b42ab-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="b42ab-117">次の手順</span><span class="sxs-lookup"><span data-stu-id="b42ab-117">Next steps</span></span>

<span data-ttu-id="b42ab-118">入門のチュートリアルについては、「[ASP.NET Core Tutorials](tutorials/index.md)」(ASP.NET Core のチュートリアル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b42ab-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="b42ab-119">ASP.NET Core の概念とアーキテクチャの概要については、「[ASP.NET Core Introduction](index.md)」(ASP.NET Core の概要) と「[ASP.NET Core Fundamentals](fundamentals/index.md)」(ASP.NET Core の基礎) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b42ab-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="b42ab-120">ASP.NET Core アプリは、.NET Core または .NET Framework 基本クラス ライブラリおよびランタイムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="b42ab-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="b42ab-121">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b42ab-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
