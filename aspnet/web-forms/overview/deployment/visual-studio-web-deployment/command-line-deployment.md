---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Visual Studio を使用して ASP.NET Web のデプロイ: コマンドラインのデプロイ |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 9b2832dd23f68f9283983b52a85e8a9d5f85fd2f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829297"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Visual Studio を使用して ASP.NET Web のデプロイ: コマンドラインからデプロイ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、Visual Studio web を呼び出す方法、コマンドラインからのパイプラインを発行します。 これはし場合に便利です[デプロイ プロセスを自動化](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)行う代わりに、手動で Visual Studio で、通常を使用して、[ソース コードのバージョン管理システム](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)。

## <a name="make-a-change-to-deploy"></a>変更をデプロイするには

現在、[About] ページでは、テンプレート コードが表示されます。

![[About] ページのテンプレート コード](command-line-deployment/_static/image1.png)

受講者の登録の概要を表示するコードに置き換えます。

開く、 *About.aspx*  ページで、すべての内部マークアップを削除、 `MainContent` `Content`要素、およびその場所に次のマークアップを挿入します。

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

プロジェクトを実行し、選択、**について**ページ。

![About ページ](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>コマンドラインを使用してテストを展開します。

もう 1 つのデータベースの変更、aspnet ContosoUniversity データベースのための無効化 dbDacFx データベース展開を展開しません。 開く、 **Web の発行**ウィザード、および、3 つの発行プロファイルをクリア、 **Update Database**チェック ボックスをオン、**設定**タブ。

Windows 8 のスタート ページで、検索**VS2012 用開発者コマンド プロンプト**します。

アイコンを右クリックして**VS2012 用開発者コマンド プロンプト**クリック**管理者として実行**します。

ソリューション ファイルへのパスをソリューション ファイルへのパスに置き換えて、コマンド プロンプトで次のコマンドを入力します。

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild では、ソリューションがビルドされ、テスト環境に展開します。

![コマンド ライン出力](command-line-deployment/_static/image3.png)

ブラウザーを開きに移動`http://localhost/ContosoUniversity`、 をクリックし、**について**ページ展開が成功したことを確認します。

学生のテストを作成していない場合は、下の空のページが表示されます、**学生本文の統計情報**見出し。 移動して、**受講者**] ページで [**受講者の追加**、いくつかの受講者を追加し、しに戻ります、**について**学生の統計情報を表示するページ。

![テスト環境でのページについて](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>キーのコマンド ライン オプション

ソリューション ファイルのパスと 2 つのプロパティを MSBuild に渡されるが入力したコマンド:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>個々 のプロジェクトの配置とソリューションのデプロイ

ソリューション ファイルを指定すると、すべてのプロジェクトがビルドするために、ソリューションとします。 ソリューションに複数の web プロジェクトがあれば、次の MSBuild の動作が適用されます。

- コマンドラインで指定したプロパティは、すべてのプロジェクトに渡されます。 そのため、各 web プロジェクトには、指定した名前の発行プロファイルが必要です。 指定した場合`/p:PublishProfile=Test`、各 web プロジェクトがという名前の発行プロファイルをいる必要があります*テスト*します。
- 正常にもう 1 つを構築することも時に 1 つのプロジェクトを発行する可能性があります。 詳細については、stackoverflow スレッドを参照してください。 [2 つのパッケージに MSBuild が失敗した](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages)します。

ソリューションではなく、個々 のプロジェクトを指定する場合は、Visual Studio のバージョンを指定するパラメーターを追加する必要があります。 Visual Studio 2012 を使用している場合、コマンド ラインが次の例のようになります。

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Visual Studio 2010 のバージョン番号は 10.0 です。 詳細については、次を参照してください。 [Visual Studio プロジェクトの互換性および VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi's ブログ。

### <a name="specifying-the-publish-profile"></a>発行プロファイルを指定します。

発行プロファイルを指定するには、名前またはへの完全パスで、 *.pubxml*ファイルを次の例に示すようにします。

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Web の発行コマンド ライン パブリッシングでサポートされるメソッド

発行方法の 3 つのコマンドラインによる発行がサポートされます。

- `MSDeploy` Web Deploy を使用して発行します。
- `Package` Web デプロイ パッケージを作成して発行します。 作成した MSBuild コマンドから個別にパッケージをインストールする必要があります。
- `FileSystem` -指定したフォルダーにファイルをコピーして発行します。

### <a name="specifying-the-build-configuration-and-platform"></a>ビルド構成とプラットフォームを指定します。

Visual Studio またはコマンド ラインでは、ビルド構成とプラットフォームを設定する必要があります。 発行プロファイルがという名前のプロパティを含める`LastUsedBuildConfiguration`と`LastUsedPlatform`が、プロジェクトをビルドする方法を決定するためにこれらのプロパティを設定することはできません。 詳細については、次を参照してください。 [MSBuild: 構成プロパティを設定する方法](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)Sayed Hashimi's ブログ。

## <a name="deploy-to-staging"></a>ステージングへのデプロイします。

Azure にデプロイするには、コマンドラインにパスワードを追加する必要があります。 暗号化された形式で格納されているが Visual Studio で発行プロファイルでパスワードを保存した場合は、 *. >.pubxml.user*ファイル。 コマンド ライン パラメーターでパスワードを渡すことが、コマンドラインの展開を行うと、そのファイルは MSBuild によってアクセスされません。

1. 必要なパスワードをコピー、 *.publishsettings*ステージング web サイトを以前ダウンロードしたファイル。 パスワードの値である、 `userPWD` Web デプロイのための属性`publishProfile`要素。

    ![Web デプロイのパスワード](command-line-deployment/_static/image5.png)
2. Windows 8 のスタート ページで、検索**VS2012 用開発者コマンド プロンプト**、コマンド プロンプトを開く アイコンをクリックします。 (に管理者として開きます今度はローカル コンピューターの IIS に配置を行わないためする必要はありません)。
3. ソリューション ファイルへのパスをパスに、ソリューション ファイルとパスワードを自分のパスワードに置き換えて、コマンド プロンプトで次のコマンドを入力します。

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    このコマンドラインに追加のパラメーターが含まれることに注意してください:`/p:AllowUntrustedCertificate=true`します。 このチュートリアルが書き込まれていると、`AllowUntrustedCertificate`コマンドラインから Azure に発行するときに、プロパティを設定する必要があります。 このバグの修正プログラムがリリースされると、そのパラメーターは必要ありません。
4. ブラウザーを開き、ステージング サイトの URL に移動して をクリックし、**について**ページ展開が成功したことを確認します。

    テスト環境の既に説明したように統計を表示するいくつかの受講者を作成する必要があります、**について**ページ。

## <a name="deploy-to-production"></a>運用環境にデプロイします。

運用環境に展開するプロセスは、ステージング プロセスに似ています。

1. 必要なパスワードをコピー、 *.publishsettings*実稼働 web サイトを以前ダウンロードしたファイル。
2. 開いている**VS2012 用開発者コマンド プロンプト**します。
3. ソリューション ファイルへのパスをパスに、ソリューション ファイルとパスワードを自分のパスワードに置き換えて、コマンド プロンプトで次のコマンドを入力します。

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    実際の運用サイトの場合も、データベースの変更があった場合は通常をコピーする、*アプリ\_offline.htm*ファイルを展開する前に、サイトと配置の成功後に削除します。
4. ブラウザーを開き、ステージング サイトの URL に移動して をクリックし、**について**ページ展開が成功したことを確認します。

## <a name="summary"></a>まとめ

コマンドラインを使用してアプリケーションの更新プログラムを配置したようになりました。

![テスト環境でのページについて](command-line-deployment/_static/image6.png)

次のチュートリアルでは、web を拡張する方法の例が表示されますパイプラインを発行します。 この例では、プロジェクトに含まれていないファイルを配置する方法を示します。

> [!div class="step-by-step"]
> [前へ](deploying-a-database-update.md)
> [次へ](deploying-extra-files.md)
