# Implementierung
Maven "pom.xml"

<div class="flex justify-center" style="overflow: scroll; width: 100%; height: 400px">

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.2</version>
    </parent>

    <groupId>com.github.sourcefranke</groupId>
    <artifactId>clock-service-demo-impl</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <java.version>21</java.version>
    </properties>

    <dependencies>
        <!--
        ##########
        # Spring #
        ##########
        -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--
        ###############
        # for CodeGen #
        ###############
        -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>javax.validation</groupId>
            <artifactId>validation-api</artifactId>
            <version>2.0.1.Final</version>
        </dependency>
        <dependency>
            <groupId>javax.annotation</groupId>
            <artifactId>javax.annotation-api</artifactId>
            <version>1.3.2</version>
        </dependency>
        <dependency>
            <groupId>org.openapitools</groupId>
            <artifactId>jackson-databind-nullable</artifactId>
            <version>0.2.6</version>
        </dependency>
        <dependency>
            <groupId>io.swagger.core.v3</groupId>
            <artifactId>swagger-annotations</artifactId>
            <version>2.2.22</version>
        </dependency>
        <dependency>
            <groupId>io.swagger.core.v3</groupId>
            <artifactId>swagger-models</artifactId>
            <version>2.2.22</version>
        </dependency>

        <!--
        ###############
        # Development #
        ###############
        -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>

        <!--
        ########
        # Test #
        ########
        -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.11.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-test</artifactId>
            <version>3.3.2</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.github.sourcefranke.clock_service_demo_impl.Application</mainClass>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.openapitools</groupId>
                <artifactId>openapi-generator-maven-plugin</artifactId>
                <version>7.7.0</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>generate</goal>
                        </goals>
                        <configuration>
                            <inputSpec>https://sourcefranke.github.io/clock-service-demo-api/swagger/api.yaml</inputSpec>
                            <generatorName>spring</generatorName>
                            <apiPackage>com.github.sourcefranke.clock_service_demo_impl.api</apiPackage>
                            <modelPackage>com.github.sourcefranke.clock_service_demo_impl.model</modelPackage>
                            <configOptions>
                                <sourceFolder>src/gen/java</sourceFolder>
                                <delegatePattern>true</delegatePattern>
                            </configOptions>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>

```

</div>


---

# Implementierung
Java

<div class="flex justify-center" style="width: 100%; height: 400px">

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

</div>


---

# Implementierung
Java

<div class="flex justify-center" style="overflow-y: scroll; width: 100%; height: 400px">

```java
@Component
@Slf4j
public class TimeApiDelegateImpl implements TimeApiDelegate {

    @Override
    public ResponseEntity<GetTime200Response> getTime(String dateFormat, String timeZone) {
        var time = currentTime(dateFormat, timeZone);
        var response = map(time, dateFormat, timeZone);
        log.info("GET /time?date-format={}&time-zone={}, Response: {}", dateFormat, timeZone, response);
        return ResponseEntity.ok(response);
    }

    private String currentTime(String dateFormat, String timeZone) {
        return LocalDateTime.now(ZoneId.of(timeZone))
                .format(DateTimeFormatter.ofPattern(dateFormat));
    }

    private GetTime200Response map(String time, String dateFormat, String timeZone) {
        return new GetTime200Response()
                .time(time)
                .dateFormat(dateFormat)
                .timeZone(timeZone);
    }
}
```

</div>


---

# Implementierung
Java

<div class="flex justify-center" style="width: 100%; height: 400px">

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**");
    }
}
```

</div>
