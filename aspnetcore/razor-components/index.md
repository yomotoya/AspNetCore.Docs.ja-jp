---
title: Razor Components の概要
author: guardrex
description: ASP.NET Core アプリ内に .NET を使った対話型のクライアント側 Web UI を構築する方法である、ASP.NET Core Razor Components について調べます。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/index
---
# <a name="introduction-to-razor-components"></a>Razor Components の概要

作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)

*Razor Components* は、.NET を使った対話型のクライアント側 Web UI を構築するための方法です。

* JavaScript の代わりに C# を使って、多機能な対話型 UI を構築します。
* すべて .NET で記述された、サーバー側とクライアント側アプリのロジックを共有します。
* モバイル ブラウザーを含めた広範なブラウザーのサポートのために、HTML および CSS として UI をレンダリングします。

Razor Components は、ほとんどのアプリで必要となる次のコア機能をサポートしています。

* パラメーター
* イベント処理
* データ バインディング
* ルーティング
* 依存関係の挿入
* レイアウト
* テンプレート
* カスケード値

Razor Components では、UI の更新を適用する方法からコンポーネントのレンダリング ロジックが分離されます。 .NET Core 3.0 の ASP.NET Core Razor Component には、ASP.NET Core アプリでサーバー上の Razor Components をホストするためのサポートが追加されています。 UI の更新は SignalR 接続を介して処理されます。

ランタイムは:

* ブラウザーからサーバーへの UI イベントの送信を処理します。
* サーバーから送信された UI の更新を、コンポーネントの実行後にブラウザーに適用します。

ブラウザーと通信するために Razor Components で使われる接続は、JavaScript 相互運用の呼び出しを処理するためにも使われます。

![Razor Components では、サーバー上で .NET コードが実行され、SignalR 接続を介してクライアント上のドキュメント オブジェクト モデルとのやりとりが行われます。](index/_static/aspnet-core-razor-components.png)

詳細については、「<xref:razor-components/hosting-models#server-side-hosting-model>」を参照してください。

*Blazor* は、Razor Components の実験的なクライアント側のホスティング モデルです。 Blazor は、オープンな Web 標準を使ってブラウザー内の .NET 上で実行され、プラグインもコード トランスパイルも必要ありません。 詳細については、次のトピックを参照してください。 <xref:spa/blazor/index> および <xref:razor-components/hosting-models#client-side-hosting-model>

## <a name="components"></a>コンポーネント

*Razor Component* は、ページ、ダイアログ、データ入力フォームなど、UI の一部です。 コンポーネントではユーザー イベントが処理され、柔軟な UI のレンダリング ロジックが定義されます。 コンポーネントは、入れ子にしたり再利用したりできます。

コンポーネントは .NET アセンブリに組み込まれた .NET クラスであり、NuGet パッケージとして共有したり配布したりできます。 クラスは通常、Razor マークアップ ページの形式で、*.razor* ファイル拡張子と共に記述します。

[Razor](xref:mvc/views/razor) とは、HTML マークアップを C# コードに結合するための構文です。 Razor は開発者の生産性向上を目的に設計されています。これにより開発者は [IntelliSense](/visualstudio/ide/using-intellisense) サポートを使用して同じファイル内でマークアップと C# を切り替えることができます。 Razor Pages と MVC ビューでも Razor が使われます。 要求/応答モデルを中心に構築される Razor Pages や MVC ビューとは異なり、コンポーネントは特に UI コンポジションを処理するために使われます。 Razor Components は特にクライアント側の UI ロジックとコンポジションのために使えます。

次のマークアップは、Razor ファイルに含まれるカスタム ダイアログ コンポーネントの例です (*DialogComponent.razor*)。

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

サード パーティ製の JavaScript ライブラリおよびブラウザーの API を必要とするアプリのために、コンポーネントは JavaScript と相互運用します。 コンポーネントでは、JavaScript で使用できるライブラリまたは API はいずれも使用することができます。 C# コードによる JavaScript コードの呼び出し、および JavaScript コードによる C# コードの呼び出しを行うことができます。 詳細については、[JavaScript 相互運用](xref:razor-components/javascript-interop)に関するページを参照してください。

## <a name="additional-resources"></a>その他の技術情報

* <xref:spa/blazor/index>
* [WebAssembly](http://webassembly.org/)
* [C# のガイド](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
