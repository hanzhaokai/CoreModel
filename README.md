![image](https://github.com/CharlinFeng/Resource/blob/master/CoreModel/logo.jpg)<br/><br/>


##更新特性太多，文档重制中！陆续为您呈现！
##创业初期精力有限，本人最后一个开源OC框架：因您而精彩！


<br/><br/><br/>
一、CoreModel使用前言
==========

<br/>
#### 1.为什么要重制？
首先感谢大量朋友对我的框架的喜欢，同时也提出了各种问题和要求，最重要的有以下：<br/>
> (1). 不支持NSData。<br/>
> (2). 不支持NSArray。<br/>
> (3). 全部主线程操作，对性能有一定的影响。<br/>

<br/>
#### 2.框架依赖
> (1). CoreFMDB 数据库操作。<br/>
> (2). CoreHttp 网络请求：第四季及第五季用到。<br/>
> (3). CoreStatus 网络状态检测：第四季及第五季用到。<br/>
> (4). MJExtension 整个框架仅仅用了他的遍历成员属性这唯一的一个功能，别无他用。<br/>


<br/>
#### 3.其他说明
> (1). 强烈建议关注：[信息公告牌](https://github.com/CharlinFeng/Show)以便获取最实时的框架更新动态。<br/>
> (2). 开源第四季动态缓存的条件为：`CoreModel的Star数据超过1000`。<br/>
> (3). 之前有朋友过于喜欢我的框架，导致在没有任何说明的情况下借用我的代码，所以本次框架去除了所有的中文注释。从使用的角度上来说对您没有任何影响。<br/>
> (4). 请添加异常断点，以便捕获我提供的大量断言。<br/>
> (5). 特别提醒：示例程序有强烈的先后顺序，最好不要随便乱点，比如一个数据都没有insert，你点击了update或者delete等操作会达到你难以理解的结果。<br/>


<br/>
#### 4.最终申明
在开始之前，请您注意以下几点：<br/>
>(1). 导入了sqlite3.lib 动态库。<br/>
>(2). 拖拽CoreModel及FrameWorks文件夹到您的项目。<br/>
>(3). 安装了Navicat Preminum。<br/>


<br/><br/><br/>
二、基本使用
==========
<br/>
####新建模型Person，继承自CoreModel，模型加入以下属性：

    #import "CoreModel.h"
    
    @interface Person : CoreModel
    
    @property (nonatomic,copy) NSString *name;
    
    @property (nonatomic,assign) NSInteger age;
    
    @end


<br/><br/><br/>
三、全自动创表
==========
<br/>
#### 框架全自动创表触发的条件：调用CoreModel子类的任意一个方法。
##### 本功能请参考项目中：Test2VC.m
现在，我们随意调用Person的任意一个方法，这里我们以实例化一个Person实例说明：

    /** 全自动创表 */
    Person *person = [[Person alloc] init];

,查看控制台输出，已经发现成功创建了数据库，成功创建表，并写入了字段信息：

        sqlite> PRAGMA table_info (Person);
        +------+--------+---------+---------+------------+------+
        | cid  | name   | type    | notnull | dflt_value | pk   |
        +------+--------+---------+---------+------------+------+
        | 0    | id     | INTEGER | 1       | 0          | 1    |
        | 1    | name   | TEXT    | 1       | ''         | 0    |
        | 2    | age    | INTEGER | 1       | 0          | 0    |
        | 3    | hostID | INTEGER | 1       | 0          | 0    |
        | 4    | pModel | TEXT    | 1       | ''         | 0    |
        | 5    | pid    | INTEGER | 1       | 0          | 0    |
        +------+--------+---------+---------+------------+------+
        6 rows in set (0.02 sec)

说明：<br/>
>(1). 框架在控制台输出的第一条信息就是您的DB位置：dbPath:path.sql，请注意因为iOS沙盒机制有变化，这个Path会不停的变化，你使用Navicat Preminum查看数据库文件的时候，最好是每次都更新path（在数据库连接上右键，修改当前DB的path）。<br/>
>(2). 有您不熟悉的字段如pid,pModel是框架辅助字段，请不要删除。<br/>
>(3). 核心字段：hostID是服务器主键，任何使用CoreModel的模型必须拥有唯一的hostID，如果您没有hostID，请构建。<br/>



<br/><br/><br/>
四、调试模式与非调试模式
==========
<br/>
#### 框架有极其全面的Debug信息与断言帮助您正确的使用CoreModel
##### 如果您不喜欢控制台大量输出Debug信息，请到CoreModelConst.h文件，修改以下宏定义：

    /** Debug */
    #define CoreModelDeBug 1

，如果您修改为1，即是Debug模式，控制台会有大量输出，如果您不需要显示，改为0即可。


<br/><br/><br/>
五、全部子线程运行 + Block回调，性能卓越！
==========
<br/>
#### 我们还是打开Debug模式，请查看控制台各种关于线程的输入
##### 不仅仅是创建是这样，后面所有关于数据库的CURD操作，全部是在子线程中完成的，基本Operation，同时有并发限制，多线程在本框架得到完美展现，性能不再是问题！

    2015-09-09 10:58:44.291 CoreModel[3427:3903] dbPath:/Users/Charlin/Library/Developer/CoreSimulator/Devices/E1B1C2D8-DC98-4571-AF45-8A6D76F07497/data/Applications/9174CFF6-3EA1-4BDB-904C-C5373E0E00E0/Documents/CoreModel/CoreModel.sql
    2015-09-09 10:58:44.293 CoreModel[3427:3903] 表创建完毕<NSThread: 0x7a679c80>{name = (null), num = 2}
    2015-09-09 10:58:44.293 CoreModel[3427:3903] 字段检查所在线程：<NSThread: 0x7a679c80>{name = (null), num = 2}
    2015-09-09 10:58:44.294 CoreModel[3427:3903] 字段也检查完毕<NSThread: 0x7a679c80>{name = (null), num = 2}
    2015-09-09 10:58:44.294 CoreModel[3427:3903] 创表所在线程：<NSThread: 0x7a679c80>{name = (null), num = 2}

#### 注意：因为是子线程，所以您的block回调，全部是子线程，如果你在block里面更新UI，或者Push界面，需要自行回到主线程。你可能会问我的block为什么不在主线程中回调，因为有的时候block回调需要可能还有一定的数据处理，在主线程中执行同样会有一定的性能浪费。这一点在第四季和第五季中有强烈的展现。所以，决定权交给您自己。


<br/><br/><br/>
五、模型字段检查，全自动增加字段
==========
<br/>
有时候你可能有这样的需求，开发到一定阶段或者版本，需要增加模型字段。CoreModel已经完全为您考虑了这种情况，下面我们修改Person模型，增加一个Height字段，结果如下：

    @interface Person : CoreModel
    
    @property (nonatomic,copy) NSString *name;
    
    @property (nonatomic,assign) NSInteger age;
    
    @property (nonatomic,assign) CGFloat height;
    
    @end
 
 ,此时我们再向Person发送任意消息，并查看控制台输出：
 
    2015-09-09 11:06:47.269 CoreModel[3474:3903] 注意：模型 Person 有新增加的字段 height,已经实时添加到数据库中！
    
,我们打开数据库查看表结构：

    sqlite> PRAGMA table_info (Person);
    +------+--------+---------+---------+------------+------+
    | cid  | name   | type    | notnull | dflt_value | pk   |
    +------+--------+---------+---------+------------+------+
    | 0    | id     | INTEGER | 1       | 0          | 1    |
    | 1    | name   | TEXT    | 1       | ''         | 0    |
    | 2    | age    | INTEGER | 1       | 0          | 0    |
    | 3    | hostID | INTEGER | 1       | 0          | 0    |
    | 4    | pModel | TEXT    | 1       | ''         | 0    |
    | 5    | pid    | INTEGER | 1       | 0          | 0    |
    | 6    | height | REAL    | 1       | 0.0        | 0    |
    +------+--------+---------+---------+------------+------+
    7 rows in set (0.04 sec)

我们发现，字段已经成功增加。

##### 你可能会问，如果我想删除一个属性，框架会自动删除字段吗？答案是否定的，原因有以下：

>(1). 如果删除字段，之前的有关此字段的数据全部丢失，如果你后期又想使用此字段，就造成了遗憾。
>(2). Sqlite 3本身无删除字段语法，有技术可以实现，但对有大量数据的表来说有极大的操作风险。


<br/><br/><br/>
五、断言
==========
<br/>
##### 本功能请参考项目中：Test3VC.m
#### 为了让您正确的使用CoreModel，框架做了大量的断言帮助您正确的使用，最容易出现的错误就是对HostID认识不够深刻，假如您的模型没有设置HostID，会触发断言：

    NSAssert(coreModel.hostID > 0, @"错误：数据插入失败,无hostID的数据插入都是耍流氓，你必须设置模型的模型hostID!");

此外，以下不合法操作均会触发断言：
>(1). 模型混用，比如使用Cat类方法对Person执行数据操作如[Cat insert:person resBlock:nil];<br/>
>(2). 数组支持中，数组申明OC数组成员为NSInteger、CGFloat、Bool等（后面会有详细介绍）。<br/>


注意：

>(1). HostID是对应服务器表的主键，在CoreModel中hostID会自动映射解析服务器json里面的id字段，你无需手动映射。<br/>
>(2). 有的朋友issue我说，他们服务器没有返回id主键，可不可以不传hostID？首先服务器数据如果涉及缓存，不传id本身就不是很规范，再者本地缓存数据是不可信任的，只有服务器的数据才是最可靠的，即是CoreModel的最核心的就是hostID，同时在第四季与第五季中，各种强大的功能全部是基于hostID完成，如果您的数据没有hostID或者是您自己手动保存的缓存数据，请结合CoreFMDDB的CountTable功能构建hostID。<br/>



<br/><br/><br/>
六、基本模型 + 单条数据插入
==========
<br/>

#### 我们构建合法的Person对象，并执行Insert操作：
##### 本功能请参考项目中：Test4VC.m

    Person *person = [[Person alloc] init];
    person.hostID = 1;
    person.name = @"冯成林";
    person.age = 28;
    person.height = 174.0;
    /** Insert */
    [Person insert:person resBlock:^(BOOL res) {
        [self show:res];
    }];

，运行结果提示成功，并且控制台输出全部是在子线程中完成，我们查看数据库：

    sqlite> select * from Person;
    +----+-----------+-----+--------+--------+-----+--------+
    | id | name      | age | hostID | pModel | pid | height |
    +----+-----------+-----+--------+--------+-----+--------+
    | 1  | 冯成林    | 28  | 1      |        | 0   | 174.0  |
    +----+-----------+-----+--------+--------+-----+--------+
    1 rows in set (0.05 sec)

##### 注： 当你再次运行Test4VC.m，会发现提示失败，那是因为hostID为1的数据已经存在，

    错误：Person表中hostID=1的数据记录已经存在！
    
你也可以通过查看控制台,能够看到上面的Debug输出，明白这是合理的。


<br/><br/><br/>
七、基本模型 + 批量数据插入
==========
<br/>
##### 本功能请参考项目中：Test5VC.m

    Person *p1 = [[Person alloc] init];
    p1.hostID = 2;
    p1.name = @"jack";
    p1.age = 25;
    p1.height = 180;
    
    Person *p2 = [[Person alloc] init];
    p2.hostID = 3;
    p2.name = @"jim";
    p2.age = 22;
    p2.height = 172;
    

    [Person inserts:@[p1,p2] resBlock:^(BOOL res) {
        [self show:res];
    }];

#### 提示成功，数据库返回结果如下：

    sqlite> select * from Person;
    +----+-----------+-----+--------+--------+--------+-----+
    | id | name      | age | height | hostID | pModel | pid |
    +----+-----------+-----+--------+--------+--------+-----+
    | 1  | 冯成林    | 28  | 174.0  | 1      |        | 0   |
    | 2  | jack      | 25  | 180.0  | 2      |        | 0   |
    | 3  | jim       | 22  | 172.0  | 3      |        | 0   |
    +----+-----------+-----+--------+--------+--------+-----+
    3 rows in set (0.01 sec)


<br/><br/><br/>
八、基本模型 + 单条数据修改
==========
<br/>
##### 本功能请参考项目中：Test6VC.m
##### 这里我修改了我的姓名和身高字段值，请注意前后对比：

    Person *person = [[Person alloc] init];
    person.hostID = 1;
    person.name = @"Charlin Feng";
    person.age = 28;
    person.height = 173.5;
    [Person update:person resBlock:^(BOOL res) {
        [self show:res];
    }];

#### 执行结果：

    sqlite> select * from Person;
    +----+--------------+-----+--------+--------+--------+-----+
    | id | name         | age | height | hostID | pModel | pid |
    +----+--------------+-----+--------+--------+--------+-----+
    | 1  | Charlin Feng | 28  | 173.5  | 1      |        | 0   |
    | 2  | jack         | 25  | 180.0  | 2      |        | 0   |
    | 3  | jim          | 22  | 172.0  | 3      |        | 0   |
    +----+--------------+-----+--------+--------+--------+-----+
    3 rows in set (0.01 sec)



<br/><br/><br/>
九、基本模型 + 批量数据修改
==========
<br/>
##### 本功能请参考项目中：Test7VC.m
##### 这里我修改了姓名字段值，请注意前后对比：

    Person *p1 = [[Person alloc] init];
    p1.hostID = 2;
    p1.name = @"杰克";
    p1.age = 25;
    p1.height = 180;
    Person *p2 = [[Person alloc] init];
    p2.hostID = 3;
    p2.name = @"吉姆";
    p2.age = 22;
    p2.height = 172;
    [Person updateModels:@[p1,p2] resBlock:^(BOOL res) {
        [self show:res];
    }];

#### 执行结果：

    sqlite> select * from Person;
    +----+--------------+-----+--------+--------+--------+-----+
    | id | name         | age | height | hostID | pModel | pid |
    +----+--------------+-----+--------+--------+--------+-----+
    | 1  | Charlin Feng | 28  | 173.5  | 1      |        | 0   |
    | 2  | 杰克         | 25  | 180.0  | 2      |        | 0   |
    | 3  | 吉姆         | 22  | 172.0  | 3      |        | 0   |
    +----+--------------+-----+--------+--------+--------+-----+
    3 rows in set (0.01 sec)


<br/><br/><br/>
十、基本模型 + 单条数据保存
==========
<br/>
##### 本功能请参考项目中：Test8VC.m
#### 请注意数据保存(Save) 和数据插入(Insert)是有区别的
##### Insert是简单的数据插入，如果数据存在要么抛出错误，要不返回不处理
##### Save是指保存数据时，进行智能判断，如果数据记录不存在，执行Insert操作。如果数据已经存在，执行Update操作，总之，执行Save操作后，你指定的数据一定会作为最新数据记录在数据库中。

模型数据：修改了名称与年龄，请注意前后比对：

    Person *p1 = [[Person alloc] init];
    p1.hostID = 2;
    p1.name = @"杰克先生";
    p1.age = 40;
    p1.height = 180;
    Person *p2 = [[Person alloc] init];
    p2.hostID = 3;
    p2.name = @"吉姆先生";
    p2.age = 38;
    p2.height = 172;
    [Person saveModels:@[p1,p2] resBlock:^(BOOL res) {
        [self show:res];
    }];

#### 执行结果：

    sqlite> select * from Person;
    +----+--------------+-----+--------+--------+--------+-----+
    | id | name         | age | height | hostID | pModel | pid |
    +----+--------------+-----+--------+--------+--------+-----+
    | 1  | Charlin Feng | 28  | 173.5  | 1      |        | 0   |
    | 2  | 杰克先生     | 40  | 180.0  | 2      |        | 0   |
    | 3  | 吉姆先生     | 38  | 172.0  | 3      |        | 0   |
    +----+--------------+-----+--------+--------+--------+-----+
    3 rows in set (0.01 sec)
<br/>
注意：
> (1). 如果是单条数据保存，请使用 `+(void)save:(id)model resBlock:(void(^)(BOOL res))resBlock;`<br/>
> (2). 如果是批量数据保存，请使用 `+(void)saveModels:(NSArray *)models resBlock:(void(^)(BOOL res))resBlock`<br/>
> (3). 有一种情况比较特殊，就是你不清楚是单条还是批量（CoreModel内部有遇到此种情况并使用了此方法），请使用 `+(void)saveDirect:(id)obj resBlock:(void(^)(BOOL res))resBlock`<br/>
> (4). 再次提醒，此方法会导致数据一定写入数据库，和insert与update有区别，请知晓。



<br/><br/><br/>
十一、基本模型 + 条件删除
==========
<br/>
##### 本功能请参考项目中：Test9VC.m
##### 我们删除年龄大于等于40岁的记录：

    [Person deleteWhere:@"age >= 40" resBlock:^(BOOL res) {
        [self show:res];
    }];

#### 执行结果，杰克先生因为年龄刚好40岁，所以被删除：

    sqlite> select * from Person;
    +----+--------------+-----+--------+--------+--------+-----+
    | id | name         | age | height | hostID | pModel | pid |
    +----+--------------+-----+--------+--------+--------+-----+
    | 1  | Charlin Feng | 28  | 173.5  | 1      |        | 0   |
    | 3  | 吉姆先生     | 38  | 172.0  | 3      |        | 0   |
    +----+--------------+-----+--------+--------+--------+-----+
    2 rows in set (0.02 sec)


说明：您还可以根据主键一键删除，注意主键指的是hostID，而非本地数据库的主键id。




<br/><br/><br/>
十二、基本模型 + 条件查询
==========
<br/>
##### 本功能请参考项目中：Test10VC.m
##### 为了更好的检验数据查询是否成功，CoreModel增加了description方法,直接打印即可。

    [Person selectWhere:nil groupBy:nil orderBy:nil limit:nil selectResultsBlock:^(NSArray *selectResults) {
        
        NSLog(@"%@",selectResults);
    }];
    
#### 执行结果，  

    2015-09-09 14:52:18.806 CoreModel[5335:3a03] (
        "[Person]<0x7c199670>: 
          name: Charlin Feng, 
          age: 28, 
          height: 173.5, 
          hostID: 1, 
          pModel: , 
          pid: 0, 
    ",
        "[Person]<0x7c19a330>: 
          name: \U5409\U59c6\U5148\U751f, 
          age: 38, 
          height: 172, 
          hostID: 3, 
          pModel: , 
          pid: 0, 
    "
    )

请注意：
> (1). 因为我们刚刚测试了删除，删除了一条记录，所以现在结果当然是两条。<br/>
> (2). 框架对CoreModel做了desctiontion自动处理，您能直接看到以上结果。<br/>
> (3). 和删除类似，您也可以根据hostID(再次提示，非本地数据库id)快速查找一条记录,使用的方法为:`+(void)find:(NSUInteger)hostID selectResultBlock:(void(^)(id selectResult))selectResultBlock;`<br/>



<br/><br/><br/>
十三、基本模型 + 清空表数据
==========
<br/>
##### 本功能请参考项目中：Test11VC.m

    [Person truncateTable:^(BOOL res) {
    }];

注意：清空了所有表记录，同时重置了本地数据库的主键id，慎用！


<br/><br/><br/>
十四、基本模型 + 清空表数据
==========
<br/>






