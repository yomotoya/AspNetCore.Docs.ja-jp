---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Visual Studio を使用して ASP.NET Web 展開: Web.config ファイルの変換 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 0a52530396efa3612d19817eeaa0498cffdac38f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842440"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a><span data-ttu-id="b4386-103">Visual Studio を使用して ASP.NET Web 展開: Web.config ファイルの変換</span><span class="sxs-lookup"><span data-stu-id="b4386-103">ASP.NET Web Deployment using Visual Studio: Web.config File Transformations</span></span>
====================
<span data-ttu-id="b4386-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b4386-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="b4386-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b4386-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="b4386-106">このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="b4386-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="b4386-107">系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="b4386-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="b4386-108">概要</span><span class="sxs-lookup"><span data-stu-id="b4386-108">Overview</span></span>

<span data-ttu-id="b4386-109">このチュートリアルでは、変更するプロセスを自動化する方法、 *Web.config*ファイルの別の変換先の環境にデプロイするとき。</span><span class="sxs-lookup"><span data-stu-id="b4386-109">This tutorial shows you how to automate the process of changing the *Web.config* file when you deploy it to different destination environments.</span></span> <span data-ttu-id="b4386-110">ほとんどのアプリケーション設定を持つ、 *Web.config*ファイルをアプリケーションの配置時に異なる必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4386-110">Most applications have settings in the *Web.config* file that must be different when the application is deployed.</span></span> <span data-ttu-id="b4386-111">このような変更のプロセスを自動化すると、展開すると面倒でエラーが発生するたびに手動で行うことがなくなります保持します。</span><span class="sxs-lookup"><span data-stu-id="b4386-111">Automating the process of making these changes keeps you from having to do them manually every time you deploy, which would be tedious and error prone.</span></span>

<span data-ttu-id="b4386-112">リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。</span><span class="sxs-lookup"><span data-stu-id="b4386-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a><span data-ttu-id="b4386-113">Web デプロイのパラメーターと Web.config の変換</span><span class="sxs-lookup"><span data-stu-id="b4386-113">Web.config transformations versus Web Deploy parameters</span></span>

<span data-ttu-id="b4386-114">2 つの方法を変更するプロセスを自動化する*Web.config*ファイルの設定: [Web.config 変換](https://msdn.microsoft.com/library/dd465326.aspx)と[Web 配置パラメーター](https://msdn.microsoft.com/library/ff398068.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="b4386-114">There are two ways to automate the process of changing *Web.config* file settings: [Web.config transformations](https://msdn.microsoft.com/library/dd465326.aspx) and [Web Deploy parameters](https://msdn.microsoft.com/library/ff398068.aspx).</span></span> <span data-ttu-id="b4386-115">A *Web.config*変換ファイルには変更する方法を指定する XML マークアップが含まれています、 *Web.config*ファイルが展開されている場合にします。</span><span class="sxs-lookup"><span data-stu-id="b4386-115">A *Web.config* transformation file contains XML markup that specifies how to change the *Web.config* file when it is deployed.</span></span> <span data-ttu-id="b4386-116">特定のさまざまな変更がビルド構成と、特定の発行プロファイルを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b4386-116">You can specify different changes for specific build configurations and for specific publish profiles.</span></span> <span data-ttu-id="b4386-117">既定のビルド構成はデバッグとリリースでは、およびカスタム ビルド構成を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="b4386-117">The default build configurations are Debug and Release, and you can create custom build configurations.</span></span> <span data-ttu-id="b4386-118">発行プロファイルは、通常、インポート先の環境に対応します。</span><span class="sxs-lookup"><span data-stu-id="b4386-118">A publish profile typically corresponds to a destination environment.</span></span> <span data-ttu-id="b4386-119">(でプロファイルの発行の詳細について説明します、[テスト環境として IIS への配置](deploying-to-iis.md)チュートリアルです)。</span><span class="sxs-lookup"><span data-stu-id="b4386-119">(You'll learn more about publish profiles in the [Deploying to IIS as a Test Environment](deploying-to-iis.md) tutorial.)</span></span>

<span data-ttu-id="b4386-120">Web 配置パラメーターを使用して、さまざまな種類の設定に含まれる設定を含む、展開中に構成する必要がありますを指定することができます*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b4386-120">Web Deploy parameters can be used to specify many different kinds of settings that must be configured during deployment, including settings that are found in *Web.config* files.</span></span> <span data-ttu-id="b4386-121">指定する際に*Web.config*ファイルの変更は、Web 配置パラメーターを設定するより複雑ですを展開するまでに設定する値がわからない場合に便利ですが。</span><span class="sxs-lookup"><span data-stu-id="b4386-121">When used to specify *Web.config* file changes, Web Deploy parameters are more complex to set up, but they are useful when you do not know the value to be set until you deploy.</span></span> <span data-ttu-id="b4386-122">など、エンタープライズ環境で作成、*展開パッケージ*と、運用環境でインストールするには、IT 部門で人にたってから、そのユーザーが、接続文字列やパスワードをしない」と入力します知っています。</span><span class="sxs-lookup"><span data-stu-id="b4386-122">For example, in an enterprise environment, you might create a *deployment package* and give it to a person in the IT department to install in production, and that person has to be able to enter connection strings or passwords that you do not know.</span></span>

<span data-ttu-id="b4386-123">このシナリオは、このチュートリアル シリーズでは、事前にわかってすべてを行う必要がありますが、 *Web.config*ファイル、Web 配置パラメーターを使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b4386-123">For the scenario that this tutorial series covers, you know in advance everything that has to be done to the *Web.config* file, so you do not need to use Web Deploy parameters.</span></span> <span data-ttu-id="b4386-124">使用すると、ビルド構成によって異なりますが、使用された発行プロファイルによって異なるいくつかをいくつかの変換を構成します。</span><span class="sxs-lookup"><span data-stu-id="b4386-124">You'll configure some transformations that differ depending on the build configuration used, and some that differ depending on the publish profile used.</span></span>

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a><span data-ttu-id="b4386-125">Azure で Web.config の設定を指定します。</span><span class="sxs-lookup"><span data-stu-id="b4386-125">Specifying Web.config settings in Azure</span></span>

<span data-ttu-id="b4386-126">場合、 *Web.config*を変更するファイルの設定は、`<connectionStrings>`または`<appSettings>`要素中の変更を自動化するためのもう 1 つのオプションが Azure App Service で Web アプリを展開する場合展開します。</span><span class="sxs-lookup"><span data-stu-id="b4386-126">If the *Web.config* file settings that you want to change are in the `<connectionStrings>` or the `<appSettings>` element, and if you are deploying to Web Apps in Azure App Service, you have another option for automating changes during deployment.</span></span> <span data-ttu-id="b4386-127">Azure で有効にするために必要な設定を入力することができます、**構成**web アプリの管理ポータル ページのタブ (下へスクロールして、**アプリ設定**と**接続文字列**セクション)。</span><span class="sxs-lookup"><span data-stu-id="b4386-127">You can enter the settings that you want to take effect in Azure in the **Configure** tab of the management portal page for your web app (scroll down to the **app settings** and **connection strings** sections).</span></span> <span data-ttu-id="b4386-128">プロジェクトを展開するときに Azure での変更が自動的に適用されます。</span><span class="sxs-lookup"><span data-stu-id="b4386-128">When you deploy the project, Azure automatically applies the changes.</span></span> <span data-ttu-id="b4386-129">詳細については、次を参照してください。 [Windows Azure Web サイト: アプリケーション文字列と接続文字列の動作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="b4386-129">For more information, see [Windows Azure Web Sites: How Application Strings and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).</span></span>

## <a name="default-transformation-files"></a><span data-ttu-id="b4386-130">既定の変換ファイル</span><span class="sxs-lookup"><span data-stu-id="b4386-130">Default transformation files</span></span>

<span data-ttu-id="b4386-131">**ソリューション エクスプ ローラー**、展開*Web.config*を参照してください、 *Web.Debug.config*と*Web.Release.config*ファイルの変換既定では 2 つの既定のビルド構成ごとに作成されます。</span><span class="sxs-lookup"><span data-stu-id="b4386-131">In **Solution Explorer**, expand *Web.config* to see the *Web.Debug.config* and *Web.Release.config* transformation files that are created by default for the two default build configurations.</span></span>

![Web.config_transform_files](web-config-transformations/_static/image1.png)

<span data-ttu-id="b4386-133">カスタム ビルド構成の変換ファイルを作成するには、Web.config ファイルを右クリックして**構成変換の追加**コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="b4386-133">You can create transformation files for custom build configurations by right-clicking the Web.config file and choosing **Add Config Transforms** from the context menu.</span></span> <span data-ttu-id="b4386-134">このチュートリアルでは、その必要はありませんし、任意のカスタム ビルド構成を作成していないために、メニュー オプションが無効にします。</span><span class="sxs-lookup"><span data-stu-id="b4386-134">For this tutorial you don't need to do that, and the menu option is disabled, because you haven't created any custom build configurations.</span></span>

<span data-ttu-id="b4386-135">後でを作成、テストでは、1 つずつ次の 3 つの複数変換ファイル ステージング、発行プロファイルを運用環境。</span><span class="sxs-lookup"><span data-stu-id="b4386-135">Later you'll create three more transformation files, one each for the test, staging, and production publish profiles.</span></span> <span data-ttu-id="b4386-136">移行先の環境に依存するため、発行プロファイルの変換ファイルでの処理と設定の典型的な例は、テストと運用環境のさまざまなである WCF エンドポイントです。</span><span class="sxs-lookup"><span data-stu-id="b4386-136">A typical example of a setting that you would handle in a publish profile transformation file because it depends on the destination environment is a WCF endpoint that is different for test versus production.</span></span> <span data-ttu-id="b4386-137">これらに対応する発行プロファイルを作成した後に、発行に、後のチュートリアルでのプロファイルの変換ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b4386-137">You'll create publish profile transformation files in later tutorials after you create the publish profiles that they go with.</span></span>

## <a name="disable-debug-mode"></a><span data-ttu-id="b4386-138">デバッグ モードを無効にします。</span><span class="sxs-lookup"><span data-stu-id="b4386-138">Disable debug mode</span></span>

<span data-ttu-id="b4386-139">移行先の環境ではなく、ビルド構成に依存する設定の例、`debug`属性。</span><span class="sxs-lookup"><span data-stu-id="b4386-139">An example of a setting that depends on build configuration rather than destination environment is the `debug` attribute.</span></span> <span data-ttu-id="b4386-140">リリース ビルドでは、通常するデバッグが無効にデプロイする環境に関係なく。</span><span class="sxs-lookup"><span data-stu-id="b4386-140">For a Release build, you typically want debugging disabled regardless of which environment you are deploying to.</span></span> <span data-ttu-id="b4386-141">そのため、既定では、Visual Studio プロジェクト テンプレート作成*Web.Release.config*を削除するコードを含むファイルの変換、`debug`属性を`compilation`要素。</span><span class="sxs-lookup"><span data-stu-id="b4386-141">Therefore, by default the Visual Studio project templates create *Web.Release.config* transform files with code that removes the `debug` attribute from the `compilation` element.</span></span> <span data-ttu-id="b4386-142">ここでは、既定の*Web.Release.config*: でコードが含まれますがコメント アウトされているサンプル変換コード、だけでなく、`compilation`要素を削除する、`debug`属性。</span><span class="sxs-lookup"><span data-stu-id="b4386-142">Here is the default *Web.Release.config*: in addition to some sample transformation code that is commented out, it includes code in the `compilation` element that removes the `debug` attribute:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

<span data-ttu-id="b4386-143">`xdt:Transform="RemoveAttributes(debug)"`属性を指定する、`debug`から削除する属性、`system.web/compilation`要素で、デプロイされている*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b4386-143">The `xdt:Transform="RemoveAttributes(debug)"` attribute specifies that you want the `debug` attribute to be removed from the `system.web/compilation` element in the deployed *Web.config* file.</span></span> <span data-ttu-id="b4386-144">これは、リリース ビルドをデプロイするたびに実行されます。</span><span class="sxs-lookup"><span data-stu-id="b4386-144">This will be done every time you deploy a Release build.</span></span>

## <a name="limit-error-log-access-to-administrators"></a><span data-ttu-id="b4386-145">管理者にエラー ログへのアクセスを制限します。</span><span class="sxs-lookup"><span data-stu-id="b4386-145">Limit error log access to administrators</span></span>

<span data-ttu-id="b4386-146">アプリケーションには、システムによって生成されたエラー ページの代わりに一般的なエラー ページが表示されますを使用して、アプリケーションの実行中にエラーがある場合、 [Elmah NuGet パッケージ](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)エラー ログとレポートします。</span><span class="sxs-lookup"><span data-stu-id="b4386-146">If there's an error while the application runs, the application displays a generic error page in place of the system-generated error page, and it uses the [Elmah NuGet package](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) for error logging and reporting.</span></span> <span data-ttu-id="b4386-147">`customErrors`アプリケーション内の要素*Web.config*ファイルは、エラー ページを指定します。</span><span class="sxs-lookup"><span data-stu-id="b4386-147">The `customErrors` element in the application *Web.config* file specifies the error page:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

<span data-ttu-id="b4386-148">エラー ページを表示するには、一時的に変更、`mode`の属性、`customErrors`要素から"RemoteOnly"を"オン"に、Visual Studio からアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="b4386-148">To see the error page, temporarily change the `mode` attribute of the `customErrors` element from "RemoteOnly" to "On" and run the application from Visual Studio.</span></span> <span data-ttu-id="b4386-149">エラーが発生するなど、無効な URL を要求することによって*Studentsxxx.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="b4386-149">Cause an error by requesting an invalid URL, such as *Studentsxxx.aspx*.</span></span> <span data-ttu-id="b4386-150">IIS で生成された「リソースが見つかりません」エラーではなくページの表示、 *GenericErrorPage.aspx*ページ。</span><span class="sxs-lookup"><span data-stu-id="b4386-150">Instead of an IIS-generated "The resource cannot be found" error page, you see the *GenericErrorPage.aspx* page.</span></span>

![[エラー] ページ](web-config-transformations/_static/image2.png)

<span data-ttu-id="b4386-152">エラー ログを表示するとポート番号の後、URL 内のすべてを置換*elmah.axd* (たとえば、 `http://localhost:51130/elmah.axd`) し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="b4386-152">To see the error log, replace everything in the URL after the port number with *elmah.axd* (for example, `http://localhost:51130/elmah.axd`) and press Enter:</span></span>

![ELMAH ページ](web-config-transformations/_static/image3.png)

<span data-ttu-id="b4386-154">設定することを忘れないでください、`customErrors`要素を完了すると、"RemoteOnly"モードに戻ります。</span><span class="sxs-lookup"><span data-stu-id="b4386-154">Don't forget to set the `customErrors` element back to "RemoteOnly" mode when you're done.</span></span>

<span data-ttu-id="b4386-155">開発用コンピューターには、エラー ログ ページには、セキュリティ リスクとなる運用環境での無料アクセスを許可すると便利です。</span><span class="sxs-lookup"><span data-stu-id="b4386-155">On your development computer it's convenient to allow free access to the error log page, but in production that would be a security risk.</span></span> <span data-ttu-id="b4386-156">実稼働サイトでは、対象管理者にエラー ログへのアクセスを制限する承認規則を追加して、制限が機能することを確認するテスト環境やステージングもします。</span><span class="sxs-lookup"><span data-stu-id="b4386-156">For the production site, you want to add an authorization rule that restricts error log access to administrators, and to make sure that the restriction works you want it in test and staging also.</span></span> <span data-ttu-id="b4386-157">これに属しているためと、リリース ビルドをデプロイするたびに実装するもう 1 つの変更は、そのため、 *Web.Release.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b4386-157">Therefore this is another change that you want to implement every time you deploy a Release build, and so it belongs in the *Web.Release.config* file.</span></span>

<span data-ttu-id="b4386-158">オープン*Web.Release.config*し、新しい追加`location`終了直前の要素`configuration`タグを次に示します。</span><span class="sxs-lookup"><span data-stu-id="b4386-158">Open *Web.Release.config* and add a new `location` element immediately before the closing `configuration` tag, as shown here.</span></span>

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

<span data-ttu-id="b4386-159">`Transform`属性値"Insert"の場合、この`location`に既存のすべての兄弟として追加する要素`location`内の要素、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b4386-159">The `Transform` attribute value of "Insert" causes this `location` element to be added as a sibling to any existing `location` elements in the *Web.config* file.</span></span> <span data-ttu-id="b4386-160">(既に 1 つである`location`の承認を指定する要素の規則、**更新クレジット**ページです)。</span><span class="sxs-lookup"><span data-stu-id="b4386-160">(There is already one `location` element that specifies authorization rules for the **Update Credits** page.)</span></span>

<span data-ttu-id="b4386-161">これで正しくコーディングすることを確認する変換をプレビューできます。</span><span class="sxs-lookup"><span data-stu-id="b4386-161">Now you can preview the transform to make sure that you coded it correctly.</span></span>

<span data-ttu-id="b4386-162">**ソリューション エクスプ ローラー**、右クリック*Web.Release.config*  をクリック**プレビュー変換**します。</span><span class="sxs-lookup"><span data-stu-id="b4386-162">In **Solution Explorer**, right-click *Web.Release.config* and click **Preview Transform**.</span></span>

![変換 メニューをプレビューします。](web-config-transformations/_static/image4.png)

<span data-ttu-id="b4386-164">開発を示すページが開きます*Web.config*左と上のファイル、デプロイされている*Web.config*ファイルのようになります、右側の変更が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="b4386-164">A page opens that shows you the development *Web.config* file on the left and what the deployed *Web.config* file will look like on the right, with changes highlighted.</span></span>

![デバッグの変換のプレビュー](web-config-transformations/_static/image5.png)

![変換の場所のプレビュー](web-config-transformations/_static/image6.png)

<span data-ttu-id="b4386-167">(プレビューでは、いくつか追加の変更を記述していない変換を確認可能性があります通常の機能には影響しませんホワイト スペースの削除などがあります。)。</span><span class="sxs-lookup"><span data-stu-id="b4386-167">( In the preview, you might notice some additional changes that you didn't write transforms for: these typically involve the removal of white space that doesn't affect functionality.)</span></span>

<span data-ttu-id="b4386-168">デプロイ後に、サイトをテストするときに、承認規則が有効であることを確認するテストも行います。</span><span class="sxs-lookup"><span data-stu-id="b4386-168">When you test the site after deployment, you'll also test to verify that the authorization rule is effective.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b4386-169">**セキュリティに関する注意**で実稼働アプリケーションでは、一般にエラーの詳細を表示または公共の場所にその情報を格納することはありません。</span><span class="sxs-lookup"><span data-stu-id="b4386-169">**Security Note** Never display error details to the public in a production application, or store that information in a public location.</span></span> <span data-ttu-id="b4386-170">攻撃者は、エラー情報を使用して、サイトの脆弱性を検出することができます。</span><span class="sxs-lookup"><span data-stu-id="b4386-170">Attackers can use error information to discover vulnerabilities in a site.</span></span> <span data-ttu-id="b4386-171">独自のアプリケーションで ELMAH を使用する場合は、セキュリティ上のリスクを最小限に抑える ELMAH を構成します。</span><span class="sxs-lookup"><span data-stu-id="b4386-171">If you use ELMAH in your own application, configure ELMAH to minimize security risks.</span></span> <span data-ttu-id="b4386-172">このチュートリアルの例では ELMAH しない推奨される構成を検討してください。</span><span class="sxs-lookup"><span data-stu-id="b4386-172">The ELMAH example in this tutorial should not be considered a recommended configuration.</span></span> <span data-ttu-id="b4386-173">アプリケーションはファイルを作成できる必要があるフォルダーを処理する方法を説明するために実装されている一例です。</span><span class="sxs-lookup"><span data-stu-id="b4386-173">It is an example that was chosen in order to illustrate how to handle a folder that the application must be able to create files in.</span></span> <span data-ttu-id="b4386-174">詳細については、次を参照してください。 [ELMAH エンドポイントをセキュリティで保護する](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)します。</span><span class="sxs-lookup"><span data-stu-id="b4386-174">For more information, see [securing the ELMAH endpoint](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).</span></span>


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a><span data-ttu-id="b4386-175">処理する設定プロファイル変換ファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="b4386-175">A setting that you'll handle in publish profile transformation files</span></span>

<span data-ttu-id="b4386-176">一般的なシナリオとして*Web.config*ファイルの設定を環境に展開するごとに変える必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4386-176">A common scenario is to have *Web.config* file settings that must be different in each environment that you deploy to.</span></span> <span data-ttu-id="b4386-177">たとえば、WCF サービスを呼び出すアプリケーションは、テストおよび運用環境で別のエンドポイントを必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4386-177">For example, an application that calls a WCF service might need a different endpoint in test and production environments.</span></span> <span data-ttu-id="b4386-178">Contoso University アプリケーションには、この種類の設定が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b4386-178">The Contoso University application includes a setting of this kind also.</span></span> <span data-ttu-id="b4386-179">この設定は、開発、テスト、運用などは、in、環境を示すサイトのページに表示されるインジケーターを制御します。</span><span class="sxs-lookup"><span data-stu-id="b4386-179">This setting controls a visible indicator on a site's pages that tells you which environment you are in, such as development, test, or production.</span></span> <span data-ttu-id="b4386-180">設定値は、アプリケーションが"(Dev)"を追加するかどうかを決定します。 または"(テスト)"メインの見出しに、 *Site.Master*マスター ページ。</span><span class="sxs-lookup"><span data-stu-id="b4386-180">The setting value determines whether the application will append "(Dev)" or "(Test)" to the main heading in the *Site.Master* master page:</span></span>

![環境のインジケーター](web-config-transformations/_static/image7.png)

<span data-ttu-id="b4386-182">アプリケーションがステージング環境または運用環境で実行されているときに、環境のインジケーターは省略されます。</span><span class="sxs-lookup"><span data-stu-id="b4386-182">The environment indicator is omitted when the application is running in staging or production.</span></span>

<span data-ttu-id="b4386-183">Contoso University の web ページで設定されている値の読み取り`appSettings`で、 *Web.config*ファイルで、アプリケーションが実行されているどのような環境を決定するには。</span><span class="sxs-lookup"><span data-stu-id="b4386-183">The Contoso University web pages read a value that is set in `appSettings` in the *Web.config* file in order to determine what environment the application is running in:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

<span data-ttu-id="b4386-184">値は、"Test"をする、テスト環境で、ステージングと運用のためには、"Prod"する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b4386-184">The value should be "Test" in the test environment, and "Prod" for staging and production.</span></span>

<span data-ttu-id="b4386-185">変換ファイルで次のコードでは、この変換を実装します。</span><span class="sxs-lookup"><span data-stu-id="b4386-185">The following code in a transform file will implement this transformation:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

<span data-ttu-id="b4386-186">`xdt:Transform`属性値"SetAttributes"では、この変換の目的で既存の要素の属性値を変更することを示します、 *Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b4386-186">The `xdt:Transform` attribute value "SetAttributes" indicates that the purpose of this transform is to change attribute values of an existing element in the *Web.config* file.</span></span> <span data-ttu-id="b4386-187">`xdt:Locator`属性値"Match(key)"では、変更する要素が、1 つであることを示しますが`key`属性と一致する、`key`ここで指定された属性。</span><span class="sxs-lookup"><span data-stu-id="b4386-187">The `xdt:Locator` attribute value "Match(key)" indicates that the element to be modified is the one whose `key` attribute matches the `key` attribute specified here.</span></span> <span data-ttu-id="b4386-188">のみその他の属性の`add`要素が`value`、変更される内容は、デプロイされているの*Web.config*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b4386-188">The only other attribute of the `add` element is `value`, and that is what will be changed in the deployed *Web.config* file.</span></span> <span data-ttu-id="b4386-189">エラーの原因を以下のコード、`value`の属性、 `Environment` `appSettings` "Test"に設定する要素、 *Web.config*が展開されているファイル。</span><span class="sxs-lookup"><span data-stu-id="b4386-189">The code shown here causes the `value` attribute of the `Environment` `appSettings` element to be set to "Test" in the *Web.config* file that is deployed.</span></span>

<span data-ttu-id="b4386-190">この変換は、発行プロファイルの変換ファイルをまだ作成していないに属しています。</span><span class="sxs-lookup"><span data-stu-id="b4386-190">This transform belongs in the publish profile transform files, which you haven't created yet.</span></span> <span data-ttu-id="b4386-191">作成し、テスト、ステージング、運用環境の発行プロファイルを作成するときに、この変更を実装する変換ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="b4386-191">You'll create and update the transform files that implement this change when you create the publish profiles for the test, staging, and production environments.</span></span> <span data-ttu-id="b4386-192">行います、[を IIS に配置](deploying-to-iis.md)と[運用環境にデプロイ](deploying-to-production.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="b4386-192">You'll do that in the [deploy to IIS](deploying-to-iis.md) and [deploy to production](deploying-to-production.md) tutorials.</span></span>

> [!NOTE]
> <span data-ttu-id="b4386-193">この設定のため、 `<appSettings>` Azure アプリ サービスの「Web アプリにデプロイするときに、変換を指定するための別の方法がある要素、[を指定する Web.config 設定を Azure](#watransforms)で以前このトピックの「します。</span><span class="sxs-lookup"><span data-stu-id="b4386-193">Because this setting is in the `<appSettings>` element, you have another alternative for specifying the transformation when you're deploying to Web Apps in Azure App Service See [Specifying Web.config settings in Azure](#watransforms) earlier in this topic.</span></span>


## <a name="setting-connection-strings"></a><span data-ttu-id="b4386-194">接続文字列の設定</span><span class="sxs-lookup"><span data-stu-id="b4386-194">Setting connection strings</span></span>

<span data-ttu-id="b4386-195">既定の変換ファイルには、接続文字列を更新する方法を示す例が含まれますが、ほとんどの場合する必要はありません、接続文字列の変換を設定するため、発行プロファイルの接続文字列を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="b4386-195">Although the default transform file contains an example that shows how to update a connection string, in most cases you do not need to set up connection string transformations, because you can specify connection strings in the publish profile.</span></span> <span data-ttu-id="b4386-196">行います、[を IIS に配置](deploying-to-iis.md)と[運用環境にデプロイ](deploying-to-production.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="b4386-196">You'll do that in the [deploy to IIS](deploying-to-iis.md) and [deploy to production](deploying-to-production.md) tutorials.</span></span>

## <a name="summary"></a><span data-ttu-id="b4386-197">まとめ</span><span class="sxs-lookup"><span data-stu-id="b4386-197">Summary</span></span>

<span data-ttu-id="b4386-198">同じくらいお行ったようになりました*Web.config*変換前に、発行プロファイルを作成して、デプロイされた Web.config ファイルではどのプレビューを見てきました。</span><span class="sxs-lookup"><span data-stu-id="b4386-198">You have now done as much as you can with *Web.config* transformations before you create the publish profiles, and you've seen a preview of what will be in the deployed Web.config file.</span></span>

![変換の場所のプレビュー](web-config-transformations/_static/image8.png)

<span data-ttu-id="b4386-200">次のチュートリアルではするに代わってプロジェクト プロパティの設定を必要とする展開のセットアップ タスク。</span><span class="sxs-lookup"><span data-stu-id="b4386-200">In the following tutorial, you'll take care of deployment set-up tasks that require setting project properties.</span></span>

## <a name="more-information"></a><span data-ttu-id="b4386-201">説明</span><span class="sxs-lookup"><span data-stu-id="b4386-201">More Information</span></span>

<span data-ttu-id="b4386-202">このチュートリアルで扱うトピックの詳細については、次を参照してください[デプロイ中に、対象の Web.config ファイルまたは app.config ファイルで設定を変更するのを使用して Web.config 変換](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms)での Web 配置コンテンツ マップ。Visual Studio および ASP.NET です。</span><span class="sxs-lookup"><span data-stu-id="b4386-202">For more information about topics covered by this tutorial, see [Using Web.config transformations to change settings in the destination Web.config file or app.config file during deployment](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) in the Web Deployment Content Map for Visual Studio and ASP.NET.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b4386-203">[前へ](preparing-databases.md)
> [次へ](project-properties.md)</span><span class="sxs-lookup"><span data-stu-id="b4386-203">[Previous](preparing-databases.md)
[Next](project-properties.md)</span></span>
