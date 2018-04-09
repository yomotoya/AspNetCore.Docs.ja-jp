---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: ASP.NET AJAX のローカライズを理解する |Microsoft ドキュメント
author: scottcate
description: ローカライズは、設計と、アプリケーションまたはアプリケーション コンポーネントに特定の言語とカルチャのサポートを統合するプロセスです。 Mic しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 565b0294f57b784bc592b286b3d8b28504110415
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-localization"></a>ASP.NET AJAX のローカライズを理解します。
====================
によって[Scott カテゴリ](https://github.com/scottcate)

[PDF をダウンロードします。](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> ローカライズは、設計と、アプリケーションまたはアプリケーション コンポーネントに特定の言語とカルチャのサポートを統合するプロセスです。 Microsoft ASP.NET プラットフォームは、標準の .NET のローカリゼーション モデル; を統合することによって標準の ASP.NET アプリケーションのローカライズ用広範なサポートを提供します。Microsoft AJAX Framework は、ローカリゼーションを実行できるさまざまなシナリオをサポートする統合型のモデルを利用します。


## <a name="introduction"></a>はじめに

ASP.NET の Microsoft のテクノロジは、オブジェクト指向し、イベント ドリブン プログラミング モデルによりに導入され、コンパイルされたコードのメリットと組み合わせたです。 ただし、いくつかの欠点の多くは System.Web.Extensions の名前空間は、.NET Framework で Microsoft AJAX サービスをカプセル化に含まれる新機能によって対処できます、テクノロジ固有は、サーバー側の処理モデル3.5。 これらの拡張機能では、ASP.NET 2.0 AJAX Extensions の一部がフレームワークの基底クラス ライブラリの一部では今すぐとして以前に使用可能な多数のリッチ クライアント機能が有効にします。 コントロール、およびこの名前空間の機能は、ページの部分のレンダリングを含める (プロファイル API、ASP.NET を含む)、クライアント スクリプトを使用して Web サービスにアクセスする機能、ページ全体の更新を必要とせず、広範なクライアント側 API は、の多くをミラー化するよう設計されていますコントロール パターンは、ASP.NET サーバー側コントロール セットに表示します。

このホワイト ペーパーに関するローカライズ サポートし、web でのローカライズのサポートを既に統合を確認するビジネス ニーズのコンテキストでは、Microsoft AJAX Framework と Microsoft AJAX スクリプト ライブラリに存在するローカリゼーション機能を検査します.NET Framework が提供するアプリケーション。 Microsoft AJAX スクリプト ライブラリでは、統合された IDE サポートと共有可能なリソースの種類を提供する .NET アプリケーションで既に使用されてする .resx ファイル形式を利用します。

このホワイト ペーパーをベースに Microsoft Visual Studio 2008 の Beta 2 リリースします。 このホワイト ペーパーにはない Visual Web Developer Express、Visual Studio 2008 を操作し、Visual Studio のユーザー インターフェイスに従ってチュートリアルを提供するも想定しています。 一部のコード サンプルでは、Visual Web Developer Express で使用できるプロジェクト テンプレートを利用します。

## <a name="the-need-for-localization"></a>*ローカライズの必要性*

エンタープライズ アプリケーションの開発者およびコンポーネントの開発者の場合は、特にのカルチャおよび言語の違いを認識できるツールを作成する機能がますます必要になりました。 クライアントのロケールに適応する機能を使用したコンポーネントをデザイン開発者の生産性が向上し、グローバルに機能するコンポーネントの学習機能に必要な作業の量が減少します。

ローカライズは、設計と、アプリケーションまたはアプリケーション コンポーネントに特定の言語とカルチャのサポートを統合するプロセスです。 Microsoft ASP.NET プラットフォームは、標準の .NET のローカリゼーション モデル; を統合することによって標準の ASP.NET アプリケーションのローカライズ用広範なサポートを提供します。Microsoft AJAX Framework は、ローカリゼーションを実行できるさまざまなシナリオをサポートする統合型のモデルを利用します。 Microsoft AJAX framework スクリプトか、ローカライズできますサテライト アセンブリに配置されているか、静的なファイル システム構造体を使用することによってです。

## <a name="embedding-scripts-with-satellite-assemblies"></a>*サテライト アセンブリを使用して埋め込みのスクリプト*

標準の .NET Framework のローカリゼーション戦略に一貫性のある、リソースはサテライト アセンブリに含めることができます。 サテライト アセンブリの利点がいくつかのバイナリの従来のリソース包含より大きいイメージを更新することがなく、特定のローカリゼーションを更新できるにサテライト アセンブリをインストールするだけでその他のローカライズ版を展開することができますプロジェクト フォルダー、およびサテライト アセンブリは、メイン プロジェクト アセンブリの再読み込みを発生させることがなく展開できます。 ASP.NET プロジェクトで特にこれと役に立つで増分更新は、使用されているシステム リソースの量が大幅に低下するため、最小 実稼働 web サイトの使用法が中断されます。

スクリプトに含めることによってアセンブリに埋め込まれているコンパイル時にアセンブリに含まれているファイルで管理される .resx (または .resources のコンパイル)。 スクリプト アプリケーションにアセンブリ レベル属性を使用して、AJAX ランタイムによって生成されるコード経由で利用できるし、そのリソース

*埋め込みのスクリプト ファイルの名前付け規則*

Microsoft AJAX Framework スクリプトの管理は、展開やスクリプトのテストで使用するためのさまざまなオプションをサポートしているし、ガイドラインは、これらのオプションを容易にするために提供されます。

*デバッグを容易にするには*

リリース (運用) スクリプトを含めることはできません、`.debug`ファイル名の修飾子です。 スクリプトに含める必要があるデバッグ用に設計された`.debug`ファイル名にします。

*ローカリゼーションしやすくします。*

ニュートラル カルチャのスクリプトは、ファイル名の任意のカルチャ識別子を含めないでください。 ローカライズされたリソースを含んでいるスクリプト、ISO 言語コードをファイル名に指定してください。 たとえば、`es-CO`スペイン語、コロンビアを意味します。

次の表は、名前付け規則の例で、ファイルをまとめたものです。

| ファイル名 | 説明 |
| --- | --- |
| Script.js | リリース バージョンのカルチャに依存しないスクリプトです。 |
| Script.debug.js | デバッグ バージョンのカルチャに依存しないスクリプトです。 |
| Script.en-US.js | リリース バージョン英語、米国スクリプト。 |
| Script.debug.es-CO.js | デバッグ バージョン スペイン語、コロンビア スクリプトです。 |

## <a name="walkthrough-create-an-localized-embedded-script"></a>チュートリアル: ローカライズされた、埋め込みのスクリプトを作成します。

*注: このチュートリアルでは、クラス ライブラリ プロジェクトのプロジェクト テンプレートを含まない Visual Web Developer Express と Visual Studio 2008 の使用が必要です。*

1. 統合された ASP.NET AJAX Extensions に新しい Web サイト プロジェクトを作成します。 別のプロジェクト、LocalizingResources と呼ばれる、ソリューション内のクラス ライブラリ プロジェクトを作成します。
2. Jscript ファイル VerifyDeletion.js LocalizingResources プロジェクトだけでなく DeletionResources.resx および DeletionResources.es.resx という .resx リソース ファイルを追加します。 前者はカルチャ ニュートラル リソースです。後者はスペイン語のリソースを含めます。
3. VerifyDeletion.js に次のコードを追加します。

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

JavaScript の正規表現の構文、1 つのスラッシュ内のテキストに慣れていない場合 (前の例では、/FILENAME/例に示します) RegExp オブジェクトを表します。 MSDN ライブラリには、広範な JavaScript の参照が含まれていますを JavaScript ネイティブ オブジェクト上のリソースをオンラインに見つかります。

1. DeletionResources.resx に次のリソース文字列を追加します。 

    **VerifyDelete**: ファイル名を削除しますか?

    **削除**: ファイル名が削除されました。

1. DeletionResources.es.resx に次のリソース文字列を追加します。 

    **VerifyDelete**: Est seguro que desee quitar FILENAME しますか?

    **削除**: FILENAME se ha quitado です。
2. AssemblyInfo ファイルに次のコード行を追加します。

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. LocalizingResources プロジェクトに System.Web および System.Web.Extensions への参照を追加します。
2. Web サイト プロジェクトから LocalizingResources プロジェクトへの参照を追加します。
3. Default.aspx で、Web サイト プロジェクトの下には、次の追加マークアップ ScriptManager コントロールを更新します。

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Default.aspx で、任意の場所ページで、このマークアップのとおりです。

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. F5 キーを押します。 メッセージが表示されたら、デバッグを有効にします。 ページが読み込まれるときに、削除ボタンをクリックします。 求められます英語で (ただし、コンピューターは、既定ではスペイン語のリソースを優先する設定が) 確認のために注意してください。
2. ブラウザー ウィンドウを閉じて、default.aspx に戻ります。 @Pageヘッダー ディレクティブ、ES-ES 持つ Culture、UICulture の auto を置換します。 F5 キーを押してもう一度、ブラウザーで web アプリケーションを再起動します。 この時点では、スペイン語でファイルを削除する入力を求め、注意してください。


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-localization/_static/image6.png))


このチュートリアルではいくつかのバリエーションがあることに注意してください。 たとえば、スクリプトでしたを登録する ScriptManager コントロール プログラムでページの読み込み中に。

## <a name="including-a-static-script-file-structure"></a>*静的なスクリプト ファイルの構造を含む*

静的なスクリプト ファイルの展開を使用する場合、本質的な .NET のローカリゼーション スキームを使用する利点の一部が失われます。 主に表示されるようなスクリプト リソース ファイルから生成された自動型が失われることは、します。たとえば、前のチュートリアルにはリソースが ScriptManager コントロールからメッセージを呼び出す自動的に生成されたの型によって公開されるされました。

ただし、静的なスクリプト ファイルの構造を使用するには、いくつか利点があります。 サテライト アセンブリを再コンパイルしてせずに更新プログラムを実行でき、静的ファイルの構造の使用をコンポーネントに出荷されていない可能性があります機能のマイナー部分を統合する埋め込みのスクリプトを上書きすることもできます。

Microsoft では、プロジェクトのコンパイル中に、スクリプト リソースを自動的に生成することによって、バージョン コントロールの問題を回避することをお勧めします。 基本の広範なスクリプト コードを維持、ときにコードの変更が各ローカライズされたスクリプトに反映されるようにすることがますます困難になることができます。 代わりに、単に維持できますロジック スクリプトを 1 つとローカリゼーションの複数のスクリプト プロジェクトのビルド中に、ファイルのマージします。

ファイルすることは、静的なスクリプトに追加することでいずれかが参照されている宣言によってリソースがないため、`<asp:ScriptElement>`要素の子として、 `<Scripts>` ScriptManager コントロールまたはプログラムによって追加するタグ`ScriptReference`オブジェクト`Scripts`のプロパティ、`ScriptManager`実行時にページ上のコントロールです。

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*Scriptmanager コントロールとその役割のローカリゼーション*

ScriptManager は、ローカライズされたアプリケーションのいくつかの自動動作を有効にします。

- スクリプト ファイルの設定と名前付け規則; に基づいてが自動的に検出します。たとえば、デバッグ モードでデバッグが有効なスクリプトが読み込まれ、ローカライズされたブラウザーのユーザー インターフェイスの選択に基づいてスクリプトを読み込みます。
- を含むカスタム カルチャのカルチャの定義を可能になります。
- HTTP 経由でスクリプト ファイルの圧縮を使用します。
- 多くの要求を効率的に管理するためのスクリプトをキャッシュします。
- 暗号化された URL からそれらをパイプして、間接レイヤーをスクリプトに追加します。

スクリプト参照は、プログラムまたは宣言型マークアップによって ScriptManager コントロールに追加できます。 宣言型マークアップ便利を扱う場合はスクリプト自体、web サイト プロジェクト以外に埋め込まれているアセンブリ、スクリプトの名前は可能性がありますが変更されない、リビジョンがプッシュされるとします。

## <a name="summary"></a>まとめ

Web アプリケーションが大勢に到達するにつれてより広範囲のカルチャおよびコミュニティに到達できるになるコアは、ビジネス モデルと電子商取引 web アプリケーションを外部の通貨を扱うことができる必要があります。、コンテンツ管理システムは、このようなことを把握する必要があります、コンテンツ、ナビゲーション ヒントやも他の言語、および企業内のフォーム フィールドだけでなくすることが存在する必要があります。アクセスできます。

.NET Framework には、リソース文字列やイメージを検索する一貫した方法を表示するには、サテライト アセンブリと XML リソース (.resx) ファイルを利用して豊富なローカライズ フレームワークでは、本質的にサポートします。 Microsoft AJAX Framework および Microsoft AJAX スクリプト ライブラリを含む、ASP.NET AJAX 拡張機能をサポートしてこのプログラミング モデル、クライアント側のコードへ簡単リソース文字列の検索を有効にします。 サテライト アセンブリは、ファイル名が指定された名前付けスキームに従う限り ScriptResource.axd でスクリプト リソース (実際の .js ファイル) の自動追加をサポートします。 このサポートは、ASP.NET AJAX 拡張機能は、スクリプトのローカリゼーションとグローバリゼーションのアプリケーションの簡略化します。

## <a name="bio"></a>*Bio*

Scott カテゴリは、1997 年以降の Microsoft の Web テクノロジの使用されているがあり、myKB.com の代表者 ([www.myKB.com](http://www.myKB.com))、専門分野は、ASP.NET の書き込みの際にベースのアプリケーションのナレッジ ベースのソフトウェア ソリューションに重点を置きます。 Scott が接続時に電子メール[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)または彼のブログで[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [次へ](understanding-asp-net-ajax-web-services.md)
