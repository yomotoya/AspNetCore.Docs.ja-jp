---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: IDataErrorInfo インターフェイス (VB) の検証 |Microsoft Docs
author: StephenWalther
description: Stephen Walther では、モデル クラスで IDataErrorInfo インターフェイスを実装することによってカスタムの検証エラー メッセージを表示する方法を示します。
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 55716b48a7991424a4d03bbbc7d96f8f12f8dabb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834020"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a><span data-ttu-id="1384e-103">IDataErrorInfo インターフェイス (VB) の検証</span><span class="sxs-lookup"><span data-stu-id="1384e-103">Validating with the IDataErrorInfo Interface (VB)</span></span>
====================
<span data-ttu-id="1384e-104">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="1384e-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="1384e-105">Stephen Walther では、モデル クラスで IDataErrorInfo インターフェイスを実装することによってカスタムの検証エラー メッセージを表示する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1384e-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="1384e-106">このチュートリアルの目的では、ASP.NET MVC アプリケーションで検証を実行するための 1 つの方法をについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1384e-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="1384e-107">必要なフォーム フィールドの値を指定せず、HTML フォームを送信されないようにする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1384e-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="1384e-108">このチュートリアルでは、IErrorDataInfo インターフェイスを使用して検証を実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1384e-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="1384e-109">外部からの影響</span><span class="sxs-lookup"><span data-stu-id="1384e-109">Assumptions</span></span>

<span data-ttu-id="1384e-110">このチュートリアルで MoviesDB データベースと映画データベース テーブルを使用します。</span><span class="sxs-lookup"><span data-stu-id="1384e-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="1384e-111">このテーブルは、次の列を持っています。</span><span class="sxs-lookup"><span data-stu-id="1384e-111">This table has the following columns:</span></span>

<a id="0.6_table01"></a>


| <span data-ttu-id="1384e-112">**列名**</span><span class="sxs-lookup"><span data-stu-id="1384e-112">**Column Name**</span></span> | <span data-ttu-id="1384e-113">**データ型**</span><span class="sxs-lookup"><span data-stu-id="1384e-113">**Data Type**</span></span> | <span data-ttu-id="1384e-114">**Null を許容します。**</span><span class="sxs-lookup"><span data-stu-id="1384e-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1384e-115">ID</span><span class="sxs-lookup"><span data-stu-id="1384e-115">Id</span></span> | <span data-ttu-id="1384e-116">Int</span><span class="sxs-lookup"><span data-stu-id="1384e-116">Int</span></span> | <span data-ttu-id="1384e-117">False</span><span class="sxs-lookup"><span data-stu-id="1384e-117">False</span></span> |
| <span data-ttu-id="1384e-118">Title</span><span class="sxs-lookup"><span data-stu-id="1384e-118">Title</span></span> | <span data-ttu-id="1384e-119">nvarchar (100)</span><span class="sxs-lookup"><span data-stu-id="1384e-119">Nvarchar(100)</span></span> | <span data-ttu-id="1384e-120">False</span><span class="sxs-lookup"><span data-stu-id="1384e-120">False</span></span> |
| <span data-ttu-id="1384e-121">ディレクター</span><span class="sxs-lookup"><span data-stu-id="1384e-121">Director</span></span> | <span data-ttu-id="1384e-122">nvarchar (100)</span><span class="sxs-lookup"><span data-stu-id="1384e-122">Nvarchar(100)</span></span> | <span data-ttu-id="1384e-123">False</span><span class="sxs-lookup"><span data-stu-id="1384e-123">False</span></span> |
| <span data-ttu-id="1384e-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="1384e-124">DateReleased</span></span> | <span data-ttu-id="1384e-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="1384e-125">DateTime</span></span> | <span data-ttu-id="1384e-126">False</span><span class="sxs-lookup"><span data-stu-id="1384e-126">False</span></span> |


<span data-ttu-id="1384e-127">このチュートリアルでは、私のデータベース モデル クラスを生成するのに Microsoft Entity Framework を使用します。</span><span class="sxs-lookup"><span data-stu-id="1384e-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="1384e-128">Entity Framework によって生成されたムービー クラスは、図 1 に表示されます。</span><span class="sxs-lookup"><span data-stu-id="1384e-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="1384e-129">[![ムービー エンティティ](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1384e-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span></span>

<span data-ttu-id="1384e-130">**図 01**: The Movie エンティティ ([フルサイズの画像を表示する をクリックします](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="1384e-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="1384e-131">Entity Framework を使用して、データベース モデル クラスを生成する方法の詳細については、拙著のチュートリアルという、Entity Framework でモデル クラスを作成するを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1384e-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="1384e-132">コント ローラー クラス</span><span class="sxs-lookup"><span data-stu-id="1384e-132">The Controller Class</span></span>

<span data-ttu-id="1384e-133">ムービーの一覧に、Home コント ローラーを使用して新しいムービーを作成します。</span><span class="sxs-lookup"><span data-stu-id="1384e-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="1384e-134">リスト 1 で、このクラスのコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1384e-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="1384e-135">**1 - Controllers\HomeController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1384e-135">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

<span data-ttu-id="1384e-136">リスト 1 で、ホーム コント ローラー クラスには、2 つの Create() アクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1384e-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="1384e-137">最初のアクションは、新しいムービーを作成するための HTML フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="1384e-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="1384e-138">2 番目の Create() アクションでは、データベースに新しいムービーの実際の挿入を実行します。</span><span class="sxs-lookup"><span data-stu-id="1384e-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="1384e-139">最初の Create() アクションによって表示されるフォームがサーバーに送信されたときに、2 番目の Create() アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="1384e-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="1384e-140">2 番目の Create() アクションに次のコード行が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1384e-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

<span data-ttu-id="1384e-141">検証エラーがある場合に、IsValid プロパティが false を返します。</span><span class="sxs-lookup"><span data-stu-id="1384e-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="1384e-142">その場合は、ムービーを作成するための HTML フォームを含む、作成ビューが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="1384e-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="1384e-143">部分クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="1384e-143">Creating a Partial Class</span></span>

<span data-ttu-id="1384e-144">ムービー クラスは、Entity Framework によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="1384e-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="1384e-145">ソリューション エクスプ ローラー ウィンドウで、MoviesDBModel.edmx ファイルを展開すると、コード エディターで MoviesDBModel.Designer.vb ファイルを開き、ムービー クラスのコードを確認できます (図 2 参照)。</span><span class="sxs-lookup"><span data-stu-id="1384e-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.vb file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="1384e-146">[![ムービー エンティティのコード](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1384e-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span></span>

<span data-ttu-id="1384e-147">**図 02**: ムービー エンティティのコード ([フルサイズの画像を表示する をクリックします](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))。</span><span class="sxs-lookup"><span data-stu-id="1384e-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span></span>


<span data-ttu-id="1384e-148">ムービー クラスは、部分クラスです。</span><span class="sxs-lookup"><span data-stu-id="1384e-148">The Movie class is a partial class.</span></span> <span data-ttu-id="1384e-149">ムービー クラスの機能を拡張する同じ名前の別の部分クラスを追加できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="1384e-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="1384e-150">新しい部分クラスに、検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="1384e-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="1384e-151">リスト 2 で、クラスを Models フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="1384e-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="1384e-152">**2 - Models\Movie.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1384e-152">**Listing 2 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

<span data-ttu-id="1384e-153">リスト 2 でクラスが含まれていますが、*部分*修飾子。</span><span class="sxs-lookup"><span data-stu-id="1384e-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="1384e-154">メソッドまたはこのクラスに追加するプロパティは、Entity Framework によって生成されたムービー クラスの一部になります。</span><span class="sxs-lookup"><span data-stu-id="1384e-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="1384e-155">OnChanging と OnChanged 部分メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="1384e-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="1384e-156">Entity Framework では、エンティティ クラスを生成するとき、Entity Framework の部分メソッドをクラスに自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="1384e-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="1384e-157">Entity Framework では、クラスの各プロパティに対応する OnChanging と OnChanged の部分的なメソッドを生成します。</span><span class="sxs-lookup"><span data-stu-id="1384e-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="1384e-158">ムービー クラスの場合は、Entity Framework は、次のメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="1384e-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="1384e-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="1384e-159">OnIdChanging</span></span>
- <span data-ttu-id="1384e-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="1384e-160">OnIdChanged</span></span>
- <span data-ttu-id="1384e-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="1384e-161">OnTitleChanging</span></span>
- <span data-ttu-id="1384e-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="1384e-162">OnTitleChanged</span></span>
- <span data-ttu-id="1384e-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="1384e-163">OnDirectorChanging</span></span>
- <span data-ttu-id="1384e-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="1384e-164">OnDirectorChanged</span></span>
- <span data-ttu-id="1384e-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="1384e-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="1384e-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="1384e-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="1384e-167">OnChanging メソッドは、対応するプロパティが変更される前に、適切なと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="1384e-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="1384e-168">OnChanged メソッドは、プロパティが変更された後、適切なと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="1384e-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="1384e-169">ムービー クラスに検証ロジックを追加するこれらの部分メソッドの利用できます。</span><span class="sxs-lookup"><span data-stu-id="1384e-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="1384e-170">更新リスト 3 のムービー クラスは、タイトルとディレクター プロパティが空でない値に割り当てられていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1384e-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="1384e-171">部分メソッドは、実装する必要はありませんが、クラスで定義されたメソッドです。</span><span class="sxs-lookup"><span data-stu-id="1384e-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="1384e-172">部分メソッドを実装しない場合、コンパイラがメソッド シグネチャを削除し、メソッドのためにすべての呼び出しには、部分メソッドに関連付けられている実行時のコストはありません。</span><span class="sxs-lookup"><span data-stu-id="1384e-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="1384e-173">Visual Studio コード エディターで、キーワードを入力して、部分メソッドに追加できます*部分*後にスペースを実装するパーシャルの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="1384e-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="1384e-174">**3 - Models\Movie.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1384e-174">**Listing 3 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

<span data-ttu-id="1384e-175">たとえば、Title プロパティに空の文字列を代入しようとすると、し、エラー メッセージに割り当てられますという名前のディクショナリ\_エラー。</span><span class="sxs-lookup"><span data-stu-id="1384e-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="1384e-176">この時点では、実際に何タイトルのプロパティに空の文字列を割り当てるし、エラーが、プライベートに追加\_エラー フィールド。</span><span class="sxs-lookup"><span data-stu-id="1384e-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="1384e-177">ASP.NET MVC フレームワークにこれらの検証エラーを公開する IDataErrorInfo インターフェイスを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1384e-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="1384e-178">IDataErrorInfo インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="1384e-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="1384e-179">IDataErrorInfo インターフェイスは、最初のバージョンから .NET framework の一部をしました。</span><span class="sxs-lookup"><span data-stu-id="1384e-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="1384e-180">このインターフェイスは、非常に単純なインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="1384e-180">This interface is a very simple interface:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

<span data-ttu-id="1384e-181">クラスは、IDataErrorInfo インターフェイスを実装する場合、クラスのインスタンスを作成するときに、ASP.NET MVC フレームワークはこのインターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="1384e-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="1384e-182">たとえば、ホーム コント ローラー Create() アクションは、Movie クラスのインスタンスを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="1384e-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

<span data-ttu-id="1384e-183">ASP.NET MVC フレームワークは、モデル バインダー (DefaultModelBinder) を使用して Create() アクションに渡されたムービーのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="1384e-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="1384e-184">モデル バインダーは、ムービー オブジェクトのインスタンスには HTML フォームのフィールドを連結してムービー オブジェクトのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="1384e-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="1384e-185">DefaultModelBinder クラスは、IDataErrorInfo インターフェイスを実装するかどうかを検出します。</span><span class="sxs-lookup"><span data-stu-id="1384e-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="1384e-186">クラスは、このインターフェイスを実装している場合、モデル バインダーは、クラスの各プロパティの IDataErrorInfo.this インデクサーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1384e-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="1384e-187">インデクサーには、エラー メッセージが返されます。 モデル バインダーによって、状態を自動的にモデル化するには、このエラー メッセージが追加されます。</span><span class="sxs-lookup"><span data-stu-id="1384e-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="1384e-188">また、DefaultModelBinder は IDataErrorInfo.Error プロパティを確認します。</span><span class="sxs-lookup"><span data-stu-id="1384e-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="1384e-189">このプロパティは、クラスに関連付けられているプロパティ以外の特定の検証エラーを表すものです。</span><span class="sxs-lookup"><span data-stu-id="1384e-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="1384e-190">たとえば、Movie クラスの複数のプロパティの値に依存している検証規則を適用します。</span><span class="sxs-lookup"><span data-stu-id="1384e-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="1384e-191">この例ではエラー プロパティから、検証エラーを返します。</span><span class="sxs-lookup"><span data-stu-id="1384e-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="1384e-192">リスト 4 の更新された Movie クラスは、IDataErrorInfo インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="1384e-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="1384e-193">**4 - Models\Movie.vb (IDataErrorInfo の実装) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="1384e-193">**Listing 4 - Models\Movie.vb (implements IDataErrorInfo)**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

<span data-ttu-id="1384e-194">リスト 4 の場合、インデクサー プロパティを確認します、\_エラー コレクションにプロパティ名に対応するキーが含まれているかどうかは、インデクサーに渡されます。</span><span class="sxs-lookup"><span data-stu-id="1384e-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="1384e-195">プロパティに関連付けられている検証エラーがない場合は空の文字列が返されます。</span><span class="sxs-lookup"><span data-stu-id="1384e-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="1384e-196">変更されたムービー クラスを使用する任意の方法で、Home コント ローラーを変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="1384e-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="1384e-197">図 3 に表示されるページは、タイトルまたはディレクター フォーム フィールドの値が入力されていないときの動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="1384e-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="1384e-198">[![アクション メソッドを自動的に作成します。](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1384e-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span></span>

<span data-ttu-id="1384e-199">**図 03**: 不足値を含むフォーム ([フルサイズの画像を表示する をクリックします](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="1384e-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span></span>


<span data-ttu-id="1384e-200">DateReleased 値が自動的に検証されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1384e-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="1384e-201">DateReleased プロパティが NULL 値を受け付けないので、DefaultModelBinder は、値があるないときに自動的にこのプロパティの検証エラーを生成します。</span><span class="sxs-lookup"><span data-stu-id="1384e-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="1384e-202">DateReleased プロパティのエラー メッセージを変更する場合は、カスタム モデル バインダーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1384e-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="1384e-203">まとめ</span><span class="sxs-lookup"><span data-stu-id="1384e-203">Summary</span></span>

<span data-ttu-id="1384e-204">このチュートリアルでは、IDataErrorInfo インターフェイスを使用して、検証エラー メッセージを生成する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="1384e-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="1384e-205">最初に、Entity Framework によって生成される部分的なムービー クラスの機能を拡張する部分のムービー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="1384e-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="1384e-206">次に、ムービー クラス OnTitleChanging() と OnDirectorChanging() 部分メソッドに検証ロジックを追加しました。</span><span class="sxs-lookup"><span data-stu-id="1384e-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="1384e-207">最後に、ASP.NET MVC フレームワークにこれらの検証メッセージを公開するために、IDataErrorInfo インターフェイスを実装しました。</span><span class="sxs-lookup"><span data-stu-id="1384e-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1384e-208">[前へ](performing-simple-validation-vb.md)
> [次へ](validating-with-a-service-layer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1384e-208">[Previous](performing-simple-validation-vb.md)
[Next](validating-with-a-service-layer-vb.md)</span></span>
