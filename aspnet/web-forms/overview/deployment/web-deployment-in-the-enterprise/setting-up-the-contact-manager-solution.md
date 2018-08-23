---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: 連絡先マネージャー ソリューションのセットアップ |Microsoft Docs
author: jrjlee
description: このトピックでは、ダウンロードしてローカル開発用ワークステーションで実行する連絡先マネージャー ソリューションを構成する方法について説明します。
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d834e6fbd2b46dca8bd7eeadc1eb68b33e62bb38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827829"
---
<a name="setting-up-the-contact-manager-solution"></a>連絡先マネージャー ソリューションの設定
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、ダウンロードしてローカル開発用ワークステーションで実行する連絡先マネージャー ソリューションを構成する方法について説明します。


## <a name="system-requirements"></a>システム要件

連絡先マネージャー ソリューションをローカルで実行して、このチュートリアルで説明されているその他のタスクを実行するには、開発者のワークステーションにこのソフトウェアをインストールする必要があります。

- Visual Studio 2010 Service Pack 1、Premium または Ultimate Edition
- インターネット インフォメーション サービス (IIS) 7.5 Express します。
- SQL Server Express 2008 R2
- IIS Web 配置ツール (Web 配置) 2.1 以降
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

Visual Studio 2010 を除くには、ダウンロードしてこれらの製品とコンポーネントを経由のすべての最新バージョンをインストールすることができます、 [Web Platform Installer](https://go.microsoft.com/?linkid=9805118)します。

## <a name="download-and-extract-the-solution"></a>ダウンロードして、ソリューションを展開します。

連絡先のマネージャーのサンプル アプリケーションをダウンロードするには、MSDN コード ギャラリーから[ここ](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0)します。

## <a name="configure-and-run-the-solution"></a>構成し、ソリューションの実行

で構成して、ローカル コンピューターに、連絡先マネージャー ソリューションを実行するには、これらの大まかな手順を実行する必要があります。

1. 既に 1 つがあるないの場合は、メンバーシップとロール管理機能を有効になっているローカル ASP.NET アプリケーション サービス データベースを作成します。
2. 内の接続文字列を編集、 *web.config*ファイルをローカルの SQL Server Express インスタンスをポイントします。
3. Visual Studio 2010 からソリューションを実行します。

このセクションの残りの部分では、これらの各タスクを完了する方法に関するガイダンスを提供します。

**アプリケーション サービス データベースを作成するには**

1. Visual Studio 2010 コマンド プロンプトを開きます。 これを実行する、**開始**メニューで、**すべてのプログラム**、 をクリックして**Microsoft Visual Studio 2010**、 をクリックして**Visual Studio Tools**、し、クリックして**Visual Studio コマンド プロンプト (2010)** します。
2. コマンド プロンプトで、このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. 使用して、 **– C**スイッチを使用するデータベース サーバーの接続文字列を指定します。
    2. 使用して、 **– A**アプリケーション サービス データベースに追加する機能を指定するスイッチ。 この場合、 **m**メンバーシップ プロバイダーのサポートを追加することを示しますと**r**ロール マネージャーのサポートを追加することを示します。
    3. 使用して、 **– d**スイッチをアプリケーション サービス データベースの名前を指定します。 このスイッチを省略した場合、ユーティリティの既定の名前を持つデータベースが作成されます**aspnetdb**します。
3. データベースが正常に作成されて、コマンド プロンプトに確認メッセージが表示されます。

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> 詳細については、aspnet\_regsql ユーティリティを参照してください[ASP.NET SQL Server の登録ツール (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx)します。


次の手順では、連絡先マネージャー ソリューション内の接続文字列が SQL Server Express のローカル インスタンスをポイントするかどうかを確認します。

**接続文字列を更新するには**

1. Visual Studio 2010 では、連絡先マネージャー ソリューションを開きます。
2. **ソリューション エクスプ ローラー**ウィンドウで、展開、 **ContactManager.Mvc**プロジェクト、し、ダブルクリック、 **Web.config**ノード。

    > [!NOTE]
    > ContactManager.Mvc プロジェクトには、2 つが含まれます*web.config*ファイル。 プロジェクト レベルのファイルを編集する必要があります。

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. **ConnectionStrings**要素、という名前の接続文字列を確認します。 **ApplicationServices** 、ローカルの ASP.NET アプリケーション サービス データベースを指します。

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. **ソリューション エクスプ ローラー**ウィンドウで、展開、 **ContactManager.Service**プロジェクト、し、ダブルクリック、 **Web.config**ノード。

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. **ConnectionStrings**という接続文字列内の要素**ContactManagerContext**、いることを確認、**データ ソース**プロパティが SQL のローカル インスタンスに設定サーバーの高速です。 接続文字列内の他に何も変更する必要はありません。

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. 開いているすべてのファイルを保存します。

ローカル コンピューターに、連絡先マネージャー ソリューションを実行する準備がなります。

> [!NOTE]
> ASP.NET は、アプリケーション サービス データベースを作成しなくてもこの手順を実行する場合は、データベースが初めてのユーザーを作成しようとしたに作成されます。 ただし、手動でデータベースの作成に制御、はるかに多くをサポートするアプリケーション サービスの機能セット。


**連絡先マネージャー ソリューションを実行するには**

1. Visual Studio 2010 で f5 キーを押します。
2. Internet Explorer が起動し、連絡先マネージャー ASP.NET MVC 3 アプリケーションの URL を要求します。 既定では、アプリケーションが表示されます、**すべての連絡先**ページ。

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. いくつかの連絡先を追加し、アプリケーションが期待どおりに動作することを確認します。

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. 参照する`http://localhost:50114/Account/Register`(別のポートでアプリケーションをホストしている場合は、URL を調整する)。 ユーザー名、電子メール アドレス、およびパスワードを追加し、アカウントを正常に登録することであることを確認します。

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. 参照する`http://localhost:50114/Account/LogOn`(別のポートでアプリケーションをホストしている場合は、URL を調整する)。 作成したアカウントを使用してログオンいただけることを確認します。

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. デバッグを停止する Internet Explorer を閉じます。

## <a name="conclusion"></a>まとめ

この時点では、ローカル コンピューター上で実行する連絡先マネージャー ソリューションを完全に構成する必要があります。 このチュートリアルでは、他のトピックに従って操作すると、参照としてソリューションを使用することができます。

次のトピックでは、[プロジェクト ファイルを理解する](understanding-the-project-file.md)、連絡先マネージャー ソリューション内では、カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、展開プロセスを制御する方法について説明します。

> [!div class="step-by-step"]
> [前へ](the-contact-manager-solution.md)
> [次へ](understanding-the-project-file.md)
