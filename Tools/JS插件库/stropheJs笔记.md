
##常量
#####连接状态常量
	连接状态常量使用的连接处理程序回调。
	Status.ERROR	发生错误  0
	Status.CONNECTING	正在建立连接  1
	Status.CONNFAIL	连接请求失败  2
	Status.AUTHENTICATING	连接进行身份验证  3
	Status.AUTHFAIL	身份验证请求失败  4
	Status.CONNECTED	连接成功  5
	Status.DISCONNECTED	连接已终止  6
	Status.DISCONNECTING	当前连接被终止  7
	Status.ATTACHED	附加的连接The connection has been attached  8
	Status.CONNTIMEOUT	连接超时
#####日志级别常量
	日志级别指标。
	LogLevel.DEBUG	调试输出
	LogLevel.INFO	输出信息
	LogLevel.WARN	警告
	LogLevel.ERROR	错误
	LogLevel.FATAL	致命错误
#Strophe.Connection
###Strophe.Connection

	Strophe.Connection = function (	service,options	)
	创建和初始化一个Strophe.Connection对象
	
	这个连接的传输协议将自动选择基于给定的服务参数。url从“ws://”或“wss://”用WebSockets,
	url从“http://”开始,“https://”或没有一个协议将使用BOSH。
	
	使Strophe连接到当前的主机可以省去协议和主机部分和传递路径 
	e.g: var conn = new Strophe.Connection("/http-bind/");

#####Websocket and BOSH 基本选项
	
	cookie选项允许您通过cookie添加到文档中，这些cookie将被包含在BOSH XMLHttpRequest或websocket连接。
	
#####传入的值必须是一个cookie名称和字符串值的视图
	{ “myCookie”: { “value”: “1234”, “domain”: “.example.org”, “path”: “/”, “expires”: expirationDate } }
	
	注意,cookie不能以这种方式设置为其他领域(如跨域)。这些cookie需要设置在这些领域,例如他们可以设置服务器端通过一个XHR调用该域问它设置任何必要的cookie。
#####WebSocket选项
	如果你想用WebSocket连接到当前主机你可以告诉strophe使用WebSockets通过“协议”属性的参数可选的选项。
	“ws”为普通的WebSocket和“wss是安全WebSocket都是有效值。所以连接到“wss:/ / CURRENT_HOSTNAME / xmpp-websocket” 
	eg： var conn = new Strophe.Connection("/xmpp-websocket/", {protocol: "wss"});

	注意,相对url不是从“/”还包括当前站点的路径。
	也因为降级安全是不允许浏览器,当使用相对urlBOSH和WebSocket连接都将使用他们的网站安全的变体如果当前连接也是安全的(https)。

#####BOSH 选项
	通过添加“sync(同步)”选项,您可以控制是否请求将同步。默认行为是异步的。如果你想要请求同步,使“sync(同步)”设置为true:
	
#####您还可以切换这对已建立的连接 
	 conn.options.sync = true;

	“customHeaders”选项可用于提供自定义HTTP报头包含在xmlhttprequest。
	“keepalive”选项可以用来指示诗节保持当前波什会话在网页加载等干扰。
	它将通过缓存sessionStorage会话令牌,当“恢复”是称它将检查是否有缓存的令牌,它可以恢复现有的会话。
	“withCredentials”选项应该接受一个布尔值,用于指示是否饼干应该被包括在ajax请求(默认情况下他们没有)。将这个值设置为true如果你连接到波什服务和出于某种原因需要发送cookie。为了使这个工作跨域,服务器还必须使凭证通过设置Access-Control-Allow-Credentials响应头“true”。对于大多数可变性然而这个设置应该是假的(这是默认的)。此外,当使用Access-Control-Allow-Credentials Access-Control-Allow-Origin头不能设置为通配符“*”,而是必须限制在实际领域。	

#####参数
	(String) service	BOSH 或者 WebSocket 服务的 URL.
	(Object) options	哈希的配置选项


##reset
	reset: function ()  重置连接，这个函数在重新连接之前，连接断开之后被调用
##pause
	pause: function ()  暂停连接管理 
	这将防止strophe再请求发送到服务器。当很多send()被很快调用时，这对暂停BOSH-Connections非常有用,这导致strophe在单个请求发送数据,节省许多请求。

##resume
	resume: function ()  恢复请求 在pause()之后被调用

##getUniqueId
	getUniqueId: function(	suffix	)
	生成一个惟一的ID用于<iq/ >元素。
	所有<iq/ >节都必须拥有惟一的id属性。这个函数可以使创建这些。每个连接实例有一个计数器,从0开始,和这个计数器的值加上一个冒号后面跟着后缀变成了唯一的id。如果没有提供的后缀,计数器是用作惟一的id。
	后缀是用来简化调试时读取流数据,和他们的使用建议。每个新连接的计数器重置为0出于同样的原因。连接到相同的服务器身份验证方式,所有的id应该是一样的,这使得它容易看到变化。这是有用的自动化测试。

#####参数
	(String) suffix	 一个可选的后缀添加到id

#####返回值 	
	一个独一无二的字符串用于id属性。
	

##connect
	connect: function (	jid,
						pass,
						callback,
						wait,
						hold,
						route,
						authcid	)
	
	开始连接过程。
	作为连接过程,用户提供的回调将多次触发状态更新。回调需要两个参数,状态代码和错误条件。
	值的状态代码将一个诗节。状态常量。错误条件将在RFC 3920中定义的一个条件或条件“strophe-parsererror”。
	参wait,hold和path都是可选的,只有与BOSH相关连接。请参阅XEP 124可选参数的更详细的解释。
#####Parameters
	(String) jid	用户的JID。这可能是一个光秃秃的JID或一个完整的JID。如果一个节点是不提供,将试图SASL匿名身份验证。
	(String) pass	用户的密码
	(Function) callback	连接的回调函数
	(Integer) wait	可选HTTPBIND wait值。这是一次服务器将返回一个空的结果之前等待请求。建议默认设置为60秒。
	(Integer) hold	可选HTTPBIND hold。这是服务器的连接数量将举行一次。这几乎总是设置为1(默认)。
	(String) route	可选 route 值.
	(String) authcid	可选的替代认证身份，如果打算冒充另一个用户(username)。

##functions
>1.addHandler
>
	  addHandler: function (	handler,
							ns,
							name,
							type,
							id,
							from,
							options	)

>这个函数添加一个节处理程序来连接。处理程序回调将呼吁任何节相匹配的参数。注意,如果多个参数提供,他们必须处理程序被调用的所有比赛。
>处理程序将收到触发它的节作为其参数。处理程序应该返回true,如果再次被调用,返回false将移除它返回后的处理程序。
	方便,ns参数适用于顶级元素以及它的任何直接的孩子。这主要是使匹配/iq/查询元素容易。
	选项参数包含处理程序匹配的标志,影响如何匹配是如何决定的。目前唯一的标志是matchBare(boolean)。matchBare为真时,从参数和属性节将匹配空的jid代替完整的jid。使用这个,通过{ matchBare:true}作为选项值。matchBare的默认值是false。
	返回值应该被保存,如果您希望删除处理程序deleteHandler()。
	
>参数：
>
>>(Function) handler	用户回调函数.
>
>>(String) ns	匹配的命名空间.
>
>>(String) name	匹配的节名称.
>
>>(String) type	The stanza type attribute to match.
>
>>(String) id	The stanza id attribute to match.

>>(String) from	The stanza from attribute to match.

>>(String) options	The handler options

>返回值:
	
>>A reference to the handler that can be used to remove it.

Functions
	
>.attach	连接到一个已经创建并验证BOSH会话
>
>.restore	试图恢复缓存BOSH会话.
>
>.xmlInput	用户overrideable函数接收XML数据发送到连接.

>.xmlOutput	用户overrideable函数接收XML数据发送到连接.

>.rawInput	User overrideable function that receives raw data coming into the connection.

>.rawOutput	User overrideable function that receives raw data sent to the connection.

>.nextValidRid	User overrideable function that receives the new valid rid.

>.send	发送一个节Send a stanza.

>.flush	立即发送任何悬而未决的输出数据Immediately send any pending outgoing data.

>.sendIQ	Helper函数发送iq节Helper function to send IQ stanzas.

>.addTimedHandler	定时处理程序添加到连接Add a timed handler to the connection.

>.deleteTimedHandler	删除一个连接的定时处理程序Delete a timed handler for a connection.

>.addHandler	为连接添加一个节处理程序Add a stanza handler for the connection.

>.deleteHandler	删除一个节处理函数Delete a stanza handler for a connection.

>.disconnect	断开连接Start the graceful disconnection process.

>.authenticate	设置身份验证Set up authentication

