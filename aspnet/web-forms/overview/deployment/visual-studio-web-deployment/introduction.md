---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Visual Studio を使用して ASP.NET Web 展開: 概要 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーをによって V を使用しています.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: a227f6564607ed9e909fc4d6297d7370f8fb62b5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838892"
---
<a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Visual Studio を使用して ASP.NET Web 展開: 概要
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーをで Azure sdk for .NET Visual Studio 2012 を使用します。 ほとんどの手順は、Visual Studio 2013 用に似ています。
> 
> インターネット経由で人に使用できるようにするために web アプリケーションを開発します。 Web プログラミングのチュートリアルは、通常、開発用コンピューターで動作するものを取得する方法を紹介しましたが直後後に停止します。 この一連のチュートリアルでは、省略、他のユーザーが開始されます。 web アプリをビルド、テストする準備が整ったら。 次の内容 これらのチュートリアルでは、最初にテストするには、ローカル開発用コンピューターに IIS してから、Azure やホスティング プロバイダーにサード パーティ製のステージングと運用環境に展開する方法を示します。 デプロイするサンプル アプリケーションは、Entity Framework、SQL Server、および ASP.NET メンバーシップ システムを使用する web アプリケーション プロジェクトです。 サンプル アプリケーションが ASP.NET Web フォームを使用しますに示す手順は、ASP.NET MVC と Web API にも適用されます。
> 
> これらのチュートリアルでは、Visual Studio で ASP.NET を使用する方法を理解すると仮定します。 ない場合は、開始点としてを[基本的な ASP.NET Web フォームのチュートリアル](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md)または[基本的な ASP.NET MVC チュートリアル](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)します。
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET 展開に関するフォーラム](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)または[StackOverflow](http://stackoverflow.com)します。
> 
> このコンテンツはの無料電子書籍としても[TechNet の電子書籍ギャラリー](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio)します。


## <a name="overview"></a>概要

これらのチュートリアルでは、SQL Server データベースを含む ASP.NET web アプリケーションを配置する方法を説明します。 テストは、ローカル開発用コンピューター上の iis Web アプリを Azure App Service とステージング環境と運用のための Azure SQL Database にし、最初にデプロイします。 ワンクリック発行、Visual Studio を使用してデプロイする方法について説明し、コマンドラインを使用してデプロイする方法について説明します。

チュートリアルの数は、一見大変な展開プロセスを行う可能性があります。 実際には、基本的な手順は簡単です。 ただし、実際の状況では、多くの場合、行う必要がある特別な展開タスク-たとえば、対象サーバーでフォルダーのアクセス許可を設定します。 チュートリアルは、実際のアプリケーションを正常に展開を妨げる可能性のある情報を忘れないでくださいことを期待して、これらの追加タスクのいくつか説明しました。

チュートリアルは順番に実行するように設計し、前のパートに各部分をビルドします。 自分の状況に関連性のない部分をスキップすることができますが、後のチュートリアルで手順を調整する必要があります。

## <a name="intended-audience"></a>対象読者

環境で作業する ASP.NET 開発者が目的としたチュートリアルでは、場所。

- 運用環境は、Azure App Service Web Apps またはサード パーティのホスティング プロバイダーです。
- 展開では、継続的インテグレーション プロセスに制限はありませんが、Visual Studio から直接行うことができます。

デプロイ[ソース管理](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)を使用して、[継続的デリバリー](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)プロセスは、コマンドラインからデプロイする方法を示す 1 つのチュートリアルを除くこれらのチュートリアルでは説明しません。 継続的デリバリーについては、次のリソースを参照してください。

- [継続的インテグレーションと継続的デリバリー (Windows Azure で現実世界のクラウド アプリの構築)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Azure App Service で web アプリをデプロイします。](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [エンタープライズのシナリオで Web アプリケーションを展開する](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)(古いセットの Visual Studio 2010 では、エンタープライズ環境の有用な情報がまだ用に作成されたチュートリアルです)。

## <a name="using-a-third-party-hosting-provider"></a>サード パーティのホスティング プロバイダーを使用します。

チュートリアルでは、Azure のアカウントを設定して、ステージングと運用のための Azure App Service で Web アプリにアプリケーションを配置するプロセスを実行します。 ただし、任意のサード パーティのホスティング プロバイダーに展開するため、同じ基本的な手順を使用することができます。 チュートリアルでは、Azure にプロセスを一意に移動、場所、ことを説明し、どのような違いがサード パーティのホスティング プロバイダーに期待できることをお勧めします。

## <a name="deploying-web-app-projects"></a>Web アプリ プロジェクトの配置

サンプル アプリケーションをダウンロードしてこれらのチュートリアルのデプロイとは、Visual Studio web アプリケーション プロジェクトです。 ただし、最新版をインストールする場合[for Visual Studio Web 発行の更新](https://msdn.microsoft.com/tr-tr/library/jj161045.aspx)、web アプリ プロジェクトを同じ展開方法とツールを使用することができます。

## <a name="deploying-aspnet-mvc-projects"></a>ASP.NET MVC プロジェクトの配置

サンプル アプリケーションは、ASP.NET Web フォーム プロジェクトがもは ASP.NET MVC に適用可能なすべてを行う方法について説明します。 Visual Studio MVC プロジェクトでは、web アプリケーション プロジェクトのもう 1 つの形式です。 唯一の違いは、ASP.NET MVC またはこれのターゲット バージョンをサポートしていないホスティング プロバイダーにデプロイする場合おく必要があります、適切なインストールしておきます ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0)、 [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)、または[MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) プロジェクトに NuGet パッケージ。

## <a name="programming-language"></a>プログラミング言語

C# サンプル アプリケーションを使用が、チュートリアルには、c# の知識が必要としないと、チュートリアルで示すように、展開方法は、言語固有ではないです。

## <a name="database-deployment-methods"></a>データベースの展開方法

3 つの方法を Visual Studio で web デプロイと共に SQL Server データベースを展開することができます。

- Entity Framework Code First Migrations
- DbDacFx Web 配置プロバイダー
- DbFullSql Web 配置プロバイダー

このチュートリアルでは、これらのメソッドの最初の 2 つを使用します。 DbFullSql Web Deploy プロバイダーは、SQL Server Compact から SQL Server への移行など特定のシナリオ以外は推奨されなくをレガシ メソッドです。

このチュートリアルで示されているメソッドはない SQL Server Compact、SQL Server データベースです。 SQL Server Compact データベースをデプロイする方法については、次を参照してください。 [Visual Studio Web 配置で SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md)します。

このチュートリアルで示されているメソッドの Web Deploy を使用することが必要なメソッドを公開します。 FTP、ファイル システム、または FPSE などのメソッドを別に発行する場合は、次を参照してください。 [web アプリケーションのデプロイから個別にデータベースを展開する](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate)for Visual Studio および ASP.NET Web 配置コンテンツ マップでします。

### <a name="entity-framework-code-first-migrations"></a>Entity Framework Code First Migrations

Entity Framework バージョン 4.3 では、Microsoft は、Code First Migrations を紹介します。 Code First Migrations は、データ モデルに増分の変更を加えると、データベースにこれらの変更を反映するプロセスを自動化します。 Code First の以前のバージョンでは、通常は削除して再作成、データベースをデータ モデルを変更するたびに、Entity Framework を使用できます。 これが問題を開発テスト データが簡単に再作成されたが、実稼働環境では、通常するデータベースを削除せずに、データベース スキーマを更新します。 移行の機能によって、Code First を削除し、再作成することがなく、データベースを更新します。 Code First の必要なスキーマ変更を行う方法を自動的に決定させることができますか、変更内容をカスタマイズするコードを記述することができます。 Code First Migrations の概要については、次を参照してください。 [Code First Migrations](https://msdn.microsoft.com/library/hh770484.aspx)します。

Web プロジェクトを展開する場合に、Visual Studio は、Code First Migrations で管理されているデータベースの配置のプロセスを自動化できます。 発行プロファイルを作成するときに、実行 Code First Migrations (アプリケーションの起動時に実行されます) というラベルの付いたチェック ボックスを選択します。 この設定により、Code First が使用されるように、移行先サーバーでアプリケーションの Web.config ファイルを自動的に構成する展開プロセス、`MigrateDatabaseToLatestVersion`初期化子クラス。

Visual Studio は行いませんデータベースで何か、展開プロセス中にします。 デプロイされたアプリケーションでは、デプロイ後に最初に、データベースにアクセス、Code First 自動的にデータベースを作成またはデータベース スキーマを最新バージョンに更新します。 アプリケーションでは、移行の Seed メソッドを実装する場合、データベースが作成されたか、スキーマが更新され、メソッドが実行されます。

このチュートリアルでは、アプリケーション データベースを展開するのに Code First Migrations を使用します。

### <a name="the-dbdacfx-web-deploy-provider"></a>DbDacFx Web 配置プロバイダー

Entity Framework Code First によって管理されていない SQL Server データベースの発行プロファイルを構成するときに、データベースの更新をラベル付けは、チェック ボックスを選択できます。 初期のデプロイ時に、dbDacFx プロバイダーは、ソース データベースに一致する転送先データベースにテーブルと他のデータベース オブジェクトを作成します。 以降のデプロイで、プロバイダーが元とコピー先のデータベース間で相違点を確認し、ソース データベースに一致する先のデータベースのスキーマを更新します。 既定では、プロバイダーは、テーブルまたは列が削除されるときなど、データ損失につながるな変更を加えてはありません。

このメソッドはデータベース テーブル内のデータの展開を自動化していませんが、そのデプロイ時にそれらを実行する Visual Studio を構成するスクリプトを作成できます。 デプロイ中にスクリプトを実行する別の理由は、データの損失になるために自動的に行うことができないスキーマ変更を行うです。

このチュートリアルでは、ASP.NET メンバーシップ データベースを展開するのに dbDacFx プロバイダーを使用します。

## <a name="troubleshooting-during-this-tutorial"></a>このチュートリアルの中のトラブルシューティング

展開中にエラーが発生したとき、またはデプロイされたサイトが正しく実行されない場合は、エラー メッセージは、明確な解決策を常に用意されていません。 問題の一般的なシナリオで役立つ、[トラブルシューティングのリファレンス ページ](troubleshooting.md)は使用できます。 エラー メッセージを取得する、または、チュートリアルを進めるときに機能しない、必ず、トラブルシューティングのページを確認してください。

## <a name="comments-welcome"></a>コメントの開始

チュートリアルでは、コメントは、[ようこそ]、およびあらゆる努力をアカウントの修正または提案のコメントのチュートリアルに用意されている改善点を考慮できるように、チュートリアルが更新されたときにします。

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルは、次の製品として記述されました。

- Windows 8 または Windows 7。
- Visual Studio 2012 または Visual Studio 2012 の Express for Web と[最新の更新プログラム](https://go.microsoft.com/fwlink/?LinkId=272486)します。
- [Azure SDK for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Visual Studio 2010 SP1 または Visual Studio 2013 を使用して、チュートリアルに従うことができますが、やスクリーン ショットは、一部の機能が異なるものになります。

Visual Studio 2013 を使用している場合は、インストール[Azure SDK for Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322)します。

Visual Studio 2010 SP1 を使用している場合は、次のソフトウェアをインストールします。

- [Azure SDK for Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx)します。

SDK の依存関係の数がコンピューターに既にある、によっては、Azure SDK のインストールに 30 分以上には数分からの時間がかかる可能性があります。 場合でも、Azure への代わりにサード パーティ製ホスティング プロバイダーにパブリッシュするには、SDK には、Visual Studio web に最新の更新プログラムが含まれているために発行機能は、Azure SDK を作成する必要があります。

> [!NOTE]
> このチュートリアルは、Azure SDK のバージョン 1.8.1 で記述されています。 その後、追加機能を持つ新しいバージョンがリリースされました。 チュートリアルでは、これらの機能とその詳細情報が存在するリソースへのリンクは言うまでも更新されました。


手順とスクリーン ショットは、Windows 8 に基づきますが、チュートリアルでは、Windows 7 の相違点を説明します。

その他のソフトウェアが、チュートリアルを完了するために必要ですが、まだがインストールされている必要はありません。 このチュートリアルでは必要なときにインストールするための手順説明をします。

## <a name="download-the-sample-application"></a>サンプル アプリケーションをダウンロードします。

デプロイするアプリケーションでは、Contoso University の名前しが既に作成済み。 これは疎で説明されている Contoso University アプリケーションに基づいて、大学の web サイトの簡略化されたバージョン、 [ASP.NET サイト上の Entity Framework チュートリアル](https://asp.net/entity-framework/tutorials)。

前提条件のインストールがある場合は、ダウンロード、 [Contoso University web アプリケーション](https://go.microsoft.com/fwlink/p/?LinkId=282627)します。 *.Zip*ファイルには、プロジェクトの複数のバージョンが含まれています。 チュートリアルの手順を実行すると、するには、c# フォルダーにあるプロジェクトを起動します。 チュートリアルの最後に、プロジェクトがどのように表示、ContosoUniversity エンド フォルダーにプロジェクトを開きます。

チュートリアルのステップを使用するため、プロジェクトを準備するには、次の手順を実行します。

1. Visual Studio プロジェクトを操作するために使用する任意のフォルダーで ContosoUniversity という名前のフォルダー内の c# フォルダーから ContosoUniversity ソリューション ファイルを保存します。

    既定の設定では、Visual Studio 2012 用の次のフォルダーです。

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (このチュートリアルのスクリーン ショットでは、プロジェクト フォルダーに配置された、ルート ディレクトリで、 `C`: ドライブ)。
2. Visual Studio を起動し、プロジェクトを開きます。
3. **ソリューション エクスプ ローラー**ソリューションを右クリックし、クリックして**EnableNuGet パッケージの復元**します。
4. ソリューションをビルドします。
5. コンパイル エラーが発生した場合は、NuGet パッケージを手動で復元します。

    1. **ソリューション エクスプ ローラー**、ソリューションを右クリックし、クリックして**ソリューションの NuGet パッケージの管理**します。
    2. 上部にある、 **NuGet パッケージの管理** ダイアログ ボックスが表示されます**一部の NuGet パッケージはこのソリューションにありません。復元する をクリックします。** をクリックして、**復元**ボタンをクリックします。
    3. ソリューションをリビルドします。
6. Ctrl キーを押し、f5 キーを押してアプリケーションを実行します。

    アプリケーションは、Contoso University のホーム ページを開きます。

    ![ホーム ページの開発](introduction/_static/image1.png)

    (Visual Studio、SQL Server Express LocalDB インスタンスの起動中に、待機時間にすることがありますとしてがタイムアウト エラーが時間がかかるプロセスです。 そのケースだけでプロジェクトを開始、もう一度。)

Web サイトのページでは、メニュー バーからアクセスできるし、次の機能を実行することができます。

- 学生の統計情報 (バージョン情報 ページ) を表示します。
- 表示、編集、削除、および学生を追加します。
- 表示し、コースを編集します。
- 表示し、インストラクターを編集します。
- 表示および部門を編集します。

いくつかの代表的なページのスクリーン ショットを次に示します。

![学生のページの開発](introduction/_static/image2.png)

![学生のページの開発を追加します。](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>展開に影響するアプリケーションの機能を確認してください。

アプリケーションの次の機能は、展開する方法や、それをデプロイするのでは行う必要があるに影響します。 これらの各操作は、以下のチュートリアル シリーズでさらに詳しく説明します。

- Contoso University では、SQL Server データベースを使用して、student および instructor 名などのアプリケーション データを格納します。 さまざまなテスト データ、および運用環境のデータを格納しているし、実稼働環境に展開するときにテスト データを除外する必要があります。
- アプリケーションでは、SQL Server データベースのユーザー アカウント情報を格納する、ASP.NET メンバーシップ システムを使用します。 アプリケーションでは、制限された情報へのアクセスを持つ管理者ユーザーを定義します。 テスト アカウントを持たないが、管理者アカウントのメンバーシップ データベースをデプロイする必要があります。
- アプリケーションでは、サード パーティ製のエラー ログとレポート ユーティリティを使用します。 このユーティリティは、アプリケーションと共に配置する必要がありますアセンブリで提供されます。
- エラーのログ記録ユーティリティでは、ファイル フォルダーに XML ファイルにエラー情報を書き込みます。 ある、配置サイトで ASP.NET を実行するアカウントは、このフォルダーに書き込みアクセス許可を持つこと、およびデプロイからこのフォルダーを除外する必要があることを確認します。 (それ以外の場合、テスト環境からのエラー ログのデータを運用環境にデプロイする場合がありますや、運用環境のエラー ログ ファイルを削除する可能性があります)。
- アプリケーションには、いくつかの設定の変更が必要で、デプロイされている*Web.config*先の環境 (テスト、ステージング、または本番) とその他の設定によっては、ビルドの変更が必要に応じてファイル(デバッグまたはリリース) を構成します。
- Visual Studio ソリューションには、クラス ライブラリ プロジェクトが含まれています。 このプロジェクトで生成されるアセンブリのみを配置する必要があります、プロジェクト自体ではありません。

## <a name="summary"></a>まとめ

シリーズの最初のチュートリアルでは、サンプルの Visual Studio プロジェクトをダウンロードし、アプリケーションをデプロイする方法に影響するサイトの機能を確認しました。 以下のチュートリアルでの展開を準備を自動的に処理するような設定です。 その他の手動で注意します。

> [!div class="step-by-step"]
> [次へ](preparing-databases.md)
