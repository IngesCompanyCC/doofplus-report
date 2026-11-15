# Capítulo V: Product Implementation, Validation & Deployment

## 5.1. Software Configuration Management

En esta sección se describen las decisiones, convenciones y herramientas utilizadas por el equipo IngesCompanyCC para gestionar el ciclo de vida, la implementación, la validación y el despliegue de **DoofPlus**. Estas decisiones permiten mantener la consistencia y la trazabilidad de los cambios realizados en cada sprint sobre el Website, la Frontend Web Application y los RESTful Web Services.

### 5.1.1. Software Development Environment Configuration

A continuación se detallan los productos de software que los miembros del equipo utilizan para colaborar en el ciclo de vida de DoofPlus, indicando su propósito y su ruta de referencia (productos SaaS) o de descarga (productos que se ejecutan en el computador de cada integrante).

* **Project Management**
    * **Jira Software:** Gestión del Product Backlog, planificación de sprints y seguimiento de tareas en el board del proyecto. (Referencia: https://www.atlassian.com/software/jira)
    * **Discord:** Canal de comunicación del equipo para las reuniones de Sprint Planning, Sprint Review y coordinación diaria. (Referencia: https://discord.com)
* **Requirements Management**
    * **Jira Software:** Registro de User Stories y Technical Stories con sus Story Points y criterios de aceptación. (Referencia: https://www.atlassian.com/software/jira)
    * **Gherkin:** Redacción de criterios de aceptación con la estructura Given-When-Then. (Referencia: https://cucumber.io/docs/gherkin/reference)
    * **Miro:** Elaboración del Big Picture EventStorming y del Design-Level EventStorming. (Referencia: https://miro.com)
* **Product UX/UI Design**
    * **Figma:** Elaboración de Wireframes, Mock-ups y Prototypes del Website y la Web Application. (Referencia: https://www.figma.com)
    * **FigJam:** Elaboración de los Wireflows y User Flows de la Web Application. (Referencia: https://www.figma.com/figjam)
    * **UXPressia:** Elaboración de User Personas, Empathy Maps, Journey Maps e Impact Maps. (Referencia: https://uxpressia.com)
    * **Structurizr DSL:** Elaboración de los diagramas C4 de contexto, contenedores y componentes bajo el enfoque Diagram-as-Code. (Referencia: https://structurizr.com)
    * **Mermaid:** Elaboración de los diagramas de clases y de base de datos bajo el enfoque Diagram-as-Code. (Referencia: https://mermaid.js.org)
* **Software Development**
    * **Git:** Sistema de control de versiones distribuido utilizado en todos los repositorios. (Descarga: https://git-scm.com/downloads)
    * **GitHub:** Plataforma de alojamiento de los repositorios de la organización y de colaboración mediante ramas y Pull Requests. (Referencia: https://github.com/IngesCompanyCC)
    * **Visual Studio Code y WebStorm:** Editores para el desarrollo del Website (HTML5, CSS3 y JavaScript) y de la Frontend Web Application en Vue 3. (Descarga: https://code.visualstudio.com/download y https://www.jetbrains.com/webstorm/download)
    * **JetBrains Rider:** IDE para el desarrollo de los RESTful Web Services en C# con ASP.NET Core. (Descarga: https://www.jetbrains.com/rider/download)
    * **Node.js y npm:** Entorno de ejecución y gestor de paquetes requeridos por Vite y json-server. (Descarga: https://nodejs.org/en/download)
    * **Vite y Vue 3:** Herramienta de build y framework de JavaScript (Composition API) para la Frontend Web Application, con **PrimeVue** y **PrimeFlex** para los componentes de interfaz, **Pinia** para el estado, **Vue Router** para la navegación y **Vue I18n** para la internacionalización. (Referencia: https://vuejs.org)
    * **ASP.NET Core y Entity Framework Core:** Framework de C# y ORM para el desarrollo de los RESTful Web Services. (Referencia: https://dotnet.microsoft.com/apps/aspnet)
    * **MySQL:** Sistema gestor de base de datos relacional (MySQL 8). (Descarga: https://dev.mysql.com/downloads)
* **Software Testing**
    * **Chrome DevTools:** Inspección del diseño responsive y depuración del Website y la Web Application. (Referencia: https://developer.chrome.com/docs/devtools)
    * **json-server:** Fake API que simuló los endpoints REST desde la Web Application hasta el Sprint 3, mientras se implementaban los Web Services; desde la versión `v1.2.0` la Web Application ya no la usa. (Referencia: https://github.com/typicode/json-server)
    * **Scalar:** Ejecución de pruebas sobre los endpoints documentados de los RESTful Web Services. (Referencia: https://scalar.com)
* **Software Documentation**
    * **Markdown en GitHub:** Redacción del informe del proyecto bajo el enfoque Docs-as-Code en el repositorio del informe. (Referencia: https://www.markdownguide.org)
    * **OpenAPI:** Documentación de los RESTful Web Services. (Referencia: https://spec.openapis.org/oas/latest.html)
* **Software Deployment**
    * **GitHub Pages:** Publicación del Website. (Referencia: https://pages.github.com)
    * **Firebase Hosting:** Publicación de la Frontend Web Application. (Referencia: https://firebase.google.com/docs/hosting)
    * **Render:** Publicación de los RESTful Web Services. (Referencia: https://render.com)
    * **Railway:** Base de datos MySQL gestionada en la nube. (Referencia: https://railway.com)

### 5.1.2. Source Code Management

El equipo utiliza **GitHub** como plataforma y **Git** como sistema de control de versiones. Todos los repositorios pertenecen a la organización IngesCompanyCC: https://github.com/IngesCompanyCC:

| Producto | Repositorio |
|----------|-------------|
| Website | https://github.com/IngesCompanyCC/doofplus-website |
| Frontend Web Application | https://github.com/IngesCompanyCC/doofplus-web-application |
| RESTful Web Services | https://github.com/IngesCompanyCC/doofplus-platform |
| Informe del proyecto | https://github.com/IngesCompanyCC/doofplus-report |

**GitFlow Workflow**

El equipo aplica el modelo GitFlow propuesto por Vincent Driessen en *A successful Git branching model*:

* **`main`:** Contiene únicamente versiones estables listas para producción. Solo recibe merges desde ramas `release/*` y `hotfix/*`, y cada merge se etiqueta con su versión (por ejemplo, `v1.0.0`).
* **`develop`:** Rama de integración. Consolida las funcionalidades terminadas antes de preparar una nueva versión.
* **Feature branches:** Nacen de `develop` y regresan a `develop` mediante Pull Request. Convención: `feature/<nombre-en-kebab-case>`, nombrando el bounded context o la funcionalidad (por ejemplo, `feature/shared`, `feature/manufacturing`, `feature/project-configuration`). En el repositorio del informe se usa `feature/chapter<n>`.
* **Release branches:** Nacen de `develop` cuando un incremento está listo para entregarse y se fusionan con `main` y `develop`. Convención: `release/v<MAJOR>.<MINOR>.<PATCH>`, admitiendo el sufijo de pre-release `-rc.<n>` (por ejemplo, `release/v1.0.2`, usada en el Website y en la Web Application).
* **Hotfix branches:** Nacen de `main` para corregir errores críticos en producción y se fusionan con `main` y `develop`. Convención: `hotfix/v<MAJOR>.<MINOR>.<PATCH>` (por ejemplo, `hotfix/v1.0.3`).

**Semantic Versioning**

Las releases se nombran según **Semantic Versioning 2.0.0** con el formato `MAJOR.MINOR.PATCH`:

* **MAJOR:** Cambios incompatibles con versiones anteriores (por ejemplo, `v2.0.0`).
* **MINOR:** Nuevas funcionalidades compatibles con la versión anterior (por ejemplo, `v1.1.0`).
* **PATCH:** Correcciones de errores compatibles (por ejemplo, `v1.0.1`).

**Conventional Commits**

Los mensajes de commit siguen la especificación **Conventional Commits 1.0.0**:

```
<type>(<optional scope>): <description>

<optional body>

<optional footer(s)>
```

* **type:** `feat` (nueva funcionalidad), `fix` (corrección de errores), `docs` (documentación), `style` (formato sin cambios de lógica), `refactor` (reestructuración sin cambio de comportamiento), `test` (pruebas), `build` (sistema de build o dependencias), `ci` (integración continua), `chore` (tareas de mantenimiento).
* **scope:** Módulo o bounded context afectado, por ejemplo `feat(lots): ...` o `docs(chapter5): ...`.
* **description:** Resumen breve en inglés, en modo imperativo y en minúsculas.
* **body y footer:** Detalle del cambio y referencias a tareas; los cambios incompatibles se marcan con `BREAKING CHANGE:` o con `!` después del type.

### 5.1.3. Source Code Style Guide & Conventions

Toda la nomenclatura del código fuente (archivos, clases, variables, métodos y comentarios) se escribe en **inglés**, respetando el Ubiquitous Language del dominio de calidad farmacéutica (por ejemplo, `ProductionBatch`, `Deviation`, `CapaAction`). Las convenciones adoptadas por lenguaje son las siguientes:

* **HTML:** HTML Style Guide and Coding Conventions (W3Schools): https://www.w3schools.com/html/html5_syntax.asp y Google HTML/CSS Style Guide: https://google.github.io/styleguide/htmlcssguide.html. Etiquetas y atributos en minúsculas, valores de atributos entre comillas dobles, uso de etiquetas semánticas (`header`, `main`, `section`, `footer`) y atributo `alt` en todas las imágenes.
* **CSS:** Google HTML/CSS Style Guide: https://google.github.io/styleguide/htmlcssguide.html. Indentación de 2 espacios, nombres de clases en kebab-case y selectores cortos; los ids no se usan para estilos.
* **JavaScript:** Google JavaScript Style Guide: https://google.github.io/styleguide/jsguide.html. Uso de `const`/`let`, lowerCamelCase para variables y funciones y punto y coma al final de cada sentencia.
* **Vue:** Vue Style Guide: https://vuejs.org/style-guide/. Componentes de un solo archivo (`.vue`) con nombres en kebab-case, `<script setup>` con Composition API, props tipadas y organización del código por bounded context (`domain`, `infrastructure`, `application`, `presentation`).
* **C#:** C# Coding Conventions (Microsoft): https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions. PascalCase para clases, métodos y propiedades, camelCase para variables locales y parámetros, y prefijo `I` en las interfaces.
* **ASP.NET Core:** ASP.NET Core Web API (Microsoft): https://learn.microsoft.com/aspnet/core/web-api. Configuración externalizada en `appsettings.json` y variables de entorno, y controladores RESTful con rutas en plural y kebab-case (por ejemplo, `/api/v1/production-batches`).
* **Gherkin:** Gherkin Conventions for Readable Specifications: https://specflow.org/gherkin/gherkin-conventions-for-readable-specifications. Palabras clave `Feature`, `Scenario`, `Given`, `When`, `Then` en inglés, un comportamiento por escenario y uso de `Scenario Outline` con `Examples` para casos basados en datos.

### 5.1.4. Software Deployment Configuration

A continuación se describen los pasos para desplegar cada producto de la solución a partir de su repositorio de código fuente:

1. **Website (GitHub Pages)**
    1. Fusionar la rama de release en `main` y etiquetar la versión.
    2. En el repositorio `doofplus-website`, ingresar a *Settings > Pages* y seleccionar *Deploy from a branch* con la rama `main` y la carpeta `/ (root)`.
    3. GitHub Pages publica el sitio en https://ingescompanycc.github.io/doofplus-website/ y lo vuelve a publicar con cada push a `main`.
2. **Frontend Web Application (Firebase Hosting)**
    1. Registrar en `.env.production` la URL base de los RESTful Web Services publicados (`VITE_PLATFORM_API_URL=https://doofplus-platform-aw.onrender.com/api/v1`) y las rutas de cada endpoint. Desde la versión `v1.2.0`, todos los bounded contexts usan ese API y la Web Application ya no necesita la Fake API.
    2. Instalar Firebase CLI (`npm install -g firebase-tools`), iniciar sesión con `firebase login` y seleccionar el proyecto `ingescompanycc-frontend` con `firebase use --add`. El repositorio incluye `firebase.json`, que publica la carpeta `dist` como Single Page Application.
    3. Compilar la versión de producción con `npm run build` (Vite).
    4. Publicar con `firebase deploy --only hosting`.
3. **RESTful Web Services (Render y Railway)**
    1. Crear la base de datos MySQL en Railway y obtener la cadena de conexión y las credenciales.
    2. Crear un Web Service en Render vinculado a la rama `main` del repositorio de Web Services, con el Dockerfile del proyecto ASP.NET Core.
    3. Registrar en Render las variables de entorno de producción (cadena de conexión de la base de datos, secreto JWT, credenciales de la pasarela de pagos Niubiz, API key de SendGrid y API key de los dispositivos de ThingsBoard).
    4. Render compila y publica el servicio con cada push a `main`; la documentación OpenAPI queda disponible en la ruta `/scalar` del servicio.

## 5.2. Landing Page, Services & Applications Implementation

En esta sección se explica y evidencia el proceso de implementación, pruebas, documentación y despliegue de **DoofPlus**, incluyendo el Website, la Frontend Web Application y los RESTful Web Services. El avance se presenta organizado por Sprints a partir del Product Backlog.

### 5.2.1. Sprint 1

En esta sección se registra y explica el avance en términos de producto y trabajo colaborativo para el **Sprint 1**, cuyo enfoque fue configurar los repositorios de la organización IngesCompanyCC, construir la base del Website de DoofPlus y configurar el proyecto de la Frontend Web Application.

#### 5.2.1.1. Sprint Planning 1

El Sprint Planning Meeting sirvió para definir los objetivos iniciales, asignar responsabilidades y seleccionar las primeras User Stories del Website y las tareas técnicas de configuración de la Web Application. A continuación, se presenta el resumen de la reunión de planificación:

| Sprint # | Sprint 1 |
|----------|----------|
| **Sprint Planning Background** | |
| **Date** | 2026-09-30 |
| **Time** | 10:00 AM |
| **Location** | Reunión virtual vía Discord |
| **Prepared By** | Zavaleta Gutierrez, Rodolfo Martin |
| **Attendees (to planning meeting)** | Angulo Ramírez, Marcelo Martín / Cobades Zamora, Yhoshua Hebert / Rojas Ambicho, Nestor Daniel / Yarleque Ruiz, Cristina Marcela / Zavaleta Gutierrez, Rodolfo Martin |
| **Sprint 0 Review Summary** | No aplica por ser el primer Sprint del proyecto. |
| **Sprint 0 Retrospective Summary** | No aplica por ser el primer Sprint del proyecto. |
| **Sprint Goal & User Stories** | |
| **Sprint 1 Goal** | **Our focus is on** setting up the IngesCompanyCC repositories, the base of the DoofPlus Website and the configuration of the DoofPlus Web Application.<br>**We believe it delivers** a first bilingual entry point that presents DoofPlus to QA/QC specialists and production supervisors of pharmaceutical laboratories, and a ready project to build the Web Application.<br>**This will be confirmed when** visitors can use the navigation bar, read the value proposition in the hero and switch between English and Spanish, and the Web Application project runs with PrimeVue, Vue I18n and the Fake API. |
| **Sprint 1 Velocity** | 10 Story Points |
| **Sum of Story Points** | 10 Story Points (US44: 3, US01: 2, US46: 5) |

#### 5.2.1.2. Aspect Leaders and Collaborators

En el Sprint 1 se consideraron cuatro aspectos: la base del Website, la configuración de la Web Application, los repositorios y la documentación del sprint. A continuación se presenta el Leadership-and-Collaboration Matrix (LACX), que indica quién es el líder (L) y quiénes son los colaboradores (C) en cada aspecto:

| Team Member (Last Name, First Name) | GitHub Username | Website | Web Application Setup | Repositories | Documentation |
|-------------------------------------|-----------------|:---:|:---:|:---:|:---:|
| Angulo Ramírez, Marcelo Martín | Zock2005 | C | | | C |
| Cobades Zamora, Yhoshua Hebert | YhoshuaCZ | C | C | C | |
| Rojas Ambicho, Nestor Daniel | danRO-20 | C | | | C |
| Yarleque Ruiz, Cristina Marcela | Cris06luna | | | | L |
| Zavaleta Gutierrez, Rodolfo Martin | gutierrezrodolfo360-bit | L | L | L | C |

#### 5.2.1.3. Sprint Backlog 1

El objetivo de este Sprint fue construir la base del Website y dejar configurado el proyecto de la Web Application. Las tareas se registraron en el board del Sprint 1 en Jira Software.

![Sprint 1 en Jira Software: actividades finalizadas y responsables](../assets/img/chapter5/evidencia/jira-sprint1.png)

**Enlace al board en Jira:** https://inges-company-cc.atlassian.net/jira/software/projects/UPC/boards/1

| Sprint # | Sprint 1 | | | | | | |
|----------|----------|---|---|---|---|---|---|
| **User Story** | | **Work-Item / Task** | | | | | |
| **Id** | **Title** | **Id** | **Title** | **Description** | **Estimation (Hours)** | **Assigned To** | **Status (To-do / In-Process / To-Review / Done)** |
| US44 | Navegación por secciones | T001 | Navbar bilingüe | Implementar la barra de navegación con el logotipo, los enlaces a las secciones y el selector de idioma, con sus estilos. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US01 | Visualización de la propuesta de valor | T002 | Hero con propuesta de valor | Implementar el hero con el título, el subtítulo, los botones de acción y la tarjeta flotante de estado de lote. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US46 | Cambio de idioma | T003 | Archivos de traducción y cambio de idioma | Crear los archivos de traducción en inglés y español y la lógica que aplica los textos con `data-i18n`. | 6 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| TS01 | Implementación de Website adaptable y accesible | T004 | Configuración del repositorio del Website | Crear el repositorio con la estructura base del sitio, el `.gitignore` y las ramas `main` y `develop`. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| TS01 | Implementación de Website adaptable y accesible | T005 | Recursos gráficos | Agregar el logotipo de DoofPlus y las fotos de los integrantes del equipo. | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| — | Tareas técnicas | T006 | Configuración del proyecto Vue | Configurar Vite, PrimeVue, Vue I18n, las variables de entorno (`.env`) y json-server con las rutas `/api/v1`. | 6 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| — | Tareas técnicas | T007 | Footer de la Web Application | Implementar el componente de footer con soporte de internacionalización. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |

#### 5.2.1.4. Development Evidence for Sprint Review

Durante el Sprint 1 se creó el repositorio del Website con su navegación, el hero y el cambio de idioma, y se configuró el proyecto de la Web Application con Vue 3, PrimeVue, Vue I18n y la Fake API. Las tablas siguientes presentan los commits de todas las ramas de ambos repositorios en el periodo del Sprint (del 30 de setiembre al 8 de octubre de 2026); la columna *Branch* indica la rama en que se realizó cada commit.

**Website** (11 commits)

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|------------|--------|-----------|----------------|---------------------|---------------------|
| IngesCompanyCC/doofplus-website | develop | `185d2c0` | feat(assets): add DoofPlus logo and team photos | — | 08/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `2acc88b` | Merge branch 'feature/navbar-hero' into develop | — | 07/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `2ca1c4b` | feat(translations): add hero content translations for English and Spanish | — | 07/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `996e8cd` | feat(hero): add hero section with dynamic content placeholders and styles | — | 07/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `ab9e295` | feat(nav): add navbar with language support in English and Spanish and styles | — | 07/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `c569c20` | feat(translations): add language switching functionality and update HTML structure | — | 07/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `0a72a3f` | Merge branch 'feature/project-configuration' into develop | — | 07/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `dc827a9` | feat(translations): implement language switching functionality file | — | 07/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `afb534f` | feat(translations): add initial language files for English and Spanish | — | 07/10/2026 |
| IngesCompanyCC/doofplus-website | main | `80e064f` | chore: add gitignore | — | 07/10/2026 |
| IngesCompanyCC/doofplus-website | main | `5a5fb40` | chore: initial commit | — | 07/10/2026 |

**Web Application** (7 commits)

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|------------|--------|-----------|----------------|---------------------|---------------------|
| IngesCompanyCC/doofplus-web-application | feature/shared | `1a507d6` | feat(footer-content): add footer component with internationalization support for English and Spanish | — | 30/09/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `91f10ed` | Merge branch 'feature/project-configuration' into develop | — | 30/09/2026 |
| IngesCompanyCC/doofplus-web-application | feature/project-configuration | `d3fe488` | feat(i18n): add initial i18n setup with English and Spanish language files for translation support | — | 30/09/2026 |
| IngesCompanyCC/doofplus-web-application | feature/project-configuration | `825d7a2` | chore: add initial db and routes configuration for json-server | — | 30/09/2026 |
| IngesCompanyCC/doofplus-web-application | feature/project-configuration | `a8b15b5` | chore: update dependencies and integrate PrimeVue components | — | 30/09/2026 |
| IngesCompanyCC/doofplus-web-application | feature/project-configuration | `d8c858c` | chore: add gitignore | — | 30/09/2026 |
| IngesCompanyCC/doofplus-web-application | feature/project-configuration | `2e07764` | chore: initial commit | — | 30/09/2026 |

#### 5.2.1.5. Execution Evidence for Sprint Review

En este Sprint se logró la base del Website, bilingüe (inglés y español), con la barra de navegación, el hero con la propuesta de valor y la tarjeta de estado de lote. La siguiente captura muestra el Website al cierre del Sprint (commit `185d2c0`):

![Website al cierre del Sprint 1](../assets/img/chapter5/screenshots/s1-full.png)

*Figura: Website al cierre del Sprint 1: navegación, hero y selector de idioma.*

#### 5.2.1.6. Services Documentation Evidence for Sprint Review

El alcance del Sprint 1 no incluyó endpoints propios. La Web Application quedó configurada con json-server y las rutas `/api/v1`, cuyos endpoints se documentan en el Sprint 2. La documentación de los RESTful Web Services con OpenAPI se elaborará en el sprint en que se implementen dichos servicios.

#### 5.2.1.7. Software Deployment Evidence for Sprint Review

En el Sprint 1 los productos se mantuvieron en los repositorios de la organización IngesCompanyCC, con las ramas `main` y `develop` de GitFlow. La publicación del Website en GitHub Pages, de la Web Application en Firebase Hosting y de la Fake API en Render se realizó en el Sprint 2 (sección 5.2.2.7).

#### 5.2.1.8. Team Collaboration Insights during Sprint

En el Sprint 1 la configuración inicial de ambos repositorios estuvo a cargo de Zavaleta Gutierrez, Rodolfo Martin, con el aporte de Cobades Zamora, Yhoshua Hebert en los recursos gráficos. La siguiente tabla resume los commits de cada integrante en ambos repositorios durante el Sprint (sin contar los commits de merge):

| Team Member | GitHub Username | Website | Web Application | Total |
|---|---|:---:|:---:|:---:|
| Zavaleta Gutierrez, Rodolfo Martin | gutierrezrodolfo360-bit | 8 | 6 | 14 |
| Cobades Zamora, Yhoshua Hebert | YhoshuaCZ | 1 | 0 | 1 |
| Angulo Ramírez, Marcelo Martín | Zock2005 | 0 | 0 | 0 |
| Rojas Ambicho, Nestor Daniel | danRO-20 | 0 | 0 | 0 |
| Yarleque Ruiz, Cristina Marcela | Cris06luna | 0 | 0 | 0 |

### 5.2.2. Sprint 2

En esta sección se registra y explica el avance en términos de producto y trabajo colaborativo para el **Sprint 2**, cuyo enfoque fue completar el Website de DoofPlus y construir la primera versión de la Frontend Web Application con sus seis bounded contexts, consumiendo una Fake API mientras se implementan los RESTful Web Services.

#### 5.2.2.1. Sprint Planning 2

El Sprint Planning Meeting sirvió para definir los objetivos del Sprint, asignar responsabilidades por bounded context y seleccionar las User Stories pendientes del Website y las de la Web Application. A continuación, se presenta el resumen de la reunión de planificación:

| Sprint # | Sprint 2 |
|----------|----------|
| **Sprint Planning Background** | |
| **Date** | 2026-10-09 |
| **Time** | 09:00 AM |
| **Location** | Reunión virtual vía Discord |
| **Prepared By** | Cobades Zamora, Yhoshua Hebert |
| **Attendees (to planning meeting)** | Angulo Ramírez, Marcelo Martín / Cobades Zamora, Yhoshua Hebert / Rojas Ambicho, Nestor Daniel / Yarleque Ruiz, Cristina Marcela / Zavaleta Gutierrez, Rodolfo Martin |
| **Sprint 1 Review Summary** | Se entregó la base del Website (navegación, hero y cambio de idioma) y el proyecto de la Web Application configurado con Vue 3, PrimeVue, Vue I18n y la Fake API. Como feedback, se pidió completar las secciones del Website según los mock-ups del capítulo 4, agregar el acceso por segmento a la Web Application y publicar ambos productos. |
| **Sprint 1 Retrospective Summary** | Como acierto, los repositorios se crearon desde el inicio con las ramas `main` y `develop` de GitFlow. Como oportunidades de mejora, se acordó repartir el trabajo entre todos los integrantes, usar una feature branch por sección o bounded context, distribuir los commits a lo largo del sprint y usar los types de Conventional Commits según su significado. |
| **Sprint Goal & User Stories** | |
| **Sprint 2 Goal** | **Our focus is on** delivering the complete DoofPlus Website and the first version of the DoofPlus Web Application for the QA/QC, Production and Administration environments.<br>**We believe it delivers** a single place to register batches, follow deviations and CAPA, release batches with electronic signature and monitor IoT equipment to QA/QC specialists and production supervisors of pharmaceutical laboratories.<br>**This will be confirmed when** each segment signs in to its own environment with two-factor authentication and completes its user goals (QA-1 to QA-6 and PR-1 to PR-6) on the published Web Application. |
| **Sprint 2 Velocity** | 182 Story Points |
| **Sum of Story Points** | 182 Story Points (US03: 2, US45: 1, US04: 1, US47: 1, US01: 2, US02: 2, US48: 2, US49: 2, US05: 1, US06: 2, US07: 3, US08: 5, US55: 3, US50: 3, US51: 3, US52: 5, US53: 3, US35: 3, US36: 5, US57: 3, US14: 3, US15: 5, US16: 2, US58: 3, US17: 3, US32: 3, US23: 3, US24: 3, US25: 5, US26: 3, US37: 3, US38: 3, US39: 3, US56: 3, US22: 2, US09: 5, US10: 3, US12: 5, US13: 3, US18: 3, US19: 5, US20: 5, US21: 3, US11: 5, US54: 5, US31: 3, US27: 5, US28: 5, US29: 5, US30: 3, US59: 3, US33: 3, US34: 3, US40: 3, US41: 3, US42: 2, US43: 3) |

#### 5.2.2.2. Aspect Leaders and Collaborators

En el Sprint 2 se consideraron ocho aspectos: el Website y, en la Web Application, los bounded contexts (Shared e IAM; Organizations y Subscriptions; Manufacturing; IoT Monitoring; Quality & Compliance), las releases y el despliegue, y la documentación. El líder (L) de cada aspecto es el integrante con más commits en él, según el historial de los repositorios de la organización IngesCompanyCC.

| Team Member (Last Name, First Name) | GitHub Username | Website | Shared & IAM | Organizations & Subscriptions | Manufacturing | IoT Monitoring | Quality & Compliance | Releases & Deployment | Documentation |
|-------------------------------------|-----------------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Angulo Ramírez, Marcelo Martín | Zock2005 | C | C | | | | | L | C |
| Cobades Zamora, Yhoshua Hebert | YhoshuaCZ | | L | L | C | L | L | | |
| Rojas Ambicho, Nestor Daniel | danRO-20 | L | | | | | | C | C |
| Yarleque Ruiz, Cristina Marcela | Cris06luna | | | | | | | | L |
| Zavaleta Gutierrez, Rodolfo Martin | gutierrezrodolfo360-bit | C | | | L | | | | C |

#### 5.2.2.3. Sprint Backlog 2

El objetivo del Sprint fue completar el Website y entregar la primera versión de la Web Application. Las tareas se organizaron por User Story; las del Website incluyen la reimplementación de las secciones del Sprint 1 sobre el diseño del capítulo 4. Las tareas se registraron en el board del Sprint 2 en Jira Software.

![Sprint 2 en Jira Software: 64 actividades finalizadas y responsables](../assets/img/chapter5/evidencia/jira-sprint2.png)

**Enlace al board en Jira:** https://inges-company-cc.atlassian.net/jira/software/projects/UPC/boards/1

| Sprint # | Sprint 2 | | | | | | |
|----------|----------|---|---|---|---|---|---|
| **User Story** | | **Work-Item / Task** | | | | | |
| **Id** | **Title** | **Id** | **Title** | **Description** | **Estimation (Hours)** | **Assigned To** | **Status (To-do / In-Process / To-Review / Done)** |
| US01 | Visualización de la propuesta de valor | T008 | Hero con propuesta de valor | Implementar el hero con el título, el subtítulo, los indicadores de confianza y los botones Get Started y View plans. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US02 | Visualización de servicios y características | T009 | Secciones Services y Features | Implementar las tarjetas de servicios y el acordeón de características con su panel de vista previa. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US48 | Acceso por segmento a la Web Application | T010 | Acceso por segmento | Implementar la sección Choose your workspace y los enlaces con data-app-segment hacia el inicio de sesión de cada entorno. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US49 | Visualización del video promocional | T011 | Video About the Product y About the Team | Implementar el reproductor que carga el video de YouTube al pulsar play y muestra un aviso mientras el video no está publicado. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US05 | Preguntas frecuentes | T012 | Sección FAQ | Implementar las preguntas frecuentes con acordeón accesible y el enlace al formulario de contacto. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| US44 | Navegación por secciones | T013 | Navbar con menú móvil | Reimplementar la barra de navegación con anclas reales, Sign in y Get Started con destino propio y el menú overlay en mobile. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US03 | Visualización de planes y precios | T014 | Planes con toggle mensual/anual | Reimplementar las tarjetas de planes con el cambio de precio mensual/anual y su nota de precios. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US45 | Visualización del equipo y de la startup | T015 | Secciones About us y Our Team | Implementar la misión, la visión y las tarjetas de los integrantes con su foto y rol. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US04 | Formulario de contacto | T016 | Formulario Contact us | Implementar el formulario de consulta con validación de campos y el estado de mensaje enviado. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| US47 | Consulta de términos y política de privacidad | T017 | Términos y política de privacidad | Implementar las páginas terms.html y privacy.html con sus estilos y traducciones. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US06 | Inicio de sesión con segundo factor | T018 | Inicio de sesión con 2FA | Implementar la elección de entorno, el inicio de sesión por entorno y la verificación con código de seis dígitos. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US07 | Gestión de roles | T019 | Acceso por rol | Implementar el guard de IAM y el estado Access not authorized cuando el rol no corresponde al entorno. | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| US08 | Firmas electrónicas | T020 | Firma electrónica | Implementar la confirmación con contraseña en las aprobaciones, liberaciones y cierres críticos. | 6 | Cobades Zamora, Yhoshua Hebert | Done |
| US55 | Alta de usuarios del equipo | T021 | Usuarios e invitaciones | Implementar el directorio de usuarios y perfiles y el formulario para invitar usuarios. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US50 | Registro de organización | T022 | Registro de organización | Implementar el registro con razón social, RUC, planta, plan y primer administrador, y el resumen de administración. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US51 | Selección de plan | T023 | Selección de plan | Implementar la revisión del cambio de plan en la página de suscripción. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US52 | Pago de suscripción | T024 | Pago de suscripción | Implementar el historial de facturación y el reintento de un pago rechazado. | 6 | Rojas Ambicho, Nestor Daniel | Done |
| US53 | Renovación y cancelación | T025 | Renovación de la suscripción | Mostrar la fecha de renovación y el estado de la suscripción en la página de suscripción y pagos. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US35 | Catálogo de productos | T026 | Catálogo de productos | Implementar el catálogo de productos con su fórmula maestra vigente. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US36 | Gestión de fórmulas maestras | T027 | Fórmulas maestras | Mostrar los componentes y la versión aprobada de la fórmula maestra de cada producto. | 6 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US57 | Gestión de órdenes de producción | T028 | Órdenes de producción | Implementar la lista y la ejecución de órdenes de producción contra la fórmula maestra. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US14 | Registro de lotes | T029 | Registro de lotes | Implementar el diálogo Create batch con el estado de lote rechazado por fórmula no aprobada. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US15 | Consulta del historial de lotes | T030 | Historial de lotes | Implementar la lista de lotes con filtros y la genealogía y el historial de estados de cada lote. | 6 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US16 | Gestión de estados de lote | T031 | Estados de lote | Implementar el cambio de estado de los lotes y sus conteos por estado. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US58 | Recepción de materias primas | T032 | Recepción de materias primas | Implementar la recepción de materias primas con cuarentena e inspección. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US17 | Asociación de materias primas | T033 | Asociación de materias primas | Asignar lotes de materia prima aprobados a un lote de producción. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US32 | Panel de producción | T034 | Resumen de producción | Implementar el resumen de producción con el programa del día, la disponibilidad de equipos y las incidencias. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US23 | Registro de dispositivos IoT | T035 | Inventario de equipos y sensores | Implementar el inventario de equipos con sus sensores IoT y el registro de un sensor. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US24 | Asociación de sensores a un lote | T036 | Asociación de sensores a lotes | Implementar la evidencia IoT del lote con la asociación de equipos y sensores. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US25 | Captura automática de evidencias | T037 | Telemetría | Implementar el gráfico de telemetría con límites y lectura pico. | 6 | Angulo Ramírez, Marcelo Martín | Done |
| US26 | Consulta de registros IoT | T038 | Consulta de registros IoT | Implementar el resumen IoT con la salud de los equipos y la cola de alertas. | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| US37 | Gestión de equipos | T039 | Gestión de equipos | Mostrar el estado operativo de cada equipo en el inventario. | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| US38 | Gestión de calibraciones | T040 | Calibraciones | Mostrar el estado de calibración (apto, por vencer, no apto) de equipos y sensores. | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| US39 | Mantenimiento preventivo | T041 | Mantenimiento preventivo | Mostrar el próximo mantenimiento y su responsable en el detalle del equipo. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US56 | Registro y escalamiento de incidencias | T042 | Incidentes | Implementar la lista de incidentes con su severidad y el reconocimiento de alertas. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| US22 | Notificaciones de desviación crítica | T043 | Alertas críticas | Implementar el detalle del sensor con la alerta activa y la regla de reconocimiento. | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| US09 | Gestión de protocolos de calidad | T044 | Documentos de calidad | Implementar el repositorio de documentos controlados con su flujo de aprobación. | 6 | Rojas Ambicho, Nestor Daniel | Done |
| US10 | Repositorio de SOP | T045 | Repositorio de SOP | Filtrar los documentos por tipo y estado para consultar la versión vigente. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US12 | Control de versiones documentales | T046 | Versiones de documentos | Implementar el historial de versiones de cada documento. | 6 | Cobades Zamora, Yhoshua Hebert | Done |
| US13 | Aprobación documental | T047 | Aprobación documental | Implementar la aprobación con firma y el bloqueo de autoaprobación (segregación de funciones). | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| US18 | Registro de desviaciones | T048 | Registro de desviaciones | Implementar el reporte de desviación con su evaluación de riesgo. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| US19 | Análisis de causa raíz | T049 | Análisis de causa raíz | Implementar el registro de la causa raíz como requisito del plan CAPA. | 6 | Cobades Zamora, Yhoshua Hebert | Done |
| US20 | Gestión CAPA | T050 | Plan CAPA | Implementar el plan CAPA con sus acciones, responsables y fechas. | 6 | Cobades Zamora, Yhoshua Hebert | Done |
| US21 | Seguimiento de acciones CAPA | T051 | Seguimiento de CAPA | Mostrar el avance de las acciones CAPA y bloquear el cierre sin evidencia de eficacia. | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| US11 | Automatización de cálculos de calidad | T052 | Resultados analíticos | Implementar los resultados analíticos con su conformidad respecto de la especificación. | 6 | Cobades Zamora, Yhoshua Hebert | Done |
| US54 | Revisión y liberación de lotes | T053 | Liberación de lotes | Implementar la liberación de lotes con requisitos previos, firma electrónica y estado bloqueado. | 6 | Cobades Zamora, Yhoshua Hebert | Done |
| US31 | Panel de indicadores de calidad | T054 | Resumen de calidad | Implementar el resumen de calidad con la cola de prioridades y las próximas decisiones. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US27 | Audit trail | T055 | Audit trail | Implementar el registro de eventos con filtros por registro y actor, compartido por los tres entornos. | 6 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US28 | Generación automática de reportes | T056 | Reportes regulatorios | Implementar la lista y generación de reportes regulatorios. | 6 | Yarleque Ruiz, Cristina Marcela | Done |
| US29 | Preparación de auditorías | T057 | Preparación de auditorías | Implementar el registro de auditorías y su estado de preparación. | 6 | Yarleque Ruiz, Cristina Marcela | Done |
| US30 | Acceso de auditor a evidencias | T058 | Evidencias para auditor | Mostrar las evidencias vinculadas a cada hallazgo de auditoría. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| US59 | Registro de auditorías y hallazgos | T059 | Hallazgos de auditoría | Implementar el registro de hallazgos con su severidad y acción asignada. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| US33 | Indicadores de trazabilidad | T060 | Indicadores de trazabilidad | Implementar los indicadores de brechas de trazabilidad. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US34 | Indicadores de desviaciones | T061 | Indicadores de desviaciones | Implementar la tendencia de desviaciones por periodo. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US40 | Bandeja de tareas | T062 | Bandeja de tareas | Implementar la lista de tareas pendientes vinculadas a registros. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| US41 | Notificaciones entre áreas | T063 | Comunicación entre áreas | Implementar los comentarios de cada tarea para la coordinación entre Producción y Calidad. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| US42 | Solicitudes de aprobación | T064 | Solicitudes de aprobación | Crear tareas de aprobación dirigidas a Calidad desde un registro. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| US43 | Seguimiento de tareas | T065 | Seguimiento de tareas | Mostrar el estado de las tareas asignadas por el usuario. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| — | Tareas técnicas | T066 | Capa shared | Implementar las clases base de la API, el layout público, el workspace shell, el page header y el selector de idioma. | 6 | Cobades Zamora, Yhoshua Hebert | Done |
| — | Tareas técnicas | T067 | Accesibilidad del layout público | Mejorar el layout público, el footer y la página 404 con atributos de accesibilidad y textos traducidos. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| — | Tareas técnicas | T068 | Traducciones de IoT Monitoring | Agregar las 121 claves de la sección monitoring en inglés y español. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| — | Tareas técnicas | T069 | Documentación del repositorio | Redactar la estructura base de la documentación, las user stories, las ADRs y el diagrama de clases del proyecto. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| — | Tareas técnicas | T070 | Releases v1.0.2 | Preparar release/v1.0.2 en el Website y en la Web Application, con merge a main y tag SemVer. | 4 | Angulo Ramírez, Marcelo Martín | Done |
| — | Tareas técnicas | T071 | Configuración de despliegue | Agregar firebase.json para Firebase Hosting y el script server:prod para publicar la Fake API. | 4 | Rojas Ambicho, Nestor Daniel | Done |

#### 5.2.2.4. Development Evidence for Sprint Review

En el Sprint 2 se trabajó en dos repositorios siguiendo GitFlow: una feature branch por sección del Website o por bounded context de la Web Application, integradas en `develop` con merges `--no-ff`, y una rama `release/v1.0.2` fusionada en `main` con el tag `v1.0.2` en ambos repositorios. Las tablas siguientes presentan los commits de todas las ramas de ambos repositorios en el periodo del Sprint (9 y 10 de octubre de 2026); la columna *Branch* indica la rama en que se realizó cada commit.

**Website** (104 commits)

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|------------|--------|-----------|----------------|---------------------|---------------------|
| IngesCompanyCC/doofplus-website | main | `18317a7` | Merge branch 'develop' into main | — | 10/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `f3cc1fb` | Merge branch 'fix/sign-in-environment-selection' into develop | — | 10/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `ff1b2c2` | fix(config): add the sign-in route of the web application | — | 10/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `639b29d` | fix(navbar): open the environment selection of the web application from sign in | — | 10/10/2026 |
| IngesCompanyCC/doofplus-website | main | `a1f0b37` | Merge branch 'release/v1.0.2' into main | — | 10/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `bde1114` | Merge branch 'feature/documents' into develop | — | 10/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `da0a3d4` | feat(user-stories): add user stories for DoofPlus Landing Page application | — | 10/10/2026 |
| IngesCompanyCC/doofplus-website | main | `7ec9099` | Merge branch 'develop' into main | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `558394a` | Merge branch 'refactor/align-with-reference-landing' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `bcf6dc9` | docs(readme): describe the modular structure aligned with the reference landing | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `ddef20a` | feat(pages): align toolbar, hero, sections and footer with the reference landing | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `f3521e5` | refactor(scripts): split behavior into ES modules and point segment access to the web application | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `d6a0fae` | refactor(i18n): move translations to JSON files with IngesCompanyCC and Cristina Yarleque | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `6a5c2a5` | refactor(styles): split the stylesheet into modules matching the reference landing design | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | main | `77b8d41` | Merge branch 'develop' into main | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `a7abbd0` | Merge branch 'fix/landing-parity' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `9c36f51` | style(ui): style link buttons and highlight the active navigation link | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `0455622` | feat(scripts): remember language, translate page language and alt, add magnetic buttons and active section link | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `0a94eee` | feat(i18n): add page titles and hero image alt translations | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `150a5ba` | fix(pages): link hero and navbar access buttons and mark translatable title and image alt | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | main | `20435d4` | Merge branch 'develop' into main | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `798bef3` | Merge branch 'chore/release-prep' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `5ecf1f5` | docs(readme): add links to the published landing page and web application | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `e9e3fd0` | docs(changelog): add release 1.0.0 with every landing section | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `86c3a9d` | perf(assets): resize and compress team photo of Cristina | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `34eb8b5` | Merge branch 'fix/web-app-url' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `ca5f4a6` | fix(scripts): set the deployed firebase url of the web application | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `92108e8` | fix(routing): update web app URL and access routes for Vue migration | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `9b4a5b7` | Merge branch 'feature/layout-structure' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `8480ad1` | style(variables): add design tokens for colors, radius, shadows, and layout width | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `a04da5d` | style(variables): add new CSS variables for consistent theming | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `126ca09` | style(pages): add styles for terms of service and privacy policy pages | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `d8fcc2a` | style(grid): add footer grid layout for brand and link groups | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `0c2e9e1` | style(grid): add contact grid layout for intro, info, and form display | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `d3c4b2a` | style(grid): add team grid layout for three cards with centered alignment | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `eb4489b` | style(grid): add about grid layout for mission and vision sections | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `a146009` | style(grid): add product grid layout for intro, details, and video display | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `1334b0d` | style(grid): add features grid layout for preview panel and accordion | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `2e77a4f` | style(grid): add hero grid layout for balanced text and image display | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `c70045e` | style(grid): add 12-column fluid grid layout for responsive design | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `ae41643` | style(layout): add footer styles for improved layout structure | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `777e2db` | style(layout): add section styles and overline for improved layout structure | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `e0ff9c5` | style(layout): define container styles for layout structure | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `9dbdf27` | style(layout): define container styles for layout structure | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `e14ef68` | style(layout): add layout.css for styling structure | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `7c34004` | Merge branch 'feature/accessibility-seo' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `edbc6d9` | docs(readme): add cursors folder to the structure | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `3b64e2e` | feat(cursor): add custom cursors in the DoofPlus colors | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `597ce5b` | feat(seo): add description, keywords, author and favicon to every page | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `091720c` | feat(a11y): add skip link to the main content | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `5db5a3d` | Merge branch 'feature/mobile-layout' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `c30f05a` | feat(mobile): add responsive breakpoints for tablet and mobile | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `e1b8d67` | feat(navbar): add full-screen mobile menu | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `33a8fd6` | Merge branch 'feature/legal-pages' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `f363df0` | feat(pages): add privacy policy page | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `c782b12` | feat(pages): add terms of service page | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `cee3eb6` | Merge branch 'fix/faq-styles' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `08995d8` | fix(faq): close faq-more rule and remove stray declarations | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `98d3c28` | Merge branch 'fix/footer-styles' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `b4d515e` | fix(footer): close unterminated footer actions rule | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `dfa48b5` | Merge branch 'feature/footer' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `1d9b665` | feat(scripts): route segment access buttons to the web application | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `523df3f` | feat(footer): add footer with link groups and segment access | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `f72e319` | Merge branch 'feature/contact-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `948e196` | feat(scripts): add contact form validation and confirmation | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `441c93f` | feat(contact): add contact section with inquiry form | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `af63084` | Merge branch 'feature/faq-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `c1581e5` | feat(faq): add faq section reusing the accordion | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `deca300` | Merge branch 'feature/testimonials-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `c051a03` | feat(testimonials): add testimonials section | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `9200684` | Merge branch 'feature/team-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `741a34e` | docs(readme): fix team member surname | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `b4bae28` | feat(team): add team section with Cristina Yarleque and team video | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `0ddf58b` | Merge branch 'fix/company-name' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `16be82e` | fix(about): use the IngesCompanyCC company name | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `1b86c4d` | Merge branch 'feature/about-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `bc4cb8b` | feat(about): add about us section | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `dfdf035` | Merge branch 'feature/plans-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `77de141` | feat(scripts): add monthly/yearly billing toggle | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `158df16` | feat(plans): add pricing plans section | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `ae1f0f1` | Merge branch 'feature/benefits-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `e86c584` | feat(benefits): add benefits section | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `93a7730` | Merge branch 'feature/product-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `fa20991` | feat(product): add about-the-product section with video player | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `0a9dba5` | Merge branch 'feature/features-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `9e781e6` | feat(scripts): add accordion behavior | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `b6b86ce` | feat(features): add features section with accordion and preview panel | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `2cb9b42` | Merge branch 'feature/services-section' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `8db6b27` | feat(services): add services section | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `9a26a0a` | Merge branch 'feature/navbar-sections' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `dedd837` | feat(navbar): add logo image and anchor links to landing sections | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `6c53f8d` | Merge branch 'docs/readme-course-info' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `8f84a9c` | docs(readme): fix broken formatting and set the Aplicaciones Web course | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `c96d62e` | Merge branch 'fix/workspace-layout-grid' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `e1a56a3` | fix(ui): add missing grid layout and container boundaries | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `7bc81fa` | Merge branch 'fix/workspace-colors' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `900aa4d` | fix(ui): inject missing css variables and color overrides for the workspace section | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `4b646fc` | Merge branch 'fix/workspace-styles' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `59e9b67` | fix(ui): add missing layout and grid styles for workspace section | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `334cf9c` | Merge branch 'docs/repository-setup' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `cb0a751` | docs(repository): add readme, license, changelog and contributing files | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `7a16201` | fix(ui): remove duplicated workspace section | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `af05340` | feat(i18n): add workspace translations | — | 09/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `6b83cbe` | feat(ui): add workspace section and translations | — | 09/10/2026 |

**Web Application** (93 commits)

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|------------|--------|-----------|----------------|---------------------|---------------------|
| IngesCompanyCC/doofplus-web-application | main | `44f4f8c` | Merge branch 'release/v1.0.2' into main | — | 10/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `abf2d33` | Merge branch 'feature/aclaration' into develop | — | 10/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `d4b0868` | feat(public-layout): enhance layout with accessibility features and improved structure | — | 10/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `73ee894` | feat(footer): enhance footer with internationalization and dynamic legal links | — | 10/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `8ee0ceb` | feat(page-not-found): enhance 404 page with improved accessibility and dynamic content | — | 10/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/setup-docs | `3080340` | docs(setup): update adrs.md and information | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/setup-docs | `dc21772` | docs(setup): update adrs.md | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/setup-docs | `ba56b54` | docs(setup): add adrs.md | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/setup-docs | `9625c29` | docs(setup): add class-diagram.puml | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/setup-docs | `1cda8fa` | docs(setup): add user-stories.md | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | main | `e2ba8bc` | Merge branch 'develop' into main | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `2e478b9` | Merge branch 'fix/landing-page-url' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/landing-page-url | `0139e73` | fix(env): point to the renamed landing page repository | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | main | `8d04860` | Merge branch 'develop' into main | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/landing-page-url | `6a4c208` | Merge branch 'build/firebase-project' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | build/firebase-project | `830efe0` | build(firebase): add firebase project alias | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | build/firebase-project | `c6488a7` | chore(gitignore): ignore firebase deploy cache | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/setup-docs | `982805e` | docs(setup):  add base project report structure and root files | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | main | `988d770` | Merge branch 'develop' into main | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | build/firebase-project | `ed8774f` | Merge branch 'fix/production-env' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/production-env | `0aca34c` | fix(env): add every endpoint path and the Render API to the production environment | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | main | `43aa77b` | Merge branch 'develop' into main | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/production-env | `b3814e3` | Merge branch 'feature/shared' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `50ba279` | docs: upgrade README.md | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/production-env | `27957e1` | fix(build): remove BOM from config files causing Vite crash | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/production-env | `249e5df` | build: add server:prod script and firebase hosting configuration | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `0ce28f7` | feat(trail-view): add the audit trail and tasks and collaboration to every environment | The audit trail filters the attributed events by record and actor, shows the selected change and its provenance, and exports the events as CSV, recording the export. Tasks and collaboration shows the team inbox, assigns tasks and keeps the discussion of a record. QA/QC and administration list both pages, and production lists the tasks. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `a01bbbc` | feat(reports-view): add the regulatory reports library with the batch dossier of each report | Lists the reports, shows the configuration of the selected one and derives its batch dossier from the genealogy, analytical results and deviations of the batch, and creates batch dossier drafts. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `c3054f4` | feat(audit-view): add the audit with its findings register, the new finding form and the evidence package | Shows the findings, the closed and pending evidence and the audit progress, opens the selected finding with its observation, response and closure requirement, and adds findings to the audit. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `32df7cc` | feat(results-view): add the analytical results with the protocol assay calculation and the OOS deviation | Lists the results of a batch, calculates the assay with the formula of PRT-QC-012, saves it as within specification or out of specification, and registers the deviation an out-of-specification result requires, linked from the page. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `e18209a` | feat(release-view): add the batch release with the six readiness checks and the QA electronic signature | Checks the manufacturing record, analytical results, materials, deviations, packaging and documents of a batch, and signs its release only when every check passes, the user has the QA release privilege and re-enters the password. The release is recorded in the audit trail. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `625b636` | feat(capa-view): add the CAPA plan with actions, evidence, root cause and routing to an independent reviewer | Attaches the evidence of each action, adds actions, saves the root cause and routes the plan for approval only when the root cause is documented, every action has evidence and the reviewer is not the owner. Adds the shared info and danger alert styles. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `7e9687a` | feat(deviation-view): add the deviation investigation with risk priority number, evidence and disposition | Edits the summary, classification and containment, calculates the RPN from severity, likelihood and detectability, reviews and attaches evidence and submits the risk assessment only when the evidence is reviewed. Other pages can open a deviation with ?code=. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `86be3b6` | feat(document-view): add controlled documents with the approval workflow and the version history | Shows the version in review and the effective version, blocks the author from approving their own version (separation of duties), publishes the approved version and makes the previous one obsolete. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `55f3a50` | feat(quality-view): add the quality overview and the quality indicators as the QA/QC home | The overview shows open deviations and CAPA actions, audits, the release queue with blocked batches, the priority queue of the signed-in user, quality operations and upcoming decisions. The indicators show complete batch records, traceability gaps and closed deviations by root cause, with a CSV export. Adds the QA/QC routes and sidebar, the shared stat card styles and the Quality translations. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `e0cd861` | feat(quality): apply separation of duties, CAPA approval, release readiness and the audit trail in a Pinia store | The store loads the quality records, approves document versions only when the approver is not the author, registers deviations and submits risk assessments with reviewed evidence, routes complete CAPA plans to an independent reviewer, records analytical results, findings, reports, tasks and comments, checks the release readiness of a batch and signs its release after re-authentication. Every change adds an attributed event to the audit trail. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `ed14ce0` | feat(iam): re-authenticate the signed-in user before an electronic signature | The IAM store checks the password of the signed-in user again, as the batch release requires before the QA electronic signature. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/quality | `4b52fc3` | feat(quality): model documents, deviations, CAPA, analytical results, audits, reports and tasks and read them through the fake API | Adds the fourteen JavaScript entities of Quality & Compliance, their assemblers and QualityApi over BaseApi, the quality collections of Laboratorios Andinos to the fake API, and the endpoint paths. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/monitoring | `45d7be2` | feat(overview-view): add the production overview with schedule, readiness and incident priorities as the production home | Shows the orders of today, the output and batches in progress, the open IoT incidents, the batch on hold with its IoT evidence, the readiness of equipment and raw materials and the shift handover. The overview returns to the production routes and sidebar now that IoT Monitoring is in place. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/monitoring | `1bc4bc0` | feat(evidence-view): add the IoT evidence of a batch with equipment association and the readings recorded during the batch | Associates fit equipment whose sensors are not feeding another batch and explains the rejections, draws the critical variable of the batch and lists the readings captured by ThingsBoard with their result. The batch detail links to its IoT evidence. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/monitoring | `84b80ff` | feat(incidents-view): add the IoT incidents with their acknowledgment and linked records | Lists the active alerts, or every alert, with the sensor and equipment, the acknowledgment, the severity, the status and the linked deviation or incident. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/monitoring | `56bbb1d` | feat(equipment-view): add the equipment and sensor inventory, sensor registration and the sensor detail with alert acknowledgment | The inventory shows the calibration and maintenance of each piece of equipment and its sensors, registers sensors with a unique ThingsBoard device ID and records calibration and maintenance. The sensor detail shows the telemetry, acknowledges the open alert with a response note and follows the alert workflow. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/monitoring | `34fd3e7` | feat(iot-view): add the IoT overview with the live sensor chart, the alert queue and equipment health | Adds the telemetry chart as an SVG component with the acceptable range and the out-of-range readings, the IoT overview in the production routes and sidebar, and the IoT Monitoring translations. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/monitoring | `ea053d5` | feat(monitoring): associate equipment, register sensors and acknowledge alerts in a Pinia store | The store loads the equipment, sensors, readings and alerts, checks out-of-range readings, links the sensors of fit equipment to one batch at a time, registers sensors with a unique ThingsBoard device ID, records calibration and maintenance, acknowledges alerts without closing them and links them to deviations. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/monitoring | `b0ba763` | feat(monitoring): model equipment, sensors, readings and alerts and read them through the fake API | Adds the JavaScript entities, the assemblers and MonitoringApi over BaseApi, the equipment, sensors, readings and alerts of the Ate plant to the fake API, and the endpoint paths. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `5a7b211` | feat(profile-view): add profile and notification preferences for every environment | Each environment opens the profile from the user menu. The page edits the personal data, the email and in-app notifications and the preferred language, which applies right away. The language chosen in the profile or in the top bar is remembered between visits. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `fe1d22b` | feat(overview-view): add the administration overview with users, facilities and account health | Shows the active users and invited accounts, the organization with its facilities, the subscription and its declined payment, the account health and the administrative activity. The overview becomes the home of the administration environment. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `86dd55c` | feat(registration-view): add the organization registration with plan and first administrator | Public page at /register with the organization details, the plan and billing cycle, the administrator account with the password rule and strength meter, the acceptance of the terms and the email verification step. The sign-in environment selection links to it, and the Organizations translations are added. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `990b3cb` | feat(organizations): register organizations and update user profiles in a Pinia store | The store loads the organization of the signed-in user, its facilities, the user profiles and the administrative activity. The registration rejects a RUC that is already registered and creates the organization, its primary plant, the pending subscription and the first administrator. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `03cd705` | feat(iam): create the first administrator of a newly registered organization | The account stays invited until the administrator verifies the email. The fake API keeps the password with the user until the RESTful API exposes a sign-up endpoint. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `b632cc6` | feat(organizations): model organizations, facilities, user profiles and administrative activity and read them through the fake API | Adds the JavaScript entities, the registration command, the assemblers and OrganizationsApi over BaseApi, with the lookup of an organization by RUC. Adds Laboratorios Andinos, its facilities, the user profiles and the administrative activity to the fake API, and the endpoint paths. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `7ded46d` | feat(subscription-view): add the subscriptions and payments page in the administration environment | Shows the plan, the active users, the amount and the next invoice, the declined payment with its retry, the billing history, the plan change review, the payment method and the payment states. Adds the subscription page to the administration routes and sidebar, the US dollar format and the subscriptions translations. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `719f1b2` | feat(iam): load the users of the organization for the administration pages | The IAM store keeps the users of the organization of the signed-in user and its active accounts, used by the subscription and administration pages. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `fe01a88` | feat(subscriptions): manage the plan change and the payment retry in a Pinia store | The store loads the public plans, the subscription of an organization and its invoices, creates the pending subscription of a new organization, changes the plan or billing cycle, and retries a declined charge by issuing an approved invoice and activating the subscription. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/organizations | `0b5589c` | feat(subscriptions): model plans, subscriptions and invoices and read them through the fake API | Adds the JavaScript entities, the assemblers and SubscriptionsApi over BaseApi, the plans, the subscription of Laboratorios Andinos and its invoices to the fake API, and the endpoint paths. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `86168aa` | refactor(routing): open the production orders as the production home until the overview arrives with IoT monitoring | The production overview shows the open IoT incidents, so its placeholder leaves the routes and the sidebar; Alberto Valle now lands on the production orders after signing in. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `b657401` | feat(product-view): add the product catalog with master formulas and the raw-material receipt with quarantine inspection | The catalog registers products with a unique code and shows the components of the effective master formula. The receipt validates the internal lot, attaches the certificate of analysis, moves the new lot to sampling and requests the QC inspection. Alerts and form fields move to the global styles. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `dfd11eb` | feat(order-view): add the production orders and the order execution against the master formula | The order list links each order to its detail. The detail records the next operation in sequence, blocks recording while an operation is on hold, and shows the effective master formula and the order context. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `ca9fc44` | feat(batch-view): add the batch list, the create batch dialog and the batch genealogy with PrimeVue | The batch list shows the status counts, the filters, the selected batch and the lookup; the dialog rejects duplicate batch numbers and products without an approved master formula; the detail shows the input lots, the production order, the operations and the status history. Adds the shared page header, the number and UTC date formats, the workspace table and chip styles, and the Manufacturing translations. The sidebar keeps a page selected on its detail pages. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `34ff6a7` | fix(manufacturing): restore the store and API built on the assemblers after the merge with develop | The merge of origin/develop kept a second store and API that call the endpoints without the assemblers or the env paths, and a production overview written in TypeScript with PrimeFlex 2 classes that do not exist in PrimeFlex 4. Restores the Pinia store and the ManufacturingApi of the Manufacturing rewrite; the production overview returns to a placeholder until IoT Monitoring is in place. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `3600d94` | Merge remote-tracking branch 'origin/develop' into develop | # Conflicts: | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `a9353c8` | refactor(manufacturing): manage batches, orders and material lots in a Pinia store | The store loads the eight collections, validates batches (unique code and approved master formula), creates the batch with its production order and history entry, records operations in sequence, releases batches and receives raw materials. json-server runs without --watch: it restarted after its own writes and refused the request that adds the batch history entry. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `692a7f7` | refactor(manufacturing): rewrite the domain and infrastructure in JavaScript over the base API | The entities and assemblers were TypeScript and Angular code pasted in .js files, so they could not run in Vue. Each entity is now a JavaScript class with public fields, each assembler maps axios responses, and ManufacturingApi extends BaseApi with one BaseEndpoint per collection. Adds the eight manufacturing collections to the fake API and their endpoint paths. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `d395dbe` | feat(manufacturing): implement production overview UI using PrimeVue | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `bfa8488` | feat(manufacturing): implement state management using Pinia | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/manufacturing | `27ebb20` | feat(manufacturing): implement API client using Axios and BaseApi pattern | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/iam | `856a84d` | feat(iam): guard each workspace by environment and add the user menu with sign out | Without a session the guard opens the sign-in of the environment, and a user of another environment goes back to their own home. The sidebar shows the organization and the navigation of the environment. Pinia is installed before the router so the guard can read the IAM store on the first navigation. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/iam | `dfe47f8` | feat(iam): sign in to an environment with the two-factor code and keep the session | Add the environment selection and the sign-in form with PrimeVue, the IAM store in Pinia, the public layout for sign-in and page not found, and the IAM translations. The session opens only when the role of the user belongs to the requested environment. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/iam | `0c82ab8` | feat(iam): model users and the sign-in command and query them through the fake API | Add the six demo users of Laboratorios Andinos to the fake API and the users endpoint path. The fake API filters users by credentials and by two-factor code until the RESTful API exposes a sign-in endpoint. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `4eebc75` | feat(shared): add the workspace layout with navigation, language switcher and legal footer | The sidebar becomes a drawer on small screens, the language switcher keeps the document language in sync, the page not found view links back home and PrimeVue uses the Material preset with the DoofPlus teal as primary color. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `5eb161b` | feat(shared): add the base API and base endpoint over the platform API | Configure axios with VITE_PLATFORM_API_URL, rewrite the env files in UTF-8 so Vite can read them, point production to the published fake API and add the server script for json-server. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `1844bfd` | chore(core): replace the Vite starter with the DoofPlus title, favicon and base styles | Remove the HelloWorld component and the Vite assets, rename App.vue to app.vue so the import in main.js also resolves on case-sensitive systems, and set the page title from the route meta. | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `e6a14b9` | fix(views): enable typescript parsing for ported logic | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `35d77f5` | feat(presentation): scaffold Vue components for manufacturing views | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `1e708fe` | Merge branch 'feature/manufacturing' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `6d9cc34` | fix(routing): properly map the manufacturing routes and root redirect | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `54c2a72` | Merge branch 'develop' into feature/manufacturing | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `065fe8d` | fix(core): remove vite default screen and add missing primevue license key env files | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `c8a6765` | feat(i18n): replace the batch translations with the manufacturing translations and add shared labels | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `c68c840` | feat(routing): route the production overview as the production home | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `5c6909c` | feat(overview): add the production overview with schedule readiness and incident priorities | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `3b83b15` | feat(routing): route the manufacturing views in the production environment | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `e1394c7` | feat(receipt-view): add the raw-material receipt with quarantine inspection | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `15931e1` | feat(product-view): add the product catalog and the effective master formula | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `8ac9b15` | feat(order-view): add production orders and the order execution against the master formula | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `85d87b5` | feat(batch-view): add batch genealogy and status history | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `584d398` | feat(batch-view): show batches with status counts, filters, lookup and the selected batch | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `a52cba6` | feat(batch-view): turn the batch form into the create batch dialog with the rejection state | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `0bc9e6e` | feat(manufacturing): validate batches, record operations and receive materials in the store with signals | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `d2066c2` | feat(manufacturing): read and write the manufacturing collections through the fake API | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `8a34c52` | feat(manufacturing): model products, master formulas, orders, operations, batches and material lots | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `6adaa6b` | Merge branch 'feature/shared' into develop | — | 09/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/shared | `edd6de1` | fix(core): add missing pinia and router setup | — | 09/10/2026 |

#### 5.2.2.5. Execution Evidence for Sprint Review

En este Sprint se completó el Website, publicada en GitHub Pages, y se implementó la Web Application con sus tres entornos. En el Website se agregaron el hero, el acceso por segmento, los servicios y características, el video About the Product, los beneficios, About us, el equipo con su video, los testimonios y las preguntas frecuentes, y se reimplementaron la navegación con menú mobile, los planes, el formulario de consulta y las páginas legales. La Web Application permite elegir el entorno, iniciar sesión con segundo factor y recorrer todos los módulos de QA/QC (documentos, desviaciones y CAPA, resultados analíticos, liberación de lotes, auditorías, audit trail, reportes y tareas), Production (órdenes, productos y fórmulas, lotes, materias primas, equipos e IoT, incidentes) y Administration (usuarios, organización y suscripción), en inglés y español, con datos servidos por una Fake API.

**Website:** https://ingescompanycc.github.io/doofplus-website/

![Hero con la propuesta de valor (US01)](../assets/img/chapter5/sprint2/landing/01-home.png)

*Figura: Hero con la propuesta de valor (US01).*

![Acceso por segmento: Choose your workspace (US48)](../assets/img/chapter5/sprint2/landing/02-workspace.png)

*Figura: Acceso por segmento: Choose your workspace (US48).*

![Servicios (US02)](../assets/img/chapter5/sprint2/landing/03-services.png)

*Figura: Servicios (US02).*

![Características con acordeón y vista previa (US02)](../assets/img/chapter5/sprint2/landing/04-features.png)

*Figura: Características con acordeón y vista previa (US02).*

![Video About the Product (US49)](../assets/img/chapter5/sprint2/landing/05-product.png)

*Figura: Video About the Product (US49).*

![Beneficios](../assets/img/chapter5/sprint2/landing/06-benefits.png)

*Figura: Beneficios.*

![Planes con toggle mensual/anual (US03)](../assets/img/chapter5/sprint2/landing/07-plans.png)

*Figura: Planes con toggle mensual/anual (US03).*

![About us: misión y visión (US45)](../assets/img/chapter5/sprint2/landing/08-about.png)

*Figura: About us: misión y visión (US45).*

![Our Team y video About the Team (US45, US49)](../assets/img/chapter5/sprint2/landing/09-team.png)

*Figura: Our Team y video About the Team (US45, US49).*

![Testimonios](../assets/img/chapter5/sprint2/landing/10-testimonials.png)

*Figura: Testimonios.*

![Preguntas frecuentes (US05)](../assets/img/chapter5/sprint2/landing/11-faq.png)

*Figura: Preguntas frecuentes (US05).*

![Llamada a la acción final y footer con datos de contacto y redes sociales](../assets/img/chapter5/sprint2/landing/12-contact.png)

*Figura: Llamada a la acción final y footer con datos de contacto y redes sociales.*

![Website en español (US46)](../assets/img/chapter5/sprint2/landing/13-home-es.png)

*Figura: Website en español (US46).*

![Términos de Servicio (US47)](../assets/img/chapter5/sprint2/landing/14-terms.png)

*Figura: Términos de Servicio (US47).*

![Vista mobile](../assets/img/chapter5/sprint2/landing/15-mobile-home.png)

*Figura: Vista mobile.*

![Menú de navegación en mobile (US44)](../assets/img/chapter5/sprint2/landing/16-mobile-menu.png)

*Figura: Menú de navegación en mobile (US44).*

**Web Application:** https://ingescompanycc-frontend.web.app (cuentas de prueba en el README del repositorio; contraseña `DoofPlus2026!` y código 2FA `482106`).

![Elección de entorno (US06)](../assets/img/chapter5/sprint2/web-app/01-sign-in.png)

*Figura: Elección de entorno (US06).*

![Verificación con código 2FA (US06)](../assets/img/chapter5/sprint2/web-app/02-two-factor.png)

*Figura: Verificación con código 2FA (US06).*

![QA/QC · Quality overview (US31)](../assets/img/chapter5/sprint2/web-app/03-qa-overview.png)

*Figura: QA/QC · Quality overview (US31).*

![QA/QC · Quality documents con versiones y aprobación (US09, US12, US13)](../assets/img/chapter5/sprint2/web-app/04-qa-documents.png)

*Figura: QA/QC · Quality documents con versiones y aprobación (US09, US12, US13).*

![QA/QC · Deviation report y evaluación de riesgo (US18)](../assets/img/chapter5/sprint2/web-app/05-qa-deviations.png)

*Figura: QA/QC · Deviation report y evaluación de riesgo (US18).*

![QA/QC · Plan CAPA (US19, US20, US21)](../assets/img/chapter5/sprint2/web-app/06-qa-capa.png)

*Figura: QA/QC · Plan CAPA (US19, US20, US21).*

![QA/QC · Liberación de lotes con firma electrónica (US54, US08)](../assets/img/chapter5/sprint2/web-app/07-qa-batch-release.png)

*Figura: QA/QC · Liberación de lotes con firma electrónica (US54, US08).*

![QA/QC · Audit trail (US27)](../assets/img/chapter5/sprint2/web-app/08-qa-audit-trail.png)

*Figura: QA/QC · Audit trail (US27).*

![Production · Production overview (US32)](../assets/img/chapter5/sprint2/web-app/09-production-overview.png)

*Figura: Production · Production overview (US32).*

![Production · Órdenes de producción (US57)](../assets/img/chapter5/sprint2/web-app/10-production-orders.png)

*Figura: Production · Órdenes de producción (US57).*

![Production · Lotes con filtros y lote seleccionado (US14, US15, US16)](../assets/img/chapter5/sprint2/web-app/11-production-batches.png)

*Figura: Production · Lotes con filtros y lote seleccionado (US14, US15, US16).*

![Production · IoT overview con telemetría (US25, US26)](../assets/img/chapter5/sprint2/web-app/12-iot-overview.png)

*Figura: Production · IoT overview con telemetría (US25, US26).*

![Production · Equipos y sensores IoT (US23, US37, US38)](../assets/img/chapter5/sprint2/web-app/13-equipment.png)

*Figura: Production · Equipos y sensores IoT (US23, US37, US38).*

![Production · Incidentes (US56, US22)](../assets/img/chapter5/sprint2/web-app/14-incidents.png)

*Figura: Production · Incidentes (US56, US22).*

![Administration · Resumen de administración (US50)](../assets/img/chapter5/sprint2/web-app/15-admin-overview.png)

*Figura: Administration · Resumen de administración (US50).*

![Administration · Usuarios y perfiles (US07, US55)](../assets/img/chapter5/sprint2/web-app/16-admin-users.png)

*Figura: Administration · Usuarios y perfiles (US07, US55).*

![Administration · Invitación de usuarios (US55)](../assets/img/chapter5/sprint2/web-app/21-admin-invite.png)

*Figura: Administration · Invitación de usuarios (US55).*

![Administration · Tareas y colaboración (US40, US41)](../assets/img/chapter5/sprint2/web-app/22-admin-tasks.png)

*Figura: Administration · Tareas y colaboración (US40, US41).*

![Administration · Suscripción y pagos (US51, US52, US53)](../assets/img/chapter5/sprint2/web-app/17-admin-subscription.png)

*Figura: Administration · Suscripción y pagos (US51, US52, US53).*

![Administration · Suscripción y pagos en español](../assets/img/chapter5/sprint2/web-app/18-admin-subscription-es.png)

*Figura: Administration · Suscripción y pagos en español.*

![Registro de la organización (US50)](../assets/img/chapter5/sprint2/web-app/19-register.png)

*Figura: Registro de la organización (US50).*

![Inicio de sesión en mobile](../assets/img/chapter5/sprint2/web-app/20-mobile-sign-in.png)

*Figura: Inicio de sesión en mobile.*

Videos de navegación del producto (upc-pre-202620-1asi0730-8130-IngesCompanyCC-productnavigation-sprint-2):

- **Website:** https://shorturl.at/Ba8hC
- **Web Application:** https://shorturl.at/NVztq

#### 5.2.2.6. Services Documentation Evidence for Sprint Review

En el Sprint 2 todavía no se implementaron los RESTful Web Services, por lo que no hay documentación OpenAPI. Mientras tanto, la Web Application consume una Fake API con json-server (`server/db.json`), cuyas rutas se exponen bajo el prefijo `/api/v1` mediante `server/routes.json`, el mismo que tendrán los Web Services. La siguiente tabla documenta los endpoints que usa la Web Application; la URL base local es `http://localhost:3000/api/v1` y la publicada en Render es `https://ingescompanycc-api.onrender.com/api/v1` (por ejemplo, `/plans` (https://ingescompanycc-api.onrender.com/api/v1/plans)).

| Endpoint | Verbo HTTP | Sintaxis | Parámetros | Response | URL documentación |
|---|---|---|---|---|---|
| `/users` | GET | `/api/v1/users` | — | 200 OK con la lista de `users` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/users` | POST | `/api/v1/users` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/users` | GET | `/api/v1/users?email={email}&password={password}` | `email`, `password` (query) | 200 OK con el usuario cuyas credenciales coinciden (inicio de sesión) | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/users` | GET | `/api/v1/users?id={id}&twoFactorCode={code}` | `id`, `twoFactorCode` (query) | 200 OK con el usuario si el código 2FA es válido | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/organizations` | GET | `/api/v1/organizations` | — | 200 OK con la lista de `organizations` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/organizations` | GET | `/api/v1/organizations/{id}` | `id` (path) | 200 OK con el recurso de `organizations` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/organizations` | POST | `/api/v1/organizations` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/organizations` | GET | `/api/v1/organizations?ruc={ruc}` | `ruc` (query) | 200 OK con la organización que ya usa ese RUC | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/facilities` | GET | `/api/v1/facilities` | — | 200 OK con la lista de `facilities` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/facilities` | POST | `/api/v1/facilities` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/user-profiles` | GET | `/api/v1/user-profiles` | — | 200 OK con la lista de `user-profiles` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/user-profiles` | POST | `/api/v1/user-profiles` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/user-profiles` | PUT | `/api/v1/user-profiles/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/administrative-activities` | GET | `/api/v1/administrative-activities` | — | 200 OK con la lista de `administrative-activities` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/administrative-activities` | POST | `/api/v1/administrative-activities` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/plans` | GET | `/api/v1/plans` | — | 200 OK con la lista de `plans` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/subscriptions` | GET | `/api/v1/subscriptions` | — | 200 OK con la lista de `subscriptions` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/subscriptions` | POST | `/api/v1/subscriptions` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/subscriptions` | PUT | `/api/v1/subscriptions/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/invoices` | GET | `/api/v1/invoices` | — | 200 OK con la lista de `invoices` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/invoices` | POST | `/api/v1/invoices` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/products` | GET | `/api/v1/products` | — | 200 OK con la lista de `products` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/products` | POST | `/api/v1/products` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/master-formulas` | GET | `/api/v1/master-formulas` | — | 200 OK con la lista de `master-formulas` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/formula-items` | GET | `/api/v1/formula-items` | — | 200 OK con la lista de `formula-items` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/production-orders` | GET | `/api/v1/production-orders` | — | 200 OK con la lista de `production-orders` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/production-orders` | POST | `/api/v1/production-orders` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/operations` | GET | `/api/v1/operations` | — | 200 OK con la lista de `operations` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/operations` | PUT | `/api/v1/operations/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/batches` | GET | `/api/v1/batches` | — | 200 OK con la lista de `batches` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/batches` | POST | `/api/v1/batches` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/batches` | PUT | `/api/v1/batches/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/batch-events` | GET | `/api/v1/batch-events` | — | 200 OK con la lista de `batch-events` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/batch-events` | POST | `/api/v1/batch-events` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/material-lots` | GET | `/api/v1/material-lots` | — | 200 OK con la lista de `material-lots` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/material-lots` | POST | `/api/v1/material-lots` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/material-lots` | PUT | `/api/v1/material-lots/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/equipment` | GET | `/api/v1/equipment` | — | 200 OK con la lista de `equipment` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/equipment` | PUT | `/api/v1/equipment/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/sensors` | GET | `/api/v1/sensors` | — | 200 OK con la lista de `sensors` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/sensors` | POST | `/api/v1/sensors` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/sensors` | PUT | `/api/v1/sensors/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/readings` | GET | `/api/v1/readings` | — | 200 OK con la lista de `readings` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/alerts` | GET | `/api/v1/alerts` | — | 200 OK con la lista de `alerts` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/alerts` | PUT | `/api/v1/alerts/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/quality-documents` | GET | `/api/v1/quality-documents` | — | 200 OK con la lista de `quality-documents` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/quality-documents` | PUT | `/api/v1/quality-documents/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/deviations` | GET | `/api/v1/deviations` | — | 200 OK con la lista de `deviations` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/deviations` | POST | `/api/v1/deviations` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/deviations` | PUT | `/api/v1/deviations/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/evidence` | GET | `/api/v1/evidence` | — | 200 OK con la lista de `evidence` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/evidence` | POST | `/api/v1/evidence` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/evidence` | PUT | `/api/v1/evidence/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/capa-plans` | GET | `/api/v1/capa-plans` | — | 200 OK con la lista de `capa-plans` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/capa-plans` | PUT | `/api/v1/capa-plans/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/capa-actions` | GET | `/api/v1/capa-actions` | — | 200 OK con la lista de `capa-actions` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/capa-actions` | POST | `/api/v1/capa-actions` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/capa-actions` | PUT | `/api/v1/capa-actions/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/analytical-results` | GET | `/api/v1/analytical-results` | — | 200 OK con la lista de `analytical-results` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/analytical-results` | PUT | `/api/v1/analytical-results/{id}` | `id` (path) y recurso en el body (JSON) | 200 OK con el recurso actualizado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/audits` | GET | `/api/v1/audits` | — | 200 OK con la lista de `audits` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/audit-findings` | GET | `/api/v1/audit-findings` | — | 200 OK con la lista de `audit-findings` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/audit-findings` | POST | `/api/v1/audit-findings` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/audit-events` | GET | `/api/v1/audit-events` | — | 200 OK con la lista de `audit-events` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/audit-events` | POST | `/api/v1/audit-events` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/regulatory-reports` | GET | `/api/v1/regulatory-reports` | — | 200 OK con la lista de `regulatory-reports` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/regulatory-reports` | POST | `/api/v1/regulatory-reports` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/collaboration-tasks` | GET | `/api/v1/collaboration-tasks` | — | 200 OK con la lista de `collaboration-tasks` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/collaboration-tasks` | POST | `/api/v1/collaboration-tasks` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/task-comments` | GET | `/api/v1/task-comments` | — | 200 OK con la lista de `task-comments` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/task-comments` | POST | `/api/v1/task-comments` | Recurso en el body (JSON) | 201 Created con el recurso creado | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/traceability-gaps` | GET | `/api/v1/traceability-gaps` | — | 200 OK con la lista de `traceability-gaps` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |
| `/deviation-trends` | GET | `/api/v1/deviation-trends` | — | 200 OK con la lista de `deviation-trends` | https://github.com/IngesCompanyCC/doofplus-web-application#readme |

#### 5.2.2.7. Software Deployment Evidence for Sprint Review

En este Sprint se prepararon y publicaron las versiones estables de ambos productos siguiendo GitFlow:

- **Website:** la rama `release/v1.0.2` se fusionó en `main` con el tag `v1.0.2`. El Website se publica en GitHub Pages desde `main` en https://ingescompanycc.github.io/doofplus-website/.
- **Web Application:** la rama `release/v1.0.2` se fusionó en `main` con el tag `v1.0.2`. El repositorio incluye `firebase.json`, que configura Firebase Hosting como Single Page Application, y el script `npm run server:prod`, que publica la Fake API en un Web Service de Render. La Web Application está publicada en Firebase Hosting en https://ingescompanycc-frontend.web.app y la Fake API en Render en https://ingescompanycc-api.onrender.com/api/v1.

![Tag v1.0.2 del Website](../assets/img/chapter5/sprint2/github/release-landing-v1.0.2.png)

*Figura: Tag v1.0.2 del repositorio del Website.*

![Tag v1.0.2 de la Web Application](../assets/img/chapter5/sprint2/github/release-frontend-v1.0.2.png)

*Figura: Tag v1.0.2 del repositorio de la Web Application.*

![Website publicado](../assets/img/chapter5/evidencia/pages.png)

*Figura: Website publicado en GitHub Pages.*

#### 5.2.2.8. Team Collaboration Insights during Sprint

Los cinco integrantes participaron en el Sprint: en el Website, en la Web Application y en su documentación. La siguiente tabla resume los commits de cada integrante en todas las ramas de ambos repositorios (sin contar los commits de merge), seguida de los analíticos de GitHub: *Contributors*, que muestra los commits por integrante, y *Network*, que muestra las ramas de GitFlow y sus merges.

| Team Member | GitHub Username | Website | Web Application | Total |
|---|---|:---:|:---:|:---:|
| Rojas Ambicho, Nestor Daniel | danRO-20 | 44 | 4 | 48 |
| Cobades Zamora, Yhoshua Hebert | YhoshuaCZ | 0 | 43 | 43 |
| Zavaleta Gutierrez, Rodolfo Martin | gutierrezrodolfo360-bit | 8 | 23 | 31 |
| Angulo Ramírez, Marcelo Martín | Zock2005 | 17 | 3 | 20 |
| Yarleque Ruiz, Cristina Marcela | Cris06luna | 0 | 6 | 6 |

![Contributors del Website](../assets/img/chapter5/sprint2/github/insights-landing-contributors.png)

*Figura: Contributors del repositorio del Website.*

![Contributors de la Web Application](../assets/img/chapter5/sprint2/github/insights-frontend-contributors.png)

*Figura: Contributors del repositorio de la Web Application.*

![Network del Website](../assets/img/chapter5/sprint2/github/insights-landing-network.png)

*Figura: Network graph del repositorio del Website.*

![Network de la Web Application](../assets/img/chapter5/sprint2/github/insights-frontend-network.png)

*Figura: Network graph del repositorio de la Web Application.*

### 5.2.3. Sprint 3

En esta sección se registra y explica el avance en términos de producto y trabajo colaborativo para el **Sprint 3** (del 18/10/2026 al 14/11/2026), cuyo enfoque fue entregar la primera versión desplegada de los RESTful Web Services, mejorar el Website y la Web Application a partir del feedback del Sprint 2 y preparar la primera versión del curso de Responsabilidad Social.

#### 5.2.3.1. Sprint Planning 3

El Sprint Planning Meeting sirvió para definir los objetivos del Sprint, asignar responsabilidades por bounded context y seleccionar las Technical Stories de los RESTful Web Services. A continuación, se presenta el resumen de la reunión de planificación:

| Sprint # | Sprint 3 |
|----------|----------|
| **Sprint Planning Background** | |
| **Date** | 2026-10-18 |
| **Time** | 10:00 AM |
| **Location** | Reunión virtual vía Discord |
| **Prepared By** | Zavaleta Gutierrez, Rodolfo Martin |
| **Attendees (to planning meeting)** | Angulo Ramírez, Marcelo Martín / Cobades Zamora, Yhoshua Hebert / Rojas Ambicho, Nestor Daniel / Yarleque Ruiz, Cristina Marcela / Zavaleta Gutierrez, Rodolfo Martin |
| **Sprint 2 Review Summary** | Se entregó el Website completo en GitHub Pages y la primera versión de la Web Application (`v1.0.2`) en Firebase Hosting, con sus tres entornos y una Fake API en Render. Como feedback, se pidió llevar el formulario de contacto a la Web Application, unir el Website con el entorno de cada segmento y empezar los RESTful Web Services que reemplacen a la Fake API. |
| **Sprint 2 Retrospective Summary** | Como acierto, los dos productos se publicaron con la rama `release/v1.0.2` y su tag SemVer. Como oportunidades de mejora, se acordó unir las feature branches a `develop` con más frecuencia, repartir los bounded contexts de los Web Services entre todos los integrantes y mantener Jira al día con el estado real de cada tarea. |
| **Sprint Goal & User Stories** | |
| **Sprint 3 Goal** | **Our focus is on** delivering the first deployed version of the DoofPlus RESTful Web Services for organizations, subscriptions, manufacturing and IoT monitoring, documented with OpenAPI.<br>**We believe it delivers** a real backend that the Web Application can use instead of the Fake API, together with a website that takes each segment to its own environment, **to** QA/QC specialists and production supervisors of small and medium-sized pharmaceutical laboratories.<br>**This will be confirmed when** the API is published on Render with its database on Railway, its Scalar documentation lists the endpoints of each bounded context and a contact inquiry sent through the API is stored. |
| **Sprint 3 Velocity** | 34 Story Points |
| **Sum of Story Points** | 34 Story Points (TS12: 3, TS09: 5, TS07: 5, TS03: 5, TS10: 5, TS11: 5, TS02: 3, US48: 2, US04: 1) |

#### 5.2.3.2. Aspect Leaders and Collaborators

En el Sprint 3 se consideraron el Website y la Web Application y, en los RESTful Web Services, los bounded contexts (Shared & IAM, Organizations, Subscriptions, Manufacturing, IoT Monitoring y Quality & Compliance), las releases y el despliegue, y la documentación. El líder (L) de cada aspecto es el integrante con más commits en él, según el historial de los repositorios de la organización IngesCompanyCC.

| Team Member (Last Name, First Name) | GitHub Username | Website | Web Application | Shared & IAM | Organizations | Subscriptions | Manufacturing | IoT Monitoring | Quality & Compliance | Releases & Deployment | Documentation |
|-------------------------------------|-----------------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Angulo Ramírez, Marcelo Martín | Zock2005 | | | | | | | L | | | C |
| Cobades Zamora, Yhoshua Hebert | YhoshuaCZ | | C | C | | | | | L | L | C |
| Rojas Ambicho, Nestor Daniel | danRO-20 | L | L | L | C | L | L | C | | C | L |
| Yarleque Ruiz, Cristina Marcela | Cris06luna | C | | C | L | | | | | C | |
| Zavaleta Gutierrez, Rodolfo Martin | gutierrezrodolfo360-bit | C | C | C | | | C | | | C | |

#### 5.2.3.3. Sprint Backlog 3

El objetivo del Sprint fue entregar la primera versión desplegada de los RESTful Web Services. El Sprint Backlog 3 junta tres bloques: las mejoras del Website y de la Web Application hechas desde el 18/10 (T184 a T198), las tareas de los Web Services por Technical Story (T137 a T165) y la autenticación con JWT (TS02, T199 a T201). Una tarea está en *Done* solo cuando su rama se unió a `develop`; lo que no se termine pasa al Sprint 4. Al cierre del Sprint hay 31 tareas en *Done*, 5 en *In-Process* y 11 en *To-do*, con 177 horas estimadas en total.

![Sprint 3 en Jira Software: 20 actividades, 16 finalizadas y 4 en curso, con sus responsables](../assets/img/chapter5/evidencia/jira-sprint3.png)

**Enlace al board en Jira:** https://inges-company-cc.atlassian.net/jira/software/projects/UPC/boards/1

| Sprint # | Sprint 3 | | | | | | |
|----------|----------|---|---|---|---|---|---|
| **User Story** | | **Work-Item / Task** | | | | | |
| **Id** | **Title** | **Id** | **Title** | **Description** | **Estimation (Hours)** | **Assigned To** | **Status (To-do / In-Process / To-Review / Done)** |
| TS12 | API de organizaciones y consultas de contacto | T137 | Organizations: dominio y persistencia | Completar los aggregates Organization, UserProfile y ContactInquiry con sus value objects y su persistencia con Entity Framework Core y MySQL. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| TS12 | API de organizaciones y consultas de contacto | T138 | Organizations: servicios de aplicación | Implementar los command services y query services de registro de la organización, sus sedes, los perfiles de usuario y las consultas de contacto. | 3 | Yarleque Ruiz, Cristina Marcela | Done |
| TS12 | API de organizaciones y consultas de contacto | T139 | Organizations: endpoints REST | Exponer /api/v1/organizations, /api/v1/facilities, /api/v1/user-profiles y POST /api/v1/contact-inquiries con resources, assemblers y documentación OpenAPI. | 4 | Yarleque Ruiz, Cristina Marcela | Done |
| TS12 | API de organizaciones y consultas de contacto | T140 | Organizations: fachada ACL | Implementar la fachada de Organizations para que Subscriptions consulte la organización sin acceder a su modelo. | 2 | Yarleque Ruiz, Cristina Marcela | To-do |
| TS12 | API de organizaciones y consultas de contacto | T141 | Web App: registro de organización y contacto contra el API | Reemplazar la Fake API en el módulo organizations de la Web Application (US50, US04). | 3 | Yarleque Ruiz, Cristina Marcela | Done |
| TS09 | API de suscripciones y pagos | T142 | Subscriptions: revisión e integración en develop | Revisar la rama de Subscriptions, corregir observaciones y fusionarla en develop. | 2 | Rojas Ambicho, Nestor Daniel | Done |
| TS09 | API de suscripciones y pagos | T143 | Subscriptions: pago con Niubiz | Agregar el pago de la suscripción: POST /api/v1/subscriptions/{subscriptionId}/payments con el sandbox de Niubiz; responde 422 si el pago se rechaza y 503 si la pasarela no está disponible, sin registrar el pago. | 5 | Rojas Ambicho, Nestor Daniel | Done |
| TS09 | API de suscripciones y pagos | T144 | Subscriptions: renovación | Agregar la renovación de la suscripción (US53) junto a la activación y cancelación. | 2 | Rojas Ambicho, Nestor Daniel | Done |
| TS09 | API de suscripciones y pagos | T145 | Web App: planes, suscripción y pago contra el API | Usar el API en el módulo subscriptions de la Web Application: planes, suscripción, pago con Niubiz, cambio de plan y facturas desde los pagos (US51, US52, US53). | 3 | Rojas Ambicho, Nestor Daniel | Done |
| TS09 | API de suscripciones y pagos | T146 | Subscriptions: pruebas | Pruebas unitarias con xUnit del dominio (Subscription, Payment, Plan y sus value objects), del caso de uso de pago con dobles de prueba y del gateway de Niubiz contra un Niubiz falso. | 3 | Rojas Ambicho, Nestor Daniel | Done |
| TS07 | API de operaciones de laboratorio | T147 | Manufacturing: productos y fórmulas maestras (dominio) | Implementar los aggregates Product y MasterFormula con sus value objects y su persistencia con Entity Framework Core. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| TS07 | API de operaciones de laboratorio | T148 | Manufacturing: endpoints de productos y fórmulas | Servicios y endpoints /api/v1/products y /api/v1/master-formulas con documentación OpenAPI (US35, US36). | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| TS07 | API de operaciones de laboratorio | T149 | IoT Monitoring: equipos, calibraciones y mantenimientos | Servicios y endpoints /api/v1/equipment; DELETE responde 409 si uno de sus sensores está asignado a un lote. Faltan las calibraciones y los mantenimientos (US37, US38, US39). | 5 | Angulo Ramírez, Marcelo Martín | In-Process |
| TS07 | API de operaciones de laboratorio | T150 | IoT Monitoring: sensores y telemetría | Servicios y endpoints /api/v1/sensors para el inventario de sensores (US23) y POST /api/v1/telemetry para recibir las lecturas de ThingsBoard con X-Api-Key y abrir una alerta si están fuera de rango. | 3 | Angulo Ramírez, Marcelo Martín | Done |
| TS07 | API de operaciones de laboratorio | T151 | Web App: productos, fórmulas y equipos contra el API | Reemplazar la Fake API en los módulos manufacturing (productos y fórmulas) e iot-monitoring (equipos y sensores). | 3 | Angulo Ramírez, Marcelo Martín | Done |
| TS03 | API de trazabilidad de lotes | T152 | Manufacturing: órdenes, lotes y materias primas (dominio) | Implementar los aggregates ProductionOrder, ProductionBatch y RawMaterialLot con sus estados y su persistencia con Entity Framework Core. | 5 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| TS03 | API de trazabilidad de lotes | T153 | Manufacturing: endpoints de órdenes y lotes | Endpoints /api/v1/production-orders y /api/v1/production-batches: registro (409 si el número de lote ya existe), los estados de la Web App (start, resume, release y progreso) y GET /{id} con sus incidencias, eventos y consumos (US57, US14, US15, US16). | 5 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| TS03 | API de trazabilidad de lotes | T154 | Manufacturing: recepción y asociación de materias primas | Endpoints /api/v1/material-lots de recepción, que dejan el lote en SAMPLING/PENDING, inspección, aprobación, rechazo y asociación a un lote de producción (US58, US17). | 3 | Cobades Zamora, Yhoshua Hebert | Done |
| TS03 | API de trazabilidad de lotes | T155 | Manufacturing: incidencias y escalamiento a Calidad | Registrar incidencias de un lote (POST /api/v1/production-batches/{id}/incidents) y publicar el domain event que Quality usa para abrir una desviación (US56). Falta el evento hacia Quality. | 3 | Angulo Ramírez, Marcelo Martín | In-Process |
| TS03 | API de trazabilidad de lotes | T156 | Manufacturing: fachada ACL | Implementar la fachada de Manufacturing para que Quality e IoT Monitoring consulten lotes y equipos. | 2 | Cobades Zamora, Yhoshua Hebert | To-do |
| TS03 | API de trazabilidad de lotes | T157 | Web App: trazabilidad de lotes contra el API | Reemplazar la Fake API en el módulo manufacturing (órdenes, lotes, materias primas e incidencias). | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| TS10 | API de documentación de calidad | T158 | Quality: servicios de documentación | Implementar los command y query services de QualityDocument: registro, nuevas versiones y aprobación (US09, US10, US12, US13). | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| TS10 | API de documentación de calidad | T159 | Quality: endpoints de documentación | Implementar el controller de QualityDocument: POST /api/v1/quality-documents responde 201 con la versión 1.0 y GET /{id} inexistente responde 404; documentación OpenAPI. | 3 | Cobades Zamora, Yhoshua Hebert | Done |
| TS10 | API de documentación de calidad | T160 | Web App: documentación de calidad contra el API | Reemplazar la Fake API en las vistas de protocolos, SOP y versiones del módulo quality. | 3 | Cobades Zamora, Yhoshua Hebert | Done |
| TS11 | API de desviaciones y CAPA | T161 | Quality: servicios de desviaciones y CAPA | Implementar los servicios de Deviation, CapaPlan y CapaAction: registro, causa raíz, acciones y cierre (US18, US19, US20, US21). | 5 | Cobades Zamora, Yhoshua Hebert | Done |
| TS11 | API de desviaciones y CAPA | T162 | Quality: endpoints de desviaciones y CAPA | Implementar los controllers de desviaciones y CAPA; cerrar una desviación sin causa raíz responde 422. | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| TS11 | API de desviaciones y CAPA | T163 | Quality: desviación desde incidencia de Producción | Policy que abre una desviación cuando Manufacturing escala una incidencia (US56). | 2 | Cobades Zamora, Yhoshua Hebert | To-do |
| TS11 | API de desviaciones y CAPA | T164 | Web App: desviaciones y CAPA contra el API | Reemplazar la Fake API en las vistas de desviaciones y CAPA del módulo quality. | 3 | Cobades Zamora, Yhoshua Hebert | Done |
| — | Tareas técnicas | T165 | Despliegue del RESTful API | Publicar el RESTful API en Render con Docker y la base de datos MySQL en Railway, con las migraciones aplicadas al arrancar y la documentación de Scalar accesible. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| US48 | Acceso por segmento a la Web Application | T184 | Website: sección Call to action | Implementar la sección Call to action final con los datos de contacto visibles, sus estilos y sus textos en en.json y es.json. | 3 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US48 | Acceso por segmento a la Web Application | T185 | Website: CTA por segmento y enlaces a la Web App | Mostrar un CTA por segmento y enlazar los botones de acceso y registro con el entorno correspondiente de la Web Application, también en local. | 3 | Rojas Ambicho, Nestor Daniel | Done |
| — | Tareas técnicas | T186 | Website: redes sociales en el footer | Agregar los enlaces a las redes sociales de DoofPlus en el footer. | 2 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| — | Tareas técnicas | T187 | Website: release y publicación en GitHub Pages | Fusionar develop en main (30/10/2026) y publicar el Website en GitHub Pages. | 3 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| — | Tareas técnicas | T188 | Website: correcciones del Website publicado | Corregir las páginas compartidas y el script de inicio, quitar el boilerplate y agregar el favicon, la licencia y las instrucciones para correrlo en local. | 2 | Rojas Ambicho, Nestor Daniel | Done |
| US04 | Formulario de contacto | T189 | Website: contacto, footer y release v1.1.0 | Llevar el contacto a la Web Application, quitar los restos del formulario del Website, ordenar la documentación y publicar v1.1.0. | 4 | Cobades Zamora, Yhoshua Hebert | Done |
| — | Tareas técnicas | T190 | Web App: correcciones de las pruebas de QA | Aplicar las observaciones de las pruebas de QA: formas farmacéuticas, detalle del audit trail, avisos de la recepción de materiales y de la causa raíz en CAPA, quitar evidencias y metadatos de las auditorías. | 4 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| US48 | Acceso por segmento a la Web Application | T191 | Web App: enlaces de la Web App al Website | Enlazar el sign-in y la zona pública de la Web Application con el Website. | 2 | Rojas Ambicho, Nestor Daniel | Done |
| — | Tareas técnicas | T192 | Web App: traducción de los permisos por rol | Traducir los permisos de cada rol en inglés y español. | 2 | Rojas Ambicho, Nestor Daniel | Done |
| — | Tareas técnicas | T193 | Web App: búsqueda global por código | Buscar lotes, órdenes, documentos y desviaciones por su código desde la barra superior. | 4 | Rojas Ambicho, Nestor Daniel | Done |
| — | Tareas técnicas | T194 | Web App: correcciones de la versión publicada | Corregir el nombre de la empresa en el footer y en la meta etiqueta author, y documentar la configuración del proyecto. | 2 | Rojas Ambicho, Nestor Daniel | Done |
| US04 | Formulario de contacto | T195 | Web App: Contact us en la zona pública | Implementar Contact us en la zona pública del sign-in con validación y envío de la consulta. | 6 | Cobades Zamora, Yhoshua Hebert | Done |
| — | Tareas técnicas | T196 | Web App: release v1.1.0 y despliegue en Firebase | Fusionar develop en main con el tag v1.1.0 y publicar la Web Application en Firebase Hosting. | 2 | Cobades Zamora, Yhoshua Hebert | Done |
| — | Tareas técnicas | T197 | Platform: configuración del proyecto y capa shared | Crear el proyecto ASP.NET Core con su configuración, OpenAPI con Scalar, Entity Framework Core con MySQL y la capa shared. | 6 | Zavaleta Gutierrez, Rodolfo Martin | Done |
| — | Tareas técnicas | T198 | Platform: shared con Result y ApplicationError | Agregar a la capa shared el manejo de errores con ProblemDetails, los mensajes en inglés y español y las validaciones de los value objects. | 6 | Rojas Ambicho, Nestor Daniel | Done |
| TS02 | Autenticación JWT en el RESTful API | T199 | IAM: dominio y persistencia de User y Role | Implementar los aggregates User y Role con su persistencia y la carga inicial de roles. | 10 | Cobades Zamora, Yhoshua Hebert | Done |
| TS02 | Autenticación JWT en el RESTful API | T200 | IAM: sign-up y sign-in con JWT | Exponer sign-up y sign-in que devuelven un JWT y filtrar las rutas por rol. | 10 | Cobades Zamora, Yhoshua Hebert | Done |
| TS02 | Autenticación JWT en el RESTful API | T201 | Web App: sign-in contra el API | Iniciar sesión en la Web Application contra el API y enviar el token en cada petición. | 6 | Cobades Zamora, Yhoshua Hebert | Done |

#### 5.2.3.4. Development Evidence for Sprint Review

En el Sprint 3 se trabajó en cuatro repositorios siguiendo GitFlow: una feature branch por sección del Website, por mejora de la Web Application o por bounded context de los Web Services, integradas en `develop` con merges `--no-ff`, y `develop` fusionado en `main` al publicar cada versión. El Website y la Web Application se publicaron el 30/10/2026 y llegaron a `v1.1.0` el 14/11/2026, y los RESTful Web Services tuvieron su primera versión, `v1.0.0`, el 14/11/2026. Las tablas siguientes presentan los commits de `main` del Sprint (desde el 18/10/2026); la columna *Branch* indica la rama en que se realizó cada commit. El curso de Responsabilidad Social se trabaja en `develop` de su repositorio, que todavía no tiene release.

**Website** (42 commits)

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|------------|--------|-----------|----------------|---------------------|---------------------|
| IngesCompanyCC/doofplus-website | feature/contact-social-and-final-cta | `41a7415` | feat(landing): add visible contact info, social media accounts and final cta section before footer | — | 26/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `bb1c38c` | Merge branch 'feature/contact-social-and-final-cta' into develop | — | 26/10/2026 |
| IngesCompanyCC/doofplus-website | main | `83d86a4` | Merge branch 'develop' into main | — | 30/10/2026 |
| IngesCompanyCC/doofplus-website | feature/call-to-action | `844fc29` | refactor(cta): align the call to action, footer and contact channels with the shared landing page | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | feature/call-to-action | `b6ab8fe` | feat(workspace): show the segment cards as information only | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | feature/call-to-action | `f31cca7` | feat(plans): show the plans as information only | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | feature/call-to-action | `b37467f` | feat(cta): add a call to action for each segment | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | feature/call-to-action | `42fef3c` | feat(navbar): lead get started to the call to action | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | feature/call-to-action | `c06cac0` | refactor(contact): remove the contact section | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | feature/call-to-action | `292ee4e` | fix(footer): center the social media icons | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `47191cb` | Merge branch 'feature/call-to-action' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | fix/shared-pages | `7ee126b` | fix(navbar): skip placeholder links when highlighting sections | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | fix/shared-pages | `7d2e1e6` | style(pages): align the terms, privacy and secondary page styles with the shared landing page | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `b704bee` | Merge branch 'fix/shared-pages' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | feature/local-web-app | `26eaf20` | feat(config): open the local web application from localhost | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `077f7a0` | Merge branch 'feature/local-web-app' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | chore/remove-boilerplate | `a7a35a4` | chore: remove unused boilerplate files | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | chore/remove-boilerplate | `7fc7c91` | fix(deploy): publish the static site on GitHub Pages without the Jekyll build | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | docs/license | `b3cfb8f` | docs: update the project license | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `621f4d9` | Merge branch 'chore/remove-boilerplate' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `2d41210` | Merge branch 'docs/license' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | style/favicon | `caac616` | style: use the doofplus shield as favicon | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `0fda8ce` | Merge branch 'style/favicon' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | docs/readme-run-locally | `a85fa69` | docs(readme): describe how to run the landing page with the local web application | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `adcb0e6` | Merge branch 'docs/readme-run-locally' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | chore/package-lock | `e27e4ae` | chore: sync the package lock with the package license | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `8e0635c` | Merge branch 'chore/package-lock' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | fix/start-script | `68db43e` | fix(scripts): serve the project root instead of the public folder in npm start | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | develop | `648b6c8` | Merge branch 'fix/start-script' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-website | feature/contact-us-link | `be74d6e` | feat(config): add the contact route of the web application | — | 01/11/2026 |
| IngesCompanyCC/doofplus-website | feature/contact-us-link | `a317535` | feat(footer): link the contact us form of the web application | — | 01/11/2026 |
| IngesCompanyCC/doofplus-website | develop | `a4aedf9` | Merge branch 'feature/contact-us-link' into develop | — | 01/11/2026 |
| IngesCompanyCC/doofplus-website | fix/remove-contact-link | `7442e24` | revert(footer): keep the contact us form only in the web application | — | 01/11/2026 |
| IngesCompanyCC/doofplus-website | develop | `1213ef4` | Merge branch 'fix/remove-contact-link' into develop | — | 01/11/2026 |
| IngesCompanyCC/doofplus-website | fix/remove-contact-form-leftovers | `51a2d05` | fix(styles): remove the unused contact form styles | — | 02/11/2026 |
| IngesCompanyCC/doofplus-website | fix/remove-contact-form-leftovers | `2c1b992` | fix(docs): describe contact us as part of the web application | — | 02/11/2026 |
| IngesCompanyCC/doofplus-website | develop | `8721aae` | Merge branch 'fix/remove-contact-form-leftovers' into develop | — | 02/11/2026 |
| IngesCompanyCC/doofplus-website | feature/cleanup-and-docs | `57dfbb9` | docs(LICENSE): update license file | — | 02/11/2026 |
| IngesCompanyCC/doofplus-website | feature/cleanup-and-docs | `06ce28f` | docs(CONTRIBUTING): fix setup and contributing steps | — | 02/11/2026 |
| IngesCompanyCC/doofplus-website | feature/cleanup-and-docs | `a577eef` | refactor(assets): rename team images and organize scripts | — | 02/11/2026 |
| IngesCompanyCC/doofplus-website | develop | `c8504e5` | Merge branch 'feature/cleanup-and-docs' into develop | — | 02/11/2026 |
| IngesCompanyCC/doofplus-website | main | `b36fecb` | Merge branch 'develop' into main | — | 14/11/2026 |

**Web Application** (58 commits)

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|------------|--------|-----------|----------------|---------------------|---------------------|
| IngesCompanyCC/doofplus-web-application | fix/company-name | `05789f5` | fix(i18n): use the IngesCompanyCC company name in the footer copyright | — | 30/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/company-name | `4791a8a` | fix(seo): set IngesCompanyCC as the author meta tag | — | 30/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `aa98e3c` | Merge branch 'fix/company-name' into develop | — | 30/10/2026 |
| IngesCompanyCC/doofplus-web-application | main | `151047c` | Merge branch 'develop' into main | — | 30/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `560a182` | Merge branch 'feature/setup-docs' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/user-management | `3b4aa62` | feat(server): add the role profiles and invitations of the fake API | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/user-management | `7e42ca4` | feat(env): add the role profiles and invitations endpoint paths | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/user-management | `0fece10` | feat(iam): model the invitations and role profiles of the laboratory | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/user-management | `a1ff5ca` | feat(iam): read role profiles and send invitations through the fake API | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/user-management | `06f8adb` | feat(iam): load the user directory and send invitations from the IAM store | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/user-management | `f961a80` | feat(i18n): add the users, profiles and invitation texts in English and Spanish | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/user-management | `3151b59` | feat(users): add the users and profiles page to the administration environment | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/user-management | `fb584d5` | feat(invitations): add the invitation form with the permission preview | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/user-management | `d4ff687` | fix(invitations): use the shared summary layout of the workspace | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `81f9b61` | Merge branch 'feature/user-management' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/landing-links | `96d4b15` | feat(env): link the local landing page in the development environment | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/landing-links | `0104276` | feat(i18n): add the navigation, help and leave confirmation texts in English and Spanish | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/landing-links | `4093c5c` | feat(navigation): link the public pages one level up and to the landing page | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/landing-links | `4d87f28` | refactor(iam): remove the link to the plans from the environment selection | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/landing-links | `6be180e` | feat(registration): link the registration back to the sign-in | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/landing-links | `bddcebb` | feat(registration): ask before leaving with unsaved data | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/landing-links | `c326882` | fix(footer): open the terms and privacy links in a new tab | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/landing-links | `df39775` | feat(shell): add the help link to the landing page FAQ in the sidebar | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/landing-links | `3c1b847` | feat(shell): add the search field and the session status of the workspace | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `fc10dde` | Merge branch 'feature/landing-links' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/dosage-forms | `47abafc` | fix(i18n): translate dosage forms in product catalog and form | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `51b1ad2` | Merge branch 'fix/dosage-forms' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/audit-trail-details | `08a7989` | fix(quality): populate change details for all audit trail events | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `8394bb8` | Merge branch 'fix/audit-trail-details' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | style/favicon | `0a2c542` | style: use the doofplus shield as favicon | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `8fd3afa` | Merge branch 'style/favicon' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/material-receipt-feedback | `f4ac22b` | fix(manufacturing): add validation feedback and success notification to material receipt | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `8a7a355` | Merge branch 'fix/material-receipt-feedback' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/capa-root-cause-feedback | `42c0d4d` | fix(quality): add feedback and lock state when saving the capa root cause | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `49d08c8` | Merge branch 'fix/capa-root-cause-feedback' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/remove-evidence | `f18ac5e` | fix(quality): let the user remove evidence attached by mistake before its review | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `e34f3b5` | Merge branch 'fix/remove-evidence' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/audit-evidence-metadata | `97278e4` | fix(quality): translate evidence package metadata in audits | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `21f47ef` | Merge branch 'fix/audit-evidence-metadata' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/permission-keys | `4f54edf` | fix(i18n): match the permission keys with the texts of the role profiles | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | fix/permission-keys | `154dca5` | fix(iam): finish loading the user directory when all its requests settle | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `60992e7` | Merge branch 'fix/permission-keys' into develop | — | 31/10/2026 |
| IngesCompanyCC/doofplus-web-application | feature/contact-inquiry | `b2ba922` | feat(organizations): add contact inquiries to the fake api | — | 01/11/2026 |
| IngesCompanyCC/doofplus-web-application | feature/contact-inquiry | `eef6158` | feat(organizations): add contact inquiry entity, assembler and api endpoint | — | 01/11/2026 |
| IngesCompanyCC/doofplus-web-application | feature/contact-inquiry | `64704ff` | feat(organizations): send contact inquiries from the organizations store | — | 01/11/2026 |
| IngesCompanyCC/doofplus-web-application | feature/contact-inquiry | `0368840` | feat(i18n): add contact us translations | — | 01/11/2026 |
| IngesCompanyCC/doofplus-web-application | feature/contact-inquiry | `4094f81` | feat(organizations): add the public contact us view and route | — | 01/11/2026 |
| IngesCompanyCC/doofplus-web-application | feature/contact-inquiry | `fa4f445` | feat(shell): link contact us from the public header and the sign-in page | — | 01/11/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `a33b179` | Merge branch 'feature/contact-inquiry' into develop | — | 01/11/2026 |
| IngesCompanyCC/doofplus-web-application | fix/contact-link-mobile | `669cb2f` | fix(shell): show only the contact icon in the public header on small screens | — | 02/11/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `5e560f6` | Merge branch 'fix/contact-link-mobile' into develop | — | 02/11/2026 |
| IngesCompanyCC/doofplus-web-application | fix/readme-routes | `a2614f0` | fix(docs): list the contact us and user management routes in the readme | — | 02/11/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `37fc6fd` | Merge branch 'fix/readme-routes' into develop | — | 02/11/2026 |
| IngesCompanyCC/doofplus-web-application | fix/landing-url-and-workspace-search | `ef0ea95` | fix(environment): link the website published by the new organization | — | 02/11/2026 |
| IngesCompanyCC/doofplus-web-application | fix/landing-url-and-workspace-search | `8ea199c` | feat(shell): search a record by its exact code from the top bar | — | 02/11/2026 |
| IngesCompanyCC/doofplus-web-application | fix/landing-url-and-workspace-search | `183e883` | feat(quality): open the document and the batch given in the url | — | 02/11/2026 |
| IngesCompanyCC/doofplus-web-application | develop | `52fea74` | Merge branch 'fix/landing-url-and-workspace-search' into develop | — | 02/11/2026 |
| IngesCompanyCC/doofplus-web-application | main | `29c337c` | Merge branch 'develop' into main | — | 14/11/2026 |

**RESTful Web Services** (184 commits)

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|------------|--------|-----------|----------------|---------------------|---------------------|
| IngesCompanyCC/doofplus-platform | main | `110a131` | chore: initial commit | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/project-configuration | `6b17dac` | chore: clean-up unused files | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/project-configuration | `6895ca2` | build: update project properties and add new packages | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/project-configuration | `284d972` | docs: add README and CHANGELOG files | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/project-configuration | `fa7800d` | build: update target framework to net10.0 and refine package references | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/project-configuration | `e47a073` | build: add global.json for SDK configuration | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/project-configuration | `13a23e9` | project configuration | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | develop | `e365f24` | Merge branch 'feature/project-configuration' into develop | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `fb06ca4` | feat(audit): add IAuditableEntity interface for Creation and Last Update Timestamps | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `928d9f1` | feat(repository): add IBaseRepository and IUnitOfWork contracts for repository and unit of work interfaces | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `16d6031` | feat(conventions): add string extensions for convention to snake case and plural | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `0dacc54` | feat(conventions): add ModelBuilderExtensions for snake case naming convention | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `579ff10` | feat(audit): add AuditableEntityInterceptor for entity timestamps | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `10a7114` | feat(orm): add appDbContext with entity configuration and interceptor | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `2b1b30b` | feat(orm): add BaseRepository for entity CRUD operation | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `457e004` | feat(orm): add UnitOfWork for EF Core repository management | — | 22/10/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `430428a` | feat(extensions): add StringExtensions for converting strings to kebab case | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `95d5773` | feat(extensions): add KebabCaseRouteNamingConvention for controller route naming | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `66868ae` | refactor: rename files for improved organization and clarity | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `769b118` | feat: add user-specific settings for unit testing environment | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `e224761` | feat(conventions): configure Kebab-Case route naming convention and enable lowercase URLs | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared | `e683a4a` | feat(result): add Result record for success and failure states | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `b31594f` | Merge branch 'feature/shared' into develop | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | fix/shared-infrastructure-configuration | `e1ee460` | refactor(shared): fix Infrastructure namespace typos across shared context | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | fix/shared-infrastructure-configuration | `afe6207` | fix(routing): correct regex replacement in ToKebabCase string extension | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | fix/shared-infrastructure-configuration | `44a3919` | feat(database): configure AppDbContext with MySQL and UnitOfWork dependency injection | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `d8ba7cd` | feat(subscriptions): add PlanKey, OrganizationId and BillingEmail value objects | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | fix/shared-infrastructure-configuration | `f1ce21a` | chore(config): add MySQL DefaultConnection string template in appsettings | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `a925deb` | Merge branch 'fix/shared-infrastructure-configuration' into develop | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `3dcac43` | feat(AlertCode): add AlertCode value object with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `c7a9cd4` | feat(CalibrationStatus): add CalibrationStatus value object with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `6515dc8` | feat(DeviceId): add DeviceId value object with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `5549e6a` | feat(EquipmentCode): add EquipmentCode value object with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `e5ef2f1` | feat(Facility): add Facility value object with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `0a85b2f` | feat(GetAlertByCodeQuery): add query object for retrieving alerts by code with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `76949bb` | feat(GetAlertByIdQuery): add query object for retrieving alerts by ID with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `e2a9dd9` | feat(GetAllAlertsByBatchCodeQuery): add query object for retrieving alerts by batch code with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `7a857fe` | feat(GetAllAlertsBySensorCodeQuery): add query object for retrieving alerts by sensor code with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `6ca62cb` | feat(GetAllAlertsQuery): add query object for retrieving all alerts | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `bd3d46d` | feat(GetAllEquipmentQuery): add query object for retrieving all equipment | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `505e0e3` | feat(GetAllReadingBySensorCodeQuery): add query object for retrieving readings by sensor code with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `e12df21` | feat(GetAllReadingQuery): add query object for retrieving all readings | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `97be8c4` | feat(GetAllReadingsByBatchCodeQuery): add query object for retrieving readings by batch code with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `030a456` | feat(GetAllSensorByEquipmentCodeQuery): add query object for retrieving sensors by equipment code with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `5dea184` | feat(GetAllSensorQuery): add query object for retrieving all sensors | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `5441135` | feat(GetEquipmentByCodeQuery): add query object for retrieving equipment by code with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `c471f95` | feat(GetEquipmentByIdQuery): add query object for retrieving equipment by ID with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `393b526` | feat(GetReadingByIdQuery): add query object for retrieving reading by ID with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `996def7` | feat(SensorCode): add value object for sensor code with validation | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `e0fea37` | Merge branch 'develop' into feature/monitoring | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `295c5f5` | feat(AlertCode): enhance validation with regex pattern and length checks | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `25fc1ac` | feat(CalibrationStatus): refactor to enum and enhance value handling | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `8786be6` | feat(DeviceId): enhance validation with regex and improve error messaging | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `81b2a38` | feat(EquipmentCode): enhance validation with regex and improve error messaging | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `de6eded` | feat(FacilityId): enhance validation with regex and improve error messaging | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `42ad642` | feat(SensorCode): enhance validation with regex and improve error messaging | — | 02/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `6547382` | feat: add bounded context and update gitignore | — | 03/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `e7057f3` | Merge branch 'develop' into feature/subscriptions | — | 04/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `ce6970b` | chore(organizations): add bounded context folder structure | — | 04/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `3df3b32` | feat(domain): add files to value objects | — | 04/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `2cc7975` | feat(domain): add files from commands | — | 04/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `cd9bcb1` | feat(domain): add files from events | — | 04/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `61e54ad` | feat(domain): add aggregate organization | — | 04/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `46c618f` | feat(organizations): add new aggregates, user profile and facility | — | 04/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `7525fb4` | feat(organizations): add domain queries | — | 04/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `148943a` | feat(organizations): add domain repository interfaces | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `0de8c96` | feat(organizations): add files from application Command Services | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `f77dbcf` | feat(organizations): add files from application Query Services | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `f223e9f` | feat(subscriptions): add Money value object | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `b6f5569` | feat(subscriptions): add PaymentCard value object | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `14ec26d` | feat(subscriptions): add EBillingCycle and ESubscriptionStatus enums | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `c6fdfd2` | feat(subscriptions): add plan and subscription commands | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `20d9ae1` | feat(organizations): implement organization and user profile command services | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `7f11321` | feat(organizations): implement organization and user profile query services | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `b588c6b` | feat(organizations): add ef core entity configurations | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `e3a3d73` | feat(organizations): implement organization repositories | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `a8f3424` | feat(shared): register organization aggregates in app db context | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `411a691` | feat(organizations): add files from rest resources | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `eb5645c` | feat(organizations): add files from rest assemblers | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `8825fb5` | feat(organizations): add organizations and user profiles controllers | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `fa69d7d` | feat(organizations): register organizations services in di container | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `dfd9520` | feat(shared): add domain event and event handler contracts with cortex mediator | — | 05/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `76655bb` | feat(shared): add aggregate root base with domain events | — | 06/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `52e6243` | feat(shared): add strongly typed entity ids and their ef core converter | — | 06/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `302e66b` | feat(shared): add the domain error and the application result | — | 06/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `51d2092` | refactor(shared): move the auditable entity contract to domain entities | — | 06/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `adebe2d` | refactor(shared): use int ids and no-tracking queries in the base repository | — | 06/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `37fe6bc` | feat(shared): add the domain events dispatch interceptor and document the app db context | — | 07/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `cfe2c6d` | feat(shared): add the logging, transaction and exception handling command behaviors | — | 07/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `98523b3` | feat(shared): add the common and error messages with english and spanish resources | The messages live next to their marker classes in Shared/Resources, so the localization must be registered without a ResourcesPath: with ResourcesPath set to Resources the localizer does not find them and returns the key instead (checked for en and es). | 07/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `dc70007` | feat(shared): add the global exception handler, its middleware and the problem details factory | The factory looks the title up as <ErrorType>.<Case>, the key format of ErrorMessages.resx; learning-center-platform uses only the case name and never finds the localized text. | 07/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `fa4c13a` | feat(shared): add the bearer security scheme transformer for the openapi document | — | 07/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-learning-center | `6ddbe57` | feat(config): register problem details, localization, cors, the mediator and the domain events interceptor | Keeps the MySQL connection and IUnitOfWork registration. Request localization runs before the exception handler; in the other order the handler answers in English even with Accept-Language: es (checked for the 500 and 409 responses). | 07/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `73881ac` | Merge branch 'feature/shared-learning-center' into develop | — | 07/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `5701267` | feat(SensorCode): improve validation regex and enhance error handling | — | 07/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `efb53e2` | Merge branch 'develop' into feature/subscriptions | — | 08/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `42c33b0` | refactor(subscriptions): make the organization id a strongly typed id | — | 08/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `9fac440` | feat(subscriptions): add the plan aggregate | — | 08/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `78e47c4` | feat(subscriptions): add the subscription aggregate | — | 08/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `a5d999f` | feat(subscriptions): add the plan and subscription queries | — | 08/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `20aeb67` | feat(subscriptions): map the plan and subscription aggregates with entity framework core | — | 09/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `81f9418` | feat(subscriptions): add the plan and subscription repositories | — | 09/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `be9f1e3` | feat(subscriptions): add the errors and the plan and subscription services | — | 09/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `f92e7b0` | feat(subscriptions): add the plan and subscription resources and assemblers | — | 09/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing | `165f197` | feat(manufacturing): add value objects and enums for production batches and incidents | — | 09/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing | `7f15a5e` | feat(manufacturing): add commands and queries for production batch operations | — | 09/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `89d4d6d` | feat(subscriptions): add the plans and subscriptions controllers | — | 10/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `3edc39e` | feat(subscriptions): add the localized subscriptions messages | — | 10/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `b81a006` | feat(config): register the subscriptions repositories, services and messages | — | 10/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing | `c675367` | feat(manufacturing): add ProductionBatch aggregate root and child entities | — | 10/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing | `c364105` | feat(manufacturing): add IProductionBatchRepository domain repository port | — | 10/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing | `db45794` | feat(manufacturing): implement ProductionBatch command and query application services | — | 11/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing | `a99e1fb` | feat(manufacturing): add EF Core persistence configuration and ProductionBatchRepository | — | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing | `93281fe` | feat(manufacturing): add REST API resources, assemblers, controller and DI registrations | — | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `72e20a1` | Merge branch 'feature/manufacturing' into develop | — | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `b903070` | Merge branch 'develop' into feature/organizations | # Conflicts: # IngesCompanyCCPlatform.API/Program.cs # IngesCompanyCCPlatform.API/Shared/Infrastructure/Persistence/EntityFrameworkCore/Configuration/AppDbContext.cs | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `c29b7ef` | fix: resolve various bugs across multiple files | — | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `bbc757b` | feat(organizations): migrate ids to int and adopt Result<T> with ProblemDetails | — | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `d007863` | feat(organizations): add OrganizationsError domain enumeration | — | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `667904d` | feat(organizations): add rganizationsActionResultAssembler for ProblemDetails responses | — | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `fef9350` | feat(organizations): add localized English and Spanish error messages in ErrorMessages resx | — | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `2f4e8a1` | chore(organizations): add EF migration for integer identifiers | — | 12/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `e7e8d10` | feat(organizations): add contact inquiry domain entities and send command | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `0f0186f` | feat(organizations): add queries to retrieve all contact inquiries and by id | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `81e4c0f` | feat(organizations): add command service and repository interface for contact inquiries | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `7956712` | feat(organizations): add contact inquiry error definitions | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `6afab16` | feat(contact): add query service and interface for contact inquiries | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `2f284f7` | feat(organizations): add entity configuration and repository implementation for contact inquiries | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `75a8a63` | feat(organizations): add action result mapping for contact inquiry errors | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `a18cee2` | feat(organizations): add controller, resources, and assemblers for contact inquiries | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `2a1c8d4` | feat(shared): add localized error messages for contact inquiries | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `1802ca1` | feat(organizations): add new configuration in DbContext and program | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations | `2e3cc47` | chore(migrations): add ContactInquiry table migration | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `69e3ea0` | Merge branch 'feature/organizations' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-value-object-messages | `2238f2b` | feat(shared): answer value object errors with a localized bad request | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-value-object-messages | `78d3033` | Merge remote-tracking branch 'origin/develop' into feature/shared-value-object-messages | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `967902b` | Merge branch 'feature/shared-value-object-messages' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `9aa8d9e` | Merge branch 'develop' into feature/subscriptions | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `70e054b` | feat(config): register the subscriptions messages for the exception handler | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions | `1fe675b` | chore(migrations): add the plans and subscriptions tables | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `24985e9` | Merge branch 'feature/subscriptions' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `ff2f2af` | Merge branch 'develop' into feature/monitoring | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `584ea44` | refactor(monitoring): move to pascal case folders and rewrite the value objects and queries in c# | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `2c4b4e5` | feat(monitoring): add the equipment, sensor, reading and alert aggregates and their commands | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `9ebc10d` | feat(monitoring): add the entity framework core mapping of the monitoring tables | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `a871138` | feat(monitoring): add the equipment, sensor, reading and alert repositories | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `a919d69` | feat(monitoring): add the monitoring errors and the command and query services | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `ff23259` | feat(monitoring): add the monitoring resources and assemblers | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `d942bf3` | feat(monitoring): add the equipment, sensors, readings, alerts and telemetry controllers | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `0cfaa76` | feat(monitoring): add the localized monitoring messages | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `aa3c5d3` | feat(config): register the monitoring services, messages, mapping and thingsboard key | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/monitoring | `c9887ff` | chore(migrations): add the equipment, sensors, readings and alerts tables | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `95dcce0` | feat(manufacturing): add the ids, codes and enums of products, formulas, orders and lots | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `fbaf8ff` | feat(manufacturing): add the product, master formula, production order and raw material lot aggregates | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `fae716c` | feat(manufacturing): add the entity framework core mapping of the catalog tables | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `c86c397` | feat(manufacturing): add the product, formula, order and lot repositories | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `f151c4f` | feat(manufacturing): add the catalog errors and the command and query services | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `3599f25` | feat(manufacturing): add the catalog resources and assemblers | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `d0ba1ca` | feat(manufacturing): add the products, master formulas, production orders and material lots controllers | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `44c90be` | feat(manufacturing): add the localized catalog messages | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `343d9dc` | feat(config): register the manufacturing catalog services, messages and mapping | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `115e7d8` | chore(migrations): add the products, formulas, orders, operations and lots tables | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/deployment | `24475f7` | feat(deployment): add the dockerfile and the production configuration | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `7eb9111` | Merge branch 'feature/deployment' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions-diagram-match | `04d5137` | fix(subscriptions): match the plans and subscriptions tables with the diagram and open | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `00e9c5a` | Merge branch 'feature/monitoring' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions-diagram-match | `925887e` | Merge branch 'develop' into feature/subscriptions-diagram-match | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions-diagram-match | `6b6ecfa` | fix(subscriptions): regenerate the migration after monitoring | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `be5fd8d` | Merge branch 'feature/subscriptions-diagram-match' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/production-batch-product-fields | `dc87d34` | feat(manufacturing): add the line, owner, progress and event tone that the web app shows | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `bdbe069` | Merge branch 'feature/production-batch-product-fields' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-catalog | `5480cb2` | Merge branch 'develop' into feature/manufacturing-catalog | # Conflicts: # IngesCompanyCCPlatform.API/Migrations/AppDbContextModelSnapshot.cs # IngesCompanyCCPlatform.API/Program.cs # IngesCompanyCCPlatform.API/Shared/Resources/Errors/ErrorMessages.es.resx # IngesCompanyCCPlatform.API/Shared/Resources/Errors/ErrorMessages.resx | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `1c52183` | Merge branch 'feature/manufacturing-catalog' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/manufacturing-statuses-product | `bfcd507` | feat(manufacturing): use the formula, order, lot and sampling statuses of the web app | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `739acc0` | Merge branch 'feature/manufacturing-statuses-product' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/shared-dateonly-mysql | `e8149b8` | fix(shared): read dates from mysql and give every bad request the same title | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `047ec3b` | Merge branch 'feature/shared-dateonly-mysql' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/organizations-facilities-activities | `d498300` | feat(organizations): add the facilities and administrative activities endpoints of the web app | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `42a2080` | Merge branch 'feature/organizations-facilities-activities' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/production-batch-statuses | `8c3a1a4` | feat(manufacturing): use the batch statuses of the web app with start, resume and release | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `6dfb1e4` | Merge branch 'feature/production-batch-statuses' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | feature/subscriptions-payments | `11bd41f` | feat(subscriptions): pay and renew subscriptions with niubiz like open | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | develop | `293cb56` | Merge branch 'feature/subscriptions-payments' into develop | — | 13/11/2026 |
| IngesCompanyCC/doofplus-platform | main | `3971afd` | Merge branch 'develop' into main | — | 14/11/2026 |

**Curso de Responsabilidad Social** (`develop`, 72 commits)

| Repository | Branch | Commit Id | Commit Message | Commit Message Body | Committed on (Date) |
|------------|--------|-----------|----------------|---------------------|---------------------|
| IngesCompanyCC/webdev-course-ingescompanycc | main | `f206173` | docs: add course plan template, readme and folder structure | — | 03/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-04 | `68d7f77` | feat(lesson-04): add the starter code | — | 05/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02 | `3bacd50` | feat(leccion-02): add the starter code in the starter-files directory | — | 05/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-04 | `5ab0100` | feat(lesson-04): add the completed example | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-04 | `10d931f` | docs(lesson-04): add the lesson script | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-04 | `28ef3db` | docs(lesson-04): add the recording script | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-04 | `f1cf575` | docs(slides): add the lesson slides | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01 | `1ed1e5b` | feat(lesson-01): add the starter code | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01 | `fb699d9` | chore: add gitignore | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01 | `f586b8c` | feat(lesson-01): add the completed lesson 1 example | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01 | `ae011d2` | docs(lesson-01): add the lesson script | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01 | `fcd91af` | docs(lesson-01): add the recording script | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01 | `e6f83a6` | docs(README): update course plan and lesson descriptions | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01 | `ed05d61` | chore: remove the template files | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `9a5911d` | Merge branch 'feature/leccion-01' into develop | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-04 | `16caaa5` | Merge branch 'develop' into feature/leccion-04 | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-04 | `50c251a` | docs: keep the recording scripts out of the repository | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `2b842c7` | Merge branch 'feature/leccion-04' into develop | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02 | `ff18199` | feat(lesson-02): add the starter code | — | 05/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02 | `8349bf2` | feat(lesson-02): add the completed example | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02 | `e213692` | Merge branch 'develop' into feature/leccion-02 | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02 | `6e76b86` | docs(lesson-02): add the lesson script | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02 | `1a8fc60` | build: add the pandoc settings for the course plan pdf | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02 | `3aefe96` | Merge remote-tracking branch 'origin/feature/leccion-02' into feature/leccion-02 | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `a1559b1` | Merge branch 'feature/leccion-02' into develop | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-06 | `86bfd02` | feat(lesson-06): add the starter code | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01-mini-challenge | `fd3336d` | feat(lesson-01): add the mini challenge to the completed example | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01-mini-challenge | `c5866f7` | docs(lesson-01): add the mini challenge to the lesson script | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-06 | `2aec6eb` | feat(lesson-06): add the completed example | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01-mini-challenge | `9a803cc` | docs(slides): add the mini challenge to the lesson 1 pause slide | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `a04b9a2` | Merge branch 'feature/leccion-01-mini-challenge' into develop | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/jsfiddle-labels | `afcedce` | docs: use the jsfiddle button and panel names the editor shows | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/jsfiddle-labels | `a940cdb` | docs(lesson-02): name jsfiddle instead of codepen in the lesson script | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `dff7f13` | Merge branch 'feature/jsfiddle-labels' into develop | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-06 | `e4edec3` | docs(lesson-06): add the lesson script | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-05 | `2fd379a` | feat(lesson-05): add the starter code | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-05 | `94c502e` | feat(lesson-05): add the completed example | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-05 | `e870429` | docs(lesson-05): add the lesson script | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-05 | `4a59122` | chore: ignore the build folder of the pdf | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `d3df359` | Merge branch 'feature/leccion-05' into develop | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-06 | `1b2b4db` | build: add the print styles for the course plan pdf | — | 06/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-06 | `0447d25` | Merge branch 'develop' into feature/leccion-06 | — | 07/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `ef2e37e` | Merge branch 'feature/leccion-06' into develop | — | 07/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-03 | `2f9a218` | feat(lesson-03): add the starter code | — | 07/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-03 | `1e33c24` | feat(lesson-03): add the completed example | — | 07/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-03 | `b60208b` | docs(lesson-03): add the lesson script | — | 07/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-03 | `5c7d245` | feat(assets): add the lesson images | — | 07/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `23584ca` | Merge branch 'feature/leccion-03' into develop | — | 07/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-04-starter | `0eadf49` | fix(lesson-04): leave the css starter file empty | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-04-starter | `8f710e9` | docs(lesson-04): name the text color and match the slides in the lesson script | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-01-script | `9f96bd8` | docs(lesson-01): add the mini challenge solution to the lesson script | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `fab2095` | Merge branch 'feature/leccion-04-starter' into develop | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02-starter | `e74a383` | fix(lesson-02): remove the placeholder comment from the starter code | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02-starter | `182dd40` | fix(lesson-02): add the comment written in the lesson to the completed example | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `a139102` | Merge branch 'feature/leccion-01-script' into develop | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-02-starter | `aebbbea` | docs(lesson-02): say what to undo after each test in the lesson script | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `2c45882` | Merge branch 'feature/leccion-02-starter' into develop | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-03-script | `e010a08` | docs(lesson-03): click the link only once it opens in a new tab | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-05-starter | `08aaf94` | fix(lesson-05): remove the placeholder comments from the starter code | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `b7699a3` | Merge branch 'feature/leccion-03-script' into develop | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-05-starter | `c2224cb` | docs(lesson-05): name every css rule written in the lesson script | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `3ccaa35` | Merge branch 'feature/leccion-05-starter' into develop | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-06-starter | `fe9bb16` | fix(lesson-06): add the section comments to the starter css | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-06-starter | `71371d7` | docs(lesson-06): match the slides and the editor steps in the lesson script | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `3d6dca0` | Merge branch 'feature/leccion-06-starter' into develop | — | 10/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/trs-scripts-and-slides | `6b8ae59` | docs(scripts): update the six lesson scripts with the new narration | — | 12/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/trs-scripts-and-slides | `3eedc0b` | docs(slides): remove the privacy rules from slides 12 and 30 | — | 12/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `fbba4dc` | Merge branch 'feature/trs-scripts-and-slides' into develop | — | 12/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-06-css-validator | `8101e92` | docs(lesson-06): show where the css validator is | — | 12/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `ef84ff1` | Merge branch 'feature/leccion-06-css-validator' into develop | — | 12/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | feature/leccion-06-restore-script | `75e3011` | docs(lesson-06): restore the lesson script narration | — | 12/11/2026 |
| IngesCompanyCC/webdev-course-ingescompanycc | develop | `cd516aa` | Merge branch 'feature/leccion-06-restore-script' into develop | — | 12/11/2026 |

#### 5.2.3.5. Execution Evidence for Sprint Review

En este Sprint el Website lleva a cada segmento a su entorno con un call to action propio, muestra los datos de contacto y las redes sociales y se publica en GitHub Pages. La Web Application recibe las consultas de Contact us en su zona pública, traduce los permisos de cada rol, busca por código desde la barra superior y enlaza con el Website. Los RESTful Web Services quedaron desplegados con los seis bounded contexts: IAM (registro, inicio de sesión con JWT y segundo factor, usuarios, roles e invitaciones), Organizations & Profiles, Subscriptions & Payments (con el pago por Niubiz), Manufacturing & Batch Management, IoT Monitoring y Quality & Compliance. La Web Application `v1.2.0` usa ese API en los seis bounded contexts y ya no usa la Fake API. Los datos de demostración del laboratorio (organización, plantas, usuarios, invitaciones, planes, Manufacturing, IoT y Quality) están cargados en la base de producción.

- **Website:** https://ingescompanycc.github.io/doofplus-website/
- **Web Application:** https://ingescompanycc-frontend.web.app
- **RESTful Web Services (Scalar):** https://doofplus-platform-aw.onrender.com/scalar

#### 5.2.3.6. Services Documentation Evidence for Sprint Review

Los RESTful Web Services se documentan con OpenAPI (Microsoft.AspNetCore.OpenApi) y Scalar. La versión `v1.1.0`, desplegada en Render, expone 90 rutas y 134 operaciones bajo el prefijo `/api/v1`; la especificación se puede descargar en https://doofplus-platform-aw.onrender.com/openapi/v1.json. La URL base publicada es `https://doofplus-platform-aw.onrender.com/api/v1`. La siguiente tabla documenta cada operación, con su enlace a Scalar. El servicio usa el plan gratuito de Render, que lo suspende cuando no recibe peticiones, por lo que la primera petición puede tardar cerca de un minuto.

| Endpoint | Verbo HTTP | Sintaxis | Parámetros | Response | URL documentación |
|---|---|---|---|---|---|
| `/administrative-activities` | GET | `/api/v1/administrative-activities` | `organizationId` (query) | 200: Get the administrative activities | https://doofplus-platform-aw.onrender.com/scalar/#tag/administrative-activities/GET/api/v1/administrative-activities |
| `/alerts` | GET | `/api/v1/alerts` | `sensorCode` (query), `batchCode` (query) | 200, 400: Get the alerts | https://doofplus-platform-aw.onrender.com/scalar/#tag/alerts/GET/api/v1/alerts |
| `/alerts` | POST | `/api/v1/alerts` | recurso en el body (JSON) | 201, 400, 409: Raise an alert | https://doofplus-platform-aw.onrender.com/scalar/#tag/alerts/POST/api/v1/alerts |
| `/alerts` | GET | `/api/v1/alerts/{alertId}` | `alertId` (path) | 200, 404: Get an alert by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/alerts/GET/api/v1/alerts/{alertId} |
| `/alerts` | PUT | `/api/v1/alerts/{alertId}` | `alertId` (path), recurso en el body (JSON) | 200, 400, 404: Update the handling of an alert | https://doofplus-platform-aw.onrender.com/scalar/#tag/alerts/PUT/api/v1/alerts/{alertId} |
| `/analytical-results` | GET | `/api/v1/analytical-results` | `batchCode` (query) | 200: Get the analytical results | https://doofplus-platform-aw.onrender.com/scalar/#tag/analytical-results/GET/api/v1/analytical-results |
| `/analytical-results` | POST | `/api/v1/analytical-results` | recurso en el body (JSON) | 201, 400: Create a analytical result | https://doofplus-platform-aw.onrender.com/scalar/#tag/analytical-results/POST/api/v1/analytical-results |
| `/analytical-results` | GET | `/api/v1/analytical-results/{analyticalResultId}` | `analyticalResultId` (path) | 200, 404: Get a analytical result by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/analytical-results/GET/api/v1/analytical-results/{analyticalResultId} |
| `/analytical-results` | PUT | `/api/v1/analytical-results/{analyticalResultId}` | `analyticalResultId` (path), recurso en el body (JSON) | 200, 400, 404: Update a analytical result | https://doofplus-platform-aw.onrender.com/scalar/#tag/analytical-results/PUT/api/v1/analytical-results/{analyticalResultId} |
| `/audit-events` | GET | `/api/v1/audit-events` | `record` (query) | 200: Get the audit events | https://doofplus-platform-aw.onrender.com/scalar/#tag/audit-events/GET/api/v1/audit-events |
| `/audit-events` | POST | `/api/v1/audit-events` | recurso en el body (JSON) | 201, 400: Create a audit event | https://doofplus-platform-aw.onrender.com/scalar/#tag/audit-events/POST/api/v1/audit-events |
| `/audit-events` | GET | `/api/v1/audit-events/{auditEventId}` | `auditEventId` (path) | 200, 404: Get a audit event by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/audit-events/GET/api/v1/audit-events/{auditEventId} |
| `/audit-findings` | GET | `/api/v1/audit-findings` | `auditCode` (query) | 200: Get the audit findings | https://doofplus-platform-aw.onrender.com/scalar/#tag/audit-findings/GET/api/v1/audit-findings |
| `/audit-findings` | POST | `/api/v1/audit-findings` | recurso en el body (JSON) | 201, 400: Create a audit finding | https://doofplus-platform-aw.onrender.com/scalar/#tag/audit-findings/POST/api/v1/audit-findings |
| `/audit-findings` | GET | `/api/v1/audit-findings/{auditFindingId}` | `auditFindingId` (path) | 200, 404: Get a audit finding by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/audit-findings/GET/api/v1/audit-findings/{auditFindingId} |
| `/audits` | GET | `/api/v1/audits` | — | 200: Get the audits | https://doofplus-platform-aw.onrender.com/scalar/#tag/audits/GET/api/v1/audits |
| `/audits` | POST | `/api/v1/audits` | recurso en el body (JSON) | 201, 400, 409: Create a audit | https://doofplus-platform-aw.onrender.com/scalar/#tag/audits/POST/api/v1/audits |
| `/audits` | GET | `/api/v1/audits/{auditId}` | `auditId` (path) | 200, 404: Get a audit by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/audits/GET/api/v1/audits/{auditId} |
| `/authentication` | POST | `/api/v1/authentication/sign-in` | recurso en el body (JSON) | 200, 401: Sign in | https://doofplus-platform-aw.onrender.com/scalar/#tag/authentication/POST/api/v1/authentication/sign-in |
| `/authentication` | POST | `/api/v1/authentication/sign-up` | recurso en el body (JSON) | 201, 400, 409: Sign up | https://doofplus-platform-aw.onrender.com/scalar/#tag/authentication/POST/api/v1/authentication/sign-up |
| `/batch-releases` | GET | `/api/v1/batch-releases` | — | 200: Get the batch releases | https://doofplus-platform-aw.onrender.com/scalar/#tag/batch-releases/GET/api/v1/batch-releases |
| `/batch-releases` | POST | `/api/v1/batch-releases` | recurso en el body (JSON) | 201, 400, 404, 409: Create a batch release | https://doofplus-platform-aw.onrender.com/scalar/#tag/batch-releases/POST/api/v1/batch-releases |
| `/batch-releases` | GET | `/api/v1/batch-releases/by-batch/{batchId}` | `batchId` (path) | 200: Get the batch releases of a batch | https://doofplus-platform-aw.onrender.com/scalar/#tag/batch-releases/GET/api/v1/batch-releases/by-batch/{batchId} |
| `/batch-releases` | GET | `/api/v1/batch-releases/{batchReleaseId}` | `batchReleaseId` (path) | 200, 404: Get a batch release by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/batch-releases/GET/api/v1/batch-releases/{batchReleaseId} |
| `/batch-releases` | PUT | `/api/v1/batch-releases/{batchReleaseId}/approve` | `batchReleaseId` (path), recurso en el body (JSON) | 200, 400, 404, 409: Approve a batch release | https://doofplus-platform-aw.onrender.com/scalar/#tag/batch-releases/PUT/api/v1/batch-releases/{batchReleaseId}/approve |
| `/batch-releases` | PUT | `/api/v1/batch-releases/{batchReleaseId}/reject` | `batchReleaseId` (path), recurso en el body (JSON) | 200, 400, 404, 409: Reject a batch release | https://doofplus-platform-aw.onrender.com/scalar/#tag/batch-releases/PUT/api/v1/batch-releases/{batchReleaseId}/reject |
| `/capa-actions` | GET | `/api/v1/capa-actions` | `capaCode` (query) | 200: Get the capa actions | https://doofplus-platform-aw.onrender.com/scalar/#tag/capa-actions/GET/api/v1/capa-actions |
| `/capa-actions` | POST | `/api/v1/capa-actions` | recurso en el body (JSON) | 201, 400: Create a capa action | https://doofplus-platform-aw.onrender.com/scalar/#tag/capa-actions/POST/api/v1/capa-actions |
| `/capa-actions` | GET | `/api/v1/capa-actions/{capaActionId}` | `capaActionId` (path) | 200, 404: Get a capa action by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/capa-actions/GET/api/v1/capa-actions/{capaActionId} |
| `/capa-actions` | PUT | `/api/v1/capa-actions/{capaActionId}` | `capaActionId` (path), recurso en el body (JSON) | 200, 400, 404: Update a capa action | https://doofplus-platform-aw.onrender.com/scalar/#tag/capa-actions/PUT/api/v1/capa-actions/{capaActionId} |
| `/capa-plans` | GET | `/api/v1/capa-plans` | `sourceDeviation` (query) | 200: Get the capa plans | https://doofplus-platform-aw.onrender.com/scalar/#tag/capa-plans/GET/api/v1/capa-plans |
| `/capa-plans` | POST | `/api/v1/capa-plans` | recurso en el body (JSON) | 201, 400, 409: Create a capa plan | https://doofplus-platform-aw.onrender.com/scalar/#tag/capa-plans/POST/api/v1/capa-plans |
| `/capa-plans` | GET | `/api/v1/capa-plans/{capaPlanId}` | `capaPlanId` (path) | 200, 404: Get a capa plan by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/capa-plans/GET/api/v1/capa-plans/{capaPlanId} |
| `/capa-plans` | PUT | `/api/v1/capa-plans/{capaPlanId}` | `capaPlanId` (path), recurso en el body (JSON) | 200, 400, 404: Update a capa plan | https://doofplus-platform-aw.onrender.com/scalar/#tag/capa-plans/PUT/api/v1/capa-plans/{capaPlanId} |
| `/collaboration-tasks` | GET | `/api/v1/collaboration-tasks` | `recordCode` (query) | 200: Get the collaboration tasks | https://doofplus-platform-aw.onrender.com/scalar/#tag/collaboration-tasks/GET/api/v1/collaboration-tasks |
| `/collaboration-tasks` | POST | `/api/v1/collaboration-tasks` | recurso en el body (JSON) | 201, 400: Create a collaboration task | https://doofplus-platform-aw.onrender.com/scalar/#tag/collaboration-tasks/POST/api/v1/collaboration-tasks |
| `/collaboration-tasks` | GET | `/api/v1/collaboration-tasks/{collaborationTaskId}` | `collaborationTaskId` (path) | 200, 404: Get a collaboration task by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/collaboration-tasks/GET/api/v1/collaboration-tasks/{collaborationTaskId} |
| `/contact-inquiries` | GET | `/api/v1/contact-inquiries` | — | 401, 403, 200: Get all contact inquiries | https://doofplus-platform-aw.onrender.com/scalar/#tag/contact-inquiries/GET/api/v1/contact-inquiries |
| `/contact-inquiries` | POST | `/api/v1/contact-inquiries` | recurso en el body (JSON) | 201, 400: Send contact inquiry | https://doofplus-platform-aw.onrender.com/scalar/#tag/contact-inquiries/POST/api/v1/contact-inquiries |
| `/contact-inquiries` | GET | `/api/v1/contact-inquiries/{inquiryId}` | `inquiryId` (path) | 401, 403, 200, 404: Get contact inquiry by id | https://doofplus-platform-aw.onrender.com/scalar/#tag/contact-inquiries/GET/api/v1/contact-inquiries/{inquiryId} |
| `/deviation-trends` | GET | `/api/v1/deviation-trends` | — | 200: Get the deviation trend | https://doofplus-platform-aw.onrender.com/scalar/#tag/deviation-trends/GET/api/v1/deviation-trends |
| `/deviations` | GET | `/api/v1/deviations` | `batchCode` (query) | 200: Get the deviations | https://doofplus-platform-aw.onrender.com/scalar/#tag/deviations/GET/api/v1/deviations |
| `/deviations` | POST | `/api/v1/deviations` | recurso en el body (JSON) | 201, 400, 409: Create a deviation | https://doofplus-platform-aw.onrender.com/scalar/#tag/deviations/POST/api/v1/deviations |
| `/deviations` | GET | `/api/v1/deviations/{deviationId}` | `deviationId` (path) | 200, 404: Get a deviation by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/deviations/GET/api/v1/deviations/{deviationId} |
| `/deviations` | PUT | `/api/v1/deviations/{deviationId}` | `deviationId` (path), recurso en el body (JSON) | 200, 400, 404: Update a deviation | https://doofplus-platform-aw.onrender.com/scalar/#tag/deviations/PUT/api/v1/deviations/{deviationId} |
| `/equipment` | GET | `/api/v1/equipment` | `code` (query) | 200, 400, 404: Get the equipment | https://doofplus-platform-aw.onrender.com/scalar/#tag/equipment/GET/api/v1/equipment |
| `/equipment` | POST | `/api/v1/equipment` | recurso en el body (JSON) | 201, 400, 409: Register a piece of equipment | https://doofplus-platform-aw.onrender.com/scalar/#tag/equipment/POST/api/v1/equipment |
| `/equipment` | GET | `/api/v1/equipment/{equipmentId}` | `equipmentId` (path) | 200, 404: Get a piece of equipment by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/equipment/GET/api/v1/equipment/{equipmentId} |
| `/equipment` | PUT | `/api/v1/equipment/{equipmentId}` | `equipmentId` (path), recurso en el body (JSON) | 200, 400, 404: Update a piece of equipment | https://doofplus-platform-aw.onrender.com/scalar/#tag/equipment/PUT/api/v1/equipment/{equipmentId} |
| `/equipment` | DELETE | `/api/v1/equipment/{equipmentId}` | `equipmentId` (path) | 204, 404, 409: Remove a piece of equipment | https://doofplus-platform-aw.onrender.com/scalar/#tag/equipment/DELETE/api/v1/equipment/{equipmentId} |
| `/evidence` | GET | `/api/v1/evidence` | `recordCode` (query) | 200: Get the evidence | https://doofplus-platform-aw.onrender.com/scalar/#tag/evidence/GET/api/v1/evidence |
| `/evidence` | POST | `/api/v1/evidence` | recurso en el body (JSON) | 201, 400: Create a evidence | https://doofplus-platform-aw.onrender.com/scalar/#tag/evidence/POST/api/v1/evidence |
| `/evidence` | GET | `/api/v1/evidence/{evidenceId}` | `evidenceId` (path) | 200, 404: Get a evidence by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/evidence/GET/api/v1/evidence/{evidenceId} |
| `/evidence` | PUT | `/api/v1/evidence/{evidenceId}` | `evidenceId` (path), recurso en el body (JSON) | 200, 400, 404: Update a evidence | https://doofplus-platform-aw.onrender.com/scalar/#tag/evidence/PUT/api/v1/evidence/{evidenceId} |
| `/evidence` | DELETE | `/api/v1/evidence/{evidenceId}` | `evidenceId` (path) | 204, 404, 409: Remove a evidence | https://doofplus-platform-aw.onrender.com/scalar/#tag/evidence/DELETE/api/v1/evidence/{evidenceId} |
| `/facilities` | GET | `/api/v1/facilities` | `organizationId` (query) | 200: Get facilities | https://doofplus-platform-aw.onrender.com/scalar/#tag/facilities/GET/api/v1/facilities |
| `/facilities` | POST | `/api/v1/facilities` | recurso en el body (JSON) | 201, 400, 404, 409: Add a facility | https://doofplus-platform-aw.onrender.com/scalar/#tag/facilities/POST/api/v1/facilities |
| `/invitations` | GET | `/api/v1/invitations` | `organizationId` (query) | 200, 401, 403: Get the invitations | https://doofplus-platform-aw.onrender.com/scalar/#tag/invitations/GET/api/v1/invitations |
| `/invitations` | POST | `/api/v1/invitations` | recurso en el body (JSON) | 201, 400, 401, 403, 409: Send an invitation | https://doofplus-platform-aw.onrender.com/scalar/#tag/invitations/POST/api/v1/invitations |
| `/invitations` | GET | `/api/v1/invitations/{invitationId}` | `invitationId` (path) | 200, 404: Get an invitation by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/invitations/GET/api/v1/invitations/{invitationId} |
| `/master-formulas` | GET | `/api/v1/master-formulas` | `productCode` (query) | 200, 400: Get the master formulas | https://doofplus-platform-aw.onrender.com/scalar/#tag/master-formulas/GET/api/v1/master-formulas |
| `/master-formulas` | POST | `/api/v1/master-formulas` | recurso en el body (JSON) | 201, 400, 404, 409: Write a master formula | https://doofplus-platform-aw.onrender.com/scalar/#tag/master-formulas/POST/api/v1/master-formulas |
| `/master-formulas` | GET | `/api/v1/master-formulas/{masterFormulaId}` | `masterFormulaId` (path) | 200, 404: Get a master formula by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/master-formulas/GET/api/v1/master-formulas/{masterFormulaId} |
| `/master-formulas` | POST | `/api/v1/master-formulas/{masterFormulaId}/approval` | `masterFormulaId` (path), recurso en el body (JSON) | 200, 400, 404, 409: Approve a master formula | https://doofplus-platform-aw.onrender.com/scalar/#tag/master-formulas/POST/api/v1/master-formulas/{masterFormulaId}/approval |
| `/material-lots` | GET | `/api/v1/material-lots` | — | 200: Get all lots | https://doofplus-platform-aw.onrender.com/scalar/#tag/material-lots/GET/api/v1/material-lots |
| `/material-lots` | POST | `/api/v1/material-lots` | recurso en el body (JSON) | 201, 400, 409: Receive a lot | https://doofplus-platform-aw.onrender.com/scalar/#tag/material-lots/POST/api/v1/material-lots |
| `/material-lots` | GET | `/api/v1/material-lots/{rawMaterialLotId}` | `rawMaterialLotId` (path) | 200, 404: Get a lot by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/material-lots/GET/api/v1/material-lots/{rawMaterialLotId} |
| `/material-lots` | POST | `/api/v1/material-lots/{rawMaterialLotId}/allocation` | `rawMaterialLotId` (path), recurso en el body (JSON) | 200, 404, 409: Allocate a lot to a batch | https://doofplus-platform-aw.onrender.com/scalar/#tag/material-lots/POST/api/v1/material-lots/{rawMaterialLotId}/allocation |
| `/material-lots` | POST | `/api/v1/material-lots/{rawMaterialLotId}/approval` | `rawMaterialLotId` (path) | 200, 404, 409: Release a lot | https://doofplus-platform-aw.onrender.com/scalar/#tag/material-lots/POST/api/v1/material-lots/{rawMaterialLotId}/approval |
| `/material-lots` | PUT | `/api/v1/material-lots/{rawMaterialLotId}/inspection` | `rawMaterialLotId` (path), recurso en el body (JSON) | 200, 400, 404, 409: Record the inspection of a lot | https://doofplus-platform-aw.onrender.com/scalar/#tag/material-lots/PUT/api/v1/material-lots/{rawMaterialLotId}/inspection |
| `/material-lots` | POST | `/api/v1/material-lots/{rawMaterialLotId}/rejection` | `rawMaterialLotId` (path) | 200, 404, 409: Reject a lot | https://doofplus-platform-aw.onrender.com/scalar/#tag/material-lots/POST/api/v1/material-lots/{rawMaterialLotId}/rejection |
| `/organizations` | GET | `/api/v1/organizations` | `ruc` (query) | 200: Get every organization, or the one registered with a RUC | https://doofplus-platform-aw.onrender.com/scalar/#tag/organizations/GET/api/v1/organizations |
| `/organizations` | POST | `/api/v1/organizations` | recurso en el body (JSON) | 201, 400, 409: Register organization | https://doofplus-platform-aw.onrender.com/scalar/#tag/organizations/POST/api/v1/organizations |
| `/organizations` | GET | `/api/v1/organizations/{organizationId}` | `organizationId` (path) | 200, 404: Get organization by id | https://doofplus-platform-aw.onrender.com/scalar/#tag/organizations/GET/api/v1/organizations/{organizationId} |
| `/plans` | GET | `/api/v1/plans` | — | 200: Get all plans | https://doofplus-platform-aw.onrender.com/scalar/#tag/plans/GET/api/v1/plans |
| `/plans` | POST | `/api/v1/plans` | recurso en el body (JSON) | 201, 400, 409: Create a plan | https://doofplus-platform-aw.onrender.com/scalar/#tag/plans/POST/api/v1/plans |
| `/plans` | GET | `/api/v1/plans/{planId}` | `planId` (path) | 200, 404: Get a plan by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/plans/GET/api/v1/plans/{planId} |
| `/production-batches` | GET | `/api/v1/production-batches` | `batchNumber` (query) | 200, 404: Get all production batches or filter by batch number | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/GET/api/v1/production-batches |
| `/production-batches` | POST | `/api/v1/production-batches` | recurso en el body (JSON) | 201, 400, 409: Create a new production batch | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/POST/api/v1/production-batches |
| `/production-batches` | GET | `/api/v1/production-batches/by-number/{batchNumber}` | `batchNumber` (path) | 200, 404: Get production batch by batch number | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/GET/api/v1/production-batches/by-number/{batchNumber} |
| `/production-batches` | GET | `/api/v1/production-batches/{id}` | `id` (path) | 200, 404: Get production batch by ID | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/GET/api/v1/production-batches/{id} |
| `/production-batches` | POST | `/api/v1/production-batches/{id}/close` | `id` (path), recurso en el body (JSON) | 200, 400, 404, 409: Close a production batch | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/POST/api/v1/production-batches/{id}/close |
| `/production-batches` | POST | `/api/v1/production-batches/{id}/incidents` | `id` (path), recurso en el body (JSON) | 201, 400, 404: Register an incident in a production batch | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/POST/api/v1/production-batches/{id}/incidents |
| `/production-batches` | PATCH | `/api/v1/production-batches/{id}/progress` | `id` (path), recurso en el body (JSON) | 200, 400, 404: Record the progress of a production batch | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/PATCH/api/v1/production-batches/{id}/progress |
| `/production-batches` | POST | `/api/v1/production-batches/{id}/release` | `id` (path), recurso en el body (JSON) | 200, 404, 409: Release a production batch | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/POST/api/v1/production-batches/{id}/release |
| `/production-batches` | POST | `/api/v1/production-batches/{id}/resume` | `id` (path) | 200, 404, 409: Resume a production batch | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/POST/api/v1/production-batches/{id}/resume |
| `/production-batches` | POST | `/api/v1/production-batches/{id}/start` | `id` (path) | 200, 404, 409: Start a production batch | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-batches/POST/api/v1/production-batches/{id}/start |
| `/production-orders` | GET | `/api/v1/production-orders` | — | 200: Get all production orders | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-orders/GET/api/v1/production-orders |
| `/production-orders` | POST | `/api/v1/production-orders` | recurso en el body (JSON) | 201, 400, 404, 409: Plan a production order | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-orders/POST/api/v1/production-orders |
| `/production-orders` | GET | `/api/v1/production-orders/{productionOrderId}` | `productionOrderId` (path) | 200, 404: Get a production order by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-orders/GET/api/v1/production-orders/{productionOrderId} |
| `/production-orders` | PUT | `/api/v1/production-orders/{productionOrderId}/operations/{sequence}` | `productionOrderId` (path), `sequence` (path), recurso en el body (JSON) | 200, 400, 404: Record the progress of an operation | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-orders/PUT/api/v1/production-orders/{productionOrderId}/operations/{sequence} |
| `/production-orders` | PUT | `/api/v1/production-orders/{productionOrderId}/status` | `productionOrderId` (path), recurso en el body (JSON) | 200, 400, 404, 409: Change the status of a production order | https://doofplus-platform-aw.onrender.com/scalar/#tag/production-orders/PUT/api/v1/production-orders/{productionOrderId}/status |
| `/products` | GET | `/api/v1/products` | — | 200: Get all products | https://doofplus-platform-aw.onrender.com/scalar/#tag/products/GET/api/v1/products |
| `/products` | POST | `/api/v1/products` | recurso en el body (JSON) | 201, 400, 409: Register a product | https://doofplus-platform-aw.onrender.com/scalar/#tag/products/POST/api/v1/products |
| `/products` | GET | `/api/v1/products/{productId}` | `productId` (path) | 200, 404: Get a product by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/products/GET/api/v1/products/{productId} |
| `/products` | PUT | `/api/v1/products/{productId}` | `productId` (path), recurso en el body (JSON) | 200, 400, 404: Update a product | https://doofplus-platform-aw.onrender.com/scalar/#tag/products/PUT/api/v1/products/{productId} |
| `/quality-documents` | GET | `/api/v1/quality-documents` | `code` (query) | 200: Get the quality documents | https://doofplus-platform-aw.onrender.com/scalar/#tag/quality-documents/GET/api/v1/quality-documents |
| `/quality-documents` | POST | `/api/v1/quality-documents` | recurso en el body (JSON) | 201, 400: Create a quality document | https://doofplus-platform-aw.onrender.com/scalar/#tag/quality-documents/POST/api/v1/quality-documents |
| `/quality-documents` | GET | `/api/v1/quality-documents/{qualityDocumentId}` | `qualityDocumentId` (path) | 200, 404: Get a quality document by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/quality-documents/GET/api/v1/quality-documents/{qualityDocumentId} |
| `/quality-documents` | PUT | `/api/v1/quality-documents/{qualityDocumentId}` | `qualityDocumentId` (path), recurso en el body (JSON) | 200, 400, 404: Update a quality document | https://doofplus-platform-aw.onrender.com/scalar/#tag/quality-documents/PUT/api/v1/quality-documents/{qualityDocumentId} |
| `/readings` | GET | `/api/v1/readings` | `sensorCode` (query), `batchCode` (query) | 200, 400: Get the readings | https://doofplus-platform-aw.onrender.com/scalar/#tag/readings/GET/api/v1/readings |
| `/readings` | POST | `/api/v1/readings` | recurso en el body (JSON) | 201, 400, 404: Record a reading | https://doofplus-platform-aw.onrender.com/scalar/#tag/readings/POST/api/v1/readings |
| `/readings` | GET | `/api/v1/readings/{readingId}` | `readingId` (path) | 200, 404: Get a reading by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/readings/GET/api/v1/readings/{readingId} |
| `/regulatory-reports` | GET | `/api/v1/regulatory-reports` | — | 200: Get the regulatory reports | https://doofplus-platform-aw.onrender.com/scalar/#tag/regulatory-reports/GET/api/v1/regulatory-reports |
| `/regulatory-reports` | POST | `/api/v1/regulatory-reports` | recurso en el body (JSON) | 201, 400, 409: Create a regulatory report | https://doofplus-platform-aw.onrender.com/scalar/#tag/regulatory-reports/POST/api/v1/regulatory-reports |
| `/regulatory-reports` | GET | `/api/v1/regulatory-reports/{regulatoryReportId}` | `regulatoryReportId` (path) | 200, 404: Get a regulatory report by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/regulatory-reports/GET/api/v1/regulatory-reports/{regulatoryReportId} |
| `/roles` | GET | `/api/v1/roles` | — | 200, 401: Get all roles | https://doofplus-platform-aw.onrender.com/scalar/#tag/roles/GET/api/v1/roles |
| `/sensors` | GET | `/api/v1/sensors` | `equipmentCode` (query) | 200, 400: Get the sensors | https://doofplus-platform-aw.onrender.com/scalar/#tag/sensors/GET/api/v1/sensors |
| `/sensors` | POST | `/api/v1/sensors` | recurso en el body (JSON) | 201, 400, 404, 409: Register a sensor | https://doofplus-platform-aw.onrender.com/scalar/#tag/sensors/POST/api/v1/sensors |
| `/sensors` | GET | `/api/v1/sensors/{sensorId}` | `sensorId` (path) | 200, 404: Get a sensor by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/sensors/GET/api/v1/sensors/{sensorId} |
| `/sensors` | PUT | `/api/v1/sensors/{sensorId}` | `sensorId` (path), recurso en el body (JSON) | 200, 400, 404: Update a sensor | https://doofplus-platform-aw.onrender.com/scalar/#tag/sensors/PUT/api/v1/sensors/{sensorId} |
| `/subscriptions` | GET | `/api/v1/subscriptions` | `organizationId` (query) | 200, 400: Get the subscriptions of an organization | https://doofplus-platform-aw.onrender.com/scalar/#tag/subscriptions/GET/api/v1/subscriptions |
| `/subscriptions` | POST | `/api/v1/subscriptions` | recurso en el body (JSON) | 201, 400, 404, 409: Create a subscription | https://doofplus-platform-aw.onrender.com/scalar/#tag/subscriptions/POST/api/v1/subscriptions |
| `/subscriptions` | GET | `/api/v1/subscriptions/{subscriptionId}` | `subscriptionId` (path) | 200, 404: Get a subscription by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/subscriptions/GET/api/v1/subscriptions/{subscriptionId} |
| `/subscriptions` | POST | `/api/v1/subscriptions/{subscriptionId}/activation` | `subscriptionId` (path) | 200, 404, 409: Activate a subscription | https://doofplus-platform-aw.onrender.com/scalar/#tag/subscriptions/POST/api/v1/subscriptions/{subscriptionId}/activation |
| `/subscriptions` | POST | `/api/v1/subscriptions/{subscriptionId}/cancellation` | `subscriptionId` (path) | 200, 404, 409: Cancel a subscription | https://doofplus-platform-aw.onrender.com/scalar/#tag/subscriptions/POST/api/v1/subscriptions/{subscriptionId}/cancellation |
| `/subscriptions` | POST | `/api/v1/subscriptions/{subscriptionId}/payments` | `subscriptionId` (path), recurso en el body (JSON) | 200, 400, 404, 409, 422, 503: Pay a subscription | https://doofplus-platform-aw.onrender.com/scalar/#tag/subscriptions/POST/api/v1/subscriptions/{subscriptionId}/payments |
| `/subscriptions` | PUT | `/api/v1/subscriptions/{subscriptionId}/plan` | `subscriptionId` (path), recurso en el body (JSON) | 200, 400, 404, 409: Change the plan of a subscription | https://doofplus-platform-aw.onrender.com/scalar/#tag/subscriptions/PUT/api/v1/subscriptions/{subscriptionId}/plan |
| `/subscriptions` | POST | `/api/v1/subscriptions/{subscriptionId}/renewal` | `subscriptionId` (path) | 200, 404, 409: Renew a subscription | https://doofplus-platform-aw.onrender.com/scalar/#tag/subscriptions/POST/api/v1/subscriptions/{subscriptionId}/renewal |
| `/task-comments` | GET | `/api/v1/task-comments` | `recordCode` (query) | 200: Get the task comments | https://doofplus-platform-aw.onrender.com/scalar/#tag/task-comments/GET/api/v1/task-comments |
| `/task-comments` | POST | `/api/v1/task-comments` | recurso en el body (JSON) | 201, 400: Create a task comment | https://doofplus-platform-aw.onrender.com/scalar/#tag/task-comments/POST/api/v1/task-comments |
| `/task-comments` | GET | `/api/v1/task-comments/{taskCommentId}` | `taskCommentId` (path) | 200, 404: Get a task comment by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/task-comments/GET/api/v1/task-comments/{taskCommentId} |
| `/telemetry` | POST | `/api/v1/telemetry` | `X-Api-Key` (header), recurso en el body (JSON) | 202, 400, 401, 404: Receive a reading from ThingsBoard | https://doofplus-platform-aw.onrender.com/scalar/#tag/telemetry/POST/api/v1/telemetry |
| `/tokens` | POST | `/api/v1/tokens` | recurso en el body (JSON) | 200, 401: Create authentication token (sign-in) | https://doofplus-platform-aw.onrender.com/scalar/#tag/tokens/POST/api/v1/tokens |
| `/traceability-gaps` | GET | `/api/v1/traceability-gaps` | `batchCode` (query) | 200: Get the traceability gaps | https://doofplus-platform-aw.onrender.com/scalar/#tag/traceability-gaps/GET/api/v1/traceability-gaps |
| `/traceability-gaps` | POST | `/api/v1/traceability-gaps` | recurso en el body (JSON) | 201, 400: Create a traceability gap | https://doofplus-platform-aw.onrender.com/scalar/#tag/traceability-gaps/POST/api/v1/traceability-gaps |
| `/traceability-gaps` | GET | `/api/v1/traceability-gaps/{traceabilityGapId}` | `traceabilityGapId` (path) | 200, 404: Get a traceability gap by its id | https://doofplus-platform-aw.onrender.com/scalar/#tag/traceability-gaps/GET/api/v1/traceability-gaps/{traceabilityGapId} |
| `/user-profiles` | GET | `/api/v1/user-profiles` | `organizationId` (query) | 200: Get every user profile, or the ones of an organization | https://doofplus-platform-aw.onrender.com/scalar/#tag/user-profiles/GET/api/v1/user-profiles |
| `/user-profiles` | POST | `/api/v1/user-profiles` | recurso en el body (JSON) | 201, 400, 404, 409: Create user profile | https://doofplus-platform-aw.onrender.com/scalar/#tag/user-profiles/POST/api/v1/user-profiles |
| `/user-profiles` | GET | `/api/v1/user-profiles/{profileId}` | `profileId` (path) | 200, 404: Get user profile by id | https://doofplus-platform-aw.onrender.com/scalar/#tag/user-profiles/GET/api/v1/user-profiles/{profileId} |
| `/user-profiles` | PUT | `/api/v1/user-profiles/{profileId}` | `profileId` (path), recurso en el body (JSON) | 200, 404: Update the notification preferences and the language of a user profile | https://doofplus-platform-aw.onrender.com/scalar/#tag/user-profiles/PUT/api/v1/user-profiles/{profileId} |
| `/users` | GET | `/api/v1/users` | — | 200, 401, 403: Get all users | https://doofplus-platform-aw.onrender.com/scalar/#tag/users/GET/api/v1/users |
| `/users` | POST | `/api/v1/users` | recurso en el body (JSON) | 201, 400, 409: Create user (sign-up) | https://doofplus-platform-aw.onrender.com/scalar/#tag/users/POST/api/v1/users |
| `/users` | GET | `/api/v1/users/{userId}` | `userId` (path) | 200, 401, 404: Get user by id | https://doofplus-platform-aw.onrender.com/scalar/#tag/users/GET/api/v1/users/{userId} |

#### 5.2.3.7. Software Deployment Evidence for Sprint Review

En este Sprint se publicaron las siguientes versiones, siguiendo GitFlow:

- **Website:** `develop` se fusionó en `main` el 30/10/2026 y el 14/11/2026, con el tag `v1.1.0`. El Website se publica en GitHub Pages desde `main`. URL: https://ingescompanycc.github.io/doofplus-website/.
- **Web Application:** `develop` se fusionó en `main` el 30/10/2026 y el 14/11/2026, con el tag `v1.1.0`, con Contact us y los permisos por rol. Después se publicó `v1.2.0` (15/11/2026), con los seis bounded contexts en el API, las invitaciones y los perfiles de rol, y sin la Fake API. Se publicó en Firebase Hosting con `npm run build` y `firebase deploy --only hosting`. URL: https://ingescompanycc-frontend.web.app.
- **RESTful Web Services:** `develop` se fusionó en `main` con el tag `v1.0.0` (14/11/2026). Después se publicó `release/v1.1.0` (15/11/2026), con IAM (JWT y segundo factor), Quality & Compliance según los diagramas y las invitaciones de nuevos miembros. El API se publicó en un Web Service de Render con el `Dockerfile` del repositorio (`sdk:10.0` para compilar y `aspnet:10.0` para correr), que se despliega automáticamente con cada push a `main`, y su base de datos MySQL 8 en Railway; las migraciones de Entity Framework Core se aplican al arrancar. Las credenciales se cargan como variables de entorno del servicio y no están en el repositorio. URL: https://doofplus-platform-aw.onrender.com y Scalar en https://doofplus-platform-aw.onrender.com/scalar. Se comprobó que `POST /api/v1/contact-inquiries` responde 201 y que los `GET` de cada bounded context responden 200.

#### 5.2.3.8. Team Collaboration Insights during Sprint

Los cinco integrantes participaron en el Sprint. La siguiente tabla resume los commits de cada integrante en el Sprint (desde el 18/10/2026, sin contar los commits de merge): en `main` del Website, de la Web Application y de los RESTful Web Services, y en `develop` del curso de Responsabilidad Social. Los analíticos de GitHub de cada repositorio están en *Insights › Contributors* y *Insights › Network*:

| Team Member | GitHub Username | Website | Web Application | RESTful Web Services | Curso (develop) | Total |
|---|---|:---:|:---:|:---:|:---:|:---:|
| Rojas Ambicho, Nestor Daniel | danRO-20 | 22 | 37 | 52 | 30 | 141 |
| Yarleque Ruiz, Cristina Marcela | Cris06luna | 3 | 0 | 37 | 4 | 44 |
| Zavaleta Gutierrez, Rodolfo Martin | gutierrezrodolfo360-bit | 1 | 3 | 31 | 7 | 42 |
| Angulo Ramírez, Marcelo Martín | Zock2005 | 0 | 0 | 27 | 5 | 32 |
| Cobades Zamora, Yhoshua Hebert | YhoshuaCZ | 0 | 0 | 8 | 4 | 12 |

- **Website:** https://github.com/IngesCompanyCC/doofplus-website/graphs/contributors y https://github.com/IngesCompanyCC/doofplus-website/network
- **Web Application:** https://github.com/IngesCompanyCC/doofplus-web-application/graphs/contributors y https://github.com/IngesCompanyCC/doofplus-web-application/network
- **RESTful Web Services:** https://github.com/IngesCompanyCC/doofplus-platform/graphs/contributors y https://github.com/IngesCompanyCC/doofplus-platform/network

## 5.3. Validation Interviews

### 5.3.1. Diseño de Entrevistas

El diseño de las entrevistas de validación se centró en evaluar la usabilidad, funcionalidad y aceptación de **DoofPlus**, mediante la interacción de representantes de los dos segmentos objetivo con la versión implementada de la plataforma. El propósito fue identificar si las funcionalidades desarrolladas responden a las necesidades de los especialistas de Aseguramiento y Control de Calidad (QA/QC) y de los Jefes o Supervisores de Producción Farmacéutica, así como reconocer posibles dificultades durante la navegación y ejecución de sus actividades.

Para obtener información relevante sobre la experiencia de los usuarios, las entrevistas se estructuraron en cuatro bloques principales: perfil y contexto, expectativas del producto, evaluación del prototipo y cierre. Asimismo, se combinaron preguntas abiertas con actividades de exploración guiada y libre, permitiendo observar el comportamiento de los participantes, conocer sus percepciones y recopilar oportunidades de mejora para la solución.

Las preguntas se organizaron de acuerdo con las responsabilidades y necesidades de cada segmento objetivo, considerando los perfiles de usuario definidos previamente en la sección 2.3.1.

### Preguntas para el Segmento: Especialista de Aseguramiento y Control de Calidad (QA/QC)

**Perfil y contexto**

1. ¿Cuál es su cargo actual, qué edad tiene y cuántos años de experiencia posee en el área de aseguramiento o control de calidad farmacéutica?
2. ¿En qué distrito reside y cuáles son las principales responsabilidades que desempeña dentro de su organización?
3. ¿Qué herramientas, dispositivos y sistemas digitales utiliza habitualmente para gestionar documentación, registros y procesos de calidad?
4. ¿Qué tan familiarizado se encuentra con el uso de plataformas digitales para la gestión de calidad farmacéutica?

**Expectativas del producto**

5. ¿Qué funcionalidades considera más importantes en una plataforma destinada a centralizar la gestión de calidad farmacéutica?
6. ¿De qué manera espera que una plataforma como DoofPlus facilite la gestión documental y la trazabilidad de los lotes farmacéuticos?
7. ¿Qué aspectos considera indispensables para confiar en la integridad, disponibilidad y seguimiento de los registros digitales?

**Evaluación del prototipo**

8. Después de explorar DoofPlus, ¿qué tan fácil le resultó localizar la información relacionada con un lote farmacéutico?
9. ¿Cómo describiría su experiencia al consultar protocolos, procedimientos operativos estándar (SOP) y registros de calidad?
10. ¿Considera que las funcionalidades de registro y seguimiento de desviaciones y acciones correctivas y preventivas (CAPA) permiten comprender adecuadamente el proceso de gestión?
11. ¿Qué dificultades encontró al navegar por las funcionalidades de la plataforma o realizar las actividades presentadas?
12. ¿De qué manera considera que DoofPlus podría mejorar la recopilación de evidencias y la preparación de auditorías e inspecciones regulatorias?

**Cierre**

13. En una escala del 1 al 10, ¿qué tan probable sería que recomendara DoofPlus a otros profesionales del área de aseguramiento y control de calidad? ¿Por qué?
14. ¿Qué funcionalidades o mejoras considera necesarias antes de utilizar la plataforma en su organización?

### Preguntas para el Segmento: Jefe o Supervisor de Producción Farmacéutica

**Perfil y contexto**

1. ¿Cuál es su cargo actual, qué edad tiene y cuántos años de experiencia posee supervisando procesos de producción farmacéutica?
2. ¿En qué distrito reside y cuáles son las principales responsabilidades que desempeña dentro de los procesos de fabricación?
3. ¿Qué herramientas, dispositivos y sistemas digitales utiliza habitualmente para supervisar la fabricación y consultar información de los lotes?
4. ¿Qué tan familiarizado se encuentra con el uso de plataformas digitales para el seguimiento y control de procesos productivos?

**Expectativas del producto**

5. ¿Qué información considera indispensable para supervisar el estado de los lotes y tomar decisiones durante la fabricación?
6. ¿De qué manera espera que una plataforma como DoofPlus facilite la coordinación entre las áreas de Producción y Calidad?
7. ¿Qué beneficios esperaría obtener al contar con información centralizada sobre órdenes de producción, materias primas e incidencias operativas?

**Evaluación del prototipo**

8. Después de explorar DoofPlus, ¿qué tan fácil le resultó consultar el estado y el historial de los lotes de producción?
9. ¿Cómo describiría su experiencia al consultar la información relacionada con las órdenes de producción, las fórmulas maestras y las materias primas?
10. ¿Considera que las funcionalidades de registro y seguimiento de incidencias facilitan la comunicación y coordinación con el área de Calidad?
11. ¿Qué tan útil considera la visualización de información procedente de dispositivos IoT para supervisar los procesos de fabricación y detectar posibles incidencias?
12. ¿Qué dificultades encontró al navegar por la plataforma o realizar las actividades relacionadas con la supervisión de producción?

**Cierre**

13. En una escala del 1 al 10, ¿qué tan probable sería que recomendara DoofPlus a otros responsables de producción farmacéutica? ¿Por qué?
14. ¿Qué funcionalidades o mejoras considera necesarias antes de utilizar la plataforma en su organización?

### Actividades de evaluación del prototipo

Además de las preguntas planteadas, se propone realizar una evaluación práctica para observar cómo los participantes interactúan con las funcionalidades de DoofPlus. Para ello, se combinará una demostración guiada de las principales características de la plataforma con un periodo de exploración libre, durante el cual cada entrevistado podrá navegar por las funcionalidades relacionadas con sus responsabilidades.

Las actividades se diferenciarán según el segmento objetivo:

**Especialista de Aseguramiento y Control de Calidad (QA/QC)**

- Consultar el historial y los estados de un lote farmacéutico.
- Localizar un procedimiento operativo estándar (SOP) y revisar su versión vigente.
- Consultar el registro de una desviación y revisar el seguimiento de las acciones CAPA.
- Revisar las evidencias y registros asociados a la trazabilidad de un lote.

**Jefe o Supervisor de Producción Farmacéutica**

- Consultar las órdenes de producción y la información de las fórmulas maestras.
- Revisar el estado y el historial de un lote de producción.
- Consultar la información relacionada con la recepción y asignación de materias primas.
- Revisar los datos de monitoreo IoT y las incidencias asociadas a los equipos y procesos productivos.

Durante estas actividades se observará la facilidad con la que los participantes localizan la información, comprenden las opciones disponibles y completan las tareas propuestas. En la exploración libre también se registrarán las funcionalidades que generan mayor interés, las dificultades encontradas y las acciones que los usuarios realizan de manera espontánea.

### 5.3.2. Registro de Entrevistas

### 5.3.3. Evaluaciones según heurísticas

## 5.4. Video About-the-Product
(![Video About-the-Product](../assets/img/chapter5/screenshots/about-the-product.png))
Enlace del video en Microsoft Stream: https://upcedupe-my.sharepoint.com/:v:/g/personal/u20231h117_upc_edu_pe/IQD67i89t5nPQbnN8YPxIrulAX0VVWMwZsYa9bOdPo2Yu3o?e=3U79az
Enlace del video en Youtube: https://youtu.be/m5nkEdCl6PY

# Conclusiones

## Conclusiones y recomendaciones

### Conclusiones

1. **Sobre los Problem Statements:** En relación a los *Problem Statements* especificados, se concluye que la falta de digitalización y la trazabilidad manual generan un riesgo regulatorio crítico y pérdidas invisibles en los laboratorios farmacéuticos. DoofPlus aborda directamente este problema centralizando la documentación de calidad y los datos de producción en una única plataforma alineada a las normativas BPM/GMP.
2. **Assumptions vs. Comportamiento Real:** Durante el proceso, el equipo estableció como *Assumption* que los especialistas de QA/QC y Supervisores de Producción tendrían una alta resistencia al cambio tecnológico. Sin embargo, en contraste con el comportamiento real observado durante las entrevistas y validaciones, los segmentos mostraron una alta disposición a adoptar la herramienta, siempre y cuando la experiencia de usuario (UX) reduzca los clics necesarios para liberar un lote.
3. **Hypotheses Statements y Criterios de Éxito (Lean UX):** Respecto a nuestros *Hypotheses Statements*, se confirmó la hipótesis de que implementar firmas electrónicas y un *Audit Trail* automatizado reduce drásticamente el tiempo de revisión de lotes. Los resultados obtenidos de las validaciones de nuestros prototipos y *wireframes* superaron los criterios de éxito especificados en el proceso de Lean UX (logrando una aceptación del flujo de trabajo superior a la métrica esperada).

### Recomendaciones

1. **Roadmap - Desarrollo Backend y Seguridad:** En relación al *Roadmap* de los productos digitales, se recomienda priorizar para el siguiente sprint o hito (AV2) la construcción de la arquitectura Backend y la base de datos relacional. Es imperativo consolidar el *Bounded Context* de IAM (Identity and Access Management) antes de avanzar, ya que el valor del sistema depende de las firmas electrónicas y la seguridad de los roles.
2. **Roadmap - Integración de Telemetría (IoT):** Como siguiente paso en el alcance del modelo de negocio digital, se sugiere que el equipo investigue e integre simuladores de *WebSockets* o APIs en tiempo real. Esto permitirá validar técnicamente el módulo de monitoreo IoT (temperatura y humedad) establecido en el *Roadmap* sin necesidad de depender de hardware físico inmediato.

## Video About-The-Team

### Resumen del video

El video About-The-Team presenta una síntesis del proceso de trabajo desarrollado por el equipo a lo largo del proyecto. Combina imágenes y video de sesiones reales de trabajo con una narración en voz en off que guía al espectador por las etapas principales: la organización del equipo, la distribución de roles, el desarrollo de las actividades y los resultados obtenidos. De este modo muestra la dinámica colaborativa y la manera en que se abordaron los desafíos del proyecto.

Además, el video incluye el testimonio ante cámara de cada participante, quien describe las actividades que realizó, el logro de los outcomes planteados y las competencias que desarrolló durante la experiencia. Estos testimonios aportan una mirada personal y complementan la visión global del trabajo en equipo, y muestran el aprendizaje alcanzado de forma individual y colectiva.

**Duración total:** [00:10:04]

### Cuadro de video representativo

[![About-The-Team](https://img.youtube.com/vi/pE0AvoDX4XY/maxresdefault.jpg)](https://www.youtube.com/watch?v=pE0AvoDX4XY)

*Cuadro representativo del video About-The-Team.*

### Pauta de secuencias de contenido

| N.º | Sección / Secuencia                        | Tiempo de inicio (hh:mm:ss) | Descripción breve                                                                                                                                                                                                      |
|:---:|--------------------------------------------|:---------------------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 | Introducción / Presentación del equipo     |          00:00:00           | Startup IngesCompanyCC, desarrolladora de Doofplus, empezamos escuchando a los usuarios, haciendo entrevistas, oyendo sus problemas, gracias a esto se pudo hacer el needfinding, para luego pasar a DDD               |
| 2 | Proceso de trabajo del equipo (voz en off) |          00:00:21           | Una explicación del trabajo realizado a lo largo del curso, se menciona lo desarrollado en el sprint 1, sprint 2 y el sprint 3. Además, de mencionar brevemente de lo que desarrollará en el sprint 4                  |
| 3 | Testimonio de Marcelo Angulo               |          00:05:13           | Trabajó en las entrevistas con los segmentos objetivos. Además, se encargó de la programación del bounded context Monitoring. <br> Para el outcome: trabajó comunicandose de forma clara tanto en oral como en escrito |
| 4 | Testimonio de Yhoshua Cobades              |          00:06:11           | Trabajó en los aspectos de Domain Driven Design (Lenguaje Ubicuo), se encargó de los bounded context Quality y Manufacturing. <br> Para el outcome: expuso en el diseño del dominio                                    |
| 5 | Testimonio de Daniel Rojas                    |          00:07:24           | Diseñó la arquitectura C4, diagramas c4, se encargó del bounded context Subscriptions. <br> Para el outcome: se encargó de explicar a sus compañeros sobre el código que implementaba.                                 |
| 6 | Testimonio de Cristina Yarleque            |          00:07:14           | Lideró las especializaciones de los requisitos, se encargó del bounded context Organizations. <br> Para el outcome: contribuyó las metas y tareas de cada sprint.                                                      |
| 7 | Testimonio de Rodolfo Zavaleta             |          00:09:11           | Presentó al equipo, trabajó en la arquitectura del código de la Web Application, se encargó del bounded context Manufacturing y Shared. <br> Para el outcome: expuso el proyecto a un público de negocio.              |

### Enlaces del video

| Plataforma | URL                                                                                                                                                                                                                                                                                                                      | Uso |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----|
| Microsoft Stream | https://upcedupe-my.sharepoint.com/:v:/g/personal/u20241f733_upc_edu_pe/IQBeNDq299hNSpRA02HN5jW1AbcEics4_MkcHq4V26kerIA?e=cSbY1Q&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D  | Versión publicada |
| YouTube | https://www.youtube.com/watch?v=pE0AvoDX4XY                                                                                                                                                                                                                                                                              | Versión incrustada en el Landing Page |


# Bibliografía

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Brown, S. (2018). <em>Software Architecture for Developers: Visualise, document and explore your software architecture</em>. Leanpub.
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Brown, T. (2009). <em>Change by design: How design thinking transforms organizations and inspires innovation</em>. HarperBusiness.
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
DrugXafe. (2025). <em>DrugXafe: Sistema de Seguimiento y Rastreo Farmacéutico</em>. tiga. https://www.tigahealth.com/es/productos/drugxafe-sistema-de-seguimiento-y-rastreo-farmaceutico/
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Evans, E. (2004). <em>Domain-driven design: Tackling complexity in the heart of software</em>. Addison-Wesley Professional.
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Gothelf, J., & Seiden, J. (2021). <em>Lean UX: Designing great products with agile teams</em> (3rd ed.). O'Reilly Media.
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
LoLimsa. (2026). <em>EMPRESA DE SOFTWARE MÉDICO. Expertos en tecnología para la salud</em>. https://www.lolimsa.com.pe
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Martin, R. C. (2017). <em>Clean architecture: A craftsman's guide to software structure and design</em>. Prentice Hall.
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
MasterControl. (2024). <em>Quality Management System (QMS) Software for Life Sciences</em>. https://www.mastercontrol.com/
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Organización Mundial de la Salud. (2014). Annex 2: WHO good manufacturing practices for pharmaceutical products: Main principles. En <em>WHO Expert Committee on Specifications for Pharmaceutical Preparations: Forty-eighth report</em> (WHO Technical Report Series, n.º 986). https://www.who.int/publications/m/item/trs986-annex2
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Osterwalder, A., & Pigneur, Y. (2010). <em>Business model generation: A handbook for visionaries, game changers, and challengers</em>. John Wiley & Sons.
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Patton, J. (2014). <em>User story mapping: Discover the whole story, build the right product</em>. O'Reilly Media.
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Ries, E. (2011). <em>The lean startup: How today's entrepreneurs use continuous innovation to create radically successful businesses</em>. Crown Business.
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Schwaber, K., & Sutherland, J. (2020). <em>The Scrum Guide</em>. Scrum.org.
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Tuesta, P. (2025, 9 de abril). <em>51 laboratorios extranjeros sin certificación de buenas prácticas aún venden medicinas en Perú</em>. Salud con Lupa. https://saludconlupa.com/noticias/51-laboratorios-extranjeros-sin-certificacion-de-buenas-practicas-aun-venden-medicinas-en-peru/
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
tuhub. (2025). <em>Más control en la operación. Menos pérdidas invisibles.</em>. https://tuhub.co
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Tulip Interfaces. (2024). <em>Frontline Operations Platform for Manufacturing</em>. https://tulip.co/
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Veeva Systems. (2024). <em>Veeva Vault Quality: Modernizing Quality Management</em>. https://www.veeva.com/products/vault-quality/
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Vue.js. (2024). <em>Vue.js: The Progressive JavaScript Framework</em>. https://vuejs.org/
</div>

<div style="padding-left: 40px; text-indent: -40px; margin-bottom: 15px;">
Vernon, V. (2013). <em>Implementing Domain-Driven Design</em>. Addison-Wesley Professional.
</div>
# Anexos

## Anexo A. Videos de Exposiciones

En este anexo se incluyen los hipervínculos a los videos de exposición de cada entrega:

* **Entrega TB1 (Sprints 1 y 2):** Video de exposición upc-pre-202620-1asi0730-8130-IngesCompanyCC-exposition-tb1, *pendiente de registrar el enlace de Microsoft Stream.*
