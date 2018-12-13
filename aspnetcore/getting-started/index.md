---
title: ASP.NET Core の概要
author: rick-anderson
description: ASP.NET Core を使用して単純な Hello World アプリを作成し、実行する簡単なチュートリアルです。
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: getting-started
ms.openlocfilehash: cf9e731f7638687b3f40b42864ef7ee8f5522b39
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284356"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="f97d8-103">チュートリアル: ASP.NET Core の概要</span><span class="sxs-lookup"><span data-stu-id="f97d8-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="f97d8-104">このチュートリアルでは、.NET Core コマンド ライン インターフェイスを使用して ASP.NET Core Web アプリを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="f97d8-105">以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f97d8-106">Web アプリ プロジェクトを作成する。</span><span class="sxs-lookup"><span data-stu-id="f97d8-106">Create a web app project.</span></span>
> * <span data-ttu-id="f97d8-107">ローカル HTTPS を有効にする。</span><span class="sxs-lookup"><span data-stu-id="f97d8-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="f97d8-108">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-108">Run the app.</span></span>
> * <span data-ttu-id="f97d8-109">Razor ページを編集する。</span><span class="sxs-lookup"><span data-stu-id="f97d8-109">Edit a Razor page.</span></span>

<span data-ttu-id="f97d8-110">最後に、作業用の Web アプリがご利用のローカル コンピューター上で実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="f97d8-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web アプリのホーム ページ](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="f97d8-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f97d8-112">Prerequisites</span></span>

* [<span data-ttu-id="f97d8-113">.NET Core 2.2 SDK</span><span class="sxs-lookup"><span data-stu-id="f97d8-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="f97d8-114">Web アプリ プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="f97d8-114">Create a web app project</span></span>

<span data-ttu-id="f97d8-115">コマンド シェルを開き、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="f97d8-116">ローカル HTTPS を有効にする</span><span class="sxs-lookup"><span data-stu-id="f97d8-116">Enable local HTTPS</span></span>

<span data-ttu-id="f97d8-117">HTTPS 開発証明書を信頼します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f97d8-118">Windows</span><span class="sxs-lookup"><span data-stu-id="f97d8-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="f97d8-119">上記のコマンドでは、次のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f97d8-119">The preceding command displays the following dialog:</span></span>

![セキュリティ警告のダイアログ](_static/cert.png)

<span data-ttu-id="f97d8-121">開発証明書を信頼することに同意する場合は、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="f97d8-122">macOS</span><span class="sxs-lookup"><span data-stu-id="f97d8-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="f97d8-123">上記のコマンドでは、次のメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f97d8-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="f97d8-124">*HTTPS 開発証明書の信頼が要求されました。証明書がまだ信頼されていない場合は、次のコマンドを実行します。* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`。</span><span class="sxs-lookup"><span data-stu-id="f97d8-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
<span data-ttu-id="f97d8-125">\* このコマンドでは、システム キーチェーン上に証明書をインストールするためにご自分のパスワードを入力するよう求められる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f97d8-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>

<span data-ttu-id="f97d8-126">パスワード:\*</span><span class="sxs-lookup"><span data-stu-id="f97d8-126">Password:\*</span></span>

<span data-ttu-id="f97d8-127">開発証明書を信頼することに同意する場合は、パスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="f97d8-128">Linux</span><span class="sxs-lookup"><span data-stu-id="f97d8-128">Linux</span></span>](#tab/linux)

<span data-ttu-id="f97d8-129">HTTPS 開発証明書を信頼する方法については、Linux ディストリビューションのドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f97d8-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

## <a name="run-the-app"></a><span data-ttu-id="f97d8-130">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="f97d8-130">Run the app</span></span>

<span data-ttu-id="f97d8-131">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="f97d8-132">[https://localhost:5001](https://localhost:5001) を参照します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="f97d8-133">**[Accept]\(承認\)** をクリックして、プライバシーとクッキーのポリシーを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="f97d8-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="f97d8-134">このアプリで個人情報が保持されることはありません。</span><span class="sxs-lookup"><span data-stu-id="f97d8-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="f97d8-135">Razor ページを編集する</span><span class="sxs-lookup"><span data-stu-id="f97d8-135">Edit a Razor page</span></span>

<span data-ttu-id="f97d8-136">*Pages/Index.cshtml* を開き、次の強調表示されたマークアップでページを変更します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="f97d8-137">[https://localhost:5001](https://localhost:5001) を参照して、変更が表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f97d8-138">次の手順</span><span class="sxs-lookup"><span data-stu-id="f97d8-138">Next steps</span></span>

<span data-ttu-id="f97d8-139">このチュートリアルでは、次の作業を行う方法を学びました。</span><span class="sxs-lookup"><span data-stu-id="f97d8-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f97d8-140">Web アプリ プロジェクトを作成する。</span><span class="sxs-lookup"><span data-stu-id="f97d8-140">Create a web app project.</span></span>
> * <span data-ttu-id="f97d8-141">ローカル HTTPS を有効にする。</span><span class="sxs-lookup"><span data-stu-id="f97d8-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="f97d8-142">プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="f97d8-142">Run the project.</span></span>
> * <span data-ttu-id="f97d8-143">変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="f97d8-143">Make a change.</span></span>

<span data-ttu-id="f97d8-144">ASP.NET Core の詳細については、次の概要を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f97d8-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>

> [!NOTE]
> <span data-ttu-id="f97d8-145">ASP.NET Core の目次の提案された新しい構造の有用性をテストしています。</span><span class="sxs-lookup"><span data-stu-id="f97d8-145">We're testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span> <span data-ttu-id="f97d8-146">現在または提案された目次で 7 つのトピックを探す演習をする時間がある場合は、[ここをクリックして、調査に参加してください](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82)。</span><span class="sxs-lookup"><span data-stu-id="f97d8-146">If you have a few minutes to try an exercise of finding seven different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>
