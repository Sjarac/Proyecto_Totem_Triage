# Tótem de Evaluación de Aptitud Física

Tótem de autoservicio para gimnasios que evalúa si una persona está en condiciones de entrenar. Combina un cuestionario de salud, mediciones biométricas y un modelo de machine learning para entregar una recomendación de aptitud física. Solo el encargado de salud, un profesional de salud calificado, puede modificar ese resultado.

Proyecto de título de Ingeniería en Informática, Duoc UC.

---

## 1. Descripción del Proyecto

### ¿Qué problema resuelve?
Hoy no existe ningún filtro de aptitud física antes de que un socio empiece a entrenar en el gimnasio. La decisión queda en manos del propio usuario, sin ningún dato objetivo, y eso implica riesgo de sobreexigencia o incluso de problemas de salud.

### ¿A quién va dirigido?
* **Usuario final:** usuario del gimnasio que se evalúa antes de entrenar.
* **Encargado del gimnasio:** ve el resultado en el dashboard y, ante una emergencia, deriva al usuario a atención médica. No puede modificar el resultado del análisis.
* **Encargado de salud:** profesional de salud calificado; es el único autorizado para revisar y modificar el resultado del análisis.

### ¿Qué hace la solución?
El usuario responde un cuestionario de salud en el tótem, se toma sus signos vitales y su composición corporal, y hace una prueba de esfuerzo breve (step test) que permite estimar su VO2 máx a partir de la frecuencia cardíaca de recuperación. La prueba no se inicia si el cuestionario o las mediciones en reposo muestran una condición de riesgo. Un modelo de machine learning asigna una de cuatro categorías de aptitud (A1 Riesgo alto, A2 Precaución, A3 Apto moderado o A4 Apto) junto con su nivel de confianza. Si detecta una condición de riesgo, envía una alerta al encargado del gimnasio para que derive al usuario a atención médica. El sistema apoya el criterio profesional, nunca lo reemplaza: cualquier cambio en el resultado lo hace el encargado de salud.

### Estado actual
Prototipo funcional de punta a punta: la app del tótem, el cuestionario de salud, el backend con el motor de evaluación, la base de datos y el dashboard con inicio de sesión. La app ya lee por Bluetooth el oxímetro y la balanza de bioimpedancia; falta integrar el tensiómetro, cuya medición por ahora se simula.

---

## 2. Tecnologías Utilizadas

* **App del tótem:** React Native (Expo) sobre una tablet Android.
* **Hardware (en integración):** ESP32, termómetro Beurer FT95, oxímetro de pulso, tensiómetro Beurer BM54, balanza con bioimpedancia y pantalla LED de estado, conectados por Bluetooth.
* **Backend:** Python (FastAPI).
* **Machine Learning:** XGBoost, con Vertex AI.
* **Base de datos:** PostgreSQL.
* **Dashboard (encargado del gimnasio y encargado de salud):** Next.js / React.js.
* **Control de versiones:** Git y GitHub.

---

## 3. Arquitectura de la Solución

```
[Sensores Bluetooth]                 [App del tótem]                 [Backend y Cloud]
 Termómetro, oxímetro,   ──BLE──>     React Native      ──HTTPS──>    FastAPI ──> XGBoost (Vertex AI)
 tensiómetro, balanza                 (tablet + ESP32)                   │
                                                                         ▼
                                                                     PostgreSQL
                                                                         │
                                                                         ▼
                                                                 [Dashboard]
                                                          Next.js / React.js
```

El detalle está en el [diagrama de componentes](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/4.%20Vista%20de%20desarrollo/Diagrama%20de%20componentes.png).

---

## 4. Metodología de Trabajo

El proyecto se gestiona con **Scrum**. Al cierre de cada sprint se hace una retrospectiva, y cada tarea se da por terminada solo cuando cumple la Definición de Terminado (DoD) del equipo. El código y la documentación se versionan en GitHub.

---

## 5. Integrantes y Roles

| Nombre | Rol | Responsabilidades principales |
| :--- | :--- | :--- |
| **Cristóbal Hernández Orellana** | Product Owner / Scrum Master | Liderazgo del proyecto. |
| **Thomas Gutiérrez Suárez** | Desarrollo (Hardware, Backend y Frontend) | Creación de la aplicación y del tótem. |
| **Sebastián Jara Correa** | Ciencia de datos y QA | Creación y entrenamiento del modelo de ML. |

---

## 6. Documentación del Proyecto

### Informes y diarios de la Fase 2
* [Evidencias grupales](Fase%202/Evidencias%20Grupales): informe de avance (guía 2.4) e informe final (guía 2.6).
* [Evidencias individuales](Fase%202/Evidencias%20Individuales): diario de reflexión de cada integrante (guía 2.1).

### Evidencias de sistema
* [Aplicación](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20sistema/Aplicaci%C3%B3n): enlace al repositorio del código (app del tótem, backend y dashboard).
* [Base de datos](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20sistema/Base%20de%20datos): diagrama entidad-relación de las 9 tablas de PostgreSQL.

Toda la documentación técnica de la Fase 2 está en [`Fase 2/Evidencias Proyecto/Evidencias de documentación`](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n).

### Requisitos funcionales
En el [Excel de requisitos](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Requisitos), hojas *Requisitos gestión usuario* a *Integración e interoperabilidad* (R.1 a R.170). Los requisitos de la prueba de esfuerzo (R.251 a R.255) están al final de la hoja *Mediciones biomédicas*.

### Requisitos no funcionales
En el mismo [Excel de requisitos](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Requisitos), en las 5 últimas hojas: Seguridad y privacidad, Rendimiento, Usabilidad y accesibilidad, Calidad y Mantenimiento (R.171 a R.250). Cada requisito indica su estado actual: Completado, En desarrollo, Diferido o Pendiente.

### Diagramas de proceso (BPMN)
* [BPMN as-is](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/BPMN/BPMN_As_is.svg): el prototipo actual, con mediciones simuladas.
* [BPMN to-be](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/BPMN/BPMN_To_be.svg): el tótem con sensores reales.

Los archivos `.bpmn` se pueden abrir y editar en [demo.bpmn.io](https://demo.bpmn.io).

### Arquitectura 4+1
* [Vista de escenarios](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/1.%20Vista%20de%20escenarios): casos de uso del tótem y del dashboard.
* [Vista lógica](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/2.%20Vista%20l%C3%B3gica): diagrama de clases y diagrama de estados.
* [Vista de procesos](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/3.%20Vista%20de%20procesos): diagramas de actividad, de secuencia y de comunicación (los BPMN están en la sección anterior).
* [Vista de desarrollo](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/4.%20Vista%20de%20desarrollo): diagrama de componentes y diagrama de paquetes.
* [Vista física](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/5.%20Vista%20f%C3%ADsica): diagrama de despliegue.

### Justificación de la innovación
* [Documento de justificación](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Justificaci%C3%B3n%20de%20la%20innovaci%C3%B3n/Justificacion_innovacion_Capstone_003.docx): comparación con soluciones existentes y elementos innovadores del proyecto.

### Metodología ágil
* [Product Backlog](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Product_Backlog_Totem_Gimnasio.xlsx) y [Sprint Backlog](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Sprint_Backlog_Totem_Gimnasio.xlsx)
* [Visión del Producto](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Vision_del_Producto.png)
* [Definición de Terminado (DoD)](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Definicion_de_Terminado.png)
* Retrospectivas: [Sprint 1](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Retrospectiva_Sprint_1.png) · [Sprint 2](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Retrospectiva_Sprint_2.png) · [Sprint 3](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Retrospectiva_Sprint_3.png) · [Sprint 4](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Retrospectiva_Sprint_4.png) · [Sprint 5](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Retrospectiva_Sprint_5.png)

---

## 7. Instrucciones de Ejecución Local

El código está en el repositorio [dev-happy-sky/TestB-Totem](https://github.com/dev-happy-sky/TestB-Totem). Ahí están la app del tótem (`totem-app`), el backend (`backend-totem`) y el dashboard (`triage-dashboard`); los pasos para levantar la app y el dashboard están en el README de sus carpetas.
