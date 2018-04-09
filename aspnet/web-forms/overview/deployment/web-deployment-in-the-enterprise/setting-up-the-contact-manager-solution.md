---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: 連絡先のマネージャー ソリューションのセットアップ |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、ダウンロードして、連絡先のマネージャー ソリューション開発用ワークステーションでローカルに実行を構成する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: e8fb24f5b2d96d864d1aa6bc0f78644773de00ab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="setting-up-the-contact-manager-solution"></a>連絡先のマネージャー ソリューションを設定します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、ダウンロードして、連絡先のマネージャー ソリューション開発用ワークステーションでローカルに実行を構成する方法について説明します。


## <a name="system-requirements"></a>システム要件

連絡先のマネージャー ソリューションをローカルで実行して、このチュートリアルで説明するその他のタスクを実行するには、開発者のワークステーションにこのソフトウェアをインストールする必要があります。

- Visual Studio 2010 Service Pack 1、Premium または Ultimate Edition
- インターネット インフォメーション サービス (IIS) 7.5 Express します。
- SQL Server Express 2008 R2
- IIS Web 配置ツール (Web 配置) 2.1 以降
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

Visual Studio 2010 を除くには、ダウンロードしてこれらの製品とコンポーネントをすべての最新バージョンをインストールすることができます、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)です。

## <a name="download-and-extract-the-solution"></a>ダウンロードして、ソリューションを展開します。

連絡先のマネージャーのサンプル アプリケーションをダウンロードするには、MSDN コード ギャラリーから[ここ](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0)です。

## <a name="configure-and-run-the-solution"></a>構成し、ソリューションの実行

構成し、ローカル コンピューター上の連絡先のマネージャー ソリューションの実行をするには、これらの大まかな手順を実行する必要があります。

1. 既に 1 つがあるない、有効になっているメンバーシップとロール管理機能をローカル ASP.NET アプリケーション サービス データベースを作成します。
2. 内の接続文字列を編集、 *web.config*ファイルは、ローカルの SQL Server Express インスタンスを参照します。
3. Visual Studio 2010 からソリューションを実行します。

このセクションの残りの部分は、これらの各タスクを完了する方法に関するガイダンスを提供します。

**アプリケーション サービス データベースを作成するには**

1. Visual Studio 2010 コマンド プロンプトを開きます。 、これを行う、**開始** メニューのをポイント**すべてのプログラム**、 をクリックして**Microsoft Visual Studio 2010**、をクリックして**Visual Studio Tools**、し、をクリックして**Visual Studio コマンド プロンプト (2010)**です。
2. コマンド プロンプトでこのコマンドを入力し、Enter キーを押します。

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. 使用して、 **– C**スイッチを使用するデータベース サーバーの接続文字列を指定します。
    2. 使用して、 **–**アプリケーション サービス データベースに追加する機能を指定するスイッチです。 この場合、 **m**メンバーシップ プロバイダーのサポートを追加することを示すと**r**ロール マネージャーのサポートを追加することを示します。
    3. 使用して、 **– d**アプリケーション サービス データベースの名前を指定するスイッチです。 このスイッチを省略した場合、ユーティリティの既定の名前とデータベースが作成されます**aspnetdb**です。
3. データベースが正常に作成されて、コマンド プロンプトに、確認メッセージが表示されます。

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> 詳細については、aspnet\_regsql ユーティリティを参照してください[ASP.NET SQL Server の登録ツール (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)です。


次の手順では、連絡先のマネージャー ソリューション内の接続文字列が SQL Server Express のローカル インスタンスを指していることを確認します。

**接続文字列を更新するには**

1. Visual Studio 2010 での連絡先のマネージャー ソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、展開、 **ContactManager.Mvc**プロジェクト、し、ダブルクリック、 **Web.config**ノード。

    > [!NOTE]
    > ContactManager.Mvc プロジェクトに含まれる 2 つ*web.config*ファイル。 プロジェクト レベル ファイルを編集する必要があります。

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. **ConnectionStrings**要素、接続文字列がという名前のことを確認**ApplicationServices**ローカルの ASP.NET アプリケーション サービス データベースを指しています。

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. **ソリューション エクスプ ローラー**ウィンドウで、展開、 **ContactManager.Service**プロジェクト、し、ダブルクリック、 **Web.config**ノード。

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. **ConnectionStrings**という名前の接続文字列内の要素**ContactManagerContext**、いることを確認、**データソース**プロパティが SQL のローカル インスタンスに設定サーバーの高速です。 接続文字列内の他の何も変更する必要はありません。

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. 開いているすべてのファイルを保存します。

ローカル コンピューター上の連絡先のマネージャー ソリューションを実行する準備ができます。

> [!NOTE]
> ASP.NET は、アプリケーション サービス データベースを作成しなくてもこの手順を実行する場合は、データベースがユーザーを作成しようとする最初の時間に作成されます。 ただし、手動でデータベースの作成を制御できますはるかに多くをサポートするアプリケーション サービス機能のセット。


**連絡先のマネージャー ソリューションを実行するには**

1. Visual Studio 2010 で F5 キーを押します。
2. Internet Explorer の起動および連絡先のマネージャー ASP.NET MVC 3 アプリケーションの URL を要求します。 既定では、アプリケーションが表示されます、**すべての連絡先**ページ。

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. いくつかの連絡先を追加し、アプリケーションが期待どおりに動作することを確認します。

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. 参照`http://localhost:50114/Account/Register`(別のポートでアプリケーションをホストしている場合は、URL を調整して)。 ユーザー名、電子メール アドレスとパスワードを追加し、アカウントを正常に登録することがあることを確認します。

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. 参照`http://localhost:50114/Account/LogOn`(別のポートでアプリケーションをホストしている場合は、URL を調整して)。 作成したアカウントを使用してログオンすることがあることを確認します。

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. デバッグを停止する Internet Explorer を閉じます。

## <a name="conclusion"></a>まとめ

この時点では、ローカル コンピューター上で実行する連絡先のマネージャー ソリューションを完全に構成する必要があります。 このチュートリアルでは、その他のトピックに従って操作するときに、参照として、ソリューションを使用できます。

次のトピックでは、[プロジェクト ファイルを理解する](understanding-the-project-file.md)、展開プロセスを制御する連絡先のマネージャー ソリューション内では、カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用する方法について説明します。

> [!div class="step-by-step"]
> [前へ](the-contact-manager-solution.md)
> [次へ](understanding-the-project-file.md)
