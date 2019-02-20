---
title: Blazor の概要
author: guardrex
description: WebAssembly を使ってブラウザー内で実行される対話型のクライアント側アプリを、 .NET を使って構築するための新しい方法である、ASP.NET Core Blazor について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: spa/blazor/index
---
# <a name="introduction-to-blazor"></a>Blazor の概要

作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor は、.NET を使って対話型のクライアント側 Web アプリを構築するための、シングルページ アプリ フレームワークです。 Blazor ではオープンな Web 標準が使われ、プラグインもコード トランスパイルも必要ありません。 Blazor はモバイル ブラウザーも含めて、すべての最新の Web ブラウザーで動作します。

クライアント側の Web 開発のためにブラウザー内で .NET を使うことには、多くの利点があります。

* **C# 言語**:JavaScript ではなく C# でコードを記述します。
* **.NET エコシステム**:.NET ライブラリの既存のエコシステムを活用します。
* **フルスタック開発**:サーバーとクライアント側のロジックを共有します。
* **スピードとスケーラビリティ**: NET はパフォーマンス、信頼性、セキュリティを重視して構築されました。
* **業界をリードするツール**: Windows、Linux、macOS 上の Visual Studio を使って生産性を維持します。
* **安定性と一貫性**:多機能で使いやすい安定した言語、フレームワーク、およびツールの共通セットに基づいて構築します。

[WebAssembly](http://webassembly.org) (略称 *wasm*) によって、Web ブラウザー内で .NET コードを実行することが可能になります。 WebAssembly はオープンな Web 標準であり、プラグインを使わずに Web ブラウザー内でサポートされます。 WebAssembly は、ダウンロードを高速化し実行速度を最大限に高めるために最適化されたコンパクトなバイトコード形式です。

WebAssembly コードを使用すると、JavaScript 相互運用を介してブラウザーのすべての機能にアクセスすることができます。 それと同時に、WebAssembly を介して実行される .NET コードは、クライアント コンピューター上での悪意のあるアクションを禁止するために JavaScript と同じ信頼されたサンドボックス内で実行されます。

![Blazor では、WebAssembly を使用してブラウザー内で .NET コードが実行されます。](index/_static/blazor.png)

Blazor アプリをビルドしてブラウザーで実行する場合: 

* C# コード ファイルと Razor ファイルが .NET アセンブリにコンパイルされます。
* そのアセンブリと .NET ランタイムがブラウザーにダウンロードされます。
* Blazor により .NET ランタイムがブートストラップされ、アプリのアセンブリを読み込むようにそのランタイムが構成されます。 ドキュメント オブジェクト モデル (DOM) 操作とブラウザー API の呼び出しは、JavaScript 相互運用を介して Blazor ランタイムによって処理されます。

Blazor は、ほとんどのアプリで必要となる次のコア機能をサポートしています。

* パラメーター
* イベント処理
* データ バインディング
* ルーティング
* 依存関係の挿入
* レイアウト
* テンプレート
* カスケード値

ダウンロードされるアプリのサイズを小さくするため、アプリが[中間言語 (IL) リンカー](xref:host-and-deploy/razor-components/configure-linker)によって発行されるときに、アプリから未使用コードが除去されます。

Blazor は、Razor Components 用のクライアント側のホスティング モデルです。 Razor Components ではコンポーネントのレンダリング ロジックが UI の更新の適用方法から切り離されているため、Razor Components をホストする方法には柔軟性があります。 ASP.NET Core Razor Component を使って、サーバー上の Razor Components を ASP.NET Core アプリでホストします。UI の更新は SignalR 接続を介して処理されます。 詳細については、「<xref:razor-components/hosting-models#server-side-hosting-model>」を参照してください。 

## <a name="components"></a>コンポーネント

*Razor Component* は、ページ、ダイアログ、データ入力フォームなど、UI の一部です。 コンポーネントではユーザー イベントが処理され、柔軟な UI のレンダリング ロジックが定義されます。 コンポーネントは、入れ子にしたり再利用したりできます。

コンポーネントは .NET アセンブリに組み込まれた .NET クラスであり、NuGet パッケージとして共有したり配布したりできます。 このクラスは、Razor マークアップ ページの形式 (*.cshtml*) で記述することも、C# クラス (*.cs*) として記述することもできます。

[Razor](xref:mvc/views/razor) とは、HTML マークアップを C# コードに結合するための構文です。 Razor は開発者の生産性向上を目的に設計されています。これにより開発者は [IntelliSense](/visualstudio/ide/using-intellisense) サポートを使用して同じファイル内でマークアップと C# を切り替えることができます。 Razor Pages と MVC ビューでも Razor が使われます。 要求/応答モデルを中心に構築される Razor Pages や MVC ビューとは異なり、コンポーネントは特に UI コンポジションを処理するために使われます。 Razor Components は特にクライアント側の UI ロジックとコンポジションのために使えます。

次のマークアップは、Razor ファイル (*DialogComponent.cshtml*) に含まれるカスタム ダイアログ コンポーネントの例です。

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

コンポーネントは、"*レンダリング ツリー*" と呼ばれるブラウザー DOM のメモリ内表現としてレンダリングされ、柔軟かつ効率的な方法で UI を更新するために使えるようになります。

## <a name="javascript-interop"></a>JavaScript 相互運用

サード パーティ製の JavaScript ライブラリおよびブラウザーの API を必要とするアプリのために、Blazor は JavaScript と相互運用します。 コンポーネントでは、JavaScript で使用できるライブラリまたは API はいずれも使用することができます。 C# コードによる JavaScript コードの呼び出し、および JavaScript コードによる C# コードの呼び出しを行うことができます。 詳細については、[JavaScript 相互運用](xref:razor-components/javascript-interop)に関するページを参照してください。

## <a name="code-sharing-and-net-standard"></a>コードの共有と .NET Standard

アプリでは、既存の [.NET Standard](/dotnet/standard/net-standard) ライブラリを参照および使用することができます。 .NET Standard は、.NET 実装全体で共通した .NET API の標準仕様です。 .NET Standard 2.0 以降がサポートされています。 Web ブラウザー内で適用できない API (たとえば、ファイル システムにアクセスする機能、ソケットを開く機能、スレッド機能など) からは、<xref:System.PlatformNotSupportedException> がスローされます。 .NET Standard クラス ライブラリは、サーバー コード全体で共有、およびブラウザー ベースのアプリ内で共有することができます。

## <a name="optimization"></a>最適化

ペイロードのサイズは、アプリの使用性に関する重要なパフォーマンス要因です。 Blazor では、ダウンロード時間を短縮するためにペイロードのサイズが最適化されます。

* .NET アセンブリの未使用部分がビルド処理中に削除されます。
* HTTP 応答が圧縮されます。
* .NET ランタイムとアセンブリがブラウザーにキャッシュされます。

[サーバー側の Razor Components](xref:razor-components/index) では、.NET アセンブリ、アプリのアセンブリ、およびランタイムをサーバー側で維持することにより、Blazor よりもさらにペイロードのサイズが縮小されます。 Razor Components アプリでは、マークアップ ファイルと静的資産のみがクライアントに提供されます。

## <a name="additional-resources"></a>その他の技術情報

* <xref:razor-components/index>
* [WebAssembly](http://webassembly.org/)
* [C# のガイド](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
