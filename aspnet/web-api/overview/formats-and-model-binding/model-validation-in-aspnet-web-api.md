---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: ASP.NET Web API でのモデル検証 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 75474d5597ba11e09f788269b9a2a278559c63a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393097"
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="be52a-102">ASP.NET Web API でモデルの検証</span><span class="sxs-lookup"><span data-stu-id="be52a-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="be52a-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="be52a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="be52a-104">クライアントは、web API へのデータを送信するときに多くの場合、検証するデータ、処理を実行する前にします。</span><span class="sxs-lookup"><span data-stu-id="be52a-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="be52a-105">この記事では、モデルの注釈を設定、データの検証のため、注釈を使用して、web API で検証エラーを処理する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="be52a-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="be52a-106">データの注釈</span><span class="sxs-lookup"><span data-stu-id="be52a-106">Data Annotations</span></span>

<span data-ttu-id="be52a-107">ASP.NET Web api からの属性を使用することができます、 [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)名前空間をモデルに検証規則のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="be52a-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="be52a-108">次のようなモデルを検討してください。</span><span class="sxs-lookup"><span data-stu-id="be52a-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="be52a-109">ASP.NET MVC のモデルの検証を使用している場合この使い慣れたになります。</span><span class="sxs-lookup"><span data-stu-id="be52a-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="be52a-110">**必要**属性という、`Name`プロパティを null にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="be52a-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="be52a-111">**範囲**属性という`Weight`0 ~ 999 にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="be52a-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="be52a-112">クライアントが次の JSON 表現を含む POST 要求を送信するとします。</span><span class="sxs-lookup"><span data-stu-id="be52a-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="be52a-113">クライアントが含まれていないことを確認できます、`Name`としてマークされているプロパティが必要です。</span><span class="sxs-lookup"><span data-stu-id="be52a-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="be52a-114">Web API に JSON を変換するときに、`Product`インスタンス、検証、`Product`検証属性に対して。</span><span class="sxs-lookup"><span data-stu-id="be52a-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="be52a-115">コント ローラー アクションでは、モデルが有効かどうかを確認できます。</span><span class="sxs-lookup"><span data-stu-id="be52a-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="be52a-116">モデルの検証では、クライアント データが安全であるは保証されません。</span><span class="sxs-lookup"><span data-stu-id="be52a-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="be52a-117">追加の検証は、アプリケーションの他のレイヤーで必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="be52a-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="be52a-118">(たとえば、データ層可能性があります外部キー制約を適用します。)このチュートリアル[Entity Framework で Web API を使用して](../data/using-web-api-with-entity-framework/part-1.md)これらの問題のいくつか解説します。</span><span class="sxs-lookup"><span data-stu-id="be52a-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="be52a-119">**「過小投稿」**: 過小転記クライアントがいくつかのプロパティを離れるときの動作。</span><span class="sxs-lookup"><span data-stu-id="be52a-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="be52a-120">たとえば、クライアントは、次に送信します。</span><span class="sxs-lookup"><span data-stu-id="be52a-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="be52a-121">ここでは、クライアントがの値を指定しなかった`Price`または`Weight`します。</span><span class="sxs-lookup"><span data-stu-id="be52a-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="be52a-122">JSON フォーマッタは、0 不足しているプロパティからの既定値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="be52a-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="be52a-123">これらのプロパティの有効な値は 0 ですので、モデルの状態は有効です。</span><span class="sxs-lookup"><span data-stu-id="be52a-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="be52a-124">これは問題であるかどうかは、シナリオによって異なります。</span><span class="sxs-lookup"><span data-stu-id="be52a-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="be52a-125">たとえば、更新操作では、可能性がある「0」と「設定なし」とを区別</span><span class="sxs-lookup"><span data-stu-id="be52a-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="be52a-126">値を設定するクライアントを強制的に設定して null 許容にプロパティを作成、**必要**属性。</span><span class="sxs-lookup"><span data-stu-id="be52a-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="be52a-127">**「オーバーポスティング」**: クライアントが送信も*詳細*が予想よりもデータ。</span><span class="sxs-lookup"><span data-stu-id="be52a-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="be52a-128">例えば:</span><span class="sxs-lookup"><span data-stu-id="be52a-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="be52a-129">JSON に存在しないプロパティ ("Color") が含まれていますここでは、`Product`モデル。</span><span class="sxs-lookup"><span data-stu-id="be52a-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="be52a-130">この場合は、JSON フォーマッタは、この値を無視するだけです。</span><span class="sxs-lookup"><span data-stu-id="be52a-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="be52a-131">(XML フォーマッタは同じです。)オーバーポスティング攻撃では、モデルに読み取り専用にしようとしたプロパティが設定されている場合に問題が発生します。</span><span class="sxs-lookup"><span data-stu-id="be52a-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="be52a-132">例えば:</span><span class="sxs-lookup"><span data-stu-id="be52a-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="be52a-133">ユーザーを更新しない、`IsAdmin`プロパティ自体を管理者に昇格させるとします。</span><span class="sxs-lookup"><span data-stu-id="be52a-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="be52a-134">最も安全な戦略を送信するクライアントが許可されていると完全に一致するモデル クラスを使用することです。</span><span class="sxs-lookup"><span data-stu-id="be52a-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="be52a-135">Brad Wilson のブログの投稿"[入力の検証とします。ASP.NET MVC でのモデル検証](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"が過少投稿およびオーバーポスティング攻撃の詳細。</span><span class="sxs-lookup"><span data-stu-id="be52a-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="be52a-136">投稿は、ASP.NET MVC 2 の詳細については、問題は、Web API にも関連します。</span><span class="sxs-lookup"><span data-stu-id="be52a-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="be52a-137">検証エラーの処理</span><span class="sxs-lookup"><span data-stu-id="be52a-137">Handling Validation Errors</span></span>

<span data-ttu-id="be52a-138">Web API は、検証が失敗したときに、クライアントにエラーを自動的に返しません。</span><span class="sxs-lookup"><span data-stu-id="be52a-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="be52a-139">モデルの状態を確認し、適切に対応するコント ローラー アクションの責任です。</span><span class="sxs-lookup"><span data-stu-id="be52a-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="be52a-140">コント ローラー アクションが呼び出される前に、モデルの状態を確認するアクション フィルターを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="be52a-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="be52a-141">次のコード例に示します。</span><span class="sxs-lookup"><span data-stu-id="be52a-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="be52a-142">モデルの検証に失敗した場合、このフィルターは、検証エラーを含む HTTP 応答を返します。</span><span class="sxs-lookup"><span data-stu-id="be52a-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="be52a-143">その場合は、コント ローラー アクションは呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="be52a-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="be52a-144">すべての Web API コント ローラーにこのフィルターを適用するフィルターのインスタンスを追加、 **HttpConfiguration.Filters**構成中にコレクション。</span><span class="sxs-lookup"><span data-stu-id="be52a-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="be52a-145">別のオプションでは、個々 のコント ローラーまたはコント ローラー アクションに属性として、フィルターを設定します。</span><span class="sxs-lookup"><span data-stu-id="be52a-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
