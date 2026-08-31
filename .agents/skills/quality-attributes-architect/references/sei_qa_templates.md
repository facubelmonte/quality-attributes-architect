# Catálogo de Templates de Escenarios de Atributos de Calidad (SEI / Bass et al.)

Este documento contiene los templates generales y específicos de las 6 partes del Software Engineering Institute (SEI) descriptos en *Software Architecture in Practice* (Bass, Clements, Kazman) Capítulos 3 a 14.

---

## 1. Estructura General del Escenario de Calidad de 6 Partes

Todo escenario de calidad formal según el SEI debe descomponerse obligatoriamente en 6 elementos ortogonales:

```text
[ Fuente del Estímulo ] ──( Estímulo )──> [ Artefacto ] ──( Respuesta )──> [ Medida de Respuesta ]
                                                │
                                          [ Ambiente ]
```

1. **Fuente del estímulo (Source of stimulus):** La entidad (humana, sistema de hardware u otro sistema de software) que genera el estímulo.
2. **Estímulo (Stimulus):** La condición o evento que llega al sistema y requiere una acción o respuesta.
3. **Artefacto (Artifact):** La porción del sistema que es estimulada (el sistema completo, un servicio, un módulo, una base de datos, una interfaz).
4. **Ambiente / Entorno operacional (Environment):** Las condiciones operativas en las que se encuentra el sistema cuando ocurre el estímulo (operación normal, sobrecarga/pico, modo degradado, durante el despliegue, en mantenimiento, sin conectividad, etc.).
5. **Respuesta (Response):** La actividad o comportamiento observable que realiza el artefacto luego de recibir el estímulo.
6. **Medida de Respuesta (Response Measure):** El criterio de éxito cuantitativo y testeable para la respuesta (tiempo de latencia, throughput, porcentaje de disponibilidad, tiempo de recuperación, esfuerzo de cambio en días/persona, tasa de error).

---

## 2. Templates Específicos por Atributo de Calidad

### A. Disponibilidad (Availability) - Bass et al. Cap. 4
*Capacidad del sistema para estar operativo y entregar el servicio cuando se lo requiere, manejando fallas de forma que no se conviertan en interrupciones del servicio.*

| Parte | Valores Típicos / Opciones |
| :--- | :--- |
| **Fuente** | Interna o externa: hardware, software, red, operador humano, entorno físico. |
| **Estímulo** | Falla (*fault*): caída de un servidor, timeout de base de datos, corrupción de mensajes, excepción no capturada, corte de red. |
| **Artefacto** | Sistema completo, clúster de servidores, proceso background, microservicio, base de datos. |
| **Ambiente** | Operación normal, operación degradada, inicio del sistema, mantenimiento programado. |
| **Respuesta** | Registrar la falla (log), aislar el componente defectuoso, realizar failover al nodo backup, operar en modo degradado, notificar al operador, reiniciar el servicio. |
| **Medida de Respuesta** | Tiempo de indisponibilidad (*downtime*), tiempo medio entre fallas (MTBF), tiempo medio de reparación (MTTR), disponibilidad porcentual (ej. 99.99%), pérdida máxima admisible de datos (RPO = 0 seg). |

---

### B. Rendimiento / Performance - Bass et al. Cap. 8
*Capacidad del sistema de cumplir con restricciones de tiempo de respuesta y procesamiento ante la llegada de eventos.*

| Parte | Valores Típicos / Opciones |
| :--- | :--- |
| **Fuente** | Usuarios concurrentes, clientes móviles, sensores IoT, sistemas externos, temporizadores cron. |
| **Estímulo** | Llegada de una petición (periódica, estocástica, en ráfaga / *burst*), ejecución de transacción, consulta masiva. |
| **Artefacto** | Servidor web, motor de búsqueda, base de datos, pipeline de eventos, API Gateway. |
| **Ambiente** | Operación normal, carga pico (*peak load*), condiciones de estrés, operación bajo red de baja latencia/alta latencia. |
| **Respuesta** | Procesar la petición, calcular el resultado, persistir la transacción, encolar el evento. |
| **Medida de Respuesta** | Latencia promedio (ej. $\le 200\text{ ms}$), percentil 95/99 (ej. $p99 \le 1.5\text{ s}$), throughput (ej. $1000\text{ req/seg}$), jitter, consumo de memoria/CPU. |

---

### C. Modificabilidad (Modifiability) - Bass et al. Cap. 7
*Facilidad con la que el sistema puede ser modificado para incorporar cambios, corregir errores o adaptarse a nuevas plataformas.*

| Parte | Valores Típicos / Opciones |
| :--- | :--- |
| **Fuente** | Desarrollador, arquitecto, administrador del sistema, integrador de terceros. |
| **Estímulo** | Solicitud de cambio: agregar una nueva regla de negocio, cambiar un proveedor de base de datos, soportar un nuevo protocolo/API, refactorizar un módulo. |
| **Artefacto** | Código fuente, configuración, esquema de BD, interfaz de usuario, capa de persistencia. |
| **Ambiente** | Tiempo de diseño, tiempo de desarrollo, tiempo de compilación/empaquetado, tiempo de despliegue, tiempo de ejecución. |
| **Respuesta** | Implementar el cambio, verificar que no rompe funcionalidad existente (regresión), desplegar la nueva versión. |
| **Medida de Respuesta** | Costo en horas/días-persona, número de archivos/módulos modificados (acoplamiento), tiempo de compilación y testeo, costo monetario. |

---

### D. Seguridad (Security) - Bass et al. Cap. 9
*Capacidad del sistema para garantizar la confidencialidad, integridad y autenticidad de los datos y servicios, resistiendo ataques y accesos no autorizados.*

| Parte | Valores Típicos / Opciones |
| :--- | :--- |
| **Fuente** | Atacante externo, usuario interno malicioso, botnet, usuario no autenticado, usuario autenticado sin permisos suficientes. |
| **Estímulo** | Intento de inyección SQL, ataque DDoS, intento de acceso a recurso restringido sin credenciales válidas, manipulación de datos en tránsito. |
| **Artefacto** | Módulo de autenticación, base de datos, canal de comunicación, API pública, logs del sistema. |
| **Ambiente** | Operación normal, sistema bajo ataque activo, canal no seguro (red pública). |
| **Respuesta** | Autenticar y autorizar al usuario, rechazar y bloquear la petición maliciosa, registrar en log de auditoría inmutable, cifrar datos en reposo/tránsito, restaurar datos corruptos desde backup. |
| **Medida de Respuesta** | Tiempo para detectar el ataque, tiempo para bloquear la IP atacante ($\le 10\text{ s}$), tiempo de restauración de datos ($\le 1\text{ hora}$), porcentaje de intentos no autorizados bloqueados ($100\%$). |

---

### E. Usabilidad (Usability) - Bass et al. Cap. 12
*Grado en que el sistema permite a los usuarios alcanzar sus objetivos de manera efectiva, eficiente y satisfactoria.*

| Parte | Valores Típicos / Opciones |
| :--- | :--- |
| **Fuente** | Usuario final, nuevo operador, usuario experto, usuario con capacidades reducidas. |
| **Estímulo** | El usuario desea aprender a usar una función, completar una tarea crítica, corregir o deshacer un error, o recibir feedback del estado del sistema. |
| **Artefacto** | Interfaz de usuario (UI), sistema de ayuda, wizard de configuración, diálogo de confirmación/cancelación. |
| **Ambiente** | Primer uso del sistema (onboarding), uso en entorno con distracciones, condiciones de estrés, operación regular. |
| **Respuesta** | Proporcionar feedback visual claro, permitir cancelar la operación sin pérdida de datos, guiar paso a paso, ofrecer autocompletado. |
| **Medida de Respuesta** | Tiempo de aprendizaje (ej. dominar la función en $< 30\text{ minutos}$), tasa de error por tarea ($< 2\%$), tiempo para cancelar/deshacer ($< 1\text{ segundo}$), puntaje SUS (System Usability Scale $\ge 80$). |

---

### F. Interoperabilidad (Interoperability) - Bass et al. Cap. 5
*Capacidad de dos o más sistemas o componentes para intercambiar información y utilizar de manera mutua la información intercambiada.*

| Parte | Valores Típicos / Opciones |
| :--- | :--- |
| **Fuente** | Sistema externo (ej. pasarela de pago Mercado Pago, servidor meteorológico, dispositivos IoT de terceros). |
| **Estímulo** | Solicitud de intercambio de datos o invocación de un servicio a través de una interfaz definida (REST, GraphQL, MQTT, gRPC). |
| **Artefacto** | Módulo conector/adaptador, bus de integración, API Gateway, transformador de datos. |
| **Ambiente** | Tiempo de ejecución, sistemas conocidos previamente o descubiertos dinámicamente. |
| **Respuesta** | Parsear el mensaje, validar el esquema sintáctico/semántico, traducir formatos, responder con los datos solicitados. |
| **Medida de Respuesta** | Porcentaje de mensajes intercambiados y procesados correctamente (ej. $99.99\%$), tiempo de integración de un nuevo proveedor ($\le 2\text{ días-persona}$). |

---

### G. Escalabilidad (Scalability) - Bass et al. Cap. 14 / Slides
*Capacidad del sistema de adaptarse a cambios en la demanda (número de usuarios, volumen de datos, número de dispositivos) agregando o removiendo recursos de hardware sin requerir rediseños significativos.*

| Parte | Valores Típicos / Opciones |
| :--- | :--- |
| **Fuente** | Crecimiento de usuarios registrados, incremento de transacciones concurrentes, adición de nuevos dispositivos IoT / monopatines. |
| **Estímulo** | Aumento de la carga operativa en un factor de $10\times$ o adición de 5.000 nuevos dispositivos. |
| **Artefacto** | Clúster de servicios, particionado de base de datos, balanceador de carga. |
| **Ambiente** | Operación en producción bajo temporada alta o crecimiento sostenido del negocio. |
| **Respuesta** | Auto-escalar horizontalmente agregando instancias de contenedores, distribuir carga sin interrupción. |
| **Medida de Respuesta** | Degradación de latencia inferior al $10\%$, tiempo de provisión de nuevas instancias $< 2\text{ min}$, soporte de hasta $N$ dispositivos con costo de infraestructura lineal. |
