---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: "ASP.NET の状態 (VB) を監視によるエラーの詳細をログ記録 |Microsoft ドキュメント"
author: rick-anderson
description: "Microsoft の正常性の監視システムは、未処理の例外を含むさまざまな、web イベントをログに記録する、簡単かつカスタマイズ可能な方法を提供します。 このチュートリアルでは、ためを使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: 6a1533b80828532b756940d0b08fe4c6dab2d5dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>ASP.NET の状態 (VB) を監視によるエラーの詳細をログ記録
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Microsoft の正常性の監視システムは、未処理の例外を含むさまざまな、web イベントをログに記録する、簡単かつカスタマイズ可能な方法を提供します。 このチュートリアルは、未処理の例外をデータベースにログインし、電子メール メッセージを開発者に通知する監視システム正常性の設定手順について説明します。


## <a name="introduction"></a>はじめに

ログは、配置済みのアプリケーションの正常性を監視して発生する可能性がある問題を診断するための便利なツールです。 これは、問題を解決するように、配置されたアプリケーションで発生したエラーをログに特に重要です。 `Error`イベントは、ASP.NET アプリケーションでハンドルされない例外が発生するたびに、[前のチュートリアル](processing-unhandled-exceptions-vb.md)エラーの開発者に通知し、のイベントハンドラーを作成してその詳細を記録する方法を示しました`Error`イベント。 ただし、作成、 `Error` ASP では、このタスクを実行できるように、エラーの詳細を記録し、開発者に通知するイベント ハンドラーは必要ありません。NET の*状態監視システム*です。

監視システム正常性は、ASP.NET 2.0 で導入されたし、アプリケーションまたは要求の有効期間中に発生するイベントを記録して配置された ASP.NET アプリケーションのヘルスを監視するよう設計されています。 状態監視システムで記録されたイベントをいいます*状態監視イベント*または*イベントを Web*が含まれます。

- アプリケーションが開始または停止などのアプリケーションの有効期間イベント
- セキュリティ イベントを含むログイン試行が失敗し、URL 承認要求に失敗しました
- 含む、アプリケーション エラー未処理の例外、例外、要求の検証例外、およびその他の種類のエラーの間でのコンパイル エラーを解析ビュー ステート。

状態監視イベントが発生したときに任意の数を記録できます指定*ログ ソース*です。 状態監視システムは、Web イベントを Windows イベント ログに、または他の電子メール メッセージを Microsoft SQL Server データベースに記録するログ ソースに付属します。 独自のログ ソースを作成することもできます。

使用すると、ログ ソースと共に、状態監視システムに記録するイベントが定義されている`Web.config`です。 構成マークアップの数行では、データベースへすべての未処理の例外のログオンと通知する電子メールを使用して、例外のように監視ヘルスを使用できます。

## <a name="exploring-the-health-monitoring-systems-configuration"></a>システムの構成の監視の正常性を調べる

ヘルスの監視システムの動作が内にある、その構成情報によって定義された、 [ `<healthMonitoring>`要素](https://msdn.microsoft.com/en-us/library/2fwh2ss9.aspx)で`Web.config`です。 この構成セクションを定義、特に、情報の次の 3 つの重要な部分。

1. 正常性イベントを監視するには、記録するか、発生時に
2. ログのソースと
3. 各状態の監視 (1) で定義されているイベントをログ ソースにマップする方法は、(2) で定義されています。

この情報は 3 つの子の構成要素を指定: [ `<eventMappings>` ](https://msdn.microsoft.com/en-us/library/yc5yk01w.aspx)、 [ `<providers>` ](https://msdn.microsoft.com/en-us/library/zaa41kz1.aspx)、および[ `<rules>` ](https://msdn.microsoft.com/en-us/library/fe5wyxa0.aspx)、それぞれします。

既定正常性監視システムの構成情報は含まれて、`Web.config`ファイル`%WINDIR%\Microsoft.NET\Framework\version\CONFIG`フォルダーです。 この既定の構成については、いくつかのマークアップを簡略化のため、削除は、次に示します。

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

目的の監視イベントが定義されている正常性、`<eventMappings>`要素は、状態監視イベントのクラスへのわかりやすい名前を提供します。 上記のマークアップで、`<eventMappings>`要素は、正常性の種類のイベントを監視する「すべてのエラー」わかりやすい名前を割り当てます`WebBaseErrorEvent`と、名前「失敗の監査」状態の種類のイベントを監視する`WebFailureAuditEvent`です。

`<providers>`要素はわかりやすい名前を与えることと、ログのソースに固有の構成情報を指定する、ログ ソースを定義します。 最初の`<add>`要素は、"EventLogProvider"プロバイダー、指定の状態の監視を使用してイベントをログに記録を定義、`EventLogWebEventProvider`クラスです。 `EventLogWebEventProvider`クラスにより、イベントが Windows イベント ログに記録します。 2 番目`<add>`要素は、"SqlWebEventProvider"プロバイダー経由で Microsoft SQL Server データベースにイベントをログに記録する、定義、`SqlWebEventProvider`クラスです。 "SqlWebEventProvider"構成データベースの接続文字列を指定します (`connectionStringName`) 間での他の構成オプション。

`<rules>`要素で指定されたイベントをマップする、`<eventMappings>`ソースのログインに要素、`<providers>`要素。 既定では、ASP.NET web アプリケーションはハンドルされない例外がすべてのログし、Windows イベント ログに失敗します。

## <a name="logging-events-to-a-database"></a>データベースにイベントを記録

追加することで web アプリケーション、web、アプリケーションごとにヘルスの監視システムの既定の構成をカスタマイズすることができます、`<healthMonitoring>`セクションをアプリケーションの`Web.config`ファイル。 内の追加要素を含めることができます、 `<eventMappings>`、 `<providers>`、および`<rules>`セクションを使用して、`<add>`要素。 既定の構成を使用してから、設定を削除する、`<remove>`要素、または使用`<clear />`からこれらのセクションのいずれかのすべての既定値を削除します。 使用して Microsoft SQL Server データベースにすべての未処理の例外の記録に書評 web アプリケーションを構成しましょう、`SqlWebEventProvider`クラスです。

`SqlWebEventProvider`クラスは、状態監視システムの一部であるし、性を監視する指定された SQL Server データベースにイベントをログに記録します。 `SqlWebEventProvider`クラスでは、指定されたデータベースがという名前のストアド プロシージャが含まれることが必要ですが`aspnet_WebEvent_LogEvent`です。 このストアド プロシージャは、イベントの詳細が渡され、イベントの詳細を格納するよう課された場合は。 良いニュースは、このストアド プロシージャを作成する必要はありませんプロシージャもイベントの詳細を格納するテーブル。 これらのオブジェクトを追加するには、データベースを使用する、`aspnet_regsql.exe`ツールです。

> [!NOTE]
> `aspnet_regsql.exe`ツールは、バックアップで説明した、 [*サービスの構成、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-vb.md)ASP のサポートを追加するとき。NET のアプリケーションのサービスです。 書評 web サイトのデータベースが既に存在する、その結果、`aspnet_WebEvent_LogEvent`ストアド プロシージャをという名前のテーブルにイベント情報を格納する`aspnet_WebEvent_Events`です。


必要なストアド プロシージャと、データベースに追加されたテーブルを作成したら、すべては状態、データベースにすべての未処理の例外をログ記録を監視するように指示します。 これを web サイトの次のマークアップを追加することで実現`Web.config`ファイル。

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

状態監視の使用上の構成のマークアップ`<clear />`ワイプする定義済みの状態から構成情報を監視する要素、 `<eventMappings>`、 `<providers>`、および`<rules>`セクション。 これらのセクションのそれぞれに 1 つのエントリを追加します。

- `<eventMappings>`要素は 1 つ性を監視する目的はハンドルされない例外が発生するたびに発生「すべてエラー」という名前のイベントを定義します。
- `<providers>`要素を使用して"SqlWebEventProvider"という名前の 1 つのログ ソースを定義、`SqlWebEventProvider`クラスです。 `connectionStringName` "ReviewsConnectionString"は、接続の名前を指定する属性が設定された文字列で定義されている、`<connectionStrings>`セクションです。
- 最後に、&lt;ルール&gt;要素は、「すべてのエラー」イベントが発生する場合にログ記録すること"SqlWebEventProvider"プロバイダーを使用してを示します。

この構成情報では、状態監視システム書評データベースにすべての未処理の例外をログ記録するように指示します。

> [!NOTE]
> `WebBaseErrorEvent`イベントはサーバー エラー以外の場合は、リソースの要求に、ASP.NET が見つからないことなど、HTTP エラーは発生しません。 これの動作とは異なります、`HttpApplication`クラスの`Error`はサーバーと HTTP エラーの両方で発生するイベントです。


監視アクションでシステム正常性を表示するには、web サイトにアクセスしにアクセスして、実行時エラーを生成`Genre.aspx?ID=foo`です。 適切なエラー ページに、例外の詳細黄色画面の死亡 (ローカル オフィスを訪れた) 場合または (実稼働環境でサイトにアクセスする) 場合は、カスタム エラー ページのいずれかが表示されます。 背後では、監視システム正常性は、データベースにエラー情報を記録します。 1 つのレコードが存在する必要があります、`aspnet_WebEvent_Events`テーブル (を参照してください**図 1**) です。 このレコードに発生したランタイム エラーに関する情報が含まれています。

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**図 1**: エラーの詳細をログに記録された、`aspnet_WebEvent_Events`テーブル  
([フルサイズのイメージを表示するをクリックして](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Web ページで、エラー ログを表示します。

Web サイトの現在の構成では、状態監視システムは、データベースにすべての未処理の例外を記録します。 ただし、正常性の監視は提供されませんを web ページで、エラー ログを表示する任意のメカニズム。 ただし、データベースからこの情報を表示するための ASP.NET ページを作成します。 (と思いますすぐに、選択できますが、電子メール メッセージで送信するエラーの詳細。)

このようなページを作成する場合は、エラーの詳細を表示するのには承認されたユーザーのみを許可するための手順を確認します。 サイトは既にユーザー アカウントを使用する場合は、特定のユーザーまたはロールのページにアクセスを制限する、URL 承認規則を使用できます。 ログインしているユーザーに基づいて web ページへのアクセスを制限または許可する方法の詳細についてを参照してください  [web サイトのセキュリティ チュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)です。

> [!NOTE]
> その後のチュートリアルでは、ELMAH という別のエラー ログに記録して通知システムについて説明します。 ELMAH には、RSS フィードと、両方の web ページからのエラー ログを表示する組み込みのメカニズムが含まれています。


## <a name="logging-events-to-e-mail"></a>電子メールをイベント ログの記録

状態監視システムでは、「イベントをログに」電子メール メッセージをログのソース プロバイダーが含まれています。 ログのソースには、電子メール メッセージの本文内のデータベースにログに記録されるのと同じ情報が含まれています。 このログ ソースを使用して、特定のヘルスの監視イベントが発生すると、開発者に通知することができます。

Web サイトの構成おに例外されるたびに電子メールを受信できるようにが行われる書評を更新してみましょう。 これを実現するには、次の 3 つのタスクを実行する必要があります。

1. 電子メールを送信する ASP.NET web アプリケーションを構成します。 使用して電子メール メッセージを送信する方法を指定してこの情報は、`<system.net>`構成要素。 ASP.NET アプリケーション内のメッセージを電子メールの送信の詳細についてを参照してください[ASP.NET で電子メールを送信する](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)と[System.Net.Mail FAQ](http://systemnetmail.com/)です。
2. 電子メール ログ ソース プロバイダーを登録、`<providers>`要素、および
3. エントリを追加、 `<rules>` (2) の手順で追加ログ ソース プロバイダーを「すべてのエラー」イベントにマップする要素。

状態監視システムには、次の 2 つの電子メール ログ ソース プロバイダー クラスが含まれています:`SimpleMailWebEventProvider`と`TemplatedMailWebEventProvider`です。 [ `SimpleMailWebEventProvider`クラス](https://msdn.microsoft.com/en-us/library/system.web.management.simplemailwebeventprovider.aspx)イベントを含むテキスト形式の電子メール メッセージの詳細、および電子メールの本文のカスタマイズはほとんどの説明を送信します。 [ `TemplatedMailWebEventProvider`クラス](https://msdn.microsoft.com/en-us/library/system.web.management.templatedmailwebeventprovider.aspx)がレンダリングされるマークアップが電子メール メッセージの本文として使用する ASP.NET ページを指定します。 [ `TemplatedMailWebEventProvider`クラス](https://msdn.microsoft.com/en-us/library/system.web.management.templatedmailwebeventprovider.aspx)多くの内容と、電子メール メッセージの形式をより柔軟に制御が、先行作業は、電子メール メッセージの本文を生成する ASP.NET ページを作成する必要は少し多く必要があります。 このチュートリアルに焦点を当てます、`SimpleMailWebEventProvider`クラスです。

監視システムの正常性を更新`<providers>`内の要素、`Web.config`ファイルにログのソースを含め、`SimpleMailWebEventProvider`クラス。

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

上記のマークアップを使用して、`SimpleMailWebEventProvider`ログ ソース プロバイダーでは、クラス"EmailWebEventProvider"フレンドリ名を割り当てます。 さらに、`<add>`属性には、追加の構成オプション、To などと、電子メール メッセージのアドレスが含まれています。

定義されている電子メールのログ ソースには状態監視「ログイン」未処理の例外には、このソースを使用するシステムに指示します。 新しいルールを追加することでこれを行う、`<rules>`セクション。

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

`<rules>`セクションにはここで 2 つの規則が含まれています。 「すべてのエラーへの電子メール」という名前の最初の 1 つは、すべての未処理の例外を"EmailWebEventProvider"ログのソースに送信します。 この規則は、web サイトを指定したエラーの詳細を送信するの効果をアドレスにします。 「すべてのエラー データベースへの」ルールは、サイトのデータベースに、エラーの詳細を記録します。 その結果、未処理の例外で、サイトでその詳細が発生したときに両方データベースにログに記録され指定した電子メール アドレスに送信されます。

**図 2**によって生成された電子メールを示しています、`SimpleMailWebEventProvider`へのアクセス時`Genre.aspx?ID=foo`です。

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**図 2**: のエラーの詳細が電子メール メッセージで送信されます  
([フルサイズのイメージを表示するをクリックして](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>概要

ASP.NET の正常性監視システムは管理者が展開された web アプリケーションのヘルスを監視するようにするよう設計されています。 状態監視イベントを発生するは、アプリケーションが停止すると、ときにユーザーが正常にログオン、サイトなど、特定のアクションを展開するとき、または、ハンドルされない例外が発生します。 これらのイベントは、ログのソースの任意の数を記録できます。 このチュートリアルでは、データベースと電子メール メッセージでは、未処理の例外の詳細を記録する方法を示しました。

このチュートリアルを使用して正常性の未処理の例外の記録、留意正常性の監視、展開済みの ASP.NET アプリケーションの全体的なヘルスを測定するものでは、さまざまな状態監視イベントが含まれていますが、ログ ソースを監視に重点を置いてください。ここで説明します。 詳細は、独自の状態のイベントとログのソースを監視を作成することができます必要に応じて発生します。 目を通すな最初の手順は、正常性の監視について詳細に知りたい場合は、 [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)の[正常性の監視に関する FAQ](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)です。 次を参照してください[How To: ASP.NET 2.0 で使用する正常性の監視](https://msdn.microsoft.com/en-us/library/ms998306.aspx)です。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET の状態監視の概要](https://msdn.microsoft.com/en-us/library/bb398933.aspx)
- [構成および監視 ASP.NET のシステム正常性をカスタマイズします。](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [よく寄せられる質問 - 正常性の ASP.NET 2.0 の監視](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [方法: 正常性の監視の通知の電子メールを送信します。](https://msdn.microsoft.com/en-us/library/ms227553.aspx)
- [ASP.NET の正常性の監視を使用する方法](https://msdn.microsoft.com/en-us/library/ms998306.aspx)
- [ASP.NET での監視のヘルス](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

>[!div class="step-by-step"]
[前へ](processing-unhandled-exceptions-vb.md)
[次へ](logging-error-details-with-elmah-vb.md)
