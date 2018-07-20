---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: チームのアクセス許可の構成のビルドの配置 |Microsoft Docs
author: jrjlee
description: このトピックでは、自動の b の一部として、コンテンツを web サーバーとデータベース サーバーをデプロイするビルド サーバーを有効にするアクセス許可を構成する方法について説明しています.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: b577d887b4a4476b6796ae9f1df538d16eededa3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820361"
---
<a name="configuring-permissions-for-team-build-deployment"></a>チームのアクセス許可の構成のビルドの配置
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、自動化されたビルド プロセスの一部として、コンテンツを web サーバーとデータベース サーバーをデプロイするビルド サーバーを有効にするアクセス許可を構成する方法について説明します。


このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

これらのチュートリアルの中核の展開方法が分割のプロジェクト ファイルの方法で説明されているに基づいて[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを格納しています。すべての変換先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に、環境固有のプロジェクト ファイルは、ビルド手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

Team Foundation Server (TFS) 2010年のビルド サービスをインストールするときに、サービスを実行する id を指定します。 既定では、これは、Network Service アカウントです。 または、ドメイン アカウントを使用して実行するビルド サービスを構成できます。

ビルド サービス id を使用して、Windows 認証とチームがビルドを使用して自動化を計画することが必要なすべての展開タスクが実行されます。 そのため、ビルド サービス id、web サーバーおよびデータベース サーバーで必要なアクセス許可を付与する必要があります。

> [!NOTE]
> ネットワーク サービス アカウントは、他のコンピューターへの認証にコンピューター アカウントを使用します。 コンピューター アカウント形式になります * [ドメイン名]\[マシン名] ***$**&#x2014;など**FABRIKAM\TFSBUILD$** します。 そのため、ビルド サービスが Network Service の id を使用して、実行する場合は、ビルド サーバーのコンピューター アカウント id に必要なアクセス許可を付与する必要があります。


## <a name="configuring-web-server-permissions"></a>Web サーバーのアクセス許可の構成

」の説明に従って[Web 配置を右側のアプローチを選択](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)、2 つの主な方法がリモート web サーバーに web パッケージを展開する場合に使用することができます。

- ターゲットにするリモートの場所からアプリケーションをデプロイ、 *Web Deployment Agent サービス*(リモート エージェントとも呼ばれます)、移行先サーバーにします。
- ターゲットにするリモートの場所からアプリケーションをデプロイ、*インターネット インフォメーション サービス*(*IIS) Web 配置ハンドラー*移行先サーバーでします。

リモート エージェントでは、ここで 2 つのキーの制限があります。

- リモート エージェントは、NTLM 認証のみをサポートします。 つまり、展開は、ビルド サービス id を使用する必要があります&#x2014;別のアカウントを偽装することはできません。
- リモート エージェントを使用するには、は、ターゲット サーバーの管理者が展開を実行するアカウントにあります。

同時に、これら 2 つの制限にすること、リモート エージェントのアプローチはチーム ビルド展開の自動化については望ましくないにします。 この方法を使用するには、と、ビルド サービスがターゲット web サーバーの管理者のアカウントを作成する必要があります。

これに対し、Web 配置ハンドラー アプローチには、さまざまな利点が提供しています。

- Web 配置ハンドラーは、IIS の Web 配置ツール (Web 配置) に別のアカウントの資格情報を渡すことができます、HTTPS 経由で基本認証をサポートします。
- Web 配置ハンドラーを使用して特定の IIS web サイトにコンテンツを展開する管理者以外のユーザーを許可する対象の web サーバーを構成することができます。

その結果、チーム ビルドから web パッケージ展開を自動化するときに、Web 配置ハンドラーを対象とすることをお勧め明確になります。 これは、推奨されるプロセスです。

1. 展開に使用する低特権のドメイン アカウントを作成します。
2. Web 配置ハンドラーを構成し、」の説明に従って、アカウントの特定の IIS web サイトにコンテンツを配置に必要なアクセス許可を付与[Web 配置発行 (Web 配置ハンドラー) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)します。
3. Web Deploy を呼び出すと、Web 配置ハンドラーの対象、基本認証を使用して、ドメイン アカウントの資格情報を提供作成、配置を実行します。

[Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションでは、認証の種類を指定する (基本または NTLM)、Web デプロイの資格情報と、環境固有のプロジェクト ファイル内のエンドポイント アドレス (リモート エージェントまたは Web 配置ハンドラー)。 これらの値は、作成して、プロジェクト ファイルを実行すると、Web Deploy コマンドを実行に使用されます。 詳細については、次を参照してください。 [Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)します。

Web デプロイのハンドラーを許可を構成する方法などの構成の詳細については、次を参照してください。 [Web 配置発行 (Web 配置ハンドラー) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)します。 リモート エージェントの構成の詳細については、次を参照してください。 [Web 配置発行 (リモート エージェント) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)します。

## <a name="configuring-database-server-permissions"></a>データベース サーバーのアクセス許可の構成

SQL Server にデータベースを展開するには、次の必要があります。

- SQL Server インスタンスに展開するアカウントのログインを作成します。
- ログインを与える**DBCreator** SQL Server インスタンスに対する権限。
- 初期のデプロイ後にログインを追加、 **db\_所有者**ターゲット データベースのロール。 以降のデプロイで、既存のデータベースを変更することはなくする新しいデータベースを作成するために必要です。

NTLM 認証または SQL Server 認証を使用して SQL Server インスタンスに対して認証することができます。

- NTLM 認証を使用する場合は、ビルド サービス アカウントに上記で説明したアクセス許可を付与する必要があります。
- SQL Server 認証を使用する場合は、SQL Server アカウントに上記で説明したアクセス許可を付与する必要があります。 また、データベースの配置に使用する接続文字列に、SQL Server ユーザー名とパスワードを含める必要があります。

データベースの配置のアクセス許可を構成する方法の詳細な手順については、次を参照してください。 [Web 配置発行のデータベース サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)します。

## <a name="conclusion"></a>まとめ

この時点で、必要なアクセス許可に、認証オプションと共に Team Build から web アプリケーションとデータベースのデプロイを自動化する場合を理解する必要があります。 IIS web サーバーおよび SQL Server データベース サーバーで必要なアクセス許可を実装するためにできる必要があります。

## <a name="further-reading"></a>関連項目

リモート展開をサポートする Windows server 環境の構成の詳細については、次を参照してください。 [Web 配置のサーバー環境の構成](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)します。

> [!div class="step-by-step"]
> [前へ](deploying-a-specific-build.md)
