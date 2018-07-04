---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[How Do i:]Reponse.Filter プロパティを使用して、ASP.NET ページの HTML を置き換える |Microsoft Docs'
author: rick-anderson
description: このビデオの Chris Pels で Reponse.Filter プロパティを使用してをインターセプトし、ページに送信される HTML を変更する方法を示しています。 まず、w サンプル ページを作成しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: f871a3969da21a22bfe10e3a85f43db3e0e522e8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382362"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="c795b-104">[How Do i:]Reponse.Filter プロパティを使用して、HTML、ASP.NET ページを置換するには</span><span class="sxs-lookup"><span data-stu-id="c795b-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="c795b-105">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="c795b-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="c795b-106">このビデオの Chris Pels で Reponse.Filter プロパティを使用してをインターセプトし、ページに送信される HTML を変更する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="c795b-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="c795b-107">最初に、サンプル ページは、いくつかの単純なテキストで作成されます。</span><span class="sxs-lookup"><span data-stu-id="c795b-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="c795b-108">次に、ユーザーのブラウザーに送信されている現在のストリームの代替ストリームとして使用されるカスタム Stream クラスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="c795b-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="c795b-109">そのカスタム ストリーム クラスで、ページの内容を応答ストリームに書き込むし、変更は、ストリームから取得されます。</span><span class="sxs-lookup"><span data-stu-id="c795b-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="c795b-110">このカスタム Stream クラスの Write メソッドをカスタマイズして、それによってユーザーのブラウザーへの送信内容を変更する、基本の応答ストリームに HTML を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c795b-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="c795b-111">ページのために Response.Filter プロパティを新しいストリーム クラスが最後に、割り当てられている\_読み込みイベント、これによって、ページのコンテンツを変更するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="c795b-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="c795b-112">&#9654;(13 分) のビデオを見る</span><span class="sxs-lookup"><span data-stu-id="c795b-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
