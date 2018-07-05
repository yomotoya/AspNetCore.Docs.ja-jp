---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: Visual Studio (c#) を使用して、サイトの展開 |Microsoft Docs
author: rick-anderson
description: Visual Studio には、web サイトを展開するためのツールが含まれています。 このチュートリアルではこれらのツールの詳細について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: d549c615ea822d58ae71876ff3acd28c5f773d8a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399761"
---
<a name="deploying-your-site-using-visual-studio-c"></a>Visual Studio (c#) を使用してサイトを展開します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> Visual Studio には、web サイトを展開するためのツールが含まれています。 このチュートリアルではこれらのツールの詳細について説明します。


## <a name="introduction"></a>はじめに

上記のチュートリアルは、web ホスト プロバイダーに単純な ASP.NET web アプリケーションをデプロイする方法を説明しました。 具体的には、FileZilla のような FTP クライアントを使用して、開発環境から運用環境に必要なファイルを転送する方法を示したチュートリアルです。 Visual Studio では、web ホスト プロバイダーへのデプロイを容易に組み込みのツールも提供します。 このチュートリアルでは、これらのツールの 2 つはについて: Web サイトのコピー ツールは、FTP または FrontPage Server Extensions; を使用してリモートの web サーバーとの間にファイルを移動できます発行ツールを web サイト全体を指定した場所にコピーします。


> [!NOTE]
> Visual Studio によって提供されるその他の展開に関連するツールが含まれます[Web セットアップ プロジェクト](https://msdn.microsoft.com/library/wx3b589t.aspx)と[Web Deployment Projects](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en)アドイン。 Web セットアップ プロジェクトは、web サイトのコンテンツと構成については、1 つの MSI ファイルにパッケージ化します。 このオプションは、または顧客がそれぞれ独自の web サーバーにインストールされる事前にパッケージされた web アプリケーションを販売している会社の web サイトをイントラネット内にデプロイされる場合に便利です。 Web 展開プロジェクト アドインは、開発環境と運用環境のビルドを Visual Studio アドインを指定する構成の違いを容易にします。 Web セットアップ プロジェクトは、このチュートリアル シリーズでは; では説明しませんWeb Deployment Projects をまとめたもの、 [*一般的な構成の相違点の間で開発および運用*](common-configuration-differences-between-development-and-production-cs.md)チュートリアル。


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Web サイトのコピー ツールを使用してサイトを展開します。

Visual Studio の Web サイトのコピー ツールは、スタンドアロン FTP クライアント機能に似ています。 簡単に言うと、Web サイトのコピー ツールを使用すると、FTP または FrontPage Server Extensions 経由のリモート web サイトに接続できます。 2 つのペインの FileZilla のユーザー インターフェイスと同様に、Web サイトのコピーのユーザー インターフェイスを構成します。 左側のウィンドウが右側のウィンドウは、移行先サーバーでファイルを表示中に、ローカル ファイルを示します。

> [!NOTE]
> Web サイトのコピー ツールは、Web サイト プロジェクトのできるだけです。 Web アプリケーション プロジェクトを使用しているときに、visual Studio はこのツールを提供します。

Web サイトのコピー ツールを使用して、運用環境に書籍レビューのアプリケーションを発行するの見てをみましょう。 Web サイトのコピー ツールは、Web サイト プロジェクト モデルを使用するプロジェクトでのみ動作するため、BookReviewsWSP プロジェクトでこのツールを使用してのみ確認できます。 そのプロジェクトを開きます。

ソリューション エクスプ ローラー (このアイコンは図 1 丸); Web サイトのコピー アイコンをクリックして、Web サイトのコピー ツールのプロジェクトを起動します。また、web サイト メニューから Web サイトのコピー オプションを選択できます。 どちらの方法は、図 1 に示すように Web サイトのコピーのユーザー インターフェイスを起動します。まだリモート サーバーに接続があるので、図 1 の左側のウィンドウのみが設定されます。


[![コピーの Web サイト ツールのユーザー インターフェイスが 2 つのペインに分割されます。](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**図 1**: The の Web サイトのコピー ツールのユーザー インターフェイスが 2 つのペインに分割されます ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-visual-studio-cs/_static/image3.png))。


私たちのサイトを展開するためには、まず、web ホスト プロバイダーに接続する必要があります。 Web サイトのコピーのユーザー インターフェイスの上部にある [接続] ボタンをクリックします。 図 2 に示すように Web サイトを開く ダイアログ ボックスが表示されます。

左から 4 つのオプションのいずれかを選択して、変換先の web サイトに接続できます。

- **ファイル システム**-お使いのコンピューターからアクセスできるフォルダーまたはネットワーク共有にサイトを展開する場合に選択します。
- **ローカル IIS** -このオプションを使用して、コンピューターにインストールされている IIS web サーバーにサイトをデプロイします。
- **FTP サイト**-FTP を使用してリモートの web サイトに接続します。
- **リモート サイト**の FrontPage Server Extensions を使用してリモートの web サイトに接続します。

ほとんどの web ホスト プロバイダーは、FTP をサポートしますが、FrontPage サーバー拡張機能のサポートを提供は少なくなります。 そのため、FTP サイト オプションを選択して図 2 に示すように、接続情報を入力します。


[![変換先の web サイトを指定します。](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**図 2**: 変換先の web サイトを指定 ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-visual-studio-cs/_static/image6.png))。


接続した後、Web サイトのコピー ツールが右側のウィンドウで、リモート サイトのファイルを読み込みし、各ファイルの状態を示します。 新規の削除、変更、または Unchanged します。 逆に、リモートのサイトに、ローカル サイトからファイルをコピーすることができます。

新しい追加 BookReviewsWSP プロジェクトへのページングし、Web サイトのコピー ツールの動作を確認できるように、デプロイします。 という名前のルート ディレクトリで、Visual Studio で新しい ASP.NET ページを作成`Privacy.aspx`です。 マスター ページを使用して、ページがある`Site.master`とこのページに、サイトのプライバシー ポリシーを追加します。 図 3 は、このページを作成した後、Visual Studio を示します。


[![という新しいページを追加&lt;コード&gt;Privacy.aspx&lt;/code&gt;を web サイトのルート フォルダー](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**図 3**: という新しいページを追加`Privacy.aspx`web サイトのルート フォルダーに ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-visual-studio-cs/_static/image9.png))。


次に、Web サイトのコピーのユーザー インターフェイスを返します。 左側のウィンドウは、新しいファイルを含む、図 4 に示すよう`Policy.aspx`と`Policy.aspx.cs`します。 さらに、これらのファイルは、矢印のアイコンと状態の新しいリモート サイトではなく、ローカル サイトが存在することを示すでマークされます。


[![Web サイトのコピー ツールは、新規を含む&lt;コード&gt;Privacy.aspx&lt;/code&gt;ページの左ペインで](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**図 4**: Web サイトのコピー ツールは、新規を含む`Privacy.aspx`ページの左ペインで ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-visual-studio-cs/_static/image12.png))。


新しい展開するファイルの選択し、リモート サイトに転送する矢印アイコンをクリックします。 転送が完了したら、`Policy.aspx`と`Policy.aspx.cs`Unchanged 状態で、ローカルおよびリモート サイトの両方にファイルが存在します。

一覧を表示するには、新しいファイルとは、Web サイトのコピー ツールには、ローカルおよびリモートのサイト間で差異がある任意のファイルが強調表示されます。 アクションの表示に戻り、`Privacy.aspx`ページおよびプライバシー ポリシーをいくつかの複数の単語を追加します。 ページを保存してから、Web サイトのコピー ツールに戻ります。 図 5 に示すよう、`Privacy.aspx`ページの左側のペインで、変更は、リモート サイトとの同期であることを示す状態。


[![Web サイトのコピー ツールでは、ことを示します、&lt;コード&gt;Privacy.aspx&lt;/code&gt;ページが変更されました](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**図 5**: Web サイトのコピー ツールでは、ことを示します、`Privacy.aspx`ページが変更されました ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-visual-studio-cs/_static/image15.png))。


Web サイトのコピー ツールでは、最後のコピー操作、ファイルが削除されたかについても示します。 削除、`Privacy.aspx`からローカルのプロジェクトと Web サイトのコピー ツールを更新します。 `Privacy.aspx`と`Privacy.aspx.cs`ファイルは、左側のウィンドウに残りますが、最後のコピー操作が削除されたことを示す削除済み状態。

## <a name="publishing-a-web-application"></a>Web アプリケーションの発行

Visual Studio 内から web アプリケーションをデプロイする別の方法では、ビルド メニューからアクセス可能である 発行 オプションを使用します。 発行 オプションは、明示的にアプリケーションをコンパイルし、すべての指定したリモート サイトに必要なファイルをコピーします。 間もなく表示されるよう、発行 オプションは、Web サイトのコピー ツールよりもより直接的にします。 Web サイトのコピー ツールでは、ローカルおよびリモートのサイト上のファイルを調べることができ、アップロードまたは必要に応じて、個々 のファイルをダウンロードすること、一方、[発行] オプションは、web アプリケーション全体を配置します。


すべての必要なファイルを指定されたリモート サイトにコピーするだけでなく、[発行] オプションは明示的にアプリケーションをコンパイルします。 信じないの発行 オプションは Web アプリケーション プロジェクトで使用できることには、Web アプリケーション プロジェクトをする必要があることのことを取得するように明示的にコンパイルします。 少し意外などのような場合がありますが、発行オプションは、Web サイト プロジェクトの使用もします。 説明したとおり、 [*を決定する必要のあるファイルを展開する*](determining-what-files-need-to-be-deployed-cs.md)と呼ばれるプロセスを介して、チュートリアルでは、Web サイト プロジェクトを明示的にコンパイルできます*プリコンパイル*します。 このチュートリアルは、Web アプリケーション プロジェクトを発行 オプションを使用する方法では説明します。今後のチュートリアルでは、この時点で戻って Web Site プロジェクトで、発行 オプションを使用して見てプリコンパイルを紹介します。

> [!NOTE]
> 発行 オプションは Visual Studio for Web サイト プロジェクトと Web Application Projects の両方で使用できますが、Visual Web Developer は Web アプリケーション プロジェクトの発行 オプションのみを提供します。


発行オプションを使用して書籍レビューのアプリケーションの配置を見てみましょう。 Visual Studio で BookReviewsWAP (Web アプリケーション プロジェクト) を開いてを開始します。 [発行] メニューからビルド BookReviewsWAP プロジェクトを選択します。 (図 6 参照)、他の構成オプションの間で、ターゲットの場所の入力を求めるダイアログ ボックスが表示されます。 かなりのように Web サイトのコピー ツールを使用して入力できますの場所をローカル フォルダー、ローカルの IIS で web サイト、FrontPage Server Extensions、または FTP サーバーのアドレスをサポートするリモートの web サイトを。 リモート web サーバー上のファイルに配置されたファイルを置き換える、またはパブリッシュする前に、リモート サイトにコンテンツをすべて削除するかどうかを選択できます。 コピーするかどうかを指定することもできます。

- 不要なソース コードとプロジェクトに関連するファイルを削除し、アプリケーションの実行に必要なプロジェクト ファイルのみです。
- すべてのソース コード ファイルを含むプロジェクト ファイル、および Visual Studio プロジェクト ファイルなどのソリューション ファイルを実行します。
- プロジェクトに含めるしているかどうかに関係なくソース プロジェクト フォルダー内のすべてのファイルをコピーするソース プロジェクト フォルダー内のすべてのファイル。

内容をアップロードするオプションがありますも、`App_Data`フォルダー。


[![変換先の web サイトを指定します。](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**図 6**: 変換先の web サイトを指定 ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-visual-studio-cs/_static/image18.png))。


ブック_レビュー アプリケーションのリモート サイトには、Web サイトのコピー ツールを使用して BookReviewsWSP プロジェクトをコピーするときに展開されているファイルが含まれています。 そのため、すべての既存のコンテンツを削除することによって開始発行オプションがありますしてみましょう。 また、だけをコピー不要なソース コードとプロジェクト ファイルで、運用環境と雑然とさせるのではなく、必要なファイル。 指定した後は、これらのオプションは、[発行] ボタンをクリックします。 次の数秒間経由で Visual Studio は、必要なファイルを展開先のサイト、出力ウィンドウにその進行状況を表示します。

図 7 は、発行操作が完了した後、FTP サイト上のファイルを示します。 マークアップのページのみと必要なサーバー側とクライアント側のサポート ファイルがアップロードされたことに注意してください。


[![運用環境に必要なファイルのみが公開されました。](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**図 7**: のみ、必要なファイルに公開された運用環境 ([フルサイズの画像を表示する をクリックします](deploying-your-site-using-visual-studio-cs/_static/image21.png))。


発行 オプションは、Web サイトのコピー ツールよりも小さい微妙ツールです。 Web サイトのコピー ツールを使用すると、ローカルおよびリモートのサイト上のファイルを検査し、どのように異なるかを参照してください、一方、[発行] オプションにはこのようなインターフェイスはありません。 さらに、Web サイトのコピー ツールには、1 回限りの変更、アップロードまたは個々 のファイルを削除することができます。 発行オプションがこのような細かいコントロールを許可しません。代わりに、その発行、*全体*アプリケーション。 この動作は、その長所と短所が。 プラスの面では、するは、重要なファイルをアップロードする忘れたりしません発行オプションを使用するタイミングがわかります。 何が起きる - 非常に大規模な web サイトに小さな変更を加えた場合発行オプションを使用してそのページまたは変更された 2 つを更新することはできませんが、代わりに、Visual Studio は、サイト全体を展開して、待機する必要があります。

運用環境と開発環境の間でコンテンツを持つとは異なる特定のファイルにすることは珍しくないです。 キーの例は、アプリケーションの構成ファイル、`Web.config`します。 発行オプションが無条件に web アプリケーションのファイルをコピーするため、開発環境でのバージョンで、運用環境のカスタマイズされた構成ファイルを上書きします。 後続のチュートリアルでは、さらに、このトピックについて説明し、このような違いが存在する場合は、web アプリケーションをデプロイするためのヒントが提供されます。

## <a name="summary"></a>まとめ

Web サイトを展開するには、開発環境から運用環境に必要なファイルをコピーする必要があります。 前のチュートリアルでは、FileZilla のような FTP クライアントを使用してファイルを転送する方法を示しました。 このチュートリアルは、Visual Studio での 2 つの展開ツールを調べる: Web サイトのコピー ツールと、[発行] オプション。 ローカル コンピューターと簡単にアップロードまたは 2 台のコンピューター間でファイルをダウンロードすることを指定したリモート コンピューター上のファイルを一覧表示する 2 つのウィンドウで構成されるインターフェイスでは、Web サイトのコピー ツールを FTP クライアントに似ています。 発行 オプションは、明示的にプロジェクトをコンパイルし、指定したコピー先に全体のアプリケーションをデプロイする直接的なツールです。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Web サイトのコピー ツールを使用した Web サイトのコピー](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [操作は Web サイトのコピー ツールを使用して Web サイトをデプロイする方法](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md)(ビデオ)
- [方法: Web アプリケーション プロジェクトを発行します。](https://msdn.microsoft.com/library/aa983453.aspx)
- [方法: Web サイトを発行します。](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [セットアップ/配置プロジェクトで Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [前へ](deploying-your-site-using-an-ftp-client-cs.md)
> [次へ](common-configuration-differences-between-development-and-production-cs.md)
