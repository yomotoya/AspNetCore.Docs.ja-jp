---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: (VB) データベースで CascadingDropDown を使用する |Microsoft Docs
author: wenz
description: CascadingDropDown コントロール、AJAX Control Toolkit では、anoth 内の値が 1 つの DropDownList の読み込みの変更に関連付けられているように DropDownList コントロールを拡張しています.
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 48e52fce4e26ec08403c3d86a4c3f85c103baa82
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824232"
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>(VB) データベースで CascadingDropDown を使用します。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードのダウンロード](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。 これが機能するためには、特別な web サービスを作成する必要があります。


## <a name="overview"></a>概要

CascadingDropDown コントロール、AJAX Control Toolkit では、もう 1 つの DropDownList の値が 1 つの DropDownList の読み込みの変更に関連付けられているように、DropDownList コントロールを拡張します。 (たとえば、1 つのリストは、私たちの状態の一覧を提供します。 し、[次へ] の一覧は、その状態の主な都市で埋められます)。これが機能するためには、特別な web サービスを作成する必要があります。

## <a name="steps"></a>手順

まず、データ ソースが必要です。 このサンプルでは、AdventureWorks データベースと Microsoft SQL Server 2005 Express Edition を使用します。 データベース (express edition を含む)、Visual Studio のインストールのオプションの一部であるし、下の個別のダウンロードとしても利用[ https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)します。 AdventureWorks データベースが SQL Server 2005 サンプルとサンプル データベースの一部 (ダウンロード[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 データベースをセットアップする最も簡単な方法は、Microsoft SQL Server Management Studio Express を使用する、([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) とアタッチ、`AdventureWorks.mdf`データベース ファイル。

このサンプルでは、SQL Server 2005 Express Edition のインスタンスが呼び出されること前提としています`SQLEXPRESS`は web サーバーと同じコンピューター上に存在して、既定の設定にもなります。 場合は、セットアップが異なる場合は、データベースの接続情報を調整する必要があります。

ASP.NET AJAX Control Toolkit の機能をアクティブ化するために、`ScriptManager`コントロールは、ページのどこでも配置する必要があります (ただし内、 &lt; `form` &gt;要素)。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

次の手順では、2 つの DropDownList コントロールが必要です。 このサンプルでは、AdventureWorks のベンダーと連絡先情報を使用、ため利用可能なベンダーと使用できる連絡先のいずれか 1 つのリストを作成します。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

次に、2 つの CascadingDropDown エクステンダーは、ページに追加する必要があります。 最初の (ベンダー) リストを塗りつぶす 1 つと、もう 1 つの入力 (連絡先) の 2 番目の一覧。 次の属性を設定する必要があります。

- `ServicePath`: 一覧のエントリを提供する web サービスの URL
- `ServiceMethod`: Web メソッドの一覧のエントリを提供します。
- `TargetControlID`。 ドロップダウン リストの ID
- `Category`: Web メソッドが呼び出されたときに送信されたカテゴリの情報
- `PromptText`: リストのデータをサーバーから非同期的に読み込むときに表示されるテキスト
- `ParentControlID`: (省略可能) 親ドロップダウン リストの現在のリストには、そのトリガー読み込み

使用するプログラミング言語によっては、問題のある web サービスの名前が変更が他のすべての属性値が同じ。 最初のドロップダウン リストの CascadingDropDown 要素を次に示します。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

2 番目の一覧については、コントロール エクステンダーを設定する必要があります、`ParentControlID`属性連絡先リスト内の要素が関連付けられている読み込みベンダー リスト トリガー内のエントリを選択します。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

実際の作業はし、次のように設定された web サービスで行います。 なお、`[ScriptService]`属性を使用すると、それ以外の場合、ASP.NET AJAX はクライアント側スクリプト コードから web メソッドにアクセスする JavaScript プロキシを作成することはできません。

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

CascadingDropDown によって呼び出される web メソッドのシグネチャは次のとおりです。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

戻り値は型の配列である必要がありますように`CascadingDropDownNameValue`Control Toolkit で定義されています。 `GetVendors()`メソッドは非常に簡単に実装できます。 コードは、AdventureWorks データベースに接続し、最初の 25 のベンダーのクエリを実行します。 最初のパラメーター、`CascadingDropDownNameValue`コンス トラクターの値を一覧のエントリ、2 つ目のキャプションには (html の value 属性&lt; `option` &gt;要素)。 次のコードに示します。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

仕入先の関連付けられている連絡先の取得 (メソッド名: `GetContactsForVendor()`) 少し複雑です。 まず、最初のドロップダウン リストで選択されている仕入先を決定する必要があります。 Control Toolkit には、そのタスク用のヘルパー メソッドが定義されています。`ParseKnownCategoryValuesString()`メソッドを返します。 を`StringDictionary`ドロップダウン リストのデータを持つ要素。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

セキュリティ上の理由から、このデータを最初に検証する必要があります。 ベンダーのエントリがある場合は (ため、 `Category` CascadingDropDown の最初の要素のプロパティに設定されて`"Vendor"`)、選択したベンダーの ID を取得することがあります。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

メソッドの残りの部分では、かなりの単純明快をしは。 ベンダーの ID は、そのベンダーのすべての関連付けられている連絡先を取得する SQL クエリのパラメーターとして使用されます。 型の配列を返します、もう一度`CascadingDropDownNameValue`します。

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

ASP.NET ページを読み込むし、後しばらくすると、ベンダーの一覧が 25 のエントリが入力されます。 1 つのエントリを選択し、データが 2 つ目のドロップダウン リストを格納する方法に注意してください。


[![最初のリストが自動的に入力します。](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

最初のリストが自動的に入力されます ([フルサイズの画像を表示する をクリックします](using-cascadingdropdown-with-a-database-vb/_static/image3.png))。


[![2 番目のリストが最初のリストの選択に応じて入力します。](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

2 番目のリストが最初のリストの選択に応じて入力 ([フルサイズの画像を表示する をクリックします](using-cascadingdropdown-with-a-database-vb/_static/image6.png))。

> [!div class="step-by-step"]
> [前へ](filling-a-list-using-cascadingdropdown-vb.md)
> [次へ](presetting-list-entries-with-cascadingdropdown-vb.md)
