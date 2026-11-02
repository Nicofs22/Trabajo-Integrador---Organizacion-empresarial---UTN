# Diccionario de datos inicial

Define las entidades críticas del negocio que soportan el flujo del sistema de información. Cada campo indica su nombre, tipo de dato y una descripción funcional. `PK` indica clave primaria y `FK` clave foránea.

## Entidad 1: CLIENTE

Persona o empresa que contrata el servicio.

| Campo | Tipo de dato | Descripción funcional |
|---|---|---|
| id_cliente | Integer (PK) | Identificador único del cliente |
| nombre_razon_social | String | Nombre completo o razón social |
| cuit_dni | String | Documento o CUIT, necesario para facturar |
| telefono | String | Contacto principal |
| email | String | Correo para enviar presupuestos y planos |

## Entidad 2: PARCELA

Inmueble sobre el que se realiza el trabajo.

| Campo | Tipo de dato | Descripción funcional |
|---|---|---|
| id_parcela | Integer (PK) | Identificador interno de la parcela |
| partida_inmobiliaria | String | Número de partida asignado por catastro |
| nomenclatura_catastral | String | Circunscripción, sección, manzana y parcela |
| partido | String | Partido o municipio donde se ubica |
| superficie_m2 | Decimal | Superficie medida, en metros cuadrados |

## Entidad 3: PERSONAL

Integrantes del estudio.

| Campo | Tipo de dato | Descripción funcional |
|---|---|---|
| id_personal | Integer (PK) | Identificador único del integrante |
| nombre_apellido | String | Nombre completo |
| area | String | Área en la que actúa: técnica, administrativa o comercial |
| titulo | String | Título o función (ingeniero civil, gestora, etc.) |
| matricula | String | Matrícula profesional, si corresponde |
| activo | Boolean | Indica si está actualmente en el estudio |

## Entidad 4: EXPEDIENTE

Cada trabajo encargado al estudio. Vincula a las demás entidades.

| Campo | Tipo de dato | Descripción funcional |
|---|---|---|
| id_expediente | Integer (PK) | Número único de trabajo |
| id_cliente | Integer (FK) | Cliente que encargó el trabajo |
| id_parcela | Integer (FK) | Parcela sobre la que se trabaja |
| id_personal | Integer (FK) | Integrante responsable del trabajo |
| tipo_trabajo | String | Mensura, subdivisión, plano de obra, trámite, etc. |
| fecha_inicio | Date | Fecha de comienzo del trabajo |
| estado | String | En curso, presentado, aprobado o finalizado |
| honorarios | Decimal | Monto total acordado por el trabajo |
| anticipo | Decimal | Monto cobrado al aceptar el presupuesto |
| anticipo_cobrado | Boolean | Indica si el anticipo ya fue recibido |
| saldo_cobrado | Boolean | Indica si el saldo final ya fue cobrado |

## Relaciones entre entidades

- Un cliente puede tener muchos expedientes.
- Una parcela puede aparecer en varios expedientes (por ejemplo, una mensura y luego una subdivisión).
- Cada expediente tiene un integrante del personal como responsable.

Las claves foráneas `id_cliente`, `id_parcela` e `id_personal` de EXPEDIENTE expresan estos vínculos.

## Relación con el proceso BPMN

Las tareas automatizadas del sistema operan sobre estas entidades: registran el cliente y la parcela, abren el expediente asignando un responsable y actualizan su estado.
