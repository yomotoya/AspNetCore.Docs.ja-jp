---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: ユーザー (c#) に基づいてデータ編集機能を制限 |Microsoft Docs
author: rick-anderson
description: ユーザー データを編集できるように web アプリケーションを別のユーザー アカウントは別のデータの編集の権限があります。 このチュートリアルではについて説明しますか t.
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f54f8ef593363f9428b663051cc71b8ef4a2e67
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829829"
---
<a name="limiting-data-modification-functionality-based-on-the-user-c"></a>ユーザー (c#) に基づいてデータ編集機能を制限
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe)または[PDF のダウンロード](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> ユーザー データを編集できるように web アプリケーションを別のユーザー アカウントは別のデータの編集の権限があります。 このチュートリアルでは、訪問したユーザーに基づくデータ変更機能を動的に調整する方法を考察します。


## <a name="introduction"></a>はじめに

多数の web アプリケーションでは、ユーザー アカウントをサポートし、さまざまなオプション、レポート、およびログインしているユーザーに基づく機能を提供します。 たとえば、このチュートリアルでする可能性がおそらく - 自分の会社名など、仕入先の情報と共に、名と、単位あたりの数量 - 自社製品のサイトと更新プログラムの全般情報にログオンする仕入れ先企業からユーザーを許可します。アドレス、s の連絡先情報、およびなど。 さらに、それらにログオンしてユニットの在庫レベルの順序を変更やなどのように、製品情報を更新できるように、会社からユーザーの一部のユーザー アカウントを含める可能性があります。 Web アプリケーションには (ログインしていない人) を参照してください。 匿名ユーザーができるように可能性がありますもが、データの表示だけを制限するとします。 このようなユーザー アカウント システムの場所は、挿入、編集、および削除、現在ログオンしているユーザーの適切な機能を提供する、ASP.NET ページでデータ Web コントロールします。

このチュートリアルでは、訪問したユーザーに基づくデータ変更機能を動的に調整する方法を考察します。 具体的には、供給業者によって提供される製品を一覧表示する GridView と共に編集可能な DetailsView で仕入先情報を表示するページを作成します。 ページにアクセスするユーザーが会社からの場合は、できる限り:; 業者の情報を表示ユーザーのアドレスを編集します。および業者によって提供される製品の情報を編集します。 のみできる場合、ただし、ユーザーは、特定の企業が、表示および独自のアドレス情報を編集し、提供が中止されたとマークされていない独自の製品のみを編集できます。


[![会社からユーザーが、業者の情報を編集できます。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**図 1**: ユーザーからの会社は編集 Any 業者の情報 ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))。


[![ユーザーが特定のサプライヤーが専用のビューと編集の情報](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**図 2**: ユーザーから特定サプライヤーできますのみ表示と編集の情報 ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))。


Let s を始めましょう。

> [!NOTE]
> ASP.NET 2.0 メンバーシップ システムを作成、管理、およびユーザー アカウントを検証するための標準化された、拡張性の高いプラットフォームを提供します。 調べる場合は、メンバーシップ システムがこれらのチュートリアルの範囲を超えているため、このチュートリアル代わりに"fakes"メンバーシップまたは、会社からの特定のサプライヤーから供給されるかどうかを選択する匿名の訪問者を許可することで。 詳細については、メンバーシップを参照してください、 [ASP.NET 2.0 の検査のメンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)記事シリーズ。


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>手順 1: ユーザー指定のアクセス権を許可します。

実際の web アプリケーションで協力して、社内用または特定の仕入先と、ユーザーがサイトにログオンすると、この情報が、ASP.NET ページからプログラムでアクセスできるでしょうかどうかをユーザー アカウント情報が含まれます。 この情報は、プロファイル システム、またはカスタムの手段を通じてユーザー レベルのアカウント情報として、ASP.NET 2.0 のロール システムを介してキャプチャでした。

このチュートリアルの目的が、ログオンしたユーザーに基づくデータ変更機能を調整することを示すために、ショーケース ASP.NET 2.0 のメンバーシップ、ロール、およびプロファイル システムするものではありませんので、非常に単純にメカニズムを使用して、特定、機能ページのかどうかを表示および編集の仕入先情報や、または、何ができる元のユーザーを示すことができます、DropDownList にアクセスするユーザーの特定の仕入先の情報を表示および編集します。 彼女表示し、すべての仕入先情報 (既定値) を編集、ユーザーが示されている場合は、すべての仕入先内でページを仕入先のアドレス情報を編集し、選択した業者によって提供される製品の単位あたりの数量と名前を編集彼女ことができます。 ユーザーは彼女だけを表示できますを編集し、ただしの特定の仕入先、彼女だけその 1 つの仕入先の詳細および製品を表示でき、のみ更新あたりユニットの情報がこれらの製品の数量と名前を示す場合*いない*廃止されました。

このチュートリアルでは、最初、手順は、この DropDownList を作成し、システムで、サプライヤーと設定を次に、です。 開く、`UserLevelAccess.aspx`ページで、`EditInsertDelete`フォルダー、DropDownList を追加が`ID`プロパティに設定されて`Suppliers`、という名前の新しい ObjectDataSource にこの DropDownList をバインドおよび`AllSuppliersDataSource`します。


[![AllSuppliersDataSource という名前の新しい ObjectDataSource を作成します。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**図 3**: 名前付き新しい ObjectDataSource 作成`AllSuppliersDataSource`([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))。


この DropDownList を all に含めるので、構成を呼び出す ObjectDataSource、`SuppliersBLL`クラスの`GetSuppliers()`メソッド。 ObjectDataSource s も確認`Update()`をメソッドにマップされて、`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッドでは、この ObjectDataSource としてもによって使用される DetailsView を手順 2. で追加されていきます。

ObjectDataSource ウィザードを完了すると、構成することで、手順を完了、 `Suppliers` DropDownList が表示されるよう、`CompanyName`使用してデータ フィールド、`SupplierID`データ フィールドの各値として`ListItem`します。


[![CompanyName と SupplierID データ フィールドを使用する仕入先の DropDownList を構成します。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**図 4**: 構成、 `Suppliers` DropDownList を使用して、`CompanyName`と`SupplierID`データ フィールド ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))。


この時点では、DropDownList には、データベースの仕入先の会社名が一覧表示します。 ただし、必要もありますし、DropDownList に「すべてサプライヤーの表示/編集」オプションを含めます。 これを行うには、設定、 `Suppliers` DropDownList s`AppendDataBoundItems`プロパティを`true`し、追加、`ListItem`が`Text`プロパティが"表示/編集 All"、値が`-1`します。 [プロパティ] ウィンドウに移動し、DropDownList s で省略記号をクリックして、宣言型マークアップから直接、またはデザイナーを使用この追加することができます`Items`プロパティ。

> [!NOTE]
> 戻って、 [*マスター/詳細のフィルター処理で、DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)の詳細については、すべての項目をデータ バインディングの DropDownList に追加するためのチュートリアルです。


後に、`AppendDataBoundItems`プロパティが設定されていると、`ListItem`よう追加されると、DropDownList s の宣言型マークアップになります。


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

図 5 は、ブラウザーで表示したときに、現在の進行状況のスクリーン ショットを示します。


[![Suppliers DropDownList には、すべての ListItem と各業者の 1 つを表示が含まれます](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**図 5**: `Suppliers` DropDownList がすべて表示が含まれています`ListItem`、および 1 つの各仕入先 ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))。


ユーザーが自分の選択を変更した直後にユーザー インターフェイスを更新するので、設定、 `Suppliers` DropDownList s`AutoPostBack`プロパティを`true`します。 手順 2. で DropDownList の選択に基づくサプライヤーの情報を表示する DetailsView コントロールを作成します。 次に、手順 3. で作成しますこの DropDownList s のイベント ハンドラー`SelectedIndexChanged`イベント、これで選択した仕入先に基づいて、DetailsView に適切な仕入先の情報をバインドするコードは追加します。

## <a name="step-2-adding-a-detailsview-control"></a>手順 2: DetailsView コントロールを追加します。

S、DetailsView を使用して仕入先情報を表示することができます。 表示およびすべての仕入先を編集できるユーザー、ユーザーの DetailsView はページングをサポートして、一度に仕入先情報の 1 つのレコードをステップ ユーザーを許可します。 特定業者の場合、ユーザーは、ただし、DetailsView がその仕入先を特定の情報が表示されます、およびページング インターフェイスは含まれません。 どちらの場合は、DetailsView を仕入先の住所や市区町村、都道府県 フィールドを編集するユーザーを許可する必要があります。

下に、DetailsView を追加、 `Suppliers` DropDownList を設定、`ID`プロパティを`SupplierDetails`にバインドし、 `AllSuppliersDataSource` ObjectDataSource は、前の手順で作成します。 次に、DetailsView s のスマート タグからのページングを有効にして、編集を有効にするチェック ボックスを確認します。

> [!NOTE]
> ないスマート DetailsView s で編集を有効にするオプションが表示される場合は、タグ付けして s ObjectDataSource s をマップしなかったため`Update()`メソッドを`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッド。 ご協力をクリックして戻り、この構成を変更、その後、編集を有効にするオプションが DetailsView s のスマート タグに表示されます。


`SuppliersBLL`クラス s`UpdateSupplierAddress`メソッドは、4 つのパラメーターのみを受け入れる`supplierID`、 `address`、 `city`、および`country`-DetailsView の BoundFields を変更できるように、`CompanyName`と`Phone`BoundFields は読み取り専用です。 さらに、削除、 `SupplierID` BoundField 全体。 最後に、 `AllSuppliersDataSource` ObjectDataSource が現在その`OldValuesParameterFormatString`プロパティに設定`original_{0}`します。 、宣言型構文全体からこのプロパティの設定を削除するか、既定の値に設定する少し`{0}`します。

構成した後、 `SupplierDetails` DetailsView と`AllSuppliersDataSource`ObjectDataSource に、次の宣言型マークアップ。


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

この時点で、DetailsView を介してページングされることができでの選択に関係なく、選択した仕入先のアドレス情報を更新できる、 `Suppliers` DropDownList (図 6 参照)。


[![仕入先情報を表示すること、やそのアドレスを更新](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**図 6**: Any 仕入先情報を表示したり、そのアドレスを更新 ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))。


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>手順 3: 選択したサプライヤーの情報のみを表示します。

このページは現在から特定の仕入先が選択されているかどうかに関係なくすべての仕入先の情報を表示、 `Suppliers` DropDownList します。 選択した仕入先の仕入先の情報だけを表示するためには、このページで、特定の仕入先に関する情報を取得する 1 つに別の ObjectDataSource を追加する必要があります。

名前のページに新しい ObjectDataSource を追加`SingleSupplierDataSource`します。 スマート タグ、データ ソースの構成 をクリックしを使用させる、`SuppliersBLL`クラスの`GetSupplierBySupplierID(supplierID)`メソッド。 同様、 `AllSuppliersDataSource` 、ObjectDataSource が、 `SingleSupplierDataSource` ObjectDataSource s`Update()`メソッドにマップする、`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッド。


[![GetSupplierBySupplierID(supplierID) メソッドを使用して SingleSupplierDataSource ObjectDataSource を構成します。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**図 7**: 構成、 `SingleSupplierDataSource` ObjectDataSource を使用して、`GetSupplierBySupplierID(supplierID)`メソッド ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))。


次に、パラメーターのソースを指定する re 求め、`GetSupplierBySupplierID(supplierID)`メソッドの`supplierID`入力パラメーター。 DropDownList を使用してから選択した供給業者の情報を表示するため、 `Suppliers` DropDownList の`SelectedValue`パラメーターのソースとしてのプロパティ。


[![Suppliers DropDownList を supplierID パラメーターのソースとして使用します。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**図 8**: 使用して、`Suppliers`として DropDownList、`supplierID`パラメーター ソース ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))。


DetailsView コントロールに現在常に使用する構成されている追加のこの 2 つ目 ObjectDataSource を使用しても、 `AllSuppliersDataSource` ObjectDataSource します。 に応じて DetailsView によって使用されるデータ ソースを調整するためのロジックを追加する必要があります、 `Suppliers` DropDownList の項目を選択します。 これを行うには、作成、 `SelectedIndexChanged` Suppliers DropDownList のイベント ハンドラー。 これは、デザイナーで DropDownList をダブルクリックすると最も簡単に作成できます。 このイベント ハンドラーは、使用するには、どのようなデータ ソースを決定する必要があり、DetailsView にデータを再バインドする必要があります。 これは、次のコードで実現されます。


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

イベント ハンドラーは、"表示/編集 All"オプションを選択したかどうかを決定することで開始します。 設定した場合、 `SupplierDetails` DetailsView s`DataSourceID`に`AllSuppliersDataSource`し、業者のセット内の最初のレコードを設定して、ユーザーを返します、`PageIndex`プロパティを 0 にします。 ただし、ユーザーが、DropDownList、DetailsView s から特定の仕入先を選択した場合、`DataSourceID`に割り当てられている`SingleSuppliersDataSource`します。 どのようなデータに関係なく、ソースを使用する、`SuppliersDetails`モードは読み取り専用モードに戻すしへの呼び出しによってデータが、DetailsView をバインドし、`SuppliersDetails`コントロールの`DataBind()`メソッド。

場所でのこのイベント ハンドラーと DetailsView コントロールでは、仕入先のすべてを参照してページング インターフェイスを使用する場合、"表示/編集 All"オプションを選択した場合を除き、選択したサプライヤーがようになりました示します。 図 9 は、"表示/編集 All"オプションを選択します。 ページを示していますユーザーがアクセスし、いずれかの供給を更新できるようにページング インターフェイスが存在することを確認します。 図 10 では、選択した Ma 商店株式会社の供給元に、ページを示します。 のみの Ma 商店株式会社 s の情報は、ここで表示して編集可能です。


[![表示および編集するには、すべての仕入先情報](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**図 9**: すべての編集と仕入先情報を表示できます ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))。


[![選択したサプライヤーの情報のみを表示および編集できます。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**図 10**: のみ Viewed および編集された仕入先の選択の情報が表示できます ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))。


> [!NOTE]
> このチュートリアルでは、DropDownList と DetailsView コントロール s`EnableViewState`に設定する必要があります`true`(既定) ため、DropDownList s`SelectedIndex`と DetailsView の`DataSourceID`のプロパティの変更は、ポストバック間で記憶する必要があります。


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>手順 4: 編集可能な GridView で Suppliers 製品を一覧表示します。

完了の DetailsView を選択した業者によって提供されるこれらの製品を一覧表示する編集可能な GridView を含めるには、次のステップです。 この GridView でのみ編集を許可する必要があります、`ProductName`と`QuantityPerUnit`フィールド。 さらに、ページにアクセスするユーザーが特定のサプライヤーからの場合は、のみを許可するのにはこれらの製品の更新プログラム*いない*廃止されました。 最初のオーバー ロードを追加する必要がありますこれを実現する、`ProductsBLL`クラス s`UpdateProducts`で受け取るメソッドだけ`ProductID`、 `ProductName`、および`QuantityPerUnit`入力としてのフィールド。 私たち事前は多数のチュートリアルで、このプロセスをステップ実行しましたが、それでは s だけコードを見て、ここでは、そのデータに追加する必要があります`ProductsBLL`:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

このオーバー ロードに、作成 GridView コントロールとその関連付けられている ObjectDataSource を追加する準備が整いました。 ページへの新しい GridView、設定、`ID`プロパティを`ProductsBySupplier`、という名前の新しい ObjectDataSource を使用するように構成および`ProductsBySupplierDataSource`します。 この GridView は選択した業者によってこれらの製品を一覧表示するため、使用、`ProductsBLL`クラスの`GetProductsBySupplierID(supplierID)`メソッド。 マップすることも、`Update()`メソッドを新しい`UpdateProduct`オーバー ロードを作成しました。


[![先ほど作成した UpdateProduct オーバー ロードを使用する ObjectDataSource を構成します。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**図 11**: 構成に使用する ObjectDataSource、`UpdateProduct`先ほど作成したオーバー ロード ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))。


パラメーターのソースを選択するように求め re、`GetProductsBySupplierID(supplierID)`メソッドの`supplierID`入力パラメーター。 使用して、DetailsView で選択されている業者の製品を表示するので、 `SuppliersDetails` DetailsView コントロールの`SelectedValue`パラメーターのソースとしてのプロパティ。


[![パラメーターのソースとして SuppliersDetails DetailsView の SelectedValue プロパティを使用します。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**図 12**: 使用して、 `SuppliersDetails` DetailsView s`SelectedValue`パラメーターのソースとしてのプロパティ ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))。


以外の GridView フィールドのすべてを削除して、GridView に返す`ProductName`、`QuantityPerUnit`と`Discontinued`、マーク、 `Discontinued` CheckBoxField 読み取り専用とします。 また、GridView s のスマート タグの編集を有効にするオプションを確認します。 これらの変更が完了したら、GridView と ObjectDataSource の宣言型マークアップする必要があります、次のようになります。


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

この 1 つの s を以前 ObjectDataSources と同様`OldValuesParameterFormatString`プロパティに設定されて`original_{0}`s の製品名または単位あたりの数量を更新する際に問題が発生します。 宣言型構文からこのプロパティを完全に削除またはその既定値に設定`{0}`します。

この構成が完了、弊社のページに表示されます、GridView で選択されている業者によって提供される製品 (図 13 を参照してください)。 現在*任意*s の製品名または単位あたりの数量を更新することができます。 ただし、このような機能が廃止された製品の特定のサプライヤーに関連付けられているユーザーの禁止されているように、ページ ロジックを更新する必要があります。 この最後の部分では、手順 5 に取り組むします。


[![選択されている業者によって提供される製品が表示されます。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**図 13**: 選択されている業者によって提供される、製品が表示されます ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))。


> [!NOTE]
> 編集可能なこの GridView を追加すると、 `Suppliers` DropDownList の`SelectedIndexChanged`イベント ハンドラーを更新して、GridView を読み取り専用状態に戻す必要があります。 それ以外の場合、異なる供給業者は製品情報の編集中に選択されている場合、その新しい業者の GridView の対応するインデックスも編集できます。 これを防ぐためには、GridView s を設定するだけ`EditIndex`プロパティを`-1`で、`SelectedIndexChanged`イベント ハンドラー。


また、s のビュー ステートが GridView に (既定の動作) が有効になっている必要があることを思い出してください。 GridView 秒に設定した場合`EnableViewState`プロパティを`false`、同時実行ユーザーが誤って削除または編集を記録するというリスクを実行します。 参照してください[警告: 同時実行の問題を ASP.NET 2.0 Gridview と DetailsView/FormViews そのサポートを編集または削除およびをビュー ステートが無効になっている](http://scottonwriting.net/sowblog/posts/10054.aspx)詳細についてはします。

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>手順 5: 廃止された製品ときに表示/編集すべて仕入先が選択されていない編集を禁止します。

中に、 `ProductsBySupplier` GridView は、完全に機能は、現在アクセスを許可が多すぎるの特定のサプライヤーから供給はこれらのユーザーにします。 当社のビジネス ルールごと、そのようなユーザーは必要があります提供が中止された製品を更新できません。 を適用するには、非ことができます (を無効にする) の提供が中止された製品、業者からのユーザーが、ページが参照されているときに、GridView の行の編集 ボタン。

GridView s のイベント ハンドラーを作成`RowDataBound`イベント。 このイベント ハンドラーで、ユーザーがこのチュートリアルでは、サプライヤーの DropDownList s をチェックして決定できますが、特定のサプライヤーと関連付けられているかどうかを判断する`SelectedValue`プロパティの場合、s が-1 で、そのユーザーよりもその他のものが特定のサプライヤーに関連付けられました。 このようなユーザーの場合、製品が提供が中止されたかどうかを判断する必要があります。 実際への参照を取得できます`ProductRow`インスタンスを使用して GridView の行にバインドされる、`e.Row.DataItem`で説明したように、プロパティ、 [ *GridView のフッターに概要情報を表示する*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md)チュートリアルです。 GridView s、前のチュートリアルで説明した手法を使用して [commandfield] で [編集] ボタンをプログラムによって参照を取得できます、製品が提供が中止された場合[*を追加するクライアント側の確認と削除*](adding-client-side-confirmation-when-deleting-cs.md). 行ってから参照し、非表示にまたは、ボタンを無効にできます。


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

このイベントにハンドラーが配置では、このページにアクセスをユーザーとして特定のサプライヤーからは提供が中止されたこれらの製品は、編集していないときに [編集] ボタンとしては表示されませんこれらの製品。 たとえば、Chef Anton の Gumbo ミックスでは、ニューオーリンズ Cajun Delights 業者の提供が中止された製品です。 この特定の仕入先のページにアクセスと、見えないようにからこの製品の編集 ボタンが非表示に (図 14 を参照してください)。 ただし、"表示/編集すべて Suppliers"を使用してアクセスして、[編集] ボタンは使用できます (図 15 参照) です。


[![供給業者に固有のユーザーの Chef Anton の Gumbo ミックスの編集 ボタンが非表示します。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**図 14**: Chef Anton の Gumbo ミックスの編集 ボタンを非表示に業者特定ユーザーの ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))。


[![仕入先ユーザーをすべて表示/編集、Chef Anton の Gumbo ミックスの編集 ボタンが表示されます。](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**図 15**: の表示/編集すべて仕入先のユーザー、Gumbo ミックスが表示される Chef Anton s の編集ボタン ([フルサイズの画像を表示する をクリックします](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))。


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>ビジネス ロジック層でのアクセス権の確認

このチュートリアルは、ASP.NET ではページはどのような情報が表示に関してすべてのロジックを処理し、更新はどのような製品です。 理想的には、このロジックもビジネス ロジック層に存在できます。 たとえば、`SuppliersBLL`クラス s `GetSuppliers()` (すべての仕入先を返す) メソッドには、現在ログオンしているユーザーがあることを確認するチェックが含まれます*いない*特定のサプライヤーに関連付けられています。 同様に、`UpdateSupplierAddress`メソッドは、現在ログオンしているユーザーのいずれか、会社の作業 (および仕入先のすべてのアドレス情報を更新できます) のチェックを含めることができますか、更新されるデータが供給業者に関連付けられています。

BLL レイヤーそのようなチェックをここでは、DropDownList BLL クラスにアクセスできません ページで、チュートリアルでは、ユーザーの権限が決定されるためは含まれませんでした。 メンバーシップ システム、または (Windows 認証の場合) など、ASP.NET によって提供される出力の既定の認証方式のいずれかを使用して現在ログインしているユーザーのロール情報と情報を BLL は、このようなアクセスをそれによってからアクセスできますプレゼンテーションと BLL レイヤーの両方で可能な権限を確認します。

## <a name="summary"></a>まとめ

ユーザー アカウントを提供するほとんどのサイトでは、ログインしているユーザーに基づいて、データ変更インターフェイスをカスタマイズする必要があります。 管理ユーザーは、管理者以外のユーザーのみを更新または自分で作成したレコードを削除するのにもありますが、削除、および任意のレコードを編集することがあります。 どのようなシナリオ、データ Web コントロールと ObjectDataSource、し、拒否、ログオン ユーザーに基づく特定の機能を追加するビジネス ロジック層のクラスを拡張することができます。 このチュートリアルでは、ユーザーが特定のサプライヤーに関連付けられていたかどうかや、企業の協力してかどうかに応じて表示して編集可能なデータを制限する方法を説明しました。

このチュートリアルでは、挿入、更新、および GridView、DetailsView、FormView コントロールを使用してデータを削除するマイクロソフトの調査で終了します。 以降、次のチュートリアルでは、ページングと並べ替えのサポートを追加することに注目有効にします。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](adding-client-side-confirmation-when-deleting-cs.md)
> [次へ](an-overview-of-inserting-updating-and-deleting-data-vb.md)
