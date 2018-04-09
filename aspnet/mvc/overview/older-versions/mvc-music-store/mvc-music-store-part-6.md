---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'パート 6: データ注釈を使用してモデルの検証の |Microsoft ドキュメント'
author: jongalloway
description: このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 6 部では、モデル V のデータの注釈の使用方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="74f46-104">モデル検証のためのパート 6: を使用してデータ注釈</span><span class="sxs-lookup"><span data-stu-id="74f46-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="74f46-105">によって[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="74f46-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="74f46-106">MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="74f46-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="74f46-107">MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。</span><span class="sxs-lookup"><span data-stu-id="74f46-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="74f46-108">このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="74f46-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="74f46-109">第 6 部では、モデル検証のためのデータ注釈の使用方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="74f46-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="74f46-110">作成および編集フォームの主要な問題があるお: 検証は一切行っていません。</span><span class="sxs-lookup"><span data-stu-id="74f46-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="74f46-111">価格 フィールドに必要なフィールドを空白または型の文字のままにするなどの処理を行って、後ほどお見せ最初エラーは、データベースからです。</span><span class="sxs-lookup"><span data-stu-id="74f46-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="74f46-112">モデル クラスをデータ注釈を追加することで、アプリケーションに検証簡単に追加できます。</span><span class="sxs-lookup"><span data-stu-id="74f46-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="74f46-113">データ注釈を使用するか、モデルのプロパティに適用される規則について説明することと、ASP.NET MVC の使用を強制して、ユーザーに適切なメッセージを表示するように注意はします。</span><span class="sxs-lookup"><span data-stu-id="74f46-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="74f46-114">アルバム フォームへの検証の追加</span><span class="sxs-lookup"><span data-stu-id="74f46-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="74f46-115">次のデータの注釈属性は使用します。</span><span class="sxs-lookup"><span data-stu-id="74f46-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="74f46-116">**必要な**– プロパティが必須フィールドであることを示します</span><span class="sxs-lookup"><span data-stu-id="74f46-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="74f46-117">**DisplayName** – フォーム フィールドと検証メッセージで使用テキストを定義</span><span class="sxs-lookup"><span data-stu-id="74f46-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="74f46-118">**StringLength** – 文字列フィールドの最大の長さを定義します。</span><span class="sxs-lookup"><span data-stu-id="74f46-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="74f46-119">**範囲**– 数値フィールドの最大値と最小値を設定</span><span class="sxs-lookup"><span data-stu-id="74f46-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="74f46-120">**バインド**– を除外したり、モデルのプロパティをパラメーターまたはフォームの値をバインドする場合は、フィールドの一覧</span><span class="sxs-lookup"><span data-stu-id="74f46-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="74f46-121">**ScaffoldColumn** – エディターのフォームのフィールドを非表示が可能になります</span><span class="sxs-lookup"><span data-stu-id="74f46-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="74f46-122">*注: データの注釈属性を使用してモデルの検証の詳細については、マニュアルを参照して、MSDN*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="74f46-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="74f46-123">アルバム クラスを開き、次の追加*を使用して*ステートメントを先頭にします。</span><span class="sxs-lookup"><span data-stu-id="74f46-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="74f46-124">次に、次に示すように、表示と検証の属性を追加するプロパティを更新します。</span><span class="sxs-lookup"><span data-stu-id="74f46-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="74f46-125">存在している時にもに加えた変更ジャンル、アーティスト仮想プロパティです。</span><span class="sxs-lookup"><span data-stu-id="74f46-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="74f46-126">これにより、遅延読み込みに必要に応じて、Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="74f46-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="74f46-127">アルバム、モデルにこれらの属性を追加すること後の作成および編集画面がすぐにフィールドの検証を開始し、表示名を使用する場合は、この (例: アルバム アートの Url AlbumArtUrl ではなく) 選択したします。</span><span class="sxs-lookup"><span data-stu-id="74f46-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="74f46-128">アプリケーションを実行し、/StoreManager/Create を参照します。</span><span class="sxs-lookup"><span data-stu-id="74f46-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="74f46-129">次に、いくつかの検証規則を破損します。</span><span class="sxs-lookup"><span data-stu-id="74f46-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="74f46-130">0 の価格を入力し、タイトルを空白のままにします。</span><span class="sxs-lookup"><span data-stu-id="74f46-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="74f46-131">[作成] ボタンをクリックしたときの検証エラー メッセージを表示するフィールドは、検証規則を満たしていませんでした定義したと表示されるフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="74f46-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="74f46-132">クライアント側検証のテスト</span><span class="sxs-lookup"><span data-stu-id="74f46-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="74f46-133">サーバー側の検証は、ユーザーがクライアント側の検証を回避するためにアプリケーションの観点からが重要です。</span><span class="sxs-lookup"><span data-stu-id="74f46-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="74f46-134">ただし、のみサーバー側の検証を実装している web ページのフォームには、次の 3 つの重要な問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="74f46-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="74f46-135">ユーザーは、フォーム ポストされた、サーバー上で検証して、ブラウザーに送信される応答を待機しています。</span><span class="sxs-lookup"><span data-stu-id="74f46-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="74f46-136">今すぐ、検証規則を通過するようにフィールドを解決するときに、ユーザーは即時のフィードバックを取得しません。</span><span class="sxs-lookup"><span data-stu-id="74f46-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="74f46-137">ユーザーのブラウザーを利用する代わりに検証ロジックを実行するサーバーのリソースを無駄にしています。</span><span class="sxs-lookup"><span data-stu-id="74f46-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="74f46-138">幸いにも、ASP.NET MVC 3 scaffold テンプレートでは、一切の補足作業をする必要はありません組み込まれており、クライアント側検証があります。</span><span class="sxs-lookup"><span data-stu-id="74f46-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="74f46-139">検証メッセージがすぐに削除するため、検証の要件を満たす"タイトル"フィールドに 1 つの文字を入力します。</span><span class="sxs-lookup"><span data-stu-id="74f46-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="74f46-140">[前へ](mvc-music-store-part-5.md)
> [次へ](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="74f46-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
