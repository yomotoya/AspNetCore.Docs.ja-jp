---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: ASP.NET 状態監視 (c#) によるエラーの詳細をログ記録 |Microsoft Docs
author: rick-anderson
description: Microsoft の正常性の監視システムは、ハンドルされない例外などのさまざまな web イベントを記録する簡単かつカスタマイズ可能な方法を提供します。 このチュートリアルでは、そのを使用しています.
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: bd6b89f63c6d0634e6d6b809d8285b9870485c43
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842644"
---
<a name="logging-error-details-with-aspnet-health-monitoring-c"></a>ASP.NET 状態監視 (c#) によるエラーの詳細をログ記録
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> Microsoft の正常性の監視システムは、ハンドルされない例外などのさまざまな web イベントを記録する簡単かつカスタマイズ可能な方法を提供します。 このチュートリアルでは、未処理の例外をデータベースにログインして、電子メール メッセージを使用して開発者に通知する状態監視システムの設定手順について説明します。


## <a name="introduction"></a>はじめに

ログは、デプロイされたアプリケーションの正常性の監視と発生する可能性のある問題を診断するための便利なツールです。 解決するように、デプロイされたアプリケーションで発生するエラーをログに特に重要ですが。 `Error` ; ASP.NET アプリケーションでハンドルされない例外が発生するたびにイベントが発生した、[前のチュートリアル](processing-unhandled-exceptions-cs.md)エラーの開発者に通知し、のイベントハンドラーを作成してその詳細をログ記録する方法を示しました`Error`イベント。 ただし、作成、 `Error` ASP では、このタスクを実行できるように、エラーの詳細をログ記録し、開発者に通知するイベント ハンドラーは不要です。NET の*状態監視システム*します。

状態監視システムは、ASP.NET 2.0 で導入され、アプリケーションまたは要求の有効期間中に発生するイベントのログ記録によってデプロイされた ASP.NET アプリケーションの正常性を監視するために設計されています。 状態監視システムによって記録されたイベントと呼びます*状態監視イベント*または*Web イベント*、含める。

- アプリケーションが開始または停止などのアプリケーションの有効期間イベント
- など、セキュリティ イベントは、ログイン試行が失敗し、URL 承認の要求に失敗しました
- 含む、アプリケーション エラー未処理の例外、例外、要求の検証例外、およびその他の種類のエラーの間でのコンパイル エラーを解析ビュー ステート。

任意の数に記録できます、正常性の監視イベントが発生したときに指定された*ログ ソース*。 状態監視システムは、Windows イベント ログに、または他のユーザーの間で、電子メール メッセージを使用して、Microsoft SQL Server データベースに Web イベントを記録するログ ソースでは出荷されます。 独自のログ ソースを作成することもできます。

使用すると、ログ ソースと共に、状態監視システムに記録するイベントが定義されている`Web.config`します。 構成マークアップの数行では、データベースへすべてのハンドルされない例外を記録して通知する電子メールを使用して例外の監視の正常性を使用できます。

## <a name="exploring-the-health-monitoring-systems-configuration"></a>システムの構成の監視の正常性を調べる

ある、その構成情報によって正常性監視システムの動作が定義されている、 [ `<healthMonitoring>`要素](https://msdn.microsoft.com/library/2fwh2ss9.aspx)で`Web.config`します。 この構成セクションには、次の 3 つ重要な情報がその他のものを定義します。

1. 正常性イベントを監視するには、発生した、ログに記録する必要があります、
2. ログのソースと
3. 各状態の監視 (1) で定義されているイベントをログ ソースにマップする方法は、(2) で定義されています。

この情報は 3 つの子構成要素を指定: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx)、 [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx)、および[ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx)、それぞれします。

既定の正常性監視システムの構成情報が記載されて、`Web.config`ファイル`%WINDIR%\Microsoft.NET\Framework\version\CONFIG`フォルダー。 簡潔にするため、削除、いくつかのマークアップでこの既定の構成情報が以下に示します。

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

関心のある監視イベントが定義されている正常性、`<eventMappings>`要素で、状態監視イベントのクラスへのわかりやすい名前を提供します。 上記のマークアップで、`<eventMappings>`要素は、正常性の種類のイベントの監視「すべてのエラー」わかりやすい名前を割り当てます`WebBaseErrorEvent`と、名前「失敗の監査」状態の種類のイベントを監視する`WebFailureAuditEvent`します。

`<providers>`要素は、わかりやすい名前を付与することと、ログ ソースに固有の構成情報を指定する、ログ ソースを定義します。 最初の`<add>`要素は、"EventLogProvider"プロバイダーを使用してイベントを監視する指定された正常性のログを定義、`EventLogWebEventProvider`クラス。 `EventLogWebEventProvider`クラスは、Windows イベント ログにイベントを記録します。 2 番目の`<add>`要素定義を使用して Microsoft SQL Server データベースにイベントを記録する"SqlWebEventProvider"プロバイダー、`SqlWebEventProvider`クラス。 "SqlWebEventProvider"構成データベースの接続文字列を指定します (`connectionStringName`) 間での他の構成オプション。

`<rules>`要素で指定されたイベントをマップする、`<eventMappings>`要素ソースのログインに、`<providers>`要素。 既定では、ASP.NET web アプリケーションはすべてのハンドルされない例外のログおよび Windows イベント ログに失敗します。

## <a name="logging-events-to-a-database"></a>データベースにイベントを記録

状態監視システムの既定の構成は、追加することで web アプリケーション、web でアプリケーションごとにカスタマイズできます、`<healthMonitoring>`をアプリケーションのセクション`Web.config`ファイル。 内の追加要素を含めることができます、 `<eventMappings>`、 `<providers>`、および`<rules>`セクションを使用して、`<add>`要素。 既定の構成で使用される設定を削除する、`<remove>`要素、または使用`<clear />`これらのセクションのいずれかからすべての既定値を削除します。 使用して Microsoft SQL Server データベースにすべての未処理の例外の記録に書籍レビューの web アプリケーションを構成しましょう、`SqlWebEventProvider`クラス。

`SqlWebEventProvider`クラスは、状態監視システムの一部であるし、性を監視する指定された SQL Server データベースにイベント ログに記録します。 `SqlWebEventProvider`クラスでは、指定されたデータベースがという名前のストアド プロシージャが含まれている必要があります`aspnet_WebEvent_LogEvent`します。 このストアド プロシージャでは、イベントの詳細が渡され、イベントの詳細を格納する機能を備えています。 良い知らせは、このストアド プロシージャを作成する必要がないことも、プロシージャもイベントの詳細を格納するテーブル。 使用してデータベースにこれらのオブジェクトを追加することができます、`aspnet_regsql.exe`ツール。

> [!NOTE]
> `aspnet_regsql.exe`に説明したツール、 [*サービスを構成する、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-cs.md)ASP のサポートを追加するとき。NET のアプリケーションのサービスです。 書籍レビューの web サイトのデータベースが既に存在する、その結果、`aspnet_WebEvent_LogEvent`ストアド プロシージャをという名前のテーブルにイベント情報を格納する`aspnet_WebEvent_Events`します。


必要なストアド プロシージャとテーブル データベースに追加したら、残っているは正常性監視のすべてのハンドルされない例外をデータベースにログインするように指示します。 Web サイトの次のマークアップを追加することでこれを行う`Web.config`ファイル。

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

正常性監視は上記の構成マークアップ`<clear />`要素から構成情報を監視する定義済みの正常性をワイプする、 `<eventMappings>`、 `<providers>`、および`<rules>`セクション。 これらのセクションのそれぞれに 1 つのエントリを追加します。

- `<eventMappings>`要素が 1 つ性を監視する目的は、ハンドルされない例外が発生したときに発生「すべてエラー」という名前のイベントを定義します。
- `<providers>`要素を使用して"SqlWebEventProvider"という名前の 1 つのログ ソースを定義する、`SqlWebEventProvider`クラス。 `connectionStringName` "ReviewsConnectionString"は、接続の名前を指定する属性が設定された文字列で定義されている、`<connectionStrings>`セクション。
- 最後に、&lt;ルール&gt;要素は、「すべてのエラー」イベントが発生するときに記録します"SqlWebEventProvider"プロバイダーを使用してを示します。

この構成情報には、状態監視システムは、書籍レビューのデータベースにすべてのハンドルされない例外を記録するように指示します。

> [!NOTE]
> `WebBaseErrorEvent`イベントはサーバーのエラーのみ発生します。 存在しない ASP.NET リソースの要求などの HTTP エラーは発生しません。 動作とこれに対し、`HttpApplication`クラスの`Error`サーバーと HTTP エラーの両方で発生するイベントです。


監視アクションでシステム正常性を確認するには、web サイトにアクセスしにアクセスして、実行時エラーを生成`Genre.aspx?ID=foo`します。 適切なエラー ページで、例外の詳細黄色い画面の死 (ときにローカルにアクセスして) または (実稼働環境でサイトにアクセスする) 場合は、カスタム エラー ページのいずれかが表示されます。 バック グラウンドでは、状態監視システムは、データベースにエラー情報を記録します。 1 件のレコードがあります、`aspnet_WebEvent_Events`テーブル (を参照してください**図 1**)。 このレコードには、発生したランタイム エラーに関する情報が含まれています。

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**図 1**: エラーの詳細をログに記録された、`aspnet_WebEvent_Events`テーブル  
([フルサイズの画像を表示する をクリックします](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))。

### <a name="displaying-the-error-log-in-a-web-page"></a>Web ページで、エラー ログを表示します。

Web サイトの現在の構成では、状態監視システムは、データベースにすべてのハンドルされない例外を記録します。 ただし、正常性の監視は提供されませんを web ページを使用して、エラー ログを表示する任意のメカニズム。 ただし、データベースからこの情報を表示する ASP.NET ページを構築できます。 (一時的に表示されるよう、選択できますが、電子メール メッセージで送信エラーの詳細。)

このようなページを作成する場合は、エラーの詳細を表示するのには承認されたユーザーのみを許可するための手順を確認します。 サイトが既にユーザー アカウントを使用する場合は、特定のユーザーまたはロール ページへのアクセスを制限する、URL 承認規則を使用できます。 ログインしているユーザーに基づいて、web ページへのアクセスを制限または許可する方法の詳細についてを参照してください、 [web サイトのセキュリティのチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)します。

> [!NOTE]
> 次のチュートリアルでは、ELMAH という名前の別のエラー ログと通知システムについて説明します。 ELMAH には、RSS フィードと web ページからのエラー ログを表示するための組み込みメカニズムが含まれています。


## <a name="logging-events-to-email"></a>電子メールへのイベントをログ記録

状態監視システムには、電子メール メッセージをイベントに「記録」するログ ソース プロバイダーが含まれています。 ログ ソースには、電子メール メッセージの本文内のデータベースにログに記録されるのと同じ情報が含まれています。 このログ ソースを使用して、特定の正常性の監視イベントが発生すると、開発者に通知することができます。

例外されるたびに電子メールを受信するために発生 web サイトの構成、書籍レビューを更新してみましょう。 これを行うには、次の 3 つのタスクを実行する必要があります。

1. 電子メールを送信する ASP.NET web アプリケーションを構成します。 使用して電子メール メッセージを送信する方法を指定することでこれは、`<system.net>`構成要素。 電子メールの送信の詳細については、ASP.NET アプリケーション内のメッセージを参照してください[ASP.NET で電子メールを送信する](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)と[System.Net.Mail FAQ](http://systemnetmail.com/)します。
2. 電子メール ログ ソース プロバイダーを登録、`<providers>`要素、および
3. エントリを追加、 `<rules>` 「すべてのエラー」イベントを (2) の手順で追加のログ ソース プロバイダーにマップされる要素。

状態監視システムには、2 つの電子メール ログ ソース プロバイダー クラスが含まれています:`SimpleMailWebEventProvider`と`TemplatedMailWebEventProvider`します。 [ `SimpleMailWebEventProvider`クラス](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx)送信イベントを含むプレーン テキストの電子メール メッセージの詳細し、電子メールの本文の少しのカスタマイズを提供します。 [ `TemplatedMailWebEventProvider`クラス](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx)が出力されるマークアップは、電子メール メッセージの本文として使用する ASP.NET ページを指定します。 [ `TemplatedMailWebEventProvider`クラス](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx)内容と、電子メール メッセージの形式を厳密に大きい制御を使用するが、さらに初期作業の電子メール メッセージの本文を生成する ASP.NET ページを作成する必要が必要です。 このチュートリアルを使用してでは説明、`SimpleMailWebEventProvider`クラス。

状態監視システムの更新`<providers>`内の要素、`Web.config`ファイルにログのソースを含め、`SimpleMailWebEventProvider`クラス。

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

上記のマークアップを使用して、`SimpleMailWebEventProvider`ログ ソース プロバイダーでは、クラスとフレンドリ名"EmailWebEventProvider"が割り当てられます。 さらに、`<add>`属性にはなど、To、および電子メール メッセージのアドレスからの追加の構成オプションが含まれています。

定義されているメールのログ ソースに残っているはこのソース「未処理の例外のログ」を使用してシステムの監視の正常性を指示します。 これで新しい規則の追加によって実現されます、`<rules>`セクション。

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

`<rules>`セクションが 2 つの規則が含まれています。 「すべてのエラーへの電子メール」、という名前の最初の 1 つは、すべてのハンドルされない例外を"EmailWebEventProvider"ログのソースに送信します。 このルールは、web サイトを指定したエラーの詳細を送信する効果をアドレスにします。 "すべてのエラー データベースへのルールは、サイトのデータベースに、エラーの詳細を記録します。 その結果、その詳細が、サイトでハンドルされない例外が発生する両方データベースにログに記録は、指定した電子メール アドレスに送信します。

**図 2**によって生成された電子メールを示しています、`SimpleMailWebEventProvider`クラスにアクセスすると`Genre.aspx?ID=foo`します。

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**図 2**:、エラーの詳細が電子メール メッセージで送信されます  
([フルサイズの画像を表示する をクリックします](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))。

## <a name="summary"></a>まとめ

ASP.NET 状態監視システムは、管理者デプロイされた web アプリケーションのヘルスを監視できるように設計されています。 アプリケーションが停止したら、ユーザーが正常にログイン時のサイトになど、特定のアクションを展開するときに、または未処理の例外が発生した場合、状態監視イベントが発生します。 ログ ソースの任意の数には、これらのイベントを記録できます。 このチュートリアルでは、データベースと、電子メール メッセージを通じて、未処理の例外の詳細を記録する方法を示しました。

このチュートリアルは、未処理の例外のログに留意する正常性の監視がデプロイされた ASP.NET アプリケーションの全体的な正常性の測定し、豊富な正常性イベントの監視が含まれていますが、ログ ソースを監視する health の使用に重点を置いてください。ここで説明します。 詳細については、独自の正常性監視イベントとログのソースを作成することができます必要に応じて発生します。 正常性の監視の詳細を習得に関心は、優れた第一歩ですに目を通して[Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)の[正常性の監視に関する FAQ](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)します。 次を参照してください[How To: ASP.NET 2.0 で使用して正常性の監視](https://msdn.microsoft.com/library/ms998306.aspx)します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET 状態監視の概要](https://msdn.microsoft.com/library/bb398933.aspx)
- [構成および監視システムは ASP.NET の正常性をカスタマイズします。](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [よく寄せられる質問 - ASP.NET 2.0 での監視の正常性](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [方法: 正常性監視通知の電子メールを送信します。](https://msdn.microsoft.com/library/ms227553.aspx)
- [ASP.NET でヘルスの監視を使用する方法](https://msdn.microsoft.com/library/ms998306.aspx)
- [ASP.NET での監視の正常性](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [前へ](processing-unhandled-exceptions-cs.md)
> [次へ](logging-error-details-with-elmah-cs.md)
