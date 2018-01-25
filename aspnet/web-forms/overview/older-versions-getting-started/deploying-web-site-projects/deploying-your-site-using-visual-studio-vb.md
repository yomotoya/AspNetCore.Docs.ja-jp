---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: "Visual Studio (VB) を使用して、サイトを展開する |Microsoft ドキュメント"
author: rick-anderson
description: "Visual Studio には、web サイトを展開するためのツールが含まれています。 このチュートリアルでは、これらのツールの詳細を説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 362f8391f3352b3abf00045bca0c212cd850b17f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="deploying-your-site-using-visual-studio-vb"></a>Visual Studio (VB) を使用して、サイトを展開します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio には、web サイトを展開するためのツールが含まれています。 このチュートリアルでは、これらのツールの詳細を説明します。


## <a name="introduction"></a>はじめに

前のチュートリアルは、web ホスト プロバイダーへの単純な ASP.NET web アプリケーションを展開する方法を検索します。 具体的には、このチュートリアル FileZilla のような FTP クライアントを使用して、開発環境から運用環境に必要なファイルを転送する方法を示しました。 Visual Studio では、web ホスト プロバイダーへの展開を支援する組み込みのツールも提供します。 このチュートリアルでは、これらのツールの 2 つを調べます FTP または; の FrontPage Server Extensions を使用してリモートの web サーバーとの間にファイルを移動する、Web サイトのコピー ツール。発行ツール、全体の web サイトを指定した場所にコピーします。


> [!NOTE]
> Visual Studio によって提供される他の展開に関連するツールのインクルード[Web セットアップ プロジェクト](https://msdn.microsoft.com/library/wx3b589t.aspx)と[Web デプロイメント プロジェクト](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en)アドイン。 Web セットアップ プロジェクトは、web サイトのコンテンツと構成については、1 つの MSI ファイルにパッケージ化します。 このオプションは、イントラネット内で展開されている web サイトまたは web サーバー上のお客様にインストールするパッケージ化された web アプリケーションを販売している会社に最も役立ちます。 この Web 展開プロジェクト追加では、Visual Studio アドインを指定する構成の違いを容易に開発環境と運用環境のビルドします。 Web セットアップ プロジェクトはこの一連のチュートリアルについては説明していませんWeb 配置プロジェクトをまとめたもの、 [*一般的な構成の相違点の間で開発および運用*](common-configuration-differences-between-development-and-production-vb.md)チュートリアルです。


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Web サイトのコピー ツールを使用して、サイトを展開します。

Visual Studio の Web サイトのコピー ツールでは、スタンドアロンの FTP クライアント機能に似ています。 簡単に言うと、Web サイトのコピー ツールには、FTP または FrontPage Server Extensions 経由のリモート web サイトに接続することができます。 FileZilla のユーザー インターフェイスと同様に、Web サイトのコピーのユーザー インターフェイスから成る 2 つのペイン: 左側のウィンドウが右側のペインが移行先サーバーでそれらのファイルを一覧表示中に、ローカル ファイルを示します。

> [!NOTE]
> Web サイトのコピー ツールは、Web サイト プロジェクトのできるだけです。 Visual Studio は、Web アプリケーション プロジェクトを使用しているときに、このツールを提供しています。


実稼働環境にブック レビュー アプリケーションを発行する Web サイトのコピー ツールを使用してで見てをみましょう。 Web サイトのコピー ツールは、Web サイト プロジェクトのモデルを使用するプロジェクトでのみ動作するため BookReviewsWSP プロジェクトでこのツールを使用してのみ確認できます。 そのプロジェクトを開きます。

ソリューション エクスプ ローラーを (このアイコンは、丸で囲まれた図 1) で; Web サイトのコピー アイコンをクリックして、Web サイトのコピー ツール プロジェクトを起動します。また、web サイト メニューから Web サイトのコピー オプションを選択できます。 どちらの方法が図 1 に示すように Web サイトのコピーのユーザー インターフェイスを起動します。まだリモート サーバーに接続しているために、図 1 の左側のペインのみが格納されます。


[![コピーの Web サイト ツールのユーザー インターフェイスが 2 つのペインに分かれています](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**図 1**: コピー Web サイト ツールのユーザー インターフェイスが 2 つのペインに分かれています ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-visual-studio-vb/_static/image3.png))


サイトを展開するためには、まず web ホスト プロバイダーに接続する必要があります。 Web サイトのコピーのユーザー インターフェイスの上部にある [接続] ボタンをクリックします。 これには、図 2 に示すように Web サイトを開く ダイアログ ボックスが表示されます。

左から 4 つのオプションのいずれかを選択して、移行先の web サイトに接続できます。

- **ファイル システム**-このコンピューターからアクセス可能なフォルダーまたはネットワーク共有に、サイトを展開するオプションを選択します。
- **ローカル IIS** -このオプションを使用して、コンピューターにインストールされている IIS web サーバーにサイトを展開します。
- **FTP サイト**-FTP を使用してリモートの web サイトに接続します。
- **リモート サイト**の FrontPage Server Extensions を使用してリモートの web サイトに接続します。

ほとんどの web ホスト プロバイダーをサポートして、FTP が FrontPage サーバー拡張機能のサポートを提供が少ない。 そのためは FTP サイト オプションを選択して図 2 に示すように、接続情報を入力します。


[![移行先の web サイトを指定します。](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**図 2**: 先の web サイトを指定 ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-visual-studio-vb/_static/image6.png))


接続した後、Web サイトのコピー ツール右側のウィンドウで、リモート サイトにあるファイルを読み込みますを示し、各ファイルの状態: 新規の削除、変更、または Unchanged です。 リモート サイト、または逆 a に、ローカル サイトからファイルをコピーできます。

新しく追加してみましょう。 BookReviewsWSP プロジェクトに ページに Web サイトのコピー ツールの動作を確認できるように配置します。 Visual Studio で新しい ASP.NET ページをという名前のルート ディレクトリに作成`Privacy.aspx`です。 マスター ページを使用してページを含む`Site.master`し、このページに、サイトのプライバシー ポリシーを追加します。 図 3 は、このページを作成した後に、Visual Studio を示します。


[![という新しいページを追加&lt;コード&gt;Privacy.aspx&lt;/code&gt;を web サイトのルート フォルダー](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**図 3**: という新しいページを追加`Privacy.aspx`、web サイトのルート フォルダーに ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-visual-studio-vb/_static/image9.png))


次に、Web サイトのコピーのユーザー インターフェイスを返します。 図 4 では、左側のウィンドウが含まれています - 新しいファイルには`Policy.aspx`と`Policy.aspx.vb`です。 さらに、これらのファイルは、矢印アイコンおよび状態の新しいローカル サイトではなく、リモート サイトが存在することを示すでマークされます。


[![Web サイトのコピー ツールを含む、新規&lt;コード&gt;Privacy.aspx&lt;/code&gt;ページの左ペインで](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**図 4**: Web サイトのコピー ツールを含む、新規`Privacy.aspx`ページの左ペインで ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-visual-studio-vb/_static/image12.png))


新しい展開にファイルがそれらを選択し、リモート サイトに転送する矢印アイコンをクリックします。 転送が完了した後、`Policy.aspx`と`Policy.aspx.vb`Unchanged 状態でローカルとリモート両方のサイトにファイルが存在します。

新しいファイルを一覧表示すると共に、Web サイトのコピー ツールには、ローカルおよびリモートのサイト間で異なっているすべてのファイルが強調表示されます。 これを見るに戻り、`Privacy.aspx`ページおよびプライバシー ポリシーに、いくつかの複数の単語を追加します。 ページを保存して、Web サイトのコピー ツールに戻ります。 図 5 に示す、`Privacy.aspx`左側のウィンドウでページが変更されたことを示す、リモート サイトとの同期の状態。


[![Web サイトのコピー ツールでは、ことを示します、&lt;コード&gt;Privacy.aspx&lt;/code&gt;ページが変更されました](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**図 5**: Web サイトのコピー ツールでは、ことを示します、`Privacy.aspx`ページが変更されました ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-visual-studio-vb/_static/image15.png))


Web サイトのコピー ツールでは、最後のコピー操作の後、ファイルが削除されたかについても示します。 削除、`Privacy.aspx`からローカルのプロジェクトと Web サイトのコピー ツールを更新します。 `Privacy.aspx`と`Privacy.aspx.vb`ファイルは、左側のウィンドウに表示されたままが、最後のコピー操作の後、削除されたことを示す削除済み状態。

## <a name="publishing-a-web-application"></a>Web アプリケーションの発行

Visual Studio 内から web アプリケーションを配置する別の方法では、[ビルド] メニューからアクセス可能である [発行] オプションを使用してます。 発行 オプションは、明示的にアプリケーションをコンパイルし、すべての指定、リモート サイトに必要なファイルがコピーされます。 思います間もなく、公開する オプションは、Web サイトのコピー ツールよりも矢印の反対です。 Web サイトのコピー ツールでは、ローカルおよびリモートのサイト上のファイルを調べることができ、アップロードまたは必要に応じて、個々 のファイルをダウンロードできるようになります、一方、[発行] オプションは、web アプリケーション全体を展開します。


すべての必要なファイルを指定したリモート サイトにコピーするだけでなく、[発行] オプションは明示的にアプリケーションをコンパイルします。 Web アプリケーション プロジェクトをする必要があることに明示的にコンパイルする必要があります付属して発行 オプションが Web アプリケーション プロジェクトで使用可能である当然として。 少しことにより意外どのような場合がありますは、公開する オプションも Web サイト プロジェクトで使用可能です。 説明したとおり、 [*を決定する必要のあるファイルを展開する*](determining-what-files-need-to-be-deployed-vb.md)と呼ばれるプロセスを介して、チュートリアルでは、Web サイト プロジェクトを明示的にコンパイルできます*プリコンパイル*です。 このチュートリアルは、Web アプリケーション プロジェクトで、発行 オプションの使用に関する重点を置いています今後のチュートリアルでは、事前コンパイル、時点を返しますが Web サイト プロジェクトに発行 オプションを使用して調べるを調査します。

> [!NOTE]
> 発行 オプションは Web サイト プロジェクトと Web アプリケーション プロジェクトの両方の Visual Studio で使用できますが、Visual Web Developer は Web アプリケーション プロジェクトの発行 オプションのみを提供します。


発行オプションを使用して書評アプリケーションの配置を見てみましょう。 まず Visual Studio で BookReviewsWAP (Web アプリケーション プロジェクト) を開きます。 [発行] メニューからビルド BookReviewsWAP プロジェクトを選択します。 (図 6 を参照してください) その他の構成オプションの中から、ターゲットの場所の入力を求めるダイアログ ボックスが表示されます。 かなりのように Web サイトのコピー ツールで入力できますの場所をローカル フォルダー、IIS でのローカル web サイト、FrontPage Server Extensions は、または FTP サーバーのアドレスをサポートするリモートの web サイトを。 配置済みのファイルで、リモート web サーバー上のファイルを置き換える、またはパブリッシュする前に、リモート サイトにコンテンツをすべて削除するかどうかを選択できます。 コピーするかどうかを指定することもできます。

- のみ、プロジェクト内のファイルで不要なソース コードとのプロジェクト関連ファイルを省略すると、アプリケーションを実行するために必要です。
- すべてのソース コード ファイルを含むプロジェクト ファイル、および Visual Studio プロジェクト ファイルなどのソリューション ファイルを実行します。
- すべてのファイルの元のプロジェクト フォルダーで、プロジェクトに含まれるかどうかに関係なく、ソース プロジェクト フォルダー内のすべてのファイルをコピーします。

コンテンツをアップロードするためのオプションはまた、`App_Data`フォルダーです。


[![移行先の web サイトを指定します。](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**図 6**: 先の web サイトを指定 ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-visual-studio-vb/_static/image18.png))


アプリケーションの書籍の確認は、リモート サイトには、Web サイトのコピー ツールを使用して BookReviewsWSP プロジェクトをコピーするときに展開されたファイルが含まれています。 したがって、既存のすべてのコンテンツを削除することによって開始発行オプションがありますしてみましょう。 また、みましょうだけをコピー、不要なソース コードとプロジェクト ファイルに、実稼働環境を雑然とさせることのではなく、必要なファイルです。 これらのオプションを指定してから、公開ボタンをクリックします。 次の数秒で Visual Studio は、必要なファイルを展開先のサイト、出力ウィンドウに、進行状況を表示します。

図 7 は、パブリッシュ操作が完了した後に、FTP サイト上のファイルを示します。 マークアップ ページのみと、必要なサーバー側とクライアント側のサポート ファイルがアップロードされていることに注意してください。


[![必要なファイルのみが、実稼働環境に公開されました。](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**図 7**: のみ、必要なファイルが、運用環境に公開 ([フルサイズのイメージを表示するをクリックして](deploying-your-site-using-visual-studio-vb/_static/image21.png))


発行 オプションは、Web サイトのコピー ツールよりも小さい微妙ツールです。 Web サイトのコピー ツールでは、ローカルおよびリモート サイト上のファイルを検査し、その違いを参照してくださいを行うことができます、一方、[発行] オプションが提供されませんインターフェイス。 さらに、Web サイトのコピー ツールには、1 回限りの変更、アップロードまたは個々 のファイルを削除することができます。 発行 オプションでは、このような粒度の細かい制御; することはできません。代わりに、発行、*全体*アプリケーションです。 この動作は、その長所と短所を確認します。 プラスの面では、重要なファイルをアップロードするし忘れるされません発行 オプションを使用するときがわかります。 非常に大規模な web サイトのわずかな変更を加えた場合発行オプションを使用してそのページまたは変更されている 2 つを更新することはできませんが、代わりに Visual Studio は、サイト全体を配置中に待機する必要がありますの動作を検討してください。

内容は、運用環境と開発環境によって異なります。 特定のファイルがあるは珍しいことではありません。 キーの例は、アプリケーションの構成ファイル、`Web.config`です。 発行 オプションは無条件の web アプリケーション ファイルをコピーするため、開発環境でのバージョンでの運用環境のカスタマイズされた構成ファイルが上書きされます。 以降のチュートリアルでは、さらにこのトピックの内容について説明し、このような違いが存在する場合は、web アプリケーションを配置するためのヒントを提供します。

## <a name="summary"></a>まとめ

Web サイトを展開するには、開発環境から運用環境に必要なファイルをコピーが含まれます。 前のチュートリアルでは、FileZilla のような FTP クライアントを使用してファイルを転送する方法を示しました。 このチュートリアルは、Visual Studio での 2 つの展開ツールを調べる: Web サイトのコピー ツールと、[発行] オプション。 Web サイトのコピー ツールは、ローカル コンピューターと簡単にアップロードまたは 2 台のコンピューター間でファイルをダウンロードする指定されたリモート コンピューター上のファイルを一覧表示する 2 つのペインを含むインターフェイスがある点で FTP クライアントに似ています。 発行 オプションは、明示的にプロジェクトをコンパイルし、指定した宛先にアプリケーション全体を配置するより矢印の反対のツールです。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [Web サイトのコピー ツールで Web サイトのコピー](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [どの i: Web サイトのコピー ツールを使用して Web サイトを展開](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md)(ビデオ)
- [方法: Web アプリケーション プロジェクトを発行します。](https://msdn.microsoft.com/library/aa983453.aspx)
- [方法: Web サイトを発行します。](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [セットアップと Visual Studio でのデプロイ プロジェクト](https://msdn.microsoft.com/library/wx3b589t.aspx)

>[!div class="step-by-step"]
[前へ](deploying-your-site-using-an-ftp-client-vb.md)
[次へ](common-configuration-differences-between-development-and-production-vb.md)
