---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: (C#) を削除するときに、クライアント側の確認を追加する |Microsoft Docs
author: rick-anderson
description: これまでに作成したインターフェイスで、ユーザーは編集ボタンをクリックするためのものと削除 ボタンをクリックしてデータを誤って削除できます。 この t.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 7ad66e24198a83d32edeb9fddf7a6b648b993fcc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386563"
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>(C#) を削除するときに、クライアント側の確認を追加します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe)または[PDF のダウンロード](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> これまでに作成したインターフェイスで、ユーザーは編集ボタンをクリックするためのものと削除 ボタンをクリックしてデータを誤って削除できます。 このチュートリアルでは、Delete ボタンがクリックされたときに表示されるクライアント側の確認 ダイアログ ボックスを追加します。


## <a name="introduction"></a>はじめに

過去のいくつかのチュートリアルで ve 連携して、挿入、編集、および削除機能を提供する、アプリケーションのアーキテクチャ、ObjectDataSource、およびデータ Web コントロールを使用する方法について説明します。 確認したらインターフェイスを削除するきませんでしたが、削除で構成されたクリックされたときに、ポストバックが発生する ObjectDataSource s を呼び出すボタンであり、`Delete()`メソッド。 `Delete()`メソッドは、実際の発行、データ アクセス層に呼び出しを伝達するビジネス ロジック層から構成されているメソッドを呼び出します`DELETE`ステートメントはデータベースにします。

このユーザー インターフェイスでは、訪問者を GridView、DetailsView、または FormView コントロールを使用してレコードを削除することが、その削除 ボタンをクリックすると、何らかの確認が不足しています。 ユーザーが誤ってクリックした場合 をクリックするもので、ときに、削除 ボタンの編集、レコードを更新するためのものがインストールされます。 代わりにします。 このチュートリアルでは、これを防ぐため、削除ボタンがクリックされたときに表示されるクライアント側の確認 ダイアログ ボックスを追加します。

JavaScript`confirm(string)`関数は、[ok] の 2 つのボタンに搭載されていて、(図 1 参照) をキャンセルするモーダル ダイアログ ボックス内のテキストとして、文字列入力パラメーターを表示します。 `confirm(string)`関数によってどのようなボタンがクリックされたブール値を返します (`true`、ユーザーが [ok] をクリックすると、および`false`[キャンセル] をクリックした場合)。


![JavaScript confirm(string) メソッドは、モーダルのクライアント側のメッセージ ボックスを表示します](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**図 1**: JavaScript`confirm(string)`メソッドは、クライアント側のモーダルのメッセージ ボックスを表示します。


値の場合、フォームの送信中に`false`が、フォームの送信をキャンセルし、クライアント側のイベント ハンドラーから返されます。 この機能を使用する場合は、この削除ボタン s のクライアント側ができる`onclick`イベント ハンドラーの呼び出しの値を返す`confirm("Are you sure you want to delete this product?")`します。 ユーザーが [キャンセル] をクリックすると`confirm(string)`原因と、フォームの送信をキャンセルするため、false が返されます。 ポストバックと t を受注の Delete ボタンがクリックされた製品が削除されます。 ただし、ユーザーは確認のダイアログ ボックスで [ok] をクリックすると場合、は、ポストバックがに伴って続行され、製品が削除されます。 参照してください[を使用して JavaScript s`confirm()`フォームの送信を制御するメソッド](http://www.webreference.com/programming/javascript/confirm/)この手法の詳細についてはします。

必要なクライアント側スクリプトを追加することが若干を [commandfield] を使用するときのテンプレートを使用する場合。 そのため、このチュートリアルでは、FormView や GridView の両方の例を見ていますが。

> [!NOTE]
> クライアント側の確認方法を使用して、このチュートリアルで説明しているようで、ユーザーが JavaScript をサポートするブラウザーでアクセスしているし、JavaScript が有効であること。 特定のユーザーの場合は true。 これらの前提条件のいずれかの場合は、削除ボタンをクリックするとはすぐがポストバックを (確認メッセージ ボックスが表示されない)。


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>手順 1: は、削除をサポートする、フォーム ビューを作成します。

フォーム ビューを追加して、開始、`ConfirmationOnDelete.aspx`ページで、`EditInsertDelete`フォルダー、バックアップを使用して製品情報をプルする新しい ObjectDataSource にバインドすること、`ProductsBLL`クラスの`GetProducts()`メソッド。 ObjectDataSource をまた構成ように、`ProductsBLL`クラス s `DeleteProduct(productID)` ObjectDataSource s メソッドにマップされて`Delete()`メソッド; INSERT および UPDATE のタブのドロップダウン リストが (None) に設定されていることを確認します。 最後に、FormView s のスマート タグでページングを有効にするチェック ボックスを確認します。

これらの手順の後は、新しい ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

オプティミスティック同時実行制御を使用していない、過去例のように少し ObjectDataSource s をクリアするため`OldValuesParameterFormatString`プロパティ。

削除、FormView s のみをサポートする ObjectDataSource コントロールにバインドされているため`ItemTemplate`のみの削除 ボタン、新機能と更新プログラムのボタンのないを提供します。 ただし、FormView s の宣言型マークアップが、余分な含まれ`EditItemTemplate`と`InsertItemTemplate`、これを削除することができます。 カスタマイズする少し、`ItemTemplate`ができるので、製品のサブセットのみのデータ フィールドを示しています。 私で製品の名前を表示するように構成 ve、 `<h3>` (削除) と共にその業者とカテゴリ名の上の見出しです。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

これらの変更は、ユーザーを削除 ボタンをクリックするだけで、製品を削除することができます、一度に 1 つの製品を通じて切り替えるできる web ページを完全に機能があります。 図 2 は、ブラウザーで表示したときにこれまで、進行状況のスクリーン ショットを示します。


[![FormView には、1 つの製品についての情報が表示されます。](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**図 2**:、FormView 表示情報について、1 つの製品 ([フルサイズの画像を表示する をクリックします](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))。


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>手順 2: は、削除ボタン クライアント側の onclick イベントから confirm(string) 関数を呼び出す

最後の手順は、FormView を作成すると、このような削除ボタンを構成するときに、s が、JavaScript、訪問者によってクリックされた`confirm(string)`関数が呼び出されます。 ボタンや LinkButton、ImageButton のクライアント側へのクライアント側スクリプトの追加`onclick`イベントの使用により実現できます、 `OnClientClick property`、これは ASP.NET 2.0 に新しいします。 値が必要なため、`confirm(string)`このプロパティを設定する関数が返されるだけです。 `return confirm('Are you certain that you want to delete this product?');`

この変更後に削除 LinkButton s の宣言型構文ようになります。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

すべてが s で終了です。 図 3 は、アクションでこの確認のスクリーン ショットを示します。 削除ボタンをクリックすると、[確認] ダイアログ ボックスが表示されます。 ユーザーは、[キャンセル] をクリックすると、ポストバックがキャンセルされ、製品は削除されません。 ポストバックの継続のかどうか、ただし、ユーザーは、[ok] をクリックして、および ObjectDataSource の`Delete()`メソッドが呼び出される、データベースのレコードを削除中に使用します。

> [!NOTE]
> 渡された文字列、 `confirm(string)` JavaScript 関数は、アポストロフィ (引用符ではなく) で区切られます。 JavaScript ではいずれかの文字を使用して文字列を区切ることができます。 渡された文字列の区切り記号のようにアポストロフィここで使用`confirm(string)`を使用する区切り記号では、あいまいさを持ち込んでいない、`OnClientClick`プロパティの値。


[![確認メッセージは、今すぐ表示と削除 ボタンをクリックして](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**図 3**: A 確認は、今すぐ表示と削除 ボタンをクリックして ([フルサイズの画像を表示する をクリックします](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))。


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>手順 3: が、[commandfield] で、Delete ボタン OnClientClick プロパティを構成します。

確認のダイアログ ボックスを構成するだけでそれに関連付けられたすることができます、テンプレートで直接、ボタン、LinkButton、ImageButton に作業するとき、 `OnClientClick` 、JavaScript の結果を返すプロパティ`confirm(string)`関数。 ただし、フィールドの削除 ボタンを追加すると、GridView、DetailsView - が commandfield はありません、`OnClientClick`宣言によって設定できるプロパティです。 代わりに、適切な GridView、DetailsView s に削除 ボタンを参照する必要がありますプログラムで`DataBound`イベント ハンドラー、および設定してその`OnClientClick`プロパティがあります。

> [!NOTE]
> S [削除] ボタンを設定するときに`OnClientClick`プロパティで、適切な`DataBound`へのアクセスがあるイベント ハンドラーでは、データは、現在のレコードにバインドされました。 つまり、「は Chai 製品を削除しますか?」など、特定のレコードの詳細を含めるに確認メッセージを拡張できます。 このようなカスタマイズは、データ バインディング構文を使用してテンプレートのこともできます。


実際に設定する、 `OnClientClick` CommandField、let s に削除ボタンのプロパティ ページに GridView を追加します。 この GridView、FormView を使用する同じ ObjectDataSource コントロールの使用を構成します。 GridView s のみ製品の名前、カテゴリ、および仕入先が含まれる BoundFields 制限もできます。 最後に、GridView s のスマート タグからの削除を有効にするチェック ボックスを確認します。 GridView s に、[commandfield] を追加します`Columns`コレクションとその`ShowDeleteButton`プロパティに設定`true`します。

これらの変更を行った後、次のように GridView s の宣言型マークアップになります。


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

[Commandfield] には、GridView s からプログラムでアクセスできる 1 つ削除 LinkButton のインスタンスが含まれています。`RowDataBound`イベント ハンドラー。 設定できる、参照とその`OnClientClick`プロパティに応じて。 イベント ハンドラーを作成、`RowDataBound`を次のコードを使用してイベント。


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

このイベント ハンドラーは、データ行 (削除 ボタンを持つもの) と連携し、プログラムで、削除 ボタンを参照して開始します。 [全般] では、次のパターンを使用します。


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* [commandfield] のボタンや LinkButton、ImageButton によって使用されているボタンの種類です。 既定では、[commandfield] は、Linkbutton を使用してが、これは、[commandfield] s からカスタマイズできます`ButtonType property`します。 *CommandFieldIndex* GridView s 内で [commandfield] の序数インデックス`Columns`、コレクションは、 *controlIndex* [commandfield] s 内の Delete ボタンのインデックス`Controls`コレクション。 *ControlIndex*値は、[commandfield] でその他のボタンを基準としたボタン s の位置に依存します。 たとえば、唯一のボタン、[commandfield] に表示されますが、削除ボタンの場合は、インデックスは 0 を使用します。 かどうか、ただしが s 削除 ボタンの前の編集 ボタンを使用して、インデックスの 2。 2 のインデックスを使用する理由は Delete ボタンの前に [commandfield] によって 2 つのコントロールが追加されたためです。 [編集] ボタンと LiteralControl s、編集、削除ボタンの間にスペースを追加するために使用します。

この特定の例で、[commandfield] Linkbutton を使用して、います、左端のフィールドを*commandFieldIndex*は 0。 使用して他のボタンはありませんが、[commandfield] で [削除] ボタンがある、ので、 *controlIndex*は 0。

後、[commandfield] で [削除] ボタンを参照するには、次に GridView の現在の行にバインドされている製品についての情報を取得します。 最後に、秒の削除 ボタンを設定します`OnClientClick`プロパティを適切な JavaScript で製品の名前が含まれています。 JavaScript 文字列が渡されるため、`confirm(string)`関数は、アポストロフィ、s の製品名内に表示される任意のアポストロフィはエスケープする必要がありますを使用して区切られます。 S の製品名にアポストロフィをエスケープする具体的には、"`\'`"。

これらの変更は完全な GridView が表示されます (図 4 参照)、カスタマイズされた確認ダイアログ ボックスで [削除] ボタンをクリックします。 として、FormView から確認メッセージ ボックスと、ユーザーが [キャンセル] をクリックした場合、ポストバックが取り消された場合、発生してから、削除できないようにすること。

> [!NOTE]
> この手法が、DetailsView で [commandfield] で [削除] ボタンをプログラムでアクセスすることもできます。 DetailsView、ただし、d を作成、イベント ハンドラーを`DataBound`DetailsView があるないため、イベント、`RowDataBound`イベント。


[![GridView の削除 ボタンをクリックすると、カスタマイズされた確認ダイアログ ボックスが表示されます。](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**図 4**: カスタマイズの確認 ダイアログ ボックスを表示する GridView の削除 ボタンをクリックすると ([フルサイズの画像を表示する をクリックします](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))。


## <a name="using-templatefields"></a>TemplateFields を使用します。

[Commandfield] の欠点の 1 つは、そのボタンは、インデックスを通じてアクセスする必要があり、結果のオブジェクトは、適切なボタンの種類 (ボタンや LinkButton、ImageButton) にキャストする必要があります。 実行時まで検出できない問題への招待の「マジック ナンバー」およびハード コーディングされた型を使用します。 たとえば、または別の開発者は、いくつかの時点を将来 (Edit ボタン) などの変更で [commandfield] に新しいボタンを追加する場合、`ButtonType`プロパティ、既存のコードは、エラーを発生させずはコンパイルされますが、例外が発生する可能性があります、ページにアクセスまたは、コードの記述方法と、加えられた変更によって、予期しない動作します。

別のアプローチでは、TemplateFields GridView および DetailsView のコマンドに変換します。 これを TemplateField が生成されます、 `ItemTemplate` LinkButton (またはボタンまたは ImageButton) を持つ各ボタン、[commandfield] にします。 これらのボタン`OnClientClick`、フォーム ビューで表示した場合、または適切なプログラムでアクセスできるに、プロパティを宣言によって、割り当てられることができます`DataBound`イベント ハンドラーを次のパターンを使用します。


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

場所*controlID*ボタン秒の値は、`ID`プロパティ。 このパターンには、キャストのハード コーディングされた型も必要ですが、削除、インデックス作成の必要性ランタイム エラーが発生せずに変更をレイアウトすることができます。

## <a name="summary"></a>まとめ

JavaScript`confirm(string)`関数は、フォームの送信を制御するための一般的に使用される手法です。 実行すると、関数には、2 つのボタン、[ok] と [キャンセル] を含む、クライアント側のモーダル ダイアログ ボックスが表示されます。 ユーザーが [ok] をクリックすると、`confirm(string)`関数が返される`true`; [キャンセル] をクリックすると戻ります`false`します。 この機能は、送信処理中に、イベント ハンドラーによって返された場合は、フォームの送信をキャンセルするブラウザーの動作を組み合わせて`false`レコードを削除するときに、確認メッセージ ボックスを表示するために使用できます。

`confirm(string)`関数は、ボタンの Web コントロール s のクライアント側を関連付けることにより`onclick`s コントロールを使用してイベント ハンドラー`OnClientClick`プロパティ。 テンプレート - または - GridView、DetailsView で TemplateField FormView のテンプレートのいずれかで [削除] ボタンを使用する場合このプロパティ宣言またはプログラムでは、このチュートリアルで説明したようにします。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](implementing-optimistic-concurrency-cs.md)
> [次へ](limiting-data-modification-functionality-based-on-the-user-cs.md)
