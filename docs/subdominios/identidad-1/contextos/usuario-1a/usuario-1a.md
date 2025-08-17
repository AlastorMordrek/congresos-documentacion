# Contexto 1A.- Control de Usuarios

## Propósito

Gestionar los usuarios registrados en el sistema, editar sus roles, privilegios y responsabilidades.

## Alcance

- Registro interno de usuarios (Organizador, Staff, Alumno).
- Perfiles de usuarios: informacion personal y de contacto, estudiantil, etc...
- Roles (Administrador, Organizador, Staff, Alumno, Invitado).
- Privilegios y responsabilidades.

## Actores y Permisos

- Usuario:
  - Editar perfil propio.

- Administrador:
  - Crear usuarios nuevos: Organizador, Staff.
  - Eliminar usuarios.

- Organizador:
  - Crear usuarios nuevos: Staff, Alumno.
  - Bloquear/desbloquear usuarios.
  - Editar privilegios y responsabilidades de usuarios: Staff.

## Lenguaje ubicuo

- Usuario:
Persona registrada que tiene asignado un rol y privilegios/responsabilidades.

- Rol:
Propiedad de un Usuario que le asigna privilegios y responsabilidades.
  - 1 Administrador
  - 2 Organizador
  - 3 Staff
  - 4 Alumno
  - 5 Invitado

- Bloqueado:
Estatus de un Usuario que le impide hacer uso del sistema.

- Carrera:
En que carrera esta estudiando un Alumno:
  - ISC: Ingeniaria en sistemas computacionales

- Autorizado:
En Staff: determina si puede realizar acciones de Staff en general.

## Agregados

- Usuario:
  - id
  - email
  - rol
  - id_creador
  - telefono
  - email
  - sal
  - hash
  - bloqueado_p
  - nombre
  - apellido_paterno
  - apellido_materno
  - fecha_nacimiento
  - no_control
  - carrera
  - semestre
  - grupo
  - externo_p
  - curp
  - email_institucional
  - staff_autorizado_p
  - staff_responsabilidades
  - staff_custodio_p
  - staff_alumnos_p
  - staff_inscripciones_p


Politicas

- POL-1A-01:
Algunas funciones requieren que el Usuario EJECUTOR no este bloqueado.

- POL-1A-02:
Algunas funciones requieren que el Usuario RECEPTOR no este bloqueado.

- POL-1A-03:
Todo Usuario debe tener al menos un nombre y al menos uno de sus apellidos.

- POL-1A-04:
  - Administrador puede registrar a cualquier Usuario.
  - Organizador puede registrar a Staff y Alumno.
  - Staff que tenga el privilegio “staff_alumnos_p” puede registrar Alumnos.
  - Invitado puede registrarse como Alumno.

- POL-1A-05:
  - Administrador puede editar a cualquier Usuario.
  - Organizador puede editar a Staff y Alumno.
  - Staff que tenga el privilegio “staff_alumnos_p” puede editar Alumnos.
  - Todo Usuario puede editarse a si mismo.

- POL-1A-06:
  - Administrador puede bloquear/desbloquear a cualquier Usuario.
  - Organizador puede bloquear/desbloquear a Staff y Alumno.
  - Staff que tenga el privilegio “staff_alumnos_p” puede bloquear/desbloquear Alumnos.

- POL-1A-07:
Solo Administrador puede eliminar Usuarios. Puede eliminar a cualquier Usuario.


Comandos y Eventos

- Registrar
  - Eventos   : Registrado
  - Agregados : Usuario
  - Actores   : Administrador, Organizador, Staff
  - Politicas : POL-1A-04

- Editar
  - Eventos   : Editado
  - Agregados : Usuario
  - Actores   : Administrador, Organizador, Staff
  - Politicas : POL-1A-05

- Editarme
  - Eventos   : Editado
  - Agregados : Usuario
  - Actores   : Usuario
  - Politicas : POL-1A-05

- Bloquear
  - Eventos   : Bloqueado, Editado
  - Agregados : Usuario
  - Actores   : Administrador, Organizador, Staff
  - Politicas : POL-1A-06

- Desbloquear
  - Eventos   : Desbloqueado, Editado
  - Agregados : Usuario
  - Actores   : Administrador, Organizador, Staff
  - Politicas : POL-1A-06

- Eliminar
  - Eventos   : Eliminado [, OrganizadorEliminado, AlumnoEliminado]
  - Agregados : Usuario
  - Actores   : Administrador
  - Politicas : POL-1A-07


Operaciones

- crear
  - Parametros : datos
  - Retorna    : Usuario
  - Produce    : Registrado
  - Acceso     :
    - Administrador, Organizador, Staff
    - Invitado se auto registra como Alumno

- editar
  - Parametros : Usuario
  - Retorna    : Usuario
  - Produce    : Editado
  - Acceso     :
    - Usuario     : edita su propio perfil.
    - Admin       : puede editar cualquier usuario.
    - Organizador : puede editar a Staff y Alumnos.
    - Staff       : edita Alumnos.

- bloquear
  - Parametros : id
  - Retorna    : Usuario
  - Produce    : Bloqueado, Editado
  - Acceso     : Administrador, Organizador, Staff

- desbloquear
  - Parametros : id
  - Retorna    : Usuario
  - Produce    : Desbloqueado, Editado
  - Acceso     : Administrador, Organizador, Staff

- eliminar
  - Parametros : id
  - Retorna    : Usuario
  - Produce    : Eliminado
  - Acceso     : Administrador


Consultas

- consultar_por_rol
  - Parametros : rol
  - Retorna    : [Usuario]
  - Acceso     : Administrador, Organizador

- consultar_por_id
  - Parametros : id
  - Retorna    : Usuario
  - Acceso     :
    - Administrador, Organizador, Staff
    - Alumno consulta su propio Usuario


Integraciones

Downstream
- Bloqueado            (Usuario)
- Eliminado            (Usuario)
- OrganizadorEliminado (Usuario)
- AlumnoEliminado      (Usuario)


Requerimientos Tecnicos

- Rate-limiting de peticiones por IP para bloqueo de ataques por fuerza bruta.
- Transito de informacion encriptado incondicionalmente.


KPIs

- Conteo total de usuarios y por rol.
