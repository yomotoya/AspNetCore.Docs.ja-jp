---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: OWIN スタートアップ クラス検出 |Microsoft Docs
author: Praburaj
description: このチュートリアルでは、OWIN スタートアップ クラスが読み込まれるを構成する方法を示します。 OWIN の詳細については、「プロジェクト Katana 概要を参照してください。 このチュートリアルがしています.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 591a04f429284ae73896807a6c2837ad498e8ae1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830235"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="3c3e0-105">OWIN スタートアップ クラス検出</span><span class="sxs-lookup"><span data-stu-id="3c3e0-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="3c3e0-106">によって[Praburaj Thiagarajan](https://github.com/Praburaj)、 [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="3c3e0-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="3c3e0-107">このチュートリアルでは、OWIN スタートアップ クラスが読み込まれるを構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="3c3e0-108">OWIN の詳細については、次を参照してください。[プロジェクト Katana の概要を、](an-overview-of-project-katana.md)します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="3c3e0-109">このチュートリアルの執筆者は、Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )、Praburaj Thiagarajan と Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="3c3e0-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3c3e0-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="3c3e0-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3c3e0-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="3c3e0-112">OWIN スタートアップ クラス検出</span><span class="sxs-lookup"><span data-stu-id="3c3e0-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="3c3e0-113">すべての OWIN アプリケーションには、アプリケーション パイプラインのコンポーネントを指定するスタートアップ クラスがあります。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="3c3e0-114">(OwinHost、IIS、および IIS Express) を選択したホスティング モデルに応じて、startup クラスを接続するには、ランタイムでさまざまな方法は。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="3c3e0-115">このチュートリアルで示すように、startup クラスは、すべてのホスト アプリケーションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="3c3e0-116">ホスティング ランタイムを使用して、これらのいずれかのアプローチでは、startup クラスを接続します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="3c3e0-117">**名前付け規則**: Katana という名前のクラスを探します`Startup`アセンブリ名またはグローバル名前空間に一致する名前空間。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="3c3e0-118">**OwinStartup 属性**: これは、startup クラスを指定するほとんどの開発者が採用するアプローチです。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="3c3e0-119">次の属性に startup クラスを設定、`TestStartup`クラス、`StartupDemo`名前空間。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="3c3e0-120">`OwinStartup`属性は、名前付け規則を上書きします。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="3c3e0-121">この属性で、フレンドリ名を指定することも、ただし、フレンドリ名を使用する必要がありますを使用しても、`appSetting`構成ファイル内の要素。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="3c3e0-122">**構成ファイル内の appSetting 要素**:`appSetting`要素をオーバーライド、`OwinStartup`属性と名前付け規則。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="3c3e0-123">複数のスタートアップ クラスがあることができます (を使用して各、`OwinStartup`属性) と構成のスタートアップ クラスは、次のようなマークアップを使用して構成ファイルに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="3c3e0-124">スタートアップ クラスとアセンブリを明示的に指定する次のキーも使用できます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="3c3e0-125">構成ファイルで次の XML のわかりやすいスタートアップ クラスの名前を指定する`ProductionConfiguration`します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="3c3e0-126">上記のマークアップは、次のために使用する必要があります`OwinStartup`フレンドリ名を指定し、属性、`ProductionStartup2`を実行するクラス。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="3c3e0-127">OWIN スタートアップの検出の追加を無効にする、`appSetting owin:AutomaticAppStartup`の値を持つ`"false"`web.config ファイルにします。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="3c3e0-128">OWIN Startup を使用して ASP.NET Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="3c3e0-129">空の Asp.Net web アプリケーションを作成し、名前**StartupDemo**します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="3c3e0-130">-インストール`Microsoft.Owin.Host.SystemWeb`NuGet パッケージ マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="3c3e0-131">**ツール**メニューの **ライブラリ パッケージ マネージャー**、し**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="3c3e0-132">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="3c3e0-133">OWIN startup クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-133">Add an OWIN startup class.</span></span> <span data-ttu-id="3c3e0-134">Visual Studio 2013 でプロジェクトを右クリックし、選択**クラスの追加**. -、**新しい項目の追加** ダイアログ ボックスに、入力*OWIN*検索フィールドに、および、Startup.cs に名前変更クリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   <span data-ttu-id="3c3e0-135">次回に追加する、 *Owin Startup クラス*はから利用可能な**追加**メニュー。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   <span data-ttu-id="3c3e0-136">または、プロジェクトを右クリックし、選択**追加**を選択し、**新しい項目の**、クリックして、 **Owin Startup クラス**。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="3c3e0-137">生成されたコードを置き換える、 *Startup.cs*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  <span data-ttu-id="3c3e0-138">`app.Use`ラムダ式を使用して、OWIN パイプラインにミドルウェアを指定したコンポーネントを登録します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="3c3e0-139">この場合、着信要求に応答する前に受信した要求のログ記録を設定しています。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="3c3e0-140">`next`パラメーターは、デリゲート ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [タスク](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) パイプラインの次のコンポーネントにします。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="3c3e0-141">`app.Run`ラムダ式は、パイプラインが受信要求をフックし、応答メカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="3c3e0-142">上記のコードで私たちがコメント アウト、`OwinStartup`という名前のクラスを実行中の規則で属性としている証明書利用者`Startup`.-キーを押して***F5***アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="3c3e0-143">更新を何回かクリックします。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  <span data-ttu-id="3c3e0-144">注: このチュートリアルでは、イメージに表示される数値は一致しません数。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="3c3e0-145">ミリ秒文字列は、ページを更新するときに、新しい応答を表示する使用されます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
  <span data-ttu-id="3c3e0-146">トレース情報を表示、**出力**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="3c3e0-147">複数のスタートアップ クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-147">Add More Startup Classes</span></span>

<span data-ttu-id="3c3e0-148">このセクションでは、別の Startup クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="3c3e0-149">アプリケーションには、複数の OWIN startup クラスを追加できます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="3c3e0-150">たとえば、開発、テスト、運用環境のスタートアップ クラスを作成する場合があります。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="3c3e0-151">新しい OWIN Startup クラスを作成し、名前`ProductionStartup`します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="3c3e0-152">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="3c3e0-153">アプリを実行するコントロールの f5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="3c3e0-154">`OwinStartup`属性は、運用環境の startup クラスが実行を指定します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="3c3e0-155">もう 1 つの OWIN Startup クラスを作成し、名前`TestStartup`します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="3c3e0-156">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="3c3e0-157">`OwinStartup`上記属性のオーバー ロードを指定します`TestingConfiguration`として、*フレンドリ*Startup クラスの名前。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="3c3e0-158">開く、 *web.config*ファイルを開き、Startup クラスのフレンドリ名を指定する OWIN アプリケーションのスタートアップ キーを追加します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="3c3e0-159">アプリを実行するコントロールの f5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="3c3e0-160">アプリ設定の要素が優先、され、テスト構成を実行します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="3c3e0-161">削除、*フレンドリ*名前を`OwinStartup`属性、`TestStartup`クラス。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="3c3e0-162">OWIN アプリケーションのスタートアップ キーを置き換える、 *web.config*を次のファイル。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="3c3e0-163">元に戻す、 `OwinStartup` Visual Studio によって生成される既定の属性のコードには、各クラスの属性。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="3c3e0-164">OWIN アプリケーションのスタートアップ キーを次の各を実行する運用クラスとなります。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="3c3e0-165">最後のスタートアップ キーには、スタートアップの構成方法を指定します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="3c3e0-166">次の OWIN アプリケーションのスタートアップ キーでは、configuration クラスの名前を変更できます。`MyConfiguration`します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="3c3e0-167">Owinhost.exe を使用します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="3c3e0-168">Web.config ファイルを次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="3c3e0-169">最後のキーが wins この場合のようになります`TestStartup`を指定します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="3c3e0-170">PMC から Owinhost をインストールします。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="3c3e0-171">アプリケーション フォルダーに移動します (フォルダーを含む、 *Web.config*ファイル) と入力してコマンド プロンプト。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="3c3e0-172">コマンド ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="3c3e0-173">URL でブラウザーを起動`http://localhost:5000/`します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   <span data-ttu-id="3c3e0-174">OwinHost には、上に示したスタートアップ規則が受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="3c3e0-175">コマンド ウィンドウで enter キーを押して OwinHost を終了します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="3c3e0-176">`ProductionStartup`クラスを次の OwinStartup 属性のフレンドリ名を指定する追加*ProductionConfiguration*します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="3c3e0-177">コマンド プロンプト」と入力します。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="3c3e0-178">運用環境の startup クラスが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
   <span data-ttu-id="3c3e0-179">アプリケーションは複数のスタートアップ クラスを備え、この例では、実行時までロードするスタートアップ クラスを遅延が。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="3c3e0-180">次のランタイム スタートアップ オプションをテストします。</span><span class="sxs-lookup"><span data-stu-id="3c3e0-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
