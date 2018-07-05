---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: サーバー コントロール |Microsoft Docs
author: microsoft
description: ASP.NET 2.0 では、さまざまな方法でサーバー コントロールを強化します。 このモジュールでいくつかの方法が ASP.NET 2.0 と Visual Studio 200 にアーキテクチャの変更について説明します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 989986c45e76a65582f35172c7d837a78484d09a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374225"
---
<a name="server-controls"></a><span data-ttu-id="d918d-104">サーバー コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-104">Server Controls</span></span>
====================
<span data-ttu-id="d918d-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d918d-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d918d-106">ASP.NET 2.0 では、さまざまな方法でサーバー コントロールを強化します。</span><span class="sxs-lookup"><span data-stu-id="d918d-106">ASP.NET 2.0 enhances server controls in many ways.</span></span> <span data-ttu-id="d918d-107">このモジュールでいくつかの方法は、ASP.NET 2.0 には、アーキテクチャの変更について説明し、Visual Studio 2005 は、サーバー コントロールを処理します。</span><span class="sxs-lookup"><span data-stu-id="d918d-107">In this module, we'll cover some of the architectural changes to the way ASP.NET 2.0 and Visual Studio 2005 deals with server controls.</span></span>


<span data-ttu-id="d918d-108">ASP.NET 2.0 では、さまざまな方法でサーバー コントロールを強化します。</span><span class="sxs-lookup"><span data-stu-id="d918d-108">ASP.NET 2.0 enhances server controls in many ways.</span></span> <span data-ttu-id="d918d-109">このモジュールでいくつかの方法は、ASP.NET 2.0 には、アーキテクチャの変更について説明し、Visual Studio 2005 は、サーバー コントロールを処理します。</span><span class="sxs-lookup"><span data-stu-id="d918d-109">In this module, we'll cover some of the architectural changes to the way ASP.NET 2.0 and Visual Studio 2005 deals with server controls.</span></span>

## <a name="view-state"></a><span data-ttu-id="d918d-110">ビュー ステート</span><span class="sxs-lookup"><span data-stu-id="d918d-110">View state</span></span>

<span data-ttu-id="d918d-111">ASP.NET 2.0 でビューステートの主な変更は、サイズが大幅に短縮します。</span><span class="sxs-lookup"><span data-stu-id="d918d-111">The primary change in view state in ASP.NET 2.0 is a dramatic reduction in size.</span></span> <span data-ttu-id="d918d-112">カレンダー コントロールにあるページを検討してください。</span><span class="sxs-lookup"><span data-stu-id="d918d-112">Consider a page with only a Calendar control on it.</span></span> <span data-ttu-id="d918d-113">ASP.NET 1.1 では、ビュー ステートを示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-113">Here is the view state in ASP.NET 1.1.</span></span>

[!code-css[Main](server-controls/samples/sample1.css)]

<span data-ttu-id="d918d-114">今すぐ次に示しますビュー ステートのと同じページに ASP.NET 2.0 で。</span><span class="sxs-lookup"><span data-stu-id="d918d-114">Now here's the view state on an identical page in ASP.NET 2.0.</span></span>

[!code-css[Main](server-controls/samples/sample2.css)]

<span data-ttu-id="d918d-115">非常に重大な変更、ビュー ステートをネットワーク経由で送受信実行を検討して、この変更開発者大幅なパフォーマンスが向上です。</span><span class="sxs-lookup"><span data-stu-id="d918d-115">That's a pretty significant change, and considering that view state is carried back and forth over the wire, this change can give developers a significant performance increase.</span></span> <span data-ttu-id="d918d-116">ビュー ステートのサイズを縮小では、主に内部的に処理した方法によるものです。</span><span class="sxs-lookup"><span data-stu-id="d918d-116">The reduction in size of view state is largely due to the way that we handle it internally.</span></span> <span data-ttu-id="d918d-117">ビュー ステートは、Base64 にエンコードされた文字列に注意してください。</span><span class="sxs-lookup"><span data-stu-id="d918d-117">Remember that view state is a Base64 encoded string.</span></span> <span data-ttu-id="d918d-118">ASP.NET 2.0 でビューステートの変更をより深く理解するには、上記の例からデコードされた値を見てをみましょうがあります。</span><span class="sxs-lookup"><span data-stu-id="d918d-118">To better understand the change in view state in ASP.NET 2.0, let's have a look at the decoded values from the examples above.</span></span>

<span data-ttu-id="d918d-119">デコードされた 1.1 のビュー ステートを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-119">Here is the 1.1 view state decoded:</span></span>

[!code-css[Main](server-controls/samples/sample3.css)]

<span data-ttu-id="d918d-120">これは少しでたらめに見えるが、ここで、パターンがあります。</span><span class="sxs-lookup"><span data-stu-id="d918d-120">This may look a bit like gibberish, but there is a pattern here.</span></span> <span data-ttu-id="d918d-121">Asp.net 1.x では、1 つの文字データ型を識別するために使用し、使用して値を区切り、、 &lt; &gt;文字。</span><span class="sxs-lookup"><span data-stu-id="d918d-121">In ASP.NET 1.x, we used single characters to identify data-types and delimited values using the &lt;&gt; characters.</span></span> <span data-ttu-id="d918d-122">上記のビュー ステートのサンプルの"t"は、3 つを表します。</span><span class="sxs-lookup"><span data-stu-id="d918d-122">The "t" in the view state sample above represents a Triplet.</span></span> <span data-ttu-id="d918d-123">うちには ("l"は、ArrayList を表します)。 Arraylist のペアが含まれていますこれらの Arraylist の int32 型が含まれています ("i") 1 および、その他の値を持つ別の組が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d918d-123">The Triplet contains a pair of ArrayLists (the "l" represents an ArrayList.) One of those ArrayLists contains an Int32 ("i") with a value of 1 and the other contains another Triplet.</span></span> <span data-ttu-id="d918d-124">うちには、Arraylist などのペアが含まれています。重要なことことのペアを含む Triplets 使用して、文字を使用してデータ型を特定および使用して、&lt;と&gt;区切り記号として文字。</span><span class="sxs-lookup"><span data-stu-id="d918d-124">The Triplet contains a pair of ArrayLists, etc. The important thing to remember is that we use Triplets that contain pairs, we identify the data-types via a letter, and we use the &lt; and &gt; characters as delimiters.</span></span>

<span data-ttu-id="d918d-125">ASP.NET 2.0 でデコードされたビュー ステートは少し異なります。</span><span class="sxs-lookup"><span data-stu-id="d918d-125">In ASP.NET 2.0, the decoded view state looks a bit different.</span></span>

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

<span data-ttu-id="d918d-126">デコードされたビュー ステートの外観に大きな変更を確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d918d-126">You should notice a huge change in the appearance of the decoded view state.</span></span> <span data-ttu-id="d918d-127">この変更は、いくつかのアーキテクチャの基盤です。</span><span class="sxs-lookup"><span data-stu-id="d918d-127">This change has several architectural underpinnings.</span></span> <span data-ttu-id="d918d-128">ビュー ステートで ASP.NET 1.x がデータをシリアル化に、LosFormatter を使用します。</span><span class="sxs-lookup"><span data-stu-id="d918d-128">View state in ASP.NET 1.x used the LosFormatter to serialize data.</span></span> <span data-ttu-id="d918d-129">2.0 では、新しい ObjectStateFormatter クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d918d-129">In 2.0, we use the new ObjectStateFormatter class.</span></span> <span data-ttu-id="d918d-130">このクラスは、ビュー ステートとコントロールの状態の逆シリアル化とシリアル化を支援するために設計されています。</span><span class="sxs-lookup"><span data-stu-id="d918d-130">This class was specifically designed to aid in the serialization and deserialization of view state and control state.</span></span> <span data-ttu-id="d918d-131">(コントロールの状態は、次のセクションで説明されます)。シリアル化および逆シリアル化実行メソッドを変更することで得られる多くの利点があります。</span><span class="sxs-lookup"><span data-stu-id="d918d-131">(Control state will be covered in the next section.) There are many benefits gained by changing the method by which serialization and deserialization take place.</span></span> <span data-ttu-id="d918d-132">最も大幅なパフォーマンスの 1 つは、使用することを TextWriter を使用する LosFormatter とは異なり、ObjectStateFormatter BinaryWriter です。</span><span class="sxs-lookup"><span data-stu-id="d918d-132">One of the most dramatic is the fact that unlike the LosFormatter which uses a TextWriter, the ObjectStateFormatter uses a BinaryWriter.</span></span> <span data-ttu-id="d918d-133">これにより、ビュー ステート、一連の文字列ではなくバイトを格納する ASP.NET 2.0 ができます。</span><span class="sxs-lookup"><span data-stu-id="d918d-133">This allows ASP.NET 2.0 to store view state a series of bytes instead of strings.</span></span> <span data-ttu-id="d918d-134">たとえば、整数を取得します。</span><span class="sxs-lookup"><span data-stu-id="d918d-134">Take, for example, an integer.</span></span> <span data-ttu-id="d918d-135">ASP.NET 1.1 では、整数値には、ビュー ステートの 4 バイトが必要です。</span><span class="sxs-lookup"><span data-stu-id="d918d-135">In ASP.NET 1.1, an integer required 4 bytes of view state.</span></span> <span data-ttu-id="d918d-136">ASP.NET 2.0 でその同じ整数には、1 バイトのみが必要です。</span><span class="sxs-lookup"><span data-stu-id="d918d-136">In ASP.NET 2.0, that same integer only requires 1 byte.</span></span> <span data-ttu-id="d918d-137">他の機能強化が格納されているビュー ステートの量を減らして行われました。</span><span class="sxs-lookup"><span data-stu-id="d918d-137">Other enhancements were made to decrease the amount of view state that is stored.</span></span> <span data-ttu-id="d918d-138">たとえば、DateTime 値は文字列ではなく、TickCount を使用してを格納はようになりました。</span><span class="sxs-lookup"><span data-stu-id="d918d-138">DateTime values, for example, are now stored using a TickCount instead of a string.</span></span>

<span data-ttu-id="d918d-139">すべてを言えば、まるで DataGrid と同様のコントロールを 1.x での表示状態の最大消費者のいずれかがファクトに特別な注意を払いました。</span><span class="sxs-lookup"><span data-stu-id="d918d-139">As if all of that weren't enough, special attention was paid to the fact that one of the greatest consumers of view state in 1.x was the DataGrid and similar controls.</span></span> <span data-ttu-id="d918d-140">ビュー ステートは DataGrid などのコントロールの主な欠点は、大量の情報が繰り返される多くの場合、含まれることです。</span><span class="sxs-lookup"><span data-stu-id="d918d-140">A major drawback of controls such as the DataGrid where view state is concerned is that it often contains large amounts of repeated information.</span></span> <span data-ttu-id="d918d-141">Asp.net 1.x では、結果として再度肥大化の表示状態および経由で情報が格納だけが繰り返し表示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-141">In ASP.NET 1.x, that repeated information was simply stored over and over again resulting in a bloated view state.</span></span> <span data-ttu-id="d918d-142">ASP.NET 2.0 では、このようなデータを格納するのに新しい IndexedString クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d918d-142">In ASP.NET 2.0, we use the new IndexedString class to store such data.</span></span> <span data-ttu-id="d918d-143">文字列が繰り返される場合、IndexedString と IndexedString オブジェクトの実行中のテーブル内のインデックスのトークンだけ格納します。</span><span class="sxs-lookup"><span data-stu-id="d918d-143">If a string repeats, we just store the token for the IndexedString and the index within a running table of IndexedString objects.</span></span>

## <a name="control-state"></a><span data-ttu-id="d918d-144">コントロールの状態</span><span class="sxs-lookup"><span data-stu-id="d918d-144">Control State</span></span>

<span data-ttu-id="d918d-145">HTTP ペイロードに追加されたサイズではビュー ステートで開発者がいた主要な gripes のいずれか。</span><span class="sxs-lookup"><span data-stu-id="d918d-145">One of the major gripes that developers had with view state was the size that it added to the HTTP payload.</span></span> <span data-ttu-id="d918d-146">既に説明したように、DataGrid コントロールのビューステートの最大消費者の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="d918d-146">As previously mentioned, one of the greatest consumers of view state is the DataGrid control.</span></span> <span data-ttu-id="d918d-147">データ グリッドによって生成されたビュー ステートの膨大な量を避けるためには、多くの開発者には、そのコントロールのビューステート単に無効になります。</span><span class="sxs-lookup"><span data-stu-id="d918d-147">To avoid the huge amounts of view state generated by a DataGrid, many developers simply disabled view state for that control.</span></span> <span data-ttu-id="d918d-148">残念ながら、そのソリューションが常に良いものにします。</span><span class="sxs-lookup"><span data-stu-id="d918d-148">Unfortunately, that solution wasn't always a good one.</span></span> <span data-ttu-id="d918d-149">ビューステート 1.x には、ASP.NET には、コントロールの適切な機能に必要なデータだけでなくが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d918d-149">View state in ASP.NET 1.x contains not only data necessary for the correct functionality of the control.</span></span> <span data-ttu-id="d918d-150">コントロールの UI の状態に関する情報も含まれています。</span><span class="sxs-lookup"><span data-stu-id="d918d-150">It also contains information concerning the state of the control's UI.</span></span> <span data-ttu-id="d918d-151">つまり、すべてを表示する UI 情報を必要としない場合でも、ビュー ステートを有効にする必要があります、DataGrid に改ページ調整を許可する場合の状態が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d918d-151">This means that if you want to allow for pagination on a DataGrid, you must enable view state even if you don't need all of the UI information that view state contains.</span></span> <span data-ttu-id="d918d-152">これは、オール_オア_ナッシングのシナリオです。</span><span class="sxs-lookup"><span data-stu-id="d918d-152">It's an all-or-nothing scenario.</span></span>

<span data-ttu-id="d918d-153">ASP.NET 2.0 では、コントロールの状態は、コントロールの状態の概要を適切に使用してその問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="d918d-153">In ASP.NET 2.0, control state solves that problem nicely via the introduction of control state.</span></span> <span data-ttu-id="d918d-154">コントロールの状態には、コントロールの適切な機能を絶対に必要なデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d918d-154">Control state contains the data that is absolutely necessary for the proper functionality of a control.</span></span> <span data-ttu-id="d918d-155">ビューの状態とは異なりは、コントロールの状態を無効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="d918d-155">Unlike view state, control state cannot be disabled.</span></span> <span data-ttu-id="d918d-156">したがって、コントロールの状態に格納されているデータを慎重に制御される重要なは。</span><span class="sxs-lookup"><span data-stu-id="d918d-156">Therefore, it is important that the data being stored in control state is carefully controlled.</span></span>

> [!NOTE]
> <span data-ttu-id="d918d-157">ビュー ステートとコントロールの状態が永続化、 \_ \_VIEWSTATE 隠しフォーム フィールド。</span><span class="sxs-lookup"><span data-stu-id="d918d-157">Control state is persisted along with the view state in the \_\_VIEWSTATE hidden form field.</span></span>


<span data-ttu-id="d918d-158">このビデオでは、ビュー ステートとコントロールの状態のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="d918d-158">This video is a walkthrough of view state and control state.</span></span>


![](server-controls/_static/image1.png)


[<span data-ttu-id="d918d-159">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="d918d-159">Open Full-Screen Video</span></span>](server-controls/_static/state1.wmv)


<span data-ttu-id="d918d-160">サーバー コントロールで読み取りし、書き込み状態を制御するためには、3 つの手順を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="d918d-160">In order for a server control to read and write to control state, you must take three steps.</span></span>

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a><span data-ttu-id="d918d-161">手順 1: RegisterRequiresControlState メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="d918d-161">Step 1: Call the RegisterRequiresControlState Method</span></span>

<span data-ttu-id="d918d-162">RegisterRequiresControlState メソッドは、コントロールがコントロールの状態を保持する必要があることを ASP.NET に通知します。</span><span class="sxs-lookup"><span data-stu-id="d918d-162">The RegisterRequiresControlState method informs ASP.NET that a control needs to persist control state.</span></span> <span data-ttu-id="d918d-163">型が登録されているコントロールにあるコントロールの 1 つの引数がかかります。</span><span class="sxs-lookup"><span data-stu-id="d918d-163">It takes one argument of type Control which is the control that is being registered.</span></span>

<span data-ttu-id="d918d-164">登録に、要求が保持されないことに注意してください。 重要です。</span><span class="sxs-lookup"><span data-stu-id="d918d-164">It is important to note that registration does not persist from request to request.</span></span> <span data-ttu-id="d918d-165">そのため、このメソッドはコントロールがコントロールの状態を保持する場合、要求ごとに呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="d918d-165">Therefore, this method must be called on every request if a control is to persist control state.</span></span> <span data-ttu-id="d918d-166">Oninit メソッドで、メソッドを呼び出すことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d918d-166">It is recommended that the method be called in OnInit.</span></span>

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a><span data-ttu-id="d918d-167">手順 2: SaveControlState をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="d918d-167">Step 2: Override SaveControlState</span></span>

<span data-ttu-id="d918d-168">SaveControlState メソッドは、最新の投稿に戻るためのコントロールの状態の変更をコントロールに保存します。</span><span class="sxs-lookup"><span data-stu-id="d918d-168">The SaveControlState method saves control state changes for a control since the last post back.</span></span> <span data-ttu-id="d918d-169">コントロールの状態を表すオブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="d918d-169">It returns an object representing the control's state.</span></span>

## <a name="step-3-override-loadcontrolstate"></a><span data-ttu-id="d918d-170">手順 3: LoadControlState をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="d918d-170">Step 3: Override LoadControlState</span></span>

<span data-ttu-id="d918d-171">LoadControlState メソッドは、コントロールに保存された状態を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="d918d-171">The LoadControlState method loads saved state into a control.</span></span> <span data-ttu-id="d918d-172">メソッドは、コントロールの保存された状態を保持するオブジェクトの種類の 1 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="d918d-172">The method takes one argument of type Object which holds the saved state for the control.</span></span>

## <a name="full-xhtml-compliance"></a><span data-ttu-id="d918d-173">完全な XHTML 準拠</span><span class="sxs-lookup"><span data-stu-id="d918d-173">Full XHTML Compliance</span></span>

<span data-ttu-id="d918d-174">Web 開発者は、Web アプリケーションで標準の重要性を認識します。</span><span class="sxs-lookup"><span data-stu-id="d918d-174">Any Web developer knows the importance of standards in Web applications.</span></span> <span data-ttu-id="d918d-175">標準ベースの開発環境を維持するためには、ASP.NET 2.0 と、XHTML 準拠では完全にします。</span><span class="sxs-lookup"><span data-stu-id="d918d-175">In order to maintain a standards-based development environment, ASP.NET 2.0 is fully XHTML compliant.</span></span> <span data-ttu-id="d918d-176">したがって、すべてのタグは、HTML 4.0 をサポートするブラウザーで XHTML 標準に従って表示されるか、大きいです。</span><span class="sxs-lookup"><span data-stu-id="d918d-176">Therefore, all tags are rendered according to XHTML standards in browsers that support HTML 4.0 or greater.</span></span>

<span data-ttu-id="d918d-177">ASP.NET 1.1 DOCTYPE 定義は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d918d-177">The DOCTYPE definition in ASP.NET 1.1 was as follows:</span></span>

[!code-html[Main](server-controls/samples/sample6.html)]

<span data-ttu-id="d918d-178">ASP.NET 2.0 で既定の DOCTYPE 定義のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d918d-178">In ASP.NET 2.0, the default DOCTYPE definition is as follows:</span></span>

[!code-html[Main](server-controls/samples/sample7.html)]

<span data-ttu-id="d918d-179">選択した場合は、構成ファイルで xhtmlConformance ノードを介して既定 XHML コンプライアンスを変更できます。</span><span class="sxs-lookup"><span data-stu-id="d918d-179">If you choose, you can alter the default XHML compliance via the xhtmlConformance node in the configuration file.</span></span> <span data-ttu-id="d918d-180">たとえば、web.config ファイルで、次のノードは XHTML 1.0 Strict を XHTML 準拠を変更します。</span><span class="sxs-lookup"><span data-stu-id="d918d-180">For example, the following node in the web.config file will change XHTML compliance to XHTML 1.0 Strict:</span></span>

[!code-xml[Main](server-controls/samples/sample8.xml)]

<span data-ttu-id="d918d-181">選択した場合、構成することも、従来の ASP.NET で使用される構成を使用する ASP.NET 1.x として次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d918d-181">If you choose, you can also configure ASP.NET to use the legacy configuration used in ASP.NET 1.x as follows:</span></span>

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a><span data-ttu-id="d918d-182">アダプティブ アダプターを使用して表示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-182">Adaptive Rendering Using Adapters</span></span>

<span data-ttu-id="d918d-183">Asp.net 1.x では、構成ファイルに含まれている、 &lt;browserCaps&gt; HttpBrowserCapabilities オブジェクトを作成するセクション。</span><span class="sxs-lookup"><span data-stu-id="d918d-183">In ASP.NET 1.x, the configuration file contained a &lt;browserCaps&gt; section that populated a HttpBrowserCapabilities object.</span></span> <span data-ttu-id="d918d-184">このオブジェクトは、開発者はどのようなデバイスは、特定の要求を行ってを特定し、コードを適切にレンダリングを許可します。</span><span class="sxs-lookup"><span data-stu-id="d918d-184">This object allowed a developer to determine what device is making a particular request and render code appropriately.</span></span> <span data-ttu-id="d918d-185">ASP.NET 2.0 では、モデルが改善され、今すぐ新しい ControlAdapter クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="d918d-185">In ASP.NET 2.0, the model has improved and now uses the new ControlAdapter class.</span></span> <span data-ttu-id="d918d-186">ControlAdapter クラスは、コントロールのライフ サイクルのイベントをオーバーライドし、ユーザー エージェントの機能に基づいてコントロールのレンダリングを制御します。</span><span class="sxs-lookup"><span data-stu-id="d918d-186">The ControlAdapter class overrides events in the control's lifecycle and controls the rendering of controls based upon the user agent's capabilities.</span></span> <span data-ttu-id="d918d-187">特定のユーザー エージェントの機能は、c:\windows\microsoft.net\framework\v2.0 に格納されているブラウザー定義ファイル (.browser ファイルの拡張子を持つファイル) によって定義されます。\* \* \* \*\CONFIG\Browsers フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d918d-187">The capabilities of a specific user agent are defined by a browser definition file (a file with a .browser file extension) stored in the c:\windows\microsoft.net\framework\v2.0.\*\*\*\*\CONFIG\Browsers folder.</span></span>

> [!NOTE]
> <span data-ttu-id="d918d-188">ControlAdapter クラスは抽象クラスです。</span><span class="sxs-lookup"><span data-stu-id="d918d-188">The ControlAdapter class is an abstract class.</span></span>


<span data-ttu-id="d918d-189">ほぼ同じように、 &lt;browserCaps&gt; 1.x では、ブラウザー定義ファイルのセクションでは、正規の式を使用して、要求側のブラウザーを識別するために、ユーザー エージェント文字列を解析します。</span><span class="sxs-lookup"><span data-stu-id="d918d-189">Much like the &lt;browserCaps&gt; section in 1.x, the browser definition file uses a Regular Expression to parse the user agent string in order to identify the requesting browser.</span></span> <span data-ttu-id="d918d-190">これは、ユーザー エージェントに対して特定の機能を定義しています。</span><span class="sxs-lookup"><span data-stu-id="d918d-190">It them defines particular capabilities for that user agent.</span></span> <span data-ttu-id="d918d-191">ControlAdapter Render メソッドを使用してコントロールをレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="d918d-191">The ControlAdapter renders the control via the Render method.</span></span> <span data-ttu-id="d918d-192">そのため、Render メソッドをオーバーライドする場合呼び出す必要はありませんレンダリング、基本クラス。</span><span class="sxs-lookup"><span data-stu-id="d918d-192">Therefore, if you override the Render method, you should not call Render on the base class.</span></span> <span data-ttu-id="d918d-193">これは、アダプターで、コントロール自体の 1 回、2 回発生するレンダリングにあります。</span><span class="sxs-lookup"><span data-stu-id="d918d-193">Doing so may cause rendering to occur twice, once for the adapter and once for the control itself.</span></span>

## <a name="developing-a-custom-adapter"></a><span data-ttu-id="d918d-194">カスタム アダプターの開発</span><span class="sxs-lookup"><span data-stu-id="d918d-194">Developing a Custom Adapter</span></span>

<span data-ttu-id="d918d-195">ControlAdapter から継承することによって、独自のカスタム アダプターを開発できます。</span><span class="sxs-lookup"><span data-stu-id="d918d-195">You can develop your own custom adapter by inheriting from ControlAdapter.</span></span> <span data-ttu-id="d918d-196">さらに、アダプターは、ページを必要な場合は、抽象クラス PageAdapter から継承することができます。</span><span class="sxs-lookup"><span data-stu-id="d918d-196">Additionally, you can inherit from the abstract class PageAdapter in cases where an adapter is needed for a page.</span></span> <span data-ttu-id="d918d-197">コントロールのカスタム アダプターへのマッピングが使用して実現、 &lt;controlAdapters&gt;ブラウザー定義ファイル内の要素。</span><span class="sxs-lookup"><span data-stu-id="d918d-197">Mapping of controls to your custom adapter is accomplished via the &lt;controlAdapters&gt; element in the browser definition file.</span></span> <span data-ttu-id="d918d-198">たとえば、ブラウザー定義ファイルから次の XML は、メニュー コントロールを MenuAdapter クラスにマッピングされます。</span><span class="sxs-lookup"><span data-stu-id="d918d-198">For example, the following XML from a browser definition file maps the Menu control to the MenuAdapter class:</span></span>

[!code-html[Main](server-controls/samples/sample10.html)]

<span data-ttu-id="d918d-199">このモデルを使用して、コントロールの開発者が特定のデバイスまたはブラウザーを対象とする非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="d918d-199">Using this model, it becomes quite easy for a control developer to target a particular device or browser.</span></span> <span data-ttu-id="d918d-200">すべてのデバイスでのページのレンダリングを完全に制御する開発者にとって非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="d918d-200">It's also quite simple for a developer to have complete control over how pages render on every device.</span></span>

## <a name="per-device-rendering"></a><span data-ttu-id="d918d-201">デバイスごとのレンダリング</span><span class="sxs-lookup"><span data-stu-id="d918d-201">Per-Device Rendering</span></span>

<span data-ttu-id="d918d-202">ASP.NET 2.0 でのサーバー コントロールのプロパティは、指定したデバイスごとのブラウザー固有のプレフィックスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d918d-202">Server control properties in ASP.NET 2.0 can be specified per-device using a browser-specific prefix.</span></span> <span data-ttu-id="d918d-203">たとえば、次のコードには、によっては、ページを参照するどのデバイスが使用されるラベルのテキストが変わります。</span><span class="sxs-lookup"><span data-stu-id="d918d-203">For example, the code below will change the Text of a label depending upon which device is being used to browse the page.</span></span>

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

<span data-ttu-id="d918d-204">このラベルを含むページが Internet Explorer から参照されると、ラベルが「を参照している Internet Explorer から」というテキストに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-204">When the page containing this label is browsed from Internet Explorer, the label will display text saying "You are browsing from Internet Explorer."</span></span> <span data-ttu-id="d918d-205">Firefox からページが参照されると、ラベルのテキストを表示」を参照している Firefox から"</span><span class="sxs-lookup"><span data-stu-id="d918d-205">When the page is browsed from Firefox, the label will display the text "You are browsing from Firefox."</span></span> <span data-ttu-id="d918d-206">その他の任意のデバイスから、ページが参照されると、「を参照する不明なデバイスから」が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-206">When the page is browsed from any other device, it will display "You are browsing from an unknown device."</span></span> <span data-ttu-id="d918d-207">この特別な構文を使用して、任意のプロパティを指定できます。</span><span class="sxs-lookup"><span data-stu-id="d918d-207">Any property can be specified using this special syntax.</span></span>

## <a name="setting-focus"></a><span data-ttu-id="d918d-208">フォーカスの設定</span><span class="sxs-lookup"><span data-stu-id="d918d-208">Setting Focus</span></span>

<span data-ttu-id="d918d-209">ASP.NET 1.x の開発者は、特定のコントロールに初期フォーカスを設定する方法についてよく寄せられます。</span><span class="sxs-lookup"><span data-stu-id="d918d-209">ASP.NET 1.x developers frequently asked about how to set initial focus on a particular control.</span></span> <span data-ttu-id="d918d-210">たとえば、[ログイン] ページで、ユーザー ID ボックスに、ページが初めて読み込まれるときに、フォーカスを取得すると便利ですが。</span><span class="sxs-lookup"><span data-stu-id="d918d-210">For example, on a login page, it's useful to have the User ID textbox get the focus when the page first loads.</span></span> <span data-ttu-id="d918d-211">Asp.net 1.x では、これを行ういくつかのクライアント側スクリプトの記述が必要です。</span><span class="sxs-lookup"><span data-stu-id="d918d-211">In ASP.NET 1.x, doing this required writing some client-side script.</span></span> <span data-ttu-id="d918d-212">このようなスクリプトには、簡単なタスクが、SetFocus メソッドに協力してくれた ASP.NET 2.0 では必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d918d-212">Even though such a script is a trivial task, it's no longer necessary in ASP.NET 2.0 thanks to the SetFocus method.</span></span> <span data-ttu-id="d918d-213">SetFocus メソッドは、コントロール フォーカスを受け取る必要があることを示す 1 つの引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="d918d-213">The SetFocus method takes one argument indicating the control that should receive focus.</span></span> <span data-ttu-id="d918d-214">この引数を文字列として、コントロールのクライアント ID またはコントロール オブジェクトとして、サーバー コントロールの名前指定できます。</span><span class="sxs-lookup"><span data-stu-id="d918d-214">This argument can either be the client ID of the control as a string or the name of the Server control as a Control object.</span></span> <span data-ttu-id="d918d-215">たとえば、txtUserID というページが初めて読み込まれるときにテキスト ボックス コントロールを初期フォーカスを設定するページに次のコードを追加\_負荷。</span><span class="sxs-lookup"><span data-stu-id="d918d-215">For example, to set the initial focus to a TextBox control called txtUserID when the page first loads, add the following code to Page\_Load:</span></span>

[!code-csharp[Main](server-controls/samples/sample12.cs)]

<span data-ttu-id="d918d-216">--または</span><span class="sxs-lookup"><span data-stu-id="d918d-216">-- or</span></span>

[!code-csharp[Main](server-controls/samples/sample13.cs)]

<span data-ttu-id="d918d-217">ASP.NET 2.0 では、フォーカスを設定するクライアント側の関数を表示するために (前に説明した) Webresource.axd ハンドラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="d918d-217">ASP.NET 2.0 uses the Webresource.axd handler (discussed previously) to render a client-side function that sets the focus.</span></span> <span data-ttu-id="d918d-218">クライアント側の関数の名前は、web フォーム\_AutoFocus 次のようにします。</span><span class="sxs-lookup"><span data-stu-id="d918d-218">The name of the client-side function is WebForm\_AutoFocus as shown here:</span></span>

[!code-html[Main](server-controls/samples/sample14.html)]

<span data-ttu-id="d918d-219">または、コントロールの Focus メソッドを使用して、そのコントロールに初期フォーカスを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="d918d-219">Alternatively, you can use the Focus method for a control to set the initial focus to that control.</span></span> <span data-ttu-id="d918d-220">Focus メソッドでは、コントロール クラスから派生し、すべての ASP.NET 2.0 コントロールを使用します。</span><span class="sxs-lookup"><span data-stu-id="d918d-220">The Focus method derives from the Control class and is available to all ASP.NET 2.0 controls.</span></span> <span data-ttu-id="d918d-221">検証エラーが発生したときに、特定のコントロールにフォーカスを設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="d918d-221">It is also possible to set focus to a particular control when a validation error occurs.</span></span> <span data-ttu-id="d918d-222">については、以降の章で説明します。</span><span class="sxs-lookup"><span data-stu-id="d918d-222">That will be covered in a later module.</span></span>

## <a name="new-server-controls-in-aspnet-20"></a><span data-ttu-id="d918d-223">ASP.NET 2.0 の新しいサーバー コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-223">New Server Controls in ASP.NET 2.0</span></span>

<span data-ttu-id="d918d-224">ASP.NET 2.0 で新しいサーバー コントロールを次に示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-224">The following are new server controls in ASP.NET 2.0.</span></span> <span data-ttu-id="d918d-225">以降の章のいくつか詳細に移動されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-225">We will go into more detail on some of them in later modules.</span></span>

## <a name="imagemap-control"></a><span data-ttu-id="d918d-226">ImageMap コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-226">ImageMap Control</span></span>

<span data-ttu-id="d918d-227">ImageMap コントロールをポストバックを開始または URL に移動できるイメージにホット スポットを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="d918d-227">The ImageMap control allows you to add hotspots to an image that can initiate a post back or navigate to a URL.</span></span> <span data-ttu-id="d918d-228">使用可能なホット スポットの 3 つの種類があります。[Circlehotspot]、RectangleHotSpot、および PolygonHotSpot です。</span><span class="sxs-lookup"><span data-stu-id="d918d-228">There are three types of hotspots available; CircleHotSpot, RectangleHotSpot, and PolygonHotSpot.</span></span> <span data-ttu-id="d918d-229">ホット スポットは、Visual Studio またはプログラムによってコードでは、コレクション エディターを使用して追加されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-229">Hotspots are added via a collection editor in Visual Studio or programmatically in code.</span></span> <span data-ttu-id="d918d-230">イメージにホット スポットを描画するために使用可能なユーザー インターフェイスはありません。</span><span class="sxs-lookup"><span data-stu-id="d918d-230">There is no user-interface available for drawing hotspots on an image.</span></span> <span data-ttu-id="d918d-231">座標とサイズやホット スポットの半径は、宣言によって指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d918d-231">The coordinates and size or radius of the hotspot must be specified declaratively.</span></span> <span data-ttu-id="d918d-232">また、デザイナーでのホット スポットの視覚的表現はありません。</span><span class="sxs-lookup"><span data-stu-id="d918d-232">There is also no visual representation of a hotspot in the designer.</span></span> <span data-ttu-id="d918d-233">ホット スポットは、URL に移動するように構成、URL は、ホット スポットの NavigateUrl プロパティを使用して指定されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-233">If a hotspot is configured to navigate to a URL, the URL is specified via the NavigateUrl property of the hotspot.</span></span> <span data-ttu-id="d918d-234">投稿の場合は、プロパティを使用すると、サーバー側コードで取得できる、ポストバックで文字列を渡す PostBackValue のホット スポットをバックアップします。</span><span class="sxs-lookup"><span data-stu-id="d918d-234">In the case of a post back hotspot, the PostBackValue property allows you to pass a string in the post back that can be retrieved in server-side code.</span></span>


![Visual Studio で hotSpot コレクション エディター](server-controls/_static/image1.jpg)

<span data-ttu-id="d918d-236">**図 1**: Visual Studio で HotSpot コレクション エディター</span><span class="sxs-lookup"><span data-stu-id="d918d-236">**Figure 1**: HotSpot Collection Editor in Visual Studio</span></span>


## <a name="bulletedlist-control"></a><span data-ttu-id="d918d-237">BulletedList コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-237">BulletedList Control</span></span>

<span data-ttu-id="d918d-238">BulletedList コントロールは、バインドされたデータを簡単にできる箇条書きリストです。</span><span class="sxs-lookup"><span data-stu-id="d918d-238">The BulletedList control is a bulleted list that can easily be data bound.</span></span> <span data-ttu-id="d918d-239">一覧に (番号) 順序を指定できます BulletStyle プロパティを使用して順序付けられていないか。</span><span class="sxs-lookup"><span data-stu-id="d918d-239">The list can be ordered (numbered) or unordered via the BulletStyle property.</span></span> <span data-ttu-id="d918d-240">リスト内の各項目は ListItem オブジェクトによって表されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-240">Each item in the list is represented by a ListItem object.</span></span>


![Visual Studio で BulletedList コントロール](server-controls/_static/image1.gif)

<span data-ttu-id="d918d-242">**図 2**: Visual Studio で BulletedList コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-242">**Figure 2**: BulletedList Control in Visual Studio</span></span>


## <a name="hiddenfield-control"></a><span data-ttu-id="d918d-243">HiddenField コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-243">HiddenField Control</span></span>

<span data-ttu-id="d918d-244">HiddenField コントロールは、この値はサーバー側コードで使用できる、ページに隠しフォーム フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="d918d-244">The HiddenField control adds a hidden form field to your page, the value of which is available in server-side code.</span></span> <span data-ttu-id="d918d-245">隠しフォーム フィールドの値はポストバック間で変更されません一般的に必要です。</span><span class="sxs-lookup"><span data-stu-id="d918d-245">The value of a hidden form field is generally expected to remain unchanged between post backs.</span></span> <span data-ttu-id="d918d-246">ただし、悪意のあるユーザーがポスト バックする前の値を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="d918d-246">However, it is possible for a malicious user to change the value prior to post back.</span></span> <span data-ttu-id="d918d-247">この場合、HiddenField コントロールが、ValueChanged イベントを発生します。</span><span class="sxs-lookup"><span data-stu-id="d918d-247">If this happens, the HiddenField control will raise the ValueChanged event.</span></span> <span data-ttu-id="d918d-248">HiddenField コントロールに機密情報があることが変更されないことを保証する場合は、コードで、ValueChanged イベントを処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d918d-248">If you have sensitive information in the HiddenField control and you want to ensure that it remains unchanged, you should handle the ValueChanged event in your code.</span></span>

## <a name="fileupload-control"></a><span data-ttu-id="d918d-249">FileUpload コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-249">FileUpload Control</span></span>

<span data-ttu-id="d918d-250">ASP.NET 2.0 で FileUpload コントロールを使うと、ASP.NET ページ経由で Web サーバーにファイルをアップロードできます。</span><span class="sxs-lookup"><span data-stu-id="d918d-250">The FileUpload control in ASP.NET 2.0 makes it possible to upload files to a Web server via an ASP.NET page.</span></span> <span data-ttu-id="d918d-251">このコントロールは、いくつかの例外は、ASP.NET 1.x HtmlInputFile クラスに非常に似ています。</span><span class="sxs-lookup"><span data-stu-id="d918d-251">This control is quite similar to the ASP.NET 1.x HtmlInputFile class with a few exceptions.</span></span> <span data-ttu-id="d918d-252">Asp.net 1.x では、適切なファイルがあったかどうかを決定するために null PostedFile プロパティをチェック推奨されました。</span><span class="sxs-lookup"><span data-stu-id="d918d-252">In ASP.NET 1.x, it was recommended that the PostedFile property be checked for null in order to determine if you had a good file.</span></span> <span data-ttu-id="d918d-253">ASP.NET 2.0 で FileUpload コントロールでは、同じ目的で使用することができますを方が少し効率的に、新しい HasFile プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="d918d-253">The FileUpload control in ASP.NET 2.0 adds a new HasFile property that you can use for the same purpose and it's a bit more efficient.</span></span>

<span data-ttu-id="d918d-254">PostedFile プロパティは、HttpPostedFile オブジェクトへのアクセスを引き続き使用できますが、HttpPostedFile の機能の一部は本質的に FileUpload コントロールで使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="d918d-254">The PostedFile property is still available for access to an HttpPostedFile object, but some of the functionality of the HttpPostedFile is now available intrinsically with the FileUpload control.</span></span> <span data-ttu-id="d918d-255">たとえば、ASP.NET では、アップロードされたファイルを保存する 1.x では、HttpPostedFile オブジェクトの SaveAs メソッドを呼び出すことです。</span><span class="sxs-lookup"><span data-stu-id="d918d-255">For example, to save an uploaded file in ASP.NET 1.x, you call the SaveAs method on the HttpPostedFile object.</span></span> <span data-ttu-id="d918d-256">FileUpload コントロールを使用して ASP.NET 2.0 では、FileUpload コントロール自体に、SaveAs メソッドを呼び出すとします。</span><span class="sxs-lookup"><span data-stu-id="d918d-256">Using the FileUpload control in ASP.NET 2.0, you would call the SaveAs method on the FileUpload control itself.</span></span>

<span data-ttu-id="d918d-257">2.0 の動作 (およびおそらく最も重要な変更) で、もう 1 つの重要な変更は、ことは、保存する前にアップロードされたファイル全体をメモリに読み込む必要はなくなりました。</span><span class="sxs-lookup"><span data-stu-id="d918d-257">Another significant change in the 2.0 behavior (and likely the most significant change) is that it is no longer necessary to load an entire uploaded file into memory before saving it.</span></span> <span data-ttu-id="d918d-258">1.x では、書き込まれる前にメモリに完全にアップロードされた任意のファイルを保存するディスク。</span><span class="sxs-lookup"><span data-stu-id="d918d-258">In 1.x, any file that was uploaded is saved entirely into memory prior to being written to disk.</span></span> <span data-ttu-id="d918d-259">このアーキテクチャでは、大きなファイルのアップロードできないようにします。</span><span class="sxs-lookup"><span data-stu-id="d918d-259">This architecture prevents the upload of large files.</span></span>

<span data-ttu-id="d918d-260">ASP.NET 2.0 での httpRuntime 要素と属性を使用するキロバイト数を構成するのには書き込まれる前にメモリ内のバッファーで保持されてディスクにします。</span><span class="sxs-lookup"><span data-stu-id="d918d-260">In ASP.NET 2.0, the requestLengthDiskThreshold attribute of the httpRuntime element allows you to configure how many Kilobytes are held in a buffer in memory prior to being written to disk.</span></span>

<span data-ttu-id="d918d-261">**重要な**: この値がバイト (キロバイト単位ではない) である、既定値は 256 を MSDN ドキュメント (および別の場所のドキュメント) を指定します。</span><span class="sxs-lookup"><span data-stu-id="d918d-261">**IMPORTANT**: MSDN documentation (and documentation elsewhere) specifies that this value is in bytes (not Kilobytes) and that the default is 256.</span></span> <span data-ttu-id="d918d-262">キロバイト単位で指定された値が実際には、既定値は 80 です。</span><span class="sxs-lookup"><span data-stu-id="d918d-262">The value is actually specified in Kilobytes and the default value is 80.</span></span> <span data-ttu-id="d918d-263">80 K の既定値を持つことによって、バッファーが終わらない大きなオブジェクト ヒープのことです。</span><span class="sxs-lookup"><span data-stu-id="d918d-263">By having a default value of 80K, we ensure that the buffer does not end up on the large object heap.</span></span>

## <a name="wizard-control"></a><span data-ttu-id="d918d-264">ウィザード コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-264">Wizard Control</span></span>

<span data-ttu-id="d918d-265">ASP.NET 開発者向けの「ページ」パネルを使用するのか、ページ間を転送することによって、一連の情報を収集しようとしてに悩まされることが発生する非常に一般的です。</span><span class="sxs-lookup"><span data-stu-id="d918d-265">It is fairly common to encounter ASP.NET developers struggling with attempting to gather information in a series of "pages" using panels or by transferring from page to page.</span></span> <span data-ttu-id="d918d-266">たいてい、作業は面倒であるし、は時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="d918d-266">More often than not, the endeavor is a frustrating one and is time consuming.</span></span> <span data-ttu-id="d918d-267">新しいウィザード コントロールは、線形と非線形の手順にユーザーが慣れているウィザード インターフェイスに許可することにより、問題を解決します。</span><span class="sxs-lookup"><span data-stu-id="d918d-267">The new Wizard control solves the problems by allowing for linear and non-linear steps in a wizard interface that users are familiar with.</span></span> <span data-ttu-id="d918d-268">ウィザード コントロールは、一連の手順で入力フォームを表示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-268">The Wizard control presents input forms in a series of steps.</span></span> <span data-ttu-id="d918d-269">各ステップでは、コントロールのいずれのプロパティで指定された特定の型です。</span><span class="sxs-lookup"><span data-stu-id="d918d-269">Each step is of a particular type specified by the StepType property of the control.</span></span> <span data-ttu-id="d918d-270">使用可能なステップの種類は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d918d-270">The available step types are as follows:</span></span>

| <span data-ttu-id="d918d-271">**ステップの種類**</span><span class="sxs-lookup"><span data-stu-id="d918d-271">**Step Type**</span></span> | <span data-ttu-id="d918d-272">**説明**</span><span class="sxs-lookup"><span data-stu-id="d918d-272">**Explanation**</span></span> |
| --- | --- |
| <span data-ttu-id="d918d-273">自動</span><span class="sxs-lookup"><span data-stu-id="d918d-273">Auto</span></span> | <span data-ttu-id="d918d-274">ウィザードでは、自動的にステップ ステップ階層内の位置に基づいての種類を決定します。</span><span class="sxs-lookup"><span data-stu-id="d918d-274">The wizard automatically determines the type of step based upon its position within the step hierarchy.</span></span> |
| <span data-ttu-id="d918d-275">[開始]</span><span class="sxs-lookup"><span data-stu-id="d918d-275">Start</span></span> | <span data-ttu-id="d918d-276">最初に、多くの場合、最初のステートメントを提示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="d918d-276">The first step, often used to present an introductory statement.</span></span> |
| <span data-ttu-id="d918d-277">手順</span><span class="sxs-lookup"><span data-stu-id="d918d-277">Step</span></span> | <span data-ttu-id="d918d-278">通常の手順です。</span><span class="sxs-lookup"><span data-stu-id="d918d-278">A normal step.</span></span> |
| <span data-ttu-id="d918d-279">完了</span><span class="sxs-lookup"><span data-stu-id="d918d-279">Finish</span></span> | <span data-ttu-id="d918d-280">最後の手順は、通常、ウィザードを完了するためのボタンを表示するために使用します。</span><span class="sxs-lookup"><span data-stu-id="d918d-280">The final step, usually used to present a button to finish the wizard.</span></span> |
| <span data-ttu-id="d918d-281">完了</span><span class="sxs-lookup"><span data-stu-id="d918d-281">Complete</span></span> | <span data-ttu-id="d918d-282">成功または失敗を通信してメッセージを表示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-282">Presents a message communicating success or failure.</span></span> |

> [!NOTE]
> <span data-ttu-id="d918d-283">ウィザード コントロールは、ASP.NET コントロールの状態を使用して、状態の追跡を保持します。</span><span class="sxs-lookup"><span data-stu-id="d918d-283">The Wizard control keeps track of its state using ASP.NET control state.</span></span> <span data-ttu-id="d918d-284">そのため、EnableViewState プロパティは、任意に損害を与えることがなく、false に設定できます。</span><span class="sxs-lookup"><span data-stu-id="d918d-284">Therefore, the EnableViewState property can be set to false without any detriment.</span></span>


<span data-ttu-id="d918d-285">このビデオでは、ウィザード コントロールのチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="d918d-285">This video is a walkthrough of the Wizard control.</span></span>


![](server-controls/_static/image2.png)


[<span data-ttu-id="d918d-286">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="d918d-286">Open Full-Screen Video</span></span>](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a><span data-ttu-id="d918d-287">コントロールをローカライズします。</span><span class="sxs-lookup"><span data-stu-id="d918d-287">Localize Control</span></span>

<span data-ttu-id="d918d-288">Localize コントロールは、リテラル コントロールに似ています。</span><span class="sxs-lookup"><span data-stu-id="d918d-288">The Localize control is similar to a Literal control.</span></span> <span data-ttu-id="d918d-289">ただし、Localize コントロールには、**モード**に追加されるマークアップのレンダリング方法を制御するプロパティ。</span><span class="sxs-lookup"><span data-stu-id="d918d-289">However, the Localize control has a **Mode** property that controls how markup that is added to it is rendered.</span></span> <span data-ttu-id="d918d-290">Mode プロパティには、次の値がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="d918d-290">The Mode property supports the following values:</span></span>

| <span data-ttu-id="d918d-291">**モード**</span><span class="sxs-lookup"><span data-stu-id="d918d-291">**Mode**</span></span> | <span data-ttu-id="d918d-292">**説明**</span><span class="sxs-lookup"><span data-stu-id="d918d-292">**Explanation**</span></span> |
| --- | --- |
| <span data-ttu-id="d918d-293">変換</span><span class="sxs-lookup"><span data-stu-id="d918d-293">Transform</span></span> | <span data-ttu-id="d918d-294">マークアップは、ブラウザーの要求のプロトコルに従って変換されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-294">Markup is transformed according to the protocol of the browser making the request.</span></span> |
| <span data-ttu-id="d918d-295">PassThrough</span><span class="sxs-lookup"><span data-stu-id="d918d-295">PassThrough</span></span> | <span data-ttu-id="d918d-296">マークアップとしてレンダリングされます-です。</span><span class="sxs-lookup"><span data-stu-id="d918d-296">Markup is rendered as-is.</span></span> |
| <span data-ttu-id="d918d-297">エンコード</span><span class="sxs-lookup"><span data-stu-id="d918d-297">Encode</span></span> | <span data-ttu-id="d918d-298">コントロールに追加されるマークアップは、HtmlEncode を使用してエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="d918d-298">Markup that is added to the control is encoded using HtmlEncode.</span></span> |

## <a name="multiview-and-view-controls"></a><span data-ttu-id="d918d-299">MultiView コントロールとビュー コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-299">MultiView and View Controls</span></span>

<span data-ttu-id="d918d-300">マルチビュー コントロールのビューのコントロールのコンテナーとして機能し、ビュー コントロールは、(同様パネル コントロール) の他のコントロールのコンテナーとして機能します。</span><span class="sxs-lookup"><span data-stu-id="d918d-300">The MultiView control acts as a container for View controls, and the View control acts as a container (much like a Panel control) for other controls.</span></span> <span data-ttu-id="d918d-301">マルチビュー コントロール内の各ビューは、1 つのビュー コントロールで表されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-301">Each view in a MultiView control is represented by a single View control.</span></span> <span data-ttu-id="d918d-302">最初のビュー、MultiView コントロールがビュー 0、2 番目は 1、ビューなど。マルチビュー コントロールの ActiveViewIndex を指定することで、ビューを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="d918d-302">The first View control in the MultiView is view 0, the second is view 1, etc. You can switch views by specifying the ActiveViewIndex of the MultiView control.</span></span>

## <a name="substitution-control"></a><span data-ttu-id="d918d-303">代替コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-303">Substitution Control</span></span>

<span data-ttu-id="d918d-304">切り替えコントロールは、ASP.NET のキャッシュと組み合わせて使用されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-304">The Substitution control is used in conjunction with ASP.NET caching.</span></span> <span data-ttu-id="d918d-305">キャッシュを活用するために必要なが各要求 (つまり、ページの一部のキャッシュから除外される) で更新する必要があります ページの部分がある場合では、代入コンポーネントは、優れたソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="d918d-305">In cases where you want to take advantage of caching, but you have portions of a page that must be updated on each request (in other words, portions of a page that are exempt from caching), the Substitution component provides a great solution.</span></span> <span data-ttu-id="d918d-306">コントロールでは、独自の出力を実際にレンダリングしません。</span><span class="sxs-lookup"><span data-stu-id="d918d-306">The control doesn't actually render any output on its own.</span></span> <span data-ttu-id="d918d-307">代わりに、サーバー側コードのメソッドにバインドされます。</span><span class="sxs-lookup"><span data-stu-id="d918d-307">Instead, it is bound to a method in server-side code.</span></span> <span data-ttu-id="d918d-308">ページが要求されたときに、メソッドが呼び出され、返されたマークアップは、代替コントロールの代わりに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-308">When the page is requested, the method is called and the returned markup is rendered in place of the substitution control.</span></span>

<span data-ttu-id="d918d-309">代替コントロールがバインドされているメソッドを指定した、 **MethodName**プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d918d-309">The method to which the Substitution control is bound is specified via the **MethodName** property.</span></span> <span data-ttu-id="d918d-310">メソッドは、次の条件を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="d918d-310">That method must meet the following criteria:</span></span>

- <span data-ttu-id="d918d-311">静的 (VB では shared) メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="d918d-311">It must be a static (shared in VB) method.</span></span>
- <span data-ttu-id="d918d-312">HttpContext の種類の 1 つのパラメーターを受け取っています。</span><span class="sxs-lookup"><span data-stu-id="d918d-312">It accepts one parameter of type HttpContext.</span></span>
- <span data-ttu-id="d918d-313">ページ上のコントロールを置き換える必要がありますマークアップを表す文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="d918d-313">It returns a string representing the markup that should replace the control on the page.</span></span>

<span data-ttu-id="d918d-314">代替コントロールには、ページで、他のコントロールを変更することはありませんが、そのパラメーターを使用して、現在の HttpContext へのアクセスが。</span><span class="sxs-lookup"><span data-stu-id="d918d-314">The Substitution control does not have the ability to modify any other control on the page, but it does have access to the current HttpContext via its parameter.</span></span>

## <a name="gridview-control"></a><span data-ttu-id="d918d-315">GridView コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-315">GridView Control</span></span>

<span data-ttu-id="d918d-316">GridView コントロールは、DataGrid コントロールを置換します。</span><span class="sxs-lookup"><span data-stu-id="d918d-316">The GridView control is the replacement for the DataGrid control.</span></span> <span data-ttu-id="d918d-317">このコントロールは、後のモジュールで詳しく説明されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-317">This control will be covered in more detail in a later module.</span></span>

## <a name="detailsview-control"></a><span data-ttu-id="d918d-318">DetailsView コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-318">DetailsView Control</span></span>

<span data-ttu-id="d918d-319">DetailsView コントロールでは、データ ソースから 1 つのレコードを表示および編集または削除することができます。</span><span class="sxs-lookup"><span data-stu-id="d918d-319">The DetailsView control allows you to display a single record from a data source and to edit or delete it.</span></span> <span data-ttu-id="d918d-320">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-320">It is covered in more detail in a later module.</span></span>

## <a name="formview-control"></a><span data-ttu-id="d918d-321">FormView コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-321">FormView Control</span></span>

<span data-ttu-id="d918d-322">FormView コントロールは、構成可能なインターフェイスでのデータ ソースから 1 つのレコードの表示に使用されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-322">The FormView control is used to display a single record from a datasource in a configurable interface.</span></span> <span data-ttu-id="d918d-323">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-323">It is covered in more detail in a later module.</span></span>

## <a name="accessdatasource-control"></a><span data-ttu-id="d918d-324">AccessDataSource コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-324">AccessDataSource Control</span></span>

<span data-ttu-id="d918d-325">AccessDataSource コントロールを使用する Access データベースにデータをバインドします。</span><span class="sxs-lookup"><span data-stu-id="d918d-325">The AccessDataSource control is used to data bind an Access database.</span></span> <span data-ttu-id="d918d-326">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-326">It is covered in more detail in a later module.</span></span>

## <a name="objectdatasource-control"></a><span data-ttu-id="d918d-327">ObjectDataSource コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-327">ObjectDataSource Control</span></span>

<span data-ttu-id="d918d-328">ObjectDataSource コントロールは、コントロールのデータ バインドできます 2 層モデルではなく中間層ビジネス オブジェクトにコントロールがデータ ソースへの直接バインドされてように 3 層アーキテクチャをサポートするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-328">The ObjectDataSource control is used to support a three-tier architecture so that controls can be data-bound to a middle-tier business object as opposed to a two-tiered model where controls are bound directly to the data source.</span></span> <span data-ttu-id="d918d-329">については、後のモジュールで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="d918d-329">It will be discussed in more detail in a later module.</span></span>

## <a name="xmldatasource-control"></a><span data-ttu-id="d918d-330">XmlDataSource コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-330">XmlDataSource Control</span></span>

<span data-ttu-id="d918d-331">XmlDataSource コントロールは、XML データ ソースへのデータ バインドに使用されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-331">The XmlDataSource control is used to data bind to an XML data source.</span></span> <span data-ttu-id="d918d-332">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-332">It is covered in more detail in a later module.</span></span>

## <a name="sitemapdatasource-control"></a><span data-ttu-id="d918d-333">SiteMapDataSource コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-333">SiteMapDataSource Control</span></span>

<span data-ttu-id="d918d-334">SiteMapDataSource コントロールは、サイト マップに基づいてサイト ナビゲーション コントロールのデータ バインディングを提供します。</span><span class="sxs-lookup"><span data-stu-id="d918d-334">The SiteMapDataSource control provides data binding for site navigation controls based on a site map.</span></span> <span data-ttu-id="d918d-335">については、後のモジュールで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="d918d-335">It will be discussed in more detail in a later module.</span></span>

## <a name="sitemappath-control"></a><span data-ttu-id="d918d-336">SiteMapPath コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-336">SiteMapPath Control</span></span>

<span data-ttu-id="d918d-337">SiteMapPath コントロールでは、一連の階層リンクとも呼ばナビゲーション リンクが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-337">The SiteMapPath control displays a series of navigation links commonly referred to as breadcrumbs.</span></span> <span data-ttu-id="d918d-338">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-338">It is covered in more detail in a later module.</span></span>

## <a name="menu-control"></a><span data-ttu-id="d918d-339">メニュー コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-339">Menu Control</span></span>

<span data-ttu-id="d918d-340">メニュー コントロールは、DHTML を使用して動的メニューを表示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-340">The Menu control displays dynamic menus using DHTML.</span></span> <span data-ttu-id="d918d-341">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-341">It is covered in more detail in a later module.</span></span>

## <a name="treeview-control"></a><span data-ttu-id="d918d-342">TreeView コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-342">TreeView Control</span></span>

<span data-ttu-id="d918d-343">[TreeView] コントロールは、データの階層ツリー ビューの表示に使用されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-343">The TreeView control is used to display a hierarchical tree view of data.</span></span> <span data-ttu-id="d918d-344">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-344">It is covered in more detail in a later module.</span></span>

## <a name="login-control"></a><span data-ttu-id="d918d-345">Login コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-345">Login Control</span></span>

<span data-ttu-id="d918d-346">Login コントロールは、Web サイトにログインするためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="d918d-346">The Login control provides for a mechanism to log into a Web site.</span></span> <span data-ttu-id="d918d-347">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-347">It is covered in more detail in a later module.</span></span>

## <a name="loginview-control"></a><span data-ttu-id="d918d-348">LoginView コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-348">LoginView Control</span></span>

<span data-ttu-id="d918d-349">LoginView コントロールでは、ユーザーのログイン ステータスに基づいてさまざまなテンプレートの表示ができます。</span><span class="sxs-lookup"><span data-stu-id="d918d-349">The LoginView control allows for the display of different templates based upon a user's login status.</span></span> <span data-ttu-id="d918d-350">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-350">It is covered in more detail in a later module.</span></span>

## <a name="passwordrecovery-control"></a><span data-ttu-id="d918d-351">PasswordRecovery コントロール</span><span class="sxs-lookup"><span data-stu-id="d918d-351">PasswordRecovery Control</span></span>

<span data-ttu-id="d918d-352">PasswordRecovery コントロールは、ASP.NET アプリケーションのユーザーがパスワードを忘れた場合の取得に使用されます。</span><span class="sxs-lookup"><span data-stu-id="d918d-352">The PasswordRecovery control is used to retrieve forgotten passwords by users of an ASP.NET application.</span></span> <span data-ttu-id="d918d-353">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-353">It is covered in more detail in a later module.</span></span>

## <a name="loginstatus"></a><span data-ttu-id="d918d-354">LoginStatus</span><span class="sxs-lookup"><span data-stu-id="d918d-354">LoginStatus</span></span>

<span data-ttu-id="d918d-355">LoginStatus コントロールは、ユーザーのログイン状態を表示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-355">The LoginStatus control displays a user's login status.</span></span> <span data-ttu-id="d918d-356">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-356">It is covered in more detail in a later module.</span></span>

## <a name="loginname"></a><span data-ttu-id="d918d-357">LoginName</span><span class="sxs-lookup"><span data-stu-id="d918d-357">LoginName</span></span>

<span data-ttu-id="d918d-358">LoginName コントロールは、ASP.NET アプリケーションにログインしている後に、ユーザーのユーザー名を表示します。</span><span class="sxs-lookup"><span data-stu-id="d918d-358">The LoginName control displays a user's username after being logged into an ASP.NET application.</span></span> <span data-ttu-id="d918d-359">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-359">It is covered in more detail in a later module.</span></span>

## <a name="createuserwizard"></a><span data-ttu-id="d918d-360">CreateUserWizard</span><span class="sxs-lookup"><span data-stu-id="d918d-360">CreateUserWizard</span></span>

<span data-ttu-id="d918d-361">CreateUserWizard は、構成可能なウィザードによってユーザーは、ASP.NET アプリケーションで使用するための ASP.NET メンバーシップ アカウントを作成する機能です。</span><span class="sxs-lookup"><span data-stu-id="d918d-361">The CreateUserWizard is a configurable wizard which gives users the ability to create an ASP.NET Membership account for use in an ASP.NET application.</span></span> <span data-ttu-id="d918d-362">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-362">It is covered in more detail in a later module.</span></span>

## <a name="changepassword"></a><span data-ttu-id="d918d-363">パスワードの変更</span><span class="sxs-lookup"><span data-stu-id="d918d-363">ChangePassword</span></span>

<span data-ttu-id="d918d-364">ChangePassword コントロールでは、ASP.NET アプリケーションのパスワードを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="d918d-364">The ChangePassword control allows users to change their password for an ASP.NET application.</span></span> <span data-ttu-id="d918d-365">後のモジュールで詳しく説明しました。</span><span class="sxs-lookup"><span data-stu-id="d918d-365">It is covered in more detail in a later module.</span></span>

## <a name="various-webparts"></a><span data-ttu-id="d918d-366">さまざまな web パーツ</span><span class="sxs-lookup"><span data-stu-id="d918d-366">Various WebParts</span></span>

<span data-ttu-id="d918d-367">ASP.NET 2.0 は、さまざまな Web パーツに付属しています。</span><span class="sxs-lookup"><span data-stu-id="d918d-367">ASP.NET 2.0 ships with various Web Parts.</span></span> <span data-ttu-id="d918d-368">これらについては、後のモジュールで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="d918d-368">These will be covered in detail in a later module.</span></span>
