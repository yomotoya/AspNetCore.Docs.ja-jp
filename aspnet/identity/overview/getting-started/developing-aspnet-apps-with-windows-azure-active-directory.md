---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Azure Active Directory と ASP.NET アプリの開発 |Microsoft ドキュメント
author: Rick-Anderson
description: Azure Active Directory 用の Microsoft ASP.NET ツールを簡単に Azure でホストされる web アプリケーションの認証を有効にします。 Azure 認証を使用するを使用することができます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 44bf29e099583bf9d49f2715d3ff4f748728ad8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Azure Active Directory と ASP.NET アプリの開発
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET ツールを Azure Active Directory では、ホストされている web アプリケーションの認証を有効にする単純な[Azure](https://www.windowsazure.com/home/features/web-sites/)です。 Azure 認証を使用して、組織、内部設置型 Active Directory から同期された会社のアカウントまたはカスタムの Azure Active Directory ドメインで作成されたユーザーから Office 365 のユーザーを認証することができます。 Windows Azure 認証を有効にすると、1 つを使用してユーザーを認証するアプリケーションを構成[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)テナントです。
> 
>  このチュートリアルは、Rick Anderson によって書き込まれました [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)


このチュートリアルがによるサインオンの構成されている ASP.NET アプリケーションを作成する方法を示します[Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD)。 現在サインインしているユーザーに関する情報を取得する Graph API を呼び出す方法と Azure にアプリケーションを展開する方法も学習します。

## <a name="prerequisites"></a>必須コンポーネント

1. [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)または[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)です。
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -更新プログラム 3 以降が必要です。
3. Azure アカウント。 [ここをクリックして](https://azure.microsoft.com/pricing/free-trial/)無料試用版に、アカウントが既にがあるない場合。

## <a name="add-a-global-administrator-to-your-active-directory"></a>Active Directory にグローバル管理者を追加します。

1. サインイン、 [Azure 管理ポータル](https://manage.windowsazure.com/)です。
2. すべての Azure アカウントが含まれて、**既定のディレクトリ**--、をクリックし、をクリックして、**ユーザー**ページの上部にあるタブ (下の画像を参照してください)。
3. ユーザーを追加する をクリックします。  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. 新しいユーザーを作成、**グローバル管理者**ロール。 をクリックして**ユーザー**クリックして上部のメニューから、**ユーザーの追加**コマンド バーのボタンをクリックします。
5. **ユーザーの追加**ダイアログ ボックスで、新しいユーザーの名前を入力し、右矢印をクリックします。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. ユーザー名を入力し、ロールを設定**グローバル管理者**です。 グローバル管理者では、パスワードの回復のための代替の電子メール アドレスが必要です。 終了したら、右矢印をクリックします。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. ダイアログ ボックスの次のページでをクリックして**作成**です。 一時パスワードは、新しいユーザーの作成し、ダイアログ ボックスに表示されます。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   パスワードの保存の初回ログオン後にパスワードを変更する必要があります。 次の図は、新しい管理者アカウントを示します。 Azure Active Directory を使用してもこのページに示されている Microsoft アカウントではなく、アプリにログインする必要があります。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>ASP.NET アプリケーションを作成します。

次の手順を使用して[Visual Studio Express 2013 for Web](https://www.microsoft.com/download/details.aspx?id=40747)、する必要があります[Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721)です。

1. Visual Studio で、**ファイル**し**新しいプロジェクト**です。 **新しいプロジェクト**ダイアログで、c#、Visual Web プロジェクトの左側のメニューからをクリックして**OK**です。 オフにすることも、**プロジェクトに Application Insights の追加**アプリケーションの機能をたくない場合。
2. **新しい ASP.NET プロジェクト**ダイアログで、 **MVC**、クリックして**認証の変更**です。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. **認証の変更**ダイアログで、**組織アカウント**です。 これらのオプションを自動的にするようにアプリケーションを Azure AD に統合できるだけでなく、アプリケーションを Azure AD に自動的に登録できます。 使用する必要はありません、**認証の変更**を登録し、アプリケーションが、これを構成 ダイアログでは、はるかに簡単です。 たとえば Visual Studio 2012 を使用している場合は、手動で Azure 管理ポータルでアプリケーションを登録し、Azure AD と統合する構成を更新します。  
   ドロップダウン メニューで選択**クラウドに 1 つの組織**と**シングル サインオン、ディレクトリ データの読み取り**です。 Azure AD ディレクトリ、たとえば (下のイメージの場合) のドメインを入力する*aricka0yahoo.onmicrosoft.com*、順にクリック**[ok]**です。 Azure ポータルの既定のディレクトリの [ドメイン] タブから、ドメイン名を取得できます (ダウンは、次のイメージを参照してください)。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   次の図は、Azure ポータルからドメイン名を示します。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > クリックして Azure AD に登録されるアプリケーション ID URI を構成することができます必要に応じて**オプションより**です。 アプリ ID URI は、アプリケーションの場合は、Azure AD に登録され、自身を識別する通信するときに Azure AD とアプリケーションで使用される一意の識別子です。 アプリ ID URI および登録されているアプリケーションの他のプロパティの詳細については、次を参照してください。[ここ](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering)です。 アプリ ID URI フィールドの下のチェック ボックスをクリックすると、同じ App ID URI を使用して Azure AD で、既存の登録を上書きすることができますもできます。
4. クリックした後**OK**、サインイン ダイアログが表示され、グローバル管理者アカウント (Microsoft アカウントではなく、サブスクリプションに関連付けられている) を使用してサインインする必要があります。 前に、新しい管理者アカウントを作成した場合は、パスワードを変更し、もう一度新しいパスワードを使用してサインインする必要があります。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. 正常に認証した後、**新しい ASP.NET プロジェクト**ダイアログ ボックスは、認証の選択で表示されます (**組織**) し、新しいアプリケーションがされるディレクトリ (の登録*aricka0yahoo.onmicrosoft.com*次の図で)。 この情報は、下 チェック ボックスをオン**クラウド内のホスト**です。 このチェック ボックスが選択されている場合、プロジェクトは Azure web アプリとしてプロビジョニングされが後で簡単な発行を有効になります。 **[OK]**をクリックします。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **Azure web サイトの構成**サイトの自動生成された名前と地域を使用して、ダイアログが表示されます。 ダイアログ ボックスで現在サインインしているアカウントにも注意してください。 このアカウントは、1 つに、Azure サブスクリプションが関連付けられていることを確認する Microsoft アカウントでは通常です。

    > [!NOTE]
    > このプロジェクトには、データベースが必要です。 既存のデータベースのいずれかを選択するか、新しいものを作成する必要があります。 プロジェクトは既に少量の認証の構成データを格納するローカル データベース ファイルを使用するため、データベースが必要です。 Azure の web サイトにアプリケーションを展開するときに、クラウドにアクセスできる 1 つを選択する必要がありますので、配置を使用してこのデータベースはありませんパッケージ化されます。 **[OK]**をクリックします。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. プロジェクトが作成され、認証オプションと web アプリケーションのオプションをプロジェクトに自動的に構成されます。 このプロセスが完了すると、ローカルでキーを押してプロジェクトを実行**^ f5 キーを押して**です。 組織アカウントを使用してサインインする必要があります。 先ほど作成したアカウントのユーザー名とパスワードを入力し、をクリックして**サインイン**です。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. 成功したサインインの後に、ページの右上隅で、ユーザー名を表示することによって認証されましたが、ASP.NET サイトが表示されます。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   エラーが発生した場合は、次のようにします。  
   値は、null または空にすることはできません。 パラメーター名: linkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   参照してください、[デバッグ](#dbg)セクションのチュートリアルの最後にします。

## <a name="basics-of-the-graph-api"></a>Graph API の基本

[Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx) CRUD およびその他のオブジェクトに対する操作を Azure AD ディレクトリで実行するために使用するプログラム インターフェイスです。 Visual Studio 2013 で新しいプロジェクトを作成するときに認証するための組織アカウント オプションを選択した場合、アプリケーションは Graph API の呼び出しを既に構成されます。 このセクションで簡単に説明する Graph API の動作です。

1. 上部にあるサインイン ユーザーの名前をクリックして、実行中のアプリケーションで、ページの右。 Home コント ローラーのアクションは、ユーザー プロファイルのページにこの操作によりします。 テーブルが以前に作成した管理者アカウントのユーザー情報が含まれていることがわかります。 この情報は、ディレクトリに格納し、ページが読み込まれるときに、この情報を取得する Graph API が呼び出されます。   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Visual Studio に戻り、展開、**コント ローラー**フォルダーとし、開く、 **HomeController.cs**ファイル。 表示されます、 **UserProfile()**トークンを取得し、Graph API を呼び出すコードを含むアクション。 このコードは、以下が重複しています。 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Graph API を呼び出すには、最初にトークンを取得する必要があります。 トークンが取得されたときに、Graph API へのすべての後続の要求の承認ヘッダーに、文字列値を追加する必要があります。 上記のコードの大部分は、トークンを取得する Azure AD への認証、Graph API への呼び出しを行うトークンを使用して、およびビューで表示できるように、応答を変換の詳細を処理します。

    最も重要な部分については、次の強調表示された行:`UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`です。 この行は、JSON 応答から逆シリアル化があり、ビューに表示されますが、ユーザーの名前を表します。

    HttpClient を使用して Graph API を呼び出すし、自分で生データを処理できますが、簡単な方法を使用する、[は NuGet 経由で利用可能なグラフのクライアント ライブラリ](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)です。 クライアント ライブラリは、生の HTTP 要求と、返されるデータの変換処理しは .NET 環境での Graph API を使用するより簡単です。 関連する Graph API のコード サンプルを参照して[GitHub です。](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Azure にアプリケーションを展開します。

次の手順では、Azure にアプリケーションを展開する方法を示します。 前の手順で接続して、新しいプロジェクトと Azure で web アプリをいくつかの手順で発行する準備ができたためです。

1. Visual Studio で、右クリックし、プロジェクト**発行**です。 **Web の発行**ダイアログが既に構成されている各設定に表示されます。 をクリックして、**次**へ移動するボタン、**設定**ページ。 を認証するように求められます可能性がありますAzure サブスクリプション アカウント (通常は Microsoft アカウント) と前に作成した組織アカウントではなくを使用して認証することを確認してください。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. チェック、**組織認証を有効にする**オプション。 **ドメイン**フィールドに、ドメイン、ディレクトリを入力します。 **アクセス レベル**ドロップダウン リストで、**シングル サインオン、ディレクトリ データの読み取り**です。 使用して、以前のデータベースが既に設定されているかがわかります、**データベース**セクションです。 **[発行]**をクリックします。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio は、web サイトの展開を開始し、新しいブラウザー ウィンドウが表示されます。 ディレクトリにもう一度認証を求められる場合があります。 次の情報を認証したらは、Azure で新たに発行された web サイトにリダイレクトされます。  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>アプリのデバッグ

: 次のエラーが発生した場合   
 値は、null または空にすることはできません。 パラメーター名: linkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

コードで置き換え、 *\shared\\_LoginPartial.cshtml*を次のファイル。

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

アプリを実行した後にログオンしたユーザーには、"Null User"が表示される場合サインアウトしてから、先ほど作成した Active Directory アカウントでもう一度サインインします。

素晴らしいチュートリアルに従うには、Rick Rainey [Deep Dive: Azure の web サイトと Azure AD を使用して組織の認証](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)です。

## <a name="more-information"></a>説明

- [Deep Dive: Azure の web サイトと Azure AD を使用して組織の認証](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Azure AD Graph API の概要](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Azure AD の認証シナリオ](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Azure AD コード サンプルの github](https://github.com/AzureADSamples)
