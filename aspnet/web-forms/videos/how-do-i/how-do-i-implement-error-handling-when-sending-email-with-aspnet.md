---
uid: web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
title: '[How Do i:]ASP.NET で電子メールを送信するときにエラー処理の実装 |Microsoft Docs'
author: rick-anderson
description: Chris Pels では、ASP.NET で電子メールを送信するときの処理エラーを実装する方法を示します。 彼は、電子メールを送信する ASP.NET web ページを作成しを構成する方法 & lt が表示されます.
ms.author: aspnetcontent
ms.date: 11/06/2008
ms.assetid: c02ffd50-aa19-4cdc-b1bf-760989979a61
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-implement-error-handling-when-sending-email-with-aspnet
msc.type: video
ms.openlocfilehash: ebe6f451cad2ba4a9311b5882337b637de7f381e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814728"
---
<a name="how-do-i-implement-error-handling-when-sending-email-with-aspnet"></a><span data-ttu-id="18920-104">[How Do i:]ASP.NET で電子メールを送信するときにエラー処理を実装します。</span><span class="sxs-lookup"><span data-stu-id="18920-104">[How Do I:] Implement Error Handling when Sending Email with ASP.NET</span></span>
====================
<span data-ttu-id="18920-105">によって[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="18920-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="18920-106">Chris Pels では、ASP.NET で電子メールを送信するときの処理エラーを実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="18920-106">Chris Pels shows how to implement error handling when sending an email with ASP.NET.</span></span> <span data-ttu-id="18920-107">電子メールを送信する ASP.NET web ページを作成しを構成する方法を示しています。 彼&lt;mailSettings&gt; web.config ファイルで、System.Net.Mail クラスとその使用を作成し、電子メール メッセージを送信する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="18920-107">He creates an ASP.NET web page to send email, shows how to configure &lt;mailSettings&gt; in the web.config file, describes the System.Net.Mail class and how it's used to create and send email messages.</span></span> <span data-ttu-id="18920-108">エラー処理の電子メールを送信するときに発生したエラーに関する情報を提供し、電子メールを送信するときに、考えられる結果の一覧を提供する SmtpStatusCode 列挙をレビュー System.Net.Mail 例外クラスを使用して追加しますし、SmtpClient します。</span><span class="sxs-lookup"><span data-stu-id="18920-108">He then adds error handling using System.Net.Mail exception classes, which provide information about errors that can occur when sending email, and reviews the SmtpStatusCode enumeration, which provides a list of possible outcomes when sending an email with the SmtpClient.</span></span> <span data-ttu-id="18920-109">最後に、彼は例外が発生し、Visual Studio デバッガーでの情報の処理エラーを確認するテスト電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="18920-109">Finally, he sends a test email that raises an exception and reviews the error handling information in the Visual Studio debugger.</span></span>

[<span data-ttu-id="18920-110">&#9654;ビデオ (24 分)</span><span class="sxs-lookup"><span data-stu-id="18920-110">&#9654; Watch video (24 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-implement-error-handling-when-sending-email-with-aspnet)
