#### Meteor 是一个建立现代网站的极简单环境. 在过去即使使用最好的工具，也需要数周的时间，使用Meteor只需要几个小时。

Web 最早在70年代专门为大型机所设计. 应用服务器渲染完整个屏幕，然后在网络上发送给哑终端. 无论用户做什么事情，服务器都会重新渲染整个屏幕. 在过去的十几年中，这个模式在 Web 上工作得很好. 它成就了 LAMP, Rails, Django, PHP 等技术。

但是现在，一些优秀的团队，他们拥护充足的预算和时间，开始在客户端使用Javascript 构建应用程序. 这些应用程序的界面是固定的. 它们不重新载入页面. 它们具有“反应性”（reactive）: 改变会从某个客户端立即显示到所有人的屏幕上.

他们构建这些采取了困难的方案. Meteor 可以使之变得非常简单和有趣. 你可以选择在周末，或是弥漫着咖啡香气的黑客聚会上，来构建一个完整的应用. 你再也不需要准备服务器资源, 在云上部署API端点, 管理数据库, 争论是否使用ORM层, 在前端JavaScript和后Ruby之间反复交换, 以及当数据失效时广播给所有客户端.

Meteor 是一项正在开展的工作, 我们希望它能够展现出我们所思考的方向. 我们乐于听到你的反馈意见.

— Geoff, Nick, Matt, David, Avital, and David


## 快速开始！

接下来的任务，所有的[支持的平台](https://github.com/meteor/meteor/wiki/Supported-Platforms)。

安装 Meteor：
	
	$ curl https://install.meteor.com | /bin/sh

新建一个项目：

	$ meteor create myapp

在本地运行：

	$ cd myapp
	$ meteor
	Running on: http://localhost:3000/

释放到 world (我们提供的免费服务器)上：

	$ meteor deploy myapp.meteor.com

## 七项“纪律”

* 数据上线。不要在网络上发送HTML。发送数据，然后让客户端决定如何渲染它们。
* 统一语言。在服务器端和客户端统一使用 JavaScript 语言。
* 数据库随处可用。在客户端或服务器使用相同的透明API来访问你的数据库。
* 延迟补偿。在客户端使用预取和模型模拟，以使数据库连接看上去是零延时的。
* Full Stack Reactivity。使默认是实时的。所有的层，从数据库到模板，应该是有有效的事件驱动接口。
* 拥抱社区。Meteor 开放源代码，并且整合而不是替代已存的开源工具和框架。
* 简单等同效率。让事情看上去简单的最好的办法，就是让它根本就很简单。通过干净、经典、优美的API来达成。

## 开发者资源

如果 Metero 已经引起了你的兴趣，我们希望你能参与到项目中来


#### STACK OVERFLOW
	
提出（还有解答！）技术问题最好的地方就是[Stack Overflow](http://stackoverflow.com/questions/tagged/meteor).记得在你的问题上加个“**meteor**”标签。

#### 邮件列表

我们有两个邮件列表。meteor-talk@googlegroups.com 用于常见问题，请求帮助，和新项目公告。meteor-core@googlegroups.com 用于 Meteor 内部讨论和建议调整。

#### IRC

irc.freenode.net 上的 \#meteor 频道。开发者们挂在这里随时回答你的问题，尽其所能。

#### GITHUB

代码放在[GitHub](http://github.com/meteor/meteor)上。为 Metero 打个补丁最好的办法，就是在 GitHub 上发送个“求拉“”请求。记录一个bug的最好方式，就是用 GitHub 的 bug tracker。

---
#概念
我们靠手工写单页的 JavaScript 应用。写一个完整的应用，用同一种语言(JavaScript)，同一种数据格式(JSON)，是一件相当有趣的事。Meteor 就是我们写这些应用所需要的全部。

## 构造你的应用程序

一个 Meteor 应用程序，混合了运行在客户端浏览器中的JavaScript，运行在 Meteor 服务器 Node.js 容器内的 JavaScript，以及所有 HTML 片段，CSS 规则，以及其他的静态资源。Meteor自动打包和传输这些不同内容。并且，你可以在你的文件树中相当灵活地组织这些内容。

唯一的服务器资源就是 JavaScript 。Meteor 聚合你的所有 JavaScript 文件，除了 client 和 public 子目录下的文件，然后将他们集中在一起加载到 Node.js 服务器实例中。在 Meteor 中，你的服务器端代码针对每一个请求都运行在一个单线程中，而不是典型的Node风格的异步回调函数中。我们发现在 Metero 应用程序里，线性的执行模式更适合典型的服务器代码。

考虑到客户端有很多资源。Meteor 为客户端将你目录树里所有 server 和 public 子目录以外的 JavaScript 文件聚在一起。它会最小化捆绑并发送给新访问的客户端。你可以为你的应用程序自由选择，使用一个单文件 JavaScript ，或彼此独立的文件，或者两者之间的形式。

client、server 和 tests 以外的文件，在客户端和服务器端两边都会被加载！那是定义模型和其他函数的地方。Meteor 提供了两个变量 isClient 和 isServer，这样你的代码可以根据他们是运行在客户端还是服务器来改变行为。（名为 tests 目录内的文件不会加载到任何地方。）

任何你不希望发送给客户端的代码，例如包含了密码或认证机制的代码，都应该保持在 server 目录内。

另外 CSS 文件会被集中在一起：客户端会得到一个含有你的目录树下的所有CSS文件的“捆绑”（不包括 server 和 public 子目录）。

在开发模式下，JavaScript 和 CSS 文件会为了调试方便而单独发送。

HTML文件在 Meteor 应用程序中被处理的方式和普通服务器端框架稍有不同。Meteor 扫描你目录内所有HTML文件中的3个顶层元素：“<head>”,“<body>”,和“<template>”。head 和 body 区会分别包含到初始载入页面的head 和 body 中，并发送给客户端。

模板区则会被转换成 Template命名空间下的 JavaScript 函数。这是一个将 HTML 传递给客户端的非常方便的方式。参考后面的模板章节。

最后，Meteor 会伺服 public 目录下的任何文件，和 Rails 或 Django 项目一样。这是放图片、favicon.ico、robots.txt 和其他文件的地方。

It is best to write your application in such a way that it is insensitive to the order in which files are loaded，例如使用 Meteor.startup，或者将加载顺序敏感的代码放到智能包中，它能明确地控制它们的内容的加载顺序，已经它们所依赖的包的加载顺序。不过有时候在你的应用程序中，加载顺序依赖仍然是无法避免的。应用程序中的 JavaScript 和 CSS 文件的加载顺序依据以下规则：

* 应用程序根目录下的 lib 目录里的文件最先加载。
* 然后是所有文件名匹配 main.* 的文件。
* 子目录里的文件在父目录之前加载，所以先加载最深层的文件，根目录下的文件最后加载。
* 在一个目录中，文件按照文件名的字母顺序加载。

这些规则叠加作用，就是说，举个例子，在 lib 目录内，文件首先都是按照文件名的字母顺序来加载的，然后有一些 main.js 文件，在这些文件当中，子目录下的会加载得早一些。

## 数据和安全

Meteor能使分布在客户端的代码，简化到就像访问本地数据库一样。干净、简单，并且安全，不需要实现一个个 RPC 端点，人为地在客户端缓存数据以避免和服务器之间来回传递，当数据发生变化时小心地协调所有的客户端。

在 Meteor 中，客户端和服务器共享相同的数据库API。完全相同的应用程序代码——例如校验器和计算好的属性——常常能在两个地方运行。但是代码在服务器上运行时，是直接访问数据库，而在客户端则不是。这个差异是 Meteor 的数据安全模式的基础。

> 一个新的 Meteor 应用默认包含了 autopublic 和 insecure 包，它们模拟客户端拥有对服务器数据库的完全读/写能力。这是个有用的原型工具，但是对于发布级产品并不合适。你在发布之前应该删除这两个包。

每个Meteor客户端都包含一份保留在内存中的数据库缓存。服务器发布JSON文档集合(sets of JSON documents)，客户端订阅这些文档集合，以管理客户端的缓存。当集合中的文档发生变化时，服务器发送“补丁”给每个客户端缓存。

每个文档集合都由客户端上的“发布”函数(publish function)定义。每当有一个新的客户端订阅文档集合时就执行“发布”函数。文档集合中的数据可以来自任何地方，但是最常见的情况是发布(publish)一个数据库查询。

	// 服务器: 发布所有房间的文档
	Meteor.publish("all-rooms", function () {
	  return Rooms.find(); // everything
	);
	
	// 服务器: 发布给定房间的所有消息
	Meteor.publish("messages", function (roomId) {
	  return Messages.find({room: roomId});
	});

	// 服务器: 发布登陆用户能看到的所有聚会的集合
	Meteor.publish("parties", function () {
	  return Parties.find({$or: [{"public": true},
	                   {invited: this.userId},
	                   {owner: this.userId}]});
	});
	
发布函数可以给每个客户端提供不同的结果。在上面的例子当中，登陆用户只能看到公开的、用户拥有的，和用户被邀请的聚会。

一旦订阅，客户端使用它的缓存就像高速本地数据库一样，代码简单得出奇。读取时从来不需要在服务器的往返之间消耗。缓存中的内容会被限制：在客户端查询表(collection)中的文档时，只有服务器发布给客户端的文档会被返回。

	// 客户端：开始一个聚会订阅
	Meteor.subscribe("parties");
	
	// 客户端：返回这个客户端能够读取的聚会数组
	return Parties.find().fetch(); // 同步的!

完备的客户端能开启或关闭订阅，以控制如何保持缓存和管理网络流量。当一个订阅被关闭，它的所有文档都会从缓存中删除，除非另一个激活的订阅提供了相同的文档。

当客户端改变了一个或多个文档，它会发送一个消息到服务器来请求这个改动。服务器通过一个你以JavaScript函数形式写的允许/拒绝规则集，来检查这个改变提议。只有当所有规则都通过时，服务器才接受改变。

	// 服务器: 不允许客户端插入一个聚会
	Parties.allow({
	  insert: function (userId, party) {
	    return false;
	  }
	});

	// 客户端: 这个操作会失败
	var party = { ... };
	Parties.insert(party);

如果服务器接受改变，就会讲变化应用到数据库，并且自动将改变散布给其他订阅了受影响文档的客户端。如果不接受，更新操作失败，服务器上的数据库不变，也没有其他客户端会看到更新。

Meteor 有个聪明的机制。当客户端提议写入服务器，它就会立刻更新自己的本地缓存，不需要等待服务器的回应。这意味着屏幕会立刻重绘。如果服务器接受改变——在行为合理的客户端中大部分情况下都会如此——客户端更新屏幕不需要等待服务器。如果服务器拒绝改变，Metero 用服务器的结果来修正客户端的缓存。

言而总之，这些技巧实现了延迟补偿。客户端用户它所需的最新数据，并且从来不需要等待和服务器之间的往返。当客户端修改数据，这些改动会在本地立即执行而不会等待来自服务器的确认，但服务器对改变请求给出最终定夺。

Meteor包含了Meteor账户，一个文艺范儿的认证系统。主要体现在登陆密码所使用[远程安全密码协议](http://en.wikipedia.org/wiki/Secure_Remote_Password_protocol)(Secure Remote Password protocol)，外部服务整合，包括Facebook，GitHub，Google，Twitter，还有新浪微博。Meteor账户定义了一个名为Meteor.users的表(collection)，开发者可以在里面存放一些应用程序特定的用户数据。

Meteor还为一些通常的任务预建了表单，例如登陆，注册，修改密码，用邮箱来修改密码。你能用一行代码就添加一个账户界面到你的应用中。账户界面甚至还明智地打包了一个配置向导，来引导你在你的应用里安装外部登陆服务。

> Meteor 当前版本支持 MongoDB，流行的文档数据库，这儿有使用[MongoDB API](http://www.mongodb.org/display/DOCS/Manual)的例子。未来会支持其他数据库。

## Reactivity

Meteor 拥抱[无功编程](http://en.wikipedia.org/wiki/Reactive_programming)(reactive programming)的概念。这意味着，你可以以简单的命令风格写代码，然后结果自动重新计算当你的代码依赖的数据变化时。

	Meteor.autosubscribe(function () {
	  Meteor.subscribe("messages", Session.get("currentRoomId"));
	});

这个例子（来自聊天室客户端）装置了一个基于会话(session)变量 currentRoomId 的数据订阅。Session.get("currentRoomId") 因为任何原因发生变化，函数就会自动重新运行，装置一个新的订阅来替换之前的。

自动重计算靠 会话(Session)和 Meteor.autosubscribe 的合作来实现。Meteor.autosubscribe这类方法在其跟踪的数据依赖项内安置一个“reactive 上下文(context)”，然后准备重新执行所需的必备参数。另一方面，数据提供者例如Session，在这个上下文中记录下他们被谁调用和哪些数据被请求，然后准备好当数据改变时发送一个失效信号。

这个简单的模式（reactive上下文 + reactive数据源）用途广泛。程序员不必写取消订阅/重新订阅的调用代码，也能保证在正确的时候被调用。一般来讲，Meteor能避免你去写各种数据传播代码，它们是会对应用程序造成影响的容易出错的逻辑。

下列Meteor函数会在reactive上下文中运行你的代码：

* Templates
* Meteor.render 和 Meteor.renderList
* Meteor.autosubscribe
* Meteor.autorun

能够触发变化的reactive数据源：

* Session variables
* Database queries on Collections
* Meteor.status
* Meteor.user
* Meteor.userId
* Meteor.loggingIn

Meteor 的reactive实现短小精悍，只有大约50行代码。你可以用Meteor.deps模块“勾入”到里面，以增加新的reactive上下文和数据源。

## 活动HTML（Live HTML）

HTML 模板是web应用程序的中心。通过Meteor的活动页面更新技术，你可以reactive地渲染HTML，就是说它会跟踪生成它的数据里的变化来自动更新。

这个可选的特性能跟任何HTML模板库工作，甚至你在JavaScript中手动生成的HTML。这里有个例子：

	var fragment = Meteor.render(
	  function () {
	    var name = Session.get("name") || "Anonymous";
	    return "<div>Hello, " + name + "</div>";
	  });
	document.body.appendChild(fragment);

	Session.set("name", "Bob"); // 页面自动更新！
	
Meteor.render接收一个渲染函数，就是说，用一个函数来返回字符串格式的HTML。它会返回一个自动更新的 DocumentFragment 。当有一个渲染函数用过的数据发生变化时，它就会重新运行。DocumentFragment里的DOM元素不管插入在页面上的何处，都能在他们所在的位置更新。这完全是自动的。Meteor.render根据reactive上下文来发现那些被渲染函数所使用的数据。

大部分时间，虽然你不会直接访问这些函数——你只需要使用你喜欢的模板库，例如Handlebars或Jade。render 和 renderList 函数是为实现新模板系准备的。

Meteor 只在你的代码不做处理的情况下，批量执行所有必要的更新。所以，你不必担心DOM会不受你控制。有时你也需要相反的行为。例如，你只是插入了一条记录到数据库，你可能想强制DOM更新，一遍使用类如jQuery的工具找到新元素。在这种情况下，使用Meteor.flush使DOM立刻更新。

手写应用程序的另一个烦恼是元素保留。假设用户正在向<input>元素输入文本，然后页面中包含这个元素的区域发生了重绘。用户停留的界面部分会抖动，失去焦点，光标位置，已经输入的文本部分都会在重建<input>后丢失。

这个问题Meteor已经为你解决了。你可以在模板上用preserve指令来指定需要在模板重新渲染后保留的元素。Meteor会保留这些元素，哪怕包含他们的文档已经重新渲染，但是仍然会更新他们的成员，并将所有发生变化的属性复制过来。

## 模板

Meteor可以很容易使你喜欢的HTML模板语言，例如Handlebars或Jade，独立于Meteor的活动页面更新技术。你只要正常编写模板，Meteor会小心处理在正确的时候更新它们。

使用这个特性，在你的项目内创建扩展名为.html的文件。这文件中，建一个<template>标签，然后给一个name属性。在标签内写模板内容。Meteor将会预编译这个模板，传送到客户端，然后作为全局对象Template下的一个函数。

> 目前，被打包道Meteor中的模板系统只有Handlebars。告诉我们你想在Meteor中使用何种模板系统。同时，也可以参考[Handlebar文档](http://www.handlebarsjs.com/)和[Meteor Handlebar扩展](https://github.com/meteor/meteor/wiki/Handlebars)

一个名为hello的模板通过调用函数 Template.hello来渲染，并传递一些数据给模板：

	<template name="hello">
	  <div class="greeting">Hello there, {{first}} {{last}}!</div>
	</template>

	// 在 JavaScript 控制台里
	> Template.hello({first: "Alyssa", last: "Hacker"});
	 => "<div class="greeting">Hello there, Alyssa Hacker!</div>"

这会返回一个字符串。在活动HTML系统之外使用模板，使DOM元素在适当的地方自动更新，使用Meteor.render:

	Meteor.render(function () {
	  return Template.hello({first: "Alyssa", last: "Hacker"});
	})
	  => 自动更新的 DOM 元素
	
在模板里取得数据最简单的办法就是在JavaScript里定义helper函数。只要在Tempate.[模板名称]对象下直接添加函数即可。例如，在这个模板里：

	<template name="players">
	  {{#each topScorers}}
	    <div>{{name}}</div>
	  {{/each}}
	</template>

我们可以定义一个函数在Template.players里，来代替传入 topScorers 数据：

	Template.players.topScorers = function () {
	  return Users.find({score: {$gt: 100}}, {sort: {score: -1}});
	};
	
这样，数据便来自数据库查询。一旦数据库的指针传递给了#each，就会根据数据库的查询结果添加和移动DOM节点。

helper 能够带参数，他们接收自当前模板里的数据：

	// 在JavaScript文件里
	Template.players.leagueIs = function (league) {
		return this.league === league;
	} ;
.

	<template name="players">
	  {{#each topScorers}}
	    {{#if leagueIs "junior"}}
	      <div>Junior: {{name}}</div>
	    {{/if}}
	    {{#if leagueIs "senior"}}
	      <div>Senior: {{name}}</div>
	    {{/if}}
	  {{/each}}
	</template>

> Handlebars 注意：能够用{{#if leagueIs "junior"}}是因为有一个能够允许将helper嵌入到块级helper里的Meteor扩展。（if 和 leagueIs 都是同样的 helper，普通的Handlerbars是不能在这个位置调用leagueIs的。）

helper也可以传入常量：

	// 同样在 {{#each sections}} 里工作
	Template.report.sections = ["Situation", "Complication", "Resolution"];

最后，你可以在模板函数上使用events声明，来安装事件处理句柄。格式在[Event Maps](http://docs.meteor.com/#eventmaps)。事件句柄函数中的this是触发事件元素的数据上下文。

	<template name="scores">
	  {{#each player}}
	    {{> playerScore}}
	  {{/each}}
	</template>

	<template name="playerScore">
	  <div>{{name}}: {{score}}
	    <span class="givePoints">Give points</span>
	  </div>
	</template>
.

	Template.playerScore.events({
	  'click .givePoints': function () {
	    Users.update({_id: this._id}, {$inc: {score: 2}});
	  }
	});

归总起来，这有一个例子演示了如何塞入各种数据到模板里，然后只要这些数据发生变化，界面就自动更新。详见上面的 活动HTML 部分。

	<template name="forecast">
	  <div>It'll be {{prediction}} tonight</div>
	</template>
.

	// JavaScript: reactive helper function
	Template.forecast.prediction = function () {
	  return Session.get("weather");
	};
.

	> Session.set("weather", "cloudy");
	> document.body.appendChild(Meteor.render(Template.forecast));
	In DOM:  <div>It'll be cloudy tonight</div>

	> Session.set("weather", "cool and dry");
	In DOM:  <div>It'll be cool and dry tonight</div>

## 智能包

Meteor有一个出奇给力的包系统。目前为止，你读到的所有功能，都是通过标准的Meteor包来实现的。

Meteor包很聪明：包本身就是JavaScript程序。它们可以注入代码到客户端或服务器端，或者勾入一个新的函数到 hundler 里，所它们能以各种方式来扩展Meteor的环境。例如说：

* [coffeescript](http://docs.meteor.com/#coffeescript)包扩展了 bundler，自动编译你的目录树下的.coffee文件。加入之后，你就可以用CoffeeScript来替代javaScript了。
* jQuery 和 Backbone 包是用Meteor来打包客户端库的例子。你自己复制JavaScript文件到你的目录下也可以实现同样的效果，但是用智能包更快。
* underscore 包扩展了服务器端和客户端的环境。很多Meteor的核心特性，包括Minimongo，Session对象，reactive Handlebars 模板，都作为内置包自动包含在每个Meteor应用程序里了。

用 meteor list 命令和可以看到所有能用的包的清单。 用 meteor add 将包添加道你的项目中，用 meteor remove 移除。

[查看已有的包](http://docs.meteor.com/#packages)。

## 部署

Meteor 是一个完整的服务器端应用程序。我们包含了所有将应用程序部署到网络上所需要的东西：你只好提供JavaScript,HTML,和CSS。

### 在 Meteor 的设施（云）上运行

部署应用程序最简单的办法就是使用命令 meteor deploy 。我们提供这个服务的原因，是我们自己经常需要：有一种简单的方式，让我们可以在周末实现一个创意，并开放给全世界访问，而没有任何东西能阻挡创造力。

	meteor deploy myapp.meteor.com

你的应用程序就在 myapp.meteor.com 上了。如果这是第一次在这个域名上部署，meteor会为你的应用程序创建一个空数据库。如果是部署一些更新，Meteor会保留数据库，只是更新代码。

你也可以使用自己的域名。只要给你想要用的域名设置一个CNAME记录到 origin.meteor.com ，就可以了。

	meteor deploy www.myapp.com

我们是免费提供这个服务器的，你可以用它来试用Meteor。这对于建立内部beta，demo这些东西是很有帮助的。

### 在你自己的设施上运行

你也可以在你自己的设施上运行，例如向 Heroku 提供的服务。

执行即可：
	
	$ meteor bundle myapp.tgz

这个命令会以tarball的形式生成一个包含Node.js的应用程序。执行这个应用程序，需要安装Node.js 0.8和MongoDB服务器。你可以用node来调用这个应用程序，指定HTTP端口，和MongoDB的连接点。如果你没有MongoDB服务器，我们推荐我们兄弟单位的提供服务： [MongoHQ](http://mongohq.com/)。

	$ PORT=3000 MONGO_URL=mongodb://localhost:27017/myapp node bundle/main.js
	
有些包需要另外一些环境变量（例如，email包需要 MAIL_URL 环境变量）。

> 目前为止，bundle只能在创建它的家平台上运行。到其他平台上，你需要将 bundle 里包含的原生包（native package）重建一下。确保 npm 可用，然后运行:
> 
	$ cd bundle/server/node_modules
	rm -r fibers
	npm install fibers@0.6.9
>


