# 参考资料

- 来源：https://apisix.apache.org/docs/apisix/plugins/key-auth/
- 获取日期：2026-11-27
- 说明：以下为页面可读文本快照，可能包含导航或页脚。

---

key-auth | Apache APISIX

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

key-auth

Description

The key-auth Plugin supports the use of an authentication key as a mechanism for clients to authenticate themselves before accessing Upstream resources.

To use the Plugin, you would configure authentication keys on Consumers and enable the Plugin on Routes or Services. The key can be included in the request URL query string or request header. APISIX will then verify the key to determine if a request should be allowed or denied to access Upstream resources.

When a Consumer is successfully authenticated, APISIX adds additional headers, such as X-Consumer-Username, X-Credential-Identifier, and other Consumer custom headers if configured, to the request, before proxying it to the Upstream service. The Upstream service will be able to differentiate between consumers and implement additional logic as needed. If any of these values is not available, the corresponding header will not be added.

Attributes

For Consumer/Credential:

NameTypeRequiredDefaultValid valuesDescription

keystringTrueA unique key that identifies the Credential for a Consumer. When apisix.data_encryption.enable_encrypt_fields is enabled and the configuration is stored in etcd, the key is encrypted with AES before storage. You can also store it in an environment variable and reference it using the $env:// prefix, or in a secret manager such as HashiCorp Vault’s KV secrets engine, and reference it using the $secret:// prefix.

For Route:

NameTypeRequiredDefaultValid valuesDescription

headerstringFalseapikeyThe header to get the key from.

querystringFalseapikeyThe query string to get the key from. Lower priority than header.

hide_credentialsbooleanFalsefalseIf true, do not pass the header or query string with key to Upstream services.

anonymous_consumerstringFalseAnonymous Consumer name. If configured, allow anonymous users to bypass the authentication.

realmstringFalsekeyRealm in the WWW-Authenticate response header returned with a 401 Unauthorized response due to authentication failure. Available in Apache APISIX version 3.15.0 and later.

Examples

The examples below demonstrate how you can work with the key-auth Plugin for different scenarios.

NOTE

admin_key=$(yq '.deployment.admin.admin_key[0].key' conf/config.yaml | sed 's/"//g')

Implement Key Authentication on Route

The following example demonstrates how to implement key authentication on a Route and include the key in the request header.

admin-api

Create a Consumer jack:

curl "http://127.0.0.1:9180/apisix/admin/consumers" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"username": "jack"

}'

Create key-auth Credential for the Consumer:

curl "http://127.0.0.1:9180/apisix/admin/consumers/jack/credentials" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "cred-jack-key-auth",

"plugins": {

"key-auth": {

"key": "jack-key"

}

}

}'

Create a Route with key-auth:

curl "http://127.0.0.1:9180/apisix/admin/routes" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "key-auth-route",

"uri": "/anything",

"plugins": {

"key-auth": {}

},

"upstream": {

"type": "roundrobin",

"nodes": {

"httpbin.org:80": 1

}

}

}'

adc

Create a Consumer with key-auth Credential and a Route with key-auth Plugin configured:

adc.yaml

consumers:

- username: jack

credentials:

- name: key-auth

type: key-auth

config:

key: jack-key

services:

- name: key-auth-service

routes:

- name: key-auth-route

uris:

- /anything

plugins:

key-auth: {}

upstream:

type: roundrobin

nodes:

- host: httpbin.org

port: 80

weight: 1

Synchronize the configuration to the gateway:

adc sync -f adc.yaml

aic

Create a Consumer with key-auth Credential and a Route with key-auth Plugin configured:

gateway-api

key-auth-ic.yaml

apiVersion: apisix.apache.org/v1alpha1

kind: Consumer

metadata:

namespace: aic

name: jack

spec:

gatewayRef:

name: apisix

credentials:

- type: key-auth

name: primary-cred

config:

key: jack-key

---

apiVersion: v1

kind: Service

metadata:

namespace: aic

name: httpbin-external-domain

spec:

type: ExternalName

externalName: httpbin.org

---

apiVersion: apisix.apache.org/v1alpha1

kind: PluginConfig

metadata:

namespace: aic

name: key-auth-plugin-config

spec:

plugins:

- name: key-auth

config:

_meta:

disable: false

---

apiVersion: gateway.networking.k8s.io/v1

kind: HTTPRoute

metadata:

namespace: aic

name: key-auth-route

spec:

parentRefs:

- name: apisix

rules:

- matches:

- path:

type: Exact

value: /anything

filters:

- type: ExtensionRef

extensionRef:

group: apisix.apache.org

kind: PluginConfig

name: key-auth-plugin-config

backendRefs:

- name: httpbin-external-domain

port: 80

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

apisix-ingress-controller

key-auth-ic.yaml

apiVersion: apisix.apache.org/v2

kind: ApisixConsumer

metadata:

namespace: aic

name: jack

spec:

ingressClassName: apisix

authParameter:

keyAuth:

value:

key: jack-key

---

apiVersion: apisix.apache.org/v2

kind: ApisixUpstream

metadata:

namespace: aic

name: httpbin-external-domain

spec:

ingressClassName: apisix

externalNodes:

- type: Domain

name: httpbin.org

---

apiVersion: apisix.apache.org/v2

kind: ApisixRoute

metadata:

namespace: aic

name: key-auth-route

spec:

ingressClassName: apisix

http:

- name: key-auth-route

match:

paths:

- /anything

upstreams:

- name: httpbin-external-domain

plugins:

- name: key-auth

enable: true

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

Verify with a Valid Key

Send a request to the Route with the valid key:

curl -i "http://127.0.0.1:9080/anything" -H 'apikey: jack-key'

You should receive an HTTP/1.1 200 OK response.

Verify with an Invalid Key

Send a request with an invalid key:

curl -i "http://127.0.0.1:9080/anything" -H 'apikey: wrong-key'

You should see an HTTP/1.1 401 Unauthorized response with the following:

{"message":"Invalid API key in request"}

Verify without a Key

Send a request to the Route without a key:

curl -i "http://127.0.0.1:9080/anything"

You should see an HTTP/1.1 401 Unauthorized response with the following:

{"message":"Missing API key found in request"}

Hide Authentication Information From Upstream

The following example first demonstrates the default behavior, where the authentication key is forwarded to the Upstream services, and then shows how to prevent the key from being sent by configuring hide_credentials. Forwarding the authentication key to Upstream services might lead to security risks in some circumstances.

admin-api

Create a Consumer jack:

curl "http://127.0.0.1:9180/apisix/admin/consumers" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"username": "jack"

}'

Create key-auth Credential for the Consumer:

curl "http://127.0.0.1:9180/apisix/admin/consumers/jack/credentials" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "cred-jack-key-auth",

"plugins": {

"key-auth": {

"key": "jack-key"

}

}

}'

Without Hiding Credentials

Create a Route with key-auth and configure hide_credentials to false, which is the default configuration:

curl "http://127.0.0.1:9180/apisix/admin/routes" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "key-auth-route",

"uri": "/anything",

"plugins": {

"key-auth": {

"hide_credentials": false

}

},

"upstream": {

"type": "roundrobin",

"nodes": {

"httpbin.org:80": 1

}

}

}'

adc

Create a Consumer with key-auth Credential and a Route with key-auth Plugin configured:

adc.yaml

consumers:

- username: jack

credentials:

- name: key-auth

type: key-auth

config:

key: jack-key

services:

- name: key-auth-service

routes:

- name: key-auth-route

uris:

- /anything

plugins:

key-auth:

hide_credentials: false

upstream:

type: roundrobin

nodes:

- host: httpbin.org

port: 80

weight: 1

Synchronize the configuration to the gateway:

adc sync -f adc.yaml

aic

Create a Consumer with key-auth Credential and a Route with key-auth Plugin configured:

gateway-api

key-auth-ic.yaml

apiVersion: apisix.apache.org/v1alpha1

kind: Consumer

metadata:

namespace: aic

name: jack

spec:

gatewayRef:

name: apisix

credentials:

- type: key-auth

name: primary-cred

config:

key: jack-key

---

apiVersion: v1

kind: Service

metadata:

namespace: aic

name: httpbin-external-domain

spec:

type: ExternalName

externalName: httpbin.org

---

apiVersion: apisix.apache.org/v1alpha1

kind: PluginConfig

metadata:

namespace: aic

name: key-auth-plugin-config

spec:

plugins:

- name: key-auth

config:

_meta:

disable: false

hide_credentials: false

---

apiVersion: gateway.networking.k8s.io/v1

kind: HTTPRoute

metadata:

namespace: aic

name: key-auth-route

spec:

parentRefs:

- name: apisix

rules:

- matches:

- path:

type: Exact

value: /anything

filters:

- type: ExtensionRef

extensionRef:

group: apisix.apache.org

kind: PluginConfig

name: key-auth-plugin-config

backendRefs:

- name: httpbin-external-domain

port: 80

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

apisix-ingress-controller

key-auth-ic.yaml

apiVersion: apisix.apache.org/v2

kind: ApisixConsumer

metadata:

namespace: aic

name: jack

spec:

ingressClassName: apisix

authParameter:

keyAuth:

value:

key: jack-key

---

apiVersion: apisix.apache.org/v2

kind: ApisixUpstream

metadata:

namespace: aic

name: httpbin-external-domain

spec:

ingressClassName: apisix

externalNodes:

- type: Domain

name: httpbin.org

---

apiVersion: apisix.apache.org/v2

kind: ApisixRoute

metadata:

namespace: aic

name: key-auth-route

spec:

ingressClassName: apisix

http:

- name: key-auth-route

match:

paths:

- /anything

upstreams:

- name: httpbin-external-domain

plugins:

- name: key-auth

enable: true

config:

hide_credentials: false

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

Send a request with the valid key:

curl -i "http://127.0.0.1:9080/anything?apikey=jack-key"

You should see an HTTP/1.1 200 OK response with the following:

{

"args": {

"apikey": "jack-key"

},

"data": "",

"files": {},

"form": {},

"headers": {

"Accept": "*/*",

"Host": "127.0.0.1",

"User-Agent": "curl/8.2.1",

"X-Consumer-Username": "jack",

"X-Credential-Identifier": "cred-jack-key-auth",

"X-Amzn-Trace-Id": "Root=1-6502d8a5-2194962a67aa21dd33f94bb2",

"X-Forwarded-Host": "127.0.0.1"

},

"json": null,

"method": "GET",

"origin": "127.0.0.1, 103.248.35.179",

"url": "http://127.0.0.1/anything?apikey=jack-key"

}

Note that the Credential jack-key is visible to the Upstream service.

Hide Credentials

admin-api

Update the Plugin’s hide_credentials to true:

curl "http://127.0.0.1:9180/apisix/admin/routes/key-auth-route" -X PATCH \

-H "X-API-KEY: ${admin_key}" \

-d '{

"plugins": {

"key-auth": {

"hide_credentials": true

}

}

}'

adc

Update the Route configuration:

adc.yaml

services:

- name: key-auth-service

routes:

- name: key-auth-route

uris:

- /anything

plugins:

key-auth:

hide_credentials: true

upstream:

type: roundrobin

nodes:

- host: httpbin.org

port: 80

weight: 1

Synchronize the configuration to the gateway:

adc sync -f adc.yaml

aic

gateway-api

Update the PluginConfig to set hide_credentials to true:

key-auth-ic.yaml

# other configs

# ---

apiVersion: apisix.apache.org/v1alpha1

kind: PluginConfig

metadata:

namespace: aic

name: key-auth-plugin-config

spec:

plugins:

- name: key-auth

config:

_meta:

disable: false

hide_credentials: true

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

apisix-ingress-controller

Update the ApisixRoute to set hide_credentials to true:

key-auth-ic.yaml

# other configs

# ---

apiVersion: apisix.apache.org/v2

kind: ApisixRoute

metadata:

namespace: aic

name: key-auth-route

spec:

ingressClassName: apisix

http:

- name: key-auth-route

match:

paths:

- /anything

upstreams:

- name: httpbin-external-domain

plugins:

- name: key-auth

enable: true

config:

hide_credentials: true

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

Send a request with the valid key:

curl -i "http://127.0.0.1:9080/anything?apikey=jack-key"

You should see an HTTP/1.1 200 OK response with the following:

{

"args": {},

"data": "",

"files": {},

"form": {},

"headers": {

"Accept": "*/*",

"Host": "127.0.0.1",

"User-Agent": "curl/8.2.1",

"X-Consumer-Username": "jack",

"X-Credential-Identifier": "cred-jack-key-auth",

"X-Amzn-Trace-Id": "Root=1-6502d85c-16f34dbb5629a5960183e803",

"X-Forwarded-Host": "127.0.0.1"

},

"json": null,

"method": "GET",

"origin": "127.0.0.1, 103.248.35.179",

"url": "http://127.0.0.1/anything"

}

Note that the Credential jack-key is no longer visible to the Upstream service.

Demonstrate Priority of Keys in Header and Query

The following example demonstrates how to implement key authentication by consumers on a Route and customize the URL parameter that should include the key. The example also shows that when the API key is configured in both the header and the query string, the request header has a higher priority.

admin-api

Create a Consumer jack:

curl "http://127.0.0.1:9180/apisix/admin/consumers" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"username": "jack"

}'

Create key-auth Credential for the Consumer:

curl "http://127.0.0.1:9180/apisix/admin/consumers/jack/credentials" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "cred-jack-key-auth",

"plugins": {

"key-auth": {

"key": "jack-key"

}

}

}'

Create a Route with key-auth:

curl "http://127.0.0.1:9180/apisix/admin/routes" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "key-auth-route",

"uri": "/anything",

"plugins": {

"key-auth": {

"query": "auth"

}

},

"upstream": {

"type": "roundrobin",

"nodes": {

"httpbin.org:80": 1

}

}

}'

adc

Create a Consumer with key-auth Credential and a Route with key-auth Plugin configured:

adc.yaml

consumers:

- username: jack

credentials:

- name: key-auth

type: key-auth

config:

key: jack-key

services:

- name: key-auth-service

routes:

- name: key-auth-route

uris:

- /anything

plugins:

key-auth:

query: auth

upstream:

type: roundrobin

nodes:

- host: httpbin.org

port: 80

weight: 1

Synchronize the configuration to the gateway:

adc sync -f adc.yaml

aic

Create a Consumer with key-auth Credential and a Route with key-auth Plugin configured:

gateway-api

key-auth-ic.yaml

apiVersion: apisix.apache.org/v1alpha1

kind: Consumer

metadata:

namespace: aic

name: jack

spec:

gatewayRef:

name: apisix

credentials:

- type: key-auth

name: primary-cred

config:

key: jack-key

---

apiVersion: v1

kind: Service

metadata:

namespace: aic

name: httpbin-external-domain

spec:

type: ExternalName

externalName: httpbin.org

---

apiVersion: apisix.apache.org/v1alpha1

kind: PluginConfig

metadata:

namespace: aic

name: key-auth-plugin-config

spec:

plugins:

- name: key-auth

config:

_meta:

disable: false

query: auth

---

apiVersion: gateway.networking.k8s.io/v1

kind: HTTPRoute

metadata:

namespace: aic

name: key-auth-route

spec:

parentRefs:

- name: apisix

rules:

- matches:

- path:

type: Exact

value: /anything

filters:

- type: ExtensionRef

extensionRef:

group: apisix.apache.org

kind: PluginConfig

name: key-auth-plugin-config

backendRefs:

- name: httpbin-external-domain

port: 80

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

apisix-ingress-controller

key-auth-ic.yaml

apiVersion: apisix.apache.org/v2

kind: ApisixConsumer

metadata:

namespace: aic

name: jack

spec:

ingressClassName: apisix

authParameter:

keyAuth:

value:

key: jack-key

---

apiVersion: apisix.apache.org/v2

kind: ApisixUpstream

metadata:

namespace: aic

name: httpbin-external-domain

spec:

ingressClassName: apisix

externalNodes:

- type: Domain

name: httpbin.org

---

apiVersion: apisix.apache.org/v2

kind: ApisixRoute

metadata:

namespace: aic

name: key-auth-route

spec:

ingressClassName: apisix

http:

- name: key-auth-route

match:

paths:

- /anything

upstreams:

- name: httpbin-external-domain

plugins:

- name: key-auth

enable: true

config:

query: auth

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

Verify with a Valid Key

Send a request to the Route with the valid key:

curl -i "http://127.0.0.1:9080/anything?auth=jack-key"

You should receive an HTTP/1.1 200 OK response.

Verify with an Invalid Key

Send a request with an invalid key:

curl -i "http://127.0.0.1:9080/anything?auth=wrong-key"

You should see an HTTP/1.1 401 Unauthorized response with the following:

{"message":"Invalid API key in request"}

Verify with a Valid Key in Query String

However, if you include the valid key in header with the invalid key still in the URL query string:

curl -i "http://127.0.0.1:9080/anything?auth=wrong-key" -H 'apikey: jack-key'

You should see an HTTP/1.1 200 OK response. This shows that the key included in the header always has a higher priority.

Add Consumer Custom ID to Header

The following example demonstrates how you can attach a Consumer custom ID to authenticated request in the X-Consumer-Custom-Id header, which can be used to implement additional logic as needed.

admin-api

Create a Consumer jack with a custom ID label:

curl "http://127.0.0.1:9180/apisix/admin/consumers" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"username": "jack",

"labels": {

"custom_id": "495aec6a"

}

}'

Create key-auth Credential for the Consumer:

curl "http://127.0.0.1:9180/apisix/admin/consumers/jack/credentials" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "cred-jack-key-auth",

"plugins": {

"key-auth": {

"key": "jack-key"

}

}

}'

Create a Route with key-auth:

curl "http://127.0.0.1:9180/apisix/admin/routes" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "key-auth-route",

"uri": "/anything",

"plugins": {

"key-auth": {}

},

"upstream": {

"type": "roundrobin",

"nodes": {

"httpbin.org:80": 1

}

}

}'

adc

Create a Consumer with key-auth Credential and a Route with key-auth Plugin enabled:

adc.yaml

consumers:

- username: jack

labels:

custom_id: "495aec6a"

credentials:

- name: key-auth

type: key-auth

config:

key: jack-key

services:

- name: key-auth-service

routes:

- name: key-auth-route

uris:

- /anything

plugins:

key-auth: {}

upstream:

type: roundrobin

nodes:

- host: httpbin.org

port: 80

weight: 1

Synchronize the configuration to the gateway:

adc sync -f adc.yaml

aic

Create a Consumer with key-auth Credential and a Route with key-auth Plugin enabled:

gateway-api

key-auth-ic.yaml

apiVersion: apisix.apache.org/v1alpha1

kind: Consumer

metadata:

namespace: aic

name: jack

labels:

custom_id: "495aec6a"

spec:

gatewayRef:

name: apisix

credentials:

- type: key-auth

name: primary-key

config:

key: jack-key

---

apiVersion: v1

kind: Service

metadata:

namespace: aic

name: httpbin-external-domain

spec:

type: ExternalName

externalName: httpbin.org

---

apiVersion: apisix.apache.org/v1alpha1

kind: PluginConfig

metadata:

namespace: aic

name: key-auth-plugin-config

spec:

plugins:

- name: key-auth

config:

_meta:

disable: false

---

apiVersion: gateway.networking.k8s.io/v1

kind: HTTPRoute

metadata:

namespace: aic

name: key-auth-route

spec:

parentRefs:

- name: apisix

rules:

- matches:

- path:

type: Exact

value: /anything

filters:

- type: ExtensionRef

extensionRef:

group: apisix.apache.org

kind: PluginConfig

name: key-auth-plugin-config

backendRefs:

- name: httpbin-external-domain

port: 80

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

apisix-crd

key-auth-ic.yaml

apiVersion: apisix.apache.org/v2

kind: ApisixConsumer

metadata:

namespace: aic

name: jack

labels:

custom_id: "495aec6a"

spec:

ingressClassName: apisix

authParameter:

keyAuth:

value:

key: jack-key

---

apiVersion: apisix.apache.org/v2

kind: ApisixUpstream

metadata:

namespace: aic

name: httpbin-external-domain

spec:

ingressClassName: apisix

externalNodes:

- type: Domain

name: httpbin.org

---

apiVersion: apisix.apache.org/v2

kind: ApisixRoute

metadata:

namespace: aic

name: key-auth-route

spec:

ingressClassName: apisix

http:

- name: key-auth-route

match:

paths:

- /anything

upstreams:

- name: httpbin-external-domain

plugins:

- name: key-auth

enable: true

config:

_meta:

disable: false

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

To verify, send a request to the Route with the valid key:

curl -i "http://127.0.0.1:9080/anything?apikey=jack-key"

You should see an HTTP/1.1 200 OK response similar to the following:

{

"args": {

"apikey": "jack-key"

},

"data": "",

"files": {},

"form": {},

"headers": {

"Accept": "*/*",

"Host": "127.0.0.1",

"User-Agent": "curl/8.6.0",

"X-Amzn-Trace-Id": "Root=1-66ea8d64-33df89052ae198a706e18c2a",

"X-Consumer-Username": "aic_jack",

"X-Credential-Identifier": "cred-jack-key-auth",

"X-Consumer-Custom-Id": "495aec6a",

"X-Forwarded-Host": "127.0.0.1"

},

"json": null,

"method": "GET",

"origin": "192.168.65.1, 205.198.122.37",

"url": "http://127.0.0.1/anything?apikey=jack-key"

}

Rate Limit with Anonymous Consumer

The following example demonstrates how you can configure different rate limiting policies by regular and anonymous consumers, where the anonymous Consumer does not need to authenticate and has less quota.

admin-api

Create a regular Consumer jack and configure the limit-count Plugin to allow for a quota of 3 within a 30-second window:

curl "http://127.0.0.1:9180/apisix/admin/consumers" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"username": "jack",

"plugins": {

"limit-count": {

"count": 3,

"time_window": 30,

"rejected_code": 429,

"policy": "local"

}

}

}'

Create the key-auth Credential for the Consumer jack:

curl "http://127.0.0.1:9180/apisix/admin/consumers/jack/credentials" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "cred-jack-key-auth",

"plugins": {

"key-auth": {

"key": "jack-key"

}

}

}'

Create an anonymous user anonymous and configure the limit-count Plugin to allow for a quota of 1 within a 30-second window:

curl "http://127.0.0.1:9180/apisix/admin/consumers" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"username": "anonymous",

"plugins": {

"limit-count": {

"count": 1,

"time_window": 30,

"rejected_code": 429,

"policy": "local"

}

}

}'

Create a Route and configure the key-auth Plugin to accept anonymous Consumer anonymous from bypassing the authentication:

curl "http://127.0.0.1:9180/apisix/admin/routes" -X PUT \

-H "X-API-KEY: ${admin_key}" \

-d '{

"id": "key-auth-route",

"uri": "/anything",

"plugins": {

"key-auth": {

"anonymous_consumer": "anonymous"

}

},

"upstream": {

"type": "roundrobin",

"nodes": {

"httpbin.org:80": 1

}

}

}'

adc

Configure Consumers with different rate limits and a Route that accepts anonymous users:

adc.yaml

consumers:

- username: jack

plugins:

limit-count:

count: 3

time_window: 30

rejected_code: 429

policy: local

credentials:

- name: key-auth

type: key-auth

config:

key: jack-key

- username: anonymous

plugins:

limit-count:

count: 1

time_window: 30

rejected_code: 429

policy: local

services:

- name: anonymous-rate-limit-service

routes:

- name: key-auth-route

uris:

- /anything

plugins:

key-auth:

anonymous_consumer: anonymous

upstream:

type: roundrobin

nodes:

- host: httpbin.org

port: 80

weight: 1

Synchronize the configuration to the gateway:

adc sync -f adc.yaml

aic

gateway-api

Configure Consumers with different rate limits and a Route that accepts anonymous users:

key-auth-ic.yaml

apiVersion: apisix.apache.org/v1alpha1

kind: Consumer

metadata:

namespace: aic

name: jack

spec:

gatewayRef:

name: apisix

credentials:

- type: key-auth

name: primary-key

config:

key: jack-key

plugins:

- name: limit-count

config:

count: 3

time_window: 30

rejected_code: 429

policy: local

---

apiVersion: apisix.apache.org/v1alpha1

kind: Consumer

metadata:

namespace: aic

name: anonymous

spec:

gatewayRef:

name: apisix

plugins:

- name: limit-count

config:

count: 1

time_window: 30

rejected_code: 429

policy: local

---

apiVersion: v1

kind: Service

metadata:

namespace: aic

name: httpbin-external-domain

spec:

type: ExternalName

externalName: httpbin.org

---

apiVersion: apisix.apache.org/v1alpha1

kind: PluginConfig

metadata:

namespace: aic

name: key-auth-plugin-config

spec:

plugins:

- name: key-auth

config:

anonymous_consumer: aic_anonymous # namespace_consumername

---

apiVersion: gateway.networking.k8s.io/v1

kind: HTTPRoute

metadata:

namespace: aic

name: key-auth-route

spec:

parentRefs:

- name: apisix

rules:

- matches:

- path:

type: Exact

value: /anything

filters:

- type: ExtensionRef

extensionRef:

group: apisix.apache.org

kind: PluginConfig

name: key-auth-plugin-config

backendRefs:

- name: httpbin-external-domain

port: 80

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

apisix-ingress-controller

Configure Consumers with different rate limits and a Route that accepts anonymous users:

key-auth-ic.yaml

apiVersion: apisix.apache.org/v2

kind: ApisixConsumer

metadata:

namespace: aic

name: jack

spec:

ingressClassName: apisix

authParameter:

keyAuth:

value:

key: jack-key

plugins:

- name: limit-count

enable: true

config:

count: 3

time_window: 30

rejected_code: 429

policy: local

---

apiVersion: apisix.apache.org/v2

kind: ApisixConsumer

metadata:

namespace: aic

name: anonymous

spec:

ingressClassName: apisix

plugins:

- name: limit-count

enable: true

config:

count: 1

time_window: 30

rejected_code: 429

policy: local

---

apiVersion: apisix.apache.org/v2

kind: ApisixUpstream

metadata:

namespace: aic

name: httpbin-external-domain

spec:

ingressClassName: apisix

externalNodes:

- type: Domain

name: httpbin.org

---

apiVersion: apisix.apache.org/v2

kind: ApisixRoute

metadata:

namespace: aic

name: key-auth-route

spec:

ingressClassName: apisix

http:

- name: key-auth-route

match:

paths:

- /anything

upstreams:

- name: httpbin-external-domain

plugins:

- name: key-auth

enable: true

config:

anonymous_consumer: aic_anonymous

Apply the configuration to your cluster:

kubectl apply -f key-auth-ic.yaml

To verify, send five consecutive requests with jack’s key:

resp=$(seq 5 | xargs -I{} curl "http://127.0.0.1:9080/anything" -H 'apikey: jack-key' -o /dev/null -s -w "%{http_code}\n") && \

count_200=$(echo "$resp" | grep "200" | wc -l) && \

count_429=$(echo "$resp" | grep "429" | wc -l) && \

echo "200": $count_200, "429": $count_429

You should see the following response, showing that out of the 5 requests, 3 requests were successful (status code 200) while the others were rejected (status code 429).

200: 3, 429: 2

Send five anonymous requests:

resp=$(seq 5 | xargs -I{} curl "http://127.0.0.1:9080/anything" -o /dev/null -s -w "%{http_code}\n") && \

count_200=$(echo "$resp" | grep "200" | wc -l) && \

count_429=$(echo "$resp" | grep "429" | wc -l) && \

echo "200": $count_200, "429": $count_429

You should see the following response, showing that only one request was successful:

200: 1, 429: 4

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
