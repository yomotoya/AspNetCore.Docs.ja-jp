---
uid: overview
title: ASP.NET の概要 |Microsoft Docs
author: rick-anderson
description: ASP.NET、web サイト、web アプリケーション、および web API を作成するための無償のフレームワークを紹介します。
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 03/12/2010
ms.technology: aspnet
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: 2dc48e1262b1807a77a9889f7e0e62c9b9ea463e
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794812"
---
# <a name="aspnet-overview"></a>ASP.NET の概要

ASP.NET は、すばらしい web サイトと HTML、CSS、および JavaScript を使用して web アプリケーションを構築するための無料の web フレームワークです。 Web Api を作成し、Web ソケットなどのリアルタイム テクノロジを使用できます。

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/)は ASP.NET の代替です。  参照してください、 [ASP.NET と ASP.NET Core のどちらを選択する方法に関するガイダンス](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework)します。

## <a name="get-started"></a>作業開始

インストール[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community edition、Windows 上の ASP.NET の無料の IDE です。

## <a name="websites-and-web-applications"></a>Web サイトと web アプリケーション

 ASP.NET web アプリケーションを作成するための 3 つのフレームワークを提供しています。 Web フォーム、ASP.NET MVC、および ASP.NET Web ページ。 3 つすべてのフレームワークが安定し、完成度の高いと、これらのいずれかで優れた web アプリケーションを作成することができます。 選択したどのようなフレームワークに関係なくすべての利点と ASP.NET の機能のすべての場所が表示されます。

各フレームワークには、さまざまな開発スタイルが対象とします。 選択した 1 つは、プログラミングの資産 (知識、スキル、および開発環境) の組み合わせ、アプリケーションを作成してに慣れている開発アプローチの種類によって異なります。

それらの間を選択する方法のいくつかのアイデアと各フレームワークの概要を次に示します。 動画で紹介する場合を参照してください[ASP.NET を使用した web サイトの作成](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET)と[Web ツールとは何ですか?。](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | 経験がある場合 | 開発スタイル | 専門知識 |
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web フォーム | Win フォーム、WPF、.NET | HTML マークアップをカプセル化するコントロールの豊富なライブラリを使用して迅速な開発 | 中程度、高度な RAD |
| MVC       | Ruby on Rails、.NET  | 完全に制御 HTML マークアップ、コードとマークアップ、分離された、簡単にテストを記述できます。 モバイルおよび単一ページ アプリケーション (SPA) に最適です。 | 中程度、高度です |
| Web ページ  | クラシック ASP、PHP     | HTML マークアップと同じファイルにまとめてコード | 新しい、中程度 |

### <a name="web-forms"></a>Web フォーム

ASP.NET Web フォームでは、使い慣れたドラッグ アンド ドロップ、イベント ドリブン モデルを使用して動的な web サイトを構築できます。 デザイン サーフェスや数百のコントロールとコンポーネントを洗練された強力な UI 駆動型サイトとデータ アクセスを迅速に構築できます。

[Web フォームを詳細します。](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC には、懸念事項の明確に分離できるようにしてを提供するマークアップを完全に制御楽しいもののアジャイル開発のための動的な web サイトを構築する強力なパターンに基づく方法。 ASP.NET MVC には、最新の web 標準を使用して、高度なアプリケーションを作成するための高速、tdd 向けの開発が可能な多くの機能が含まれています。

[MVC を詳細します。](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web ページ

ASP.NET Web ページと Razor 構文は、サーバー コードを組み合わせて動的 web コンテンツを作成する HTML の高速で、わかりやすく、軽量な方法を提供します。 データベースに接続し、動画の追加、ソーシャル ネットワーク サイトへのリンク多く際に役立つ多くの機能は、最新の web 標準に準拠する美しいサイトを作成します。

[Web ページを詳細します。](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Web フォーム、MVC、および Web ページに関する注意事項

次の 3 つすべての ASP.NET フレームワークは、.NET Framework に基づいており、ASP.NET および .NET のコア機能を共有します。 たとえば、3 つすべてのフレームワークが、メンバーシップに基づくログインのセキュリティ モデルを提供し、3 つすべての共有機能の ASP.NET core の一部である要求や処理のセッションを管理するための同じ機能。

さらに、3 つのフレームワークが完全に独立していないし、いずれかを選択しても別の使用はでは。 フレームワークは、同じ web アプリケーションで共存させることができます、ためにはさまざまなフレームワークを使用して記述されたアプリケーションの個々 のコンポーネントを表示する珍しくありません。 たとえば、データ アクセスと管理の部分はデータ コントロールと単純なデータ アクセスを活用するために Web フォームで開発中に、マークアップを最適化するために mvc アプリの顧客向けの部分を開発する可能性があります。

## <a name="web-apis"></a>Web API

ASP.NET Web API をさまざまなブラウザーやモバイル デバイスなどのクライアントに提供される HTTP サービスを構築するが容易にするフレームワークです。 ASP.NET Web API は、.NET Framework に基づいて RESTful アプリケーションを構築するのに最適なプラットフォームです。

[Web API の詳細について説明します](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>リアルタイム テクノロジ

ASP.NET SignalR は、ASP.NET 開発者向けの新しいライブラリをリアルタイム web 機能の開発が容易です。 SignalR では、サーバーとクライアント間の双方向通信を許可します。 サーバーに接続されているクライアントにすぐに利用可能になったコンテンツをプッシュできます。 SignalR では、Web ソケットの場合をサポートし、古いブラウザーの互換性のあるその他の手法にフォールバックします。 SignalR には接続管理用の Api が含まれています (接続し、切断イベントなど)、接続、および承認をグループ化します。

[詳細については、SignalR は](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>モバイル アプリとサイト

ASP.NET に電源、レスポンシブ デザイン Twitter Bootstrap フレームワークを使用して、モバイル web サイトと同様に Web API バック エンドでは、ネイティブ モバイル アプリを使用できます。 ネイティブ モバイル アプリを構築する場合は、ハンドルのデータ アクセス、認証、およびアプリのプッシュ通知を JSON ベースの Web API の作成が容易になります。 応答性の高いモバイル サイトを構築する場合は、任意の CSS フレームワークまたはオープンのグリッド システムするか、jQuery Mobile または Sencha や PhoneGap の優れたモバイル アプリケーションなどの強力なモバイル システムを選択を使用することができます。

[モバイル アプリとサイトの開発に関する詳細します。](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>シングル ページ アプリケーション

ASP.NET Single Page Application (SPA) を使用して、HTML 5、CSS 3、JavaScript を使用して重要なクライアント側の対話を含むアプリケーションを構築できます。 Visual Studio には、knockout.js と ASP.NET Web API を使用してシングル ページ アプリケーションを構築するためのテンプレートが含まれています。 組み込みの SPA テンプレートだけでなくコミュニティによって作成された SPA テンプレートもダウンロードできます。

[シングル ページ アプリの開発に関する詳細します。](single-page-application/index.md)

## <a name="webhooks"></a>Web フック

Webhook は、Web Api と SaaS サービスをまとめて配線の単純なパブリッシュ/サブスクライブ モデルを提供する軽量な HTTP パターンです。 サービスで、イベントが発生したときに通知が登録されているサブスクライバーに HTTP POST 要求の形式で送信されます。 POST 要求には、それに従って動作する受信者を表すことができます、イベントに関する情報が含まれています。

Webhook は、Dropbox、GitHub、Instagram、MailChimp、PayPal、Slack、Trello、およびその他を含むサービスの数が多いによって公開されます。 たとえば、WebHook では、こと、ファイルが Dropbox では、変更または GitHub でコード変更がコミットされた、PayPal の支払いが開始されたまたは Trello カードが作成されてを指定できます。

[Webhook を詳細します。](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
