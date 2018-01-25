---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: "ASP.NET 4.5 で非同期メソッドの使用 |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、Web、これは、無料の Visual Studio Express 2012 を使用して、非同期の ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 73e46134cfafb9edc4c1888211eab44b8f2bf828
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>ASP.NET 4.5 で非同期メソッドの使用
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルが非同期 ASP.NET Web フォーム アプリケーションを使用して、作成の基本を教える[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11)、これは、無料版の Microsoft Visual Studio です。 使用することも[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)です。 このチュートリアルでは、次のセクションが含まれています。
> 
> - [スレッド プール内に要求を処理する方法](#HowRequestsProcessedByTP)
> - [同期または非同期のメソッドを選択します。](#ChoosingSyncVasync)
> - [サンプル アプリケーション](#SampleApp)
> - [装置の同期 ページ](#GizmosSynch)
> - [非同期の装置ページを作成します。](#CreatingAsynchGizmos)
> - [同時に複数の操作を実行します。](#Parallel)
> - [キャンセル トークンを使用します。](#CancelToken)
> - [高い同時実行/高待機時間の Web サービス呼び出しのサーバー構成](#ServerConfig)
> 
> このチュートリアルの完全なサンプルが提供されます。  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) on the [GitHub](https://github.com/) site.


ASP.NET 4.5 Web Pages の組み合わせで[.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx)を型のオブジェクトを返す非同期メソッドを登録することができます[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)です。 .NET Framework 4 と呼ばれる非同期のプログラミング概念を導入された、[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)ASP.NET 4.5 をサポートしていると[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)です。 タスクがによって表される、**タスク**型と関連する型を[System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx)名前空間。 この非同期サポートに基づいて、.NET Framework 4.5、 [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)と[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードを使用した作業[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)以前よりもそれほど複雑なオブジェクト非同期の認証方法 [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)キーワードは、コードの断片がコードの他のいくつかの部分で非同期的に待機することを示す構文短縮形です。 [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードは、タスク ベースの非同期メソッドとしてメソッドを示すために使用できるヒントを表します。 組み合わせ**await**、 **async**、および**タスク**オブジェクトは .NET 4.5 での非同期コードを記述するためのより簡単です。 非同期メソッド用の新しいモデルを呼び出す、*タスク ベースの非同期パターン*(**タップ**)。 このチュートリアルでは、非同期プログラミングを使用した経験があると仮定[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)と[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードおよび[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)名前空間。

使用して、詳細について[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)と[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードおよび[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)名前空間には、次を参照します。

- [.NET でのホワイト ペーパー: 非同期性](https://go.microsoft.com/fwlink/?LinkId=204844)
- [よく寄せられる質問の Async と Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio の非同期プログラミング](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>スレッド プール内に要求を処理する方法

Web サーバーでは、.NET Framework は、ASP.NET 要求をサービスに使用されるスレッドのプールを保持します。 要求が到着すると、その要求を処理するプールのスレッドがディスパッチされます。 要求が同期的に処理される要求を処理するスレッドが要求中にビジー状態が処理されると、スレッドが別の要求を処理します。   
  
ない問題になるは、スレッド プールに多くのビジー状態スレッド数を十分に格納できるためです。 ただし、スレッド プール内のスレッドの数は無制限です (既定の .NET 4.5 の最大値は 5,000)。 実行時間の長い要求の高度に並列で大規模なアプリケーションで利用可能なすべてのスレッドがビジー状態である可能性があります。 この条件は、スレッドの不足と呼ばれます。 この状態に達すると、web サーバーは要求をキューします。 要求キューがいっぱいになると、web サーバーは HTTP 503 ステータス (サーバーがビジー状態) で要求を拒否します。 CLR スレッド プールでは、新しいスレッドのインジェクションの制限があります。 同時実行性が集中的な場合 (つまり、web サイトできます突然取得要求の数が多い) と使用可能な要求のすべてのスレッドがビジー状態でバックエンドの呼び出しのため、高レイテンシ、限られたスレッドのインジェクション レートが非常に適切に対応するアプリケーションを行うことができます。 また、スレッド プールに追加された新しいスレッドごとには、(スタック メモリの 1 MB) などのオーバーヘッドがあります。 ここで、スレッド プールに拡大され、.NET 4.5 既定値 5 の最大サービス待機時間の長い呼び出しを同期メソッドを使用して web アプリケーション、000 スレッドはおよそ 5 GB 多くのメモリ消費できるアプリケーションよりも、サービスと同じ要求を使用します。非同期メソッドは、50 個だけスレッド。 非同期操作を実行しているときは、スレッドが使用している常にではありません。 など、非同期の web サービス要求すると、ASP.NET は使わない間のすべてのスレッド、 **async**メソッドの呼び出しと**await**です。 待機時間の長い要求を処理するスレッド プールの使用は、大量のメモリ使用量とサーバー ハードウェアの使用率の低下につながります。

## <a name="processing-asynchronous-requests"></a>非同期要求の処理

起動時に同時要求数が多いを参照してくださいまたは (ここで同時実行制御が増える突然) バースト負荷が用意されている web アプリケーションでは、web サービス呼び出しを非同期に作成すると、アプリケーションの応答性が向上します。 非同期の要求には、同期要求として処理には同じ時間がかかります。 たとえば、要求によって、web サービスを呼び出す場合に、2 秒完了するには、要求が 2 秒間同期または非同期で実行されるかどうかが必要です。 ただし、非同期の呼び出し中に、スレッドはブロックされません、最初の要求を完了するまで待機する間、他の要求に応答します。 そのため、非同期要求は、実行時間の長い操作の呼び出しの多くの同時要求がある場合に、要求キューおよびスレッド プールの拡張を防ぐ。

## <a id="ChoosingSyncVasync"></a>同期または非同期のメソッドを選択します。

このセクションでは、同期または非同期のメソッドを使用する際のガイドラインを示します。 単なるガイドラインです。非同期メソッドは、パフォーマンスを向上するかどうかを決定するには、個別に各アプリケーションのアプリケーションを確認します。

一般に、次の条件の同期メソッドを使用します。

- 操作が単純型または短時間であります。
- 簡潔さは効率よりも重要です。
- 操作は、膨大なディスクまたはネットワークのオーバーヘッドが含まれる操作ではなく、主に CPU 操作です。 CPU 主体の操作に非同期メソッドを使用しても利点し、オーバーヘッドが大きくなります。

 一般に、次の条件の非同期メソッドを使用します。

- 非同期メソッドは、を通じて利用できるサービスを呼び出しているし、.NET 4.5 以上を使用しています。
- 操作があるネットワーク バインドまたは I/O-バインドではなく CPU 主体です。
- 並列処理は、コードの単純化よりも重要です。
- ユーザーが実行時間の長い要求をキャンセルできるメカニズムを提供します。
- ときにスレッドを切り替えるの利点には、コンテキストの切り替えのコストが重み付けされます。 一般に、する必要がありますメソッド非同期同期メソッドでは、ASP.NET 要求スレッドをブロック中の作業を行っていない場合。 非同期呼び出しを行って、によって ASP.NET 要求スレッドはブロックされませんなしの作業を行う web サービス要求が完了するまで待機するときにします。
- テストは、ブロック操作は、サイトのパフォーマンスのボトルネックであるし、IIS で非同期メソッドのようなブロッキング呼び出しを使用してより多くの要求を処理できることを示します。

 ダウンロード可能なサンプルでは、非同期メソッドを効果的に使用する方法を示します。 このサンプルでは、ASP.NET 4.5 での非同期プログラミングの簡単なデモを提供するよう設計されました。 このサンプルは、ASP.NET で非同期プログラミングの参照アーキテクチャではありません。 サンプル プログラム呼び出し[ASP.NET Web API](../../../web-api/index.md)メソッドを呼び出している[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)実行時間の長い web サービス呼び出しをシミュレートするためにします。 ほとんどの実稼働アプリケーションでは、非同期メソッドを使用するこのような明らかな利点は表示されません。   
  
ほとんどのアプリケーションでは、すべてのメソッドを非同期にする必要があります。 多くの場合、いくつかの同期メソッドを非同期メソッドに変換するために必要な作業の量に対して最適な効率向上を提供します。

## <a id="SampleApp"></a>サンプル アプリケーション

サンプル アプリケーションをダウンロードする[https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET)上、 [GitHub](https://github.com/)サイトです。 リポジトリは、3 つのプロジェクトで構成されます。

- *WebAppAsync*: Web API を使用する ASP.NET Web フォームの プロジェクト**WebAPIpwg**サービス。 ほとんどのコードから、これは、このチュートリアルのプロジェクトです。
- *WebAPIpgw*: ASP.NET MVC 4 Web API は、プロジェクトを実装する、`Products, Gizmos and Widgets`コント ローラー。 データを提供、 *WebAppAsync*プロジェクトおよび*Mvc4Async*プロジェクト。
- *Mvc4Async*: 別のチュートリアルで使用するコードが含まれています: ASP.NET MVC 4 プロジェクト。 Web API 呼び出しがなさ、 **WebAPIpwg**サービス。

## <a id="GizmosSynch"></a>装置の同期 ページ

 次のコードは、`Page_Load`装置の一覧を表示するために使用する同期メソッド。 (この記事ではの商品は架空の機械です。) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

次のコードは、`GetGizmos`商品サービスのメソッドです。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos`メソッドは、装置のデータの一覧を返す ASP.NET Web API HTTP サービスに URI を渡します。 *WebAPIpgw*プロジェクトには、Web API の実装が含まれている`gizmos, widget`と`product`コント ローラー。  
次の図は、サンプル プロジェクトから装置ページを示します。

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>非同期の装置ページを作成します。

このサンプルでは、新しい[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)と[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (.NET 4.5 と Visual Studio 2012 で使用できる) キーワード コンパイラで複雑な変換のために必要な保守を担当します。非同期プログラミングします。 コンパイラでは、# の同期の制御フローの構築を使用してコードを記述することができ、コンパイラが自動的にスレッドがブロックされないようにするためにコールバックを使用するための変換を適用します。

非同期の ASP.NET ページを含める必要があります、[ページ](https://msdn.microsoft.com/library/ydy4x04a.aspx)でディレクティブ、`Async`属性が"true"に設定します。 次のコードは、[ページ](https://msdn.microsoft.com/library/ydy4x04a.aspx)でディレクティブ、`Async`属性を"true"に設定、 *GizmosAsync.aspx*ページ。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

次のコードは、`Gizmos`同期`Page_Load`メソッドおよび`GizmosAsync`非同期ページ。 お使いのブラウザーがサポートされている場合、 [HTML 5&lt;マーク&gt;要素](http://www.w3.org/wiki/HTML/Elements/mark)での変更を確認`GizmosAsync`黄色のハイライトでします。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

非同期バージョン:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 使用できるように、次の変更が適用された、`GizmosAsync`ページが非同期になります。

- [ページ](https://msdn.microsoft.com/library/ydy4x04a.aspx)ディレクティブが必要、`Async`属性が"true"に設定します。
- `RegisterAsyncTask`メソッドは、非同期タスクを非同期的に実行されるコードを含む登録に使用します。
- 新しい`GetGizmosSvcAsync`メソッドが付いて、 [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)本文の一部のコールバックを生成して自動的に作成するのには、コンパイラに指示するキーワード、`Task`が返されます。
- &quot;非同期&quot;非同期のメソッド名に追加されました。 "Async"を追加することは必要ありませんが、非同期メソッドを記述する場合、規則は。
- 戻り値の型の新しい新しい`GetGizmosSvcAsync`メソッドは`Task`します。 戻り値の型`Task`進行中の作業を表し、メソッドの呼び出し元の非同期操作の完了を待機するためのハンドルに提供します。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)キーワードは、web サービスの呼び出しに適用されました。
- 非同期の web サービス API が呼び出されました (`GetGizmosAsync`)。

内の`GetGizmosSvcAsync`メソッド本体の別の非同期メソッド、`GetGizmosAsync`と呼びます。 `GetGizmosAsync`直ちに返されます、`Task<List<Gizmo>>`データが使用可能な場合に最終的には完了です。 コードがタスクを待機するための商品データを取得するまで何も行うたく (を使用して、 **await**キーワード) です。 使用することができます、 **await**キーワード注釈が付けられたメソッドでのみ、 **async**キーワード。

**Await**タスクが完了するまでキーワードが、スレッドをブロックしません。 タスクでのコールバック メソッドの残りの部分を署名し、直ちに返されます。 最終的に待機中のタスクが完了したら、そのコールバックを呼び出し、したがってところメソッド右側の実行が再開されます。 使用の詳細について、 [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)と[async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)キーワードおよび[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)名前空間を参照してください、 [async 参照](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)です。

次のコードは、`GetGizmos`と`GetGizmosAsync`メソッドです。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 非同期の変更と似ていますに加え、 **GizmosAsync**上。 

- メソッドのシグネチャが注釈が付けられた、 [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)戻り値の型が変更されたキーワード、 `Task<List<Gizmo>>`、および*Async*メソッド名に追加されました。
- 非同期の[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)クラスは、同期ではなく、使用[WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx)クラスです。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)キーワードに適用された、 [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[されます](https://msdn.microsoft.com/library/hh158944(VS.110).aspx)非同期メソッドです。

次の図では、非同期の商品について表示されます。

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

装置のデータのブラウザーのプレゼンテーションでは、同期呼び出しによって作成されるビューと同じです。 唯一の違いは、非同期バージョンの効率的で負荷の高い可能性があります。

## <a name="registerasynctask-notes"></a>RegisterAsyncTask ノート

メソッドをフック`RegisterAsyncTask`後すぐに実行されます[PreRender](https://msdn.microsoft.com/library/ms178472.aspx)です。 次のコードに示すようにページが void の非同期イベントを直接、使用することができますも。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Void の非同期イベントの欠点は、開発者は、イベントの実行中を完全に制御を不要になったです。 たとえば、両方 .aspx とします。マスター定義`Page_Load`イベントとの一方または両方には、非同期、実行の順序は保証されません。 以外のイベント ハンドラーに対して同じ indeterminiate 順序 (など`async void Button_Click`) 適用されます。 ほとんどの開発者にとってこれは、許容されるが、実行の順序を完全に制御を必要とする管理者のみのような Api を使用してください`RegisterAsyncTask`タスク オブジェクトを返すメソッドを使用します。

## <a id="Parallel"></a>同時に複数の操作を実行します。

非同期メソッドでは、アクションは、複数の独立した操作を実行する必要がある時重要な利点は、同期メソッドがあります。 このサンプルでは、同期のページで*PWG.aspx*(製品、ウィジェット、および装置) 用の製品、ウィジェット、装置の一覧を取得する 3 つの web サービス呼び出しの結果が表示されます。 [ASP.NET Web API](../../../web-api/index.md)提供されますが、プロジェクトのサービス使用[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)に関する遅延または低速ネットワークをシミュレートするために呼び出します。 500 (ミリ秒単位)、非同期に遅延を設定すると*PWGasync.aspx*ページは、同期中に完了するは少し以上 500 ミリ秒`PWG`バージョンが 1,500 (ミリ秒) を引き継ぎます。 同期*PWG.aspx*ページが次のコードに示すようにします。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

非同期の`PWGasync`分離コードを次に示します。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

次の図は、非同期から返されたビュー *PWGasync.aspx*ページ。

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>キャンセル トークンを使用します。

返す非同期メソッド`Task`は取り消し可能なこれらのパラメーターである、 [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)がで提供されている場合に、パラメーター、`AsyncTimeout`の属性、[ページ](https://msdn.microsoft.com/library/ydy4x04a.aspx)ディレクティブです。 次のコードは、 *GizmosCancelAsync.aspx*タイムアウトは 2 番目のページです。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

次のコードは、 *GizmosCancelAsync.aspx.cs*ファイル。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

提供されたサンプル アプリケーションでを選択すると、 *GizmosCancelAsync*呼び出しをリンク、 *GizmosCancelAsync.aspx*ページし、非同期呼び出しの (タイムアウト) によって取り消しを示します。 遅延時間は、任意の範囲内では、タイムアウト エラー メッセージを取得するページを 2 回の更新が必要です。

## <a id="ServerConfig"></a>高い同時実行/高待機時間の Web サービス呼び出しのサーバー構成

非同期の web アプリケーションのメリットを実現するには、既定のサーバー構成にいくつかの変更を加える必要があります。 構成する場合の注意点およびストレスの非同期的な web アプリケーションのテストで、次に注意します。

- Windows 7、Windows Vista、ウィンドウ 8、およびすべての Windows クライアント オペレーティング システムは最大 10 個の同時実行要求があります。 Windows サーバー オペレーティング システムに高負荷時の非同期メソッドのメリットを確認する必要があります。
- 次のコマンドを使用して、管理者特権でコマンド プロンプトから、IIS に .NET 4.5 を登録します。  
 %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
 参照してください[ASP.NET IIS 登録ツール (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- 大きく必要があります、 [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) 1,000 ~ 5,000 の既定値からキューの制限。 表示設定が低すぎる場合は、 [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) HTTP 503 ステータスの要求を拒否します。 HTTP.sys のキューの制限を変更します。

    - IIS マネージャーを開き、アプリケーション プール ウィンドウに移動します。
    - 対象のアプリケーション プールを右クリックし、選択**詳細設定**です。  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - **詳細設定** ダイアログ ボックスで、変更*Queue Length* 5,000 に 1,000 からです。  
        ![キューの長さ](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
 V4.0、として、.NET framework が表示されているアプリケーション プールが .NET 4.5 を使用しても、上記のイメージに注意してください。 このような差異を理解するのには、次を参照してください。

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- 場合は、アプリケーションが web サービスを使用するか、HTTP 経由でバックエンドと通信する System.NET を増やす必要があります、 [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx)要素。 ASP.NET アプリケーションの場合これは Cpu の 12 倍の数を自動構成機能によって制限されます。 つまりするクアッド プロセッサであり、最大で 12 \* 4 = 48 IP エンドポイントへの同時接続。 これに関連付けられているため[autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)、向上させる最も簡単な方法として`maxconnection`では、ASP.NET アプリケーションの設定を[System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx)プログラムでの`Application_Start`メソッドで、 *global.asax*ファイル。 例については、ダウンロード サンプルを参照してください。
- .NET 4.5、5000 までの既定値で[MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)細かくする必要があります。

## <a name="contributors"></a>共同作成者

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
