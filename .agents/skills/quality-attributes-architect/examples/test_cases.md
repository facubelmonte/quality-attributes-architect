# Casos de Prueba y Ejemplos de Validación de la Skill

Este documento contiene la validación y testing de la skill `quality-attributes-architect` utilizando los ejemplos conocidos del **Trabajo Práctico No. 3 (Diseño de Sistemas de Software - UNICEN)**.

---

## Caso de Prueba 1: Chequeo de Escenario Incompleto y Refinamiento (Ejercicio 2a TP3)

### Input al Asistente:
> *"Un sistema de cajero automático debe ser fácil de usar por una persona mayor."*

### Salida Generada por la Skill:

#### 1. Diagnóstico de Escenario
- **Enunciado Original:** "Un sistema de cajero automático debe ser fácil de usar por una persona mayor."
- **Atributo Identificado:** **Usabilidad (Usability)** - Facilidad de aprendizaje y operabilidad.

| Elemento SEI | Estado | Valor Detectado / Observación |
| :--- | :---: | :--- |
| **1. Fuente** | ✅ Presente | Persona mayor (usuario no habituado a la tecnología / tercera edad). |
| **2. Estímulo** | ⚠️ Ambiguo | "Usar el cajero" es vago; debe especificarse una tarea concreta (ej. realizar una extracción de dinero o consulta de saldo). |
| **3. Artefacto** | ✅ Presente | Sistema de cajero automático (específicamente la interfaz de usuario / pantalla y teclado). |
| **4. Ambiente** | ❌ Ausente | No se especifica el contexto de operación (ej. horario diurno, entorno ruidoso en sucursal bancaria, primera vez que usa el sistema). |
| **5. Respuesta** | ⚠️ Ambiguo | "Fácil de usar" es una cualidad subjetiva, no una respuesta técnica del sistema (debe brindar guiado por voz, tipografía ampliada y confirmaciones simples). |
| **6. Medida de Respuesta** | ❌ Ausente | No existe ninguna métrica cuantificable (falta tiempo de ejecución, tasa de errores o tasa de éxito). |

#### 2. Refinamiento y Aplicación de *Straw Man*:
- **Asunciones / Straw Man:** Se asume que la tarea típica es una extracción simple de efectivo. Se propone como medida cuantitativa que el 90% de los usuarios de más de 65 años completen la transacción en menos de 2 minutos sin asistencia humana y con 0 transacciones fallidas por confusión.

#### 3. Escenario Completado (SEI 6 Partes):
- **Atributo de Calidad:** Usabilidad
- **1. Fuente:** Persona mayor (usuario $\ge 65$ años sin entrenamiento previo).
- **2. Estímulo:** Intenta realizar una extracción de efectivo seleccionando la opción en pantalla.
- **3. Artefacto:** Interfaz de usuario (pantalla táctil con fuentes aumentadas, guiado por voz y teclado accesible) del cajero automático.
- **4. Ambiente:** Operación normal en horario diurno dentro de una sucursal bancaria.
- **5. Respuesta:** El sistema presenta una interfaz simplificada de alto contraste con audio-guía paso a paso y confirmación visual de cada botón presionado.
- **6. Medida de Respuesta:** El usuario completa la extracción exitosamente en menos de **90 segundos**, con una tasa de error inferior al **2%** y sin requerir asistencia del personal.

> **Resumen narrativo:** *Una persona mayor de 65 años sin entrenamiento previo realiza una extracción de efectivo en un cajero automático durante la operación normal; el sistema guía al usuario mediante interfaz táctil de alto contraste y audio-guía, permitiendo completar la transacción en menos de 90 segundos con menos del 2% de errores.*

---

## Caso de Prueba 2: Refinamiento de Tolerancia a Fallas en Rendering (Ejercicio 2b TP3)

### Input al Asistente:
> *"Se desea desarrollar un procesador de texto que sea tolerante a fallas, particularmente en casos de errores al hacer el rendering (pre-visualización) de un documento previamente a su impresión."*

### Salida Generada por la Skill:

#### 1. Diagnóstico de Escenario
- **Atributo Identificado:** **Disponibilidad / Tolerancia a Fallas (Availability / Fault Tolerance)**.
- **Partes Faltantes:** Faltaba la medida del tiempo de recuperación y preservación del documento en memoria/disco.

#### 2. Escenario Completado (SEI 6 Partes):
- **Atributo de Calidad:** Disponibilidad (Tolerancia a fallas)
- **1. Fuente:** Motor de renderizado gráfico o plugin de impresión externo.
- **2. Estímulo:** Se produce una excepción no controlada / crash durante la pre-visualización de un documento complejo.
- **3. Artefacto:** Módulo de pre-visualización e impresión del procesador de texto.
- **4. Ambiente:** Operación normal mientras el usuario tiene un documento con cambios sin guardar.
- **5. Respuesta:** El procesador de texto aísla el proceso de renderizado en un hilo/proceso independiente, captura la falla, notifica al usuario con un mensaje comprensible y preserva la sesión de edición activa sin congelar la aplicación ni perder datos.
- **6. Medida de Respuesta:** El proceso principal se recupera en menos de **500 milisegundos**, con **0% de pérdida de texto o formato** del documento original y permitiendo reintentar la operación.

---

## Caso de Prueba 3: Árbol de Utilidad Completo para el Sistema de Monopatines Eléctricos (Apéndice TP3)

### Contexto del Sistema:
Flota de monopatines eléctricos distribuidos en paradas con GPS, habilitación vía App móvil leyendo código QR, vinculación con Mercado Pago, pausas de hasta 15 min, aplicación Web para administración de paradas/precios y gestión de mantenimiento basada en kilometraje/tiempo de uso.

### Árbol de Utilidad Estructurado:

```text
                                  ┌── Geolocalización y Mapa ── (H, M) ─── Escenario 1 (Perf-1)
               ┌── Performance ───┤
               │                  └── Activación QR ─────────── (H, H) ─── Escenario 2 (Perf-2) [DRIVER]
               │
               │                  ┌── GPS / Pérdida Conexión ── (H, H) ─── Escenario 3 (Avail-1) [DRIVER]
               │── Availability ──┤
               │                  └── Failover de Servidor ──── (H, M) ─── Escenario 4 (Avail-2)
               │
               │                  ┌── Pasarela Mercado Pago ─── (H, H) ─── Escenario 5 (Sec-1) [DRIVER]
Utility (Raíz) ┼── Security ──────┤
               │                  └── Control de Tarifas / Admin(M, L) ─── Escenario 6 (Sec-2)
               │
               │                  ┌── Finalización en Parada ── (H, M) ─── Escenario 7 (Usab-1)
               │── Usability ─────┤
               │                  └── Pausa de Viaje (15 min) ─ (M, L) ─── Escenario 8 (Usab-2)
               │
               │                  ┌── Integración Mercado Pago  (H, M) ─── Escenario 9 (Interop-1)
               │── Interoperab. ──┤
               │                  └── GPS Hardware Monopatín ── (H, H) ─── Escenario 10 (Interop-2) [DRIVER]
               │
               └── Modifiability ─└── Nuevas Reglas de Cobro ── (M, M) ─── Escenario 11 (Mod-1)
```

---

### Tabla de Escenarios Detallados y Priorización `(Importancia, Dificultad)`:

| ID | Atributo | Sub-factor | Prioridad (Imp, Dif) | Escenario de Calidad SEI (Resumen Medible) |
| :---: | :--- | :--- | :---: | :--- |
| **QA-1** | **Performance** | Activación QR | **(H, H)** | *Un usuario escanea el código QR de un monopatín desde su app móvil con crédito disponible bajo carga pico (1.000 activaciones concurrentes); el backend valida el saldo con Mercado Pago, destraba el monopatín e inicia el viaje en menos de **1.5 segundos**.* |
| **QA-2** | **Performance** | Mapa de Disponibilidad | **(H, M)** | *Un usuario abre la aplicación móvil en un área concurrida; el mapa interactivo consulta y renderiza la ubicación y nivel de batería de todos los monopatines en un radio de 1 km en menos de **2 segundos** con conexión 4G estándar.* |
| **QA-3** | **Availability** | Desconexión GPS | **(H, H)** | *Un monopatín en tránsito pierde señal celular/GPS temporalmente en un área de sombra de cobertura; el dispositivo almacena localmente las coordenadas y tiempo transcurrido en memoria no volátil, sincronizando los datos con el servidor en menos de **5 segundos** tras recuperar la señal sin duplicar ni perder cobros.* |
| **QA-4** | **Availability** | Alta Disponibilidad Backend | **(H, M)** | *El nodo principal del servidor de viajes sufre una caída de hardware durante las horas pico de uso; el balanceador redirige el tráfico a los nodos secundarios en menos de **10 segundos**, manteniendo el $99.9\%$ de disponibilidad mensual.* |
| **QA-5** | **Security** | Pagos y Créditos | **(H, H)** | *Un atacante intenta manipular las solicitudes HTTP de recarga de saldo o finalización de viaje falseando datos de Mercado Pago; el API Gateway y el módulo de pago autentican la firma criptográfica y tokens OAuth, rechazando el $100\%$ de las transacciones inválidas y registrando la anomalía en el log de auditoría en $< 100\text{ ms}$.* |
| **QA-6** | **Security** | Autorización Admin | **(M, L)** | *Un usuario intenta invocar las APIs administrativas de anulación de cuentas o cambio de precios; el servicio de autorización valida los roles JWT y bloquea el acceso en menos de **50 ms**, alertando al equipo de seguridad.* |
| **QA-7** | **Usability** | Validación de Parada Permitida | **(H, M)** | *El usuario intenta finalizar el viaje fuera de una parada permitida; la app detecta mediante GPS la inconsistencia en menos de **1 segundo**, le muestra en el mapa la parada permitida más cercana y previene el cobro erróneo o finalización inválida.* |
| **QA-8** | **Usability** | Gestión de Pausa | **(M, L)** | *El usuario activa el botón "Pausar" durante su recorrido; la app apaga el motor del monopatín en menos de **1 segundo**, muestra un temporizador visible con cuenta regresiva de 15 minutos y le avisa con una notificación 2 minutos antes del cambio de tarifa.* |
| **QA-9** | **Interoperability**| Integración Mercado Pago | **(H, M)** | *El backend del servicio invoca la API REST de webhook de Mercado Pago para procesar débitos y consultar acreditaciones; el adaptador procesa las notificaciones de pago con un $99.99\%$ de éxito sintáctico y semántico.* |
| **QA-10**| **Interoperability**| Telemetría GPS IoT | **(H, H)** | *Los microcontroladores GPS de los monopatines (de distintos fabricantes o revisiones de firmware) transmiten posición, odometría y estado de batería vía protocolo MQTT/JSON; el concentrador IoT procesa y normaliza los paquetes con latencia $< 300\text{ ms}$ y tasa de descarte de paquetes válidos del $0\%$.* |
| **QA-11**| **Modifiability** | Reglas de Tarificación | **(M, M)** | *El administrador del negocio requiere incorporar un nuevo esquema de tarifas dinámicas (por horario o zona); el equipo de desarrollo implementa y testea la nueva regla en la capa de negocio en menos de **2 días-persona** sin modificar los componentes de la app móvil ni el firmware de los monopatines.* |

---

### Análisis de Architectural Drivers Críticos `(H, H)`:
Los escenarios categorizados como **`(H, H)`** son los principales impulsores del diseño arquitectónico del sistema de monopatines:
1. **QA-1 (Activación QR en $< 1.5$s)**: Impone una arquitectura desacoplada y orientada a eventos con caché en memoria (Redis) para validar sesiones y balancear la comunicación con los dispositivos IoT.
2. **QA-3 (Resiliencia ante pérdida de GPS)**: Requiere soporte de arquitectura distribuida offline-first en el hardware del monopatín (almacenamiento local en buffer y sincronización idempotente).
3. **QA-5 (Seguridad en pagos y tokens)**: Exige un API Gateway seguro, autenticación OAuth2/JWT y comunicación cifrada TLS 1.3 de extremo a extremo.
4. **QA-10 (Interoperabilidad IoT con flota heterogénea)**: Demanda un Gateway IoT con patrón *Adapter / Facade* que desacople el protocolo del dispositivo (MQTT/CoAP) del dominio del backend.
