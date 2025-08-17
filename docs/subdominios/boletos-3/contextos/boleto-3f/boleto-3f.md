# COntexto 3F.- Boleto

### Propósito

Permitir a los Alumnos inscribirse a Congresos y generar sus Boletos. Tambien permite a los Organizadores tener una relación de los Alumnos que asistirán a sus Congresos.


## Alcance

- Inscripcion a Congresos (generacion de Boletos).
- Listado de Boletos por Congreso, por Alumno, Usados, Cancelados, etc...
- Cancelacion de Boletos.


## Actores y Permisos

- Alumno:
  - Inscribirse a un Congreso y generar su Boleto.
  - Cancelar sus Boletos.
  - Gestionar sus Boletos.

- Staff:
  - Inscribir Alumnos y generar sus boletos.
  - Cancelar/Restaurar Boletos.

- Organizador:
  - Gestionar los Boletos y consultar estadísticas.


## Lenguaje ubicuo

- Boleto:
Registro que consta la Inscripción de un Alumno en un Congreso y el cual le reserva un lugar del cupo definido para dicho Congreso.

- Inscripcion:
Proceso mediante el cual un Alumno se reserva su lugar en un Congreso y genera su Boleto.

- Cancelado:
Un Boleto que ya no puede ser usado porque fue dado de baja por el Alumno o un Staff o el Organizador del Congreso.

- Restaurado:
Boleto que fue reactivado y ya no esta Cancelado, por lo que puede usarse.

- Media de Evento:
Fotos o videos que fueron tomados en el evento cuando sucedio.


## Agregados

- Boleto:
  - id
  - folio
  - folio_largo
  - fecha_registro
  - alumno_id
  - congreso_id
  - cancelado_p
  - usado_p
  - conferencia_asistencias


## Búsquedas contextuales

- Alumno
  - Contexto: Control de usuarios.
  - Datos: id, no_control, nombre, bloqueado_p,

- Organizador
  - Contexto: Control de usuarios.
  - Datos: id,

- Staff
  - Contexto: Control de usuarios.
  - Datos: id, inscribe_alumnos_p, cancela_boletos_p, consulta_boletos_p,

- Congreso
  - Contexto: Congreso.
  - Datos: id, organizador_id, nombre, fecha_inicio, fecha_fin, direccion,
  publicado_p, cancelado_p, inscripciones_fecha_inicio, inscripciones_fecha_fin,
  cupo, inscritos.


## Proyecciones

- Boleto
  - id
  - folio
  - folio_largo
  - fecha_registro
  - cancelado_p
  - usado_p
  - alumno_id
  - alumno_no_control
  - alumno_nombre
  - congreso_id
  - congreso_nombre
  - congreso_fecha_inicio
  - congreso_fecha_fin
  - congreso_direccion
  - conferencia_asistencias


## Politicas

- POL-3F-01:
Alumnos solo pueden inscribirse a Congresos Publicados y no Cancelados que esten dentro de su periodo de Inscripciones y aun tengan Cupo.

- POL-3F-02:
Organizadores pueden inscribir Alumnos a Congresos aunque estos no esten en periodo de Inscripciones ni tengan Cupo ni esten Publicados, siempre y cuando el Congreso no este Cancelado ni hayan Concluido.

- POL-3F-03:
Alumnos Bloqueados no pueden Inscribirse, ser Inscritos o estar Inscritos a un Congreso.

- POL-3F-04:
Si un Alumno es Eliminado/Bloqueado, se deben Cancelar todos sus Boletos para Congresos Proximos.

- POL-3F-05:
Staff puede Inscribir Alumno a Congreso solo si tiene el Privilegio “staff_inscripciones_p”.

- POL-3F-06:
Staff puede Cancelar/Restaurar Boletos solo si tiene el Privilegio “staff_inscripciones_p”.

- POL-3F-07:
Alumnos pueden Cancelar sus Boletos pero no Restaurarlos, para Restaurarlos requieren ayuda de un Organizador o Staff.

- POL-3F-08:
No se pueden Editar/Cancelar/Restaurar Boletos para Congresos que estan Cancelados o que ya concluyeron.

- POL-3F-09:
Solo se puede generar un Boleto por Alumno para un Congreso.


## Comandos y Eventos

- Inscribirse
  - Eventos   : Inscrito
  - Actores   : Alumno
  - Agregados : Boleto
  - Politicas : POL-3F-01, POL-3F-03, POL-3F-09

- Inscribir
  - Eventos   : Inscrito
  - Actores   : Organizador, Staff
  - Agregados : Boleto
  - Politicas : POL-3F-02, POL-3F-03, POL-3F-05, POL-3F-09

- Cancelar
  - Eventos   : Cancelado
  - Actores   : Organizador, Staff, Alumno
  - Agregados : Boleto
  - Politicas : POL-3F-06, POL-3F-07, POL-3F-08

- Restaurar
  - Eventos   : Restaurado
  - Actores   : Organizador, Staff
  - Agregados : Boleto
  - Politicas : POL-3F-03, POL-3F-06, POL-3F-07, POL-3F-08


## Operaciones

- inscribirse
  - Parametros : id_congreso
  - Retorna    : Boleto
  - Produce    : Inscrito
  - Acceso     : Alumno

- inscribir
  - Parametros : id_alumno, id_congreso
  - Retorna    : Boleto
  - Produce    : Inscrito
  - Acceso     : Organizador, Staff

- cancelar
  - Parametros : id_boleto
  - Retorna    : Boleto
  - Produce    : Cancelado
  - Acceso     : Organizador, Staff, Alumno

- restaurar
  - Parametros : id_boleto
  - Retorna    : Boleto
  - Produce    : Restaurado
  - Acceso     : Organizador, Staff

- marcar
  - Parametros : id_boleto
  - Retorna    : Boleto
  - Desc       :
    La primera vez que se usa para entrar al Congreso se “marca” el Boleto, lo cual
    indica que al menos entro al Congreso.

- estampar
  - Parametros : id_boleto
  - Retorna    : Boleto
  - Desc       :
    La primera vez que se usa para entrar a cada Conferencia especifica se
    le pone una “estampa”, al final se pueden contar las estampas para saber a cuantas
    conferencias se entro con ese Boleto.

- purgar_congreso
  - Parametros : id_congreso
  - Retorna    : [Boleto] (los que si se eliminaron)
  - Produce    : [Eliminado]
  - Desc       :
    Elimina todos los Boletos de un Congreso para los que su Alumno ya no exista.
    Si el Alumno aun existe puede necesitar de su Boleto aunque ya no exista el Congreso
    al que originalmente se Inscribio.

- purgar_alumno
  - Parametros : id_alumno
  - Retorna    : [Boleto] (los que si se eliminaron)
  - Produce    : [Eliminado]
  - Desc       :
    Elimina todos los Boletos de un Alumno para los que su Congreso ya no exista.
    Si el Congreso aun existe el Organizador puede necesitar de los Boletos aunque ya no
    existan los Alumnos al que originalmente se Inscribieron.


## Consultas

- mis_boletos
  - Retorna    : [Boleto]
  - Acceso     : Alumno
  - Desc       :
    Consulta todos los Boletos del Usuario indiscriminadamente, empezando por el creado
    mas recientemente.

- por_congreso
  - Parametros : id_congreso
  - Retorna    : [Boleto]
  - Acceso     : Organizador
  - Desc       :
    Consulta los Boletos de un Congreso indiscriminadamente, empezando por el mas
    reciente.

- por_congreso_no_cancelados
  - Parametros : id_congreso
  - Retorna    : [Boleto]
  - Acceso     : Organizador
  - Desc       :
    Consulta los Boletos no-Cancelados de un Congreso indiscriminadamente, empezando por
    el mas eciente.

- por_folio
  - Parametros : folio
  - Retorna    : [Boleto]
  - Acceso     : Organizador, Staff
  - Desc       :
    Consulta un Boleto especifico, siempre y cuando se tenga permiso.

- por_folio_largo
  - Parametros : folio
  - Retorna    : [Boleto]
  - Acceso     : Invitado
  - Desc       :
    Consulta un Boleto especifico, usando su folio largo para fines de auditoria o
    acceso sin registro. Util para compartir evidencia de un Boleto mediante link (a un
    profesor tal vez).


## Integraciones

### Downstream

- Inscrito   (Boleto)
- Cancelado  (Boleto)
- Restaurado (Boleto)
- Eliminado  (Boleto)

### Upstream

- 1A.AlumnoEliminado
  - Contexto   : Control de usuarios
  - Parametros : Usuario
  - Ejecuta    : purgar_alumno (Usuario.id)

- 2D.Eliminado
  - Contexto   : Congreso
  - Parametros : Congreso
  - Ejecuta    : purgar_congreso (Congreso.id)

- 4G.AsistioCongreso (Asistencia)
  - Contexto   : Asistencia
  - Parametros : Asistencia
  - Ejecuta    : marcar (Asistencia.boleto_id)

- 4G.AsistioConferencia (Asistencia)
  - Contexto   : Asistencia
  - Parametros : Asistencia
  - Ejecuta    : estampar (Asistencia.boleto_id)


## KPIs

- Total historico de Boletos.
- Cantidad de Boletos no cancelados para Congresos Proximos.
- Cantidad de Boletos Cancelados para Congresos Proximos.
- Porcentaje historico de Boletos Cancelados.
- Porcentaje historico de Boletos Usados.
- Promedio historico de Boletos por Alumno.
- Promedio historico de Boletos por Alumno (Usados).
- Promedio historico de Conferencias asistidas por Alumno.
- Promedio historico de Conferencias asistidas por Boleto (Usado).


