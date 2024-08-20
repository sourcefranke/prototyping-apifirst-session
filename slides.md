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
Definition File

<div class="flex justify-center" style="overflow-y: scroll; width: 100%; height: 400px">

<div style="width: 100%;">

```yaml
openapi: 3.0.3
info:
  title: Clock Service Demo
  description: |-
    This is an example API for demonstration purposes.
    
    I use this resource for a presentation about Rapid Prototyping in software development.
    
    The specification file for this API is hosted here: [**https://github.com/sourcefranke/clock-service-demo-api**](https://github.com/sourcefranke/clock-service-demo-api)
    
    The implementation code for this API is hosted here: [**https://github.com/sourcefranke/clock-service-demo-impl**](https://github.com/sourcefranke/clock-service-demo-impl)

  contact:
    email: lukas.tietenberg@outlook.de

  license:
    name: MIT
    url: https://github.com/sourcefranke/clock-service-demo-api?tab=MIT-1-ov-file

  version: 1.0.11

servers:
  - url: https://clock-service-demo-4210f5ad8b0c.herokuapp.com/
    description: productive service deployed on Heroku
  - url: http://localhost:8080
    description: if you start the service locally

tags:
  - name: time
    description: Take your time ;P

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
              - Africa/Addis_Ababa
              - Africa/Algiers
              - Africa/Asmara
              - Africa/Asmera
              - Africa/Bamako
              - Africa/Bangui
              - Africa/Banjul
              - Africa/Bissau
              - Africa/Blantyre
              - Africa/Brazzaville
              - Africa/Bujumbura
              - Africa/Cairo
              - Africa/Casablanca
              - Africa/Ceuta
              - Africa/Conakry
              - Africa/Dakar
              - Africa/Dar_es_Salaam
              - Africa/Djibouti
              - Africa/Douala
              - Africa/El_Aaiun
              - Africa/Freetown
              - Africa/Gaborone
              - Africa/Harare
              - Africa/Johannesburg
              - Africa/Juba
              - Africa/Kampala
              - Africa/Khartoum
              - Africa/Kigali
              - Africa/Kinshasa
              - Africa/Lagos
              - Africa/Libreville
              - Africa/Lome
              - Africa/Luanda
              - Africa/Lubumbashi
              - Africa/Lusaka
              - Africa/Malabo
              - Africa/Maputo
              - Africa/Maseru
              - Africa/Mbabane
              - Africa/Mogadishu
              - Africa/Monrovia
              - Africa/Nairobi
              - Africa/Ndjamena
              - Africa/Niamey
              - Africa/Nouakchott
              - Africa/Ouagadougou
              - Africa/Porto-Novo
              - Africa/Sao_Tome
              - Africa/Timbuktu
              - Africa/Tripoli
              - Africa/Tunis
              - Africa/Windhoek
              - America/Adak
              - America/Anchorage
              - America/Anguilla
              - America/Antigua
              - America/Araguaina
              - America/Argentina/Buenos_Aires
              - America/Argentina/Catamarca
              - America/Argentina/ComodRivadavia
              - America/Argentina/Cordoba
              - America/Argentina/Jujuy
              - America/Argentina/La_Rioja
              - America/Argentina/Mendoza
              - America/Argentina/Rio_Gallegos
              - America/Argentina/Salta
              - America/Argentina/San_Juan
              - America/Argentina/San_Luis
              - America/Argentina/Tucuman
              - America/Argentina/Ushuaia
              - America/Aruba
              - America/Asuncion
              - America/Atikokan
              - America/Atka
              - America/Bahia
              - America/Bahia_Banderas
              - America/Barbados
              - America/Belem
              - America/Belize
              - America/Blanc-Sablon
              - America/Boa_Vista
              - America/Bogota
              - America/Boise
              - America/Buenos_Aires
              - America/Cambridge_Bay
              - America/Campo_Grande
              - America/Cancun
              - America/Caracas
              - America/Catamarca
              - America/Cayenne
              - America/Cayman
              - America/Chicago
              - America/Chihuahua
              - America/Coral_Harbour
              - America/Cordoba
              - America/Costa_Rica
              - America/Creston
              - America/Cuiaba
              - America/Curacao
              - America/Danmarkshavn
              - America/Dawson
              - America/Dawson_Creek
              - America/Denver
              - America/Detroit
              - America/Dominica
              - America/Edmonton
              - America/Eirunepe
              - America/El_Salvador
              - America/Ensenada
              - America/Fort_Nelson
              - America/Fort_Wayne
              - America/Fortaleza
              - America/Glace_Bay
              - America/Godthab
              - America/Goose_Bay
              - America/Grand_Turk
              - America/Grenada
              - America/Guadeloupe
              - America/Guatemala
              - America/Guayaquil
              - America/Guyana
              - America/Halifax
              - America/Havana
              - America/Hermosillo
              - America/Indiana/Indianapolis
              - America/Indiana/Knox
              - America/Indiana/Marengo
              - America/Indiana/Petersburg
              - America/Indiana/Tell_City
              - America/Indiana/Vevay
              - America/Indiana/Vincennes
              - America/Indiana/Winamac
              - America/Indianapolis
              - America/Inuvik
              - America/Iqaluit
              - America/Jamaica
              - America/Jujuy
              - America/Juneau
              - America/Kentucky/Louisville
              - America/Kentucky/Monticello
              - America/Knox_IN
              - America/Kralendijk
              - America/La_Paz
              - America/Lima
              - America/Los_Angeles
              - America/Louisville
              - America/Lower_Princes
              - America/Maceio
              - America/Managua
              - America/Manaus
              - America/Marigot
              - America/Martinique
              - America/Matamoros
              - America/Mazatlan
              - America/Mendoza
              - America/Menominee
              - America/Merida
              - America/Metlakatla
              - America/Mexico_City
              - America/Miquelon
              - America/Moncton
              - America/Monterrey
              - America/Montevideo
              - America/Montreal
              - America/Montserrat
              - America/Nassau
              - America/New_York
              - America/Nipigon
              - America/Nome
              - America/Noronha
              - America/North_Dakota/Beulah
              - America/North_Dakota/Center
              - America/North_Dakota/New_Salem
              - America/Nuuk
              - America/Ojinaga
              - America/Panama
              - America/Pangnirtung
              - America/Paramaribo
              - America/Phoenix
              - America/Port-au-Prince
              - America/Port_of_Spain
              - America/Porto_Acre
              - America/Porto_Velho
              - America/Puerto_Rico
              - America/Punta_Arenas
              - America/Rainy_River
              - America/Rankin_Inlet
              - America/Recife
              - America/Regina
              - America/Resolute
              - America/Rio_Branco
              - America/Rosario
              - America/Santa_Isabel
              - America/Santarem
              - America/Santiago
              - America/Santo_Domingo
              - America/Sao_Paulo
              - America/Scoresbysund
              - America/Shiprock
              - America/Sitka
              - America/St_Barthelemy
              - America/St_Johns
              - America/St_Kitts
              - America/St_Lucia
              - America/St_Thomas
              - America/St_Vincent
              - America/Swift_Current
              - America/Tegucigalpa
              - America/Thule
              - America/Thunder_Bay
              - America/Tijuana
              - America/Toronto
              - America/Tortola
              - America/Vancouver
              - America/Virgin
              - America/Whitehorse
              - America/Winnipeg
              - America/Yakutat
              - America/Yellowknife
              - Antarctica/Casey
              - Antarctica/Davis
              - Antarctica/DumontDUrville
              - Antarctica/Macquarie
              - Antarctica/Mawson
              - Antarctica/McMurdo
              - Antarctica/Palmer
              - Antarctica/Rothera
              - Antarctica/South_Pole
              - Antarctica/Syowa
              - Antarctica/Troll
              - Antarctica/Vostok
              - Arctic/Longyearbyen
              - Asia/Aden
              - Asia/Almaty
              - Asia/Amman
              - Asia/Anadyr
              - Asia/Aqtau
              - Asia/Aqtobe
              - Asia/Ashgabat
              - Asia/Ashkhabad
              - Asia/Atyrau
              - Asia/Baghdad
              - Asia/Bahrain
              - Asia/Baku
              - Asia/Bangkok
              - Asia/Barnaul
              - Asia/Beirut
              - Asia/Bishkek
              - Asia/Brunei
              - Asia/Calcutta
              - Asia/Chita
              - Asia/Choibalsan
              - Asia/Chongqing
              - Asia/Chungking
              - Asia/Colombo
              - Asia/Dacca
              - Asia/Damascus
              - Asia/Dhaka
              - Asia/Dili
              - Asia/Dubai
              - Asia/Dushanbe
              - Asia/Famagusta
              - Asia/Gaza
              - Asia/Harbin
              - Asia/Hebron
              - Asia/Ho_Chi_Minh
              - Asia/Hong_Kong
              - Asia/Hovd
              - Asia/Irkutsk
              - Asia/Istanbul
              - Asia/Jakarta
              - Asia/Jayapura
              - Asia/Jerusalem
              - Asia/Kabul
              - Asia/Kamchatka
              - Asia/Karachi
              - Asia/Kashgar
              - Asia/Kathmandu
              - Asia/Katmandu
              - Asia/Khandyga
              - Asia/Kolkata
              - Asia/Krasnoyarsk
              - Asia/Kuala_Lumpur
              - Asia/Kuching
              - Asia/Kuwait
              - Asia/Macao
              - Asia/Macau
              - Asia/Magadan
              - Asia/Makassar
              - Asia/Manila
              - Asia/Muscat
              - Asia/Nicosia
              - Asia/Novokuznetsk
              - Asia/Novosibirsk
              - Asia/Omsk
              - Asia/Oral
              - Asia/Phnom_Penh
              - Asia/Pontianak
              - Asia/Pyongyang
              - Asia/Qatar
              - Asia/Qostanay
              - Asia/Qyzylorda
              - Asia/Rangoon
              - Asia/Riyadh
              - Asia/Saigon
              - Asia/Sakhalin
              - Asia/Samarkand
              - Asia/Seoul
              - Asia/Shanghai
              - Asia/Singapore
              - Asia/Srednekolymsk
              - Asia/Taipei
              - Asia/Tashkent
              - Asia/Tbilisi
              - Asia/Tehran
              - Asia/Tel_Aviv
              - Asia/Thimbu
              - Asia/Thimphu
              - Asia/Tokyo
              - Asia/Tomsk
              - Asia/Ujung_Pandang
              - Asia/Ulaanbaatar
              - Asia/Ulan_Bator
              - Asia/Urumqi
              - Asia/Ust-Nera
              - Asia/Vientiane
              - Asia/Vladivostok
              - Asia/Yakutsk
              - Asia/Yangon
              - Asia/Yekaterinburg
              - Asia/Yerevan
              - Atlantic/Azores
              - Atlantic/Bermuda
              - Atlantic/Canary
              - Atlantic/Cape_Verde
              - Atlantic/Faeroe
              - Atlantic/Faroe
              - Atlantic/Jan_Mayen
              - Atlantic/Madeira
              - Atlantic/Reykjavik
              - Atlantic/South_Georgia
              - Atlantic/St_Helena
              - Atlantic/Stanley
              - Australia/ACT
              - Australia/Adelaide
              - Australia/Brisbane
              - Australia/Broken_Hill
              - Australia/Canberra
              - Australia/Currie
              - Australia/Darwin
              - Australia/Eucla
              - Australia/Hobart
              - Australia/LHI
              - Australia/Lindeman
              - Australia/Lord_Howe
              - Australia/Melbourne
              - Australia/NSW
              - Australia/North
              - Australia/Perth
              - Australia/Queensland
              - Australia/South
              - Australia/Sydney
              - Australia/Tasmania
              - Australia/Victoria
              - Australia/West
              - Australia/Yancowinna
              - Brazil/Acre
              - Brazil/DeNoronha
              - Brazil/East
              - Brazil/West
              - CET
              - CST6CDT
              - Canada/Atlantic
              - Canada/Central
              - Canada/Eastern
              - Canada/Mountain
              - Canada/Newfoundland
              - Canada/Pacific
              - Canada/Saskatchewan
              - Canada/Yukon
              - Chile/Continental
              - Chile/EasterIsland
              - Cuba
              - EET
              - EST5EDT
              - Egypt
              - Eire
              - Etc/GMT
              - Etc/GMT+0
              - Etc/GMT+1
              - Etc/GMT+10
              - Etc/GMT+11
              - Etc/GMT+12
              - Etc/GMT+2
              - Etc/GMT+3
              - Etc/GMT+4
              - Etc/GMT+5
              - Etc/GMT+6
              - Etc/GMT+7
              - Etc/GMT+8
              - Etc/GMT+9
              - Etc/GMT-0
              - Etc/GMT-1
              - Etc/GMT-10
              - Etc/GMT-11
              - Etc/GMT-12
              - Etc/GMT-13
              - Etc/GMT-14
              - Etc/GMT-2
              - Etc/GMT-3
              - Etc/GMT-4
              - Etc/GMT-5
              - Etc/GMT-6
              - Etc/GMT-7
              - Etc/GMT-8
              - Etc/GMT-9
              - Etc/GMT0
              - Etc/Greenwich
              - Etc/UCT
              - Etc/UTC
              - Etc/Universal
              - Etc/Zulu
              - Europe/Amsterdam
              - Europe/Andorra
              - Europe/Astrakhan
              - Europe/Athens
              - Europe/Belfast
              - Europe/Belgrade
              - Europe/Berlin
              - Europe/Bratislava
              - Europe/Brussels
              - Europe/Bucharest
              - Europe/Budapest
              - Europe/Busingen
              - Europe/Chisinau
              - Europe/Copenhagen
              - Europe/Dublin
              - Europe/Gibraltar
              - Europe/Guernsey
              - Europe/Helsinki
              - Europe/Isle_of_Man
              - Europe/Istanbul
              - Europe/Jersey
              - Europe/Kaliningrad
              - Europe/Kiev
              - Europe/Kirov
              - Europe/Lisbon
              - Europe/Ljubljana
              - Europe/London
              - Europe/Luxembourg
              - Europe/Madrid
              - Europe/Malta
              - Europe/Mariehamn
              - Europe/Minsk
              - Europe/Monaco
              - Europe/Moscow
              - Europe/Nicosia
              - Europe/Oslo
              - Europe/Paris
              - Europe/Podgorica
              - Europe/Prague
              - Europe/Riga
              - Europe/Rome
              - Europe/Samara
              - Europe/San_Marino
              - Europe/Sarajevo
              - Europe/Saratov
              - Europe/Simferopol
              - Europe/Skopje
              - Europe/Sofia
              - Europe/Stockholm
              - Europe/Tallinn
              - Europe/Tirane
              - Europe/Tiraspol
              - Europe/Ulyanovsk
              - Europe/Uzhgorod
              - Europe/Vaduz
              - Europe/Vatican
              - Europe/Vienna
              - Europe/Vilnius
              - Europe/Volgograd
              - Europe/Warsaw
              - Europe/Zagreb
              - Europe/Zaporozhye
              - Europe/Zurich
              - GB
              - GB-Eire
              - GMT
              - GMT0
              - Greenwich
              - Hongkong
              - Iceland
              - Indian/Antananarivo
              - Indian/Chagos
              - Indian/Christmas
              - Indian/Cocos
              - Indian/Comoro
              - Indian/Kerguelen
              - Indian/Mahe
              - Indian/Maldives
              - Indian/Mauritius
              - Indian/Mayotte
              - Indian/Reunion
              - Iran
              - Israel
              - Jamaica
              - Japan
              - Kwajalein
              - Libya
              - MET
              - MST7MDT
              - Mexico/BajaNorte
              - Mexico/BajaSur
              - Mexico/General
              - NZ
              - NZ-CHAT
              - Navajo
              - PRC
              - PST8PDT
              - Pacific/Apia
              - Pacific/Auckland
              - Pacific/Bougainville
              - Pacific/Chatham
              - Pacific/Chuuk
              - Pacific/Easter
              - Pacific/Efate
              - Pacific/Enderbury
              - Pacific/Fakaofo
              - Pacific/Fiji
              - Pacific/Funafuti
              - Pacific/Galapagos
              - Pacific/Gambier
              - Pacific/Guadalcanal
              - Pacific/Guam
              - Pacific/Honolulu
              - Pacific/Johnston
              - Pacific/Kanton
              - Pacific/Kiritimati
              - Pacific/Kosrae
              - Pacific/Kwajalein
              - Pacific/Majuro
              - Pacific/Marquesas
              - Pacific/Midway
              - Pacific/Nauru
              - Pacific/Niue
              - Pacific/Norfolk
              - Pacific/Noumea
              - Pacific/Pago_Pago
              - Pacific/Palau
              - Pacific/Pitcairn
              - Pacific/Pohnpei
              - Pacific/Ponape
              - Pacific/Port_Moresby
              - Pacific/Rarotonga
              - Pacific/Saipan
              - Pacific/Samoa
              - Pacific/Tahiti
              - Pacific/Tarawa
              - Pacific/Tongatapu
              - Pacific/Truk
              - Pacific/Wake
              - Pacific/Wallis
              - Pacific/Yap
              - Poland
              - Portugal
              - ROK
              - Singapore
              - SystemV/AST4
              - SystemV/AST4ADT
              - SystemV/CST6
              - SystemV/CST6CDT
              - SystemV/EST5
              - SystemV/EST5EDT
              - SystemV/HST10
              - SystemV/MST7
              - SystemV/MST7MDT
              - SystemV/PST8
              - SystemV/PST8PDT
              - SystemV/YST9
              - SystemV/YST9YDT
              - Turkey
              - UCT
              - US/Alaska
              - US/Aleutian
              - US/Arizona
              - US/Central
              - US/East-Indiana
              - US/Eastern
              - US/Hawaii
              - US/Indiana-Starke
              - US/Michigan
              - US/Mountain
              - US/Pacific
              - US/Samoa
              - UTC
              - Universal
              - W-SU
              - WET
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
    <img width="60%" src="/REST.PNG" />
</div>


---

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


---

# Deployment

<div class="flex flex-col gap-5" style="height: 300px">
  <div class="flex justify-around">
    <img width="30%" src="/swagger.PNG">
    <img width="30%" src="/github_pages.PNG">
  </div>
  
  <div class="flex justify-around">
    <img width="14%" src="/spring-boot.PNG">
    <img width="15%" src="/heroku.PNG">
  </div>
</div>

<Arrow x1="405" y1="200" x2="570" y2="200" />
<Arrow x1="335" y1="400" x2="635" y2="400" />

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
