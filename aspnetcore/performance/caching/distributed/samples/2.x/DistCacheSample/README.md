# <a name="aspnet-core-distributed-cache-sample"></a>ASP.NET Core の分散キャッシュのサンプル

このサンプルでは、分散キャッシュの使用を示します。 このサンプルで説明するシナリオ、[で ASP.NET Core での分散キャッシュ操作](https://docs.microsoft.com/aspnet/core/performance/caching/distributed)トピック。

運用環境では、サンプル アプリを構成して、SQL Server の分散キャッシュを使用します。 分散の Redis cache を使用するアプリを再構成するには、変更、プリプロセッサ ディレクティブの上部にある、 *Startup.cs* Redis を使用するファイル (`#define Redis // SQLServer`)。 詳細については、次を参照してください。[サンプル コードでプリプロセッサ ディレクティブ](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code)します。
