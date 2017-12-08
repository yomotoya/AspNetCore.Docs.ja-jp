---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: "OWIN 起動クラス検出 |Microsoft ドキュメント"
author: Praburaj
description: "このチュートリアルでは、OWIN スタートアップ クラスが読み込まれるを構成する方法を示します。 OWIN の詳細については、「プロジェクト Katana 概要を参照してください。 このチュートリアルは次のとおりでした."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: a6ac34307b7558ad13684448f339ca74ade9e997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="owin-startup-class-detection"></a>OWIN 起動クラス検出
====================
によって[Praburaj Thiagarajan](https://github.com/Praburaj)、 [Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、OWIN スタートアップ クラスが読み込まれるを構成する方法を示します。 OWIN の詳細については、次を参照してください。[プロジェクト Katana の概要を、](an-overview-of-project-katana.md)です。 このチュートリアルは、Rick Anderson によって書き込まれました ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )、Praburaj Thiagarajan と Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。
> 
> ## <a name="prerequisites"></a>必須コンポーネント
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>OWIN 起動クラス検出

 すべての OWIN アプリケーションには、アプリケーション パイプラインのコンポーネントを指定したスタートアップ クラスがあります。 スタートアップ クラスを接続するには、ランタイムでさまざまな方法は、ホスト モデルに応じてを選択する (OwinHost、IIS、および IIS Express)。 このチュートリアルで示したスタートアップ クラスは、すべてのホスト アプリケーションで使用できます。 スタートアップ クラスは、ホスティング ランタイムを使用して、これらのいずれかに近づくと接続します。  

1. **名前付け規則**: という名前のクラスを探して Katana`Startup`アセンブリ名またはグローバル名前空間に一致する名前空間にします。
2. **OwinStartup 属性**: スタートアップ クラスを指定するほとんどの開発者が採用するアプローチです。 次の属性はスタートアップ クラス設定、`TestStartup`クラス内で、`StartupDemo`名前空間。 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 `OwinStartup`属性は、名前付け規則を上書きします。 この属性を持つ、フレンドリ名を指定することも、ただし、フレンドリ名を使用する必要がありますを使用しても、`appSetting`構成ファイル内の要素。
3. **構成ファイルの appSetting 要素**:`appSetting`要素よりも優先、`OwinStartup`属性と名前付け規則です。 複数のスタートアップ クラスがあることができます (を使用して各、`OwinStartup`属性) および構成するスタートアップ クラスは、次のようなマークアップを使用して、構成ファイルに読み込まれます。  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 次のキー、スタートアップ クラスとアセンブリを明示的に指定することもできます。 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 構成ファイルで次の XML のわかりやすいスタートアップ クラス名を指定する`ProductionConfiguration`です。  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 次に、上記のマークアップを使用する必要が`OwinStartup`フレンドリ名を指定し、属性、`ProductionStartup2`を実行するクラス。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. OWIN 起動検出の追加を無効にする、`appSetting owin:AutomaticAppStartup`の値を持つ`"false"`web.config ファイルにします。

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>OWIN 起動を使用して ASP.NET Web アプリを作成します。

1. 空の Asp.Net web アプリケーションを作成し、名前**StartupDemo**です。 -インストール`Microsoft.Owin.Host.SystemWeb`NuGet パッケージ マネージャーを使用します。 **ツール**メニューの **ライブラリ パッケージ マネージャー**、し**Package Manager Console**です。 次のコマンドを入力します。  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- OWIN 起動クラスを追加します。 Visual Studio 2013 でプロジェクトを右クリックし、選択**クラスの追加**. -、**新しい項目の追加** ダイアログ ボックスで、入力*OWIN*検索フィールドと、Startup.cs に名前を変更クリックして**追加**です。  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 次回を追加する、 *Owin スタートアップ クラス*になりますから使用可能な**追加**メニュー。  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 または、プロジェクトを右クリックし、選択**追加**選択してから、**新しい項目の**、クリックして、 **Owin スタートアップ クラス**です。  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- 生成されたコードで置き換え、 *Startup.cs*を次のファイル。  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 `app.Use` OWIN パイプラインにミドルウェアを指定したコンポーネントを登録するラムダ式を使用します。 ここでは、受信要求に応答する前に受信した要求のログ記録を設定しています。 `next`パラメーターは、デリゲート ( [Func](https://msdn.microsoft.com/en-us/library/bb534960(v=vs.100).aspx) &lt; [タスク](https://msdn.microsoft.com/en-us/library/dd321424(v=vs.100).aspx) &gt; ) パイプラインの次のコンポーネントにします。 `app.Run`ラムダ式は、着信要求を処理パイプラインをフックし、応答のメカニズムを提供します。
     > [!NOTE]
     > 上記のコードでおがコメント アウトされている、`OwinStartup`という名前のクラスを実行しているの規約の属性としている証明書利用者`Startup`.-キーを押して***f5 キーを押して***アプリケーションを実行します。 更新を何回ヒットします。  
  
    ![](owin-startup-class-detection/_static/image4.png)  
注: このチュートリアルでは、イメージ内の数字と一致しません番号を参照してください。 ミリ秒の文字列を使用して、ページを更新するときに、新しい応答を表示します。  
 トレース情報を表示できます、**出力**ウィンドウです。  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>複数のスタートアップ クラスを追加します。

このセクションでは、別のスタートアップ クラスを追加します。 複数の OWIN スタートアップ クラスは、アプリケーションに追加できます。 たとえば、開発、テスト、運用環境のスタートアップ クラスを作成することができます。

1. OWIN 起動の新しいクラスを作成し、名前`ProductionStartup`です。
2. 生成されたコードを次のコードに置き換えます。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. コントロール f5 キーを押してアプリを実行します。 `OwinStartup`属性は、実行を運用環境のスタートアップ クラスを指定します。  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. OWIN 起動の別のクラスを作成し、名前`TestStartup`です。
5. 生成されたコードを次のコードに置き換えます。  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 `OwinStartup`属性のオーバー ロードが上記で指定`TestingConfiguration`として、*フレンドリ*スタートアップ クラスの名前。
6. 開く、 *web.config*ファイルし、スタートアップ クラスのフレンドリ名を指定する OWIN アプリケーションのスタートアップ キーを追加します。

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. コントロール f5 キーを押してアプリを実行します。 アプリ設定要素には、参照元、およびテスト構成を実行します。  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. 削除、*フレンドリ*から名前、`OwinStartup`属性、`TestStartup`クラスです。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. OWIN アプリケーションのスタートアップ キーを置き換える、 *web.config*を次のファイル。

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. 元に戻す、 `OwinStartup` Visual Studio によって生成される既定の属性コードを各クラスで属性。  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 OWIN アプリケーションのスタートアップ キー下の各を実行する実稼働クラスとなります。 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 最後のスタートアップ キーは、起動時の構成方法を指定します。 次の OWIN アプリケーションのスタートアップ キーには、構成クラスの名前を変更することができます`MyConfiguration`です。

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Owinhost.exe を使用します。

1. Web.config ファイルを次のマークアップに置き換えます。  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 最後のキーが wins それには、このケース`TestStartup`を指定します。
2. PMC から Owinhost をインストールします。 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. アプリケーション フォルダーに移動 (フォルダーを含む、 *Web.config*ファイル) と入力してコマンド プロンプト。 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 コマンド ウィンドウが表示されます。 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. URL でブラウザーを起動`http://localhost:5000/`です。  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 OwinHost には、上に示したスタートアップ規則が無視されます。
5. コマンド ウィンドウで enter キーを押して OwinHost を終了します。
6. `ProductionStartup`クラス、追加のフレンドリ名を指定する次の OwinStartup 属性*ProductionConfiguration*です。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. コマンド プロンプト」と入力します。 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 運用環境のスタートアップ クラスが読み込まれます。  
    ![](owin-startup-class-detection/_static/image9.png)  
 アプリケーションは複数のスタートアップ クラスを持ち、この例では、実行時までロードするスタートアップ クラスを遅延おが指定されています。
8. 次のランタイム スタートアップ オプションをテストします。

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
