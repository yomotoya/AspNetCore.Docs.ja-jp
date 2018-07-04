---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: ASP.NET AJAX UpdatePanel トリガーについて |Microsoft Docs
author: scottcate
description: Visual Studio でのマークアップ エディターで作業しているときにお気付き (IntelliSense) から、UpdatePanel コントロールの 2 つの子要素があることです。 値を下回った場合のいずれかを指定しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 180491b6aee188a9dca750d6d325217f1ad5212c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363722"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>ASP.NET AJAX UpdatePanel トリガーをについてください。
====================
によって[Scott Cate](https://github.com/scottcate)

[PDF のダウンロード](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Visual Studio でのマークアップ エディターで作業しているときにお気付き (IntelliSense) から、UpdatePanel コントロールの 2 つの子要素があることです。 ページ (または 1 つを使用している場合、ユーザー コントロール) 上のコントロールを指定するトリガー要素は、その 1 つの要素が存在する UpdatePanel コントロールは、部分的なレンダリングがトリガーされます。


## <a name="introduction"></a>はじめに

マイクロソフトの ASP.NET テクノロジ オブジェクト指向し、イベント ドリブン プログラミング モデルとそれを組み合わせたのコンパイル済みコードの利点があります。 ただし、そのサーバー側の処理モデルでは、Microsoft ASP.NET 3.5 の AJAX Extensions に含まれる新しい機能では多くのアドレスを指定することができます、テクノロジ固有のいくつかの欠点があります。 これらの拡張機能は、ページ全体の更新では、(ASP.NET のプロファイル API を含む)、クライアント スクリプトと広範なクライアント側 API を使用して Web サービスにアクセスする機能を必要とせずにページの部分的なレンダリングを含む、多くの新機能豊富なクライアント機能を有効にします。ASP.NET サーバー側コントロール セットに表示されるコントロール パターンの多くをミラーに設計されています。

このホワイト ペーパーでは、ASP.NET AJAX の XML のトリガー機能を調べて`UpdatePanel`コンポーネント。 XML のトリガーでは、特定の UpdatePanel コントロールの部分的なレンダリングを引き起こす可能性のあるコンポーネントの詳細な制御を提供します。

このホワイト ペーパーでは、.NET Framework 3.5 のベータ 2 リリースと Visual Studio 2008 に基づきます。 ASP.NET AJAX の拡張機能であり、以前 ASP.NET 2.0 を対象としたアドオン アセンブリは .NET Framework 基本クラス ライブラリに統合されました。 このホワイト ペーパー Visual Studio 2008 のない Visual Web Developer Express で使用される (ただし、コードの一覧が完全な互換性に関係なくするには、Visual Studio のユーザー インターフェイスに従って概説はし、、も前提としています開発環境の場合)。

## <a name="triggers"></a>*トリガー*

既定では、特定の UpdatePanel のトリガーが自動的に (たとえば) を持つテキスト ボックス コントロールを含む、ポストバックを起動する子コントロールを含める、`AutoPostBack`プロパティに設定**true**します。 ただし、トリガー含めることもできます。 マークアップを使用して宣言内でこれは、 `<triggers>` UpdatePanel コントロールの宣言のセクション。 トリガーを使用してアクセスできますが、`Triggers`コレクション プロパティ、それが推奨されます (たとえば、デザイン時に、コントロールを使用できません) 場合は、実行時に、部分的なレンダリングのトリガーを登録することを使用して、`RegisterAsyncPostBackControl(Control)`のメソッド、内部オブジェクト、ページの ScriptManager、`Page_Load`イベント。 ページはステートレスで、これは再登録するこれらのコントロールが作成されるたびにことに注意してください。

子の自動トリガー包含も無効にできます (そのポストバックを作成する子コントロールは、自動的に部分的なレンダリングをトリガーしません) を設定して、`ChildrenAsTriggers`プロパティを**false**します。 これによりどの特定のコントロールを割り当てることで最大の柔軟性、ページ レンダリングを呼び出すことができますし、お勧め、開発者はオプトイン可能性があるすべてのイベントを処理するのではなく、イベントに応答できるようにできます。

UpdatePanel コントロールは入れ子になったときに、UpdateMode 設定されている場合注意してください**条件付き**子 UpdatePanel がトリガーされますが、UpdatePanel が更新されます子のみが、親ではないの場合は、します。 ただし、親 UpdatePanel が更新された場合、子 UpdatePanel も更新されます。

## <a name="the-lttriggersgt-element"></a>*&lt;トリガー&gt;要素*

Visual Studio でのマークアップ エディターで作業しているときにすることがありますに注意してください (IntelliSense) から 2 つの子要素があることの`UpdatePanel`コントロール。 最も多く見要素は、`<ContentTemplate>`要素は、本質的に更新パネルによって保持されるコンテンツをカプセル化する (コンテンツの部分的なレンダリングを有効にします)。 その他の要素が、`<Triggers>`ページ (または 1 つを使用している場合、ユーザー コントロール) 上のコントロールを指定する、要素を UpdatePanel コントロールの部分的なレンダリングをトリガーする、&lt;トリガー&gt;要素が存在します。

`<Triggers>`要素は、任意の数ごとの 2 つの子ノードを含めることができます:`<asp:AsyncPostBackTrigger>`と`<asp:PostBackTrigger>`します。 2 つの属性が受け入れる`ControlID`と`EventName`、カプセル化の現在の単位内の任意のコントロールを指定することができます (たとえば、Web ユーザー コントロール内で、UpdatePanel コントロールが存在する場合をしようとしないでくださいでコントロールを参照ページ、ユーザー コントロールが存在する)。

`<asp:AsyncPostBackTrigger>`要素は、の子として存在するコントロールからイベントをターゲットにできることに特に便利です*任意*UpdatePanel コントロールをこのトリガーは、子 UpdatePanel だけでなく、カプセル化の単位. したがって、部分ページ更新を開始する任意のコントロールを作成できます。

同様に、`<asp:PostBackTrigger>`部分ページ レンダリング、トリガーが、サーバーへの完全なラウンドト リップが必要な要素を使用できます。 このトリガーの要素がコントロールは通常、部分ページ レンダリングがトリガーするそれ以外の場合と、完全なページ レンダリングを強制することもできます (時など、`Button`コントロールが、 `<ContentTemplate>` UpdatePanel コントロールの要素)。 ここでも、PostBackTrigger 要素は、カプセル化の現在の単位の UpdatePanel コントロールの子である任意のコントロールを指定できます。

## <a name="lttriggersgt-element-reference"></a>*&lt;トリガー&gt;要素のリファレンス*

*マークアップの子孫。*

| **タグ** | **説明** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | コントロールと、部分ページ更新をこのトリガーの参照を含んでいる UpdatePanel の原因となるイベントを指定します。 |
| &lt;asp:PostBackTrigger&gt; | コントロールと、完全なページ更新 (ページ全体の更新) が発生するイベントを指定します。 このタグは、コントロールは、部分的なレンダリングをトリガーするそれ以外の場合と、完全な更新を強制的に使用できます。 |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*チュートリアル: クロス UpdatePanel トリガー*

1. Scriptmanager コントロール オブジェクトが部分的なレンダリングを有効にする設定で新しい ASP.NET ページを作成します。 最初のこのページに 2 つの Updatepanel を追加、ラベル コントロール (Label1) と 2 つのボタン コントロール (Button1 と Button2) が含まれます。 Button1 の両方を更新する をクリックする必要がありますと答えてし、Button2 する必要があります、または次のようなこれらの行を更新する をクリックします。 2 番目の UpdatePanel では、ラベル コントロール (Label2) のみを含めるを区別するために既定値以外に、前景色プロパティを設定します。
2. 両方の UpdatePanel タグの UpdateMode プロパティを設定**条件付き**します。

**Default.aspx のリスト 1: マークアップ:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Button1 のクリック イベント ハンドラーで何か (DateTime.Now.ToLongTimeString()) など時間に依存する Label1.Text と Label2.Text を設定します。 Button2 の Click イベント ハンドラー、Label1.Text のみ時間依存の値に設定します。

**Default.aspx.cs の (切り捨て)、リスト 2: 分離:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. F5 キーを押して、プロジェクトをビルドして実行します。 更新の両方のパネルをクリックするとは、両方のラベルのテキストと変更に注意してください。ただし、このパネルの Update をクリックすると、Label1 のみに更新します。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))。


## <a name="under-the-hood"></a>*内部的な処理*

作成したばかりの例を利用することができますを見ていきます、ASP.NET AJAX が行っている内容と、UpdatePanel クロス パネル トリガーの動作します。 FireBug - と呼ばれる、Mozilla Firefox 拡張機能と同様に、生成されたページのソース HTML、いたしますそのためには、AJAX ポストバックを簡単に確認できます。 また、Lutz Roeder の .NET Reflector ツールを使用します。 どちらのツールは、オンラインで自由に利用できると、インターネット検索で見つかんだことができます。

ページのソース コードの検査を示していません。 通常どおりほぼ何もUpdatePanel コントロールとしてレンダリングされます`<div>`コンテナー、および私たちスクリプト リソースには提供が含まれていますを確認できますが、 `<asp:ScriptManager>`。 いくつか新しい AJAX 固有 AJAX のクライアント スクリプト ライブラリの内部に呼び出し、PageRequestManager にもあります。 最後に、2 つの UpdatePanel コンテナー - で、表示されたいずれかがわかります`<input>`、2 つのボタン`<asp:Label>`としてコントロールがレンダリング`<span>`コンテナー。 (FireBug で DOM ツリーを検査するわかることを表示されるコンテンツを生成しないされることを示すために、ラベルが淡色表示されます)。

このパネルの更新 ボタンをクリックして、現在のサーバー時刻で最上位の UpdatePanel が更新されますを注意してください。 、FireBug では、要求を確認するようにコンソール タブを選択します。 最初に POST 要求パラメーターを確認します。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))。


UpdatePanel に示されて、サーバー側の AJAX コード ScriptManager1 パラメーターを使用してコントロール ツリーが発生した正確な注:`Button1`の`UpdatePanel1`コントロール。 両方のパネルの更新 ボタンをクリックします。 パイプで区切られた一連の変数を文字列に設定をわかります応答を検証するには、次に、具体的には、最上位の UpdatePanel がわかります`UpdatePanel1`が全体、HTML ブラウザーに送信します。 AJAX のクライアント スクリプト ライブラリには、UpdatePanel の元の HTML コンテンツを使用して新しいコンテンツが置換されます、`.innerHTML`プロパティ、ため、サーバーは、HTML としてサーバーから変更されたコンテンツを送信します。

ここで、更新の両方のパネルのボタンをクリックし、サーバーから結果を確認します。 結果は非常に似ています - 両方の Updatepanel がサーバーから新しい HTML を受信します。 前のコールバックと同様、追加のページの状態が送信されます。

ご覧のとおり、AJAX ポストバックを実行する特別なコードを使用しないため、AJAX のクライアント スクリプト ライブラリがコードを追加しなくても、フォームのポストバックをインターセプトします。 サーバー コントロールは、フォームを自動的に送信しない - ASP.NET は、主に自動スクリプト リソースを含めること、PostBackOptions クラスによって実現フォーム検証および状態は既に、コードを自動的に挿入するために自動的に JavaScript を利用します。、、ClientScriptManager クラス。

たとえば、チェック ボックス コントロール.NET Reflector でクラスの逆アセンブリを調べます。 これを行うには、System.Web アセンブリが、開いていることを確認しに移動します、`System.Web.UI.WebControls.CheckBox`クラス、開く、`RenderInputTag`メソッド。 チェックする条件を探して、`AutoPostBack`プロパティ。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))。


自動ポストバックが有効な場合、 `CheckBox` (、AutoPostBack プロパティが true) を使用して、結果を制御`<input>`ASP.NET イベントでのスクリプトの処理されたタグが表示されるため、`onclick`属性。 次に、フォームの送信の傍受は、互換性に影響する可能性がある不正確な文字列置換を使用することによって発生する可能性の変更可能性を回避するために支援 nonintrusively、ページに挿入する ASP.NET AJAX を使用できます。 これにより、さらに、*任意*コードを追加しなくても、UpdatePanel のコンテナー内での使用をサポートするために ASP.NET AJAX の機能を利用するカスタムの ASP.NET コントロール。

`<triggers>` PageRequestManager の呼び出しで初期化される値に対応する機能\_updateControls (注、メソッド、イベント、およびフィールドの名前が始まる、ASP.NET AJAX のクライアント スクリプト ライブラリは、規則を使用して。アンダー スコアでは内部としてマークされて、ライブラリ自体の外部で使用するためのものではありません)。 コントロールは、AJAX ポストバックが発生することを確認できます。

たとえば、Updatepanel の外部で 1 つのコントロールを完全に終了し、UpdatePanel 内で 1 つのままにして、ページにしましょう 2 つの他のコントロールを追加します。 上部の UpdatePanel 内のチェック ボックス コントロールを追加いたします。 そのリスト内で定義されている色の数が、DropDownList を削除します。 新しいマークアップを次に示します。

**新しいマークアップの 3 を一覧表示します。**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

新しい分離コードを次に示します。

**リスト 4: 分離コード**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

このページの背後にある考え方は、ドロップダウン リストが 2 つ目のラベルを表示する 3 つの色のいずれかを選択することで、両方が太字かどうかと、ラベルが、日付と時刻を表示するかどうかのチェック ボックスを決定することです。 チェック ボックスには、AJAX の更新する必要がありますされませんが、ドロップダウン リストは、しない、UpdatePanel 内で格納されている場合でも。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))。


上記のスクリーン ショットで明らかには、最近のボタンがクリックされるは下部にある時間に依存しない、最上位の時間を更新する更新このパネルの右ボタンをしました。 日付もオフになって、数回のクリックの間で日付が一番下のラベルに表示します。 最後に関心のあるは、下部にあるラベルの色: コントロールの状態が重要なことについては、ラベルのテキストよりも最近更新されたユーザーだと思って AJAX ポストバック中に保持します。 *ただし*時刻は更新されませんでした。 時間がの永続性を自動的に再生成、 \_\_コントロールがサーバーに再表示されているときに、ASP.NET ランタイムによって解釈されるページの VIEWSTATE フィールド。 ASP.NET AJAX サーバー コードでは、状態を変更する方法、コントロールは認識しませんビューステートから単純に再作成し、適切なイベントが実行されます。

これは、指摘、ただし、いたは初期化されているページ内で時間\_Load イベントでは、時間がインクリメントされていた正しくします。 その結果、開発者が、適切なイベント ハンドラーの中に、適切なコードが実行されることに注意してくださいし、ページの使用を回避する必要があります\_コントロールのイベント ハンドラーが適切なときにロードします。

## <a name="summary"></a>まとめ

ASP.NET AJAX 拡張機能の UpdatePanel コントロールは、用途が広く、多くのメソッドを更新する必要がありますが、コントロールのイベントを識別するために利用できます。 その子コントロールによって自動的に更新されているサポートしていますが、ページ上のコントロール イベントにも応答できます。

サーバーの処理負荷が発生する可能性を削減することが推奨されます、 `ChildrenAsTriggers` UpdatePanel のプロパティに設定する`false`、10 営業日に既定で含まれているのではなくイベントであるとします。 これも検証、および入力フィールドへの変更など、望ましくない可能性の効果の原因から不要なイベントを防止します。 これらの種類のバグは、分離、ページがユーザーに透過的に更新され、原因、そのためすぐにわかりにくいためににくい場合があります。

ASP.NET AJAX のフォームの内部動作を調べることで、傍受モデルを投稿、ASP.NET によって既に提供されているフレームワークを利用することを確認することができました。 そのためは、同じフレームワークを使用してデザインされたコントロールと最大の互換性を維持し、ページ用に記述された任意の追加の JavaScript で最低限が関与します。

## <a name="bio"></a>自己紹介

Rob Paveza Terralever でシニア .NET アプリケーションの開発者は、([www.terralever.com](http://www.terralever.com))、Tempe、さんのマーケティング企業の主要な対話型 彼に到達できる[ robpaveza@gmail.com ](mailto:robpaveza@gmail.com)、彼のブログはあると[ http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/)します。

1997 年からマイクロソフトの Web テクノロジで働いてあり myKB.com プレジデント、Scott Cate ([www.myKB.com](http://www.myKB.com)) ベースのナレッジ ベースのソフトウェア ソリューションに重点を置いてアプリケーションを ASP.NET の記述を専門としています。 Scott は時に電子メールが接続可能[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)またはで彼のブログ[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-partial-page-updates-with-asp-net-ajax.md)
> [次へ](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
