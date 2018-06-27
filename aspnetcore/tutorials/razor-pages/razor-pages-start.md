---
title: ASP.NET Core の Razor ページの概要
author: rick-anderson
description: ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。 Razor ページは、ASP.NET Core の Web ワークロードで推奨されています。
manager: wpickett
ms.author: riande
ms.date: 5/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: d7cdf7c8fac3b2ac1e526c6eeee8205068964ec9
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/01/2018
ms.locfileid: "34582818"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="33121-104">ASP.NET Core の Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="33121-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="33121-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="33121-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="33121-106">このチュートリアルの ASP.NET Core 2.1 バージョンのご利用をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="33121-106">We recommend you follow the ASP.NET Core 2.1 version of this tutorial.</span></span> <span data-ttu-id="33121-107">より簡単に使用でき、**より**多くの機能があります。</span><span class="sxs-lookup"><span data-stu-id="33121-107">It's **much** easier to follow and covers more features.</span></span> <span data-ttu-id="33121-108">バージョン セレクターで、**[ASP.NET Core 2.1]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="33121-108">Select **ASP.NET Core 2.1** in the version selector.</span></span>

![TOC のバージョン セレクター](razor-pages-start/_static/v21.png)

::: moniker-end

<span data-ttu-id="33121-110">このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。</span><span class="sxs-lookup"><span data-stu-id="33121-110">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="33121-111">ASP.NET Core で Web アプリ用の UI を構築する際に Razor ページを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="33121-111">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="33121-112">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="33121-112">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="33121-113">Windows 向け: 本チュートリアル</span><span class="sxs-lookup"><span data-stu-id="33121-113">Windows: This tutorial</span></span>
* <span data-ttu-id="33121-114">MacOS 向け: [Visual Studio for Mac 向けの Razor ページの概要 ](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="33121-114">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="33121-115">macOS、Linux、Windows 向け: [Visual Studio Code を使用する ASP.NET Core の Razor ページの概要](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="33121-115">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="33121-116">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="33121-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a><span data-ttu-id="33121-117">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="33121-117">Prerequisites</span></span>

<span data-ttu-id="33121-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="33121-118">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="33121-119">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="33121-119">Create a Razor web app</span></span>

* <span data-ttu-id="33121-120">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="33121-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="33121-121">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="33121-121">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="33121-122">プロジェクトに **RazorPagesMovie** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="33121-122">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="33121-123">コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="33121-123">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="33121-124">![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="33121-124">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="33121-125">ドロップダウン リストで **[ASP.NET Core 2.1]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="33121-125">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

 ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2_2.1.png)

<span data-ttu-id="33121-127">以下のように、Visual Studio のテンプレートでスタート プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="33121-127">The Visual Studio template creates a starter project:</span></span>

![ソリューション エクスプローラー](razor-pages-start/_static/se2.1.png)

<span data-ttu-id="33121-129">**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。</span><span class="sxs-lookup"><span data-stu-id="33121-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span> <span data-ttu-id="33121-130">**[同意する]** を選択し、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="33121-130">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="33121-131">このアプリでは個人情報は追跡されません。</span><span class="sxs-lookup"><span data-stu-id="33121-131">This app doesn't track personal information.</span></span> <span data-ttu-id="33121-132">テンプレートで生成されたコードには、[一般的なデータ保護規制 (GDPR)](xref:security/gdpr) を満たす資産が含まれます。</span><span class="sxs-lookup"><span data-stu-id="33121-132">The template generated code includes assets to help meet [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR.png)

<span data-ttu-id="33121-134">次の図は、追跡に同意した後のアプリを示しています。</span><span class="sxs-lookup"><span data-stu-id="33121-134">The following image shows the app after accepting tracking:</span></span>

![ホームまたはインデックス ページ](razor-pages-start/_static/home2.1.png)

* <span data-ttu-id="33121-136">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="33121-136">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="33121-137">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="33121-137">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="33121-138">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="33121-138">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="33121-139">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="33121-139">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="33121-140">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="33121-140">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="33121-141">上の図では、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="33121-141">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="33121-142">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="33121-142">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="33121-143">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="33121-143">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="33121-144">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="33121-144">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a><span data-ttu-id="33121-145">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="33121-145">Prerequisites</span></span>

<span data-ttu-id="33121-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span><span class="sxs-lookup"><span data-stu-id="33121-146">[!INCLUDE [](~/includes/net-core-prereqs-windows.md) [](~/includes/net-core-prereqs-windows.md)]</span></span>

## <a name="create-a-razor-web-app"></a><span data-ttu-id="33121-147">Razor Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="33121-147">Create a Razor web app</span></span>

* <span data-ttu-id="33121-148">Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="33121-148">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="33121-149">新しい ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="33121-149">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="33121-150">プロジェクトに **RazorPagesMovie** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="33121-150">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="33121-151">コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。</span><span class="sxs-lookup"><span data-stu-id="33121-151">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="33121-152">![新しい ASP.NET Core Web アプリケーション](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="33121-152">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="33121-153">ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="33121-153">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="33121-154">以下のように、Visual Studio のテンプレートでスタート プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="33121-154">The Visual Studio template creates a starter project:</span></span>

![ソリューション エクスプローラー](razor-pages-start/_static/se.png)

<span data-ttu-id="33121-156">**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。</span><span class="sxs-lookup"><span data-stu-id="33121-156">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![ホームまたはインデックス ページ](razor-pages-start/_static/home.png)

* <span data-ttu-id="33121-158">Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="33121-158">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="33121-159">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="33121-159">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="33121-160">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="33121-160">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="33121-161">localhost では、ローカル コンピューターからの Web 要求のみが処理されます。</span><span class="sxs-lookup"><span data-stu-id="33121-161">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="33121-162">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="33121-162">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="33121-163">上の図では、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="33121-163">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="33121-164">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="33121-164">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="33121-165">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="33121-165">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="33121-166">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="33121-166">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="33121-167">次: モデルの追加</span><span class="sxs-lookup"><span data-stu-id="33121-167">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
