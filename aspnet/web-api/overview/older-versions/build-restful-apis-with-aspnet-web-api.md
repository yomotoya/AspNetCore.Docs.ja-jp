---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: ASP.NET Web API を使用した RESTful Api の構築 |Microsoft Docs
author: rick-anderson
description: 近年、HTTP が HTML ページを提供するためだけでないことは明らかになった。 いくつかの o を使用して、Web Api を構築するための強力なプラットフォームではもしています.
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9866b5f75771c633165587ba04e694f72a1e626c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835600"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>ASP.NET Web API を使用した RESTful Api を構築します。
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> 近年、HTTP が HTML ページを提供するためだけでないことは明らかになった。 さらにいくつかの単純な概念など、いくつかの動詞 (GET、POST、およびなど) を使用して、Web Api を構築するための強力なプラットフォームではまた*Uri*と*ヘッダー*します。 ASP.NET Web API は、一連の HTTP プログラミングを簡略化するコンポーネントです。 ASP.NET MVC ランタイムの上に用意されているので Web API は HTTP の低レベルのトランスポートの詳細を自動的に処理します。 同時に、Web API は自然な HTTP プログラミング モデルを公開します。 Web API の 1 つの目標が、実際には、*いない*HTTP の現実で抽象化します。 結果として、Web API は、柔軟で簡単に拡張できます。 このハンズオン ラボでは、連絡先のマネージャーのアプリケーション用の単純な REST API を構築するのに Web API を使用します。 また、API を使用するクライアントをビルドします。 REST のアーキテクチャ スタイルは確かにありませんが、唯一の有効な方法で HTTP する HTTP - を活用する効果的な方法を実証済みです。 連絡先のマネージャーでは、一覧表示、追加、およびその他の連絡先を削除するための RESTful を公開します。 このラボでは、HTTP、REST の基本的な知識を必要とし、HTML、JavaScript、および jQuery の基本的な知識があることを前提します。
> 
> > [!NOTE]
> > ASP.NET Web サイトで ASP.NET Web API フレームワークに専用の領域がある[ https://asp.net/web-api](https://asp.net/web-api)します。 このサイトが引き続き最新の情報、サンプル、および Web API に関連するニュースご確認いただけます。 よくを提供する場合は、事実上あらゆるデバイスや開発フレームワークを使用できるカスタムの Web Api の作成のアートをもっと深く掘り下げてたいです。
> > 
> > ASP.NET Web API、ASP.NET MVC 4 に似ていますが、使用可能な依存関係挿入フレームワークのいくつか非常に簡単に使用できるように、コント ローラーから、サービス層を分離することの点で優れた柔軟性。 Ninject をからダウンロード可能な ASP.NET Web API プロジェクトの依存関係挿入を使用する方法を示しています。 MSDN にある優れたサンプル[ここ](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)します。
> 
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)します。


<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- RESTful Web API を実装します。
- HTML クライアントから API を呼び出す

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 B](#AppendixB)をインストールする方法について)。

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

**コード スニペットをインストールします。**

便宜上、このラボに管理するコードの多くは、Visual Studio コード スニペットとして利用できます。 実行コード スニペットをインストールする **.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 a: を使用してコード スニペット](#AppendixA)&quot;します。

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の演習が含まれます。

1. [手順 1: 読み取り専用の Web API を作成します。](#Exercise1)
2. [手順 2: 読み取り/書き込み Web API の作成します。](#Exercise2)
3. [手順 3: HTML クライアントから Web API を使用します。](#Exercise3)

> [!NOTE]
> 各演習が用意されており、**エンド**演習を完了した後に取得する必要があります、結果として得られるソリューションに含まれているフォルダー。 作業、演習を通じて追加のヘルプが必要な場合は、このソリューションをガイドとして使用できます。


この演習の所要時間を推定: **60 分**します。

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>手順 1: 読み取り専用の Web API を作成します。

この演習では、連絡先のマネージャーの読み取り専用の GET メソッドを実装します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>タスク 1 - API プロジェクトの作成

このタスクでは、Web API の web アプリケーションを作成するのに新しい ASP.NET web プロジェクト テンプレートを使用します。

1. 実行**Visual Studio 2012 の Express for Web**に移動する**開始**と種類**VS Express for Web**キーを押します **」と入力**します。
2. **ファイル**メニューの **新しいプロジェクト**します。 選択、 **(Visual C#) |Web**プロジェクトの種類のツリー ビューからの型を射影し、選択、 **ASP.NET MVC 4 Web アプリケーション**プロジェクトの種類。 プロジェクトの設定**名前**に*ContactManager*と**ソリューション名**に*開始*、 をクリックし、 **ok**.

    ![新しい ASP.NET MVC 4.0 Web アプリケーション プロジェクトを作成する](build-restful-apis-with-aspnet-web-api/_static/image1.png "新しい ASP.NET MVC 4.0 Web アプリケーション プロジェクトを作成します。")

    *新しい ASP.NET MVC 4.0 Web アプリケーション プロジェクトを作成します。*
3. ASP.NET MVC 4 プロジェクトの種類 ダイアログ ボックスで、選択、 **Web API**プロジェクトの種類。 **[OK]** をクリックします。

    ![Web API プロジェクトの種類を指定する](build-restful-apis-with-aspnet-web-api/_static/image2.png "Web API プロジェクトの種類を指定します。")

    *Web API プロジェクトの種類を指定します。*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>タスク 2 - 連絡先のマネージャーの API コント ローラーの作成

このタスクでは、API のメソッドが存在するコント ローラー クラスを作成します。

1. という名前のファイルを削除**ValuesController.cs**内**コント ローラー**プロジェクトからフォルダー。
2. 右クリックし、**コント ローラー**フォルダーをクリックし、プロジェクトで**追加 |コント ローラー**コンテキスト メニュー。

    ![プロジェクトに新しいコント ローラーを追加する](build-restful-apis-with-aspnet-web-api/_static/image3.png "をプロジェクトに新しいコント ローラーの追加")

    *プロジェクトに新しいコント ローラーの追加*
3. **コント ローラーの追加**ダイアログが表示されたら、選択**空の API コント ローラー**テンプレート メニュー。 コント ローラー クラスの名前を**ContactController**します。 をクリックし、**追加します。**

    ![コント ローラーの追加 ダイアログを使用して、新しい Web API コント ローラーを作成する](build-restful-apis-with-aspnet-web-api/_static/image4.png "コント ローラーの追加 ダイアログを使用して、新しい Web API コント ローラーを作成するには")

    *コント ローラーの追加 ダイアログを使用して、新しい Web API コント ローラーを作成するには*
4. 次のコードを追加、 **ContactController**します。

    (コード スニペット - *API メソッドを取得する Web API のラボ - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. キーを押して**F5**アプリケーションをデバッグします。 Web API プロジェクトの既定のホーム ページが表示されます。

    ![ASP.NET Web API アプリケーションの既定のホーム ページ](build-restful-apis-with-aspnet-web-api/_static/image5.png "ASP.NET Web API アプリケーションの既定のホーム ページ")

    *ASP.NET Web API アプリケーションの既定のホーム ページ*
6. Internet Explorer のウィンドウでキーを押して、 **F12**キーを開く、 **Developer Tools**ウィンドウ。 をクリックして、**ネットワーク**タブをクリックし、をクリックし、**キャプチャを開始**ウィンドウにネットワーク トラフィックのキャプチャを開始するボタンをクリックします。

    ![[ネットワーク] タブを開き、開始するネットワーク キャプチャ](build-restful-apis-with-aspnet-web-api/_static/image6.png "ネットワーク キャプチャの [ネットワーク] タブを開き、開始します。")

    *[ネットワーク] タブを開き、ネットワーク キャプチャを開始します。*
7. ブラウザーのアドレス バーで URL を追加**api/連絡先**し、enter キーを押します。 送信の詳細は、ネットワーク キャプチャのウィンドウに表示されます。 応答の MIME の種類は **、application/json**します。 これは、既定の出力形式の JSON の方法を示します。

    ![ネットワーク ビューで、Web API 要求の出力を表示する](build-restful-apis-with-aspnet-web-api/_static/image7.png "ネットワーク ビューで、Web API 要求の出力を表示します。")

    *ネットワーク ビューで、Web API 要求の出力を表示します。*

    > [!NOTE]
    > この時点で、Internet Explorer 10 の既定の動作は、ユーザーは保存または Web API の呼び出しから結果ストリームを開くようなかどうかになります。 出力は、Web API URL の呼び出しの JSON の結果を含むテキスト ファイルになります。 開発者ツール ウィンドウから、応答のコンテンツを視聴できるようにするには、ダイアログを取り消さないでください。
8. をクリックして、**詳細ビューに移動**の詳細については、この API 呼び出しの応答を表示するボタンをクリックします。

    ![詳細ビューに切り替える](build-restful-apis-with-aspnet-web-api/_static/image8.png "詳細ビューに切り替える")

    *詳細ビューに切り替える*
9. をクリックして、**応答本文**タブには、実際の JSON 応答のテキストを表示します。

    ![ネットワーク モニターでのテキストを出力 JSON の表示](build-restful-apis-with-aspnet-web-api/_static/image9.png "ネットワーク モニターでのテキストを出力 JSON の表示")

    *ネットワーク モニターで JSON 出力のテキストを表示します。*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>タスク 3 - 連絡先のモデルを作成して、連絡先のコント ローラーの拡張

このタスクでは、API のメソッドが存在するコント ローラー クラスを作成します。

1. 右クリックし、**モデル**フォルダーと選択**追加 |クラス.** コンテキスト メニュー。

    ![Web アプリケーションに新しいモデルを追加する](build-restful-apis-with-aspnet-web-api/_static/image10.png "web アプリケーションに新しいモデルを追加します。")

    *Web アプリケーションに新しいモデルを追加します。*
2. **新しい項目の追加**ダイアログ ボックスで、ファイルの名前**Contact.cs**クリック**追加します。**

    ![連絡先の新しいクラス ファイルを作成する](build-restful-apis-with-aspnet-web-api/_static/image11.png "Contact の新しいクラス ファイルを作成します。")

    *連絡先の新しいクラス ファイルを作成します。*
3. 次の強調表示されたコードを追加、**連絡先**クラス。

    (コード スニペット - *Web API のラボ - Ex01 - 連絡先クラス*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. **ContactController**クラス、単語を選択する**文字列**のメソッド定義で、**取得**メソッド、および、word 型*連絡先*。 単語の先頭にインジケーターが表示されますで単語を入力すると、**連絡先**します。 いずれかを押し、 **Ctrl**キーしピリオド (.) キーを押すかマウスを使用して自動的に入力する、コード エディターでアシスタンス ダイアログを開く アイコンをクリックして、**を使用して**モデルのディレクティブ名前空間。

    ![名前空間宣言の Intellisense の機能を使用します。](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *名前空間宣言の Intellisense の機能を使用します。*
5. コードを変更する、**取得**メソッドにお問い合わせくださいモデル インスタンスの配列返すようにします。

    (コード スニペット -*連絡先の一覧を返す Web API ラボ - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. キーを押して**F5**ブラウザーで web アプリケーションをデバッグします。 API の応答の出力に加えられた変更を表示するには、次の手順を実行します。

   1. ブラウザーが開いたら、次のようにキーを押して**F12**開発者ツールが開いていないまだ場合。
   2. をクリックして、**ネットワーク**タブ。
   3. キーを押して、**キャプチャ開始**ボタンをクリックします。
   4. URL サフィックスを追加する**api/連絡先**キーを押して、アドレス バーに URL を**Enter**キー。
   5. キーを押して、**詳細ビューに移動**ボタンをクリックします。
   6. 選択、**応答本文**タブ。連絡先のインスタンスの配列のシリアル化された形式を表す JSON 文字列が表示されます。

      ![複雑な Web API のメソッド呼び出しの出力が JSON にシリアル化された](build-restful-apis-with-aspnet-web-api/_static/image13.png "複雑な Web API のメソッド呼び出しの出力が JSON にシリアル化されました。")

      *複雑な Web API のメソッド呼び出しの出力が JSON にシリアル化されました。*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>タスク 4 - 展開の機能をサービス層

このタスクでは、開発者は、サービス機能により、実際に作業を実行するサービスの再利用性、コント ローラーのレイヤーから分離しやすく、サービス層に機能を抽出する方法を示します。

1. ソリューションのルートに新しいフォルダーを作成し、名前**サービス**します。 これを行うには、右クリックして**ContactManager**プロジェクトで、**追加** | **新しいフォルダー**、名前を付けます*サービス*します。

    ![作成サービス フォルダー](build-restful-apis-with-aspnet-web-api/_static/image14.png "サービスを作成するフォルダー")

    *サービスのフォルダーを作成します。*
2. 右クリックし、**サービス**フォルダーと選択**追加 |クラス.** コンテキスト メニュー。

    ![サービス フォルダーに新しいクラスの追加](build-restful-apis-with-aspnet-web-api/_static/image15.png "Services フォルダーに新しいクラスの追加")

    *サービス フォルダーに新しいクラスの追加*
3. ときに、**新しい項目の追加**ダイアログが表示されたら、新しいクラスの名前**ContactRepository**クリック**追加**します。

    ![連絡先リポジトリ サービス層のコードを格納するクラス ファイルを作成する](build-restful-apis-with-aspnet-web-api/_static/image16.png "連絡先リポジトリ サービス層のコードを記述するクラス ファイルを作成します。")

    *連絡先リポジトリ サービス層のコードを格納するクラス ファイルを作成します。*
4. 使用して、追加するディレクティブ、 **ContactRepository.cs**モデル名前空間を追加するファイル。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. 次の強調表示されたコードを追加、 **ContactRepository.cs** GetAllContacts メソッドを実装するファイル。

    (コード スニペット - *Web API のラボ - Ex01 - 連絡先リポジトリ*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. 開く、 **ContactController.cs**ファイルがまだ開いていない場合。
7. 次の追加、ファイルの名前空間の宣言セクションにステートメントを使用します。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. 次の強調表示されたコードを追加、 **ContactController.cs**サービス実装のメンバーが作成できるクラスの残りの部分を使用するために、リポジトリのインスタンスを表すプライベート フィールドを追加するクラス。

    (コード スニペット - *Web API のラボ - Ex01 - 連絡先のコント ローラー*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. 変更、**取得**その it では、メソッド、連絡先リポジトリ サービスを使用します。

    (コード スニペット -*リポジトリを使用して連絡先の一覧を返す Web API ラボ - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. ブレークポイントを設定、 **ContactController**の**取得**メソッドの定義。

   ![連絡先のコント ローラーにブレークポイントを追加する](build-restful-apis-with-aspnet-web-api/_static/image17.png "連絡先のコント ローラーにブレークポイントを追加します。")

   *連絡先のコント ローラーにブレークポイントを追加します。*
11. **F5** キーを押してアプリケーションを実行します。
12. ブラウザーが開き、キーを押して**F12**開発者ツールを開きます。
13. をクリックして、**ネットワーク**タブ。
14. をクリックして、**キャプチャ開始**ボタンをクリックします。
15. サフィックスを持つ、アドレス バーに URL を追加**api/連絡先**キーを押します **」と入力**API コント ローラーを読み込めません。
16. Visual Studio 2012 が 1 回だけ中断する必要があります**取得**メソッドが実行を開始します。

   ![Get メソッド内での重大な](build-restful-apis-with-aspnet-web-api/_static/image18.png "Get メソッド内で互換性に影響します。")

   *Get メソッド内で互換性に影響します。*
17. **F5** キーを押して続行します。
18. インターネット エクスプ ローラーに戻ってになっていないフォーカスがある場合。 ネットワーク キャプチャのウィンドウに注意してください。

    ![Web API 呼び出しの結果を示すインターネット エクスプ ローラーのビューをネットワーク](build-restful-apis-with-aspnet-web-api/_static/image19.png "ネットワーク、Web API 呼び出しの結果を示すインターネット エクスプ ローラーのビュー")

    *Web API の呼び出しの結果を表示する Internet Explorer でネットワークの表示*
19. をクリックして、**詳細ビューに移動**ボタンをクリックします。
20. をクリックして、**応答本文**タブ。API の呼び出しと、サービス層によって取得された 2 つの連絡先を表示する方法の JSON の出力に注意してください。

    ![開発者ツール ウィンドウで、Web API からの JSON 出力を表示する](build-restful-apis-with-aspnet-web-api/_static/image20.png "開発者ツール ウィンドウで、Web API からの JSON 出力の表示")

    *開発者ツール ウィンドウで、Web API からの JSON 出力の表示*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>手順 2: 読み取り/書き込み Web API の作成します。

この演習では、POST を実装したと PUT メソッドのデータ編集機能を有効にする連絡先のマネージャー。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>タスク 1 - Web API プロジェクトを開く

このタスクでは、ユーザー入力を受け入れることができるように、演習 1 で作成した Web API プロジェクトを強化するために準備します。

1. 実行**Visual Studio 2012 の Express for Web**に移動する**開始**と種類**VS Express for Web**キーを押します **」と入力**します。
2. 開く、**開始**ソリューションがある**ソース/Ex02-ReadWriteWebAPI/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
3. 開く、 **Services/ContactRepository.cs**ファイル。

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>タスク 2 - データ永続化機能を連絡先リポジトリの実装に追加します。

このタスクでは、ContactRepository クラスを永続化およびユーザーの入力と連絡先の新しいインスタンスをそのまま使用できるように、演習 1 で作成した Web API プロジェクトを強化します。

1. 次の定数を追加、 **ContactRepository** web サーバー キャッシュ項目キー名この演習の後半の名を表すクラス。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. コンス トラクターを追加、 **ContactRepository**次のコードを格納しています。

    (コード スニペット - *Web API のラボ - Ex02 - 連絡先リポジトリ コンス トラクター*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. コードを変更する、 **GetAllContacts**メソッドを次のようにします。

    (コード スニペット - *Web API のラボ - Ex02 - Get All Contacts*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > この例では、デモンストレーションのためは、値は同時に、複数のクライアントに利用できるではなく、セッションの記憶域メカニズムまたは要求の記憶域の有効期間を使用できるように、ストレージ メディアとして、web サーバーのキャッシュを使用します。 Entity Framework、XML ストレージ、またはその他のさまざまなを使用して、web サーバーのキャッシュの代わりに 1 つでした。
4. という名前の新しいメソッドを実装**SaveContact**を**ContactRepository**の連絡先を保存する作業を行うクラス。 **SaveContact**メソッドは、1 つを実行する必要があります**連絡先**パラメーターおよび戻り値のブール値の成功または失敗を示す値します。

    (コード スニペット - *API ラボ - Ex02 - SaveContact メソッドを実装する Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>手順 3: HTML クライアントから Web API を使用します。

この演習では、Web API を呼び出す HTML クライアントを作成します。 このクライアントは、JavaScript を使用して Web API を使用したデータ交換を容易になり、HTML マークアップを使用して web ブラウザーで結果が表示されます。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>タスク 1 - 連絡先を表示するための GUI を提供する、インデックス ビューを変更します。

このタスクでは、既存の連絡先の一覧を表示する、HTML ブラウザーでの要件をサポートする web アプリケーションの既定のインデックス ビューを変更します。

1. 開く**Visual Studio 2012 の Express for Web**がまだ開いていない場合。
2. 開く、**開始**ソリューションがある**ソース/Ex03-ConsumingWebAPI/開始/** フォルダー。 使用を続ける可能性がありますそれ以外の場合、**エンド**ソリューションは、前の演習を完了して取得します。

   1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うには、クリックして、**プロジェクト**メニュー選択し、 **NuGet パッケージの管理**します。
   2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**不足しているパッケージをダウンロードするには。
   3. 最後をクリックして、ソリューションをビルド**ビルド** | **ソリューションのビルド**します。

      > [!NOTE]
      > NuGet を使用する利点の 1 つは、必要はありません、プロジェクトのすべてのライブラリを配布するプロジェクトのサイズを減らすことです。 With NuGet Power Tools では、Packages.config ファイルでパッケージのバージョンを指定することによってができる初めてのプロジェクトを実行するすべての必要なライブラリをダウンロードします。 これは、なぜこのラボから既存のソリューションを開いた後、次の手順を実行する必要があります。
3. 開く、 **Index.cshtml**ファイルにある**ビュー/ホーム**フォルダー。
4. Id を持つ div 要素内の HTML コードを置き換える**本文**次のように見えるようにします。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Web API への HTTP 要求を実行するファイルの下部にある次の Javascript コードを追加します。

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. 開く、 **ContactController.cs**ファイルがまだ開いていない場合。
7. ブレークポイントを配置、**取得**のメソッド、 **ContactController**クラス。

    ![API コント ローラーの Get メソッドにブレークポイントを配置する](build-restful-apis-with-aspnet-web-api/_static/image21.png "API コント ローラーの Get メソッドにブレークポイントを配置します。")

    *API コント ローラーの Get メソッドにブレークポイントを配置します。*
8. キーを押して**F5**プロジェクトを実行します。 ブラウザーでは、HTML ドキュメントを読み込みます。

    > [!NOTE]
    > アプリケーションのルート URL を参照していることを確認します。
9. ページが読み込まれ、JavaScript の実行、ブレークポイントがヒットして、コント ローラーでは、コードの実行を一時停止します。

    ![VS Express for Web を使用する Web API 呼び出しにデバッグ](build-restful-apis-with-aspnet-web-api/_static/image22.png "Web の VS Express を使用して Web API の呼び出しにデバッグ")

    *Visual Studio 2012 Express for Web を使用する Web API 呼び出しにデバッグ*
10. キーを押して、ブレークポイントを削除**F5**またはデバッグ ツールバーの**続行**ブラウザーで、ビューの読み込みを続行ボタンをクリックします。 Web API の呼び出しが完了すると、ブラウザーでリスト項目として表示されている呼び出し、Web API から返された連絡先が表示されます。

    ![リスト項目として、ブラウザーに表示される API 呼び出しの結果](build-restful-apis-with-aspnet-web-api/_static/image23.png "リスト項目として、ブラウザーに表示される API 呼び出しの結果")

    *リスト項目として、ブラウザーに表示される API 呼び出しの結果*
11. デバッグを停止します。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>タスク 2 - 連絡先を作成するための GUI を提供する、インデックス ビューを変更します。

このタスクでは、MVC アプリケーションのインデックス ビューを変更する続けます。 フォームがユーザー入力をキャプチャして、新しい連絡先を作成する Web API に送信する HTML ページに追加され、GUI から日付を収集する新しい Web API コント ローラー メソッドが作成されます。

1. 開く、 **ContactController.cs**ファイル。
2. という名前のコント ローラー クラスに新しいメソッドを追加**Post**次のコードに示すようにします。

    (コード スニペット - *Web API のラボ - Ex03 の Post メソッド*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. 開く、 **Index.cshtml** Visual Studio でファイルがまだ開いていない場合。
4. 前のタスクで追加した順序なしリストの直後後、ファイルに次の HTML コードを追加します。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. ドキュメントの下部にあるスクリプト要素内では、データを投稿、Web API HTTP POST 呼び出しを使用する ボタンのクリック イベントを処理する次の強調表示されたコードを追加します。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. **ContactController.cs**にブレークポイントを配置、 **Post**メソッド。
7. キーを押して**F5**ブラウザーでアプリケーションを実行します。
8. 新しい連絡先の名前と Id をクリックしますをブラウザーでページが読み込まれると、入力、**保存**ボタンをクリックします。

    ![クライアント HTML ドキュメントがブラウザーに読み込まれた](build-restful-apis-with-aspnet-web-api/_static/image24.png "クライアント HTML ドキュメントがブラウザーに読み込まれました。")

    *クライアントの HTML ドキュメントがブラウザーに読み込まれました。*
9. デバッガー ウィンドウの分割される場合、 **Post**メソッド、プロパティの確認、**にお問い合わせください**パラメーター。 値は、フォームに入力したデータを一致する必要があります。

    ![クライアントから Web API に送信される Contact オブジェクト](build-restful-apis-with-aspnet-web-api/_static/image25.png "Contact オブジェクトがクライアントから Web API に送信されます。")

    *クライアントから Web API に送信される Contact オブジェクト*
10. ステップ実行するまで、デバッガー内のメソッド、**応答**変数が作成されました。 検査時に、**ローカル**デバッガーのウィンドウは、すべてのプロパティが設定されている表示されます。

   ![次のデバッガーで作成応答](build-restful-apis-with-aspnet-web-api/_static/image26.png "応答を次のデバッガーでの作成")

   *次のデバッガーでの作成の応答*
11. キーを押す場合**F5**  をクリックしてまたは**続行**デバッガーで、要求が完了します。 によって格納されている連絡先の一覧に新しい連絡先が追加されて、ブラウザーに切り替えた後、 **ContactRepository**実装します。

   ![ブラウザーが新しい連絡先のインスタンスの作成に成功を反映して](build-restful-apis-with-aspnet-web-api/_static/image27.png "ブラウザーには、新しい連絡先のインスタンスの作成に成功が反映されます。")

   *ブラウザーには、新しい連絡先のインスタンスの作成に成功が反映されます。*

> [!NOTE]
> また、Azure の次に、このアプリケーションを展開できます[付録 c: 発行 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixC)します。


* * *

<a id="Summary"></a>
## <a name="summary"></a>まとめ

新しい ASP.NET Web API フレームワークとフレームワークを使用して、RESTful Web Api の実装にこのラボでは紹介しました。 ここでは、任意の数のメカニズムを使用してデータの永続化を容易にする新しいリポジトリを作成し、このラボでは、例として指定する単純なものではなくそのサービスを接続します。 Web API には、さまざまな HTTP と JSON または XML をサポートする任意の言語で記述された、HTML 以外のクライアントからの通信を有効にするなどの追加機能がサポートされています。 一般的な web アプリケーションの外部で Web API をホストする機能も可能であれば、できるだけでなく、独自のシリアル化形式を作成する機能です。

ASP.NET Web サイトで ASP.NET Web API フレームワークに専用の領域がある[ [ https://asp.net/web-api ](https://asp.net/web-api)](https://asp.net/web-api)します。 このサイトが引き続き最新の情報、サンプル、および Web API に関連するニュースご確認いただけます。 よくを提供する場合は、事実上あらゆるデバイスや開発フレームワークを使用できるカスタムの Web Api の作成のアートをもっと深く掘り下げてたいです。

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>付録 a: コード スニペットの使用

コードのスニペットでは、指先ひとつで必要なすべてのコードがあります。 ラボ ドキュメントがわかりますだけをいつ使用できる、次の図に示すようにします。

![Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入する](build-restful-apis-with-aspnet-web-api/_static/image28.png "プロジェクトにコードを挿入するコード スニペットを Visual Studio の使用")

*Visual Studio コード スニペットを使用して、プロジェクトにコードを挿入するには*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>キーボード (c# のみ) を使用するコード スニペットを追加するには

1. コードを挿入するには、カーソルを置きます。
2. スニペットの名前 (なし、スペースやハイフン) の入力を開始します。
3. スニペットの名前に一致する IntelliSense の表示を確認します。
4. 適切なスニペットを選択します (または全体のスニペットの名前が選択されるまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

    ![スニペットの名前の入力を開始](build-restful-apis-with-aspnet-web-api/_static/image29.png "スニペット名の入力を開始")

    *スニペットの名前の入力を開始します。*

    ![強調表示されているスニペットを選択して Tab キーを押して](build-restful-apis-with-aspnet-web-api/_static/image30.png "キーを押してタブが強調表示されているスニペットを選択するには")

    *Tab キーを押して、強調表示されているスニペットを選択します*

    ![キーを押して タブで再度とスニペットが展開](build-restful-apis-with-aspnet-web-api/_static/image31.png "キーを押して タブで再度とスニペットが展開されます")

    *キーを押して タブで再度とスニペットが展開されます。*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>(C#、Visual Basic および XML) にマウスを使用するコード スニペットを追加するには

1. コード スニペットを挿入するを右クリックします。
2. 選択**スニペットの挿入**続けて**マイ コード スニペット**します。
3. クリックして、一覧から関連するスニペットを選択します。

    ![コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリックして](build-restful-apis-with-aspnet-web-api/_static/image32.png "コード スニペットを挿入し、スニペットの挿入を選択する場所を右クリック")

    *コード スニペットを挿入して、スニペットの挿入先の選択します。*

    ![クリックして、一覧から関連するスニペットを選択](build-restful-apis-with-aspnet-web-api/_static/image33.png "クリックして、一覧から関連するスニペットを選択")

    *クリックして、一覧から関連するスニペットを選択します。*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>付録 b: Installing Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、 **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 次の手順をインストールするために必要な手順をガイドします。 *Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*します。

1. 移動して[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)します。 または、既に Web Platform Installer をインストールした場合を開くことも、製品を検索して、 &quot; <em>Visual Studio Express 2012 for Web と Azure SDK</em>&quot;します。
2. をクリックして**を今すぐインストール**します。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**を開くと、クリックして**インストール**セットアップを開始します。

    ![Visual Studio Express のインストール](build-restful-apis-with-aspnet-web-api/_static/image34.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと使用条件を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *インストールの進行状況*
6. インストールが完了したら、クリックして**完了**します。

    ![インストールが完了しました](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *インストールが完了しました*
7. クリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、 をクリックし、 **VS Express for Web**並べて表示します。

    ![VS Express for Web のタイル](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express for Web のタイル*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 c: Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

この付録では、Azure Portal から新しい web サイトを作成して Azure によって提供される、Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを発行する方法を示します。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>タスク 1 - Azure Portal から新しい Web サイトの作成

1. 移動して、 [Azure 管理ポータル](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Azure を無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増加に応じてスケールできます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)します。

    ![Windows Azure ポータルにログオン](build-restful-apis-with-aspnet-web-api/_static/image39.png "Windows Azure ポータルにログオン")

    *ポータルにログオン*
2. クリックして**新規**コマンド バーにします。

    ![新しい Web サイトを作成する](build-restful-apis-with-aspnet-web-api/_static/image40.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. クリックして**コンピューティング** | **Web サイト**します。 選び**簡易作成**オプション。 新しい web サイトの URL を指定し、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Azure は、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。 簡易作成 オプションを使用すると、ポータルの外部から Azure に完成した web アプリケーションをデプロイできます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](build-restful-apis-with-aspnet-web-api/_static/image41.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**が作成されます。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列。 新しい Web サイトが動作していることを確認します。

    ![新しい web サイトを参照](build-restful-apis-with-aspnet-web-api/_static/image42.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトが実行されている](build-restful-apis-with-aspnet-web-api/_static/image43.png "実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻り、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](build-restful-apis-with-aspnet-web-api/_static/image44.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**ページの**概要**セクションで、、**発行プロファイルのダウンロード**リンク。

    > [!NOTE]
    > *発行プロファイル*を有効になっているパブリケーションの各メソッドの Azure の web アプリケーションを発行するために必要な情報がすべて含まれます。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベースの文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートと、発行プロファイルのこれらのプログラムの構成を自動化するにはAzure に web アプリケーションを公開します。

    ![発行プロファイルのダウンロード web サイト](build-restful-apis-with-aspnet-web-api/_static/image45.png "発行プロファイルの web サイトのダウンロード")

    *発行プロファイルの Web サイトのダウンロード*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの演習ではこのファイルを使用して、Visual Studio から Azure への web アプリケーションを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存する](build-restful-apis-with-aspnet-web-api/_static/image46.png "発行プロファイルを保存しています")

    *発行プロファイル ファイルを保存しています*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合のデータベースは SQL Database サーバーを作成する必要があります。 SQL Server を使用しない単純なアプリケーションをデプロイする場合は、このタスクをスキップする可能性があります。

1. SQL Database サーバーは、アプリケーション データベースを格納する必要があります。 SQL Database サーバーを表示するには、サブスクリプションで Azure 管理ポータルで**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**. 使用して 1 つを作成するには作成したサーバーがない、**追加**コマンド バーのボタンをクリックします。 メモ、**サーバー名および URL、管理者のログイン名とパスワード**次のタスクで使用すると、します。 データベースを作成しない、まだ、後の段階でそれが作成されます。

    ![SQL データベース サーバーのダッシュ ボード](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧で、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**します。 次のようにクリックします**構成**、から IP アドレスを選択して**現在のクライアント IP アドレス**で貼り付けます、**開始 IP アドレス**と**終了 IP アドレス。** テキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png)ボタンをクリックします。

    ![クライアント IP アドレスを追加します。](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *クライアント IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可された IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *変更を確認します。*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 のソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**します。

    ![アプリケーションの発行](build-restful-apis-with-aspnet-web-api/_static/image51.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初のタスクで保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](build-restful-apis-with-aspnet-web-api/_static/image52.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. クリックして**接続の検証**です。 検証が完了したら、クリックして**次**します。

    > [!NOTE]
    > 接続の検証ボタンの横に表示される緑のチェック マークが表示されたら、検証が完了しました。

    ![接続の検証](build-restful-apis-with-aspnet-web-api/_static/image53.png "接続の検証")

    *接続の検証*
4. **設定**ページの**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックします (つまり**DefaultConnection**)。

    ![Web 配置の構成](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web 配置の構成")

    *Web 配置の構成*
5. データベース接続を次のように構成します。

   - **サーバー名**SQL Database サーバーの URL を使用して、入力、 *tcp:* プレフィックス。
   - **ユーザー名**サーバー管理者ログイン名を入力します。
   - **パスワード**サーバー管理者ログイン パスワードを入力します。
   - たとえば、新しいデータベース名を入力: *MVC4SampleDB*します。

     ![ターゲットの接続文字列を構成する](build-restful-apis-with-aspnet-web-api/_static/image55.png "ターゲットの接続文字列を構成します。")

     *ターゲットの接続文字列を構成します。*
6. 次に、 **[OK]** をクリックします。 データベースのクリックを作成するように求められたら**はい**します。

    ![データベースを作成する](build-restful-apis-with-aspnet-web-api/_static/image56.png "データベース文字列を作成します。")

    *データベースの作成*
7. Windows azure SQL Database への接続に使用する接続文字列は、接続の既定のテキスト ボックス内に表示されます。 その後、 **[次へ]** をクリックします。

    ![SQL データベースを指す接続文字列](build-restful-apis-with-aspnet-web-api/_static/image57.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**します。

    ![Web アプリケーションの発行](build-restful-apis-with-aspnet-web-api/_static/image58.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが完了すると、既定のブラウザーが発行した web サイトを開きます。

    ![Windows Azure にアプリケーションが発行される](build-restful-apis-with-aspnet-web-api/_static/image59.png "アプリケーションは Windows Azure に発行")

    *Azure に発行されたアプリケーション*
