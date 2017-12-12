---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: "Visual Studio を使用して ASP.NET Web 配置: テストへの展開 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 97910940f9de26ca71b111b945581d2de6650b02
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Visual Studio を使用して ASP.NET Web 配置: テストへの展開
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでは、ローカル コンピューターの IIS に ASP.NET web アプリケーションを展開する方法を示します。

アプリケーションを開発するときに、Visual Studio で実行して、通常でテストします。 既定では、Visual Studio 2012 で web アプリケーション プロジェクトを使用して IIS Express 開発 web サーバーとします。 Visual Studio 開発サーバー (Cassini とも呼ばれます) を既定で Visual Studio 2010 を使用するよりも完全な IIS のように、IIS Express がよりは動作します。 いますが、どちらも開発 web サーバーが IIS と同じように動作します。 その結果、可能であれば、アプリケーションが、Visual Studio で、テストが IIS に展開すると失敗する場合に、正常に実行されます。

次の方法でより信頼性の高いアプリケーションをテストすることができます。

1. 開発用コンピューター上の IIS にアプリケーションを配置するには、後で、実稼働環境に展開に使用するのと同じプロセスを使用します。 Web プロジェクトを実行するが、展開プロセスをいないテストはこれを行うときに、IIS を使用する Visual Studio を構成することができます。 このメソッドは、IIS 下で、アプリケーションが正しく実行するだけでなく展開プロセスを検証します。
2. 実稼働環境とほぼ同じであるテスト環境へのアプリケーションを展開します。 これらのチュートリアルの運用環境が Azure App service Web アプリのため、理想的なテスト環境は、Azure App Service で作成された追加の web アプリです。 この 2 つ目の web アプリを使用して、テスト目的でのみはように設定して運用 web アプリと同じ方法。

オプション 2 をテストする最も確実な方法は、する場合がないと必ずしもはオプション 1 にします。 ただしを展開している場合、サード パーティ製ホスティング プロバイダー オプション 2 することは不可能またはため、このチュートリアルのシリーズが両方の方法を示していますが高くなる可能性があります。 オプション 2 のガイダンスがで提供される、[実稼働環境に展開する](deploying-to-production.md)チュートリアルです。

Visual Studio で web サーバーの使用の詳細については、次を参照してください。 [ASP.NET Web プロジェクト用の Visual Studio で Web サーバー](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx)です。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](troubleshooting.md)です。

## <a name="install-iis"></a>IIS をインストールします。

開発用コンピューター上の IIS に展開するには IIS が必要し、Web Deploy をインストールします。 既定では、Visual Studio で web Deploy がインストールされますが、IIS の既定の Windows 8 または Windows 7 の構成には含まれません。 IIS が既にインストールされている既定のアプリケーション プールが既に .NET 4 に設定されている場合に進みます[、次のセクション](#sqlexpress)です。

1. 使用して、 [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) Web Platform Installer が IIS の推奨構成をインストールし、IIS と Web の前提条件が自動的にインストールするために、IIS および Web Deploy をインストールすることをお勧めは、必要に応じてを展開します。

    IIS および Web Deploy をインストールする Web Platform Installer を実行するには、次のリンクを使用します。 既にをインストールした IIS、Web デプロイまたはその必須コンポーネントのいずれかの場合、Web Platform Installer は、何が足りないのみをインストールします。

    - [IIS および Web Deploy WebPI を使用したインストールします。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

    IIS 7 をインストールすることを示すメッセージが表示されます。 リンクが機能の Windows 8 では Windows 8、IIS 8 の次の手順を実行して ASP.NET 4.5 がインストールされていることを確認します。

    1. 開いている**コントロール パネルの** 、**プログラムと機能**、 **Windows の機能のオンまたはオフ**です。
    2. 展開**インターネット インフォメーション サービス**、 **World Wide Web サービス**、および**アプリケーション開発機能**します。
    3. 確認して**ASP.NET 4.5**が選択されています。

        ![ASP.NET 4.5 を選択します。](deploying-to-iis/_static/image1.png)

IIS をインストールすると、実行**IIS マネージャー** .NET Framework version 4 が既定のアプリケーション プールに割り当てられているかどうかを確認します。

1. 開くには、WINDOWS R キーを押して、**実行** ダイアログ ボックス。

    (または Windows 8 では入力"run"、**開始**ページ、または Windows 7 では次のように選択します。**実行**から、**開始**メニュー。 場合**実行**でないが、**開始**メニューのタスク バーを右クリックし、[**プロパティ**を選択、 **[スタート] メニュー** ] タブで、[ ]をクリックして**カスタマイズ**を選択して**コマンドを実行**)。
2. "Inetmgr"を入力し、クリックして**OK**です。
3. **接続** ウィンドウで、サーバー ノードを展開し、選択**アプリケーション プール**です。 **アプリケーション プール** ウィンドウで場合、 **DefaultAppPool**に .NET framework バージョン 4、次の図のように割り当てると、次のセクションにスキップします。

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. 2 つのアプリケーション プールを参照してください、.NET Framework 2.0 に設定されます両方の場合は、IIS で ASP.NET 4 をインストールする必要があります。

    Windows 8 セクションことを確認する ASP.NET 4.5 がインストールされている、またはを参照してください、前の手順を参照して[このサポート技術情報記事](https://support.microsoft.com/kb/2736284)です。 Windows 7 を右クリックして、コマンド プロンプト ウィンドウを開く**コマンド プロンプト**windows**開始**メニューを選択して**管理者として実行**です。 実行して[aspnet\_regiis.exe](https://msdn.microsoft.com/en-us/library/k6h9cz8h.aspx)を次のコマンドを使用して、IIS で ASP.NET 4 をインストールします。 (32 ビット システムでは、"Framework"と"Framework64"を置き換えてください)。

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    このコマンドは、.NET Framework 4 の新しいアプリケーション プールを作成しますが、2.0 にも既定のアプリケーション プールは設定されます。 デプロイするアプリケーションを対象とする .NET 4 そのアプリケーション プールに .NET 4 にアプリケーション プールを変更する必要があるためです。
5. 閉じた場合**IIS マネージャー**、もう一度実行、サーバー ノードを展開およびクリックして**アプリケーション プール**を表示する、**アプリケーション プール**ペインをもう一度です。
6. **アプリケーション プール** ウィンドウで、をクリックして**DefaultAppPool**、し、、**アクション**ペイン をクリック**基本設定**です。

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. **アプリケーション プールの編集** ダイアログ ボックスで、変更**.NET Framework のバージョン**に**.NET Framework v4.0.30319**  をクリック**OK**です。

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS は、web アプリケーションを発行するための準備ができましたが、テスト環境で使用するデータベースを作成する必要が前に、行うことができます。

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>SQL Server Express をインストールします。

LocalDB は、SQL Server Express をインストールしておく必要が、テスト環境のために IIS では、作業に設計されていません。 Visual Studio 2010 SQL Server Express を使用している場合は、既定では既にインストールします。 Visual Studio 2012 を使用している場合は、インストールする必要です。

SQL Server Express をインストールするインストールから[ダウンロード センター: Microsoft SQL Server 2012 Express](https://www.microsoft.com/en-us/download/details.aspx?id=29062)  をクリックして[ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe)または[ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe)です。 問題の 1 つを選択した場合、システムのインストールに失敗し、もう 1 つを試みることができます。

SQL Server インストール センターの最初のページで、をクリックして**SQL Server の新規スタンドアロン インストールまたは既存のインストールに機能の追加**デフォルトの選択を受け入れ、指示に従います。 インストール ウィザードでは、既定の設定をそのまま使用します。 インストール オプションの詳細については、次を参照してください。[インストール ウィザード (セットアップ) からの SQL Server 2012 のインストール](https://msdn.microsoft.com/en-us/library/ms143219.aspx)です。

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>テスト環境の SQL Server Express データベースを作成します。

Contoso 大学アプリケーションが 2 つのデータベース: メンバーシップ データベースとアプリケーション データベース。 2 つの異なるデータベースまたは 1 つのデータベースには、これらのデータベースを展開できます。 アプリケーションのデータベースと、メンバーシップ データベース間の結合がデータベースを容易にするために結合する可能性があります。 サード パーティのホスティング プロバイダーに展開する場合は、ホスティング プランするときに、それらを結合するための可能性がありますも提供します。 ホスティング プロバイダーの複数のデータベースの課金する場合がありますなど、複数のデータベースをも許可しない可能性があります。

このチュートリアルでは、テスト環境で 2 つのデータベースとステージングと運用環境で 1 つのデータベースを展開します。

**ビュー**で Visual Studio を選択 メニューの **サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Visual Web Developer で)、し、右クリックし、**データ接続**選択**新しい SQL Server データベースの作成**です。

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

**新しい SQL Server データベースの作成** ダイアログ ボックスで、入力". \SQLExpress"で、**サーバー名**ボックスと"aspnet ContosoUniversity"で、**新しいデータベース名**ボックスし、をクリックして**OK**です。

![Aspnet ContosoUniversity を作成します。](deploying-to-iis/_static/image9.png)

"ContosoUniversity"という名前の新しい SQL Server Express の School データベースを作成する同じ手順に従います。

**サーバー エクスプ ローラー** 2 つの新しいデータベースが表示されます。

![サーバー エクスプ ローラーで新しいデータベース](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Grant スクリプト、新しいデータベースを作成します。

アプリケーションを開発用コンピューターに IIS で実行すると、アプリケーションは既定のアプリケーション プールの資格情報を使用して、データベースにアクセスします。 ただし、既定では、アプリケーション プール id は、データベースを開くアクセス許可がありません。 ように権限を付与するスクリプトを実行する必要があります。 このセクションでは、後で、アプリケーションも、IIS での実行時に、データベースを開くことができるかどうかを確認を実行しますスクリプトを作成します。

(いない 1 つのプロジェクトの)、ソリューションを右クリックし、をクリックして**新しい項目の追加**、し、作成、新しい**SQL ファイル**という*Grant.sql*です。 ファイルに次の SQL コマンドをコピーおよび保存し、ファイルを閉じます。

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> このスクリプトは、このチュートリアルでは指定されているように、SQL Server Express 2012 と Windows 8 または Windows 7 で IIS 設定を操作する設計されています。 または、Windows の SQL Server の別のバージョンを使用している場合、または IIS を設定するコンピューターに異なる場合は、次のスクリプトへの変更が必要があります。 SQL Server スクリプトの詳細については、次を参照してください。 [SQL Server オンライン ブック](https://go.microsoft.com/fwlink/?LinkId=132511)です。


> [!NOTE] 
> 
> **セキュリティに関する注意**このスクリプトは、db\_、実稼働環境で必要がありますが、実行時に、データベースにアクセスするユーザーを所有者のアクセス許可。 一部のシナリオでのみ展開については、アクセス許可を更新し、実行時のデータを読み書きするのみのアクセス許可を持つ別のユーザーを指定します。 完全なデータベース スキーマを持つユーザーを指定することができます。 詳細については、次を参照してください。 [Code First Migrations を自動の Web.config の変更を確認](#reviewingmigrations)このチュートリアルで後述します。


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>アプリケーション データベースで許可スクリプトを実行します。

スクリプトを実行する、grant、メンバーシップ データベース内の展開時にそのデータベースの配置が dbDacFx プロバイダーを使用するため、発行プロファイルを構成することができます。 これは、アプリケーション データベースを配置する方法、Code First Migrations 展開時にスクリプトを実行することはできません。 そのため、アプリケーション データベースに手動で展開する前にスクリプトを実行する必要があります。

1. Visual Studio で開く、 *Grant.sql*先ほど作成したファイルです。
2. **[接続]**をクリックします。 

    ![[接続] ボタン](deploying-to-iis/_static/image11.png)
3. **サーバーへの接続** ダイアログ ボックスに、入力*. \SQLExpress*として、**サーバー名**、クリックして**接続**です。
4. データベースのボックスの一覧で選択**ContosoUniversity**、クリックして**Execute**です。 

    ![](deploying-to-iis/_static/image12.png)

既定のアプリケーション プール id で、Code First Migrations をアプリケーションの実行時に、データベース テーブルを作成するため、アプリケーション データベースに十分なアクセス許可できるようになりました。

## <a name="publish-to-iis"></a>IIS への公開します。

いくつかの方法が Visual Studio および Web Deploy を使用して IIS に展開することができます。

- ワンクリック発行 Visual Studio を使用します。
- コマンドラインから発行します。
- 作成、*展開パッケージ*し、IIS マネージャーの UI を使用してインストールします。 展開パッケージの構成要素、 *.zip*をすべてのファイルと IIS のサイトのインストールに必要なメタデータを含むファイルです。
- 展開パッケージを作成し、コマンドラインを使用してインストールします。

プロセスを経てきましたを自動化する Visual Studio を設定する前のチュートリアルでの展開タスクがこれらのメソッドのすべてに適用されます。 これらのチュートリアルでは、これらのメソッドの最初の 2 つを使用します。 展開パッケージを使用する方法の詳細については、次を参照してください。[作成し、web 配置パッケージをインストールした web アプリケーションの配置](https://go.microsoft.com/fwlink/p/?LinkId=282413#package)で Visual Studio と ASP.NET の Web 展開のコンテンツ マップします。

発行前に、管理者モードで Visual Studio を実行していることを確認します。 表示されない場合**(管理者)**タイトル バーで、Visual Studio を閉じます。 Windows 8 で**開始**ページまたは Windows 7**開始**メニューで、ご使用の Visual Studio のバージョンのアイコンを右クリックして**管理者として実行**です。 管理者モードでは、のみときにパブリッシュする IIS、ローカル コンピューター上の発行に必要です。

### <a name="create-the-publish-profile"></a>発行プロファイルを作成します。

1. **ソリューション エクスプ ローラー**ContosoUniversity プロジェクト (ContosoUniversity.DAL プロジェクトではない) を右クリックし、選択、**発行**です。

    **Web の発行**ウィザードが表示されます。

    ![公開 Web ウィザード プロファイル タブ](deploying-to-iis/_static/image13.png)
2. ドロップダウン リストで選択**&lt;新規しています.&gt;**. (最新 Visual Studio 更新プログラムをインストール、ドロップダウン リストがないと、最初から新しいプロファイルを作成するためにをクリックするボタンは**カスタム**)。
3. **新しいプロファイル**ダイアログ ボックスで、"Test"を入力し、をクリックして**OK**です。

    ウィザードが自動的に進みます、**接続**タブです。
4. **サービス URL**ボックスに、入力*localhost*です。
5. **サイト/アプリケーション**ボックスに、入力*既定の Web サイト/ContosoUniversity*
6. **送信先 URL**ボックスに、入力`http://localhost/ContosoUniversity`

    **送信先 URL**設定は必要ありません。 Visual Studio では、アプリケーションの展開が完了すると、自動的にこの URL に、既定のブラウザーを開きます。 展開後に自動的に開く、ブラウザーがたくない場合は、このボックスを空白のままにします。
7. をクリックして**接続の検証**設定が正しいことと、ローカル コンピューター上の IIS に接続できることを確認します。

    緑のチェック マークは、接続が成功したことを確認します。

    ![公開 Web ウィザードの [接続] タブ](deploying-to-iis/_static/image14.png)
8. をクリックして**次**に進める、**設定**タブです。
9. **構成** ボックスの一覧を展開するビルド構成を指定します。 リリースの既定値に設定したままにします。 このチュートリアルではデバッグ ビルドが配置されません。
10. 展開**ファイルの発行オプション**、し、**アプリからファイルを除外する\_データ フォルダー**です。

    .Mdf ファイルではないファイルをアプリケーションがローカルの SQL Server Express のインスタンスで作成したデータベースにアクセスをテスト環境で、*アプリ\_データ*フォルダーです。
11. ままにして、**発行時にプリコンパイル**と**先で追加ファイルを削除する**チェック ボックスをオフします。

    ![[設定] タブでファイルの発行オプション](deploying-to-iis/_static/image15.png)

    主に非常に大規模なサイトのために役立つオプションは、プリコンパイル最初に、サイトをパブリッシュした後、ページが要求されたページの開始時間の短縮、ことができます。

    これは、最初の展開で、されません、ファイルが存在する、対象フォルダーにまだ追加のファイルを削除する必要はありません。

    > [!NOTE] 
    > 
    > [!CAUTION]
    > 選択した場合**追加ファイルを削除する**後続の同じサイトに展開を表示するには、表示されるように事前に展開する前に削除するファイルは、プレビュー機能を使用することを確認します。 動作は、Web Deploy は、プロジェクトの削除が移行先サーバーでファイルが削除されます。 ただし、フォルダー構造全体元とコピー先フォルダー の下の比較、および一部のシナリオで Web Deploy 可能性があります削除しないファイルを削除します。
    > 
    > たとえば、ルート フォルダーにプロジェクトを配置するときに、サーバー上のサブフォルダーに、web アプリケーションをした場合、サブフォルダーは削除されます。 Contoso.com でメイン サイトの 1 つのプロジェクトと contoso.com/blog でブログの別のプロジェクトがあります。 サブフォルダーには、ブログ アプリケーションです。 メイン サイトを展開するときに転送先に削除する追加のファイルを選択した場合は、ブログ アプリケーションが削除されます。
    > 
    > 別の例では、アプリの\_データ フォルダーが予期せず削除可能性があります。 SQL Server Compact などの特定のデータベースでは、アプリのデータベース ファイルを格納\_データ フォルダーです。 初期展開後を除外するアプリを選択するため、後続のデプロイでデータベース ファイルのコピーを保持したくない\_Web のパッケージ化/発行 タブ上のデータ。選択した場所にあるその他のファイルを削除、データベース ファイルと、アプリがある場合、実行後\_次回の公開に、データ フォルダー自体が削除されます。

### <a name="configure-deployment-for-the-membership-database"></a>メンバーシップ データベースの展開を構成します。

次の手順を適用する、 **DefaultConnection**データベースに格納されて、**データベース** ダイアログ ボックスのセクションです。

1. **リモート接続文字列**ボックスに、新しい SQL Server Express メンバーシップ データベースを指す次の接続文字列を入力します。

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    展開プロセスは、ために、展開済みの Web.config ファイルのこの接続文字列を格納する**実行時にこの接続文字列を使用して**が選択されています。

    接続文字列を取得することもできます。**サーバー エクスプ ローラー**です。 **サーバー エクスプ ローラー**、展開**データ接続**を選択し、  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity**データベース、してから、**プロパティ**ウィンドウのコピー、**接続文字列**値。 接続文字列が削除できる 1 つの追加設定を持つ:`Pooling=False`です。
2. 選択**更新データベース**です。

    これにより、データベース スキーマの展開時に転送先データベースに作成されます。 次の手順で実行する必要のある追加のスクリプトを指定する: 既定のアプリケーション プールとデータを展開する 1 つのデータベース アクセスを許可する 1 つです。
3. をクリックして**データベース更新を構成する**です。
4. **データベース更新を構成する**ダイアログ ボックスで、をクリックして**SQL スクリプトの追加**に移動し、 *Grant.sql*ソリューション フォルダーに保存したスクリプト。
5. 追加手順を繰り返して、 *aspnet データ-dev.sql*スクリプト。

    ![メンバーシップ データベースのデータベースの更新を構成します。](deploying-to-iis/_static/image16.png)
6. **[閉じる]**をクリックします。

### <a name="configure-deployment-for-the-application-database"></a>アプリケーション データベースの展開を構成します。

Visual Studio で Entity Framework が検出された場合`DbContext`内のエントリを作成、クラス、**データベース**セクションを**Code First Migrations を実行**チェック ボックスをオンの代わりに、 **データベースを更新する**チェック ボックスをオンします。 このチュートリアルでは、Code First Migrations デプロイを指定するのに、このチェック ボックスを使用します。

一部のシナリオで使用している、`DbContext`データベース場合でも、移行ではなく dbDacFx プロバイダーを使用して、データベースを展開します。 その場合を参照してください[移行なしの Code First のデータベースの配置方法?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#deploy_code_first_without_migrations) MSDN で ASP.NET Web 展開 FAQ にします。

次の手順を適用する、 **SchoolContext**データベースに格納されて、**データベース** ダイアログ ボックスのセクションです。

1. **リモート接続文字列**ボックスに、新しい SQL Server Express のアプリケーション データベースを指している次の接続文字列を入力します。

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    展開プロセスは、ために、展開済みの Web.config ファイルのこの接続文字列を格納する**実行時にこの接続文字列を使用して**が選択されています。

    アプリケーションのデータベース接続文字列を取得することもできます。**サーバー エクスプ ローラー**同様のメンバーシップを取得したデータベース接続文字列。
2. 選択**実行 Code First Migrations (アプリケーション開始時に実行されます)**です。

    このオプションの設定により、展開プロセスを指定する展開済みの Web.config ファイルを構成するのには、`MigrateDatabaseToLatestVersion`初期化子。 この初期化子を使用最新バージョンにアプリケーションの展開後に最初に、データベースにアクセスするときのデータベースが自動的に更新します。

### <a name="configure-publish-profile-transforms"></a>構成プロファイルの変換の発行

1. をクリックして**閉じる**、順にクリック**はい**と表示されたら変更を保存するかどうか。
2. **ソリューション エクスプ ローラー**、展開**プロパティ**、展開**PublishProfiles**です。
3. Rright クリック*Test.pubxml、*  をクリックし、 **Config 変換の追加**です。

    ![追加の構成変換メニュー](deploying-to-iis/_static/image17.png)

    Visual Studio によって作成、 *Web.Test.config*変換ファイルを開きます。
4. *Web.Test.config*変換ファイルが、開始直後に次のコードを挿入構成タグ。

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    テストを使用する場合は、発行プロファイルをこの変換が"Test"に、環境のインジケーターを設定します。 配置済みのサイトで、「Contoso 大学」H1 見出し"(Test)"が表示されます。
5. ファイルを保存して閉じます。
6. 右クリックし、 *Web.Test.config*ファイルし、をクリックして**プレビュー変換**ようにコーディングした変換が必要な変更を生成するかどうかを確認します。

    **Web.config プレビュー**ウィンドウは、両方に適用した結果を示しています、 *Web.Release.config*変換と*Web.Test.config*を変換します。

### <a name="preview-the-deployment-updates"></a>展開の更新プログラムをプレビューします。

1. 開く、 **Web の発行**ウィザードをもう一度 (ContosoUniversity プロジェクトを右クリックし、をクリックして**発行**)。
2. **プレビュー**  タブで、ことを確認して、**テスト**プロファイルは、クリックしてオンのまま**開始プレビュー**コピーされるファイルの一覧を表示します。

    ![[プレビュー] ボタン](deploying-to-iis/_static/image18.png)

    ![プレビューを公開します。](deploying-to-iis/_static/image19.png)

    クリックすることも、**プレビューのデータベース**メンバーシップ データベースで実行されるスクリプトを表示するリンクです。 (スクリプトは実行されません Code First Migrations 展開のため、アプリケーション データベースをプレビューするものはありません。)
3. **[発行]**をクリックします。

    Visual Studio が管理者モードでない場合は、アクセス許可エラーを示すエラー メッセージを取得可能性があります。 その場合は、Visual Studio を閉じます、管理者モードで開き、再度発行します。

    Visual Studio は、管理者モードの場合、**出力**ウィンドウのレポートが正常にビルドし、発行します。

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    内の URL を入力した場合、**送信先 URL**ボックス、発行プロファイルを**接続** タブがブラウザーは自動的に、ローカル コンピューター上の IIS で実行されている Contoso 大学のホーム ページに表示されます。

## <a name="test-in-the-test-environment"></a>テスト環境でのテストします。

環境のインジケーターは、"(Test)"を表示"(Dev)"ではなくなることを示しています、 *Web.config*環境のインジケーターの変換が成功しました。

実行、**講習においてインストラクター**ページを Code First シード インストラクター データとデータベースを確認します。 このページを選択すると、Code First、データベースを作成してから実行するための読み込みにしばらく時間がかかる場合があります、`Seed`メソッドです。 (そのようになっていませんため、まだデータベースにアクセスしようとしていないアプリケーション ホーム ページにあった場合。)

クリックして、**受講者** タブに配置されるデータベースに受講者がいないことを確認します。

選択**受講者を追加**から、**受講者** メニューの 、受講者を追加し、新しい学生を表示、**受講者**データベースに正常に記述できることを確認する ページ.

**コース**メニューの **更新クレジット**です。 **更新クレジット**ページには、管理者のアクセス許可が必要がありますので、**ログで**ページが表示されます。 以前 ("admin"および"devpwd") で作成した管理者アカウントの資格情報を入力します。 **更新クレジット**ページが表示されたら、前のチュートリアルで作成した管理者アカウントが、テスト環境に正しく展開されたことを確認します。

いることを確認、 *Elmah*フォルダーに存在、 *c:\inetpub\wwwroot\ContosoUniversity*にプレース ホルダー ファイルのみを持つフォルダーです。

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First Migrations を自動の Web.config の変更を確認します。

開く、 *Web.config*に展開されたアプリケーション内でファイル*C:\inetpub\wwwroot\ContosoUniversity*し、ここで、展開プロセスを Code First Migrations が自動的に構成を参照してください最新バージョンにデータベースを更新します。

![](deploying-to-iis/_static/image21.png)

展開プロセスには、Code First Migrations データベース スキーマの更新をのみ使用するための新しい接続文字列も作成されます。

![Database_Publish 接続文字列](deploying-to-iis/_static/image22.png)

この追加の接続文字列では、データベース スキーマの更新プログラムの 1 つのユーザー アカウントとアプリケーション データへのアクセスを別のユーザー アカウントを指定することができます。 割り当てますなど、 **db\_所有者**Code First Migrations をするにはロールおよび**db\_datareader**と**db\_datawriter**をアプリケーション ロールです。 これは、悪意のあるアプリケーションのコードで、データベース スキーマを変更するを防止する一般的な多層防御パターンです。 (たとえば、これで行われる処理 SQL インジェクション攻撃が成功します。)このパターンは、これらのチュートリアルでは使用されません。 シナリオにおけるこのパターンを実装する場合は、次の手順で実行できます。

1. **設定**のタブ、 **Web の発行**ウィザード、完全なデータベース スキーマの更新権限を持つユーザーを指定する接続文字列を入力し、オフ、**この接続文字列を使用実行時に**チェック ボックスをオンします。 これは展開済みの Web.config ファイルで、`DatabasePublish`接続文字列。
2. アプリケーションが実行時に使用する接続文字列の Web.config ファイルの変換を作成します。

## <a name="summary"></a>概要

今すぐ開発用コンピューター上の IIS にアプリケーションを配置してテストしたことがあります。

![テストでのホーム ページ](deploying-to-iis/_static/image23.png)

これは、展開プロセスが、適切な場所 (配置をしないようにするには、ファイルを除く) に、アプリケーションのコンテンツをコピーされもその Web Deploy 構成されている IIS 正しく配置時に確認します。 チュートリアルでは、[次へ]、展開するタスクがまだ行われていないを検索する 1 つの複数のテストを実行します。 上のフォルダーのアクセス許可の設定、 *Elmah*フォルダーです。

## <a name="more-information"></a>詳細情報

Visual Studio での IIS または IIS Express の実行については、次のリソースを参照してください。

- [IIS Express の概要](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net サイトです。
- [IIS Express の概要](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx)Scott Guthrie のブログです。
- [ASP.NET Web プロジェクトのために、Visual Studio でのサーバーを web](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx)です。
- [コアの相違点の間で IIS と ASP.NET 開発サーバー](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET サイトです。

どのような問題については、中程度の信頼でアプリケーションを実行するときに発生する可能性を参照してください[中程度の信頼で ASP.NET アプリケーションをホストしている](http://www.4guysfromrolla.com/articles/100307-1.aspx)Rolla サイトから 4 Guys にします。

>[!div class="step-by-step"]
[前へ](project-properties.md)
[次へ](setting-folder-permissions.md)
