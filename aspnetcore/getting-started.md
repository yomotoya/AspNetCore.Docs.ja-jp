---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="71390-103">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="71390-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="71390-104">[!INCLUDE[](~/includes/net-core-sdk-download-link.md)] をインストールします。</span><span class="sxs-lookup"><span data-stu-id="71390-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="71390-105">新しい .NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="71390-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="71390-106">macOS と Linux では、ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="71390-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="71390-107">Windows では、コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="71390-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="71390-108">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="71390-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="71390-109">次のコマンドでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="71390-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="71390-110">[http://localhost:5000](http://localhost:5000) を参照します。</span><span class="sxs-lookup"><span data-stu-id="71390-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="71390-111">*Pages/About.cshtml* を開き、"Hello, world!</span><span class="sxs-lookup"><span data-stu-id="71390-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="71390-112">サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。</span><span class="sxs-lookup"><span data-stu-id="71390-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="71390-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="71390-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="71390-114">[http://localhost:5000/About](http://localhost:5000/About) を参照して、変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="71390-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="71390-115">[.NET の「All Downloads」](https://www.microsoft.com/net/download/all)から、SDK 1.0.4 の .NET Core **SDK インストーラー**をインストールします。</span><span class="sxs-lookup"><span data-stu-id="71390-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="71390-116">新しい .NET Core プロジェクト用のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="71390-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="71390-117">macOS と Linux では、ターミナル ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="71390-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="71390-118">Windows では、コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="71390-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="71390-119">コンピューターに新しい SDK バージョンがインストールされている場合は、1.0.4 SDK を選択する *global.json* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="71390-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="71390-120">新しい .NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="71390-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="71390-121">パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="71390-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="71390-122">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="71390-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="71390-123">必要に応じて、まず [dotnet run](/dotnet/core/tools/dotnet-run) コマンドでアプリを構築します。</span><span class="sxs-lookup"><span data-stu-id="71390-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="71390-124">`http://localhost:5000` を参照します。</span><span class="sxs-lookup"><span data-stu-id="71390-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end