---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: "モデルの ASP.NET Web API での検証 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: dc91ddb64294e686825076d5bcc636766f2f6f01
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="cafec-102">ASP.NET Web API でのモデルの検証</span><span class="sxs-lookup"><span data-stu-id="cafec-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="cafec-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cafec-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cafec-104">クライアントは、web API へのデータを送信するときに多くの場合、検証するデータ処理を実行する前にします。</span><span class="sxs-lookup"><span data-stu-id="cafec-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="cafec-105">この記事に、モデルの注釈を付け、データ検証のため、注釈を使用して、web API で検証エラーの処理方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cafec-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cafec-106">データの注釈</span><span class="sxs-lookup"><span data-stu-id="cafec-106">Data Annotations</span></span>

<span data-ttu-id="cafec-107">ASP.NET Web API から属性を使用することができます、 [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx)名前空間をモデルに検証規則のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="cafec-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="cafec-108">次のようなモデルを考慮してください。</span><span class="sxs-lookup"><span data-stu-id="cafec-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="cafec-109">ASP.NET MVC のモデルの検証を使用した場合、使い慣れたこれがはずです。</span><span class="sxs-lookup"><span data-stu-id="cafec-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="cafec-110">**必要**属性を表示する、`Name`プロパティを null することはできません。</span><span class="sxs-lookup"><span data-stu-id="cafec-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="cafec-111">**範囲**いる属性という`Weight`0 ~ 999 でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="cafec-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="cafec-112">クライアントが次の JSON 表現で POST 要求を送信するとします。</span><span class="sxs-lookup"><span data-stu-id="cafec-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="cafec-113">クライアントが含まれていないことを確認できます、`Name`としてマークされているプロパティが必要です。</span><span class="sxs-lookup"><span data-stu-id="cafec-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="cafec-114">Web API に JSON を変換するときに、`Product`インスタンスを検証して、`Product`検証の属性に対してです。</span><span class="sxs-lookup"><span data-stu-id="cafec-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="cafec-115">コント ローラー動作では、モデルが有効かどうかを確認できます。</span><span class="sxs-lookup"><span data-stu-id="cafec-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="cafec-116">モデルの検証では、クライアントのデータを安全なことは保証されません。</span><span class="sxs-lookup"><span data-stu-id="cafec-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="cafec-117">追加の検証は、アプリケーションの他のレイヤーで必要な可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cafec-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="cafec-118">(たとえば、データ層可能性があります外部キー制約を適用します。)チュートリアル[Entity Framework と Web API を使用して](../data/using-web-api-with-entity-framework/part-1.md)これらの問題について説明します。</span><span class="sxs-lookup"><span data-stu-id="cafec-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="cafec-119">**「過少転記」**: 過少ポストされた内容の場合は、クライアントがいくつかのプロパティを残します。</span><span class="sxs-lookup"><span data-stu-id="cafec-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="cafec-120">たとえば、クライアントは、次に送信します。</span><span class="sxs-lookup"><span data-stu-id="cafec-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="cafec-121">ここでは、クライアントがの値を指定しなかった`Price`または`Weight`です。</span><span class="sxs-lookup"><span data-stu-id="cafec-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="cafec-122">JSON フォーマッタは、0、不足しているプロパティを既定値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="cafec-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="cafec-123">モデルの状態は無効は、これらのプロパティの有効な値は 0 です。</span><span class="sxs-lookup"><span data-stu-id="cafec-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="cafec-124">これは問題であるかどうかは、シナリオによって異なります。</span><span class="sxs-lookup"><span data-stu-id="cafec-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="cafec-125">たとえば、操作では、更新、可能性がある"zero"と"されていません を区別するには</span><span class="sxs-lookup"><span data-stu-id="cafec-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="cafec-126">値を設定するクライアントに強制するように、プロパティの null 値を許容し、設定、**必要**属性。</span><span class="sxs-lookup"><span data-stu-id="cafec-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="cafec-127">**"過剰ポスティング**: クライアントが送信も*詳細*予想以上にデータ。</span><span class="sxs-lookup"><span data-stu-id="cafec-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="cafec-128">例:</span><span class="sxs-lookup"><span data-stu-id="cafec-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="cafec-129">ここでは、JSON がの存在しないプロパティ ("Color") が含まれます、`Product`モデル。</span><span class="sxs-lookup"><span data-stu-id="cafec-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="cafec-130">この例では、JSON フォーマッタでは、単にこの値を無視します。</span><span class="sxs-lookup"><span data-stu-id="cafec-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="cafec-131">(XML フォーマッタは同じです。)過剰な投稿では、モデルにプロパティが読み取り専用にしようとしたものがある場合に問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="cafec-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="cafec-132">例:</span><span class="sxs-lookup"><span data-stu-id="cafec-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="cafec-133">ユーザーを更新しない、`IsAdmin`プロパティ自体を管理者に昇格させると!</span><span class="sxs-lookup"><span data-stu-id="cafec-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="cafec-134">最も安全な方法は、送信するクライアントが許可されていると一致するモデル クラスを使用するは。</span><span class="sxs-lookup"><span data-stu-id="cafec-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="cafec-135">Brad Wilson のブログの投稿"[入力検証とします。ASP.NET MVC での検証をモデル](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"過少ポストされた内容と過剰ポストされた内容の詳細がします。</span><span class="sxs-lookup"><span data-stu-id="cafec-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="cafec-136">投稿は、ASP.NET MVC 2 については、問題が Web API にも関係します。</span><span class="sxs-lookup"><span data-stu-id="cafec-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="cafec-137">検証エラーの処理</span><span class="sxs-lookup"><span data-stu-id="cafec-137">Handling Validation Errors</span></span>

<span data-ttu-id="cafec-138">Web API は、検証が失敗したときに、クライアントにエラーを自動的に返しません。</span><span class="sxs-lookup"><span data-stu-id="cafec-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="cafec-139">モデルの状態を確認し、適切に対応するコント ローラー アクションの責任です。</span><span class="sxs-lookup"><span data-stu-id="cafec-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="cafec-140">コント ローラーのアクションが呼び出される前に、モデルの状態を確認するアクション フィルターを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="cafec-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="cafec-141">次のコード例に示します。</span><span class="sxs-lookup"><span data-stu-id="cafec-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="cafec-142">モデルの検証に失敗した場合、このフィルターには、検証エラーを含む HTTP 応答が返されます。</span><span class="sxs-lookup"><span data-stu-id="cafec-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="cafec-143">その場合は、コント ローラーのアクションは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="cafec-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="cafec-144">すべての Web API コント ローラーにフィルターを適用するフィルターのインスタンスに追加、 **HttpConfiguration.Filters**構成中にコレクション。</span><span class="sxs-lookup"><span data-stu-id="cafec-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="cafec-145">個々 のコント ローラーまたはコント ローラーのアクションを属性としてフィルターを設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="cafec-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
