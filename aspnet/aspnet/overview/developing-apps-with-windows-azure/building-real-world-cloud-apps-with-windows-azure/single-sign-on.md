---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: シングル サインオン (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント
author: MikeWasson
description: Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 82f2f99154d94074b03d580a0f491053d6f53bde
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873886"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>シングル サインオン (Azure と実際のクラウド アプリのビルド)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


クラウド アプリケーションを開発する場合について検討する多くのセキュリティの問題がありますが、この系列のうちの 1 つに注目します。 シングル サインオンします。 これは、質問人が多くの場合、要求:"、主に構築していてアプリ自分の会社の従業員に対してクラウドでこれらのアプリケーションをホストしてすれば従業員が理解し、アプリを実行しているときに、内部設置型環境で使用する同じセキュリティ モデルを使用するようにでも有効にはホスト ファイアウォールの内側にありますか。" このシナリオを有効にする方法の 1 つは Azure Active Directory (Azure AD) と呼ばれます。 Azure AD では、エンタープライズ基幹業務 (LOB) アプリ使用できるように、インターネットを経由でき、ビジネス パートナーにもこれらのアプリを使用できるようにすることができます。

## <a name="introduction-to-azure-ad"></a>Azure AD の概要

[Azure AD](https://docs.microsoft.com/azure/active-directory/)提供[Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx)クラウドでします。 主な機能を以下に示します。

- 内部設置型 Active Directory と統合できます。
- アプリでのシングル サインオンが有効にします。
- などのオープン スタンダードをサポートしている[SAML](http://en.wikipedia.org/wiki/SAML_2.0)、 [Ws-fed](http://en.wikipedia.org/wiki/WS-Federation)、および[OAuth 2.0](http://oauth.net/2/)です。
- エンタープライズがサポートしている[Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx)です。

イントラネット アプリケーションにサインオンする従業員を有効にするために使用、内部設置型 Windows Server Active Directory 環境があるとします。

![](single-sign-on/_static/image1.png)

クラウドにディレクトリを作成は、どのような Azure AD を実行できます。 これは無料の機能と簡単にセットアップします。

内部設置型 Active Directory; から完全に独立してできます。すべてのユーザーに選択して、インターネットのアプリでの認証を配置することができます。

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

これを統合するには、内部設置型または AD です。

![AD と WAAD の統合](single-sign-on/_static/image3.png)

今すぐ、内部設置型を認証できるすべての従業員は、しなくてもファイアウォールを開くか、データ センター内の新しいサーバーを展開する – インターネット経由で認証もできます。 すべて既存 Active Directory 環境を把握し、現在使用して、社内アプリのシングル サインオン機能に与えるを活用する続行することができます。

AD と Azure AD の間には、この接続を確立した後に、web アプリとクラウドでは、従業員を認証するようにモバイル デバイスを有効することを受け入れるように、Office 365、SalesForce.com、または Google のアプリなどのサード パーティ製アプリを有効にすることができます、従業員の資格情報です。 Office 365 を使用している場合は、既にセットアップが完了を Azure AD と Office 365 では、Azure AD を使用して、認証および承認するためです。

![サード パーティ製アプリ](single-sign-on/_static/image4.png)

このアプローチの長所はいつでも、組織を追加または、ユーザーを削除、またはユーザーがパスワードを変更する、内部設置型環境で現在使用している同じプロセスを使用します。 すべての内部設置型の AD 変更は自動的にクラウド環境に反映します。

会社を使用してまたは良いニュースを Office 365 へ移動する場合は、Azure AD が Office 365 では、Azure AD を使用して、認証するために自動的に設定があります。 ように、簡単に、独自のアプリでは、Office 365 を使用する同じ認証を使用することができます。

## <a name="set-up-an-azure-ad-tenant"></a>Azure AD テナントを設定します。

Azure AD、Azure AD ディレクトリと呼びます[テナント](https://technet.microsoft.com/library/jj573650.aspx)、非常に簡単には、テナントを設定するとします。 紹介する方法、Azure 管理ポータルで、概念を説明するためには、当然のことながら、他のポータル関数と同様に行うことも、スクリプトまたは管理 API を使用しています。

管理ポータルでは、Active Directory タブをクリックします。

![ポータルで WAAD](single-sign-on/_static/image5.png)

Azure アカウントに自動的に 1 つの Azure AD テナントがあるしをクリックして、**追加**追加のディレクトリを作成するページの下部にあるボタンをクリックします。 テスト環境を実稼働環境での 1 つをたとえば場合があります。 新しいディレクトリの名前について慎重に検討します。 場合は、ディレクトリの名前を使用して、いずれかの混乱を招くことができると、ユーザー用にもう一度、名前を使用します。

![ディレクトリを追加します。](single-sign-on/_static/image6.png)

ポータルが、作成、削除、およびこの環境内でユーザーの管理を完全にサポートします。 など、追加するユーザーに、**ユーザー**  タブでをクリックし、**ユーザーの追加**ボタンをクリックします。

![ユーザー ボタンを追加します。](single-sign-on/_static/image7.png)

![追加のユーザー ダイアログ ボックス](single-sign-on/_static/image8.png)

存在する新しいユーザーを作成するには、このディレクトリでのみ、またはこのディレクトリ内のユーザーとして、このディレクトリまたはレジスタ内のユーザーまたは別の Azure AD ディレクトリからユーザーとして Microsoft アカウントを登録することができます。 (実際のディレクトリに既定のドメインになります ContosoTest.onmicrosoft.com です。使用できます、ドメイン contoso.com のように、独自に選択します。)

![ユーザーの種類](single-sign-on/_static/image9.png)

![追加のユーザー ダイアログ ボックス](single-sign-on/_static/image10.png)

ロールにユーザーを割り当てることができます。

![ユーザー プロファイル](single-sign-on/_static/image11.png)

一時パスワードを持つアカウントを作成します。

![一時パスワード](single-sign-on/_static/image12.png)

この方法で作成したユーザーは、このクラウド ディレクトリを使用して、web アプリにすぐにログインできます。

ただし、エンタープライズ シングル サインオンに最適なが、**ディレクトリ統合** タブ。

![ディレクトリ統合 タブ](single-sign-on/_static/image13.png)

ディレクトリ統合を有効にした場合と[ツールをダウンロード](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)組織内既に使用している既存の内部設置型 Active Directory とは、このクラウド ディレクトリを同期することができます。 すべてのディレクトリに格納されているユーザーに表示されますこのクラウド ディレクトリです。 クラウド アプリでは、すべての既存の Active Directory の資格情報を使用して、従業員の認証できるようになりました。 同期ツールと Azure AD 自体 – これはすべて無料です。

ツールは、以下のスクリーン ショットからわかるように簡単に使用されるウィザードです。 これらは、完全な手順については、単なる基本的なプロセスを示す例ではありません。 詳細 how に-は it については、リンクを参照して、[リソース](#resources)章の末尾。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image14.png)

をクリックして**次**、Azure Active Directory の資格情報を入力します。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image15.png)

をクリックして **[次へ]**、し、内部設置型を入力し、AD の資格情報。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image16.png)

をクリックして**次**を示し、クラウドで AD パスワードのハッシュを保存するかどうか。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image17.png)

クラウドに格納できるパスワード ハッシュは、一方向のハッシュです。実際のパスワードは、Azure AD では格納されません。 に対してクラウドでハッシュを保存する場合は、使用する必要があります[Active Directory フェデレーション サービス](https://technet.microsoft.com/library/hh831502.aspx)(ADFS)。 [ときに考慮するその他の要因 ADFS を使用するかどうかを選択する](https://technet.microsoft.com/library/jj573653.aspx)です。 ADFS オプションには、いくつかの追加の構成手順が必要です。

クラウド内のハッシュを保存する場合は、完了したら、ツールは、クリックすると、ディレクトリの同期を開始**次**です。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image18.png)

完了したら、数分後にします。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image19.png)

のみ、以降 Windows 2003 では、組織内の 1 つのドメイン コント ローラーでこれを実行する必要があるとします。 再起動する必要はありません。 完了したら、すべてのユーザーは、クラウドと行うことができますでのシングル サインオンから任意の web またはモバイル アプリケーション、SAML、OAuth、または Ws-fed を使用します。

これは、セキュリティで保護する方法について尋ね場合があります: は Microsoft を使用して、自分の機密性の高いビジネス データのですか。 答えがはいの作業を行います。 内部の Microsoft SharePoint サイトにアクセスする場合など、 [ https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/)にログインを要求を取得します。

![Office 365 のサインイン](single-sign-on/_static/image20.png)

Microsoft には、ad FS が有効に Microsoft ID を入力すると、ADFS ログイン ページにリダイレクトを取得するようにします。

![Ad FS のサインイン](single-sign-on/_static/image21.png)

内部 Microsoft AD アカウントに格納されている資格情報を入力するとを使用してこの社内アプリケーションにアクセスします。

![MS SharePoint サイト](single-sign-on/_static/image22.png)

既に ADFS が Azure AD が使用可能になりましたが、ログイン プロセスは、クラウドで Azure AD ディレクトリを通過する前に設定されていたために主にお AD サインオンでサーバーを使用しています。 重要なドキュメント、ソース管理、パフォーマンス管理ファイル、売上レポート、および詳細は、クラウド内に配置してを保護するため、これとまったく同じソリューションを使用しています。

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Azure AD でのシングル サインオンを使用する ASP.NET アプリケーションを作成します。

Visual Studio では、本当に楽にいくつかのスクリーン ショットからわかるように、Azure AD シングル サインオンを使用するアプリを作成します。

新しい ASP.NET アプリケーション、MVC または Web フォームのいずれかを作成するとき、既定の認証方法は ASP.NET Identity です。 クリックすることを Azure AD に変更するには**変更認証**ボタンをクリックします。

![認証の変更](single-sign-on/_static/image23.png)

組織アカウントを選択して、ドメイン名を入力およびシングル サインオンを選択し、します。

![認証のダイアログ ボックスを構成します。](single-sign-on/_static/image24.png)

また、アプリの読み取りを付与したりディレクトリ データに対するアクセス許可の読み取り/書き込みできます。 使用できることを行うと場合、 [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx)をユーザーの電話番号を調べる調べる、最終ログオンしているときなどに、オフィスにいるかどうか。

行うに必要である Visual Studio の資格情報を要求、Azure AD テナントの管理者と、新しいアプリケーションの Azure AD テナントと、プロジェクトの両方を構成します。

プロジェクトを実行してサインイン ページが表示されます、Azure AD ディレクトリ内のユーザーの資格情報でサインインすることができます。

![組織アカウントでサインイン](single-sign-on/_static/image25.png)

![現在のサインイン](single-sign-on/_static/image26.png)

アプリを Azure にデプロイするときに必要なを行うには、選択、**組織認証を有効にする**チェック ボックスをオンし、もう一度 Visual Studio はのすべての構成を処理します。

![Web を発行します。](single-sign-on/_static/image27.png)

Azure AD の認証を使用するアプリをビルドする方法を示す完全なステップ バイ ステップ チュートリアル由来以下のスクリーン ショット: [Azure Active Directory での ASP.NET アプリの開発](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)です。

## <a name="summary"></a>まとめ

この章で、Azure Active Directory、Visual Studio、および ASP.NET を簡単に、組織のユーザーのインターネット アプリケーションでのシングル サインオンを設定を確認しました。 ユーザーは、内部ネットワークの Active Directory を使用してサインオンを使用して、同じ資格情報を使用してアプリをインターネットでサインオンできます。

[次のチャプター](data-storage-options.md)クラウド アプリの使用可能なデータ ストレージのオプションを見ます。

<a id="resources"></a>
## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

- [Azure Active Directory のドキュメント](https://docs.microsoft.com/azure/active-directory/)です。 Windowsazure.com サイトのドキュメントについては Azure AD ポータルのページです。 ステップ バイ ステップ チュートリアルについては、次を参照してください。、**開発**セクションです。
- [Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)です。 Azure で多要素認証に関するドキュメントについてはポータル ページです。
- [組織アカウントの認証オプション](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions)です。 Visual Studio 2013 の [新しいプロジェクト] ダイアログ ボックスで、Azure AD の認証オプションの説明です。
- [Microsoft Patterns and Practices - Federated Identity パターン](https://msdn.microsoft.com/library/dn589790.aspx)です。
- [方法: Azure Active Directory 同期ツールのインストール](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)です。
- [Active Directory フェデレーション サービス 2.0 コンテンツ マップ](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)です。 Ad FS 2.0 に関するドキュメントへのリンク。
- [Windows Azure AD アプリケーションでのロールベースおよび ACL ベース承認](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1)です。 サンプル アプリケーション。
- [Azure Active Directory Graph API のブログ](https://blogs.msdn.com/b/aadgraphteam/)です。
- [アクセス制御では、BYOD とハイブリッド Id インフラストラクチャのディレクトリ統合](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=)です。 技術 Ed 2014 セッション ビデオ Gayana Bagdasaryan によってです。

> [!div class="step-by-step"]
> [前へ](web-development-best-practices.md)
> [次へ](data-storage-options.md)
