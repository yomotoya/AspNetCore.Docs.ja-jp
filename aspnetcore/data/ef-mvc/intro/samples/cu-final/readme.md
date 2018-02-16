# <a name="contoso-university-sample-app"></a>Contoso 大学サンプル アプリ

Contoso 大学では、ASP.NET Core MVC Web アプリケーションで Entity Framework Core を使用する方法を示します。

## <a name="build-it-from-scratch"></a>ゼロから作成する

アプリケーションをビルドするには、[チュートリアルのシリーズ](https://docs.microsoft.com/aspnet/core/data/ef-mvc/intro)の手順に従います。

## <a name="download-it"></a>ダウンロードする

[aspnet/Docs リポジトリ](https://github.com/aspnet/Docs)をダウンロードするか複製することで、[完成したプロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)を GitHub からダウンロードし、ローカル ファイル システムの `aspnetcore\data\ef-mvc\intro\samples\cu-final` に移動します。  プロジェクトをダウンロードした後に、コマンド ライン プロンプトに「`dotnet ef database update`」と入力してデータベースを作成します。 別の方法として、**パッケージ マネージャー コンソール**を使用することができます。詳細については、「[Command-line interface (CLI) vs.Package Manager Console (PMC)](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#command-line-interface-cli-vs-package-manager-console-pmc)」(コマンドライン インターフェイス (CLI) とパッケージ マネージャー コンソール (PMI) の比較) を参照してください。
