---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 5 - テスト環境として IIS への配置 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: ba939012e8fb11a50992eeaef70e8ebf61cea851
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835844"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 5 - テスト環境として IIS に展開します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、ローカル コンピューターの IIS に ASP.NET web アプリケーションをデプロイする方法を説明します。

アプリケーションを開発するときに、Visual Studio で実行して一般的でテストします。 既定では、Visual Studio 開発サーバー (Cassini とも呼ばれます) を使用しているこのことを意味します。 Visual Studio 開発サーバーでは、簡単に、Visual Studio での開発中にテストできますが、IIS とまったく同じように機能しません。 その結果、アプリケーションが、Visual Studio でテストしてホスティング環境で IIS にデプロイされたときに失敗する場合に、正常に実行が可能なです。

次の方法でより信頼性の高いアプリケーションをテストすることができます。

1. Visual Studio で開発中にテストするときに、Visual Studio 開発サーバーではなく IIS Express または完全な IIS を使用します。 IIS 下で、サイトがどのように実行される、このメソッドがより正確にエミュレート一般にします。 ただし、このメソッドが、デプロイ プロセスをテストしない、または、展開プロセスの結果が正しく実行されることを検証しません。
2. 後で、運用環境にデプロイに使用するのと同じプロセスを使用して開発用コンピューターに IIS にアプリケーションをデプロイします。 このメソッドは、IIS 下で、アプリケーションが正しく実行するだけでなく展開プロセスを検証します。
3. 運用環境にできるだけ近いテスト環境にアプリケーションをデプロイします。 これらのチュートリアルについては、運用環境は、サード パーティのホスティング プロバイダーであるため、理想的なテスト環境は、ホスティング プロバイダーで 2 つ目のアカウントになります。 テストのみにこの 2 つ目のアカウントを使用するとしますが、運用環境のアカウントと同じ方法を設定する必要があります。

このチュートリアルでは、オプション 2 の手順を使用します。 末尾にオプション 3 のガイダンスが提供される、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルでは、このチュートリアルの最後には、オプション 1 のリソースへのリンクとします。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。

## <a name="configuring-the-application-to-run-in-medium-trust"></a>中程度の信頼で実行するようにアプリケーションを構成します。

IIS をインストールして展開する前に行うために、サイト実行以上では、一般的な共有ホスティング環境と同様に、Web.config ファイルの設定を変更します。

ホスティング プロバイダーで web サイトを実行する通常*中程度の信頼*、つまり、いくつか実行することはできません。 たとえば、アプリケーション コードことはできません、Windows レジストリへのアクセスしことはできません読み取りまたは書き込みフォルダー階層のアプリケーションの外部にあるファイル。 既定では、アプリケーションの実行*信頼性の高い*ローカル コンピューターにつまりが運用環境にデプロイするときに失敗するための作業を行うことが、アプリケーションである可能性があります。 そのためより正確に反映されているテスト環境を運用環境にするには、中程度の信頼で実行するアプリケーションを構成します。

アプリケーションの Web.config ファイルで追加、**信頼**内の要素、 **system.web**要素は、この例で示すようにします。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

アプリケーションは、ローカル コンピューター上でも今すぐ IIS での中程度の信頼で実行されます。 この設定では、作業を行うが運用環境で失敗するアプリケーション コードによってできるだけ早くしようとするとをキャッチすることができます。

> [!NOTE]
> Entity Framework Code First Migrations を使用している場合は、バージョン 5.0 を行ったことを確認または後でインストールされていること。 Entity Framework バージョン 4.3 での移行には、データベース スキーマを更新するには完全な信頼が必要です。


## <a name="installing-iis-and-web-deploy"></a>インストールを実行する IIS および Web デプロイします。

開発用コンピューターに IIS に展開するには、IIS をいる必要があり、Web Deploy をインストールします。 これらは Windows 7 の既定の構成には含まれません。 IIS と Web デプロイの両方をまだインストールして場合、次のセクションに進みます。

使用して、 [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)は、Web Platform Installer は、IIS の推奨される構成をインストールし、IIS と Web の前提条件を自動的にインストールされますので、IIS および Web Deploy をインストールすることをお勧め必要に応じてをデプロイします。

IIS と Web Deploy をインストールする Web Platform Installer を実行するには、次のリンクを使用します。 既に IIS で Web デプロイまたは、必要なコンポーネントのいずれかをインストールする場合は、不足しているだけ、Web Platform Installer がインストールされます。

- [IIS および Web Deploy WebPI を使用したインストールします。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>既定のアプリケーション プールを .NET 4 に設定します。

IIS をインストールすると、実行**IIS Manager** .NET Framework version 4 が既定のアプリケーション プールに割り当てられているかどうかを確認します。

Windows から**開始**メニューの **実行**"inetmgr"を入力し、クリックして**OK**。 (場合、**実行**コマンドが、**開始** メニューを開くには、Windows キーと R を押すことができます。 または、タスク バーを右クリックし、をクリックして**プロパティ**を選択します、 **スタート メニュー**  タブで、をクリックして**カスタマイズ**を選択し、**コマンドを実行して**)。

**接続**ウィンドウで、サーバー ノードを展開し、選択**アプリケーション プール**します。 **アプリケーション プール**ウィンドウで場合、 **DefaultAppPool**を .NET framework version 4 を次の図のように割り当てられている、次のセクションに進んでください。

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

2 つだけのアプリケーション プールを参照してください、.NET Framework 2.0 に両方の設定は、IIS で ASP.NET 4 をインストールする必要があります。

- 右クリックし、コマンド プロンプト ウィンドウを開く**コマンド プロンプト**、Windows で**開始**メニュー**管理者として実行**します。 実行して[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)次のコマンドを使用して、IIS で ASP.NET 4 をインストールします。 (64 ビット システムでは、"Framework64"と「フレームワーク」を置き換えてください)。

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    このコマンドは、.NET Framework 4 の新しいアプリケーション プールを作成しますが、既定のアプリケーション プールは、2.0 にも設定されます。 展開アプリケーションを対象とする .NET 4 をそのアプリケーション プールを .NET 4 アプリケーション プールを変更する必要があるためです。

閉じた場合**IIS Manager**、もう一度実行、サーバー ノードを展開およびクリックして**アプリケーション プール**を表示する、**アプリケーション プール**ペインをもう一度です。

**アプリケーション プール**ウィンドウで、をクリックして**DefaultAppPool**、し、**アクション**ペイン をクリックします**基本設定**します。

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

**アプリケーション プールの編集** ダイアログ ボックスで、変更 **.NET Framework のバージョン**に **.NET Framework v4.0.30319**  をクリック**OK**します。

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

IIS に発行する準備が整いました。

## <a name="publishing-to-iis"></a>IIS に発行します。

Visual Studio 2010 および Web Deploy を使用してを展開するいくつかの方法はあります。

- Visual Studio のワンクリック発行を使用します。
- 作成、*展開パッケージ*し、IIS マネージャーの UI を使用してインストールします。 展開パッケージの構成要素を *.zip*ファイルをすべてのファイルと IIS にサイトをインストールに必要なメタデータが含まれています。
- 展開パッケージを作成し、コマンドラインを使用してインストールします。

プロセス自動化する Visual Studio を設定する前のチュートリアルでデプロイ タスクがこれら 3 つのメソッドのすべてに適用されます。 これらのチュートリアルでは、これらのメソッドの最初の数値を使用します。 展開パッケージの使用方法の詳細については、次を参照してください。 [ASP.NET 配置コンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx)します。

発行前に、管理者モードで Visual Studio を実行していることを確認します。 (Windows 7 で**開始**] メニューの [は、ご使用の Visual Studio のバージョンのアイコンを右クリックし、選択**管理者として実行**)。管理者モードでは、のみときに発行している IIS、ローカル コンピューター上の発行に必要です。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクト (ContosoUniversity.DAL プロジェクトではなく) を右クリックし、選択、**発行**します。

**Web の発行**ウィザードが表示されます。

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

ドロップダウン リストで選択**&lt;新規.&gt;**.

**新しいプロファイル** ダイアログ ボックスが"Test"を入力し、クリックして**OK**します。

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

この名前は、先ほど作成したファイルを変換、Web.Test.config の中間のノードと同じです。 この対応は、このプロファイルを使用して発行するときに適用する Web.Test.config 変換が原因です。

ウィザードが自動的に進みます、**接続**タブ。

**サービス URL**ボックスに、入力*localhost*します。

**サイト/アプリケーション**ボックスに、入力*既定の Web サイト/ContosoUniversity*します。

**送信先 URL**ボックスに、入力`http://localhost/ContosoUniversity`します。

**送信先 URL**設定は必要ありません。 Visual Studio では、アプリケーションのデプロイが完了したら、この URL を既定のブラウザーが自動的に開きます。 展開した後に自動的に開くブラウザーをしたくない場合は、このボックスを空白のままにします。

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

クリックして**接続の検証**設定が正しいことと、ローカル コンピューターの IIS に接続できることを確認します。

緑色のチェック マークは、接続が成功したことを確認します。

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

クリックして**次**に進めておく、**設定** タブ。

**構成** ボックスの一覧を展開するビルド構成を指定します。 既定値は、リリースでは、これが目的です。

ままに、**転送先に追加のファイルを削除**チェック ボックスをオフにします。 これは、最初のデプロイであるためにがすべてのファイル変換先のフォルダーにまだします。

**データベース**セクションでの接続文字列 ボックスで、次の値を入力します**SchoolContext**:。

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

展開プロセスは、ために、デプロイした Web.config ファイルでのこの接続文字列を配置は**実行時にこの接続文字列を使用して**が選択されています。

また**SchoolContext**を選択します**適用の Code First Migrations**します。 このオプションを指定する配置の Web.config ファイルを構成する展開プロセスでは、`MigrateDatabaseToLatestVersion`初期化子。 この初期化子は、アプリケーションの展開後に最初に、データベースにアクセスするときに自動的にデータベースと最新バージョンに更新します。

接続文字列 ボックスで**DefaultConnection**、次の値を入力します。

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

ままに**Update database**オフにします。 アプリでの .sdf ファイルをコピーして、メンバーシップ データベースを展開します。\_データ、およびするには、配置プロセスがこのデータベースを他に何をしたくないです。

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

クリックして**次**に進めておく、**プレビュー**  タブ。

**プレビュー** ] タブで [**プレビューの開始**にコピーされるファイルの一覧を参照してください。

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

**[発行]** をクリックします。

管理者モードで Visual Studio でない場合は、アクセス許可エラーを示すエラー メッセージを取得可能性があります。 その場合は、Visual Studio を閉じます、管理者モードで開くし、再度パブリッシュしてください。

Visual Studio が管理者モードである場合、**出力**ウィンドウのレポートが正常にビルドして発行します。

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

ブラウザーは、ローカル コンピューターの IIS で実行されている Contoso University のホーム ページに自動的に開きます。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>テスト環境でのテスト

「(テスト)」の環境のインジケーターを示すことに注意してください。"(Dev)"ではなくすることを示しています、 *Web.config*環境インジケーターの変換が成功しました。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

実行、**学生**ページに配置されているデータベースに学生がないことを確認します。 Code First のデータベースを作成しを実行するため、読み込みに数分かかる場合がありますこのページを選択すると、`Seed`メソッド。 (しないをまだデータベースにアクセスするアプリケーションを試行していないので、ホーム ページで行ったときにします。)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

実行、 **Instructors** Code First シード インストラクター データでデータベースを確認します。

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

選択**受講者の追加**から、**学生** メニューの は、生徒を追加しで新しい学生を表示し、**受講者**データベースに正常に記述できることを確認する ページ:

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

**コース**メニューの **更新クレジット**します。 **更新クレジット**ページには、管理者のアクセス許可が必要ですので、**ログで**ページが表示されます。 以前 ("admin"と"Pa$ $w0rd") で作成した管理者アカウントの資格情報を入力します。 **更新クレジット**ページが表示され、前のチュートリアルで作成した管理者アカウントがテスト環境に正しくデプロイされたことを確認します。

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

いることを確認、 *Elmah*がプレース ホルダー ファイルのみのフォルダーが存在します。

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First Migrations に対する自動の Web.config の変更を確認します。

開く、 *Web.config*にデプロイされたアプリケーション内でファイル*C:\inetpub\wwwroot\ContosoUniversity*場所に Code First Migrations の構成、展開プロセスに自動的に確認できます最新バージョンにデータベースを更新します。

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

展開プロセスには、データベース スキーマの更新専用に使用する Code First Migrations に対する新しい接続文字列も作成されます。

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

この追加の接続文字列では、データベース スキーマの更新プログラムの 1 つのユーザー アカウントとアプリケーションのデータ アクセス用の別のユーザー アカウントを指定することができます。 たとえば、データベースを割り当てる\_Code First Migrations、および db には所有者ロール\_datareader と db\_datawriter の各アプリケーション ロール。 これは、多層防御の一般的なパターンを妨げる可能性のある悪意のあるコードからデータベース スキーマを変更するアプリケーションです。 (たとえば、これで行われる処理 SQL インジェクション攻撃が成功します。)このパターンは、これらのチュートリアルでは使用されません。 SQL Server Compact には適用されませんし、このシリーズの以降のチュートリアルでは、SQL Server に移行する場合は適用されません。 Cytanium サイト Cytanium で作成した SQL Server データベースにアクセスするための 1 つだけのユーザー アカウントを提供します。 自分のシナリオでこのパターンを実装するためにできない場合は、次の手順を実行してしないでください。

1. **設定**のタブ、 **Web の発行**ウィザード、データベースの完全スキーマの更新権限を持つユーザーを指定する接続文字列を入力し、オフ、**この接続文字列を使用して、実行時に**チェック ボックスをオンします。 これは、配置の Web.config ファイルで、`DatabasePublish`接続文字列。
2. アプリケーションは、実行時に使用する接続文字列を Web.config ファイルの変換を作成します。

開発用コンピューターに IIS にアプリケーションをデプロイして存在テストようになりました。 これは、ことを検証、展開プロセスでは、アプリケーションのコンテンツをコピー (デプロイしないファイルを除く)、適切な場所にも、Web Deploy IIS が正しく構成されて展開時にします。 次のチュートリアルがまだ実行されている展開のタスクを検索する 1 つ以上のテストを実行します。 上のフォルダーのアクセス許可の設定、 *Elmah*フォルダー。

## <a name="more-information"></a>説明

Visual Studio での IIS または IIS Express の実行方法の詳細については、次のリソースを参照してください。

- [IIS Express の概要](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net サイト。
- [IIS Express の概要](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx)Scott Guthrie のブログ。
- [方法: Visual Studio で Web プロジェクトに対して Web サーバーを指定する](https://msdn.microsoft.com/library/ms178108.aspx)します。
- [コアの相違点の間で IIS と ASP.NET 開発サーバー](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET サイト。
- [30 秒以内に、ASP.NET MVC または Web フォーム アプリケーションを IIS 7 をテスト](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx)Rick Anderson のブログ。 このエントリは、Visual Studio 開発サーバー (Cassini) でのテストがない理由で IIS Express をテストする場合ほど信頼性と IIS Express でのテストがない理由 IIS でテストする場合ほど信頼性の例を示します。

どのような問題については、中程度の信頼でアプリケーションを実行するときに発生する可能性が、参照してください[中程度の信頼で ASP.NET アプリケーションをホストしている](http://www.4guysfromrolla.com/articles/100307-1.aspx)Rolla サイトから 4 Guys にします。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [次へ](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
