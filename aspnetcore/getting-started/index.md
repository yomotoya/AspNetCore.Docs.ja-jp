---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228583"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="10f41-103">ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="10f41-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="10f41-104">[!INCLUDE [](~/includes/2.1-SDK.md)] をインストールします。</span><span class="sxs-lookup"><span data-stu-id="10f41-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="10f41-105">ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="10f41-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="10f41-106">コマンド シェルを開き、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="10f41-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="10f41-107">HTTPS 開発証明書を信頼します。</span><span class="sxs-lookup"><span data-stu-id="10f41-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="10f41-108">Windows</span><span class="sxs-lookup"><span data-stu-id="10f41-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="10f41-109">上記のコマンドでは、次のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10f41-109">The preceding command displays the following dialog:</span></span>

   ![セキュリティ警告のダイアログ](_static/cert.png)

   <span data-ttu-id="10f41-111">開発証明書を信頼することに同意する場合は、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="10f41-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="10f41-112">macOS</span><span class="sxs-lookup"><span data-stu-id="10f41-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="10f41-113">上記のコマンドでは、次のメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10f41-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="10f41-114">*HTTPS 開発証明書の信頼が要求されました。証明書がまだ信頼されていない場合は、次のコマンドを実行します:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *このコマンドでは、システム キーチェーン上に証明書をインストールするためにパスワードが求められる場合があります。  パスワード:*</span><span class="sxs-lookup"><span data-stu-id="10f41-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *This command might prompt you for your password to install the certificate on the system keychain.    Password:*</span></span>

   <span data-ttu-id="10f41-115">開発証明書を信頼することに同意する場合は、パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="10f41-115">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="10f41-116">Linux</span><span class="sxs-lookup"><span data-stu-id="10f41-116">Linux</span></span>](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a><span data-ttu-id="10f41-117">HTTPS 開発証明書を信頼する方法については、Linux ディストリビューションのドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="10f41-117">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
---

4. <span data-ttu-id="10f41-118">次のようにアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="10f41-118">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="10f41-119">[http://localhost:5001](http://localhost:5001) を参照します。</span><span class="sxs-lookup"><span data-stu-id="10f41-119">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="10f41-120">**[Accept]\(承認\)** をクリックして、プライバシーとクッキーのポリシーを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="10f41-120">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="10f41-121">このアプリで個人情報が保持されることはありません。</span><span class="sxs-lookup"><span data-stu-id="10f41-121">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="10f41-122">*Pages/About.cshtml* を開き、次の強調表示されたマークアップでページを変更します。</span><span class="sxs-lookup"><span data-stu-id="10f41-122">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="10f41-123">[http://localhost:5001/About](http://localhost:5001/About) を参照して、変更が表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="10f41-123">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="10f41-124">[!INCLUDE [](~/includes/net-core-sdk-download-link.md)] をインストールします。</span><span class="sxs-lookup"><span data-stu-id="10f41-124">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="10f41-125">新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="10f41-125">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="10f41-126">コマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="10f41-126">Open a command shell.</span></span> <span data-ttu-id="10f41-127">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="10f41-127">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="10f41-128">次のコマンドでアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="10f41-128">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="10f41-129">[http://localhost:5000](http://localhost:5000) を参照します。</span><span class="sxs-lookup"><span data-stu-id="10f41-129">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="10f41-130">*Pages/About.cshtml* を開き、"Hello, world!</span><span class="sxs-lookup"><span data-stu-id="10f41-130">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="10f41-131">サーバー上の時刻は @DateTime.Now です" というメッセージが表示されるようにページを修正します。</span><span class="sxs-lookup"><span data-stu-id="10f41-131">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="10f41-132">[http://localhost:5000/About](http://localhost:5000/About) を参照して、変更を確認します。</span><span class="sxs-lookup"><span data-stu-id="10f41-132">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="10f41-133">[.NET の「All Downloads」](https://www.microsoft.com/net/download/all)から、SDK 1.0.4 の .NET Core **SDK インストーラー**をインストールします。</span><span class="sxs-lookup"><span data-stu-id="10f41-133">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="10f41-134">新しい ASP.NET Core プロジェクト用のフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="10f41-134">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="10f41-135">コマンド シェルを開きます。</span><span class="sxs-lookup"><span data-stu-id="10f41-135">Open a command shell.</span></span> <span data-ttu-id="10f41-136">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="10f41-136">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="10f41-137">コンピューターに新しい SDK バージョンがインストールされている場合は、1.0.4 SDK を選択する *global.json* ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="10f41-137">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="10f41-138">新しい ASP.NET Core プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="10f41-138">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="10f41-139">パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="10f41-139">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="10f41-140">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="10f41-140">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="10f41-141">必要に応じて、まず [dotnet run](/dotnet/core/tools/dotnet-run) コマンドでアプリを構築します。</span><span class="sxs-lookup"><span data-stu-id="10f41-141">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="10f41-142">`http://localhost:5000` を参照します。</span><span class="sxs-lookup"><span data-stu-id="10f41-142">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
