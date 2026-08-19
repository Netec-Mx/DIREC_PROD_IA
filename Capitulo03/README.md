# Práctica 3. Diseñar un plan de gobernanza y gestión para una iniciativa de IA a gran escala, estructurando estrategias de comunicación para mitigar las falsas expectativas de los stakeholders, definiendo controles ágiles contra el scope creep y los bloqueadores de desarrollo, planificando la estructura del equipo y la selección de proveedores, y construyendo un roadmap ágil que mitigue los riesgos presupuestarios.

## Metadatos

| Elemento | Valor |
|---|---|
| Duración | 120 minutos |
| Complejidad | Difícil |
| Nivel de Bloom | Crear |

## Descripción general

En esta práctica se transformará el paquete de descubrimiento, planificación y experimentación de las Prácticas 1 y 2 en un dossier de gobernanza para `SoporteB2B_GenAI_MVP`. El dossier definirá quién toma decisiones, qué evidencia debe existir, con qué cadencia se revisa el producto y bajo qué criterios se autoriza el paso de experimento a MVP, de MVP a piloto y de piloto a escalamiento.

El resultado incluirá un mapa de stakeholders, un contrato de expectativas, controles ágiles de alcance, gestión de bloqueadores, estructura de equipo, matriz RACI, criterios de proveedores, roadmap financiable y mecanismos de auditoría. Todas las decisiones deberán mantener trazabilidad con el problema de negocio, las restricciones de datos, los requisitos verificables y el plan de experimentación previos.

## Objetivos de aprendizaje

Al finalizar la práctica, podrá:

- [ ] Diseñar un sistema de gobernanza de IA con roles de decisión, evidencias, cadencias y criterios de paso entre fases.
- [ ] Elaborar un mapa de stakeholders y un plan de comunicación que distinga capacidades demostradas, hipótesis, límites y riesgos.
- [ ] Establecer controles ágiles de alcance, triage de bloqueadores y un registro de decisiones auditable.
- [ ] Definir una estructura de equipo, una matriz RACI y criterios para seleccionar y gestionar proveedores de IA, datos e integración.
- [ ] Construir un roadmap por incrementos con presupuesto, indicadores de valor, hitos de evidencia y condiciones de escalamiento o detención.

## Prerrequisitos

### Conocimientos requeridos

Antes de iniciar, debe poder:

- Distinguir métricas de actividad, calidad, adopción y resultado de negocio.
- Interpretar los artefactos creados en las Prácticas 1 y 2: caso de negocio, dataset, restricciones, arquitectura, EDT, Story Map, backlog, requisitos, guías de estilo y experimentos.
- Diferenciar entre una **capacidad** de IA y una **garantía verificable**.
- Identificar los niveles de autonomía de una solución de IA: informativo, asistido, recomendado, ejecutado con aprobación y automatizado con controles.
- Comprender que una demostración convincente no equivale a evidencia suficiente para operación productiva.

### Acceso y archivos requeridos

Debe disponer de:

- Acceso autorizado a ChatGPT, Microsoft 365 Copilot o Google Gemini.
- Los entregables de las Prácticas 1 y 2 dentro de `C:\IA_Product_Labs\`.
- El dataset sintético o anonimizado `C:\IA_Product_Labs\shared\datasets\tickets_sinteticos_v1.csv`.
- Los archivos de biblioteca de prompts previos:
  - `C:\IA_Product_Labs\shared\prompts\prompt_library_v1.md`
  - `C:\IA_Product_Labs\shared\prompts\prompt_library_v2.md`
- El manifiesto de ejecución:
  - `C:\IA_Product_Labs\shared\evidence\ai_execution_manifest.md`

> **Regla de seguridad:** no copie datos personales, secretos, contratos reales, credenciales, datos de clientes ni información confidencial en herramientas de IA. Utilice exclusivamente datos sintéticos o correctamente anonimizados.

## Entorno de laboratorio

### Requisitos de hardware

| Recurso | Requisito mínimo |
|---|---|
| Procesador | 64 bits, 2 núcleos o superior |
| Memoria RAM | 8 GB |
| Espacio libre | 10 GB |
| Red | 10 Mbps de descarga y 2 Mbps de carga |
| Pantalla | 1366x768; recomendado 1920x1080 |
| Periféricos | Teclado físico recomendado |

### Software de referencia

| Herramienta | Versión de referencia |
|---|---|
| Sistema operativo | Windows 11 Pro 23H2, compilación 22631.4890 |
| Navegador | Mozilla Firefox 136.0.1 |
| Editor | Visual Studio Code 1.98.2 |
| Suite documental | LibreOffice 25.2.1 |
| Herramienta de IA | ChatGPT GPT-4.1, Microsoft 365 Copilot o Google Gemini 2.5 Pro |

### Preparación de carpetas

Abra PowerShell y ejecute los siguientes comandos. No guarde entregables fuera de `C:\IA_Product_Labs\`.

```powershell
$root = "C:\IA_Product_Labs"
$folders = @(
  "$root\01_discovery",
  "$root\02_planning",
  "$root\03_governance",
  "$root\shared\datasets",
  "$root\shared\prompts",
  "$root\shared\evidence"
)

foreach ($folder in $folders) {
  New-Item -ItemType Directory -Force -Path $folder | Out-Null
}

Get-ChildItem -Path $root -Recurse -File | Select-Object FullName
```

Cree la estructura de trabajo de esta práctica:

```powershell
$labPath = "C:\IA_Product_Labs\03_governance\SoporteB2B_GenAI_MVP"
New-Item -ItemType Directory -Force -Path $labPath | Out-Null

$files = @(
  "00_indice_evidencias.md",
  "01_mapa_stakeholders_y_comunicacion.md",
  "02_contrato_expectativas.md",
  "03_control_alcance_bloqueadores_decisiones.md",
  "04_equipo_raci_y_proveedores.md",
  "05_roadmap_financiacion_riesgos.md",
  "06_dossier_gobernanza_ejecutivo.md"
)

foreach ($file in $files) {
  New-Item -ItemType File -Force -Path "$labPath\$file" | Out-Null
}

code $labPath
```

Si Visual Studio Code no está disponible desde PowerShell, abra el directorio manualmente desde el explorador de archivos.

## Procedimiento paso a paso

### Paso 1. Consolidar las evidencias de entrada y establecer trazabilidad

**Objetivo:** verificar que el plan de gobernanza se basa en evidencia previa y no en supuestos no documentados.

**Instrucciones:**

1. Revise los entregables de las Prácticas 1 y 2. Localice, como mínimo:
   - Problema de negocio priorizado.
   - Población objetivo y flujo actual.
   - Línea base de datos y restricciones.
   - Métricas de calidad y de negocio.
   - Arquitectura propuesta.
   - Story Map, backlog y requisitos verificables.
   - Plan de experimentación y criterios de éxito.
   - Biblioteca de prompts `v1` y `v2`.

2. Abra `C:\IA_Product_Labs\03_governance\SoporteB2B_GenAI_MVP\00_indice_evidencias.md`.

3. Documente una tabla de trazabilidad como la siguiente. Sustituya los ejemplos por la información de su caso.

```markdown
# Índice de evidencias y trazabilidad

| ID | Elemento de entrada | Ubicación o fuente | Evidencia clave | Implicación de gobernanza |
|---|---|---|---|---|
| EV-01 | Problema de negocio | Práctica 1 | Reducir tiempo de clasificación de tickets B2B | Medir tiempo medio por ticket antes y después |
| EV-02 | Restricción de datos | Práctica 1 | Solo datos sintéticos/anonimizados; sin PII en prompts | Revisión de seguridad antes de piloto |
| EV-03 | Métrica de calidad | Práctica 1 | Exactitud de clasificación objetivo >= 90 % | Gate de calidad para pasar a MVP |
| EV-04 | Arquitectura | Práctica 2 | RAG con base documental aprobada y logging | Evidencia de trazabilidad y observabilidad |
| EV-05 | Story Map | Práctica 2 | Agente revisa sugerencia antes de enviar respuesta | Autonomía nivel 1: asistido |
| EV-06 | Experimento | Práctica 2 | Comparar prompt base frente a prompt con recuperación | Decisión de prompt basada en evaluación controlada |
```

4. Añada una sección titulada `Supuestos pendientes de validación`. Registre únicamente supuestos que no tengan evidencia suficiente. Cada supuesto debe incluir un responsable y una fecha o hito de validación.

```markdown
| ID | Supuesto pendiente | Riesgo si es falso | Método de validación | Responsable | Hito |
|---|---|---|---|---|---|
| HP-01 | Los agentes usarán sugerencias en al menos 50 % de casos elegibles | Baja adopción y ausencia de valor | Piloto instrumentado y entrevistas | UX / Product Manager | Fin de piloto |
```

5. Verifique que los datos del dataset de prueba son sintéticos o anonimizados.

```powershell
Import-Csv "C:\IA_Product_Labs\shared\datasets\tickets_sinteticos_v1.csv" | Measure-Object
Get-Content "C:\IA_Product_Labs\shared\datasets\tickets_sinteticos_v1.csv" -TotalCount 5
```

**Resultado esperado:**

Un índice de evidencias que conecte explícitamente los artefactos previos con decisiones de gobernanza, controles, métricas y gates.

**Verificación:**

- El índice contiene al menos seis evidencias.
- Cada evidencia incluye una implicación concreta para la gobernanza.
- Los supuestos pendientes no se presentan como hechos.
- El dataset contiene al menos 10 registros y no expone información personal real.

---

### Paso 2. Construir el mapa de stakeholders y el plan de comunicación

**Objetivo:** gestionar expectativas mediante comunicación adaptada a la influencia, interés, riesgos y decisiones de cada stakeholder.

**Instrucciones:**

1. Abra `01_mapa_stakeholders_y_comunicacion.md`.

2. Identifique al menos ocho stakeholders o grupos. Debe incluir como mínimo:
   - Patrocinador de negocio.
   - Usuario final o representante de usuarios.
   - Product Manager o responsable de producto.
   - Líder técnico.
   - Responsable de datos.
   - Seguridad, privacidad o cumplimiento.
   - Operaciones o soporte.
   - Finanzas o responsable presupuestario.
   - Proveedor de IA, cloud o integración, si aplica.

3. Construya la matriz de influencia e interés. Utilice las categorías `Alta`, `Media` o `Baja`, pero justifique brevemente la estrategia de relación.

```markdown
# Mapa de stakeholders y comunicación

## Matriz de influencia e interés

| Stakeholder | Influencia | Interés | Necesidad principal | Riesgo o expectativa a gestionar | Estrategia |
|---|---:|---:|---|---|---|
| Patrocinador de negocio | Alta | Alta | Evidencia de valor y control de inversión | Esperar automatización total en el primer incremento | Gestionar de cerca; comité quincenal |
| Usuarios de soporte B2B | Media | Alta | Reducir esfuerzo sin perder control | Desconfianza en sugerencias incorrectas | Co-diseño, pruebas semanales y canal de feedback |
| Seguridad y privacidad | Alta | Alta | Protección de datos y trazabilidad | Bloqueo tardío por falta de evidencia | Involucrar desde diseño y en gates |
| Finanzas | Alta | Media | Coste por fase y retorno estimado | Escalamiento sin presupuesto controlado | Informe mensual de coste, valor y escenarios |
| Proveedor de modelo | Media | Media | Requisitos técnicos y consumo | Dependencia, cambios de modelo o coste variable | SLA, pruebas de regresión y cláusula de salida |
```

4. Para cada stakeholder, documente:
   - Decisión que puede afectar.
   - Evidencia que necesita.
   - Cadencia de comunicación.
   - Responsable de relación.
   - Canal de comunicación.
   - Mecanismo de escalamiento ante desacuerdo.

5. Añada un plan de comunicación con eventos operativos y ejecutivos.

```markdown
## Plan de comunicación

| Evento | Audiencia | Propósito | Entrada mínima | Salida o decisión | Cadencia | Responsable |
|---|---|---|---|---|---|---|
| Revisión de experimento | Producto, datos, ingeniería, UX | Evaluar hipótesis y calidad | Dataset, resultados, fallos y coste | Continuar, ajustar o detener experimento | Semanal | Product Manager |
| Comité de gobernanza | Patrocinador, producto, técnico, riesgo, finanzas | Autorizar paso de fase y resolver trade-offs | Tablero de métricas, riesgos y presupuesto | Go / Conditional Go / No-Go | Quincenal | Patrocinador |
| Revisión de seguridad | Seguridad, datos, técnico | Validar controles y excepciones | Clasificación de datos, logs, pruebas | Aprobación, condición o bloqueo | En cada gate | Responsable de seguridad |
| Sesión de usuarios | Usuarios piloto, UX, soporte | Evaluar utilidad y adopción | Prototipo, tareas y feedback | Mejoras de flujo y formación | Semanal en piloto | UX Lead |
| Informe ejecutivo | Dirección y finanzas | Comunicar valor, límites y gasto | Métricas de resultado, riesgos y forecast | Priorización financiera | Mensual | Product Manager |
```

6. Cree una matriz de mensajes para separar claramente capacidad, garantía, hipótesis y límite operativo. Evite frases ambiguas como “la IA resolverá todos los tickets”.

```markdown
## Matriz de mensajes y expectativas verificables

| Categoría | Mensaje para stakeholders | Evidencia requerida | Estado |
|---|---|---|---|
| Capacidad demostrada | El asistente puede proponer una clasificación y una respuesta basada en documentación aprobada para tickets elegibles. | Resultados de evaluación y demo controlada | Demostrada / por validar |
| Garantía objetivo | Al menos 90 % de respuestas evaluables deberán ser correctas, relevantes y trazables a una fuente aprobada. | Conjunto de evaluación versionado | Por validar |
| Hipótesis | Al menos 60 % de agentes piloto usará la sugerencia en más de la mitad de los casos elegibles. | Telemetría y entrevistas | Por validar |
| Límite operativo | El sistema no enviará respuestas externas automáticamente durante MVP y piloto. | Control de permisos y flujo de aprobación | Obligatorio |
| Riesgo comunicado | La cobertura depende de la vigencia de documentos fuente; respuestas sin fuente se derivan a revisión humana. | Monitoreo de recuperación y tasa de abstención | Gestionado |
```

7. Clasifique las incertidumbres identificadas en: datos, modelo, integración, adopción y regulación. Para cada una, indique la respuesta de gestión.

**Resultado esperado:**

Un mapa de stakeholders priorizado y un plan de comunicación que convierta expectativas difusas en compromisos observables, evidencia requerida y decisiones explícitas.

**Verificación:**

- Hay al menos ocho stakeholders identificados.
- Están presentes los cuatro stakeholders mínimos requeridos.
- Cada comunicación importante define audiencia, propósito, evidencia, cadencia y responsable.
- La documentación separa capacidad demostrada, garantía objetivo, hipótesis pendiente y límite operativo.
- Existe al menos una incertidumbre documentada para datos, modelo, integración y adopción.

---

### Paso 3. Definir el contrato de expectativas y los niveles de autonomía

**Objetivo:** establecer un acuerdo operativo versionado sobre alcance, valor, calidad, límites, supervisión humana y proceso de cambio.

**Instrucciones:**

1. Abra `02_contrato_expectativas.md`.

2. Redacte el contrato de expectativas con los siguientes apartados obligatorios:
   1. Problema y población objetivo.
   2. Resultado de negocio esperado.
   3. Casos de uso incluidos.
   4. Casos excluidos.
   5. Nivel de autonomía.
   6. Criterios de calidad.
   7. Supervisión humana.
   8. Datos permitidos y prohibidos.
   9. Riesgos y controles.
   10. Proceso de cambio.

3. Use una formulación concreta para el caso de soporte B2B. Puede emplear la siguiente plantilla y ajustarla a sus artefactos previos.

```markdown
# Contrato de expectativas del producto

**Versión:** 1.0  
**Caso:** SoporteB2B_GenAI_MVP  
**Propietario:** Product Manager  
**Fecha de revisión:** AAAA-MM-DD  

## 1. Problema y población objetivo
El producto busca reducir el tiempo necesario para clasificar y preparar respuestas iniciales para tickets B2B de baja y media complejidad. La población objetivo inicial son agentes internos de soporte B2B durante el piloto controlado.

## 2. Resultado esperado
Reducir el tiempo medio de preparación de respuesta en al menos 25 % frente a la línea base, sin aumentar la tasa de escalamiento ni disminuir la calidad percibida por los agentes.

## 3. Casos de uso incluidos
- Clasificación sugerida de tickets elegibles.
- Recuperación de políticas internas aprobadas.
- Borrador de respuesta para revisión humana.
- Explicación de la fuente utilizada cuando esté disponible.

## 4. Casos excluidos
- Asesoramiento legal, financiero o contractual individual.
- Decisiones automatizadas de alto impacto.
- Procesamiento de datos personales no autorizados.
- Envío automático de respuestas externas durante MVP y piloto.

## 5. Nivel de autonomía
Nivel 1: asistido. El sistema propone; el agente revisa, modifica, acepta o descarta antes de ejecutar cualquier comunicación externa.

## 6. Criterios de calidad
- Exactitud de clasificación: >= 90 % en conjunto de evaluación versionado.
- Respuestas con fuente verificable cuando aplique: >= 95 %.
- Incidentes de datos sensibles expuestos: 0.
- Tasa de aceptación o edición menor del umbral definido: evaluar semanalmente.
- Latencia y coste por caso dentro de límites aprobados.

## 7. Supervisión humana
El agente es responsable de la decisión final. Casos de baja confianza, ausencia de fuente, contenido sensible o intención no reconocida se derivan a revisión humana reforzada.

## 8. Datos permitidos y prohibidos
Permitidos: tickets sintéticos/anonimizados y documentos internos aprobados para el entorno autorizado.
Prohibidos: PII no autorizada, secretos, credenciales, contratos reales no aprobados y datos de producción fuera del procedimiento aprobado.

## 9. Riesgos y controles
Los riesgos de alucinación, obsolescencia documental, sesgo, sobrecoste, fuga de datos y dependencia de proveedor se gestionan mediante evaluación, guardrails, logging, revisión humana, límites de consumo y cláusulas de portabilidad.

## 10. Proceso de cambio
Todo cambio de modelo, prompt, fuente documental, regla de seguridad, autonomía o integración requiere registro de decisión, evaluación de regresión y aprobación según la matriz de gobernanza.
```

4. Añada una tabla de criterios de paso entre fases. Utilice tres resultados posibles: `Go`, `Go condicionado` o `No-Go`.

```markdown
## Gates de gobernanza

| Transición | Evidencia mínima | Decisor | Go | Go condicionado | No-Go |
|---|---|---|---|---|---|
| Experimento → MVP | Calidad y seguridad evaluadas; coste estimado; riesgo documentado | Comité de gobernanza | Métricas cumplen umbrales y no hay riesgo crítico abierto | Brechas con plan, dueño y fecha de resolución | Riesgo crítico o calidad insuficiente |
| MVP → Piloto | Flujo integrado, supervisión humana, formación y observabilidad | Patrocinador + seguridad + producto | Flujo funcional y usuarios preparados | Piloto limitado con restricciones adicionales | Falta de control de datos o operación |
| Piloto → Escalamiento | Valor, adopción, estabilidad y presupuesto observados | Comité ejecutivo | Resultado de negocio y coste dentro de rango | Extensión limitada para validar brecha específica | Sin valor observable o coste no sostenible |
```

5. Defina los umbrales usando los valores de sus prácticas previas. Si aún no existe un valor validado, etiquételo como `provisional` e indique el experimento que lo validará.

**Resultado esperado:**

Un contrato de expectativas versionado que explique qué hará el producto, qué no hará, cómo se controlará y cuándo se podrá ampliar su alcance.

**Verificación:**

- El nivel de autonomía está definido y es coherente con el flujo de la Práctica 2.
- Los casos excluidos contienen al menos un límite de seguridad y uno de negocio.
- Los gates no se basan solo en actividad; incluyen calidad, seguridad, adopción, valor y coste.
- El proceso de cambio exige evidencia y una decisión registrada.

---

### Paso 4. Establecer controles ágiles de alcance, bloqueadores y decisiones

**Objetivo:** prevenir scope creep, proteger el foco en resultados de negocio y resolver impedimentos mediante un proceso visible y trazable.

**Instrucciones:**

1. Abra `03_control_alcance_bloqueadores_decisiones.md`.

2. Defina la política de control de alcance. Debe indicar que una solicitud no entra automáticamente al backlog de implementación por ser técnicamente posible o por provenir de un stakeholder influyente.

3. Incluya criterios de entrada para una nueva iniciativa, historia o cambio:

```markdown
# Política de control de alcance

## Criterios de entrada para cambios

Una solicitud de cambio solo podrá ser analizada si incluye:

1. Problema de negocio o usuario claramente formulado.
2. Stakeholder solicitante y población afectada.
3. Métrica de resultado que se espera modificar.
4. Relación con Story Map, requisito o riesgo existente.
5. Impacto estimado en datos, seguridad, arquitectura, coste y operación.
6. Hipótesis verificable o evidencia disponible.
7. Propuesta de nivel de autonomía.
8. Criterio de aceptación verificable.
```

4. Defina criterios de salida para aceptar un incremento dentro de una fase:

```markdown
## Criterios de salida de un incremento

- Requisitos funcionales y no funcionales verificables cumplidos.
- Evaluación de regresión ejecutada contra conjunto versionado.
- Controles de datos, seguridad y trazabilidad validados.
- Métrica de calidad dentro del umbral aprobado.
- Coste por caso y consumo dentro de la tolerancia de la fase.
- Guía operativa, formación o instrucciones de uso actualizadas.
- Registro de decisión actualizado.
- Riesgos residuales aceptados explícitamente por el rol responsable.
```

5. Cree un flujo de triage de bloqueadores con categorías, severidad, propietario y tiempo objetivo de resolución.

```markdown
## Triage de bloqueadores

| Severidad | Definición | Ejemplo | Acción inicial | Escalamiento | Tiempo objetivo |
|---|---|---|---|---|---|
| Crítica | Riesgo de datos, seguridad, cumplimiento o daño relevante | Exposición potencial de PII | Detener flujo afectado y preservar evidencia | Seguridad + patrocinador | Inmediato; revisión el mismo día |
| Alta | Impide un gate o afecta métrica clave | API crítica no disponible | Crear plan de contingencia y dueño | Líder técnico + Product Manager | 1-2 días hábiles |
| Media | Afecta alcance o experiencia sin detener piloto | Baja calidad en una intención secundaria | Priorizar en backlog o experimento | Equipo de producto | Próxima revisión semanal |
| Baja | Mejora deseable sin impacto inmediato | Ajuste cosmético de mensaje | Registrar y evaluar por valor | Product Manager | Refinamiento |
```

6. Añada un registro de bloqueadores con al menos tres entradas. Incluya uno de datos o seguridad, uno técnico y uno de adopción.

```markdown
| ID | Fecha | Bloqueador | Categoría | Severidad | Impacto | Dueño | Decisión o mitigación | Estado |
|---|---|---|---|---|---|---|---|---|
| BL-01 | AAAA-MM-DD | Documento fuente sin fecha de vigencia | Datos | Alta | Riesgo de respuesta obsoleta | Data Owner | Excluir fuente hasta versionado | Abierto |
| BL-02 | AAAA-MM-DD | Variabilidad de formato en API de tickets | Integración | Media | Retrasa automatización de clasificación | Líder técnico | Adaptador y pruebas contractuales | En curso |
| BL-03 | AAAA-MM-DD | Agentes perciben sugerencias como imposición | Adopción | Media | Baja aceptación | UX Lead | Sesiones de co-diseño y control de edición | En curso |
```

7. Cree un registro de decisiones. Cada decisión debe mostrar alternativas consideradas y evidencia usada. No utilice “porque se solicitó” como justificación suficiente.

```markdown
## Registro de decisiones

| ID | Fecha | Decisión | Alternativas consideradas | Evidencia | Decisor | Consecuencia | Próxima revisión |
|---|---|---|---|---|---|---|---|
| AD-01 | AAAA-MM-DD | Mantener autonomía nivel 1 en piloto | Nivel 2 recomendado; nivel 3 con aprobación | Riesgo de calidad, feedback de usuarios y requisitos de seguridad | Comité de gobernanza | Menor automatización inicial; mayor control | Fin de piloto |
| AD-02 | AAAA-MM-DD | Limitar fuentes a documentos aprobados y versionados | Incluir repositorio completo | Evaluación de obsolescencia y revisión de datos | Data Owner + Seguridad | Menor cobertura inicial; menor riesgo | Próximo gate |
| AD-03 | AAAA-MM-DD | Implementar límite de coste por entorno | Sin límite; límite mensual | Forecast de consumo y presupuesto de fase | Finanzas + Líder técnico | Posible degradación controlada ante límite | Mensual |
```

8. Añada una sección de reglas contra scope creep:
   - No ampliar autonomía sin evidencia de calidad, seguridad y adopción.
   - No incorporar una fuente de datos sin propietario, clasificación y proceso de actualización.
   - No cambiar de modelo sin evaluación de regresión, análisis de coste y aprobación.
   - No comprometer fecha de escalamiento sin pasar los gates definidos.
   - Las solicitudes urgentes siguen el triage y quedan registradas.

**Resultado esperado:**

Una política operativa de alcance que convierta solicitudes, bloqueadores y decisiones en elementos medibles, priorizables y auditables.

**Verificación:**

- Cada cambio exige una métrica de resultado y criterio de aceptación.
- El triage distingue bloqueadores críticos de mejoras deseables.
- Hay al menos tres bloqueadores y tres decisiones registradas.
- Al menos una decisión aborda explícitamente autonomía, datos o coste.
- Los bloqueadores críticos contemplan detener el flujo afectado cuando corresponda.

---

### Paso 5. Diseñar la estructura de equipo, matriz RACI y estrategia de proveedores

**Objetivo:** asignar responsabilidades sin ambigüedad y definir criterios verificables para seleccionar y gestionar proveedores.

**Instrucciones:**

1. Abra `04_equipo_raci_y_proveedores.md`.

2. Proponga una estructura de equipo multifuncional adecuada para una iniciativa de IA a gran escala. Como mínimo incluya:
   - Patrocinador ejecutivo.
   - Product Manager.
   - Líder técnico o arquitecto.
   - Ingeniería de software e integración.
   - Ingeniería de datos o Data Owner.
   - Responsable de IA/ML o evaluación de modelos.
   - Seguridad, privacidad, legal o cumplimiento.
   - UX/Research.
   - Operaciones, soporte o MLOps/LLMOps.
   - Finanzas o FinOps.

3. Diferencie responsabilidades de producto, entrega y control. Por ejemplo:

```markdown
# Estructura de equipo

| Rol | Responsabilidad principal | Participación en gobernanza |
|---|---|---|
| Patrocinador ejecutivo | Prioriza valor, aprueba inversión y acepta riesgos estratégicos | Preside o delega comité de gobernanza |
| Product Manager | Mantiene trazabilidad problema-resultado-backlog y gestiona expectativas | Responsable de agenda, decisiones de producto y comunicación |
| Líder técnico | Define arquitectura, integración, observabilidad y viabilidad técnica | Presenta riesgos técnicos y planes de mitigación |
| Data Owner | Autoriza fuentes, calidad, procedencia y vigencia de datos | Aprueba incorporación o retiro de fuentes |
| Responsable de seguridad | Define controles, revisa excepciones e incidentes | Tiene autoridad de bloqueo ante riesgo crítico |
| Responsable IA/ML | Evalúa modelos, prompts, guardrails y regresiones | Mantiene conjunto de evaluación y evidencia de calidad |
| UX Lead | Evalúa utilidad, confianza, carga cognitiva y adopción | Gestiona evidencia cualitativa de usuarios |
| FinOps/Finanzas | Monitorea presupuesto, forecast y coste unitario | Valida continuidad financiera por fase |
```

4. Cree una matriz RACI. Asegúrese de que cada actividad tenga exactamente una persona o rol `A` (Accountable).

```markdown
## Matriz RACI

**Leyenda:** R = Responsible (ejecuta), A = Accountable (aprueba o responde por el resultado), C = Consulted (consultado), I = Informed (informado).

| Actividad | Patrocinador | Product Manager | Líder técnico | Data Owner | Seguridad | IA/ML | UX | Finanzas |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Priorizar objetivo de negocio | A | R | C | I | I | I | C | C |
| Aprobar caso de uso y autonomía | A | R | C | C | C | C | C | I |
| Aprobar fuentes de datos | I | C | C | A/R | C | C | I | I |
| Evaluar modelo, prompts y regresión | I | C | C | C | C | A/R | C | I |
| Aprobar controles de seguridad | I | C | R | C | A | C | I | I |
| Gestionar backlog y alcance | I | A/R | C | C | C | C | C | I |
| Operar piloto y feedback | I | A | R | C | C | C | R | I |
| Aprobar presupuesto de fase | A | R | C | I | I | I | I | C |
| Seleccionar proveedor estratégico | A | R | C | C | C | C | I | C |
| Autorizar escalamiento | A | R | C | C | C | C | C | C |
```

5. Defina el proceso de selección de proveedores. Debe incluir, como mínimo, modelos de IA, plataforma cloud, integración, datos o servicios especializados cuando sean necesarios.

6. Documente criterios ponderados de selección. Adapte las ponderaciones a su caso, pero el total debe sumar 100 %.

```markdown
## Criterios de selección de proveedores

| Criterio | Peso | Evidencia solicitada | Umbral o condición |
|---|---:|---|---|
| Seguridad y privacidad | 20 % | Certificaciones, controles de acceso, cifrado, DPA | Sin brechas críticas; aprobación de seguridad |
| Residencia y tratamiento de datos | 15 % | Regiones disponibles, retención, entrenamiento con datos del cliente | Compatible con política organizacional |
| Calidad y evaluación del modelo | 15 % | Resultados sobre conjunto propio, estabilidad, capacidades de guardrails | Cumplir umbral mínimo de calidad |
| Coste y previsibilidad | 15 % | Precios, límites, descuentos, forecast y alertas | Coste dentro del presupuesto de fase |
| Portabilidad y cláusula de salida | 10 % | Exportación de datos, formatos, plan de migración, terminación | Sin bloqueo inaceptable |
| Integración y observabilidad | 10 % | APIs, logs, métricas, auditoría, soporte de SDK | Compatible con arquitectura |
| Soporte y SLA | 10 % | Tiempo de respuesta, escalamiento, disponibilidad | SLA alineado a criticidad |
| Madurez y continuidad del proveedor | 5 % | Referencias, roadmap, estabilidad financiera | Riesgo aceptable |
```

7. Añada requisitos contractuales o de gestión de proveedor:
   - Prohibición o control explícito de entrenamiento con datos del cliente.
   - Propiedad y exportación de prompts, logs, evaluaciones y configuraciones.
   - Notificación de cambios de modelo, precios, retención o región.
   - Acuerdos de nivel de servicio y mecanismo de escalamiento.
   - Derecho de auditoría o evidencia equivalente.
   - Cláusula de salida y plan de reversibilidad.
   - Pruebas de regresión antes de adoptar una nueva versión de modelo.
   - Límites de gasto, alertas de consumo y responsable FinOps.

**Resultado esperado:**

Una estructura de equipo con responsabilidades claras, una RACI sin aprobadores ambiguos y un marco objetivo para seleccionar y controlar proveedores.

**Verificación:**

- La matriz RACI tiene un único `A` por actividad.
- Seguridad, datos y finanzas participan en decisiones que afectan sus dominios.
- Los criterios de proveedor suman 100 %.
- La estrategia contempla seguridad, residencia de datos, coste, portabilidad, soporte, evaluación y salida.
- La selección de proveedor no depende solo de la calidad percibida del modelo.

---

### Paso 6. Construir el roadmap, plan de financiación y mitigación de riesgos presupuestarios

**Objetivo:** establecer una secuencia de incrementos financiables basada en evidencia, valor y riesgo, no únicamente en fechas.

**Instrucciones:**

1. Abra `05_roadmap_financiacion_riesgos.md`.

2. Diseñe un roadmap por trimestres o incrementos. Para una práctica de laboratorio, use cuatro incrementos:
   - Incremento 0: preparación y validación.
   - Incremento 1: MVP asistido.
   - Incremento 2: piloto controlado.
   - Incremento 3: escalamiento condicionado.

3. Para cada incremento, documente:
   - Objetivo.
   - Alcance incluido y excluido.
   - Evidencia requerida.
   - Métrica de valor.
   - Métricas de calidad, adopción y coste.
   - Gate de decisión.
   - Presupuesto estimado.
   - Riesgos y mitigaciones.

```markdown
# Roadmap, financiación y riesgos presupuestarios

| Incremento | Horizonte | Objetivo | Entregables | Evidencia / gate | Métrica principal | Presupuesto estimado | Decisión |
|---|---|---|---|---|---|---:|---|
| 0. Preparación | Semanas 1-4 | Validar datos, prompts, arquitectura y restricciones | Dataset evaluado, conjunto de pruebas, controles base, forecast | Calidad inicial, revisión de seguridad y coste unitario estimado | Cobertura y calidad de evaluación | 10 % | Go / No-Go hacia MVP |
| 1. MVP asistido | Semanas 5-8 | Proponer clasificación y borradores con revisión humana | Flujo integrado limitado, logging, guardrails, guía de usuario | Regresión aprobada y operación observable | Exactitud y tasa de respuestas trazables | 25 % | Go condicionado hacia piloto |
| 2. Piloto controlado | Semanas 9-16 | Validar valor y adopción con usuarios reales autorizados | Piloto, formación, dashboard y feedback | Resultado de negocio, adopción, seguridad y coste observados | Reducción de tiempo sin degradación de calidad | 35 % | Go / extender / detener |
| 3. Escalamiento condicionado | Semanas 17-24 | Extender cobertura de forma gradual | Automatización limitada aprobada, operación y soporte ampliados | Cumplimiento de SLO, presupuesto y valor sostenido | Coste por caso y beneficio neto | 30 % | Escalar, mantener o retirar |
```

4. Defina un modelo de financiación por gates. No asigne todo el presupuesto al inicio sin criterios de continuidad.

```markdown
## Principios de financiación por evidencia

1. Cada incremento recibe financiación limitada y aprobada antes de iniciar.
2. El presupuesto siguiente se libera solo tras revisar métricas, riesgos y evidencia del incremento actual.
3. Los costes se separan en:
   - Costes fijos: desarrollo, integración, seguridad, formación.
   - Costes variables: tokens, inferencia, almacenamiento, llamadas API y soporte.
   - Costes de contingencia: incidentes, pruebas adicionales, migración o cambio de proveedor.
4. Todo forecast debe incluir escenario base, escenario de alta adopción y escenario de sobreconsumo.
5. El coste por caso se monitorea junto con la métrica de negocio; reducir coste sin valor no constituye éxito.
```

5. Añada una tabla de riesgos presupuestarios y mitigaciones.

```markdown
## Registro de riesgos presupuestarios

| ID | Riesgo | Probabilidad | Impacto | Indicador temprano | Mitigación | Dueño | Umbral de escalamiento |
|---|---|---:|---:|---|---|---|---|
| PR-01 | Consumo de tokens superior al forecast | Media | Alta | Coste diario por caso > 120 % del baseline | Límites, caché, prompts más breves y alertas | FinOps + Líder técnico | Revisar si supera 120 % durante 5 días |
| PR-02 | Bajo uso del piloto | Media | Alta | Adopción semanal < objetivo | Formación, UX, análisis de fricción y redefinir casos | Product Manager + UX | Detener expansión si no mejora en dos ciclos |
| PR-03 | Cambio de precio o modelo del proveedor | Media | Media | Aviso contractual o cambio de tarifa | Arquitectura portable y proveedor alternativo evaluado | Líder técnico | Comité extraordinario |
| PR-04 | Coste de integración heredada subestimado | Media | Alta | Desviación de esfuerzo > 20 % | Spike técnico y alcance incremental | Líder técnico | Repriorizar antes del siguiente gate |
| PR-05 | Revisión de seguridad retrasa el piloto | Baja | Alta | Controles sin evidencia a una semana del gate | Involucrar seguridad desde Incremento 0 | Seguridad + PM | No avanzar sin aprobación |
```

6. Defina los indicadores de cada nivel. Debe incluir al menos uno por categoría:

| Nivel | Indicador sugerido |
|---|---|
| Actividad | Número de tickets elegibles procesados |
| Calidad | Exactitud de clasificación y tasa de respuesta trazable |
| Adopción | Porcentaje de agentes que usan la sugerencia en casos elegibles |
| Resultado de negocio | Reducción del tiempo medio de preparación de respuesta |
| Riesgo | Incidentes de seguridad, tasa de abstención y fallos de guardrails |
| Finanzas | Coste por caso y desviación frente al forecast |

7. Especifique criterios cuantitativos de detención o reducción de alcance. Ejemplos:
   - Detener temporalmente el flujo ante un incidente de datos sensibles.
   - No escalar si la calidad no supera el umbral durante dos ciclos de mejora.
   - Reducir el piloto si el coste por caso supera el límite definido y no existe compensación de valor.
   - No aumentar autonomía si la tasa de aceptación, trazabilidad o seguridad no cumple el gate.

**Resultado esperado:**

Un roadmap que conecte inversión, entregables, evidencia, métricas, riesgos y decisiones de escalamiento o detención.

**Verificación:**

- El roadmap tiene al menos cuatro incrementos.
- Cada incremento contiene un gate explícito.
- El presupuesto se libera por fases y evidencia, no como compromiso irreversible.
- Existen riesgos de sobreconsumo, baja adopción, proveedor e integración.
- Se diferencian métricas de actividad, calidad, adopción, negocio, riesgo y finanzas.

---

### Paso 7. Crear la biblioteca de prompts v3 y registrar la ejecución de IA

**Objetivo:** mantener trazabilidad de cualquier uso de IA generativa utilizado para elaborar, revisar o mejorar los artefactos de gobernanza.

**Instrucciones:**

1. Cree el archivo `C:\IA_Product_Labs\shared\prompts\prompt_library_v3.md`.

2. No sobrescriba `prompt_library_v1.md` ni `prompt_library_v2.md`.

3. Registre al menos tres prompts utilizados durante esta práctica. Incluya propósito, entrada permitida, salida esperada, restricciones y criterio de revisión humana.

```markdown
# Biblioteca de prompts v3

## PR-03-01: Revisión de expectativas verificables

**Propósito:** transformar una afirmación ambigua sobre IA en capacidad, garantía, hipótesis y límite operativo.  
**Datos permitidos:** únicamente información sintética o anonimizada del caso SoporteB2B_GenAI_MVP.  
**Prompt:**

> Actúa como analista de producto de IA. A partir del problema, métricas y restricciones proporcionadas, clasifica cada afirmación en: capacidad demostrada, garantía objetivo, hipótesis pendiente o límite operativo. Reescribe las afirmaciones ambiguas como compromisos observables. No inventes resultados. Si falta evidencia, marca "por validar". Devuelve una tabla con afirmación original, clasificación, versión verificable, evidencia necesaria y riesgo.

**Salida esperada:** tabla revisable por Product Manager, seguridad y líderes técnicos.  
**Revisión humana obligatoria:** Product Manager valida alcance; seguridad valida restricciones.

## PR-03-02: Análisis de riesgos de proveedor

**Propósito:** identificar riesgos de proveedor para una solución de IA generativa.  
**Datos permitidos:** requisitos técnicos anonimizados, arquitectura conceptual y criterios de selección.  
**Prompt:**

> Evalúa los siguientes criterios de proveedor de IA: seguridad, residencia de datos, coste, portabilidad, soporte, observabilidad, evaluación de modelos y cláusula de salida. Genera riesgos, evidencia solicitada, preguntas de due diligence y señales de alerta. No recomiendes un proveedor específico sin evidencia comparativa.

**Salida esperada:** registro de riesgos y preguntas de evaluación.  
**Revisión humana obligatoria:** compras, seguridad, legal y líder técnico.

## PR-03-03: Revisión de coherencia del roadmap

**Propósito:** detectar inconsistencias entre métricas, gates, presupuesto y decisiones del roadmap.  
**Datos permitidos:** roadmap sin datos confidenciales.  
**Prompt:**

> Revisa el roadmap de una iniciativa de IA. Identifica incoherencias entre objetivos, métricas de calidad, adopción, resultado de negocio, coste, riesgos y gates. Señala métricas de actividad que se estén usando incorrectamente como evidencia de valor. Propón preguntas de validación, sin cambiar cifras ni inventar evidencia.

**Salida esperada:** lista priorizada de observaciones y preguntas.  
**Revisión humana obligatoria:** comité de gobernanza.
```

4. Use una herramienta de IA autorizada para ejecutar uno de los prompts anteriores con información sintética del caso.

5. Revise críticamente la respuesta. No copie resultados sin validación. Marque las recomendaciones aceptadas, rechazadas o pendientes.

6. Abra o cree `C:\IA_Product_Labs\shared\evidence\ai_execution_manifest.md` y añada una entrada sin eliminar el historial existente.

```markdown
## Ejecución IA - Práctica 3

| Campo | Registro |
|---|---|
| Fecha | AAAA-MM-DD |
| Hora y zona horaria | HH:MM, zona horaria |
| Caso | SoporteB2B_GenAI_MVP |
| Herramienta | ChatGPT / Microsoft 365 Copilot / Google Gemini |
| Modelo o versión exacta | Ejemplo: GPT-4.1, instantánea visible YYYY-MM-DD |
| Idioma | Español |
| Configuración visible | Ejemplo: chat estándar; sin conectores; temperatura no visible |
| ID de prompt | PR-03-01 |
| Propósito | Revisar expectativas verificables |
| Datos introducidos | Descripción de información sintética/anonimizada |
| Resultado utilizado | Resumen de observaciones aceptadas |
| Observaciones de calidad | Exactitud percibida, omisiones, sesgos, necesidad de revisión |
| Revisor humano | Nombre o rol anonimizado |
```

**Resultado esperado:**

Una biblioteca de prompts `v3` versionada y un manifiesto que documente de forma reproducible el uso de IA durante la práctica.

**Verificación:**

- Existe `prompt_library_v3.md`.
- Existen al menos tres prompts con restricciones de datos y revisión humana.
- Las versiones `v1` y `v2` permanecen sin sobrescribir.
- El manifiesto contiene herramienta, modelo o versión, fecha, hora, idioma, configuración visible, propósito y observaciones de calidad.
- No se han introducido datos reales no autorizados en la herramienta de IA.

---

### Paso 8. Integrar el dossier ejecutivo y operativo de gobernanza

**Objetivo:** crear un documento final apto para revisión por patrocinadores, producto, tecnología, seguridad y finanzas.

**Instrucciones:**

1. Abra `06_dossier_gobernanza_ejecutivo.md`.

2. Integre y resuma los artefactos de los pasos anteriores. No copie tablas completas si ya están en documentos de detalle; incluya enlaces o rutas relativas a los archivos de soporte.

3. Use la siguiente estructura:

```markdown
# Dossier de gobernanza: SoporteB2B_GenAI_MVP

## 1. Resumen ejecutivo
- Problema priorizado:
- Resultado de negocio esperado:
- Nivel de autonomía inicial:
- Decisión solicitada a patrocinadores:
- Riesgos principales:
- Próximo gate:

## 2. Alcance y límites
- Incluido:
- Excluido:
- Datos permitidos:
- Restricciones no negociables:

## 3. Modelo de gobernanza
| Decisión | Evidencia requerida | Cadencia | Decisor | Resultado posible |
|---|---|---|---|---|

## 4. Stakeholders y comunicación
Resumen y enlace a `01_mapa_stakeholders_y_comunicacion.md`.

## 5. Controles ágiles
Resumen de criterios de entrada, salida, triage y decisiones.
Enlace a `03_control_alcance_bloqueadores_decisiones.md`.

## 6. Equipo, RACI y proveedores
Resumen de roles, autoridad de bloqueo y estrategia de proveedor.
Enlace a `04_equipo_raci_y_proveedores.md`.

## 7. Roadmap y financiación
Resumen de incrementos, presupuesto, valor esperado y condiciones de continuidad.
Enlace a `05_roadmap_financiacion_riesgos.md`.

## 8. Decisiones requeridas
| ID | Decisión requerida | Evidencia disponible | Recomendación | Decisor | Fecha objetivo |
|---|---|---|---|---|---|

## 9. Anexo de trazabilidad
Enlace a `00_indice_evidencias.md`.
```

4. Incluya una tabla mínima de decisiones de gobernanza:

```markdown
| Decisión | Evidencia requerida | Cadencia | Decisor | Resultado posible |
|---|---|---|---|---|
| Cambio de prompt | Evaluación de regresión, impacto de coste y seguridad | Según cambio; revisión semanal | Product Manager con IA/ML | Aprobar, ajustar o rechazar |
| Cambio de modelo | Benchmark, coste, seguridad, compatibilidad y plan de reversión | En gate extraordinario | Comité de gobernanza | Go, Go condicionado o No-Go |
| Inclusión de fuente de datos | Propietario, clasificación, vigencia y controles | En cada incorporación | Data Owner + Seguridad | Aprobar o bloquear |
| Paso de fase | Métricas de calidad, adopción, valor, riesgo y presupuesto | Quincenal o al cierre de fase | Patrocinador / comité | Go, Go condicionado o No-Go |
| Incidente crítico | Evidencia del incidente, alcance e impacto | Inmediata | Seguridad con patrocinador informado | Detener, contener y remediar |
```

5. Revise el dossier desde la perspectiva de cada stakeholder:
   - El patrocinador debe poder identificar valor, coste, decisión y riesgo.
   - Seguridad debe poder identificar datos, controles, excepciones y autoridad de bloqueo.
   - El usuario debe poder identificar autonomía, supervisión y mecanismo de feedback.
   - El equipo técnico debe poder identificar gates, requisitos de evaluación y gestión de cambios.
   - Finanzas debe poder identificar presupuesto, forecast, umbrales y condiciones de continuidad.

**Resultado esperado:**

Un dossier ejecutivo y operativo que permita tomar decisiones informadas sin ocultar la incertidumbre inherente a la IA.

**Verificación:**

- El dossier identifica una decisión próxima y el decisor correspondiente.
- El dossier declara explícitamente el nivel de autonomía y los límites.
- Incluye rutas o referencias a los documentos de detalle.
- Las decisiones se basan en evidencia y no solo en una demostración.
- El documento comunica valor, riesgos, coste y criterios de continuidad.

## Validación y pruebas

Ejecute la siguiente lista de validación antes de entregar.

### Validación de estructura de archivos

```powershell
$labPath = "C:\IA_Product_Labs\03_governance\SoporteB2B_GenAI_MVP"

Get-ChildItem $labPath -File | Select-Object Name, Length, LastWriteTime

Test-Path "C:\IA_Product_Labs\shared\prompts\prompt_library_v3.md"
Test-Path "C:\IA_Product_Labs\shared\evidence\ai_execution_manifest.md"
```

Todos los comandos `Test-Path` deben devolver `True`.

### Lista de control de contenido

Marque cada criterio una vez validado:

- [ ] El índice de evidencias conecta problema, datos, métricas, arquitectura, experimentos y decisiones de gobernanza.
- [ ] El mapa contiene al menos ocho stakeholders y los cuatro grupos mínimos requeridos.
- [ ] El plan de comunicación distingue capacidades, garantías, hipótesis y límites operativos.
- [ ] El contrato de expectativas define población, resultado, alcance, exclusiones, autonomía, supervisión, datos y proceso de cambio.
- [ ] Los gates de experimento, MVP, piloto y escalamiento usan evidencia de calidad, riesgo, adopción, valor y coste.
- [ ] La política de alcance contiene criterios de entrada y salida verificables.
- [ ] El registro de bloqueadores incluye severidad, dueño, impacto y mecanismo de escalamiento.
- [ ] El registro de decisiones contiene alternativas y evidencia.
- [ ] La RACI tiene exactamente un responsable `A` por actividad.
- [ ] Los criterios de proveedor incluyen seguridad, residencia de datos, coste, portabilidad, soporte, evaluación y cláusula de salida.
- [ ] El roadmap vincula presupuesto, hitos, indicadores y decisiones de continuación o detención.
- [ ] La biblioteca `prompt_library_v3.md` y el manifiesto de IA están actualizados.
- [ ] No existen datos personales, secretos ni información real no autorizada en los entregables.

### Prueba de coherencia ejecutiva

Realice una revisión de cinco minutos respondiendo estas preguntas solo con el dossier final:

1. ¿Qué problema de negocio se prioriza y cómo se medirá su mejora?
2. ¿Qué puede hacer el sistema en la fase actual y qué no puede hacer?
3. ¿Quién puede detener el avance por un riesgo de seguridad o datos?
4. ¿Qué evidencia habilita el paso de piloto a escalamiento?
5. ¿Qué condición financiera obligaría a reducir, rediseñar o detener el alcance?
6. ¿Cómo se registra una modificación de prompt, modelo o fuente documental?

Si una pregunta no puede responderse con claridad, actualice el documento correspondiente antes de entregar.

## Resolución de problemas

### Problema 1: El comité solicita “automatizar todo” aunque el piloto aún no cumple las métricas

**Síntomas:** se solicita aumentar el nivel de autonomía o incluir casos de alto riesgo antes de alcanzar los umbrales de calidad, trazabilidad, adopción o seguridad.

**Causa:** se está confundiendo una demostración de capacidad con una garantía operativa, o se ha omitido comunicar las hipótesis y límites del producto.

**Solución:**

1. Consulte el contrato de expectativas y confirme el nivel de autonomía aprobado.
2. Presente las métricas reales del piloto separadas por calidad, adopción, resultado de negocio, riesgo y coste.
3. Registre la solicitud como cambio de alcance y aplique los criterios de entrada.
4. Proponga un `Go condicionado` únicamente si existe una mitigación verificable, un dueño y una fecha de revisión.
5. Si el cambio introduce riesgo crítico o contradice los límites operativos, regístrelo como `No-Go` hasta contar con evidencia suficiente.

### Problema 2: El coste estimado del proveedor de IA supera el presupuesto durante el piloto

**Síntomas:** el coste por ticket o el consumo diario supera el forecast; se activan alertas de presupuesto o disminuye la viabilidad financiera del escalamiento.

**Causa:** estimación incompleta de tokens, aumento de longitud de prompts, contexto documental excesivo, reintentos, baja eficiencia de recuperación o cambio de precios del proveedor.

**Solución:**

1. Compare el coste real por caso con el baseline y el umbral definido en `05_roadmap_financiacion_riesgos.md`.
2. Identifique el componente dominante: entrada, salida, recuperación, reintentos, almacenamiento o llamadas API.
3. Aplique mitigaciones controladas: acortar prompts, limitar contexto recuperado, usar caché, reducir reintentos y establecer límites de consumo.
4. Ejecute una evaluación de regresión para confirmar que la reducción de coste no deteriora calidad ni seguridad.
5. Registre la decisión y, si el valor no compensa el coste, reduzca el alcance o no autorice el escalamiento.

## Limpieza

1. Guarde todos los entregables exclusivamente en:

   ```text
   C:\IA_Product_Labs\03_governance\SoporteB2B_GenAI_MVP\
   ```

2. Confirme que las bibliotecas de prompts y el manifiesto permanecen en las rutas compartidas obligatorias:

   ```text
   C:\IA_Product_Labs\shared\prompts\prompt_library_v3.md
   C:\IA_Product_Labs\shared\evidence\ai_execution_manifest.md
   ```

3. Cierre las sesiones de herramientas de IA si trabajó en un equipo compartido.

4. Elimine únicamente archivos temporales no entregables creados fuera del directorio obligatorio. No elimine:
   - Los entregables de Prácticas 1 y 2.
   - `prompt_library_v1.md`, `prompt_library_v2.md` o `prompt_library_v3.md`.
   - El historial de `ai_execution_manifest.md`.
   - El dataset sintético de referencia.

5. Realice una copia de respaldo autorizada dentro de `C:\IA_Product_Labs\` si el procedimiento del curso lo requiere.

## Resumen

En esta práctica se construyó un sistema de gobernanza para una iniciativa de IA generativa que hace explícita la relación entre valor de negocio, restricciones, evidencia, decisiones y financiación. El dossier final establece expectativas verificables, controles contra scope creep, mecanismos de escalamiento, responsabilidades RACI, criterios para proveedores y un roadmap basado en gates.

El principio central es que el escalamiento de IA no debe depender de una demostración atractiva ni de una promesa genérica de automatización. Debe depender de evidencia trazable sobre calidad, seguridad, adopción, resultado de negocio, coste y capacidad operativa para gestionar cambios e incidentes.
