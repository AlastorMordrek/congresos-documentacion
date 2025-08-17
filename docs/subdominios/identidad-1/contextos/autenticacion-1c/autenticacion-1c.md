# Contexto 1C.- Autenticación

### Propósito

Proveer autenticación segura y sesiones para acceder al sistema con roles.


## Alcance

Inicio, cierre y continuación de sesión.


## Actores y Permisos

- Invitado:
  - Iniciar sesión.

- Usuario:
  - Cerrar sesión.


## Lenguaje Ubicuo

- Sesión:
Comprobante temporal de acceso al sistema.

- Usuario:
Persona registrada que tiene asignado un rol y privilegios/responsabilidades.

- Rol:
Propiedad de un Usuario que le asigna privilegios y responsabilidades.

- Bloqueado:
Usuario que no tiene permitido entrar al sistema.

- Validado:
Usuario que tiene validado su email correctamente.

- Token:
Texto aleatorio criptograficamente-seguro que identifica de manera unica a una Sesion.


## Agregados

- Sesion:
  - id
  - token
  - usuario_id
  - fecha_creacion

- Usuario:
  - id
  - email
  - bloqueado_p
  - sal
  - hash


## Proyecciones

- Usuario:
Excluye: sal, hash


## Politicas

- POL-1C-01:
Implementa: POL-1A-01, POL-1B-02

- POL-1C-02:
Usuarios pueden tener máximo 4 Sesiones simultaneas, al crear una 5ta Sesion, la mas antigua se elimina.

- POL-1C-03:
Usuarios al ser eliminados/bloqueados, pierden todas sus Sesiones.

- POL-1C-04:
Usuarios al recuperar su cuenta, pierden todas sus Sesiones, excepto la que se creo al recuperar.


## Comandos y Eventos

- Entrar
  - Eventos   : Entrada
  - Actor     : Invitado
  - Agregados : Sesion, Usuario
  - Politicas : POL-1C-01, POL-1C-02

- Salir
  - Eventos   : Salida
  - Actor     : Usuario
  - Agregados : Sesion


## Operaciones

- entrar
  - Parametros : email, contraseña
  - Retorna    : Sesion, Usuario
  - Produce    : Entrada [, Salida]
  - Acceso     :
      - Invitado inicia sesion usando sus Credenciales de acceso de Usuario.

- salir
  - Retorna : Sesion
  - Produce : Salida
  - Acceso  :
    - Usuario termina su Sesion actual.

- purgar_usuario
  - Parametros : id_usuario
  - Retorna    : [Sesion]
  - Produce    : [Salida]
  - Acceso     :
    - Administrador: elimina/bloquea a Usuario, lo cual purga sus Sesiones.
    - Organizador: bloquea a Staff/Alumno, lo cual purga sus Sesiones.
    - Staff (con privilegio): bloquea a Alumno, lo cual purga sus Sesiones.


## Integraciones

### Downstream

- Entrada (Sesion)
- Salida  (Sesion)

### Upstream

- 1A.Eliminado
  - Contexto   : Control de Usuarios
  - Parametros : Usuario
  - Ejecuta    : purgar_usuario (Usuario.id)

- 1A.Bloqueado
  - Contexto   : Control de Usuarios
  - Parametros : Usuario
  - Ejecuta    : purgar_usuario (Usuario.id)

- 1B.CredencialesReestablecidas
  - Contexto   : Validacion
  - Parametros : Usuario
  - Ejecuta    : purgar_usuario (Usuario.id)


## Preguntas, decisiones y posibles mejoras

- ADR-1C-01:
Permitir SSO institucional, google, facebook, etc...

- ADR-1C-02:
Permitir a un Usuario ver sus sesiones activas.

- ADR-1C-03:
Permitir a un Usuario eliminar selectivamente sus sesiones activas.

- ADR-1C-04:
Proveer funcion que permita a un Usuario cerrar todas sus sesiones simultaneamente.

- ADR-1C-05:
Proveer funcion que permita a un Usuario cerrar todas sus OTRAS sesiones simultaneamente.
