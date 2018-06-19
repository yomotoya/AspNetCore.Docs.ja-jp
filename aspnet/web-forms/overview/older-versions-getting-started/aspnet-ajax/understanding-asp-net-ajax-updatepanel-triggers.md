---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: ASP.NET AJAX UpdatePanel トリガーについて |Microsoft ドキュメント
author: scottcate
description: Visual Studio でのマークアップ エディターで作業しているときにすることがありますに注意してください (IntelliSense) から UpdatePanel コントロールの 2 つの子要素があります。 Wh のいずれかを指定しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: f30f2ead402d2f49a89b2caf47cc30b6445d4cfb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890750"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>ASP.NET AJAX UpdatePanel トリガーをについてください。
====================
によって[Scott カテゴリ](https://github.com/scottcate)

[PDF をダウンロードします。](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Visual Studio でのマークアップ エディターで作業しているときにすることがありますに注意してください (IntelliSense) から UpdatePanel コントロールの 2 つの子要素があります。 ページ (または 1 つを使用している場合、ユーザー コントロール) 上のコントロールを指定するトリガー要素の 1 つは、要素が含まれている UpdatePanel コントロールの部分的なレンダリングはトリガーとなります。


## <a name="introduction"></a>はじめに

ASP.NET の Microsoft のテクノロジは、オブジェクト指向し、イベント ドリブン プログラミング モデルによりに導入され、コンパイルされたコードのメリットと組み合わせたです。 ただし、そのサーバー側の処理モデルでは、Microsoft ASP.NET 3.5 AJAX Extensions に含まれる新機能によってうちの多くに対処することができます、テクノロジ固有のいくつかの欠点があります。 これらの拡張機能は、ページ全体の更新、クライアント スクリプト (プロファイル API、ASP.NET を含む)、および広範なクライアント側 API を介して Web サービスにアクセスする機能を必要とせずにページの部分のレンダリングを含む、多くの新しいのリッチ クライアント機能を有効にします。ASP.NET サーバー側コントロール セットに表示するコントロール パターンの多くのミラーに設計されています。

このホワイト ペーパーでは、ASP.NET AJAX の XML トリガー機能を調べて`UpdatePanel`コンポーネントです。 XML のトリガーは、特定の UpdatePanel コントロールの部分的なレンダリングを引き起こす可能性のあるコンポーネントをきめ細かく制御を付けます。

このホワイト ペーパーでは、.NET Framework 3.5 のベータ 2 リリースと Visual Studio 2008 に基づいています。 .NET Framework の基本クラス ライブラリには、ASP.NET AJAX Extensions に ASP.NET 2.0 を対象としたアドオン アセンブリ以前は統合されました。 Visual Studio 2008 では、いない Visual Web Developer Express を操作して、(コードの一覧が完全な互換性に関係なく、Visual Studio のユーザー インターフェイスに従ってチュートリアルを提供することにもこのホワイト ペーパーで想定しています開発環境の場合)。

## <a name="triggers"></a>*トリガー*

既定では、特定の UpdatePanel のトリガーは、呼び出し (たとえば) を持つテキスト ボックス コントロールを含む、ポストバックが子コントロールを自動的に含めるその`AutoPostBack`プロパティに設定**true**です。 ただし、トリガー含めることもできますマークアップ; を使用して宣言型これは、内で、 `<triggers>` UpdatePanel コントロール宣言のセクションでします。 トリガーを使用してアクセスできますが、`Triggers`コレクション プロパティをお勧め (たとえば、デザイン時に、コントロールを使用できません) 場合は、実行時に、部分的なレンダリング トリガーを登録することを使用して、`RegisterAsyncPostBackControl(Control)`のメソッド、内部オブジェクト、ページの ScriptManager、`Page_Load`イベント。 ページはステートレスで、これは再登録するこれらのコントロールが作成されるたびにことに注意してください。

自動子トリガー含めることができますも無効になります (ようにするためのポストバックを作成する子コントロールは、部分的なレンダリングを自動的にトリガーしない) を設定して、`ChildrenAsTriggers`プロパティを**false**です。 これによりどの特定のコントロールを割り当てるときに最大限の柔軟性は、ページ レンダリングを呼び出すことがあり、お勧め、開発者はオプトイン生じる可能性のあるすべてのイベントを処理するのではなく、イベントに応答できるようにできます。

いる UpdatePanel コントロールが入れ子になっているときに、UpdateMode 設定されている場合注意してください**条件付き**UpdatePanel の子がトリガーされるが、UpdatePanel の更新を子のみが、親ではない場合、します。 ただし、親の UpdatePanel が更新された場合は、し、UpdatePanel の子も更新されます。

## <a name="the-lttriggersgt-element"></a>*&lt;トリガー&gt;要素*

Visual Studio でのマークアップ エディターで作業しているときにすることがありますに注意してください (IntelliSense) から 2 つの子要素があるに`UpdatePanel`コントロール。 最も多く見要素は、`<ContentTemplate>`要素は、本質的に更新パネルによって保持されるコンテンツをカプセル化 (部分的なレンダリングを有効化コンテンツ)。 その他の要素は、`<Triggers>`ページ (または 1 つを使用している場合、ユーザー コントロール) 上のコントロールを指定する要素で UpdatePanel コントロールの部分的なレンダリングはトリガーとなる、&lt;トリガー&gt;要素が存在します。

`<Triggers>`要素は、任意の数ごとの 2 つの子ノードを含めることができます:`<asp:AsyncPostBackTrigger>`と`<asp:PostBackTrigger>`です。 2 つの属性が受け入れる`ControlID`と`EventName`、カプセル化の現在の単位内の任意のコントロールを指定できます (たとえば、UpdatePanel コントロールがある場合、Web ユーザー コントロール内で、しないでくださいでコントロールを参照するにはページ、ユーザー コントロールが存在する)。

`<asp:AsyncPostBackTrigger>`要素は、の子として存在するコントロールからイベントをターゲットにできるという点で便利*任意*このトリガーが子となる UpdatePanel だけでなく、カプセル化の単位で UpdatePanel コントロール. したがって、部分ページ更新を開始する任意のコントロールを作成できます。

同様に、`<asp:PostBackTrigger>`要素は、部分ページ レンダリング、トリガーが、サーバーへのフル ラウンド トリップを必要とする 1 つを使用することができます。 このトリガー要素は、コントロールが部分ページ レンダリングを通常トリガーそれ以外の場合と、ページ全体のレンダリングを強制にも使用できます (たとえば、ときに、`Button`にコントロールが存在する、 `<ContentTemplate>` UpdatePanel コントロールの要素)。 もう一度、PostBackTrigger 要素は、カプセル化の現在の単位で UpdatePanel コントロールの子である任意のコントロールを指定できます。

## <a name="lttriggersgt-element-reference"></a>*&lt;トリガー&gt;要素リファレンス*

*マークアップ子孫:*

| **タグ** | **説明** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | コントロールとこのトリガーの参照を含む UpdatePanel、部分ページ更新が発生するイベントを指定します。 |
| &lt;asp:PostBackTrigger&gt; | コントロールと完全のページ更新 (ページ全体の更新) が発生するイベントを指定します。 このタグは、コントロールでは、部分的なレンダリングは、それ以外の場合と、フル更新を強制するために使用します。 |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*チュートリアル: クロス UpdatePanel トリガー*

1. 部分的なレンダリングを有効にする設定は、ScriptManager をオブジェクトに新しい ASP.NET ページを作成します。 このページ - 最初に 2 つの Updatepanel を追加、ラベル コントロール (Label1) および 2 つのボタン コントロール (Button1 と Button2) が含まれます。 Button1 必要がありますクリック両方を更新して Button2 必要があります言ってと言います、またはそれらの行に沿ったものを更新する をクリックします。 2 番目の UpdatePanel でラベル コントロール (Label2) のみを含めるが、区別するために既定値以外に、前景色プロパティを設定します。
2. 両方 UpdatePanel タグの UpdateMode プロパティを設定する**条件付き**します。

**Default.aspx の 1 の一覧を表示するには: マークアップ:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Button1 の Click イベント ハンドラー、Label1.Text と Label2.Text に設定 (DateTime.Now.ToLongTimeString()) など時間に依存するもの。 Button2 の Click イベント ハンドラー、Label1.Text のみ時間に依存する値に設定します。

**2 を一覧表示します。 分離コードで default.aspx.cs (トリム):** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. F5 キーを押して、プロジェクトをビルドおよび実行します。 更新の両方のパネルをクリックすると、ときに両方のラベル変更は、テキストに注意してください。ただし、このパネルの更新プログラムをクリックすると、Label1 のみが更新されます。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*内部的な処理*

作成したばかりの例を使用して、ASP.NET AJAX を実行して内容と、UpdatePanel クロス パネル トリガーのしくみを見て取得ことができます。 HTML、生成されたページのソースと同様、FireBug - と呼ばれる、Mozilla Firefox 拡張機能させるため AJAX ポストバックを簡単に確認できます。 Lutz Roeder によって .NET Reflector ツールを使ってもします。 これらのツールの両方の自由に利用可能なオンラインでは、インターネットの検索で検出できます。

ページのソース コードの検査を示していますほとんどないです。 からUpdatePanel コントロールとしてレンダリングされます`<div>`コンテナー、およびおスクリプト リソースには、指定が含まれているを確認できますが、`<asp:ScriptManager>`です。 いくつかの新しい AJAX 固有呼び出し PageRequestManager を AJAX クライアント スクリプト ライブラリの内部にもあります。 UpdatePanel は、レンダリングされる 1 つの 2 つのコンテナーを参照して最後に、 `<input>` 、2 つのボタン`<asp:Label>`としてレンダリングされますコントロール`<span>`コンテナーです。 (FireBug の DOM ツリーを検査するわかります表示されているコンテンツを生成しないが、ことを示すために、ラベルは淡色表示されていること)。

このパネルの更新ボタンをクリックし、現在のサーバー時刻で最上位の UpdatePanel が更新されることを確認します。 FireBug では、要求を確認することができるように、[コンソール] タブを選択します。 パラメーターを調べて、POST 要求先。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


UpdatePanel が示されるが、サーバー側の AJAX コードを ScriptManager1 パラメーターを使用しているコントロールのツリーが発生した正確に注意してください:`Button1`の`UpdatePanel1`コントロール。 ここで、更新の両方のパネルのボタンをクリックします。 パイプで区切られた一連の文字列に設定されている変数がわかります応答を検証するには、次に、具体的には、最上位の UpdatePanel がわかります`UpdatePanel1`が、その全体、HTML ブラウザーに送信します。 AJAX クライアント スクリプト ライブラリで UpdatePanel の元の HTML コンテンツを使用して新しいコンテンツに置き換え、`.innerHTML`プロパティ、およびそのサーバーが HTML としてサーバーから、変更されたコンテンツを送信します。

ここで、更新両方パネル ボタンをクリックし、サーバーから結果を確認します。 結果は非常に似ています - 両方の Updatepanel がサーバーから新しい HTML を受信します。 同様に、以前のコールバックでは、その他のページの状態が送信されます。

ご覧のとおり、AJAX ポストバックを実行する特別なコードが使用されていないため、AJAX クライアント スクリプト ライブラリがコードを追加せず、フォームのポストバックをインターセプトします。 サーバー コントロールは JavaScript を自動的に利用できるように、フォームを自動的に送信しない - ASP.NET は、自動スクリプト リソースを含めること、PostBackOptions クラスによって、主に実現フォーム検証および状態は既に、コードを自動的に挿入、および ClientScriptManager クラスです。

たとえば、CheckBox コントロールです.NET Reflector でクラスの逆アセンブリを確認します。 これを行うには、System.Web アセンブリが、開いていることを確認しに移動、`System.Web.UI.WebControls.CheckBox`クラス、開く、`RenderInputTag`メソッドです。 チェックする条件を探して、`AutoPostBack`プロパティ。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


自動のポストバックが有効にすると、 `CheckBox` (AutoPostBack プロパティが true) を使用して、結果を制御`<input>`タグが処理しているスクリプトで ASP.NET イベントでレンダリングされてしたがってその`onclick`属性。 次に、フォームの送信の途中受信では、重大な変更が不正確な可能性のある文字列の置換を使用することによって発生する可能性のあるを回避することができ nonintrusively、ページに挿入する ASP.NET AJAX を使用できます。 これにより、さらに、*任意*UpdatePanel コンテナー内での使用をサポートするためにコードを追加せず、ASP.NET AJAX の電源を使用するカスタム コントロールを ASP.NET です。

`<triggers>` PageRequestManager の呼び出しで初期化される値に対応する機能\_updateControls (メモをメソッド、イベント、およびで始まるフィールド名、ASP.NET AJAX クライアント スクリプト ライブラリは、規則を利用して。アンダー スコアで内部としてマーク、およびライブラリ自体の外部で使用するためのものではありません)。 どのコントロールが AJAX ポストバックが発生するものではおを確認できます。

たとえば、みましょう 2 つ追加コントロールを追加 ページで、Updatepanel の外部で 1 つのコントロールを完全に終了して、UpdatePanel 内で 1 つの終了します。 上部の UpdatePanel 内にあるチェック ボックス コントロールを追加し、一覧内で定義されている色の数が、DropDownList をドロップおはします。 新しいマークアップを次に示します。

**新しいマークアップの 3 を一覧表示する:**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

また、新しい分離コードを次に示します。

**リスト 4: 分離コード**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

このページの考え方は、ドロップダウン リストには 2 番目のラベルを表示する次の 3 つの色のいずれかが選択され、両方が太字かどうかと、ラベルに日付と時刻を表示するかどうかのチェック ボックスを決定することです。 チェック ボックスには、AJAX の更新する必要がありますされませんが、ドロップダウン リストは、UpdatePanel 内でない格納されている場合でも。


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


上記のスクリーン ショットの見かけ上は、最新のボタンがクリックされるでした下部時間に依存しない、最上位の時間を更新更新このパネルの右のボタン。 日付もオフに切り替えるクリックの間で日付が下部にあるラベルに表示されます。 最後に目的の下部にあるラベルの色である: ラベルのテキストは、コントロールの状態が重要であることを示していますより最近更新されたされ、ユーザーは、AJAX ポストバックを通じて保持されます。 *ただし*時刻は更新されませんでした。 時間がの永続性を自動的に再生成、 \_\_コントロールは、サーバーに再表示されていたときに、ASP.NET ランタイムによって解釈されるページの VIEWSTATE フィールドです。 ASP.NET AJAX サーバー コードは状態を変更する方法、コントロールは認識されません。状態の表示から単純に再作成し、適切となるイベントを実行します。

これは、必要がありますで指摘、ただし、いたは初期化されているページ内で時間\_Load イベント時間とがインクリメント正しくです。 その結果、開発者を適切なイベント ハンドラーの中に適切なコードが実行されていることに注意してページの使用を避ける\_コントロールのイベント ハンドラーが適切ながある場合にロードします。

## <a name="summary"></a>まとめ

ASP.NET AJAX 拡張機能の UpdatePanel コントロールは汎用性の数を更新することが原因となる必要がありますコントロール イベントを識別するためのメソッドを利用できます。 子コントロールをによって自動的に更新されるをサポートしていますが、ページ上のコントロールのイベントにも応答できます。

サーバーの処理負荷が発生する可能性を減らすためには、ことを推奨、 `ChildrenAsTriggers` UpdatePanel のプロパティに設定する`false`、選択にはなく、既定で含まれているイベントがあるとします。 これにより、検証、および入力フィールドへの変更を含む可能性のある望ましくない影響の原因からも、不要なイベントが回避されます。 これらの種類のバグを分離する場合、ページは、ユーザーに透過的に更新し、原因したがってわかりにくいかもしれませんがすぐにためににくい場合があります。

確認するには、ASP.NET AJAX フォームの内部動作の途中受信モデルを投稿、ASP.NET によって既に提供されているフレームワークを利用することを確認することができました。 これにより、同じのフレームワークを使用して設計されているコントロールに関する最大の互換性を保持し、ページ用に記述された任意の追加の JavaScript の最小が関与します。

## <a name="bio"></a>略歴

Rob Paveza Terralever 上級の .NET アプリケーション開発者は、([www.terralever.com](http://www.terralever.com))、Tempe 弊社はアリゾナ州でマーケティング会社の主要な対話型 彼に到達できる[ robpaveza@gmail.com ](mailto:robpaveza@gmail.com)、彼のブログにあると[ http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/)です。

Scott カテゴリは、1997 年以降の Microsoft の Web テクノロジの使用されているがあり、myKB.com の代表者 ([www.myKB.com](http://www.myKB.com))、専門分野は、ASP.NET の書き込みの際にベースのアプリケーションのナレッジ ベースのソフトウェア ソリューションに重点を置きます。 Scott が接続時に電子メール[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)または彼のブログで[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-partial-page-updates-with-asp-net-ajax.md)
> [次へ](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
