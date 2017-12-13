---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: "[How Do i:]ASP.NET ページの HTML を置換する Reponse.Filter プロパティを使用して |Microsoft ドキュメント"
author: rick-anderson
description: "このビデオ Chris Pels で Reponse.Filter プロパティを使用してをインターセプトし、ページに送信されている HTML を変更する方法を示しています。 まず、w サンプル ページを作成しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="3e313-104">[How Do i:]ASP.NET ページの HTML を置換する Reponse.Filter プロパティを使用します</span><span class="sxs-lookup"><span data-stu-id="3e313-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="3e313-105">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="3e313-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="3e313-106">このビデオ Chris Pels で Reponse.Filter プロパティを使用してをインターセプトし、ページに送信されている HTML を変更する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="3e313-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="3e313-107">最初に、いくつかの単純なテキストのサンプルのページが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3e313-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="3e313-108">次に、カスタム ストリーム クラスでは、ユーザーのブラウザーに送信される現在のストリームの置換ストリームとして機能するが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3e313-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="3e313-109">カスタム ストリーム クラスでは、ページの内容はし、応答ストリームに書き込むし、変更は、ストリームから取得されます。</span><span class="sxs-lookup"><span data-stu-id="3e313-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="3e313-110">このカスタム ストリーム クラスで書き込みメソッドをカスタマイズして、これにより、ユーザーのブラウザーに送信される新機能を変更する、基本の応答ストリームに HTML を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3e313-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="3e313-111">新しいストリーム クラスが、ページのために Response.Filter プロパティに割り当てられた最後に、\_読み込みイベント、これにより、ページのコンテンツを変更するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="3e313-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="3e313-112">&#9654;です。(13 分) のビデオを見る</span><span class="sxs-lookup"><span data-stu-id="3e313-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
