---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: "追加のユーザー情報 (VB) を格納する |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、非常に基本的なゲストブック アプリケーションを構築してこの質問に回答されます。 そうでは、modeli のさまざまなオプションを確認しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b9acc02a1280446b9826c3f8f0022b4726139c7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="storing-additional-user-information-vb"></a>追加のユーザー情報 (VB) を格納します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> このチュートリアルでは、非常に基本的なゲストブック アプリケーションを構築してこの質問に回答されます。 これにより、データベースでは、ユーザー情報をモデルのさまざまなオプションを拝見し、メンバーシップ framework によって作成されたユーザー アカウントにこのデータを関連付ける方法について説明し、されます。


## <a name="introduction"></a>はじめに

ASP です。NET のメンバーシップのフレームワークでは、ユーザーを管理するための柔軟なインターフェイスを提供します。 メンバーシップ API には、資格情報の検証、現在ログオンしているユーザーに関する情報を取得する、新しいユーザー アカウントを作成および他のユーザー アカウントを削除するためのメソッドが含まれています。 メンバーシップ フレームワーク内の各ユーザー アカウントには、資格情報を検証し、重要なユーザー アカウントに関連するタスクを実行するために必要なプロパティのみが含まれています。 メソッドとプロパティが示すこれは、 [ `MembershipUser`クラス](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)、メンバーシップ フレームワーク内のユーザー アカウントをモデル化します。 このクラスのようなプロパティには[ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx)、 [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)、および[ `IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)などのメソッドと[ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx)と[ `UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)です。

多くの場合、アプリケーションは、メンバーシップ framework に含まれていない追加のユーザー情報を格納する必要があります。 たとえば、オンラインの小売店舗では、各ユーザー、および請求先住所、お支払い情報は、配信設定を保存し、連絡先の電話番号を使用できます必要があります。 さらに、各注文システムでは、特定のユーザー アカウントに関連付けられます。

`MembershipUser`クラスなどのプロパティを含まない`PhoneNumber`または`DeliveryPreferences`または`PastOrders`です。 どのように行うお、アプリケーションで必要なユーザー情報を追跡してそれをメンバーシップ フレームワークに統合しますか。 このチュートリアルでは、非常に基本的なゲストブック アプリケーションを構築してこの質問に回答されます。 これにより、データベースでは、ユーザー情報をモデルのさまざまなオプションを拝見し、メンバーシップ framework によって作成されたユーザー アカウントにこのデータを関連付ける方法について説明し、されます。 開始しましょう!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>手順 1: ゲストブック アプリケーションのデータ モデルの作成

これは、さまざまな手法を利用すると、データベース内のユーザー情報をキャプチャし、メンバーシップ、フレームワークによって作成されたユーザー アカウントに関連付けます。 これらの手法を示すためには、するためには、何らかのユーザー関連データがキャプチャされるように、チュートリアルの web アプリケーションを強化する必要があります。 (現在、アプリケーションのデータ モデルに、アプリケーション サービスで必要になったテーブルのみが含まれています、 `SqlMembershipProvider`)。

認証されたユーザーが、コメントを任せることができます、非常に単純なゲストブック アプリケーションを作成してみましょう。 ゲストブック コメントを保存するだけでなくできるようにして、各ユーザーは自分の出身地、ホーム ページ、および署名を保存します。 指定した場合、ユーザーのホーム町、ホーム ページ、および署名は、ゲストブック内の残りが彼は各メッセージに表示されます。

### <a name="adding-theguestbookcommentstable"></a>追加する、`GuestbookComments`テーブル

ゲストブック コメントをキャプチャするために必要がありますをという名前のデータベース テーブルを作成する`GuestbookComments`のように列を持つ`CommentId`、 `Subject`、 `Body`、および`CommentDate`です。 各レコードがある必要も、`GuestbookComments`テーブルの参照、コメントを左したユーザー。

データベースには、このテーブルを追加するには、Visual Studio の データベース エクスプ ローラーに移動し、ドリル ダウン、`SecurityTutorials`データベース。 テーブル フォルダーを右クリックし、新しいテーブルの追加 をクリックします。 使用すると、新しいテーブルの列を定義するインターフェイスが表示されます。


[![SecurityTutorials データベースに新しいテーブルを追加します。](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**図 1**: 新しいテーブルを追加、`SecurityTutorials`データベース ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image3.png))


次に、定義、`GuestbookComments`の列です。 という名前の列を追加することによって開始`CommentId`型の`uniqueidentifier`します。 この列は、ゲストブックには、各コメントを一意に識別する、許可しないように`NULL`s し、テーブルの主キーとしてマークします。 値を提供するのではなく、`CommentId`フィールドごとに`INSERT`、ことを示すお新しい`uniqueidentifier`値に自動的に生成するこのフィールドの`INSERT`列の既定値を設定して`NEWID()`です。 Primary key、および設定として、既定値を指定してこの最初のフィールドを追加した後、画面は、スクリーン ショット図 2 に示すようになります。


[![CommentId をという名前の主な列を追加します。](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**図 2**: プライマリという名前の列を追加`CommentId`([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image6.png))


という名前の列を次に、追加`Subject`型の`nvarchar(50)`という名前の列と`Body`型の`nvarchar(MAX)`、禁止する`NULL`両方の列にします。 次に、という名前の列を追加`CommentDate`型の`datetime`します。 禁止`NULL`s とセット、`CommentDate`列の既定値を`getdate()`です。

各ゲストブック コメントに、ユーザー アカウントを関連付ける列を追加します。 という名前の列を追加する方法もあります`UserName`型の`nvarchar(256)`します。 メンバーシップ プロバイダーを使用する以外の場合これは、適切な選択、`SqlMembershipProvider`です。 使用する場合は、`SqlMembershipProvider`でこのチュートリアル シリーズの、`UserName`内の列、`aspnet_Users`テーブルは、一意であることは保証されません。 `aspnet_Users`テーブルの主キーが`UserId`と型`uniqueidentifier`です。 したがって、`GuestbookComments`という名前の列がテーブルに必要がある`UserId`型の`uniqueidentifier`(禁止`NULL`値)。 この列を追加してください。

> [!NOTE]
> 説明したよう、 [*メンバーシップ スキーマを作成する SQL Server で*](creating-the-membership-schema-in-sql-server-vb.md)チュートリアル、メンバーシップ フレームワークが別のユーザー アカウントでの複数の web アプリケーションを同じ共有を有効にするように設計されていますユーザーのストア。 この機能を使用するには、異なるアプリケーションにユーザー アカウントをパーティション分割します。 同じユーザー ストアを使用して別のアプリケーションで同じユーザー名は使用可能性があります各ユーザー名は、アプリケーション内で一意であることが保証、中に。 複合`UNIQUE`内の制約、`aspnet_Users`テーブルに対して、`UserName`と`ApplicationId`フィールド、ものではないのだけ`UserName`フィールドです。 したがって、可能であれば、aspnet の\_ユーザー テーブルで、同じ 2 つ (以上) のレコードに`UserName`値。 ただし、`UNIQUE`に制約、`aspnet_Users`テーブルの`UserId`(があるため、主キー) フィールドです。 A`UNIQUE`指定されていない場合は、間の外部キー制約を確立できないために、制約は重要、`GuestbookComments`と`aspnet_Users`テーブル。


追加した後、`UserId`列で、ツールバーの [保存] アイコンをクリックすると、テーブルを保存します。 新しいテーブルの名前を`GuestbookComments`です。

最後に注意する問題の 1 つがある、`GuestbookComments`テーブル: を作成する必要があります、[外部キー制約](https://msdn.microsoft.com/library/ms175464.aspx)間、`GuestbookComments.UserId`列と`aspnet_Users.UserId`列です。 これを実現するには、するには、外部キー リレーションシップ ダイアログ ボックスを起動するには、ツールバーの リレーションシップ アイコンをクリックします。 (またはを起動できますこのダイアログ ボックス、テーブル デザイナー メニューに移動し、リレーションシップを選択する。)

外部キーのリレーションシップ ダイアログ ボックスの左下隅の 追加 ボタンをクリックします。 リレーションシップに参加しているテーブルを定義する必要がありますが、新しい外部キー制約は追加します。


[![外部キー リレーションシップ ダイアログ ボックスを使用して、テーブルの外部キー制約を管理するには](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**図 3**: 外部キー リレーションシップ ダイアログ ボックスを使用して、テーブルの外部キー制約を管理する ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image9.png))


次に、右側の「テーブルと列の仕様」行にある省略記号アイコンをクリックします。 これは、テーブルと列 ダイアログ ボックス起動、主キー テーブルと列から外部キー列を指定お元となる、`GuestbookComments`テーブル。 具体的には、選択`aspnet_Users`と`UserId`として主キー テーブルと列、および`UserId`から、`GuestbookComments`が外部キー列とテーブル (図 4 を参照してください)。 主キーと外部キー テーブルと列を定義した後は、外部キー リレーションシップ ダイアログ ボックスに戻るには ok をクリックします。


[![外部キー制約との間、aspnet_Users と GuesbookComments テーブルを確立します。](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**図 4**: 外部キー制約の間に確立、`aspnet_Users`と`GuesbookComments`テーブル ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image12.png))


この時点で、外部キー制約が確立されました。 この制約が存在することを確認[リレーショナル整合性](http://en.wikipedia.org/wiki/Referential_integrity)が保証されませんがあるゲストブック エントリが存在しないユーザー アカウントを参照する 2 つのテーブルの間です。 既定では、外部キー制約には親レコードを子レコードに対応する場合に削除が許可されなくなります。 つまり、ユーザーが 1 つまたは複数のゲストブック コメントし、そのユーザー アカウントを削除しようとは、削除が失敗、ゲストブック コメントが最初に削除しない限り、します。

親レコードが削除されたときに自動的に関連付けられている子レコードを削除する外部キー制約を構成することができます。 つまり、ユーザーのゲストブック エントリは、自分のユーザー アカウントが削除されたときに自動的に削除できるように、この外部キー制約をセットアップおできます。 これを実現するには、「挿入と更新の指定」セクションを展開し Cascade に「ルールの削除」プロパティを設定します。


[![連鎖削除する外部キー制約を構成します。](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**図 5**: Cascade を削除する外部キー制約を構成する ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image15.png))


外部キー制約を付けて保存するには、外部キーのリレーションシップを終了する閉じるボタンをクリックします。 テーブルと、これを保存するには、ツールバーで [保存] アイコンをクリックしてリレーションシップです。

### <a name="storing-the-users-home-town-homepage-and-signature"></a>ユーザーのホーム町、ホーム ページ、および署名を格納します。

`GuestbookComments`テーブルは、ユーザー アカウントと一対多のリレーションシップを共有する情報を格納する方法を示しています。 各ユーザー アカウントには、関連付けられているコメントの任意の数を含めることがあります、ために、このリレーションシップは、特定のユーザーにリンクが各コメントをバックする列を含むコメントのセットを保持するテーブルを作成してモデル化されます。 使用する場合、 `SqlMembershipProvider`、という名前の列を作成することでこのリンクが確立される最適な`UserId`型の`uniqueidentifier`とこの列の間の外部キー制約と`aspnet_Users.UserId`です。

これに 3 つの列をユーザーのホーム町、ホーム ページ、および彼ゲストブック コメントに表示されるシグネチャを格納するには、各ユーザー アカウントに関連付ける必要があります。 これを実現するさまざまな方法の数。

- **新しい列を追加、* * *`aspnet_Users`* * * または * * *`aspnet_Membership`* * * テーブル。** によって使用されるスキーマを変更するためにこの方法を推奨、I、`SqlMembershipProvider`です。 この決定は、落ちて発言に返される可能性があります。 たとえば、what-if ASP.NET の将来のバージョンを使用して、さまざまな`SqlMembershipProvider`スキーマです。 Microsoft は、ASP.NET 2.0 を移行するためのツールを含めることが`SqlMembershipProvider`ASP.NET 2.0 を変更した場合は、新しいスキーマにデータ`SqlMembershipProvider`スキーマ、変換できない可能性があります。

- **ASP を使用します。NET のプロファイルのフレームワーク、ホーム町、ホーム ページ、および署名のプロファイル プロパティを定義します。** ASP.NET には、その他のユーザーに固有のデータを格納するように設計されたプロファイルのフレームワークが含まれています。 メンバーシップ フレームワークと同様に、プロファイルのフレームワークはプロバイダー モデル上に構築されます。 .NET Framework が付属しています、`SqlProfileProvider`ストアが SQL Server データベース内のデータをプロファイルします。 実際には、データベースは既にで使用されるテーブル、 `SqlProfileProvider` (`aspnet_Profile`) で、アプリケーション サービスのバックアップを追加したときに追加された、 [*メンバーシップ スキーマを作成する SQL Server で*](creating-the-membership-schema-in-sql-server-vb.md)チュートリアルです。   
 プロファイルのフレームワークの主な利点は、開発者のプロファイルのプロパティを定義することができる`Web.config`– コードを基になるデータ ストアと、プロファイル データをシリアル化に書き込む必要はありません。 つまり、プロファイルのプロパティのセットを定義して、コードで作業するには驚くほど簡単です。 ただし、プロファイル システム不十分、バージョン管理する際に必要なため、後の時刻、または既存のテーブルを削除または変更されると、追加された新しいユーザーに固有のプロパティを期待するし、プロファイルのフレームワークができない可能性があります、アプリケーションがある場合、 最適なオプションです。 さらに、`SqlProfileProvider`次に不可能にすることをプロファイル データ (など、ユーザーの数では、New York のホーム町がある) に対して直接クエリを実行する、高非正規化方式で、プロファイルのプロパティを格納します。   
 プロファイルのフレームワークの詳細については、このチュートリアルの最後に、「これ以上の測定値」セクションを参照してください。

- **データベースに新しいテーブルにはこれら 3 つの列を追加し、このテーブルの間の一対一の関係を確立し、* * *`aspnet_Users`* * *。** この方法は、プロファイル フレームワークよりも少し多くの作業が含まれますが、追加のユーザー プロパティがデータベースにモデル化方法で最大限の柔軟性を提供しています。 これは、このチュートリアルで使用するオプションです。

という名前の新しいテーブルを作成、`UserProfiles`ホーム町、ホーム ページ、および各ユーザーの署名を保存します。 データベース エクスプ ローラー ウィンドウで テーブル フォルダーを右クリックし、新しいテーブルを作成するを選択します。 最初の列の名前を付けます`UserId`にその型を設定および`uniqueidentifier`です。 禁止`NULL`値し、列を主キーとしてマークします。 次に、という名前の列を追加:`HomeTown`型の`nvarchar(50)`です。`HomepageUrl`型の`nvarchar(100)`; 型のシグネチャと`nvarchar(500)`です。 これら 3 つの列のそれぞれを受け入れることができます、`NULL`値。


[![ユーザー テーブルを作成します。](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**図 6**: 作成、`UserProfiles`テーブル ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image18.png))


テーブルを保存し、名前を付けます`UserProfiles`です。 最後との間の外部キー制約を確立、`UserProfiles`テーブルの`UserId`フィールドおよび`aspnet_Users.UserId`フィールドです。 間で外部キー制約と同様、`GuestbookComments`と`aspnet_Users`テーブルがあるこの制約の連鎖的に削除します。 以降、`UserId`フィールドに`UserProfiles`プライマリ キー、これにより、内の 2 つ以上のレコードがある、`UserProfiles`ユーザー アカウントごとにテーブルです。 この種のリレーションシップは、一対一に呼ばれます。

これが、データ モデルを作成、使用する準備ができました。 手順 2 および 3 を見てみましょう方法、現在ログオンしているユーザーを表示およびそのホーム町、ホーム ページ、および署名情報を編集できます。 手順 4. で認証されたユーザー、ゲストブックに新しいコメントを送信し、既存のサーバーを表示するためのインターフェイスを作成します。

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>手順 2: ユーザーのホーム町、ホーム ページ、および署名の表示

これは、さまざまな方法は現在ログオンしているユーザーが自分のホームの町、ホーム ページ、および署名情報を表示および編集できるようにします。 テキスト ボックスと、ユーザー インターフェイスを手動で作成する可能性がありますおよびラベル コントロールまたはおが使用データ DetailsView コントロールなどの Web コントロールのいずれか。 データベースを実行する`SELECT`と`UPDATE`ADO.NET 作成ステートメントは、ページの分離コード クラス内のコードまたは、代わりに、SqlDataSource による宣言型の方法を使用します。 理想的には、アプリケーションには、おでしたか、プログラムで起動または宣言によって、ページの分離コード クラスから、ObjectDataSource コントロールを使用して、階層化されたアーキテクチャが含まれます。

このチュートリアルの系列では、フォーム認証、承認、ユーザー アカウント、およびロールに焦点を当てています、以降はありませんこれらのさまざまなデータ アクセス オプションまたは理由階層化されたアーキテクチャよりもが望ましい直接 SQL ステートメントを実行する方法についてASP.NET ページです。 順を追って DetailsView と SqlDataSource (最も速いと最も簡単なオプション) を使用するつもりが説明する概念は、別の Web コントロールとデータ アクセス ロジックに確実に適用できます。 ASP.NET 内のデータ処理の詳細についてを参照してください   *[ASP.NET 2.0 のデータを扱う](../../data-access/index.md)*一連のチュートリアルです。

開いている、 `AdditionalUserInfo.aspx`  ページで、`Membership`フォルダー ページで、ID プロパティを設定する DetailsView コントロールを追加する`UserProfile`を消去して、`Width`と`Height`プロパティです。 DetailsView のスマート タグを展開し、新しいデータ ソース コントロールにバインドを選択します。 これは、データ ソース構成ウィザードが起動 (図 7 を参照してください)。 最初の手順では、データ ソースの種類を指定するよう求められます。 直接接続するので、`SecurityTutorials`データベース、データベース アイコンを選択を指定する、`ID`として`UserProfileDataSource`です。


[![UserProfileDataSource をという名前の新しい SqlDataSource コントロールを追加します。](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**図 7**: 新しい SqlDataSource コントロールという追加`UserProfileDataSource`([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image21.png))


次の画面は、使用するデータベースのメッセージが表示されます。 内の接続文字列は既に定義`Web.config`の`SecurityTutorials`データベース。 この接続文字列名 – `SecurityTutorialsConnectionString` – ドロップダウン リストにする必要があります。 このオプションを選択し、[次へ] をクリックします。


[![SecurityTutorialsConnectionString をドロップダウン リストから選択します。](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**図 8**: 選択`SecurityTutorialsConnectionString`ドロップダウン リストから ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image24.png))


後続の画面では、テーブルと列のクエリを指定するよう求められます。 選択、`UserProfiles`ドロップダウン リストからテーブルし、すべての列を確認します。


[![Bring ユーザー テーブルからすべての列のバックアップ](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**図 9**: Bring 戻るすべての列から、`UserProfiles`テーブル ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image27.png))


図 9 を返しますの現在のクエリ*すべて*内のレコードの`UserProfiles`、だけでは、現在ログオンしているユーザーのレコードが、します。 追加する、`WHERE`句をクリックして、 `WHERE`  ボタンの追加 を`WHERE`句 ダイアログ ボックス (図 10 参照)。 ここでは、フィルターを適用する列、演算子、およびフィルター パラメーターのソースを選択できます。 選択`UserId`列と演算子として「=」として。

残念ながら、現在ログオンしているユーザーを返す組み込みのパラメーターの送信元がない`UserId`値。 プログラムでこの値を取得する必要があります。 このため、"なしに、"の追加は、パラメーターを追加するボタンをクリックし、[ok] をクリックしをクリックしてソースのドロップダウン リストを設定します。


[![ユーザー Id の列にフィルター パラメーターを追加します。](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**図 10**: で Filter パラメーターを追加、`UserId`列 ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image30.png))


[Ok] をクリックした後に、図 9 に示すように画面に返されるされます。 このとき、ただし、画面の下部にある SQL クエリ含める必要があります、`WHERE`句。 「テスト クエリ」画面に移動するには、次にクリックします。 ここで、クエリを実行し、結果を確認します。 ウィザードを完了するには、[完了] をクリックします。

データ ソース構成ウィザードを完了すると、Visual Studio は、ウィザードで指定された設定に基づいて、SqlDataSource コントロールを作成します。 さらに、手動で追加 BoundFields SqlDataSource のによって返される列ごとに DetailsView に`SelectCommand`です。 表示する必要はありません、`UserId`ユーザーがこの値を調べる必要がないため、DetailsView のフィールドです。 DetailsView コントロールの宣言型マークアップから直接フィールドを削除します。 または、フィールドをクリックして、"編集"のスマート タグからリンクできます。

この時点で、ページの宣言型マークアップは、次のようになります。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

SqlDataSource コントロールをプログラムで設定する必要があります`UserId`パラメーターを現在ログインしているユーザーの`UserId`データが選択される前にします。 これには、SqlDataSource のイベント ハンドラーを作成することで`Selecting`イベントと、次を追加するコードがあります。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

上記のコードを呼び出すことによって、現在ログオンしているユーザーへの参照を取得することによって開始、`Membership`クラスの`GetUser`メソッドです。 これを返します、`MembershipUser`オブジェクト、`ProviderUserKey`プロパティが含まれています、`UserId`です。 `UserId`し、値は、SqlDataSource`@UserId`パラメーター。

> [!NOTE]
> `Membership.GetUser()`メソッドは、現在ログオンしているユーザーに関する情報を返します。 匿名ユーザーがページを訪問している場合の値が返されます。`Nothing`です。 このような場合は、結果には、 `NullReferenceException` 、次のコード行に読み取りを試みたとき、`ProviderUserKey`プロパティです。 について心配する必要はないもちろん、`Membership.GetUser()`では Nothing を返す、`AdditionalUserInfo.aspx`おように構成されている URL 承認前のチュートリアルで認証されたユーザーのみがこのフォルダーに ASP.NET リソースにアクセスできなかったためにのページです。 匿名アクセスが許可されているページで、現在ログオンしているユーザーに関する情報にアクセスする必要がある場合を確認することができます、`MembershipUser`から返されたオブジェクト、`GetUser()`メソッド Nothing ではないプロパティを参照する前にします。


アクセスした場合、`AdditionalUserInfo.aspx`ページ ブラウザーを使ってまだに任意の行を追加しているために、空白のページを表示は、`UserProfiles`テーブル。 手順 6. で見ていきますを自動的に新しい行を追加する CreateUserWizard コントロールをカスタマイズする方法、`UserProfiles`表に、新しいユーザー アカウントの作成時にします。 ここでは、ただしは必要があります、テーブルにレコードを手動で作成します。

Visual Studio の データベース エクスプ ローラーに移動し、テーブル フォルダーを展開します。 右クリックし、`aspnet_Users`テーブルし"を表示するテーブルのデータの選択"、テーブル内のレコードを表示する; 同じ操作を行う、`UserProfiles`テーブル。 図 11 は、垂直方向に並べて表示されるときに、これらの結果を示します。 データベースで発生している`aspnet_Users`ブルース、Fred、および Tito、レコードはレコードがありません、`UserProfiles`テーブル。


[![Aspnet_Users の内容は、ユーザー テーブルが表示されます。](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**図 11**: の 目次、`aspnet_Users`と`UserProfiles`テーブルが表示されます ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image33.png))


新しいレコードを追加、`UserProfiles`テーブルの値に手動で入力して、 `HomeTown`、 `HomepageUrl`、および`Signature`フィールドです。 簡単に有効な`UserId`、新しい値`UserProfiles`レコードは、選択する、`UserId`フィールド内の特定のユーザー アカウントを`aspnet_Users`の表に、コピーして貼り付けます、`UserId`フィールドに`UserProfiles`です。 図 12 を示しています、`UserProfiles`ブルースに対して新しいレコードを追加した後の表にします。


[![レコードがのブルース ユーザーに追加されました](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**図 12**: に A レコードを追加した`UserProfiles`ブルースの ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image36.png))


戻り、 `AdditionalUserInfo.aspx page`、ブルースとしてログインしています。 図 13 に示す Bruce's 設定が表示されます。


[![現在訪問ユーザーは His 設定の表示](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**図 13**: 現在訪問ユーザーは His 設定の表示 ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> 移動先を手動で追加のレコード、`UserProfiles`メンバーシップ ユーザーごとにテーブルです。 手順 6. で見ていきますを自動的に新しい行を追加する CreateUserWizard コントロールをカスタマイズする方法、`UserProfiles`表に、新しいユーザー アカウントの作成時にします。


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>手順 3: ユーザーが自分のホーム町、ホーム ページ、および署名の編集を許可します。

そのホーム町、ホーム ページ、および署名の設定、この時点で、現在ログインしているユーザーを表示できますが、まだ変更されることはできません。 データを編集できるように DetailsView コントロールを更新してみましょう。

まずを行うには必要なは、追加、 `UpdateCommand` 、SqlDataSource を指定する、`UPDATE`を実行するステートメントとその対応するパラメーターです。 SqlDataSource を選択し、[プロパティ] ウィンドウからコマンドおよびパラメーターのエディター ダイアログ ボックスを表示する抽出プロパティの横にある省略記号ボタンをクリックします。 次の入力`UPDATE`ステートメントをテキスト ボックスに。

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

次に、 をクリック「パラメーターの更新」、SqlDataSource コントロールのパラメーターを作成する`UpdateParameters`ごとでは、パラメーターのコレクション、`UPDATE`ステートメントです。 [なし] にパラメーター セットのすべてのソースのままにし、ダイアログ ボックスに [ok] ボタンをクリックします。


[![SqlDataSource の UpdateCommand と UpdateParameters を指定します。](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**図 14**: 指定 SqlDataSource の`UpdateCommand`と`UpdateParameters`([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image42.png))


追加機能のためには、SqlDataSource コントロール、コントロールが編集をサポートできるようになりました DetailsView に行われます。 DetailsView のスマート タグから「編集を有効にする」のチェック ボックスを確認します。 コントロールの追加、CommandField`Fields`コレクションでその`ShowEditButton`プロパティを True に設定します。 [編集] ボタンは、読み取り専用モードと Update DetailsView が表示され、編集モードを [キャンセル] ボタンに表示されるときにこのレンダリングします。 されなくても、[編集] をクリックするユーザーがあっても DetailsView レンダリング「常に編集可能」状態で DetailsView コントロールの設定によって[`DefaultMode`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx)に`Edit`です。

これらの変更、DetailsView コントロールの宣言型マークアップを次のようになります。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

CommandField の追加に注意してください、`DefaultMode`プロパティです。

ブラウザーからこのページをテストしてください。 対応するレコードのあるユーザーにアクセスしたとき`UserProfiles`ユーザーの設定が編集可能なインターフェイスに表示されます。


[![DetailsView が編集可能なインターフェイスを表示します。](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**図 15**: DetailsView が編集可能なインターフェイスを表示 ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image45.png))


再試行の値を変更および更新 をクリックします。 何かのように表示されます。 ポストバックがあると値は、データベースに保存されますが、保存が発生した視覚的なフィードバックはありません。

この問題を解決するには、Visual Studio に戻り、DetailsView 上のラベル コントロールを追加します。 設定の`ID`に`SettingsUpdatedMessage`、その`Text`プロパティを「の設定が更新されました」とその`Visible`と`EnableViewState`プロパティ`False`です。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

表示する必要があります、 `SettingsUpdatedMessage` DetailsView が更新されるたびにラベルを付けます。 これを実現するには、detailsview のイベント ハンドラーを作成`ItemUpdated`イベントし、次のコードを追加します。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

戻り、`AdditionalUserInfo.aspx`ブラウザー内でページし、データを更新します。 このとき、役立つステータス メッセージが表示されます。


[![短いメッセージが表示されるときに、設定を更新](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**図 16**: 設定が更新されたときに、短いメッセージが表示されます ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> DetailsView コントロールのインターフェイスのままに編集するが必要なことはたくさんのです。 テキスト ボックスの標準サイズを使用してが署名フィールドには、複数行テキスト ボックスある必要があります。 RegularExpressionValidator はホームページの URL を入力した場合で開始する"http://"または"https://"ことを確認するために必要があります。 さらに、コントロールが DetailsView 以降その`DefaultMode`プロパティに設定`Edit`、[キャンセル] ボタンは何も行わない。 これは、いずれかを削除するか、または、クリックすると、ユーザーをリダイレクト他のページ (など`~/Default.aspx`)。 これらの拡張機能は、リーダーの課題として退出です。


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>リンクを追加する、`AdditionalUserInfo.aspx`マスター ページのページ

現時点では、web サイトの管轄外へのリンク、`AdditionalUserInfo.aspx`ページ。 アクセスする唯一の方法では、ブラウザーのアドレス バーに直接、ページの URL を入力します。 ここで、このページへのリンクを追加、`Site.master`マスター ページ。

マスター ページがで LoginView Web コントロールを含むことに注意してください、 `LoginContent` ContentPlaceHolder 認証済みおよび匿名のビジターに対して異なるマークアップを表示します。 LoginView コントロールの更新`LoggedInTemplate`へのリンクを含める、`AdditionalUserInfo.aspx`ページ。 LoginView、これらの変更を行った後コントロールの宣言型マークアップを次のようになります。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

追加に注意してください、`lnkUpdateSettings`ハイパーリンク コントロールを`LoggedInTemplate`です。 このリンクで認証されたユーザーにすばやく移動できます、ページを表示およびそのホーム町、ホーム ページ、および署名の設定を変更します。

## <a name="step-4-adding-new-guestbook-comments"></a>手順 4: 新しいゲストブック コメントを追加します。

`Guestbook.aspx`ページが認証されたユーザーが、ゲストブックを表示し、コメントのままにします。 新しいゲストブック コメントを追加するインターフェイスの作成を始めましょう。

開く、 `Guestbook.aspx` Visual Studio でページおよび新しいコメントの件名と本文の 2 つのテキスト ボックス コントロールで構成されるユーザー インターフェイスを構築します。 最初の TextBox コントロールの設定`ID`プロパティを`Subject`とその`Columns`40 プロパティ以外の設定の 2 番目の`ID`に`Body`、その`TextMode`に`MultiLine`、およびその`Width`と`Rows`プロパティを「95%」や 8、それぞれします。 完了するには、ユーザー インターフェイスという名前のボタンの Web コントロールを追加`PostCommentButton`設定とその`Text`プロパティを「投稿、コメント」です。

各ゲストブック コメントには、件名と本文が必要とするため、各テキスト ボックス、RequiredFieldValidator を追加します。 設定、 `ValidationGroup` "EnterComment"です。 これらのコントロールのプロパティ同様に、設定、`PostCommentButton`コントロールの`ValidationGroup`プロパティを"EnterComment"です。 ASP の詳細についてはします。NET の検証コントロール、チェック アウト[ASP.NET でのフォーム検証](http://www.4guysfromrolla.com/webtech/090200-1.shtml)、 [ASP.NET 2.0 の検証コントロールを分解](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)、および[検証サーバー コントロールのチュートリアル](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp)[W3Schools](http://www.w3schools.com/)です。

ユーザー インターフェイスを作成したら、ページの宣言型マークアップなります次のような。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

完全なユーザー インターフェイスと、次のタスクに新しいレコードを挿入する、`GuestbookComments`時にテーブル、`PostCommentButton`をクリックします。 これには、さまざまな方法で: ボタンの ADO.NET コードを記述できます`Click`イベント ハンドラーですお SqlDataSource コントロール、ページを追加、構成することができます、 `InsertCommand`、まずその`Insert`メソッドから、`Click`イベント。ハンドラーです。またはおが新しいゲストブック コメントの挿入を担当する中間層を作成してそのからこの機能を実行することも、`Click`イベント ハンドラー。 手順 3 で、SqlDataSource を使用して検索すると、以降は、ここで ADO.NET コードを使用してみましょう。

> [!NOTE]
> Microsoft SQL Server データベースからデータをプログラムでアクセスするために使用する ADO.NET クラスにある、`System.Data.SqlClient`名前空間。 ページの分離コード クラスにこの名前空間をインポートする必要があります (つまり、 `Imports System.Data.SqlClient`)。


イベント ハンドラーを作成、`PostCommentButton`の`Click`イベントし、次のコードを追加します。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

`Click`ユーザーが指定したデータが有効であるかをチェックしてイベント ハンドラーを起動します。 そうでない場合、イベント ハンドラーは、レコードを挿入する前に終了します。 指定されたデータは、有効なと仮定すると、現在ログオンしているユーザーの`UserId`値が取得されに格納されている、`currentUserId`ローカル変数。 提供する必要がありますので、この値が必要な`UserId`にレコードを挿入するときの値`GuestbookComments`です。

次に、接続文字列が、`SecurityTutorials`からデータベースを取得`Web.config`と`INSERT`SQL ステートメントを指定します。 A`SqlConnection`オブジェクトが作成され、開かれました。 次に、`SqlCommand`オブジェクトが構築されで使用されるパラメーターの値、`INSERT`クエリが割り当てられています。 `INSERT`ステートメントを実行して、接続が終了します。 イベント ハンドラーの最後に、`Subject`と`Body`テキスト ボックスの`Text`プロパティがオフになってできるように、ユーザーの値は、ポストバック間で保持されません。

このページをブラウザーでテストしてください。 このページがであるため、`Membership`フォルダーはアクセスできない匿名の訪問者にします。 したがって、最初 (されませんが既にある場合) にログオンする必要があります。 値を入力して、`Subject`と`Body`テキスト ボックス をクリックし、`PostCommentButton`ボタンをクリックします。 これにより、新しいレコードに追加する`GuestbookComments`です。 ポストバックで件名と本文の指定したが、テキスト ボックスからワイプされます。

クリックした後、`PostCommentButton`ボタンがありますに視覚的なフィードバックはありません、コメント、ゲストブックを追加します。 既存のゲストブックのコメントは、手順 5. での処理を表示するには、このページを更新する必要があります。 これを実行するだけで追加したコメントが適切な視覚的なフィードバックを提供する、コメントの一覧に表示されます。 ここでは、確認の内容を確認するには、ゲストブック コメントが保存されたこと、`GuestbookComments`テーブル。

図 17 の内容を示しています、`GuestbookComments`表の後、2 つのコメントが残されています。


[![GuestbookComments テーブルでゲストブック コメントを確認できます。](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**図 17**: でゲストブック コメントを表示できます、`GuestbookComments`テーブル ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> ユーザーを含む可能性のあるゲストブック コメントを挿入しようとしました。 危険性のあるマークアップ: HTML – ASP.NET などがスローされます、`HttpRequestValidationException`です。 この例外がスローされた原因と危険性のある値を送信するユーザーを許可する方法の詳細についてを参照してください、[要求の検証に関するホワイト ペーパー](../../../../whitepapers/request-validation.md)です。


## <a name="step-5-listing-the-existing-guestbook-comments"></a>手順 5:、既存のゲストブック コメントが一覧表示します。

コメント、ユーザーが訪問のままに加え、`Guestbook.aspx`ページをゲストブックの既存のコメントを表示することもする必要があります。 これを実現する、という名前の ListView コントロールを追加`CommentList`ページの下部にします。

> [!NOTE]
> ListView コントロールは ASP.NET 3.5 が新たに追加します。 非常にカスタマイズ可能な柔軟なレイアウトに項目の一覧を表示しながら組み込みの編集、挿入、削除、ページング、および並べ替え GridView のような機能をまだ提供されています。 ASP.NET 2.0 を使用している場合は、代わりに、DataList またはリピータ コントロールを使用する必要があります。 ListView の使用に関する詳細については、次を参照してください。 [Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ エントリ「 [asp: ListView コントロール](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)、と私の記事[ListView コントロールにデータを表示する](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)です。


ListView のスマート タグを開き、データ ソースのドロップダウン リストから新しいデータ ソースにコントロールをバインドします。 手順 2 で説明したとおり、これが、データ ソース構成ウィザードが起動します。 [データベース] アイコンを選択し、結果として得られる SqlDataSource という名前を`CommentsDataSource`、[ok] をクリックします。 次に、選択、`SecurityTutorialsConnectionString`接続がドロップダウン リストから文字列を [次へ] をクリックします。

この時点で手順 2. で指定したデータのクエリをピッキングで、`UserProfiles`ドロップダウン リストからテーブルが表示され、返される列を選択する (戻るは図 9 を参照してください)。 このとき、ただし、たい戻るだけでなく、レコードを取得する SQL ステートメントを作成する`GuestbookComments`が、コメントの出身地も、ホーム ページ、署名、およびユーザー名。 したがって、「カスタム SQL ステートメントまたはストアド プロシージャを指定する」オプション ボタンを選択し、[次へ] をクリックします。

これは、「を定義するカスタム ステートメントまたはストアド プロシージャ」画面が表示されます。 クエリをグラフィカルに作成するクエリ ビルダー ボタンをクリックします。 クエリ ビルダーからクエリを実行するテーブルを指定するよう求められますが開始されます。 選択、 `GuestbookComments`、 `UserProfiles`、および`aspnet_Users`テーブルし、[ok] をクリックします。 これでは、3 つすべてのテーブルをデザイン画面に追加します。 間で外部キー制約があるため、 `GuestbookComments`、 `UserProfiles`、および`aspnet_Users`テーブル、クエリ ビルダーは、自動的に`JOIN`s これらのテーブルです。

返される列を指定します。 `GuestbookComments`テーブルの選択、 `Subject`、 `Body`、および`CommentDate`列以外の戻り値の場合は、 `HomeTown`、 `HomepageUrl`、および`Signature`からの列、 `UserProfiles` ; テーブルし、返す`UserName`から`aspnet_Users`. また、追加"`ORDER BY CommentDate DESC`"の末尾に、`SELECT`最新の投稿が最初に返されるようにクエリを実行します。 これらの選択を行った後、クエリ ビルダー インターフェイスは図 18 でスクリーン ショットのようになります。


[![構築クエリ GuestbookComments、ユーザー、および aspnet_Users テーブルを結合します。](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**図 18**:、構築されたクエリ`JOIN`s、 `GuestbookComments`、 `UserProfiles`、および`aspnet_Users`テーブル ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image54.png))


クエリ ビルダー ウィンドウを閉じて、「を定義するカスタム ステートメントまたはストアド プロシージャ」の画面に戻るには、ok をクリックします。 クエリのテスト ボタンをクリックして、クエリの結果を表示することがあります、「テスト クエリ」の画面に詳細の横にあるをクリックします。 準備ができたら、データ ソース構成ウィザードを完了するには、[完了] をクリックします。

ステップ 2 で、関連付けられた DetailsView コントロールのデータ ソース構成ウィザードを完了して`Fields`コレクションは、によって返される列ごとに BoundField を含むように更新されました、`SelectCommand`です。 ListView、ただしは変更されていません。そのレイアウトを定義する必要があります。 ListView のレイアウトは、その宣言型マークアップまたはのスマート タグで"ListView の構成"オプションから、手動で作成できます。 通常、マークアップを手動で定義したがりますが、最も自然なメソッドを使用します。

結局、次を使用して`LayoutTemplate`、 `ItemTemplate`、および`ItemSeparatorTemplate`ListView コントロールの。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

`LayoutTemplate`のマークアップを定義、コントロールによって発生したときに、 `ItemTemplate` SqlDataSource によって返される各項目をレンダリングします。 `ItemTemplate`の結果として得られるマークアップが配置されます、`LayoutTemplate`の`itemPlaceholder`コントロール。 加え、 `itemPlaceholder`、 `LayoutTemplate` ListView を 1 ページ (既定) のみ 10 ゲストブック コメントの表示を制限する DataPager コントロールが含まれており、ページング インターフェイスをレンダリングします。

マイ`ItemTemplate`で各ゲストブック コメントの内容を表示、`<h4>`サブジェクトの下に離れた本文を持つ要素。 本文を表示するための構文がによって返されるデータを受け取ることに注意してください、`Eval("Body")`データ バインド ステートメントでは、文字列に変換すること、および置換の改行を`<br />`要素。 HTML での空白は無視されますので、コメントを送信するときに入力した行区切りを表示するのには、この変換が必要です。 ユーザーの署名は、ユーザーのホーム町、自分のホーム ページ、コメントが行われた、およびコメントを左人のユーザー名へのリンクを続けて、斜体で本文の下に表示されます。

すぐをブラウザーでページを表示します。 ここに表示される手順 5 でゲストブックを追加したコメントが表示されます。


[![Guestbook.aspx Now には、ゲストブックのコメントが表示されます。](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**図 19**:`Guestbook.aspx`ゲストブックのコメントが表示されます ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image57.png))


新しいコメントを追加するには、ゲストブックを再試行してください。 クリックすると、`PostCommentButton`ボタン、ページがポストバックしコメントは、データベースを追加が ListView コントロールは、新しいコメントを表示するのには更新されません。 これは、いずれかで修正できます。

- 更新、`PostCommentButton`ボタンの`Click`イベント ハンドラーを呼び出します ListView コントロールのため`DataBind()`メソッド、データベースに新しいコメントを挿入した後に、または
- ListView コントロールの設定`EnableViewState`プロパティを`False`です。 この方法は、コントロールのビューステートを無効にすると、ポストバックのたびに基になるデータにバインドする必要がありますので機能します。

このチュートリアルからダウンロード可能なチュートリアルの web サイトは、両方の方法を示しています。 ListView コントロールの`EnableViewState`プロパティを`False`ListView にデータをプログラムによって再バインドするために必要なコードがである、 `Click` 、イベント ハンドラーがコメント アウトしますが、します。

> [!NOTE]
> 現在、 `AdditionalUserInfo.aspx`  ページでは、ユーザーを表示およびそのホーム町、ホーム ページ、および署名の設定を編集します。 更新する便利な場合があります`AdditionalUserInfo.aspx`ゲストブックのユーザーのコメントに、ログ記録を表示します。 つまり、確認して、自分の情報を変更する、に加えて、ユーザーがアクセスできる、`AdditionalUserInfo.aspx`ページを参照するどのゲストブック コメント過去で彼女が行われます。 I のままに練習として、興味のリーダーの。


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>手順 6: ホーム町、ホーム ページ、および署名のインターフェイスを含める CreateUserWizard コントロールのカスタマイズ

`SELECT`によって使用されるクエリ、`Guestbook.aspx`ページの使用、`INNER JOIN`間で関連レコードを結合して、 `GuestbookComments`、 `UserProfiles`、および`aspnet_Users`テーブル。 場合にレコードがないユーザー`UserProfiles`ゲストブック行うコメントの追加、コメント、ListView には表示されません、`INNER JOIN`のみが返されます`GuestbookComments`レコード内の一致レコードがある場合に`UserProfiles`と`aspnet_Users`です。 ユーザーがレコード手順 3. で示したように、`UserProfiles`彼女を表示または自分で設定を編集できない、`AdditionalUserInfo.aspx`ページ。

言うまでも、設計のため、メンバーシップ システム内のすべてのユーザー アカウントは、一致することが重要である意思決定を記録で、`UserProfiles`テーブル。 対応するレコードに追加するのには、必要な`UserProfiles`CreateUserWizard、新しいメンバーシップ ユーザー アカウントを作成するときにします。

説明したように、 [*ユーザー アカウントを作成する*](creating-user-accounts-vb.md)チュートリアルでは、新しいメンバーシップ ユーザーのアカウントには CreateUserWizard コントロールが作成された後発生その[`CreatedUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). このイベントのイベント ハンドラーを作成、だけが作成したユーザーのユーザー Id を取得したりにレコードを挿入できます、`UserProfiles`の既定値を持つテーブル、 `HomeTown`、 `HomepageUrl`、および`Signature`列です。 さらに含める追加のテキスト ボックス、CreateUserWizard コントロールのインターフェイスをカスタマイズすることにより、ユーザーにこれらの値を確認することができます。

最初に新しい行を追加する方法を見てみましょう、`UserProfiles`テーブルに、`CreatedUser`既定値を持つイベント ハンドラー。 次に、新しいユーザーのホーム町、ホーム ページ、および署名を収集する追加のフォーム フィールドを含める CreateUserWizard コントロールのユーザー インターフェイスをカスタマイズする方法が表示されます。

### <a name="adding-a-default-row-touserprofiles"></a>既定の行を追加します。`UserProfiles`

[*ユーザー アカウントを作成する*](creating-user-accounts-vb.md)チュートリアルに CreateUserWizard コントロールを追加しました、 `CreatingUserAccounts.aspx`  ページで、`Membership`フォルダーです。 コントロールを CreateUserWizard を確保するためにレコードを追加する`UserProfiles`テーブル ユーザー アカウントの作成時に CreateUserWizard コントロールの機能を更新する必要があります。 これらの変更を行うのではなく、 `CreatingUserAccounts.aspx`  ページで、代わりに新しい CreateUserWizard コントロールを追加してみましょう`EnhancedCreateUserWizard.aspx`ページし、このチュートリアルでは、ある変更を行います。

開く、 `EnhancedCreateUserWizard.aspx` Visual Studio でのページし、ページには、ツールボックスから CreateUserWizard コントロールをドラッグします。 CreateUserWizard コントロールの設定`ID`プロパティを`NewUserWizard`です。 説明したよう、 [*ユーザー アカウントを作成する*](creating-user-accounts-vb.md)チュートリアル、CreateUserWizard の既定のユーザー インターフェイスに必要な情報のビジターをメッセージが表示されます。 この情報が指定されていますコントロール内部的に作成、新しいユーザー アカウント framework では、メンバーシップ、すべてせずに 1 行のコードを記述します。

CreateUserWizard コントロールでは、そのワークフロー内で複数のイベントを発生させます。 CreateUserWizard コントロールが最初に起動訪問者は要求情報を提供し、フォームを送信する、その[`CreatingUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)です。 作成プロセス中に問題がある場合、 [ `CreateUserError`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)が発生します。 ただし、ユーザーが正常に作成される場合は、次に、 [ `CreatedUser`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)が発生します。 [*ユーザー アカウントを作成する*](creating-user-accounts-vb.md)のイベント ハンドラーを作成したチュートリアル、`CreatingUser`イベントを指定されたユーザー名が含まれていないこと、先頭または末尾の空白とすることを確認しますパスワードに、ユーザー名は任意の場所に出現しませんでした。

内の行を追加するために、`UserProfiles`テーブルだけが作成したユーザーの必要がありますのイベント ハンドラーを作成する、`CreatedUser`イベント。 時間によって、`CreatedUser`イベントが発生した、ユーザー アカウントが有効にすると、アカウントのユーザー Id の値を取得するメンバーシップ framework は、既に作成されています。

イベント ハンドラーを作成、`NewUserWizard`の`CreatedUser`イベントし、次のコードを追加します。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

だけで追加したユーザー アカウントのユーザー Id を取得することによって上記のコード開始。 使用してこの情報は、 `Membership.GetUser(username)` 、特定のユーザーと、使用に関する情報を返すメソッド、`ProviderUserKey`ユーザー Id を取得するプロパティです。 CreateUserWizard コントロールでユーザーが入力されたユーザー名を利用し、 [ `UserName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx)です。

次に、接続文字列がから取得されます`Web.config`と`INSERT`ステートメントを指定します。 必要な ADO.NET オブジェクトがインスタンス化され、コマンドを実行します。 コードを割り当てます、 [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx)インスタンスを`@HomeTown`、 `@HomepageUrl`、および`@Signature`パラメーターで、データベースの挿入の効果を`NULL`の値を`HomeTown`、 `HomepageUrl`、および`Signature`フィールドです。

参照してください、`EnhancedCreateUserWizard.aspx`ブラウザー内でページおよび新しいユーザー アカウントを作成します。 その後、Visual Studio に戻りの内容の確認、`aspnet_Users`と`UserProfiles`テーブル (同じように図 12 に戻ります)。 新しいユーザー アカウントが表示されます`aspnet_Users`と対応する`UserProfiles`行 (に`NULL`の値を`HomeTown`、 `HomepageUrl`、および`Signature`)。


[![新しいユーザー アカウントとユーザー レコードが追加されました](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**図 20**: ユーザー アカウントの新規および`UserProfiles`レコードが追加されました ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image60.png))


訪問者が、新しいアカウント情報を提供し、"Create User"ボタンをクリックしたユーザー アカウントの作成は、行に追加した後、`UserProfiles`テーブル。 CreateUserWizard を表示し、その`CompleteWizardStep`、成功メッセージと、[続行] ボタンが表示されます。 [続行] ボタンをクリックすると、ポストバックがアクションは実行されませんでスタックしているユーザーのまま、`EnhancedCreateUserWizard.aspx`ページ。

CreateUserWizard コントロールの経由で続行 ボタンがクリックされたときにユーザーに送信する URL を指定できます[`ContinueDestinationPageUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)です。 設定、`ContinueDestinationPageUrl`プロパティを"~/Membership/AdditionalUserInfo.aspx"です。 これにかかる時間に新しいユーザー `AdditionalUserInfo.aspx`、ここで、表示して設定を更新します。

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>新しいユーザーのホーム町、ホーム ページ、および署名を要求する CreateUserWizard のインターフェイスのカスタマイズ

CreateUserWizard コントロールの既定のインターフェイスは、ユーザー名、パスワード、および電子メールのように唯一のコア ユーザー アカウント情報を収集する必要がある簡易アカウントの作成シナリオで十分です。 しかし、自分のアカウントを作成するときに自分の出身地、ホーム ページ、および署名を入力するビジターを要求する場合でしょうか。 サインアップの詳細情報を収集する CreateUserWizard コントロールのインターフェイスをカスタマイズすることは、この情報を使用することがあります、`CreatedUser`基になるデータベースに追加のレコードを挿入するイベント ハンドラー。

CreateUserWizard コントロールは、一連の順序を定義するページの開発者ができるコントロールを ASP.NET ウィザード コントロールの拡張`WizardSteps`です。 ウィザード コントロールは、アクティブなステップを表示し、次の手順を移動するビジターをできるナビゲーション インターフェイスを提供します。 ウィザード コントロールは、複数の短いステップに長いタスクの分割に最適です。 ウィザード コントロールの詳細については、次を参照してください。 [ASP.NET 2.0 のウィザードの制御のステップ バイ ステップ ユーザー インターフェイスを作成する](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)です。

2 つ CreateUserWizard コントロールの既定のマークアップ定義`WizardSteps`:`CreateUserWizardStep`と`CompleteWizardStep`です。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

最初の`WizardStep`、 `CreateUserWizardStep`、ユーザー名、パスワード、電子メール、およびなどの入力を求めるインターフェイスをレンダリングします。 彼女が示すように訪問者は、この情報を提供し、「ユーザーの作成」をクリックすると、 `CompleteWizardStep`、成功メッセージと、[続行] ボタンが示されます。

追加のフォーム フィールドを含める CreateUserWizard コントロールのインターフェイスをカスタマイズするには、ことを実行できます。

- **1 つまたは複数を新規作成 * * *`WizardStep`* * * 追加のユーザー インターフェイス要素を格納する s**です。 新しいを追加する`WizardStep`、CreateUserWizard をクリックして、"追加/削除`WizardStep`s"を起動するスマート タグからのリンク、`WizardStep`コレクション エディターです。 そこから追加、削除、またはウィザードの手順の順序を変更することができます。 これは、このチュートリアルのために使用するアプローチです。

- **変換、* * *`CreateUserWizardStep`* * *、編集可能なに * * *`WizardStep`* * *。** これに置き換えられます、`CreateUserWizardStep`同等な`WizardStep`をマークアップに一致するユーザー インターフェイスを定義する、 `CreateUserWizardStep`' s。 変換することで、`CreateUserWizardStep`に、`WizardStep`おコントロールの位置を変更したり、このステップに追加のユーザー インターフェイス要素を追加します。 変換する、`CreateUserWizardStep`または`CompleteWizardStep`が編集可能に`WizardStep`コントロールのスマート タグからのリンクを「完了ステップのカスタマイズ」または「をカスタマイズするユーザーの作成ステップ」をクリックします。

- **上記の 2 つのオプションの組み合わせを使用します。**

1 つの重要な点に注意してくださいは CreateUserWizard コントロールでは、内から、「ユーザーの作成」ボタンがクリックされたときに、そのユーザー アカウントの作成プロセスが実行されるその`CreateUserWizardStep`です。 追加がある場合は重要でありません`WizardStep`秒後に、`CreateUserWizardStep`か。

カスタムを追加するときに`WizardStep`CreateUserWizard コントロール、カスタムである追加のユーザー入力を収集するために`WizardStep`前に、または後に配置することができます、`CreateUserWizardStep`です。 前になる場合、`CreateUserWizardStep`カスタムから追加のユーザー入力を収集し`WizardStep`利用、`CreatedUser`イベント ハンドラー。 ただし場合、カスタム`WizardStep`の後に続く`CreateUserWizardStep`時間によって、カスタム`WizardStep`が表示されます、新しいユーザー アカウントが既に作成されて、`CreatedUser`イベントが既に発生しています。

図 21 は、ワークフローを示しています。 ときに、追加した`WizardStep`の前に、`CreateUserWizardStep`です。 追加のユーザー情報が、時間によって収集されたため、`CreatedUser`イベントが発生する必要がありますすべては、更新、`CreatedUser`これらの入力を取得しのものを使用するイベント ハンドラー、`INSERT`ステートメントのパラメーター値 (なく`DBNull.Value`).


[![追加の WizardStep の直前に、CreateUserWizardStep CreateUserWizard ワークフロー](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**図 21**:「CreateUserWizard ワークフローときに、追加`WizardStep`Precedes、 `CreateUserWizardStep` ([フルサイズのイメージを表示するには、をクリックして](storing-additional-user-information-vb/_static/image63.png))


場合、カスタム`WizardStep`配置*後*、 `CreateUserWizardStep`、ただし、ユーザー アカウントの作成処理は実行前に、ユーザーが自分の出身地、ホームページ、または署名を入力します。 このような場合は、この追加情報はデータベースに挿入される、ユーザー アカウントが作成されて、22 の図に示すようにする必要があります。


[![追加の WizardStep は、CreateUserWizardStep の後に続くときに、CreateUserWizard ワークフロー](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**図 22**:「CreateUserWizard ワークフローときに、追加`WizardStep`後は、 `CreateUserWizardStep` ([フルサイズ イメージを表示するに、をクリックして](storing-additional-user-information-vb/_static/image66.png))


図 22 に示すようにワークフローを待機にレコードを挿入する、`UserProfiles`手順 2 が完了後するまでテーブルします。 場合は、訪問者は、手順 1 の後に、ブラウザーを閉じ、ただしに到達したこと、ユーザー アカウント作成されましたに追加されたレコードがない状態`UserProfiles`です。 1 つの回避策は持つレコードが存在する`NULL`または既定の値を挿入`UserProfiles`で、 `CreatedUser` (手順 1 の後に発生) するイベント ハンドラー、および手順 2 が完了したらこの記録し、更新します。 これにより、`UserProfiles`レコードは、ユーザーが登録プロセスが途中終了した場合でも、ユーザー アカウントの追加されます。

このチュートリアルを作成しましょう新しい`WizardStep`以降後に発生した、`CreateUserWizardStep`前に、`CompleteWizardStep`です。 最初の get で WizardStep 配置および構成し、そこでを見てみましょうコード。

CreateUserWizard コントロールのスマート タグから選択、"追加/削除`WizardStep`s"、これにより、`WizardStep`コレクション エディター ダイアログ。 新しい`WizardStep`、設定その`ID`に`UserSettings`、その`Title`"の設定 に、その`StepType`に`Step`です。 配置後に続くことができるように、 `CreateUserWizardStep` (「へのサインアップ、新しいアカウント」) との前に、 `CompleteWizardStep` (「完了」)、図 23 に示すようにします。


[![新しい WizardStep コントロールへ追加 CreateUserWizard](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**図 23**: 新規追加`WizardStep`CreateUserWizard コントロールに ([フルサイズのイメージを表示するをクリックして](storing-additional-user-information-vb/_static/image69.png))


閉じるには、ok をクリックして、`WizardStep`コレクション エディター ダイアログ。 新しい`WizardStep`CreateUserWizard コントロールの更新された宣言型マークアップを検出します。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

新しいメモ`<asp:WizardStep>`要素。 新しいユーザーのホーム町、ホーム ページ、およびここに署名を収集するためのユーザー インターフェイスを追加する必要があります。 デザイナーを使用または宣言の構文では、このコンテンツを入力できます。 デザイナーを使用するには、デザイナーでの手順を参照するスマート タグのドロップ ダウン リストから「の設定」の手順を選択します。

> [!NOTE]
> CreateUserWizard コントロールのスマート タグのドロップダウン リストからステップを選択すると更新[`ActiveStepIndex`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)、開始ステップのインデックスを指定します。 そのため、このドロップダウン リストを使用して、デザイナーで [ユーザー設定] ステップを編集する場合を必ず最初にアクセスして、この手順が表示されるように「記号に、新しいアカウント」に設定、`EnhancedCreateUserWizard.aspx`ページ。


という 3 つのテキスト ボックス コントロールが含まれています「の設定」のステップ内のユーザー インターフェイスを作成する`HomeTown`、 `HomepageUrl`、および`Signature`です。 このインターフェイスを構築した後 CreateUserWizard の宣言型マークアップを次のようになります。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

ブラウザーからこのページにアクセスしてくださいホーム町、ホーム ページ、および署名の値を指定する、新しいユーザー アカウントを作成します。 完了した後、`CreateUserWizardStep`メンバーシップ フレームワークでユーザー アカウントを作成し、`CreatedUser`イベント ハンドラーが実行される、新しい行を追加する`UserProfiles`、データベースが`NULL`値を`HomeTown`、 `HomepageUrl`、および`Signature`. ホーム町、ホーム ページ、および署名に入力された値は使用されません。 最終的な結果は、新しいユーザー アカウントに、`UserProfiles`レコードを`HomeTown`、 `HomepageUrl`、および`Signature`を指定するフィールドがまだ必要です。

ユーザーが入力したホーム町、honepage、および署名の値を受け取り、適切な更新プログラム「の設定」の手順の後にコードを実行する必要があります`UserProfiles`レコード。 ユーザーが、ウィザードの手順の間で移動するたびにコントロールをウィザードの[`ActiveStepChanged`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx)発生します。 このイベントと更新プログラムのイベント ハンドラーを生み出すことができます、`UserProfiles`表に、「Your 設定」の手順が完了するとします。

CreateUserWizard のイベント ハンドラーを追加`ActiveStepChanged`イベントし、次のコードを追加します。

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

場合は、「完了」手順を単に到達したことを確認することで、上記のコードを開始します。 "の設定 ステップの直後に、「完了」手順が発生したため、訪問者に達すると「完了」ステップ「の設定」の手順が彼女に終了したことを意味しますします。

このような場合は、必要があります内のテキスト ボックス コントロールをプログラムで参照する、`UserSettings WizardStep`です。 最初を使用してこの情報は、`FindControl`プログラムで参照するメソッド、 `UserSettings WizardStep`、およびその内からのテキスト ボックスを参照するためにもう一度、`WizardStep`です。 実行する準備ができました、テキスト ボックスが参照されていると、`UPDATE`ステートメントです。 `UPDATE`ステートメントでは同じ数とパラメーターの`INSERT`内のステートメント、 `CreatedUser` 、イベント ハンドラーが、ここでユーザーによって指定されたホーム町、ホーム ページ、および署名の値を使用します。

場所にこのイベント ハンドラーで、次を参照してください。、`EnhancedCreateUserWizard.aspx`ブラウザー内でページとホーム町、ホーム ページ、および署名の値を指定する新しいユーザー アカウントを作成します。 新しいアカウントの作成後にリダイレクトする必要があります、 `AdditionalUserInfo.aspx`  ページで、だけが入力したホーム町、ホーム ページ、および署名情報が表示されます。

> [!NOTE]
> 当社の web サイトの訪問者が新しいアカウントを作成できる 2 つのページが現在:`CreatingUserAccounts.aspx`と`EnhancedCreateUserWizard.aspx`です。 Web サイトのサイト マップし、ログイン ページを指す、 `CreatingUserAccounts.aspx`  ページで、ですが、`CreatingUserAccounts.aspx`ページ ホーム町、ホーム ページ、および署名については、ユーザー プロンプトが表示されず、対応する行は追加されません`UserProfiles`です。 そのため、更新するか、`CreatingUserAccounts.aspx`この機能を提供できるようにのページか、更新、sitemap とログイン ページを参照する`EnhancedCreateUserWizard.aspx`の代わりに`CreatingUserAccounts.aspx`です。 後者のオプションを選択する場合は必ず更新して、`Membership`フォルダーの`Web.config`匿名ユーザーへのアクセスを許可するためのファイル、`EnhancedCreateUserWizard.aspx`ページ。


## <a name="summary"></a>まとめ

このチュートリアルでは、メンバーシップ フレームワーク内のユーザー アカウントに関連付けられているデータのモデリング手法について説明しました。 具体的には、一対多のリレーションシップを一対一のリレーションシップを共有するデータだけでなく、ユーザー アカウントと共有するエンティティをモデル化について説明しました。 この関連する情報を表示、挿入、および更新、およびその他の SqlDataSource コントロールを使用する例をいくつかだった方法を説明しましたさらに、ADO.NET コードを使用します。

このチュートリアルでは、ユーザー アカウントを確認を完了します。 以降、次のチュートリアルでの役割に注目を変換されます。 今後は、ロール フレームワークを見ていきましょういくつかのチュートリアルは、新しいロールを作成する方法を参照してください。 どのような役割を決定する、ユーザーが所属する方法と、ユーザーにロールを割り当てる方法、ロールベースの承認を適用します。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [アクセスして、ASP.NET 2.0 のデータの更新](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0 のウィザード コントロール](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [ASP.NET 2.0 のウィザード コントロールでステップ バイ ステップのユーザー インターフェイスの作成](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [カスタム データ ソース コントロールのパラメーターを作成します。](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [CreateUserWizard コントロールのカスタマイズ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [DetailsView コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [ListView コントロールでデータの表示](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [ASP.NET 2.0 の検証コントロールの例を分析します。](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [挿入を編集し、データを削除します。](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [ASP.NET でのフォームの検証](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [カスタム ユーザー登録情報の収集](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [ASP.NET 2.0 のプロファイル](http://www.odetocode.com/Articles/440.aspx)
- [Asp: ListView コントロール](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [ユーザー プロファイルのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)です。

>[!div class="step-by-step"]
[前へ](user-based-authorization-vb.md)
