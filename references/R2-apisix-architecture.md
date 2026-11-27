# 参考资料

- 来源：https://apisix.apache.org/docs/apisix/architecture-design/apisix/
- 获取日期：2026-11-27
- 说明：以下为页面可读文本快照，可能包含导航或页脚。

---

Architecture | Apache APISIX

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

Architecture

APISIX is built on NGINX and ngx_lua, using LuaJIT for request-time logic. See Why Apache APISIX chose NGINX and Lua to build an API gateway.

At runtime, APISIX has two main parts:

The APISIX core and plugin runtimes, which match requests, select upstreams, and execute plugins.

Built-in plugins for authentication, traffic control, observability, transformations, and other opt-in policies.

The APISIX core handles Route matching, load balancing, service discovery, and configuration updates. Lua plugins run in the APISIX worker process. External plugin runners support selected additional languages, while the WebAssembly plugin runtime remains experimental.

Built-in plugins are written in Lua and are opt-in: a plugin affects traffic only when it is configured on a matching Route, Service, Consumer, or other supported scope. Browse the Plugin Hub for the available plugins and their configuration.

Configuration and deployment modes

APISIX supports multiple control-plane and data-plane topologies:

Traditional mode: One APISIX role handles traffic and exposes the Admin API, with configuration stored in etcd.

Decoupled mode: Separate control-plane and data-plane roles share configuration through etcd. The data plane does not expose the Admin API.

Standalone file-driven mode: A data-plane role loads full configuration from a local YAML or JSON file and does not use etcd as its configuration center.

Standalone API-driven mode: A traditional role accepts full, in-memory configuration through the dedicated Standalone Admin API. This path is designed for integrations such as the APISIX Ingress Controller and ADC; do not use it directly without understanding its full-replacement and versioning behavior.

Choose the topology before designing network access, configuration delivery, and failure handling. See Deployment modes for the current role and configuration-provider settings.

Request handling process

The diagram below shows how APISIX handles an incoming request and applies corresponding Plugins:

Plugin hierarchy

The chart below shows the order in which different types of Plugin are applied to a request:

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
