# Tótem Inteligente de Triage Automatizado con IA

Sistema ciberfísico de autoatención clínica y telemetría médica preliminar diseñado para optimizar el flujo de admisión y categorización en servicios de urgencia hospitalaria mediante IoT, Machine Learning y arquitecturas web modernas.

---

## 1. Descripción del Proyecto

### ¿Qué problema resuelve?
En las unidades de urgencia hospitalaria, el proceso tradicional de admisión presenta cuellos de botella derivados de la toma manual y transcripción analógica de signos vitales. Esta latencia retrasa la detección temprana del deterioro hemodinámico en salas de espera y sobrecarga operativa al personal clínico.

### ¿A quién va dirigido?
* **Usuarios finales:** Pacientes que ingresan a los servicios de urgencia hospitalaria.
* **Usuarios clínicos:** Personal de enfermería y médicos encargados de la supervisión, validación y asignación final de prioridades de atención (box de triage).

### ¿Qué hace la solución?
Permite al paciente registrar de forma autónoma sus signos vitales mediante sensores biomédicos conectados directamente a una tablet/kiosko interactivo. La información fisiológica se consolida junto con una anamnesis estructurada de antecedentes mórbidos y es evaluada por un modelo de Machine Learning que genera una recomendación objetiva del nivel de urgencia (basado en protocolos estandarizados como ESI / Manchester), disponibilizando los resultados en tiempo real para el equipo de salud.

---

## 2. Tecnologías Utilizadas

* **Frontend & Ingesta IoT:**
  * Framework: React / Next.js (Progressive Web App - PWA)
  * Hardware APIs: Web Bluetooth API (`navigator.bluetooth`), Web Serial API (`navigator.serial`)
  * Lenguaje: TypeScript / JavaScript
* **Backend & Servicios:**
  * Framework: Python (FastAPI)
  * Validación y esquemas: Pydantic / JSON Schema
* **Machine Learning & Procesamiento:**
  * Librerías: Scikit-Learn, Pandas, NumPy
  * Modelo: Clasificación multiclase supervisada para estratificación de riesgo clínico
* **Persistencia de Datos:**
  * Base de datos: PostgreSQL / SQL Server
* **Contenedores & Despliegue:**
  * Docker & Docker Compose

---

## 3. Arquitectura de la Solución

El sistema desacopla la adquisición de señales fisiológicas de bajo nivel en el cliente web, comunicándose de manera asíncrona con el backend para la inferencia algorítmica:


```

[Sensores Biomédicos]
├── Oxímetro de Pulso (BLE GATT 0x1822)     ──> [Web Bluetooth API]
├── Báscula Bioimpedancia (BLE GATT genérico)──> [Web Bluetooth API] ──> [Tablet / PWA Client]
├── Termómetro Infrarrojo (UART Serial)       ──> [Web Serial API]              │
└── Tensiómetro Oscilométrico                 ──> [Formulario Asistido]         │ JSON / HTTPS
▼
[Backend API REST (FastAPI)]
│
┌─────────────────────────┴─────────────────────────┐
▼                                                   ▼
[Motor de Inferencia ML]                            [Base de Datos Relacional]
(Estratificación de Urgencia)                              (Historial y Logs)

```

---

## 4. Metodología de Trabajo

El proyecto se gestiona bajo el marco de trabajo **Scrum**, estructurado en sprints quincenales orientados a la entrega incremental de valor:
* **Planificación & Backlog:** Gestión de épicas e historias de usuario en GitHub Projects.
* **Control de versiones:** Flujo Git estructurado con ramas por funcionalidad (`feature/*`), pruebas (`develop`) y versiones estables (`main`).
* **Calidad y Revisiones:** Revisiones de código mediante Pull Requests, pruebas de integración por sprint y retrospectivas periódicas.

---

## 5. Integrantes y Roles

| Nombre | Rol | Responsabilidades Principales |
| :--- | :--- | :--- |
| **Cristóbal Hernández** | Scrum Master / Tech Lead | Arquitectura del sistema, integración IoT (Web Bluetooth/Serial) y pipeline de inferencia ML. |
| **[Nombre Integrante 2]** | Full Stack Developer | Desarrollo de interfaz táctil PWA, formularios de anamnesis y consumo de APIs. |
| **[Nombre Integrante 3]** | Data & Backend Engineer | Modelado de datos relacional, endpoints en FastAPI y contenedorización Docker. |

---

## 6. Instrucciones de Ejecución Local

### Prerrequisitos
* [Docker Desktop](https://www.docker.com/) instalado y en ejecución.
* [Node.js](https://nodejs.org/) v18+ y [Python](https://www.python.org/) 3.10+ (para depuración local sin contenedor).
* Navegador basado en Chromium (Google Chrome / Edge) con soporte de Web Bluetooth y Web Serial habilitado.

### Despliegue con Docker Compose

1. **Clonar el repositorio:**
   ```bash
   git clone [https://github.com/tu-usuario/nombre-del-proyecto.git](https://github.com/tu-usuario/nombre-del-proyecto.git)
   cd nombre-del-proyecto

```

2. **Configurar variables de entorno:**
Crear un archivo `.env` en la raíz del proyecto basándose en la plantilla:
```bash
cp .env.example .env

```


3. **Construir y levantar los contenedores:**
```bash
docker-compose up --build -d

```


4. **Acceso a los servicios:**
* **Frontend (Tótem PWA):** `http://localhost:3000`
* **Backend API (Swagger Docs):** `http://localhost:8000/docs`
* **Base de Datos:** `localhost:5432`



```

```
