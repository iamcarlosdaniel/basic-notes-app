# Aplicación básica de notas

Este repositorio contiene la `Especificación de Requerimientos de Software (SRS)` para una aplicación básica de notas. El objetivo de este documento es establecer una base sólida de reglas de negocio y atributos de calidad, mediante la definición detallada de requerimientos funcionales y no funcionales.

Este diseño ha sido concebido bajo un enfoque agnóstico a la tecnología, garantizando que las definiciones funcionales sean completamente independientes del stack tecnológico, el motor de persistencia o el modelo de despliegue seleccionado para su implementación.

> [!NOTE]
> This document is also available in its `english version` at [docs/en/README.md](/docs/en/README.md)

> [!IMPORTANT]
> El contenido de este repositorio se distribuye bajo la `Licencia Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)`.
> Todos los términos se rigen por lo establecido en el archivo [LICENSE](/LICENSE).

## Índice

- [Requerimientos funcionales](#requerimientos-funcionales)
  - [Módulo de autenticación](#módulo-de-autenticación)
    - [RF-AU-01: Registro de identidad](#rf-auth-01-registro-de-identidad)
    - [RF-AU-02: Inicio de sesión](#rf-auth-02-inicio-de-sesión)
    - [RF-AU-03: Cierre de sesión](#rf-auth-03-cierre-de-sesión)
    - [RF-AU-04: Recuperar contraseña](#rf-auth-04-recuperar-contraseña)
    - [RF-AU-05: Restaurar contraseña](#rf-auth-05-restaurar-contraseña)
  - [Módulo de sesiones](#módulo-de-sesiones)
    - [RF-SE-01: Obtener todas las sesiones](#rf-se-01-obtener-todas-las-sesiones-del-usuario)
    - [RF-SE-02: Obtener la información una sesion](#rf-se-02-obtener-la-información-de-una-sesion)
    - [RF-SE-03: Cerrar sesión desde otro dispositivo](#rf-se-03-cerrar-sesión-desde-otro-dispositivo)
  - [Módulo de usuarios](#módulo-de-usuarios)
    - [RF-US-01: Obtener perfil del usuario](#rf-us-01-obtener-perfil-del-usuario)
    - [RF-US-02: Editar perfil del usuario](#rf-us-02-editar-perfil-del-usuario)
  - [Módulo de notas](#módulo-de-notas)
    - [RF-NT-01: Obtener todas las notas]()
    - [RF-NT-02: Obtener el contenido de una nota]()
    - [RF-NT-03: Crear una nota]()
    - [RF-NT-04: Editar el contenido de una nota]()
    - [RF-NT-05: Eliminar una nota]()
  - [Módulo de etiquetas](#módulo-de-etiquetas)
    - [RF-ET-01: Obtener todas las etiquetas]()
    - [RF-ET-02: Obtener la información de una etiqueta]()
    - [RF-ET-03: Crear una etiqueta]()
    - [RF-RT-04: Editar la información de una etiqueta]()
    - [RF-RT-05: Eliminar una etiqueta]()

- [Requerimientos no funcionales]()
  - [Seguridad]()
  - [Rendimiento]()
  - [Confiabilidad]()
  - [Disponibilidad]()
  - [Escalabilidad]()
  - [Usabilidad]()
  - [Compatibilidad]()
  - [Mantenibilidad]()
  - [Portabilidad]()

## Requerimientos funcionales

### Módulo de autenticación

#### RF-AUTH-01: Registro de identidad

**Descripción**

El sistema deberá proveer un mecanismo para el alta de nuevos usuarios en la plataforma mediante la captura de un identificador único basado en correo electrónico y una credencial de acceso segura.

**Criterios de aceptación**

- El correo electrónico debe funcionar como identificador unívoco y el sistema debe rechazar registros duplicados.

- El correo electrónico debe ser validado mediante el envío y verificación de un código de un solo uso.

- Las contraseñas deberán cumplir con criterios de complejidad mínima antes de ser aceptadas por el sistema.

- Las contraseñas deben ser procesadas mediante algoritmos de hashing seguro con sal antes de su persistencia en la base de datos.

---

#### RF-AUTH-02: Inicio de sesión

**Descripción**

El sistema deberá proveer un mecanismo para la validación de identidad de usuarios registrados, permitiendo el acceso a las funcionalidades privadas mediante la comprobación de sus credenciales (correo electrónico y contraseña).

**Criterios de aceptación**

- El sistema debe validar las credenciales ingresadas contra los registros de la capa de persistencia y emitir un comprobante de acceso seguro en caso de éxito.

- El comprobante de acceso generado debe viajar de forma cifrada y poseer un tiempo de expiración definido para garantizar la integridad de la sesión.

- El sistema debe notificar al usuario mediante correo electrónico únicamente cuando se realice un nuevo inicio de sesión.

---

#### RF-AUTH-03: Cierre de sesión

**Descripción**

El sistema deberá proveer un mecanismo para la terminación de la sesión activa del usuario, garantizando la revocación de su acceso a las funcionalidades privadas.

**Criterios de aceptación**

- El sistema debe invalidar el comprobante de acceso actual de forma que no pueda ser reutilizado para futuras peticiones.

- El sistema debe asegurar que el acceso a los recursos protegidos sea denegado inmediatamente después de confirmada la salida.

- El sistema debe redirigir al usuario hacia una interfaz pública o de inicio de sesión tras completar el proceso de cierre con éxito.

---

#### RF-AUTH-04: Recuperar contraseña

**Descripción**

El sistema deberá proveer un mecanismo seguro para que el usuario pueda restablecer su credencial de acceso en caso de olvido o pérdida, utilizando su correo electrónico como canal de verificación.

**Criterios de aceptación**

- El sistema debe validar la existencia del correo electrónico antes de iniciar el proceso, emitiendo una respuesta que no confirme ni niegue la existencia de la cuenta por motivos de seguridad.

- El sistema debe generar y enviar un enlace o código de recuperación temporal con un tiempo de expiración limitado para garantizar la integridad del proceso.

---

#### RF-AUTH-05: Restaurar contraseña

**Descripción**

El sistema deberá permitir al usuario establecer una nueva credencial de acceso tras haber completado con éxito el proceso de verificación de identidad por recuperación.

**Criterios de aceptación**

- El sistema debe validar que el código o enlace de recuperación utilizado sea vigente y no haya sido revocado previamente.

- La nueva contraseña proporcionada debe cumplir con las mismas políticas de robustez y procesamiento de hashing con sal definidas en el requerimiento [RF-AUTH-01: Registro de identidad](#rf-auth-01-registro-de-identidad).

- El sistema debe invalidar automáticamente el comprobante de recuperación tras la actualización exitosa de la contraseña para evitar su reutilización.

> [!TIP]
> [Regresar al índice](#índice)

---

### Módulo de sesiones

#### RF-SE-01: Obtener todas las sesiones

**Descripción**

El sistema deberá proveer un mecanismo para listar todos los comprobantes de acceso activos asociados a la cuenta del usuario, permitiendo visualizar la actividad de su identidad en diferentes dispositivos o navegadores.

**Criterios de aceptación**

- El sistema debe consultar la capa de persistencia para recuperar únicamente los registros de sesiones que no hayan expirado o hayan sido revocadas.

- La información retornada debe incluir metadatos básicos como el identificador de la sesión, la fecha de última actividad y el origen aproximado de la conexión.

- El sistema debe permitir la actualización en tiempo real de esta lista cada vez que se cree o invalide un nuevo comprobante de acceso.

---

#### RF-SE-02: Obtener la información de una sesion

**Descripción**

El sistema deberá proveer un mecanismo para recuperar la información detallada de un comprobante de acceso específico asociado a la cuenta del usuario, permitiendo verificar los atributos técnicos y el estado actual de dicha sesión.

**Criterios de aceptación**

- El sistema debe validar que el comprobante de acceso consultado pertenezca efectivamente al usuario autenticado, denegando el acceso en caso de falta de autorización o propiedad.

- La información retornada debe detallar atributos técnicos adicionales, tales como la dirección IP de origen, el agente de usuario (User-Agent) y el tiempo restante para la expiración del comprobante.

---

#### RF-SE-03: Cerrar sesión desde otro dispositivo

**Descripción**

El sistema deberá permitir al usuario revocar de forma selectiva cualquier comprobante de acceso activo, finalizando la sesión en dispositivos remotos sin afectar su conexión actual.

**Criterios de aceptación**

- Se aplican los mismo criterios de aceptación definidos en el requerimiento [RF-AUTH-03: Cierre de sesión](#rf-auth-03-cierre-de-sesión).

- Tras la revocación, el dispositivo afectado deberá ser redirigido a la interfaz de inicio de sesión en su próxima interacción con un recurso protegido.

---

### Módulo de usuarios

#### RF-US-01: Obtener perfil del usuario

**Descripción**

El sistema deberá proveer un mecanismo para que el usuario autenticado pueda recuperar y visualizar la información almacenada en su perfil, permitiendo la consulta de sus datos personales.

**Criterios de aceptación**

El sistema debe verificar la validez del comprobante de acceso antes de procesar la solicitud de información del perfil.

El sistema debe retornar únicamente los datos autorizados, garantizando la privacidad de información sensible (como contraseñas o hashes de seguridad).

El sistema debe permitir la consulta de atributos como nombre completo, correo electrónico vinculado, fecha de creación de la cuenta y configuración de preferencias generales.

---

#### RF-US-02: Editar perfil del usuario

**Descripción**

**Criterios de aceptación**

---

### Módulo de notas

**Descripción**

**Criterios de aceptación**

---

### Módulo de etiquetas

**Descripción**

**Criterios de aceptación**

---
