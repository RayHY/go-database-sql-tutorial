---
layout: article
title: 导入数据库驱动
---

为了使用`database/sql`,你需要同时引用`database/sql`本身和一个针对你想使用的数据库的相应驱动。

一些数据库驱动作者鼓励你越过`database/sql`直接使用驱动包，一般而言，这不是一个好选择。通常情况下，你都应该使用`database/sql`而不是直接使用驱动：你的代码最好只使用`database/sql`中定义好的数据类型和相应方法。这意味着你能保证你的代码独立于你使用的特定驱动，这样你可以改动很少的代码以更换底层驱动以及相应的数据库。这样做同时也保证了你的代码遵循了Go语言程序的惯例，而不是依赖于某个特定驱动包作者提供的AD-HOC接口。

在本文档中，我们用 @julienschmidt 和 @arnehormann 编写的[MySQL
drivers](https://github.com/go-sql-driver/mysql)作为示例。

在你Go源代码文件的顶部加入如下几行

<pre class="prettyprint lang-go">
import (
	"database/sql"
	_ "github.com/go-sql-driver/mysql"
)
</pre>

注意在代码中，我们使用`_`作为驱动包的别名来匿名（驱动的变量在源文件中不可见）导入驱动包。驱动在导入时会隐式地把自己注册到包`database/sql`中。总之，除了init函数会被调用外，没有做其它任何操作。

加入上面这几行代码后，你就可以访问数据库了。

**Previous: [Go语言database/sql库概览](zh-CN/overview.html)**
**Next: [访问数据库](zh-CN/accessing.html)**
