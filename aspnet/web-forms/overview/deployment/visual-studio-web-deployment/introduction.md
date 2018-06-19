---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Visual Studio を使用した ASP.NET Web 展開: 概要 |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、展開する方法を示します (発行)、ASP.NET web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーをによって V を使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 1ff4cc3b0fa6ce7e6cdc833a0c2f7fea2050c4e6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890685"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Visual Studio を使用した ASP.NET Web 展開: 概要
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (発行)、ASP.NET web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーをで .NET 用 Azure SDK と Visual Studio 2012 を使用します。 ほとんどの手順は、Visual Studio 2013 用と似ています。
> 
> インターネット経由で人に使用できるようにするために web アプリケーションを開発するとします。 Web プログラミングのチュートリアルは、通常、開発用コンピューターで作業して何かを取得する方法を表示するしたされる直後後に停止します。 他を省略して、この一連のチュートリアルを開始: web アプリをビルド、テストする準備完了であるとします。 次の内容 これらのチュートリアルでは、テストは、ローカル開発用コンピューター上の IIS してから、Azure またはステージングと運用のサード パーティのホスティング プロバイダーに最初に配置する方法を示します。 展開するサンプル アプリケーションは、Entity Framework、SQL Server、および ASP.NET メンバーシップ システムを使用する web アプリケーション プロジェクトです。 サンプル アプリケーションが ASP.NET Web フォームを使用しますが、プロシージャには、ASP.NET MVC、Web API にも当てはまります。
> 
> これらのチュートリアルでは、Visual Studio での ASP.NET の使用方法を知っていると仮定します。 開始するに適してがない場合は、[基本的な ASP.NET Web フォーム チュートリアル](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md)または[基本的な ASP.NET MVC のチュートリアル](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)です。
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET 展開フォーラム](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)または[StackOverflow](http://stackoverflow.com)です。
> 
> このコンテンツはの無料電子書籍として利用も[TechNet 電子書籍ギャラリー](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio)です。


## <a name="overview"></a>概要

これらのチュートリアルでは、SQL Server データベースを含む ASP.NET web アプリケーションを配置する方法を説明します。 テストは、ローカル開発用コンピューター上の IIS にし、Azure App Service と Azure SQL Database にステージングと運用環境で Web アプリに、まず展開をします。 ワンクリック発行、Visual Studio を使用して展開する方法について説明し、コマンドラインを使用して展開する方法について説明します。

チュートリアルの数は、圧倒されて、展開プロセスを行うことがあります。 実際には、基本的な手順は単純です。 ただし、実際の状況では、多くの場合、必要がありますを特別な展開タスクを行うには、たとえば、ターゲット サーバーでフォルダーのアクセス許可を設定します。 いくつかのチュートリアルでは、実際のアプリケーションを正常に展開を妨げる可能性のある情報を離れることを期待して、これらの追加タスクを示すおしました。

チュートリアルが順番に実行するように設計し、各部分が、前のパートを上に構築します。 自分の状況に関連性のない部分をスキップすることができますが、後のチュートリアルの手順を調整する必要があります。

## <a name="intended-audience"></a>想定読者

チュートリアルでは、目的としています環境で作業する ASP.NET 開発者場所。

- 運用環境は、Azure App Service Web Apps またはサード パーティのホスティング プロバイダーです。
- 展開では、継続的インテグレーション プロセスに制限はありませんが、Visual Studio から直接行うことができます。

デプロイ[ソース コントロール](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)を使用して、[した継続的配信](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)プロセスは、コマンドラインから展開する方法を示している 1 つのチュートリアルを除くこれらのチュートリアルでは説明しません。 継続的な配信方法については、次のリソースを参照してください。

- [継続的インテグレーションと継続的な配信 (Windows Azure と実際のクラウド アプリのビルド)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Azure App service web アプリを配置します。](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)(古いセットの Visual Studio 2010 では、エンタープライズ環境に関する有用な情報がまだ用に記述されたチュートリアルです)。

## <a name="using-a-third-party-hosting-provider"></a>サード パーティのホスティング プロバイダーを使用します。

チュートリアルでは、Azure アカウントを設定して、アプリケーションは、ステージングと運用環境の Azure App service Web アプリを展開するプロセスを実行します。 ただし、任意のサード パーティのホスティング プロバイダーに展開するため、同じ基本的な手順を使用できます。 チュートリアルは、一意なプロセスを Azure に移動、場所、ことを説明し、サード パーティのホスティング プロバイダーで予想されるどのような相違点をお勧めします。

## <a name="deploying-web-app-projects"></a>Web アプリのプロジェクトの配置

サンプル アプリケーションをダウンロードして、これらのチュートリアルの配置は、Visual Studio web アプリケーション プロジェクトです。 ただし、最新版をインストールする場合[for Visual Studio Web 発行更新](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx)、web アプリのプロジェクトの場合、同じ展開方法とツールを使用することができます。

## <a name="deploying-aspnet-mvc-projects"></a>ASP.NET MVC プロジェクトの配置

サンプル アプリケーションは、ASP.NET Web フォーム プロジェクトを行う方法を説明するすべてのものはも ASP.NET MVC には適用します。 Visual Studio の MVC プロジェクトは、web アプリケーション プロジェクトの単なる別の形式です。 唯一の違いは、ことを ASP.NET MVC またはこれのターゲット バージョンをサポートしていないホスティング プロバイダーに配置する場合は、する必要があります、適切なインストールしてください ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)、 [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)、または[MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc))、プロジェクトの NuGet パッケージです。

## <a name="programming-language"></a>プログラミング言語

C# サンプル アプリケーションを使用が、チュートリアルには、C# の場合の知識は不要し、チュートリアルで示すように展開方法は、言語固有ではないです。

## <a name="database-deployment-methods"></a>データベースの展開方法

Visual Studio での web デプロイに加えて、SQL Server データベースを導入することが 3 つの方法があります。

- Entity Framework Code First Migrations
- DbDacFx Web Deploy プロバイダー
- DbFullSql Web 配置プロバイダー

このチュートリアルでは、これらのメソッドの最初の 2 つを使用します。 DbFullSql Web Deploy プロバイダーは、SQL Server Compact から SQL Server への移行など特定のシナリオを除くは推奨されなく従来の方法です。

このチュートリアルで示したメソッドはいない SQL Server Compact の SQL Server データベースです。 SQL Server Compact データベースを配置する方法については、次を参照してください。 [Visual Studio Web 配置を SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)です。

このチュートリアルで示した方法では、Web Deploy を使用することが条件とメソッドを公開します。 FTP、ファイル システム、または FPSE などのメソッドを別にパブリッシュする場合は、次を参照してください。 [web アプリケーションの展開から個別にデータベースを展開する](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate)で Visual Studio と ASP.NET の Web 展開のコンテンツ マップします。

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First Migrations

Entity Framework バージョン 4.3 では、Microsoft は、Code First Migrations を紹介します。 Code First Migrations は、データ モデルに増分変更を加えると、それらの変更、データベースへの反映のプロセスを自動化します。 Code First の以前のバージョンでは、通常、Entity Framework を削除し、データ モデルを変更するたびにデータベースを再作成を使用できます。 これは開発に問題があるテスト データを簡単に再作成されますが、実稼働環境で通常するデータベースを削除しない限り、データベース スキーマを更新するためです。 移行の機能によって、Code First を削除し、再作成することがなく、データベースを更新します。 Code First が自動的に必要なスキーマ変更を加える方法を決定することができますか、変更内容をカスタマイズするコードを記述することができます。 Code First Migrations の概要については、次を参照してください。 [Code First Migrations](https://msdn.microsoft.com/library/hh770484.aspx)です。

Web プロジェクトを展開しているときに、Visual Studio は、Code First Migrations で管理されているデータベースの配置のプロセスを自動化できます。 発行プロファイルを作成するときに、実行 Code First Migrations (アプリケーション開始時に実行されます) というラベルが付いたチェック ボックスを選択します。 この設定により、展開プロセス Code First が使用されるように、移行先サーバー上のアプリケーションの Web.config ファイルを自動的に構成するのには、`MigrateDatabaseToLatestVersion`初期化子のクラスです。

Visual Studio は、展開プロセス中に、データベースのすべてのものを実行してされません。 展開後に初めてアクセスするデータベースと、展開されたアプリケーション、Code First 自動的にデータベースを作成またはデータベース スキーマを最新バージョンに更新します。 アプリケーションでは、移行シード メソッドを実装する場合は、データベースを作成またはスキーマが更新された後に、メソッドが実行されます。

このチュートリアルでは、アプリケーション データベースを配置するのに Code First Migrations を使用します。

### <a name="the-dbdacfx-web-deploy-provider"></a>DbDacFx Web Deploy プロバイダー

Entity Framework Code First によって管理されていない SQL Server データベースの発行プロファイルを構成するときに、データベースの更新が付いてチェック ボックスを選択できます。 初期の展開時に、dbDacFx プロバイダーは、ソース データベースに一致するコピー先データベースにテーブルおよびその他のデータベース オブジェクトを作成します。 後続のデプロイでは、元とコピー先のデータベース間で相違点は、プロバイダーを決定し、ソース データベースと一致する変換先データベースのスキーマを更新します。 既定では、プロバイダーは、テーブルまたは列が削除されるときなど、データ損失につながる変更を加えませんは。

このメソッドはデータベース テーブル内のデータの展開を自動化していませんを実行して、Visual Studio の展開時に実行を構成するためのスクリプトを作成することができます。 展開時にスクリプトを実行する別の理由は、データの損失になるために自動的に行うことができないスキーマ変更を加えるです。

このチュートリアルで使用する dbDacFx プロバイダー、ASP.NET メンバーシップ データベースを展開します。

## <a name="troubleshooting-during-this-tutorial"></a>このチュートリアルでのトラブルシューティング

展開時に、エラーの発生時、または展開されたサイトが正しく実行されない場合は、エラー メッセージは常に明確なソリューションを提供しません。 一般的な問題のシナリオで役立つ、[リファレンス ページのトラブルシューティング](troubleshooting.md)は使用できます。 エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、トラブルシューティングのページを確認することを確認します。

## <a name="comments-welcome"></a>コメントを開始します。

チュートリアルでは、上のコメントはへようこそ とチュートリアルが更新されたときにすべての作業量になりますチュートリアルのコメントに用意された機能強化に関するアカウントの修正または提案を考慮します。

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルは、次の製品用に記述されました。

- Windows 8 または Windows 7。
- Visual Studio 2012 または Visual Studio 2012 の Express for Web と[最新の更新プログラム](https://go.microsoft.com/fwlink/?LinkId=272486)です。
- [Azure SDK for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Visual Studio 2010 SP1 または Visual Studio 2013 を使用して、チュートリアルを行うことができるが、いくつかのスクリーン ショットは、異なるいくつかの機能になります。

Visual Studio 2013 を使用している場合は、インストール[Azure SDK for Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322)です。

Visual Studio 2010 SP1 を使用している場合は、次のソフトウェアをインストールします。

- [Azure SDK for Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx)です。

SDK の依存関係の数を既に持っているによってコンピューターに、Azure SDK をインストールする数分から 30 分以上に、時間がかかる可能性があります。 Azure への代わりにサード パーティ製ホスティング プロバイダーにパブリッシュする場合、SDK には、Visual Studio web に最新の更新プログラムが含まれているために、機能を公開する場合でもは、Azure SDK を作成する必要があります。

> [!NOTE]
> このチュートリアルは、Azure SDK のバージョン 1.8.1 で記述されています。 その後、追加の機能を持つ新しいバージョンがリリースされました。 チュートリアルでは、これらの機能とその詳細情報を持つリソースへのリンクは言うまでも更新されました。


手順およびスクリーン ショットは Windows 8 に基づくが、チュートリアルでは、Windows 7 の相違点をについて説明します。

他のソフトウェアが、チュートリアルを完了するために必要な必要はありませんがまだインストールしています。 このチュートリアルでは必要なときにインストールするための手順を説明します。

## <a name="download-the-sample-application"></a>サンプル アプリケーションをダウンロードします。

展開するアプリケーションは Contoso 大学の名前はおよびを既に作成されています。 疎で説明されている Contoso 大学アプリケーションに基づいて、大学の web サイトの簡素化されたバージョンは、 [Entity Framework、ASP.NET サイトのチュートリアル](https://asp.net/entity-framework/tutorials)です。

インストールの前提条件がある場合は、ダウンロード、 [Contoso 大学 web アプリケーション](https://go.microsoft.com/fwlink/p/?LinkId=282627)です。 *.Zip*ファイルには、プロジェクトの複数のバージョンが含まれています。 チュートリアルの手順を使用するには、c# フォルダーにあるプロジェクトを起動します。 プロジェクトの外観をチュートリアルの最後を参照してください、ContosoUniversity 終了フォルダーにプロジェクトを開きます。

チュートリアルの手順全体を操作するプロジェクトを準備するには、次の手順を実行します。

1. Visual Studio プロジェクトを操作するために使用する任意のフォルダーで ContosoUniversity をという名前のフォルダー内の c# フォルダーから ContosoUniversity ソリューション ファイルを保存します。

    既定では、Visual Studio 2012 用の次のフォルダーを示します。

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (このチュートリアルのスクリーン ショットのプロジェクト フォルダーがルート ディレクトリ内にある、 `C`: ドライブです)。
2. Visual Studio を起動し、プロジェクトを開きます。
3. **ソリューション エクスプ ローラー**ソリューションを右クリックし、クリックして、 **EnableNuGet パッケージの復元**です。
4. ソリューションをビルドします。
5. コンパイル エラーが発生した場合は、NuGet パッケージを手動で復元します。

    1. **ソリューション エクスプ ローラー**、ソリューションを右クリックし、クリックして**Manage NuGet Packages for Solution**です。
    2. 上部にある、 **NuGet パッケージの管理** ダイアログ ボックスが表示されます**一部の NuGet パッケージは、このソリューションにありません。復元する をクリックします。** クリックして、**復元**ボタンをクリックします。
    3. ソリューションをリビルドします。
6. Ctrl キーを押し、f5 キーを押してアプリケーションを実行します。

    アプリケーションは、Contoso 大学のホーム ページを開きます。

    ![ホーム ページの開発](introduction/_static/image1.png)

    (Visual Studio の SQL Server Express LocalDB インスタンスを起動中に、待機時間にすることがありますとして場合がありますタイムアウト エラーが処理に時間がかかりすぎています。 そのケースだけで、プロジェクト再度開始します。)

Web サイトのページは、メニュー バーからアクセスできる、次の関数を実行できます。

- 学生の統計情報 (バージョン情報 ページ) を表示します。
- 表示、編集、削除、および受講者を追加します。
- 編集のコースを表示します。
- 編集講習においてインストラクターを表示します。
- 部門の編集を表示します。

いくつかの代表的なページのスクリーン ショットを次に示します。

![受講者 ページの開発](introduction/_static/image2.png)

![ページ デベロッパーの受講者を追加します。](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>展開に影響するアプリケーションの機能を確認します。

アプリケーションの次の機能は、展開する方法、展開する必要があるかに影響します。 これらの各操作は、系列内の次のチュートリアルでさらに詳しく説明します。

- Contoso 大学では、SQL Server データベースを使用して、学生やインストラクター名などのアプリケーション データを格納します。 データベースには、テスト データと、実稼働データの両方が含まれているし、実稼働環境に展開するときにテスト データを除外する必要があります。
- アプリケーションでは、SQL Server データベースにユーザー アカウント情報を格納、ASP.NET メンバーシップ システムを使用します。 アプリケーションでは、制限された情報へのアクセスを持つ管理者ユーザーを定義します。 テスト アカウントを持たないが、管理者アカウントを使用して、メンバーシップ データベースを展開する必要があります。
- アプリケーションでは、サード パーティ製のエラー ログ機能およびユーティリティのレポートを使用します。 このユーティリティは、アプリケーションと共に配置する必要がありますアセンブリで提供されます。
- エラーのログ記録ユーティリティは、ファイル フォルダーに、XML ファイルにエラー情報を書き込みます。 配置済みのサイトで ASP.NET を実行するアカウントが、このフォルダーの書き込みアクセス許可をこのフォルダーを展開から除外する必要があること確認する必要があります。 (それ以外の場合、テスト環境からのエラー ログのデータを実稼働環境に展開するとや、実稼働のエラー ログ ファイルを削除する可能性があります)。
- アプリケーションに変更する必要がある設定が含まれています、展開済みで*Web.config*先の環境 (テスト、ステージング、または運用) とその他の設定によっては、ビルドの変更が必要に応じてファイル(デバッグまたはリリース) を構成します。
- Visual Studio ソリューションには、クラス ライブラリ プロジェクトが含まれています。 このプロジェクトで生成されるアセンブリのみを展開すると、プロジェクト自体ではありません。

## <a name="summary"></a>まとめ

系列の最初のチュートリアルでは、サンプルの Visual Studio プロジェクトをダウンロードし、アプリケーションを展開する方法に影響するサイトの機能を確認しました。 次のチュートリアルでは、展開準備で自動的に処理するのには次の作業の一部を設定します。 他のユーザーを処理する手動でします。

> [!div class="step-by-step"]
> [次へ](preparing-databases.md)
