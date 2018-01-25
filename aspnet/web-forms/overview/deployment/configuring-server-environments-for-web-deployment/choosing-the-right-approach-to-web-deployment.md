---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: "Web 配置の適切なアプローチの選択 |Microsoft ドキュメント"
author: jrjlee
description: "インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用する場合が 3 つのメイン アプローチを使用することができますを取得しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b77aa37160f3822f58908866e44497aea3d3bdc8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Web 配置の適切なアプローチを選択します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) 2.0 以降を使用する場合は、3 つの主要なアプローチを web サーバー上にパッケージ化された web アプリケーションの取得を行うこともできます。 方法があります。
> 
> - 対象にするリモートの場所からアプリケーションを展開、 *Web Deployment Agent サービス*(とも呼ばれる、「リモート エージェント」)、移行先サーバーにします。
> - Web 展開要求時に (とも呼ばれる、一時エージェント」) を使用してリモートの場所からアプリケーションを展開します。
> - 対象にするリモートの場所からアプリケーションを展開、 *IIS Web 配置ハンドラー*移行先サーバーにします。
> - 手動で移行先サーバーに web パッケージをコピーして、IIS マネージャーからインポートすることによって、アプリケーションを展開します。
> 
> 移行先 web サーバーを構成する方法を使用する展開するための方法に左右されます。 このトピックの内容を展開するための方法が適しているかを決定できます。


次の表は、主な利点とそれぞれの方法に合わせて最も多くのシナリオと、各展開方法の短所を示します。

| 方法 | 長所 | 短所 | 一般的なシナリオ |
| --- | --- | --- | --- |
| リモート エージェント | セットアップに簡単です。 Web アプリケーションとコンテンツへの通常の更新プログラムに適しています。 | ユーザーは、ターゲット サーバーの管理者にする必要があります。 ユーザーは、代替資格情報を提供できません。 | 開発環境です。 環境をテストします。 |
| 一時エージェント | ターゲット コンピューターに Web 配置をインストールする必要はありません。 Web Deploy の最新バージョンは自動的に使用します。 | ユーザーは、ターゲット サーバーの管理者にする必要があります。 ユーザーは、代替資格情報を提供できません。 | 開発環境です。 環境をテストします。 |
| Web Deploy ハンドラー | 管理者以外のユーザーには、コンテンツを展開できます。 Web アプリケーションとコンテンツへの通常の更新プログラムに適しています。 | これを設定するよりもずっと複雑です。 | ステージング環境。 イントラネットの実稼働環境。 ホストされている環境です。 |
| オフラインの展開 | これは非常に簡単にセットアップできます。 分離環境に適しています。 | サーバーの管理者は、手動でコピーしてたびに、web のパッケージをインポートする必要があります。 | インターネットに接続された運用環境。 ネットワーク分離環境。 |
  

## <a name="using-the-remote-agent"></a>リモート エージェントを使用します。

Web 配置の移行先サーバーで、既定の設定を使用してをインストールするときに Web Deployment Agent サービス (「リモート エージェント」) が自動的にインストールされ、開始します。 既定では、リモート エージェントは、このアドレスに HTTP エンドポイントを公開します。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> 置き換えることができます [*サーバー*]、web サーバーのコンピューターの名前を持つ、web サーバーまたはホスト名の IP アドレスに解決される、web サーバー。


サーバー管理者は、このエンドポイント アドレスを指定することによって、開発者のコンピューターなど、ビルド サーバー、リモートの場所から web パッケージを配置できます。 たとえば、Fabrikam, inc. Matt 明は開発者のコンピューター上、ContactManager.Mvc web アプリケーション プロジェクトをビルドします。 ビルド プロセスと共に web パッケージを生成する、 *. deploy.cmd*パッケージをインストールするために必要な Web Deploy コマンドを含むファイルです。 Matt が TESTWEB1 サーバーでのサーバー管理者である場合は開発者のコンピューターでこのコマンドを実行してテストの web サーバーに web アプリケーションを展開できます。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


実際には、Web 配置の実行可能ファイルがリモート エージェントのエンドポイント アドレスを推論できる場合はコンピューター名を指定するために入力 Matt がのみ必要があります。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Web Deploy のコマンドライン構文についての詳細と*. deploy.cmd*ファイルを参照してください[する方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)です。


リモート エージェントがリモートの場所からコンテンツを展開する簡単な方法を提供し、この方法でも 1 回のクリックまたは自動の展開でうまく機能します。 ただし、展開コマンドを実行するユーザーにも必要があります、ドメイン管理者または移行先サーバーでローカルの administrators グループのメンバーのいずれか。 さらに、コマンドラインで代替資格情報を渡すことはできませんので、リモート エージェントは、基本認証をサポートしていません。

リモート エージェント提供方法としては有効では、開発またはテストのシナリオでは珍しくありません開発者が完全な管理者は、テスト サーバー環境を制御するため、アプリケーションは通常再構築し、非常に再展開の展開頻繁にします。 ただし、この手法は、通常、ステージング環境または実稼働環境に許容される小さいです。

リモート エージェントのアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web デプロイのテスト環境を構成する](scenario-configuring-a-test-environment-for-web-deployment.md)です。

## <a name="using-the-temp-agent"></a>一時のエージェントを使用します。

展開するための一時エージェント アプローチは、リモート エージェントのアプローチに似ています。 ただし、リモート エージェントのアプローチとは異なり、ターゲット web サーバーに Web 配置をインストールする必要ありません。 代わりに、展開を実行するときに Web デプロイ、移行先サーバーに web deployment agent サービスの一時的なバージョンをインストールおよびこれを使用して、コンテンツを IIS に展開します。 展開が完了したら、すべての一時ファイルは削除されます。

一時エージェント プロバイダー設定を使用して、追加する場合、 **/g**には、展開コマンド フラグ。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> サービスが実行されていない場合でも、web deployment agent サービスが移行先コンピューターにインストールされている場合に、temp のエージェントを使用することはできません。


このアプローチの利点は、移行先サーバーへの Web Deploy のインストールを管理する必要はありません。 さらに、元と移行先のコンピューターが同じバージョンの Web 配置を実行していることを確認する必要はありません。 ただし、このアプローチが低下、プリンシパルと同じ制限リモート エージェントのアプローチからつまり、コンテンツを展開するために、移行先サーバー上のローカル管理者をする必要があり、NTLM 認証のみがサポートされていること。 一時エージェント アプローチには、展開先の環境のもっと初期構成が必要です。

一時のエージェントを使用する方法については、次を参照してください。[する方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)と[Web 展開オンデマンド](https://technet.microsoft.com/library/ee517345(WS.10).aspx)です。

## <a name="using-the-web-deploy-handler"></a>Web を使用してハンドラーを展開

IIS 7 以降の場合は、Web Deploy は、IIS Web 配置ハンドラーから代替の展開方法を提供します。 Web 展開ハンドラーには、ユーザーをリモートの場所からの IIS web サイトの管理を許可するように設計された IIS Web 管理サービス (WMSvc) は緊密に統合されています。

既定では、リモート エージェントは、このアドレスに HTTP エンドポイントを公開します。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> 置き換えることができます [*サーバー*]、web サーバーのコンピューターの名前を持つ、web サーバーまたはホスト名の IP アドレスに解決される、web サーバー。


リモート エージェントをおよび一時エージェントを経由で Web 展開ハンドラーの大きな利点は、特定の IIS web サイトにアプリケーションとコンテンツを展開する管理者以外のユーザーを許可するように IIS を構成することができます。 Web 展開ハンドラーでは、Web Deploy コマンドでパラメーターとして別の資格情報を提供できるように、基本認証もサポートします。 主な欠点は、Web 配置ハンドラーが最初に、セットアップし、構成をはるかに複雑です。

管理者以外のユーザーの場合 Web 管理サービス (WMSvc) が許可のみを IIS に接続するサーバー レベルの接続ではなくサイト レベルの接続を使用します。 特定のサイトにアクセスするには、エンドポイントのアドレスにサイト固有のクエリ文字列を含めることができます。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


たとえば、ビルド プロセスが自動的にデプロイする web アプリケーションをステージング環境を正常にビルドするたびに構成されているとします。 リモート エージェントのアプローチを使用した場合は、ビルド プロセスの id を移行先サーバーを管理者にする必要があります。 これに対し、Web 配置のハンドラーの方法を使用できるようにする管理者以外のユーザー & #x 2014 です。**FABRIKAM\stagingdeployer**特定 IIS の web サイトのみ、およびビルド プロセスにアクセス許可でこのケース & #x 2014; web パッケージを展開するこれらの資格情報を提供できます。


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Web Deploy のコマンドライン操作と構文の詳細については、次を参照してください。 [Web 展開コマンド ライン リファレンス](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)です。 使用の詳細について、 *. deploy.cmd*ファイルを参照してください[する方法: インストールの展開パッケージを使用して、deploy.cmd ファイル](https://msdn.microsoft.com/library/ff356104.aspx)です。


Web 展開ハンドラーは、環境、ホストされる環境、およびイントラネット ベースの運用環境で、サーバーへのリモート アクセスは使用できますが、管理者の資格情報がステージング環境の配置に有効な解決方法を提供します。

Web 展開ハンドラー アプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web 配置用のステージング環境構成](scenario-configuring-a-staging-environment-for-web-deployment.md)です。

## <a name="using-offline-deployment"></a>オフラインの展開を使用してください。

場合によっては、それが不可能または IIS の web サイトにリモートの場所からアプリケーションおよびコンテンツを展開することは実用的です。 たとえば、元と移行先コンピューターは、分離されたネットワークまたはネットワーク セグメント内にある場合がありますか、ファイアウォール ポリシーは、リモート アクセスを許可しない場合があります。

上記のようなシナリオで使用することできますも、パッケージと Web 配置の機能を公開だけ使うことはできませんにリモートの場所からします。 代わりに、移行先サーバーで管理者は web パッケージをサーバーにコピーし、IIS マネージャーからインポートする必要があります。

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

オフラインの展開方法は、境界ネットワーク内のサーバー可能性がありますが制限されている内部ネットワーク内のコンピューターとの接続、インターネットに接続された運用環境で通常役立ちます。

オフライン展開のアプローチを使用するシナリオのエンド ツー エンド例は、次を参照してください。[シナリオ: Web 配置の実稼働環境を構成する](scenario-configuring-a-production-environment-for-web-deployment.md)です。

## <a name="further-reading"></a>関連項目

Web Deploy のコマンドライン操作と構文の詳細については、次を参照してください。 [Web 展開コマンド ライン リファレンス](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)です。 使用の詳細について、 *. deploy.cmd*ファイルを参照してください[する方法: インストールの展開パッケージを使用して、deploy.cmd ファイル](https://msdn.microsoft.com/library/ff356104.aspx)です。

リモート コンピューターから web パッケージを展開するさまざまな方法に関する一般的なガイダンスについては、次を参照してください。[を使用して Web 展開でのリモート](https://technet.microsoft.com/library/ee461175(WS.10).aspx)です。 Web 展開要求時に使用する方法については、次を参照してください。 [Web 展開オンデマンド](https://technet.microsoft.com/library/ee517345(WS.10).aspx)です。

>[!div class="step-by-step"]
[前へ](configuring-server-environments-for-web-deployment.md)
[次へ](scenario-configuring-a-test-environment-for-web-deployment.md)
