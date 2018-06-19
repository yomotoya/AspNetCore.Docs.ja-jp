---
uid: overview
title: ASP.NET の概要 |Microsoft ドキュメント
author: rick-anderson
description: ASP.NET、web サイト、web アプリケーション、および web Api を作成するための無料のフレームワークの概要です。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: 0ba7814d4004b17e678eab9a2a41a6d6f34773e1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
ms.locfileid: "29793741"
---
# <a name="aspnet-overview"></a>ASP.NET の概要

ASP.NET は、優れた web サイトや HTML、CSS、および JavaScript を使用して web アプリケーションを構築するための無料の web フレームワークです。 Web Api を作成し、Web ソケットのようなリアルタイム テクノロジを使用することができますも。

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/)は ASP.NET する代わりにします。  参照してください、 [ASP.NET と ASP.NET Core から選択する方法についてのガイダンス](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework)です。

## <a name="get-started"></a>作業開始

[Visual Studio コミュニティ 2017](https://www.visualstudio.com/downloads/)、Windows 上の ASP.NET の IDE を解放します。

## <a name="websites-and-web-applications"></a>Web サイトと web アプリケーション

 ASP.NET web アプリケーションを作成するための 3 つのフレームワークを提供しています: Web フォーム、ASP.NET MVC、ASP.NET Web ページ。 次の 3 つすべてのフレームワークは、安定性と完成度の高い、およびそれらのいずれかで優れた web アプリケーションを作成することができます。 どのようなフレームワークを選択するに関係なくすべての利点と ASP.NET の機能のすべての場所が表示されます。

各フレームワークには、別の開発スタイルが対象です。 選択する 1 つはプログラミング アセット (ナレッジ、スキル、および開発環境) の組み合わせ、作成している場合は、アプリケーションに慣れている開発方法の種類によって異なります。

以下のフレームワークは、それらの間を選択する方法のいくつかアイデアの概要を示します。 紹介ビデオを希望する場合は、次を参照してください。 [ASP.NET での web サイトを作成](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET)と[Web ツールは何ですか。](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | 経験がある場合 | 開発スタイル | 専門知識 | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web フォーム | Win フォーム、WPF、.NET | HTML マークアップをカプセル化するコントロールの機能豊富なライブラリを使用して、迅速な開発 | 中間レベル、高度な RAD |
| MVC       | .NET のレールに ruby  | 完全に制御 HTML マークアップ、コードとマークアップ、区切られたと簡単にテストを記述します。 モバイル アプリケーションや単一ページ アプリケーション (SPA) に最適です。 | 中間レベル、高度です |
| Web ページ  | Classic ASP、PHP     | HTML マークアップと同じファイルで同時にコード | 新しい、中間レベル |

### <a name="web-forms"></a>Web フォーム

ASP.NET Web フォームでは、使い慣れたドラッグ アンド ドロップ、イベント ドリブン モデルを使用して動的な web サイトを構築できます。 デザイン サーフェスや数百のコントロールとコンポーネントを洗練された強力な UI 駆動型サイトとデータ アクセスを迅速に構築できます。 

[詳細については、Web フォーム](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC ことができます、強力なパターン ベースの問題を明確に分離を有効にしてを完全に制御のマークアップを切し離しの動的な web サイトを構築します。 ASP.NET MVC には、最新の web 標準を使用する高度なアプリケーションを作成するため、高速な TDD 対応の開発を有効にする多くの機能が含まれています。 

[MVC の詳細を表示します](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web ページ

ASP.NET Web ページと Razor 構文は、高速、わかりやすく、軽量な方法を組み合わせて動的 web コンテンツを作成する HTML、サーバー コードを提供します。 データベースへの接続、動画の追加、ソーシャル ネットワーク サイトへのリンク、および多くのに役立つ多くの機能は、最新の web 標準に準拠する美しいサイトを作成します。

[Web ページの詳細を表示します](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Web フォーム、MVC、および Web ページに関する注意事項

次の 3 つすべての ASP.NET フレームワークは、.NET Framework に基づいており、ASP.NET および .NET のコア機能を共有します。 たとえば、3 つすべてのフレームワークは、メンバーシップに基づいてログイン セキュリティ モデルを提供し、3 つすべての共有、同じ施設要求や処理セッションを管理するためのコア ASP.NET の機能の一部であります。

さらに、3 つのフレームワークは完全に独立してではないと、いずれかを選択する場合は別に使用します。 フレームワークは、同じ web アプリケーションで共存させることができます、ために、さまざまなフレームワークを使用して記述されたアプリケーションの個々 のコンポーネントを表示するは珍しいことはできません。 たとえば、アプリの顧客に対面する部分は、データ アクセスと管理の部分はデータ コントロールと単純なデータ アクセスを活用するために Web フォームで開発中に、マークアップを最適化するために MVC で開発された可能性があります。

## <a name="web-apis"></a>Web API

ASP.NET Web API は、さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達できる HTTP サービスを作成しやすくフレームワークです。 ASP.NET Web API は、.NET Framework に基づいて RESTful アプリケーションを構築するのに最適なプラットフォームです。

[Web API の詳細を表示します](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>リアルタイム テクノロジ

ASP.NET SignalR は、リアルタイム web 機能の開発が容易、ASP.NET 開発者向けの新しいライブラリです。 SignalR では、サーバーとクライアント間の双方向通信を許可します。 サーバーは、利用可能になった接続クライアントに対して瞬時にコンテンツでプッシュできます。 SignalR は、Web ソケットをサポートし、古いブラウザーの互換性のあるその他の手法にフォールバックします。 SignalR には、接続管理用の Api が含まれています (たとえば、接続し、切断イベント、) 接続、および承認をグループ化します。

[SignalR の詳細を表示します](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>モバイル アプリとサイト 

ASP.NET は、モバイル web サイトの Twitter のブートス トラップと同様に、レスポンシブ デザイン フレームワークを使用すると同様に、Web API バック エンド ネイティブ モバイル アプリで電源ことができます。 ネイティブ モバイル アプリを構築する場合は、ハンドルのデータ アクセス、認証、およびプッシュ通知をアプリに JSON ベースの Web API の作成が容易になります。 応答性の高いモバイル サイトを構築する場合は、CSS フレームワークやオープン グリッド システムするか、jQuery Mobile または Sencha PhoneGap で優れたのモバイル アプリケーションなどの強力なモバイル システムを選択を行うこともできます。

[モバイル アプリとサイトの開発に関する詳細します。](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>単一ページ アプリケーション 

ASP.NET の単一ページ アプリケーション (SPA) を使用して、HTML 5、CSS 3、JavaScript を使用して重要なのクライアント側の相互作用を含むアプリケーションを構築できます。 Visual Studio には、knockout.js と ASP.NET Web API を使用する単一ページ アプリケーションを構築するためのテンプレートが含まれています。 組み込みの SPA テンプレート以外にも SPA テンプレートのコミュニティによって作成されたもダウンロードできます。

[詳細については、単一ページ アプリケーションの開発](single-page-application/index.md)

## <a name="webhooks"></a>Web フック

Webhook は、Web Api および SaaS サービスを一緒に配線の単純なパブリッシュ/サブスクライブ モデルを提供する簡易 HTTP パターンです。 イベント発生すると、サービスで HTTP POST 要求の形式で登録されたサブスクライバーに通知が送信されます。 POST 要求には、これによりを適宜動作し、受信側のイベントに関する情報が含まれています。

Webhook は、Dropbox、GitHub、Instagram、MailChimp、PayPal、余裕期間、Trello、さらに多くなどのサービスの数が多いによって公開されます。 たとえば、WebHook するか Dropbox の場合で、ファイルが変更または、GitHub でコードの変更がコミットされてまたは、PayPal の支払いが開始されました Trello でカードが作成されました。

[Webhook の詳細を表示します](webhooks/index.md)





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
