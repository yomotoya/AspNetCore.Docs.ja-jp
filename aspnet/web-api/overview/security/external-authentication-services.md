---
uid: web-api/overview/security/external-authentication-services
title: ASP.NET Web API を使用した外部認証サービス (c#) |Microsoft Docs
author: rmcmurray
description: ASP.NET Web API で外部の認証サービスの使用について説明します。
ms.author: aspnetcontent
ms.date: 06/26/2013
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: fe821384a195e69c102aad95e534d543de705a00
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822795"
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>ASP.NET Web API を使用した外部認証サービス (c#)
====================
によって[Robert McMurray](https://github.com/rmcmurray)

Visual Studio 2013 と ASP.NET 4.5.1 の展開のセキュリティ オプション[シングル ページ アプリケーション](../../../single-page-application/index.md)(SPA) と[Web API](../../index.md)サービスがいくつかありますが、外部認証サービスと統合するにはOauth/openid とソーシャル メディア サービスの認証: Microsoft アカウント、Twitter、Facebook、Google、します。

### <a name="in-this-walkthrough"></a>このチュートリアルでは

- [外部認証サービスを使用します。](#USING)
- [サンプル Web アプリケーションを作成します。](#SAMPLE)
- [Facebook 認証を有効にします。](#FACEBOOK)
- [Google 認証を有効にします。](#GOOGLE)
- [Microsoft 認証を有効にします。](#MICROSOFT)
- [Twitter 認証を有効にします。](#TWITTER)
- [追加情報](#MOREINFO)

    - [外部認証サービスを組み合わせること](#COMBINE)
    - [IIS Express を使用して、完全修飾ドメイン名を構成します。](#FQDN)
    - [Microsoft 認証用のアプリケーションの設定を取得する方法](#OBTAIN)
    - [省略可能: ローカルの登録を無効にします。](#DISABLE)

### <a name="prerequisites"></a>必須コンポーネント

このチュートリアルのサンプルを理解するには、次が必要です。

- Visual Studio 2013
- 次の外部認証サービスの少なくとも 1 つのアカウント:

    - Google のユーザー アカウント
    - アプリケーション識別子とソーシャル メディアの次の認証サービスの 1 つの秘密鍵の開発者アカウント:

        - Microsoft アカウント ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>外部認証サービスを使用します。

開発の削減は、web 開発者のヘルプを現在利用できる外部認証サービスの豊富な新しい web アプリケーションを作成するときの時間します。 Web ユーザーを通常いくつか既存のアカウントがある一般的な web サービスとソーシャル メディアの web サイトのため保存するとき、外部 web サービスやソーシャル メディアの web サイトから、認証サービスの web アプリケーションの実装、開発時間費やされた認証の実装を作成します。 外部認証サービスを使用して、web アプリケーションの別のアカウントを作成する必要がありませんし、別のユーザー名とパスワードを覚えておく必要がなくなりますエンドユーザーを保存します。

以前は、開発者には 2 つの選択肢も必要があるが: 独自の認証の実装を作成または外部認証サービスをアプリケーションに統合する方法について説明します。 最も基本的なレベルは、次の図は、外部認証サービスを使用するように構成されている web アプリケーションから情報を要求して、ユーザー エージェント (web ブラウザー) 単純な要求のフローを示しています。

[![](external-authentication-services/_static/image2.png "クリックして、イメージの展開")](external-authentication-services/_static/image1.png)

前の図では、ユーザー エージェント (またはこの例では、web ブラウザー) は、外部認証サービスに web ブラウザーをリダイレクトする web アプリケーションに、要求を行います。 ユーザー エージェントは、外部認証サービスにその資格情報を送信し、外部認証サービスが何らかの形で元の web アプリケーションにユーザー エージェントをリダイレクトして、ユーザー エージェントが正常に認証されると場合、トークンが、ユーザー エージェントは、web アプリケーションに送信されます。 Web アプリケーションは、外部認証サービスによってユーザー エージェントが正常に認証されたされ、web アプリケーションがトークンを使用してユーザー エージェントに関する情報を収集するのにことを確認トークンを使用します。 アプリケーションが完了するとユーザー エージェントの情報を処理するには、web アプリケーションはその承認設定に基づき、ユーザー エージェントを適切な応答が返すされます。

この 2 つ目の例では、web アプリケーションと外部承認サーバーとネゴシエートし、ユーザー エージェントと web アプリケーションは、ユーザーに関する追加情報を取得する外部承認サーバーに追加の通信を実行します。エージェント:

[![](external-authentication-services/_static/image4.png "クリックして、イメージの展開")](external-authentication-services/_static/image3.png)

Visual Studio 2013 と ASP.NET 4.5.1 やすく外部認証サービスとの統合開発者のため、次の認証サービスの組み込みの統合を提供することで。

- Facebook
- Google
- Microsoft アカウント (Windows Live ID アカウント)
- Twitter

このチュートリアルの例では、サポートされている外部認証サービスの各 Visual Studio 2013 に付属している新しい ASP.NET Web アプリケーション テンプレートを使用して構成する方法を示します。

> [!NOTE]
> 必要に応じては、外部認証サービスの設定に、FQDN を追加する必要があります。 この要件は、クライアントによって使用される FQDN と一致する、アプリケーションの設定で FQDN を必要とするいくつかの外部認証サービスに対するセキュリティ制約に基づいています。 (この手順は各外部認証サービスの大幅に異なります。 これは必要なかどうかに表示するには、各外部認証サービスとこれらの設定を構成する方法のドキュメントを参照する必要があります)。IIS Express を参照してください、この環境をテストするために FQDN を使用して構成する必要がある場合、[完全修飾ドメイン名を使用する IIS Express を構成する](#FQDN)このチュートリアルの後半の「します。


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>サンプル Web アプリケーションを作成します。

次の手順に従って、ASP.NET Web アプリケーション テンプレートを使用して、サンプル アプリケーションを作成して、このチュートリアルの後半で外部認証サービスごとにこのサンプル アプリケーションを使用します。

Visual Studio 2013 の選択の開始**新しいプロジェクト**スタート ページから。 またはから、**ファイル**メニューの **新規**し**プロジェクト**します。

[![](external-authentication-services/_static/image6.png "クリックして、イメージの展開")](external-authentication-services/_static/image5.png)

ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、選択**インストール済み****テンプレート**展開**Visual c#** します。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET Web アプリケーション**します。 プロジェクトの名前を入力し、クリックして**OK**します。

[![](external-authentication-services/_static/image8.png "クリックして、イメージの展開")](external-authentication-services/_static/image7.png)

ときに、**新しい ASP.NET プロジェクト**表示されている、select、 **SPA**テンプレートをクリックします**プロジェクトの作成**です。

[![](external-authentication-services/_static/image10.png "クリックして、イメージの展開")](external-authentication-services/_static/image9.png)

Visual Studio 2013 と待機は、プロジェクトを作成します。

[![](external-authentication-services/_static/image12.png "クリックして、イメージの展開")](external-authentication-services/_static/image11.png)

Visual Studio 2013 は、プロジェクトの作成が完了したら、開く、 *Startup.Auth.cs*に配置されているファイル、**アプリ\_開始**フォルダー。

[![](external-authentication-services/_static/image14.png "クリックして、イメージの展開")](external-authentication-services/_static/image13.png)

外部認証サービスのどれも有効で最初にプロジェクトを作成するときに*Startup.Auth.cs*ファイルはどのようなコードのようになります次の図は、先の強調表示されているセクションを使用すると有効にします。外部認証サービスと ASP.NET アプリケーションで Microsoft アカウント、Twitter、Facebook、または Google の認証を使用するには関連の設定:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

F5 キーをビルドして、web アプリケーションのデバッグを押すと、外部認証サービスが定義されていないことを「がログイン画面が表示されます。

[![](external-authentication-services/_static/image16.png "クリックして、イメージの展開")](external-authentication-services/_static/image15.png)

次のセクションでは、各 Visual Studio 2013 での ASP.NET で用意されている外部認証サービスを有効にする方法を学びます。

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Facebook 認証を有効にします。

Facebook を使用して認証では、Facebook 開発者アカウントを作成する必要があります、プロジェクトが必要し、なりますアプリケーション ID と秘密キーの Facebook から機能するためにします。 Facebook 開発者アカウントを作成して、アプリケーション ID と秘密キーの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)します。

1 回、アプリケーション ID とシークレット キーを取得した、web アプリケーションの Facebook 認証を有効にする、次の手順を使用します。

1. プロジェクトが Visual Studio 2013 で開いているときは、開く、 *Startup.Auth.cs*ファイル。

    [![](external-authentication-services/_static/image18.png "クリックして、イメージの展開")](external-authentication-services/_static/image17.png)
2. コードの強調表示されたセクションを見つけます。

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. 削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、アプリケーション ID とシークレット キーを追加する文字。 これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Facebook を外部認証サービスとして定義されていることが表示されます。

    [![](external-authentication-services/_static/image20.png "クリックして、イメージの展開")](external-authentication-services/_static/image19.png)
5. クリックすると、 **Facebook**ボタン、お使いのブラウザーは、Facebook のログイン ページにリダイレクトされます。

    [![](external-authentication-services/_static/image22.png "クリックして、イメージの展開")](external-authentication-services/_static/image21.png)
6. Facebook の資格情報を入力し、をクリックした後**ログイン**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**関連付けるしたい、Facebook のアカウント:

    [![](external-authentication-services/_static/image24.png "クリックして、イメージの展開")](external-authentication-services/_static/image23.png)
7. ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Facebook アカウント。

    [![](external-authentication-services/_static/image26.png "クリックして、イメージの展開")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Google 認証を有効にします。

Google ははるかに簡単に、開発者アカウントを必要としないことも、アプリケーションの ID やシークレット キーなど、他の外部認証サービスとしての追加情報は必要であるために、有効に外部認証サービスの必要になります。

Web アプリケーション用に Google 認証を有効にするには、次の手順を使用します。

1. プロジェクトが Visual Studio 2013 で開いているときは、開く、 *Startup.Auth.cs*ファイル。

    [![](external-authentication-services/_static/image28.png "クリックして、イメージの展開")](external-authentication-services/_static/image27.png)
2. コードの強調表示されたセクションを見つけます。

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. 削除、 &quot; // &quot;コードの強調表示された行をコメント解除し、プロジェクトを再コンパイルする文字。

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Google が外部認証サービスとして定義されていることが表示されます。

    [![](external-authentication-services/_static/image30.png "クリックして、イメージの展開")](external-authentication-services/_static/image29.png)
5. クリックすると、 **Google**ボタン、お使いのブラウザーは Google のログイン ページにリダイレクトされます。

    [![](external-authentication-services/_static/image32.png "クリックして、イメージの展開")](external-authentication-services/_static/image31.png)
6. Google の資格情報を入力し をクリックした後**サインイン**Google には、web アプリケーションに Google アカウントにアクセスするアクセス許可があることを確認するように求められます。

    [![](external-authentication-services/_static/image34.png "クリックして、イメージの展開")](external-authentication-services/_static/image33.png)
7. クリックすると**Accept**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**Google アカウントに関連付けるにします。

    [![](external-authentication-services/_static/image36.png "クリックして、イメージの展開")](external-authentication-services/_static/image35.png)
8. ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Google アカウント。

    [![](external-authentication-services/_static/image38.png "クリックして、イメージの展開")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Microsoft 認証を有効にします。

Microsoft 認証では、開発者アカウントを作成する必要があり、機能するために、クライアント ID とクライアント シークレットが必要です。 Microsoft 開発者アカウントを作成して、クライアント ID とクライアント シークレットの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)します。

1 回、コンシューマー キーとコンシューマー シークレットを取得した、次の手順を使用して web アプリケーション用に Microsoft 認証を有効にします。

1. プロジェクトが Visual Studio 2013 で開いているときは、開く、 *Startup.Auth.cs*ファイル。

    [![](external-authentication-services/_static/image40.png "クリックして、イメージの展開")](external-authentication-services/_static/image39.png)
2. コードの強調表示されたセクションを見つけます。

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. 削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、クライアント ID とクライアント シークレットを追加する文字。 これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Microsoft が、外部認証サービスとして定義されていることが表示されます。

    [![](external-authentication-services/_static/image42.png "クリックして、イメージの展開")](external-authentication-services/_static/image41.png)
5. クリックすると、 **Microsoft**ボタン、お使いのブラウザーは Microsoft のログイン ページにリダイレクトされます。

    [![](external-authentication-services/_static/image44.png "クリックして、イメージの展開")](external-authentication-services/_static/image43.png)
6. Microsoft の資格情報を入力し をクリックした後**サインイン**、web アプリケーションが Microsoft アカウントへのアクセス権限を持っていることを確認するメッセージが表示されます。

    [![](external-authentication-services/_static/image46.png "クリックして、イメージの展開")](external-authentication-services/_static/image45.png)
7. クリックすると**はい**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**を Microsoft アカウントに関連付けるにします。

    [![](external-authentication-services/_static/image48.png "クリックして、イメージの展開")](external-authentication-services/_static/image47.png)
8. ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Microsoft アカウント。

    [![](external-authentication-services/_static/image50.png "クリックして、イメージの展開")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Twitter 認証を有効にします。

Twitter の認証では、開発者アカウントを作成する必要があり、機能するために、コンシューマー キーとコンシューマー シークレットが必要です。 Twitter 開発者アカウントを作成して、コンシューマー キーとコンシューマー シークレットの取得については、次を参照してください。 [ https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)します。

1 回、コンシューマー キーとコンシューマー シークレットを取得した、web アプリケーション用に Twitter 認証を有効にする、次の手順を使用します。

1. プロジェクトが Visual Studio 2013 で開いているときは、開く、 *Startup.Auth.cs*ファイル。

    [![](external-authentication-services/_static/image52.png "クリックして、イメージの展開")](external-authentication-services/_static/image51.png)
2. コードの強調表示されたセクションを見つけます。

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. 削除、 &quot; // &quot;コードの強調表示された行のコメントを解除してから、コンシューマー キーとコンシューマー シークレットを追加する文字。 これらのパラメーターを追加した後、プロジェクトを再コンパイルすることができます。

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Web ブラウザーで、web アプリケーションを開くには f5 キーを押すと、Twitter が外部認証サービスとして定義されていることが表示されます。

    [![](external-authentication-services/_static/image54.png "クリックして、イメージの展開")](external-authentication-services/_static/image53.png)
5. クリックすると、 **Twitter**ボタン、お使いのブラウザーは、Twitter ログイン ページにリダイレクトされます。

    [![](external-authentication-services/_static/image56.png "クリックして、イメージの展開")](external-authentication-services/_static/image55.png)
6. Twitter の資格情報を入力し、をクリックした後**Authorize app**、web ブラウザーは、web アプリケーションにリダイレクトするを求めるプロンプトが、**ユーザー名**関連付けるしたいです。Twitter アカウント:

    [![](external-authentication-services/_static/image58.png "クリックして、イメージの展開")](external-authentication-services/_static/image57.png)
7. ユーザー名を入力し、クリックした後、**サインアップ**ボタン、web アプリケーションは、既定で表示されます**ホーム ページ**Twitter アカウント。

    [![](external-authentication-services/_static/image60.png "クリックして、イメージの展開")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>追加情報

OAuth および OpenID を使用するアプリケーションの作成に関する追加情報は、次の Url を参照してください。

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>外部認証サービスを組み合わせること

柔軟性を高め、同時に複数の外部認証サービスを定義する - これにより、web アプリケーションのユーザーからいずれかの外部認証を有効になっているサービス アカウントを使用します。

[![](external-authentication-services/_static/image62.png "クリックして、イメージの展開")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>IIS Express を使用して、完全修飾ドメイン名を構成します。

一部の外部認証プロバイダーは、アプリケーションのような HTTP アドレスを使用してテストをサポートしていません`http://localhost:port/`します。 この問題を回避するには、は、静的な完全修飾ドメイン名 (FQDN) のマッピング、HOSTS ファイルに追加し、テストおよびデバッグの FQDN を使用する Visual Studio 2013 で、プロジェクトのオプションを構成します。 これを行うには、次の手順を使用します。

- ホスト ファイル マッピング静的 FQDN を追加します。

  1. Windows では、管理者特権のコマンド プロンプトを開きます。
  2. 次のコマンドを入力します。

      <kbd>メモ帳 %WinDir%\system32\drivers\etc\hosts</kbd>
  3. 次のようなエントリを HOSTS ファイルに追加します。

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. 保存し、HOSTS ファイルを閉じます。

- FQDN を使用して、Visual Studio プロジェクトを構成するには。

  1. プロジェクトが Visual Studio 2013 で開いているときは、クリックして、**プロジェクト**] メニューの [し、プロジェクトのプロパティを選択します。 たとえば、選択した可能性があります**WebApplication1 プロパティ**します。
  2. 選択、 **Web**タブ。
  3. FQDN を入力、<strong>プロジェクト Url</strong>します。 たとえばは入力<kbd> <http://www.wingtiptoys.com> </kbd> HOSTS ファイルに追加した FQDN マッピングだったかどうか。

- IIS Express で、アプリケーションの FQDN を使用するかを構成します。

    1. Windows では、管理者特権のコマンド プロンプトを開きます。
    2. IIS Express フォルダーに変更するのには、次のコマンドを入力します。

        <kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. アプリケーションに FQDN を追加するには、次のコマンドを入力します。

        <kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

  場所**WebApplication1** 、プロジェクトの名前を指定し、 **bindingInformation**テストに使用するポート番号と FQDN が含まれています。

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Microsoft 認証用のアプリケーションの設定を取得する方法

Microsoft 認証用の Windows Live にアプリケーションをリンクは、単純なプロセスです。 Windows Live にアプリケーションをリンクしていない場合は、次の手順を使用できます。

1. 参照する[ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) 、Microsoft アカウント名とメッセージが表示されたら、パスワードを入力し、をクリックして**サインイン**:

    [![](external-authentication-services/_static/image64.png "クリックして、イメージの展開")](external-authentication-services/_static/image63.png)
2. メッセージが表示されたら、アプリケーションの言語と名前を入力し、クリックして**同意**:

    [![](external-authentication-services/_static/image66.png "クリックして、イメージの展開")](external-authentication-services/_static/image65.png)
3. **API 設定**、アプリケーションのページで、コピー、アプリケーション リダイレクト ドメインを入力、**クライアント ID**と**クライアント シークレット**プロジェクトのしクリックして**保存**:

    [![](external-authentication-services/_static/image68.png "クリックして、イメージの展開")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>省略可能: ローカルの登録を無効にします。

現在の ASP.NET ローカル登録機能も自動プログラム (ボット) からメンバーのアカウントを作成します。たとえば、ボット防止と検証などのテクノロジを使用して[CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md)します。 このため、ログイン ページのローカル ログイン フォームと登録リンクを削除する必要があります。 これを行うには、開く、  *\_Login.cshtml*プロジェクトで、ページし、ローカル ログイン パネルと登録リンクの行をコメントにします。 結果のページは、次のコード サンプルのようになります。

[!code-html[Main](external-authentication-services/samples/sample10.html)]

ローカル ログイン パネルと登録リンクを無効になっていると、ログイン ページは有効にした外部認証プロバイダーをのみ表示されます。

[![](external-authentication-services/_static/image70.png "クリックして、イメージの展開")](external-authentication-services/_static/image69.png)
