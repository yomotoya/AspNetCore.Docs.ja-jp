---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Visual Studio を使用して ASP.NET Web 展開: のトラブルシューティング |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 65cd5cd9f7d1f9c5fdaea9b0d16bdfd84259efdd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837123"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Visual Studio を使用して ASP.NET Web 展開: のトラブルシューティング
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


このページには、Visual Studio を使用して ASP.NET web アプリケーションを展開するときに発生する可能性がある一般的な問題について説明します。 1 つの 1 つ以上の考えられる原因と対応するソリューションは提供されます。

表示されるシナリオは、Azure とサード パーティのホスティング プロバイダーの両方に適用されます。 Azure App Service で web アプリのトラブルシューティングの詳細については、次のリソースを参照してください。

- [Visual Studio を使用した Azure App Service での Web アプリのトラブルシューティング](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Azure App Service で Web アプリの監視](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [.NET 用の Windows Azure SDK 2.0 のリリースの発表](http:// https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net)(ScottGu のブログでは、Visual Studio で診断ログを取得する方法を示します)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>サーバー エラーからリモートで表示されている現在のカスタム エラーの設定が、エラーの詳細を防ぐため '/' アプリケーション - で

### <a name="scenario"></a>シナリオ

リモート ホストに、サイトを展開した後は、メンションの Web.config ファイルの customErrors 設定が、実際のエラーの原因が示されていませんが、エラー メッセージを取得します。

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

既定では、ASP.NET では、web アプリケーションがローカル コンピューターで実行されている場合にのみに詳細なエラー情報が表示されます。 一般に、web アプリケーションが、ハッカーがこの情報を使用して、アプリケーションの脆弱性を検索することができますので、インターネット経由で公開されている使用可能なときに、詳細なエラー情報を表示したくないです。 ただし、サイト、サイトまたは更新プログラムを展開する場合に場合があります何かが悪化して実際のエラー メッセージを取得する必要があります。

リモート ホストで実行されるときに、詳細なエラー メッセージを表示するアプリケーションを有効にするには、customErrors モードを off に設定、アプリケーションを再デプロイ、およびアプリケーションを再度実行するには、Web.config ファイルを編集します。

1. アプリケーションの Web.config ファイルでは、thesystem.web 要素に acustomErrors 要素が含まれる場合は、"off"に themode 属性を変更します。 それ以外の場合要素に追加 acustomErrors 要素 thesystem.web themode 属性が"off"に設定された次の例に示すようにします。 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. アプリケーションをデプロイします。
3. アプリケーションを実行し、どの前に行ったエラーが発生する原因となったを繰り返します。 ここでは、実際のエラー メッセージを確認できます。
4. エラーを解決するときに、customErrors の元の設定を復元し、アプリケーションを再デプロイします。

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>できませんを作成/シャドウ コピー 'ContosoUniversity' そのファイルが既に存在する場合。

### <a name="scenario"></a>シナリオ

Visual Studio でプロジェクトを実行するときは、次の例のようなメッセージを示すエラー ページを取得します。

'/' アプリケーションにサーバー エラーがあります。 できませんを作成/シャドウ コピー 'ContosoUniversity' そのファイルが既に存在する場合。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

ちょっと待ってくださいとブラウザーを更新するまたはサイトを再コンパイルし、もう一度実行していることをお試しください。

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>SQL Server の使用を最適化することで、Web ページ アクセスが拒否されました

### <a name="scenario"></a>シナリオ

SQL Server Compact を使用するサイトを展開すると、データベースにアクセスするデプロイされたサイトのページを実行する、次のエラー メッセージを参照してください。

アクセスが拒否されました。 (HRESULT からの例外: 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

サーバー上のネットワーク サービス アカウントがされている SQL Service Compact のネイティブ バイナリを読めるようにする必要があります、 *bin\amd64*または*bin\x86*フォルダーでは、これは読み取り権限がないそれらのフォルダー。 ネットワーク サービスのアクセス許可の読み取りの設定、 *bin*フォルダー、サブフォルダーへのアクセス許可を拡張することを確認します。

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>十分なアクセス許可により、構成ファイルを読み取ることができません。

### <a name="scenario"></a>シナリオ

クリックすると、Visual Studio の発行、ローカル コンピューター上の IIS にアプリケーションを配置する発行が失敗して、**出力**ウィンドウは次のようなエラー メッセージを表示。

IIS 構成ファイル 'マシン/リダイレクト' を読み込むときにエラーが発生しました。 この操作を実行する id がしています.エラー: 十分なアクセス許可により、構成ファイルを読み取ることができません。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

1 回のクリックを使用するローカル コンピューター上の IIS に発行、管理者のアクセス許可を持つ Visual Studio を実行する必要があります。 Visual Studio を閉じてから、管理者権限で再起動します。

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>対象のコンピューターに接続できませんでした.指定されたプロセスを使用します。

### <a name="scenario"></a>シナリオ

クリックすると、Visual Studio の発行、アプリケーションの展開ボタン発行が失敗して、**出力**ウィンドウは次のようなエラー メッセージを表示。

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

プロキシ サーバーは、移行先サーバーとの通信を中断することです。 Windows コントロール パネルから、または Internet Explorer で、選択**インターネット オプション**を選択し、**接続**タブ。**インターネット プロパティ**ダイアログ ボックスで、をクリックして**LAN の設定**します。 **ローカル エリア ネットワーク (LAN) 設定** ダイアログ ボックスで、クリア、**設定を自動的に検出**チェック ボックスをオンします。 [発行] ボタンを再度クリックします。

問題が解決しない場合は、プロキシまたはファイアウォールの設定で何ができる、システム管理者に問い合わせてください。 Web Deploy Web 管理サービスの展開 (8172); の非標準ポートを使用するために、問題が発生します。他の接続では、Web Deploy はポート 80 を使用します。 サード パーティのホスティング プロバイダーに展開する場合に通常 Web 管理サービスを使用しています。

## <a name="default-net-40-application-pool-does-not-exist"></a>既定の .NET 4.0 のアプリケーション プールが存在しません

### <a name="scenario"></a>シナリオ

.NET Framework 4 が必要なアプリケーションを展開するときに、次のエラー メッセージを参照してください。

既定の .NET 4.0 アプリケーション プールが存在しないか、アプリケーションを追加できませんでした。 このコンピューターで ASP.NET 4.0 がインストールされていることを確認してください。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

IIS では、ASP.NET 4 がインストールされていません。 配置するサーバーは、開発用コンピューターに Visual Studio 2010 をインストールする場合は、ASP.NET 4 がコンピューターにインストールされますが、IIS にインストールされていない可能性があります。 展開して、サーバーでコマンド プロンプトを開くし、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。 詳細については、このシリーズではテスト環境のチュートリアルとして IIS に展開するを参照してください。

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>初期化文字列の形式は、インデックス 0 から始まる位置の仕様に準拠していません。

### <a name="scenario"></a>シナリオ

1 回のクリックを使用してアプリケーションを展開した後の発行、次のエラー メッセージを取得したデータベースにアクセスするページを実行するとき。

初期化文字列の形式は、インデックス 0 から始まる位置の仕様に準拠していません。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

開く、 *Web.config*ファイルで、展開されたサイトと接続文字列の値が $ で始まるかどうかを確認 (ReplacableToken\_、次の例。

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

接続文字列は、この例のように、プロジェクト ファイルを編集し、次のプロパティをすべてのビルド構成である PropertyGroup 要素に追加。

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

アプリケーションを再デプロイします。

## <a name="http-500-internal-server-error"></a>HTTP 500 内部サーバー エラー

### <a name="scenario"></a>シナリオ

デプロイされたサイトを実行するとは、エラーの原因を示す特定の情報がない場合、次のエラー メッセージを表示します。

HTTP エラー 500 - 内部サーバー エラー。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

500 のエラーの原因の多くがありますが、これらのチュートリアルをフォローしている場合は、考えられる原因の 1 つは XML 要素を Web.config 変換ファイルのいずれかで間違った場所に配置すること。 挿入する変換を配置する場合にこのエラーが得られるなど、&lt;場所&gt;要素  &lt;system.web&gt;の代わりの直下にある&lt;構成&gt;します。 Web.config 変換のプレビュー機能を使用すると、変換が意図したとおりに動作していることを確認します。 コーディングがある変換を検索する場合は、変換ファイルを修正して再デプロイします。 エラーが明らかな場合は、変換をコメント アウトし、500 エラーの原因を確認する 1 つの再デプロイします。

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 内部サーバー エラー

### <a name="scenario"></a>シナリオ

デプロイされたサイトを実行すると、次のエラー メッセージを参照してください。

HTTP エラー 500.21 - 内部サーバー エラーです。 「PageHandlerFactory 統合」ハンドラーが、そのモジュールの一覧で無効なモジュール"ManagedPipelineHandler"。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

サイトが、ASP.NET 4、ASP.NET 4 に登録されていない IIS サーバー上のターゲットを導入しています。 サーバーで管理者特権のコマンド プロンプトを開き、次のコマンドを実行して、ASP.NET 4 を登録します。

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。 詳細については、このシリーズではテスト環境のチュートリアルとして IIS に展開するを参照してください。

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>アプリには、SQL Server Express のデータベースを開くはログインできませんでした\_データ

### <a name="scenario"></a>シナリオ

更新する、 *Web.config*ファイルとしての SQL Server Express データベースを指す接続文字列、 *.mdf*ファイル、*アプリ\_データ*フォルダー、および最初次のエラー メッセージが表示アプリケーションを実行する時刻:

System.data.sqlclient.sqlexception: には、データベース"DatabaseName"ログインで要求を開くことができません。 The login failed.\(ログインに失敗しました。\)

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

名前、 *.mdf*削除した場合でも、ファイルがコンピューターには、これまで存在していた SQL Server Express のデータベースの名前を一致ことはできません、 *.mdf*既存のデータベースのファイル。 名前を変更、 *.mdf*ファイル変更とデータベースの名前として使用されていない名前を*Web.config*ファイルを新しい名前を使用します。 代わりに、使用することができます[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)データベースを以前の既存の SQL Server Express を削除します。

## <a name="model-compatibility-cannot-be-checked"></a>チェックするモデルの互換性ことはできません。

### <a name="scenario"></a>シナリオ

更新する、 *Web.config*ファイルを新しい SQL Server Express データベースを指す接続文字列と、初めてアプリケーションを実行する次のエラー メッセージを表示します。

データベースにモデルのメタデータが含まれていないために、モデルの互換性をチェックできません。 IncludeMetadataConvention が DbModelBuilder 規則に追加されたことを確認します。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

場合に、Web.config ファイルを配置するデータベース名がコンピューターには、データベースが既に存在するいくつかのテーブルを含む前にこれまで使用されます。 前に、のコンピューターと変更に使用されていない新しい名前を選択、 *Web.config*ファイルをポイントして、この新しいデータベース名を使用します。 代わりに、使用することができます[SQL Server Express ユーティリティ](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)または[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)を既存のデータベースを削除します。

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>スクリプトがユーザーまたはロールを作成しようとしたときに、SQL エラー

### <a name="scenario"></a>シナリオ

構成されているデータベースの配置を使用している、**パッケージ化/発行 SQL**  タブで、デプロイ時に実行される SQL スクリプトなどのユーザーの作成またはロールの作成のコマンドをスクリプトの実行が失敗したこれらのコマンドを実行するとします。 次のように、メッセージの詳細を参照して可能性があります。

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

データベースの配置を構成したときにこのエラーが発生、 **Web の発行**ウィザードではなく、**パッケージ化/発行 SQL**  タブでスレッドを作成、[構成し、展開](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)フォーラム、およびソリューションは、このトラブルシューティングのページに追加されます。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

展開の実行を使用しているユーザー アカウントには、ユーザーまたはロールを作成するアクセス許可がありません。 たとえば、ホスティング企業が、db を割り当てることができます\_datareader、db\_、datawriter の各と db\_ddladmin ロールを設定するユーザー アカウントにします。 これらは、十分なユーザーまたはロールの作成ではなく、ほとんどのデータベース オブジェクトを作成するためです。 エラーを回避する方法の 1 つは、データベースの配置からのユーザーとロールを除外することです。 次の属性が含まれるようにデータベースの自動生成されたスクリプトの PreSource 要素を編集して、これを行うことができます。

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

プロジェクト ファイルで PreSource 要素を編集する方法については、次を参照してください。[方法: プロジェクト ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)します。 ユーザーまたはロールは、開発用データベースでは、移行先データベースに存在する必要がある場合、は、ホスティング プロバイダーに問い合わせてしてください。

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>デプロイ時にカスタム スクリプトを実行するときに、SQL Server のタイムアウト エラー

### <a name="scenario"></a>シナリオ

デプロイ時に、実行するカスタムの SQL スクリプトを指定したし、タイムアウトと Web 配置を実行して、します。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

別のトランザクション モードを持つ複数のスクリプトを実行すると、タイムアウト エラーが発生することができます。 既定では、自動的に生成されたスクリプトは、トランザクションで実行しますが、カスタム スクリプトはありません。 選択した場合、**データや既存のデータベースからスキーマをプル**オプション、**パッケージ化/発行 SQL**  タブで、カスタム SQL スクリプトを追加する場合は、いくつかのスクリプトでのトランザクション設定を変更する必要がありますようにすべてのスクリプトでは、同じトランザクションの設定を使用します。 詳細については、次を参照してください。[方法: Web アプリケーション プロジェクトでのデータベース配置](https://msdn.microsoft.com/library/dd465343.aspx)します。

すべてが同じになるように、トランザクションの設定を構成したが、このエラーが引き続き発生すると場合、考えられる回避策とは別にスクリプトを実行します。 **データベース スクリプト**グリッドで、**パッケージ化/発行**SQL タブで、、 **Include**タイムアウト エラーが発生するスクリプトのチェック ボックスは、プロジェクトを発行します。 移動し、**データベース スクリプト**グリッド、スクリプトを選択します**Include**チェック ボックスをオンし、オフ、 **Include**他のスクリプトのチェック ボックスを。 プロジェクトをもう一度発行します。 この時間を発行すると、選択したカスタム スクリプトのみが実行されます。

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>サイトのマニフェストの Stream データはまだ使用できません。

### <a name="scenario"></a>シナリオ

使用してパッケージをインストールする場合、 *deploy.cmd*ファイルを t (テスト) オプションでは、次のエラー メッセージを表示します。

エラー: のストリーム データが、' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' はまだ使用できません。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

エラー メッセージでは、コマンドがテスト レポートを生成できないことを意味します。 ただし、y (実際のインストール) オプションを使用する場合、コマンドは実行可能性があります。 メッセージは、テスト モードでコマンドを実行に問題があることにのみを示します。

## <a name="this-application-requires-managedruntimeversion-v40"></a>このアプリケーションに ManagedRuntimeVersion v4.0 が必要です。

### <a name="scenario"></a>シナリオ

デプロイしようとすると、次のエラー メッセージを参照してください。

使用しようとしているアプリケーション プールが、'managedRuntimeVersion' プロパティが 'v2.0' に設定します。 このアプリケーションでは、'v4.0' が必要です。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

IIS では、ASP.NET 4 がインストールされていません。 配置するサーバーは、開発用コンピューターに Visual Studio 2010 をインストールする場合は、ASP.NET 4 がコンピューターにインストールされますが、IIS にインストールされていない可能性があります。 展開して、サーバーでコマンド プロンプトを開くし、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Microsoft.Web.Deployment.DeploymentProviderOptions をキャストできません。

### <a name="scenario"></a>シナリオ

パッケージを展開する場合に、次のエラー メッセージを参照してください。

型 'Microsoft.Web.Deployment.DeploymentProviderOptions' を ' Microsoft.Web.Deployment.DeploymentProviderOptions' のオブジェクトをキャストできません。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

Web Deploy 2.0 がインストールされているサーバーに Web デプロイ 1.1 UI を使用して IIS マネージャーからのデプロイしようとしました。 チェック、パッケージをインポートして展開する IIS のリモート管理ツールを使用している場合、**機能利用可能な新しい** ダイアログ ボックス、接続を確立するときにします。 (このダイアログ ボックスがありますのみ表示されます 1 回、接続が最初に確立されたときに。 接続を消去し、最初からやり直す、IIS マネージャーを閉じますと inetmgr を入力して、もう一度開始が/コマンド プロンプトでリセットします。)機能のいずれかが表示されている場合は、 **Web デプロイ UI**、8 よりも低いバージョン番号があるを展開して、サーバーは 1.1 と 2.0 の両方のバージョンの Web 配置がインストールされている必要があります。 2.0 をインストールしているクライアントからのデプロイ、サーバーにのみ Web Deploy 2.0 をインストールする必要があります。 この問題を解決するのには、ホスティング プロバイダーに連絡する必要があります。

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>SQL Server Compact のネイティブ コンポーネントを読み込めませんでした。

### <a name="scenario"></a>シナリオ

デプロイされたサイトを実行すると、次のエラー メッセージを参照してください。

SQL Server Compact の 8482 のバージョンの ADO.NET プロバイダーに対応のネイティブ コンポーネントを読み込めません。 SQL Server Compact の正しいバージョンをインストールします。 詳細については、KB 記事 974247 を参照してください。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

展開されたサイトがない*amd64*と*x86*アプリケーションの下にネイティブ アセンブリを含むサブフォルダー *bin*フォルダー。 SQL Server Compact がインストールされているコンピューターで、ネイティブ アセンブリ内にある*C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private*します。 Visual Studio プロジェクトに適切なフォルダーに正しいファイルを取得する最善の方法では、NuGet SqlServerCompact パッケージをインストールします。 パッケージのインストールにネイティブ アセンブリをコピーするビルド後のスクリプトを追加します*amd64*と*x86*します。 これらを展開するためには、ただし、手動で、プロジェクトに追加する必要があります。 詳細については、次を参照してください。、[を展開する SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)チュートリアル。

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Entity Framework Code First アプリケーションのデプロイ後にエラーが「パスが無効です」

### <a name="scenario"></a>シナリオ

SQL Server Compact、アプリ内のファイルにそのデータベースを格納するなど、Entity Framework Code First Migrations と DBMS を使用するアプリケーションを展開する\_データ フォルダー。 Code First Migrations の最初のデプロイ後にデータベースを作成するように構成があります。 アプリケーションを実行するときに、次の例のようなエラー メッセージを取得します。

パスが無効です。 データベースのディレクトリを確認します。 [Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

コードは、データベースが、アプリを作成する試行が最初\_データ フォルダーが存在しません。 ファイルがあるいないか、*アプリ\_データ*フォルダーを展開したか、選択したときに**を除外するアプリ\_データ**上、**パッケージ化/発行 Web**プロジェクトのプロパティ ウィンドウのタブ。 サーバーにコピーするフォルダーにファイルがない場合、展開プロセス サーバーのフォルダーは作成されません。 展開プロセスがファイルを削除、サイトの設定、データベースがある場合、*アプリ\_データ*フォルダー自体を選択した場合**転送先に追加のファイルを削除**で発行プロファイル。 問題を解決するには、内の .txt ファイルなどのプレース ホルダー ファイルを配置、*アプリ\_データ*フォルダーがないことを確認**を除外するアプリ\_データ**を選択し、再デプロイします。

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>「基になる RCW から分割された COM オブジェクト使用できません。」

### <a name="scenario"></a>シナリオ

正常にされている必要が 1 回のクリックを使用してアプリケーションをデプロイする発行開始してこのエラーが発生します。

Web deploy タスクに失敗しました。 (リモート エージェントの URL に要求を完了できませんでした '<https://serverurl.com/msdeploy.axd?site=sitename>' です)。  
 リモート エージェントの URL に要求を完了できませんでした '<https://url/msdeploy.axd?site=sitename>'。  
要求が中止されました: 要求が取り消されました。  
基になる RCW から分割された COM オブジェクトを使用できません。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

閉じると、Visual Studio を再起動して、通常このエラーを解決するために必要な。

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>デプロイが失敗したため、ユーザー資格情報の使用がない公開 setACL 機関

### <a name="scenario"></a>シナリオ

発行が失敗することを示すエラーは、(ユーザー アカウントを使用している必要はありません setACL 機関) フォルダーのアクセス許可を設定する権限を必要はありません。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

既定では、Visual Studio の設定、サイトのルート フォルダーに対する読み取り権限と書き込みアクセス許可をアプリで\_データ フォルダー。 追加することで、この動作を無効にするサイトのフォルダーの既定のアクセス許可が正しいことと、設定する必要はありませんがわかっている場合 **&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** (1 つのプロファイルに影響を与える) に発行プロファイル ファイルをまたは (にすべてのプロファイルの影響を与える) wpp.targets ファイル。 これらのファイルを編集する方法については、次を参照してください。[方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)します。

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>アプリケーションがアプリケーション フォルダーへの書き込みを試みると、アクセス拒否エラー

### <a name="scenario"></a>シナリオ

アプリケーション エラーそのフォルダーに対する書き込み権限があるないため、作成、またはアプリケーションのフォルダーのいずれかのファイルを編集しようとします。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

既定では、Visual Studio の設定、サイトのルート フォルダーに対する読み取り権限と書き込みアクセス許可をアプリで\_データ フォルダー。 アプリケーションでは、サブ フォルダーへの書き込みアクセスを必要とする場合は、このシリーズでは、運用環境のチュートリアルをフォルダーのアクセス許可の設定と展開で示すようにそのフォルダーのアクセス許可を設定できます。 ルート フォルダーに追加することで読み取り専用アクセスを設定することを防ぐことがある場合は、アプリケーションでは、サイトのルート フォルダーへの書き込みアクセスが必要な **&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** (1 つのプロファイルに影響を与える) に発行プロファイル ファイルをまたは (にすべてのプロファイルの影響を与える) wpp.targets ファイル。 これらのファイルを編集する方法については、次を参照してください。[方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)します。

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>構成エラー - targetFramework 属性は、.NET Framework のインストールされているバージョンより後のバージョンを参照します。

### <a name="scenario"></a>シナリオ

ASP.NET 4.5 を対象とする web プロジェクトを正常に発行したが、次のエラーが発生した (customErrors モードを"off"に、web.config ファイルに設定) をアプリケーションを実行する場合。

'TargetFramework' 属性、&lt;コンパイル&gt;のみをターゲット バージョン 4.0 と .NET Framework の以降の Web.config ファイルの要素が使用されます (たとえば、'&lt;コンパイル targetFramework =「4.0」&gt;'). 現在、'targetFramework' 属性は、.NET Framework のインストールされているバージョンより後のバージョンを参照します。 有効な対象とする .NET Framework のバージョンを指定するか、必要な .NET Framework のバージョンをインストールします。

エラー ページの [ソースのエラー] ボックスは、エラーの原因として、web.config ファイルから次の行を示しています。

&lt;compilation targetFramework="4.5" /&gt;

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

サーバーは、ASP.NET 4.5 をサポートしていません。 ASP.NET 4.5 用のサポートを追加できるかどうかを判断するホスティング プロバイダーにお問い合わせください。 ASP.NET 4 またはそれ以前を対象とする web プロジェクトを配置する必要がある場合は、サーバーをアップグレードすることは、オプションではありません、代わりにします。

同じ宛先へ、ASP.NET 4 またはそれ以前の web プロジェクトを展開する場合は、選択、**転送先に追加のファイルを削除**チェック ボックスをオン、**設定**のタブ、 **Web の発行**ウィザード。 選択しない場合**転送先に追加のファイルを削除**、構成エラー ページを取得する続行されます。

プロジェクト**プロパティ**windows には、ターゲット フレームワークのドロップダウン リストが含まれていますが、だけを変更することでこの問題を解決できない **.NET Framework 4.5**に **.NET Framework 4**. 以前の framework バージョンをターゲット フレームワークを変更した場合、プロジェクトは framework の以降のバージョンのアセンブリへの参照が残っているし、は実行されません。 手動でこれらの参照を変更または .NET Framework 4 以前を対象とする新しいプロジェクトを作成する必要があります。 詳細については、次を参照してください。 [Web サイトの .NET Framework Targeting](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)します。

## <a name="medium-trust-errors"></a>中程度の信頼のエラー

### <a name="scenario"></a>シナリオ

運用環境でアプリケーションを実行する場合は、中程度の信頼に関連したエラーを取得します。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

多くのサード パーティのホスティング プロバイダーは、中程度の信頼があり、行うことが許可されていないものがあることを意味するで、web サイトを実行します。 たとえば、アプリケーション コードことはできません、Windows レジストリへのアクセスしことはできません読み取りまたは書き込みフォルダー階層のアプリケーションの外部にあるファイル。 既定では、アプリケーションの実行*完全信頼*ローカル コンピューターにつまりが運用環境にデプロイするときに失敗するための作業を行うことが、アプリケーションである可能性があります。

トラブルシューティングのために、ローカルの IIS 環境での中程度の信頼で実行するアプリケーションを構成することができます。 そのためには、アプリケーションを開きます*Web.config*ファイルを開き、追加、**信頼**内の要素、 **system.web**要素は、この例で示すようにします。

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

アプリケーションは、ローカル コンピューター上でも今すぐ IIS での中程度の信頼で実行されます。

この場合は Azure が中程度の信頼を要求していないため、Azure App Service にデプロイします。 このチュートリアルは 2012 年 2 月で記述されている時にアプリケーションを中程度の信頼で実行させるのにはこのメソッドを使用して、エラーが発生 azure。

Entity Framework Code First Migrations を使用しているアプリケーションを中程度の信頼で実行されているホスティング プロバイダーにデプロイする場合は、バージョン 5.0 を行ったことを確認または後でインストールされていること。 Entity Framework バージョン 4.3 での移行には、データベース スキーマを更新するには完全な信頼が必要です。

## <a name="http-40417-not-found-error"></a>HTTP 404.17 が見つからないというエラー

### <a name="scenario"></a>シナリオ

IIS で開発用コンピューターに展開されたサイトを実行すると、サーバーで Default.aspx を処理できないことを報告する次のエラー メッセージを参照してください。

HTTP Error 404.17 - が見つかりませんでした。

要求されたコンテンツにはスクリプトが表示され、静的ファイル ハンドラーは発生しません。

### <a name="possible-cause-and-solution"></a>考えられる原因とソリューション

ASP.NET 4.5 をコンピューターにインストールされていない可能性があります。 ASP.NET 4.5 をインストールする方法を説明するこのシリーズではテスト環境のチュートリアルとしては、IIS に展開の手順を参照してください。

> [!div class="step-by-step"]
> [前へ](deploying-extra-files.md)
