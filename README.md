# ICN292 - Laboratorio 3: Automatización y Triage de Devoluciones en n8n

* **Nombre:** Montserratt Rojas
* **RUT:** 21497666 (Semilla $S = 666$)
* **Fecha:** 20 de septiembre 2026

---

##  Descripción del Proyecto

Implementación de un sistema desacoplado en **n8n** para la recepción, evaluación y clasificación automatizada de solicitudes de devolución de productos en base a reglas de negocio parametrizadas ($D = 21$ días y umbral $U = \$46\,000$) y valorización en tiempo real en Unidades de Fomento (UF) mediante la API de `mindicador.cl`.

---

##  Archivos del Repositorio y Guía de Reproducción

A continuación se detalla cada archivo incluido y las instrucciones para abrirlo o reproducirlo:

### 1. `ICN292-Lab3-Rojas-Montserratt-triage.json` (Flujo Receptor)
* **Descripción:** Workflow en n8n que contiene el endpoint Webhook, la lógica de bifurcación condicional (`Switch`), la consulta a la API de UF (`mindicador.cl`) y la respuesta consolidada.
* **Cómo reproducirlo:**
  1. Ingresar a la instancia de n8n.
  2. En el menú superior de Workflows, hacer clic en los tres puntos (`...`) y seleccionar **Import from File**.
  3. Seleccionar este archivo `.json`.
  4. Publicar el workflow (**Publish**) para activar el Webhook de producción permanente.

### 2. `ICN292-Lab3-Rojas-Montserratt-emisor.json` (Flujo Emisor)
* **Descripción:** Workflow en n8n con disparador manual, bloque de código Python para estructurar las 15 solicitudes de prueba y nodo HTTP Request para su envío por lotes.
* **Cómo reproducirlo:**
  1. Importar el archivo `.json` en n8n siguiendo el mismo procedimiento anterior.
  2. Abrir el nodo **HTTP Request** y verificar que la URL apunte al Webhook de producción del flujo de triage.
  3. Hacer clic en **Execute workflow** para enviar los 15 registros y visualizar las respuestas en el panel `OUTPUT` en vista tabla o JSON.


### 3. `ICN292-Lab3-Rojas-Montserratt.pdf` (Informe Técnico)
* **Descripción:** Informe final que incluye resumen ejecutivo de una página, análisis de arquitectura, parámetros de la semilla personal, evidencia de ejecuciones fallidas/exitosas y tabla con los 15 resultados procesados.
* **Cómo abrirlo:** Visualizar con cualquier visor de archivos PDF estándar (Adobe Acrobat, navegador web o visor de sistema operativo).

---

##  Parámetros de Negocio Aplicados

* **Semilla ($S$):** `666`
* **Umbral de Monto ($U$):** \$46\,000 CLP 
* **Plazo Máximo ($D$):** $21$ días
