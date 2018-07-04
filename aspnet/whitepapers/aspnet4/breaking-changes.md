---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 の重大な変更 |Microsoft Docs
author: rick-anderson
description: このドキュメントでは、行われた .NET Framework バージョン 4 のリリースを使用して作成されたアプリケーションに影響を与える可能性のある変更について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: bf9968c49e904c064338d7123405cb6578a915f2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378651"
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 の重大な変更
====================
> このドキュメントでは、行われた .NET Framework のバージョンの ASP.NET 4 Beta 1 とベータ 2 リリースを含む、以前のリリースを使用して作成されたアプリケーションに影響を与える可能性のある 4 のリリースの変更について説明します。
> 
> [このホワイト ペーパーをダウンロードします。](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>目次

[Web.config ファイルの設定を ControlRenderingCompatibilityVersion](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode 変更](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode と UrlEncode ここでは、単一引用符をエンコード](#0.1__Toc256770143 "_Toc256770143")  
[ASP.NET ページ (.aspx) パーサーが Stricter](#0.1__Toc256770144 "_Toc256770144")  
[ブラウザー定義ファイルの更新](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll は、ルート Web 構成ファイルから削除](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET 要求の検証](#0.1__Toc256770147 "_Toc256770147")  
[既定のハッシュ アルゴリズムが、HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[新しい ASP.NET 4 ルート構成に関連する構成エラー](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 つの子アプリケーション起動しないときに ASP.NET 2.0 または ASP.NET 3.5 アプリケーション](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 Web サイトは、SharePoint がインストールされているコンピューターで起動に失敗する](#0.1__Toc256770151 "_Toc256770151")  
[HttpRequest.FilePath プロパティが含まれなくなりました PathInfo 値](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 アプリケーションで eurl.axd を参照する HttpException エラーが生成](#0.1__Toc256770153 "_Toc256770153")  
[イベント ハンドラーがいない発生しないで、既定のドキュメントでは、IIS 7 または IIS 7.5 統合モード](#0.1__Toc256770154 "_Toc256770154")  
[ASP.NET コード アクセス セキュリティ (CAS) の実装に](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser および System.Web.Security Namespace で他の型が移動された](#0.1__Toc256770156 "_Toc256770156")  
[出力キャッシュの変更を変えるに\*HTTP ヘッダー](#0.1__Toc256770157 "_Toc256770157")  
[Passport for System.Web.Security タイプは廃止](#0.1__Toc256770158 "_Toc256770158")  
[ASP.NET 4 でイメージをレンダリングする MenuItem.PopOutImageUrl プロパティが失敗した](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl とパスに円記号が含まれている場合は、イメージを表示するために Menu.DynamicPopOutImageUrl Fail](#0.1__Toc256770160 "_Toc256770160")  
[免責事項](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Web.config ファイルで ControlRenderingCompatibilityVersion 設定

ASP.NET コントロールより正確にマークアップをレンダリングする方法を指定するには、.NET Framework version 4 に変更されています。 .NET Framework の以前のバージョンでは、一部のコントロールには、無効にする方法がなかったマークアップが生成されます。 既定では、ASP.NET 4 のマークアップは、この型は生成されません。

Visual Studio 2010 を使用して ASP.NET 2.0 または ASP.NET 3.5 からアプリケーションをアップグレードする場合にする設定がツールで自動的に追加されます、`Web.config`ファイルを古い表示を維持します。 ただし、IIS のアプリケーション プールを変更して .NET Framework 4 をターゲットにするようにアプリケーションをアップグレードする場合、ASP.NET では新しい表示モードが既定で使用されます。 新しい表示モードを無効にするには、次の設定を追加、`Web.config`ファイル。

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

新しい動作がもたらす主なレンダリングの変更点は次のとおりです。

- **イメージ**と**ImageButton**コントロールが表示されなく、`border="0"`属性。
- **BaseValidator**そこから派生するクラスと検証コントロールでは、既定で赤いテキストされなくが表示されます。
- **HtmlForm**コントロールがレンダリングされない、**名前**属性。
- **テーブル**レンダリングを制御できなく、`border="0"`属性。
- ユーザー入力用に設計されていないコントロール (たとえば、**ラベル**コントロール) 表示されなく、`disabled="disabled"`属性の場合、**有効**プロパティに設定されて**false**(またはコンテナー コントロールからこの設定を継承する場合)。

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode 変更

**ClientIDMode** ASP.NET 4 での設定では、ASP.NET の生成を指定することができます、 **id**の HTML 要素の属性。 ASP.NET の以前のバージョンで、既定の動作が等しく、 **AutoID**設定**ClientIDMode**します。 ただし、既定の設定は、今すぐ**予測可能**します。

Visual Studio 2010 を使用して ASP.NET 2.0 または ASP.NET 3.5 からアプリケーションをアップグレードする場合にする設定がツールで自動的に追加されます、`Web.config`ファイルを以前のバージョンの .NET Framework の動作を維持します。 ただし、IIS のアプリケーション プールを変更して .NET Framework 4 をターゲットにするようにアプリケーションをアップグレードする場合、ASP.NET では新しいモードが既定で使用されます。 新しいクライアント ID モードを無効にするには、次の設定を追加、`Web.config`ファイル。

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode と UrlEncode ここでは、単一引用符をエンコードします。

ASP.NET 4 で、 **HtmlEncode**と**UrlEncode**のメソッド、 **HttpUtility**と**HttpServerUtility**クラスに更新されました一重引用符文字 (') を次のようにエンコードするには。

- **HtmlEncode**メソッドは、単一引用符のインスタンスをエンコードします '。
- **UrlEncode**メソッドは、27% として単一引用符のインスタンスをエンコードします。

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>ASP.NET ページ (.aspx) パーサーが Stricter

ASP.NET ページのページ パーサー (`.aspx`ファイル) およびユーザー コントロール (`.ascx`ファイル) は、ASP.NET 4 で、無効なマークアップのインスタンスが拒否されます。 たとえば、次の 2 つのスニペットは、ASP.NET の以前のリリースで正常に解析されますが、ASP.NET 4 でパーサー エラーが発生ようになりました。

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

末尾に無効なセミコロンに注意してください、 **HiddenField**タグ。

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

閉じられていないことを確認**スタイル**属性に実行される、 **CssClass**属性。

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>ブラウザー定義ファイルの更新

ブラウザー定義ファイルは、新しいブラウザーとデバイスや、更新されたブラウザーとデバイスに関する情報を含むように、更新されました。 Netscape Navigator などの古いブラウザーとデバイスは削除され、Google Chrome や Apple iPhone などの新しいブラウザーとデバイスが追加されました。

削除されたブラウザー定義の 1 つを継承するカスタム ブラウザー定義がご利用のアプリケーションに含まれている場合は、エラーが表示されます。 たとえば場合、`App_Browsers`フォルダーには IE2 ブラウザーの定義から継承するブラウザーの定義が含まれています、次の構成のエラー メッセージが表示されます。

- ID 'IE2' を持つブラウザーまたはゲートウェイの要素が見つかりません。

> [!NOTE]
> **HttpBrowserCapabilities**オブジェクト (ページのによって公開される**Request.Browser**プロパティ) のブラウザーの定義ファイルによって駆動されます。 そのため、ASP.NET 4 では、このオブジェクトのプロパティへのアクセスによって返される情報は、ASP.NET の以前のバージョンで返される情報よりも異なる可能性があります。


次のフォルダーから、ブラウザー定義ファイルをコピーして、古いブラウザー定義ファイルに戻すことができます。

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

対応するファイルをコピー `\CONFIG\Browsers` ASP.NET 4 用のフォルダー。 ファイルをコピーした後の実行、Aspnet\_regbrowsers.exe コマンド ライン ツール。

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll ルート Web 構成ファイルから削除

ルートに含まれていた System.Web.Mobile.dll アセンブリへの参照を ASP.NET の以前のバージョンで`Web.config`ファイル、**アセンブリ**セクションします。 パフォーマンスを向上させるためには、このアセンブリへの参照が削除されました。

ASP.NET 4 では、System.Web.Mobile.dll アセンブリが含まれますは推奨されていません。 System.Web.Mobile.dll アセンブリから型を使用する場合は、いずれかのルートにこのアセンブリへの参照を追加`Web.config`ファイルやアプリケーション`Web.config`ファイル。 たとえば、(非推奨) の ASP.NET モバイル コントロールのいずれかを使用する場合は、System.Web.Mobile.dll アセンブリへの参照を追加する必要があります、`Web.config`ファイル。

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>ASP.NET 要求の検証

Asp.net 要求の検証機能では、特定のレベルのクロス サイト スクリプティング (XSS) 攻撃に対する既定の保護を提供します。 ASP.NET の以前のバージョンでは、要求の検証は、既定で有効にされました。 ただし、ASP.NET ページにのみ適用 (`.aspx`ファイルと、クラス ファイル) とそれらのページが実行のみです。

ASP.NET 4 では、既定では、要求の検証が有効で、すべての要求の前に有効になっている、 **BeginRequest** HTTP 要求のフェーズ。 その結果、要求の検証は、.aspx ページの要求だけでなく、すべての ASP.NET リソースの要求に適用されます。 これには、Web サービスの呼び出しとカスタム HTTP ハンドラーなどの要求が含まれます。 要求の検証はカスタム HTTP モジュールは、HTTP 要求の内容を読み取るときにもアクティブです。

その結果、要求検証エラーがのリクエストの以前のエラーをトリガーしませんでした発生するようになりました。 ASP.NET 2.0 要求の検証機能の動作に戻すには、次の設定を追加、`Web.config`ファイル。

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

ただし、XSS 可能性がある HTTP 入力が安全でない攻撃ベクターで既存のハンドラー、モジュール、またはその他のカスタム コード アクセスする可能性があるかどうかを判断する要求検証エラーを分析することをお勧めします。

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>既定のハッシュ アルゴリズムが、HMACSHA256

ASP.NET では、暗号化とハッシュ アルゴリズムの両方を使用して、フォーム認証 Cookie やビュー ステートなどのデータをセキュリティで保護します。 既定では、ASP.NET 4 は cookie とビュー ステートのハッシュ操作のようになりましたデータの HMACSHA256 アルゴリズムを使用します。 ASP.NET の以前のバージョンでは、古い HMACSHA1 アルゴリズムを使用します。

混合 ASP.NET 2.0/ASP.NET 4 を実行する場合、アプリケーションが影響を受ける可能性環境フォーム認証 cookie などのデータが across.NET Framework のバージョンを使用する必要があります。 古い HMACSHA1 アルゴリズムを使用して ASP.NET 4 Web アプリケーションを構成するには、次の設定を追加、`Web.config`ファイル。

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>新しい ASP.NET 4 ルート構成に関連する構成エラー

ルートの構成ファイル (、`machine.config`ファイルとルート`Web.config`ファイル) の .NET Framework 4 (とそのため ASP.NET 4) を ASP.NET 3.5 で見つかった定型構成情報の大部分を含むように更新されました、アプリケーション`Web.config`ファイル。 管理対象の IIS 7 および IIS 7.5 構成システムは複雑であるため ASP.NET 4 および IIS 7 および IIS 7.5 の ASP.NET 3.5 アプリケーションを実行している可能性が ASP.NET または IIS のいずれかで構成エラー。

現実的な場合、Visual Studio 2010 でプロジェクト アップグレード ツールを使用して、ASP.NET 4 の ASP.NET 3.5 アプリケーションをアップグレードすることをお勧めします。 Visual Studio 2010 では、ASP.NET 3.5 アプリケーションの自動的に変更されます`Web.config`ASP.NET 4 用の適切な設定を格納するファイル。

ただし、これは再コンパイルしないで .NET Framework 4 を使用して ASP.NET 3.5 アプリケーションを実行するシナリオはサポートです。 その場合は、アプリケーションを手動で変更する必要があります`Web.config`ファイルと IIS 7 または IIS 7.5 の下で、.NET Framework 4 でのアプリケーションを実行する前にします。

次の 2 つのセクションでは、ソフトウェアの組み合わせにする必要がある変更について説明します。

**Windows Vista SP1 または Windows Server 2008 SP1 では、修正プログラム KB958854 も、SP2 がインストールされています。** この構成で、IIS 7 構成システム正しくマージしないアプリケーションの管理対象構成アプリケーション レベルを比較することによって`Web.config`ファイルを ASP.NET 2.0`machine.config`ファイル。 このため、アプリケーション レベルのため`Web.config`ファイルから、.NET Framework 3.5 以降が必要、 **system.web.extensions** IIS 7 の検証エラーが発生しないように構成セクションへの定義 (要素)。

ただし、アプリケーション レベルを手動で変更`Web.config`元定型の構成セクションの定義を Visual Studio 2008 で導入されたを正確に一致しないファイルのエントリには、ASP.NET の構成エラーが発生します。 (Visual Studio 2008 によって生成される既定の構成エントリが正しく動作します。)一般的な問題では、手動で変更される`Web.config`ファイルの除外、**仮想ディレクトリ**と**requirePermission**さまざまな構成セクションに含まれている構成属性定義です。 これにより、アプリケーション レベルで省略構成セクションの不一致`Web.config`ファイルと、ASP.NET 4 で完全な定義`machine.config`ファイル。 その結果、ASP.NET 4 の構成システムは、実行時に、構成エラーをスローします。

**Windows Vista SP2、Windows Server 2008 SP2、Windows 7、Windows Server 2008 R2、および Windows Vista SP1 もおよび KB958854 修正プログラムがインストールされている Windows Server 2008 SP1。**

テキスト比較を実行するため、このシナリオでは IIS 7 および IIS 7.5 ネイティブ構成システムが構成エラーを返します、**型**マネージ構成セクション ハンドラーが定義されている属性。 すべて`Web.config`Visual Studio 2008 と Visual Studio 2008 SP1 によって生成されるファイルが「3.5」の文字列型である、 **system.web.extensions** (および関連する) の構成セクション ハンドラー、ためと、ASP.NET4`machine.config`ファイル「4.0」を持つ、**型**属性を同じ構成セクション ハンドラー、Visual Studio 2008 または Visual Studio 2008 SP1 で常に生成されるアプリケーションの IIS 7 での構成の検証に失敗し、IIS 7.5 です。

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>これらの問題を解決します。

最初のシナリオの回避策は、アプリケーション レベルを更新する`Web.config`ファイルからの定型構成テキストを含めることによって、 `Web.config` Visual Studio 2008 によって自動的に生成されたファイル。

最初のシナリオ別の回避策が Vista または Windows Server 2008 Service Pack 2 をコンピューターにインストールまたは KB958854 修正プログラムをインストールするには ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) の不適切な構成マージ動作を修正する、IIS 構成システム。 ただし、これらのアクションのいずれかを実行した後、アプリケーションではこの 2 番目のシナリオで説明した問題のための構成エラーが発生可能性があります。

2 番目のシナリオの回避策は削除またはコメント アウトすべてを**system.web.extensions**構成セクションの定義および構成セクション グループの定義は、アプリケーション レベルから`Web.config`ファイル。 これらの定義は、通常、アプリケーション レベルの上部にある`Web.config`で識別できますファイルを開き、 **configSections**要素とその子。

どちらのシナリオは勧めも手動で削除する、 **system.codedom**セクションで、これは必要ありません。

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 つの子アプリケーション起動しないときに ASP.NET 2.0 または ASP.NET 3.5 アプリケーション

旧バージョンの ASP.NET を実行する子アプリケーションとして構成されている ASP.NET 4 アプリケーションは、構成エラーまたはコンパイル エラーにより起動しない場合があります。 次の例では、影響を受けるアプリケーションのディレクトリ構造を示します。

`/parentwebapp` (ASP.NET 2.0 または ASP.NET 3.5 を使用するように構成)  
`/childwebapp` (ASP.NET 4 を使用するように構成)

アプリケーションを`childwebapp`フォルダーは IIS 7 または IIS 7.5 で開始し、構成エラーをレポートには失敗します。 エラー テキストには、次のようなメッセージが含まれます。

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

IIS 6 では、アプリケーションで、`childwebapp`フォルダーも失敗を開始するには、別のエラーが報告されます。 たとえば、エラー テキストは、次の状態可能性があります。

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

これらのシナリオが発生するために、親アプリケーションから構成情報、`parentwebapp`フォルダーは、子 web によって使用される最終的なマージされた構成設定を決定する構成情報の階層の一部アプリケーションで、`childwebapp`フォルダー。 かどうか、ASP.NET 4 Web アプリケーションには、IIS 6 または IIS 7 (または IIS 7.5) が実行中、によって、IIS 構成システムまたは 4 の ASP.NET コンパイル システムはエラーを返します。

この問題を解決し、ASP.NET 4 アプリケーションが動作する子を取得するに必要な手順は、IIS 7 (または IIS 7.5) で、ASP.NET 4 アプリケーションが IIS 6 でまたはを実行するかどうかによって異なります。

### <a name="step-1-iis-7-or-iis-75-only"></a>手順 1 (IIS 7 または IIS 7.5 のみ)

この手順では、IIS 7 を実行するオペレーティング システムまたは Windows Vista、Windows Server 2008、Windows 7、および Windows Server 2008 R2 を含む IIS 7.5 でのみ必要があります。

移動、 **configSections**の定義、`Web.config`親アプリケーション (ASP.NET 2.0 または ASP.NET 3.5 を実行しているアプリケーション) のファイルをルートに`Web.config`の.net Framework 2.0 のファイル。 IIS 7 および IIS 7.5 ネイティブ構成システムのスキャン、 **configSections**構成ファイルの階層がマージされる際の要素。 移動、 **configSections**から親 Web アプリケーションの定義`Web.config`ルートにファイル`Web.config`ファイルに効果的に ASP.NET 4 の子に対して行われる構成マージ プロセスで、元の要素が非表示にアプリケーション。

または、32 ビット オペレーティング システムで 32 ビット アプリケーション プールの場合、ルート`Web.config`ASP.NET 2.0 および ASP.NET 3.5 用のファイルは、次のフォルダーに通常あります。

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

64 ビットのオペレーティング システムまたは 64 ビット アプリケーション プールの場合、ルート`Web.config`ASP.NET 2.0 および ASP.NET 3.5 用のファイルは、次のフォルダーに通常あります。

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

移動する必要があります、64 ビット コンピューターで 32 ビットと 64 ビットの両方の Web アプリケーションを実行する場合、 **configSections**要素をルートに`Web.config`32 ビットと 64 ビット システムの両方のファイル。

配置すると、 **configSections**ルート内の要素`Web.config`ファイルに貼り付けますセクションの直後に、**構成**要素。 次の例では、どのようなルートの上部`Web.config`ファイル、要素の移動が完了したらようになります。

> [!NOTE]
> 次の例では、行を読みやすくするため改行されています。


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>手順 2 (すべての IIS のバージョン)

この手順は、IIS 6 または IIS 7 (または IIS 7.5) で ASP.NET 4 子 Web アプリケーションが実行されているかどうか必要があります。

`Web.config`親 2 の ASP.NET または ASP.NET 3.5 で実行している Web アプリケーションのファイルを追加、**場所**(の構成システムの IIS と ASP.NET の両方) を明示的に指定するタグを構成エントリのみ親 Web アプリケーションに適用されます。 次の例の構文を示しています、**場所**要素を追加します。

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

次の例は方法、**場所**で始まるすべての構成セクションをラップするタグを使用して、 **appSettings**セクションから開始して、 **system.webServer**セクション。

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

手順 1. および 2. を完了すると、エラーのない子 ASP.NET 4 Web アプリケーションが開始されます。

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 Web サイトは、SharePoint がインストールされているコンピューターで起動に失敗します。

SharePoint を実行する web サーバーが、 `Web.config` SharePoint Web サイトのルートに配置されているファイル (たとえば、`c:\inetpub\wwwroot\web.config`既定 Web サイトに対して)。 この`Web.config`ファイル、SharePoint カスタム部分的に信頼レベル セットという名前の WSS\_最小限です。

この種類の SharePoint Web サイトの子として展開されている ASP.NET 4 Web サイトを実行しようとする場合は、次のエラーが表示されます。

`Could not find permission set named 'ASP.NET'.`

ASP.NET をという名前のアクセス許可セットの ASP.NET 4 コード アクセス セキュリティ (CAS) インフラストラクチャが見えるために、このエラーが発生します。 ただし、一部は WSS によって参照されている構成ファイルを信頼\_最小限はその名前を持つすべてのアクセス許可セット。

現在 ASP.NET と互換性がある SharePoint のバージョンはありません。 その結果、サイトを SharePoint Web の下に子サイトとして、ASP.NET 4 Web サイトを実行しない必要があります。

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>HttpRequest.FilePath プロパティが含まれなくなりました PathInfo 値

以前のバージョンの ASP.NET に含まれる、 **PathInfo**プロパティから返されたさまざまなファイル パスに関連するなどの値で値**HttpRequest.FilePath**、 **HttpRequest.AppRelativeCurrentExecutionFilePath**、および**HttpRequest.CurrentExecutionFilePath**します。 ASP.NET 4 が含まれなく、 **PathInfo**これらのプロパティからの戻り値の値。 代わりに、 **PathInfo**情報が表示されます。 **HttpRequest.PathInfo**します。 たとえば、次のような URL フラグメントがあるとします。

`/testapp/Action.mvc/SomeAction`

以前のバージョンの ASP.NET、 **HttpRequest**プロパティに次の値があります。

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (空)

ASP.NET 4 で**HttpRequest**プロパティは、次の値を代わりにあります。

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 アプリケーションで eurl.axd を参照する HttpException エラーを生成可能性があります

IIS 6 で ASP.NET 4 を有効にすると、(Windows Server 2003 または Windows Server 2003 R2 のいずれか) で IIS 6 で実行される ASP.NET 2.0 アプリケーションは、次のエラーを生成可能性があります。

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

さらに処理するために、ASP.NET 4 のネイティブなコンポーネントが ASP.NET のマネージ部分に拡張子なしの URL を渡します ASP.NET では、ASP.NET 4 を使用する Web サイトが構成されていることを検出するとこのエラーが発生します。 ただし、ASP.NET 2.0 を使用して、ASP.NET 4 Web サイトの下にある仮想ディレクトリを構成する場合は、この方法は"eurl.axd"文字列を含む URL を変更した結果で拡張子のない URL を処理しています。 この変更された URL は、ASP.NET 2.0 アプリケーションに送信されます。 ASP.NET 2.0 では、"eurl.axd"形式を認識できません。 ASP.NET 2.0 がという名前のファイルを検索しようとしたため、`eurl.axd`し実行します。 要求が失敗するこのようなファイルが存在しないので、 **HttpException**例外。

次のオプションのいずれかを使用してこの問題を回避できます。

### <a name="option-1"></a>オプション 1

ASP.NET 4 が Web サイトを実行するために必要ない場合は、代わりに ASP.NET 2.0 を使用するサイトに再マップします。

### <a name="option-2"></a>オプション 2

ASP.NET 4 が Web サイトを実行するために必要な場合は、その子の ASP.NET 2.0 の仮想ディレクトリを ASP.NET 2.0 にマップされている別の Web サイトに移動します。

### <a name="option-3"></a>オプション 3

場合は、Web サイトを ASP.NET 2.0 を再マップする、または仮想ディレクトリの場所を変更するには実用的ではない、拡張子のない URL を ASP.NET 4 での処理を明示的に無効にします。 次の手順に従います。

1. Windows レジストリ内で、次のノードを開きます。

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. 新規作成**DWORD**という名前の値**EnableExtensionlessUrls**します。
2. 設定**EnableExtensionlessUrls**を 0 にします。 これには、拡張子のない URL の動作が無効にします。
3. レジストリ値を保存して、レジストリ エディターを閉じます。
4. 実行、 **iisreset**コマンド ライン ツールは、iis に新しいレジストリ値を読み取れません。

> [!NOTE]
> 設定**EnableExtensionlessUrls**を 1 に拡張子のない URL の動作を有効にします。 これは、既定の設定値が指定されていない場合です。


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>イベント ハンドラーがいない発生しないで、既定のドキュメントでは、IIS 7 または IIS 7.5 統合モード

ASP.NET 4 に変更する変更が含まれています、**アクション**HTML の属性**フォーム**拡張子なしの URL は、既定のドキュメントに解決されると、要素が表示されます。 既定のドキュメントに解決する拡張子なしの URL の例があります[ http://contoso.com/](http://contoso.com/)への要求の結果として得られる、 [ http://contoso.com/Default.aspx](http://contoso.com/Default.aspx)します。

ASP.NET 4 がこれで HTML を表示します**フォーム**要素の**アクション**拡張子なしの url をマップされている既定のドキュメントを持つ要求が行われたときに、空の文字列として属性値。 などの ASP.NET 要求の以前のリリースで[ http://contoso.com ](http://contoso.com)への要求になる`Default.aspx`します。 そのドキュメントを開くに**フォーム**タグは、次の例のようにレンダリングされます。

`<form action="Default.aspx" />`

ASP.NET 4 では、要求で[ http://contoso.com ](http://contoso.com)への要求の結果も`Default.aspx`します。 ただし、ASP.NET は、HTML を開くようになりましたをレンダリング**フォーム**次の例のようにタグ。

`<form action="" />`

この方法の違い**アクション**属性をレンダリングする IIS と ASP.NET によってフォーム ポストを処理する方法で微妙な変更が発生することができます。 ときに、**アクション**属性が空の文字列で、IIS **DefaultDocumentModule**オブジェクトを作成する子要求`Default.aspx`します。 ほとんどの状況では、この子要求は、アプリケーション コードへの透過的な`Default.aspx`ページは通常どおり実行されます。

ただし、マネージド コードと IIS 7 または IIS 7.5 統合モードとの間のやり取りによって、マネージド .aspx ページが子要求中に正常に動作を停止することがあります。 かどうか、次の条件が発生するへの子要求を`Default.aspx`ドキュメントは、エラーまたは予期しない動作になります。

1. .Aspx ページがブラウザーに送信される、**フォーム**要素の**アクション**属性に設定""です。
2. フォームは、ASP.NET に再度送信されます。
3. マネージ HTTP モジュールは、エンティティ本体の一部を読み取ります。 たとえば、モジュールを読み込みます**Request.Form**または**Request.Params**します。 これにより、POST 要求のエンティティ本体がマネージド メモリに読み込まれます。 その結果、エンティティ本体は、IIS 7 または IIS 7.5 統合モードで実行されているネイティブ コード モジュールから使用できなくなります。
4. IIS **DefaultDocumentModule**オブジェクトは、最終的に実行されへの子要求を作成し、`Default.aspx`ドキュメント。 ただし、エンティティ本体はマネージド コードの一部によって既に読み取られているため、子要求に対する送信に使用できるエンティティ本体はありません。
5. HTTP パイプラインが子要求は、ハンドラーを実行すると`.aspx`されたフェーズ中にファイルを実行します。
6. エンティティ本体がないため、フォーム変数となしのビュー ステートがあるし、したがって情報はありません (ある場合) は、どのようなイベントが発生することは想定されて決定に .aspx ページ ハンドラー。 その結果、影響を受ける .aspx ページのポストバック イベント ハンドラーは実行されません。

次の方法でこの動作を回避することができます。

- 既定のドキュメントの要求中に、要求のエンティティ本体がアクセスしている HTTP モジュールを識別し、管理対象の要求に対してのみ実行する構成できるかどうかを判断します。 IIS 7 と IIS 7.5 統合モードでモジュールに、次の属性を追加して管理対象の要求に対してのみ実行する HTTP モジュールをマークできます**system.webServer/modules**エントリ。

- `precondition="managedHandler"`

- この設定を無効にについては、モジュールを要求しないように IIS 7 および IIS 7.5 を調べることでは、要求を管理します。 既定のドキュメントの要求には、最初の要求は、拡張子なしの url が。 そのため、IIS を実行しないとマークされているすべてのマネージ モジュール マネージ ハンドラーの前提条件と最初の要求の処理中にします。 その結果、マネージ モジュールは、エンティティ本体で誤って読み取りできませんにため、エンティティ本体は引き続き使用できますされ子要求を既定のドキュメントに渡されますが。

- 問題のある HTTP モジュールがすべての要求を実行する必要があるかどうか (拡張子のない Url に解決されるため、静的ファイル、 **DefaultDocumentModule**オブジェクトを管理対象の要求など)、によって影響を受ける .aspx ページを明示的に変更設定、**アクション**プロパティのページの**System.Web.UI.HtmlControls.HtmlForm**空でない文字列に制御します。 たとえば、既定のドキュメントが`Default.aspx`、明示的に設定するページのコードを変更、 **HtmlForm**コントロールの**アクション**プロパティを"Default.aspx"にします。

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>ASP.NET コード アクセス セキュリティ (CAS) の実装への変更

ASP.NET 2.0、3.5 では、追加された ASP.NET の機能拡張機能では、.NET Framework 1.1 および 2.0 コード アクセス セキュリティ (CAS) モデルの操作を使用しているとします。 ただし、ASP.NET 4 での CAS の実装は大幅に見直されました。 その結果、グローバル アセンブリ キャッシュ (GAC) で実行されている信頼されたコードに依存する部分信頼 ASP.NET アプリケーションは、さまざまなセキュリティの例外で失敗可能性があります。 マシンの CAS ポリシーに対する広範な変更に依存する部分信頼アプリケーションに可能性がありますもセキュリティ例外で失敗します。

ASP.NET 1.1 および 2.0 の新しいの動作に部分的に信頼された ASP.NET 4 アプリケーションを元に戻すことができます**legacyCasModel**属性、**信頼**次の例に示すように構成要素:

`<trust level= "Medium" legacyCasModel="true" />`

レガシ CAS モデルに戻すには、次の古い CA の動作は有効になります。

- マシンの CAS ポリシーが受け入れられます。
- 1 つのアプリケーション ドメイン内の複数の異なるアクセス許可セットが許可されます。
- 明示的なアクセス許可のアサーションが ASP.NET またはその他の .NET Framework コードのみが、スタックのときに呼び出される、GAC でアセンブリが必要ではありません。

1 つのシナリオを .NET Framework 4 で元に戻すことはできません。 非 Web 部分信頼アプリケーションでは、System.Web.dll および System.Web.Extensions.dll で特定の Api を呼び出すことができなくします。 .NET Framework の以前のバージョンでは非 Web 部分的に信頼されたアプリケーションは明示的に許可する<strong>AspNetHostingPermission</strong>アクセス許可。 これらのアプリケーションを使用して<strong>System.Web.HttpUtility</strong>、型、 <strong>System.Web.ClientServices\< 。/strong > *、名前空間と型は、メンバーシップ、ロール、およびプロファイルに関連します。 非 Web 部分信頼のアプリケーションからこれらの型の呼び出しは、.NET Framework 4 ではサポートされていません。

> [!NOTE]
> **HtmlEncode**と**HtmlDecode**の機能、 **System.Web.HttpUtility**クラスは、新しい .NET Framework 4 に移動された**System.Net.WebUtility**クラス。 使用されている唯一の ASP.NET 機能する場合は、変更、新しいアプリケーションのコード**WebUtility**クラスの代わりにします。


ASP.NET 4 の既定の CA の実装への変更の概要を次に示します。

- ASP.NET アプリケーション ドメインは、同種のアプリケーション ドメインで、ようになりました。 部分信頼および完全信頼の許可セットだけは、アプリケーション ドメインで使用できます。
- ASP.NET の部分的な信頼の許可セットは、エンタープライズ レベル、コンピューター レベルまたはユーザー レベルの CAS ポリシーから依存しません。
- 3.5 および 3.5 SP1 に付属する ASP.NET アセンブリが .NET Framework 4 の透過性モデルを使用する変換されました。
- ASP.NET の使用**AspNetHostingPermission**属性が大幅に削減されました。 ASP.NET のパブリック Api からこの属性のほとんどのインスタンスがなくなりました。
- 透明色としてアセンブリを明示的にマークする、ASP.NET ビルド プロバイダーによって作成される動的にコンパイルされたアセンブリが更新されました。
- ASP.NET のすべてのアセンブリは APTCA 属性は、Web ホスティング環境でのみ受け入れられます、このような方法でマークされているようになりました。 ClickOnce などの部分的に信頼された以外の Web ホスティング環境は ASP.NET のアセンブリを呼び出すことができません。

新しい ASP.NET 4 コード アクセス セキュリティ モデルの詳細については、次を参照してください。 [ASP.NET アプリケーションを使用したコード アクセス セキュリティを使用して](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx)MSDN Web サイト。

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser および System.Web.Security Namespace で他の型が移動されました

ASP.NET メンバーシップで使用されるいくつかの型から移動された`System.Web.dll`新しい System.Web.ApplicationServices.dll アセンブリにします。 これらのタイプは、クライアントのタイプと拡張 .NET Framework SKU のタイプとの間のアーキテクチャ層の依存関係を解決するために移動しました。

Web サイト プロジェクトは、system.web.applicationservices.dll へは、ASP.NET コンパイル システムによって既定で使用される参照アセンブリの一覧に追加されたため、これらの型を移動した結果としての問題がありません。 Visual Studio 2010 で開き、ASP.NET から ASP.NET 4 の以前のバージョンを使用して作成された Web サイト プロジェクトをアップグレードする場合、プロジェクトはエラーなしでコンパイルします。

同様に、Visual Studio 2010 で開くことによって、以前のバージョンの ASP.NET から ASP.NET 4 で作成された Web アプリケーション プロジェクトをアップグレードする場合、アップグレード プロセスは System.Web.ApplicationServices.dll への参照をプロジェクトに追加します。 そのため、Web アプリケーション プロジェクトがエラーなしでコンパイルもアップグレードされます。

メンバーシップの種類は、別のアセンブリに移動された場合でも、旧バージョンの ASP.NET を使用して作成されたコンパイル済みの (バイナリ) ファイルもエラーのない「ASP.NET 4 で実行します。 ASP.NET 4 のバージョンの情報の転送の種類が追加されました`System.Web.dll`を自動的にこれらの型の実行時の参照を型の新しい場所にルーティングします。

ただし、特定のメンバーシップの種類を使用して、ASP.NET の以前のバージョンからアップグレードされたクラス ライブラリは、ASP.NET 4 プロジェクトで使用すると、コンパイルに失敗します。 たとえば、クラス ライブラリ プロジェクトが失敗するをコンパイルして、次のようなエラーを報告します。

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

この問題は、クラス ライブラリ プロジェクトで System.Web.ApplicationServices.dll への参照を追加することで回避できます。

次の一覧表示されます、 *System.Web.Security*から移動された型`System.Web.dll`System.Web.ApplicationServices.dll へ。

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

## <a name="output-caching-changes-to-vary--http-header"></a>出力キャッシュの変更を変えるに\*HTTP ヘッダー

ASP.NET 1.0 では、バグが生じることが指定されているページをキャッシュ`Location="ServerAndClient"`を出力する出力 – キャッシュ設定として、`Vary:*`応答の HTTP ヘッダー。 その結果、クライアント ブラウザーに対して、ローカルにページをキャッシュしないように指示がなされていました。

ASP.NET 1.1 で、 **System.Web.HttpCachePolicy.SetOmitVaryStar**メソッドが追加された非表示を呼び出すことができる`Vary:*`ヘッダー。 このメソッドは、重大な可能性があると見なされた出力の HTTP ヘッダーを変更するために選択したが、いつでも変更します。 ただし、開発者が ASP.NET では、動作が混乱し、バグの報告は、開発者が既存の対応していないことをお勧めします**SetOmitVaryStar**動作します。

ASP.NET 4 で、意思決定根本的な問題を修正しました。 `Vary:*` HTTP ヘッダーは、次のディレクティブを指定する応答からは生成されなく。

`<%@OutputCache Location="ServerAndClient" %>`

その結果、 **SetOmitVaryStar**を抑制するには不要になった、`Vary:*`ヘッダー。

指定されたアプリケーションで`Location="ServerAndClient"`で、 **@ OutputCache**ページ ディレクティブが表示されます、動作の名前が含まれる、**場所**属性の値は、ページが表示されます呼び出すことが必要とせず、ブラウザーでキャッシュ可能な**SetOmitVaryStar**メソッド。

場合は、アプリケーション内のページを生成する必要があります`Vary:*`を呼び出し、 **AppendHeader**次の例のように、メソッド。

`HttpResponse.AppendHeader("Vary","*");`

出力キャッシュの値を変更する代わりに、**場所**属性を"Server"にします。

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Passport for System.Web.Security タイプは廃止

ASP.NET 2.0 に組み込まれた Passport のサポートが廃止され、サポートされていない Passport (今すぐ LiveID) が変更されたのため数年。 その結果、5 種類 Passport に関連する**System.Web.Security**でマークしている、 **ObsoleteAttribute**属性。

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>ASP.NET 4 でイメージをレンダリングする MenuItem.PopOutImageUrl プロパティが失敗します。

ASP.NET 3.5 で、 *MenuItem.PopOutImageUrl*プロパティでは、メニュー項目に動的なサブメニューがあることを示すにメニュー項目に表示されるイメージの URL を指定することができます。 次の例では、ASP.NET 3.5 のマークアップでこのプロパティを指定する方法を示します。

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

ASP.NET 4 でのデザイン変更の結果の出力をレンダリングなしの*PopOutImageUrl*のプロパティが設定されている場合、 *MenuItem*クラス。 代わりに、直接、画像の URL を指定する必要があります、*メニュー*いずれかを使用して制御、 *StaticPopOutImageUrl*プロパティまたは*DynamicPopOutImageUrl*プロパティ。 静的メニューの場合は、使用する場合、 *Menu.StaticPopOutImageUrl*プロパティは、次の例で示すように、静的メニュー項目にサブメニューの場合を示すために表示されるイメージの URL を指定します。

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

使用する動的メニューを使用する場合、 *Menu.DynamicPopOutImageUrl*動的メニュー項目にサブメニューがあるかを示すイメージの URL を指定するプロパティ。 次の例は前に似ていますが、設定する方法を示しています、 *DynamicPopOutImageUrl*動的メニューのプロパティ。

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

場合、 *Menu.DynamicPopOutImageUrl*プロパティが設定されていないと、 *Menu.DynamicEnableDefaultPopOutImage*プロパティに設定されて*false*イメージは表示されません。 同様に場合、 *StaticPopOutImageUrl*プロパティが設定されていないと、 *StaticEnableDefaultPopOutImage*プロパティに設定されて*false*イメージは表示されません。

これらのプロパティのパスを設定すると、円記号の代わりにスラッシュ (/) を使用して、(\)します。 詳細については、次を参照してください。 [Menu.StaticPopOutImageUrl とレンダリング イメージとパスを含む円記号を Menu.DynamicPopOutImageUrl Fail](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu します。") 他の場所でこのドキュメントでは。

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl と Menu.DynamicPopOutImageUrl パスに円記号が含まれている場合は、イメージを表示するために失敗します。

ASP.NET 4 を使用して、指定したイメージで、 *Menu.StaticPopOutImageUrl*と*Menu.DynamicPopOutImageUrl* backlashes がパスに含まれている場合、プロパティは表示されません (\)します。 これは、ASP.NET の以前のバージョンからの変更点です。

次の例の*メニュー*マークアップの表示を制御、 *StaticPopOutImageUrl*プロパティ バック スラッシュを含むパスを使用して設定します。 ASP.NET 4 では、プロパティで指定されたイメージはレンダリングされません。

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

この問題を修正するで指定されているパスの値を変更、 *StaticPopOutImageUrl*と*DynamicPopOutImageUrl*スラッシュ (/) を使用するプロパティ。 次の例は、この変更を示しています。

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

ASP.NET から ASP.NET 4 の以前のバージョンから移行されたアプリケーションもありますに注意してください、影響を受けるため、 *MenuItem.PopOutImageUrl*プロパティが変更されました。 詳細については、次を参照してください。 [、MenuItem.PopOutImageUrl プロパティは失敗 ASP.NET 4 でイメージをレンダリングする](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert")このドキュメントで別の場所。

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>免責情報

このドキュメントは暫定版であり、ここで説明するソフトウェアの最終的な商業リリースより前に大幅に変更される可能性があります。

このドキュメントに記載されている情報は、説明されている問題に関する Microsoft Corporation の発行日時点における最新の見解を表します。 マイクロソフトは変化する市場の状況に対応する必要があるため、本ドキュメントをマイクロソフトの確約と解釈することはできず、発行日以降は、提示されている情報の正確性を保証できません。

このホワイト ペーパーは、情報提供のみを目的としています。 マイクロソフトは、このドキュメント内の情報に関して、明示的、暗黙的、または法的ないかなる保証も行いません。

お客様ご自身の責任において、適用されるすべての著作権関連法規に従ったご使用を願います。 このドキュメントのいかなる部分も、米国 Microsoft Corporation の書面による許諾を受けることなく、その目的を問わず、どのような形態であっても、複製または譲渡することは禁じられています。ここでいう形態とは、複写や記録など、電子的な、または物理的なすべての手段を含みます。ただしこれは、著作権法上のお客様の権利を制限するものではありません。

マイクロソフトは、このドキュメントに記載されている内容に関し、特許、特許申請、商標、著作権、またはその他の無体財産権を有する場合があります。 別途マイクロソフトのライセンス契約上に明示の規定のない限り、このドキュメントはこれらの特許、商標、著作権、またはその他の無体財産権に関する権利をお客様に許諾するものではありません。

明記されない限り、会社、組織、製品、ドメイン名、電子メール アドレス、ロゴ、人物、場所、イベント実在架空のものと関連会社、組織、製品、ドメイン名、電子メールをアドレス、ロゴ、人物、場所、またはイベントは、または推論する必要があります。

© 2010 Microsoft Corporation. All rights reserved.

Microsoft および Windows は、米国および他の国における Microsoft Corporation の登録商標または商標です。

本ドキュメントで説明する実際の製造元および製品名は、それぞれの所有者の商標である場合があります。
