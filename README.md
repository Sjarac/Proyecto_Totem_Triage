# Tótem de Evaluación de Aptitud Física

Tótem de autoservicio para gimnasios que evalúa si una persona está en condiciones de entrenar. Combina un cuestionario de salud, mediciones biométricas y un motor de evaluación para entregar una recomendación de aptitud física. Solo el encargado de salud, un profesional de salud calificado, puede modificar ese resultado.

Proyecto de título de Ingeniería en Informática, Duoc UC (Capstone APT122).

* **Dashboard en línea:** [triage-dashboard-three.vercel.app](https://triage-dashboard-three.vercel.app/)

---

## 1. Descripción del Proyecto

### ¿Qué problema resuelve?
Hoy no existe ningún filtro de aptitud física antes de que un socio empiece a entrenar en el gimnasio. La decisión queda en manos del propio usuario, sin ningún dato objetivo, y eso implica riesgo de sobreexigencia o incluso de problemas de salud.

### ¿A quién va dirigido?
* **Usuario final:** usuario del gimnasio que se evalúa antes de entrenar.
* **Encargado del gimnasio:** ve el resultado en el dashboard y, ante una emergencia, deriva al usuario a atención médica. No puede modificar el resultado del análisis.
* **Encargado de salud:** profesional de salud calificado; es el único autorizado para revisar y modificar el resultado del análisis.

### ¿Qué hace la solución?
El usuario se identifica en el tótem, responde un cuestionario de salud, se toma sus signos vitales y su composición corporal, y hace una prueba de esfuerzo breve (step test) que permite estimar su VO2 máx a partir de la frecuencia cardíaca de recuperación. La prueba no se inicia si el cuestionario o las mediciones en reposo muestran una condición de riesgo. El motor de evaluación asigna una de cuatro categorías de aptitud (A1 Riesgo alto, A2 Precaución, A3 Apto moderado o A4 Apto) junto con su nivel de confianza. Si detecta una condición de riesgo, envía una alerta al encargado del gimnasio para que derive al usuario a atención médica. El sistema apoya el criterio profesional, nunca lo reemplaza: cualquier cambio en el resultado lo hace el encargado de salud.

### Estado actual
Prototipo funcional de punta a punta: la app del tótem, el cuestionario de salud, el backend con el motor de evaluación, la base de datos y el dashboard con inicio de sesión. El backend está desplegado en Render y el dashboard en Vercel.

* **Sensores:** la app ya lee por Bluetooth el oxímetro, la balanza de bioimpedancia y el tensiómetro (Beurer BM 54) a través de sus puentes; con el tensiómetro real, la app muestra los mismos valores que la pantalla del aparato. El termómetro todavía no se integra. En el tótem final, la lectura de los sensores pasará a una tablet.
* **Evaluación:** hoy la categoría la decide un motor de reglas clínicas (`reglas_aptitud.json`). El pipeline del modelo XGBoost está listo para entrenarse en cuanto exista un dataset etiquetado por un profesional; cuando se active, se combina con las reglas y el resultado final es siempre el más conservador de los dos.

---

## 2. Tecnologías Utilizadas

* **App del tótem:** React Native (Expo), ejecutada como web en modo quiosco en el equipo del tótem.
* **Hardware:** oxímetro de pulso, balanza con bioimpedancia y tensiómetro Beurer BM54 (Bluetooth LE). Cada sensor se conecta mediante un puente en Python (`bleak`) que entrega la medición a la app por HTTP local.
* **Backend:** Python (FastAPI), desplegado en Render.
* **Evaluación de aptitud:** motor de reglas clínicas y pipeline de Machine Learning con XGBoost.
* **Base de datos:** PostgreSQL (9 tablas).
* **Dashboard (encargado del gimnasio y encargado de salud):** Next.js / React.js, desplegado en Vercel.
* **Pruebas:** pytest (backend y puentes) y Jest (app y dashboard).
* **Control de versiones:** Git y GitHub.

---

## 3. Arquitectura de la Solución

```
[Sensores Bluetooth]            [Equipo del tótem]                                [Cloud]
 Oxímetro, balanza,   ──BLE──>   Puentes Python   ──HTTP──>  App del tótem  ──HTTPS──>  FastAPI (Render)
 tensiómetro                     (localhost)                 (Expo, quiosco)              │
                                                                                          ├──> Motor de reglas / XGBoost
                                                                                          ▼
                                                                                      PostgreSQL
                                                                                          │
                                                                                          ▼
                                                                                    [Dashboard]
                                                                               Next.js / React (Vercel)
```

El detalle está en el [diagrama de componentes](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/4.%20Vista%20de%20desarrollo/Diagrama%20de%20componentes.png) y en el [diagrama de despliegue](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/5.%20Vista%20f%C3%ADsica/Diagrama_de_despliegue.png).

---

## 4. Metodología de Trabajo

El proyecto se gestiona con **Scrum**. Al cierre de cada sprint se hace una retrospectiva, y cada tarea se da por terminada solo cuando cumple la Definición de Terminado (DoD) del equipo. El código y la documentación se versionan en GitHub.

---

## 5. Integrantes y Roles

| Nombre | Rol | Responsabilidades principales |
| :--- | :--- | :--- |
| **Cristóbal Hernández Orellana** | Scrum Master | Liderazgo del proyecto y desarrollo. |
| **Thomas Gutiérrez Suárez** | Product Owner | Comunicación con los clientes y desarrollo. |
| **Sebastián Jara Correa** | Equipo de Scrum | Desarrollo y gestión. |

---

## 6. Documentación del Proyecto

### Fase 1
* [Evidencias grupales](Fase%201/Evidencias%20Grupales): formativa (guía 1.4), definición del proyecto (guía 1.5), diagramas de casos de uso y de componentes, y planilla de evaluación.
* [Evidencias individuales](Fase%201/Evidencias%20Individuales): autoevaluación de competencias (1.1), diario de reflexión (1.2) y autoevaluación (1.3) de cada integrante.

### Fase 2
* [Evidencias grupales](Fase%202/Evidencias%20Grupales): informe de avance (guía 2.4), informe final (guía 2.6) y planillas de evaluación.
* [Evidencias individuales](Fase%202/Evidencias%20Individuales): diario de reflexión de cada integrante (guía 2.1).
* [Presentación del proyecto](Fase%202/Evidencias%20Proyecto/Presentaci%C3%B3n%20Proyecto.pptx).

### Evidencias de sistema
* [Código fuente](Fase%202/Evidencias%20Proyecto/TestB-Totem): app del tótem, backend, dashboard, puentes de los sensores y quiosco (ver sección 7).
* [Capturas del programa](Fase%202/Evidencias%20Proyecto/Capturas%20programa).
* [Base de datos](Fase%202/Evidencias%20Proyecto/Base%20de%20datos): [diagrama entidad-relación](Fase%202/Evidencias%20Proyecto/Base%20de%20datos/Diagrama_ER_Totem_Gimnasio.png) de las 9 tablas de PostgreSQL.
* Enlace de despliegue: [Vercel](Fase%202/Evidencias%20Proyecto/Vercel.txt) (dashboard).

Toda la documentación técnica de la Fase 2 está en [`Fase 2/Evidencias Proyecto/Evidencias de documentación`](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n).

### Requisitos funcionales
En el [Excel de requisitos](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Requisitos/Requisitos_Capstone_Thomas_Guti%C3%A9rrez_Sebasti%C3%A1n_Jara_Cristobal_Hern%C3%A1ndez.xlsx), hojas *Requisitos gestión usuario* a *Integración e interoperabilidad* (R.1 a R.170). Los requisitos de la prueba de esfuerzo (R.251 a R.255) están al final de la hoja *Mediciones biomédicas*.

### Requisitos no funcionales
En el mismo [Excel de requisitos](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Requisitos/Requisitos_Capstone_Thomas_Guti%C3%A9rrez_Sebasti%C3%A1n_Jara_Cristobal_Hern%C3%A1ndez.xlsx), en las 5 últimas hojas: Seguridad y privacidad, Rendimiento, Usabilidad y accesibilidad, Calidad y Mantenimiento (R.171 a R.250). Cada requisito indica su estado actual: Completado, En desarrollo, Diferido o Pendiente.

### Diagramas de proceso (BPMN)
* [BPMN as-is](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/BPMN/BPMN_As_is.bpmn): el prototipo actual, con mediciones simuladas.
* [BPMN to-be](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/BPMN/BPMN_To_be.svg): el tótem con sensores reales ([archivo `.bpmn`](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/BPMN/BPMN_To_be.bpmn)).

Los archivos `.bpmn` se pueden abrir y editar en [demo.bpmn.io](https://demo.bpmn.io).

### Arquitectura 4+1
* [Vista de escenarios](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/1.%20Vista%20de%20escenarios): casos de uso del tótem y del dashboard.
* [Vista lógica](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/2.%20Vista%20l%C3%B3gica): diagrama de clases y diagrama de estados.
* [Vista de procesos](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/3.%20Vista%20de%20procesos): diagramas de secuencia y de comunicación (los BPMN están en la sección anterior).
* [Vista de desarrollo](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/4.%20Vista%20de%20desarrollo): diagrama de componentes y diagrama de paquetes.
* [Vista física](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Arquitectura%204%2B1/5.%20Vista%20f%C3%ADsica): diagrama de despliegue.

### Justificación de la innovación
* [Documento de justificación](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Justificaci%C3%B3n%20de%20la%20innovaci%C3%B3n/Justificacion_innovacion_Capstone_003.docx): comparación con soluciones existentes y elementos innovadores del proyecto.

### Metodología ágil
* [Product Backlog](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Backlogs/Product_Backlog_Totem_Gimnasio.xlsx) y [Sprint Backlog](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Backlogs/Sprint_Backlog_Totem_Gimnasio.xlsx)
* [Visión del Producto](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Visi%C3%B3n%20y%20DoD/Vision_del_Producto.png)
* [Definición de Terminado (DoD)](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Visi%C3%B3n%20y%20DoD/Definicion_de_Terminado.png)
* Retrospectivas: [Sprint 1](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Retrospectivas/Retrospectiva_Sprint_1.png) · [Sprint 2](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Retrospectivas/Retrospectiva_Sprint_2.png) · [Sprint 3](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Retrospectivas/Retrospectiva_Sprint_3.png) · [Sprint 4](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Retrospectivas/Retrospectiva_Sprint_4.png) · [Sprint 5](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Retrospectivas/Retrospectiva_Sprint_5.png) · [Sprint 6](Fase%202/Evidencias%20Proyecto/Evidencias%20de%20documentaci%C3%B3n/Retrospectivas/Retrospectiva_Sprint_6.png)

---

## 7. Instrucciones de Ejecución Local

El código está en [`Fase 2/Evidencias Proyecto/TestB-Totem`](Fase%202/Evidencias%20Proyecto/TestB-Totem):

| Carpeta | Qué contiene |
| :--- | :--- |
| `backend-totem` | API en FastAPI, motor de reglas y pipeline de IA (`ia/`). |
| `triage-dashboard` | Dashboard en Next.js para el encargado del gimnasio y el encargado de salud. |
| `totem-app` | App del tótem en Expo / React Native. |
| `puente-oximetro`, `puente-bascula`, `puente-tensiometro` | Servicios en Python que leen cada sensor por Bluetooth y entregan la medición a la app. |
| `quiosco` | Scripts de Windows que arrancan los puentes, la app y el navegador a pantalla completa en el tótem. |

### Backend
```bash
cd backend-totem
pip install -r requirements.txt
cp .env.example .env        # completar DATABASE_URL y el resto de variables
uvicorn main:app --reload
```

### Dashboard
```bash
cd triage-dashboard
npm install
# definir NEXT_PUBLIC_BACKEND_URL con la URL del backend (por ejemplo, http://localhost:8000)
npm run dev                 # http://localhost:3000
```

### App del tótem
```bash
cd totem-app
npm install
cp .env.example .env        # EXPO_PUBLIC_API_URL y EXPO_PUBLIC_SENSORES ("simulado" para probar sin hardware)
npx expo start -c
```

Configurar el equipo del tótem (Windows)

El PC que se conecta a los sensores por Bluetooth se configura con un solo comando. Se ejecuta una vez en cada equipo nuevo, con Python, Node.js y el Bluetooth encendido:

### powershell
```bash
git clone https://github.com/Sjarac/Proyecto_Totem_Triage.git
cd "Proyecto_Totem_Triage\Fase 2\Evidencias Proyecto\TestB-Totem\quiosco"
.\configurar_equipo.ps1
```
El script hace todo esto:

Comprueba los requisitos.
Instala las dependencias de los puentes y de la app.
Crea el puente.env de cada sensor con las direcciones de dispositivos.json.
Fija la IP del equipo y la clave de los puentes.
Abre el firewall. Para esto pide permisos de administrador una vez.
Compila la app.
Arranca los puentes y la app, y comprueba que respondan.

Al final muestra la dirección que hay que abrir en la tablet.

Opción	Para qué sirve
-SoloVerificar	Muestra qué haría y qué falta, sin cambiar nada.
-IP 192.168.1.50	Fija la IP si la detectada no es la de la red de la tablet.
-SinDependencias, -SinFirewall, -SinCompilar, -SinIniciar	Omiten ese paso.

Si PowerShell bloquea los scripts, ejecuta antes Set-ExecutionPolicy -Scope CurrentUser RemoteSigned. El emparejamiento Bluetooth del tensiómetro es propio de cada PC, así que hay que repetirlo en cada equipo nuevo. Para detener todo, usa .\detener_quiosco.ps1.

Los detalles para conectar los sensores reales y montar el tótem en modo quiosco están en el README de cada puente y en el de la carpeta quiosco. Las pruebas se ejecutan con pytest en el backend y los puentes, y con npm test en la app y el dashboard.
