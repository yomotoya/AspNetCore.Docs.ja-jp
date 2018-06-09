---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: ビルドの配置をチームのアクセス許可の構成 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、自動 b の一部として、web サーバーおよびデータベース サーバーにコンテンツを展開するようにビルド サーバーを有効にする権限を構成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 4698349d664816ec49475bbfe71fb32af79ea96d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30890282"
---
<a name="configuring-permissions-for-team-build-deployment"></a>チームのアクセス許可の構成のビルドの配置
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックでは、自動ビルド プロセスの一環として、web サーバーおよびデータベース サーバーにコンテンツを展開するようにビルド サーバーを有効にする権限を構成する方法について説明します。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によって制御されるビルド プロセスでは、2 つのプロジェクト ファイル&#x2014;1 つを含む各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用される手順をビルドします。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="task-overview"></a>タスクの概要

Team Foundation Server (TFS) 2010年のビルド サービスをインストールするときに、サービスを実行する id を指定します。 既定では、これは、Network Service アカウントです。 代わりに、ドメイン アカウントを使用して実行するビルド サービスを構成することができます。

ビルド サービス id を使用して、Windows 認証、およびチームがビルドを使用して自動化を計画することを必要とする展開タスクが実行されます。 そのため、ビルド サービス id に、web サーバーと、データベース サーバーに必要なアクセス許可を付与する必要があります。

> [!NOTE]
> ネットワーク サービス アカウントは、他のコンピューターへの認証にコンピューター アカウントを使用します。 コンピューター アカウントは、形式をとる * [ドメイン名]\[マシン名] ***$**&#x2014;など**FABRIKAM\TFSBUILD$** です。 ような場合、ビルド サービスでは、Network Service の id を使用して実行する、ビルド サーバーのコンピューター アカウント id に必要なアクセス許可を付与する必要があります。


## <a name="configuring-web-server-permissions"></a>Web サーバーのアクセス許可の構成

」の説明に従って[Web 配置を右側の方法を選択する](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md)、2 つの主要なアプローチがリモート web サーバーに web パッケージを展開する場合に使用することができます。

- 対象にするリモートの場所からアプリケーションを展開、 *Web Deployment Agent サービス*(リモート エージェントとも呼ばれます)、移行先サーバーにします。
- 対象にするリモートの場所からアプリケーションを展開、*インターネット インフォメーション サービス*(*IIS) Web 展開ハンドラー*移行先サーバーにします。

リモート エージェントでは、ここでは 2 つのキーの制限があります。

- リモート エージェントには、NTLM 認証のみがサポートしています。 つまり、展開がビルド サービス id を使用する必要があります&#x2014;別のアカウントを偽装することはできません。
- リモート エージェントを使用するのには、ターゲット サーバーの管理者が展開を実行するアカウントにあります。

同時に、これら 2 つの制限にするようにリモート エージェントのアプローチは自動化されたチーム ビルド展開の望ましくないにします。 このアプローチを使用するのには、ビルド サービスがターゲット web サーバー上の管理者のアカウントを作成する必要があります。

これに対し、Web 配置ハンドラー アプローチには、さまざまな利点が提供しています。

- Web 展開ハンドラーは、IIS Web 配置ツール (Web 配置) を別のアカウントの資格情報を渡すことができる HTTPS 経由で基本認証をサポートします。
- Web 展開ハンドラーを使用して特定の IIS web サイトにコンテンツを展開する管理者以外のユーザーを許可する対象の web サーバーを構成することができます。

その結果、お勧め明確にチーム ビルドから web パッケージの展開を自動化するときに、Web 配置のハンドラーを対象にします。 これは、推奨されるプロセスです。

1. 展開に使用する低特権のドメイン アカウントを作成します。
2. Web 展開ハンドラーを構成および」の説明に従って、アカウントに特定の IIS web サイトにコンテンツを展開に必要なアクセス許可を付与[Web 配置発行 (Web 配置ハンドラー) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)です。
3. Web Deploy を呼び出し、Web 配置のハンドラーを対象し、基本認証を使用し、ドメイン アカウントの資格情報を指定して、作成した配置を実行します。

[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューション、認証の種類を指定する (基本または NTLM)、Web Deploy の資格情報と環境固有のプロジェクト ファイル内のエンドポイント アドレス (リモート エージェントまたは Web デプロイ ハンドラー)。 これらの値は、作成し、プロジェクト ファイルを実行すると、Web Deploy コマンドを実行に使用されます。 詳細については、次を参照してください。 [Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)です。

Web 配置のハンドラーを許可を構成する方法を含む構成の詳細については「 [Web 配置発行 (Web 配置ハンドラー) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)です。 リモート エージェントを構成する方法については、次を参照してください。 [Web 配置発行 (リモート エージェント) の Web サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)です。

## <a name="configuring-database-server-permissions"></a>データベース サーバーのアクセス許可の構成

SQL Server にデータベースを展開するには、次の必要があります。

- SQL Server インスタンスに展開するアカウントのログインを作成します。
- ログインを与える**DBCreator** SQL Server インスタンスに対する権限。
- 最初の展開後にログインを追加、 **db\_所有者**ターゲット データベースのロール。 後続のデプロイの既存のデータベースの変更はなくする新しいデータベースを作成するために必要です。

NTLM 認証または SQL Server 認証を使用して SQL Server インスタンスに認証することができます。

- NTLM 認証を使用する場合は、ビルド サービス アカウントに上記のアクセス許可を付与する必要があります。
- SQL Server 認証を使用する場合は、SQL Server アカウントに上記のアクセス許可を付与する必要があります。 また、データベースの配置に使用する接続文字列に、SQL Server のユーザー名とパスワードを含める必要があります。

データベースの配置のアクセス許可を構成する方法の詳細な手順については、次を参照してください。 [Web 配置発行のデータベース サーバーを構成する](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)です。

## <a name="conclusion"></a>まとめ

この時点で、オプションと共に、認証するには、開いているときに必要なチーム ビルドからの web アプリケーションとデータベース デプロイを自動化するアクセス許可を理解する必要があります。 IIS web サーバーと SQL Server データベース サーバーに必要なアクセス許可を実装できる必要があります。

## <a name="further-reading"></a>関連項目

リモート展開をサポートするために Windows server の環境を構成する方法については、次を参照してください。 [Web 配置のサーバー環境の構成](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)です。

> [!div class="step-by-step"]
> [前へ](deploying-a-specific-build.md)
