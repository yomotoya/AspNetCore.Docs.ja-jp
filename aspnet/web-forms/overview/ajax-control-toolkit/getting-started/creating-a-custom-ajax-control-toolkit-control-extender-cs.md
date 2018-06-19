---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Toolkit コントロール エクステンダー (c#) を制御するカスタムの AJAX の作成 |Microsoft ドキュメント
author: microsoft
description: カスタムのエクステンダーを使用すると、カスタマイズし、新しいクラスを作成することがなく ASP.NET コントロールの機能を拡張できます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: dc058d1d19df880109352caf2dc7d1860121a104
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875917"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="1c8c1-103">カスタムの AJAX コントロール Toolkit コントロール エクステンダー (c#) を作成します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>
====================
<span data-ttu-id="1c8c1-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1c8c1-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1c8c1-105">カスタムのエクステンダーを使用すると、カスタマイズし、新しいクラスを作成することがなく ASP.NET コントロールの機能を拡張できます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="1c8c1-106">このチュートリアルでは、カスタムの AJAX コントロール Toolkit コントロール エクステンダーを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="1c8c1-107">無効から有効にテキスト ボックスにテキストを入力すると、ボタンの状態を変更する便利で新しい extender は、単純なを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="1c8c1-108">このチュートリアルを読むと、後に、独自のコントロール エクステンダーを使用して ASP.NET AJAX ツールキットを拡張することができます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="1c8c1-109">Visual Studio または Visual Web Developer のいずれかを使用してカスタム コントロール エクステンダーを作成することができます (Visual Web Developer の最新バージョンがあることを確認してください)。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="1c8c1-110">DisabledButton エクステンダーの概要</span><span class="sxs-lookup"><span data-stu-id="1c8c1-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="1c8c1-111">DisabledButton エクステンダーの名前は、新しいコントロール エクステンダーです。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="1c8c1-112">このエクステンダーは、3 つのプロパティが適用されます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-112">This extender will have three properties:</span></span>

- <span data-ttu-id="1c8c1-113">TargetControlID - テキスト ボックス コントロールを拡張します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="1c8c1-114">TargetButtonIID - が無効または有効になっているボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="1c8c1-115">DisabledText - ボタンで最初に表示されるテキストです。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="1c8c1-116">入力を開始するときに、ボタンは、ボタンのテキスト プロパティの値を表示します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="1c8c1-117">テキスト ボックスとボタン コントロールに DisabledButton extender をフックするとします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="1c8c1-118">任意のテキストを入力する前に、ボタンが無効にし、テキスト ボックスとボタンを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="1c8c1-119">([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1c8c1-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="1c8c1-120">テキストの入力を開始した後、ボタンが有効になっているし、テキスト ボックスとボタンを次に示します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="1c8c1-121">([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1c8c1-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="1c8c1-122">当社コントロール エクステンダーを作成するには、次の 3 つのファイルを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="1c8c1-123">DisabledButtonExtender.cs - このファイルは、サーバー側コントロール クラスを extender の作成を管理し、デザイン時にプロパティを設定することにします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="1c8c1-124">Extender に設定できるプロパティも定義します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="1c8c1-125">これらのプロパティは、デザイン時にコードを使用してアクセスできる DisableButtonBehavior.js ファイルで定義されたプロパティと一致します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="1c8c1-126">DisabledButtonBehavior.js--このファイルは、すべてのクライアント スクリプトのロジックが追加されます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="1c8c1-127">DisabledButtonDesigner.cs - このクラスは、デザイン時の機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="1c8c1-128">Visual Studio または Visual Web Developer デザイナーで正しく機能するコントロール エクステンダーをする場合は、このクラスを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="1c8c1-129">したがってコントロール エクステンダーは、サーバー側コントロール、クライアント側の動作、およびサーバー側デザイナー クラスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="1c8c1-130">次のセクションでこれらのファイルの 3 つすべてを作成する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="1c8c1-131">カスタム Extender の web サイトおよびプロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="1c8c1-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="1c8c1-132">最初の手順では、Visual Studio または Visual Web Developer で、クラス ライブラリ プロジェクトと web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="1c8c1-133">お ll、クラス ライブラリ プロジェクトでカスタムのエクステンダーを作成し、web サイトでカスタムのエクステンダーをテストします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="1c8c1-134">S、web サイトを開始できるようにします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-134">Let�s start with the website.</span></span> <span data-ttu-id="1c8c1-135">Web サイトを作成する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="1c8c1-136">メニュー オプションを選択**ファイル]、[新しい Web サイト**です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="1c8c1-137">選択、 **ASP.NET Web サイト**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="1c8c1-138">新しい web サイトの名前を付けます*Website1*です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="1c8c1-139">クリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-139">Click the **OK** button.</span></span>

<span data-ttu-id="1c8c1-140">次に、コントロール エクステンダーのコードを含むクラス ライブラリ プロジェクトを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="1c8c1-141">メニュー オプションを選択**ファイルを追加、新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="1c8c1-142">選択、**クラス ライブラリ**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="1c8c1-143">名前を持つ、新しいクラス ライブラリの名前を付けます**CustomExtenders**です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="1c8c1-144">クリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-144">Click the **OK** button.</span></span>

<span data-ttu-id="1c8c1-145">これらの手順を実行した後、ソリューション エクスプ ローラー ウィンドウは図 1 のようになります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="1c8c1-146">[![Web サイトとクラス ライブラリ プロジェクトとソリューション](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="1c8c1-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="1c8c1-147">**図 01**: web サイトとクラス ライブラリ プロジェクトとソリューション ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="1c8c1-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="1c8c1-148">次に、すべてのクラス ライブラリ プロジェクトに必要なアセンブリ参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="1c8c1-149">CustomExtenders プロジェクトを右クリックし、メニュー オプションを選択**参照の追加**です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="1c8c1-150">[.NET] タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="1c8c1-151">次のアセンブリへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="1c8c1-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="1c8c1-152">System.Web.dll</span></span>
    2. <span data-ttu-id="1c8c1-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="1c8c1-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="1c8c1-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="1c8c1-154">System.Design.dll</span></span>
    4. <span data-ttu-id="1c8c1-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="1c8c1-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="1c8c1-156">[参照] タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="1c8c1-157">AjaxControlToolkit.dll アセンブリへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="1c8c1-158">このアセンブリは、AJAX コントロール ツールキットをダウンロードしたフォルダーにあります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="1c8c1-159">次の手順を完了すると、クラス ライブラリ プロジェクト参照フォルダーは図 2 のようになります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="1c8c1-160">[![必要な参照の参照 フォルダー](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="1c8c1-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="1c8c1-161">**図 02**: 必要な参照の参照 フォルダー ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="1c8c1-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="1c8c1-162">カスタム コントロール エクステンダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="1c8c1-163">ようになりましたが、クラス ライブラリ、当社エクステンダー コントロールのビルドが始めることができます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="1c8c1-164">S、骨のカスタム エクステンダー コントロール クラス (1 のリストを参照) で始まることができます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="1c8c1-165">**1 - MyCustomExtender.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1c8c1-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="1c8c1-166">クラスの概要、コントロール エクステンダーを一覧表示する 1 ことを確認するいくつかの点があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="1c8c1-167">最初に、クラスが基本 ExtenderControlBase クラスから継承されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="1c8c1-168">すべての AJAX コントロール Toolkit エクステンダー コントロールは、この基本クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="1c8c1-169">たとえば、基本クラスには、各コントロール エクステンダーの必須プロパティである TargetID プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="1c8c1-170">次に、クラスがクライアント スクリプトに関連する次の 2 つの属性が含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="1c8c1-171">WebResource - には、アセンブリ内の埋め込みリソースとして含まれるファイルが原因です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="1c8c1-172">ClientScriptResource - には、アセンブリから取得するスクリプト リソースが原因です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="1c8c1-173">WebResource 属性を使用して、カスタムの extender がコンパイルされるときに、MyControlBehavior.js JavaScript ファイルをアセンブリに埋め込みます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="1c8c1-174">ClientScriptResource 属性を使用して、web ページで、カスタムの extender を使用すると、アセンブリから MyControlBehavior.js スクリプトを取得します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="1c8c1-175">WebResource と ClientScriptResource 属性動作するためには、埋め込みリソースとして JavaScript ファイルをコンパイルする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="1c8c1-176">ソリューション エクスプ ローラー ウィンドウでファイルを選択し、プロパティ シートを開き、値を割り当てる*埋め込まれたリソース*を**ビルド アクション**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="1c8c1-177">コントロールの extender も TargetControlType 属性が含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="1c8c1-178">この属性を使用して、コントロール エクステンダーによって拡張されたコントロールの種類を指定します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="1c8c1-179">リストの 1 の場合は、コントロール エクステンダーを使用して、テキスト ボックスを拡張します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="1c8c1-180">最後に、カスタムの extender に MyProperty というプロパティが含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="1c8c1-181">プロパティは、ExtenderControlProperty 属性でマークされています。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="1c8c1-182">クライアント側の動作にサーバー側コントロール エクステンダーからプロパティ値を渡す、GetPropertyValue() および SetPropertyValue() メソッドが使用されます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="1c8c1-183">S さあ、当社 DisabledButton エクステンダーのコードを実装することができます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="1c8c1-184">このエクステンダーのコードは、一覧表示する 2 で確認できます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="1c8c1-185">**2 - DisabledButtonExtender.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1c8c1-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="1c8c1-186">DisabledButton エクステンダーを一覧表示する 2 は TargetButtonID および DisabledText という 2 つのプロパティを持ちます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="1c8c1-187">TargetButtonID プロパティに適用される IDReferenceProperty では、このプロパティに割り当てることのボタン コントロールの ID 以外のものをできません。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="1c8c1-188">WebResource と ClientScriptResource 属性では、このエクステンダー DisabledButtonBehavior.js をという名前のファイルにあるクライアント側の動作を関連付けます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="1c8c1-189">次のセクションでは、この JavaScript ファイルをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="1c8c1-190">Extender のカスタム動作を作成します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="1c8c1-191">コントロール エクステンダーのクライアント側のコンポーネントには、動作は呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="1c8c1-192">DisabledButton 動作に、無効にして、ボタンを有効化の実際のロジックが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="1c8c1-193">リスト 3 の動作では、JavaScript コードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="1c8c1-194">**3 - DisabledButton.js を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1c8c1-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="1c8c1-195">リスト 3 での JavaScript ファイルには、クライアント側のクラスが DisabledButtonBehavior をという名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="1c8c1-196">TargetButtonID をという名前の 2 つのプロパティが、サーバー側ツインと同様に、このクラスに含まれていて、を使用してアクセスできる DisabledText\_TargetButtonID/設定\_TargetButtonID 取得および\_DisabledText/設定\_DisabledText です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="1c8c1-197">Initialize() メソッドは、動作のターゲット要素の keyup イベント ハンドラーを関連付けます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="1c8c1-198">この動作に関連付けられているテキスト ボックスに、文字を入力するたびに、keyup ハンドラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="1c8c1-199">Keyup ハンドラーが有効か、または動作に関連付けられているテキスト ボックスが任意のテキストが含まれるかどうかに応じて、ボタンを無効にします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="1c8c1-200">埋め込みリソースとして一覧表示する 3 での JavaScript ファイルをコンパイルする必要がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="1c8c1-201">ソリューション エクスプ ローラー ウィンドウでファイルを選択し、プロパティ シートを開き、値を割り当てる*埋め込まれたリソース*を**ビルド アクション**プロパティ (図 3 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="1c8c1-202">このオプションは、Visual Studio と Visual Web Developer の両方で使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="1c8c1-203">[![埋め込みリソースとしての JavaScript ファイルを追加します。](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="1c8c1-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="1c8c1-204">**図 03**: 埋め込みリソースとしての JavaScript ファイルを追加する ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="1c8c1-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="1c8c1-205">Extender のカスタム デザイナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="1c8c1-206">作成、extender を完了に必要な最後クラスは 1 つです。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="1c8c1-207">デザイナー クラスを一覧表示する 4 で作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="1c8c1-208">Visual Studio または Visual Web Developer デザイナーで正しく動作拡張を行うには、このクラスが必要です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="1c8c1-209">**4 - DisabledButtonDesigner.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1c8c1-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="1c8c1-210">デザイナーを一覧表示する 4 では、デザイナーの属性を持つ DisabledButton extender に関連付けます。次のように DisabledButtonExtender クラスに Designer 属性を適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="1c8c1-211">カスタムの Extender の使用</span><span class="sxs-lookup"><span data-stu-id="1c8c1-211">Using the Custom Extender</span></span>

<span data-ttu-id="1c8c1-212">DisabledButton コントロール エクステンダーの作成を完了しましたが、これで、ASP.NET web サイトで使用する時間を勧めします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="1c8c1-213">最初に、ツールボックスにカスタムのエクステンダーを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="1c8c1-214">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-214">Follow these steps:</span></span>

1. <span data-ttu-id="1c8c1-215">ソリューション エクスプ ローラー ウィンドウでページをダブルクリックして、ASP.NET ページを開きます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="1c8c1-216">ツールボックスを右クリックし、メニュー オプションを選択**アイテムの選択**です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="1c8c1-217">ツールボックス アイテムの選択 ダイアログ ボックスで、CustomExtenders.dll アセンブリを参照します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="1c8c1-218">をクリックして、 **OK**ダイアログを閉じるボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="1c8c1-219">次の手順を完了すると、DisabledButton コントロール エクステンダーがツールボックスに表示する必要があります (図 4 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="1c8c1-220">[![ツールボックスで DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="1c8c1-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="1c8c1-221">**図 04**: ツールボックスに DisabledButton ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="1c8c1-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="1c8c1-222">次に、新しい ASP.NET ページを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="1c8c1-223">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-223">Follow these steps:</span></span>

1. <span data-ttu-id="1c8c1-224">ShowDisabledButton.aspx をという名前の新しい ASP.NET ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="1c8c1-225">ScriptManager をページにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="1c8c1-226">テキスト ボックス コントロールをページにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="1c8c1-227">ボタン コントロールをページにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="1c8c1-228">[プロパティ] ウィンドウでボタン ID プロパティ値を変更<em>btnSave</em>し、テキストのプロパティ値を*保存\** です。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="1c8c1-229">標準の ASP.NET のテキスト ボックスとボタン コントロールと、ページを作成しました。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="1c8c1-230">次に、DisabledButton エクステンダーのある TextBox コントロールを拡張する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="1c8c1-231">選択、 **Extender の追加**Extender ウィザード ダイアログを開くにはオプションのタスク (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="1c8c1-232">ダイアログ ボックスに、カスタムの DisabledButton extender が含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="1c8c1-233">DisabledButton extender を選択し、クリックして、 **OK**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="1c8c1-234">[![Extender ウィザード ダイアログ](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="1c8c1-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="1c8c1-235">**図 05**: Extender ウィザードは、ダイアログ ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="1c8c1-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="1c8c1-236">最後に、DisabledButton extender のプロパティを設定できます。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="1c8c1-237">DisabledButton extender のプロパティを変更するには、テキスト ボックス コントロールのプロパティを変更します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="1c8c1-238">デザイナーで、テキスト ボックスを選択します。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="1c8c1-239">[プロパティ] ウィンドウでノードを展開しますエクステンダー (図 6 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="1c8c1-240">値を割り当てる*保存*DisabledText プロパティと値を*btnSave* TargetButtonID プロパティにします。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="1c8c1-241">[![エクステンダー プロパティを設定](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="1c8c1-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="1c8c1-242">**図 06**: エクステンダー プロパティを設定 ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="1c8c1-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="1c8c1-243">(F5 キーを押す) によって、ページを実行すると、ボタン コントロールは、最初に無効になります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="1c8c1-244">テキスト ボックスにテキストの入力を開始するとすぐに、コントロールは、ボタンには、(図 7 を参照) が有効になります。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="1c8c1-245">[![アクションで DisabledButton エクステンダー](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="1c8c1-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="1c8c1-246">**図 07**: アクションで、DisabledButton エクステンダー ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="1c8c1-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="1c8c1-247">まとめ</span><span class="sxs-lookup"><span data-stu-id="1c8c1-247">Summary</span></span>

<span data-ttu-id="1c8c1-248">このチュートリアルの目的は、カスタムのエクステンダー コントロールの AJAX コントロール ツールキットを拡張する方法を説明することでした。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="1c8c1-249">このチュートリアルでは、単純な DisabledButton コントロール エクステンダーを作成しました。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="1c8c1-250">DisabledButtonExtender クラス、DisabledButtonBehavior JavaScript 動作、および DisabledButtonDesigner クラスを作成することでこのエクステンダーを実装しました。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="1c8c1-251">カスタム コントロール エクステンダーを作成するときに類似した一連の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="1c8c1-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1c8c1-252">[前へ](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [次へ](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1c8c1-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>
