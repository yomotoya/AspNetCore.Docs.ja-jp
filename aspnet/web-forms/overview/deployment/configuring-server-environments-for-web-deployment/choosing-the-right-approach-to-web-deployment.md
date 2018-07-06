---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Web 配置の適切なアプローチの選択 |Microsoft Docs
author: jrjlee
description: インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用する場合は、3 つのアプローチの取得に使用できます.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: eb1b7d50e5d7461d760ad7a963cc70369b7a4513
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807052"
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Web 配置の適切なアプローチを選択します。
====================
によって[Jason Lee](https://github.com/jrjlee)

[PDF のダウンロード](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用する場合は、3 つのアプローチを web サーバーにパッケージ化された web アプリケーションの取得に使用できます。 方法があります。
> 
> - ターゲットにするリモートの場所からアプリケーションをデプロイ、 *Web Deployment Agent サービス*(別名、「リモート エージェント」)、移行先サーバーにします。
> - オンデマンドで Web デプロイ (別名、「一時エージェント」) を使用してリモートの場所からアプリケーションをデプロイします。
> - ターゲットにするリモートの場所からアプリケーションをデプロイ、 *IIS Web 配置ハンドラー*移行先サーバーでします。
> - 手動で移行先サーバーに web パッケージをコピーして、IIS マネージャーからインポートすることによって、アプリケーションをデプロイします。
> 
> 変換先の web サーバーを構成する方法は、使用する展開する方法に左右されます。 このトピックでは、自分に適した展開する方法を判断するのに役立ちます。


次の表は、主な利点とそれぞれのアプローチを最も一般的に合わせてシナリオと共に各展開方法の短所を示します。

| 方法 | 長所 | 短所 | 一般的なシナリオ |
| --- | --- | --- | --- |
| リモート エージェント | 設定するには簡単です。 Web アプリケーションとコンテンツへの通常の更新プログラムに適しています。 | ユーザーは、ターゲット サーバーの管理者である必要があります。 ユーザーは、代替資格情報を提供できません。 | 開発環境。 環境をテストします。 |
| 一時エージェント | ターゲット コンピューターで Web 配置をインストールする必要はありません。 最新バージョンの Web 配置は自動的に使用します。 | ユーザーは、ターゲット サーバーの管理者である必要があります。 ユーザーは、代替資格情報を提供できません。 | 開発環境。 環境をテストします。 |
| Web 配置ハンドラー | 管理者以外のユーザーには、コンテンツをデプロイできます。 Web アプリケーションとコンテンツへの通常の更新プログラムに適しています。 | 設定するずっと複雑です。 | ステージング環境です。 運用環境のイントラネットです。 ホスト環境です。 |
| オフライン展開 | 設定する非常に簡単になります。 分離環境に適しています。 | サーバーの管理者は、手動でコピーして毎回 web パッケージをインポートする必要があります。 | 運用環境をインターネットに接続します。 ネットワーク分離環境。 |
  

## <a name="using-the-remote-agent"></a>リモート エージェントを使用します。

Web 配置の移行先サーバーで、既定の設定を使用してをインストールするときに Web Deployment Agent サービス (「リモート エージェント」) は自動的にインストールおよび開始します。 既定では、リモート エージェントは、このアドレスで HTTP エンドポイントを公開します。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> 置き換えることができます [*server*] で、web サーバーのコンピューター名、web サーバー、またはホスト名の IP アドレスに解決される、web サーバー。


サーバー管理者は、このエンドポイント アドレスを指定することで、開発者のコンピューターや、ビルド サーバーなどのリモートの場所から web パッケージを展開できます。 たとえば、Fabrikam, inc. の Matt 明では、自分の開発マシン上に構築された ContactManager.Mvc web アプリケーション プロジェクトがあるとします。 ビルド プロセスと組み合わせて、web のパッケージが生成されます、 *. deploy.cmd*パッケージをインストールするために必要な Web Deploy コマンドを含むファイル。 Matt が TESTWEB1 サーバーでのサーバー管理者の場合は、彼の開発者のコンピューターでこのコマンドを実行して、テスト web サーバーに web アプリケーションを展開彼できます。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


実際には、Web デプロイの実行可能ファイルがリモート エージェントのエンドポイント アドレスを推論できる Matt がのみこれを入力する必要があるため、コンピューター名を指定する場合。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Web デプロイ コマンドライン構文の詳細については、 *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)。


リモート エージェントは、リモートの場所からコンテンツを展開する簡単な方法を提供し、この方法でも 1 回のクリックや自動化されたデプロイでうまく機能します。 ただし、デプロイ コマンドを実行しているユーザーがドメイン管理者または移行先サーバーでローカルの administrators グループのメンバーのいずれかあるも必要があります。 さらに、リモート エージェントは、コマンドラインで代替資格情報を渡すことはできませんので、基本認証をサポートしていません。

リモート エージェントは、開発またはテスト シナリオで、開発者、テスト サーバー環境を完全な管理者に制御することは珍しくない、アプリケーションの再構築し非常に再デプロイは通常の展開に役立つアプローチを提供します。頻繁にします。 ただし、この手法は、通常、ステージング環境または運用環境の小さい問題です。

リモート エージェントのアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web 展開のテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)します。

## <a name="using-the-temp-agent"></a>一時エージェントを使用します。

展開するための一時エージェント アプローチは、リモート エージェントのアプローチに似ています。 ただし、リモート エージェントのアプローチとは異なり、移行先の web サーバーで Web 配置をインストールする必要はありません。 代わりに、展開を実行すると、Web、移行先サーバーで web デプロイ エージェントのサービスの一時的なバージョンをインストールし、展開これを使用して、コンテンツを IIS に展開します。 デプロイが完了したら、すべての一時ファイルは削除されます。

一時エージェント プロバイダー設定を使用してを追加する場合、 **/g**展開コマンドにフラグ。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> サービスが実行されていない場合でも、対象のコンピューターで、web deployment agent サービスがインストールされている場合に、temp のエージェントを使用することはできません。


このアプローチの利点は、移行先サーバーへの Web 配置のインストールを維持する必要があることです。 さらに、元と変換先のコンピューターが同じバージョンの Web 配置を実行していることを確認する必要はありません。 ただし、このアプローチに苦しんでリモート エージェントのアプローチと同じプリンシパル制限 namely NTLM 認証のみがサポートされている、移行先サーバー上のローカル管理者は、コンテンツを展開するためにある必要があります。 一時エージェント アプローチには、移行先の環境の多くの初期構成も必要です。

一時エージェントの使用に関する詳細については、次を参照してください。[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)と[オンデマンドで Web デプロイ](https://technet.microsoft.com/library/ee517345(WS.10).aspx)します。

## <a name="using-the-web-deploy-handler"></a>ハンドラー、Web を使用してデプロイ

IIS 7 以降では、Web Deploy は、IIS Web 配置ハンドラーを別の展開方法を提供します。 Web 配置ハンドラーは、遠隔地から IIS の web サイトを管理するために設計された IIS Web 管理サービス (WMSvc) と密接に統合します。

既定では、リモート エージェントは、このアドレスで HTTP エンドポイントを公開します。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> 置き換えることができます [*server*] で、web サーバーのコンピューター名、web サーバー、またはホスト名の IP アドレスに解決される、web サーバー。


リモート エージェント、および一時のエージェント経由で Web 配置ハンドラーの大きな利点は、特定の IIS web サイトにアプリケーションとコンテンツを展開する管理者以外のユーザーを許可するように IIS を構成することができます。 Web 配置ハンドラーでは、Web Deploy コマンドでパラメーターとして別の資格情報を提供できるように、基本認証もサポートします。 主な短所は、Web 配置ハンドラーが最初にセットアップして構成をはるかに複雑です。

管理者以外のユーザーの場合 Web 管理サービス (WMSvc) のみにより、ユーザーを IIS に接続するサーバー レベルの接続ではなくサイト レベルの接続を使用します。 特定のサイトにアクセスするには、エンドポイント アドレスのサイト固有のクエリ文字列を含めることができます。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


たとえば、ビルド プロセスが成功したビルドの後、ステージング環境への web アプリケーションを自動的に展開する構成されているとします。 リモート エージェントのアプローチを使用した場合は、ビルド プロセスの id を移行先サーバーで管理者にする必要があります。 これに対し、Web 配置ハンドラーのアプローチを使用することが管理者以外をユーザーに与える&#x2014;**FABRIKAM\stagingdeployer**ここで&#x2014;特定 IIS の web サイトのみ、およびビルド プロセスにアクセス許可は、これらを指定できますweb パッケージを展開する資格情報。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Web デプロイ コマンドライン操作と構文の詳細については、次を参照してください。 [Web 展開コマンド ライン リファレンス](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)します。 使用しての詳細については、 *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)します。


Web 配置ハンドラーは、ステージング環境、ホストされている環境、およびサーバーへのリモート アクセスがあるが管理者の資格情報が、イントラネット ベースの実稼働環境で展開するのに役立つアプローチを提供します。

Web 配置ハンドラーのアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web デプロイのステージング環境を構成する](scenario-configuring-a-staging-environment-for-web-deployment.md)します。

## <a name="using-offline-deployment"></a>オフライン展開を使用します。

場合によっては、それが不可能または IIS の web サイトにリモートの場所からアプリケーションとコンテンツを展開するは実用的です。 たとえば、分離されたネットワークまたはネットワークのセグメントの元と変換先のコンピューターがあります。 またはファイアウォールのポリシーは、リモート アクセスを許可しない場合があります。

上記のようなシナリオで使用することできますも、パッケージ化と Web Deploy; の機能を公開だけは使用できませんにリモートの場所から。 代わりに、移行先サーバーの管理者でする必要があります、サーバー上に web パッケージをコピーして、IIS マネージャーを使用をインポートします。

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

オフライン展開方法は、通常、場所、この境界ネットワーク内のサーバーは内部ネットワーク内のコンピューターとの接続と制限がある場合があります、インターネットに接続する実稼働環境で便利です。

オフライン展開のアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web 展開、運用環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)します。

## <a name="further-reading"></a>関連項目

Web デプロイ コマンドライン操作と構文の詳細については、次を参照してください。 [Web 展開コマンド ライン リファレンス](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)します。 使用しての詳細については、 *. deploy.cmd*ファイルを参照してください[方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)します。

リモート コンピューターから web パッケージをデプロイするさまざまな方法に関する一般的なガイダンスについては、次を参照してください。[を使用して Web デプロイ リモート](https://technet.microsoft.com/library/ee461175(WS.10).aspx)します。 オンデマンドで Web デプロイの使用に関する詳細については、次を参照してください。[オンデマンドで Web デプロイ](https://technet.microsoft.com/library/ee517345(WS.10).aspx)します。

> [!div class="step-by-step"]
> [前へ](configuring-server-environments-for-web-deployment.md)
> [次へ](scenario-configuring-a-test-environment-for-web-deployment.md)
