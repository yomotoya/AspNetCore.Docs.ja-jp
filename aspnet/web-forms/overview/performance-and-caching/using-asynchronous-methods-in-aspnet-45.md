---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: ASP.NET 4.5 で非同期メソッドの使用 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Web は、無料の Visual Studio Express 2012 を使用して非同期の ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: eeb8ac4402b5e3d233082a749ad16ed98d4a71fc
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577809"
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>ASP.NET 4.5 で非同期メソッドの使用
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> このチュートリアルが非同期 ASP.NET Web フォームを使用するアプリケーションの構築の基礎を講義[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11)、これは Microsoft Visual Studio の無料バージョンです。 使用することも[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)します。 このチュートリアルでは、次のセクションが含まれます。
> 
> - [スレッド プール内に要求を処理する方法](#HowRequestsProcessedByTP)
> - [同期または非同期のメソッドを選択します。](#ChoosingSyncVasync)
> - [サンプル アプリケーション](#SampleApp)
> - [ギズモ同期ページ](#GizmosSynch)
> - [非同期ギズモ ページの作成](#CreatingAsynchGizmos)
> - [複数の操作を並列で実行します。](#Parallel)
> - [キャンセル トークンを使用します。](#CancelToken)
> - [高い同時実行/高待機時間の Web サービス呼び出しのサーバー構成](#ServerConfig)
> 
> このチュートリアルの完全なサンプルが提供されます。  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) [GitHub](https://github.com/)サイト。


ASP.NET 4.5 Web ページの組み合わせで[.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx)型のオブジェクトを返す非同期メソッドを登録することができます[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)します。 .NET Framework 4 と呼ばれる非同期のプログラミング概念を導入する、[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)ASP.NET 4.5 をサポートし、[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)します。 タスクがによって表される、**タスク**型と関連する型を[System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx)名前空間。 この非同期サポートに基づいて、.NET Framework 4.5、 [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)と[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)操作できるようにするキーワード[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)前よりもはるかに単純なオブジェクト非同期のアプローチです。 [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)キーワードは簡略記法を示すためのコードがコードの他のいくつかの一部で非同期的に待機します。 [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードは、タスク ベースの非同期メソッドとしてメソッドをマークするために使用できるヒントを表します。 組み合わせ**await**、 **async**、および**タスク**オブジェクトにより、.NET 4.5 で非同期コードを記述するためのはるかに簡単です。 非同期メソッドの場合、新しいモデルと呼ばれる、*タスクベースの非同期パターン*(**タップ**)。 このチュートリアルは、非同期プログラミングを使用して知識があることを前提[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)と[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードと[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)名前空間。

使用して、詳細については[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)と[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードと[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)名前空間には、次の参照を参照してください。

- [.NET での非同期のホワイト ペーパー:](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Async/await のよく寄せられる質問](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio の非同期プログラミング](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  スレッド プール内に要求を処理する方法

Web サーバーでは、.NET Framework は、ASP.NET 要求をサービスに使用されるスレッドのプールを保持します。 要求が到着すると、その要求を処理するプールのスレッドがディスパッチされます。 要求が同期的に処理される場合、要求を処理するスレッドは、要求の中でビジー状態が処理されると、スレッドが別の要求をサービスことはできませんが。   
  
これは、ありますの問題をスレッド プールに多くのビジー状態スレッド数を十分に格納できるためです。 ただし、スレッド プール内のスレッドの数は制限されています (既定の .NET 4.5 の最大値は 5,000)。 実行時間の長い要求の高度に並列で大規模なアプリケーションで利用可能なすべてのスレッドはビジー状態にあります。 この条件は、スレッドの不足と呼ばれます。 この条件に達すると、web サーバーは要求をキューします。 要求キューがいっぱいになると、web サーバーは HTTP 503 ステータス (サーバーがビジー状態です) で要求を拒否します。 CLR スレッド プールでは、新しいスレッドのインジェクションの制限があります。 同時実行を集中的な状態にした場合 (つまり、web サイトが突然を取得できます要求の数が多い) と使用可能な要求のすべてのスレッドがビジー状態バックエンドの呼び出しにより高待機時間で、限られたスレッドの挿入率が非常にうまく対応アプリケーションを作成できます。 また、スレッド プールに追加された新しい各スレッドでは、(スタック メモリの 1 MB) などのオーバーヘッドがあります。 サービス間の待機時間の長い呼び出しの同期メソッドを使用して、スレッド プールの拡大、.NET 4.5 の既定値に最大 5 個の web アプリケーション、000 スレッドはおよそ 5 GB 以上のメモリを消費できるアプリケーションよりも、サービスと同じ要求を使用して非同期メソッドは、50 個だけのスレッド。 非同期操作を実行するとき、スレッドを常にではありませんを使用しています。 などの非同期 web サービス要求をすると、ASP.NET は使用しないすべてのスレッド間で、 **async**メソッドの呼び出しと**await**します。 高待機時間で要求を処理するスレッド プールの使用は、大量のメモリ フット プリントとサーバー ハードウェアの使用率の低下につながります。

## <a name="processing-asynchronous-requests"></a>非同期要求の処理

Web アプリケーションの起動時に同時要求の数が多いを参照してください。 または、(、同時実行性が大きくなりますが突然) バースト負荷では、web サービス呼び出しの非同期化すると、アプリケーションの応答性が向上します。 非同期の要求には、同期要求の処理に時間の同じ時間がかかります。 たとえば、要求は、web サービスの呼び出しを場合に、2 秒を完了する要求は 2 秒間同期または非同期で実行されるかどうかが必要です。 ただし、非同期の呼び出し中にスレッドがブロックされていない最初の要求が完了するまで待機する間、他の要求に応答しています。 そのため、非同期要求は、多数の同時要求実行時間の長い操作を呼び出すことがある場合に要求キューとスレッド プールの増大を防ぐ。

## <a id="ChoosingSyncVasync"></a>  同期または非同期のメソッドを選択します。

このセクションでは、同期または非同期のメソッドを使用する場合のガイドラインを示します。 これらはガイドラインです。非同期メソッドは、パフォーマンスを向上するかどうかを判断するには、個別に各アプリケーションを確認します。

一般に、次の条件の同期メソッドを使用します。

- 操作は、単純なまたは実行時間が短いが。
- 簡潔さが効率性よりも重要です。
- 操作は、膨大なディスクやネットワークのオーバーヘッドを伴う操作ではなく主に CPU 操作です。 CPU バインド操作で非同期メソッドを使用して、利点はなく、オーバーヘッドが大きくなります。

  一般に、非同期メソッドを使用して、次の条件。

- 非同期のメソッドで使用できるサービスを呼び出しているし、.NET 4.5 以降を使用しています。
- 操作は、ネットワーク バインドまたは O バウンド CPU バインドではなくです。
- 並列処理は、コードの簡潔さよりも重要です。
- ユーザーが実行時間の長い要求をキャンセルできるようにするメカニズムを提供します。
- ときにスレッドの切り替えの利点は、重みのコンテキストの切り替えのコスト。 一般に、する必要がありますメソッドを非同期に同期メソッドは、作業を行っていないときに、ASP.NET 要求スレッドをブロックする場合。 非同期呼び出しを行って、ASP.NET 要求スレッドがブロックされていない web サービス要求の完了を待つ間に作業が行いません。
- テストには、ブロック操作が、サイトのパフォーマンスのボトルネックになると、IIS がこれらのブロック呼び出しの非同期メソッドを使用して、多くの要求をサービスするが表示されます。

  ダウンロード可能なサンプルでは、非同期メソッドを効果的に使用する方法を示します。 このサンプルでは、ASP.NET 4.5 での非同期プログラミングの簡単なデモを提供する設計されました。 サンプルは、ASP.NET での非同期プログラミング向けのリファレンス アーキテクチャではありません。 サンプル プログラムの呼び出し[ASP.NET Web API](../../../web-api/index.md)メソッドを呼び出す[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)実行時間の長い web サービスの呼び出しをシミュレートします。 ほとんどの実稼働アプリケーションでは、非同期メソッドを使用してこのような明らかなメリットは表示されません。   
  
ほとんどのアプリケーションでは、すべてのメソッドを非同期にする必要があります。 多くの場合、いくつかの同期メソッドを非同期メソッドに変換するために必要な作業量の最適な効率向上を提供します。

## <a id="SampleApp"></a>  サンプル アプリケーション

サンプル アプリケーションをダウンロードする[ https://github.com/RickAndMSFT/Async-ASP.NET ](https://github.com/RickAndMSFT/Async-ASP.NET)上、 [GitHub](https://github.com/)サイト。 リポジトリは、3 つのプロジェクトで構成されます。

- *WebAppAsync*: Web API を使用する、ASP.NET Web フォーム プロジェクト**WebAPIpwg**サービス。 コードのほとんどは、このチュートリアルでは、これからのプロジェクト。
- *WebAPIpgw*: を実装する ASP.NET MVC 4 Web API プロジェクト、`Products, Gizmos and Widgets`コント ローラー。 データを提供しますが、 *WebAppAsync*プロジェクトと*Mvc4Async*プロジェクト。
- *Mvc4Async*: 別のチュートリアルで使用するコードが含まれています: ASP.NET MVC 4 プロジェクト。 Web API の呼び出し、 **WebAPIpwg**サービス。

## <a id="GizmosSynch"></a>  ギズモ同期ページ

 次のコードは、`Page_Load`ギズモの一覧を表示するために使用する同期メソッド。 (この記事ではの商品は架空の機械的なデバイス) です。 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

次のコードは、`GetGizmos`の商品サービスのメソッド。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos`メソッドは URI、ギズモ データの一覧を返す ASP.NET Web API HTTP サービスに渡します。 *WebAPIpgw*プロジェクトには、Web API の実装が含まれている`gizmos, widget`と`product`コント ローラー。  
次の図では、サンプル プロジェクトからギズモ ページを示します。

![ギズモ](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  非同期ギズモ ページの作成

このサンプルは、新しい[非同期](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)と[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)キーワード (.NET 4.5 と Visual Studio 2012 で使用可能) コンパイラで複雑な変換のために必要な保守を担当します。非同期プログラミングします。 コンパイラでは、# の同期の制御フローの構築を使用してコードを記述できるし、コンパイラは、スレッドがブロックされないようにするためにコールバックを使用するために必要な変換を自動的に適用されます。

ASP.NET の非同期ページを含める必要があります、[ページ](https://msdn.microsoft.com/library/ydy4x04a.aspx)ディレクティブ、`Async`属性が"true"に設定します。 次のコードは、[ページ](https://msdn.microsoft.com/library/ydy4x04a.aspx)ディレクティブ、`Async`属性を"true"に設定、 *GizmosAsync.aspx*ページ。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

次のコードは、`Gizmos`同期`Page_Load`メソッドと`GizmosAsync`非同期ページです。 お使いのブラウザーがサポートしている場合、 [HTML 5&lt;マーク&gt;要素](http://www.w3.org/wiki/HTML/Elements/mark)で変更が表示されます`GizmosAsync`黄色の強調表示にします。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

非同期バージョン。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 次の変更を許可する適用された、`GizmosAsync`ページが非同期にします。

- [ページ](https://msdn.microsoft.com/library/ydy4x04a.aspx)ディレクティブがあります、`Async`属性が"true"に設定します。
- `RegisterAsyncTask`を非同期的に実行されるコードを格納している非同期タスクを登録するメソッドを使用します。
- 新しい`GetGizmosSvcAsync`メソッドが設定されて、[非同期](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードは、本文の一部にコールバックを生成して、自動的に作成するコンパイラに指示する、`Task`が返されます。
- &quot;非同期&quot;に非同期のメソッド名が追加されました。 "Async"を追加する必要はありませんが、非同期メソッドを記述する場合、規則は、します。
- 戻り値の型の新しい`GetGizmosSvcAsync`メソッドは`Task`します。 戻り値の型`Task`進行中の作業を表し、非同期操作の完了を待機するためのハンドルを持つメソッドの呼び出し元を提供します。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)キーワードは、web サービス呼び出しに適用されました。
- 非同期 web サービス API が呼び出された (`GetGizmosAsync`)。

内の`GetGizmosSvcAsync`メソッド本文の別の非同期メソッド`GetGizmosAsync`が呼び出されます。 `GetGizmosAsync` 直ちに返し、`Task<List<Gizmo>>`データが使用可能な場合に最終的には完了です。 コードが、タスクを待機の商品データを設定するまで何も行うしないため、(を使用して、 **await**キーワード)。 使用することができます、 **await**注釈が付けられたメソッドでのみキーワード、 **async**キーワード。

**Await**タスクが完了するまでキーワードが、スレッドをブロックしません。 メソッドの残りの部分が、タスクのコールバックとしてサインアップし、即座に返されます。 待機中のタスクが最終的に完了すると、そのコールバックを呼び出し、したがって箇所から右側のメソッドの実行が再開されます。 使用しての詳細については、 [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)と[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードと[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)名前空間を参照してください、[非同期参照](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)。

次のコードは、`GetGizmos`と`GetGizmosAsync`メソッド。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 非同期の変更に行われたものと似ています、 **GizmosAsync**上。 

- メソッド シグネチャが付きますが、[非同期](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)戻り値の型が変更されたキーワード、`Task<List<Gizmo>>`と*Async*メソッド名に追加されました。
- 非同期の[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)クラスは、同期ではなく使用[WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx)クラス。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)にキーワードが適用された、 [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx)非同期メソッドです。

次の図では、非同期の商品について表示します。

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

ギズモ データのブラウザーの表示は、同期呼び出しで作成したビューと同じです。 唯一の違いは、非同期バージョンは負荷の高いパフォーマンスが向上する可能性があります。

## <a name="registerasynctask-notes"></a>RegisterAsyncTask のノート

メソッドがでフック`RegisterAsyncTask`後すぐに実行が[PreRender](https://msdn.microsoft.com/library/ms178472.aspx)します。 次のコードに示すように、async void のページ イベントを直接使用も。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Async void イベントに欠点は、開発者が完全にイベントの実行と制御になくなりました。 たとえば、.aspx 両方の場合、します。マスター定義`Page_Load`イベントおよび 1 つまたはその両方が非同期で実行の順序は保証されません。 以外のイベント ハンドラーの同じ indeterminiate 順序 (など`async void Button_Click`) 適用されます。 ほとんどの開発者、ありますなどの Api を使用する必要がありますのみの実行順序を完全に制御を必要とするユーザー`RegisterAsyncTask`タスク オブジェクトを返すメソッドを使用します。

## <a id="Parallel"></a>  複数の操作を並列で実行します。

非同期メソッドでは、アクションがいくつかの独立した操作を実行するときに同期メソッドに優る大きな利点があります。 サンプルでは、同期ページ*PWG.aspx*(製品、ウィジェットとギズモ) の製品、ウィジェット、およびギズモの一覧を取得する次の 3 つの web サービス呼び出しの結果が表示されます。 [ASP.NET Web API](../../../web-api/index.md)では、プロジェクト サービスの使用[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)待機時間または低速ネットワークをシミュレートするために呼び出します。 500 ミリ秒、非同期に遅延を設定すると*PWGasync.aspx*ページの中に、同期を完了する少し 500 ミリ秒かかる`PWG`バージョンは 1,500 (ミリ秒)。 同期*PWG.aspx*次のコード ページが表示されます。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

非同期の`PWGasync`分離コードを次に示します。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

次の図は、非同期から返されるビュー *PWGasync.aspx*ページ。

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>  キャンセル トークンを使用します。

返す非同期メソッド`Task`を受け取るには、キャンセル可能なは、 [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)パラメーターのいずれかを指定した場合、`AsyncTimeout`の属性、[ページ](https://msdn.microsoft.com/library/ydy4x04a.aspx)ディレクティブ。 次のコードは、 *GizmosCancelAsync.aspx*秒のタイムアウトでページ。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

次のコードは、 *GizmosCancelAsync.aspx.cs*ファイル。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

提供されるサンプル アプリケーションで選択すると、 *GizmosCancelAsync*呼び出しのリンク、 *GizmosCancelAsync.aspx*ページおよび非同期の呼び出しの (タイムアウト) をキャンセルした場合の例を示します。 遅延時間では、任意の範囲内であるために、タイムアウト エラー メッセージを取得するページを 2 回の更新する必要があります。

## <a id="ServerConfig"></a>  高い同時実行/高待機時間の Web サービス呼び出しのサーバー構成

非同期の web アプリケーションのメリットを実現するには、するには、既定のサーバー構成を変更する必要があります。 構成する場合の注意点とストレス テストを非同期の web アプリケーションで、次に注意します。

- Windows 7、Windows Vista、Window 8、およびすべての Windows クライアント オペレーティング システムはある 10 個の同時要求の最大値です。 Windows サーバー オペレーティング システムの高負荷の下での非同期メソッドのメリットを確認する必要があります。
- 次のコマンドを使用して、管理者特権でコマンド プロンプトから、IIS と .NET 4.5 を登録します。  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
  参照してください[ASP.NET IIS 登録ツール (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- 大きく必要がありますが、 [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) 1,000 から 5,000 の既定値からキューの制限。 表示設定が低すぎる場合は、 [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) HTTP 503 ステータスの要求を拒否します。 HTTP.sys のキューの制限を変更するには。

    - IIS マネージャーを開き、アプリケーション プール ウィンドウに移動します。
    - 対象のアプリケーション プールを右クリックし、選択**詳細設定**します。  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - **詳細設定** ダイアログ ボックスで、変更*キューの長さ*5,000 に 1,000 から。  
        ![キューの長さ](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  上記のイメージでアプリケーション プールが .NET 4.5 を使用している場合でも、.NET framework が v4.0、として一覧表示に注意してください。 この違いを理解するのには、次を参照してください。

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- アプリケーションが web サービスを使用して、または HTTP 経由でバックエンドと通信する System.NET を増やす必要があります、 [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx)要素。 ASP.NET アプリケーションでこれは Cpu の数の 12 倍の自動構成機能によって制限されます。 つまり、クアッド プロセッサでは必要な最大で 12 \* 4 = 48 IP エンドポイントへの同時接続。 これに関連付けられているため、 [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)、向上させる最も簡単な方法として`maxconnection`では、ASP.NET アプリケーションの設定は、 [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx)でプログラムを使用`Application_Start`メソッドで、 *global.asax*ファイル。 例については、ダウンロード、サンプルを参照してください。
- .NET 4.5 で、既定値の 5000 [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)正しくする必要があります。

## <a name="contributors"></a>共同作成者

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
