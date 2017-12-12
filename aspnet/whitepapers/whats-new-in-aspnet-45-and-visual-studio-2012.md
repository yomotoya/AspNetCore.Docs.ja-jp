---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: "ASP.NET 4.5 と Visual Studio 2012 の新機能 |Microsoft ドキュメント"
author: rick-anderson
description: "このドキュメントでは、新しい機能と ASP.NET 4.5 で導入された機能強化について説明します。 Web 開発の強化についても説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 93fdc7ca241198dc1d7c4c1f6be0a61b15790039
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>ASP.NET 4.5 と Visual Studio 2012 の新機能
====================
> このドキュメントでは、新しい機能と ASP.NET 4.5 で導入された機能強化について説明します。 Visual Studio 2012 での web 開発の強化についても説明します。 このドキュメントは、2012 年 2 月 29 日に本来パブリッシュされていた。


- [ASP.NET Core ランタイムおよび Framework](#_Toc318097372)

    - [非同期的に読み取ったり書き込んだり HTTP 要求と応答](#_Toc318097373)
    - [HttpRequest 処理の機能強化](#_Toc318097374)
    - [非同期的に応答をフラッシュ](#_Toc318097375)
    - [サポート*await*と*タスク*-ベースの非同期モジュールとハンドラー](#_Toc318097376)
    - [非同期 HTTP モジュール](#_Toc318097377)
    - [非同期 HTTP ハンドラー](#_Toc318097378)
    - [新しい ASP.NET 要求の検証機能](#_Toc318097379)
    - [(「レイジー」) の要求の検証が遅延](#_Toc318097380)
    - [未検証の要求のサポート](#_Toc318097381)
    - [AntiXSS ライブラリ](#_Toc318097382)
    - [Websocket プロトコルのサポート](#_Toc318097383)
    - [バンドルと縮小](#_Toc318097384)
    - [Web ホスト用のパフォーマンスの向上](#_Toc_perf)

        - [主要なパフォーマンスの要因](#_Toc_perf_1)
        - [新しいパフォーマンス機能の要件](#_Toc_perf_2)
        - [一般的なアセンブリを共有](#_Toc_perf_3)
        - [短時間で起動マルチコア JIT のコンパイルを使用します。](#_Toc_perf_4)
        - [メモリ最適化するためにチューニングのガベージ コレクション](#_Toc_perf_5)
        - [Web アプリケーションのプリフェッチ](#_Toc_perf_6)
- [ASP.NET Web フォーム](#_Toc318097385)

    - [厳密に型指定されたデータ コントロール](#_Toc318097386)
    - [モデル バインド](#_Toc318097387)

        - [データの選択](#_Toc318097388)
        - [値プロバイダー](#_Toc318097389)
        - [コントロールの値によってフィルター処理](#_Toc318097390)
    - [データ バインディング式に HTML エンコード](#_Toc318097391)
    - [控えめな検証](#_Toc318097392)
    - [HTML5 の更新プログラム](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 リリース候補](#_Toc318097396)

    - [Visual Studio 2010 と Visual Studio 2012 リリース候補 (プロジェクト互換性) の間で共有プロジェクト](#project-compatibility)
    - [ASP.NET 4.5 web サイト テンプレートの構成の変更](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [ASP.NET ルーティングの IIS 7 でネイティブ サポート](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML エディター](#_Toc318097397)

        - [スマート タスク](#_Toc318097398)
        - [WAI ARIA サポート](#_Toc318097399)
        - [新しい HTML5 スニペット](#_Toc318097400)
        - [ユーザー コントロールに展開します。](#_Toc318097401)
        - [属性のコード ナゲット用の IntelliSense](#_Toc318097402)
        - [自動のタグに対応する開始タグまたは終了タグの名前を変更するときに名前を変更します。](#_Toc318097403)
        - [イベント ハンドラーの生成](#_Toc318097404)
        - [スマート インデント](#_Toc318097405)
        - [ステートメント入力候補の自動縮小](#_Toc318097406)
    - [JavaScript エディター](#_Toc318097407)

        - [コードのアウトライン表示](#_Toc318097408)
        - [中かっこの一致](#_Toc318097409)
        - [定義に移動](#_Toc318097410)
        - [ECMAScript5 サポート](#_Toc318097411)
        - [DOM の IntelliSense](#_Toc318097412)
        - [VSDOC 署名のオーバー ロード](#_Toc318097413)
        - [暗黙の参照](#_Toc318097414)
    - [CSS エディター](#_Toc318097415)

        - [ステートメント入力候補の自動縮小](#_Toc318097416)
        - [階層インデントします。](#_Toc318097417)
        - [CSS ハック サポート](#_Toc318097418)
        - [ベンダー固有のスキーマ (のプロパティ-、- webkit)](#_Toc318097419)
        - [コメントおよびコメントを解除のサポート](#_Toc318097420)
        - [カラー ピッカー](#_Toc318097421)
        - [スニペット](#_Toc318097422)
        - [カスタムの領域](#_Toc318097423)
    - [Page Inspector](#_Toc318097424)
    - [発行](#_Toc318097425)

        - [プロファイルを発行します。](#_Toc318097426)
        - [ASP.NET プリコンパイルとマージ](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [免責事項](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core ランタイムおよび Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>非同期的に読み取ったり書き込んだり HTTP 要求と応答

ASP.NET 4 には、HTTP 要求エンティティとしてを使用してストリームを読み取る機能が導入されました、 *HttpRequest.GetBufferlessInputStream*メソッドです。 このメソッドは、要求のエンティティへのストリーミング アクセスを提供します。 ただし、実行されるが同期的に、要求の期間のスレッドが占有します。

ASP.NET 4.5 は、HTTP 要求エンティティに非同期でストリームを読み取る機能、および非同期的にフラッシュする機能をサポートします。 ASP.NET 4.5 も機能を提供、ダブル バッファー、コント ローラーの統合が容易に .aspx ページのハンドラーと ASP.NET MVC などのダウン ストリームの HTTP ハンドラーを提供する HTTP 要求エンティティにします。

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>HttpRequest 処理の機能強化

ASP.NET 4.5 をによって返されたストリーム参照*HttpRequest.GetBufferlessInputStream*同期および非同期の読み取りメソッドをサポートしています。 *ストリーム*から返されたオブジェクト*GetBufferlessInputStream* BeginRead と EndRead メソッドを実装するようになりました。 非同期の*ストリーム*メソッドを使用して、非同期的に読むだけで要求のエンティティをまとめて、ASP.NET は、非同期の読み取りループの各反復処理の間、現在のスレッドを解放できます。

ASP.NET 4.5 では、バッファー内の方法で要求のエンティティを読み取るためのコンパニオン メソッドも追加されています: *HttpRequest.GetBufferedInputStream*です。 この新しいオーバー ロードはように機能*GetBufferlessInputStream*、同期および非同期の読み取りをサポートします。 ただし、読み取り、 *GetBufferedInputStream*もダウン ストリームのモジュールとハンドラーによりまだ要求のエンティティがアクセスできるようにする ASP.NET 内部バッファーにエンティティのバイトをコピーします。 たとえば場合は、アップ ストリームの一部、パイプライン内のコードは既に読み取ら、要求を使用してエンティティ*GetBufferedInputStream*、使用することもできます*HttpRequest.Form*または*HttpRequest.Files*. これにより、(たとえば、データベースに大きなファイルのアップロードをストリーミング)、要求の非同期処理を実行する実行まだ .aspx ページや ASP.NET の MVC コント ローラーが、後でします。

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>非同期的に応答をフラッシュ

HTTP クライアントに応答を送信すると、クライアントが離れまたは低帯域幅接続時間がかかることができます。 通常、アプリケーションによって作成されると、ASP.NET は応答のバイト数をバッファーします。 ASP.NET は、要求の処理の最後に未収バッファーの 1 つの送信操作を実行します。

バッファー内の応答が大きい (たとえば、クライアントに大きなファイルのストリーミングなど) の場合は、定期的に呼び出す必要があります*HttpResponse.Flush*バッファリングされている出力をクライアントに送信し、管理下にあるメモリ使用率を維持します。 ただし、ため*フラッシュ*繰り返し呼び出し、同期呼び出しは、*フラッシュ*まだ可能性のある実行の時間の長い要求の実行中のスレッドを使用します。

ASP.NET 4.5 を使用して非同期的にフラッシュを実行するのサポートが追加されて、 *BeginFlush*と*EndFlush*のメソッド、 *HttpResponse*クラスです。 これらのメソッドを使用して、非同期のモジュールと段階的にデータを送信するクライアント オペレーティング システムのスレッドを停止せず、非同期のハンドラーを作成できます。 間に*BeginFlush*と*EndFlush*呼び出し、ASP.NET は、現在のスレッドを解放します。 これにより、長時間の HTTP ダウンロードをサポートするために必要なアクティブなスレッドの合計数が大幅に低下します。

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>サポート*await*と*タスク*-ベースの非同期モジュールとハンドラー

.NET Framework 4 と呼ばれる非同期のプログラミング概念を導入された、*タスク*です。 タスクがによって表される、*タスク*型と関連する型を*System.Threading.Tasks*名前空間。 操作を構成するコンパイラの機能強化を使用して、.NET Framework 4.5 がこのビルド*タスク*オブジェクト単純です。 2 つの新しいキーワードをサポートしているコンパイラでは、.NET Framework 4.5 で: *await*と*async*です。 *Await*キーワードは、コードの断片がコードの他のいくつかの部分で非同期的に待機することを示す構文短縮形です。 *Async*キーワードは、タスク ベースの非同期メソッドとしてメソッドを示すために使用できるヒントを表します。

組み合わせ*await*、 *async*、および*タスク*オブジェクトは .NET 4.5 での非同期コードを記述するためのより簡単です。 ASP.NET 4.5 には、非同期 HTTP モジュールと、新しいコンパイラの機能強化を使用して非同期の HTTP ハンドラーを記述することのできる新しい Api を使用したこれらの簡略化がサポートされています。

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>非同期 HTTP モジュール

返すメソッド内での非同期操作を実行する場合について、*タスク*オブジェクト。 次のコード例では、Microsoft のホーム ページをダウンロードする非同期呼び出しが実行される非同期メソッドを定義します。 使用に注意してください、 *async*メソッド シグネチャ内でキーワードおよび*await*への呼び出し*DownloadStringTaskAsync*です。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

記述する必要があるだけです: .NET Framework のダウンロードが完了したら、呼び出し履歴を自動的に復元するとともに、ダウンロードを完了するを待っているときに、呼び出し履歴をアンワインドは自動的に処理します。

これで、非同期の ASP.NET HTTP モジュールでこの非同期メソッドを使用するとします。 ASP.NET 4.5 には、ヘルパー メソッドが含まれています (*EventHandlerTaskAsyncHelper*) と新しいデリゲートの型 (*TaskEventHandler*) 旧バージョンとタスク ベースの非同期メソッドを統合に使用できます。ASP.NET HTTP パイプラインによって公開される非同期プログラミング モデルです。 この例ではどのようにします。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>非同期 HTTP ハンドラー

ASP.NET で非同期のハンドラーを記述するには、従来のアプローチは、実装する、 *IHttpAsyncHandler*インターフェイスです。 ASP.NET 4.5 では、 *HttpTaskAsyncHandler*非同期基本型から派生したことができます、非同期ハンドラーの記述をはるかに簡単になります。

*HttpTaskAsyncHandler*型が抽象であり、オーバーライドする必要があります、 *ProcessRequestAsync*メソッドです。 内部的に ASP.NET を行います戻り値の署名を統合する (、*タスク*オブジェクト) の*ProcessRequestAsync*古い非同期プログラミング モデルで ASP.NET パイプラインを使用します。

次の例は、使用する方法を示しています。*タスク*と*await*非同期 HTTP ハンドラーの実装の一部として。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>新しい ASP.NET 要求の検証機能

既定では、ASP.NET が要求の検証を実行: マークアップやフィールド、ヘッダー、cookie、およびなどのスクリプトを探す要求を調べます。 いずれかが検出された場合、ASP.NET は例外をスローします。 これは潜在的なクロスサイト スクリプティング攻撃に対する防御の最前線として機能します。

ASP.NET 4.5 では、未検証の要求データを選択的に読みやすくします。 ASP.NET 4.5 には、外部ライブラリであったの一般的な AntiXSS ライブラリも統合されています。

開発者が、選択的に、アプリケーションの要求の検証をオフにする機能についてよく寄せられます。 たとえば、アプリケーションは、フォーラム ソフトウェアは、可能性がある場合は、HTML 形式のフォーラムの投稿とコメントの送信がまだ要求の検証は確認が他のすべてであることを確認できるようにします。

ASP.NET 4.5 を選択して検証されていない入力操作を簡単にする 2 つの機能が導入されています: (「レイジー」) の要求の検証および未検証の要求のデータへのアクセスを延期します。

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>(「レイジー」) の要求の検証が遅延

既定を ASP.NET 4.5 で、すべての要求データは要求の検証の対象です。 ただし、要求の検証を延期して、要求データを実際にアクセスするまでアプリケーションを構成することができます。 (この場合もあります呼びます限定的な要求の検証、特定のデータ シナリオの遅延読み込みのような条件に基づいています。)設定して、Web.config ファイルで遅延の検証を使用するアプリケーションを構成することができます、 *requestValidationMode*属性で 4.5 を*httpRUntime*例を次の要素。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

要求の検証モードが 4.5 に設定されている場合、特定の要求の値に対してのみと、コードがその値にアクセスする場合にのみ要求の検証がトリガーされます。 たとえば、次のコードが Request.Form["forum の値を取得\_投稿"]、そのコレクションの要素は、フォームに対してのみ要求の検証が呼び出されます。 内の他の要素のいずれも、*フォーム*コレクションが検証されます。 ASP.NET の以前のバージョンでは、コレクション内の要素にアクセスしたときに要求の検証が要求全体のコレクションをトリガーされました。 新しい動作では、さまざまなアプリケーション コンポーネントを他の部分で要求の検証をトリガーすることがなく、要求データの各部分を見るのやすくなります。

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>未検証の要求のサポート

遅延要求の検証を単独で要求の検証をバイパスする選択的に問題が解決しません。 Request.Form["forum への呼び出し\_投稿"] もトリガーは、その特定の要求値の検証を要求します。 ただし、そのフィールド内のマークアップを許可するために、検証をトリガーすることがなくこのフィールドにアクセスすることができます。

これは、ASP.NET 4.5 は今すぐ未検証データにアクセスする要求をサポートします。 ASP.NET 4.5 が含まれていますが、新しい*Unvalidated*コレクション プロパティで、 *HttpRequest*クラスです。 このコレクションと同様にすべての要求データの共通の値へのアクセスを提供*フォーム*、 *QueryString*、 *Cookie*、および*Url*です。

フォーラムの使用例を使用して、未検証の要求データを読み取ることができる、まず新しい要求の検証モードを使用するアプリケーションを構成します。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

使用してできます、 *HttpRequest.Unvalidated*未検証のフォーム値を読み取るためのプロパティ。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> セキュリティ -*未検証の要求データを使用して、注意してください。* ASP.NET 4.5 には、未検証の要求のプロパティおよびやすく具体的な未検証の要求のデータにアクセスするコレクションが追加されました。 ただし、危険なテキストがユーザーにレンダリングされないことを確認してください、要求の生データに対してカスタム検証を実行することも必要があります。


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>AntiXSS ライブラリ

ASP.NET 4.5 には、AntiXSS ライブラリの利用者数が多いのため、そのライブラリのバージョン 4.0 からエンコード ルーチンの中核となるようになりましたが組み込まれます。

エンコードのルーチンはによって実装される、 *AntiXssEncoder* 、新しい型*System.Web.Security.AntiXss*名前空間。 使用することができます、 *AntiXssEncoder*型に実装されているエンコーディング メソッドのいずれかを呼び出すことによって直接型です。 ただし、新しいアンチ XSS ルーチンを使用するための最も簡単な方法を使用する ASP.NET アプリケーションを構成するのには、 *AntiXssEncoder*既定クラスです。 これを行うには、Web.config ファイルに次の属性を追加します。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

ときに、 *encoderType*を使用する属性を設定、 *AntiXssEncoder*型、すべての出力を新しいエンコード ルーチンを使用して自動的に ASP.NET でのエンコードします。

ASP.NET 4.5 に組み込まれた外部 AntiXSS ライブラリの一部を次に示します。

- *HtmlEncode*、 *HtmlFormUrlEncode*、および*HtmlAttributeEncode*
- *XmlAttributeEncode*と*XmlEncode*
- *UrlEncode*と*UrlPathEncode* (新規)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Websocket プロトコルのサポート

WebSockets プロトコルは、HTTP 経由で、クライアントとサーバー間のセキュリティで保護された、リアルタイムの双方向通信を確立する方法を定義する標準ベースのネットワーク プロトコルです。 Microsoft は、プロトコルの定義を支援する IETF および W3C の両方の標準化団体と連携します。 Websocket プロトコルは、マイクロソフトのクライアントとモバイル オペレーティング システムの両方で WebSockets プロトコルをサポートする多くのリソースを投資と任意のクライアント (だけでなくブラウザー) でサポートされてです。

Websocket プロトコルでは、はるかに簡単に、クライアントとサーバー間の実行時間の長いデータ転送を作成します。 たとえば、チャット アプリケーションの作成ははるかに簡単クライアントとサーバー間の実際の実行時間の長い接続を確立するためです。 定期的なポーリングまたはソケットの動作をシミュレートするために HTTP ロング ポーリングなどの回避策に頼る必要はありません。

ASP.NET 4.5 および IIS 8 を非同期的に読み取ったり書き込んだり文字列およびバイナリ データの両方 Websocket オブジェクトでのマネージ Api を使用する ASP.NET 開発者を有効にすると、低レベルの Websocket のサポートに含まれます。 ASP.NET 4.5 には、新しい*System.Web.WebSockets* WebSockets プロトコルを使用するための型を含む名前空間です。

DOM を作成することで、ブラウザー クライアントが Websocket 接続を確立*WebSocket*を次の例のように、ASP.NET アプリケーションの URL を指すオブジェクト。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

モジュールまたはハンドラーの任意の種類を使用して ASP.NET では、Websocket のエンドポイントを作成できます。 前の例では、.ashx ファイルされたため、.ashx ファイルは、ハンドラーを作成する簡単な方法です。

WebSockets プロトコルに従っては、ASP.NET アプリケーションは、Websocket 要求に、HTTP GET 要求から要求をアップグレードする必要がありますを示すことにより、クライアントの Websocket 要求を受け付けます。 次に例を示します。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest*メソッドは、ASP.NET の現在の HTTP 要求をアンワインドし、制御を関数デリゲートを転送する関数デリゲートを受け取ります。 概念的には、このアプローチを使用する方法に似ていますは*中止*、バック グラウンドで実行されるスレッドの開始デリゲートを定義します。

ASP.NET と、クライアント Websocket ハンドシェイクが正常に完了、ASP.NET は、デリゲートを呼び出し、Websocket アプリケーションが実行を開始します。 次のコード例は、ASP.NET の Websocket のサポートが組み込まれてを使用する単純なエコー アプリケーションを示しています。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

.NET 4.5 でのサポート、 *await*キーワードとタスク ベースの非同期操作は、Websocket のアプリケーションの作成によく適合します。 ASP.NET 内 Websocket 要求で完全に非同期的に実行されるコードの例を示しています。 アプリケーションが呼び出すことによって、クライアントから送信するメッセージを非同期的に待機*ソケットを待機します。ReceiveAsync*です。 呼び出すことによって、クライアントに、非同期メッセージを送信する同様に、*ソケットを待機します。SendAsync*です。

アプリケーションはブラウザーで、Websocket 経由でメッセージを受信する*onmessage*関数。 呼び出すブラウザーから、メッセージを送信する、*送信*のメソッド、 *WebSocket* DOM タイプ、この例で示すようにします。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

今後、この機能の抽象離れた、低レベルのコーディングされているに必要であるこのリリース WebSockets のアプリケーションへの更新プログラムをリリースお可能性があります。

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>バンドルと縮小

バンドルには、1 つのファイルと同様に扱うことができますをバンドルに個別の JavaScript および CSS ファイルを結合することができます。 縮小では、空白と不要なその他の文字を削除することによって JavaScript および CSS ファイルでまとめます。 これらの機能は、Web フォーム、ASP.NET MVC、Web ページを使用します。

バンドルは、バンドル クラスまたは ScriptBundle と StyleBundle その子クラスのいずれかを使用して作成されます。 バンドルのインスタンスを構成した後、バンドルが行われます要求を受信できる単にグローバル BundleCollection インスタンスに追加することで。 既定のテンプレートでは、バンドルの構成を BundleConfig ファイルで実行されます。 この既定の構成では、すべてのコア スクリプトと、テンプレートで使用される css ファイル バンドルを作成します。

バンドルは、いくつかの可能なヘルパー メソッドのいずれかを使用して、ビュー内から参照されます。 デバッグとリリース モードのときにバンドルの異なるマークアップのレンダリングをサポートするために、ScriptBundle および StyleBundle クラス、ヘルパー メソッドを持ち、レンダリングします。 デバッグ モードのときにレンダリングは、バンドル内の各リソースのマークアップを生成します。 リリース モードで、Render はバンドル全体を 1 つのマークアップ要素を生成します。 切り替えてデバッグとリリース間モードを次に示すように、デバッグ、コンパイルの要素の属性 web.config を変更することによって実現できます。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

さらに、有効化または最適化を無効化を BundleTable.EnableOptimizations プロパティ経由で直接設定できます。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

ファイルは、バンドルされているときに最初に並べ替えたアルファベット順 (に表示されている方法**ソリューション エクスプ ローラー**)。 ライブラリが認識されるため、構成されると (jQuery、MooTools、Dojo など) にカスタムの拡張機能は最初に読み込まれます。 たとえば、上記のように、Scripts フォルダーのバンドルの最終的な順序になります。

1. jquery 1.6.2.js
2. jquery ui.js
3. jquery.tools.js
4. a.js

CSS ファイルのもアルファベット順に並べ替えおよび reset.css と normalize.css は、他のファイルの前にできるようにし、再編成します。 前に示した、Styles フォルダー バンドルの最終的な並べ替えを行うと、これになります。

1. reset.css
2. content.css
3. forms.css
4. globals.css
5. menu.css
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Web ホスト用のパフォーマンスの向上

.NET Framework 4.5 および Windows 8 は、web サーバーのワークロードの大幅なパフォーマンスの向上を実現するのに役立つ機能を紹介します。 これはパフォーマンスが低下が含まれます (35%) web サイトで ASP.NET を使用するホストのメモリ使用量と両方の起動時にします。

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>主要なパフォーマンスの要因

理想的には、すべての web サイトをアクティブにする必要があります、および、次の要求に迅速な応答を保証するメモリ内するたびに提供します。 サイトの応答性に影響を与える要因は次のとおりです。

- アプリケーション プールのリサイクル後に再起動するサイトにかかる時間です。 これは、サイトのアセンブリがメモリ内になくなると、サイトの web サーバー プロセスを起動するまでにかかる時間です。 (プラットフォーム アセンブリは、メモリ内の他のサイトで使用されているため) です。このような状況は「コールド サイト、ウォーム framework 起動」と呼ばれますだけ「コールド サイトが起動します」または。
- サイトが占有するメモリの量。 この用語は、「サイトごとのメモリ消費量」または「ワーキング セットを共有します」

新しいパフォーマンスの向上は、これらの要因の両方に注目します。

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>新しいパフォーマンス機能の要件

新機能に関する要件は、これらのカテゴリに分けることができます。

- .NET Framework 4 で実行される機能を強化します。
- .NET Framework 4.5 を必要とするが、任意のバージョンの Windows で実行できる機能を強化します。
- Windows 8 で実行されている .NET Framework 4.5 でのみ利用できる機能を強化します。

パフォーマンスは、有効にすることができる向上のレベルごとに増加します。

.NET Framework 4.5 の機能強化の一部も他のシナリオに適用される広範なパフォーマンス機能を活用します。

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>一般的なアセンブリを共有

**要件**: .NET Framework 4 および Visual Studio 11 開発者プレビュー SDK

サーバー上の別のサイトは、多くの場合、同じヘルパー アセンブリ (たとえば、スターター キットまたはサンプル アプリケーションからアセンブリ) を使用します。 各サイトでは、その Bin ディレクトリでこれらのアセンブリのコピーを含んでいます。 アセンブリのオブジェクト コードが同じであって物理的に独立したアセンブリは、各アセンブリは、コールド サイトの起動中に個別に読み込まれるようにしてメモリ内で個別に保持します。

インターンの新しい機能は、この非効率を解決し、RAM の要件と負荷の両方が減少時間。 インターン処理により Windows ファイル システムに各アセンブリの 1 つのコピーを保持して、サイトの Bin フォルダー内の個々 のアセンブリは 1 つのコピーへのシンボリック リンクに置き換えられます。 個々 のサイトでは、アセンブリの異なるバージョンを必要とする場合、シンボリック リンクは、アセンブリの新しいバージョンに置き換えし、対象のサイトだけが影響を受けます。

シンボリック リンクを使用してアセンブリを共有するには、aspnet をという名前の新しいツールが必要です。\_intern.exe、作成し、アセンブリのインターン処理後のストアを管理できます。 Visual Studio 11 開発者プレビュー SDK の一部として提供されます。 (ただし、のみ、.NET Framework 4 がインストールされて、最新版をインストールした場合のあるシステムで動作[更新](https://support.microsoft.com/kb/2468871))。

Aspnet を実行するすべての対象となるアセンブリが確保されていることを確認するに\_intern.exe 定期的に (たとえば、スケジュールされたタスクとして毎週 1 回)。 一般的な使用方法は次のとおりです。

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

すべてのオプションを表示するには、引数なしで、ツールを実行します。

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>短時間で起動マルチコア JIT のコンパイルを使用します。

**要件**: .NET Framework 4.5

コールド サイト スタートアップ、アセンブリでは、ディスクから読み取るだけでなく、サイトは、JIT でコンパイルする必要があります。 複雑なサイトでは、これには、大幅な遅延を追加します。 .NET Framework 4.5 で新しい汎用的な手法では、JIT コンパイルを利用可能なプロセッサ コア間で分散させることによってこれらの遅延が軽減されます。 これは多くの部分、し、できるだけ早い段階中に収集される情報を使用して以前起動サイトのします。 この機能によって実装される、 [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/en-us/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)メソッドです。

ASP.NET では、既定では複数のコアを使用しての JIT コンパイルが有効になっているため、この機能を活用するために何もする必要はありません。 この機能を無効にする場合は、次の設定を Web.config ファイルで行います。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>メモリ最適化するためにチューニングのガベージ コレクション

**要件**: .NET Framework 4.5

サイトが実行されていると、ガベージ コレクター (GC) ヒープの使用は、メモリの消費に重要な要因を指定できます。 ガベージ コレクターはすべてと同様には、.NET Framework GC は、CPU 時間 (頻度とコレクションの有意性) とメモリ消費量 (新しい、解放された、または解放できないオブジェクトに使用される余分なスペース) 間のトレードオフをなります。 適切なバランスを実現するために、GC を構成する方法についてガイダンスを提供して以前のリリースの (たとえばを参照してください[ASP.NET 2.0/3.5 共有をホストしている構成](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration))。

サイトの追加のパフォーマンスを提供するすべての GC の以前の推奨設定に加え、新しいチューニングできるようにする複数のスタンドアロンの設定、ワークロードで定義されている構成設定ではなく、.NET Framework 4.5 を使用できます。ワーキング セット。

GC メモリ調整を有効にするには、Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config ファイルに次の設定を追加します。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Aspnet.config への変更前のガイダンスは、使い慣れたなら、この設定が古い設定を置き換えることに注意してください: たとえば、gcServer、gcConcurrent などを設定する必要はありません。ありません古い設定を削除します。)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Web アプリケーションのプリフェッチ

**要件**: Windows 8 で実行されている .NET Framework 4.5

Windows にはいくつかのリリースと呼ばれるテクノロジが含まれる、[プリフェッチャ](http://en.wikipedia.org/wiki/Prefetcher)アプリケーションの起動時のディスクの読み取りにかかるコストを削減するためです。 コールド起動は主にクライアント アプリケーションの問題であるため、このテクノロジが組み込まれていませんでした Windows Server は、サーバーに不可欠なコンポーネントのみが含まれます。 プリフェッチは個々 の web サイトの起動を最適化、Windows Server の最新バージョンで使用できるようになりました。

Windows Server のプリフェッチャは既定で有効にできません。 を有効にして、高密度の web ホスティングのためプリフェッチャを構成するには、コマンドラインで次の一連のコマンドを実行します。

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

その後、プリフェッチャを ASP.NET アプリケーションを統合するには、Web.config ファイルに、次のように追加します。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web フォーム

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>厳密に型指定されたデータ コントロール

ASP.NET 4.5 では、Web フォームには、データを操作するためのいくつかの機能強化が含まれています。 最初の向上は、厳密に型指定されたデータ コントロールです。 以前のバージョンの ASP.NET Web フォーム コントロールを表示するデータ バインドされた値を使用して、 *Eval*とデータ バインディング式。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

使用する、双方向データ バインディングの*バインド*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

、実行時に、これらの呼び出しは、リフレクションを使用して、指定されたメンバーの値を読み取るし、マークアップで結果が表示されます。 この方法によりしやすくなります任意、unshaped データに対してデータをバインドします。

ただし、次のようにデータ バインド式では、メンバー名、ナビゲーションのような定義へ移動)、またはコンパイル時のこれらの名前のチェックの IntelliSense などの機能をサポートしません。

この問題に対処するには、ASP.NET 4.5 は、コントロールにバインドされるデータのデータ型を宣言する機能を追加します。 こうと、new を使用して*ItemType*プロパティです。 このプロパティを設定するときに 2 つの新しい型指定された変数は、使用可能なスコープ内のデータ バインディング式:*項目*と*BindItem*です。 変数は厳密に型指定するために、Visual Studio 開発環境のすべてのメリットを取得します。


双方向のデータ バインディング式を使用して、 *BindItem*変数。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


データ バインディングをサポートする ASP.NET Web フォーム フレームワークのほとんどのコントロールをサポートするために更新されている、 *ItemType*プロパティです。

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>モデル バインディング

モデル バインドは、コード中心のデータ アクセスを使用する ASP.NET Web フォーム コントロールでのデータ バインディングを拡張します。 概念が組み込まれています、 *ObjectDataSource*コントロールと ASP.NET mvc モデル バインディングの間です。

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>データの選択

バインディングを使用してモデル データを選択するデータ コントロールを構成するには、コントロールを設定*SelectMethod*プロパティ ページのコード内のメソッドの名前にします。 データ コントロールは、ページのライフ サイクルの適切なタイミングでメソッドを呼び出すし、自動的に返されるデータをバインドします。 明示的に呼び出す必要はありません、 *DataBind*メソッドです。

次の例で、 *GridView*という名前のメソッドを使用するコントロールが構成されている*GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

作成する、 *GetCategories*ページのコード内のメソッドです。 単純な選択操作のメソッドはパラメーターを必要し、返す必要があります、 *IEnumerable*または*IQueryable*オブジェクト。 場合、新しい*ItemType*プロパティの設定 (有効にする厳密に型指定、データ バインディング式で説明した方法[データ コントロールを厳密に型指定された](#_Toc318097386)以前)、これらのインターフェイスのジェネリック バージョン返される必要があります: *IEnumerable&lt;T&gt;* または*IQueryable&lt;T&gt;*で、 *T*型と一致するパラメーター、 *ItemType*プロパティ (たとえば、 *IQueryable&lt;カテゴリ&gt;*)。

次の例のコードを示しています、 *GetCategories*メソッドです。 この例では、Northwind サンプル データベースで Entity Framework Code First モデルを使用します。 コードは、クエリがによって各カテゴリに関連する製品の詳細を返すことを確認して、 *Include*メソッドです。 (これにより、 *TemplateField*マークアップ内の要素では、各カテゴリの製品の数を表示を必要とせず、 [n + 1 選択](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem))。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

ページを実行すると、 *GridView*通話を制御、 *GetCategories*メソッドに自動的に構成されているフィールドを使用して、返されたデータを表示。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Select メソッドが返されるため、 *IQueryable*オブジェクト、 *GridView*コントロールでは、クエリを実行する前にさらに細かく操作ことができます。 たとえば、 *GridView*コントロールでの並べ替えとページングに返されたクエリ式を追加できます*IQueryable*これらの操作は、基になるによって実行されるように、実行される前に、のオブジェクトLINQ プロバイダーです。 この例では、Entity Framework では、これらの操作は、データベースで実行されますを確認します。

次の例は、 *GridView*コントロール並べ替えとページングを許可するように変更します。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

これで、ページを実行する際、コントロールがデータの現在のページのみが表示されていると、選択した列が並べされていることを確認してようにできます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

返されたデータをフィルター処理、パラメーターを select メソッドに追加する必要があります。 これらのパラメーターは、実行時に、モデル バインディングによって設定し、それらを使用して、データを返す前にクエリを変更することができます。

たとえば、クエリ文字列のキーワードを入力してユーザーのフィルターの製品を使用するとします。 メソッドにパラメーターを追加して、パラメーター値を使用するコードを更新することができます。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

このコードが含まれています、*場所*式には、値を指定してください。 場合*キーワード*し、クエリの結果を返します。

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>値プロバイダー

前の例でした場所に関する特定の値、*キーワード*パラメーターの由来します。 この情報を示すためには、パラメーター属性を使用することができます。 この例で使用することができます、 *QueryStringAttribute*は、クラス、 *System.Web.ModelBinding*名前空間。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

これに指示するクエリ文字列から値をバインドしようとするモデル バインド、*キーワード*実行時にパラメーター。 (これが含まれます型変換を実行するが、ここではない。)値を指定することはできません、型が null 非許容の場合は、例外がスローされます。

これらのメソッドの値のソースし、呼ばれ値プロバイダーを使用する値プロバイダーを示すパラメーターの属性の値プロバイダー属性と呼びます。 Web フォームには、クエリ文字列、cookie、フォームの値、コントロール、ビュー状態、セッション状態、およびプロファイルのプロパティなどの Web フォーム アプリケーションで値プロバイダーとすべてのユーザー入力の典型的なソースの対応する属性が含まれます。 カスタム値プロバイダーを記述することもできます。

既定では、パラメーター名は、値プロバイダーのコレクションに値を検索するキーとして使用します。 例では、コードはキーワードをという名前のクエリ文字列値になります (たとえば、~/default.aspx?keyword=chef)。 パラメーターの属性に引数として渡すことによって、カスタムのキーを指定できます。 たとえば、q をという名前のクエリ文字列変数の値を使用することがこれを行います。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

このメソッドは、ページのコードでは場合、ユーザーは、クエリ文字列を使用して、キーワードを渡すことによって、結果をフィルター処理できます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

モデル バインドは手動でコーディングする必要がありますそれ以外の場合の多くのタスクを実行します値を読み取る、の null 値を確認、適切な型に変換中に、変換が成功したかどうかをチェックおよび最後に、の値を使用して、。クエリ。 モデルのはるかに少ないコードでは、アプリケーション全体の機能を再利用できるように結果をバインドします。

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>コントロールの値によってフィルター処理

ユーザーがフィルター値はドロップダウン リストから選択できるようにするように例を拡張するとします。 マークアップに次のドロップダウン リストを追加してから、別のメソッドを使用してそのデータを取得するように構成、 *SelectMethod*プロパティ。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

通常追加することも、 *EmptyDataTemplate*要素を*GridView*コントロールは、一致する製品が見つからない場合、メッセージが表示できるようにします。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

ページのコードでは、ドロップダウン リストの方法を選択して、新しいを追加します。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

最後に、更新、 *GetProducts*ドロップダウン リストから選択したカテゴリの ID を格納する新しいパラメーターを受け取る方法を選択します。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

これで、ユーザーがドロップダウン リストからカテゴリを選択、ページを実行すると、 *GridView*コントロールのフィルター選択されたデータを表示する自動的に再バインドがします。 これにはモデル バインディングが選択メソッドのパラメーターの値を追跡し、ポストバック後にパラメーター値が変更されたかどうかを検出したため。 場合は、モデル バインディングはデータを再バインドに関連付けられたデータ コントロールを強制します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>データ バインディング式に HTML エンコード

できます。 ここでの HTML エンコード データ バインディング式の結果します。 末尾にコロン (:) を追加、&lt;データ バインディング式をマークする %# プレフィックス。

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>控えめな検証

クライアント側の検証ロジックの控えめな JavaScript を使用する組み込みの検証コントロールを構成できます。 大幅に量が減り、JavaScript、ページ マークアップ内にインライン表示し、全体的なページ サイズを削減します。 控えめな JavaScript の検証コントロールは、以下の方法のいずれかで構成できます。

- 次の設定を追加してグローバルに、  *&lt;appSettings&gt;*  Web.config ファイル内の要素。 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- グローバルにするには、静的な*System.Web.UI.ValidationSettings.UnobtrusiveValidationMode*プロパティを*UnobtrusiveValidationMode.WebForms* (では通常、*アプリケーション\_開始*Global.asax ファイル内のメソッド)。
- 新しい設定してページを個別に*UnobtrusiveValidationMode*のプロパティ、*ページ*クラスを*UnobtrusiveValidationMode.WebForms*です。

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5 の更新プログラム

一部の機能強化に加えられた Web フォーム サーバー コントロールの HTML5 の新機能を利用します。

- *TextMode*のプロパティ、 *TextBox*と同様に、新しい HTML5 入力型をサポートするためにコントロールが更新された*電子メール*、 *datetime*、およびします。
- *ファイルアップロード*制御この HTML5 機能をサポートするブラウザーから複数のファイルのアップロードをサポートするようになりました。
- 検証コントロールは、今すぐサポート検証 HTML5 入力要素を制御します。
- ここで URL を表す属性を持つ新しい HTML5 要素をサポートして runat ="server"です。 その結果、規則を使用できます ASP.NET URL のパスと同様に、~ アプリケーション ルートを表す演算子 (たとえば、&lt;ビデオ runat ="server"src="~/myVideo.wmv"/&gt;)。
- *UpdatePanel*転記 HTML5 入力フィールドをサポートするコントロールが修正されました。

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta が、Visual Studio 11 Beta に含まれています。 ASP.NET MVC は、モデル ビュー コント ローラー (MVC) パターンを活用することで高度なテスト機能と保守が容易な Web アプリケーションを開発するためのフレームワークです。 ASP.NET MVC 4 では、モバイル web アプリケーションを構築するが容易し、任意のデバイスに到達可能な HTTP サービスを構築するのに役立つ、ASP.NET Web API が含まれています。 詳細については、次を参照してください。、 [ASP.NET MVC 4 リリース ノート](mvc4-release-notes.md)です。

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web ページ 2

新機能を以下に示します。

- 新規および更新されたサイト テンプレート。
- サーバー側とクライアント側の検証を使用して追加する、*検証*ヘルパー。
- 資産マネージャーを使用してスクリプトを登録する権限です。
- Facebook と OAuth および OpenID を使用して他のサイトからのログインを有効にします。
- 使用したマップを追加する、*マップ*ヘルパー。
- Web ページのアプリケーションによってサイドで実行されています。
- モバイル デバイス用のページを表示します。

詳細については、これらの機能とページ全体のコード例については、次を参照してください。 [Web Pages 2 のベータ版で、上部機能](https://go.microsoft.com/fwlink/?LinkID=227824)します。

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

このセクションでは、Visual Web Developer 11 Beta と Visual Studio 2012 リリース候補版での web 開発機能強化についての情報を提供します。

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Visual Studio 2010 と Visual Studio 2012 リリース候補 (プロジェクト互換性) の間で共有プロジェクト

Visual Studio 2012 リリース候補、まで、新しいバージョンの Visual Studio で既存のプロジェクトを開くと、変換ウィザードが起動します。 これは、でした。 旧バージョンと互換性のある新しい形式にプロジェクトとソリューションのコンテンツ (資産) のアップグレードを強制します。 そのため、変換した後にするを開けませんでした、プロジェクトの Visual Studio の以前のバージョン。

多くの顧客が意見が適切な方法でないこと。 Visual Studio 11 のベータ版のサポート プロジェクトおよび Visual Studio 2010 SP1 でのソリューションを共有します。 つまりこと場合 2010年プロジェクトを開くには、Visual Studio 2012 リリース候補に、まだされます Visual Studio 2010 SP1 でプロジェクトを開くことができません。

> [!NOTE]
> いくつかの種類のプロジェクトは、Visual Studio 2010 SP1 と Visual Studio 2012 リリース候補間で共有できません。 セットアップ プロジェクトの場合) などの特殊な目的の一部 (ASP.NET MVC 2 プロジェクト) などの旧バージョンのプロジェクトまたはプロジェクトが含まれます。

Visual Studio 11 Beta で最初に Visual Studio 2010 SP1 の Web プロジェクトを開くときに、次のプロパティは、プロジェクト ファイルに追加されます。

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags、UpgradeBackupLocation、および OldToolsVersion は、プロジェクト ファイルをアップグレードするプロセスによって使用されます。 Visual Studio 2010 でプロジェクトの操作への影響があるありません。

VisualStudioVersion は、現在のプロジェクトの Visual Studio のバージョンを示す MSBuild 4.5 で使用される新しいプロパティです。 このプロパティは、MSBuild 4.0 (Visual Studio 2010 SP1 を使用する MSBuild のバージョン) に存在していない、ためのプロジェクト ファイルに既定値を挿入します。

VSToolsPath プロパティは MSBuildExtensionsPath32 設定によって表されるパスからインポートする正しい .targets ファイルを特定するために使用します。

インポート要素に関連するいくつかの変更もあります。 これらの変更が、両方のバージョンの Visual Studio の間の互換性をサポートするために必要です。

> [!NOTE]
> プロジェクトでは、アプリで、ローカル データベースが含まれている場合、プロジェクトは 2 つの異なるコンピューターに Visual Studio 2010 SP1 と Visual Studio 11 Beta の間で共有されている場合と\_データ フォルダー、する必要があります、データベースで使用される SQL Server のバージョンがあることを確認両方のコンピューターにインストールされます。

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>ASP.NET 4.5 web サイト テンプレートの構成の変更

既定値に、次の変更が加えられた*Web.config* Visual Studio 2012 リリース候補に web サイト テンプレートを使用して作成されたサイトのファイル。

- `<httpRuntime>`要素、`encoderType`属性は既定でに設定を ASP.NET に追加された AntiXSS の種類を使用します。 詳細については、「 [AntiXSS ライブラリ](#_Toc318097382)です。
- また、`<httpRuntime>`要素、`requestValidationMode`属性が「4.5」に設定します。 これは、既定では、要求の検証が構成されている遅延 (「レイジー」) の検証を使用することを意味します。 詳細については、「 [ASP.NET 要求の検証機能の新しい](#_Toc318097379)です。
- `<modules>`の要素、`<system.webServer>`セクションが含まれていない、`runAllManagedModulesForAllRequests`属性。 (既定値は、false は) です。つまりが SP1 に更新されていない IIS 7 のバージョンを使用している場合、いる可能性があります、新しいサイトでルーティングの問題。 詳細については、次を参照してください。 [ASP.NET ルーティングの IIS 7 でネイティブ サポート](#Native_Support_In_IIS7_For_ASPNET_Routine)です。

これらの変更は、既存のアプリケーションには影響しません。 ただし、既存の web サイトと、新しいテンプレートを使用して ASP.NET 4.5 用に作成した新しい web サイトの動作の違いを表すことができます。

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>ASP.NET ルーティングの IIS 7 でネイティブ サポート

これはなく ASP.NET などの変更、SP1 更新プログラムが適用されていない IIS 7 のバージョンを使用している場合の影響を新しい web サイト プロジェクトのテンプレートに変更します。

ASP.NET では、ルーティングをサポートするためにアプリケーションを次の構成設定を追加できます。

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

ときに**runAllManagedModulesForAllRequests**は true、ような URL`http://mysite/myapp/home`があっても、ASP.NET にはない*.aspx*、 *.mvc*、またはで同様の拡張機能、URL です。

IIS 7 に加えられた更新プログラム、 **runAllManagedModulesForAllRequests**不要な設定と、ASP.NET ルーティングでネイティブにサポートしています。 (更新プログラムについては、マイクロソフト サポートの記事を参照してください[更新プログラムを処理する IIS 7.0 または IIS 7.5 のハンドラーの要求の Url を特定の有効期間で終わっていないことがある](https://support.microsoft.com/kb/980368))。

IIS 7 で web サイトが実行されると、IIS が更新された場合は設定する必要はありません**runAllManagedModulesForAllRequests** true に設定します。 実際には、true に設定するとは使用しないで、不要な処理オーバーヘッド要求に追加されるためです。 この設定が true の場合を含め、すべての要求*.htm*、 *.jpg*、その他の静的ファイル経由させることも、ASP.NET 要求パイプラインとします。

Visual Studio 2012 RC に用意されているテンプレートを使用して、新しい ASP.NET 4.5 web サイトを作成する場合、web サイトの構成は含まれません、 **runAllManagedModulesForAllRequests**設定します。 つまり、既定では、設定がある場合は false。

Windows 7 で、SP1 をインストールせず、web サイトを実行し、IIS 7 では、必要な更新プログラムは含まれません。 その結果、ルーティングが機能しなくなり、エラーが表示されます。 ルーティングが機能しないという問題があるを場合は、次のいずれかを行うことができます。

- SP1 では、IIS 7 に、更新プログラムを追加するには、Windows 7 を更新します。
- 前の表に、Microsoft サポート記事で説明されている更新プログラムをインストールします。
- 設定**runAllManagedModulesForAllRequests**その web サイトの Web.config ファイルで true に設定します。 これが要求に一部のオーバーヘッドを追加は注意してください。

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML エディター

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>スマート タスク

デザイン ビューで、ダイアログ ボックスとウィザードに設定するが簡単に、多くの場合のサーバー コントロールの複合プロパティに関連付けられています。 ダイアログ ボックスを使用して、データ ソースを追加するなど、*リピータ*制御または列を追加する、 *GridView*コントロール。

ただし、この種類の複合プロパティ UI のヘルプは、ソース ビューで使用されていないがします。 そのため、Visual Studio 11 は、ソース ビューのスマート タスクを紹介します。 スマート タスクは、一般的に使用される機能では、c# および Visual Basic エディターのコンテキストに対応するショートカットです。

ASP.NET Web フォーム コントロールのスマート タスクに表示されるサーバー タグとして小さなグリフ カーソルが要素内にある場合。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

スマート タスク グリフをクリックするか、ctrl キーを展開します。 (ドット)、コード エディターと同様です。 デザイン ビューで、スマート タスクに類似するショートカットが表示されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

たとえば、前の図に、スマート タスクには、GridView タスク オプションが表示されます。 列の編集を選択すると、次のダイアログ ボックスが表示されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

同じプロパティ ダイアログ ボックスのセットを読み込むデザイン ビューで設定できます。 [Ok] をクリックすると、コントロールのマークアップが、新しい設定で更新されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>WAI ARIA サポート

アクセス可能な web サイトを作成するには、ますます重要が高まっています。 [WAI ARIA ユーザー補助に関する標準](http://www.w3.org/WAI/intro/aria)開発者がアクセスできる web サイトを記述する方法を定義します。 この標準が Visual Studio で完全にサポートされています。

たとえば、*ロール*属性が完全 IntelliSense になりました。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

WAI ARIA 標準が付いている属性も導入されています。 *aria-*のセマンティクスが HTML5 ドキュメントに追加するためです。 これらは、visual Studio が完全にサポート*aria-*属性。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>新しい HTML5 スニペット

処理速度と一般的に使用される HTML5 マークアップの記述を簡単にするには、Visual Studio には、スニペットの数が含まれています。 例では、ビデオのスニペットを示します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

呼び出すには、スニペットを IntelliSense での要素が選択されている場合に 2 回 Tab キーを押します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

これには、カスタマイズできるスニペットが生成されます。

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>ユーザー コントロールに展開します。

大規模な web ページで、ユーザー コントロールの個々 の項目を移動することをお勧めできます。 リファクタリングのこのフォームでは、ページの読みやすさを高めることができ、ページの構造を簡略化できます。

このしやすくするには、ソース ビューで Web フォーム ページを編集するときに、することができます、ページにテキストを選択するようになりました、右クリックし、ユーザー コントロールに展開 を選択します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>属性のコード ナゲット用の IntelliSense

Visual Studio では、任意のページまたはコントロール内のサーバー側コード ナゲットの IntelliSense が用意常にします。 これで Visual Studio には、HTML 属性もコード ナゲットの IntelliSense が含まれます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

これにより、簡単にデータ バインディング式を作成します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>自動のタグに対応する開始タグまたは終了タグの名前を変更するときに名前を変更します。

HTML 要素の名前を変更する場合 (たとえば、変更する、 *div*するタグ、*ヘッダー*タグ) では、対応する開く、または終了タグがリアルタイムでも変化します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

これは、終了タグを変更するか、誤ったマイクを変更するには、忘れたエラーを回避できます。

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>イベント ハンドラーの生成

Visual Studio には、イベント ハンドラーを記述し、手動でバインドするためのソース ビューの機能が含まれています。 ソース ビュー内のイベントの名前を編集する場合は、IntelliSense によって表示されます&lt;新しいイベントの作成&gt;、右側のシグネチャを持ちますイベント ハンドラーを作成、ページのコードでを。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

既定では、イベント ハンドラーはイベント処理メソッドの名前のコントロールの ID を使用します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

結果として得られるイベント ハンドラーを (この場合は、c# で) 次のようになります。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>スマート インデント

空の HTML 要素の中に内部 Enter キーを押したときに、エディターは適切な場所にカーソルを置きます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

この場所で Enter キーを押した場合、終了タグを下に移動し、開始タグと対応するためにインデントします。 カーソルがインデントされます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>ステートメント入力候補の自動縮小

Visual Studio の今すぐフィルターのみに関連するオプションを表示するように入力した内容に基づいてで IntelliSense の一覧:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense が IntelliSense リスト内の個々 の単語のタイトル ケースに基づくフィルターもします。 たとえば、"dl"を入力すると、配布リストと asp: データの両方が表示されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

この機能によって、高速の既知の要素の入力候補を取得します。

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript エディター

Visual Studio 2012 リリース候補に JavaScript エディターが完全に新しいと、Visual Studio の JavaScript の使用のエクスペリエンスを大幅に向上させます。

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>コードのアウトライン表示

アウトライン領域はいない、現在のフォーカスに関連するファイルの部分を縮小することができます、すべての関数は自動的に作成されました。

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>中かっこの一致

始めまたは終わりかっこにカーソルを配置するときに、エディターには、一致するものが強調表示されます。

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>定義へ移動

Go to Definition コマンドでは、関数または変数のソースにジャンプすることができます。

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>ECMAScript5 サポート

エディターは、ECMAScript5、JavaScript 言語を表す標準の最新バージョンに新しい構文および Api をサポートします。

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM の IntelliSense

多くの新しい HTML5 Api などのサポートが、DOM Api 用の IntelliSense を強化されています*querySelector*、DOM の記憶域、クロス ドキュメントのメッセージング、および*キャンバス*です。 ネイティブ タイプ ライブラリの定義ではなく 1 つの単純な JavaScript ファイルでは、DOM の IntelliSense はドリブンようになりました。 これにより、簡単に拡張または置換します。

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC 署名のオーバー ロード

IntelliSense の詳細なコメントは今すぐ、new を使用して JavaScript 関数の別のオーバー ロードの宣言された*&lt;署名&gt;*要素は、この例で示すようにします。

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>暗黙の参照

JavaScript ファイルを暗黙的に含まれるファイルの一覧で指定された JavaScript ファイルまたはブロックの参照、つまりがそのコンテンツの IntelliSense を利用しますサーバーの全体の一覧に追加できます。 たとえば、jQuery ファイルをファイルの集約型の一覧を追加することができ、します IntelliSense jQuery 関数の任意のファイルの JavaScript ブロックで明示的に参照しているかどうか (を使用して///&lt;参照/&gt;) か。

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS エディター

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>ステートメント入力候補の自動縮小

CSS プロパティに基づく CSS 今フィルターと、選択したスキーマによってサポートされる値の IntelliSense リスト。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense には、タイトル ケース検索もサポートしています。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>階層インデント

CSS エディターを使用してインデント階層のルールを表示するカスケード型のルールが論理的に編成する方法の概要を説明します。 次の例で、#list セレクター リストのカスケード子である、したがってインデントされます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

次の例より複雑な継承を示します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

ルールのインデントは、その親規則によって決定されます。 既定では、階層インデントが有効になっているが、オプション ダイアログ ボックス (ツール、メニュー バーのオプション) は無効にできます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS ハック サポート

現実世界の CSS ファイルの数百の分析で表示される CSS ハッキングが非常に一般的なおよび Visual Studio で最も広く使用されているものをサポートするようになりました。 このサポートには、IntelliSense と星の検証が含まれています (\*)、およびアンダー スコア (\_) プロパティ ハッキング。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

適用される場合でも階層インデントを維持するためにも、一般的なセレクター ハッキングがサポートされます。 Internet Explorer 7 をターゲットに使用される一般的なセレクター ハッキングが前にセレクターでを付加するが *\*::first-child + html*です。 その規則を使用すると、階層のインデントを保持します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>ベンダー固有のスキーマ (プロパティ-、- webkit)

CSS3 では、異なる時刻でさまざまなブラウザーで実装されている多くのプロパティについて説明します。 これ以前ベンダー固有の構文を使用して、開発者は特定のブラウザーのコードを強制します。 これらのブラウザーに固有のプロパティは IntelliSense に追加されました。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>コメントおよびコメントを解除のサポート

コメントをコード エディター (Ctrl + K、コメントに C と Ctrl + K、コメントを解除する) で使用する同じショートカットを使用して CSS 規則のコメントを解除できます。

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>カラー ピッカー

以前のバージョンの Visual Studio では、色関連の属性に対する IntelliSense は、名前付きの色の値のドロップダウン リストで構成されています。 そのリストは、多機能なカラー ピッカーによって置き換えられました。

色の値を入力すると、カラー ピッカーは自動的が表示されは既定の色パレットを続けて使用していた色の一覧が表示されます。 マウスまたはキーボードを使用して色を選択することができます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

リストは、完全なカラー ピッカーに展開できます。 ピッカーを使用して、不透明度のスライダーを移動すると、RGBA に任意の色を自動的に変換してアルファ チャネルを制御できます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>スニペット

CSS エディターのスニペットやすく簡単かつ迅速にクロス ブラウザーのスタイルを作成します。 ブラウザーに固有の設定を必要とする多くの CSS3 プロパティは、スニペットにロールされているようになりました。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS のスニペットでシンボルを入力して (CSS3 メディア クエリ) のような高度なシナリオをサポートする (@)、IntelliSense の一覧が示されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

選択すると@media値し Tab キーを押して、CSS エディターは、次のスニペットを挿入します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

同様に、コード スニペットは、独自の CSS スニペットを作成できます。

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>カスタムの領域

コード領域は、コード エディターで使用されている、という名前は、CSS を編集するために使用できるようになりました。 これにより、関連するスタイル ブロックを簡単にグループ化できます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

領域が折りたたまれた領域の名前が表示されます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Page Inspector

Page Inspector は、ツール、Visual Studio IDE での web ページ (HTML、Web フォーム、ASP.NET MVC または Web ページ) を表示し、ソース コードと、結果の出力の両方を調べることができます。 ASP.NET ページの Page Inspector ではサーバー側コードによって生成されてブラウザーにレンダリングされる HTML マークアップを確認できます。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Page Inspector の詳細については、次のチュートリアルを参照してください。

- Page Inspector を使用して[ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Page Inspector を使用して[ASP.NET Web フォーム](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>置換

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>プロファイルを発行する

Visual Studio 2010 で Web アプリケーション プロジェクトの情報を公開はバージョン管理に保存されていないと、他のユーザーと共有するためのものではありません。 Visual Studio 2012 リリース候補、発行プロファイルの形式が変更されました。 チームの成果物が行われ、MSBuild に基づいて、ビルドから利用することができます。 ビルド構成情報が設定されて、[発行] ダイアログ ボックスで発行する前にビルド構成を簡単に切り替えることができます。

発行プロファイル PublishProfiles フォルダーに格納されます。 フォルダーの場所が依存しているプログラミング言語に使用しています。

- C# の場合: Properties\PublishProfiles
- Visual Basic: My Project\PublishProfiles

各プロファイルは、MSBuild ファイルです。 発行時に、このファイルは、プロジェクトの MSBuild ファイルにインポートされます。 Visual Studio 2010 で発行またはパッケージのプロセスに変更を加える場合があるという名前のファイルに、カスタマイズを格納する**ProjectName**。 wpp.targets です。 これは引き続きサポートされますが、発行プロファイル自体に、カスタマイズを配置できるようになりました。 このように、カスタマイズは、そのプロファイルに対してのみ使用されます。

活用が MSBuild を使用してプロファイルを発行できます。 これを行うには、プロジェクトをビルドするときに、次のコマンドを使用します。

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Project.csproj 値は、プロジェクトのパスであり ProfileName とは、発行するプロファイルの名前。 プロファイル名を渡すのではなく、代わりに、 *PublishProfile*プロパティを渡すことができます、完全なパスで、発行プロファイルをします。

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET プリコンパイルとマージ

Web アプリケーション プロジェクトでは、Visual Studio 2012 リリース候補をプリコンパイルし、サイトのコンテンツを発行するときに、またはパッケージ、プロジェクトのマージをできるようにパッケージ化/発行 Web プロパティ ページのオプションを追加します。 これらのオプションを表示するには、ソリューション エクスプ ローラーでプロジェクトを右クリックして、プロパティを選択およびパッケージ化/発行の Web プロパティ ページを選択します。 次の図は、プリコンパイル オプションを発行する前にこのアプリケーションを示します。

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

このオプションを選択すると、Visual Studio は、発行するたびにアプリケーションまたはパッケージ web アプリケーションをプリコンパイルします。 サイトのプリコンパイル方法またはアセンブリにマージする方法を制御する場合は、これらのオプションを構成する高度なボタンをクリックします。

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Visual Studio でのテストの web プロジェクトの既定の web サーバーは、IIS Express ようになりました。 Visual Studio 開発サーバー オプションですが、ローカル web サーバーの開発中が IIS Express、推奨されるサーバー。 IIS Express を使用して、Visual Studio 11 Beta でのエクスペリエンスは、Visual Studio 2010 SP1 で使用するとよく似ています。

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>免責情報

このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。

このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。 マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。

このホワイト ペーパーは、情報提供のみを目的としています。 マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。

お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。 このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。

マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。 別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。

別途記載されていない場合、本ドキュメントで使用している会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、出来事などの名称は架空のものです。実在する会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、出来事などを意図したものではなく、推論されるべきでもありません。

(C) 2012 Microsoft Corporation. All rights reserved.

Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。

本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。
