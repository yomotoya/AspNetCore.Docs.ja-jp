---
uid: webhooks/source
title: ASP.NET の Webhook のソース コードと NuGet パッケージの管理 |Microsoft ドキュメント
author: rick-anderson
description: ASP.NET の Webhook のソース コードと NuGet パッケージへのリンク
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/09/2018
ms.locfileid: "27709971"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="c4e2e-103">ASP.NET の Webhook のソース コードと NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="c4e2e-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="c4e2e-104">Microsoft ASP.NET の Webhook はモジュールの Microsoft ASP.NET ファミリーの一部でありとしてホストされます、 [GitHub のオープン ソース プロジェクト](https://github.com/aspnet/WebHooks)です。</span><span class="sxs-lookup"><span data-stu-id="c4e2e-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="c4e2e-105">つまりは、投稿を受け入れていますを参照してください、[貢献ガイドライン](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)プル要求を送信する前にします。</span><span class="sxs-lookup"><span data-stu-id="c4e2e-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="c4e2e-106">このオンライン ドキュメントをお読みになっているようになりましたがもとしてホストされる[GitHub のオープン ソース](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)しも貢献を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="c4e2e-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="c4e2e-107">NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="c4e2e-107">NuGet packages</span></span>

<span data-ttu-id="c4e2e-108">[NuGet パッケージの](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)は 3 つの部分に分けられます。</span><span class="sxs-lookup"><span data-stu-id="c4e2e-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="c4e2e-109">[一般的な](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): 送信者と受信者の間で共有される共通のパッケージです。</span><span class="sxs-lookup"><span data-stu-id="c4e2e-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="c4e2e-110">[送信者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): 一連のパッケージの他のユーザーに、独自の Webhook の送信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c4e2e-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="c4e2e-111">Webhook を送信するための機能がで詳しく説明されている[を送信する Webhook](sending/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="c4e2e-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="c4e2e-112">[レシーバー](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): 一連のパッケージの他のユーザーからの Webhook の受信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c4e2e-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="c4e2e-113">Webhook を受信するための機能がで詳しく説明されている[受信 Webhook](receiving/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="c4e2e-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
