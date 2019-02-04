---
title: Razor Components の概要
author: guardrex
description: Blazor を探索します。これは C#/Razor や HTML を使用する .NET Web フレームワークであり WebAssembly を使用してブラウザー内で実行されます。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/index
ms.openlocfilehash: 0f7a4b2ec404b6ceec2e643fea9e0635de6ad716
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667940"
---
# <a name="introduction-to-razor-components"></a>Razor Components の概要

作成者: [Steve Sanderson](http://blog.stevensanderson.com)、[Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

*ASP.NET Core Razor Components* はサーバー側において ASP.NET Core で実行されます。一方、*Blazor* (クライアント側で実行される Razor Components) は C#/Razor や HTML を使用する実験的な .NET Web フレームワークであり、WebAssembly を使用してブラウザー内で実行されます。 Blazor では、クライアント上で .NET を使用するクライアント側 Web UI フレームワークの利点がすべて提供されます。

Web 開発は長年にわたって多くの点で改善されてきましたが、最新の Web アプリの構築にはまだ課題があります。 ブラウザー内で .NET を使用すると、開発の簡略化や生産性向上に役立つ次のような多くの利点が得られます。

* **安定性と一貫性**: .NET では、安定性に優れ機能が豊富で使いやすい、標準化されたプログラミング フレームワークがプラットフォームを超えて実現されます。
* **最新の革新的な言語**: .NET 言語は、革新的な新しい言語機能によって常に向上し続けています。
* **業界をリードするツール**: Visual Studio 製品ファミリでは、Windows、Linux、および macOS のプラットフォームを超えて非常にすばらしい .NET 開発エクスペリエンスが提供されています。
* **速度とスケーラビリティ**: .NET では、アプリ開発のためのパフォーマンス、信頼性、およびセキュリティにおける強固な歴史があります。 フル スタック ソリューションとして .NET を使用することにより、信頼性が高くセキュリティで保護された高速のアプリをさらに容易に構築できるようになります。
* **既存のスキルを活用するフルスタック開発**: C#/Razor 開発者は、既に身に付けている C#/Razor スキルを使用してクライアント側コードを記述し、サーバーおよびクライアント側ロジックをアプリ間で共有します。
* **広範なブラウザー サポート**: Razor Components では、通常のマークアップおよび JavaScript として UI がレンダリングされます。 Blazor は、オープンな Web 標準を使用してブラウザー内の .NET 上で実行され、プラグインもコード トランスパイルも必要ありません。 Blazor はモバイル ブラウザーも含めて、すべての最新の Web ブラウザーで動作します。

## <a name="hosting-models"></a>ホスティング モデル

### <a name="server-side-hosting-model"></a>サーバー側のホスティング モデル

Razor Components ではコンポーネントのレンダリング ロジックが UI の更新の適用方法から切り離されているため、Razor Components をホストする方法には柔軟性があります。 .NET Core 3.0 の ASP.NET Core Razor Component には、ASP.NET Core アプリによってサーバー上で Razor Components をホストするためのサポートが追加されています。この場合、すべての UI の更新が SignalR 接続を介して処理されます。 ランタイムでは、ブラウザーからサーバーへの UI イベントの送信が処理され、次に、コンポーネントの実行後、サーバーによってブラウザーに返送された UI の更新が適用されます。 JavaScript 相互運用の呼び出しの処理にも同じ接続が使用されます。

![Razor Components では、サーバー上で .NET コードが実行され、SignalR 接続を介してクライアント上のドキュメント オブジェクト モデルとのやりとりが行われます。](index/_static/aspnet-core-razor-components.png)

詳細については、「<xref:razor-components/hosting-models#server-side-hosting-model>」を参照してください。

### <a name="client-side-hosting-model"></a>クライアント側のホスティング モデル

比較的新しいテクノロジである [WebAssembly](http://webassembly.org) (*wasm* と略される) によって、Web ブラウザー内で .NET コードを実行することが可能になります。 WebAssembly はオープンな Web 標準であり、プラグインを使わずに Web ブラウザー内でサポートされます。 WebAssembly は、ダウンロードを高速化し実行速度を最大限に高めるために最適化されたコンパクトなバイトコード形式です。

WebAssembly コードを使用すると、JavaScript 相互運用を介してブラウザーのすべての機能にアクセスすることができます。 それと同時に、WebAssembly コードは、クライアント コンピューター上での悪意のあるアクションを禁止するために JavaScript と同じ信頼されたサンドボックス内で実行されます。

![Blazor では、WebAssembly を使用してブラウザー内で .NET コードが実行されます。](index/_static/blazor.png)

Blazor アプリをビルドしてブラウザーで実行する場合: 

1. C# コード ファイルと Razor ファイルが .NET アセンブリにコンパイルされます。
1. そのアセンブリと .NET ランタイムがブラウザーにダウンロードされます。
1. Blazor では JavaScript を使用して .NET ランタイムがブートス トラップされると共に、必要なアセンブリ参照を読み込むようにランタイムが構成されます。 ドキュメント オブジェクト モデル (DOM) 操作とブラウザー API 呼び出しは、JavaScript 相互運用を介して Blazor ランタイムによって処理されます。

WebAssembly をサポートしていない、より古いブラウザーをサポートするには、ASP.NET Core Razor Components[ のサーバー側ホスティング モデル](#server-side-hosting-model)を使用します。

詳細については、「<xref:razor-components/hosting-models#client-side-hosting-model>」を参照してください。

## <a name="components"></a>コンポーネント

アプリは "*コンポーネント*" を使用してビルドされます。 コンポーネントとはページ、ダイアログ、データ エントリ フォームなど UI の一部です。 コンポーネントは入れ子にしたり、再利用したり、プロジェクト間で共有したりできます。

"*コンポーネント*" は .NET クラスです。 このクラスは C# クラス (*\*.cs*) として直接書き込むことも、より一般的な方法として Razor マークアップ ページの形式 (*\*.cshtml*) で書き込むこともできます。

[Razor](/aspnet/core/mvc/views/razor) とは、HTML マークアップを C# コードに結合するための構文です。 Razor は開発者の生産性向上を目的に設計されています。これにより開発者は [IntelliSense](/visualstudio/ide/using-intellisense) サポートを使用して同じファイル内でマークアップと C# を切り替えることができます。 次のマークアップは、Razor ファイル (*DialogComponent.cshtml*) に含まれる基本的なカスタム ダイアログ コンポーネントの例です。

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

このコンポーネントをアプリ内の他の場所で使用する場合、IntelliSense の構文およびパラメーター補完により開発を迅速化できます。

コンポーネントに対しては次の処理が行えます。

* 入れ子にする。
* Razor (*\*.cshtml*) または C# (*\*.cs*) コードを使用して作成する。
* クラス ライブラリを介して共有する。
* ブラウザー DOM を必要とせずに単体テストを行う。

## <a name="infrastructure"></a>インフラストラクチャ

Razor Components には、ほとんどのアプリで必要となる次のコア機能が用意されています。

* レイアウト
* ルーティング
* 依存関係の挿入

これらの機能はすべて必要に応じて設定できます。 これらの機能のいずれか 1 つがアプリで使用されていない場合、実装は、[中間言語 (IL) リンカー](xref:host-and-deploy/razor-components/configure-linker)によって発行されたとき、アプリから除去されます。

## <a name="code-sharing-and-net-standard"></a>コードの共有と .NET Standard

アプリでは、既存の [.NET Standard](/dotnet/standard/net-standard) ライブラリを参照および使用することができます。 .NET Standard は、.NET 実装全体で共通した .NET API の標準仕様です。 .NET Standard 2.0 以降がサポートされています。 Web ブラウザー内で適用できない API (たとえば、ファイル システムにアクセスする機能、ソケットを開く機能、スレッド機能など) からは、[PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception) がスローされます。 .NET Standard クラス ライブラリは、サーバー コード全体で共有、およびブラウザー ベースのアプリ内で共有することができます。

## <a name="javascript-interop"></a>JavaScript 相互運用

サード パーティ製の JavaScript ライブラリおよびブラウザーの API を必要とするアプリのために、WebAssembly は JavaScript と相互運用できるように設計されています。 Razor Components では、JavaScript で使用できるライブラリまたは API はいずれも使用することができます。 C# コードによる JavaScript コードの呼び出し、および JavaScript コードによる C# コードの呼び出しを行うことができます。 詳細については、[JavaScript 相互運用](xref:razor-components/javascript-interop)に関するページを参照してください。

## <a name="optimization"></a>最適化

クライアント側アプリの場合は、ペイロードのサイズが重要です。 Blazor では、ダウンロード時間を短縮できるようにペイロードのサイズが最適化されます。 たとえば、ビルド プロセス中に .NET アセンブリの未使用の部分が削除され、HTTP 応答が圧縮され、.NET ランタイムとアセンブリがブラウザーでキャッシュされます。

Razor Components では、.NET アセンブリ、アプリのアセンブリ、およびランタイムをサーバー側で維持することにより、Blazor よりもさらにペイロードのサイズが縮小されます。 Razor Components アプリでは、クライアントに対してマークアップ、スクリプト、スタイル シートのみが提供されます。

## <a name="deployment"></a>配置

Blazor を使用すると、純粋なスタンドアロン クライアント側アプリをビルドすることも、サーバー アプリとクライアント アプリの両方を含むフル スタック ASP.NET Core アプリをビルドすることもできます。

* **クライアント側スタンドアロン アプリ**では、Blazor アプリは静的ファイルのみが格納される *dist* フォルダーにコンパイルされます。 Azure App Service、GitHub ページ、IIS (静的ファイル サーバーとして構成されている)、Node.js サーバーなど、さまざまなサーバーおよびサービス上で、それらのファイルをホストすることができます。 運用環境のサーバー上では、.NET は必要はありません。
* **フル スタック ASP.NET Core アプリ**では、サーバー アプリとクライアント アプリの間でコードを共有することができます。 結果として生成される ASP.NET Core Razor Components アプリ (クライアント側 UI とその他のサーバー側 API エンドポイントに対応する) をビルドし、それを、ASP.NET Core によってサポートされる任意のクラウドまたはオンプレミスのホストに展開することができます。

## <a name="suggest-a-feature-or-file-a-bug-report"></a>機能の提案またはバグ報告の提出

提案またはバグ報告の提出を行うには、[懸案事項を開いてください](https://github.com/aspnet/AspNetCore/issues/new)。 一般的なヘルプを取得する場合や、コミュニティから回答を得る場合は、[Gitter](https://gitter.im/aspnet/Blazor) での会話に参加してください。

## <a name="additional-resources"></a>その他の技術情報

* [WebAssembly](http://webassembly.org/)
* [C# のガイド](/dotnet/csharp/)
* [Razor](/aspnet/core/mvc/views/razor)
* [HTML](https://www.w3.org/html/)
