---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: "イテレーション 1 – (VB) アプリケーションの作成 |Microsoft ドキュメント"
author: microsoft
description: "最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および D."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: 11d3d4f174207f5370849fdf4517f272b4b6bc6b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="iteration-1--create-the-application-vb"></a>イテレーション 1 – (VB) アプリケーションを作成します。
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> 最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>連絡先管理 ASP.NET MVC アプリケーション (VB) のビルド

この一連のチュートリアルは、連絡先管理アプリケーション全体が開始されてから完了するを構築します。 お問い合わせのマネージャー アプリケーションでは、人のユーザーの一覧については使用すると、連絡先情報の名前、電話番号、電子メール アドレスを格納できます。

私たちは、複数のイテレーションにおける、アプリケーションを構築します。 各イテレーションで、アプリケーション、徐々 に向上します。 この複数のイテレーション アプローチの目的は、各変更の理由を理解するためです。

- イテレーション 1 には、アプリケーションを作成します。 最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。

- イテレーション 2 では、素敵に見えるアプリケーションを作成します。 このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。

- イテレーション 3 - フォーム検証を追加します。 3 番目のイテレーションは、基本フォーム検証を追加します。 おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。 私たちも電子メール アドレスと電話番号を検証します。

- 4: イテレーションは、疎結合アプリケーションを作成します。 この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。 たとえば、リポジトリ パターンと依存関係の挿入のパターンを使用するようにアプリケーションをリファクターします。

- イテレーション #5 - 単体テストを作成します。 5 番目のイテレーションでおやすく、アプリケーションを維持し、単体テストを追加して変更できます。 データ モデル クラスを模擬表示し、コント ローラーと検証ロジックの単体テストをビルドします。

- イテレーション 6 - テスト駆動開発を使用します。 この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。

- イテレーション #7 - Ajax 機能を追加します。 7 番目のイテレーションでお、応答性およびパフォーマンスの向上、アプリケーションの Ajax のサポートを追加することで。

## <a name="this-iteration"></a>このイテレーション

この最初のイテレーションでは、基本的なアプリケーションをビルドします。 目標は、可能な限り最速かつ最も簡単な方法で連絡先のマネージャーを作成します。 後期のイテレーションでは、アプリケーションの設計を向上します。

連絡先のマネージャー アプリケーションは、基本的なデータベースに基づくアプリケーションです。 アプリケーションを使用して、新しい連絡先を作成、既存の連絡先を編集および連絡先を削除することができます。

このイテレーションは、次の手順を実行します。

1. ASP.NET MVC アプリケーション
2. データベースを作成して、連絡先を保存
3. Microsoft の Entity Framework と、データベースのモデル クラスを生成します。
4. コント ローラーのアクションとすべてのデータベースに連絡先の一覧を表示することにより、ビューを作成します。
5. コント ローラーのアクションと、データベースに新しい連絡先を作成することにより、ビューを作成します。
6. コント ローラーのアクションと、データベース内の既存の連絡先を編集することにより、ビューを作成します。
7. コント ローラーのアクションと、データベース内の既存の連絡先を削除することにより、ビューを作成します。

## <a name="software-prerequisites"></a>ソフトウェアの前提条件

ASP.NET MVC アプリケーションでは、Visual Studio 2008 または Visual Web Developer 2008 (Visual Web Developer は、無料版の Visual Studio のすべての Visual Studio の高度な機能は含まれません)、コンピューターにインストールが必要です。 次のアドレスからは、Visual Studio 2008 の試用版または Visual Web Developer のいずれかをダウンロードできます。

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Visual Web Developer と ASP.NET MVC アプリケーションの場合は、Visual Web Developer Service Pack 1 インストールが必要です。 Service Pack 1 なしには、Web アプリケーション プロジェクトを作成することはできません。


ASP.NET MVC フレームワーク。 ASP.NET MVC フレームワークは、次のアドレスからダウンロードできます。

[https://www.asp.net/mvc](../../../index.md)

このチュートリアルでは、Microsoft の Entity Framework を使用して、データベースにアクセスします。 Entity Framework は、.NET Framework 3.5 Service Pack 1 に含まれています。 この service pack は、次の場所からダウンロードできます。

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

これらのダウンロードを 1 つずつのそれぞれを実行する代わりに、Web Platform Installer (Web PI) の利点を実行できます。 Web PI では、次のアドレスからダウンロードできます。

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET MVC プロジェクト

ASP.NET MVC Web アプリケーション プロジェクト。 Visual Studio を起動し、メニュー オプションを選択**ファイル]、[新しいプロジェクト**です。 **新しいプロジェクト**ダイアログが表示されます (図 1)。 選択、 **Web**プロジェクトの種類と**ASP.NET MVC Web アプリケーション**テンプレート。 新しいプロジェクトの名前を付けます*ContactManager*し、[ok] ボタンをクリックします。


上部にあるドロップダウン リストから選択した .NET Framework 3.5 があることを確認の右、**新しいプロジェクト**ダイアログ。 それ以外の場合、t が勝利した ASP.NET MVC Web アプリケーション テンプレートが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**図 01**: [新しいプロジェクト] ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image2.png))


ASP.NET MVC アプリケーション、**単体テスト プロジェクトの作成**ダイアログが表示されます。 このダイアログを使用して、作成し、ASP.NET MVC アプリケーションを作成するときに、単体テスト プロジェクトをソリューションに追加することを指定することができます。 T が勝利したことがイテレーションで単体テストをビルドするには、オプションを選択する必要があります**はい、単体テスト プロジェクトを作成**後のイテレーションで単体テストを追加する予定なのでです。 最初に、新しい ASP.NET MVC プロジェクトを作成するときにテスト プロジェクトを追加することは、ASP.NET MVC プロジェクトが作成された後にテスト プロジェクトを追加するよりもはるかに簡単です。

> [!NOTE] 
> 
> Visual Web Developer では、テスト プロジェクトはサポートされていません、Visual Web Developer を使用するときに単体テスト プロジェクトの作成 ダイアログが得られなかった。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**図 02**:、単体テスト プロジェクトの作成 ダイアログ ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image4.png))


ASP.NET MVC アプリケーションは、Visual Studio ソリューション エクスプ ローラー ウィンドウに表示されます (図 3 を参照してください)。 メニュー オプションを選択してこのウィンドウを開くことができますし、表示されないにソリューション エクスプ ローラー ウィンドウが表示される場合**表示]、[ソリューション エクスプ ローラー**です。 2 つのプロジェクトがソリューションに含まれることに注意してください: ASP.NET MVC プロジェクトとテストのプロジェクトです。 ContactManager は ASP.NET MVC プロジェクトの名前し、ContactManager.Tests はテスト プロジェクトの名前します。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**図 03**: ソリューション エクスプ ローラー ウィンドウ ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>プロジェクトのサンプル ファイルを削除します。

ASP.NET MVC プロジェクト テンプレートには、コント ローラーとビューのサンプル ファイルが含まれています。 新しい ASP.NET MVC アプリケーションを作成する前に、これらのファイルを削除する必要があります。 ソリューション エクスプ ローラー ウィンドウでファイルおよびフォルダーを削除するには、ファイルまたはフォルダーを右クリックし、メニュー オプションを選択して**削除**です。

ASP.NET MVC プロジェクトから、次のファイルを削除する必要があります。

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

また、テスト プロジェクトから次のファイルを削除する必要があります。

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>データベースの作成

連絡先のマネージャー アプリケーションは、データベースに基づく web アプリケーションです。 連絡先情報を格納するデータベースを使用しています。

Microsoft SQL Server、Oracle、MySQL、および IBM DB2 データベースを含む、最新のデータベースと ASP.NET MVC フレームワーク。 このチュートリアルでは、Microsoft SQL Server データベースを使用します。 Visual Studio をインストールするときに、無料版の Microsoft SQL Server データベースである Microsoft SQL Server Express をインストールするオプションが表示されます。

アプリケーションを右クリックして新しいデータベースを作成\_メニュー オプションを選択して、ソリューション エクスプ ローラー ウィンドウで、データ フォルダー**追加、新しい項目の**します。 **新しい項目の追加**ダイアログで、選択、**データ**カテゴリおよび**SQL Server データベース**テンプレート (図 4 を参照してください)。 新しいデータベース ContactManagerDB.mdf 名し、[ok] ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**図 04**: 新しい Microsoft SQL Server Express データベースを作成する ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image8.png))


新しいデータベースを作成すると、アプリでに、データベースが表示されます\_ソリューション エクスプ ローラー ウィンドウで、データ フォルダーです。 サーバー エクスプ ローラー ウィンドウを開き、データベースに接続する ContactManager.mdf ファイルをダブルクリックします。

> [!NOTE] 
> 
> サーバー エクスプ ローラー ウィンドウには、Microsoft Visual Web Developer の場合、データベース エクスプ ローラー ウィンドウが呼び出されます。


サーバー エクスプ ローラー ウィンドウを使用して、データベース テーブル、ビュー、トリガー、ストアド プロシージャなどの新しいデータベース オブジェクトを作成することができます。 [テーブル] フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**です。 データベースのテーブル デザイナーでは、(図 5 を参照) が表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**図 05**: データベース テーブル デザイナー ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image10.png))


次の列を含むテーブルを作成する必要があります。

<a id="0.2_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | int | False |
| FirstName | Nvarchar (50) | False |
| LastName | Nvarchar (50) | False |
| 電話番号 | Nvarchar (50) | False |
| 電子メール | nvarchar (255) | False |


最初の列、Id 列は特殊です。 Id 列と主キー列として Id 列をマークする必要があります。 列が Identity 列である列のプロパティ (図 6 の下部にあるを参照) を展開し、Identity の指定 プロパティまでスクロールを指定します。 設定、 **(Is Identity)**プロパティ値を**はい**です。

主キー列として列を選択し、キーのアイコンが付いたボタンをクリックして列をマークするとします。 列が主キー列としてマークされているキーのアイコンが、列の横に表示されます (図 6 を参照してください)。

テーブルの作成が完了したら、新しいテーブルを保存する、保存ボタン (フロッピー ディスクのアイコンのボタン) をクリックします。 新しいテーブルに名前を付ける*連絡先*です。

連絡先データベース テーブルの作成が完了した後は、テーブルにレコードの一部を追加する必要があります。 サーバー エクスプ ローラー ウィンドウで、Contacts テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**です。 表示されるグリッドで 1 つまたは複数の連絡先を入力します。

## <a name="creating-the-data-model"></a>データ モデルを作成します。

ASP.NET MVC アプリケーションは、モデル、ビュー、およびコント ローラーで構成されます。 前のセクションで作成した、Contacts テーブルを表すモデル クラスを作成して開始します。

このチュートリアルでは、Microsoft の Entity Framework を使用して、データベースからモデル クラスを自動的に生成します。

> [!NOTE] 
> 
> ASP.NET MVC フレームワークは、任意の方法で Microsoft Entity Framework に関連付けられていません。 NHibernate、LINQ to SQL、または ADO.NET を含む別のデータベース アクセス テクノロジでは、ASP.NET MVC を使用できます。


データ モデル クラスを作成する手順に従います。

1. ソリューション エクスプ ローラー] ウィンドウで [モデル] フォルダーを右クリックし [**追加]、[新しい項目の**します。 **新しい項目の追加**(図 6 を参照してください) ダイアログが表示されます。
2. 選択、**データ**カテゴリおよび**ADO.NET エンティティ データ モデル**テンプレート。 データ モデルの名前を付けます*ContactManagerModel.edmx*  をクリックし、**追加**ボタンをクリックします。 Entity Data Model ウィザードでは、(図 7 を参照) が表示されます。
3. **モデルのコンテンツ**手順で、**データベースから生成**(図 7 を参照してください)。
4. **データ接続の選択**ステップ、ContactManagerDB.mdf データベースを選択し、名前を入力*ContactManagerDBEntities*のエンティティの接続設定 (図 8 を参照してください)。
5. **データベース オブジェクトの選択**手順、テーブル (図 9 を参照してください) というラベルの付いたチェック ボックスを選択します。 データ モデル (は 1 つだけで、Contacts テーブル)、データベースに格納されているすべてのテーブルが含まれます。 名前空間を入力*モデル*です。 ウィザードを完了するには、[完了] をクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**図 06**: [新しい項目の追加] ダイアログ ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image12.png))


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**図 07**: モデルのコンテンツの選択 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image14.png))


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**図 08**: データ接続の選択 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image16.png))


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**図 09**: データベース オブジェクトの選択 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image18.png))


Entity Data Model ウィザードを完了すると、Entity Data Model デザイナーが表示されます。 デザイナーでは、モデル化されている各テーブルに対応するクラスが表示されます。 連絡先をという名前の 1 つのクラスが表示されます。

Entity Data Model ウィザードでは、データベース テーブル名に基づくクラス名を生成します。 ほとんどの場合、ウィザードによって生成されたクラスの名前を変更する必要があります。 デザイナーで連絡先クラスを右クリックし、メニュー オプションを選択**の名前を変更**です。 (単数) の連絡先に連絡先 (複数) から、クラスの名前を変更します。 クラス名を変更した後、クラス図 10 のようになります。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**図 10**: 連絡先クラス ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image20.png))


この時点では、このデータベース モデルを作成しました。 連絡先クラスを使用して、データベース内の特定の連絡先レコードを表すおできます。

## <a name="creating-the-home-controller"></a>Home コント ローラーを作成します。

次の手順では、Home コント ローラーを作成します。 Home コント ローラーは、ASP.NET MVC アプリケーションで呼び出される既定のコント ローラーです。

Home コント ローラー クラスを作成するには、ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、メニュー オプションを選択する**追加、コント ローラー** (図 11 を参照してください)。 チェック ボックスに注意してください**アクション メソッドの作成、更新、および詳細シナリオを追加**です。 クリックする前に、このチェック ボックスがオンになっていることを確認、**追加**ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**図 11**: Home コント ローラーを追加する ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image22.png))


Home コント ローラーを作成するときに、1 のリストにクラスを取得します。

**1 - Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>連絡先の一覧を表示します。

連絡先データベース テーブルにレコードを表示するのには、Index() アクションと、インデックス ビューを作成する必要があります。

Home コント ローラーには、既に Index() アクションが含まれています。 このメソッドを一覧表示する 2 のように変更が必要です。

**2 - Controllers\HomeController.vb を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

通知を一覧表示する 2 で、ホーム コント ローラー クラスにというプライベート フィールドが含まれている\_エンティティです。 \_エンティティ フィールドは、データ モデルからエンティティを表します。 使用して、\_エンティティがデータベースと通信するフィールドです。

Index() メソッドでは、連絡先データベース テーブルからすべての連絡先を表すビューを返します。 式\_エンティティです。ContactSet.ToList() は、ジェネリック リストとして連絡先の一覧を返します。

これまで、インデックスのコント ローラーを作成したら次に、インデックス ビューを作成する必要があります。 インデックス ビューを作成する前に、メニュー オプションを選択してアプリケーションをコンパイルします**ビルド、ソリューションのビルド**です。 常に表示されるモデル クラスの一覧の順序でビューを追加する前にプロジェクトをコンパイルする必要があります、**ビューの追加**ダイアログ。

Index() メソッドを右クリックし、メニュー オプションを選択して、インデックス ビューを作成する**ビューの追加**(図 12 を参照してください)。 このメニュー オプションを選択すると開きます、**ビューの追加**ダイアログ (図 13 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**図 12**: インデックス ビューの追加 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image24.png))


**ビューの追加**ダイアログ ボックスで、チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成**です。 ビューのデータ クラス ContactManager.Contact とビューのコンテンツの一覧を選択します。 これらのオプションを選択するには、連絡先のレコードの一覧を表示するビューが生成されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**図 13**: ビューの追加 ダイアログ ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image26.png))


クリックすると、**追加** ボタン、リスト 3 で、インデックス ビューを生成します。 通知、 &lt;% ページ % @&gt;ディレクティブ ファイルの上部に表示されます。 インデックス ビューは、ViewPage から継承&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt;クラスです。 つまり、ビューのモデル クラスは、Contact エンティティの一覧を表します。

インデックス ビューの本文には、それぞれのモデル クラスによって表される連絡先を反復処理する foreach ループが含まれています。 連絡先クラスの各プロパティの値は、HTML テーブル内で表示されます。

**3 - Views\Home\Index.aspx (未変更の状態) を一覧表示します。**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

インデックス ビューに 1 つの変更が必要です。 詳細ビューを作成できませんから詳細情報のリンクは削除できます。 検索して、インデックス ビューから、次のコードを削除します。

{.id = 項目。Id})&gt;

インデックス ビューを変更した後、連絡先のマネージャー アプリケーションを実行することができます。 [デバッグ開始] メニュー オプション、デバッグを選択するか、単に F5 キーを押します。 初めてアプリケーションを実行するダイアログ ボックスを図 14 に取得します。 オプションを選択**Web.config ファイルを変更してデバッグを有効にする**し、[ok] ボタンをクリックします。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**図 14**: デバッグの有効化 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image28.png))


既定では、インデックス ビューが返されます。 このビューには、すべての連絡先データベース テーブルからデータを一覧表示 (図 15 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**図 15**:、インデックス ビュー ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image30.png))


インデックス ビューに、ビューの下部にある場合は、新規作成というラベルのリンクが含まれていることを確認します。 次のセクションでは、新しい連絡先を作成する方法を学習します。

## <a name="creating-new-contacts"></a>新しい連絡先を作成します。

新しい連絡先を作成するユーザーを有効にするには、Home コント ローラーに 2 つの Create() アクションを追加する必要があります。 新しい連絡先を作成するための HTML フォームを表す 1 つの Create() アクションを作成する必要があります。 新しい連絡先の実際のデータベースの挿入を実行する 2 番目の Create() アクションを作成する必要があります。

4 の一覧表示するのには、Home コント ローラーに追加する必要がある新しい Create() メソッドが含まれています。

**4 - Controllers\HomeController.vb (のメソッドを作成する) を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

最初の Create() メソッド呼び出すことができる HTTP GET 中に、2 番目の Create() メソッドは、HTTP POST によってのみ呼び出すことができます。 つまり、2 番目の Create() メソッドは、HTML フォームの送信時にのみ起動できます。 最初の Create() メソッドは、単に、新しい連絡先を作成するための HTML フォームを含むビューを返します。 2 番目の Create() メソッドがよりわかりやすく: データベースに新しい連絡先を追加します。

連絡先クラスのインスタンスをそのまま使用する 2 番目の Create() メソッドが変更されたことに注意してください。 HTML フォームのポストされたフォーム値にバインドされますこの連絡先クラス、ASP.NET MVC フレームワークによって自動的に。 各フォーム フィールド HTML を作成フォームからは、連絡先のパラメーターのプロパティに割り当てられます。

連絡先のパラメーターが [バインド] 属性で修飾されたことに注意してください。 [バインド] 属性を使用して、バインドからの連絡先 Id プロパティを除外します。 Id プロパティを表すため、Identity プロパティ、Id プロパティを設定したくありません。

Create() メソッドの本体、データベースに新しい連絡先を挿入する Entity Framework を使用します。 連絡先の既存のセットに新しい連絡先が追加され、基になるデータベースにこれらの変更をプッシュして戻しますに SaveChanges() メソッドが呼び出されます。

2 つの Create() メソッドのいずれかを右クリックし、メニュー オプションを選択して新しい連絡先を作成するための HTML フォームを生成する**ビューの追加**(図 16 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**図 16**: 作成ビューの追加 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image32.png))


**ビューの追加**ダイアログで、選択、 **ContactManager.Contact**クラスおよび**作成**コンテンツの表示のオプション (図 17 を参照してください)。 クリックすると、**追加** ボタン、ビューが自動的に生成される作成します。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**図 17**: 分解ページが表示されて ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image34.png))


ビュー作成するにはには、各連絡先クラスのプロパティのフォーム フィールドが含まれています。 ビューを作成するためのコードは、5 の一覧に含まれます。

**5 - Views\Home\Create.aspx を一覧表示します。**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Create() メソッドを変更し、作成ビューを追加した後は、連絡先のマネージャー アプリケーションを実行し、新しい連絡先を作成します。 クリックして、**新規作成**作成ビューに移動するインデックス ビューに表示されるリンク。 図 18 にビューが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**図 18**: ビューの作成 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image36.png))


## <a name="editing-contacts"></a>連絡先を編集

連絡先レコードを編集するための機能を追加することは、新しい連絡先レコードを作成するための機能を追加するとよく似ています。 最初に、ホーム コント ローラー クラスに次の 2 つの新しい編集メソッドを追加する必要があります。 6 の一覧表示するのには、これらの新しい Edit() メソッドが含まれています。

**6 - Controllers\HomeController.vb (の編集メソッド) を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

最初の Edit() メソッドが HTTP GET 操作で呼び出されます。 Id パラメーターは、この編集されている連絡先のレコードの Id を表すメソッドに渡されます。 Entity Framework は、id と一致する連絡先を取得するために使用します。レコードの編集の HTML フォームを含むビューが返されます。

2 番目の Edit() メソッドでは、データベースへの実際の更新プログラムを実行します。 このメソッドは、連絡先クラスのインスタンスをパラメーターとして受け取ります。 ASP.NET MVC フレームワークはフォームのフィールド編集用のフォームからこのクラスを自動的にバインドします。 T がないことに注意してくださいには、(必要があります、Id プロパティの値) の連絡先を編集するときに、[バインド] 属性が含まれます。

Entity Framework を使用すると、変更されたメンバーをデータベースに保存されます。 元にお問い合わせくださいは、まず、データベースから取得する必要があります。 次に、連絡先に変更を記録する Entity Framework ApplyPropertyChanges() メソッドが呼び出されます。 最後に、基になるデータベースに変更を保持する Entity Framework SaveChanges() メソッドが呼び出されます。

Edit() メソッドを右クリックし、追加のビュー メニュー オプションを選択して編集用のフォームを含むビューを生成することができます。 ビューの追加 ダイアログ ボックスで、 **ContactManager.Models.Contact**クラスおよび**編集**コンテンツを表示 (図 19 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**図 19**: ビューの編集を追加する ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image38.png))


[追加] ボタンをクリックすると、新しい編集ビューが自動的に生成されます。 生成される HTML フォームには、各連絡先クラス (7 のリストを参照) のプロパティに対応するフィールドが含まれています。

**7 - Views\Home\Edit.aspx を一覧表示します。**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>連絡先を削除します。

連絡先を削除する場合は、ホーム コント ローラー クラスに次の 2 つの Delete() アクションを追加する必要があります。 Delete() の最初のアクションには、削除の確認フォームが表示されます。 2 番目の Delete() アクションでは、実際の削除を実行します。

> [!NOTE] 
> 
> 後で、イテレーション 7 では、お変更連絡先のマネージャー Ajax 削除 1 つのステップをサポートできるようにします。


8 の一覧表示するのには、2 つの新しい Delete() メソッドが含まれています。

**8 - Controllers\HomeController.vb (削除メソッド) を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

最初の Delete() メソッドが、データベースからの連絡先レコードを削除するための確認形式を返します (Figure20 を参照してください)。 2 番目の Delete() メソッドでは、データベースに対して、実際の削除操作を実行します。 データベースから元の連絡先の取得が完了した後は、データベースの削除を実行する、Entity Framework DeleteObject() および SaveChanges() メソッドが呼び出されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**図 20**: 削除の確認ビュー ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image40.png))


(図 21 を参照してください) に連絡先レコードを削除するためのリンクが含まれているように、インデックス ビューを変更する必要があります。 [編集] リンクを含む表のセルに次のコードを追加する必要があります。

{.id = 項目。Id})&gt;


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**図 21**: 編集リンクを使用してビューにインデックス ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image42.png))


次に、削除の確認のビューを作成する必要があります。 Home コント ローラー クラスで Delete() メソッドを右クリックし、メニュー オプションの追加のビューを選択します。 ビューの追加 ダイアログが表示されます (図 22)。

異なりは、リスト、作成、およびエディット ビューの場合、ビューの追加ダイアログ ボックスには、削除ビューを作成するオプションがありません。 代わりに、選択、 **ContactManager.Models.Contact**データ クラスおよび**空**コンテンツを表示します。 コンテンツのオプションをする必要が社内でビューを作成する空のビューを選択します。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**図 22**: 削除の確認ビューの追加 ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image44.png))


削除ビューの内容は、9 の一覧に含まれます。 このビューには、ことを確認するフォームが含まれています (図 21 を参照してください) を削除するかどうか、特定の連絡先があります。

**9 - Views\Home\Delete.aspx を一覧表示します。**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>既定のコント ローラーの名前を変更します。

連絡先を操作するため、コント ローラー クラスの名前がテンプレートを使用するクラスをという名前のわざわざ可能性があります。 T、コント ローラーの名前 ContactController しますか。

この問題は、簡単に修正します。 最初に、Home コント ローラーの名前をリファクターする必要があります。 HomeController クラスを Visual Studio コード エディターで開く、クラスの名前を右クリックし、メニュー オプションを選択**の名前を変更**です。 このメニュー オプションを選択すると、名前の変更 ダイアログが開きます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**図 23**: コント ローラーの名前をリファクタリング ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image46.png))


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**図 24**: 名前の変更 ダイアログを使用して ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image48.png))


コント ローラー クラスの名前を変更すると、Visual Studio でのビューのフォルダーで、フォルダーの名前が更新されます。 Visual Studio は、\Views\Contact フォルダーに \Views\Home フォルダーの名前が変更されます。

この変更を加えた後、アプリケーションは、Home コント ローラーを不要になったがあります。 アプリケーションを実行するときに図 25 のエラー ページが表示されます。


[![[新しいプロジェクト] ダイアログ ボックス](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**図 25**: 既定のコント ローラーなし ([フルサイズのイメージを表示するをクリックして](iteration-1-create-the-application-vb/_static/image50.png))


Home コント ローラーではなく、連絡先のコント ローラーを使用する Global.asax ファイルの既定のルートを更新する必要があります。 Global.asax ファイルを開き、既定のルート (10 のリストを参照) で使用される既定のコント ローラーを変更します。

**10 - Global.asax.vb を一覧表示します。**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

これらの変更を加えた後は、連絡先のマネージャーが正しく実行されます。 ここで、既定のコント ローラーとして連絡先コント ローラーのクラスが使用されます。

## <a name="summary"></a>概要

この最初のイテレーションで作成した基本的な連絡先のマネージャー アプリケーション最速の方法で考えられる。 このコント ローラーとビューの初期コードを自動的に生成する Visual Studio の利点を作成しました。 データベース モデル クラスを自動的に生成する Entity Framework を活用してもかかりました。

現時点では、おことができますを一覧表示を作成、編集、レコード、および削除にお問い合わせにお問い合わせのマネージャー アプリケーションにします。 つまり、すべてのデータベースに基づく web アプリケーションで必要な基本的なデータベース操作を実行できます。

残念ながら、このアプリケーションでは、いくつかの問題があります。 これを許可して最初になる躊躇、連絡先のマネージャー アプリケーションは、最も魅力的なアプリケーションではありません。 いくつかの設計作業が必要です。 次のイテレーションで、既定のビューのマスター ページとアプリケーションの外観を改善するカスケード スタイル シートを変更して方法について見ていきます。

次に、お実装していないすべてのフォーム検証をします。 たとえばがあるフォーム フィールドのいずれかの値を入力することがなく連絡先フォームの作成を送信することを防止するため何も行われません。 さらに、無効な電話番号と電子メール アドレスを入力することができます。 イテレーション 3 でのフォーム検証の問題に取り組むを開始します。

最後に、最も重要なは、連絡先のマネージャー アプリケーションの現在のイテレーション簡単に変更またはできません保持されます。 たとえば、データベース アクセス ロジックに組み込まれて右コント ローラーのアクション。 つまり、コント ローラーを変更することがなく、データ アクセス コードを修正することはできません。 後のイテレーションでは、連絡先のマネージャーの変更を回復力を高めるに実装できるソフトウェア設計パターンについて説明します。

>[!div class="step-by-step"]
[前へ](iteration-7-add-ajax-functionality-cs.md)
[次へ](iteration-2-make-the-application-look-nice-vb.md)
