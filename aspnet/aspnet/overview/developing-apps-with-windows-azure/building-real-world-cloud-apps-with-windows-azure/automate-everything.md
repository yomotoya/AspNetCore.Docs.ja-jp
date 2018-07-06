---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: (Azure での実際のクラウド アプリの構築) すべてを自動化 |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 87b697847845ab88943e7a659ccd9487b9408d5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839636"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>(Azure での実際のクラウド アプリの構築) すべてを自動化します。
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の概要については、次を参照してください。[第 1 章](introduction.md)します。


実際には、見て最初の 3 つのパターンには、あらゆるソフトウェア開発プロジェクト、特にクラウド プロジェクトが適用されます。 このパターンは、開発タスクの自動化についてです。 手動プロセスは低速でエラーが発生しやすい; は、重要なトピック高速かつ信頼性が高く、アジャイルのワークフローを設定することの支援としてそれらの多くを自動化します。 困難または不可能なオンプレミス環境を自動化するには多くのタスクを簡単に自動化できるため、クラウド開発を一意に重要です。 たとえば、テスト全体を設定することが新しい web サーバーとバックエンド Vm を含む環境では、データベース、blob storage (ファイル ストレージ)、キューなど。

## <a name="devops-workflow"></a>DevOps ワークフロー

"DevOps"という用語が聞こえるますます ソフトウェアを効率的に開発するために開発と運用タスクを統合することがある認識外という用語を開発しました。 有効にワークフローの種類をすることができますアプリを開発、デプロイ運用環境の演算子の使用からについて説明します、学習した内容への応答で変更、迅速かつ確実に、サイクルを繰り返します。

一部成功するクラウドの開発チームは、実際の環境に 1 日複数回をデプロイします。 Azure チーム、大規模な展開に使用更新でも管理はすべて 2 ~ 3 か月ごとの 2 ~ 3 日間とメジャー リリース 2 ~ 3 週間ごとのリリースのマイナー アップデートします。 そのリズムにではお客様のフィードバックに素早く対応する実際に役立ちます。

このためには、開発とデプロイ サイクルが反復可能な信頼性の高い、予測可能なおよび低サイクル時間を有効にする必要があります。

![DevOps ワークフロー](automate-everything/_static/image1.png)

つまり、機能のアイデアがある場合と、顧客の使用しフィードバックを提供するとの間の期間はできるだけ短くする必要があります。 最初の 3 つのパターン: は、すべてのソースの管理を自動化し、継続的インテグレーションと配信の本質はそのようなプロセスを有効にするために推奨されるベスト プラクティス。

## <a name="azure-management-scripts"></a>Azure の管理スクリプト

[この電子書籍の紹介](introduction.md)、web ベース コンソールで、Azure 管理ポータルを説明しました。 管理ポータルを使用すると、監視およびすべての Azure にデプロイしたリソースを管理できます。 作成、web アプリと仮想マシンなどのサービスを削除し、それらのサービスを構成、サービスの操作を監視およびなどを簡単になります。 すばらしいツールですが、これを使用して手動プロセスであります。 場合は、あらゆるサイズの実稼働アプリケーションを開発しようとしているし、チーム環境で特にお勧め、ポータルについて説明し、Azure を調査するために UI を使用し、繰り返し行うプロセスを自動化します。

管理ポータルで、または Visual Studio から手動で行うことができますほぼすべてのものは、REST 管理 API を呼び出すことによっても実行できます。 使用してスクリプトを記述する[Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)などのオープン ソース フレームワークを使用できますか[Chef](http://www.opscode.com/chef/)または[Puppet](http://puppetlabs.com/puppet/what-is-puppet)します。 Mac または Linux の環境での Bash コマンド ライン ツールを使用することもできます。 Azure では、これらすべての異なる環境でのスクリプト Api とは、 [.NET 管理 API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)スクリプトではなくコードを記述する場合。

Fix It アプリのテスト環境を作成し、その環境にプロジェクトをデプロイのプロセスを自動化するいくつかの Windows PowerShell スクリプトを作成しましたし、これらのスクリプトの内容の一部を確認します。

## <a name="environment-creation-script"></a>環境の作成スクリプト

最初のスクリプトを見ていきますが名前付き*新規 AzureWebsiteEnv.ps1*します。 修正をテスト用のアプリを導入することが Azure 環境を作成します。 このスクリプトを実行する主なタスクは次のとおりです。

- Web アプリを作成します。
- ストレージ アカウントを作成します。 (必要な blob やキューの後半の章でわかります。)
- SQL Database サーバーと 2 つのデータベースの作成: アプリケーション データベース、およびメンバーシップ データベース。
- アプリは、ストレージ アカウントとデータベースへのアクセスに使用する azure の設定を格納します。
- 展開を自動化するために使用する設定ファイルを作成します。

### <a name="run-the-script"></a>スクリプトを実行します


> [!NOTE]
> この章のこの部分は、それらを実行するために入力したコマンドおよびスクリプトの例を示します。 このデモ スクリプトを実行するために知っておくべきすべてのものが提供されません。 方法を操作を行います it 手順については、次を参照してください。[付録:、、サンプル アプリケーションの修正](the-fix-it-sample-application.md#deploybase)します。


Azure サービスを管理する PowerShell スクリプトを実行するには、Azure PowerShell コンソールをインストールし、Azure サブスクリプションで動作するように構成する必要があります。 設定しているとは、次のようなコマンドを使用して、Fix It 環境の作成スクリプトを実行できます。

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name`パラメーターは、データベースとストレージ アカウントを作成するときに使用される名前を指定し、`SqlDatabasePassword`パラメーターは、SQL Database 用に作成される管理者アカウントのパスワードを指定します。 見て後で使用できるその他のパラメーターがあります。

![PowerShell ウィンドウ](automate-everything/_static/image2.png)

スクリプトが完了後することができますを参照してください、管理ポータルで作成したもの。 2 つのデータベースにあります。

![Databases](automate-everything/_static/image3.png)

ストレージ アカウント:

![ストレージ アカウント](automate-everything/_static/image4.png)

Web アプリ:

![Web サイト](automate-everything/_static/image5.png)

**構成**タブ web アプリがストレージ アカウントの設定および SQL データベース接続文字列設定されている修正プログラムをアプリを表示できます。

![appSettings、connectionStrings](automate-everything/_static/image6.png)

*Automation*フォルダーもに、  *&lt;websitename&gt;.pubxml*ファイル。 このファイルには、MSBuild がアプリケーションを作成した Azure 環境のデプロイに使用する設定が格納されます。 例えば:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

ご覧のように、スクリプトが、完全なテスト環境を作成し、プロセス全体は約 90 秒間で行われます。

テスト環境を作成する場合、チームの他のユーザーが、スクリプトを実行することができます。 だけでなく、高速ですが、できると確信を使用するものと同じ環境を使用していることもできます。 さほど設定する場合は、すべてのユーザーがモ ノ手動で管理ポータル UI を使用しているは確信はできませんでした。

### <a name="a-look-at-the-scripts"></a>スクリプトを参照してください。

実際には 3 つのスクリプトをこの作業を行うにがあります。 コマンドラインから 1 つを呼び出すし、一部のタスクの実行を自動的に他の 2 つを使用します。

- *新しい AzureWebSiteEnv.ps1*メイン スクリプトに示します。

    - *新しい AzureStorage.ps1*ストレージ アカウントを作成します。
    - *新しい AzureSql.ps1*データベースが作成されます。

### <a name="parameters-in-the-main-script"></a>メインのスクリプトのパラメーター

メインのスクリプトでは、*新規 AzureWebSiteEnv.ps1*、いくつかのパラメーターを定義します。

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

2 つのパラメーターが必要です。

- このスクリプトを作成する web アプリの名前。 (これにも、URL の使用: `<name>.azurewebsites.net`)。
- このスクリプトを作成するデータベース サーバーの場合は、新しい管理ユーザーのパスワード。

省略可能なパラメーターを使用すると、データ センターの場所 (既定値は「米国西部」)、データベース サーバー管理者名 ("dbuser"既定値)、およびデータベース サーバーのファイアウォール規則を指定できます。

### <a name="create-the-web-app"></a>Web アプリを作成します。

まず、スクリプトでは呼び出すことによって、web アプリを作成、`New-AzureWebsite`内に web アプリの名前と場所パラメーター値を渡すコマンドレット。

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>ストレージ アカウントを作成します。

メイン スクリプトを実行し、<em>新規 AzureStorage.ps1</em>スクリプトを指定する"<em>&lt;websitename&gt;</em>ストレージ"、ストレージ アカウント名の同じデータ センターの場所として、web アプリです。

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*新しい AzureStorage.ps1*呼び出し、`New-AzureStorageAccount`ストレージ アカウントを作成するコマンドレットは、アカウント名とアクセス キーの値を返します。 アプリケーションでは、blob とストレージ アカウント内のキューにアクセスするために、これらの値を必要があります。

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

常にたくない、新しいストレージ アカウントを作成するには必要に応じて既存のストレージ アカウントを使用するよう指示するパラメーターを追加することにより、スクリプトを強化する可能性があります。

### <a name="create-the-databases"></a>データベースを作成します。

メイン スクリプトが、データベース作成スクリプトを実行し、*新規 AzureSql.ps1*、後に既定のデータベースの設定およびファイアウォールのルール名。

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

データベースの作成スクリプトでは、開発用コンピューターの IP アドレスを取得し、開発用コンピューターに接続して、サーバーを管理するためのファイアウォール規則を設定します。 データベース作成スクリプトのデータベースを設定するいくつかの手順に進みます。

- 使用して、サーバーを作成、`New-AzureSqlDatabaseServer`コマンドレット。

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- 開発用コンピューターに接続する web アプリを有効にして、サーバーを管理するを有効にするファイアウォール規則を作成します。 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- 使用して、サーバー名と資格情報を含むデータベース コンテキストを作成、`New-AzureSqlDatabaseServerContext`コマンドレット。

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` 関数を呼び出すスクリプトでは、`ConvertTo-SecureString`返し、パスワードを暗号化するコマンドレット、`PSCredential`オブジェクト、同じ型の型を`Get-Credential`コマンドレットを返します。
- 使用して、アプリケーション データベースと、メンバーシップ データベースを作成、`New-AzureSqlDatabase`コマンドレット。

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- 各データベースのローカルに定義された関数 tocreates 接続文字列を呼び出します。 アプリケーションは、データベースにアクセスするのにこれらの接続文字列を使用します。 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get SQLAzureDatabaseConnectionString は、渡されたパラメーター値から接続文字列を作成するスクリプトで定義されている関数です。

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- データベース サーバー名と接続文字列のハッシュ テーブルを返します。

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Fix It アプリは、個別にメンバーシップとアプリケーション データベースを使用します。 単一のデータベース内のメンバーシップとアプリケーションの両方のデータを格納することもできます。

### <a name="store-app-settings-and-connection-strings"></a>ストア アプリの設定と接続文字列

Azure では、機能の設定と接続文字列を読み取ろうとしたときに、アプリケーションに返されるものを自動的に上書きを保存することができます、`appSettings`または`connectionStrings`Web.config ファイル内のコレクション。 これは、適用する代替[Web.config 変換](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)を展開する場合。 詳細については、次を参照してください。[機密データを Azure に保存](source-control.md#appsettings)この電子書籍の後半。

Azure のすべての環境の作成スクリプトを格納、`appSettings`と`connectionStrings`アプリケーションが Azure での実行時、ストレージ アカウントとデータベースにアクセスする必要がある値。

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/)テレメトリ フレームワークで示したです、[監視とテレメトリ](monitoring-and-telemetry.md)」の章。 環境の作成スクリプトでは、New Relic の設定を選択するかどうかを確認する web アプリも再起動します。

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>展開の準備

プロセスの最後に、環境の作成スクリプトは、配置スクリプトで使用されるファイルを作成する 2 つの関数を呼び出します。

これらの関数のいずれかの発行プロファイルを作成します *(&lt;websitename&gt;.pubxml*ファイル)。 コードは、発行設定を取得する Azure REST API を呼び出すし、内の情報を保存、 *.publishsettings*ファイル。 テンプレート ファイルとそのファイルから情報を使用して (*pubxml.template*) を作成する、 *.pubxml*発行プロファイルを含むファイル。 この 2 段階のプロセスは、Visual Studio で何をシミュレート: ダウンロード、 *.publishsettings*ファイルし、発行プロファイルを作成するためにインポートします。

その他の関数では、別のテンプレート ファイル (web サイト environment.template) を使用して、作成、 *web サイト environment.xml*と共に配置スクリプトで使用する設定を含むファイル、 *.pubxml*ファイル。

### <a name="troubleshooting-and-error-handling"></a>トラブルシューティングとエラー処理

スクリプトは、プログラムと同様に、: 失敗したと、エラーとその原因に関する情報を確認するようにします。 このため、環境の作成スクリプトがの値を変更、`VerbosePreference`変数から`SilentlyContinue`に`Continue`すべて詳細メッセージが表示されるようします。 値も変更、`ErrorActionPreference`変数から`Continue`に`Stop`でも未終了エラーを検出、スクリプトを停止しますように。

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

すべての作業は、前に、スクリプトは、処理が完了したら、経過時間を求めるために、開始時刻を格納します。

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

その作業を完了した後、スクリプトの経過時間が表示されます。

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

すべてのキー操作のスクリプトを書き込みます詳細メッセージは、たとえば。

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>配置スクリプト

どのような*新規 AzureWebsiteEnv.ps1*環境の作成のスクリプトでは、*発行 AzureWebsite.ps1*スクリプトでは、アプリケーションを展開します。

配置スクリプトから web アプリの名前を取得する、 *web サイト environment.xml*環境の作成スクリプトによって作成されたファイル。

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

デプロイ ユーザー パスワードを取得、 *.publishsettings*ファイル。

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

実行、 [MSBuild](http://msbuildbook.com/)構築し、プロジェクトを展開するコマンド。

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

指定した場合と、`Launch`呼び出し、コマンドラインでパラメーター、`Show-AzureWebsite`コマンドレットを web サイトの URL を既定のブラウザーを開きます。

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

次のようなコマンドを使用して、配置スクリプトを実行できます。

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

ブラウザーが開き、サイトでは、クラウドで実行されているが完了したら、 `<websitename>.azurewebsites.net` URL。

![Windows Azure にデプロイされたアプリを修正します。](automate-everything/_static/image7.png)

## <a name="summary"></a>まとめ

これらのスクリプトを同じ手順が常に同じオプションを使用して、同じ順序で実行することを確信できます。 こうことを確認、チームの各開発者がどうしても見落としまたはめちゃくちゃに何か別のチーム メンバーの環境または運用環境で同様に機能しません実際に自分のマシンで独自のものをデプロイします。

同様の方法では、REST API、Windows PowerShell スクリプト、.NET 言語 API、または Linux またはファルダで実行できる Bash ユーティリティを使用して、管理ポータルで実行できるほとんどの Azure の管理機能を自動化できます。

[[次へ] の章](source-control.md)をソース コードを確認し、ソース コード リポジトリで、スクリプトを含める重要な理由について説明します。

## <a name="resources"></a>リソース

- [インストールし、Azure の Windows PowerShell を構成](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)します。 Azure PowerShell コマンドレットをインストールする方法も考慮に入れて必要がある、Azure を管理するためにコンピューターに証明書をインストールする方法について説明します。 これは、PowerShell 自体を学習するためのリソースへのリンクもあるために開始する最適な場所です。
- [Azure スクリプト センター](https://docs.microsoft.com/azure/automation/automation-runbook-gallery)します。 チュートリアル、コマンドレットのリファレンス ドキュメントとソース コード、およびサンプル スクリプトへのリンクでの Azure サービスを管理するスクリプトを開発するためのリソースへの WindowsAzure.com ポータル
- [週末 Scripter: Azure と PowerShell の開始](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx)します。 Windows PowerShell に専用のブログでは、この投稿は、PowerShell を使用して、Azure の管理機能の導入として優れていますを提供します。
- [インストールし、構成、Azure クロス プラットフォーム コマンド ライン インターフェイス](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)します。 システムを Windows だけでなく、Mac および Linux とで動作する Azure のスクリプティング フレームワークの入門チュートリアルです。
- [Azure Sdk のダウンロードとツール」のコマンド ライン ツールの「](https://azure.microsoft.com/downloads/)します。 ドキュメントと azure コマンド ライン ツールに関連するダウンロード ポータル ページです。
- [すべてを Azure 管理ライブラリと .NET を使用した自動化](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)します。 Scott Hanselman は、Azure 用の .NET 管理 API が導入されています。
- [Windows PowerShell スクリプトを使用して、開発環境およびテスト環境に発行する](https://msdn.microsoft.com/library/azure/dn642480.aspx)します。 使用する方法を説明する MSDN ドキュメントでは、web プロジェクトの Visual Studio が自動的に生成するスクリプトを発行します。
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597)します。 Visual Studio 拡張機能を Visual Studio での Windows PowerShell 言語サポートを追加します。

> [!div class="step-by-step"]
> [前へ](introduction.md)
> [次へ](source-control.md)
