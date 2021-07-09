  ## 1.问题背景:
      异步传参：HttpRequest 后获取请求头信息
      
  ## 2.问题现象
      日志告警提示如下:RESTEASY003880: Unable to find contextual data of type: org.jboss.resteasy.spi.HttpRequest
      
      ```java
      /**
       * 告警日志
       **/
      2021-07-02 00:07:05 Caused by: org.jboss.resteasy.spi.LoggableFailure: RESTEASY003880: Unable to find contextual data of type: org.jboss.resteasy.spi.HttpRequest
      2021-07-02 00:07:05 at org.jboss.resteasy.core.ContextParameterInjector$GenericDelegatingProxy.invoke(ContextParameterInjector.java:62)
      2021-07-02 00:07:05 at com.sun.proxy.$Proxy97.getHttpHeaders(Unknown Source)
      2021-07-02 00:07:05 at java.util.concurrent.CompletableFuture$AsyncSupply.run(CompletableFuture.java:1604)
      2021-07-02 00:07:05 at java.util.concurrent.CompletableFuture$AsyncSupply.exec(CompletableFuture.java:1596)
      2021-07-02 00:07:05 at java.util.concurrent.ForkJoinTask.doExec(ForkJoinTask.java:289)
      2021-07-02 00:07:05 at java.util.concurrent.ForkJoinPool$WorkQueue.runTask(ForkJoinPool.java:1056)
      2021-07-02 00:07:05 at java.util.concurrent.ForkJoinPool.runWorker(ForkJoinPool.java:1692)
      2021-07-02 00:07:05 at java.util.concurrent.ForkJoinWorkerThread.run(ForkJoinWorkerThread.java:175)
      ```
## 3.解决方案：
      在主线程中取出HttpRequest中请求头信息，封装成异步线程所需参数传递。
