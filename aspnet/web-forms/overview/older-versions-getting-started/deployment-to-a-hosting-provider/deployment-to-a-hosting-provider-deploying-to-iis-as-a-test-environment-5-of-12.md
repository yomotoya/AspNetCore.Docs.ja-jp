---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: "SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します 5/12 - テスト環境として IIS に展開する |。Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: a7995844ee6ed19efa130c4f6c019214d6652ea7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します 5/12 - テスト環境として IIS に展開する。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでは、ローカル コンピューターの IIS に ASP.NET web アプリケーションを展開する方法を示します。

アプリケーションを開発するときに、Visual Studio で実行して、通常でテストします。 既定では、つまり、Visual Studio 開発サーバー (Cassini とも呼ばれます) を使用しています。 Visual Studio 開発サーバーでは、簡単に、Visual Studio での開発中にテストが、IIS とまったく同じでは機能しません。 その結果、可能であればアプリケーションが、Visual Studio で、テスト、ホスティング環境で IIS に配置された場合に失敗する場合に、正常に実行されます。

次の方法でより信頼性の高いアプリケーションをテストすることができます。

1. 開発時に Visual Studio でテストするときに、Visual Studio 開発サーバーの代わりに IIS Express または完全な IIS を使用します。 IIS 下で、サイトがどのように実行される、このメソッドがより正確にエミュレート一般にします。 ただし、このメソッドはテスト、展開プロセスや、展開プロセスの結果が正しく実行される検証。
2. 開発用コンピューター上の IIS にアプリケーションを配置するには、後で、実稼働環境に展開に使用するのと同じプロセスを使用します。 このメソッドは、IIS 下で、アプリケーションが正しく実行するだけでなく展開プロセスを検証します。
3. 運用環境にできるだけ近いであるテスト環境へのアプリケーションを展開します。 これらのチュートリアルの運用環境は、サード パーティのホスティング プロバイダーであるため、理想的なテスト環境は、ホスティング プロバイダーを持つ 2 つ目のアカウントになります。 この 2 つ目のアカウントを使用して、テスト目的でのみはように設定して、実稼働アカウントと同じ方法。

このチュートリアルでは、オプション 2 の手順を使用します。 オプション 3 のガイダンスの最後に記載されて、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルでは、このチュートリアルの最後には、オプション 1 のリソースへのリンクとします。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。

## <a name="configuring-the-application-to-run-in-medium-trust"></a>中程度の信頼で実行するようにアプリケーションを構成します。

IIS をインストールして展開する前に、サイトの実行がなるより一般的な共有ホスティング環境ではこれと同じようにするために、Web.config ファイルの設定を変更します。

ホスティング プロバイダーで web サイトを実行する通常*中程度の信頼*、意味を行うには許可されていないいくつかの点があります。 たとえば、アプリケーション コードことはできません、Windows レジストリにアクセスおよびことはできません読み取りまたは書き込み、アプリケーションのフォルダー階層外にあるファイル。 既定では、アプリケーションで実行*信頼性の高い*ローカル コンピューターでができることを意味アプリケーション可能性がありますが、実稼働環境に展開するときに失敗することの作業を行います。 そのためより正確に反映されているテスト環境を運用環境に中程度の信頼で実行するアプリケーションを構成します。

アプリケーションの Web.config ファイルで追加、**信頼**内の要素、 **system.web**要素は、この例で示すようにします。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

アプリケーションは、ローカル コンピューター上でも今すぐ IIS で中程度の信頼で実行されます。 この設定では、アプリケーション コードは何かが実稼働環境で失敗することによってできるだけ早くしようとするとをキャッチすることができます。

> [!NOTE]
> Entity Framework Code First Migrations を使用している場合は、バージョン 5.0 を行ったことを確認またはインストールされた後でください。 Entity Framework バージョン 4.3 での移行には、データベース スキーマを更新するために完全な信頼が必要です。


## <a name="installing-iis-and-web-deploy"></a>インストールを実行する IIS Web 配置と

開発用コンピューター上の IIS に展開するには IIS が必要し、Web Deploy をインストールします。 これらは、Windows 7 の既定の構成には含まれません。 IIS および Web デプロイの両方をまだインストールして場合、は、次のセクションに進んでください。

使用して、 [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) Web Platform Installer が IIS の推奨構成をインストールし、IIS と Web の前提条件が自動的にインストールするために、IIS および Web Deploy をインストールすることをお勧めは、必要に応じてを展開します。

IIS および Web Deploy をインストールする Web Platform Installer を実行するには、次のリンクを使用します。 既にをインストールした IIS、Web デプロイまたはその必須コンポーネントのいずれかの場合、Web Platform Installer は、何が足りないのみをインストールします。

- [IIS および Web Deploy WebPI を使用したインストールします。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>既定のアプリケーション プールを .NET 4 に設定します。

IIS をインストールすると、実行**IIS マネージャー** .NET Framework version 4 が既定のアプリケーション プールに割り当てられているかどうかを確認します。

Windows から**開始**メニューの **実行**"inetmgr"を入力し、クリックして**ok**です。 (場合、**実行**コマンドに含まれていない、**開始** メニューを開くには、Windows キーと R を押すことができます。 または、タスク バーを右クリックし、をクリックして**プロパティ**を選択、 **スタート メニュー**  タブで、をクリックして**カスタマイズ**を選択して**コマンドを実行**)。

**接続** ウィンドウで、サーバー ノードを展開し、選択**アプリケーション プール**です。 **アプリケーション プール** ウィンドウで場合、 **DefaultAppPool**に .NET framework バージョン 4、次の図のように割り当てると、次のセクションにスキップします。

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

2 つのアプリケーション プールを参照してください、.NET Framework 2.0 に設定されます両方の場合は、IIS で ASP.NET 4 をインストールする必要があります。

- 右クリックして、コマンド プロンプト ウィンドウを開く**コマンド プロンプト**windows**開始**メニューを選択して**管理者として実行**です。 実行して[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)を次のコマンドを使用して、IIS で ASP.NET 4 をインストールします。 (64 ビット システムでは、"Framework64"と"Framework"を置き換えてください)。

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    このコマンドは、.NET Framework 4 の新しいアプリケーション プールを作成しますが、2.0 にも既定のアプリケーション プールは設定されます。 デプロイするアプリケーションを対象とする .NET 4 そのアプリケーション プールに .NET 4 にアプリケーション プールを変更する必要があるためです。

閉じた場合**IIS マネージャー**、もう一度実行、サーバー ノードを展開およびクリックして**アプリケーション プール**を表示する、**アプリケーション プール**ペインをもう一度です。

**アプリケーション プール** ウィンドウで、をクリックして**DefaultAppPool**、し、、**アクション**ペイン をクリック**基本設定**です。

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

**アプリケーション プールの編集** ダイアログ ボックスで、変更**.NET Framework のバージョン**に**.NET Framework v4.0.30319**  をクリック**OK**です。

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

IIS に発行する準備が整いました。

## <a name="publishing-to-iis"></a>IIS に発行します。

いくつかの方法が Visual Studio 2010 および Web Deploy を使用してを展開することができます。

- ワンクリック発行 Visual Studio を使用します。
- 作成、*展開パッケージ*し、IIS マネージャーの UI を使用してインストールします。 展開パッケージの構成要素、 *.zip*をすべてのファイルと IIS のサイトのインストールに必要なメタデータを含むファイルです。
- 展開パッケージを作成し、コマンドラインを使用してインストールします。

プロセスを経てきましたを自動化する Visual Studio を設定する前のチュートリアルでの展開タスクがこれら 3 つのメソッドのすべてに適用されます。 これらのチュートリアルでは、これらのメソッドの最初の数値を使用します。 展開パッケージを使用する方法の詳細については、次を参照してください。 [ASP.NET 展開のコンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx)です。

発行前に、管理者モードで Visual Studio を実行していることを確認します。 (Windows 7 で**開始**メニューで、ご使用の Visual Studio のバージョンのアイコンを右クリックして**管理者として実行**)。管理者モードでは、のみときにパブリッシュする IIS、ローカル コンピューター上の発行に必要です。

**ソリューション エクスプ ローラー**ContosoUniversity プロジェクト (ContosoUniversity.DAL プロジェクトではない) を右クリックし、選択、**発行**です。

**Web の発行**ウィザードが表示されます。

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

ドロップダウン リストで選択**&lt;新規しています.&gt;**.

**新しいプロファイル**ダイアログ ボックスで、"Test"を入力し、をクリックして**OK**です。

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

この名前は、先ほど作成したファイルを変換、Web.Test.config の中間のノードと同じです。 この通信は、何が原因でこのプロファイルを使用して発行するときに適用する Web.Test.config 変換です。

ウィザードが自動的に進みます、**接続**タブです。

**サービス URL**ボックスに、入力*localhost*です。

**サイト/アプリケーション**ボックスに、入力*既定の Web サイト/ContosoUniversity*です。

**送信先 URL**ボックスに、入力`http://localhost/ContosoUniversity`です。

**送信先 URL**設定は必要ありません。 Visual Studio では、アプリケーションの展開が完了すると、自動的にこの URL に、既定のブラウザーを開きます。 展開後に自動的に開く、ブラウザーがたくない場合は、このボックスを空白のままにします。

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

をクリックして**接続の検証**設定が正しいことと、ローカル コンピューター上の IIS に接続できることを確認します。

緑のチェック マークは、接続が成功したことを確認します。

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

をクリックして**次**に進める、**設定**タブです。

**構成** ボックスの一覧を展開するビルド構成を指定します。 既定値は、リリースは、よろしいですか。

ままにして、**先で追加ファイルを削除**チェック ボックスをオフにします。 これは、最初の展開であるためにされませんすべてのファイル コピー先フォルダーにまだです。

**データベース**セクションで、接続文字列のボックスで、次の値を入力**SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

展開プロセスは、ために、展開済みの Web.config ファイルのこの接続文字列を格納する**実行時にこの接続文字列を使用して**が選択されています。

でも**SchoolContext****適用の Code First Migrations**です。 このオプションの設定により、展開プロセスを指定する展開済みの Web.config ファイルを構成するのには、`MigrateDatabaseToLatestVersion`初期化子。 この初期化子を使用最新バージョンにアプリケーションの展開後に最初に、データベースにアクセスするときのデータベースが自動的に更新します。

接続文字列 ボックスで**DefaultConnection**、次の値を入力します。

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

ままにして**更新データベース**オフにします。 アプリでの .sdf ファイルのコピーによって、メンバーシップ データベースを展開する予定\_をこのデータベースの他の操作を行うには、展開プロセスをしたくないデータ、およびします。

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

をクリックして**次**に進める、**プレビュー**タブです。

**プレビュー**  タブで、をクリックして**開始プレビュー**コピーされるファイルの一覧を表示します。

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

**[発行]**をクリックします。

Visual Studio が管理者モードでない場合は、アクセス許可エラーを示すエラー メッセージを取得可能性があります。 その場合は、Visual Studio を閉じます、管理者モードで開き、再度発行します。

Visual Studio は、管理者モードの場合、**出力**ウィンドウのレポートが正常にビルドし、発行します。

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

ブラウザーは、ローカル コンピューター上の IIS で実行されている Contoso 大学のホーム ページに自動的に開きます。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>テスト環境でのテスト

環境のインジケーターは、"(Test)"を表示"(Dev)"ではなくなることを示しています、 *Web.config*環境のインジケーターの変換が成功しました。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

実行、**受講者**ページに配置されるデータベースに受講者がいないことを確認します。 このページを選択すると、Code First、データベースを作成してから実行するための読み込みにしばらく時間がかかる場合があります、`Seed`メソッドです。 (そのようになっていませんため、まだデータベースにアクセスしようとしていないアプリケーション ホーム ページにあった場合。)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

実行、**講習においてインストラクター**ページを Code First シード インストラクター データとデータベースを確認します。

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

選択**受講者を追加**から、**受講者** メニューの 、受講者を追加し、新しい学生を表示、**受講者**データベースに正常に記述できることを確認する ページ:

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

**コース**メニューの **更新クレジット**です。 **更新クレジット**ページには、管理者のアクセス許可が必要がありますので、**ログで**ページが表示されます。 以前 ("admin"および"Pa$ $w0rd") で作成した管理者アカウントの資格情報を入力します。 **更新クレジット**ページが表示されたら、前のチュートリアルで作成した管理者アカウントが、テスト環境に正しく展開されたことを確認します。

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

いることを確認、 *Elmah*にプレース ホルダー ファイルのみのフォルダーが存在します。

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First Migrations を自動 Web.config の変更を確認

開く、 *Web.config*に展開されたアプリケーション内でファイル*C:\inetpub\wwwroot\ContosoUniversity*し、ここで、展開プロセスを Code First Migrations が自動的に構成を参照してください最新バージョンにデータベースを更新します。

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

展開プロセスには、Code First Migrations データベース スキーマの更新をのみ使用するための新しい接続文字列も作成されます。

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

この追加の接続文字列では、データベース スキーマの更新プログラムの 1 つのユーザー アカウントとアプリケーション データへのアクセスを別のユーザー アカウントを指定することができます。 たとえば、データベースを割り当てることが\_Code First Migrations、および db には所有者ロール\_datareader および db\_datawriter のロール、アプリケーションにします。 これは、悪意のあるアプリケーションのコードで、データベース スキーマを変更するを防止する一般的な多層防御パターンです。 (たとえば、これで行われる処理 SQL インジェクション攻撃が成功します。)このパターンは、これらのチュートリアルでは使用されません。 SQL Server Compact には適用されませんし、このシリーズの後のチュートリアルでは、SQL Server に移行するときに適用されません。 Cytanium サイトは、Cytanium で作成した SQL Server データベースにアクセスするための 1 つのユーザー アカウントを提供します。 場合は、シナリオではこのパターンを実装することは、次の手順を実行しないでください。

1. **設定**のタブ、 **Web の発行**ウィザード、完全なデータベース スキーマの更新権限を持つユーザーを指定する接続文字列を入力し、オフ、**この接続文字列を使用実行時に**チェック ボックスをオンします。 これは展開済みの Web.config ファイルで、`DatabasePublish`接続文字列。
2. アプリケーションが実行時に使用する接続文字列の Web.config ファイルの変換を作成します。

今すぐ開発用コンピューター上の IIS にアプリケーションを配置してテストしたことがあります。 これは、展開プロセスが、適切な場所 (配置をしないようにするには、ファイルを除く) に、アプリケーションのコンテンツをコピーされもその Web Deploy 構成されている IIS 正しく配置時に確認します。 チュートリアルでは、[次へ]、展開するタスクがまだ行われていないを検索する 1 つの複数のテストを実行します。 上のフォルダーのアクセス許可の設定、 *Elmah*フォルダーです。

## <a name="more-information"></a>説明

Visual Studio での IIS または IIS Express の実行については、次のリソースを参照してください。

- [IIS Express の概要](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net サイトです。
- [IIS Express の概要](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx)Scott Guthrie のブログです。
- [方法: Visual Studio で Web プロジェクトの Web サーバーを指定](https://msdn.microsoft.com/library/ms178108.aspx)です。
- [コアの相違点の間で IIS と ASP.NET 開発サーバー](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET サイトです。
- [30 秒以内に、ASP.NET MVC または IIS 7 で Web フォーム アプリケーションをテスト](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx)Rick Anderson のブログです。 このエントリは、Visual Studio 開発サーバー (Cassini) でのテストが理由 IIS Express で、テストとして信頼できると、IIS Express でのテストが理由として、IIS でのテストと信頼性の高いの例を提供します。

どのような問題については、中程度の信頼でアプリケーションを実行するときに発生する可能性を参照してください[中程度の信頼で ASP.NET アプリケーションをホストしている](http://www.4guysfromrolla.com/articles/100307-1.aspx)Rolla サイトから 4 Guys にします。

>[!div class="step-by-step"]
[前へ](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
[次へ](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
