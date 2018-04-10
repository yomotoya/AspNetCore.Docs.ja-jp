---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: チェック アウトおよび PayPal の支払い |Microsoft ドキュメント
author: Erikre
description: このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 0dba613594686a28b82bc6d7701cda6e24b82e2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="checkout-and-payment-with-paypal"></a>チェック アウトおよび PayPal の支払い
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。


このチュートリアルでは、Wingtip Toys サンプル アプリケーションは、ユーザーの承認、登録、PayPal を使用して支払を変更する方法について説明します。 ログオンしているユーザーだけでは、製品を購入するための承認があります。 既に ASP.NET 4.5 Web フォーム プロジェクト テンプレートの組み込みのユーザーの登録の機能にはでは必要なものの多くが含まれます。 PayPal Express のチェック アウト機能を追加します。 このチュートリアルでは、テスト環境では、実際の資金は転送されません PayPal developer を使用します。 チュートリアルの最後に、ショッピング カート、チェック アウト] ボタンをクリックすると、PayPal のテストの web サイトにデータを転送するに追加する製品を選択して、アプリケーションをテストします。 PayPal テストの web サイトの配布と支払い情報を確認して、ローカルの Wingtip Toys サンプル アプリケーションを確認し、購入を完了に戻ります。

そのアドレスのスケーラビリティとセキュリティは、オンライン ショッピングに特化したいくつかのサード パーティの支払いを経験豊富なプロセッサがあります。 ASP.NET 開発者は、ショッピングを実装して、ソリューションを購入する前に、サード パーティの支払いソリューションを利用する利点を検討してください。

> [!NOTE] 
> 
> Wingtip Toys のサンプル アプリケーションは、ASP.NET web 開発者に特定の ASP.NET 概念と使用可能な機能を示すために設計されました。 このサンプル アプリケーションは、スケーラビリティ、およびセキュリティに関してどのような状況が最適化されません。


## <a name="what-youll-learn"></a>学習する内容。

- フォルダー内の特定のページへのアクセスを制限する方法。
- 匿名のショッピング カートから既知のショッピング カートを作成する方法です。
- プロジェクトの SSL を有効にする方法です。
- OAuth プロバイダーをプロジェクトに追加する方法。
- PayPal を使用して、PayPal のテスト環境を使用して製品を購入する方法です。
- PayPal から詳細を表示する方法、 **DetailsView**コントロール。
- PayPal から取得の詳細で、Wingtip Toys アプリケーションのデータベースを更新する方法。

## <a name="adding-order-tracking"></a>注文の追跡を追加します。

このチュートリアルでは、ユーザーが作成される、注文からデータを追跡するために 2 つの新しいクラスを作成します。 クラスは、出荷情報、注文書合計、および支払いの確認に関するデータを追跡します。

### <a name="add-the-order-and-orderdetail-model-classes"></a>順序と OrderDetail モデル クラスを追加します。

このチュートリアル シリーズの前半のカテゴリ、製品、スキーマを定義し、ショッピング カートのアイテムを作成することで、 `Category`、 `Product`、および`CartItem`内のクラス、*モデル*フォルダーです。 製品の注文と注文の詳細のスキーマを定義する 2 つの新しいクラスを追加します。

1. **モデル**フォルダー、という名前の新しいクラスを追加*Order.cs*です。   
   新しいクラス ファイルがエディターで表示されます。
2. 次のように、既定のコードに置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. 追加、 *OrderDetail.cs*クラスを*モデル*フォルダーです。
4. 既定のコードを次のコードに置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order`と`OrderDetail`クラスには、購入および配布に使用される順序情報を定義するスキーマが含まれています。

さらに、エンティティ クラスを管理して、データベースへのデータ アクセスを提供するデータベース コンテキスト クラスを更新する必要があります。 新しく作成された順序を追加すると`OrderDetail`モデル クラスを`ProductContext`クラスです。

1. **ソリューション エクスプ ローラー**を検索して開く、 *ProductContext.cs*ファイル。
2. 強調表示されたコードを追加、 *ProductContext.cs*ファイルの次のようにします。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

このチュートリアル シリーズのコードで説明したように、 *ProductContext.cs*ファイルを追加、`System.Data.Entity`名前空間、Entity Framework のすべてのコア機能にアクセスできるようにします。 この機能には、クエリ、挿入、更新、および厳密に型指定されたオブジェクトを使用してデータを削除する機能が含まれています。 上記のコードで、`ProductContext`クラスでは、Entity Framework へのアクセス権を追加、新たに追加する`Order`と`OrderDetail`クラスです。

## <a name="adding-checkout-access"></a>チェック アウトのアクセスを追加します。

Wingtip Toys のサンプル アプリケーションは、匿名ユーザーを確認し、製品をショッピング カートに追加を許可します。 ただし、匿名ユーザーがショッピング カートに追加する製品を購入する場合、する必要がありますログオン サイトにします。 ログオンしている、されたら、制限されたページ、Web アプリケーションのチェック アウトを処理し、購入のプロセスにアクセスできます。 これらの制限されたページに含まれる、*チェック アウト*アプリケーションのフォルダーです。

### <a name="add-a-checkout-folder-and-pages"></a>チェック アウト フォルダーとページを追加します。

作成、*チェック アウト*フォルダーとにチェック アウト プロセス中に、顧客に表示されるページです。 このチュートリアルで後でこれらのページが更新されます。

1. プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**新しいフォルダーを追加**です。 

    ![チェック アウトおよび PayPal - 新しいフォルダーに支払い](checkout-and-payment-with-paypal/_static/image1.png)
2. 新しいフォルダーの名前を付けます*チェック アウト*です。
3. 右クリックし、*チェック アウト*クリックしてフォルダー**追加**-&gt;**新しい項目の**します。 

    ![チェック アウトおよび PayPal の新しい項目の追加の支払い](checkout-and-payment-with-paypal/_static/image2.png)
4. **[新しい項目の追加]** ダイアログ ボックスが表示されます。
5. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。 次に、中央のペインでは、次のように選択します。**マスター ページを含む Web フォーム**し名前を付けます*CheckoutStart.aspx*です。 

    ![チェック アウトおよび - PayPal の支払いは、新しい項目] ダイアログ ボックスを追加します。](checkout-and-payment-with-paypal/_static/image3.png)
6. 以前と同様、選択、 *Site.Master*マスター ページとしてファイル。
7. ページを追加、次に、*チェック アウト*前と同じ手順を使用してフォルダー。   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Web.config ファイルを追加します。

新しいを追加して*Web.config*ファイルの名前を*チェック アウト*フォルダー、ことができます、フォルダーに含まれるすべてのページにアクセスを制限します。

1. 右クリックし、*チェック アウト*フォルダーと選択**追加** - &gt; **新しい項目の**します。  
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。 次に、中央のペインでは、次のように選択します。 **Web 構成ファイル**、の既定の名前を受け入れる*Web.config*、し、[**追加**です。
3. 既存の XML の内容を置き換える、 *Web.config*を次のファイル。  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. 保存、 *Web.config*ファイル。

*Web.config*ファイルは、Web アプリケーションのすべての不明なユーザーにする必要がありますに含まれているページへのアクセスが拒否されることを指定します、*チェック アウト*フォルダーです。 ただし、ユーザー アカウントは登録、ログオンしている場合、既知のユーザーであるし、内のページにはアクセス、*チェック アウト*フォルダーです。

ASP.NET の構成が、階層構造に従うことを確認することが重要で各*Web.config*ファイルが含まれているフォルダーとすべての子ディレクトリ下にある構成設定が適用されます。

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>プロジェクトの SSL を有効にします。

 安全な Sockets Layer (SSL) は、Web サーバーと Web クライアントの暗号化を使用するより安全に通信するために使用できるように定義されているプロトコルです。 SSL を使用しない場合、クライアントとサーバー間で送信されるデータには、ネットワークに物理的にアクセスできる任意のユーザーを見つけ出すパケットできます。 さらに、いくつかの一般的な認証スキーム安全ではありませんプレーンな HTTP 経由でします。 具体的には、基本認証とフォーム認証は、暗号化されていない資格情報を送信します。 セキュリティで保護するには、これらの認証スキームは、SSL を使用する必要があります。 

1. **ソリューション エクスプ ローラー**をクリックして、 **WingtipToys** [プロジェクト] を押します**F4**を表示する、**プロパティ**ウィンドウです。
2. 変更**SSL を有効に**に`true`です。
3. コピー、 **SSL URL**後で使用できるようにします。   
 SSL url は、 `https://localhost:44300/` (下図のように) 以前 SSL Web サイトに作成した場合を除き、します。   
    ![プロジェクト プロパティ](checkout-and-payment-with-paypal/_static/image4.png)
4. **ソリューション エクスプ ローラー**を右クリックして、 **WingtipToys**プロジェクトし、クリックして**プロパティ**です。
5. 左側のタブをクリックして**Web**です。
6. 変更、**プロジェクト Url**を使用する、 **SSL URL**以前に保存します。   
    ![プロジェクトの Web プロパティ](checkout-and-payment-with-paypal/_static/image5.png)
7. キーを押してページを保存**CTRL + S**です。
8. **Ctrl キーを押しながら F5 キーを押して**アプリケーションを実行します。 Visual Studio の SSL の警告を回避できるようにするオプションが表示されます。
9. をクリックして**はい**IIS Express の SSL 証明書を信頼して続行します。   
    ![IIS Express の SSL 証明書の詳細](checkout-and-payment-with-paypal/_static/image6.png)  
 セキュリティの警告が表示されます。
10. をクリックして**はい**localhost 証明書をインストールします。   
    ![セキュリティ警告] ダイアログ ボックス](checkout-and-payment-with-paypal/_static/image7.png)  
 ブラウザー ウィンドウが表示されます。

これで SSL を使用してローカルで、Web アプリケーションを簡単にテストできます。

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>OAuth 2.0 プロバイダーを追加します。

ASP.NET Web フォームでは、メンバーシップと認証の拡張オプションを提供します。 これらの拡張機能には、OAuth が含まれます。 OAuth は、web、モバイル、およびデスクトップ アプリケーションからの単純で標準的な方法で安全な承認を許可するオープン プロトコルです。 ASP.NET Web フォーム テンプレートは、認証プロバイダーとして Facebook、Twitter、Google、Microsoft を公開するのに OAuth を使用します。 このチュートリアルでは、認証プロバイダーとして Google のみを使用して、任意のプロバイダーを使用するコードを簡単に変更できます。 その他のプロバイダーを実装する手順は、このチュートリアルに表示される手順に非常に似ています。

認証に加えて、チュートリアルもロールを使用して承認を実装します。 追加するユーザーのみ、`canEdit`ロールはデータを変更するようになります (作成、編集、または連絡先を削除) します。

> [!NOTE] 
> 
> アプリケーションの Windows Live では、作業用の web サイトの URL をライブのみ承認し、ログインをテストするため、ローカルの web サイトの URL を使用することはできません。


次の手順を使用すると、Google の認証プロバイダーを追加できます。

1. 開く、*アプリ\_Start\Startup.Auth.cs*ファイル。
2. コメント文字を削除、`app.UseGoogleAuthentication()`メソッドとしてメソッドが表示されるように依存します。 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. 移動し、 [Google Developers Console](https://console.developers.google.com/)です。 Google developer 電子メール アカウント (gmail.com など) を使用してサインインする必要があります。 Google アカウントがあるない場合は、選択、**アカウントを作成する**リンクします。   
   次に、表示、 **Google Developers Console**です。   
    ![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)
4. クリックして、**プロジェクトの作成**] ボタンをクリックし、プロジェクト名と (既定値を使用することができます) ID を入力します。 [] をクリックして、**契約のチェック ボックス**と**作成**ボタンをクリックします。  

    ![Google - 新しいプロジェクト](checkout-and-payment-with-paypal/_static/image9.png)

   数秒で新しいプロジェクトを作成して、ブラウザーは、新しいプロジェクト] ページを表示します。
5. 左側のタブをクリックして**Api &amp; auth**、順にクリック**資格情報**です。
6. クリックして、**新しいクライアント ID を作成する**[ **OAuth**です。   
   **クライアント ID を作成**ダイアログが表示されます。   
    ![Google のクライアント ID を作成します。](checkout-and-payment-with-paypal/_static/image10.png)
7. **クライアント ID を作成**ダイアログ ボックスで、既定値を保持**Web アプリケーション**アプリケーションの種類。
8. 設定、**承認済みの JavaScript 生成元**このチュートリアルで以前に使用する SSL URL を (`https://localhost:44300/` SSL の他のプロジェクトを作成した場合を除く)。   
   この URL は、アプリケーションのための原点です。 このサンプルでは、のみ入力する localhost の URL のテスト。 ただし、localhost および運用のために複数の Url を入力することができます。
9. 設定、**リダイレクト URI の承認**以下。 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   この値は、その ASP.NET OAuth の URI をユーザーが google OAuth サーバーと通信します。 上記で使用する SSL URL に注意してください ( `https://localhost:44300/` SSL の他のプロジェクトを作成した場合を除く)。
10. クリックして、**クライアント ID を作成**ボタンをクリックします。
11. Google 開発者コンソールの左側のメニューをクリックして、**同意画面**メニュー項目は、電子メール アドレスと製品名を設定します。 フォームが完了したら、をクリックして**保存**です。
12. クリックして、 **Api**メニュー項目、スクロール ダウン] をクリック、**オフ**横に**Google + API**です。   
    このオプションを受け入れると、Google + API が有効になります。
13. 更新する必要があります、 **Microsoft.Owin** 3.0.0 NuGet パッケージです。   
    **ツール**メニューの [ **NuGet Package Manager**し、[ **Manage NuGet Packages for Solution**です。  
    **NuGet パッケージの管理**ウィンドウ、検索、更新、 **Microsoft.Owin** 3.0.0 パッケージです。
14. Visual Studio で、更新、`UseGoogleAuthentication`のメソッド、 *Startup.Auth.cs*コピー アンド ペーストでページ、**クライアント ID**と**クライアント シークレット**メソッドにします。 **クライアント ID**と**クライアント シークレット**の下に表示される値のサンプルし、は機能しません。 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. キーを押して**ctrl キーを押しながら f5 キーを押して**アプリケーションをビルドして実行します。 クリックして、**ログイン**リンクします。
16. 下にある**のログインに別のサービスを使用して**をクリックして**Google**です。  
    ![ログイン](checkout-and-payment-with-paypal/_static/image11.png)
17. 資格情報を入力する必要がある場合、資格情報を入力する google サイトにリダイレクトされます。  
    ![Google のサインイン](checkout-and-payment-with-paypal/_static/image12.png)
18. 資格情報を入力した後は、作成した web アプリケーションへのアクセス許可を与えるが求められます。  
    ![プロジェクトの既定のサービス アカウント](checkout-and-payment-with-paypal/_static/image13.png)
19. をクリックして**受け入れる**です。 今すぐにリダイレクトされます、**登録**のページ、 **WingtipToys**アプリケーションが Google アカウントを登録することができます。  
    ![Google アカウントに登録します。](checkout-and-payment-with-paypal/_static/image14.png)
20. Gmail アカウントで使用するローカル電子メール登録名を変更することがあるが、通常、既定の電子メール エイリアス (つまり、認証に使用するもの) を保持します。 をクリックして**ログイン**上記のようにします。

### <a name="modifying-login-functionality"></a>ログインの機能を変更します。

既に説明したようこのチュートリアルのシリーズで、ユーザー登録機能の多くに含まれている ASP.NET Web フォーム テンプレート既定です。 これで、既定値を変更するは*Login.aspx*と*Register.aspx*を呼び出してページ、`MigrateCart`メソッドです。 `MigrateCart`メソッドは、匿名のショッピング カートに新しくログインしたユーザーを関連付けます。 ユーザーを関連付けると、ショッピング カート、Wingtip Toys のサンプル アプリケーションはビジットの間でユーザーのショッピング カートを保持することになります。

1. **ソリューション エクスプ ローラー**を検索して開く、*アカウント*フォルダーです。
2. という名前の分離コード ページを変更して*Login.aspx.cs*を次のように表示されるように、黄色で強調表示されているコードを含めます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. 保存、 *Login.aspx.cs*ファイル。

ここでは、警告の定義が存在しないことを無視することができます、`MigrateCart`メソッドです。 このチュートリアルでは、少し後でそれを追加するは。

*Login.aspx.cs*分離コード ファイルには、ログイン方法がサポートしています。 Login.aspx ページを調べることによってわかりますこのページに「ログイン」のボタンが含まれている時に [トリガー] をクリックして、`LogIn`分離コードでハンドラー。

ときに、`Login`メソッドを*Login.aspx.cs*が呼び出されたが、ショッピング カートという名前の新しいインスタンス`usersShoppingCart`を作成します。 ショッピング カート (GUID) の ID が取得され、設定、`cartId`変数。 次に、`MigrateCart`両方を渡して、メソッドが呼び出されます、`cartId`と、このメソッドへのログイン ユーザーの名前。 移行時に、ショッピング カートは、匿名のショッピング カートの識別に使用される GUID は、ユーザー名に置き換えられます。

変更だけでなく、 *Login.aspx.cs* 、ユーザーがログインするときに、ショッピング カートを移行する分離コード ファイルも変更する必要があります、 *Register.aspx.cs 分離コード ファイル*ショッピング カートを移行するにはときに、ユーザーは新しいアカウントを作成しログに記録します。

1. *アカウント*フォルダーを開き、分離コード ファイルの名前は*Register.aspx.cs*です。
2. 次のように表示されるように、黄色でコードを含めることによって分離コード ファイルを変更します。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. 保存、 *Register.aspx.cs*ファイル。 に関する警告を無視してもう一度、`MigrateCart`メソッドです。

使用したコードに注目してください、`CreateUser_Click`イベント ハンドラーにで使用するコードによく似ていますが、`LogIn`メソッドです。 ユーザーが登録またはサイトへの呼び出しへのログイン、`MigrateCart`メソッドになります。

## <a name="migrating-the-shopping-cart"></a>ショッピング カートを移行します。

ショッピング カートを使用して、移行するコードを追加するには、ログで、登録プロセスの更新がある場合は、これで、`MigrateCart`メソッドです。

1. **ソリューション エクスプ ローラー**、検索、*ロジック*フォルダーと open、 *ShoppingCartActions.cs*クラス ファイルです。
2. 既存のコードに黄色で強調表示のコードを追加、 *ShoppingCartActions.cs*ファイル、ように内のコード、 *ShoppingCartActions.cs*ファイルが次のように表示されます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart`メソッドでは、既存の cartId を使用して、ユーザーのショッピング カートを検索します。 コードが次に、ショッピング カートのすべての項目をループし、置換、`CartId`プロパティ (で指定されたとおり、`CartItem`スキーマ) のログイン ユーザー名。

### <a name="updating-the-database-connection"></a>データベース接続の更新

このチュートリアルを使用する場合、**構築済み**Wingtip Toys サンプル アプリケーション、既定のメンバーシップ データベースを再作成する必要があります。 既定の接続文字列を変更すると、メンバーシップ データベースは次回アプリケーションを実行します。

1. 開く、 *Web.config*プロジェクトのルートにあるファイルです。
2. 次のように表示されるように、既定の接続文字列を更新します。   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>PayPal の統合

PayPal は、オンライン ショップで支払いを受け付ける web ベースの課金プラットフォームです。 このチュートリアルでは、PayPal のチェック アウトの Express の機能をアプリケーションに統合する方法について次に説明します。 Express のチェック アウトにより、PayPal をショッピング カートに追加したアイテムの支払いに使用できます。

### <a name="create-paylpal-test-accounts"></a>PaylPal テスト アカウントを作成します。

PayPal のテスト環境を使用するのには、作成し、開発者のテスト アカウントを確認する必要があります。 開発者テスト アカウントを使用して、購入者のテスト アカウントとアカウントの作成、seller テストされます。 開発者のテスト アカウントの資格情報もにより、Wingtip Toys サンプル アプリケーション PayPal テスト環境にアクセスします。

1. ブラウザーでは、サイトのテスト、PayPal 開発者に移動します。   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. PayPal の開発者アカウントを持っていない場合は、新しいアカウントを作成] をクリックして**サインアップ**サインアップの手順に従います。 既存の PayPal の開発者アカウントがある場合は、サインイン] をクリックして**ログで**です。 PayPal 開発者アカウントをこのチュートリアルで後ほど Wingtip Toys のサンプル アプリケーションをテストする必要があります。
3. だけ、PayPal 開発者アカウントのサインアップしている場合は、PayPal、PayPal 開発者アカウントを確認する必要があります。 電子メール アカウントに送信される PayPal の手順に従ってアカウントを確認できます。 PayPal 開発者アカウントを確認した後は、サイトのテスト、PayPal 開発者に再度ログインします。
4. 後で、PayPal の開発者アカウントを既にしていない場合は、PayPal buyer テスト アカウントを作成する必要があります、PayPal developer サイトにログインしているいずれかであります。 PayPal の [サイト] をクリックに購入者のテスト アカウントを作成する、**アプリケーション**] タブをクリックして**サンド ボックス アカウント**です。   
 **サンド ボックスのテスト アカウント**ページが表示されます。   

    > [!NOTE] 
    > 
    > PayPal の開発者向けサイトには、販売者のテスト用のアカウントが既に用意されています。

    ![チェック アウトおよび PayPal のサンド ボックスのテスト アカウントでの支払い](checkout-and-payment-with-paypal/_static/image15.png)
5. サンド ボックス テスト アカウント] ページで、**アカウントの作成**です。
6. **テスト アカウントの作成**] ページでは、テスト アカウントの電子メール アドレスと、任意のパスワードに、購入者を選択します。   

    > [!NOTE] 
    > 
    > 購入者の電子メール アドレスと、このチュートリアルの最後の Wingtip Toys サンプル アプリケーションをテストするためのパスワードが必要です。

    ![チェック アウトおよび PayPal のサンド ボックスのテスト アカウントでの支払い](checkout-and-payment-with-paypal/_static/image16.png)
7. クリックして、購入者のテスト アカウントを作成、**アカウントの作成**ボタンをクリックします。  
 **サンド ボックスのテスト アカウント**ページが表示されます。 

    ![チェック アウトおよび PayPal - PaylPal アカウントに支払い](checkout-and-payment-with-paypal/_static/image17.png)
8. **サンド ボックスのテスト アカウント**] ページで、をクリックして、**読み上げて**電子メール アカウント。  
    **プロファイル**と**通知**オプションが表示されます。
9. 選択、**プロファイル**オプション、をクリックして**API の資格情報**商業テスト アカウント用の API 資格情報を表示します。
10. テスト API 資格情報をメモ帳にコピーします。

テスト環境 PayPal に表示されているテスト API をクラシック資格情報 (ユーザー名、パスワード、および署名)、Wingtip Toys サンプル アプリケーションの API 呼び出しを行う必要になります。 次の手順では、資格情報を追加します。

### <a name="add-paypal-class-and-api-credentials"></a>PayPal クラスと API の資格情報を追加します。

1 つのクラスに PayPal のコードの大半を配置します。 このクラスには、PayPal との通信に使用するメソッドが含まれています。 またには、PayPal の資格情報は、このクラスに追加します。

1. Wingtip Toys サンプル アプリケーションの Visual Studio 内で右クリックし、**ロジック**クリックしてフォルダー**追加** - &gt; **新しい項目の**します。   
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. **Visual c#**から、**インストール**左側のウィンドウ、[**コード**です。
3. 中央のペインでは、次のように選択します。**クラス**です。 この新しいクラスの名前を**PayPalFunctions.cs**です。
4. **[追加]** をクリックします。  
   新しいクラス ファイルがエディターで表示されます。
5. 既定のコードを次のコードに置き換えます。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. ショップ API 資格情報 (ユーザー名、パスワード、および署名)、PayPal のテスト環境に関数呼び出しを行うことができるように、このチュートリアルで先ほどの表示を追加します。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> このサンプル アプリケーションでは、c# ファイル (*.cs) に単に資格情報を追加します。 ただし、実装済みのソリューションで、構成ファイルで、資格情報の暗号化を検討してください。


NVPAPICaller クラスには、PayPal 機能の大部分が含まれています。 クラスのコードでは、購入、PayPal のテスト環境からテストの作成に必要なメソッドを提供します。 次の 3 つの PayPal 関数は、購入に使用されます。

- `SetExpressCheckout` 関数
- `GetExpressCheckoutDetails` 関数
- `DoExpressCheckoutPayment` 関数

`ShortcutExpressCheckout`メソッドは、ショッピング カートの呼び出しからテストの購入情報と製品の詳細を収集、 `SetExpressCheckout` PayPal 関数。 `GetCheckoutDetails`メソッドは、購入の詳細と呼び出しを確認、`GetExpressCheckoutDetails`テスト購買を行う前に、PayPal 関数。 `DoCheckoutPayment`メソッドを呼び出して、テスト環境からテスト購入を完了すると、 `DoExpressCheckoutPayment` PayPal 関数。 残りのコードは、PayPal メソッドおよび文字列をエンコード、文字列をデコード、配列、処理、および資格情報を決定するなどのプロセスをサポートします。

> [!NOTE] 
> 
> PayPal を使用すると、基に省略可能な購入の詳細を含める[PayPal の API 仕様](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)です。 Wingtip Toys サンプル アプリケーションでコードを拡張するには、ローカライズの詳細、製品の説明、税、カスタマー サービスの数、だけでなく他の多くの省略可能なフィールドを含めることができます。


注意して、戻り値と [キャンセル] Url で指定されている、 **ShortcutExpressCheckout**メソッドは、ポート番号を使用します。

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Visual Web Developer は、SSL を使用して、web プロジェクトを実行するときに一般的 44300 ポートは、web サーバーの使用します。 上記のように、ポート番号は 44300 がします。 アプリケーションを実行するときに、別のポート番号を表示できます。 ポート番号のニーズを正しく設定するコードでことができます成功したは、このチュートリアルの最後に、Wingtip Toys のサンプル アプリケーションを実行します。 このチュートリアルの次のセクションでは、ローカル ホストのポート番号を取得し、PayPal クラスを更新する方法について説明します。

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>更新プログラム、LocalHost にポート番号、PayPal クラス

Wingtip Toys のサンプル アプリケーションは、PayPal テスト サイトに移動し、Wingtip Toys のサンプル アプリケーションのローカル インスタンスを返すことで、製品を購入します。 正しい URL を返す PayPal するために、ローカルで実行中のポート番号を指定する必要があります。 アプリケーション、PayPal のコードに上記のサンプルです。

1. プロジェクト名を右クリックして (**WingtipToys**) で**ソリューション エクスプ ローラー**選択**プロパティ**です。
2. 左側の列を選択、 **Web**タブです。
3. ポート番号を取得、**プロジェクト Url**ボックス。
4. 必要な場合、更新、`returnURL`と`cancelURL`PayPal クラスで (`NVPAPICaller`) で、 *PayPalFunctions.cs* web アプリケーションのポート番号を使用するファイル。   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

ここで追加したコードでは、ローカル Web アプリケーションの適切なポートは一致します。 PayPal は、ローカル コンピューターに正しい URL に戻るにことができます。

### <a name="add-the-paypal-checkout-button"></a>PayPal のチェック アウト] ボタンを追加します。

プライマリの PayPal 関数は、サンプル アプリケーションに追加されましたが、これで、マークアップとこれらの関数の呼び出しに必要なコードを追加することを開始できます。 最初に、ユーザーが買い物カゴのページに表示されるチェック アウト] ボタンを追加する必要があります。

1. 開く、*後*ファイル。
2. ファイルの一番下までスクロールし、検索、`<!--Checkout Placeholder -->`コメントです。
3. コメントを置き換える、`ImageButton`コントロールを印は次のように置き換えられます。  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. *ShoppingCart.aspx.cs*後、ファイル、 `UpdateBtn_Click` 、ファイルの末尾付近のイベント ハンドラーを追加、`CheckOutBtn_Click`イベントのハンドラー。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. また、 *ShoppingCart.aspx.cs*ファイルの参照を追加する、`CheckoutBtn`新しいイメージ ボタンは次のように参照されているため。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. 両方に変更を保存、*後*ファイルおよび*ShoppingCart.aspx.cs*ファイル。
7. メニューから、次のように選択します。**デバッグ**-&gt;**ビルド WingtipToys**です。  
   プロジェクトが再構築で新しく追加された**ImageButton**コントロール。

### <a name="send-purchase-details-to-paypal"></a>注文書の詳細を PayPal に送信します。

ユーザーがクリックしたとき、**チェック アウト**ショッピング カート ページ上のボタン (*後*)、発注プロセスを始めます。 次のコードでは、製品を購入するために必要な最初の PayPal 関数を呼び出します。

1. *チェック アウト*フォルダーを開き、分離コード ファイルの名前は*CheckoutStart.aspx.cs*です。   
   分離コード ファイルを開くことを確認します。
2. 既存のコードを次のコードに置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

アプリケーションのユーザーがクリックしたとき、**チェック アウト**ショッピング カート] ページで、ブラウザーのボタンに移動、 *CheckoutStart.aspx*ページ。 ときに、 *CheckoutStart.aspx*ロード、ページ、`ShortcutExpressCheckout`メソッドが呼び出されます。 この時点では、ユーザーは、PayPal のテストの web サイトに転送されます。 PayPal サイトで、ユーザー、PayPal の資格情報を入力、購入の詳細を確認、PayPal 契約を受け入れるおよび Wingtip Toys サンプル アプリケーションに返します場所、`ShortcutExpressCheckout`メソッドが完了しました。 ときに、`ShortcutExpressCheckout`メソッドが完了したら、それがユーザーをリダイレクトして、 *CheckoutReview.aspx* ] ページで指定された、`ShortcutExpressCheckout`メソッドです。 これにより、ユーザーは、Wingtip Toys サンプル アプリケーション内で注文の詳細を確認できます。

### <a name="review-order-details"></a>注文の詳細の確認

PayPal から返された後、 *CheckoutReview.aspx* Wingtip Toys のサンプル アプリケーションのページには、注文の詳細が表示されます。 このページでは、ユーザーが製品を購入する前に、注文の詳細を確認します。 *CheckoutReview.aspx*ページを次のように作成する必要があります。

1. *チェック アウト*フォルダーを開き、という名前のページ*CheckoutReview.aspx*です。
2. 既存のマークアップを次のように置き換えます。   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. という名前の分離コード ページを開く*CheckoutReview.aspx.cs*し、既存のコードを次に置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**DetailsView** PayPal から返された注文の詳細を表示するコントロールを使用します。 また、上記のコード、注文の詳細、Wingtip Toys をデータベースに保存として、`OrderDetail`オブジェクト。 ユーザーがクリックしたときに、**注文完了**ボタンにリダイレクトされます、 *CheckoutComplete.aspx*ページ。

> [!NOTE] 
> 
> **ヒント。**
> 
> マークアップで、 *CheckoutReview.aspx* ] ページで、ことに注意して、`<ItemStyle>`タグが使用中の項目のスタイルを変更する、 **DetailsView**ページの下部にあるコントロールです。 内のページを表示して**デザイン ビュー** (を選択して**デザイン**Visual Studio の左下隅にある) を選択し、 **DetailsView**制御、および、を選択します。**スマート タグ**(上部にある矢印アイコン コントロールの右) を表示することができます、 **DetailsView タスク**です。
> 
> ![チェック アウトと PayPal の支払フィールドを編集します。](checkout-and-payment-with-paypal/_static/image18.png)
> 
> 選択して**フィールドの編集**、**フィールド**] ダイアログ ボックスが表示されます。 このダイアログ ボックスで簡単に制御できます visual のプロパティなど**後**の**DetailsView**コントロール。
> 
> ![チェック アウトおよび PayPal - フィールド] ダイアログ ボックスでの支払い](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>購入を完了します。

*CheckoutComplete.aspx* PayPal のページでは、購入します。 前述のように、ユーザーをクリックして、**注文完了**ボタンをアプリケーションが移動する前に、 *CheckoutComplete.aspx*ページ。

1. *チェック アウト*フォルダーを開き、という名前のページ*CheckoutComplete.aspx*です。
2. 既存のマークアップを次のように置き換えます。   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. という名前の分離コード ページを開く*CheckoutComplete.aspx.cs*し、既存のコードを次に置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

ときに、 *CheckoutComplete.aspx*ページが読み込まれると、`DoCheckoutPayment`メソッドが呼び出されます。 以前に説明したように、`DoCheckoutPayment`メソッドが、PayPal のテスト環境から購入を完了します。 PayPal の注文の購入が完了した後、 *CheckoutComplete.aspx*ページには、支払トランザクションが表示されます。`ID`購入者にします。

### <a name="handle-cancel-purchase"></a>キャンセルの注文書を処理します。

ユーザーは、購入を取り消しますが、それらに送られます、 *CheckoutCancel.aspx*ページの場所が表示の順序が取り消されました。

1. という名前のページを開く*CheckoutCancel.aspx*で、*チェック アウト*フォルダーです。
2. 既存のマークアップを次のように置き換えます。   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>注文書のエラーを処理します。

発注プロセス中にエラーを処理する、 *CheckoutError.aspx*ページ。 分離コード、 *CheckoutStart.aspx* ] ページで、 *CheckoutReview.aspx* ] ページで、および*CheckoutComplete.aspx*ページそれぞれをリダイレクトする、 *CheckoutError.aspx* ] ページで、エラーが発生します。

1. という名前のページを開く*CheckoutError.aspx*で、*チェック アウト*フォルダーです。
2. 既存のマークアップを次のように置き換えます。   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx*チェック アウト プロセス中にエラーが発生したときにエラーの詳細にページが表示されます。

## <a name="running-the-application"></a>アプリケーションの実行

製品を購入する方法を説明するアプリケーションを実行します。 実行する、PayPal のテスト環境に注意してください。 実際のコストは交換されているされません。

1. 次のすべての Visual Studio で、ファイルが保存を確認します。
2. Web ブラウザーを開きに移動[ https://developer.paypal.com](https://developer.paypal.com/)です。
3. このチュートリアルで先ほど作成した、PayPal 開発者アカウントでログインします。  
   PayPal の開発者のサンド ボックスにログインする必要があります。 [ https://developer.paypal.com ](https://developer.paypal.com/) express のチェック アウトをテストします。 これは、テスト、PayPal の実稼働環境にない PayPal のサンド ボックスにのみ適用されます。
4. Visual Studio で、キーを押して**f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。  
   データベースが再構築後に、ブラウザーが開き、表示、 *Default.aspx*ページ。
5. 「自動車」など、製品カテゴリを選択し、をクリックして 3 つの異なる製品をショッピング カートに追加**をカートに追加**各製品の横にあります。  
   ショッピング カートでは、選択した製品を表示します。
6. クリックして、 **PayPal**チェック アウトするボタンをクリックします。 

    ![チェック アウトおよび PayPal のカートに支払い](checkout-and-payment-with-paypal/_static/image20.png)

   チェック アウトするには、Wingtip Toys サンプル アプリケーションのユーザー アカウントを持っている必要があります。
7. クリックして、 **Google**既存 gmail.com などの電子メール アカウントでログインするページの右上のリンク。  
   Gmail.com などのアカウントがあるない場合、テスト目的で 1 つを作成できます[www.gmail.com](https://www.gmail.com/)です。"Register"をクリックして、標準のローカル アカウントを使用することもできます。 

    ![チェック アウトおよび - PayPal の支払いにログインします。](checkout-and-payment-with-paypal/_static/image21.png)
8. Gmail アカウントとパスワードでサインインします。 

    ![チェック アウトと支払 PayPal - gmail などのサインイン](checkout-and-payment-with-paypal/_static/image22.png)
9. クリックして、**ログイン**Wingtip Toys サンプル アプリケーションのユーザー名、gmail アカウントを登録するにはボタン。 

    ![チェック アウトおよび PayPal のレジスタのアカウントに支払い](checkout-and-payment-with-paypal/_static/image23.png)
10. PayPal テスト サイトの追加、 **buyer**電子メール アドレス、このチュートリアルで先ほど作成したパスワードをクリックして、**ログで**ボタンをクリックします。 

    ![チェック アウトおよび PayPal のサインインの PayPal の支払い](checkout-and-payment-with-paypal/_static/image24.png)
11. PayPal ポリシーに同意し、をクリックして、**同意コンティニュ**ボタンをクリックします。  
    このページはのみ注には、この PayPal アカウントを使用する最初の時刻が表示されます。 もう一度テスト用のアカウントは、このことは、実際のコストは交換されませんに注意してください。 

    ![チェック アウトおよび PayPal の PayPal のポリシーでの支払い](checkout-and-payment-with-paypal/_static/image25.png)
12. テスト環境の確認] ページをクリック PayPal の注文情報を確認**続行**です。 

    ![チェック アウトと支払 PayPal - 情報の確認](checkout-and-payment-with-paypal/_static/image26.png)
13. *CheckoutReview.aspx* ] ページで注文量を確認して、生成された出荷先住所を表示します。 次に、をクリックして、**注文完了**ボタンをクリックします。 

    ![チェック アウトと支払 PayPal の注文の確認](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx**支払トランザクション ID を持つページが表示されます 

    ![チェック アウトおよび PayPal の完全なチェック アウトと支払い](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>データベースを確認します。

Wingtip Toys サンプル アプリケーションのデータベース内のデータ更新を確認すると、アプリケーションを実行した後、アプリケーションは、製品の注文書を正常に録画がわかります。

含まれるデータを検査することができます、 *Wingtiptoys.mdf*データベース ファイルを使用して、**データベース エクスプ ローラー**ウィンドウ (**サーバー エクスプ ローラー** Visual Studio のウィンドウで) 場合と同様このチュートリアルのシリーズ前半。

1. まだ開いている場合は、ブラウザー ウィンドウを閉じます。
2. Visual Studio で、選択、 **[すべてのファイル**の上部にあるアイコン**ソリューション エクスプ ローラー**を展開できるようにする、**アプリ\_データ**フォルダーです。
3. 展開して、**アプリ\_データ**フォルダーです。  
 選択する必要があります、 **[すべてのファイル**フォルダーのアイコン。
4. 右クリックし、 *Wingtiptoys.mdf*データベース ファイルと選択**開く**です。  
    **サーバー エクスプ ローラー**が表示されます。
5. 展開して、**テーブル**フォルダーです。
6. 右クリックし、 **Orders**テーブルを選択して**テーブル データの表示**です。  
 **Orders**テーブルが表示されます。
7. 確認、 **PaymentTransactionID**成功したトランザクションを確認する列。 

    ![チェック アウトおよび PayPal のレビュー データベースでの支払い](checkout-and-payment-with-paypal/_static/image29.png)
8. 閉じる、 **Orders**テーブル ウィンドウです。
9. サーバー エクスプ ローラーで右クリックし、 **OrderDetails**テーブルを選択して**テーブル データの表示**です。
10. 確認、`OrderId`と`Username`の値が、 **OrderDetails**テーブル。 これらの値に一致するメモ、`OrderId`と`Username`に含まれる値、 **Orders**テーブル。
11. 閉じる、 **OrderDetails**テーブル ウィンドウです。
12. Wingtip Toys のデータベース ファイルを右クリックして (*Wingtiptoys.mdf*) を選択して**接続を閉じる**です。
13. 表示されない場合、**ソリューション エクスプ ローラー**ウィンドウで、をクリックして**ソリューション エクスプ ローラー**の下部にある、**サーバー エクスプ ローラー**を表示するウィンドウ、**ソリューション エクスプ ローラー**もう一度です。

## <a name="summary"></a>まとめ

このチュートリアルでは、製品の注文書を追跡するためには、注文と注文詳細スキーマを追加します。 また、PayPal 機能は、Wingtip Toys のサンプル アプリケーションに統合されます。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET の構成の概要](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Azure App Service のメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET Web フォーム アプリケーションを展開します。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>免責情報

このチュートリアルには、サンプル コードが含まれています。 このようなサンプル コードには、どのような種類の保証も伴わず「現状有姿を示します。 同様に、Microsoft は、正確性、整合性、または、サンプル コードの品質保証されません。 うえで、サンプル コードを使用するものとします。 状況においても Microsoft 責任を負いかねますに任意の方法で、サンプル コードは、コンテンツなどですが、これらに、エラーまたは不作為サンプル コード、コンテンツ、または任意の損失や任意の種類のすべてのサンプル コードを使用した結果として発生する破損に限定されません。 無償通知が表示され、保存および免責 Microsoft から、すべてが失われる、信頼性情報の損失、負傷事故やの kind 含め、これらに限定して occasioned またはマテリアルを投稿することにより発生する破損の影響を与えません同意しないでください。送信使用するかに限らず見解をそこに依存します。

> [!div class="step-by-step"]
> [前へ](shopping-cart.md)
> [次へ](membership-and-administration.md)
