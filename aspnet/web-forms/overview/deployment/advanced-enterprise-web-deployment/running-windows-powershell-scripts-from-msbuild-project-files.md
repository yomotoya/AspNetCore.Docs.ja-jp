---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: "MSBuild プロジェクト ファイルから Windows PowerShell スクリプトを実行している |Microsoft ドキュメント"
author: jrjlee
description: "このトピックでは、ビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。 スクリプトをローカルで実行することができます (言い換えると、b 上..。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: afee7b0621df42a8bc70fc6f7c4a8fd0383fa83a
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>MSBuild プロジェクト ファイルから Windows PowerShell スクリプトの実行
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、ビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。 (つまり、ビルド サーバー上) にローカルでスクリプトを実行したり、リモートで、ターゲット web サーバーまたはデータベース サーバー上と同様にできます。
> 
> さまざまな理由により、配置後の Windows PowerShell スクリプトを実行する理由があります。 たとえば、次の操作を行います。
> 
> - カスタム イベント ソースをレジストリに追加します。
> - アップロードのファイル システム ディレクトリを生成します。
> - ビルド ディレクトリをクリーンアップします。
> - カスタム ログ ファイルにエントリを記述します。
> - 新しくプロビジョニングされた web アプリケーションへのユーザーの招待の電子メールを送信します。
> - 適切なアクセス許可を持つユーザー アカウントを作成します。
> - SQL Server インスタンス間のレプリケーションを構成します。
> 
> このトピックでは、Microsoft Build Engine (MSBuild) プロジェクト ファイルでカスタム ターゲットからローカルとリモートの両方に Windows PowerShell スクリプトを実行する方法を示します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。サンプル ソリューション & #x 2014; このチュートリアルのシリーズを使用して、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; を ASP.NET MVC 3 アプリケーションを Windows のなどの複雑性のレベルが現実的な web アプリケーションを表すCommunication Foundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスで 2 つのプロジェクト ファイル & #x 2014; 1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

自動または 1 ステップの展開プロセスの一環として、Windows PowerShell スクリプトを実行するには、これらの高度なタスクを完了する必要があります。

- ソリューションとソース管理には、Windows PowerShell スクリプトを追加します。
- Windows PowerShell スクリプトを起動するコマンドを作成します。
- コマンド内の予約済み XML 文字をエスケープします。
- カスタム MSBuild プロジェクト ファイルでターゲットを作成しを使用して、 **Exec**コマンドを実行するタスク。

このトピックでは、これらの手順を実行する方法を示します。 タスクとここでチュートリアルは、ことに慣れている既に MSBuild ターゲットおよびプロパティ、およびカスタム MSBuild プロジェクト ファイルを使用してビルドおよび配置プロセスを制御する方法を理解することを想定しています。 詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。

## <a name="creating-and-adding-windows-powershell-scripts"></a>作成して、Windows PowerShell スクリプトを追加します。

このトピックの実習では、という名前のサンプル Windows PowerShell スクリプトを使用して**LogDeploy.ps1** MSBuild からスクリプトを実行する方法について説明します。 **LogDeploy.ps1**スクリプトには、ログ ファイルに単一行エントリを書き込む単純な関数が含まれています。


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


**LogDeploy.ps1**スクリプトは 2 つのパラメーターを受け付けます。 最初のパラメーターでは、完全なパスを表す、エントリを追加するログ ファイルに、2 番目のパラメーターは、ログ ファイルに記録する、配置先を表します。 スクリプトを実行するときに、この形式でログ ファイルに行が追加されます。


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


させる、 **LogDeploy.ps1** msbuild の使用可能なスクリプト、する必要があります。

- ソース管理には、スクリプトを追加します。
- スクリプトを Visual Studio 2010 のソリューションに追加します。

ビルド サーバー上またはリモート コンピューターでスクリプトを実行する予定があるかに関係なく、ソリューションのコンテンツを含むスクリプトを展開する必要はありません。 1 つのオプションでは、ソリューション フォルダーにスクリプトを追加します。 連絡先のマネージャーの例で、展開プロセスの一部として、Windows PowerShell スクリプトを使用するため理にかなって発行ソリューション フォルダーにスクリプトを追加します。

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

ソリューション フォルダーの内容は、ソース マテリアルとしてビルド サーバーにコピーされます。 ただし、なしの一部を形成プロジェクト出力します。

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>ビルド サーバー上の Windows PowerShell スクリプトの実行

一部のシナリオでは、プロジェクトをビルドしているコンピューターで Windows PowerShell スクリプトを実行することがあります。 たとえば、ビルド フォルダーをクリーンアップまたはカスタムのログ ファイルにエントリを書き込むに Windows PowerShell スクリプトを使用する可能性があります。

構文の観点から MSBuild プロジェクト ファイルから Windows PowerShell スクリプトの実行は正規のコマンド プロンプトから Windows PowerShell スクリプトを実行すると同じです。 実行可能ファイル、powershell.exe を起動し、使用する必要があります、 **– コマンド**を実行する Windows PowerShell のコマンドを提供するスイッチです。 (Windows PowerShell v2 で使用することも、 **– ファイル**切り替える)。 コマンドは、この形式を実行する必要があります。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


例:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


スクリプトへのパスには、スペースが含まれている場合は、ファイルのパスを単一引用符の前にアンパサンドで囲む必要があります。 コマンドを囲むために既に使用しているため、二重引用符を使用することはできません。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


MSBuild からこのコマンドを呼び出す場合は、いくつか追加の考慮事項にもあります。 最初を含めるように、 **– NonInteractive**スクリプトがサイレント モードで実行されることを確認するフラグ。 次を含めるように、 **– ExecutionPolicy**適切な引数の値を含むフラグ。 これには、実行ポリシーと Windows PowerShell スクリプトが適用され、スクリプトの実行を防ぐことがあります既定の実行ポリシーをオーバーライドすることができることを指定します。 これらの引数の値から選択できます。

- 値**Unrestricted** Windows PowerShell スクリプトが署名されているかどうかに関係なく、スクリプトを実行できるようになります。
- 値**RemoteSigned**をローカル コンピューターで作成された未署名のスクリプトを実行する Windows PowerShell を許可します。 ただし、他の場所で作成されたスクリプトを署名する必要があります。 (実際には、していない場合、Windows PowerShell スクリプトをビルド サーバーでローカルに作成する多くの場合)。
- 値**AllSigned**署名済みスクリプトのみを実行する Windows PowerShell を許可します。

既定の実行ポリシーは**Restricted**、Windows PowerShell のスクリプト ファイルを実行できなくなります。

最後に、Windows PowerShell コマンドで発生する予約済み XML 文字をエスケープする必要があります。

- 単一引用符を置き換える **&amp;apos;**
- 二重引用符で置換 **&amp;quot;**
- アンパサンドを置き換える **&amp;amp;**

- これらの変更を行うと、コマンドはのようになります。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


カスタムの MSBuild プロジェクト ファイル内には、新しいターゲットを作成して使用することができます、 **Exec**タスクを次のコマンドを実行します。


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


この例に注意してください。

- パラメーター値と、Windows PowerShell 実行可能ファイルの場所と同様に、任意の変数は、MSBuild プロパティとして宣言されます。
- コマンドラインからこれらの値を上書きするようにするのには、条件が含まれています。
- **MSDeployComputerName**プロパティがプロジェクト ファイルで別の場所で宣言されています。

ビルド プロセスの一部としてこのターゲットを実行するときに、Windows PowerShell は、コマンドを実行し、指定したファイルにログ エントリを書き込みます。

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>リモート コンピューター上の Windows PowerShell スクリプトの実行

Windows PowerShell は経由でリモート コンピューターでスクリプトを実行できる[Windows リモート管理](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx)(WinRM)。 これを行うには、使用する必要があります、 [Invoke-command](https://technet.microsoft.com/library/dd347578.aspx)コマンドレット。 これにより、リモート コンピューターに、スクリプトをコピーすることがなく 1 つまたは複数のリモート コンピューターに対して、スクリプトを実行できます。 スクリプトを実行した、ローカル コンピューターには、すべての結果が返されます。

> [!NOTE]
> 使用する前に、 **Invoke-command**コマンドレットを Windows PowerShell を実行するスクリプトのリモート コンピューターで、リモートのメッセージを受け入れるように WinRM リスナーを構成する必要があります。 コマンドを実行してこれを行う**winrm quickconfig**リモート コンピューターにします。 詳細については、次を参照してください。[インストールと構成の Windows リモート管理](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx)です。


Windows PowerShell ウィンドウを実行するこの構文を使用すると、 **LogDeploy.ps1**リモート コンピューター上のスクリプト。


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> 使用する別の方法でさまざまな**Invoke-command**スクリプトを実行するファイルがこのアプローチは最も簡単なパラメーター値を提供し、スペースを含むパスを管理する必要がある場合。


コマンド プロンプトからこれを実行するときを実行可能ファイル、Windows PowerShell を起動し、使用する必要があります、 **– コマンド**指示を提供するパラメーター。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


前に、追加の一部のスイッチを提供し、MSBuild からコマンドを実行すると、予約済み XML 文字をエスケープする必要があります。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


最後に、以前と同様を使えば、 **Exec**コマンドを実行するカスタム MSBuild ターゲット内のタスク。


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Windows PowerShell はで指定したコンピューターで、スクリプトを実行して、ビルド プロセスの一部としてこのターゲットを実行するときに、 **– computername**引数。

## <a name="conclusion"></a>まとめ

このトピックでは、MSBuild プロジェクト ファイルから Windows PowerShell スクリプトを実行する方法について説明します。 このアプローチを使用すると、自動またはシングル ステップ ビルドおよび配置プロセスの一部としてローカルまたはリモート コンピューター上の Windows PowerShell スクリプトを実行します。

## <a name="further-reading"></a>関連項目

Windows PowerShell スクリプトへの署名と、実行ポリシーの管理に関するガイダンスについては、次を参照してください。 [Windows PowerShell スクリプトの実行](https://technet.microsoft.com/library/ee176949.aspx)です。 リモート コンピューターから Windows PowerShell コマンドを実行する方法については、次を参照してください。[リモート コマンドの実行](https://technet.microsoft.com/library/dd819505.aspx)です。

展開プロセスを制御するカスタム MSBuild プロジェクト ファイルを使用する方法については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスの理解](../web-deployment-in-the-enterprise/understanding-the-build-process.md)です。

>[!div class="step-by-step"]
[前へ](taking-web-applications-offline-with-web-deploy.md)
[次へ](troubleshooting-the-packaging-process.md)
