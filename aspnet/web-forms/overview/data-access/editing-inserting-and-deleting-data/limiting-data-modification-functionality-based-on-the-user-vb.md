---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: (VB) のユーザーに基づくデータ変更機能を制限する |Microsoft ドキュメント
author: rick-anderson
description: ユーザー データを編集できるように、web アプリケーションを別のユーザー アカウントには別のデータ編集権限が必要です。 このチュートリアルで取り上げる方法 t しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: eccb8e114e0ecf772680a2e5b36a761736cf8025
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>(VB) のユーザーに基づくデータ変更機能を制限します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe)または[PDF のダウンロード](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> ユーザー データを編集できるように、web アプリケーションを別のユーザー アカウントには別のデータ編集権限が必要です。 このチュートリアルでは、アクセスしたユーザーに基づくデータ変更機能を動的に調整する方法について確認します。


## <a name="introduction"></a>はじめに

Web アプリケーションの数は、ユーザー アカウントをサポートし、さまざまなオプション、レポート、およびログインのユーザーに基づく機能を提供します。 たとえば、このチュートリアルで必要になるにログオンするため自社製品の名前と、単位あたりの数量のサイトと更新プログラムの全般情報など、-、会社名など、仕入先の情報と共に仕入れ先企業からユーザーを許可するにはアドレス、s の連絡先情報、およびなどです。 さらに、ログオン、株価、上のユニットが、レベルの順序を変更するなどと同様に、製品情報を更新できるように、会社の人の一部のユーザー アカウントを含める可能性があります。 Web アプリケーションも、匿名ユーザーは (ユーザーがログオンしていない) を参照してくださいが、データの表示だけを制限するとします。 このようなユーザー アカウント システムの場所に、Web コントロールのデータを挿入、編集、および削除、現在ログオンしているユーザーの適切な機能を提供する、ASP.NET ページでたいと思うでしょう。

このチュートリアルでは、アクセスしたユーザーに基づくデータ変更機能を動的に調整する方法について確認します。 具体的には、業者によって提供される製品を一覧表示する GridView と共に、編集可能な DetailsView の仕入先情報を表示するページを作成します。 会社からのページにアクセスしたユーザーの場合は、ように設定できます:; あらゆる業者の情報を表示ユーザーのアドレスを編集します。および業者によって提供される製品の情報を編集します。 のみできる場合、ただし、ユーザーは、特定の企業は、表示、独自のアドレス情報を編集および生産中止とマークされていない独自の製品のみを編集できます。


[![ポータルからユーザーが業者の情報を編集できます。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**図 1**: Our 会社できます編集 Any 業者の情報から A のユーザー ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![ユーザーが特定業者ことのみを表示および編集の情報](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**図 2**: ユーザーから、特定業者できますのみ表示および編集の情報 ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


開始 s を let!

> [!NOTE]
> ASP.NET 2.0 のメンバーシップ システムは、作成、管理、およびユーザー アカウントを検証するための標準化された、拡張可能なプラットフォームを提供します。 メンバーシップ システムの検査がこれらのチュートリアルの範囲を超えているため、このチュートリアル代わりに"fakes"メンバーシップにより、匿名のユーザーが会社からまたはの特定のサプライヤーから供給されるかどうかを選択します。 詳細については、メンバーシップを参照してください  [ASP.NET 2.0 の検査 s メンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)一連の記事です。


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>手順 1: ユーザーがアクセス権を指定するを許可します。

実際の web アプリケーションで、社内用または特定の仕入先の作業を行い、この情報にユーザーがサイトにログオンした後、ASP.NET ページからプログラムでアクセスするかどうかユーザーのアカウント情報が含まれます。 プロファイル システム、またはカスタム手段を通じてユーザー レベルのアカウント情報として、ASP.NET 2.0 のロール システムを介してこの情報をキャプチャすることができます。

このチュートリアルの目的は、ログオン ユーザーに基づくデータ変更機能を調整することを示すためには、ASP.NET 2.0 の showcase s メンバーシップ、ロール、およびプロファイル システムへのものではありませんので、非常に簡単にメカニズムを使用してを決定します機能ページ - ユーザー元となるかどうかがありますを表示および編集 suppliers 情報や、別の方法として、新機能のいずれかを示すことができます DropDownList にアクセスしたユーザーを特定の仕入先の情報を表示および編集します。 ユーザーがいる彼女を表示および編集 (既定値) のすべての仕入先情報を示している場合は、all 内でページを業者 s 任意アドレスについてを編集し、選択した業者によって提供される製品の単位あたりの数量と名前を編集彼女ことができます。 ユーザーが自分だけを表示できますおよび編集し、ただしの特定の仕入先、彼女はのみその 1 つの仕入先の詳細および製品を表示でき、のみ更新いるユニットの情報は、これらの製品ごとの数と名前を示す場合*いない*廃止されました。

このチュートリアルでは、最初の手順は、次に、この DropDownList を作成し、サプライヤーとシステムの設定にです。 開いている、 `UserLevelAccess.aspx`  ページで、`EditInsertDelete`フォルダー、DropDownList の追加が`ID`プロパティに設定されている`Suppliers`、という名前の新しい ObjectDataSource をこの DropDownList をバインド`AllSuppliersDataSource`です。


[![AllSuppliersDataSource をという名前の新しい ObjectDataSource を作成します。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**図 3**: 名前付き新しい ObjectDataSource 作成`AllSuppliersDataSource`([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


すべての仕入先を含めるには、この DropDownList ので、構成を呼び出す ObjectDataSource、`SuppliersBLL`クラスの`GetSuppliers()`メソッドです。 ObjectDataSource s も確認`Update()`メソッドのマップ先、`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッドでは、この ObjectDataSource としても使用されますが追加されます。 手順 2 で DetailsView でします。

ObjectDataSource ウィザードの完了後の手順を構成して、 `Suppliers` DropDownList が表示されるよう、`CompanyName`使用してデータ フィールド、`SupplierID`データ フィールドの各値として`ListItem`です。


[![CompanyName と SupplierID データ フィールドを使用する仕入先の DropDownList を構成します。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**図 4**: 構成、 `Suppliers` DropDownList で使用する、`CompanyName`と`SupplierID`データ フィールド ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


この時点で、DropDownList では、データベースの仕入先の会社名が一覧表示します。 ただしも含める必要は DropDownList する"表示/編集 All"オプション。 これを実現するには設定、 `Suppliers` DropDownList s`AppendDataBoundItems`プロパティを`true`し、追加、`ListItem`が`Text`プロパティは、"表示/編集 All"を値`-1`です。 これによって追加できます宣言型マークアップを使用して直接、またはデザイナーを使用しようとして、[プロパティ] ウィンドウと DropDownList s で省略記号ボタンをクリックすると`Items`プロパティです。

> [!NOTE]
> 戻って、 [*マスター/詳細のフィルター処理で、DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)の詳細については、すべての項目をデータ バインディングの DropDownList に追加するためのチュートリアルです。


後に、`AppendDataBoundItems`プロパティが設定されていると、`ListItem`追加されると、DropDownList s の宣言型マークアップのようになります。


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

図 5 は、ブラウザーで表示したときに、現在の進行状況のスクリーン ショットを示します。


[![Suppliers DropDownList が含まれています、表示にはすべて ListItem には、各仕入先の 1 つには](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**図 5**: `Suppliers` DropDownList がすべて表示が含まれています`ListItem`、および 1 つの各仕入先 ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


ユーザーには、選択項目が変更された直後後に、ユーザー インターフェイスを更新するので、設定、 `Suppliers` DropDownList s`AutoPostBack`プロパティを`true`です。 手順 2 での DropDownList の選択に基づくサプライヤーの情報を表示する DetailsView コントロールを作成します。 次に、手順 3 を作成してこの DropDownList s のイベント ハンドラーを`SelectedIndexChanged`を追加、選択した業者に基づいて DetailsView を適切な仕入先の情報をバインドするコードのイベントです。

## <a name="step-2-adding-a-detailsview-control"></a>手順 2: DetailsView コントロールの追加

S、DetailsView を使用して仕入先の情報を表示することができます。 ユーザーに表示し、すべての仕入先の編集、DetailsView はページングをサポートして、一度に手順業者について 1 つのレコードを実行するユーザーを許可します。 場合は、ユーザーは、特定の業者の動作、ただし、DetailsView がその仕入先を特定の情報が表示されます、およびページング インターフェイスは含まれません。 どちらの場合は、DetailsView を仕入先のアドレス、市区町村、都道府県 フィールドを編集するユーザーを許可する必要があります。

DetailsView を下にあるページに追加、 `Suppliers` DropDownList、設定、`ID`プロパティを`SupplierDetails`にバインドし、 `AllSuppliersDataSource` ObjectDataSource が前の手順で作成します。 DetailsView s のスマート タグからページングを有効にして、編集を有効にするチェック ボックスを次に、確認します。

> [!NOTE]
> スマート DetailsView s の編集を有効にするオプションが表示されない場合にタグ付け、s、ObjectDataSource s をマップしなかったため`Update()`メソッドを`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッドです。 すぐに戻って、この構成をその後編集を有効にするオプションが DetailsView s のスマート タグで表示する必要がありますを変更します。


以降、`SuppliersBLL`クラス s`UpdateSupplierAddress`メソッドは、4 つのパラメーターのみを受け入れます`supplierID`、 `address`、 `city`、および`country`-DetailsView の BoundFields を変更できるように、`CompanyName`と`Phone`BoundFields は読み取り専用です。 さらに、削除、 `SupplierID` BoundField を完全です。 最後に、 `AllSuppliersDataSource` ObjectDataSource は現在はその`OldValuesParameterFormatString`プロパティに設定`original_{0}`です。 すぐ、宣言構文全体からこのプロパティの設定を削除するか、既定値に設定を`{0}`です。

構成した後、 `SupplierDetails` DetailsView と`AllSuppliersDataSource`ObjectDataSource が、次の宣言型マークアップ。


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

この時点で DetailsView にもポケットベル経由およびで選択した内容に関係なく、選択した仕入先のアドレス情報が更新されることができます、 `Suppliers` DropDownList (図 6 を参照してください)。


[![仕入先情報を表示できます、およびそのアドレスを更新](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**図 6**: Any 仕入先情報を表示できます、およびそのアドレスを更新 ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>手順 3: 情報を表示するのみ、選択したサプライヤー s

このページが現在から特定の仕入先が選択されているかどうかに関係なくすべての仕入先の情報を表示、 `Suppliers` DropDownList です。 選択した業者の仕入先の情報だけを表示するために、1 ページ目に、特定の仕入先に関する情報を取得する別の ObjectDataSource を追加する必要があります。

新しい ObjectDataSource ページを追加、名前を付け、`SingleSupplierDataSource`です。 スマート タグからデータ ソースの構成のリンクをクリックしてそれを使用して、`SuppliersBLL`クラスの`GetSupplierBySupplierID(supplierID)`メソッドです。 同様、 `AllSuppliersDataSource` ObjectDataSource が、 `SingleSupplierDataSource` ObjectDataSource s`Update()`にマップされるメソッド、`SuppliersBLL`クラスの`UpdateSupplierAddress`メソッドです。


[![GetSupplierBySupplierID(supplierID) メソッドを使用して SingleSupplierDataSource ObjectDataSource を構成します。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**図 7**: 構成、 `SingleSupplierDataSource` ObjectDataSource を使用する、`GetSupplierBySupplierID(supplierID)`メソッド ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


次に、パラメーターのソースを指定する re お求め、`GetSupplierBySupplierID(supplierID)`メソッドの`supplierID`入力パラメーターです。 使用して、DropDownList から選択した仕入先の情報を表示するので、 `Suppliers` DropDownList の`SelectedValue`プロパティ パラメーターのソースとして。


[![Suppliers DropDownList を supplierID パラメーターのソースとして使用します。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**図 8**: を使用して、`Suppliers`として DropDownList、`supplierID`パラメーター ソース ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


DetailsView コントロールに現在常に使用する構成されている 2 つ目の ObjectDataSource この追加を使用する場合でも、 `AllSuppliersDataSource` ObjectDataSource です。 に応じて DetailsView で使用されるデータ ソースを調整するためのロジックを追加する必要があります、 `Suppliers` DropDownList 項目を選択します。 これを実現する、作成、`SelectedIndexChanged`サプライヤーの DropDownList のイベント ハンドラー。 これは、デザイナーでの DropDownList をダブルクリックすると最も簡単に作成できます。 このイベント ハンドラーは、使用するには、どのようなデータ ソースを決定する必要があり、DetailsView にデータを再バインドする必要があります。 これは、次のコードで行われます。


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

「すべてサプライヤーの表示/編集」オプションを選択したかどうかを決定することにより、イベント ハンドラーを開始します。 いた場合は、設定、 `SupplierDetails` DetailsView s`DataSourceID`に`AllSuppliersDataSource`し、業者のセットの最初のレコードを設定してユーザーを返します、`PageIndex`プロパティを 0 にします。 ただし、ユーザーが選択特定のサプライヤーから DropDownList を DetailsView s 場合は、`DataSourceID`に割り当てられている`SingleSuppliersDataSource`です。 どのようなデータに関係なくソースを使用する、`SuppliersDetails`モードが読み取り専用モードに戻されていてへの呼び出しによってデータが DetailsView にバインドし直す、`SuppliersDetails`コントロールの`DataBind()`メソッドです。

インプレースのこのイベント ハンドラーと DetailsView コントロールでは、仕入先のすべてを参照して、ページング インターフェイスを通じている場合、「すべてサプライヤーの表示/編集」オプションを選択した場合を除き、仕入先の選択したが今すぐ表示されます。 図 9 に、「すべてサプライヤーの表示/編集」オプションを選択して; ページを示しますユーザーにアクセスし、プロバイダーを更新できるようにするページング インターフェイスが存在することを確認します。 図 10 では、選択 Ma 商店株式会社業者で、ページを示します。 のみの Ma 商店株式会社 s の情報は、ここで表示および編集可能です。


[![表示および編集するには、すべての仕入先情報](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**図 9**: すべての仕入先情報を表示でき、編集された ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![選択したサプライヤーの情報のみを表示および編集できます。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**図 10**: のみ Viewed および編集された仕入先の選択されている s 情報が表示できます ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> このチュートリアルでは、DropDownList と DetailsView の両方を制御 s`EnableViewState`に設定する必要があります`true`(既定) ための DropDownList s`SelectedIndex`と DetailsView の`DataSourceID`ポストバック間で s プロパティの変更を記録する必要があります。


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>手順 4:、編集可能な GridView のサプライヤー製品が一覧表示します。

完了の DetailsView で次の手順は、選択した業者によって提供されるこれらの製品を一覧表示する編集可能な GridView を含めるにです。 この GridView はのみに編集を許可する必要があります、`ProductName`と`QuantityPerUnit`フィールドです。 さらに、特定の業者からのページにアクセスしたユーザーの場合は、のみを許可する更新プログラムは、これらの製品を*いない*廃止されました。 最初のオーバー ロードを追加する必要がありますこれを実現する、`ProductsBLL`クラス s`UpdateProducts`で受け取るメソッドだけを`ProductID`、 `ProductName`、および`QuantityPerUnit`入力としてのフィールドです。 お ve 事前は多数のチュートリアルで、このプロセスをステップ インはそこでのみコードを確認して、ここに追加しなければならない秒`ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

このオーバー ロードに作成されると、お GridView コントロールと関連付けられている、ObjectDataSource を追加する準備が整いました。 新しい GridView をページに追加、設定、`ID`プロパティを`ProductsBySupplier`、という名前の新しい ObjectDataSource を使用するように構成および`ProductsBySupplierDataSource`です。 選択した業者によってこれらの製品を一覧表示するには、この GridView のでを使用して、`ProductsBLL`クラスの`GetProductsBySupplierID(supplierID)`メソッドです。 マップすることも、`Update()`を新しいメソッド`UpdateProduct`作成したばかりのオーバー ロードします。


[![先ほど作成した UpdateProduct オーバー ロードを使用する ObjectDataSource を構成します。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**図 11**: 構成を使用する ObjectDataSource、`UpdateProduct`先ほど作成した、オーバー ロード ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


パラメーターのソースを選択するよう再求め、`GetProductsBySupplierID(supplierID)`メソッドの`supplierID`入力パラメーターです。 使用して、DetailsView で選択した業者の製品を表示するので、 `SuppliersDetails` DetailsView コントロールの`SelectedValue`パラメーターのソースとしてのプロパティです。


[![SuppliersDetails DetailsView の SelectedValue プロパティ パラメーターのソースとして使用します。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**図 12**: を使用して、 `SuppliersDetails` DetailsView s`SelectedValue`パラメーターのソースとしてのプロパティ ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


除く GridView フィールドのすべてを削除する GridView に返す`ProductName`、 `QuantityPerUnit`、および`Discontinued`、マーク、 `Discontinued` CheckBoxField 読み取り専用とします。 また、GridView s のスマート タグの編集を有効にするオプションを確認します。 これらの変更が完了したら、ObjectDataSource、GridView の宣言型マークアップは、次のようになります。


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

当社以前 ObjectDataSources この 1 つの s と同様に`OldValuesParameterFormatString`プロパティに設定されている`original_{0}`s の製品名や単位あたりの数量を更新しようとするときに問題が発生します。 またはその既定値に設定する宣言構文からこのプロパティを完全に削除`{0}`です。

この構成が完了、ページに表示されます、GridView で選択した業者によって提供される製品 (図 13 を参照してください)。 現在*任意*s の製品名、単位あたりの数量を更新することができます。 ただし、特定の供給業者に関連付けられているユーザーの提供が中止された製品のような機能が禁止されているように、ページのロジックを更新する必要があります。 この最後のピースは手順 5. で取り上げるです。


[![選択した供給業者によって提供される製品が表示されます。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**図 13**: 選択業者によって提供される、製品が表示されます ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> 追加すると、この編集可能な GridView、 `Suppliers` DropDownList の`SelectedIndexChanged`GridView を読み取り専用の状態に戻るにはイベント ハンドラーを更新する必要があります。 それ以外の場合、製品情報を編集の最中に別のサプライヤーを選択すると、新しい業者の GridView の対応するインデックスも編集できます。 これを防ぐためには、単に設定 GridView s`EditIndex`プロパティを`-1`で、`SelectedIndexChanged`イベント ハンドラー。


また、s ビューステートする GridView に (既定の動作) が有効になっている重要であることに注意してください。 GridView s を設定した場合`EnableViewState`プロパティを`false`、同時実行ユーザーが誤って削除または編集を記録する危険を実行します。 参照してください[警告: 同時実行の問題と ASP.NET 2.0 Gridview/DetailsView/FormViews その編集をサポートするか削除すると、をビュー ステートが無効になっている](http://scottonwriting.net/sowblog/posts/10054.aspx)詳細についてはします。

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>手順 5: 廃止された製品と表示/編集 All が選択されていない編集を許可しません。

中に、 `ProductsBySupplier` GridView は、完全に機能は、現在過剰にアクセスを許可特定の仕入先からであるユーザー。 当社のビジネス ルールに従ってこのようなユーザーいないはず提供が中止された製品を更新します。 これを適用するのにお非ことができます (を無効にする)、業者から、ユーザーが、ページが参照されているときに提供が中止された製品と、GridView 行の編集 ボタン。

GridView s のイベント ハンドラーを作成`RowDataBound`イベント。 このイベント ハンドラーで、ユーザーがこのチュートリアルでは、サプライヤー DropDownList s をチェックして決定できますを特定の供給業者に関連付けられたかどうかを判断する必要があります`SelectedValue`プロパティの場合はそれ以外の何か-1 で、そのユーザーは特定の供給業者に関連付けられました。 このようなユーザー、製品が提供が中止されたかどうかを決定する必要があります。 実際への参照を取得して`ProductRow`インスタンスを使用して GridView の行にバインド、`e.Row.DataItem`で説明したように、プロパティ、 [ *GridView のフッターで概要情報を表示する*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md)チュートリアルです。 GridView s CommandField 前のチュートリアルで説明した手法を使用して [編集] ボタンをプログラムによって参照を取得できるお場合は、製品が廃止されましたが、 [*を追加するクライアント側の確認時に削除する*](adding-client-side-confirmation-when-deleting-vb.md). お非ことができますし、ボタンを無効にする参照を作成したらです。


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

このイベントにハンドラーが、配置訪問このページのユーザーとして特定の仕入先から提供が中止されたこれらの製品は、編集できません [編集] ボタンとしては非表示にこれらの製品。 たとえば、Chef Anton の Gumbo ミックスは、新規の Orleans Cajun コーヒー供給業者の提供が中止された製品です。 この特定の仕入先のページを訪問する際にこの製品の編集 ボタンは表示されません視認 (図 14 を参照してください)。 ただし、"表示/編集すべて Suppliers"を使用してへのアクセス、ときに [編集] ボタンは使用可能なものであるか (参照図 15)。


[![供給業者に固有のユーザーの Chef Anton の Gumbo ミックスの編集 ボタンが非表示します。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**図 14**: のサプライヤー固有のユーザー Chef Anton の Gumbo ミックスの編集 ボタンが非表示 ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![表示/編集 Suppliers、すべてのユーザーの Chef Anton の Gumbo ミックスの編集 ボタンが表示されます。](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**図 15**: の表示/編集すべて仕入先のユーザー、Gumbo ミックスが表示される Chef Anton s の編集 ボタン ([フルサイズのイメージを表示するをクリックして](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>ビジネス ロジック層でのアクセス権のチェック

このチュートリアルは、ASP.NET でページによってに関してどのような情報をユーザーに表示できるすべてのロジックが処理および更新はどのような製品です。 理想的には、このロジックもビジネス ロジック層で存在できます。 たとえば、`SuppliersBLL`クラス s`GetSuppliers()`メソッド (これは、すべての仕入先が返されます) は、現在ログオンしているユーザーがあることを確認するチェックを含めることが*いない*特定の仕入先に関連付けられています。 同様に、`UpdateSupplierAddress`メソッドがいることを確認するか会社に費やされた作業 (および仕入先のすべてのアドレス情報を更新できます)、現在ログオンしてユーザーにチェックを含めることがまたは更新されるデータが供給業者に関連付けられています。

含まれていません BLL レイヤーそのようなチェックをここでこのチュートリアルでは、ユーザーの権利が BLL クラスにアクセスできない ページでの DropDownList によって決定されるためです。 メンバーシップ システムまたは (Windows 認証の場合) など、ASP.NET によって提供される出力の既定の認証方式のいずれかを使用して、現在ログオンしているユーザーに情報やロール情報をこのようなアクセスをし、それによって、BLL からアクセスできます。プレゼンテーションと BLL レイヤーの両方で可能な権限をチェックします。

## <a name="summary"></a>まとめ

ユーザー アカウントを提供するほとんどのサイトは、ログインしたユーザーに基づくデータ変更のインターフェイスをカスタマイズする必要があります。 管理者以外のユーザーのみを更新または自分で作成したレコードを削除するのに制限できますが、管理ユーザーは削除、および任意のレコードを編集することがあります。 どのようなシナリオがあります、ObjectDataSource を Web コントロールのデータと、拒否、ログオン ユーザーに基づく特定の機能を追加またはビジネス ロジック層クラスを拡張することができます。 このチュートリアルでは、ユーザーが特定の供給業者に関連付けられているかどうか、または、当社の作業したかどうかによっては表示および編集可能なデータを制限する方法を説明しました。

このチュートリアルでは、挿入、更新、および GridView、DetailsView、およびフォームのコントロールを使用してデータを削除するマイクロソフトの調査で終了します。 以降、次のチュートリアルでは、ページングや並べ替えのサポートを追加することに注目有効にします。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](adding-client-side-confirmation-when-deleting-vb.md)
