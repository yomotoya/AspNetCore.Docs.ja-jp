---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: AJAX を使用して、マッピングのシナリオを実装する |Microsoft Docs
author: microsoft
description: 手順 11 では、作成、編集または表示 dinners l を表示するユーザーを有効にすると、NerdDinner アプリケーションにマッピングの AJAX のサポートを統合する方法を示す.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 173ba551ca453ad46dbfd83ce1a2eb4a9b8eba3f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374293"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a><span data-ttu-id="d7d5d-103">AJAX を使用して、マッピングのシナリオを実装するには</span><span class="sxs-lookup"><span data-stu-id="d7d5d-103">Use AJAX to Implement Mapping Scenarios</span></span>
====================
<span data-ttu-id="d7d5d-104">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d7d5d-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d7d5d-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="d7d5d-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="d7d5d-106">これは、無料の手順 11 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-106">This is step 11 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="d7d5d-107">手順 11 では、NerdDinner アプリケーションでは、作成、編集または表示 dinners dinner の場所をグラフィカルに表示するユーザーを有効にするのに AJAX マッピング サポートを統合する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-107">Step 11 shows how to integrate AJAX mapping support into our NerdDinner application, enabling users who are creating, editing or viewing dinners to see the location of the dinner graphically.</span></span>
> 
> <span data-ttu-id="d7d5d-108">次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a><span data-ttu-id="d7d5d-109">NerdDinner 手順 11: AJAX のマップを統合します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-109">NerdDinner Step 11: Integrating an AJAX Map</span></span>

<span data-ttu-id="d7d5d-110">今すぐしましょうアプリケーションでは少しより視覚的に刺激マッピングの AJAX のサポートを統合することで。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-110">We'll now make our application a little more visually exciting by integrating AJAX mapping support.</span></span> <span data-ttu-id="d7d5d-111">これは、作成、編集または表示 dinners dinner の場所をグラフィカルに表示するユーザーを有効になります。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-111">This will enable users who are creating, editing or viewing dinners to see the location of the dinner graphically.</span></span>

### <a name="creating-a-map-partial-view"></a><span data-ttu-id="d7d5d-112">マップの部分ビューを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-112">Creating a Map Partial View</span></span>

<span data-ttu-id="d7d5d-113">アプリケーション内で複数の場所でマッピングの機能を使用するでしょう。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-113">We are going to use mapping functionality in several places within our application.</span></span> <span data-ttu-id="d7d5d-114">このコードは DRY を保持するには、複数のコント ローラー アクションとビューの間で再使用できる 1 つの部分的なテンプレート内で共通のマップ機能カプセル化します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-114">To keep our code DRY we'll encapsulate the common map functionality within a single partial template that we can re-use across multiple controller actions and views.</span></span> <span data-ttu-id="d7d5d-115">この部分ビュー"map.ascx"という名前がされ \Views\Dinners ディレクトリ内の作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-115">We'll name this partial view "map.ascx" and create it within the \Views\Dinners directory.</span></span>

<span data-ttu-id="d7d5d-116">生み出すことができます、map.ascx 部分 \Views\Dinners ディレクトリを右クリックし、追加の選択によって&gt;メニュー コマンドを表示します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-116">We can create the map.ascx partial by right-clicking on the \Views\Dinners directory and choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="d7d5d-117">します"Map.ascx"ビューを指定して、として、部分ビューをして、ここでは、厳密に型指定された"Dinner"モデル クラスを渡すことを示します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-117">We'll name the view "Map.ascx", check it as a partial view, and indicate that we are going to pass it a strongly-typed "Dinner" model class:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

<span data-ttu-id="d7d5d-118">[追加] ボタンをクリックしたとき、テンプレートの部分的なが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-118">When we click the "Add" button our partial template will be created.</span></span> <span data-ttu-id="d7d5d-119">Map.ascx ファイルに次の内容に更新し、います。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-119">We'll then update the Map.ascx file to have the following content:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

<span data-ttu-id="d7d5d-120">最初の&lt;スクリプト&gt;ポイントは、Microsoft Virtual Earth 6.2 マッピング ライブラリを参照します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-120">The first &lt;script&gt; reference points to the Microsoft Virtual Earth 6.2 mapping library.</span></span> <span data-ttu-id="d7d5d-121">2 番目の&lt;スクリプト&gt;map.js のファイルを作成後、一般的な Javascript のマッピングのロジックをカプセル化するポイントを参照します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-121">The second &lt;script&gt; reference points to a map.js file that we will shortly create which will encapsulate our common Javascript mapping logic.</span></span> <span data-ttu-id="d7d5d-122">&lt;Div id =「マップ」&gt;要素は、マップをホストする Virtual Earth を使用する HTML コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-122">The &lt;div id="theMap"&gt; element is the HTML container that Virtual Earth will use to host the map.</span></span>

<span data-ttu-id="d7d5d-123">埋め込みがある&lt;スクリプト&gt;このビューに固有の 2 つの JavaScript 関数を含むブロックします。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-123">We then have an embedded &lt;script&gt; block that contains two JavaScript functions specific to this view.</span></span> <span data-ttu-id="d7d5d-124">最初の関数は、ページがクライアント側スクリプトを実行する準備を実行する関数に接続する jQuery を使用します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-124">The first function uses jQuery to wire-up a function that executes when the page is ready to run client-side script.</span></span> <span data-ttu-id="d7d5d-125">LoadMap() ヘルパー関数を呼び出して、virtual earth のマップ コントロールを読み込む Map.js、スクリプト ファイル内で定義します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-125">It calls a LoadMap() helper function that we'll define within our Map.js script file to load the virtual earth map control.</span></span> <span data-ttu-id="d7d5d-126">2 番目の関数は、場所を識別するマップをピン留めが追加するコールバック イベント ハンドラーです。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-126">The second function is a callback event handler that adds a pin to the map that identifies a location.</span></span>

<span data-ttu-id="d7d5d-127">サーバー側は使用する方法に注意してください。 &lt;% = %&gt;ように、JavaScript にマップする Dinner の経度と緯度を埋め込むには、クライアント側のスクリプト ブロック内でブロックします。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-127">Notice how we are using a server-side &lt;%= %&gt; block within the client-side script block to embed the latitude and longitude of the Dinner we want to map into the JavaScript.</span></span> <span data-ttu-id="d7d5d-128">これは、(個別 AJAX 呼び出しをサーバーに – 値を取得する処理が速くなりますなし) クライアント側スクリプトで使用できる動的な値を出力する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-128">This is a useful technique to output dynamic values that can be used by client-side script (without requiring a separate AJAX call back to the server to retrieve the values – which makes it faster).</span></span> <span data-ttu-id="d7d5d-129">&lt;% = %&gt; – サーバーで、ビューのレンダリングは、ブロックが実行され、そのため、HTML の出力だけ結局は埋め込みの JavaScript 値 (例: var の緯度 = 47.64312;)。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-129">The &lt;%= %&gt; blocks will execute when the view is rendering on the server – and so the output of the HTML will just end up with embedded JavaScript values (for example: var latitude = 47.64312;).</span></span>

### <a name="creating-a-mapjs-utility-library"></a><span data-ttu-id="d7d5d-130">Map.js ユーティリティ ライブラリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-130">Creating a Map.js utility library</span></span>

<span data-ttu-id="d7d5d-131">Map.js ファイルを使用して、マップの JavaScript の機能をカプセル化 (および上記 LoadMap と LoadPin メソッドを実装) を今すぐ作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-131">Let's now create the Map.js file that we can use to encapsulate the JavaScript functionality for our map (and implement the LoadMap and LoadPin methods above).</span></span> <span data-ttu-id="d7d5d-132">これには、このプロジェクト内の \Scripts ディレクトリを右クリックして選択し、"追加-&gt;新しい項目の"メニュー コマンド、JScript の項目を選択し、"Map.js"という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-132">We can do this by right-clicking on the \Scripts directory within our project, and then choose the "Add-&gt;New Item" menu command, select the JScript item, and name it "Map.js".</span></span>

<span data-ttu-id="d7d5d-133">以下を追加する JavaScript コードは、Virtual Earth、マップを表示し、dinners のピンの場所を追加するとやり取りする Map.js ファイル。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-133">Below is the JavaScript code we'll add to the Map.js file that will interact with Virtual Earth to display our map and add locations pins to it for our dinners:</span></span>

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a><span data-ttu-id="d7d5d-134">マップを作成および編集フォームと統合します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-134">Integrating the Map with Create and Edit Forms</span></span>

<span data-ttu-id="d7d5d-135">既存の作成と編集シナリオのマップのサポートを統合します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-135">We'll now integrate the Map support with our existing Create and Edit scenarios.</span></span> <span data-ttu-id="d7d5d-136">これは非常に簡単のタスクで、コント ローラー コードの変更を必要としないです。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-136">The good news is that this is pretty easy to-do, and doesn't require us to change any of our Controller code.</span></span> <span data-ttu-id="d7d5d-137">Create と Edit ビューでは、dinner フォームの UI を実装する一般的な"DinnerForm"の部分ビューを共有するため 1 つの場所にマップを追加して、両方当社のそれを使用して任意の Create と Edit シナリオがある場合します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-137">Because our Create and Edit views share a common "DinnerForm" partial view to implement the dinner form UI, we can add the map in one place and have both our Create and Edit scenarios use it.</span></span>

<span data-ttu-id="d7d5d-138">\Views\Dinners\DinnerForm.ascx 部分ビューを開き、更新、部分的な新しいマップを追加するために必要な to do が。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-138">All we need to-do is to open the \Views\Dinners\DinnerForm.ascx partial view and update it to include our new map partial.</span></span> <span data-ttu-id="d7d5d-139">更新された DinnerForm がどのようにマップが追加されるを以下に示します (注: HTML フォーム要素は、簡潔にするための以下のコード スニペットから省略されます)。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-139">Below is what the updated DinnerForm will look like once the map is added (note: the HTML form elements are omitted from the code snippet below for brevity):</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

<span data-ttu-id="d7d5d-140">DinnerForm 上部分では、(Dinner オブジェクトと国の dropdownlist を設定する SelectList の両方が必要です) ためにそのモデルの種類として"DinnerFormViewModel"の型のオブジェクトを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-140">The DinnerForm partial above takes an object of type "DinnerFormViewModel" as its model type (because it needs both a Dinner object, as well as a SelectList to populate the dropdownlist of countries).</span></span> <span data-ttu-id="d7d5d-141">部分的なマップは、そのモデルの種類として"Dinner"の型のオブジェクトだけが必要し、ため、マップを部分的なレンダリングするときに渡される DinnerFormViewModel の Dinner サブ プロパティだけに。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-141">Our Map partial just needs an object of type "Dinner" as its model type, and so when we render the map partial we are passing just the Dinner sub-property of DinnerFormViewModel to it:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

<span data-ttu-id="d7d5d-142">JavaScript 関数を"Address"HTML テキスト ボックスに「ぼかし」イベントをアタッチする部分は jQuery に追加されました。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-142">The JavaScript function we've added to the partial uses jQuery to attach a "blur" event to the "Address" HTML textbox.</span></span> <span data-ttu-id="d7d5d-143">思いますが、ユーザーがクリックしたときに発生する「フォーカス」イベントまたはタブのテキスト ボックスにします。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-143">You've probably heard of "focus" events that fire when a user clicks or tabs into a textbox.</span></span> <span data-ttu-id="d7d5d-144">逆の場合、ユーザーがテキスト ボックスを終了するときに発生する「ぼかし」イベント。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-144">The opposite is a "blur" event that fires when a user exits a textbox.</span></span> <span data-ttu-id="d7d5d-145">上記のイベント ハンドラーは、これが行われ、マップ上の新しいアドレス場所をプロットし、ときにテキスト ボックスの緯度と経度の値をクリアします。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-145">The above event handler clears the latitude and longitude textbox values when this happens, and then plots the new address location on our map.</span></span> <span data-ttu-id="d7d5d-146">Map.js ファイル内で定義したコールバック イベント ハンドラーを書き出しましょうアドレスに基づいて、virtual earth によって返される値を使用して、フォーム上の経度と緯度のテキスト ボックスを更新します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-146">A callback event handler that we defined within the map.js file will then update the longitude and latitude textboxes on our form using values returned by virtual earth based on the address we gave it.</span></span>

<span data-ttu-id="d7d5d-147">ここでときに、アプリケーションを再実行し、標準の Dinner フォーム要素と共に表示される既定のマップを見てみましょう"ホスト Dinner" タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-147">And now when we run our application again and click the "Host Dinner" tab we'll see a default map displayed along with our standard Dinner form elements:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

<span data-ttu-id="d7d5d-148">、アドレスを入力してから tab、、場所を表示するマップを動的に更新し、場所の値で、緯度/経度のテキスト ボックスに、イベント ハンドラーが設定されます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-148">When we type in an address, and then tab away, the map will dynamically update to display the location, and our event handler will populate the latitude/longitude textboxes with the location values:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

<span data-ttu-id="d7d5d-149">新しい夕食を保存し、編集するために再度開く場合、ページが読み込まれるときに、マップの地域が表示されることを検索します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-149">If we save the new dinner and then open it again for editing, we'll find that the map location is displayed when the page loads:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

<span data-ttu-id="d7d5d-150">[アドレス] フィールドが変更されるたびに、マップと緯度/経度の座標が更新されます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-150">Every time the address field is changed, the map and the latitude/longitude coordinates will update.</span></span>

<span data-ttu-id="d7d5d-151">これで、マップには、Dinner 場所が表示されたらに代わりに非表示要素 (マップが自動的に更新して、アドレスが入力されるたび) から表示するテキスト ボックスの中から緯度と経度のフォーム フィールドを変更したこともできます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-151">Now that the map displays the Dinner location, we can also change the Latitude and Longitude form fields from being visible textboxes to instead be hidden elements (since the map is automatically updating them each time an address is entered).</span></span> <span data-ttu-id="d7d5d-152">To do これから Html.Hidden() のヘルパー メソッドを使用する Html.TextBox() HTML ヘルパーの使用に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-152">To-do this we'll switch from using the Html.TextBox() HTML helper to using the Html.Hidden() helper method:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

<span data-ttu-id="d7d5d-153">および、フォームは少しわかりやすいようになりましたし (まだ各 Dinner データベース内では、それらを格納) 中に生の緯度/経度を表示しないようにします。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-153">And now our forms are a little more user-friendly and avoid displaying the raw latitude/longitude (while still storing them with each Dinner in the database):</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a><span data-ttu-id="d7d5d-154">詳細ビューと、マップの統合</span><span class="sxs-lookup"><span data-stu-id="d7d5d-154">Integrating the Map with the Details View</span></span>

<span data-ttu-id="d7d5d-155">このシナリオでは詳細情報とも統合してみましょうサンプルを作成および編集のシナリオでの統合マップができました。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-155">Now that we have the map integrated with our Create and Edit scenarios, let's also integrate it with our Details scenario.</span></span> <span data-ttu-id="d7d5d-156">呼び出すには to do 必要がありますすべて&lt;Html.RenderPartial("map"); %&gt;詳細ビュー内で。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-156">All we need to-do is to call &lt;% Html.RenderPartial("map"); %&gt; within the Details view.</span></span>

<span data-ttu-id="d7d5d-157">完全な詳細ビュー (マップ統合) にソース コードがどのようにを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-157">Below is what the source code to the complete Details view (with map integration) looks like:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

<span data-ttu-id="d7d5d-158">今すぐ/Dinners/詳細/[id] URL に移動したときをに関する詳細を表示 dinner、dinner マップ上の場所 (ピンの完了時にマウスでポイントが表示されます、dinner のタイトルとそのアドレス)、RSVP fo に AJAX のリンクとr に。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-158">And now when a user navigates to a /Dinners/Details/[id] URL they'll see details about the dinner, the location of the dinner on the map (complete with a push-pin that when hovered over displays the title of the dinner and the address of it), and have an AJAX link to RSVP for it:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a><span data-ttu-id="d7d5d-159">データベースとリポジトリの場所の検索を実装します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-159">Implementing Location Search in our Database and Repository</span></span>

<span data-ttu-id="d7d5d-160">オフ、AJAX の実装を完了するには、ユーザーが近くに dinners を視覚的に検索できるようにするアプリケーションのホーム ページにマップを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-160">To finish off our AJAX implementation, let's add a Map to the home page of the application that allows users to graphically search for dinners near them.</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

<span data-ttu-id="d7d5d-161">まず、Dinners を検索する場所ベースの radius を効率的に実行する、データベースとデータのリポジトリ層内のサポートを実装します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-161">We'll begin by implementing support within our database and data repository layer to efficiently perform a location-based radius search for Dinners.</span></span> <span data-ttu-id="d7d5d-162">使用して、新しい[SQL 2008 の地理空間機能](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx)、これを実装するか、または Gary Dryden がこちらの記事で説明されている SQL 関数のアプローチを使用しています: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx)と Rob Coneryここでの LINQ to SQL の使用に関するブログ投稿しました。 [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)</span><span class="sxs-lookup"><span data-stu-id="d7d5d-162">We could use the new [geospatial features of SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) to implement this, or alternatively we can use a SQL function approach that Gary Dryden discussed in article here: [http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) and Rob Conery blogged about using with LINQ to SQL here: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)</span></span>

<span data-ttu-id="d7d5d-163">この手法を実装するためには Explorer を開き"サーバー"Visual Studio 内で、NerdDinner データベースを選択しその下にある「関数」のサブ ノードを右クリックし、新しい「スカラー値関数」を作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-163">To implement this technique, we will open the "Server Explorer" within Visual Studio, select the NerdDinner database, and then right-click on the "functions" sub-node under it and choose to create a new "Scalar-valued function":</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

<span data-ttu-id="d7d5d-164">次の関数をピクセルにし、貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-164">We'll then paste in the following DistanceBetween function:</span></span>

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

<span data-ttu-id="d7d5d-165">SQL Server の"NearestDinners"を呼び出すことで、新しいテーブル値関数から作成します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-165">We'll then create a new table-valued function in SQL Server that we'll call "NearestDinners":</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

<span data-ttu-id="d7d5d-166">この"NearestDinners"テーブル関数では、ピクセルのヘルパー関数を使用して、緯度の 100 マイル以内のすべて Dinners を返すし、指定経度。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-166">This "NearestDinners" table function uses the DistanceBetween helper function to return all Dinners within 100 miles of the latitude and longitude we supply it:</span></span>

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

<span data-ttu-id="d7d5d-167">この関数を呼び出すには、私たちを最初に開きます LINQ to SQL デザイナー、\Models ディレクトリ内で NerdDinner.dbml ファイルをダブルクリックしています。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-167">To call this function, we'll first open up the LINQ to SQL designer by double-clicking on the NerdDinner.dbml file within our \Models directory:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

<span data-ttu-id="d7d5d-168">To SQL デザイナーは、SQL NerdDinnerDataContext クラスに、LINQ のメソッドとして追加することにより、LINQ に NearestDinners およびピクセルの関数をドラッグしたしますし。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-168">We'll then drag the NearestDinners and DistanceBetween functions onto the LINQ to SQL designer, which will cause them to be added as methods on our LINQ to SQL NerdDinnerDataContext class:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

<span data-ttu-id="d7d5d-169">返す今後 NearestDinner 関数を使用するときは必ず DinnerRepository クラスに"FindByLocation"クエリ メソッドを公開できますし、指定した場所の 100 マイル内にある Dinners:</span><span class="sxs-lookup"><span data-stu-id="d7d5d-169">We can then expose a "FindByLocation" query method on our DinnerRepository class that uses the NearestDinner function to return upcoming Dinners that are within 100 miles of the specified location:</span></span>

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a><span data-ttu-id="d7d5d-170">AJAX の JSON ベースの検索アクション メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-170">Implementing a JSON-based AJAX Search Action Method</span></span>

<span data-ttu-id="d7d5d-171">マップを設定するために使用する Dinner データの一覧を返す新しい FindByLocation() リポジトリ メソッドを活用したコント ローラー アクション メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-171">We'll now implement a controller action method that takes advantage of the new FindByLocation() repository method to return back a list of Dinner data that can be used to populate a map.</span></span> <span data-ttu-id="d7d5d-172">返す Dinner データ、JSON (JavaScript Object Notation) 形式でクライアントで JavaScript を使用して簡単に操作できることができるように、このアクション メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-172">We'll have this action method return back the Dinner data in a JSON (JavaScript Object Notation) format so that it can be easily manipulated using JavaScript on the client.</span></span>

<span data-ttu-id="d7d5d-173">を実装する、\Controllers ディレクトリを右クリックし、[追加]-選択して、新しい"SearchController"クラスを作成します、&gt;コント ローラーのメニュー コマンド。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-173">To implement this, we'll create a new "SearchController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span> <span data-ttu-id="d7d5d-174">以下のような新しい SearchController クラス内で"SearchByLocation"アクション メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-174">We'll then implement a "SearchByLocation" action method within the new SearchController class like below:</span></span>

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

<span data-ttu-id="d7d5d-175">SearchController の SearchByLocation のアクション メソッドは、近くにある dinners の一覧を取得する DinnerRespository で FindByLocation メソッドを内部的に呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-175">The SearchController's SearchByLocation action method internally calls the FindByLocation method on DinnerRespository to get a list of nearby dinners.</span></span> <span data-ttu-id="d7d5d-176">クライアントに直接 Dinner オブジェクトを返すのではなく、代わりに返します JsonDinner オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-176">Rather than return the Dinner objects directly to the client, though, it instead returns JsonDinner objects.</span></span> <span data-ttu-id="d7d5d-177">JsonDinner クラスが Dinner プロパティのサブセットを公開します (例: セキュリティ上の理由を夕食に対する RSVP が人の名前を公開しません)。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-177">The JsonDinner class exposes a subset of Dinner properties (for example: for security reasons it doesn't disclose the names of the people who have RSVP'd for a dinner).</span></span> <span data-ttu-id="d7d5d-178">Dinner: 存在しないし、特定の夕食に関連付けられている RSVP オブジェクトの数をカウントすることによって動的に計算されます、RSVPCount プロパティも含まれています。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-178">It also includes an RSVPCount property that doesn't exist on Dinner– and which is dynamically calculated by counting the number of RSVP objects associated with a particular dinner.</span></span>

<span data-ttu-id="d7d5d-179">コント ローラーの基本クラスで Json() のヘルパー メソッドを使用して、返す一連の dinners JSON ベースのワイヤ形式を使用していますが。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-179">We are then using the Json() helper method on the Controller base class to return the sequence of dinners using a JSON-based wire format.</span></span> <span data-ttu-id="d7d5d-180">JSON は、単純なデータ構造を表すための標準のテキスト形式です。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-180">JSON is a standard text format for representing simple data-structures.</span></span> <span data-ttu-id="d7d5d-181">次の 2 つの JsonDinner オブジェクトの JSON 形式の一覧がどのように、アクション メソッドから返されるときに例を示します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-181">Below is an example of what a JSON-formatted list of two JsonDinner objects looks like when returned from our action method:</span></span>

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a><span data-ttu-id="d7d5d-182">JQuery を使用して、JSON ベースの AJAX メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="d7d5d-182">Calling the JSON-based AJAX method using jQuery</span></span>

<span data-ttu-id="d7d5d-183">今すぐ SearchController の SearchByLocation アクション メソッドを使用して NerdDinner アプリケーションのホーム ページを更新する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-183">We are now ready to update the home page of the NerdDinner application to use the SearchController's SearchByLocation action method.</span></span> <span data-ttu-id="d7d5d-184">To do、/Views/Home/Index.aspx ビュー テンプレートを開くを更新して、テキスト ボックス、[検索] ボタン、当社のマップおよび&lt;div&gt; dinnerList という名前の要素。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-184">To-do this, we'll open the /Views/Home/Index.aspx view template and update it to have a textbox, search button, our map, and a &lt;div&gt; element named dinnerList:</span></span>

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

<span data-ttu-id="d7d5d-185">2 つの JavaScript 関数のページに追加できます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-185">We can then add two JavaScript functions to the page:</span></span>

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

<span data-ttu-id="d7d5d-186">最初の JavaScript 関数は、ページが初めて読み込まれるときに、マップを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-186">The first JavaScript function loads the map when the page first loads.</span></span> <span data-ttu-id="d7d5d-187">JavaScript を 2 つ目の JavaScript 関数ワイヤは、イベント ハンドラーを検索 ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-187">The second JavaScript function wires up a JavaScript click event handler on the search button.</span></span> <span data-ttu-id="d7d5d-188">ボタンが押されたときに、Map.js ファイルに追加する FindDinnersGivenLocation() JavaScript 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-188">When the button is pressed it calls the FindDinnersGivenLocation() JavaScript function which we'll add to our Map.js file:</span></span>

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

<span data-ttu-id="d7d5d-189">この FindDinnersGivenLocation() 関数では、マップを呼び出します。入力した場所を中心に配置する Virtual Earth コントロールで Find() します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-189">This FindDinnersGivenLocation() function calls map.Find() on the Virtual Earth Control to center it on the entered location.</span></span> <span data-ttu-id="d7d5d-190">Virtual earth のマップ サービスが返す場合は、マップします。Find() メソッドでは、最後の引数として渡して callbackUpdateMapDinners コールバック メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-190">When the virtual earth map service returns, the map.Find() method invokes the callbackUpdateMapDinners callback method we passed it as the final argument.</span></span>

<span data-ttu-id="d7d5d-191">CallbackUpdateMapDinners() メソッドでは、実際の作業を実行する場所です。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-191">The callbackUpdateMapDinners() method is where the real work is done.</span></span> <span data-ttu-id="d7d5d-192">JQuery の $.post() ヘルパー メソッドを使用して、新しく中央揃えのマップの経度と緯度を引数として –、SearchController の SearchByLocation() アクション メソッドへの AJAX 呼び出しを実行します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-192">It uses jQuery's $.post() helper method to perform an AJAX call to our SearchController's SearchByLocation() action method – passing it the latitude and longitude of the newly centered map.</span></span> <span data-ttu-id="d7d5d-193">$.Post() のヘルパー メソッドが完了したら、呼び出されるインライン関数を定義し、"dinners"という名前の変数を使用してアクション メソッドが渡されます SearchByLocation() から JSON 形式の dinner 結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-193">It defines an inline function that will be called when the $.post() helper method completes, and the JSON-formatted dinner results returned from the SearchByLocation() action method will be passed it using a variable called "dinners".</span></span> <span data-ttu-id="d7d5d-194">返された各夕食経由での foreach は、dinner の緯度と経度、およびその他のプロパティを使用して、マップに新しい pin を追加します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-194">It then does a foreach over each returned dinner, and uses the dinner's latitude and longitude and other properties to add a new pin on the map.</span></span> <span data-ttu-id="d7d5d-195">Dinner エントリは、マップの右に dinners の HTML の一覧にも追加します。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-195">It also adds a dinner entry to the HTML list of dinners to the right of the map.</span></span> <span data-ttu-id="d7d5d-196">ワイヤ アップ、プッシュピンと HTML の一覧の両方のポイント イベントをユーザーが上に置いたときに、夕食についての詳細が表示されるよう。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-196">It then wires-up a hover event for both the pushpins and the HTML list so that details about the dinner are displayed when a user hovers over them:</span></span>

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

<span data-ttu-id="d7d5d-197">ここでときに、アプリケーションを実行し、マップが表示されますホーム ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-197">And now when we run the application and visit the home-page we'll be presented with a map.</span></span> <span data-ttu-id="d7d5d-198">市区町村の名前を入力すると、近くに今後の dinners がマップに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-198">When we enter the name of a city the map will display the upcoming dinners near it:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

<span data-ttu-id="d7d5d-199">ポインターを合わせると、dinner と、詳細な情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-199">Hovering over a dinner will display details about it.</span></span>

<span data-ttu-id="d7d5d-200">Dinner タイトルをクリックすると、バブルまたは HTML の一覧の右側にあるいずれかに移動します私たち、dinner は – ここできますし、必要に応じて RSVP の。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-200">Clicking the Dinner title either in the bubble or on the right-hand side in the HTML list will navigate us to the dinner – which we can then optionally RSVP for:</span></span>

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a><span data-ttu-id="d7d5d-201">次の手順</span><span class="sxs-lookup"><span data-stu-id="d7d5d-201">Next Step</span></span>

<span data-ttu-id="d7d5d-202">NerdDinner アプリケーションのすべてのアプリケーションの機能を実装しましたようになりました。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-202">We've now implemented all the application functionality of our NerdDinner application.</span></span> <span data-ttu-id="d7d5d-203">みましょう今すぐを達成する方法を見て自動化された単体テストのこと。</span><span class="sxs-lookup"><span data-stu-id="d7d5d-203">Let's now look at how we can enable automated unit testing of it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d7d5d-204">[前へ](use-ajax-to-deliver-dynamic-updates.md)
> [次へ](enable-automated-unit-testing.md)</span><span class="sxs-lookup"><span data-stu-id="d7d5d-204">[Previous](use-ajax-to-deliver-dynamic-updates.md)
[Next](enable-automated-unit-testing.md)</span></span>
