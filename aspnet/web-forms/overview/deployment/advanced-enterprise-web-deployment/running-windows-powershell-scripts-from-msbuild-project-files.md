---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: MSBuild プロジェクト ファイルからの Windows PowerShell スクリプトの実行 |Microsoft Docs
author: jrjlee
description: このトピックでは、ビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。 スクリプトをローカルで実行することができます (つまり、b の..。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362896"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>MSBuild プロジェクト ファイルからの Windows PowerShell スクリプトの実行
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、ビルドおよび配置プロセスの一部として Windows PowerShell スクリプトを実行する方法について説明します。 (つまり、ビルド サーバー) でローカル スクリプトを実行したり、送信先の web サーバーまたはデータベース サーバーなどのリモートでできます。
> 
> 配置後の Windows PowerShell スクリプトを実行するかの理由が います。 たとえば、次の操作を行います。
> 
> - カスタム イベント ソースをレジストリに追加します。
> - ファイル システム ディレクトリのアップロードを生成します。
> - ビルド ディレクトリをクリーンアップします。
> - カスタム ログ ファイルにエントリを記述します。
> - 新しくプロビジョニングされた web アプリケーションにユーザーの招待メールを送信します。
> - 適切なアクセス許可を持つユーザー アカウントを作成します。
> - SQL Server インスタンス間のレプリケーションを構成します。
> 
> このトピックでは、Microsoft Build Engine (MSBuild) プロジェクト ファイルでカスタム ターゲットからローカルとリモートの両方に、Windows PowerShell スクリプトを実行する方法を示します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

を、自動またはシングル ステップの展開プロセスの一部として Windows PowerShell スクリプトを実行するには、これらの高度なタスクを完了する必要があります。

- ソリューションをソース管理には、Windows PowerShell スクリプトを追加します。
- Windows PowerShell スクリプトを起動するコマンドを作成します。
- コマンドで、予約済みの XML 文字をエスケープします。
- カスタム MSBuild プロジェクト ファイルでターゲットを作成し、使用、 **Exec**コマンドを実行するタスク。

このトピックでは、これらの手順を実行する方法を説明します。 タスクとチュートリアルでは、このトピックでは、MSBuild ターゲットおよびプロパティ、慣れている既にことと、カスタム MSBuild プロジェクト ファイルを使用してビルドおよび配置プロセスを促進する方法を理解することを想定しています。 詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。

## <a name="creating-and-adding-windows-powershell-scripts"></a>作成して、Windows PowerShell スクリプトの追加

という名前のサンプル Windows PowerShell スクリプトを使用して、このトピックのタスクで**LogDeploy.ps1** MSBuild からスクリプトを実行する方法について説明します。 **LogDeploy.ps1**スクリプトには、ログ ファイルを単一行のエントリを書き込む単純な関数が含まれています。


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


**LogDeploy.ps1**スクリプトは 2 つのパラメーターを受け取ります。 最初のパラメーターは、エントリを追加するログ ファイルに完全なパスを表すし、2 番目のパラメーターは、ログ ファイルに記録する、配置先を表します。 スクリプトを実行するときに、この形式でログ ファイルに行が追加されます。


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


させる、 **LogDeploy.ps1** MSBuild を使用可能なスクリプト、する必要があります。

- スクリプトをソース コントロールに追加します。
- Visual Studio 2010 でのソリューションに対して、スクリプトを追加します。

ビルド サーバー上またはリモート コンピューターでスクリプトを実行するかどうかに関係なく、ソリューションのコンテンツでスクリプトを展開する必要はありません。 1 つのオプションでは、ソリューション フォルダーにスクリプトを追加します。 連絡先のマネージャーの例で、展開プロセスの一部として、Windows PowerShell スクリプトを使用するため、理にかなって発行ソリューション フォルダーにスクリプトを追加します。

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

ソリューション フォルダーの内容は、ソース マテリアルとしてビルド サーバーにコピーされます。 ただし、プロジェクト出力の一部を形成しません。

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>ビルド サーバーで Windows PowerShell スクリプトの実行

一部のシナリオで、プロジェクトをビルドしているコンピューターで Windows PowerShell スクリプトを実行する場合があります。 たとえば、ビルド フォルダーをクリーンアップまたはカスタムのログ ファイルにエントリを書き込むに Windows PowerShell スクリプトを使用する可能性があります。

構文の観点から MSBuild プロジェクト ファイルから Windows PowerShell スクリプトを実行しているが通常コマンド プロンプトから、Windows PowerShell スクリプトを実行すると同じ。 実行可能ファイル、powershell.exe を起動し、使用する必要がある、 **– コマンド**スイッチを実行する Windows PowerShell のコマンドを提供します。 (Windows PowerShell v2 を使用することも、 **– ファイル**切り替えます)。 コマンドは、この形式を実行する必要があります。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


例えば:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


スクリプトへのパスにスペースが含まれている場合は、ファイルのパスを単一引用符の前にアンパサンドで囲む必要があります。 コマンドを囲むために既に使用しているため、二重引用符を使用することはできません。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


MSBuild からこのコマンドを呼び出す場合は、いくつか追加の考慮事項にもあります。 最初に、含める必要がある、 **– NonInteractive**スクリプトをサイレント モードで実行されるようにするフラグ。 次に、含める必要がある、 **– ExecutionPolicy**フラグを適切な引数の値。 これには、Windows PowerShell はスクリプトに適用され、スクリプトの実行を防ぐことがあります既定の実行ポリシーをオーバーライドすることができます、実行ポリシーを指定します。 これらの引数の値から選択できます。

- 値**Unrestricted** Windows PowerShell スクリプトを署名するかどうかに関係なく、スクリプトを実行できるようになります。
- 値**RemoteSigned** Windows PowerShell をローカル コンピューター上に作成された符号なしのスクリプトを実行できるようになります。 ただし、別の場所で作成されたスクリプトを署名する必要があります。 (実際には、していない場合に、ビルド サーバーで Windows PowerShell スクリプトをローカルで作成した)。
- 値**AllSigned**により、署名済みスクリプトのみを実行する Windows PowerShell。

既定の実行ポリシーは**Restricted**、され、Windows PowerShell は任意のスクリプト ファイルを実行できなきます。

最後に、Windows PowerShell コマンドで発生する予約済み XML 文字をエスケープする必要があります。

- 単一引用符を置き換える **&amp;apos;**
- 二重引用符で置き換える **&amp;quot;**
- アンパサンドを置き換える **&amp;amp;**

- これらの変更を行ったときに、コマンドがこのようになります。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


カスタム MSBuild プロジェクト ファイル内には、新しいターゲットを作成して使用することができます、 **Exec**このコマンドを実行するタスク。


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


この例に注意してください。

- パラメーター値と、Windows PowerShell 実行可能ファイルの場所など、任意の変数は、MSBuild プロパティとして宣言されます。
- コマンドラインからこれらの値を上書きするユーザーを有効にするのには、条件が含まれています。
- **MSDeployComputerName**どこかプロジェクト ファイルでプロパティを宣言します。

ビルド プロセスの一部としてこのターゲットを実行するときに Windows PowerShell がコマンドを実行し、指定したファイルにログ エントリを書き込みます。

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>リモート コンピューターで Windows PowerShell スクリプトの実行

Windows PowerShell がリモート コンピューターでスクリプトを実行できる[Windows リモート管理](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx)(WinRM)。 これを行うには、使用する必要があります、 [Invoke-command](https://technet.microsoft.com/library/dd347578.aspx)コマンドレット。 これにより、スクリプトをリモート コンピューターにコピーすることがなく 1 つまたは複数のリモート コンピューターに対してスクリプトを実行できます。 スクリプトの実行に使用したローカル コンピューターには、すべての結果が返されます。

> [!NOTE]
> 使用する前に、 **Invoke-command**リモート コンピューターでスクリプトを Windows PowerShell を実行するコマンドレット、リモートのメッセージを受け入れるように WinRM リスナーを構成する必要があります。 コマンドを実行してこれを行う**winrm quickconfig**リモート コンピューター。 詳細については、次を参照してください。[インストールと構成の Windows リモート管理](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx)します。


実行するこの構文を使用する、Windows PowerShell ウィンドウから、 **LogDeploy.ps1**リモート コンピューター上のスクリプト。


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> 使用するさまざまな他の方法がある**Invoke-command**スクリプトを実行するが、この方法が最も簡単なパラメーター値を指定して、パスにスペースを管理する必要がある場合。


これをコマンド プロンプトから実行するときに実行可能ファイル、Windows PowerShell を起動して使用する必要があります、 **– コマンド**の説明を指定するパラメーター。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


前に、いくつか追加のスイッチを行い、MSBuild からコマンドを実行すると、予約済み XML 文字をエスケープする必要があります。


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


使用できると同様に、最後に、 **Exec**コマンドを実行するカスタム MSBuild ターゲット内のタスク。


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Windows PowerShell がで指定したコンピューターでスクリプトを実行、ビルド プロセスの一部としてこのターゲットを実行するときに、 **– computername**引数。

## <a name="conclusion"></a>まとめ

このトピックでは、MSBuild プロジェクト ファイルから Windows PowerShell スクリプトを実行する方法について説明します。 このアプローチを使用すると、またはシングル ステップの自動ビルドと展開プロセスの一環としてローカルまたはリモート コンピューター上の Windows PowerShell スクリプトを実行します。

## <a name="further-reading"></a>関連項目

Windows PowerShell スクリプトに署名して、実行ポリシーの管理に関するガイダンスについては、次を参照してください。 [Windows PowerShell スクリプトの実行](https://technet.microsoft.com/library/ee176949.aspx)します。 リモート コンピューターから Windows PowerShell コマンドを実行する方法の詳細については、次を参照してください。[リモート コマンドを実行している](https://technet.microsoft.com/library/dd819505.aspx)します。

カスタム MSBuild プロジェクト ファイルを使用して、展開プロセスを制御する詳細については、次を参照してください。[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)と[ビルド プロセスを理解する](../web-deployment-in-the-enterprise/understanding-the-build-process.md)します。

> [!div class="step-by-step"]
> [前へ](taking-web-applications-offline-with-web-deploy.md)
> [次へ](troubleshooting-the-packaging-process.md)
