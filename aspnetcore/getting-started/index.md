---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: eb049dea2800cf2e12c044b88d1664ee80bb95a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296769"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="b0f48-103">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="b0f48-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="b0f48-104">[!INCLUDE[](~/includes/2.1-SDK.md)] をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b0f48-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="b0f48-105">ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="b0f48-106">コマンド シェルを開き、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="b0f48-108">HTTPS 開発証明書を信頼します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b0f48-109">Windows</span><span class="sxs-lookup"><span data-stu-id="b0f48-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="b0f48-110">macOS</span><span class="sxs-lookup"><span data-stu-id="b0f48-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="b0f48-111">Linux</span><span class="sxs-lookup"><span data-stu-id="b0f48-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="b0f48-112">次のようにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="b0f48-113">[http://localhost:5001](http://localhost:5001) を参照します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="b0f48-114">**[Accept]\(承認\)** をクリックして、プライバシーとクッキーのポリシーを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="b0f48-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="b0f48-115">このアプリで個人情報が保持されることはありません。</span><span class="sxs-lookup"><span data-stu-id="b0f48-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="b0f48-116">*Pages/About.cshtml* を開き、次の強調表示されたマークアップでページを変更します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    <span data-ttu-id="b0f48-117">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="b0f48-117">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]</span></span>

7. <span data-ttu-id="b0f48-118">[http://localhost:5001/About](http://localhost:5001/About) を参照して、変更が表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-118">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="b0f48-119">[!INCLUDE[](~/includes/net-core-sdk-download-link.md)] をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b0f48-119">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="b0f48-120">新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-120">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="b0f48-121">コマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="b0f48-121">Open a command shell.</span></span> <span data-ttu-id="b0f48-122">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-122">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="b0f48-123">次のコマンドでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-123">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="b0f48-124">[http://localhost:5000](http://localhost:5000) を参照します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-124">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="b0f48-125">*Pages/About.cshtml* を開き、"Hello, world!</span><span class="sxs-lookup"><span data-stu-id="b0f48-125">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="b0f48-126">サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-126">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="b0f48-127">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="b0f48-127">[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="b0f48-128">[http://localhost:5000/About](http://localhost:5000/About) を参照して、変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-128">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="b0f48-129">[.NET の「All Downloads」](https://www.microsoft.com/net/download/all)から、SDK 1.0.4 の .NET Core **SDK インストーラー**をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b0f48-129">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="b0f48-130">新しい ASP.NET Core プロジェクト用のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-130">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="b0f48-131">コマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="b0f48-131">Open a command shell.</span></span> <span data-ttu-id="b0f48-132">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-132">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="b0f48-133">コンピューターに新しい SDK バージョンがインストールされている場合は、1.0.4 SDK を選択する *global.json* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-133">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="b0f48-134">新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-134">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="b0f48-135">パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-135">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="b0f48-136">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-136">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="b0f48-137">必要に応じて、まず [dotnet run](/dotnet/core/tools/dotnet-run) コマンドでアプリを構築します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-137">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="b0f48-138">`http://localhost:5000` を参照します。</span><span class="sxs-lookup"><span data-stu-id="b0f48-138">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
