# Tótem de Evaluación de Aptitud Física con IA

Tótem de autoservicio para gimnasios que evalúa si una persona está en condiciones de entrenar. Combina un cuestionario de salud, mediciones biométricas y un modelo de machine learning para entregar una recomendación de aptitud física. Solo un profesional de salud calificado puede modificar ese resultado.

Proyecto de título de Ingeniería en Informática, Duoc UC.

---

## 1. Descripción del Proyecto

### ¿Qué problema resuelve?
Hoy no existe ningún filtro de aptitud física antes de que un socio empiece a entrenar en el gimnasio. La decisión queda en manos del propio usuario, sin ningún dato objetivo, y eso implica riesgo de sobreexigencia o incluso de problemas de salud.

### ¿A quién va dirigido?
* **Usuario final:** usuario del gimnasio que se evalúa antes de entrenar.
* **Encargado del gimnasio:** ve el resultado en el dashboard y, ante una emergencia, deriva al usuario a atención médica. No puede modificar el resultado del análisis.
* **Profesional de salud calificado:** único autorizado para revisar y modificar el resultado del análisis.

### ¿Qué hace la solución?
El usuario responde un cuestionario de salud en el tótem y se toma sus signos vitales y su composición corporal. Un modelo de machine learning genera una categoría de aptitud (por ejemplo: apto, apto con precaución o requiere evaluación médica) junto con su nivel de confianza. Si detecta una condición de riesgo, envía una alerta al encargado para que derive al usuario a atención médica. El sistema apoya el criterio profesional, nunca lo reemplaza: cualquier cambio en el resultado lo hace un profesional de salud calificado.

### Estado actual
Prototipo funcional de punta a punta: la app del tótem (con mediciones simuladas), el cuestionario de salud, el backend con el modelo de IA, la base de datos y el dashboard con inicio de sesión. La integración con los sensores reales está pendiente de la compra del hardware.

---

## 2. Tecnologías Utilizadas

* **App del tótem:** React Native (Expo) sobre una tablet Android.
* **Hardware (en integración):** ESP32, termómetro Beurer FT95, oxímetro de pulso, tensiómetro Beurer BM54, balanza con bioimpedancia y pantalla LED de estado, conectados por Bluetooth.
* **Backend:** Python (FastAPI).
* **Machine Learning:** XGBoost, con Vertex AI.
* **Base de datos:** PostgreSQL.
* **Dashboard (encargado y profesional de salud):** Next.js / React.js.
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

El detalle está en el [diagrama de componentes](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Diagramas%20UML/Diagrama%20de%20componentes.png).

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

Toda la documentación de la Fase 2 está en [`Fase 2/Evidencias Proyecto/Evidencias de documentación`](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n).

### Requisitos funcionales
En el [Excel de requisitos](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Requisitos), hojas *Requisitos gestión usuario* a *Integración e interoperabilidad* (R.1 a R.170).

### Requisitos no funcionales
En el mismo [Excel de requisitos](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Requisitos), en las 5 últimas hojas: Seguridad y privacidad, Rendimiento, Usabilidad y accesibilidad, Calidad y Mantenimiento (R.171 a R.250). Cada requisito indica su estado actual: Completado, En desarrollo, Diferido o Pendiente.

### Diagramas de proceso (BPMN)
* [BPMN as-is](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/BPMN/BPMN_As_is.svg): el prototipo actual, con mediciones simuladas.
* [BPMN to-be](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/BPMN/BPMN_To_be.svg): el tótem con sensores reales.

Los archivos `.bpmn` se pueden abrir y editar en [demo.bpmn.io](https://demo.bpmn.io).

### Diagramas UML
* [Diagrama de componentes](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Diagramas%20UML/Diagrama%20de%20componentes.png)
* [Diagramas de casos de uso](Fase%201/Evidencias%20Grupales/Diagramas%20de%20caso%20de%20uso.pdf) (Fase 1)

### Metodología ágil
* [Visión del Producto](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Vision_del_Producto.png)
* [Definición de Terminado (DoD)](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Definicion_de_Terminado.png)
* [Retrospectiva del Sprint 1](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Metodolog%C3%ADa%20%C3%A1gil/Retrospectiva_Sprint_1.png)

---

## 7. Instrucciones de Ejecución Local

*TODO. Se completará con los pasos para levantar la app del tótem, el backend y el dashboard.*
