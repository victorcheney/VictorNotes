
#安装和配置SSL支持
##什么是SSL(Secure Sockets Layer 安全套接层)技术


##理解数字证书
 >注意:应用程序服务器的数字证书已经生成并可以在目录中找到<J2EE_HOME>/domains/domain1/config/。这些数字证书签名,目的是在开发环境中使用,不用于生产目的。用于生产目的,生成自己的证书和CA签署的。
 >
 >要使用SSL，应用服务器必须为每个外部接口或IP地址，接受安全连接的相关证书。这种设计背后的理论是，一个服务器应该提供某种合理的保证，它的主人是你认为谁是这样，特别是在接收任何敏感信息之前。这可能是想出一个证书作为“数码驾驶执照”Internet地址有用的。它规定与公司网站关联，并提供有关网站所有者或管理员的一些基本联系信息。
 >
 >数字证书加密由其所有者签署，是很难为别人伪造。对于涉足电子商务或任何其他商业交易中身份验证的重要场所，证书可以从一个众所周知的证书颁发机构（CA）如VeriSign或Thawte购买。
 
 >有时认证是不是一个真正的关注 - 例如，管理员可能只是想确保所发送和接收的数据由服务器是私人的，不能被任何人窃听连接上窃听。在这种情况下，可以节省获取CA证书的时间和费用，并简单地使用自签名的证书。

 >SSL使用公共密钥加密，这是基于密钥对。密钥对包含一个公钥和一个私钥。如果数据是用一个密钥加密，它只能与该对的另一密钥来解密。此属性是建立在交易中的信任和隐私的根本。例如，使用SSL，服务器计算值和加密使用其私钥的值。加密的值被称为数字签名。客户端解密使用服务器的公共密钥的加密值和比较值到它自己的计算值。如果两个值匹配，则客户端可以相信，签名是可信的，因为只有私钥可能已被用来生产这样的签名。
 >
 >数字证书的HTTPS协议用于Web客户端进行身份验证。除非数字证书已安装大多数Web服务器的HTTPS服务将无法运行。使用概述后设置，可以通过Web服务器用来启用SSL数字证书的过程。
 
 >可用于建立数字证书的工具是keytool，它是一个密钥和证书管理实用程序附带的J2SE SDK。它使用户能够管理他们自己的公共/私有密钥对和相关联的证书中自我认证（用户认证他或她自己对其他用户或服务）或数据完整性和认证服务，使用数字签名的使用。它也允许用户缓存的公共密钥（以证书的形式）与其通信的另一方。为了更好地理解密钥工具和公钥密码，请阅读以下网址keytool文档：
 >[http://java.sun.com/j2se/1.5.0/docs/tooldocs/solaris/key-tool.html](http://java.sun.com/j2se/1.5.0/docs/tooldocs/solaris/key-tool.html)
 
>###**创建服务器证书**
>>服务器证书已为Application Server创建。该证书可以在<J2EE_HOME>/domain/domain1/ config /目录中找到。服务器证书在keystore.jks。该cacerts.jks文件包含所有受信任的证书，包括客户端证书。

>>如果有必要，可以使用密钥工具来生成证书。该密钥工具存储在文件中的密钥和证书称为密钥库，用于识别客户端或服务器证书的存储库。通常情况下，密钥库包含一个客户端或者一个服务器的身份。缺省的密钥仓库实现密钥仓库实现为一个文件。它通过使用密码保护私钥。

>>该密钥存储在您从中运行密钥工具的目录中。这可以是应用程序所在的目录，或者它可以是常见的许多应用中的一个目录。如果没有指定密钥库文件名，在用户的主目录中创建的密钥库。

>>**创建服务器端证书的步骤:**
>
>>1. 创建密钥库。
>
>>2. 从导出密钥库证书。
>
>>3. 注册证书。
>
>>4. 将证书导入信任店( trust-store)：用于验证证书的证书的存储库。信托商店通常包含多个证书。与JAX-RPC客户端证书身份验证通过HTTP / SSL：使用信任商店基于SSL的相互认证的例子为例进行讨论。

>>运行密钥工具来生成服务器密钥库，我们将其命名为keystore.jks。此步骤使用别名服务器别名来生成新的公钥/私钥对，敷公钥到里面keystore.jks自签名的证书。密钥对使用类型RSA的算法产生的，具有的changeit的默认密码。有关密钥工具选项的详细信息，请参阅[http://java.sun.com/j2se/1.5.0/docs/tooldocs/solaris/keytool.html]()它的在线帮助。

>>注：RSA是由RSA Data Security开发的公钥加密技术，Inc.的缩写代表的Rivest，Shamir和阿德尔曼，该技术的发明者。

>>在目录中创建密钥库，用下面的参数运行密钥工具
>
>>1. 生成服务器证书

>>          <JAVA_HOME>\bin\keytool -genkey -alias server-alias -keyalg RSA -keypass changeit -storepass changeit -keystore keystore.jks

>>       按Enter键时，密钥工具会提示您输入服务器名称，组织单位，组织，地方，州和国家代码。请注意，您必须在响应密钥工具的第一个提示，其中要求姓和名输入服务器名称。出于测试目的，这可以是localhost。在密钥库中指定的主机必须运行示例应用程序时，在< INSTALL >/j2eetutorial14/examples/common/build.properties指定的主机变量标识的主机相匹配。

>>2. 在导出的keystore.jks生成服务器证书导入文件server.cer

>>          <JAVA_HOME>\bin\keytool -export -alias server-alias -storepass changeit -file server.cer -keystore keystore.jks

>>3. 如果你想有一个由CA签署的证书，读签名数字证书([Signing Digital Certificates]() )的详细信息

>>4. 要创建信任存储文件cacerts.jks并将从创建密钥存储和服务器证书的目录添加服务器证书的信任店面，运行密钥工具。使用下面的参数：

>>         <JAVA_HOME>\bin\keytool -import -v -trustcacerts
>>         -alias server-alias -file server.cer 
>>         -keystore cacerts.jks -keypass changeit 
>>         -storepass changeit

>>       证书上的信息，如该图所示

>>         <INSTALL>/j2eetutorial14/examples/gs 60% keytool -import 
>>         -v -trustcacerts -alias server-alias -file server.cer 
>>         -keystore cacerts.jks -keypass changeit -storepass changeit
>>         Owner: CN=localhost, OU=Sun Micro, O=Docs, L=Santa Clara, ST=CA, C=US
>>         Issuer: CN=localhost, OU=Sun Micro, O=Docs, L=Santa Clara, ST=CA, C=US
>>         Serial number: 3e932169
>>         Valid from: Tue Apr 08
>>         Certificate fingerprints:
>>         MD5: 52:9F:49:68:ED:78:6F:39:87:F3:98:B3:6A:6B:0F:90 
>>         SHA1: EE:2E:2A:A6:9E:03:9A:3A:1C:17:4A:28:5E:97:20:78:3F:
>>         Trust this certificate? [no]:

>>5. 输入yes，然后按Enter键或Return键。该显示以下信息：

>>         Certificate was added to keystore
>>         [Saving cacerts.jks]


>###**签名数字证书**
>>创建了数字证书后，你会希望将其交由其所有者签署。数字证书已经被它的主人被加密签名后，很难让其他人伪造。对涉及电子商务或任何其他商业交易中身份验证的重要场所，证书可以从一个众所周知的证书颁发机构如VeriSign或Thawte购买。
>
>>正如前面提到的，如果认证是不是一个真正的关心，你可以节省获取CA证书的时间和费用，并简单地使用自签名的证书。


>###**使用与应用程序服务器不同的服务器证书**
>>按照创建服务器证书的步骤[ Creating a Server Certificate]()，创建自己的服务器证书，交由CA签名，证书导入keystore.jks。

>>确保当您创建的证书，遵循以下规则：
>
>> * 当您按下创建服务器证书，密钥工具会提示您输入您的名字和姓氏。在响应此提示，您必须输入您的服务器的名称。出于测试目的，这可以是localhost。
>> * 在密钥库中指定的服务器/主机必须在< INSTALL >/j2eetutorial14/examples/common/build.properties文件运行示例应用程序中指定的主机变量标识的主机相匹配。
>> * 在keystore.jks您的密钥/证书密码应与密钥库的密码，keystore.jks。这是一个错误。如果存在不匹配，则Java SDK无法读取证书，你会得到一个“篡改”的消息。
>> * 如果要替换现有的keystore.jks，必须更改密钥库的密码，默认密码（changeit），或者更改默认密码为密钥存储的密码：

>>要指定应用程序服务器应该使用新的密钥库进行身份验证和授权决策，就必须让他们认识到新的密钥库设置应用程序服务器的JVM选项。要使用不同的密钥库比提供了发展的宗旨，请按照下列步骤:
>> 
>>1. 启动应用程序服务器，如果您还没有这样做。在启动应用程序服务器的信息可以在启动和停止的应用程序服务器上找到。
>>2. 启动管理控制台。在启动管理控制台的信息可以在启动管理控制台中找到。
>>3. 选择应用程序服务器管理控制台树。
>>4. 选择JVM设置选项卡。
>>5. 选择JVM Options选项卡。
>>6. 使它们指向新的密钥库的位置和名称更改以下JVM选项。有当前设置如下所示：
>
>>          -Djavax.net.ssl.keyStore=${com.sun.aas.instanceRoot}/config/keystore.jks
>>          -Djavax.net.ssl.trustStore=${com.sun.aas.instanceRoot}/config/cacerts.jks
>>7. 如果您已经从它的默认值改为密钥库密码，则需要添加密码选项，以及：
>
>>          -Djavax.net.ssl.keyStorePassword=your_new_password

>>8. 注销管理控制台，然后重新启动应用程序服务器。

>###**创建用于相互验证客户端证书**

>>本节讨论设置客户端验证。当两个服务器端和客户端的认证被启用，它被称为相互的，或双向的，身份验证。在客户端认证，客户端需要提交由你选择接受证书颁发机构颁发的证书。从要创建客户端证书，运行密钥工具如这里列出的目录中。按Enter键时，密钥工具会提示您输入服务器名称，组织单位，组织，地方，州和国家代码。

>>注意：必须针对密钥工具的第一个提示，其中要求姓和名输入服务器名称。出于测试目的，这可以是localhost。在密钥库中指定的主机必须在< INSTALL >/j2eetutorial14/examples/common/build.properties文件中指定的主机变量标识的主机相匹配。如果这个例子是验证相互身份验证，您会收到一个运行时错误，指出HTTPS主机名错误，重新创建客户端证书，请务必使用运行示例时，您将使用相同的主机名。例如，如果你的机器名是duke，然后进入duke作为证书CN或当系统提示输入名字和姓氏。当访问应用程序，输入指向同一个位置的URL - 例如，[https://duke:8181/ mutualauth/hello]()。这是必要的，因为在SSL握手期间，服务器通过比较证书名称，并从它起源的主机名验证客户端证书。

>>要创建一个名为密钥存储客户keystore.jks包含客户端命名证书client.cer，请按照下列步骤操作：
>
>>1. 生成客户端证书

>>         <JAVA_HOME>\bin\keytool -genkey -alias client-alias -keyalg RSA -keypass changeit 
>>         -storepass changeit -keystore keystore.jks

>>2. 导出生成的客户端证书到文件client.cert

>>         <JAVA_HOME>\bin\keytool -export -alias client-alias 
>>         -storepass changeit -file client.cer -keystore keystore.jks

>>3. 该证书添加到信任存储文件<J2EE_HOME>/domains/domain1/config/cacerts.jks。从创建密钥库和客户端证书的目录中运行密钥工具。使用下面的参数：

>>         <JAVA_HOME>\bin\keytool -import -v -trustcacerts
>>         -alias client-alias -file client.cer 
>>         -keystore <J2EE_HOME>/domains/domain1/config/cacerts.jks 
>>         -keypass changeit -storepass changeit
>>keytool实用程序返回此消息：

>>         Owner: CN=J2EE Client, OU=Java Web Services, O=Sun, L=Santa Clara, ST=CA, C=US
>>         Issuer: CN=J2EE Client, OU=Java Web Services, O=Sun, L=Santa Clara, ST=CA, C=US
>>         Serial number: 3e39e66a
>>         Valid from: Thu Jan 30 18:58:50 PST 2003 until: Wed Apr 30 19:58:50 PDT 2003
>>         Certificate fingerprints:
>>         MD5: 5A:B0:4C:88:4E:F8:EF:E9:E5:8B:53:BD:D0:AA:8E:5A
>>         SHA1:90:00:36:5B:E0:A7:A2:BD:67:DB:EA:37:B9:61:3E:26:B3:89:46:32
>>         Trust this certificate? [no]: yes
>>         Certificate was added to keystore

>>对于使用相互身份验证的应用实例，请参阅示例：[ Example: Client-Certificate Authentication over HTTP/SSL with JAX-RPC]()。有关验证的相互验证正在运行的信息，请参阅[Verifying That Mutual Authentication Is Running]()。



>###**证书的杂项命令**
>>要检查是否包含一个别名服务器别名证书密钥库的内容，使用这个命令
>
>>      keytool -list -keystore keystore.jks -alias server-alias -v

>>要检查cacerts文件的内容，使用这个命令：

>>      keytool -list -keystore cacerts.jks

##使用SSL
>一个SSL连接器已预先配置为应用程序服务器。您不必进行任何配置。如果您正在使用其他应用服务器的工作，看到其设立的SSL连接器文档。

>###**验证SSL支持**

>>为了进行测试，并验证SSL支持已经正确安装，以连接到服务器部署描述符中定义的端口URL加载默认的介绍页面：

>>       https://localhost:8181/ 

>>此URL中的https表明浏览器应该使用SSL协议。在这个例子中的本地主机假定您正在运行在本地计算机上的例子作为开发过程的一部分。本例中的8181是其中的SSL连接器在使用SSL创建时指定的安全端口。如果您使用的是不同的服务器或端口，相应地修改此值。

>>第一次用户加载该应用程序，新的站点证书或安全警报对话框。选择Next通过一系列对话框移动，并选择完成，当你到达最后一个对话框。该证书将只显示第一次。当你接受证书，随后访问这个网站的假定你还是信任的内容。

>###**运行SSL的提示**

>>SSL协议被设计成稳固地尽可能高效。然而，加密和解密是从性能的观点出发计算昂贵的过程。它不是绝对必要跑在SSL整个Web应用程序，这是习惯的开发者决定哪个页面需要安全连接，哪些没有。可能需要安全连接的页面包括登录页面，个人信息页面，购物车结账，或者信用卡信息也可能会被传送的任何页面。一个应用程序中的任何页面都可以通过简单的前缀是https地址被要求通过安全套接字：而不是http：。这绝对需要一个安全连接的任何页面应该检查与页面请求相关联的协议类型，如果HTTPS采取适当的措施：未指定。

>>安全连接上使用基于域名的虚拟主机可能会出现问题。这是SSL协议本身的设计限制。 SSL握手，客户端浏览器接受服务器证书，被访问HTTP请求之前，必须进行。其结果是，不能认证之前确定含有该虚拟主机名的请求的信息，因此它不可能多个证书分配一个IP地址。如果一个IP地址上的所有虚拟主机都需要通过同一证书的验证，则添加多个虚拟主机不应该在服务器上正常的SSL操作。请注意，但是，大多数客户端浏览器将根据证书中列出的域名比较服务器的域名，如果有的话（这是主要适用于官方的CA签名的证书）。如果域名不匹配，这些浏览器会显示一个警告到客户端。在一般情况下，仅基于地址的虚拟主机通常与在生产环境中使用SSL。
>
>###**启用通过SSL相互认证**

>>本节讨论设置客户端验证。如前面提到的，当两个服务器端和客户端的认证被启用，它被称为相互的，或双向的，身份验证。在客户端认证，客户端需要提交由你选择接受证书颁发机构颁发的证书。如果规范它通过应用程序（通过客户端证书的认证要求），支票，当应用程序需要客户端身份验证执行。您必须在Web服务器配置文件中输入密钥库位置和密码启用SSL，如使用SSL的讨论。

>>这里有两种方法通过SSL启用相互身份验证：

>>* 首选：设置身份验证到客户端证书使用deploytool的方法。这通过修改给定的应用的部署描述符强制相互验证。通过以这种方式使客户端认证，客户端验证仅由安全约束控制的特定资源启用。这种方式设置客户机认证的示例中讨论[Client-Certificate Authentication over HTTP/SSL with JAX-RPC]()。

>>* 很少：在证书境界clientAuth属性设置为true。为此，请按照下列步骤操作：
>
>>      a.启动应用程序服务器，如果您还没有这样做。在启动应用程序服务器的信息可以在启动和停止的应用程序服务器[Starting and Stopping the Application Server]()上找到。
>>      b.启动管理控制台。在启动管理控制台的信息可以在启动管理控制台[Starting the Admin Console]()中找到。
>>      c.在管理控制台树中展开配置，展开安全，然后展开域，然后选择证书。 certificate领域用于通过SSL的HTTP传输的所有。
>>      d.选择添加到clientAuth的属性添加到服务器。在Name字段中输入clientAuth，并在值字段中输入true。
>>      e.单击保存，保存这些新的特性。
>>      f.注销管理控制台。

>>当客户端身份验证在这两种方式被激活，客户端身份验证将进行两次。

>>*检验相互认证正在运行*

>>>您可以验证相互认证是通过获取调试信息的工作。这应该在客户年底完成，而这个例子显示了如何在targets.xml通过一个系统属性，使targets.xml叉在其系统属性javax.net.debug一个客户端，它可以在一个文件中，例如加入 如
>
>>>     <INSTALL>/j2eetutorial14/examples/security/common/targets.xml.

>>>要启用SSL相互认证调试消息，通过系统属性javax.net.debug= SSL，握手，这将提供相互验证是否工作信息。下面的示例从修改运行mutualauth-client目标

>>>     <INSTALL>/j2eetutorial14/examples/security/common/targets.xml 
>文件通过以粗体显示添加sysproperty：

>>>     <target name="run-mutualauth-client" description="Runs a client with mutual authentication over SSL">
	    <java classname="${client.class}" fork="yes" >
	          <arg line="${key.store} ${key.store.password} 
	          ${trust.store} ${trust.store.password} 
	          ${endpoint.address}" />
	    <sysproperty key="javax.net.debug" value="ssl, 
	          handshake" />
	    <sysproperty key="javax.net.ssl.keyStore"
	          value="${key.store}" />
	    <sysproperty key="java.net.ssl.keyStorePassword"
	          value="${key.store.password}"/>
	    <classpath refid="run.classpath" />
	  </java>
	</target> 