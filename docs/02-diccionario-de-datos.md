# Diccionario de datos inicial

Define las entidades críticas del negocio que soportan el flujo del sistema de información. Cada campo indica su nombre, tipo de dato y una descripción funcional. `PK` indica clave primaria y `FK` clave foránea.

Los atributos están **atomizados**: cada campo guarda un único dato (por ejemplo, el DNI y el CUIT van en campos distintos, y la dirección se divide en calle, número, localidad y partido). Esto facilita las consultas, evita ambigüedades y permite completar formularios oficiales sin tener que separar datos a mano.

## Entidad 1: CLIENTE

Persona física o jurídica que contrata el servicio.

| Campo | Tipo de dato | Descripción funcional |
|---|---|---|
| id_cliente | Integer (PK) | Identificador único e inequívoco del cliente. Es la clave que usa EXPEDIENTE para referenciarlo |
| tipo_persona | String | Física o jurídica. Determina si se completan nombre y apellido o razón social |
| nombre | String | Nombre de pila (personas físicas) |
| apellido | String | Apellido (personas físicas) |
| razon_social | String | Denominación legal (personas jurídicas) |
| dni | String | Documento nacional de identidad. Identifica a personas físicas |
| cuit | String | Clave Única de Identificación Tributaria. Se usa para facturar |
| telefono | String | Teléfono de contacto principal |
| email | String | Correo para enviar presupuestos y planos |
| calle | String | Nombre de la calle del domicilio |
| numero | Integer | Altura del domicilio |
| localidad | String | Localidad del domicilio |
| partido | String | Partido o municipio del domicilio |

DNI y CUIT se guardan como `String` para conservar ceros iniciales y formato, ya que no se usan en cálculos. Debe completarse al menos uno de los dos.

## Entidad 2: PARCELA

Inmueble sobre el que se realiza el trabajo.

| Campo | Tipo de dato | Descripción funcional |
|---|---|---|
| id_parcela | Integer (PK) | Identificador interno de la parcela |
| partida_inmobiliaria | String | Número de partida asignado por catastro |
| circunscripcion | String | Circunscripción de la nomenclatura catastral |
| seccion | String | Sección de la nomenclatura catastral |
| manzana | String | Manzana de la nomenclatura catastral |
| nro_parcela | String | Número de parcela de la nomenclatura catastral |
| calle | String | Calle donde se ubica el inmueble |
| numero | Integer | Altura del inmueble |
| localidad | String | Localidad donde se ubica |
| partido | String | Partido o municipio donde se ubica |
| superficie_m2 | Decimal | Superficie medida, en metros cuadrados |

## Entidad 3: PERSONAL

Integrantes del estudio.

| Campo | Tipo de dato | Descripción funcional |
|---|---|---|
| id_personal | Integer (PK) | Identificador único del integrante |
| nombre | String | Nombre de pila |
| apellido | String | Apellido |
| area | String | Área en la que actúa: técnica, administrativa o comercial |
| titulo | String | Título o función (ingeniero civil, gestora, etc.) |
| matricula | String | Matrícula profesional, si corresponde |
| activo | Boolean | Indica si está actualmente en el estudio |

## Entidad 4: EXPEDIENTE

Cada trabajo encargado al estudio. Vincula a las demás entidades.

| Campo | Tipo de dato | Descripción funcional |
|---|---|---|
| id_expediente | Integer (PK) | Número único de trabajo |
| id_cliente | Integer (FK) | Cliente que encargó el trabajo. Referencia a CLIENTE |
| id_parcela | Integer (FK) | Parcela sobre la que se trabaja. Referencia a PARCELA |
| id_personal | Integer (FK) | Integrante responsable del trabajo. Referencia a PERSONAL |
| tipo_trabajo | String | Mensura, subdivisión, plano de obra, trámite, etc. |
| fecha_inicio | Date | Fecha de comienzo del trabajo |
| estado | String | En curso, presentado, aprobado o finalizado |
| honorarios | Decimal | Monto total acordado por el trabajo |
| anticipo | Decimal | Monto cobrado al aceptar el presupuesto |
| anticipo_cobrado | Boolean | Indica si el anticipo ya fue recibido |
| saldo_cobrado | Boolean | Indica si el saldo final ya fue cobrado |

## Claves y relaciones entre entidades

| Entidad | Clave primaria | Claves foráneas |
|---|---|---|
| CLIENTE | id_cliente | — |
| PARCELA | id_parcela | — |
| PERSONAL | id_personal | — |
| EXPEDIENTE | id_expediente | id_cliente → CLIENTE, id_parcela → PARCELA, id_personal → PERSONAL |

| Relación | Cardinalidad | Lectura |
|---|---|---|
| CLIENTE — EXPEDIENTE | 1 a N | Un cliente puede tener muchos expedientes; cada expediente pertenece a un solo cliente |
| PARCELA — EXPEDIENTE | 1 a N | Una parcela puede aparecer en varios expedientes (por ejemplo, una mensura y luego una subdivisión) |
| PERSONAL — EXPEDIENTE | 1 a N | Un integrante puede ser responsable de muchos expedientes; cada expediente tiene un solo responsable |

Las claves foráneas aseguran la integridad referencial: no puede existir un expediente sin cliente, sin parcela y sin responsable asignado.

## Relación con el proceso BPMN

Las tareas automatizadas del sistema operan sobre estas entidades: registran el cliente y la parcela, abren el expediente asignando un responsable y actualizan su estado.
