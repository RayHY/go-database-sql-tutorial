---
layout: article
title: 访问数据库
---

导入驱动包后，你需要用`sql.Open()`创建一个数据库对象`*sql.DB`:

<pre class="prettyprint lang-go">
func main() {
	db, err := sql.Open("mysql",
		"user:password@tcp(127.0.0.1:3306)/hello")
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()
}
</pre>

上面的例子中有几点需要说明:

1. `sql.Open`的第一个参数是驱动名。这个字符串被驱动用来在`database/sql`底层注册自己。一般而言第一个参数和驱动包名相同以避免冲突，如[github.com/go-sql-driver/mysql](https://github.com/go-sql-driver/mysql)需要设置参数为`mysql`。不过也有一些驱动包名和参数名不一样，比如[github.com/mattn/go-sqlite3](https://github.com/mattn/go-sqlite3)需要设置为`sqlite3`，[github.com/lib/pq](https://github.com/lib/pq)需要设置为`postgres`。

2. `sql.Open`的第二个参数是每个驱动特有的连接字符串，告诉驱动如何连接底层数据库。在本例中，我们尝试连接本地MySQL服务器的"hello"数据库。

3. `database/sql`的各种操作返回的错误一般都必须检查。当然，也存在一些没有必要检查错误的特殊情况，这种情况很少发生，我们以后再讨论。

4. 如果`sql.DB`的生命周期不需要超过函数的范围，我们习惯及时使用`defer db.Close()`来保证函数结束时能关闭数据库连接。

也许与你的直觉相反，`sql.Open()` **没有实际建立起到数据库的连接**
同时也没有验证数据库连接参数是否可用，它只是创建了一个数据库的抽象接口。当你实际需要一个数据库连接执行语句时，`sql.DB`才会在底层建立数据库连接。如果你想在得到`sql.DB`对象后立马检查一下它是否可用(比如你想检查下网络是否可以连接或者账户是否可用)，你需要调用`db.Ping()`函数，再次强调要记得检查是否有错误发生：

<pre class="prettyprint lang-go">
err = db.Ping()
if err != nil {
	// do something here
}
</pre>

虽然前面强调要记得及时执行`Close()`关闭数据库连接，但是 **`sql.DB`对象实际上是为长连接(long-lived)而设计的**。不要对数据库频繁地执行`Open()`和`Close()`. 总而言之，在你程序中数据库相关的事务完成之前，每个单独的数据库都最好只创建一个`sql.DB`对象，对数据库的所有访问都通过这一个`sql.DB`对象来进行。你可以将其作为参数自由地传递或者将其注册为全局变量，只要保证它在用完之前没有关闭就行。不要在生命周期很短的函数调用中`Open()`和`Close()`数据库，而要将`sql.DB`作为这个函数的参数来执行SQL语句。

如果你使用`sql.DB`时忘记了它是一个长连接(long-lived)对象，经常打开关闭`sql.DB`，你可能为此面临严重的问题：无法复用和共享连接，耗尽网络资源，由于TCP连接保持在TIME_WAIT状态而间断性的失败等……这些问题表明你可能没有按照`database/sql`的设计理念行事。

现在是时候学习如何使用`sql.DB`对象了！

**Previous: [Importing a Database Driver](importing.html)**
**Next: [Retrieving Result Sets](retrieving.html)**
