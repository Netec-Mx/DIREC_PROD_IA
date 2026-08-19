# Práctica 1. Evaluar una necesidad de negocio real o basada en un caso de estudio para estructurar los cimientos de una iniciativa de IA, identificando la madurez de sus datos, mitigando sus restricciones de flujo y definiendo sus criterios iniciales de calidad y prompts.

## Metadatos

| Campo | Valor |
|---|---|
| Duración | 120 minutos |
| Complejidad | Media |
| Nivel de Bloom | Crear |

## Descripción general

En esta práctica se construirá un paquete de descubrimiento para un MVP de IA Generativa aplicado al soporte técnico B2B. El caso por defecto, `SoporteB2B_GenAI_MVP`, utiliza tickets sintéticos para evaluar una oportunidad de clasificación, resumen y preparación de borradores de respuesta para agentes humanos.

El resultado no será un sistema autónomo que responda a clientes, sino una propuesta verificable de asistencia humana con datos controlados, criterios de calidad, una restricción de flujo explícita y prompts versionados. Los entregables serán la entrada obligatoria de la Práctica 2.

## Objetivos de aprendizaje

Al finalizar la práctica, podrá:

- [ ] Formular un problema de negocio priorizado y una hipótesis de valor medible para una iniciativa de IA.
- [ ] Elaborar un inventario de datos y una matriz de madurez que incluya calidad, acceso, sensibilidad, actualización y propiedad.
- [ ] Identificar la restricción dominante del flujo de valor mediante la Teoría de las Restricciones.
- [ ] Definir un MVP de asistencia con supervisión humana, métricas de aceptación, casos de prueba y rúbrica de evaluación.
- [ ] Crear y ejecutar una biblioteca versionada de prompts para clasificación, resumen y borrador de respuesta.

## Prerrequisitos

### Conocimientos

- Comprensión básica de problemas de negocio, indicadores operativos y flujo de trabajo.
- Capacidad para redactar prompts y revisar críticamente una respuesta generada.
- Conocimiento introductorio de los conceptos problema–decisión–datos–acción–resultado.
- Comprensión de que la IA Generativa puede producir contenido plausible pero incorrecto y requiere supervisión humana.

### Acceso y condiciones

- Acceso activo y autorizado a ChatGPT, Microsoft 365 Copilot, Google Gemini u otra herramienta institucional aprobada.
- Uso exclusivo de datos sintéticos, públicos o previamente anonimizados.
- Prohibido cargar datos personales, secretos comerciales, credenciales, contratos no públicos o información regulada.
- Directorio raíz obligatorio: `C:\IA_Product_Labs\`
- Si utiliza un caso propio, sustituya el nombre de la organización por un identificador anonimizado.

## Entorno de laboratorio

### Hardware recomendado

| Recurso | Mínimo |
|---|---|
| Procesador | 64 bits, 2 núcleos o superior |
| Memoria RAM | 8 GB |
| Espacio libre | 10 GB |
| Pantalla | 1366x768; recomendado 1920x1080 |
| Conectividad | 10 Mbps de descarga y 2 Mbps de carga |

### Software

| Herramienta | Uso en la práctica |
|---|---|
| Windows 11 Pro | Sistema operativo |
| PowerShell | Creación de carpetas y archivos iniciales |
| Visual Studio Code o editor Markdown | Documentación de entregables |
| LibreOffice Calc | Revisión del dataset CSV |
| Herramienta de IA Generativa autorizada | Ejecución controlada de prompts |
| Firefox u otro navegador aprobado | Acceso a la herramienta de IA |

### Preparación inicial

1. Abra **PowerShell** sin privilegios de administrador.
2. Ejecute los siguientes comandos para crear la estructura obligatoria:

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

New-Item -ItemType Directory -Force -Path $folders
```

3. Verifique la estructura:

```powershell
Get-ChildItem -Path "C:\IA_Product_Labs" -Recurse -Directory | Select-Object FullName
```

**Resultado esperado:** se muestran las seis carpetas obligatorias.

**Verificación:** confirme que no trabajará ni guardará entregables fuera de `C:\IA_Product_Labs\`.

## Procedimiento paso a paso

### Paso 1. Delimitar el caso, el problema y la hipótesis de valor

**Objetivo:** transformar una aspiración tecnológica en un problema de negocio priorizado, accionable y medible.

#### Instrucciones

1. Utilice el caso recomendado:

   - **Caso:** `SoporteB2B_GenAI_MVP`
   - **Contexto:** una empresa de soporte técnico B2B recibe solicitudes por correo electrónico.
   - **Situación actual:** los agentes revisan manualmente cada mensaje, determinan su categoría, buscan antecedentes y redactan respuestas repetitivas.
   - **Aspiración inicial:** reducir el tiempo dedicado a clasificar, resumir y preparar borradores, manteniendo siempre la aprobación humana antes del envío.

2. Cree el archivo:

```text
C:\IA_Product_Labs\01_discovery\01_problema_priorizado.md
```

3. Copie y complete la siguiente plantilla. Puede conservar los valores propuestos para el caso de estudio o reemplazarlos por valores sintéticos de su caso anonimizado.

```markdown
# Problema priorizado — SoporteB2B_GenAI_MVP

## Objetivo estratégico
Mejorar la productividad del soporte técnico B2B sin reducir la calidad, trazabilidad ni seguridad de las respuestas al cliente.

## Problema operacional
Los agentes de soporte de primer nivel invierten tiempo en leer correos extensos, clasificar solicitudes, resumir el contexto y redactar respuestas iniciales. Esto incrementa el tiempo de preparación y retrasa el tratamiento de tickets prioritarios.

## Usuarios afectados
- Agentes de soporte de primer nivel.
- Supervisores de soporte.
- Clientes B2B que esperan una respuesta inicial clara y oportuna.

## Línea base sintética
- Volumen estimado: 80 tickets por día.
- Tiempo medio de lectura, clasificación y preparación inicial: 8 minutos por ticket.
- Tiempo disponible estimado de agentes: 600 minutos por día.
- Correcciones de calidad en respuestas iniciales: 18 % de los casos.
- Objetivo de referencia del MVP: reducir en 20 % el tiempo de preparación sin incrementar la tasa de correcciones.

## Problema–decisión–datos–acción–resultado
| Elemento | Definición |
|---|---|
| Problema | Preparación manual lenta y variable de tickets B2B. |
| Decisión | Determinar categoría, prioridad y contenido inicial de la respuesta. |
| Datos | Asunto, cuerpo anonimizado, producto, canal, prioridad esperada y respuesta de referencia sintética. |
| Acción | El agente revisa la clasificación, el resumen y el borrador; corrige y decide si envía o escala. |
| Resultado | Menor tiempo de preparación, calidad revisada y sin envío autónomo. |

## Hipótesis de valor
Para los agentes de soporte de primer nivel, que actualmente necesitan leer, clasificar y redactar respuestas iniciales para tickets B2B, proporcionaremos un asistente de IA Generativa que sugiera una categoría, un resumen estructurado y un borrador de respuesta. El objetivo es reducir en al menos 20 % el tiempo de preparación por ticket, manteniendo una exactitud de clasificación mínima de 80 %, una puntuación media de calidad de borrador de al menos 3 sobre 4 y una tasa de corrección sustantiva no superior al 20 %. La solución no enviará mensajes automáticamente ni usará datos personales o confidenciales.

## Alternativas evaluadas
| Alternativa | Ventaja | Limitación | Decisión |
|---|---|---|---|
| Capacitación y plantillas manuales | Bajo coste y rápida aplicación | No resume ni clasifica de forma consistente | Complementaria |
| Reglas fijas de palabras clave | Predecible para casos simples | Frágil ante lenguaje variable | Complementaria |
| Búsqueda mejorada en base de conocimiento | Ayuda a localizar contenido | No reduce por sí sola el esfuerzo de redacción | Complementaria |
| Asistente de IA Generativa con revisión humana | Clasifica, resume y prepara borradores | Puede alucinar o variar entre ejecuciones | Seleccionada para el MVP |

## Nivel de automatización
Asistencia. La IA propone; el agente humano revisa, corrige, aprueba, rechaza y ejecuta cualquier comunicación externa.

## Límites del MVP
- Sin envío automático de correos.
- Sin decisiones contractuales, financieras, legales o de seguridad.
- Sin uso de datos personales o no autorizados.
- Sin integración productiva con sistemas de tickets durante esta práctica.
- Sin afirmaciones que no estén respaldadas por los datos suministrados.
```

4. Revise que la hipótesis incluya: usuario, tarea, capacidad de IA, métrica cuantitativa y límites de riesgo.
5. Identifique si el problema podría resolverse solo con una alternativa más simple. Documente por qué la IA se evaluará como asistencia y no como sustitución.

**Resultado esperado:** un problema priorizado con una cadena completa desde objetivo estratégico hasta métricas y controles.

**Verificación:** el archivo debe contener una hipótesis de valor medible y declarar explícitamente que un humano conserva la decisión y el envío de respuestas.

---

### Paso 2. Crear el dataset sintético y la línea base de datos

**Objetivo:** disponer de una muestra controlada de al menos 10 tickets para pruebas reproducibles del MVP.

#### Instrucciones

1. Cree el archivo obligatorio:

```text
C:\IA_Product_Labs\shared\datasets\tickets_sinteticos_v1.csv
```

2. Ejecute el siguiente bloque en PowerShell:

```powershell
@'
ticket_id,fecha,canal,producto,asunto,cuerpo,categoria_esperada,prioridad_esperada,resumen_referencia,respuesta_referencia
TCK-001,2026-08-01,email,CloudDesk,"No puedo iniciar sesión","Usuario de prueba informa que recibe un mensaje de credenciales no válidas al acceder a CloudDesk desde el navegador. Solicita orientación.",acceso,media,"Fallo de inicio de sesión por credenciales no válidas en CloudDesk.","Gracias por contactarnos. Verifique que utiliza la cuenta asignada y restablezca la contraseña mediante el portal autorizado. Si el problema continúa, responda con la hora aproximada del intento."
TCK-002,2026-08-01,email,CloudDesk,"Error al exportar reporte","Al exportar el reporte mensual en formato CSV aparece el mensaje Export job failed. El cliente solicita una alternativa.",incidencia_tecnica,alta,"Falla de exportación CSV del reporte mensual con mensaje Export job failed.","Hemos registrado la incidencia de exportación. Como alternativa temporal, intente exportar en formato XLSX. El caso será revisado por soporte técnico y le informaremos el avance."
TCK-003,2026-08-02,email,SecureFlow,"Consulta sobre renovación","Cliente solicita confirmar el proceso general para renovar una suscripción anual que vence el próximo mes.",facturacion,media,"Consulta general sobre proceso de renovación anual.","Gracias por su consulta. La renovación se gestiona antes de la fecha de vencimiento mediante el canal comercial autorizado. Un agente revisará las opciones aplicables a su cuenta."
TCK-004,2026-08-02,email,CloudDesk,"Agregar nuevo usuario","Administrador de una cuenta de prueba pregunta cómo invitar a un integrante adicional al espacio de trabajo.",configuracion,baja,"Solicitud de instrucciones para invitar un usuario adicional.","Para agregar un integrante, ingrese a la administración del espacio de trabajo y seleccione la opción de usuarios o invitaciones. Verifique que su rol tenga permisos de administración."
TCK-005,2026-08-03,email,SecureFlow,"Alerta duplicada","Se generaron varias alertas iguales para la misma regla durante la mañana. El cliente pregunta si es un comportamiento conocido.",incidencia_tecnica,media,"Alertas duplicadas generadas por una regla durante la mañana.","Gracias por informarlo. Revisaremos el comportamiento de la regla y las alertas generadas. Por favor, conserve los identificadores de alerta para facilitar la revisión."
TCK-006,2026-08-03,email,CloudDesk,"Solicitud de capacitación","Cliente solicita una sesión introductoria para cinco usuarios nuevos sobre las funciones básicas de CloudDesk.",solicitud_servicio,baja,"Solicitud de capacitación introductoria para cinco usuarios.","Gracias por su interés. Registraremos la solicitud de capacitación y un responsable revisará disponibilidad, modalidad y alcance de la sesión."
TCK-007,2026-08-04,email,SecureFlow,"Acceso bloqueado tras varios intentos","Usuario de prueba indica que su acceso fue bloqueado después de varios intentos fallidos y necesita volver a entrar.",acceso,alta,"Cuenta bloqueada tras intentos fallidos de acceso.","Para proteger la cuenta, el acceso puede bloquearse tras varios intentos fallidos. Utilice el proceso autorizado de restablecimiento o contacte al administrador de su organización."
TCK-008,2026-08-04,email,CloudDesk,"Cambio de plan","Cliente pregunta si puede cambiar de un plan estándar a un plan avanzado y cuándo se aplicaría el cambio.",facturacion,media,"Consulta sobre cambio de plan estándar a avanzado.","Gracias por su consulta. Un responsable comercial revisará las opciones de cambio de plan y la fecha de aplicación según las condiciones vigentes."
TCK-009,2026-08-05,email,SecureFlow,"Integración no recibe eventos","Después de configurar una integración de prueba, no se reciben eventos en el destino configurado. Se adjunta descripción textual del comportamiento.",integracion,alta,"La integración configurada no entrega eventos al destino.","Hemos registrado la incidencia de integración. Verifique la configuración del destino y los permisos autorizados. El equipo técnico revisará el flujo de eventos reportado."
TCK-010,2026-08-05,email,CloudDesk,"Eliminar proyecto de prueba","Administrador solicita instrucciones para eliminar un proyecto creado para pruebas y confirmar si la acción se puede revertir.",configuracion,media,"Solicitud para eliminar un proyecto de prueba y conocer reversibilidad.","La eliminación de un proyecto debe realizarse solo por un administrador autorizado. Revise las opciones de recuperación disponibles antes de confirmar la acción, ya que algunas eliminaciones pueden no ser reversibles."
'@ | Set-Content -Path "C:\IA_Product_Labs\shared\datasets\tickets_sinteticos_v1.csv" -Encoding utf8
```

3. Abra el archivo en LibreOffice Calc o Visual Studio Code.
4. Confirme que existen al menos 10 registros de datos, sin contar la fila de encabezados.
5. Verifique que no hay nombres reales, correos electrónicos, direcciones, números de contrato, credenciales ni identificadores personales.

**Resultado esperado:** un CSV sintético con 10 tickets y valores de referencia para categorías, prioridades, resúmenes y respuestas.

**Verificación:** ejecute:

```powershell
(Import-Csv "C:\IA_Product_Labs\shared\datasets\tickets_sinteticos_v1.csv").Count
```

El resultado debe ser `10`.

---

### Paso 3. Elaborar el inventario y la matriz de madurez de datos

**Objetivo:** determinar si los datos disponibles son suficientemente utilizables, autorizados y trazables para el MVP.

#### Instrucciones

1. Cree el archivo:

```text
C:\IA_Product_Labs\01_discovery\02_inventario_madurez_datos.md
```

2. Utilice la escala siguiente para la evaluación:

| Puntuación | Interpretación |
|---|---|
| 1 | Deficiente: no apto sin corrección importante |
| 2 | Limitado: utilizable solo con restricciones |
| 3 | Aceptable: apto para pruebas controladas |
| 4 | Bueno: utilizable con controles operativos |
| 5 | Alto: confiable, gobernado y mantenido |

3. Documente el inventario con esta estructura:

```markdown
# Inventario y madurez de datos — SoporteB2B_GenAI_MVP

## Inventario de fuentes

| Fuente | Uso previsto | Completitud | Actualidad | Consistencia | Accesibilidad | Sensibilidad | Propietario | Riesgo principal | Control requerido |
|---|---|---:|---:|---:|---:|---|---|---|---|
| tickets_sinteticos_v1.csv | Pruebas de clasificación, resumen y borrador | 3 | 3 | 4 | 5 | Baja | Equipo de producto del laboratorio | Muestra pequeña y sintética | No extrapolar resultados a producción |
| Base de conocimiento aprobada simulada | Contexto futuro para respuestas | 2 | 2 | 3 | 2 | Media | Operaciones de soporte | Contenido incompleto o no actualizado | Validación de dueño y fecha de vigencia |
| Taxonomía de tickets simulada | Etiquetas para clasificación | 3 | 3 | 4 | 4 | Baja | Supervisión de soporte | Categorías ambiguas | Glosario y ejemplos por categoría |
| Métricas operativas sintéticas | Línea base de tiempo y calidad | 2 | 2 | 3 | 4 | Baja | Producto | No representan operación real | Confirmar antes de cualquier decisión presupuestaria |

## Matriz de madurez

| Dimensión | Evidencia observada | Puntuación | Interpretación | Acción antes de escalar |
|---|---|---:|---|---|
| Disponibilidad | Dataset disponible localmente con 10 registros | 3 | Apto para experimento inicial | Aumentar muestra y diversidad |
| Calidad de contenido | Campos completos y referencias definidas | 3 | Aceptable para prueba | Revisar ambigüedades y casos límite |
| Representatividad | Casos de soporte frecuentes, pero sintéticos | 2 | Limitada | Comparar con muestra anonimizada autorizada |
| Trazabilidad | Cada ticket tiene ID y resultados esperados | 4 | Buena | Mantener versionado de cambios |
| Actualidad | Fechas de ejemplo, sin actualización continua | 2 | Limitada | Definir responsable y frecuencia de actualización |
| Acceso autorizado | Archivo local y sintético | 5 | Alto | Mantener control de ubicación y permisos |
| Privacidad y sensibilidad | No contiene datos personales reales | 5 | Alto | Aplicar revisión antes de nuevos datasets |
| Propiedad de datos | Propietario académico definido para el laboratorio | 4 | Buena | Asignar propietario real antes de producción |

## Conclusión de aptitud
El dataset es apto para validar el diseño del prompt y la rúbrica de calidad en un entorno de laboratorio. No es apto para afirmar rendimiento productivo ni para automatizar respuestas. La principal brecha es la representatividad: se requiere una muestra mayor, autorizada y anonimizada antes de escalar.

## Criterio de salida de datos para Práctica 2
- Dataset versionado disponible.
- Propietario y sensibilidad documentados.
- Riesgos de acceso y actualización identificados.
- Restricciones de uso explícitas.
```

4. Calcule una puntuación promedio simple de madurez, si lo considera útil, pero no la use como sustituto del análisis cualitativo.
5. Declare una decisión explícita: **apto para experimento controlado**, **apto con restricciones** o **no apto**.

**Resultado esperado:** un inventario que distingue disponibilidad de datos, calidad, permisos y riesgos operativos.

**Verificación:** la matriz debe incluir, como mínimo, completitud, actualidad, consistencia, accesibilidad, sensibilidad, propietario y control requerido para cada fuente.

---

### Paso 4. Mapear el flujo de valor e identificar la restricción dominante

**Objetivo:** aplicar la Teoría de las Restricciones para evitar que la IA acelere actividades que simplemente trasladen el cuello de botella a otra etapa.

#### Instrucciones

1. Cree el archivo:

```text
C:\IA_Product_Labs\01_discovery\03_flujo_y_restriccion.md
```

2. Documente el flujo actual del caso de estudio:

```markdown
# Flujo de valor y restricción — SoporteB2B_GenAI_MVP

## Flujo actual

| Paso | Actividad | Responsable | Tiempo medio por ticket | Capacidad estimada | Salida |
|---:|---|---|---:|---:|---|
| 1 | Recibir y registrar correo | Sistema / agente | 1 min | 80 tickets/día | Ticket creado |
| 2 | Leer y entender solicitud | Agente L1 | 3 min | 200 tickets/día | Contexto comprendido |
| 3 | Clasificar categoría y prioridad | Agente L1 | 2 min | 300 tickets/día | Ticket clasificado |
| 4 | Buscar antecedentes o guía | Agente L1 | 4 min | 150 tickets/día | Información localizada |
| 5 | Redactar borrador inicial | Agente L1 | 3 min | 200 tickets/día | Borrador preparado |
| 6 | Revisar, corregir y enviar | Agente L1 | 2 min | 300 tickets/día | Respuesta aprobada |
| 7 | Escalar casos complejos | Especialista L2 | 12 min | 45 tickets/día | Caso especializado |

## Restricción dominante
La restricción dominante es la disponibilidad de especialistas L2 para casos complejos, con una capacidad estimada de 45 tickets por día. Acelerar indiscriminadamente la clasificación y redacción podría aumentar el número de escalados hacia L2 y empeorar el tiempo total de resolución.

## Aplicación de la Teoría de las Restricciones
1. Identificar: L2 es la restricción.
2. Explotar: asegurar que L2 reciba casos completos, bien resumidos y correctamente priorizados.
3. Subordinar: el asistente L1 no debe escalar por defecto ni generar ruido.
4. Elevar: evaluar capacitación, documentación y capacidad adicional de L2 solo si persiste la restricción.
5. Repetir: volver a medir si la restricción cambia.

## Intervención inicial de IA
La IA asistirá al agente L1 con:
- Clasificación sugerida.
- Resumen estructurado.
- Borrador de respuesta revisable.
- Señal de posible escalado acompañada de evidencia.

La IA no:
- Enviará respuestas.
- Escalará automáticamente.
- Cerrará tickets.
- Tomará decisiones de seguridad, contrato o facturación.

## Regla de protección de la restricción
Todo ticket recomendado para escalado debe ser revisado por el agente L1 y contener resumen, categoría propuesta, prioridad propuesta y motivo explícito. La tasa de escalado será monitoreada; no debe aumentar más de 5 puntos porcentuales respecto de la línea base sin aprobación del supervisor.
```

3. Compruebe que la restricción no se identifica solo por la actividad más lenta, sino por la capacidad que limita el flujo completo.
4. Explique cómo la propuesta de IA protege la restricción en lugar de desplazarle trabajo no filtrado.

**Resultado esperado:** un flujo visible, una restricción justificada y una intervención de IA acotada.

**Verificación:** el documento debe indicar qué actividad se asiste, cuál no se automatiza y qué métrica evitará que aumenten los escalados sin control.

---

### Paso 5. Definir criterios de calidad, casos de prueba y rúbrica

**Objetivo:** convertir la expectativa de “una buena respuesta” en criterios observables y evaluables.

#### Instrucciones

1. Cree el archivo:

```text
C:\IA_Product_Labs\01_discovery\04_calidad_y_pruebas.md
```

2. Registre los criterios de aceptación:

```markdown
# Calidad, criterios de aceptación y pruebas — SoporteB2B_GenAI_MVP

## Criterios de aceptación del MVP

| ID | Criterio verificable | Umbral de aceptación | Método de medición |
|---|---|---|---|
| CA-01 | Clasificación correcta | ≥ 8 de 10 tickets | Comparar con categoria_esperada |
| CA-02 | Prioridad adecuada | ≥ 8 de 10 tickets | Comparar con prioridad_esperada |
| CA-03 | Resumen fiel | Puntuación media ≥ 3/4 | Rúbrica humana |
| CA-04 | Borrador útil y profesional | Puntuación media ≥ 3/4 | Rúbrica humana |
| CA-05 | Ausencia de afirmaciones no respaldadas | 0 casos críticos | Revisión humana |
| CA-06 | Protección de datos | 0 datos personales o secretos introducidos | Inspección de entradas |
| CA-07 | Supervisión humana | 100 % de borradores revisados antes de uso | Registro de prueba |
| CA-08 | Efecto sobre la restricción | Escalados no aumentan > 5 puntos porcentuales | Métrica de flujo |

## Rúbrica de evaluación

| Dimensión | 1 — Deficiente | 2 — Parcial | 3 — Aceptable | 4 — Excelente |
|---|---|---|---|---|
| Clasificación | Categoría incorrecta sin justificación | Ambigua o poco útil | Correcta | Correcta y con justificación breve |
| Prioridad | Riesgo de priorización incorrecta | Necesita corrección importante | Adecuada | Adecuada y evidencia clara |
| Fidelidad del resumen | Omite o inventa hechos relevantes | Omite información importante | Resume hechos principales | Preciso, conciso y accionable |
| Calidad del borrador | Contiene errores, promesas o tono inadecuado | Requiere reescritura amplia | Requiere correcciones menores | Listo para revisión final |
| Seguridad y límites | Expone, inventa o decide indebidamente | Límite poco claro | Respeta límites | Declara límites y solicita revisión cuando corresponde |

## Casos de prueba

| Caso | Ticket | Condición a probar | Resultado esperado |
|---|---|---|---|
| CP-01 | TCK-001 | Problema de credenciales | Categoría acceso; prioridad media; sin pedir credenciales |
| CP-02 | TCK-002 | Error técnico con impacto operativo | Incidencia técnica; prioridad alta; borrador prudente |
| CP-03 | TCK-003 | Consulta comercial general | Facturación; prioridad media; sin inventar condiciones |
| CP-04 | TCK-004 | Configuración simple | Configuración; prioridad baja; instrucciones generales |
| CP-05 | TCK-005 | Posible defecto repetitivo | Incidencia técnica; prioridad media; solicitar evidencia autorizada |
| CP-06 | TCK-006 | Solicitud de servicio | Solicitud de servicio; prioridad baja |
| CP-07 | TCK-007 | Bloqueo de acceso | Acceso; prioridad alta; no solicitar contraseña |
| CP-08 | TCK-008 | Cambio de plan | Facturación; prioridad media; sin prometer precio |
| CP-09 | TCK-009 | Integración sin eventos | Integración; prioridad alta; recomendar revisión humana |
| CP-10 | TCK-010 | Acción potencialmente irreversible | Configuración; prioridad media; advertir revisión previa |

## Regla de fallo crítico
Un caso obtiene fallo crítico si el resultado:
- Solicita o revela credenciales.
- Afirma haber realizado una acción que no se realizó.
- Inventa políticas, precios, plazos o causas técnicas.
- Autoriza, rechaza o ejecuta una acción de alto impacto.
- Recomienda envío automático o escalado automático.
```

3. Añada una columna denominada `resultado_observado` a la tabla de casos después de ejecutar las pruebas en el Paso 7.
4. Asegure que un resultado técnicamente correcto no se considere aceptable si viola límites de seguridad o supervisión humana.

**Resultado esperado:** una definición objetiva de “aceptable” para el MVP y diez casos de prueba vinculados al dataset.

**Verificación:** cada criterio debe tener un umbral, método de medición y relación con una decisión de aceptación o rechazo.

---

### Paso 6. Crear la biblioteca inicial de prompts versionada

**Objetivo:** diseñar prompts reproducibles, con instrucciones de seguridad y formatos de salida verificables.

#### Instrucciones

1. Cree el archivo obligatorio:

```text
C:\IA_Product_Labs\shared\prompts\prompt_library_v1.md
```

2. Copie el contenido siguiente. No incluya datos reales en las ejecuciones.

```markdown
# Biblioteca de prompts v1 — SoporteB2B_GenAI_MVP

## Reglas comunes de uso
- Utilizar exclusivamente tickets sintéticos o anonimización aprobada.
- No solicitar, mostrar ni procesar contraseñas, datos personales, secretos comerciales o credenciales.
- No inventar políticas, precios, plazos, acciones realizadas ni causas técnicas.
- No enviar mensajes; producir solo una propuesta para revisión humana.
- Si faltan datos, declarar la incertidumbre y proponer una pregunta de aclaración.
- Idioma de salida: español.
- El agente humano es responsable de revisar, corregir y aprobar todo resultado.

---

## P-01 — Clasificación de ticket

**Propósito:** sugerir una categoría y prioridad para apoyar el enrutamiento humano.

```text
Actúa como asistente de clasificación para soporte técnico B2B. Analiza únicamente el ticket sintético proporcionado.

Categorías permitidas:
- acceso
- incidencia_tecnica
- facturacion
- configuracion
- solicitud_servicio
- integracion

Prioridades permitidas:
- baja
- media
- alta

Reglas:
1. No inventes hechos ausentes.
2. No solicites contraseñas ni datos personales.
3. No tomes acciones ni escales automáticamente.
4. Si la evidencia es insuficiente, usa "confianza": "baja" y explica qué falta.
5. Devuelve solo JSON válido.

Ticket:
ID: {{ticket_id}}
Asunto: {{asunto}}
Cuerpo: {{cuerpo}}

Formato:
{
  "ticket_id": "{{ticket_id}}",
  "categoria_sugerida": "",
  "prioridad_sugerida": "",
  "confianza": "alta|media|baja",
  "justificacion": "",
  "requiere_revision_humana": true,
  "motivo_revision": ""
}
```

---

## P-02 — Resumen estructurado

**Propósito:** reducir el tiempo de comprensión del ticket sin perder información relevante.

```text
Actúa como asistente de resumen para soporte técnico B2B. Resume exclusivamente los hechos presentes en el ticket sintético.

Reglas:
1. No inventes causas, acciones realizadas, políticas ni compromisos.
2. No incluyas datos personales, contraseñas o secretos.
3. Distingue entre hechos reportados e incertidumbres.
4. No recomiendes una acción irreversible.
5. Devuelve solo JSON válido en español.

Ticket:
ID: {{ticket_id}}
Producto: {{producto}}
Asunto: {{asunto}}
Cuerpo: {{cuerpo}}

Formato:
{
  "ticket_id": "{{ticket_id}}",
  "problema_reportado": "",
  "producto_afectado": "",
  "impacto_observado": "",
  "datos_faltantes": [],
  "siguiente_revision_humana_sugerida": "",
  "requiere_revision_humana": true
}
```

---

## P-03 — Borrador de respuesta asistida

**Propósito:** preparar un borrador que un agente humano pueda revisar antes de cualquier envío.

```text
Actúa como asistente de redacción para soporte técnico B2B. Redacta un borrador de respuesta basado únicamente en el ticket y en el resumen proporcionados.

Reglas obligatorias:
1. El borrador no debe afirmar que se ejecutó una acción si no existe evidencia.
2. No inventes precios, acuerdos, políticas, plazos o causas técnicas.
3. No solicites contraseñas ni información sensible.
4. Usa un tono profesional, claro y prudente.
5. Cuando corresponda, indica que un agente revisará el caso.
6. No incluyas firma personal, datos de contacto ni promesas no verificadas.
7. Devuelve solo JSON válido en español.

Ticket:
ID: {{ticket_id}}
Producto: {{producto}}
Asunto: {{asunto}}
Cuerpo: {{cuerpo}}

Resumen validado:
{{resumen_validado}}

Formato:
{
  "ticket_id": "{{ticket_id}}",
  "borrador_respuesta": "",
  "supuestos_o_limites": [],
  "requiere_revision_humana": true,
  "razon_revision": ""
}
```
```

3. Revise que cada prompt tenga propósito, instrucciones, restricciones, variables de entrada y formato de salida.
4. No sobrescriba versiones futuras: durante esta práctica solo debe existir y modificarse `prompt_library_v1.md`.

**Resultado esperado:** una biblioteca inicial con tres prompts identificables y reproducibles.

**Verificación:** confirme que los tres prompts contienen explícitamente revisión humana, prohibición de inventar hechos y prohibición de solicitar datos sensibles.

---

### Paso 7. Ejecutar los prompts y registrar evidencia de IA

**Objetivo:** probar los prompts sobre el dataset, comparar salidas contra referencias y mantener trazabilidad de herramienta, configuración y calidad.

#### Instrucciones

1. Cree el manifiesto obligatorio:

```text
C:\IA_Product_Labs\shared\evidence\ai_execution_manifest.md
```

2. Complete una entrada por cada sesión o configuración de ejecución. Use la fecha, hora y modelo que realmente aparezcan en su herramienta:

```markdown
# Manifiesto de ejecución de IA — SoporteB2B_GenAI_MVP

| Ejecución | Herramienta | Modelo o versión exacta visible | Fecha | Hora local | Idioma | Configuración visible | Propósito del prompt | Dataset o casos | Observaciones de calidad |
|---|---|---|---|---|---|---|---|---|---|
| E-01 | [Ej.: ChatGPT] | [Modelo mostrado por la herramienta] | [AAAA-MM-DD] | [HH:MM] | Español | [Ej.: configuración predeterminada; sin conectores] | P-01 Clasificación | TCK-001 a TCK-010 | [Pendiente] |
| E-02 | [Herramienta usada] | [Modelo mostrado] | [AAAA-MM-DD] | [HH:MM] | Español | [Configuración visible] | P-02 Resumen | TCK-001 a TCK-010 | [Pendiente] |
| E-03 | [Herramienta usada] | [Modelo mostrado] | [AAAA-MM-DD] | [HH:MM] | Español | [Configuración visible] | P-03 Borrador | TCK-001 a TCK-010 | [Pendiente] |
```

3. Abra `tickets_sinteticos_v1.csv`.
4. Para cada ticket, ejecute P-01. Copie únicamente el contenido sintético del ticket en las variables del prompt.
5. Registre los resultados en:

```text
C:\IA_Product_Labs\01_discovery\05_resultados_pruebas.csv
```

6. Cree el archivo de resultados con esta estructura:

```csv
ticket_id,categoria_esperada,categoria_observada,prioridad_esperada,prioridad_observada,clasificacion_correcta,prioridad_correcta,resumen_puntuacion_1a4,borrador_puntuacion_1a4,fallo_critico,observaciones,revisor
TCK-001,,,,,,,,,,,
TCK-002,,,,,,,,,,,
TCK-003,,,,,,,,,,,
TCK-004,,,,,,,,,,,
TCK-005,,,,,,,,,,,
TCK-006,,,,,,,,,,,
TCK-007,,,,,,,,,,,
TCK-008,,,,,,,,,,,
TCK-009,,,,,,,,,,,
TCK-010,,,,,,,,,,,
```

7. Ejecute P-02 para cada ticket. Evalúe la fidelidad del resumen mediante la rúbrica del Paso 5.
8. Ejecute P-03 usando el ticket y un resumen previamente revisado por usted. Evalúe el borrador mediante la rúbrica.
9. Para cada ticket, indique `SI` o `NO` en la columna `fallo_critico`.
10. Actualice las observaciones de calidad del manifiesto. Ejemplos válidos:
    - “La clasificación fue consistente; dos borradores requirieron eliminar una promesa temporal no respaldada.”
    - “El modelo respetó el formato JSON en 8 de 10 casos; se requirió reintento en dos casos.”
    - “No se detectaron solicitudes de credenciales ni exposición de información sensible.”

**Resultado esperado:** evidencia trazable de las ejecuciones y resultados comparables contra las referencias del dataset.

**Verificación:** el manifiesto debe registrar herramienta, modelo o versión visible, fecha, hora, idioma, configuración visible, propósito y observaciones de calidad.

---

### Paso 8. Calcular resultados, decidir el estado del MVP y consolidar el paquete de descubrimiento

**Objetivo:** tomar una decisión explícita basada en evidencias, no en impresiones generales sobre la calidad del modelo.

#### Instrucciones

1. Abra `05_resultados_pruebas.csv` en LibreOffice Calc.
2. Calcule las métricas:

| Métrica | Fórmula |
|---|---|
| Exactitud de clasificación | Tickets con `clasificacion_correcta = SI` / 10 × 100 |
| Exactitud de prioridad | Tickets con `prioridad_correcta = SI` / 10 × 100 |
| Calidad media del resumen | Suma de `resumen_puntuacion_1a4` / 10 |
| Calidad media del borrador | Suma de `borrador_puntuacion_1a4` / 10 |
| Tasa de fallos críticos | Tickets con `fallo_critico = SI` / 10 × 100 |

3. Añada al final de `04_calidad_y_pruebas.md` la siguiente sección y complete los valores reales:

```markdown
## Resultados de la ejecución

| Métrica | Resultado observado | Umbral | Estado |
|---|---:|---:|---|
| Exactitud de clasificación | [ ] % | ≥ 80 % | [Cumple / No cumple] |
| Exactitud de prioridad | [ ] % | ≥ 80 % | [Cumple / No cumple] |
| Calidad media del resumen | [ ] / 4 | ≥ 3 / 4 | [Cumple / No cumple] |
| Calidad media del borrador | [ ] / 4 | ≥ 3 / 4 | [Cumple / No cumple] |
| Fallos críticos | [ ] % | 0 % | [Cumple / No cumple] |

## Decisión del experimento
Estado: [Aprobado para planificación / Aprobado con ajustes / Requiere rediseño]

Justificación:
[Explicar brevemente la decisión usando las métricas, observaciones de calidad, restricciones de datos y protección de la restricción L2.]

## Ajustes propuestos para la siguiente iteración
1. [Ejemplo: añadir ejemplos de categorías ambiguas al prompt de clasificación.]
2. [Ejemplo: incorporar una base de conocimiento aprobada y fechada para el borrador.]
3. [Ejemplo: ampliar el dataset sintético con casos de baja confianza y casos límite.]
```

4. Aplique esta regla de decisión:

| Condición | Decisión |
|---|---|
| Todos los criterios se cumplen y no hay fallos críticos | Aprobado para planificación |
| No hay fallos críticos, pero una o más métricas no alcanzan el umbral | Aprobado con ajustes |
| Existe al menos un fallo crítico | Requiere rediseño antes de avanzar |

5. Revise que el paquete de descubrimiento contiene estos archivos:

```text
C:\IA_Product_Labs\01_discovery\01_problema_priorizado.md
C:\IA_Product_Labs\01_discovery\02_inventario_madurez_datos.md
C:\IA_Product_Labs\01_discovery\03_flujo_y_restriccion.md
C:\IA_Product_Labs\01_discovery\04_calidad_y_pruebas.md
C:\IA_Product_Labs\01_discovery\05_resultados_pruebas.csv
C:\IA_Product_Labs\shared\datasets\tickets_sinteticos_v1.csv
C:\IA_Product_Labs\shared\prompts\prompt_library_v1.md
C:\IA_Product_Labs\shared\evidence\ai_execution_manifest.md
```

**Resultado esperado:** una decisión trazable sobre la preparación del MVP para pasar a planificación ágil.

**Verificación:** ejecute el siguiente comando y confirme que los ocho archivos existen:

```powershell
$requiredFiles = @(
  "C:\IA_Product_Labs\01_discovery\01_problema_priorizado.md",
  "C:\IA_Product_Labs\01_discovery\02_inventario_madurez_datos.md",
  "C:\IA_Product_Labs\01_discovery\03_flujo_y_restriccion.md",
  "C:\IA_Product_Labs\01_discovery\04_calidad_y_pruebas.md",
  "C:\IA_Product_Labs\01_discovery\05_resultados_pruebas.csv",
  "C:\IA_Product_Labs\shared\datasets\tickets_sinteticos_v1.csv",
  "C:\IA_Product_Labs\shared\prompts\prompt_library_v1.md",
  "C:\IA_Product_Labs\shared\evidence\ai_execution_manifest.md"
)

$requiredFiles | ForEach-Object {
  [PSCustomObject]@{
    Archivo = $_
    Existe = Test-Path $_
  }
}
```

## Validación y pruebas

Utilice esta lista de control final antes de entregar la práctica:

| Área | Validación |
|---|---|
| Problema de negocio | Existe un problema medible, usuarios identificados, línea base, hipótesis de valor y límites de riesgo. |
| Idoneidad de IA | Se comparó la IA Generativa con alternativas más simples y se justificó su uso como asistencia. |
| Supervisión humana | El diseño prohíbe envío, escalado y cierre automático de tickets. |
| Datos | El dataset tiene al menos 10 registros sintéticos y no contiene información personal o confidencial. |
| Madurez de datos | El inventario registra calidad, acceso, sensibilidad, propietario, riesgo y control requerido. |
| Flujo y restricción | La restricción dominante está identificada y la intervención no aumenta escalados sin control. |
| Prompts | Existe `prompt_library_v1.md` con prompts de clasificación, resumen y borrador. |
| Calidad | Existen criterios medibles, rúbrica, diez casos de prueba y regla de fallo crítico. |
| Evidencia | El manifiesto contiene herramienta, modelo o versión, fecha, hora, idioma, configuración, propósito y observaciones. |
| Decisión | El resultado se clasifica como aprobado, aprobado con ajustes o requiere rediseño. |

La práctica se considera completa cuando todos los archivos requeridos existen, el dataset contiene únicamente datos sintéticos o anonimizados y la decisión del experimento está justificada con métricas.

## Resolución de problemas

### Problema 1: PowerShell muestra errores de ruta o no crea las carpetas

**Síntoma:** aparece un mensaje como “No se encuentra una parte de la ruta” o los archivos se crean fuera de `C:\IA_Product_Labs\`.

**Causa probable:** la variable de ruta fue modificada, se ejecutó parcialmente el bloque de creación o se usaron comillas incorrectas.

**Solución:**

1. Abra una nueva ventana de PowerShell.
2. Ejecute nuevamente el bloque de preparación inicial completo.
3. Compruebe la ruta actual con:

```powershell
Test-Path "C:\IA_Product_Labs"
```

4. Use siempre rutas completas que comiencen con `C:\IA_Product_Labs\`.
5. No guarde copias de trabajo en Escritorio, Descargas u otras ubicaciones.

### Problema 2: La herramienta de IA devuelve texto adicional o JSON inválido

**Síntoma:** la respuesta incluye explicaciones antes o después del JSON, usa categorías no permitidas o genera un borrador con afirmaciones no respaldadas.

**Causa probable:** el modelo no siguió completamente el formato, el prompt se copió incompleto o el ticket contiene contexto ambiguo.

**Solución:**

1. Verifique que copió todas las reglas del prompt, especialmente “Devuelve solo JSON válido”.
2. Ejecute nuevamente el prompt con la instrucción adicional: `Corrige tu salida anterior y devuelve únicamente JSON válido según el formato indicado.`
3. Mantenga el resultado original en las observaciones si desea evidenciar la variabilidad, pero evalúe la salida corregida de forma separada.
4. Si persiste una afirmación no respaldada, márquela como fallo crítico o corrección sustantiva y proponga un ajuste para `prompt_library_v2.md` en la siguiente práctica.

## Limpieza

1. Cierre LibreOffice Calc, Visual Studio Code y las pestañas de la herramienta de IA que contengan datos de prueba.
2. Elimine únicamente archivos temporales, capturas no requeridas o copias locales fuera de la ruta obligatoria.
3. No elimine los entregables del laboratorio.
4. No sobrescriba `prompt_library_v1.md`; esta versión debe conservarse como evidencia de la primera iteración.
5. Confirme que todos los archivos finales permanecen dentro de:

```text
C:\IA_Product_Labs\
```

## Resumen

En esta práctica se creó el paquete de descubrimiento de `SoporteB2B_GenAI_MVP`. El paquete conecta un objetivo estratégico con un problema operacional, datos disponibles, una restricción de flujo, un nivel de automatización asistida, métricas de aceptación y evidencia de experimentos con prompts.

La salida principal para la Práctica 2 será el conjunto de documentos de `01_discovery`, el dataset `tickets_sinteticos_v1.csv`, la biblioteca `prompt_library_v1.md` y el manifiesto de ejecución. La siguiente iteración deberá usar esta evidencia para elaborar el plan ágil de producto, la EDT, el Story Map, los requisitos verificables y una automatización de prompts sin sobrescribir los artefactos de esta práctica.
