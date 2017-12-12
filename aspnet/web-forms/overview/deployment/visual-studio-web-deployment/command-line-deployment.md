---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: "Visual Studio を使用した ASP.NET Web 展開: コマンド ライン展開 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8446b3fc05e3ef4a5a30c753c989252fd7f1a56f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Visual Studio を使用した ASP.NET Web 展開: コマンド ライン展開
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでは、Visual Studio web を起動するコマンド ラインからパイプラインを発行します。 場合に便利です[展開プロセスを自動化](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)Visual Studio で手動で行っているのではなく通常を使用して、[ソース コードのバージョン コントロール システム](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)です。

## <a name="make-a-change-to-deploy"></a>展開する変更を行う

現在、バージョン情報 ページでは、テンプレート コードが表示されます。

![テンプレート コードを含むページについて](command-line-deployment/_static/image1.png)

学生の登録の概要を表示するコードで置き換えることがあります。

開く、 *About.aspx*  ページで、すべての内部のマークアップを削除、 `MainContent` `Content`要素、およびその場所に次のマークアップを挿入します。

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

プロジェクトの実行を選択して、**に関する**ページ。

![ページについて](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>コマンドラインを使用してテストに展開します。

別のデータベースの変更、その dbDacFx データベース展開の無効化、aspnet ContosoUniversity データベースを展開しません。 開く、 **Web の発行**ウィザード、および、3 つで、発行プロファイル、クリア、**データベースの更新**チェック ボックスをオン、**設定**タブです。

Windows 8 の [スタート] ページで、検索**VS2012 用開発者コマンド プロンプト**です。

アイコンを右クリックして**VS2012 用開発者コマンド プロンプト** をクリック**管理者として実行**です。

ソリューション ファイルへのパスをソリューション ファイルへのパスに置き換えて、コマンド プロンプトで次のコマンドを入力します。

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild では、ソリューションをビルドし、テスト環境に展開します。

![コマンド ライン出力](command-line-deployment/_static/image3.png)

ブラウザーを開きに移動`http://localhost/ContosoUniversity`、クリックして、**に関する**ページを展開が成功したことを確認します。

任意の受講者でテストを作成していない場合は、下にある空のページが表示されます、**学生本文統計**見出し。 移動、**受講者** ページで、をクリックして**受講者を追加**、いくつかの受講者を追加しからに戻り、**に関する**受講者統計を表示するページ。

![テスト環境でのページについて](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>キーのコマンド ライン オプション

入力したコマンドには、MSBuild にソリューション ファイルのパスと 2 つのプロパティが渡されます。

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>個々 のプロジェクトの配置とソリューションの配置

ソリューション ファイルを指定すると、ソリューションを構築できるですべてのプロジェクトが発生します。 複数の web プロジェクト、ソリューションである場合は、次の MSBuild 動作が適用されます。

- コマンドラインで指定したプロパティは、すべてのプロジェクトに渡されます。 したがって、各 web プロジェクトには、指定した名前の発行プロファイルが必要です。 指定した場合`/p:PublishProfile=Test`、各 web プロジェクトは、という名前の発行プロファイルを持つ必要があります*テスト*です。
- もう 1 つを構築することも時にが正常に 1 つのプロジェクトを発行する可能性があります。 詳細については、stackoverflow のスレッドを参照してください。 [MSBuild が 2 つのパッケージが失敗し](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages)です。

ソリューションではなく個々 のプロジェクトを指定する場合は、Visual Studio のバージョンを指定するパラメーターを追加する必要があります。 Visual Studio 2012 を使用している場合、コマンドラインは次の例にようになります。

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Visual Studio 2010 のバージョン番号は 10.0 です。 詳細については、次を参照してください。 [Visual Studio プロジェクトの互換性と VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi ブログ。

### <a name="specifying-the-publish-profile"></a>発行プロファイルを指定します。

名前またはへの完全パスでは、発行プロファイルを指定できます、 *.pubxml*ファイルを次の例で示すようにします。

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Web は、コマンド ライン発行のためサポートされているメソッドを発行します。

発行方法の 3 つのコマンドラインによる公開でサポートされます。

- `MSDeploy`Web Deploy を使用して発行します。
- `Package`Web Deploy のパッケージを作成して発行します。 作成した、MSBuild コマンドからパッケージを個別にインストールする必要があります。
- `FileSystem`-指定したフォルダーにファイルをコピーして発行します。

### <a name="specifying-the-build-configuration-and-platform"></a>ビルド構成とプラットフォームの指定

Visual Studio またはコマンド ラインには、ビルド構成とプラットフォームを設定する必要があります。 発行プロファイルは、名前付きであるプロパティを含める`LastUsedBuildConfiguration`と`LastUsedPlatform`プロジェクトをビルドする方法を決定するためにこれらのプロパティを設定することはできません。 詳細については、次を参照してください。 [MSBuild: 構成プロパティを設定する方法](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)Sayed Hashimi ブログ。

## <a name="deploy-to-staging"></a>ステージング展開します。

に Azure を配置するには、コマンドラインに、パスワードを追加する必要があります。 暗号化された形式で格納されているが Visual Studio で、発行プロファイルで、パスワードを保存した場合は、 *. pubxml.user*ファイル。 コマンド ライン パラメーターでパスワードを渡す必要があるため、コマンドラインの展開を行うと、そのファイルは MSBuild によってアクセスされません。

1. 必要なパスワードをコピー、 *.publishsettings*ステージング web サイトを以前ダウンロードしたファイルです。 パスワードがの値、 `userPWD` Web Deploy の属性`publishProfile`要素。

    ![Web Deploy のパスワード](command-line-deployment/_static/image5.png)
2. Windows 8 の [スタート] ページで、検索**VS2012 用開発者コマンド プロンプト**、コマンド プロンプトを開き、アイコンをクリックします。 (する必要はありません、管理者として開きます今度は、ローカル コンピューター上の IIS に展開されないためです。)
3. ソリューション ファイルへのパスをソリューション ファイルと、パスワードを自分のパスワードへのパスに置き換えて、コマンド プロンプトで次のコマンドを入力します。

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    このコマンドラインに余分なパラメーターが含まれることに注意してください:`/p:AllowUntrustedCertificate=true`です。 このチュートリアルが書き込まれると、`AllowUntrustedCertificate`コマンドラインから Azure に発行するときに、プロパティを設定する必要があります。 このバグの修正プログラムがリリースされたときにそのパラメーターは必要ありません。
4. ブラウザーを開き、および、ステージング サイトの URL に移動し、クリックして、**に関する**ページを展開が成功したことを確認します。

    テスト環境の前半で説明したとおりに統計を表示するいくつかの受講者を作成する必要があります、**に関する**ページ。

## <a name="deploy-to-production"></a>実稼働環境に展開します。

実稼働環境に展開するプロセスは、ステージング処理に似ています。

1. 必要なパスワードをコピー、 *.publishsettings*前、実稼働 web サイトのダウンロードしたファイルです。
2. 開いている**VS2012 用開発者コマンド プロンプト**です。
3. ソリューション ファイルへのパスをソリューション ファイルと、パスワードを自分のパスワードへのパスに置き換えて、コマンド プロンプトで次のコマンドを入力します。

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    実際の運用サイトの場合も、データベースの変更があった場合は通常をコピーする、*アプリ\_offline.htm*展開する前に、サイトへのファイルおよび配置の成功後削除します。
4. ブラウザーを開き、および、ステージング サイトの URL に移動し、クリックして、**に関する**ページを展開が成功したことを確認します。

## <a name="summary"></a>概要

コマンドラインを使用して、アプリケーションの更新プログラムを配置したようになりました。

![テスト環境でのページについて](command-line-deployment/_static/image6.png)

次のチュートリアルでは場合、web を拡張する方法の例を参照してください。 パイプラインの発行します。 この例では、プロジェクトに含まれていないファイルを配置する方法を示します。

>[!div class="step-by-step"]
[前へ](deploying-a-database-update.md)
[次へ](deploying-extra-files.md)
