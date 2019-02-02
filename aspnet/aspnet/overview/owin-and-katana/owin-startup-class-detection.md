---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: OWIN スタートアップ クラス検出 |Microsoft Docs
author: Praburaj
description: このチュートリアルでは、OWIN スタートアップ クラスが読み込まれるを構成する方法を示します。 OWIN の詳細については、「プロジェクト Katana 概要を参照してください。 このチュートリアルがしています.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0b34cca8b48383dbb028106651758dff889ed614
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667298"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="a43c3-105">OWIN スタートアップ クラス検出</span><span class="sxs-lookup"><span data-stu-id="a43c3-105">OWIN Startup Class Detection</span></span>
====================

> <span data-ttu-id="a43c3-106">このチュートリアルでは、OWIN スタートアップ クラスが読み込まれるを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="a43c3-107">OWIN の詳細については、次を参照してください。[プロジェクト Katana の概要を、](an-overview-of-project-katana.md)します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="a43c3-108">このチュートリアルの執筆者は、Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )、Praburaj Thiagarajan と Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。</span><span class="sxs-lookup"><span data-stu-id="a43c3-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="a43c3-109">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a43c3-109">Prerequisites</span></span>
>
> [<span data-ttu-id="a43c3-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a43c3-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="a43c3-111">OWIN スタートアップ クラス検出</span><span class="sxs-lookup"><span data-stu-id="a43c3-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="a43c3-112">すべての OWIN アプリケーションには、アプリケーション パイプラインのコンポーネントを指定するスタートアップ クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="a43c3-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="a43c3-113">(OwinHost、IIS、および IIS Express) を選択したホスティング モデルに応じて、startup クラスを接続するには、ランタイムでさまざまな方法は。</span><span class="sxs-lookup"><span data-stu-id="a43c3-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="a43c3-114">このチュートリアルで示すように、startup クラスは、すべてのホスト アプリケーションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="a43c3-115">ホスティング ランタイムを使用して、これらのいずれかのアプローチでは、startup クラスを接続します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="a43c3-116">**名前付け規則**:Katana という名前のクラスを探します`Startup`アセンブリ名またはグローバル名前空間に一致する名前空間。</span><span class="sxs-lookup"><span data-stu-id="a43c3-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="a43c3-117">**OwinStartup 属性**:これは、startup クラスを指定するほとんどの開発者が採用するアプローチです。</span><span class="sxs-lookup"><span data-stu-id="a43c3-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="a43c3-118">次の属性に startup クラスを設定、`TestStartup`クラス、`StartupDemo`名前空間。</span><span class="sxs-lookup"><span data-stu-id="a43c3-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="a43c3-119">`OwinStartup`属性は、名前付け規則を上書きします。</span><span class="sxs-lookup"><span data-stu-id="a43c3-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="a43c3-120">この属性で、フレンドリ名を指定することも、ただし、フレンドリ名を使用する必要がありますを使用しても、`appSetting`構成ファイル内の要素。</span><span class="sxs-lookup"><span data-stu-id="a43c3-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="a43c3-121">**構成ファイル内の appSetting 要素**:`appSetting`要素をオーバーライド、`OwinStartup`属性と名前付け規則。</span><span class="sxs-lookup"><span data-stu-id="a43c3-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="a43c3-122">複数のスタートアップ クラスがあることができます (を使用して各、`OwinStartup`属性) と構成のスタートアップ クラスは、次のようなマークアップを使用して構成ファイルに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="a43c3-123">スタートアップ クラスとアセンブリを明示的に指定する次のキーも使用できます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="a43c3-124">構成ファイルで次の XML のわかりやすいスタートアップ クラスの名前を指定する`ProductionConfiguration`します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="a43c3-125">上記のマークアップは、次のために使用する必要があります`OwinStartup`フレンドリ名を指定し、属性、`ProductionStartup2`を実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="a43c3-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="a43c3-126">OWIN スタートアップの検出の追加を無効にする、`appSetting owin:AutomaticAppStartup`の値を持つ`"false"`web.config ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="a43c3-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="a43c3-127">OWIN Startup を使用して ASP.NET Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="a43c3-128">空の Asp.Net web アプリケーションを作成し、名前**StartupDemo**します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="a43c3-129">-インストール`Microsoft.Owin.Host.SystemWeb`NuGet パッケージ マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="a43c3-130">**ツール**メニューの  **NuGet パッケージ マネージャー**、し**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="a43c3-131">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="a43c3-132">OWIN startup クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-132">Add an OWIN startup class.</span></span> <span data-ttu-id="a43c3-133">Visual Studio 2017 でプロジェクトを右クリックし、選択**クラスの追加**. -、**新しい項目の追加** ダイアログ ボックスに、入力*OWIN*検索フィールドに、および、Startup.cs に名前変更選び**追加**します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="a43c3-134">次回に追加する、 *Owin Startup クラス*はから利用可能な**追加**メニュー。</span><span class="sxs-lookup"><span data-stu-id="a43c3-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="a43c3-135">または、プロジェクトを右クリックし、選択**追加**を選択し、**新しい項目の**、クリックして、 **Owin Startup クラス**。</span><span class="sxs-lookup"><span data-stu-id="a43c3-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="a43c3-136">生成されたコードを置き換える、 *Startup.cs*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="a43c3-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="a43c3-137">`app.Use`ラムダ式を使用して、OWIN パイプラインにミドルウェアを指定したコンポーネントを登録します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="a43c3-138">この場合、着信要求に応答する前に受信した要求のログ記録を設定しています。</span><span class="sxs-lookup"><span data-stu-id="a43c3-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="a43c3-139">`next`パラメーターは、デリゲート ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [タスク](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) パイプラインの次のコンポーネントにします。</span><span class="sxs-lookup"><span data-stu-id="a43c3-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="a43c3-140">`app.Run`ラムダ式は、パイプラインが受信要求をフックし、応答メカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="a43c3-141">上記のコードで私たちがコメント アウト、`OwinStartup`という名前のクラスを実行中の規則で属性としている証明書利用者`Startup`.-キーを押して***F5***アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="a43c3-142">更新を何回かクリックします。</span><span class="sxs-lookup"><span data-stu-id="a43c3-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="a43c3-143">![](owin-startup-class-detection/_static/image4.png) 注:このチュートリアルでは、イメージ内の数字では、この数が一致しません。</span><span class="sxs-lookup"><span data-stu-id="a43c3-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="a43c3-144">ミリ秒文字列は、ページを更新するときに、新しい応答を表示する使用されます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="a43c3-145">トレース情報を表示、**出力**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="a43c3-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="a43c3-146">複数のスタートアップ クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-146">Add More Startup Classes</span></span>

<span data-ttu-id="a43c3-147">このセクションでは、別の Startup クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="a43c3-148">アプリケーションには、複数の OWIN startup クラスを追加できます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="a43c3-149">たとえば、開発、テスト、運用環境のスタートアップ クラスを作成する場合があります。</span><span class="sxs-lookup"><span data-stu-id="a43c3-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="a43c3-150">新しい OWIN Startup クラスを作成し、名前`ProductionStartup`します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="a43c3-151">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="a43c3-152">アプリを実行するコントロールの f5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="a43c3-153">`OwinStartup`属性は、運用環境の startup クラスが実行を指定します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="a43c3-154">もう 1 つの OWIN Startup クラスを作成し、名前`TestStartup`します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="a43c3-155">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="a43c3-156">`OwinStartup`上記属性のオーバー ロードを指定します`TestingConfiguration`として、*フレンドリ*Startup クラスの名前。</span><span class="sxs-lookup"><span data-stu-id="a43c3-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="a43c3-157">開く、 *web.config*ファイルを開き、Startup クラスのフレンドリ名を指定する OWIN アプリケーションのスタートアップ キーを追加します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="a43c3-158">アプリを実行するコントロールの f5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="a43c3-159">アプリ設定の要素が優先、され、テスト構成を実行します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="a43c3-160">削除、*フレンドリ*名前を`OwinStartup`属性、`TestStartup`クラス。</span><span class="sxs-lookup"><span data-stu-id="a43c3-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="a43c3-161">OWIN アプリケーションのスタートアップ キーを置き換える、 *web.config*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="a43c3-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="a43c3-162">元に戻す、 `OwinStartup` Visual Studio によって生成される既定の属性のコードには、各クラスの属性。</span><span class="sxs-lookup"><span data-stu-id="a43c3-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="a43c3-163">OWIN アプリケーションのスタートアップ キーを次の各を実行する運用クラスとなります。</span><span class="sxs-lookup"><span data-stu-id="a43c3-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="a43c3-164">最後のスタートアップ キーには、スタートアップの構成方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="a43c3-165">次の OWIN アプリケーションのスタートアップ キーでは、configuration クラスの名前を変更できます。`MyConfiguration`します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="a43c3-166">Owinhost.exe を使用します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="a43c3-167">Web.config ファイルを次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="a43c3-168">最後のキーが wins この場合のようになります`TestStartup`を指定します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="a43c3-169">PMC から Owinhost をインストールします。</span><span class="sxs-lookup"><span data-stu-id="a43c3-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="a43c3-170">アプリケーション フォルダーに移動します (フォルダーを含む、 *Web.config*ファイル) と入力してコマンド プロンプト。</span><span class="sxs-lookup"><span data-stu-id="a43c3-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="a43c3-171">コマンド ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="a43c3-172">URL でブラウザーを起動`http://localhost:5000/`します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="a43c3-173">OwinHost には、上に示したスタートアップ規則が受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="a43c3-174">コマンド ウィンドウで enter キーを押して OwinHost を終了します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="a43c3-175">`ProductionStartup`クラスを次の OwinStartup 属性のフレンドリ名を指定する追加*ProductionConfiguration*します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="a43c3-176">コマンド プロンプト」と入力します。</span><span class="sxs-lookup"><span data-stu-id="a43c3-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="a43c3-177">運用環境の startup クラスが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="a43c3-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="a43c3-178">アプリケーションは複数のスタートアップ クラスを備え、この例では、実行時までロードするスタートアップ クラスを遅延が。</span><span class="sxs-lookup"><span data-stu-id="a43c3-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="a43c3-179">次のランタイム スタートアップ オプションをテストします。</span><span class="sxs-lookup"><span data-stu-id="a43c3-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
