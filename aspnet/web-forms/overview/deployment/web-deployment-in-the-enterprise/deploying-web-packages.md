---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Web パッケージの配置 |Microsoft Docs
author: jrjlee
description: このトピックでは、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web... を使用して、リモート サーバーに web 配置パッケージを発行する方法について説明します
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 44c3772263cbebaf03e1393542fda32467a97cd8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825266"
---
<a name="deploying-web-packages"></a>Web パッケージを展開します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、インターネット インフォメーション サービス (IIS) の Web 配置ツール (Web 配置) を使用して、リモート サーバーに web 配置パッケージを発行する方法について説明します 2.0。
> 
> リモート サーバーに web パッケージを展開する 2 つの主な方法はあります。
> 
> - MSDeploy.exe コマンド ライン ユーティリティを直接使用することができます。
> - 実行することができます、 *[プロジェクト名].deploy.cmd*ビルド プロセスによって生成されるファイル。
> 
> 最終的な結果は、使用する方法にかかわらず同じです。 基本的には、すべて、 *. deploy.cmd*ファイルは、パッケージをデプロイするには多くの情報を提供する必要があるないように、いくつかの事前に定義された値に MSDeploy.exe を実行することです。 これには、展開プロセスが簡略化します。 その一方で、MSDeploy.exe を直接使用する柔軟性、はるかに多く、パッケージの配置を正確に経由。
> 
> 使用する方法はさまざまな要因によって異なります、展開プロセスを必要とどの程度制御など、Web デプロイのリモート エージェントのサービスまたは Web 配置ハンドラーのどちらを対象としているかどうか。 このトピックでは、それぞれのアプローチを使用する方法について説明し、それぞれのアプローチは適切なタイミングを識別します。
> 
> タスクとこのトピックの「チュートリアル仮定します。
> 
> - 」の説明に従って、web アプリケーションをパッケージ化し、[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。
> - 変更した、 *SetParameters.xml* 」の説明に従って、ターゲット環境の適切なパラメーター値を提供するファイル[Web パッケージ展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)します。


実行して、[*プロジェクト名*]*. deploy.cmd*ファイルは web パッケージを配置する最も簡単な方法です。 具体的を使用して、 *. deploy.cmd*ファイルは、これら MSDeploy.exe を直接使用する利点を提供しています。

- Web デプロイ パッケージの場所を指定する必要はありません&#x2014;、 *. deploy.cmd*ファイルが既にあるを知っています。
- 場所を指定する必要はありません、 *SetParameters.xml*ファイル&#x2014;、 *. deploy.cmd*ファイルが既にあるを知っています。
- ソースと宛先の MSDeploy プロバイダーを指定する必要はありません&#x2014;、 *. deploy.cmd*ファイルに使用するには、どの値が既に認識しています。
- MSDeploy 操作設定を指定する必要はありません&#x2014;、 *. deploy.cmd*ファイル一般的に必要な値 MSDeploy.exe コマンドを自動的に追加します。

使用する前に、 *. deploy.cmd*ファイル、web パッケージを展開することを確認する必要があります。

- *. Deploy.cmd*ファイルで、[*プロジェクト名*].*SetParameters.xml*ファイル、および web パッケージ ([*プロジェクト名*].*zip*) と同じフォルダーにします。
- Web Deploy (MSDeploy.exe) が実行しているコンピューターにインストールされている、 *. deploy.cmd*ファイル。

*. Deploy.cmd*ファイルには、さまざまなコマンド ライン オプションがサポートしています。 コマンド プロンプトから、ファイルを実行するときにこの基本的な構文を示します。


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


いずれかを指定する必要があります、 **/T**フラグまたは **/Y**試用版の実行または実際の展開をそれぞれ実行するかどうかを示すフラグ、(同じコマンド内に両方のフラグを使用しないでください)。 このテーブルには、これらのフラグの目的について説明します。

| フラグ | 説明 |
| --- | --- |
| **/T** | MSDeploy.exe を呼び出すと、 **– whatif**を試用版の実行を示すフラグをします。 パッケージを配置するのではなく、パッケージを展開している場合、何が起こるのレポートを作成します。 |
| **/Y** | MSDeploy.exe を呼び出します、 **– whatif**フラグ。 これは、パッケージがローカル コンピューターまたは指定された移行先サーバーにデプロイします。 |
| **/M** | 移行先サーバーを指定します。 名前を付けるか、サービスの URL。 ここで指定できる値の詳細については、次を参照してください。、**エンドポイントに関する考慮事項**このトピックの「します。 省略した場合、 **/M**フラグは、パッケージは、ローカル コンピューターに展開されます。 |
| **/A** | MSDeploy.exe が、展開の実行に使用する認証の種類を指定します。 指定できる値は**NTLM**と**基本的な**します。 省略した場合、 **/A**フラグは、既定の認証の種類**NTLM**にされ、Web デプロイのリモート エージェント サービスの展開の**基本的な**Web Deploy への展開ハンドラー。 |
| **/U** | ユーザー名を指定します。 これは、基本認証を使用している場合にのみ適用されます。 |
| **/P** | パスワードを指定します。 これは、基本認証を使用している場合にのみ適用されます。 |
| **/L** | パッケージがローカルの IIS Express のインスタンスに配置することを示します。 |
| **/G** | 使用して、パッケージを配置することを指定します、 [tempAgent プロバイダー設定](https://technet.microsoft.com/library/ee517345(WS.10).aspx)します。 省略した場合、 **/G**フラグは、既定値は**false**します。 |

> [!NOTE]
> という名前のファイルも作成、ビルド プロセスでは、web パッケージを作成するたびに*readme.txt-[プロジェクト名] .deploy*これらの展開オプションをについて説明します。


これらのフラグに加え、追加として Web 配置操作の設定を指定できます *. deploy.cmd*パラメーター。 追加設定を指定するは、基になる MSDeploy.exe コマンドに渡さだけです。 これらの設定の詳細については、次を参照してください。 [Web 配置操作の設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)します。

実行して、テスト環境に ContactManager.Mvc web アプリケーション プロジェクトを展開すると、 *. deploy.cmd*ファイル。 」の説明に従って、Web デプロイのリモート エージェント サービスを使用するテスト環境が構成されている[Web 配置発行 (リモート エージェント) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)します。 Web アプリケーションを配置するには、次の手順を完了する必要があります。

**使用して web アプリケーションを展開します deploy.cmd ファイル。**

1. ビルドおよび」の説明に従って、web アプリケーション プロジェクトをパッケージ化[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。
2. 変更、 *ContactManager.Mvc.SetParameters.xml* 」の説明に従って、テスト環境内の適切なパラメーターの値を格納するファイル[Web パッケージ展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)します。
3. コマンド プロンプト ウィンドウを開きの場所に移動し、 *ContactManager.Mvc.deploy.cmd*ファイル。
4. このコマンドを入力し、Enter キーを押します。

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

この例では、次のように記述されています。

- **/Y**フラグは、ことを実際には、パッケージを展開するではなく実行、試用版を行うことを示します。
- **/M**フラグは TESTWEB1 という名前のサーバーにパッケージを展開することを示します。 Web デプロイのリモート エージェントのサービスにパッケージを展開する試行は MSDeploy.exe からこの値は、 http://TESTWEB1/MSDeployAgentService します。
- **/A**フラグは、NTLM 認証を使用することを示します。 そのため、ユーザー名とパスワードを指定する必要はありません。

説明するためを使用する方法、 *. deploy.cmd*ファイルには、展開プロセスが簡略化され、生成を取得しを実行するときに実行する MSDeploy.exe コマンドについて見て*ContactManager.Mvc.deploy.cmd*上記のオプションを使用します。


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


使用しての詳細については、 *. deploy.cmd*ファイルを web パッケージを展開しを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)します。

## <a name="using-msdeployexe"></a>MSDeploy.exe を使用します。

使用すること、 *. deploy.cmd*ファイルは通常、展開プロセスを簡略化、状況によっては、ときに、MSDeploy.exe を直接使用することをお勧めします。 例えば:

- 管理者以外のユーザーとして Web 配置ハンドラーを展開する場合は、使用できません、 *. deploy.cmd*ファイル。 」の説明に従って、Web Deploy 2.0 では、バグが原因はこの**エンドポイントに関する考慮事項**します。
- 手動で別の間で切り替えたい場合*SetParameters.xml*異なる場所にファイル、MSDeploy.exe を直接使用することもできます。
- いくつかの MSDeploy.exe コマンドライン引数をオーバーライドする場合は、MSDeploy.exe を直接使用することができます。

MSDeploy.exe を使用する場合は、次の 3 つの重要な情報を提供する必要があります。

- A **– ソース**からデータが送信される場所を示すパラメーターです。
- A **– dest**にデータを移動する場所を示すパラメーターです。
- A **– 動詞**を示すパラメーターです、[操作](https://technet.microsoft.com/library/dd568989(WS.10).aspx)を実行します。

MSDeploy.exe が依存[Web Deploy プロバイダー](https://technet.microsoft.com/library/dd569040(WS.10).aspx)元とコピー先のデータを処理します。 Web Deploy には使用するアプリケーションとデータ ソースの範囲を表す、プロバイダーの多くが含まれています&#x2014;などの SQL Server データベース、IIS web サーバー、証明書、グローバル アセンブリ キャッシュ (GAC) アセンブリでは、プロバイダーにはさまざまな別の構成ファイル、および多数のデータの他の型。 両方の **– ソース**パラメーターおよび **– dest**パラメーターは、フォームで、プロバイダーを指定する必要があります **– ソース**: [*providerName*] = [*場所*]。 IIS の web サイトに web パッケージをデプロイするときは、これらの値を使用する必要があります。

- **– ソース**プロバイダーは常に[パッケージ](https://technet.microsoft.com/library/dd569019(WS.10).aspx)します。 例えば:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **– Dest**プロバイダーは常に[自動](https://technet.microsoft.com/library/dd569016(WS.10).aspx)します。例えば:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **– 動詞**は常に**同期**します。

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

さらに、必要がありますを指定するさまざまな他の[プロバイダーに固有の設定](https://technet.microsoft.com/library/dd569001(WS.10).aspx)や一般的な[操作設定](https://technet.microsoft.com/library/dd569089(WS.10).aspx)します。 たとえば、ContactManager.Mvc web アプリケーションをステージング環境をデプロイするとします。 展開では、Web 配置ハンドラーをターゲットし、基本認証を使用する必要があります。 Web アプリケーションを配置するには、次の手順を完了する必要があります。

**MSDeploy.exe を使用して web アプリケーションをデプロイするには**

1. ビルドおよび」の説明に従って、web アプリケーション プロジェクトをパッケージ化[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)します。
2. 変更、 *ContactManager.Mvc.SetParameters.xml* 」の説明に従って、ステージング環境に適切なパラメーター値を格納するファイル[Web パッケージ展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)します。
3. コマンド プロンプト ウィンドウを開き、MSDeploy.exe の場所を指定します。 これは通常 %PROGRAMFILES%\IIS\Microsoft Web デプロイ V2\msdeploy.exe にします。
4. このコマンドを入力し、Enter キーを押します (改行は無視)。

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

この例では、次のように記述されています。

- **– ソース**パラメーターを指定します、**パッケージ**プロバイダーと web のパッケージの場所を示します。
- **– Dest**パラメーターを指定します、**自動**プロバイダー。 **ComputerName**設定が移行先サーバーで Web 配置ハンドラーのサービスの URL を提供します。 **Authtype**設定は、基本認証を使用するよう指定する必要があります、ことを示します、 **username**と**パスワード**します。 最後に、 **includeAcls ="False"** 設定では、移行先サーバーに、ソース web アプリケーションで、ファイルのアクセス制御リスト (Acl) をコピーしないことを示します。
- **– 動詞: 同期**引数は、移行先サーバー上のソース コンテンツをレプリケートすることを示します。
- **–-Disablelink:contentextension**引数は、アプリケーション プール、仮想ディレクトリの構成、または移行先サーバーで Secure Sockets Layer (SSL) 証明書にレプリケートしないことを示します。 詳細については、次を参照してください。 [Web デプロイ リンク拡張機能](https://technet.microsoft.com/library/dd569028(WS.10).aspx)します。
- **– SetParamFile**パラメーターの場所を提供する、 *SetParameters.xml*ファイル。
- **– AllowUntrusted**スイッチでは、Web Deploy で信頼された証明機関によって発行されたしない SSL 証明書を受け入れる必要があることを示します。 Web 配置ハンドラーにデプロイするサービスの URL をセキュリティで保護する自己署名証明書を使用した場合は、このスイッチに含める必要があります。

## <a name="automating-web-package-deployment"></a>Web パッケージ展開を自動化します。

多数のエンタープライズ シナリオでは、シングル ステップの拡大または自動展開の一部として、web パッケージを配置するします。 Web パッケージを実行してデプロイを選択するかどうかに関係なく、 *. deploy.cmd*ファイルまたは MSDeploy.exe を直接使用すると、お客様のコマンドをパラメーター化し Microsoft Build Engine (MSBuild) 内のターゲットからこれらを呼び出すプロジェクト ファイルです。

連絡先のマネージャーのサンプル ソリューションで見て、 **PublishWebPackages**ターゲット、 *Publish.proj*ファイル。 このターゲットは、それぞれに 1 回実行 *. deploy.cmd*という名前の項目一覧で識別されるファイル**PublishPackages**します。 ターゲットの各引数の値の完全なセットを構築するプロパティと項目のメタデータを使用して *. deploy.cmd*ファイルおよび、使用、 **Exec**コマンドを実行するタスク。


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> サンプル ソリューションでは、および一般的なカスタムのプロジェクト ファイルの概要で、プロジェクト ファイルのモデルのより広範な概要については、次を参照してください。[プロジェクト ファイルを理解する](understanding-the-project-file.md)と[ビルド プロセスを理解する](understanding-the-build-process.md)します。


## <a name="endpoint-considerations"></a>エンドポイントに関する考慮事項

実行して、web パッケージを展開するかどうかに関係なく、 *. deploy.cmd*ファイルまたは MSDeploy.exe を直接使用すると、コンピューター名またはデプロイのサービス エンドポイントを指定する必要があります。

Web デプロイのリモート エージェント サービスを使用した展開先の web サーバーを構成する場合は、変換先として、ターゲット サービスの URL を指定します。


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


または、サーバー名だけを指定するには、変換先としてし、Web Deploy は、リモート エージェント サービスの URL を推論します。


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Web 配置ハンドラーを使用した展開先の web サーバーを構成する場合は、変換先として、IIS Web 管理サービス (WMSvc) のエンドポイント アドレスを指定する必要があります。 既定では、形式をとります。


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


いずれかを使用してこれらのエンドポイントのいずれかの対象にすることができます、 *. deploy.cmd*ファイルまたは直接 MSDeploy.exe します。 ただし、」の説明に従って管理者以外のユーザーとして Web 配置ハンドラーを配置する場合[Web 配置発行 (Web 配置ハンドラー) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)サービスのエンドポイント アドレスにクエリ文字列を追加する必要があります。


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


これは、管理者以外のユーザーが iis サーバー レベルのアクセスを持っていないためです。そのユーザーは、特定の IIS web サイトへのアクセスのみが。 Web 発行パイプライン (WPP) でのバグが原因の書き込み時に実行することはできません、 *. deploy.cmd*ファイルのクエリ文字列を含むエンドポイント アドレスを使用します。 このシナリオで直接 MSDeploy.exe を使用して、web パッケージを展開する必要があります。

> [!NOTE]
> Web デプロイのリモート エージェントのサービスと Web 配置ハンドラーの詳細については、次を参照してください。 [Web 配置を右側のアプローチを選択](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)します。 これらのエンドポイントにデプロイする環境に固有のプロジェクト ファイルを構成する方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)します。


## <a name="authentication-considerations"></a>認証に関する注意点

実行して、web パッケージを展開するかどうかに関係なく、 *. deploy.cmd*ファイルまたは MSDeploy.exe を直接使用すると、認証の種類を指定する必要があります。 Web Deploy は、2 つの値を受け入れます。 **NTLM**または**基本的な**します。 基本認証を指定する場合は、ユーザー名とパスワードを指定する必要があります。 認証の種類を選択するときに注意する必要があるさまざまな要素があります。

- Web デプロイのリモート エージェントのサービスにデプロイする場合は、NTLM 認証を使用する必要があります。 リモート エージェント サービスでは、基本認証資格情報を受け入れません。
- Web 配置ハンドラーに配置する場合は、NTLM または基本認証のいずれかを使用することができます。 既定の設定は、基本認証です。 基本認証は、ユーザー名とパスワードがプレーン テキストで送信中に依存するが、Web 配置ハンドラーが常に SSL 暗号化を使用して、資格情報が保護されます。
- ために NTLM 認証を使用してデータベースをデプロイすることはできませんする場合は、web のパッケージには、データベースが含まれています。 web サーバーとデータベース サーバーが別のコンピューター、、 [NTLM"ダブルホップ"制限](https://go.microsoft.com/?linkid=9805120)します。 デプロイの接続文字列に SQL Server 資格情報を使用するか、Web Deploy に基本認証資格情報を指定する必要があります。 この問題がでさらに詳しく記載されている[エンタープライズ環境にメンバーシップ データベースを展開する](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md)します。

## <a name="conclusion"></a>まとめ

このトピックで説明するデプロイ方法 web パッケージを実行するか、 *. deploy.cmd*ファイルまたは MSDeploy.exe を直接使用しています。 それぞれのアプローチは、適切な可能性があり、パラメーター化し、大規模なシングル ステップまたは自動化されたビルド プロセスの一部としてデプロイ コマンドを実行する方法が説明されていることについて説明します。

## <a name="further-reading"></a>関連項目

作成し、web 配置パッケージのパラメーター化する方法のガイダンスについては、次を参照してください。[のビルドとパッケージ化 Web Application Projects](building-and-packaging-web-application-projects.md)と[Web パッケージ展開の構成パラメーター](configuring-parameters-for-web-package-deployment.md)します。 ビルドし、Team Foundation Server (TFS) のインスタンスから web パッケージを配置する方法のガイダンスについては、次を参照してください。 [Web デプロイの自動化用の Team Foundation Server の構成](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)します。 カスタマイズし、展開プロセスをトラブルシューティングする方法については、次を参照してください。[展開から除外ファイルとフォルダー](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md)します。

> [!div class="step-by-step"]
> [前へ](configuring-parameters-for-web-package-deployment.md)
> [次へ](deploying-database-projects.md)
