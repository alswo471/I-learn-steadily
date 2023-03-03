# Springboot @Scheduled 애노테이션 쓰레드 문제

</br>
## 쓰레드 풀 설정
</br>

- 기본적으로 @EnableScheduling 어노테이션을 사용 시 작업을 실행할 스케줄링을 위해서 스레드가 하나만 있는 스레드 풀을 만듭니다. @Scheduled 작업은 대기열에 쌓이게 되며 단일 스레드에 의해서만 실행됩니다.

- 여러 작업을 제시간에 맞추어서 실행하기 위해서는 스레드를 늘릴 필요가 있을 것입니다. SchedulingConfigurer 인터페이스를 구현하여 설정을 변경할 수 있습니다.

- setThreadNamePrefix를 이용하여 로그로 남길시 현재 실행하는 쓰레드 명 Prefix를 붙일 수 있습니다.

</br>

```java

@Configuration
public class SchedulerConfig implements SchedulingConfigurer {

@Value("${thread.pool.size}")
private int POOL_SIZE;

@Override
public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {
ThreadPoolTaskScheduler scheduler = new ThreadPoolTaskScheduler();

scheduler.setPoolSize(POOL_SIZE);
scheduler.setThreadNamePrefix("현재 쓰레드-");
scheduler.initialize();

taskRegistrar.setTaskScheduler(scheduler);
}
}

```

</br>

> thread.pool.size는 application.yml 파일을 통하여 3개로 늘려줍니다.

</br>

```
thread:
pool:
size: 3
[    현재 쓰레드-2] com.covenant.schedule.ScheduledTasks     : 현재시간 - 50:12:760
[    현재 쓰레드-3] com.covenant.schedule.ScheduledTasks     : 현재시간 - 50:13:763
[    현재 쓰레드-1] com.covenant.schedule.ScheduledTasks     : 현재시간 - 50:13:763
[    현재 쓰레드-1] com.covenant.schedule.ScheduledTasks     : 현재시간 - 50:13:766
```

</br>

- setThreadNamePrefix 지정한 이름으로 현재 쓰레드라는 쓰레드 명이 추가되었으며 쓰레드 3개로 작업을 수행하는 것을 확인할 수 있습니다.
