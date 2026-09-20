# ICN292 - Laboratorio 3: Automatización y Triage de Devoluciones en n8n

Este repositorio contiene la implementación y los artefactos técnicos del **Laboratorio 3** para el curso ICN292 (Sistemas de Información / Automatización de Procesos). El proyecto implementa un sistema desacoplado de recepción, clasificación y evaluación automatizada de solicitudes de devolución de productos utilizando la plataforma de orquestación **n8n**.

---

##  Descripción del Proyecto

El sistema evalúa solicitudes transaccionales contra reglas de negocio parametrizadas ($D=21$ días de plazo máximo y umbral económico $U=\$46\,000$), enriqueciendo cada transacción mediante la consulta en tiempo real al indicador económico oficial de la Unidad de Fomento (UF) a través del servicio REST de `mindicador.cl`.

### Arquitectura de la Solución
El sistema se compone de flujos independientes:
1. **Flujo Receptor (`triage`):**
   * **Webhook:** Expuesto en entorno de producción mediante método `POST`.
   * **Switch:** Clasificación condicional en 4 rutas: `APROBACION`, `REVISION`, `RECHAZO` y `DATOS_INVALIDOS`.
   * **Consulta API UF:** Consumo dinámico de `https://mindicador.cl/api` vía HTTP `GET`.
   * **Registro y Notificación (JavaScript):** Consolidación programática tolerante a bifurcaciones no ejecutadas y cálculo de montos en UF.
   * **Respond to Webhook:** Emisión síncrona de la respuesta en formato JSON.
2. **Flujo Emisor (`emisor`):**
   * **Trigger manual:** Disparo bajo demanda.
   * **Code in Python:** Generación estructurada de 15 solicitudes de devolución para pruebas de carga y casos de borde.
   * **HTTP Request:** Despacho con serialización JSON (`{{ JSON.stringify($json) }}`) hacia el endpoint productivo.
3. **Flujo Resumen (`resumen`):**
   * Consolidación y procesamiento de métricas agregadas del lote transaccional.

---

##  Parámetros de Negocio Aplicados

En base a la semilla personal $S = 666$:
* **Semilla ($S$):** `666`
* **Umbral de Monto ($U$):** $\$46\,000$ CLP ($30000 + 1000 \times (666 \pmod{50})$)
* **Plazo Máximo ($D$):** $21$ días ($7 + 7 \times (S \dots)$)

---

##  Estructura de Archivos

```text
├── ICN292-Lab3-Apellido-Nombre-triage.json     # Workflow exportado de Triage (Receptor)
├── ICN292-Lab3-Apellido-Nombre-emisor.json     # Workflow exportado de Emisor de pruebas
├── ICN292-Lab3-Apellido-Nombre-resumen.json    # Workflow exportado de Resumen/Consolidación
├── ICN292-Lab3-Apellido-Nombre.pdf             # Informe técnico final con Resumen Ejecutivo
├── screenshots/                                # Evidencias de ejecuciones (exitosas y fallidas)
└── README.md                                   # Documentación del repositorio
