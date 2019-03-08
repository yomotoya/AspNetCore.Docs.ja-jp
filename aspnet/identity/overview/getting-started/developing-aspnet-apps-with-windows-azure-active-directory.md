---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Azure Active Directory と ASP.NET アプリの開発 |Microsoft Docs
author: Rick-Anderson
description: Azure Active Directory 用の Microsoft ASP.NET ツールにより、簡単に Azure でホストされている web アプリケーションの認証を有効にします。 Azure 認証を使用することができます.
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 7f0e569458c9a294cc281b86e731c2fda48768be
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912879"
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Azure Active Directory と ASP.NET アプリの開発
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

Azure Active Directory でホストされる web アプリの認証を有効化を簡素化用のツールは Microsoft ASP.NET [Azure](https://www.windowsazure.com/home/features/web-sites/)します。 Azure 認証を使用して、組織、オンプレミスの Active Directory から同期された会社のアカウントまたはカスタムの Azure Active Directory ドメインで作成されたユーザーから Office 365 ユーザーを認証することができます。 Windows Azure Authentication を有効にすると、1 つを使用してユーザーを認証するアプリケーションを構成します[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)テナント。

このチュートリアルがサインオン用に構成された ASP.NET アプリケーションを作成する方法を示します[Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD)。 アプリケーションを Azure にデプロイする方法と、現在サインインしているユーザーに関する情報を取得する Graph API を呼び出す方法も学習します。

## <a name="prerequisites"></a>必須コンポーネント

1. [Visual Studio Express 2013 for Web](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express)または[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)します。
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 以降が必要です。
3. Azure アカウント。 [ここをクリックして](https://azure.microsoft.com/pricing/free-trial/)無料試用版アカウントを既に持っていない場合。

## <a name="add-a-global-administrator-to-your-active-directory"></a>Active Directory にグローバル管理者を追加します。

1. サインイン、 [Azure 管理ポータル](https://manage.windowsazure.com/)します。
2. すべての Azure アカウントを含む、**既定のディレクトリ**--[] をクリックし、をクリックして、**ユーザー**ページの上部にあるタブ (下図を参照してください)。
3. ユーザーの追加 をクリックします。
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. 新しいユーザーを作成、**グローバル管理者**ロール。 をクリックして**ユーザー**から上部のメニューをクリック、**ユーザーの追加**コマンド バーのボタンをクリックします。
5. **ユーザーの追加**ダイアログ ボックスで、新しいユーザーの名前を入力し、右矢印をクリックします。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. ユーザー名を入力し、ロールに設定**グローバル管理者**します。 グローバル管理者では、パスワードの回復のため、連絡用電子メール アドレスが必要です。 完了したら、右矢印をクリックします。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. ダイアログ ボックスの次のページで次のようにクリックします。**作成**です。 一時パスワードは、新しいユーザーの作成し、ダイアログに表示します。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)

   パスワードを保存後、最初のログイン パスワードを変更する必要があります。 次の図は、新しい管理者アカウントを示します。 Azure Active Directory を使用してもこのページに表示される Microsoft アカウントではなく、アプリにログインする必要があります。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>ASP.NET アプリケーションを作成します。

使用して、次の手順[Visual Studio Express 2013 for Web](https://www.microsoft.com/download/details.aspx?id=40747)、する必要があります[Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721)します。

1. Visual Studio で、次のようにクリックします。**ファイル**し**新しいプロジェクト**します。 **新しいプロジェクト**ダイアログで、選択、Visual C# Web を左側のメニューからプロジェクト をクリックして**OK**します。 オフにすることも、**プロジェクトに Application Insights の追加**アプリケーション機能が必要ない場合。
2. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **MVC**、 をクリックし、**認証の変更**します。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. **認証の変更**ダイアログ ボックスで、**組織アカウント**します。 これらのオプションは、自動的に Azure AD と統合するアプリケーションを構成するほか、アプリケーションを Azure AD に自動的に登録を使用できます。 使用する必要はありません、**認証の変更**登録し、これは、アプリケーションで構成するためのダイアログでは、はるかに簡単です。 たとえば、Visual Studio 2012 を使用している場合、手動で、Azure 管理ポータルにアプリケーションを登録し、Azure AD と統合するには、その構成を更新します。
   ドロップダウン メニューで、選択**クラウド - 単一の組織**と**シングル サインオン、ディレクトリ データの読み取り**します。 たとえば (下の画像) で、Azure AD ディレクトリのドメインを入力します。 *aricka0yahoo.onmicrosoft.com*、順にクリックします**OK**します。 Azure portal で、既定のディレクトリの [ドメイン] タブから、ドメイン名を取得できます (ダウンは、次のイメージを参照してください)。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)

   次の図には、Azure portal からドメイン名が表示されます。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)

    > [!NOTE]
    > クリックして Azure AD に登録されるアプリケーション ID URI を構成することもできます。**オプションより**します。 アプリ ID URI は、アプリケーションは、Azure AD に登録され、Azure AD と通信するときに識別する、アプリケーションで使用される一意の識別子です。 アプリ ID URI と登録済みのアプリケーションの他のプロパティの詳細については、次を参照してください。[このトピックの「](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering)します。 アプリ ID URI フィールドの下のチェック ボックスをクリックすると、同じアプリ ID URI を使用して Azure AD で既存の登録を上書きすることもできます。
4. クリックすると**OK**サインイン ダイアログが表示され、グローバル管理者アカウント (Microsoft アカウントではなく、サブスクリプションに関連付けられている) を使用してサインインする必要があります。 新しい管理者アカウントを先ほど作成した場合は、パスワードを変更し、もう一度新しいパスワードを使用してサインインする必要があります。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. 正常に認証した後、**新しい ASP.NET プロジェクト**ダイアログに、認証の選択肢を表示 (**組織**) し、新しいアプリケーションがされるディレクトリの登録 (*aricka0yahoo.onmicrosoft.com*下図)。 この情報は、以下の選択 チェック ボックス**クラウドでホスト**します。 このチェック ボックスが選択されている場合、プロジェクトが Azure web アプリとしてプロビジョニングされ、後で簡単に公開するために有効になります。 **[OK]** をクリックします。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. **Azure web サイトの構成**サイトの自動生成された名前とリージョンを使用して、ダイアログが表示されます。 ダイアログ ボックスに現在サインイン アカウントにも注意してください。 このアカウントが、Azure サブスクリプションが関連付けられているものであるかどうかを確認する Microsoft アカウントでは通常します。

    > [!NOTE]
    > このプロジェクトには、データベースが必要です。 既存のデータベースのいずれかを選択するか、新しいものを作成する必要があります。 データベースは、プロジェクトは既に少量の認証の構成データを格納するローカル データベース ファイルを使用するために必要です。 Azure の web サイトにアプリケーションを展開するときにクラウドにアクセスできるいずれかを選択する必要があるためにもデプロイでこのデータベースがパッケージ化はありません。 **[OK]** をクリックします。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. プロジェクトが作成され、認証オプションと web アプリのオプションをプロジェクトに自動的に構成されます。 このプロセスが完了すると、ローカルでキーを押してプロジェクトを実行 **^ F5**します。 お客様の組織アカウントを使用してサインインする必要があります。 先ほど作成したアカウントのユーザー名とパスワードを入力し、クリックして**サインイン**します。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. 正常にサインインした後、ページの右上隅で、ユーザー名を表示することで認証されましたが、ASP.NET サイトが表示されます。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)

   エラーが発生した場合: 値が null または空にすることはできません。 パラメーター名: linkText ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)

   参照してください、[デバッグ](#dbg)チュートリアルの最後のセクション。

## <a name="basics-of-the-graph-api"></a>Graph API の基本

[Graph API](https://msdn.microsoft.com/library/azure/hh974476.aspx)は、Azure AD ディレクトリの CRUD とオブジェクトの他の操作を実行するために使用するプログラム インターフェイスです。 Visual Studio 2013 で新しいプロジェクトを作成するときに認証するための組織のアカウントのオプションを選択した場合、アプリケーションは Graph API を呼び出す構成既に。 このセクションについて簡単にでは、Graph API のしくみです。

1. 上部にあるサインイン ユーザーの名前をクリックして、実行中のアプリケーションで、ページの右。 これで、ホーム コント ローラーのアクションであるユーザー プロファイル ページに移動されます。 先ほど作成した管理者アカウントのユーザー情報が、テーブルに含まれていることがわかります。 ディレクトリに、この情報を格納し、Graph API が呼び出され、ページが読み込まれるときに、この情報を取得します。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Visual Studio に戻り、展開、**コント ローラー**フォルダーと順に開いて、 **HomeController.cs**ファイル。 表示されます、 **UserProfile()** トークンを取得し、Graph API を呼び出すコードが含まれるアクション。 このコードは、以下が重複しています。

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Graph API を呼び出すには、まず、トークンを取得する必要があります。 トークンが取得されたときに、Graph API へのすべての後続の要求の Authorization ヘッダー内の文字列値を追加する必要があります。 上記のコードの大部分は、トークンを取得する Azure AD への認証、トークンを使用して、Graph api 呼び出しを実行して、ビューで表現できるように、応答を変換の詳細を処理します。

    最も重要な部分については、次の強調表示された行:`UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`します。 この行は、JSON 応答から逆シリアル化されたがあり、ビューに表示されますが、ユーザーの名前を表します。

    簡単な方法は、使用する HttpClient を使用して Graph API を呼び出すし、自分で生データを処理することができますが、 [Graph クライアント ライブラリは NuGet から入手可能](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/)します。 クライアント ライブラリでは、未加工の HTTP 要求と、返されたデータの変換を処理し、.NET 環境での Graph API を使用するはるかに簡単になります。 関連の Graph API コード サンプルを参照してください[GitHub します。](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>アプリケーションを Azure にデプロイします。

次の手順では、Azure にアプリケーションをデプロイする方法を示します。 前の手順で接続して、新しいプロジェクトを Azure に web アプリでできるようになりますが、いくつかの手順で発行する準備が。

1. Visual Studio でクリックし、プロジェクトを右クリックして**発行**します。 **Web の発行**既に構成されている各設定にダイアログが表示されます。 をクリックして、**次**へ移動するボタン、**設定**ページ。 認証を求められますAzure サブスクリプション アカウント (通常は Microsoft アカウント) と前に作成した組織アカウントではなくを使用して認証することを確認します。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. チェック、**組織認証を有効にする**オプション。 **ドメイン**フィールドに、ディレクトリのドメインを入力します。 **アクセス レベル**ドロップダウン リストで選択**シングル サインオン、ディレクトリ データの読み取り**します。 使用した以前のデータベースが既に設定されているかがわかります、**データベース**セクション。 **[発行]** をクリックします。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio により、web サイトのデプロイが開始し、新しいブラウザー ウィンドウが表示されます。 ここでも、ディレクトリに対する認証を求めるメッセージが表示可能性があります。 次の情報を認証したら、Azure で新たに発行された web サイトにリダイレクトします。

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>アプリのデバッグ

次のエラーが発生した場合: 値が null または空にすることはできません。 パラメーター名: linkText

![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)


コードに置き換えます、 *views \shared\\_LoginPartial.cshtml*を次のファイル。

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

ログインしているユーザーには、「Null ユーザー」が表示される場合、アプリを実行した後、サインアウトして先ほど作成した Active Directory アカウントを使用して再度サインインします。

従うの優れたチュートリアルが Rick Rainey の[Deep Dive: Azure Websites および Azure AD を使用して組織の認証](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)します。

## <a name="more-information"></a>説明

- [Deep Dive: Azure Websites と Azure AD を使用して組織の認証](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Azure AD Graph API の概要](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Azure AD での認証シナリオ](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [GitHub での azure AD コード サンプルします。](https://github.com/AzureADSamples)
