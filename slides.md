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
src: pages/rest.md
---

---
src: pages/implementierung.md
---

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
