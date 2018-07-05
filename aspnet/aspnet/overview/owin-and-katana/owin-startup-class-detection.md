---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: OWIN スタートアップ クラス検出 |Microsoft Docs
author: Praburaj
description: このチュートリアルでは、OWIN スタートアップ クラスが読み込まれるを構成する方法を示します。 OWIN の詳細については、「プロジェクト Katana 概要を参照してください。 このチュートリアルがしています.
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0a4b87192296054bf6aef6c9406c64f19677a061
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828294"
---
<a name="owin-startup-class-detection"></a>OWIN スタートアップ クラス検出
====================
によって[Praburaj Thiagarajan](https://github.com/Praburaj)、 [Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、OWIN スタートアップ クラスが読み込まれるを構成する方法を示します。 OWIN の詳細については、次を参照してください。[プロジェクト Katana の概要を、](an-overview-of-project-katana.md)します。 このチュートリアルの執筆者は、Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )、Praburaj Thiagarajan と Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。
> 
> ## <a name="prerequisites"></a>必須コンポーネント
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>OWIN スタートアップ クラス検出

 すべての OWIN アプリケーションには、アプリケーション パイプラインのコンポーネントを指定するスタートアップ クラスがあります。 (OwinHost、IIS、および IIS Express) を選択したホスティング モデルに応じて、startup クラスを接続するには、ランタイムでさまざまな方法は。 このチュートリアルで示すように、startup クラスは、すべてのホスト アプリケーションで使用できます。 ホスティング ランタイムを使用して、これらのいずれかのアプローチでは、startup クラスを接続します。  

1. **名前付け規則**: Katana という名前のクラスを探します`Startup`アセンブリ名またはグローバル名前空間に一致する名前空間。
2. **OwinStartup 属性**: これは、startup クラスを指定するほとんどの開発者が採用するアプローチです。 次の属性に startup クラスを設定、`TestStartup`クラス、`StartupDemo`名前空間。 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   `OwinStartup`属性は、名前付け規則を上書きします。 この属性で、フレンドリ名を指定することも、ただし、フレンドリ名を使用する必要がありますを使用しても、`appSetting`構成ファイル内の要素。
3. **構成ファイル内の appSetting 要素**:`appSetting`要素をオーバーライド、`OwinStartup`属性と名前付け規則。 複数のスタートアップ クラスがあることができます (を使用して各、`OwinStartup`属性) と構成のスタートアップ クラスは、次のようなマークアップを使用して構成ファイルに読み込まれます。  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   スタートアップ クラスとアセンブリを明示的に指定する次のキーも使用できます。 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   構成ファイルで次の XML のわかりやすいスタートアップ クラスの名前を指定する`ProductionConfiguration`します。  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   上記のマークアップは、次のために使用する必要があります`OwinStartup`フレンドリ名を指定し、属性、`ProductionStartup2`を実行するクラス。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. OWIN スタートアップの検出の追加を無効にする、`appSetting owin:AutomaticAppStartup`の値を持つ`"false"`web.config ファイルにします。

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>OWIN Startup を使用して ASP.NET Web アプリを作成します。

1. 空の Asp.Net web アプリケーションを作成し、名前**StartupDemo**します。 -インストール`Microsoft.Owin.Host.SystemWeb`NuGet パッケージ マネージャーを使用します。 **ツール**メニューの **ライブラリ パッケージ マネージャー**、し**パッケージ マネージャー コンソール**します。 次のコマンドを入力します。  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. OWIN startup クラスを追加します。 Visual Studio 2013 でプロジェクトを右クリックし、選択**クラスの追加**. -、**新しい項目の追加** ダイアログ ボックスに、入力*OWIN*検索フィールドに、および、Startup.cs に名前変更クリックして**追加**します。  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   次回に追加する、 *Owin Startup クラス*はから利用可能な**追加**メニュー。  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   または、プロジェクトを右クリックし、選択**追加**を選択し、**新しい項目の**、クリックして、 **Owin Startup クラス**。  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- 生成されたコードを置き換える、 *Startup.cs*を次のファイル。  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  `app.Use`ラムダ式を使用して、OWIN パイプラインにミドルウェアを指定したコンポーネントを登録します。 この場合、着信要求に応答する前に受信した要求のログ記録を設定しています。 `next`パラメーターは、デリゲート ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [タスク](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) パイプラインの次のコンポーネントにします。 `app.Run`ラムダ式は、パイプラインが受信要求をフックし、応答メカニズムを提供します。
     > [!NOTE]
     > 上記のコードで私たちがコメント アウト、`OwinStartup`という名前のクラスを実行中の規則で属性としている証明書利用者`Startup`.-キーを押して***F5***アプリケーションを実行します。 更新を何回かクリックします。  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  注: このチュートリアルでは、イメージに表示される数値は一致しません数。 ミリ秒文字列は、ページを更新するときに、新しい応答を表示する使用されます。  
  トレース情報を表示、**出力**ウィンドウ。  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>複数のスタートアップ クラスを追加します。

このセクションでは、別の Startup クラスを追加します。 アプリケーションには、複数の OWIN startup クラスを追加できます。 たとえば、開発、テスト、運用環境のスタートアップ クラスを作成する場合があります。

1. 新しい OWIN Startup クラスを作成し、名前`ProductionStartup`します。
2. 生成されたコードを次のコードに置き換えます。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. アプリを実行するコントロールの f5 キーを押します。 `OwinStartup`属性は、運用環境の startup クラスが実行を指定します。  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. もう 1 つの OWIN Startup クラスを作成し、名前`TestStartup`します。
5. 生成されたコードを次のコードに置き換えます。  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   `OwinStartup`上記属性のオーバー ロードを指定します`TestingConfiguration`として、*フレンドリ*Startup クラスの名前。
6. 開く、 *web.config*ファイルを開き、Startup クラスのフレンドリ名を指定する OWIN アプリケーションのスタートアップ キーを追加します。

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. アプリを実行するコントロールの f5 キーを押します。 アプリ設定の要素が優先、され、テスト構成を実行します。  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. 削除、*フレンドリ*名前を`OwinStartup`属性、`TestStartup`クラス。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. OWIN アプリケーションのスタートアップ キーを置き換える、 *web.config*を次のファイル。

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. 元に戻す、 `OwinStartup` Visual Studio によって生成される既定の属性のコードには、各クラスの属性。  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    OWIN アプリケーションのスタートアップ キーを次の各を実行する運用クラスとなります。 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    最後のスタートアップ キーには、スタートアップの構成方法を指定します。 次の OWIN アプリケーションのスタートアップ キーでは、configuration クラスの名前を変更できます。`MyConfiguration`します。

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Owinhost.exe を使用します。

1. Web.config ファイルを次のマークアップに置き換えます。  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   最後のキーが wins この場合のようになります`TestStartup`を指定します。
2. PMC から Owinhost をインストールします。 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. アプリケーション フォルダーに移動します (フォルダーを含む、 *Web.config*ファイル) と入力してコマンド プロンプト。 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   コマンド ウィンドウが表示されます。 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. URL でブラウザーを起動`http://localhost:5000/`します。  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   OwinHost には、上に示したスタートアップ規則が受け入れられます。
5. コマンド ウィンドウで enter キーを押して OwinHost を終了します。
6. `ProductionStartup`クラスを次の OwinStartup 属性のフレンドリ名を指定する追加*ProductionConfiguration*します。

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. コマンド プロンプト」と入力します。 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   運用環境の startup クラスが読み込まれます。  
    ![](owin-startup-class-detection/_static/image9.png)  
   アプリケーションは複数のスタートアップ クラスを備え、この例では、実行時までロードするスタートアップ クラスを遅延が。
8. 次のランタイム スタートアップ オプションをテストします。

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
