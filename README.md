# Slack with Spring boot (1)

[Slack](https://join.slack.com/t/w1656554845-ewl688059/shared_invite/zt-1bjhwu7wn-gBAQswzGi5K53u5Y6~RvLQ)

# 슬랙 활용법과 봇 만들기

# 왜 슬랙인가?

## 🥇전세계 슬랙 허들 1위 롯데 e커머스

[우리는 Slack으로 출근! Slack에서 퇴근!](https://techblog.lotteon.com/%EC%9A%B0%EB%A6%AC%EB%8A%94-slack%EC%9C%BC%EB%A1%9C-%EC%B6%9C%EA%B7%BC-slack%EC%97%90%EC%84%9C-%ED%87%B4%EA%B7%BC-bf966aa9915d)

롯데의 기술 블로그 글을 보면 다양한 업무에서 슬랙이 필수적으로 자리잡고 있으며 롯데측과의 대화에서도 슬랙에 대한 대단한 자부심이 드러날 만큼 의존하고 있다. 

<aside>
💡 허들이란? 
허들은 슬랙의 음성통화 기능으로서 주로 온라인 회의를 진행할때 사용된다.

</aside>

## 🔧다양한 활용법

[오류 모니터링 자동화해 볼까요?](https://techblog.lotteon.com/%EB%B3%91%EC%95%84%EB%A6%AC-%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98-%EC%9D%B8%ED%84%B4-%EC%84%B1%EC%9E%A5%EA%B8%B0-36d19c66dac9)

다양한 앱을 연동해 슬랙 사용자간의 빠른 연동이 가능하며 봇을 사용해 운영중인 서버 상태의 대한 추적 및 감시가 가능하다.

# 앱 연동

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7a0f0c86-5bcd-4582-9e34-d047daaea2ef/Untitled.png)

슬랙에서 제공하는 다양한 앱을 연동함으로 슬랙 내부적으로 편리하고 빠르게 서비스에 접근이 가능합니다.

## 🛎️ 대표 서비스

### GitHub 서버

![추적하는 리포지토리 or 오가니제이션에 변경사항 발생시 알려주는 서비스](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d59b5d2-644e-453a-84fe-a2bb767d47ba/Untitled.png)

추적하는 리포지토리 or 오가니제이션에 변경사항 발생시 알려주는 서비스

### Zoom

![/zoom 명령어 하나로 줌방 즉시 생성부터 참여 탈퇴까지 가능하다.](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2056fa83-10a8-47be-937c-a98b0f19347e/Untitled.png)

/zoom 명령어 하나로 줌방 즉시 생성부터 참여 탈퇴까지 가능하다.

# 슬랙 봇

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/577ec1b3-a5c4-45e5-bc13-473f429bec0b/Untitled.png)

## 🤖슬랙 자체 제공 봇

간단하고 빠르게 생성 및 활용이 가능하지만 복잡한 기능을 넣을 수 없다는 단점이 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bf2c39c4-4f58-4660-97e3-41e40eacc1aa/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c87b727-0690-4d56-8464-bbd0f21c6b0f/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ade08db2-69f9-4c29-a00c-b1914d7e376b/Untitled.png)

---

## 🤖Spring Boot + Slack Bot

운영중인 시스템과 연동하여 시스템에서 발생하는 에러 등의 다양한 이벤트를 슬랙을 통해 받을 수 있다.

실제 롯데에서도 이러한 방식으로 사용중이다.

### 환경

Spring boot 2.3..1 

java 1.8 이상

Gradle

Bolt 1.23.0

**Bolt for Java** 는 최신 플랫폼 기능을 사용하여 Slack 앱을 빠르게 구축할 수 있도록 추상화 계층을 제공하는 JVM의 프레임워크입니다.

```java
dependencies **{
implementation 'com.slack.api:bolt-servlet:1.23.0'
}**
```

![application.properties](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ba688dbe-f4f7-46ac-b318-5a8a167fa3ee/Untitled.png)

application.properties

```java
// If you would like to run this app for a single workspace,
    // enabling this Bean factory should work for you.
     @Bean
    public AppConfig loadSingleWorkspaceAppConfig() {
        return AppConfig.builder()
                .singleTeamBotToken(System.getenv("SLACK_BOT_TOKEN"))
                .signingSecret(System.getenv("SLACK_SIGNING_SECRET"))
                .build();
    }
```

```java
// If you would like to run this app for multiple workspaces,
    // enabling this Bean factory should work for you.
    // @Bean
    public AppConfig loadOAuthConfig() {
        return AppConfig.builder()
                .singleTeamBotToken(null)
                .clientId(System.getenv("SLACK_CLIENT_ID"))
                .clientSecret(System.getenv("SLACK_CLIENT_SECRET"))
                .signingSecret(System.getenv("SLACK_SIGNING_SECRET"))
                .scope("app_mentions:read,channels:history,channels:read,chat:write")
                .oauthInstallPath("/slack/install")
                .oauthRedirectUriPath("/slack/oauth_redirect")
                .build();
    }
```

---

## 🧩 봇 활용

다양한 이벤트나 특정 조건에서 슬랙으로 메세지를 보내게함으로서 빠르고 쉽게 담당자에게 보고가 가능하다.

### 스케듈러를 활용

```java
@EnableScheduling
@Configuration
publicclassSlackScheduler {
privatefinalSlackService slackService;

publicSlackScheduler(SlackService slackService) {
this.slackService = slackService;
    }

//    @Scheduled(cron="0 0/1 * * * *") //1분
//    public void todayCocktail(){
//        slackService.postSlackMessage("안녕..");
//    }

@Scheduled(cron = "0 50 * * * ?")
publicvoidbreakTime(){
        slackService.postSlackMessage("쉬는 시간~^^");
    }

    @Scheduled(cron = "0 50 12 ? * MON-FRI")
publicvoidlaunchTime(){
        slackService.postSlackMessage("점심 시간~^^");
    }

}
```

(cron = “초 분 시 일 월 년”) 순서

### 서버 시작시

```java
@Component
publicclassStartConfigimplementsCommandLineRunner{
privatefinalSlackService slackService;

publicStartConfig(SlackService slackService) {
this.slackService = slackService;
    }
    @Override
publicvoidrun(String... args) {
        Date date =newDate(System.currentTimeMillis());

//구현 부분
slackService.postSlackMessage("Server started\n이벤트 발생 시간(timestamp) : "+date);
    }
}
```

CommandLineRunner는 main 메서드와 거의 같다고 이해하면 된다. 
(run 메서드)"스프링 애플리케이션"이 시작할 때 "1회"만 호출되는 점이 똑같고 다른 점은 
static이 아니라는 점에서 이득을 가져갈 수 있는 부분이 있다. argument도 main에 들어오는 것과 똑같다

### 서버 종료시

```java
@Component
public class ShutdownEventListener implements ApplicationListener<ContextClosedEvent> {

    private final SlackService slackService;

    public ShutdownEventListener(SlackService slackService) {
        this.slackService = slackService;
    }
    @Override
    public void onApplicationEvent(ContextClosedEvent event) {
        Date date = new Date(System.currentTimeMillis());

        slackService.postSlackMessage("Server shut down \n 이벤트 발생 시간(timestamp) : "+date);
    }
}
```

ApplicationListener는 이벤트와 관련이 있다. (onApplicationEvent 메서드)ApplicationEvent(추상클래스)를 상속받은 모든 이벤트들을 넣을 수 있다.
따라서 내가 ApplicationEvent를 상속받은 클래스를 만들어서 내가 만든 이벤트가 발생했을 때 호출될 메서드를 만들 수도 있는 것이다.
위의 예제에서는ContextClosedEvent를 감지하는 인터페이스를 구현했기 때문에,애플리케이션이 종료되기 직전에 1회만 호출된다.실무에서 graceful shutdown이 필요할 때 이 메서드를 구현하도록 하자. applicationListener에 ApplicationReadyEvent를 감지하도록 하면 애플리케이션이 온전히 실행되고나서 1회만 호출하도록 할 수 있다.

# 출처

> 자바용 슬랙 SDK
> 
> 
> [API Client Basics](https://slack.dev/java-slack-sdk/guides/web-api-basics)
> 
> 스프링 애플리케이션이 시작, 종료될 때 수행할 메서드 지정하는 방법
>
