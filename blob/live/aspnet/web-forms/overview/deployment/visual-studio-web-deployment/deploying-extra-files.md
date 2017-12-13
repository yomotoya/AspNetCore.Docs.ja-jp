---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: "Visual Studio を使用した ASP.NET Web 展開: 余分なファイルの展開 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: a34607b25f6cf502f5fbf2fe51bf1937f470159e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a><span data-ttu-id="da048-103">Visual Studio を使用した ASP.NET Web 展開: 余分なファイルの展開</span><span class="sxs-lookup"><span data-stu-id="da048-103">ASP.NET Web Deployment using Visual Studio: Deploying Extra Files</span></span>
====================
<span data-ttu-id="da048-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="da048-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="da048-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="da048-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="da048-106">この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。</span><span class="sxs-lookup"><span data-stu-id="da048-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="da048-107">系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="da048-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="da048-108">概要</span><span class="sxs-lookup"><span data-stu-id="da048-108">Overview</span></span>

<span data-ttu-id="da048-109">このチュートリアルでは、web の発行、Visual Studio の展開時に、追加のタスクを実行するためのパイプラインを拡張する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="da048-109">This tutorial shows how to extend the Visual Studio web publish pipeline to do an additional task during deployment.</span></span> <span data-ttu-id="da048-110">タスクでは、プロジェクト フォルダーに移動先の web サイトに含まれていない余分なファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="da048-110">The task is to copy extra files that are not in the project folder to the destination web site.</span></span>

<span data-ttu-id="da048-111">このチュートリアルを 1 つの余分なファイルをコピーします: *robots.txt*です。</span><span class="sxs-lookup"><span data-stu-id="da048-111">For this tutorial you'll copy one extra file: *robots.txt*.</span></span> <span data-ttu-id="da048-112">実稼働環境ではなく、ステージングには、このファイルを展開するには。</span><span class="sxs-lookup"><span data-stu-id="da048-112">You want to deploy this file to staging but not to production.</span></span> <span data-ttu-id="da048-113">[実稼働環境に展開する](deploying-to-production.md)チュートリアル プロジェクトにこのファイルを追加し、実稼働環境を構成する発行プロファイルを除外します。</span><span class="sxs-lookup"><span data-stu-id="da048-113">In [the Deploying to Production](deploying-to-production.md) tutorial, you added this file to the project and configured the Production publish profile to exclude it.</span></span> <span data-ttu-id="da048-114">このチュートリアルでは、この状況は、いずれかのファイルを配置するプロジェクトに含めるようにすることが役に立つに対処する代替方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="da048-114">In this tutorial you'll see an alternative method to handle this situation, one that will be useful for any files that you want to deploy but don't want to include in the project.</span></span>

## <a name="move-the-robotstxt-file"></a><span data-ttu-id="da048-115">Robots.txt ファイルを移動します。</span><span class="sxs-lookup"><span data-stu-id="da048-115">Move the robots.txt file</span></span>

<span data-ttu-id="da048-116">別の方法で処理を準備する*robots.txt*、チュートリアルのこのセクションで、プロジェクトに含まれていないフォルダーにファイルを移動して、削除する*robots.txt*ステージングから環境。</span><span class="sxs-lookup"><span data-stu-id="da048-116">To prepare for a different method of handling *robots.txt*, in this section of the tutorial you move the file to a folder that is not included in the project, and you delete *robots.txt* from the staging environment.</span></span> <span data-ttu-id="da048-117">ファイルをその環境に展開する新しいメソッドが正常に機能していることを検証できるようにステージングからファイルを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da048-117">It is necessary to delete the file from staging so that you can verify that your new method of deploying the file to that environment is working correctly.</span></span>

1. <span data-ttu-id="da048-118">**ソリューション エクスプ ローラー**を右クリックし、 *robots.txt*ファイルし、をクリックして**プロジェクトから除外**です。</span><span class="sxs-lookup"><span data-stu-id="da048-118">In **Solution Explorer**, right-click the *robots.txt* file and click **Exclude From Project**.</span></span>
2. <span data-ttu-id="da048-119">Windows エクスプ ローラーを使用して、ソリューション フォルダーに新しいフォルダーを作成し、名前*ExtraFiles*です。</span><span class="sxs-lookup"><span data-stu-id="da048-119">Using Windows File Explorer, create a new folder in the solution folder and name it *ExtraFiles*.</span></span>
3. <span data-ttu-id="da048-120">移動、 *robots.txt*ファイルから、 *ContosoUniversity*プロジェクト フォルダーに、 *ExtraFiles*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="da048-120">Move the *robots.txt* file from the *ContosoUniversity* project folder to the *ExtraFiles* folder.</span></span>

    ![ExtraFiles フォルダー](deploying-extra-files/_static/image1.png)
4. <span data-ttu-id="da048-122">削除、FTP ツールを使用して、 *robots.txt*ステージング web サイトからのファイルです。</span><span class="sxs-lookup"><span data-stu-id="da048-122">Using your FTP tool, delete the *robots.txt* file from the staging web site.</span></span>

    <span data-ttu-id="da048-123">代わりを選択できます**先で追加ファイルを削除する****ファイルの発行オプション**上、**設定**ステージング発行プロファイルのタブとステージングに再発行します。</span><span class="sxs-lookup"><span data-stu-id="da048-123">As an alternative, you can select **Remove additional files at destination** under **File Publish Options** on the **Settings** tab of the Staging publish profile, and republish to staging.</span></span>

## <a name="update-the-publish-profile-file"></a><span data-ttu-id="da048-124">発行プロファイル ファイルを更新します。</span><span class="sxs-lookup"><span data-stu-id="da048-124">Update the publish profile file</span></span>

<span data-ttu-id="da048-125">必要なだけ*robots.txt* 、ステージング環境のため、それを展開するために更新する必要が唯一の発行プロファイルがステージングされます。</span><span class="sxs-lookup"><span data-stu-id="da048-125">You only need *robots.txt* in staging, so the only publish profile you need to update in order to deploy it is Staging.</span></span>

1. <span data-ttu-id="da048-126">Visual Studio で開く*Staging.pubxml*です。</span><span class="sxs-lookup"><span data-stu-id="da048-126">In Visual Studio, open *Staging.pubxml*.</span></span>
2. <span data-ttu-id="da048-127">終了する前に、ファイルの最後に`</Project>`タグは、次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="da048-127">At the end of the file, before the closing `</Project>` tag, add the following markup:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    <span data-ttu-id="da048-128">このコードでは、新しい*ターゲット*を配置する追加のファイルが収集されます。</span><span class="sxs-lookup"><span data-stu-id="da048-128">This code creates a new *target* that will collect additional files to be deployed.</span></span> <span data-ttu-id="da048-129">ターゲットは 1 つまたは指定した条件に基づいて、MSBuild を実行するタスクの詳細。</span><span class="sxs-lookup"><span data-stu-id="da048-129">A target is composed of one or more tasks that MSBuild will execute based on conditions you specify.</span></span>

    <span data-ttu-id="da048-130">`Include`属性を指定、ファイルを検索するためのフォルダーは、ある*ExtraFiles*プロジェクト フォルダーと同じレベルに配置されている。</span><span class="sxs-lookup"><span data-stu-id="da048-130">The `Include` attribute specifies that the folder in which to find the files is *ExtraFiles*, located at the same level as the project folder.</span></span> <span data-ttu-id="da048-131">MSBuild (2 つのアスタリスクを指定するサブフォルダーを再帰的な) すべてのサブフォルダーからを再帰的にそのフォルダーからすべてのファイルが収集されます。</span><span class="sxs-lookup"><span data-stu-id="da048-131">MSBuild will collect all files from that folder and recursively from any subfolders (the double asterisk specifies recursive subfolders).</span></span> <span data-ttu-id="da048-132">このコードを持つでした配置する複数のファイル、およびファイル内のサブフォルダーに、 *ExtraFiles*フォルダー、およびそのすべてが展開されます。</span><span class="sxs-lookup"><span data-stu-id="da048-132">With this code you could put multiple files, and files in subfolders inside the *ExtraFiles* folder, and all will be deployed.</span></span>

    <span data-ttu-id="da048-133">`DestinationRelativePath`要素は、ファイルとフォルダーがで見つかった場合は、移動先 web サイトの同じファイルおよびフォルダー構造内のルート フォルダーにコピーする必要がありますを指定します、 *ExtraFiles*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="da048-133">The `DestinationRelativePath` element specifies that the folders and files should be copied to the root folder of the destination web site, in the same file and folder structure as they are found in the *ExtraFiles* folder.</span></span> <span data-ttu-id="da048-134">コピーする場合は、 *ExtraFiles*フォルダー自体、`DestinationRelativePath`値になります*ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*です。</span><span class="sxs-lookup"><span data-stu-id="da048-134">If you wanted to copy the *ExtraFiles* folder itself, the `DestinationRelativePath` value would be *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.</span></span>
3. <span data-ttu-id="da048-135">終了する前に、ファイルの最後に`</Project>`タグは、新しいターゲットを実行するかを指定する次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="da048-135">At the end of the file, before the closing `</Project>` tag, add the following markup that specifies when to execute the new target.</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    <span data-ttu-id="da048-136">このコードにより、新しい`CustomCollectFiles`先フォルダーにファイルをコピーするターゲットが実行されるたびに実行するターゲット。</span><span class="sxs-lookup"><span data-stu-id="da048-136">This code causes the new `CustomCollectFiles` target to be executed whenever the target that copies files to the destination folder is executed.</span></span> <span data-ttu-id="da048-137">別のターゲットが展開パッケージの作成と発行し、発行ではなく、展開パッケージを使用して、展開を決定する場合に、新しいターゲットが両方のターゲットに挿入されます。</span><span class="sxs-lookup"><span data-stu-id="da048-137">There is a separate target for publish versus deployment package creation, and the new target is injected in both targets in case you decide to deploy by using a deployment package instead of publishing.</span></span>

    <span data-ttu-id="da048-138">*.Pubxml*ファイルは次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="da048-138">The *.pubxml* file now looks like the following example:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. <span data-ttu-id="da048-139">保存して閉じます、 *Staging.pubxml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="da048-139">Save and close the *Staging.pubxml* file.</span></span>

## <a name="publish-to-staging"></a><span data-ttu-id="da048-140">ステージングへの公開します。</span><span class="sxs-lookup"><span data-stu-id="da048-140">Publish to staging</span></span>

<span data-ttu-id="da048-141">1 回のクリックを使用して発行またはコマンド ラインで、ステージングのプロファイルを使用して、アプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="da048-141">Using one-click publish or the command line, publish the application by using the Staging profile.</span></span>

<span data-ttu-id="da048-142">1 回のクリックを使用する場合は、次の発行のことを確認することができます、**プレビュー**ウィンドウを*robots.txt*コピーされます。</span><span class="sxs-lookup"><span data-stu-id="da048-142">If you use one-click publish, you can verify in the **Preview** window that *robots.txt* will be copied.</span></span> <span data-ttu-id="da048-143">それ以外の場合、FTP ツールを使用していることを確認、 *robots.txt*ファイルが、web サイトのルート フォルダーに配置します。</span><span class="sxs-lookup"><span data-stu-id="da048-143">Otherwise, use your FTP tool to verify that the *robots.txt* file is in the root folder of the web site after deployment.</span></span>

## <a name="summary"></a><span data-ttu-id="da048-144">概要</span><span class="sxs-lookup"><span data-stu-id="da048-144">Summary</span></span>

<span data-ttu-id="da048-145">これは、この一連のサード パーティのホスティング プロバイダーへの ASP.NET web アプリケーションの配置に関するチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="da048-145">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="da048-146">これらのチュートリアルで説明されているトピックのいずれかの詳細については、次を参照してください。、 [ASP.NET 展開のコンテンツ マップ](https://go.microsoft.com/fwlink/p/?LinkId=282413)です。</span><span class="sxs-lookup"><span data-stu-id="da048-146">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://go.microsoft.com/fwlink/p/?LinkId=282413).</span></span>

## <a name="more-information"></a><span data-ttu-id="da048-147">詳細情報</span><span class="sxs-lookup"><span data-stu-id="da048-147">More information</span></span>

<span data-ttu-id="da048-148">MSBuild ファイルを操作する方法がわかっている場合でコードを記述して他の展開タスクの多くを自動化できます*.pubxml* (プロファイルに固有のタスク) のファイルまたはプロジェクト*. wpp.targets*ファイル (のタスクプロファイルに適用されるすべて)。</span><span class="sxs-lookup"><span data-stu-id="da048-148">If you know how to work with MSBuild files, you can automate many other deployment tasks by writing code in *.pubxml* files (for profile-specific tasks) or the project *.wpp.targets* file (for tasks that apply to all profiles).</span></span> <span data-ttu-id="da048-149">詳細については*.pubxml*と*. wpp.targets*ファイルを参照してください[する方法: 発行プロファイル (.pubxml) ファイルでの展開設定の編集、および wpp.targets Visual Studio Web 内のファイル。プロジェクト](https://msdn.microsoft.com/en-us/library/ff398069)です。</span><span class="sxs-lookup"><span data-stu-id="da048-149">For more information about *.pubxml* and *.wpp.targets* files, see [How to: Edit Deployment Settings in Publish Profile (.pubxml) Files and the .wpp.targets File in Visual Studio Web Projects](https://msdn.microsoft.com/en-us/library/ff398069).</span></span> <span data-ttu-id="da048-150">MSBuild のコードに基本的な概要については、次を参照してください。**プロジェクト ファイルの構造**で[エンタープライズ展開シリーズ: プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。</span><span class="sxs-lookup"><span data-stu-id="da048-150">For a basic introduction to MSBuild code, see **The Anatomy of a Project File** in [Enterprise Deployment Series: Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="da048-151">タスク、独自のシナリオを実行する MSBuild ファイルを操作する方法については、このブックを参照してください。:[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://msbuildbook.com)Sayed Ibraham Hashimi して William Bartholomew です。</span><span class="sxs-lookup"><span data-stu-id="da048-151">To learn how to work with MSBuild files to perform tasks for your own scenarios, see this book: [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://msbuildbook.com) by Sayed Ibraham Hashimi and William Bartholomew.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="da048-152">謝辞</span><span class="sxs-lookup"><span data-stu-id="da048-152">Acknowledgements</span></span>

<span data-ttu-id="da048-153">このチュートリアル シリーズのコンテンツに多大な協力者次の方々 に感謝したいと思います。</span><span class="sxs-lookup"><span data-stu-id="da048-153">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="da048-154">Alberto Poblacion、MVP &amp; MCT、スペイン</span><span class="sxs-lookup"><span data-stu-id="da048-154">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/en-us/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="da048-155">Jarod Ferguson、データ プラットフォームの開発 MVP、United States</span><span class="sxs-lookup"><span data-stu-id="da048-155">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="da048-156">過酷 Mittal、Microsoft</span><span class="sxs-lookup"><span data-stu-id="da048-156">Harsh Mittal, Microsoft</span></span>
- <span data-ttu-id="da048-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="da048-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- [<span data-ttu-id="da048-158">Kristina Olson、Microsoft</span><span class="sxs-lookup"><span data-stu-id="da048-158">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="da048-159">Mike 教皇、Microsoft</span><span class="sxs-lookup"><span data-stu-id="da048-159">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="da048-160">Mohit Srivastava、Microsoft</span><span class="sxs-lookup"><span data-stu-id="da048-160">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="da048-161">Raffaele Rialdi (イタリア)</span><span class="sxs-lookup"><span data-stu-id="da048-161">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="da048-162">Rick Anderson、Microsoft</span><span class="sxs-lookup"><span data-stu-id="da048-162">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="da048-163">[Sayed Hashimi、Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="da048-163">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="da048-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="da048-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="da048-165">[Scott ハンター、Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="da048-165">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="da048-166">Srđan Božović、セルビア</span><span class="sxs-lookup"><span data-stu-id="da048-166">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="da048-167">[Vishal Joshi、Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="da048-167">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="da048-168">[前へ](command-line-deployment.md)
[次へ](troubleshooting.md)</span><span class="sxs-lookup"><span data-stu-id="da048-168">[Previous](command-line-deployment.md)
[Next](troubleshooting.md)</span></span>
