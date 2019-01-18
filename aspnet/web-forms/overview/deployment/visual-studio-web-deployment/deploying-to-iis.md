---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: Visual Studio を使用して ASP.NET Web の展開:テストへのデプロイ |Microsoft Docs
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: d49dfad368ca4b81bb865103a99ec223a1cc66df
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396338"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Visual Studio を使用して ASP.NET Web の展開:テストへのデプロイ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、または Visual Studio 2017 を使用して、サード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。

## <a name="overview"></a>概要

このチュートリアルでは、ローカル コンピューターにインターネット インフォメーション サーバー (IIS) に ASP.NET web アプリケーションをデプロイします。

一般にアプリケーションを開発するときにを実行し、Visual Studio でテストします。 既定では、Visual Studio 2017 での web アプリケーション プロジェクトを使用して、IIS Express 開発 web サーバーとします。 Visual Studio 開発サーバー (Cassini とも呼ばれます)、既定で Visual Studio 2017 を使用するよりも完全な IIS のように、IIS Express がよりは動作します。 どちらも開発 web サーバーが IIS とまったく同じように動作します。 その結果、実行しますが、Visual Studio で正しくテストしてとを IIS に配置されるときに失敗するアプリがでしたとします。

2 つの方法でアプリケーションを確実にテストできます。

1. 後で、運用環境にデプロイに使用するのと同じプロセスを使用して開発用コンピューターに IIS にアプリケーションをデプロイします。

   Web プロジェクトを実行するが、デプロイ プロセスをテストするはときに、IIS を使用する Visual Studio を構成することができます。 このメソッドは、デプロイ プロセスを検証し、IIS 下で、アプリケーションが正常に実行します。

2. 運用環境のようなテスト環境にアプリケーションをデプロイします。 
 
   これらのチュートリアルの実稼働環境では、Azure App Service で Web アプリが。 理想的なテスト環境では、Azure サービスで作成された追加の web アプリです。 運用 web アプリと同様の設定は、テストに対してのみ使用が。

オプション 2 は、テストする最も信頼性の高い方法です。 オプション 2 を使用する場合は、オプション 1 を使用する必要はありませんとは限りません。 ただし、サードパーティにデプロイする場合、ホスティング プロバイダー、オプション 2 できない場合もありますか、または、高価なため、このチュートリアル シリーズは、両方の方法を示しています。 オプション 2 のガイダンスがで提供される、[実稼働環境に展開する](deploying-to-production.md)チュートリアル。

Visual Studio で web サーバーの使用に関する詳細については、次を参照してください。 [ASP.NET Web プロジェクト用の Visual Studio で Web サーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx)します。

リマインダー:エラー メッセージが表示される、または、チュートリアルを進めるときに機能しない、するを確認してください、[トラブルシューティング ページ](troubleshooting.md)します。

## <a name="download-the-contoso-university-starter-project"></a>Contoso University のスタート プロジェクトをダウンロードします。

ダウンロードし、Contoso University の Visual Studio スタート ソリューションとプロジェクトをインストールします。 このソリューションには、完了したチュートリアルが含まれています。 

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>IIS をインストールします。

開発用コンピューターに IIS に展開するには、IIS および Web Deploy がインストールされていることを確認します。 既定では、Visual Studio では、Web Deploy がインストールされますが、IIS が既定の Windows 10、Windows 8、または Windows 7 の構成に含まれていません。 場合は IIS をインストールしてあると、既定のアプリケーション プールが既に .NET 4 に設定されて、 [、次のセクション](#sqlexpress)します。

1. 使用することをお勧め、 [Web プラットフォーム インストーラー (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) IIS と Web Deploy をインストールします。 WPI では、必要な場合は、IIS と Web デプロイの前提条件を含む、推奨される IIS 構成をインストールします。

   IIS、Web デプロイ、または、必要なコンポーネントのいずれかをインストールしてある場合は、不足しているだけ、WPI がインストールされます。

   * IIS と Web Deploy をインストールするのにには、Web Platform Installer を使用します。
    
     ![WPI を使用して、IIS をインストールします。](deploying-to-iis/_static/image30.png)

     ![Web 配置 WPI を使用してをインストールします。](deploying-to-iis/_static/image31.png)

     IIS 7 をインストールすることを示すメッセージが表示されます。 リンクが Windows 8 で IIS 8 の機能します。ただし、Windows 8 以降では ASP.NET 4.7 がインストールされていることを確認する次の手順を進めます。

   * 開いている**コントロール パネルの ** > **プログラム** > **プログラムと機能** > **オンまたはオフにする Windows の機能**.

   * 展開**インターネット インフォメーション サービス**、 **World Wide Web サービス**、および**アプリケーション開発機能**します。
   
   * 確認します**ASP.NET 4.7**が選択されています。

     ![ASP.NET 4.7 を選択します。](deploying-to-iis/_static/image1a.png)

   * 確認します**World Wide Web サービス**と**IIS 管理コンソール**が選択されています。 これは、IIS および IIS マネージャーをインストールします。
    
     ![World Wide Web サービスを選択します。](deploying-to-iis/_static/image24.png)    
  
   * **[OK]** を選択します。 インストールが実行を示すダイアログ ボックスのメッセージが表示されます。

IIS をインストールすると、実行**IIS Manager** .NET Framework version 4 が既定のアプリケーション プールに割り当てられているかどうかを確認します。

1. 開くには、WINDOWS + R キーを押して、**実行** ダイアログ ボックス。

   (Windows 8 以降、"run"で入力、**開始**ページ。 Windows 7 では、次のように選択します。**実行**から、**開始**メニュー。 場合**実行**に含まれていない、**開始** メニューの タスク バーを右クリックし、選択**プロパティ**を選択、 **スタート メニュー** の選択タブで、**カスタマイズ**、選び**コマンドを実行して**)。

2. "Inetmgr"を入力し、 **OK**します。

3. **接続**ウィンドウで、サーバー ノードを展開し、選択**アプリケーション プール**します。 **アプリケーション プール**ウィンドウ場合**DefaultAppPool**を .NET framework version 4 を次の図のように割り当てられている、次のセクションに進んでください。

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. 2 つだけのアプリケーション プールを参照してください、.NET Framework 2.0 に設定する場合は、IIS で ASP.NET 4 をインストールします。

   Windows 8 以降がインストールされていることを ASP.NET 4.7 を確認して、前のセクションの手順を参照してくださいまたはを参照してください[Windows 8 および Windows Server 2012 で ASP.NET 4.5 をインストールする方法](https://support.microsoft.com/kb/2736284)します。 Windows 7 を右クリックし、コマンド プロンプト ウィンドウを開きます。**コマンド プロンプト**、Windows で**開始**メニュー**管理者として実行**します。 実行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)次のコマンドを使用して、IIS で ASP.NET 4 をインストールします。 (32 ビット システムでは、「フレームワーク」と"Framework64"を置き換えてください)。

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   このコマンドは、新しいアプリケーション、.NET Framework 4 用のプールが既定のアプリケーション プールが残ります 2.0 に設定を作成します。 そのアプリケーション プールには、.NET 4 を対象が、.NET 4 に、アプリケーション プールをので変更すること、アプリケーションをデプロイします。

5. 閉じた場合**IIS Manager**、もう一度実行、サーバー ノードを展開および選択**アプリケーション プール**します。

6. **アプリケーション プール**ペインで、 **DefaultAppPool**します。 **アクション**ペインで、**基本設定**します。

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. **アプリケーション プールの編集** ダイアログ ボックスで、変更、 **.NET CLR バージョン**に **.NET CLR v4.0.30319**します。 **[OK]** を選択します。

   ![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

IIS に web アプリケーションを発行する準備ができました。 最初に、ただし、テスト用データベースを作成します。

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>SQL Server Express をインストールします。

LocalDB ので、テスト環境、SQL Server Express をインストールして、IIS で動作するものではありません。 Visual Studio 2010 SQL Server Express を使用している場合は、既定では既にインストールされています。 Visual Studio 2012 またはそれ以降を使用している場合は、SQL Server Express をインストールします。

SQL Server Express をインストールするには、ダウンロードしてからインストール[ダウンロード センター。Microsoft SQL Server 2017 Express edition](https://www.microsoft.com/sql-server/sql-server-editions-express)します。 

SQL Server インストール センターの最初のページで次のように選択します。 **SQL Server の新規スタンドアロン インストールまたは既存のインストールに機能の追加**既定の選択を受け入れ、指示に従います。 インストール ウィザードでは、既定の設定をそのまま使用します。 インストール オプションの詳細については、次を参照してください。[インストール ウィザード (セットアップ) からの SQL Server のインストール](https://msdn.microsoft.com/library/ms143219.aspx)します。

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>テスト環境用の SQL Server Express データベースを作成します。

Contoso University アプリケーションには、2 つのデータベースがあります。 

1. メンバーシップ データベース 
2. アプリケーション データベース 

2 つの異なるデータベースまたは単一のデータベースには、これらのデータベースをデプロイできます。 それらを組み合わせることにより、簡単にそれらの間のデータベースの結合です。 

サード パーティのホスティング プロバイダーにデプロイする場合、ホスティング プランがすることもそれらを結合する理由。 たとえば、プロバイダーは、複数のデータベースの詳細は課金可能性があります。 または複数のデータベースをも許可しない場合があります。

このチュートリアルでは、テスト環境で 2 つのデータベースとステージングと運用環境で 1 つのデータベースをデプロイします。

**ビュー**  メニューの選択 Visual Studio で**サーバー エクスプ ローラー** (**データベース エクスプ ローラー** Visual Web Developer で)。 右クリック**データ接続**選択**新しい SQL Server データベースの作成**です。

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

**新しい SQL Server データベースの作成** ダイアログ ボックスに、入力". \SQLExpress"で、**サーバー名**ボックスと"aspnet ContosoUniversity"で、**新しいデータベース名**ボックス。 **[OK]** を選択します。

![Aspnet ContosoUniversity を作成します。](deploying-to-iis/_static/image9.png)

という名前の新しい SQL Server Express の School データベースを作成する同じ手順に従って`ContosoUniversity`します。

**サーバー エクスプ ローラー** 2 つの新しいデータベースを示しています。

![サーバー エクスプ ローラーで新しいデータベース](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>新しいデータベースの許可スクリプトを作成します。

アプリケーションを開発用コンピューターに IIS で実行すると、アプリケーションは、データベースにアクセスするのに既定のアプリケーション プールの資格情報を使用します。 ただし、既定では、アプリケーション プールでは、データベースを開くアクセス許可がありません。 これは、アクセス許可を付与するスクリプトを実行する必要があることを意味します。 このセクションではそのスクリプトを作成し、アプリケーションは IIS で実行時に、データベースを開くことができるかどうかを確認するには、後で実行します。

テキスト エディターで新しいファイルに次の SQL コマンドをコピーし、として保存*Grant.sql*します。 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

Visual Studio で、Contoso University のソリューションを開きます。 (いないプロジェクトの 1 つ、)、ソリューションを右クリックして**追加**します。 選択**既存項目の**を参照する*Grant.sql*、し、開きます。

> [!NOTE]
> このチュートリアルで指定されているとして Windows 10、Windows 8、または Windows 7 で IIS 設定とおよび SQL Server Express 2012 と連携して、またはそれ以降にこのスクリプトが設計います。 別のバージョンの SQL Server または Windows を使用している場合、または IIS を設定するコンピューターに異なる場合は、このスクリプトへの変更が必要な場合があります。 SQL Server スクリプトの詳細については、次を参照してください。 [SQL Server オンライン ブックの「](https://go.microsoft.com/fwlink/?LinkId=132511)します。

> [!NOTE] 
> **セキュリティに関する注意**このスクリプトは、 `db_owner` 、運用環境でがありますが、実行時にデータベースにアクセスするユーザーへのアクセス許可。 一部のシナリオでは、展開に対してのみアクセス許可を更新し、実行時のデータを読み書きするのみのアクセス許可を持つ別のユーザーを指定します。 データベースの完全スキーマを持つユーザーを指定する場合があります。 詳細については、次を参照してください。 [Code First Migrations に対する自動の Web.config の変更をレビュー](#reviewingmigrations)このチュートリアルで後述します。

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>アプリケーションのデータベースで許可スクリプトを実行します。

そのデータベースの配置を使用するためのデプロイ中に、メンバーシップ データベース内の許可スクリプトを実行する発行プロファイルを構成することができます、 [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing)プロバイダー。 アプリケーション データベースをデプロイする方法は、Code First Migrations の配置時にスクリプトを実行することはできません。 つまり、アプリケーション データベースに展開する前にスクリプトを手動で実行する必要があります。

1. Visual Studio で開く、 *Grant.sql*先ほど作成したファイル。

2. 選択**接続**します。 

    ![[接続] ボタン](deploying-to-iis/_static/image11.png)

3. **サーバーへの接続** ダイアログ ボックスに、入力 *. \SQLExpress*として、**サーバー名**します。 選択**接続**します。

4. データベースのドロップダウン リストで選択**ContosoUniversity**します。 選択**実行**します。 

   ![](deploying-to-iis/_static/image12.png)

既定のアプリケーション プール id で、アプリケーションの実行時に、データベース テーブルを作成する Code First Migrations に対するアプリケーションのデータベースに十分なアクセス許可できるようになりました。

## <a name="publish-to-iis"></a>IIS に公開する

Visual Studio と Web Deploy を使用して IIS に配置できるいくつかの方法はあります。

* Visual Studio のワンクリック発行を使用します。
* コマンドラインから発行します。
* 展開パッケージを作成し、IIS マネージャーを使用してインストールします。 パッケージには、すべてのファイルと IIS にサイトをインストールに必要なメタデータで .zip ファイルがあります。
* 展開パッケージを作成し、コマンドラインを使用してインストールします。

プロセス自動化する Visual Studio を設定する前のチュートリアルでデプロイ タスクは、これらのメソッドのすべてに適用されます。 これらのチュートリアルでは、最初の 2 つのメソッドを使用します。 展開パッケージの使用方法の詳細については、次を参照してください。[作成し、web 配置パッケージをインストールした web アプリケーションの配置](https://go.microsoft.com/fwlink/p/?LinkId=282413#package)for Visual Studio および ASP.NET Web 配置コンテンツ マップでします。

発行前に、管理者モードで Visual Studio を実行していることを確認します。 表示されない場合 **(管理者)** タイトル バーで、Visual Studio を閉じます。 Windows 8 (またはそれ以降)**開始**ページまたは Windows 7**開始**] メニューの [Visual Studio アイコンを右クリックし、選択**管理者として実行**します。 管理者モードはローカル コンピューターの IIS に発行するときに公開するために必要です。

### <a name="create-the-publish-profile"></a>発行プロファイルを作成します。

1. **ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクト (いない、 **ContosoUniversity.DAL**プロジェクト)。 **[発行]** を選びます。 **発行**ページが表示されます。

2. 選択**新しいプロファイル**します。 **発行先を選択** ダイアログ ボックスが表示されます。

3. 選択**IIS、FTP など**します。**[プロファイルの作成]** を選択します。 **発行**ウィザードが表示されます。

   ![発行ウィザード プロファイル タブ](deploying-to-iis/_static/image26.png)

4. **メソッドを公開**ドロップダウン メニューで、 **Web Deploy**します。

5. **Server**、入力*localhost*します。

6. **サイト名**、入力*既定の Web サイト/ContosoUniversity*します。

7. **送信先 URL**、入力 *http://localhost/ContosoUniversity*します。

   **送信先 URL**設定は必要ありません。 Visual Studio では、アプリケーションのデプロイが完了したら、この URL を既定のブラウザーが自動的に開きます。 展開した後に自動的に開くブラウザーをしたくない場合は、このボックスを空白のままにします。

8. 選択**接続の検証**設定が正しいことと、ローカル コンピューターの IIS に接続できることを確認します。

   緑色のチェック マークは、接続が成功したことを確認します。

   ![公開 Web ウィザードの [接続] タブ](deploying-to-iis/_static/image27.png)

9. 選択**次**に進めておく、**設定**タブ。

10. **構成** ボックスの一覧を展開するビルド構成を指定します。 既定値のままに**リリース**します。 このチュートリアルではデバッグ ビルドが配置されません。

11. 展開**ファイル発行オプション**します。 選択**アプリからファイルを除外する\_データ フォルダー**します。

    テスト環境でアプリケーションにアクセスし、ローカル SQL Server Express インスタンスに .mdf ファイルではなくで作成したデータベース、*アプリ\_データ*フォルダー。

12. ままに、**発行中にプリコンパイル**と**転送先に追加のファイルを削除**チェック ボックスをオフします。

    ![[設定] タブでファイル発行オプション](deploying-to-iis/_static/image15a.png)

    プリコンパイルは、主に大規模なサイトのために役立つオプションです。 起動時間、サイトをパブリッシュした後、ページが要求された最初の時間短縮できることができます。

    これは、最初のデプロイがすべてのファイル変換先のフォルダーにまだ追加ファイルを削除する必要はありません。

    > [!NOTE] 
    > 選択した場合**転送先に追加のファイルを削除**の後続の配置が同じサイトに表示されるよう事前に展開する前に削除するファイルは、プレビュー機能を使用することを確認します。 想定される動作は、Web 配置と、プロジェクトの削除が移行先サーバー上のファイルが削除されます。 ただし、元とコピー先フォルダー の下のフォルダー全体の構造体と比較されます。一部のシナリオで Web Deploy 可能性がありますファイルと削除を削除する必要はありません。
    > 
    > たとえば、ルート フォルダーにプロジェクトを配置するときに、サーバー上のサブフォルダーに、web アプリケーションをした場合、サブフォルダーは削除されます。 Contoso.com でメイン サイトの 1 つのプロジェクトとブログは contoso.com/blog の別のプロジェクトがあります。 ブログ アプリケーションがサブフォルダーです。 選択した場合**転送先に追加のファイルを削除**メイン サイトを展開すると、ブログ アプリケーションが削除されます。
    > 
    > 別の例では、アプリの\_データ フォルダーが予期せず削除可能性があります。 SQL Server Compact などの特定のデータベースでは、アプリのデータベース ファイルを保存\_データ フォルダー。 初期のデプロイ後を選ぶことが、後続の配置でデータベース ファイルのコピーを保持したくない**を除外するアプリ\_データ**[パッケージ/web の発行] タブ。ある場合にその後**転送先に追加のファイルを削除**選択すると、データベース ファイルとアプリ\_次回に発行するときに、データ フォルダー自体が削除されます。

### <a name="configure-deployment-for-the-membership-database"></a>メンバーシップ データベースの展開を構成します。

次の手順に適用されます、 **DefaultConnection**  ダイアログ ボックスでのデータベース**データベース**セクション。

1. **リモート接続文字列**ボックスに、SQL Server Express の新しいメンバーシップ データベースを指す次の接続文字列を入力します。

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   展開プロセスがデプロイした Web.config ファイルでこの接続文字列を格納**実行時にこの接続文字列を使用して**が選択されています。

    接続文字列を取得することができますも**サーバー エクスプ ローラー**します。 **サーバー エクスプ ローラー**、展開**データ接続**を選択し、  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity**データベースから、**プロパティ**ウィンドウのコピー、**接続文字列**値。 接続文字列が削除できる追加設定を 1 つにある:`Pooling=False`します。

2. 選択**Update database**します。

   これにより、デプロイ中に転送先データベース内に作成するデータベースのスキーマです。 次の手順で実行する必要がある追加のスクリプトを指定する: 既定のアプリケーション プール、データを展開する 1 つのデータベース アクセスを許可する 1 つ。

3. 選択**データベース更新を構成する**します。
 
4. **データベース更新を構成する**ダイアログ ボックスで、 **SQL スクリプトの追加**します。 移動し、 *Grant.sql*ソリューション フォルダーに保存したスクリプト。

5. 追加するプロセスを繰り返し、 *aspnet-データ-dev.sql*スクリプト。

   ![メンバーシップ データベースのデータベースの更新プログラムを構成します。](deploying-to-iis/_static/image16.png)

6. 選択**閉じる**します。

### <a name="configure-deployment-for-the-application-database"></a>アプリケーション データベースの展開を構成します。

Visual Studio で、Entity Framework が検出した場合`DbContext`でエントリを作成しますが、クラス、**データベース**セクションを**Code First Migrations の実行**チェック ボックスの代わりに、 **データベースを更新する**チェック ボックスをオンします。 このチュートリアルでは、Code First Migrations のデプロイを指定して、そのチェック ボックスを使用します。

一部のシナリオで使用している、`DbContext`データベースが移行ではなく dbDacFx プロバイダーを使用してデータベースを配置します。 その場合を参照してください[移行せず、Code First のデータベースをデプロイする方法でしょうか。](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) msdn ASP.NET Web 配置の faq。

次の手順に適用されます、 **SchoolContext**  ダイアログ ボックスでのデータベース**データベース**セクション。

1. **リモート接続文字列**ボックスに、新しい SQL Server Express アプリケーション データベースを指す次の接続文字列を入力します。

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   展開プロセスがデプロイした Web.config ファイルでこの接続文字列を格納**実行時にこの接続文字列を使用して**が選択されています。

   アプリケーション データベースの接続文字列を取得することができますも**サーバー エクスプ ローラー**と同様に、メンバーシップ データベースの接続文字列を取得しました。

2. 選択**実行 Code First Migrations (アプリケーションの起動時に実行)** します。

   このオプションを指定する配置の Web.config ファイルを構成する展開プロセスでは、`MigrateDatabaseToLatestVersion`初期化子。 この初期化子は、アプリケーションの展開後に最初に、データベースにアクセスするときに自動的にデータベースと最新バージョンに更新します。

### <a name="configure-publish-profile-transforms"></a>構成プロファイルの変換の発行

1. 選択**閉じる**します。 選択**はい**されたら変更を保存するかどうか。

2. **ソリューション エクスプ ローラー**、展開**プロパティ**、展開**PublishProfiles**します。

3. 右クリック*CustomProfile.pubxml*名前を変更および*Test.pubxml*します。

4. 右クリックして*Test.pubxml*します。 選択**Config 変換を追加**します。

   ![Config 変換 メニューを追加します。](deploying-to-iis/_static/image17.png)

   Visual Studio によって作成、 *Web.Test.config*変換ファイルを開きます。

5. *Web.Test.config*ファイルの変換を開始した直後に次のコードを挿入構成タグ。

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    テストを使用する場合、発行プロファイルのこの変換は、"Test"に環境のインジケーターを設定します。 デプロイされたサイトでは、"Contoso University"H1 見出し"(Test)"を確認します。

6. ファイルを保存して閉じます。

7. 右クリックし、 *Web.Test.config*ファイルおよび選択**プレビュー変換**を組み込んだ変換が期待どおりの変更を生成するかどうかを確認します。

    **Web.config プレビュー**ウィンドウには、両方を適用した結果が表示されます、 *Web.Release.config*変換と*Web.Test.config*を変換します。

### <a name="preview-the-deployment-updates"></a>展開の更新プログラムをプレビューします。

1. 開く、 **Web の発行**ウィザードをもう一度 (ContosoUniversity プロジェクトを右クリックして**発行**、し**プレビュー**)。

2. **プレビュー**ダイアログ ボックスで、**プレビューの開始**にコピーされるファイルの一覧を参照してください。 

   ![発行のプレビュー](deploying-to-iis/_static/image29.png)

   選択することも、**プレビュー データベース**メンバーシップ データベースで実行されるスクリプトを表示するリンク。 (スクリプトは実行されません Code First Migrations の展開のため、アプリケーション データベースをプレビューするものはありません。)

3. **[発行]** を選びます。

   管理者モードで Visual Studio でない場合は、アクセス許可のエラー メッセージを取得可能性があります。 その場合は、Visual Studio を閉じます、管理者モードで開くし、再度パブリッシュしてください。

   Visual Studio が管理者モードである場合、**出力**ウィンドウのレポートが正常にビルドして発行します。

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   内の URL を入力した場合、**送信先 URL**発行プロファイルでボックス**接続** タブで、ブラウザーは自動的にコンピューターに IIS で実行されている Contoso University のホーム ページが開きます。

## <a name="test-in-the-test-environment"></a>テスト環境でテストします。

「(テスト)」の環境のインジケーターを示すことに注意してください。"(Dev)、"ではなくすることを示しています、 *Web.config*環境インジケーターの変換が成功しました。

実行、 **Instructors** Code First シード インストラクター データでデータベースを確認します。 Code First のデータベースを作成しを実行するため、読み込みに数分かかる場合がありますこのページを選択すると、`Seed`メソッド。 (しないをまだデータベースにアクセスするアプリケーションを試行していないので、ホーム ページで行ったときにします。)

選択、**学生** タブに配置されているデータベースに学生がないことを確認します。

選択**受講者の追加**から、**学生**メニュー。 、生徒を追加し、新しい学生を表示、**学生**ページ。 これは、データベースに正常に記述できることを確認します。

**コース**メニューの **更新クレジット**します。 **更新クレジット**ページには、管理者のアクセス許可が必要ですので、**ログで**ページが表示されます。 以前 ("admin"および"devpwd") で作成した管理者アカウントの資格情報を入力します。 **更新クレジット**ページが表示されます。 これは、前のチュートリアルで作成した管理者アカウントがテスト環境に正しくデプロイされたことを確認します。

いることを確認、 *ELMAH*フォルダーに存在します、 *c:\inetpub\wwwroot\ContosoUniversity*フォルダー内のプレース ホルダー ファイルのみです。

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First Migrations に対する自動の Web.config の変更を確認してください。

開く、 *Web.config*にデプロイされたアプリケーション内でファイル*C:\inetpub\wwwroot\ContosoUniversity*場所に Code First Migrations の構成、展開プロセスに自動的に確認できます最新バージョンにデータベースを更新します。

![](deploying-to-iis/_static/image21.png)

展開プロセスには、データベース スキーマの更新専用に使用する Code First Migrations に対する新しい接続文字列も作成されます。

![Database_Publish connection string](deploying-to-iis/_static/image22.png)

この追加の接続文字列では、データベース スキーマの更新プログラムの 1 つのユーザー アカウントとアプリケーションのデータ アクセス用の別のユーザー アカウントを指定することができます。 たとえば、割り当てたり、 **db\_所有者**Code First Migrations をロールと**db\_datareader**で**db\_datawriterの各**アプリケーションにロール。 これは、多層防御の一般的なパターンを妨げる可能性のある悪意のあるコードからデータベース スキーマを変更するアプリケーションです。 (たとえば、これで行われる処理 SQL インジェクション攻撃が成功します。)これらのチュートリアルでは、このパターンを使用しないでください。 自分のシナリオでこのパターンを実装するには、次の手順を実行します。

1. **Web の発行**ウィザード [、**設定**] タブで、データベースの完全スキーマの更新権限を持つユーザーを指定する接続文字列を入力します。 クリア、**実行時にこの接続文字列を使用して**チェック ボックスをオンします。 これは、配置の Web.config ファイルで、`DatabasePublish`接続文字列。

2. アプリケーションは、実行時に使用する接続文字列を Web.config ファイルの変換を作成します。

## <a name="summary"></a>まとめ

開発用コンピューターに IIS にアプリケーションをデプロイし、存在テストしましたようになりました。

![テストでのホーム ページ](deploying-to-iis/_static/image23.png)

これは、展開プロセスを適切な場所 (配置しないファイルを除く)、アプリケーションのコンテンツをコピーしてもその Web Deploy IIS が正しく構成されて展開中にことを検証します。 次のチュートリアルがまだ実行されている展開のタスクを検索する 1 つ以上のテストを実行します。 上のフォルダーのアクセス許可の設定、 *Elm ah*フォルダー。

## <a name="more-information"></a>詳細情報

Visual Studio での IIS または IIS Express の実行方法の詳細については、次のリソースを参照してください。

- [IIS Express の概要](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net サイト。
- [IIS Express の概要](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx)Scott Guthrie のブログ。
- [Web では、ASP.NET Web プロジェクトを Visual Studio でサーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx)します。
- [コアの相違点の間で IIS と ASP.NET 開発サーバー](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET サイト。

どのような問題については、中程度の信頼でアプリケーションを実行するときに発生する可能性が、参照してください[中程度の信頼で ASP.NET アプリケーションをホストしている](http://www.4guysfromrolla.com/articles/100307-1.aspx)Rolla サイトから次の 4 つ Guys にします。

> [!div class="step-by-step"]
> [前へ](project-properties.md)
> [次へ](setting-folder-permissions.md)
