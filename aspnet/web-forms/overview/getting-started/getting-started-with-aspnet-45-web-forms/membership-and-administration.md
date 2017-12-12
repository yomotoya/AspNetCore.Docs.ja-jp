---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: "メンバーシップと管理 |Microsoft ドキュメント"
author: Erikre
description: "このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: a10dbfe1ca49baee1604aac8dd9a1f93ccfcb7f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="membership-and-administration"></a>メンバーシップと管理
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。


このチュートリアルでは、カスタム ロールを追加して、ASP.NET Identity Wingtip Toys サンプル アプリケーションを更新する方法を示します。 元のカスタム ロールを持つユーザーを追加したり、web サイトから製品を削除、管理ページを実装する方法も示します。

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) ASP.NET web アプリケーションをビルドするために使用するメンバーシップ システムは、ASP.NET 4.5 で使用できます。 用のテンプレートと同様に、Visual Studio 2013 の Web フォーム プロジェクト テンプレートで使用される ASP.NET Id [ASP.NET MVC](../../../../mvc/index.md)、 [ASP.NET Web API](../../../../web-api/index.md)、および[ASP.NET Single Page Application](../../../../single-page-application/index.md). 具体的にも空の Web アプリケーションを開始するときに、NuGet を使用して、ASP.NET Identity システムをインストールできます。 ただし、このチュートリアルの系列に使用する、 **Web フォーム**projecttemplate で、ASP.NET Identity システムが含まれています。 ASP.NET Identity しやすいアプリケーションのデータとユーザー固有のプロファイル データを統合します。 また、ASP.NET Identity には、アプリケーションでユーザー プロファイルの永続化モデルを選択することができます。 データを保存するには、SQL Server データベースまたは別のデータ ストアに含む*NoSQL* Windows Azure ストレージ テーブルなどのデータを格納します。

このチュートリアルは、一連のチュートリアルについて、Wingtip Toys"チェック アウトと支払いの PayPal"というタイトルの前のチュートリアルに基づいています。

## <a name="what-youll-learn"></a>学習する内容。

- アプリケーションにカスタムのロールとユーザーを追加するコードを使用する方法。
- 管理フォルダーとページへのアクセスを制限する方法です。
- カスタム ロールに属しているユーザーのナビゲーションを提供する方法です。
- モデル バインディングを使用して設定する方法、 [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx)製品カテゴリを制御します。
- Web アプリケーションを使用して、ファイルをアップロードする方法、[ファイルアップロード](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx)コントロール。
- 検証コントロールを使用して、入力の検証を実装する方法。
- 追加し、アプリケーションから製品を削除する方法です。

## <a name="these-features-are-included-in-the-tutorial"></a>このチュートリアルでは、これらの機能が含まれています。

- ASP.NET Identity
- 構成と承認
- モデル バインディング
- 控えめな検証

ASP.NET Web フォームでは、メンバーシップ機能を提供します。 既定のテンプレートを使用するには、アプリケーションの実行時にすぐに使用できる組み込みのメンバーシップ機能があります。 このチュートリアルでは、ASP.NET Identity を使用してカスタム ロールを追加し、そのロールにユーザーを割り当てる方法を示します。 管理フォルダーへのアクセスを制限する方法を学習します。 カスタム ロールを持つユーザーを追加および削除の製品、および追加された後に製品をプレビューする管理フォルダーには、ページを追加します。

## <a name="adding-a-custom-role"></a>カスタム ロールを追加します。

ASP.NET の Id を使用して、カスタム ロールを追加してコードを使用してそのロールにユーザーを割り当てます。

1. **ソリューション エクスプ ローラー**を右クリックし、*ロジック*フォルダーと、新しいクラスを作成します。
2. 新しいクラスの名前を*RoleActions.cs*です。
3. 次のように表示されるようにコードを変更します。  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. **ソリューション エクスプ ローラー**を開き、 *Global.asax.cs*ファイル。
5. 変更、 *Global.asax.cs*黄色で強調表示されている次のように表示されるようにコードを追加することによってファイル。  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. 注意して`AddUserAndRole`赤い下線が引かれます。 AddUserAndRole コードをダブルクリックします。  
 強調表示されているメソッドの先頭に文字"A"は下線が表示されます。
7. 文字"A"ポインターを合わせるし、UI のメソッド スタブを生成できるようにする をクリックして、`AddUserAndRole`メソッドです。 

    ![メンバーシップと Advministration - メソッド スタブを生成します。](membership-and-administration/_static/image1.png)
8. という名前のオプションをクリックします。  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. 開く、 *RoleActions.cs*ファイルから、*ロジック*フォルダーです。  
 `AddUserAndRole`メソッドがクラス ファイルに追加されました。
10. 変更、 *RoleActions.cs*ファイルを削除することによって、`NotImplementedeException`し、次のように表示されるように、黄色で強調表示されているコードを追加します。  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

上記のコードは、まず、メンバーシップ データベースのデータベース コンテキストを確立します。 メンバーシップ データベースとしても格納されている、 *.mdf*ファイルで、*アプリ\_データ*フォルダーです。 最初のユーザーがこの web アプリケーションにサインインしたら、このデータベースを表示することができます。 

> [!NOTE] 
> 
> 使用して、同じを検討するには、製品データと共にメンバーシップ データを格納する場合は、 **DbContext**上記のコードで、製品データを格納するために使用することです。


 *内部*キーワードは、アクセス修飾子 (クラス) などの型と型のメンバー (メソッド、プロパティなど)。 内部型またはメンバーは、同じアセンブリに含まれているファイル内でのみアクセス*(.dll*ファイル)。 アプリケーションをアセンブリ ファイルをビルドするときに*(.dll*) が作成されるアプリケーションを実行するときに実行されるコードを含むです。 

A`RoleStore`ロール管理を提供するオブジェクトは、データベース コンテキストをに基づいて作成します。

> [!NOTE] 
> 
> 場合に、`RoleStore`オブジェクトが作成される、ジェネリックを使用して`IdentityRole`型です。 つまり、`RoleStore`を含むことがのみ`IdentityRole`オブジェクト。 ジェネリックを使用すると、メモリ内のリソースの処理が向上します。


次に、`RoleManager`オブジェクト、に基づいて作成されて、`RoleStore`先ほど作成したオブジェクト。 `RoleManager`オブジェクト公開ロール関連 API を自動的に変更を保存するために使用する、`RoleStore`です。 `RoleManager`を含むことがのみ`IdentityRole`コードを使用するためのオブジェクト、`<IdentityRole>`ジェネリック型です。

呼び出す、`RoleExists`メソッドが"canEdit"ロールのメンバーシップ データベース内にあるかを判断します。 そうでない場合は、ロールを作成します。

作成する、`UserManager`よりも複雑であるオブジェクトが表示されます、`RoleManager`を制御することがほぼ同じです。 だけのいくつかではなく 1 つの行に記述されています。 ここでは、渡されたパラメーターは、かっこ内に含まれる新しいオブジェクトとしてインスタンス化します。

次に作成する"canEditUser"ユーザーを作成、新しい`ApplicationUser`オブジェクト。 次に、ユーザーを正常に作成する場合は、新しいロールにユーザーを追加します。

> [!NOTE] 
> 
> エラー処理は、このチュートリアル シリーズの後で「ASP.NET エラーの処理」チュートリアルの中で更新されます。


アプリケーションの起動時、次回"canEditUser"をという名前のユーザーは、アプリケーションの"canEdit"という名前のロールとして追加されます。 このチュートリアルの後半は、このチュートリアル中に追加する追加機能を表示する"canEditUser"のユーザーとしてログインします。 ASP.NET Identity の API 詳細は、次を参照してください。、 [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx)です。 追加の詳細については、ASP.NET Identity システムの初期化には、次を参照してください。、 [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs)です。

### <a name="restricting-access-to-the-administration-page"></a>[管理] ページへのアクセスを制限

Wingtip Toys のサンプル アプリケーションは、匿名ユーザーとユーザーのログインの両方を表示および製品を購入できます。 ただし、カスタム"canEdit"ロールを持つログインしているユーザーは追加して、製品を削除するために制限されているページにアクセスできます。

#### <a name="add-an-administration-folder-and-page"></a>管理フォルダーとページを追加します。

次に、という名前のフォルダーを作成、 *Admin* Wingtip Toys のカスタム ロールに属している"canEditUser"ユーザーのサンプル アプリケーションです。

1. プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**追加** - &gt; **の新しいフォルダー**.
2. 新しいフォルダーの名前を付けます*Admin*です。
3. 右クリックし、 *Admin*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。   
 **[新しい項目の追加]** ダイアログ ボックスが表示されます。
4. 選択、 **Visual c#** - &gt; **Web**左側のテンプレートのグループです。 中央のリストから選択**マスター ページを含む Web フォーム**、名前を付けます*AdminPage.aspx***、**し、**追加**です。
5. 選択、 *Site.Master*クリックしてファイルをマスター ページとして**OK**です。

#### <a name="add-a-webconfig-file"></a>Web.config ファイルを追加します。

追加することによって、 *Web.config*ファイルの名前を*Admin*フォルダー、フォルダー内のページにアクセスを制限することができます。

1. 右クリックし、 *Admin*フォルダーと選択**追加** - &gt; **新しい項目の**します。  
 **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 、Visual c# web テンプレートの一覧から選択**Web 構成ファイル**中央のリストからの既定の名前を受け入れる*Web.config***、**し、 **追加**です。
3. 既存の XML の内容を置き換える、 *Web.config*を次のファイル。  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

保存、 *Web.config*ファイル。 *Web.config*ファイルは、その唯一のユーザーがアプリケーションの"canEdit"ロールに所属することができますに含まれているページへのアクセスを指定します、 *Admin*フォルダーです。

### <a name="including-custom-role-navigation"></a>カスタム ロールのナビゲーションを含む

アプリケーションの管理 セクションに移動する、カスタム"canEdit"ロールのユーザーを有効にするへのリンクを追加する必要があります、 *Site.Master*ページ。 "CanEdit"ロールに属しているユーザーが参照できるのみ、 **Admin**リンクし、[管理] セクションにアクセスします。

1. ソリューション エクスプ ローラーで、検索して開く、 *Site.Master*ページ。
2. "CanEdit"ロールのユーザーへのリンクを作成するには、次の順序なしリストを黄色で強調表示のマークアップを追加`<ul>`要素として一覧が表示されるように依存します。  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. 開く、 *Site.Master.cs*ファイル。 ように、 **Admin**に黄色で強調表示されているコードを追加することで"canEditUser"のユーザーにのみ表示されるリンク、`Page_Load`ハンドラー。 `Page_Load`ハンドラーは次のように表示されます。   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

ページが読み込まれると、コードは、ログイン ユーザーが"canEdit"の役割を持つかどうかを確認します。 ユーザーが、"canEdit"の役割へのリンクを含む span 要素に属している場合、 *AdminPage.aspx*ページとその範囲内のリンク) が表示されます。 します。

### <a name="enabling-product-administration"></a>製品の管理を有効にします。

これまで"canEdit"ロールを作成し、"canEditUser"ユーザー、[管理] フォルダー、および管理ページを追加しています。 ページで、管理フォルダーのアクセス権を設定して、アプリケーションに"canEdit"ロールのユーザーのナビゲーション リンクを追加しました。 次に、するマークアップを追加、 *AdminPage.aspx*ページし、コードを*AdminPage.aspx.cs*を追加して、製品の削除"canEdit"の役割を持つユーザーを有効にするには、分離コード ファイル。

1. **ソリューション エクスプ ローラー**を開き、 *AdminPage.aspx*ファイルから、 *Admin*フォルダーです。
2. 既存のマークアップを次のように置き換えます。  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. 次に、開く、 *AdminPage.aspx.cs*分離コード ファイルを右クリックして、 *AdminPage.aspx*をクリックすると、**コードの表示**です。
4. 既存のコードを置き換える、 *AdminPage.aspx.cs*を次のコードの分離コード ファイル。  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

に対して入力したコードで、 *AdminPage.aspx.cs*分離コード ファイルと呼ばれるクラス`AddProducts`データベースに製品の追加の実際の作業です。 このクラスは、今すぐ作成は、まだ、存在しません。

1. **ソリューション エクスプ ローラー**を右クリックし、*ロジック*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。   
 **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **コード**左側のテンプレートのグループです。 次に、選択**クラス**中央から一覧表示し、名前を付けます*AddProducts.cs*です。   
 新しいクラス ファイルが表示されます。
3. 既存のコードを次のコードに置き換えます。  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx*ページでは、"canEdit"ロールに所属するユーザーを追加して、製品を削除します。 新しい製品が追加されると、製品に関する詳細が検証し、データベースに入力します。 新しい製品では、web アプリケーションのすべてのユーザーにすぐに使用できます。

#### <a name="unobtrusive-validation"></a>控えめな検証

製品の詳細については、ユーザーを*AdminPage.aspx*ページは、検証コントロールを使用して検証されます (`RequiredFieldValidator`と`RegularExpressionValidator`)。 これらのコントロールは、控えめな検証を自動的に使用します。 控えめな検証では、ため、ページが検証に使用するサーバーへのトリップが必要ないクライアント側の検証ロジックは、JavaScript を使用する検証コントロールを使用します。 既定では、控えめな検証が含まれている、 *Web.config*ファイルは、次の構成設定に基づきます。

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>正規表現

製品の価格、 *AdminPage.aspx*を使用してページを検証、 **RegularExpressionValidator**コントロール。 このコントロールは、関連付けられた入力コントロール ("AddProductPrice"TextBox) の値が正規表現で指定されたパターンに一致するかどうかを検証します。 正規表現とは、パターン一致表記法をすばやく検索および特定の文字パターンに一致することができます。 **RegularExpressionValidator**コントロールには、という名前のプロパティが含まれています。`ValidationExpression`次に示すように、価格の入力の検証に使用される正規表現を格納しています。

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>ファイルアップロード コントロール

追加しただけでなく、入力との検証コントロール、**ファイルアップロード**コントロールを*AdminPage.aspx*ページ。 このコントロールは、ファイルをアップロードする機能を提供します。 この場合、のみを許可するイメージ ファイルをアップロードします。 分離コード ファイル内 (*AdminPage.aspx.cs*)、ときに、`AddProductButton`がクリックすると、コードのチェック、`HasFile`のプロパティ、**ファイルアップロード**コントロール。 コントロールにファイルがあると、イメージに保存 (ファイル拡張子に基づいた) ファイルの種類が許可された場合、*イメージ*フォルダーおよび*イメージ/親指*アプリケーションのフォルダーです。

#### <a name="model-binding"></a>モデル バインディング

このチュートリアルの系列で前を設定するモデル バインドを使用して、 **ListView**コントロール、 **FormsView**コントロール、 **GridView**コントロール、および**DetailView**コントロール。 このチュートリアルではモデル バインディングを使用する設定、 **DropDownList**製品カテゴリの一覧を制御します。

追加するマークアップを*AdminPage.aspx*ファイルが含まれています、 **DropDownList**と呼ばれるコントロール`DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

これを設定するモデル バインドを使用する**DropDownList**を設定して、`ItemType`属性および`SelectMethod`属性。 `ItemType`属性は、使用することを指定します、`WingtipToys.Models.Category`コントロールを設定するときに入力します。 このチュートリアル シリーズの先頭には、この型を作成することで定義した、`Category`クラス (下図参照)。 `Category`クラスが含まれて、*モデル*内のフォルダー、 *Category.cs*ファイル。

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod`の属性、 **DropDownList**コントロールは、使用することを指定します、`GetCategories`メソッド (下図参照) は、分離コード ファイルに含まれる (*AdminPage.aspx.cs*)。

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

このメソッドを指定する、`IQueryable`に対するクエリを評価するインターフェイスを使用する`Category`型です。 返された値が設定に使用されます、 **DropDownList**ページのマークアップ (*AdminPage.aspx*)。

設定して、リスト内の各項目が指定されて表示されるテキスト、`DataTextField`属性。 `DataTextField`属性の使用、`CategoryName`の`Category`クラス内の各カテゴリを表示する (上記)、 **DropDownList**コントロール。 項目が選択したときに渡される実際の値、 **DropDownList**コントロールがに基づいて、`DataValueField`属性。 `DataValueField`属性に設定されている、`CategoryID`での定義、 `Category` (上記) クラス。

### <a name="how-the-application-will-work"></a>アプリケーションがどのように動作

"CanEdit"ロールに属しているユーザーが最初に、ページに移動すると、 `DropDownAddCategory` **DropDownList**前述のようにコントロールが設定されます。 `DropDownRemoveProduct` **DropDownList**コントロールが、これと同じアプローチを使用して製品も表示されます。 "CanEdit"ロールに属しているユーザーは、カテゴリの種類を選択し、製品の詳細を追加します (**名前**、**説明**、**価格**、および**のイメージファイル**). "CanEdit"ロールに属しているユーザーがクリックしたとき、**製品の追加** ボタン、`AddProductButton_Click`のイベント ハンドラーを起動します。 `AddProductButton_Click`イベント ハンドラーが、分離コード ファイルにあります (*AdminPage.aspx.cs*) 許可されているファイルの種類と一致するかどうかを確認するイメージ ファイルをチェック*(.gif*、 *.png*、 *.jpeg*、または*.jpg*)。 その後、イメージ ファイルは、Wingtip Toys のサンプル アプリケーションのフォルダーに保存されます。 次に、新しい製品は、データベースに追加されます。 新しい製品の新しいインスタンスを追加するために、`AddProducts`クラスが作成され、製品をという名前です。 `AddProducts`クラスという名前のメソッドには`AddProduct`、製品オブジェクトが製品をデータベースに追加するには、このメソッドを呼び出します。

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

クエリ文字列の値、ページが再読み込みされるコードが正常に追加する場合、新しい製品をデータベースに`ProductAction=add`です。

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

ページが再読み込み時に、URL にクエリ文字列が含まれています。 ページを再度読み込んで、によって"canEdit"ロールに属しているユーザーはすぐに内の更新プログラムを確認、 **DropDownList**コントロールに対して、 *AdminPage.aspx*ページ。 また、URL を使用してクエリ文字列に含めても、ページを表示できます、成功メッセージ"canEdit"ロールに属しているユーザー。

ときに、 *AdminPage.aspx*再読み込みをページ、`Page_Load`イベントが呼び出されます。

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load`イベント ハンドラーは、クエリ文字列値をチェックし、成功メッセージを表示するかどうかを決定します。

## <a name="running-the-application"></a>アプリケーションの実行

今すぐアプリケーションを追加する方法を表示、削除、および更新アイテムは、ショッピング カートで実行できます。 ショッピング カートの合計金額、ショッピング カート内のすべての項目の総コストが反映されます。

1. ソリューション エクスプ ローラーでキーを押して**f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. クリックして、**ログイン**ページの上部にあるリンクします。 

    ![メンバーシップと管理 - ログイン リンク](membership-and-administration/_static/image2.png)

 *Login.aspx*ページが表示されます。
3. 次のユーザー名とパスワードを使用します。  
 ユーザー名:canEditUser@wingtiptoys.com  
 パスワード: Pa $word1 

    ![メンバーシップと管理 - ログイン ページ](membership-and-administration/_static/image3.png)
4. クリックして、**ログイン**ページの下部にあるボタンをクリックします。
5. 次のページの上部には、選択、 **Admin**に移動するリンク、 *AdminPage.aspx*ページ。 

    ![メンバーシップと管理 - Admin リンク](membership-and-administration/_static/image4.png)
6. 入力の検証をテストするには、クリックして、**製品の追加**製品詳細を追加することがなくボタンをクリックします。 

    ![メンバーシップと管理 - [管理] ページ](membership-and-administration/_static/image5.png)

 必須フィールドのメッセージが表示されることに注意してください。
7. 新しい製品では、詳細を追加して、をクリックして、**製品の追加**ボタンをクリックします。 

    ![メンバーシップと管理 - 製品を追加します。](membership-and-administration/_static/image6.png)
8. 選択**製品**追加してから、上部のナビゲーション メニューを新しい製品を表示します。 

    ![メンバーシップと管理 - 新しい製品を表示します。](membership-and-administration/_static/image7.png)
9. をクリックして、 **Admin**管理ページに戻るにリンクします。
10. **削除製品**セクションで追加した新しい製品を選択、ページの**DropDownListBox**です。
11. をクリックして、**製品の削除**アプリケーションから、新しい製品を削除するボタンをクリックします。 

    ![メンバーシップと管理 - 削除製品](membership-and-administration/_static/image8.png)
12. 選択**製品**製品が削除されたことを確認する、上部のナビゲーション メニューからです。
13. をクリックして**ログオフ**管理モードが存在します。   
 上部のナビゲーション ウィンドウは表示されなくなりますに注意してください、 **Admin**メニュー項目。

## <a name="summary"></a>概要

このチュートリアルでは、カスタムのロールとカスタムのロール ページで、管理フォルダーへのアクセス制限に属しているユーザーを追加し、カスタムのロールに属しているユーザーのナビゲーションを指定しました。 モデル バインディングの設定に使用する、 **DropDownList**コントロールにデータ。 実装する、**ファイルアップロード**コントロールと検証コントロール。 また、追加し、データベースから製品を削除する方法を学習しました。 次のチュートリアルでは、ASP.NET のルーティングを実装する方法を学習します。

## <a name="additional-resources"></a>その他のリソース

[Web.config で承認要素](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Id](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Azure の Web サイトにメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET Web フォーム アプリケーションを展開します。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
[前へ](checkout-and-payment-with-paypal.md)
[次へ](url-routing.md)
