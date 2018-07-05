---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: プロジェクト Katana の概要 |Microsoft Docs
author: howarddierking
description: 10 年以上にわたってきましたが、ASP.NET フレームワークとプラットフォームが無数の Web サイトとサービスの開発を有効にします。 Web アプリケーションとしています.
ms.author: aspnetcontent
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: a4c7d6cb5dc40ad68280a87acd89f60106a260a8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803095"
---
<a name="an-overview-of-project-katana"></a>プロジェクト Katana の概要
====================
によって[Howard dierking が](https://github.com/howarddierking)

> 10 年以上にわたってきましたが、ASP.NET フレームワークとプラットフォームが無数の Web サイトとサービスの開発を有効にします。 Web アプリケーションの開発戦略に進化してきましたが、フレームワークに ASP.NET MVC や ASP.NET Web API などのテクノロジの手順で展開できるされています。 プロジェクトのように Web アプリケーションの開発では、クラウド コンピューティングの世界に次の革新的な手順を受け取り、 [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)に柔軟で移植可能で、有効にすると、ASP.NET アプリケーション コンポーネントの基になるセットを提供します。軽量で、パフォーマンスの向上 – と別の方法で、プロジェクト[Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)クラウドは、ASP.NET アプリケーションを最適化します。


## <a name="why-katana--why-now"></a>なぜ Katana – なぜ今取り組むでしょうか。

 いずれかが開発者のフレームワークやエンドユーザーによる製品を説明するかどうかにかかわらずが基になる動機を作成するために重要です – 製品とその一部は、製品をに対して作成されたユーザーを知ることが含まれています。 ASP.NET は、2 人の顧客を念頭に最初に作成されました。   
  
**顧客の最初のグループは、クラシック ASP 開発者でした。** 時点では、ASP はマークアップとサーバー側スクリプトに組み込む、動的なデータ ドリブン Web サイトおよびアプリケーションを作成するための主要なテクノロジのいずれかでした。 ASP のランタイムは、基になる HTTP プロトコルと Web サーバーの主要な要素を抽象化され、キャッシュなどの追加へのアクセスは、このようなセッションとアプリケーションの状態管理をサービス提供するオブジェクトの一連のサーバー側スクリプトを指定します。強力ですが、従来の ASP アプリケーションはサイズと複雑さで遊んだりするように管理するの困難になりました。 これは、主にスクリプト環境のコードとマークアップのインターリーブによるコードの重複と組み合わせると、構造がないことによるものでした。 その課題の一部に対処するときに、従来の ASP の長所を活用大文字にする、するために ASP.NET を活かして、サーバー側のプログラミング モデルを維持しながら、.NET Framework のオブジェクト指向言語によって提供されるコードの編成どの classic ASP を開発者が使い慣れたまで増加しました。

**ASP.NET の対象顧客の 2 番目のグループには、Windows ビジネス アプリケーション開発者がでした。** 従来の ASP 開発者が HTML マークアップと複数の HTML マークアップを生成するコードを記述することに慣れていたのとは異なり (前に VB6 開発者) のような WinForms 開発者のキャンバスとユーザーの豊富なセットが含まれているデザイン時の操作に慣れていたインターフェイスを制御します。 ASP.NET – の最初のバージョンとも呼ばれます"Web Forms"提供されるユーザー インターフェイス コンポーネントのサーバー側のイベント モデルとインフラストラクチャの機能 (ViewState) などの一連のようなデザイン時のエクスペリエンス、シームレスな開発エクスペリエンスを作成するにはクライアントとサーバー側プログラミングの間 Web フォームは、WinForms 開発者がステートフルのイベント モデルでは、Web のステートレスな性質を効果的に hid します。

### <a name="challenges-raised-by-the-historical-model"></a>過去のモデルによって発生した問題

**最終的な結果は、成熟した、機能豊富なランタイムと開発者のプログラミング モデルでした。** ただし、その機能豊富な注目すべきいくつかの課題が付属しています。 最初に、フレームワークが**モノリシック**の論理的に個別の単位数を同じ System.Web.dll アセンブリ (たとえば、Web フォーム フレームワークを使用 core HTTP オブジェクト) に緊密に連携する機能。 次に、ASP.NET を大規模な .NET Framework の一部として含まれている、**年間の順序があったリリース間の時間。** これで、ASP.NET のすべての Web 開発を急速に進化で発生した変更のペースに困難になります。 最後に、System.Web.dll 自体が特定の Web ホスティングの選択肢をいくつかの異なる方法で結合された: インターネット インフォメーション サービス (IIS)。

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>革新的な手順: ASP.NET MVC と ASP.NET Web API

Web 開発で多数の変更が発生しています。 Web アプリケーションは、一連の小さい、大規模なフレームワークではなく、コンポーネントの焦点にますます開発されているされました。 これまで高速のレートで上昇を見せていた、リリースされた頻度だけでなく、コンポーネントの数。 Web に追従が大きいと複数の機能豊富ではなくしたがって小さく、分離されたより対象を絞ったを取得するフレームワークをしなければ明確だ、 **ASP.NET チームは時間のファミリとして ASP.NET を有効にするいくつかの革新的な手順1 つのフレームワークではなく、プラグ可能な Web コンポーネント**します。

初期の変更の 1 つが、Rails の Ruby などの Web 開発フレームワークに協力してくれた、よく知られているモデル-ビュー-コント ローラー (MVC) デザイン パターンの人気上昇します。 このスタイルの Web アプリケーションを構築する開発者に提供されましたが ASP.NET の初期のセールス ポイントのいずれかのマークアップとビジネス ロジックの分離を維持したまま、アプリケーションのマークアップを制御します。 このスタイルの Web アプリケーションの開発の需要を満たすためには、Microsoft が自身を配置することで将来ことをお勧め**帯域外の ASP.NET MVC の開発**(および、.NET Framework に含まれません)。 ASP.NET MVC は、個別のダウンロードとしてリリースされました。 これにより、エンジニア リング チームは柔軟になっていた、以前よりもより頻繁に更新プログラムを配信します。

Web アプリケーションの開発のもう 1 つの大きな変化が、サーバーによって生成される動的な Web ページからクライアント側スクリプトとの通信から生成されたページのセクションでは動的で静的な初期マークアップへの移行**バックエンド Web Api の使用AJAX 要求**します。 このアーキテクチャのシフトでは、Web Api の上昇と ASP.NET Web API フレームワークの開発を促進を支援します。 ASP.NET MVC、する場合などは、ASP.NET Web API のリリースには、ASP.NET のモジュール性の高いフレームワークとしてさらに進化する別の機会が用意されています。 営業案件のエンジニア リング チームが利用および**System.Web.dll で検出されたコア フレームワークの型のいずれかの依存関係があるないように、ASP.NET Web API をビルド**します。 これは、2 つの処理を有効になって: 最初に、そのため、自己完結型の方法で ASP.NET Web API の進化でした (および NuGet 経由で配信されるため、すばやく反復処理し続ける可能性があります)。 第 2 に、System.Web.dll、外部の依存関係と、そのため、IIS に依存関係のないがないため、ASP.NET Web API に含まれる (たとえばのコンソール アプリケーション、Windows サービスなど) のカスタム ホストで実行する機能

### <a name="the-future-a-nimble-framework"></a>今後: 敏捷性向上のフレームワーク

フレームワークを今すぐでしたから 1 つ別のフレームワークのコンポーネントを分離し、それらを NuGet でリリース、**より個別にかつより迅速に反復処理**します。 機能と Web API の自己ホスト機能の柔軟性のことを望む開発者にとって非常に魅力的なさらに、支障を**小型で軽量のホスト**サービス。 わかりました。 そのために魅力的な、実際には、その他のフレームワークは、この機能も必要およびという課題が表面化これで、各フレームワークを独自のベース アドレスで、独自のホスト プロセスで実行された、(開始、停止など) を管理するために必要とは独立しています。 モダン Web アプリケーションは、一般に、静的ファイル サービス、動的なページの生成、Web API、およびより最近リアルタイム-時間やプッシュ通知をサポートします。 これらの各サービスが実行し、は別に管理が必要ですが単に現実的です。

必要なものはさまざまなコンポーネントと、フレームワークのさまざまなアプリケーションを作成する開発者を有効にし、サポートのホストでそのアプリケーションを実行する単一のホスティング抽象化が発生しました。

## <a name="the-open-web-interface-for-net-owin"></a>オープンな Web Interface for .NET (OWIN)

 によって実現されるメリット触発[ラック](http://rack.github.io/)で Ruby のコミュニティでは、.NET コミュニティのメンバーがいくつか設定 Web サーバーおよびフレームワーク コンポーネント間の抽象化を作成します。 OWIN 抽象化のための 2 つの設計目標は、単純だし、他のフレームワーク型に最小限の可能な依存関係をかかったでした。 これら 2 つの目標はヘルプを確認します。

- 新しいコンポーネントがより簡単に開発し、使用します。
- ホストと可能性のある全体のプラットフォームまたはオペレーティング システムの間には、アプリケーションをより簡単に移植する可能性があります。

結果の抽象化は、2 つの主要な要素で構成されます。 最初は、環境のディクショナリです。 このデータ構造は、すべての HTTP 要求と応答、さらに、関連するサーバーの状態を処理するために必要な状態を格納する責任を負います。 環境のディクショナリの定義は次のとおりです。

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

OWIN と互換性のある Web サーバーは、本文ストリームと、HTTP 要求と応答のヘッダー コレクションなどのデータを使用して環境ディクショナリを設定します。 設定または追加の値のディクショナリを更新し、応答のボディ ストリームに書き込むアプリケーションまたはフレームワーク コンポーネントの責任です。

環境のディクショナリの種類を指定するだけでなく、OWIN 仕様は、core ディクショナリのキー値ペアのリストを定義します。 たとえば、次の表は、HTTP 要求の必要な dictionary のキーを示しています。

| キー名 | 値の説明 |
| --- | --- |
| `"owin.RequestBody"` | 存在する場合は、要求本文を含む Stream。 要求本文がない場合、Stream.Null はプレース ホルダーとして使用できます。 参照してください[要求本文](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics)します。 |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>`の要求ヘッダー。 参照してください[ヘッダー](http://owin.org/html/owin.html#3-3-headers)します。 |
| `"owin.RequestMethod"` | A`string`要求の HTTP 要求メソッドを格納している (例: `"GET"`、 `"POST"`)。 |
| `"owin.RequestPath"` | A`string`要求パスを格納しています。 「ルート」、アプリケーション デリゲートの相対パスでなければなりません参照してください[パス](http://owin.org/html/owin.html#5-3-paths)します。 |
| `"owin.RequestPathBase"` | A`string`を参照してください。 アプリケーション デリゲートの"root"に対応する要求パスの部分を含む[パス](http://owin.org/html/owin.html#5-3-paths)します。 |
| `"owin.RequestProtocol"` | A`string`プロトコルの名前とバージョンを格納している (例:`"HTTP/1.0"`または`"HTTP/1.1"`)。 |
| `"owin.RequestQueryString"` | A`string`先頭に追加しない HTTP 要求の URI のクエリ文字列のコンポーネントを含む"?"(例: `"foo=bar&baz=quux"`)。 空の文字列値があります。 |
| `"owin.RequestScheme"` | A`string`要求に使用される URI スキームを格納している (例: `"http"`、 `"https"`); を参照してください[URI スキーム](http://owin.org/html/owin.html#5-1-uri-scheme)します。 |

OWIN の 2 番目のキー要素は、アプリケーション デリゲートです。 これは、OWIN アプリケーションのすべてのコンポーネント間のプライマリ インターフェイスとして機能する関数のシグネチャです。 アプリケーション デリゲートの定義は次のとおりです。

`Func<IDictionary<string, object>, Task>;`

アプリケーション デリゲートでは、Func デリゲート型、関数が入力として環境のディクショナリを受け入れるし、タスクを返しますの実装では単にできるようになります。 この設計には、開発者向けのいくつかの影響があります。

- OWIN コンポーネントを作成するために必要な種類の依存関係の数が非常に少ないがあります。 これは、開発者に OWIN のアクセシビリティが大幅に向上します。
- 非同期デザインでは、コンピューティング リソース、特に多くの I/O 処理を要する操作の処理の効率を抽象化を使用できます。
- アプリケーション デリゲートは、実行のアトミック単位と OWIN コンポーネントを簡単に連結できるため、環境のディクショナリは、デリゲートのパラメーターとして送信されるが、ため、複雑な HTTP を処理パイプラインを作成するためです。

OWIN は、仕様、実装の観点からです ([http://owin.org/html/owin.html](http://owin.org/html/owin.html))。 その目標は、次の Web フレームワークが Web フレームワークと Web サーバーの対話方法に関する仕様ではなく、ありません。

調査した場合[OWIN](http://owin.org/)または[Katana](https://github.com/aspnet/AspNetKatana/wiki)、する可能性がありますもお気付き、 [Owin NuGet パッケージ](http://nuget.org/packages/Owin)Owin.dll とします。 このライブラリには、1 つのインターフェイスが含まれています。 [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)、形式化しで説明されているスタートアップ シーケンスを体系を[セクション 4](http://owin.org/html/owin.html#4-application-startup) OWIN 仕様の。 OWIN のサーバーを構築するためには必要ありません、 [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs)インターフェイスは、具象参照ポイントを提供し、Katana プロジェクト コンポーネントによって使用されます。

## <a name="project-katana"></a>プロジェクト Katana

両方、 [OWIN](http://owin.org/html/owin.html)仕様と*Owin.dll*所有コミュニティやコミュニティのオープン ソースの作業を実行するには、 [Katana](https://github.com/aspnet/AspNetKatana/wiki)プロジェクトは、OWIN のセットを表しますコンポーネントもオープン ソースの中をビルドして、Microsoft によってリリースします。 これらのコンポーネントはホストやサーバーなどのインフラストラクチャ コンポーネントと認証コンポーネントやフレームワークへのバインドなどの機能コンポーネントの両方をなど、 [SignalR](../../../signalr/index.md)と[ASP.NET WebAPI](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md)します。 プロジェクトには、次の 3 つの高レベル目標があります。 

- **ポータブル**– コンポーネントは、利用可能になる新しいコンポーネントを簡単に置換できる必要があります。 これには、すべての種類サーバーとホストするためにフレームワークからのコンポーネントにはが含まれます。 この目標は、Microsoft フレームワークがサード パーティのサーバーとホストで実行できる可能性があるときに、サード パーティ製フレームワークできますマイクロソフトのサーバーで実行シームレスにことです。
- **モジュール/柔軟な**– 小規模とフォーカスがある、にコンポーネントを判断することで、アプリケーション開発者にコントロールを与える無数の既定で有効にする機能を含むさまざまなフレームワークとは異なり Katana プロジェクト コンポーネントがあります自分のアプリケーションで使用します。
- **ライトウェイト/パフォーマンスの高い/スケーラブルな**– フレームワークの従来の概念の小さなセットに分割することにより追加される明示的にアプリケーションの開発者によって、結果として得られる Katana アプリケーションはより少ないコンピューティングを使用できるコンポーネントの重点を置いています。リソース、およびその結果、他のサーバーとフレームワークの型よりも、負荷の増加を処理します。 アプリケーションの要件を満たすには、基になるインフラストラクチャからより多くの機能が必要で、OWIN パイプラインに追加できるものが、アプリケーション開発者、明示的な意思決定する必要があります。 さらを可能になる、新しい高パフォーマンス サーバーにシームレスに導入できますそれらのアプリケーションを中断することがなく、OWIN アプリケーションのパフォーマンスを向上させるために下位レベルのコンポーネントの代替性を意味します。

## <a name="getting-started-with-katana-components"></a>Katana コンポーネントの概要

初めて導入された、1 つの側面、 [Node.js](http://nodejs.org/)のユーザーの注意をすぐに描画したフレームワークが、簡潔さを作成および Web サーバーを実行する 1 つでした。 Katana の目標は、light のフレーム化された場合[Node.js](http://nodejs.org/)を要約言っておきたい Katana に多くのメリットがもたらされているいずれかの可能性があります[Node.js](http://nodejs.org/) (と同じように、フレームワーク) スローする開発者を強制することがなくすべての ASP.NET Web アプリケーションの開発について知っています。 このステートメントが true を保持するには、Katana プロジェクトの概要は、均等に単純に本質的に[Node.js](http://nodejs.org/)します。

## <a name="creating-hello-world"></a>"Hello World!"を作成します。

JavaScript および .NET 開発の大きな違いの 1 つは、(が存在するか)、コンパイラのです。 そのため、単純な Katana サーバーの開始点は、Visual Studio プロジェクトです。 ただし、プロジェクトの種類の中で最も最小限で始めることができます。 空の ASP.NET Web アプリケーション。

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

次に、インストールしますが、 [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) NuGet パッケージをプロジェクトにします。 このパッケージは、ASP.NET の要求パイプラインで実行されている OWIN サーバーを提供します。 は、 [NuGet ギャラリー](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)と Visual Studio パッケージ マネージャー ダイアログまたはパッケージ マネージャー コンソールのいずれかを次のコマンドを使用してインストールできます。

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

インストール、`Microsoft.Owin.Host.SystemWeb`パッケージは依存関係としていくつか追加のパッケージをインストールします。 これらの依存関係の 1 つは`Microsoft.Owin`、いくつかのヘルパー型および OWIN アプリケーションを開発するためのメソッドを提供するライブラリ。 次のような"hello world"サーバーに迅速にこれらの型を使用できます。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

この非常に単純な Web サーバーは、Visual Studio を使用して実行するようになりましたことができます**F5**コマンドし、デバッグを完全にサポートが含まれています。

## <a name="switching-hosts"></a>ホストの切り替え

既定では、"hello world"の前の例は、IIS のコンテキストで System.Web を使用して、ASP.NET 要求パイプラインで実行されます。 これを単独で追加できます非常に高い価値の柔軟性と管理機能を OWIN パイプラインの構成可能性と IIS の全体的な成熟度を利用することができます。 ただし、IIS で提供される特典は必要ありませんより小さなより軽量なホストは、ある場合があります。 必要なものを次に、IIS や System.Web の外部で単純な Web サーバーを実行しますか。

移植性の目標を示すために、コマンド ライン ホストを Web サーバー ホストから移動、プロジェクトの出力フォルダーに新しいサーバーとホストの依存関係を追加するだけとホストを開始してが必要です。 この例では、Web サーバーと呼ばれる Katana ホストでホストされます`OwinHost.exe`Katana HttpListener ベースのサーバーを使用するとします。 同様に、他の Katana コンポーネントをこれらは、次のコマンドを使用して NuGet から取得したするは。

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

コマンドラインからプロジェクトのルート フォルダーに移動し、実行するだけです、 `OwinHost.exe` (がインストールされている各 NuGet パッケージの [ツール] フォルダーで)。 既定では、`OwinHost.exe`が HttpListener ベースのサーバーを探すように構成されているため、追加の構成は必要ありません。 Web ブラウザーでの移動`http://localhost:5000/`コンソールから実行されているアプリケーションを示しています。

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana のアーキテクチャ

 Katana のコンポーネント アーキテクチャは、次のようにアプリケーションを 4 つの論理層を分割:*ホスト、サーバー、ミドルウェア、* と*アプリケーション*します。 コンポーネントのアーキテクチャは、方法をこれらのレイヤーの実装に簡単に代わりに使用できる、多くの場合、アプリケーションの再コンパイルせずに考慮されます。   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>ホスト

 ホストが責任を負います。

- 基になるプロセスを管理します。
- ワークフローでは、どの要求を OWIN パイプラインの構築と、サーバーの選択、結果を調整することが処理されます。

  現時点では、Katana ベース アプリケーション用の 3 つのプライマリ ホスティング オプションがあります。  
  
**IIS/ASP.NET**: 標準の HttpModule と HttpHandler の種類を使用して、OWIN パイプラインで実行できます IIS、ASP.NET 要求フローの一部として。 ASP.NET ホストのサポートは、Web アプリケーション プロジェクトに Microsoft.AspNet.Host.SystemWeb NuGet パッケージのインストールで有効です。 さらに、IIS は、ホストとサーバーの両方として動作するための SystemWeb ホストを使用する場合、開発者によって、別のサーバー実装を置き換えることはできませんので、この NuGet パッケージで OWIN サーバー/ホストの違いが結び付いています。  
  
**カスタム ホスト**:、Katana コンポーネント スイートできるように、開発者が自分のカスタムのプロセスでアプリケーションをホストするコンソール アプリケーション、Windows サービスなどがあるかどうか。この機能は、Web API によって提供される自己ホスト機能に似ています。 次の例は、Web API コードのカスタム ホストを示しています。  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Katana アプリケーションの自己ホストのセットアップは似ています。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Web API と Katana の自己ホスト例の 1 つの大きな違いは、Web API の構成コードが Katana の自己ホスト例から不足していることです。 移植性と構成可能性の両方を有効にするためには、Katana は、要求処理パイプラインを構成するコードからサーバーを起動するコードを分離します。 Web API を構成するコードは、WebApplication.Start で型パラメーターとして指定されたさらに、スタートアップ クラスに含まれます。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

スタートアップ クラスについては、記事の後半で詳しく説明します。 ただし、コードは、自己ホスト プロセスは自己ホスト アプリケーションの ASP.NET Web API で今すぐ使用する場合のコードに非常に似て Katana を開始する必要があります。

**OwinHost.exe**: Katana Web アプリケーションを実行するカスタム プロセスを記述する必要があるいくつかが、中に多くを希望するサーバーを起動して、アプリケーションを実行できるビルド済み実行可能ファイルを起動するだけです。 このシナリオでは、Katana コンポーネントのスイートが含まれています`OwinHost.exe`します。 プロジェクトのルート ディレクトリ内から実行する、この実行可能ファイルは (サーバーを使用して、HttpListener 既定で) サーバーを起動し、規則を使用して検索し、ユーザーの startup クラスを実行します。 実行可能ファイルより細かな制御には、多数の追加のコマンド ライン パラメーターを提供します。

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>サーバー

 ホストが開始すると、プロセスをアプリケーションを実行するサーバーの役割を維持するネットワーク ソケットを開き、要求をリッスンして OWIN コンポーネントのパイプラインを使用して送信で指定された (とするユーザー既に気付いたかもしれませんが、このパイプラインは、アプリケーション開発者の Startup クラスで指定)。 現時点では、Katana プロジェクトには、2 つのサーバー実装が含まれています。 

- **Microsoft.Owin.Host.SystemWeb**: 既に触れましたが、ASP.NET パイプラインと連携して IIS がホストとサーバーの両方として機能します。 そのため、ホスト オプションこれを選択するときに IIS プロセスのアクティブ化などのホスト レベルの懸案事項の管理し、の両方が HTTP 要求をリッスンします。 ASP.NET Web アプリケーションの場合はその後、ASP.NET パイプラインに要求を送信します。 Katana SystemWeb ホストは、HTTP パイプラインを通過し、ユーザー指定の OWIN パイプラインを使用して送信要求をインターセプトするには、ASP.NET HttpModule と HttpHandler を登録します。
- **Microsoft.Owin.Host.HttpListener**: この Katana サーバーが、ソケットを開くし、開発者が指定した OWIN パイプラインに要求を送信する .NET Framework の HttpListener クラスを使用して名前が示すとおりです。 これは、現在、Katana 自己ホスト API と OwinHost.exe の既定のサーバーの選択です。

## <a name="middlewareframework"></a>ミドルウェア/フレームワーク

 前述のように、サーバーがクライアントからの要求を受け入れるときを担当の開発者のスタートアップ コードで指定された OWIN コンポーネントは、パイプラインを介して渡すことです。 これらのパイプライン コンポーネントは、ミドルウェアと呼ばれます。  
 非常に基本的なレベルで、OWIN ミドルウェア コンポーネントは呼び出しができるように、OWIN アプリケーション デリゲートを実装するだけで済みます。

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

ただし、開発とミドルウェア コンポーネントの構成を簡素化するために Katana をいくつかの規則とヘルパー型のサポートのミドルウェア コンポーネントします。 これらの最も一般的な状況は、`OwinMiddleware`クラス。 このクラスを使用して構築されたカスタムのミドルウェア コンポーネントは、次のようになります。 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 このクラスから派生`OwinMiddleware`、実装、コンス トラクターを引数の 1 つとして、パイプラインの次のミドルウェアのインスタンスを受け取り、基本コンス トラクターに渡されます。 ミドルウェアを構成するために使用する追加の引数は、次のミドルウェア パラメーターの後にもコンス トラクターのパラメーターとして宣言されます。   
  
ミドルウェアの実行時に、使用して、オーバーライドされた`Invoke`メソッド。 このメソッドは、型の 1 つの引数を受け取ります`OwinContext`します。 このコンテキスト オブジェクトは、によって提供される、 `Microsoft.Owin` NuGet パッケージを選択し、前に説明したいくつかの追加のヘルパー型と共に、要求、応答、および環境のディクショナリに厳密に型指定されたアクセスを提供します。   
  
ミドルウェア クラスに簡単に追加できますアプリケーションのスタートアップ コードで OWIN パイプラインにとおり。   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

ミドルウェア コンポーネントの単純なものから複雑性の範囲は Katana インフラストラクチャは、単に、OWIN ミドルウェア コンポーネントのパイプラインを構築し、単にコンポーネントをパイプラインに参加するアプリケーション デリゲートをサポートする必要があるため、ASP.NET Web API などの全体のフレームワークにロガーまたは[SignalR](../../../signalr/index.md)します。 たとえば、前の OWIN パイプラインへの ASP.NET Web API の追加には、次のスタートアップ コードを追加する必要があります。

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Katana インフラストラクチャでは、構成方法の IAppBuilder オブジェクトに追加された順序に基づくミドルウェア コンポーネントのパイプラインを作成します。 この例では、LoggerMiddleware 処理できる最終的にそれらの要求が処理される方法に関係なく、パイプラインを通過するすべての要求。 これにより、強力なシナリオのミドルウェア コンポーネント (例: 認証コンポーネント) が複数のコンポーネントやフレームワーク (ASP.NET Web API、SignalR、および静的なファイル サーバーなど) を含むパイプラインの要求を処理できます。
 
## <a name="applications"></a>アプリケーション

前の例に示すよう、OWIN と Katana プロジェクトいないと考えられますがの新しいアプリケーション プログラミング モデルはアプリケーション プログラミング モデルとサーバーとホスト インフラストラクチャからフレームワークを分離するために抽象化としてではなく。 たとえば、Web API アプリケーションを構築するときに開発者フレームワークは引き続き、Katana プロジェクトからコンポーネントを使用して、OWIN パイプラインでアプリケーションを実行するかどうかに関係なく、ASP.NET Web API フレームワークを使用します。 OWIN に関連するコードは、アプリケーション開発者に示される 1 つの場所が、開発者が、OWIN パイプラインを作成、アプリケーションのスタートアップ コードでになります。 スタートアップ コードで、開発者は、一連の受信要求を処理する各ミドルウェア コンポーネントのいずれかの一般的に UseXx ステートメントを登録します。 このエクスペリエンスは、System.Web の現在の世界では、HTTP モジュールを登録すると同じ効果があります。 通常より大きな framework などのミドルウェア、ASP.NET Web API または[SignalR](../../../signalr/index.md)パイプラインの最後に登録されます。 認証や、キャッシュなどの横断ミドルウェア コンポーネントは、フレームワークと、パイプラインの後半で登録されているコンポーネントのすべての要求を処理するように、通常、パイプラインの先頭に向かって登録されます。 この分離ミドルウェア コンポーネントの基になるインフラストラクチャ コンポーネントとの相互からは、システム全体の安定状態が保たを確保しながら、さまざまな速度で進化するコンポーネントを使用できます。

## <a name="components--nuget-packages"></a>コンポーネント – NuGet パッケージ

多数の現在のライブラリおよびフレームワークのように、Katana プロジェクト コンポーネントは、一連の NuGet パッケージとして配信されます。 今後のバージョン 2.0、Katana パッケージの依存関係グラフは次のとおりです。 (画像が拡大表示をクリックします。)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Katana プロジェクト内のほぼすべてのパッケージによって異なります、直接または間接的に、Owin パッケージ。 OWIN 仕様のセクション 4 で説明されているアプリケーションの起動処理の具象実装を提供する IAppBuilder インターフェイスが含まれるパッケージであること。 さらに、パッケージの多くは、HTTP 要求と応答を操作するための一連のヘルパー型を提供する Microsoft.Owin に依存します。 パッケージの残りの部分は、ホスティング インフラストラクチャ パッケージ (サーバーまたはホスト) またはミドルウェアのいずれかとして分類することができます。 パッケージと Katana プロジェクトの外部の依存関係は、オレンジ色で表示されます。

Katana 2.0 のホスティング インフラストラクチャの SystemWeb と HttpListener ベースのサーバー、OwinHost.exe を使用して、OWIN アプリケーションの実行に OwinHost パッケージと Microsoft.Owin.Hosting パッケージでの OWIN アプリケーションを自己ホストの両方を含む、カスタム ホスト (例: コンソール アプリケーション、Windows サービスなど)

Katana 2.0 のミドルウェア コンポーネントは主に異なる認証方法を提供します。 開始し、エラー ページのサポートを有効にする診断の 1 つの追加のミドルウェア コンポーネントが提供されます。 OWIN が事実上のホスティングの抽象化になるにつれて、ミドルウェア コンポーネントは、Microsoft とサード パーティによって開発されたもの両方のエコシステムも数に大きくなります。

## <a name="conclusion"></a>まとめ

 作成し、それによってにより開発者はさらに別の Web フレームワークについて説明しますが、の先頭から Katana プロジェクトの目標をされていません。 代わりに、目標は以前よりも可能な多くの選択肢を .NET Web アプリケーションの開発者に提供する抽象化を作成するようにしています。 置き換え可能なコンポーネントのセットに一般的な Web アプリケーション スタックの論理層を分割しては、Katana プロジェクトは、これらのコンポーネントの意味がどのような比率で向上させるために、スタック全体のコンポーネントを使用できます。 簡単な OWIN 抽象化の周りのすべてのコンポーネントを構築することにより Katana によりフレームワークおよびさまざまな別のサーバーとホスト間で移植できる基盤に構築されたアプリケーションを使用できます。 スタックのコントロールには、開発者を配置すること、Katana を行うことにより、開発者がどのように軽量の ultimate の選択を行ったことか、機能豊富などのように自分の Web スタックがする必要があります。  
  

## <a name="for-more-information-about-katana"></a>Katana の詳細については

- GitHub の Katana プロジェクト: [ https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/)します。
- ビデオ: [Katana プロジェクト - ASP.NET の OWIN](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET)、Howard dierking が、します。

## <a name="acknowledgements"></a>謝辞

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick は、Microsoft Azure と MVC に取り組んでいますのシニア プログラミング ライター。
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
