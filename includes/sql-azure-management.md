
# 使用 SQL Server Management Studio 管理 Azure SQL 数据库 

你可以使用 Azure SQL 数据库 管理门户或 SQL Server Management Studio (SSMS) 客户端应用程序来管理你的 SQL 数据库 订阅以及创建并管理关联的逻辑服务器和数据库。下面的指南介绍如何使用 Management Studio 管理 SQL 数据库 逻辑服务器和数据库。

<div class="dev-callout-new-collapsed">
<strong>注意 <span>单击可折叠</span></strong>
<div class="dev-callout-content">
<p>你可以使用 SQL Server 2014、SQL Server 2012 或 SQL Server 2008 R2 版本的 SSMS。不支持早期版本。</p>
</div>
</div>

本主题包括下列步骤：

-   [步骤 1：获取 SQL Server Management Studio][]
-   [步骤 2：连接到 SQL 数据库][]
-   [步骤 3：创建并管理数据库][]
-   [步骤 4：创建并管理登录名][]
-   [步骤 5：使用动态管理视图监视 SQL 数据库][]

<h2><a id="Step1" name="Step1"> </a>步骤 1：获取 Management Studio</h2>

Management Studio 是用于管理 SQL 数据库 的
集成环境。在管理 
Azure 上的数据库时，你可以使用随
SQL Server 一起安装的 Management Studio 应用程序或下载免费的 SQL Server 2012 Management Studio Express (SSMSE) 版本。下面的步骤介绍
如何安装 SSMSE。

1.  在 [Microsoft SQL Server 2012 Express][] 页上，选择 x86 版本的 Management Studio（如果你运行的是 32 位操作系统）或 x64（如果你运行的是 64 位操作系统）。单击"下载"，然后在出现提示时，运行安装程序。

2.  单击"全新 SQL Server 独立安装或向现有安装添加功能"，然后单击"确定"。

3.  接受许可协议，然后单击"下一步"。

4. 单击"安装"来安装 SQL Server 安装程序所需的文件。

5.  在"功能选择"屏幕上，预先选择了"管理工具 - 基本"。这是因为你运行的是 Management Studio 的安装程序。如果你运行的是所有 SQL Server Express 的安装程序，请选择"管理工具 - 基本"选项，然后单击"下一步"。

6.  在"错误报告"屏幕上，你可以选择将
    错误报告发送给 Microsoft。这对使用 SSMSE 来说不是必需的。单击
    "下一步"以开始安装。

7.  在安装完成后，你将看到"完成"
    页面。单击"关闭"。 


<h2><a id="Step2" name="Step2"> </a>步骤 2：连接到 SQL 数据库</h2>

连接到 SQL 数据库 需要你知道在 Azure 上的服务器名称。你可能需要登录到门户来获取此信息。

1.  登录到 [Azure 管理门户][]。

2.  在左侧窗格中，单击"SQL 数据库"。

3.  在 SQL 数据库 主页上，单击页面顶部的"服务器"以列出与你的订阅关联的所有服务器。查找要连接到的服务器的名称，然后将它复制到剪贴板上。

	接下来，将你的 SQL 数据库 防火墙配置
为允许从你的本地计算机连接。通过将你的本地计算机 IP 地址添加到防火墙例外列表中来执行此操作。

1.  在 SQL 数据库 主页上，单击"服务器"，然后单击要连接到的服务器。

2.  单击页面顶部的"配置"。

3.  复制"当前客户端 IP 地址"中的 IP 地址。

4.  在"配置"页中，"允许的 IP 地址"包括三个框，你可以在其中指定规则名称和作为开始和结束值的 IP 地址范围。对于规则名称，你可以输入你的计算机的名称。对于开始和结束范围，将你的计算机的 IP 地址粘贴到两个框中，然后单击显示的复选框。

	规则名称必须是唯一的。如果这是你的开发计算机，则你可以将 IP 地址输入到 IP 范围开始框和 IP 范围结束框中。否则，你可能需要输入一组范围更广泛的 IP 地址来容纳来自你组织中的其他计算机的连接。
 
5. 单击页面底部的"保存"。

    **注意：**在防火墙设置的更改生效之前，可能
    最多有五分钟的延迟。

	你现在已准备好使用 Management Studio 连接到 SQL 数据库。

1.  在任务栏上，单击"开始"，指向"所有程序"，指向"Microsoft SQL Server 2012"，然后单击"SQL Server Management Studio"。

2.  在"连接到服务器"中，指定
     *serverName*.database.chinacloudapi.cn 作为完全限定的服务器名称。在 Azure 上，服务器名称是由字母数字字符组成的自动生成的字符串。

3.  选择"SQL Server 身份验证"。

4.  在"登录"框中，输入你在创建服务器时在门户中指定的 SQL Server 管理员登录名，格式为
    *login*@*yourServerName*。

5.  在"密码"框中，输入你在创建服务器时在门户中指定的密码。

8.  单击"连接"以建立连接。

在 Azure 上，每个 SQL 数据库 逻辑服务器是定义数据库分组的抽象。每个数据库的物理位置可能在数据中心中的任何计算机上。 

在以前版本中，在 Management Studio 中设置连接时，你必须直接连接到 **master**。此步骤不再是必需的。现在，有了服务器名称、身份验证类型和管理员凭据，连接就会成功。

你可用于任务（例如在 SQL Server 数据库上
创建和修改登录名和数据库）的许多 SSMS 向导
不可用于 Azure 上的 SQL 数据库，因此你将需要利用
Transact-SQL 语句来完成这些任务。下面的步骤
提供了这些语句的示例。有关
将 Transact-SQL 与 SQL 数据库 结合使用的详细信息（包括有关受支持命令的详细信息），
请参阅 [Transact-SQL 参考 (SQL 数据库)][]。

<h2><a id="Step3" name="Step3"> </a>步骤 3：创建并管理数据库</h2>

连接到 **master** 数据库时，你可以
在服务器上创建新数据库以及修改或删除现有数据库。下面的步骤
介绍如何通过 Management Studio 完成
几个常见数据库管理任务。若要执行这些任务，请确保
使用你在设置服务器时所创建的服务器级别主体登录名连接到 **master**
数据库。

若要在 Management Studio 中打开查询窗口，请打开 Databases 文件夹，展开"系统数据库"，右键单击"master"，然后单击"新建查询"。

-   使用 **CREATE DATABASE** 语句可创建新数据库。下面的语句创建一个新数据库，名称为
    **myTestDB** 的新数据库并指定它是 Web 版数据库
    且最大大小为 1 GB。

        CREATE DATABASE myTestDB
        (MAXSIZE=1GB,
         EDITION='web');

单击"执行"来运行查询。

-   使用 **ALTER DATABASE** 语句来修改现有数据库，
    例如，在想要更改数据库的名称、最大大小
    或版本（企业或 Web 版）时。
    下面的语句修改你在上一步中创建的数据库来将最大大小更改为 5 GB。

        ALTER DATABASE myTestDB
        MODIFY
        (MAXSIZE=5GB,
         EDITION='web');

-   使用 **DROP DATABASE** 语句可删除现有数据库。
    下面的语句将删除 **myTestDB**
    数据库，但现在没有删除它，因为你将在下一步中使用它创建登录名。

        DROP DATABASE myTestBase;

-   master 数据库具有你可用于查看有关所有数据库的详细信息的 **sys.databases**
    视图。若要查看所有现有数据库，
    请执行以下语句：

        SELECT * FROM sys.databases;

-   在 SQL 数据库 中，不支持将 **USE** 语句用于在数据库
    之间切换。你需要改为建立直接到
    目标数据库的连接。

<div class="dev-callout-new">
 <strong>注意 <span>单击可折叠</span></strong>
 <div class="dev-callout-content">
   <p>创建或修改数据库的许多 Transact-SQL 语句
必须在其自己的批处理中运行，无法与其他 Transact-SQL 语句
分组在一起。有关详细信息，请参阅上面列出的链接中提供的特定于语句的信息。</p>
</div>
</div>

<h2><a id="Step4" name="Step4"> </a>步骤 4：创建并管理登录名</h2>

**master** 数据库跟踪登录名
以及哪些登录名有权创建数据库或其他登录名。通过使用
你在设置服务器时创建的服务器级别主体登录名连接到 **master**
数据库来管理登录名。你可以使用
**CREATE LOGIN**、**ALTER LOGIN** 或 **DROP LOGIN** 语句
对将管理整个服务器上的登录名的 master 数据库
执行查询。有关详细信息，请参阅[在 SQL 数据库 中管理数据库和登录名][]。 


-   使用 **CREATE LOGIN** 语句可创建新的服务器级别
    登录名。下面的语句将创建一个名为
    **login1** 的新登录名。将 **password1** 替换为你选择的
    密码。

        CREATE LOGIN login1 WITH password='password1';

-   使用 **CREATE USER** 语句来授予数据库级别
    权限。必须在 **master** 数据库中创建所有登录名，
    但若要使登录名连接到不同的数据库，你
    必须使用 **CREATE USER** 语句授予它对
    该数据库的数据库级别权限。 

-   若要为 login1
    提供对名为 **myTestDB** 的数据库的权限，请完成以下
    步骤：

 1.  若要刷新对象资源管理器以查看你刚才创建的 **myTestDB** 数据库，请在对象资源管理器中右键单击服务器名称，然后单击"刷新"。  

     如果你关闭了连接，可以通过选择"文件"菜单上的"连接对象资源管理器"来重新连接。重复[步骤 2：连接到 SQL 数据库][] 中的说明来连接到数据库。

 2. 右键单击 **myTestDB** 数据库并选择"新建查询"。

    3.  对 myTestDB 数据库执行以下语句
        来创建名为 **login1User** 的数据库用户
        （对应于服务器级别登录名 **login1**）。

            CREATE USER login1User FROM LOGIN login1;

-   使用 **sp\_addrolemember** 存储过程为用户帐户
    提供对数据库的适当级别的权限。有关
    详细信息，请参阅 [sp_addrolemember (Transact-SQL)][]。下面的语句将为 **login1User**
    提供对数据库的只读权限，方法是将 **login1User** 添加到
    **db\_datareader** 角色。

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   例如，如果你要更改用于登录的密码，请使用 **ALTER LOGIN** 语句
    修改现有登录名。应对 **master** 数据库
    运行 **ALTER LOGIN** 语句。切换回连接到该数据库的查询窗口。 

    下面的语句修改 **login1** 登录名来重置密码。
    使用你选择的密码替换 **newPassword**，并
    使用登录名对应的当前密码替换 **oldPassword**。

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   使用 **DROP LOGIN** 语句来删除现有登录名。
    删除处于服务器级别的登录名也会删除任何关联的
    数据库用户帐户。有关详细信息，
    应对 **master**
    数据库运行 **DROP LOGIN** 语句。下面的
    语句将删除 **login1** 登录名。

        DROP LOGIN login1;

-   master 数据库具有你可用于查看登录名的 **sys.sql\_logins**
    视图。若要查看所有现有登录名，请执行
    以下语句：

        SELECT * FROM sys.sql_logins;

<h2><a id="Step5" name="Step5"> </a>步骤 5：使用动态管理视图监视 SQL 数据库</h2>

SQL 数据库 支持多个
可用于监视单个数据库的动态管理视图。下面是你可通过
这些视图检索的监视器数据类型的一些示例。有关
完整的详细信息和更多用法示例，请参阅[使用动态管理视图监视 SQL 数据库][]。

-   查询动态管理视图需要 **VIEW DATABASE STATE**
    权限。若要向特定数据库用户授予 **VIEW DATABASE STATE**
    权限，请连接到要使用你的服务器
    级别主体登录名管理的数据库
    并对该数据库执行以下语句：

        GRANT VIEW DATABASE STATE TO login1User;

-   使用 **sys.dm\_db\_partition\_stats** 视图计算数据库
    大小。**sys.dm\_db\_partition\_stats** 视图返回
    数据库中每个分区的页和行计数信息，你可以
    使用这些信息来计算数据库大小。下面的查询返回
    数据库的大小（以 MB 为单位）：

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   使用 **sys.dm\_exec\_connections** 和 **sys.dm\_exec\_sessions**
    视图来检索与数据库关联的当前用户连接
    和内部任务的信息。下面的查询
    返回有关当前连接的信息：

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   使用 **sys.dm\_exec\_query\_stats** 视图可检索
    缓存的查询计划的整体性能统计信息。下面的查询
    的查询返回有关按平均 CPU 时间排名的前五个查询的
    信息。

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;

<h2>其他资源</h2>

* [在 SQL 数据库 中管理数据库和登录名][]
* [使用动态管理视图监视 SQL 数据库][]
* [将用户添加到你的 SQL 数据库 中][]
* [Transact-SQL 参考 (SQL 数据库)][]

  [如何使用 Azure SQL 数据库]: /documentation/articles/sql-database-dotnet-how-to-use/
  [步骤 1：获取 SQL Server Management Studio]: #Step1
  [步骤 2：连接到 SQL 数据库]: #Step2
  [步骤 3：创建并管理数据库]: #Step3
  [步骤 4：创建并管理登录名]: #Step4
  [步骤 5：使用动态管理视图监视 SQL 数据库]:#Step5
  [Microsoft SQL Server 2012 Express]: http://www.microsoft.com/zh-cn/download/details.aspx?id=29062
  [SSMS 安装程序 - 选择安装类型]: ./media/installer_installation_type.png
  [SSMS 安装程序 - 选择功能]: ./media/installer_feature_selection.png
  [SSMS 安装程序 - 安装完成]: ./media/installer_completed.png
  [Azure 管理门户]: http://manage.windowsazure.cn/
  [从管理门户获取 SQL 数据库 服务器名称]: ./media/portal_get_database_name.png
  [连接到 SSMS]: ./media/ssms_connect.png
  [连接到 SSMS - 属性]: ./media/ssms_connect_properties.png
  [Transact-SQL 参考 (SQL 数据库)]: http://msdn.microsoft.com/zh-cn/library/bb510741(v=sql.120).aspx
  [在 SQL 数据库 中管理数据库和登录名]: http://msdn.microsoft.com/zh-cn/library/azure/ee336235.aspx
  [sp_addrolemember (Transact-SQL)]: http://msdn.microsoft.com/zh-cn/library/ms187750.aspx
  [使用动态管理视图监视 SQL 数据库]: http://msdn.microsoft.com/zh-cn/library/azure/ff394114.aspx
  [将用户添加到你的 SQL 数据库 中]: http://blogs.msdn.com/b/sqlazure/archive/2010/06/21/10028038.aspx

<!--HONumber=55-->