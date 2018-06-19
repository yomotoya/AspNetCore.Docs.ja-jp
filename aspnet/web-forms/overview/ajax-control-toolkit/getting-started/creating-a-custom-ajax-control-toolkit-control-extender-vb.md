---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Toolkit コントロール エクステンダー (VB) を制御するカスタムの AJAX の作成 |Microsoft ドキュメント
author: microsoft
description: カスタムのエクステンダーを使用すると、カスタマイズし、新しいクラスを作成することがなく ASP.NET コントロールの機能を拡張できます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 06950770bf788fff4a03e9d41fd448ea675a8bce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875137"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>カスタムの AJAX コントロール Toolkit コントロール エクステンダー (VB) を作成します。
====================
によって[Microsoft](https://github.com/microsoft)

> カスタムのエクステンダーを使用すると、カスタマイズし、新しいクラスを作成することがなく ASP.NET コントロールの機能を拡張できます。


このチュートリアルでは、カスタムの AJAX コントロール Toolkit コントロール エクステンダーを作成する方法を学習します。 無効から有効にテキスト ボックスにテキストを入力すると、ボタンの状態を変更する便利で新しい extender は、単純なを作成します。 このチュートリアルを読むと、後に、独自のコントロール エクステンダーを使用して ASP.NET AJAX ツールキットを拡張することができます。

Visual Studio または Visual Web Developer のいずれかを使用してカスタム コントロール エクステンダーを作成することができます (Visual Web Developer の最新バージョンがあることを確認してください)。

## <a name="overview-of-the-disabledbutton-extender"></a>DisabledButton エクステンダーの概要

DisabledButton エクステンダーの名前は、新しいコントロール エクステンダーです。 このエクステンダーは、3 つのプロパティが適用されます。

- TargetControlID - テキスト ボックス コントロールを拡張します。
- TargetButtonIID - が無効または有効になっているボタンをクリックします。
- DisabledText - ボタンで最初に表示されるテキストです。 入力を開始するときに、ボタンは、ボタンのテキスト プロパティの値を表示します。

テキスト ボックスとボタン コントロールに DisabledButton extender をフックするとします。 任意のテキストを入力する前に、ボタンが無効にし、テキスト ボックスとボタンを次に示します。


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


テキストの入力を開始した後、ボタンが有効になっているし、テキスト ボックスとボタンを次に示します。


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


当社コントロール エクステンダーを作成するには、次の 3 つのファイルを作成する必要があります。

- DisabledButtonExtender.vb - このファイルは、サーバー側コントロール クラスを extender の作成を管理し、デザイン時にプロパティを設定することにします。 Extender に設定できるプロパティも定義します。 これらのプロパティは、デザイン時にコードを使用してアクセスできる DisableButtonBehavior.js ファイルで定義されたプロパティと一致します。
- DisabledButtonBehavior.js--このファイルは、すべてのクライアント スクリプトのロジックが追加されます。
- DisabledButtonDesigner.vb - このクラスは、デザイン時の機能を使用します。 Visual Studio または Visual Web Developer デザイナーで正しく機能するコントロール エクステンダーをする場合は、このクラスを作成する必要があります。

したがってコントロール エクステンダーは、サーバー側コントロール、クライアント側の動作、およびサーバー側デザイナー クラスで構成されます。 次のセクションでこれらのファイルの 3 つすべてを作成する方法を学びます。

## <a name="creating-the-custom-extender-website-and-project"></a>カスタム Extender の web サイトおよびプロジェクトの作成

最初の手順では、Visual Studio または Visual Web Developer で、クラス ライブラリ プロジェクトと web サイトを作成します。 お ll、クラス ライブラリ プロジェクトでカスタムのエクステンダーを作成し、web サイトでカスタムのエクステンダーをテストします。

S、web サイトを開始できるようにします。 Web サイトを作成する手順に従います。

1. メニュー オプションを選択**ファイル]、[新しい Web サイト**です。
2. 選択、 **ASP.NET Web サイト**テンプレート。
3. 新しい web サイトの名前を付けます*Website1*です。
4. クリックして、 **OK**ボタンをクリックします。

次に、コントロール エクステンダーのコードを含むクラス ライブラリ プロジェクトを作成する必要があります。

1. メニュー オプションを選択**ファイルを追加、新しいプロジェクト**です。
2. 選択、**クラス ライブラリ**テンプレート。
3. 名前を持つ、新しいクラス ライブラリの名前を付けます**CustomExtenders**です。
4. クリックして、 **OK**ボタンをクリックします。

これらの手順を実行した後、ソリューション エクスプ ローラー ウィンドウは図 1 のようになります。


[![Web サイトとクラス ライブラリ プロジェクトとソリューション](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**図 01**: web サイトとクラス ライブラリ プロジェクトとソリューション ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


次に、すべてのクラス ライブラリ プロジェクトに必要なアセンブリ参照を追加する必要があります。

1. CustomExtenders プロジェクトを右クリックし、メニュー オプションを選択**参照の追加**です。
2. [.NET] タブを選択します。
3. 次のアセンブリへの参照を追加します。

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. [参照] タブを選択します。
5. AjaxControlToolkit.dll アセンブリへの参照を追加します。 このアセンブリは、AJAX コントロール ツールキットをダウンロードしたフォルダーにあります。

追加されていることの右の参照をすべて、プロジェクトを右クリックし、プロパティを選択すると、参照設定 タブをクリックして確認できます (図 2 を参照してください)。


[![必要な参照の参照 フォルダー](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**図 02**: 必要な参照の参照 フォルダー ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>カスタム コントロール エクステンダーを作成します。

ようになりましたが、クラス ライブラリ、当社エクステンダー コントロールのビルドが始めることができます。 S、骨のカスタム エクステンダー コントロール クラス (1 のリストを参照) で始まることができます。

**1 - MyCustomExtender.vb を一覧表示します。**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

クラスの概要、コントロール エクステンダーを一覧表示する 1 ことを確認するいくつかの点があります。 最初に、クラスが基本 ExtenderControlBase クラスから継承されていることを確認します。 すべての AJAX コントロール Toolkit エクステンダー コントロールは、この基本クラスから派生します。 たとえば、基本クラスには、各コントロール エクステンダーの必須プロパティである TargetID プロパティが含まれています。

次に、クラスがクライアント スクリプトに関連する次の 2 つの属性が含まれることに注意してください。

- WebResource - には、アセンブリ内の埋め込みリソースとして含まれるファイルが原因です。
- ClientScriptResource - には、アセンブリから取得するスクリプト リソースが原因です。

WebResource 属性を使用して、カスタムの extender がコンパイルされるときに、MyControlBehavior.js JavaScript ファイルをアセンブリに埋め込みます。 ClientScriptResource 属性を使用して、web ページで、カスタムの extender を使用すると、アセンブリから MyControlBehavior.js スクリプトを取得します。


WebResource と ClientScriptResource 属性動作するためには、埋め込みリソースとして JavaScript ファイルをコンパイルする必要があります。 ソリューション エクスプ ローラー ウィンドウでファイルを選択し、プロパティ シートを開き、値を割り当てる*埋め込まれたリソース*を**ビルド アクション**プロパティです。


コントロールの extender も TargetControlType 属性が含まれることに注意してください。 この属性を使用して、コントロール エクステンダーによって拡張されたコントロールの種類を指定します。 リストの 1 の場合は、コントロール エクステンダーを使用して、テキスト ボックスを拡張します。

最後に、カスタムの extender に MyProperty というプロパティが含まれていることを確認します。 プロパティは、ExtenderControlProperty 属性でマークされています。 クライアント側の動作にサーバー側コントロール エクステンダーからプロパティ値を渡す、GetPropertyValue() および SetPropertyValue() メソッドが使用されます。

S さあ、当社 DisabledButton エクステンダーのコードを実装することができます。 このエクステンダーのコードは、一覧表示する 2 で確認できます。

**2 - DisabledButtonExtender.vb を一覧表示します。**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

DisabledButton エクステンダーを一覧表示する 2 は TargetButtonID および DisabledText という 2 つのプロパティを持ちます。 TargetButtonID プロパティに適用される IDReferenceProperty では、このプロパティに割り当てることのボタン コントロールの ID 以外のものをできません。

WebResource と ClientScriptResource 属性では、このエクステンダー DisabledButtonBehavior.js をという名前のファイルにあるクライアント側の動作を関連付けます。 次のセクションでは、この JavaScript ファイルをについて説明します。

## <a name="creating-the-custom-extender-behavior"></a>Extender のカスタム動作を作成します。

コントロール エクステンダーのクライアント側のコンポーネントには、動作は呼び出されます。 DisabledButton 動作に、無効にして、ボタンを有効化の実際のロジックが含まれます。 リスト 3 の動作では、JavaScript コードが含まれます。

**3 - DisabledButton.js を一覧表示します。**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

リスト 3 での JavaScript ファイルには、クライアント側のクラスが DisabledButtonBehavior をという名前が含まれています。 TargetButtonID をという名前の 2 つのプロパティが、サーバー側ツインと同様に、このクラスに含まれていて、を使用してアクセスできる DisabledText\_TargetButtonID/設定\_TargetButtonID 取得および\_DisabledText/設定\_DisabledText です。

Initialize() メソッドは、動作のターゲット要素の keyup イベント ハンドラーを関連付けます。 この動作に関連付けられているテキスト ボックスに、文字を入力するたびに、keyup ハンドラーを実行します。 Keyup ハンドラーが有効か、または動作に関連付けられているテキスト ボックスが任意のテキストが含まれるかどうかに応じて、ボタンを無効にします。

埋め込みリソースとして一覧表示する 3 での JavaScript ファイルをコンパイルする必要がありますに注意してください。 ソリューション エクスプ ローラー ウィンドウでファイルを選択し、プロパティ シートを開き、値を割り当てる*埋め込まれたリソース*を**ビルド アクション**プロパティ (図 3 を参照してください)。 このオプションは、Visual Studio と Visual Web Developer の両方で使用できます。


[![埋め込みリソースとしての JavaScript ファイルを追加します。](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**図 03**: 埋め込みリソースとしての JavaScript ファイルを追加する ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Extender のカスタム デザイナーを作成します。

作成、extender を完了に必要な最後クラスは 1 つです。 デザイナー クラスを一覧表示する 4 で作成する必要があります。 Visual Studio または Visual Web Developer デザイナーで正しく動作拡張を行うには、このクラスが必要です。

**4 - DisabledButtonDesigner.vb を一覧表示します。**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

デザイナーを一覧表示する 4 では、デザイナーの属性を持つ DisabledButton extender に関連付けます。次のように DisabledButtonExtender クラスに Designer 属性を適用する必要があります。

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>カスタムの Extender の使用

DisabledButton コントロール エクステンダーの作成を完了しましたが、これで、ASP.NET web サイトで使用する時間を勧めします。 最初に、ツールボックスにカスタムのエクステンダーを追加する必要があります。 この場合は、以下の手順に従ってください。

1. ソリューション エクスプ ローラー ウィンドウでページをダブルクリックして、ASP.NET ページを開きます。
2. ツールボックスを右クリックし、メニュー オプションを選択**アイテムの選択**です。
3. ツールボックス アイテムの選択 ダイアログ ボックスで、CustomExtenders.dll アセンブリを参照します。
4. をクリックして、 **OK**ダイアログを閉じるボタンをクリックします。

次の手順を完了すると、DisabledButton コントロール エクステンダーがツールボックスに表示する必要があります (図 4 を参照してください)。


[![ツールボックスで DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**図 04**: ツールボックスに DisabledButton ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


次に、新しい ASP.NET ページを作成する必要があります。 この場合は、以下の手順に従ってください。

1. ShowDisabledButton.aspx をという名前の新しい ASP.NET ページを作成します。
2. ScriptManager をページにドラッグします。
3. テキスト ボックス コントロールをページにドラッグします。
4. ボタン コントロールをページにドラッグします。
5. [プロパティ] ウィンドウでボタン ID プロパティ値を変更<em>btnSave</em>し、テキストのプロパティ値を*保存\** です。
  

標準の ASP.NET のテキスト ボックスとボタン コントロールと、ページを作成しました。

次に、DisabledButton エクステンダーのある TextBox コントロールを拡張する必要があります。

1. 選択、 **Extender の追加**Extender ウィザード ダイアログを開くにはオプションのタスク (図 5 を参照してください)。 ダイアログ ボックスに、カスタムの DisabledButton extender が含まれることに注意してください。
2. DisabledButton extender を選択し、クリックして、 **OK**ボタンをクリックします。


[![Extender ウィザード ダイアログ](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**図 05**: Extender ウィザードは、ダイアログ ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


最後に、DisabledButton extender のプロパティを設定できます。 DisabledButton extender のプロパティを変更するには、テキスト ボックス コントロールのプロパティを変更します。

1. デザイナーで、テキスト ボックスを選択します。
2. [プロパティ] ウィンドウでノードを展開しますエクステンダー (図 6 を参照してください)。
3. 値を割り当てる*保存*DisabledText プロパティと値を*btnSave* TargetButtonID プロパティにします。


[![エクステンダー プロパティを設定](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**図 06**: エクステンダー プロパティを設定 ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


(F5 キーを押す) によって、ページを実行すると、ボタン コントロールは、最初に無効になります。 テキスト ボックスにテキストの入力を開始するとすぐに、コントロールは、ボタンには、(図 7 を参照) が有効になります。


[![アクションで DisabledButton エクステンダー](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**図 07**: アクションで、DisabledButton エクステンダー ([フルサイズのイメージを表示するをクリックして](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>まとめ

このチュートリアルの目的は、カスタムのエクステンダー コントロールの AJAX コントロール ツールキットを拡張する方法を説明することでした。 このチュートリアルでは、単純な DisabledButton コントロール エクステンダーを作成しました。 DisabledButtonExtender クラス、DisabledButtonBehavior JavaScript 動作、および DisabledButtonDesigner クラスを作成することでこのエクステンダーを実装しました。 カスタム コントロール エクステンダーを作成するときに類似した一連の手順に従います。

> [!div class="step-by-step"]
> [前へ](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
