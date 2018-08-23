---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: サーバー コントロール |Microsoft Docs
author: microsoft
description: ASP.NET 2.0 では、さまざまな方法でサーバー コントロールを強化します。 このモジュールでいくつかの方法が ASP.NET 2.0 と Visual Studio 200 にアーキテクチャの変更について説明します.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: ecf99fa894c1f662542aa8a613195b828bf2c67b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832205"
---
<a name="server-controls"></a>サーバー コントロール
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 では、さまざまな方法でサーバー コントロールを強化します。 このモジュールでいくつかの方法は、ASP.NET 2.0 には、アーキテクチャの変更について説明し、Visual Studio 2005 は、サーバー コントロールを処理します。


ASP.NET 2.0 では、さまざまな方法でサーバー コントロールを強化します。 このモジュールでいくつかの方法は、ASP.NET 2.0 には、アーキテクチャの変更について説明し、Visual Studio 2005 は、サーバー コントロールを処理します。

## <a name="view-state"></a>ビュー ステート

ASP.NET 2.0 でビューステートの主な変更は、サイズが大幅に短縮します。 カレンダー コントロールにあるページを検討してください。 ASP.NET 1.1 では、ビュー ステートを示します。

[!code-css[Main](server-controls/samples/sample1.css)]

今すぐ次に示しますビュー ステートのと同じページに ASP.NET 2.0 で。

[!code-css[Main](server-controls/samples/sample2.css)]

非常に重大な変更、ビュー ステートをネットワーク経由で送受信実行を検討して、この変更開発者大幅なパフォーマンスが向上です。 ビュー ステートのサイズを縮小では、主に内部的に処理した方法によるものです。 ビュー ステートは、Base64 にエンコードされた文字列に注意してください。 ASP.NET 2.0 でビューステートの変更をより深く理解するには、上記の例からデコードされた値を見てをみましょうがあります。

デコードされた 1.1 のビュー ステートを次に示します。

[!code-css[Main](server-controls/samples/sample3.css)]

これは少しでたらめに見えるが、ここで、パターンがあります。 Asp.net 1.x では、1 つの文字データ型を識別するために使用し、使用して値を区切り、、 &lt; &gt;文字。 上記のビュー ステートのサンプルの"t"は、3 つを表します。 うちには ("l"は、ArrayList を表します)。 Arraylist のペアが含まれていますこれらの Arraylist の int32 型が含まれています ("i") 1 および、その他の値を持つ別の組が含まれています。 うちには、Arraylist などのペアが含まれています。重要なことことのペアを含む Triplets 使用して、文字を使用してデータ型を特定および使用して、&lt;と&gt;区切り記号として文字。

ASP.NET 2.0 でデコードされたビュー ステートは少し異なります。

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

デコードされたビュー ステートの外観に大きな変更を確認する必要があります。 この変更は、いくつかのアーキテクチャの基盤です。 ビュー ステートで ASP.NET 1.x がデータをシリアル化に、LosFormatter を使用します。 2.0 では、新しい ObjectStateFormatter クラスを使用します。 このクラスは、ビュー ステートとコントロールの状態の逆シリアル化とシリアル化を支援するために設計されています。 (コントロールの状態は、次のセクションで説明されます)。シリアル化および逆シリアル化実行メソッドを変更することで得られる多くの利点があります。 最も大幅なパフォーマンスの 1 つは、使用することを TextWriter を使用する LosFormatter とは異なり、ObjectStateFormatter BinaryWriter です。 これにより、ビュー ステート、一連の文字列ではなくバイトを格納する ASP.NET 2.0 ができます。 たとえば、整数を取得します。 ASP.NET 1.1 では、整数値には、ビュー ステートの 4 バイトが必要です。 ASP.NET 2.0 でその同じ整数には、1 バイトのみが必要です。 他の機能強化が格納されているビュー ステートの量を減らして行われました。 たとえば、DateTime 値は文字列ではなく、TickCount を使用してを格納はようになりました。

すべてを言えば、まるで DataGrid と同様のコントロールを 1.x での表示状態の最大消費者のいずれかがファクトに特別な注意を払いました。 ビュー ステートは DataGrid などのコントロールの主な欠点は、大量の情報が繰り返される多くの場合、含まれることです。 Asp.net 1.x では、結果として再度肥大化の表示状態および経由で情報が格納だけが繰り返し表示します。 ASP.NET 2.0 では、このようなデータを格納するのに新しい IndexedString クラスを使用します。 文字列が繰り返される場合、IndexedString と IndexedString オブジェクトの実行中のテーブル内のインデックスのトークンだけ格納します。

## <a name="control-state"></a>コントロールの状態

HTTP ペイロードに追加されたサイズではビュー ステートで開発者がいた主要な gripes のいずれか。 既に説明したように、DataGrid コントロールのビューステートの最大消費者の 1 つです。 データ グリッドによって生成されたビュー ステートの膨大な量を避けるためには、多くの開発者には、そのコントロールのビューステート単に無効になります。 残念ながら、そのソリューションが常に良いものにします。 ビューステート 1.x には、ASP.NET には、コントロールの適切な機能に必要なデータだけでなくが含まれています。 コントロールの UI の状態に関する情報も含まれています。 つまり、すべてを表示する UI 情報を必要としない場合でも、ビュー ステートを有効にする必要があります、DataGrid に改ページ調整を許可する場合の状態が含まれています。 これは、オール_オア_ナッシングのシナリオです。

ASP.NET 2.0 では、コントロールの状態は、コントロールの状態の概要を適切に使用してその問題を解決します。 コントロールの状態には、コントロールの適切な機能を絶対に必要なデータが含まれています。 ビューの状態とは異なりは、コントロールの状態を無効にすることはできません。 したがって、コントロールの状態に格納されているデータを慎重に制御される重要なは。

> [!NOTE]
> ビュー ステートとコントロールの状態が永続化、 \_ \_VIEWSTATE 隠しフォーム フィールド。


このビデオでは、ビュー ステートとコントロールの状態のチュートリアルです。


![](server-controls/_static/image1.png)


[開いているビデオを全画面](server-controls/_static/state1.wmv)


サーバー コントロールで読み取りし、書き込み状態を制御するためには、3 つの手順を行う必要があります。

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>手順 1: RegisterRequiresControlState メソッドを呼び出す

RegisterRequiresControlState メソッドは、コントロールがコントロールの状態を保持する必要があることを ASP.NET に通知します。 型が登録されているコントロールにあるコントロールの 1 つの引数がかかります。

登録に、要求が保持されないことに注意してください。 重要です。 そのため、このメソッドはコントロールがコントロールの状態を保持する場合、要求ごとに呼び出す必要があります。 Oninit メソッドで、メソッドを呼び出すことをお勧めします。

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>手順 2: SaveControlState をオーバーライドします。

SaveControlState メソッドは、最新の投稿に戻るためのコントロールの状態の変更をコントロールに保存します。 コントロールの状態を表すオブジェクトを返します。

## <a name="step-3-override-loadcontrolstate"></a>手順 3: LoadControlState をオーバーライドします。

LoadControlState メソッドは、コントロールに保存された状態を読み込みます。 メソッドは、コントロールの保存された状態を保持するオブジェクトの種類の 1 つの引数を受け取ります。

## <a name="full-xhtml-compliance"></a>完全な XHTML 準拠

Web 開発者は、Web アプリケーションで標準の重要性を認識します。 標準ベースの開発環境を維持するためには、ASP.NET 2.0 と、XHTML 準拠では完全にします。 したがって、すべてのタグは、HTML 4.0 をサポートするブラウザーで XHTML 標準に従って表示されるか、大きいです。

ASP.NET 1.1 DOCTYPE 定義は次のとおりです。

[!code-html[Main](server-controls/samples/sample6.html)]

ASP.NET 2.0 で既定の DOCTYPE 定義のとおりです。

[!code-html[Main](server-controls/samples/sample7.html)]

選択した場合は、構成ファイルで xhtmlConformance ノードを介して既定 XHML コンプライアンスを変更できます。 たとえば、web.config ファイルで、次のノードは XHTML 1.0 Strict を XHTML 準拠を変更します。

[!code-xml[Main](server-controls/samples/sample8.xml)]

選択した場合、構成することも、従来の ASP.NET で使用される構成を使用する ASP.NET 1.x として次のとおりです。

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>アダプティブ アダプターを使用して表示します。

Asp.net 1.x では、構成ファイルに含まれている、 &lt;browserCaps&gt; HttpBrowserCapabilities オブジェクトを作成するセクション。 このオブジェクトは、開発者はどのようなデバイスは、特定の要求を行ってを特定し、コードを適切にレンダリングを許可します。 ASP.NET 2.0 では、モデルが改善され、今すぐ新しい ControlAdapter クラスを使用します。 ControlAdapter クラスは、コントロールのライフ サイクルのイベントをオーバーライドし、ユーザー エージェントの機能に基づいてコントロールのレンダリングを制御します。 特定のユーザー エージェントの機能は、c:\windows\microsoft.net\framework\v2.0 に格納されているブラウザー定義ファイル (.browser ファイルの拡張子を持つファイル) によって定義されます。\* \* \* \*\CONFIG\Browsers フォルダー。

> [!NOTE]
> ControlAdapter クラスは抽象クラスです。


ほぼ同じように、 &lt;browserCaps&gt; 1.x では、ブラウザー定義ファイルのセクションでは、正規の式を使用して、要求側のブラウザーを識別するために、ユーザー エージェント文字列を解析します。 これは、ユーザー エージェントに対して特定の機能を定義しています。 ControlAdapter Render メソッドを使用してコントロールをレンダリングします。 そのため、Render メソッドをオーバーライドする場合呼び出す必要はありませんレンダリング、基本クラス。 これは、アダプターで、コントロール自体の 1 回、2 回発生するレンダリングにあります。

## <a name="developing-a-custom-adapter"></a>カスタム アダプターの開発

ControlAdapter から継承することによって、独自のカスタム アダプターを開発できます。 さらに、アダプターは、ページを必要な場合は、抽象クラス PageAdapter から継承することができます。 コントロールのカスタム アダプターへのマッピングが使用して実現、 &lt;controlAdapters&gt;ブラウザー定義ファイル内の要素。 たとえば、ブラウザー定義ファイルから次の XML は、メニュー コントロールを MenuAdapter クラスにマッピングされます。

[!code-html[Main](server-controls/samples/sample10.html)]

このモデルを使用して、コントロールの開発者が特定のデバイスまたはブラウザーを対象とする非常に簡単です。 すべてのデバイスでのページのレンダリングを完全に制御する開発者にとって非常に簡単です。

## <a name="per-device-rendering"></a>デバイスごとのレンダリング

ASP.NET 2.0 でのサーバー コントロールのプロパティは、指定したデバイスごとのブラウザー固有のプレフィックスを使用できます。 たとえば、次のコードには、によっては、ページを参照するどのデバイスが使用されるラベルのテキストが変わります。

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

このラベルを含むページが Internet Explorer から参照されると、ラベルが「を参照している Internet Explorer から」というテキストに表示されます。 Firefox からページが参照されると、ラベルのテキストを表示」を参照している Firefox から" その他の任意のデバイスから、ページが参照されると、「を参照する不明なデバイスから」が表示されます。 この特別な構文を使用して、任意のプロパティを指定できます。

## <a name="setting-focus"></a>フォーカスの設定

ASP.NET 1.x の開発者は、特定のコントロールに初期フォーカスを設定する方法についてよく寄せられます。 たとえば、[ログイン] ページで、ユーザー ID ボックスに、ページが初めて読み込まれるときに、フォーカスを取得すると便利ですが。 Asp.net 1.x では、これを行ういくつかのクライアント側スクリプトの記述が必要です。 このようなスクリプトには、簡単なタスクが、SetFocus メソッドに協力してくれた ASP.NET 2.0 では必要はありません。 SetFocus メソッドは、コントロール フォーカスを受け取る必要があることを示す 1 つの引数を受け取ります。 この引数を文字列として、コントロールのクライアント ID またはコントロール オブジェクトとして、サーバー コントロールの名前指定できます。 たとえば、txtUserID というページが初めて読み込まれるときにテキスト ボックス コントロールを初期フォーカスを設定するページに次のコードを追加\_負荷。

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--または

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 では、フォーカスを設定するクライアント側の関数を表示するために (前に説明した) Webresource.axd ハンドラーを使用します。 クライアント側の関数の名前は、web フォーム\_AutoFocus 次のようにします。

[!code-html[Main](server-controls/samples/sample14.html)]

または、コントロールの Focus メソッドを使用して、そのコントロールに初期フォーカスを設定することができます。 Focus メソッドでは、コントロール クラスから派生し、すべての ASP.NET 2.0 コントロールを使用します。 検証エラーが発生したときに、特定のコントロールにフォーカスを設定することもできます。 については、以降の章で説明します。

## <a name="new-server-controls-in-aspnet-20"></a>ASP.NET 2.0 の新しいサーバー コントロール

ASP.NET 2.0 で新しいサーバー コントロールを次に示します。 以降の章のいくつか詳細に移動されます。

## <a name="imagemap-control"></a>ImageMap コントロール

ImageMap コントロールをポストバックを開始または URL に移動できるイメージにホット スポットを追加することができます。 使用可能なホット スポットの 3 つの種類があります。[Circlehotspot]、RectangleHotSpot、および PolygonHotSpot です。 ホット スポットは、Visual Studio またはプログラムによってコードでは、コレクション エディターを使用して追加されます。 イメージにホット スポットを描画するために使用可能なユーザー インターフェイスはありません。 座標とサイズやホット スポットの半径は、宣言によって指定する必要があります。 また、デザイナーでのホット スポットの視覚的表現はありません。 ホット スポットは、URL に移動するように構成、URL は、ホット スポットの NavigateUrl プロパティを使用して指定されます。 投稿の場合は、プロパティを使用すると、サーバー側コードで取得できる、ポストバックで文字列を渡す PostBackValue のホット スポットをバックアップします。


![Visual Studio で hotSpot コレクション エディター](server-controls/_static/image1.jpg)

**図 1**: Visual Studio で HotSpot コレクション エディター


## <a name="bulletedlist-control"></a>BulletedList コントロール

BulletedList コントロールは、バインドされたデータを簡単にできる箇条書きリストです。 一覧に (番号) 順序を指定できます BulletStyle プロパティを使用して順序付けられていないか。 リスト内の各項目は ListItem オブジェクトによって表されます。


![Visual Studio で BulletedList コントロール](server-controls/_static/image1.gif)

**図 2**: Visual Studio で BulletedList コントロール


## <a name="hiddenfield-control"></a>HiddenField コントロール

HiddenField コントロールは、この値はサーバー側コードで使用できる、ページに隠しフォーム フィールドを追加します。 隠しフォーム フィールドの値はポストバック間で変更されません一般的に必要です。 ただし、悪意のあるユーザーがポスト バックする前の値を変更することができます。 この場合、HiddenField コントロールが、ValueChanged イベントを発生します。 HiddenField コントロールに機密情報があることが変更されないことを保証する場合は、コードで、ValueChanged イベントを処理する必要があります。

## <a name="fileupload-control"></a>FileUpload コントロール

ASP.NET 2.0 で FileUpload コントロールを使うと、ASP.NET ページ経由で Web サーバーにファイルをアップロードできます。 このコントロールは、いくつかの例外は、ASP.NET 1.x HtmlInputFile クラスに非常に似ています。 Asp.net 1.x では、適切なファイルがあったかどうかを決定するために null PostedFile プロパティをチェック推奨されました。 ASP.NET 2.0 で FileUpload コントロールでは、同じ目的で使用することができますを方が少し効率的に、新しい HasFile プロパティを追加します。

PostedFile プロパティは、HttpPostedFile オブジェクトへのアクセスを引き続き使用できますが、HttpPostedFile の機能の一部は本質的に FileUpload コントロールで使用できるようになりました。 たとえば、ASP.NET では、アップロードされたファイルを保存する 1.x では、HttpPostedFile オブジェクトの SaveAs メソッドを呼び出すことです。 FileUpload コントロールを使用して ASP.NET 2.0 では、FileUpload コントロール自体に、SaveAs メソッドを呼び出すとします。

2.0 の動作 (およびおそらく最も重要な変更) で、もう 1 つの重要な変更は、ことは、保存する前にアップロードされたファイル全体をメモリに読み込む必要はなくなりました。 1.x では、書き込まれる前にメモリに完全にアップロードされた任意のファイルを保存するディスク。 このアーキテクチャでは、大きなファイルのアップロードできないようにします。

ASP.NET 2.0 での httpRuntime 要素と属性を使用するキロバイト数を構成するのには書き込まれる前にメモリ内のバッファーで保持されてディスクにします。

**重要な**: この値がバイト (キロバイト単位ではない) である、既定値は 256 を MSDN ドキュメント (および別の場所のドキュメント) を指定します。 キロバイト単位で指定された値が実際には、既定値は 80 です。 80 K の既定値を持つことによって、バッファーが終わらない大きなオブジェクト ヒープのことです。

## <a name="wizard-control"></a>ウィザード コントロール

ASP.NET 開発者向けの「ページ」パネルを使用するのか、ページ間を転送することによって、一連の情報を収集しようとしてに悩まされることが発生する非常に一般的です。 たいてい、作業は面倒であるし、は時間がかかります。 新しいウィザード コントロールは、線形と非線形の手順にユーザーが慣れているウィザード インターフェイスに許可することにより、問題を解決します。 ウィザード コントロールは、一連の手順で入力フォームを表示します。 各ステップでは、コントロールのいずれのプロパティで指定された特定の型です。 使用可能なステップの種類は次のとおりです。

| **ステップの種類** | **説明** |
| --- | --- |
| 自動 | ウィザードでは、自動的にステップ ステップ階層内の位置に基づいての種類を決定します。 |
| [開始] | 最初に、多くの場合、最初のステートメントを提示するために使用します。 |
| 手順 | 通常の手順です。 |
| 完了 | 最後の手順は、通常、ウィザードを完了するためのボタンを表示するために使用します。 |
| 完了 | 成功または失敗を通信してメッセージを表示します。 |

> [!NOTE]
> ウィザード コントロールは、ASP.NET コントロールの状態を使用して、状態の追跡を保持します。 そのため、EnableViewState プロパティは、任意に損害を与えることがなく、false に設定できます。


このビデオでは、ウィザード コントロールのチュートリアルです。


![](server-controls/_static/image2.png)


[開いているビデオを全画面](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>コントロールをローカライズします。

Localize コントロールは、リテラル コントロールに似ています。 ただし、Localize コントロールには、**モード**に追加されるマークアップのレンダリング方法を制御するプロパティ。 Mode プロパティには、次の値がサポートされています。

| **モード** | **説明** |
| --- | --- |
| 変換 | マークアップは、ブラウザーの要求のプロトコルに従って変換されます。 |
| PassThrough | マークアップとしてレンダリングされます-です。 |
| エンコード | コントロールに追加されるマークアップは、HtmlEncode を使用してエンコードされます。 |

## <a name="multiview-and-view-controls"></a>MultiView コントロールとビュー コントロール

マルチビュー コントロールのビューのコントロールのコンテナーとして機能し、ビュー コントロールは、(同様パネル コントロール) の他のコントロールのコンテナーとして機能します。 マルチビュー コントロール内の各ビューは、1 つのビュー コントロールで表されます。 最初のビュー、MultiView コントロールがビュー 0、2 番目は 1、ビューなど。マルチビュー コントロールの ActiveViewIndex を指定することで、ビューを切り替えることができます。

## <a name="substitution-control"></a>代替コントロール

切り替えコントロールは、ASP.NET のキャッシュと組み合わせて使用されます。 キャッシュを活用するために必要なが各要求 (つまり、ページの一部のキャッシュから除外される) で更新する必要があります ページの部分がある場合では、代入コンポーネントは、優れたソリューションを提供します。 コントロールでは、独自の出力を実際にレンダリングしません。 代わりに、サーバー側コードのメソッドにバインドされます。 ページが要求されたときに、メソッドが呼び出され、返されたマークアップは、代替コントロールの代わりに表示されます。

代替コントロールがバインドされているメソッドを指定した、 **MethodName**プロパティ。 メソッドは、次の条件を満たす必要があります。

- 静的 (VB では shared) メソッドがあります。
- HttpContext の種類の 1 つのパラメーターを受け取っています。
- ページ上のコントロールを置き換える必要がありますマークアップを表す文字列を返します。

代替コントロールには、ページで、他のコントロールを変更することはありませんが、そのパラメーターを使用して、現在の HttpContext へのアクセスが。

## <a name="gridview-control"></a>GridView コントロール

GridView コントロールは、DataGrid コントロールを置換します。 このコントロールは、後のモジュールで詳しく説明されます。

## <a name="detailsview-control"></a>DetailsView コントロール

DetailsView コントロールでは、データ ソースから 1 つのレコードを表示および編集または削除することができます。 後のモジュールで詳しく説明しました。

## <a name="formview-control"></a>FormView コントロール

FormView コントロールは、構成可能なインターフェイスでのデータ ソースから 1 つのレコードの表示に使用されます。 後のモジュールで詳しく説明しました。

## <a name="accessdatasource-control"></a>AccessDataSource コントロール

AccessDataSource コントロールを使用する Access データベースにデータをバインドします。 後のモジュールで詳しく説明しました。

## <a name="objectdatasource-control"></a>ObjectDataSource コントロール

ObjectDataSource コントロールは、コントロールのデータ バインドできます 2 層モデルではなく中間層ビジネス オブジェクトにコントロールがデータ ソースへの直接バインドされてように 3 層アーキテクチャをサポートするために使用されます。 については、後のモジュールで詳しく説明します。

## <a name="xmldatasource-control"></a>XmlDataSource コントロール

XmlDataSource コントロールは、XML データ ソースへのデータ バインドに使用されます。 後のモジュールで詳しく説明しました。

## <a name="sitemapdatasource-control"></a>SiteMapDataSource コントロール

SiteMapDataSource コントロールは、サイト マップに基づいてサイト ナビゲーション コントロールのデータ バインディングを提供します。 については、後のモジュールで詳しく説明します。

## <a name="sitemappath-control"></a>SiteMapPath コントロール

SiteMapPath コントロールでは、一連の階層リンクとも呼ばナビゲーション リンクが表示されます。 後のモジュールで詳しく説明しました。

## <a name="menu-control"></a>メニュー コントロール

メニュー コントロールは、DHTML を使用して動的メニューを表示します。 後のモジュールで詳しく説明しました。

## <a name="treeview-control"></a>TreeView コントロール

[TreeView] コントロールは、データの階層ツリー ビューの表示に使用されます。 後のモジュールで詳しく説明しました。

## <a name="login-control"></a>Login コントロール

Login コントロールは、Web サイトにログインするためのメカニズムを提供します。 後のモジュールで詳しく説明しました。

## <a name="loginview-control"></a>LoginView コントロール

LoginView コントロールでは、ユーザーのログイン ステータスに基づいてさまざまなテンプレートの表示ができます。 後のモジュールで詳しく説明しました。

## <a name="passwordrecovery-control"></a>PasswordRecovery コントロール

PasswordRecovery コントロールは、ASP.NET アプリケーションのユーザーがパスワードを忘れた場合の取得に使用されます。 後のモジュールで詳しく説明しました。

## <a name="loginstatus"></a>LoginStatus

LoginStatus コントロールは、ユーザーのログイン状態を表示します。 後のモジュールで詳しく説明しました。

## <a name="loginname"></a>LoginName

LoginName コントロールは、ASP.NET アプリケーションにログインしている後に、ユーザーのユーザー名を表示します。 後のモジュールで詳しく説明しました。

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard は、構成可能なウィザードによってユーザーは、ASP.NET アプリケーションで使用するための ASP.NET メンバーシップ アカウントを作成する機能です。 後のモジュールで詳しく説明しました。

## <a name="changepassword"></a>パスワードの変更

ChangePassword コントロールでは、ASP.NET アプリケーションのパスワードを変更することができます。 後のモジュールで詳しく説明しました。

## <a name="various-webparts"></a>さまざまな web パーツ

ASP.NET 2.0 は、さまざまな Web パーツに付属しています。 これらについては、後のモジュールで詳しく説明します。
