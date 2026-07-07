# Análisis crítico — "Búsqueda de proyectos y clientes. ver.1"

**Fecha de análisis:** 2026-07-06
**Documento analizado:** `Busqueda de proyectos y clientes. ver.1.docx`
**Naturaleza del documento:** Brief de un prospecto (empresa que vende personal técnico de ingeniería y construcción, aparentemente "Engineering Resources") que ya investigó por su cuenta —con ChatGPT, a juzgar por los enlaces con `utm_source=chatgpt.com`— y pegó las respuestas en un solo fichero. Contiene: contexto/necesidad, especificación funcional, fuentes, stack, arquitectura, procesos, costes y una segunda parte "hazlo tú mismo sin implementador".

---

## 1. Resumen ejecutivo

El documento es un punto de partida **notablemente bueno para un prospecto**: la necesidad de negocio está clara, la especificación funcional es accionable, y contiene varias decisiones maduras (MVP semi-automático, trazabilidad de fuentes, benchmark de proveedores de datos, criterios de salida antes de escalar gasto).

Sin embargo, es un **collage sin editar de al menos 2-3 respuestas de ChatGPT**: la numeración de secciones se duplica y se rompe, los presupuestos del MVP se contradicen entre secciones (25-45 k€, 35-55 k€, 50-75 k€ y 60 k€ según dónde se lea), hay un error conceptual de base (la definición de "FED"), el alcance geográfico es ambiguo (¿Europa o UE?), y **el documento termina cortado a mitad de una pregunta ("¿Es")**, señal de que falta contenido.

Para convertirlo en una especificación contratable hace falta una pasada de consolidación: un solo presupuesto, un solo alcance, una sola numeración, y resolver las contradicciones listadas abajo.

---

## 2. Contradicciones detectadas

### C1. El presupuesto del MVP tiene cuatro cifras distintas
| Dónde aparece | Cifra de implantación del MVP |
|---|---|
| "Resumen de costes" — MVP prudente | 25.000–45.000 € |
| "MVP profesional ligero" (mismo bloque) | 50.000–75.000 € |
| "Fase 1 — MVP, 8–12 semanas" — Implementación | 35.000–55.000 € (total fase: 45.000–75.000 €) |
| "Mi recomendación final" | 60.000 € + 3.000 €/mes × 6 = 78.000 € |

La sección de bloques de trabajo añade un "total realista" de **63.000–158.000 €**, aclarando que 25-45 k€ solo vale "con mucha validación manual". Las cifras son reconciliables si se lee con cuidado (son escenarios distintos), pero tal como está redactado un implementador puede anclar la negociación donde le convenga. **Debe quedar UNA cifra de referencia con sus supuestos.**

### C2. "No hacer scraping" vs. una arquitectura que hace scraping
La sección de riesgos legales dice tajantemente "**No hacer scraping**", pero:
- La arquitectura incluye "Web monitoring: páginas de proyectos, notas de prensa, permisos, EIA, licitaciones" — eso es monitorización/extracción de webs, es decir, scraping (de datos no personales, que es distinto, pero el documento no hace esa distinción).
- El Contact Finder "cruza web corporativa y noticias" para encontrar personas.

La distinción correcta sería: *scraping de información de proyectos (empresas, hitos) = aceptable con cuidado; scraping de datos personales, especialmente de LinkedIn = no*. El documento no la formula y deja una regla incumplible por su propia arquitectura.

### C3. "No debe inventar emails" vs. "Formato corporativo probable: nombre.apellido@empresa.com"
La especificación exige al sistema "No debe inventar emails" y "ningún dato sin fuente verificable" (cláusula contractual 10). Pero el proceso manual recomienda como paso 4 deducir el email por patrón corporativo. Eso es exactamente inventar un email (mitigado por el verificador del paso 5, pero contradice la regla absoluta que se le quiere imponer contractualmente al implementador). Hay que decidir: o se admite el patrón + verificación como fuente válida con nivel de confianza bajo, o no se admite para nadie.

### C4. Priorización geográfica inconsistente
- El **scoring comercial** solo puntúa (+15) Alemania, Países Bajos, Bélgica y Dinamarca.
- Los **filtros de Sales Navigator** añaden Francia, España y Polonia.
- Las **alertas por país** incluyen además Italia.

Si Francia, España, Polonia e Italia son mercados objetivo, deben puntuar en el scoring; si no lo son, sobran de las alertas. Tal como está, el sistema buscaría proyectos que su propio scoring penaliza.

### C5. "Europa (EU)" — alcance geográfico ambiguo
El contexto dice "en Europa (EU)" y la especificación "Solo Europa / UE", tratando ambos como sinónimos. No lo son: **Reino Unido y Noruega** concentran gran parte del offshore wind y oil & gas europeo (>300 M€) y quedan fuera de la UE, de TED y de la lista PCI/PMI. Es probablemente la decisión de alcance más cara del documento y está sin tomar.

### C6. Volúmenes objetivo que no cuadran entre secciones
- Entregable del MVP: 100 proyectos cargados, 30 monitorizados/semana, 10 oportunidades.
- Objetivo Fase 1 (el mismo MVP): 300–500 proyectos cargados, 50–100 oportunidades.
- Proceso semanal recomendado: 20 proyectos nuevos + 100 contactos **por semana**.
- Objetivo DIY a 60 días: 300 proyectos revisados, 100 contactos buenos.

El entregable contractual del MVP (sección 7) es 3-5 veces menor que el objetivo declarado para la misma fase (sección 8 de costes). El implementador se comprometerá con la cifra baja.

### C7. Doble estructura de numeración
El documento tiene dos series de secciones "1..9/11" (la especificación y el desglose de costes) más una tercera parte DIY con su propia numeración 1-9, sin separadores claros. Hay dos "sección 2", dos "sección 8", dos "sección 9" con contenidos distintos. Es el síntoma más visible de que son respuestas de ChatGPT pegadas sin consolidar.

### C8. Herramientas que bailan entre secciones
- Búsqueda web: el stack MVP dice "SerpAPI"; la recomendación final dice "Tavily + SerpAPI puntual"; la tabla de costes lista ambos.
- Automatización: "n8n, Celery" en el stack (mezcla incoherente: Celery es una cola de tareas Python, no una herramienta de automatización no-code comparable); luego "n8n o Make".
- Monedas mezcladas: Sales Navigator a 121 €/mes en una sección y 119,99 USD/mes en otra; Apollo en € y en USD según la tabla.

---

## 3. Errores y desaciertos

### E1. "FED (Fuentes de Energía Renovables)" — error conceptual de base ⚠️
El contexto define las fases del proyecto como "FED (Fuentes de Energía Renovables), FEED, construcción, operaciones". **FED no es una fase de proyecto y esa expansión del acrónimo no existe en la industria.** Lo que va antes del FEED se llama feasibility / conceptual design / pre-FEED (o FEL-1/FEL-2 en metodología Front-End Loading). La propia especificación (sección 2, clasificador de fases) lo corrige implícitamente sin decirlo: lista "Idea/feasibility, Pre-FEED/FEED, Permitting…". Es un error menor en la práctica pero grave en un documento que se va a entregar a terceros: delata confusión sobre el vocabulario del sector en el que se quiere vender.

### E2. Valor de TED sobreestimado para este caso de uso
TED se marca como fuente "obligatoria", pero TED cubre **contratación pública de la UE**. Los megaproyectos privados de energía (la mayoría del pipeline de hidrógeno, LNG, gigafactorías, eólica de desarrolladores privados) **no licitan por TED**. TED sirve para infraestructura pública, puertos, TSOs y utilities estatales; para el resto, las señales están en prensa especializada, notas de prensa de FID/EPC award y bases privadas. El documento no advierte esta limitación y puede generar expectativas equivocadas sobre el "coste bajo, valor medio" de las fuentes públicas.

### E3. Omisión legal importante: ePrivacy y normativa nacional de email en frío
El análisis GDPR es correcto hasta donde llega (interés legítimo, nota por contacto, EDPB), pero **omite la capa que de verdad restringe el outbound propuesto**: la directiva ePrivacy y sus transposiciones nacionales. En concreto:
- **Alemania** (mercado nº 1 del propio scoring): el §7 UWG exige en la práctica consentimiento previo incluso para email B2B en frío. Las secuencias de Lemlist/Instantly hacia contactos alemanes son sancionables (Abmahnung).
- Otros países objetivo (Países Bajos, Francia, Polonia) tienen matices propios B2B.
- Cita al **ICO (regulador británico)** como referencia para marketing B2B cuando el negocio apunta a la UE post-Brexit; la referencia útil serían el EDPB (que sí cita) y los reguladores nacionales (AEPD, BfDI, CNIL...).

El riesgo real del proyecto no es GDPR en abstracto, es enviar cold emails automatizados a Alemania.

### E4. Todos los datos de precios provienen de ChatGPT sin verificación
Todas las citas llevan `utm_source=chatgpt.com`. Precios como "SerpAPI desde 25 USD/mes", "Bing Grounding 14 USD/1.000 transacciones" o los rangos de bases privadas (15-150 k€/año) deben tratarse como **estimaciones sin verificar**, no como datos. En un documento que exige contractualmente "ningún dato sin fuente verificable" (¡acierto!), resulta irónico que sus propias cifras no cumplan ese estándar. Antes de presupuestar, verificar precios directamente con los proveedores.

### E5. Las citas del Anexo 1 no corresponden al alcance definido
Los ejemplos citados para ilustrar los eventos son un tender solar en **Níger**, una planta de **10 MW en Líbano** y un proyecto gasístico en **Costa de Marfil** — fuera de Europa y órdenes de magnitud por debajo de 300 M€. Son ilustrativos del concepto pero contradicen los dos filtros centrales del sistema (geografía y CAPEX), y de nuevo delatan copy-paste sin revisión.

### E6. Dependencia de Apollo/Clay sin señalar su zona gris
Se prohíbe el scraping pero se recomienda Apollo como fuente de contactos; buena parte de los datos de estos proveedores proviene de scraping/agregación cuya licitud bajo GDPR para contactos de la UE es discutida (Apollo, de hecho, restringe datos de ciertos países de la UE). No invalida la elección, pero el documento presenta "usar herramientas con licencia" como si eso transfiriera el riesgo legal por completo, y no es así: el responsable del tratamiento sigue siendo quien usa los datos.

### E7. Falta el coste de la validación humana interna
El proceso semanal exige validación humana (martes) y el DIY estima 10 h/semana, pero **el escenario con implementador no presupuesta ninguna dedicación interna**. Un sistema así sin un owner interno que valide y haga el outbound es la causa de fracaso más común; debería figurar como coste y como requisito.

### E8. Fases fuera de interés incluidas sin filtro
El contexto dice que solo interesan proyectos "en estudio y empezando construcción", pero el clasificador incluye commissioning/operations sin marcar que son fases de descarte (o de menor prioridad). Menor, pero fácil de arreglar.

---

## 4. Aciertos (a preservar en cualquier versión 2)

1. **MVP semi-automático antes que IA autónoma** — la recomendación más valiosa del documento. Evita el error clásico de automatizar un proceso no validado.
2. **Criterio de salida explícito**: "si no salen reuniones, no compres software más caro". Objetivo DIY de 60 días con métrica final (5-10 reuniones) — es un test de canal barato y bien diseñado.
3. **Benchmark comparativo de 2-3 proveedores de datos con 30 proyectos conocidos** antes de contratar bases privadas caras. Excelente práctica, poco habitual.
4. **Trazabilidad obligatoria**: cada dato con fuente y fecha, cada contacto con fuente y nivel de confianza, motor IA explicable ("nada de 'la IA cree que…'"). Correctísimo, y además condición para el cumplimiento GDPR.
5. **Lista de entregables contractuales** (sección 7) y sobre todo la sección "qué pedir en la oferta económica": precio cerrado, costes unitarios (por 1.000 búsquedas / 1.000 contactos), **propiedad del código y la base de datos**. Protege muy bien al cliente frente al implementador.
6. **Nota GDPR por contacto** (fuente, motivo, fecha de captura, opt-out, retención) — operativiza el interés legítimo tal como pide el EDPB.
7. **Escalado por fases condicionado a resultados** (Fase 3 "solo si ya genera reuniones comerciales") y no comprar Salesforce/ZoomInfo/Rystad/GlobalData de entrada.
8. **El modelo de datos mínimo** (proyecto y contacto) está bien pensado: incluye phase confidence score, source URLs, GDPR lawful basis note, next action — campos que suelen olvidarse.
9. **La taxonomía de señales/eventos** (FEED awarded, FID approved, EPC tender…) es el enfoque correcto: vender personal técnico depende de llegar en la ventana entre adjudicación y movilización, y el motor de señales apunta exactamente ahí.
10. **"Mejor 30 contactos buenos que 1.000 malos"** — filosofía correcta para outbound B2B de ticket alto, y coherente con la protección de la reputación de dominio.
11. La identificación de **cargos decisores por disciplina** (Package Manager, Subcontract Manager, QA/QC, Talent Acquisition…) demuestra conocimiento real del sector — es la parte más "propia" y menos genérica del documento.

---

## 5. Omisiones relevantes (ni acierto ni error: falta)

- **Reino Unido y Noruega**: decisión de alcance pendiente (ver C5).
- **Competencia y diferenciación**: no se analiza qué hacen los competidores (otras agencias de personal técnico) ni por qué el prospecto ganaría llegando antes.
- **Tasa de conversión esperada**: se fijan volúmenes (contactos, mensajes) pero ninguna hipótesis de conversión que justifique los 78 k€ del presupuesto de control frente al valor de un contrato ganado. Con un dato de margen por ingeniero desplazado/año, el business case se cierra o se descarta en una línea.
- **Idiomas**: el monitoreo de prensa/permisos en Alemania, Polonia, Francia o Italia requiere fuentes en idioma local; todo el ejemplo de alertas está en inglés.
- **Deduplicación entre fuentes públicas y privadas** se menciona, pero no quién arbitra cuando las fuentes se contradicen (CAPEX o fase distintos).
- **El documento está incompleto**: termina en "¿Es" — falta al menos la última pregunta/sección de la conversación original.

---

## 6. Recomendaciones para la conversación con el prospecto

1. **Pedir la versión completa** del documento (está cortado) y confirmar si hay más contexto de la conversación original con ChatGPT.
2. **Consolidar antes de presupuestar**: una sola cifra de MVP con supuestos explícitos (proponer partir del rango 45-75 k€ de la Fase 1, que es el más detallado), una sola lista de países con su peso en el scoring, y decisión Europa-geográfica vs. UE.
3. **Corregir el vocabulario** (FED → pre-FEED/feasibility) antes de que el documento circule ante terceros del sector.
4. **Rebajar expectativas sobre TED** y plantear desde el día 1 un piloto con 1 base privada (p. ej. IJGlobal o IIR) usando su propio benchmark de 30 proyectos.
5. **Revisar la estrategia de outbound por país** con asesoría legal ePrivacy/UWG antes de contratar Lemlist/Instantly, especialmente para Alemania (su mercado prioritario). Alternativa de menor riesgo: LinkedIn + teléfono para DACH, email para donde el opt-out sea suficiente.
6. **Presupuestar la dedicación interna** (mínimo el equivalente a las 10 h/semana del plan DIY) como condición de éxito del proyecto.
7. Valorar empezar directamente por el **plan DIY de 60 días** (150-600 €/mes) como fase 0: es el test más barato de la hipótesis comercial y el propio documento lo sugiere sin llegar a ordenarlo como paso previo al desembolso de 60 k€.

---

## 7. Veredicto

| Dimensión | Valoración |
|---|---|
| Claridad de la necesidad de negocio | Alta |
| Conocimiento del sector (roles, señales, fases) | Alta, con un error de vocabulario (FED) |
| Especificación funcional | Buena base, contratable tras consolidación |
| Coherencia interna | Baja: presupuestos, alcance y numeración contradictorios |
| Fiabilidad de datos (precios, fuentes) | Baja: todo sin verificar, origen ChatGPT |
| Cobertura legal | Parcial: GDPR sí, ePrivacy/UWG no |
| Madurez de la estrategia (fases, criterios de salida) | Alta |

**Conclusión:** prospecto valioso y bien orientado, con criterio comercial real; el documento necesita una versión 2 consolidada antes de usarse como base contractual. Las contradicciones presupuestarias (C1) y la omisión legal del cold email en Alemania (E3) son los dos puntos que conviene poner sobre la mesa primero.
