# Errores de instalación

Instalé **Odoo 19.0 Community** con el instalador oficial de Windows (`odoo_19.0.latest.exe`, compilación del 04/11/2026) en mi PC. El instalador funcionó sin pararse, pero en el log y al crear la base de datos aparecieron los avisos y errores de la primera tabla.

Antes había hecho una **instalación de prueba desde el código fuente** (Ubuntu 24.04, Python 3.13 y PostgreSQL 16) para practicar. En ella reproduje a propósito los problemas típicos que pide la tarea (segunda tabla).

## Comprobaciones antes y después de instalar

| Comprobación | Cómo la hice | Resultado |
|---|---|---|
| No había otro Odoo ni otro PostgreSQL | Carpeta `C:\Program Files` antes de instalar | No había ninguno, así que dejé marcado el componente PostgreSQL del instalador |
| Odoo responde en el puerto 8069 | `http://localhost:8069` en el navegador | Sale el gestor de bases de datos |
| PostgreSQL responde en el 5432 | `odoo.conf` (`db_port = 5432`, `db_user = odoo`) y creando la base de datos | La base de datos `zumbido` se crea sin errores |
| Log | `C:\Program Files\Odoo 19.0.20261104\server\odoo.log` | Solo los avisos de la tabla siguiente |

## Errores y avisos reales (Windows)

| Error o mensaje | Causa | Solución aplicada | Fuente consultada |
|---|---|---|---|
| `odoo.sql_db: Connection to the database failed` (dos veces en `odoo.log`, nada más instalar) | El instalador arranca el servicio de Odoo **antes** de terminar de descargar e instalar PostgreSQL, así que durante unos segundos no hay base de datos a la que conectarse | Esperar a que el instalador termine y comprobar que el servicio `PostgreSQL_For_Odoo` está en ejecución. Al recargar `localhost:8069` ya conecta | `odoo.log` y el script del instalador, donde la sección de Odoo va antes que la de PostgreSQL ([setup.nsi](https://github.com/odoo/odoo/blob/19.0/setup/win32/setup.nsi)) |
| Al crear la base de datos se instalan solas Proyecto, Empleados, Marketing por correo electrónico y Encuestas | El instalador escribe `default_productivity_apps = True` en `odoo.conf`, y con esa opción Odoo instala esas apps en las bases de datos nuevas | Desinstalarlas desde *Aplicaciones* para quedarme solo con Ventas, Facturación, Inventario y Contactos | `odoo.conf` y el código de [base_install_request](https://github.com/odoo/odoo/blob/19.0/addons/base_install_request/__init__.py) |
| `WARNING ... missing --http-interface/http_interface, using 0.0.0.0 by default, will change to 127.0.0.1 in 20.0` | Sin `http_interface` en `odoo.conf`, Odoo escucha en todas las interfaces de red, no solo en el propio PC | Ninguna: para practicar no molesta. Para que solo se pueda entrar desde el propio PC habría que poner `http_interface = 127.0.0.1` en `odoo.conf` y reiniciar el servicio | `odoo.log` y [opciones de línea de comandos de Odoo](https://www.odoo.com/documentation/19.0/developer/reference/cli.html) |
| *Warning, your Odoo database manager is not protected* al abrir `localhost:8069` | La contraseña maestra seguía siendo la de por defecto | Poner una contraseña maestra propia al crear la base de datos y guardarla fuera del repositorio | Gestor de bases de datos de Odoo |

## Problemas típicos reproducidos (instalación de prueba)

| Error o mensaje | Causa | Solución aplicada | Fuente consultada |
|---|---|---|---|
| `Port 8069 is in use by another program. Either identify and stop that program, or start the server with a different port.` | Lancé un segundo servidor Odoo con el primero ya escuchando en el 8069 | Ver qué programa usa el puerto (`lsof -i :8069`; en Windows `netstat -ano \| findstr :8069` y `taskkill /PID <pid>`) y pararlo, o arrancar con `--http-port=8070` | Mensaje de Odoo y [opciones de línea de comandos](https://www.odoo.com/documentation/19.0/developer/reference/cli.html) |
| Odoo enseña el formulario de **crear base de datos** como si no existiera ninguna; al intentar crearla: `connection to server at "localhost" (127.0.0.1), port 5432 failed: Connection refused` | Paré PostgreSQL: Odoo no puede listar las bases de datos | Volver a arrancar PostgreSQL (en Windows, el servicio `PostgreSQL_For_Odoo` en `services.msc`). Al recargar vuelve a salir la base de datos | [Instalación desde el código fuente](https://www.odoo.com/documentation/19.0/administration/on_premise/source.html) |
| `No es posible instalar el módulo "account_peppol". Hay una dependencia externa sin resolver: phonenumbers` al instalar Ventas | Ventas instala Facturación, que en Odoo 19 depende de Peppol, y este necesita la librería de Python `phonenumbers`, que no estaba en mi entorno virtual | `pip install phonenumbers` y volver a instalar la app. En Windows no pasa porque el instalador ya trae Python con todas las librerías | Mensaje de Odoo y `requirements.txt` de Odoo 19 |
