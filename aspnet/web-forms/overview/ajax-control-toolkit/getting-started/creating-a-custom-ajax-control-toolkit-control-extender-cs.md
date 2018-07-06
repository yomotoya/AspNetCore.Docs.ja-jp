---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Toolkit コントロール エクステンダー (c#) を制御するカスタムの AJAX の作成 |Microsoft Docs
author: microsoft
description: カスタム エクステンダーを使用すると、カスタマイズし、新しいクラスを作成することがなく ASP.NET コントロールの機能を拡張できます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 0dd243ec1709324174ff48b52e7dfb5185d3aaa2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390954"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="e1c1f-103">カスタム AJAX Control Toolkit コントロール エクステンダー (c#) を作成します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>
====================
<span data-ttu-id="e1c1f-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e1c1f-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e1c1f-105">カスタム エクステンダーを使用すると、カスタマイズし、新しいクラスを作成することがなく ASP.NET コントロールの機能を拡張できます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="e1c1f-106">このチュートリアルでは、カスタム AJAX Control Toolkit コントロール エクステンダーを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="e1c1f-107">便利で新しいエクステンダーをテキスト ボックスにテキストを入力するときに有効と無効のボタンの状態を変更するが、単純なを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="e1c1f-108">このチュートリアルを読むと、次の独自のコントロール エクステンダーを使用して ASP.NET AJAX ツールキットを拡張することができます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="e1c1f-109">Visual Studio または Visual Web Developer を使用してカスタム コントロール エクステンダーを作成することができます (Visual Web Developer の最新バージョンがあることを確認してください)。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="e1c1f-110">DisabledButton エクステンダーの概要</span><span class="sxs-lookup"><span data-stu-id="e1c1f-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="e1c1f-111">DisabledButton extender の名前は、新しいコントロール エクステンダー。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="e1c1f-112">このエクステンダーでは、次の 3 つのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-112">This extender will have three properties:</span></span>

- <span data-ttu-id="e1c1f-113">TargetControlID - テキスト ボックス コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="e1c1f-114">TargetButtonIID - が無効または有効にするボタン。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="e1c1f-115">DisabledText - 最初に、ボタンに表示されるテキスト。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="e1c1f-116">入力を開始するときに、ボタンには、ボタンのテキスト プロパティの値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="e1c1f-117">テキスト ボックスとボタン コントロールに DisabledButton エクステンダーをフックするとします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="e1c1f-118">任意のテキストを入力する前に、ボタンが無効にし、ボタンをクリックし、テキスト ボックスに次のようになります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="e1c1f-119">([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="e1c1f-120">テキストの入力を開始した後、ボタンが有効になっているし、ボタンをクリックし、テキスト ボックスに次のようになります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="e1c1f-121">([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="e1c1f-122">このコントロール エクステンダーを作成するには、次の 3 つのファイルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="e1c1f-123">DisabledButtonExtender.cs - このファイルは、extender の作成を管理およびデザイン時にプロパティを設定することはサーバー側コントロールのクラスです。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="e1c1f-124">Extender に設定できるプロパティも定義します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="e1c1f-125">これらのプロパティは、コードを使用して、およびデザイン時にアクセスし、DisableButtonBehavior.js ファイルで定義されているプロパティと一致します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="e1c1f-126">DisabledButtonBehavior.js--このファイルは、すべてのクライアント スクリプトのロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="e1c1f-127">DisabledButtonDesigner.cs - このクラスは、デザイン時の機能を使用できます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="e1c1f-128">Visual Studio または Visual Web Developer デザイナーで正しく機能するコントロール エクステンダーをする場合は、このクラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="e1c1f-129">したがってコントロール エクステンダーは、サーバー側コントロール、クライアント側の動作、およびサーバー側デザイナー クラスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="e1c1f-130">次のセクションでこれらのファイルの 3 つすべてを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="e1c1f-131">カスタム エクステンダーの web サイトおよびプロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="e1c1f-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="e1c1f-132">最初の手順では、Visual Studio または Visual Web Developer でクラス ライブラリ プロジェクトと web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="e1c1f-133">私たち ll クラス ライブラリ プロジェクトでカスタム エクステンダーを作成し、web サイトでカスタム エクステンダーをテストします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="e1c1f-134">S の web サイトが開始できるようにします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-134">Let�s start with the website.</span></span> <span data-ttu-id="e1c1f-135">Web サイトを作成する次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="e1c1f-136">メニュー オプションを選択**ファイル]、[新しい Web サイト**します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="e1c1f-137">選択、 **ASP.NET Web サイト**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="e1c1f-138">新しい web サイトの名前を付けます*Website1*します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="e1c1f-139">をクリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-139">Click the **OK** button.</span></span>

<span data-ttu-id="e1c1f-140">次に、コントロール エクステンダーのコードを含むクラス ライブラリ プロジェクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="e1c1f-141">メニュー オプションを選択**ファイルを追加、新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="e1c1f-142">選択、**クラス ライブラリ**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="e1c1f-143">名前の新しいクラス ライブラリの名前を付けます**CustomExtenders**します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="e1c1f-144">をクリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-144">Click the **OK** button.</span></span>

<span data-ttu-id="e1c1f-145">次の手順を完了すると、図 1 よう、ソリューション エクスプ ローラー ウィンドウになります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="e1c1f-146">[![Web サイトとクラス ライブラリ プロジェクトとソリューション](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e1c1f-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="e1c1f-147">**図 01**: web サイトとクラス ライブラリ プロジェクトとソリューション ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="e1c1f-148">次に、すべてのクラス ライブラリ プロジェクトに必要なアセンブリ参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="e1c1f-149">CustomExtenders プロジェクトを右クリックし、メニュー オプションを選択**参照の追加**します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="e1c1f-150">[.NET] タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="e1c1f-151">次のアセンブリへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="e1c1f-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="e1c1f-152">System.Web.dll</span></span>
    2. <span data-ttu-id="e1c1f-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="e1c1f-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="e1c1f-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="e1c1f-154">System.Design.dll</span></span>
    4. <span data-ttu-id="e1c1f-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="e1c1f-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="e1c1f-156">[参照] タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="e1c1f-157">AjaxControlToolkit.dll アセンブリへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="e1c1f-158">このアセンブリは、AJAX Control Toolkit をダウンロードしたフォルダーにあります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="e1c1f-159">次の手順を完了すると、図 2 よう、クラス ライブラリ プロジェクト参照 フォルダーになります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="e1c1f-160">[![必要な参照と参照 フォルダー](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="e1c1f-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="e1c1f-161">**図 02**: 必要な参照と参照 フォルダー ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="e1c1f-162">カスタム コントロール エクステンダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="e1c1f-163">クラス ライブラリをしたら、エクステンダー コントロールの作成を始めることができます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="e1c1f-164">S カスタム エクステンダー コントロール クラス (1 のリストを参照) のベア ボーンを始めることができます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="e1c1f-165">**1 - MyCustomExtender.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e1c1f-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="e1c1f-166">リスト 1 でコントロール エクステンダー クラスに関する注意するいくつかの点があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="e1c1f-167">最初に、ExtenderControlBase の基本クラスからクラスを継承することを確認します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="e1c1f-168">すべての AJAX Control Toolkit エクステンダー コントロールは、この基本クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="e1c1f-169">たとえば、基底クラスには、すべてコントロール エクステンダーの必要なプロパティである TargetID プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="e1c1f-170">次に、クラスがクライアント スクリプトに関連する次の 2 つの属性が含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="e1c1f-171">WebResource - には、アセンブリ内の埋め込みリソースとして含まれるファイルが原因です。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="e1c1f-172">ClientScriptResource - は、アセンブリから取得するスクリプト リソースをによりします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="e1c1f-173">WebResource 属性は、アセンブリに埋め込む MyControlBehavior.js JavaScript ファイル、カスタム エクステンダーはコンパイル時に使用されます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="e1c1f-174">ClientScriptResource 属性は web ページで、カスタム エクステンダーを使用する場合は、アセンブリから MyControlBehavior.js スクリプトを取得するために使用します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="e1c1f-175">WebResource および ClientScriptResource 属性操作するためには、埋め込みリソースとしての JavaScript ファイルをコンパイルする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="e1c1f-176">ソリューション エクスプ ローラー ウィンドウで、ファイルを選択、プロパティ シートを開き、値を割り当てる*埋め込まれたリソース*を**ビルド アクション**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="e1c1f-177">コントロールのエクステンダーが TargetControlType 属性も含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="e1c1f-178">この属性を使用して、コントロール エクステンダーによって拡張されたコントロールの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="e1c1f-179">リストの 1 の場合は、コントロール エクステンダーがテキスト ボックスの拡張に使用されます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="e1c1f-180">最後に、カスタム エクステンダーが MyProperty という名前のプロパティが含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="e1c1f-181">プロパティは、ExtenderControlProperty 属性でマークされます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="e1c1f-182">GetPropertyValue() と SetPropertyValue() メソッドを使用して、クライアント側の動作に、サーバー側コントロール エクステンダーのプロパティの値を渡します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="e1c1f-183">S、DisabledButton エクステンダーのコードを実装して使用できます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="e1c1f-184">このエクステンダーのコードは、リスト 2 で確認できます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="e1c1f-185">**2 - DisabledButtonExtender.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e1c1f-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="e1c1f-186">リスト 2 で DisabledButton エクステンダーには、TargetButtonID DisabledText という 2 つのプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="e1c1f-187">TargetButtonID プロパティに適用される IDReferenceProperty からボタン コントロールの ID 以外のものをこのプロパティに割り当てることができないようにします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="e1c1f-188">WebResource および ClientScriptResource 属性は、このエクステンダー DisabledButtonBehavior.js という名前のファイルにあるクライアント側の動作を関連付けます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="e1c1f-189">次のセクションでは、この JavaScript ファイルをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="e1c1f-190">カスタム エクステンダーの動作を作成します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="e1c1f-191">コントロール エクステンダーのクライアント側のコンポーネントには、動作は呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="e1c1f-192">DisabledButton 動作を無効にすると、ボタンを有効にすると、実際のロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="e1c1f-193">動作の JavaScript コードは、リスト 3 に含まれます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="e1c1f-194">**3 - DisabledButton.js を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e1c1f-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="e1c1f-195">リスト 3 の JavaScript ファイルには、DisabledButtonBehavior をという名前のクライアント側クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="e1c1f-196">TargetButtonID という名前の 2 つのプロパティが、サーバー側ツインのように、このクラスに含まれていてを使用してアクセスできる DisabledText 取得\_TargetButtonID/設定\_TargetButtonID 取得と\_DisabledText/設定\_DisabledText します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="e1c1f-197">Initialize() メソッドは、keyup イベント ハンドラーを動作のターゲット要素に関連付けます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="e1c1f-198">この動作に関連付けられているテキスト ボックスに文字を入力するたびに、keyup ハンドラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="e1c1f-199">Keyup ハンドラーが有効か、または動作に関連付けられているテキスト ボックスが任意のテキストを含むかどうかに応じて、ボタンを無効にします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="e1c1f-200">埋め込みリソースとしてリスト 3 の JavaScript ファイルをコンパイルする必要がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="e1c1f-201">ソリューション エクスプ ローラー ウィンドウで、ファイルを選択、プロパティ シートを開き、値を割り当てる*埋め込まれたリソース*を**ビルド アクション**プロパティ (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="e1c1f-202">このオプションは、Visual Studio と Visual Web Developer の両方で使用できます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="e1c1f-203">[![埋め込みリソースとしての JavaScript ファイルを追加します。](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="e1c1f-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="e1c1f-204">**図 03**: 埋め込みリソースとしての JavaScript ファイルを追加する ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="e1c1f-205">カスタム エクステンダー デザイナーの作成</span><span class="sxs-lookup"><span data-stu-id="e1c1f-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="e1c1f-206">Extender を完了するを作成する必要がある 1 つの最後のクラスがあります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="e1c1f-207">リスト 4 デザイナー クラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="e1c1f-208">このクラスは、Visual Studio または Visual Web Developer デザイナーで正しく動作するエクステンダーに必要です。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="e1c1f-209">**4 - DisabledButtonDesigner.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="e1c1f-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="e1c1f-210">デザイナーの属性を持つ DisabledButton エクステンダー リスト 4 デザイナーに関連付けます。このような DisabledButtonExtender クラスに Designer 属性を適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="e1c1f-211">カスタム エクステンダーを使用</span><span class="sxs-lookup"><span data-stu-id="e1c1f-211">Using the Custom Extender</span></span>

<span data-ttu-id="e1c1f-212">DisabledButton コントロール エクステンダーを作成することが完了したら、これで、ASP.NET web サイトで使用する時間を勧めします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="e1c1f-213">最初に、ツールボックスにカスタム エクステンダーを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="e1c1f-214">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-214">Follow these steps:</span></span>

1. <span data-ttu-id="e1c1f-215">ASP.NET ページを開くには、ソリューション エクスプ ローラー ウィンドウでページをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="e1c1f-216">ツールボックスを右クリックし、メニュー オプションを選択**アイテムの選択**します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="e1c1f-217">ツールボックス アイテムの選択 ダイアログ ボックスでは、CustomExtenders.dll アセンブリを参照します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="e1c1f-218">をクリックして、 **OK**ボタンをクリックしてダイアログを閉じます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="e1c1f-219">次の手順を完了すると、DisabledButton コントロール エクステンダーがツールボックスに表示する必要があります (図 4 参照)。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="e1c1f-220">[![ツールボックスで DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="e1c1f-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="e1c1f-221">**図 04**: ツールボックスに DisabledButton ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="e1c1f-222">次に、新しい ASP.NET ページを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="e1c1f-223">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-223">Follow these steps:</span></span>

1. <span data-ttu-id="e1c1f-224">ShowDisabledButton.aspx をという名前の新しい ASP.NET ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="e1c1f-225">ScriptManager をページにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="e1c1f-226">TextBox コントロールをページにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="e1c1f-227">ボタン コントロールをページにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="e1c1f-228">[プロパティ] ウィンドウでボタンの ID プロパティ値に変更<em>btnSave</em>とテキスト プロパティに値*保存\**。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="e1c1f-229">標準の ASP.NET の TextBox とボタン コントロールと、ページを作成しました。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="e1c1f-230">次に、DisabledButton エクステンダーの TextBox コントロールを拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="e1c1f-231">選択、 **Extender の追加**タスク オプション エクステンダー ウィザード ダイアログを開きます (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="e1c1f-232">ダイアログ ボックスに、カスタムの DisabledButton エクステンダーが含まれていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="e1c1f-233">DisabledButton エクステンダーを選択し、クリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="e1c1f-234">[![Extender ウィザード ダイアログ](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="e1c1f-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="e1c1f-235">**図 05**: エクステンダー ウィザードは、ダイアログ ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="e1c1f-236">最後に、DisabledButton エクステンダーのプロパティを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="e1c1f-237">DisabledButton エクステンダーのプロパティを変更するには、TextBox コントロールのプロパティを変更します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="e1c1f-238">デザイナーで、テキスト ボックスを選択します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="e1c1f-239">エクステンダー ノードを展開し、[プロパティ] ウィンドウ (図 6 参照)。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="e1c1f-240">値を割り当てる*保存*DisabledText プロパティと値を*btnSave* TargetButtonID プロパティにします。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="e1c1f-241">[![エクステンダー プロパティの設定](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="e1c1f-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="e1c1f-242">**図 06**: エクステンダー プロパティの設定 ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="e1c1f-243">(F5 キーを押して)、ページを実行すると、ボタン コントロールは最初に無効になります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="e1c1f-244">テキスト ボックスにテキストの入力を開始するとすぐに、コントロールがボタンには、(図 7 を参照) が有効になります。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="e1c1f-245">[![DisabledButton エクステンダーの操作](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="e1c1f-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="e1c1f-246">**図 07**: The DisabledButton エクステンダーの操作 ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="e1c1f-247">まとめ</span><span class="sxs-lookup"><span data-stu-id="e1c1f-247">Summary</span></span>

<span data-ttu-id="e1c1f-248">このチュートリアルの目的は、カスタム エクステンダー コントロールを AJAX Control Toolkit の拡張方法を説明することでした。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="e1c1f-249">このチュートリアルでは、単純な DisabledButton コントロール エクステンダーを作成しました。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="e1c1f-250">このエクステンダー DisabledButtonExtender クラス、DisabledButtonBehavior JavaScript 動作、および DisabledButtonDesigner クラスを作成して実装します。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="e1c1f-251">カスタム コントロール エクステンダーを作成するたびに、同様の一連の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="e1c1f-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e1c1f-252">[前へ](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [次へ](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e1c1f-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>
