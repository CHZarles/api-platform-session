# 参考资料

- 来源：https://apisix.apache.org/docs/apisix/admin-api/
- 获取日期：2026-11-27
- 说明：以下为页面可读文本快照，可能包含导航或页脚。

---

Admin API | Apache APISIX

🤔 Introducing APISIX AI Gateway – Built for LLMs and AI workloads. Learn More

Apache APISIX®

Docs ▾

Apache APISIX®Apache APISIX® Ingress ControllerApache APISIX® Helm ChartsApache APISIX® DockerApache APISIX® Java Plugin RunnerApache APISIX® Go Plugin RunnerApache APISIX® Python Plugin RunnerGeneral

Learning CenterComparisonsAI GatewayBlog Ecosystem ▾

IntegrationsCookbooksPlugin Hub

DownloadsTeam GitHub 中

☰

Docs

Docs Apache APISIX®Apache APISIX® Ingress ControllerApache APISIX® Helm ChartsApache APISIX® DockerApache APISIX® Java Plugin RunnerApache APISIX® Go Plugin RunnerApache APISIX® Python Plugin RunnerGeneral

Learning CenterComparisonsAI GatewayBlog Ecosystem

IntegrationsCookbooksPlugin Hub

DownloadsTeam GitHub 简体中文

Docs / Admin API 3.21.0

APISIX 3.21.0

3.18

3.17

3.16

3.15

3.14

3.13

3.12

3.11

3.10

Next (unreleased)

Download OpenAPI

Loading API reference…

APISIX 3.21.0

APISIX 3.21.0

3.18

3.17

3.16

3.15

3.14

3.13

3.12

3.11

3.10

Next (unreleased)

Getting Started

Getting Started with Apache APISIX

Configure Routes

Load Balancing

Key Authentication

Rate Limiting

Installation

Architecture

Tutorials

Expose API

Proxy LocalAI APIs with Apache APISIX

Protect API

Observability

Observe APIs

Health Check

Monitor API Health Check with Prometheus

Manage API Consumers

Cache API responses

Add multiple API versions

Configure mTLS for client to APISIX

WebSocket Authentication

Set Up SSO with Keycloak (OIDC)

Terminology

API Gateway

Consumer

Consumer Group

Credential

Global Rules

Plugin

Plugin Config

Plugin Metadata

Route

Router

Script

Service

Upstream

Secret

Plugins

AI

ai-proxy

ai-proxy-multi

ai-cache

ai-rate-limiting

ai-prompt-guard

ai-aws-content-moderation

ai-aliyun-content-moderation

ai-lakera-guard

ai-prompt-decorator

ai-prompt-template

ai-rag

ai-request-rewrite

General

batch-requests

redirect

echo

Gzip Compression (gzip)

Brotli Compression (brotli)

real-ip

server-info

External Plugin Pre-Request (ext-plugin-pre-req)

External Plugin Post-Request (ext-plugin-post-req)

ext-plugin-post-resp

inspect

ocsp-stapling

Transformation

response-rewrite

error-page

proxy-rewrite

gRPC Transcoding (grpc-transcode)

grpc-web

fault-injection

API Mocking (mocking)

degraphql

body-transformer

request-decompress

attach-consumer-label

exit-transformer

Authentication

key-auth

jwt-auth

jwe-decrypt

basic-auth

Keycloak Authorization (authz-keycloak)

Casdoor Authorization (authz-casdoor)

wolf-rbac

openid-connect

cas-auth

dingtalk-auth

feishu-auth

hmac-auth

Casbin Authorization (authz-casbin)

ldap-auth

ldap-auth-advanced

Open Policy Agent (opa)

Forward Authentication (forward-auth)

multi-auth

saml-auth

Security

cors

uri-blocker

ip-restriction

User-Agent Restriction (ua-restriction)

referer-restriction

consumer-restriction

acl

CSRF Protection (csrf)

public-api

GM Encryption (gm)

chaitin-waf

data-mask

Traffic

Rate Limiting (limit-req)

Concurrency Limiting (limit-conn)

Rate Limiting by Count (limit-count)

graphql-limit-count

proxy-cache

graphql-proxy-cache

request-validation

oas-validator

proxy-mirror

API Circuit Breaker (api-breaker)

traffic-split

traffic-label

request-id

proxy-control

proxy-buffering

client-control

workflow

Observability

Tracers

zipkin

skywalking

opentelemetry

Metrics

prometheus

node-status

datadog

Loggers

http-logger

skywalking-logger

tcp-logger

kafka-logger

rocketmq-logger

udp-logger

clickhouse-logger

syslog

log-rotate

error-log-logger

Alibaba Cloud SLS Logger (sls-logger)

google-cloud-logging

splunk-hec-logging

file-logger

loggly

elasticsearch-logger

tencent-cloud-cls

loki-logger

Lago Plugin

Serverless

Serverless Functions (serverless)

azure-functions

Apache OpenWhisk (openwhisk)

aws-lambda

openfunction

Other protocols

dubbo-proxy

mqtt-proxy

kafka-proxy

websocket-proxy

HTTP to Dubbo (http-dubbo)

openapi-to-mcp

API

Admin API

Control API

Status API

Apache APISIX Dashboard

Development

Build development environment with Dev Containers

Building APISIX from source

Build development environment on Mac

Support FIPS in APISIX

External Plugin

Wasm

internal

The Implementation of Plugin Runner

Introducing APISIX's testing framework

Plugin Develop

Debug mode

Deployment modes

Apache APISIX FAQ: API Gateway Questions

Others

Discovery

Integration service discovery registry

DNS

consul

consul_kv

nacos

eureka

Control Plane Service Discovery

Kubernetes

PubSub

PubSub

Apache Kafka

xRPC

redis

xRPC

router-radixtree

Stream Proxy

gRPC Proxy

Customize Nginx configuration

Certificate

Batch Processor

Benchmark

Install Dependencies

APISIX variable

Running APISIX in AWS with AWS CDK

Mutual TLS Authentication

Debug Function

Configuration based on environments

SSL Protocol

security-threat-model

HTTP/3 and QUIC in Apache APISIX

Upgrade Guide

Admin API

Description

The Admin API lets users control their deployed Apache APISIX instance. The architecture design gives an idea about how everything fits together.

Configuration

When APISIX is started, the Admin API will listen on port 9180 by default and take the API prefixed with /apisix/admin.

Therefore, to avoid conflicts between your designed API and /apisix/admin, you can modify the configuration file /conf/config.yaml to modify the default listening port.

APISIX supports setting the IP access allowlist of Admin API to prevent APISIX from being illegally accessed and attacked. You can configure the IP addresses to allow access in the deployment.admin.allow_admin option in the ./conf/config.yaml file.

The X-API-KEY shown below refers to the deployment.admin.admin_key.key in the ./conf/config.yaml file, which is the access token for the Admin API.

TIP

For security reasons, please modify the default admin_key, and check the allow_admin IP access list.

./conf/config.yaml

deployment:

admin:

admin_key:

- name: admin

key: your-admin-key # set a secure Admin API key; if left empty, APISIX will generate one during initialization and write it back to this file

role: admin

allow_admin: # http://nginx.org/en/docs/http/ngx_http_access_module.html#allow

- 127.0.0.0/24

admin_listen:

ip: 0.0.0.0 # Specific IP, if not set, the default value is `0.0.0.0`.

port: 9180 # Specific port, which must be different from node_listen's port.

The role of each key is admin, which can read and change the configuration, or viewer, which can only read it. Unless admin_key_required is false, which turns off the key check, configuration loading refuses any other value and a missing one. When key checks are enabled, each entry must be a map, and its key must be a string or null if present. An empty, missing or null key matches no request. APISIX generates a key when it starts only for a key of the admin role that is set to '', and writes the generated key to the configuration file.

Using environment variables

To configure via environment variables, you can use the ${{VAR}} syntax. For instance:

./conf/config.yaml

deployment:

admin:

admin_key:

- name: admin

key: ${{ADMIN_KEY}}

role: admin

allow_admin:

- 127.0.0.0/24

admin_listen:

ip: 0.0.0.0

port: 9180

And then run export ADMIN_KEY=$your_admin_key before running make init.

If the configured environment variable can’t be found, an error will be thrown.

If you want to use a default value when the environment variable is not set, use ${{VAR:=default_value}} instead. For instance:

./conf/config.yaml

deployment:

admin:

admin_key:

- name: admin

key: ${{ADMIN_KEY:=your-admin-key}}

role: admin

allow_admin:

- 127.0.0.0/24

admin_listen:

ip: 0.0.0.0

port: 9180

This will find the environment variable ADMIN_KEY first, and if it does not exist, it will use the fallback value you provided in the configuration.

You can also specify environment variables in yaml keys. This is specifically useful in the standalone mode where you can specify the upstream nodes as follows:

./conf/apisix.yaml

routes:

-

uri: "/test"

upstream:

nodes:

"${{HOST_IP}}:${{PORT}}": 1

type: roundrobin

#END

Force Delete

By default, the Admin API checks for references between resources and will refuse to delete resources in use.

You can make a force deletion by adding the request argument force=true to the delete request, for example:

NOTE

You can fetch the admin_key from config.yaml and save to an environment variable with the following command:

admin_key=$(yq '.deployment.admin.admin_key[0].key' conf/config.yaml | sed 's/"//g')

$ curl http://127.0.0.1:9180/apisix/admin/upstreams/1 -H "X-API-KEY: $admin_key" -X PUT -d '{

"nodes": {

"127.0.0.1:8080": 1

},

"type": "roundrobin"

}'

$ curl http://127.0.0.1:9180/apisix/admin/routes/1 -H "X-API-KEY: $admin_key" -X PUT -d '{

"uri": "/*",

"upstream_id": 1

}'

{"value":{"priority":0,"upstream_id":1,"uri":"/*","create_time":1689038794,"id":"1","status":1,"update_time":1689038916},"key":"/apisix/routes/1"}

$ curl http://127.0.0.1:9180/apisix/admin/upstreams/1 -H "X-API-KEY: $admin_key" -X DELETE

{"error_msg":"can not delete this upstream, route [1] is still using it now"}

$ curl "http://127.0.0.1:9180/apisix/admin/upstreams/1?force=anyvalue" -H "X-API-KEY: $admin_key" -X DELETE

{"error_msg":"can not delete this upstream, route [1] is still using it now"}

$ curl "http://127.0.0.1:9180/apisix/admin/upstreams/1?force=true" -H "X-API-KEY: $admin_key" -X DELETE

{"deleted":"1","key":"/apisix/upstreams/1"}

V3 new feature

The Admin API has made some breaking changes in V3 version, as well as supporting additional features.

Support new response body format

Remove action field in response body;

Adjust the response body structure when fetching the list of resources, the new response body structure like:

Return single resource:

{

"modifiedIndex": 2685183,

"value": {

"id": "1",

...

},

"key": "/apisix/routes/1",

"createdIndex": 2684956

}

Return multiple resources:

{

"list": [

{

"modifiedIndex": 2685183,

"value": {

"id": "1",

...

},

"key": "/apisix/routes/1",

"createdIndex": 2684956

},

{

"modifiedIndex": 2685163,

"value": {

"id": "2",

...

},

"key": "/apisix/routes/2",

"createdIndex": 2685163

}

],

"total": 2

}

Support paging query

Paging query is supported when getting the resource list, paging parameters include:

parameterDefaultValid rangeDescription

page1[1, …]Number of pages.

page_size[10, 500]Number of resources per page.

The example is as follows:

curl "http://127.0.0.1:9180/apisix/admin/routes?page=1&page_size=10" \

-H "X-API-KEY: $admin_key" -X GET

{

"total": 1,

"list": [

{

...

}

]

}

Resources that support paging queries:

Consumer

Consumer Group

Global Rules

Plugin Config

Proto

Route

Service

SSL

Stream Route

Upstream

Secret

Support filtering query

When getting a list of resources, it supports filtering resources based on name, label, uri.

parameterparameter

nameQuery resource by their name, which will not appear in the query results if the resource itself does not have name.

labelQuery resource by their label, which will not appear in the query results if the resource itself does not have label.

uriSupported on Route resources only. If the uri of a Route is equal to the uri of the query or if the uris contains the uri of the query, the Route resource appears in the query results.

TIP

When multiple filter parameters are enabled, use the intersection of the query results for different filter parameters.

The following example will return a list of routes, and all routes in the list satisfy: the name of the route contains the string “test”, the uri contains the string “foo”, and there is no restriction on the label of the route, since the label of the query is the empty string.

curl 'http://127.0.0.1:9180/apisix/admin/routes?name=test&uri=foo&label=' \

-H "X-API-KEY: $admin_key" -X GET

{

"total": 1,

"list": [

{

...

}

]

}

Support reference filtering query

NOTE

This feature was introduced in APISIX 3.13.0.

APISIX supports querying routes and stream routes by service_id and upstream_id. Other resources or fields are not currently supported.

When getting a list of resources, it supports a filter for filtering resources by filters.

It is encoded in the following manner.

filter=escape_uri(key1=value1&key2=value2)

The following example filters routes using service_id. Applying multiple filters simultaneously will return results that match all filter conditions.

curl 'http://127.0.0.1:9180/apisix/admin/routes?filter=service_id%3D1' \

-H "X-API-KEY: $admin_key" -X GET

{

"total": 1,

"list": [

{

...

}

]

}

Route

Routes match the client’s request based on defined rules, loads and executes the corresponding plugins, and forwards the request to the specified Upstream.

Route API

Route resource request address: /apisix/admin/routes/{id}?ttl=0

Quick Note on ID Syntax

ID’s as a text string must be of a length between 1 and 64 characters and they should only contain uppercase, lowercase, numbers and no special characters apart from dashes ( - ), periods ( . ) and underscores ( _ ). For integer values they simply must have a minimum character count of 1.

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/routesNULLFetches a list of all configured Routes.

GET/apisix/admin/routes/{id}NULLFetches specified Route by id.

PUT/apisix/admin/routes/{id}{…}Creates a Route with the specified id.

POST/apisix/admin/routes{…}Creates a Route and assigns a random id.

DELETE/apisix/admin/routes/{id}NULLRemoves the Route with the specified id.

PATCH/apisix/admin/routes/{id}{…}Standard PATCH, which modifies the specified attributes of the Route, while all other attributes remain unchanged. To delete an attribute, set its value to null. Note that if an attribute is an array, it will be completely replaced.

PATCH/apisix/admin/routes/{id}/{path}{…}Subpath PATCH, which specifies the Route attribute to update via {path} and completely replaces that attribute’s data, while all other attributes remain unchanged.

URI Request Parameters

parameterRequiredTypeDescriptionExample

ttlFalseAuxiliaryRequest expires after the specified target seconds.ttl=1

Request Body Parameters

ParameterRequiredTypeDescriptionExample

nameFalseAuxiliaryIdentifier for the Route.route-xxxx

descFalseAuxiliaryDescription of usage scenarios.route xxxx

uriTrue, can’t be used with urisMatch RulesMatches the uri. For more advanced matching see Router.”/hello”

urisTrue, can’t be used with uriMatch RulesMatches with any one of the multiple uris specified in the form of a non-empty list.[“/hello”, “/word”]

hostFalse, can’t be used with hostsMatch RulesMatches with domain names such as foo.com or PAN domain names like *.foo.com.”foo.com”

hostsFalse, can’t be used with hostMatch RulesMatches with any one of the multiple hosts specified in the form of a non-empty list.[“foo.com”, “*.bar.com”]

remote_addrFalse, can’t be used with remote_addrsMatch RulesMatches with the specified IP address in standard IPv4 format (192.168.1.101), CIDR format (192.168.1.0/24), or in IPv6 format (::1, fe80::1, fe80::1/64).“192.168.1.0/24”

remote_addrsFalse, can’t be used with remote_addrMatch RulesMatches with any one of the multiple remote_addrs specified in the form of a non-empty list.[“127.0.0.1”, “192.0.0.0/8”, “::1”]

methodsFalseMatch RulesMatches with the specified methods. Matches all methods if empty or unspecified.[“GET”, “POST”]

priorityFalseMatch RulesIf different Routes matches to the same uri, then the Route is matched based on its priority. A higher value corresponds to higher priority. It is set to 0 by default.priority = 10

varsFalseMatch RulesMatches based on the specified variables consistent with variables in Nginx. Takes the form [[var, operator, val], [var, operator, val], ...]]. Note that this is case sensitive when matching a cookie name. See lua-resty-expr for more details.[[“arg_name”, ”==”, “json”], [“arg_age”, ”>”, 18]]

filter_funcFalseMatch RulesMatches using a user-defined function in Lua. Used in scenarios where vars is not sufficient. Functions accept an argument vars which provides access to built-in variables (including Nginx variables).function(vars) return tonumber(vars.arg_userid) % 4 > 2; end

pluginsFalsePluginPlugins that are executed during the request/response cycle. See Plugin for more.

scriptFalseScriptUsed for writing arbitrary Lua code or directly calling existing plugins to be executed. See Script for more.

upstreamFalseUpstreamConfiguration of the Upstream.

upstream_idFalseUpstreamId of the Upstream service.

service_idFalseServiceConfiguration of the bound Service.

plugin_config_idFalse, can’t be used with scriptPluginPlugin config bound to the Route.

labelsFalseMatch RulesAttributes of the Route specified as key-value pairs.{“version”:“v2”,“build”:“16”,“env”:“production”}

timeoutFalseAuxiliarySets the timeout (in seconds) for connecting to, and sending and receiving messages between the Upstream and the Route. This will overwrite the timeout value configured in your Upstream.{“connect”: 3, “send”: 3, “read”: 3}

enable_websocketFalseAuxiliaryEnables a websocket. Set to false by default. This is a plain protocol upgrade with no access to individual frames; see the note under Upstream scheme if a plugin needs to inspect or rewrite them.

statusFalseAuxiliaryEnables the current Route. Set to 1 (enabled) by default.1 to enable, 0 to disable

Example configuration:

{

"id": "1", # id, unnecessary.

"uris": ["/a","/b"], # A set of uri.

"methods": ["GET","POST"], # Can fill multiple methods

"hosts": ["a.com","b.com"], # A set of host.

"plugins": {}, # Bound plugin

"priority": 0, # If different routes contain the same `uri`, determine which route is matched first based on the attribute` priority`, the default value is 0.

"name": "route-xxx",

"desc": "hello world",

"remote_addrs": ["127.0.0.1"], # A set of Client IP.

"vars": [["http_user", "==", "ios"]], # A list of one or more `[var, operator, val]` elements

"upstream_id": "1", # upstream id, recommended

"upstream": {}, # upstream, not recommended

"timeout": { # Set the upstream timeout for connecting, sending and receiving messages of the route.

"connect": 3,

"send": 3,

"read": 3

},

"filter_func": "" # User-defined filtering function

}

Example API usage

Create a route

curl http://127.0.0.1:9180/apisix/admin/routes/1 -H "X-API-KEY: $admin_key" -X PUT -i -d '

{

"uri": "/index.html",

"hosts": ["foo.com", "*.bar.com"],

"remote_addrs": ["127.0.0.0/8"],

"methods": ["PUT", "GET"],

"enable_websocket": true,

"upstream": {

"type": "roundrobin",

"nodes": {

"127.0.0.1:1980": 1

}

}

}'

HTTP/1.1 201 Created

Date: Sat, 31 Aug 2019 01:17:15 GMT

...

Create a route expires after 60 seconds, then it’s deleted automatically

curl 'http://127.0.0.1:9180/apisix/admin/routes/2?ttl=60' \

-H "X-API-KEY: $admin_key" -X PUT -i -d '

{

"uri": "/aa/index.html",

"upstream": {

"type": "roundrobin",

"nodes": {

"127.0.0.1:1980": 1

}

}

}'

HTTP/1.1 201 Created

Date: Sat, 31 Aug 2019 01:17:15 GMT

...

Add an upstream node to the Route

curl http://127.0.0.1:9180/apisix/admin/routes/1 \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"upstream": {

"nodes": {

"127.0.0.1:1981": 1

}

}

}'

HTTP/1.1 200 OK

...

After successful execution, upstream nodes will be updated to:

{

"127.0.0.1:1980": 1,

"127.0.0.1:1981": 1

}

Update the weight of an upstream node to the Route

curl http://127.0.0.1:9180/apisix/admin/routes/1 \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"upstream": {

"nodes": {

"127.0.0.1:1981": 10

}

}

}'

HTTP/1.1 200 OK

...

After successful execution, upstream nodes will be updated to:

{

"127.0.0.1:1980": 1,

"127.0.0.1:1981": 10

}

Delete an upstream node for the Route

curl http://127.0.0.1:9180/apisix/admin/routes/1 \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"upstream": {

"nodes": {

"127.0.0.1:1980": null

}

}

}'

HTTP/1.1 200 OK

...

After successful execution, upstream nodes will be updated to:

{

"127.0.0.1:1981": 10

}

Replace methods of the Route — array

curl http://127.0.0.1:9180/apisix/admin/routes/1 \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '{

"methods": ["GET", "POST"]

}'

HTTP/1.1 200 OK

...

After successful execution, methods will not retain the original data, and the entire update is:

["GET", "POST"]

Replace upstream nodes of the Route — sub path

curl http://127.0.0.1:9180/apisix/admin/routes/1/upstream/nodes \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"127.0.0.1:1982": 1

}'

HTTP/1.1 200 OK

...

After successful execution, nodes will not retain the original data, and the entire update is:

{

"127.0.0.1:1982": 1

}

Replace methods of the Route — sub path

curl http://127.0.0.1:9180/apisix/admin/routes/1/methods \

-H "X-API-KEY: $admin_key" -X PATCH -i -d'["POST", "DELETE", " PATCH"]'

HTTP/1.1 200 OK

...

After successful execution, methods will not retain the original data, and the entire update is:

["POST", "DELETE", "PATCH"]

Disable route

curl http://127.0.0.1:9180/apisix/admin/routes/1 \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"status": 0

}'

HTTP/1.1 200 OK

...

After successful execution, status nodes will be updated to:

{

"status": 0

}

Enable route

curl http://127.0.0.1:9180/apisix/admin/routes/1 \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"status": 1

}'

HTTP/1.1 200 OK

...

After successful execution, status nodes will be updated to:

{

"status": 1

}

Response Parameters

Currently, the response is returned from etcd.

Service

A Service is an abstraction of an API (which can also be understood as a set of Route abstractions). It usually corresponds to an upstream service abstraction.

The relationship between Routes and a Service is usually N

.

Service API

Service resource request address: /apisix/admin/services/{id}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/servicesNULLFetches a list of available Services.

GET/apisix/admin/services/{id}NULLFetches specified Service by id.

PUT/apisix/admin/services/{id}{…}Creates a Service with the specified id.

POST/apisix/admin/services{…}Creates a Service and assigns a random id.

DELETE/apisix/admin/services/{id}NULLRemoves the Service with the specified id.

PATCH/apisix/admin/services/{id}{…}Standard PATCH, which modifies the specified attributes of the Service, while all other attributes remain unchanged. To delete an attribute, set its value to null. Note that if an attribute is an array, it will be completely replaced.

PATCH/apisix/admin/services/{id}/{path}{…}Subpath PATCH, which specifies the Service attribute to update via {path} and completely replaces that attribute’s data, while all other attributes remain unchanged.

Request Body Parameters

ParameterRequiredTypeDescriptionExample

pluginsFalsePluginPlugins that are executed during the request/response cycle. See Plugin for more.

upstreamFalseUpstreamConfiguration of the Upstream.

upstream_idFalseUpstreamId of the Upstream service.

nameFalseAuxiliaryIdentifier for the Service.service-xxxx

descFalseAuxiliaryDescription of usage scenarios.service xxxx

labelsFalseMatch RulesAttributes of the Service specified as key-value pairs.{“version”:“v2”,“build”:“16”,“env”:“production”}

enable_websocketFalseAuxiliaryEnables a websocket. Set to false by default. This is a plain protocol upgrade with no access to individual frames; see the note under Upstream scheme if a plugin needs to inspect or rewrite them.

hostsFalseMatch RulesMatches with any one of the multiple hosts specified in the form of a non-empty list.[“foo.com”, “*.bar.com”]

Example configuration:

{

"id": "1", # id

"plugins": {}, # Bound plugin

"upstream_id": "1", # upstream id, recommended

"upstream": {}, # upstream, not recommended

"name": "service-test",

"desc": "hello world",

"enable_websocket": true,

"hosts": ["foo.com"]

}

Example API usage

Create a service

curl http://127.0.0.1:9180/apisix/admin/services/201 \

-H "X-API-KEY: $admin_key" -X PUT -i -d '

{

"plugins": {

"limit-count": {

"count": 2,

"time_window": 60,

"rejected_code": 503,

"key": "remote_addr"

}

},

"enable_websocket": true,

"upstream": {

"type": "roundrobin",

"nodes": {

"127.0.0.1:1980": 1

}

}

}'

HTTP/1.1 201 Created

...

Add an upstream node to the Service

curl http://127.0.0.1:9180/apisix/admin/services/201 \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"upstream": {

"nodes": {

"127.0.0.1:1981": 1

}

}

}'

HTTP/1.1 200 OK

...

After successful execution, upstream nodes will be updated to:

{

"127.0.0.1:1980": 1,

"127.0.0.1:1981": 1

}

Update the weight of an upstream node to the Service

curl http://127.0.0.1:9180/apisix/admin/services/201 \

-H'X-API-KEY: your-admin-key' -X PATCH -i -d '

{

"upstream": {

"nodes": {

"127.0.0.1:1981": 10

}

}

}'

HTTP/1.1 200 OK

...

After successful execution, upstream nodes will be updated to:

{

"127.0.0.1:1980": 1,

"127.0.0.1:1981": 10

}

Delete an upstream node for the Service

curl http://127.0.0.1:9180/apisix/admin/services/201 \

-H'X-API-KEY: your-admin-key' -X PATCH -i -d '

{

"upstream": {

"nodes": {

"127.0.0.1:1980": null

}

}

}'

HTTP/1.1 200 OK

...

After successful execution, upstream nodes will be updated to:

{

"127.0.0.1:1981": 10

}

Replace upstream nodes of the Service

curl http://127.0.0.1:9180/apisix/admin/services/201/upstream/nodes \

-H'X-API-KEY: your-admin-key' -X PATCH -i -d '

{

"127.0.0.1:1982": 1

}'

HTTP/1.1 200 OK

...

After successful execution, upstream nodes will not retain the original data, and the entire update is:

{

"127.0.0.1:1982": 1

}

Response Parameters

Currently, the response is returned from etcd.

Consumer

Consumers are users of services and can only be used in conjunction with a user authentication system. A Consumer is identified by a username property. So, for creating a new Consumer, only the HTTP PUT method is supported.

Consumer API

Consumer resource request address: /apisix/admin/consumers/{username}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/consumersNULLFetches a list of all Consumers.

GET/apisix/admin/consumers/{username}NULLFetches specified Consumer by username.

PUT/apisix/admin/consumers{…}Create new Consumer.

DELETE/apisix/admin/consumers/{username}NULLRemoves the Consumer with the specified username.

Request Body Parameters

ParameterRequiredTypeDescriptionExample

usernameTrueNameName of the Consumer.

group_idFalseNameGroup of the Consumer.

pluginsFalsePluginPlugins that are executed during the request/response cycle. See Plugin for more.

descFalseAuxiliaryDescription of usage scenarios.customer xxxx

labelsFalseMatch RulesAttributes of the Consumer specified as key-value pairs.{“version”:“v2”,“build”:“16”,“env”:“production”}

Example Configuration:

{

"plugins": {}, # Bound plugin

"username": "name", # Consumer name

"desc": "hello world" # Consumer desc

}

When bound to a Route or Service, the Authentication Plugin infers the Consumer from the request and does not require any parameters. Whereas, when it is bound to a Consumer, username, password and other information needs to be provided.

Example API usage

curl http://127.0.0.1:9180/apisix/admin/consumers \

-H "X-API-KEY: $admin_key" -X PUT -i -d '

{

"username": "jack",

"plugins": {

"key-auth": {

"key": "auth-one"

},

"limit-count": {

"count": 2,

"time_window": 60,

"rejected_code": 503,

"key": "remote_addr"

}

}

}'

HTTP/1.1 200 OK

Date: Thu, 26 Dec 2019 08:17:49 GMT

...

{"node":{"value":{"username":"jack","plugins":{"key-auth":{"key":"auth-one"},"limit-count":{"time_window":60,"count":2,"rejected_code":503,"key":"remote_addr","policy":"local"}}},"createdIndex":64,"key":"\/apisix\/consumers\/jack","modifiedIndex":64},"prevNode":{"value":"{\"username\":\"jack\",\"plugins\":{\"key-auth\":{\"key\":\"auth-one\"},\"limit-count\":{\"time_window\":60,\"count\":2,\"rejected_code\":503,\"key\":\"remote_addr\",\"policy\":\"local\"}}}","createdIndex":63,"key":"\/apisix\/consumers\/jack","modifiedIndex":63}}

Since v2.2, we can bind multiple authentication plugins to the same consumer.

Response Parameters

Currently, the response is returned from etcd.

Credential

Credential is used to hold the authentication credentials for the Consumer.

Credentials are used when multiple credentials need to be configured for a Consumer.

Credential API

Credential resource request address：/apisix/admin/consumers/{username}/credentials/{credential_id}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/consumers/{username}/credentialsNULLFetches list of all credentials of the Consumer

GET/apisix/admin/consumers/{username}/credentials/{credential_id}NULLFetches the Credential by credential_id

PUT/apisix/admin/consumers/{username}/credentials/{credential_id}{…}Create or update a Credential

DELETE/apisix/admin/consumers/{username}/credentials/{credential_id}NULLDelete the Credential

Request Body Parameters

ParameterRequiredTypeDescriptionExample

pluginsFalsePluginAuth plugins configuration.

nameFalseAuxiliaryName of the Consumer Credential.credential_primary

descFalseAuxiliaryDescription of usage scenarios.credential xxxx

labelsFalseMatch RulesAttributes of the Credential specified as key-value pairs.{“version”:“v2”,“build”:“16”,“env”:“production”}

Example Configuration:

{

"plugins": {

"key-auth": {

"key": "auth-one"

}

},

"desc": "hello world"

}

Example API usage

Prerequisite: Consumer jack has been created.

Create the key-auth Credential for consumer jack:

curl http://127.0.0.1:9180/apisix/admin/consumers/jack/credentials/auth-one \

-H "X-API-KEY: $admin_key" -X PUT -i -d '

{

"plugins": {

"key-auth": {

"key": "auth-one"

}

}

}'

HTTP/1.1 200 OK

Date: Thu, 26 Dec 2019 08:17:49 GMT

...

{"key":"\/apisix\/consumers\/jack\/credentials\/auth-one","value":{"update_time":1666260780,"plugins":{"key-auth":{"key":"auth-one"}},"create_time":1666260780}}

Upstream

Upstream is a virtual host abstraction that performs load balancing on a given set of service nodes according to the configured rules.

An Upstream configuration can be directly bound to a Route or a Service, but the configuration in Route has a higher priority. This behavior is consistent with priority followed by the Plugin object.

Upstream API

Upstream resource request address: /apisix/admin/upstreams/{id}

For notes on ID syntax please refer to: ID Syntax

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/upstreamsNULLFetch a list of all configured Upstreams.

GET/apisix/admin/upstreams/{id}NULLFetches specified Upstream by id.

PUT/apisix/admin/upstreams/{id}{…}Creates an Upstream with the specified id.

POST/apisix/admin/upstreams{…}Creates an Upstream and assigns a random id.

DELETE/apisix/admin/upstreams/{id}NULLRemoves the Upstream with the specified id.

PATCH/apisix/admin/upstreams/{id}{…}Standard PATCH, which modifies the specified attributes of the existing Upstream, while all other attributes remain unchanged. To delete an attribute, set its value to null. Note that if an attribute is an array, it will be completely replaced.

PATCH/apisix/admin/upstreams/{id}/{path}{…}Subpath PATCH, which specifies the Upstream attribute to update via {path} and completely replaces that attribute’s data, while all other attributes remain unchanged.

Request Body Parameters

In addition to the equalization algorithm selections, Upstream also supports passive health check and retry for the upstream. See the table below for more details:

ParameterRequiredTypeDescriptionExample

typeFalseEnumerationLoad balancing algorithm to be used, and the default value is roundrobin.

nodesTrue, can’t be used with service_nameNodeIP addresses (with optional ports) of the Upstream nodes represented as a hash table or an array. In the hash table, the key is the IP address and the value is the weight of the node for the load balancing algorithm. For hash table case, if the key is IPv6 address with port, then the IPv6 address must be quoted with square brackets. In the array, each item is a hash table with keys host, weight, and the optional port and priority (defaults to 0). Nodes with lower priority are used only when all nodes with a higher priority are tried and are unavailable. Empty nodes are treated as placeholders and clients trying to access this Upstream will receive a 502 response.192.168.1.100:80, [::1]:80

service_nameTrue, can’t be used with nodesStringService name used for service discovery.a-bootiful-client

discovery_typeTrue, if service_name is usedStringThe type of service discovery.eureka

hash_onFalseAuxiliaryOnly valid if the type is chash. Supports Nginx variables (vars), custom headers (header), cookie and consumer. Defaults to vars.

keyFalseMatch RulesOnly valid if the type is chash. Finds the corresponding node id according to hash_on and key values. When hash_on is set to vars, key is a required parameter and it supports Nginx variables. When hash_on is set as header, key is a required parameter, and header name can be customized. When hash_on is set to cookie, key is also a required parameter, and cookie name can be customized. When hash_on is set to consumer, key need not be set and the key used by the hash algorithm would be the authenticated consumer_name.uri, server_name, server_addr, request_uri, remote_port, remote_addr, query_string, host, hostname, arg_***, arg_***

checksFalseHealth CheckerConfigures the parameters for the health check.

retriesFalseIntegerSets the number of retries while passing the request to Upstream using the underlying Nginx mechanism. Set according to the number of available backend nodes by default. Setting this to 0 disables retry.

retry_timeoutFalseNumberTimeout to continue with retries. Setting this to 0 disables the retry timeout.

timeoutFalseTimeoutSets the timeout (in seconds) for connecting to, and sending and receiving messages to and from the Upstream.{"connect": 0.5,"send": 0.5,"read": 0.5}

nameFalseAuxiliaryIdentifier for the Upstream.

descFalseAuxiliaryDescription of usage scenarios.

pass_hostFalseEnumerationConfigures the host when the request is forwarded to the upstream. Can be one of pass, node or rewrite. Defaults to pass if not specified. pass- transparently passes the client’s host to the Upstream. node- uses the host configured in the node of the Upstream. rewrite- Uses the value configured in upstream_host.

upstream_hostFalseAuxiliarySpecifies the host of the Upstream request. This is only valid if the pass_host is set to rewrite.

schemeFalseAuxiliaryThe scheme used when communicating with the Upstream. For an L7 proxy, this value can be one of http, https, grpc, grpcs, ws, wss. For an L4 proxy, this value could be one of tcp, udp, tls. Defaults to http.

labelsFalseMatch RulesAttributes of the Upstream specified as key-value pairs.{“version”:“v2”,“build”:“16”,“env”:“production”}

tls.client_certFalse, can’t be used with tls.client_cert_idHTTPS certificateSets the client certificate while connecting to a TLS Upstream.

tls.client_keyFalse, can’t be used with tls.client_cert_idHTTPS certificate private keySets the client private key while connecting to a TLS Upstream.

tls.client_cert_idFalse, can’t be used with tls.client_cert and tls.client_keySSLSet the referenced SSL id.

tls.verifyFalseBooleanEnables or disables verification of the Upstream certificate. Falls back to the nginx configuration when unset. Also used by the kafka scheme.

tls.ca_certsFalseArray of HTTPS certificatesCA certificates used to verify the Upstream certificate, replacing the ones loaded from ssl_trusted_certificate. Not supported when scheme is ws or wss, which only trust ssl_trusted_certificate.

keepalive_pool.sizeFalseAuxiliarySets keepalive directive dynamically.

keepalive_pool.idle_timeoutFalseAuxiliarySets keepalive_timeout directive dynamically.

keepalive_pool.requestsFalseAuxiliarySets keepalive_requests directive dynamically.

warm_up_conf.slow_start_time_secondsTrue, when warm_up_conf is setIntegerSlow start window in seconds. A node the gateway observes for the first time takes a reduced share of the traffic and ramps back to its configured weight over this window. Must be at least 1.300

warm_up_conf.min_weight_percentTrue, when warm_up_conf is setIntegerLowest effective weight during the ramp, as a percentage of the configured weight, from 1 to 100.1

warm_up_conf.intervalFalseIntegerSeconds between two effective weight refreshes. Defaults to 1, and cannot be greater than slow_start_time_seconds.1

warm_up_conf.aggressionFalseNumberShape of the ramp. 1 (default) is linear, above 1 ramps up faster at the beginning, below 1 slower. At least 0.01.1

warm_up_conf.startup_grace_period_secondsFalseIntegerSeconds after the gateway starts during which a node observed for the first time is treated as already warmed up, so that a restart does not ramp the whole node set again. A node that was already observed and then becomes unavailable still ramps when it comes back, also during this period. Defaults to 0.180

An Upstream can be one of the following types:

roundrobin: Round robin balancing with weights.

chash: Consistent hash.

ewma: Pick the node with minimum latency. See EWMA Chart for more details.

least_conn: Picks the node with the lowest value of (active_conn + 1) / weight. Here, an active connection is a connection being used by the request and is similar to the concept in Nginx.

user-defined load balancer loaded via require("apisix.balancer.your_balancer").

The following should be considered when setting the hash_on value:

When set to vars, a key is required. The value of the key can be any of the Nginx variables without the $ prefix.

When set to header, a key is required. This is equal to “http_key”.

When set to cookie, a key is required. This key is equal to “cookie_key”. The cookie name is case-sensitive.

When set to consumer, the key is optional and the key is set to the consumer_name captured from the authentication Plugin.

When set to vars_combinations, the key is required. The value of the key can be a combination of any of the Nginx variables like $request_uri$remote_addr.

APISIX supports proxying WebSocket connections in two different ways, and they don’t combine:

Route or Service level enable_websocket with an http/https Upstream scheme. This is a plain protocol upgrade: nginx’s own proxy_pass forwards the raw TCP stream after the 101 Switching Protocols handshake, and no plugin phase sees the individual WebSocket frames.

Upstream scheme: ws or scheme: wss. APISIX parses and proxies the WebSocket frames itself in both directions, which lets a plugin inspect or rewrite frames in flight through the ws_handshake, ws_client_frame, ws_upstream_frame, and ws_close phases. See the “extra phase” section of the plugin development guide for how to hook into them. enable_websocket is ignored on a Route or a Service whose Upstream uses this scheme, since the connection never reaches the proxy_pass path it configures.

The features described below requires APISIX to be run on APISIX-Runtime:

You can set the scheme to tls, which means “TLS over TCP”.

To use mTLS to communicate with Upstream, you can use the tls.client_cert/key in the same format as SSL’s cert and key fields.

Or you can reference SSL object by tls.client_cert_id to set SSL cert and key. The SSL object can be referenced only if the type field is client, otherwise the request will be rejected by APISIX. In addition, only cert and key will be used in the SSL object.

To verify the certificate presented by the Upstream, set tls.verify to true. Leaving it unset keeps the behaviour configured in nginx, which is off unless proxy_ssl_verify is turned on. The certificate is checked against the CA certificates in tls.ca_certs, or against ssl_trusted_certificate from config.yaml when tls.ca_certs is not set. In the latter case, a CRL configured in nginx with proxy_ssl_crl (grpc_ssl_crl for gRPC) is also checked:

{

"scheme": "https",

"type": "roundrobin",

"nodes": {

"127.0.0.1:8443": 1

},

"tls": {

"verify": true,

"ca_certs": ["<content of ca.crt>"]

}

}

To allow Upstream to have a separate connection pool, use keepalive_pool. It can be configured by modifying its child fields.

warm_up_conf enables slow start for the nodes of a roundrobin Upstream. Whether a node is new is decided by the gateway itself, from the node set it observes, and the start of each ramp is recorded locally in the upstream-slow-start shared dict:

The node set an Upstream has when the gateway first builds a load balancer for it is treated as warmed up. So is the node set it already has when warm_up_conf is turned on.

A node added afterwards ramps from min_weight_percent back to its configured weight over slow_start_time_seconds. A node held out of the load balancer by a health check starts its ramp when it first becomes available.

A node that becomes unavailable, either because a health check takes it out of the load balancer or because it leaves the Upstream, ends its ramp, or its warmed-up state. When it is available again it ramps from min_weight_percent from the start, however short the unavailability was, because the process answering on that address may have just restarted. Changing the weight of a node, including to 0, does not count as unavailability.

Every APISIX instance ramps independently, from the moment it observed the node.

warm_up_conf is only supported by roundrobin Upstreams whose nodes share a single priority, and it is rejected in the Upstreams of the traffic-split Plugin, which are rebuilt per request. Like the other Upstream fields that only apply to HTTP, it is ignored when the Upstream is used by a stream route. A ramp only shifts traffic between nodes: a single-node Upstream, or one whose nodes are all new, keeps sending every request to them.

Example Configuration:

{

"id": "1", # id

"retries": 1, # retry times

"timeout": { # Set the timeout for connecting, sending and receiving messages, each is 15 seconds.

"connect":15,

"send":15,

"read":15

},

"nodes": {"host:80": 100}, # Upstream machine address list, the format is `Address + Port`

# is the same as "nodes": [ {"host": "host", "port": 80, "weight": 100} ],

"type":"roundrobin",

"checks": {}, # Health check parameters

"hash_on": "",

"key": "",

"name": "upstream-for-test",

"desc": "hello world",

"scheme": "http" # The scheme used when communicating with upstream, the default is `http`

}

Example API usage

Create an Upstream and modify the data in nodes

Create upstream

curl http://127.0.0.1:9180/apisix/admin/upstreams/100 \

-H "X-API-KEY: $admin_key" -i -X PUT -d '

{

"type":"roundrobin",

"nodes":{

"127.0.0.1:1980": 1

}

}'

HTTP/1.1 201 Created

...

Add a node to the Upstream

curl http://127.0.0.1:9180/apisix/admin/upstreams/100 \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"nodes": {

"127.0.0.1:1981": 1

}

}'

HTTP/1.1 200 OK

...

After successful execution, nodes will be updated to:

{

"127.0.0.1:1980": 1,

"127.0.0.1:1981": 1

}

Update the weight of a node to the Upstream

curl http://127.0.0.1:9180/apisix/admin/upstreams/100 \

-H'X-API-KEY: your-admin-key' -X PATCH -i -d '

{

"nodes": {

"127.0.0.1:1981": 10

}

}'

HTTP/1.1 200 OK

...

After successful execution, nodes will be updated to:

{

"127.0.0.1:1980": 1,

"127.0.0.1:1981": 10

}

Delete a node for the Upstream

curl http://127.0.0.1:9180/apisix/admin/upstreams/100 \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"nodes": {

"127.0.0.1:1980": null

}

}'

HTTP/1.1 200 OK

...

After successful execution, nodes will be updated to:

{

"127.0.0.1:1981": 10

}

Replace the nodes of the Upstream

curl http://127.0.0.1:9180/apisix/admin/upstreams/100/nodes \

-H "X-API-KEY: $admin_key" -X PATCH -i -d '

{

"127.0.0.1:1982": 1

}'

HTTP/1.1 200 OK

...

After the execution is successful, nodes will not retain the original data, and the entire update is:

{

"127.0.0.1:1982": 1

}

Proxy client request to https Upstream service

Create a route and configure the upstream scheme as https.

curl -i http://127.0.0.1:9180/apisix/admin/routes/1 \

-H "X-API-KEY: $admin_key" -X PUT -d '

{

"uri": "/get",

"upstream": {

"type": "roundrobin",

"scheme": "https",

"nodes": {

"httpbin.org:443": 1

}

}

}'

After successful execution, the scheme when requesting to communicate with the upstream will be https.

Send a request to test.

curl http://127.0.0.1:9080/get

{

"args": {},

"headers": {

"Accept": "*/*",

"Host": "127.0.0.1",

"User-Agent": "curl/7.29.0",

"X-Amzn-Trace-Id": "Root=1-6058324a-0e898a7f04a5e95b526bb183",

"X-Forwarded-Host": "127.0.0.1"

},

"origin": "127.0.0.1",

"url": "https://127.0.0.1/get"

}

The request is successful, meaning that the proxy Upstream https is valid.

NOTE

Each node can be configured with a priority. A node with low priority will only be

used when all the nodes with higher priority have been tried or are unavailable.

As the default priority is 0, nodes with negative priority can be configured as a backup.

For example:

{

"uri": "/hello",

"upstream": {

"type": "roundrobin",

"nodes": [

{ "host": "127.0.0.1", "port": 1980, "weight": 2000 },

{ "host": "127.0.0.1", "port": 1981, "weight": 1, "priority": -1 }

],

"checks": {

"active": {

"http_path": "/status",

"healthy": {

"interval": 1,

"successes": 1

},

"unhealthy": {

"interval": 1,

"http_failures": 1

}

}

}

}

}

Node 127.0.0.2 will be used only after 127.0.0.1 is tried or unavailable.

It can therefore act as a backup for the node 127.0.0.1.

Response Parameters

Currently, the response is returned from etcd.

SSL

SSL API

SSL resource request address: /apisix/admin/ssls/{id}

For notes on ID syntax please refer to: ID Syntax

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/sslsNULLFetches a list of all configured SSL resources.

GET/apisix/admin/ssls/{id}NULLFetch specified resource by id.

PUT/apisix/admin/ssls/{id}{…}Creates a resource with the specified id.

POST/apisix/admin/ssls{…}Creates a resource and assigns a random id.

DELETE/apisix/admin/ssls/{id}NULLRemoves the resource with the specified id.

Request Body Parameters

ParameterRequiredTypeDescriptionExample

certTrueCertificateHTTPS certificate. This field supports saving the value in Secret Manager using the APISIX Secret resource.

keyTruePrivate keyHTTPS private key. This field supports saving the value in Secret Manager using the APISIX Secret resource.

certsFalseAn array of certificatesUsed for configuring multiple certificates for the same domain excluding the one provided in the cert field. This field supports saving the value in Secret Manager using the APISIX Secret resource.

keysFalseAn array of private keysPrivate keys to pair with the certs. This field supports saving the value in Secret Manager using the APISIX Secret resource.

client.caFalseCertificateSets the CA certificate that verifies the client. Requires OpenResty 1.19+.

client.depthFalseCertificateSets the verification depth in client certificate chains. Defaults to 1. Requires OpenResty 1.19+.

client.skip_mtls_uri_regexFalseAn array of regular expressions, in PCRE formatUsed to match URI, if matched, this request bypasses the client certificate checking, i.e. skip the MTLS. When match_uri_encoded_slash is enabled, a URI containing an encoded slash (%2F) must match in both its decoded and its %2F-kept form.[“/hello[0-9]+”, “/foobar”]

snisTrue, only if type is serverMatch RulesA non-empty array of HTTPS SNI

descFalseAuxiliaryDescription of usage scenarios.certs for production env

labelsFalseMatch RulesAttributes of the resource specified as key-value pairs.{“version”:“v2”,“build”:“16”,“env”:“production”}

typeFalseAuxiliaryIdentifies the type of certificate, default server.client Indicates that the certificate is a client certificate, which is used when APISIX accesses the upstream; server Indicates that the certificate is a server-side certificate, which is used by APISIX when verifying client requests.

statusFalseAuxiliaryEnables the current SSL. Set to 1 (enabled) by default.1 to enable, 0 to disable

ssl_protocolsFalseAn array of ssl protocolsIt is used to control the SSL/TLS protocol version used between servers and clients. See SSL Protocol for more examples.["TLSv1.1", "TLSv1.2", "TLSv1.3"]

Example Configuration:

{

"id": "1", # id

"cert": "cert", # Certificate

"key": "key", # Private key

"snis": ["t.com"] # https SNI

}

See Certificate for more examples.

Global Rule

Sets Plugins which run globally. i.e these Plugins will be run before any Route/Service level Plugins.

Global Rule API

Global Rule resource request address: /apisix/admin/global_rules/{id}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/global_rulesNULLFetches a list of all Global Rules.

GET/apisix/admin/global_rules/{id}NULLFetches specified Global Rule by id.

PUT/apisix/admin/global_rules/{id}{…}Creates a Global Rule with the specified id.

DELETE/apisix/admin/global_rules/{id}NULLRemoves the Global Rule with the specified id.

PATCH/apisix/admin/global_rules/{id}{…}Standard PATCH, which modifies the specified attributes of the existing Global Rule, while all other attributes remain unchanged. To delete an attribute, set its value to null. Note that if an attribute is an array, it will be completely replaced.

PATCH/apisix/admin/global_rules/{id}/{path}{…}Subpath PATCH, which specifies the Global Rule attribute to update via {path} and completely replaces that attribute’s data, while all other attributes remain unchanged.

Request Body Parameters

ParameterRequiredDescriptionExample

pluginsTruePlugins that are executed during the request/response cycle. See Plugin for more.

Consumer group

Group of Plugins which can be reused across Consumers.

Consumer group API

Consumer group resource request address: /apisix/admin/consumer_groups/{id}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/consumer_groupsNULLFetches a list of all Consumer groups.

GET/apisix/admin/consumer_groups/{id}NULLFetches specified Consumer group by id.

PUT/apisix/admin/consumer_groups/{id}{…}Creates a new Consumer group with the specified id.

DELETE/apisix/admin/consumer_groups/{id}NULLRemoves the Consumer group with the specified id.

PATCH/apisix/admin/consumer_groups/{id}{…}Standard PATCH, which modifies the specified attributes of the existing Consumer Group, while all other attributes remain unchanged. To delete an attribute, set its value to null. Note that if an attribute is an array, it will be completely replaced.

PATCH/apisix/admin/consumer_groups/{id}/{path}{…}Subpath PATCH, which specifies the Consumer Group attribute to update via {path} and completely replaces that attribute’s data, while all other attributes remain unchanged.

Request Body Parameters

ParameterRequiredDescriptionExample

pluginsTruePlugins that are executed during the request/response cycle. See Plugin for more.

nameFalseIdentifier for the consumer group.premium-tier

descFalseDescription of usage scenarios.customer xxxx

labelsFalseAttributes of the Consumer group specified as key-value pairs.{“version”:“v2”,“build”:“16”,“env”:“production”}

Plugin config

Group of Plugins which can be reused across Routes.

Plugin Config API

Plugin Config resource request address: /apisix/admin/plugin_configs/{id}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/plugin_configsNULLFetches a list of all Plugin configs.

GET/apisix/admin/plugin_configs/{id}NULLFetches specified Plugin config by id.

PUT/apisix/admin/plugin_configs/{id}{…}Creates a new Plugin config with the specified id.

DELETE/apisix/admin/plugin_configs/{id}NULLRemoves the Plugin config with the specified id.

PATCH/apisix/admin/plugin_configs/{id}{…}Standard PATCH, which modifies the specified attributes of the existing Plugin Config, while all other attributes remain unchanged. To delete an attribute, set its value to null. Note that if an attribute is an array, it will be completely replaced.

PATCH/apisix/admin/plugin_configs/{id}/{path}{…}Subpath PATCH, which specifies the Plugin Config attribute to update via {path} and completely replaces that attribute’s data, while all other attributes remain unchanged.

Request Body Parameters

ParameterRequiredDescriptionExample

pluginsTruePlugins that are executed during the request/response cycle. See Plugin for more.

descFalseDescription of usage scenarios.customer xxxx

labelsFalseAttributes of the Plugin config specified as key-value pairs.{“version”:“v2”,“build”:“16”,“env”:“production”}

Plugin Metadata

Plugin Metadata API

Plugin Metadata resource request address: /apisix/admin/plugin_metadata/{plugin_name}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/plugin_metadataNULLFetches a list of all Plugin metadata.

GET/apisix/admin/plugin_metadata/{plugin_name}NULLFetches the metadata of the specified Plugin by plugin_name.

PUT/apisix/admin/plugin_metadata/{plugin_name}{…}Creates metadata for the Plugin specified by the plugin_name.

DELETE/apisix/admin/plugin_metadata/{plugin_name}NULLRemoves metadata for the Plugin specified by the plugin_name.

Request Body Parameters

A JSON object defined according to the metadata_schema of the Plugin ({plugin_name}).

Example Configuration:

curl http://127.0.0.1:9180/apisix/admin/plugin_metadata/example-plugin \

-H "X-API-KEY: $admin_key" -i -X PUT -d '

{

"skey": "val",

"ikey": 1

}'

HTTP/1.1 201 Created

Date: Thu, 26 Dec 2019 04:19:34 GMT

Content-Type: text/plain

Plugin

Plugin API

Plugin resource request address: /apisix/admin/plugins/{plugin_name}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/plugins/listNULLFetches a list of all Plugins.

GET/apisix/admin/plugins/{plugin_name}NULLFetches the specified Plugin by plugin_name.

GET/apisix/admin/plugins?all=trueNULLGet all properties of all plugins.

GET/apisix/admin/plugins?all=true&subsystem=streamNULLGets properties of all Stream plugins.

GET/apisix/admin/plugins?all=true&subsystem=httpNULLGets properties of all HTTP plugins.

PUT/apisix/admin/plugins/reloadNULLReloads the plugin according to the changes made in code

GETapisix/admin/plugins/{plugin_name}?subsystem=streamNULLGets properties of a specified plugin if it is supported in Stream/L4 subsystem.

GETapisix/admin/plugins/{plugin_name}?subsystem=httpNULLGets properties of a specified plugin if it is supported in HTTP/L7 subsystem.

CAUTION

The interface of getting properties of all plugins via /apisix/admin/plugins?all=true will be deprecated soon.

Request Body Parameters

The Plugin ({plugin_name}) of the data structure.

Request Arguments

NameDescriptionDefault

subsystemThe subsystem of the Plugins.http

The plugin can be filtered on subsystem so that the ({plugin_name}) is searched in the subsystem passed through query params.

Example API usage:

curl "http://127.0.0.1:9180/apisix/admin/plugins/list" \

-H 'X-API-KEY: your-admin-key'

["zipkin","request-id",...]

curl "http://127.0.0.1:9180/apisix/admin/plugins/key-auth?subsystem=http" -H 'X-API-KEY: your-admin-key'

{"$comment":"this is a mark for our injected plugin schema","properties":{"header":{"default":"apikey","type":"string"},"hide_credentials":{"default":false,"type":"boolean"},"_meta":{"properties":{"filter":{"type":"array","description":"filter determines whether the plugin needs to be executed at runtime"},"disable":{"type":"boolean"},"error_response":{"oneOf":[{"type":"string"},{"type":"object"}]},"priority":{"type":"integer","description":"priority of plugins by customized order"}},"type":"object"},"query":{"default":"apikey","type":"string"}},"type":"object"}

TIP

You can use the /apisix/admin/plugins?all=true API to get all properties of all plugins. This API will be deprecated soon.

Stream Route

Route used in the Stream Proxy.

Stream Route API

Stream Route resource request address: /apisix/admin/stream_routes/{id}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/stream_routesNULLFetches a list of all configured Stream Routes.

GET/apisix/admin/stream_routes/{id}NULLFetches specified Stream Route by id.

PUT/apisix/admin/stream_routes/{id}{…}Creates a Stream Route with the specified id.

POST/apisix/admin/stream_routes{…}Creates a Stream Route and assigns a random id.

DELETE/apisix/admin/stream_routes/{id}NULLRemoves the Stream Route with the specified id.

Request Body Parameters

ParameterRequiredTypeDescriptionExample

nameFalseAuxiliaryIdentifier for the Stream Route.postgres-proxy

descFalseAuxiliaryDescription of usage scenarios.proxy endpoint for postgresql

labelsFalseMatch RulesAttributes of the Proto specified as key-value pairs.{“version”:“17”,“service”:“user”,“env”:“production”}

upstreamFalseUpstreamConfiguration of the Upstream.

upstream_idFalseUpstreamId of the Upstream service.

service_idFalseStringId of the Service service.

remote_addrFalseIPv4, IPv4 CIDR, IPv6Filters Upstream forwards by matching with client IP.”127.0.0.1” or “127.0.0.1/32” or “::1”

server_addrFalseIPv4, IPv4 CIDR, IPv6Filters Upstream forwards by matching with APISIX Server IP.”127.0.0.1” or “127.0.0.1/32” or “::1”

server_portFalseIntegerFilters Upstream forwards by matching with APISIX Server port.9090

sniFalseHostServer Name Indication.”test.com”

protocol.nameFalseStringName of the protocol proxyed by xRPC framework.”redis”

protocol.confFalseConfigurationProtocol-specific configuration.

To learn more about filtering in stream proxies, check this document.

Secret

Secret means Secrets Management, which could use any secret manager supported, e.g. vault.

Secret API

Secret resource request address: /apisix/admin/secrets/{secretmanager}/{id}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/secretsNULLFetches a list of all secrets.

GET/apisix/admin/secrets/{manager}/{id}NULLFetches specified secrets by id.

PUT/apisix/admin/secrets/{manager}{…}Create new secrets configuration.

DELETE/apisix/admin/secrets/{manager}/{id}NULLRemoves the secrets with the specified id.

PATCH/apisix/admin/secrets/{manager}/{id}{…}Standard PATCH, which modifies the specified attributes of the existing secret, while all other attributes remain unchanged. To delete an attribute, set its value to null.

PATCH/apisix/admin/secrets/{manager}/{id}/{path}{…}Subpath PATCH, which specifies the secret attribute to update via {path} and completely replaces that attribute’s data, while all other attributes remain unchanged.

Request Body Parameters

When Secret Manager is Vault

ParameterRequiredTypeDescriptionExample

uriTrueURIURI of the vault server.

prefixTruestringkey prefix

tokenTruestringvault token.

namespaceFalsestringVault namespace, no default valueadmin

Example Configuration:

{

"uri": "https://localhost/vault",

"prefix": "/apisix/kv",

"token": "343effad"

}

Example API usage:

curl -i http://127.0.0.1:9180/apisix/admin/secrets/vault/test2 \

-H "X-API-KEY: $admin_key" -X PUT -d '

{

"uri": "http://xxx/get",

"prefix" : "apisix",

"token" : "apisix"

}'

HTTP/1.1 200 OK

...

{"key":"\/apisix\/secrets\/vault\/test2","value":{"id":"vault\/test2","token":"apisix","prefix":"apisix","update_time":1669625828,"create_time":1669625828,"uri":"http:\/\/xxx\/get"}}

When Secret Manager is AWS

ParameterRequiredTypeDescription

access_key_idTruestringAWS Access Key ID

secret_access_keyTruestringAWS Secret Access Key

session_tokenFalsestringTemporary access credential information

regionFalsestringAWS Region

endpoint_urlFalseURIAWS Secret Manager URL

Example Configuration:

{

"endpoint_url": "http://127.0.0.1:4566",

"region": "us-east-1",

"access_key_id": "access",

"secret_access_key": "secret",

"session_token": "token"

}

Example API usage:

curl -i http://127.0.0.1:9180/apisix/admin/secrets/aws/test3 \

-H "X-API-KEY: $admin_key" -X PUT -d '

{

"endpoint_url": "http://127.0.0.1:4566",

"region": "us-east-1",

"access_key_id": "access",

"secret_access_key": "secret",

"session_token": "token"

}'

HTTP/1.1 200 OK

...

{"value":{"create_time":1726069970,"endpoint_url":"http://127.0.0.1:4566","region":"us-east-1","access_key_id":"access","secret_access_key":"secret","id":"aws/test3","update_time":1726069970,"session_token":"token"},"key":"/apisix/secrets/aws/test3"}

When Secret Manager is GCP

ParameterRequiredTypeDescriptionExample

auth_configTrueobjectEither auth_config or auth_file must be provided.

auth_config.client_emailTruestringEmail address of the Google Cloud service account.

auth_config.private_keyTruestringPrivate key of the Google Cloud service account.

auth_config.project_idTruestringProject ID in the Google Cloud service account.

auth_config.token_uriFalsestringToken URI of the Google Cloud service account.https://oauth2.googleapis.com/token

auth_config.entries_uriFalsestringThe API access endpoint for the Google Secrets Manager.https://secretmanager.googleapis.com/v1

auth_config.scopeFalsestringAccess scopes of the Google Cloud service account. See OAuth 2.0 Scopes for Google APIshttps://www.googleapis.com/auth/cloud-platform

auth_fileTruestringPath to the Google Cloud service account authentication JSON file. Either auth_config or auth_file must be provided.

ssl_verifyFalsebooleanWhen set to true, enables SSL verification as mentioned in OpenResty docs.true

Example Configuration:

{

"auth_config" : {

"client_email": "email@apisix.iam.gserviceaccount.com",

"private_key": "private_key",

"project_id": "apisix-project",

"token_uri": "https://oauth2.googleapis.com/token",

"entries_uri": "https://secretmanager.googleapis.com/v1",

"scope": ["https://www.googleapis.com/auth/cloud-platform"]

}

}

Example API usage:

curl -i http://127.0.0.1:9180/apisix/admin/secrets/gcp/test4 \

-H "X-API-KEY: $admin_key" -X PUT -d '

{

"auth_config" : {

"client_email": "email@apisix.iam.gserviceaccount.com",

"private_key": "private_key",

"project_id": "apisix-project",

"token_uri": "https://oauth2.googleapis.com/token",

"entries_uri": "https://secretmanager.googleapis.com/v1",

"scope": ["https://www.googleapis.com/auth/cloud-platform"]

}

}'

HTTP/1.1 200 OK

...

{"value":{"id":"gcp/test4","ssl_verify":true,"auth_config":{"token_uri":"https://oauth2.googleapis.com/token","scope":["https://www.googleapis.com/auth/cloud-platform"],"entries_uri":"https://secretmanager.googleapis.com/v1","client_email":"email@apisix.iam.gserviceaccount.com","private_key":"private_key","project_id":"apisix-project"},"create_time":1726070161,"update_time":1726070161},"key":"/apisix/secrets/gcp/test4"}

Response Parameters

Currently, the response is returned from etcd.

Proto

Proto is used to store protocol buffers so that APISIX can communicate in gRPC.

See grpc-transcode plugin doc for more examples.

Proto API

Proto resource request address: /apisix/admin/protos/{id}

Request Methods

MethodRequest URIRequest BodyDescription

GET/apisix/admin/protosNULLList all Protos.

GET/apisix/admin/protos/{id}NULLGet a Proto by id.

PUT/apisix/admin/protos/{id}{…}Create or update a Proto with the given id.

POST/apisix/admin/protos{…}Create a Proto with a random id.

DELETE/apisix/admin/protos/{id}NULLDelete Proto by id.

Request Body Parameters

ParameterRequiredTypeDescriptionExample

contentTrueStringContent of .proto or .pb filesSee here

nameFalseAuxiliaryIdentifier for the Protobuf definition.user-proto

descFalseAuxiliaryDescription of usage scenarios.protobuf for user service

labelsFalseMatch RulesAttributes of the Proto specified as key-value pairs.{“version”:“v2”,“service”:“user”,“env”:“production”}

Schema validation

Check the validity of a configuration against its entity schema. This allows you to test your input before submitting a request to the entity endpoints of the Admin API.

Note that this only performs the schema validation checks, checking that the input configuration is well-formed. Requests to the entity endpoint using the given configuration may still fail due to other reasons, such as invalid foreign key relationships or uniqueness check failures against the contents of the data store.

Schema validation

Schema validation request address: /apisix/admin/schema/validate/{resource}

Request Methods

MethodRequest URIRequest BodyDescription

POST/apisix/admin/schema/validate/{resource}{..resource conf..}Validate the resource configuration against corresponding schema.

Request Body Parameters

200: validate ok.

400: validate failed, with error as response body in JSON format.

Example:

curl http://127.0.0.1:9180/apisix/admin/schema/validate/routes \

-H "X-API-KEY: $admin_key" -X POST -i -d '{

"uri": 1980,

"upstream": {

"scheme": "https",

"type": "roundrobin",

"nodes": {

"nghttp2.org": 1

}

}

}'

HTTP/1.1 400 Bad Request

Date: Mon, 21 Aug 2023 07:37:13 GMT

Content-Type: application/json

Transfer-Encoding: chunked

Connection: keep-alive

Server: APISIX/3.4.0

Access-Control-Allow-Origin: *

Access-Control-Allow-Credentials: true

Access-Control-Expose-Headers: *

Access-Control-Max-Age: 3600

{"error_msg":"property \"uri\" validation failed: wrong type: expected string, got number"}

Edit this page on GitHub Report an issue

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
