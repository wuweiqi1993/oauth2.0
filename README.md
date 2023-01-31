# oauth2.0
oauth2.0官方文档翻译

官方文档地址:https://oauth.net/2/
本文是对oauth2.0框架翻译，框架文档地址：https://tools.ietf.org/html/rfc6749


# oauth2.0授权框架
## 简介
Ouath2.0授权框架支持第三方应用通过协调资源所属者与HTTP服务之间的交互（交流）来代表资源所属者去获取一个HTTP服务的受限资源。或者允许第三方以自己的名义来访问受限资源。这份框架说明替换并淘汰了在RFC 5849内描述的Oauth1.0协议（过时）。

## 文档信息（Status of This Memo）
这是一个国际标准的跟踪文档。
这篇文档由国际互联网工程任务组（IETF）产物。它代表了全体国际互联网工程任务组成员的产出。它已经接受大众检阅并由互联网工程指导小组（IESG）认证出版。更多相关的国际标准相关的信息可以参考阅读：RFC 5741的第二章节。
当前文章的更新状态以及所有的勘误信息和如何提供信息反馈都可以查看 http://www.rfc-editor.org/info/rfc6749 了解。

## 版本信息（此翻译只做辅助介绍，不具备响应效益，具体效益请自行阅读原文档）
Copyright (c) 2012 IETF Trust and the persons identified as the document authors.  All rights reserved。
（版权所属IETF Trust 2012 和所有认证的所有文档作者，保留所有权限。）
这篇文档从属于BCP 78。此文档以及所有与IETF文档相关的IETF Trust的合法规定在此文档发布日即时生效。请尊重并仔细阅读这些文档，他们介绍了使用oauth2.0的相应权益和受限条件。从此文档提取的代码组件必须包含IETF Trust的合法规定的第四章中描述的简化的BSD许可。如果没有相应的保证书这部分代码不允许使用（这句不确定，原文写的是provided，自己看原文）

# 目录
1.介绍  
&#160;&#160;&#160;&#160;1.1 角色  
&#160;&#160;&#160;&#160;1.2 协议流程  
&#160;&#160;&#160;&#160;1.3 认证授权  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;1.3.1 授权码授权  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;1.3.2 简单授权  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;1.3.3 资源所属者密码认证  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;1.3.4 客户端认证  
&#160;&#160;&#160;&#160;1.4 access token（访问令牌）  
&#160;&#160;&#160;&#160;1.5 refresh token（刷新令牌）  
&#160;&#160;&#160;&#160;1.6 安全传输层协议（TLS）版本  
&#160;&#160;&#160;&#160;1.7 HTTP 重定向  
&#160;&#160;&#160;&#160;1.8 复用性（*）  
&#160;&#160;&#160;&#160;1.9 标记约定（*）  
2.客户端注册  
&#160;&#160;&#160;&#160;2.1 客户端类型  
&#160;&#160;&#160;&#160;2.2 客户端标识  
&#160;&#160;&#160;&#160;2.3 客户端授权  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;2.3.1 客户端密码  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;2.3.2 其他授权认证方式  
&#160;&#160;&#160;&#160;2.4 未注册的客户端  
3.协议终端  
&#160;&#160;&#160;&#160;3.1 授权终端  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;3.1.1 响应类型（response type）  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;3.1.2 重定向端点  
&#160;&#160;&#160;&#160;3.2 token终端  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;3.2.1 客户端授权  
&#160;&#160;&#160;&#160;3.3 访问令牌范围  
4.获取授权  
&#160;&#160;&#160;&#160;4.1 授权码授权  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.1.1 授权请求  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.1.2 授权响应  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.1.3 访问令牌请求  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.1.4 访问令牌响应  
&#160;&#160;&#160;&#160;4.2 简单授权  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.2.1 授权请求  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.2.2 访问令牌响应  
&#160;&#160;&#160;&#160;4.3 资源所属者密码授权  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.3.1 授权请求和响应  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.3.2 访问令牌请求  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.3.3 访问令牌响应  
&#160;&#160;&#160;&#160;4.4 客户端授权  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.4.1 授权请求和响应  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.4.2 访问令牌请求  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;4.4.3 访问令牌响应  
&#160;&#160;&#160;&#160;4.5 扩展授权（其他授权）  
5.发放访问令牌  
&#160;&#160;&#160;&#160;5.1 成功响应  
&#160;&#160;&#160;&#160;5.2 错误响应  
6.刷新访问令牌  
7.访问受限资源  
&#160;&#160;&#160;&#160;7.1 访问令牌类型  
&#160;&#160;&#160;&#160;7.2 错误响应  
8.扩展性  
&#160;&#160;&#160;&#160;8.1 定义访问令牌类型  
&#160;&#160;&#160;&#160;8.2 定义端点参数  
&#160;&#160;&#160;&#160;8.3 定义授权类型  
&#160;&#160;&#160;&#160;8.4 定义授权端返回类型  
&#160;&#160;&#160;&#160;8.5 定义附加错误码  
9.本地应用  
10.安全性考虑  
&#160;&#160;&#160;&#160;10.1 客户端授权  
&#160;&#160;&#160;&#160;10.2 客户端模拟  
&#160;&#160;&#160;&#160;10.3 访问令牌  
&#160;&#160;&#160;&#160;10.4 刷新令牌  
&#160;&#160;&#160;&#160;10.5 授权码  
&#160;&#160;&#160;&#160;10.6 授权码重定向地址处理  
&#160;&#160;&#160;&#160;10.7 资源所属人密码证书  
&#160;&#160;&#160;&#160;10.8 请求的保密性  
&#160;&#160;&#160;&#160;10.9 确保客户端真实性  
&#160;&#160;&#160;&#160;10.10 证书猜测攻击  
&#160;&#160;&#160;&#160;10.11 钓鱼攻击  
&#160;&#160;&#160;&#160;10.12 跨站伪造请求  
&#160;&#160;&#160;&#160;10.13 点击劫持  
&#160;&#160;&#160;&#160;10.14 代码注入和输入验证  
&#160;&#160;&#160;&#160;10.15 开放重定向器  
&#160;&#160;&#160;&#160;10.16 简单模式中模拟资源从属者滥用访问令牌  
11.互联网驱动器符权限考虑  
&#160;&#160;&#160;&#160;11.1 Oauth访问令牌类型注册  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;11.1.1 注册模板  
&#160;&#160;&#160;&#160;11.2 Oauth参数注册  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;11.2.1 注册模板  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;11.2.2 初始化注册内容  
&#160;&#160;&#160;&#160;11.3 Oauth授权终端响应类型注册  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;11.3.1 注册模板  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;11.3.2 初始化注册内容  
&#160;&#160;&#160;&#160;11.4 Oauth扩展错误注册  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;11.4.1 注册模板  
12.参考文献  
&#160;&#160;&#160;&#160;12.1 规范性参考文献  
&#160;&#160;&#160;&#160;12.2 资料性参考文献
  
附录A. 语法格式描述规范语法  
&#160;&#160;&#160;&#160;A1.client_id 语法  
&#160;&#160;&#160;&#160;A2.client_secret 语法  
&#160;&#160;&#160;&#160;A3.response_type 语法  
&#160;&#160;&#160;&#160;A4.scope 语法  
&#160;&#160;&#160;&#160;A5.state 语法  
&#160;&#160;&#160;&#160;A6.redirect_uri 语法  
&#160;&#160;&#160;&#160;A7.error 语法  
&#160;&#160;&#160;&#160;A8.error_description 语法  
&#160;&#160;&#160;&#160;A9.error_uri 语法  
&#160;&#160;&#160;&#160;A10.grant_type 语法  
&#160;&#160;&#160;&#160;A11.code 语法  
&#160;&#160;&#160;&#160;A12.access_token 语法  
&#160;&#160;&#160;&#160;A13.token_type 语法  
&#160;&#160;&#160;&#160;A14.expires_in 语法  
&#160;&#160;&#160;&#160;A15.username 语法  
&#160;&#160;&#160;&#160;A16.password 语法  
&#160;&#160;&#160;&#160;A17.refresh_token 语法  
&#160;&#160;&#160;&#160;A18.端点参数 语法  
附录B. application/x-www-form-urlencoded类型的使用  
附录C. 致谢  

# 1.介绍
在传统的客户端-服务端认证模型中，客户端使用用户的证书通过服务器的认证来请求一个服务器上访问受限的资源（被保护的资源）。为了能够支持第三方拿到用户的受限资源，用户不得不和第三方共享认证的证书。这会带来一些问题和限制：
* 第三方应用为了能够实现长期访问必须要存储用户给予他们的认证证书。比较有代表性的操作，第三方必须明文存储用户的密码。
* 尽管存在密码的安全风险，服务端依然要支持密码认证模式。
* 第三方应用过度获取了用户的对所有受限资源的全部权限（个人看法：密码登录就等效于具备操作所有功能），而用户却无法对在第三方使用密码的期间限制第三方访问的资源，或者说做不到将第三方的访问权限限制在某些受限资源上。
* 用户无法做到对单个第三方收回授权的同时又不收回其他第三方的授权。如果非要保留其他第三方的授权就只有在改密码以后去其他每个第三方平台更新一遍最新的密码。
* 任何一家第三方存储的密码泄露都意味着终端（服务提供方）用户的密码被泄露以及所有使用此密码保护的数据（包含原服务方和使用此密码授权的其他第三方平台的数据）都发生了泄露。（个人观点和官方文档说的是两个维度的问题：同时密码泄露意味着相同账号在其他平台上的安全也存在风险，举个例子，我们习惯性的在不同平台上注册账号的时候会使用相同的密码，比如在网易邮箱和淘宝使用了相同的邮箱账号注册使用相同的密码为了方便好记。那么网易泄露密码，你的支付宝账号也会存在被盗的风险，因此当前很多企业的做法是在数据库内不会存在明文密码，基本上都是不可逆的加密算法）

Oauth通过引入一个认证层的概念来解决这些问题。在oauth框架内，当客户端想要访问那些被用户控制的和资源服务器持有的数据资源时，除数据本身外，他还会被授予一系列不同的认证证书。
客户端会获得一个访问令牌（一个指定了访问数据范围、有效时间、和访问限制参数的字符串）而不是直接使用用户认证的账号密码。在获得用户许可的前提下，认证服务器会授予第三方一个访问令牌，客户端使用这个令牌来访问资源服务器所持有的受限的用户资源。

例如，一个终端用户（资源拥有者）在不告知打印图片处理服务（客户端）自己的账号密码的前提下，可以将自己存在图片分享服务（资源服务）上的私人照片（受限资源）的访问权限授权给打印图片处理服务（客户端）。

这份框架说明是基于 HTTP ([RFC2616])https://www.rfc-editor.org/rfc/rfc6749的使用场景设计的，除了HTTP协议外，oauth协议不适用与其他协议。

oauth1.0协议文档([RFC5849]）https://www.rfc-editor.org/rfc/rfc5849 是一个小型社区共同努力讨论解决方案而产出的结论整理后的一份文档。而这份国际标准追踪文档是基于oauth1.0的历史经验做了升级，同时也对从更大的IETF社区收集到的其他用例和扩展性需求做了支持。oauth2.0协议不向后兼容之前的oauth1.0协议。这两个版本可以同时共存在网络上，具体实现的时候可以选择同时支持两个版本的协议。但是本文档会更建议开发者新的功能或者应用以oauth2.0的协议来实现，而对于已经用oauth1.0实现的业务保持现状。oauth2.0协议和oauth1.0协议在实现上有很大的差异，只有很少的部分是复用了一样的逻辑。熟悉oauth1.0的开发者或者之前实现oauth1.0协议的用户建议不要带着对oauth1.0的结构和实现细节的经验来阅读该2.0文档，

