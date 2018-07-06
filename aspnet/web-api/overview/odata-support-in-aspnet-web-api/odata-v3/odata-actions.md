---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: ASP.NET web API 2 OData アクションをサポートしている |Microsoft Docs
author: MikeWasson
description: Odata では、アクションは、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 アクションの使用が含まれます実装しています.。
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837773"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="23731-104">ASP.NET web API 2 OData アクションをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="23731-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="23731-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="23731-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="23731-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="23731-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="23731-107">Odata では、*アクション*エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。</span><span class="sxs-lookup"><span data-stu-id="23731-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="23731-108">アクションの使用は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="23731-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="23731-109">複雑なトランザクションを実装します。</span><span class="sxs-lookup"><span data-stu-id="23731-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="23731-110">一度に複数のエンティティを操作します。</span><span class="sxs-lookup"><span data-stu-id="23731-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="23731-111">エンティティの特定のプロパティにのみ更新できるようにします。</span><span class="sxs-lookup"><span data-stu-id="23731-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="23731-112">エンティティで定義されていないサーバーに情報を送信します。</span><span class="sxs-lookup"><span data-stu-id="23731-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="23731-113">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="23731-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="23731-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="23731-114">Web API 2</span></span>
> - <span data-ttu-id="23731-115">OData バージョン 3</span><span class="sxs-lookup"><span data-stu-id="23731-115">OData Version 3</span></span>
> - <span data-ttu-id="23731-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="23731-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="23731-117">例: 製品の評価</span><span class="sxs-lookup"><span data-stu-id="23731-117">Example: Rating a Product</span></span>

<span data-ttu-id="23731-118">この例ではユーザーが、製品を評価し、各製品の平均評価を公開できるようにします。</span><span class="sxs-lookup"><span data-stu-id="23731-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="23731-119">データベースでは、評価、製品にキーの一覧を格納します。</span><span class="sxs-lookup"><span data-stu-id="23731-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="23731-120">表す、Entity Framework での評価を使用するモデルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="23731-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="23731-121">投稿へのクライアントのたくはありませんが、 `ProductRating` 「評価」コレクションにオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="23731-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="23731-122">直感的な評価は、製品のコレクションに関連付けと、クライアントは、だけ評価値を投稿する必要があります。</span><span class="sxs-lookup"><span data-stu-id="23731-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="23731-123">そのため、通常の CRUD 操作を使用する代わりに、製品のクライアントが呼び出すことができるアクションを定義します。</span><span class="sxs-lookup"><span data-stu-id="23731-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="23731-124">OData 用語では、アクションは*バインド*Product エンティティにします。</span><span class="sxs-lookup"><span data-stu-id="23731-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="23731-125">アクションでは、サーバー上に副作用があります。</span><span class="sxs-lookup"><span data-stu-id="23731-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="23731-126">このため、HTTP POST 要求を使用してに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="23731-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="23731-127">アクションは、パラメーターと戻り値の型がサービス メタデータで説明されていることができます。</span><span class="sxs-lookup"><span data-stu-id="23731-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="23731-128">クライアントが要求本文でパラメーターを送信し、サーバーが応答本文で、戻り値を送信します。</span><span class="sxs-lookup"><span data-stu-id="23731-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="23731-129">「製品の単価」アクションを呼び出すには、クライアントで、次のような URI に POST を送信します。</span><span class="sxs-lookup"><span data-stu-id="23731-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="23731-130">POST 要求内のデータは、製品の評価だけです。</span><span class="sxs-lookup"><span data-stu-id="23731-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="23731-131">エンティティ データ モデルのアクションを宣言します。</span><span class="sxs-lookup"><span data-stu-id="23731-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="23731-132">Web API 構成には、entity data model (EDM) にアクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="23731-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="23731-133">このコードは、製品のエンティティで実行できるアクションとして"RateProduct"を定義します。</span><span class="sxs-lookup"><span data-stu-id="23731-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="23731-134">アクションが受け取ることも宣言、 **int**パラメーター「評価」という名前を返します、 **int**値。</span><span class="sxs-lookup"><span data-stu-id="23731-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="23731-135">コント ローラーに、アクションを追加します。</span><span class="sxs-lookup"><span data-stu-id="23731-135">Add the Action to the Controller</span></span>

<span data-ttu-id="23731-136">"RateProduct"アクションは、Product エンティティにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="23731-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="23731-137">アクションを実装するには、という名前のメソッドを追加`RateProduct`Products コント ローラーにします。</span><span class="sxs-lookup"><span data-stu-id="23731-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="23731-138">メソッド名が、EDM 内のアクションの名前と一致することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="23731-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="23731-139">メソッドでは、2 つのパラメーターがあります。</span><span class="sxs-lookup"><span data-stu-id="23731-139">The method has two parameters:</span></span>

- <span data-ttu-id="23731-140">*キー*: レートに製品のキー。</span><span class="sxs-lookup"><span data-stu-id="23731-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="23731-141">*パラメーター*: アクション パラメーターの値のディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="23731-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="23731-142">既定のルーティング規約を使用している場合、キー パラメーターを「キー」名前する必要があります。</span><span class="sxs-lookup"><span data-stu-id="23731-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="23731-143">必ず、 **[FromOdataUri]** 属性が示すようにします。</span><span class="sxs-lookup"><span data-stu-id="23731-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="23731-144">この属性では、Web API 要求 URI からキーを解析するときに、OData 構文の規則を使用するように指示します。</span><span class="sxs-lookup"><span data-stu-id="23731-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="23731-145">使用して、*パラメーター*アクション パラメーターを取得するためのディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="23731-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="23731-146">クライアントが適切で、アクション パラメーターを送信する場合の値の書式**ModelState.IsValid**は true。</span><span class="sxs-lookup"><span data-stu-id="23731-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="23731-147">その場合は、使用することができます、 **ODataActionParameters**パラメーターの値を取得するためのディクショナリ。</span><span class="sxs-lookup"><span data-stu-id="23731-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="23731-148">この例で、`RateProduct`アクションは、「評価」という名前の 1 つのパラメーターを受け取る。</span><span class="sxs-lookup"><span data-stu-id="23731-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="23731-149">アクションのメタデータ</span><span class="sxs-lookup"><span data-stu-id="23731-149">Action Metadata</span></span>

<span data-ttu-id="23731-150">サービス メタデータを表示するには、/odata/$ メタデータに、GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="23731-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="23731-151">宣言するメタデータの一部を次に示します、`RateProduct`アクション。</span><span class="sxs-lookup"><span data-stu-id="23731-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="23731-152">**FunctionImport**要素は、アクションを宣言します。</span><span class="sxs-lookup"><span data-stu-id="23731-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="23731-153">ほとんどのフィールドは、一目瞭然ですが、注目すべき 2 つの。</span><span class="sxs-lookup"><span data-stu-id="23731-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="23731-154">**IsBindable**意味、アクションを少なくとも、ターゲット エンティティに呼び出すことが、時間の一部です。</span><span class="sxs-lookup"><span data-stu-id="23731-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="23731-155">**IsAlwaysBindable**ターゲット エンティティに常に、操作を呼び出せることを意味します。</span><span class="sxs-lookup"><span data-stu-id="23731-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="23731-156">違いは、いくつかの操作は、クライアントで使用可能では常が、その他のアクションがエンティティの状態によって異なります。</span><span class="sxs-lookup"><span data-stu-id="23731-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="23731-157">たとえば、[購入] アクションを定義するとします。</span><span class="sxs-lookup"><span data-stu-id="23731-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="23731-158">在庫にあるアイテムのみ購入できます。</span><span class="sxs-lookup"><span data-stu-id="23731-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="23731-159">項目が在庫切れになった場合、クライアントはそのアクションを呼び出すことはできません。</span><span class="sxs-lookup"><span data-stu-id="23731-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="23731-160">EDM を定義するときに、**アクション**メソッドは、常にバインド可能なアクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="23731-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="23731-161">Not を常に、バインド可能なアクションについて説明します (とも呼ばれる*一時的な*アクション) このトピックで後述します。</span><span class="sxs-lookup"><span data-stu-id="23731-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="23731-162">アクションを呼び出す</span><span class="sxs-lookup"><span data-stu-id="23731-162">Invoking the Action</span></span>

<span data-ttu-id="23731-163">これで、クライアントはこの操作を呼び出す方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="23731-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="23731-164">ID の製品への 2 の評価を付与する必要があると 4 を = です。</span><span class="sxs-lookup"><span data-stu-id="23731-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="23731-165">要求本文の JSON 形式を使用して、例要求メッセージを次に示します。</span><span class="sxs-lookup"><span data-stu-id="23731-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="23731-166">応答メッセージを次に示します。</span><span class="sxs-lookup"><span data-stu-id="23731-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="23731-167">エンティティ セットへのアクションのバインド</span><span class="sxs-lookup"><span data-stu-id="23731-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="23731-168">前の例では、アクションが 1 つのエンティティにバインドされて: クライアントは、1 つの製品を評価します。</span><span class="sxs-lookup"><span data-stu-id="23731-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="23731-169">アクションは、エンティティのコレクションにバインドすることもできます。</span><span class="sxs-lookup"><span data-stu-id="23731-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="23731-170">次の変更を加えるだけです。</span><span class="sxs-lookup"><span data-stu-id="23731-170">Just make the following changes:</span></span>

<span data-ttu-id="23731-171">EDM では、アクションを追加するエンティティの**コレクション**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="23731-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="23731-172">コント ローラー メソッドで省略、*キー*パラメーター。</span><span class="sxs-lookup"><span data-stu-id="23731-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="23731-173">クライアントが、製品のエンティティ セットに対して操作を呼び出すようになりました。</span><span class="sxs-lookup"><span data-stu-id="23731-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="23731-174">コレクションのパラメーターを使って操作</span><span class="sxs-lookup"><span data-stu-id="23731-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="23731-175">アクションには、パラメーター値のコレクションを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="23731-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="23731-176">EDM では、次のように使用します。 **CollectionParameter&lt;T&gt;** パラメーターを宣言します。</span><span class="sxs-lookup"><span data-stu-id="23731-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="23731-177">これは、コレクションを受け取り「評価」という名前のパラメーターを宣言します。 **int**値。</span><span class="sxs-lookup"><span data-stu-id="23731-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="23731-178">パラメーター値の取得も、コント ローラー メソッドで、 **ODataActionParameters**オブジェクトが、値のようになりましたが、 **ICollection&lt;int&gt;** 値。</span><span class="sxs-lookup"><span data-stu-id="23731-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="23731-179">一時的なアクション</span><span class="sxs-lookup"><span data-stu-id="23731-179">Transient Actions</span></span>

<span data-ttu-id="23731-180">"RateProduct"例では、アクションが常に使用できるように、製品の場所をユーザーが評価常にできます。</span><span class="sxs-lookup"><span data-stu-id="23731-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="23731-181">いくつかの操作は、エンティティの状態によって異なります。</span><span class="sxs-lookup"><span data-stu-id="23731-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="23731-182">たとえば、ビデオ レンタル サービスで「チェック アウト」アクションは常に使用できません。</span><span class="sxs-lookup"><span data-stu-id="23731-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="23731-183">(依存するビデオのコピーが使用できるかどうか。)この種類のアクションが呼び出されます、*一時的な*アクション。</span><span class="sxs-lookup"><span data-stu-id="23731-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="23731-184">一時的なアクションには、サービス メタデータで**IsAlwaysBindable**を false に等しい。</span><span class="sxs-lookup"><span data-stu-id="23731-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="23731-185">メタデータのようになりますので、実際には、既定値です。</span><span class="sxs-lookup"><span data-stu-id="23731-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="23731-186">ここのこれが重要な理由: サーバーが、アクションが使用可能な場合、クライアントに指示する必要がありますアクションが一時的である場合。</span><span class="sxs-lookup"><span data-stu-id="23731-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="23731-187">この機能を使用するには、エンティティのアクションへのリンクを含みます。</span><span class="sxs-lookup"><span data-stu-id="23731-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="23731-188">ムービーのエンティティの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="23731-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="23731-189">"#CheckOut"プロパティには、チェック アウトの操作へのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="23731-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="23731-190">アクションが使用できない場合、サーバーは、リンクを省略します。</span><span class="sxs-lookup"><span data-stu-id="23731-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="23731-191">EDM では一時的なアクションを宣言するには、呼び出し、 **TransientAction**メソッド。</span><span class="sxs-lookup"><span data-stu-id="23731-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="23731-192">またを特定のエンティティのアクション リンクを返す関数を用意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="23731-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="23731-193">この関数を呼び出すことによって設定**HasActionLink**します。</span><span class="sxs-lookup"><span data-stu-id="23731-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="23731-194">ラムダ式として関数を記述できます。</span><span class="sxs-lookup"><span data-stu-id="23731-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="23731-195">アクションを使用できる場合、ラムダ式は、アクションにリンクを返します。</span><span class="sxs-lookup"><span data-stu-id="23731-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="23731-196">OData シリアライザーには、エンティティをシリアル化時に、このリンクが含まれます。</span><span class="sxs-lookup"><span data-stu-id="23731-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="23731-197">アクションが利用できない場合、関数を返します`null`します。</span><span class="sxs-lookup"><span data-stu-id="23731-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="23731-198">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="23731-198">Additional Resources</span></span>

[<span data-ttu-id="23731-199">OData アクションのサンプル</span><span class="sxs-lookup"><span data-stu-id="23731-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
