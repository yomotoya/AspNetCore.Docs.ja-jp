---
title: "ASP.NET Core のタグ ヘルパー"
author: rick-anderson
description: "タグ ヘルパーと ASP.NET Core での使用方法について説明します。"
keywords: "ASP.NET Core、タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 10471210075dc8a5366b7d5170d6594c2e66ce94
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a>ASP.NET Core のタグ ヘルパーの概要 

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>タグ ヘルパーは?

タグ ヘルパーでは、サーバー側コードを作成して、Razor ファイルでの HTML 要素のレンダリングに参加を有効にします。 たとえば、組み込み`ImageTagHelper`イメージ名にバージョン番号を追加することができます。 イメージが変更されるたびに、サーバーは、クライアント (古いキャッシュされた画像) ではなく現在のイメージを取得することが保証されますので、イメージの新しい一意のバージョンを生成します。 これは、フォーム、リンク、読み込みの資産および詳細 - およびさらに高いで利用できるパブリックの GitHub リポジトリとして NuGet パッケージの作成などの一般的なタスクの多くの組み込みタグ ヘルパー。 タグ ヘルパーは、C# の場合は、作成し、要素名、属性名、または親タグに基づく HTML 要素を対象にします。 たとえば、組み込み`LabelTagHelper`HTML を対象にできます`<label>`要素ときに、`LabelTagHelper`属性が適用されます。 慣れている場合[HTML ヘルパー](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)、タグ ヘルパーは、Razor ビューでの HTML と c# の間で明示的な遷移を削減します。 多くの場合、HTML ヘルパー、別の方法を特定のタグ ヘルパーに提供するが、タグ ヘルパーの HTML ヘルパーを置き換えないし、各 HTML ヘルパーのタグ ヘルパーがないことを認識することが重要です。 [タグ ヘルパーの HTML ヘルパーと比較して](#tag-helpers-compared-to-html-helpers)詳細の相違点について説明します。

## <a name="what-tag-helpers-provide"></a>タグ ヘルパーの提供

**HTML に適した開発エクスペリエンス**大部分 Razor マークアップ タグ ヘルパーを使用して次のように標準 HTML。 HTML と CSS/JavaScript に精通フロント エンドのデザイナーは、C# の場合は、Razor 構文を習得しなくても、Razor を編集できます。

**HTML および Razor マークアップを作成するための豊富な IntelliSense 環境**これは、HTML ヘルパー、Razor ビューのマークアップのサーバー側で作成する前のアプローチをコントラストをシャープにします。 [タグ ヘルパーの HTML ヘルパーと比較して](#tag-helpers-compared-to-html-helpers)詳細の相違点について説明します。 [タグ ヘルパーの IntelliSense サポート](#intellisense-support-for-tag-helpers)IntelliSense 環境について説明します。 Razor の c# の構文の使用経験が偶数の開発者は、c# Razor マークアップを記述するよりもタグ ヘルパーを使用して生産性を向上します。

**作成する方法をより生産的かつ生成できるより堅牢で信頼性が高く、およびサーバーでのみ利用可能な情報を使用してコードを保守しやすい**たとえば、従来イメージを更新する方法の信念でしたを変更すると、イメージの名前を変更するにはイメージです。 パフォーマンス上の理由から、積極的にイメージをキャッシュする必要があり、クライアントの古いコピーを取得する危険性が画像の名前を変更すると、しない限り、します。 従来、イメージが編集された後に名前が変更されましたし、web アプリのイメージへの参照の更新が必要です。 これは、処理を要するもエラーになります (する可能性がありますの参照を誤って間違った文字列などを入力します。) が発生しやすいが非常に労力だけでなく組み込み`ImageTagHelper`できますこの処理を自動的にします。 `ImageTagHelper`バージョンに追加することができます、イメージが変更されるたびに、サーバーを自動的に生成イメージの一意の新しいバージョンようにをイメージ名番号。 クライアントを現在のイメージを取得することが保証されます。 使用してこの堅牢性と人件費の節約は本質的には無料、`ImageTagHelper`です。

組み込みタグ ヘルパーのほとんどは、既存の HTML 要素をターゲットし、要素のサーバー側の属性を指定します。 たとえば、`<input>`要素内のビューの多くで使用されて、*ビュー/アカウント*フォルダーが含まれています、`asp-for`属性は、レンダリングされる HTML に指定されたモデルのプロパティの名前を抽出します。 次の Razor マークアップ。

```html
<label asp-for="Email"></label>
```

次の HTML を生成します。

```html
<label for="Email">Email</label>
```

`asp-for`属性を使用可能で、`For`プロパティに、`LabelTagHelper`です。 参照してください[タグ ヘルパーのオーサリング](authoring.md)詳細についてはします。

## <a name="managing-tag-helper-scope"></a>タグ ヘルパーのスコープを管理します。

タグ ヘルパーのスコープがの組み合わせによって制御される`@addTagHelper`、 `@removeTagHelper`、および"!"オプトアウト文字です。

<a name=add-helper-label></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper`タグ ヘルパーを使用可能します。

という名前の新しい ASP.NET Core web アプリを作成するかどうかは*AuthoringTagHelpers* (認証なしで)、次*Views/_ViewImports.cshtml*ファイルは、プロジェクトに追加されます。

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

`@addTagHelper`ディレクティブにより、タグ ヘルパーのビューに表示します。 ここでは、ファイルの表示は*Views/_ViewImports.cshtml*、内のすべてのビュー ファイルによって継承される既定で、*ビュー*フォルダーとサブディレクトリ; タグ ヘルパーを使用できるようにします。 上記のコードでは、ワイルドカードの構文を使用して ("\*") ことを指定する、指定したアセンブリ内のすべてのタグ ヘルパー (*Microsoft.AspNetCore.Mvc.TagHelpers*) できるように、すべてのファイルの表示になります、*ビュー*ディレクトリまたはサブディレクトリ。 最初のパラメーターの後に`@addTagHelper`を読み込むタグ ヘルパーを指定します (を使用して"\*"すべてのタグ ヘルパーの)、し"Microsoft.AspNetCore.Mvc.TagHelpers"の 2 番目のパラメーターは、タグ ヘルパーを含むアセンブリを指定します。 *Microsoft.AspNetCore.Mvc.TagHelpers*組み込みの ASP.NET Core タグ ヘルパーのアセンブリです。

このプロジェクトでタグ ヘルパーのすべてを公開するために (という名前のアセンブリを作成する*AuthoringTagHelpers*)、以下を使用するとします。

[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

プロジェクトが含まれている場合、`EmailTagHelper`既定の名前空間 (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)、タグ ヘルパーの完全修飾名 (FQN) を指定することができます。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

タグ ヘルパーに追加する、FQN を使用するビューを最初に追加する、FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)、およびアセンブリ名では、(*AuthoringTagHelpers*)。 ほとんどの開発者が使用を好む、"\*"ワイルドカードの構文。 ワイルドカードの構文では、ワイルドカード文字を挿入することができます"\*"、FQN のサフィックスとして。 表示されます、次のディレクティブのいずれかなど、 `EmailTagHelper`:

```csharp
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

前述のように、追加、`@addTagHelper`ディレクティブを*Views/_ViewImports.cshtml*ファイル タグ ヘルパーに使用可能ですべてのファイルの表示、*ビュー*ディレクトリとサブディレクトリです。 使用することができます、`@addTagHelper`ディレクティブでこれらのビューにタグ ヘルパーの公開にオプトインする場合に特定のビューはファイルです。

<a name=remove-razor-directives-label></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper`タグ ヘルパーを削除します。

`@removeTagHelper` 、同じ 2 つのパラメーターとして`@addTagHelper`、既に追加されているタグ ヘルパーを削除します。 たとえば、`@removeTagHelper`ビューから特定のビューを削除指定されたタグ ヘルパーに適用します。 使用して`@removeTagHelper`で、 *Views/Folder/_ViewImports.cshtml*ファイルからすべてのビューの指定されたタグ ヘルパーの削除*フォルダー*です。

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>タグ ヘルパーのスコープを制御する、 *_ViewImports.cshtml*ファイル

追加することができます、 *_ViewImports.cshtml*エンジンがその両方のファイルのディレクティブを適用するビューの任意のフォルダーおよびビューと*Views/_ViewImports.cshtml*ファイル。 場合は、空の追加*Views/Home/_ViewImports.cshtml*ファイルで、*ホーム*ビュー、あるは変更されないため、 *_ViewImports.cshtml*ファイルが加法です。 任意`@addTagHelper`ディレクティブを追加する、 *Views/Home/_ViewImports.cshtml*ファイル (既定値に含まれていない*Views/_ViewImports.cshtml*ファイル) が公開されますをビューには、そのタグ ヘルパーでのみ、*ホーム*フォルダーです。

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>個々 の要素を除外

タグ ヘルパーのオプトアウト文字で、要素レベルでタグ ヘルパーを無効にすることができます ("!") です。 たとえば、`Email`で検証が無効になっている、`<span>`タグ ヘルパーのオプトアウト文字。

```csharp
<!span asp-validation-for="Email" class="text-danger"></!span>
```

開始タグと終了タグにタグ ヘルパーのオプトアウト文字を適用する必要があります。 (Visual Studio エディターに自動的にオプトアウトに文字を追加、終了タグ開始タグに追加する場合)。 オプトアウト文字を追加した後、要素とタグ ヘルパー属性は印象的なフォントで表示されません。

<a name=prefix-razor-directives-label></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>使用して`@tagHelperPrefix`タグ ヘルパーの使用状況を明示するには

`@tagHelperPrefix`ディレクティブでは、タグ ヘルパーのサポートを有効にして、タグ ヘルパーの使用法を明確にするタグ プレフィックス文字列を指定することができます。 たとえば、次に示すマークアップを追加、 *Views/_ViewImports.cshtml*ファイル。

```html
@tagHelperPrefix th:
```
設定されているタグ ヘルパーのプレフィックス コード次の図、 `th:`、プレフィックスを使用して要素のみ`th:`タグ ヘルパー (タグ ヘルパーが有効な要素がある印象的なフォント) をサポートします。 `<label>`と`<input>`要素タグ ヘルパーのプレフィックス、タグ ヘルパーに対応している、ときに、`<span>`要素はありません。

![イメージ](intro/_static/thp.png)

適用される、同じ規則が階層`@addTagHelper`にも適用`@tagHelperPrefix`です。

## <a name="intellisense-support-for-tag-helpers"></a>タグ ヘルパーの IntelliSense のサポート

Visual Studio で新しい ASP.NET web アプリを作成するときに、NuGet パッケージ"Microsoft.AspNetCore.Razor.Tools"を追加します。 これは、タグ ヘルパー ツールを追加するパッケージです。

HTML の作成を検討`<label>`要素。 入力するとすぐに`<l`Visual Studio エディターで IntelliSense が一致する要素を表示します。

![イメージ](intro/_static/label.png)

アイコンは、HTML ヘルプを表示するだけでなく (、"@"のシンボルがその下にある"<>") です。

![イメージ](intro/_static/tagSym.png)

タグ ヘルパーの対象となるように、要素を識別します。 純粋な HTML 要素 (など、 `fieldset`)"<>"アイコンを表示します。

純粋な HTML`<label>`タグが、茶色のフォントが赤で属性に (既定の Visual Studio の色のテーマ) の HTML タグを表示し、属性は青色で示される値します。

![イメージ](intro/_static/LableHtmlTag.png)

入力した後`<label`IntelliSense には、使用可能な HTML と CSS とタグ ヘルパーの対象となる属性が一覧表示されます。

![イメージ](intro/_static/labelattr.png)

IntelliSense 入力候補では、キーを入力します タブを選択した値を指定してステートメントを完了することができます。

![イメージ](intro/_static/stmtcomplete.png)

タグ ヘルパー属性を入力するとすぐ、タグおよび属性のフォントを変更します。 既定の Visual Studio"Blue"または"Light"色のテーマを使用して、フォントは、太字紫です。 [ダーク] テーマを使用している場合、フォントは太字青緑がします。 このドキュメント内のイメージは、既定のテーマを使用して取得されました。

![イメージ](intro/_static/labelaspfor2.png)

Visual Studio を入力することができます*CompleteWord*ショートカット (ctrl キーを押しながら space キーは、[既定](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)二重引用符の内側 ("")、実行され、今すぐ、C# の場合は、c# のクラスにする場合と同じようにします。 IntelliSense は、ページのモデルのすべてのメソッドとプロパティを表示します。 メソッドとプロパティが使用可能なプロパティの型があるため`ModelExpression`です。 次の図では編集して、`Register`ビュー、ため、`RegisterViewModel`は使用できます。

![イメージ](intro/_static/intellemail.png)

IntelliSense は、プロパティと、ページ上のモデルが使用できる方法を示します。 豊富な IntelliSense 環境では、CSS クラスを選択できます。

![イメージ](intro/_static/iclass.png)

![イメージ](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>HTML ヘルパーと比較してタグ ヘルパー

Razor ビューでの HTML 要素にタグ ヘルパーのアタッチ中に[HTML ヘルパー](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) Razor ビューの HTML が混在してメソッドが呼び出されます。 次の Razor のマークアップは、HTML ラベルを「キャプション」の CSS クラスの作成を検討してください。

```html
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

で (`@`) 記号は、これは、コードの先頭を Razor に指示します。 次の 2 つのパラメーター ("FirstName"と"名:")、文字列のためは[IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense)ヘルプことはできません。 最後の引数。

```html
new {@class="caption"}
```

属性を表すために使用する匿名オブジェクト。 **クラス**を予約済みキーワードを使用する (C#)、 `@` c# の解釈を強制する記号"@class="(プロパティ名) のシンボルとして。 フロント エンドのデザイナーを (他のユーザー/Html/javascript CSS およびその他のクライアント テクノロジに慣れているが、c# と Razor に精通していない)、行のほとんどが外部です。 ヘルプは、intellisense では、行全体を作成する必要があります。

使用して、 `LabelTagHelper`、として同じマークアップを記述できます。

![イメージ](intro/_static/label2.png)

入力するとすぐには、タグ ヘルパーのバージョンで`<l`Visual Studio エディターで IntelliSense が一致する要素を表示します。

![イメージ](intro/_static/label.png)

IntelliSense では、行全体を記述するのに役立ちます。 `LabelTagHelper`ものコンテンツの設定に既定値は、 `asp-for` 「姓」を属性値 ("FirstName")Camel 形式のプロパティを各新しい大文字が発生する領域を持つプロパティ名から成る文に変換します。 次のマークアップ。

![イメージ](intro/_static/label2.png)

生成されます。

```html
<label class="caption" for="FirstName">First Name</label>
```

Camel 形式の大文字小文字の文のコンテンツには使用されませんにコンテンツを追加する場合、`<label>`です。 例:

![イメージ](intro/_static/1stName.png)

生成されます。

```html
<label class="caption" for="FirstName">Name First</label>
```

次のコードの画像、フォームの部分を示します、 *Views/Account/Register.cshtml* Visual Studio 2015 に含まれているレガシ ASP.NET 4.5.x MVC テンプレートから生成された Razor ビュー。

![イメージ](intro/_static/regCS.png)

Visual Studio エディターでは、背景が灰色の c# コードを表示します。 たとえば、 `AntiForgeryToken` HTML ヘルパー。

```html
@Html.AntiForgeryToken()
```

グレーの背景が表示されます。 レジスタ ビュー内のマークアップのほとんどは c# です。 タグ ヘルパーの使用と同じアプローチを比較します。

![イメージ](intro/_static/regTH.png)

マークアップ方がクリーナー簡単に読み取り、編集、および HTML ヘルパー アプローチよりも管理します。 C# コードは、について知っておく必要があるサーバーに最低限に短縮されます。 Visual Studio エディターでは、印象的なフォントでタグ ヘルパーの対象とするマークアップを表示します。

検討してください、*電子メール*グループ。

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

各「asp-」属性には"Email"の値が"Email"は文字列ではありません。 ここでは、"Email"は、c# 式のモデル プロパティ、`RegisterViewModel`です。

Visual Studio エディターでは、作成できます。**すべて**の Visual Studio で HTML ヘルパーの方法でコードのほとんどのヘルプはありません、登録フォームのタグ ヘルパーの方法でマークアップ。 [タグ ヘルパーの IntelliSense サポート](#intellisense-support-for-tag-helpers)Visual Studio エディターでタグ ヘルパーの使用方法の詳細を見ていきます。

## <a name="tag-helpers-compared-to-web-server-controls"></a>Web サーバー コントロールと比較してタグ ヘルパー

* 関連付けられている要素を所有していないタグ ヘルパーそれらの要素およびコンテンツのレンダリングに単に参加します。 ASP.NET [Web サーバー コントロール](https://msdn.microsoft.com/library/7698y1f0.aspx)の宣言し、ページに呼び出されます。

* [Web サーバー コントロール](https://msdn.microsoft.com/library/zsyt68f1.aspx)が困難に開発およびデバッグする重要なライフ サイクルがあります。

* Web サーバー コントロールを使用すると、クライアントのコントロールを使用して、クライアントのドキュメント オブジェクト モデル (DOM) 要素に機能を追加できます。 タグ ヘルパーが DOM. をあるなし

* Web サーバー コントロールには、ブラウザーの自動検出が含まれます。 タグ ヘルパーは、ブラウザーの知識を持っていません。

* 複数のタグ ヘルパーが同じ要素を処理できる (を参照してください[タグ ヘルパーを回避する競合](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts)) 中に、通常 Web サーバー コントロールを構成できません。

* タグ ヘルパーは、タグおよびこれらを対象としている HTML 要素の内容を変更できますが、ページ上の他のものを直接変更しないで。 Web サーバー コントロールより限定スコープが存在し、ページの他の部分に影響を与えるアクションを実行できます。意図しない副作用を有効にします。

* Web サーバー コントロールでは、型コンバーターを使用して、文字列をオブジェクトに変換します。 タグ ヘルパーで作業するネイティブ C# の場合、入力は、変換する必要はありません。

* Web サーバー コントロールを使用して[System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel)コンポーネントやコントロールの実行時およびデザイン時の動作を実装します。 `System.ComponentModel`基本クラスとインターフェイス、属性と型コンバーターの実装、ソース、データへのバインド、およびコンポーネントのライセンスが含まれます。 タグ ヘルパーは、通常から派生するコントラストを`TagHelper`、および`TagHelper`基底クラスのみに 2 つのメソッドを公開する`Process`と`ProcessAsync`です。

## <a name="customizing-the-tag-helper-element-font"></a>タグ ヘルパー要素のフォントのカスタマイズ

色付けとフォントをカスタマイズすることができます**ツール** > **オプション** > **環境** > **フォントおよび色**:

![イメージ](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>その他のリソース

* [タグ ヘルパーの作成](authoring.md)
* [フォームの使用](../working-with-forms.md)
* [GitHub の TagHelperSamples](https://github.com/dpaquette/TagHelperSamples)を操作するためのタグ ヘルパーのサンプルが含まれます[ブートス トラップ](http://getbootstrap.com/)です。

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
