---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Details メソッドと Delete メソッドを調べること |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: ce4413f81e49c1467b574347f69f855ae4ba5bf8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839048"
---
<a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="87ff6-102">Details メソッドと Delete メソッドを調べる</span><span class="sxs-lookup"><span data-stu-id="87ff6-102">Examining the Details and Delete Methods</span></span>
====================
<span data-ttu-id="87ff6-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="87ff6-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="87ff6-104">このチュートリアルでは、自動的に生成された調べます`Details`と`Delete`メソッド。</span><span class="sxs-lookup"><span data-stu-id="87ff6-104">In this part of the tutorial, you'll examine the automatically generated `Details` and `Delete` methods.</span></span>

## <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="87ff6-105">Details メソッドと Delete メソッドを調べる</span><span class="sxs-lookup"><span data-stu-id="87ff6-105">Examining the Details and Delete Methods</span></span>

<span data-ttu-id="87ff6-106">開く、`Movie`コント ローラーを調べ、`Details`メソッド。</span><span class="sxs-lookup"><span data-stu-id="87ff6-106">Open the `Movie` controller and examine the `Details` method.</span></span>

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

<span data-ttu-id="87ff6-107">このアクション メソッドを作成した MVC スキャフォールディング エンジンは、メソッドを呼び出す HTTP 要求を示すコメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-107">The MVC scaffolding engine that created this action method adds a comment showing a HTTP request that invokes the method.</span></span> <span data-ttu-id="87ff6-108">ここでは、 `GET` 3 つの URL セグメントで要求を`Movies`コント ローラー、`Details`メソッドと`ID`値。</span><span class="sxs-lookup"><span data-stu-id="87ff6-108">In this case it's a `GET` request with three URL segments, the `Movies` controller, the `Details` method and a `ID` value.</span></span>

<span data-ttu-id="87ff6-109">コード最初に簡単にデータを使用して検索、`Find`メソッド。</span><span class="sxs-lookup"><span data-stu-id="87ff6-109">Code First makes it easy to search for data using the `Find` method.</span></span> <span data-ttu-id="87ff6-110">メソッドに組み込まれている、重要なセキュリティ機能は、コードが確認します、`Find`メソッドは、コードが、それ以降を実行する前に、ムービーが検出されます。</span><span class="sxs-lookup"><span data-stu-id="87ff6-110">An important security feature built into the method is that the code verifies that the `Find` method has found a movie before the code tries to do anything with it.</span></span> <span data-ttu-id="87ff6-111">たとえば、ハッカーでしたにエラーが発生、サイトからのリンクが作成する URL を変更することで`http://localhost:xxxx/Movies/Details/1`ようなものに`http://localhost:xxxx/Movies/Details/12345`(または実際のムービーを表していないその他のいくつかの値)。</span><span class="sxs-lookup"><span data-stu-id="87ff6-111">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="87ff6-112">Null のムービーを確認しなかった、null のムービーのデータベース エラーになります。</span><span class="sxs-lookup"><span data-stu-id="87ff6-112">If you did not check for a null movie, a null movie would result in a database error.</span></span>

<span data-ttu-id="87ff6-113">`Delete` メソッドと `DeleteConfirmed` メソッドを調べます。</span><span class="sxs-lookup"><span data-stu-id="87ff6-113">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

<span data-ttu-id="87ff6-114">HTTP get`Delete`メソッドは、指定されたビデオを削除しないを送信することができます、ムービーのビューを返します (`HttpPost`)、削除します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-114">Note that the HTTP GET `Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (`HttpPost`) the deletion.</span></span> <span data-ttu-id="87ff6-115">GET 要求の応答で削除操作を実行すると (さらに言えば、編集操作、作成操作、データを変更するその他のあらゆる操作を実行すると)、セキュリティに穴が空きます。</span><span class="sxs-lookup"><span data-stu-id="87ff6-115">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span> <span data-ttu-id="87ff6-116">詳細については、Stephen Walther のブログ記事を参照してください。 [ASP.NET MVC ヒントと 46: セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-116">For more information about this, see Stephen Walther's blog entry [ASP.NET MVC Tip #46 — Don't use Delete Links because they create Security Holes](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).</span></span>

<span data-ttu-id="87ff6-117">データを削除する `HttpPost` メソッドの名前は「`DeleteConfirmed`」になり、HTTP POST メソッドに一意のシグネチャまたは名前が与えられます。</span><span class="sxs-lookup"><span data-stu-id="87ff6-117">The `HttpPost` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="87ff6-118">2 つのメソッド シグネチャは下の画像のようになります。</span><span class="sxs-lookup"><span data-stu-id="87ff6-118">The two method signatures are shown below:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

<span data-ttu-id="87ff6-119">共通言語ランタイム (CLR) は、オーバーロードのメソッドに一意のパラメーター シグネチャを持つことを要求します (メソッド名は同じであるが、パラメーターの一覧が異なる)。</span><span class="sxs-lookup"><span data-stu-id="87ff6-119">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="87ff6-120">ただし、ここで必要 2 つ削除メソッド--GET の 1 つ--、投稿の両方が同じパラメーター シグネチャを持つこと。</span><span class="sxs-lookup"><span data-stu-id="87ff6-120">However, here you need two Delete methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="87ff6-121">(いずれも、1 つの整数をパラメーターとして受け取る必要があります。)</span><span class="sxs-lookup"><span data-stu-id="87ff6-121">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="87ff6-122">この出力を並べ替えるには、いくつかの点を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="87ff6-122">To sort this out, you can do a couple of things.</span></span> <span data-ttu-id="87ff6-123">メソッドの別の名前を付けて 1 つです。</span><span class="sxs-lookup"><span data-stu-id="87ff6-123">One is to give the methods different names.</span></span> <span data-ttu-id="87ff6-124">先の例では、スキャフォールディング メカニズムがこれを行いました。</span><span class="sxs-lookup"><span data-stu-id="87ff6-124">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="87ff6-125">ただし、これは小さな問題を引き起こします。ASP.NET が URL のセグメントをアクション メソッドに名前でマッピングします。メソッドの名前を変更すると、通常、ルーティングでそのメソッドが見つからなくなります。</span><span class="sxs-lookup"><span data-stu-id="87ff6-125">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="87ff6-126">この解決策はこの例で確認できます。`ActionName("Delete")` 属性を `DeleteConfirmed` メソッドに追加しています。</span><span class="sxs-lookup"><span data-stu-id="87ff6-126">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="87ff6-127">これが効果的に実行マッピング、ルーティング システムの URL を含むように */delete/* 要求を検索する投稿について、`DeleteConfirmed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="87ff6-127">This effectively performs mapping for the routing system so that a URL that includes */Delete/* for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="87ff6-128">同じ名前とシグネチャを持つメソッドでの問題を回避するもう 1 つの一般的な方法では、意図的に未使用のパラメーターを含める POST メソッドのシグネチャを変更します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-128">Another common way to avoid a problem with methods that have identical names and signatures is to artificially change the signature of the POST method to include an unused parameter.</span></span> <span data-ttu-id="87ff6-129">たとえば、一部の開発者がパラメーターの型を追加`FormCollection`は POST メソッドに渡されると、単にパラメーターを使用しません。</span><span class="sxs-lookup"><span data-stu-id="87ff6-129">For example, some developers add a parameter type `FormCollection` that is passed to the POST method, and then simply don't use the parameter:</span></span>

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a><span data-ttu-id="87ff6-130">まとめ</span><span class="sxs-lookup"><span data-stu-id="87ff6-130">Summary</span></span>

<span data-ttu-id="87ff6-131">ローカル DB データベースにデータを格納する完全な ASP.NET MVC アプリケーションがあるようになりました。</span><span class="sxs-lookup"><span data-stu-id="87ff6-131">You now have a complete ASP.NET MVC application that stores data in a local DB database.</span></span> <span data-ttu-id="87ff6-132">ことができますを作成、読み取り、更新、削除、および映画を検索します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-132">You can create, read, update, delete, and search for movies.</span></span>

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a><span data-ttu-id="87ff6-133">次の手順</span><span class="sxs-lookup"><span data-stu-id="87ff6-133">Next Steps</span></span>

<span data-ttu-id="87ff6-134">ビルドして、web アプリケーションをテストした後、次の手順は、インターネット経由で使用するには、他のユーザーが使用できるようにするは。</span><span class="sxs-lookup"><span data-stu-id="87ff6-134">After you have built and tested a web application, the next step is to make it available to other people to use over the Internet.</span></span> <span data-ttu-id="87ff6-135">そのためには、web ホスティング プロバイダーにデプロイするがあります。</span><span class="sxs-lookup"><span data-stu-id="87ff6-135">To do that, you have to deploy it to a web hosting provider.</span></span> <span data-ttu-id="87ff6-136">マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、[無料 Azure 試用版アカウント](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-136">Microsoft offers free web hosting for up to 10 web sites in a [free Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="87ff6-137">拙著のチュートリアルの次の利用をお勧めします[メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC アプリを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-137">I suggest you next follow my tutorial [Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="87ff6-138">優れたチュートリアルは、Tom Dykstra の中間レベル[、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-138">An excellent tutorial is Tom Dykstra's intermediate-level [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="87ff6-139">[Stackoverflow](http://stackoverflow.com/help)と[ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)は質問への優れたを配置します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-139">[Stackoverflow](http://stackoverflow.com/help) and the [ASP.NET MVC forums](https://forums.asp.net/1146.aspx) are a great places to ask questions.</span></span> <span data-ttu-id="87ff6-140">次の[me](https://twitter.com/RickAndMSFT) twitter で最新のチュートリアルでは、更新プログラムを取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="87ff6-140">Follow [me](https://twitter.com/RickAndMSFT) on twitter so you can get updates on my latest tutorials.</span></span>

<span data-ttu-id="87ff6-141">フィードバックを歓迎します。</span><span class="sxs-lookup"><span data-stu-id="87ff6-141">Feedback is welcome.</span></span>

<span data-ttu-id="87ff6-142">— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="87ff6-142">— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)</span></span>  
<span data-ttu-id="87ff6-143">— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="87ff6-143">— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="87ff6-144">前へ</span><span class="sxs-lookup"><span data-stu-id="87ff6-144">Previous</span></span>](adding-validation.md)
