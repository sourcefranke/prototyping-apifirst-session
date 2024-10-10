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

# Rapid Prototyping in Action
Ein Erfahrungsaustausch

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2 items-baseline opacity-50">
    <span class="text-xs">(c) 2024 Lukas Tietenberg - GPLv3 - v2024-10-12</span>
    <a href="https://github.com/sourcefranke/prototyping-apifirst-session" target="_blank" alt="GitHub" title="Open in GitHub"
        class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
        <carbon-logo-github />
    </a>
</div>


---
layout: section
---

## Letztens im Rahmen eines Bewerbungsgesprächs ...


---

# Programmieraufgabe

<div class="flex justify-center items-center text-4xl" style="height: 80%;">
  <span style="width: 70%;">
    "[...] erstellen Sie in z.B. Java<br>
    für einen Server einen Endpunkt<br>
    der eine Uhrzeit zurückliefert. [...]"
  </span>
</div>

---

# Annahmen zur Umsetzung
Was nicht explizit in der Aufgabenstellung stand

<div class="flex justify-center items-center text-2xl" style="height: 80%">

- Abfrage der Uhrzeit mittels HTTP-GET Request an REST-Schnittstelle
- Rückgabe der aktuellen Uhrzeit zum Zeitpunkt des Aufrufs
- Optionale Angabe von:
  - **Datumsformat** - Default Value: "*yyyy-MM-dd'T'HH:mm:ss*"
  - **Zeitzone** - Default Value: *Greenwich Mean Time (GMT)*

</div>


---

# REST-API

<div class="grid grid-cols-2 gap-6">
<div>

<h3>OpenAPI YML (Ausschnitt)</h3>

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
</div>
<div>

<h3>Swagger UI</h3>

<div class="flex justify-center">
    <img src="/rest.png" />
</div>

<span class="text-sm flex justify-end mt-2">
    <a>https://sourcefranke.github.io/clock-service-demo-api/</a>
</span>
</div>
</div>

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
layout: section
---

# Persönliche Erkenntnisse


---

# PDCA-Zyklus
Die Mutter aller agilen Vorgehensmodelle!

<div class="flex justify-center mt-10">

```mermaid {scale: 1.3}
flowchart LR
    Plan ---> Do ---> Check ---> Act ---> Plan
```
</div>

<div class="grid grid-cols-4 gap-4 mt-15">
  <div class="flex flex-col items-center">
    <h2>Plan</h2>
    <ul class="text-sm mt-3">
      <li>Nächste Schritte definieren</li>
    </ul>
  </div>
  <div class="flex flex-col items-center">
    <h2>Do</h2>
    <ul class="text-sm mt-3">
      <li>Schritte ausführen</li>
    </ul>
  </div>
  <div class="flex flex-col items-center">
    <h2>Check</h2>
    <ul class="text-sm mt-3">
      <li>Erfolg bewerten</li>
    </ul>
  </div>
  <div class="flex flex-col items-center">
    <h2>Act</h2>
    <ul class="text-sm mt-3">
      <li>Anpassungen definieren</li>
    </ul>
  </div>
</div>


---

# Vorgehen im Vergleich

<div class="grid grid-cols-2 gap-6 mt-20">
  <div class="flex flex-col items-center">
    <h2>Rapid Prototyping</h2>
    <ul class="mt-3">
      <li>Schnelles Herausfinden und Validieren von Anforderungen</li>
      <li>Kein Anspruch auf eine vollständige oder produktionsreife Software</li>
      <li>Mit bewusst wenig Aufwand etwas "Sichtbares" produzieren, um Feedback zu sammeln</li>
    </ul>
  </div>

  <div class="flex flex-col items-center">
    <h2>Scrum</h2>
    <ul class="mt-3">
      <li>Entwicklung und Auslieferung funktionsfähiger Software</li>
      <li>Möglichkeit der schnellen Reaktion auf veränderte Bedingungen oder Anforderungen während der Entwicklung</li>
    </ul>
  </div>
</div>

---

# Stufen von Prototypen
... ok, im Wesentlichen Frontend

<div class="grid grid-cols-2 gap-20 mt-12">
  <div class="flex flex-col items-center">
    <h3>1) Wireframe</h3>
    <ul class="text-sm mt-3">
      <li>Statische Skizzen für grundlegendes Layout und Struktur</li>
      <li><b><u>Tool</u>:</b> Figma</li>
    </ul>
  </div>
  <div class="flex flex-col items-center">
    <h3>2) Click-Dummy</h3>
    <ul class="text-sm mt-3">
      <li>Interaktive Klickmodelle, die Navigation simulieren, aber ohne Backend-Logik</li>
      <li><b><u>Tool</u>:</b> Figma</li>
    </ul>
  </div>
  <div class="flex flex-col items-center">
    <h3>3) Low Function</h3>
    <ul class="text-sm mt-3">
      <li>Teilweise funktionsfähige Modelle mit Basisfunktionalität</li>
      <li><b><u>Tool</u>:</b> HTML, JavaScript</li>
    </ul>
  </div>
  <div class="flex flex-col items-center">
    <h3>4) High Function</h3>
    <ul class="text-sm mt-3">
      <li>Detailgetreue Prototypen mit umfassender Interaktivität und echtem Design</li>
      <li><b><u>Tool</u>:</b> Angular / Vue / React, etc.</li>
    </ul>
  </div>
</div>

---
layout: section
---

# Und im Backend?

---

# Wir erinnern uns ...

<div class="flex justify-center h-100">
    <img src="/rest.png" />
</div>


---

# Ein komplett generierter Mock Server
Mit Prism im Handumdrehen ein Mock Server aus dem Boden gestampft

#### package.json
```json
{
  "name": "clock-service-demo-mock",
  "version": "1.0.0",
  "description": "A mock server using Prism for demo purposes",
  "scripts": {
    "deploy": "prism mock https://sourcefranke.github.io/clock-service-demo-api/swagger/api.yaml --port $PORT --host 0.0.0.0",
    "local": "prism mock https://sourcefranke.github.io/clock-service-demo-api/swagger/api.yaml --port 8080"
  },
  "dependencies": {
    "@stoplight/prism-cli": "^5.10.0"
  },
  "engines": {
    "node": ">=12.0.0"
  },
  "license": "MIT"
}
```

<span class="text-sm flex justify-end">
  <a>https://github.com/sourcefranke/clock-service-demo-mock</a>
</span>

---
layout: section
---

# Diskussion !!
