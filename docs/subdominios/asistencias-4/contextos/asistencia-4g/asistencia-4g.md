# Contexto 4G.- Asistencia

### Propósito

Permitir a los Organizadores y Staff controlar la entrada a los Congresos y Conferencias y ademas llevar una lista de Asistencias para poder tener estadisticas y metricas de eventos y para que los Alumnos puedan demostrar que si asistieron a los eventos.


## Alcance

- Validar el acceso a un Congreso a travez de un Boleto.
- Marcar Boletos como que ya fueron usados para entrar a un Congreso.
- Estampar Boletos cada que entren a una nueva Conferencia.
- Llevar una lista de Asistencias de Alumnos a Congresos y Conferencias.


## Actores y Permisos

- Alumno:
  - Consultar su lista de asistencias a Congresos y Conferencias.

- Staff:
  - Validar acceso de un Alumno a un Congreso/Conferencia a travez de un Boleto.
  - Registrar Asistencia de un Alumno a un Congreso/Conferencia a travez de un Boleto.

- Organizador:
  - Mismas que el Staff.
  - Consultar listados y estadisticas de Asistencias.

- Invitado:
  - Consultar listado de Asistencias de un Alumno a travez del folio_largo de su Boleto.


## Lenguaje ubicuo

- Asistencia:
Registro que consta la entrada de un Alumno a un Congreso a travez de su Boleto.


## Agregados

- Asistencia:
  - id,
  - congreso_id
  - conferencia_id
  - alumno_id
  - alumno_no_control
  - boleto_id
  - boleto_folio
  - staff_id
  - fecha_creacion


## Búsquedas contextuales

- Alumno
  - Contexto: Control de usuarios.
  - Datos: id, no_control, nombre, bloqueado_p,

- Organizador
  - Contexto: Control de usuarios.
  - Datos: id,

- Staff
  - Contexto: Control de usuarios.
  - Datos: id, nombre, staff_custodio_p,

- Congreso
  - Contexto: Congreso.
  - Datos: id, organizador_id, fecha_inicio, fecha_fin, cancelado_p.

- Conferencia
  - Contexto: Conferencia.
  - Datos: id, nombre, cancelada_p.

- Boleto
  - Contexto: Boleto.
  - Datos: id, folio, folio_largo, cancelado_p.


## Proyecciones

- Asistencia:
  - id,
  - congreso_id
  - conferencia_id
  - conferencia_nombre
  - alumno_id
  - alumno_no_control
  - alumno_nombre
  - boleto_id
  - boleto_folio
  - staff_id
  - staff_nombre
  - fecha_creacion


## Politicas

- POL-4G-01:
  - Organizador solo puede registrar Asistencias a Congresos y sus Conferencias para Congresos a los que este asignado.
  - Staff puede registrar Asistencias a Congresos/Conferencias si tiene el privilegio “staff_custodio_p”.

- POL-4G-02:
Solo puede registrarse Asistencias si el Boleto en cuestion no esta cancelado.

- POL-4G-03:
Solo puede registrarse Asistencias si el Congreso en cuestion no esta cancelado y se encuentra dentro de su rango de fechas establecido.

- POL-4G-04:
Solo puede registrarse Asistencias si la Conferencia en cuestion no esta cancelada.

- POL-4G-05:
Solo puede registrarse Asistencias si el Alumno del Boleto no esta bloqueado.


## Comandos y Eventos

- AsistirCongreso
  - Eventos   : AsistioCongreso
  - Actores   : Organizador, Staff
  - Agregados : Asistencia
  - Politicas : POL-4G-01, POL-4G-02, POL-4G-03, POL-4G-05

- AsistirConferencia
  - Eventos   : AsistioConferencia
  - Actores   : Organizador, Staff
  - Agregados : Asistencia
  - Politicas : POL-4G-01, POL-4G-02, POL-4G-03, POL-4G-04, POL-4G-05


## Operaciones

- asistir_congreso
  - Parametros : id_congreso, folio_boleto
  - Retorna    : Asistencia
  - Produce    : AsistioCongreso
  - Acceso     : Organizador, Staff

- asistir_conferencia
  - Parametros : id_conferencia, folio_boleto
  - Retorna    : Asistencia
  - Produce    : AsistioConferencia
  - Acceso     : Organizador, Staff

- purgar_conferencia
  - Parametros : id_conferencia
  - Desc       :
    Elimina todas las Asistencias de una Conferencia para los que su Alumno ya no exista.
    Si el Alumno aun existe puede necesitar de su Asistencia aunque ya no exista la
    Conferencia a la que asistio.

- purgar_alumno
  - Parametros : id_alumno
  - Desc       :
    Elimina todas las Asistencias de un Alumno para los que su Conferencia
    ya no exista.
    Si la Conferencia aun existe, el Organizador puede necesitar de esa
    Asistencia aunque ya no exista el Alumno que asistio.


## Consultas

- por_conferencia
  - Parametros : id_conferencia
  - Retorna    : [Asistencia]
  - Acceso     : Organizador, Staff
  - Desc       :
    Consulta las Asistencias de una Conferencia, empezando por la mas reciente.

- por_folio_largo
  - Parametros : folio
  - Retorna    : [Asistencia]
  - Acceso     : Invitado
  - Desc       :
    Consulta un Boleto especifico, usando su folio largo para fines de auditoria o
    acceso sin registro.
    Util para compartir constancia de Asistencias mediante link (a un profesor tal vez).


## Integraciones

### Downstream

- AsistioCongreso    (Asistencia)
- AsistioConferencia (Asistencia)

### Upstream

- 1A.AlumnoEliminado
  - Contexto   : Control de usuarios
  - Parametros : Usuario
  - Ejecuta    : purgar_alumno (Usuario.id)

- 2E.Eliminada
  - Contexto   : Conferencia
  - Parametros : Conferencia
  - Ejecuta    : purgar_conferencia (Conferencia.id)


## KPIs

- Total de Asistencias historico.


