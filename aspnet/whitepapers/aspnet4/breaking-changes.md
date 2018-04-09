---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 の重大な変更 |Microsoft ドキュメント
author: rick-anderson
description: このドキュメントでは、行われた .NET Framework のバージョンを使用して作成されたアプリケーションに影響を与える可能性のある 4 つのリリースの変更について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 7eea51add6b05684357314e3d6aa5087383c6408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 の重大な変更
====================
> このドキュメントでは、行われた .NET Framework のバージョンの ASP.NET 4 Beta 1、Beta 2 リリースを含む、以前のリリースを使用して作成されたアプリケーションに影響を与える可能性のある 4 つのリリースの変更について説明します。
> 
> [このホワイト ペーパーをダウンロードします。](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>目次

[Web.config ファイルで ControlRenderingCompatibilityVersion 設定](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode 変更](#0.1__Toc256770142 "_Toc256770142")  
[単一引用符をエンコード HtmlEncode そして UrlEncode](#0.1__Toc256770143 "_Toc256770143")  
[ASP.NET ページ (.aspx) パーサーが Stricter](#0.1__Toc256770144 "_Toc256770144")  
[更新のブラウザー定義ファイル](#0.1__Toc256770145 "_Toc256770145")  
[ルート Web 構成ファイルから削除された System.Web.Mobile.dll](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET 要求の検証](#0.1__Toc256770147 "_Toc256770147")  
[既定のハッシュ アルゴリズムが、HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[新しい ASP.NET 4 のルートの構成に関連する構成エラー](#0.1__Toc256770149 "_Toc256770149")  
[4 つの子の ASP.NET アプリケーションでのエラーに起動するときに ASP.NET 2.0 または ASP.NET 3.5 アプリケーション](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 Web サイトは、SharePoint がインストールされているコンピューターの起動に失敗する](#0.1__Toc256770151 "_Toc256770151")  
[HttpRequest.FilePath プロパティが含まれなくなりました PathInfo 値](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 アプリケーションは eurl.axd を参照している HttpException エラーを生成する可能性があります](#0.1__Toc256770153 "_Toc256770153")  
[イベント ハンドラーは IIS 7 や IIS 7.5 の既定のドキュメントではされません発生しないする可能性が統合モード](#0.1__Toc256770154 "_Toc256770154")  
[ASP.NET コード アクセス セキュリティ (CAS) の実装を変更](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser および System.Web.Security Namespace で他の型が移動されました](#0.1__Toc256770156 "_Toc256770156")  
[出力キャッシュ方法を変更するための変更\*HTTP ヘッダー](#0.1__Toc256770157 "_Toc256770157")  
[Passport System.Web.Security 型は Obsolete](#0.1__Toc256770158 "_Toc256770158")  
[ASP.NET 4 のイメージを表示するために失敗した MenuItem.PopOutImageUrl プロパティ](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl とパスに円記号が含まれている場合は、イメージを表示するために Menu.DynamicPopOutImageUrl 失敗](#0.1__Toc256770160 "_Toc256770160")  
[Disclaimer](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Web.config ファイルで ControlRenderingCompatibilityVersion 設定

ASP.NET コントロールは、具体的には、マークアップをレンダリングする方法を指定するために、.NET Framework version 4 に変更されています。 .NET Framework の以前のバージョンでは、一部のコントロールには、無効にする方法がなかったマークアップが生成されます。 既定では、ASP.NET 4 のマークアップのこの型は生成されません。

Visual Studio 2010 を使用して ASP.NET 2.0 の ASP.NET 3.5 アプリケーションをアップグレードする場合、ツールに設定を自動的に追加、`Web.config`レガシ レンダリングを保持するファイル。 ただし、IIS のアプリケーション プールを変更して .NET Framework 4 をターゲットにするようにアプリケーションをアップグレードする場合、ASP.NET では新しい表示モードが既定で使用されます。 新しいレンダリング モードを無効にするで次の設定を追加、`Web.config`ファイル。

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

新しい動作によってもたらされる主要なレンダリングの変更点は次のとおりです。

- **イメージ**と**ImageButton**コントロールが表示されなく、`border="0"`属性。
- **BaseValidator**それから派生するクラスと検証コントロールが既定では不要になった赤いテキストを表示します。
- **HtmlForm**コントロールが表示されない、**名前**属性。
- **テーブル**レンダリングを制御できなく、`border="0"`属性。
- ユーザー入力用に設計されていないコントロール (たとえば、**ラベル**コントロール) 表示されなく、`disabled="disabled"`属性の場合、**有効**プロパティに設定されている**false**(または、コンテナー コントロールからこの設定を継承する場合)。

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode 変更

**ClientIDMode** ASP.NET 4 で設定を使用して、ASP.NET によって生成される方法を指定できます、 **id**の HTML 要素の属性です。 ASP.NET の以前のバージョンで、既定の動作は、 **AutoID**設定**ClientIDMode**です。 ただし、既定の設定は**Predictable**です。

Visual Studio 2010 を使用して ASP.NET 2.0 の ASP.NET 3.5 アプリケーションをアップグレードする場合、ツールに設定を自動的に追加、 `Web.config` .NET Framework の以前のバージョンの動作を保持するファイル。 ただし、IIS のアプリケーション プールを変更して .NET Framework 4 をターゲットにするようにアプリケーションをアップグレードする場合、ASP.NET では新しいモードが既定で使用されます。 新しいクライアント ID モードを無効にするには、追加の次の設定、`Web.config`ファイル。

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode そして UrlEncode エンコードの単一引用符

ASP.NET 4 で、 **HtmlEncode**と**UrlEncode**のメソッド、 **HttpUtility**と**HttpServerUtility**クラスに更新されました単一引用符文字 (') を次のようにエンコードするには。

- **HtmlEncode**メソッドには、単一引用符のインスタンスができるようにエンコード ' です。
- **UrlEncode**メソッドは、%27 としての単一引用符のインスタンスをエンコードします。

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>ASP.NET ページ (.aspx) パーサーが Stricter

ASP.NET ページのページ パーサー (`.aspx`ファイル) およびユーザー コントロール (`.ascx`ファイル) ASP.NET 4 の厳格なは、無効なマークアップの複数のインスタンスを拒否します。 たとえば、次の 2 つのスニペットは、ASP.NET の以前のリリースで正常に解析しますが、ASP.NET 4 のパーサー エラーが発生ようになりました。

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

末尾に無効なセミコロンに注意してください、 **HiddenField**タグ。

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

閉じられていないことを確認**スタイル**実行時に属性、 **CssClass**属性。

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>ブラウザー定義ファイルの更新

ブラウザー定義ファイルは、新しいブラウザーとデバイスや、更新されたブラウザーとデバイスに関する情報を含むように、更新されました。 Netscape Navigator などの古いブラウザーとデバイスは削除され、Google Chrome や Apple iPhone などの新しいブラウザーとデバイスが追加されました。

削除されたブラウザー定義の 1 つを継承するカスタム ブラウザー定義がご利用のアプリケーションに含まれている場合は、エラーが表示されます。 たとえば場合、`App_Browsers`フォルダーには IE2 ブラウザーの定義を継承するブラウザーの定義が含まれています、次の構成エラー メッセージが表示されます。

- ID 'IE2' を持つブラウザーまたはゲートウェイ要素が見つかりません。

> [!NOTE]
> **HttpBrowserCapabilities**オブジェクト (ページのによって公開される**Request.Browser**プロパティ) は、ブラウザーの定義ファイルによって決まります。 そのため、ASP.NET 4 では、このオブジェクトのプロパティへのアクセスによって返される情報は、ASP.NET の以前のバージョンで返される情報よりも異なる可能性があります。


次のフォルダーからのブラウザー定義ファイルをコピーして、古いブラウザー定義ファイルに戻すことができます。

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

対応するファイルをコピー `\CONFIG\Browsers` ASP.NET 4 用フォルダーです。 ファイルをコピーした後の実行、Aspnet\_regbrowsers.exe コマンド ライン ツールです。

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll ルート Web 構成ファイルから削除

System.Web.Mobile.dll アセンブリへの参照がルートに含まれている以前のバージョンの ASP.NET では、`Web.config`ファイルで、**アセンブリ**セクションです。 パフォーマンスを向上するために、このアセンブリへの参照は削除されました。

System.Web.Mobile.dll アセンブリは、ASP.NET 4 に含まれるが、推奨されていません。 System.Web.Mobile.dll アセンブリの型を使用する場合は、どちらかルートにこのアセンブリへの参照を追加`Web.config`ファイルまたはアプリケーションへ`Web.config`ファイル。 たとえば、(非推奨) の ASP.NET モバイル コントロールのいずれかを使用する場合は、System.Web.Mobile.dll アセンブリへの参照を追加する必要があります、`Web.config`ファイル。

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>ASP.NET 要求の検証

ASP.NET の要求の検証機能は、特定のレベルのクロス サイト スクリプト (XSS) 攻撃に対する既定の保護を提供します。 ASP.NET の以前のバージョンでは、要求の検証は、既定で有効にされました。 ただし、ASP.NET ページにのみ適用 (`.aspx`ファイルと、クラス ファイル) と、それらのページが実行するときにのみです。

ASP.NET 4 で既定では、要求の検証が有効になっている、すべての要求の前に有効になっているため、 **BeginRequest**の HTTP 要求のフェーズです。 その結果、要求の検証は、.aspx ページの要求だけでなく、すべての ASP.NET リソースに対する要求に適用されます。 これには、Web サービスの呼び出しとカスタム HTTP ハンドラーなどの要求が含まれます。 要求の検証をアクティブなは、カスタム HTTP モジュールは HTTP 要求の内容を読み取るときにもです。

その結果、要求検証エラーは、以前のエラーをトリガーしませんでした要求が発生ようになりました。 ASP.NET 2.0 要求の検証機能の動作に戻すには、追加の次の設定、`Web.config`ファイル。

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

ただし、XSS 可能性のある安全でない HTTP 入力の攻撃ベクトル既存ハンドラー、モジュール、またはその他のカスタム コードのアクセス可能性があるかどうかを決定する要求の検証エラーを分析することをお勧めします。

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>既定のハッシュ アルゴリズムは HMACSHA256 はようになりました。

ASP.NET では、暗号化とハッシュ アルゴリズムの両方を使用して、フォーム認証 Cookie やビュー ステートなどのデータをセキュリティで保護します。 既定では、ASP.NET 4 は今すぐ cookie やビュー ステートにハッシュ操作の HMACSHA256 アルゴリズムを使用します。 ASP.NET の以前のバージョンでは、古い HMACSHA1 アルゴリズムを使用します。

混合 ASP.NET 2.0/ASP.NET 4 を実行する場合、アプリケーションに影響があります環境フォーム認証 cookie などのデータが across.NET Framework のバージョンを使用する必要があります。 古い HMACSHA1 アルゴリズムを使用する ASP.NET 4 Web アプリケーションを構成するには、次の設定を追加、`Web.config`ファイル。

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>新しい ASP.NET 4 のルートの構成に関連する構成エラー

ルートの構成ファイル (、`machine.config`ファイルとルート`Web.config`ファイル) 内にある ASP.NET 3.5 では定型的な構成情報の大部分を含めるに更新されている .NET Framework 4 (とそのため ASP.NET 4) のアプリケーション`Web.config`ファイル。 管理対象の IIS 7 および IIS 7.5 の構成システムは複雑であるため ASP.NET 4 と IIS 7 および IIS 7.5 ASP.NET 3.5 アプリケーションを実行している可能性が ASP.NET または IIS のいずれかで構成エラーです。

現実的な場合、Visual Studio 2010 でのプロジェクトのアップグレード ツールを使用して ASP.NET 4 を ASP.NET 3.5 アプリケーションをアップグレードすることをお勧めします。 Visual Studio 2010 では、ASP.NET 3.5 アプリケーションの自動的に変更されます`Web.config`ASP.NET 4 用の適切な設定を格納するファイル。

ただし、再コンパイルせず、.NET Framework 4 を使用して ASP.NET 3.5 アプリケーションを実行するシナリオのサポートを勧めします。 その場合は、アプリケーションを手動で変更する必要があります`Web.config`ファイル、アプリケーションの .NET Framework 4] と [IIS 7 や IIS 7.5 を実行する前にします。

次の 2 つのセクションでは、ソフトウェアのさまざまな組み合わせのために必要な場合があります変更について説明します。

**Windows Vista SP1 または Windows Server 2008 SP1 では、どちらもの修正プログラム KB958854 も SP2 がインストールされています。** この構成では、IIS 7 構成システム正しくマージできませんアプリケーションの管理対象の構成アプリケーション レベルを比較することによって`Web.config`ファイル、ASP.NET 2.0 を`machine.config`ファイル。 このため、アプリケーション レベルのため`Web.config`ファイルから、.NET Framework 3.5 またはそれ以降が必要な**system.web.extensions**で IIS 7 の検証エラーが発生しないように構成セクションで定義 (要素)。

ただし、アプリケーション レベルを手動で変更`Web.config`の元定型構成セクションの定義を Visual Studio 2008 で導入されたを正確に一致しないファイルのエントリには、ASP.NET の構成エラーが発生します。 (Visual Studio 2008 で生成される既定の構成エントリが正しく動作します。)一般的な問題では、手動で変更される`Web.config`ファイルを省略して、 **allowDefinition**と**requirePermission**さまざまな構成セクションに含まれている構成属性定義です。 これが原因でアプリケーション レベルの省略形の構成セクションの間で不整合`Web.config`ファイルおよび ASP.NET 4 で完全な定義`machine.config`ファイル。 その結果、実行時に、ASP.NET 4 構成システムは構成エラーをスローします。

**Windows Vista SP2、Windows Server 2008 SP2、Windows 7、Windows Server 2008 R2 および Windows Vista SP1 もおよび KB958854 の修正プログラムがインストールされている Windows Server 2008 SP1。**

このシナリオでは、IIS 7 および IIS 7.5 のネイティブな構成システム エラーが返されます、構成で文字列比較を実行するため、**型**マネージ構成セクション ハンドラーに対して定義されている属性です。 すべて`Web.config`Visual Studio 2008 と Visual Studio 2008 SP1 で生成されるファイルが「3.5」の文字列型である、 **system.web.extensions** (および関連する) 構成セクション ハンドラーのためと ASP.NET4`machine.config`ファイルが、「4.0」、**型**属性に同じ構成セクション ハンドラー、Visual Studio 2008 または Visual Studio 2008 SP1 で常に生成されるアプリケーションの IIS 7 での構成の検証に失敗し、IIS 7.5 です。

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>これらの問題を解決します。

最初のシナリオの回避策は、アプリケーション レベルの更新を`Web.config`ファイルから定型構成を含めることによって、 `Web.config` Visual Studio 2008 で自動的に生成されたファイルです。

最初のシナリオでは、別の回避策は、コンピューターに Vista または Windows Server 2008 Service Pack 2 をインストールするか、または KB958854 の修正プログラムをインストールする ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) の不適切な構成のマージの動作を修正しますIIS の構成システムです。 ただし、これらのアクションのいずれかを実行した後、アプリケーションではこの 2 番目のシナリオについて説明した問題のための構成エラーが検出可能性があります。

2 番目のシナリオの回避策が削除またはすべてをコメントには、 **system.web.extensions**構成セクションの定義と構成セクションは、アプリケーション レベルの定義をグループ化`Web.config`ファイル。 これらの定義は、通常、アプリケーション レベルの上部にある`Web.config`ファイルし、を識別できます、 **configSections**要素とその子要素です。

両方のシナリオでは、ことをお勧めことも手動で削除する、 **system.codedom**セクションで、これは必須ではありません。

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>4 つの子の ASP.NET アプリケーションでのエラーに起動するときに ASP.NET 2.0 または ASP.NET 3.5 アプリケーション

旧バージョンの ASP.NET を実行する子アプリケーションとして構成されている ASP.NET 4 アプリケーションは、構成エラーまたはコンパイル エラーにより起動しない場合があります。 次の例は、影響を受けるアプリケーションのディレクトリ構造を示しています。

`/parentwebapp` (ASP.NET 2.0 または ASP.NET 3.5 を使用するように構成)  
`/childwebapp` (ASP.NET 4 を使用するように構成)

アプリケーション、`childwebapp`フォルダーは、IIS 7 や IIS 7.5 でを起動し、構成エラーをレポートには失敗します。 エラー テキストには、次のようなメッセージが含まれます。

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

IIS 6 では、アプリケーション、`childwebapp`フォルダーも失敗を開始するには、別のエラーが報告されます。 たとえば、エラー テキストは、次状態可能性があります。

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

これらのシナリオが発生する親のアプリケーションから構成情報、`parentwebapp`フォルダーは、子 web によって使用される最終的なマージされた構成設定を決定する構成情報の階層の一部アプリケーションを`childwebapp`フォルダーです。 かどうか、ASP.NET 4 Web アプリケーションには、IIS 7 (または IIS 7.5) または IIS 6 でが実行中、によって、IIS の構成システムまたは ASP.NET 4 コンパイル システムはエラーを返します。

この問題を解決するのには、ASP.NET 4 アプリケーションを使用する子を取得するを実行する必要がある手順は、IIS 7 (または IIS 7.5) で、ASP.NET 4 アプリケーションが IIS 6 でまたはを実行するかどうかによって異なります。

### <a name="step-1-iis-7-or-iis-75-only"></a>手順 1 (IIS 7 や IIS 7.5 のみ)

この手順は、IIS 7 を実行するオペレーティング システムまたは Windows Vista、Windows Server 2008、Windows 7、および Windows Server 2008 R2 を含む IIS 7.5 でのみ必要があります。

移動、 **configSections**内の定義、`Web.config`親アプリケーション (ASP.NET 2.0 または ASP.NET 3.5 を実行しているアプリケーション) のファイルをルートに`Web.config`the.NET Framework 2.0 用のファイルです。 IIS 7 および IIS 7.5 のネイティブな構成システム スキャンが、 **configSections**構成ファイルの階層がマージされる際の要素。 移動、 **configSections**から親 Web アプリケーションの定義を`Web.config`ルートにファイル`Web.config`ファイルには、ASP.NET 4 子に対して行われる構成のマージ プロセスから要素を効果的に非表示になりますアプリケーション。

32 ビット オペレーティング システム上または 32 ビット アプリケーション プールの場合、ルート`Web.config`ASP.NET 2.0 および ASP.NET 3.5 用のファイルは次のフォルダーにある通常。

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

64 ビット オペレーティング システムまたは 64 ビット アプリケーション プールの場合、ルート`Web.config`ASP.NET 2.0 および ASP.NET 3.5 用のファイルは次のフォルダーにある通常。

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

移動する必要があります、64 ビット コンピューターで 32 ビットおよび 64 ビットの両方の Web アプリケーションを実行する場合、 **configSections**要素をルートに`Web.config`32 ビットと 64 ビット システムの両方のファイルです。

配置すると、 **configSections**ルート内の要素`Web.config`ファイルでセクションを貼り付けした直後に、**構成**要素。 次の例では、どのようなルートの上部`Web.config`ファイルは、要素を移動が完了したようになります。

> [!NOTE]
> 次の例では、コマンドラインを読みやすくするため改行されています。


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>手順 2 (IIS の全バージョン)

この手順は、IIS 6 または IIS 7 (または IIS 7.5) で ASP.NET 4 子 Web アプリケーションが実行されているかどうか必要です。

`Web.config`親 ASP.NET 2 または ASP.NET 3.5 では、実行されている Web アプリケーションのファイルを追加、**場所**(システムの場合、IIS と ASP.NET の構成) を明示的に指定されるタグを構成エントリのみ親 Web アプリケーションに適用されます。 次の例の構文を示しています、**場所**要素を追加します。

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

例を次にどのように**場所**ラップ始まるすべての構成セクションにタグを使用、 **appSettings**セクションで終わると**system.webServer**セクションです。

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

手順 1. と 2. を完了したら、エラーが発生しない子 ASP.NET 4 Web アプリケーションが開始されます。

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 Web サイトは、SharePoint がインストールされているコンピューターの起動に失敗します。

SharePoint を実行する web サーバーが、 `Web.config` SharePoint Web サイトのルートに配置されているファイル (たとえば、`c:\inetpub\wwwroot\web.config`の既定の Web サイト)。 この`Web.config`ファイルは、SharePoint 設定カスタムの部分的な信頼レベル WSS をという名前\_最小限に抑える。

この種類の SharePoint Web サイトの子として展開されている ASP.NET 4 Web サイトを実行しようとする場合は、次のエラーが表示されます。

`Could not find permission set named 'ASP.NET'.`

このエラーは、ASP.NET 4 のコード アクセス セキュリティ (CAS) インフラストラクチャが次の ASP.NET を名前付き権限セットのために発生します。 ただし、一部が WSS によって参照されている構成ファイルを信頼\_最小限が含まれていないその名前を持つすべての権限セットにはです。

現在 ASP.NET と互換性がある SharePoint のバージョンがありません。 その結果、必要がありますいないしようとする SharePoint Web の下に子サイトとサイトで ASP.NET 4 Web サイトを実行します。

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>HttpRequest.FilePath プロパティでは、PathInfo 値は廃止されています

以前のバージョンの ASP.NET が含まれている、 **PathInfo**プロパティから返されたさまざまなファイル パスに関連するなどの値で値**HttpRequest.FilePath**、 **HttpRequest.AppRelativeCurrentExecutionFilePath**、および**HttpRequest.CurrentExecutionFilePath**です。 ASP.NET 4 が含まれなくなりました、 **PathInfo**これらのプロパティからの戻り値の値。 代わりに、 **PathInfo**情報は提供**HttpRequest.PathInfo**です。 たとえば、次のような URL フラグメントがあるとします。

`/testapp/Action.mvc/SomeAction`

以前のバージョンの ASP.NET、 **HttpRequest**プロパティは、次の値を取ります。

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (空の)

ASP.NET 4 で**HttpRequest**プロパティは、次の値を代わりにあります。

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 アプリケーションは eurl.axd を参照している HttpException エラーを生成する可能性があります

IIS 6 で ASP.NET 4 を有効にすると、(Windows Server 2003 または Windows Server 2003 R2 のいずれか) 内の IIS 6 で実行される ASP.NET 2.0 アプリケーションは、次のようにエラーを生成可能性があります。

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

このエラーは、さらに処理のために、ASP.NET 4 のネイティブ コンポーネントが ASP.NET のマネージ部分を拡張子のない URL を渡します ASP.NET 検出すると、ASP.NET 4 を使用する Web サイトが構成されているために発生します。 ただし、ASP.NET 2.0 を使用するように ASP.NET 4 Web サイトの下にある仮想ディレクトリを構成している場合は、この方法は結果として文字列"eurl.axd"が含まれる変更の URL に拡張子のない URL を処理しています。 この変更の URL は、ASP.NET 2.0 アプリケーションに送信されます。 ASP.NET 2.0 では、"eurl.axd"形式を認識できません。 ASP.NET 2.0 がという名前のファイルを検索しようとしたため、`eurl.axd`し実行します。 要求に失敗したため、このようなファイルが存在しない、 **HttpException**例外。

次のオプションのいずれかを使用してこの問題を回避できます。

### <a name="option-1"></a>オプション 1

ASP.NET 4 が Web サイトを実行するために必要でない場合は、ASP.NET 2.0 を使用するようにサイトに再マップします。

### <a name="option-2"></a>オプション 2

ASP.NET 4 が必要な場合、Web サイトを実行するために、その子の ASP.NET 2.0 の仮想ディレクトリを ASP.NET 2.0 にマップされている別の Web サイトに移動します。

### <a name="option-3"></a>オプション 3

ASP.NET 2.0 Web サイトを再マップする、または仮想ディレクトリの場所を変更するのには、実用的ではない場合、は、拡張子のない URL を ASP.NET 4 での処理を明示的に無効にします。 次の手順に従います。

1. Windows レジストリで、次のノードを開きます。

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. 新しい**DWORD**という名前の値**EnableExtensionlessUrls**です。
2. 設定**EnableExtensionlessUrls**を 0 にします。 これは、拡張子のない URL の動作を無効にします。
3. レジストリ値を保存して、レジストリ エディターを閉じます。
4. 実行、 **iisreset**コマンド ライン ツールは、iis に新しいレジストリ値を読み取れません。

> [!NOTE]
> 設定**EnableExtensionlessUrls**を 1 に拡張子のない URL の動作を有効にします。 これは、既定の設定値が指定されていない場合です。


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>イベント ハンドラーは IIS 7 や IIS 7.5 の既定のドキュメントではされません発生しないする可能性が統合モード

ASP.NET 4 には、変更の変更が含まれています。 どのように**アクション**HTML の属性**フォーム**拡張子のない URL が既定のドキュメントに解決されると、要素が表示されます。 既定のドキュメントに解決するには拡張子のない URL の例としては[ http://contoso.com/](http://contoso.com/)への要求で結果として得られる、 [ http://contoso.com/Default.aspx](http://contoso.com/Default.aspx)です。

ASP.NET 4 は、HTML を表示するようになりました**フォーム**要素の**アクション**がマップされている既定のドキュメントを拡張子のない URL に要求が行われたときに属性値に空の文字列。 たとえば、要求を ASP.NET の以前のリリースで[ http://contoso.com ](http://contoso.com)への要求になる`Default.aspx`です。 そのドキュメントでは、オープンで**フォーム**タグは、次の例のように表示は。

`<form action="Default.aspx" />`

ASP.NET 4 で要求を[ http://contoso.com ](http://contoso.com)への要求では結果も`Default.aspx`します。 ただし、ASP.NET は、HTML を開くようになりましたを表示**フォーム**次の例のようにタグ。

`<form action="" />`

この方法の違い**アクション**属性が表示されるフォーム ポストが IIS と ASP.NET によって処理される方法でわずかな変更が発生することができます。 ときに、**アクション**属性は、空の文字列は、IIS **DefaultDocumentModule**オブジェクトを作成する子要求`Default.aspx`です。 ほとんどの状況では、この子の要求は、アプリケーション コードに対して透過的と`Default.aspx`ページが正常に実行します。

ただし、マネージ コードと IIS 7 または IIS 7.5 統合モードとの間のやり取りによって、マネージ .aspx ページが子要求中に正常に動作を停止することがあります。 かどうか、次の条件が発生するへの子要求、`Default.aspx`ドキュメントは、エラーまたは予期しない動作になります。

1. 使用してブラウザーを .aspx ページが送信される、**フォーム**要素の**アクション**属性に設定""です。
2. フォームを ASP.NET に送信します。
3. 管理対象の HTTP モジュールは、エンティティの本文の一部を読み取ります。 たとえば、モジュールを読み込みます**Request.Form**または**Request.Params**です。 これにより、POST 要求のエンティティ本体がマネージ メモリに読み込まれます。 その結果、エンティティ本体は、IIS 7 または IIS 7.5 統合モードで実行されているネイティブ コード モジュールから使用できなくなります。
4. IIS **DefaultDocumentModule**オブジェクトは、最終的に実行され、子の要求を作成、`Default.aspx`ドキュメント。 ただし、エンティティ本体はマネージ コードの一部によって既に読み取られているため、子要求に対する送信に使用できるエンティティ本体はありません。
5. 子の要求のハンドラーは、HTTP パイプラインを実行すると`.aspx`ハンドラー フェーズ中にファイルを実行します。
6. エンティティ本体がないためがフォーム変数がなく、ビュー状態がないあり、したがって情報がありません (存在する場合) は、どのようなイベントが発生すると想定されるを決定する .aspx ページ ハンドラーの使用可能です。 その結果、影響を受ける .aspx ページのポストバック イベント ハンドラーは実行されません。

この動作は、次の方法で回避できます。

- 既定のドキュメントの要求時に、要求のエンティティ ボディにアクセスしている HTTP モジュールを特定し、管理対象の要求に対してのみ実行する構成できるかどうかを確認します。 IIS 7 と IIS 7.5 の両方を統合モードで、モジュールに、次の属性を追加して管理対象の要求に対してのみ実行する HTTP モジュールをマークできます**system.webServer/modules**エントリ。

- `precondition="managedHandler"`

- この設定を無効には、モジュールを要求しないように IIS 7 および IIS 7.5 を決定するには、要求が管理されています。 ドキュメントの要求の既定の最初の要求は、拡張子のない URL にです。 そのため、IIS を実行しないとマークされている任意のマネージ モジュール マネージ ハンドラーの前提条件と最初の要求の処理中にします。 その結果、マネージ モジュールが誤って読み取れませんエンティティ ボディにエンティティ ボディがためは引き続き使用できますされ、子要求して、既定のドキュメントに渡されますが。

- 問題のある HTTP モジュールがすべての要求を実行する必要があるかどうか (静的ファイルの場合に解決される拡張子のない Url に対して、 **DefaultDocumentModule**マネージ要求などのオブジェクト)、によって影響を受ける .aspx ページを明示的に変更設定、**アクション**プロパティのページの**System.Web.UI.HtmlControls.HtmlForm**空でない文字列を制御します。 たとえば、既定のドキュメントが`Default.aspx`、明示的に設定するページのコードを変更、 **HtmlForm**コントロールの**アクション**プロパティを"Default.aspx"です。

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>ASP.NET コード アクセス セキュリティ (CAS) の実装への変更

ASP.NET 2.0、3.5 では、追加された ASP.NET の機能拡張機能では、.NET Framework 1.1 および 2.0 のコード アクセス セキュリティ (CAS) モデルの操作を使用するとします。 ただし、ASP.NET 4 での CAS の実装は大幅に見直されました。 その結果、グローバル アセンブリ キャッシュ (GAC) で実行されている信頼されたコードに依存する ASP.NET アプリケーションを部分的に信頼されたは、さまざまなセキュリティ例外により失敗可能性があります。 コンピューターの CAS ポリシーに対して大幅な変更に依存する部分的に信頼されたアプリケーションは可能性がありますも、セキュリティ例外で失敗します。

ASP.NET 1.1 および 2.0、new を使用しての動作に部分的に信頼された ASP.NET 4 アプリケーションを戻すことができます**legacyCasModel**属性、**信頼**次の例のように、構成要素:

`<trust level= "Medium" legacyCasModel="true" />`

レガシーの CAS モデルに戻す、ときに、次の古い CA の動作が有効にします。

- マシンの CA のポリシーが有効にします。
- 1 つのアプリケーション ドメイン内の複数の異なるアクセス許可セットが許可されます。
- 明示的なアクセス許可のアサーションが ASP.NET またはその他の .NET Framework コードのみがスタック上にあるときに呼び出される、GAC でアセンブリが必要ではありません。

.NET Framework 4 にシナリオの 1 つを元に戻すことはできません: 非 Web 部分信頼アプリケーションでは、System.Web.dll および System.Web.Extensions.dll で特定の Api を呼び出すことができなくします。 .NET Framework の以前のバージョンでが明示的に許可する部分的に信頼されたアプリケーションは Web 以外<strong>AspNetHostingPermission</strong>アクセス許可。 これらのアプリケーションを使用し、でした<strong>System.Web.HttpUtility</strong>、型、 <strong>System.Web.ClientServices\< 。/strong > * メンバーシップ、ロール、およびプロファイルに関連する名前空間、および種類です。 部分信頼の以外の Web アプリケーションからこれらの型の呼び出しは、.NET Framework 4 ではサポートされていません。

> [!NOTE]
> **HtmlEncode**と**HtmlDecode**の機能、 **System.Web.HttpUtility**クラスは、新しい .NET Framework 4 に移動された**System.Net.WebUtility**クラスです。 使用されている唯一の ASP.NET 機能する場合は、アプリケーションのコードを変更、新しい**WebUtility**クラスの代わりにします。


ASP.NET 4 で既定の CA の実装への変更の概要を次に示します。

- ASP.NET アプリケーション ドメインは、同種のアプリケーション ドメインで、ようになりました。 のみ部分的に信頼および完全信頼の付与は、アプリケーション ドメインでします。
- ASP.NET 部分的に信頼された許可セットはエンタープライズ レベル、コンピューター レベルまたはユーザー レベルの CAS ポリシーから独立しています。
- 3.5 および 3.5 SP1 に付属している ASP.NET アセンブリが .NET Framework 4 の透過性モデルを使用する変換されました。
- ASP.NET の使用**AspNetHostingPermission**属性を大幅に削減します。 この属性のほとんどの場合は、ASP.NET のパブリック Api から削除されましたが。
- ASP.NET ビルド プロバイダーによって作成される動的にコンパイルされたアセンブリは、透過的とアセンブリを明示的にマークに更新されました。
- ASP.NET のすべてのアセンブリの APTCA 属性は、Web ホスト環境でのみ受け入れ ようにマークが付きます。 ClickOnce と同様に、部分的に信頼されたの以外の Web ホスティング環境は ASP.NET アセンブリへの呼び出しできません。

新しい ASP.NET 4 のコード アクセス セキュリティ モデルの詳細については、次を参照してください。 [ASP.NET アプリケーションを使用したコード アクセス セキュリティを使用して](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx)MSDN Web サイトです。

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser および System.Web.Security Namespace で他の型が移動されました

ASP.NET メンバーシップで使用されるいくつかの型から移動された`System.Web.dll`新しい System.Web.ApplicationServices.dll アセンブリにします。 これらのタイプは、クライアントのタイプと拡張 .NET Framework SKU のタイプとの間のアーキテクチャ層の依存関係を解決するために移動しました。

Web サイト プロジェクト System.Web.ApplicationServices.dll は、既定で使用される参照アセンブリの一覧に、ASP.NET コンパイルのシステムで追加されたため、これらの型の移動の結果としての問題の必要はありません。 Visual Studio 2010 で開き、以前のバージョンの ASP.NET を ASP.NET 4 を使用して作成された Web サイト プロジェクトをアップグレードする場合は、プロジェクトがエラーなしコンパイルされます。

同様に、Visual Studio 2010 で開くことによって、以前のバージョンの ASP.NET を ASP.NET 4 で作成された Web アプリケーション プロジェクトをアップグレードする場合、アップグレード プロセスは System.Web.ApplicationServices.dll への参照をプロジェクトに追加します。 したがって、Web アプリケーション プロジェクトがエラーなしでコンパイルもをアップグレードします。

メンバーシップの種類は、別のアセンブリに移動された場合でも、以前のバージョンの ASP.NET を使用して作成されたコンパイル済みの (バイナリ) ファイルもエラーが発生しない「ASP.NET 4 で実行します。 ASP.NET 4 のバージョンに転送情報の種類が追加されました`System.Web.dll`に自動的にこれらの型の実行時の参照を型の新しい場所にルーティングします。

ただし、特定のメンバーシップの種類を使用して、ASP.NET の以前のバージョンからアップグレードされたクラス ライブラリは、ASP.NET 4 プロジェクトで使用する場合のコンパイルに失敗します。 たとえばをコンパイルし、次のようなエラーを報告するクラス ライブラリ プロジェクトが失敗する可能性があります。

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

System.Web.ApplicationServices.dll にクラス ライブラリ プロジェクトでの参照を追加することによってこの問題を回避することができます。

一覧を次に示しています、 *System.Web.Security*から移動された種類`System.Web.dll`System.Web.ApplicationServices.dll に。

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *System.Web.Security.MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>出力キャッシュ方法を変更するための変更\*HTTP ヘッダー

ASP.NET 1.0 では、バグの原因となったキャッシュを指定するページ`Location="ServerAndClient"`出力 – キャッシュ設定を出力として、`Vary:*`応答の HTTP ヘッダー。 その結果、クライアント ブラウザーに対して、ローカルにページをキャッシュしないように指示がなされていました。

ASP.NET 1.1 では、 **System.Web.HttpCachePolicy.SetOmitVaryStar**メソッドが追加されたを抑制する状況を呼び出す可能性があります`Vary:*`ヘッダー。 この方法は、重大な可能性があると見なされました出力の HTTP ヘッダーを変更するを選択した時に変更します。 ただし、ASP.NET では、動作を使用して混乱開発者とバグの報告は、開発者が、既存に対応していないことを提案**SetOmitVaryStar**動作します。

ASP.NET 4 で、意思決定を根本的な問題を修正しました。 `Vary:*`次のディレクティブを指定する応答から HTTP ヘッダーが送出された不要になった。

`<%@OutputCache Location="ServerAndClient" %>`

その結果、 **SetOmitVaryStar**が不要になったを抑制するために、`Vary:*`ヘッダー。

指定するアプリケーションで`Location="ServerAndClient"`で、 **@ OutputCache**ディレクティブ ページで、表示されます、動作の名前が含まれる、**場所**属性の値 – は、ページがあります呼び出すことを必要とせず、ブラウザーのキャッシュ可能な**SetOmitVaryStar**メソッドです。

場合は、アプリケーション内のページを生成する必要があります`Vary:*`を呼び出して、 **AppendHeader**次の例のように、メソッド。

`HttpResponse.AppendHeader("Vary","*");`

出力キャッシュの値を変更する代わりに、**場所**属性を"Server"にします。

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Passport System.Web.Security 種類は、廃止です。

ASP.NET 2.0 に組み込まれた Passport サポートには、Passport (今すぐ LiveID) の変更のため、数年に廃止され、サポートがされました。 5 つの型がで Passport に関連するその結果、 **System.Web.Security**でマークしている、 **ObsoleteAttribute**属性。

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>ASP.NET 4 のイメージを表示するために失敗した MenuItem.PopOutImageUrl プロパティ

ASP.NET 3.5 では、 *MenuItem.PopOutImageUrl*プロパティを使用して、メニュー項目を動的なサブメニューがあることを示すにメニュー項目に表示されるイメージの URL を指定できます。 次の例では、ASP.NET 3.5 ではマークアップでこのプロパティを指定する方法を示します。

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

ASP.NET 4 のデザインを変更、結果として出力に対してがレンダリングされません、 *PopOutImageUrl*のプロパティが設定されている場合、 *MenuItem*クラスです。 代わりで直接、画像の URL を指定する必要があります、*メニュー*いずれかを使用して制御、 *StaticPopOutImageUrl*プロパティまたは*DynamicPopOutImageUrl*プロパティです。 静的メニューを使用する場合、 *Menu.StaticPopOutImageUrl*プロパティがあることを示す静的メニュー項目サブメニューの場合、次の例に示すようにするために表示されるイメージの URL を指定します。

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

使用する動的メニューを使用する場合、 *Menu.DynamicPopOutImageUrl*プロパティを動的メニュー項目にサブメニューがあることを示すイメージの URL を指定します。 次の例は、1 つ前に似ていますが、設定する方法を示しています、 *DynamicPopOutImageUrl*動的メニューのプロパティです。

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

場合、 *Menu.DynamicPopOutImageUrl*プロパティが設定されていないと、 *Menu.DynamicEnableDefaultPopOutImage*プロパティに設定されている*false*イメージは表示されません。 同様に場合、 *StaticPopOutImageUrl*プロパティが設定されていないと、 *StaticEnableDefaultPopOutImage*プロパティに設定されている*false*イメージは表示されません。

これらのプロパティのパスを設定するときに、円記号の代わりにスラッシュ (/) を使用して (\)です。 詳細については、次を参照してください。 [Menu.StaticPopOutImageUrl とレンダリング イメージとパスを含む円記号を Menu.DynamicPopOutImageUrl 失敗](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu です。") 他の場所でこのドキュメントです。

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl および Menu.DynamicPopOutImageUrl がパスに円記号が含まれている場合は、イメージを表示するために失敗します。

ASP.NET 4 で指定すると、イメージ、 *Menu.StaticPopOutImageUrl*と*Menu.DynamicPopOutImageUrl* backlashes がパスに含まれている場合、プロパティは表示されません (\)です。 これは、ASP.NET の以前のバージョンからの変更点です。

次の例の*メニュー*マークアップの表示を制御、 *StaticPopOutImageUrl*プロパティ バック スラッシュを含むパスを使用して設定します。 ASP.NET 4 では、プロパティで指定したイメージは表示されません。

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

この問題を解決するで指定されているパスの値を変更、 *StaticPopOutImageUrl*と*DynamicPopOutImageUrl*スラッシュ (/) を使用するプロパティです。 次の例は、この変更を示しています。

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

以前のバージョンの ASP.NET を ASP.NET 4 から移行されたアプリケーション可能性がありますも、影響を受けるため、 *MenuItem.PopOutImageUrl*プロパティが変更されました。 詳細については、次を参照してください。 [、MenuItem.PopOutImageUrl プロパティが失敗した ASP.NET 4 のイメージをレンダリングする](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert")このドキュメントの他の場所。

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>免責情報

このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。

このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。 マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。

このホワイト ペーパーは、情報提供のみを目的としています。 マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。

お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。 このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。

マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。 別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。

特に明記されない限り、使用している会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、およびイベント実在は、架空のもの、および関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所またはイベントはまたは意図して推論する必要があります。

© 2010 Microsoft Corporation. All rights reserved.

Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。

本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。
