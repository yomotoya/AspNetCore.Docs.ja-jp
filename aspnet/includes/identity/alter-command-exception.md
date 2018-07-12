> <span data-ttu-id="bdab5-101">いくつかのコマンドは、アプリは、その Id のデータ ストアとして SQLite を使用する場合にサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="bdab5-101">Some commands aren't supported if the app uses SQLite as its Identity data store.</span></span> <span data-ttu-id="bdab5-102">データベース エンジンの制限により`Alter`コマンドは、次の例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="bdab5-102">Due to limitations in the database engine, `Alter` commands throw the following exception:</span></span>
>
> <span data-ttu-id="bdab5-103">"System.NotSupportedException: SQLite では、この移行操作はサポートされていません"。</span><span class="sxs-lookup"><span data-stu-id="bdab5-103">"System.NotSupportedException: SQLite does not support this migration operation."</span></span> 
>
> <span data-ttu-id="bdab5-104">回避策としては、テーブルを変更するには、データベース、Code First migrations を実行します。</span><span class="sxs-lookup"><span data-stu-id="bdab5-104">As a work around, run Code First migrations on the database to change the tables.</span></span>
