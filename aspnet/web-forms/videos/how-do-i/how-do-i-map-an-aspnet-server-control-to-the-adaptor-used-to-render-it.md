---
uid: web-forms/videos/how-do-i/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it
title: '[How Do i:]レンダリングに使用されるアダプターに、ASP.NET サーバー コントロールをマップ |Microsoft Docs'
author: rick-anderson
description: このビデオの Chris Pels で、コントロール アダプターを使用して、実際には、c を変更することがなく、ASP.NET サーバー コントロールのさまざまなレンダリングを提供する方法を紹介しています.
ms.author: riande
ms.date: 06/19/2008
ms.assetid: d4b498ef-8e1c-4fa2-9c35-1f32f20bb9b7
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it
msc.type: video
ms.openlocfilehash: 5e9a8194fb6a51362e1426654a0afb4c7ffd5dd3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827148"
---
<a name="how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it"></a><span data-ttu-id="99bd6-103">[How Do i:]マップのレンダリングに使用されるアダプターに、ASP.NET サーバー コントロール</span><span class="sxs-lookup"><span data-stu-id="99bd6-103">[How Do I:] Map an ASP.NET Server Control to the Adaptor Used to Render It</span></span>
====================
<span data-ttu-id="99bd6-104">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="99bd6-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="99bd6-105">このビデオの Chris Pels で、コントロール アダプターを使用して、実際には、コントロール自体を変更することがなく、ASP.NET サーバー コントロールのさまざまなレンダリングを提供する方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="99bd6-105">In this video Chris Pels will show how to use a control adaptor to provide different renderings for an ASP.NET server control without actually changing the control itself.</span></span> <span data-ttu-id="99bd6-106">このビデオで、ASP.NET BulletList コントロールを水平方向に従来の UL 要素ではなく DIV 要素を使用して各リスト項目を表示するように適合になります。</span><span class="sxs-lookup"><span data-stu-id="99bd6-106">In this video, an ASP.NET BulletList control will be adapted to display each list item horizontally using DIV elements instead of the traditional UL elements.</span></span> <span data-ttu-id="99bd6-107">まず、WebControlAdaptor を継承し、新しい一覧の形式を表示するためにコードを実装するクラスを作成する方法を参照してください。</span><span class="sxs-lookup"><span data-stu-id="99bd6-107">First, see how to create a class that inherits WebControlAdaptor and then implements the code to render the new list format.</span></span> <span data-ttu-id="99bd6-108">次に、新しいコントロール アダプターを .browser 定義ファイル内の ASP.NET サーバー コントロールにマップする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="99bd6-108">Next, learn how to map the new control adaptor to the ASP.NET server control in the .browser definition file.</span></span> <span data-ttu-id="99bd6-109">Web サイトのページで、新しいコントロール アダプターを使用する方法を参照してください。</span><span class="sxs-lookup"><span data-stu-id="99bd6-109">Then see how to use the new control adaptor on pages in a web site.</span></span> <span data-ttu-id="99bd6-110">最後に、コントロール アダプターがブラウザーまたはブラウザーの特定の種類に関連付けする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="99bd6-110">Finally, learn how a control adaptor can be associated with either all browsers or specific types of browsers.</span></span>

[<span data-ttu-id="99bd6-111">&#9654;ビデオ (23 分)</span><span class="sxs-lookup"><span data-stu-id="99bd6-111">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it)
