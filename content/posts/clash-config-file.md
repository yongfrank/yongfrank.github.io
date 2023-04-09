---
title: "Clash & Config File"
date: 2023-03-20T10:26:07+08:00
---

## infrastructure

* port
* hosts
* dns
* mode: Rule
* proxies: proxy name, server, Password
* proxies-groups: name, type, proxies
* rules: Match RULE, DOMAIN, Request Rule

```yaml
port: 7890
socks-port: 7891
redir-port: 7892
mixed-port: 7893
allow-lan: false
mode: Rule
log-level: info
ipv6: false
hosts: 
    services.googleapis.cn: 142.250.196.131
    www.google.cn: 142.250.196.131
external-controller: 0.0.0.0:9090
clash-for-android:
  append-system-dns: false
profile:
  tracing: true
experimental:
  sniff-tls-sni: true
dns:
    enable: true
    listen: 127.0.0.1:8853
    default-nameserver:
        - 223.5.5.5
        - 8.8.4.4
    ipv6: false
    enhanced-mode: fake-ip
    fake-ip-filter:
        {- "*.lan"}
    nameserver:
        {- "114.114.114.114"}
    fallback:
        {- https://1.0.0.1/dns-query}
    fallback-filter:
        geoip: false
        ipcidr:
            {- 240.0.0.0/4}
        domain:
            {- +.facebook.com}
proxies:
    {
        - 
        {
            name: "Hong Kong",
            server: abc.top,
            port: 200001,
            type: trojan,
            password: 123456,
            sni: abcde.top,
            skip-cert-verify: true,
            udp: true
        }
        -
        {

        }
    }
proxy-groups:
    -   name: Rule
        type: select
        proxies:
            - Hong Kong
            -
    -   name: YouTuBe
        type: select
        proxies:
            - Hong Kong
            -
rules:
    - DOMAIN-SUFFIX, local, DIRECT
    - MATCH, Rule
```

## Help Desk

* [Help Desk for WestData](https://gleaming-fedora-39d.notion.site/WestData-280992c869574200b910ced779f93e9b)
* [Subscription Converter](https://api.nameless13.com/)

## Tutorials

* [ClashX Pro 使用教程 最好用的Mac苹果电脑科学上网工具](https://heaid.top/index.php/archives/34/)
* [Mac 电脑使用 ClashX Pro 作为网关旁路由](https://qust.me/post/clashxProMac/)
