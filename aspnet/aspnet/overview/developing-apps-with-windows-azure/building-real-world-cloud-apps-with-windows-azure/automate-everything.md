---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: "(Azure での実際のクラウド アプリの構築) すべてを自動化 |Microsoft ドキュメント"
author: MikeWasson
description: "Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: cf1cb7b07ffe8750724e58e4fb66854c9a033a54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>(Azure での実際のクラウド アプリの構築) すべてを自動化します。
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の概要については、次を参照してください。[第 1 章](introduction.md)です。


クラウド プロジェクトでは特にはあらゆるソフトウェア開発プロジェクトに実際に見ていきます最初の 3 つのパターンが適用されます。 このパターンは、開発タスクの自動化に関するです。 これは手動のプロセスは低速でエラーが発生しやすい; ためにの重要なトピック高速な信頼性、およびアジャイルのワークフローを設定可能なこととしてそれらの多くを自動化します。 困難または不可能な内部設置型環境で自動化する多数のタスクを簡単に自動化できるためクラウド開発を一意に重要です。 テスト全体を設定するなど、新しい web サーバーとバック エンドの Vm を含む環境では、データベース、blob ストレージ (ファイル記憶域)、キューなどです。

## <a name="devops-workflow"></a>DevOps ワークフロー

用語"DevOps"聞き取ったますます ソフトウェアを効率的に開発するために開発と操作のタスクを統合することがある認識外という用語を開発しました。 有効にワークフローの種類をすることができますアプリ開発、展開、生産量から学ぶ、学習した内容への応答で変更して、迅速および確実にサイクルを繰り返します。

一部成功したクラウドの開発チームは、複数回、1 日の実稼働環境に展開します。 大規模な展開に使用する Azure チーム更新 2 ~ 3 か月ごと、ここではすべて 2 ~ 3 日間とメジャー リリース 2 ~ 3 週間ごとマイナーな更新をリリースします。 そのリズムに取得するではお客様のフィードバックに応答する実際に役立ちます。

そのためには、反復可能な信頼性と予測可能なを低サイクル時間を持つの開発と配置のサイクルを有効にする必要です。

![DevOps ワークフロー](automate-everything/_static/image1.png)

つまり、機能の概要を把握する必要がある場合と、お客様がこれを使用しておよびフィードバックを提供するまでの期間はできるだけ短くする必要があります。 – 最初の 3 つのパターンでは、すべて、ソース管理を自動化し、そのようなプロセスを有効にするために推奨されるベスト プラクティスに関するすべての継続的インテグレーションと配信がします。

## <a name="azure-management-scripts"></a>Azure 管理スクリプト

[この電子書籍の概要](introduction.md)、web ベース コンソールで、Azure 管理ポータルを確認しました。 管理ポータルを使用すると、監視し、すべての Azure に展開されているリソースを管理できます。 作成し、web アプリおよび Vm などのサービスを削除、それらのサービスを構成、サービス操作の監視およびなどの簡単な方法であります。 便利なツールですが、手動での処理は、使用すること。 場合は、任意のサイズの実稼働アプリケーションを開発して、チームの環境で特にことをお勧めするポータルについて説明し、Azure の理解するために UI を通過し、繰り返し行うプロセスを自動化します。

管理ポータルで、または Visual Studio から手動で行うことができますをほぼすべては、REST 管理 API を呼び出すことによっても実行できます。 使用してスクリプトを記述する[Windows PowerShell](https://msdn.microsoft.com/en-us/library/windowsazure/jj156055.aspx)などのオープン ソース フレームワークを使用できますか[Chef](http://www.opscode.com/chef/)または[Puppet](http://puppetlabs.com/puppet/what-is-puppet)です。 また、Mac または Linux の環境でバッシュ コマンド ライン ツールを使用することができます。 Azure では、これらすべてのさまざまな環境の Api のスクリプト作成を保持している、 [.NET 管理 API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)スクリプトではなくコードを記述する場合にします。

修正アプリケーション用のテスト環境を作成し、その環境にプロジェクトを配置するプロセスを自動化するいくつかの Windows PowerShell スクリプトを作成しましたし、それらのスクリプトの内容の一部を確認します。

## <a name="environment-creation-script"></a>環境の作成スクリプト

最初のスクリプトを見ていきますが名前付き*新規 AzureWebsiteEnv.ps1*です。 修正をテスト用のアプリを導入することが Azure 環境を作成します。 このスクリプトを実行する主なタスクは次のとおりです。

- Web アプリを作成します。
- ストレージ アカウントを作成します。 (必須の blob およびキュー、後続の章でわかります。)
- SQL データベース サーバーと 2 つのデータベースを作成します。 アプリケーション データベース、およびメンバーシップ データベース。
- Azure ストレージ アカウントとデータベースにアクセスするアプリを使用するには、設定を格納します。
- 展開を自動化するために使用する設定ファイルを作成します。

### <a name="run-the-script"></a>スクリプトを実行します。


> [!NOTE]
> この章のこの部分では、それらを実行するために入力したコマンドおよびスクリプトの例を示します。 このデモし、スクリプトを実行するために把握する必要があります。 すべてのものが用意されていません。 How-には、it 手順については、次を参照してください。[付録:、そのサンプル アプリケーションの修正](the-fix-it-sample-application.md#deploybase)です。


Azure PowerShell コンソールをインストールして、Azure サブスクリプションを使用するように構成する必要がある Azure のサービスを管理する PowerShell スクリプトを実行します。 セットアップが完了した後は、次のようなコマンドを使用して修正環境の作成スクリプトを実行できます。

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name`パラメーターは、データベースとストレージ アカウントを作成するときに使用される名前を指定し、`SqlDatabasePassword`パラメーターは、SQL データベースに作成される管理者アカウントのパスワードを指定します。 他のパラメーターを見ていきます以降を使用することができます。

![PowerShell ウィンドウ](automate-everything/_static/image2.png)

スクリプトの終了後に表示できます、管理ポータルで作成内容。 2 つのデータベースがあります。

![データベース](automate-everything/_static/image3.png)

ストレージ アカウント:

![ストレージ アカウント](automate-everything/_static/image4.png)

Web アプリ:

![Web サイト](automate-everything/_static/image5.png)

**構成** タブ、web アプリについては、ストレージ アカウントの設定があるおよび SQL データベース接続文字列設定されている修正プログラムをアプリを確認できます。

![appSettings、connectionStrings](automate-everything/_static/image6.png)

*オートメーション*フォルダー今すぐも含まれています、  *&lt;websitename&gt;.pubxml*ファイル。 このファイルには、MSBuild が作成した Azure 環境にアプリケーションを配置に使用する設定が格納されます。 例:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

ご覧のように、スクリプトが、完全なテスト環境を作成およびプロセス全体は約 90 秒間で行われます。

チームの他のユーザーは、テスト環境を作成する場合、スクリプトだけ実行できます。 高速で、できるだけでなく、確実に使用しているものと同じ環境を使用することもできます。 さほどを保証する場合はすべてのユーザーが設定手動で管理ポータル UI を使用してできることができませんでした。

### <a name="a-look-at-the-scripts"></a>スクリプトを見る

実際には次の 3 つはスクリプトはこの作業を実行します。 コマンドラインから 1 つを呼び出すし、を一部のタスクを行うには、その他の 2 つが自動的に使用します。

- *新しい AzureWebSiteEnv.ps1*メイン スクリプトです。

    - *新しい AzureStorage.ps1*ストレージ アカウントを作成します。
    - *新しい AzureSql.ps1*データベースを作成します。

### <a name="parameters-in-the-main-script"></a>メインのスクリプトのパラメーター

メイン スクリプト*新規 AzureWebSiteEnv.ps1*、いくつかのパラメーターを定義します。

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

2 つのパラメーターが必要です。

- スクリプトを作成する web アプリの名前。 (これは、URL の使用も: `<name>.azurewebsites.net`)。
- このスクリプトを作成するデータベース サーバーの場合は、新しい管理ユーザーのパスワード。

省略可能なパラメーターを使用すると、データ センターの場所 (既定は"West US")、データベース サーバーの管理者名 ("dbuser"既定値)、およびデータベース サーバー用のファイアウォール ルールを指定できます。

### <a name="create-the-web-app"></a>Web アプリを作成します。

スクリプトは、まずは呼び出すことによって、web アプリを作成、`New-AzureWebsite`内に web アプリの名前と場所パラメーター値を渡すコマンドレット。

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>ストレージ アカウントを作成します。

メイン スクリプトを実行し、*新規 AzureStorage.ps1*スクリプトを指定する"*&lt;websitename&gt;*記憶域"ストレージ アカウントの名前に、同じデータ センターの場所として、web アプリです。

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*新しい AzureStorage.ps1*呼び出し、`New-AzureStorageAccount`ストレージ アカウントを作成するコマンドレットに、アカウント名とアクセス キーの値が返されます。 アプリケーションでは、blob およびキューのストレージ アカウントにアクセスするために、これらの値を必要があります。

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

常にたくないです。 新しいストレージ アカウントを作成するには必要に応じて既存のストレージ アカウントを使用するように誘導するパラメーターを追加して、スクリプトを強化する可能性があります。

### <a name="create-the-databases"></a>データベースを作成します。

メイン スクリプトが、データベース作成スクリプトを実行し、*新規 AzureSql.ps1*、後の既定のデータベース設定とファイアウォールのルール名。

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

データベース作成スクリプトでは、開発用コンピューターの IP アドレスを取得し、開発用コンピューターに接続して、サーバーを管理するためにファイアウォール ルールを設定します。 データベース作成スクリプトのデータベースを設定するいくつかの手順に進みます。

- 使用して、サーバーを作成、`New-AzureSqlDatabaseServer`コマンドレット。

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- 開発用コンピューターに接続するための web アプリを有効にして、サーバーを管理するを有効にするファイアウォール規則を作成します。 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- 使用して、サーバー名と資格情報を含むデータベース コンテキストを作成、`New-AzureSqlDatabaseServerContext`コマンドレット。

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText`呼び出すスクリプト内の関数は、`ConvertTo-SecureString`を返し、パスワードを暗号化するコマンドレット、`PSCredential`オブジェクト、同じ型の型を`Get-Credential`コマンドレットを返します。
- 使用して、アプリケーション データベースと、メンバーシップ データベースを作成、`New-AzureSqlDatabase`コマンドレット。

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- データベースごとにローカルに定義された関数 tocreates 接続文字列を呼び出します。 アプリケーションでは、これらの接続文字列をデータベースへのアクセスを使用します。 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get SQLAzureDatabaseConnectionString は、渡されたパラメーター値から、接続文字列を作成するスクリプトで定義されている関数です。

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- データベース サーバー名と接続文字列を含むハッシュ テーブルを返します。

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

修正、アプリは、個別にメンバーシップ データベースとアプリケーション データベースを使用します。 メンバーシップとアプリケーションの両方のデータを 1 つのデータベースに配置することもできます。

### <a name="store-app-settings-and-connection-strings"></a>ストア アプリの設定と接続文字列

Azure では、機能の設定と接続文字列を読み取ろうとしたときに、アプリケーションに返されるものを自動的に上書きを保存することができます、`appSettings`または`connectionStrings`Web.config ファイル内のコレクション。 これは、適用する代わりに[Web.config 変換](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)を展開する場合。 詳細については、次を参照してください。[機密データを Azure に保存](source-control.md#appsettings)この電子書籍の後半。

Azure のすべての環境の作成スクリプトを格納、`appSettings`と`connectionStrings`アプリケーションが Azure での実行時に、ストレージ アカウントとデータベースにアクセスする必要がある値。

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/)で説明する製品利用統計情報フレームワークには、[監視と遠隔測定](monitoring-and-telemetry.md)章します。 環境の作成スクリプトでは、web アプリを New Relic 設定を選択するかどうかを確認も再起動します。

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>展開の準備

プロセスの最後に、環境の作成スクリプトは、展開スクリプトにより使用されるファイルを作成する 2 つの関数を呼び出します。

発行プロファイルを作成するこれらの関数のいずれかの*(&lt;websitename&gt;.pubxml*ファイル)。 発行設定を取得する Azure REST API を呼び出すコードとの情報は、保存、 *.publishsettings*ファイル。 テンプレート ファイルとそのファイルから情報を使用して、(*pubxml.template*) を作成する、 *.pubxml*発行プロファイルを含むファイルです。 Visual Studio で行うこの 2 段階のプロセスをシミュレート: ダウンロード、 *.publishsettings*ファイルし、インポートすると、発行プロファイルを作成します。

その他の関数では、別のテンプレート ファイル (web サイト environment.template) を使用して、作成、 *web サイト environment.xml*と共に配置スクリプトで使用する設定を格納しているファイル、 *.pubxml*ファイル。

### <a name="troubleshooting-and-error-handling"></a>トラブルシューティングとエラー処理

スクリプトは、プログラムと同様に、: 失敗することし、エラーとその原因について可能な限り多くを知りたい場合は、します。 このため、環境の作成スクリプトがの値を変更、`VerbosePreference`から変数`SilentlyContinue`に`Continue`すべて詳細メッセージが表示されるようにします。 値も変更、`ErrorActionPreference`から変数`Continue`に`Stop`でもそれを検出した場合、未終了エラー、スクリプトを停止するように。

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

作業は、前に、スクリプト、開始時刻はように格納が終わったときに、経過時間を算出できます。

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

作業が完了すると、スクリプトの経過時間が表示されます。

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

すべてのキー操作スクリプトを書き込みます詳細メッセージは、たとえば。

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>配置スクリプト

新機能、*新規 AzureWebsiteEnv.ps1*スクリプトでは、環境を作成するため、*発行 AzureWebsite.ps1*スクリプトでは、アプリケーションを展開します。

配置スクリプトから web アプリの名前を取得する、 *web サイト environment.xml*環境の作成スクリプトで作成されたファイルです。

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

展開のユーザー パスワードを取得、 *.publishsettings*ファイル。

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

実行して、 [MSBuild](http://msbuildbook.com/)コマンドを構築し、プロジェクトを展開します。

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

指定した場合と、`Launch`コマンドラインでパラメーターを呼び出す、`Show-AzureWebsite`を開くには、既定の web サイトの URL をブラウザーのコマンドレットです。

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

このようなコマンドでは、配置スクリプトを実行できます。

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

クラウドで実行されているサイトとブラウザーが開きますが完了したら、 `<websitename>.azurewebsites.net` URL。

![Windows Azure にデプロイされたアプリケーションを修正します。](automate-everything/_static/image7.png)

## <a name="summary"></a>概要

これらのスクリプトを同じ手順が常に同じオプションを使用して、同じ順序で実行することを確信できます。 これにより、チームの各開発者しない誤操作または何か操作かを別のチーム メンバーの環境または実稼働環境で同じように動作しない実際には自分のコンピューターにカスタムのものを展開します。

同様の方法で行うことができます、管理ポータルで、REST API、Windows PowerShell スクリプト、.NET 言語 API、または Linux またはファルダで実行できる Bash ユーティリティを使用して、ほとんどの Azure の管理機能を自動化できます。

[次のチャプター](source-control.md)ソース コードを拝見し、説明理由は、ソース コード リポジトリで、スクリプトを含める必要があります。

## <a name="resources"></a>リソース

- [インストールし、Azure の Windows PowerShell を構成](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)です。 Azure PowerShell コマンドレットをインストールする必要がある、Azure を管理するためにコンピューターに明らかにすること、証明書をインストールする方法について説明します。 これは、PowerShell 自体を学習するためのリソースへのリンクが作業を開始する最適な場所です。
- [Azure スクリプト センター](https://docs.microsoft.com/azure/automation/automation-runbook-gallery)です。 チュートリアル、コマンドレット リファレンス ドキュメントおよびソース コード、およびサンプル スクリプトを取得するへのリンクを使用して Azure サービスを管理するスクリプトを開発するためのリソースに WindowsAzure.com ポータル
- [週末 Scripter: Azure と PowerShell の開始](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx)です。 Windows PowerShell に専用のブログでは、この投稿は、Azure の管理機能の PowerShell を使用して優れた概要を提供します。
- [インストールし、構成、Azure クロスプラット フォーム コマンドライン インターフェイス](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)です。 システム Mac と Linux ウィンドウと同様に動作する Azure スクリプト フレームワークの入門チュートリアルです。
- [Azure Sdk のダウンロードとツール」のコマンド ライン ツール「](https://azure.microsoft.com/downloads/)です。 ドキュメントおよびコマンド ライン ツールに関連する Azure のダウンロードに関するポータル ページです。
- [Azure 管理ライブラリおよび .NET のすべてを自動化する](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)です。 Scott Hanselman には、Azure 用の .NET 管理 API が導入されています。
- [Windows PowerShell スクリプトを使用して、開発とテスト環境に公開する](https://msdn.microsoft.com/library/azure/dn642480.aspx)です。 使用する方法を説明する MSDN ドキュメントでは、web プロジェクトの Visual Studio を自動的に生成するスクリプトを発行します。
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597)です。 Visual Studio 拡張機能を Visual Studio での Windows PowerShell 言語のサポートを追加します。

>[!div class="step-by-step"]
[前へ](introduction.md)
[次へ](source-control.md)
