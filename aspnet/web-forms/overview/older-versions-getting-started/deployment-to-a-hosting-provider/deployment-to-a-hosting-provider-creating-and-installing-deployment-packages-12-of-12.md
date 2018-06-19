---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します (12/12) のトラブルシューティング |。Microsoft ドキュメント
author: tdykstra
description: この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2a8342f026498a7cf3ff4a3c158ed177c15b7111
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890386"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します (12/12) のトラブルシューティング。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示しています、および Windows Azure Web サイトを展開する方法を示しますをチュートリアルでは、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


このページでは、Visual Studio を使用して、ASP.NET web アプリケーションを展開するときに発生する可能性がある一般的な問題について説明します。 1 つの 1 つ以上の考えられる原因と対応するソリューションは提供されます。

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>サーバー エラーからリモートで表示されている現在のカスタム エラー設定が、エラーの詳細を防ぐため '/' アプリケーション -

### <a name="scenario"></a>シナリオ

リモート ホストに、サイトを展開した後は、Web.config ファイルで customErrors 設定特記しているエラーの実際の原因が示されないエラー メッセージを表示します。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

既定では、web アプリケーションがローカル コンピューターで実行されている場合にのみ、ASP.NET は詳細なエラー情報を示します。 一般に、web アプリケーションは、ハッカーがこの情報を使用して、アプリケーションでの脆弱性を検索することができますので、インターネット経由で公開されている使用可能なときに詳細なエラー情報を表示するしたくありません。 ただし、サイトに、サイトまたは更新プログラムを展開することがあります正しくないものになるし、実際のエラー メッセージを取得する必要があります。

リモート ホストで実行されるときに、詳細なエラー メッセージを表示するアプリケーションを有効にするに設定する Web.config ファイルを編集`customErrors`モードがオフ、アプリケーションを再展開およびアプリケーションを再実行します。

1. アプリケーションの Web.config ファイルがある場合、`customErrors`内の要素、`system.web`要素、変更、`mode`属性を"off"にします。 それ以外の場合、追加、`customErrors`内の要素、`system.web`を持つ要素、`mode`属性では、次の例のようにを"off"に設定。

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. アプリケーションを展開します。
3. アプリケーションを実行し、すべて以前に行っていたエラーが発生の原因となったを繰り返します。 ここでは、実際のエラー メッセージを表示できます。
4. エラーを解決したら、復元元`customErrors`設定とアプリケーションを再配置します。

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>使用して SQL Server Compact、Web ページにアクセスが拒否されました

### <a name="scenario"></a>シナリオ

SQL Server Compact を使用するサイトを展開すると、データベースにアクセスする配置済みのサイトでページを実行する、次のエラー メッセージを参照してください。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

サーバー上のネットワーク サービス アカウントをされている SQL サービス Compact のネイティブ バイナリを読み取ることができる必要があります、 *bin\amd64*または*bin\x86*フォルダーが、これは読み取り権限がないそれらのフォルダーです。 ネットワーク サービスに対するアクセス許可の読み取りの設定、 *bin*フォルダー、サブフォルダーへのアクセス許可を拡張することを確認します。

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>十分なアクセス許可により、構成ファイルを読み取ることができません。

### <a name="scenario"></a>シナリオ

クリックすると、Visual Studio 公開 ボタンをローカル コンピューター上の IIS にアプリケーションを展開する発行が失敗して、**出力**ウィンドウに次のようなエラー メッセージが表示。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

1 回のクリックを使用するローカル コンピューターに IIS にパブリッシュする場合、管理者権限を持つ Visual Studio を実行する必要があります。 Visual Studio を閉じ、管理者のアクセス許可を再起動します。

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>対象のコンピューターに接続できませんでした.指定されたプロセスを使用します。

### <a name="scenario"></a>シナリオ

クリックすると、Visual Studio 公開 ボタン、アプリケーションの展開は失敗を発行し、**出力**ウィンドウに次のようなエラー メッセージが表示。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

プロキシ サーバーは、移行先サーバーとの通信を中断することです。 Windows コントロール パネルから、または Internet Explorer では、**インターネット オプション**を選択し、**接続**タブです。**インターネット プロパティ**ダイアログ ボックスで、をクリックして**LAN の設定**です。 **ローカル エリア ネットワーク (LAN) 設定**ダイアログ ボックスで、クリア、**設定を自動的に検出**チェック ボックスをオンします。 [発行] ボタンを再度クリックします。

問題が解決しない場合は、プロキシまたはファイアウォールの設定で実行できる、システム管理者に問い合わせてください。 Web Deploy Web 管理サービスの展開 (8172); の非標準ポートを使用するために、問題が発生します。他の接続では、Web Deploy はポート 80 を使用します。 サード パーティのホスティング プロバイダーに配置するときに、Web 管理サービスを通常を使用しています。

## <a name="default-net-40-application-pool-does-not-exist"></a>既定の .NET 4.0 アプリケーション プールが存在しません

### <a name="scenario"></a>シナリオ

.NET Framework 4 が必要なアプリケーションを展開するときに、次のエラー メッセージを参照してください。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

IIS で ASP.NET 4 がインストールされていません。 展開しているサーバーは、開発用コンピューターに Visual Studio 2010 がインストールする場合は、ASP.NET 4 はコンピューターにインストールされますが、IIS にインストールされていない可能性があります。 展開しているサーバーで管理者特権でコマンド プロンプトを開き、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。 詳細については、次を参照してください。、[テスト環境として IIS に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアルです。

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>初期化文字列の形式は、インデックス 0 から始まる仕様に準拠していません。

### <a name="scenario"></a>シナリオ

1 回のクリックを使用してアプリケーションを配置した後を発行、次のエラー メッセージを取得するデータベースにアクセスするページを実行する場合。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

開く、 *Web.config*ファイルで、展開されたサイトと接続文字列の値がで始まるかどうかを確認`$(ReplacableToken_`次の例のように、します。

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

接続文字列は、この例のように、プロジェクト ファイルを編集し、次のプロパティを追加、`PropertyGroup`すべてのビルド構成にある要素。

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

アプリケーションを再展開します。

## <a name="http-500-internal-server-error"></a>HTTP 500 内部サーバー エラー

### <a name="scenario"></a>シナリオ

配置済みのサイトを実行すると、エラーの原因を示す特定の情報がない場合、次のエラー メッセージが表示します。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

500 のエラーの原因の多くが、これらのチュートリアルに従う場合は、考えられる原因の 1 つは XML 変換ファイルのいずれかで間違った場所では、XML 要素を配置すること。 挿入する変換を配置する場合にこのエラーが得られるなど、`<location>`要素の下`<system.web>`の代わりに直下`<configuration>`です。 ソリューションは、XML 変換ファイルを修正して再配置する場合です。

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 内部サーバー エラー

### <a name="scenario"></a>シナリオ

配置済みのサイトを実行すると、次のエラー メッセージを参照してください。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

サイトが、ASP.NET 4 で、ASP.NET 4 に登録されていない IIS サーバー上のターゲットを展開しました。 サーバーで管理者特権でコマンド プロンプトを開き、次のコマンドを実行して、ASP.NET 4 を登録します。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

また、既定のアプリケーション プールの .NET Framework のバージョンを手動で設定する必要があります。 詳細については、次を参照してください。、[テスト環境として IIS に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアルです。

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>アプリには、SQL Server Express のデータベースを開くはログインできませんでした\_データ

### <a name="scenario"></a>シナリオ

更新する、 *Web.config*ファイルとして SQL Server Express データベースを指す接続文字列、 *.mdf*ファイルで、*アプリ\_データ*フォルダー、および最初次のエラー メッセージが表示アプリケーションを実行する時間:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

名前、 *.mdf*ファイルは、削除した場合でもがこれまで、コンピューター上に存在していた SQL Server Express のデータベースの名前に一致ことはできません、 *.mdf*が既存のデータベース ファイル。 名前を変更、 *.mdf*変更とデータベースの名前として使用されていない名前にファイル、 *Web.config*ファイルを新しい名前を使用します。 代わりに、使用することができます[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)を既存の SQL Server Express を削除するデータベースします。

## <a name="model-compatibility-cannot-be-checked"></a>チェックするモデルの互換性ことはできません。

### <a name="scenario"></a>シナリオ

更新する、 *Web.config*ファイルを新しい SQL Server Express データベースを指す接続文字列と初めてアプリケーションを実行する次のエラー メッセージを表示します。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

Web.config ファイルに配置するデータベース名がこれまで使用された場合、コンピューターにデータベースが既に存在するいくつかのテーブルを含む前にします。 前に、のコンピューターや変更に使用されていない新しい名前を選択、 *Web.config*ファイルをこの新しいデータベース名を使用 をポイントします。 代わりに、使用することができます[SQL Server Express ユーティリティ](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)または[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)を既存のデータベースを削除します。

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>スクリプトがユーザーまたはロールを作成しようとしたときに、SQL エラー

### <a name="scenario"></a>シナリオ

構成されているデータベースの配置を使用している、**パッケージ化/発行 SQL**  タブで、展開時に実行する SQL スクリプトを含める Create User またはロールの作成のコマンドとスクリプトの実行が失敗したこれらのコマンドを実行するとします。 次のように、メッセージの詳細を参照して可能性があります。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

このエラーが発生のデータベースの配置を構成したときに、 **Web の発行**ウィザードではなく、**パッケージ化/発行 SQL**  タブで、内のスレッドを作成、[構成と展開](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)フォーラム、およびソリューションは、このトラブルシューティングのページに追加されます。

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

展開の実行を使用しているユーザー アカウントには、ユーザーまたはロールを作成する権限がありません。 たとえば、ホスティング企業が割り当てることが、 `db_datareader`、 `db_datawriter`、および`db_ddladmin`するを設定するユーザー アカウントをロールします。 これらは、十分なほとんどのデータベース オブジェクトを作成するためのではなくユーザーまたはロールを作成します。 エラーを回避する方法の 1 つは、データベースの配置からのユーザーおよびロールを除外することでです。 編集することによってこれを行う、`PreSource`次の属性が含まれるように、データベースの要素のスクリプトの生成をされた自動的に。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

編集する方法については、`PreSource`プロジェクト ファイル内の要素を参照してください[する方法: プロジェクト ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)です。 ユーザーまたはロールの開発用データベースでは、転送先データベース内にある必要がある場合、は、ホスティング プロバイダーに問い合わせてしてください。

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>配置時にカスタム スクリプトを実行するときに、SQL Server のタイムアウト エラー

### <a name="scenario"></a>シナリオ

展開時に、実行するカスタムの SQL スクリプトを指定したし、Web 配置の実行時にそれらはタイムアウトが発生します。

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

別のトランザクション モードを持つ複数のスクリプトを実行すると、タイムアウト エラーが発生することができます。 既定では、自動的に生成されたスクリプトは、トランザクションで実行が、カスタム スクリプトはこれはできません。 選択した場合、**または既存のデータベースからスキーマのデータをプル** オプションを選択、**パッケージ化/発行 SQL**  タブで、カスタム SQL スクリプトを追加する場合は、いくつかのスクリプトでのトランザクション設定を変更する必要がありますとようにすべてのスクリプトは、同じトランザクション設定を使用します。 詳細については、次を参照してください。[する方法: Web アプリケーション プロジェクトでのデータベースを配置](https://msdn.microsoft.com/library/dd465343.aspx)です。

すべてが同じように、トランザクションの設定を構成した引き続きこのエラーが発生した場合、可能な回避策とは別に、スクリプトの実行を開始します。 **データベース スクリプト**グリッドで、**パッケージ化/発行**SQL タブで、、 **Include**タイムアウト エラーが発生したスクリプトのチェック ボックスは、プロジェクトを発行します。 移動し、**データベース スクリプト**グリッドで、そのスクリプトの選択**Include**チェック ボックスをオンおよびオフに、 **Include**他のスクリプトのチェック ボックスをします。 続いて、プロジェクトをもう一度発行します。 この時間を発行すると、選択したカスタム スクリプトのみを実行します。

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>サイトのマニフェストのストリーム データはまだ使用できません。

### <a name="scenario"></a>シナリオ

使用してパッケージをインストールする場合、 *deploy.cmd*ファイルと、 `t` (テスト) オプションでは、次のエラー メッセージを参照してください。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

エラー メッセージの意味、コマンドが、テスト レポートを生成できません。 ただし、コマンド実行可能性がありますを使用する、 `y` (実際のインストール) オプション。 メッセージは、テスト モードでコマンドを実行に問題があることにのみを示します。

## <a name="this-application-requires-managedruntimeversion-v40"></a>このアプリケーションに ManagedRuntimeVersion v4.0 が必要です。

### <a name="scenario"></a>シナリオ

展開しようとすると、次のエラー メッセージを参照してください。

 エラー: のストリーム データが、' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' はまだ使用できません。 'V2.0' に設定された 'managedRuntimeVersion' プロパティを使用しようとしているアプリケーション プールに、します。 このアプリケーションには、'v4.0' が必要です。 

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

IIS で ASP.NET 4 がインストールされていません。 展開しているサーバーは、開発用コンピューターに Visual Studio 2010 がインストールする場合は、ASP.NET 4 はコンピューターにインストールされますが、IIS にインストールされていない可能性があります。 展開しているサーバーで管理者特権でコマンド プロンプトを開き、次のコマンドを実行して、IIS で ASP.NET 4 をインストールします。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Microsoft.Web.Deployment.DeploymentProviderOptions をキャストできません。

### <a name="scenario"></a>シナリオ

パッケージを展開しているときに、次のエラー メッセージを参照してください。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

Web Deploy 2.0 がインストールされているサーバーに Web 配置 1.1 UI を使用して IIS マネージャーからを展開しようとするとします。 チェック、パッケージをインポートして展開する IIS のリモート管理ツールを使用している場合、**機能利用可能な新しい** ダイアログ ボックスの接続を確立するときにします。 (このダイアログ ボックスがありますのみ表示されます 1 回の接続が最初に確立されている場合。 接続を消去し、最初からやり直す、IIS マネージャーを閉じるし、開始してもう一度入力して`inetmgr /reset`コマンド プロンプトでします)。機能のいずれかがある場合は**Web 展開 UI**、および 8 よりも低いバージョン番号を展開しているサーバーは、Web Deploy のインストールの 1.1 および 2.0 の両方のバージョンを必要があります。 2.0 をインストールしているクライアントから展開するには、のみ Web Deploy 2.0 インストールがサーバーに必要です。 この問題を解決するのには、ホスティング プロバイダーに連絡する必要があります。

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>SQL Server Compact のネイティブ コンポーネントを読み込めません。

### <a name="scenario"></a>シナリオ

配置済みのサイトを実行すると、次のエラー メッセージを参照してください。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

展開されたサイトがない*amd64*と*x86*アプリケーションの下にネイティブ アセンブリを含むサブフォルダー *bin*フォルダーです。 SQL Server Compact がインストールされているコンピューターでのネイティブ アセンブリ内にある*C:\Program files \microsoft SQL Server Compact Edition\v4.0\Private*です。 Visual Studio プロジェクトに適切なフォルダーに正しいファイルを取得する最善の方法では、NuGet SqlServerCompact パッケージをインストールします。 パッケージのインストールにネイティブ アセンブリをコピーするビルド後のスクリプトを追加する*amd64*と*x86*です。 ただし、これらを展開するためには、手動で、プロジェクトに含める必要があります。 詳細については、次を参照してください。、[を展開する SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)チュートリアルです。

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Entity Framework Code First のアプリケーションを展開した後、「パスが正しくありません」エラー

### <a name="scenario"></a>シナリオ

SQL Server Compact、アプリ内のファイルにそのデータベースを格納するなど、Entity Framework Code First Migrations と DBMS を使用するアプリケーションを展開する\_データ フォルダーです。 Code First Migrations が、最初の展開の後、データベースを作成するように構成があります。 アプリケーションを実行するときに、次の例のようなエラー メッセージを取得します。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

コード最初はしようとすると、データベースが、アプリを作成する\_データ フォルダーは存在しません。 任意ファイルがあるいないか、*アプリ\_データ*フォルダーを展開したか、選択したときに**除外アプリ\_データ**上、**パッケージ化/発行 Web**のタブ、**プロジェクト プロパティ**ウィンドウです。 サーバーにコピーするフォルダーにファイルがない場合、展開プロセス サーバーのフォルダーは作成されません。 展開プロセスがファイルを削除、データベースのサイトのセットアップが既に存在する場合、*アプリ\_データ*フォルダー自体を選択した場合**先で追加ファイルを削除する**で発行プロファイル。 問題を解決するで .txt ファイルなどのプレース ホルダー ファイルを格納、*アプリ\_データ*フォルダーがないことを確認**を除外するアプリ\_データ**選択され、再展開します。 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>「基になる RCW から分割された COM オブジェクト使用できません。」

### <a name="scenario"></a>シナリオ

正常にされているアプリケーションを配置する発行 1 回のクリックを使用して開始してこのエラーが発生します。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

閉じると、Visual Studio を再起動は通常このエラーを解決するのには必要なすべてのです。

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>展開が失敗するためのユーザー資格情報が使用がない公開 setACL 機関

### <a name="scenario"></a>シナリオ

示すエラーで失敗する発行フォルダーのアクセス許可 (を使用しているユーザー アカウントは setACL 機関がありません) を設定する権限がありません。

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

既定では、Visual Studio の設定、サイトのルート フォルダーに対するアクセス許可の読み取りおよび書き込みアクセス許可アプリ\_データ フォルダーです。 追加することで、この動作を無効にするサイト フォルダの既定のアクセス許可が正しいし、設定する必要はありませんがわかっている場合**&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 発行プロファイル ファイル (1 つのプロファイルに影響を与える)、または (影響を与えるすべてのプロファイル) wpp.targets ファイル。 これらのファイルを編集する方法については、次を参照してください。[する方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)です。 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>アプリケーションがアプリケーション フォルダーへの書き込みを行うとき、アクセス拒否エラー

### <a name="scenario"></a>シナリオ

アプリケーション エラーそのフォルダーに対する書き込み権限があるないために作成やアプリケーション フォルダーのいずれかのファイルを編集しようとします。

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

既定では、Visual Studio の設定、サイトのルート フォルダーに対するアクセス許可の読み取りおよび書き込みアクセス許可アプリ\_データ フォルダーです。 示すように、そのフォルダーのアクセス許可を設定するアプリケーションでは、サブ フォルダーへの書き込みアクセスを必要とする場合、[フォルダー アクセス許可の設定](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)と[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルです。 ルート フォルダーに追加することで読み取り専用アクセスを設定することを防ぐことがある場合は、アプリケーションでは、サイトのルート フォルダーへの書き込みアクセスが必要な**&lt;IncludeSetACLProviderOn 先&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 発行プロファイル ファイル (1 つのプロファイルに影響を与える)、または (影響を与えるすべてのプロファイル) wpp.targets ファイル。 これらのファイルを編集する方法については、次を参照してください。[する方法: プロファイル (.pubxml) ファイルでの展開設定の編集](https://msdn.microsoft.com/library/ff398069.aspx)です。 <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>構成エラー - targetFramework 属性は、.NET Framework のインストールされているバージョンより後のバージョンを参照します。

### <a name="scenario"></a>シナリオ

ASP.NET 4.5 を対象とする web プロジェクトを正常に発行されたアプリケーションを実行すると、(で、`customErrors`モードは、Web.config ファイルには、「無効」に設定)、次のエラーを取得します。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

エラー ページの [ソースのエラー] ボックスでは、エラーの原因として web.config ファイルから次の行を示しています。

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>考えられる原因と解決策

サーバーは、ASP.NET 4.5 をサポートしていません。 ASP.NET 4.5 用のサポートを追加できるかどうかを決定するホスティング プロバイダーに問い合わせてください。 ASP.NET 4 以降を対象とする web プロジェクトを配置する必要がある場合は、サーバーをアップグレードすることは、オプションではありません、代わりにします。同じ送信先に ASP.NET 4 または以前の web プロジェクトを展開する場合は、選択、**先で追加ファイルを削除する**チェック ボックスをオン、**設定**のタブ、 **Web の発行**ウィザード。 選択しない場合**先で追加ファイルを削除する**、引き続き構成エラー ページを取得することができます。

プロジェクト**プロパティ**windows には、ターゲット フレームワーク ドロップダウン リストが含まれていますが、だけを変更することによってこの問題を解決することはできません **.NET Framework 4.5**に **.NET Framework 4**. 以前のフレームワーク バージョンにターゲット フレームワークを変更する場合、プロジェクトが framework の以降のバージョンのアセンブリへの参照が残っているしは実行されません。 手動でこれらの参照を変更または .NET Framework 4 以前を対象とする新しいプロジェクトを作成する必要があります。 詳細については、次を参照してください。 [Web サイトの .NET Framework のターゲット](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)です。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
