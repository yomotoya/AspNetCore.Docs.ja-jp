---
title: ASP.NET Core での Blazor の概要
author: guardrex
description: ASP.NET Core アプリ内に .NET を使った対話型のクライアント側 Web UI を構築する方法である、ASP.NET Core Blazor について調べます。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982998"
---
# <a name="introduction-to-blazor"></a>Blazor の概要

作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)

Blazor にようこそ!

.NET を使った対話型のクライアント側 Web UI を構築します。

* JavaScript の代わりに C# を使って、多機能な対話型 UI を構築します。
* .NET で記述された、サーバー側とクライアント側のアプリのロジックを共有します。
* モバイル ブラウザーを含めた広範なブラウザーのサポートのために、HTML および CSS として UI をレンダリングします。

Blazor は、ほとんどのアプリで必要となる次のコア シナリオをサポートしています。

* パラメーター
* イベント処理
* データ バインディング
* ルーティング
* 依存関係の挿入
* レイアウト
* テンプレート
* カスケード値

## <a name="components"></a>コンポーネント

Blazor の*コンポーネント*とはページ、ダイアログ、データ エントリ フォームなど UI の要素です。 コンポーネントではユーザー イベントが処理され、柔軟な UI のレンダリング ロジックが定義されます。 コンポーネントは、入れ子にしたり再利用したりできます。

コンポーネントは .NET アセンブリに組み込まれた .NET クラスであり、NuGet パッケージとして共有したり配布したりできます。 コンポーネント クラスは通常、Razor マークアップ ページの形式で、*.razor* ファイル拡張子で記述されます。 Blazor のコンポーネントは、Razor コンポーネントと呼ばれることもあります。

[Razor](xref:mvc/views/razor) とは、HTML マークアップを C# コードに結合するための構文です。 Razor は開発者の生産性向上を目的に設計されています。これにより開発者は [IntelliSense](/visualstudio/ide/using-intellisense) サポートを使用して同じファイル内でマークアップと C# を切り替えることができます。 Razor Pages と MVC ビューでも Razor が使われます。 要求/応答モデルを中心に構築される Razor Pages や MVC ビューとは異なり、コンポーネントは特に UI コンポジションを処理するために使われます。 Razor コンポーネントは特にクライアント側の UI ロジックとコンポジションのために使えます。

次のマークアップは、カスタム ダイアログ コンポーネントの例です。

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

このコンポーネントをアプリ内の他の場所で使用すると、[Visual Studio](https://visualstudio.microsoft.com/vs/) の IntelliSense の構文およびパラメーター補完により開発を迅速化できます。

コンポーネントは、"*レンダリング ツリー*" と呼ばれるブラウザー DOM のメモリ内表現としてレンダリングされ、柔軟かつ効率的な方法で UI を更新するために使えるようになります。

## <a name="blazor-server-side"></a>サーバー側 Blazor

Blazor では、UI の更新プログラムを適用する方法からコンポーネントのレンダリング ロジックが分離されます。 サーバー側 Blazor では、ASP.NET Core アプリでサーバー上の Razor コンポーネントをホストするためのサポートが提供されます。 UI の更新は SignalR 接続を介して処理されます。

ランタイムは:

* ブラウザーからサーバーへの UI イベントの送信を処理します。
* サーバーから送信された UI の更新を、コンポーネントの実行後にブラウザーに適用します。

ブラウザーと通信するために Blazor のサーバー側で使われる接続は、JavaScript 相互運用の呼び出しを処理するためにも使われます。

![サーバー側 Blazor では、サーバー上で .NET コードが実行され、SignalR 接続を介してクライアント上のドキュメント オブジェクト モデルとのやりとりが行われます](index/_static/blazor-server-side.png)

詳細については、「<xref:blazor/hosting-models#server-side>」を参照してください。

## <a name="blazor-client-side"></a>クライアント側 Blazor

クライアント側 Blazor は、.NET を使って対話型のクライアント側 Web アプリを構築するための、シングルページ アプリ フレームワークです。 クライアント側 Blazor ではオープンな Web 標準が使われ、プラグインもコード トランスパイルも必要ありません。 クライアント側 Blazor はモバイル ブラウザーも含めて、すべての最新の Web ブラウザーで動作します。

クライアント側の Web 開発のためにブラウザー内で .NET を使うことには、多くの利点があります。

* **C# 言語**:JavaScript ではなく C# でコードを記述します。
* **.NET エコシステム**:.NET ライブラリの既存のエコシステムを活用します。
* **フルスタック開発**:サーバーとクライアント側のロジックを共有します。
* **スピードとスケーラビリティ**: NET はパフォーマンス、信頼性、セキュリティを重視して構築されました。
* **業界をリードするツール**: Windows、Linux、macOS 上の Visual Studio を使って生産性を維持します。
* **安定性と一貫性**:多機能で使いやすい安定した言語、フレームワーク、およびツールの共通セットに基づいて構築します。

[WebAssembly](http://webassembly.org) (略称 *wasm*) によって、Web ブラウザー内で .NET コードを実行することが可能になります。 WebAssembly はオープンな Web 標準であり、プラグインを使わずに Web ブラウザー内でサポートされます。 WebAssembly は、ダウンロードを高速化し実行速度を最大限に高めるために最適化されたコンパクトなバイトコード形式です。

WebAssembly コードを使用すると、JavaScript 相互運用を介してブラウザーのすべての機能にアクセスすることができます。 それと同時に、WebAssembly を介して実行される .NET コードは、クライアント コンピューター上での悪意のあるアクションを禁止するために JavaScript と同じ信頼されたサンドボックス内で実行されます。

![クライアント側 Blazor は WebAssembly を使用してブラウザーで .NET コードを実行します。](index/_static/blazor-client-side.png)

クライアント側 Blazor アプリをビルドしてブラウザーで実行する場合: 

* C# コード ファイルと Razor ファイルが .NET アセンブリにコンパイルされます。
* そのアセンブリと .NET ランタイムがブラウザーにダウンロードされます。
* クライアント側 Blazor により .NET ランタイムがブートストラップされ、アプリのアセンブリを読み込むようにそのランタイムが構成されます。 ドキュメント オブジェクト モデル (DOM) 操作とブラウザー API の呼び出しは、JavaScript 相互運用を介してクライアント側 Blazor ランタイムによって処理されます。

ダウンロードされるアプリのサイズを小さくするため、アプリが[中間言語 (IL) リンカー](xref:host-and-deploy/blazor/configure-linker)によって発行されるときに、アプリから未使用コードが除去されます。

Blazor のクライアント側は、クライアント側のホスティング モデルです。 Blazor ではコンポーネントのレンダリング ロジックが UI の更新の適用方法から切り離されているため、Blazor をホストする方法には柔軟性があります。 [クライアント側 Blazor](#blazor-server-side) を使って、サーバー上の Blazor を ASP.NET Core アプリでホストします。UI の更新は SignalR 接続を介して処理されます。 詳細については、「<xref:blazor/hosting-models#server-side>」を参照してください。 

ペイロードのサイズは、アプリの使用性に関する重要なパフォーマンス要因です。 クライアント側 Blazor では、ダウンロード時間を短縮するためにペイロードのサイズが最適化されます。

* .NET アセンブリの未使用部分がビルド処理中に削除されます。
* HTTP 応答が圧縮されます。
* .NET ランタイムとアセンブリがブラウザーにキャッシュされます。

[サーバー側 Blazor](#blazor-server-side) では、.NET アセンブリ、アプリのアセンブリ、およびランタイムをサーバー側で維持することにより、クライアント側 Blazor よりもペイロードのサイズが縮小されます。 サーバー側 Blazor アプリでは、マークアップ ファイルと静的資産のみがクライアントに提供されます。

## <a name="javascript-interop"></a>JavaScript 相互運用

サード パーティ製の JavaScript ライブラリおよびブラウザーの API を必要とするアプリのために、コンポーネントは JavaScript と相互運用します。 コンポーネントでは、JavaScript で使用できるライブラリまたは API はいずれも使用することができます。 C# コードによる JavaScript コードの呼び出し、および JavaScript コードによる C# コードの呼び出しを行うことができます。 詳細については、[JavaScript 相互運用](xref:blazor/javascript-interop)に関するページを参照してください。

## <a name="code-sharing-and-net-standard"></a>コードの共有と .NET Standard

アプリでは、既存の [.NET Standard](/dotnet/standard/net-standard) ライブラリを参照および使用することができます。 .NET Standard は、.NET 実装全体で共通した .NET API の標準仕様です。 Blazor では .NET Standard 2.0 が実装されます。 Web ブラウザー内で適用できない API (たとえば、ファイル システムにアクセスする機能、ソケットを開く機能、スレッド機能など) からは、<xref:System.PlatformNotSupportedException> がスローされます。 .NET Standard のクラス ライブラリは、Blazor、.NET Framework、.NET Core、Xamarin、Mono、Unity など、さまざまな .NET プラットフォーム全体で共有することができます。

## <a name="additional-resources"></a>その他の技術情報

* [WebAssembly](http://webassembly.org/)
* [C# のガイド](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
