---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Web パッケージを展開する |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web... を使用して、リモート サーバーに web 配置パッケージを発行する方法について説明します
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 5d3af0fdcc6e7ae20194ba658e0cf72ad22c1234
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="deploying-web-packages"></a>Web パッケージを展開します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、(Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールを使用して、リモート サーバーに web 配置パッケージを発行する方法について説明 2.0。
> 
> 2 つのメイン方法 web パッケージをリモート サーバーを展開できます。
> 
> - MSDeploy.exe コマンド ライン ユーティリティを直接使用することができます。
> - 実行することができます、 *[プロジェクト名].deploy.cmd*ビルド プロセスによって生成されるファイルです。
> 
> 最終的な結果は、使用するための方法に関係なく同じです。 基本的には、すべて、 *. deploy.cmd*ファイルがいくつかのあらかじめ決められた値を使用して MSDeploy.exe を実行するパッケージを展開するために多くの情報を提供する必要はありません。 これには、展開プロセスが簡略化します。 その一方で、MSDeploy.exe を直接使用する柔軟性はるかに多くを正確に、パッケージを展開する方法。
> 
> 使用する方法はさまざまな要因によって異なりますどの程度制御など、展開プロセスを必要し、Web デプロイのリモート エージェント サービスまたは Web デプロイ ハンドラーのどちらを対象としているかどうか。 このトピックでは、それぞれのアプローチを使用する方法について説明し、それぞれの方法が適切である場合を特定します。
> 
> タスクとこのトピックの「チュートリアル仮定します。
> 
> - ビルドを」の説明に従って、web アプリケーションをパッケージ化した[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。
> - 変更した、 *SetParameters.xml* 」の説明に従って、ターゲット、環境内の適切なパラメーター値を提供するファイル[Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)です。


実行して、[*プロジェクト名*]*. deploy.cmd*ファイルは web パッケージを配置する最も簡単な方法です。 具体的を使用して、 *. deploy.cmd*ファイルは、これら MSDeploy.exe を直接使用する利点を提供しています。

- Web 配置パッケージの場所を指定する必要はありません&#x2014;、 *. deploy.cmd*ファイル既に知っていることができます。
- 場所を指定する必要はありません、 *SetParameters.xml*ファイル&#x2014;、 *. deploy.cmd*ファイル既に知っていることができます。
- 送信元と送信先の MSDeploy プロバイダーを指定する必要はありません&#x2014;、 *. deploy.cmd*ファイルを使用する値を既に知っています。
- MSDeploy 操作の設定を指定する必要はありません&#x2014;、 *. deploy.cmd*ファイル一般的に必要な値 MSDeploy.exe コマンドを自動的に追加します。

使用する前に、 *. deploy.cmd*ファイル web パッケージを配置することを確認する必要があります。

- *. Deploy.cmd*ファイルで、[*プロジェクト名*].*SetParameters.xml*ファイル、および web のパッケージ ([*プロジェクト名*].*zip*) が同じフォルダーにします。
- Web Deploy (MSDeploy.exe) が実行しているコンピューターにインストールされている、 *. deploy.cmd*ファイル。

*. Deploy.cmd*ファイルには、さまざまなコマンド ライン オプションがサポートされています。 コマンド プロンプトから、ファイルを実行するときにこの基本的な構文を示します。


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


どちらかを指定する必要があります、 **/T**フラグまたは**/Y**試行または実際の展開をそれぞれ実行するかどうかを示すために、フラグ (同じコマンド内に両方のフラグを使用しないでください)。 次の表では、これらのフラグの目的について説明します。

| フラグ | 説明 |
| --- | --- |
| **/T** | MSDeploy.exe を呼び出す、 **-whatif**を試用版の実行を示すフラグ。 パッケージを展開するのではなく、パッケージを展開している場合、何が起こるのレポートを作成します。 |
| **/Y** | せず MSDeploy.exe を呼び出す、 **-whatif**フラグ。 これは、ローカル コンピューターまたは指定された移行先サーバーにパッケージを展開します。 |
| **/M** | 移行先サーバーを指定する名前を付けるか、サービスの URL。 ここで指定できる値の詳細については、次を参照してください。、**エンドポイントに関する考慮事項**」セクションを参照します。 省略した場合、 **/M**フラグは、パッケージは、ローカル コンピューターに配置されます。 |
| **/A** | MSDeploy.exe が、展開の実行に使用する認証の種類を指定します。 指定できる値は**NTLM**と**基本**です。 省略した場合、 **/A**フラグ、認証の種類のデフォルト**NTLM**にされ、Web デプロイのリモート エージェント サービスの展開の**基本的な**Web Deploy への展開のハンドラー。 |
| **/U** | ユーザー名を指定します。 これは、基本認証を使用している場合にのみ適用されます。 |
| **/P** | パスワードを指定します。 これは、基本認証を使用している場合にのみ適用されます。 |
| **/L** | パッケージがローカルの IIS Express のインスタンスに配置することを示します。 |
| **/G** | 使用してパッケージを配置することを指定します、 [tempAgent プロバイダー設定](https://technet.microsoft.com/library/ee517345(WS.10).aspx)です。 省略した場合、 **/G**フラグを既定値は**false**です。 |

> [!NOTE]
> という名前のファイルも作成、ビルド処理では、web のパッケージを作成するたびに*[プロジェクト名] .deploy readme.txt*これらの展開オプションをについて説明します。


これらのフラグだけでなく追加として Web Deploy 操作の設定を指定できます*. deploy.cmd*パラメーター。 指定する追加の設定は、基になる MSDeploy.exe コマンドを単純にを通して渡されます。 これらの設定の詳細については、次を参照してください。 [Web Deploy 操作 Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx)です。

実行して、テスト環境に ContactManager.Mvc web アプリケーション プロジェクトを展開すると、 *. deploy.cmd*ファイル。 Web デプロイのリモート エージェント サービスを使用して、テスト環境が構成されている」の説明に従って[Web 配置発行 (リモート エージェント) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)です。 Web アプリケーションを展開するには、次の手順を完了する必要があります。

**使用して web アプリケーションを展開します deploy.cmd ファイル。**

1. ビルドして」の説明に従って、web アプリケーション プロジェクトをパッケージ[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。
2. 変更、 *ContactManager.Mvc.SetParameters.xml* 」の説明に従って、テスト環境を適切なパラメーター値を格納するファイル[Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)です。
3. コマンド プロンプト ウィンドウを開きの場所に移動、 *ContactManager.Mvc.deploy.cmd*ファイル。
4. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

この例では、次のように記述されています。

- **/Y**フラグは、実際には、パッケージを展開するではなく実行、試用版を行うことを示します。
- **/M**フラグは TESTWEB1 をという名前のサーバーにパッケージを展開することを示します。 この値から MSDeploy.exe 試みます Web デプロイのリモート エージェントのサービスにパッケージを展開するhttp://TESTWEB1/MSDeployAgentServiceです。
- **/A**フラグは、NTLM 認証を使用することを示します。 そのため、ユーザー名とパスワードを指定する必要はありません。

説明するために使用して、 *. deploy.cmd*ファイルには、展開プロセスが簡略化され、生成を取得しを実行するときに実行される MSDeploy.exe コマンドを見て*ContactManager.Mvc.deploy.cmd*上記のオプションを使用します。


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


使用する方法について、 *. deploy.cmd*ファイルを web パッケージを展開しを参照してください[する方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)です。

## <a name="using-msdeployexe"></a>Using MSDeploy.exe

使用していますが、 *. deploy.cmd*ファイルが一般に、展開プロセスを簡略化、状況によっては MSDeploy.exe を直接使用することをお勧めときにします。 例えば:

- 使用することはできません、管理者以外のユーザーとしての Web 展開ハンドラーを展開する場合、 *. deploy.cmd*ファイル。 これが原因で Web Deploy 2.0 では、バグの下の説明に従って**エンドポイントに関する考慮事項**です。
- 別の間で手動で切り替える場合*SetParameters.xml*異なる場所にファイル、MSDeploy.exe を直接使用する望ましい場合もあります。
- いくつかの MSDeploy.exe コマンドライン引数をオーバーライドする場合は、MSDeploy.exe を直接使用することができます。

MSDeploy.exe を使用する場合は、次の 3 つの重要な情報を提供する必要があります。

- A **– ソース**を示す、データの取得先のパラメーターです。
- A **– dest**にデータを移動する場所を示すパラメーターです。
- A **– 動詞**パラメーターを示す、[操作](https://technet.microsoft.com/library/dd568989(WS.10).aspx)を実行します。

MSDeploy.exe が依存[Web Deploy プロバイダー](https://technet.microsoft.com/library/dd569040(WS.10).aspx)元とコピー先のデータを処理します。 Web Deploy にで使用するアプリケーションとデータ ソースの範囲を表す、プロバイダーの多くが含まれる&#x2014;などの SQL Server データベース、IIS web サーバー、証明書、グローバル アセンブリ キャッシュ (GAC) アセンブリでは、プロバイダーにはさまざまな異なる構成ファイル、および多数の他のデータ型。 両方の**– ソース**パラメーターおよび**– dest**パラメーターは、フォームで、プロバイダーを指定する必要があります**– ソース**: [*providerName*] = [*場所*] です。 を IIS の web サイトに web パッケージを配置するときに、これらの値を使用する必要があります。

- **– ソース**プロバイダーは、常に[パッケージ](https://technet.microsoft.com/library/dd569019(WS.10).aspx)です。 例えば:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **– Dest**プロバイダーは、常に[自動](https://technet.microsoft.com/library/dd569016(WS.10).aspx)です。例えば:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **– 動詞**は常に**同期**です。

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

さらに、する必要がありますを指定するさまざまな他の[プロバイダーに固有の設定](https://technet.microsoft.com/library/dd569001(WS.10).aspx)や一般的な[操作設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)です。 たとえば、ステージング環境に ContactManager.Mvc web アプリケーションを展開するとします。 展開は、Web 配置のハンドラーを対象し、基本認証を使用する必要があります。 Web アプリケーションを展開するには、次の手順を完了する必要があります。

**MSDeploy.exe を使用して web アプリケーションを展開するには**

1. ビルドして」の説明に従って、web アプリケーション プロジェクトをパッケージ[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)です。
2. 変更、 *ContactManager.Mvc.SetParameters.xml* 」の説明に従って、ステージング環境用の適切なパラメーター値を格納するファイル[Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)です。
3. コマンド プロンプト ウィンドウを開き、MSDeploy.exe の場所を指定します。 これは、通常 %PROGRAMFILES%\IIS\Microsoft Web 展開 V2\msdeploy.exe にします。
4. このコマンドを入力し、Enter キーを押します (改行記号を無視)。

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

この例では、次のように記述されています。

- **– ソース**パラメーターを指定します、**パッケージ**プロバイダー、web のパッケージの場所を示します。
- **– Dest**パラメーターを指定します、**自動**プロバイダー。 **ComputerName**設定が移行先サーバーで、Web 配置のハンドラーのサービスの URL を提供します。 **空の authtype**設定ことを示し、基本認証を使用するよう指定する必要があります、 **username**と**パスワード**です。 最後に、 **includeAcls ="False"**設定は、移行先サーバーに、ソース web アプリケーションで、ファイルのアクセス制御リスト (Acl) をコピーしないことを示します。
- **– 動詞: 同期**引数は、移行先サーバー上のソース コンテンツをレプリケートすることを示します。
- **– DisableLink**引数は、アプリケーション プール、仮想ディレクトリの構成、または移行先サーバーで Secure Sockets Layer (SSL) 証明書をレプリケートしないことを示します。 詳細については、次を参照してください。 [Web 展開リンク拡張](https://technet.microsoft.com/library/dd569028(WS.10).aspx)です。
- **– SetParamFile**パラメーターの場所を提供する、 *SetParameters.xml*ファイル。
- **– AllowUntrusted**スイッチでは、Web Deploy を受け付けることが信頼された証明機関によって発行されていない SSL 証明書を示します。 Web 展開ハンドラーに配置して、サービスの URL をセキュリティで保護する自己署名証明書を使用した場合、は、このスイッチを含める必要があります。

## <a name="automating-web-package-deployment"></a>Web パッケージの展開を自動化します。

多数のエンタープライズ シナリオでは、大規模なシングル ステップまたは自動化された展開の一部として、web のパッケージを配置するします。 実行して、web のパッケージを配置を選択するかどうかに関係なく、 *. deploy.cmd*ファイルまたは MSDeploy.exe を直接使用すると、コマンドをパラメーター化し Microsoft Build Engine (MSBuild) 内のターゲットからこれらを呼び出すプロジェクト ファイルです。

連絡先のマネージャーのサンプル ソリューションで見て、 **PublishWebPackages**ターゲットを*Publish.proj*ファイル。 このターゲットは、それぞれに 1 回実行*. deploy.cmd*という名前の項目リストで識別されるファイル**PublishPackages**です。 ターゲットの各引数の値の完全なセットを構築するプロパティと項目のメタデータを使用して*. deploy.cmd*ファイルおよび、使用、 **Exec**コマンドを実行するタスク。


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> プロジェクト ファイルのモデルのサンプル ソリューションと全般にカスタム プロジェクト ファイルの概要についての広範な概要については、次を参照してください。[プロジェクト ファイルを理解する](understanding-the-project-file.md)と[ビルド プロセスの理解](understanding-the-build-process.md)です。


## <a name="endpoint-considerations"></a>エンドポイントに関する考慮事項

実行して、web のパッケージを展開するかどうかに関係なく、 *. deploy.cmd*ファイルまたは MSDeploy.exe を直接使用すると、コンピューター名または展開にサービス エンドポイントを指定する必要があります。

移行先の web サーバーが Web デプロイのリモート エージェント サービスを使用した展開構成されている場合は、先として、ターゲット サービスの URL を指定します。


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


または、単独でサーバー名を指定するには、転送先を Web 配置と、リモート エージェント サービスの URL が推論されます。


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


移行先の web サーバーが、Web 配置のハンドラーを使用した展開構成されている場合は、先として、IIS Web 管理サービス (WMSvc) のエンドポイント アドレスを指定する必要があります。 既定では、この形式になります。


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


いずれかを使用してこれらのエンドポイントのいずれかの対象にすることができます、 *. deploy.cmd*ファイルまたは直接 MSDeploy.exe です。 ただし、」の説明に従って、管理者以外のユーザーとして、Web 配置のハンドラーを配置する場合[Web 配置発行 (Web 配置ハンドラー) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)、サービス エンドポイントのアドレスにクエリ文字列を追加する必要があります。


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


これは、管理者以外のユーザーが iis サーバー レベルのアクセスを持っていないためそのユーザーは、特定の IIS web サイトへのアクセスのみがします。 Web 発行パイプライン (WPP) で、バグが原因の書き込み時に実行することはできません、 *. deploy.cmd*ファイルのクエリ文字列が含まれるエンドポイント アドレスを使用します。 このシナリオでは、MSDeploy.exe を直接使用して、web のパッケージを展開する必要があります。

> [!NOTE]
> Web デプロイのリモート エージェント サービスと Web 展開ハンドラーの詳細については、次を参照してください。 [Web 配置を右側の方法を選択する](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)です。 これらのエンドポイントを展開する、環境固有のプロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。


## <a name="authentication-considerations"></a>認証に関する注意点

実行して、web のパッケージを展開するかどうかに関係なく、 *. deploy.cmd*ファイルまたは MSDeploy.exe を直接使用すると、認証の種類を指定する必要があります。 Web Deploy は、2 つの値を受け入れます。 **NTLM**または**基本**です。 基本認証を指定する場合は、ユーザー名とパスワードを入力する必要があります。 さまざまな要因の認証の種類を選択すると認識する必要があります。

- を、Web デプロイのリモート エージェント サービスに配置する場合は、NTLM 認証を使用する必要があります。 リモート エージェント サービスは、基本認証資格情報をそのまま使用しません。
- を、Web 配置のハンドラーに配置する場合は、NTLM または基本認証のいずれかを使用できます。 既定の設定は、基本認証です。 基本認証は、ユーザー名とパスワードがプレーン テキストで送信中に依存するように Web 配置ハンドラーが常に SSL 暗号化を使用して、資格情報は保護されます。
- 場合は、web のパッケージには、データベースが含まれています。 web サーバーとデータベース サーバーが別のコンピューター、することはできませんために NTLM 認証を使用してデータベースを展開する、 [NTLM"ダブルホップ"制限](https://go.microsoft.com/?linkid=9805120)です。 展開の接続文字列で SQL Server 資格情報を使用するか、Web Deploy に基本認証資格情報を指定する必要があります。 この問題がでさらに詳しく記載されている[エンタープライズ環境にメンバーシップ データベースを展開する](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)です。

## <a name="conclusion"></a>まとめ

このトピックでは、展開する方法、web のパッケージを実行して、いずれかの説明されている、 *. deploy.cmd*ファイルまたは MSDeploy.exe を直接使用しています。 これには、それぞれの方法は、適切な可能性があり、パラメーター化して、大規模なシングル ステップまたは自動化されたビルド プロセスの一部として展開コマンドを実行する方法が説明されているがについて説明します。

## <a name="further-reading"></a>関連項目

作成し、web 配置パッケージのパラメーター化する方法のガイダンスについては、次を参照してください。[パッケージ Web アプリケーション プロジェクトのビルドと](building-and-packaging-web-application-projects.md)と[Web パッケージの展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)です。 ビルドし、Team Foundation Server (TFS) のインスタンスから web パッケージを配置する方法のガイダンスについては、次を参照してください。 [Web 配置の自動化の Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)です。 カスタマイズし、展開プロセスをトラブルシューティングする方法については、次を参照してください。[除外ファイルおよびフォルダーの展開から](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md)です。

> [!div class="step-by-step"]
> [前へ](configuring-parameters-for-web-package-deployment.md)
> [次へ](deploying-database-projects.md)
