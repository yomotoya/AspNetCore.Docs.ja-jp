---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: "ASP.NET Web API の rest ベースの Api を作成 |Microsoft ドキュメント"
author: rick-anderson
description: "近年、HTTP が HTML ページを提供しているだけではなく、明確になりました。 少数 o を使用して、Web Api を構築するための強力なプラットフォームです。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 49dcd86649ceb77cd5a02ebeb5d9d7b11ff4f344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="build-restful-apis-with-aspnet-web-api"></a>ASP.NET Web API の rest ベースの Api をビルドします。
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

> 近年、HTTP が HTML ページを提供しているだけではなく、明確になりました。 さらに、いくつかの単純な概念など、いくつかの動詞 (GET、POST、およびなど) を使用して、Web Api を構築するための強力なプラットフォームではも*Uri*と*ヘッダー*です。 ASP.NET Web API は、一連の HTTP プログラミングを簡略化するコンポーネントです。 ASP.NET MVC ランタイム上に用意されているので Web API は、HTTP の低レベルのトランスポートの詳細を自動的に処理します。 同時に、Web API は必然的に HTTP プログラミング モデルを公開します。 Web API の 1 つの目標が、実際には、*いない*実際に HTTP で抽象化します。 その結果、Web API には、柔軟で簡単に拡張の両方ができます。 このハンズオン ラボでは、Web API を使用して、連絡先のマネージャー アプリケーション用の単純な REST API を構築します。 また、API を使用するクライアントをビルドします。 REST アーキテクチャ スタイルが実証されて HTTP - を活用する効果的な方法は確実にありませんが HTTP の唯一の有効なアプローチです。 連絡先のマネージャーを一覧表示する、追加、およびその他の連絡先を削除する RESTful を公開します。 このラボでは、HTTP、REST の基本的な知識が必要で、、HTML、JavaScript、および jQuery の基本的な知識が必要です。
> 
> > [!NOTE]
> > ASP.NET Web サイトで ASP.NET Web API フレームワークに専用の領域がある[ [https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api)です。 このサイトは引き続き最新情報、サンプル、および Web API に関連するニュース確認して、頻繁を提供するかどうかは、事実上あらゆるデバイスや開発フレームワークに使用できるカスタムの Web Api の作成の手法をさらに掘り下げてたいです。
> > 
> > ASP.NET Web API、ASP.NET MVC 4 に似ていますが、比較的簡単に利用可能な依存性の注入フレームワークのいくつか使用できるため、コント ローラーから、サービス レイヤーを分離する観点からは非常に柔軟です。 Msdn からダウンロード可能な ASP.NET Web API プロジェクトの依存関係の挿入に Ninject を使用する方法を示す良い例がある[ここ](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)です。
> 
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)です。


<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- RESTful Web API を実装します。
- HTML クライアントから API を呼び出す

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)または上位 (読み取り[付録 B](#AppendixB)をインストールする方法について)。

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

**コード スニペットをインストールします。**

便宜上、このラボに沿ったを管理するコードの多くは、Visual Studio のコード スニペットとして利用できます。 実行のコード スニペットをインストールする**.\Source\Setup\CodeSnippets.vsi**ファイル。

このドキュメントの付録を参照することができます、Visual Studio のコード スニペットとその使用方法を学習するに慣れていない場合&quot;[付録 a: を使用してコード スニペット](#AppendixA)&quot;です。

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の手順が含まれます。

1. [手順 1: が読み取り専用の Web API を作成します。](#Exercise1)
2. [手順 2: 読み取り/書き込みの Web API を作成します。](#Exercise2)
3. [手順 3: HTML クライアントから Web API を使用します。](#Exercise3)

> [!NOTE]
> 各演習が組み込まれた、**終了**演習を完了した後に取得する必要があります、結果として得られるソリューションを含むフォルダー。 演習では操作のヘルプを参照する必要がある場合は、このソリューションをガイドとして使用できます。


この演習を完了する時間を推定: **60 分**です。

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>手順 1: が読み取り専用の Web API を作成します。

この演習では、連絡先のマネージャーの読み取り専用の GET メソッドを実装します。

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>タスク 1 - API プロジェクトを作成します。

このタスクでは、新しい ASP.NET web プロジェクト テンプレートを使用して Web API の web アプリケーションを作成します。

1. 実行**Visual Studio 2012 の Express for Web**を参照してください**開始**および種類**VS Express for Web**キーを押します**Enter**です。
2. **ファイル**メニューの **新しいプロジェクト**です。 選択、 **Visual c# |Web**プロジェクトの種類がプロジェクトの種類のツリー ビューから、選択、 **ASP.NET MVC 4 Web Application**プロジェクトの種類。 プロジェクトの設定**名前**に*ContactManager*と**ソリューション名**に*開始*、順にクリックして**[ok]**.

    ![新しい ASP.NET MVC 4.0 Web アプリケーション プロジェクトを作成する](build-restful-apis-with-aspnet-web-api/_static/image1.png "新しい ASP.NET MVC 4.0 Web アプリケーション プロジェクトを作成します。")

    *新しい ASP.NET MVC 4.0 Web アプリケーション プロジェクトを作成します。*
3. ASP.NET MVC 4 プロジェクトの種類 ダイアログ ボックスで、選択、 **Web API**プロジェクトの種類。 **[OK]** をクリックします。

    ![Web API プロジェクトの種類を指定する](build-restful-apis-with-aspnet-web-api/_static/image2.png "Web API プロジェクトの種類を指定します。")

    *Web API プロジェクトの種類を指定します。*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>タスク 2 - 連絡先のマネージャー API コント ローラーの作成

このタスクでは、API のメソッドが存在するコント ローラー クラスを作成します。

1. という名前のファイルを削除**ValuesController.cs**内**コント ローラー**プロジェクトからフォルダーです。
2. 右クリックし、**コント ローラー**クリックし、プロジェクト内のフォルダー**追加 |コント ローラー**コンテキスト メニュー。

    ![新しいコント ローラーをプロジェクトに追加する](build-restful-apis-with-aspnet-web-api/_static/image3.png "新しいコント ローラーをプロジェクトに追加します。")

    *新しいコント ローラーをプロジェクトに追加します。*
3. **コント ローラーの追加**ダイアログ ボックスが表示されたら、選択**空の API コント ローラー**テンプレート メニューからです。 コント ローラー クラスの名前を**ContactController**です。 をクリックし、**追加します。**

    ![コント ローラーの追加 ダイアログを使用して、新しい Web API コント ローラーを作成する](build-restful-apis-with-aspnet-web-api/_static/image4.png "コント ローラーの追加 ダイアログを使用して、新しい Web API コント ローラーを作成するには")

    *コント ローラーの追加 ダイアログを使用して、新しい Web API コント ローラーを作成するには*
4. 次のコードを追加、 **ContactController**です。

    (コード スニペットの*API メソッドを取得する Web API ラボ - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. キーを押して**f5 キーを押して**アプリケーションをデバッグします。 Web API プロジェクトの既定のホーム ページが表示されます。

    ![ASP.NET Web API アプリケーションの既定のホーム ページ](build-restful-apis-with-aspnet-web-api/_static/image5.png "ASP.NET Web API アプリケーションの既定のホーム ページ")

    *ASP.NET Web API アプリケーションの既定のホーム ページ*
6. Internet Explorer のウィンドウでキーを押して、 **F12**キーを開くには、 **Developer Tools**ウィンドウです。 クリックして、**ネットワーク** タブをクリックして、**キャプチャを開始**ウィンドウにネットワーク トラフィックをキャプチャを開始するボタンをクリックします。

    ![[ネットワーク] タブを開き、開始するネットワーク キャプチャ](build-restful-apis-with-aspnet-web-api/_static/image6.png "ネットワーク キャプチャを [ネットワーク] タブを開き、開始しています")

    *[ネットワーク] タブを開き、ネットワーク キャプチャを開始しています*
7. ブラウザーのアドレス バーに URL を追加**api/連絡先**enter キーを押します。 送信の詳細は、ネットワークのキャプチャ ウィンドウに表示されます。 応答の MIME タイプがメモ**アプリケーション/json**です。 これは、既定の出力形式の JSON の方法を示します。

    ![ネットワーク ビューで、Web API 要求の出力を表示する](build-restful-apis-with-aspnet-web-api/_static/image7.png "ネットワーク ビューで、Web API 要求の出力を表示します。")

    *ネットワーク ビューで、Web API 要求の出力を表示します。*

    > [!NOTE]
    > Internet Explorer 10 の既定の動作この時点では、するなかどうかは、ユーザーは保存するか、Web API 呼び出しの結果ストリームを開くように依頼します。 出力は、Web API URL の呼び出しの JSON 結果を含むテキスト ファイルになります。 開発者ツール ウィンドウからの応答のコンテンツを監視するために、ダイアログ ボックスを取り消すことができませんしないでください。
8. をクリックして、**詳細ビューに移動して**の詳細については、この API 呼び出しの応答を表示するボタンをクリックします。

    ![詳細ビューに切り替える](build-restful-apis-with-aspnet-web-api/_static/image8.png "詳細ビューに切り替える")

    *詳細ビューに切り替える*
9. クリックして、**応答本文**タブには、実際の JSON 応答のテキストを表示します。

    ![ネットワーク モニターにテキストを出力、JSON を表示する](build-restful-apis-with-aspnet-web-api/_static/image9.png "ネットワーク モニターにテキストを出力、JSON を表示します。")

    *ネットワーク モニターで、JSON 出力のテキストを表示します。*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>タスク 3 - 連絡先のモデルの作成および連絡先のコント ローラーの拡張

このタスクでは、API のメソッドが存在するコント ローラー クラスを作成します。

1. 右クリックし、**モデル**フォルダーと選択**追加 |クラス.**コンテキスト メニュー。

    ![Web アプリケーションに新しいモデルを追加する](build-restful-apis-with-aspnet-web-api/_static/image10.png "web アプリケーションに新しいモデルを追加します。")

    *Web アプリケーションに新しいモデルを追加します。*
2. **新しい項目の追加**ダイアログ ボックスで、ファイルの名前**Contact.cs**  をクリック**追加します。**

    ![新しい連絡先クラス ファイルを作成する](build-restful-apis-with-aspnet-web-api/_static/image11.png "新しい連絡先クラス ファイルを作成します。")

    *新しい連絡先クラス ファイルを作成します。*
3. 次の強調表示されたコードを追加、**連絡先**クラスです。

    (コード スニペットの*Web API ラボ - Ex01 - 連絡先クラス*)


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. **ContactController**クラス、単語を選択して**文字列**のメソッドの定義で、**取得**メソッド、および、word 型*連絡先*です。 単語を入力すると、インジケーターがという単語の先頭に表示されます**連絡先**です。 いずれかを押しながら、 **Ctrl**キーしピリオド (.) キーを押すかマウスを使用する、コード エディターで、によって自動的に入力アシスタンス ダイアログ ボックスを開く アイコンをクリックして、**を使用して**のモデルのディレクティブ名前空間です。

    ![名前空間の宣言の Intellisense の機能を使用します。](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *名前空間の宣言の Intellisense の機能を使用します。*
5. コードを変更する、**取得**メソッドを連絡先モデル インスタンスの配列を返すようにします。

    (コード スニペットの*連絡先の一覧を返す Web API ラボ - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. キーを押して**f5 キーを押して**ブラウザーで web アプリケーションをデバッグします。 API の応答の出力への変更を表示するには、次の手順を実行します。

    1. ブラウザーが開いたら、一度のキーを押して**F12**開発者ツールになっていないまだ場合。
    2. クリックして、**ネットワーク**タブです。
    3. キーを押して、**キャプチャ開始**ボタンをクリックします。
    4. URL サフィックスを追加して**api/連絡先**キーを押して、アドレス バーの URL に、 **Enter**キー。
    5. キーを押して、**詳細ビューに移動して**ボタンをクリックします。
    6. 選択、**応答本文**タブです。連絡先のインスタンスの配列のシリアル化形式を表す JSON 文字列が表示されます。

    ![複雑な Web API メソッドの呼び出しの出力が JSON にシリアル化された](build-restful-apis-with-aspnet-web-api/_static/image13.png "複雑な Web API メソッドの呼び出しの出力が JSON にシリアル化されました。")

    *複雑な Web API メソッドの呼び出しのシリアル化する JSON の出力*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>タスク 4 - 展開の機能をサービス層

このタスクは、開発者ができるため、実際の作業を行うサービスの再利用性、コント ローラーの層から、サービス機能を分離するが簡単にサービス層に機能を抽出する方法をデモンストレーションします。

1. ソリューションのルートに新しいフォルダーを作成し、名前**Services**です。 これを行うを右クリックして**ContactManager**プロジェクトで、**追加** | **新しいフォルダー**、名前を付けます*Services*です。

    ![作成サービス フォルダー](build-restful-apis-with-aspnet-web-api/_static/image14.png "サービスを作成するフォルダー")

    *サービスのフォルダーを作成します。*
2. 右クリックし、 **Services**フォルダーと選択**追加 |クラス.**コンテキスト メニュー。

    ![サービス フォルダーに新しいクラスを追加する](build-restful-apis-with-aspnet-web-api/_static/image15.png "Services フォルダーに新しいクラスを追加します。")

    *サービスのフォルダーに新しいクラスの追加*
3. ときに、**新しい項目の追加**ダイアログが表示されたら、新しいクラスの名前を**ContactRepository**  をクリック**追加**です。

    ![連絡先リポジトリ サービス層のコードを格納するクラス ファイルを作成する](build-restful-apis-with-aspnet-web-api/_static/image16.png "連絡先リポジトリ サービス層のコードを格納するクラス ファイルを作成します。")

    *連絡先リポジトリ サービス層のコードを格納するクラス ファイルを作成します。*
4. 使用して、追加するディレクティブ、 **ContactRepository.cs**モデル名前空間を含めるファイルです。


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. 次の強調表示されたコードを追加、 **ContactRepository.cs** GetAllContacts メソッドを実装するファイル。

    (コード スニペットの*Web API ラボ - Ex01 - 連絡先リポジトリ*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. 開く、 **ContactController.cs**ファイルがまだ開いていない場合。
7. 次の追加、ファイルの名前空間の宣言セクションにステートメントを使用します。


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. 次の強調表示されたコードを追加、 **ContactController.cs**サービス実装のメンバーが作成できるクラスの残りの部分を使用できるように、リポジトリのインスタンスを表すプライベート フィールドを追加するクラス。

    (コード スニペットの*Web API ラボ - Ex01 - 連絡先コント ローラー*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. 変更、**取得**やすく伝えるためのメソッド、連絡先リポジトリ サービスを使用します。

    (コード スニペットの*リポジトリを使用して、連絡先の一覧を返す Web API ラボ - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. ブレークポイントを配置、 **ContactController**の**取得**メソッドの定義。

    ![連絡先のコント ローラーにブレークポイントを追加する](build-restful-apis-with-aspnet-web-api/_static/image17.png "連絡先のコント ローラーにブレークポイントを追加します。")

    *連絡先のコント ローラーにブレークポイントを追加します。*
11. **F5** キーを押してアプリケーションを実行します。
12. ブラウザーが開かれ、キーを押して**F12**を開くには、開発者ツール。
13. クリックして、**ネットワーク**タブです。
14. クリックして、**キャプチャ開始**ボタンをクリックします。
15. サフィックスを持つアドレス バーの URL を追加**api/連絡先**とキーを押します**Enter** API コント ローラーを読み込めません。
16. Visual Studio 2012 を 1 回だけ中断する必要があります**取得**メソッドの実行を開始します。

    ![Get メソッド内での重大な](build-restful-apis-with-aspnet-web-api/_static/image18.png "Get メソッド内で互換性に影響します。")

    *Get メソッド内で互換性に影響します。*
17. キーを押して**f5 キーを押して**を続行します。
18. 戻る Internet Explorer になっていないフォーカスがある場合。 ネットワーク キャプチャ ウィンドウに注意してください。

    ![ネットワークの Web API の呼び出しの結果を示すインターネット エクスプ ローラー ビュー](build-restful-apis-with-aspnet-web-api/_static/image19.png "ネットワークの Web API の呼び出しの結果を示すインターネット エクスプ ローラー ビュー")

    *Web API の呼び出しの結果を示す Internet Explorer でネットワークの表示*
19. クリックして、**詳細ビューに移動して**ボタンをクリックします。
20. クリックして、**応答本文**タブです。API の呼び出しと、サービス層によって取得された 2 つの連絡先を表す方法の JSON の出力に注意してください。

    ![開発者ツール ウィンドウで、Web API からの JSON 出力を表示する](build-restful-apis-with-aspnet-web-api/_static/image20.png "開発者ツール ウィンドウで、Web API からの JSON 出力を表示します。")

    *開発者ツール ウィンドウで、Web API からの JSON 出力を表示します。*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>手順 2: 読み取り/書き込みの Web API を作成します。

この練習では、POST を実装およびデータ編集機能を有効にする連絡先のマネージャーの PUT メソッドです。

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>タスク 1 - Web API プロジェクトを開く

このタスクでは、ユーザー入力を受け付けることができるように、手順 1 で作成した Web API プロジェクトを強化するために準備します。

1. 実行**Visual Studio 2012 の Express for Web**を参照してください**開始**および種類**VS Express for Web**キーを押します**Enter**です。
2. 開く、**開始**ソリューションにある**ソース/Ex02-ReadWriteWebAPI/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

    1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
    2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
    3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

    > [!NOTE]
    > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
3. 開く、 **Services/ContactRepository.cs**ファイル。

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>タスク 2 - 連絡先リポジトリの実装にデータ持続性の機能を追加します。

このタスクでは、永続化し、ユーザー入力と連絡先の新しいインスタンスをそのまま使用できるように、手順 1 で作成した Web API プロジェクトの ContactRepository クラスが補強されます。

1. 次の定数を追加、 **ContactRepository** web サーバー キャッシュ項目のキー名後でこの手順の名前を表すクラス。

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. コンス トラクターを追加、 **ContactRepository**次のコードを含むです。

    (コード スニペットの*API ラボ - Ex02 - 連絡先リポジトリ コンス トラクターを Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. コードを変更する、 **GetAllContacts**メソッド以下に示すようにします。

    (コード スニペットの*Web API ラボ - Ex02 - すべての連絡先を取得する*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > この例では、デモンストレーションのためには、され、値は同時に、複数のクライアントに利用できるではなく、セッション ストレージ機構または要求記憶域の有効期間を使用できるように、記憶媒体としての web サーバーのキャッシュが使用されます。 Entity Framework、XML ストレージ、またはその他のさまざまなを使用して、web サーバーのキャッシュの代わりに 1 つでした。
4. という名前の新しいメソッドを実装する**SaveContact**を**ContactRepository**の連絡先を保存する作業を実行するクラス。 **SaveContact**メソッドは、1 つを受け取る必要があります**連絡先**パラメーターと戻り値、ブール値の成功または失敗を示すです。

    (コード スニペットの*API ラボ - Ex02 - SaveContact メソッドの実装を Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>手順 3: HTML クライアントから Web API を使用します。

この練習では、Web API を呼び出す HTML クライアントを作成します。 このクライアントは、JavaScript を使用して Web API でのデータ交換を容易になり、HTML マークアップを使用して、web ブラウザーで結果が表示されます。

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>タスク 1 の連絡先を表示するための GUI を提供するインデックス ビューを変更します。

このタスクでは、HTML ブラウザーで既存の連絡先の一覧を表示する要件をサポートする web アプリケーションの既定のインデックス ビューを変更します。

1. 開く**Visual Studio 2012 の Express for Web**がまだ開いていない場合。
2. 開く、**開始**ソリューションにある**ソース/Ex03-ConsumingWebAPI/開始/**フォルダーです。 それ以外の場合、作業を続行できますを使用して、**終了**ソリューションは、前の手順を完了して取得します。

    1. 指定されたを開いた場合**開始**ソリューションでは、いくつか不足している NuGet パッケージをダウンロードする必要がある前にします。 これを行うをクリックして、**プロジェクト**メニュー **NuGet パッケージの管理**です。
    2. **NuGet パッケージの管理**ダイアログ ボックスで、をクリックして**復元**足りないパッケージをダウンロードするためにします。
    3. 最後に、ソリューションをビルドします をクリックして**ビルド** | **ソリューションのビルド**です。

    > [!NOTE]
    > NuGet を使用する利点の 1 つは、ことがない、プロジェクト内のすべてのライブラリを出荷するプロジェクトのサイズを減らすことです。 NuGet Power Tools で Packages.config ファイル内のパッケージのバージョンを指定することによってことができますを初めてのプロジェクトを実行する必要なすべてのライブラリをダウンロードします。 これは、このラボから既存のソリューションを開いた後に、次の手順を実行する必要が理由です。
3. 開く、 **Index.cshtml**にあるファイル**ビュー/ホーム**フォルダーです。
4. Id を持つ div 要素の HTML コードを置き換える**本文**次のコードのように見えるようにします。


    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Web API への HTTP 要求を実行するファイルの下部にある次の Javascript コードを追加します。


    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. 開く、 **ContactController.cs**ファイルがまだ開いていない場合。
7. ブレークポイントを配置、**取得**のメソッド、 **ContactController**クラスです。

    ![API コント ローラーの Get メソッドにブレークポイントを配置する](build-restful-apis-with-aspnet-web-api/_static/image21.png "API コント ローラーの Get メソッドにブレークポイントを配置します。")

    *API コント ローラーの Get メソッドにブレークポイントを配置します。*
8. キーを押して**f5 キーを押して**プロジェクトを実行します。 ブラウザーでは、HTML ドキュメントを読み込みます。

    > [!NOTE]
    > アプリケーションのルート URL を参照していることを確認します。
9. ページが読み込まれ、JavaScript の実行、ブレークポイントにヒットして、コント ローラーでは、コードの実行を一時停止します。

    ![VS Express for Web を使用する Web API の呼び出しにデバッグ](build-restful-apis-with-aspnet-web-api/_static/image22.png "Web の VS Express を使用して Web API の呼び出しにデバッグ")

    *Visual Studio 2012 Express for Web を使用する Web API の呼び出しにデバッグ*
10. キーを押して、ブレークポイントを削除する**f5 キーを押して**または [デバッグ] ツールバーの**続行**ブラウザーでビューの読み込みを続行するにはボタン。 Web API の呼び出しが完了すると、ブラウザーでリスト項目として表示されている呼び出し、Web API から返された連絡先が表示されます。

    ![リスト項目として、ブラウザーに表示される API 呼び出しの結果](build-restful-apis-with-aspnet-web-api/_static/image23.png "リスト項目として、ブラウザーに表示される API 呼び出しの結果")

    *リスト項目として、ブラウザーに表示される API 呼び出しの結果*
11. デバッグを停止します。

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>タスク 2 - 連絡先を作成するための GUI を提供するインデックス ビューを変更します。

このタスクでは引き続き、MVC アプリケーションのインデックス ビューを変更します。 フォームがユーザー入力をキャプチャして、新しい連絡先を作成する Web API に送信できる HTML ページに追加され、GUI から日付を収集する新しい Web API コント ローラー メソッドが作成されます。

1. 開く、 **ContactController.cs**ファイル。
2. という名前のコント ローラー クラスに新しいメソッドを追加**Post**次のコードに示すようにします。

    (コード スニペットの*Web API ラボ - Ex03 - Post メソッド*)


    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. 開く、 **Index.cshtml**がまだ開いていない場合、Visual Studio でのファイルです。
4. 直後に、前のタスクで追加した順序なしリストをファイルに次の HTML コードを追加します。


    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. ドキュメントの下部にあるスクリプト要素内では、データに投稿、Web API HTTP POST 呼び出しを使用する ボタンのクリック イベントを処理する次の強調表示されたコードを追加します。

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. **ContactController.cs**にブレークポイントを配置、 **Post**メソッドです。
7. キーを押して**f5 キーを押して**ブラウザーで、アプリケーションを実行します。
8. 新しい連絡先の名前と Id クリックすると、ページがブラウザーで読み込まれると、入力、**保存**ボタンをクリックします。

    ![ブラウザーでクライアント HTML ドキュメントが読み込まれる](build-restful-apis-with-aspnet-web-api/_static/image24.png "ブラウザーでクライアント HTML ドキュメントが読み込まれました。")

    *ブラウザーでクライアントの HTML ドキュメントが読み込まれる*
9. デバッガー ウィンドウの分割される場合、 **Post**メソッド、プロパティの確認、**にお問い合わせください**パラメーター。 値の形式で入力したデータが一致する必要があります。

    ![クライアントから Web API に送信される、連絡先オブジェクト](build-restful-apis-with-aspnet-web-api/_static/image25.png "連絡先オブジェクトが、クライアントから Web API に送信されます。")

    *クライアントから Web API に送信される、連絡先オブジェクト*
10. ステップ実行するまで、デバッガー内のメソッド、**応答**変数が作成されました。 検査時に、**ローカル**デバッガーのウィンドウで、すべてのプロパティが設定されている表示されます。

    ![次のデバッガーで作成応答](build-restful-apis-with-aspnet-web-api/_static/image26.png "応答を次のデバッガーでの作成")

    *次のデバッガーで作成応答*
11. キーを押す場合**f5 キーを押して** をクリックしてまたは**続行**デバッガーで要求が完了します。 によって格納されている連絡先の一覧に新しい連絡先が追加されて、ブラウザーに切り替えた後、 **ContactRepository**実装します。

    ![ブラウザーが、新しい連絡先インスタンスの作成に成功を反映して](build-restful-apis-with-aspnet-web-api/_static/image27.png "ブラウザーには、新しい連絡先インスタンスの作成に成功が反映されます。")

    *ブラウザーには、新しい連絡先インスタンスの作成に成功が反映されます。*

> [!NOTE]
> Azure の次にこのアプリケーションを展開するさらに、[付録 c: 公開 Web Deploy を使用して ASP.NET MVC 4 アプリケーション](#AppendixC)です。


* * *

<a id="Summary"></a>
## <a name="summary"></a>概要

このラボが導入する、新しい ASP.NET Web API フレームワークしてフレームワークを使用する rest ベースの Web Api の実装。 ここでは、任意の数のメカニズムを使用してデータの永続性を容易にする新しいリポジトリを作成し、ネットワーク上でのサービスをこのラボで例として、単純なものではなくです。 Web API には、HTTP および JSON または XML をサポートする任意の言語で記述された HTML 以外のクライアントからの通信を有効にするなどの追加機能の数がサポートされています。 一般的な web アプリケーションの外部の Web API をホストする機能がもだけでなく、独自のシリアル化形式を作成する機能です。

ASP.NET Web サイトで ASP.NET Web API フレームワークに専用の領域がある[ [https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api)です。 このサイトは引き続き最新情報、サンプル、および Web API に関連するニュース確認して、頻繁を提供するかどうかは、事実上あらゆるデバイスや開発フレームワークに使用できるカスタムの Web Api の作成の手法をさらに掘り下げてたいです。

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>付録 a: コード スニペットの使用

コード スニペットでは、すぐに利用できる必要があるすべてのコードがあります。 ラボ ドキュメントがわかります正確に使用する場合が、次の図に示すようにします。

![Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入する](build-restful-apis-with-aspnet-web-api/_static/image28.png "プロジェクトにコードを挿入する Visual Studio を使ってコード スニペット")

*Visual Studio のコード スニペットを使用して、プロジェクトにコードを挿入するには*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>キーボード (C# の場合のみ) を使用してコード スニペットを追加するには

1. コードを挿入するには、カーソルを置きます。
2. (なし、スペースやハイフン) スニペット名を入力してを起動します。
3. スニペットの名前に一致する IntelliSense 表示を確認します。
4. 正しいスニペットを選択 (または、全体のスニペットの名前を選択するまで」と入力してください)。
5. カーソル位置にスニペットを挿入するには、2 回、Tab キーを押します。

    ![スニペット名を入力する開始](build-restful-apis-with-aspnet-web-api/_static/image29.png "スニペット名の入力を開始")

    *スニペット名の入力を開始します。*

    ![Tab キーを押して、強調表示されているスニペットを選択する](build-restful-apis-with-aspnet-web-api/_static/image30.png "強調表示されているスニペットを選択するキーを押してタブ")

    *Tab キーを押して、強調表示されているスニペットを選択するには*

    ![キーを押して タブで再度と、スニペットが展開](build-restful-apis-with-aspnet-web-api/_static/image31.png "キーを押して タブで再度と、スニペットが展開されます")

    *キーを押して タブで再度と、スニペットが展開されます。*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>(C#、Visual Basic および XML) にマウスを使用してコード スニペットを追加するには

1. コード スニペットを挿入する場所を右クリックします。
2. 選択**スニペットの挿入**続く**マイ コード スニペット**です。
3. クリックして一覧から、関連するスニペットを選択します。

    ![コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックして](build-restful-apis-with-aspnet-web-api/_static/image32.png "コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリック")

    *コード スニペットを挿入し、スニペットの挿入 を選択する場所を右クリックします。*

    ![クリックして一覧から、関連するスニペットを選択](build-restful-apis-with-aspnet-web-api/_static/image33.png "クリックして一覧から、関連するスニペットを選択")

    *クリックして一覧から、関連するスニペットを選択します。*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>付録 b: をインストールする Visual Studio Express 2012 for Web

インストールすることができます**Microsoft Visual Studio Express 2012 for Web**別または&quot;Express&quot;バージョンを使用して、  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . 次の手順を案内するインストールに必要な手順*Visual studio Express 2012 for Web*を使用して*Microsoft Web Platform Installer*です。

1. 移動して[ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)です。 または、既に Web Platform Installer をインストールした場合、開くことができ、製品を検索&quot; *Visual Studio Express 2012 for Web と Azure SDK*&quot;です。
2. をクリックして**を今すぐインストール**です。 ない場合**Web Platform Installer**をダウンロードして、最初にインストールしてリダイレクトされます。
3. 1 回**Web Platform Installer**が開いて、をクリックして**インストール**セットアップを開始します。

    ![Visual Studio Express インストール](build-restful-apis-with-aspnet-web-api/_static/image34.png "Visual Studio Express のインストール")

    *Visual Studio Express をインストールします。*
4. すべての製品のライセンスと条項を読み、クリックして**同意**を続行します。

    ![ライセンス条項に同意](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *ライセンス条項に同意*
5. ダウンロードとインストール プロセスが完了するまで待機します。

    ![インストールの進行状況](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *インストールの進行状況*
6. インストールが完了したらをクリックして**完了**です。

    ![インストールが完了しました](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *インストールが完了しました*
7. をクリックして**終了**Web Platform Installer を閉じます。
8. Visual Studio Express for Web を開きするには、**開始**画面し、書き込みを開始&quot; **VS Express**&quot;、順にクリックして、 **VS Express for Web**並べて表示します。

    ![VS Express Web タイルを](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express Web タイルを*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>付録 c: Web Deploy を使用して ASP.NET MVC 4 アプリケーションを発行します。

この付録では、Azure ポータルから新しい web サイトを作成し、Azure によって提供される Web 配置発行機能を活用して、次の演習では、取得したアプリケーションを公開する方法を示します。

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>タスク 1 - Azure ポータルから新しい Web サイトを作成します。

1. 移動して、 [Azure Management Portal](https://manage.windowsazure.com/)サブスクリプションに関連付けられている Microsoft の資格情報を使用してサインインします。

    > [!NOTE]
    > Azure 無料で 10 個の ASP.NET Web サイトをホストでき、トラフィックの増大に応じて拡大縮小できます。 サインアップする[ここ](http://aka.ms/aspnet-hol-azure)です。

    ![Windows Azure ポータルにログオンする](build-restful-apis-with-aspnet-web-api/_static/image39.png "Windows Azure ポータルにログオンします。")

    *ポータルにログオンします。*
2. をクリックして**新規**コマンド バーでします。

    ![新しい Web サイトを作成する](build-restful-apis-with-aspnet-web-api/_static/image40.png "新しい Web サイトを作成します。")

    *新しい Web サイトを作成します。*
3. をクリックして**コンピューティング** | **Web サイト**です。 選択し、**簡易作成**オプション。 新しい web サイトの利用可能な URL を指定して、をクリックして**Web サイトの作成**です。

    > [!NOTE]
    > Azure は、制御および管理できる、クラウドで実行されている web アプリケーションのホストです。 簡易作成 オプションでは、ポータルの外部から Azure に完了した web アプリケーションを展開することができます。 データベースを設定するための手順は含まれません。

    ![簡易作成を使用して新しい Web サイトを作成する](build-restful-apis-with-aspnet-web-api/_static/image41.png "簡易作成を使用して新しい Web サイトを作成します。")

    *簡易作成を使用して新しい Web サイトを作成します。*
4. 新しいまで待つ**Web サイト**を作成します。
5. Web サイトを作成した後は、下のリンクをクリックして、 **URL**列です。 新しい Web サイトが動作していることを確認してください。

    ![新しい web サイトを参照して](build-restful-apis-with-aspnet-web-api/_static/image42.png "新しい web サイトを参照")

    *新しい web サイトを参照*

    ![Web サイトを実行している](build-restful-apis-with-aspnet-web-api/_static/image43.png "を実行している Web サイト")

    *実行している web サイト*
6. ポータルに戻るし、下にある web サイトの名前をクリックして、**名前**管理ページを表示する列。

    ![Web サイトの管理ページを開く](build-restful-apis-with-aspnet-web-api/_static/image44.png "web サイトの管理ページを開く")

    *Web サイトの管理ページを開く*
7. **ダッシュ ボード**] ページの [、**クイック グランス**セクションで、をクリックして、**発行プロファイルのダウンロード**リンクします。

    > [!NOTE]
    > *発行プロファイル*すべての有効なパブリケーション メソッドごとに Azure に web アプリケーションを発行するために必要な情報が含まれています。 発行プロファイルには、Url、ユーザーの資格情報およびに接続し、各発行メソッドが有効になっているエンドポイントに対して認証するために必要なデータベース文字列が含まれています。 **Microsoft WebMatrix 2**、 **Microsoft Visual Studio Express for Web**と**Microsoft Visual Studio 2012**読み取りのサポートをこれらのプログラムの構成を自動化するプロファイルの発行Azure に web アプリケーションを公開します。

    ![発行プロファイルを web サイトをダウンロードする](build-restful-apis-with-aspnet-web-api/_static/image45.png "発行プロファイルを web サイトをダウンロードします。")

    *発行プロファイルを Web サイトをダウンロードします。*
8. 既知の場所に発行プロファイル ファイルをダウンロードします。 さらにこの練習では、このファイルを使用して、Visual Studio から Azure に web アプリケーションを発行する方法が表示されます。

    ![発行プロファイル ファイルを保存](build-restful-apis-with-aspnet-web-api/_static/image46.png "発行プロファイルの保存")

    *発行プロファイル ファイルを保存します。*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>タスク 2 - データベース サーバーの構成

アプリケーションが SQL Server を使用する場合は、データベースの SQL データベース サーバーを作成する必要があります。 SQL Server を使用しない簡単なアプリケーションを展開する場合は、このタスクをスキップする場合があります。

1. SQL データベース サーバーは、アプリケーション データベースを格納する必要があります。 SQL データベース サーバーを表示するには、Azure 管理ポータルで自分のサブスクリプションから**Sql データベース** | **サーバー** | **サーバーのダッシュ ボード**. 使用して 1 つを作成するには作成されたサーバーがない、**追加**コマンド バーのボタンをクリックします。 注意してください、**サーバー名および URL、管理者のログイン名とパスワード**、次のタスクで使用することができます。 データベースを作成しない、まだと後の段階で作成されます。

    ![SQL データベース サーバーのダッシュ ボード](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL データベース サーバーのダッシュ ボード")

    *SQL データベース サーバーのダッシュ ボード*
2. 次のタスクでは、サーバーの一覧に、ローカル IP アドレスを含める必要があるため、Visual Studio からデータベース接続をテストします**使用できる IP アドレス**です。 実行するには、クリックして**構成**から IP アドレスを選択する**現在のクライアント IP アドレス**に貼り付けます、**開始 IP アドレス**と**終了IPアドレス**のテキスト ボックスをクリックして、 ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png)ボタンをクリックします。

    ![クライアントの IP アドレスを追加します。](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *クライアントの IP アドレスを追加します。*
3. 1 回、**クライアント IP アドレス**が許可される IP アドレスに追加一覧で、をクリックして**保存**変更を確認します。

    ![変更を確認します。](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *変更を確認します。*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>タスク 3 - Web Deploy を使用して ASP.NET MVC 4 アプリケーションの発行

1. ASP.NET MVC 4 ソリューションに戻ります。 **ソリューション エクスプ ローラー**を web サイト プロジェクトを右クリックし、**発行**です。

    ![アプリケーションの発行](build-restful-apis-with-aspnet-web-api/_static/image51.png "アプリケーションの発行")

    *Web サイトの発行*
2. 最初の作業を保存した発行プロファイルをインポートします。

    ![発行プロファイルをインポートする](build-restful-apis-with-aspnet-web-api/_static/image52.png "発行プロファイルをインポートします。")

    *発行プロファイルのインポート*
3. をクリックして**への接続検証**です。 検証が完了したらクリックして**次**です。

    > [!NOTE]
    > 検証が完了すると、接続の検証 ボタンの横に表示される緑色のチェック マークを参照してください。

    ![接続の検証](build-restful-apis-with-aspnet-web-api/_static/image53.png "接続の検証")

    *接続の検証*
4. **設定**] ページの [、**データベース**セクションで、データベース接続のテキスト ボックスの横にあるボタンをクリックして (つまり**DefaultConnection**)。

    ![Web 配置の構成](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web 配置の構成")

    *Web 配置の構成*
5. 次のように、データベースの接続を構成します。

    - **サーバー名**SQL データベース サーバーの URL を使用して、入力、 *tcp:*プレフィックス。
    - **ユーザー名**サーバー管理者のログイン名を入力します。
    - **パスワード**サーバー管理者のログイン パスワードを入力します。
    - たとえば、新しいデータベース名を入力: *MVC4SampleDB*です。

    ![対象の接続文字列を構成する](build-restful-apis-with-aspnet-web-api/_static/image55.png "対象の接続文字列を構成します。")

    *対象の接続文字列を構成します。*
6. 次に、 **[OK]**をクリックします。 データベースをクリックを作成するように求められたら**はい**です。

    ![データベースを作成する](build-restful-apis-with-aspnet-web-api/_static/image56.png "データベース文字列を作成します。")

    *データベースの作成*
7. 接続の既定のテキスト ボックス内では、Windows azure SQL データベースへの接続に使用する接続文字列が表示されます。 その後、 **[次へ]**をクリックします。

    ![SQL データベースを指す接続文字列](build-restful-apis-with-aspnet-web-api/_static/image57.png "SQL データベースを指す接続文字列")

    *SQL データベースを指す接続文字列*
8. **プレビュー** ] ページで [**発行**です。

    ![Web アプリケーションの発行](build-restful-apis-with-aspnet-web-api/_static/image58.png "web アプリケーションの発行")

    *Web アプリケーションの発行*
9. 発行プロセスが終了すると、既定のブラウザーが公開された web サイトを開きます。

    ![アプリケーションが Windows Azure に発行](build-restful-apis-with-aspnet-web-api/_static/image59.png "アプリケーションが Windows Azure に発行")

    *Azure に発行されるアプリケーション*
