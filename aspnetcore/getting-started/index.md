---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077661"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="ec28d-103">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="ec28d-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="ec28d-104">[!INCLUDE [](~/includes/2.1-SDK.md)] をインストールします。</span><span class="sxs-lookup"><span data-stu-id="ec28d-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="ec28d-105">ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="ec28d-106">コマンド シェルを開き、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="ec28d-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="ec28d-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="ec28d-108">HTTPS 開発証明書を信頼します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="ec28d-109">Windows</span><span class="sxs-lookup"><span data-stu-id="ec28d-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="ec28d-110">macOS</span><span class="sxs-lookup"><span data-stu-id="ec28d-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="ec28d-111">Linux</span><span class="sxs-lookup"><span data-stu-id="ec28d-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="ec28d-112">次のようにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="ec28d-113">[http://localhost:5001](http://localhost:5001) を参照します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="ec28d-114">**[Accept]\(承認\)** をクリックして、プライバシーとクッキーのポリシーを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="ec28d-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="ec28d-115">このアプリで個人情報が保持されることはありません。</span><span class="sxs-lookup"><span data-stu-id="ec28d-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="ec28d-116">*Pages/About.cshtml* を開き、次の強調表示されたマークアップでページを変更します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="ec28d-117">[http://localhost:5001/About](http://localhost:5001/About) を参照して、変更が表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="ec28d-118">[!INCLUDE [](~/includes/net-core-sdk-download-link.md)] をインストールします。</span><span class="sxs-lookup"><span data-stu-id="ec28d-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="ec28d-119">新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="ec28d-120">コマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="ec28d-120">Open a command shell.</span></span> <span data-ttu-id="ec28d-121">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="ec28d-122">次のコマンドでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="ec28d-123">[http://localhost:5000](http://localhost:5000) を参照します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="ec28d-124">*Pages/About.cshtml* を開き、"Hello, world!</span><span class="sxs-lookup"><span data-stu-id="ec28d-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="ec28d-125">サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="ec28d-126">[http://localhost:5000/About](http://localhost:5000/About) を参照して、変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="ec28d-127">[.NET の「All Downloads」](https://www.microsoft.com/net/download/all)から、SDK 1.0.4 の .NET Core **SDK インストーラー**をインストールします。</span><span class="sxs-lookup"><span data-stu-id="ec28d-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="ec28d-128">新しい ASP.NET Core プロジェクト用のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="ec28d-129">コマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="ec28d-129">Open a command shell.</span></span> <span data-ttu-id="ec28d-130">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="ec28d-131">コンピューターに新しい SDK バージョンがインストールされている場合は、1.0.4 SDK を選択する *global.json* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="ec28d-132">新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="ec28d-133">パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="ec28d-134">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="ec28d-135">必要に応じて、まず [dotnet run](/dotnet/core/tools/dotnet-run) コマンドでアプリを構築します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="ec28d-136">`http://localhost:5000` を参照します。</span><span class="sxs-lookup"><span data-stu-id="ec28d-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
