---
uid: web-api/overview/security/external-authentication-services
title: ASP.NET Web API の外部認証サービス (c#) |Microsoft ドキュメント
author: rmcmurray
description: ASP.NET Web API での外部認証サービスの使用について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 406a85db7055910cb7a4e15fec8ef68dff5a19dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876268"
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>ASP.NET Web API の外部認証サービス (c#)
====================
によって[Robert McMurray](https://github.com/rmcmurray)

展開のセキュリティ オプションの visual Studio 2013 と ASP.NET 4.5.1[シングル ページ アプリケーション](../../../single-page-application/index.md)(SPA) および[Web API](../../index.md)含めるいくつかの外部認証サービスと統合するサービスOauth/openid やソーシャル メディア認証サービス: Microsoft アカウント、Twitter、Facebook、および Google。

### <a name="in-this-walkthrough"></a>このチュートリアルで

- [外部認証サービスを使用します。](#USING)
- [サンプル Web アプリケーションの作成](#SAMPLE)
- [Facebook 認証を有効にします。](#FACEBOOK)
- [Google 認証を有効にします。](#GOOGLE)
- [Microsoft 認証を有効にします。](#MICROSOFT)
- [Twitter 認証を有効にします。](#TWITTER)
- [追加情報](#MOREINFO)

    - [外部認証サービスを組み合わせること](#COMBINE)
    - [IIS Express で完全修飾ドメイン名を使用するを構成します。](#FQDN)
    - [Microsoft 認証のため、アプリケーションの設定を取得する方法](#OBTAIN)
    - [省略可能: ローカルの登録を無効にします。](#DISABLE)

### <a name="prerequisites"></a>必須コンポーネント

を実行する例は、このチュートリアルでは、次が必要です。

- Visual Studio 2013
- 少なくとも 1 つで、次の外部認証サービスのアカウント:

    - Google ユーザー アカウント
    - アプリケーション識別子、ソーシャル メディアの次の認証サービスのいずれかの秘密キーで開発者アカウント:

        - Microsoft アカウント ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>外部認証サービスを使用します。

外部認証サービスを開発を減らすために web 開発者のヘルプを現在使用できる豊富な時刻の新しい web アプリケーションを作成するときにします。 Web ユーザーを通常いくつか既存のアカウントを持つ人気のある web サービスやソーシャル メディアの web サイト、したがって保存するとき、web アプリケーションを実装して、外部 web サービスやソーシャル メディアの web サイトからサービスの認証、開発時間費やされた認証の実装を作成します。 外部認証サービスを使用して保存エンドユーザーから別のアカウントを web アプリケーションを作成することもを別のユーザー名とパスワードを覚えておかなくてからします。

2 つの選択肢が開発者まで、必要があります: 独自の認証の実装を作成または外部認証サービスをアプリケーションに統合する方法について説明します。 最も基本的なレベルは、次の図は、外部認証サービスを使用するように構成されている web アプリケーションから情報を要求して、ユーザー エージェント (web ブラウザー) 単純な要求のフローを示しています。

[![](external-authentication-services/_static/image2.png "クリックして、イメージの展開")](external-authentication-services/_static/image1.png)

上の図で、ユーザー エージェント (またはこの例では web ブラウザー) は要求を web アプリケーションを外部認証サービスに web ブラウザーをリダイレクトします。 ユーザー エージェントは、外部認証サービスにその資格情報を送信し、外部認証サービスが、ユーザー エージェントを何らかの形で元の web アプリケーションにリダイレクトする場合は、ユーザー エージェントが正常に認証されると、トークンが、ユーザー エージェントは、web アプリケーションに送信されます。 Web アプリケーションは、外部認証サービスは正常に認証されたユーザー エージェントされ、web アプリケーションでは、トークンを使用しての詳細については、ユーザー エージェントを収集するのにことがありますが検証トークンを使用します。 アプリケーションが終了するとユーザー エージェントの情報を処理するには、web アプリケーションは、承認の設定に基づき、ユーザー エージェントに適切な応答が返すされます。

この 2 つ目の例では、ユーザー エージェントは、web アプリケーションと外部承認サーバーとネゴシエートし、web アプリケーションがユーザーに関する追加情報を取得する外部承認サーバーと他の通信を実行エージェント:

[![](external-authentication-services/_static/image4.png "クリックして、イメージの展開")](external-authentication-services/_static/image3.png)

Visual Studio 2013 と ASP.NET 4.5.1 が簡単に外部認証サービスとの統合の開発者により、次の認証サービスの組み込み統合。

- Facebook
- Google
- Microsoft アカウント (Windows Live ID アカウント)
- Twitter

このチュートリアルの例はサポートされている外部認証サービスの各 Visual Studio 2013 で出荷される新しい ASP.NET Web アプリケーション テンプレートを使用して構成する方法を示します。

> [!NOTE]
> 必要に応じては、外部認証サービスの設定に、FQDN を追加する必要があります。 この要件は、クライアントによって使用される FQDN と一致する、アプリケーションの設定で FQDN を必要とするいくつかの外部認証サービスのセキュリティ上の制約に基づいています。 (各外部認証サービスの手順は大幅に異なります。 これは、必要なかどうかに表示するには、各外部認証サービスとこれらの設定を構成する方法、ドキュメントを参照する必要があります)。この環境をテストするために FQDN を使用して、参照してくださいの IIS Express を構成する必要がある場合、[完全修飾ドメイン名を使用する IIS Express を構成する](#FQDN)このチュートリアルの後半の「します。


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>サンプル Web アプリケーションを作成します。

次の手順を案内します ASP.NET Web アプリケーション テンプレートを使用して、サンプル アプリケーションを作成して、このチュートリアルの後半で外部認証サービスごとにこのサンプル アプリケーションを使用します。

Visual Studio 2013 の選択を開始**新しいプロジェクト**スタート ページからです。 またはから、**ファイル**メニューの **新規**し**プロジェクト**です。

[![](external-authentication-services/_static/image6.png "クリックして、イメージの展開")](external-authentication-services/_static/image5.png)

ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、選択**インストール****テンプレート**展開**Visual c#** です。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET Web アプリケーション**です。 プロジェクトの名前を入力し、クリックして**OK**です。

[![](external-authentication-services/_static/image8.png "クリックして、イメージの展開")](external-authentication-services/_static/image7.png)

ときに、**新しい ASP.NET プロジェクト**表示されている、select、 **SPA**テンプレートとクリック**プロジェクトの作成**です。

[![](external-authentication-services/_static/image10.png "クリックして、イメージの展開")](external-authentication-services/_static/image9.png)

Visual Studio 2013 と待機では、プロジェクトを作成します。

[![](external-authentication-services/_static/image12.png "クリックして、イメージの展開")](external-authentication-services/_static/image11.png)

Visual Studio 2013 は、プロジェクトの作成が完了したら、開く、 *Startup.Auth.cs*に配置されているファイル、**アプリ\_開始**フォルダーです。

[![](external-authentication-services/_static/image14.png "クリックして、イメージの展開")](external-authentication-services/_static/image13.png)

外部認証サービスのどれも有効で最初にプロジェクトを作成するときに*Startup.Auth.cs*ファイルです次に示します新機能、コードのようになります、場所の強調表示されているセクションでは有効にします。外部認証サービスや ASP.NET アプリケーションで Microsoft アカウント、Twitter、Facebook、または Google の認証を使用するために関連する設定:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

ビルドおよび web アプリケーションをデバッグする f5 キーを押すと、外部認証サービスが定義されていないことを「がログイン画面が表示されます。

[![](external-authentication-services/_static/image16.png "クリックして、イメージの展開")](external-authentication-services/_static/image15.png)

次のセクションでは、各 Visual Studio 2013 で ASP.NET に用意されている外部認証サービスを有効にする方法を学習します。

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Facebook 認証を有効にします。

Facebook を使用して、認証では、Facebook 開発者アカウントを作成する必要があり、プロジェクトに必要なアプリケーション ID と Facebook からの秘密キーに機能するためにします。 Facebook 開発者アカウントを作成して、アプリケーション ID と秘密キーの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)です。

1 回、アプリケーションの ID およびシークレット キーを取得した、web アプリケーションの Facebook 認証を有効にする、次の手順を使用します。

1. プロジェクトが Visual Studio 2013 で開いているとき、開く、 *Startup.Auth.cs*ファイル。

    [![](external-authentication-services/_static/image18.png "クリックして、イメージの展開")](external-authentication-services/_static/image17.png)
2. コードの強調表示されたセクションを見つけます。

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. 削除、 &quot; // &quot;コードの強調表示された行のコメントを解除し、アプリケーション ID とシークレット キーを追加する文字。 これらのパラメーターを追加した後は、プロジェクトを再コンパイルすることができます。

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Web ブラウザーで web アプリケーションを開いた f5 キーを押すと、Facebook を外部認証サービスとして定義されていることが表示されます。

    [![](external-authentication-services/_static/image20.png "クリックして、イメージの展開")](external-authentication-services/_static/image19.png)
5. クリックすると、 **Facebook**ボタン、お使いのブラウザーは Facebook ログイン ページにリダイレクトされます。

    [![](external-authentication-services/_static/image22.png "クリックして、イメージの展開")](external-authentication-services/_static/image21.png)
6. Facebook 資格情報を入力してをクリックした後に**ログイン**、web ブラウザーは、web アプリケーションにリダイレクトするのプロンプトが表示されます、**ユーザー名**関連付ける、Facebook アカウント:

    [![](external-authentication-services/_static/image24.png "クリックして、イメージの展開")](external-authentication-services/_static/image23.png)
7. ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは既定値を表示**ホーム ページ**Facebook アカウントを。

    [![](external-authentication-services/_static/image26.png "クリックして、イメージの展開")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Google 認証を有効にします。

Google がはるかに最も容易で、開発者アカウントを必要としないことも、他の外部認証サービスとアプリケーション ID またはシークレット キーなどの追加情報は必要であるために、有効にする外部認証サービス必要になります。

Web アプリケーション用に Google 認証を有効にするには、次の手順を使用します。

1. プロジェクトが Visual Studio 2013 で開いているとき、開く、 *Startup.Auth.cs*ファイル。

    [![](external-authentication-services/_static/image28.png "クリックして、イメージの展開")](external-authentication-services/_static/image27.png)
2. コードの強調表示されたセクションを見つけます。

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. 削除、 &quot; // &quot;をコードの強調表示された行のコメントを解除し、プロジェクトを再コンパイルする文字。

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Web ブラウザーで web アプリケーションを開いた f5 キーを押すと、Google を外部認証サービスとして定義されていることが表示されます。

    [![](external-authentication-services/_static/image30.png "クリックして、イメージの展開")](external-authentication-services/_static/image29.png)
5. クリックすると、 **Google**ボタン、お使いのブラウザーは Google ログイン ページにリダイレクトされます。

    [![](external-authentication-services/_static/image32.png "クリックして、イメージの展開")](external-authentication-services/_static/image31.png)
6. Google の資格情報を入力し、をクリックした後**サインイン**Google には、web アプリケーションが Google アカウントにアクセスする権限を持っていることを確認するように求められます。

    [![](external-authentication-services/_static/image34.png "クリックして、イメージの展開")](external-authentication-services/_static/image33.png)
7. クリックすると、 **Accept**、web ブラウザーは、web アプリケーションにリダイレクトするのプロンプトが表示されます、**ユーザー名**を Google アカウントに関連付けるします。

    [![](external-authentication-services/_static/image36.png "クリックして、イメージの展開")](external-authentication-services/_static/image35.png)
8. ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは既定値を表示**ホーム ページ**Google アカウントの。

    [![](external-authentication-services/_static/image38.png "クリックして、イメージの展開")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Microsoft 認証を有効にします。

Microsoft 認証では、開発者アカウントを作成する必要があり、機能するために、クライアント ID とクライアント シークレットが必要ですが。 Microsoft 開発者アカウントを作成して、クライアント ID およびクライアント シークレットの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)です。

1 回のコンシューマー キーおよびコンシューマー シークレットを取得した、マイクロソフトの web アプリケーションの認証を有効にする、次の手順を使用します。

1. プロジェクトが Visual Studio 2013 で開いているとき、開く、 *Startup.Auth.cs*ファイル。

    [![](external-authentication-services/_static/image40.png "クリックして、イメージの展開")](external-authentication-services/_static/image39.png)
2. コードの強調表示されたセクションを見つけます。

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. 削除、 &quot; // &quot;コードの強調表示された行のコメントを解除し、クライアント ID とクライアント シークレットを追加する文字。 これらのパラメーターを追加した後は、プロジェクトを再コンパイルすることができます。

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Web ブラウザーで web アプリケーションを開いた f5 キーを押すと、Microsoft は、外部認証サービスとして定義されていることが表示されます。

    [![](external-authentication-services/_static/image42.png "クリックして、イメージの展開")](external-authentication-services/_static/image41.png)
5. クリックすると、 **Microsoft**ボタン、お使いのブラウザーは、Microsoft のログイン ページにリダイレクトされます。

    [![](external-authentication-services/_static/image44.png "クリックして、イメージの展開")](external-authentication-services/_static/image43.png)
6. Microsoft の資格情報を入力し、をクリックした後**サインイン**、web アプリケーションが Microsoft アカウントにアクセスする権限を持っていることを確認するように求められます。

    [![](external-authentication-services/_static/image46.png "クリックして、イメージの展開")](external-authentication-services/_static/image45.png)
7. クリックすると、**はい**、web ブラウザーは、web アプリケーションにリダイレクトするのプロンプトが表示されます、**ユーザー名**を Microsoft アカウントに関連付けるします。

    [![](external-authentication-services/_static/image48.png "クリックして、イメージの展開")](external-authentication-services/_static/image47.png)
8. ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは既定値を表示**ホーム ページ**Microsoft アカウントの。

    [![](external-authentication-services/_static/image50.png "クリックして、イメージの展開")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Twitter 認証を有効にします。

Twitter 認証では、開発者アカウントを作成する必要があるし、機能するために、コンシューマー キーおよびコンシューマー シークレットと必要なのです。 Twitter 開発者アカウントを作成して、コンシューマー キーおよびコンシューマー シークレットの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)です。

1 回のコンシューマー キーおよびコンシューマー シークレットを取得した、web アプリケーションの Twitter 認証を有効にする、次の手順を使用します。

1. プロジェクトが Visual Studio 2013 で開いているとき、開く、 *Startup.Auth.cs*ファイル。

    [![](external-authentication-services/_static/image52.png "クリックして、イメージの展開")](external-authentication-services/_static/image51.png)
2. コードの強調表示されたセクションを見つけます。

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. 削除、 &quot; // &quot;コンシューマー キーおよびコンシューマー シークレットを追加し、コードの強調表示された行のコメントを解除する文字。 これらのパラメーターを追加した後は、プロジェクトを再コンパイルすることができます。

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Web ブラウザーで web アプリケーションを開いた f5 キーを押すと、Twitter を外部認証サービスとして定義されていることが表示されます。

    [![](external-authentication-services/_static/image54.png "クリックして、イメージの展開")](external-authentication-services/_static/image53.png)
5. クリックすると、 **Twitter**ボタン、お使いのブラウザーは、Twitter ログイン ページにリダイレクトされます。

    [![](external-authentication-services/_static/image56.png "クリックして、イメージの展開")](external-authentication-services/_static/image55.png)
6. Twitter の資格情報を入力し、をクリックした後**Authorize アプリ**、web ブラウザーは、web アプリケーションにリダイレクトするを指定するプロンプトが表示されます、**ユーザー名**関連付けるします。Twitter アカウント:

    [![](external-authentication-services/_static/image58.png "クリックして、イメージの展開")](external-authentication-services/_static/image57.png)
7. ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは既定値を表示**ホーム ページ**Twitter アカウントの。

    [![](external-authentication-services/_static/image60.png "クリックして、イメージの展開")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>追加情報

OAuth および OpenID を使用するアプリケーションの作成に関する詳細については、次の Url を参照してください。

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>外部認証サービスを組み合わせること

柔軟性を高めるため、同時に複数の外部認証サービスを定義することができます - これにより、web アプリケーションのユーザーを有効になっている外部認証サービスのいずれかからアカウントを使用します。

[![](external-authentication-services/_static/image62.png "クリックして、イメージの展開")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>IIS Express で完全修飾ドメイン名を使用するを構成します。

外部認証プロバイダーによってはアプリケーションのように HTTP アドレスを使用してテストをサポートしていません`http://localhost:port/`です。 この問題を回避するには、は、HOSTS ファイルに完全修飾ドメイン名 (FQDN) に静的なマッピングを追加してテスト デバッグに FQDN を使用する Visual Studio 2013 で、プロジェクトのオプションを構成します。 これを行うには、次の手順に従います。

- 静的な FQDN、HOSTS ファイルのマッピングを追加します。

  1. Windows で、管理者特権のコマンド プロンプトを開きます。
  2. 次のコマンドを入力します。

      <kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd>
  3. 次のようなエントリを HOSTS ファイルに追加します。

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. 保存し、HOSTS ファイルを閉じます。

- FQDN を使用して Visual Studio プロジェクトを構成するには。

  1. プロジェクトでは Visual Studio 2013 で開き、をクリックして、**プロジェクト**] メニューの [プロジェクトのプロパティを選択します。 たとえば、選択する場合があります**WebApplication1 プロパティ**です。
  2. 選択、 **Web**タブです。
  3. FQDN を入力、<strong>プロジェクト Url</strong>です。 たとえば、入力<kbd> <http://www.wingtiptoys.com> </kbd> HOSTS ファイルに追加した FQDN マッピングとなったかどうか。

- IIS Express で、アプリケーションの FQDN を使用するを構成します。

    1. Windows で、管理者特権のコマンド プロンプトを開きます。
    2. IIS Express フォルダーに変更するのには、次のコマンドを入力します。

        <kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. FQDN をアプリケーションに追加するには、次のコマンドを入力します。

        <kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

  ここで**WebApplication1** 、プロジェクトの名前を指定し、 **bindingInformation**テストに使用するポート番号と FQDN が含まれています。

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Microsoft 認証のため、アプリケーションの設定を取得する方法

簡単なプロセスは、Microsoft の認証用の Windows Live にアプリケーションをリンクします。 Windows Live にアプリケーションをリンクしていない場合は、次の手順を使用できます。

1. 参照[ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) 、Microsoft アカウント名とメッセージが表示されたら、パスワードを入力し、をクリックして**サインイン**:

    [![](external-authentication-services/_static/image64.png "クリックして、イメージの展開")](external-authentication-services/_static/image63.png)
2. メッセージが表示されたら、アプリケーションの言語と名前を入力し、クリックして**同意**:

    [![](external-authentication-services/_static/image66.png "クリックして、イメージの展開")](external-authentication-services/_static/image65.png)
3. **API 設定**、アプリケーションのページで、アプリケーションとコピーのリダイレクト ドメインを入力、**クライアント ID**と**クライアント シークレット**プロジェクトにし、をクリックして**保存**:

    [![](external-authentication-services/_static/image68.png "クリックして、イメージの展開")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>省略可能: ローカルの登録を無効にします。

現在 ASP.NET ローカルの登録の機能は防止しません自動プログラム (bot) アカウントです。 メンバーを作成します。たとえば、bot 防止と検証のようなテクノロジを使用して[CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md)です。 このためには、ログイン ページにローカル ログイン フォームと登録のリンクを削除する必要があります。 これを行うには、開く、  *\_Login.cshtml*プロジェクトで、ページし、行をコメント アウト、ローカル ログイン パネルと登録のリンク。 結果のページは、次のコード サンプルのようになります。

[!code-html[Main](external-authentication-services/samples/sample10.html)]

ローカル ログイン パネルと登録のリンクを無効になっていますが後、ログイン ページは有効にした外部認証プロバイダーをのみ表示されます。

[![](external-authentication-services/_static/image70.png "クリックして、イメージの展開")](external-authentication-services/_static/image69.png)
