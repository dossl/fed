# Búsqueda de proyectos y clientes — ver. 2 (consolidada)

**Fecha:** 2026-07-06
**Sustituye a:** `Busqueda de proyectos y clientes. ver.1.docx`
**Base:** análisis crítico del 2026-07-06 (`Analisis - Busqueda de proyectos y clientes ver.1.md`). Esta versión resuelve las 8 contradicciones (C1–C8) y los 8 desaciertos (E1–E8) detectados, siempre a favor de la opción **legal, profesional y realista**, y optimiza la combinación coste/beneficio para un MVP funcional de coste contenido.

> ⚠️ **Nota sobre precios:** todos los precios de herramientas y bases de datos de este documento son estimaciones de mercado a julio de 2026 y deben **verificarse directamente con cada proveedor antes de firmar nada**. Ninguna cifra de este documento se toma de respuestas de IA sin contrastar (ese fue uno de los defectos de la ver. 1).

---

## 1. Objetivo y alcance (decisiones cerradas)

**Objetivo de negocio:** detectar semanalmente proyectos industriales de gran CAPEX en Europa en fase temprana (estudio → inicio de construcción), identificar las empresas participantes (owner, EPC, ingenierías, subcontratistas) y sus decisores, y generar acciones comerciales para vender personal técnico de ingeniería y construcción.

**Decisiones de alcance** (resuelven C4, C5, E1, E8 de la ver. 1):

| Dimensión | Decisión | Justificación |
|---|---|---|
| Geografía | **Europa geográfica**, no solo UE: incluye **Reino Unido y Noruega** | UK y Noruega concentran gran parte del offshore wind y oil & gas >300 M€; excluirlos por usar "UE" como sinónimo de Europa era la decisión más cara de la ver. 1 |
| Priorización de países | **Tier 1 (foco comercial): Alemania, Países Bajos, Bélgica, Dinamarca.** Tier 2 (monitorizados): Francia, España, Polonia, Italia, Reino Unido, Noruega | Una sola lista, alineada entre alertas, filtros y scoring (la ver. 1 usaba tres listas distintas) |
| Sectores | Renovables (eólica on/offshore, solar utility scale, BESS, hidrógeno, biometano, PtX/amoníaco), oil & gas / LNG / infraestructura gasista, infraestructura industrial (puertos, data centers, químicas, gigafactorías, interconexiones) | Sin cambios respecto a ver. 1 |
| CAPEX | > 300 M€ (o indicios de escala equivalente). Excepciones marcables manualmente | Sin cambios |
| Fases de interés | **Feasibility / diseño conceptual / pre-FEED → FEED → permitting → FID → EPC tender/award → inicio de construcción.** Commissioning y operación = **descarte automático** | Corrige E1: la fase previa al FEED se llama *pre-FEED / feasibility* (el acrónimo "FED = Fuentes de Energía Renovables" de la ver. 1 no existe en la industria). Corrige E8: las fases sin interés comercial se filtran explícitamente |
| Idiomas de monitoreo | Inglés + **alemán, neerlandés, francés, español, polaco, italiano, danés/noruego** en alertas y prensa local | Nuevo: los permisos y la prensa regional de los países objetivo no se publican en inglés |

---

## 2. Cómo se resuelve cada contradicción de la ver. 1

| # | Contradicción original | Decisión en esta versión |
|---|---|---|
| C1 | Cuatro presupuestos distintos para el MVP (25–45 k€, 35–55 k€, 50–75 k€, 60 k€) | **Un solo presupuesto** por fases con supuestos explícitos (sección 8) |
| C2 | "No hacer scraping" vs. arquitectura con web monitoring | **Política de scraping en dos niveles** (sección 6.1): monitorizar información de proyectos sí; extraer datos personales no |
| C3 | "No inventar emails" vs. deducir email por patrón corporativo | **Política de emails verificados** (sección 6.4): el patrón inferido se admite solo como último recurso, siempre verificado, marcado con confianza baja, y nunca se envía a un email sin verificar |
| C4 | Scoring solo puntuaba 4 países pero se buscaba en 8 | Scoring por **tiers** alineado con la lista única de países (sección 7.3) |
| C5 | "Europa (EU)" ambiguo | Europa geográfica incl. UK y Noruega (sección 1) |
| C6 | Volúmenes objetivo incoherentes entre secciones | **Un solo cuadro de KPIs** (sección 10) usado también como entregable contractual |
| C7 | Doble numeración de secciones | Documento único con numeración única |
| C8 | Herramientas que cambiaban entre secciones (SerpAPI/Tavily, n8n/Celery/Make, monedas mezcladas) | **Una sola tabla de stack** con una elección por función, todo en EUR (sección 5) |

---

## 3. Estrategia general: tres fases con puertas de decisión

El principio de la ver. 1 que se conserva íntegro: **el valor no está en "usar IA", sino en señales tempranas + contactos correctos + acción comercial rápida**, y no se escala el gasto sin evidencia de retorno.

```
Fase 0 (60 días, operada por el implementador, ~4.800–6.200 €)  → puerta: ≥5 reuniones comerciales
Fase 1 (MVP con implementador, ~65.000 €)                       → puerta: ≥8 reuniones en 3 meses de operación
Fase 2 (profesionalización, 25–60 k€)                           → puerta: pipeline/contratos atribuibles al canal
```

**Fase 0 es obligatoria antes de construir el sistema.** La ver. 1 la describía como alternativa "hazlo tú mismo"; aquí se convierte en el test de la hipótesis comercial, **operado por el equipo del implementador como servicio facturable**: configura las herramientas, monta la infraestructura mínima, ejecuta el proceso semanal (10 h/semana) y documenta la infraestructura, los procedimientos y sus resultados (alcance y costes en la sección 8). Si con proceso manual + IA no salen reuniones, un sistema automatizado no las creará: automatizar multiplica un proceso que funciona, no arregla uno que no.

---

## 4. Fuentes de datos

### 4.1 Fuentes públicas (gratuitas o casi)

**Con expectativas corregidas** (resuelve E2): TED cubre **contratación pública** — sirve para TSOs, puertos, utilities estatales e infraestructura pública, pero **la mayoría de los megaproyectos privados de energía no licitan por TED**. Por eso las fuentes públicas se complementan con la cobertura del sector privado (4.2).

| Fuente | Cubre | Vía |
|---|---|---|
| TED (Tenders Electronic Daily) | Licitación pública UE | API oficial, acceso anónimo |
| EU Funding & Tenders Portal | Proyectos con financiación UE | API REST pública |
| Lista PCI/PMI de la Comisión Europea | Proyectos energéticos transfronterizos prioritarios | Publicación periódica |
| ENTSO-E TYNDP / ENTSOG TYNDP | Transmisión eléctrica, almacenamiento, gas e H2 | Publicaciones + datos |
| European Hydrogen Observatory / H2 Infrastructure Map | Proyectos de hidrógeno | Web/datos abiertos |
| **🆕 Registros nacionales de permisos** — no estaban en la ver. 1 y son la mejor señal temprana gratuita: RVO Bureau Energieprojecten (NL), portal UVP (DE), Planning Inspectorate NSIP (UK), enquêtes publiques (FR), NVE (NO), tramitación ambiental MITECO (ES) | Permitting/EIA de grandes proyectos, país por país | Web pública, monitorizable |
| Prensa especializada por RSS (Hydrogen Insight, Offshore Wind Biz, Upstream, enerG, prensa local por idioma) + notas de prensa de los ~30 mayores EPCs europeos | **Sector privado**: FEED awards, FID, EPC awards | Feedly + Google Alerts |

### 4.2 Cobertura del sector privado y bases de datos de pago

Decisión coste/beneficio para no depender de TED ni pagar 30–150 k€/año de entrada:

1. **MVP (Fases 0–1):** sector privado cubierto vía prensa especializada + notas de prensa de EPCs/owners + registros de permisos + LinkedIn. Coste ≈ 0.
2. **🆕 EIC DataStream (Energy Industries Council)** — no considerada en la ver. 1: base de datos de proyectos energéticos orientada precisamente a la cadena de suministro (empresas que venden servicios/personal a proyectos), accesible vía membresía EIC a un coste típicamente de **un orden de magnitud inferior** a GlobalData/Rystad/WoodMac (verificar cuota vigente según tamaño de empresa). Es el **primer piloto de pago recomendado**, en Fase 1 tardía o Fase 2.
3. **🆕 4C Offshore / TGS** — si la eólica offshore resulta ser el subsector con más tracción, base especializada de coste medio.
4. **Benchmark antes de comprar** (se conserva de la ver. 1, es una de sus mejores ideas): antes de contratar cualquier base grande (IJGlobal, Industrial Info Resources, GlobalData, Rystad, WoodMac), prueba comparativa de 2–3 proveedores contra **30 proyectos conocidos** — cobertura, frescura de fase, contactos, precio.
5. **No contratar en ninguna fase inicial:** GlobalData, Rystad, WoodMac, S&P, ZoomInfo, Salesforce.

---

## 5. Stack tecnológico (una sola elección por función)

Criterio: mínimo coste que no comprometa funcionalidad ni cumplimiento. Todo en EUR aprox./mes; verificar precios con el proveedor.

| Función | Elección | Coste aprox. | Por qué esta y no las alternativas de la ver. 1 |
|---|---|---|---|
| CRM | **Pipedrive** (Essential→Advanced) | 15–40 €/usuario | Suficiente para pipeline B2B; HubSpot Starter como alternativa válida; Salesforce descartado por coste |
| Base operativa | **Airtable Team** | ~20 €/usuario | Proyectos/empresas/contactos con interfaces; migrable a PostgreSQL si el volumen lo exige |
| Automatización | **n8n** (cloud o self-host) | 10–25 € | Se elimina la mención incoherente a Celery (cola de tareas Python, no herramienta de automatización); Make como alternativa no-code |
| IA — clasificación masiva | **🆕 API de Claude — modelo Haiku (`claude-haiku-4-5`)**, con Batch API (−50 %) | ~1 $/M tokens entrada, 5 $/M salida → estimado 10–40 €/mes al volumen del MVP | La ver. 1 solo contemplaba OpenAI. Regla de la propia ver. 1 aplicada con datos: modelo barato para clasificar cientos de noticias/semana |
| IA — análisis e informe semanal | **API de Claude — modelo Opus (`claude-opus-4-8`)** | 5 $/M entrada, 25 $/M salida → estimado 10–40 €/mes | Un solo informe semanal profundo: el coste es marginal y la calidad importa aquí |
| Búsqueda web programática | **Tavily** (pay-as-you-go ~0,008 $/crédito) + SerpAPI solo puntual | 20–60 € | Resuelve C8: una primaria y una de respaldo, no dos en paralelo |
| Noticias/alertas | **Feedly Pro + Google Alerts** (alertas en idiomas locales) | 0–10 € | |
| Prospección de decisores | **LinkedIn Sales Navigator Core** (1 licencia en MVP) | ~120 €/licencia | Herramienta con licencia, sin automatización sobre LinkedIn |
| Enriquecimiento de emails | **🆕 Dropcontact** | 25–50 € | No considerada en la ver. 1. Diseñada para cumplimiento RGPD (calcula y verifica emails bajo demanda, proveedor europeo, sin revender bases masivas). Sustituye a Apollo como fuente primaria y resuelve la zona gris legal (E6). Apollo queda como opción secundaria a evaluar con su DPA |
| Outbound email | **Manual con Google Workspace en el MVP.** Secuenciador (Lemlist/Instantly) solo en Fase 2 y **solo para países donde el email frío B2B es legal** (ver 6.3) | 0 € (MVP) | La ver. 1 recomendaba secuencias automatizadas sin filtro legal por país — inviable en Alemania o Dinamarca |
| Dashboard | **Looker Studio** | 0 € | Resuelve C8: gratuito y suficiente; Power BI/Metabase solo si hay una razón concreta |
| Documentos | Google Drive | incluido | |

**Total software MVP: ≈ 250–400 €/mes** en Fase 0 (1 usuario; herramientas configuradas y operadas por el equipo del implementador) y **≈ 400–700 €/mes** en Fase 1 (con 2 licencias Sales Navigator y volúmenes mayores), más hosting/backups (50–150 €) y mantenimiento del implementador (500–1.000 €) a partir de la puesta en producción.

---

## 6. Marco legal (integrado en el diseño, no como anexo)

Esta sección resuelve C2, C3, E3 y E6. El implementador elabora y entrega una **Propuesta de Cumplimiento GDPR** (matriz de canal por país + plantillas art. 14 + LIA; partida presupuestada en la sección 8). **El cliente debe validarla con sus propios asesores legales antes del primer envío; esa validación externa corre por cuenta del cliente y no está incluida ni se factura en este presupuesto.** El sistema debe **imponer** estas reglas, no solo documentarlas: el CRM bloquea el canal email para contactos de países donde no procede.

### 6.1 Política de scraping (dos niveles)

- **Permitido:** monitorización automatizada de **información de proyectos y empresas** en fuentes públicas (prensa, notas de prensa, registros de permisos, webs de proyectos), respetando robots.txt y condiciones de uso, sin extraer datos personales.
- **Prohibido:** scraping de **datos personales**; cualquier extracción automatizada de LinkedIn (contrario a sus condiciones); compra de listas masivas.
- Los contactos se obtienen solo por: búsqueda manual en Sales Navigator, Dropcontact (bajo DPA), web corporativa oficial y noticias donde la persona aparece públicamente en su rol.

### 6.2 GDPR operativo

Se conserva la **nota GDPR por contacto** de la ver. 1 (fuente, motivo comercial, relevancia, fecha de captura, opt-out, retención) y se añade lo que faltaba:

- **Base jurídica:** interés legítimo (art. 6.1.f) con **evaluación LIA documentada** (necesidad, finalidad, ponderación), según las directrices del EDPB.
- **🆕 Deber de información (art. 14 RGPD):** al captar datos de un contacto sin pedírselos a él, hay que informarle **en la primera comunicación o antes de 1 mes**: incluir en el primer mensaje una línea con el origen del dato y enlace a la política de privacidad con derechos y opt-out. La ver. 1 omitía esta obligación por completo.
- **Registro de actividades de tratamiento (RoPA)** con esta actividad de prospección.
- **Retención:** borrado o anonimización de contactos sin interacción tras **12 meses**; lista de supresión permanente para opt-outs.
- **DPAs firmados** con todos los encargados: Airtable, Pipedrive, Anthropic, Dropcontact, Google, n8n.
- Referencias normativas: EDPB (UE) y autoridades nacionales (AEPD, BfDI, CNIL…). La cita al ICO británico de la ver. 1 solo aplica si se trabaja el mercado UK (UK GDPR + PECR).

### 6.3 Matriz de canal de primer contacto por país 🆕

El punto legal más importante que la ver. 1 pasó por alto: el email frío B2B a personas identificadas está **restringido o prohibido en la mayoría de los mercados Tier 1** por la normativa ePrivacy nacional. Matriz de trabajo incluida en la Propuesta de Cumplimiento GDPR (pendiente de validación por los asesores legales del cliente):

| País | Email frío a persona nombrada | Canal de primer contacto recomendado |
|---|---|---|
| **Alemania** (Tier 1) | ❌ No (§7 UWG: opt-in incluso B2B; riesgo de *Abmahnung*) | LinkedIn + teléfono |
| **Dinamarca** (Tier 1) | ❌ No (Ley de prácticas de marketing: consentimiento, también B2B) | LinkedIn + teléfono |
| **Países Bajos** (Tier 1) | ✅ Sí a cuentas corporativas, con opt-out claro | Email + LinkedIn |
| **Bélgica** (Tier 1) | ⚠️ Con condiciones (excepción B2B para personas jurídicas / direcciones funcionales) | Email funcional + LinkedIn |
| Francia | ✅ Sí si el mensaje se relaciona con la función profesional, con opt-out (criterio CNIL) | Email + LinkedIn |
| España | ⚠️ Restringido (art. 21 LSSI: consentimiento salvo relación previa) | LinkedIn + teléfono; email tras el primer contacto |
| Polonia | ❌ Consentimiento requerido | LinkedIn + teléfono |
| Italia | ❌ Consentimiento como regla general | LinkedIn + teléfono |
| Reino Unido | ✅ Sí a *corporate subscribers* con opt-out (PECR) | Email + LinkedIn |
| Noruega | ⚠️ Direcciones corporativas genéricas sí; email personal de trabajo requiere consentimiento | LinkedIn + teléfono + email genérico |

**Consecuencia de diseño:** el playbook principal en Alemania y Dinamarca (los dos mercados con más peso en el scoring) es **LinkedIn + teléfono**, no email. El secuenciador de email de Fase 2 solo se activa para NL, FR, BE, UK.

### 6.4 Política de emails (resuelve C3)

Orden de obtención: (1) publicado por la empresa/web corporativa → confianza alta; (2) Dropcontact bajo DPA → confianza media; (3) **solo si 1 y 2 fallan**, patrón corporativo inferido (`nombre.apellido@empresa.com`) **siempre pasado por verificador SMTP** → confianza baja, marcado como "inferido". **Regla dura: jamás se envía a un email no verificado**, y todo contacto registra fuente + nivel de confianza. Así se mantiene el espíritu de "no inventar emails" sin la regla incumplible de la ver. 1.

---

## 7. Especificación funcional del sistema (MVP)

### 7.1 Módulos

1. **Ingesta:** APIs públicas (TED, EU F&T, ENTSO-E/G, Hydrogen Observatory) + RSS/alertas de prensa y permisos (multiidioma) + fuentes de pago cuando se activen.
2. **Normalización:** unificar nombres de empresa, deduplicar proyectos entre fuentes, estandarizar CAPEX/fechas/países/sectores; **cada dato guarda fuente, fecha y, en caso de conflicto entre fuentes, prevalece la más reciente de mayor rango** (fuente oficial > base de pago > prensa) con el conflicto registrado.
3. **Clasificador IA** (Haiku, por lotes): sector, país, CAPEX estimado, fase, probabilidad de oportunidad, perfiles técnicos probables. Salida estructurada con esquema JSON validado; toda clasificación con fase o CAPEX dudoso se marca para revisión humana. "No inventes datos; si no hay información, devuelve `no_encontrado`".
4. **Motor de señales:** FEED awarded · EPC tender launched · EPC award · FID approved · construction start · environmental permit granted · owner's engineer selected · framework agreement · hydrogen valley funding · port expansion approved. (Definiciones del anexo de la ver. 1, con ejemplos sustituidos por casos europeos >300 M€ — los ejemplos originales eran de Níger, Líbano y Costa de Marfil, fuera del alcance.)
5. **Company mapping:** owner, EPC, PMC/owner's engineer, ingenierías, subcontratistas por paquete.
6. **Contact finder:** Sales Navigator (manual) + Dropcontact, bajo la política 6.4. Cargos objetivo: Project Director, Construction Director/Manager, Site Manager, EPC Director, Engineering Manager, Procurement/Subcontract/Contract Manager, QA/QC Manager, HSE Manager, Commissioning Manager, Package Manager por disciplina, Talent Acquisition/Resource Manager.
7. **Scoring comercial** (7.3) → prioriza la cola de trabajo.
8. **Salida a CRM:** empresa + proyecto + oportunidad + contactos + tareas + resumen IA **con fuentes** — se conserva el principio de la ver. 1: motor explicable, "nada de 'la IA cree que…', siempre evidencia".
9. **Informe semanal** automático (Opus): nuevos proyectos, cambios de fase, nuevos actores, top 20 oportunidades con ficha.

### 7.2 Validación humana obligatoria

MVP **semi-automático** (decisión conservada de la ver. 1): ninguna acción de contacto se ejecuta sin aprobación humana. La dedicación operativa es **condición de éxito y partida presupuestaria**: mínimo **10 h/semana** de una persona con criterio comercial (la ver. 1 la exigía en el proceso pero no la presupuestaba — E7). **En Fase 0 estas 10 h/semana las ejecuta el equipo del implementador como servicio facturable** (sección 8); a partir de la puesta en producción de la Fase 1 las asume el cliente (o se contratan como servicio gestionado aparte).

### 7.3 Scoring (alineado con el alcance)

| Criterio | Puntos |
|---|---|
| CAPEX > 1.000 M€ | +20 (300–1.000 M€: +10) |
| Fase EPC award / inicio de construcción | +30 |
| Fase pre-FEED / FEED / EPC tender | +20 |
| País Tier 1 (DE, NL, BE, DK) | +15 |
| País Tier 2 (FR, ES, PL, IT, UK, NO) | +8 |
| Disciplinas núcleo presentes (LNG, H2, tanques, QA/QC, coating, insulation) | +20 |
| Contactos decisores identificados | +15 |
| Última noticia < 30 días | +10 |
| Fase commissioning / operación | descarte |

### 7.4 Modelo de datos mínimo

**Proyecto:** ID, nombre, país, región, sector, subsector, CAPEX + moneda, fase + confianza de fase, fecha estimada de inicio de construcción y de operación, owner/developer, EPC/FEED contractor, PMC/owner's engineer, subcontratistas conocidos, estado de permitting, estado de tender, URLs fuente, fecha de última actualización, score comercial, servicio recomendado, próxima acción, comercial responsable.

**Contacto:** nombre, empresa, cargo, seniority, departamento, proyecto relacionado, país, URL LinkedIn, email + **nivel de confianza + método de obtención**, teléfono, fuente, **base jurídica + fecha de captura + fecha de información art. 14 + opt-out + fecha de borrado programado** 🆕, último contacto, próxima acción, **canal permitido según matriz 6.3** 🆕.

### 7.5 Proceso semanal

| Día | Actividad |
|---|---|
| Lunes | El sistema entrega el informe semanal; revisión (2 h) |
| Martes | Validación humana: proyectos que encajan, empresas objetivo, contactos aprobados, estrategia owner/EPC/subcontratista (2 h) |
| Miércoles–jueves | Outbound **por el canal legal de cada país**: mensajes personalizados citando proyecto y fase; registro en CRM (3–4 h) |
| Viernes | Respuestas, follow-ups, descartes, ajuste de scoring (1 h) |

---

## 8. Presupuesto único (resuelve C1)

### Fase 0 — Validación del modelo de negocio, operada por el implementador (60 días)

El equipo del implementador ejecuta la Fase 0 como servicio facturable, con este alcance comprometido:

1. **Configuración de herramientas:** alta y parametrización del stack de la sección 5 (Airtable, Pipedrive, n8n, alertas multiidioma, Feedly, Sales Navigator, Dropcontact, plantillas de mensaje por canal y país).
2. **Infraestructura mínima:** entorno de automatización (n8n), repositorio de configuración, copias de seguridad y registro de actividad.
3. **Operación del proceso semanal (10 h/semana):** el ciclo lunes–viernes de la sección 7.5, ejecutado por el equipo del implementador durante las ~8–9 semanas de la fase, para validar el modelo de negocio.
4. **Documentación:** de la infraestructura montada, de los procedimientos empleados y de sus resultados (informe final con métricas y recomendación go/no-go).
5. **Propuesta de Cumplimiento GDPR:** matriz de canal por país + plantillas art. 14 + LIA (sección 6), elaborada por el implementador. La validación por asesores legales es responsabilidad y coste del cliente, fuera de este presupuesto.

**Partida de mano de obra del implementador** (tarifa media facturable de referencia: **30 €/h**; ajustar al tarifario vigente):

| Concepto de mano de obra | Horas | Coste |
|---|---|---|
| Configuración de herramientas e infraestructura mínima | 24–40 h | 720–1.200 € |
| Operación del proceso semanal (10 h/semana × 8–9 semanas) | 80–90 h | 2.400–2.700 € |
| Documentación de infraestructura, procedimientos y resultados | 16–24 h | 480–720 € |
| Propuesta de Cumplimiento GDPR (matriz por país + plantillas art. 14 + LIA) | 20–30 h | 600–900 € |
| **Subtotal mano de obra** | **140–184 h** | **4.200–5.520 €** |

**Presupuesto total de la Fase 0:**

| Concepto | Coste |
|---|---|
| Mano de obra del implementador (tabla anterior) | 4.200–5.520 € |
| Software (stack sección 5, 1 usuario, 2 meses) | ~600 € |
| Validación legal externa de la Propuesta de Cumplimiento GDPR | A cargo del cliente (no facturada, fuera de este presupuesto) |
| **Total Fase 0** | **≈ 4.800–6.200 €** |

**Puerta de decisión:** ≥ 5 reuniones comerciales generadas. Si no se alcanzan, **no se contrata el desarrollo del sistema** y se revisa la propuesta de valor, no la tecnología. La infraestructura, la configuración y la documentación entregadas en Fase 0 quedan en propiedad del cliente y son reutilizables como base de la Fase 1.

### Fase 1 — MVP con implementador (10–12 semanas + 6 meses de operación)

| Concepto | Coste |
|---|---|
| Implantación (precio cerrado objetivo; rango de negociación 45–55 k€) | **50.000 €** |
| Software 6 meses (400–700 €/mes) | ~3.300 € |
| Hosting, backups, logging 6 meses | ~600 € |
| Mantenimiento implementador 6 meses (500–1.000 €/mes desde puesta en producción) | ~3.000 € |
| Contingencia (~10 %) | 5.000 € |
| Actualización de la Propuesta de Cumplimiento GDPR a la implementación real (implementador, ~16 h) | 500 € |
| Validación legal externa de la implementación | A cargo del cliente (no facturada) |
| **Presupuesto de control Fase 1** | **≈ 65.000 €** |

Supuestos del precio cerrado: fuentes públicas + prensa/permisos (sin bases de pago), clasificador IA con salida estructurada, CRM Pipedrive, informe semanal, dashboard Looker Studio, matriz legal implementada como bloqueo de canal en CRM, manual operativo y formación. Bloques de trabajo y rangos unitarios: los de la tabla "coste de construcción" de la ver. 1 siguen siendo una referencia razonable para el desglose de la oferta.

**Puerta de decisión:** ≥ 8 reuniones comerciales en los 3 primeros meses de operación.

### Fase 2 — Profesionalización (solo tras superar la puerta)

| Concepto | Coste |
|---|---|
| Piloto de base privada (empezar por EIC DataStream; benchmark de 30 proyectos antes de cualquier base cara) | 5.000–25.000 €/año |
| Secuenciador outbound (solo NL/FR/BE/UK) + verificación de dominios/buzones | 100–300 €/mes |
| Mejoras técnicas (más fuentes, mejor deduplicación, scoring aprendido) | 15.000–35.000 € |
| **Total Fase 2** | **≈ 25.000–60.000 €** |

---

## 9. Exigencias contractuales al implementador

Se conservan las de la ver. 1 (eran de lo mejor del documento) y se añaden dos:

1. Documento de arquitectura (fuentes, APIs, modelo de datos, seguridad, RGPD).
2. Precio cerrado del MVP y coste mensual de mantenimiento.
3. Costes unitarios: por 1.000 búsquedas web, por 1.000 contactos enriquecidos, por nueva fuente, por cambio de CRM.
4. **Propiedad del código y de la base de datos** para Engineering Resources.
5. Cláusula: **ningún dato sin fuente verificable**; motor explicable (cada clasificación muestra su evidencia).
6. 🆕 **Cumplimiento como requisito funcional:** matriz de canal por país implementada como bloqueo en CRM; nota GDPR por contacto; plantilla art. 14 en el primer mensaje; borrado automático a los 12 meses; lista de supresión.
7. 🆕 **Criterio de aceptación medible** (los KPIs de la sección 10, no "el sistema funciona").
8. Manual operativo: añadir fuentes, revisar errores, cambiar criterios, exportar leads.
9. Sistema de alertas: informe semanal + alertas críticas en FEED/EPC/FID/construction start.

---

## 10. KPIs únicos (resuelve C6)

**Semana tipo en régimen (a partir del mes 2 de operación):**

| Métrica | Objetivo |
|---|---|
| Proyectos nuevos detectados | 20 |
| Cambios de fase relevantes | 10 |
| Empresas mapeadas | 50 |
| Contactos candidatos → validados | 100 → 30 |
| Oportunidades priorizadas en CRM | 10 |

**Aceptación del MVP (cierre de Fase 1):** ≥ 300 proyectos cualificados cargados y monitorizados; ≥ 50 oportunidades priorizadas acumuladas; ≥ 300 contactos validados con fuente y nota GDPR; CRM, informe semanal y dashboard operativos; matriz legal activa.

**Métrica de negocio (la única que justifica seguir invirtiendo):** reuniones comerciales — 5 en Fase 0, 8 por trimestre en Fase 1. Filosofía conservada de la ver. 1: *mejor 30 contactos buenos que 1.000 malos*; si no salen reuniones, no se compra software más caro.

---

## 11. Qué NO hacer (consolidado)

- Scraping de datos personales o automatización sobre LinkedIn.
- Email frío a personas nombradas en DE, DK, PL, IT (y ES sin relación previa).
- Enviar a emails no verificados o comprar listas masivas.
- Contratar GlobalData/Rystad/WoodMac/ZoomInfo/Salesforce antes de validar el canal.
- Construir IA totalmente autónoma sin validación humana.
- Confiar en TED como fuente principal de proyectos privados.
- Aprobar presupuestos sin costes unitarios, propiedad del código y criterios de aceptación medibles.
- Citar precios de herramientas sin verificarlos con el proveedor.

---

## 12. Novedades de esta versión respecto al documento original

| # | Novedad | Sustituye / añade |
|---|---|---|
| 1 | **Matriz legal de canal por país (ePrivacy/UWG/LSSI/PECR…)** implementada en el sistema | Añade la capa legal que faltaba; cambia el playbook de DE/DK a LinkedIn + teléfono |
| 2 | **Deber de información del art. 14 RGPD** en el primer contacto | Omitido en la ver. 1 |
| 3 | **Dropcontact** como enriquecedor de emails GDPR-first | Sustituye a Apollo como fuente primaria |
| 4 | **Registros nacionales de permisos** (RVO, UVP, NSIP, NVE…) como señal temprana gratuita | Fuente nueva, coste cero |
| 5 | **EIC DataStream** como primera base de pago (bajo coste, orientada a cadena de suministro) y **4C Offshore** para eólica marina | Alternativa al salto directo a bases de 30–150 k€/año |
| 6 | **API de Claude: Haiku para clasificación masiva (con Batch API −50 %) y Opus para el informe semanal** | Sustituye el genérico "OpenAI API"; aplica con datos la regla "modelo barato para volumen, potente para análisis" |
| 7 | **Looker Studio (0 €)** como dashboard único | Cierra la triple opción Power BI/Looker/Metabase |
| 8 | **Alertas y prensa en idiomas locales** | El monitoreo solo en inglés pierde permisos y prensa regional |
| 9 | **Fase 0 obligatoria con puerta de decisión, operada por el equipo del implementador** como servicio facturable: configuración, infraestructura mínima, operación de 10 h/semana y documentación de procedimientos y resultados | Convierte el "DIY" opcional en un servicio medible de validación del canal |
| 10 | **UK y Noruega dentro del alcance** con tiers de países únicos | Resuelve la ambigüedad Europa/UE |
| 11 | **Un presupuesto, un stack, unos KPIs** | Elimina las cuatro cifras de MVP, las herramientas duplicadas y los volúmenes incoherentes |
| 12 | Corrección terminológica: **pre-FEED/feasibility** (no "FED") | Evita el error de vocabulario ante clientes del sector |
| 13 | **Propuesta de Cumplimiento GDPR elaborada por el implementador** (partida propia), con validación legal externa a cargo del cliente y no facturada | Sustituye la partida genérica de "asesoría legal externa" de 1.500–3.000 € |
