---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: TagBuilder クラスを使用して、HTML ヘルパー (c#) をビルドする |Microsoft Docs
author: StephenWalther
description: Stephen Walther TagBuilder クラスという名前の ASP.NET MVC フレームワークで便利なユーティリティ クラスを紹介します。 TagBuilder クラスを簡単に使用できます.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9990fc7ad8093643a564a5e02ff65264d4a7fe15
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813227"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="becf9-104">TagBuilder クラスを使用して、HTML ヘルパー (c#) をビルドするには</span><span class="sxs-lookup"><span data-stu-id="becf9-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="becf9-105">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="becf9-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="becf9-106">Stephen Walther TagBuilder クラスという名前の ASP.NET MVC フレームワークで便利なユーティリティ クラスを紹介します。</span><span class="sxs-lookup"><span data-stu-id="becf9-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="becf9-107">TagBuilder クラスを使用すると、HTML タグを簡単に構築します。</span><span class="sxs-lookup"><span data-stu-id="becf9-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="becf9-108">ASP.NET MVC フレームワークには、TagBuilder クラスで HTML ヘルパーを作成するときに使用できるという便利なユーティリティ クラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="becf9-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="becf9-109">TagBuilder クラスでは、クラスの名前からわかるように、使用する HTML タグを簡単に構築できます。</span><span class="sxs-lookup"><span data-stu-id="becf9-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="becf9-110">この簡単なチュートリアルでは、TagBuilder クラスの概要が提供され、HTML を表示する単純な HTML ヘルパーをビルドするときに、このクラスを使用する方法を学習します&lt;img&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="becf9-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="becf9-111">TagBuilder クラスの概要</span><span class="sxs-lookup"><span data-stu-id="becf9-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="becf9-112">TagBuilder クラスは、System.Web.Mvc 名前空間に含まれます。</span><span class="sxs-lookup"><span data-stu-id="becf9-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="becf9-113">5 つのメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="becf9-113">It has five methods:</span></span>

- <span data-ttu-id="becf9-114">新しいを追加することができます - AddCssClass()*クラス =""* タグに属性します。</span><span class="sxs-lookup"><span data-stu-id="becf9-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="becf9-115">GenerateId() - には、id 属性、タグを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="becf9-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="becf9-116">このメソッドは id の期間を自動的に置き換えられます (既定では、ピリオド、アンダー スコアに置換)</span><span class="sxs-lookup"><span data-stu-id="becf9-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="becf9-117">MergeAttribute() - には、タグに属性を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="becf9-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="becf9-118">このメソッドの複数のオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="becf9-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="becf9-119">SetInnerText() - には、タグの内部テキ ストを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="becf9-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="becf9-120">内部テキ ストは、HTML が自動的にエンコードします。</span><span class="sxs-lookup"><span data-stu-id="becf9-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="becf9-121">ToString() のでは、タグをレンダリングすることができます。</span><span class="sxs-lookup"><span data-stu-id="becf9-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="becf9-122">通常のタグ、開始タグ、終了タグ、または自己終了タグを作成するかどうかを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="becf9-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="becf9-123">TagBuilder クラスでは、次の 4 つの重要なプロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="becf9-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="becf9-124">[属性]-すべてのタグの属性を表します。</span><span class="sxs-lookup"><span data-stu-id="becf9-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="becf9-125">IdAttributeDotReplacement - は、期間 (既定ではアンダー スコア) を置換する GenerateId() メソッドによって使用される文字を表します。</span><span class="sxs-lookup"><span data-stu-id="becf9-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="becf9-126">InnerHTML には、タグの内部の内容を表します。</span><span class="sxs-lookup"><span data-stu-id="becf9-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="becf9-127">このプロパティに文字列を割り当てる*しない*HTML 文字列をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="becf9-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="becf9-128">タグ名には、タグの名前を表します。</span><span class="sxs-lookup"><span data-stu-id="becf9-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="becf9-129">これらのメソッドとプロパティをすべて提供基本メソッドとプロパティを HTML タグを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="becf9-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="becf9-130">本当に TagBuilder クラスを使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="becf9-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="becf9-131">StringBuilder クラスは、代わりに使用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="becf9-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="becf9-132">ただし、TagBuilder クラスで、容易に少し。</span><span class="sxs-lookup"><span data-stu-id="becf9-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="becf9-133">イメージの HTML ヘルパーの作成</span><span class="sxs-lookup"><span data-stu-id="becf9-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="becf9-134">TagBuilder クラスのインスタンスを作成するときは、TagBuilder コンス トラクターにビルドするタグの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="becf9-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="becf9-135">次に、タグの属性を変更する AddCssClass と MergeAttribute() メソッドと同様のメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="becf9-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="becf9-136">最後に、タグを表示するために、ToString() メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="becf9-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="becf9-137">たとえば、リスト 1 には、イメージの HTML ヘルパーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="becf9-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="becf9-138">イメージ ヘルパーは HTML を表す TagBuilder で内部的に実装&lt;img&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="becf9-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="becf9-139">**1 - Helpers\ImageHelper.cs を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="becf9-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="becf9-140">リスト 1 でクラスには、2 つの静的なオーバー ロードされたメソッドという名前のイメージにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="becf9-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="becf9-141">Image() メソッドを呼び出すときにをかどうかは、一連の HTML 属性を表すオブジェクトを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="becf9-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="becf9-142">TagBuilder.MergeAttribute() メソッドを使用して、TagBuilder に src 属性などの個々 の属性を追加する方法に注意してください。</span><span class="sxs-lookup"><span data-stu-id="becf9-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="becf9-143">確認、さらに、TagBuilder.MergeAttributes() メソッドを使用して、TagBuilder に属性のコレクションを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="becf9-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="becf9-144">MergeAttributes() メソッドは、辞書を受け取ります&lt;object&gt;パラメーター。</span><span class="sxs-lookup"><span data-stu-id="becf9-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="becf9-145">ディクショナリに属性のコレクションを表すオブジェクトを変換する RouteValueDictionary クラスが使用される&lt;object&gt;します。</span><span class="sxs-lookup"><span data-stu-id="becf9-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="becf9-146">イメージ ヘルパーを作成した後は、その他の標準の HTML ヘルパーのいずれかのように、ASP.NET MVC ビューで、ヘルパーを使用できます。</span><span class="sxs-lookup"><span data-stu-id="becf9-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="becf9-147">リスト 2 での表示では、イメージ ヘルパーを使用して、2 回、Xbox の同じイメージを表示 (図 1 参照)。</span><span class="sxs-lookup"><span data-stu-id="becf9-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="becf9-148">HTML 属性のコレクションの有無にかかわらず、Image() ヘルパーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="becf9-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="becf9-149">**2 - Home\Index.aspx を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="becf9-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="becf9-150">[![[新しいプロジェクト] ダイアログ ボックス](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="becf9-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="becf9-151">**図 01**: イメージ ヘルパーを使用して ([フルサイズの画像を表示する をクリックします](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="becf9-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="becf9-152">Index.aspx ビューの上部にあるイメージ ヘルパーに関連付けられている名前空間をインポートする必要がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="becf9-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="becf9-153">次のディレクティブを使用して、ヘルパーがインポートされます。</span><span class="sxs-lookup"><span data-stu-id="becf9-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> <span data-ttu-id="becf9-154">[前へ](creating-custom-html-helpers-cs.md)
> [次へ](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="becf9-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
