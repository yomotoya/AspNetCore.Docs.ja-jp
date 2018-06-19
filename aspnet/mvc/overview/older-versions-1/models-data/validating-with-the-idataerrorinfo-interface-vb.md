---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: IDataErrorInfo インターフェイス (VB) の検証 |Microsoft ドキュメント
author: StephenWalther
description: Stephen Walther では、モデル クラスで IDataErrorInfo インターフェイスを実装することによってカスタムの検証エラー メッセージを表示する方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 60df0f934432484e0c97e0caef25c15605beb14f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870090"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a><span data-ttu-id="a3e6a-103">IDataErrorInfo インターフェイス (VB) の検証</span><span class="sxs-lookup"><span data-stu-id="a3e6a-103">Validating with the IDataErrorInfo Interface (VB)</span></span>
====================
<span data-ttu-id="a3e6a-104">によって[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a3e6a-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="a3e6a-105">Stephen Walther では、モデル クラスで IDataErrorInfo インターフェイスを実装することによってカスタムの検証エラー メッセージを表示する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="a3e6a-106">このチュートリアルの目的は、ASP.NET MVC アプリケーションの検証の実行に 1 つの方法を説明するためです。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="a3e6a-107">他のユーザーから必要なフォーム フィールドの値を指定せず、HTML フォームの送信を回避する方法について学びます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="a3e6a-108">このチュートリアルでは、IErrorDataInfo インターフェイスを使用して検証を実行する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="a3e6a-109">外部からの影響</span><span class="sxs-lookup"><span data-stu-id="a3e6a-109">Assumptions</span></span>

<span data-ttu-id="a3e6a-110">このチュートリアルでは、MoviesDB データベースとムービーのデータベース テーブルを使用します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="a3e6a-111">このテーブルは、次の列を持っています。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-111">This table has the following columns:</span></span>

<a id="0.6_table01"></a>


| <span data-ttu-id="a3e6a-112">**列名**</span><span class="sxs-lookup"><span data-stu-id="a3e6a-112">**Column Name**</span></span> | <span data-ttu-id="a3e6a-113">**データ型**</span><span class="sxs-lookup"><span data-stu-id="a3e6a-113">**Data Type**</span></span> | <span data-ttu-id="a3e6a-114">**Null を許容します。**</span><span class="sxs-lookup"><span data-stu-id="a3e6a-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a3e6a-115">ID</span><span class="sxs-lookup"><span data-stu-id="a3e6a-115">Id</span></span> | <span data-ttu-id="a3e6a-116">Int</span><span class="sxs-lookup"><span data-stu-id="a3e6a-116">Int</span></span> | <span data-ttu-id="a3e6a-117">False</span><span class="sxs-lookup"><span data-stu-id="a3e6a-117">False</span></span> |
| <span data-ttu-id="a3e6a-118">Title</span><span class="sxs-lookup"><span data-stu-id="a3e6a-118">Title</span></span> | <span data-ttu-id="a3e6a-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="a3e6a-119">Nvarchar(100)</span></span> | <span data-ttu-id="a3e6a-120">False</span><span class="sxs-lookup"><span data-stu-id="a3e6a-120">False</span></span> |
| <span data-ttu-id="a3e6a-121">ディレクター</span><span class="sxs-lookup"><span data-stu-id="a3e6a-121">Director</span></span> | <span data-ttu-id="a3e6a-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="a3e6a-122">Nvarchar(100)</span></span> | <span data-ttu-id="a3e6a-123">False</span><span class="sxs-lookup"><span data-stu-id="a3e6a-123">False</span></span> |
| <span data-ttu-id="a3e6a-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="a3e6a-124">DateReleased</span></span> | <span data-ttu-id="a3e6a-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="a3e6a-125">DateTime</span></span> | <span data-ttu-id="a3e6a-126">False</span><span class="sxs-lookup"><span data-stu-id="a3e6a-126">False</span></span> |


<span data-ttu-id="a3e6a-127">このチュートリアルでは、データベース モデル クラスを生成するのに Microsoft Entity Framework を使用します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="a3e6a-128">Entity Framework によって生成されたムービー クラスは、図 1 に表示されます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="a3e6a-129">[![ムービー エンティティ](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a3e6a-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span></span>

<span data-ttu-id="a3e6a-130">**図 01**:「ムービー エンティティ ([フルサイズのイメージを表示するには、をクリックして](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a3e6a-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="a3e6a-131">Entity Framework を使用して、データベース モデル クラスを生成する方法の詳細についてを参照してください チュートリアルと Entity Framework モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="a3e6a-132">コント ローラー クラス</span><span class="sxs-lookup"><span data-stu-id="a3e6a-132">The Controller Class</span></span>

<span data-ttu-id="a3e6a-133">ムービーの一覧に、Home コント ローラーを使用し、新しいムービーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="a3e6a-134">このクラスのコードは、1 のリストに含まれます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="a3e6a-135">**1 - Controllers\HomeController.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a3e6a-135">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

<span data-ttu-id="a3e6a-136">リスト 1 で、ホーム コント ローラー クラスには、次の 2 つの Create() アクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="a3e6a-137">最初のアクションは、新しいムービーを作成するための HTML フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="a3e6a-138">2 番目の Create() アクションは、データベースに新しいムービーの実際の挿入を実行します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="a3e6a-139">最初の Create() アクションによって表示されるフォームがサーバーに送信されるときに、2 番目の Create() アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="a3e6a-140">2 番目の Create() アクションが次のコード行が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

<span data-ttu-id="a3e6a-141">IsValid プロパティは、検証エラーがある場合に false を返します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="a3e6a-142">その場合は、ムービーを作成するための HTML フォームを含むビューを作成するが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="a3e6a-143">部分クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-143">Creating a Partial Class</span></span>

<span data-ttu-id="a3e6a-144">ムービー クラスは、Entity Framework によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="a3e6a-145">ソリューション エクスプ ローラー ウィンドウで、MoviesDBModel.edmx ファイルを展開すると、コード エディターで MoviesDBModel.Designer.vb ファイルを開く、ムービー クラスのコードを確認できます (図 2 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.vb file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="a3e6a-146">[![ムービー エンティティのコード](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a3e6a-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span></span>

<span data-ttu-id="a3e6a-147">**図 02**: ムービー エンティティのコード ([フルサイズのイメージを表示するをクリックして](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a3e6a-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span></span>


<span data-ttu-id="a3e6a-148">ムービー クラスは、部分クラスです。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-148">The Movie class is a partial class.</span></span> <span data-ttu-id="a3e6a-149">つまり、ムービー クラスの機能を拡張する同じ名前の別の部分クラスを追加できます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="a3e6a-150">新しい部分クラスに、検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="a3e6a-151">Models フォルダーに一覧表示する 2 で、クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="a3e6a-152">**2 - Models\Movie.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a3e6a-152">**Listing 2 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

<span data-ttu-id="a3e6a-153">通知を一覧表示する 2 でクラスを含む、*部分*修飾子です。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="a3e6a-154">任意のメソッドやプロパティをこのクラスに追加する Entity Framework によって生成されたムービー クラスの一部になります。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="a3e6a-155">OnChanging と OnChanged 部分メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="a3e6a-156">Entity Framework では、エンティティ クラスを生成するとき、Entity Framework 部分メソッドをクラスに自動的に追加します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="a3e6a-157">Entity Framework では、クラスの各プロパティに対応する OnChanging と OnChanged の部分的なメソッドを生成します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="a3e6a-158">ムービー クラスの場合は、Entity Framework は、次のメソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="a3e6a-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="a3e6a-159">OnIdChanging</span></span>
- <span data-ttu-id="a3e6a-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="a3e6a-160">OnIdChanged</span></span>
- <span data-ttu-id="a3e6a-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="a3e6a-161">OnTitleChanging</span></span>
- <span data-ttu-id="a3e6a-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="a3e6a-162">OnTitleChanged</span></span>
- <span data-ttu-id="a3e6a-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="a3e6a-163">OnDirectorChanging</span></span>
- <span data-ttu-id="a3e6a-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="a3e6a-164">OnDirectorChanged</span></span>
- <span data-ttu-id="a3e6a-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="a3e6a-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="a3e6a-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="a3e6a-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="a3e6a-167">対応するプロパティを変更するには、OnChanging メソッドが適切に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="a3e6a-168">プロパティが変更された後は、OnChanged メソッドが適切に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="a3e6a-169">ムービー クラスに検証ロジックを追加するこれらの部分メソッドの利用できます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="a3e6a-170">更新リスト 3 のムービー クラスは、タイトルとディレクター プロパティが空でない値を割り当てられているを確認します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a3e6a-171">部分メソッドは、実装する必要はありません、クラスで定義されている方法です。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="a3e6a-172">部分メソッドを実装しない場合は、コンパイラがメソッドのシグネチャを削除し、メソッドをこのようにすべての呼び出しには、部分メソッドに関連付けられているランタイムのコストはありません。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="a3e6a-173">Visual Studio コード エディターで、キーワードを入力して部分メソッドを追加することができます*部分*後にスペースを実装するパーシャルの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="a3e6a-174">**3 - Models\Movie.vb を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a3e6a-174">**Listing 3 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

<span data-ttu-id="a3e6a-175">たとえば、タイトルのプロパティに空の文字列を代入しようとすると、し、エラー メッセージに割り当てられますという名前のディクショナリ\_エラーです。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="a3e6a-176">この時点では、実際に何も、Title プロパティに空の文字列を割り当てるし、エラーが、プライベートに追加\_エラー フィールドです。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="a3e6a-177">ASP.NET MVC フレームワークにこれらの検証エラーを公開する IDataErrorInfo インターフェイスを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="a3e6a-178">IDataErrorInfo インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="a3e6a-179">IDataErrorInfo インターフェイスは、最初のバージョンから .NET framework の一部をされました。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="a3e6a-180">このインターフェイスは、非常に単純なインターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-180">This interface is a very simple interface:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

<span data-ttu-id="a3e6a-181">クラスは、IDataErrorInfo インターフェイスを実装する場合、クラスのインスタンスを作成するときに、ASP.NET MVC フレームワークはこのインターフェイスを使用します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="a3e6a-182">たとえば、Home コント ローラー Create() アクションは、ムービー クラスのインスタンスを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

<span data-ttu-id="a3e6a-183">ASP.NET MVC フレームワークは、モデル バインダー (DefaultModelBinder) を使用して、Create() アクションに渡される、ムービーのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="a3e6a-184">モデル バインダーは、ムービー オブジェクトのインスタンスには HTML フォームのフィールドを連結してムービー オブジェクトのインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="a3e6a-185">DefaultModelBinder クラス IDataErrorInfo インターフェイスを実装するかどうかを検出します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="a3e6a-186">クラスは、このインターフェイスを実装している場合、モデル バインダーは、クラスの各プロパティの IDataErrorInfo.this インデクサーを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="a3e6a-187">インデクサーにエラー メッセージが返された場合、モデル バインダーは状態を自動的にモデル化するには、このエラー メッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="a3e6a-188">DefaultModelBinder は IDataErrorInfo.Error プロパティも確認します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="a3e6a-189">このプロパティは、クラスに関連付けられたプロパティ以外の特定の検証エラーを表すために使用します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="a3e6a-190">たとえば、ムービー クラスの複数のプロパティの値に依存する検証規則を適用することができます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="a3e6a-191">その場合は、エラー プロパティから、検証エラーを返すとします。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="a3e6a-192">コード サンプル 4 の更新後のムービー クラスでは、IDataErrorInfo インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="a3e6a-193">**4 - Models\Movie.vb (IDataErrorInfo を実装) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="a3e6a-193">**Listing 4 - Models\Movie.vb (implements IDataErrorInfo)**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

<span data-ttu-id="a3e6a-194">インデクサー プロパティは、チェックを一覧表示する 4 で、\_インデクサーに渡されたプロパティの名前に対応するキーが含まれているかどうかはエラーのコレクション。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="a3e6a-195">プロパティに関連付けられている検証エラーがない場合は、空の文字列が返されます。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="a3e6a-196">変更したムービー クラスを使用する任意の方法で、Home コント ローラーを変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="a3e6a-197">図 3 に表示されるページは、タイトルやディレクター フォーム フィールドの値が入力されていない場合の動作を示しています。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="a3e6a-198">[![アクション メソッドを自動的に作成します。](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a3e6a-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span></span>

<span data-ttu-id="a3e6a-199">**図 03**: 欠損値を含むフォーム ([フルサイズのイメージを表示するをクリックして](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a3e6a-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span></span>


<span data-ttu-id="a3e6a-200">DateReleased 値を自動的に検証することを確認します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="a3e6a-201">DateReleased プロパティが NULL 値を受理しないため、DefaultModelBinder は、値があるないときに自動的にこのプロパティの検証エラーを生成します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="a3e6a-202">DateReleased プロパティは、エラー メッセージを変更する場合は、カスタム モデル バインダーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="a3e6a-203">まとめ</span><span class="sxs-lookup"><span data-stu-id="a3e6a-203">Summary</span></span>

<span data-ttu-id="a3e6a-204">このチュートリアルでは、IDataErrorInfo インターフェイスを使用して、検証エラー メッセージを生成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="a3e6a-205">最初に、Entity Framework によって生成された部分的なムービー クラスの機能を拡張する部分ムービー クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="a3e6a-206">次にムービー クラス OnTitleChanging() と OnDirectorChanging() 部分メソッドに検証ロジックを追加します。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="a3e6a-207">最後に、ASP.NET MVC フレームワークにこれらの検証メッセージを公開するために、IDataErrorInfo インターフェイスを実装しました。</span><span class="sxs-lookup"><span data-stu-id="a3e6a-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3e6a-208">[前へ](performing-simple-validation-vb.md)
> [次へ](validating-with-a-service-layer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a3e6a-208">[Previous](performing-simple-validation-vb.md)
[Next](validating-with-a-service-layer-vb.md)</span></span>
