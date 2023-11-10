
# Cliente para el servicio de autenticación

Implementación de una librería en Python para el acceso a la API REST implementada en [auth_service](https://github.com/ptobiasdiaz/auth_service). Además también se incluye un cliente en formato _shell_ que mediante la librería, permite el uso de dicha API.

## Instalación
Descargar el repositorio:
```
git clone https://github.com/ptobiasdiaz/auth_client.git
```
Crear y activar un entorno virtual de python:
```
cd auth_client
python3 -m venv .venv
source .venv/bin/activate
```
Instalar dependencias:
```
pip install -r requirements.txt
```
## Ejecución
Ahora se puede ejecutar directamente el cliente:
```
./auth_cli
```
## Uso básico
Inicialmente el cliente no está conectado a ningún servicio, para ello hay que utilizar una serie de comandos dentro de la shell (que dispone de un comando _help_ para solicitar ayuda). Pero puede iniciarse el cliente de manera que se conecte inicialmente a un servicio dado, para ello se utilizarán las siguientes opciones por línea de órdenes:
```
$ ./auth_cli --help
usage: auth_cli [-h] [--version] [-a ADMIN] [-u USERNAME] [-p PASSWORD]
                [--force] [--debug]
                [URL] [SCRIPTS ...]

Client for ADI Auth service

positional arguments:
  URL                   URL of ADI Auth API.
  SCRIPTS               Scripts to run. Stdin (interactive) used if omited.

options:
  -h, --help            show this help message and exit
  --version             show program's version number and exit

Auto-login options:
  -a ADMIN, --admin ADMIN
                        Set administrator token
  -u USERNAME, --username USERNAME
                        Username to auto-login (default: disable auto-login)
  -p PASSWORD, --password PASSWORD
                        Set password instead of prompt (not recommended,
                        insecure)

Running options:
  --force               Continue even if some error is reported

Debugging options:
  --debug, -d           Show debugging messages
```
Si, por ejemplo, deseamos conectarnos a un servicio que esté disponible en nuestra máquina de manera local y en el puerto TCP 3001, podemos utilizar:
```
$ ./auth_cli http://localhost:3001
ADI Auth interactive shell initialized
Use "?" for help. Ctrl-d to quit.
ADI-Auth(offline):
```
Como vemos, el _prompt_ en este caso cambia, para indicar que el cliente dispone de un endpoint al que mandar solicitudes pero que actualmente no hay un usuario _loggeado_. Podemos solicitar la ayuda para averiguar el comando de _login_:
```
ADI-Auth(offline): help

Documented commands (type help <topic>):
========================================
EOF         delete_user  help   logout    quit                set_admin_token
connect_to  disconnect   login  new_user  refresh_auth_token  show_token

ADI-Auth(offline):
```
Mediante el comando _login_ podremos realizar dicha autenticación:
```
ADI-Auth(offline): login
Enter username: usuario1
Password:
ADI-Auth(user):
```
Ahora el _prompt_ informa de que el cliente está utilizando el servicio en modo usuario, con lo que podemos mostrar el _token_ asociado:
```
ADI-Auth(user): show_token
EZ2BJCViZ1QqOYik-8mxVdm60DyD-Omty78okb2I
```
Además de las opciones de la _shell_ para configurar el acceso a la API y el usuario que realiza las acciones, por línea de comandos podemos establecer también la URL de acceso (como se vio en el ejemplo inicial) así como otros parámetros tales como el _token_ administrativo (necesario para crear y eliminar usuarios) o el usuario y contraseña para acceder como usuario normal. Si se especifica -u/--user y no se especifica _password_ (no recomendado), el programa solicitará la contraseña de manera interactiva.

Se pueden escribir scripts de comandos en archivos separados y se pueden pasar como argumentos adicionales después de la URL de acceso. Si durante la ejecución de estos scripts (también válido para ejecución de _shell_ interactiva) se produce un error, la ejecución se interrumpe. Para evitar esto, lance el cliente con la opción **--force**.