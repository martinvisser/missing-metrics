# Issue: reduced set of metrics

In a Webflux app, after upgrading to Spring Boot `3.0.6` the amount of exposed metrics is reduced significantly.

There seems to be a correlation between `spring.main.cloud-platform: cloud_foundry` and the following dependency:

```xml

<dependency>
    <groupId>io.pivotal.spring.cloud</groupId>
    <artifactId>spring-cloud-services-starter-config-client</artifactId>
</dependency>
```

Setting `spring.main.cloud-platform` to anything else doesn't seem to affect anything. Also
using `spring-boot-starter-web` seems to work just fine. However, the combination
of `spring-boot-starter-webflux`, `spring-cloud-services-starter-config-client` and `cloud_foundry` seems to have
influence on the exposed metrics.

Further down this readme I also posted the output of the app during startup with SB `3.0.5` and `3.0.6` to compare,
because it's quite different.

**Note** This also seems to affect Spring Boot `2.7.11` (using version `3.5.5`
of `spring-cloud-services-starter-config-client`).

## Expected metrics:

```
# HELP jvm_gc_overhead_percent An approximation of the percent of CPU time used by GC activities over the last lookback period or since monitoring began, whichever is shorter, in the range [0..1]
# TYPE jvm_gc_overhead_percent gauge
jvm_gc_overhead_percent 2.5658106179019018E-5
# HELP jvm_buffer_total_capacity_bytes An estimate of the total capacity of the buffers in this pool
# TYPE jvm_buffer_total_capacity_bytes gauge
jvm_buffer_total_capacity_bytes{id="mapped - 'non-volatile memory'",} 0.0
jvm_buffer_total_capacity_bytes{id="mapped",} 0.0
jvm_buffer_total_capacity_bytes{id="direct",} 8396807.0
# HELP jvm_gc_max_data_size_bytes Max size of long-lived heap memory pool
# TYPE jvm_gc_max_data_size_bytes gauge
jvm_gc_max_data_size_bytes 8.589934592E9
# HELP jvm_gc_memory_promoted_bytes_total Count of positive increases in the size of the old generation memory pool before GC to after GC
# TYPE jvm_gc_memory_promoted_bytes_total counter
jvm_gc_memory_promoted_bytes_total 0.0
# HELP system_cpu_count The number of processors available to the Java virtual machine
# TYPE system_cpu_count gauge
system_cpu_count 16.0
# HELP jvm_memory_used_bytes The amount of used memory
# TYPE jvm_memory_used_bytes gauge
jvm_memory_used_bytes{area="heap",id="G1 Survivor Space",} 3545696.0
jvm_memory_used_bytes{area="heap",id="G1 Old Gen",} 2.0512768E7
jvm_memory_used_bytes{area="nonheap",id="Metaspace",} 3.7927464E7
jvm_memory_used_bytes{area="nonheap",id="CodeCache",} 8642432.0
jvm_memory_used_bytes{area="heap",id="G1 Eden Space",} 4194304.0
jvm_memory_used_bytes{area="nonheap",id="Compressed Class Space",} 5596904.0
# HELP logback_events_total Number of log events that were enabled by the effective log level
# TYPE logback_events_total counter
logback_events_total{level="warn",} 0.0
logback_events_total{level="debug",} 0.0
logback_events_total{level="error",} 0.0
logback_events_total{level="trace",} 0.0
logback_events_total{level="info",} 2.0
# HELP jvm_memory_max_bytes The maximum amount of memory in bytes that can be used for memory management
# TYPE jvm_memory_max_bytes gauge
jvm_memory_max_bytes{area="heap",id="G1 Survivor Space",} -1.0
jvm_memory_max_bytes{area="heap",id="G1 Old Gen",} 8.589934592E9
jvm_memory_max_bytes{area="nonheap",id="Metaspace",} -1.0
jvm_memory_max_bytes{area="nonheap",id="CodeCache",} 5.0331648E7
jvm_memory_max_bytes{area="heap",id="G1 Eden Space",} -1.0
jvm_memory_max_bytes{area="nonheap",id="Compressed Class Space",} 1.073741824E9
# HELP process_start_time_seconds Start time of the process since unix epoch.
# TYPE process_start_time_seconds gauge
process_start_time_seconds 1.682406021469E9
# HELP executor_queue_remaining_tasks The number of additional elements that this queue can ideally accept without blocking
# TYPE executor_queue_remaining_tasks gauge
executor_queue_remaining_tasks{name="applicationTaskExecutor",} 2.147483647E9
# HELP jvm_info JVM version info
# TYPE jvm_info gauge
jvm_info{runtime="OpenJDK Runtime Environment",vendor="Eclipse Adoptium",version="17.0.7+7",} 1.0
# HELP jvm_threads_states_threads The current number of threads
# TYPE jvm_threads_states_threads gauge
jvm_threads_states_threads{state="runnable",} 10.0
jvm_threads_states_threads{state="blocked",} 0.0
jvm_threads_states_threads{state="waiting",} 3.0
jvm_threads_states_threads{state="timed-waiting",} 3.0
jvm_threads_states_threads{state="new",} 0.0
jvm_threads_states_threads{state="terminated",} 0.0
# HELP jvm_threads_daemon_threads The current number of live daemon threads
# TYPE jvm_threads_daemon_threads gauge
jvm_threads_daemon_threads 14.0
# HELP application_started_time_seconds Time taken (ms) to start the application
# TYPE application_started_time_seconds gauge
application_started_time_seconds{main_application_class="com.example.demo.DemoApplicationKt",} 1.791
# HELP executor_queued_tasks The approximate number of tasks that are queued for execution
# TYPE executor_queued_tasks gauge
executor_queued_tasks{name="applicationTaskExecutor",} 0.0
# HELP jvm_compilation_time_ms_total The approximate accumulated elapsed time spent in compilation
# TYPE jvm_compilation_time_ms_total counter
jvm_compilation_time_ms_total{compiler="HotSpot 64-Bit Tiered Compilers",} 1127.0
# HELP executor_completed_tasks_total The approximate total number of tasks that have completed execution
# TYPE executor_completed_tasks_total counter
executor_completed_tasks_total{name="applicationTaskExecutor",} 0.0
# HELP jvm_threads_peak_threads The peak live thread count since the Java virtual machine started or peak was reset
# TYPE jvm_threads_peak_threads gauge
jvm_threads_peak_threads 19.0
# HELP jvm_classes_loaded_classes The number of classes that are currently loaded in the Java virtual machine
# TYPE jvm_classes_loaded_classes gauge
jvm_classes_loaded_classes 9222.0
# HELP executor_active_threads The approximate number of threads that are actively executing tasks
# TYPE executor_active_threads gauge
executor_active_threads{name="applicationTaskExecutor",} 0.0
# HELP executor_pool_size_threads The current number of threads in the pool
# TYPE executor_pool_size_threads gauge
executor_pool_size_threads{name="applicationTaskExecutor",} 0.0
# HELP disk_total_bytes Total space for path
# TYPE disk_total_bytes gauge
disk_total_bytes{path="/Users/martin/projects/missing-metrics/.",} 4.99963174912E11
# HELP jvm_threads_live_threads The current number of live threads including both daemon and non-daemon threads
# TYPE jvm_threads_live_threads gauge
jvm_threads_live_threads 16.0
# HELP application_ready_time_seconds Time taken (ms) for the application to be ready to service requests
# TYPE application_ready_time_seconds gauge
application_ready_time_seconds{main_application_class="com.example.demo.DemoApplicationKt",} 1.794
# HELP jvm_gc_live_data_size_bytes Size of long-lived heap memory pool after reclamation
# TYPE jvm_gc_live_data_size_bytes gauge
jvm_gc_live_data_size_bytes 0.0
# HELP jvm_classes_unloaded_classes_total The total number of classes unloaded since the Java virtual machine has started execution
# TYPE jvm_classes_unloaded_classes_total counter
jvm_classes_unloaded_classes_total 0.0
# HELP executor_pool_core_threads The core number of threads for the pool
# TYPE executor_pool_core_threads gauge
executor_pool_core_threads{name="applicationTaskExecutor",} 8.0
# HELP process_uptime_seconds The uptime of the Java virtual machine
# TYPE process_uptime_seconds gauge
process_uptime_seconds 196.875
# HELP jvm_memory_committed_bytes The amount of memory in bytes that is committed for the Java virtual machine to use
# TYPE jvm_memory_committed_bytes gauge
jvm_memory_committed_bytes{area="heap",id="G1 Survivor Space",} 4194304.0
jvm_memory_committed_bytes{area="heap",id="G1 Old Gen",} 5.0331648E7
jvm_memory_committed_bytes{area="nonheap",id="Metaspace",} 3.8469632E7
jvm_memory_committed_bytes{area="nonheap",id="CodeCache",} 1.1403264E7
jvm_memory_committed_bytes{area="heap",id="G1 Eden Space",} 4.6137344E7
jvm_memory_committed_bytes{area="nonheap",id="Compressed Class Space",} 5832704.0
# HELP http_server_requests_seconds  
# TYPE http_server_requests_seconds summary
http_server_requests_seconds_count{error="none",exception="none",method="GET",outcome="SUCCESS",status="200",uri="/actuator/prometheus",} 3.0
http_server_requests_seconds_sum{error="none",exception="none",method="GET",outcome="SUCCESS",status="200",uri="/actuator/prometheus",} 0.069057626
# HELP http_server_requests_seconds_max  
# TYPE http_server_requests_seconds_max gauge
http_server_requests_seconds_max{error="none",exception="none",method="GET",outcome="SUCCESS",status="200",uri="/actuator/prometheus",} 0.0
# HELP process_files_open_files The open file descriptor count
# TYPE process_files_open_files gauge
process_files_open_files 158.0
# HELP disk_free_bytes Usable space for path
# TYPE disk_free_bytes gauge
disk_free_bytes{path="/Users/martin/projects/missing-metrics/.",} 1.66315896832E11
# HELP http_server_requests_active_seconds_max  
# TYPE http_server_requests_active_seconds_max gauge
http_server_requests_active_seconds_max{exception="none",method="GET",outcome="SUCCESS",status="200",uri="UNKNOWN",} 0.002943414
# HELP http_server_requests_active_seconds  
# TYPE http_server_requests_active_seconds summary
http_server_requests_active_seconds_active_count{exception="none",method="GET",outcome="SUCCESS",status="200",uri="UNKNOWN",} 1.0
http_server_requests_active_seconds_duration_sum{exception="none",method="GET",outcome="SUCCESS",status="200",uri="UNKNOWN",} 0.002936169
# HELP jvm_memory_usage_after_gc_percent The percentage of long-lived heap pool used after the last GC event, in the range [0..1]
# TYPE jvm_memory_usage_after_gc_percent gauge
jvm_memory_usage_after_gc_percent{area="heap",pool="long-lived",} 0.00238800048828125
# HELP jvm_buffer_count_buffers An estimate of the number of buffers in the pool
# TYPE jvm_buffer_count_buffers gauge
jvm_buffer_count_buffers{id="mapped - 'non-volatile memory'",} 0.0
jvm_buffer_count_buffers{id="mapped",} 0.0
jvm_buffer_count_buffers{id="direct",} 6.0
# HELP process_cpu_usage The "recent cpu usage" for the Java Virtual Machine process
# TYPE process_cpu_usage gauge
process_cpu_usage 5.055560368150865E-5
# HELP jvm_gc_pause_seconds Time spent in GC pause
# TYPE jvm_gc_pause_seconds summary
jvm_gc_pause_seconds_count{action="end of minor GC",cause="Metadata GC Threshold",} 1.0
jvm_gc_pause_seconds_sum{action="end of minor GC",cause="Metadata GC Threshold",} 0.005
# HELP jvm_gc_pause_seconds_max Time spent in GC pause
# TYPE jvm_gc_pause_seconds_max gauge
jvm_gc_pause_seconds_max{action="end of minor GC",cause="Metadata GC Threshold",} 0.0
# HELP jvm_gc_memory_allocated_bytes_total Incremented for an increase in the size of the (young) heap memory pool after one GC to before the next
# TYPE jvm_gc_memory_allocated_bytes_total counter
jvm_gc_memory_allocated_bytes_total 2.9360128E7
# HELP executor_pool_max_threads The maximum allowed number of threads in the pool
# TYPE executor_pool_max_threads gauge
executor_pool_max_threads{name="applicationTaskExecutor",} 2.147483647E9
# HELP jvm_buffer_memory_used_bytes An estimate of the memory that the Java virtual machine is using for this buffer pool
# TYPE jvm_buffer_memory_used_bytes gauge
jvm_buffer_memory_used_bytes{id="mapped - 'non-volatile memory'",} 0.0
jvm_buffer_memory_used_bytes{id="mapped",} 0.0
jvm_buffer_memory_used_bytes{id="direct",} 8396808.0
# HELP system_cpu_usage The "recent cpu usage" of the system the application is running in
# TYPE system_cpu_usage gauge
system_cpu_usage 0.09495958014553117
# HELP process_files_max_files The maximum file descriptor count
# TYPE process_files_max_files gauge
process_files_max_files 10240.0
# HELP system_load_average_1m The sum of the number of runnable entities queued to available processors and the number of runnable entities running on the available processors averaged over a period of time
# TYPE system_load_average_1m gauge
system_load_average_1m 2.99658203125
```

## Actual metrics

```
# HELP executor_active_threads The approximate number of threads that are actively executing tasks
# TYPE executor_active_threads gauge
executor_active_threads{name="applicationTaskExecutor",} 0.0
# HELP executor_pool_core_threads The core number of threads for the pool
# TYPE executor_pool_core_threads gauge
executor_pool_core_threads{name="applicationTaskExecutor",} 8.0
# HELP executor_queue_remaining_tasks The number of additional elements that this queue can ideally accept without blocking
# TYPE executor_queue_remaining_tasks gauge
executor_queue_remaining_tasks{name="applicationTaskExecutor",} 2.147483647E9
# HELP executor_pool_max_threads The maximum allowed number of threads in the pool
# TYPE executor_pool_max_threads gauge
executor_pool_max_threads{name="applicationTaskExecutor",} 2.147483647E9
# HELP executor_pool_size_threads The current number of threads in the pool
# TYPE executor_pool_size_threads gauge
executor_pool_size_threads{name="applicationTaskExecutor",} 0.0
# HELP application_ready_time_seconds Time taken (ms) for the application to be ready to service requests
# TYPE application_ready_time_seconds gauge
application_ready_time_seconds{main_application_class="com.example.demo.DemoApplicationKt",} 1.892
# HELP executor_completed_tasks_total The approximate total number of tasks that have completed execution
# TYPE executor_completed_tasks_total counter
executor_completed_tasks_total{name="applicationTaskExecutor",} 0.0
# HELP application_started_time_seconds Time taken (ms) to start the application
# TYPE application_started_time_seconds gauge
application_started_time_seconds{main_application_class="com.example.demo.DemoApplicationKt",} 1.888
# HELP executor_queued_tasks The approximate number of tasks that are queued for execution
# TYPE executor_queued_tasks gauge
executor_queued_tasks{name="applicationTaskExecutor",} 0.0
```

## Startup information SB 3.0.5

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v3.0.5)

2023-04-25T09:14:03.988+02:00  INFO 45019 --- [           main] com.example.demo.DemoApplicationKt       : Starting DemoApplicationKt using Java 17.0.7 with PID 45019 (/Users/martin/projects/missing-metrics/target/classes started by martin in /Users/martin/projects/missing-metrics)
2023-04-25T09:14:03.991+02:00  INFO 45019 --- [           main] com.example.demo.DemoApplicationKt       : No active profile set, falling back to 1 default profile: "default"
2023-04-25T09:14:04.432+02:00  WARN 45019 --- [           main] o.s.c.annotation.AnnotationTypeMapping   : Support for convention-based annotation attribute overrides is deprecated and will be removed in Spring Framework 6.1. Please annotate the following attributes in @org.springframework.boot.actuate.autoconfigure.cloudfoundry.EndpointCloudFoundryExtension with appropriate @AliasFor declarations: [endpoint]
2023-04-25T09:14:04.602+02:00  INFO 45019 --- [           main] o.s.cloud.context.scope.GenericScope     : BeanFactory id=ed477fac-e53d-376d-a783-35557c752522
2023-04-25T09:14:04.631+02:00  INFO 45019 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.cloudfoundry.reactive.ReactiveCloudFoundryActuatorAutoConfiguration$IgnoredPathsSecurityConfiguration' of type [org.springframework.boot.actuate.autoconfigure.cloudfoundry.reactive.ReactiveCloudFoundryActuatorAutoConfiguration$IgnoredPathsSecurityConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:14:05.331+02:00  INFO 45019 --- [           main] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 16 endpoint(s) beneath base path '/actuator'
2023-04-25T09:14:05.484+02:00  INFO 45019 --- [           main] ctiveUserDetailsServiceAutoConfiguration : 

Using generated security password: c0b549f0-dbda-4c28-ac44-3510584bf842

2023-04-25T09:14:05.574+02:00  INFO 45019 --- [           main] o.s.b.web.embedded.netty.NettyWebServer  : Netty started on port 8080
2023-04-25T09:14:05.591+02:00  INFO 45019 --- [           main] com.example.demo.DemoApplicationKt       : Started DemoApplicationKt in 2.041 seconds (process running for 2.42)
```

## Startup information SB 3.0.6

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v3.0.6)

2023-04-25T09:15:00.389+02:00  INFO 45082 --- [           main] com.example.demo.DemoApplicationKt       : Starting DemoApplicationKt using Java 17.0.7 with PID 45082 (/Users/martin/projects/missing-metrics/target/classes started by martin in /Users/martin/projects/missing-metrics)
2023-04-25T09:15:00.391+02:00  INFO 45082 --- [           main] com.example.demo.DemoApplicationKt       : No active profile set, falling back to 1 default profile: "default"
2023-04-25T09:15:00.817+02:00  WARN 45082 --- [           main] o.s.c.annotation.AnnotationTypeMapping   : Support for convention-based annotation attribute overrides is deprecated and will be removed in Spring Framework 6.1. Please annotate the following attributes in @org.springframework.boot.actuate.autoconfigure.cloudfoundry.EndpointCloudFoundryExtension with appropriate @AliasFor declarations: [endpoint]
2023-04-25T09:15:00.991+02:00  INFO 45082 --- [           main] o.s.cloud.context.scope.GenericScope     : BeanFactory id=ed477fac-e53d-376d-a783-35557c752522
2023-04-25T09:15:01.020+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.cloudfoundry.reactive.ReactiveCloudFoundryActuatorAutoConfiguration$IgnoredPathsSecurityConfiguration' of type [org.springframework.boot.actuate.autoconfigure.cloudfoundry.reactive.ReactiveCloudFoundryActuatorAutoConfiguration$IgnoredPathsSecurityConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.022+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.cloudfoundry.reactive.ReactiveCloudFoundryActuatorAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.cloudfoundry.reactive.ReactiveCloudFoundryActuatorAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.023+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.endpoint.EndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.endpoint.EndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.026+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'endpointOperationParameterMapper' of type [org.springframework.boot.actuate.endpoint.invoke.convert.ConversionServiceParameterValueMapper] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.031+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.commons.config.CommonsConfigAutoConfiguration' of type [org.springframework.cloud.commons.config.CommonsConfigAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.032+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.client.loadbalancer.LoadBalancerDefaultMappingsProviderAutoConfiguration' of type [org.springframework.cloud.client.loadbalancer.LoadBalancerDefaultMappingsProviderAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.032+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'loadBalancerClientsDefaultsMappingsProvider' of type [org.springframework.cloud.client.loadbalancer.LoadBalancerDefaultMappingsProviderAutoConfiguration$$Lambda$496/0x0000000801340250] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.033+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'defaultsBindHandlerAdvisor' of type [org.springframework.cloud.commons.config.DefaultsBindHandlerAdvisor] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.039+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'management.endpoints.web-org.springframework.boot.actuate.autoconfigure.endpoint.web.WebEndpointProperties' of type [org.springframework.boot.actuate.autoconfigure.endpoint.web.WebEndpointProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.039+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.endpoint.web.WebEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.endpoint.web.WebEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.043+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'endpointMediaTypes' of type [org.springframework.boot.actuate.endpoint.web.EndpointMediaTypes] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.044+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration' of type [org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.047+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration' of type [org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.047+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorConfiguration$ReactorNetty' of type [org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorConfiguration$ReactorNetty] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.048+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.reactor.netty.ReactorNettyConfigurations$ReactorResourceFactoryConfiguration' of type [org.springframework.boot.autoconfigure.reactor.netty.ReactorNettyConfigurations$ReactorResourceFactoryConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.049+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'spring.reactor.netty-org.springframework.boot.autoconfigure.reactor.netty.ReactorNettyProperties' of type [org.springframework.boot.autoconfigure.reactor.netty.ReactorNettyProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.097+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'reactorResourceFactory' of type [org.springframework.http.client.reactive.ReactorResourceFactory] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.262+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'reactorClientHttpConnector' of type [org.springframework.http.client.reactive.ReactorClientHttpConnector] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.262+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'clientConnectorCustomizer' of type [org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration$$Lambda$534/0x00000008013c7698] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.263+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration$WebClientCodecsConfiguration' of type [org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration$WebClientCodecsConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.264+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration$DefaultCodecsConfiguration' of type [org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration$DefaultCodecsConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.265+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'spring.codec-org.springframework.boot.autoconfigure.codec.CodecProperties' of type [org.springframework.boot.autoconfigure.codec.CodecProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.265+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'defaultCodecCustomizer' of type [org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration$DefaultCodecsConfiguration$$Lambda$535/0x00000008013c78c0] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.266+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration$JacksonCodecConfiguration' of type [org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration$JacksonCodecConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.266+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$JacksonObjectMapperConfiguration' of type [org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$JacksonObjectMapperConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.267+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$JacksonObjectMapperBuilderConfiguration' of type [org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$JacksonObjectMapperBuilderConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.268+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$Jackson2ObjectMapperBuilderCustomizerConfiguration' of type [org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$Jackson2ObjectMapperBuilderCustomizerConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.270+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'spring.jackson-org.springframework.boot.autoconfigure.jackson.JacksonProperties' of type [org.springframework.boot.autoconfigure.jackson.JacksonProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.271+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$ParameterNamesModuleConfiguration' of type [org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$ParameterNamesModuleConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.273+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'parameterNamesModule' of type [com.fasterxml.jackson.module.paramnames.ParameterNamesModule] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.273+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$JacksonMixinConfiguration' of type [org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$JacksonMixinConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.276+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'jsonMixinModuleEntries' of type [org.springframework.boot.jackson.JsonMixinModuleEntries] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.277+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'jsonMixinModule' of type [org.springframework.boot.jackson.JsonMixinModule] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.278+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration' of type [org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.289+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'jsonComponentModule' of type [org.springframework.boot.jackson.JsonComponentModule] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.290+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'standardJacksonObjectMapperBuilderCustomizer' of type [org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration$Jackson2ObjectMapperBuilderCustomizerConfiguration$StandardJackson2ObjectMapperBuilderCustomizer] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.291+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'jacksonObjectMapperBuilder' of type [org.springframework.http.converter.json.Jackson2ObjectMapperBuilder] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.300+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'jacksonObjectMapper' of type [com.fasterxml.jackson.databind.ObjectMapper] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.300+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'jacksonCodecCustomizer' of type [org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration$JacksonCodecConfiguration$$Lambda$544/0x00000008013d6460] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.300+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.autoconfigure.web.reactive.ReactiveMultipartAutoConfiguration' of type [org.springframework.boot.autoconfigure.web.reactive.ReactiveMultipartAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.302+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'spring.webflux.multipart-org.springframework.boot.autoconfigure.web.reactive.ReactiveMultipartProperties' of type [org.springframework.boot.autoconfigure.web.reactive.ReactiveMultipartProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.303+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'defaultPartHttpMessageReaderCustomizer' of type [org.springframework.boot.autoconfigure.web.reactive.ReactiveMultipartAutoConfiguration$$Lambda$545/0x00000008013d6688] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.303+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'exchangeStrategiesCustomizer' of type [org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientCodecCustomizer] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.303+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.observation.web.client.WebClientObservationConfiguration' of type [org.springframework.boot.actuate.autoconfigure.observation.web.client.WebClientObservationConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.304+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.observation.ObservationAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.observation.ObservationAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.305+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'observationRegistry' of type [io.micrometer.observation.SimpleObservationRegistry] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.307+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'management.observations-org.springframework.boot.actuate.autoconfigure.observation.ObservationProperties' of type [org.springframework.boot.actuate.autoconfigure.observation.ObservationProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.309+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'management.metrics-org.springframework.boot.actuate.autoconfigure.metrics.MetricsProperties' of type [org.springframework.boot.actuate.autoconfigure.metrics.MetricsProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.313+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'observationWebClientCustomizer' of type [org.springframework.boot.actuate.metrics.web.reactive.client.ObservationWebClientCustomizer] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.316+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'webClientBuilder' of type [org.springframework.web.reactive.function.client.DefaultWebClientBuilder] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.318+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'webEndpointPathMapper' of type [org.springframework.boot.actuate.autoconfigure.endpoint.web.MappingWebEndpointPathMapper] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.319+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'controllerExposeExcludePropertyEndpointFilter' of type [org.springframework.boot.actuate.autoconfigure.endpoint.expose.IncludeExcludeEndpointFilter] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.321+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'controllerEndpointDiscoverer' of type [org.springframework.boot.actuate.endpoint.web.annotation.ControllerEndpointDiscoverer] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.330+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.cache.CachesEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.cache.CachesEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.333+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'cachesEndpoint' of type [org.springframework.boot.actuate.cache.CachesEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.337+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.health.HealthEndpointConfiguration' of type [org.springframework.boot.actuate.autoconfigure.health.HealthEndpointConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.340+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'management.endpoint.health-org.springframework.boot.actuate.autoconfigure.health.HealthEndpointProperties' of type [org.springframework.boot.actuate.autoconfigure.health.HealthEndpointProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.342+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'healthStatusAggregator' of type [org.springframework.boot.actuate.health.SimpleStatusAggregator] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.343+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'healthHttpCodeStatusMapper' of type [org.springframework.boot.actuate.health.SimpleHttpCodeStatusMapper] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.345+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'healthEndpointGroups' of type [org.springframework.boot.actuate.autoconfigure.health.AutoConfiguredHealthEndpointGroups] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.345+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.config.client.ConfigClientAutoConfiguration$ConfigServerHealthIndicatorConfiguration' of type [org.springframework.cloud.config.client.ConfigClientAutoConfiguration$ConfigServerHealthIndicatorConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.347+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'configClientHealthProperties' of type [org.springframework.cloud.config.client.ConfigClientHealthProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.348+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'clientConfigServerHealthIndicator' of type [org.springframework.cloud.config.client.ConfigServerHealthIndicator] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.348+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.system.DiskSpaceHealthContributorAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.system.DiskSpaceHealthContributorAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.349+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'management.health.diskspace-org.springframework.boot.actuate.autoconfigure.system.DiskSpaceHealthIndicatorProperties' of type [org.springframework.boot.actuate.autoconfigure.system.DiskSpaceHealthIndicatorProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.349+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'diskSpaceHealthIndicator' of type [org.springframework.boot.actuate.system.DiskSpaceHealthIndicator] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.349+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.health.HealthContributorAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.health.HealthContributorAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.350+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'pingHealthContributor' of type [org.springframework.boot.actuate.health.PingHealthIndicator] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.350+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.autoconfigure.RefreshEndpointAutoConfiguration' of type [org.springframework.cloud.autoconfigure.RefreshEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.351+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.autoconfigure.ConfigurationPropertiesRebinderAutoConfiguration' of type [org.springframework.cloud.autoconfigure.ConfigurationPropertiesRebinderAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.353+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'configurationPropertiesRebinder' of type [org.springframework.cloud.context.properties.ConfigurationPropertiesRebinder] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.353+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'refreshScopeHealthIndicator' of type [org.springframework.cloud.health.RefreshScopeHealthIndicator] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.353+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.client.CommonsClientAutoConfiguration$DiscoveryLoadBalancerConfiguration' of type [org.springframework.cloud.client.CommonsClientAutoConfiguration$DiscoveryLoadBalancerConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.355+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'spring.cloud.discovery.client.health-indicator-org.springframework.cloud.client.discovery.health.DiscoveryClientHealthIndicatorProperties' of type [org.springframework.cloud.client.discovery.health.DiscoveryClientHealthIndicatorProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.356+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'discoveryClientHealthIndicator' of type [org.springframework.cloud.client.discovery.health.DiscoveryClientHealthIndicator] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.357+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'discoveryCompositeHealthContributor' of type [org.springframework.cloud.client.discovery.health.DiscoveryCompositeHealthContributor] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.357+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.client.ReactiveCommonsClientAutoConfiguration$ReactiveDiscoveryLoadBalancerConfiguration' of type [org.springframework.cloud.client.ReactiveCommonsClientAutoConfiguration$ReactiveDiscoveryLoadBalancerConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.358+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.client.discovery.simple.reactive.SimpleReactiveDiscoveryClientAutoConfiguration$HealthConfiguration' of type [org.springframework.cloud.client.discovery.simple.reactive.SimpleReactiveDiscoveryClientAutoConfiguration$HealthConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.365+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'server-org.springframework.boot.autoconfigure.web.ServerProperties' of type [org.springframework.boot.autoconfigure.web.ServerProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.366+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.commons.util.UtilAutoConfiguration' of type [org.springframework.cloud.commons.util.UtilAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.368+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'inetUtilsProperties' of type [org.springframework.cloud.commons.util.InetUtilsProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.369+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'inetUtils' of type [org.springframework.cloud.commons.util.InetUtils] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.369+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.client.discovery.simple.reactive.SimpleReactiveDiscoveryClientAutoConfiguration' of type [org.springframework.cloud.client.discovery.simple.reactive.SimpleReactiveDiscoveryClientAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.372+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'simpleReactiveDiscoveryProperties' of type [org.springframework.cloud.client.discovery.simple.reactive.SimpleReactiveDiscoveryProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.372+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'simpleReactiveDiscoveryClient' of type [org.springframework.cloud.client.discovery.simple.reactive.SimpleReactiveDiscoveryClient] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.372+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'simpleReactiveDiscoveryClientHealthIndicator' of type [org.springframework.cloud.client.discovery.health.reactive.ReactiveDiscoveryClientHealthIndicator] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.373+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'reactiveDiscoveryClients' of type [org.springframework.cloud.client.discovery.health.reactive.ReactiveDiscoveryCompositeHealthContributor] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.376+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'healthContributorRegistry' of type [org.springframework.boot.actuate.autoconfigure.health.AutoConfiguredHealthContributorRegistry] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.378+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'healthEndpoint' of type [org.springframework.boot.actuate.health.HealthEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.378+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.context.properties.ConfigurationPropertiesReportEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.context.properties.ConfigurationPropertiesReportEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.380+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'management.endpoint.configprops-org.springframework.boot.actuate.autoconfigure.context.properties.ConfigurationPropertiesReportEndpointProperties' of type [org.springframework.boot.actuate.autoconfigure.context.properties.ConfigurationPropertiesReportEndpointProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.383+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'configurationPropertiesReportEndpoint' of type [org.springframework.boot.actuate.context.properties.ConfigurationPropertiesReportEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.383+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.env.EnvironmentEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.env.EnvironmentEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.385+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'management.endpoint.env-org.springframework.boot.actuate.autoconfigure.env.EnvironmentEndpointProperties' of type [org.springframework.boot.actuate.autoconfigure.env.EnvironmentEndpointProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.386+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'environmentEndpoint' of type [org.springframework.boot.actuate.env.EnvironmentEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.386+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.beans.BeansEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.beans.BeansEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.387+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'beansEndpoint' of type [org.springframework.boot.actuate.beans.BeansEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.394+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'cachesEndpointWebExtension' of type [org.springframework.boot.actuate.cache.CachesEndpointWebExtension] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.396+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.health.HealthEndpointReactiveWebExtensionConfiguration' of type [org.springframework.boot.actuate.autoconfigure.health.HealthEndpointReactiveWebExtensionConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.396+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.health.ReactiveHealthEndpointConfiguration' of type [org.springframework.boot.actuate.autoconfigure.health.ReactiveHealthEndpointConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.399+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'reactiveHealthContributorRegistry' of type [org.springframework.boot.actuate.autoconfigure.health.AutoConfiguredReactiveHealthContributorRegistry] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.400+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'reactiveHealthEndpointWebExtension' of type [org.springframework.boot.actuate.health.ReactiveHealthEndpointWebExtension] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.400+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'cloudFoundryReactiveHealthEndpointWebExtension' of type [org.springframework.boot.actuate.autoconfigure.cloudfoundry.reactive.CloudFoundryReactiveHealthEndpointWebExtension] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.400+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.info.InfoEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.info.InfoEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.401+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'infoEndpoint' of type [org.springframework.boot.actuate.info.InfoEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.401+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.condition.ConditionsReportEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.condition.ConditionsReportEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.402+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'conditionsReportEndpoint' of type [org.springframework.boot.actuate.autoconfigure.condition.ConditionsReportEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.409+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'configurationPropertiesReportEndpointWebExtension' of type [org.springframework.boot.actuate.context.properties.ConfigurationPropertiesReportEndpointWebExtension] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.410+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'environmentEndpointWebExtension' of type [org.springframework.boot.actuate.env.EnvironmentEndpointWebExtension] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.411+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.logging.LoggersEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.logging.LoggersEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.412+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'loggersEndpoint' of type [org.springframework.boot.actuate.logging.LoggersEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.414+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.management.HeapDumpWebEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.management.HeapDumpWebEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.414+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'heapDumpWebEndpoint' of type [org.springframework.boot.actuate.management.HeapDumpWebEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.415+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.management.ThreadDumpEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.management.ThreadDumpEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.415+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'dumpEndpoint' of type [org.springframework.boot.actuate.management.ThreadDumpEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.416+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.metrics.export.prometheus.PrometheusMetricsExportAutoConfiguration$PrometheusScrapeEndpointConfiguration' of type [org.springframework.boot.actuate.autoconfigure.metrics.export.prometheus.PrometheusMetricsExportAutoConfiguration$PrometheusScrapeEndpointConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.416+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.metrics.export.prometheus.PrometheusMetricsExportAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.metrics.export.prometheus.PrometheusMetricsExportAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.417+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'collectorRegistry' of type [io.prometheus.client.CollectorRegistry] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.417+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'prometheusEndpoint' of type [org.springframework.boot.actuate.metrics.export.prometheus.PrometheusScrapeEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.419+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.metrics.MetricsEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.metrics.MetricsEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.420+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'management.prometheus.metrics.export-org.springframework.boot.actuate.autoconfigure.metrics.export.prometheus.PrometheusProperties' of type [org.springframework.boot.actuate.autoconfigure.metrics.export.prometheus.PrometheusProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.422+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'prometheusConfig' of type [org.springframework.boot.actuate.autoconfigure.metrics.export.prometheus.PrometheusPropertiesConfigAdapter] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.422+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.metrics.MetricsAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.metrics.MetricsAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.423+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'micrometerClock' of type [io.micrometer.core.instrument.Clock$1] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.435+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'prometheusMeterRegistry' of type [io.micrometer.prometheus.PrometheusMeterRegistry] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.436+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'metricsEndpoint' of type [org.springframework.boot.actuate.metrics.MetricsEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.437+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.scheduling.ScheduledTasksEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.scheduling.ScheduledTasksEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.437+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'scheduledTasksEndpoint' of type [org.springframework.boot.actuate.scheduling.ScheduledTasksEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.438+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.web.mappings.MappingsEndpointAutoConfiguration' of type [org.springframework.boot.actuate.autoconfigure.web.mappings.MappingsEndpointAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.438+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.boot.actuate.autoconfigure.web.mappings.MappingsEndpointAutoConfiguration$ReactiveWebConfiguration' of type [org.springframework.boot.actuate.autoconfigure.web.mappings.MappingsEndpointAutoConfiguration$ReactiveWebConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.439+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'dispatcherHandlerMappingDescriptionProvider' of type [org.springframework.boot.actuate.web.mappings.reactive.DispatcherHandlersMappingDescriptionProvider] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.440+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'mappingsEndpoint' of type [org.springframework.boot.actuate.web.mappings.MappingsEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.440+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.autoconfigure.RefreshEndpointAutoConfiguration$RefreshEndpointConfiguration' of type [org.springframework.cloud.autoconfigure.RefreshEndpointAutoConfiguration$RefreshEndpointConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.441+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.autoconfigure.RefreshAutoConfiguration' of type [org.springframework.cloud.autoconfigure.RefreshAutoConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.442+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'spring.cloud.refresh-org.springframework.cloud.autoconfigure.RefreshAutoConfiguration$RefreshProperties' of type [org.springframework.cloud.autoconfigure.RefreshAutoConfiguration$RefreshProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.443+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'configDataContextRefresher' of type [org.springframework.cloud.context.refresh.ConfigDataContextRefresher] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.443+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'refreshEndpoint' of type [org.springframework.cloud.endpoint.RefreshEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.444+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'commonsFeatures' of type [org.springframework.cloud.client.actuator.HasFeatures] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.444+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'reactiveCommonsFeatures' of type [org.springframework.cloud.client.actuator.HasFeatures] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.444+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.client.CommonsClientAutoConfiguration$ActuatorConfiguration' of type [org.springframework.cloud.client.CommonsClientAutoConfiguration$ActuatorConfiguration] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.445+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'featuresEndpoint' of type [org.springframework.cloud.client.actuator.FeaturesEndpoint] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.480+02:00  INFO 45082 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'cloudFoundryWebFluxEndpointHandlerMapping' of type [org.springframework.boot.actuate.autoconfigure.cloudfoundry.reactive.CloudFoundryWebFluxEndpointHandlerMapping] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
2023-04-25T09:15:01.602+02:00  INFO 45082 --- [           main] o.s.b.a.e.web.EndpointLinksResolver      : Exposing 16 endpoint(s) beneath base path '/actuator'
2023-04-25T09:15:01.760+02:00  INFO 45082 --- [           main] ctiveUserDetailsServiceAutoConfiguration : 

Using generated security password: 623e8a95-650c-41dc-8913-32430b09a0e5

2023-04-25T09:15:01.839+02:00  INFO 45082 --- [           main] o.s.b.web.embedded.netty.NettyWebServer  : Netty started on port 8080
2023-04-25T09:15:01.856+02:00  INFO 45082 --- [           main] com.example.demo.DemoApplicationKt       : Started DemoApplicationKt in 1.896 seconds (process running for 2.265)
```
