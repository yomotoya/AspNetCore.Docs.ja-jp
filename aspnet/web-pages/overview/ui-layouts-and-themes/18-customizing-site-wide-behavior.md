---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: ASP.NET Web Pages (Razor) サイトのサイト全体の動作のカスタマイズ |Microsoft Docs
author: tfitzmac
description: この章では、ページだけではなく、web サイト全体または、フォルダー全体を設定する方法について説明します。
ms.author: aspnetcontent
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 6dcc9e2882e0a0ce28715d5d90b6d27c030f9aea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801957"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) サイトのサイト全体の動作をカスタマイズします。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトでページのサイト側設定する方法について説明します。
> 
> 学習内容。
> 
> - コードを実行できるようにする方法は、サイト内のすべてのページの (グローバル値またはヘルパーの設定) が値セット。
> - コード フォルダー内のすべてのページの値を設定することができますを実行する方法。
> - ページの前後にコードを実行する方法を読み込みます。
> - サーバーの全体のエラー ページにエラーを送信する方法。
> - フォルダー内のすべてのページに認証を追加する方法。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library (NuGet パッケージ)
>   
> 
> また、このチュートリアルは ASP.NET Web ページ 3 と連携し、Visual Studio 2013 (または Visual Studio Express 2013 for Web)、こと以外は、ASP.NET Web Helpers Library を使用できません。


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>ASP.NET Web ページの web サイトのスタートアップ コードを追加します。

ASP.NET Web ページを記述するコードの大部分には、個々 のページは、そのページに必要なすべてのコードを含めることができます。 たとえば、ページは、電子メール メッセージを送信する場合、1 つのページにその操作のすべてのコードを配置することは。 これは、電子メールを送信するための設定を初期化するためにコードを含めることができます (つまり、SMTP サーバーの) と、電子メール メッセージを送信するためです。

ただし、状況によっては、任意のページで実行する前に、いくつかのコードを実行する必要があります。 これは、サイト内の任意の場所で使用できる値を設定するために役立ちます (と呼ばれる*グローバル値*)。たとえば、いくつかのヘルパーには、電子メールの設定またはアカウント キーのような値を指定することが必要です。 グローバル値にこれらの設定を保持する便利なことができます。

という名前のページを作成してこれを行う *\_AppStart.cshtml*サイトのルートにします。 このページが存在する場合は、初めてサイト内の任意のページが要求を実行します。 そのため、これはグローバル値を設定するコードを実行することをお勧めします。 (ため *\_AppStart.cshtml*アンダー スコア プレフィックスでは、ASP.NET は、ユーザーは、それを直接要求した場合でもに、ブラウザーのページを送信しません)。

次の図は、どのように *\_AppStart.cshtml* works ページします。 ページの要求を受信し、場合、これは、いずれかの最初の要求 ページ、サイトでは、ASP.NET で最初に確認かどうかを *\_AppStart.cshtml*ページが存在します。 内の場合は、コード、  *\_AppStart.cshtml*ページを実行し、要求されたページを実行します。

![[イメージ]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Web サイトのグローバル値の設定

1. WebMatrix web サイトのルート フォルダー、という名前のファイルを作成 *\_AppStart.cshtml*します。 ファイルは、サイトのルートでなければなりません。
2. 次のように、既存のコンテンツを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    このコードで値を格納する、`AppState`サイト内のすべてのページに自動的に設定されているディクショナリ。 なお、  *\_AppStart.cshtml*ファイルでは、これでは、すべてのマークアップはありません。 ページはコードを実行し、最初に要求されたページにリダイレクトします。

    > [!NOTE]
    > コードを配置すると、注意、  *\_AppStart.cshtml*ファイル。 コード内でエラーが発生した場合、  *\_AppStart.cshtml*ファイル、web サイトを開始しません。
3. ルート フォルダーに作成するという名前の新しいページ*AppName.cshtml*します。
4. 次のように、既定のマークアップとコードに置き換えます。 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    このコードから値を抽出し、`AppState`で設定するオブジェクト、  *\_AppStart.cshtml*ページ。
5. 実行、 *AppName.cshtml*ブラウザーでページ。 (内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。ページには、グローバルな値が表示されます。 

    ![[イメージ]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>ヘルパーの値の設定

適切な使い方、  *\_AppStart.cshtml*ファイルは、サイトで使用して、初期化する必要があるヘルパーの値を設定します。 一般的な例としては、の電子メール設定、`WebMail`ヘルパーとのプライベートおよびパブリック キー、`ReCaptcha`ヘルパー。 上記のような場合で 1 回の値を設定することができます、  *\_AppStart.cshtml*し、いる既にすべてのページで設定、サイト。

この手順は、設定する方法を示します`WebMail`設定グローバルにします。 (使用の詳細については、`WebMail`ヘルパーを参照してください[ASP.NET Web ページ サイトを追加する電子メール](../getting-started/11-adding-email-to-your-web-site.md))。

1. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。
2. まだ持っていない場合、  *\_AppStart.cshtml*という名前のファイルを作成、web サイトのルート フォルダーにファイルを *\_AppStart.cshtml*します。
3. 次の追加`WebMail`設定を *\_AppStart.cshtml*ファイル。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    変更、次のコードに関連する設定を電子メールで送信します。

   - 設定`your-SMTP-host`へのアクセスが SMTP サーバーの名前にします。
   - 設定`your-user-name-here`SMTP サーバー アカウントのユーザー名にします。
   - 設定`your-account-password`SMTP サーバー アカウントのパスワードにします。
   - 設定`your-email-address-here`を自分の電子メール アドレス。 これは、メッセージの送信元電子メール アドレスです。 (一部の電子メール プロバイダーから、別の指定は、`From`アドレスし、としてユーザー名を使用、`From`アドレスです)。

     SMTP 設定の詳細については、次を参照してください[電子メール設定を構成する](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings)記事[ASP.NET Web Pages (Razor) サイトから電子メールを送信する](https://go.microsoft.com/fwlink/?LinkID=202899)と[メールの送信問題](https://go.microsoft.com/fwlink/?LinkId=253001#email)。で、 [ASP.NET Web Pages (Razor) トラブルシューティング ガイド](https://go.microsoft.com/fwlink/?LinkId=253001)します。
4. 保存、  *\_AppStart.cshtml*ファイルして閉じます。
5. Web サイトのルート フォルダーに作成するという名前の新しいページ*TestEmail.cshtml*します。
6. 次のように、既存のコンテンツを置き換えます。 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. 実行、 *TestEmail.cshtml*ブラウザーでページ。
8. 自分宛てに電子メール メッセージを送信し、フィールドに入力**送信**します。
9. メッセージを取得したかどうかを確認する電子メールを確認します。

この例の重要な部分は通常は変更しない設定 — などの SMTP サーバーと電子メールの資格情報の名前: で設定されて、  *\_AppStart.cshtml*ファイル。 これにより、再度設定する各ページで電子メールを送信する必要はありません。 (が何らかの理由は、これらの設定を変更する必要が場合を設定できますに個別にページにします。)ページで、通常は毎回、受信者、電子メール メッセージの本文などを変更する値を設定するだけです。

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>フォルダー内のファイルの前後にコードを実行します。

同様に使用できること *\_AppStart.cshtml*前に、(と後) で実行されるコードを記述するコードを記述する、サイト内のページの実行前に、実行の特定のフォルダー内の任意のページ。 これは、同じレイアウト ページ、フォルダー内のすべてのページまたは確認するための中、フォルダー内のページを実行する前に、ユーザーがログインする設定などに便利です。

ページの具体的にはフォルダー、コードを作成できますという名前のファイルで *\_PageStart.cshtml*します。 次の図は、どのように *\_PageStart.cshtml* works ページします。 ASP.NET が最初のチェック、ページの要求を受信すると、  *\_AppStart.cshtml*ページしを実行します。 ASP.NET があるかどうかをチェックし、  *\_PageStart.cshtml*ページ、および場合を実行します。 これは、後、要求されたページを実行します。

内で、  *\_PageStart.cshtml*  ページで、場所を指定するなどして実行する、要求されたページの処理中に、`RunPage`メソッド。 これにより、要求されたページの実行前に、コードを実行できますし、その後にもう一度です。 含めない場合`RunPage`、すべてのコードで *\_PageStart.cshtml*実行してから、要求されたページが自動的に実行されます。

![[イメージ]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET では、階層を作成できます。  *\_PageStart.cshtml*ファイル。 配置することができます、  *\_PageStart.cshtml*ファイルとサブフォルダー、サイトのルートにします。 ページが要求されたときに、  *\_PageStart.cshtml*に続けて、最上位レベル (サイトのルート) に最も近い実行ファイル、  *\_PageStart.cshtml*次のファイルサブフォルダー、サブフォルダーの構造を要求されたページを含むフォルダーに到達するまでの下位にあります。 すべての適用後 *\_PageStart.cshtml*ファイルを実行、要求されたページが実行されます。

たとえばの次の組み合わせがある *\_PageStart.cshtml*ファイルと*Default.cshtml*ファイル。

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

実行すると */myfolder/default.cshtml*次を確認します。

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>フォルダー内のすべてのページの初期化コードを実行しています。

良い使用 *\_PageStart.cshtml*ファイルは、1 つのフォルダーですべてのファイルと同じレイアウト ページを初期化します。

1. という名前の新しいフォルダーを作成、ルート フォルダーで*InitPages*します。
2. *InitPages*という名前のファイルを作成、web サイトのフォルダー  *\_PageStart.cshtml*次の既定のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. という名前のフォルダーを作成、web サイトのルートに*Shared*します。
4. *Shared*フォルダー、という名前のファイルを作成する *\_Layout1.cshtml*次の既定のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. *InitPages*フォルダー、という名前のファイルを作成する*Content1.cshtml*次の既存のコンテンツを置き換えます。 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. *InitPages*フォルダー、という別のファイルを作成する*Content2.cshtml*次の既定のマークアップを置き換えます。 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. 実行*Content1.cshtml*ブラウザーでします。 

    ![[イメージ]](18-customizing-site-wide-behavior/_static/image4.jpg)

    ときに、 *Content1.cshtml*ページの実行、  *\_PageStart.cshtml*ファイルのセット`Layout`され、また`PageData["MyBackground"]`色にします。 *Content1.cshtml*レイアウトと色が適用されます。
8. 表示*Content2.cshtml*ブラウザーでします。 

    レイアウトは同じですが、両方のページを使用して、同じページのレイアウトと色で初期化するため *\_PageStart.cshtml*します。

## <a name="using-pagestartcshtml-to-handle-errors"></a>使用して\_PageStart.cshtml エラーを処理するには

別なを使用して、  *\_PageStart.cshtml*ファイルは、いずれかで発生するプログラミング エラー (例外) を処理する方法を作成する *.cshtml*フォルダー内のページ。 この例では、これを行う 1 つの方法を示します。

1. という名前のフォルダーを作成、ルート フォルダーで*InitCatch*します。
2. *InitCatch*という名前のファイルを作成、web サイトのフォルダー  *\_PageStart.cshtml*次の既存のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    このコードで指定して実行する、要求されたページに明示的に呼び出すことによって、`RunPage`メソッド内で、`try`ブロックします。 要求された任意のプログラミング エラーが発生した場合のページの内部のコード、`catch`ブロックが実行されます。 コードをページにリダイレクトするこの例では、(*Error.cshtml*) し、URL の一部として、エラーが発生したファイルの名前を渡します。 (ページを作成します、間もなく。)
3. *InitCatch*という名前のファイルを作成、web サイトのフォルダー *Exception.cshtml*次の既存のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    この例のために、このページで何をしているが意図的に作成エラーが存在しないデータベース ファイルを開こうとしました。
4. という名前のファイルを作成、ルート フォルダーで*Error.cshtml*次の既存のマークアップとコードを置き換えます。 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    このページでは、式で`@Request["source"]`URL から値を取得し、それを表示します。
5. ツールバーで、次のようにクリックします。**保存**します。
6. 実行*Exception.cshtml*ブラウザーでします。 

    ![[イメージ]](18-customizing-site-wide-behavior/_static/image5.jpg)

    エラーが発生したため、 *Exception.cshtml*、  *\_PageStart.cshtml*ページにリダイレクト、 *Error.cshtml*ファイルで、メッセージが表示されます。

    例外の詳細については、次を参照してください。 [ASP.NET Web ページは、Razor 構文を使用プログラミングの概要について](https://go.microsoft.com/fwlink/?LinkID=251587)します。

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>使用して\_PageStart.cshtml フォルダーへのアクセスを制限するには

使用することも、  *\_PageStart.cshtml*フォルダー内のすべてのファイルへのアクセスを制限するファイル。

1. WebMatrix で、使用して新しい web サイトを作成、**テンプレートからサイト**オプション。
2. 使用可能なテンプレートから次のように選択します。**スターター サイト**します。
3. という名前のフォルダーを作成、ルート フォルダーで*AuthenticatedContent*します。
4. *AuthenticatedContent*フォルダー、という名前のファイルを作成する *\_PageStart.cshtml*次の既存のマークアップとコードを置き換えます。 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    コードは、フォルダー内のすべてのファイルがキャッシュされないようにすることで開始します。 (これは、次のユーザーが利用できる 1 つのユーザーのキャッシュされたページをたくない公共のコンピューターのようなシナリオに必要)。次に、コードでは、ユーザーがサイトにサインイン済みで、フォルダー内の各ページを表示するかどうかを判断します。 ユーザーがサインインされていない場合、コードは、ログイン ページにリダイレクトします。 ログイン ページにユーザーという名前のクエリ文字列値を含める場合は最初に要求されたページを返すことができます`ReturnUrl`します。
5. 新しいページを作成、 *AuthenticatedContent*という名前のフォルダー *Page.cshtml*します。
6. 次のように、既定のマークアップに置き換えます。  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. 実行*Page.cshtml*ブラウザーでします。 コードでは、ログイン ページにリダイレクトします。 ログインする前に登録する必要があります。 登録し、ログインして後、は、ページに移動し、その内容を表示します。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース

[ASP.NET Web ページの Razor 構文を使用したプログラミングの概要](https://go.microsoft.com/fwlink/?LinkID=251587)
