# Piggy Metrics (Kubernetes) - Config service

[![CircleCI](https://circleci.com/gh/afermon/PiggyMetrics-config-service.svg?style=svg)](https://circleci.com/gh/afermon/PiggyMetrics-config-service) [![codecov](https://codecov.io/gh/afermon/PiggyMetrics-config-service/branch/master/graph/badge.svg)](https://codecov.io/gh/afermon/PiggyMetrics-config-service) [![GitHub license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/afermon/PiggyMetrics-config-service/blob/master/LICENCE)

[Spring Cloud Config](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html) is horizontally scalable centralized configuration service for distributed systems. It uses a pluggable repository layer that currently supports local storage, Git, and Subversion. 

In this project, I use `native profile`, which simply loads config files from the local classpath. You can see `shared` directory in [Config service resources](https://github.com/afermon/PiggyMetrics-config-service/tree/master/src/main/resources). Now, when Notification-service requests it's configuration, Config service responses with `shared/notification-service.yml` and `shared/application.yml` (which is shared between all client applications).

##### Client side usage
Just build Spring Boot application with `spring-cloud-starter-config` dependency, autoconfiguration will do the rest.

Now you don't need any embedded properties in your application. Just provide `bootstrap.yml` with application name and Config service url:
```yml
spring:
  application:
    name: notification-service
  cloud:
    config:
      uri: http://config:8888
      fail-fast: true
```

##### With Spring Cloud Config, you can change app configuration dynamically. 
For example, [EmailService bean](https://github.com/afermon/PiggyMetrics-notification-service/blob/master/src/main/java/com/piggymetrics/notification/service/EmailServiceImpl.java) was annotated with `@RefreshScope`. That means, you can change e-mail text and subject without rebuild and restart Notification service application.

First, change required properties in Config server. Then, perform refresh request to Notification service:
`curl -H "Authorization: Bearer #token#" -XPOST http://127.0.0.1:8000/notifications/refresh`

Also, you could use Repository [webhooks to automate this process](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html#_push_notifications_and_spring_cloud_bus)

##### Notes
- There are some limitations for dynamic refresh though. `@RefreshScope` doesn't work with `@Configuration` classes and doesn't affect `@Scheduled` methods
- `fail-fast` property means that Spring Boot application will fail startup immediately, if it cannot connect to the Config Service.
- There are significant [security notes](https://github.com/afermon/PiggyMetrics-Kubernetes#security) below

For more information please refer to the main repository [afermon/PiggyMetrics-Kubernetes](https://github.com/afermon/PiggyMetrics-Kubernetes)

## Refereces
* Forked from [sqshq/PiggyMetrics](https://github.com/sqshq/PiggyMetrics)