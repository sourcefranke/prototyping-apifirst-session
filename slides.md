---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# some information about your slides (markdown enabled)
title: Rapid Prototyping and API-First
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# Ein Uhrzeit-Server

Mit Rapid Prototyping und API-First agil zum Ziel ;P

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2 items-baseline opacity-50">
    <span class="text-xs">(c) 2024 Lukas Tietenberg - v2024-08-21</span>
    <a href="https://github.com/sourcefranke/prototyping-apifirst-session" target="_blank" alt="GitHub" title="Open in GitHub"
        class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
        <carbon-logo-github />
    </a>
</div>

---

# Aufgabenstellung

<div class="flex justify-center items-center text-4xl" style="height: 80%;">
  <span style="width: 70%;">
    "[...] erstellen Sie in z.B. Java<br>
    für einen Server einen Endpunkt<br>
    der eine Uhrzeit zurückliefert. [...]"
  </span>
</div>

---

# Annahmen zur fachlichen Umsetzung
Was nicht explizit in der Aufgabenstellung stand

<div class="flex justify-center items-center text-2xl" style="height: 80%">

- Abfrage der Uhrzeit mittels HTTP-GET Request an REST-Schnittstelle
- Rückgabe der aktuellen Uhrzeit zum Zeitpunkt des Aufrufs
- Optionale Angabe von:
  - **Datumsformat** - Default Value: "*yyyy-MM-dd'T'HH:mm:ss*"
  - **Zeitzone** - Default Value: *Greenwich Mean Time (GMT)*

</div>


---

# Technologie-Stack

| ***Aspekt***                     | ***Entscheidung*** |
|----------------------------------|--------------------|
| Programmiersprache               | Java & Spring Boot |
| Build Tool                       | Maven              |
| API-Dokumentation & -Generierung | Swagger / OpenApi  |
| Quellcode Hosting                | GitHub             |
| Cloud-Plattform                  | Heroku             |
| Präsentation                     | Slidev             |

---

# REST-API
Definition als .yaml-Datei (Ausschnitt)

<div class="flex justify-center" style="overflow-y: scroll; width: 100%; height: 400px">

<div style="width: 100%;">

```yaml
paths:
  /time:
    get:
      tags:
        - time
      summary: Returns current time
      description: Request the current time. Therefore, you can optionally specify date format and time zone
      operationId: getTime
      parameters:
        - name: date-format
          in: query
          description: Specify format for date and/or time
          required: false
          explode: true
          schema:
            type: string
            default: yyyy-MM-dd'T'HH:mm:ss
        - name: time-zone
          in: query
          description: Specify the time zone
          required: false
          explode: true
          schema:
            type: string
            enum:
              - Africa/Abidjan
              - Africa/Accra
              - Asia/Macau
              - Australia/Broken_Hill
              - Europe/Berlin
              - GMT
              - US/Mountain
              - US/Pacific
              - UTC
              - Zulu
            default: GMT
      responses:
        '200':
          description: successful operation
          content:
            application/json:
              schema:
                type: object
                properties:
                  time:
                    type: string
                    description: The actual time value
                  date-format:
                    type: string
                    description: The date format used
                  time-zone:
                    type: string
                    description: The time zone used
        '400':
          description: Invalid tag value
```

</div>
</div>

---

# REST-API
Swagger UI

<div class="flex justify-center">
    <img width="60%" src="/rest.png" />
</div>


---

# Deployment

<div class="flex flex-col gap-5" style="height: 300px">
  <div class="flex justify-around">
    <img width="30%" src="/swagger.png">
    <img width="30%" src="/github_pages.png">
  </div>

  <div class="flex justify-around">
    <img width="14%" src="/spring-boot.png">
    <img width="15%" src="/heroku.png">
  </div>
</div>

<Arrow x1="405" y1="200" x2="570" y2="200" />
<Arrow x1="335" y1="400" x2="635" y2="400" />


---

# Implementierung
Maven "pom.xml" (Ausschnitt)

<div class="flex justify-center" style="overflow: scroll; width: 100%; height: 400px">

```xml
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

<div class="flex justify-around">
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
</div>


---

# Live-Demo
Eine simple HTML-Seite mit JavaScript als Client 

<iframe width=100% height=80% src="live-demo.html"></iframe>

---

# Links

- GitHub Repositories
  - **API-Definition:** *https://github.com/sourcefranke/clock-service-demo-api*
  - **Implementierung:** *https://github.com/sourcefranke/clock-service-demo-impl*
  - **Präsentation:** *https://github.com/sourcefranke/prototyping-apifirst-session*
- **Doku REST-API (Swagger UI):** *https://sourcefranke.github.io/clock-service-demo-api/*
