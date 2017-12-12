---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: "プロジェクトの Katana の概要 |Microsoft ドキュメント"
author: howarddierking
description: "ASP.NET フレームワークが経ちましたが 10 年以上にわたっており、プラットフォームには、無数の Web サイトとサービスの開発が有効にします。 Web アプリケーションとしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 8f28116f88f3cf5143d3d5c9821519d62c4e5452
ms.sourcegitcommit: 6541c8b11001dd617adf5eb04c814cda165070b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2017
---
<a name="an-overview-of-project-katana"></a>プロジェクトの Katana の概要
====================
によって[Howard Dierking](https://github.com/howarddierking)

> ASP.NET フレームワークが経ちましたが 10 年以上にわたっており、プラットフォームには、無数の Web サイトとサービスの開発が有効にします。 Web アプリケーションの開発戦略がきたように、フレームワークが同じ ASP.NET MVC、ASP.NET Web API などの技術の手順で進化することができました。 プロジェクトのように Web アプリケーションの開発では、クラウド コンピューティングの世界に次の革新的な手順を受け取り、 [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)柔軟性の高い、移植性をできるように、ASP.NET アプリケーション コンポーネントの基になるセットを提供します。軽量のパフォーマンスの向上: と言い換えると、プロジェクト[Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)クラウドは、ASP.NET アプリケーションを最適化します。


## <a name="why-katana--why-now"></a>なぜ Katana – 理由ようになりましたか?

 1 つが開発者のフレームワークまたはエンドユーザーの製品を説明するかどうかにかかわらず、することが重要を作成する基になる動機の製品とその一環製品用に作成されたユーザーを知ることが含まれています。 ASP.NET は、2 人の顧客を念頭に最初に作成されました。   
  
**顧客の最初のグループには、従来の ASP 開発者がでした。** 時点では、ASP は interweaving マークアップとサーバー側スクリプトによって、データ ドリブン動的な Web サイトおよびアプリケーションを作成するための主要なテクノロジの 1 つでした。 ASP ランタイムでは、基になる HTTP プロトコルと Web サーバーの主要な要素を抽象化され、キャッシュなどの追加へのアクセス サービスのようなセッションおよびアプリケーションの状態管理を提供するオブジェクトのセットをサーバー側スクリプトを指定します。強力ですが、従来の ASP アプリケーションはサイズおよび複雑さするにつれて、管理が容易になりました。 これは、主に構造ではスクリプト コード、およびマークアップのインターリーブされるコードの重複と組み合わせると環境での不足によるものでした。 ASP.NET の課題の一部に対処しながら、classic ASP の長所を活用、するために、サーバー側のプログラミング モデルを維持しながら、.NET Framework のオブジェクト指向言語によって提供されるコードの組織の利点がかかりましたどの classic ASP を開発者が使い慣れたまで増加しました。

**ASP.NET の対象顧客の 2 番目のグループには、Windows のビジネス アプリケーション開発者がでした。** 従来の ASP 開発者が HTML マークアップと複数の HTML マークアップを生成するコードを記述することに慣れて存在していたとは異なり WinForms 開発者 (前に VB6 開発者) のような日常的にデザイン時のエクスペリエンスのキャンバスと豊富なユーザーを含めることがインターフェイスを制御します。 ASP.NET – の最初のバージョンとも呼ばれます"Web Forms"提供およびサーバー側のイベント モデルのユーザー インターフェイス コンポーネントと一連の (ViewState) などのインフラストラクチャ機能のようなデザイン時のエクスペリエンスをシームレスな開発者エクスペリエンスを作成するにはクライアントとサーバー側プログラミングします。 Web フォームは、WinForms の開発者がステートフルなイベント モデルでは、Web のステートレスな性質を効果的に hid です。

### <a name="challenges-raised-by-the-historical-model"></a>履歴モデルによって発生した問題

**最終的な結果は、完成度の高い、豊富な機能のランタイムと開発者のプログラミング モデルはでした。** ただし、その機能豊富な機能がいくつかの重要な課題が付属しています。 まず、フレームワークが**モノリシック**System.Web.dll の同じアセンブリ (たとえば、Web フォームのフレームワークで HTTP 核となるオブジェクト) に密に結合されている機能の論理的に個別の単位にします。 次に、ASP.NET がいることを意図した大規模な .NET Framework の一部として含まれている、**リリースまでの時間は、年およそです。** これで、ASP.NET のすべての Web 開発を迅速に進化で行われている変更に対応するが困難です。 最後に、System.Web.dll 自体が特定の Web ホスト オプションに、いくつかの異なる方法で結合された: インターネット インフォメーション サービス (IIS)。

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>革新的な手順: ASP.NET MVC と ASP.NET Web API

Web 開発で多数の変更が行われています。 大規模なフレームワークではなく、コンポーネントが、一連の小さなに重点を置いて、web アプリケーションを開発中がますますです。 これらは、リリースされた頻度だけでなく、コンポーネントの数は、これまでに早くに増加してされました。 Web にペースのままの状態が必要になりますよりも大規模かつ複数の機能豊富ではなくしたがって小さく、分離されたより対象を絞ったを取得するフレームワークは明らかでした、 **ASP.NET チームかかりましたのファミリとして ASP.NET を有効にするいくつかの革新的な手順1 つのフレームワークではなく、プラグ可能な Web コンポーネント**です。

レールをよく知られているモデル-ビュー-コント ローラー (MVC) デザイン パターン Ruby などの Web 開発フレームワーク感謝の人気の増加は初期の変更の 1 つでした。 このスタイルの Web アプリケーションを構築した開発者は、ASP.NET の初期 selling ポイントのいずれかのマークアップとビジネス ロジックの分離を維持したまま、アプリケーションのマークアップをより細かく制御できます。 このスタイルの Web アプリケーションの開発の需要を満たすためには、Microsoft かかりました自体を配置することによって、今後の改善**帯域外の ASP.NET MVC の開発**(および、.NET Framework に含まれません)。 ASP.NET MVC は、個別のダウンロードとしてリリースされました。 これは、エンジニア リング チームに、これまで可能だったされていたよりもより頻繁に更新プログラムを配信する柔軟性を付けました。

Web アプリケーションの開発のもう 1 つの大きな変化が通信のクライアント側スクリプトから生成されたページのセクションでは動的に初期 static のマークアップをサーバーによって生成される動的な Web ページから、シフト**バックエンドの Web Api と使用AJAX 要求**です。 このアーキテクチャのシフトに Web Api の増加と ASP.NET Web API フレームワークの開発を促進役に立ちます。 ASP.NET MVC の場合は、ASP.NET Web API のリリースには、ASP.NET をモジュール化フレームワークとさらに発展する別の営業案件が用意されています。 営業案件のエンジニア リング チームが利用し、 **System.Web.dll 内で見つかった core framework 型のいずれかに依存関係があるないような ASP.NET Web API が組み込まれている**です。 これには、次の 2 つが有効にします。 最初に、そのためのものを ASP.NET Web API は、完全に独立的に進化でした (およびその NuGet 経由で配信されるため、すばやく反復処理を継続してでした)。 次が存在しなかったので、System.Web.dll に対する外部依存関係とそのため、IIS に依存関係のない、ASP.NET Web API 含まれているカスタムのホスト (たとえばのコンソール アプリケーション、Windows サービスなど) で実行する機能

### <a name="the-future-a-nimble-framework"></a>今後: 機敏フレームワーク

フレームワークを今すぐでした framework コンポーネントを互いから分離することと、NuGet でリリースする、**とは別より迅速に反復処理する**です。 機能と Web API の自己ホスト機能の柔軟性を抑えようとした開発者にとって非常に魅力的なさらに、証明、**小型、軽量のホスト**各自のサービスです。 ように魅力的なことがわかりました、実際には、他のフレームワークもこの機能により、あると、新しいチャレンジを表面化これで、各フレームワークを独自のベース アドレスで、独自のホスト プロセスで実行された、(開始、停止するなど) を管理するために必要とは独立しています。 一般に、最新の Web アプリケーションは、静的ファイル サービング、動的なページの生成、Web API、およびより最近リアルタイム-回/プッシュ通知をサポートします。 これらの各サービスを実行するは別に管理が必要でした単に現実的です。

さまざまなコンポーネントと、フレームワークのさまざまなアプリケーションを作成する開発者を有効にし、サポートするホスト上でそのアプリケーションを実行する単一のホスティング抽象化が必要だったしました。

## <a name="the-open-web-interface-for-net-owin"></a>For .NET (OWIN) 開いている Web インターフェイス

 によって実現されるメリットにインスパイアされた[ラック](http://rack.github.io/)Ruby のコミュニティでいくつかのメンバー .NET コミュニティ設定 Web サーバーおよびフレームワーク コンポーネント間の抽象化を作成します。 OWIN 抽象化のための 2 つの設計目標は、単純であったことと、他の framework 型の場合は、最小限の可能な依存関係にかかったでした。 これら 2 つの目標のヘルプを確認してください。

- 新しいコンポーネントをより簡単に開発され、利用可能性があります。
- アプリケーションは、ホストと全体の可能性のあるプラットフォームまたはオペレーティング システム間でより簡単に移植する可能性があります。

結果の抽象型は、次の 2 つの主要な要素で構成されます。 最初は、環境のディクショナリです。 このデータ構造体は、すべての HTTP 要求と応答、さらに、関連するサーバーの状態を処理するために必要な状態を格納します。 環境のディクショナリの定義は次のとおりです。

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

OWIN と互換性のある Web サーバーは、本文ストリームとの HTTP 要求と応答ヘッダーのコレクションなどのデータを使用して環境ディクショナリを作成します。 アプリケーションまたはフレームワーク コンポーネントを作成または追加の値のディクショナリを更新し、応答のボディ ストリームへの書き込みの役割です。

環境のディクショナリの種類を指定するだけでなく、OWIN 仕様はコア ディクショナリのキー値のペアのリストを定義します。 たとえば、次の表は、HTTP 要求に対する必要なディクショナリのキーを示しています。

| キー名 | 値の説明 |
| --- | --- |
| `"owin.RequestBody"` | 存在する場合、要求本文のストリーム。 要求本文が存在しない場合、Stream.Null はプレース ホルダーとして使用できます。 参照してください[要求本文](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics)です。 |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>`要求ヘッダー。 参照してください[ヘッダー](http://owin.org/html/owin.html#3-3-headers)です。 |
| `"owin.RequestMethod"` | A`string`要求の HTTP 要求メソッドを含む (例: `"GET"`、 `"POST"`)。 |
| `"owin.RequestPath"` | A`string`要求パスを含むです。 アプリケーションのデリゲートの「ルート」からの相対パスでなければなりません参照してください[パス](http://owin.org/html/owin.html#5-3-paths)です。 |
| `"owin.RequestPathBase"` | A`string`を参照してください。 アプリケーション デリゲートの"root"に対応する要求パスの部分を含む[パス](http://owin.org/html/owin.html#5-3-paths)です。 |
| `"owin.RequestProtocol"` | A`string`プロトコル名とバージョンを含む (例:`"HTTP/1.0"`または`"HTTP/1.1"`)。 |
| `"owin.RequestQueryString"` | A`string`クエリ文字列の部分を格納せず、先頭は、HTTP 要求の URI、"?"(例: `"foo=bar&baz=quux"`)。 値は、空の文字列を指定できます。 |
| `"owin.RequestScheme"` | A`string`要求で使用される URI スキームを格納している (たとえば、 `"http"`、 `"https"`); を参照してください[URI スキーム](http://owin.org/html/owin.html#5-1-uri-scheme)です。 |

OWIN の 2 番目のキー要素は、アプリケーション デリゲートです。 これは、OWIN アプリケーションのすべてのコンポーネント間の基本インターフェイスとして機能する関数のシグネチャです。 アプリケーションのデリゲートの定義は次のとおりです。

`Func<IDictionary<string, object>, Task>;`

アプリケーション デリゲートは、Func デリゲート型、関数が入力として環境ディクショナリを受け入れるし、タスクを返しますの実装だけです。 この設計では、開発者にいくつかの影響があります。

- OWIN コンポーネントを記述するために必要な種類の依存関係の数が非常に少ないがあります。 開発者に OWIN のユーザー補助機能が大幅に増加します。
- 非同期デザインでは、その処理がコンピューティング リソース、特に複数の I/O 処理を要する操作での効率的な抽象化を使用できます。
- アプリケーションのデリゲートは、実行のアトミック単位、OWIN コンポーネントを簡単にチェーンできるため、デリゲートのパラメーターとして、環境のディクショナリが実行される、ため組み合わせて複雑な HTTP のパイプライン処理を作成します。

実装の観点から、OWIN は仕様 ([http://owin.org/html/owin.html](http://owin.org/html/owin.html))。 その目的は次の Web フレームワークが Web フレームワークと Web サーバーの対話方法に関する仕様ではなくしないでください。

調査した場合[OWIN](http://owin.org/)または[Katana](https://github.com/aspnet/AspNetKatana/wiki)もお気付き、 [Owin NuGet パッケージ](http://nuget.org/packages/Owin)Owin.dll とします。 このライブラリには、1 つのインターフェイスが含まれています。 [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)、形式化しで説明、スタートアップ シーケンスを体系を[セクション 4](http://owin.org/html/owin.html#4-application-startup) OWIN 仕様のです。 OWIN サーバーを構築するためには必要ありません、 [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)インターフェイスは、具象参照ポイントを提供し、Katana プロジェクト コンポーネントによって使用されます。

## <a name="project-katana"></a>プロジェクトの Katana

一方両方、 [OWIN](http://owin.org/html/owin.html)仕様と*Owin.dll*が所有するコミュニティやコミュニティのオープン ソースの作業を実行、 [Katana](https://github.com/aspnet/AspNetKatana/wiki)プロジェクトは、OWIN のセットを表しますまだオープン ソースの中に構築され、マイクロソフトによってリリースされたコンポーネント。 これらのコンポーネントなどの認証コンポーネントやフレームワークへのバインドなどの機能のコンポーネントだけでなく、ホストやサーバーなどのインフラストラクチャ コンポーネント[SignalR](../../../signalr/index.md)と[ASP.NET WebAPI](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md)です。 プロジェクトには、次の 3 つの高レベル目標があります。 

- **ポータブル**– コンポーネントは、利用可能になる新しいコンポーネントを簡単に置換できる必要があります。 これには、すべての種類サーバーおよびホストするためにフレームワークからのコンポーネントにはが含まれます。 この目標のつまり Microsoft フレームワークが実際にサード パーティ製のサーバーおよびホストで実行中に、サード パーティ製フレームワークがマイクロソフトのサーバーで実行シームレスにことです。
- **モジュール/柔軟な**–、無数の既定で有効にする機能を含む多くのフレームワークとは異なり、Katana プロジェクト コンポーネントを小規模でフォーカスのある、経由で制御を行うコンポーネントを決定するとき、アプリケーション開発者にする必要があります自分のアプリケーションで使用します。
- **ライトウェイト/パフォーマンスの高い/スケーラブルな**– フレームワークの従来の概念の小さなのセットに分割することによりフォーカスが明示的に追加、アプリケーション開発者によってより少ない計算結果として得られる Katana アプリケーションを使用するコンポーネントリソース、およびその結果、他の種類のサーバー、およびフレームワークのよりも多くの負荷を処理します。 アプリケーションの要件は、基になるインフラストラクチャからより多くの機能を要求、ようものは、OWIN パイプラインに追加することができますが、アプリケーション開発者は、部分、明示的な意思決定をする必要があります。 さらに、下位レベルのコンポーネントの代替性は、利用可能になる、新しい高パフォーマンス サーバーにシームレスに導入できますそれらのアプリケーションを分断することがなく、OWIN アプリケーションのパフォーマンスを向上させるためを意味します。

## <a name="getting-started-with-katana-components"></a>Katana コンポーネントの概要

初めて導入された、1 つの要素、 [Node.js](http://nodejs.org/)すぐにユーザーの関心が集まりましたフレームワークが使用する 1 つでしたを作成および実行 Web サーバー、わかりやすくするためです。 Katana 目標が light のフレーム化される場合[Node.js](http://nodejs.org/)、いずれかの可能性がありますまとめたり Katana に多くのメリットがもたらされているを言うことにより[Node.js](http://nodejs.org/) (およびそのようなフレームワーク) スローする開発者を強制することがなくASP.NET Web アプリケーションの開発について知ってすべてのものです。 このステートメントは true を保持するには、Katana プロジェクトの使用を開始する必要があります性質を持つ実に単純な[Node.js](http://nodejs.org/)です。

## <a name="creating-hello-world"></a>"Hello World!"を作成します。

JavaScript および .NET の開発の大きな違いの 1 つは、(または存在しない) のコンパイラです。 そのため、単純な Katana サーバーの開始点は、Visual Studio プロジェクトです。 ただし、プロジェクトの種類の中で最も最小限に抑えて始めることができます: 空の ASP.NET Web アプリケーションです。

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

次をインストール、 [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) NuGet パッケージをプロジェクトにします。 このパッケージは、ASP.NET の要求パイプラインで実行する OWIN サーバーを提供します。 [NuGet ギャラリー](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)し、次のコマンドを使用して、Visual Studio パッケージ マネージャー ダイアログまたはパッケージ マネージャー コンソールを使用してインストールすることができます。

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

インストール、`Microsoft.Owin.Host.SystemWeb`パッケージの依存関係として、いくつかの他のパッケージでインストールされます。 これらの依存関係の 1 つは`Microsoft.Owin`ライブラリがいくつかのヘルパー型と OWIN アプリケーションを開発するためのメソッドを提供します。 次の"hello world"サーバーを簡単に作成、それらの型を使用できます。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

この非常に単純な Web サーバーは、Visual Studio を使用して実行するようになりましたことができます**f5 キーを押して**コマンドし、デバッグを完全にサポートが含まれています。

## <a name="switching-hosts"></a>ホストの切り替え

既定では、"hello world"の前の例は、IIS のコンテキストで System.Web を使用して ASP.NET 要求パイプラインで実行されます。 これを単独で追加できます膨大な価値に柔軟性と管理機能を OWIN パイプラインの構成可能性と IIS の成熟度の全体的なメリットを享受することができます。 ただし、IIS が提供する利点は必要ありませんより小さくより軽量のホストは、ある場合があります。 必要な情報、その後、IIS、System.Web の外部で単純な Web サーバーを実行しますか。

移植性の目標を示すためには、Web サーバーのホストとコマンド ライン ホストから移動、単に新しいサーバーとホストの依存関係をプロジェクトの出力フォルダーに追加し、ホストを起動が必要です。 この例ではという Katana ホストで、Web サーバーをホストお`OwinHost.exe`Katana HttpListener ベースのサーバーを使用しています。 同様に、他の Katana コンポーネントにこれらは獲得されます、次のコマンドを使用して NuGet から。

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

コマンドラインからプロジェクト ルート フォルダーに移動したりできます単に実行する、 `OwinHost.exe` (がインストールされている、それぞれの NuGet パッケージの [ツール] フォルダー内)。 既定では、`OwinHost.exe`が HttpListener ベースのサーバーを探すように構成されているため、追加の構成は必要ありません。 Web ブラウザー内を移動する`http://localhost:5000/`コンソールから実行されているアプリケーションを示しています。

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana アーキテクチャ

 Katana コンポーネントのアーキテクチャは、次のようにアプリケーションを次の 4 つの論理層を分割:*ホスト、サーバー、ミドルウェア、*と*アプリケーション*です。 コンポーネントのアーキテクチャは、これらのレイヤーの実装に簡単にで置換できる、多くの場合、アプリケーションの再コンパイルせずこのような方法で考慮されます。   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>ホスト

 ホストが担当します。

- 基になるプロセスを管理します。
- サーバーの選択や、どの要求を OWIN パイプラインの構築するためのワークフローの調整が処理されます。

 現時点では、Katana ベース アプリケーション用の 3 つの主要なホスティング オプションがあります。  
  
**IIS/ASP.NET**: 標準の HttpModule と HttpHandler 型を使用して、OWIN パイプラインで実行できます IIS、ASP.NET 要求フローの一部として。 Web アプリケーション プロジェクトに Microsoft.AspNet.Host.SystemWeb NuGet パッケージをインストールすることで、ASP.NET ホストのサポートが有効です。 さらに、IIS は、ホストとサーバーの両方として動作するため OWIN サーバー/ホスト上の違いは結び付いて SystemWeb ホストを使用する場合、開発者によって、代替サーバー実装置き換えることはできませんので、この NuGet パッケージにします。  
  
**カスタム ホスト**:「Katana コンポーネント スイートことができます開発者独自のカスタム プロセスでアプリケーションをホストするコンソール アプリケーション、Windows サービスなどであるどうか。この機能は、Web API によって提供される自己ホストの機能に似ています。 次の例は、Web API のコードのカスタム ホストを示しています。  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

自己ホスト アプリケーションのセットアップを Katana は似ています。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Web API と Katana の自己ホスト例の 1 つの大きな違いは、Web API の構成コードが Katana 自己ホスト例から不足していることです。 移植性と構成可能性の両方を有効にするためには、Katana は、要求処理パイプラインを構成するコードからサーバーを起動するコードを分離します。 Web API を構成し、スタートアップ時に、また WebApplication.Start の型パラメーターとして指定されたクラスに含まれているコード。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

スタートアップ クラスは、記事の後半で詳しく説明されます。 ただし、コードは、自己ホストのプロセスが ASP.NET Web API の自己ホスト アプリケーションで現在使用する場合のコードに非常に似て Katana を開始する必要があります。

**OwinHost.exe**: Katana Web アプリケーションを実行するカスタム プロセスを記述する必要があるいくつかが、多くは方がより適して単にサーバーを起動し、そのアプリケーションを実行できる構成済みの実行可能ファイルを起動します。 Katana コンポーネント スイートには、このシナリオでは、`OwinHost.exe`です。 実行からプロジェクトのルート ディレクトリ内で、この実行可能ファイルは (サーバーを使用して、HttpListener 既定で) サーバーを起動し、規則を使用して検索して、ユーザーのスタートアップ クラスを実行します。 詳細な制御、実行可能ファイルは、追加のコマンド ライン パラメーターの数を提供します。

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>サーバー

 起動して、アプリケーションが実行される、サーバーの責任において、プロセスを維持するため、ホストは、中にネットワーク ソケットを開き、要求をリッスンし、OWIN コンポーネントのパイプラインを使用して送信するユーザーが指定して、(としてします。既にお気付き、このパイプラインは、アプリケーション開発者のスタートアップ クラスで指定)。 現在、Katana プロジェクトには、次の 2 つのサーバーの実装が含まれます。 

- **Microsoft.Owin.Host.SystemWeb**: 既に触れましたが、IIS と ASP.NET パイプライン連携では、ホストと、サーバーの両方として機能します。 そのため、選択するときにこのホスト オプション、IIS プロセスのアクティブ化などのホスト レベルの懸案事項の管理し、両方 HTTP 要求をリッスンします。 ASP.NET Web アプリケーションの場合は、ASP.NET パイプラインに、要求を送信します。 Katana SystemWeb ホストは、HTTP パイプラインを通過し、ユーザー指定の OWIN パイプラインを使用して送信要求をインターセプトするには、ASP.NET HttpModule と HttpHandler を登録します。
- **Microsoft.Owin.Host.HttpListener**: この Katana サーバーで、.NET Framework の HttpListener クラスを使用して、ソケットを開くし、開発者が指定の OWIN パイプラインに要求を送信する名前が示すとおりです。 これは、現在、Katana 自己ホスト API と OwinHost.exe の両方の既定のサーバーの選択です。

## <a name="middlewareframework"></a>ミドルウェア/フレームワーク

 前述のように、サーバーが、クライアントからの要求を受け入れる場合は、開発者のスタートアップ コードで指定された OWIN コンポーネントのパイプラインを介して渡すことを担当します。 これらのパイプライン コンポーネントは、ミドルウェアと呼ばれます。  
 非常に基本的なレベルで OWIN ミドルウェア コンポーネントは呼び出しができるように、OWIN アプリケーションのデリゲートを実装するだけで済みます。

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

ただし、開発とミドルウェア コンポーネントの構成を簡略化するために Katana、いくつかの規則とヘルパー型のサポート ミドルウェア コンポーネントします。 これらの最も一般的な方法は、`OwinMiddleware`クラスです。 このクラスを使用して構築されたカスタム ミドルウェア コンポーネントは、次のようになります。 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 このクラスから派生`OwinMiddleware`、その引数の 1 つとして、パイプラインの次のミドルウェアのインスタンスを受け取り、基底コンス トラクターに渡しますをコンス トラクターを実装します。 ミドルウェアの構成に使用する追加の引数は、次のミドルウェア パラメーターは、後もコンス トラクターのパラメーターとして宣言されます。   
  
ミドルウェアが実行される、実行時に、オーバーライドされたを介して`Invoke`メソッドです。 このメソッドは、型の 1 つの引数を受け取ります`OwinContext`です。 このコンテキスト オブジェクトは、によって提供される、 `Microsoft.Owin` NuGet パッケージの前に説明したし、いくつかの追加のヘルパー型と共に、要求、応答、および環境の辞書に厳密に型指定されたアクセスを提供します。   
  
ミドルウェア クラスできます簡単にパイプラインに追加する、OWIN アプリケーションのスタートアップ コードで次のように。   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

ミドルウェア コンポーネントの単純なものからの複雑さの範囲は Katana インフラストラクチャは、単に OWIN ミドルウェア コンポーネントのパイプラインを構築し、単にコンポーネントをパイプラインに参加するアプリケーションのデリゲートをサポートする必要があるため、ASP.NET Web API などの全体のフレームワークにロガーまたは[SignalR](../../../signalr/index.md)です。 たとえば、前の OWIN パイプラインへの ASP.NET Web API の追加には、次のスタートアップ コードを追加することが必要です。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Katana インフラストラクチャでは、構成方法の IAppBuilder オブジェクトに追加された順序に基づいてミドルウェア コンポーネントのパイプラインを構築します。 例では、次に、LoggerMiddleware 扱える最終的にそれらの要求が処理される方法に関係なく、パイプラインを通過するすべての要求。 これは、ミドルウェア コンポーネント (例: 認証コンポーネント) が複数のコンポーネントとフレームワーク (ASP.NET Web API、SignalR、および静的なファイル サーバーなど) を含むパイプラインに対する要求を処理できる強力なシナリオを実現できます。
 
## <a name="applications"></a>アプリケーション

前の例で示すように OWIN と Katana プロジェクト見なす必要がありますいないのアプリケーション プログラミング モデルとサーバーとホスト インフラストラクチャからフレームワークを分離するための抽象化としてではなくが、新しいアプリケーション プログラミング モデルとして。 たとえば、Web API アプリケーションを構築するときに開発者 framework し続けます Katana プロジェクトからコンポーネントを使用する OWIN パイプラインでアプリケーションを実行するかどうかに関係なく、ASP.NET Web API フレームワークを使用します。 OWIN に関連するコードがアプリケーション開発者に表示される 1 つの場所にアプリケーションのスタートアップ コードがなる場合は、開発者が、OWIN パイプラインを構成します。 スタートアップ コードでは、開発者は、UseXx ステートメントは、受信要求を処理するミドルウェア コンポーネントごとに 1 つの系列を登録します。 このエクスペリエンスは、現在の System.Web 世界で HTTP モジュールを登録すると同じ効果があります。 通常より大きな framework ミドルウェア、ASP.NET Web API などまたは[SignalR](../../../signalr/index.md)パイプラインの最後に登録されます。 認証またはキャッシュなどの広範囲にミドルウェア コンポーネントは通常、登録、パイプラインの先頭までの範囲のフレームワークと、パイプラインで後で登録されているコンポーネントのすべての要求を処理します。 この分離ミドルウェア コンポーネントの基になるインフラストラクチャ コンポーネントとの相互からは、全体的なシステムを使ったまま、安定したことを確保しながら、さまざまな速度で進化するコンポーネントを使用できます。

## <a name="components--nuget-packages"></a>コンポーネント – NuGet パッケージの管理

現在のライブラリやフレームワークの多くのように Katana プロジェクト コンポーネントは、一連の NuGet パッケージとして配布されます。 今後のバージョン 2.0 の Katana パッケージの依存関係グラフ次のとおりです。 (画像が拡大表示をクリックします。)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Katana プロジェクトのほぼすべてのパッケージによって異なります、直接または間接的に Owin パッケージ。 OWIN 仕様のセクション 4 で説明したアプリケーションの起動処理の具象実装を提供する IAppBuilder インターフェイスが含まれるパッケージは、このことに留意することがあります。 さらに、パッケージの多くは、HTTP 要求と応答を操作するためのヘルパー型のセットを提供する Microsoft.Owin に依存します。 パッケージの残りの部分は、ホスティング インフラストラクチャ パッケージ (サーバーまたはホスト) またはミドルウェアのいずれかとして分類されることができます。 パッケージと Katana プロジェクト外部の依存関係がオレンジ色で表示されます。

Katana 2.0 のホスティング インフラストラクチャ、SystemWeb と HttpListener ベースのサーバー、OwinHost.exe を使用する OWIN アプリケーションを実行するため、OwinHost パッケージ、および自己ホスト型の OWIN アプリケーションの Microsoft.Owin.Hosting パッケージ、カスタム ホスト (例: コンソール アプリケーション、Windows サービスなど)

Katana 2.0 のミドルウェア コンポーネント、主に注目している異なる認証方法を提供することにします。 開始し、エラー ページのサポートを有効な診断に 1 つの他のミドルウェア コンポーネントを説明します。 OWIN ホスティング事実上の抽象化に増大すると、Microsoft とサード パーティによって開発されたもの両方ミドルウェア コンポーネントのエコシステムも数に拡張されます。

## <a name="conclusion"></a>まとめ

 先頭から Katana プロジェクトの目標を作成し、それによってさらに別の Web フレームワークを学習する開発者を強制されていません。 代わりに、目標は、以前よりも考えられる複数の選択肢を .NET Web アプリケーションの開発者に提供する抽象化を行うされました。 通常 Web アプリケーションのスタックの置き換え可能コンポーネントのセットに論理層を分割は、Katana プロジェクトは、どのような比率にはそれらのコンポーネントが合理的に向上させるためにスタック全体でコンポーネントを使用できます。 単純な OWIN 抽象化の周りのすべてのコンポーネントを構築するには、Katana ことによって、フレームワーク上に構築されたアプリケーションをさまざまな別のサーバーとホスト間で移植可能にします。 開発者は、スタックのコントロールに配置すること、Katana を行うことにより、開発者が軽量な方法に関する最終的な選択を行うことまたは機能が豊富でどのように自分の Web スタックがする必要があります。  
  

## <a name="for-more-information-about-katana"></a>Katana の詳細については

- GitHub の Katana プロジェクト: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/)です。
- ビデオ: [Katana プロジェクト - ASP.NET の OWIN](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)、Howard Dierking でします。

## <a name="acknowledgements"></a>謝辞

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick は Microsoft Azure と MVC に焦点を当てたのスキルの限られたプログラミング ライター。
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
