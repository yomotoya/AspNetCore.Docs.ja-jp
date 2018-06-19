---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: MVC 4 で OAuth プロバイダーの使用 |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルでは、Facebo などの外部プロバイダーからの資格情報でログインできるようにする ASP.NET MVC 4 web アプリケーションをビルドする方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: f0d053cecbf9a59f258470ee370852e3f112908c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28033576"
---
<a name="using-oauth-providers-with-mvc-4"></a>MVC 4 で OAuth プロバイダーの使用
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルでユーザーが Facebook、Twitter、Microsoft、Google などの外部プロバイダーからの資格情報でログインし、これらのプロバイダーにから機能の一部を統合できる ASP.NET MVC 4 web アプリケーションを構築する方法、web アプリケーションです。 わかりやすくするため、このチュートリアルは Facebook からの資格情報の操作に焦点を当てます。
> 
> 外部資格情報を ASP.NET MVC 5 web アプリケーションを使用するのを参照してください。 [Facebook、Google OAuth2 および OpenID サイン オンで、ASP.NET MVC 5 アプリを作成する](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。
> 
> 数百万のユーザーでは、これらの外部プロバイダーを持つアカウントが既にあるために、大きな利点を提供して web サイトでこれらの資格情報を有効にします。 これらのユーザーを作成し、一連の資格情報を覚えていない場合、サイトにサインアップする傾向があります。 また、これらのプロバイダーのいずれかで、ユーザーがログインに後、は、プロバイダーから操作をソーシャルを組み込むことができます。


## <a name="what-youll-build"></a>新機能のビルド

このチュートリアルでは、次の 2 つの主な目的があります。

1. OAuth プロバイダーからの資格情報でログインするユーザーを有効にします。
2. プロバイダーからアカウント情報を取得し、サイトのアカウントの登録とその情報を統合します。

このチュートリアルの例は、認証プロバイダーとして Facebook を使用してに注目して、任意のプロバイダーを使用するコードを変更することができます。 任意のプロバイダーを実装する手順は、このチュートリアルに表示される手順に非常に似ています。 重要な相違点、プロバイダーの API への直接呼び出しを行うときに設定のみがわかります。

## <a name="prerequisites"></a>必須コンポーネント

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs)または[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

または

- Microsoft Visual Studio 2010 SP1 または[Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

さらに、このトピックでは、ASP.NET MVC と Visual Studio に関する基本的な知識があることを前提とします。 ASP.NET MVC 4 の概要については、必要な場合は、次を参照してください。[紹介し、ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)です。

## <a name="create-the-project"></a>プロジェクトの作成

Visual Studio で、新しい ASP.NET MVC 4 Web アプリケーションを作成し、名前&quot;OAuthMVC&quot;です。 .NET Framework 4.5 または 4 のいずれかを対象にすることができます。

![プロジェクトを作成します。](using-oauth-providers-with-mvc/_static/image1.png)

新しい ASP.NET MVC 4 プロジェクト ウィンドウで、選択**インターネット アプリケーション**のままにして**Razor**ビュー エンジンとして。

![インターネット アプリケーションを選択します。](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>プロバイダーを有効にします。

アプリで AuthConfig.cs をという名前のファイルとプロジェクトが作成されたインターネット アプリケーション テンプレートを使って、MVC 4 web アプリケーションを作成するときに\_開始フォルダーです。

![AuthConfig ファイル](using-oauth-providers-with-mvc/_static/image3.png)

AuthConfig ファイルには、外部認証プロバイダーのクライアントを登録するコードが含まれています。 既定では、このコードはコメント アウトされて、ので、どの外部プロバイダーが有効にします。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

外部認証クライアントを使用するには、このコードのコメントを解除する必要があります。 サイトに追加するプロバイダーのみがコメントを解除します。 このチュートリアルでは、Facebook の資格情報を有効になりますのみです。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

メソッドに、登録パラメーター用の空の文字列が含まれている上記の例に注意してください。 今すぐアプリケーションを実行しようとする場合のパラメーターは、空の文字列は許可されていないため、アプリケーション引数の例外をスローします。 有効な値を提供するには、次のセクションで示すように外部のプロバイダーで、web サイトを登録する必要があります。

## <a name="registering-with-an-external-provider"></a>外部プロバイダーを登録します。

外部プロバイダーからの資格情報を持つユーザーを認証するには、プロバイダーと、web サイトを登録する必要があります。 サイトを登録するときに、クライアントを登録するときに含まれるパラメーター (たとえば、キーまたは id とシークレット) が表示されます。 使用する場合、プロバイダーでは、アカウントが必要です。

このチュートリアルで、これらのプロバイダーを登録するための手順の一部が表示されません。 手順は、通常困難されません。 サイトを正常に登録するには、それらのサイト上の指示に従います。 最初にサイトを登録すると、開発者向けサイトを参照してください。

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

登録時に、サイトを Facebook と、使用できる&quot;localhost&quot;のサイトのドメインと`&quot;http://localhost/&quot;`URL についても、次の図に示すようにします。 Localhost を使用して、ほとんどのプロバイダーを使用できますが、Microsoft プロバイダーで現在動作しません。 Microsoft プロバイダーでは、有効な web サイトの URL を含める必要があります。

![サイトを登録します。](using-oauth-providers-with-mvc/_static/image4.png)

前の図に、アプリ id、アプリのシークレット、および連絡先の電子メールの値が削除されました。 実際には、サイトを登録するときに、それらの値が表示されます。 アプリ id とアプリのシークレットの値をアプリケーションに追加するために注意してくださいすることができます。

## <a name="creating-test-users"></a>テスト ユーザーを作成します。

既存の Facebook アカウントを使用して、サイトをテストするかまわない場合は、このセクションを省略できます。

Facebook アプリの管理 ページで、アプリケーションのテスト ユーザーを簡単に作成できます。 これらを使用して、サイトへのログインにアカウントをテストします。 テスト ユーザーを作成する をクリックして、**ロール**左側のナビゲーション ウィンドウをクリックすると、リンク、**作成**リンクします。

![テスト ユーザーを作成します。](using-oauth-providers-with-mvc/_static/image5.png)

Facebook サイトは、要求するテスト アカウントの数を自動的に作成します。

## <a name="adding-application-id-and-secret-from-the-provider"></a>プロバイダーからのアプリケーション id とシークレットを追加します。

これで、Facebook を id とシークレットが表示されている、AuthConfig ファイルに戻ってし、パラメーター値として追加します。 次に示す値は、実際の値ではありません。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>外部資格情報でログインします。

サイトの外部資格情報を有効にするために必要です。 アプリケーションを実行し、右上隅でログイン リンクをクリックします。 プロバイダーとして Facebook を登録し、プロバイダーのボタンが含まれています、テンプレートが自動的に認識します。 複数のプロバイダーを登録する場合は、それぞれのボタンを自動的に含まれます。

![外部ログイン](using-oauth-providers-with-mvc/_static/image6.png)

このチュートリアルでは、外部プロバイダーのボタンでログをカスタマイズする方法については説明しません。 その情報を参照してください。 [Oauth/openid を使用するときに、ログイン UI をカスタマイズする](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx)です。

Facebook の資格情報でログイン [Facebook] ボタンをクリックします。 外部プロバイダーのいずれかを選択するとそのサイトにリダイレクトしてそのサービスによりにログインするように要求します。

次の図は、Facebook のログイン画面を示しています。 Oauthmvcexample をという名前のサイトへのログインに、Facebook アカウントを使用していることを知ります。

![facebook の認証](using-oauth-providers-with-mvc/_static/image7.png)

Facebook の資格情報を使用してログインは、ページは、サイトが基本的な情報へのアクセスを持つことをユーザーに通知します。

![要求のアクセス許可](using-oauth-providers-with-mvc/_static/image8.png)

選択した後**アプリに移動して**サイトのユーザーを登録する必要があります。 次の図は、ユーザーが Facebook の資格情報でログオンした後、登録ページを示します。 ユーザー名は、通常、プロバイダーから名前が入力されています。

![register](using-oauth-providers-with-mvc/_static/image9.png)

をクリックして**登録**登録を完了します。 ブラウザーを閉じます。

新しいアカウントが、データベースに追加されたことを確認できます。 サーバー エクスプ ローラーで開く、 **DefaultConnection**開くデータベースにあり、**テーブル**フォルダーです。

![データベース テーブル](using-oauth-providers-with-mvc/_static/image10.png)

右クリックし、 **UserProfile**テーブルを選択して**テーブル データの表示**です。

![データを表示します。](using-oauth-providers-with-mvc/_static/image11.png)

追加した新しいアカウントが表示されます。 内のデータを見て**web ページ\_OAuthMembership**テーブル。 追加したアカウントの外部プロバイダーに関連する多くのデータが表示されます。

外部の認証を有効にする場合は完了します。 ただし、さらに統合できますプロバイダーから情報を新しいユーザーの登録プロセスで、次のセクションで示すようにします。

## <a name="create-models-for-additional-user-information"></a>追加のユーザー情報のモデルを作成します。

前のセクションでは、通知するよう、作業をビルトイン アカウントの登録に関する追加情報を取得する必要はありません。 ただし、ほとんどの外部プロバイダーは、ユーザーに関する追加情報を渡します。 次のセクションでは、その情報を保持し、データベースに保存する方法を示します。 具体的には、ユーザーの完全名、ユーザーの個人用の web ページの URI の値、および Facebook がアカウントを検証するかどうかを示す値が保持されます。

使用して[Code First Migrations](https://msdn.microsoft.com/data/jj591621)追加のユーザー情報を格納するテーブルを追加します。 現在のデータベースのスナップショットを作成する必要があります最初、既存のデータベースにテーブルを追加します。 現在のデータベースのスナップショットを作成すると、新しいテーブルのみを含む移行を後で作成できます。 現在のデータベースのスナップショットを作成します。

1. 開く、**パッケージ マネージャー コンソール**
2. コマンドを実行**有効な移行**
3. コマンドを実行**IgnoreChanges 追加移行初期 –**
4. コマンドを実行**データベースの更新**

ここで、新しいプロパティを追加します。 Models フォルダーに、AccountModels.cs ファイルを開き、RegisterExternalLoginModel クラスを検索します。 RegisterExternalLoginModel クラスでは、認証プロバイダーから返される値を保持します。 次の強調表示されている FullName およびリンクをという名前のプロパティを追加します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

また、AccountModels.cs では、ExtraUserInformation という新しいクラスを追加します。 このクラスは、データベース内に作成される新しいテーブルを表します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

UsersContext クラスでは、新しいクラスの DbSet プロパティを作成するのには、次の強調表示されたコードを追加します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

新しいテーブルを作成する準備が整いました。 もう一度と、今度は、パッケージ マネージャー コンソールを開きます。

1. コマンドを実行**追加移行 AddExtraUserInformation**
2. コマンドを実行**データベースの更新**

新しいテーブルは、今すぐデータベースに存在します。

## <a name="retrieve-the-additional-data"></a>追加のデータを取得します。

追加のユーザー データを取得する 2 つの方法ができます。 最初の方法では、既定では、認証要求時に、渡されるユーザーのデータを保持します。 2 番目の方法では、具体的にはプロバイダー API を呼び出すし、詳細情報を要求します。 FullName およびリンクの値は自動的に、Facebook によって再び渡されます。 Facebook がアカウントを検証するかどうかを示す値が、Facebook API を呼び出すことによって取得されます。 最初に、FullName およびリンクの値が設定し、後は値を取得することを確認します。

追加のユーザー データを取得するには、開く、 **AccountController.cs**ファイルで、**コント ローラー**フォルダーです。

このファイルには、ログ記録、登録、およびアカウントを管理するためのロジックが含まれています。 具体的には、呼び出されたメソッドに注意してください**ExternalLoginCallback**と**ExternalLoginConfirmation**です。 これらのメソッド内では、アプリケーションの外部ログイン操作のカスタマイズにコードを追加できます。 最初の行、 **ExternalLoginCallback**メソッドが含まれます。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

追加のユーザー データがで戻された、 **ExtraData**のプロパティ、 **AuthenticationResult**から返されるオブジェクト、 **VerifyAuthentication**メソッドです。 Facebook クライアントにはで次の値が含まれています、 **ExtraData**プロパティ。

- ID
- name
- リンクをクリックする
- 性別
- accesstoken

その他のプロバイダーは、ExtraData プロパティに似ていますが、若干異なるデータがあります。

ユーザーが、サイトに新しい場合は、いくつかの追加のデータを取得して確認ビューにそのデータを渡します。 ユーザーが、サイトに新しい場合にのみ、メソッド内のコードの最後のブロックが実行されます。 次の行に置き換えます。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

次の行。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

この変更には、FullName およびリンクのプロパティの値にはだけが含まれます。

**ExternalLoginConfirmation**メソッド、強調表示されている追加のユーザー情報を保存するのには、次のコードを変更します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>表示を調整します。

[登録] ページで、プロバイダーから取得する追加のユーザー データが表示されます。

**ビュー**/**アカウント**フォルダーを開き、 **ExternalLoginConfirmation.cshtml**です。 ユーザー名の既存のフィールドの下には、FullName、リンク、および PictureLink のフィールドを追加します。

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

アプリケーションを実行し、その他の情報が保存されて新しいユーザーの登録にほぼ準備が整いました。 サイトに既に登録されていないアカウントが必要です。 別のテスト アカウントを使用するか、内の行を削除することができます、 **UserProfile**と**web ページ\_OAuthMembership**で再利用するアカウントのテーブルです。 これらの行を削除するは、アカウントがもう一度登録されていることを確認します。

アプリケーションを実行し、新しいユーザーを登録します。 この時刻には確認 ページが含まれているより多くの値に注意してください。

![register](using-oauth-providers-with-mvc/_static/image12.png)

登録を完了すると、ブラウザーを閉じます。 新しい値を確認するデータベース ファイルの場所、 **ExtraUserInformation**テーブル。

## <a name="install-nuget-package-for-facebook-api"></a>Facebook API の NuGet パッケージをインストールします。

Facebook の提供、 [API](https://developers.facebook.com/docs/reference/apis/)呼び出すことができる操作を実行します。 送信側の HTTP 要求をリダイレクトして、またはそれらの要求の送信を容易にする NuGet パッケージのインストールを使用して、Facebook API を呼び出すことができます。 NuGet をインストールするパッケージ必須ではありませんが、このチュートリアルでは表示されている NuGet パッケージを使用します。 このチュートリアルでは、Facebook c# SDK パッケージを使用する方法を示します。 Facebook API の呼び出しを支援するその他の NuGet パッケージがあります。

**NuGet パッケージの管理**ウィンドウ Facebook c# SDK パッケージの選択します。

![パッケージをインストールします。](using-oauth-providers-with-mvc/_static/image13.png)

ユーザーのアクセス トークンを必要とする操作を呼び出す、Facebook c# SDK を使用します。 次のセクションでは、アクセス トークンを取得する方法を示します。

## <a name="retrieve-access-token"></a>アクセス トークンを取得します。

ほとんどの外部プロバイダーから経過した戻るアクセス トークン、ユーザーの資格情報が検証されます。 このアクセス トークンは、認証されたユーザーに提供される操作を呼び出すことができますので非常に重要です。 そのため、取得し、アクセス トークンを格納するが不可欠より多くの機能を提供する場合です。

外部プロバイダーによってアクセス トークンでがあります有効なだけ時間数に制限されます。 有効なアクセス トークンがあることを確認にを取得するたびに、ユーザー、ログインし、セッションの値として保存することではなく、データベースに保存します。

**ExternalLoginCallback**メソッドにアクセス トークンが渡されるも、 **ExtraData**のプロパティ、 **AuthenticationResult**オブジェクト。 強調表示されたコードを追加**ExternalLoginCallback**でアクセス トークンを保存する、**セッション**オブジェクト。 このコードは、Facebook アカウントを使用して、ユーザーがログインするたびに実行されます。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

この例では、Facebook からアクセス トークンを取得、取得するアクセス トークン、外部プロバイダーから使用という名前のと同じキー &quot;accesstoken&quot;です。

## <a name="logging-off"></a>ログオフ

既定値**ログオフ**メソッドによって、アプリケーションからユーザーをログ記録は、外部プロバイダーからユーザーをログオンできません。 ユーザーが Facebook、外を防止するトークンが、ユーザーがログオフした後に永続化を強調表示されている次のコードを追加することができます、**ログオフ**AccountController 内のメソッドです。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

指定した値、`next`パラメーターは、ユーザーが Facebook からログアウトした後に使用する URL。 を、ローカル コンピューターにテストする場合、ローカル サイトの正しいポート番号を入力します。 、実稼働環境では、contoso.com などの既定のページを提供します。

## <a name="retrieve-user-information-that-requires-the-access-token"></a>アクセス トークンを必要とするユーザー情報を取得します。

これで、アクセス トークンを格納し、Facebook c# SDK パッケージをインストールしたら、Facebook から追加のユーザー情報を要求するのに、それらを一緒に使用することができます。 **ExternalLoginConfirmation**メソッドのインスタンスを作成、 **FacebookClient**アクセス トークンの値を渡すことによってクラスです。 値を要求、**検証**現在の認証されたユーザーのプロパティです。 **検証**プロパティは、Facebook が携帯電話にメッセージを送信するなど、他の手段でアカウントを検証するかどうかを示します。 この値は、データベースに保存します。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

再び、ユーザーのデータベース内のレコードを削除するか、別の Facebook アカウントを使用する必要があります。

アプリケーションを実行し、新しいユーザーを登録します。 見て、 **ExtraUserInformation**確認プロパティの値を表示するテーブル。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、ユーザー認証および登録データを Facebook と統合されているサイトを作成します。 既定の動作が設定されている MVC 4 web アプリケーション、およびその既定の動作をカスタマイズする方法について説明しました。

## <a name="related-topics"></a>関連トピック

- [Azure App Service に認証と SQL DB の ASP.NET MVC アプリの作成し、展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
