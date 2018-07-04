---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: ASP.NET AJAX のローカライズを理解する |Microsoft Docs
author: scottcate
description: ローカリゼーションとは、設計し、アプリケーションまたはアプリケーション コンポーネントに特定の言語とカルチャのサポートを統合するプロセスです。 Mic.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 7f089e147ae9c4c42da0ca798149488043480a79
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382521"
---
<a name="understanding-aspnet-ajax-localization"></a>ASP.NET AJAX のローカライズを理解します。
====================
によって[Scott Cate](https://github.com/scottcate)

[PDF のダウンロード](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> ローカリゼーションとは、設計し、アプリケーションまたはアプリケーション コンポーネントに特定の言語とカルチャのサポートを統合するプロセスです。 Microsoft ASP.NET プラットフォームの標準の ASP.NET アプリケーションのローカライズを標準 .NET ローカライズ モデルを統合することによって広範なサポートを提供します。Microsoft AJAX フレームワークでは、ローカライズを実行できる多様なシナリオをサポートする統合モデルを利用します。


## <a name="introduction"></a>はじめに

マイクロソフトの ASP.NET テクノロジ オブジェクト指向し、イベント ドリブン プログラミング モデルとそれを組み合わせたのコンパイル済みコードの利点があります。 ただし、System.Web.Extensions の名前空間は、.NET Framework で Microsoft AJAX サービスをカプセル化に含まれる新しい機能では多くのアドレスを指定することができます、テクノロジ固有のいくつかの欠点には、サーバー側の処理モデル3.5。 これらの拡張機能では、ASP.NET 2.0 AJAX Extensions の一部が、Framework 基本クラス ライブラリの一部になりました以前に使用できる多くの機能豊富なクライアント機能が有効にします。 コントロール、およびこの名前空間の機能は、ページの部分的なレンダリングを含める (ASP.NET のプロファイル API を含む)、クライアント スクリプトを使用して Web サービスにアクセスする機能、ページ全体の更新を必要とせず、広範なクライアント側 API は、多くのミラー化するように設計ASP.NET サーバー側コントロール セットに表示される制御スキーム。

このホワイト ペーパーは、ローカライズのサポートと既に統合 web でのローカライズのサポートを確認するビジネス ニーズのコンテキストで Microsoft AJAX スクリプト ライブラリ、Microsoft AJAX フレームワーク内にあるローカライズ機能を調べます.NET Framework が提供するアプリケーション。 Microsoft AJAX スクリプト ライブラリでは、統合された IDE サポートと共有可能なリソースの種類を提供する、.NET アプリケーションで既に使用する .resx ファイル形式で使用します。

このホワイト ペーパーでは、ベースの Microsoft Visual Studio 2008 の Beta 2 リリースにします。 このホワイト ペーパーには、いない Visual Web Developer Express を Visual Studio 2008 で使用され、チュートリアルに従って、Visual Studio のユーザー インターフェイスを提供することも想定しています。 一部のコード サンプルでは、Visual Web Developer Express で使用できるプロジェクト テンプレートを利用します。

## <a name="the-need-for-localization"></a>*ローカライズの必要性*

エンタープライズ アプリケーション開発者およびコンポーネントの開発者、特にのカルチャおよび言語間の違いを認識できるツールを作成する機能がますます必要になっています。 クライアントのロケールに適応することができますコンポーネントを設計して、開発者の生産性の向上、グローバルに機能するコンポーネントの調整に必要な作業の量が減少します。

ローカリゼーションとは、設計し、アプリケーションまたはアプリケーション コンポーネントに特定の言語とカルチャのサポートを統合するプロセスです。 Microsoft ASP.NET プラットフォームの標準の ASP.NET アプリケーションのローカライズを標準 .NET ローカライズ モデルを統合することによって広範なサポートを提供します。Microsoft AJAX フレームワークでは、ローカライズを実行できる多様なシナリオをサポートする統合モデルを利用します。 Microsoft AJAX フレームワーク スクリプトかローカライズできますサテライト アセンブリに配置されているか、静的なファイル システムの構造を利用することで。

## <a name="embedding-scripts-with-satellite-assemblies"></a>*サテライト アセンブリを使用した埋め込みスクリプト*

標準の .NET Framework ローカライズ戦略と一貫性のある、リソースはサテライト アセンブリに含めることができます。 サテライト アセンブリの利点がいくつかのバイナリの従来のリソースの包含大きなイメージを更新することがなく、特定のローカリゼーションを更新できるにサテライト アセンブリをインストールするだけで追加のローカライズ版を展開することができますメイン プロジェクト アセンブリの再読み込みを発生させることがなく、プロジェクト フォルダー、およびサテライト アセンブリを展開できます。 ASP.NET プロジェクトで特にはこれで増分更新は、使用されるシステム リソースの量を大幅に削減することができますので便利ですし、実稼働 web サイトの使用量を最小限中断します。

それらを含めることによってアセンブリに埋め込まれているスクリプト ファイルで、コンパイル時にアセンブリに含まれるでは、.resx を管理 (または .resources にコンパイル)。 アセンブリ レベル属性を使用して、AJAX ランタイムによって生成されるコードのスクリプト アプリケーションで利用できるし、そのリソース

*埋め込みスクリプト ファイルの名前付け規則*

Microsoft AJAX フレームワーク スクリプトの管理は、配置と、スクリプトのテストで使用するためのさまざまなオプションをサポートしているし、ガイドラインは、これらのオプションを容易に提供されます。

*デバッグを容易にするには*

リリース (運用) スクリプトを含めないでください、`.debug`ファイル名の修飾子です。 スクリプトに含める必要がありますデバッグ向けに設計された`.debug`ファイル名にします。

*ローカライズしやすくします。*

ニュートラル カルチャのスクリプトは、任意のカルチャ識別子、ファイル名を含めないでください。 ローカライズされたリソースが含まれているスクリプトでは、ファイル名で ISO 言語コードを指定してください。 たとえば、`es-CO`スペイン語、コロンビア特別区の略です。

次の表は、ファイルの名前付け規則の例をまとめたものです。

| ファイル名 | 説明 |
| --- | --- |
| Script.js | リリース バージョンのカルチャに依存しないスクリプトです。 |
| Script.debug.js | デバッグ バージョンのカルチャに依存しないスクリプトです。 |
| Script.en-US.js | リリース バージョン英語、米国スクリプトです。 |
| Script.debug.es-CO.js | デバッグ バージョンのスペイン語、コロンビア特別区スクリプトです。 |

## <a name="walkthrough-create-an-localized-embedded-script"></a>チュートリアル: ローカライズされた、埋め込みのスクリプトを作成します。

*注: このチュートリアルでは、クラス ライブラリ プロジェクトのプロジェクト テンプレートを含まない Visual Web Developer Express と Visual Studio 2008 の使用が必要です。*

1. 統合 ASP.NET AJAX extensions には、新しい Web サイト プロジェクトを作成します。 別のプロジェクト、LocalizingResources と呼ばれる、ソリューション内のクラス ライブラリ プロジェクトを作成します。
2. LocalizingResources プロジェクトに VerifyDeletion.js をという名前の Jscript ファイルだけでなく、DeletionResources.resx と DeletionResources.es.resx という .resx リソース ファイルを追加します。 前者はカルチャに依存しないリソースを含む後者はスペイン語のリソースを含めます。
3. VerifyDeletion.js には、次のコードを追加します。

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

JavaScript の正規表現の構文、1 つのスラッシュ内のテキストに不慣れな方 (前の例では、/FILENAME/一例です)、RegExp オブジェクトを表します。 MSDN ライブラリには、広範な JavaScript のリファレンスが含まれていますを JavaScript のネイティブ オブジェクトにリソースをオンラインに見つかります。

1. DeletionResources.resx には、次のリソース文字列を追加します。 

    **VerifyDelete**: ファイル名を削除してよろしいですか?

    **削除**: ファイル名が削除されました。

1. DeletionResources.es.resx には、次のリソース文字列を追加します。 

    **VerifyDelete**: Est seguro キュー desee quitar FILENAME でしょうか。

    **削除**: FILENAME se ha quitado します。
2. AssemblyInfo ファイルには、次のコード行を追加します。

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. LocalizingResources プロジェクトには、System.Web および System.Web.Extensions への参照を追加します。
2. Web サイト プロジェクトから LocalizingResources プロジェクトへの参照を追加します。
3. Web サイト プロジェクトで、default.aspx では、次の追加のマークアップで、ScriptManager コントロールを更新します。

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. このページでは、上の任意の default.aspx には、このマークアップを含めます。

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. F5 キーを押します。 メッセージが表示されたら、デバッグを有効にします。 ページが読み込まれるときに、削除ボタンをクリックします。 求められます英語で (ただし、コンピューターは、既定ではスペイン語のリソースを優先する設定が) 確認のために注意してください。
2. ブラウザー ウィンドウを閉じて、default.aspx に戻ります。 @Pageヘッダー ディレクティブ、ES-ES 持つカルチャと UICulture 置換 auto。 ブラウザーで web アプリケーションをもう一度を起動するには、もう一度 F5 キーを押します。 この時点では、スペイン語でファイルの削除を求められることに注意してください。


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-localization/_static/image3.png))。


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-localization/_static/image6.png))。


このチュートリアルでは、いくつかのバリエーションがあることに注意してください。 たとえば、スクリプトでしたを登録する、ScriptManager コントロール プログラムでページの読み込み中に。

## <a name="including-a-static-script-file-structure"></a>*静的なスクリプト ファイルの構造を含む*

静的なスクリプト ファイルを使用して、展開、本質的な .NET のローカリゼーションのスキームを使用する利点の一部が失われます。 主に表示されるようなスクリプト リソース ファイルから生成された自動型が失われるは、します。たとえば、上記のチュートリアルではリソースが、ScriptManager コントロールからメッセージを呼び出す自動的に生成されたの型によって公開されるされました。

ただし、静的なスクリプト ファイルの構造を使用するには、いくつか利点があります。 再コンパイルして、サテライト アセンブリを再デプロイせずに更新プログラムを実行でき、静的ファイルの構造体の使用を行うコンポーネントとマイナーな出荷されていない可能性があります機能を統合する埋め込みのスクリプトを上書きすることもできます。

Microsoft では、プロジェクトのコンパイル中に、スクリプト リソースを自動的に生成することによって、バージョン管理に関する問題を回避することをお勧めします。 広範なスクリプト コードを基本保守する際にローカライズされた各スクリプトにコードの変更が反映されることを確認ますます難しくになることができます。 代わりに、だけを保持できます 1 つのロジック スクリプトと複数のローカリゼーション スクリプト プロジェクトのビルド中にファイルの結合します。

静的なスクリプトをファイルにする必要がありますが、追加することでいずれかで参照宣言によってリソースがないため、`<asp:ScriptElement>`要素の子として、`<Scripts>`またはプログラムで追加することで、ScriptManager コントロールのタグ`ScriptReference`オブジェクト`Scripts`のプロパティ、`ScriptManager`実行時にページ上のコントロール。

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*Scriptmanager コントロールとそのロールのローカリゼーション*

Scriptmanager コントロールには、ローカライズされたアプリケーションの複数の自動動作が有効にします。

- スクリプト ファイルの設定と名前付け規則; に基づいてが自動的に検出します。たとえば、デバッグ モードでデバッグが有効なスクリプトを読み込むし、ローカライズされたブラウザーのユーザー インターフェイスの選択に基づいてスクリプトを読み込みます。
- これにより、カルチャ、カスタムのカルチャなどの定義ができます。
- HTTP 経由でのスクリプト ファイルの圧縮ができます。
- 多くの要求を効率的に管理するスクリプトをキャッシュします。
- 暗号化された URL をパイプすることによって、間接レイヤーをスクリプトに追加します。

プログラムまたは宣言型マークアップによって、スクリプト参照を ScriptManager コントロールに追加できます。 宣言型マークアップは特に便利です自体には、web サイト プロジェクト以外に埋め込まれたアセンブリ スクリプトを操作するときに、スクリプトの名前は可能性がありますが変更されない、リビジョンがプッシュされるとします。

## <a name="summary"></a>まとめ

広範囲のカルチャおよびコミュニティにアクセスできる必要になります core は、ビジネス モデルを web アプリケーションがより多くの使用者に到達するにつれて、電子商取引 web アプリケーションは、外部の通貨を扱うことができる必要があります。、コンテンツ管理システムは、この必要があることを把握する必要がありますのコンテンツが、そのナビゲーション ヒントとその他の言語、および企業内のフォーム フィールドもできるだけでなく存在する必要があります。アクセスできます。

.NET Framework には、リソース文字列とイメージを検索する一貫した方法を表示するには、サテライト アセンブリと XML リソース (.resx) ファイルを利用して豊富なローカライズ フレームワークでは、本質的にサポートします。 Microsoft AJAX フレームワークおよび Microsoft AJAX スクリプト ライブラリを含む、ASP.NET AJAX Extensions をサポートしてこのプログラミング モデルをクライアント側のコードに簡単にリソース文字列の検索を有効にします。 サテライト アセンブリは、ファイル名が指定された名前付けスキームに従う限り、自動 ScriptResource.axd により、スクリプト リソース (実際の .js ファイル) を含めることをサポートします。 このサポートにより、ASP.NET AJAX Extensions には、スクリプトのローカライズおよびアプリケーションのグローバリゼーションが簡略化します。

## <a name="bio"></a>*自己紹介*

1997 年からマイクロソフトの Web テクノロジで働いてあり myKB.com プレジデント、Scott Cate ([www.myKB.com](http://www.myKB.com)) ベースのナレッジ ベースのソフトウェア ソリューションに重点を置いてアプリケーションを ASP.NET の記述を専門としています。 Scott は時に電子メールが接続可能[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)またはで彼のブログ[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [次へ](understanding-asp-net-ajax-web-services.md)
