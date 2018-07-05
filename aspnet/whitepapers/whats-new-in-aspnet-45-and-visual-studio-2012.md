---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: ASP.NET 4.5 と Visual Studio 2012 における新 |Microsoft Docs
author: rick-anderson
description: このドキュメントでは、新機能と ASP.NET 4.5 で追加される予定の機能強化について説明します。 Web 開発の強化についても説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: ''
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: e89b8faa7d513209436d40d15821694cf24fa453
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369348"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>ASP.NET 4.5 と Visual Studio 2012 における新します。
====================
> このドキュメントでは、新機能と ASP.NET 4.5 で追加される予定の機能強化について説明します。 Visual Studio 2012 での web 開発の強化についても説明します。 このドキュメントでは、2012 年 2 月 29 日発行されました。


- [ASP.NET Core ランタイムおよびフレームワーク](#_Toc318097372)

    - [非同期的に読み取りと HTTP 要求と応答の書き込み](#_Toc318097373)
    - [HttpRequest の処理の機能強化](#_Toc318097374)
    - [非同期的に応答をフラッシュ](#_Toc318097375)
    - [サポート*await*と*タスク*-ベースの非同期モジュールとハンドラー](#_Toc318097376)
    - [非同期 HTTP モジュール](#_Toc318097377)
    - [非同期 HTTP ハンドラ](#_Toc318097378)
    - [新しい ASP.NET 要求の検証機能](#_Toc318097379)
    - [(「レイジー」) の要求の検証を延期](#_Toc318097380)
    - [未検証の要求のサポート](#_Toc318097381)
    - [AntiXSS ライブラリ](#_Toc318097382)
    - [Websocket プロトコルのサポート](#_Toc318097383)
    - [バンドルと縮小](#_Toc318097384)
    - [Web ホストのパフォーマンスの向上](#_Toc_perf)

        - [主要なパフォーマンスの要因](#_Toc_perf_1)
        - [新しいパフォーマンス機能の要件](#_Toc_perf_2)
        - [一般的なアセンブリの共有](#_Toc_perf_3)
        - [マルチコア JIT コンパイルを使用して、すばやい起動時間の](#_Toc_perf_4)
        - [チューニングのガベージ コレクションのメモリを最適化するには](#_Toc_perf_5)
        - [Web アプリケーションのプリフェッチ](#_Toc_perf_6)
- [ASP.NET Web フォーム](#_Toc318097385)

    - [厳密に型指定されたデータ コントロール](#_Toc318097386)
    - [モデル バインド](#_Toc318097387)

        - [データの選択](#_Toc318097388)
        - [値プロバイダー](#_Toc318097389)
        - [コントロールの値によってフィルター処理](#_Toc318097390)
    - [HTML エンコードされたデータ バインディング式](#_Toc318097391)
    - [控え目な検証](#_Toc318097392)
    - [HTML5 の更新プログラム](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web ページ 2](#_Toc318097395)
- [Visual Studio 2012 リリース候補](#_Toc318097396)

    - [Visual Studio 2010 と Visual Studio 2012 リリース候補 (プロジェクト互換性) 間の共有プロジェクト](#project-compatibility)
    - [ASP.NET 4.5 web サイト テンプレートの構成の変更](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [ASP.NET ルーティング IIS 7 でネイティブ サポート](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML エディター](#_Toc318097397)

        - [スマート タスク](#_Toc318097398)
        - [待機する ARIA のサポート](#_Toc318097399)
        - [新しい HTML5 スニペット](#_Toc318097400)
        - [ユーザー コントロールに展開します。](#_Toc318097401)
        - [属性でコード ナゲット用の IntelliSense](#_Toc318097402)
        - [自動開始タグまたは終了タグの名前を変更するときの対応タグの名前を変更します。](#_Toc318097403)
        - [イベント ハンドラーの生成](#_Toc318097404)
        - [スマート インデント](#_Toc318097405)
        - [ステートメント入力候補の自動減少します。](#_Toc318097406)
    - [JavaScript エディター](#_Toc318097407)

        - [コードのアウトライン表示](#_Toc318097408)
        - [かっこの一致](#_Toc318097409)
        - [定義に移動](#_Toc318097410)
        - [ECMAScript5 のサポート](#_Toc318097411)
        - [DOM の IntelliSense](#_Toc318097412)
        - [VSDOC の署名のオーバー ロード](#_Toc318097413)
        - [暗黙的な参照](#_Toc318097414)
    - [CSS エディター](#_Toc318097415)

        - [ステートメント入力候補の自動減少します。](#_Toc318097416)
        - [階層インデントします。](#_Toc318097417)
        - [CSS ハックのサポート](#_Toc318097418)
        - [ベンダー固有のスキーマ (- moz-、- webkit)](#_Toc318097419)
        - [コメント化とコメント解除のサポート](#_Toc318097420)
        - [カラー ピッカー](#_Toc318097421)
        - [スニペット](#_Toc318097422)
        - [カスタムのリージョン](#_Toc318097423)
    - [Page Inspector](#_Toc318097424)
    - [発行](#_Toc318097425)

        - [発行プロファイル](#_Toc318097426)
        - [ASP.NET プリコンパイルとマージ](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [免責事項](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core ランタイムおよびフレームワーク

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>非同期的に読み取りと HTTP 要求と応答の書き込み

ASP.NET 4 に導入されたを使用してストリームとして HTTP 要求のエンティティを読み取る、 *HttpRequest.GetBufferlessInputStream*メソッド。 このメソッドは、要求のエンティティへのストリーミング アクセスを提供します。 ただし、その同期的に実行、要求の期間のスレッドが関連付けられます。

ASP.NET 4.5 には、HTTP 要求エンティティで非同期的にストリームを読み取る機能、および非同期的にフラッシュする機能がサポートしています。 ASP.NET 4.5 は倍、コント ローラーの統合が容易に .aspx ページ ハンドラーと ASP.NET MVC などのダウン ストリームの HTTP ハンドラーを提供する HTTP 要求エンティティをバッファーする機能も提供します。

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>HttpRequest の処理の機能強化

ASP.NET 4.5 からによって返される Stream 参照*HttpRequest.GetBufferlessInputStream*同期および非同期の読み取りメソッドをサポートしています。 *Stream*から返されるオブジェクト*GetBufferlessInputStream* BeginRead および EndRead の両方のメソッドを実装するようになりました。 非同期の*Stream*メソッドを使用して、ASP.NET の非同期読み取りループの各反復処理の間、現在のスレッドを解放するときに、チャンク単位で要求のエンティティを非同期的に読み取りますできます。

ASP.NET 4.5 は、バッファー内の方法で要求のエンティティを読み取るためのコンパニオン メソッドも追加されています: *HttpRequest.GetBufferedInputStream*します。 この新しいオーバー ロードのように機能*GetBufferlessInputStream*、同期および非同期の読み取りをサポートします。 ただし、読み取る、 *GetBufferedInputStream*もダウン ストリームのモジュールとハンドラーにより引き続き要求のエンティティがアクセスできるように ASP.NET の内部バッファーにエンティティのバイトをコピーします。 たとえば、アップ ストリームの一部、パイプライン内のコードは既に読み取られて、要求のエンティティを使用して*GetBufferedInputStream*、使用することもできます*HttpRequest.Form*または*指定ディレクトリ*. これにより、(データベースに大きなファイルのアップロードのストリーミングなど) の要求の非同期処理を実行するが .aspx ページや ASP.NET の MVC コント ローラーが、その後です。

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>非同期的に応答をフラッシュ

HTTP クライアントに応答を送信すると、クライアントが遠くにあるまたは接続が低帯域幅のかなりの時間がかかる場合があります。 通常、アプリケーションによって作成される、ASP.NET は応答のバイト数をバッファーします。 ASP.NET は、最後に、要求処理の未収バッファーの 1 つの送信操作を実行します。

定期的に呼び出す必要がある場合、バッファー内の応答が大きい (たとえば、クライアントに大きなファイルのストリーミング) *HttpResponse.Flush*バッファー内の出力をクライアントに送信を管理下にあるメモリ使用量を保持します。 ただし、ため*フラッシュ*同期呼び出しを繰り返し呼び出すは*フラッシュ*スレッド実行可能性のある時間の長い要求の継続時間を消費します。

ASP.NET 4.5 を使用して非同期的にフラッシュを実行するサポートが追加、 *BeginFlush*と*EndFlush*のメソッド、 *HttpResponse*クラス。 これらのメソッドを使用して、非同期モジュールと増分データをオペレーティング システムのスレッドを停止せず、クライアントに送信する非同期のハンドラーを作成できます。 間に*BeginFlush*と*EndFlush*呼び出し、ASP.NET が現在のスレッドを解放します。 これを実行時間の長い HTTP ダウンロードをサポートするために必要なアクティブなスレッドの合計数大幅に削減します。

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>サポート*await*と*タスク*-ベースの非同期モジュールとハンドラー

.NET Framework 4 と呼ばれる非同期のプログラミング概念を導入された、*タスク*します。 タスクがによって表される、*タスク*型と関連する型を*System.Threading.Tasks*名前空間。 .NET Framework 4.5 は、操作できるようにするコンパイラが強化された*タスク*オブジェクトの単純な。 コンパイラが 2 つの新しいキーワードをサポートする、.NET Framework 4.5: *await*と*async*します。 *Await*キーワードは簡略記法を示すためのコードがコードの他のいくつかの一部で非同期的に待機します。 *Async*キーワードは、タスク ベースの非同期メソッドとしてメソッドをマークするために使用できるヒントを表します。

組み合わせ*await*、 *async*、および*タスク*オブジェクトにより、.NET 4.5 で非同期コードを記述するためのはるかに簡単です。 ASP.NET 4.5 では、非同期 HTTP モジュールと、新しいコンパイラの機能強化を使用して、非同期 HTTP ハンドラーを記述できるようにする新しい Api でこれらの簡略化をサポートします。

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>非同期 HTTP モジュール

返すメソッド内で非同期操作を実行しながら、*タスク*オブジェクト。 次のコード例では、Microsoft のホーム ページをダウンロードする非同期の呼び出しを実行する非同期メソッドを定義します。 使用に注意してください、 *async*メソッド シグネチャ内のキーワードと*await*呼び出し*DownloadStringTaskAsync*。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

すべて記述する必要がある、.NET Framework のダウンロードを完了するを待つだけでなく、ダウンロードが完了したら、呼び出し履歴を自動的に復元中に、コール スタックをアンワインドは自動的に処理します。

ここで、非同期の ASP.NET HTTP モジュールでこの非同期メソッドを使用するとします。 ASP.NET 4.5 には、ヘルパー メソッドが含まれています (*EventHandlerTaskAsyncHelper*) と新しいデリゲート型 (*TaskEventHandler*) と、古いタスク ベースの非同期メソッドを統合するように使用できます。ASP.NET HTTP パイプラインによって公開される非同期プログラミング モデル。 この例ではどのように。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>非同期 HTTP ハンドラ

ASP.NET の非同期のハンドラーを記述する従来の方法で実装するためには、 *IHttpAsyncHandler*インターフェイス。 ASP.NET 4.5 では、 *HttpTaskAsyncHandler*非同期基本型から派生したことができます、非同期ハンドラーの記述が簡単になります。

*HttpTaskAsyncHandler*型が抽象であり、オーバーライドする必要があります、 *ProcessRequestAsync*メソッド。 内部的に ASP.NET が、戻り値の署名の統合のくれます。 (、*タスク*オブジェクト) の*ProcessRequestAsync* ASP.NET パイプラインで使用される古いの非同期プログラミング モデルを使用します。

次の例は、使用する方法を示しています。*タスク*と*await*非同期 HTTP ハンドラーの実装の一部として。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>新しい ASP.NET 要求の検証機能

既定では、ASP.NET は要求の検証を実行します: マークアップやフィールド、ヘッダー、cookie、およびなどでスクリプトを探す要求を調べます。 いずれかが検出された場合、ASP.NET は例外をスローします。 これは、潜在的なクロス サイト スクリプティング攻撃に対する防御の最前線として機能します。

ASP.NET 4.5 では、未検証要求データを選択的に読みやすくします。 ASP.NET 4.5 は、人気のある AntiXSS ライブラリで、外部ライブラリは、以前にも統合します。

開発者が、選択的に、アプリケーションの要求検証を無効にする機能をよく寄せられます。 たとえば、アプリケーションのフォーラム ソフトウェアが、可能性がある場合は、HTML 形式のフォーラムの投稿とコメントの送信が要求の検証はチェックは、他のすべてを確認できるようにします。

ASP.NET 4.5 を選択して検証されていない入力操作を簡単にする 2 つの機能が導入されています: (「レイジー」) の要求の検証および未検証要求データへのアクセスを遅延します。

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>(「レイジー」) の要求の検証を延期

既定を ASP.NET 4.5 で、すべての要求データは要求の検証の対象です。 ただし、要求データを実際にアクセスするまで、要求の検証を延期するアプリケーションを構成することができます。 (これとも呼ば遅延要求の検証、特定のデータ シナリオの遅延読み込みのような条件に基づいています。)遅延検証を設定して、Web.config ファイルで使用するアプリケーションを構成することができます、 *requestValidationMode*属性で 4.5 を*httpRUntime*要素は、次の例のように。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

要求の検証モードが 4.5 に設定されている場合、コードがその値にアクセスする場合にのみ、特定の要求の値に対してのみ、要求の検証がトリガーされます。 たとえば、次のコードは、Request.Form["forum の値を取得します。\_投稿"]、要求の検証はフォームのコレクション内の要素に対してのみ呼び出されます。 内の他の要素のいずれも、*フォーム*コレクションが検証されます。 要求の検証を行っていれば、以前のバージョンの ASP.NET では、要求全体のコレクションのコレクション内の任意の要素にアクセスしたときにトリガーされました。 新しい動作によって、別のアプリケーション コンポーネントを他の部分で要求の検証をトリガーすることがなく、要求データの異なる部分を見て簡単にできます。

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>未検証の要求のサポート

遅延要求の検証が単独で要求の検証をバイパスする選択的に問題が解決しません。 Request.Form["forum 呼び出し\_投稿"] もトリガーは、その特定の要求値の検証を要求します。 ただし、そのフィールド内のマークアップを許可するための検証をトリガーせずにこのフィールドにアクセスする可能性があります。

これは、ASP.NET 4.5 には、未検証要求データへのようになりましたサポートしています。 ASP.NET 4.5 が含まれていますが、新しい*Unvalidated*コレクション プロパティで、 *HttpRequest*クラス。 このコレクションがこのようなすべての要求のデータの一般的な値へのアクセスを提供します*フォーム*、 *QueryString*、 *Cookie*、および*Url*します。

フォーラムの例を使用して、未検証要求データを読めるように、まず新しい要求の検証モードを使用するアプリケーションを構成します。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

使用することができますし、 *HttpRequest.Unvalidated*未検証のフォーム値を読み取るためのプロパティ。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> セキュリティ -*未検証要求データを使用して、注意してください。* ASP.NET 4.5 では、未検証の要求のプロパティと非常に特定の未検証要求データのアクセスが容易にコレクションを追加します。 ただし、危険なテキストがない、ユーザーに表示されることを確認する要求の生データのカスタム検証を実行することも必要があります。


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>AntiXSS ライブラリ

ASP.NET 4.5 により、Microsoft AntiXSS ライブラリの普及すると、そのライブラリのバージョン 4.0 からコア エンコーディング ルーチンはようになりましたが組み込まれています。

エンコーディング ルーチンはによって実装される、 *AntiXssEncoder* 、新しい型*System.Web.Security.AntiXss*名前空間。 使用することができます、 *AntiXssEncoder*型で実装されている静的なエンコード方法のいずれかを呼び出すことによって、直接型。 ただし、新しい ANTI-XSS ルーチンを使用するための最も簡単な方法が使用する ASP.NET アプリケーションを構成するのには、 *AntiXssEncoder*既定クラス。 これを行うには、Web.config ファイルに次の属性を追加します。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

ときに、 *encoderType*属性を使用して、設定されて、 *AntiXssEncoder*型、すべての出力を新しいエンコーディング ルーチンを使用して自動的に ASP.NET でのエンコードします。

ASP.NET 4.5 組み込まれている外部 AntiXSS ライブラリの一部を次に示します。

- *HtmlEncode*、 *HtmlFormUrlEncode*、および*HtmlAttributeEncode*
- *XmlAttributeEncode*と*XmlEncode*
- *UrlEncode*と*UrlPathEncode* (新規)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Websocket プロトコルのサポート

Websocket プロトコルは、HTTP 経由でクライアントとサーバー間のセキュリティで保護された、リアルタイムの双方向通信を確立する方法を定義する標準ベースのネットワーク プロトコルです。 Microsoft は、プロトコルを定義するために、IETF と W3C 標準の本体を持つきました。 WebSockets プロトコルは、Microsoft のクライアントとモバイル オペレーティング システムの両方で Websocket プロトコルをサポートしている多くのリソースへの投資と任意のクライアント (ブラウザーだけでなく) でサポートされています。

Websocket プロトコルでは、はるかに簡単に、クライアントとサーバー間の実行時間の長いデータ転送を作成します。 たとえば、チャット アプリケーションの作成がはるかに簡単クライアントとサーバー間の実際の実行時間の長い接続を確立するため。 定期的なポーリングまたは HTTP 長いポーリングのソケットの動作をシミュレートするような回避策に頼る必要はありません。

ASP.NET 4.5 と IIS 8 の非同期的に読み取りと Websocket オブジェクトの文字列とバイナリ データの両方を記述するマネージ Api を使用する ASP.NET 開発者を有効にする WebSockets のサポート、低レベルに含まれます。 ASP.NET 4.5 では、新しい*System.Web.WebSockets* Websocket プロトコルを使用するための型を含む名前空間。

ブラウザー クライアントが、DOM を作成して Websocket 接続を確立*WebSocket*次の例のように、ASP.NET アプリケーションで URL を指すオブジェクト。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

あらゆる種類のモジュールまたはハンドラーを使用して ASP.NET では、Websocket のエンドポイントを作成できます。 前の例では、.ashx ファイルが使用された、.ashx ファイル ハンドラーを作成する簡単な方法であるためです。

に従って、Websocket プロトコルは、ASP.NET アプリケーションは、Websocket 要求に要求が HTTP GET 要求からアップグレードする必要がありますを指定して、クライアントの Websocket 要求を受け入れます。 次に例を示します。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest*メソッドは、ASP.NET は、現在の HTTP 要求をアンワインドして、関数デリゲートに制御を転送するために関数デリゲートを受け取ります。 この方法は、使用する方法のような概念的に*System.Threading.Thread*作業を実行するバック グラウンドでスレッドの開始のデリゲートを定義します。

ASP.NET とクライアントでは、Websocket ハンドシェイクを正常に完了しました、ASP.NET は、デリゲートを呼び出すし、Websocket アプリケーションでは、実行が開始されます。 次のコード例では、ASP.NET の組み込みの Websocket のサポートを使用する単純なエコー アプリケーションを示します。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

.NET 4.5 でのサポート、 *await*キーワードとタスク ベースの非同期操作は、Websocket のアプリケーションを記述するために自然に適合します。 Websocket 要求を ASP.NET 内で完全に非同期的に実行するコード例に示します。 アプリケーションが呼び出すことによってクライアントから送信されるメッセージの非同期的に待機*ソケットを待機します。ReceiveAsync*します。 呼び出すことによって、クライアントに非同期メッセージを送信する同様に、*ソケットを待機します。SendAsync*します。

Websocket 経由でメッセージを受信するアプリケーション、ブラウザーで、 *onmessage*関数。 ブラウザーからメッセージを送信するを呼び出す、*送信*のメソッド、 *WebSocket* DOM の種類、この例で示すように。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

今後、この機能を抽象離れたは低レベル コーディングの一部に必要であるこのリリースの Websocket アプリケーションへの更新プログラムがリリース可能性があります。

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>バンドルと縮小

バンドルには、個々 の JavaScript と CSS ファイルをまとめて 1 つのファイルのように扱うことができますをバンドルすることができます。 縮小では、空白や不要なその他の文字を削除することで JavaScript と CSS ファイルでまとめます。 これらの機能は、Web フォーム、ASP.NET MVC、および Web ページを使用します。

バンドルはバンドル クラスまたは ScriptBundle と StyleBundle その子クラスのいずれかを使用して作成されます。 バンドルのインスタンスを構成した後、バンドルは公開着信要求にグローバル BundleCollection インスタンスに追加するだけで。 既定のテンプレートでは、バンドルの構成は BundleConfig ファイルで実行されます。 この既定の構成は、中核となるスクリプトとテンプレートで使用される css ファイルのすべてのバンドルを作成します。

バンドルは、2 つの可能なヘルパー メソッドのいずれかを使用して、ビュー内から参照されます。 デバッグとリリース モードでのバンドルの異なるマークアップのレンダリングをサポートするために、ScriptBundle および StyleBundle クラスはレンダリング ヘルパー メソッドがあります。 デバッグ モードでレンダリングは、バンドル内の各リソースのマークアップを生成します。 リリース モードで Render はバンドル全体を 1 つのマークアップ要素を生成します。 切り替えてデバッグからリリースまでモードを次に示すように、web.config の compilation 要素の debug 属性を変更することで実現できます。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

さらに、有効化または最適化を無効にするは、BundleTable.EnableOptimizations プロパティ経由で直接設定できます。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

ファイルがバンドルされているときにまず並べ替えのアルファベット順 (に表示されている方法は、**ソリューション エクスプ ローラー**)。 ライブラリが認識されるため、構成されると、(jQuery、MooTools、Dojo など) にカスタムの拡張機能は最初に読み込まれます。 たとえば、上記のように、Scripts フォルダーをバンドルするため、最終的な順序になります。

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

CSS ファイルもアルファベット順に並べ替えるし、reset.css と normalize.css が、他のファイルの前にできるようにし、再編成します。 前に示した、Styles フォルダーのバンドルの最終的な並べ替えを行うと、これになります。

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Web ホストのパフォーマンスの向上

.NET Framework 4.5 および Windows 8 は、web サーバーのワークロードの大幅なパフォーマンス向上を実現するのに役立つ機能を紹介します。 軽減が含まれます (最大 35%) 両方の起動時間と ASP.NET を使用するサイトをホストしている web のメモリ使用量。

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>主要なパフォーマンスの要因

理想的には、すべての web サイトをアクティブにする必要があり、次の要求に迅速な応答を保証するために、メモリ内たびになった。 サイトの応答性に影響を与える要因は次のとおりです。

- サイト、アプリ プールのリサイクル後に再起動するためにかかる時間。 これは、サイトのアセンブリがメモリ内に不要になった場合に、サイトの web サーバー プロセスの起動にかかる時間です。 (プラットフォーム アセンブリは、メモリ内の他のサイトで使用されているため) です。このような状況は「コールド サイト、フレームワークのウォーム起動」と呼ばれますだけ"コールド サイトまたはスタートアップ"。
- サイトが占有するメモリの量。 この用語は「サイトごとのメモリ使用量」または「ワーキング セットを共有します」

新しいパフォーマンスの向上は、これらの要因の両方に注目します。

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>新しいパフォーマンス機能の要件

新機能に関する要件は、これらのカテゴリに分けることができます。

- .NET Framework 4 で実行される機能を強化します。
- .NET Framework 4.5 を必要とするが、任意のバージョンの Windows で実行できる機能を強化します。
- Windows 8 で実行されている .NET Framework 4.5 でのみ利用できる機能を強化します。

各レベルを有効にすることができる改善のパフォーマンスが向上します。

他のシナリオに適用される広範なパフォーマンス機能の .NET Framework 4.5 の機能強化の一部を利用します。

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>一般的なアセンブリの共有

**要件**: .NET Framework 4 および Visual Studio 11 Developer Preview SDK

サーバー上の別のサイトは、多くの場合、同じヘルパー アセンブリ (たとえば、スターター キットまたはサンプル アプリケーションからのアセンブリ) を使用します。 各サイトの Bin ディレクトリにこれらのアセンブリのコピーには。 アセンブリのオブジェクト コードは同じですが、物理的に独立したアセンブリは、各アセンブリは、サイトのコールド起動時に個別に読み込む必要がありますので、メモリとは別に保持されます。

新しい interning 機能がこの非効率性を解決し、RAM の要件と負荷の両方を削減時間。 インターンにより Windows ファイル システムでは、各アセンブリの 1 つのコピーを保持して、サイトの Bin フォルダーに個別のアセンブリは 1 つのコピーへのシンボリック リンクに置き換えられます。 個々 のサイトでは、アセンブリの異なるバージョンを必要とする場合、シンボリック リンクは、アセンブリの新しいバージョンに置き換え、対象のサイトだけが影響を受けます。

シンボリック リンクを使用してアセンブリを共有するには、aspnet という名前の新しいツールが必要です。\_intern.exe を作成し、隔離されたアセンブリのストアを管理できます。 Visual Studio 11 Developer Preview SDK の一部として提供されます。 (ただし、のみ、.NET Framework 4 がインストールされた、最新版がインストールされていると仮定すると、システムの機能は[更新](https://support.microsoft.com/kb/2468871))。

Aspnet を実行するすべての対象となるアセンブリが確保されていることを確認するには、\_intern.exe 定期的に (たとえば、スケジュールされたタスクとして毎週 1 回)。 一般的な用途は次のとおりです。

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

すべてのオプションを表示するには、引数なしでツールを実行します。

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>マルチコア JIT コンパイルを使用して、すばやい起動時間の

**要件**: .NET Framework 4.5

コールド サイトの起動時では、アセンブリは、ディスクから読み取る必要はだけでなくが、サイトは、JIT コンパイルする必要があります。 複雑なサイトでは、これには、大幅な遅延を追加します。 .NET Framework 4.5 では新しい汎用的な手法では、JIT コンパイルを利用可能なプロセッサ コアに分散させることでこれらの遅延が軽減されます。 これは多くの部分、し、できるだけ早い段階中に収集された情報を使用して以前起動サイトのします。 この機能によって実装される、 [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)メソッド。

ASP.NET では、既定では複数のコアを使用しての JIT コンパイルが有効になっているため、この機能を活用するために何もする必要はありません。 この機能を無効にする場合は、Web.config ファイルで、次の設定を確認します。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>チューニングのガベージ コレクションのメモリを最適化するには

**要件**: .NET Framework 4.5

サイトが実行されている場合、ガベージ コレクター (GC) ヒープの使用、メモリの消費の重要な要素を使用できます。 任意のガベージ コレクターのように、.NET Framework GC では CPU 時間 (頻度とコレクションの有意性) と (新しい、解放された、または解放できるオブジェクトに使用される余分なスペース) のメモリ消費量のバランス。 適切なバランスを実現するために GC を構成する方法のガイダンスを提供して、以前のリリース (たとえばを参照してください[ASP.NET 2.0 および 3.5 共有をホストしている構成](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration))。

サイトの追加のパフォーマンスを提供するすべての GC の以前の推奨設定だけでなく、新しいチューニングできるようにする複数のスタンドアロン設定、ワークロード固有の構成設定ではなく、.NET Framework 4.5 を使用できます。ワーキング セット。

GC メモリ調整を有効にするには、Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config ファイルに次の設定を追加します。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(変更前のガイダンス aspnet.config を使い慣れている場合、は、この設定により、以前の設定を置き換えられることに注意してください。 — たとえば、gcServer、gcConcurrent などを設定する必要はありません。されませんが、以前の設定を削除する。)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Web アプリケーションのプリフェッチ

**要件**: Windows 8 で実行されている .NET Framework 4.5

Windows には複数のリリースと呼ばれるテクノロジが含まれて、[プリフェッチャ](http://en.wikipedia.org/wiki/Prefetcher)アプリケーションの起動時のディスク読み取りのコストを減らすことができます。 コールド起動はクライアント アプリケーションの大部分の問題であるため、このテクノロジいないサーバーに不可欠なコンポーネントのみを含む Windows Server に含まれています。 プリフェッチは、個々 の web サイトの起動を最適化できます、Windows Server の最新バージョンで使用できるようになりました。

Windows server では、既定では、プリフェッチャは有効になっていません。 有効にし、高密度の web ホスティングのプリフェッチャを構成には、コマンドラインで次の一連のコマンドを実行します。

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

次に、ASP.NET アプリケーションでプリフェッチャを統合するには、Web.config ファイルに、次のように追加します。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web フォーム

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>厳密に型指定されたデータ コントロール

ASP.NET 4.5 には Web フォームには、データを操作するためのいくつかの機能強化が含まれています。 最初の向上は、厳密に型指定されたデータ コントロールです。 使用してデータ バインドされた値を表示する前のバージョンの ASP.NET Web フォーム コントロールの*Eval*とデータ バインディング式。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

使用する双方向データ バインド、*バインド*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

、実行時に、これらの呼び出しは、リフレクションを使用して、指定されたメンバーの値を読み取るし、マークアップで結果が表示されます。 この方法を簡単に、自由に unshaped データに対してデータをバインドします。

ただし、このようなデータ バインディング式では、メンバー名 (定義へ移動) などのナビゲーションやコンパイル時にこれらの名前のチェックの IntelliSense などの機能をサポートしません。

この問題を解決するには、ASP.NET 4.5 では、コントロールにバインドされたデータのデータ型を宣言する機能を追加します。 これを行う新しい*ItemType*プロパティ。 2 つの新しい型指定された変数は、データ バインディング式のスコープ内で使用可能なこのプロパティを設定すると:*項目*と*BindItem*します。 変数は厳密に型指定、ため、Visual Studio の開発エクスペリエンスのすべてのメリットを取得します。


双方向のデータ バインディング式を使用して、 *BindItem*変数。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


データ バインディングをサポートする ASP.NET Web フォーム フレームワークのほとんどのコントロールがサポートするために更新されました、 *ItemType*プロパティ。

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>モデル バインド

モデル バインドでは、コードに重点を置いたデータ アクセスを使用する ASP.NET Web フォーム コントロールでのデータ バインディングを拡張します。 概念が組み込まれて、 *ObjectDataSource*コントロールと ASP.NET mvc モデル バインディングから。

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>データの選択

モデル バインドを使用してデータを選択するデータ コントロールを構成するには、コントロールを設定する*SelectMethod*プロパティ ページのコード内のメソッドの名前にします。 データ コントロールでは、ページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、自動的に返されるデータをバインドします。 明示的に呼び出す必要はありません、 *DataBind*メソッド。

次の例では、 *GridView*という名前のメソッドを使用するコントロールが構成されている*GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

作成する、 *GetCategories*ページのコード内のメソッド。 単純な select 操作のメソッド パラメーターは必要ありませんし返す必要があります、 *IEnumerable*または*IQueryable*オブジェクト。 場合、新しい*ItemType*プロパティが設定されます (下で説明したようにできるが、データ バインド式に厳密に型指定[データ コントロールを厳密に型指定された](#_Toc318097386)前)、これらのインターフェイスのジェネリック バージョン返される必要があります: *IEnumerable&lt;T&gt;* または*IQueryable&lt;T&gt;* で、 *T*パラメーターの型と一致する、 *ItemType*プロパティ (たとえば、 *IQueryable&lt;カテゴリ&gt;*)。

次の例のコードを示しています、 *GetCategories*メソッド。 この例では、Northwind サンプル データベースで Entity Framework Code First モデルを使用します。 コードは、クエリが、関連する製品での各カテゴリの詳細を返すことを確認して、 *Include*メソッド。 (これにより、 *TemplateField*マークアップ内の要素では、各カテゴリの製品の数を表示を必要とせず、 [n + 1 選択](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem))。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

ページが実行すると、 *GridView*の制御呼び出し、 *GetCategories*メソッドに自動的に構成されているフィールドを使用して、返されるデータを表示します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Select メソッドを返すため、 *IQueryable*オブジェクト、 *GridView*コントロールが実行する前に、クエリをさらに細かく操作できます。 たとえば、 *GridView*コントロールの並べ替えとページングに返されたクエリ式を追加できます*IQueryable*が実行される前にオブジェクトの基になるによってこれらの操作が実行されるようにLINQ プロバイダーです。 ここでは、Entity Framework では、これらの操作がデータベースで実行される保証されます。

次の例は、 *GridView*コントロールの並べ替えとページングを許可するように変更します。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

これで、ページを実行する際は、コントロールがデータの現在のページのみが表示されることと、選択した列順序付けがあることを確認してようにできます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

返されたデータをフィルター処理、パラメーターを select メソッドに追加する必要があります。 これらのパラメーターは、実行時にモデル バインディングに設定して、それらを使用して、データを返す前にクエリを変更することができます。

たとえば、クエリ文字列のキーワードを入力して、ユーザー フィルター製品を使用するとします。 メソッドにパラメーターを追加およびパラメーターの値を使用するコードを更新できます。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

このコードが含まれています、*場所*式の値が指定されている場合*キーワード*し、クエリの結果を返します。

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>値プロバイダー

前の例が場所に関する特定の値、*キーワード*からパラメーターが登場します。 この情報を示すためには、パラメーターの属性を使用できます。 この例で使用することができます、 *QueryStringAttribute*クラス内にある、 *System.Web.ModelBinding*名前空間。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

これに指示するクエリ文字列から値をバインドしようとするモデル バインド、*キーワード*実行時にパラメーター。 (これを伴う可能性が型の変換を実行するが、ここではない。)値を指定することはできません、型が null 非許容の場合は、例外がスローされます。

これらのメソッドの値のソース値のプロバイダーとして参照され、使用する値プロバイダーを示すパラメーターの属性の値プロバイダー属性として参照されます。 Web フォームには、クエリ文字列、cookie、フォーム値、コントロール、ビュー ステート、セッション状態、およびプロファイルのプロパティなどの Web フォーム アプリケーションで値プロバイダーとすべてのユーザー入力の一般的なソースの対応する属性が含まれます。 カスタム値プロバイダーを記述することもできます。

既定では、パラメーター名は、値プロバイダーのコレクションの値を検索するキーとして使用されます。 キーワードをという名前のクエリ文字列値の検索は、コード例では、(たとえば、~/default.aspx?keyword=chef)。 パラメーター属性に引数として渡すことによって、カスタムのキーを指定できます。 たとえば、q をという名前のクエリ文字列変数の値を使用することがこれを行います。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

このメソッドがページのコードの場合は、ユーザーは、クエリ文字列を使用してキーワードを渡すことによって、結果をフィルター処理できます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

モデル バインドを手動でコーディングする必要がありますそれ以外の場合、多くのタスクを実行します値を読み取る、の null 値の確認、適切な型に変換しようとして、変換が成功したかどうかをチェックおよび最後に、の値を使用して、。クエリ。 モデルのはるかに少ないコードでは、アプリケーション全体で機能を再利用できるように、結果をバインドします。

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>コントロールの値によってフィルター処理

ユーザーがドロップダウン リストからフィルター値を選択できるようにする例を拡張するとします。 マークアップに次のドロップダウン リストを追加してから、もう 1 つのメソッドを使用してそのデータを取得するように構成、 *SelectMethod*プロパティ。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

通常、追加することも、*後*要素を*GridView*コントロール コントロール一致する製品が見つからない場合、メッセージが表示されます。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

ページのコードで、新しい選択ドロップダウン リストのメソッドを追加します。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

最後に、更新、 *GetProducts*ドロップダウン リストから選択したカテゴリの ID を格納する新しいパラメーターを受け取る方法を選択します。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

ユーザーがドロップダウン リストからカテゴリを選択、ページの実行時にこれで、 *GridView*コントロールが自動的にフィルター選択されたデータを表示する再バインドします。 モデル バインドが選択メソッドのパラメーターの値を追跡し、ポストバック後にすべてのパラメーター値が変更されたかどうかを検出したため、可能です。 場合は、モデル バインドはデータに再バインドに関連付けられているデータ コントロールを強制します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>HTML エンコードされたデータ バインディング式

できますようになりましたの HTML エンコード データ バインディング式の結果。 末尾にコロン (:) を追加、 &lt;%# データ バインディング式を示すプレフィックス。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>控え目な検証

クライアント側検証ロジックに控え目な JavaScript を使用する組み込みの検証コントロールを構成できます。 これが大幅にインラインで、ページのマークアップでレンダリングを JavaScript の量が減少し、全体的なページ サイズを縮小します。 控えめな JavaScript の検証コントロールは、次の方法のいずれかで構成できます。

- 次の設定を追加してグローバルに、 *&lt;appSettings&gt;* Web.config ファイル内の要素。 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- 静的なを設定することによってグローバルに*System.Web.UI.ValidationSettings.UnobtrusiveValidationMode*プロパティを*UnobtrusiveValidationMode.WebForms* (通常、*アプリケーション\_開始*Global.asax ファイル内のメソッド)。
- 新しい設定 ページを個別に*UnobtrusiveValidationMode*のプロパティ、*ページ*クラスを*UnobtrusiveValidationMode.WebForms*します。

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5 の更新プログラム

一部の機能強化が HTML5 の新機能を活用するためにサーバー コントロールを Web フォームに加えられました。

- *TextMode*のプロパティ、 *TextBox*のような新しい HTML5 入力タイプをサポートするためにコントロールが更新された*電子メール*、 *datetime*と具合です。
- *FileUpload*なりましたこの HTML5 機能をサポートするブラウザーから複数のファイルのアップロードを制御します。
- 検証コントロールは、今すぐサポート検証 HTML5 入力要素を制御します。
- これで、URL を表す属性を持つ新しい HTML5 の要素をサポートして runat ="server"です。 その結果、規則を使用できます ASP.NET URL のパスのような ~ を表すアプリケーション ルート演算子 (たとえば、&lt;ビデオ runat ="server"src="~/myVideo.wmv"/&gt;)。
- *UpdatePanel*入力フィールドの転記 HTML5 をサポートするコントロールが修正されました。

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta が Visual Studio 11 Beta に含まれています。 ASP.NET MVC は、モデル-ビュー-コント ローラー (MVC) パターンを活用することでテスト可能かつ保守が容易な Web アプリケーションを開発するためのフレームワークです。 ASP.NET MVC 4 では、簡単に、モバイル web アプリケーションを構築することと、任意のデバイスにアクセスできる HTTP サービスの構築に役立つ、ASP.NET Web API が含まれています。 詳細については、次を参照してください。、 [ASP.NET MVC 4 リリース ノート](mvc4-release-notes.md)します。

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web ページ 2

新機能を以下に示します。

- 新規および更新されたサイト テンプレートです。
- サーバー側とクライアント側の検証を使用して追加、*検証*ヘルパー。
- 資産マネージャーを使用するスクリプトを登録する権限です。
- Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。
- 使用してマップを追加する、*マップ*ヘルパー。
- Web ページ アプリケーションでの並列実行されています。
- モバイル デバイス用のページをレンダリングします。

詳細については、これらの機能とページ全体のコード例は、次を参照してください。 [Web ページ 2 のベータ版のベスト機能](https://go.microsoft.com/fwlink/?LinkID=227824)します。

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

このセクションでは、Visual Web Developer の 11 ベータ版および Visual Studio 2012 Release Candidate での web 開発の強化についての情報を提供します。

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Visual Studio 2010 と Visual Studio 2012 リリース候補 (プロジェクト互換性) 間の共有プロジェクト

Visual Studio 2012 Release Candidate、まで Visual Studio の新しいバージョンで既存のプロジェクトを開くと、変換ウィザードが起動します。 これは、でした旧バージョンと互換性のある新しい形式にプロジェクトとソリューションのコンテンツ (資産) のアップグレードを強制します。 そのため、変換した後を開けませんでしたプロジェクト Visual Studio の以前のバージョン。

多くのお客様が意見が適切なアプローチでないこと。 Visual Studio 11 Beta でサポートされる共有のプロジェクトおよび Visual Studio 2010 SP1 を使用したソリューション。 これは、ことを意味 2010年プロジェクトを Visual Studio 2012 Release Candidate を開く場合は引き続き Visual Studio 2010 SP1 でプロジェクトを開くことができません。

> [!NOTE]
> いくつかの種類のプロジェクトは、Visual Studio 2010 SP1 と Visual Studio 2012 リリース候補間で共有できません。 セットアップ プロジェクトの場合) などの特別な目的の一部 (ASP.NET MVC 2 プロジェクト) などの古いプロジェクトまたはプロジェクトが含まれます。

Visual Studio 11 Beta で初めての Visual Studio 2010 SP1 の Web プロジェクトを開くと、次のプロパティは、プロジェクト ファイルに追加されます。

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags、UpgradeBackupLocation、および OldToolsVersion は、プロジェクト ファイルをアップグレードするプロセスによって使用されます。 Visual Studio 2010 でプロジェクトの操作に影響を与えるありません。

VisualStudioVersion は、現在のプロジェクトの Visual Studio のバージョンを示す MSBuild 4.5 で使用される新しいプロパティです。 MSBuild 4.0 (Visual Studio 2010 SP1 を使用する MSBuild のバージョン) でこのプロパティが存在していないため、プロジェクト ファイルに既定値を挿入します。

MSBuildExtensionsPath32 設定によって表されるパスからインポートする正しい .targets ファイルを特定する VSToolsPath プロパティが使用されます。

インポート要素に関連するいくつかの変更もあります。 これらの変更は、Visual Studio の両方のバージョン間の互換性をサポートするために必要です。

> [!NOTE]
> プロジェクトは、2 つの異なるコンピューターに Visual Studio 2010 SP1 と Visual Studio 11 Beta の間で共有されている場合と、プロジェクトでは、アプリでローカルのデータベースが含まれている場合\_データ フォルダー、行う必要があります、データベースで使用される SQL Server のバージョンがあることを確認両方のコンピューターにインストールします。

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>ASP.NET 4.5 web サイト テンプレートの構成の変更

既定値に、次の変更を加え*Web.config* Visual Studio 2012 Release Candidate で web サイト テンプレートを使用して作成されたサイトのファイル。

- `<httpRuntime>`要素、`encoderType`属性が ASP.NET に追加された AntiXSS 型を使用する既定で設定ようになりました。 詳細については、次を参照してください。 [AntiXSS ライブラリ](#_Toc318097382)します。
- さらに、`<httpRuntime>`要素、`requestValidationMode`属性が「4.5」に設定します。 つまり、既定では遅延 (「レイジー」) の検証を使用する要求の検証が構成されます。 詳細については、次を参照してください。 [ASP.NET 要求の検証機能の新しい](#_Toc318097379)します。
- `<modules>`の要素、`<system.webServer>`セクションが含まれていない、`runAllManagedModulesForAllRequests`属性。 (既定値は、false)。つまり版を SP1 に更新されていない IIS 7 を使用している場合は、新しいサイトでのルーティングに関する問題を必要する可能性があります。 詳細については、次を参照してください。 [ASP.NET ルーティング IIS 7 でネイティブ サポート](#Native_Support_In_IIS7_For_ASPNET_Routine)します。

これらの変更は、既存のアプリケーションには影響しません。 ただし、既存の web サイトと新しいテンプレートを使用して ASP.NET 4.5 用に作成した新しい web サイトの動作の違いを表すことができます。

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>ASP.NET ルーティング IIS 7 でネイティブ サポート

これはなく ASP.NET への変更がそのため、SP1 更新プログラムが適用されていない IIS 7 のバージョンを使用している場合の影響を新しい web サイト プロジェクトのテンプレートの変更です。

ASP.NET では、ルーティングをサポートするためにアプリケーションに次の構成設定を追加できます。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

ときに**runAllManagedModulesForAllRequests**ような URL を真になる`http://mysite/myapp/home`があっても、ASP.NET にはない *.aspx*、 *.mvc*、または上のような拡張機能、URL。

IIS 7 に対して行われた更新プログラム、 **runAllManagedModulesForAllRequests**不要な設定と、ASP.NET ルーティングをネイティブにサポートしています。 (更新プログラムについては、Microsoft サポート記事を参照してください[更新プログラムを処理する IIS 7.0 または IIS 7.5 のハンドラーは要求 Url を特定できるように、期間で終わっていないことがある](https://support.microsoft.com/kb/980368)。)。

Web サイトが IIS 7 で実行されると、IIS が更新された場合は設定する必要はありません**runAllManagedModulesForAllRequests**を true にします。 実際には、true に設定するとは使用しないで、不要な処理オーバーヘッドを要求に追加されるためです。 この設定が true の場合のものも含め、すべての要求 *.htm*、 *.jpg*、他の静的ファイルは、ASP.NET 要求パイプラインを介しても移動します。

Visual Studio 2012 RC に用意されているテンプレートを使用して、新しい ASP.NET 4.5 web サイトを作成する場合、web サイトの構成は含まれません、 **runAllManagedModulesForAllRequests**設定します。 これは、既定では、設定が false であることを意味します。

Windows 7 で、SP1 をインストールせず、web サイトを実行し、IIS 7 では、必要な更新プログラムは含まれません。 その結果、ルーティングは動作しませんし、エラーが表示されます。 ルーティングが機能しないという問題があるを場合は、次のいずれかを行うことができます。

- SP1 では、IIS 7 に、更新プログラムを追加するには、Windows 7 を更新します。
- 前の表に、Microsoft サポート記事に記載されている更新プログラムをインストールします。
- 設定**runAllManagedModulesForAllRequests** web サイトの Web.config ファイルで true に設定します。 要求にいくつかのオーバーヘッドのこれが追加されることに注意してください。

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML エディター

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>スマート タスク

デザイン ビューで、ダイアログ ボックスおよびウィザードに設定するが簡単に、多くの場合のサーバー コントロールの複雑なプロパティに関連付けられています。 特別なダイアログ ボックスを使用して、データ ソースを追加するなど、 *Repeater*制御または列を追加する、 *GridView*コントロール。

ただし、この種類の複雑なプロパティの UI のヘルプは、ソース ビューで使用されていませんが。 そのため、Visual Studio 11 では、ソース ビューのスマート タスクを紹介します。 スマート タスクは、c# および Visual Basic エディターでよく使用される機能のコンテキスト対応のショートカットです。

ASP.NET Web フォームのコントロールのスマート タスクに表示されるサーバー タグとして小さいグリフ カーソルが要素の内部がときに。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

スマート タスク グリフをクリックするか、ctrl キーを押しますを展開します。 (ドット)、コード エディターと同じようにします。 デザイン ビューでのスマート タスクと同様のショートカットが表示されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

たとえば、前の図に、スマート タスクは、GridView タスク オプションを示します。 列の編集を選択した場合は、次のダイアログ ボックスが表示されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

入力と同じプロパティ ダイアログ ボックスのセットをデザイン ビューで設定できます。 [Ok] をクリックすると、コントロールのマークアップは、新しい設定で更新されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>待機する ARIA のサポート

アクセス可能な web サイトを作成するには、ますます重要が高まっています。 [WAI ARIA のユーザー補助の標準](http://www.w3.org/WAI/intro/aria)開発者がアクセスできる web サイトを作成する方法を定義します。 この標準が Visual Studio で完全にサポートされています。

たとえば、*ロール*属性が intellisense の全機能を持つようになりました。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

待機する ARIA 標準は、属性が付いているも導入されています。 *aria-* セマンティクスを HTML5 のドキュメントに追加することができます。 これらは、visual Studio が完全にサポート*aria-* 属性。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>新しい HTML5 スニペット

高速かつ一般的に使用される HTML5 マークアップを記述しやすくするには、Visual Studio には、さまざまなスニペットが含まれています。 ビデオのスニペットを例に示します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

IntelliSense で、要素が選択されたときに 2 回は、スニペットを呼び出すには、Tab キーを押します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

これには、カスタマイズ可能なスニペットが生成されます。

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>ユーザー コントロールに展開します。

大規模な web ページでユーザー コントロールに個々 の項目を移動することをお勧めできます。 この形式リファクタリングのでは、ページの読みやすさを高めることができ、ページの構造を簡略化できます。

、ソース ビュー内の Web フォーム ページを編集するときに、簡単にすることができます、ページにテキストを選択するようになりました、右クリックし、抽出をユーザー コントロールを選択します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>属性でコード ナゲット用の IntelliSense

Visual Studio は、任意のページまたはコントロール内のサーバー側コード ナゲットの IntelliSense を常に提供しました。 今すぐ Visual Studio には、HTML の属性もにコード ナゲットの IntelliSense が含まれます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

これにより、簡単にデータ バインディング式を作成します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>自動開始タグまたは終了タグの名前を変更するときの対応タグの名前を変更します。

HTML 要素の名前を変更する場合 (などを変更する、 *div*するタグを*ヘッダー*タグ)、対応する、開くまたは終了タグがリアルタイムでも変更します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

これは、終了タグを変更するか、誤ったマイクを変更するには、忘れたエラーを回避できます。

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>イベント ハンドラーの生成

Visual Studio で、イベント ハンドラーを記述し、それらを手動でバインドするためのソース ビューで機能できるようになりました。 ソース ビューで、イベントの名前を編集する場合は、IntelliSense が表示される&lt;新しいイベントの作成&gt;、正しいシグネチャを持つはイベント ハンドラーを作成、ページのコードにします。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

既定では、イベント ハンドラーはイベント処理メソッドの名前のコントロールの ID を使用します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

結果として得られるイベント ハンドラーを (この場合は、c# で) 次のようになります。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>スマート インデント

空の HTML 要素内で Enter を押すと、エディターは、適切な場所にカーソルを配置します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

この場所で Enter を押すと、終了タグは下に移動し、開始タグと対応するためにインデントします。 カーソルがインデントされます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>ステートメント入力候補の自動減少します。

IntelliSense の一覧のみに関連するオプションを表示するように入力した内容に基づいて Visual Studio 今すぐフィルター:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense リスト内の個々 の単語のタイトル ケースに基づくフィルターも IntelliSense。 たとえば、"dl"を入力する場合は、dl と asp: データの両方が表示されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

この機能によって、高速化を既知の要素の入力候補を取得します。

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript エディター

Visual Studio 2012 Release Candidate の JavaScript エディターがまったく新しくなり、Visual Studio の JavaScript の操作のエクスペリエンスが大幅に向上します。

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>コードのアウトライン表示

アウトライン領域はすると、現在、フォーカスに関連するではないファイルの部分を折りたたむことができます、すべての関数は自動的に作成されました。

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>かっこの一致

開始タグまたは右中かっこにカーソルを配置するときに、エディターには、一致するものが強調表示されます。

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>定義へ移動

定義コマンドへの移動では、関数または変数のソースにジャンプできます。

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>ECMAScript5 のサポート

エディターでは、ECMAScript5、JavaScript 言語を記述する標準の最新バージョンで、新しい構文および Api をサポートします。

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM の IntelliSense

多くの新しい HTML5 Api などのサポートが、DOM Api 用の IntelliSense を強化されています*querySelector*、DOM ストレージ、クロス ドキュメント メッセージング、および*キャンバス*します。 ネイティブ型ライブラリの定義ではなく 1 つの単純な JavaScript ファイルで DOM の IntelliSense はこれで駆動されます。 これにより、簡単に拡張または置換できます。

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC の署名のオーバー ロード

IntelliSense の詳細なコメントは今すぐ新しい JavaScript 関数のオーバー ロードを別々 の宣言された*&lt;署名&gt;* 要素は、この例で示すようにします。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>暗黙的な参照

JavaScript ファイルを暗黙的に含まれるファイルの一覧で指定された JavaScript ファイルまたはブロックの参照、つまりがその内容に IntelliSense を取得しますサーバーの全体の一覧に追加できます。 たとえば、jQuery ファイルをファイルのサーバーの全体の一覧を追加することができ、受け取ります IntelliSense jQuery 関数のファイルの任意の JavaScript ブロックに明示的に参照しているかどうか (を使用して///&lt;参照/&gt;) かどうか。

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS エディター

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>ステートメント入力候補の自動減少します。

CSS プロパティに基づく CSS 今すぐフィルターと、選択したスキーマによってサポートされる値の IntelliSense の一覧です。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense には、タイトルの大文字と小文字の検索もサポートしています。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>階層インデント

CSS エディターは、提供する、カスケードする規則が論理的に編成する方法の概要に階層のルールを表示するのに、インデントに使用します。 次の例では、#list セレクターがリストのカスケード型の子とそのためインデントされます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

次の例より複雑な継承を示します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

ルールのインデントは、その親規則によって決定されます。 階層インデントが既定で有効になっているが、[オプション] ダイアログ ボックス (ツール、メニュー バーのオプション) は無効にできます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS ハックのサポート

現実世界の CSS ファイルの数百の分析には、CSS ハックは非常に一般的なと、Visual Studio で最も広く使用されているものをサポートするようになりましたことが表示されます。 このサポートには、IntelliSense と、星の検証が含まれています (\*)、およびアンダー スコア (\_) プロパティ ハック。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

階層インデントを適用する場合にも維持するためにも、一般的なセレクターのハッキングがサポートされます。 Internet Explorer 7 をターゲットに使用される一般的なセレクターの裏技は、前に、セレクターを付加する *\*::first-child + html*します。 その規則を使用して、階層のインデントを保持します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>ベンダー固有のスキーマ (-moz、webkit)

CSS3 では、異なる時刻でさまざまなブラウザーで実装されている多くのプロパティについて説明します。 これ以前ベンダー固有の構文を使用して、開発者は特定のブラウザーのコードを強制します。 これらのブラウザー固有プロパティは、IntelliSense には含まれるようになりました。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>コメント化とコメント解除のサポート

コメントをして、コード エディター (Ctrl + K、コメントに C と Ctrl + K、コメントを解除する) を使用する同じショートカット キーを使用して CSS ルールのコメントを解除できます。

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>カラー ピッカー

Visual Studio の以前のバージョンでは、色関連の属性用の IntelliSense は、名前付きの色の値のドロップダウン リストを行いました。 そのリストは、全機能装備のカラー ピッカーによって置き換えられました。

色の値を入力すると、カラー ピッカーは自動的が表示され、既定の色パレットを続けて使用されていた色の一覧が表示されます。 マウスまたはキーボードを使用してカラーを選択することができます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

一覧は、完全なカラー ピッカーに展開できます。 ピッカーを使用して、RGBA に不透明度のスライダーを移動すると、任意の色を自動的に変換することでアルファ チャネルを制御できます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>スニペット

CSS エディターのスニペットやすく簡単かつ迅速にクロス ブラウザーのスタイルを作成します。 ブラウザーに固有の設定を必要とする多くの CSS3 プロパティは、スニペットにロールされているようになりました。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS スニペットでシンボルを入力して (CSS3 メディア クエリ) のような高度なシナリオをサポートする (@)、IntelliSense の一覧を示しています。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

選択すると@media値、Tab キーを押すと、CSS エディターは、次のスニペットを挿入します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

コード スニペットと同様、独自の CSS スニペットを作成できます。

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>カスタムのリージョン

コード エディターで使用されている、コード領域をという名前は、CSS の編集に利用できるようになりました。 これにより、関連するスタイル要素を簡単にグループ化することができます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

領域が折りたたまれている領域の名前が表示されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Page Inspector

Page Inspector は、ツール、Visual Studio IDE での web ページ (HTML、Web フォーム、ASP.NET MVC、または Web ページ) を表示し、ソース コードと結果の出力の両方を調べることができます。 ASP.NET ページ、Page Inspector ではサーバー側コードがブラウザーにレンダリングされる HTML マークアップを生成して判断できます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Page Inspector の詳細については、次のチュートリアルを参照してください。

- Page Inspector を使用して[ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Page Inspector を使用して[ASP.NET Web フォーム](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>置換

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>プロファイルを発行する

Visual Studio 2010 では、Web アプリケーション プロジェクトの発行の情報は、バージョン コントロールに格納されていないと、他のユーザーと共有するためのものではありません。 Visual Studio 2012 Release Candidate、発行プロファイルの形式が変更されました。 チームの成果物、公開されているし、MSBuild に基づくビルドから利用することができます。 ビルドの構成情報が設定されて、[発行] ダイアログ ボックスでパブリッシュする前に、ビルド構成を簡単に切り替えることができます。

発行プロファイル PublishProfiles フォルダーに格納されます。 フォルダーの場所はどのようなプログラミング言語を使用しています。

- C#: Properties\PublishProfiles
- Visual Basic: My Project\PublishProfiles

各プロファイルは、MSBuild ファイルです。 発行中には、このファイルは、プロジェクトの MSBuild ファイルにインポートされます。 Visual Studio 2010 で発行またはパッケージのプロセスを変更する場合があるという名前のファイルに、カスタマイズを配置する**ProjectName**。 wpp.targets します。 これはまだサポートされてが、発行プロファイル自体で、カスタマイズを配置できるようになりました。 これにより、そのプロファイルに対してのみ、カスタマイズが使用されます。

活用が MSBuild を使用してプロファイルを発行することもできます。 これを行うには、プロジェクトをビルドするときに、次のコマンドを使用します。

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Project.csproj 値は、プロジェクトのパスと ProfileName は発行プロファイルの名前です。 用のプロファイル名を渡す代わりに、または、 *PublishProfile*プロパティを渡すことができます、完全なパスで発行プロファイルにします。

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET プリコンパイルとマージ

Web アプリケーション プロジェクトでは、Visual Studio 2012 Release Candidate をプリコンパイルし、サイトのコンテンツを発行するときに、またはプロジェクトのパッケージをマージすることができます、パッケージ化/発行の Web プロパティ ページのオプションを追加します。 これらのオプションを参照してください。 し、ソリューション エクスプ ローラーでプロジェクトを右クリックし、プロパティを選択、パッケージ化/発行の Web プロパティ ページを選択します。 次の図は、プリコンパイル オプションを公開する前にこのアプリケーションを示します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

このオプションを選択すると、Visual Studio は、発行するたびに、アプリケーションまたはパッケージ、web アプリケーションをプリコンパイルします。 サイトのプリコンパイル方法またはアセンブリをマージする方法を制御する場合は、これらのオプションを構成する [詳細設定] をクリックします。

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Visual Studio でのテストの web プロジェクトの既定の web サーバーは、IIS Express ようになりました。 Visual Studio 開発サーバーは開発中は、ローカル web サーバーのオプションですが IIS Express は、推奨されるサーバーではようになりました。 IIS Express を使用して、Visual Studio 11 Beta でのエクスペリエンスは、Visual Studio 2010 SP1 で使用するとよく似ています。

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>免責情報

このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。

このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。 マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。

このホワイト ペーパーは、情報提供のみを目的としています。 マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。

お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。 このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。

マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。 別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。

明記されない限り、会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、イベント実在架空のものと関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所、またはイベントは、または推論する必要があります。

(C) 2012 Microsoft Corporation. All rights reserved.

Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。

本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。
