# apollo-portal-oidc-issue

Apollo portal 配置 OIDC 后启动失败。

Apollo portal fail to start if enable oidc.

## 重现方法 How to reproduce

1. `docker-compose up -d`
2. 等待一分钟，Portal 第一次启动可能因为连不上 mysql 重启一次。 Wait for one minute. The portal may be restarted for the first time because it cannot connect to MySQL.
3. `docker-compose logs -f apollo-portal`
4. Portal 启动失败，日志如下。 Portal failed to start, here are the logs.
```
apollo-portal_1  | 2022-03-15 13:51:56.907  WARN 1 --- [           main] ConfigServletWebServerApplicationContext : Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'authConfiguration.OidcWebSecurityConfigurerAdapter' defined in URL [jar:file:/apollo-portal/apollo-portal-1.9.2.jar!/BOOT-INF/classes!/com/ctrip/framework/apollo/portal/spi/configuration/AuthConfiguration$OidcWebSecurityConfigurerAdapter.class]: Unsatisfied dependency expressed through constructor parameter 0; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'org.springframework.security.oauth2.client.registration.InMemoryClientRegistrationRepository' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {}
...
apollo-portal_1  | 2022-03-15 13:51:57.076  WARN 1 --- [           main] o.a.c.loader.WebappClassLoaderBase       : The web application [ROOT] appears to have started a thread named [Apollo-ConfigRefresher-1] but has failed to stop it. This is very likely to create a memory leak. Stack trace of thread:
apollo-portal_1  |  sun.misc.Unsafe.park(Native Method)
apollo-portal_1  |  java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:215)
apollo-portal_1  |  java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(AbstractQueuedSynchronizer.java:2078)
apollo-portal_1  |  java.util.concurrent.ScheduledThreadPoolExecutor$DelayedWorkQueue.take(ScheduledThreadPoolExecutor.java:1093)
apollo-portal_1  |  java.util.concurrent.ScheduledThreadPoolExecutor$DelayedWorkQueue.take(ScheduledThreadPoolExecutor.java:809)
apollo-portal_1  |  java.util.concurrent.ThreadPoolExecutor.getTask(ThreadPoolExecutor.java:1074)
apollo-portal_1  |  java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1134)
apollo-portal_1  |  java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
apollo-portal_1  |  java.lang.Thread.run(Thread.java:748)
apollo-portal_1  | 2022-03-15 13:51:57.077  WARN 1 --- [           main] o.a.c.loader.WebappClassLoaderBase       : The web application [ROOT] appears to have started a thread named [Apollo-ConsumerAuditUtil-1] but has failed to stop it. This is very likely to create a memory leak. Stack trace of thread:
apollo-portal_1  |  sun.misc.Unsafe.park(Native Method)
apollo-portal_1  |  java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:215)
apollo-portal_1  |  java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(AbstractQueuedSynchronizer.java:2078)
apollo-portal_1  |  java.util.concurrent.LinkedBlockingQueue.poll(LinkedBlockingQueue.java:467)
apollo-portal_1  |  com.google.common.collect.Queues.drain(Queues.java:291)
apollo-portal_1  |  com.ctrip.framework.apollo.openapi.util.ConsumerAuditUtil.lambda$afterPropertiesSet$0(ConsumerAuditUtil.java:88)
apollo-portal_1  |  com.ctrip.framework.apollo.openapi.util.ConsumerAuditUtil$$Lambda$892/1977508673.run(Unknown Source)
apollo-portal_1  |  java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
apollo-portal_1  |  java.util.concurrent.FutureTask.run(FutureTask.java:266)
apollo-portal_1  |  java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
apollo-portal_1  |  java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
apollo-portal_1  |  java.lang.Thread.run(Thread.java:748)
apollo-portal_1  | 2022-03-15 13:51:57.112  INFO 1 --- [           main] ConditionEvaluationReportLoggingListener :
apollo-portal_1  |
apollo-portal_1  | Error starting ApplicationContext. To display the conditions report re-run your application with 'debug' enabled.
apollo-portal_1  | 2022-03-15 13:51:57.184 ERROR 1 --- [           main] o.s.b.d.LoggingFailureAnalysisReporter   :
apollo-portal_1  |
apollo-portal_1  | ***************************
apollo-portal_1  | APPLICATION FAILED TO START
apollo-portal_1  | ***************************
apollo-portal_1  |
apollo-portal_1  | Description:
apollo-portal_1  |
apollo-portal_1  | Parameter 0 of constructor in com.ctrip.framework.apollo.portal.spi.configuration.AuthConfiguration$OidcWebSecurityConfigurerAdapter required a bean of type 'org.springframework.security.oauth2.client.registration.InMemoryClientRegistrationRepository' that could not be found.
apollo-portal_1  |
apollo-portal_1  |
apollo-portal_1  | Action:
apollo-portal_1  |
apollo-portal_1  | Consider defining a bean of type 'org.springframework.security.oauth2.client.registration.InMemoryClientRegistrationRepository' in your configuration.
apollo-portal_1  |
```