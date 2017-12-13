---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: "JavaScript クライアントを作成 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b397c5a413ae213c9b79da1c0e0626efe21c7e21
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="create-the-javascript-client"></a><span data-ttu-id="ccefc-102">JavaScript クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="ccefc-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ccefc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ccefc-104">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="ccefc-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="ccefc-105">このセクションでは、HTML、JavaScript を使用して、アプリケーション用のクライアントを作成し、 [Knockout.js](http://knockoutjs.com/)ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="ccefc-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="ccefc-106">クライアント アプリのビルドは段階的に。</span><span class="sxs-lookup"><span data-stu-id="ccefc-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="ccefc-107">書籍の一覧を表示しています。</span><span class="sxs-lookup"><span data-stu-id="ccefc-107">Showing a list of books.</span></span>
- <span data-ttu-id="ccefc-108">書籍の詳細を表示しています。</span><span class="sxs-lookup"><span data-stu-id="ccefc-108">Showing a book detail.</span></span>
- <span data-ttu-id="ccefc-109">新しいブックを追加します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-109">Adding a new book.</span></span>

<span data-ttu-id="ccefc-110">Knockout ライブラリは、モデル View-viewmodel (MVVM) パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="ccefc-111">**モデル**(当社の場合、ブックおよび作成者) に、ビジネス ドメイン内のデータのサーバー側表現です。</span><span class="sxs-lookup"><span data-stu-id="ccefc-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="ccefc-112">**ビュー**は、プレゼンテーション層 (HTML)。</span><span class="sxs-lookup"><span data-stu-id="ccefc-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="ccefc-113">**ビュー モデル**JavaScript オブジェクト モデルを保持します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="ccefc-114">ビュー モデルは、UI のコードの抽象化です。</span><span class="sxs-lookup"><span data-stu-id="ccefc-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="ccefc-115">HTML 形式の知識がないです。</span><span class="sxs-lookup"><span data-stu-id="ccefc-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="ccefc-116">代わりに、そのなどを表す抽象機能、ビューの&quot;書籍の一覧&quot;です。</span><span class="sxs-lookup"><span data-stu-id="ccefc-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="ccefc-117">ビュー データにバインドされてビュー モデルです。</span><span class="sxs-lookup"><span data-stu-id="ccefc-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="ccefc-118">ビュー モデルへの更新は、ビューに自動的に反映されます。</span><span class="sxs-lookup"><span data-stu-id="ccefc-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="ccefc-119">ビュー モデルは、ボタンのクリックしてなどもイベントと、ビューから取得します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="ccefc-120">この方法が便利レイアウトと、アプリの UI を変更する任意のコードを書き直すことがなく、バインドを変更できるためです。</span><span class="sxs-lookup"><span data-stu-id="ccefc-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="ccefc-121">たとえば、としてアイテムの一覧を表示する場合があります、 `<ul>`、後で、テーブルに変更します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="ccefc-122">Knockout ライブラリを追加します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-122">Add the Knockout Library</span></span>

<span data-ttu-id="ccefc-123">Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**です。</span><span class="sxs-lookup"><span data-stu-id="ccefc-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="ccefc-124">選択し、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="ccefc-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="ccefc-125">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="ccefc-126">このコマンドは、Scripts フォルダーに Knockout ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="ccefc-127">ビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-127">Create the View Model</span></span>

<span data-ttu-id="ccefc-128">スクリプト フォルダーに app.js をという名前の JavaScript ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="ccefc-129">(ソリューション エクスプ ローラーで、Scripts フォルダーを右クリックし、選択**追加**選択してから、 **JavaScript ファイル**)。次のコードを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="ccefc-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="ccefc-130">Knockout で、`observable`クラスは、データ バインディングを使用します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="ccefc-131">監視対象の内容を変更するときに、観測可能なオブジェクトに通知のすべてのデータ バインド コントロール自体を更新できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ccefc-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="ccefc-132">(、`observableArray`クラスは、配列のバージョンの*observable*)。開始するには、ビュー モデルには、次の 2 つの観測可能なオブジェクトがあります。</span><span class="sxs-lookup"><span data-stu-id="ccefc-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="ccefc-133">`books`書籍の一覧を保持します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="ccefc-134">`error`AJAX 呼び出しが失敗した場合は、エラー メッセージに含まれています。</span><span class="sxs-lookup"><span data-stu-id="ccefc-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="ccefc-135">`getAllBooks`メソッドは、書籍の一覧を取得するための AJAX 呼び出しを使用します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="ccefc-136">結果をプッシュし、`books`配列。</span><span class="sxs-lookup"><span data-stu-id="ccefc-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="ccefc-137">`ko.applyBindings`メソッドは、Knockout ライブラリの一部です。</span><span class="sxs-lookup"><span data-stu-id="ccefc-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="ccefc-138">ビュー モデルとしてパラメーターを受け取るし、データ バインディングを設定します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="ccefc-139">スクリプト バンドルを追加します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-139">Add a Script Bundle</span></span>

<span data-ttu-id="ccefc-140">バンドルは、ASP.NET 4.5 を結合または 1 つのファイルを複数のファイルにバンドルしやすく機能です。</span><span class="sxs-lookup"><span data-stu-id="ccefc-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="ccefc-141">ページ読み込み時間を向上させることができるサーバーに要求の数を削減するバンドルします。</span><span class="sxs-lookup"><span data-stu-id="ccefc-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="ccefc-142">アプリのファイルを開く\_Start/BundleConfig.cs です。</span><span class="sxs-lookup"><span data-stu-id="ccefc-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="ccefc-143">RegisterBundles メソッドに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="ccefc-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

>[!div class="step-by-step"]
<span data-ttu-id="ccefc-144">[前へ](part-5.md)
[次へ](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="ccefc-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
