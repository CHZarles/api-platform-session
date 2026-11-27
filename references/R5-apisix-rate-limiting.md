# 参考资料

- 来源：https://apisix.apache.org/docs/apisix/getting-started/rate-limiting/
- 获取日期：2026-11-27
- 说明：以下为页面可读文本快照，可能包含导航或页脚。

---

Rate Limiting | Apache APISIX

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

Rate Limiting

The Getting Started tutorials are contributed by API7.ai.

APISIX is a unified control point, managing the ingress and egress of APIs and microservices traffic. In addition to the legitimate client requests, these requests may also include unwanted traffic generated by web crawlers as well as cyber attacks, such as DDoS.

APISIX offers rate limiting capabilities to protect APIs and microservices by limiting the number of requests sent to upstream services in a given period of time. The count of requests is done efficiently in memory with low latency and high performance.

In this tutorial, you will enable the limit-count plugin to set a rate limiting constraint on the incoming traffic.

Prerequisite(s)

Complete the Get APISIX step to install APISIX first.

Complete the Configure Routes step.

Enable Rate Limiting

The following route getting-started-ip is inherited from Configure Routes. You only need to use the PATCH method to add the limit-count plugin to the route:

curl -i "http://127.0.0.1:9180/apisix/admin/routes/getting-started-ip" -X PATCH -d '

{

"plugins": {

"limit-count": {

"count": 2,

"time_window": 10,

"rejected_code": 503

}

}

}'

You will receive an HTTP/1.1 201 Created response if the plugin was added successfully. The above configuration limits the incoming requests to a maximum of 2 requests within 10 seconds.

Validate

Let’s generate 100 simultaneous requests to see the rate limiting plugin in effect.

count=$(seq 100 | xargs -I {} curl "http://127.0.0.1:9080/ip" -I -sL | grep "503" | wc -l); echo \"200\": $((100 - $count)), \"503\": $count

The results are as expected: out of the 100 requests, 2 requests were sent successfully (status code 200) while the others were rejected (status code 503).

"200": 2, "503": 98

Disable Rate Limiting

Disable rate limiting by setting the _meta.disable parameter to true:

curl -i "http://127.0.0.1:9180/apisix/admin/routes/getting-started-ip" -X PATCH -d '

{

"plugins": {

"limit-count": {

"_meta": {

"disable": true

}

}

}

}'

Validate

Let’s generate 100 requests again to validate if it is disabled:

count=$(seq 100 | xargs -i curl "http://127.0.0.1:9080/ip" -I -sL | grep "503" | wc -l); echo \"200\": $((100 - $count)), \"503\": $count

The results below show that all of the requests were sent successfully:

"200": 100, "503": 0

More

You can use the APISIX variables to configure fined matching rules of rate limiting, such as $host and $uri. In addition, APISIX also supports rate limiting at the cluster level using Redis.

What’s Next

Congratulations! You have learned how to configure rate limiting and completed the Getting Started tutorials.

You can continue to explore other documentations to customize APISIX and meet your production needs.

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
