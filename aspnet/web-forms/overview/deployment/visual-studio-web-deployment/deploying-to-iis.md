---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Visual Studio を使用して ASP.NET Web 配置: テストへのデプロイ |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: riande
ms.date: 03/23/2015
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: ce361d2826733d5ecb9005713993e5ecf7f11860
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826252"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Visual Studio を使用して ASP.NET Web 配置: テストへのデプロイ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、ローカル コンピューターの IIS に ASP.NET web アプリケーションをデプロイする方法を説明します。

アプリケーションを開発するときに、Visual Studio で実行して一般的でテストします。 既定では、Visual Studio 2012 で web アプリケーション プロジェクトを使用して、IIS Express 開発 web サーバーとします。 Visual Studio 開発サーバー (Cassini とも呼ばれます)、既定で Visual Studio 2010 を使用するよりも完全な IIS のように、IIS Express がよりは動作します。 どちらも開発 web サーバーが IIS とまったく同じように動作します。 結果として、アプリケーションが、Visual Studio でテストすることが IIS に展開すると失敗する場合に、正常に実行が可能です。

次の方法でより信頼性の高いアプリケーションをテストすることができます。

1. 後で、運用環境にデプロイに使用するのと同じプロセスを使用して開発用コンピューターに IIS にアプリケーションをデプロイします。 Web プロジェクトを実行するが、これを行う、デプロイ プロセスのテストはときに、IIS を使用する Visual Studio を構成することができます。 このメソッドは、IIS 下で、アプリケーションが正しく実行するだけでなく展開プロセスを検証します。
2. 運用環境とほぼ同じであるテスト環境にアプリケーションをデプロイします。 これらのチュートリアルについては、運用環境は、Azure App Service で Web アプリであるために、理想的なテスト環境は、Azure App Service で作成された追加の web アプリが。 テストのみにこの 2 つ目の web アプリを使用するとしますが、運用 web アプリと同じ方法を設定する必要があります。

オプション 2 をテストする最も確実な方法し、する場合は、オプション 1 を実行する必要はありませんとは限りません。 ただしにデプロイする場合サード パーティ製ホスティング プロバイダー オプション 2 ができない場合もあります。 またはため、このチュートリアル シリーズは、両方の方法を示しています。 が高くなる可能性があります。 オプション 2 のガイダンスがで提供される、[実稼働環境に展開する](deploying-to-production.md)チュートリアル。

Visual Studio で web サーバーの使用に関する詳細については、次を参照してください。 [ASP.NET Web プロジェクト用の Visual Studio で Web サーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx)します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。

## <a name="install-iis"></a>IIS をインストールします。

開発用コンピューターに IIS に展開するには、IIS をいる必要があり、Web Deploy をインストールします。 Web Deploy は、Visual Studio では既定でインストールされますが、IIS が既定の Windows 8 または Windows 7 の構成に含まれていません。 IIS が既にインストールされている場合、既定のアプリケーション プールは、既に .NET 4 に設定をスキップする[、次のセクション](#sqlexpress)します。

1. 使用して、 [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)は、Web Platform Installer は、IIS の推奨される構成をインストールし、IIS と Web の前提条件を自動的にインストールされますので、IIS および Web Deploy をインストールすることをお勧め必要に応じてをデプロイします。

    IIS と Web Deploy をインストールする Web Platform Installer を実行するには、次のリンクを使用します。 既に IIS で Web デプロイまたは、必要なコンポーネントのいずれかをインストールする場合は、不足しているだけ、Web Platform Installer がインストールされます。

   - [IIS および Web Deploy WebPI を使用したインストールします。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     IIS 7 をインストールすることを示すメッセージが表示されます。 リンクの動作が Windows 8 向け Windows 8 で IIS 8 について、次の手順を実行することによって、ASP.NET 4.5 がインストールされていることを確認します。

   - 開いている**コントロール パネルの**、**プログラムと機能**、 **Windows の機能のオンまたはオフ**です。
   - 展開**インターネット インフォメーション サービス**、 **World Wide Web サービス**、および**アプリケーション開発機能**します。
   - 必ず**ASP.NET 4.5**が選択されています。

      ![ASP.NET 4.5 を選択します。](deploying-to-iis/_static/image1.png)

IIS をインストールすると、実行**IIS Manager** .NET Framework version 4 が既定のアプリケーション プールに割り当てられているかどうかを確認します。

1. 開くには、WINDOWS + R キーを押して、**実行** ダイアログ ボックス。

    (Windows 8 を入力して「実行」、**開始**ページ、または Windows 7 で次のように選択します。**実行**から、**開始**メニュー。 場合**実行**に含まれていない、**開始**] メニューの [タスク バーを右クリックし、をクリックして**プロパティ**を選択、 **[スタート] メニュー** ] タブで [ **カスタマイズ**、選び**コマンドを実行して**)。
2. "Inetmgr"を入力し、クリックして**OK**します。
3. **接続**ウィンドウで、サーバー ノードを展開し、選択**アプリケーション プール**します。 **アプリケーション プール**ウィンドウで場合、 **DefaultAppPool**を .NET framework version 4 を次の図のように割り当てられている、次のセクションに進んでください。

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. 2 つだけのアプリケーション プールを参照してくださいし、.NET Framework 2.0 に両方の設定は場合に、IIS で ASP.NET 4 をインストールする必要があります。

    Windows 8 では、セクションは、その ASP.NET 4.5 ようにがインストールされている、またはを参照してください、前の手順をご覧ください。[このサポート技術情報記事](https://support.microsoft.com/kb/2736284)します。 Windows 7 を右クリックし、コマンド プロンプト ウィンドウを開きます。**コマンド プロンプト**、Windows で**開始**メニュー**管理者として実行**します。 実行して[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)次のコマンドを使用して、IIS で ASP.NET 4 をインストールします。 (32 ビット システムでは、「フレームワーク」と"Framework64"を置き換えてください)。

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    このコマンドは、.NET Framework 4 の新しいアプリケーション プールを作成しますが、既定のアプリケーション プールは、2.0 にも設定されます。 展開アプリケーションを対象とする .NET 4 をそのアプリケーション プールを .NET 4 アプリケーション プールを変更する必要があるためです。
5. 閉じた場合**IIS Manager**、もう一度実行、サーバー ノードを展開およびクリックして**アプリケーション プール**を表示する、**アプリケーション プール**ペインをもう一度です。
6. **アプリケーション プール**ウィンドウで、をクリックして**DefaultAppPool**、し、**アクション**ペイン をクリックします**基本設定**します。

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. **アプリケーション プールの編集** ダイアログ ボックスで、変更 **.NET Framework のバージョン**に **.NET Framework v4.0.30319**  をクリック**OK**します。

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS は、web アプリケーションを発行するための準備ができましたが、テスト環境で使用するデータベースを作成する必要が前に、行うことができます。

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>SQL Server Express をインストールします。

LocalDB は、テスト環境用に SQL Server Express をインストールする必要があります。 IIS では、動作に設計されていません。 Visual Studio 2010 SQL Server Express を使用している場合は、既定でインストールが既に。 Visual Studio 2012 を使用している場合は、インストールする必要です。

SQL Server Express をインストールするインストールから[ダウンロード センター: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062)をクリックして[ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe)または[ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe)します。 問題の 1 つを選択した場合、システムのインストールに失敗し、もう 1 つを試みることができます。

SQL Server インストール センターの最初のページで次のようにクリックします。 **SQL Server の新規スタンドアロン インストールまたは既存のインストールに機能の追加**、既定の選択を受け入れ、指示に従います。 インストール ウィザードでは、既定の設定をそのまま使用します。 インストール オプションの詳細については、次を参照してください。[インストール ウィザード (セットアップ) から SQL Server 2012 のインストール](https://msdn.microsoft.com/library/ms143219.aspx)します。

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>テスト環境用の SQL Server Express データベースを作成します。

Contoso University アプリケーションが 2 つのデータベース: メンバーシップ データベースとアプリケーション データベース。 2 つの異なるデータベースまたは単一のデータベースには、これらのデータベースをデプロイできます。 アプリケーションのデータベースと、メンバーシップ データベース間のデータベースの結合を容易にするため、それらを結合する可能性があります。 サード パーティのホスティング プロバイダーに展開する場合は、ホスティング プランするときに、それらを結合するための可能性がありますも提供します。 など、ホスティング プロバイダーは、複数のデータベースの詳細は課金可能性があります。 または 1 つ以上のデータベースをも許可しない場合があります。

このチュートリアルでは、テスト環境で 2 つのデータベースと、ステージングと運用環境で 1 つデータベースをデプロイします。

**ビュー**で Visual Studio を選択します] メニューの [**サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Visual Web Developer で)、右クリックし、**データ接続**選択**新しい SQL Server データベースの作成**です。

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

**新しい SQL Server データベースの作成** ダイアログ ボックスに、入力". \SQLExpress"で、**サーバー名**ボックスと"aspnet ContosoUniversity"で、**新しいデータベース名**ボックスで、クリックして**OK**します。

![Aspnet ContosoUniversity を作成します。](deploying-to-iis/_static/image9.png)

"ContosoUniversity"をという名前の新しい SQL Server Express の School データベースを作成する同じ手順に従います。

**サーバー エクスプ ローラー** 2 つの新しいデータベースが表示されます。

![サーバー エクスプ ローラーで新しいデータベース](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>新しいデータベースの許可スクリプトを作成します。

アプリケーションを開発用コンピューターに IIS で実行すると、アプリケーションは既定のアプリケーション プールの資格情報を使用して、データベースにアクセスします。 ただし、既定では、アプリケーション プール id は、データベースを開くアクセス許可がありません。 アクセス許可を付与するスクリプトを実行する必要がようにします。 このセクションでは、IIS での実行時に、アプリケーションが、データベースを開くことができるかどうかを確認する後で実行しますスクリプトを作成します。

(いないプロジェクトの 1 つ、)、ソリューションを右クリックし、をクリックして**新しい項目の追加**、し、作成、新しい**SQL ファイル**という*Grant.sql*します。 ファイルに次の SQL コマンドをコピーおよび保存して、ファイルを閉じる。

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> このスクリプトは、このチュートリアルで指定されている SQL Server Express 2012 と Windows 8 または Windows 7 で IIS 設定の動作設計されています。 または、Windows の SQL Server の別のバージョンを使用している場合、または IIS を設定するコンピューターに異なる場合は、このスクリプトへの変更が必要な場合があります。 SQL Server スクリプトの詳細については、次を参照してください。 [SQL Server オンライン ブックの「](https://go.microsoft.com/fwlink/?LinkId=132511)します。


> [!NOTE] 
> 
> **セキュリティに関する注意**このスクリプトは、db\_、運用環境でがありますが、実行時にデータベースにアクセスするユーザーに対する所有者アクセス許可。 一部のシナリオで、展開にのみアクセス許可を更新し、実行時のデータを読み書きするのみのアクセス許可を持つ別のユーザーを指定します。 データベースの完全スキーマを持つユーザーを指定する場合があります。 詳細については、次を参照してください。 [Code First Migrations に対する自動の Web.config の変更をレビュー](#reviewingmigrations)このチュートリアルで後述します。


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>アプリケーションのデータベースで許可スクリプトを実行します。

そのデータベースの配置は dbDacFx プロバイダーを使用するためのデプロイ中に、メンバーシップ データベース内の許可スクリプトを実行する発行プロファイルを構成することができます。 アプリケーション データベースをデプロイする方法は、Code First Migrations の配置時にスクリプトを実行することはできません。 そのためは、アプリケーション データベースに展開する前にスクリプトを手動で実行する必要があります。

1. Visual Studio で開く、 *Grant.sql*先ほど作成したファイル。
2. **[接続]** をクリックします。 

    ![[接続] ボタン](deploying-to-iis/_static/image11.png)
3. **サーバーへの接続** ダイアログ ボックスに、入力 *. \SQLExpress*として、**サーバー名**、 をクリックし、 **Connect**。
4. データベースのドロップダウン リストで選択**ContosoUniversity**、 をクリックし、 **Execute**します。 

    ![](deploying-to-iis/_static/image12.png)

既定のアプリケーション プール id で、アプリケーションの実行時に、データベース テーブルを作成する Code First Migrations に対するアプリケーションのデータベースに十分なアクセス許可できるようになりました。

## <a name="publish-to-iis"></a>IIS への公開します。

Visual Studio と Web Deploy を使用して IIS に配置できるいくつかの方法はあります。

- Visual Studio のワンクリック発行を使用します。
- コマンドラインから発行します。
- 作成、*展開パッケージ*し、IIS マネージャーの UI を使用してインストールします。 展開パッケージの構成要素を *.zip*ファイルをすべてのファイルと IIS にサイトをインストールに必要なメタデータが含まれています。
- 展開パッケージを作成し、コマンドラインを使用してインストールします。

プロセス自動化する Visual Studio を設定する前のチュートリアルでデプロイ タスクは、これらのメソッドのすべてに適用されます。 これらのチュートリアルでは、これらのメソッドの最初の 2 つを使用します。 展開パッケージの使用方法の詳細については、次を参照してください。[作成し、web 配置パッケージをインストールした web アプリケーションの配置](https://go.microsoft.com/fwlink/p/?LinkId=282413#package)for Visual Studio および ASP.NET Web 配置コンテンツ マップでします。

発行前に、管理者モードで Visual Studio を実行していることを確認します。 表示されない場合 **(管理者)** タイトル バーで、Visual Studio を閉じます。 Windows 8 で**開始**ページまたは Windows 7**開始**] メニューの [は、ご使用の Visual Studio のバージョンのアイコンを右クリックし、選択**管理者として実行**します。 管理者モードでは、のみときに発行している IIS、ローカル コンピューター上の発行に必要です。

### <a name="create-the-publish-profile"></a>発行プロファイルを作成します。

1. **ソリューション エクスプ ローラー**ContosoUniversity プロジェクト (ContosoUniversity.DAL プロジェクトではなく) を右クリックし、選択、**発行**します。

    **Web の発行**ウィザードが表示されます。

    ![発行ウィザード プロファイル タブ](deploying-to-iis/_static/image13.png)
2. ドロップダウン リストで選択**&lt;新規.&gt;**. (最新の Visual Studio は、インストールを更新プログラム、ドロップダウン リストではありませんし、最初から新しいプロファイルを作成するためにクリックするボタンが**カスタム**)。
3. **新しいプロファイル** ダイアログ ボックスが"Test"を入力し、クリックして**OK**します。

    ウィザードが自動的に進みます、**接続**タブ。
4. **サービス URL**ボックスに、入力*localhost*します。
5. **サイト/アプリケーション**ボックスに、入力*既定の Web サイト/ContosoUniversity*
6. **送信先 URL**ボックスに、入力 `http://localhost/ContosoUniversity`

    **送信先 URL**設定は必要ありません。 Visual Studio では、アプリケーションのデプロイが完了したら、この URL を既定のブラウザーが自動的に開きます。 展開した後に自動的に開くブラウザーをしたくない場合は、このボックスを空白のままにします。
7. クリックして**接続の検証**設定が正しいことと、ローカル コンピューターの IIS に接続できることを確認します。

    緑色のチェック マークは、接続が成功したことを確認します。

    ![公開 Web ウィザードの [接続] タブ](deploying-to-iis/_static/image14.png)
8. クリックして**次**に進めておく、**設定** タブ。
9. **構成** ボックスの一覧を展開するビルド構成を指定します。 リリースの既定値に設定したままにします。 このチュートリアルではデバッグ ビルドが配置されません。
10. 展開**ファイル発行オプション**、し、**アプリからファイルを除外する\_データ フォルダー**します。

    .Mdf ファイルではないファイルをアプリケーションは、ローカルの SQL Server Express インスタンスで作成したデータベースへのアクセスのテスト環境で、*アプリ\_データ*フォルダー。
11. ままに、**発行中にプリコンパイル**と**転送先に追加のファイルを削除**チェック ボックスをオフします。

    ![[設定] タブでファイル発行オプション](deploying-to-iis/_static/image15.png)

    プリコンパイルのオプションには、非常に大規模なサイトは; は、します。最初に、サイトをパブリッシュした後、ページが要求されたページ スタートアップ時間短縮できることができます。

    これは、最初のデプロイがすべてのファイル変換先のフォルダーにまだ追加ファイルを削除する必要はありません。

    > [!NOTE] 
    > 
    > [!CAUTION]
    > 選択した場合**追加ファイルを削除**の後続の配置が同じサイトに表示されるよう事前に展開する前に削除するファイルは、プレビュー機能を使用することを確認します。 想定される動作は、Web 配置と、プロジェクトの削除が移行先サーバー上のファイルが削除されます。 ただし、元とコピー先フォルダー の下のフォルダー全体の構造体を比較すると、および一部のシナリオで Web Deploy 可能性があります削除ファイルを削除する必要はありません。
    > 
    > たとえば、ルート フォルダーにプロジェクトを配置するときに、サーバー上のサブフォルダーに、web アプリケーションをした場合、サブフォルダーは削除が。 Contoso.com でメイン サイトの 1 つのプロジェクトとブログは contoso.com/blog の別のプロジェクトがあります。 ブログ アプリケーションがサブフォルダーです。 メイン サイトを展開するときに、転送先に追加ファイルの削除を選択した場合は、ブログのアプリケーションが削除されます。
    > 
    > 別の例では、アプリの\_データ フォルダーが予期せず削除可能性があります。 SQL Server Compact などの特定のデータベースでは、アプリのデータベース ファイルを保存\_データ フォルダー。 初期デプロイ後に除外するアプリを選ぶことが以降のデプロイでデータベース ファイルのコピーを保持したくない\_[パッケージ/web の発行] タブのデータ。転送先の選択に追加ファイルの削除、データベース ファイルと、アプリがある場合、その後\_次回に発行するときに、データ フォルダー自体が削除されます。

### <a name="configure-deployment-for-the-membership-database"></a>メンバーシップ データベースの展開を構成します。

次の手順に適用されます、 **DefaultConnection**データベースに、**データベース** ダイアログ ボックスのセクション。

1. **リモート接続文字列**ボックスに、SQL Server Express の新しいメンバーシップ データベースを指す次の接続文字列を入力します。

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    展開プロセスは、ために、デプロイした Web.config ファイルでのこの接続文字列を配置は**実行時にこの接続文字列を使用して**が選択されています。

    接続文字列を取得することができますも**サーバー エクスプ ローラー**します。 **サーバー エクスプ ローラー**、展開**データ接続**を選択し、  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity**データベースから、**プロパティ**ウィンドウのコピー、**接続文字列**値。 接続文字列が削除できる追加設定を 1 つにある:`Pooling=False`します。
2. 選択**Update database**します。

    デプロイ中に転送先データベース内に作成するデータベース スキーマになります。 次の手順を実行する必要がある追加のスクリプトを指定する: 既定のアプリケーション プール、データを展開する 1 つのデータベース アクセスを許可する 1 つ。
3. クリックして**データベース更新を構成する**します。
4. **データベース更新を構成する**ダイアログ ボックスで、 をクリックして**SQL スクリプトの追加**に移動し、 *Grant.sql*ソリューション フォルダーに保存したスクリプト。
5. 追加するプロセスを繰り返し、 *aspnet-データ-dev.sql*スクリプト。

    ![メンバーシップ データベースのデータベースの更新プログラムを構成します。](deploying-to-iis/_static/image16.png)
6. **[閉じる]** をクリックします。

### <a name="configure-deployment-for-the-application-database"></a>アプリケーション データベースの展開を構成します。

Visual Studio で、Entity Framework が検出した場合`DbContext`でエントリを作成しますが、クラス、**データベース**セクションを**Code First Migrations の実行**チェック ボックスの代わりに、 **データベースを更新する**チェック ボックスをオンします。 このチュートリアルでは、Code First Migrations のデプロイを指定するのにチェック ボックスを使用します。

一部のシナリオで使用している、`DbContext`データベースが移行ではなく dbDacFx プロバイダーを使用してデータベースを配置します。 その場合を参照してください[移行せず、Code First のデータベースをデプロイする方法でしょうか。](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) msdn ASP.NET Web 配置の faq。

次の手順に適用されます、 **SchoolContext**データベースに、**データベース** ダイアログ ボックスのセクション。

1. **リモート接続文字列**ボックスに、新しい SQL Server Express アプリケーション データベースを指す次の接続文字列を入力します。

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    展開プロセスは、ために、デプロイした Web.config ファイルでのこの接続文字列を配置は**実行時にこの接続文字列を使用して**が選択されています。

    アプリケーション データベースの接続文字列を取得することができますも**サーバー エクスプ ローラー**同様のメンバーシップを取得したデータベース接続文字列。
2. 選択**実行 Code First Migrations (アプリケーションの起動時に実行)** します。

    このオプションを指定する配置の Web.config ファイルを構成する展開プロセスでは、`MigrateDatabaseToLatestVersion`初期化子。 この初期化子は、アプリケーションの展開後に最初に、データベースにアクセスするときに自動的にデータベースと最新バージョンに更新します。

### <a name="configure-publish-profile-transforms"></a>構成プロファイルの変換の発行

1. クリックして**閉じる**、順にクリックします**はい**されたら変更を保存するかどうか。
2. **ソリューション エクスプ ローラー**、展開**プロパティ**、展開**PublishProfiles**します。
3. Rright クリック*Test.pubxml、* 順にクリックします**構成変換の追加**します。

    ![Config 変換 メニューを追加します。](deploying-to-iis/_static/image17.png)

    Visual Studio によって作成、 *Web.Test.config*変換ファイルを開きます。
4. *Web.Test.config*ファイルの変換を開始した直後に次のコードを挿入構成タグ。

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    テストを使用する場合、発行プロファイルのこの変換は、"Test"に環境のインジケーターを設定します。 デプロイされたサイトでは、"Contoso University"H1 見出し"(Test)"を確認します。
5. ファイルを保存して閉じます。
6. 右クリックし、 *Web.Test.config*ファイルを**プレビュー変換**を組み込んだ変換が期待どおりの変更を生成するかどうかを確認します。

    **Web.config プレビュー**ウィンドウには、両方を適用した結果が表示されます、 *Web.Release.config*変換と*Web.Test.config*を変換します。

### <a name="preview-the-deployment-updates"></a>展開の更新プログラムをプレビューします。

1. 開く、 **Web の発行**ウィザードをもう一度 (ContosoUniversity プロジェクトを右クリックし、をクリックして**発行**)。
2. **プレビュー**  タブで、ことを確認します、**テスト**プロファイルがオンのままにして**プレビューの開始**にコピーされるファイルの一覧を参照してください。

    ![[プレビュー] ボタン](deploying-to-iis/_static/image18.png)

    ![発行のプレビュー](deploying-to-iis/_static/image19.png)

    クリックすることも、**プレビュー データベース**メンバーシップ データベースで実行されるスクリプトを表示するリンク。 (スクリプトは実行されません Code First Migrations の展開のため、アプリケーション データベースをプレビューするものはありません。)
3. **[発行]** をクリックします。

    管理者モードで Visual Studio でない場合は、アクセス許可エラーを示すエラー メッセージを取得可能性があります。 その場合は、Visual Studio を閉じます、管理者モードで開くし、再度パブリッシュしてください。

    Visual Studio が管理者モードである場合、**出力**ウィンドウのレポートが正常にビルドして発行します。

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    内の URL を入力した場合、**送信先 URL**発行プロファイルでボックス**接続** タブで、ブラウザーは自動的にローカル コンピューターの IIS で実行されている Contoso University のホーム ページが開きます。

## <a name="test-in-the-test-environment"></a>テスト環境でテストします。

「(テスト)」の環境のインジケーターを示すことに注意してください。"(Dev)"ではなくすることを示しています、 *Web.config*環境インジケーターの変換が成功しました。

実行、 **Instructors** Code First シード インストラクター データでデータベースを確認します。 Code First のデータベースを作成しを実行するため、読み込みに数分かかる場合がありますこのページを選択すると、`Seed`メソッド。 (しないをまだデータベースにアクセスするアプリケーションを試行していないので、ホーム ページで行ったときにします。)

をクリックして、**学生** タブに配置されているデータベースに学生がないことを確認します。

選択**受講者の追加**から、**学生** メニューの は、生徒を追加しで新しい学生を表示し、**受講者**データベースに正常に記述できることを確認する ページ.

**コース**メニューの **更新クレジット**します。 **更新クレジット**ページには、管理者のアクセス許可が必要ですので、**ログで**ページが表示されます。 以前 ("admin"および"devpwd") で作成した管理者アカウントの資格情報を入力します。 **更新クレジット**ページが表示され、前のチュートリアルで作成した管理者アカウントがテスト環境に正しくデプロイされたことを確認します。

いることを確認、 *Elmah*フォルダーに存在します、 *c:\inetpub\wwwroot\ContosoUniversity*フォルダー内のプレース ホルダー ファイルのみです。

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First Migrations に対する自動の Web.config の変更を確認してください。

開く、 *Web.config*にデプロイされたアプリケーション内でファイル*C:\inetpub\wwwroot\ContosoUniversity*場所に Code First Migrations の構成、展開プロセスに自動的に確認できます最新バージョンにデータベースを更新します。

![](deploying-to-iis/_static/image21.png)

展開プロセスには、データベース スキーマの更新専用に使用する Code First Migrations に対する新しい接続文字列も作成されます。

![Database_Publish 接続文字列](deploying-to-iis/_static/image22.png)

この追加の接続文字列では、データベース スキーマの更新プログラムの 1 つのユーザー アカウントとアプリケーションのデータ アクセス用の別のユーザー アカウントを指定することができます。 たとえば、割り当てたり、 **db\_所有者**Code First Migrations では、ロールと**db\_datareader**と**db\_datawriterの各**アプリケーションにロール。 これは、多層防御の一般的なパターンを妨げる可能性のある悪意のあるコードからデータベース スキーマを変更するアプリケーションです。 (たとえば、これで行われる処理 SQL インジェクション攻撃が成功します。)このパターンは、これらのチュートリアルでは使用されません。 自分のシナリオでこのパターンを実装する場合は、次の手順を実行してしないでください。

1. **設定**のタブ、 **Web の発行**ウィザード、データベースの完全スキーマの更新権限を持つユーザーを指定する接続文字列を入力し、オフ、**この接続文字列を使用して、実行時に**チェック ボックスをオンします。 これは、配置の Web.config ファイルで、`DatabasePublish`接続文字列。
2. アプリケーションは、実行時に使用する接続文字列を Web.config ファイルの変換を作成します。

## <a name="summary"></a>まとめ

開発用コンピューターに IIS にアプリケーションをデプロイして存在テストようになりました。

![テストでのホーム ページ](deploying-to-iis/_static/image23.png)

これは、ことを検証、展開プロセスでは、アプリケーションのコンテンツをコピー (デプロイしないファイルを除く)、適切な場所にも、Web Deploy IIS が正しく構成されて展開時にします。 次のチュートリアルがまだ実行されている展開のタスクを検索する 1 つ以上のテストを実行します。 上のフォルダーのアクセス許可の設定、 *Elmah*フォルダー。

## <a name="more-information"></a>詳細情報

Visual Studio での IIS または IIS Express の実行方法の詳細については、次のリソースを参照してください。

- [IIS Express の概要](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net サイト。
- [IIS Express の概要](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx)Scott Guthrie のブログ。
- [Web では、ASP.NET Web プロジェクトを Visual Studio でサーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx)します。
- [コアの相違点の間で IIS と ASP.NET 開発サーバー](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET サイト。

どのような問題については、中程度の信頼でアプリケーションを実行するときに発生する可能性が、参照してください[中程度の信頼で ASP.NET アプリケーションをホストしている](http://www.4guysfromrolla.com/articles/100307-1.aspx)Rolla サイトから 4 Guys にします。

> [!div class="step-by-step"]
> [前へ](project-properties.md)
> [次へ](setting-folder-permissions.md)
