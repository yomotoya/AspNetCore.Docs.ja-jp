---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Toolkit コントロール エクステンダー (c#) を制御するカスタムの AJAX の作成 |Microsoft Docs
author: microsoft
description: カスタム エクステンダーを使用すると、カスタマイズし、新しいクラスを作成することがなく ASP.NET コントロールの機能を拡張できます。
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: b9a3b9a8d5c86cc7aac6aeb8b4bac48af2e2edc7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835264"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>カスタム AJAX Control Toolkit コントロール エクステンダー (c#) を作成します。
====================
によって[Microsoft](https://github.com/microsoft)

> カスタム エクステンダーを使用すると、カスタマイズし、新しいクラスを作成することがなく ASP.NET コントロールの機能を拡張できます。


このチュートリアルでは、カスタム AJAX Control Toolkit コントロール エクステンダーを作成する方法について説明します。 便利で新しいエクステンダーをテキスト ボックスにテキストを入力するときに有効と無効のボタンの状態を変更するが、単純なを作成します。 このチュートリアルを読むと、次の独自のコントロール エクステンダーを使用して ASP.NET AJAX ツールキットを拡張することができます。

Visual Studio または Visual Web Developer を使用してカスタム コントロール エクステンダーを作成することができます (Visual Web Developer の最新バージョンがあることを確認してください)。

## <a name="overview-of-the-disabledbutton-extender"></a>DisabledButton エクステンダーの概要

DisabledButton extender の名前は、新しいコントロール エクステンダー。 このエクステンダーでは、次の 3 つのプロパティがあります。

- TargetControlID - テキスト ボックス コントロールを拡張します。
- TargetButtonIID - が無効または有効にするボタン。
- DisabledText - 最初に、ボタンに表示されるテキスト。 入力を開始するときに、ボタンには、ボタンのテキスト プロパティの値が表示されます。

テキスト ボックスとボタン コントロールに DisabledButton エクステンダーをフックするとします。 任意のテキストを入力する前に、ボタンが無効にし、ボタンをクリックし、テキスト ボックスに次のようになります。


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))。


テキストの入力を開始した後、ボタンが有効になっているし、ボタンをクリックし、テキスト ボックスに次のようになります。


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))。


このコントロール エクステンダーを作成するには、次の 3 つのファイルを作成する必要があります。

- DisabledButtonExtender.cs - このファイルは、extender の作成を管理およびデザイン時にプロパティを設定することはサーバー側コントロールのクラスです。 Extender に設定できるプロパティも定義します。 これらのプロパティは、コードを使用して、およびデザイン時にアクセスし、DisableButtonBehavior.js ファイルで定義されているプロパティと一致します。
- DisabledButtonBehavior.js--このファイルは、すべてのクライアント スクリプトのロジックを追加します。
- DisabledButtonDesigner.cs - このクラスは、デザイン時の機能を使用できます。 Visual Studio または Visual Web Developer デザイナーで正しく機能するコントロール エクステンダーをする場合は、このクラスを作成する必要があります。

したがってコントロール エクステンダーは、サーバー側コントロール、クライアント側の動作、およびサーバー側デザイナー クラスで構成されます。 次のセクションでこれらのファイルの 3 つすべてを作成する方法について説明します。

## <a name="creating-the-custom-extender-website-and-project"></a>カスタム エクステンダーの web サイトおよびプロジェクトの作成

最初の手順では、Visual Studio または Visual Web Developer でクラス ライブラリ プロジェクトと web サイトを作成します。 私たち ll クラス ライブラリ プロジェクトでカスタム エクステンダーを作成し、web サイトでカスタム エクステンダーをテストします。

S の web サイトが開始できるようにします。 Web サイトを作成する次の手順に従います。

1. メニュー オプションを選択**ファイル]、[新しい Web サイト**します。
2. 選択、 **ASP.NET Web サイト**テンプレート。
3. 新しい web サイトの名前を付けます*Website1*します。
4. をクリックして、 **OK**ボタンをクリックします。

次に、コントロール エクステンダーのコードを含むクラス ライブラリ プロジェクトを作成する必要があります。

1. メニュー オプションを選択**ファイルを追加、新しいプロジェクト**します。
2. 選択、**クラス ライブラリ**テンプレート。
3. 名前の新しいクラス ライブラリの名前を付けます**CustomExtenders**します。
4. をクリックして、 **OK**ボタンをクリックします。

次の手順を完了すると、図 1 よう、ソリューション エクスプ ローラー ウィンドウになります。


[![Web サイトとクラス ライブラリ プロジェクトとソリューション](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**図 01**: web サイトとクラス ライブラリ プロジェクトとソリューション ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))。


次に、すべてのクラス ライブラリ プロジェクトに必要なアセンブリ参照を追加する必要があります。

1. CustomExtenders プロジェクトを右クリックし、メニュー オプションを選択**参照の追加**します。
2. [.NET] タブを選択します。
3. 次のアセンブリへの参照を追加します。

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. [参照] タブを選択します。
5. AjaxControlToolkit.dll アセンブリへの参照を追加します。 このアセンブリは、AJAX Control Toolkit をダウンロードしたフォルダーにあります。

次の手順を完了すると、図 2 よう、クラス ライブラリ プロジェクト参照 フォルダーになります。


[![必要な参照と参照 フォルダー](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**図 02**: 必要な参照と参照 フォルダー ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))。


## <a name="creating-the-custom-control-extender"></a>カスタム コントロール エクステンダーを作成します。

クラス ライブラリをしたら、エクステンダー コントロールの作成を始めることができます。 S カスタム エクステンダー コントロール クラス (1 のリストを参照) のベア ボーンを始めることができます。

**1 - MyCustomExtender.cs を一覧表示します。**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

リスト 1 でコントロール エクステンダー クラスに関する注意するいくつかの点があります。 最初に、ExtenderControlBase の基本クラスからクラスを継承することを確認します。 すべての AJAX Control Toolkit エクステンダー コントロールは、この基本クラスから派生します。 たとえば、基底クラスには、すべてコントロール エクステンダーの必要なプロパティである TargetID プロパティが含まれています。

次に、クラスがクライアント スクリプトに関連する次の 2 つの属性が含まれることに注意してください。

- WebResource - には、アセンブリ内の埋め込みリソースとして含まれるファイルが原因です。
- ClientScriptResource - は、アセンブリから取得するスクリプト リソースをによりします。

WebResource 属性は、アセンブリに埋め込む MyControlBehavior.js JavaScript ファイル、カスタム エクステンダーはコンパイル時に使用されます。 ClientScriptResource 属性は web ページで、カスタム エクステンダーを使用する場合は、アセンブリから MyControlBehavior.js スクリプトを取得するために使用します。


WebResource および ClientScriptResource 属性操作するためには、埋め込みリソースとしての JavaScript ファイルをコンパイルする必要があります。 ソリューション エクスプ ローラー ウィンドウで、ファイルを選択、プロパティ シートを開き、値を割り当てる*埋め込まれたリソース*を**ビルド アクション**プロパティ。


コントロールのエクステンダーが TargetControlType 属性も含まれることに注意してください。 この属性を使用して、コントロール エクステンダーによって拡張されたコントロールの種類を指定します。 リストの 1 の場合は、コントロール エクステンダーがテキスト ボックスの拡張に使用されます。

最後に、カスタム エクステンダーが MyProperty という名前のプロパティが含まれることに注意してください。 プロパティは、ExtenderControlProperty 属性でマークされます。 GetPropertyValue() と SetPropertyValue() メソッドを使用して、クライアント側の動作に、サーバー側コントロール エクステンダーのプロパティの値を渡します。

S、DisabledButton エクステンダーのコードを実装して使用できます。 このエクステンダーのコードは、リスト 2 で確認できます。

**2 - DisabledButtonExtender.cs を一覧表示します。**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

リスト 2 で DisabledButton エクステンダーには、TargetButtonID DisabledText という 2 つのプロパティがあります。 TargetButtonID プロパティに適用される IDReferenceProperty からボタン コントロールの ID 以外のものをこのプロパティに割り当てることができないようにします。

WebResource および ClientScriptResource 属性は、このエクステンダー DisabledButtonBehavior.js という名前のファイルにあるクライアント側の動作を関連付けます。 次のセクションでは、この JavaScript ファイルをについて説明します。

## <a name="creating-the-custom-extender-behavior"></a>カスタム エクステンダーの動作を作成します。

コントロール エクステンダーのクライアント側のコンポーネントには、動作は呼び出されます。 DisabledButton 動作を無効にすると、ボタンを有効にすると、実際のロジックが含まれています。 動作の JavaScript コードは、リスト 3 に含まれます。

**3 - DisabledButton.js を一覧表示します。**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

リスト 3 の JavaScript ファイルには、DisabledButtonBehavior をという名前のクライアント側クラスが含まれています。 TargetButtonID という名前の 2 つのプロパティが、サーバー側ツインのように、このクラスに含まれていてを使用してアクセスできる DisabledText 取得\_TargetButtonID/設定\_TargetButtonID 取得と\_DisabledText/設定\_DisabledText します。

Initialize() メソッドは、keyup イベント ハンドラーを動作のターゲット要素に関連付けます。 この動作に関連付けられているテキスト ボックスに文字を入力するたびに、keyup ハンドラーを実行します。 Keyup ハンドラーが有効か、または動作に関連付けられているテキスト ボックスが任意のテキストを含むかどうかに応じて、ボタンを無効にします。

埋め込みリソースとしてリスト 3 の JavaScript ファイルをコンパイルする必要がありますに注意してください。 ソリューション エクスプ ローラー ウィンドウで、ファイルを選択、プロパティ シートを開き、値を割り当てる*埋め込まれたリソース*を**ビルド アクション**プロパティ (図 3 を参照してください)。 このオプションは、Visual Studio と Visual Web Developer の両方で使用できます。


[![埋め込みリソースとしての JavaScript ファイルを追加します。](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**図 03**: 埋め込みリソースとしての JavaScript ファイルを追加する ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))。


## <a name="creating-the-custom-extender-designer"></a>カスタム エクステンダー デザイナーの作成

Extender を完了するを作成する必要がある 1 つの最後のクラスがあります。 リスト 4 デザイナー クラスを作成する必要があります。 このクラスは、Visual Studio または Visual Web Developer デザイナーで正しく動作するエクステンダーに必要です。

**4 - DisabledButtonDesigner.cs を一覧表示します。**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

デザイナーの属性を持つ DisabledButton エクステンダー リスト 4 デザイナーに関連付けます。このような DisabledButtonExtender クラスに Designer 属性を適用する必要があります。

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>カスタム エクステンダーを使用

DisabledButton コントロール エクステンダーを作成することが完了したら、これで、ASP.NET web サイトで使用する時間を勧めします。 最初に、ツールボックスにカスタム エクステンダーを追加する必要があります。 この場合は、以下の手順に従ってください。

1. ASP.NET ページを開くには、ソリューション エクスプ ローラー ウィンドウでページをダブルクリックします。
2. ツールボックスを右クリックし、メニュー オプションを選択**アイテムの選択**します。
3. ツールボックス アイテムの選択 ダイアログ ボックスでは、CustomExtenders.dll アセンブリを参照します。
4. をクリックして、 **OK**ボタンをクリックしてダイアログを閉じます。

次の手順を完了すると、DisabledButton コントロール エクステンダーがツールボックスに表示する必要があります (図 4 参照)。


[![ツールボックスで DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**図 04**: ツールボックスに DisabledButton ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))。


次に、新しい ASP.NET ページを作成する必要があります。 この場合は、以下の手順に従ってください。

1. ShowDisabledButton.aspx をという名前の新しい ASP.NET ページを作成します。
2. ScriptManager をページにドラッグします。
3. TextBox コントロールをページにドラッグします。
4. ボタン コントロールをページにドラッグします。
5. [プロパティ] ウィンドウでボタンの ID プロパティ値に変更<em>btnSave</em>とテキスト プロパティに値*保存\**。
  

標準の ASP.NET の TextBox とボタン コントロールと、ページを作成しました。

次に、DisabledButton エクステンダーの TextBox コントロールを拡張する必要があります。

1. 選択、 **Extender の追加**タスク オプション エクステンダー ウィザード ダイアログを開きます (図 5 を参照してください)。 ダイアログ ボックスに、カスタムの DisabledButton エクステンダーが含まれていることに注意してください。
2. DisabledButton エクステンダーを選択し、クリックして、 **OK**ボタンをクリックします。


[![Extender ウィザード ダイアログ](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**図 05**: エクステンダー ウィザードは、ダイアログ ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))。


最後に、DisabledButton エクステンダーのプロパティを設定することができます。 DisabledButton エクステンダーのプロパティを変更するには、TextBox コントロールのプロパティを変更します。

1. デザイナーで、テキスト ボックスを選択します。
2. エクステンダー ノードを展開し、[プロパティ] ウィンドウ (図 6 参照)。
3. 値を割り当てる*保存*DisabledText プロパティと値を*btnSave* TargetButtonID プロパティにします。


[![エクステンダー プロパティの設定](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**図 06**: エクステンダー プロパティの設定 ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))。


(F5 キーを押して)、ページを実行すると、ボタン コントロールは最初に無効になります。 テキスト ボックスにテキストの入力を開始するとすぐに、コントロールがボタンには、(図 7 を参照) が有効になります。


[![DisabledButton エクステンダーの操作](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**図 07**: The DisabledButton エクステンダーの操作 ([フルサイズの画像を表示する をクリックします](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))。


## <a name="summary"></a>まとめ

このチュートリアルの目的は、カスタム エクステンダー コントロールを AJAX Control Toolkit の拡張方法を説明することでした。 このチュートリアルでは、単純な DisabledButton コントロール エクステンダーを作成しました。 このエクステンダー DisabledButtonExtender クラス、DisabledButtonBehavior JavaScript 動作、および DisabledButtonDesigner クラスを作成して実装します。 カスタム コントロール エクステンダーを作成するたびに、同様の一連の手順に従ってください。

> [!div class="step-by-step"]
> [前へ](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [次へ](get-started-with-the-ajax-control-toolkit-vb.md)
