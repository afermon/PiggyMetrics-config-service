# Piggy Metrics (Kubernetes) - Config service

[![CircleCI](https://circleci.com/gh/afermon/PiggyMetrics-config-service.svg?style=svg)](https://circleci.com/gh/afermon/PiggyMetrics-config-service) [![codecov](https://codecov.io/gh/afermon/PiggyMetrics-config-service/branch/master/graph/badge.svg)](https://codecov.io/gh/afermon/PiggyMetrics-config-service) [![GitHub license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/afermon/PiggyMetrics-config-service/blob/master/LICENCE)

[Spring Cloud Config](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html) is horizontally scalable centralized configuration service for distributed systems. It uses a pluggable repository layer that currently supports local storage, Git, and Subversion. 

In this project, I use `native profile`, which simply loads config files from the local classpath. You can see `shared` directory in [Config service resources](https://github.com/sqshq/PiggyMetrics/tree/master/config/src/main/resources). Now, when Notification-service requests it's configuration, Config service responses with `shared/notification-service.yml` and `shared/application.yml` (which is shared between all client applications).

For more information please refer to the main repository [afermon/PiggyMetrics-Kubernetes](https://github.com/afermon/PiggyMetrics-Kubernetes)

## Refereces
* Forked from [sqshq/PiggyMetrics](https://github.com/sqshq/PiggyMetrics)