---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: MVC 4 で OAuth プロバイダーの使用 |Microsoft Docs
author: tfitzmac
description: このチュートリアルでは、ユーザー Facebo などの外部プロバイダーからの資格情報でログインできるようにする ASP.NET MVC 4 web アプリケーションを構築する方法を紹介しています.
ms.author: aspnetcontent
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 15f6b45706c0711d68b0780a7474d4c939a85fba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823327"
---
<a name="using-oauth-providers-with-mvc-4"></a>MVC 4 で OAuth プロバイダーを使用
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルでは、ユーザーが Facebook、Twitter、Microsoft、Google などの外部プロバイダーからの資格情報でログインし、これらのプロバイダーにから機能の一部を統合できる ASP.NET MVC 4 web アプリケーションを構築する方法、web アプリケーションです。 わかりやすくするため、このチュートリアルは Facebook からの資格情報の操作に焦点を当てます。
> 
> 外部資格情報を使用して、ASP.NET MVC 5 web アプリケーションを参照してください。 [Facebook と Google の OAuth2 や OpenID サインオンでの ASP.NET MVC 5 アプリケーションの作成](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。
> 
> 数百万のユーザーでは、これらの外部プロバイダーを持つアカウントが既にあるため、大きな利点を提供して web サイトでこれらの資格情報を有効にするとします。 これらのユーザーを作成し、一連の資格情報を覚えていない場合、サイトにサインアップする傾向があります。 また、これらのプロバイダーのいずれかで、ユーザーがログインに後、は、ソーシャル プロバイダーの操作を組み込むことができます。


## <a name="what-youll-build"></a>構築します

このチュートリアルでは、2 つの主な目標があります。

1. OAuth プロバイダーからの資格情報でログインするユーザーを有効にします。
2. プロバイダーからのアカウント情報を取得し、サイトのアカウントの登録とその情報を統合します。

このチュートリアルの例では、認証プロバイダーとして Facebook を使用する焦点として、任意のプロバイダーを使用するコードを変更できます。 任意のプロバイダーを実装する手順は、このチュートリアルに表示される手順とよく似ています。 大きな違い、プロバイダーの API への直接呼び出しを行うときに設定のみ表示されます。

## <a name="prerequisites"></a>必須コンポーネント

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs)または[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

または

- Microsoft Visual Studio 2010 SP1 または[Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

さらに、このトピックでは、ASP.NET MVC と Visual Studio に関する基本的な知識がある前提としています。 ASP.NET MVC 4 の概要が必要な場合は、次を参照してください。 [ASP.NET MVC 4 の概要](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)します。

## <a name="create-the-project"></a>プロジェクトの作成

Visual Studio で、新しい ASP.NET MVC 4 Web アプリケーションを作成し、名前を付けます&quot;OAuthMVC&quot;します。 .NET Framework 4.5 または 4 を対象にすることができます。

![プロジェクトを作成します。](using-oauth-providers-with-mvc/_static/image1.png)

新しい ASP.NET MVC 4 プロジェクト ウィンドウで、次のように選択します。**インターネット アプリケーション**して**Razor**ビュー エンジンとして。

![インターネット アプリケーションを選択します。](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>プロバイダーを有効にします。

アプリで AuthConfig.cs という名前のファイル プロジェクトを作成するインターネット アプリケーション テンプレートを使用した MVC 4 web アプリケーションを作成するときに\_開始フォルダー。

![AuthConfig ファイル](using-oauth-providers-with-mvc/_static/image3.png)

AuthConfig ファイルには、クライアントの外部認証プロバイダーを登録するコードが含まれています。 既定では、このコードはコメント アウト、ので、外部プロバイダーがない、有効にします。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

外部認証クライアントを使用するには、このコードのコメントを解除する必要があります。 サイトに追加するプロバイダーのみのコメントを解除します。 このチュートリアルでは、Facebook の資格情報のみ有効にします。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

メソッドが、登録パラメーター用の空の文字列が含まれている上記の例に注目してください。 アプリケーションを実行しようとする場合、アプリケーションは、パラメーターは空の文字列で許可されていないため、引数の例外をスローします。 次のセクションで示すように有効な値を提供するには、外部プロバイダーと、web サイトを登録する必要があります。

## <a name="registering-with-an-external-provider"></a>外部プロバイダーを登録します。

外部プロバイダーからの資格情報を持つユーザーを認証するには、プロバイダーと、web サイトを登録する必要があります。 サイトを登録するときに、クライアントを登録するときに含める (キーまたは id、シークレットなど) のパラメーターを受け取ります。 使用するプロバイダーを使用したアカウントが必要です。

このチュートリアルは、すべての手順でこれらのプロバイダーを登録するために必要にも表示されません。 手順は、通常難しくありません。 サイトを正常に登録するには、これらのサイト上の指示に従います。 サイトの登録を開始するには、開発者向けサイトを参照してください。

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Facebook を使用してサイトを登録するときに行うことができます&quot;localhost&quot;サイト ドメインと`&quot;http://localhost/&quot;`url、次の図に示すようにします。 Localhost を使用してでは、ほとんどのプロバイダーで機能しますが、Microsoft プロバイダーで現在動作しません。 Microsoft プロバイダーでは、有効な web サイトの URL を含める必要があります。

![サイトを登録します。](using-oauth-providers-with-mvc/_static/image4.png)

前の画像では、アプリ id、アプリのシークレット、および連絡先の電子メールの値が削除されました。 実際には、サイトを登録するときに、これらの値が表示されます。 それらをアプリケーションに追加するために、アプリ id とアプリ シークレットの値をメモしてするされます。

## <a name="creating-test-users"></a>テスト ユーザーの作成

既存の Facebook アカウントを使用してサイトをテストしてかまわない場合は、このセクションをスキップできます。

Facebook アプリの管理ページ内で、アプリケーションのテスト ユーザーを簡単に作成することができます。 これらを使用して、サイトへのログインにアカウントをテストします。 クリックしてテスト ユーザーを作成する、**ロール**、クリックすると、左側のナビゲーション ウィンドウのリンク、**作成**リンク。

![テスト ユーザーを作成します。](using-oauth-providers-with-mvc/_static/image5.png)

Facebook のサイトには、要求したテスト アカウントの数が自動的に作成されます。

## <a name="adding-application-id-and-secret-from-the-provider"></a>プロバイダーからのアプリケーション id とシークレットの追加

Facebook から id とシークレットを受信したできた AuthConfig ファイルに戻ってし、パラメーター値として追加します。 次に示す値は、実際の値ではありません。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>外部資格情報でログインします。

サイトの外部資格情報を有効にするために必要です。 アプリケーションを実行し、右上隅のログイン リンクをクリックします。 テンプレートは、プロバイダーとして Facebook を登録して、プロバイダーのボタンが含まれていますに自動的に認識されます。 複数のプロバイダーを登録する場合、それぞれのボタンは、自動的に含まれます。

![外部ログイン](using-oauth-providers-with-mvc/_static/image6.png)

このチュートリアルでは、外部プロバイダーのボタンでログをカスタマイズする方法は説明しません。 詳細については、次を参照してください。 [Oauth/openid を使用する場合は、ログイン UI をカスタマイズする](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx)します。

Facebook の資格情報でログインする Facebook ボタンをクリックします。 外部プロバイダーのいずれかを選択すると、そのサイトにリダイレクトされ、ログインにそのサービスによってメッセージが表示するは。

次の図は、Facebook のログイン画面を示しています。 Oauthmvcexample という名前のサイトにログインする、Facebook アカウントを使用していることを示します。

![facebook 認証](using-oauth-providers-with-mvc/_static/image7.png)

Facebook の資格情報でログインした後は、ページは、サイトが基本的な情報にアクセスできるようをユーザーに通知します。

![要求のアクセス許可](using-oauth-providers-with-mvc/_static/image8.png)

選択した後**アプリに移動して**サイトのユーザーを登録する必要があります。 次の図は、ユーザーが Facebook の資格情報でログインした後に、登録ページを示します。 ユーザー名は、通常、プロバイダーからの名前が入力されています。

![register](using-oauth-providers-with-mvc/_static/image9.png)

クリックして**登録**登録を完了します。 ブラウザーを閉じます。

新しいアカウントが、データベースに追加されたことを確認できます。 サーバー エクスプ ローラーで開く、 **DefaultConnection**開くデータベースにあり、**テーブル**フォルダー。

![データベース テーブル](using-oauth-providers-with-mvc/_static/image10.png)

右クリックし、 **UserProfile**テーブルを選択**テーブル データの表示**します。

![データを表示します。](using-oauth-providers-with-mvc/_static/image11.png)

追加した新しいアカウントが表示されます。 内のデータを見て**web ページ\_OAuthMembership**テーブル。 追加したアカウントの外部プロバイダーに関連する多くのデータが表示されます。

外部認証を有効にする場合は、完了です。 ただし、次のセクションで示すようには、新しいユーザーの登録プロセスに、プロバイダーからの情報を統合することができますさらにします。

## <a name="create-models-for-additional-user-information"></a>追加のユーザー情報のモデルを作成します。

前のセクションでは、通知するように動作する組み込みのアカウントの登録に関する追加情報を取得する必要はありません。 ただし、ほとんどの外部プロバイダーは、ユーザーに関する追加情報を渡します。 次のセクションでは、その情報を保持し、データベースに保存する方法を示します。 具体的には、ユーザーの完全名、ユーザーの個人の web ページの URI の値と Facebook がアカウントを確認するかどうかを示す値が保持されます。

使用する[Code First Migrations](https://msdn.microsoft.com/data/jj591621)追加のユーザー情報を格納するためのテーブルを追加します。 テーブルは、既存のデータベースのため、現在のデータベースのスナップショットを作成する必要があります追加されます。 現在のデータベースのスナップショットを作成すると、後で新しいテーブルのみを含む移行を作成できます。 現在のデータベースのスナップショットを作成します。

1. 開く、**パッケージ マネージャー コンソール**
2. コマンドを実行 **-移行を有効にします。**
3. コマンドを実行**IgnoreChanges 追加移行初期 –**
4. コマンドを実行**データベースの更新**

ここで、新しいプロパティを追加します。 Models フォルダーで、AccountModels.cs ファイルを開き RegisterExternalLoginModel クラスを探します。 RegisterExternalLoginModel クラスは、認証プロバイダーから返される値を保持します。 FullName とリンクを以下の強調表示されているという名前のプロパティを追加します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

また、AccountModels.cs では、ExtraUserInformation という新しいクラスを追加します。 このクラスは、データベースに作成される新しいテーブルを表します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

UsersContext クラスでは、新しいクラスの DbSet プロパティを作成するのには、次の強調表示されたコードを追加します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

新しいテーブルを作成する準備が整いました。 もう一度と、今度は、パッケージ マネージャー コンソールを開きます。

1. コマンドを実行**追加移行 AddExtraUserInformation**
2. コマンドを実行**データベースの更新**

新しいテーブルは、今すぐデータベースに存在します。

## <a name="retrieve-the-additional-data"></a>追加のデータを取得します。

追加のユーザー データを取得する 2 つの方法はあります。 最初の方法は、既定では、認証要求時に、渡されるユーザー データを保持します。 2 番目の方法では、具体的にはプロバイダー API を呼び出すし、詳細情報を要求します。 FullName およびリンクの値は Facebook によって自動的に渡されます。 Facebook がアカウントを確認するかどうかを示す値は、Facebook API を呼び出すことによって取得されます。 最初に、FullName およびリンクの値が設定され、確認済みの値を取得した後で、します。

追加のユーザー データを取得するには、開く、 **AccountController.cs**ファイル、**コント ローラー**フォルダー。

このファイルには、ログ記録、登録、およびアカウントを管理するためのロジックが含まれています。 具体的には、呼び出されたメソッドに注目してください**ExternalLoginCallback**と**ExternalLoginConfirmation**します。 これらのメソッド内では、アプリケーションの外部ログイン操作をカスタマイズするコードを追加できます。 最初の行、 **ExternalLoginCallback**メソッドが含まれます。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

渡される追加のユーザー データ、 **ExtraData**のプロパティ、 **AuthenticationResult**オブジェクトから返される、 **VerifyAuthentication**メソッド。 Facebook クライアントには、次の値が含まれています、 **ExtraData**プロパティ。

- ID
- name
- リンクをクリックする
- 性別
- accesstoken

その他のプロバイダーは、ExtraData プロパティに似ていますが、若干異なるデータがあります。

ユーザーがサイトに新しい場合は、いくつか追加のデータを取得し、確認ビューにそのデータを渡すします。 メソッドのコードの最後のブロックは、ユーザーがサイトに新しい場合にのみ実行されます。 次の行に置き換えます。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

この行には。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

この変更には、FullName およびリンクのプロパティの値にはだけが含まれます。

**ExternalLoginConfirmation**メソッド、強調表示されている追加のユーザー情報を保存するのには、次のコードを変更します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>表示を調整します。

登録ページで、プロバイダーから取得された追加のユーザー データが表示されます。

**ビュー**/**アカウント**フォルダーを開き、 **ExternalLoginConfirmation.cshtml**します。 ユーザー名の既存のフィールドの下には、FullName、リンク、および PictureLink フィールドを追加します。

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

アプリケーションを実行し、保存された追加情報を新しいユーザーを登録する、ほぼ準備が整いました。 サイトに既に登録されていないアカウントが必要です。 別のテスト アカウントを使用か、内の行を削除することができます、 **UserProfile**と**web ページ\_OAuthMembership**で再利用するアカウントのテーブル。 これらの行を削除すると、アカウントが再登録するようになります。

アプリケーションを実行し、新しいユーザーを登録します。 この時間には確認 ページが含まれているより多くの値に注意してください。

![register](using-oauth-providers-with-mvc/_static/image12.png)

登録を完了すると、ブラウザーを閉じます。 新しい値を確認するデータベース ファイルの場所、 **ExtraUserInformation**テーブル。

## <a name="install-nuget-package-for-facebook-api"></a>Facebook API の NuGet パッケージをインストールします。

Facebook の提供、 [API](https://developers.facebook.com/docs/reference/apis/)操作を実行するを呼び出すことができます。 送信側の HTTP 要求を送信するか、それらの要求を送信することを容易にする NuGet パッケージのインストールを使用して、Facebook API を呼び出すことができます。 このチュートリアルで示すようには NuGet パッケージを使用してが NuGet のインストール パッケージが不可欠です。 このチュートリアルでは、Facebook c# SDK パッケージを使用する方法を示します。 Facebook API の呼び出しを支援するその他の NuGet パッケージがあります。

**NuGet パッケージの管理**windows では、Facebook c# SDK パッケージを選択します。

![パッケージをインストールします。](using-oauth-providers-with-mvc/_static/image13.png)

Facebook c# SDK を使用して、ユーザーのアクセス トークンを必要とする操作を呼び出します。 次のセクションでは、アクセス トークンを取得する方法を示します。

## <a name="retrieve-access-token"></a>アクセス トークンを取得します。

ほとんどの外部プロバイダーは、ユーザーの資格情報が検証された後、アクセス トークンで渡すバックアップします。 このアクセス トークンは、認証されたユーザーに提供される操作を呼び出すことができるため、非常に重要です。 そのため、取得して、アクセス トークンを格納するが不可欠ですより多くの機能を提供する場合。

外部プロバイダーによってアクセス トークンがある一定期間のみ、有効なする可能性があります。 有効なアクセス トークンがあることを確認が取得するたびに、ユーザー、ログインし、セッションの値として保存することではなく、データベースに保存します。

**ExternalLoginCallback**メソッドに戻り、アクセス トークンが渡されるも、 **ExtraData**のプロパティ、 **AuthenticationResult**オブジェクト。 強調表示されたコードを追加**ExternalLoginCallback**でアクセス トークンを保存する、**セッション**オブジェクト。 このコードは、Facebook アカウントを使用して、ユーザーがログオンするたびに実行されます。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

この例では、Facebook からアクセス トークンを取得、外部プロバイダーからのアクセス トークンを取得という名前のと同じキーを&quot;accesstoken&quot;します。

## <a name="logging-off"></a>ログオフ

既定の**ログオフ**メソッドは、アプリケーションからユーザーをログ記録は、外部プロバイダーからユーザーをログオンできません。 ログインからユーザーを Facebook、サインアウトし、トークンが、ユーザーがログオフした後に永続化しないようにする、次の強調表示されたコードを追加することができます、**ログオフ**AccountController でメソッド。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

指定した値、`next`パラメーターは、Facebook からユーザーがログオンした後に使用する URL。 をローカル コンピューターにテストする場合、ローカル サイトの正しいポート番号を入力します。 運用環境では、contoso.com などの既定のページを提供します。

## <a name="retrieve-user-information-that-requires-the-access-token"></a>アクセス トークンが必要なユーザー情報を取得します。

これで、アクセス トークンを格納して、Facebook c# SDK パッケージをインストールしたことができます一緒に使用するそれらの Facebook から追加のユーザー情報を要求します。 **ExternalLoginConfirmation**メソッドのインスタンスを作成、 **FacebookClient**アクセス トークンの値を渡すことによってクラス。 値を要求、**検証**の現在の認証済みユーザーのプロパティ。 **検証**プロパティは、Facebook が携帯電話にメッセージを送信するなど、他の方法でアカウントを検証するかどうかを示します。 この値は、データベースに保存します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

再度、ユーザーのデータベース内のレコードを削除するか、別の Facebook アカウントを使用する必要があります。

アプリケーションを実行し、新しいユーザーを登録します。 見て、 **ExtraUserInformation** Verified プロパティの値を表示するテーブル。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、ユーザーの認証と登録データを Facebook と統合されているサイトを作成します。 MVC 4 web アプリケーション、およびその既定の動作をカスタマイズする方法に設定されている既定の動作について説明しました。

## <a name="related-topics"></a>関連トピック

- [Auth と SQL DB を使って、ASP.NET MVC アプリを作成し、Azure App Service にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
