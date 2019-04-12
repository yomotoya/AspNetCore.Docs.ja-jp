---
title: Razor コンポーネントのホスティング モデル
author: guardrex
description: クライアント側 Blazor とサーバー側の ASP.NET Core で Razor コンポーネント ホスティング モデルを理解します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515650"
---
# <a name="razor-components-hosting-models"></a>Razor コンポーネントのホスティング モデル

によって[Daniel Roth](https://github.com/danroth27)

Razor のコンポーネントは web framework がクライアント側で実行するように設計上のブラウザーで、 [WebAssembly](http://webassembly.org/)-ベースの .NET ランタイム (*Blazor*) または ASP.NET Core でのサーバー側 (*ASP.NET Core の Razorコンポーネント*)。 ホスティング モデルでは、アプリおよびコンポーネントのモデルに関係なく*は同じまま*します。 この記事では、使用可能なホスティング モデルについて説明します。

* [クライアント側 Blazor](#client-side-hosting-model)
* [サーバー側 ASP.NET Core Razor Components](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a>クライアント側のホスティング モデル

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor のプリンシパルのホスティング モデルは、WebAssembly のブラウザーで実行されているクライアント側です。 Blazor アプリ、その依存関係、.NET ランタイムがブラウザーにダウンロードされます。 アプリがブラウザー UI スレッド上で直接実行されます。 UI の更新とイベントの処理は、同じプロセス内で発生します。 アプリの資産は、静的ファイルを web サーバーまたは静的なコンテンツをクライアントに提供できるサービスとしてデプロイされます。

![Blazor クライアント側:Blazor アプリは、ブラウザー内での UI スレッドで実行されます。](hosting-models/_static/client-side.png)

クライアント側のホスティング モデルを使用して Blazor アプリを作成するには、次のテンプレートのいずれかを使用します。

* **Blazor** ([dotnet の新しい blazor](/dotnet/core/tools/dotnet-new))&ndash;一連の静的ファイルとしてデプロイします。
* **(ASP.NET Core のホストされる) Blazor** ([dotnet の新しい blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; ASP.NET Core サーバーによってホストされています。 ASP.NET Core アプリでは、クライアントに Blazor アプリが機能します。 クライアント側の Blazor アプリは、web API の呼び出しを使用してネットワーク経由でサーバーと対話できるまたは[SignalR](xref:signalr/introduction)します。

テンプレートが含まれる、 *components.webassembly.js*を処理するスクリプト。

* .NET ランタイムで、アプリとアプリの依存関係をダウンロードしています。
* アプリを実行するランタイムを初期化します。

クライアント側のホスティング モデルでは、いくつかの利点を提供します。 クライアント側 Blazor:

* .NET サーバー側の依存関係がありません。
* クライアント リソースと機能を完全に活用します。
* オフロードは、サーバーからクライアントに機能します。
* オフライン シナリオをサポートしています。

クライアント側のホストには欠点があります。 クライアント側 Blazor:

* アプリをブラウザーの機能に制限されます。
* 対応クライアントのハードウェアとソフトウェア (たとえば、WebAssembly サポート) が必要です。
* より大きなダウンロード サイズと読み込み時間長くアプリ。
* .NET ランタイムおよびツールのサポートを成熟させる必要がある (の制限など、 [.NET Standard](/dotnet/standard/net-standard)サポートおよびデバッグ)。

## <a name="server-side-hosting-model"></a>サーバー側のホスティング モデル

ASP.NET Core の Razor コンポーネント サーバー側のホスティング モデルでは、アプリは、ASP.NET Core アプリ内からサーバーで実行されます。 UI の更新、イベント処理、および JavaScript の呼び出しが経由で処理を[SignalR](xref:signalr/introduction)接続します。

![ASP.NET Core Razor コンポーネント サーバー側:ブラウザーでは、SignalR 接続経由でサーバー (ASP.NET Core アプリの内部でホストされている) アプリと対話します。](hosting-models/_static/server-side.png)

サーバー側のホスティング モデルを使用して Razor コンポーネント アプリを作成するには、ASP.NET Core を使用して**Razor コンポーネント**テンプレート ([dotnet の新しい razorcomponents](/dotnet/core/tools/dotnet-new))。 ASP.NET Core アプリは、Razor コンポーネント サーバー側のアプリをホストし、クライアントが接続する SignalR エンドポイントを設定します。 ASP.NET Core アプリを参照して、アプリの`Startup`クラスを追加します。

* サーバー側コンポーネントの Razor サービス。
* 要求パイプラインを処理するアプリです。

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

*Components.server.js*スクリプト&dagger;クライアント接続を確立します。 永続化および (たとえば、失われたネットワーク接続の場合は) 発生時に必要なアプリの状態を復元するアプリの役目です。

サーバー側のホスティング モデルでは、いくつかの利点を提供します。 Razor のサーバー側コンポーネント:

* クライアント側の Blazor アプリよりも大幅に小さくアプリのサイズを持ち、はるかに高速に読み込みます。
* 含む任意の .NET Core の互換性のある Api を使用して、サーバー機能の完全な利用できます。
* 既存の .NET tooling、デバッグなどが期待どおりに動作するため、サーバー上で .NET Core で実行します。
* シン クライアントと連動 (たとえば、WebAssembly とリソースをサポートしないブラウザーがデバイスを制限する)。
* .NET/C#アプリのコンポーネントのコードを含む、コード ベースは、クライアントに提供されません。

サーバー側のホストには欠点があります。 Razor のサーバー側コンポーネント:

* 待機時間があります。すべてのユーザー操作には、ネットワーク ホップが必要があります。
* オフライン サポートを提供しません。クライアント接続に失敗した場合、アプリは稼働を停止します。
* スケーラビリティが低下します。サーバーは、複数のクライアント接続を管理し、クライアントの状態を処理する必要があります。
* アプリを処理するために、ASP.NET Core サーバーが必要です。 (たとえば、CDN) からサーバーなしの展開は、ことはできません。

&dagger;*Components.server.js*スクリプトは、次のパスに発行: *bin/{デバッグ |リリース}/{ターゲット フレームワーク}/publish/{アプリケーション名}。アプリ/dist/フレームワーク (_f)* します。
