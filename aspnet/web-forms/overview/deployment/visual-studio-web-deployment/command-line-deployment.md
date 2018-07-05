---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Visual Studio を使用して ASP.NET Web のデプロイ: コマンドラインのデプロイ |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: f079beabccd253ff13c19d3192181ddbdf00b3bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830968"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="a0d66-103">Visual Studio を使用して ASP.NET Web のデプロイ: コマンドラインからデプロイ</span><span class="sxs-lookup"><span data-stu-id="a0d66-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="a0d66-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a0d66-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="a0d66-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a0d66-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="a0d66-106">このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="a0d66-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="a0d66-107">系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="a0d66-108">概要</span><span class="sxs-lookup"><span data-stu-id="a0d66-108">Overview</span></span>

<span data-ttu-id="a0d66-109">このチュートリアルでは、Visual Studio web を呼び出す方法、コマンドラインからのパイプラインを発行します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="a0d66-110">これはし場合に便利です[デプロイ プロセスを自動化](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)行う代わりに、手動で Visual Studio で、通常を使用して、[ソース コードのバージョン管理システム](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)。</span><span class="sxs-lookup"><span data-stu-id="a0d66-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="a0d66-111">変更をデプロイするには</span><span class="sxs-lookup"><span data-stu-id="a0d66-111">Make a change to deploy</span></span>

<span data-ttu-id="a0d66-112">現在、[About] ページでは、テンプレート コードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a0d66-112">Currently the About page displays the template code.</span></span>

![[About] ページのテンプレート コード](command-line-deployment/_static/image1.png)

<span data-ttu-id="a0d66-114">受講者の登録の概要を表示するコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a0d66-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="a0d66-115">開く、 *About.aspx*  ページで、すべての内部マークアップを削除、 `MainContent` `Content`要素、およびその場所に次のマークアップを挿入します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="a0d66-116">プロジェクトを実行し、選択、**について**ページ。</span><span class="sxs-lookup"><span data-stu-id="a0d66-116">Run the project and select the **About** page.</span></span>

![About ページ](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="a0d66-118">コマンドラインを使用してテストを展開します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="a0d66-119">もう 1 つのデータベースの変更、aspnet ContosoUniversity データベースのための無効化 dbDacFx データベース展開を展開しません。</span><span class="sxs-lookup"><span data-stu-id="a0d66-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="a0d66-120">開く、 **Web の発行**ウィザード、および、3 つの発行プロファイルをクリア、 **Update Database**チェック ボックスをオン、**設定**タブ。</span><span class="sxs-lookup"><span data-stu-id="a0d66-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="a0d66-121">Windows 8 のスタート ページで、検索**VS2012 用開発者コマンド プロンプト**します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="a0d66-122">アイコンを右クリックして**VS2012 用開発者コマンド プロンプト**クリック**管理者として実行**します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="a0d66-123">ソリューション ファイルへのパスをソリューション ファイルへのパスに置き換えて、コマンド プロンプトで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="a0d66-124">MSBuild では、ソリューションがビルドされ、テスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![コマンド ライン出力](command-line-deployment/_static/image3.png)

<span data-ttu-id="a0d66-126">ブラウザーを開きに移動`http://localhost/ContosoUniversity`、 をクリックし、**について**ページ展開が成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="a0d66-127">学生のテストを作成していない場合は、下の空のページが表示されます、**学生本文の統計情報**見出し。</span><span class="sxs-lookup"><span data-stu-id="a0d66-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="a0d66-128">移動して、**受講者**] ページで [**受講者の追加**、いくつかの受講者を追加し、しに戻ります、**について**学生の統計情報を表示するページ。</span><span class="sxs-lookup"><span data-stu-id="a0d66-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![テスト環境でのページについて](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="a0d66-130">キーのコマンド ライン オプション</span><span class="sxs-lookup"><span data-stu-id="a0d66-130">Key command line options</span></span>

<span data-ttu-id="a0d66-131">ソリューション ファイルのパスと 2 つのプロパティを MSBuild に渡されるが入力したコマンド:</span><span class="sxs-lookup"><span data-stu-id="a0d66-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="a0d66-132">個々 のプロジェクトの配置とソリューションのデプロイ</span><span class="sxs-lookup"><span data-stu-id="a0d66-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="a0d66-133">ソリューション ファイルを指定すると、すべてのプロジェクトがビルドするために、ソリューションとします。</span><span class="sxs-lookup"><span data-stu-id="a0d66-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="a0d66-134">ソリューションに複数の web プロジェクトがあれば、次の MSBuild の動作が適用されます。</span><span class="sxs-lookup"><span data-stu-id="a0d66-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="a0d66-135">コマンドラインで指定したプロパティは、すべてのプロジェクトに渡されます。</span><span class="sxs-lookup"><span data-stu-id="a0d66-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="a0d66-136">そのため、各 web プロジェクトには、指定した名前の発行プロファイルが必要です。</span><span class="sxs-lookup"><span data-stu-id="a0d66-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="a0d66-137">指定した場合`/p:PublishProfile=Test`、各 web プロジェクトがという名前の発行プロファイルをいる必要があります*テスト*します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="a0d66-138">正常にもう 1 つを構築することも時に 1 つのプロジェクトを発行する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="a0d66-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="a0d66-139">詳細については、stackoverflow スレッドを参照してください。 [2 つのパッケージに MSBuild が失敗した](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages)します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="a0d66-140">ソリューションではなく、個々 のプロジェクトを指定する場合は、Visual Studio のバージョンを指定するパラメーターを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0d66-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="a0d66-141">Visual Studio 2012 を使用している場合、コマンド ラインが次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="a0d66-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="a0d66-142">Visual Studio 2010 のバージョン番号は 10.0 です。</span><span class="sxs-lookup"><span data-stu-id="a0d66-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="a0d66-143">詳細については、次を参照してください。 [Visual Studio プロジェクトの互換性および VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi's ブログ。</span><span class="sxs-lookup"><span data-stu-id="a0d66-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="a0d66-144">発行プロファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-144">Specifying the publish profile</span></span>

<span data-ttu-id="a0d66-145">発行プロファイルを指定するには、名前またはへの完全パスで、 *.pubxml*ファイルを次の例に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="a0d66-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="a0d66-146">Web の発行コマンド ライン パブリッシングでサポートされるメソッド</span><span class="sxs-lookup"><span data-stu-id="a0d66-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="a0d66-147">発行方法の 3 つのコマンドラインによる発行がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="a0d66-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="a0d66-148">`MSDeploy` Web Deploy を使用して発行します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="a0d66-149">`Package` Web デプロイ パッケージを作成して発行します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="a0d66-150">作成した MSBuild コマンドから個別にパッケージをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0d66-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="a0d66-151">`FileSystem` -指定したフォルダーにファイルをコピーして発行します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="a0d66-152">ビルド構成とプラットフォームを指定します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="a0d66-153">Visual Studio またはコマンド ラインでは、ビルド構成とプラットフォームを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0d66-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="a0d66-154">発行プロファイルがという名前のプロパティを含める`LastUsedBuildConfiguration`と`LastUsedPlatform`が、プロジェクトをビルドする方法を決定するためにこれらのプロパティを設定することはできません。</span><span class="sxs-lookup"><span data-stu-id="a0d66-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="a0d66-155">詳細については、次を参照してください。 [MSBuild: 構成プロパティを設定する方法](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)Sayed Hashimi's ブログ。</span><span class="sxs-lookup"><span data-stu-id="a0d66-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="a0d66-156">ステージングへのデプロイします。</span><span class="sxs-lookup"><span data-stu-id="a0d66-156">Deploy to staging</span></span>

<span data-ttu-id="a0d66-157">Azure にデプロイするには、コマンドラインにパスワードを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0d66-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="a0d66-158">暗号化された形式で格納されているが Visual Studio で発行プロファイルでパスワードを保存した場合は、 *. >.pubxml.user*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a0d66-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="a0d66-159">コマンド ライン パラメーターでパスワードを渡すことが、コマンドラインの展開を行うと、そのファイルは MSBuild によってアクセスされません。</span><span class="sxs-lookup"><span data-stu-id="a0d66-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="a0d66-160">必要なパスワードをコピー、 *.publishsettings*ステージング web サイトを以前ダウンロードしたファイル。</span><span class="sxs-lookup"><span data-stu-id="a0d66-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="a0d66-161">パスワードの値である、 `userPWD` Web デプロイのための属性`publishProfile`要素。</span><span class="sxs-lookup"><span data-stu-id="a0d66-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web デプロイのパスワード](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="a0d66-163">Windows 8 のスタート ページで、検索**VS2012 用開発者コマンド プロンプト**、コマンド プロンプトを開く アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a0d66-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="a0d66-164">(に管理者として開きます今度はローカル コンピューターの IIS に配置を行わないためする必要はありません)。</span><span class="sxs-lookup"><span data-stu-id="a0d66-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="a0d66-165">ソリューション ファイルへのパスをパスに、ソリューション ファイルとパスワードを自分のパスワードに置き換えて、コマンド プロンプトで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="a0d66-166">このコマンドラインに追加のパラメーターが含まれることに注意してください:`/p:AllowUntrustedCertificate=true`します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="a0d66-167">このチュートリアルが書き込まれていると、`AllowUntrustedCertificate`コマンドラインから Azure に発行するときに、プロパティを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a0d66-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="a0d66-168">このバグの修正プログラムがリリースされると、そのパラメーターは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="a0d66-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="a0d66-169">ブラウザーを開き、ステージング サイトの URL に移動して をクリックし、**について**ページ展開が成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="a0d66-170">テスト環境の既に説明したように統計を表示するいくつかの受講者を作成する必要があります、**について**ページ。</span><span class="sxs-lookup"><span data-stu-id="a0d66-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="a0d66-171">運用環境にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="a0d66-171">Deploy to production</span></span>

<span data-ttu-id="a0d66-172">運用環境に展開するプロセスは、ステージング プロセスに似ています。</span><span class="sxs-lookup"><span data-stu-id="a0d66-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="a0d66-173">必要なパスワードをコピー、 *.publishsettings*実稼働 web サイトを以前ダウンロードしたファイル。</span><span class="sxs-lookup"><span data-stu-id="a0d66-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="a0d66-174">開いている**VS2012 用開発者コマンド プロンプト**します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="a0d66-175">ソリューション ファイルへのパスをパスに、ソリューション ファイルとパスワードを自分のパスワードに置き換えて、コマンド プロンプトで次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="a0d66-176">実際の運用サイトの場合も、データベースの変更があった場合は通常をコピーする、*アプリ\_offline.htm*ファイルを展開する前に、サイトと配置の成功後に削除します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="a0d66-177">ブラウザーを開き、ステージング サイトの URL に移動して をクリックし、**について**ページ展開が成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="a0d66-178">まとめ</span><span class="sxs-lookup"><span data-stu-id="a0d66-178">Summary</span></span>

<span data-ttu-id="a0d66-179">コマンドラインを使用してアプリケーションの更新プログラムを配置したようになりました。</span><span class="sxs-lookup"><span data-stu-id="a0d66-179">You have now deployed an application update by using the command line.</span></span>

![テスト環境でのページについて](command-line-deployment/_static/image6.png)

<span data-ttu-id="a0d66-181">次のチュートリアルでは、web を拡張する方法の例が表示されますパイプラインを発行します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="a0d66-182">この例では、プロジェクトに含まれていないファイルを配置する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a0d66-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a0d66-183">[前へ](deploying-a-database-update.md)
> [次へ](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="a0d66-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
