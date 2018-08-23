---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: JavaScript クライアントの作成 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 4967e21190c34f698e9c28fd9b921f07bef2ffaf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831310"
---
<a name="create-the-javascript-client"></a><span data-ttu-id="f658e-102">JavaScript クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="f658e-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="f658e-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f658e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f658e-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="f658e-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="f658e-105">このセクションでは、HTML、JavaScript を使用して、アプリケーションのクライアントを作成して、 [Knockout.js](http://knockoutjs.com/)ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="f658e-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="f658e-106">段階的に、クライアント アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="f658e-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="f658e-107">書籍の一覧を表示しています。</span><span class="sxs-lookup"><span data-stu-id="f658e-107">Showing a list of books.</span></span>
- <span data-ttu-id="f658e-108">書籍の詳細を表示しています。</span><span class="sxs-lookup"><span data-stu-id="f658e-108">Showing a book detail.</span></span>
- <span data-ttu-id="f658e-109">新しい書籍を追加します。</span><span class="sxs-lookup"><span data-stu-id="f658e-109">Adding a new book.</span></span>

<span data-ttu-id="f658e-110">Knockout ライブラリは、モデル-ビュー-ビューモデル (MVVM) パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="f658e-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="f658e-111">**モデル**(当社の場合は、書籍と著者) で、ビジネス ドメイン内のデータのサーバー側の表現です。</span><span class="sxs-lookup"><span data-stu-id="f658e-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="f658e-112">**ビュー**はプレゼンテーション層 (HTML) です。</span><span class="sxs-lookup"><span data-stu-id="f658e-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="f658e-113">**ビュー モデル**は、モデルを保持する JavaScript オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="f658e-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="f658e-114">ビュー モデルは、UI のコードの抽象化です。</span><span class="sxs-lookup"><span data-stu-id="f658e-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="f658e-115">HTML 形式の知識がありません。</span><span class="sxs-lookup"><span data-stu-id="f658e-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="f658e-116">など、ビューの抽象の機能を表す、代わりに、&quot;書籍の一覧&quot;します。</span><span class="sxs-lookup"><span data-stu-id="f658e-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="f658e-117">ビューはデータ バインドはビュー モデルにします。</span><span class="sxs-lookup"><span data-stu-id="f658e-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="f658e-118">ビュー モデルに更新プログラムは、ビューで自動的に反映されます。</span><span class="sxs-lookup"><span data-stu-id="f658e-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="f658e-119">ビュー モデルは、ボタンのクリックしてなどもイベントと、ビューから取得します。</span><span class="sxs-lookup"><span data-stu-id="f658e-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="f658e-120">このアプローチでは、任意のコードを書き直すことがなく、バインドを変更できるため、簡単にレイアウトし、アプリの UI を変更できます。</span><span class="sxs-lookup"><span data-stu-id="f658e-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="f658e-121">たとえば、として項目の一覧を表示する場合があります、 `<ul>`、後で、テーブルに変更します。</span><span class="sxs-lookup"><span data-stu-id="f658e-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="f658e-122">Knockout ライブラリを追加します。</span><span class="sxs-lookup"><span data-stu-id="f658e-122">Add the Knockout Library</span></span>

<span data-ttu-id="f658e-123">Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**します。</span><span class="sxs-lookup"><span data-stu-id="f658e-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="f658e-124">選び**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="f658e-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="f658e-125">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="f658e-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="f658e-126">このコマンドは、Scripts フォルダーに Knockout ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="f658e-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="f658e-127">ビュー モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="f658e-127">Create the View Model</span></span>

<span data-ttu-id="f658e-128">Scripts フォルダーに app.js をという名前の JavaScript ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="f658e-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="f658e-129">(ソリューション エクスプ ローラーで、Scripts フォルダーを右クリックして**追加**を選択し、 **JavaScript ファイル**)。次のコードを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f658e-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="f658e-130">Knockout で、`observable`クラスは、データ バインディングを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f658e-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="f658e-131">可能なオブジェクトの内容を変更するときに、観測可能なオブジェクトに通知のすべてのデータ バインド コントロール自体を更新できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f658e-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="f658e-132">(、`observableArray`クラスは、配列のバージョンの*オブザーバブル*)。最初に、このビュー モデルでは、2 つの観測可能なオブジェクトがあります。</span><span class="sxs-lookup"><span data-stu-id="f658e-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="f658e-133">`books` 書籍の一覧を保持します。</span><span class="sxs-lookup"><span data-stu-id="f658e-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="f658e-134">`error` AJAX 呼び出しが失敗した場合は、エラー メッセージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f658e-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="f658e-135">`getAllBooks`メソッドは、AJAX 呼び出しをブックの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="f658e-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="f658e-136">結果をプッシュし、`books`配列。</span><span class="sxs-lookup"><span data-stu-id="f658e-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="f658e-137">`ko.applyBindings`メソッドは、Knockout ライブラリの一部です。</span><span class="sxs-lookup"><span data-stu-id="f658e-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="f658e-138">パラメーターとしてビュー モデルを受け取るし、データ バインディングを設定します。</span><span class="sxs-lookup"><span data-stu-id="f658e-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="f658e-139">スクリプト バンドルを追加します。</span><span class="sxs-lookup"><span data-stu-id="f658e-139">Add a Script Bundle</span></span>

<span data-ttu-id="f658e-140">バンドルは、ASP.NET 4.5 を結合または複数のファイルを 1 つのファイルをバンドルするが容易にする機能です。</span><span class="sxs-lookup"><span data-stu-id="f658e-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="f658e-141">バンドル化と、ページ読み込み時間を改善できると、サーバーに要求の数が減少します。</span><span class="sxs-lookup"><span data-stu-id="f658e-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="f658e-142">アプリのファイルを開く\_Start/BundleConfig.cs します。</span><span class="sxs-lookup"><span data-stu-id="f658e-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="f658e-143">RegisterBundles メソッドに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="f658e-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="f658e-144">[前へ](part-5.md)
> [次へ](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="f658e-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
