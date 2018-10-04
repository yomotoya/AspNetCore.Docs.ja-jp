---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: シングル サインオン (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 22ef4c2908783e513bfb6fb63364e71378cb8719
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578433"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>シングル サインオン (Azure で現実世界のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson]((https://twitter.com/RickAndMSFT))、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


クラウド アプリを開発しているときに考慮する多くのセキュリティの問題がありますが、このシリーズの 1 つだけに注目します。 シングル サインオンします。 これは、質問人が多くの場合、要求:"主にビルド アプリ自分の会社の従業員クラウドでのこれらのアプリをホストしてはどうすれば引き続き従業員がいるし、アプリを実行しているときに、オンプレミス環境で使用するのと同じセキュリティ モデルを使用するように有効にするホスト ファイアウォールの内側にでしょうか。" このシナリオを有効にする方法の 1 つには、Azure Active Directory (Azure AD) が呼び出されます。 Azure AD では、エンタープライズ基幹業務 (LOB) アプリ使用できるように、インターネットを経由でき、ビジネス パートナーもこれらのアプリを使用できるようにすることができます。

## <a name="introduction-to-azure-ad"></a>Azure AD の概要

[Azure AD](https://docs.microsoft.com/azure/active-directory/)提供[Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx)クラウドで。 主な機能を以下に示します。

- オンプレミスの Active Directory と統合します。
- アプリでのシングル サインオンが有効にします。
- などのオープン標準をサポートしています[SAML](http://en.wikipedia.org/wiki/SAML_2.0)、 [Ws-fed](http://en.wikipedia.org/wiki/WS-Federation)、および[OAuth 2.0](http://oauth.net/2/)します。
- エンタープライズがサポートしている[Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx)します。

イントラネット アプリケーションにサインオンする従業員を有効化に使用するオンプレミス Windows Server Active Directory 環境があるとします。

![](single-sign-on/_static/image1.png)

クラウドにディレクトリを作成は、どのような Azure AD を使用する操作を行います。 これは無料の機能と簡単にセットアップできます。

オンプレミスの Active Directory; から完全に独立したできます。すべてのユーザーが選択して、インターネットからアプリに認証を配置することができます。

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

オンプレミスと統合することができます AD。

![AD と WAAD の統合](single-sign-on/_static/image3.png)

これで、オンプレミスで認証できるすべての従業員は、ファイアウォールを開くか、データ センター内の新しいサーバーをデプロイする必要がありません – インターネット経由で認証もできます。 すべての既存 Active Directory 環境を把握し、現在使用して、内部アプリのシングル サインオンを機能に与えるを活用する続行することができます。

AD と Azure AD の間には、この接続を確立した後に、web アプリと、従業員、クラウドでの認証にモバイル デバイスを有効することも受け入れるように、Office 365、SalesForce.com、または Google apps などのサードパーティ製アプリを有効にすることができます、従業員の資格情報。 Office 365 を使用している場合既にセットアップが Azure AD と Office 365 では、Azure AD を使用して、認証と承認するためです。

![サード パーティ製アプリ](single-sign-on/_static/image4.png)

このアプローチの美しさはいつでも、組織を追加またはユーザーを削除またはユーザーがパスワードを変更する、オンプレミス環境で現在使用されている同じプロセスを使用します。 すべてで、オンプレミスの AD 変更を自動的に、クラウド環境を反映します。

会社を使用して良い知らせは、Office 365 に移行する場合は、Azure AD の Office 365 では、Azure AD を使用して、認証するために自動的にセットアップを必要があります。 このため、簡単に独自のアプリでは、Office 365 を使用する同じ認証を使用することができます。

## <a name="set-up-an-azure-ad-tenant"></a>Azure AD テナントをセットアップします。

Azure AD ディレクトリが、Azure AD と呼ばれる[テナント](https://technet.microsoft.com/library/jj573650.aspx)、し、テナントを設定することは非常に簡単です。 紹介する方法の概念を説明するために、Azure 管理ポータルで行われますが、もちろん他のポータルの関数と同様に行うこともできますが、スクリプトまたは管理 API を使用しています。

管理ポータルでは、Active Directory タブをクリックします。

![ポータルで WAAD](single-sign-on/_static/image5.png)

自動的に、Azure のアカウントの 1 つの Azure AD テナントがあるし、クリックすることができます、**追加**追加のディレクトリを作成するページの下部にあるボタンをクリックします。 テスト環境用と運用環境で 1 つは、たとえばする可能性があります。 新しいディレクトリの名前について慎重に検討します。 いずれかの混乱を招くことができると、ユーザー用にもう一度、名前を使用し、ディレクトリの名前を使用する場合。

![ディレクトリを追加します。](single-sign-on/_static/image6.png)

ポータルでは、作成、削除、およびこの環境内でユーザーを管理する完全にサポートがあります。 たとえば、追加ユーザーに移動、**ユーザー**  タブでをクリックし、**ユーザーの追加**ボタン。

![ユーザー ボタンを追加します。](single-sign-on/_static/image7.png)

![追加ユーザー ダイアログ ボックス](single-sign-on/_static/image8.png)

存在する新しいユーザーを作成するには、このディレクトリにのみ、またはこのディレクトリ内のユーザーとして、このディレクトリのユーザー登録または別の Azure AD ディレクトリのユーザーとして Microsoft アカウントを登録することができます。 (実際のディレクトリに既定のドメインになります ContosoTest.onmicrosoft.com します。 使用することできますも、ドメインの contoso.com のような独自に選択します。)

![ユーザーの種類](single-sign-on/_static/image9.png)

![追加ユーザー ダイアログ ボックス](single-sign-on/_static/image10.png)

ロールにユーザーを割り当てることができます。

![ユーザー プロファイル](single-sign-on/_static/image11.png)

一時パスワードを使用して、アカウントが作成されます。

![一時パスワード](single-sign-on/_static/image12.png)

この方法で作成したユーザーは、このクラウド ディレクトリを使用して、web アプリにすぐにログインできます。

エンタープライズ シングル サインオン、優れた新、**ディレクトリ統合** タブ。

![ディレクトリ統合 タブ](single-sign-on/_static/image13.png)

ディレクトリの統合を有効にした場合と[ツールをダウンロード](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)組織内で既に使用している既存のオンプレミス Active Directory とは、このクラウド ディレクトリを同期することができます。 このクラウド ディレクトリ ディレクトリに格納されているユーザーをすべて表示されます。 クラウド アプリでは、すべての既存の Active Directory の資格情報を使用して、従業員の認証できるようになりました。 同期ツールと Azure AD 自体 – これはすべて無料です。

このツールは、これらのスクリーン ショットからわかるように、簡単に使用するウィザードです。 これらは完全な手順については、基本的なプロセスを示すほんの一例ではありません。 詳細な方法を操作を行います it に関する情報は、内のリンクを参照してください、[リソース](#resources)章の最後のセクション。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image14.png)

クリックして**次**、Azure Active Directory の資格情報を入力します。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image15.png)

をクリックして **[次へ]**、して enter、オンプレミス AD の資格情報。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image16.png)

クリックして**次**、され、クラウドでの AD パスワードのハッシュを格納することを示します。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image17.png)

クラウドに格納できるパスワード ハッシュは、一方向のハッシュです。実際のパスワードは、Azure AD では格納されません。 使用する必要がありますに対して、クラウドにハッシュを格納する場合は、 [Active Directory フェデレーション サービス](https://technet.microsoft.com/library/hh831502.aspx)(ADFS)。 [ときに考慮するその他の要因 ADFS を使用するかどうかを選択する](https://technet.microsoft.com/library/jj573653.aspx)します。 ADFS オプションには、いくつかの追加の構成手順が必要です。

クラウドでハッシュを格納するか、完了したら、およびツールをクリックするとディレクトリの同期を開始する場合**次**します。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image18.png)

数分で完了します。

![WAAD 同期ツール構成ウィザード](single-sign-on/_static/image19.png)

のみ、これ以上 Windows 2003 では、組織内の 1 つのドメイン コント ローラー上で実行する必要があります。 再起動する必要はありません。 完了すると、すべてのユーザーは、クラウド、行うことができますから任意の web またはモバイル アプリケーション、SAML、OAuth、Ws-fed を使用してシングル サインオンします。

これは、セキュリティで保護する方法について尋ね場合があります: は Microsoft を使用して、機密性の高いビジネス データのでしょうか。 答えはイエスでしょう。 内部の Microsoft SharePoint サイトにアクセスする場合など、 [ https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/)にログインするように求めを取得します。

![Office 365 のサインイン](single-sign-on/_static/image20.png)

Microsoft では、ADFS が有効になって Microsoft ID を入力すると、ADFS ログイン ページにリダイレクトを取得するようにします。

![Ad FS のサインイン](single-sign-on/_static/image21.png)

内部の Microsoft の AD アカウントに格納されている資格情報を入力すると、この内部アプリケーションへのアクセスがあります。

![MS の SharePoint サイト](single-sign-on/_static/image22.png)

私たちは既に ADFS が Azure AD が、使用できるようになりましたが、ログイン プロセスは、クラウドで Azure AD ディレクトリを通過する前に設定するために主に AD のサインインにサーバーを使います。 当社の重要なドキュメント、ソース管理、パフォーマンス管理ファイル、売上レポート、および詳細は、クラウドでの配置し、それらをセキュリティで保護する正確なこれと同じソリューションを使用しています。

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Azure AD シングル サインオンを使用する ASP.NET アプリケーションを作成します。

Visual Studio では、実に容易にいくつかのスクリーン ショットからわかるように、Azure AD シングル サインオンを使用するアプリを作成します。

MVC または Web フォームでは、新しい ASP.NET アプリケーションを作成するときに既定の認証方法は、ASP.NET の Id です。 Azure AD に変更をクリックする、**認証の変更**ボタンをクリックします。

![認証の変更](single-sign-on/_static/image23.png)

組織アカウントを選択、ドメイン名を入力し、シングル サインオンの します。

![認証ダイアログ ボックスを構成します。](single-sign-on/_static/image24.png)

こともアプリの読み取りまたは読み取り/書き込みディレクトリ データに対するアクセス許可。 使用できるようにする場合、 [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx)をユーザーの電話番号を調べる調べる on などの前回のログインするときに、オフィスにいるかどうか。

実行する必要がある Visual Studio を要求する資格情報、Azure AD テナントの管理者と、プロジェクトと新しいアプリケーションに Azure AD テナントの両方を構成します。

プロジェクトを実行して、サインイン ページが表示されます、Azure AD ディレクトリでユーザーの資格情報でサインインすることができます。

![組織アカウントのサインイン](single-sign-on/_static/image25.png)

![ログに記録](single-sign-on/_static/image26.png)

アプリを Azure にデプロイするときに行う必要があるすべてが選択されて、**組織認証を有効にする**チェック ボックスをオンし、もう一度 Visual Studio のすべての構成を行います。 します。

![Web を発行します。](single-sign-on/_static/image27.png)

これらのスクリーン ショットは、Azure AD 認証を使用するアプリをビルドする方法を示す完全なステップ バイ ステップ チュートリアルから取得: [ASP.NET アプリ開発と Azure Active Directory に](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)します。

## <a name="summary"></a>まとめ

この章では、Azure Active Directory、Visual Studio および ASP.NET をしやすく、組織のユーザーのインターネット アプリケーションでシングル サインオンを設定する説明しました。 ユーザーは、Active Directory を使用して、内部ネットワークでのサインオンに使用するのと同じ資格情報を使用してアプリをインターネットでサインオンできます。

[[次へ] の章](data-storage-options.md)クラウド アプリの使用可能なデータ ストレージ オプションになります。

<a id="resources"></a>
## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

- [Azure Active Directory のドキュメント](https://docs.microsoft.com/azure/active-directory/)します。 Windowsazure.com サイトのドキュメントについては Azure AD ポータルのページです。 ステップ バイ ステップ チュートリアルについては、次を参照してください。、**開発**セクション。
- [Azure Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)します。 Azure で多要素認証に関するドキュメントについてはポータル ページです。
- [組織アカウントの認証オプション](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions)します。 Visual Studio 2013 の [新しいプロジェクト] ダイアログ ボックスで、Azure AD の認証オプションの説明です。
- [Microsoft Patterns and Practices - フェデレーション Id パターン](https://msdn.microsoft.com/library/dn589790.aspx)します。
- [方法: Azure Active Directory Sync ツールのインストール](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx)します。
- [Active Directory フェデレーション サービス 2.0 コンテンツ マップ](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx)します。 ADFS 2.0 に関するドキュメントへのリンク。
- [Windows Azure AD アプリケーションでのロールベースおよび ACL ベース承認](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1)します。 サンプル アプリケーションです。
- [Azure Active Directory Graph API ブログ](https://blogs.msdn.com/b/aadgraphteam/)します。
- [アクセス制御では、BYOD と Directory ハイブリッド Id インフラストラクチャを統合](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=)します。 Tech Ed 2014 セッション ビデオ Gayana Bagdasaryan によってです。

> [!div class="step-by-step"]
> [前へ](web-development-best-practices.md)
> [次へ](data-storage-options.md)
