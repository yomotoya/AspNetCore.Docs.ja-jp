---
title: "ビューに依存関係の挿入"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: ff3ded36a04fdbba0628dc5f223bfd865d58612a
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="dependency-injection-into-views"></a>ビューに依存関係の挿入

によって[Steve Smith](https://ardalis.com/)

ASP.NET Core をサポートしている[依存性の注入](xref:fundamentals/dependency-injection)の表示にします。 これは、ローカライズや要素の表示を設定するためだけに必要なデータなどのビューに固有のサービスに役立ちます。 管理しようとする必要があります[関心の分離](http://deviq.com/separation-of-concerns/)コント ローラーとビューの間です。 表示するビューは、ユーザー データのほとんどは、コント ローラーからに渡されます。

[サンプル コードを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)

## <a name="a-simple-example"></a>単純な例

使用してビューに、サービスを挿入することができます、`@inject`ディレクティブです。 考えることができます`@inject`プロパティを表示します を追加して、DI を使用してプロパティを設定するとします。

構文`@inject`:`@inject <type> <name>`

例`@inject`の動作。

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]

このビューの一覧を表示`ToDoItem`全体的な統計を示す概要と共にのインスタンス。 概要が表示されます、挿入されたから`StatisticsService`です。 このサービスに登録されてで依存性の注入`ConfigureServices`で*Startup.cs*:

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]

`StatisticsService`のセットに対して計算をいくつかを実行`ToDoItem`リポジトリを使用してアクセスするインスタンス。

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]

サンプル リポジトリでは、メモリ内コレクションを使用します。 上記の実装が、リモートでアクセスされる大規模なデータ セットは (これはすべてメモリ内のデータの動作) は推奨されません。

このサンプルには、ビューにバインドされたモデルとビューに挿入される、サービスからデータが表示されます。

![表示、アイテムの合計を一覧表示するには、項目、平均の優先度、およびその優先度レベルと完了を示すブール値を使用してタスクの一覧を完了します。](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a>参照データを設定します。

ビューの挿入をドロップダウン リストなどの UI 要素のオプションを設定することができます。 性別、状態、およびその他の設定を指定するためのオプションを含むユーザー プロファイルのフォームを検討してください。 これらのオプションのセットの各データ アクセス サービスを要求し、モデルを作成し、コント ローラーが必要になります、標準の MVC アプローチを使用して、このようなフォームを表示または`ViewBag`をバインドするオプションのセットごとにします。

その他の方法では、オプションを取得するビューに直接サービスを挿入します。 これには、このビューの要素の構築ロジックをビュー自体に移動し、コント ローラーで必要なコードの量が最小限に抑えます。 プロファイルの編集フォームに表示するコント ローラーのアクションは、のみ、フォームにプロファイル インスタンスを渡す必要があります。

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]

HTML フォーム使用してこれらの設定を更新するにはには、プロパティの 3 つのドロップダウン リストが含まれています。

![名前、性別、状態、および好きな色のエントリを許可するフォームで [プロファイル] ビューを更新します。](dependency-injection/_static/updateprofile.png)

ビューに挿入された、サービスによっては、これらのリストが設定されます。

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]

`ProfileOptionsService`このフォームに必要なデータだけを提供するよう設計された UI レベル サービスには。

[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]

>[!TIP]
> 忘れずに登録されている種類の依存性の注入を通じて要求が、`ConfigureServices`メソッド*Startup.cs*です。

## <a name="overriding-services"></a>サービスをオーバーライドします。

新しいサービスを提供するだけでなくこの手法をページ上の前に挿入したサービスを上書きする使用もできます。 次の図は、最初の例で使用されるページのすべての利用可能なフィールドを示します。

![Intellisense のコンテキスト メニューで、型指定された @ 記号を Html、コンポーネント、StatsService、および Url のフィールドを一覧表示します。](dependency-injection/_static/razor-fields.png)

既定のフィールドを含めることがわかります`Html`、 `Component`、および`Url`(だけでなく`StatsService`挿入お)。 インスタンスに、独自の既定の HTML ヘルパーを置き換えたい、でした簡単に行うを使用して`@inject`:

[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]

既存のサービスを拡張する場合からの継承、または独自の既存の実装の折り返しの中に単にこの手法を使用できます。

## <a name="see-also"></a>関連項目

* Simon Timms ブログ:[ビュー内に参照データの取得](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)
