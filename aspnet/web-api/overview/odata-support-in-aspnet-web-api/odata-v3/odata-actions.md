---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: ASP.NET web API 2 OData アクションをサポートする |Microsoft ドキュメント
author: MikeWasson
description: 'OData では、アクションは、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 などのアクションを使用する一部: を実装しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="70c4f-104">ASP.NET web API 2 OData アクションをサポートします。</span><span class="sxs-lookup"><span data-stu-id="70c4f-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="70c4f-105">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="70c4f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="70c4f-106">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="70c4f-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="70c4f-107">OData では、*アクション*エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法は、します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="70c4f-108">いくつかの操作の使用は、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="70c4f-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="70c4f-109">複雑なトランザクションを実装します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="70c4f-110">一度に複数のエンティティを操作します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="70c4f-111">エンティティの特定のプロパティにのみ更新を許可します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="70c4f-112">エンティティで定義されていないサーバーに情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="70c4f-113">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="70c4f-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="70c4f-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="70c4f-114">Web API 2</span></span>
> - <span data-ttu-id="70c4f-115">OData バージョン 3</span><span class="sxs-lookup"><span data-stu-id="70c4f-115">OData Version 3</span></span>
> - <span data-ttu-id="70c4f-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="70c4f-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="70c4f-117">例: 製品の評価</span><span class="sxs-lookup"><span data-stu-id="70c4f-117">Example: Rating a Product</span></span>

<span data-ttu-id="70c4f-118">この例では、製品を評価し、各製品の平均評価を公開できるようにすることができます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="70c4f-119">データベースでは、評価と適合する製品の一覧を格納します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="70c4f-120">Entity Framework の評価を表す使用モデルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="70c4f-121">クライアントが POST したくありませんが、 `ProductRating` 「評価」コレクションにオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="70c4f-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="70c4f-122">直感的に、評価は製品コレクションに関連付けられ、クライアントを規制の値をポストするだけです。</span><span class="sxs-lookup"><span data-stu-id="70c4f-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="70c4f-123">そのため、通常の CRUD 操作を使用する代わりに製品をクライアントが呼び出すことのできるアクションを定義します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="70c4f-124">OData の用語で、アクションは*バインド*Product エンティティにします。</span><span class="sxs-lookup"><span data-stu-id="70c4f-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="70c4f-125">操作には、サーバー上に副作用があります。</span><span class="sxs-lookup"><span data-stu-id="70c4f-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="70c4f-126">このため、HTTP POST 要求を使用してに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="70c4f-127">操作は、パラメーターと戻り値の型が、サービス メタデータで説明されていることができます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="70c4f-128">クライアントが要求本文内のパラメーターを送信し、サーバーが応答本文に、戻り値を送信します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="70c4f-129">"レート Product"アクションを呼び出すには、クライアントで次のような URI に POST を送信します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="70c4f-130">POST 要求のデータだけで、製品評価しています。</span><span class="sxs-lookup"><span data-stu-id="70c4f-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="70c4f-131">エンティティ データ モデルのアクションを宣言します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="70c4f-132">構成では、Web API には、entity data model (EDM) にアクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="70c4f-133">このコードは、製品のエンティティで実行できるアクションとして"RateProduct"を定義します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="70c4f-134">実行する操作も宣言、 **int**パラメーター「評価」をという名前を返します、 **int**値。</span><span class="sxs-lookup"><span data-stu-id="70c4f-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="70c4f-135">コント ローラーにアクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-135">Add the Action to the Controller</span></span>

<span data-ttu-id="70c4f-136">"RateProduct"アクションは、製品のエンティティにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="70c4f-137">アクションを実装するには、という名前のメソッドを追加`RateProduct`Products コント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="70c4f-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="70c4f-138">メソッド名が、EDM のアクションの名前と一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="70c4f-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="70c4f-139">メソッドには、2 つのパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="70c4f-139">The method has two parameters:</span></span>

- <span data-ttu-id="70c4f-140">*キー*: 速度に製品のキー。</span><span class="sxs-lookup"><span data-stu-id="70c4f-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="70c4f-141">*パラメーター*: アクション パラメーターの値のディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="70c4f-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="70c4f-142">既定のルーティング規則を使用している場合キー パラメーターを「キー」という必要があります。</span><span class="sxs-lookup"><span data-stu-id="70c4f-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="70c4f-143">含める必要も、 **[FromOdataUri]** 属性が示すようにします。</span><span class="sxs-lookup"><span data-stu-id="70c4f-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="70c4f-144">この属性では、Web API 要求 URI のキーを解析する場合は、OData 構文の規則を使用するように指示します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="70c4f-145">使用して、*パラメーター*アクション パラメーターを取得するためのディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="70c4f-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="70c4f-146">クライアントが適切で、アクション パラメーターを送信する場合の値の書式**ModelState.IsValid**は true です。</span><span class="sxs-lookup"><span data-stu-id="70c4f-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="70c4f-147">その場合は、使用することができます、 **ODataActionParameters**パラメーター値を取得するためのディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="70c4f-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="70c4f-148">この例では、`RateProduct`アクションが「評価」をという名前の単一パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="70c4f-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="70c4f-149">アクションのメタデータ</span><span class="sxs-lookup"><span data-stu-id="70c4f-149">Action Metadata</span></span>

<span data-ttu-id="70c4f-150">サービス メタデータを表示するには、/odata/$ メタデータに GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="70c4f-151">ここでは、メタデータを宣言するの部分であり、`RateProduct`アクション。</span><span class="sxs-lookup"><span data-stu-id="70c4f-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="70c4f-152">**FunctionImport**要素は、アクションを宣言します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="70c4f-153">ほとんどのフィールドが自明ですが 2 つの注意。</span><span class="sxs-lookup"><span data-stu-id="70c4f-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="70c4f-154">**IsBindable**アクションをターゲット エンティティには、少なくともから呼び出されることができますを意味する時間の一部です。</span><span class="sxs-lookup"><span data-stu-id="70c4f-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="70c4f-155">**IsAlwaysBindable**ターゲット エンティティに対して常にアクションを呼び出せることを意味します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="70c4f-156">違いは、いくつかのアクションは、常に、クライアントが使用できるが、その他のアクションがエンティティの状態によって異なります。</span><span class="sxs-lookup"><span data-stu-id="70c4f-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="70c4f-157">たとえば、「購買」アクションを定義するとします。</span><span class="sxs-lookup"><span data-stu-id="70c4f-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="70c4f-158">在庫にある項目のみ購入できます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="70c4f-159">在庫切れ、アイテムがある場合、クライアントはそのアクションを呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="70c4f-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="70c4f-160">EDM を定義するとき、**アクション**メソッドは常にバインド可能なアクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="70c4f-161">Not を常に、バインド可能な操作について説明します (とも呼ばれる*一時的な*アクション) このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="70c4f-162">アクションを呼び出す</span><span class="sxs-lookup"><span data-stu-id="70c4f-162">Invoking the Action</span></span>

<span data-ttu-id="70c4f-163">これで、クライアントはこの操作を呼び出す方法を確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="70c4f-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="70c4f-164">クライアントがプロダクトの product ID への 2 の年齢区分を付与すると 4 を = です。</span><span class="sxs-lookup"><span data-stu-id="70c4f-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="70c4f-165">要求本文の JSON 形式を使用して、例要求メッセージを次に示します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="70c4f-166">応答メッセージを次に示します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="70c4f-167">エンティティ セットへのアクションのバインド</span><span class="sxs-lookup"><span data-stu-id="70c4f-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="70c4f-168">前の例では、アクションが単一のエンティティにバインドされて: クライアントが 1 つの製品を評価します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="70c4f-169">アクションは、エンティティのコレクションにバインドすることもできます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="70c4f-170">次の変更を加えるだけ。</span><span class="sxs-lookup"><span data-stu-id="70c4f-170">Just make the following changes:</span></span>

<span data-ttu-id="70c4f-171">EDM では、アクションを追加するエンティティの**コレクション**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="70c4f-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="70c4f-172">コント ローラーのメソッドで省略、*キー*パラメーター。</span><span class="sxs-lookup"><span data-stu-id="70c4f-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="70c4f-173">クライアントは、Products エンティティ セットに対するアクションを呼び出すようになりました。</span><span class="sxs-lookup"><span data-stu-id="70c4f-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="70c4f-174">コレクションのパラメーターを使って操作</span><span class="sxs-lookup"><span data-stu-id="70c4f-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="70c4f-175">アクション パラメーターを値のコレクションを受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="70c4f-176">EDM では、次のように使用します。 **CollectionParameter&lt;T&gt;** パラメーターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="70c4f-177">コレクションを受け取って「評価」をという名前のパラメーターを宣言してこの**int**値。</span><span class="sxs-lookup"><span data-stu-id="70c4f-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="70c4f-178">メソッドでは、コント ローラー、まだ値を取得するパラメーターから、 **ODataActionParameters**オブジェクトが現在値は、 **ICollection&lt;int&gt;** 値。</span><span class="sxs-lookup"><span data-stu-id="70c4f-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="70c4f-179">一時的な操作</span><span class="sxs-lookup"><span data-stu-id="70c4f-179">Transient Actions</span></span>

<span data-ttu-id="70c4f-180">例では、"RateProduct"、ユーザーことができます常に製品を評価する、アクションが常に使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="70c4f-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="70c4f-181">一部の操作は、エンティティの状態によって異なります。</span><span class="sxs-lookup"><span data-stu-id="70c4f-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="70c4f-182">たとえば、ビデオ レンタル サービスで、「チェック アウト」アクションは常に使用できません。</span><span class="sxs-lookup"><span data-stu-id="70c4f-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="70c4f-183">(これによって異なるそのビデオのコピーが使用できるかどうかです。)この種類の操作が呼び出されます、*一時的な*アクション。</span><span class="sxs-lookup"><span data-stu-id="70c4f-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="70c4f-184">一時的なアクションには、サービス メタデータで**IsAlwaysBindable**を false に等しい。</span><span class="sxs-lookup"><span data-stu-id="70c4f-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="70c4f-185">既定値は実際には、メタデータは次のようになりますのでです。</span><span class="sxs-lookup"><span data-stu-id="70c4f-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="70c4f-186">ここでこれが重要な理由: サーバーがアクションを使用すると、クライアントに指示する必要があるアクションが一時的である場合。</span><span class="sxs-lookup"><span data-stu-id="70c4f-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="70c4f-187">この機能を使用するには、エンティティのアクションへのリンクを含みます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="70c4f-188">ムービーのエンティティの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="70c4f-189">"#CheckOut"プロパティには、チェック アウト操作へのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="70c4f-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="70c4f-190">アクションが使用できない場合、サーバーは、リンクを除外します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="70c4f-191">EDM の一時的なアクションを宣言するを呼び出して、 **TransientAction**メソッド。</span><span class="sxs-lookup"><span data-stu-id="70c4f-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="70c4f-192">また、特定のエンティティのアクション リンクを返す関数を用意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="70c4f-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="70c4f-193">この関数を呼び出すことによって設定**HasActionLink**です。</span><span class="sxs-lookup"><span data-stu-id="70c4f-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="70c4f-194">関数は、ラムダ式として記述できます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="70c4f-195">アクションがある場合、ラムダ式は、アクションにリンクを返します。</span><span class="sxs-lookup"><span data-stu-id="70c4f-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="70c4f-196">OData シリアライザーには、エンティティをシリアル化時に、このリンクが含まれます。</span><span class="sxs-lookup"><span data-stu-id="70c4f-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="70c4f-197">アクションを使用できない場合、関数を返します`null`です。</span><span class="sxs-lookup"><span data-stu-id="70c4f-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="70c4f-198">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="70c4f-198">Additional Resources</span></span>

[<span data-ttu-id="70c4f-199">OData アクションのサンプル</span><span class="sxs-lookup"><span data-stu-id="70c4f-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
