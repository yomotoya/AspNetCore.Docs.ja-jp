---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: "部分ページ更新 ASP.NET AJAX を理解する |Microsoft ドキュメント"
author: scottcate
description: "おそらく、最も可視機能、ASP.NET AJAX Extensions の機能を t に完全なポストバックを実行せず、または部分的な差分のページ更新を行うには."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 1d8d3009df0a264e466d3f7decfb65978d8ae7a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Understanding 部分ページは、ASP.NET AJAX と共に更新されます。
====================
によって[Scott カテゴリ](https://github.com/scottcate)

[PDF をダウンロードします。](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> おそらく、ASP.NET AJAX Extensions の最も可視機能は、最小限のマークアップの変更とし、コードの変更を持たない、サーバーへの完全なポストバックを実行せず、または部分的な差分のページ更新プログラムを実行する機能。 利点は、広範な – (Adobe Flash や Windows Media) など、マルチ メディアの状態は変更されません、帯域幅のコストが少なくなり、クライアントでは、通常、ポストバックに関連付けられているちらつきは発生しません。


## <a name="introduction"></a>はじめに

ASP.NET の Microsoft のテクノロジは、オブジェクト指向し、イベント ドリブン プログラミング モデルによりに導入され、コンパイルされたコードのメリットと組み合わせたです。 ただし、そのサーバー側の処理モデルでは、テクノロジに固有のいくつかの欠点があります。

- ページの更新プログラムでは、ページの更新を必要とすると、サーバーへのラウンド トリップが必要です。
- ラウンドト リップが Javascript またはその他のクライアント側などのテクノロジ (Adobe Flash) によって生成される任意の効果を保持されません。
- ポストバック中に Microsoft Internet Explorer 以外のブラウザーはサポートされませんスクロール位置を自動的に復元します。 Internet Explorer であってもまだ、ちらつきを調整するページが更新されて。
- ポストバックとして帯域幅の量が多い場合があります、 \_ \_VIEWSTATE フォーム フィールドのサイズが増加、GridView コントロールまたはリピータなどのコントロールを扱う場合に特にです。
- JavaScript またはその他のクライアント側のテクノロジから Web サービスにアクセスするための統一されたモデルはありません。

Microsoft の ASP.NET AJAX 拡張機能を入力します。 AJAX **A**同期**J** avaScript **A** nd **X** ML、増分のページを提供するための統合のフレームワークではクロス プラットフォームの JavaScript を使用して更新プログラムは、Microsoft AJAX Framework および Microsoft AJAX スクリプト ライブラリと呼ばれるスクリプト コンポーネントで構成されるサーバー側コードで構成されます。 ASP.NET AJAX 拡張機能では、JavaScript を使用して ASP.NET Web サービスにアクセスするクロスプラット フォーム サポートも提供します。

このホワイト ペーパーでは、ScriptManager コンポーネント、UpdatePanel コントロールおよび UpdateProgress コントロールが含まれ、シナリオをする必要がありますまたはすることはできませんと見なします ASP.NET AJAX Extensions のページの部分的な更新機能を調べます使用率が低い。

このホワイト ペーパーでは、Visual Studio 2008 の Beta 2 リリースされ、基底クラス ライブラリ (が以前 ASP.NET 2.0 の使用可能なアドオン コンポーネント) に ASP.NET AJAX 拡張機能を統合する .NET Framework 3.5 に基づいています。 このホワイト ペーパーが Visual Studio 2008 とない Visual Web Developer Express Edition; を使用していることを想定しても参照されている一部のプロジェクト テンプレートは、Visual Web Developer Express のユーザーにできない場合があります。

## <a name="partial-page-updates"></a>部分ページ更新

おそらく、ASP.NET AJAX Extensions の最も可視機能は、最小限のマークアップの変更とし、コードの変更を持たない、サーバーへの完全なポストバックを実行せず、または部分的な差分のページ更新プログラムを実行する機能。 利点は、広範な - (Adobe Flash や Windows Media) など、マルチ メディアの状態は変更されません、帯域幅のコストが少なくなり、クライアントでは、通常、ポストバックに関連付けられているちらつきは発生しません。

部分ページ レンダリングを統合する機能は、プロジェクトに最小限の変更で ASP.NET に統合されます。

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>チュートリアル: 既存のプロジェクトへの部分的なレンダリングの統合


1. Microsoft Visual Studio 2008 でに移動して、新しい ASP.NET Web Site プロジェクトを作成*ファイル* *- &gt;新規* *- &gt; のWebサイト*ダイアログから ASP.NET Web サイトを選択します。 名前を付けることお好きなようとをファイル システムまたはインターネット インフォメーション サービス (IIS) にインストールすることがあります。
2. 基本的な ASP.NET マークアップに空白の既定のページが表示されます (サーバー側フォームと`@Page`ディレクティブ)。 という名前のラベルにドロップ`Label1`ボタンと呼ばれると`Button1`form 要素内でページ上にします。 お好きなように、テキストのプロパティを設定することがあります。
3. デザイン ビューでダブルクリック`Button1`分離コードのイベント ハンドラーを生成します。 このイベント ハンドラー内で次のように設定します`Label1.Text`にボタンをクリックしました!。 。

**部分的なレンダリングを有効にする前に、default.aspx のマークアップを 1 の一覧を表示するには:**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**2 を一覧表示します。 分離コードが default.aspx.cs に (切り捨て)**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. F5 キーを押して、web サイトを起動します。 Visual Studio には、デバッグを有効にする web.config ファイルを追加するように求められますそのための操作を行います。 ボタンをクリックすると、ラベルのテキストを変更するページが更新され、ページが再描画されると、簡単なちらつきがあることに注意してください。
2. ブラウザー ウィンドウを閉じると、Visual studio、およびマークアップのページを返します。 Visual Studio ツールボックスにスクロールし、[AJAX Extensions] タブを検索します。 ない場合はこのタブの AJAX 機能または地図の拡張機能の古いバージョンを使用しているため、このホワイト ペーパーの後半で、AJAX Extensions ツールボックス項目を登録するためのチュートリアルを参照してください (ダウンロード可能な Windows インストーラーで、現在のバージョンをインストールweb サイトから)。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([フルサイズのイメージを表示するをクリックして](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. *既知の問題点:*Visual Studio 2008 が AJAX Extensions ツールボックス アイテムをインポート、ASP.NET 2.0 AJAX Extensions にインストールされている Visual Studio 2005 には既にコンピューターに Visual Studio 2008 をインストールする場合。 コンポーネントにツールヒントを確認するにはそうではあるかどうかを判断できます。バージョン 3.5.0.0 と記述する必要があります。 バージョン 2.0.0.0 と主張する場合、古いツールボックス アイテムをインポートし、それらを Visual Studio のツールボックス アイテムの選択 ダイアログを使用して手動でインポートする必要があります。 デザイナーを使用してバージョン 2 のコントロールを追加することはできません。

1. 前に、`<asp:Label>`タグの開始、空白の行を作成し、ツールボックスで UpdatePanel コントロールをダブルクリックします。 なお、新しい`@Register`ディレクティブを使用して派生内のコントロールをインポートすることを示す、ページの上部に含まれる、`asp:`プレフィックス。
2. 終了をドラッグして`</asp:UpdatePanel>`ボタン要素の末尾を越えたタグ付けしてことでラップされたコントロールのラベルおよびボタンのコントロール要素が適切な形式です。
3. オープン後`<asp:UpdatePanel>`タグ、開始新しいタグを開始します。 IntelliSense 確認が求めるされる 2 つのオプションに注意してください。 この場合、作成、`<ContentTemplate>`タグ。 マークアップが整形式ようにラベルとボタンの周囲には、このタグをラップすることを確認します。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([フルサイズのイメージを表示するをクリックして](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. 任意の場所に、`<form>`要素をダブルクリックして ScriptManager コントロールを含める、`ScriptManager`ツールボックス内の項目。
2. 編集、`<asp:ScriptManager>`属性が含まれるようにタグ付け`EnablePartialRendering= true`です。

**部分的なレンダリングを有効になっている default.aspx のマークアップを 3 の一覧を表示するには:**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Web.config ファイルを開きます。 Visual Studio が System.Web.Extensions.dll への参照をコンパイルを自動的に追加することを確認します。

1. Visual Studio 2008 の新: 自動的に ASP.NET Web サイト プロジェクト テンプレートに付属している web.config ファイルは、ASP.NET AJAX Extensions に必要なすべての参照が含まれています、使用可能な構成情報のセクションでにはコメントが含まれていますその他の機能を有効にされていないコメント。 Visual Studio 2005 では、ASP.NET 2.0 AJAX Extensions がインストールされたときのようなテンプレートがありました。 ただし、Visual Studio 2008 で AJAX 拡張機能は、オプトアウト既定で (つまりが既定では、参照されるが、参照として削除されることができます)。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([フルサイズのイメージを表示するをクリックして](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. F5 キーを押して、web サイトを起動します。 ソース コードの変更の部分的なレンダリングをサポートする必要はありません - マークアップのみが変更された方法に注意してください。

Web サイトを起動するときに、部分的なレンダリングは をクリックする場合、ボタンがあります。 ちらつきはされませんまた (この例では示しませんを)、ページのスクロール位置を変更してもありますので、これで、有効になっていることが表示されます。 ボタンをクリックした後、ページのレンダリングのソースに検索する場合は、これはことを確認実際には、ポストバックが発生していない - 元のラベルのテキストは、まだソース マークアップの javascript ラベルが変更されました。

Visual Studio 2008 は、ASP.NET AJAX 対応の web サイトの定義済みテンプレートに付属するは表示されません。 ただし、このようなテンプレートが Visual Studio 2005 内で使用できる、Visual Studio 2005 と ASP.NET 2.0 AJAX Extensions がインストールされた場合。 その結果、AJAX-Enabled Web サイト テンプレートで、web サイトを構成し、開始が場合があります。、さらに簡単にテンプレートをサポートしているすべての Web サービスのアクセスなど、ASP.NET AJAX Extensions 完全に構成された web.config ファイルを含める必要があります。および JSON のシリアル化の JavaScript Object Notation) と既定では、UpdatePanel と ContentTemplate をメインの Web フォーム ページ内に含まれています。 この既定のページで部分のレンダリングを有効にするはこのチュートリアルの手順 10 を見直す必要し、ページ上にコントロールを削除するように単純です。

## <a name="the-scriptmanager-control"></a>ScriptManager コントロール

## <a name="scriptmanager-control-reference"></a>ScriptManager コントロールのリファレンス

マークアップが有効なプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| AllowCustomErrors リダイレクト | Bool | エラーを処理する web.config ファイルのカスタム エラー セクションを使用するかどうかを指定します。 |
| AsyncPostBackError メッセージ | 文字列型 | 取得またはエラーが発生した場合、クライアントに送信するエラー メッセージを設定します。 |
| AsyncPostBack タイムアウト | Int32 | 取得またはクライアントが完了する非同期要求を待機する時間の既定値を設定します。 |
| EnableScript グローバリゼーション | Bool | 取得またはスクリプトのグローバリゼーションを有効にするかどうかを設定します。 |
| EnableScript ローカリゼーション | Bool | 取得またはスクリプトのローカライズが有効になっているかどうかを設定します。 |
| ScriptLoadTimeout | Int32 | クライアントにスクリプトを読み込むためのできる秒数を決定します。 |
| ScriptMode | 列挙型 (自動、デバッグ、リリース、継承) | 取得またはリリース バージョンのスクリプトをレンダリングするかどうかを設定 |
| ScriptPath | 文字列型 | 取得またはクライアントに送信するスクリプト ファイルの場所にルート パスを設定します。 |

コード専用のプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| 認証サービス | 認証サービス マネージャー | 詳細については、クライアントに送信される ASP.NET 認証サービス プロキシを取得します。 |
| IsDebuggingEnabled | Bool | 取得するかどうかスクリプトを作成し、コードのデバッグを有効にします。 |
| 含ま | Bool | ページが現在非同期ポストバック要求であるかどうかを取得します。 |
| ProfileService | ProfileService マネージャー | 詳細については、クライアントに送信される ASP.NET プロファイリング サービス プロキシを取得します。 |
| スクリプト | コレクション&lt;スクリプト リファレンス&gt; | クライアントに送信されるスクリプト参照のコレクションを取得します。 |
| サービス | コレクション&lt;サービス参照&gt; | クライアントに送信される Web サービス プロキシの参照のコレクションを取得します。 |
| SupportsPartialRendering | Bool | 現在のクライアントは、部分的なレンダリングをサポートしているかどうかを取得します。 このプロパティを返す場合**false**、すべてのページ要求は標準的なポストバックになります。 |

コードのパブリック メソッド:

| **メソッド名** | **Type** | **説明** |
| --- | --- | --- |
| SetFocus(string) | Void | 要求が完了したときに、特定のコントロールに、クライアントのフォーカスを設定します。 |

マークアップ子孫:

| **タグ** | **説明** |
| --- | --- |
| &lt;認証サービス&gt; | ASP.NET 認証サービスへのプロキシの詳細を提供します。 |
| &lt;ProfileService&gt; | ASP.NET のサービスをプロファイリングするプロキシの詳細を提供します。 |
| &lt;スクリプト&gt; | その他のスクリプト参照を提供します。 |
| &lt;うに&gt; | 特定のスクリプト参照を表します。 |
| &lt;サービス&gt; | 生成されたプロキシ クラスを持つその他の Web サービス参照を提供します。 |
| &lt;asp: ServiceReference&gt; | 特定の Web サービス参照を表します。 |

ScriptManager コントロールは、ASP.NET AJAX 拡張機能の必須コアです。 (広範なクライアント側-スクリプトの型システムを含む)、スクリプト ライブラリへのアクセスを提供、部分のレンダリングをサポートおよびその他の ASP.NET サービス (認証と、プロファイリングも他の Web サービス) などの広範なサポートを提供します。 ScriptManager コントロールは、グローバリゼーションとローカリゼーションは、クライアント スクリプトのサポートも提供します。

## <a name="providing-alterative-and-supplemental-scripts"></a>こうしたおよび補足的なスクリプトを提供します。

Microsoft ASP.NET 2.0 AJAX Extensions では、両方のデバッグに全体のスクリプト コードを追加、参照されるアセンブリに埋め込まれたリソースとしてエディションを離すと、開発者は、ScriptManager をカスタマイズしたスクリプト ファイルにリダイレクトだけでなく登録するために解放必要なスクリプトを追加します。

登録できます (Sys.WebForms 名前空間とカスタム入力システムをサポートするもの) などの通常含まれるスクリプトの既定のバインディングをオーバーライドする、 `ResolveScriptReference` ScriptManager クラスのイベントです。 イベント ハンドラーの問題; のスクリプト ファイルへのパスを変更することがこのメソッドが呼び出されると、スクリプト マネージャーは、異なるまたはカスタマイズされたコピーを送信するスクリプトのクライアントです。

さらに、スクリプト参照 (によって表される、`ScriptReference`クラス) プログラムから、またはマークアップを使用して指定できます。 これを行うには、プログラムを変更するか、`ScriptManager.Scripts`コレクション、または含める`<asp:ScriptReference>`タグの下、 `<Scripts>` ScriptManager コントロールの最初のレベルの子であるタグ。

## <a name="custom-error-handling-for-updatepanels"></a>カスタム エラー処理を抑えられる

UpdatePanel コントロールで指定されたトリガーにより、更新プログラムの処理が、エラー処理とカスタム エラー メッセージのサポートは、ページの ScriptManager コントロールのインスタンスによって処理されます。 これには、イベントの公開を`AsyncPostBackError`がページにし、カスタム例外処理のロジックを提供します。

指定することによって、AsyncPostBackError イベントを使用するには、`AsyncPostBackErrorMessage`プロパティで、コールバックの完了時に発生するメッセージ ボックスを引き起こします。

クライアント側のカスタマイズも可能です。 既定のアラートのボックスを使用する代わりにです。インスタンスが表示する、カスタマイズされた`<div>`既定のブラウザーのモーダル ダイアログ ボックスではなく要素。 この場合、クライアント スクリプトでエラーを処理できます。

**カスタム エラーを表示するクライアント側スクリプトの 5 の一覧を表示するには:**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

簡単に言えば、前述のスクリプトは、非同期要求が完了したときとのクライアント側の AJAX ランタイムと、コールバックを登録します。 これは、後、エラーが報告されたかどうかを確認し、場合は、カスタム スクリプトでエラーが処理されたことをランタイムに最後に示すの詳細を処理します。

## <a name="globalization-and-localization-support"></a>グローバリゼーションとローカリゼーションのサポート

ScriptManager コントロールのスクリプトの文字列とユーザー インターフェイス コンポーネント; ローカライズ広範なサポートを提供します。ただし、このトピックでは、このホワイト ペーパーの範囲外です。 詳細については、ASP.NET AJAX Extensions にグローバル化のサポートに関するホワイト ペーパーを参照してください。

## <a name="the-updatepanel-control"></a>UpdatePanel コントロール

## <a name="updatepanel-control-reference"></a>UpdatePanel コントロールの参照

マークアップが有効なプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| 場合、ChildrenAsTriggers | bool | 子コントロールがポストバック時に更新を自動的に起動するかどうかを指定します。 |
| RenderMode | (ブロック、インライン) 列挙型 | コンテンツの方法は視覚的に表示されるかを指定します。 |
| UpdateMode | enum (常に、条件付き) | UpdatePanel が部分的なレンダリング中に常に更新されるかどうか、またはトリガーにヒットしたときのみ更新されるかどうかを指定します。 |

コード専用のプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| IsInPartialRendering | bool | 現在の要求の UpdatePanel が部分的なレンダリングをサポートするかどうかを取得します。 |
| ContentTemplate | ITemplate | 更新の要求のマークアップのテンプレートを取得します。 |
| ContentTemplateContainer | コントロール | 更新の要求のプログラムでテンプレートを取得します。 |
| トリガー | UpdatePanel の TriggerCollection | 現在の UpdatePanel に関連付けられているトリガーの一覧を取得します。 |

コードのパブリック メソッド:

| **メソッド名** | **Type** | **説明** |
| --- | --- | --- |
| Update() | Void | プログラムで指定された UpdatePanel を更新します。 それ以外の場合トリガーの UpdatePanel の部分的なレンダリングをトリガーするサーバーの要求を許可します。 |

マークアップ子孫:

| **タグ** | **説明** |
| --- | --- |
| &lt;ContentTemplate&gt; | 部分的なレンダリングの結果を表示するために使用するマークアップを指定します。 子&lt;asp: UpdatePanel&gt;です。 |
| &lt;トリガー&gt; | コレクションを指定 *n* この UpdatePanel の更新に関連するコントロール。 子&lt;asp: UpdatePanel&gt;です。 |
| &lt;asp: AsyncPostBackTrigger&gt; | 特定の UpdatePanel の部分ページ レンダリングを起動するトリガーを指定します。 これは、問題の UpdatePanel の子として、コントロールができない可能性がありますもかまいません。 イベント名に細分化されました。子&lt;トリガー&gt;です。 |
| &lt;asp: PostBackTrigger&gt; | 全体のページが更新を原因となるコントロールを指定します。 これは、問題の UpdatePanel の子として、コントロールができない可能性がありますもかまいません。 オブジェクトに細分化されました。 子&lt;トリガー&gt;です。 |

`UpdatePanel`コントロールはコントロールの AJAX 拡張機能の部分的なレンダリング機能に参加するサーバー側のコンテンツを取り出すためです。 ページでは、使用可能な UpdatePanel コントロールの数に制限はありませんし、入れ子にできます。 各 UpdatePanel は、それぞれは独立して動作できるように、分離性、(ページのさまざまな部分をページのポストバックに依存しないレンダリング、同時に実行されている 2 つ Updatepanel を持つことができます)。

UpdatePanel 中心的なトリガーがコントロール - 既定では、UpdatePanel の内に含まれる任意のコントロールを制御する`ContentTemplate`ポストバックを作成する、UpdatePanel のトリガーとして登録されます。 つまり、UpdatePanel は、ユーザー コントロール (、GridView など)、既定のデータ バインド コントロールを使用できるようにスクリプトでプログラミングできます。

既定では、ページ上のすべての UpdatePanel コントロールが更新されます部分ページ レンダリングがトリガーされたときに、このようなアクションのトリガーが定義されている UpdatePanel を制御するかどうか。 たとえば、1 つの UpdatePanel ボタン コントロールの定義、そのボタン コントロールがクリックされた場合は、そのページ上のすべての UpdatePanel コントロールが既定では更新されます。 これは、理由は、既定では、 `UpdateMode` UpdatePanel のプロパティに設定されて`Always`です。 また UpdateMode プロパティを設定することがあります`Conditional`場合は、特定のトリガーがヒットは、UpdatePanel の更新されることのみを意味します。

## <a name="custom-control-notes"></a>カスタム コントロールの注意事項

UpdatePanel を任意のユーザー コントロールまたはカスタム コントロールを追加できます。ただし、これらのコントロールが含まれているページもあります EnablePartialRendering に設定されたプロパティを持つ ScriptManager コントロール**true**です。

1 つの方法がありますを考慮するこの Web カスタム コントロールを使用して、保護されたをオーバーライドするとき`CreateChildControls()`のメソッド、`CompositeControl`クラスです。 これにより、ページは、部分的なレンダリング; をサポートするいると判断した場合、コントロールの子と外部の間で UpdatePanel を挿入できます。コンテナー内に子コントロールを単にレイヤーをそれ以外の場合、`Control`インスタンス。

## <a name="updatepanel-considerations"></a>UpdatePanel の考慮事項

UpdatePanel は ASP.NET ポストバック JavaScript XMLHttpRequest のコンテキスト内での折り返しの黒いボックス、ものとして動作します。 ただし、大幅なパフォーマンスの考慮事項に注意して、両方の面での動作と速度があります。 最適なときに使用することは適切なこともできます、UpdatePanel のしくみを理解するには、AJAX exchange を調べる必要があります。 次の例では、(Firebug は XMLHttpRequest データがキャプチャ) Firebug 拡張子を持つ既存のサイトと、Mozilla Firefox を使用します。

ある、特に、郵便番号コード のテキスト ボックスは、フォームまたはコントロールの市区町村と都道府県のフィールドを設定する必要がありますフォームを検討してください。 このフォームは、最終的には、ユーザーの名前、アドレス、および連絡先情報を含む、メンバーシップ情報を収集します。 考慮するために、特定のプロジェクトの要件に基づく多くの設計の考慮事項があります。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([フルサイズのイメージを表示するをクリックして](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([フルサイズのイメージを表示するをクリックして](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


このアプリケーションの元のイテレーションでは、コントロールは、郵便番号、市区町村、および状態を含む、ユーザー登録データの全体を組み込むことでビルドされました。 コントロール全体が UpdatePanel 内でラップし、Web フォームにドロップします。 ユーザーが郵便番号を入力すると、UpdatePanel イベント (バックエンド、トリガーを指定することによって、または true の場合、ChildrenAsTriggers プロパティ セットを使用してに対応する TextChanged イベント) が検出されます。 AJAX では、UpdatePanel 内のフィールドのすべてがポストされる firebug を使用してキャプチャ (右側の図を参照してください)。

画面のキャプチャが示されているように、UpdatePanel 内のすべてのコントロールからの値が配信されます (この場合はすべて空)、ViewState フィールドとします。 総じて、以上 9 kb のデータが送信される、実際には 5 バイトのデータのみがこの特定の要求を行うために必要なときにします。 応答はさらに高い肥大化: 合計 57 kb がクライアントに送信、ドロップダウン フィールドとテキスト フィールドを更新するためだけにします。

ASP.NET AJAX でプレゼンテーションを更新する方法を表示する目的の場合もあります。 UpdatePanel の更新要求の応答部分が左側に、Firebug コンソール画面に表示されます。これは、特別に策定パイプで区切られた文字列クライアント スクリプトによって分割され、ページで、再構築されます。 具体的には、ASP.NET AJAX の設定、 *innerHTML* UpdatePanel を表すクライアント上の HTML 要素のプロパティです。 ブラウザーは、DOM を再生成を処理する必要がある情報の量によって、多少の時間があります。

DOM の再生成には、その他の問題の数がトリガーされます。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([フルサイズのイメージを表示するをクリックして](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- フォーカスのある HTML 要素が、UpdatePanel 内にある場合は、フォーカスが失われます。 そのため、ユーザーの郵便番号コードのテキスト ボックスを終了するタブ キーを押した場合が次の宛先が、City テキスト ボックス。 ただし、UpdatePanel を更新されると、表示、フォーム不要になったがあります、フォーカスし、Tab キーを押してが開始した (リンク) などのフォーカス要素の強調表示します。
- カスタムのクライアント側スクリプトの任意の型が使用される関数で DOM 要素をアクセス、参照が保持されている場合は機能しなくなります部分的なポストバック後に。

Updatepanel は、キャッチ オール ソリューションではありません。 代わりに、特定の状況は、プロトタイプを作成、小さなコントロールの更新プログラムを含むの簡単な解決策を提供およびする可能性がありますが DOM では低いのため、.NET オブジェクト モデルを使い慣れた ASP.NET 開発者にとって使い慣れたインターフェイスを提供 シナリオによっては、アプリケーションのパフォーマンスが向上につながる可能性のある選択肢の数があります。

- 内容の使用を検討し、JSON (JavaScript Object Notation) には、ページ上の静的メソッドを呼び出す web サービスの呼び出しが呼び出されているかのようにできます。 のメソッドは静的ため、状態は必要ありません。スクリプトの呼び出し元には、パラメーターが指定されて、結果が非同期的に返されます。
- 1 つのコントロールは、アプリケーション全体で複数の場所で使用する必要がある場合は、Web サービスと JSON を使用してください。 もう一度ほとんどの特殊な処理が要求され、非同期的に動作します。

Web サービスまたはページ メソッドを使用して機能を組み込むことにも短所があります。 第一に、ユーザー コントロール (.ascx ファイル) に小規模コンポーネントの機能を構築する通常の ASP.NET 開発者傾向があります。 ページのメソッドは、これらのファイルでホストされることはできません。これらは、実際の .aspx ページ クラス内でホストする必要があります。 同様に、web サービスの場合は、.asmx クラス内でホストされる必要があります。 アプリケーションによっては、このアーキテクチャ違反する可能性が単一の責任の原則をほとんどまたはまったくないまとまり ties が可能性がある 2 つ以上の物理コンポーネントを 1 つのコンポーネントの機能に分散ようになりました。

最後に、アプリケーションでは、Updatepanel が使用されている必要がある場合は、次のガイドラインがトラブルシューティングやメンテナンスを支援する必要があります。

- **入れ子にする Updatepanel が最小限に抑え、だけでなく内での単位もコードのユニット間で。** たとえば、UpdatePanel を制御するには、UpdatePanel を含む別のコントロールを含む、UpdatePanel も含まれていますが、コントロールをラップするページがクロス単位入れ子です。 これにより、どの要素を更新する必要がありますになり予期しない更新 Updatepanel の子を明確にします。
- **保持、*場合、ChildrenAsTriggers*プロパティを false に設定し、イベントをトリガーするを明示的に設定します。** 使用して、`<Triggers>`コレクションはより明確イベントを処理する方法であり、メンテナンス タスクを支援し、開発者は、イベントのオプトインの強制、予期しない動作を防ぐことがあります。
- **機能を実現するのにには、最小の可能な単位を使用します。** 前述の折り返し、郵便サービスの説明で最低限のものだけが、サーバー、合計処理、およびクライアントとサーバーの exchange の使用量を時間を短縮、パフォーマンスが向上します。

## <a name="the-updateprogress-control"></a>UpdateProgress コントロール

## <a name="updateprogress-control-reference"></a>UpdateProgress の制御参照

マークアップが有効なプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| AssociatedUpdate PanelID | 文字列型 | UpdateProgress を報告する必要がある UpdatePanel の ID を指定します。 |
| DisplayAfter | Int | 非同期要求を開始した後、このコントロールが表示される前に、タイムアウト (ミリ秒単位) を指定します。 |
| DynamicLayout | bool | 進行状況を動的にレンダリングするかどうかを指定します。 |

マークアップ子孫:

| **タグ** | **説明** |
| --- | --- |
| &lt;ProgressTemplate&gt; | コントロール テンプレートこのコントロールに表示されるコンテンツのセットにはが含まれています。 |

UpdateProgress コントロールでは、サーバーへの転送に必要な作業を行う際に、ユーザーの関心を保持するためのフィードバックの測定できます。 これはユーザーに通知する作業をしている場合でも、そのわかりにくいかもしれません、ほとんどのユーザーがページを更新して、強調表示、ステータス バーの表示に使用されるために特に役立ちます。

メモとして UpdateProgress コントロールに表示できる任意の場所ページ階層。 ただし、部分的なポストバックが子 UpdatePanel (UpdatePanel が別の UpdatePanel にネストされている) から開始される場合、そのトリガー UpdatePanel では、子の UpdateProgress テンプレートが表示されますが、子をポストバックします。UpdatePanel だけでなく、親 UpdatePanel です。 ただし、トリガーが親 UpdatePanel の直接の子の場合、親に関連付けられている UpdateProgress テンプレートのみが表示されます。

## <a name="summary"></a>概要

Microsoft ASP.NET AJAX 拡張機能は、高度な製品の web コンテンツにアクセスする際に支援するために、web アプリケーションに豊富なユーザー エクスペリエンスを提供するよう設計されています。 ASP.NET AJAX Extensions に、部分ページ レンダリング コントロールの一部としては、ScriptManager を UpdatePanel、および UpdateProgress コントロールを含むはツールキットの最も可視のコンポーネントの一部です。

ScriptManager コンポーネント、拡張機能の JavaScript のクライアントのプロビジョニングの統合だけでなく最小限の開発への投資と連携するさまざまなサーバー側とクライアント側コンポーネントを使用します。

UpdatePanel コントロールは、明確なマジック ボックス - UpdatePanel 内のマークアップはサーバー側の分離コードがあるし、ページの更新はトリガーされません。 UpdatePanel コントロールは、入れ子にすることができ、その他の Updatepanel でのコントロールに依存することができます。 既定では、Updatepanel は、この機能細かくチューニングできる、宣言またはプログラムがその子孫のコントロールによって呼び出された任意のポストバックを処理します。

UpdatePanel コントロールを使用する場合は、開発者が問題の発生可能性のあるパフォーマンスに与える影響の注意してください。 潜在的な代替手段は、アプリケーションの設計を考慮する必要がありますが、web サービスおよびページ メソッドです。

UpdateProgress のコントロールで者が無視されていないと、ページの中に起こっているバック グラウンドの要求がそれ以外の場合のことを知っているユーザーは、何も、ユーザー入力に応答します。 部分的なレンダリングの結果を中止する機能も含まれています。

同時に、これらのツールは、リッチでシームレスなユーザー エクスペリエンスを作成するには、ユーザーに見えること server の作業を行うと、小さいワークフローに割り込むことを支援します。

## <a name="bio"></a>略歴

Scott カテゴリは、1997 年以降の Microsoft の Web テクノロジの使用されているがあり、myKB.com の代表者 ([www.myKB.com](http://www.myKB.com))、専門分野は、ASP.NET の書き込みの際にベースのアプリケーションのナレッジ ベースのソフトウェア ソリューションに重点を置きます。 Scott が接続時に電子メール[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)または彼のブログで[ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[次へ](understanding-asp-net-ajax-updatepanel-triggers.md)
