# 参考资料

- 来源：https://apisix.apache.org/zh/blog/2022/01/20/upyun-public-gateway-usecase/
- 获取日期：2026-11-27
- 说明：以下为页面可读文本快照，可能包含导航或页脚。

---

CDN 业务场景下，又拍云的公网网关场景实践 | Apache APISIX

🤔 Introducing APISIX AI Gateway – Built for LLMs and AI workloads. Learn More

Apache APISIX®

文档 ▾

Apache APISIX®Apache APISIX® Ingress ControllerApache APISIX® Helm ChartsApache APISIX® DockerApache APISIX® Java Plugin RunnerApache APISIX® Go Plugin RunnerApache APISIX® Python Plugin RunnerGeneral

学习中心对比AI Gateway博客 生态 ▾

集成Cookbook插件中心

下载团队 GitHub EN

☰

文档

文档 Apache APISIX®Apache APISIX® Ingress ControllerApache APISIX® Helm ChartsApache APISIX® DockerApache APISIX® Java Plugin RunnerApache APISIX® Go Plugin RunnerApache APISIX® Python Plugin RunnerGeneral

学习中心对比AI Gateway博客 生态

集成Cookbook插件中心

下载团队 GitHub English

标签

Case Studies

← 所有文章

CDN 业务场景下，又拍云的公网网关场景实践

2022年1月20日 · 苏钰 · 约 4 分钟

Case Studies

本页目录

业务场景概览

场景用例展示

插件场景：登录内部平台

插件场景：联动飞书任务提醒

CDN 业务下，影响网关选型的关键因素

稳定性

开源与扩展性

总结

在之前的分享中，我们为大家带来了又拍云在 Ingress 层面的应用。而这次我们会在公网网关场景下，给大家介绍目前又拍云的应用实例，希望能在云存储行业领域，给大家带来一些实用的 Apache APISIX 场景分享。

业务场景概览

又拍云是目前在业务场景中已部署 Apache APISIX 的用户之一。又拍云以场景化 CDN 为核心业务，为客户提供云存储、云处理、云安全、流量营销等服务，帮助用户实现内容分发加速、产品研发加速、业务成长加速。

目前 Apache APISIX 已在又拍云内部用于「公网网关」和「Ingress 网关」两种业务场景进行使用与部署。社区内之前已经发布了一篇关于又拍云使用 Apache APISIX Ingress 的相关用例分享，而这次我们会在「公网网关」场景下，给大家介绍一些目前又拍云的应用实例，希望能在云存储行业领域，给大家带来一些实用的 Apache APISIX 场景分享。

场景用例展示

目前又拍云内部的网关架构如下图所示。外部流量通过 Apache APISIX 传输到内部，之后再将流量通过 Apache APISIX 传输到 APISIX Ingress，最后到达后端服务进行后续业务处理。

而作为外部流量的入口第一关，公网网关则要担起重任，将外部进入内部数据中心的流量全部控制起来。

又拍云原本的公网网关部署是 Kong，现在已将 Kong 慢慢迁移到 Apache APISIX 中。目前已将之前 Kong 处理流量的 30% 迁移到了 APISIX 中，迁移工作还在逐步进行。

之所以将 Kong 切换为 APISIX 是因为，Kong 架构中需要连接 PostgreSQL 数据库。在添加路由或重启路由的情况下，会出现几千上万个连接链到 Postgre。而此时一旦进行更新或重启，即便在前侧加了代理也无法控制这套连接，尤其是路由越多这种情况越麻烦。

基于此，Apache APISIX 开始逐步接手又拍云的公网网关功能，目前主要应用于以下业务场景服务：

通过 APISIX 网关处理 CDN 边缘节点的 API 访问，主要用于后续流量管理

通过 APISIX 处理官网静态页面和技术支持相关的流量

由于目前还未全面迁移完毕，目前 Apache APISIX 仅对接了内部 3 个数据中心，大概 20 台机器。在流量处理方面，Apache APISIX 很出色地完成了备份与流量承载方面的需求。

而作为又拍云的公网网关，Apache APISIX 除了要在入口进行统一的流量管理，还在数据层面对用户登录等场景进行了一些不同功能的使用。

插件场景：登录内部平台

目前在又拍云公司内部，员工登录内部平台需要通过邮箱和飞书等相关认证进行接入。

具体代码示例如下：

"openid-connect": {

"access_token_in_authorization_header": false,

"bearer_only": false,

"client_id": "upyun-oauth",

"client_secret": "xxxx",

"disable": false,

"discovery": "https://xxxx.upyun.com/oauth2/.well-known/openid-configuration",

"introspection_endpoint_auth_method": "client_secret_basic",

"logout_path": "/logout",

"realm": "apisix",

"redirect_uri": "/upyun-oauth",

"scope": "openid email profile offline_access",

"session_contents": {

"access_token": true,

"user": true

},

"set_access_token_header": false,

"set_id_token_header": false,

"set_userinfo_header": true,

"timeout": 15

},

利用 Apache APISIX 的 openid-connect 插件，可以非常方便地对接上述平台进行员工身份认证，进而统一登录到又拍云的管理平台。

插件场景：联动飞书任务提醒

而在更细节的应用场景下，又拍云内部也通过 openid-connect 插件配合 serverless-post-function 插件完成了与飞书应用的联动。

通过以上插件的配合，可以将相关用户信息（username、email 或者用户在飞书有应用内的唯一标识）等传输给公网网关后的服务。网关获取到相关标识信息后转发给服务端，完成飞书提醒或者@等联动功能。

return function(config, ctx)

local core = require('apisix.core');

local user = ngx.req.get_headers()['X-Userinfo'];

....

if name then

if not ctx.consumer_name then

ctx.consumer_name = name;

end;

core.request.set_header(ctx, 'X-Forwarded-Username', name)

end;

if user_id then

core.request.set_header(ctx, 'X-Forwarded-UserID', user_id)

end;

end

当然，在此过程中也可以利用 consumer-restriction 插件对 Consumer 进行一些权限限制。

CDN 业务下，影响网关选型的关键因素

作为 Apache APISIX 的企业用户，又拍云这种以场景化 CDN 进行云存储与多线处理的业务场景，在网关产品的选型上也分享了他们的一些标准。

稳定性

作为云上业务的对外分发，首先看重的必然是稳定性。一个具有稳定性的网关业务产品，除了能保证对外用户侧的使用体验外，还能保证公司内部业务部署的运维成本得到有效可控。尤其是对于一些公司规模不大或者是运维团队人员不多的情况下，稳定性是其首要考虑的关键因素。

开源与扩展性

对于又拍云技术团队而言，开源与否也成为网关选型的重要因素。得益于社区的活跃属性，通常在出现 Bug 的第一时间反馈给社区就会很快得到回应。不同于闭源软件，如果出现 Bug，通过官方渠道反馈后可能还不知道要等多久才能收到官方推送带修复 Bug 的新版本。尤其是对于又拍云这种云上业务处理情况，响应速度直接关系着公司业务。

同时开源的高扩展性也是给开发人员带来了更方便的适配与集成。像是 Apache APISIX 的多语言扩展，方便了又拍云在业务扩展场景下，基于 APISIX 去开发更多适用于他们业务场景的功能。在减轻开发成本的同时，也为后续的功能迭代和维护提高了便利性。

总结

又拍云作为 CDN 云上处理业务方，已经逐渐在公网网关和 Ingress 层面深度使用 Apache APISIX。希望通过这一系列的用户案例分享，能为大家在网关选型的过程中提供一些思路与方向。

同时，Apache APISIX 也欢迎正在使用 APISIX 的企业用户积极参与分享相关用户案例实践，也欢迎随时在 GitHub Discussions 中发起讨论，或通过邮件列表进行交流。

← 所有文章

← 较新文章 多协议接入框架 xRPC 细节前瞻 较早文章 → 社区双周报（1.1-1.16）

本页目录

业务场景概览

场景用例展示

插件场景：登录内部平台

插件场景：联动飞书任务提醒

CDN 业务下，影响网关选型的关键因素

稳定性

开源与扩展性

总结

相关文章

APISIX 在 360 统一七层负载均衡的落地实践

360 使用 Apache APISIX 统一七层负载均衡，通过其 Admin API 和 etcd 热同步机制，实现了 VPC、云原生能力以及精细化路由的一体化升级。

2025年9月3日 · 360 智汇云

MCP 智能化管理实践：一句话搞定 APISIX 网关

作者：林志煌，API7.ai 前端开发、产品经理。本文整理自 2025 年 4 月 12 日林志煌在 APISIX 深圳 Meetup 的演讲。

2025年6月4日 · 林志煌

从云原生到 AI 原生：Apache APISIX 的 AI 网关架构设计与智能实践

了解 Apache APISIX 如何从云原生 API 网关扩展到 AI 网关，覆盖 LLM 流量治理、安全、观测和成本控制。

2025年5月29日 · 王院生

ASF

Foundation

License

Events

Security

Sponsorship

Thanks

Community

GitHub

Slack

Twitter

YouTube

Resources

Blog

Learning Center

Comparisons

Integrations

Cookbooks

Events

Case Studies

More

Plugin Hub

Downloads

Team

Contribute

Copyright © 2019-2026 The Apache Software Foundation. Apache APISIX, APISIX®, Apache, the Apache feather logo, and the Apache APISIX project logo are either registered trademarks or trademarks of the Apache Software Foundation.
