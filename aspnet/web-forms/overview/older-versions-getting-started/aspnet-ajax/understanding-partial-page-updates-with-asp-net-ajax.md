---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: ASP.NET AJAX の部分的ページ更新を理解する |Microsoft Docs
author: scottcate
description: おそらく、ASP.NET AJAX Extensions の最も目につく機能は、t への完全なポストバックを実行しなくても、部分的なまたは増分ページの更新プログラムを実行する機能しています.
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4883046aa16d5e67b7f0c92e15c897ef1a933b67
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098936"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>ASP.NET AJAX の理解の部分的ページ更新します。
====================
によって[Scott Cate](https://github.com/scottcate)

[PDF のダウンロード](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> おそらく、ASP.NET AJAX Extensions の最も目につく機能はないコードの変更と最小限のマークアップの変更を使用して、サーバーに完全なポストバックを実行しなくても、部分的なまたは増分ページ更新プログラムを実行する機能です。 利点は、広範な – (Adobe Flash や Windows Media) など、マルチ メディアの状態は変更されません、帯域幅のコストが削減されます、およびクライアントでは、通常のポストバックに関連付けられているちらつきは発生しません。


## <a name="introduction"></a>はじめに

マイクロソフトの ASP.NET テクノロジ オブジェクト指向し、イベント ドリブン プログラミング モデルとそれを組み合わせたのコンパイル済みコードの利点があります。 ただし、そのサーバー側の処理モデルでは、テクノロジに固有のいくつかの欠点があります。

- ページの更新プログラムでは、ページの更新を必要とすると、サーバーへのラウンドト リップが必要です。
- ラウンドト リップには、Javascript または他のクライアント側のテクノロジ (Adobe Flash) などによって生成されたすべての効果はありません。
- ポストバック時に、Microsoft Internet Explorer 以外のブラウザーはサポートされませんスクロール位置を自動的に復元します。 および Internet Explorer であってもがありますが、ちらつき、ページが更新されるようにします。
- ポストバックは、高帯域幅の量を必要があります、 \_ \_GridView コントロールまたはリピータなどのコントロールを扱う場合に特に、VIEWSTATE のフォーム フィールドを拡張可能性があります。
- JavaScript または他のクライアント側のテクノロジを Web サービスにアクセスするための統一モデルはありません。

マイクロソフトの ASP.NET AJAX extensions を入力します。 AJAX の略**A**同期**J** avaScript **A** nd **X** ML では、差分ページを提供するための統合フレームワーククロスプラット フォームでの JavaScript を使用して更新プログラムは、Microsoft AJAX フレームワーク、および Microsoft AJAX スクリプト ライブラリと呼ばれるスクリプト コンポーネントで構成されるサーバー側コードで構成されます。 ASP.NET AJAX 拡張機能では、JavaScript を使用して ASP.NET Web サービスにアクセスするクロスプラット フォーム対応のサポートも提供します。

このホワイト ペーパーの検査を ScriptManager コンポーネント、UpdatePanel コントロール、および、UpdateProgress コントロールを含む考慮シナリオでの 必要がありますまたはすることはできませんが、ASP.NET AJAX Extensions のページの部分的な更新プログラムの機能使用されます。

このホワイト ペーパーでは、Visual Studio 2008 の Beta 2 リリースと、ASP.NET AJAX Extensions を (それが以前 ASP.NET 2.0 の使用可能なアドオン コンポーネント) の基本クラス ライブラリに統合された .NET Framework 3.5 に基づいています。 このホワイト ペーパーが Visual Studio 2008 としない Visual Web Developer Express Edition; を使用していることを想定しても参照されている一部のプロジェクト テンプレートは、Visual Web Developer Express のユーザーにできない場合があります。

## <a name="partial-page-updates"></a>ページの部分的な更新プログラム

おそらく、ASP.NET AJAX Extensions の最も目につく機能はないコードの変更と最小限のマークアップの変更を使用して、サーバーに完全なポストバックを実行しなくても、部分的なまたは増分ページ更新プログラムを実行する機能です。 利点は、広範な - (Adobe Flash や Windows Media) など、マルチ メディアの状態は変更されません、帯域幅のコストが削減、およびクライアントでは、通常のポストバックに関連付けられているちらつきは発生しません。

ページの部分的なレンダリングを統合する機能は、プロジェクトに最小限の変更と ASP.NET に統合されます。

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>チュートリアル: 既存のプロジェクトへの部分的なレンダリングの統合


1. Microsoft Visual Studio 2008 では、新しい ASP.NET Web サイト プロジェクトを作成しようとして<em>ファイル</em> <em>- &gt;新規</em> <em>- &gt; のWebサイト</em>し、ダイアログ ボックスから ASP.NET Web サイトを選択します。 ことができます、どのような名前、およびファイル システムまたはインターネット インフォメーション サービス (IIS) にインストールすることがあります。
2. 基本的な ASP.NET マークアップに空の既定のページが表示されます (サーバー側のフォームと`@Page`ディレクティブ)。 という名前のラベルにドロップ`Label1`ボタンと呼ばれると`Button1`フォーム要素内でページ上にします。 自由にそのテキスト プロパティを設定することがあります。
3. デザイン ビューで、ダブルクリック`Button1`分離コードのイベント ハンドラーを生成します。 このイベント ハンドラー内で次のように設定します。 `Label1.Text` 、 をクリックします。 .

**リスト 1。部分的なレンダリングを有効にするには default.aspx のマークアップ**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**リスト 2。(トリム) で default.aspx.cs 分離コード**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. F5 キーを押して、web サイトを起動します。 Visual Studio には、デバッグを有効にする web.config ファイルを追加するように求められますそのためです。 ボタンをクリックするとは、ラベルのテキストを変更するにページが更新され、ページが再描画されるように簡単なちらつきがあることに注意してください。
2. ブラウザー ウィンドウを閉じた後、Visual Studio には、マークアップのページを返します。 Visual Studio のツールボックスで下へスクロールし、[AJAX Extensions] タブを見つけてください。 (以前のバージョンの AJAX または Atlas 拡張機能を使用しているため、このタブがあるありません、このホワイト ペーパーの後半で AJAX の拡張機能のツールボックス項目を登録するためのチュートリアルを参照してください場合や、Windows インストーラーのダウンロード可能な最新バージョンをインストールweb サイトから)。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([フルサイズの画像を表示する をクリックします](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))。


1. <em>既知の問題:</em>Visual Studio 2008 が AJAX Extensions のツールボックス項目をインポート、ASP.NET 2.0 AJAX Extensions にインストールされている、Visual Studio 2005 が既にコンピューターに Visual Studio 2008 をインストールする場合。 コンポーネントにツールヒントを調べることで、大文字と小文字がこれかどうかを判断できます。バージョン 3.5.0.0 言わする必要があります。 バージョン 2.0.0.0 以降と言っているならが、古いツールボックス アイテムをインポートし、Visual Studio のツールボックス アイテムの選択 ダイアログを使用して手動でインポートする必要があります。 デザイナーを使用してバージョン 2 のコントロールを追加することはできません。

2. 前に、`<asp:Label>`タグの開始、空白の行を作成し、ツールボックスの UpdatePanel コントロールをダブルクリックします。 なお、新しい`@Register`ディレクティブを使用して System.Web.UI 名前空間内のコントロールをインポートすることを示す、ページの上部に含まれる、`asp:`プレフィックス。
3. ドラッグ終了`</asp:UpdatePanel>`ラップ ラベルとボタンのコントロールと、要素が整形式にようにボタンの要素の末尾を越えたタグします。
4. オープン後`<asp:UpdatePanel>`タグ、開始新しいタグを開始します。 IntelliSense 確認が求めるされる 2 つのオプションに注意してください。 この場合、作成、`<ContentTemplate>`タグ。 マークアップが正しく構成されているように、ラベルとボタンの周囲には、このタグをラップしてください。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([フルサイズの画像を表示する をクリックします](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))。


1. 任意の場所、`<form>`要素をダブルクリックして、ScriptManager コントロールを含める、`ScriptManager`ツールボックスの項目。
2. 編集、`<asp:ScriptManager>`タグ、属性が含まれるように`EnablePartialRendering= true`します。

**3 を一覧表示します。部分的なレンダリングを有効になっている default.aspx のマークアップ**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Web.config ファイルを開きます。 Visual Studio が System.Web.Extensions.dll への参照をコンパイルを自動的に追加することを確認します。

1. Visual Studio 2008 の新機能新機能。ASP.NET Web サイトで自動的にプロジェクト テンプレートは、ASP.NET AJAX Extensions に必要なすべての参照が含まれています、含まれていますが付属している web.config のコメント追加を有効にすることができる構成情報のセクションのコメント機能。 Visual Studio 2005 では、ASP.NET 2.0 AJAX Extensions がインストールされたときのようなテンプレートがありました。 ただし、Visual Studio 2008 で AJAX の拡張機能は、オプトアウト既定で (つまり、既定では、参照されるが参照として削除されることができます)。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([フルサイズの画像を表示する をクリックします](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))。


1. F5 キーを押して、web サイトを起動します。 部分的なレンダリングをサポートするために必要なソース コードの変更がなかった - マークアップのみが変更された方法に注意してください。

Web サイトを起動するときに、部分的なレンダリングをクリックすると、ボタンがありますがちらつきはされませんも (この例を示していませんが)、ページのスクロール位置の変更があるため、有効になっているようになりましたが表示されます。 ボタンをクリックした後、ページのレンダリングされたソースを確認する場合は、これはことを確認実際には、ポストバックが発生していない - 元のラベル テキストは、ソース マークアップの一部ではまだ JavaScript を使用、ラベルが変更されました。

Visual Studio 2008 は、ASP.NET AJAX 対応の web サイトの定義済みのテンプレートが付属してには表示されません。 ただし、Visual Studio 2005 および ASP.NET 2.0 AJAX Extensions がインストールされた場合は、このようなテンプレートが Visual Studio 2005 内で使用できます。 その結果、AJAX-Enabled Web Site テンプレートで、web サイトを構成し、開始があります、さらに簡単にテンプレートが完全に構成された web.config ファイルが (Web サービスのアクセスなど、ASP.NET AJAX Extensions のすべてのサポートを含める必要があります。および JSON のシリアル化の JavaScript Object Notation) と、UpdatePanel と ContentTemplate を既定では、メインの Web フォーム ページ内に含まれています。 この既定のページの部分的なレンダリングを有効にすることは、このチュートリアルの手順 10 を再考し、ページ上にコントロールを削除する同じくらい簡単です。

## <a name="the-scriptmanager-control"></a>ScriptManager コントロール

## <a name="scriptmanager-control-reference"></a>ScriptManager コントロールの参照

マークアップが有効なプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | エラーを処理するために、web.config ファイルのカスタム エラー セクションを使用するかどうかを指定します。 |
| AsyncPostBackError-Message | String | 取得または設定エラーが発生した場合、クライアントに送信するエラー メッセージ。 |
| AsyncPostBack-Timeout | Int32 | 取得または既定のクライアントが完了する非同期要求を待機する時間の量を設定します。 |
| EnableScript-Globalization | Bool | 取得またはスクリプトのグローバル化が有効になっているかどうかを設定します。 |
| EnableScript-Localization | Bool | 取得またはスクリプトのローカライズが有効になっているかどうかを設定します。 |
| ScriptLoadTimeout | Int32 | クライアントにスクリプトを読み込むために許容される秒数を決定します。 |
| ScriptMode | 列挙型 (Auto、デバッグ、リリース、継承) | 取得またはリリース バージョンのスクリプトをレンダリングするかどうかを設定します |
| ScriptPath | String | 取得またはクライアントに送信するスクリプト ファイルの場所にルート パスを設定します。 |

コードのみのプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | クライアントに送信される ASP.NET 認証サービス プロキシの詳細を取得します。 |
| IsDebuggingEnabled | Bool | 取得するかどうかスクリプトを作成し、コードのデバッグが有効になっています。 |
| IsInAsyncPostback | Bool | ページの非同期ポストバック要求が現在あるかどうかを取得します。 |
| ProfileService | ProfileService-Manager | クライアントに送信される ASP.NET プロファイリング サービス プロキシの詳細を取得します。 |
| スクリプト | コレクション&lt;スクリプト参照&gt; | クライアントに送信されるスクリプト参照のコレクションを取得します。 |
| Services | コレクション&lt;サービス参照&gt; | クライアントに送信される Web サービス プロキシ参照のコレクションを取得します。 |
| SupportsPartialRendering | Bool | 現在のクライアントは、部分的なレンダリングをサポートしているかどうかを取得します。 このプロパティを返す場合**false**、すべてのページ要求が標準的なポストバックになります。 |

コードのパブリック メソッド:

| **メソッド名** | **Type** | **説明** |
| --- | --- | --- |
| SetFocus(string) | Void | 要求が完了したときに、特定のコントロールに、クライアントのフォーカスを設定します。 |

マークアップの子孫。

| **タグ** | **説明** |
| --- | --- |
| &lt;AuthenticationService&gt; | ASP.NET 認証サービスへのプロキシの詳細を提供します。 |
| &lt;ProfileService&gt; | ASP.NET のサービスをプロファイリングするプロキシの詳細を提供します。 |
| &lt;スクリプト&gt; | その他のスクリプト参照を提供します。 |
| &lt;asp:ScriptReference&gt; | 特定のスクリプト参照を表します。 |
| &lt;サービス&gt; | 生成されるプロキシ クラスを持つその他の Web サービス参照を提供します。 |
| &lt;asp:ServiceReference&gt; | 特定の Web サービス参照を表します。 |

ScriptManager コントロールは、ASP.NET AJAX 拡張機能の重要なコアです。 (広範なクライアント側スクリプトの型システムを含む)、スクリプト ライブラリにアクセスできるように、部分的なレンダリングをサポートし、(認証と、プロファイルも他の Web サービス) などの他の ASP.NET サービスの広範なサポートを提供します。 ScriptManager コントロールでは、クライアント スクリプトのグローバリゼーションおよびローカリゼーションのサポートも提供します。

## <a name="providing-alternative-and-supplemental-scripts"></a>代替と補足スクリプト

開発者が自由にカスタマイズしたスクリプト ファイルに、ScriptManager をリダイレクトしたり、登録中に、Microsoft ASP.NET 2.0 AJAX Extensions では、両方のデバッグに全体のスクリプト コードを追加、参照されたアセンブリに埋め込まれたリソースとしてエディションを離すと、必要なスクリプトを追加します。

登録できます (Sys.WebForms 名前空間とカスタム入力システムをサポートするもの) などの通常が含まれているスクリプトの既定のバインディングを無効にする、 `ResolveScriptReference` ScriptManager クラスのイベント。 問題のスクリプト ファイルへのパスを変更する機会がイベント ハンドラーには、このメソッドが呼び出されると、します。スクリプト マネージャーは、クライアントに、スクリプトのさまざまなまたはカスタマイズされたコピーが送信されます。

さらに、スクリプト参照 (によって表される、`ScriptReference`クラス) マークアップを使用してプログラムから、または含めることができます。 これを行うには、プログラムで変更するか、`ScriptManager.Scripts`コレクション、または含める`<asp:ScriptReference>`タグの下、`<Scripts>`タグで、ScriptManager コントロールの最初のレベルの子です。

## <a name="custom-error-handling-for-updatepanels"></a>カスタム エラー処理を Updatepanel

UpdatePanel コントロールによって指定されたトリガーによって、更新プログラムの処理が、エラー処理とカスタム エラー メッセージのサポートは、ページの ScriptManager コントロールのインスタンスによって処理されます。 これには、イベントの公開を`AsyncPostBackError`できるページにし、カスタム例外処理ロジックを提供します。

指定することがあります AsyncPostBackError イベントを消費することによって、`AsyncPostBackErrorMessage`プロパティで、コールバックの完了時に発生する警告ボックスが表示されます。

クライアント側のカスタマイズが既定の警告ボックス; を使用する代わりにこともできます。たとえば、カスタマイズされた表示したい場合があります`<div>`既定のブラウザーのモーダル ダイアログ ボックスではなく要素。 この場合、クライアント スクリプトでエラーを処理できます。

**5 を一覧表示します。カスタム エラーを表示するクライアント側スクリプト**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

簡単に言えば、上記のスクリプトは、非同期の要求が完了したときのクライアント側 AJAX ランタイムでコールバックを登録します。 これは、後でエラーが報告されたかどうかをチェックし、そうである場合でカスタム スクリプトでエラーが処理されたことをランタイムに最後に示すは、詳細を処理します。

## <a name="globalization-and-localization-support"></a>グローバリゼーションとローカライズのサポート

ScriptManager コントロールは、スクリプトの文字列とユーザー インターフェイス コンポーネントのローカリゼーションの広範なサポートを提供しますただし、このトピックでは、このホワイト ペーパーの範囲外です。 詳細については、ASP.NET AJAX extensions のグローバル化のサポートに関するホワイト ペーパーを参照してください。

## <a name="the-updatepanel-control"></a>UpdatePanel コントロール

## <a name="updatepanel-control-reference"></a>UpdatePanel コントロールのリファレンス

マークアップが有効なプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | 子コントロールがポストバックで更新を自動的に起動するかどうかを指定します。 |
| RenderMode | 列挙型 (ブロック、インライン) | 方法は、コンテンツを視覚的に表示されますを指定します。 |
| UpdateMode | 列挙型 (常に、条件付き) | 部分的なレンダリング中に、UpdatePanel が常に更新されるかどうか、またはトリガーがヒットしたときにのみ更新されるかどうかを指定します。 |

コードのみのプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| IsInPartialRendering | bool | Updatepanel コントロールが現在の要求の部分的なレンダリングをサポートするかどうかを取得します。 |
| ContentTemplate | ITemplate | 更新の要求のマークアップのテンプレートを取得します。 |
| ContentTemplateContainer | コントロール | 更新の要求をプログラムでテンプレートを取得します。 |
| トリガー | UpdatePanel の TriggerCollection | 現在の updatepanel コントロールに関連付けられているトリガーの一覧を取得します。 |

コードのパブリック メソッド:

| **メソッド名** | **Type** | **説明** |
| --- | --- | --- |
| Update() | Void | 指定した updatepanel コントロールをプログラムで更新します。 それ以外の場合いつ UpdatePanel の部分的なレンダリングをトリガーするサーバーの要求を許可します。 |

マークアップの子孫。

| **タグ** | **説明** |
| --- | --- |
| &lt;ContentTemplate&gt; | 部分的なレンダリングの結果を表示するために使用するマークアップを指定します。 子&lt;asp: UpdatePanel&gt;します。 |
| &lt;トリガー&gt; | コレクションを指定します*n*この UpdatePanel の更新に関連するコントロール。 子&lt;asp: UpdatePanel&gt;します。 |
| &lt;asp:AsyncPostBackTrigger&gt; | 特定の UpdatePanel の部分ページ レンダリングを起動するトリガーを指定します。 これは、問題の UpdatePanel の子としてコントロールができない可能性がありますもかまいません。 イベント名を管理できます。子&lt;トリガー&gt;します。 |
| &lt;asp:PostBackTrigger&gt; | 全体のページが更新を実行するコントロールを指定します。 これは、問題の UpdatePanel の子としてコントロールができない可能性がありますもかまいません。 オブジェクトを管理できます。 子&lt;トリガー&gt;します。 |

`UpdatePanel`コントロールがコントロールを AJAX Extensions の部分的なレンダリング機能に参加するサーバー側のコンテンツを区切る。 ページで、可能性のある UpdatePanel コントロールの数に制限はありませんし、入れ子にできます。 個別に各作業できるように、各 UpdatePanel が分離された、(ページのポストバックの独立した、ページのさまざまな部分のレンダリング、同時に実行されている 2 つ Updatepanel を保持できます)。

UpdatePanel コントロールのトリガーを使用して - 既定では、UpdatePanel の内に含まれる任意のコントロールを主に取引を制御する`ContentTemplate`ポストバックを作成する、UpdatePanel のトリガーとして登録されます。 つまりは、UpdatePanel は、ユーザー コントロールに GridView)、(既定のデータ バインド コントロールを使用できます、スクリプトのようにプログラミングできます。

既定では、ページ上のすべての UpdatePanel コントロールが更新される部分ページ レンダリングがトリガーされたときに、このようなアクションのトリガーが定義されているかどうか、UpdatePanel を制御します。 たとえば、1 つの UpdatePanel ボタン コントロールの定義、そのボタン コントロールがクリックされた場合は、そのページ上のすべての UpdatePanel コントロールは既定で更新されます。 これは、既定で、 `UpdateMode` updatepanel コントロールのプロパティに設定されて`Always`します。 または、UpdateMode プロパティを設定できます`Conditional`、特定のトリガーがヒットした場合は、UpdatePanel の更新されることのみを意味します。

## <a name="custom-control-notes"></a>カスタム コントロールのノート

UpdatePanel をユーザー コントロールまたはカスタム コントロールに追加できます。ただし、これらのコントロールが含まれています ページには EnablePartialRendering に設定するプロパティを使用して ScriptManager コントロールが含める必要がありますも**true**します。

1 つの方法がありますを考慮するこの Web カスタム コントロールを使用して、保護されたオーバーライドとすると`CreateChildControls()`のメソッド、`CompositeControl`クラス。 これにより、ページは、部分的なレンダリングは; をサポートするいると判断した場合、UpdatePanel コントロールの子と外部との間を挿入できます。コンテナー内に子コントロールを単にレイヤーをそれ以外の場合、`Control`インスタンス。

## <a name="updatepanel-considerations"></a>UpdatePanel の考慮事項

UpdatePanel は、JavaScript の XMLHttpRequest のコンテキスト内での ASP.NET ポストバックのラッピング黒いボックスのものとして動作します。 ただし、両方の面での動作と速度の点に大幅なパフォーマンスの考慮事項があります。 使用が適切な場合に最適な決定できますは、UpdatePanel のしくみを理解するには、AJAX exchange を調べる必要があります。 次の例では、(Firebug は、XMLHttpRequest のデータをキャプチャ) Firebug 拡張機能で、既存のサイトと、Mozilla Firefox を使用します。

その他のものをフォームまたはコントロールの市区町村と都道府県のフィールドを設定することになっている postal code テキスト ボックスがあるフォームを検討してください。 このフォームは、最終的には、ユーザーの名前、アドレス、および連絡先情報を含む、メンバーシップ情報を収集します。 特定のプロジェクトの要件に基づいて、考慮に入れるさまざまな設計の考慮事項があります。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([フルサイズの画像を表示する をクリックします](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([フルサイズの画像を表示する をクリックします](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))。


このアプリケーションの元のイテレーションでは、コントロールは、郵便番号、市区町村、および状態を含む、ユーザー登録データの全体を組み込むことでビルドされました。 コントロール全体が、UpdatePanel 内でラップし、Web フォームにドロップします。 ユーザーが郵便番号を入力すると、UpdatePanel は、イベント (バックエンド、トリガーを指定することで、または ChildrenAsTriggers プロパティが true に設定を使用して、いずれかに対応する TextChanged イベント) を検出します。 AJAX ポストバック、UpdatePanel 内のフィールドのすべて FireBug によってキャプチャされます (右側の図を参照してください)。

画面キャプチャに示すよう、UpdatePanel 内のすべてのコントロールからの値が配信されます (ここではすべて空)、ViewState フィールドとします。 話を超える 9 kb のデータ送信、実際には 5 バイトのデータのみがこの特定の要求を行うために必要なときにされます。 応答は、さらに肥大化: 合計では、クライアントに 57 kb を送信テキスト フィールドおよびドロップダウン フィールドを更新するだけです。

ASP.NET AJAX がプレゼンテーションを更新する方法を確認する関心のあることもあります。 UpdatePanel の更新要求の応答部分に示した左側; Firebug コンソールの表示特別に作成パイプで区切られた文字列でクライアント スクリプトによって分割し、ページの再構築することをお勧めします。 具体的には、ASP.NET AJAX の設定、 *innerHTML* UpdatePanel を表すクライアント上の HTML 要素のプロパティ。 ブラウザーは、DOM を再生成、処理する必要がある情報の量によって、わずかに遅延があります。

DOM の再生成には、さまざまなその他の問題がトリガーされます。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([フルサイズの画像を表示する をクリックします](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))。


- フォーカスがある HTML 要素は、UpdatePanel 内では、フォーカスが失われます。 そのため、郵便番号のテキスト ボックスを終了する Tab キーが押されたユーザーが次の宛先がでした都道府県のテキスト ボックス。 ただし、UpdatePanel には、表示が更新されるとは、フォームには、フォーカスがあったが不要になったし、Tab キーを押してが開始した (リンク) などのフォーカス要素の強調表示します。
- カスタムのクライアント側スクリプトの任意の型が使用されて関数で DOM 要素をアクセス、参照が保持されている場合場合がありますが機能しなくなります部分ポストバック後にします。

Updatepanel は、包括的なソリューションではありません。 代わりに、特定の状況は、プロトタイプ作成、更新プログラムの小規模な制御などの簡単な解決方法を提供し、使い慣れたインターフェイスを持つ可能性があるが DOM ではないのため、.NET オブジェクト モデルを使い慣れた ASP.NET 開発者に提供 アプリケーション シナリオによっては、パフォーマンスが向上につながる可能性のある選択肢を数多くあります。

- PageMethods と JSON (JavaScript Object Notation) を使用してにより、開発者は、web サービス呼び出しが呼び出される場合、ページ上の静的メソッドを呼び出すことを検討してください。 メソッドは静的であるため状態必要はありません。スクリプトの呼び出し元は、パラメーターを提供し、非同期的に結果が返されます。
- 1 つのコントロールは、アプリケーション全体で複数の場所で使用する必要がある場合は、Web サービスと JSON を使用してください。 これは再び、ほとんどの特殊な処理が必要ですし、非同期的に動作します。

Web サービスまたはページ メソッドを介して機能を組み込むには、欠点もがあります。 第一に、ユーザー コントロール (.ascx ファイル) に小規模コンポーネントの機能を構築する通常の ASP.NET 開発者傾向があります。 ページ メソッドは、これらのファイルでホストされることはできません。これらは、実際の .aspx ページのクラス内でホストする必要があります。 同様に、web サービスの場合は、.asmx クラス内でホストする必要があります。 アプリケーションによっては、その 1 つのコンポーネントの機能がまとまりのある ties がほとんどまたはまったくないことがある 2 つ以上の物理コンポーネント間で分散ようになりましたこのアーキテクチャが単一責任の原則を侵害する可能性があります。

最後に、アプリケーションでは、Updatepanel を使用する必要がある場合、次のガイドラインはトラブルシューティングやメンテナンスを支援する必要があります。

- **コードの単位も Updatepanel が最小限に抑え、だけでなく内でユニット、入れ子にします。** たとえば、UpdatePanel を制御するには、UpdatePanel は、UpdatePanel を格納している別のコントロールが含まれているも含まれていますが、コントロールをラップするページでは間単位の入れ子。 これにより、どの要素が、更新する必要があり、Updatepanel の子に予期しない更新を防止を明確にします。
- **保持、 *ChildrenAsTriggers*プロパティを false に設定し、イベントをトリガーするを明示的に設定します。** 使用して、`<Triggers>`コレクションのイベントを処理する方が明確な方法し、でのメンテナンス タスクを支援し、開発者は、イベントのオプトインを強制的に、予期しない動作ができない可能性があります。
- **機能を実現するのにには、最小の可能な単位を使用します。** 最低限でのみ、サーバー、合計処理、およびクライアントとサーバーの exchange のフット プリントに時間が短縮の折り返し、郵便サービスの説明で説明したように、パフォーマンスの向上。

## <a name="the-updateprogress-control"></a>UpdateProgress コントロール

## <a name="updateprogress-control-reference"></a>UpdateProgress コントロールのリファレンス

マークアップが有効なプロパティ:

| **プロパティ名** | **Type** | **説明** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | String | この UpdateProgress を報告する必要がある updatepanel コントロールの ID を指定します。 |
| DisplayAfter | Int | 非同期要求を開始した後、このコントロールが表示される前に、(ミリ秒単位)、タイムアウトを指定します。 |
| DynamicLayout | bool | 進行状況を動的にレンダリングするかどうかを指定します。 |

マークアップの子孫。

| **タグ** | **説明** |
| --- | --- |
| &lt;ProgressTemplate&gt; | このコントロールに表示されるコンテンツの設定、コントロール テンプレートが含まれています。 |

UpdateProgress コントロールは、サーバーへの転送に必要な処理を行う際に、ユーザーの関心を保持するフィードバックのメジャーを提供します。 ユーザーには、する作業をしている場合でも、そのわかりにくいかもしれません、更新、および強調表示、ステータス バーが表示されるページにほとんどのユーザーが使用されるために特に役立ちます。

UpdateProgress コントロールに表示できる任意の場所 ページの階層。 ただし、部分ポストバックが子 (UpdatePanel が別の UpdatePanel 内で入れ子になって) UpdatePanel からが開始した場合、ポストバック UpdatePanel では、子の UpdateProgress テンプレートが表示されますが、子をトリガーします。UpdatePanel と UpdatePanel の親。 ただし、トリガーが UpdatePanel、親の直接の子の場合、親に関連付けられている UpdateProgress テンプレートのみが表示されます。

## <a name="summary"></a>まとめ

Microsoft ASP.NET AJAX 拡張機能は、web コンテンツをよりアクセスできるように支援するために、web アプリケーションに豊富なユーザー エクスペリエンスを提供する高度な製品です。 ASP.NET AJAX の拡張機能であり、ページの部分的なレンダリング コントロールの一部として ScriptManager、UpdatePanel、および UpdateProgress コントロールを含むいくつかのツールキットの最も目立つコンポーネント。

ScriptManager コンポーネントの拡張機能が、クライアントの JavaScript のプロビジョニングの統合だけでなく最小限の開発の投資と連動するさまざまなサーバー側とクライアント側コンポーネントを使用します。

UpdatePanel コントロールは、明らかなマジック ボックス - UpdatePanel 内のマークアップのサーバー側の分離コードがあるし、ページの更新がトリガーされないことができます。 UpdatePanel コントロールは、入れ子にすることができで他の Updatepanel コントロールに依存することができます。 既定では、Updatepanel は、この機能細かくチューニングできる、宣言またはプログラムがその子孫のコントロールによって呼び出されるすべてのポストバックを処理します。

UpdatePanel コントロールを使用する場合は、開発者が潜在的に発生する可能性がパフォーマンスに与える影響に注意してくださかった。 必要があります。 潜在的な代替手段には、web サービスとページ メソッドが含まれますが、アプリケーションの設計を考慮する必要があります。

コントロールで者を無視されてはいないは、ページの中にでバック グラウンドで要求が行われていることを知っているユーザーは、UpdateProgress は何ユーザー入力に応答します。 部分的なレンダリングの結果を中止する機能も含まれています。

同時に、これらのツールは、server の作業をユーザーに見えることに作成し、小さいワークフローを中断することによって、リッチでシームレスなユーザー エクスペリエンスの作成を支援します。

## <a name="bio"></a>自己紹介

1997 年からマイクロソフトの Web テクノロジで働いてあり myKB.com プレジデント、Scott Cate ([www.myKB.com](http://www.myKB.com)) ベースのナレッジ ベースのソフトウェア ソリューションに重点を置いてアプリケーションを ASP.NET の記述を専門としています。 Scott は時に電子メールが接続可能[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)またはで彼のブログ[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Next](understanding-asp-net-ajax-updatepanel-triggers.md)
