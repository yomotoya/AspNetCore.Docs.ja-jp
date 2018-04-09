---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: ASP.NET Web Pages (Razor) サイトのサイト全体の動作をカスタマイズする |Microsoft ドキュメント
author: tfitzmac
description: この章では、ページだけではなく、web サイト全体またはフォルダー全体に設定する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 4457318bcf1d2886eb8ed68fdd795eea7905368b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) サイトのサイト全体の動作のカスタマイズ
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトにサイト側の設定のページを作成する方法について説明します。
> 
> 学習する内容。
> 
> - コードが実行を可能にする方法セットは (グローバル値またはヘルパーの設定)、サイト内のすべてのページの値です。
> - コードを実行できるように、フォルダー内のすべてのページの値を設定する方法。
> - ページの前後にコードを実行する方法を読み込みます。
> - 中央のエラー ページにエラーを送信する方法。
> - フォルダー内のすべてのページに認証を追加する方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library (NuGet パッケージ)
>   
> 
> このチュートリアルでは ASP.NET Web Pages 3、また、Visual Studio 2013 (または Visual Studio Express 2013 for Web)、自分以外は、ASP.NET Web Helpers Library を使用できません。


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>ASP.NET Web ページの web サイトのスタートアップ コードを追加します。

ASP.NET Web Pages で作成したコードの多くの個々 のページは、そのページのために必要なすべてのコードを含めることができます。 たとえば、ページは、電子メール メッセージを送信する場合、1 つのページにその操作用のすべてのコードを配置することです。 これは、電子メールを送信するための設定を初期化するコードを含めることができます (つまり、SMTP サーバーの) の電子メール メッセージを送信します。

ただし、状況によっては、サイトのすべてのページの実行前に、いくつかのコードを実行する必要があります。 これは、サイト内の任意の場所で使用できる値を設定するために役立ちます (と呼ばれる*グローバル値*)。たとえば、いくつかのヘルパーには、電子メールの設定やアカウント キーのような値を指定する必要があります。 グローバル値にこれらの設定を保持する便利なことができます。

という名前のページを作成することでこれを行う *\_AppStart.cshtml*サイトのルートにします。 このページが存在する場合は、初めて、サイトの任意のページを要求を実行します。 したがって、グローバル値を設定するコードを実行する適切な場所です。 (ため *\_AppStart.cshtml*アンダー スコア プレフィックスでは、ASP.NET は、ユーザーは、それを直接要求した場合でもに、ブラウザーのページを送信しません)。

次の図に示す方法、  *\_AppStart.cshtml* works のページです。 ページの要求を受け取ったし、場合、これは、いずれかの最初の要求 ページ サイトでは、ASP.NET は、まずかどうか、  *\_AppStart.cshtml*ページが存在します。 内の場合は、コード、  *\_AppStart.cshtml*ページを実行してから、要求されたページを実行します。

![[イメージ]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Web サイトのグローバル値の設定

1. WebMatrix web サイトのルート フォルダーにという名前のファイルを作成する *\_AppStart.cshtml*です。 ファイルは、サイトのルートにする必要があります。
2. 次のように、既存のコンテンツを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    このコードで値を格納する、`AppState`サイト内のすべてのページに自動的に設定されているディクショナリ。 注意して、  *\_AppStart.cshtml*ファイルには、すべてのマークアップはありません。 ページはコードを実行し、最初に要求されたページにリダイレクトします。

    > [!NOTE]
    > コードを配置すると、必ず、  *\_AppStart.cshtml*ファイル。 コードでエラーが発生した場合、  *\_AppStart.cshtml*ファイル、web サイトが起動しません。
3. ルート フォルダーの作成という名前の新しいページ*AppName.cshtml*です。
4. 次のように、既定のマークアップとコードに置き換えます。 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    このコードから値を抽出、`AppState`で設定したオブジェクト、  *\_AppStart.cshtml*ページ。
5. 実行、 *AppName.cshtml*ブラウザーのページです。 (ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。ページには、グローバルの値が表示されます。 

    ![[イメージ]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>ヘルパーの値の設定

適切な使い方、  *\_AppStart.cshtml*ファイルは、サイトで使用して、初期化する必要をするヘルパーの値を設定します。 一般的な例としては、の電子メール設定を`WebMail`ヘルパーとのプライベートおよびパブリック キー、`ReCaptcha`ヘルパー。 上記のような場合は、1 回の値を設定することができます、  *\_AppStart.cshtml*し、いる既にすべてのページで設定、サイトです。

この手順は、設定する方法を示します`WebMail`設定グローバルにします。 (使用の詳細については、`WebMail`ヘルパーに渡しを参照してください[ASP.NET Web Pages サイトを追加する電子メール](../getting-started/11-adding-email-to-your-web-site.md))。

1. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。
2. まだしていない場合、  *\_AppStart.cshtml*ファイル、という名前のファイルを作成、web サイトのルート フォルダーに *\_AppStart.cshtml*です。
3. 次の追加`WebMail`設定を *\_AppStart.cshtml*ファイル。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    変更、次のコードに関連する設定を電子メールで送信します。

   - 設定`your-SMTP-host`へのアクセスが SMTP サーバーの名前にします。
   - 設定`your-user-name-here`SMTP サーバーのアカウントのユーザー名にします。
   - 設定`your-account-password`SMTP サーバーのアカウントのパスワードにします。
   - 設定`your-email-address-here`の電子メール アドレス。 これは、メッセージの送信元電子メール アドレスです。 (電子メール プロバイダーによってしない指定できます異なる`From`解決し、として、ユーザー名を使用、`From`アドレスです。)。

     SMTP 設定の詳細については、次を参照してください[電子メール設定を構成する](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings)アーティクルで[ASP.NET Web Pages (Razor) サイトからの電子メールを送信する](https://go.microsoft.com/fwlink/?LinkID=202899)と[電子メールの送信に関する問題](https://go.microsoft.com/fwlink/?LinkId=253001#email)。で、 [ASP.NET Web Pages (Razor) トラブルシューティング ガイド](https://go.microsoft.com/fwlink/?LinkId=253001)です。
4. 保存、  *\_AppStart.cshtml*ファイルして、閉じます。
5. という名前の新しいページを作成、web サイトのルート フォルダーに*TestEmail.cshtml*です。
6. 次のように、既存のコンテンツを置き換えます。 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. 実行、 *TestEmail.cshtml*ブラウザーのページです。
8. 自分宛てに電子メール メッセージを送信し、フィールドに入力**送信**です。
9. メッセージを取得したかどうかを確認する電子メールを確認します。

この例の重要な部分は、通常は変更しないことの設定 — などの SMTP サーバーと、電子メールの資格情報の名前: で設定されている、  *\_AppStart.cshtml*ファイル。 このように再度設定する各ページで電子メールを送信する必要はありません。 (が何らかの理由をこれらの設定を変更する必要があるを場合を設定できますに個別に、ページにします。)ページで、通常ごとに、受信者の電子メール メッセージの本文などを変更する値を設定するだけです。

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>フォルダー内のファイルの前後にコードを実行します。

使用することと同じように *\_AppStart.cshtml* (前後) を実行するコードを記述するコードを記述する、サイト内のページの実行前に、実行の特定のフォルダー内の任意のページです。 これは、ページの設定、同じレイアウト フォルダー内のすべてのページまたはチェックするため、フォルダー内のページを実行する前に、ユーザーがログインするなどの便利です。

ページの具体的にはフォルダー、コードを作成できますという名前のファイルで *\_PageStart.cshtml*です。 次の図に示す方法、  *\_PageStart.cshtml* works のページです。 ASP.NET が最初のチェック、ページの要求を受け取ったときに、  *\_AppStart.cshtml*ページしを実行します。 ASP.NET があるかどうかを確認してから、  *\_PageStart.cshtml*ページ、および場合を実行します。 これは、後、要求されたページを実行します。

内部、  *\_PageStart.cshtml*  ページで、場所を指定するなどして実行する、要求されたページの処理中に、`RunPage`メソッドです。 これにより、要求されたページの実行前に、コードを実行してその後に再び試みます。 含めない場合`RunPage`、内のすべてのコード *\_PageStart.cshtml*実行してから、要求されたページが自動的に実行されます。

![[イメージ]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET の階層を作成することができます *\_PageStart.cshtml*ファイル。 配置することができます、  *\_PageStart.cshtml*ファイルとサブフォルダーに、サイトのルートにします。 ページが要求されたときに、  *\_PageStart.cshtml*に続けて、最上位レベル (サイトのルート) に最も近い実行ファイル、  *\_PageStart.cshtml*次のファイル要求に到達するまで、フォルダーをサブフォルダー構造の下位に、サブフォルダーには、要求されたページが含まれています。 すべての適用後 *\_PageStart.cshtml*ファイルを実行した場合は、要求されたページ実行します。

たとえば、次の組み合わせがある *\_PageStart.cshtml*ファイルと*Default.cshtml*ファイル。

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

実行すると*/myfolder/default.cshtml*次が表示されます。

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>フォルダー内のすべてのページの初期化コードを実行します。

良い使用 *\_PageStart.cshtml*ファイルは、1 つのフォルダー内のすべてのファイルに対して同じレイアウト ページを初期化します。

1. ルート フォルダーにという名前の新しいフォルダーを作成する*InitPages*です。
2. *InitPages*という名前のファイルの作成、web サイトのフォルダー  *\_PageStart.cshtml*次に、既定のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Web サイトのルートで、という名前のフォルダーを作成する*Shared*です。
4. *Shared*フォルダー、という名前のファイルを作成する *\_Layout1.cshtml*次に、既定のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. *InitPages*フォルダー、という名前のファイルを作成する*Content1.cshtml*既存のコンテンツを次に置き換えます。 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. *InitPages*フォルダー、という別のファイルを作成する*Content2.cshtml*既定のマークアップを次に置き換えます。 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. 実行*Content1.cshtml*ブラウザーでします。 

    ![[イメージ]](18-customizing-site-wide-behavior/_static/image4.jpg)

    ときに、 *Content1.cshtml*ページの実行、  *\_PageStart.cshtml*ファイルのセットを`Layout`され、また`PageData["MyBackground"]`色にします。 *Content1.cshtml*レイアウトと色が適用されます。
8. 表示*Content2.cshtml*ブラウザーでします。 

    レイアウトは同じで、両方のページを使用するため、同じページをレイアウトと色の初期化 *\_PageStart.cshtml*です。

## <a name="using-pagestartcshtml-to-handle-errors"></a>使用して\_PageStart.cshtml エラーを処理するには

別なを使用して、  *\_PageStart.cshtml*ファイルは、いずれかで発生する可能性がプログラミング エラー (例外) を処理する方法を作成する*.cshtml*フォルダー内のページです。 この例では、これを行う 1 つの方法を示します。

1. ルート フォルダーにという名前のフォルダーを作成する*InitCatch*です。
2. *InitCatch*という名前のファイルの作成、web サイトのフォルダー  *\_PageStart.cshtml*次に、既存のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    このコードで指定して実行する、要求されたページ明示的に呼び出すことによって、`RunPage`メソッドの中、`try`ブロックします。 要求されたプログラミング エラーが発生した場合のページ、内のコード、`catch`ブロックが実行されます。 コード ページにリダイレクトここでは、(*Error.cshtml*) し、URL の一部として、エラーが発生したため、ファイルの名前を渡します。 (ページを作成、間もなく。)
3. *InitCatch*という名前のファイルの作成、web サイトのフォルダー *Exception.cshtml*次に、既存のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    この例の目的は、このページで何をしているが意図的に作成するエラーが存在しないデータベース ファイルを開くしようとしています。
4. ルート フォルダーにという名前のファイルを作成する*Error.cshtml*次に、既存のマークアップとコードを置き換えます。 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    このページでは、式で`@Request["source"]`URL 外の値を取得し、それを表示します。
5. ツールバーで、をクリックして**保存**です。
6. 実行*Exception.cshtml*ブラウザーでします。 

    ![[イメージ]](18-customizing-site-wide-behavior/_static/image5.jpg)

    エラーが発生したため*Exception.cshtml*、  *\_PageStart.cshtml*ページにリダイレクト、 *Error.cshtml*ファイルで、メッセージが表示されます。

    例外の詳細については、次を参照してください。 [ASP.NET Web ページは、Razor 構文を使用プログラミングの概要](https://go.microsoft.com/fwlink/?LinkID=251587)です。

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>使用して\_PageStart.cshtml フォルダーへのアクセスを制限するには

使用することも、  *\_PageStart.cshtml*ファイル、フォルダー内のすべてのファイルにアクセスを制限します。

1. WebMatrix で使用する新しい web サイトを作成、**サイトからテンプレート**オプション。
2. 使用可能なテンプレートから選択**スターター サイト**です。
3. ルート フォルダーにという名前のフォルダーを作成する*AuthenticatedContent*です。
4. *AuthenticatedContent*フォルダー、という名前のファイルを作成する *\_PageStart.cshtml*次に、既存のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    コードは、フォルダー内のすべてのファイルがキャッシュされないようにすることで開始します。 (これは、次のユーザーに使用する 1 つのユーザーのキャッシュされたページをたくない、公共のコンピューターのようなシナリオに必要です。)次に、コードでは、ユーザーがサイトにサインイン済みで、フォルダー内のページを表示するかどうかを判断します。 ユーザーがサインインされていない場合、コードは、ログイン ページにリダイレクトします。 ログイン ページは、という名前のクエリ文字列値を含める場合は最初に要求されたページにユーザーを返すことができます`ReturnUrl`です。
5. 新しいページを作成、 *AuthenticatedContent*という名前のフォルダー *Page.cshtml*です。
6. 既定のマークアップを次のように置き換えます。  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. 実行*Page.cshtml*ブラウザーでします。 コードでは、ログイン ページにリダイレクトします。 ログ記録する前に登録する必要があります。 登録されているし、ログに記録した後のページに移動でき、コンテンツを表示できます。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web ページの Razor 構文を使用するプログラミングの概要](https://go.microsoft.com/fwlink/?LinkID=251587)
