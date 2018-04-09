---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: ELMAH (VB) によるエラーの詳細をログ記録 |Microsoft ドキュメント
author: rick-anderson
description: エラー ログのモジュールとハンドラー (ELMAH) には、実稼働環境で実行時エラーのログ記録をもう 1 つの方法が用意されています。 ELMAH には、無料、オープン ソースのエラーが、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: 584791a944c9e8eb0113da68719292f448573980
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="logging-error-details-with-elmah-vb"></a>ELMAH (VB) によるエラーの詳細をログ記録
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> エラー ログのモジュールとハンドラー (ELMAH) には、実稼働環境で実行時エラーのログ記録をもう 1 つの方法が用意されています。 ELMAH は、エラーをフィルター処理や RSS フィード、として、web ページからのエラー ログを表示するか、コンマ区切りファイルとしてダウンロードする機能などの機能を含んだ無料、オープン ソース エラーのログ記録ライブラリです。 このチュートリアルは、ダウンロードを ELMAH を構成する方法について説明します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](logging-error-details-with-asp-net-health-monitoring-vb.md)ASP を検査します。NET のヘルスを監視システムで、さまざまな Web イベントを記録するためのボックス ライブラリの外を提供します。 多くの開発者は、状態をログに記録して、未処理の例外の詳細を電子メールで監視を使用します。 ただし、これにはこのシステムに、いくつかの問題点があります。 まずがないことをログに記録されたイベントに関する情報を表示するためのユーザー インターフェイスのあらゆる種類です。 どちらポーク、database を使用する必要があります 10 の最後のエラーの概要を参照してください。 または、1 週間以内に発生したエラーの詳細を表示する場合は、、、電子メールの受信トレイをスキャンまたはからの情報を表示する web ページを構築、`aspnet_WebEvent_Events`テーブル。

別の問題点を中心に正常性の監視の複雑さです。 正常性の監視は、多数の異なるイベントの記録に使用できる、さまざまなイベントがログに記録する方法とタイミングに指示するためのオプションがあるため、監視システム正常性を正しく構成するは、煩雑作業です。 最後に、互換性の問題があります。 正常性の監視最初が追加されたので、.NET Framework バージョン 2.0 は ASP.NET のバージョンを使用して構築された古いバージョンの web アプリケーションの使用可能な 1.x です。 さらに、`SqlWebEventProvider`データベース エラーの詳細をログには、前のチュートリアルで使用して、クラスは、Microsoft SQL Server データベースでのみ動作します。 XML ファイルまたは Oracle データベースなど、代替データ ストアへのエラーのログが必要にカスタム ログ プロバイダー クラスを作成する必要があります。

ヘルスのシステムを監視する代わりに、エラー ログのモジュールとハンドラー (ELMAH)、によって作成された、無料、オープン ソースのエラーのログ記録システム[Atif 自分](http://www.raboof.com/)です。 2 つのシステムの最も顕著な違いは、RSS フィードとしてのエラーと、web ページから、特定のエラーの詳細の一覧を表示する ELAMH の機能です。 ELMAH は、正常性の監視のエラーを記録するだけのための構成を容易にします。 さらに、ELMAH には ASP.NET 1.x、ASP.NET 2.0 では、アプリケーションおよび ASP.NET 3.5 アプリケーション、およびさまざまなソースのログ プロバイダーが付属していますのサポートが含まれています。

このチュートリアルは、ELMAH ASP.NET アプリケーションへの追加に関連する手順を示します。 開始しましょう!

> [!NOTE]
> 長所と短所の独自セットを持っている両方状態システムと ELMAH を監視します。 ぜひ両方のシステムに合ったものを決定するニーズください。


## <a name="adding-elmah-to-an-aspnet-web-application"></a>ELMAH は、ASP.NET Web アプリケーションへの追加

ELMAH を新規または既存の ASP.NET アプリケーションに統合することは、5 分未満となるを簡単に、簡単なプロセスです。 簡単に言うと、次の 4 つの簡単な手順が含まれます。

1. ELMAH をダウンロードし、追加、 `Elmah.dll` web アプリケーションにアセンブリ
2. ELMAH の HTTP モジュールとハンドラーに登録`Web.config`、
3. ELMAH の構成オプションを指定し、
4. 必要な場合は、エラー ログのソース インフラストラクチャを作成します。

これら 4 つのステップの 1 つずつ各みましょう。

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>手順 1: ELMAH プロジェクト ファイルをダウンロードして、追加`Elmah.dll`Web アプリケーション

このチュートリアルで使用可能なダウンロードには ELMAH 1.0 BETA 3 (ビルド 10617) ドキュメントの作成時に、最新バージョンが含まれます。 またはを参照してください、 [ELMAH web サイト](https://code.google.com/p/elmah/)を最新バージョンを取得するか、ソース コードをダウンロードします。 デスクトップ上のフォルダーに ELMAH ダウンロードを抽出し、ELMAH アセンブリ ファイルが見つかりません (`Elmah.dll`)。

> [!NOTE]
> `Elmah.dll` 、ダウンロードに保存されている`Bin`サブフォルダー異なる .NET Framework のバージョンとリリース バージョンとデバッグ ビルドのあるフォルダーです。 リリース ビルドを使用して、適切なフレームワークのバージョン。 ASP.NET 3.5 web アプリケーションを構築する場合はコピーなど、`Elmah.dll`ファイルから、`Bin\net-3.5\Release`フォルダーです。


次に、Visual Studio を開き、ソリューション エクスプ ローラーと選択、コンテキスト メニューからの参照の追加の web サイト名を右クリックして、アセンブリをプロジェクトに追加します。 [参照の追加] ダイアログ ボックスが表示されます。 [参照] タブに移動し、選択、`Elmah.dll`ファイル。 この操作は追加、`Elmah.dll`ファイルを web アプリケーションの`Bin`フォルダーです。

> [!NOTE]
> Web アプリケーション プロジェクト (WAP) 型が表示されない、`Bin`ソリューション エクスプ ローラーでフォルダーです。 代わりに、[参照] フォルダーの下でこれらの項目が一覧表示します。


`Elmah.dll` ELMAH システムで使用されるクラスがアセンブリに含まれています。 これらのクラスは、3 つのカテゴリに分類されます。

- **HTTP モジュール**-HTTP モジュールのイベント ハンドラーを定義するクラスは、`HttpApplication`イベントなど、`Error`イベント。 ELMAH には、次の 3 つの最も密接な複数の HTTP モジュールが含まれています。 中。 

    - `ErrorLogModule` -ログのソースに未処理の例外をログに記録します。
    - `ErrorMailModule` -電子メール メッセージでハンドルされない例外の詳細を送信します。
    - `ErrorFilterModule` -どのような例外がログに記録されますを決定する開発者が指定したフィルターと新機能に適用されるものは無視されます。
- **HTTP ハンドラー** -HTTP ハンドラーが特定の種類の要求のマークアップを生成するクラス。 ELMAH には、web ページとして、RSS フィード、またはコンマ区切り (CSV) ファイルとしては、エラーの詳細を表示する HTTP ハンドラーが含まれています。
- **エラー ログのソース**- ELMAH ログに記録できますが、Oracle データベースへの Microsoft Access データベースへの Microsoft SQL Server データベースをメモリにエラーをすぐ SQLite データベース、または Vista DB データベース、XML ファイルです。 監視システム正常性と同様に ELMAH のアーキテクチャは、つまり、作成し、必要な場合は、独自のカスタム ログ ソース プロバイダーをシームレスに統合できるプロバイダー モデルを使用してビルドされました。

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>手順 2: ELMAH の HTTP モジュールとハンドラーを登録します。

中に、`Elmah.dll`ファイルには、HTTP モジュールが含まれています、未処理の例外を自動的にログオンして、web ページからのエラーの詳細を表示する必要があるハンドラーと、これらの web アプリケーションの構成で明示的に登録する必要があります。 `ErrorLogModule`を HTTP モジュール、登録後、サブスクライブしている、`HttpApplication`の`Error`イベント。 このイベントは発生するたびに、`ErrorLogModule`指定されたログのソースに、例外の詳細を記録します。 次のセクションで、ログのソース プロバイダーを定義する方法をお見せ「ELMAH 構成」。 `ErrorLogPageFactory` HTTP ハンドラー ファクトリが、web ページから、エラー ログを表示するときに、マークアップを生成します。

HTTP モジュールとハンドラーを登録するための特定の構文は、サイトの電源が web サーバーによって異なります。 ASP.NET 開発サーバーと Microsoft の iis 6.0 とそれ以前のバージョンで HTTP モジュールとハンドラーを登録します。、`<httpModules>`と`<httpHandlers>`セクションでは、内部に記述する、`<system.web>`要素。 IIS 7.0 を使用している場合に登録する必要がある、`<system.webServer>`要素の`<modules>`と`<handlers>`セクションです。 HTTP モジュールとハンドラーを定義するさいわい、*両方*使用されている web サーバーに関係なく配置します。 このオプションは最もポータブル使用されている web サーバーに関係なく、開発および運用環境で使用する同じ構成が許可されているです。

登録することによって開始、 `ErrorLogModule` HTTP モジュールと`ErrorLogPageFactory`で HTTP ハンドラー、`<httpModules>`と`<httpHandlers>`」の「`<system.web>`です。 定義している場合、構成は既にこれら 2 つの要素、単に含める、 `<add>` ELMAH の HTTP モジュールとハンドラー用の要素。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

ELMAH の HTTP モジュールとハンドラーを次に、登録、`<system.webServer>`要素。 以前と同様、この要素が、構成内に存在しない場合、追加します。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

既定では、IIS 7 という苦情で HTTP モジュールとハンドラーが登録されているかどうか、`<system.web>`セクションです。 `validateIntegratedModeConfiguration`属性、`<validation>`要素をこのようなエラー メッセージを抑制する IIS 7 に指示します。

なおを登録するための構文、 `ErrorLogPageFactory` HTTP ハンドラーが含まれています、`path`に設定されている属性`elmah.axd`です。 この属性にする場合、web アプリケーションに通知という名前のページに対する要求を受信した`elmah.axd`で要求を処理するかして、 `ErrorLogPageFactory` HTTP ハンドラー。 後ほどお見せ、 `ErrorLogPageFactory` HTTP ハンドラーのこのチュートリアルの後半で動作します。

### <a name="step-3-configuring-elmah"></a>手順 3: ELMAH を構成します。

Web サイトの構成オプションを探します ELMAH`Web.config`という名前のカスタム構成セクション内のファイル`<elmah>`です。 カスタム セクションを使用するために`Web.config`で定義する必要があります最初、`<configSections>`要素。 開く、`Web.config`ファイルし、次のマークアップを追加、 `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> ELMAH ASP.NET 1.x アプリケーションを構成する場合は、削除、`requirePermission="false"`属性から、`<section>`要素上です。


上記の構文を登録、カスタム`<elmah>`セクションと、サブセクション: `<security>`、 `<errorLog>`、 `<errorMail>`、および`<errorFilter>`です。

次に、追加、`<elmah>`セクション`Web.config`です。 このセクションと同じレベルで表示する必要があります、`<system.web>`要素。 内部、`<elmah>`セクションを追加、`<security>`と`<errorLog>`のセクションでは次のように。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

`<security>`セクションの`allowRemoteAccess`属性は、リモート アクセスが許可されているかどうかを示します。 この値が 0 に設定されている場合、エラー ログの web ページだけ表示できますローカルです。 この属性が 1 に設定されている場合、エラー ログの web ページは、ローカルとリモートの両方の訪問者が有効です。 ここでは、リモートの訪問者、エラー ログの web ページを無効にしてみましょう。 これによりセキュリティ上の問題を説明する機会を取得したらお後でリモート アクセスを許可します。

`<errorLog>`セクション定義のエラー ログのソースのエラーの詳細を記録する場所ですに似ていますがどの決定、`<providers>`監視システム正常性のセクションです。 上記の構文を指定します、`SqlErrorLog`クラスによって指定された Microsoft SQL Server データベースにエラーが記録されるエラー ログのソースとして、`connectionStringName`属性の値。

> [!NOTE]
> ELMAH は、エラーをログに、XML ファイル、Microsoft Access データベース、Oracle データベース、およびその他のデータ ストアを使用できる追加のエラー ログ プロバイダーが付属します。 このサンプルを参照してください`Web.config`これらの代替のエラー ログ プロバイダーを使用する方法について ELMAH ダウンロードに含まれているファイル。


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>手順 4: エラー ログのソース インフラストラクチャの作成

ELMAH の`SqlErrorLog`プロバイダーに指定された Microsoft SQL Server データベース エラーの詳細をログに記録します。 `SqlErrorLog`プロバイダーという名前のテーブルには、このデータベースで求められる`ELMAH_Error`および 3 つのストアド プロシージャ: `ELMAH_GetErrorsXml`、 `ELMAH_GetErrorXml`、および`ELMAH_LogError`です。 ELMAH ダウンロードには、という名前のファイルが含まれている`SQLServer.sql`で、`db`ストアド プロシージャを次の表と、これらを作成するための T-SQL が含まれているフォルダーです。 使用するデータベースでこれらのステートメントを実行する必要があります、`SqlErrorLog`プロバイダー。

**図 1**と**2**で必要なデータベース オブジェクトの後に Visual Studio でデータベース エクスプ ローラーを表示、`SqlErrorLog`プロバイダーが追加されました。

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**図 1**:`SqlErrorLog`プロバイダー エラーをログに記録、`ELMAH_Error`テーブル

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**図 2**:`SqlErrorLog`プロバイダーは、次の 3 つのストアド プロシージャを使用します。

## <a name="elmah-in-action"></a>アクションで ELMAH

この時点で登録されている、web アプリケーションに ELMAH を追加しましたが、 `ErrorLogModule` HTTP モジュールと`ErrorLogPageFactory`HTTP ハンドラーに ELMAH の構成オプションを指定する`Web.config`の必要なデータベース オブジェクトを追加し、 `SqlErrorLog`エラーのログ プロバイダーです。 アクションで ELMAH を表示する準備が整いました。 書評 web サイトにアクセスしなど、実行時エラーを生成するページを参照してください`Genre.aspx?ID=foo`、または存在しないページなど`NoSuchPage.aspx`です。 これらのページにアクセスしたときに表示される異なります、`<customErrors>`構成し、ローカルまたはリモートにアクセスしたかどうか。 (を参照、 [*カスタム エラー ページを表示する*チュートリアル](displaying-a-custom-error-page-vb.md)をこのトピックの内容を再確認します)。

未処理の例外が発生したときに、ユーザーにどのようなコンテンツが表示に ELMAH は影響しませんだけ、その詳細を記録します。 このエラー ログには、web ページからアクセス`elmah.axd`、web サイトのルートからなど`http://localhost/BookReviews/elmah.axd`です。 (プロジェクトでは、要求を受け取ったときにこのファイルが物理的に存在しない`elmah.axd`ランタイム ディスパッチ、 `ErrorLogPageFactory` HTTP ハンドラーは、ブラウザーに送信されるマークアップを生成します)。

> [!NOTE]
> 使用することも、 `elmah.axd` ELMAH テスト エラーを生成するよう指示するページ。 オフィスを訪れた`elmah.axd/test`(でとして`http://localhost/BookReviews/elmah.axd/test`) ELMAH 型の例外をスローすると、 `Elmah.TestException`、エラー メッセージを持つ:「この例外では、テストを無視してかまいません」。


**図 3**にアクセスすると、エラー ログを表示`elmah.axd`開発環境からです。

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**図 3**: `Elmah.axd` Web ページから、エラー ログを表示  
([フルサイズのイメージを表示するをクリックして](logging-error-details-with-elmah-vb/_static/image7.png))

エラー ログ**図 3** 6 つのエラー エントリが含まれています。 各エントリには、HTTP ステータス コード 404 (これらのエラーでは、500)、種類、説明、エラーが発生したときに、ログオン ユーザーの名前と、日付と時刻が含まれています。 エラーの詳細黄色画面の死亡に示すように、同じエラー メッセージを含んでいるページの [詳細] をクリックするとが表示されます (を参照してください**図 4**) と共に、エラーの発生時にサーバー変数の値 (を参照してください**図 5**)。 エラーの詳細が保存されている HTTP POST ヘッダーの値などの追加情報を含んだ生の XML を表示することもできます。

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**図 4**: YSOD のエラーの詳細を表示  
([フルサイズのイメージを表示するをクリックして](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**図 5**: エラー時にサーバー変数のコレクションの値を調べる  
([フルサイズのイメージを表示するをクリックして](logging-error-details-with-elmah-vb/_static/image13.png))

ELMAH を実稼働 web サイトに展開が必要です。

- コピー、`Elmah.dll`ファイルの名前を`Bin`運用上のフォルダー
- ELMAH 固有の構成設定をコピー、`Web.config`実稼働環境で使用されるファイルと
- 実稼働データベースに、エラー ログのソース インフラストラクチャを追加します。

実稼働前のチュートリアルで開発からファイルをコピーするための手法はについて説明してきました。 SQL Server Management Studio を使用して、実稼働データベースに接続しを実行する実稼働データベースで、エラー ログのソース インフラストラクチャを取得する最も簡単な方法は、おそらく、`SqlServer.sql`必要なテーブルが作成され、格納されているスクリプト ファイルプロシージャです。

### <a name="viewing-the-error-details-page-on-production"></a>実稼働環境でのエラーの詳細 ページの表示

実稼働環境には、サイトを展開後に実稼働 web サイトにアクセスし、未処理の例外を生成します。 開発環境と同様に ELMAH に対して効果がありません未処理の例外が発生したときに表示されるエラー ページ代わりに、エラーを記録だけを実行します。 エラー ログ ページにアクセスしようとすると (`elmah.axd`)、運用環境から出てきますに示すように許可されていません ページで**図 6**です。

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**図 6**: 既定では、リモート ユーザーが、エラー ログの Web ページを表示できません  
([フルサイズのイメージを表示するをクリックして](logging-error-details-with-elmah-vb/_static/image16.png))

ELMAH 構成のことに注意してください`<security>`設定セクション、 `allowRemoteAccess` 0 で、リモート ユーザーが、エラー ログを表示することを禁止するための属性です。 エラーの詳細が明らかにセキュリティの脆弱性や他の機密情報として、エラー ログの表示から匿名の訪問者を禁止する重要です。 この属性を 1 に設定し、エラー ログへのリモート アクセスを有効にするかどうかは、ロック ダウンすることが重要、`elmah.axd`パスため、承認されたユーザーのみのがアクセスできます。 追加することでこれを行う、`<location>`要素を`Web.config`ファイル。

次の構成では、エラー ログの web ページにアクセスする管理者の役割でのユーザーのみを許可します。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> 管理者の役割と - Scott、Jisun、および Alice - システムの 3 人のユーザーが追加されている場合に、 [*サービスの構成、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-vb.md)です。 ユーザー Scott と Jisun Admin ロールのメンバーであります。 認証と承認の詳細についてを参照してください  [web サイトのセキュリティ チュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)です。


リモート ユーザーによって、運用環境のエラー ログを表示できます。参照**図 3**、 **4**、および**5**のエラー ログの web ページのスクリーン ショット。 ただし、エラー ログ ページを表示しようとしている匿名または管理者以外のユーザーが自動的にリダイレクト ログイン ページに (`Login.aspx`)、として**図 7**を示しています。

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**図 7**: 承認されていないユーザーがログイン ページに自動的にリダイレクト  
([フルサイズのイメージを表示するをクリックして](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>プログラムでエラーのログ記録

ELMAH の`ErrorLogModule`HTTP モジュールが、指定されたログのソースを自動的に未処理の例外を記録します。 使用して、未処理の例外を発生させることがなく、エラーを記録する代わりに、`ErrorSignal`クラスとその`Raise`メソッドです。 `Raise`メソッドに渡されます、`Exception`オブジェクトし、その例外がスローされた、処理されることがなく、ASP.NET ランタイムの限界に到達場合と、ログに記録します。 差をデータが、要求が後に通常の実行を続行する、`Raise`メソッドが呼び出されて、スローされた、ハンドルされない例外が、要求の通常の実行が中断され、は ASP.NET ランタイムによって、構成を表示するにはエラー ページです。

`ErrorSignal`クラスはここではできない可能性がありますが、何らかのアクションがそのエラーは、全体的な操作を実行中に致命的ではない場合に便利です。 たとえば、web サイト可能性がありますが含まれるユーザーの入力を受け取り、データベースでは、格納、およびユーザーに知らせる電子メールを送信することがある情報が処理されました。 場合は、情報がデータベースに正常に保存するが、電子メール メッセージを送信するときにエラーがある必要がありますか。 1 つのオプションは、例外をスローし、エラー ページに、ユーザーに送信することです。 ただし、これに入力した情報は保存されませんでした、ユーザーを混乱する可能性があります。 別の方法としては、電子メールに関連するエラーを記録、任意の方法で、ユーザーのエクスペリエンスを変更することです。 これは、ような場合、`ErrorSignal`クラスは便利です。

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>エラーの通知を電子メールで

データベース エラーのログ記録、と共にに、エラーの詳細を指定した受信者に電子メールを ELMAH を構成することもできます。 この機能が用意されて、 `ErrorMailModule` HTTP モジュールです。 そのため、この HTTP モジュールを登録する必要があります`Web.config`電子メールでエラーの詳細を送信するためにします。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

次に、エラー電子メールに情報の指定、`<elmah>`要素の`<errorMail>` セクションで、電子メールの送信者と受信者、件名、ことを示す電子メールが非同期的に送信されるかどうかとします。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

インプレース上、設定を使用するたびに、ランタイム エラーが発生 ELMAH メールを送信してsupport@example.comエラーの詳細にします。 ELMAH のエラーの電子メールには、エラーの詳細 web ページ、つまり、エラー メッセージ、スタック トレース、およびサーバー変数に示すように、同じ情報が含まれています (を参照**図 4**と**5**)。 エラー電子メールに添付ファイルとして例外詳細死亡黄色画面のコンテンツも含まれています (`YSOD.html`)。

**図 8** ELMAH のエラーの電子メールにアクセスして生成されるを示しています。`Genre.aspx?ID=foo`です。 中に**図 8**サーバー変数が含まれますさらにダウン電子メールの本文でのみ、エラー メッセージおよびスタック トレースを示しています。

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**図 8**: 電子メールでエラーの詳細を送信する ELMAH を構成することができます  
([フルサイズのイメージを表示するをクリックして](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>のみ関心のあるエラーのログ記録

既定では、ELMAH は 404 およびその他の HTTP エラーなど、すべてのハンドルされない例外の詳細を記録します。 ELMAH これらやその他のエラーをフィルター処理を使用してエラーを無視するよう指示することができます。 フィルター処理のロジックは ELMAH の`ErrorFilterModule`HTTP モジュールを登録する必要があります`Web.config`フィルター処理のロジックを使用するためにします。 フィルターの規則がで指定された、`<errorFilter>`セクションです。

次のマークアップでは、404 エラー ログに記録されません ELMAH ように指示します。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> 必ず、登録する必要がありますエラーをフィルター処理を使用するために、 `ErrorFilterModule` HTTP モジュールです。


`<equal>`内の要素、`<test>`セクションではアサーションと呼ばれます。 アサーションが true に評価された場合は、ELMAH のログからエラーが除外されます。 その他のアサーションがあるなどの: `<greater>`、 `<greater-or-equal>`、 `<not-equal>`、 `<lesser>`、`<lesser-or-equal>`のようにします。 使用してアサーションを組み合わせることも、`<and>`と`<or>`ブール演算子。 さらに、でもをアサーションとして単純な JavaScript 式を含めるしたり、独自のアサーションを c# または Visual Basic で記述できます。

ELMAH のエラーをフィルタ リング機能の詳細についてを参照してください、[セクションのエラーをフィルター処理](https://code.google.com/p/elmah/wiki/ErrorFiltering)で、 [ELMAH wiki](https://code.google.com/p/elmah/w/list)です。

## <a name="summary"></a>まとめ

ELMAH は、ASP.NET web アプリケーションでエラーのログ記録の単純なものでありながら強力なメカニズムを提供します。 同様に Microsoft の正常性の監視システムでは、ELMAH エラーをログをデータベース メールで開発者にエラーの詳細を送信できます。 広範なエラー ログのデータ ストアなどのボックス サポート外、システムの監視状態とは異なり ELMAH が含まれています: Microsoft SQL Server、Microsoft Access、Oracle、XML ファイル、およびその他のいくつか。 さらに、エラー ログと、web ページから特定のエラーの詳細を表示するための組み込みメカニズムは、ELMAH`elmah.axd`です。 `elmah.axd`ページでも、RSS フィード、または Microsoft Excel を使用して読み取ることができるコンマ区切り値ファイル (CSV) としてのエラー情報をレンダリングできます。 宣言的またはプログラムでのアサーションを使用して、ログからエラーをフィルタ ELMAH を指示することもできます。 ELMAH ASP.NET バージョン 1.x アプリケーションで使用できます。

すべての展開済みアプリケーションの未処理の例外を自動的にログ記録と開発チームに通知を送信するための機構が必要です。 かどうかこれは正常性の監視または ELMAH はセカンダリです。 つまり、問題ではありません正常性の監視と ELMAH; を使用するかより両方のシステムを評価し、ニーズに最も合ったものを選択します。 根本的に重要な点は、いくつかのメカニズムが実稼働環境で未処理の例外をログに配置されます。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ELMAH - Error Logging Modules and ハンドラー](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [プロジェクトの ELMAH ページ](https://code.google.com/p/elmah/)(ソース コード、サンプル、wiki)
- [ELMAH アプリケーションへの Web 未処理の例外をキャッチするをプラグインする](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx)(ビデオ)
- [セキュリティ エラーのログ ページ](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [HTTP モジュールとハンドラーを使用して、プラグ可能な ASP.NET コンポーネントを作成するには](https://msdn.microsoft.com/library/aa479332.aspx)
- [Web サイトのセキュリティのチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [前へ](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [次へ](precompiling-your-website-vb.md)
