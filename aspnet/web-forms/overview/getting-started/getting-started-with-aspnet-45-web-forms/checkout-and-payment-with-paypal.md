---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: 精算と PayPal による支払い |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 3299da33a68f02ac1b3ffe7c037d06d8ece9455e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835632"
---
<a name="checkout-and-payment-with-paypal"></a>精算と PayPal による支払い
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。


このチュートリアルでは、Wingtip Toys のサンプル アプリケーションは、ユーザーの承認、登録、および PayPal を使用して支払いを変更する方法について説明します。 ログオンしているユーザーのみが製品を購入するための承認があります。 既に ASP.NET 4.5 Web フォーム プロジェクト テンプレートの組み込みのユーザーの登録機能にはでは必要なものの多くが含まれます。 PayPal Express のチェック アウト機能を追加します。 このチュートリアルで使用する、PayPal 開発者がテスト環境では、実際の資金は転送されません。 チュートリアルの最後に、ショッピング カート、チェック アウト ボタンをクリックして転送するデータ、PayPal テスト web サイトに追加する製品を選択して、アプリケーションをテストします。 PayPal テスト web サイトでは、配布と支払い情報を確認しを確認し、購入を完了しますローカル Wingtip Toys のサンプル アプリケーションに戻りますされます。

そのアドレスのスケーラビリティとセキュリティ、オンライン ショッピングに特化したいくつかのサード パーティの支払いを経験豊富なプロセッサがあります。 ASP.NET 開発者は、ショッピングを実装して、ソリューションを購入する前に、サード パーティの支払いソリューションを利用するメリットを検討してください。

> [!NOTE] 
> 
> Wingtip Toys のサンプル アプリケーションは、ASP.NET web 開発者に特定の ASP.NET の概念と使用可能な機能を示すために設計されました。 このサンプル アプリケーションは、スケーラビリティとセキュリティに関してどのような状況は最適化されていませんでした。


## <a name="what-youll-learn"></a>学習内容。

- フォルダー内の特定のページへのアクセスを制限する方法。
- 匿名のショッピング カートから既知のショッピング カートを作成する方法。
- プロジェクトの SSL を有効にする方法。
- プロジェクトに、OAuth プロバイダーを追加する方法。
- PayPal を使用して、PayPal のテスト環境を使用して製品を購入する方法。
- あれば PayPal から詳細を表示する方法、 **DetailsView**コントロール。
- PayPal から取得の詳細と、Wingtip Toys アプリケーションのデータベースを更新する方法。

## <a name="adding-order-tracking"></a>注文の追跡を追加します。

このチュートリアルでは、ユーザーが作成された注文からデータを追跡するために 2 つの新しいクラスを作成します。 クラスでは、発送情報、発注の合計、および支払いの確認に関するデータを追跡します。

### <a name="add-the-order-and-orderdetail-model-classes"></a>順序と OrderDetail モデル クラスを追加します。

このチュートリアルのシリーズの前半では、カテゴリ、製品のスキーマを定義し、ショッピング カートのアイテムを作成して、 `Category`、 `Product`、および`CartItem`クラス、*モデル*フォルダー。 製品の注文や注文の詳細については、スキーマを定義する 2 つの新しいクラスを追加します。

1. **モデル**フォルダー、という名前の新しいクラスを追加*Order.cs*します。   
   新しいクラス ファイルがエディターで表示されます。
2. 次のように、既定のコードに置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. 追加、 *OrderDetail.cs*クラスを*モデル*フォルダー。
4. 既定のコードを次のコードに置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order`と`OrderDetail`クラスは、購入および出荷するため、注文情報を定義するスキーマを含めることができます。

さらに、エンティティ クラスは、管理し、データベースへのデータ アクセスを提供する、データベース コンテキスト クラスを更新する必要があります。 これを行うには、新しく作成された注文を追加して`OrderDetail`モデル クラスを`ProductContext`クラス。

1. **ソリューション エクスプ ローラー**を検索して開く、 *ProductContext.cs*ファイル。
2. 強調表示されたコードを追加、 *ProductContext.cs*次に示すようにファイルします。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

コードで、このチュートリアル シリーズで説明したよう、 *ProductContext.cs*ファイルを追加、`System.Data.Entity`名前空間、Entity Framework のすべてのコア機能にアクセスするようにします。 この機能には、クエリ、挿入、更新、および厳密に型指定されたオブジェクトを使用してデータを削除する機能が含まれています。 上記のコードで、`ProductContext`クラスを新しく追加した Entity Framework のアクセス権を追加します`Order`と`OrderDetail`クラス。

## <a name="adding-checkout-access"></a>チェック アウトのアクセスを追加します。

Wingtip Toys のサンプル アプリケーションは、匿名ユーザーをレビューして、ショッピング カートに製品を追加できます。 ただし、匿名ユーザーが選択したショッピング カートに追加された、製品を購入するときに、必要がありますログオン サイトにします。 ログオンするがチェック アウトを処理し、購入のプロセスは、Web アプリケーションの制限されたページにアクセスできます。 これらの制限付きのページに含まれる、*チェック アウト*アプリケーションのフォルダー。

### <a name="add-a-checkout-folder-and-pages"></a>チェック アウト フォルダーとページを追加します。

作成、*チェック アウト*フォルダーとがチェック アウト プロセス中に、顧客に表示されるページ。 このチュートリアルの後半でこれらのページが更新されます。

1. プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**新しいフォルダーを追加**します。 

    ![精算と PayPal の新しいフォルダーによる支払い](checkout-and-payment-with-paypal/_static/image1.png)
2. 新しいフォルダーの名前*チェック アウト*します。
3. 右クリックし、*チェック アウト*クリックしてフォルダー**追加**-&gt;**新しい項目の**します。 

    ![精算と PayPal - 新しい項目による支払い](checkout-and-payment-with-paypal/_static/image2.png)
4. **[新しい項目の追加]** ダイアログ ボックスが表示されます。
5. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレート グループ。 次に、中央のウィンドウから次のように選択します。**マスター ページを使用した Web フォーム**名前を付けます*CheckoutStart.aspx*します。 

    ![精算と PayPal - による支払いが新しい項目 ダイアログ ボックスを追加します。](checkout-and-payment-with-paypal/_static/image3.png)
6. 同様に、選択、 *Site.Master*のマスター ページとしてファイル。
7. 次の追加ページを追加、*チェック アウト*前述の手順を使用してフォルダー。   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Web.config ファイルを追加します。

新しいを追加して*Web.config*ファイルを*チェック アウト*フォルダー、ことができます、フォルダーに含まれるすべてのページにアクセスを制限します。

1. 右クリックし、*チェック アウト*フォルダーと選択**追加** - &gt; **新しい項目の**します。  
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレート グループ。 次に、中央のウィンドウから次のように選択します。 **Web 構成ファイル**、の既定の名前をそのまま使用*Web.config*、し、**追加**します。
3. 既存の XML コンテンツを置き換える、 *Web.config*を次のファイル。  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. 保存、 *Web.config*ファイル。

*Web.config*ファイルでは、Web アプリケーションのすべての不明なユーザーにする必要がありますに含まれるページへのアクセスが拒否されることを指定します、*チェック アウト*フォルダー。 ただし場合は、ユーザーは、アカウントが登録され、ログオンしている、既知のユーザーになります。 また、があります内のページへのアクセス、*チェック アウト*フォルダー。

ASP.NET の構成が、階層構造に従うことを確認することが重要で各*Web.config*ファイル内にあるフォルダーとその下の子ディレクトリのすべての構成設定が適用されます。

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>プロジェクトの SSL を有効にします。

 セキュリティで保護された Sockets Layer (SSL) は、Web サーバーと Web クライアントが暗号化を使用してより安全に通信できるように定義されているプロトコルです。 SSL を使用しない場合、クライアントとサーバー間で送信されるデータは、ネットワークに物理的にアクセスできるすべてのユーザーによるパケット スニッフィングを開いています。 また、いくつかの一般的な認証方式、プレーンな HTTP 経由でセキュリティで保護されたもなりました。 具体的には、基本認証とフォーム認証、暗号化されていない資格情報を送信します。 セキュリティで保護するには、これらの認証方式は、SSL を使用する必要があります。 

1. **ソリューション エクスプ ローラー**、 をクリックして、 **WingtipToys**プロジェクト、キーを押して**F4**を表示する、**プロパティ**ウィンドウ。
2. 変更**SSL が有効になっている**に`true`します。
3. コピー、 **SSL URL**後で使用できるようにします。   
 SSL url は、 `https://localhost:44300/` (下図参照) と SSL Web サイトを以前作成した場合を除き、します。   
    ![プロジェクト プロパティ](checkout-and-payment-with-paypal/_static/image4.png)
4. **ソリューション エクスプ ローラー**を右クリックして、 **WingtipToys**プロジェクトし、クリックして**プロパティ**。
5. 左側のタブで次のようにクリックします。 **Web**します。
6. 変更、**プロジェクト Url**を使用する、 **SSL URL**以前に保存します。   
    ![プロジェクトの Web プロパティ](checkout-and-payment-with-paypal/_static/image5.png)
7. キーを押してページを保存**CTRL + S**します。
8. **Ctrl キーを押しながら F5 キーを押して**アプリケーションを実行します。 Visual Studio の SSL の警告を回避できるようにするオプションが表示されます。
9. をクリックして**はい**IIS Express SSL 証明書を信頼して続行します。   
    ![IIS Express SSL 証明書の詳細](checkout-and-payment-with-paypal/_static/image6.png)  
 セキュリティの警告が表示されます。
10. クリックして**はい**localhost 証明書をインストールします。   
    ![セキュリティ警告 ダイアログ ボックス](checkout-and-payment-with-paypal/_static/image7.png)  
 ブラウザー ウィンドウが表示されます。

これで SSL を使用してローカルで Web アプリケーションを簡単にテストできます。

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>OAuth 2.0 プロバイダーを追加します。

ASP.NET Web フォームでは、メンバーシップと認証のオプションの強化を提供します。 これらの拡張機能には、OAuth が含まれます。 OAuth は、web、モバイル、およびデスクトップ アプリケーションからシンプルで標準的な方法で安全に認証を許可するオープン プロトコルです。 ASP.NET Web フォーム テンプレートは OAuth を使用して、認証プロバイダーとして Facebook、Twitter、Google、Microsoft を公開します。 このチュートリアルでは、認証プロバイダーとして Google のみを使用して、任意のプロバイダーを使用するコードを簡単に変更できます。 その他のプロバイダーを実装する手順は、このチュートリアルに表示される手順とよく似ています。

認証に加えて、このチュートリアルはロールを使用して承認を実装します。 追加したユーザーのみ、`canEdit`ロールは、データを変更することになります (作成、編集、または連絡先を削除) します。

> [!NOTE] 
> 
> Windows Live アプリケーションでは、ログインをテストするため、ローカルの web サイトの URL を使用することはできませんので作業用の web サイトでライブ URL のみ受け入れます。


次の手順では Google の認証プロバイダーを追加することを許可します。

1. 開く、*アプリ\_Start\Startup.Auth.cs*ファイル。
2. コメント文字を削除、`app.UseGoogleAuthentication()`メソッドとして、メソッドが表示されるように従います。 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. 移動し、 [Google Developers Console](https://console.developers.google.com/)します。 また、Google デベロッパーの電子メール アカウント (gmail.com) でサインインする必要があります。 Google アカウントがいない場合は、選択、**アカウントを作成する**リンク。   
   次に、表示されます、 **Google Developers Console**します。   
    ![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)
4. をクリックして、**プロジェクトの作成**ボタンをクリックし、プロジェクトの名前と ID (既定値を使用することができます) を入力します。 をクリックし、**契約のチェック ボックス**と**作成**ボタンをクリックします。  

    ![Google - 新しいプロジェクト](checkout-and-payment-with-paypal/_static/image9.png)

   数秒で新しいプロジェクトを作成して、新しいプロジェクトのページがブラウザーに表示されます。
5. 左側のタブで次のようにクリックします。 **Api &amp; auth**、 をクリックし、**資格情報**します。
6. をクリックして、**新しいクライアント ID の作成** **OAuth**します。   
   **Create Client ID**ダイアログが表示されます。   
    ![Google - クライアント ID の作成](checkout-and-payment-with-paypal/_static/image10.png)
7. **Create Client ID**ダイアログ ボックスで、既定値を保持**Web アプリケーション**アプリケーションの種類。
8. 設定、**承認済みの JavaScript 生成元**このチュートリアルで先ほど使用した SSL URL を (`https://localhost:44300/`した SSL の他のプロジェクトを作成していない場合)。   
   この URL は、アプリケーションの原点です。 このサンプルでは、localhost の URL のテストをのみ入力されます。 ただし、localhost と運用環境に対応する複数の Url を入力することができます。
9. 設定、 **Authorized Redirect URI**次。 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   この値は URI その ASP.NET OAuth ユーザーが google OAuth サーバーと通信します。 上記で使用する SSL URL に注意してください (`https://localhost:44300/`した SSL の他のプロジェクトを作成していない場合)。
10. をクリックして、 **Create Client ID**ボタンをクリックします。
11. Google 開発者コンソールの左側のメニューでをクリックして、**同意画面**メニュー項目は、電子メール アドレスと製品名を設定します。 フォームを完了すると、クリックして**保存**します。
12. をクリックして、 **Api**メニュー項目、下にスクロールし、**オフ**横に**Google + API**します。   
    このオプションを受け入れると、Google + API が有効になります。
13. 更新することも必要があります、 **Microsoft.Owin**バージョン 3.0.0 への NuGet パッケージ。   
    **ツール**メニューの  **NuGet パッケージ マネージャー**選び**ソリューションの NuGet パッケージの管理**します。  
    **NuGet パッケージの管理**ウィンドウで検索し、更新、 **Microsoft.Owin**バージョン 3.0.0 へのパッケージ。
14. Visual Studio で、更新、`UseGoogleAuthentication`のメソッド、 *Startup.Auth.cs*コピー アンド ペーストでページ、**クライアント ID**と**クライアント シークレット**メソッドにします。 **クライアント ID**と**クライアント シークレット**の下に表示される値のサンプルし、は機能しません。 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. キーを押して**CTRL + F5**をビルドして、アプリケーションを実行します。 をクリックして、**ログイン**リンク。
16. [**別のサービスを使用してログイン**、] をクリックして**Google**します。  
    ![ログイン](checkout-and-payment-with-paypal/_static/image11.png)
17. 資格情報を入力する必要がある場合、資格情報を入力する google サイトにリダイレクトされます。  
    ![Google - サインイン](checkout-and-payment-with-paypal/_static/image12.png)
18. 資格情報を入力した後は、作成した web アプリケーションへのアクセス許可を与える促されます。  
    ![プロジェクトの既定のサービス アカウント](checkout-and-payment-with-paypal/_static/image13.png)
19. クリックして**受け入れる**します。 今すぐにリダイレクトされます、**登録**のページ、 **WingtipToys**アプリケーションが Google アカウントを登録することができます。  
    ![Google アカウントの登録します。](checkout-and-payment-with-paypal/_static/image14.png)
20. Gmail アカウントに使用されるローカルの電子メール登録名を変更するオプションがありますが、通常、既定の電子メール エイリアス (認証に使用するもの) を保持したいです。 クリックして**ログイン**上記のようです。

### <a name="modifying-login-functionality"></a>ログイン機能の変更

このチュートリアル シリーズで説明したとおりは以前、ユーザーの登録機能の多くが含まれています、ASP.NET Web フォーム テンプレートで既定では。 これは、既定値を変更*Login.aspx*と*Register.aspx*を呼び出すページ、`MigrateCart`メソッド。 `MigrateCart`メソッドは、匿名のショッピング カートに新しくログインしているユーザーを関連付けます。 ユーザーを関連付けると、ショッピング カート、Wingtip Toys のサンプル アプリケーションは訪問者の間でユーザーのショッピング カートを維持することになります。

1. **ソリューション エクスプ ローラー**を検索して開く、*アカウント*フォルダー。
2. という名前の分離コード ページを変更*Login.aspx.cs*を次のように表示されるように、黄色で強調表示されているコードを含めます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. 保存、 *Login.aspx.cs*ファイル。

ここでは、警告の定義が存在しないことを無視することができます、`MigrateCart`メソッド。 このチュートリアルでは、少し後でそれを追加する予定です。

*Login.aspx.cs*分離コード ファイルには、LogIn メソッドがサポートしています。 Login.aspx ページを調べることでこのページに ログイン ボタンが含まれているを確認しますトリガー をクリックすると、`LogIn`分離コードでハンドラー。

ときに、`Login`メソッドを*Login.aspx.cs*を呼び出すと、ショッピング カートという名前の新しいインスタンス`usersShoppingCart`が作成されます。 ショッピング カート (GUID) の ID が取得され、設定、`cartId`変数。 次に、`MigrateCart`メソッドが呼び出され、どちらも、`cartId`と、この方法では、ログインのユーザーの名前。 ショッピング カートが移行されると、匿名ショッピング カートを識別するために使用される GUID は、ユーザー名に置き換えられます。

変更するだけでなく、 *Login.aspx.cs* 、ユーザーがログインすると、ショッピング カートを移行する分離コード ファイルも変更する必要があります、 *Register.aspx.cs 分離コード ファイル*ショッピング カートを移行するにはときに、ユーザーは新しいアカウントを作成しログに記録します。

1. *アカウント*フォルダーで、という名前の分離コード ファイルを開く*Register.aspx.cs*します。
2. 次のように表示されるように、黄色でコードを含めることによって分離コード ファイルを変更します。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. 保存、 *Register.aspx.cs*ファイル。 もう一度、に関する警告は無視、`MigrateCart`メソッド。

使用したコードに注目してください、`CreateUser_Click`イベント ハンドラーで使用したコードとよく似ていますが、`LogIn`メソッド。 ユーザーが登録またはへの呼び出し、サイトにログイン、`MigrateCart`メソッドになります。

## <a name="migrating-the-shopping-cart"></a>ショッピング カートを移行します。

使用してショッピング カートを移行するコードを追加するには、ログインと登録プロセスを更新したら、`MigrateCart`メソッド。

1. **ソリューション エクスプ ローラー**、検索、*ロジック*フォルダーとオープン、 *ShoppingCartActions.cs*クラス ファイル。
2. 既存のコードに黄色で強調表示されているコードを追加、 *ShoppingCartActions.cs*ファイル、ようにコードでは、 *ShoppingCartActions.cs*ファイルが次のように表示されます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart`メソッドでは、既存の cartId を使用して、ユーザーのショッピング カートを検索します。 コードが次に、ショッピング カートのアイテムをループ処理し、置換、`CartId`プロパティ (で指定された、`CartItem`スキーマ) では、ログイン ユーザー名。

### <a name="updating-the-database-connection"></a>データベース接続を更新しています

このチュートリアルを使用している場合、**事前構築済み**Wingtip Toys のサンプル アプリケーションでは、既定のメンバーシップ データベースを再作成する必要があります。 既定の接続文字列を変更すると、メンバーシップ データベースが作成されます、次回、アプリケーションが実行されます。

1. 開く、 *Web.config*プロジェクトのルートにあるファイル。
2. 次のように表示されるように、既定の接続文字列を更新します。   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>PayPal の統合

PayPal は、オンライン ショップで支払いを受け付ける web ベースの課金プラットフォームです。 このチュートリアルは、次に、PayPal の Express のチェック アウト機能をアプリケーションに統合する方法を説明します。 Express のチェック アウトにより、顧客が買い物カゴに追加した項目の料金を支払う PayPal を使用します。

### <a name="create-paylpal-test-accounts"></a>PaylPal テスト アカウントを作成します。

PayPal のテスト環境を使用する必要があるには、作成し、開発者のテスト アカウントを確認します。 開発者のテスト アカウントを使用して、テスト アカウントと販売者のテスト アカウントに、購入者を作成します。 開発者、テスト アカウントの資格情報も PayPal のテスト環境にアクセスする Wingtip Toys のサンプル アプリケーションが、できます。

1. ブラウザーでは、サイトのテスト、PayPal 開発者に移動します。   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. PayPal の開発者アカウントを持っていない場合は、クリックして新しいアカウントを作成**サインアップ**サインアップ手順に従います。 既存の PayPal の開発者アカウントがあればがクリックしてサインイン**ログで**します。 PayPal 開発者アカウントをこのチュートリアルの後半で、Wingtip Toys のサンプル アプリケーションをテストする必要があります。
3. 場合は、PayPal の開発者アカウントにサインアップしただけでは、PayPal、PayPal 開発者アカウントを確認する必要があります。 電子メール アカウントに送信される PayPal の手順に従って、アカウントを確認できます。 PayPal 開発者アカウントを確認した後は、サイトのテスト、PayPal 開発者に再度ログインします。
4. 後で、PayPal 開発者アカウントがまだない場合は、PayPal buyer テスト アカウントを作成する必要がある、PayPal の開発者向けサイトにログインしているいずれかであります。 PayPal の サイト をクリックで、buyer テスト アカウントを作成する、**アプリケーション** タブをクリックして**サンド ボックス アカウント**します。   
 **サンド ボックス テスト アカウント**ページが表示されます。   

    > [!NOTE] 
    > 
    > PayPal の開発者向けサイトには、テストのマーチャント アカウントが既に用意されています。

    ![精算と PayPal - サンド ボックス テスト アカウントによる支払い](checkout-and-payment-with-paypal/_static/image15.png)
5. サンド ボックス テスト アカウント ページで、次のようにクリックします。**アカウントの作成**です。
6. **テスト アカウントの作成**ページは、テスト アカウントの電子メール アドレスと、任意のパスワードに、購入者を選択します。   

    > [!NOTE] 
    > 
    > 購入者の電子メール アドレスとパスワードをこのチュートリアルの最後に、Wingtip Toys のサンプル アプリケーションをテストする必要があります。

    ![精算と PayPal - サンド ボックス テスト アカウントによる支払い](checkout-and-payment-with-paypal/_static/image16.png)
7. クリックして、購入者のテスト アカウントを作成、**アカウントの作成**ボタンをクリックします。  
 **サンド ボックス テスト アカウント**ページが表示されます。 

    ![精算と PayPal - PaylPal アカウントによる支払い](checkout-and-payment-with-paypal/_static/image17.png)
8. **サンド ボックス テスト アカウント** ページで、をクリックして、**進行**電子メール アカウント。  
    **プロファイル**と**通知**オプションが表示されます。
9. 選択、**プロファイル** をクリックし、 **API 資格情報**マーチャント テスト アカウントの資格情報、API を表示します。
10. API のテストの資格情報をメモ帳にコピーします。

表示されているクラシック API のテスト資格情報 (ユーザー名、パスワード、および署名)、Wingtip Toys のサンプル アプリケーションの API 呼び出しを行うには、PayPal の環境をテストする必要があります。 次の手順では、資格情報を追加します。

### <a name="add-paypal-class-and-api-credentials"></a>PayPal クラスと API の資格情報を追加します。

1 つのクラスに PayPal コードの大部分が配置されます。 このクラスには、PayPal との通信に使用するメソッドが含まれています。 また、このクラスには、PayPal の資格情報を追加します。

1. Visual Studio 内で Wingtip Toys のサンプル アプリケーションを右クリックし、**ロジック**フォルダーと、選択**追加** - &gt; **新しい項目の**します。   
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. **Visual c#** から、**インストール済み**、左側のペイン**コード**します。
3. 中央のウィンドウから次のように選択します。**クラス**します。 この新しいクラスの名前を**PayPalFunctions.cs**します。
4. **[追加]** をクリックします。  
   新しいクラス ファイルがエディターで表示されます。
5. 既定のコードを次のコードに置き換えます。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. マーチャント API 資格情報 (ユーザー名、パスワード、および署名) できるように、PayPal のテスト環境への関数呼び出しを行うことができます、このチュートリアルで先ほど表示したを追加します。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> このサンプル アプリケーションで c# ファイル (.cs) に資格情報を追加するだけです。 ただし、実装済みのソリューションで、構成ファイルで、資格情報の暗号化を検討してください。


NVPAPICaller クラスには、PayPal の機能の大半が含まれています。 クラスのコードでは、購入、PayPal のテスト環境からテストの作成に必要なメソッドを提供します。 次の 3 つの PayPal 関数は、購入のために使用されます。

- `SetExpressCheckout` 関数
- `GetExpressCheckoutDetails` 関数
- `DoExpressCheckoutPayment` 関数

`ShortcutExpressCheckout`メソッドは、ショッピング カートと呼び出しからテストの購入情報と製品の詳細を収集、 `SetExpressCheckout` PayPal 関数。 `GetCheckoutDetails`メソッドは、購入の詳細と呼び出しを確認します。、`GetExpressCheckoutDetails`テスト購入を行う前に、PayPal 関数。 `DoCheckoutPayment`メソッドが呼び出すことにより、テスト環境からテストの購入を完了、 `DoExpressCheckoutPayment` PayPal 関数。 残りのコードでは、PayPal メソッドと文字列をエンコード、文字列をデコード、配列、処理、および資格情報を決定するなどのプロセスをサポートします。

> [!NOTE] 
> 
> PayPal を使用すると、に基づいて省略可能な購入の詳細を含む[PayPal の API 仕様](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)します。 Wingtip Toys のサンプル アプリケーションでコードを拡張するには、ローカライズの詳細、製品の説明、税金、顧客サービスの数、だけでなく他の多くの省略可能なフィールドを含めることができます。


注意して、戻り値と [キャンセル] Url で指定されている、 **ShortcutExpressCheckout**メソッドは、ポート番号を使用します。

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Visual Web Developer は、SSL を使用して web プロジェクトを実行するとよくポート 44300 が web サーバーの使用されます。 上記のように、ポート番号は 44300 が。 アプリケーションを実行するときに、別のポート番号を表示できます。 ポート番号のニーズを正しく設定するコードを行えるように成功は、このチュートリアルの最後に、Wingtip Toys のサンプル アプリケーションを実行します。 このチュートリアルの次のセクションでは、ローカル ホストのポート番号を取得して、PayPal クラスを更新する方法について説明します。

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>PayPal クラスでは、LocalHost のポート番号を更新します。

Wingtip Toys のサンプル アプリケーションは、PayPal テスト サイトに移動し、Wingtip Toys のサンプル アプリケーションのローカル インスタンスを返すことで、製品を購入します。 PayPal の正しい URL を返すためには、ローカルで実行中のポート番号を指定する必要があります。 上記で説明した PayPal コードでのアプリケーションのサンプルです。

1. プロジェクト名を右クリックして (**WingtipToys**) で**ソリューション エクスプ ローラー**選択**プロパティ**します。
2. 左の列を選択、 **Web**タブ。
3. ポート番号を取得、**プロジェクト Url**ボックス。
4. 必要な場合、更新、`returnURL`と`cancelURL`PayPal クラスで (`NVPAPICaller`) で、 *PayPalFunctions.cs* web アプリケーションのポート番号を使用するファイル。   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

ここで追加したコードでは、ローカル Web アプリケーションの必要なポートが一致します。 PayPal はローカル コンピューターに、正しい URL に返すことになります。

### <a name="add-the-paypal-checkout-button"></a>PayPal のチェック アウト ボタンを追加します。

プライマリの PayPal 関数は、サンプル アプリケーションに追加されましたが、これで、マークアップとコードをこれらの関数を呼び出すために必要な追加を開始できます。 最初に、ショッピング カート ページで、ユーザーに表示されるチェック アウト ボタンを追加する必要があります。

1. 開く、*後*ファイル。
2. ファイルの一番下までスクロールし、検索、`<!--Checkout Placeholder -->`コメント。
3. コメントを置き換える、`ImageButton`コントロール マークアップは次のように置き換えられます。  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. *ShoppingCart.aspx.cs*した後、ファイル、 `UpdateBtn_Click` 、ファイルの末尾近くのイベント ハンドラーを追加、`CheckOutBtn_Click`イベント ハンドラー。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. さらに、 *ShoppingCart.aspx.cs*ファイルへの参照を追加、`CheckoutBtn`新しいイメージ ボタンは次のように参照されているため。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. 両方に変更を保存、*後*ファイルと*ShoppingCart.aspx.cs*ファイル。
7. メニューから、次のように選択します。**デバッグ**-&gt;**ビルド WingtipToys**します。  
   プロジェクトが再構築で新しく追加された**ImageButton**コントロール。

### <a name="send-purchase-details-to-paypal"></a>PayPal を購入の詳細情報を送信します。

ユーザーがクリックすると、**チェック アウト**ショッピング カート ページのボタン (*後*)、発注プロセスを始めます。 次のコードでは、製品を購入するために必要な最初の PayPal 関数を呼び出します。

1. *チェック アウト*フォルダーで、という名前の分離コード ファイルを開く*CheckoutStart.aspx.cs*します。   
   必ず、分離コード ファイルを開きます。
2. 既存のコードを次のコードに置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

アプリケーションのユーザーがクリックすると、**チェック アウト**ショッピング カート ページで、ブラウザーのボタンに移動、 *CheckoutStart.aspx*ページ。 ときに、 *CheckoutStart.aspx*ページの読み込み、`ShortcutExpressCheckout`メソッドが呼び出されます。 この時点では、ユーザーは、PayPal テスト web サイトに転送されます。 PayPal のサイトでユーザー PayPal 資格情報を入力、購入の詳細を確認、PayPal 契約を受け取り、Wingtip Toys のサンプル アプリケーションに返します場所、`ShortcutExpressCheckout`メソッドが完了するとします。 ときに、`ShortcutExpressCheckout`メソッドが完了したら、ユーザーがリダイレクトされる、 *CheckoutReview.aspx*で指定されたページ、`ShortcutExpressCheckout`メソッド。 これにより、ユーザーは、Wingtip Toys のサンプル アプリケーション内で注文の詳細を確認できます。

### <a name="review-order-details"></a>注文の詳細を確認してください。

PayPal から返された後、 *CheckoutReview.aspx* Wingtip Toys のサンプル アプリケーションのページには、注文の詳細が表示されます。 このページは、製品を購入する前に、注文の詳細を確認できます。 *CheckoutReview.aspx*ページを次のように作成する必要があります。

1. *チェック アウト*フォルダーで、という名前のページを開く*CheckoutReview.aspx*します。
2. 次のように既存のマークアップに置き換えます。   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. という名前の分離コード ページを開く*CheckoutReview.aspx.cs*次の既存のコードを置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**DetailsView** PayPal から返された注文の詳細を表示するコントロールを使用します。 上記のコードが、注文の詳細として Wingtip Toys データベースに保存することも、`OrderDetail`オブジェクト。 ユーザーがクリックしたときに、**注文完了**ボタンにリダイレクトされます、 *CheckoutComplete.aspx*ページ。

> [!NOTE] 
> 
> **ヒント。**
> 
> マークアップで、 *CheckoutReview.aspx*  ページで、注意、`<ItemStyle>`タグを使用中の項目のスタイルを変更して、 **DetailsView**ページの下部にあるコントロール。 内のページを表示して**デザイン ビュー** (を選択して**デザイン**Visual Studio の左上隅にある)、選択し、 **DetailsView**を制御して、を選択します。**スマート タグ**(上部にある矢印アイコン コントロールの右) を表示することができます、 **DetailsView タスク**します。
> 
> ![精算と PayPal - による支払いのフィールドを編集します。](checkout-and-payment-with-paypal/_static/image18.png)
> 
> 選択して**フィールドの編集**、**フィールド** ダイアログ ボックスが表示されます。 このダイアログ ボックスで簡単に制御できますビジュアルのプロパティなど**後**の**DetailsView**コントロール。
> 
> ![精算と PayPal - フィールド ダイアログ ボックスでの支払い](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>購入を完了します。

*CheckoutComplete.aspx*ページでは、PayPal から購入します。 前述のように、ユーザーをクリックする必要があります、**注文完了**ボタン、アプリケーションが移動する前に、 *CheckoutComplete.aspx*ページ。

1. *チェック アウト*フォルダーで、という名前のページを開く*CheckoutComplete.aspx*します。
2. 次のように既存のマークアップに置き換えます。   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. という名前の分離コード ページを開く*CheckoutComplete.aspx.cs*次の既存のコードを置き換えます。   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

ときに、 *CheckoutComplete.aspx*ページが読み込まれる、`DoCheckoutPayment`メソッドが呼び出されます。 前に説明したように、`DoCheckoutPayment`メソッドは、PayPal のテスト環境からの購入を完了します。 PayPal の注文の購入が完了すると、 *CheckoutComplete.aspx*ページには、支払取引が表示されます。`ID`購入者にします。

### <a name="handle-cancel-purchase"></a>購入をキャンセルを処理します。

ユーザー、購入をキャンセルする場合、それらが表示されます、 *CheckoutCancel.aspx*ページの場所の順序が取り消されましたが表示されます。

1. という名前のページを開く*CheckoutCancel.aspx*で、*チェック アウト*フォルダー。
2. 次のように既存のマークアップに置き換えます。   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>購入のエラーの処理

によって、購入プロセス中にエラーが処理される、 *CheckoutError.aspx*ページ。 分離コード、 *CheckoutStart.aspx*  ページで、 *CheckoutReview.aspx*  ページで、および*CheckoutComplete.aspx*ページごとにリダイレクトされます、 *CheckoutError.aspx*ページ エラーが発生した場合。

1. という名前のページを開く*CheckoutError.aspx*で、*チェック アウト*フォルダー。
2. 次のように既存のマークアップに置き換えます。   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx*チェック アウト プロセス中にエラーが発生するとエラーの詳細ページが表示されます。

## <a name="running-the-application"></a>アプリケーションの実行

製品を購入する方法について、アプリケーションを実行します。 実行する、PayPal のテスト環境に注意してください。 交換される実際のコストはありません。

1. 次のすべての Visual Studio で、ファイルが保存を確認します。
2. Web ブラウザーを開きに移動します[ https://developer.paypal.com](https://developer.paypal.com/)します。
3. このチュートリアルで先ほど作成した、PayPal 開発者アカウントでログインします。  
   PayPal の開発者のサンド ボックスでログに記録する必要があります。 [ https://developer.paypal.com ](https://developer.paypal.com/) express のチェック アウトをテストします。 これは、テスト、PayPal の実際の環境が PayPal のサンド ボックスにのみ適用されます。
4. Visual Studio で、キーを押して**F5** Wingtip Toys のサンプル アプリケーションを実行します。  
   データベースが再構築した後、ブラウザーが開き、表示、 *Default.aspx*ページ。
5. クリックして、"Cars"など、製品カテゴリを選択すると、ショッピング カートに 3 つの異なる製品を追加**カートに追加**各製品の横にあります。  
   選択した製品は、ショッピング カートが表示されます。
6. をクリックして、 **PayPal**チェック アウトするボタンをクリックします。 

    ![精算と PayPal - カートによる支払い](checkout-and-payment-with-paypal/_static/image20.png)

   チェック アウトすると、Wingtip Toys のサンプル アプリケーションのユーザー アカウントを持っている必要があります。
7. をクリックして、 **Google**既存 gmail.com 電子メール アカウントでログインするページの右上のリンク。  
   Gmail.com アカウントがいない場合、テストの目的で 1 つを作成できます[www.gmail.com](https://www.gmail.com/)します。 登録する をクリックして、標準のローカル アカウントを使用することもできます。 

    ![チェック アウトと - PayPal による支払いログインします。](checkout-and-payment-with-paypal/_static/image21.png)
8. Gmail アカウントとパスワードでサインインします。 

    ![精算と PayPal - Gmail サインインによる支払い](checkout-and-payment-with-paypal/_static/image22.png)
9. をクリックして、**ログイン**を Wingtip Toys のサンプル アプリケーションのユーザー名、gmail アカウントを登録するボタンをクリックします。 

    ![精算と PayPal のアカウントの登録による支払い](checkout-and-payment-with-paypal/_static/image23.png)
10. PayPal テスト サイトで追加、 **buyer**電子メール アドレスと、このチュートリアルで先ほど作成したパスワードをクリック、**ログで**ボタンをクリックします。 

    ![精算と PayPal - サインイン PayPal による支払い](checkout-and-payment-with-paypal/_static/image24.png)
11. PayPal ポリシーに同意し、をクリックして、**同意コンティニュ**ボタンをクリックします。  
    このページはのみに注意してくださいでは、この PayPal アカウントを使用して最初に表示されます。 これは、テスト アカウントを実際のコストは交換されませんを再度確認します。 

    ![精算と PayPal - ポリシーの PayPal による支払い](checkout-and-payment-with-paypal/_static/image25.png)
12. テスト環境の確認 ページとクリック PayPal で注文情報を確認**続行**します。 

    ![精算と PayPal - 情報の確認による支払い](checkout-and-payment-with-paypal/_static/image26.png)
13. *CheckoutReview.aspx*ページで、注文の合計を確認し、生成された出荷先住所を表示します。 をクリックし、**注文完了**ボタンをクリックします。 

    ![精算と PayPal の注文の確認による支払い](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx**支払いトランザクションの ID を持つページが表示されます 

    ![精算と PayPal のチェック アウトの完了による支払い](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>データベースを確認します。

Wingtip Toys のサンプル アプリケーションのデータベース内のデータの更新を確認すると、アプリケーションを実行した後、アプリケーションが正常に製品の購入を記録することがわかります。

含まれるデータを検査することができます、 *Wingtiptoys.mdf*データベース ファイルを使用して、**データベース エクスプ ローラー**ウィンドウ (**サーバー エクスプ ローラー** Visual Studio のウィンドウ) と同様このチュートリアルのシリーズで前。

1. まだ開いている場合は、ブラウザー ウィンドウを閉じます。
2. Visual Studio で、選択、 **すべてのファイル**の上部にあるアイコン**ソリューション エクスプ ローラー**展開できるように、**アプリ\_データ**フォルダー。
3. 展開、**アプリ\_データ**フォルダー。  
 選択する必要があります、 **すべてのファイル**フォルダーのアイコン。
4. 右クリックし、 *Wingtiptoys.mdf*データベース ファイルと選択**オープン**します。  
    **サーバー エクスプ ローラー**が表示されます。
5. 展開、**テーブル**フォルダー。
6. 右クリックし、**注文**テーブルを選択**テーブル データの表示**します。  
 **注文**テーブルが表示されます。
7. レビュー、 **PaymentTransactionID**列を成功したトランザクションを確認します。 

    ![精算と PayPal - レビュー データベースによる支払い](checkout-and-payment-with-paypal/_static/image29.png)
8. 閉じる、**注文**テーブル ウィンドウ。
9. サーバー エクスプ ローラーで右クリックし、 **OrderDetails**テーブルを選択**テーブル データの表示**します。
10. レビュー、`OrderId`と`Username`値、 **OrderDetails**テーブル。 これらの値に一致するメモ、`OrderId`と`Username`に含まれる値、**注文**テーブル。
11. 閉じる、 **OrderDetails**テーブル ウィンドウ。
12. Wingtip Toys のデータベース ファイルを右クリックして (*Wingtiptoys.mdf*) を選択および**接続を閉じる**します。
13. 表示されない場合、**ソリューション エクスプ ローラー**ウィンドウで、をクリックして**ソリューション エクスプ ローラー**の下部にある、**サーバー エクスプ ローラー**ウィンドウに表示、**ソリューション エクスプ ローラー**もう一度です。

## <a name="summary"></a>まとめ

このチュートリアルでは、製品の購入を追跡するためには、注文と注文詳細スキーマを追加します。 また、PayPal の機能は、Wingtip Toys のサンプル アプリケーションに統合。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET の構成の概要](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Azure App Service へのメンバーシップ、OAuth、SQL Database を使用したセキュリティで保護された ASP.NET Web フォーム アプリをデプロイします。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>免責情報

このチュートリアルには、サンプル コードが含まれています。 このようなサンプル コードでは、いかなる保証も伴わず「現状有姿を提供されます。 されます したがって、Microsoft は正確性、整合性、または、サンプル コードの品質保証されません。 ご自身の責任でサンプル コードを使用するものとします。 状況でも Microsoft 責任を負いかねますに何らかの方法でコンテンツを含む、すべてのサンプル コード、コンテンツ、または損失またはすべてのサンプル コードの使用の結果として発生したあらゆる種類の破損でのエラーまたはに限定されませんが、サンプル コード用。 改変通知が表示され補償、保存、および免責 Microsoft から、すべての損失、けがの損失や種類含めて、これらに限定して occasioned または投稿するマテリアルに起因の破損の要求に同意しないでください。送信で使用するかに限定されませんが、その中の見解を含むに依存します。

> [!div class="step-by-step"]
> [前へ](shopping-cart.md)
> [次へ](membership-and-administration.md)
