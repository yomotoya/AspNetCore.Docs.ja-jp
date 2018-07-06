---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: ページ (Razor) サイトを ASP.NET Web から電子メールを送信する |Microsoft Docs
author: tfitzmac
description: この章では、web サイトから自動化された電子メール メッセージを送信する方法について説明します。
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: e94cba91da43101ef1c058b49be746821bb0fe16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841472"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトから電子メールを送信します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) を使用すると、web サイトから電子メール メッセージを送信する方法について説明します。
> 
> 学習内容。
> 
> - Web サイトから電子メール メッセージを送信する方法。
> - 電子メール メッセージにファイルを添付する方法。
> 
> これは、この記事で導入された ASP.NET の機能です。
> 
> - `WebMail`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも機能します。


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Web サイトから電子メール メッセージの送信

あらゆる種類の web サイトから電子メールを送信しなければならない理由があります。 ユーザーに確認メッセージを送信する可能性があります。 か (たとえば、新しいユーザーが登録されている。) を自分に通知を送信する可能性があります。`WebMail`ヘルパーにより、簡単に電子メールを送信できます。

使用する、`WebMail`ヘルパー、SMTP サーバーにアクセスする必要があります。 (略で SMTP *Simple Mail Transfer Protocol*)。SMTP サーバーはのみ、譲渡先のサーバーにメッセージを転送する電子メール サーバー&#8212;電子メールの送信側になります。 Web サイトのホスティング プロバイダーを使用する場合、おそらくセットアップした電子メールとできますに通知する SMTP サーバー名とは何です。 企業ネットワーク内で作業する場合、管理者または IT 部門通常すれば、使用できる SMTP サーバーに関する情報。 自宅で作業している場合でも、通常の電子メール プロバイダーに SMTP サーバーの名前を確認するユーザーを使用してテストすることできます可能性があります。 通常必要です。

- SMTP サーバーの名前。
- ポート番号。 これは、ほとんどの場合は 25 です。 ただし、お使いの ISP ポート 587 を使用する必要があります。 電子メールをセキュリティで保護された sockets layer (SSL) を使用している場合は、別のポートを必要があります。 電子メール プロバイダーに確認します。
- (ユーザー名、パスワード) の資格情報。

この手順では、2 つのページを作成します。 最初のページには、テクニカル サポート フォームに入力されている場合、ユーザーの説明を入力できるようにする形式があります。 最初のページでは、2 番目のページにその情報を送信します。 2 番目のページでは、コードは、ユーザーの情報を抽出し、電子メール メッセージを送信します。 また、問題のレポートが受信されたことを確認するメッセージも表示されます。

![[イメージ]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> この例を簡潔にするに、コードを初期化します、`WebMail`ヘルパーの右側で使用するページ。 ただし、実際の web サイトであるグローバル ファイルの場合は、このような初期化コードを配置することをお勧めを初期化する、 `WebMail` web サイトのすべてのファイルのヘルパー。 詳細については、次を参照してください。[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers)します。


1. 新しい web サイトを作成します。
2. という名前の新しいページを追加*EmailRequest.cshtml*し、次のマークアップを追加します。 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    注意、`action`フォーム要素の属性設定されている*ProcessRequest.cshtml*します。 つまり、フォームは、現在のページに戻るのではなく、そのページに送信されます。
3. という名前の新しいページを追加*ProcessRequest.cshtml* web サイトにし、次のコードとマークアップを追加します。   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    コードでは、ページに送信されたフォーム フィールドの値を取得します。 呼び出して、`WebMail`ヘルパーの`Send`メソッドを作成し、電子メール メッセージを送信します。 この場合、使用する値は、フォームから送信された値を連結する文字列の構成されます。

    このページのコードが内部では、`try/catch`ブロックします。 いずれかの理由で送信しようとすると、電子メールがうまくいかない場合は (たとえば、この設定は適切な)、コードでは、`catch`ブロックが実行され、設定、`errorMessage`変数をエラーが発生したことにします。 (の詳細については`try/catch`ブロックまたは`<text>`タグは、「 [ASP.NET Web ページは、Razor 構文を使用プログラミングの概要について](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors)。)

    ページの本文の場合、`errorMessage`変数が空 (既定)、ユーザーには、電子メール メッセージが送信されたメッセージが表示されます。 場合、`errorMessage`変数が true の場合、メッセージの送信に問題があったメッセージをユーザーが参照に設定します。

    エラー メッセージを表示するページの部分では、追加のテストに注意してください:`if(debuggingFlag)`します。 これは、電子メールの送信問題がある場合は true に設定できる変数です。 ときに`debuggingFlag`が true の場合、追加のエラー メッセージがあらゆる ASP.NET が報告する電子メール メッセージを送信しようとしたときに表示する表示は電子メールの送信に問題がある場合。 公正な警告、ただし: ASP.NET が電子メール メッセージを送信できない場合に報告するエラー メッセージをジェネリックにすることができます。 たとえば場合、(たとえば、サーバー名 で、エラーが発生した) ために、ASP.NET が SMTP サーバーに接続できない、エラーが`Failure sending mail`します。

    > [!NOTE] 
    > 
    > **重要な**例外オブジェクトからエラー メッセージを取得する (`ex`コードで)、操作を行います*いない*日常的にでは、そのメッセージをユーザーに渡します。 多くの場合、例外オブジェクトには、ユーザーが見るべきでないとセキュリティの脆弱性にもある情報が含まれます。 その理由はこのコードには、変数が含まれています`debuggingFlag`エラー メッセージを表示するスイッチとして使用されると、既定では、変数を false に設定する理由。 True (およびそのため、エラー メッセージを表示) するには、その変数を設定する必要があります*のみ*電子メールの送信に関する問題が発生して、デバッグする必要がある場合。 問題を解決した後は、設定`debuggingFlag`を false に戻します。

    変更、次のコードに関連する設定を電子メールで送信します。

   - 設定`your-SMTP-host`へのアクセスが SMTP サーバーの名前にします。
   - 設定`your-user-name-here`SMTP サーバー アカウントのユーザー名にします。
   - 設定`your-account-password`SMTP サーバー アカウントのパスワードにします。
   - 設定`your-email-address-here`を自分の電子メール アドレス。 これは、メッセージの送信元電子メール アドレスです。 (一部の電子メール プロバイダーから、別の指定は、`From`アドレスし、としてユーザー名を使用、`From`アドレスです)。

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>電子メールの設定を構成します。
     > 
     > SMTP サーバー、ポート番号、およびなどの適切な設定があるかどうかを確認することがありますの課題になります。 いくつかのヒントを次に示します。
     > 
     > - SMTP サーバー名はのような多くの場合、`smtp.provider.com`または`smtp.provider.net`します。 ただし、サイトをホスティング プロバイダーにパブリッシュする場合、SMTP サーバー名その時点であります`localhost`します。 これは、公開した後、プロバイダーのサーバーで、サイトが実行されている場合でも、電子メール サーバーは、アプリケーションの観点からローカル可能性があるためにです。 このサーバー名の変更と、発行のプロセスの一部として、SMTP サーバーの名前を変更する必要があります。
     > - ポート番号は、通常は 25 です。 ただし、一部のプロバイダー使用する必要がポート 587 またはいくつかその他のポート。
     > - 適切な資格情報を使用することを確認します。 ホスティング プロバイダーには、サイトを公開した場合は、プロバイダーは、電子メールが個別に指定された資格情報を使用します。 これらは、パブリッシュに使用する資格情報を異なる可能性があります。
     > - 場合によって資格情報をまったく必要はありません。 個人の ISP を使用して電子メールを送信する場合は、メール プロバイダーには、資格情報既にわかっているかもしれません。 パブリッシュした後は、ローカル コンピューターにテストするときに異なる資格情報を使用する必要があります。
     > - 設定しておく場合は、電子メール プロバイダーは、暗号化を使用して、`WebMail.EnableSsl`に`true`します。
4. 実行、 *EmailRequest.cshtml*ブラウザーでページ。 (内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。
5. 自分の名前と問題の説明を入力し、クリックして、**送信**ボタンをクリックします。 リダイレクトされます、 *ProcessRequest.cshtml*  ページで、メッセージを確認し、電子メール メッセージを送信しています。 

    ![[イメージ]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>電子メールを使用してファイルを送信します。

電子メール メッセージに関連付けられているファイルを送信することもできます。 この手順では、テキスト ファイルと 2 つの HTML ページを作成します。 電子メールの添付ファイルとして、テキスト ファイルを使用します。

1. Web サイトで新しいテキスト ファイルを追加し、名前*MyFile.txt*します。
2. 次のテキストをコピーし、ファイルに貼り付けます。 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. という名前のページを作成する*SendFile.cshtml*し、次のマークアップを追加します。 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. という名前のページを作成する*ProcessFile.cshtml*し、次のマークアップを追加します。 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. 変更、次の例のコードに関連する設定を電子メールで送信します。

    - 設定`your-SMTP-host`にアクセスできる SMTP サーバーの名前にします。
    - 設定`your-user-name-here`SMTP サーバー アカウントのユーザー名にします。
    - 設定`your-email-address-here`を自分の電子メール アドレス。 これは、メッセージの送信元電子メール アドレスです。
    - 設定`your-account-password`SMTP サーバー アカウントのパスワードにします。
    - 設定`target-email-address-here`を自分の電子メール アドレス。 (ようにする前に、他のユーザーには、テスト用に電子メールを送信する、通常、送信できますを自分自身にします。)
6. 実行、 *SendFile.cshtml*ブラウザーでページ。
7. 自分の名前、件名、およびアタッチするテキスト ファイルの名前を入力します (*MyFile.txt*)。
8. [`Submit`] ボタンをクリックします。 リダイレクトする前に、 *ProcessFile.cshtml*  ページで、メッセージを確認するメッセージを送信する電子メール添付ファイルにします。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


- [ASP.NET Web ページ (Razor) トラブルシューティング ガイド](https://go.microsoft.com/fwlink/?LinkId=253001)
- [簡易メール転送プロトコル](https://msdn.microsoft.com/library/aa480435.aspx)
- [ASP.NET Web ページのサイト全体の動作をカスタマイズします。](https://go.microsoft.com/fwlink/?LinkId=202906)
