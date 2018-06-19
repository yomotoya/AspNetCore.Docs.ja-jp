---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: サーバー コントロール |Microsoft ドキュメント
author: microsoft
description: ASP.NET 2.0 では、さまざまな方法でサーバー コントロールを強化します。 このモジュールで、ASP.NET 2.0 の方法と Visual Studio 200 にアーキテクチャの変更の一部について説明します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 72e9cac7cf9a01791c30783fa56ad7ea205a5a11
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885194"
---
<a name="server-controls"></a><span data-ttu-id="ed54a-104">サーバー コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-104">Server Controls</span></span>
====================
<span data-ttu-id="ed54a-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ed54a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="ed54a-106">ASP.NET 2.0 では、さまざまな方法でサーバー コントロールを強化します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-106">ASP.NET 2.0 enhances server controls in many ways.</span></span> <span data-ttu-id="ed54a-107">このモジュールで ASP.NET 2.0 にアーキテクチャの変更の一部を説明し、Visual Studio 2005 は、サーバー コントロールを処理します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-107">In this module, we'll cover some of the architectural changes to the way ASP.NET 2.0 and Visual Studio 2005 deals with server controls.</span></span>


<span data-ttu-id="ed54a-108">ASP.NET 2.0 では、さまざまな方法でサーバー コントロールを強化します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-108">ASP.NET 2.0 enhances server controls in many ways.</span></span> <span data-ttu-id="ed54a-109">このモジュールで ASP.NET 2.0 にアーキテクチャの変更の一部を説明し、Visual Studio 2005 は、サーバー コントロールを処理します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-109">In this module, we'll cover some of the architectural changes to the way ASP.NET 2.0 and Visual Studio 2005 deals with server controls.</span></span>

## <a name="view-state"></a><span data-ttu-id="ed54a-110">ビューの状態</span><span class="sxs-lookup"><span data-stu-id="ed54a-110">View state</span></span>

<span data-ttu-id="ed54a-111">ビュー ステートに ASP.NET 2.0 の主な変更は、サイズが大幅に短縮します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-111">The primary change in view state in ASP.NET 2.0 is a dramatic reduction in size.</span></span> <span data-ttu-id="ed54a-112">カレンダー コントロールに含まれているページを検討してください。</span><span class="sxs-lookup"><span data-stu-id="ed54a-112">Consider a page with only a Calendar control on it.</span></span> <span data-ttu-id="ed54a-113">ASP.NET 1.1 でのビュー ステートを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-113">Here is the view state in ASP.NET 1.1.</span></span>

[!code-css[Main](server-controls/samples/sample1.css)]

<span data-ttu-id="ed54a-114">今すぐここではビュー ステートと同じページで ASP.NET 2.0 のです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-114">Now here's the view state on an identical page in ASP.NET 2.0.</span></span>

[!code-css[Main](server-controls/samples/sample2.css)]

<span data-ttu-id="ed54a-115">非常に大きな変更は、ビュー ステートが、ネットワーク上で前後持ち込まれることを検討してこの変更の開発者がパフォーマンスが大幅に増加います。</span><span class="sxs-lookup"><span data-stu-id="ed54a-115">That's a pretty significant change, and considering that view state is carried back and forth over the wire, this change can give developers a significant performance increase.</span></span> <span data-ttu-id="ed54a-116">ビュー ステートのサイズを縮小では、主に内部的に処理したことによるものです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-116">The reduction in size of view state is largely due to the way that we handle it internally.</span></span> <span data-ttu-id="ed54a-117">ビューの状態が、Base64 でエンコードされた文字列ことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ed54a-117">Remember that view state is a Base64 encoded string.</span></span> <span data-ttu-id="ed54a-118">ASP.NET 2.0 のビューステートの変更をより深く理解するをみましょう見て、上記の例からデコードされた値。</span><span class="sxs-lookup"><span data-stu-id="ed54a-118">To better understand the change in view state in ASP.NET 2.0, let's have a look at the decoded values from the examples above.</span></span>

<span data-ttu-id="ed54a-119">デコードされた 1.1 ビュー ステートを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-119">Here is the 1.1 view state decoded:</span></span>

[!code-css[Main](server-controls/samples/sample3.css)]

<span data-ttu-id="ed54a-120">これは可能性があります、不自然に少し似た外観がここでパターンがあります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-120">This may look a bit like gibberish, but there is a pattern here.</span></span> <span data-ttu-id="ed54a-121">ASP.NET で 1.x では、単一の文字データ型を識別するために使用し、使用して値を区切り、、 &lt; &gt;文字です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-121">In ASP.NET 1.x, we used single characters to identify data-types and delimited values using the &lt;&gt; characters.</span></span> <span data-ttu-id="ed54a-122">上記のビュー状態のサンプルでは、"t"は、3 つを表します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-122">The "t" in the view state sample above represents a Triplet.</span></span> <span data-ttu-id="ed54a-123">トリプレットには ("l"は、ArrayList を表します)。 ArrayLists のペアが含まれていますInt32 にはそれら ArrayLists のいずれかが含まれています ("i") を 1 およびその他の値を持つ別の組が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-123">The Triplet contains a pair of ArrayLists (the "l" represents an ArrayList.) One of those ArrayLists contains an Int32 ("i") with a value of 1 and the other contains another Triplet.</span></span> <span data-ttu-id="ed54a-124">トリプレットには、ArrayLists などのペアが含まれています。重要な点に注意してくださいがお Triplets のペアを含むはアルファベットを使用してデータ型を割り出すを使用して、&lt;と&gt;区切り記号として文字です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-124">The Triplet contains a pair of ArrayLists, etc. The important thing to remember is that we use Triplets that contain pairs, we identify the data-types via a letter, and we use the &lt; and &gt; characters as delimiters.</span></span>

<span data-ttu-id="ed54a-125">ASP.NET 2.0 では、デコードされたビュー ステートは少し異なるは検索します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-125">In ASP.NET 2.0, the decoded view state looks a bit different.</span></span>

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

<span data-ttu-id="ed54a-126">デコードされたビュー ステートの外観に大きな変更を確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-126">You should notice a huge change in the appearance of the decoded view state.</span></span> <span data-ttu-id="ed54a-127">この変更は、いくつかのアーキテクチャの基盤がします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-127">This change has several architectural underpinnings.</span></span> <span data-ttu-id="ed54a-128">ビューステートを ASP.NET で 1.x がデータをシリアル化、LosFormatter を使用します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-128">View state in ASP.NET 1.x used the LosFormatter to serialize data.</span></span> <span data-ttu-id="ed54a-129">2.0 では、新しい ObjectStateFormatter クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-129">In 2.0, we use the new ObjectStateFormatter class.</span></span> <span data-ttu-id="ed54a-130">このクラスは、ビュー状態、およびコントロールの状態の逆シリアル化とシリアル化に役立つ具体的には設計されています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-130">This class was specifically designed to aid in the serialization and deserialization of view state and control state.</span></span> <span data-ttu-id="ed54a-131">(コントロールの状態は次のセクションでカバーされます)。シリアル化と逆シリアル化が行わメソッドを変更することによって得られる利点もあります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-131">(Control state will be covered in the next section.) There are many benefits gained by changing the method by which serialization and deserialization take place.</span></span> <span data-ttu-id="ed54a-132">最も大幅なパフォーマンスの 1 つは、TextWriter を使用する LosFormatter とは異なり、ObjectStateFormatter を使用する、BinaryWriter ことです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-132">One of the most dramatic is the fact that unlike the LosFormatter which uses a TextWriter, the ObjectStateFormatter uses a BinaryWriter.</span></span> <span data-ttu-id="ed54a-133">これにより、ASP.NET 2.0 をビューステート一連の文字列ではなくバイトを格納できます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-133">This allows ASP.NET 2.0 to store view state a series of bytes instead of strings.</span></span> <span data-ttu-id="ed54a-134">たとえば、整数を取得します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-134">Take, for example, an integer.</span></span> <span data-ttu-id="ed54a-135">ASP.NET 1.1 では、整数は、ビュー ステートの 4 バイト必要です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-135">In ASP.NET 1.1, an integer required 4 bytes of view state.</span></span> <span data-ttu-id="ed54a-136">その同じ整数には、ASP.NET 2.0 では、1 バイトだけが必要です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-136">In ASP.NET 2.0, that same integer only requires 1 byte.</span></span> <span data-ttu-id="ed54a-137">格納されているビュー ステートの量を減らす他の機能強化が行われました。</span><span class="sxs-lookup"><span data-stu-id="ed54a-137">Other enhancements were made to decrease the amount of view state that is stored.</span></span> <span data-ttu-id="ed54a-138">DateTime 値などの保存され、TickCount を使用して、文字列の代わりにします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-138">DateTime values, for example, are now stored using a TickCount instead of a string.</span></span>

<span data-ttu-id="ed54a-139">場合と、すべてが言えば、特別な注意支払いが行われたという事実 1.x での表示状態の最大のコンシューマーのいずれかのようなコントロールと DataGrid がされていることにします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-139">As if all of that weren't enough, special attention was paid to the fact that one of the greatest consumers of view state in 1.x was the DataGrid and similar controls.</span></span> <span data-ttu-id="ed54a-140">ビュー ステートがに関しては、データ グリッドなどのコントロールの主な欠点は、大量の情報が繰り返される頻度が含まれているです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-140">A major drawback of controls such as the DataGrid where view state is concerned is that it often contains large amounts of repeated information.</span></span> <span data-ttu-id="ed54a-141">ASP.NET で 1.x では、および結果として再度肥大化表示状態を介してに単に情報が格納されている繰り返し表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-141">In ASP.NET 1.x, that repeated information was simply stored over and over again resulting in a bloated view state.</span></span> <span data-ttu-id="ed54a-142">ASP.NET 2.0 では、このようなデータを格納するのに新しい IndexedString クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-142">In ASP.NET 2.0, we use the new IndexedString class to store such data.</span></span> <span data-ttu-id="ed54a-143">文字列を繰り返す場合だけ、IndexedString と IndexedString オブジェクトの実行中のテーブル内のインデックスのトークンを格納します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-143">If a string repeats, we just store the token for the IndexedString and the index within a running table of IndexedString objects.</span></span>

## <a name="control-state"></a><span data-ttu-id="ed54a-144">コントロールの状態</span><span class="sxs-lookup"><span data-stu-id="ed54a-144">Control State</span></span>

<span data-ttu-id="ed54a-145">HTTP ペイロードにして追加したサイズでは開発者のビュー ステートにある主要な gripes のいずれか。</span><span class="sxs-lookup"><span data-stu-id="ed54a-145">One of the major gripes that developers had with view state was the size that it added to the HTTP payload.</span></span> <span data-ttu-id="ed54a-146">既に触れましたが、DataGrid コントロールのビュー状態の最大のコンシューマーの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-146">As previously mentioned, one of the greatest consumers of view state is the DataGrid control.</span></span> <span data-ttu-id="ed54a-147">大量のデータ グリッドによって生成されたビュー ステートを避けるためには、多くの開発者には、そのコントロールのビュー ステート単に無効になります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-147">To avoid the huge amounts of view state generated by a DataGrid, many developers simply disabled view state for that control.</span></span> <span data-ttu-id="ed54a-148">残念ながら、そのソリューションはありませんでした常に正常であります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-148">Unfortunately, that solution wasn't always a good one.</span></span> <span data-ttu-id="ed54a-149">ビュー ステート 1.x には ASP.NET では、コントロールの適切な機能に必要なデータだけでなくが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-149">View state in ASP.NET 1.x contains not only data necessary for the correct functionality of the control.</span></span> <span data-ttu-id="ed54a-150">コントロールの UI の状態に関する情報も含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-150">It also contains information concerning the state of the control's UI.</span></span> <span data-ttu-id="ed54a-151">つまり、DataGrid で改ページを表示する UI 情報がすべて必要としない場合でも、ビュー ステートを有効にする必要がありますを許可する場合の状態が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-151">This means that if you want to allow for pagination on a DataGrid, you must enable view state even if you don't need all of the UI information that view state contains.</span></span> <span data-ttu-id="ed54a-152">これは、0/1 シナリオです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-152">It's an all-or-nothing scenario.</span></span>

<span data-ttu-id="ed54a-153">ASP.NET 2.0 では、コントロールの状態は、コントロールの状態の概要を使用して適切に問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-153">In ASP.NET 2.0, control state solves that problem nicely via the introduction of control state.</span></span> <span data-ttu-id="ed54a-154">コントロールの状態には、コントロールの適切な機能を絶対に必要なデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-154">Control state contains the data that is absolutely necessary for the proper functionality of a control.</span></span> <span data-ttu-id="ed54a-155">ビュー状態とは異なりコントロールの状態を無効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="ed54a-155">Unlike view state, control state cannot be disabled.</span></span> <span data-ttu-id="ed54a-156">したがって、コントロールの状態に格納されているデータを慎重に管理することがあります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-156">Therefore, it is important that the data being stored in control state is carefully controlled.</span></span>

> [!NOTE]
> <span data-ttu-id="ed54a-157">表示状態と共にコントロールの状態が永続化、 \_ \_VIEWSTATE 隠しフォーム フィールドです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-157">Control state is persisted along with the view state in the \_\_VIEWSTATE hidden form field.</span></span>


<span data-ttu-id="ed54a-158">このビデオは、ビュー状態、およびコントロールの状態のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-158">This video is a walkthrough of view state and control state.</span></span>


![](server-controls/_static/image1.png)


[<span data-ttu-id="ed54a-159">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="ed54a-159">Open Full-Screen Video</span></span>](server-controls/_static/state1.wmv)


<span data-ttu-id="ed54a-160">サーバー コントロール状態を制御する読み書きするためには、3 つの手順を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-160">In order for a server control to read and write to control state, you must take three steps.</span></span>

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a><span data-ttu-id="ed54a-161">手順 1: RegisterRequiresControlState メソッドの呼び出し</span><span class="sxs-lookup"><span data-stu-id="ed54a-161">Step 1: Call the RegisterRequiresControlState Method</span></span>

<span data-ttu-id="ed54a-162">RegisterRequiresControlState メソッドは、コントロールがコントロールの状態を永続化する必要があることを ASP.NET に通知します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-162">The RegisterRequiresControlState method informs ASP.NET that a control needs to persist control state.</span></span> <span data-ttu-id="ed54a-163">登録するコントロールにあるコントロールの種類の 1 つの引数を受け取る。</span><span class="sxs-lookup"><span data-stu-id="ed54a-163">It takes one argument of type Control which is the control that is being registered.</span></span>

<span data-ttu-id="ed54a-164">登録では要求からは保持されないことに注意してくださいに重要です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-164">It is important to note that registration does not persist from request to request.</span></span> <span data-ttu-id="ed54a-165">そのため、このメソッドはコントロールがコントロールの状態を永続化する場合、要求ごとに呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-165">Therefore, this method must be called on every request if a control is to persist control state.</span></span> <span data-ttu-id="ed54a-166">OnInit で、メソッドを呼び出すことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-166">It is recommended that the method be called in OnInit.</span></span>

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a><span data-ttu-id="ed54a-167">手順 2: SaveControlState をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-167">Step 2: Override SaveControlState</span></span>

<span data-ttu-id="ed54a-168">SaveControlState メソッドでは、最後のポスト バック以降コントロール、コントロールの状態の変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-168">The SaveControlState method saves control state changes for a control since the last post back.</span></span> <span data-ttu-id="ed54a-169">コントロールの状態を表すオブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-169">It returns an object representing the control's state.</span></span>

## <a name="step-3-override-loadcontrolstate"></a><span data-ttu-id="ed54a-170">手順 3: LoadControlState をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-170">Step 3: Override LoadControlState</span></span>

<span data-ttu-id="ed54a-171">LoadControlState メソッドは、コントロールに保存された状態を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-171">The LoadControlState method loads saved state into a control.</span></span> <span data-ttu-id="ed54a-172">メソッドは、コントロールの保存された状態を保持するオブジェクトの種類の 1 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-172">The method takes one argument of type Object which holds the saved state for the control.</span></span>

## <a name="full-xhtml-compliance"></a><span data-ttu-id="ed54a-173">完全 XHTML コンプライアンス</span><span class="sxs-lookup"><span data-stu-id="ed54a-173">Full XHTML Compliance</span></span>

<span data-ttu-id="ed54a-174">Web 開発者は、Web アプリケーションで標準の重要性を認識します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-174">Any Web developer knows the importance of standards in Web applications.</span></span> <span data-ttu-id="ed54a-175">標準ベースの開発環境を維持するためには、ASP.NET 2.0 と、XHTML 準拠では完全です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-175">In order to maintain a standards-based development environment, ASP.NET 2.0 is fully XHTML compliant.</span></span> <span data-ttu-id="ed54a-176">したがって、すべてのタグは HTML 4.0 をサポートするブラウザーで XHTML 標準に従って表示されるか、またはです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-176">Therefore, all tags are rendered according to XHTML standards in browsers that support HTML 4.0 or greater.</span></span>

<span data-ttu-id="ed54a-177">ASP.NET 1.1 DOCTYPE の定義は次のとおりでした。</span><span class="sxs-lookup"><span data-stu-id="ed54a-177">The DOCTYPE definition in ASP.NET 1.1 was as follows:</span></span>

[!code-html[Main](server-controls/samples/sample6.html)]

<span data-ttu-id="ed54a-178">ASP.NET 2.0 の既定の DOCTYPE 定義のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-178">In ASP.NET 2.0, the default DOCTYPE definition is as follows:</span></span>

[!code-html[Main](server-controls/samples/sample7.html)]

<span data-ttu-id="ed54a-179">選択した場合、構成ファイルで、長期ノードを介して既定 XHML コンプライアンスを変更できます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-179">If you choose, you can alter the default XHML compliance via the xhtmlConformance node in the configuration file.</span></span> <span data-ttu-id="ed54a-180">たとえば、web.config ファイルに次のノードは XHTML 1.0 Strict を XHTML のコンプライアンス対応を変更します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-180">For example, the following node in the web.config file will change XHTML compliance to XHTML 1.0 Strict:</span></span>

[!code-xml[Main](server-controls/samples/sample8.xml)]

<span data-ttu-id="ed54a-181">選択した場合、構成することも、ASP.NET で使用するレガシの構成を使用する ASP.NET 1.x 次のようにします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-181">If you choose, you can also configure ASP.NET to use the legacy configuration used in ASP.NET 1.x as follows:</span></span>

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a><span data-ttu-id="ed54a-182">アダプティブ アダプターを使用して表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-182">Adaptive Rendering Using Adapters</span></span>

<span data-ttu-id="ed54a-183">ASP.NET で 1.x では、構成ファイルが含まれている、 &lt;browserCaps&gt; HttpBrowserCapabilities オブジェクトを作成するセクション。</span><span class="sxs-lookup"><span data-stu-id="ed54a-183">In ASP.NET 1.x, the configuration file contained a &lt;browserCaps&gt; section that populated a HttpBrowserCapabilities object.</span></span> <span data-ttu-id="ed54a-184">このオブジェクトは、開発者はどのようなデバイスの特定の要求は作成を特定し、コードを適切に表示を許可します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-184">This object allowed a developer to determine what device is making a particular request and render code appropriately.</span></span> <span data-ttu-id="ed54a-185">ASP.NET 2.0 では、モデルが改善され、新しいある ControlAdapter クラスを使用するようになりました。</span><span class="sxs-lookup"><span data-stu-id="ed54a-185">In ASP.NET 2.0, the model has improved and now uses the new ControlAdapter class.</span></span> <span data-ttu-id="ed54a-186">ある ControlAdapter クラスは、コントロールのライフ サイクルのイベントを上書きし、ユーザー エージェントの能力に基づいてコントロールのレンダリングを制御します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-186">The ControlAdapter class overrides events in the control's lifecycle and controls the rendering of controls based upon the user agent's capabilities.</span></span> <span data-ttu-id="ed54a-187">特定のユーザー エージェントの機能は、c:\windows\microsoft.net\framework\v2.0 に格納されているブラウザー定義ファイル (.browser ファイル拡張子を持つファイル) によって定義されます。\* \* \* \*\CONFIG\Browsers フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-187">The capabilities of a specific user agent are defined by a browser definition file (a file with a .browser file extension) stored in the c:\windows\microsoft.net\framework\v2.0.\*\*\*\*\CONFIG\Browsers folder.</span></span>

> [!NOTE]
> <span data-ttu-id="ed54a-188">ある ControlAdapter クラスは、抽象クラスです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-188">The ControlAdapter class is an abstract class.</span></span>


<span data-ttu-id="ed54a-189">ほぼ同様に、 &lt;browserCaps&gt; 1.x では、ブラウザー定義ファイルのセクションでは、正規表現を使用して、要求側のブラウザーを識別するためにユーザー エージェント文字列を解析します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-189">Much like the &lt;browserCaps&gt; section in 1.x, the browser definition file uses a Regular Expression to parse the user agent string in order to identify the requesting browser.</span></span> <span data-ttu-id="ed54a-190">これは、ユーザー エージェントに対して特定の機能を定義しています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-190">It them defines particular capabilities for that user agent.</span></span> <span data-ttu-id="ed54a-191">Render メソッドを使用してコントロールをある ControlAdapter にレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-191">The ControlAdapter renders the control via the Render method.</span></span> <span data-ttu-id="ed54a-192">したがって、Render メソッドをオーバーライドする場合呼び出す必要はありませんレンダリング、基本クラスです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-192">Therefore, if you override the Render method, you should not call Render on the base class.</span></span> <span data-ttu-id="ed54a-193">これは、アダプターの 1 回に 1 回と、コントロール自体を 2 回発生するレンダリングにあります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-193">Doing so may cause rendering to occur twice, once for the adapter and once for the control itself.</span></span>

## <a name="developing-a-custom-adapter"></a><span data-ttu-id="ed54a-194">カスタム アダプターの開発</span><span class="sxs-lookup"><span data-stu-id="ed54a-194">Developing a Custom Adapter</span></span>

<span data-ttu-id="ed54a-195">ある ControlAdapter から継承することで、独自のカスタム アダプターを開発できます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-195">You can develop your own custom adapter by inheriting from ControlAdapter.</span></span> <span data-ttu-id="ed54a-196">さらに、アダプターが、ページのために必要な場所の場合は、抽象クラス PageAdapter から継承することができます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-196">Additionally, you can inherit from the abstract class PageAdapter in cases where an adapter is needed for a page.</span></span> <span data-ttu-id="ed54a-197">使用して、カスタム アダプターへのコントロールのマッピングを実現、 &lt;controlAdapters&gt;ブラウザー定義ファイル内の要素。</span><span class="sxs-lookup"><span data-stu-id="ed54a-197">Mapping of controls to your custom adapter is accomplished via the &lt;controlAdapters&gt; element in the browser definition file.</span></span> <span data-ttu-id="ed54a-198">たとえば、ブラウザーの定義ファイルから次の XML は、メニュー コントロールを MenuAdapter クラスにマッピングされます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-198">For example, the following XML from a browser definition file maps the Menu control to the MenuAdapter class:</span></span>

[!code-html[Main](server-controls/samples/sample10.html)]

<span data-ttu-id="ed54a-199">このモデルを使用して、コントロールの開発者が特定のデバイスまたはブラウザーを対象とする非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-199">Using this model, it becomes quite easy for a control developer to target a particular device or browser.</span></span> <span data-ttu-id="ed54a-200">開発者がすべてのデバイス上のページの表示方法を完全に制御するために非常にシンプルです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-200">It's also quite simple for a developer to have complete control over how pages render on every device.</span></span>

## <a name="per-device-rendering"></a><span data-ttu-id="ed54a-201">デバイスごとのレンダリング</span><span class="sxs-lookup"><span data-stu-id="ed54a-201">Per-Device Rendering</span></span>

<span data-ttu-id="ed54a-202">ASP.NET 2.0 のサーバー コントロールのプロパティは、指定したデバイスごとのブラウザーに固有のプレフィックスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-202">Server control properties in ASP.NET 2.0 can be specified per-device using a browser-specific prefix.</span></span> <span data-ttu-id="ed54a-203">たとえば、次のコードには、によっては、ページを参照するどのデバイスが使用されるラベルのテキストが変わります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-203">For example, the code below will change the Text of a label depending upon which device is being used to browse the page.</span></span>

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

<span data-ttu-id="ed54a-204">ラベルが「を参照している Internet Explorer からです」というメッセージがテキストを表示するこのラベルを含むページが Internet Explorer から参照されると、</span><span class="sxs-lookup"><span data-stu-id="ed54a-204">When the page containing this label is browsed from Internet Explorer, the label will display text saying "You are browsing from Internet Explorer."</span></span> <span data-ttu-id="ed54a-205">Firefox からページが参照されると、ラベルのテキストを表示」を参照している Firefox からです"</span><span class="sxs-lookup"><span data-stu-id="ed54a-205">When the page is browsed from Firefox, the label will display the text "You are browsing from Firefox."</span></span> <span data-ttu-id="ed54a-206">その他の任意のデバイスからページが参照されると、「を参照している不明なデバイスからです」が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-206">When the page is browsed from any other device, it will display "You are browsing from an unknown device."</span></span> <span data-ttu-id="ed54a-207">この特別な構文を使用して、任意のプロパティを指定できます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-207">Any property can be specified using this special syntax.</span></span>

## <a name="setting-focus"></a><span data-ttu-id="ed54a-208">フォーカスを設定</span><span class="sxs-lookup"><span data-stu-id="ed54a-208">Setting Focus</span></span>

<span data-ttu-id="ed54a-209">ASP.NET 1.x 開発者は、特定のコントロールに初期フォーカスを設定する方法についてよく寄せられます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-209">ASP.NET 1.x developers frequently asked about how to set initial focus on a particular control.</span></span> <span data-ttu-id="ed54a-210">たとえば、ログイン ページで、ユーザー ID ボックスに、ページが初めて読み込まれるときに、フォーカスを取得すると便利ですが。</span><span class="sxs-lookup"><span data-stu-id="ed54a-210">For example, on a login page, it's useful to have the User ID textbox get the focus when the page first loads.</span></span> <span data-ttu-id="ed54a-211">ASP.NET で 1.x では、これを行うために必要ないくつかのクライアント側スクリプトを記述します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-211">In ASP.NET 1.x, doing this required writing some client-side script.</span></span> <span data-ttu-id="ed54a-212">このスクリプトは、簡単なタスクが、必要がある不要になった ASP.NET 2.0 の SetFocus メソッドご協力に感謝します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-212">Even though such a script is a trivial task, it's no longer necessary in ASP.NET 2.0 thanks to the SetFocus method.</span></span> <span data-ttu-id="ed54a-213">SetFocus メソッドは、フォーカスを受け取る必要のあるコントロールを示す 1 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-213">The SetFocus method takes one argument indicating the control that should receive focus.</span></span> <span data-ttu-id="ed54a-214">この引数は文字列としてのコントロールのクライアント ID またはコントロール オブジェクトとして、サーバー コントロールの名前にするか、できます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-214">This argument can either be the client ID of the control as a string or the name of the Server control as a Control object.</span></span> <span data-ttu-id="ed54a-215">たとえば、txtUserID というページが初めて読み込まれるときにテキスト ボックス コントロールを初期フォーカスを設定するに ページに次のコードを追加\_負荷。</span><span class="sxs-lookup"><span data-stu-id="ed54a-215">For example, to set the initial focus to a TextBox control called txtUserID when the page first loads, add the following code to Page\_Load:</span></span>

[!code-csharp[Main](server-controls/samples/sample12.cs)]

<span data-ttu-id="ed54a-216">--または</span><span class="sxs-lookup"><span data-stu-id="ed54a-216">-- or</span></span>

[!code-csharp[Main](server-controls/samples/sample13.cs)]

<span data-ttu-id="ed54a-217">ASP.NET 2.0 では、(前述) Webresource.axd ハンドラーを使用して、フォーカスを設定するクライアント側の関数を表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-217">ASP.NET 2.0 uses the Webresource.axd handler (discussed previously) to render a client-side function that sets the focus.</span></span> <span data-ttu-id="ed54a-218">クライアント側の関数の名前は WebForm\_AutoFocus 次に示すようにします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-218">The name of the client-side function is WebForm\_AutoFocus as shown here:</span></span>

[!code-html[Main](server-controls/samples/sample14.html)]

<span data-ttu-id="ed54a-219">代わりに、コントロールにフォーカス メソッドを使用すると、そのコントロールに初期フォーカスを設定します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-219">Alternatively, you can use the Focus method for a control to set the initial focus to that control.</span></span> <span data-ttu-id="ed54a-220">フォーカス メソッドは、コントロール クラスから派生しはすべての ASP.NET 2.0 コントロールに使用できます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-220">The Focus method derives from the Control class and is available to all ASP.NET 2.0 controls.</span></span> <span data-ttu-id="ed54a-221">検証エラーが発生したときに、特定のコントロールにフォーカスを設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-221">It is also possible to set focus to a particular control when a validation error occurs.</span></span> <span data-ttu-id="ed54a-222">後のモジュールにとり上げますです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-222">That will be covered in a later module.</span></span>

## <a name="new-server-controls-in-aspnet-20"></a><span data-ttu-id="ed54a-223">ASP.NET 2.0 の新しいサーバー コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-223">New Server Controls in ASP.NET 2.0</span></span>

<span data-ttu-id="ed54a-224">ASP.NET 2.0 の新しいサーバー コントロールを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-224">The following are new server controls in ASP.NET 2.0.</span></span> <span data-ttu-id="ed54a-225">以降のモジュール内のいくつか詳細に進むされます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-225">We will go into more detail on some of them in later modules.</span></span>

## <a name="imagemap-control"></a><span data-ttu-id="ed54a-226">ImageMap コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-226">ImageMap Control</span></span>

<span data-ttu-id="ed54a-227">ImageMap コントロールでは、ホット スポットをポスト バックを開始するか、URL に移動することができますをイメージに追加することができます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-227">The ImageMap control allows you to add hotspots to an image that can initiate a post back or navigate to a URL.</span></span> <span data-ttu-id="ed54a-228">使用可能なホット スポットの 3 つの種類があります。CircleHotSpot、RectangleHotSpot、および PolygonHotSpot です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-228">There are three types of hotspots available; CircleHotSpot, RectangleHotSpot, and PolygonHotSpot.</span></span> <span data-ttu-id="ed54a-229">ホット スポットは、Visual Studio またはプログラムによってコードでは、コレクション エディターを使用して追加されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-229">Hotspots are added via a collection editor in Visual Studio or programmatically in code.</span></span> <span data-ttu-id="ed54a-230">イメージのホット スポットを描画するために使用できるユーザー インターフェイスはありません。</span><span class="sxs-lookup"><span data-stu-id="ed54a-230">There is no user-interface available for drawing hotspots on an image.</span></span> <span data-ttu-id="ed54a-231">座標とサイズまたはホット スポットの半径は、宣言によって指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-231">The coordinates and size or radius of the hotspot must be specified declaratively.</span></span> <span data-ttu-id="ed54a-232">また、デザイナーでホット スポットの視覚的表現はありません。</span><span class="sxs-lookup"><span data-stu-id="ed54a-232">There is also no visual representation of a hotspot in the designer.</span></span> <span data-ttu-id="ed54a-233">ホット スポットが構成した場合、URL に移動して、ホット スポットの NavigateUrl プロパティを使用して、URL が指定されています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-233">If a hotspot is configured to navigate to a URL, the URL is specified via the NavigateUrl property of the hotspot.</span></span> <span data-ttu-id="ed54a-234">Post の場合は、プロパティを使用すると、サーバー側コードで取得できる、ポストバックで文字列を渡す PostBackValue のホット スポットをバックアップします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-234">In the case of a post back hotspot, the PostBackValue property allows you to pass a string in the post back that can be retrieved in server-side code.</span></span>


![Visual Studio で hotSpot コレクション エディター](server-controls/_static/image1.jpg)

<span data-ttu-id="ed54a-236">**図 1**: Visual Studio で HotSpot コレクション エディター</span><span class="sxs-lookup"><span data-stu-id="ed54a-236">**Figure 1**: HotSpot Collection Editor in Visual Studio</span></span>


## <a name="bulletedlist-control"></a><span data-ttu-id="ed54a-237">BulletedList コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-237">BulletedList Control</span></span>

<span data-ttu-id="ed54a-238">BulletedList コントロールは、データ バインドを簡単にすることができますを箇条書き一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-238">The BulletedList control is a bulleted list that can easily be data bound.</span></span> <span data-ttu-id="ed54a-239">一覧順序を指定できます (番号付き) または BulletStyle プロパティ経由で順序付けられていません。</span><span class="sxs-lookup"><span data-stu-id="ed54a-239">The list can be ordered (numbered) or unordered via the BulletStyle property.</span></span> <span data-ttu-id="ed54a-240">リスト内の各項目は ListItem オブジェクトによって表されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-240">Each item in the list is represented by a ListItem object.</span></span>


![Visual Studio での BulletedList コントロール](server-controls/_static/image1.gif)

<span data-ttu-id="ed54a-242">**図 2**: Visual Studio での BulletedList コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-242">**Figure 2**: BulletedList Control in Visual Studio</span></span>


## <a name="hiddenfield-control"></a><span data-ttu-id="ed54a-243">HiddenField コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-243">HiddenField Control</span></span>

<span data-ttu-id="ed54a-244">HiddenField コントロールは、ページに、値はサーバー側コードで使用可能な非表示のフォーム フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-244">The HiddenField control adds a hidden form field to your page, the value of which is available in server-side code.</span></span> <span data-ttu-id="ed54a-245">隠しフォーム フィールドの値は、通常のバックアップ作成する post 間変更されないままにしてが必要です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-245">The value of a hidden form field is generally expected to remain unchanged between post backs.</span></span> <span data-ttu-id="ed54a-246">ただし、悪意のあるユーザーをポスト バックする前に、値を変更することはできます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-246">However, it is possible for a malicious user to change the value prior to post back.</span></span> <span data-ttu-id="ed54a-247">この場合、HiddenField コントロールに ValueChanged イベントが発生します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-247">If this happens, the HiddenField control will raise the ValueChanged event.</span></span> <span data-ttu-id="ed54a-248">HiddenField コントロール内の機密情報があるが変更されないことを保証する場合は、コードで ValueChanged イベントを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-248">If you have sensitive information in the HiddenField control and you want to ensure that it remains unchanged, you should handle the ValueChanged event in your code.</span></span>

## <a name="fileupload-control"></a><span data-ttu-id="ed54a-249">ファイルアップロード コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-249">FileUpload Control</span></span>

<span data-ttu-id="ed54a-250">ASP.NET 2.0 のファイルアップロード コントロールにより ASP.NET ページを使用して Web サーバーにファイルをアップロードできます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-250">The FileUpload control in ASP.NET 2.0 makes it possible to upload files to a Web server via an ASP.NET page.</span></span> <span data-ttu-id="ed54a-251">このコントロールは ASP.NET 1.x HtmlInputFile クラスは、いくつかの例外を非常に似ています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-251">This control is quite similar to the ASP.NET 1.x HtmlInputFile class with a few exceptions.</span></span> <span data-ttu-id="ed54a-252">ASP.NET で 1.x では、推奨されていた適切なファイルがあったかどうかを判断するために null を PostedFile プロパティをチェックします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-252">In ASP.NET 1.x, it was recommended that the PostedFile property be checked for null in order to determine if you had a good file.</span></span> <span data-ttu-id="ed54a-253">ASP.NET 2.0 のファイルアップロード コントロールは、同じ目的で使用することができます、少し方が効率的に、新しい HasFile プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-253">The FileUpload control in ASP.NET 2.0 adds a new HasFile property that you can use for the same purpose and it's a bit more efficient.</span></span>

<span data-ttu-id="ed54a-254">PostedFile プロパティは、HttpPostedFile オブジェクトへのアクセスを引き続き使用できますが、HttpPostedFile の機能の一部がファイルアップロード コントロールの本質的に使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="ed54a-254">The PostedFile property is still available for access to an HttpPostedFile object, but some of the functionality of the HttpPostedFile is now available intrinsically with the FileUpload control.</span></span> <span data-ttu-id="ed54a-255">たとえば、ASP.NET でアップロードされたファイルを保存する 1.x では、メソッドを呼び出す SaveAs HttpPostedFile オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="ed54a-255">For example, to save an uploaded file in ASP.NET 1.x, you call the SaveAs method on the HttpPostedFile object.</span></span> <span data-ttu-id="ed54a-256">ファイルアップロード コントロールを使用して、ASP.NET 2.0 では、ファイルアップロード コントロール自体に SaveAs メソッドを呼び出すとします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-256">Using the FileUpload control in ASP.NET 2.0, you would call the SaveAs method on the FileUpload control itself.</span></span>

<span data-ttu-id="ed54a-257">2.0 の動作 (およびの場合、最も重要な変更)、もう 1 つの重要な変更は、保存する前にアップロードされたファイル全体をメモリに読み込む必要が不要になったことです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-257">Another significant change in the 2.0 behavior (and likely the most significant change) is that it is no longer necessary to load an entire uploaded file into memory before saving it.</span></span> <span data-ttu-id="ed54a-258">1.x では、書き込まれる前にメモリに完全にアップロードされた任意のファイルを保存をディスクにします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-258">In 1.x, any file that was uploaded is saved entirely into memory prior to being written to disk.</span></span> <span data-ttu-id="ed54a-259">このアーキテクチャでは、大きなファイルのアップロードできなくなります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-259">This architecture prevents the upload of large files.</span></span>

<span data-ttu-id="ed54a-260">ASP.NET 2.0 では、requestLengthDiskThreshold 属性 httpRuntime 要素のでは、キロバイト数を構成する書き込まれる前にメモリ内のバッファー内に保持されてディスクにします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-260">In ASP.NET 2.0, the requestLengthDiskThreshold attribute of the httpRuntime element allows you to configure how many Kilobytes are held in a buffer in memory prior to being written to disk.</span></span>

<span data-ttu-id="ed54a-261">**重要な**: この値がバイト (キロバイト単位ではない) である、既定値は 256 を MSDN ドキュメント (と他の場所のドキュメント) を指定します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-261">**IMPORTANT**: MSDN documentation (and documentation elsewhere) specifies that this value is in bytes (not Kilobytes) and that the default is 256.</span></span> <span data-ttu-id="ed54a-262">キロバイト単位で指定された値が実際には、既定値は 80 です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-262">The value is actually specified in Kilobytes and the default value is 80.</span></span> <span data-ttu-id="ed54a-263">で、既定値は 80 K、いるバッファーが終わらない大きなオブジェクト ヒープのことを確認します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-263">By having a default value of 80K, we ensure that the buffer does not end up on the large object heap.</span></span>

## <a name="wizard-control"></a><span data-ttu-id="ed54a-264">ウィザード コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-264">Wizard Control</span></span>

<span data-ttu-id="ed54a-265">「ページ」パネルを使用するのか、ページ間を転送することにより、系列内の情報を収集する際に苦慮 ASP.NET 開発者が発生することがよくなります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-265">It is fairly common to encounter ASP.NET developers struggling with attempting to gather information in a series of "pages" using panels or by transferring from page to page.</span></span> <span data-ttu-id="ed54a-266">ほとんどの場合、努力と手間が 1 つ、時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-266">More often than not, the endeavor is a frustrating one and is time consuming.</span></span> <span data-ttu-id="ed54a-267">新しいウィザード コントロールは、線形と非線形の手順に慣れているユーザー ウィザードのインターフェイスにより、問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-267">The new Wizard control solves the problems by allowing for linear and non-linear steps in a wizard interface that users are familiar with.</span></span> <span data-ttu-id="ed54a-268">ウィザード コントロールは、一連の手順で入力フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-268">The Wizard control presents input forms in a series of steps.</span></span> <span data-ttu-id="ed54a-269">各ステップでは、特定の種類のコントロールの StepType プロパティで指定します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-269">Each step is of a particular type specified by the StepType property of the control.</span></span> <span data-ttu-id="ed54a-270">使用可能なステップの種類は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-270">The available step types are as follows:</span></span>

| <span data-ttu-id="ed54a-271">**ステップの種類**</span><span class="sxs-lookup"><span data-stu-id="ed54a-271">**Step Type**</span></span> | <span data-ttu-id="ed54a-272">**説明**</span><span class="sxs-lookup"><span data-stu-id="ed54a-272">**Explanation**</span></span> |
| --- | --- |
| <span data-ttu-id="ed54a-273">自動</span><span class="sxs-lookup"><span data-stu-id="ed54a-273">Auto</span></span> | <span data-ttu-id="ed54a-274">ウィザードでは、手順階層内の位置に基づいてステップの種類が自動的に決定します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-274">The wizard automatically determines the type of step based upon its position within the step hierarchy.</span></span> |
| <span data-ttu-id="ed54a-275">[開始]</span><span class="sxs-lookup"><span data-stu-id="ed54a-275">Start</span></span> | <span data-ttu-id="ed54a-276">最初の手順、多くの場合、最初のステートメントを提示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-276">The first step, often used to present an introductory statement.</span></span> |
| <span data-ttu-id="ed54a-277">手順</span><span class="sxs-lookup"><span data-stu-id="ed54a-277">Step</span></span> | <span data-ttu-id="ed54a-278">通常の手順です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-278">A normal step.</span></span> |
| <span data-ttu-id="ed54a-279">完了</span><span class="sxs-lookup"><span data-stu-id="ed54a-279">Finish</span></span> | <span data-ttu-id="ed54a-280">最後の手順、通常、ウィザードを完了するためのボタンを表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-280">The final step, usually used to present a button to finish the wizard.</span></span> |
| <span data-ttu-id="ed54a-281">完了</span><span class="sxs-lookup"><span data-stu-id="ed54a-281">Complete</span></span> | <span data-ttu-id="ed54a-282">成功または失敗を通信してメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-282">Presents a message communicating success or failure.</span></span> |

> [!NOTE]
> <span data-ttu-id="ed54a-283">ウィザード コントロールの追跡の状態を ASP.NET コントロール状態を使用します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-283">The Wizard control keeps track of its state using ASP.NET control state.</span></span> <span data-ttu-id="ed54a-284">そのため、エントリのプロパティは、任意に不利益をもたらすことがなく false に設定できます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-284">Therefore, the EnableViewState property can be set to false without any detriment.</span></span>


<span data-ttu-id="ed54a-285">このビデオは、ウィザード コントロールのチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-285">This video is a walkthrough of the Wizard control.</span></span>


![](server-controls/_static/image2.png)


[<span data-ttu-id="ed54a-286">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="ed54a-286">Open Full-Screen Video</span></span>](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a><span data-ttu-id="ed54a-287">コントロールをローカライズします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-287">Localize Control</span></span>

<span data-ttu-id="ed54a-288">Localize コントロールは、Literal コントロールに似ています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-288">The Localize control is similar to a Literal control.</span></span> <span data-ttu-id="ed54a-289">ただし、Localize コントロールには、**モード**に追加されるマークアップを表示する方法を制御するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-289">However, the Localize control has a **Mode** property that controls how markup that is added to it is rendered.</span></span> <span data-ttu-id="ed54a-290">Mode プロパティには、次の値がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="ed54a-290">The Mode property supports the following values:</span></span>

| <span data-ttu-id="ed54a-291">**モード**</span><span class="sxs-lookup"><span data-stu-id="ed54a-291">**Mode**</span></span> | <span data-ttu-id="ed54a-292">**説明**</span><span class="sxs-lookup"><span data-stu-id="ed54a-292">**Explanation**</span></span> |
| --- | --- |
| <span data-ttu-id="ed54a-293">変換</span><span class="sxs-lookup"><span data-stu-id="ed54a-293">Transform</span></span> | <span data-ttu-id="ed54a-294">マークアップは、要求を行っているブラウザーのプロトコルに従って変換されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-294">Markup is transformed according to the protocol of the browser making the request.</span></span> |
| <span data-ttu-id="ed54a-295">PassThrough</span><span class="sxs-lookup"><span data-stu-id="ed54a-295">PassThrough</span></span> | <span data-ttu-id="ed54a-296">マークアップとして表示されます-はします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-296">Markup is rendered as-is.</span></span> |
| <span data-ttu-id="ed54a-297">エンコード</span><span class="sxs-lookup"><span data-stu-id="ed54a-297">Encode</span></span> | <span data-ttu-id="ed54a-298">コントロールに追加されるマークアップは、HtmlEncode を使用してエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-298">Markup that is added to the control is encoded using HtmlEncode.</span></span> |

## <a name="multiview-and-view-controls"></a><span data-ttu-id="ed54a-299">MultiView とビュー コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-299">MultiView and View Controls</span></span>

<span data-ttu-id="ed54a-300">マルチビュー コントロールのビュー コントロールのコンテナーとして機能し、ビュー コントロールが (と同様にパネル コントロール) の他のコントロールのコンテナーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-300">The MultiView control acts as a container for View controls, and the View control acts as a container (much like a Panel control) for other controls.</span></span> <span data-ttu-id="ed54a-301">MultiView コントロール内の各ビューは、1 つのビュー コントロールで表されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-301">Each view in a MultiView control is represented by a single View control.</span></span> <span data-ttu-id="ed54a-302">MultiView の最初のビュー コントロールがビュー 0、2 番目はビュー 1, などです。マルチビュー コントロールの ActiveViewIndex を指定することによって、ビューを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-302">The first View control in the MultiView is view 0, the second is view 1, etc. You can switch views by specifying the ActiveViewIndex of the MultiView control.</span></span>

## <a name="substitution-control"></a><span data-ttu-id="ed54a-303">Substitution コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-303">Substitution Control</span></span>

<span data-ttu-id="ed54a-304">Substitution コントロールは ASP.NET のキャッシュと組み合わせて使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-304">The Substitution control is used in conjunction with ASP.NET caching.</span></span> <span data-ttu-id="ed54a-305">ここでキャッシュを活用するためにしたいが、各要求 (つまり、部分ページのキャッシュから除外されている) に更新する必要があるページの部分がある場合は、代入コンポーネントは、優れたソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-305">In cases where you want to take advantage of caching, but you have portions of a page that must be updated on each request (in other words, portions of a page that are exempt from caching), the Substitution component provides a great solution.</span></span> <span data-ttu-id="ed54a-306">コントロールは、自身で出力を実際にレンダリングしません。</span><span class="sxs-lookup"><span data-stu-id="ed54a-306">The control doesn't actually render any output on its own.</span></span> <span data-ttu-id="ed54a-307">代わりに、サーバー側コード内のメソッドにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-307">Instead, it is bound to a method in server-side code.</span></span> <span data-ttu-id="ed54a-308">ページが要求されたときに、メソッドが呼び出され、返されたマークアップは substitution コントロールの代わりに表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-308">When the page is requested, the method is called and the returned markup is rendered in place of the substitution control.</span></span>

<span data-ttu-id="ed54a-309">代替コントロールがバインドされているメソッドを指定した、 **MethodName**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-309">The method to which the Substitution control is bound is specified via the **MethodName** property.</span></span> <span data-ttu-id="ed54a-310">そのメソッドは、次の条件を満たしている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-310">That method must meet the following criteria:</span></span>

- <span data-ttu-id="ed54a-311">静的 (VB のでは shared) メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-311">It must be a static (shared in VB) method.</span></span>
- <span data-ttu-id="ed54a-312">これは、型 HttpContext の 1 つのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ed54a-312">It accepts one parameter of type HttpContext.</span></span>
- <span data-ttu-id="ed54a-313">ページ上のコントロールを置き換える必要があるマークアップを表す文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-313">It returns a string representing the markup that should replace the control on the page.</span></span>

<span data-ttu-id="ed54a-314">Substitution コントロール ページで、他のコントロールを変更する権限がありませんが、パラメーターを使用して現在の HttpContext へのアクセス。</span><span class="sxs-lookup"><span data-stu-id="ed54a-314">The Substitution control does not have the ability to modify any other control on the page, but it does have access to the current HttpContext via its parameter.</span></span>

## <a name="gridview-control"></a><span data-ttu-id="ed54a-315">GridView コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-315">GridView Control</span></span>

<span data-ttu-id="ed54a-316">GridView コントロールは、DataGrid コントロールに代わるです。</span><span class="sxs-lookup"><span data-stu-id="ed54a-316">The GridView control is the replacement for the DataGrid control.</span></span> <span data-ttu-id="ed54a-317">このコントロールは、後のモジュールで詳しく説明されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-317">This control will be covered in more detail in a later module.</span></span>

## <a name="detailsview-control"></a><span data-ttu-id="ed54a-318">DetailsView コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-318">DetailsView Control</span></span>

<span data-ttu-id="ed54a-319">DetailsView コントロールでは、データ ソースから 1 つのレコードを表示および編集または削除することができます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-319">The DetailsView control allows you to display a single record from a data source and to edit or delete it.</span></span> <span data-ttu-id="ed54a-320">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-320">It is covered in more detail in a later module.</span></span>

## <a name="formview-control"></a><span data-ttu-id="ed54a-321">FormView コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-321">FormView Control</span></span>

<span data-ttu-id="ed54a-322">フォーム ビュー コントロールを使用して、構成可能なインターフェイスで、データ ソースから 1 つのレコードを表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-322">The FormView control is used to display a single record from a datasource in a configurable interface.</span></span> <span data-ttu-id="ed54a-323">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-323">It is covered in more detail in a later module.</span></span>

## <a name="accessdatasource-control"></a><span data-ttu-id="ed54a-324">AccessDataSource コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-324">AccessDataSource Control</span></span>

<span data-ttu-id="ed54a-325">AccessDataSource コントロールが使用するデータは、Access データベースをバインドします。</span><span class="sxs-lookup"><span data-stu-id="ed54a-325">The AccessDataSource control is used to data bind an Access database.</span></span> <span data-ttu-id="ed54a-326">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-326">It is covered in more detail in a later module.</span></span>

## <a name="objectdatasource-control"></a><span data-ttu-id="ed54a-327">ObjectDataSource コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-327">ObjectDataSource Control</span></span>

<span data-ttu-id="ed54a-328">ObjectDataSource コントロールは、コントロールでくデータ バインドされた 2 階層のモデルではなく、中間層のビジネス オブジェクトにコントロールがデータ ソースに直接バインドされてように 3 層アーキテクチャをサポートするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-328">The ObjectDataSource control is used to support a three-tier architecture so that controls can be data-bound to a middle-tier business object as opposed to a two-tiered model where controls are bound directly to the data source.</span></span> <span data-ttu-id="ed54a-329">後のモジュールでさらに詳しく説明されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-329">It will be discussed in more detail in a later module.</span></span>

## <a name="xmldatasource-control"></a><span data-ttu-id="ed54a-330">XmlDataSource コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-330">XmlDataSource Control</span></span>

<span data-ttu-id="ed54a-331">XmlDataSource コントロールは、XML データ ソースへのデータ バインドに使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-331">The XmlDataSource control is used to data bind to an XML data source.</span></span> <span data-ttu-id="ed54a-332">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-332">It is covered in more detail in a later module.</span></span>

## <a name="sitemapdatasource-control"></a><span data-ttu-id="ed54a-333">SiteMapDataSource コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-333">SiteMapDataSource Control</span></span>

<span data-ttu-id="ed54a-334">SiteMapDataSource コントロールは、サイト マップに基づくサイトのナビゲーション コントロールのデータ バインドを提供します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-334">The SiteMapDataSource control provides data binding for site navigation controls based on a site map.</span></span> <span data-ttu-id="ed54a-335">後のモジュールでさらに詳しく説明されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-335">It will be discussed in more detail in a later module.</span></span>

## <a name="sitemappath-control"></a><span data-ttu-id="ed54a-336">SiteMapPath Control</span><span class="sxs-lookup"><span data-stu-id="ed54a-336">SiteMapPath Control</span></span>

<span data-ttu-id="ed54a-337">サイト マップ パス コントロールには、一般に呼ば階層リンク ナビゲーション リンクの系列が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-337">The SiteMapPath control displays a series of navigation links commonly referred to as breadcrumbs.</span></span> <span data-ttu-id="ed54a-338">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-338">It is covered in more detail in a later module.</span></span>

## <a name="menu-control"></a><span data-ttu-id="ed54a-339">メニュー コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-339">Menu Control</span></span>

<span data-ttu-id="ed54a-340">メニュー コントロールは、DHTML を使用して動的メニューを表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-340">The Menu control displays dynamic menus using DHTML.</span></span> <span data-ttu-id="ed54a-341">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-341">It is covered in more detail in a later module.</span></span>

## <a name="treeview-control"></a><span data-ttu-id="ed54a-342">TreeView コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-342">TreeView Control</span></span>

<span data-ttu-id="ed54a-343">TreeView コントロールを使用して、データの階層ツリー ビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-343">The TreeView control is used to display a hierarchical tree view of data.</span></span> <span data-ttu-id="ed54a-344">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-344">It is covered in more detail in a later module.</span></span>

## <a name="login-control"></a><span data-ttu-id="ed54a-345">ログイン コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-345">Login Control</span></span>

<span data-ttu-id="ed54a-346">ログイン コントロールは、Web サイトにログインするためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-346">The Login control provides for a mechanism to log into a Web site.</span></span> <span data-ttu-id="ed54a-347">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-347">It is covered in more detail in a later module.</span></span>

## <a name="loginview-control"></a><span data-ttu-id="ed54a-348">LoginView コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-348">LoginView Control</span></span>

<span data-ttu-id="ed54a-349">LoginView コントロールは、ユーザーのログイン状態に基づいて異なるテンプレートの表示にできます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-349">The LoginView control allows for the display of different templates based upon a user's login status.</span></span> <span data-ttu-id="ed54a-350">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-350">It is covered in more detail in a later module.</span></span>

## <a name="passwordrecovery-control"></a><span data-ttu-id="ed54a-351">PasswordRecovery コントロール</span><span class="sxs-lookup"><span data-stu-id="ed54a-351">PasswordRecovery Control</span></span>

<span data-ttu-id="ed54a-352">PasswordRecovery コントロールを ASP.NET アプリケーションのユーザーがパスワードを忘れた場合を取得するされます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-352">The PasswordRecovery control is used to retrieve forgotten passwords by users of an ASP.NET application.</span></span> <span data-ttu-id="ed54a-353">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-353">It is covered in more detail in a later module.</span></span>

## <a name="loginstatus"></a><span data-ttu-id="ed54a-354">LoginStatus</span><span class="sxs-lookup"><span data-stu-id="ed54a-354">LoginStatus</span></span>

<span data-ttu-id="ed54a-355">ログイン状態コントロールは、ユーザーのログイン状態を表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-355">The LoginStatus control displays a user's login status.</span></span> <span data-ttu-id="ed54a-356">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-356">It is covered in more detail in a later module.</span></span>

## <a name="loginname"></a><span data-ttu-id="ed54a-357">LoginName</span><span class="sxs-lookup"><span data-stu-id="ed54a-357">LoginName</span></span>

<span data-ttu-id="ed54a-358">LoginName コントロールは、ASP.NET アプリケーションにログインしている後に、ユーザーのユーザー名を表示します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-358">The LoginName control displays a user's username after being logged into an ASP.NET application.</span></span> <span data-ttu-id="ed54a-359">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-359">It is covered in more detail in a later module.</span></span>

## <a name="createuserwizard"></a><span data-ttu-id="ed54a-360">CreateUserWizard</span><span class="sxs-lookup"><span data-stu-id="ed54a-360">CreateUserWizard</span></span>

<span data-ttu-id="ed54a-361">CreateUserWizard は、構成可能なウィザードによってユーザーは、ASP.NET アプリケーションで使用する ASP.NET メンバーシップ アカウントを作成する権限です。</span><span class="sxs-lookup"><span data-stu-id="ed54a-361">The CreateUserWizard is a configurable wizard which gives users the ability to create an ASP.NET Membership account for use in an ASP.NET application.</span></span> <span data-ttu-id="ed54a-362">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-362">It is covered in more detail in a later module.</span></span>

## <a name="changepassword"></a><span data-ttu-id="ed54a-363">ChangePassword</span><span class="sxs-lookup"><span data-stu-id="ed54a-363">ChangePassword</span></span>

<span data-ttu-id="ed54a-364">ChangePassword コントロールでは、ASP.NET アプリケーションのパスワードを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="ed54a-364">The ChangePassword control allows users to change their password for an ASP.NET application.</span></span> <span data-ttu-id="ed54a-365">これは、後のモジュールでさらに詳しくについて説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-365">It is covered in more detail in a later module.</span></span>

## <a name="various-webparts"></a><span data-ttu-id="ed54a-366">さまざまな web パーツ</span><span class="sxs-lookup"><span data-stu-id="ed54a-366">Various WebParts</span></span>

<span data-ttu-id="ed54a-367">ASP.NET 2.0 では、さまざまな Web パーツに付属します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-367">ASP.NET 2.0 ships with various Web Parts.</span></span> <span data-ttu-id="ed54a-368">これらについては、後のモジュールで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="ed54a-368">These will be covered in detail in a later module.</span></span>
