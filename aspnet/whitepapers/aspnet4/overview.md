---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 および Visual Studio 2010 の Web 開発の概要 |Microsoft ドキュメント
author: rick-anderson
description: このドキュメントでは、.net Framework 4 におけると Visual Studio 2010 に含まれている ASP.NET の新機能の多くの概要を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 6ce52c387ff835eda46bc1882b8b974889e2d4af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2018
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 および Visual Studio 2010 の Web 開発の概要
====================
> このドキュメントでは、.net Framework 4 におけると Visual Studio 2010 に含まれている ASP.NET の新機能の多くの概要を示します。
> 
> [このホワイト ペーパーをダウンロードします。](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**目次**

**[Core Services](#0.2__Toc253429238 "_Toc253429238")**  
[Web.config ファイルがリファクタリング](#0.2__Toc253429239 "_Toc253429239")  
[拡張可能な出力キャッシュ](#0.2__Toc253429240 "_Toc253429240")  
[Web アプリケーションの自動開始](#0.2__Toc253429241 "_Toc253429241")  
[永続的なリダイレクト ページ](#0.2__Toc253429242 "_Toc253429242")  
[セッション状態を圧縮](#0.2__Toc253429243 "_Toc253429243")  
[許容される Url の範囲を拡大](#0.2__Toc253429244 "_Toc253429244")  
[拡張可能な要求の検証](#0.2__Toc253429245 "_Toc253429245")  
[オブジェクト キャッシュとキャッシュ機能拡張、オブジェクトを](#0.2__Toc253429246 "_Toc253429246")  
[拡張可能な HTML、URL、および HTTP ヘッダーのエンコーディング](#0.2__Toc253429247 "_Toc253429247")  
[1 つのワーカー プロセス内の個々 のアプリケーションのパフォーマンスを監視](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[含まれる Web フォームと MVC の jQuery](#0.2__Toc253429251 "_Toc253429251")  
[コンテンツ配信ネットワーク サポート](#0.2__Toc253429252 "_Toc253429252")  
[ScriptManager の明示的なスクリプト](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Page.MetaKeywords と Page.MetaDescription プロパティをメタ タグを設定](#0.2__Toc253429257 "_Toc253429257")  
[個々 のコントロール ビュー ステートを有効にする](#0.2__Toc253429258 "_Toc253429258")  
[ブラウザーの機能に変更](#0.2__Toc253429259 "_Toc253429259")  
[ASP.NET 4 でルーティング](#0.2__Toc253429260 "_Toc253429260")  
[クライアント Id を設定](#0.2__Toc253429261 "_Toc253429261")  
[データ コントロールで行の選択を保持する](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET Chart Control](#0.2__Toc253429263 "_Toc253429263")  
[QueryExtender コントロールにデータをフィルタ リング](#0.2__Toc253429264 "_Toc253429264")  
[コード式に html エンコード](#0.2__Toc253429265 "_Toc253429265")  
[テンプレートの変更をプロジェクト](#0.2__Toc253429266 "_Toc253429266")  
[CSS の機能強化](#0.2__Toc253429267 "_Toc253429267")  
[Div 要素を非表示のフィールドを非表示にする](#0.2__Toc253429268 "_Toc253429268")  
[テンプレート コントロールの外部テーブルをレンダリング](#0.2__Toc253429269 "_Toc253429269")  
[ListView コントロールの機能強化](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList と RadioButtonList コントロールの機能強化](#0.2__Toc253429271 "_Toc253429271")  
[メニュー コントロールの機能強化](#0.2__Toc253429272 "_Toc253429272")  
[ウィザードおよび CreateUserWizard コントロール 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[領域サポート](#0.2__Toc253429275 "_Toc253429275")  
[データ注釈属性の検証のサポート](#0.2__Toc253429276 "_Toc253429276")  
[テンプレート化されたヘルパー](#0.2__Toc253429277 "_Toc253429277")

**[動的データ](#0.2__Toc253429278 "_Toc253429278")**  
[既存のプロジェクトの動的なデータの有効化](#0.2__Toc253429279 "_Toc253429279")  
[宣言型 DynamicDataManager コントロール構文](#0.2__Toc253429280 "_Toc253429280")  
[エンティティ テンプレート](#0.2__Toc253429281 "_Toc253429281")  
[Url と電子メール アドレスの新しいフィールド テンプレート](#0.2__Toc253429282 "_Toc253429282")  
[DynamicHyperLink コントロールとのリンクを作成する](#0.2__Toc253429283 "_Toc253429283")  
[データ モデルにおける継承のサポート](#0.2__Toc253429284 "_Toc253429284")  
[多対多のリレーションシップ (Entity Framework) のサポート](#0.2__Toc253429285 "_Toc253429285")  
[コントロールの表示と列挙のサポートを新しい属性](#0.2__Toc253429286 "_Toc253429286")  
[フィルターのサポート強化](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web 開発の改良](#0.2__Toc253429288 "_Toc253429288")**  
[CSS の互換性を改善](#0.2__Toc253429289 "_Toc253429289")  
[HTML および JavaScript スニペット](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript IntelliSense の機能強化](#0.2__Toc253429291 "_Toc253429291")

**[Web アプリケーションの配置を Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**  
[パッケージを web](#0.2__Toc253429293 "_Toc253429293")  
[Web.config 変換](#0.2__Toc253429294 "_Toc253429294")  
[データベース配置](#0.2__Toc253429295 "_Toc253429295")  
[Web アプリケーション用の 1 回のクリックを公開](#0.2__Toc253429296 "_Toc253429296")  
[Resources](#0.2__Toc253429297 "_Toc253429297")

**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>コア サービス

ASP.NET 4 には、いくつかの出力キャッシュおよびセッション状態の記憶域などの ASP.NET コア サービスを向上させる機能が導入されています。

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Web.config ファイルのリファクタリング

`Web.config`構成を含むファイル、Web アプリケーションが大きく成長過去のいくつかリリースの .NET Framework 経由でように新しい機能が追加されました、Ajax などのルーティング、および IIS 7 との統合。 これはで、構成するか、Visual Studio のようなツールを使用せずに新しい Web アプリケーションを起動するは困難です。 .NET Framework 4、主要な構成要素を移動されています、`machine.config`ファイル、およびアプリケーションを今すぐこれらの設定を継承します。 これにより、`Web.config`を空にするのにまたは線は次だけ、Visual Studio のアプリケーションが対象とする framework のバージョンを指定します。 これを含む ASP.NET 4 アプリケーション内のファイル。

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>拡張可能な出力キャッシュ

以来、ASP.NET 1.0 がリリースされた、出力キャッシュが有効になっているページ、コントロール、および HTTP 応答の生成された出力をメモリに格納する開発者です。 以降の Web 要求で ASP.NET 提供できますコンテンツより迅速に最初から出力を再生成する代わりに、メモリから生成された出力を取得することで。 ただし、このアプローチには、制限、生成されるコンテンツは常にメモリに格納されますが、大量のトラフィックが発生しているサーバーに出力キャッシュによって消費されるメモリは Web アプリケーションの他の部分からのメモリの要求に競合します。

ASP.NET 4 では、1 つまたは複数のカスタム出力キャッシュ プロバイダーを構成することができますを出力キャッシュの機能拡張ポイントを追加します。 出力キャッシュ プロバイダーは、任意のストレージ機構を使用して、HTML コンテンツを保持します。 これを多様な永続化のメカニズムは、ローカルまたはリモート ディスクを含めることができます、クラウドの記憶域のカスタム出力キャッシュ プロバイダーを作成できるようになり、分散キャッシュ エンジン。

新しいから派生するクラスとしてカスタム出力キャッシュ プロバイダーを作成する*System.Web.Caching.OutputCacheProvider*型です。 プロバイダーを構成することができますし、`Web.config`新しいを使用してファイル*プロバイダー*のサブセクション、 *outputCache*要素は、次の例で示すように。

[!code-xml[Main](overview/samples/sample2.xml)]

ASP.NET 4 ですべての HTTP 応答では、既定では、ページを表示し、前の例で示すように、コントロールが、メモリ内の出力キャッシュを使用している、 *defaultProvider* AspNetInternalProvider に属性が設定されています。 別のプロバイダー名を指定して Web アプリケーションの使用既定の出力キャッシュ プロバイダーを変更することができます*defaultProvider*です。

さらに、要求ごと、およびコントロールごとに別の出力キャッシュ プロバイダーを選択することができます。 別の Web ユーザー コントロールの別の出力キャッシュ プロバイダーを選択する最も簡単な方法は、new を使用して、宣言を行う、 *providerName*制御のディレクティブでは、次の例で示すように属性します。

[!code-aspx[Main](overview/samples/sample3.aspx)]

HTTP 要求に対する別の出力キャッシュ プロバイダーを指定するには、少しの作業が必要です。 プロバイダーを宣言によって指定するには、代わりに上書きする新しい*GetOuputCacheProviderName*メソッドで、`Global.asax`ファイルをプログラムで、特定の要求を使用するプロバイダーを指定します。 その方法を次の例に示します。

[!code-csharp[Main](overview/samples/sample4.cs)]

ASP.NET 4 に出力キャッシュ プロバイダー拡張機能の追加により、Web サイトのようになりましたより積極的なとより高度な出力キャッシュの戦略を試みることができます。 たとえば、ディスク上のトラフィックが低いを取得するページをキャッシュ中に、メモリ内のサイトの「トップ 10」ページをキャッシュすることはようになりました。 代わりに、キャッシュ、レンダリングされるページのすべての変更によって組み合わせが、分散キャッシュを使用して、メモリ使用量は、フロント エンド Web サーバーからオフロードされるようにします。

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Web アプリケーションの自動開始

一部の Web アプリケーションは、大量のデータを読み込んだり、最初の要求を処理する前に処理負荷のかかる初期化を実行する必要があります。 以前のバージョンの ASP.NET では、このような状況にする必要がありました脱するよう、ASP.NET アプリケーションおよび時に初期化コードを実行するカスタムの認証方法を検討、*アプリケーション\_ロード*メソッドで、 `Global.asax`ファイルです。

という名前の新しいスケーラビリティ機能*自動開始*ことアドレスこのシナリオでは使用可能な直接 ASP.NET 4 で実行すると Windows Server 2008 R2 に IIS 7.5 です。 Auto-start 機能は、アプリケーション プールを開始、ASP.NET アプリケーションの初期化および HTTP 要求を受け入れるのための制御方法を提供します。

> [!NOTE] 
> 
> IIS 7.5 用の IIS アプリケーションのウォーム アップ モジュール
> 
> IIS チームには、IIS 7.5 のアプリケーションのウォーム アップ モジュールの最初のベータ版のテスト バージョンがリリースされました。 これにより、前に説明したよりも簡単にはアプリケーションを準備します。 カスタム コードを記述する代わりに、Web アプリケーションがネットワークからの要求を受け入れる前に実行するリソースの Url を指定します。 IIS サービスの起動中に発生したこのウォーム アップ (として IIS アプリケーション プールを構成した場合*AlwaysRunning*) および IIS ワーカー プロセスがリサイクルされるときにします。 、リサイクル中には、以前の IIS ワーカー プロセスは、アプリケーションなしの中断または unprimed キャッシュのためには、その他の問題が発生するように、新しく起動されたワーカー プロセスのウォーム アップ、が完全にするまで要求を実行し続けます。 このモジュールは、ASP.NET、バージョン 2.0 以降の任意のバージョンで動作する注意してください。
> 
> 詳細については、次を参照してください。 [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) IIS.net Web サイトです。 ウォーム アップ機能を使用する方法について説明するチュートリアルでは、次を参照してください。 [IIS 7.5 アプリケーション ウォーム アップ モジュールの使用を開始する](https://www.iis.net/learn/manage)IIS.net Web サイトです。


自動開始機能を使用して、IIS 管理者では、次の構成を使用して自動的に起動する IIS 7.5 におけるアプリケーション プールを設定、`applicationHost.config`ファイル。

[!code-xml[Main](overview/samples/sample5.xml)]

次の構成を使用して自動的に開始する個々 のアプリケーションを指定する単一のアプリケーション プールには、複数のアプリケーションが含まれていることができます、ため、`applicationHost.config`ファイル。

[!code-xml[Main](overview/samples/sample6.xml)]

IIS 7.5 がで情報を使用して IIS 7.5 サーバーは、コールドによって開始されたときに、または個々 のアプリケーション プールがリサイクルされるときに、`applicationHost.config`ファイルを確認してどの Web アプリケーションは自動的に開始します。 Auto-start 用にマークされている各アプリケーションの IIS7.5 は ASP.NET 4 アプリケーションを起動するアプリケーション一時的に受け付けない状態で HTTP 要求に要求を送信します。 ASP.NET がによって定義された型をインスタンス化でこの状態であるときに、 *serviceAutoStartProvider* (ように、前の例で示した部分) の属性とそのパブリック エントリ ポイントへの呼び出しです。

実装することによって必要なエントリ ポイントとマネージの自動開始の種類を作成する、 *、IProcessHostPreloadClient*インターフェイス、次の例で示すようにします。

[!code-csharp[Main](overview/samples/sample7.cs)]

初期化後にコードが実行されます、*プリロード*メソッドおよびメソッドが戻る、ASP.NET アプリケーションが要求を処理できる状態にします。

.5 IIS および ASP.NET 4 に自動開始の追加により、最初の HTTP 要求を処理する前に、負荷の高いアプリケーションの初期化を実行するための適切に定義されたアプローチがあるようになりました。 たとえば、アプリケーションを初期化して、アプリケーションが初期化され、HTTP トラフィックを受け入れる準備ができているロード バランサーを通知する新しい自動開始機能を使用することができます。

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>永続的なリダイレクト ページ

を超えて繰り返し発生する検索エンジンに古いリンクの基になる時間の経過と共にページおよびその他のコンテンツの周囲を移動する Web アプリケーションでよくあることを勧めします。 ASP.NET では、開発者が従来古い Url への要求を使用して処理を使用して、 *Response.Redirect*新しい URL に要求を転送する方法です。 ただし、*リダイレクト*メソッドは、ユーザーが、古い Url にアクセスしようとしています。 余分な http ラウンド トリップにより、HTTP 302 検出 (一時的なリダイレクト) 応答を発行します。

ASP.NET 4 で追加、新しい*RedirectPermanent*容易問題 HTTP 301 にヘルパー メソッドは恒久的次の例のように、応答。

[!code-csharp[Main](overview/samples/sample8.cs)]

検索エンジンと永続的なリダイレクトを認識できる他のユーザー エージェントは、一時的なリダイレクトのブラウザーによって行われた、不要なラウンド トリップが不要になるため、コンテンツに関連付けられている新しい URL を格納します。

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>セッション状態の圧縮

ASP.NET には、Web ファーム上でセッション状態を格納するための 2 つの既定オプションが用意されています。 セッション状態プロバイダー、アウト プロセス セッション状態サーバーを呼び出すと、Microsoft SQL Server データベースにデータを格納するセッション状態プロバイダー。 両方のオプションは、Web アプリケーションのワーカー プロセスの外部の状態情報を格納する、ために、リモート記憶域に送信される前にシリアル化するセッション状態があります。 、開発者は、セッション状態で保存される情報量によって、シリアル化データのサイズが非常に大きいまで拡大できます。

ASP.NET 4 では、両方のアウト プロセス セッション状態プロバイダーの新しい圧縮オプションが導入されています。 ときに、 *compressionEnabled*に次の例に示すように構成オプションが設定されている*true*ASP.NET は圧縮 (および圧縮解除) .NET Frameworkを使用してシリアル化されたセッションの状態*System.IO.Compression.GZipStream*クラスです。

[!code-xml[Main](overview/samples/sample9.xml)]

単純な新しい属性を追加すると、`Web.config`ファイル、Web サーバー上の予備の CPU サイクルを持つアプリケーションは、シリアル化されたセッション状態データのサイズを大幅に削減を実現できます。

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>許容される Url の範囲を拡張

ASP.NET 4 には、アプリケーションの Url のサイズを拡大するための新しいオプションが導入されています。 ASP.NET の以前のバージョンには、NTFS ファイル パスの制限に基づく、260 文字までに URL パスの長さが制限されます。 ASP.NET 4 がある (増減) この制限として、アプリケーションの適切なオプションを使用して 2 つの新しい*httpRuntime*構成属性。 次の例では、これらの新しい属性を示します。

[!code-xml[Main](overview/samples/sample10.xml)]

長いまたは短いパス (プロトコル、サーバー名、およびクエリ文字列を含まない URL の部分) を許可するのには、変更、 *[済みの maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* 属性。 長いまたは短いクエリ文字列を許可するには、値を変更、 *[済みの maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* 属性。

ASP.NET 4 では、URL 文字チェックで使用される文字を構成することもできます。 ASP.NET には、URL のパス部分の無効な文字が検出されると、これは、要求を拒否し、HTTP 400 エラーを発行します。 ASP.NET の以前のバージョンでは、URL の文字のチェックは、文字の固定セットに制限されていました。 ASP.NET 4 では、new を使用して有効な文字のセットをカスタマイズできます*requestPathInvalidChars*の属性、 *httpRuntime*構成要素を次の例で示すようにします。

[!code-xml[Main](overview/samples/sample11.xml)]

既定では、 <em>requestPathInvalidChars</em>属性は無効として 8 文字を定義します。 (に割り当てられている文字列で<em>requestPathInvalidChars</em>既定では<em>、</em>より小さい (&lt;)、大なり (&gt;)、およびアンパサンド (&amp;) 文字は、エンコードされたため、`Web.config`ファイルは XML ファイルです)。無効な文字のセットは、必要に応じてカスタマイズできます。

> [!NOTE]
> ASP.NET 4 は 0x00 ~ 0x1F の ASCII の範囲の文字を含む URL のパスを常に拒否の IETF RFC 2396 で定義されているは URL の無効な文字であるため注意してください ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt))。 IIS 6 を実行する Windows Server のバージョンにまたは以降では、http.sys プロトコルのデバイス ドライバーに自動的に拒否 Url これらの文字でします。


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>拡張可能な要求の検証

ASP.NET 要求の検証は、クロスサイト スクリプト (XSS) 攻撃によく使用される文字列の入力方向の HTTP 要求データを検索します。 潜在的な XSS 文字列が見つかった場合、要求の検証は、問題のある文字列のフラグを設定し、エラーを返します。 組み込みの要求の検証は、XSS 攻撃で使用される最も一般的な文字列が見つかった場合にのみ、エラーを返します。 誤検知が多すぎます XSS 検証をより積極的に実行する前の試行が発生しました。 ただし、環境では、比較的余裕のない、または逆にすることも XSS を意図的に緩和される要求の検証は、特定のページまたは特定の種類の要求を確認することがあります。

ASP.NET 4 では、要求の検証機能が行われて拡張可能な要求のカスタム検証ロジックを使用できるようにします。 新しいから派生するクラスを作成する要求の検証を拡張する*System.Web.Util.RequestValidator*型とするアプリケーションの構成 (で、 *httpRuntime* のセクション`Web.config`ファイル) をカスタムの型を使用します。 次の例では、要求のカスタム検証クラスを構成する方法を示します。

[!code-xml[Main](overview/samples/sample12.xml)]

新しい*requestValidationType*属性には、標準 .NET Framework 型識別子を指定する文字列をカスタム要求の検証を提供するクラスが必要です。 各要求に対しては、ASP.NET は、受信 HTTP 要求データの各部分を処理するカスタム型を呼び出します。 受信 URL、すべての HTTP ヘッダー (クッキーとカスタム ヘッダーの両方)、およびエンティティ ボディはすべて次の例に示すようなカスタム要求の検証クラスによって検査で利用できます。

[!code-csharp[Main](overview/samples/sample13.cs)]

入力方向の HTTP データの一部を検査しない場合、要求の検証クラスが切り替えられる ASP.NET 既定の要求の検証だけで呼び出すことによって実行できるようにする*ベースです。IsValidRequestString です。*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>オブジェクト キャッシュとキャッシュ機能拡張オブジェクト

最初のリリース以降、ASP.NET の強力なメモリ内オブジェクト キャッシュが含まれる (*System.Web.Caching.Cache*)。 キャッシュの実装は非常に普及以外の Web アプリケーションで使用されてきました。 ただし、これはへの参照を含むように Windows フォームや WPF アプリケーションの不適切な`System.Web.dll`ASP.NET オブジェクトのキャッシュを使用できるだけにします。

キャッシュ使用できるようにするすべてのアプリケーションに対して、.NET Framework 4 には、新しいアセンブリ、新しい名前空間、いくつかの基本型、および実装をキャッシュする具象型が導入されています。 新しい`System.Runtime.Caching.dll`アセンブリには、新しいキャッシュ API が含まれている、 *System.Runtime.Caching*名前空間。 名前空間には、クラスの 2 つのコア セットが含まれています。

- カスタムのキャッシュ実装の任意の型を構築するための基盤を提供する抽象型です。
- 具体的なメモリ内のオブジェクトのキャッシュ実装 (、 *System.Runtime.Caching.MemoryCache*クラス)。

新しい*MemoryCache* ASP.NET キャッシュで密接にモデル化されたクラスと、ASP.NET とエンジンの内部キャッシュ ロジックの多くを共有します。 パブリックのキャッシュ Api *System.Runtime.Caching* ASP.NET を使用している場合に、カスタム キャッシュの開発をサポートするために更新された*キャッシュ*オブジェクトでの一般的な概念が見つかります、新しい Api です。

新しいの詳しい説明*MemoryCache*はクラスと基本 Api をサポートするドキュメント全体が必要です。 ただし、次の例は、新しいキャッシュ API のしくみの概要を把握します。 例は、任意の依存関係なしの Windows フォーム アプリケーション向けに書かれた`System.Web.dll`です。

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>拡張可能な HTML、URL、および HTTP ヘッダーのエンコーディング

ASP.NET 4 では、次の一般的なテキスト エンコーディング タスクのカスタム エンコード ルーチンを作成できます。

- HTML エンコード。
- URL エンコードします。
- HTML 属性エンコード。
- 出力方向の HTTP ヘッダーのエンコーディング。

カスタム エンコーダーを作成するには、新しいから派生することによって*System.Web.Util.HttpEncoder*にカスタムの型を使用する型と ASP.NET を構成、 *httpRuntime*のセクションで、`Web.config`ファイルとして次の例に示します。

[!code-xml[Main](overview/samples/sample15.xml)]

ASP.NET が自動的にエンコーディングのメソッドを公開するたびに、カスタム エンコード実装を呼び出すカスタム エンコーダーを構成した後、 *System.Web.HttpUtility*または*System.Web.HttpServerUtility*クラスと呼ばれます。 これにより、Web 開発チームの Api をエンコードするパブリックの ASP.NET を使用して Web 開発チームの残りの部分がし続けながら、余裕のない文字のエンコードを実装するカスタム エンコーダーを作成する 1 つの部分です。 カスタム エンコーダーを一元的に構成することによって、 *httpRuntime*要素、カスタム エンコーダーでエンコード Api パブリック ASP.NET からのすべてのテキスト エンコーディングの呼び出しがルーティングされる保証されます。

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>1 つのワーカー プロセス内の個々 のアプリケーションのパフォーマンスを監視

1 台のサーバーでホストできる Web サイトの数を増やすためには、多くのホスト側は、1 つのワーカー プロセスで複数の ASP.NET アプリケーションを実行します。 ただし、複数のアプリケーションでは、1 つの共有ワーカー プロセスを使用する場合は、サーバー管理者の問題が発生している個々 のアプリケーションを識別するが困難です。

ASP.NET 4 では、CLR によって導入された新しいリソース監視機能を活用します。 この機能を有効にするには、次の XML 構成スニペットを追加することができます、`aspnet.config`構成ファイル。

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> 注、`aspnet.config`ファイルが .NET Framework がインストールされているディレクトリにします。 `Web.config`ファイル。


ときに、 *appDomainResourceMonitoring*機能が有効になって、2 つの新しいパフォーマンス カウンターは、"ASP.NET Applications"のパフォーマンス カテゴリで利用可能な: *% プロセッサ時間のマネージ*と*マネージ メモリの使用*です。 これら両方のパフォーマンス カウンターは、推定の CPU 時間と個々 の ASP.NET アプリケーションの管理対象のメモリ使用率を追跡するために新しい CLR アプリケーション ドメインのリソース管理機能を使用します。 その結果、ASP.NET 4 で管理者ようになりましたがある 1 つのワーカー プロセスで実行されている個々 のアプリケーションのリソース消費量により詳細なビューです。

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>マルチ ターゲット

.NET Framework の特定のバージョンを対象とするアプリケーションを作成することができます。 ASP.NET 4 で、新しい属性で、*コンパイル*の要素、`Web.config`ファイルでは、.NET Framework 4 以降を対象することができます。 省略可能な要素を含める場合と明示的に、.NET Framework 4 を対象とする場合、`Web.config`ファイルのエントリなど*system.codedom*、これらの要素は、.NET Framework 4 に対して適切である必要があります。 (ターゲット フレームワークは内のエントリがないことから推論する明示的を対象としない、.NET Framework 4 の場合、`Web.config`ファイルです)。

次の例では、使用、 *targetFramework*属性、*コンパイル*の要素、`Web.config`ファイル。

[!code-xml[Main](overview/samples/sample17.xml)]

.NET Framework の特定のバージョンを対象とする際は、次に注意してください。

- .NET Framework 4 のアプリケーション プールでは、ASP.NET ビルド システムと見なされます、.NET Framework 4 をターゲットとして、`Web.config`ファイルは含まれません、 *targetFramework*属性場合、または、`Web.config`ファイルが見つかりません。 (.NET Framework 4 で実行されるようにアプリケーションをコーディングに変更する必要があります)。
- 必要な場合、 *targetFramework*属性、および場合、 *system.codeDom*で要素が定義されている、`Web.config`ファイル、このファイルは、.NET Framework 4 の正しいエントリを含める必要があります。
- 使用している場合、 *aspnet\_コンパイラ*(ビルド環境など)、アプリケーションをプリコンパイルするコマンドを正しいバージョンを使用する必要があります、 *aspnet\_コンパイラ*ターゲット フレームワークのコマンド。 .NET Framework 3.5 と以前のバージョンのコンパイルに .NET Framework 2.0 (%windir%\microsoft.net\framework\v2.0.50727) 同梱されているコンパイラを使用します。 .NET Framework 4 で付属しているコンパイラを使用して、そのフレームワークを使用して、またはそれ以降のバージョンを使用して作成されたアプリケーションをコンパイルします。
- コンパイラがコンピューターにインストールされている最新のフレームワーク アセンブリを使用して、実行時に (とそのため、GAC 内)。 フレームワークに更新プログラムを後で行われた場合 (たとえば、仮定バージョン 4.1 がインストールされます)、いなくても、新しいバージョンのフレームワークで機能を使用することができます、 *targetFramework*属性ターゲットを下位バージョン(4.0 など)。 (ただし、デザイン時に Visual Studio 2010 または使用すると、 *aspnet\_コンパイラ*コマンド、フレームワークの最新の機能を使用すると、コンパイラ エラーが発生)。

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>含まれる Web フォームと MVC の jQuery

Web フォームと MVC の両方の Visual Studio テンプレートには、オープン ソース jQuery ライブラリが含まれます。 新しい web サイトまたはプロジェクトを作成するときに、次の 3 つのファイルを含むスクリプト フォルダーが作成されます。

- jQuery-1.4.1.js –、人間が判読できるは unminified jQuery ライブラリのバージョンです。
- jQuery-14.1.min.js – jQuery ライブラリの縮小されたバージョンです。
- jQuery-1.4.1-vsdoc.js – jQuery ライブラリの Intellisense ドキュメント ファイル。

アプリケーションの開発中には、jQuery の unminified バージョンを含めます。 実可動アプリケーション用の jQuery の縮小されたバージョンが含まれます。

たとえば、次の Web フォーム ページは、jQuery を使用して、黄色のフォーカスがあるときに ASP.NET TextBox コントロールの背景色を変更する方法を示しています。

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>コンテンツ配信ネットワークのサポート

Microsoft Ajax コンテンツ配信ネットワーク (CDN) では、Web アプリケーションに ASP.NET Ajax および jQuery スクリプトを簡単に追加することができます。 JQuery ライブラリを追加するだけで使用を開始するなど、`<script>`が指す Ajax.microsoft.com 次のように、ページへのタグ。

[!code-html[Main](overview/samples/sample19.html)]

Microsoft Ajax CDN を利用して、Ajax アプリケーションのパフォーマンスを大幅に向上させることができます。 Microsoft Ajax CDN の内容は、世界各地に配置されたサーバーでキャッシュされます。 さらに、Microsoft Ajax CDN では、ブラウザーは別のドメインに配置されている Web サイトの JavaScript のキャッシュ ファイルを再利用が有効にします。

Microsoft Ajax Content Delivery Network は、Secure Sockets Layer を使用し、web ページを使用する必要がある場合に、SSL (HTTPS) をサポートしています。

CDN が利用できない場合は、フォールバックを実装します。 フォールバックをテストします。

Microsoft Ajax CDN の詳細については、次の web サイトを参照してください。

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ASP.NET ScriptManager は、Microsoft Ajax CDN をサポートします。 1 つのプロパティの設定、EnableCdn プロパティだけでは、CDN からすべての ASP.NET フレームワークの JavaScript ファイルを取得できます。

[!code-aspx[Main](overview/samples/sample20.aspx)]

EnableCdn プロパティを true に設定した後、ASP.NET framework は検証と UpdatePanel 使用されるすべての JavaScript ファイルを含む、CDN からすべての ASP.NET フレームワークの JavaScript ファイルを取得します。 この 1 つのプロパティを設定すると、web アプリケーションのパフォーマンスに大きな影響があります。

WebResource 属性を使用して、独自の JavaScript ファイル CDN パスを設定できます。 新しい CdnPath プロパティは、EnableCdn プロパティを true に設定するときに使用する CDN へのパスを指定します。

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>ScriptManager Explicit Scripts

以前は、ASP.NET ScriptManger を使用した場合、必要があるモノリシックな ASP.NET Ajax ライブラリ全体を読み込めません。 新しい ScriptManager.AjaxFrameworkMode プロパティを利用して、ASP.NET Ajax ライブラリのコンポーネントが読み込まれる完全に制御し、必要な ASP.NET Ajax ライブラリのコンポーネントのみをロードできます。

ScriptManager.AjaxFrameworkMode プロパティは、次の値に設定できます。

- -有効にするには、ScriptManager コントロールは、これは、結合したスクリプト ファイルのすべてのコア フレームワーク スクリプト (従来の動作)、MicrosoftAjax.js スクリプト ファイルを自動的に含まれていますを指定します。
- 無効になっている--には、すべての Microsoft Ajax スクリプト機能を無効になっていることと、ScriptManager コントロールは任意のスクリプトを自動的に参照していませんことを指定します。
- --明示的なを指定します、ページを必要とする個々 の framework core スクリプト ファイルへのスクリプト参照を明示的に含める、および各スクリプト ファイルを必要とする依存関係への参照にが含まれます。

たとえば、AjaxFrameworkMode プロパティを明示的な値に設定した場合は、必要な特定の ASP.NET Ajax コンポーネントのスクリプトを指定できます。

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web フォーム

Web フォームは、ASP.NET 1.0 のリリース後に ASP.NET で中心的な機能にされました。 多くの機能強化は、次を含む、ASP.NET 4 用のこの領域にされています。

- 設定できる*メタ*タグ。
- ビュー ステートより詳細に制御します。
- ブラウザーの機能を使用する簡単な方法です。
- ASP.NET Web フォームでのルーティングを使用するためのサポート。
- 詳細に制御は、Id を生成します。
- データ コントロールで選択された行を保持できるようにします。
- 詳細な制御でレンダリングされる HTML、 *FormView*と*ListView*コントロール。
- データ ソース コントロールのサポートをフィルター処理します。

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Page.MetaKeywords と Page.MetaDescription プロパティをメタ タグを設定

ASP.NET 4 で追加する 2 つのプロパティ、*ページ*クラス、 *MetaKeywords*と*MetaDescription*です。 対応する 2 つのプロパティを表す*メタ*ページを開き、タグを次の例で示すようにします。

[!code-aspx[Main](overview/samples/sample23.aspx)]

これら 2 つのプロパティは、同じ動作方法 ページの*タイトル*プロパティにします。 これらのルールに従います。

1. ある場合ありません*メタ*内にタグを*ヘッド*プロパティ名と一致する要素 (は、名前 =「キーワード」 *Page.MetaKeywords*と名前の「説明」を=*Page.MetaDescription*、これらのプロパティが設定されていないことを意味) では、*メタ*タグは、レポートが表示されるページに追加されます。
2. 既にある場合*メタ*get し、set メソッド、既存のタグの内容としてにこれらの名前を持つタグは、これらのプロパティの動作です。

データベースまたはその他のソースからコンテンツを取得することができ、作業内容を動的にタグを設定することができます、実行時にこれらのプロパティを設定するには、特定のページです。

設定することも、*キーワード*と*説明*プロパティで、 *@ Page*次の例のように、Web フォーム ページのマークアップの上部にあるディレクティブ。

[!code-aspx[Main](overview/samples/sample24.aspx)]

これよりも優先されます、*メタ*タグ内容 (存在する場合) がページで既に宣言されています。

説明の内容*メタ*タグは Google でのプレビューを一覧表示する検索を向上させるために使用します。 (詳細については、「[メタ説明作り直しでスニペットを向上させる](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html)Google web マスター サーバー全体のブログです。)Google や Windows Live Search では、任意のキーワードの内容は使用しないで、その他の検索エンジン可能性があります。 詳細については、次を参照してください。[メタ キーワード アドバイス](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php)検索エンジンのガイドの Web サイトです。

これらの新しいプロパティは単純な機能が、これらを手動で追加の要件やを作成するコードの記述から保存した、*メタ*タグ。

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>個々 のコントロール ビュー ステートを有効にします。

既定では、ビュー ステートは、アプリケーションの必要がない場合でも、ページ上の各コントロールがそのビュー ステートを可能性がある格納する結果のページで有効です。 ページが生成し、ページをクライアントに送信し、バックアップに要する時間の増加は、マークアップでビュー状態データが含まれます。 必要以上のビュー ステートを格納すると、大幅なパフォーマンスの低下を引き起こすことができます。 以前のバージョンの ASP.NET では、開発者は、ページ サイズを小さくために、個々 のコントロールのビューステートを無効には、個々 のコントロールのために明示的に行う必要があります。 Web サーバー コントロールには、ASP.NET 4 で、 *ViewStateMode*プロパティを使用する既定のビュー ステートを無効にして、ページで必要とするコントロールに対してのみ有効にします。

*ViewStateMode*プロパティを 3 つの値を持つ列挙体には:*有効*、*無効になっている*、および*継承*です。 *有効になっている*有効にし、そのコントロールのすべての子コントロールに設定されている状態の表示*継承*ことか、または何も設定します。 *無効になっている*ビュー ステートを無効にし、*継承*コントロールが使用するように指定、 *ViewStateMode*親コントロールから設定します。

次の例は、どのように*ViewStateMode*プロパティはします。 マークアップと、ページのコントロールを次のコードの値が含まれています、 *ViewStateMode*プロパティ。

[!code-aspx[Main](overview/samples/sample25.aspx)]

ご覧のように、コードには、PlaceHolder1 コントロールのビュー ステートが無効にします。 子 label1 コントロールは、このプロパティの値を継承 (*継承*の既定値は、 *ViewStateMode*コントロールの) しビュー ステートしたがって保存されません。 PlaceHolder2 コントロールで、 *ViewStateMode*に設定されている*有効*label2 は、このプロパティを継承し、保存状態を表示するため、します。 ページが最初に読み込まれたときに、*テキスト*両方のプロパティ*ラベル*コントロールが、文字列"[DynamicValue]"に設定します。

これらの設定の効果は、ページが初めて読み込まれるときに、次の出力がブラウザーで表示されることです。

無効になっています。 `: [DynamicValue]`

有効になります。`[DynamicValue]`

ポストバックされた後に、次の出力が表示されます。

無効になっています。 `: [DeclaredValue]`

有効になります。`[DynamicValue]`

Label1 コントロール (が*ViewStateMode*に値が設定されている*無効になっている*) がコード内設定されている値を保持していません。 ただし、label2 制御 (が*ViewStateMode*に値が設定されている*有効*) の状態を保持しています。

設定することも*ViewStateMode*で、 *@ Page*ディレクティブを次の例のように。

[!code-aspx[Main](overview/samples/sample26.aspx)]

*ページ*クラスは、単なる別のコントロール以外の場合は、ページの他のすべてのコントロールの親コントロールとして機能します。 既定値*ViewStateMode*は*有効*のインスタンスの*ページ*です。 コントロールが既定になるため*継承*、コントロールが継承、*有効*プロパティ値を設定しない限り*ViewStateMode*ページまたはコントロール レベル。

値、 *ViewStateMode*プロパティは、ビュー状態を維持する場合にのみかどうかを決定、*エントリ*プロパティに設定されている*true*です。 場合、*エントリ*プロパティに設定されている*false*、ビュー状態が維持されない場合でも*ViewStateMode*に設定されている*有効*です。

この機能の良い使用*ContentPlaceHolder*マスター ページで、設定できるコントロール*ViewStateMode*に*無効になっている*マスター ページし、有効に個別に*ContentPlaceHolder*順番が必要なコントロールを格納しているコントロールの状態を表示します。

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>ブラウザーの機能への変更

ASP.NET と呼ばれる機能を使用してサイトを閲覧するユーザーを使用しているブラウザーの機能を決定する*ブラウザーの性能*です。 ブラウザーの機能がによって表される、 *HttpBrowserCapabilities*オブジェクト (によって公開されている、 *Request.Browser*プロパティ)。 たとえば、使用することができます、 *HttpBrowserCapabilities*オブジェクト、型と、現在のブラウザーのバージョンが JavaScript の特定のバージョンをサポートするかどうかを判別します。 または、使用することができます、 *HttpBrowserCapabilities*モバイル デバイスから、要求が発生したかどうかを決定するオブジェクト。

*HttpBrowserCapabilities*オブジェクトは、一連のブラウザー定義ファイルによって決まります。 これらのファイルには、特定のブラウザーの機能に関する情報が含まれます。 ASP.NET 4 に最近導入されたブラウザーと Google Chrome、研究などモーション BlackBerry スマート フォンと Apple の iPhone でのデバイスに関する情報にこれらのブラウザー定義ファイルが更新されました。

次の一覧は、新しいブラウザー定義ファイルを示しています。

- *blackberry.browser*
- *chrome.browser*
- *Default.browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>ブラウザー機能のプロバイダーを使用します。

Asp.net version 3.5 Service Pack 1 を次のように、ブラウザーが持つ機能を定義することができます。

- コンピューター レベルで作成または更新、`.browser`次のフォルダーに XML ファイル。

- [!code-console[Main](overview/samples/sample27.cmd)]

- ブラウザーの機能を定義した後、次のコマンドに、Visual Studio コマンド プロンプトからブラウザー機能アセンブリを再構築して GAC に追加するためを実行します。

- [!code-console[Main](overview/samples/sample28.cmd)]

- 個々 のアプリケーションを作成する、`.browser`アプリケーションのファイル`App_Browsers`フォルダーです。

これらのアプローチでは、XML ファイルを変更する必要があり、コンピューター レベルの変更の aspnet を実行した後、アプリケーションを再起動する必要があります\_regbrowsers.exe プロセスです。

ASP.NET 4 と呼ばれる機能が含まれています。*ブラウザー機能プロバイダー*です。 名前からわかるように、これにより、さらに使用できるプロバイダーをビルドするは、ブラウザーの機能を決定するのに、独自のコードを使用します。

実際には、開発者頻度を定義しないでカスタム ブラウザーの機能です。 ブラウザー ファイルは、更新の場合は、更新することは非常に複雑なプロセスをハードおよびの XML 構文`.browser`ファイルを使用してを定義する複雑になることができます。 新機能は、このプロセスをより簡潔とは、一般的なブラウザーの定義の構文があった場合または最新のブラウザーの定義、またはデータベースなどの Web サービスも含まれているデータベース。 新しいブラウザーの機能のプロバイダーの機能により、これらのシナリオ可能で実用的なサード パーティの開発者向けです。

ASP.NET 4 ブラウザー機能プロバイダーの新しい機能を使用する 2 つの主な方法: ASP.NET ブラウザーの機能の定義機能を拡張するか、完全に置き換えることができます。 次のセクションでは、最初と方法について説明、機能を置き換えるし、それを拡張する方法です。

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>ASP.NET ブラウザー機能の機能を置き換える

ASP.NET ブラウザー機能の定義機能を完全に置き換えるには、次の手順を実行します。

1. 派生するプロバイダー クラスを作成する*HttpCapabilitiesProvider*をオーバーライドして、 *GetBrowserCapabilities*次の例のように、メソッド。 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    この例では、コードが新たに作成*HttpBrowserCapabilities*オブジェクト、MyCustomBrowser にその機能を設定してブラウザーをという名前の機能のみを指定します。
2. プロバイダーは、アプリケーションを登録します。 

    追加する必要があります、プロバイダーをアプリケーションで使用するために、*プロバイダー*属性を*browserCaps* 」の「、`Web.config`または`Machine.config`ファイル。 (プロバイダーの属性を定義することも、*場所*アプリケーションでは、特定のモバイル デバイス用のフォルダーなどの特定のディレクトリの要素です)。次の例は、設定する方法を示します、*プロバイダー*構成ファイル内の属性。

    [!code-xml[Main](overview/samples/sample30.xml)]

    新しいブラウザーの機能の定義を登録する別の方法は、次の例で示すようにコードを使用するのには。

    [!code-csharp[Main](overview/samples/sample31.cs)]

    このコードを実行する必要があります、*アプリケーション\_開始*のイベント、`Global.asax`ファイル。 変更を加えた、 *BrowserCapabilitiesProvider*クラスは、キャッシュが、解決済みの有効な状態に残っているかどうかを確認するために、アプリケーションの任意のコードを実行する前に行う必要があります*HttpCapabilitiesBase*オブジェクト。

#### <a name="caching-the-httpbrowsercapabilities-object"></a>HttpBrowserCapabilities オブジェクトのキャッシュ

前の例は、コードは、カスタム プロバイダーを取得するために呼び出されるたびに実行ことである 1 つの問題、 *HttpBrowserCapabilities*オブジェクト。 これでは、複数回を各要求中に発生します。 例では、プロバイダーのコードはほど多くありません。 ただし、カスタム プロバイダーでコードの重要な作業を実行する場合を取得するため、 *HttpBrowserCapabilities*オブジェクト、これはパフォーマンスに影響することができます。 これを防ぐをキャッシュできます、 *HttpBrowserCapabilities*オブジェクト。 この場合は、以下の手順に従ってください。

1. 派生するクラスを作成する*HttpCapabilitiesProvider*などの次の例 1。 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    例では、コードはカスタム BuildCacheKey メソッドを呼び出すことによって、キャッシュ キーを生成し、カスタム GetCacheTime メソッドを呼び出して、キャッシュする時間の長さを取得します。 次のコードは解決された追加*HttpBrowserCapabilities*キャッシュするオブジェクト。 オブジェクトをキャッシュから取得しで再利用する後続の要求をカスタム プロバイダーの使用します。
2. 前の手順で説明されているアプリケーションで、プロバイダーを登録します。

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>ASP.NET ブラウザー機能の機能を拡張します。

前のセクションには、新しいを作成する方法が説明されている*HttpBrowserCapabilities* ASP.NET 4 のオブジェクト。 ASP.NET に既に含まれている新しいブラウザーの機能の定義を追加することで ASP.NET ブラウザー機能機能を拡張することもできます。 XML のブラウザーの定義を使用せず、これを行うことができます。 次の手順に示す方法。

1. 派生するクラスを作成する*HttpCapabilitiesEvaluator*をオーバーライドして、 *GetBrowserCapabilities*メソッドを次の例で示すようにします。 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    このコードでは、まず、ASP.NET ブラウザー機能の機能を使用してブラウザーを識別しようとしています。 ただし、ブラウザーが指定されていない場合、情報に基づいて、要求で定義されている (つまり、*ブラウザー*のプロパティ、 *HttpBrowserCapabilities*オブジェクトは、文字列"Unknown")、コードの呼び出しカスタムのプロバイダー (MyBrowserCapabilitiesEvaluator) をブラウザーを識別します。
2. 前の例に示すように、プロバイダーをアプリケーションを登録します。

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>ブラウザー機能の機能を拡張する既存の機能の定義に新機能の追加

カスタムのブラウザー定義プロバイダーを作成するだけでなく、新しいブラウザーの定義を動的に作成するのには、追加の機能の既存のブラウザー定義を拡張できます。 これにより、定義対象の近くには、いくつかの機能のみがないを使用できます。 そのためには、次の手順に従います。

1. 派生するクラスを作成する*HttpCapabilitiesEvaluator*をオーバーライドして、 *GetBrowserCapabilities*メソッドを次の例で示すようにします。 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    コード例は、既存の ASP.NET を拡張*HttpCapabilitiesEvaluator*クラスを取得するため、 *HttpBrowserCapabilities*次のコードを使用して現在の要求の定義と一致するオブジェクト:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    コードでは追加し、またはこのブラウザーの機能を変更できます。 これにはブラウザーの新機能を指定する 2 つの方法があります。

    - キー/値ペアを追加、 *IDictionary*によって公開されるオブジェクト、*機能*のプロパティ、 *HttpCapabilitiesBase*オブジェクト。 前の例では、コードは、機能が追加されて、マルチタッチをという名前の値を持つ*true*です。
    - 既存のプロパティの設定、 *HttpCapabilitiesBase*オブジェクト。 前の例では、コードを設定、*フレーム*プロパティを*true*です。 アクセサーを単に、このプロパティは、 *IDictionary*によって公開されるオブジェクト、*機能*プロパティです。 

        > [!NOTE]
        > このモデルは、任意のプロパティに適用されます*HttpBrowserCapabilities*、コントロール アダプターを含むです。
2. 前の手順で説明されているアプリケーションで、プロバイダーを登録します。

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>ASP.NET 4 でのルーティング

ASP.NET 4 Web フォームでルーティングを使用するための組み込みサポートを追加します。 要求の物理ファイルにマップされていない Url のルーティングは、そのまま使用するアプリケーションを構成することができます。 代わりに、ルーティングを使用する、ユーザーにわかりやすいとが、アプリケーションの検索エンジン最適化 (SEO) を使用して Url を定義することができます。 たとえば、既存のアプリケーションの製品カテゴリを表示するページの URL は、次の例のようになります。

[!code-console[Main](overview/samples/sample36.cmd)]

ルーティングを使用するには、同じ情報を表示するために、次の URL を受け入れるようにアプリケーションを構成できます。

[!code-console[Main](overview/samples/sample37.cmd)]

ルーティングは ASP.NET 3.5 SP1 以降で使用されました。 (ASP.NET 3.5 SP1 でルーティングを使用する方法の例は、エントリを参照してください。 [WebForms とルーティングを使用して](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "このエントリのタイトル。") Phil Haack のブログです。)ただし、ASP.NET 4 が含まれています、ルーティングを使用するより簡単にするいくつかの機能には、次を含みます。

- *PageRouteHandler*クラスは、ルートを定義するときに使用する単純な HTTP ハンドラーします。 クラスに要求をルーティングするページへのデータを渡します。
- 新しいプロパティ*HttpRequest.RequestContext*と*Page.RouteData* (これは、プロキシを*HttpRequest.RequestContext.RouteData*オブジェクト)。 これらのプロパティをやすくをルートから渡される情報にアクセスします。
- 次の新しい式ビルダーで定義されている*System.Web.Compilation.RouteUrlExpressionBuilder*と*System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*、ASP.NET サーバー コントロール内のルート URL に対応する URL を作成する簡単な方法を提供します。
- *RouteValue*からの情報を抽出する簡単な方法を提供する、 *RouteContext*オブジェクト。
- *RouteParameter*を簡単に含まれるデータを渡すにクラス、 *RouteContext*データ ソース コントロールに対するクエリへのオブジェクト (に似て[ *FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Web フォーム ページのルーティング

次の例は、新しいを使用して Web フォームのルートを定義する方法を示します*MapPageRoute*のメソッド、*ルート*クラス。

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 では、 *MapPageRoute*メソッドです。 次の例は、前の例で示す SearchRoute 定義と同じですを使用して、 *PageRouteHandler*クラスです。

[!code-csharp[Main](overview/samples/sample39.cs)]

コードの例では、物理ページにルートをマップ (最初のルート上でに`~/search.aspx`)。 ルートの最初の定義は、searchterm をという名前のパラメーターを URL から抽出され、ページに渡されることにも指定します。

*MapPageRoute*メソッドは、次のメソッドのオーバー ロードをサポートしています。

- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults)*
- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults, RouteValueDictionary constraints)*

*CheckPhysicalUrlAccess*パラメーターは、ルートにルーティングされている物理ページの セキュリティ アクセス許可を確認するかどうかを指定します (この場合は、完成しました) と、着信 URL のアクセス許可 (この場合、検索/{searchterm})。 場合の値*checkPhysicalUrlAccess*は*false*、着信 URL のアクセス許可だけがチェックされます。 これらのアクセス許可が定義されている、`Web.config`ファイルの次のように設定を使用します。

[!code-xml[Main](overview/samples/sample40.xml)]

例の構成へのアクセスが拒否物理ページ`search.aspx`管理者の役割であるものを除くすべてのユーザーです。 ときに、 *checkPhysicalUrlAccess*にパラメーターが設定されている*true* (がその既定値)、物理ページ完成しましたので、URL/search/{searchterm} にアクセスする管理ユーザーのみが許可されてそのロールのユーザーに制限されています。 場合*checkPhysicalUrlAccess*に設定されている*false*と前の例で示すように、サイトが構成されている、すべての認証ユーザーが URL/search/{searchterm} のアクセスを許可します。

#### <a name="reading-routing-information-in-a-web-forms-page"></a>ルーティング情報を Web フォーム ページの読み取り

Web フォームの物理ページのコードでのルーティングが URL から抽出される情報にアクセスすることができます (または別のオブジェクトに追加したその他の情報、 *RouteData*オブジェクト) 2 つの新しいプロパティを使用して: *HttpRequest.RequestContext*と*Page.RouteData*です。 (*Page.RouteData*ラップ*HttpRequest.RequestContext.RouteData*)。次の例は、使用する方法を示しています。 *Page.RouteData*です。

[!code-csharp[Main](overview/samples/sample41.cs)]

コードでは、前の例のルートで定義されている searchterm パラメーターに渡された値を抽出します。 次の要求の URL を考慮してください。

[!code-console[Main](overview/samples/sample42.cmd)]

この要求が行われる場合、"scott"はでレンダリングされ、単語、`search.aspx`ページ。

#### <a name="accessing-routing-information-in-markup"></a>マークアップでルーティング情報にアクセスします。

前のセクションで説明されているメソッドは、Web フォーム ページ内のコードのルート データを取得する方法を示します。 また、同じ情報にアクセスできるようにマークアップ内の式を使用することができます。 式ビルダーは、宣言型コードを操作する強力で簡潔な方法です。 (詳細については、エントリを参照してください[Express 自分でカスタム式ビルダー](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) Phil Haack のブログです。)。

ASP.NET 4 Web フォームのルーティング用の 2 つの新しい式ビルダーが含まれています。 次の例では、その使用方法を示します。

[!code-aspx[Main](overview/samples/sample43.aspx)]

この例で、 *RouteUrl*ルートのパラメーターに基づいている URL を定義する式を使用します。 これは、マークアップに完全な URL をハードコーディングしなくて済むためであり、このリンクを変更することがなく、URL の構造を後で変更することができます。

以前に定義されたルートに基づき、このマークアップには、次の URL が生成されます。

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET は、正しいルートを自動的に機能 (正しい URL を生成、)、入力パラメーターに基づきます。 式では、使用するルートを指定するため、ルート名を含めることもできます。

次の例を使用する方法を示しています、 *RouteValue*式。

[!code-aspx[Main](overview/samples/sample45.aspx)]

このコントロールを含むページが実行されると、"scott"の値がラベルに表示されます。

*RouteValue*式を簡単にマークアップでは、ルート データを使用しより複雑な Page.RouteData["x を操作することを回避できます"] マークアップの構文。

#### <a name="using-route-data-for-data-source-control-parameters"></a>ルート データ データ ソース コントロール パラメーターの使用

*RouteParameter*クラスでは、ルート データをデータ ソース コントロール内のクエリのパラメーター値として指定することができます。 これは、[動作とほぼ同様に、](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)クラスに、次の例に示すようにします。

[!code-aspx[Main](overview/samples/sample46.aspx)]

ルート パラメーター searchterm の値が使用しての例では、@companyname内のパラメーター、<em>選択</em>ステートメントです。

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>クライアント Id の設定

新しい*ClientIDMode*プロパティ ASP.NET では、長年にわたる問題に対処してつまりコントロールを作成する方法、 *id*をレンダリングする要素の属性です。 知ること、 *id*レンダリングされる要素の属性は、アプリケーションには、これらの要素を参照しているクライアント スクリプトが含まれている場合に重要です。

*Id*に基づいて Web サーバー コントロールのレンダリングされる html 属性を生成、 *ClientID*コントロールのプロパティです。 ASP.NET 4 で、アルゴリズムを生成するまで、 *id*属性から、 *ClientID*プロパティが、ID、およびに (ように繰り返し使用されるコントロールの場合、(存在する場合)、名前付けコンテナーを連結するされてデータ コントロール)、プレフィックスと連続する番号を追加します。 コントロールがクライアント スクリプトで参照するが困難ためと、予測可能なインポートされなかった Id アルゴリズムが、ページ内のコントロールの Id が一意であることを保証これが常に中が発生しました。

新しい*ClientIDMode*プロパティを使用して、具体的には、クライアント ID を生成する方法のコントロールを指定できます。 設定することができます、 *ClientIDMode*ページを含む任意のコントロールのプロパティです。 使用可能な設定は次のとおりです。

- *AutoID* – これは、生成するためのアルゴリズムを*ClientID* ASP.NET の以前のバージョンで使用されたプロパティの値。
- *静的*– これを指定する、 *ClientID*値は、親の名前付けコンテナーの Id を連結することがなく ID と同じになります。 これは、Web ユーザー コントロールに役に立ちます。 Web ユーザー コントロールできますが、別のコンテナー コントロールおよび別のページに配置しているために使用するコントロールのクライアント スクリプトを記述するが困難できます、 *AutoID*アルゴリズム ID の値がどうなるかを予測することはできませんので.
- *予測可能な*– このオプションは、主に繰り返しのテンプレートを使用するデータ コントロールで使用します。 生成されたが、コントロールの名前付けコンテナーの ID プロパティを連結*ClientID* "ctlxxx"などの文字列を含めることはできません。 この設定は機能と組み合わせて、 *ClientIDRowSuffix*コントロールのプロパティです。 設定する、 *ClientIDRowSuffix*プロパティをデータ フィールドの名前とそのフィールドの値が生成されるサフィックスとして使用*ClientID*値。 データのレコードとしての主キーを使用する通常、 *ClientIDRowSuffix*値。
- *継承*– この設定はコントロールの既定の動作です。 コントロールの ID の生成がその親と同じであることを指定します。

設定することができます、 *ClientIDMode*ページ レベルのプロパティです。 既定値を定義*ClientIDMode*現在のページのすべてのコントロールの値。

既定値*ClientIDMode*ページ レベルの値は*AutoID*と既定*ClientIDMode*コントロール レベルの値は*継承*. その結果、設定しないこのプロパティ任意の場所、コードで、すべてのコントロールは既定の*AutoID*アルゴリズムです。

ページ レベルの値で設定する、 *@ Page*ディレクティブについては、次の例で示すようにします。

[!code-aspx[Main](overview/samples/sample47.aspx)]

設定することも、 *ClientIDMode*コンピューター (machine) レベルまたはアプリケーション レベルで、構成ファイル内の値。 既定値を定義*ClientIDMode*アプリケーション内のすべてのページのすべてのコントロールを設定します。 既定値を定義、コンピューター レベルで、値を設定すると*ClientIDMode*そのコンピューターのすべての Web サイトを設定します。 次の例は、 *ClientIDMode*構成ファイルで設定します。

[!code-xml[Main](overview/samples/sample48.xml)]

既に述べたとおりの値として、 *ClientID*プロパティはコントロールの親の名前付けコンテナーから派生します。 マスター ページを使用する場合など、一部のシナリオ コントロールが最終的に Id を持つもので、次のようにレンダリングされる HTML:

[!code-html[Main](overview/samples/sample49.html)]

場合でも、*入力*マークアップに示される要素 (から、 *テキスト ボックス*コントロール) 2 つの名前付けコンテナーは、ページの深さ (入れ子になった*ContentPlaceholder*コントロール)、マスター ページが処理されるため、最終結果は、次のようにコントロール ID です。

[!code-console[Main](overview/samples/sample50.cmd)]

この ID ページで、一意であることが保証しますが、不必要にほとんどの目的の長さ。 レンダリングの ID の長さを短くして ID を生成する方法より細かく制御することを想像してください。 (たとえば、する"ctlxxx"プレフィックスを排除します。)これを実現する最も簡単な方法は、設定して、 *ClientIDMode*次の例で示すようにプロパティ。

[!code-aspx[Main](overview/samples/sample51.aspx)]

このサンプルでは、 *ClientIDMode*プロパティに設定されている*静的*、最も外側の*NamingPanel*要素、および設定する*Predictable*内部の*NamingControl*要素。 これらの設定 (ページおよびマスター ページの残りの部分は前の例では、同じであると見なされます)、次のマークアップで生成されます。

[!code-html[Main](overview/samples/sample52.html)]

*静的*設定は、最も外側にある、そのすべてのコントロールの名前付けの階層をリセットすることによる影響*NamingPanel*要素、および削除する、 *ContentPlaceHolder*と*MasterPage*生成された ID からの Id (、*名前*レンダリングされる要素の属性が影響を受けるので、通常の ASP.NET 機能を保持するイベントの状態を表示し、します)。命名階層をリセットする際の副作用のマークアップを移動する場合でも、 *NamingPanel*を別の要素*ContentPlaceholder*コントロール、レンダリングされたクライアント Id は変わりません。

> [!NOTE]
> レンダリングされたコントロール Id が一意であるかどうかを確認するかどうかは注意してください。 クライアントなど、個々 の HTML 要素の一意の Id を必要とする機能をすべてを壊れる可能性がない場合、*に対して*関数。


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>データ バインド コントロールでの予測可能なクライアント Id の作成

*ClientID*レガシ アルゴリズムによってデータ連結リスト コントロール内のコントロールにすることができますに生成される値が長いし、は実際に予測できません。 *ClientIDMode*機能をどのようにこれらの Id の生成を制御の詳細があるのに役立ちます。

次の例では、マークアップが含まれています、 *ListView*コントロール。

[!code-aspx[Main](overview/samples/sample53.aspx)]

前の例で、 *ClientIDMode*と*RowClientIDRowSuffix*プロパティ マークアップで設定されます。 *ClientIDRowSuffix*プロパティは、データ バインド コントロールでのみ使用することができを使用するコントロールに応じてでその動作が異なります。 この違いは、これらは。

- *GridView*コントロールなどを指定できます 1 つまたは複数の列の名前、データ ソースでは、クライアント Id を作成する実行時に組み合わされます。 設定する場合など、 *RowClientIDRowSuffix* "ProductName、ProductId"にレンダリングされる要素は、次のような形式であるために、コントロール Id:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView*コントロール-クライアント ID に追加されているデータ ソースで 1 つの列を指定できます 設定する場合など、 *ClientIDRowSuffix* "ProductName"にレンダリングされたコントロール Id は、次のような形式。

- [!code-console[Main](overview/samples/sample55.cmd)]

- ここでは、末尾の 1 は、現在のデータ項目の製品 ID から派生されます。

- *リピータ*コントロール — このコントロールはサポートされない、 *ClientIDRowSuffix*プロパティです。 *リピータ*コントロール、現在の行のインデックスを使用します。 ClientIDMode を使用する場合に"Predictable"を =、*リピータ*を制御する、次の形式を持つクライアント Id が生成されます。

- [!code-console[Main](overview/samples/sample56.cmd)]

- 末尾の 0 は、現在の行のインデックスです。

*FormView*と*DetailsView*コントロール表示しない、複数の行をサポートしていないため、 *ClientIDRowSuffix*プロパティです。

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>データ コントロール内で保持する行の選択

*GridView*と*ListView*コントロールは行を選択してユーザーをさせることができます。 ASP.NET の以前のバージョンではされてに基づいて選択 ページで行インデックスされます。 たとえば、1 ページ目の 3 番目の項目を選択して、2 ページを移動して、そのページで、3 番目の項目が選択されます。

持続する選択が最初に .NET Framework 3.5 SP1 での動的なデータ プロジェクトでのみサポート。 この機能を有効にすると、現在の選択項目が項目のデータのキーに基づいています。 これは、1 ページ目の 3 番目の行を選択して、2 ページに移動して場合、nothing が選択されている 2 ページ目にことを意味します。 1 ページに戻ると 3 番目の行が選択されたままです。 持続する選択がサポートされるようになりました、 *GridView*と*ListView*を使用してすべてのプロジェクト内のコントロール、 *EnablePersistedSelection*プロパティのように、次の例:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>ASP.NET のグラフ コントロール

ASP.NET*グラフ*コントロールは、.NET Framework のデータ ビジュアライゼーション ソリューションを拡張します。 使用して、*グラフ*コントロール、必要な複雑な統計分析や財務分析の直感的で視覚的に説得力のあるグラフを ASP.NET ページを作成することができます。 ASP.NET*グラフ*コントロールは、.NET Framework version 3.5 SP1 のリリースにアドオンとして導入され、.NET Framework 4 のリリースの一部であります。

コントロールには、次の機能が含まれています。

- 35 種類のグラフ
- グラフ領域、タイトル、凡例、および注釈の無制限の数。
- さまざまなすべてのグラフ要素の外観設定します。
- ほとんどの種類のグラフの 3-D サポートします。
- スマート データ ラベルをデータ ポイントに関連自動的に合わせることができます。
- ストリップ ライン、スケールの区切り線、およびスケーリングの対数。
- データの分析と変換に使用できる 50 を超える財務式および統計式。
- 単純バインディングとグラフのデータを操作します。
- 日付、時刻、通貨などの一般的なデータ形式をサポートします。
- 対話機能とクライアントを含む、イベント ドリブンのカスタマイズのサポートは、Ajax を使用してイベントをクリックします。
- 状態管理。
- バイナリ ストリーム転送。

次の図は、ASP.NET のグラフ コントロールによって生成される財務グラフの例を紹介します。

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

図 2: ASP.NET のグラフ コントロールの例

ASP.NET のグラフ コントロールを使用する方法の例については、サンプル コードをダウンロード、[用 Microsoft グラフ コントロールのサンプル環境](https://go.microsoft.com/fwlink/?LinkId=128300)MSDN Web サイトのページです。 コンテンツ コミュニティの他のサンプルを見つけることができます、 [Chart Control Forum](https://go.microsoft.com/fwlink/?LinkId=128713)です。

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>ASP.NET ページへのグラフ コントロールの追加

次の例は、追加する方法を示します、*グラフ*マークアップを使用して、ASP.NET ページを制御します。 この例で、*グラフ*コントロールが静的なデータ ポイントの縦棒グラフを生成します。

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>3-D グラフの使用

*グラフ*コントロールに含まれる、 *ChartAreas*含めることができるコレクション*ChartArea*グラフ領域の特性を定義するオブジェクト。 たとえば、グラフ領域の 3-D を使用する次のように使用します。、 *Area3DStyle*例を次のプロパティ。

[!code-aspx[Main](overview/samples/sample59.aspx)]

次の図は、4 つの系列を 3-D グラフを示しています、*バー*グラフの種類。

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

図 3: 3-D の横棒グラフ

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>スケール区切り、対数スケールを使用します。

スケール区切り、対数スケールは、洗練されたグラフを追加する、2 つの追加方法です。 これらの機能は、グラフ領域内の各軸に固有です。 たとえば、グラフ領域の主軸 Y 軸にこれらの機能を使用する次のように使用します。、 *AxisY.IsLogarithmic*と*ScaleBreakStyle*プロパティで、 *ChartArea*オブジェクト。 次のスニペットは、主軸の Y 軸のスケール区切りを使用する方法を示します。

[!code-aspx[Main](overview/samples/sample60.aspx)]

次の図は、スケール区切りを有効になっていると、Y 軸を示します。

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

図 4: スケール区切り

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>QueryExtender コントロールにデータをフィルター処理

データ ドリブン Web ページを作成する開発者向けに非常に一般的なタスク データをフィルター処理を開始します。 これは、従来によって実行されたビルド*場所*ソース コントロールのデータ内の句。 この方法は複雑になることができ、場合によっては、*場所*構文では、基になるデータベースのすべての機能を活用することはできません。

新しいフィルター容易にする*QueryExtender* ASP.NET 4 でコントロールが追加されました。 このコントロールに追加できます*EntityDataSource*または*LinqDataSource*これらのコントロールによって返されるデータをフィルター処理するためにコントロールできます。 *QueryExtender* LINQ に依存しているコントロール、データが非常に効率的な操作の結果 ページに送信される前に、データベース サーバーで、フィルターが適用されます。

*QueryExtender*コントロールは、さまざまなフィルター オプションをサポートしています。 次のセクションは、これらのオプションについて説明し、その使用方法の例を紹介します。

#### <a name="search"></a>検索

検索オプションの場合、 *QueryExtender*コントロールは、指定したフィールドに検索を実行します。 次の例では、コントロールを使用し、TextBoxSearch コントロール内でそのコンテンツを検索に入力されたテキスト、`ProductName`と`Supplier.CompanyName`から返されるデータの列、 *LinqDataSource*コントロール。

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>範囲

範囲のオプションは、検索オプションに似ていますが、範囲を定義する値のペアを指定します。 次の例で、 *QueryExtender*検索の制御、`UnitPrice`から返されるデータの列、 *LinqDataSource*コントロール。 範囲は、ページ上の TextBoxFrom と TextBoxTo コントロールから読み取られます。

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

プロパティ式のオプションを使用して、プロパティの値の比較を定義できます。 式が評価された場合*true*、検査されているデータが返されます。 次の例で、 *QueryExtender*コントロール内のデータを比較することでデータをフィルター処理、`Discontinued`ページ上の CheckBoxDiscontinued コントロールからの列には、値。

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

最後で使用するカスタム式を指定することができます、 *QueryExtender*コントロール。 このオプションにより、カスタム フィルターのロジックを定義する ページで、関数を呼び出すことができます。 次の例は、宣言でカスタム式を指定する方法を示します、 *QueryExtender*コントロール。

[!code-aspx[Main](overview/samples/sample64.aspx)]

次の例では、ユーザー定義関数によって呼び出される、 *QueryExtender*コントロール。 データベースを含むクエリを使用する代わりに、ここで、*場所*句、コードを使用して LINQ クエリ、データをフィルター処理します。

[!code-csharp[Main](overview/samples/sample65.cs)]

これらの例で使用されている 1 つだけ式を表示する、 *QueryExtender*一度にコントロール。 ただし、内の複数の式を含めることができます、 *QueryExtender*コントロール。

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Html エンコードのコード式

使用してに大きく依存します (特に、ASP.NET MVC) の ASP.NET サイト`<%` =  `expression %>`構文 (「コード ナゲット」と呼ばれる多くの場合)、応答にいくつかのテキストを書き込む。 コード式を使用する場合は、ことを忘れがちに HTML でエンコードするテキストがユーザーを元の場合は、テキスト入力ですが、これがページを開いておく (クロスサイト スクリプティング) XSS 攻撃に対して簡単です。

ASP.NET 4 には、次の新しい構文のコード式が導入されています。

[!code-aspx[Main](overview/samples/sample66.aspx)]

この構文は、応答に書き込むときに既定では HTML エンコードを使用します。 この新しい式は、次を効果的に変換します。

[!code-aspx[Main](overview/samples/sample67.aspx)]

たとえば、 &lt;%: 要求 ["UserInput"] %&gt; HTML エンコーディングの値を実行*要求 ["UserInput"]*です。

この機能の目的は、新しい構文を使用する各手順で決定する強制されないように、古い構文のすべてのインスタンスを置換できるようにすることです。 ただし、場合があります出力されているテキストが HTML を使用するものではまたは既にエンコードされている場合にダブル エンコードする可能性が。

ASP.NET 4 のような場合に、新しいインターフェイスが導入されています*IHtmlString*、具体的な実装と共に*HtmlString*です。 これらの型のインスタンスを示し、こと、戻り値は、既に正しくエンコードされて (またはそれ以外の場合) を HTML として表示するため、値がされないこと HTML エンコードされた再度できます。 たとえば、次することはできません (とは) HTML エンコードします。

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET MVC 2 のヘルパー メソッドは、ASP.NET 4 を実行しているときにこの新しい構文の動作は二重のエンコードするために更新されています。 この新しい構文では、ASP.NET 3.5 SP1 を使用してアプリケーションを実行するときに機能しません。

XSS 攻撃から保護は保証されないことに注意してください。 たとえば、引用符に含まれていない属性値を使用する HTML がユーザー入力を含めることができます。 ASP.NET コントロールや ASP.NET MVC ヘルパーの出力には常が含まれている属性値に引用符では推奨される方法に注意してください。

同様に、この構文では、ユーザー入力に基づいて JavaScript 文字列を作成する場合など、JavaScript のエンコードは行いません。

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>プロジェクト テンプレートの変更

以前のバージョンの ASP.NET では、Visual Studio を使用して、新しい Web サイト プロジェクトまたは Web アプリケーション プロジェクトを作成するときに結果として得られるプロジェクトでは、Default.aspx ページだけで、既定値`Web.config`ファイル、および`App_Data`では、次に示すように、フォルダー図:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio には、空の Web サイト プロジェクトの種類も含まれていないファイルも、次の図に示すようにもサポートされます。

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

結果は、初心者にとってはほとんどのガイダンスが実稼働 Web アプリケーションを構築する方法です。 そのため、ASP.NET 4 には、次の 3 つの新しいテンプレート、空の Web アプリケーション プロジェクトと 1 つ各プロジェクトの Web アプリケーションと Web サイトが導入されています。

#### <a name="empty-web-application-template"></a>空の Web アプリケーション テンプレート

名前が示すように、空の Web アプリケーション テンプレートは抑えた Web アプリケーション プロジェクトです。 次の図に示すように、Visual Studio の新しいプロジェクト ダイアログ ボックスからこのプロジェクト テンプレートを選択します。

[![](overview/_static/image7.png)](overview/_static/image6.png)

([フルサイズのイメージを表示するをクリックして](overview/_static/image8.png))

空の ASP.NET Web アプリケーションを作成するときに、Visual Studio には、次のフォルダーのレイアウトが作成されます。

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

以前のバージョンの ASP.NET、1 つの例外を空の Web サイトのレイアウトに似ています。 Visual Studio 2010 では、空の Web アプリケーションおよび空の Web サイト プロジェクトが含まれている、次の最小限`Web.config`を Visual Studio によって、プロジェクトのターゲット フレームワークの識別に使用される情報を含むファイル。

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

指定しない場合*targetFramework*プロパティ、Visual Studio の既定値は、古いバージョンのアプリケーションを開くときに、互換性を保つために、.NET Framework 2.0 を対象とします。

#### <a name="web-application-and-web-site-project-templates"></a>Web アプリケーションおよび Web サイト プロジェクト テンプレート

その他の 2 つ新しいプロジェクト テンプレート Visual Studio 2010 に付属するにはには、主要な変更が含まれて。 次の図は、新しい Web アプリケーション プロジェクトを作成するときに作成されるプロジェクト レイアウトを示しています。 (Web サイト プロジェクトのレイアウトは、ほぼ同じです)。

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

プロジェクトには、以前のバージョンで作成されなかったファイルの数が含まれています。 さらに、新しい Web アプリケーション プロジェクトが構成に基本メンバーシップ機能は、新しいアプリケーションへのアクセスをセキュリティで保護するを迅速に取得することができます。 この信頼のため、`Web.config`ファイルの新しいプロジェクトには、メンバーシップ、ロール、およびプロファイルを構成するためのエントリが含まれています。 次の例は、`Web.config`新しい Web アプリケーション プロジェクトのファイルです。 (この場合、 *roleManager*は無効になります)。

[![](overview/_static/image13.png)](overview/_static/image12.png)

([フルサイズのイメージを表示するをクリックして](overview/_static/image14.png))

プロジェクトは、2 つ目も含まれています。`Web.config`ファイルで、`Account`ディレクトリ。 2 番目の構成ファイルでは、ログに記録されないユーザー ChangePassword.aspx のページへのアクセスをセキュリティで保護する方法を提供します。 次の例は、2 番目の内容を示しています。`Web.config`ファイル。

![](overview/_static/image15.png)

既定では、新しいプロジェクト テンプレートで作成されたページには、以前のバージョンよりもより多くのコンテンツも含まれます。 プロジェクトには、既定のマスター ページ、CSS ファイルが含まれていて、既定では、既定のページ (Default.aspx) がマスター ページを使用するよう構成します。 結果は、最初に、Web アプリケーションまたは Web サイトを実行するときに既定値 (ホーム) ページを機能は既にです。 実際には、新しい MVC アプリケーションを起動するときに表示する既定のページに似ています。

[![](overview/_static/image17.png)](overview/_static/image16.png)

([フルサイズのイメージを表示するをクリックして](overview/_static/image18.png))

これらの変更をプロジェクト テンプレートの目的は、新しい Web アプリケーションの構築を開始する方法についてガイダンスを提供します。 意味的に正しい、厳密な XHTML 1.0 に準拠したマークアップ、CSS を使用して指定されているレイアウトで、テンプレート内のページは ASP.NET 4 Web アプリケーションを構築するためのベスト プラクティスを表します。 既定のページには、簡単にカスタマイズできる、2 列構成レイアウトもがあります。

たとえば、場合を考えます新しい Web アプリケーションの一部の色を変更し、My ASP.NET アプリケーションのロゴの代わりに、会社のロゴを挿入します。 これを行うには、下に新しいディレクトリを作成する`Content`ロゴのイメージを保存します。

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

ページに、イメージを追加するを開く、`Site.Master`マイ ASP.NET アプリケーションのテキストが定義され、コードに置き換えますを検索して、ファイル、*イメージ*要素が*src*新しいロゴに属性が設定されています。次の例のようにの画像:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([フルサイズのイメージを表示するをクリックして](overview/_static/image22.png))

Site.css ファイルに移動し、ページの背景色だけでなく、ヘッダーのものを変更する CSS クラスの定義を変更します。

これらの変更の結果を非常に簡単にカスタマイズされたホーム ページを表示することができます。

[![](overview/_static/image24.png)](overview/_static/image23.png)

([フルサイズのイメージを表示するをクリックして](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>CSS の機能強化

ASP.NET 4 での作業の主要な領域のいずれかを最新の HTML 標準に準拠した HTML のレンダリングを支援するされました。 これには、ASP.NET Web サーバー コントロールが CSS スタイルを使用する方法の変更が含まれます。

#### <a name="compatibility-setting-for-rendering"></a>レンダリングの互換性設定

既定では、Web アプリケーションまたは Web サイトは、.NET Framework 4 を対象の場合、 *controlRenderingCompatibilityVersion*の属性、*ページ*要素が「4.0」に設定します。 この要素がコンピューター レベルで定義されている`Web.config`ファイルし、既定ですべての ASP.NET 4 アプリケーションに適用されます。

[!code-xml[Main](overview/samples/sample69.xml)]

値は、 *controlRenderingCompatibility*潜在的な新しいバージョンの定義今後のリリースでは、文字列です。 現在のリリースでは、次の値がこのプロパティのサポートされています。

- "3.5". この設定は、従来のレンダリングやマークアップを示します。 コントロールによって表示されるマークアップが 100% の旧バージョンと互換性のあるの設定、*長期*プロパティが受け入れられます。
- "4.0". プロパティにこの設定がある場合は、ASP.NET Web サーバー コントロールは、次を行います。
- *長期*プロパティは常に"Strict"として扱われます。 その結果、コントロールは、XHTML 1.0 Strict マークアップを表示します。
- 不要になった非入力コントロールを無効にするには、無効なスタイルが表示されます。
- *div* CSS 規則をユーザーが作成したこれらの影響を与えないように、非表示フィールドの周りに要素がスタイルようになりました。
- メニュー コントロールは、意味的に正しく、ユーザ補助ガイドラインに準拠しているマークアップを表示します。
- 検証コントロールは、インライン スタイルをレンダリングできません。
- 以前の境界線をレンダリングするコントロール =「0」(ASP.NET から派生したコントロール*テーブル*制御、および ASP.NET*イメージ*コントロール) 不要になったこの属性を表示します。

#### <a name="disabling-controls"></a>コントロールを無効にします。

フレームワークは ASP.NET 3.5 SP1 およびそれ以前のバージョンで、レンダリング、*無効になっている*そのコントロールの HTML マークアップの属性*有効*プロパティに設定*false*です。 ただし、仕様に従った、HTML 4.01、のみ*入力*要素は、この属性を持つ必要があります。

ASP.NET 4 で設定することができます、 *controlRenderingCompatabilityVersion*を次の例のように「3.5」のプロパティ。

[!code-xml[Main](overview/samples/sample70.xml)]

用のマークアップを作成する場合があります、*ラベル*コントロールを無効にすると、次のようなコントロール。

[!code-aspx[Main](overview/samples/sample71.aspx)]

*ラベル*コントロールは、次の HTML をレンダリングします。

[!code-html[Main](overview/samples/sample72.html)]

ASP.NET 4 で設定することができます、 *controlRenderingCompatabilityVersion* 「4.0」にします。 その場合は、そのレンダリングでのみ制御*入力*要素は表示されます、*無効になっている*属性の場合に、コントロールの*有効*プロパティに設定されている*false*. HTML をレンダリングできませんコントロール*入力*要素をレンダリング、*クラス*を無効になっているコントロールの外観を定義している CSS クラスを参照する属性。 たとえば、*ラベル*先ほどの例に示すようにコントロールが、次のマークアップを生成します。

[!code-html[Main](overview/samples/sample73.html)]

このコントロールの指定したクラスの既定値は、"aspNetDisabled"です。 ただし、この既定値を変更するには、静的な*DisabledCssClass*の静的プロパティ、 *WebControl*クラスです。 コントロールの開発者の特定のコントロールを使用する動作も定義できますを使用して、 *SupportsDisabledAttribute*プロパティです。

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Div 要素を非表示のフィールドを非表示にします。

ASP.NET 2.0 とそれ以降のバージョンがシステムに固有の非表示フィールドを表示 (など、*隠し*ビューステート情報を格納するための要素) の内部*div* XHTML 標準に準拠するために要素。 ただし、この問題が発生 CSS 規則の影響を受ける場合*div*ページ上の要素。 たとえば、発生するページに表示される 1 ピクセル行非表示には約*div*要素。 ASP.NET 4 で*div* ASP.NET によって生成された非表示フィールドを囲む要素は、次の例のように CSS クラスの参照を追加します。

[!code-html[Main](overview/samples/sample74.html)]

のみ適用される CSS クラスを定義し、*隠し*次の例のように、ASP.NET によって生成される要素。

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>テンプレート コントロールの外部テーブルを表示

既定では、テンプレートをサポートする次の ASP.NET Web サーバー コントロールは自動的にインライン スタイルを適用するために使用する外部テーブルでラップされます。

- *FormView*
- *ログイン*
- *PasswordRecovery*
- *ChangePassword*
- *ウィザード*
- *CreateUserWizard*

という名前の新しいプロパティ*RenderOuterTable*マークアップから削除する外部テーブルは、これらのコントロールに追加されました。 たとえば、次の例の*FormView*コントロール。

[!code-aspx[Main](overview/samples/sample76.aspx)]

このマークアップでは、次の出力を HTML テーブルを含むページに表示します。

[!code-html[Main](overview/samples/sample77.html)]

テーブルが表示されないようにするには設定、 *FormView*コントロールの*RenderOuterTable*例を次のプロパティ。

[!code-aspx[Main](overview/samples/sample78.aspx)]

前の例は、次の出力、なしを表示、*テーブル*、 *tr*、および*td*要素。

> Content


この機能強化やすくスタイルの css でコントロールのコンテンツにため、コントロールによって予期しないタグは表示されません。

> [!NOTE]
> 注この変更が不要になったために、Visual Studio 2010 デザイナーで自動的に書式設定関数のサポートを無効にする、*テーブル*要素を自動的に書式設定オプションによって生成されたスタイル属性をホストできます。


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>ListView コントロールの機能強化

*ListView*コントロールを ASP.NET 4 でより使いやすくなりました。 コントロールの以前のバージョンの既知の ID を持つサーバー コントロールに含まれるレイアウト テンプレートを指定することが必要な 次のマークアップを使用する方法の一般的な例を示しています、 *ListView* ASP.NET 3.5 で制御します。

[!code-aspx[Main](overview/samples/sample79.aspx)]

ASP.NET 4 で、 *ListView*コントロールでは、レイアウト テンプレートは必要はありません。 前の例に示すようにマークアップは、次のマークアップで置換できます。

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList と RadioButtonList コントロールの機能強化

ASP.NET 3.5 では、レイアウトを指定することができます、 *CheckBoxList*と*RadioButtonList*次の 2 つの設定を使用します。

- *フロー*です。 コントロールをレンダリング*span*そのコンテンツを格納する要素。
- *テーブル*です。 コントロールを表示、*テーブル*要素をそのコンテンツが含まれています。

次の例は、これらのコントロールのマークアップを示しています。

[!code-aspx[Main](overview/samples/sample81.aspx)]

既定では、コントロールをレンダリング HTML 次のようにします。

[!code-html[Main](overview/samples/sample82.html)]

これらのコントロールには、意味的に正しい HTML を表示するために、項目の一覧が含まれているため、前に、HTML の一覧を使用してその内容を表示する必要があります (*li*) 要素です。 これは、やすく補助的な技術を使用して Web ページを読み取るし、コントロールを簡単に CSS を使用してスタイルをユーザー用です。

ASP.NET 4 で、 *CheckBoxList*と*RadioButtonList*コントロールに次の新しい値をサポートする、 *RepeatLayout*プロパティ。

- *OrderedList* – として、コンテンツが表示される*li*内の要素、 *ol*要素。
- *います*– として、コンテンツが表示される*li*内の要素、 *ul*要素。

次の例では、これらの新しい値を使用する方法を示します。

[!code-aspx[Main](overview/samples/sample83.aspx)]

上記のマークアップは、次の HTML を生成します。

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> 設定するかどうかに注意してください*RepeatLayout*に*OrderedList*または*しています*、 *RepeatDirection*プロパティが使用できなくでき、プロパティが、マークアップやコード内で設定されている場合は、実行時に例外をスローします。 プロパティはありません値これらのコントロールのビジュアルなレイアウトでは、代わりに CSS を使用して定義されているためです。


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>メニュー コントロールの機能強化

ASP.NET 4 の前に、*メニュー*コントロールは、一連の HTML テーブルを表示します。 これにより、インラインのプロパティの設定の外部で CSS スタイルを適用するより困難になることと、ユーザー補助の標準に準拠しているでしたも。

ASP.NET 4 で、コントロールはこれで順序なしリストとリストの要素で構成されるセマンティック マークアップを使用して HTML をレンダリングします。 次の例は、ASP.NET ページ用のマークアップを示しています、*メニュー*コントロール。

[!code-aspx[Main](overview/samples/sample85.aspx)]

コントロールが次の HTML を生成するページが表示される、(、 *onclick*コードがわかりやすくするため省略されました)。

[!code-html[Main](overview/samples/sample86.html)]

レンダリング機能強化に加え、メニューのキーボード ナビゲーションが改善されましたフォーカス管理を使用します。 ときに、*メニュー*コントロールがフォーカスを取得、方向キーを使用して要素を移動することができます。 *メニュー*コントロールはまた、アクセス可能なリッチ インターネット アプリケーション (ARIA) ロールを添付し、次の属性[翼、](http://www.w3.org/TR/wai-aria-practices/#menu "メニュー ARIA ガイドライン")改善ユーザー補助機能します。

上部にあるスタイル ブロックに沿ってレンダリングされる HTML 要素ではなく、ページのでは、メニュー コントロールのスタイルが表示されます。 コントロールのスタイル設定を完全に制御を実行する場合は、設定できます、新しい*IncludeStyleBlock*プロパティを*false*、後者スタイル ブロックは出力されません。 このプロパティを使用する方法の 1 つでは、機能を使用する、自動的に書式設定、Visual Studio デザイナーで、メニューの外観を設定します。 ことができますし、ページの実行、ページ ソースを開くし、コピー、レンダリングされたスタイル ブロック外部 CSS ファイルにします。 Visual Studio で、スタイル設定および設定を元に戻す*IncludeStyleBlock*に*false*です。 外部スタイル シートのスタイルを使用するメニューの外観を定義することになります。

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>ウィザードおよび CreateUserWizard コントロール

ASP.NET*ウィザード*と*CreateUserWizard*コントロールが HTML が表示されるかを定義できるようにするテンプレートをサポートします。 (*CreateUserWizard*から派生した*ウィザード*)。次の例は、完全にテンプレートのマークアップを示しています。 *CreateUserWizard*コントロール。

[!code-aspx[Main](overview/samples/sample87.aspx)]

コントロールは、次のような HTML を表示します。

[!code-html[Main](overview/samples/sample88.html)]

ASP.NET 3.5 sp1 では、テンプレートの内容を変更できますが、まだ制限されているの出力を制御、*ウィザード*コントロール。 ASP.NET 4 で作成することができます、 *LayoutTemplate*テンプレートおよび insert*プレース ホルダー* (予約済みの名前を使用) を制御する方法を指定する、*ウィザード コントロール*をレンダリングします。 次の例では、これは示しています。

[!code-aspx[Main](overview/samples/sample89.aspx)]

例では、次名前付きのプレース ホルダーにはが含まれています、 *LayoutTemplate*要素。

- *headerPlaceholder* – の内容で置き換えられます、実行時に、 *HeaderTemplate*要素。
- *sideBarPlaceholder* – の内容で置き換えられます、実行時に、 *SideBarTemplate*要素。
- *wizardStepPlaceHolder* – の内容で置き換えられます、実行時に、 *WizardStepTemplate*要素。
- *navigationPlaceholder* –、実行時に定義されているナビゲーション テンプレートで置き換えられます。

プレース ホルダーを使用しての例では、マークアップでは、次の HTML を表示します (実際には、テンプレートで定義されているコンテンツ) なし。

[!code-html[Main](overview/samples/sample90.html)]

ここで定義されていないユーザーの唯一の HTML が、 *span*要素。 (後のリリースでは、予想でも、 *span*要素はレンダリングされません)。今すぐ完全に制御によって生成されるほとんどすべてのコンテンツを*ウィザード*コントロール。

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC は、2009 年 3 月で、アドオン フレームワークとして、ASP.NET 3.5 SP1 に導入されました。 Visual Studio 2010 には、ASP.NET MVC 2 には、新機能と機能が含まれていますが含まれています。

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>領域のサポート

領域使用するグループのコント ローラーとビューを他のセクションからの相対的な分離で大規模なアプリケーションのセクションにできます。 各領域は、メイン アプリケーションによって、参照できる個別の ASP.NET MVC プロジェクトとして実装できます。 これにより、大規模なアプリケーションをビルドすると複雑性の管理を支援し、1 つのアプリケーションで連携して動作する複数のチームで簡単になります。

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>データ注釈属性の検証のサポート

*DataAnnotations*属性を使用して、メタデータ属性を使用して、モデルに検証ロジックを適用できます。 *DataAnnotations*属性は ASP.NET 3.5 SP1 での ASP.NET 動的データで導入されました。 これらの属性は、既定のモデル バインダーに統合されており、ユーザー入力を検証するメタデータ ドリブン手段を提供します。

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>テンプレート化されたヘルパー

テンプレート化されたヘルパーは、自動的に関連付ける編集ができ、データ型を持つテンプレートを表示します。 日付選択カレンダー UI 要素が自動的に表示されることを指定するテンプレート ヘルパーを使用するなど、 *System.DateTime*値。 ASP.NET 動的データのフィールド テンプレートに似ています。

*Html.EditorFor*と*Html.DisplayFor*ヘルパー メソッドには組み込みサポートがされている型の複数のプロパティと同様に複雑なオブジェクトの標準的なデータを表示します。 などのデータ注釈属性を適用することによってレンダリングをカスタマイズするも*DisplayName*と*ScaffoldColumn*を*ViewModel*オブジェクト。

多くの場合、さらに UI ヘルパーからの出力をカスタマイズして、生成されたものを完全に制御します。 *Html.EditorFor*と*Html.DisplayFor*ヘルパー メソッドをサポートしてこれをオーバーライドできます外部のテンプレートと表示される出力のコントロールを定義できるテンプレート メカニズムを使用します。 テンプレートは、クラスに対して個別にレンダリングできます。

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>動的データ

動的なデータが導入された 2008 中旬で .NET Framework 3.5 SP1 のリリースでします。 この機能は、次を含む、データ駆動型アプリケーションを作成するための多くの機能強化を提供します。

- データ ドリブン Web サイトの構築をすばやくの RAD エクスペリエンス。
- データ モデルで定義された制約に基づく自動検証。
- 内のフィールドに対して生成されるマークアップを簡単に変更すること、 *GridView*と*DetailsView*動的なデータ プロジェクトの一部であるフィールド テンプレートを使用して制御します。

> [!NOTE]
> 注の詳細についてを参照してください、[動的データ ドキュメント](https://msdn.microsoft.com/library/cc488545.aspx)MSDN ライブラリです。


ASP.NET 4 では、データ ドリブン Web サイトをすばやく構築するためのさらに多くの電源を開発者に提供する動的なデータが強化されました。

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>既存のプロジェクトの動的なデータの有効化

.NET Framework 3.5 SP1 に付属している動的データ機能では、新機能は次のようになります。

- フィールド テンプレート – これらテンプレートを使用できるデータ型に基づくデータ バインド コントロールのです。 フィールド テンプレートは、各フィールドのテンプレートのフィールドを使用するよりもデータ コントロールの外観をカスタマイズする簡単な方法を提供します。
- 検証: 動的なデータをで属性クラスを使用してデータを必要なフィールド、範囲のチェック、型チェック、正規表現を使用して一致するパターンなどの一般的なシナリオの検証とカスタム検証を指定できます。 検証は、データ コントロールによって強制されます。

ただし、これらの機能には、次の要件が必要があります。

- データ アクセス層は、Entity Framework または LINQ to SQL に基づいている必要があります。
- 唯一のデータ ソースのこれらの機能がサポートされているコントロール、 *EntityDataSource*または*LinqDataSource*コントロール。
- 機能には、機能をサポートする必要があるすべてのファイルがあるために、動的なデータまたは動的データ エンティティ テンプレートを使用して作成した Web プロジェクトが必要です。

ASP.NET 4 での動的データ サポートの主な目標は、ASP.NET アプリケーションの動的なデータの新しい機能を有効にするのにです。 次の例は、利用できる動的なデータ機能の既存のページにコントロールのマークアップを示しています。

[!code-aspx[Main](overview/samples/sample91.aspx)]

ページのコードでは、これらのコントロールの動的データ サポートを有効にするために、次のコードを追加する必要があります。

[!code-csharp[Main](overview/samples/sample92.cs)]

ときに、 *GridView*コントロールが編集モードでは、動的なデータが自動的に適切な形式で入力したデータが検証されます。 そうでない場合は、エラー メッセージが表示されます。

この機能は、その他のメリットも用意されています。、値を挿入モードの既定値を指定することなどです。 動的なデータがない場合、フィールドの既定値を実装する必要がありますイベントにアタッチ、コントロールの検索 (を使用して*FindControl*)、およびその値を設定します。 ASP.NET 4 で、 *EnableDynamicData*呼び出しは、この例で示すように、オブジェクトのいずれかのフィールドの既定値を渡すことができますを 2 番目のパラメーターをサポートしています。

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>DynamicDataManager コントロールを宣言の構文

*DynamicDataManager*宣言によって、構成できるようにするコントロールが強化されましたのみでコードの代わりに、ASP.NET では、ほとんどのコントロールと同様です。 マークアップ、 *DynamicDataManager*コントロールは次の例のようになります。

[!code-aspx[Main](overview/samples/sample94.aspx)]

このマークアップで参照されている GridView1 コントロールの動的なデータの動作を有効に、 *DataControls*のセクションで、 *DynamicDataManager*コントロール。

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>エンティティのテンプレート

エンティティのテンプレートでは、カスタム ページを作成することがなくデータのレイアウトをカスタマイズする新しい方法を提供します。 ページのテンプレートを使用して、 *FormView*コントロール (の代わりに、 *DetailsView*コントロールが以前のバージョンの動的なデータ ページ テンプレートで使用)、および*DynamicEntity*エンティティのテンプレートをレンダリングするコントロール。 動的なデータを表示するマークアップをより詳細に制御できます。

次に、エンティティ テンプレートを含む新しいプロジェクト ディレクトリ レイアウトを示します。

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate`ディレクトリには、データ モデル オブジェクトを表示する方法のテンプレートが含まれています。 使用して既定では、オブジェクトがレンダリングされる、`Default.ascx`によって作成されたマークアップと同じように検索するマークアップを提供するテンプレート、 *DetailsView* ASP.NET 3.5 SP1 での動的なデータによって使用されるコントロールです。 次の例では、マークアップを`Default.ascx`コントロール。

[!code-aspx[Main](overview/samples/sample96.aspx)]

サイト全体の外観を変更するのには、既定のテンプレートを編集できます。 表示、編集、および挿入操作用のテンプレートがあります。 新しいテンプレート追加できますデータ オブジェクトの名前に基づいてオブジェクトの種類を 1 つだけの外観を変更するためにします。 たとえば、次のテンプレートを追加できます。

[!code-console[Main](overview/samples/sample97.cmd)]

テンプレートには、次のマークアップが含まれます。

[!code-aspx[Main](overview/samples/sample98.aspx)]

新しいエンティティ テンプレートは、新しいを使用して、ページに表示される*DynamicEntity*コントロール。 実行時に、このコントロールは、エンティティ テンプレートの内容に置き換えられます。 次のマークアップに示します、 *FormView*内の制御、`Detail.aspx`エンティティ テンプレートを使用するページのテンプレートです。 通知、 *DynamicEntity*マークアップ内の要素。

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Url および電子メール アドレスの新しいフィールド テンプレート

ASP.NET 4 では、次の 2 つの新しい組み込みフィールド テンプレート`EmailAddress.ascx`と`Url.ascx`です。 これらのテンプレートとしてマークされているフィールドの使用は*EmailAddress*または*Url*で、 *DataType*属性。 *EmailAddress*オブジェクトを使用して作成されるハイパーリンクとして、フィールドが表示されます、 *mailto:*プロトコルです。 ユーザーは、リンクをクリックして、それとが開き、ユーザーの電子メール クライアント スケルトンのメッセージを作成します。 オブジェクトとして型指定された*Url*は通常のハイパーリンクとして表示されます。

次の例では、フィールドとマークする方法を示します。

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>DynamicHyperLink コントロールとのリンクを作成します。

動的なデータは、エンドユーザーが、Web サイトにアクセスするときに表示される Url を制御するには、.NET Framework 3.5 SP1 で追加された、新しいルーティングの機能を使用します。 新しい*DynamicHyperLink*コントロールでは、簡単に動的データ サイト内のページへのリンクを作成できます。 次の例を使用する方法を示しています、 *DynamicHyperLink*コントロール。

[!code-aspx[Main](overview/samples/sample101.aspx)]

このマークアップのリスト ページを参照するリンクの作成、`Products`テーブルで定義されているルートに基づいて、`Global.asax`ファイル。 コントロールでは、動的なデータ ページの基になっている既定のテーブル名が自動的に使用します。

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>データ モデルにおける継承のサポート

Entity Framework でも LINQ to SQL の両方のデータ モデルで継承をサポートします。 この例を持つデータベースである可能性があります、`InsurancePolicy`テーブル。 含まれる場合も`CarPolicy`と`HousePolicy`と同じフィールドを持つテーブルを`InsurancePolicy`しより多くのフィールドを追加します。 データ モデルで継承されたオブジェクトを理解して、継承されたテーブルのスキャフォールディングをサポートするために、動的なデータが変更されました。

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>多対多のリレーションシップ (Entity Framework) のサポート

Entity Framework のコレクションとしてのリレーションシップを公開することで実装されているテーブル間の多対多リレーションシップの豊富なサポートでは、*エンティティ*オブジェクト。 新しい`ManyToMany.ascx`と`ManyToMany_Edit.ascx`を表示して、多対多リレーションシップに関係するデータの編集をサポートするフィールド テンプレートが追加されました。

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>新しい属性をコントロールの表示と列挙のサポート

*DisplayAttribute*フィールドの表示方法を詳細に制御することが追加されました。 *DisplayName*以前のバージョンを使用するフィールドのキャプションとして使用される名前を変更する動的なデータの属性です。 新しい*DisplayAttribute*クラスを使用して、順序、フィールドを表示およびフィールドをフィルターとして使用するかどうかなど、フィールドを表示するための詳細オプションを指定できます。 属性は、独立したコントロールのラベルに使用される名前のも用意されています、 *GridView*で使用される名前を制御する、 *DetailsView*のレベルのウォーターマークが使用されると、フィールドのヘルプ テキストを制御します。、。フィールド (フィールドは、テキスト入力を受け入れる) 場合です。

*EnumDataTypeAttribute*列挙体にフィールドをマップできるクラスが追加されました。 フィールドにこの属性を適用する場合は、列挙型を指定します。 動的なデータを使用して、新しい`Enumeration.ascx`フィールド テンプレートを表示および編集する列挙値の UI を作成します。 テンプレートは、列挙体の名前に、データベースから値をマップします。

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>フィルターのサポートの強化

ブール型の列と外部キー列の組み込みフィルターを使用して動的データ 1.0 が付属しています。 フィルターでは、それらに表示されていたか、どのような順序では表示を指定不可能です。 新しい*DisplayAttribute*属性のアドレスを両方の提供することでこれらの問題をフィルターとして列を表示するかどうかを制御し、どのような順序で表示されます。

追加の機能強化がフィルター処理のサポートされていること[、new を使用して書き込む](#0.2__QueryExtender "_QueryExtender") Web フォームの機能。 これにより、フィルターで使用されるデータ ソース コントロールの知識がなくても、フィルターを作成できます。 これらの拡張機能、およびフィルターもなっているコントロールのテンプレートに新しいものを追加することができます。 最後に、 *DisplayAttribute*述べたクラスが同じで、オーバーライドできる既定のフィルターを使用する方法*UIHint*オーバーライドされるように列の既定のフィールド テンプレートを使用します。

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web 開発の改良

Visual Studio 2010 での web 開発は、互換性のため大きい CSS、HTML および ASP.NET のマークアップ スニペットと新しい動的 IntelliSense JavaScript による生産性の向上に拡張されています。

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>強化された CSS 互換性

Visual Studio 2010 で Visual Web Developer デザイナーが CSS 2.1 基準のコンプライアンス対応を向上させるために更新されました。 デザイナーは、HTML ソースの整合性を維持されは、Visual Studio の以前のバージョンよりも堅固です。 フードのアーキテクチャの改善によっても加えられたよりレンダリング、レイアウト、および保守で将来の機能強化を有効にすることになります。

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML および JavaScript スニペット

HTML エディターで IntelliSense をオートコンプリート タグ名。 IntelliSense スニペット機能をオートコンプリート タグ全体などです。 Visual Studio 2010 では、IntelliSense スニペットは、javascript、c# および Visual Basic、Visual Studio の以前のバージョンでサポートされていたと共にサポートされます。

Visual Studio 2010 には、オート コンプリート一般的な ASP.NET と HTML タグ、必須の属性を含む役立つ 200 を超えるのスニペットが含まれています (など runat ="server") と、タグに固有の共通の属性 (など*ID*、 *DataSourceID*、 *ControlToValidate*、および*テキスト*)。

他のスニペットをダウンロードすることも自分やチームの一般的なタスクを使用するマークアップのブロックをカプセル化する、独自のスニペットを記述することができます。

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>JavaScript IntelliSense の機能強化

Visual 2010 で JavaScript IntelliSense は再設計されましたより豊富な編集エクスペリエンスを提供します。 IntelliSense が動的に生成されたメソッドでなどのオブジェクトを今すぐ認識*registerNamespace*他の JavaScript フレームワークで使用される同様の手法です。 スクリプトの大規模なライブラリを分析して、ほとんどまたはまったく処理の遅延で IntelliSense を表示するパフォーマンスが向上します。 互換性が大幅に増加したほとんどすべてのサード パーティ製ライブラリをサポートし、多様なコーディング スタイルをサポートするためです。 ドキュメント コメントは、入力して、IntelliSense によってすぐに活用できるように今すぐ解析されます。

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Visual Studio 2010 で web アプリケーションの展開

ASP.NET 開発者は、Web アプリケーションを展開、多くの場合、検索するなど、次の問題に遭遇します。

- 共有のホスト サイトに展開するには、低速であることができます、FTP などのテクノロジが必要です。 また、手動でデータベースを構成する SQL スクリプトを実行しているなどのタスクを実行する必要があり、アプリケーションとして、仮想ディレクトリ フォルダーを構成するなどの IIS 設定を変更する必要があります。
- Web アプリケーション ファイルを展開するだけでなく、エンタープライズ環境で管理者頻繁にする必要があります変更 ASP.NET 構成ファイルおよび IIS の設定。 データベース管理者は、一連の実行中のアプリケーション データベースを取得する SQL スクリプトを実行する必要があります。 このようなインストールに相当する労力は、多くの場合、完了までに時間がかかると、慎重に文書化する必要があります。

Visual Studio 2010 には、これらの問題に対処して、シームレスに Web アプリケーションを展開するためのテクノロジが含まれています。 これらのテクノロジの 1 つは、IIS Web 配置ツール (MsDeploy.exe) です。

Visual Studio 2010 で web デプロイ機能では、次の主要な領域があります。

- Web のパッケージ化
- Web.config 変換
- データベースの配置
- 1 回のクリックを Web アプリケーションを発行します。

次のセクションでは、これらの機能に関する詳細情報を提供します。

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Web のパッケージ化

Visual Studio 2010 MSDeploy ツールを使用すると呼ばれますが、アプリケーションの圧縮 (.zip) ファイルを作成、 *Web パッケージ*です。 パッケージ ファイルには、アプリケーションのほか、次のコンテンツに関するメタデータが含まれています。

- IIS の設定、アプリケーション プールの設定や、エラー ページの設定が含まれています。
- 実際 Web コンテンツ、Web ページには、ユーザー コントロールには、静的なコンテンツ (イメージや HTML ファイル) が含まれます。
- SQL Server データベース スキーマとデータ。
- セキュリティ証明書は、GAC、レジストリ設定でインストールするコンポーネントです。

Web パッケージを任意のサーバーにコピーされ、IIS マネージャーを使用して手動でインストールされていることができます。 または、展開の自動化、パッケージをインストールするコマンド ライン コマンドを使用して、または配置 Api を使用しています。

Visual Studio 2010 は、組み込みの MSBuild タスクと Web のパッケージを作成するターゲットを提供します。 詳細については、次を参照してください。 [ASP.NET Web アプリケーション プロジェクトの展開の概要](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx)MSDN Web サイトおよび[10 + 20 上の理由から、Web のパッケージを作成する必要があります理由](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html)Vishal Joshi ブログ。

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web.config 変換

Web アプリケーションの配置に対して Visual Studio 2010 が導入されて[XML ドキュメントの変換 (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)、変換する機能は、`Web.config`ファイルの開発設定を実稼働の設定にします。 変換の設定がという名前の変換ファイルで指定された`web.debug.config`、`web.release.config`のようにします。 (これらのファイルの名前は一致 MSBuild 構成です)。変換ファイルには、展開済みにする必要がある変更のみが含まれています。`Web.config`ファイル。 変更内容を指定するには、単純な構文を使用します。

次の例の一部を示しています、`web.release.config`リリース構成の展開用にファイルが生成される可能性があります。 置換キーワードの例を指定する配置時に、 *connectionString*内のノード、`Web.config`ファイルは例に記載されている値に置き換えられます。

[!code-xml[Main](overview/samples/sample102.xml)]

詳細については、次を参照してください[Web アプリケーション プロジェクトの配置の Web.config 変換構文](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx)を MSDN <a id="0.2_a"> </a> Web サイトおよび[Web 展開: Web.Config 変換](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)。Vishal Joshi ブログ。

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>データベースの配置

Visual Studio 2010 の配置パッケージには、SQL Server データベースへの依存関係を含めることができます。 パッケージの定義の一部として、ソース データベースの接続文字列を提供します。 Web パッケージを作成するときに、Visual Studio 2010 は、データベース スキーマについて、必要に応じて、データの SQL スクリプトの作成し、これらをパッケージに追加されます。 カスタムの SQL スクリプトを提供し、サーバーで実行する必要がありますシーケンスを指定できます。 展開時に、対象サーバーを適切な接続文字列を指定します。展開プロセスは、この接続文字列を使用して、データベース スキーマを作成し、データを追加するスクリプトを実行します。

さらに、1 回のクリックを使用して、発行、アプリケーションがリモート共有ホスティング サイトに発行されると、データベースを直接発行する展開を構成することができます。 詳細については、次を参照してください。[する方法: Web アプリケーション プロジェクトでのデータベースを配置](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx)MSDN Web サイトおよび[VS 2010 でのデータベースの配置](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi ブログ。

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>1 回のクリックを Web アプリケーションを発行します。

Visual Studio 2010 では、リモート サーバーに Web アプリケーションを公開する、IIS のリモート管理サービスを使用することもできます。 サーバーをテストまたはステージング サーバーまたはホスト アカウントの発行プロファイルを作成できます。 各プロファイルは、適切な資格情報を安全に保存できます。 ターゲットのいずれかに展開することができます Web 1 回のクリックを使用して 1 回のクリックでのサーバーは、ツールバーを公開します。 Visual Studio 2010 で、MSBuild コマンドラインを使用して公開することもできます。 これにより発行を継続的インテグレーション モデルに含めるチーム ビルド環境を構成できます。

詳細については、次を参照してください。[する方法: Web アプリケーション プロジェクトを使用して 1 回のクリックの 発行および Web デプロイのデプロイ](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx)MSDN Web サイトと[VS 2010 を 1 クリック発行 Web](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi ブログ。 Visual Studio 2010 で Web アプリケーションのデプロイに関するビデオ プレゼンテーションを表示するを参照してください。 [Web Developer Preview の VS 2010](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Vishal Joshi ブログ。

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>リソース

次の Web サイトでは、ASP.NET 4 および Visual Studio 2010 に関する追加情報を提供します。

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) -MSDN Web サイトで ASP.NET 4 用の公式のドキュメントです。
- [https://www.asp.net/](https://www.asp.net/) -ASP.NET チームの Web サイトです。
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) および[ASP.NET 動的データのコンテンツ マップ](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx)-チーム サイトで ASP.NET し ASP.NET 動的データの公式のドキュメントでのオンライン リソース。
- [https://www.asp.net/ajax/](../../ajax/index.md) -メインの Web リソースを ASP.NET Ajax 開発します。
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Visual Web Developer チーム ブログ Visual Studio 2010 の機能に関する情報が含まれています。
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) : ASP.NET のプレビュー リリースの Web リソースをメインです。

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>免責情報

このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。

このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。 マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。

このホワイト ペーパーは、情報提供のみを目的としています。 マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。

お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。 このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。

マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。 別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。

特に明記されない限り、使用している会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、およびイベント実在は、架空のもの、および関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所またはイベントはまたは意図して推論する必要があります。

© 2009 Microsoft Corporation. All rights reserved.

Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。

本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。
