---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: ELMAH (VB) によるエラーの詳細をログ記録 |Microsoft Docs
author: rick-anderson
description: エラー ログのモジュールとハンドラー (ELMAH) には、実稼働環境で実行時エラーのログを別のアプローチが用意されています。 ELMAH には、無料のオープン ソースのエラーが、.
ms.author: riande
ms.date: 06/09/2009
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: ef87ae342f660a84c9359c7a1674001ed578a68e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838367"
---
<a name="logging-error-details-with-elmah-vb"></a>ELMAH (VB) によるエラーの詳細をログ記録
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> エラー ログのモジュールとハンドラー (ELMAH) には、実稼働環境で実行時エラーのログを別のアプローチが用意されています。 ELMAH は、エラーをフィルター処理、RSS フィードとして、web ページからのエラー ログを表示するか、コンマ区切りファイルとしてダウンロードする機能などの機能が含まれる無料のオープン ソース エラー ログ記録ライブラリです。 このチュートリアルでは、ダウンロードして ELMAH を構成します。


## <a name="introduction"></a>はじめに

[前のチュートリアル](logging-error-details-with-asp-net-health-monitoring-vb.md)ASP を検査します。NET のヘルスを監視システムで、さまざまな Web イベントを記録するためのボックス ライブラリの不足を提供します。 多くの開発者は、正常性をログに記録して、未処理の例外の詳細を電子メールで監視を使用します。 ただし、このシステムでいくつかの問題点があります。 何よりもまずがないことをログに記録されたイベントについての情報を表示するためのユーザー インターフェイスの任意の並べ替えです。 10 の最後のエラーの概要を参照してください。 または、過去 1 週間に発生したエラーの詳細を表示するには、すると、データベースのいずれか開ける必要があります、スキャン、電子メールの受信トレイ、またはビルドからの情報を表示する web ページ、`aspnet_WebEvent_Events`テーブル。

別の問題点は、正常性監視の複雑さを中心として展開します。 正常性の監視は、多くの異なるイベントの記録に使用できる、さまざまなイベントがログに記録する方法とタイミングに指示するためのオプションがあるため、状態監視システムを正しく構成するは、面倒の作業です。 最後に、互換性の問題があります。 正常性の監視は、バージョン 2.0 では、.NET Framework には追加された最初、ために、ASP.NET のバージョンを使用して構築された以前の web アプリケーションの使用はできません 1.x します。 さらに、`SqlWebEventProvider`データベースにエラーの詳細をログには、前のチュートリアルで使用したクラスは、Microsoft SQL Server データベースでのみ動作します。 エラーを XML ファイルまたは Oracle データベースなど、代替データ ストアにログインする必要があるカスタム ログ プロバイダー クラスを作成する必要があります。

正常性システムを監視する代わりに、エラー ログのモジュールとハンドラー (ELMAH)、によって作成された無料のオープン ソースのエラーのログ記録システム[Atif Aziz](http://www.raboof.com/)します。 2 つのシステム間で最も顕著な違いは、RSS フィードとしてのエラーと、web ページと、特定のエラーの詳細の一覧を表示する ELAMH の機能です。 ELMAH は、正常性監視のため、エラーのみがログよりも構成するのには簡単です。 さらに、ELMAH には、ASP.NET 1.x、ASP.NET 2.0、および ASP.NET 3.5 のアプリケーションおよび付属のさまざまなログ ソース プロバイダーのサポートが含まれています。

このチュートリアルでは、ELMAH を ASP.NET アプリケーションに追加するのに関連する手順について説明します。 それでは、始めましょう!

> [!NOTE]
> 状態監視システムと ELMAH 両方長所と短所の独自セットがあります。 お客様のニーズ両方のシステムをどのような 1 つに合ったを決定することです。


## <a name="adding-elmah-to-an-aspnet-web-application"></a>ELMAH を ASP.NET Web アプリケーションに追加します。

ELMAH を新規または既存の ASP.NET アプリケーションに統合することは、5 分以内に受け取る簡単プロセスです。 簡単に言うと、次の 4 つの簡単な手順が含まれます。

1. ELMAH をダウンロードし、追加、 `Elmah.dll` web アプリケーションのアセンブリ
2. ELMAH の HTTP モジュールとハンドラーを登録`Web.config`、
3. ELMAH の構成のオプションを指定し、
4. 必要な場合は、エラー ログのソース インフラストラクチャを作成します。

これら 4 つの手順を 1 つずつの各を見てみましょう。

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>手順 1: ELMAH プロジェクト ファイルをダウンロードし、追加`Elmah.dll`Web アプリケーション

ELMAH 1.0 BETA 3 (10617 ビルド)、記事の執筆時点で最新バージョンは、このチュートリアルで使用可能なダウンロードに含まれます。 または、参照してください、 [ELMAH web サイト](https://code.google.com/p/elmah/)最新バージョンを取得するか、ソース コードをダウンロードします。 デスクトップ上のフォルダーに ELMAH ダウンロードを抽出し、ELMAH のアセンブリ ファイルが見つかりません (`Elmah.dll`)。

> [!NOTE]
> `Elmah.dll`ダウンロードの保存されている`Bin`フォルダーで、異なる .NET Framework バージョンとリリース ビルドかデバッグ ビルドのサブフォルダーがあります。 適切なフレームワークのバージョンには、リリース ビルドを使用します。 たとえば、ASP.NET 3.5 の web アプリケーションを作成する場合は、コピー、`Elmah.dll`ファイルから、`Bin\net-3.5\Release`フォルダー。


次に、Visual Studio を開き、ソリューション エクスプ ローラーと選択、コンテキスト メニューから 参照の追加の web サイトの名前を右クリックして、アセンブリをプロジェクトに追加します。 参照の追加 ダイアログ ボックスが表示されます。 [参照] タブに移動し、選択、`Elmah.dll`ファイル。 このアクションを追加、`Elmah.dll`ファイルを web アプリケーションの`Bin`フォルダー。

> [!NOTE]
> Web アプリケーション プロジェクト (WAP) の種類が表示されない、`Bin`ソリューション エクスプ ローラーでフォルダー。 代わりに、[参照] フォルダーの下でこれらの項目が一覧表示します。


`Elmah.dll` ELMAH システムで使用されるクラスがアセンブリに含まれています。 これらのクラスは、3 つのカテゴリに分類されます。

- **HTTP モジュール**-HTTP モジュールのイベント ハンドラーを定義するクラスは、`HttpApplication`イベントなど、`Error`イベント。 ELMAH には、複数の HTTP モジュールでは、3 つの最も関係のものが含まれています。 中。 

    - `ErrorLogModule` -未処理の例外ログ ソースをログに記録します。
    - `ErrorMailModule` -電子メール メッセージでハンドルされない例外の詳細を送信します。
    - `ErrorFilterModule` - とどのような例外がログに記録されますを決定する開発者が指定したフィルターを適用するものは無視されます。
- **HTTP ハンドラー** -HTTP ハンドラは要求の特定の型のマークアップの生成を担当するクラス。 ELMAH には、web ページ、RSS フィード、またはコンマ区切りファイル (CSV) は、エラーの詳細を表示する HTTP ハンドラーが含まれています。
- **エラー ログ ソース**- すぐに Microsoft SQL Server データベース、Oracle データベースへの Microsoft Access データベースに、メモリにエラーを記録できます ELMAH SQLite データベースに指定するか、または Vista DB データベース、XML ファイル。 システムの監視、正常性のように ELMAH のアーキテクチャは、つまりを作成して必要な場合に、独自のカスタム ログ ソース プロバイダーをシームレスに統合できますプロバイダー モデルを使用して構築されました。

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>手順 2: ELMAH の HTTP モジュールとハンドラーの登録

中に、`Elmah.dll`ファイルには、HTTP モジュールが含まれていて、未処理の例外を自動的にログインして、web ページからのエラーの詳細を表示するハンドラーが必要なこれらの web アプリケーションの構成で明示的に登録する必要があります。 `ErrorLogModule`登録後、HTTP モジュールをサブスクライブする、`HttpApplication`の`Error`イベント。 このイベントが発生するたびに、`ErrorLogModule`指定されたログ ソースに、例外の詳細を記録します。 次のセクションで、ログ ソース プロバイダーを定義する方法を見ていきます"Configuring ELMAH"。 `ErrorLogPageFactory` HTTP ハンドラー ファクトリは web ページから、エラー ログを表示するときに、マークアップを生成します。

HTTP モジュールとハンドラーを登録するための特定の構文は、サイトの電源が web サーバーによって異なります。 ASP.NET 開発サーバーと Microsoft の iis 6.0 とそれ以前のバージョンで HTTP モジュールとハンドラーを登録します。、`<httpModules>`と`<httpHandlers>`セクションでは、内に表示されます、`<system.web>`要素。 IIS 7.0 を使用しているかどうかに登録する必要がありますが、`<system.webServer>`要素の`<modules>`と`<handlers>`セクション。 さいわい、HTTP モジュールとハンドラーを定義できます*両方*使用されている web サーバーに関係なく配置されます。 このオプションは、使用されている web サーバーに関係なく、開発および運用環境で使用される同じ構成が許可されている最もポータブルな 1 つ。

登録することで開始、 `ErrorLogModule` HTTP モジュールと`ErrorLogPageFactory`で HTTP ハンドラー、`<httpModules>`と`<httpHandlers>`セクション`<system.web>`します。 定義している場合、構成は既にこれら 2 つの要素、単に含める、 `<add>` ELMAH の HTTP モジュールとハンドラー要素。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

次に、ELMAH の HTTP モジュールとハンドラーを登録、`<system.webServer>`要素。 同様に、この要素が、構成内に存在しない場合、追加します。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

既定では、IIS 7 文句を言うで HTTP モジュールとハンドラーが登録されているかどうか、`<system.web>`セクション。 `validateIntegratedModeConfiguration`属性、`<validation>`要素には、このようなエラー メッセージを抑制する IIS 7 がように指示します。

注意を登録するための構文、 `ErrorLogPageFactory` HTTP ハンドラーが含まれています、`path`属性に設定されている`elmah.axd`します。 この属性は web アプリケーションを場合に通知という名前のページ要求を受信した`elmah.axd`で要求を処理する必要があります、 `ErrorLogPageFactory` HTTP ハンドラー。 見て、 `ErrorLogPageFactory` HTTP ハンドラーのこのチュートリアルの後半で動作します。

### <a name="step-3-configuring-elmah"></a>手順 3: ELMAH を構成します。

ELMAH で web サイトの構成オプションを探します`Web.config`という名前のカスタム構成セクション内のファイル`<elmah>`します。 カスタムのセクションを使用するには`Web.config`最初で定義する必要がありますが、`<configSections>`要素。 開く、`Web.config`ファイルを開き、次のマークアップを追加、 `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> ELMAH ASP.NET 1.x アプリケーションを構成する場合は、削除、`requirePermission="false"`属性を`<section>`上記の要素。


上記の構文の登録、カスタム`<elmah>`セクションと、サブセクション: `<security>`、 `<errorLog>`、 `<errorMail>`、および`<errorFilter>`します。

次に、追加、`<elmah>`セクション`Web.config`します。 このセクションと同じレベルで表示する必要があります、`<system.web>`要素。 内で、`<elmah>`セクションを追加、`<security>`と`<errorLog>`のセクションでは次のように。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

`<security>`セクションの`allowRemoteAccess`属性は、リモート アクセスが許可されているかどうかを示します。 この値が 0 に設定されている場合、エラー ログの web ページのみ表示できますローカル。 この属性が 1 に設定されている場合は、ローカルとリモートの両方の訪問者のエラー ログの web ページが有効にします。 ここでは、リモートの訪問者のエラー ログの web ページを無効にしてみましょう。 これによりセキュリティ上の問題について説明する機会を取得したら後でリモート アクセスを許可します。

`<errorLog>`セクションでは、エラーの詳細が記録されている以外に似ていますが決定するエラー ログのソースを定義します。、`<providers>`監視システム正常性」セクション。 上記の構文を指定します、`SqlErrorLog`クラスで指定された Microsoft SQL Server データベースに、エラー ログに記録されるエラー ログのソースとして、`connectionStringName`属性の値。

> [!NOTE]
> ELMAH は、エラーをログに、XML ファイル、Microsoft Access データベース、Oracle データベース、および他のデータ ストアを使用できる追加のエラー ログ プロバイダーに同梱されています。 このサンプルを参照してください`Web.config`これら代替エラーのログ プロバイダーを使用する方法については、ELMAH のダウンロードに含まれているファイル。


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>手順 4: エラー ログのソースのインフラストラクチャを作成します。

ELMAH の`SqlErrorLog`プロバイダーが指定された Microsoft SQL Server データベースにエラーの詳細を記録します。 `SqlErrorLog`プロバイダーは、このデータベースのという名前のテーブル`ELMAH_Error`と 3 つのストアド プロシージャ: `ELMAH_GetErrorsXml`、 `ELMAH_GetErrorXml`、および`ELMAH_LogError`します。 ELMAH ダウンロードには、という名前のファイルが含まれている`SQLServer.sql`で、`db`ストアド プロシージャを次の表とこれらを作成するための T-SQL が含まれているフォルダー。 これらのステートメントを使用するデータベースで実行する必要があります、`SqlErrorLog`プロバイダー。

**図 1**と**2**で必要なデータベース オブジェクトの後に、Visual Studio でデータベース エクスプ ローラーを表示、`SqlErrorLog`プロバイダーが追加されています。

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**図 1**:`SqlErrorLog`プロバイダー エラーをログに記録、`ELMAH_Error`テーブル

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**図 2**:`SqlErrorLog`プロバイダーは次の 3 つのストアド プロシージャを使用

## <a name="elmah-in-action"></a>アクションで ELMAH

この時点で ELMAH を登録、web アプリケーションに追加しましたが、 `ErrorLogModule` HTTP モジュールと`ErrorLogPageFactory`HTTP ハンドラー、ELMAH の構成オプションを指定した`Web.config`の必要なデータベース オブジェクトを追加し、 `SqlErrorLog`エラーのログ プロバイダーです。 アクションで ELMAH を表示する準備ができました! 書籍レビューの web サイトにアクセスし、ページなど、実行時エラーを生成する`Genre.aspx?ID=foo`、または存在しないページなど`NoSuchPage.aspx`します。 これらのページにアクセスしたときに参照によって異なります、`<customErrors>`構成し、ローカルまたはリモートのどちらをアクセスしているかどうか。 (を参照、 [*カスタム エラー ページを表示する*チュートリアル](displaying-a-custom-error-page-vb.md)のこのトピックでは、します)。

ハンドルされない例外が発生したときに、ユーザーにどのような内容が表示に ELMAH は影響しませんだけ、その詳細を記録します。 このエラーのログは、web ページからアクセスできる`elmah.axd`、web サイトのルートからなど`http://localhost/BookReviews/elmah.axd`します。 (プロジェクトでは、要求を受け取ったときにこのファイルが物理的に存在しない`elmah.axd`ランタイムにディスパッチされます、 `ErrorLogPageFactory` HTTP ハンドラーは、ブラウザーに送信されるマークアップが生成されます)。

> [!NOTE]
> 使用することも、 `elmah.axd` ELMAH テスト エラーを生成するように指示するページ。 訪問`elmah.axd/test`(、 `http://localhost/BookReviews/elmah.axd/test`) ELMAH 型の例外をスローすると、`Elmah.TestException`がエラー メッセージ:「これは、安全に無視できるテスト例外です」。


**図 3**にアクセスすると、エラー ログを表示`elmah.axd`開発環境から。

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**図 3**: `Elmah.axd` Web ページから、エラー ログを表示します。  
([フルサイズの画像を表示する をクリックします](logging-error-details-with-elmah-vb/_static/image7.png))。

ログイン エラー**図 3** 6 つのエラー エントリが含まれています。 各エントリには、HTTP 状態コード (404 またはこれらのエラーでは、500)、種類、説明、エラーが発生したときに、ログオン ユーザーの名前と日付と時刻が含まれています。 エラー詳細黄色い画面の終焉で示すように、同じエラー メッセージを含むページが表示の詳細 リンクをクリックすると (を参照してください**図 4**) と共に、エラー時にサーバー変数の値 (を参照してください**図 5**)。 エラーの詳細が保存されている HTTP POST ヘッダーの値などの追加情報を含んだ生の XML を表示することもできます。

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**図 4**: YSOD エラーの詳細を表示  
([フルサイズの画像を表示する をクリックします](logging-error-details-with-elmah-vb/_static/image10.png))。

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**図 5**: エラーの発生時にサーバー変数のコレクションの値を調べる  
([フルサイズの画像を表示する をクリックします](logging-error-details-with-elmah-vb/_static/image13.png))。

ELMAH を実稼働 web サイトにデプロイする必要があります。

- コピー、`Elmah.dll`ファイルを`Bin`運用上のフォルダー
- ELMAH に固有の構成設定をコピー、 `Web.config` 、運用環境で使用されるファイルと
- 実稼働データベースには、エラー ログのソースのインフラストラクチャを追加します。

開発から実稼働前のチュートリアルにファイルをコピーするための技術紹介しました。 SQL Server Management Studio を使用して、実稼働データベースに接続し、実行する、実稼働データベースでエラー ログのソースのインフラストラクチャを取得する最も簡単な方法は、おそらく、`SqlServer.sql`必要なテーブルが作成され、格納されているスクリプト ファイル手順です。

### <a name="viewing-the-error-details-page-on-production"></a>運用環境でエラーの詳細ページを表示します。

サイトを運用環境をデプロイした後実稼働 web サイトにアクセスし、未処理の例外を生成します。 開発環境のように ELMAH に影響しませんが、未処理の例外が発生したときに表示されるエラー ページ代わりに、単なるエラーが記録されます。 エラー ログのページにアクセスしようとした場合 (`elmah.axd`)、運用環境から出てきますに示すように許可されていません ページで**図 6**します。

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**図 6**: 既定では、リモートの訪問者が、エラー ログの Web ページを表示できません  
([フルサイズの画像を表示する をクリックします](logging-error-details-with-elmah-vb/_static/image16.png))。

ELMAH 構成の取り消しを`<security>`設定セクション、`allowRemoteAccess`属性を 0 で、リモート ユーザーがエラー ログを表示することを禁止します。 匿名の訪問者から、エラー ログの表示を禁止するように、エラーの詳細は、セキュリティの脆弱性やその他の機密情報を表示可能性がありますに重要です。 この属性を 1 に設定し、エラー ログへのリモート アクセスを有効にするかどうかは、ロック ダウンすることが重要、`elmah.axd`パスため、承認されたユーザーのみのがアクセスできます。 追加することでこれを実現する、`<location>`要素を`Web.config`ファイル。

次の構成は、エラー ログの web ページにアクセスする管理者の役割でのユーザーのみを許可します。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> 管理者ロールと Scott、Jisun、Alice のシステムでは、3 人のユーザーが追加された、 [*サービスを構成する、web サイトを使用してアプリケーション*チュートリアル](configuring-a-website-that-uses-application-services-vb.md)します。 ユーザー Scott と Jisun 管理者ロールのメンバーであります。 認証と承認の詳細についてを参照してください、 [web サイトのセキュリティのチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)します。


リモート ユーザーによって実稼働環境でのエラー ログを表示できます。再び参照**図 3**、 **4**、および**5**のエラー ログの web ページのスクリーン ショット。 ただし、ユーザーが匿名または非管理者が、エラー ログ ページを表示しようとしています。 自動的にリダイレクトされますログイン ページに (`Login.aspx`)、として**図 7**を示しています。

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**図 7**: 承認されていないユーザーがログイン ページに自動的にリダイレクトされます  
([フルサイズの画像を表示する をクリックします](logging-error-details-with-elmah-vb/_static/image19.png))。

### <a name="programmatically-logging-errors"></a>プログラムでエラーのログ記録

ELMAH の`ErrorLogModule`HTTP モジュールが、指定されたログ ソースに自動的に未処理の例外を記録します。 また、エラーをログインを使用して、未処理の例外を発生させることがなく、`ErrorSignal`クラスとその`Raise`メソッド。 `Raise`メソッドに渡されますが、`Exception`オブジェクトし、その例外がスローされた、処理されることがなく、ASP.NET ランタイムに達し場合とログに記録します。 要求が通常は、後の実行を続行する、違いは、`Raise`スローされた、ハンドルされない例外、要求の通常の実行を中断し、構成を表示する ASP.NET ランタイムはメソッドが呼び出されましたエラー ページ。

`ErrorSignal`クラスは何らかのアクションが失敗する可能性がありますが、エラーは、全体的な操作を実行中に致命的ではない場合に便利です。 たとえば、web サイトがユーザーの入力を受け取り、データベースに格納およびユーザーに知らせる電子メールを送信するフォームを含めることができます、その情報の処理します。 情報が正常のデータベースに保存されますが、電子メール メッセージを送信するときにエラーがある場合の動作ですか。 1 つのオプションは、例外をスローし、エラー ページに、ユーザーに送信することです。 ただし、これに入力した情報は保存されませんでしたユーザーが混乱する可能性があります。 任意の方法でユーザーのエクスペリエンスは変更を電子メールに関連するエラーをログに別の方法があります。 これは、ような場合、`ErrorSignal`クラスは便利です。

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>エラーの通知を電子メールで

データベース エラーのログ記録、と共に ELMAH をエラーの詳細を指定した受信者に電子メールを構成こともできます。 この機能によって提供されます、 `ErrorMailModule` HTTP モジュールです。 そのため、この HTTP モジュールを登録する必要があります`Web.config`エラーの詳細を電子メールで送信するためにします。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

次に、エラーの電子メールに関する情報の指定、`<elmah>`要素の`<errorMail>` セクションで、電子メールの送信者と受信者、件名、ことを示す電子メールが非同期的に送信するかどうかと。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

上記の設定に使用されるたびにランタイム エラーが発生するメールを送信して ELMAHsupport@example.comとエラーの詳細。 ELMAH のエラー電子メールにはエラー メッセージ、スタック トレース、およびサーバー変数 namely のエラー詳細 web ページに表示される同じ情報が含まれています (参照**図 4**と**5**)。 エラーの電子メールに添付ファイルとして例外詳細死亡黄色い画面のコンテンツも含まれています (`YSOD.html`)。

**図 8** ELMAH のエラーの電子メールにアクセスして生成されたを示しています。`Genre.aspx?ID=foo`します。 中に**図 8**サーバー変数が含まれていますさらに下へと電子メールの本文でのみ、エラー メッセージやスタック トレースを示しています。

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**図 8**: 電子メールでエラーの詳細を送信する ELMAH を構成します。  
([フルサイズの画像を表示する をクリックします](logging-error-details-with-elmah-vb/_static/image22.png))。

## <a name="only-logging-errors-of-interest"></a>関心のあるエラーをログ記録のみ

既定では、ELMAH は、404 およびその他の HTTP エラーなど、すべてのハンドルされない例外の詳細を記録します。 ELMAH これらまたは他の種類のエラーをフィルター処理を使用してエラーを無視するように指示できます。 フィルター処理のロジックは、ELMAH のによって実行`ErrorFilterModule`HTTP モジュールに登録する必要があります`Web.config`フィルタ リング ロジックを使用するためにします。 フィルター処理規則がで指定された、`<errorFilter>`セクション。

次のマークアップでは、ELMAH いない 404 エラーを記録するように指示します。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> エラーは、登録する必要がありますをフィルター処理を使用するには、導入、 `ErrorFilterModule` HTTP モジュールです。


`<equal>`内の要素、`<test>`セクションはアサーションと呼ばれます。 アサーションが true に評価される場合は、ELMAH のログからエラーがフィルター処理します。 その他のアサーション利用可能: `<greater>`、 `<greater-or-equal>`、 `<not-equal>`、 `<lesser>`、`<lesser-or-equal>`など。 使用してアサーションを組み合わせることも、`<and>`と`<or>`ブール演算子。 さらに、でも、アサーションとして単純な JavaScript 式を含めるしたり、独自のアサーションを c# または Visual Basic で記述できます。

ELMAH のエラーをフィルタ リング機能の詳細についてを参照してください、[セクションのエラーをフィルター処理](https://code.google.com/p/elmah/wiki/ErrorFiltering)で、 [ELMAH wiki](https://code.google.com/p/elmah/w/list)します。

## <a name="summary"></a>まとめ

ELMAH は、ASP.NET web アプリケーションのエラーを記録するためのシンプルでありながら強力なメカニズムを提供します。 同様に Microsoft の正常性の監視システムでは、ELMAH のエラーをログをデータベースに、エラーの詳細を電子メールでの開発者に送信できます。 エラー ログのデータ ストアなどの広い範囲のボックスのサポート対象外システムを監視、正常性とは異なり ELMAH が含まれています: Microsoft SQL Server、Microsoft Access、Oracle、XML ファイル、およびその他のいくつか。 ELMAH でエラー ログと、web ページから特定のエラーの詳細を表示するための組み込みのメカニズムを提供するさらに、`elmah.axd`します。 `elmah.axd`ページは、RSS フィード、または Microsoft Excel を使用して確認できるコンマ区切り値ファイル (CSV) としてのエラー情報を表示できますも。 宣言型またはプログラムによるアサーションを使用して、ログからエラーをフィルタ ELMAH を指示することもできます。 ELMAH は ASP.NET バージョン 1.x アプリケーションで使用できます。

すべてのデプロイされたアプリケーションは、未処理の例外を自動的にログ記録と開発チームに通知を送信するメカニズムを備えている必要があります。 かどうかは、これを正常性の監視または ELMAH を使用して、セカンダリ。 つまり、本当に関係ありません正常性の監視と ELMAH; を使用するかはるか両方のシステムを評価し、ニーズに最適な 1 つを選択します。 運用環境で未処理の例外をログにいくつかのメカニズムが配置されるは、根本的に重要です。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ELMAH でエラー Logging Modules and Handlers](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [ELMAH プロジェクト ページ](https://code.google.com/p/elmah/)(ソース コード、サンプル、wiki)
- [ELMAH に Web アプリケーションを未処理の例外をキャッチするプラグイン](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx)(ビデオ)
- [セキュリティ エラーのログのページ](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [HTTP モジュールとハンドラーを使用して、プラグ可能な ASP.NET コンポーネントを作成するには](https://msdn.microsoft.com/library/aa479332.aspx)
- [Web サイトのセキュリティのチュートリアル](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [前へ](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [次へ](precompiling-your-website-vb.md)
