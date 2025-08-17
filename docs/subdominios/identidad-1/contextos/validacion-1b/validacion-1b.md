# Contexto 1B.- Validacion

### Propósito

Permitir a las personas registrarse y validar su identidad en el sistema.


## Alcance

- Registro de usuarios nuevos.
- Validacion de identidad.
- Recuperacion de cuenta.
- Cambio de credenciales.


## Actores y Permisos

- Invitado:
  - Registrarse

- Administrador:
  - Reestablecer credenciales de un Usuario.


## Lenguaje ubicuo

- Usuario:
Persona registrada que tiene asignado un rol y privilegios/responsabilidades.

- Rol:
Propiedad de un Usuario que le asigna privilegios y responsabilidades.

- Credenciales:
Clave de acceso que un Usuario tiene y puede usar para iniciar sesion.


## Agregados

- Usuario:
  - id
  - email
  - sal
  - hash


## Proyecciones

- Usuario:
Excluye: sal, hash


## Politicas

- POL-1B-01:
Dos Usuarios no pueden tener el mismo email.

- POL-1B-02:
Solo Administrador puede reestablecer las credenciales de un Usuario.


## Comandos y Eventos

- Registrarse
  - Eventos   : Registrado, 1A.Registrado, 1C.Entrada
  - Actores   : Invitado
  - Agregados : Usuario
  - Politicas : POL-1B-01

- ReestablecerCredenciales
  - Eventos   : CredencialesReestablecidas, 1A.Editado
  - Actores   : Administrador
  - Agregados : Usuario
  - Politicas : POL-1B-02


## Operaciones

- registrarse
  - Parametros : datos_del_alumno
  - Retorna    : Alumno, Sesion
  - Produce    : Registrado, 1A.Registrado, 1C.Entrada
  - Acceso     : Invitado

- reestablecer_credenciales
  - Parametros : contraseña_nueva
  - Retorna    : Usuario
  - Produce    : CredencialesReestablecidas, 1A.Editado
  - Acceso     : Administrador


## Integraciones

### Downstream

- CredencialesReestablecidas (Usuario)


## Requerimientos Tecnicos

- Hashing y salteo de contraseñas.
- Validacion de direcciones de email mediante 2FA.


## KPIs

- Taza de usuarios validados sobre totales.
