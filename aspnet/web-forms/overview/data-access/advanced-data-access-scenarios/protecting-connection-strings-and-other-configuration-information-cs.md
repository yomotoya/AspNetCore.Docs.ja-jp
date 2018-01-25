---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: "接続文字列とその他の構成情報 (c#) を保護する |Microsoft ドキュメント"
author: rick-anderson
description: "通常、ASP.NET アプリケーションは、Web.config ファイルで構成情報を格納します。 この情報の一部と小文字を区別し、保護することを保証します。 によって定義."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e3782e3d4acc2db0e744128dad64fdfae1e8766d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>保護提供側の接続文字列やその他の構成情報 (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip)または[PDF のダウンロード](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> 通常、ASP.NET アプリケーションは、Web.config ファイルで構成情報を格納します。 この情報の一部と小文字を区別し、保護することを保証します。 既定でこのファイルは提供されません、Web サイトの訪問者が、管理者や、ハッカーによる可能性があります、Web サーバーのファイル システムにアクセスし、ファイルの内容を表示します。 このチュートリアルでは、ASP.NET 2.0 では、Web.config ファイルのセクションを暗号化することで機密情報の保護には学習します。


## <a name="introduction"></a>はじめに

ASP.NET アプリケーションの構成情報は、という名前の XML ファイルに一般的に格納される`Web.config`です。 これらのチュートリアルの過程で更新した、`Web.config`回数のほんの一部です。 作成するときに、`Northwind`に型指定されたデータセット、[の最初のチュートリアル](../introduction/creating-a-data-access-layer-cs.md)、たとえば、接続文字列情報を自動的に追加されたに`Web.config`で、`<connectionStrings>`セクションです。 後の「、[マスター ページとサイト ナビゲーション](../introduction/master-pages-and-site-navigation-cs.md)チュートリアルでは、手動で更新された`Web.config`を追加する、`<pages>`要素は、すべてのプロジェクト内の ASP.NET ページを使用することを示す、`DataWebControls`テーマ。

`Web.config`接続文字列などの機密データを含めることがありますが重要の内容`Web.config`セーフである、許可されていないビューアーから非表示に保持します。 既定では、任意の HTTP の要求を持つファイルに、`.config`拡張機能を返す ASP.NET エンジンによって処理される、*この種類のページが処理されない*図 1 に示すメッセージ。 つまり、訪問者を表示できない、 `Web.config` http://www.YourServer.com/Web.config をブラウザーのアドレス バーに入力するだけでファイルのコンテンツ。


[![Web.config で、ブラウザーを返しますの種類 ページにアクセスしてもメッセージは処理できません。](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**図 1**: 訪問`Web.config`を通じて、ブラウザーを返しますページの種類、これはメッセージを処理できません ([フルサイズのイメージを表示するには、をクリックして](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))。


攻撃者を使用して表示するその他のいくつかの悪用を検索することが場合は、`Web.config`ファイルの内容を s しますか? でした攻撃者で何がこの情報は、および内の機密情報の保護を強化する手順を実行できる`Web.config`しますか? 幸いにも、ほとんどのセクション`Web.config`機密情報は含まれません。 既定の ASP.NET ページによって使用されるテーマの名前がわかっている場合に、攻撃者が利用するどのような損害ことができますか。

特定`Web.config`のセクションでは、ただし、機密情報を接続文字列、ユーザー名、パスワード、サーバー名、暗号化キー、およびなどを含む可能性があります。 通常、この情報は、次にある`Web.config`セクション。

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

このチュートリアルをこのような機密性の高い構成情報を保護する方法について取り上げます。 おように、.NET Framework version 2.0 には、簡単に選択した構成セクションではプログラムで暗号化と復号化が保護された構成システムが含まれています。

> [!NOTE]
> このチュートリアルの最後に、ASP.NET アプリケーションからデータベースに接続するための Microsoft s の推奨事項を確認します。 だけでなく、接続文字列を暗号化するには、セキュリティで保護された方式でデータベースに接続していることを確認して、システムのセキュリティを強化することができます。


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>手順 1: ASP.NET 2.0 の検証の保護構成オプション

ASP.NET 2.0 には、構成情報を暗号化および暗号化の保護された構成システムが含まれています。 これには、プログラムでの暗号化または構成情報を復号化に使用できる .NET Framework のメソッドが含まれます。 保護された構成システムを使用して、[プロバイダー モデル](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)、これにより、開発者はどのような暗号化実装を使用して、選択します。

.NET Framework 2 つの保護構成プロバイダーが付属しています。

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx)使用、非対称[RSA アルゴリズム](http://en.wikipedia.org/wiki/Rsa)暗号化と復号化します。
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx)-Windows を使用して[データ保護 API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx)暗号化と復号化します。

保護された構成システムは、プロバイダーのデザイン パターンを実装するためには、独自の保護構成プロバイダーを作成し、アプリケーションに接続することです。 参照してください[保護構成プロバイダーを実装する](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx)このプロセスについての詳細。

RSA および DPAPI のプロバイダーが暗号化および暗号化解除ルーチンのキーを使用してで、コンピューターまたはユーザー レベルでのこれらのキーを格納することができます。 マシン レベル キーは、独自の専用サーバーでは、web アプリケーションが実行されているシナリオに最適または共有する必要があるサーバーに複数のアプリケーションがあるかどうかは、情報を暗号化します。 ユーザー レベルのキーは、共有ホスティング環境が、同じサーバー上の他のアプリケーションすることはできません、アプリケーションの s が保護された構成セクションを復号化することでより安全なオプションです。

このチュートリアルでは、例は、DPAPI プロバイダーとマシン レベル キーが使用されます。 具体的には、暗号化見ていきます、 `<connectionStrings>` 」の「 `Web.config`、保護された構成システムは、いずれかほとんど暗号化に使用できますが、`Web.config`セクションです。 ユーザー レベルのキーを使用してまたは RSA プロバイダーを使用する方法については、このチュートリアルの最後に、これ以上の測定値セクション内のリソースを参照してください。

> [!NOTE]
> `RSAProtectedConfigurationProvider`と`DPAPIProtectedConfigurationProvider`にプロバイダーが登録されている、`machine.config`プロバイダー名を持つファイル`RsaProtectedConfigurationProvider`と`DataProtectionConfigurationProvider`、それぞれします。 暗号化または適切なプロバイダー名を指定する必要があります。 構成情報を復号化するとき (`RsaProtectedConfigurationProvider`または`DataProtectionConfigurationProvider`) 実際の型名ではなく (`RSAProtectedConfigurationProvider`と`DPAPIProtectedConfigurationProvider`)。 検索することができます、`machine.config`ファイルで、`$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`フォルダーです。


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>手順 2: プログラムによって暗号化し、構成セクションを復号化

数行のコードでは、暗号化または指定したプロバイダーを使用して、特定の構成セクションを復号化おできます。 コード、間もなくに表示される同じ必要があるだけをプログラムで、該当する構成セクションを参照を呼び出す、`ProtectSection`または`UnprotectSection`メソッド、および 呼び出し、`Save`変更を保持するメソッド。 さらに、.NET Framework には、暗号化したり、構成情報を復号化できる便利なコマンド ライン ユーティリティが含まれています。 手順 3 では、このコマンド ライン ユーティリティについて学びます。

Let s をプログラムから保護提供側の構成情報を示すためには、暗号化と復号化するためのボタンを含む ASP.NET ページを作成する、 `<connectionStrings>` 」の「`Web.config`です。

開いて開始、 `EncryptingConfigSections.aspx`  ページで、`AdvancedDAL`フォルダーです。 TextBox コントロールをデザイナーの設定には、ツールボックスからドラッグその`ID`プロパティを`WebConfigContents`、その`TextMode`プロパティを`MultiLine`、およびその`Width`と`Rows`95% と 15 プロパティそれぞれします。 このテキスト ボックス コントロールの内容が表示されます`Web.config`内容を暗号化するかかどうかをすばやく確認することを許可します。 もちろん、実際のアプリケーションではしないの内容を表示する`Web.config`です。

テキスト ボックスに、下にある、という 2 つのボタン コントロールを追加`EncryptConnStrings`と`DecryptConnStrings`です。 接続文字列の暗号化と接続文字列の暗号化を解除するには、そのテキスト プロパティを設定します。

この時点で、画面は図 2 のようになります。


[![テキスト ボックスと 2 つのボタン Web コントロールをページに追加します。](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**図 2**: テキスト ボックスと 2 つのボタン Web コントロールをページに追加 ([フルサイズのイメージを表示するをクリックして](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


次に、読み込みの内容を表示するコードを記述する必要があります`Web.config`で、 `WebConfigContents`  ボックスに、ページが最初に読み込まれています。 ページの分離コード クラスに次のコードを追加します。 このコードは、という名前のメソッドを追加`DisplayWebConfig`からそれを呼び出すと、`Page_Load`イベント ハンドラーと`Page.IsPostBack`は`false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

`DisplayWebConfig`メソッドを使用、 [ `File`クラス](https://msdn.microsoft.com/library/system.io.file.aspx)を開くには、アプリケーション s`Web.config`ファイル、 [ `StreamReader`クラス](https://msdn.microsoft.com/library/system.io.streamreader.aspx)文字列、および、にその内容を読み取る[`Path`クラス](https://msdn.microsoft.com/library/system.io.path.aspx)への物理パスを生成する、`Web.config`ファイル。 これら 3 つのクラスすべて内にある、 [ `System.IO`名前空間](https://msdn.microsoft.com/library/system.io.aspx)です。 その結果、追加する場合は、 `using` `System.IO` 、分離コード クラスか、または、これらのクラスと名のプレフィックスの先頭にステートメント`System.IO.`です。

次に、2 つのボタン コントロールのイベント ハンドラーを追加する必要があります`Click`イベントの暗号化し、復号化に必要なコードを追加し、`<connectionStrings>`セクションのマシン レベル キー DPAPI プロバイダーと共に使用します。 デザイナーで、各ボタンを追加する をダブルクリック、`Click`分離コード内のイベント ハンドラー クラスし、次のコードを追加します。


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

2 つのイベント ハンドラーで使用するコードとほぼ同じです。 現在のアプリケーション s に関する情報を取得することによって、どちらも開始`Web.config`経由でファイル、 [ `WebConfigurationManager`クラス](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx)s [ `OpenWebConfiguration`メソッド](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx)です。 このメソッドは、指定した仮想パスの web 構成ファイルを返します。 次に、`Web.config`ファイル %s`<connectionStrings>`セクションを使用してアクセス、 [ `Configuration`クラス](https://msdn.microsoft.com/library/system.configuration.configuration.aspx)s [ `GetSection(sectionName)`メソッド](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx)、返された、 [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx)オブジェクト。

`ConfigurationSection`オブジェクトが含まれています、 [ `SectionInformation`プロパティ](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx)追加情報や構成セクションに関する機能を提供します。 上のコードとしてをチェックして構成セクションを暗号化するかどうかを決定できます、`SectionInformation`プロパティの`IsProtected`プロパティです。 セクションを暗号化またはを使用して復号化、さらに、`SectionInformation`プロパティ %s`ProtectSection(provider)`と`UnprotectSection`メソッドです。

`ProtectSection(provider)`メソッドは暗号化に使用する保護された構成プロバイダーの名前を指定する文字列を入力として受け取ります。 `EncryptConnString`ボタン s のイベント ハンドラーに DataProtectionConfigurationProvider を渡して、`ProtectSection(provider)`メソッドは DPAPI プロバイダーを使用できるようにします。 `UnprotectSection`メソッドはプロバイダの構成セクションの暗号化に使用されたをためは必要ありません任意の入力パラメーターを確認できます。

呼び出した後、`ProtectSection(provider)`または`UnprotectSection`メソッドを呼び出す必要があります、`Configuration`オブジェクト s [ `Save`メソッド](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx)変更を保持します。 構成情報が暗号化または復号化されているし、呼び、変更を保存すると、`DisplayWebConfig`を読み込む、更新された`Web.config`にテキスト ボックス コントロールの内容。

上記のコードを入力すると、テストにアクセスして、`EncryptingConfigSections.aspx`ページがブラウザーを使用します。 内容を一覧するページを最初に表示する必要があります`Web.config`で、`<connectionStrings>`プレーン テキストで表示されているセクション (図 3 を参照してください)。


[![テキスト ボックスと 2 つのボタン Web コントロールをページに追加します。](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**図 3**: テキスト ボックスと 2 つのボタン Web コントロールをページに追加 ([フルサイズのイメージを表示するをクリックして](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


接続文字列の暗号化 ボタンをクリックします。 要求の検証が有効になっている場合、マークアップからポスト バックされた、 `WebConfigContents`  テキスト ボックスが生成されます、 `HttpRequestValidationException`、潜在的に危険のメッセージを表示する`Request.Form`クライアントからの値が検出されました。 ASP.NET 2.0 では既定で有効になっている要求の検証は、エンコードされていない HTML を含むポストバックを禁止し、スクリプト インジェクション攻撃を防ぐために役立つよう設計されています。 このチェックで、ページまたはアプリケーション レベルを無効にすることができます。 このページをオフに、次のように設定します。、`ValidateRequest`設定`false`で、`@Page`ディレクティブです。 `@Page`宣言型マークアップをページの上部にあるディレクティブがあります。


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

要求の検証、その目的についての詳細で、ページおよびアプリケーションのレベル、方法を HTML マークアップをエンコードとを無効にすることを参照してください[要求の検証 - スクリプト攻撃の防止](../../../../whitepapers/request-validation.md)です。

ページの要求の検証を無効にした後、接続文字列の暗号化 ボタンを再度クリックすると再試行してください。 ポストバックで、構成ファイルにアクセスして、その`<connectionStrings>`DPAPI プロバイダーを使用して暗号化されたセクションです。 テキスト ボックスを更新して、新しい表示し、`Web.config`コンテンツ。 図 4 に示す、`<connectionStrings>`情報は暗号化されています。


[![暗号化接続文字列 ボタンで暗号化をクリックすると、 &lt;connectionString&gt;セクション](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**図 4**: 暗号化接続文字列 ボタンで暗号化をクリックすると、`<connectionString>`セクション ([フルサイズのイメージを表示するをクリックして](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


暗号化された`<connectionStrings>`でコンテンツの一部ですが、自分のコンピューターで生成されたセクションの次に、`<CipherData>`要素が簡略化のため削除されました。


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>`要素の暗号化を実行するために使用するプロバイダーを指定します (`DataProtectionConfigurationProvider`)。 この情報を使用して、`UnprotectSection`メソッド、接続文字列の暗号化を解除 ボタンがクリックされたときにします。


接続文字列情報のアクセスと`Web.config`- いずれか書き込み、SqlDataSource コントロールからのコードや、型指定されたデータセットを Tableadapter から自動生成されたコードによって、自動的に暗号化の解除はします。 つまり、余分なコードや、暗号化された暗号化を解除するためのロジックを追加お必要はありません`<connectionString>`セクションです。 これを示すためを参照してください前のチュートリアルのいずれかの基本的なレポート セクションから単純な表示チュートリアルなど、この時点で (`~/BasicReporting/SimpleDisplay.aspx`)。 図 5 に示すチュートリアルが予想されるが、こと、暗号化された接続文字列の情報は自動的に暗号化解除されて、ASP.NET ページによってを示すとおりに動作します。


[![データ アクセス層には、接続文字列情報を自動的に復号化します。](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**図 5**: データ アクセス層には、接続文字列情報を自動的に復号化 ([フルサイズのイメージを表示するをクリックして](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


元に戻す、`<connectionStrings>`プレーン テキスト形式に戻すセクションで、接続文字列の暗号化を解除 ボタンをクリックします。 ポストバックの内の接続文字列を表示する必要があります`Web.config`プレーン テキストでします。 この時点で画面にこのページ (図 3 を参照) を最初にアクセスしたときと同じようになります。

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>手順 3: を使用して構成セクションを暗号化します。`aspnet_regiis.exe`

.NET Framework には、さまざまなコマンド ライン ツールが含まれています、`$WINDOWS$\Microsoft.NET\Framework\version\`フォルダーです。 [を使用して SQL キャッシュ依存関係](../caching-data/using-sql-cache-dependencies-cs.md)チュートリアルでは、たとえば、について説明しましたを使用して、 `aspnet_regsql.exe` SQL キャッシュ依存関係のために必要なインフラストラクチャを追加するコマンド ライン ツールです。 このフォルダー内の他の便利なコマンド ライン ツールは、 [ASP.NET IIS 登録ツール (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx)です。 その名前からわかるように、ASP.NET IIS 登録ツールは Microsoft s professional グレード Web サーバー、IIS と ASP.NET 2.0 アプリケーションを登録する、主に使用されます。 IIS に関連する機能に加えて、ASP.NET IIS 登録ツールこともできますを暗号化またはで指定された構成セクションを復号化`Web.config`です。

次のステートメントで構成セクションを暗号化するために使用する一般的な構文を示しています、`aspnet_regiis.exe`コマンド ライン ツール。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*セクション*構成セクション (connectionStrings) などの暗号化には、*物理\_ディレクトリ*web アプリケーションのルート ディレクトリへの完全、物理パスと*プロバイダー* (DataProtectionConfigurationProvider) などを使用する保護された構成プロバイダーの名前を指定します。 また、web アプリケーションが IIS に登録されている場合、次の構文を使用して、物理パスではなく仮想パスを入力できます。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

次`aspnet_regiis.exe`の例では暗号化、`<connectionStrings>`マシン レベル キーで DPAPI プロバイダーを使用してセクションします。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

同様に、`aspnet_regiis.exe`コマンド ライン ツールは、構成セクションを復号化を使用することができます。 使用する代わりに、`-pef`スイッチを使用して`-pdf`(または instead of`-pe`を使用して`-pd`)。 また、復号化するときに、プロバイダー名が必要ないことに注意してください。


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> 実行する必要がありますをコンピューターに固有のキーを使用して、DPAPI プロバイダーを使用するため`aspnet_regiis.exe`web ページが処理されている元の同一のコンピューターからです。 たとえば、ローカル開発用コンピューターからこのコマンド ライン プログラムを実行し、実稼働サーバーに暗号化された Web.config ファイルをアップロードすると、実稼働サーバーできなくが暗号化されているため、接続文字列情報の暗号化を解除するには開発用コンピューターに固有のキーを使用します。 RSA プロバイダーは別のコンピューターに、RSA キーをエクスポート可能であれば、このような制限がありません。


## <a name="understanding-database-authentication-options"></a>データベース認証オプションの理解

任意のアプリケーションを発行する前に`SELECT`、 `INSERT`、 `UPDATE`、または`DELETE`Microsoft SQL Server データベース、データベースへのクエリはまず、要求元を特定する必要があります。 このプロセスと呼ばれます*認証*し、SQL Server 認証の 2 つのメソッドを提供します。

- **Windows 認証**-アプリケーションが実行されているプロセスを使用して、データベースと通信します。 Visual Studio 2005 の ASP.NET 開発サーバー経由での ASP.NET アプリケーションを実行するときに、ASP.NET アプリケーションは、現在ログオンしているユーザーの id を想定しています。 ASP.NET アプリケーション Microsoft インターネット インフォメーション サーバー (IIS) で、ASP.NET アプリケーション通常の id を想定`domainName``\MachineName`または`domainName``\NETWORK SERVICE`、これをカスタマイズすることができます。
- **SQL 認証**-ユーザーの ID とパスワードの値は、認証の資格情報として提供されています。 SQL 認証でユーザー ID とパスワードは、接続文字列で提供されます。

Windows 認証はより安全であるために、SQL 認証よりも優先します。 Windows 認証を使用、接続文字列からユーザー名とパスワードは、web サーバーとデータベース サーバーは、次の 2 つの異なるコンピューターに存在する場合、資格情報がプレーン テキストでネットワーク経由で送信されません。 SQL 認証では、ただし、認証資格情報、接続文字列にハードコードされて、web サーバーからプレーン テキストで、データベース サーバーに転送されます。

これらのチュートリアルでは、Windows 認証を使用しています。 接続文字列の検査がどのような認証モードを使用していることがわかります。 内の接続文字列`Web.config`チュートリアルされてきました。

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Integrated Security = True とユーザー名とパスワードの欠如は、Windows 認証を使用していることを示します。 いくつかの接続で用語の信頼された接続文字列を = [はい] または統合セキュリティ = 統合セキュリティではなく SSPI を使用 = true の場合、3 つすべての Windows 認証の使用がします。

次の例では、SQL 認証を使用する接続文字列を示します。 接続文字列に埋め込まれた資格情報に注意してください。

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

攻撃者が、アプリケーションの s を表示できることを想像`Web.config`ファイル。 インターネット経由でアクセスできるデータベースへの接続に SQL 認証を使用する場合、攻撃者は、SQL Management studio または独自の web サイトで、ASP.NET ページから、データベースへの接続にこの接続文字列を使用できます。 この脅威を軽減するで接続文字列情報を暗号化`Web.config`保護された構成システムを使用します。

> [!NOTE]
> SQL Server で利用可能な認証のさまざまな種類の詳細については、次を参照してください。[セキュリティで保護された ASP.NET アプリケーションの構築: 認証、承認、およびセキュリティ保護された通信](https://msdn.microsoft.com/library/aa302392.aspx)です。 さらに接続文字列例については Windows と SQL 認証の構文の違いを示すを参照してください[ConnectionStrings.com](http://www.connectionstrings.com/)です。


## <a name="summary"></a>まとめ

既定では、ファイルが、 `.config` ASP.NET アプリケーションで拡張機能は、ブラウザーからアクセスすることはできません。 データベース接続文字列、ユーザー名やパスワードなどの機密情報を含むに可能性がありますので、これらの種類のファイルは返されません。 .NET 2.0 で保護された構成システムは、さらに指定された構成セクションを暗号化するようにすることで機密情報を保護します。 2 つの組み込みの保護構成プロバイダーがあります。 1 つを使用した RSA アルゴリズムと 1 つを、Windows データ保護 API (DPAPI) を使用します。

このチュートリアルでは、DPAPI プロバイダーを使用して構成設定の暗号化/暗号化解除する方法を説明しました。 これには、プログラムでは、手順 2 で示したように、同様の両方から、`aspnet_regiis.exe`コマンド ライン ツールは、手順 3. で対象となっていた。 ユーザー レベルのキーを使用してや RSA プロバイダーの代わりに使用する方法の詳細については、関連項目」セクション内のリソースを参照してください。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [セキュリティで保護された ASP.NET アプリケーションの構築: 認証、承認、およびセキュリティで保護された通信](https://msdn.microsoft.com/library/aa302392.aspx)
- [ASP.NET 2.0 の構成情報の暗号化アプリケーション](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [暗号化`Web.config`ASP.NET 2.0 内の値](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [方法: ASP.NET 2.0 の構成セクションを暗号化する DPAPI を使用します。](https://msdn.microsoft.com/library/ms998280.aspx)
- [方法: ASP.NET 2.0 の構成セクションを暗号化する RSA を使用して](https://msdn.microsoft.com/library/ms998283.aspx)
- [.NET 2.0 の構成 API](http://www.odetocode.com/Articles/418.aspx)
- [Windows データ保護](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Teresa マーフィーおよびものです。 Schmidt がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
[次へ](debugging-stored-procedures-cs.md)
