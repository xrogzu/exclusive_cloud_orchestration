# 流程控制类活动

## 开始-结束节点

![](/assets/7/image119.png)

开始-结束结点

如上图，为一个简单流程，只包含一个开始结点，一个结束结点。只有子流程中才有开始节点,而且有且仅有一个。流程中使用开始端点。



### 开始结点

开始结点是流程开始的标志，没有特殊的配置。开始节点只在子流程中才能使用。

![](/assets/7/image120.png)

显示名是所有结点的公共属性，显示名是结点的名称，用于在流程图中显示，也会在日志里输出。每个结点有一个唯一的id(可在Outline中查看)，用于标识结点，由系统生成，不能修改。

### 结束结点

结束结点表示流程结束，不管结束结点是在主流程中，还是在循环结点内部，都会终止整个流程，有点类似于Java程序中的return语句。

End结点的配置如下：

![](/assets/7/image121.png)

l 返回数据表达式：要返回的数据表达式，这是在流程执行的过程中产生的数据。

流程自动将“返回数据表达式”所指示的数据，经过与表达式相应的内置转换器处理后输出给协议组件处理。

## 连线

各类流程结点间用连线组织下：

![](/assets/7/image122.png)

路径的属性如下图直接在路径的属性里配置。

![](/assets/7/image123.png)

l 异步路径在流程图上显示为绿色。

l 异步路径将会在未来某个时间在后台线程中执行，并且它们执行成功与否不影响主流程执行的结果。

l 异常处理相当于程序中的try－catch，每一条异常捕获路径相当于一个catch。

l 当流程中的某个结点有异常抛出时，可以通过配置捕获异常的分支流程来处理这个异常。

l 捕捉异常的路径图上显示为红色。捕捉异常的条件为true时路径名称是必填项。

. 异常类型：要捕获的异常类型，只有这种类型的异常或其子类异常发生时才会走这条分支。

. 捕捉顺序：当一个结点配置了多条异常捕获路径时，会按这个顺序去逐个判断。注意要把子类的异常处理分支排在前面，否则可能出现父类的异常处理分支阻挡子类异常的处理分支。

## 条件判断结点

条件判断结点分为三类：路径决策结点，表达式决策结点，自定义表达式结点。

### 路径决策结点

![](/assets/7/image124.png)

路径决策

![](/assets/7/image125.png)


l 路径决策结点是表达式配置在路径上的结点。

l 路径决策结点输出的路径需要配置路径名称。

l 路径决策结点可以有多条输出路径，在每条路径上都需要配置Boolean表达式。

l 当流程运行到路径决策结点的时候，会逐一判断每条输出路径的表达式的值，如果表达式的值为true，则流程沿着这条路径继续进行。判断的顺序是由属性“表达式执行顺序”决定的，序号越小的，越先进行判断。

l 条件表达式和表达式执行顺序是隐藏属性，只有当把路径的“定义条件”属性配置为true的时候，这两个属性才会显示。同时，路径在流程图上将会由灰色变为蓝色。

### 表达式决策结点

![](/assets/7/image126.png)

![](/assets/7/image127.png)

l 表达式决策结点是表达式配置在结点上的，并且它的每一条输出路径都有一个名字。

l 当流程运行到表达式决策结点上的时候，会计算表达式的值，表达式的值与哪条输出路径的名字相同，就会走那条路径。如果不存在路径名与表达式的值相同，则会抛异常。

l 表达式包括：变量及常量和公式表达式。表达式的具体使用，请见表达式相关章节。

### 自定义决策结点

![](/assets/7/image128.png)

![](/assets/7/image129.png)

l 自定义动作结点可以在其上注册一个Java类，这个类需要实现ActionHandler接口。一个更便捷的方式是继承AbstractActionHandler抽象类，这里提供很多诸如取得流程变量等的便利方法。

l 自定义决策结点的每一条输出路径都有一个名字。

l 自定义决策结点需要在其上放置一个自定义动作内嵌结点。

l ActionHandler接口只有一个perform()方法，传给它的参数是流程实例，它的返回类型是字符串。

l 当流程运行到自定义决策结点的时候，会执行其上注册的ActionHandler的perform()方法，该方法的返回值与哪条输出路径的名字相同，就会走那条路径。如果不存在路径名与perform()方法返回值相同，则会抛异常。

## 分组结点

![](/assets/7/image130.png)

l 分组结点的内部可以嵌套子结点

l 当流程进入分组结点后，会先执行它内部的子结点，执行完以后再跳到外部结点继续向下执行。

l 当内部结点产生事件后，可以传播到外部结点，也可以配置成不向外传播。

l 外部结点可以直接连接到内部结点，内部结点也可以直接连接到外部结点。

## 循环节点

### ForEach

![](/assets/7/image131.png)


ForEach循环

![](/assets/7/image132.png)

循环表达式配置

l ForEach是一个嵌套的结点，它的内部可以容纳多个子结点，多个子结点构成一个内部的流程。

l ForEach结点需要配置一个集合表达式，这个表达式的值必须是一个集合。

l 当流程运行到ForEach结点的时候，会逐个把集合的每一个元素放到以EngineConstants.CIRCLE_VARIABLE为key的流程变量中，ForEach内部的各个结点都可以使用这个变量。循环index为EngineConstants. CIRCLE_INDEX。

l 当ForEach的集合中的每一个元素都执行一遍内部的流程以后，流程就会跳出ForEach继续运行。

### While

![](/assets/7/image133.png)

![](/assets/7/image134.png)


l While是一个嵌套的结点，它的内部可以容纳多个子结点，多个子结点构成一个内部的流程。

l While结点需要配置一个布尔表达式，这个表达式的值必须是一个Boolean类型

l 当流程运行到While结点时，会判断布尔表达式的值，如果表达式的值为true，则继续进行循环，执行内部的流程。如果表达式的值为false，则跳出循环。在while循环内部，通过一定的逻辑处理，将表达式修改为false,即可跳出循环。

### Do_While

l Do_While与While结点类似，也是一个嵌套的结点，它的内部可以容纳多个子结点，多个子结点构成一个内部的流程。

l Do_While结点需要配置一个布尔表达式，这个表达式的值必须是一个Boolean类型

l 当流程运行到Do_While结点时，首先会执行内部的流程，然后判断布尔表达式的值，如果表达式的值为true，则继续进行循环，执行内部的流程。如果表达式的值为false，则跳出循环。

### Continue

![](/assets/7/image135.png)

![](/assets/7/image136.png)


l Continue结点必须放在循环类结点的内部。

l Continue结点上可以配置一个布尔表达式，如果没有配置布尔表达，则默认为该表达式永远为真。

l 当流程运行到Continue结点时，会判断Continue上的布尔表达式，当表达式的值为true的时候，则结束本轮循环，流程将进行循环条件判断。

### Break

![](/assets/7/image137.png)

![](/assets/7/image138.png)


l Break结点必须放在循环类结点的内部。

l Break结点上可以配置一个布尔表达式，如果没有配置布尔表达，则默认为该表达式永远为真。

l 当流程运行到Break结点时，会判断Break上的布尔表达式，当表达式的值为true的时候，则结束循环，流程将跳到循环结点之后继续执行。

### 失败重试结点

失败重试结点，是一个嵌套、循环结点，内部可以嵌套一个流程片断，如果内嵌的流程出现异常，失败重试结点可以重复执行内部流程片断。还可在重复执行动作后指定一个任务处理。

![](/assets/7/image139.png)

![](/assets/7/image140.png)


l 重复次数(retryCount)：内部流程片断失败（发生异常），重复失败结点最大重试次数。

l 重复间隔(interval)：两次重试动作之间的时间间隔，以毫秒(ms)为单位。

l 异常类型(Excetpion Type)：失败重试结点的重试条件，即如果内部发生此类异常才进行重试操作。

l 是否指定任务(hasTask)：指定最大重试次完成后是否需要指定特定的任务，以进行人工干预。

l 任务定义(Task Definition)：此项设置在指定任务为true时才显示，这里进行任务的具体设置：任务名称，描述，执行者，系统默认处理地址为：http://127.0.0.1:8080/taskhandleservice

l 执行完任务后，会再执行一遍内部流程片断。

## Fork-Join节点

![](/assets/7/image141.png)

![](/assets/7/image142.png)


Fork结点属性

![](/assets/7/image143.png)

路径条件配置

![](/assets/7/image144.png)


Join结点属性

l Fork-Join是分支聚合结点，从Fork结点分出来的每一条分支路径都有机会被执行，并且最后于Join结点处汇聚。

l Fork结点有一个属性是否同步，表示多个分支是同步的执行，还是异步的执行。同步的执行表示多条路径在同一个线程中逐条执行。异步执行表示为每一条路径分配一个线程并行执行。

l 在Fork的每条输出路径上可以配置一个布尔表达式，如果布尔表达式为true，这条路径将被执行，如果为false，这条路径将不被执行。如果不配置布尔表达式，默认为true。

l Join结点的进入连线必须设置名称，用于Join节点合并MessageContext使用。将会以路径名作为key,分支MessageContext作为value.传递给实现类。

l 当分支结点改变了MessageContext结构时，则需要在Join节点配置合并后的API定义，并选择相应的方法。其次还需要创建MessageContext合并类，继承父类AbstractMessageContextHandler，并返回MessageContext.

## 异步节点

![](/assets/7/image145.png)

![](/assets/7/image146.png)




路径属性

l 异步结点表示流程在这个结点上除了沿主路径执行外，还可以连接几条异步的路径。

l 异步结点必须连接且只能连接一个同步路径，但是异步路径可以连接多条。

l 异步路径将会在未来某个时间在后台线程中执行，并且它们执行成功与否不影响主流程执行的结果。

l 路径是否异步可以直接在路径的属性里配置，异步路径在流程图上显示为绿色。

## 子流程结点

![](/assets/7/image147.png)

![](/assets/7/image148.png)


上方为主流程，下方为子流程。上方子流程结点的子流程ID属性标明引用的流程ID。子流程ID唯一定位一个流程，其结构如下图：

![](/assets/7/image149.png)

l 子流程结点可以引用其它的流程，作为当前流程的子流程。

l 子流程结点有一个属性“子流程ID”，用来指定它所引用的子流程的ID。

l 当流程运行到子流程结点时，将会执行指定的子流程，当子流程执行结束后，再返回到这个结点，继续主流程的执行。

l 流程的修改会导致版本号的增加，如果要引用修改后的子流程，用户需要重新选择。

l 流程会在集成项目部署时自动发布，但子流程须在有主流程中有引用的时候，随着一起发布。

l 在定义流程的时候，如果是子流程，右键新建子流程即可。

## 任务结点

![](/assets/7/image150.png)

![](/assets/7/image151.png)


l 任务结点属于等待结点的一种。当流程运行到等待类型的结点时，流程会等在那里，不会继续向下运行，直到有一个外部消息唤醒该流程。

l 任务结点主要包含任务定义属性，任务定义又包含如下属性：

. 任务名称：概括任务的主题

. 任务描述：简要描述任务的内容

. 任务处理者：可以通过参照的方式从系统数据库中选择人员。

. 完成任务的url：外系统可以通过提交http请求到这个url，给流程发消息，来完成任务，唤醒流程继续运行。系统默认处理地址为：

http://127.0.0.1:8080/taskhandleservice

l 任务的通知：任务结点可以配置一个动作处理器，可以通过配置“服务调用动作处理器”来访问其它业务系统的服务，以把任务的内容通知到相应的业务系统中。此处理器是在任务产生之后，完成之前运行。

l 任务的完成：在ESB中提供了一个ITaskService的osgi服务，可以通过建立一个业务组件，引用这个ITaskService服务来完成任务。并且对外提供相应的协议服务，来接收其它业务系统的调用。

## 等待结点

![](/assets/7/image152.png)


l 对于等待类型的结点，可以配置定时器，当流程进入到等待结点的时候，定时器开始启动，当定时器到期时，会发布相应的事件或使流程沿着指定的流程执行。当流程离开等待结点后，定时器将会被销毁，销毁的定时器不会有任何动作。

l 流程结点可以配置属性、定时器、变量、事件监听。

l 等待结点的定时器配置在选择信号名称时，需要用户配置等待结点输出路径上的名称：

![](/assets/7/image153.png)

l 定时器的其它配置参见“定时器”一节

l 事件的配置参见“事件监听”一节

## 自定义动作

自定义动作结点不能独立存在，它必须内嵌到任务结点或自定义决策结点上。当流程运行到这些结点的时候，会调用所配置的动作类的perform()方法。

自定义动作可以注册一个自定义的ActionHandler，可以点后面的按钮进行选择或新建一个继承com.ufida.eip.pvmengine.AbstractActionHandler的子类。

![](/assets/7/image154.png)

图：自定义动作属性配置 