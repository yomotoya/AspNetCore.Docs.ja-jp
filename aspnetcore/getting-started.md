---
title: "ASP.NET Core 2.0 の概要"
author: rick-anderson
description: "ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。"
keywords: "ASP.NET Core,チュートリアル,概要"
ms.author: riande
manager: wpickett
ms.date: 08/30/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: c81e1328fda6d1652ab937bd580be2342924d241
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="f6a41-104">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="f6a41-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="f6a41-105">ここで説明する手順は最新バージョンの ASP.NET Core 用です。</span><span class="sxs-lookup"><span data-stu-id="f6a41-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="f6a41-106">以前のバージョンの概要については、</span><span class="sxs-lookup"><span data-stu-id="f6a41-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="f6a41-107">[このチュートリアルの 1.1 バージョン](xref:getting-started-1.1)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6a41-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="f6a41-108">[.NET Core](https://www.microsoft.com/net/core/) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="f6a41-108">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="f6a41-109">新しい .NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="f6a41-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="f6a41-110">macOS と Linux では、ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="f6a41-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f6a41-111">Windows では、コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="f6a41-111">On Windows, open a command prompt.</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="f6a41-112">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="f6a41-112">Run the app.</span></span>

    <span data-ttu-id="f6a41-113">次のコマンドを使用してアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="f6a41-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="f6a41-114">[http://localhost:5000](http://localhost:5000) に移動します。</span><span class="sxs-lookup"><span data-stu-id="f6a41-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="f6a41-115">*Pages/About.cshtml* を開き、"Hello, world!</span><span class="sxs-lookup"><span data-stu-id="f6a41-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="f6a41-116">サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。</span><span class="sxs-lookup"><span data-stu-id="f6a41-116">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="f6a41-117">[!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="f6a41-117">[!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

7. <span data-ttu-id="f6a41-118">[http://localhost:5000/About](http://localhost:5000/About) を表示し、変更内容を確認します。</span><span class="sxs-lookup"><span data-stu-id="f6a41-118">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="f6a41-119">次のステップ</span><span class="sxs-lookup"><span data-stu-id="f6a41-119">Next steps</span></span>

<span data-ttu-id="f6a41-120">入門のチュートリアルについては、「[ASP.NET Core Tutorials](tutorials/index.md)」(ASP.NET Core のチュートリアル) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6a41-120">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="f6a41-121">ASP.NET Core の概念とアーキテクチャの概要については、「[ASP.NET Core Introduction](index.md)」(ASP.NET Core の概要) と「[ASP.NET Core Fundamentals](fundamentals/index.md)」(ASP.NET Core の基礎) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6a41-121">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="f6a41-122">ASP.NET Core アプリは、.NET Core または .NET Framework 基本クラス ライブラリおよびランタイムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f6a41-122">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="f6a41-123">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f6a41-123">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
