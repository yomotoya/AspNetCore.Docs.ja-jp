---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'パート 6: モデル検証のためのデータ注釈の使用 |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 6 では、V のモデルのデータ注釈の使用について説明しています.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 10884f569f0f5d95517b73daab31fbd269a4726a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825954"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="41772-104">モデル検証のためのパート 6: を使用してデータ注釈</span><span class="sxs-lookup"><span data-stu-id="41772-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="41772-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="41772-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="41772-106">MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。</span><span class="sxs-lookup"><span data-stu-id="41772-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="41772-107">MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。</span><span class="sxs-lookup"><span data-stu-id="41772-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="41772-108">このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="41772-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="41772-109">パート 6 では、モデル検証のためのデータ注釈の使用について説明します。</span><span class="sxs-lookup"><span data-stu-id="41772-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="41772-110">Create と Edit のフォームの主要な問題がある: いずれかの検証を行わない。</span><span class="sxs-lookup"><span data-stu-id="41772-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="41772-111">価格フィールドに、型の数字または必要なフィールドを空白のままにするなどの処理を行ってし、データベースが最初のエラーを説明します。</span><span class="sxs-lookup"><span data-stu-id="41772-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="41772-112">データ注釈をモデル クラスに追加することでアプリケーションに検証を簡単に追加できます。</span><span class="sxs-lookup"><span data-stu-id="41772-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="41772-113">データ注釈を使用すると、必要、モデルのプロパティに適用される規則を詳しく説明して、ASP.NET MVC が使用を強制して、ユーザーに適切なメッセージを表示するが取得されます。</span><span class="sxs-lookup"><span data-stu-id="41772-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="41772-114">アルバムのフォーム検証の追加</span><span class="sxs-lookup"><span data-stu-id="41772-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="41772-115">次のデータ注釈属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="41772-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="41772-116">**必要な**– プロパティが必須のフィールドであることを示します。</span><span class="sxs-lookup"><span data-stu-id="41772-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="41772-117">**DisplayName** – フォーム フィールドと検証メッセージで使用される対象のテキストを定義します。</span><span class="sxs-lookup"><span data-stu-id="41772-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="41772-118">**StringLength** – 文字列フィールドの最大長を定義します。</span><span class="sxs-lookup"><span data-stu-id="41772-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="41772-119">**範囲**– 数値フィールドの最大値と最小値は、</span><span class="sxs-lookup"><span data-stu-id="41772-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="41772-120">**バインド**– を除外または含めるパラメーターまたはフォームの値をモデルのプロパティにバインドするときにフィールドを一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="41772-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="41772-121">**ScaffoldColumn** – エディターのフォームのフィールドを非表示を許可</span><span class="sxs-lookup"><span data-stu-id="41772-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="41772-122">*注: データ注釈属性を使用してモデルの検証の詳細については、MSDN ドキュメントを参照します。*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="41772-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="41772-123">アルバム クラスを開き、次の追加*を使用して*ステートメントを先頭にします。</span><span class="sxs-lookup"><span data-stu-id="41772-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="41772-124">次に、次に示すように、表示と検証の属性を追加するプロパティを更新します。</span><span class="sxs-lookup"><span data-stu-id="41772-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="41772-125">調査中にもに変更されましたジャンル、アーティスト仮想プロパティ。</span><span class="sxs-lookup"><span data-stu-id="41772-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="41772-126">これにより、遅延読み込みに必要に応じて、Entity Framework です。</span><span class="sxs-lookup"><span data-stu-id="41772-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="41772-127">アルバム、モデルにこれらの属性を追加すること後、は、フィールドの検証、作成および編集画面がすぐに開始され、表示名を使用して (例: アルバム アートの Url AlbumArtUrl ではなく) を選択しました。</span><span class="sxs-lookup"><span data-stu-id="41772-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="41772-128">アプリケーションを実行し、/StoreManager/Create を参照します。</span><span class="sxs-lookup"><span data-stu-id="41772-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="41772-129">次に、いくつかの検証ルールを破るします。</span><span class="sxs-lookup"><span data-stu-id="41772-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="41772-130">0 の価格を入力し、タイトルを空白のままにします。</span><span class="sxs-lookup"><span data-stu-id="41772-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="41772-131">[作成] ボタンをクリックしたときの検証エラーを示すメッセージ フィールド検証規則を満たしていない定義したと表示されるフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="41772-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="41772-132">クライアント側検証のテスト</span><span class="sxs-lookup"><span data-stu-id="41772-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="41772-133">サーバー側検証は、ユーザーはクライアント側の検証を回避できるため、アプリケーションの観点から非常に重要です。</span><span class="sxs-lookup"><span data-stu-id="41772-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="41772-134">ただし、web ページ フォームのみサーバー側の検証を実装するには、次の 3 つの重要な問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="41772-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="41772-135">ユーザーは、投稿、サーバーで、検証するフォームと、ブラウザーに送信される応答を待機します。</span><span class="sxs-lookup"><span data-stu-id="41772-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="41772-136">今すぐ、検証規則を通過するようにフィールドを修正するときに、ユーザーは迅速なフィードバックを取得しません。</span><span class="sxs-lookup"><span data-stu-id="41772-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="41772-137">ユーザーのブラウザーを利用する代わりに検証ロジックを実行するサーバーのリソースを無駄にしています。</span><span class="sxs-lookup"><span data-stu-id="41772-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="41772-138">さいわい、ASP.NET MVC 3 のスキャフォールディング テンプレートは一切追加の作業を必要としない組み込まれており、クライアント側検証があります。</span><span class="sxs-lookup"><span data-stu-id="41772-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="41772-139">検証メッセージがすぐに削除するため、検証の要件を満たすタイトル フィールドに 1 つの文字を入力します。</span><span class="sxs-lookup"><span data-stu-id="41772-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="41772-140">[前へ](mvc-music-store-part-5.md)
> [次へ](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="41772-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
