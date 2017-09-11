作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

このセクションでは、データベースのムービーを管理するクラスを追加します。 データベースで使用できるよう、これらのクラスは [Entity Framework Core](https://docs.microsoft.com/ef/core) (EF Core) を使用します。 EF Core は、記述するデータ アクセス コードを簡略化するオブジェクト リレーショナル マッピング (ORM) フレームワークです。

作成するモデル クラスは、EF Core に対する依存関係がないために、POCO クラス (plain-old CLR オブジェクト、つまり単純な従来の CLR) と呼ばれます。 これらは、データベースに格納されるデータのプロパティを定義します。

このチュートリアルでは、まずモデル クラスを記述し、EF コアによってデータベースが作成されます。 ここで取り上げていない別の方法は、[既存のデータベースからモデル クラスを生成する](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)方法です。

サンプルを[表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie)します。