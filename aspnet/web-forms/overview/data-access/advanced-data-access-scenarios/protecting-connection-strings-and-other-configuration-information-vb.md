---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: 接続文字列とその他の構成情報 (VB) を保護する |Microsoft Docs
author: rick-anderson
description: ASP.NET アプリケーションは、通常、構成情報を Web.config ファイルに格納します。 この情報の一部は小文字が区別し、保護を保証します。 によって定義.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 83be60143ceaaf074e54ae25721cebdf8c375261
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363262"
---
<a name="protecting-connection-strings-and-other-configuration-information-vb"></a>接続文字列とその他の構成情報 (VB) を保護します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip)または[PDF のダウンロード](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> ASP.NET アプリケーションは、通常、構成情報を Web.config ファイルに格納します。 この情報の一部は小文字が区別し、保護を保証します。 既定でこのファイルは発生しません Web サイトの訪問者が、管理者や、ハッカー可能性があります Web サーバーのファイル システムにアクセスし、ファイルの内容を表示します。 このチュートリアルでは ASP.NET 2.0 では、Web.config ファイルのセクションを暗号化して機密情報を保護することを説明します。


## <a name="introduction"></a>はじめに

ASP.NET アプリケーションの構成情報がよくという XML ファイルに格納されている`Web.config`します。 これらのチュートリアルの過程で更新した、`Web.config`いくつかの時間。 作成するときに、`Northwind`で型指定されたデータセット、[最初のチュートリアル](../introduction/creating-a-data-access-layer-vb.md)、たとえば、接続文字列情報を自動的に追加されたに`Web.config`で、`<connectionStrings>`セクション。 後の「、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-vb.md)チュートリアルでは、手動で更新しました`Web.config`を追加する、`<pages>`要素は、すべてのプロジェクト内の ASP.NET ページを使用することを示す、`DataWebControls`テーマ。

`Web.config` 、接続文字列などの機密データを含めることができますが重要ですがの内容`Web.config`安全で承認されていないビューアーから非表示を保持します。 既定では、任意の HTTP の要求を持つファイルに、`.config`拡張機能は、返す ASP.NET エンジンによって処理されます、*この種類のページが提供されない*図 1 に表示されるメッセージ。 つまり、訪問者を表示できない、 `Web.config` s の内容を入力するだけでファイルhttp://www.YourServer.com/Web.configブラウザーのアドレス バーにします。


[![Web.config で、ブラウザーを返します。 ページのこの型にアクセスして、メッセージは処理されません。](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**図 1**: 訪問`Web.config`を通じて、ブラウザーを返しますページの種類、このメッセージは提供されませんが ([フルサイズの画像を表示する をクリックします。](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))。


しかし、攻撃者を使用して表示するその他のいくつかの悪用を検索することができない場合、`Web.config`ファイルの内容を s でしょうか。 攻撃者何がこの情報を使用し、内の機密情報の保護を強化する手順を実行できる`Web.config`でしょうか。 ほとんどのセクションでさいわい、`Web.config`機密情報は含まれません。 ASP.NET ページによって使用されるテーマの既定の名前がわかっている場合に、攻撃者が利用するどのような損害ことができますか。

特定`Web.config`セクションでには、ただし、接続文字列、ユーザー名、パスワード、サーバー名、暗号化キー、およびなどを含めることがある機密情報が含まれています。 この情報は、次でよく見られる`Web.config`セクション。

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

このチュートリアルでは、このような機密性の高い構成情報を保護するための手法に注目します。 、わかるとおり、.NET Framework version 2.0 を簡単に選択した構成セクションではプログラムで暗号化と復号化できる保護された構成システムが含まれています。

> [!NOTE]
> このチュートリアルの最後に、ASP.NET アプリケーションからデータベースに接続するための Microsoft s の推奨事項を参照してください。 接続文字列を暗号化するには、安全な方法でデータベースに接続していることを確認して、システムを強化することができます。


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>手順 1: ASP.NET 2.0 の探索の保護の構成オプション

ASP.NET 2.0 には、構成情報を暗号化および暗号化の保護された構成システムが含まれています。 これには、プログラムでの暗号化または構成情報を復号化に使用できる .NET Framework にはメソッドが含まれます。 保護された構成システムを使用して、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)開発者はどのような暗号化実装の使用を選択することができます。

.NET Framework では、2 つの保護構成プロバイダーが付属しています。

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -は、非対称[RSA アルゴリズム](http://en.wikipedia.org/wiki/Rsa)暗号化と復号化します。
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) Windows を使用して[データ保護 API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx)暗号化と復号化します。

保護された構成システムは、プロバイダーの設計パターンを実装するためを独自の保護構成プロバイダーを作成し、アプリケーションに組み込むことです。 参照してください[保護構成プロバイダーを実装する](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx)このプロセスの詳細についてはします。

RSA および DPAPI プロバイダー キーを暗号化と復号化ルーチンを使用して、コンピューター、またはユーザー レベルでこれらのキーを格納することができます。 マシン レベル キーは、独自の専用サーバーでは、web アプリケーションが実行されているシナリオに最適ですか、共有する必要があるサーバーで複数のアプリケーションがあるかどうかは、情報を暗号化します。 ユーザー レベルのキーは、場所、同じサーバー上の他のアプリケーションで必要があります、アプリケーション s 保護の構成セクションを復号化できません。 共有のホスト環境でより安全なオプションです。

このチュートリアルでは、DPAPI プロバイダーとマシン レベル キーには、例を使用します。 具体的には、暗号化するに注目するは、`<connectionStrings>`セクション`Web.config`、保護された構成システムは、ほとんどすべての暗号化に使用できますが、`Web.config`セクション。 ユーザー レベルのキーを使用する RSA プロバイダーを使用してでは、このチュートリアルの最後に、それ以上の測定値のセクションでリソースを参照してください。

> [!NOTE]
> `RSAProtectedConfigurationProvider`と`DPAPIProtectedConfigurationProvider`にプロバイダーが登録されている、`machine.config`プロバイダーの名前を含むファイル`RsaProtectedConfigurationProvider`と`DataProtectionConfigurationProvider`、それぞれします。 暗号化または適切なプロバイダー名を指定する必要があります。 構成情報を暗号化解除 (`RsaProtectedConfigurationProvider`または`DataProtectionConfigurationProvider`) 実際の型名ではなく (`RSAProtectedConfigurationProvider`と`DPAPIProtectedConfigurationProvider`)。 検索することができます、`machine.config`ファイル、`$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`フォルダー。


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>手順 2: をプログラムで暗号化し、構成セクションを復号化

数行のコードでは、暗号化または指定したプロバイダーを使用して特定の構成セクションを復号化できます。 コードでわかるとおり、だけで済みますをプログラムで、適切な構成セクションを参照を呼び出すその`ProtectSection`または`UnprotectSection`メソッド、および、呼び出し、`Save`メソッドの変更を保持します。 さらに、.NET Framework には、暗号化および構成情報を復号化できる便利なコマンド ライン ユーティリティが含まれています。 手順 3 では、このコマンド ライン ユーティリティを紹介します。

プログラムによる保護の構成情報を示すためには、let s は暗号化および復号化するボタンを含む ASP.NET ページを作成、`<connectionStrings>`セクション`Web.config`します。

開いて開始、`EncryptingConfigSections.aspx`ページで、`AdvancedDAL`フォルダー。 TextBox コントロールをドラッグして、デザイナーの設定には、ツールボックスからその`ID`プロパティを`WebConfigContents`その`TextMode`プロパティを`MultiLine`とその`Width`と`Rows`95%、15、プロパティそれぞれします。 このテキスト ボックス コントロールの内容が表示されます`Web.config`かどうか、コンテンツは暗号化されているかをすばやく確認することができます。 もちろん、実際のアプリケーションではしないの内容を表示する`Web.config`します。

テキスト ボックスに、下にある、という名前の 2 つのボタン コントロールを追加`EncryptConnStrings`と`DecryptConnStrings`します。 接続文字列の暗号化および復号化の接続文字列にそのテキスト プロパティを設定します。

この時点で、画面は図 2 のようなはずです。


[![テキスト ボックスと 2 つのボタンの Web コントロールをページに追加します。](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**図 2**: テキスト ボックスと 2 つのボタンの Web コントロールをページに追加 ([フルサイズの画像を表示する をクリックします](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))。


次に、読み込んでの内容を表示するコードを記述する必要があります`Web.config`で、`WebConfigContents`テキスト ボックスに、ページが最初に読み込まれます。 ページの分離コード クラスには、次のコードを追加します。 このコードでは、という名前のメソッドを追加します`DisplayWebConfig`からそれを呼び出すと、`Page_Load`イベント ハンドラーと`Page.IsPostBack`は`False`:。


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

`DisplayWebConfig`メソッドは、 [ `File`クラス](https://msdn.microsoft.com/library/system.io.file.aspx)にアプリケーションを開きます`Web.config`ファイル、 [ `StreamReader`クラス](https://msdn.microsoft.com/library/system.io.streamreader.aspx)文字列であり、にその内容を読み取る[`Path`クラス](https://msdn.microsoft.com/library/system.io.path.aspx)への物理パスを生成する、`Web.config`ファイル。 これら 3 つのクラスは、すべてで、 [ `System.IO`名前空間](https://msdn.microsoft.com/library/system.io.aspx)します。 その結果、追加する必要がありますには、`Imports``System.IO`分離コード クラスか、または、これらのクラス名をプレフィックスの先頭にステートメント `System.IO.`

次に、2 つのボタン コントロールのイベント ハンドラーを追加する必要があります`Click`イベントの暗号化し、復号化に必要なコードを追加し、`<connectionStrings>`セクションでは、DPAPI プロバイダー マシン レベル キーを使用します。 デザイナーでは、各追加ボタンをダブルクリックして、`Click`分離コード内のイベント ハンドラー クラスを次のコードを追加。


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

2 つのイベント ハンドラーで使用するコードとほぼ同じです。 現在のアプリケーションの情報を取得することで開始するには両方`Web.config`ファイルを使用して、 [ `WebConfigurationManager`クラス](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx)s [ `OpenWebConfiguration`メソッド](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx)します。 このメソッドは、指定された仮想パスの web 構成ファイルを返します。 次に、`Web.config`ファイル %s`<connectionStrings>`セクションにはによってアクセス、 [ `Configuration`クラス](https://msdn.microsoft.com/library/system.configuration.configuration.aspx)s [ `GetSection(sectionName)`メソッド](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx)、返された、 [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx)オブジェクト。

`ConfigurationSection`オブジェクトが含まれています、 [ `SectionInformation`プロパティ](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx)追加情報および構成セクションに関する機能を提供します。 上のコードとを確認して構成セクションを暗号化するかどうかを決定できます、`SectionInformation`プロパティの`IsProtected`プロパティ。 セクションを暗号化またはを使用して復号化、さらに、`SectionInformation`プロパティ %s`ProtectSection(provider)`と`UnprotectSection`メソッド。

`ProtectSection(provider)`メソッドは暗号化に使用する保護された構成プロバイダーの名前を指定する文字列を入力として受け取ります。 `EncryptConnString`ボタン s のイベント ハンドラーに DataProtectionConfigurationProvider を渡して、`ProtectSection(provider)`メソッド DPAPI プロバイダーが使用されるようにします。 `UnprotectSection`メソッドはプロバイダー構成セクションの暗号化に使用されたために必要としないある入力パラメーターを確認できます。

呼び出した後、`ProtectSection(provider)`または`UnprotectSection`メソッドを呼び出す必要があります、`Configuration`オブジェクト[`Save`メソッド](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx)変更を確定します。 呼び出して、変更を保存し、構成情報が暗号化または復号化された`DisplayWebConfig`を読み込む、更新された`Web.config`TextBox コントロールに内容。

上記のコードを入力するとテストを参照してください、`EncryptingConfigSections.aspx`ページがブラウザーを使用します。 内容を一覧表示されたページを表示する必要があります最初に`Web.config`で、`<connectionStrings>`セクションがプレーン テキストで表示されます (図 3 を参照してください)。


[![テキスト ボックスと 2 つのボタンの Web コントロールをページに追加します。](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**図 3**: テキスト ボックスと 2 つのボタンの Web コントロールをページに追加 ([フルサイズの画像を表示する をクリックします](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))。


接続文字列の暗号化 ボタンをクリックします。 返されたマークアップが投稿された要求の検証が有効になっている場合、 `WebConfigContents` TextBox が生成されます、 `HttpRequestValidationException`、可能性のある危険なメッセージを表示する`Request.Form`クライアントからの値が検出されました。 要求の検証は、ASP.NET 2.0 では既定で有効に、エンコードされていない HTML を含むポストバックとは、スクリプト インジェクション攻撃を防ぐために設計されています。 このチェックは、ページ、またはアプリケーション レベルでを無効にすることができます。 このページを無効に、設定、`ValidateRequest`設定`False`で、`@Page`ディレクティブ。 `@Page`ディレクティブがページの宣言型マークアップの上部にあるが見つかりました。


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

要求の検証は、目的の詳細については方法で、ページ レベルとアプリケーション レベル、方法についても HTML マークアップをエンコードと無効にするを参照してください[要求の検証 - スクリプト攻撃の防止](../../../../whitepapers/request-validation.md)します。

ページの要求の検証を無効にした後、接続文字列の暗号化 ボタンをクリックすると、もう一度お試しください。 構成ファイルのアクセス、ポストバックのおよびその`<connectionStrings>`セクション DPAPI プロバイダーを使用して暗号化します。 テキスト ボックスが、新しい表示を更新し`Web.config`コンテンツ。 図 4 に示すよう、`<connectionStrings>`情報が暗号化されています。


[![クリックすると、暗号化接続文字列 ボタンは暗号化、 &lt;connectionString&gt;セクション](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**図 4**: クリックすると、暗号化接続文字列 ボタンは暗号化、`<connectionString>`セクション ([フルサイズの画像を表示する をクリックします](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))。


暗号化された`<connectionStrings>`でコンテンツの一部ですが、自分のコンピューターで生成されたセクションの次に、`<CipherData>`簡潔にするための要素が削除されました。


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>`要素の暗号化を実行するために使用するプロバイダーを指定します (`DataProtectionConfigurationProvider`)。 この情報によって使用されます、`UnprotectSection`メソッド、接続文字列の暗号化を解除 ボタンがクリックされたとき。


接続文字列の情報にアクセスするときに`Web.config`- かによって、コードの記述、SqlDataSource コントロールから、または、型指定されたデータセットを Tableadapter から自動生成されたコード - は、自動的に暗号化解除します。 つまり余分なコードまたは暗号化、復号化するためのロジックを追加する必要はありません`<connectionString>`セクション。 これを示すために、基本レポートのセクションから単純な表示チュートリアルなど、この時点でアクセス前のチュートリアルのいずれか (`~/BasicReporting/SimpleDisplay.aspx`)。 図 5 に示すよう、チュートリアルはとってい、ASP.NET ページで、暗号化された接続文字列情報の自動的に暗号化解除することを示すとおりに動作します。


[![データ アクセス層には、接続文字列情報を自動的に復号化します](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**図 5**: データ アクセス層には、接続文字列情報を自動的に復号化します ([フルサイズの画像を表示する をクリックします](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))。


元に戻す、`<connectionStrings>`戻るをプレーン テキスト形式のセクションで、接続文字列の暗号化を解除 ボタンをクリックします。 ポストバックの内の接続文字列を表示する必要があります`Web.config`プレーン テキストでします。 この時点で、画面に最初にこのページ (図 3 参照) にアクセスしたときと同じようになります。

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>手順 3: を使用して構成セクションを暗号化します。`aspnet_regiis.exe`

.NET Framework には、さまざまなコマンド ライン ツールが含まれています、`$WINDOWS$\Microsoft.NET\Framework\version\`フォルダー。 [を使用して SQL キャッシュ依存関係](../caching-data/using-sql-cache-dependencies-vb.md)チュートリアルでは、たとえば、ところを使用して、 `aspnet_regsql.exe` SQL キャッシュ依存関係のために必要なインフラストラクチャを追加するコマンド ライン ツール。 このフォルダー内の別の便利なコマンド ライン ツールは、 [ASP.NET IIS 登録ツール (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx)します。 その名前が示すように、ASP.NET IIS 登録ツールは Microsoft の専門性の高い Web サーバー、IIS と ASP.NET 2.0 アプリケーションを登録する主に使用します。 IIS に関連する機能だけでなく、ASP.NET IIS 登録ツールこともできますを暗号化またはで指定した構成セクションを復号化する`Web.config`します。

次のステートメントで構成セクションを暗号化するために使用する一般的な構文を示しています、`aspnet_regiis.exe`コマンド ライン ツール。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*セクション*構成セクション (connectionStrings) などの暗号化には、*物理\_ディレクトリ*、web アプリケーションのルート ディレクトリへの完全な物理パスと*プロバイダー* (DataProtectionConfigurationProvider) などを使用する保護された構成プロバイダーの名前を指定します。 または、web アプリケーションが IIS に登録されている場合は、次の構文を使用して物理パスではなく仮想パスを入力できます。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

次`aspnet_regiis.exe`例暗号化、`<connectionStrings>`マシン レベル キーで DPAPI プロバイダーを使用してセクションします。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

同様に、`aspnet_regiis.exe`構成セクションを復号化するコマンド ライン ツールを使用できます。 使用する代わりに、`-pef`スイッチを使用して`-pdf`(またはその代わりに`-pe`を使用して、 `-pd`)。 また、復号化するときに、プロバイダー名は必要ないことに注意してください。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> 実行する必要がありますコンピューターに固有のキーを使用して DPAPI プロバイダーを使用しているため`aspnet_regiis.exe`元となる web ページの処理が同じコンピューターから。 たとえば、実稼働サーバーに暗号化された Web.config ファイルをアップロードして、ローカル開発用コンピューターからこのコマンド ライン プログラムを実行する場合、実稼働サーバーできなくが暗号化の後に、接続文字列情報を復号化するには開発用コンピューターに固有のキーを使用します。 RSA プロバイダーは別のコンピューターに RSA キーをエクスポートすることは、この制限がありません。


## <a name="understanding-database-authentication-options"></a>データベース認証オプションの理解

任意のアプリケーションを発行する前に`SELECT`、 `INSERT`、 `UPDATE`、または`DELETE`Microsoft SQL Server データベースに対するクエリは最初に、要求元を識別する必要があります。 このプロセスと呼ばれる*認証*し SQL Server 認証の 2 つのメソッドを提供します。

- **Windows 認証**-アプリケーションが実行されているプロセスは、データベースとの通信に使用します。 Visual Studio 2005 の ASP.NET 開発サーバーを使用して ASP.NET アプリケーションを実行するときに、ASP.NET アプリケーションは、現在ログオンしているユーザーの id を想定しています。 ASP.NET アプリケーションで Microsoft インターネット インフォメーション サーバー (IIS) で ASP.NET アプリケーションは、通常のある id`domainName``\MachineName`または`domainName``\NETWORK SERVICE`、これをカスタマイズできます。
- **SQL 認証**-ユーザーの ID とパスワードの値は、認証の資格情報として指定されます。 SQL 認証では、ユーザー ID とパスワードは、接続文字列で提供されます。

Windows 認証はより安全なであるために、SQL 認証をお勧めします。 Windows 認証を使用した接続文字列は、ユーザー名とパスワードから解放して、web サーバーとデータベース サーバー 2 台のコンピューターに存在する場合、資格情報は、プレーン テキストでネットワーク経由では送信されません。 SQL 認証では、ただし、認証資格情報は、接続文字列でハードコーディングされて、プレーン テキストで、データベース サーバーに web サーバーから送信されます。

これらのチュートリアルでは、Windows 認証を使用しています。 認証モードは、接続文字列の検査で使用されていることがわかります。 内の接続文字列`Web.config`チュートリアル務めています。

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Integrated Security = True とユーザー名とパスワードの欠如は、Windows 認証が使用されていることを示します。 用語の信頼された接続文字列をいくつかの接続で = [はい] または統合セキュリティ = SSPI が統合セキュリティの代わりに使用される = true の場合、3 つすべての Windows 認証の使用が。

次の例では、SQL 認証を使用する接続文字列を示します。 接続文字列内に埋め込まれた資格情報に注意してください。

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

攻撃者が、アプリケーションの s を表示できることを想像してみてください`Web.config`ファイル。 SQL 認証を使用して、インターネット経由でアクセス可能なデータベースに接続する場合、攻撃者は、SQL Management studio または独自の web サイトの ASP.NET ページから、データベースに接続するこの接続文字列を使用できます。 この脅威を軽減するでの接続文字列情報を暗号化`Web.config`保護された構成システムを使用します。

> [!NOTE]
> SQL Server で利用可能な認証のさまざまな種類の詳細については、次を参照してください。 [Building Secure ASP.NET Applications: 認証、承認、およびセキュリティ保護された通信](https://msdn.microsoft.com/library/aa302392.aspx)します。 さらに接続文字列例については、Windows と SQL 認証の構文の違いを示すを参照してください[ConnectionStrings.com](http://www.connectionstrings.com/)します。


## <a name="summary"></a>まとめ

既定では、ファイルを`.config`ASP.NET アプリケーションの拡張機能は、ブラウザーからアクセスできません。 データベース接続文字列、ユーザー名やパスワードなどの機密情報が含まれてし、可能性がありますがあるために、この種のファイルは返されません。 .NET 2.0 で保護された構成システムは、さらに指定した構成セクションを暗号化するようにすることで機密情報を保護します。 2 つの組み込みの保護された構成プロバイダーがあります。 1 つは、RSA アルゴリズムと、Windows データ保護 API (DPAPI) を使用する 1 つを使用します。

このチュートリアルでは、暗号化し、DPAPI プロバイダーを使用して構成設定の暗号化を解除する方法について説明しました。 これには、手順 2. で説明したように、プログラムによって、およびによって、`aspnet_regiis.exe`コマンド ライン ツールは、手順 3 で説明されています。 ユーザー レベルのキーを使用してまたは RSA プロバイダーの代わりに使用する方法の詳細については、関連項目」セクション内のリソースを参照してください。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [セキュリティで保護された ASP.NET アプリケーションの構築: 認証、承認、およびセキュリティで保護された通信](https://msdn.microsoft.com/library/aa302392.aspx)
- [ASP.NET 2.0 の構成情報の暗号化アプリケーション](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [暗号化`Web.config`ASP.NET 2.0 内の値](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [方法: ASP.NET 2.0 の構成セクションを暗号化する DPAPI を使用します。](https://msdn.microsoft.com/library/ms998280.aspx)
- [方法: ASP.NET 2.0 の構成セクションを暗号化する RSA を使用して](https://msdn.microsoft.com/library/ms998283.aspx)
- [.NET 2.0 の構成 API](http://www.odetocode.com/Articles/418.aspx)
- [Windows データ保護](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy および Randy Schmidt でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [次へ](debugging-stored-procedures-vb.md)
