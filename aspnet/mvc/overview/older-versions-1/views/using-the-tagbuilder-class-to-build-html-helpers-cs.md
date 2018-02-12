---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: "HTML ヘルパー (c#) をビルドする TagBuilder クラスを使用して |Microsoft ドキュメント"
author: StephenWalther
description: "Stephen Walther には、名前付き TagBuilder クラス、ASP.NET MVC フレームワークで便利なユーティリティ クラスをについて説明します。 クラスを使用して、TagBuilder を簡単にしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 63f07c3f95c520dbc74f3568aa65dc6a6f34a901
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="9cc27-104">HTML ヘルパー (c#) をビルドするのに TagBuilder クラスの使用</span><span class="sxs-lookup"><span data-stu-id="9cc27-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="9cc27-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="9cc27-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="9cc27-106">Stephen Walther には、名前付き TagBuilder クラス、ASP.NET MVC フレームワークで便利なユーティリティ クラスをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="9cc27-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="9cc27-107">HTML タグを簡単にビルドするのにには、TagBuilder クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="9cc27-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="9cc27-108">ASP.NET MVC フレームワークには、名前付き TagBuilder クラス HTML ヘルパーを構築するときに使用できる便利なユーティリティ クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9cc27-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="9cc27-109">TagBuilder クラス、クラスの名前からわかるように、使用する HTML タグを簡単に構築できます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="9cc27-110">この簡単なチュートリアルでは提供 TagBuilder クラスの概要の説明と HTML を表示する単純な HTML ヘルパーを構築するときに、このクラスを使用する方法について学びます&lt;img&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="9cc27-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="9cc27-111">TagBuilder クラスの概要</span><span class="sxs-lookup"><span data-stu-id="9cc27-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="9cc27-112">TagBuilder クラスは、System.Web.Mvc 名前空間に含まれています。</span><span class="sxs-lookup"><span data-stu-id="9cc27-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="9cc27-113">5 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="9cc27-113">It has five methods:</span></span>

- <span data-ttu-id="9cc27-114">AddCssClass() - を使用すると、新しい*クラス =""*属性をタグにします。</span><span class="sxs-lookup"><span data-stu-id="9cc27-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="9cc27-115">GenerateId() - には、id 属性をタグに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="9cc27-116">このメソッドは、id でピリオドを自動的に置き換えられます (既定では、ピリオド、アンダー スコアに置き換え)</span><span class="sxs-lookup"><span data-stu-id="9cc27-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="9cc27-117">MergeAttribute() - には、タグに属性を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="9cc27-118">このメソッドの複数のオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="9cc27-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="9cc27-119">SetInnerText() - には、タグの内部テキ ストを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="9cc27-120">その内部テキ ストは、HTML が自動的にエンコードします。</span><span class="sxs-lookup"><span data-stu-id="9cc27-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="9cc27-121">ToString() - には、タグをレンダリングすることができます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="9cc27-122">通常、タグ、開始タグ、終了タグ、または自己終了タグを作成するかどうかを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="9cc27-123">TagBuilder クラスには、次の 4 つの重要なプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="9cc27-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="9cc27-124">[属性]-すべてのタグの属性を表します。</span><span class="sxs-lookup"><span data-stu-id="9cc27-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="9cc27-125">IdAttributeDotReplacement - は、期間 (既定では、アンダー スコア) を置き換える GenerateId() メソッドで使用される文字を表します。</span><span class="sxs-lookup"><span data-stu-id="9cc27-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="9cc27-126">InnerHTML のでは、タグの内部コンテンツを表します。</span><span class="sxs-lookup"><span data-stu-id="9cc27-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="9cc27-127">このプロパティに指定できる文字列*しない*HTML 文字列をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="9cc27-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="9cc27-128">タグ名には、タグの名前を表します。</span><span class="sxs-lookup"><span data-stu-id="9cc27-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="9cc27-129">これらのメソッドとプロパティをすべて提供、基本的なするメソッドとプロパティに HTML タグを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9cc27-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="9cc27-130">本当に TagBuilder クラスを使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9cc27-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="9cc27-131">StringBuilder クラスは、代わりに使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9cc27-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="9cc27-132">ただし、TagBuilder クラス容易に少しです。</span><span class="sxs-lookup"><span data-stu-id="9cc27-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="9cc27-133">イメージ HTML ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="9cc27-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="9cc27-134">TagBuilder クラスのインスタンスを作成するときは、TagBuilder コンス トラクターにビルドするタグの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="9cc27-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="9cc27-135">次に、タグの属性を変更する AddCssClass および MergeAttribute() メソッドなどのメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="9cc27-136">最後に、タグを表示するために ToString() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="9cc27-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="9cc27-137">たとえば、1 一覧表示するにはには、イメージの HTML ヘルパーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="9cc27-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="9cc27-138">イメージ ヘルパーは、HTML を表す TagBuilder で内部的に実装される&lt;img&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="9cc27-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="9cc27-139">**1 - Helpers\ImageHelper.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="9cc27-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="9cc27-140">1 のリスト内のクラスには、2 つの静的なオーバー ロードされたメソッドがイメージをという名前が含まれています。</span><span class="sxs-lookup"><span data-stu-id="9cc27-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="9cc27-141">Image() メソッドを呼び出すときに、か HTML 属性のセットを表すオブジェクトを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="9cc27-142">TagBuilder.MergeAttribute() メソッドを使用、TagBuilder に src 属性などの個々 の属性を追加する方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="9cc27-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="9cc27-143">注意してください、さらに追加する属性のコレクション、TagBuilder TagBuilder.MergeAttributes() メソッドを使用する方法です。</span><span class="sxs-lookup"><span data-stu-id="9cc27-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="9cc27-144">MergeAttributes() メソッドは、ディクショナリを受け取ります&lt;文字列, オブジェクト&gt;パラメーター。</span><span class="sxs-lookup"><span data-stu-id="9cc27-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="9cc27-145">ディクショナリに属性のコレクションを表すオブジェクトを変換する RouteValueDictionary クラスが使用される&lt;文字列, オブジェクト&gt;です。</span><span class="sxs-lookup"><span data-stu-id="9cc27-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="9cc27-146">イメージ ヘルパーを作成した後は、その他の標準の HTML ヘルパーのいずれかのように、ASP.NET MVC ビューで、ヘルパーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="9cc27-147">ビューを一覧表示する 2 で、2 回 Xbox の同じイメージを表示するイメージ ヘルパーを使用して (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="9cc27-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="9cc27-148">Image() ヘルパーは、両方および HTML 属性のコレクションを使用せずに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9cc27-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="9cc27-149">**2 - Home\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="9cc27-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="9cc27-150">[![[新しいプロジェクト] ダイアログ ボックス](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9cc27-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="9cc27-151">**図 01**: イメージ ヘルパーの使用 ([フルサイズのイメージを表示するをクリックして](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9cc27-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="9cc27-152">Index.aspx ビューの上部にあるイメージ ヘルパーに関連付けられている名前空間をインポートする必要がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9cc27-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="9cc27-153">ヘルパーは、次のディレクティブを使用してインポートは。</span><span class="sxs-lookup"><span data-stu-id="9cc27-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

>[!div class="step-by-step"]
<span data-ttu-id="9cc27-154">[前へ](creating-custom-html-helpers-cs.md)
[次へ](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9cc27-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
