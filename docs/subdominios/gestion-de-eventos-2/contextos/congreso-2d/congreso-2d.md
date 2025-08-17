2D.- Eventos: Congreso

Propósito

Permitir a los Organizadores planear y gestionar eventos de Congresos.


Alcance

- Registro de Congresos.
- Publicacion de Congresos.
- Asignacion de Organizadores a Congresos.
- Control de fechas de inscripcion.
- Estadisticas de suscripciones.
- Estadisticas de asistencias.


Actores y Permisos

- Invitado:
Consultar los Congresos publicados proximos.

- Organizador:
Gestionar sus Congresos.

- Administrador:
Reasignar Organizadores a Congresos.


Lenguaje ubicuo

- Congreso:
Evento que sucedera en cierto lugar y en el que se expondran talleres, Conferencias, etc...

- Cupo:
Cuantas personas podran asistir a un Congreso.
Este limita el numero maximo de Alumnos que pueden inscribirse a un Congreso por el portal designado.

- Periodo de inscripciones:
Rango de fechas dentro de las cuales estara permitiro para que los Alumnos se inscriban a un Congreso y generen sus Boletos.

- Publicado:
Congreso que oficialmente se llevara a cabo, su informacion esta lista para presentarse al publico general.

- Retractado:
Congreso que ya no se considera listo y ya no esta publicado para el publico general.

- Cancelado:
Un Congreso que ya no se llevara a cabo.

- Restaurado:
Congreso que ya no esta Cancelado, se ha vuelto a poner en marcha.

- Proximo:
Un Congreso que aun no ha sucedido, es decir: su fecha de inicio no ha llegado.

- Concluido:
Congreso que ya sucedio, es decir: su fecha de terminacion ya paso.

- Media:
Fotos o videos que que proveen ilustracion adicional al Congreso.

- Media de Evento:
Fotos o videos que fueron tomados en el evento cuando sucedio.


Agregados

- Congreso:
  id, organizador_id,
  resumen, descripcion,
  fecha_inicio, fecha_fin,
  direccion,
  publicado_p, cancelado_p,

  inscripciones_fecha_inicio, inscripciones_fecha_fin,
  cupo, inscritos, asistencias,

  staff_cantidad, staff_requerimientos,

  media_1, media_2, media_3, media_4, media_5, media_6,
  media_evt_1, media_evt_2, media_evt_3, media_evt_4, media_evt_5, media_evt_6


Politicas

- POL-2D-01:
Solo Organizadores pueden crear Congresos, y estos son asignados al Congreso que crearon.

- POL-2D-02:
Solo Organizadores pueden editar/eliminar Congresos y solo aquellos a los que estan asignados.

- POL-2D-03:
Administradores pueden reasignar un Congreso a otro Organizador.


Comandos y Eventos

- Crear
  - Eventos   : Creado
  - Actores   : Organizador
  - Agregados : Congreso
  - Politicas : POL-2D-01

- Editar
  - Eventos   : Editado
  - Actores   : Organizador
  - Agregados : Congreso
  - Politicas : POL-2D-02

- Eliminar
  - Eventos   : Eliminado
  - Actores   : Organizador
  - Agregados : Congreso
  - Politicas : POL-2D-02

- Publicar
  - Eventos   : Publicado, Editado
  - Actores   : Organizador
  - Agregados : Congreso
  - Politicas : POL-2D-02

- Retractar
  - Eventos   : Retractado, Editado
  - Actores   : Organizador
  - Agregados : Congreso
  - Politicas : POL-2D-02

- Cancelar
  - Eventos   : Cancelado, Editado
  - Actores   : Organizador
  - Agregados : Congreso
  - Politicas : POL-2D-02

- Restaurar
  - Eventos   : Restaurado, Editado
  - Actores   : Organizador
  - Agregados : Congreso
  - Politicas : POL-2D-02

- Asignar
  - Eventos   : Asignado, [Soltado], Editado
  - Actores   : Administrador
  - Agregados : Congreso
  - Politicas : POL-2D-03


Operaciones

- crear
  - Parametros : datos_del_congreso
  - Retorna    : Congreso
  - Produce    : Creado
  - Acceso     : Organizador

- editar
  - Parametros : Congreso
  - Retorna    : Congreso
  - Produce    : Editado
  - Acceso     : Organizador

- eliminar
  - Parametros : id
  - Retorna    : Congreso
  - Produce    : Eliminado
  - Acceso     : Organizador

- publicar
  - Parametros : id
  - Retorna    : Congreso
  - Produce    : Publicado, Editado
  - Acceso     : Organizador

- retractar
  - Parametros : id
  - Retorna    : Congreso
  - Produce    : Retractado, Editado
  - Acceso     : Organizador

- cancelar
  - Parametros : id
  - Retorna    : Congreso
  - Produce    : Cancelado, Editado
  - Acceso     : Organizador

- restaurar
  - Parametros : id
  - Retorna    : Congreso
  - Produce    : Restaurado, Editado
  - Acceso     : Organizador

- soltar
  - Parametros : id_congreso
  - Retorna    : Congreso
  - Produce    : Soltado, Editado
  - Acceso     : Administrador
  - Desc       : Remueve de un Congreso el Organizador que tiene asignado.

- asignar
  - Parametros : id_congreso, id_organizador
  - Retorna    : Congreso
  - Produce    : Asignado, Soltado, Editado
  - Acceso     : Administrador
  - Desc       : Remueve de un Congreso el Organizador que tiene asignado.

- contar_asistencia
  - Parametros : id_congreso
  - Retorna    : [Congreso]
  - Produce    : [Editado]
  - Desc       : Incrementa el contador de asistencias al Congreso.

- purgar_organizador
  - Parametros : id_usuario
  - Retorna    : [Congreso]
  - Produce    : [Soltado], [Editado]
  - Desc       : Remueve un Organizador de todos los Congresos a los que esta asignado.

- asistencia
  - Parametros : id_congreso
  - Retorna    : Congreso
  - Produce    : Editado
  - Desc       : Aumenta el contador de Asistencias a un Congreso.


Consultas

- todos
  - Retorna : [Congreso]
  - Acceso  : Administrador
  - Desc    : Consulta todos los Congresos indiscriminadamente.

- por_organizador
  - Retorna : [Congreso]
  - Acceso  : Administrador
  - Desc    : Consulta los Congresos de un Organizador.

- mios
  - Retorna : [Congreso]
  - Acceso  : Organizador
  - Desc    : Permite a un Organizador consultar sus Congresos.

- destacados
  - Retorna : [Congreso]
  - Acceso  : Invitado
  - Desc    : Consulta los Congresos publicados proximos o que sucedieron recientemente.

- por_id
  - Parametros : id
  - Retorna    : [Congreso]
  - Acceso     :
    - Invitado, Alumno                  : Consulta un Congreso publicado.
    - Staff, Organizador, Administrador : Consulta un Congreso.


Integraciones

Downstream

- Creado     (Congreso)
- Editado    (Congreso)
- Eliminado  (Congreso)
- Publicado  (Congreso)
- Retractado (Congreso)
- Cancelado  (Congreso)
- Restaurado (Congreso)
- Asignado   (Congreso)
- Soltado    (Congreso)

Upstream

- 1A.OrganizadorEliminado
  - Contexto   : Control de Usuarios
  - Parametros : Usuario
  - Ejecuta    : purgar_organizador (Usuario.id)

- 4G.AsistioCongreso
  - Contexto   : Asistencia
  - Parametros : Asistencia
  - Ejecuta    : asistencia (Asistencia.congreso_id)


KPIs

- Cuantos Alumnos se inscribieron a un Congreso.
- Porcentaje de cupo ocupado por inscripciones.
- Porcentaje de cupo que se consolido en asistencias.

