# Contexto 2E.- Conferencia

### Propósito

Permitir a los Organizadores planear y gestionar las Conferencias que sucederan en un Congreso.


## Alcance

- Registro de Conferencias, sus requerimientos y detalles.
- Descripcion de los conferencistas que daran cada Conferencia.
- Estadisaticas de asistencias.


## Actores y Permisos

- Invitado:
Consultar las Conferencias publicadas que se daran en un Congreso.

- Organizador:
Gestionar las Conferencias de sus Congresos.


## Lenguaje ubicuo

- Conferencia:
Un evento de tipo Clase, Taller, Conferencia, que sucedera en cierto Congreso, en cierta fecha y ubicacion.

- Cupo:
Espacio disponible en la sala (lugar) donde se llevara a cabo la Conferencia. Este no es un limite duro, el Staff puede permitir entrar mas gente si lo consideran prudente.

- Conferencista:
Persona que estara a cargo de exponer una conferencia.

- Semblanza:
Datos del perfil de el Conferencista, su CV, certificaciones, etc...

- Publicada:
Conferencia que oficialmente se llevara a cabo, su informacion esta lista para presentarse al publico general.

- Retractada:
Conferencia que ya no se considera lista y ya no esta publicada para el publico general.

- Cancelada:
Una Conferencia que ya no se llevara a cabo.

- Restaurada:
Conferencia que ya no esta Cancelada, se ha vuelto a poner en marcha.

- Media:
Fotos o videos que que proveen ilustracion adicional a la Conferencia.

- Media de Evento:
Fotos o videos que fueron tomados en el evento cuando sucedio.


## Agregados

- Conferencia:
  - id
  - congreso_id
  - titulo
  - resumen
  - descripcion
  - fecha
  - sala
  - cupo
  - asistencias
  - publicada_p
  - cancelada_p
  - conferencista_nombre
  - conferencista_email
  - conferencista_telefono
  - conferencista_semblanza
  - conferencista_foto
  - conferencista_banner
  - conferencista_logo_empresa
  - staff_cantidad
  - staff_requerimientos
  - media_1
  - media_2
  - media_3
  - media_4
  - media_5
  - media_6
  - media_evt_1
  - media_evt_2
  - media_evt_3
  - media_evt_4
  - media_evt_5
  - media_evt_6


## Politicas

- POL-2E-01:
Solo Organizadores pueden crear Conferencias, y solo dentro de los Congresos a los que estan asignados.

- POL-2E-02:
Solo Organizadores pueden editar/eliminar Conferencias y solo aquellas que esten dentro de Congresos a los que esten asignados.

- POL-3F-01:
Alumnos solo pueden inscribirse a Congresos publicados y no cancelados que esten dentro de su periodo de inscripciones y aun tengan cupo.

- POL-3F-02:
Organizadores pueden inscribir Alumnos a Congresos aunque estos no esten en periodo de inscripciones ni tengan cupo ni esten publicados, siempre y cuando el Congreso no este Cancelado.

- POL-3F-03:
Alumnos bloqueados no pueden inscribirse, ser inscritos o estar inscritos a un Congreso.


## Comandos y Eventos

- Crear
  - Eventos   : Creada
  - Actores   : Organizador
  - Agregados : Conferencia
  - Politicas : POL-2E-01

- Editar
  - Eventos   : Editada
  - Actores   : Organizador
  - Agregados : Conferencia
  - Politicas : POL-2E-02

- Eliminar
  - Eventos   : Eliminada
  - Actores   : Organizador
  - Agregados : Conferencia
  - Politicas : POL-2E-02

- Publicar
  - Eventos   : Publicada, Editada
  - Actores   : Organizador
  - Agregados : Conferencia
  - Politicas : POL-2E-02

- Retractar
  - Eventos   : Retractada, Editada
  - Actores   : Organizador
  - Agregados : Conferencia
  - Politicas : POL-2E-02

- Cancelar
  - Eventos   : Cancelada, Editada
  - Actores   : Organizador
  - Agregados : Conferencia
  - Politicas : POL-2E-02

- Restaurar
  - Eventos   : Restaurada, Editada
  - Actores   : Organizador
  - Agregados : Conferencia
  - Politicas : POL-2E-02


## Operaciones

- crear
  - Parametros : datos_de_la_conferencia
  - Retorna    : Conferencia
  - Produce    : Creada
  - Acceso     : Organizador

- editar
  - Parametros : Conferencia
  - Retorna    : Conferencia
  - Produce    : Editada
  - Acceso     : Organizador

- eliminar
  - Parametros : id
  - Retorna    : Conferencia
  - Produce    : Eliminada
  - Acceso     : Organizador

- publicar
  - Parametros : id
  - Retorna    : Conferencia
  - Produce    : Publicada, Editada
  - Acceso     : Organizador

- retractar
  - Parametros : id
  - Retorna    : Conferencia
  - Produce    : Retractada, Editada
  - Acceso     : Organizador

- cancelar
  - Parametros : id
  - Retorna    : Conferencia
  - Produce    : Cancelada, Editada
  - Acceso     : Organizador

- restaurar
  - Parametros : id
  - Retorna    : Conferencia
  - Produce    : Restaurada, Editada
  - Acceso     : Organizador

- purgar_congreso
  - Parametros : id_congreso
  - Retorna    : [Conferencia]
  - Produce    : [Eliminada]
  - Desc       : Elimina todas las Conferencias de un Congreso.

- asistencia
  - Parametros : id_conferencia
  - Retorna    : Conferencia
  - Produce    : Editada
  - Desc       : Aumenta el contador de Asistencias a una Conferencia.


## Integraciones

### Downstream

- Creada     (Conferencia)
- Editada    (Conferencia)
- Eliminada  (Conferencia)
- Publicada  (Conferencia)
- Retractada (Conferencia)
- Cancelada  (Conferencia)
- Restaurada (Conferencia)

### Upstream

- 2D.Eliminado
  - Contexto   : Congreso
  - Parametros : Congreso
  - Ejecuta    : purgar_congreso (Congreso.id)

4G.AsistioConferencia (Asistencia)
- Contexto   : Asistencia
- Parametros : Asistencia
- Ejecuta    : asistencia (Asistencia.conferencia_id)


## KPIs

- Cuantos Alumnos asistieron a una Conferencia.
- Porcentaje de cupo que se consolido en asistencias.
