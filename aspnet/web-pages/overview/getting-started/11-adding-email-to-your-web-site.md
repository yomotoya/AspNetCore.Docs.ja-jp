---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: "ページ (Razor) サイトの ASP.NET Web から電子メールを送信する |Microsoft ドキュメント"
author: tfitzmac
description: "この章では、web サイトから自動化された電子メール メッセージを送信する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2db94088a04e87c6ab89df26a7a7b0e2a2653a1a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトから電子メールを送信します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) を使用すると、web サイトから電子メール メッセージを送信する方法について説明します。
> 
> 学習する内容。
> 
> - Web サイトから電子メール メッセージを送信する方法です。
> - 電子メール メッセージにファイルを添付する方法。
> 
> これは、アーティクルで導入された ASP.NET の機能です。
> 
> - `WebMail`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> このチュートリアルは、ASP.NET Web Pages 2 でも動作します。


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Web サイトからの電子メール メッセージの送信

あらゆる種類の web サイトから電子メールを送信しなければならない理由があります。 ユーザーに確認メッセージを送信する可能性があります。 か (たとえば、新しいユーザーが登録されている)。 自分宛てに通知を送信する可能性があります。`WebMail`ヘルパーを簡単にすると、電子メールを送信します。

使用する、`WebMail`ヘルパーに渡し、SMTP サーバーにアクセスする必要があります。 (は、SMTP *Simple Mail Transfer Protocol*)。SMTP サーバーは、メール サーバーのみメッセージを受信者のサーバー &#8212; を転送します。電子メールの送信側です。 Web サイトのホスティング プロバイダーを使用する場合、おそらくセットアップした電子メールとできます教えてくれる SMTP サーバー名とは何です。 企業ネットワーク内、使用している場合、管理者または IT 部門通常得られますに使用できる SMTP サーバーに関する情報。 自宅で作業している場合でも、通常の電子メール プロバイダーに SMTP サーバーの名前を確認するユーザーを使用してテストすることできます可能性があります。 通常必要です。

- SMTP サーバーの名前です。
- ポート番号。 これは、ほとんどの場合は 25 です。 ただし、ISP ポート 587 を使用する必要があります。 電子メールで安全な sockets layer (SSL) を使用している場合は、別のポートを必要があります。 電子メール プロバイダーに確認します。
- 資格情報 (ユーザー名とパスワード)。

この手順では、2 つのページを作成します。 最初のページには、入力できるように説明は、テクニカル サポート フォームに入力されている場合、形式があります。 最初のページでは、2 番目のページにその情報を送信します。 2 番目のページでは、コードは、ユーザーの情報を抽出し、電子メール メッセージを送信します。 問題のレポートが受信されたことを確認するメッセージも表示されます。

![[イメージ]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> この例を簡潔にするに、コードを初期化します、`WebMail`使用するページの右側にヘルパー。 ただし、実際の web サイトであるグローバルのファイルに次のように初期化コードを配置することをお勧めを初期化するように、 `WebMail` helper for web サイトのすべてのファイルを使用します。 詳細については、次を参照してください。[サイト全体の動作をカスタマイズする ASP.NET Web Pages の](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers)します。


1. 新しい web サイトを作成します。
2. という名前の新しいページを追加*EmailRequest.cshtml*し、次のマークアップを追加します。 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    注意して、 `action` 、form 要素の属性設定されている*ProcessRequest.cshtml*です。 これは、フォームを現在のページに戻るの代わりにそのページに送信することを意味します。
3. という名前の新しいページを追加*ProcessRequest.cshtml* web サイトにし、次のコードとマークアップを追加します。   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    コードでは、ページに送信されたフォーム フィールドの値を取得します。 次に呼び出し、`WebMail`ヘルパーの`Send`メソッドを作成し、電子メール メッセージを送信します。 この場合、使用する値は、フォームから送信された値と連結されるテキストされています。

    このページのコードは、内部、`try/catch`ブロックします。 何らかの理由で送信しようとすると、メールが動作しない場合 (たとえば、この設定は右側)、内のコード、`catch`ブロックが実行され、設定、`errorMessage`変数をエラーが発生したことにします。 (詳細については`try/catch`ブロックまたは`<text>`タグは、「 [ASP.NET Web ページは、Razor 構文を使用プログラミングの概要](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors))。

    ページの本文に場合、`errorMessage`変数が空 (既定)、ユーザーには、電子メール メッセージが送信されたメッセージが表示されます。 場合、`errorMessage`変数が true の場合、ユーザーが参照されていると、メッセージを送信中にエラー メッセージに設定します。

    エラー メッセージが表示されるページの部分では、追加のテストに注意してください:`if(debuggingFlag)`です。 これは、電子メールを送信する問題がある場合は true に設定できる変数です。 ときに`debuggingFlag`が true の場合、電子メールの送信に問題がある場合、追加のエラー メッセージが表示何でも ASP.NET が報告を電子メール メッセージを送信しようとしたときに表示するとします。 正当な警告、ただし: ASP.NET が電子メール メッセージを送信できない場合に報告されるエラー メッセージをジェネリックにすることができます。 たとえば場合、(たとえば、サーバー名 に、エラーが発生した) ために、ASP.NET は、SMTP サーバーを接続できません、エラーは`Failure sending mail`します。

    > [!NOTE] 
    > 
    > **重要な**例外オブジェクトからエラー メッセージを取得するときに (`ex`コードで)、操作を行います*いない*たままにし、通常では、そのメッセージをユーザーに渡します。 多くの場合、例外オブジェクトには、ユーザーは表示されず、セキュリティの脆弱性をすることができますも情報が含まれます。 理由は次のコードには、変数が含まれています。`debuggingFlag`エラー メッセージを表示するスイッチとして使用されると、既定では、変数を false に設定する理由。 True (そのため、エラー メッセージを表示) するには、その変数を設定する必要があります*のみ*電子メールの送信に関する問題が発生すると、デバッグする必要があります。 問題を修正して、一度設定`debuggingFlag`を false に戻します。

    変更、次のコードに関連する設定を電子メールで送信します。

    - 設定`your-SMTP-host`へのアクセスが SMTP サーバーの名前にします。
    - 設定`your-user-name-here`SMTP サーバーのアカウントのユーザー名にします。
    - 設定`your-account-password`SMTP サーバーのアカウントのパスワードにします。
    - 設定`your-email-address-here`の電子メール アドレス。 これは、メッセージの送信元電子メール アドレスです。 (電子メール プロバイダーによってしない指定できます異なる`From`解決し、として、ユーザー名を使用、`From`アドレスです。)。

    > [!TIP] 
    > 
    > <a id="configuring_email_settings"></a>
    > ### <a name="configuring-email-settings"></a>電子メールの設定を構成します。
    > 
    > SMTP サーバー、ポート番号、およびなどの適切な設定があるかどうかを確認することもあります課題があります。 いくつかのヒントを次に示します。
    > 
    > - SMTP サーバー名は、のようなものでは多くの場合、`smtp.provider.com`または`smtp.provider.net`です。 ただし、ホスティング プロバイダーにサイトを発行する場合、SMTP サーバー名の点にあります`localhost`です。 これは、公開したすると、サイトが、プロバイダーのサーバーで実行されている、電子メール サーバーは、アプリケーションの観点からローカル可能性があるためです。 サーバー名には、この変更は、発行プロセスの一部として SMTP サーバー名を変更する必要がある可能性があります。
    > - ポート番号は、通常は 25 です。 ただし、一部のプロバイダーを使用する必要ポート 587 またはいくつか他のポートです。
    > - 適切な資格情報を使用することを確認します。 ホスティング プロバイダーには、サイトを公開した場合は、プロバイダーが具体的には示されている電子メールには、資格情報を使用します。 これらは、発行に使用する資格情報と異なる可能性があります。
    > - 場合によって資格情報をまったく必要ありません。 個人の ISP を使用して電子メールを送信する場合は、電子メール プロバイダーが、資格情報を知っている可能性があります。 パブリッシュした後は、ローカル コンピューターでテストするとよりも別の資格情報を使用する必要があります。
    > - 設定する必要がある、電子メール プロバイダーには、暗号化が使用されている場合`WebMail.EnableSsl`に`true`です。
4. 実行、 *EmailRequest.cshtml*ブラウザーのページです。 (ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。
5. 自分の名前と問題の説明を入力し、をクリックして、**送信**ボタンをクリックします。 リダイレクトしている、 *ProcessRequest.cshtml*  ページで、メッセージのことを確認して、電子メール メッセージを送信しています。 

    ![[イメージ]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>電子メールを使用してファイルを送信します。

電子メール メッセージに添付されているファイルを送信することもできます。 この手順では、テキスト ファイルと、2 つの HTML ページを作成します。 電子メールの添付ファイルとして、テキスト ファイルを使用します。

1. Web サイトで、新しいテキスト ファイルを追加し、名前を付けます*MyFile.txt*です。
2. 次のテキストをコピーし、ファイルに貼り付けます。 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. という名前のページを作成する*SendFile.cshtml*し、次のマークアップを追加します。 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. という名前のページを作成する*ProcessFile.cshtml*し、次のマークアップを追加します。 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. 変更、次の例のコードに関連する設定を電子メールで送信します。

    - 設定`your-SMTP-host`へのアクセスが SMTP サーバーの名前にします。
    - 設定`your-user-name-here`SMTP サーバーのアカウントのユーザー名にします。
    - 設定`your-email-address-here`の電子メール アドレス。 これは、メッセージの送信元電子メール アドレスです。
    - 設定`your-account-password`SMTP サーバーのアカウントのパスワードにします。
    - 設定`target-email-address-here`の電子メール アドレス。 (ようにする前に、テストが、別のユーザーに電子メールを送信すると通常、送信できますを自分でします。)
6. 実行、 *SendFile.cshtml*ブラウザーのページです。
7. 名前、件名、および添付するテキスト ファイルの名前を入力 (*MyFile.txt*)。
8. [`Submit`] ボタンをクリックします。 前にリダイレクトするするいると、 *ProcessFile.cshtml*  ページで、メッセージを確認するメッセージを送信する電子メール添付ファイルにします。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


- [ASP.NET Web Pages (Razor) トラブルシューティング ガイド](https://go.microsoft.com/fwlink/?LinkId=253001)
- [簡易メール転送プロトコル](https://msdn.microsoft.com/en-us/library/aa480435.aspx)
- [ASP.NET Web ページのサイト全体の動作をカスタマイズします。](https://go.microsoft.com/fwlink/?LinkId=202906)
