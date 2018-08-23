---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 および Visual Studio 2010 Web 開発の概要 |Microsoft Docs
author: rick-anderson
description: このドキュメントは、ASP.NET と Visual Studio 2010 での.net Framework 4 に含まれている数多くの新しい機能の概要を提供します。
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 775286df610df9040cbf04125b1742b6befa055b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828026"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 および Visual Studio 2010 Web 開発の概要
====================
> このドキュメントは、ASP.NET と Visual Studio 2010 での.net Framework 4 に含まれている数多くの新しい機能の概要を提供します。
> 
> [このホワイト ペーパーをダウンロードします。](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**目次**

**[コア サービス](#0.2__Toc253429238 "_Toc253429238")**  
[Web.config ファイルがリファクタリング](#0.2__Toc253429239 "_Toc253429239")  
[拡張可能な出力キャッシュ](#0.2__Toc253429240 "_Toc253429240")  
[Web アプリケーションの自動開始](#0.2__Toc253429241 "_Toc253429241")  
[永続的なリダイレクト ページ](#0.2__Toc253429242 "_Toc253429242")  
[セッション状態を圧縮](#0.2__Toc253429243 "_Toc253429243")  
[許容される Url の範囲を拡大](#0.2__Toc253429244 "_Toc253429244")  
[拡張可能な要求の検証](#0.2__Toc253429245 "_Toc253429245")  
[オブジェクト キャッシュとキャッシュの機能拡張オブジェクト](#0.2__Toc253429246 "_Toc253429246")  
[拡張可能な HTML、URL、および HTTP ヘッダーのエンコーディング](#0.2__Toc253429247 "_Toc253429247")  
[1 つのワーカー プロセス内の個々 のアプリケーションのパフォーマンスを監視](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[含まれる Web フォームと MVC の jQuery](#0.2__Toc253429251 "_Toc253429251")  
[コンテンツ配信ネットワーク サポート](#0.2__Toc253429252 "_Toc253429252")  
[Scriptmanager コントロールの明示的なスクリプト](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Page.MetaKeywords Page.MetaDescription プロパティとメタ タグを設定](#0.2__Toc253429257 "_Toc253429257")  
[個々 のコントロール ビュー ステートを有効にする](#0.2__Toc253429258 "_Toc253429258")  
[ブラウザーの機能への変更](#0.2__Toc253429259 "_Toc253429259")  
[ASP.NET 4 でのルーティング](#0.2__Toc253429260 "_Toc253429260")  
[クライアント Id を設定](#0.2__Toc253429261 "_Toc253429261")  
[データ コントロールで行の選択を保持する](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET のグラフ コントロール](#0.2__Toc253429263 "_Toc253429263")  
[による QueryExtender コントロールでデータをフィルター処理](#0.2__Toc253429264 "_Toc253429264")  
[Html エンコードのコード式](#0.2__Toc253429265 "_Toc253429265")  
[テンプレートの変更をプロジェクト](#0.2__Toc253429266 "_Toc253429266")  
[CSS の改良点](#0.2__Toc253429267 "_Toc253429267")  
[Div を非表示フィールドの周囲の要素を非表示](#0.2__Toc253429268 "_Toc253429268")  
[テンプレート化されたコントロールの外側のテーブルをレンダリング](#0.2__Toc253429269 "_Toc253429269")  
[ListView コントロールの機能強化](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList、RadioButtonList コントロールの強化](#0.2__Toc253429271 "_Toc253429271")  
[メニュー コントロールの機能強化](#0.2__Toc253429272 "_Toc253429272")  
[ウィザードおよび CreateUserWizard コントロール 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[領域のサポート](#0.2__Toc253429275 "_Toc253429275")  
[データ注釈属性の検証サポート](#0.2__Toc253429276 "_Toc253429276")  
[テンプレート化されたヘルパー](#0.2__Toc253429277 "_Toc253429277")

**[動的データ](#0.2__Toc253429278 "_Toc253429278")**  
[既存のプロジェクトに対して動的なデータを有効にする](#0.2__Toc253429279 "_Toc253429279")  
[DynamicDataManager コントロールの宣言型構文](#0.2__Toc253429280 "_Toc253429280")  
[エンティティ テンプレート](#0.2__Toc253429281 "_Toc253429281")  
[Url および電子メール アドレスの新しいフィールド テンプレート](#0.2__Toc253429282 "_Toc253429282")  
[DynamicHyperLink コントロールへのリンクを作成する](#0.2__Toc253429283 "_Toc253429283")  
[データ モデルにおける継承のサポート](#0.2__Toc253429284 "_Toc253429284")  
[多対多のリレーションシップ (Entity Framework) のサポート](#0.2__Toc253429285 "_Toc253429285")  
[コントロールの表示と列挙のサポートを新しい属性](#0.2__Toc253429286 "_Toc253429286")  
[フィルターのサポートが強化されて](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web 開発の改良](#0.2__Toc253429288 "_Toc253429288")**  
[CSS に関する互換性の改善](#0.2__Toc253429289 "_Toc253429289")  
[HTML および JavaScript スニペット](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript IntelliSense の機能強化](#0.2__Toc253429291 "_Toc253429291")

**[Visual Studio 2010 でアプリケーションのデプロイを web](#0.2__Toc253429292 "_Toc253429292")**  
[Web パッケージ](#0.2__Toc253429293 "_Toc253429293")  
[Web.config 変換](#0.2__Toc253429294 "_Toc253429294")  
[データベース配置](#0.2__Toc253429295 "_Toc253429295")  
[1 回のクリックは、Web アプリケーションの発行](#0.2__Toc253429296 "_Toc253429296")  
[Resources](#0.2__Toc253429297 "_Toc253429297")

**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>コア サービス

ASP.NET 4 では、さまざまな出力のキャッシュおよびセッション状態の記憶域などの ASP.NET コア サービスを改善する機能が導入されています。

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Web.config ファイルのリファクタリング

`Web.config` Web アプリケーションが大きく成長、.NET Framework のいくつか過去のリリースで、Ajax など、新しい機能が追加されているように構成を含んでいるファイル ルーティング、および IIS 7 との統合。 これはで、構成、または Visual Studio などのツールを使用せずに新しい Web アプリケーションを起動するは困難です。 .NET Framework 4 では、主要な構成要素を移動されていますが、`machine.config`ファイル、およびアプリケーションがこれらの設定を継承するようになりました。 これにより、`Web.config`を空にするかにのみ、次の行を Visual Studio のアプリケーションが対象とする framework のバージョンを指定します。 これを含めることが、ASP.NET 4 アプリケーション内のファイル。

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>拡張可能な出力キャッシュ

ASP.NET 1.0 がリリースされた時刻、以降は、出力キャッシュは、ページ、コントロール、および HTTP 応答の生成された出力をメモリに格納する開発者に有効に。 以降の Web 要求で ASP.NET 使用できるコンテンツより迅速にゼロからの出力を再生成する代わりに、メモリから生成された出力を取得することによって。 ただし、このアプローチには、制限、生成されたコンテンツは常にメモリに格納されるが、大量のトラフィックが発生しているサーバーで出力キャッシュによって消費されるメモリは、Web アプリケーションの他の部分からのメモリ要求と競合ことができます。

ASP.NET 4 では、1 つまたは複数のカスタム出力キャッシュ プロバイダーを構成することができますを出力キャッシュの機能拡張ポイントを追加します。 出力キャッシュ プロバイダーは、任意のストレージ メカニズムを使用して、HTML コンテンツを保持することができます。 これにより、多様な永続化メカニズムは、ローカルまたはリモートのディスクを含めることができます、クラウドの記憶域のカスタム出力キャッシュ プロバイダーを作成することにより、なり、分散キャッシュ エンジン。

新しいから派生するクラスとしてカスタム出力キャッシュ プロバイダーを作成する*System.Web.Caching.OutputCacheProvider*型。 プロバイダーを構成することができますし、`Web.config`新しいを使用してファイル*プロバイダー*のサブセクション、 *outputCache*要素は、次の例に示すように。

[!code-xml[Main](overview/samples/sample2.xml)]

ASP.NET 4 では、すべての HTTP 応答で既定で表示するページ、およびコントロールの前の例に示すように、メモリ内の出力キャッシュを使用している、 *defaultProvider* AspNetInternalProvider に属性を設定します。 別のプロバイダー名を指定して Web アプリケーションの使用、既定の出力キャッシュ プロバイダーを変更する*defaultProvider*します。

さらに、コントロールおよび要求ごとに別の出力キャッシュ プロバイダーを選択できます。 別の Web ユーザー コントロールの別の出力キャッシュ プロバイダーを選択する最も簡単な方法は、new を使用して、宣言を行う、 *providerName*コントロール ディレクティブでは、次の例に示すように属性します。

[!code-aspx[Main](overview/samples/sample3.aspx)]

HTTP 要求の別の出力キャッシュ プロバイダーを指定するには、少し作業が必要です。 新しい上書きプロバイダーを宣言によって指定するには、代わりに*GetOuputCacheProviderName*メソッドで、`Global.asax`ファイルをプログラムで特定の要求に使用するプロバイダーを指定します。 その方法を次の例に示します。

[!code-csharp[Main](overview/samples/sample4.cs)]

ASP.NET 4 を出力キャッシュ プロバイダー機能拡張の追加により、Web サイトのようになりましたよりアグレッシブなとより高度な出力キャッシュの戦略を追求できることにします。 たとえば、ディスク上のトラフィックの削減を取得するページをキャッシュ中には、メモリ内のサイトの「トップ 10」ページをキャッシュすることはようになりました。 または、キャッシュ、レンダリングされるページのすべての変更によって組み合わせは、フロント エンド Web サーバーのメモリ使用量の負荷を軽減するために、分散キャッシュを使用できます。

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Web アプリケーションの自動開始

一部の Web アプリケーションは、大量のデータを読み込んだり、最初の要求を処理する前に処理負荷のかかる初期化を実行する必要があります。 以前のバージョンの ASP.NET では、このような状況にする必要がありました ASP.NET アプリケーションを「起こす」し、時に初期化コードを実行するカスタム方法を考案、*アプリケーション\_ロード*メソッドで、 `Global.asax`ファイルです。

という名前の新しいスケーラビリティ機能*自動開始*ことアドレスこのシナリオは、使用可能な直接 ASP.NET 4 で実行すると Windows Server 2008 R2 に IIS 7.5 です。 自動開始機能は、アプリケーション プールの開始、ASP.NET アプリケーションの初期化、および HTTP 要求の受け入れのための制御方法を提供します。

> [!NOTE] 
> 
> Iis 7.5、IIS アプリケーション ウォーム アップ モジュール
> 
> For IIS 7.5、IIS チームはアプリケーションのウォーム アップ モジュールの最初のベータ版のテスト バージョンに公開しました。 これにより、前に説明したよりもずっと簡単にアプリケーションを準備します。 カスタム コードを記述する代わりに、Web アプリケーションがネットワークからの要求を受け入れる前に実行するリソースの Url を指定します。 このウォーム アップは、IIS サービスの起動処理中に発生します (として IIS アプリケーション プールを構成した場合*AlwaysRunning*) して、IIS ワーカー プロセスが再利用されます。 、リサイクル中には、古い IIS ワーカー プロセスは、アプリケーションの中断なしまたは unprimed キャッシュのための他の問題が発生するように、新しく作成されるワーカー プロセスが、ウォームが完全にまで要求を実行し続けます。 このモジュールがバージョン 2.0 以降、ASP.NET の任意のバージョンで動作するに注意してください。
> 
> 詳細については、次を参照してください。 [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) 、IIS.net Web サイト。 ウォーム アップ機能を使用する方法について説明するチュートリアルでは、次を参照してください。[入門 IIS 7.5 アプリケーション ウォーム アップ モジュール](https://www.iis.net/learn/manage)、IIS.net Web サイト。


Auto-start 機能を使用して、IIS 管理者では、次の構成を使用して自動的に起動する IIS 7.5 におけるアプリケーション プールを設定、`applicationHost.config`ファイル。

[!code-xml[Main](overview/samples/sample5.xml)]

次の構成を使用して自動的に開始する個々 のアプリケーションを指定するため、1 つのアプリケーション プールは、複数のアプリケーションを含めることができます、`applicationHost.config`ファイル。

[!code-xml[Main](overview/samples/sample6.xml)]

IIS 7.5 サーバーがコールド起動の場合、または個別のアプリケーション プールがリサイクルされるときに、IIS 7.5 は、情報を使用して、`applicationHost.config`ファイルが自動的に開始する Web アプリケーションです。 自動開始をマークされている各アプリケーションの IIS7.5 は ASP.NET 4 をアプリケーション一時的に受け入れません状態でアプリケーションを起動する HTTP 要求に、要求を送信します。 ASP.NET がによって定義された型をインスタンス化がこの状態にあるときに、 *serviceAutoStartProvider* (されている前の例で示した) 属性とそのパブリック エントリ ポイントへの呼び出し。

実装することで、必要なエントリ ポイントで管理された自動開始の種類を作成する、 *IProcessHostPreloadClient*インターフェイスを次の例に示すようにします。

[!code-csharp[Main](overview/samples/sample7.cs)]

初期化後にコードを実行、*プリロード*メソッドとメソッドから制御が戻る、ASP.NET アプリケーションに要求を処理する準備ができます。

.5 IIS および ASP.NET 4 に自動開始の追加により、最初の HTTP 要求を処理する前に、負荷の高いアプリケーションの初期化を実行するための適切に定義された方法があるようになりました。 たとえば、新しい自動開始機能を使用して、アプリケーションを初期化して、アプリケーションが初期化され、HTTP トラフィックを受け入れる準備ができてであるロード バランサーを通知することができます。

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>完全にページにリダイレクトします。

検索エンジンに古くなったリンクの累積が発生する可能性があります時間の経過と共にページおよびその他のコンテンツの周囲を移動する Web アプリケーションで一般的です。 ASP.NET では、開発者がこれまで古い Url への要求を使用して処理を使用して、 *Response.Redirect*新しい URL に要求を転送するメソッド。 ただし、*リダイレクト*メソッドは、ユーザーが、古い Url にアクセスしようとしています。 ときに、余分な http ラウンド トリップ結果、HTTP 302 Found (一時的なリダイレクト) 応答を発行します。

ASP.NET 4 に新しく追加*RedirectPermanent*ヘルパー メソッドで簡単に HTTP 301 の問題を次の例のように、応答を完全に移動します。

[!code-csharp[Main](overview/samples/sample8.cs)]

検索エンジンと永続的なリダイレクトを認識するその他のユーザー エージェントは、一時的なリダイレクトのブラウザーによって行われた不要なラウンド トリップを排除すると、コンテンツに関連付けられている新しい URL に格納されます。

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>セッション状態を圧縮します。

ASP.NET Web ファーム全体でセッション状態を格納するための 2 つの既定オプションを提供します。 セッション状態プロバイダーをアウト プロセス セッション状態サーバーを呼び出すと、Microsoft SQL Server データベースにデータを格納するセッション状態プロバイダー。 両方のオプションは、Web アプリケーションのワーカー プロセスの外部の状態情報を格納するが含まれる、ために、セッション状態をリモート ストレージに送信される前にシリアル化する必要があります。 によって、開発者がセッション状態の保存情報の量、シリアル化されたデータのサイズが非常に大きく拡張可能です。

ASP.NET 4 では、アウト プロセス セッション状態プロバイダーの両方の種類の新しい圧縮オプションを紹介します。 ときに、 *compressionEnabled*に次の例に示すように構成オプションが設定されている*true*ASP.NET は圧縮 (および圧縮解除) .NETFrameworkを使用してシリアル化されたセッションの状態*System.IO.Compression.GZipStream*クラス。

[!code-xml[Main](overview/samples/sample9.xml)]

単純に新しい属性の追加、`Web.config`ファイル、Web サーバーの予備の CPU サイクルを持つアプリケーションは、シリアル化されたセッション状態データのサイズが大幅に削減を実現できます。

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>許容される Url の範囲を拡大

ASP.NET 4 では、アプリケーションの Url のサイズを拡大するための新しいオプションについて説明します。 以前のバージョンの ASP.NET では、260 文字まで、NTFS ファイル パスの上限に基づく URL パスの長さを制限します。 ASP.NET 4 である (増減) この制限として、アプリケーションの適切なオプションを使用して 2 つの新しい*httpRuntime*構成属性。 次の例では、これらの新しい属性を示します。

[!code-xml[Main](overview/samples/sample10.xml)]

長いまたは短いパス (プロトコル、サーバー名、およびクエリ文字列が含まれていない URL の一部) を許可するのには、変更、 *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* 属性。 長いまたは短いクエリ文字列を許可するには、値を変更、 *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* 属性。

ASP.NET 4 では、URL の文字のチェックで使用される文字を構成することもできます。 ASP.NET では、URL のパス部分では無効な文字が検出されると、要求は拒否され、HTTP 400 エラーを発行します。 ASP.NET の以前のバージョンでは、URL の文字のチェックは、文字の固定セットに制限されていました。 ASP.NET 4 では、新しい有効な文字のセットをカスタマイズできます*requestPathInvalidChars*の属性、 *httpRuntime*構成要素を次の例に示すようにします。

[!code-xml[Main](overview/samples/sample11.xml)]

既定で、 <em>requestPathInvalidChars</em>属性が無効として 8 文字を定義します。 (に割り当てられている文字列で<em>requestPathInvalidChars</em>既定<em>、</em>より小さい (&lt;)、大 (&gt;)、アンパサンドと (&amp;) 文字は、エンコードされたため、`Web.config`ファイルは、XML ファイルです)。無効な文字のセットは、必要に応じてカスタマイズできます。

> [!NOTE]
> ASP.NET 4 は、無効な URL 文字は、IETF の RFC 2396 で定義されているため、常に 0x00 ~ 0x1F の ASCII の範囲内の文字が含まれている URL パスを拒否するメモ ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt))。 IIS 6 を実行するバージョンの Windows Server または以降では、http.sys プロトコルのデバイス ドライバーに自動的に拒否 Url でこれらの文字。


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>拡張可能な要求の検証

ASP.NET 要求の検証は、クロス サイト スクリプティング (XSS) 攻撃によく使用される文字列の入力方向の HTTP 要求データを検索します。 XSS の潜在的な文字列が見つかった場合、要求の検証は疑いのある文字列のフラグを設定し、エラーを返します。 組み込みの要求の検証は、XSS 攻撃で使用される最も一般的な文字列を見つけた場合にのみ、エラーを返します。 偽陽性が多すぎる、XSS 検証をより積極的な実行する前の試行が発生しました。 ただし、お客様は、XSS を意図的に緩和する逆にすることがありますかより積極的な要求の検証は、特定のページまたは特定の種類の要求を確認します。 する可能性があります。

ASP.NET 4 の要求の検証機能は拡張可能な要求のカスタム検証ロジックを使用できるようにします。 要求の検証を拡張する新しいから派生したクラスを作成する*System.Web.Util.RequestValidator*型とするアプリケーションの構成 (で、 *httpRuntime* の`Web.config`ファイル) にカスタムの型を使用します。 次の例では、要求のカスタム検証クラスを構成する方法を示します。

[!code-xml[Main](overview/samples/sample12.xml)]

新しい*requestValidationType*属性には、カスタム要求の検証を提供するクラスを指定する標準 .NET Framework 型識別子文字列が必要です。 要求ごとには、ASP.NET は、受信 HTTP 要求データの各部分を処理するカスタム型を呼び出します。 受信 URL、すべての HTTP ヘッダー (クッキーとカスタム ヘッダーの両方)、およびエンティティ本体は、次の例のような要求のカスタム検証クラスの使用可能なすべての。

[!code-csharp[Main](overview/samples/sample13.cs)]

HTTP の受信データの一部を検査しない場合、要求の検証のクラスに戻ることが ASP.NET 既定の要求の検証呼び出すだけで実行できるように*ベース。IsValidRequestString します。*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>オブジェクト キャッシュとキャッシュの機能拡張オブジェクト

最初のリリース以降、ASP.NET の強力なメモリ内オブジェクト キャッシュが含まれる (*System.Web.Caching.Cache*)。 キャッシュの実装は普及非 Web アプリケーションで使用されています。 ただしへの参照を含むように Windows フォームや WPF アプリケーションの面倒は`System.Web.dll`ASP.NET オブジェクト キャッシュを使用することができるようにするだけです。

キャッシュで使用できるようにすべてのアプリケーションは、.NET Framework 4 は、新しいアセンブリを新しい名前空間、一部の基本型および実装をキャッシュする具象型について説明します。 新しい`System.Runtime.Caching.dll`アセンブリには、新しいキャッシュ API が含まれている、 *System.Runtime.Caching*名前空間。 名前空間には、クラスの 2 つのコア セットが含まれています。

- 任意の種類のカスタム キャッシュ実装を構築するための基盤を提供する抽象型。
- 具体的なメモリ内オブジェクト キャッシュ実装 (、 *System.Runtime.Caching.MemoryCache*クラス)。

新しい*MemoryCache*クラスは、ASP.NET キャッシュで密接にモデル化し、ASP.NET と多くの内部キャッシュ エンジン ロジックを共有します。 パブリックのキャッシュ Api *System.Runtime.Caching* 、ASP.NET を使用している場合に、カスタムのキャッシュの開発をサポートするために更新されました*キャッシュ*オブジェクトでなじみのある概念を検索は、新しい Api です。

新しいの詳細については*MemoryCache*クラスと基本 Api をサポートしているドキュメント全体を必要となります。 ただし、次の例では、使用する、新しいキャッシュ API のしくみの概要を把握できます。 依存することがなく、Windows フォーム アプリケーションの上の例が書き込まれた`System.Web.dll`します。

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>拡張可能な HTML、URL、および HTTP ヘッダーのエンコード

ASP.NET 4 では、次の一般的なテキスト エンコーディング タスクのカスタムのエンコーディング ルーチンを作成できます。

- HTML エンコードします。
- URL エンコードします。
- HTML 属性エンコード。
- 送信 HTTP ヘッダーをエンコードします。

カスタム エンコーダーを作成するには、新しいから派生することによって*System.Web.Util.HttpEncoder*にカスタムの型を使用する型と ASP.NET を構成し、 *httpRuntime*のセクション、`Web.config`ファイルとして次の例に示します。

[!code-xml[Main](overview/samples/sample15.xml)]

ASP.NET が自動的にエンコードのメソッドを公開するたびに、カスタム エンコード実装を呼び出すカスタム エンコーダーを構成した後、 *System.Web.HttpUtility*または*System.Web.HttpServerUtility*クラスと呼ばれます。 これには、Web 開発チームの 1 つの部分を積極的な文字エンコーディング、Web の開発チームの残りの部分がパブリックの ASP.NET Api のエンコードを使用して引き続きを実装するカスタム エンコーダーを作成することができます。 カスタム エンコーダーを一元的に構成することによって、 *httpRuntime*要素、カスタム エンコーダーでエンコード Api パブリック ASP.NET からのすべてのテキスト エンコーディングの呼び出しがルーティングされる保証されます。

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>1 つのワーカー プロセス内の個々 のアプリケーションのパフォーマンスの監視

1 台のサーバーをホストできる Web サイトの数を増やすためには、多くのホスト側は、1 つのワーカー プロセスで複数の ASP.NET アプリケーションを実行します。 ただし、複数のアプリケーションは、1 つの共有ワーカー プロセスを使用すると、問題が発生している個々 のアプリケーションを識別するためにサーバー管理者のため困難ですが。

ASP.NET 4 では、CLR で導入された新しいリソースの監視の機能を活用します。 この機能を有効にするには、次の XML 構成スニペットを追加することができます、`aspnet.config`構成ファイル。

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> 注、`aspnet.config`ファイルは、.NET Framework がインストールされているディレクトリ。 `Web.config`ファイル。


ときに、 *appDomainResourceMonitoring*機能が有効になって、2 つの新しいパフォーマンス カウンターは、[ASP.NET アプリケーション] のパフォーマンス カテゴリで利用可能な: *% プロセッサ時間のマネージ*と*マネージ メモリの使用*します。 これらのパフォーマンス カウンターの両方は、推定の CPU 時間と個別の ASP.NET アプリケーションのマネージ メモリの使用率を追跡するために新しい CLR アプリケーション ドメインのリソースの管理機能を使用します。 その結果、ASP.NET 4 で管理者ようになりましたがある 1 つのワーカー プロセスで実行されている個々 のアプリケーションのリソースの消費をより詳細に表示します。

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>マルチ ターゲット

.NET Framework の特定のバージョンを対象とするアプリケーションを作成することができます。 ASP.NET 4 では、新しい属性で、*コンパイル*の要素、`Web.config`ファイルでは、.NET Framework 4 以降をターゲットすることができます。 明示的に、.NET Framework 4 を対象とする場合とで省略可能な要素を含める場合は、`Web.config`ファイルのエントリなど*system.codedom*、これらの要素は、.NET Framework 4 に対して適切である必要があります。 (.NET Framework 4 を明示的にターゲットはない場合、ターゲット フレームワークは内のエントリがないことから推論されます、`Web.config`ファイルです)。

次の例では、使用、 *targetFramework*属性、*コンパイル*の要素、`Web.config`ファイル。

[!code-xml[Main](overview/samples/sample17.xml)]

対象となる .NET Framework の特定のバージョンについては、次に注意してください。

- .NET Framework 4 アプリケーション プールでは、ASP.NET のビルド システムが想定、.NET Framework 4 をターゲットとして場合、`Web.config`ファイルは含まれません、 *targetFramework*属性場合は、`Web.config`ファイルが見つかりません。 (、.NET Framework 4 で実行されるようにアプリケーションをコーディングに変更する必要があります)。
- 必要な場合、 *targetFramework*属性、場合に、 *system.codeDom*で要素が定義されている、`Web.config`ファイルでは、このファイルは、.NET Framework 4 の適切なエントリを含める必要があります。
- 使用する場合、 *aspnet\_コンパイラ*(ビルド環境など)、アプリケーションをプリコンパイルするコマンドでの正しいバージョンを使用する必要があります、 *aspnet\_コンパイラ*ターゲット フレームワークのコマンド。 .NET Framework 3.5 およびそれ以前のバージョン用にコンパイルすると、.NET Framework 2.0 (%windir%\microsoft.net\framework\v2.0.50727) の同梱されているコンパイラを使用します。 .NET Framework 4 でに付属するコンパイラを使用すると、そのフレームワークを使用するか、以降のバージョンを使用して作成されたアプリケーションをコンパイルできます。
- 実行時に、コンパイラは、コンピューターにインストールされている最新のフレームワーク アセンブリを使用 (とそのため、GAC 内)。 フレームワークに更新プログラムを後で行われた場合 (仮想的なバージョン 4.1 がインストールされてなど)、場合でも、framework の新しいバージョンで機能を使用することができます、 *targetFramework*属性は、古いバージョンを対象と(4.0 など)。 (ただし、デザイン時に Visual Studio 2010 または使用すると、 *aspnet\_コンパイラ*コマンド、フレームワークの最新の機能を使用すると、コンパイラ エラーが発生)。

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>含まれる Web フォームと MVC の jQuery

Web フォームと MVC の Visual Studio テンプレートには、オープン ソースの jQuery ライブラリが含まれます。 新しい web サイトまたはプロジェクトを作成するときに、次の 3 つのファイルを含む Scripts フォルダーが作成されます。

- jQuery-1.4.1.js –、人間が判読できる、unminified、jQuery ライブラリのバージョン。
- jQuery-14.1.min.js – jQuery ライブラリの縮小されたバージョン。
- jQuery の 1.4.1-vsdoc.js – jQuery ライブラリの Intellisense ドキュメント ファイル。

アプリケーションの開発中には、jQuery の unminified バージョンを含めます。 実稼働アプリケーションの jQuery の縮小されたバージョンが含まれます。

たとえば、次の Web フォーム ページは、jQuery を使用して、黄色のフォーカスがあるときに、ASP.NET の TextBox コントロールの背景色を変更する方法を示しています。

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Content Delivery Network のサポート

Microsoft Ajax Content Delivery Network (CDN) では、Web アプリケーションに ASP.NET Ajax および jQuery スクリプトを簡単に追加することができます。 JQuery ライブラリを追加するだけで使用を開始するなど、`<script>`ページを Ajax.microsoft.com を指すようにタグ。

[!code-html[Main](overview/samples/sample19.html)]

Microsoft Ajax CDN を利用して、Ajax アプリケーションのパフォーマンスを大幅に向上させることができます。 Microsoft Ajax CDN の内容は、世界各地に配置されたサーバーにキャッシュされます。 さらに、Microsoft Ajax CDN は、別のドメインに配置されている Web サイトのキャッシュされた JavaScript ファイルを再利用するブラウザーを使用できます。

Microsoft Ajax Content Delivery Network は、Secure Sockets Layer を使用して web ページを使用する必要がある場合に、SSL (HTTPS) をサポートしています。

CDN が利用できない場合は、フォールバックを実装します。 フォールバックをテストします。

Microsoft Ajax CDN の概要の詳細については、次の web サイトを参照してください。

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ASP.NET の scriptmanager コントロールは、Microsoft Ajax CDN をサポートします。 1 つのプロパティの設定、EnableCdn プロパティだけでは、CDN からすべての ASP.NET フレームワークの JavaScript ファイルを取得できます。

[!code-aspx[Main](overview/samples/sample20.aspx)]

EnableCdn プロパティを true に設定した後、ASP.NET フレームワークがそのすべての ASP.NET フレームワークの JavaScript ファイルを検証し、UpdatePanel を使用するすべての JavaScript ファイルを含む CDN から取得されます。 この 1 つのプロパティを設定すると、web アプリケーションのパフォーマンスに大きな影響を及ぼす場合があります。

WebResource 属性を使用して、独自の JavaScript ファイルの CDN パスを設定できます。 新しい CdnPath プロパティは、EnableCdn プロパティ値の true に設定するときに使用する CDN へのパスを指定します。

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Scriptmanager コントロールの明示的なスクリプト

以前は、ASP.NET ScriptManger を使用している場合する必要がある全体のモノリシック ASP.NET Ajax ライブラリを読み込みます。 新しい ScriptManager.AjaxFrameworkMode プロパティを利用して、ASP.NET Ajax ライブラリのコンポーネントが読み込まれる正確に制御し、必要な ASP.NET Ajax ライブラリのコンポーネントのみをロードできます。

ScriptManager.AjaxFrameworkMode プロパティは、次の値に設定できます。

- -有効にするには、ScriptManager コントロールに自動的に結合したスクリプト ファイルの各コア フレームワーク スクリプト (従来の動作) である MicrosoftAjax.js スクリプト ファイルが含まれているを指定します。
- -無効にするには、Microsoft Ajax スクリプトのすべての機能が無効になっていると、ScriptManager コントロールは、スクリプトを自動的に参照していないことを指定します。
- --明示的なページに必要な個別のフレームワーク コア スクリプト ファイルへのスクリプト参照を明示的に含めるが、各スクリプト ファイルに必要な依存関係への参照を含めることを指定します。

たとえば、AjaxFrameworkMode プロパティ値を明示的に設定する場合は、必要な特定の ASP.NET Ajax コンポーネントのスクリプトを指定できます。

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web フォーム

Web フォームは、ASP.NET 1.0 のリリース以降では、ASP.NET core の機能をしました。 ASP.NET 4 で、次のようには、この領域で多くの機能強化がされています。

- 設定する機能*メタ*タグ。
- ビュー ステートを細かく制御します。
- ブラウザーの機能を使用する簡単な方法です。
- ASP.NET が Web フォームによるルーティングを使用するためのサポート。
- 生成された Id をより詳細に制御します。
- データ コントロールで選択された行を保持できるようにします。
- さらにレンダリングされる HTML の制御、 *FormView*と*ListView*コントロール。
- データ ソース コントロールのフィルター処理のサポート。

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Page.MetaKeywords Page.MetaDescription プロパティとメタ タグを設定

ASP.NET 4 で追加する 2 つのプロパティ、*ページ*クラス、 *MetaKeywords*と*MetaDescription*します。 これら 2 つのプロパティを表す対応する*メタ*ページで、次の例に示すようにタグを付けます。

[!code-aspx[Main](overview/samples/sample23.aspx)]

これら 2 つのプロパティは、同じ動作方法、ページの*タイトル*プロパティには。 これらには、これらの規則に従ってください。

1. ある場合ありません*メタ*でタグ付け、*ヘッド*プロパティ名と一致する要素 (名前は、= の"keywords" *Page.MetaKeywords*と名前の"description"を=*Page.MetaDescription*、つまり、これらのプロパティが設定されていない)、*メタ*タグは、表示される場合は、ページに追加されます。
2. 既にある場合*メタ*get および set メソッドの既存のタグの内容としてにこれらの名前を持つタグは、これらのプロパティの動作します。

これらのプロパティを設定するには、データベースまたはその他のソースからのコンテンツを入手することができ、対象を記述するには、動的にタグを設定することができます、実行時に特定のページです。

設定することも、*キーワード*と*説明*プロパティで、 *@ Page*次の例のように、Web フォーム ページのマークアップの上部にあるディレクティブ。

[!code-aspx[Main](overview/samples/sample24.aspx)]

これよりも優先されます、*メタ*タグ ページで既に宣言されている内容 (ある場合)。

説明の内容*メタ*タグは Google でのプレビューを一覧表示する検索を向上させるために使用します。 (詳細については、次を参照してください[メタ説明の改訂にスニペットを向上させる](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html)Google web マスターの中央のブログです。)。Google、Windows Live Search では、任意のキーワードの内容は使用しないで、他の検索エンジンの可能性があります。 詳細については、次を参照してください。[メタ キーワード アドバイス](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php)、検索エンジンのガイドの Web サイト。

これらの新しいプロパティは、単純な機能が、これらを手動で追加するための要件から、または作成するコードの記述から保存した、*メタ*タグ。

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>個々 のコントロール ビュー ステートを有効にします。

既定では、アプリケーションに必要でない場合でも、ページ上の各コントロールがそのビュー ステートを可能性のある格納する結果と、ページのビュー ステートが有効にします。 ページが生成し、ページをクライアントに送信し、ポストバックにかかる時間の増加は、マークアップでビュー ステート データが含まれます。 複数のビュー ステートを格納すると、パフォーマンスが著しく低下が生じる場合があります。 以前のバージョンの ASP.NET では、開発者は、ページ サイズを軽減するために、個々 のコントロールのビューステートを無効には、個々 のコントロールを明示的に行う必要があります。 Web サーバー コントロールには、ASP.NET 4 で、*に ViewStateMode*プロパティ ビュー ステートを既定で無効にして、ページが必要なコントロールに対してのみ有効にすることができます。

*に ViewStateMode*プロパティには次の 3 つの値を持つ列挙体:*有効*、*無効*、および*継承*します。 *有効になっている*有効に設定されているすべての子コントロールのそのコントロールの状態を表示する*継承*ことか、または何も設定します。 *無効になっている*無効にしますが、状態を表示し、*継承*コントロールが使用することを指定します、*に ViewStateMode*親コントロールから設定します。

次の例は、どのように*に ViewStateMode*プロパティは。 マークアップとコードを次のページのコントロールの値が含まれています、*に ViewStateMode*プロパティ。

[!code-aspx[Main](overview/samples/sample25.aspx)]

ご覧のように、コードには、PlaceHolder1 コントロールのビューステートが無効にします。 子 label1 コントロールは、このプロパティの値を継承 (*継承*の既定値は、*に ViewStateMode*コントロールの) ビューステートしたがってを保存しないとします。 PlaceHolder2 コントロールで*に ViewStateMode*に設定されている*有効*のため、label2 は、このプロパティを継承およびビューステートを保存します。 ページが最初に読み込まれたときに、*テキスト*両方のプロパティ*ラベル*コントロールは、文字列"[DynamicValue]"に設定されます。

これらの設定の効果は、ページが初めて読み込まれる、ブラウザーで次の出力で表示します。

無効になっています。 `: [DynamicValue]`

有効になります。`[DynamicValue]`

ポストバック後に、次の出力が表示されます。

無効になっています。 `: [DeclaredValue]`

有効になります。`[DynamicValue]`

Label1 コントロール (が*に ViewStateMode*値に設定されて*無効*) がコードで設定されていた値を保持していません。 ただし、label2 制御 (が*に ViewStateMode*値に設定されて*有効*) の状態を保持しています。

設定することも*に ViewStateMode*で、 *@ Page*ディレクティブを次の例のように。

[!code-aspx[Main](overview/samples/sample26.aspx)]

*ページ*クラスは単なる別のコントロールは、ページの他のすべてのコントロールの親コントロールとして機能します。 既定値*に ViewStateMode*は*有効*のインスタンスの*ページ*します。 コントロールが既定になるため*継承*、コントロールが継承、*有効*プロパティ値を設定しない限り*に ViewStateMode*ページまたはコントロールのレベル。

値、*に ViewStateMode*プロパティは、ビュー状態を維持する場合にのみかどうかを決定します。、 *EnableViewState*プロパティに設定されて*true*します。 場合、 *EnableViewState*プロパティに設定されて*false*、ビュー ステートは保持されない場合でも*に ViewStateMode*に設定されている*有効*します。

この機能は適切な使い方は*ContentPlaceHolder*設定することができます、マスター ページのコントロール*に ViewStateMode*に*無効になっている*マスターのページし、それを有効にします。個別に*ContentPlaceHolder*順番を必要とするコントロールを格納しているコントロールの状態を表示します。

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>ブラウザーの機能への変更

ASP.NET と呼ばれる機能を使用して、サイトを参照するユーザーを使用しているブラウザーの機能を決定する*ブラウザー機能*します。 ブラウザーの機能がによって表される、 *HttpBrowserCapabilities*オブジェクト (によって公開されている、 *Request.Browser*プロパティ)。 たとえば、使用することができます、 *HttpBrowserCapabilities*オブジェクトの種類と現在のブラウザーのバージョンが JavaScript の特定のバージョンをサポートするかどうかを確認します。 または、使用することができます、 *HttpBrowserCapabilities*要求がモバイル デバイスから送られたかどうかを判断するオブジェクト。

*HttpBrowserCapabilities*オブジェクトは、一連のブラウザー定義ファイルによって駆動されます。 これらのファイルには、特定のブラウザーの機能に関する情報が含まれます。 ASP.NET 4 で最近導入されたブラウザーと Google Chrome、研究などモーション BlackBerry スマート フォンと Apple iPhone でのデバイスに関する情報を含むように、これらのブラウザー定義ファイルが更新されました。

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

Asp.net version 3.5 Service Pack 1 では、次のように、ブラウザーを含む機能を定義できます。

- コンピューター レベルでは、作成または更新を`.browser`次のフォルダーに XML ファイル。

- [!code-console[Main](overview/samples/sample27.cmd)]

- ブラウザーの機能を定義した後、次のコマンドに、Visual Studio コマンド プロンプトからブラウザー機能のアセンブリを再構築し、GAC に追加するにを実行します。

- [!code-console[Main](overview/samples/sample28.cmd)]

- 個々 のアプリケーションの作成、`.browser`アプリケーションのファイル`App_Browsers`フォルダー。

これらの方法では、XML ファイルを変更する必要があり、コンピューター レベルの変更、aspnet を実行した後、アプリケーションを再起動する必要があります\_regbrowsers.exe プロセス。

ASP.NET 4 と呼ばれる機能が含まれています*ブラウザー機能プロバイダー*します。 名前からわかるように、ブラウザーの機能を決定するのに、独自のコードが使用できるプロバイダーを構築するこのできます。

実際には、開発者多くの場合を定義しないでカスタム ブラウザーの機能です。 ブラウザー ファイルの更新プログラム、プロセスは非常に複雑で、更新することが難しいとの XML 構文`.browser`ファイルは、定義を使用して複雑になることができます。 何がこのプロセスがより簡単とは、一般的なブラウザーの定義の構文がある場合または最新のブラウザーの定義、またはそのようなデータベース、Web サービスも含まれているデータベース。 新しいブラウザーの機能のプロバイダー機能により、これらのシナリオ実現してサード パーティ製の開発者向け。

新しい ASP.NET 4 のブラウザー機能のプロバイダー機能を使用するための 2 つの主な方法: ASP.NET ブラウザー機能の定義の機能を拡張するか、完全に置換することです。 次のセクションではの機能を置き換える方法と、それを拡張する方法最初について説明します。

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>ASP.NET ブラウザー機能の機能を置き換える

ASP.NET ブラウザー機能の定義機能を完全に置き換えるには、次の手順を実行します。

1. 派生するプロバイダー クラスを作成*HttpCapabilitiesProvider*をオーバーライドして、 *GetBrowserCapabilities*次の例のように、メソッド。 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    この例では、コードが新たに作成*HttpBrowserCapabilities*オブジェクト、MyCustomBrowser にその機能を設定してブラウザーという名前の機能のみを指定します。
2. アプリケーション プロバイダーに登録します。 

    アプリケーションとプロバイダーを使用するために追加する必要があります、*プロバイダー*属性を*browserCaps*セクション、`Web.config`または`Machine.config`ファイル。 (プロバイダーの属性を定義することもできます、*場所*要素の特定のモバイル デバイス用のフォルダーなどのアプリケーションで特定のディレクトリ)。次の例は、設定する方法を示します、*プロバイダー*構成ファイル内の属性。

    [!code-xml[Main](overview/samples/sample30.xml)]

    新しいブラウザー機能の定義を登録する別の方法は、次の例に示すようにコードを使用するは。

    [!code-csharp[Main](overview/samples/sample31.cs)]

    このコードを実行する必要があります、*アプリケーション\_開始*のイベント、`Global.asax`ファイル。 何らかの変更、 *BrowserCapabilitiesProvider*クラスは、キャッシュが、解決の有効な状態に残っているかどうかを確認するために、アプリケーション内のコードの実行前に行う必要があります*HttpCapabilitiesBase*オブジェクト。

#### <a name="caching-the-httpbrowsercapabilities-object"></a>HttpBrowserCapabilities オブジェクトのキャッシュ

前の例は、コードは、カスタム プロバイダーを取得するために呼び出すたびに実行ことである 1 つの問題、 *HttpBrowserCapabilities*オブジェクト。 これでは、複数回を各要求中に発生します。 例では、プロバイダーのコードは行いません多くします。 ただし、取得する場合は、カスタム プロバイダーのコードで、重要な作業を実行するため、 *HttpBrowserCapabilities*オブジェクト、パフォーマンスに影響することができます。 これが事態を防ぐためにキャッシュすることができます、 *HttpBrowserCapabilities*オブジェクト。 この場合は、以下の手順に従ってください。

1. 派生するクラスを作成*HttpCapabilitiesProvider*などの次の例 1。 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    例では、コードでは、カスタムの BuildCacheKey メソッドを呼び出して、キャッシュ キーを生成し、カスタム GetCacheTime メソッドを呼び出すことでキャッシュする時間の長さを取得します。 コードは、解決された追加*HttpBrowserCapabilities*キャッシュするオブジェクト。 オブジェクトをキャッシュから取得しで再利用する後続の要求が、カスタム プロバイダーの使用します。
2. 前の手順で説明されているアプリケーションでプロバイダーを登録します。

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>ASP.NET ブラウザー機能の機能を拡張します。

前のセクションには、新たに作成する方法が説明されている*HttpBrowserCapabilities* ASP.NET 4 でのオブジェクト。 ASP.NET で既にできるものの新しいブラウザーの機能の定義を追加することで、ASP.NET ブラウザー機能の機能を拡張することもできます。 XML のブラウザー定義を使用せず、これを行うことができます。 次の手順に示す方法。

1. 派生するクラスを作成*HttpCapabilitiesEvaluator*をオーバーライドして、 *GetBrowserCapabilities*メソッドを次の例に示すようにします。 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    まず、このコードは、ブラウザーを識別しようとするのに ASP.NET ブラウザー機能の機能を使用します。 ただし、ブラウザーが指定されていない場合、情報に基づいて、要求で定義されている (つまり場合、*ブラウザー*のプロパティ、 *HttpBrowserCapabilities*オブジェクトが文字列"Unknown")、コードでは、カスタム プロバイダー (MyBrowserCapabilitiesEvaluator)、ブラウザーを識別するためにします。
2. 前の例」の説明に従って、アプリケーションにプロバイダーを登録します。

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>既存の機能の定義に新しい機能を追加してブラウザーの機能の機能を拡張します。

に加えて、カスタムのブラウザー定義プロバイダーを作成して、新しいブラウザーの定義を動的に作成するのには、追加の機能を備えた既存のブラウザー定義を拡張できます。 これにより、目的の近くにあるが、いくつかの機能のみがない定義を使用できます。 そのためには、次の手順に従います。

1. 派生するクラスを作成*HttpCapabilitiesEvaluator*をオーバーライドして、 *GetBrowserCapabilities*メソッドを次の例に示すようにします。 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    コード例は、既存の ASP.NET を拡張*HttpCapabilitiesEvaluator*クラスを取得するため、 *HttpBrowserCapabilities*次のコードを使用して現在の要求の定義に一致するオブジェクト:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    コードでは追加、またはこのブラウザーの機能を変更できます。 新しいブラウザーの機能を指定する 2 つの方法はあります。

    - キー/値ペアを追加、 *IDictionary*オブジェクトによって公開される、*機能*のプロパティ、 *HttpCapabilitiesBase*オブジェクト。 前の例では、マルチタッチをという名前の値を持つ機能を追加するコードを*true*します。
    - 既存のプロパティの設定、 *HttpCapabilitiesBase*オブジェクト。 前の例では、コードを設定、*フレーム*プロパティを*true*します。 このプロパティはアクセサーを単に、 *IDictionary*オブジェクトによって公開される、*機能*プロパティ。 

        > [!NOTE]
        > このモデルは任意のプロパティに適用されます*HttpBrowserCapabilities*、コントロール アダプターを含むです。
2. 前の手順で説明されているアプリケーションで、プロバイダーを登録します。

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>ASP.NET 4 でのルーティング

ASP.NET 4 では、Web フォームによるルーティングを使用するための組み込みサポートを追加します。 要求の物理ファイルにマップされていない Url のルーティングは、そのまま使用するアプリケーションを構成することができます。 代わりに、ルーティングを使用する、ユーザーにわかりやすいと、アプリケーションの検索エンジン最適化 (SEO) に役立つことができます、Url を定義します。 たとえば、既存のアプリケーションの製品カテゴリを表示するページの URL は、次の例のようになります。

[!code-console[Main](overview/samples/sample36.cmd)]

ルーティングを使用するには、同じ情報を表示するために、次の URL を受け入れるようにアプリケーションを構成できます。

[!code-console[Main](overview/samples/sample37.cmd)]

ルーティングは、ASP.NET 3.5 SP1 以降で使用されました。 (ASP.NET 3.5 SP1 でのルーティングを使用する方法の例は、エントリを参照してください。 [WebForms でルーティングを使用して](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "このエントリのタイトル。") Phil Haack のブログです。)ただし、ASP.NET 4 が含まれています、ルーティングを使用するより簡単にするいくつかの機能には次を含みます。

- *ある PageRouteHandler*はルートを定義するときに使用する単純な HTTP ハンドラー クラス。 クラスは、要求にルーティングするページにデータを渡します。
- 新しいプロパティ*HttpRequest.RequestContext*と*Page.RouteData* (これは、プロキシを*HttpRequest.RequestContext.RouteData*オブジェクト)。 これらのプロパティを簡単に、ルートから渡される情報にアクセスします。
- 定義されている次新しい式ビルダー、 *System.Web.Compilation.RouteUrlExpressionBuilder*と*System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*、ASP.NET サーバー コントロール内のルート URL に対応する URL を作成する簡単な方法を提供します。
- *RouteValue*からの情報を抽出する簡単な方法を提供する、 *RouteContext*オブジェクト。
- *RouteParameter*クラスは、簡単に含まれるデータを渡す、 *RouteContext*オブジェクト データ ソース コントロールのクエリを (のような[ *FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Web フォーム ページのルーティング

次の例は、新しいを使用して Web フォームのルートを定義する方法を示します*MapPageRoute*のメソッド、*ルート*クラス。

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 では、 *MapPageRoute*メソッド。 次の例は前の例で示した SearchRoute 定義と同じですが、使用、*ある PageRouteHandler*クラス。

[!code-csharp[Main](overview/samples/sample39.cs)]

コード例では、物理ページにルートをマップする (最初のルート上でに`~/search.aspx`)。 最初のルート定義は、searchterm をという名前のパラメーターを URL から抽出し、ページに渡される必要があることにも指定します。

*MapPageRoute*メソッドは、次のメソッドのオーバー ロードをサポートしています。

- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults)*
- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary defaults, RouteValueDictionary constraints)*

*CheckPhysicalUrlAccess*パラメーターは、ルートにルーティングされている物理ページの セキュリティ アクセス許可を確認する必要があるかどうかを指定します (この場合は、完成しました) と、着信 URL のアクセス許可 (この場合、検索/{searchterm})。 場合の値*checkPhysicalUrlAccess*は*false*、着信 URL のアクセス許可のみがチェックされます。 これらのアクセス許可が定義されている、`Web.config`ファイルに次のように設定を使用します。

[!code-xml[Main](overview/samples/sample40.xml)]

物理ページに、構成の例ではアクセスが拒否されました`search.aspx`管理者の役割ではお客様を除くすべてのユーザー。 ときに、 *checkPhysicalUrlAccess*にパラメーターが設定されている*true* (既定値は)、物理ページ完成しましたので、URL/search/{searchterm} にアクセスする管理者ユーザーのみが許可されてそのロールのユーザーに制限されています。 場合*checkPhysicalUrlAccess*に設定されている*false*と前の例で示すように、サイトが構成されている、認証済みのすべてのユーザーは URL/search/{searchterm} へのアクセスを許可します。

#### <a name="reading-routing-information-in-a-web-forms-page"></a>ルーティング情報を Web フォーム ページの読み取り

Web フォームの物理的なページのコードでのルーティングが URL から抽出される情報にアクセスすることができます (または別のオブジェクトに追加したその他の情報、 *RouteData*オブジェクト) 2 つの新しいプロパティを使用して: *HttpRequest.RequestContext*と*Page.RouteData*します。 (*Page.RouteData*ラップ*HttpRequest.RequestContext.RouteData*)。次の例は、使用する方法を示します*Page.RouteData*します。

[!code-csharp[Main](overview/samples/sample41.cs)]

コードは、前の例のルートで定義されている searchterm パラメーターには、渡された値を抽出します。 次の要求 URL を考慮してください。

[!code-console[Main](overview/samples/sample42.cmd)]

この要求が行われる場合でレンダリングされます"scott"という単語、`search.aspx`ページ。

#### <a name="accessing-routing-information-in-markup"></a>マークアップでのルーティング情報にアクセスします。

前のセクションで説明されているメソッドは、Web フォーム ページのコードでルート データを取得する方法を示します。 同じ情報にアクセスするためのマークアップ内の式を使用することもできます。 式ビルダーは、宣言型コードを操作する強力で洗練された方法です。 (詳細については、エントリを参照してください[Express 自分でカスタム式ビルダー](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) Phil Haack のブログです。)。

ASP.NET 4 には、Web フォームのルーティング用の 2 つの新しい式ビルダーが含まれています。 次の例では、それらを使用する方法を示します。

[!code-aspx[Main](overview/samples/sample43.aspx)]

この例で、 *RouteUrl*ルート パラメーターに基づいている URL を定義する式を使用します。 これは、マークアップに完全な URL をハードコーディングする手間が省けます、このリンクの変更を必要とせず、URL の構造を後で変更することができます。

以前に定義されているルートに基づいて、このマークアップは、次の URL が生成されます。

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET は、適切なルートを自動的に動作 (正しい URL を生成、)、入力パラメーターに基づきます。 式で使用するルートを指定するため、ルート名を含めることもできます。

次の例は、使用する方法を示します、 *RouteValue*式。

[!code-aspx[Main](overview/samples/sample45.aspx)]

このコントロールを含むページを実行すると、ラベルに値"scott"が表示されます。

*RouteValue*式では、マークアップでは、ルート データを使用する単純なより複雑な Page.RouteData["x を使用することを避ける"] マークアップ構文です。

#### <a name="using-route-data-for-data-source-control-parameters"></a>ルート データ データ ソース コントロールのパラメーターを使用します。

*RouteParameter*クラスでは、ルート データをデータ ソース コントロール内のクエリのパラメーター値として指定することができます。 これは、 [works はほぼ同じように、](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)クラスに、次の例に示すように。

[!code-aspx[Main](overview/samples/sample46.aspx)]

ルート パラメーターの searchterm の値が使用しての例では、@companynameパラメーター、<em>選択</em>ステートメント。

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>クライアント Id の設定

新しい*ClientIDMode*プロパティが ASP.NET では、長期にわたる問題に対処 namely コントロールを作成する方法、 *id*をレンダリングする要素の属性。 把握、 *id*レンダリングされる要素の属性は、アプリケーションには、これらの要素を参照するクライアント スクリプトが含まれている場合に重要です。

*Id*に基づいて Web サーバー コントロールのレンダリングされる HTML 属性を生成、 *ClientID*コントロールのプロパティ。 ASP.NET 4 で生成するためのアルゴリズムまで、 *id*属性を*ClientID*プロパティは、ID のように、繰り返し使用されるコントロールの場合、(ある場合)、名前付けコンテナーを連結するようにしていますデータ コントロール)、プレフィックスとシーケンシャル番号を追加します。 コントロール Id は、予測可能なクライアント スクリプトで参照するが困難でしたので、アルゴリズムが常に、ページ内のコントロールの Id が一意であることを保証これが、中が発生しました。

新しい*ClientIDMode*プロパティでは、コントロールのクライアント ID が生成される方法により正確に指定することができます。 設定することができます、 *ClientIDMode*ページなど、あらゆるコントロールのプロパティ。 設定できる次のとおりです。

- *AutoID* – これは、生成するためのアルゴリズムを*ClientID*旧バージョンの ASP.NET で使用されていたプロパティ値。
- *静的*– ことを指定します、 *ClientID*値には、親の名前付けコンテナーの Id を連結することがなく ID と同じになります。 これは、Web ユーザー コントロールに役立ちます。 Web ユーザー コントロールなさまざまなページで、別のコンテナー コントロールで配置されているので、難しいことがありますを使用するコントロールのクライアント スクリプトを記述、 *AutoID*アルゴリズムする ID 値を予測することはできませんので、.
- *予測可能な*– このオプションは、主に繰り返しのテンプレートを使用するデータ コントロールで使用します。 生成されたが、コントロールの名前付けコンテナーの ID プロパティを連結*ClientID*値には"ctlxxx"などの文字列が含まれていません。 この設定は機能と組み合わせて、 *ClientIDRowSuffix*コントロールのプロパティ。 設定する、 *ClientIDRowSuffix*プロパティをデータ フィールドの名前とそのフィールドの値が生成されるサフィックスとして使用*ClientID*値。 通常としてデータ レコードの主キーを使用すると、 *ClientIDRowSuffix*値。
- *継承*– この設定は、コントロールの既定の動作は、コントロールの ID の生成がその親と同じであることを指定します。

設定することができます、 *ClientIDMode*ページ レベルのプロパティ。 これは、既定値を定義*ClientIDMode*現在のページのすべてのコントロールの値。

既定の*ClientIDMode*ページ レベルの値は*AutoID*と既定*ClientIDMode*コントロール レベルの値である*継承*. その結果、コードで、このプロパティを任意の場所設定はできませんと、すべてのコントロールは既定、 *AutoID*アルゴリズム。

ページ レベルの値で設定する、 *@ Page*ディレクティブは、次の例に示すようにします。

[!code-aspx[Main](overview/samples/sample47.aspx)]

設定することも、 *ClientIDMode* (コンピューター) をコンピューター レベルまたはアプリケーション レベルで、構成ファイル内の値。 これは、既定値を定義*ClientIDMode*アプリケーション内のすべてのページのすべてのコントロールを設定します。 コンピューター レベルで、値を設定する場合、既定値を定義します。 *ClientIDMode*そのコンピューターですべての Web サイトを設定します。 次の例は、 *ClientIDMode*構成ファイルで設定します。

[!code-xml[Main](overview/samples/sample48.xml)]

値を前に説明したように、 *ClientID*プロパティは、コントロールの親の名前付けコンテナーから派生します。 マスター ページを使用する場合など、いくつかのシナリオでコントロールが最終的に Id を持つもので、次のようにレンダリングされる HTML:

[!code-html[Main](overview/samples/sample49.html)]

場合でも、*入力*マークアップに示される要素 (から、 *テキスト ボックス*コントロール) 2 つの名前付けコンテナーは、ページの深さ (入れ子になった*ContentPlaceholder*コントロール)、マスター ページが処理されるため、最終結果は、次のようにコントロール ID です。

[!code-console[Main](overview/samples/sample50.cmd)]

この ID は ページで、一意であることが保証されますが、ほとんどの目的の時間が不必要にします。 レンダリングの ID の長さを短くと、ID を生成する方法をより細かく制御することを想像してください。 (たとえば、する"ctlxxx"プレフィックスを削除します。)これを実現する最も簡単な方法は、設定して、 *ClientIDMode*プロパティを次の例で示した。

[!code-aspx[Main](overview/samples/sample51.aspx)]

このサンプルで、 *ClientIDMode*プロパティに設定されて*静的*最も外側の*NamingPanel*要素、*予測可能*内部の*NamingControl*要素。 これらの設定と、次のマークアップ (ページとマスター ページの残りの部分は、前の例のように同じであると見なされます)。

[!code-html[Main](overview/samples/sample52.html)]

*静的*設定は、最も外側にある、そのすべてのコントロールの名前付けの階層をリセットする際の影響*NamingPanel*要素がなくなると、 *ContentPlaceHolder*と*マスター* Id 生成された id。 から (、*名前*レンダリングする要素の属性が影響を受ける、イベントの通常の ASP.NET 機能が保持されるため、状態を表示します)。マークアップを移動する場合でも、名前付けの階層をリセットする際の副作用は、 *NamingPanel*要素を異なる*ContentPlaceholder*コントロール、レンダリングされたクライアント Id は変わりません。

> [!NOTE]
> レンダリングされたコントロールの Id が一意であることを確認することに注意してください。 そうでない場合は、クライアントなど、個々 の HTML 要素の一意の Id を必要とする任意の機能を壊すことできます*document.getElementById*関数。


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>データ バインド コントロールで予測可能なクライアント Id を作成します。

*ClientID*の従来のアルゴリズムによってデータ連結リスト コントロール内のコントロールは、生成される値を実際に予測可能でないです。 *ClientIDMode*機能を使用して、詳細にどのようにこれらの Id を生成を制御できます。

次の例のマークアップが含まれています、 *ListView*コントロール。

[!code-aspx[Main](overview/samples/sample53.aspx)]

前の例では、 *ClientIDMode*と*RowClientIDRowSuffix*マークアップでプロパティを設定します。 *ClientIDRowSuffix*プロパティは、データ バインド コントロールでのみ使用できを使用するコントロールに応じてでその動作が異なります。 違いは、これらは。

- *GridView*コントロール-指定できます 1 つまたは複数の列の名前、データ ソースで、クライアント Id を作成する実行時に結合されます。 例では、設定した場合の*RowClientIDRowSuffix* "ProductName、ProductId"をレンダリングする要素は、次のような形式になっているために、コントロール Id:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView*コントロール-クライアント ID に追加されるデータ ソースの 1 つの列を指定することができます たとえば、設定した場合*ClientIDRowSuffix* "ProductName"に表示されるコントロール Id の次のような形式はなります。

- [!code-console[Main](overview/samples/sample55.cmd)]

- ここで、末尾の 1 は現在のデータ項目の製品 ID から派生します。

- *Repeater*コントロール-このコントロールはサポートしていません、 *ClientIDRowSuffix*プロパティ。 *Repeater*コントロール、現在の行のインデックスを使用します。 ClientIDMode を使用すると =「予測可能」、 *Repeater*を制御する次の形式になっているクライアント Id が生成されます。

- [!code-console[Main](overview/samples/sample56.cmd)]

- 末尾の 0 は、現在の行のインデックスです。

*FormView*と*DetailsView*コントロール表示しない複数の行をサポートしていないため、 *ClientIDRowSuffix*プロパティ。

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>データ コントロールで保持する行の選択

*GridView*と*ListView*コントロールを使用することができますが、行の選択ができます。 ASP.NET の以前のバージョンでの選択はページで行のインデックスに基づいています。 たとえば、1 ページ目では、3 番目の項目を選択して、2 ページ目に移動する場合、そのページでは、3 番目の項目が選択されます。

永続化された選択は、.NET Framework 3.5 SP1 での動的なデータ プロジェクトでのみ最初にサポートされていました。 この機能を有効にすると、現在の選択項目は、項目のデータ キーに基づきます。 これは、ページ 1 では、3 番目の行を選択してページ 2 に移動する場合は nothing が選択されているページ 2 ことを意味します。 1 ページに戻ると、3 番目の行が選択されたままです。 永続化された選択範囲がサポートされているようになりました、 *GridView*と*ListView*コントロールを使用してすべてのプロジェクトで、 *EnablePersistedSelection*プロパティのように、次の例:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>ASP.NET のグラフ コントロール

ASP.NET*グラフ*コントロールは、.NET Framework でのデータ視覚化製品を拡張します。 使用して、*グラフ*コントロール、複雑な統計や財務分析のための直感的で視覚的に説得力のあるグラフがある ASP.NET ページを作成することができます。 ASP.NET*グラフ*コントロール アドオンとして .NET Framework version 3.5 SP1 のリリースに導入され、は、.NET Framework 4 のリリースの一部です。

コントロールには、次の機能が含まれています。

- 35 種類のグラフ
- グラフ領域、タイトル、凡例、および注釈の無制限の数。
- さまざまなすべてのグラフ要素の外観設定します。
- ほとんどのグラフの種類の 3-D のサポート。
- スマート データ ラベルをデータ ポイントの周りに自動的に調整できます。
- 背景の縞模様、スケールの区切り、対数スケール化します。
- データの分析と変換に使用できる 50 を超える財務式および統計式。
- 単純なバインド、およびグラフ データの操作。
- 日付、時刻、通貨などの一般的なデータ形式をサポートします。
- 対話機能とクライアントを含む、イベント ドリブンのカスタマイズのサポートは、Ajax を使用してイベントをクリックします。
- 状態管理。
- バイナリ ストリーム転送。

次の図は、ASP.NET グラフ コントロールによって生成される財務グラフの例を紹介します。

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

図 2: ASP.NET のグラフ コントロールの例

ASP.NET グラフ コントロールを使用する方法の例についてをサンプル コードをダウンロード、 [Microsoft グラフ コントロールのサンプル環境](https://go.microsoft.com/fwlink/?LinkId=128300)MSDN Web サイトのページ。 コンテンツにコミュニティの他のサンプルを見つけることができます、 [Chart Control Forum](https://go.microsoft.com/fwlink/?LinkId=128713)します。

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>ASP.NET ページにグラフ コントロールを追加します。

次の例は、追加する方法を示します、*グラフ*マークアップを使用して、ASP.NET ページを制御します。 この例で、*グラフ*コントロールが静的なデータ ポイントの縦棒グラフを生成します。

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>3-D グラフの使用

*グラフ*コントロールが含まれています、 *ChartAreas*コレクションを含めることができる*ChartArea*グラフ エリアの特性を定義するオブジェクト。 たとえば、使用して 3-D グラフ エリアを使用する、 *Area3DStyle*例を次のプロパティ。

[!code-aspx[Main](overview/samples/sample59.aspx)]

次の図は、4 つの系列を 3-D グラフを示しています、*バー*グラフの種類。

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

図 3: 3-D 横棒グラフ

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>スケール区切り、対数スケールを使用します。

対数スケールのスケール区切りは、洗練されたグラフを追加する、2 つの追加方法です。 これらの機能は、グラフ領域内の各軸に固有です。 たとえば、グラフ領域の主軸の Y 軸にこれらの機能を使用する次のように使用します。、 *AxisY.IsLogarithmic*と*ScaleBreakStyle*でプロパティを*ChartArea*オブジェクト。 次のスニペットでは、主軸の Y 軸のスケール区切りを使用する方法を示します。

[!code-aspx[Main](overview/samples/sample60.aspx)]

次の図は、スケール区切りを有効になっていると、Y 軸を示します。

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

図 4: スケール区切り

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>による QueryExtender コントロールでデータをフィルター処理

データ駆動型 Web ページを作成する開発者向けの非常に一般的なタスクでは、データをフィルター処理します。 これは、従来によって実行されたビルド*場所*句では、データ ソース コントロール。 この方法は複雑になることができ、場合によっては、*場所*構文では、基になるデータベースのすべての機能を利用することはできません。

新しいフィルターを容易にする*による QueryExtender*コントロールが ASP.NET 4 で追加されました。 このコントロールに追加できます*EntityDataSource*または*LinqDataSource*これらのコントロールによって返されるデータをフィルター処理するためにコントロール。 *による QueryExtender*コントロールは、LINQ では、非常に効率的な操作の結果 ページにデータが送信される前に、データベース サーバーで、フィルターが適用されます。

*による QueryExtender*コントロールは、さまざまなフィルター オプションをサポートしています。 次のセクションでは、これらのオプションについて説明し、その使用方法の例を示します。

#### <a name="search"></a>検索

検索オプションで、*による QueryExtender*コントロールは、指定したフィールドに検索を実行します。 次の例では、コントロールは TextBoxSearch コントロールし、その内容の検索に入力したテキストを使用して、`ProductName`と`Supplier.CompanyName`から返されるデータの列、 *LinqDataSource*コントロール。

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>範囲

範囲のオプションは、検索オプションに似ていますが、範囲を定義する値のペアを指定します。 次の例では、*による QueryExtender*検索を制御、`UnitPrice`から返されるデータの列、 *LinqDataSource*コントロール。 範囲は、ページ上の TextBoxFrom と TextBoxTo コントロールから読み取られます。

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

プロパティ式のオプションを使用して、プロパティの値の比較を定義できます。 式が評価された場合*true*、検査されているデータが返されます。 次の例では、*による QueryExtender*コントロール内のデータを比較することでデータをフィルター処理、`Discontinued`ページ上の CheckBoxDiscontinued コントロールからの列には、値。

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

最後で使用するカスタム式を指定することができます、*による QueryExtender*コントロール。 このオプションにより、カスタム フィルターのロジックを定義するページの関数を呼び出すことができます。 次の例は、宣言でカスタム式を指定する方法を示します、*による QueryExtender*コントロール。

[!code-aspx[Main](overview/samples/sample64.aspx)]

次の例は、カスタム関数によって呼び出される、*による QueryExtender*コントロール。 含むデータベース クエリを使用する代わりに、ここで、*場所*句、コードを使用して LINQ クエリ、データをフィルター処理します。

[!code-csharp[Main](overview/samples/sample65.cs)]

これらの例で使用されている 1 つだけの式の表示、*による QueryExtender*一度にコントロール。 ただし、内部の複数の式を含めることができます、*による QueryExtender*コントロール。

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Html エンコードのコード式

使用してに大きく依存します (特に、ASP.NET MVC) の一部の ASP.NET サイト`<%` =  `expression %>`構文 (「コード ナゲット」と呼ばれる多くの場合)、応答にいくつかのテキストを書き込む。 コード式を使用する場合は、HTML エンコード (クロス サイト スクリプティング) XSS 攻撃できるページを開いたままに、ユーザーから、テキストの場合は、テキストを入力し忘れないように簡単です。

ASP.NET 4 には、式のコードは、次の新しい構文が導入されています。

[!code-aspx[Main](overview/samples/sample66.aspx)]

この構文では、応答に書き込むときに、既定では HTML エンコードを使用します。 この新しい式は、次を効果的に変換します。

[!code-aspx[Main](overview/samples/sample67.aspx)]

たとえば、 &lt;%: 要求 ["UserInput"] %&gt; HTML エンコードの値を実行します*要求 ["UserInput"]* します。

この機能の目的は、新しい構文を使用する各手順で決定する強制されないように、古い構文のすべてのインスタンスを置換することを可能にです。 ただしが出力されるテキストを HTML にするものではまたは既にエンコードされている場合にダブル エンコードする場合これ可能性があります。

ASP.NET 4 のような場合には、新しいインターフェイスが導入されています*IHtmlString*、具体的な実装と共に*HtmlString*します。 これらの型のインスタンスでは、そのため、値がされないこと HTML エンコードされたもう一度と、戻り値は既に正しくエンコードされて (またはそれ以外の場合)、HTML として表示するためを指定できます。 たとえば、次することはできません (とは) でエンコードされた HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET MVC 2 のヘルパー メソッドは、ASP.NET 4 を実行しているときに更新されるのでない二重エンコードされる場合は、この新しい構文を使用するが、唯一されています。 この新しい構文では、ASP.NET 3.5 SP1 を使用してアプリケーションを実行するときに機能しません。

XSS 攻撃からの保護は保証されないことに留意してください。 たとえば、引用符ではありません。 属性値を使用する HTML は、引き続き受けやすいユーザー入力を含めることができます。 ASP.NET コントロールと ASP.NET MVC ヘルパーの出力常に含まれている属性値、引用符では、推奨される方法に注意してください。

同様に、この構文では、ユーザー入力に基づいて JavaScript 文字列を作成する場合など、JavaScript のエンコーディングは行いません。

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>プロジェクト テンプレートの変更

以前のバージョンの ASP.NET では、Visual Studio を使用して新しい Web サイト プロジェクトまたは Web アプリケーション プロジェクトを作成する場合、結果として得られるプロジェクトが含まれますのみ、Default.aspx ページで、既定の`Web.config`ファイル、および`App_Data`では、次に示すように、フォルダー図:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio には、すべての次の図に示すようにファイルが含まれない空の Web サイト プロジェクト型もサポートしています。

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

初心者にとって非常に指針はほとんどありません、運用環境の Web アプリケーションを構築する方法になります。 そのため、ASP.NET 4 は、次の 3 つの新しいテンプレート、空の Web アプリケーション プロジェクトと Web アプリケーションと Web サイト プロジェクトに対して 1 つずつについて説明します。

#### <a name="empty-web-application-template"></a>空の Web アプリケーション テンプレート

名前からわかるように、空の Web アプリケーション テンプレートはたしか Web アプリケーション プロジェクトです。 このプロジェクト テンプレートは、次の図に示すように、Visual Studio の新しいプロジェクト ダイアログ ボックスから選択します。

[![](overview/_static/image7.png)](overview/_static/image6.png)

([フルサイズの画像を表示する をクリックします](overview/_static/image8.png))。

空の ASP.NET Web アプリケーションを作成するときに、Visual Studio には、次のフォルダー レイアウトが作成されます。

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

これは、例外が 1 つの ASP.NET の以前のバージョンから空の Web サイトのレイアウトに似ています。 Visual Studio 2010 では、空の Web アプリケーションと空の Web サイト プロジェクトが含まれている、次の最小限`Web.config`Visual Studio でプロジェクトのターゲット フレームワークを識別するために使用される情報を含むファイル。

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

これを行わない*targetFramework*プロパティ、Visual Studio の既定値は、古いアプリケーションを開くときに、互換性を維持するために、.NET Framework 2.0 を対象とします。

#### <a name="web-application-and-web-site-project-templates"></a>Web アプリケーションや Web サイト プロジェクト テンプレート

その他の 2 つ新しいプロジェクト テンプレートが Visual Studio 2010 に付属するには、大幅な変更が含まれます。 次の図は、新しい Web アプリケーション プロジェクトを作成するときに作成されるプロジェクト レイアウトを示します。 (Web サイト プロジェクトのレイアウトは、事実上同じです)。

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

プロジェクトには、多数以前のバージョンで作成されていないファイルにはが含まれています。 さらに、新しい Web アプリケーション プロジェクトは、すぐに新しいアプリケーションへのアクセスをセキュリティで保護することで開始することができますが、基本メンバーシップ機能で構成されます。 、これらを含めることにより、`Web.config`ファイルの新しいプロジェクトにはメンバーシップ、ロール、およびプロファイルを構成するために使用するエントリが含まれています。 次の例は、`Web.config`新しい Web アプリケーション プロジェクトのファイル。 (この場合、 *roleManager*は無効です)。

[![](overview/_static/image13.png)](overview/_static/image12.png)

([フルサイズの画像を表示する をクリックします](overview/_static/image14.png))。

プロジェクトは、2 つ目も含まれています。`Web.config`ファイル、`Account`ディレクトリ。 2 番目の構成ファイルは、ログに記録されないユーザーで ChangePassword.aspx のページへのアクセスをセキュリティで保護する方法を提供します。 次の例は、2 つ目の内容を示しています。`Web.config`ファイル。

![](overview/_static/image15.png)

既定では、新しいプロジェクト テンプレートで作成されたページには、以前のバージョンよりもより多くのコンテンツも含まれます。 プロジェクトには、既定のマスター ページと CSS ファイルが含まれていて、既定のページ (Default.aspx) が既定でマスター ページを使用するよう構成します。 初めての Web アプリケーションまたは Web サイトを実行すると、既定のページ (ホーム) を機能が既にになります。 実際には、新しい MVC アプリケーションを起動するときに表示する既定のページに似ています。

[![](overview/_static/image17.png)](overview/_static/image16.png)

([フルサイズの画像を表示する をクリックします](overview/_static/image18.png))。

これらの変更をプロジェクト テンプレートの目的は、新しい Web アプリケーションの構築を開始する方法のガイダンスを提供します。 意味的に正しく、厳密な XHTML 1.0 に準拠したマークアップと CSS を使用して指定されているレイアウト、テンプレート内のページは ASP.NET 4 Web アプリケーションを構築するためのベスト プラクティスを表します。 既定のページには、簡単にカスタマイズ可能な 2 つの列のレイアウトもがあります。

たとえば、場合を考えます新しい Web アプリケーションの一部の色を変更し、My ASP.NET Application ロゴの代わりに、会社のロゴを挿入します。 新しいディレクトリを作成するには、`Content`ロゴ イメージを格納します。

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

ページには、イメージを追加するを開く、`Site.Master`ファイル、My ASP.NET Application テキストが定義され、置き換え、それを検索、*イメージ*要素が*src*属性がこの新しいロゴに設定次の例に示すの画像:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([フルサイズの画像を表示する をクリックします](overview/_static/image22.png))。

Site.css ファイルに移動し、ページの背景色とのヘッダーを変更する CSS クラスの定義を変更します。

これらの変更の結果を非常に簡単にカスタマイズされたホーム ページを表示することができます。

[![](overview/_static/image24.png)](overview/_static/image23.png)

([フルサイズの画像を表示する をクリックします](overview/_static/image25.png))。

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>CSS の改良点

ASP.NET 4 での作業の主要な分野の 1 つは、最新の HTML 標準に準拠している HTML のレンダリングを支援するようにしています。 これには、ASP.NET Web サーバー コントロールで CSS スタイルを使用する方法の変更が含まれます。

#### <a name="compatibility-setting-for-rendering"></a>レンダリングの互換性設定

Web アプリケーションまたは Web サイトは、.NET Framework 4 を対象とした場合、既定、 *controlRenderingCompatibilityVersion*の属性、*ページ*要素が「4.0」に設定します。 この要素がコンピューター レベルで定義されている`Web.config`ファイルし、既定ですべての ASP.NET 4 アプリケーションに適用されます。

[!code-xml[Main](overview/samples/sample69.xml)]

値は、 *controlRenderingCompatibility*は潜在的な新しいバージョンの定義は今後のリリースにより、文字列です。 現在のリリースでは、次の値がこのプロパティのサポートされています。

- "3.5". この設定は、従来のレンダリングとマークアップを示します。 コントロールによってレンダリングされるマークアップが 100% の旧バージョンと互換性のあるの設定、 *xhtmlConformance*プロパティが受け入れられます。
- "4.0". プロパティにこの設定がある場合は、ASP.NET Web サーバー コントロールは、次を実行します。
- *XhtmlConformance*プロパティが常に"Strict"として扱われます。 その結果、コントロールは、XHTML 1.0 Strict マークアップをレンダリングします。
- 不要になった非入力コントロールを無効にするには、無効なスタイルが表示されます。
- *div* CSS 規則のユーザーが作成したこれらの影響を与えないように、非表示フィールドを囲む要素はスタイリングようになりました。
- メニュー コントロールは、意味的に正しいとアクセシビリティのガイドラインに準拠しているマークアップをレンダリングします。
- 検証コントロールでは、インライン スタイルはレンダリングされません。
- 以前の境界線を描画するコントロール =「0」(ASP.NET から派生したコントロール*テーブル*制御、および ASP.NET*イメージ*コントロール) この属性は表示されなくします。

#### <a name="disabling-controls"></a>コントロールを無効にします。

ASP.NET 3.5 SP1 およびそれ以前のバージョンでは、フレームワークがレンダリング、*無効になっている*そのコントロールの HTML マークアップ属性*有効*プロパティに設定*false*。 ただし、HTML 4.01 仕様のみに従って*入力*要素は、この属性を持つ必要があります。

ASP.NET 4 で設定することができます、 *controlRenderingCompatabilityVersion*プロパティを次の例のように「3.5」。

[!code-xml[Main](overview/samples/sample70.xml)]

用のマークアップを作成する場合があります、*ラベル*コントロールを無効にすると、次のようなコントロール。

[!code-aspx[Main](overview/samples/sample71.aspx)]

*ラベル*コントロールは、次の HTML をレンダリングします。

[!code-html[Main](overview/samples/sample72.html)]

ASP.NET 4 で設定することができます、 *controlRenderingCompatabilityVersion* 「4.0」にします。 その場合は、そのレンダリングを制御するだけ*入力*要素が表示されます、*無効になっている*属性の場合に、コントロールの*有効*プロパティに設定されて*false*. HTML にレンダリングされないコントロール*入力*要素をレンダリングする*クラス*無効になっているコントロールの外観を定義に使用できる CSS クラスを参照する属性。 たとえば、*ラベル*前の例に示すようにコントロールが次のマークアップを生成します。

[!code-html[Main](overview/samples/sample73.html)]

このコントロールに指定するクラスの既定値は、"aspNetDisabled"です。 ただし、この既定値を変更するには、静的な*DisabledCssClass*の静的プロパティ、 *WebControl*クラス。 コントロール開発者は、特定のコントロールを使用する動作も定義できますを使用して、 *SupportsDisabledAttribute*プロパティ。

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Div を非表示フィールドの周囲の要素を非表示

ASP.NET 2.0 およびそれ以降のバージョンがシステムに固有の非表示フィールドを表示 (など、*隠し*ビュー状態情報を格納するための要素) 内で*div* XHTML 標準に準拠するために要素。 ただし、この問題が発生 CSS 規則の影響を与える場合*div*ページ上の要素。 たとえば、ページに表示されている 1 ピクセルの行でそのよう非表示に約*div*要素。 ASP.NET 4 で*div* ASP.NET によって生成された非表示フィールドを囲む要素は、次の例のように CSS クラスの参照を追加します。

[!code-html[Main](overview/samples/sample74.html)]

のみに適用される CSS クラスを定義することができますし、*隠し*次の例のように、ASP.NET によって生成される要素。

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>テンプレート化されたコントロールの外側のテーブルを表示

既定では、テンプレートをサポートする次の ASP.NET Web サーバー コントロールが、インライン スタイルを適用するために使用する外部テーブルで自動的にラップします。

- *FormView*
- *Login*
- *PasswordRecovery*
- *ChangePassword*
- *ウィザード*
- *CreateUserWizard*

という名前の新しいプロパティ*RenderOuterTable*マークアップから削除する外部テーブルは、これらのコントロールに追加されました。 たとえば、次の例を*FormView*コントロール。

[!code-aspx[Main](overview/samples/sample76.aspx)]

このマークアップは、HTML テーブルを含むページには、次の出力を表示します。

[!code-html[Main](overview/samples/sample77.html)]

テーブルが表示されていることを防ぐために設定することができます、 *FormView*コントロールの*RenderOuterTable*例を次のプロパティ。

[!code-aspx[Main](overview/samples/sample78.aspx)]

前の例では、次の出力、なしのレンダリング、*テーブル*、 *tr*、および*td*要素。

> Content


この機能強化できますやすくスタイル、CSS を使用してコントロールのコンテンツのため、コントロールによって予期しないタグは表示されません。

> [!NOTE]
> 注この変更が不要になったため、デザイナーの Visual Studio 2010 でのオート フォーマット関数のサポートを無効にする*テーブル*オート フォーマット オプションによって生成されるスタイル属性をホストできる要素。


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>ListView コントロールの機能強化

*ListView*コントロールを ASP.NET 4 で使いやすくなりました。 コントロールの以前のバージョンの既知の ID を持つサーバー コントロールに含まれているレイアウト テンプレートを指定することが必要です。 次のマークアップを使用する方法の典型的な例を示しています、 *ListView* ASP.NET 3.5 のコントロール。

[!code-aspx[Main](overview/samples/sample79.aspx)]

ASP.NET 4 で、 *ListView*コントロールでは、レイアウト テンプレートは必要はありません。 前の例に示すようにマークアップは、次のマークアップで置換できます。

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList、RadioButtonList コントロールの強化

ASP.NET 3.5 では、レイアウトを指定することができます、 *CheckBoxList*と*RadioButtonList*次の 2 つの設定を使用します。

- *フロー*します。 コントロールのレンダリング*span*そのコンテンツを格納する要素。
- *テーブル*します。 コントロールのレンダリングを*テーブル*そのコンテンツを格納する要素。

次の例は、これらのコントロールのマークアップを示しています。

[!code-aspx[Main](overview/samples/sample81.aspx)]

既定では、コントロールをレンダリング HTML、次のようにします。

[!code-html[Main](overview/samples/sample82.html)]

これらのコントロールには、意味的に正しくの HTML を表示するために、項目の一覧が含まれているために、HTML リストを使用してその内容をレンダリングする必要があります (*li*) 要素。 これにより、簡単に支援のテクノロジを使用して Web ページが読み取られ、コントロールを簡単にスタイルが CSS を使用しているユーザー。

ASP.NET 4 で、 *CheckBoxList*と*RadioButtonList*コントロールに次の新しい値をサポートする、*この*プロパティ。

- *OrderedList* – としてコンテンツがレンダリングされる*li*内の要素、 *ol*要素。
- *UnorderedList* – としてコンテンツがレンダリングされる*li*内の要素を*ul*要素。

次の例では、これらの新しい値を使用する方法を示します。

[!code-aspx[Main](overview/samples/sample83.aspx)]

上記のマークアップでは、次の HTML が生成されます。

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> 設定するかどうかに注意してください*この*に*OrderedList*または*UnorderedList*、 *RepeatDirection*プロパティが使用できなくできますとは。マークアップまたはコード内でプロパティが設定されている場合は、実行時に例外をスローします。 プロパティがない値 CSS を代わりを使用してこれらのコントロールのレイアウトが定義されているためです。


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>メニュー コントロールの機能強化

ASP.NET 4 では、前に、*メニュー*コントロールは、一連の HTML テーブルを表示します。 これにより、インライン プロパティの設定の外部での CSS スタイルを適用する複数が困難し、ユーザー補助の標準に準拠してでしたも。

ASP.NET 4 で、コントロールはこれで順不同のリストとリストの要素で構成されるセマンティック マークアップを使用して HTML をレンダリングします。 次の例では、ASP.NET ページ用のマークアップを示しています、*メニュー*コントロール。

[!code-aspx[Main](overview/samples/sample85.aspx)]

コントロールが、次の HTML を生成するページが表示される、(、 *onclick*コードがわかりやすくするために省略されています)。

[!code-html[Main](overview/samples/sample86.html)]

レンダリング機能強化に加え、フォーカス管理を使用して、メニューのキーボード ナビゲーションを向上しています。 ときに、*メニュー*コントロールがフォーカスを取得、方向キーを使用して要素を移動することができます。 *メニュー*コントロールはもアクセス可能なリッチ インターネット applications (ARIA) の役割をアタッチし、follo 属性[翼、](http://www.w3.org/TR/wai-aria-practices/#menu "メニュー ARIA ガイドライン")を向上させるためアクセシビリティ。

上部にあるスタイル ブロックに沿ってレンダリングされる HTML 要素ではなく、ページのでは、メニュー コントロールのスタイルが表示されます。 新しい設定することができる場合、コントロールのスタイル設定を完全に制御を実行するには、 *IncludeStyleBlock*プロパティを*false*、スタイル ブロックが出力されない場合。 このプロパティを使用する方法の 1 つ、メニューの外観を設定する、Visual Studio デザイナーで、オート フォーマットの機能を使用することです。 ページの実行、ページのソースを開く、および外部 CSS ファイルに表示されるスタイルのブロックをコピーし、ことができます。 Visual Studio で、スタイルと設定を元に戻す*IncludeStyleBlock*に*false*します。 外部スタイル シートのスタイルを使用して、メニューの外観を定義することになります。

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>ウィザードおよび CreateUserWizard コントロール

ASP.NET*ウィザード*と*CreateUserWizard*コントロールがレンダリングする HTML を定義するためのテンプレートをサポートします。 (*CreateUserWizard*から派生した*ウィザード*)。次の例では、完全なテンプレートのマークアップ*CreateUserWizard*コントロール。

[!code-aspx[Main](overview/samples/sample87.aspx)]

コントロールは、次のような HTML を表示します。

[!code-html[Main](overview/samples/sample88.html)]

ASP.NET 3.5 sp1 では、テンプレートの内容を変更できますが、まだ制限されている場合の出力を制御、*ウィザード*コントロール。 ASP.NET 4 で作成することができます、 *LayoutTemplate*テンプレートおよび insert*プレース ホルダー* (予約済みの名前を使用) を制御する方法を指定する、*ウィザード コントロール*をレンダリングします。 次の例では、これは示しています。

[!code-aspx[Main](overview/samples/sample89.aspx)]

例では、名前付きプレース ホルダーで、次が含まれています、 *LayoutTemplate*要素。

- *headerPlaceholder* – の内容で置き換えられます、実行時に、*使って HeaderTemplate*要素。
- *sideBarPlaceholder* – の内容で置き換えられます、実行時に、 *SideBarTemplate*要素。
- *wizardStepPlaceHolder* – の内容で置き換えられます、実行時に、 *WizardStepTemplate*要素。
- *navigationPlaceholder* – 実行時に定義されているナビゲーション テンプレートで置き換えられます。

プレース ホルダーを使用する例のマークアップでは、次の HTML をレンダリングします (実際には、テンプレートで定義されているコンテンツ) なし。

[!code-html[Main](overview/samples/sample90.html)]

ここで定義されていないユーザーの唯一の HTML は、 *span*要素。 (後のリリースでは、予想も、 *span*要素は表示されません)。これでは、によって生成されるほぼすべてのコンテンツを完全に制御を提供する、*ウィザード*コントロール。

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC は、2009 年 3 月で、アドオン、フレームワークとして、ASP.NET 3.5 SP1 を導入されました。 Visual Studio 2010 には、ASP.NET MVC 2 にには新しい機能や機能が含まれています。

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>領域のサポート

領域使用するグループのコント ローラーとビューを他のセクションから相対的に分離で大規模なアプリケーションのセクションにできます。 各領域は、メイン アプリケーションによって参照し、可能な個別の ASP.NET MVC プロジェクトとして実装できます。 これにより、大規模なアプリケーションをビルドするときの複雑さし、1 つのアプリケーションで連携して動作する複数のチームで簡単になります。

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>データ注釈属性の検証のサポート

*DataAnnotations*属性を使用して、メタデータ属性を使用して、モデルに検証ロジックを適用できます。 *DataAnnotations*属性は、ASP.NET 3.5 SP1 での ASP.NET Dynamic Data で導入されました。 これらの属性は、既定のモデル バインダーに統合されており、ユーザー入力を検証するメタデータ ドリブン手段を提供します。

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>テンプレート化されたヘルパー

テンプレート化されたヘルパーは、関連付けが自動的に編集することができ、データ型を持つテンプレートを表示します。 たとえばの日付の選択の UI 要素が自動的に表示されることを指定するテンプレート ヘルパーを使用することができます、 *System.DateTime*値。 ASP.NET Dynamic Data でのフィールド テンプレートに似ています。

*Html.EditorFor*と*Html.DisplayFor*ヘルパー メソッドが複数のプロパティでも複雑なオブジェクト型の標準的なデータを表示、組み込みサポートを備えています。 などのデータ注釈属性を適用することができますもレンダリングをカスタマイズする*DisplayName*と*ScaffoldColumn*を*ViewModel*オブジェクト。

多くの場合、さらに UI ヘルパーからの出力をカスタマイズして、生成されたものを完全に制御します。 *Html.EditorFor*と*Html.DisplayFor*ヘルパー メソッドをサポートしてこれをオーバーライドできる外部テンプレートとコントロール レンダリングされる出力を定義できるテンプレート メカニズムを使用します。 クラスのテンプレートを個別に表示できます。

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>動的データ

動的なデータが導入された 2008 年の半ばに .NET Framework 3.5 SP1 のリリースでします。 この機能は、次を含む、データ駆動型アプリケーションを作成するための多くの拡張機能を提供します。

- データ ドリブンの Web サイトをすばやく構築するための RAD エクスペリエンス。
- データ モデルで定義された制約に基づいている自動検証します。
- 内のフィールドに対して生成されるマークアップを簡単に変更する機能、 *GridView*と*DetailsView*動的データ プロジェクトの一部であるフィールド テンプレートを使用してコントロール。

> [!NOTE]
> 注詳細についてを参照してください、[動的データ ドキュメント](https://msdn.microsoft.com/library/cc488545.aspx)MSDN ライブラリ。


ASP.NET 4 では、データ駆動型 Web サイトをすばやく構築するためのさらに多くの電力を開発者に提供する動的なデータが強化されました。

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>既存のプロジェクトの動的なデータの有効化

.NET Framework 3.5 SP1 に同梱されている動的データ機能では、次などの新機能になります。

- フィールド テンプレート – これらは、データ種類ベースのテンプレートのデータ バインド コントロール。 フィールド テンプレートには、各フィールドのテンプレートのフィールドを使用するよりもデータ コントロールの外観をカスタマイズする簡単なことができます。
- 検証: 動的なデータでのデータ クラスに属性を使用して、必要なフィールド、範囲のチェック、型チェック、照合、正規表現パターンのような一般的なシナリオの検証とカスタム検証を指定するは。 データ コントロールで検証が適用されます。

ただし、これらの機能には、次の要件がありました。

- データ アクセス層は、Entity Framework や LINQ to SQL に基づく必要があります。
- 唯一のデータ ソースのこれらの機能がサポートされているコントロール、 *EntityDataSource*または*LinqDataSource*コントロール。
- 機能が、機能をサポートするために必要なすべてのファイルを確保するために、動的なデータまたは動的データ エンティティのテンプレートを使用して作成した Web プロジェクトが必要です。

ASP.NET 4 での動的なデータのサポートの主な目的では、ASP.NET アプリケーションの動的なデータの新しい機能が有効にします。 次の例では、マークアップを利用して動的データ機能の既存のページでコントロールを示しています。

[!code-aspx[Main](overview/samples/sample91.aspx)]

ページのコードでこれらのコントロールの動的なデータ サポートを有効にするには次のコードを追加する必要があります。

[!code-csharp[Main](overview/samples/sample92.cs)]

ときに、 *GridView*コントロールが編集モードでは、入力されたデータが適切な形式である動的なデータが自動的に検証します。 そうでない場合、エラー メッセージが表示されます。

この機能は、その他の特典も用意されています。 の値を挿入モードの既定値を指定できるなどです。 動的なデータがない場合、フィールドの既定値を実装するにはイベントへのアタッチ、コントロールの検索 (を使用して*FindControl*)、その値を設定します。 ASP.NET 4 で、 *EnableDynamicData*呼び出しは、この例で示すように、オブジェクトのいずれかのフィールドの既定値を渡すことができますが、2 番目のパラメーターをサポートしています。

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>DynamicDataManager コントロールの宣言構文

*DynamicDataManager*コントロールは、宣言によって構成できるようにするために強化されていますのコードだけではなく、ASP.NET では、ほとんどのコントロールと同様です。 マークアップを*DynamicDataManager*コントロールは次のようになります。

[!code-aspx[Main](overview/samples/sample94.aspx)]

このマークアップにより、動的データで参照されている GridView1 コントロールの動作、 *DataControls*のセクション、 *DynamicDataManager*コントロール。

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>エンティティ テンプレート

エンティティ テンプレートは、新しいカスタム ページを作成することがなくデータのレイアウトをカスタマイズする方法を提供します。 テンプレートの使用のページ、 *FormView*コントロール (の代わりに、 *DetailsView*コントロールが動的なデータの以前のバージョンのページ テンプレートで使用) および*DynamicEntity*エンティティ テンプレートをレンダリングするコントロール。 これにより、動的なデータによって表示されるマークアップの詳細に制御できるようにします。

次に、エンティティのテンプレートを含む新しいプロジェクト ディレクトリのレイアウトを示します。

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate`ディレクトリには、データ モデル オブジェクトを表示する方法のテンプレートが含まれています。 使用して既定では、オブジェクトの表示、`Default.ascx`テンプレートによって作成されたマークアップのような外観のマークアップを提供する、 *DetailsView* ASP.NET 3.5 SP1 での Dynamic Data で使用されるコントロール。 次の例のマークアップを示しています、`Default.ascx`コントロール。

[!code-aspx[Main](overview/samples/sample96.aspx)]

サイト全体の外観を変更するのには、既定のテンプレートを編集できます。 表示、編集、および挿入操作のテンプレートがあります。 新しいテンプレート オブジェクトの種類を 1 つだけの外観を変更するにがデータ オブジェクトの名前に基づいて追加されたことができます。 たとえば、次のテンプレートを追加できます。

[!code-console[Main](overview/samples/sample97.cmd)]

テンプレートには、次のマークアップが含まれます。

[!code-aspx[Main](overview/samples/sample98.aspx)]

新しいエンティティ テンプレートが新しいを使用して、ページに表示される*DynamicEntity*コントロール。 実行時に、このコントロールは、エンティティ テンプレートの内容に置き換えられます。 次のマークアップに示す、 *FormView*を制御、`Detail.aspx`エンティティ テンプレートを使用するページのテンプレート。 通知、 *DynamicEntity*マークアップ内の要素。

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Url および電子メール アドレスの新しいフィールド テンプレート

ASP.NET 4 では、2 つの新しい組み込みフィールド テンプレート`EmailAddress.ascx`と`Url.ascx`します。 これらのテンプレートとしてマークされているフィールドの使用は*EmailAddress*または*Url*で、 *DataType*属性。 *EmailAddress*オブジェクトを使用して作成されるハイパーリンクとして、フィールドが表示されます、 *mailto:* プロトコル。 ユーザーがリンクをクリック、ユーザーの電子メール クライアントを開くし、スケルトン メッセージを作成します。 オブジェクトとして型指定された*Url*は通常のハイパーリンクとして表示されます。

次の例では、フィールドがマークする方法を示します。

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>DynamicHyperLink コントロールへのリンクを作成します。

動的データでは、エンドユーザーが Web サイトにアクセスするときに表示される Url を制御するには、.NET Framework 3.5 SP1 で追加された新しいルーティング機能を使用します。 新しい*DynamicHyperLink*コントロールにより、簡単に動的データ サイト内のページへのリンクを構築できます。 次の例は、使用する方法を示します、 *DynamicHyperLink*コントロール。

[!code-aspx[Main](overview/samples/sample101.aspx)]

このマークアップのリスト ページを指すリンクが作成、`Products`で定義されているルートに基づいて、テーブル、`Global.asax`ファイル。 コントロールは、自動的に既定のテーブル名に基づく動的なデータ ページを使用します。

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>データ モデルにおける継承のサポート

Entity Framework と LINQ to SQL は、そのデータ モデルで継承をサポートします。 この例を持つデータベースである可能性があります、`InsurancePolicy`テーブル。 含まれる場合も`CarPolicy`と`HousePolicy`と同じフィールドを持つテーブルを`InsurancePolicy`しより多くのフィールドを追加します。 データ モデルで継承されたオブジェクトを理解し、継承されたテーブルのスキャフォールディングをサポートするために、動的なデータが変更されました。

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>多対多のリレーションシップ (Entity Framework) のサポート

Entity Framework では、テーブルのリレーションシップをコレクションとして公開することで実装されている間に多対多リレーションシップの豊富なサポート、*エンティティ*オブジェクト。 新しい`ManyToMany.ascx`と`ManyToMany_Edit.ascx`を表示して、多対多のリレーションシップに関係するデータの編集をサポートするフィールド テンプレートが追加されました。

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>コントロールの表示と列挙のサポートを新しい属性

*DisplayAttribute*フィールドの表示方法を細かく制御に追加されました。 *DisplayName*以前のバージョン フィールドのキャプションとして使用される名前を変更することを許可されている動的データの属性。 新しい*DisplayAttribute*クラスを使用して、順序のフィールドを表示およびフィルターとしてフィールドを使用するかどうかなどのフィールドを表示するための他のオプションを指定できます。 属性は、独立したコントロールのラベルに使用される名前のも用意されています、 *GridView*コントロールで使用される名前、 *DetailsView*の透かしが使用されると、フィールドのヘルプ テキストを制御します。、。フィールド (フィールドは、テキスト入力を受け入れる) 場合。

*EnumDataTypeAttribute*列挙体にフィールドをマップできるようにクラスが追加されました。 フィールドにこの属性を適用する場合は、列挙型を指定します。 動的なデータは、新しい`Enumeration.ascx`フィールド テンプレートを表示および編集する列挙値の UI を作成します。 テンプレートは、列挙の名前に、データベースから値をマップします。

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>フィルターのサポートの強化

動的データ 1.0 は、ブール型の列と外部キー列の組み込みのフィルターに付属します。 フィルターでは、表示されているかどうか、またはどのような順序では表示の指定は許可しませんでした。 新しい*DisplayAttribute*属性のアドレスを両方提供することでこれらの問題をフィルターとして列を表示するかどうかを制御し、どのような順序で表示されます。

追加の機能強化がフィルター処理のサポートがされている[新しいを使用するように記述](#0.2__QueryExtender "_QueryExtender") Web フォームの機能です。 これにより、フィルターで使用されるデータ ソース コントロールの知識がなくても、フィルターを作成できます。 これらの拡張機能とフィルターもなっているコントロールのテンプレートに新しいものを追加できます。 最後に、 *DisplayAttribute*前に説明したクラスが同じで、オーバーライドできる既定のフィルターを使用する*UIHint*オーバーライドする列の既定のフィールド テンプレートを使用します。

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web 開発機能の強化

大きい CSS 互換性、HTML および ASP.NET のマークアップ スニペットと新しい動的な IntelliSense の JavaScript による生産性の向上のため、Visual Studio 2010 での web 開発が強化されました。

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>強化された CSS 互換性

Visual Studio 2010 で Visual Web Developer デザイナーは CSS 2.1 標準への準拠を向上させるために更新されました。 デザイナーは、HTML ソースの整合性を維持し、Visual Studio の以前のバージョンよりも堅牢です。 内部的には、アーキテクチャの機能強化も行われました将来的で、表示、レイアウト、および保守しやすくなります。

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML および JavaScript のスニペット

HTML エディターで IntelliSense オートコンプリート タグ名。 IntelliSense スニペット機能が自動補完し、タグ全体。 Visual Studio 2010 では、IntelliSense スニペットは、javascript、c# および Visual Basic、Visual Studio の以前のバージョンでサポートされていたと共にサポートされます。

Visual Studio 2010 に役立つ、オート コンプリート一般的な ASP.NET タグと HTML タグ、必要な属性を含む 200 を超えるのスニペットが含まれています (など、runat ="server") と共通の属性をタグに特定 (など*ID*、 *DataSourceID*、 *ControlToValidate*、および*テキスト*)。

他のスニペットをダウンロードすることも一般的なタスクのユーザーまたはチームを使用するマークアップのブロックをカプセル化する、独自のスニペットを記述することができます。

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>JavaScript IntelliSense の機能強化

Visual 2010 では、JavaScript IntelliSense は再設計されましたより豊富な編集エクスペリエンスを提供します。 IntelliSense は動的に生成されたメソッドによってなどのオブジェクト認識される*registerNamespace*およびその他の JavaScript フレームワークで使用される同様の手法です。 スクリプトの大規模なライブラリを分析して、ほとんどまたはまったく処理の遅延で IntelliSense を表示するパフォーマンスが向上します。 ほぼすべてのサード パーティ製ライブラリをサポートし、多様なコーディングのスタイルをサポートするために、互換性を大幅に増加されています。 入力して、IntelliSense によってすぐに活用できるように、ドキュメントのコメントは解析ようになりました。

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Visual Studio 2010 で web アプリケーションのデプロイ

ASP.NET 開発者は、Web アプリケーションをデプロイ、多くの場合、検索するなど、次の問題に遭遇します。

- 共有ホスティング サイトにデプロイすると、時間がかかり、FTP などのテクノロジが必要です。 さらに、手動でデータベースを構成する SQL スクリプトの実行などのタスクを実行する必要があり、アプリケーションとして仮想ディレクトリのフォルダーの構成などの IIS 設定を変更する必要があります。
- Web アプリケーションのファイルを展開するだけでなく、エンタープライズ環境で管理者頻繁にする必要があります変更 ASP.NET 構成ファイルと IIS の設定。 データベース管理者は、一連の実行中のアプリケーション データベースを取得する SQL スクリプトを実行する必要があります。 このようなインストールは多くの労力で、多くの場合、完了するには時間がかかると、慎重に文書化する必要があります。

Visual Studio 2010 には、これらの問題に対処して、シームレスに Web アプリケーションをデプロイするためのテクノロジが含まれています。 これらのテクノロジの 1 つは、IIS Web 配置ツール (MsDeploy.exe) です。

Visual Studio 2010 で web デプロイ機能には、次の主要な領域があります。

- Web のパッケージ化
- Web.config 変換
- データベースの配置
- 1 回のクリックは、Web アプリケーションを発行します。

次のセクションでは、これらの機能について詳しく説明します。

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Web のパッケージ化

Visual Studio 2010 では、MSDeploy ツールを使用して、圧縮 (.zip) ファイルと呼ばれる、アプリケーションを作成、 *Web パッケージ*します。 パッケージ ファイルには、アプリケーションと、次のコンテンツに関するメタデータが含まれています。

- IIS の設定、アプリケーション プールの設定や、エラー ページの設定が含まれています。
- 実際 Web コンテンツ、Web ページやユーザー コントロール、静的コンテンツ (イメージや HTML ファイル) などが含まれます。
- SQL Server データベース スキーマとデータ。
- セキュリティ証明書は、GAC、レジストリの設定でインストールするコンポーネント。

Web パッケージを任意のサーバーにコピーされ、IIS マネージャーを使用して手動でインストールされていることができます。 または、配置の自動化のコマンド ライン コマンドを使用して、または配置 Api を使用してなるパッケージをインストールすることができます。

Visual Studio 2010 は、組み込みの MSBuild タスクと Web のパッケージを作成するターゲットを提供します。 詳細については、次を参照してください。 [ASP.NET Web アプリケーション プロジェクトの配置の概要](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx)MSDN Web サイトと[10 + 20 の理由がなぜ Web パッケージを作成する必要があります](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html)Vishal Joshi's ブログ。

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web.config 変換

Visual Studio 2010 を紹介する Web アプリケーションの配置の[XML Document Transform (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)、変換できる機能は、`Web.config`ファイルの開発設定の運用環境の設定をします。 変換の設定がという名前の変換ファイルで指定された`web.debug.config`、`web.release.config`など。 (これらのファイルの名前は一致 MSBuild 構成です)。変換ファイルには、展開済みにする必要のある変更のみが含まれています。`Web.config`ファイル。 単純な構文を使用して変更を指定します。

次の例の一部を示しています、`web.release.config`リリース構成のデプロイのファイルを生成する可能性があります。 指定する展開時の例では置換キーワードを指定します、 *connectionString*内のノード、`Web.config`例では、記載されている値を持つファイルが置き換えられます。

[!code-xml[Main](overview/samples/sample102.xml)]

詳細については、次を参照してください[Web アプリケーション プロジェクトの配置の Web.config 変換構文](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx)msdn <a id="0.2_a"> </a> Web サイトと[Web 展開: Web.Config 変換](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)。Vishal Joshi's ブログ。

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>データベースの配置

Visual Studio 2010 の展開パッケージには、SQL Server データベースへの依存関係を含めることができます。 パッケージの定義の一部として、ソース データベースの接続文字列を指定します。 Web パッケージを作成するときに、Visual Studio 2010 は、データベース スキーマとデータでは、必要に応じて、SQL スクリプトの作成し、パッケージに追加します。 カスタムの SQL スクリプトを提供し、サーバーで実行する必要がありますシーケンスを指定できます。 デプロイ時に、ターゲット サーバーの適切な接続文字列を指定します。展開プロセスは、この接続文字列を使用してデータベース スキーマを作成し、データを追加するスクリプトを実行します。

さらに、1 回のクリックを使用して、発行、アプリケーションがリモート共有ホスティング サイトに発行されると、データベースを直接発行する配置を構成することができます。 詳細については、次を参照してください。[方法: Web アプリケーション プロジェクトでのデータベース配置](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx)MSDN Web サイトと[VS 2010 を使用したデータベースの配置](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi's ブログ。

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>1 回のクリックは、Web アプリケーションを発行します。

Visual Studio 2010 では、リモート サーバーに Web アプリケーションを発行する IIS のリモート管理サービスを使用することもできます。 ホスティング アカウントまたはサーバーをテストまたはステージング サーバーには、発行プロファイルを作成できます。 各プロファイルは、適切な資格情報を安全に保存できます。 ターゲットのいずれかに展開することができますし、Web の 1 回のクリックを使用して 1 回のクリックでのサーバーは、ツールバーを公開します。 Visual Studio 2010 で MSBuild のコマンドラインを使用して公開することもできます。 これにより、発行を継続的インテグレーション モデルに含めるチーム ビルド環境を構成できます。

詳細については、次を参照してください。[方法: Web アプリケーション プロジェクトを使用して 1 回のクリックの 発行および Web デプロイのデプロイ](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx)MSDN Web サイトと[1 クリックで VS 2010 での発行を Web](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi's ブログ。 Visual Studio 2010 で Web アプリケーションのデプロイに関するビデオ プレゼンテーションを表示するを参照してください。 [Web 開発者プレビューには、VS 2010](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Vishal Joshi's ブログ。

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>リソース

次の Web サイトでは、ASP.NET 4 および Visual Studio 2010 に関する追加情報を提供します。

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) -MSDN Web サイト上の ASP.NET 4 用の公式ドキュメント。
- [https://www.asp.net/](https://www.asp.net/) -ASP.NET チームの Web サイト。
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) [ASP.NET 動的データのコンテンツ マップ](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx)-ASP.NET チーム サイトと ASP.NET Dynamic Data の公式ドキュメントでのオンライン リソースです。
- [https://www.asp.net/ajax/](../../ajax/index.md) -メインの Web リソースを ASP.NET Ajax 開発します。
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Visual Web Developer チーム ブログ Visual Studio 2010 の機能に関する情報が含まれています。
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) -ASP.NET のプレビュー リリースのメインの Web リソース。

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>免責情報

このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。

このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。 マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。

このホワイト ペーパーは、情報提供のみを目的としています。 マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。

お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。 このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。

マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。 別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。

明記されない限り、会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、イベント実在架空のものと関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所、またはイベントは、または推論する必要があります。

© 2009 Microsoft Corporation. All rights reserved.

Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。

本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。
