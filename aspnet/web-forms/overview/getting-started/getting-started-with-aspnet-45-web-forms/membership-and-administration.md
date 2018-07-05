---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: メンバーシップと管理 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: cdd67b946f58d5a6543bb2443dbf193e21751c48
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829795"
---
<a name="membership-and-administration"></a>メンバーシップと管理
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。


このチュートリアルでは、カスタム ロールを追加し、ASP.NET Identity を使用する Wingtip Toys のサンプル アプリケーションを更新する方法を示します。 カスタム ロールを持つユーザーを追加し、web サイトから製品を削除する元の管理ページを実装する方法も示します。

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) ASP.NET web アプリケーションをビルドするために使用するメンバーシップ システムは、ASP.NET 4.5 で使用します。 ASP.NET Identity が用のテンプレートと同様に、Visual Studio 2013 Web フォーム プロジェクト テンプレートで使用される[ASP.NET MVC](../../../../mvc/index.md)、 [ASP.NET Web API](../../../../web-api/index.md)、および[ASP.NET Single Page Application](../../../../single-page-application/index.md). 空の Web アプリケーションで開始すると、NuGet を使用して、ASP.NET Identity システムも具体的にはインストールできます。 ただし、このチュートリアル シリーズで使用する、 **Web フォーム**projecttemplate で、ASP.NET の Id システムが含まれています。 ASP.NET Identity を簡単にアプリケーション データとユーザー固有のプロファイル データを統合します。 また、ASP.NET Identity を使用すると、アプリケーションでユーザー プロファイルの永続化モデルを選択できます。 データを保存するには、SQL Server データベースまたは別のデータ ストアに含む*NoSQL* Windows Azure ストレージ テーブルなどのデータが格納されます。

このチュートリアルでは、"チェック アウトと支払いの PayPal"を「Wingtip Toys のチュートリアル シリーズで前のチュートリアルに基づいています。

## <a name="what-youll-learn"></a>学習内容。

- コードを使用して、アプリケーションにカスタム ロールとユーザーを追加する方法。
- 管理者用フォルダーおよびページへのアクセスを制限する方法。
- カスタム ロールに属しているユーザーのナビゲーションを提供する方法。
- モデル バインドを使用して設定する方法、 [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx)製品カテゴリを持つコントロール。
- 使用して web アプリケーションにファイルをアップロードする方法、 [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx)コントロール。
- 検証コントロールを使用して、入力の検証を実装する方法。
- 追加し、アプリケーションから製品を削除する方法。

## <a name="these-features-are-included-in-the-tutorial"></a>このチュートリアルでは、これらの機能が含まれています。

- ASP.NET Identity
- 構成と承認
- モデル バインド
- 控え目な検証

ASP.NET Web フォームでは、メンバーシップ機能を提供します。 既定のテンプレートを使用するには、アプリケーションの実行時にすぐに使用できる組み込みのメンバーシップ機能があります。 このチュートリアルでは、ASP.NET Identity を使用してカスタム ロールを追加し、そのロールにユーザーを割り当てる方法を示します。 管理フォルダーへのアクセスを制限する方法を学習します。 カスタム ロールを持つユーザーを追加した後に製品をプレビューして、追加したり、製品を削除する管理フォルダーには、ページを追加します。

## <a name="adding-a-custom-role"></a>カスタム ロールを追加します。

ASP.NET Identity を使用して、カスタム ロールの追加し、コードを使用してそのロールにユーザーを割り当てます。

1. **ソリューション エクスプ ローラー**を右クリックし、*ロジック*フォルダーと新しいクラスを作成します。
2. 新しいクラスの名前*RoleActions.cs*します。
3. 次のように表示されるように、コードを変更します。  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. **ソリューション エクスプ ローラー**、オープン、 *Global.asax.cs*ファイル。
5. 変更、 *Global.asax.cs*黄色で強調表示されているように表示されるようにコードを追加してファイル。  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. 注意`AddUserAndRole`赤い下線が引かれます。 AddUserAndRole コードをダブルクリックします。  
   強調表示されているメソッドの先頭に文字"A"は下線が表示されます。
7. 文字"A"をポイントし、クリックして UI のメソッド スタブを生成することができる`AddUserAndRole`メソッド。 

    ![メンバーシップと Advministration - メソッド スタブを生成します。](membership-and-administration/_static/image1.png)
8. オプションをクリックします。  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. 開く、 *RoleActions.cs*ファイルから、*ロジック*フォルダー。  
   `AddUserAndRole`メソッドがクラス ファイルに追加されました。
10. 変更、 *RoleActions.cs*ファイルを削除して、`NotImplementedeException`し、次のように表示されるように黄色で強調表示されているコードを追加します。  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

上記のコードは、まず、メンバーシップ データベースのデータベース コンテキストを確立します。 メンバーシップ データベースとしても格納されている、 *.mdf*ファイル、*アプリ\_データ*フォルダー。 最初のユーザーがこの web アプリケーションにサインインすると、このデータベースを表示することができます。 

> [!NOTE] 
> 
> 製品データと共にメンバーシップ データを格納する場合は、同じ使用を検討することができます**DbContext**上記のコードでは製品データを格納するために使用することです。


 *内部*キーワードは、アクセス修飾子 (クラス) などの型と型のメンバー (メソッド、プロパティなど)。 内部型またはメンバーは、同じアセンブリに含まれるファイル内でのみアクセス可能な *(.dll*ファイル)。 アセンブリ ファイル、アプリケーションをビルドするときに *(.dll*) が作成されるアプリケーションを実行するときに実行されるコードを格納しています。 

A`RoleStore`ロール管理を提供するオブジェクトがデータベース コンテキストに基づいて作成されます。

> [!NOTE] 
> 
> された場合、`RoleStore`オブジェクトが作成される、ジェネリックを使用して`IdentityRole`型。 つまり、`RoleStore`を格納することができる`IdentityRole`オブジェクト。 ジェネリックを使用すると、メモリ内のリソースの処理が向上します。


次に、`RoleManager`オブジェクト、に基づいて作成されますが、`RoleStore`作成したオブジェクト。 `RoleManager`オブジェクトの公開ロール関連 API を自動的に変更を保存するために使用できる、`RoleStore`します。 `RoleManager`を格納することができる`IdentityRole`オブジェクト コードを使用するため、`<IdentityRole>`ジェネリック型。

呼び出す、 `RoleExists` "canEdit"ロールが、メンバーシップ データベースに存在するかどうかを判断するメソッド。 そうでない場合は、ロールを作成します。

作成、`UserManager`よりも複雑にするオブジェクトが表示されます、`RoleManager`制御、ただしほぼ同じです。 いくつかではなく 1 つの行に記述されてだけです。 ここでは、渡されるパラメーターは、かっこ内に含まれる新しいオブジェクトとしてインスタンス化します。

次に新しい"canEditUser"ユーザーを作成する`ApplicationUser`オブジェクト。 次に、ユーザーを正常に作成する場合は、新しいロールにユーザーを追加します。

> [!NOTE] 
> 
> エラー処理は、このチュートリアル シリーズの後半の「ASP.NET エラーの処理"チュートリアル中に更新されます。


次回、アプリケーションの起動時"canEditUser"という名前のユーザーは、という名前のアプリケーションの"canEdit"ロールとして追加されます。 このチュートリアルの後半では、このチュートリアルの中に追加するその他の機能を表示する"canEditUser"ユーザーとしてログインします。 ASP.NET Identity の API については、次を参照してください。、 [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx)します。 ASP.NET の Id システムを初期化する方法について詳細を参照してください、 [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs)します。

### <a name="restricting-access-to-the-administration-page"></a>管理ページにアクセスを制限します。

Wingtip Toys のサンプル アプリケーションは、匿名ユーザーとユーザーのログインの両方を表示および製品を購入できます。 ただし、カスタムの"canEdit"ロールを持つログインしているユーザーは追加して、製品を削除するには制限されているページにアクセスできます。

#### <a name="add-an-administration-folder-and-page"></a>管理者用フォルダーとページを追加します。

次に、という名前のフォルダーを作成、*管理者*Wingtip Toys のカスタム ロールに属している"canEditUser"ユーザーのサンプル アプリケーション。

1. プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**追加** - &gt; **の新しいフォルダー**.
2. 新しいフォルダーの名前*管理者*します。
3. 右クリックし、*管理者*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。   
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
4. 選択、 <strong>Visual c#</strong> - &gt; <strong>Web</strong>左側のテンプレート グループ。 真ん中の一覧から選択<strong>マスター ページを使用した Web フォーム</strong>、という名前を付けます<em>AdminPage.aspx</em><strong>、</strong>し、<strong>追加</strong>します。
5. 選択、 *Site.Master*ファイルをマスター ページとして選び、 **OK**。

#### <a name="add-a-webconfig-file"></a>Web.config ファイルを追加します。

追加することで、 *Web.config*ファイルを*管理者*フォルダー、フォルダー内に含まれているページにアクセスを制限することができます。

1. 右クリックし、*管理者*フォルダーと選択**追加** - &gt; **新しい項目の**します。  
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. Visual c# web テンプレートのリストから選択<strong>Web 構成ファイル</strong>、真ん中の一覧から既定の名前をそのまま使用<em>Web.config</em><strong>、</strong>し、 <strong>追加</strong>します。
3. 既存の XML コンテンツを置き換える、 *Web.config*を次のファイル。  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

保存、 *Web.config*ファイル。 *Web.config*ファイルは、その唯一のユーザーがアプリケーションの"canEdit"ロールに属していることができますに含まれているページへのアクセスを指定します、*管理者*フォルダー。

### <a name="including-custom-role-navigation"></a>カスタム ロールのナビゲーションを含む

アプリケーションの管理 セクションに移動する、カスタムの"canEdit"ロールのユーザーを有効にするへのリンクを追加する必要があります、 *Site.Master*ページ。 "CanEdit"ロールに属しているユーザーが参照できるのみ、**管理者**リンクし、[管理] セクションにアクセスします。

1. ソリューション エクスプ ローラーで検索して開く、 *Site.Master*ページ。
2. "CanEdit"ロールのユーザーへのリンクを作成するには、次の順序なしリストを黄色で強調表示のマークアップを追加`<ul>`要素として一覧が表示されるように従います。  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. 開く、 *Site.Master.cs*ファイル。 ように、**管理者**に黄色で強調表示されているコードを追加することで"canEditUser"ユーザーにのみ表示されるリンク、`Page_Load`ハンドラー。 `Page_Load`ハンドラーは次のように表示されます。   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

ページが読み込まれると、コードがログインしたユーザーが"canEdit"の役割を持つかどうかを確認します。 "CanEdit"ロールへのリンクを含む span 要素に、ユーザーが属している場合、 *AdminPage.aspx*ページ (およびその結果、範囲内のリンク) が表示されます。

### <a name="enabling-product-administration"></a>製品の管理を有効にします。

ここまでは、"canEdit"ロールを作成して、"canEditUser"ユーザーが、[管理] フォルダーと管理ページを追加します。 ページで、その管理フォルダーのアクセス権を設定し、アプリケーションを"canEdit"ロールのユーザーのナビゲーション リンクを追加します。 マークアップを次に、追加は、 *AdminPage.aspx*ページし、コードを*AdminPage.aspx.cs*分離コード ファイルを追加および削除の製品を"canEdit"ロールを持つユーザーを有効になります。

1. **ソリューション エクスプ ローラー**、オープン、 *AdminPage.aspx*ファイルから、*管理者*フォルダー。
2. 次のように既存のマークアップに置き換えます。  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. 次に、開く、 *AdminPage.aspx.cs*分離コード ファイルを右クリックし、 *AdminPage.aspx*  をクリック**コードの表示**します。
4. 既存のコードを置き換える、 *AdminPage.aspx.cs*を次のコードの分離コード ファイル。  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

入力したコードで、 *AdminPage.aspx.cs* 、分離コード ファイルと呼ばれるクラス`AddProducts`データベースに製品を追加する実際の作業です。 このクラスは、今すぐ作成がまだ存在しません。

1. **ソリューション エクスプ ローラー**を右クリックし、*ロジック*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。   
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **コード**左側のテンプレート グループ。 次に、選択**クラス**中央から一覧表示し、名前を*AddProducts.cs*します。   
   新しいクラス ファイルが表示されます。
3. 既存のコードを次のコードに置き換えます。  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx*ページでは、"canEdit"ロールに属しているユーザーを追加して製品を削除します。 新製品が追加されたときに、製品に関する詳細な情報が検証され、データベースに入力し。 新しい製品では、web アプリケーションのすべてのユーザーにすぐに利用できます。

#### <a name="unobtrusive-validation"></a>控え目な検証

ユーザーが提供する製品の詳細、 *AdminPage.aspx*ページは、検証コントロールを使用して検証される (`RequiredFieldValidator`と`RegularExpressionValidator`)。 これらのコントロールは、控え目な検証を自動的に使用します。 控え目な検証では、ページに検証するサーバーへのトリップが必要ありませんが、クライアント側検証ロジックを JavaScript を使用する検証コントロールを使用します。 既定では、控え目な検証が含まれている、 *Web.config*次の構成設定に基づいて、ファイル。

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>正規表現

製品価格、 *AdminPage.aspx*を使用してページを検証、 **RegularExpressionValidator**コントロール。 このコントロールは、関連付けられている入力コントロール ("AddProductPrice"のテキスト ボックス) の値が正規表現で指定されたパターンと一致するかどうかを検証します。 正規表現はパターン マッチングの表記法ですばやく検索して、特定の文字パターンと一致することができます。 **RegularExpressionValidator**コントロールにはという名前のプロパティが含まれています`ValidationExpression`次に示すように、価格の入力の検証に使用される正規表現を格納しています。

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>FileUpload コントロール

追加した入力と検証コントロールのほか、 **FileUpload**への制御、 *AdminPage.aspx*ページ。 このコントロールは、ファイルをアップロードする機能を提供します。 この場合、アップロードするイメージ ファイルのみが許可されます。 分離コード ファイル内 (*AdminPage.aspx.cs*)、ときに、`AddProductButton`がクリックされた、コードのチェック、`HasFile`のプロパティ、 **FileUpload**コントロール。 コントロールにファイルがある場合、イメージの保存 (ファイル拡張子に基づいた) ファイルの種類が許可されている場合、*イメージ*フォルダーと*イメージ/Thumbs*アプリケーションのフォルダー。

#### <a name="model-binding"></a>モデル バインド

このチュートリアル シリーズで前を設定するモデル バインドを使用して、 **ListView**コントロール、 **FormsView**コントロール、 **GridView**コントロール、および**DetailView**コントロール。 このチュートリアルで設定をモデル バインドを使用して、 **DropDownList**製品カテゴリの一覧を持つコントロール。

マークアップに追加した、 *AdminPage.aspx*ファイルが含まれています、 **DropDownList**というコントロール`DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

モデル バインドを使用してこれを設定する**DropDownList**を設定して、`ItemType`属性と`SelectMethod`属性。 `ItemType`属性を使用することを指定します、`WingtipToys.Models.Category`コントロールを設定するときに入力します。 このチュートリアル シリーズの先頭には、この種類の作成によって定義された、`Category`クラス (下記参照)。 `Category`クラスは、*モデル*内のフォルダー、 *Category.cs*ファイル。

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod`の属性、 **DropDownList**コントロールを使用することを指定します、`GetCategories`メソッド (下図) は、分離コード ファイルに含まれる (*AdminPage.aspx.cs*)。

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

このメソッドでは、ことを指定します、`IQueryable`に対してクエリを評価するインターフェイスを使用する`Category`型。 返される値は設定に使用、 **DropDownList**ページのマークアップで (*AdminPage.aspx*)。

設定して、リストの各項目が指定された表示されるテキスト、`DataTextField`属性。 `DataTextField`属性の使用、`CategoryName`の`Category`クラス内の各カテゴリを表示する (上記)、 **DropDownList**コントロール。 項目が選択されたときに渡される実際の値、 **DropDownList**コントロールがに基づいて、`DataValueField`属性。 `DataValueField`属性に設定されて、`CategoryID`の定義と、`Category`クラス (上記参照)。

### <a name="how-the-application-will-work"></a>アプリケーションのしくみ

"CanEdit"ロールに属しているユーザーが最初のページに移動したとき、 `DropDownAddCategory` **DropDownList**コントロールは、前述のように設定します。 `DropDownRemoveProduct` **DropDownList**コントロールが同じアプローチを使用して製品も設定されます。 "CanEdit"ロールに属しているユーザーがカテゴリの種類を選択し、製品の詳細を追加します (**名前**、**説明**、**価格**、および**のイメージファイル**). "CanEdit"ロールに属しているユーザーがクリックすると、**追加製品** ボタン、`AddProductButton_Click`イベント ハンドラーがトリガーされます。 `AddProductButton_Click`イベント ハンドラーが分離コード ファイル内にある (*AdminPage.aspx.cs*) 許可されているファイルの種類と一致するかどうかを確認するイメージ ファイルをチェック *(.gif*、 *.png*、 *.jpeg*、または *.jpg*)。 次に、イメージ ファイルは、Wingtip Toys のサンプル アプリケーションのフォルダーに保存されます。 次に、新製品は、データベースに追加されます。 新製品の新しいインスタンスを追加する、`AddProducts`クラスを作成し、製品の名前します。 `AddProducts`クラスという名前のメソッドには`AddProduct`、および製品オブジェクトは、データベースに製品を追加するには、このメソッドを呼び出します。

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

ページのクエリ文字列の値を再読み込みがコードでは、データベースに新しい製品が正常に追加する場合`ProductAction=add`します。

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

ページが再読み込み、クエリ文字列が URL に含まれます。 ページを再読み込みして"canEdit"ロールに属しているユーザーはすぐに内の更新プログラムを確認、 **DropDownList**のコントロールに対して、 *AdminPage.aspx*ページ。 また、url クエリ文字列を含めることによって、ページを表示できます、成功メッセージ"canEdit"ロールに属しているユーザー。

ときに、 *AdminPage.aspx*再読み込み、ページ、`Page_Load`イベントが呼び出されます。

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load`イベント ハンドラーは、クエリ文字列の値をチェックし、成功メッセージを表示するかどうかを決定します。

## <a name="running-the-application"></a>アプリケーションの実行

ショッピング カートでは、アプリケーションを追加する方法を確認するようになりました、delete、および更新プログラムを実行できます。 ショッピング カート合計は、ショッピング カート内のすべての項目の合計コストに反映されます。

1. ソリューション エクスプ ローラーでキーを押して**F5** Wingtip Toys のサンプル アプリケーションを実行します。  
   ブラウザーが開きを示しています、 *Default.aspx*ページ。
2. をクリックして、**ログイン**ページの上部にあるリンクです。 

    ![メンバーシップと管理 - ログイン リンク](membership-and-administration/_static/image2.png)

   *Login.aspx*ページが表示されます。
3. 次のユーザー名とパスワードを使用します。  
   ユーザー名: canEditUser@wingtiptoys.com  
   Password: Pa$$word1 

    ![メンバーシップと管理 - ログイン ページ](membership-and-administration/_static/image3.png)
4. をクリックして、**ログイン**ページの下部にあるボタンをクリックします。
5. 次のページの上部にある選択、**管理者**に移動するリンク、 *AdminPage.aspx*ページ。 

    ![メンバーシップと管理の管理者リンク](membership-and-administration/_static/image4.png)
6. 入力の検証をテストするには、クリックして、**成果物の追加**せず、製品の詳細を追加するボタンをクリックします。 

    ![メンバーシップと管理の管理者 ページ](membership-and-administration/_static/image5.png)

   必須フィールドのメッセージが表示されることに注意してください。
7. 新製品の詳細を追加し、クリックして、**追加製品**ボタンをクリックします。 

    ![メンバーシップと管理 - 製品を追加します。](membership-and-administration/_static/image6.png)
8. 選択**製品**追加した新しい製品を表示する、上部のナビゲーション メニューから。 

    ![メンバーシップと管理 - 新しい製品を表示します。](membership-and-administration/_static/image7.png)
9. をクリックして、**管理者**リンクの管理 ページに戻ります。
10. **削除製品**セクションで追加した新しい製品を選択、ページの**DropDownListBox**します。
11. をクリックして、**製品の削除**アプリケーションから、新しい成果物を削除するボタンをクリックします。 

    ![メンバーシップと管理 - 製品の削除](membership-and-administration/_static/image8.png)
12. 選択**製品**から上部のナビゲーション メニューの製品が削除されていることを確認します。
13. クリックして**ログオフ**管理モードが存在します。   
    上部のナビゲーション ウィンドウが表示されなくなりますが、**管理者**メニュー項目。

## <a name="summary"></a>まとめ

このチュートリアルでは、カスタム ロールとカスタムのロール ページで、その管理フォルダーへのアクセス制限に属しているユーザーを追加し、カスタム ロールに属しているユーザーのナビゲーションを提供します。 モデル バインドの設定に使用する、 **DropDownList**コントロールにデータ。 実装する、 **FileUpload**コントロールと検証コントロール。 また、追加し、データベースから製品を削除する方法を学習しました。 次のチュートリアルでは、ASP.NET ルーティングを実装する方法を学習します。

## <a name="additional-resources"></a>その他のリソース

[Web.config の authorization 要素](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Azure の Web サイトにメンバーシップ、OAuth、SQL Database を使用したセキュリティで保護された ASP.NET Web フォーム アプリをデプロイします。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [前へ](checkout-and-payment-with-paypal.md)
> [次へ](url-routing.md)
