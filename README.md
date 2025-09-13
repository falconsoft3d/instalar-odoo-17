# Instalación de Odoo 17 CE

Guía completa para la instalación y configuración de Odoo 17 Community Edition en Linux Ubuntu.

**Autor:** Marlon Falcón Hernández | Madrid, España
- ERP, CRM y Software
- WhatsApp: +34 662 47 06 45
- Telegram: falconsoft
- Email: mfalconsoft@gmail.com , falconsoft.3d@gmail.com
- Github: https://github.com/falconsoft3d
- LinkedIn: https://linkedin.com/in/marlon-falcón-3a2aa9a4

## 📋 Índice

1. [Preparación del Servidor](#1-preparación-del-servidor)
   - [Creación de cuenta en Digital Ocean](#creación-de-cuenta-en-digital-ocean)
   - [Configuración de acceso SSH](#configuración-de-acceso-ssh)
   - [Actualización del sistema](#actualización-del-sistema)

2. [Instalación de Odoo 17](#2-instalación-de-odoo-17)
   - [Instalación desde repositorio oficial](#instalación-desde-repositorio-oficial)
   - [Dependencias adicionales](#dependencias-adicionales)

3. [Dependencias y Librerías](#3-dependencias-y-librerías)
   - [Librerías del sistema](#librerías-del-sistema)
   - [Librerías de Python](#librerías-de-python)
   - [Instalación de wkhtmltopdf](#instalación-de-wkhtmltopdf)

4. [Configuración Inicial](#4-configuración-inicial)
   - [Configuración básica de Odoo](#configuración-básica-de-odoo)
   - [Estructura de directorios](#estructura-de-directorios)
   - [Configuración de Nginx](#configuración-de-nginx)

5. [Configuración Avanzada](#5-configuración-avanzada)
   - [Certificados SSL](#certificados-ssl)
   - [Filtrado de bases de datos](#filtrado-de-bases-de-datos)
   - [Configuración de logs](#configuración-de-logs)
   - [Optimización de rendimiento](#optimización-de-rendimiento)

6. [Gestión y Mantenimiento](#6-gestión-y-mantenimiento)
   - [Herramientas de servicio](#herramientas-de-servicio)
   - [Actualizaciones](#actualizaciones)
   - [Shell de Odoo](#shell-de-odoo)
   - [Respaldos](#respaldos)

7. [Configuración de Desarrollo](#7-configuración-de-desarrollo)
   - [Integración con PyCharm](#integración-con-pycharm)
   - [Repositorios adicionales](#repositorios-adicionales)

8. [Solución de Problemas](#8-solución-de-problemas)
   - [Búsqueda de archivos](#búsqueda-de-archivos)
   - [Análisis de logs](#análisis-de-logs)
   - [Instalación alternativa](#instalación-alternativa)

---

## 1. Preparación del Servidor

### Creación de cuenta en Digital Ocean
Para obtener créditos gratuitos, utiliza este enlace de referencia:
```
https://m.do.co/c/7f5c3af8d6bb
```

### Configuración de acceso SSH
Configura el acceso sin contraseña para facilitar la administración:
```bash
ssh-copy-id root@tu_servidor_ip
```

### Actualización del sistema
Actualiza el sistema antes de comenzar la instalación:
```bash
apt-get update && apt-get upgrade -y
```

## 2. Instalación de Odoo 17

### Instalación desde repositorio oficial
Método recomendado usando el repositorio oficial de Odoo:
```bash
# Agregar la clave GPG de Odoo
wget -O - https://nightly.odoo.com/odoo.key | sudo gpg --dearmor -o /usr/share/keyrings/odoo-archive-keyring.gpg

# Agregar el repositorio
echo 'deb [signed-by=/usr/share/keyrings/odoo-archive-keyring.gpg] https://nightly.odoo.com/17.0/nightly/deb/ ./' | sudo tee /etc/apt/sources.list.d/odoo.list

# Instalar Odoo
sudo apt-get update && sudo apt-get install odoo
```

### Dependencias adicionales
Instalar dependencias Node.js necesarias:
```bash
apt-get install node-less
```


## 3. Dependencias y Librerías

### Librerías del sistema
Instalar las librerías y dependencias necesarias para el correcto funcionamiento de Odoo:
```bash
sudo apt-get install build-essential python3-pil python3-lxml python3-dev python3-pip python3-setuptools npm nodejs git gdebi libldap2-dev libxml2-dev libxslt1-dev libjpeg-dev -y
sudo npm install -g less less-plugin-clean-css -y && sudo ln -s /usr/bin/nodejs /usr/bin/node
python3 -m pip install --upgrade pip setuptools wheel
```

### Librerías de Python
Instalar librerías adicionales de Python para funcionalidades extendidas:
```bash
# Librerías principales
sudo pip3 install paramiko
sudo pip3 install xmltodict
sudo pip3 install dicttoxml
sudo pip3 install xmlsig
sudo pip3 install num2words
sudo pip3 install pandas
sudo pip3 install phonenumbers
sudo pip3 install cchardet
sudo pip3 install cryptography
sudo pip3 install pyOpenSSL
sudo pip3 install SOAPpy
sudo pip3 install signxml
sudo pip3 install pdf417gen
sudo pip3 install pycrypto

# M2Crypto para Ubuntu/Linux
sudo apt-get install libssl-dev swig python3-dev gcc
sudo pip3 install M2Crypto
```

#### Instalación de M2Crypto en macOS
Para usuarios de macOS que desarrollen localmente:
```bash
brew install openssl
brew install swig
env LDFLAGS="-L$(brew --prefix openssl)/lib" \
CFLAGS="-I$(brew --prefix openssl)/include" \
SWIG_FEATURES="-cpperraswarn -includeall -I$(brew --prefix openssl)/include" \
pip install m2crypto
```

### Instalación de wkhtmltopdf
Herramienta necesaria para la generación de PDF en Odoo:

#### Ubuntu 22.04/20.04
```bash
sudo apt-get install -y xfonts-75dpi
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb
sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4_amd64.deb
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb
sudo apt install ./wkhtmltox_0.12.6-1.focal_amd64.deb
```

#### Ubuntu 18.04
```bash
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.bionic_amd64.deb
sudo apt install ./wkhtmltox_0.12.6-1.bionic_amd64.deb
```

#### Ubuntu 16.04
```bash
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.xenial_amd64.deb
sudo apt install ./wkhtmltox_0.12.6-1.xenial_amd64.deb
```

#### Verificación de instalación
```bash
wkhtmltopdf --version
```

## 4. Configuración Inicial

### Configuración básica de Odoo
Editar el archivo de configuración principal:
```bash
nano /etc/odoo/odoo.conf
```

Configuración mínima recomendada:
```ini
limit_time_real = 360
```

### Estructura de directorios
Crear las carpetas necesarias para addons y respaldos:
```bash
# Carpeta para addons adicionales
mkdir /opt/extra-addons
chown odoo: /opt/extra-addons/ -R

# Carpeta para respaldos
mkdir /opt/backup
chown odoo: /opt/backup/ -R
```

### Configuración de Nginx
Instalar y configurar Nginx como proxy reverso:
```bash
sudo apt-get install nginx -y
cd /etc/nginx/sites-available
git clone https://github.com/falconsoft3d/ngix-para-odoo-erp/
cd ngix-para-odoo-erp/
sudo cp /etc/nginx/sites-available/ngix-para-odoo-erp/default.conf /etc/nginx/sites-available/default.conf
cd ..
mv default default-temp
mv default.conf default
```

Editar la configuración para tu dominio:
```bash
nano /etc/nginx/sites-available/default
# Cambiar: server_name tu_dominio.com tu_ip;
nginx -s reload
```

### Generación de claves SSH
Para acceso remoto y despliegues:
```bash
ssh-keygen
cat ~/.ssh/id_rsa.pub
```

## 5. Configuración Avanzada

### Certificados SSL
Configuración de HTTPS usando Let's Encrypt (para Ubuntu 16.04/18.04):
```bash
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python3-certbot-nginx
sudo certbot --nginx
```

Durante el proceso de configuración:
- Seleccionar opción A (para aplicar a todos los dominios)
- Seleccionar opción 2 (para redirigir HTTP a HTTPS)

#### Configuración en Odoo después del SSL
En Odoo, ir a `Configuración > Parámetros > Parámetros del sistema` y configurar:
```
web.base.url: https://tu_dominio.com (cambiar http por https)
web.base.url.freeze: True
```

### Filtrado de bases de datos
Configurar el filtro de bases de datos según tus necesidades:

#### Filtrado por subdominio
```ini
dbfilter = ^%d$
```

#### Filtrado por dominio
```ini
dbfilter = ^%h$
```

### Configuración de logs
Configurar el nivel de logging en `/etc/odoo/odoo.conf`:
```ini
log_level = info
```

Opciones disponibles: `info`, `debug_rpc`, `warn`, `test`, `critical`, `runbot`, `debug_sql`, `error`, `debug`, `debug_rpc_answer`, `notset`

### Optimización de rendimiento
Configuración recomendada para servidores en producción:

#### Configuración básica en odoo.conf
```ini
addons_path = /usr/lib/python3/dist-packages/odoo/addons,/opt/extra-addons
admin_passwd = tu_password_seguro
data_dir = /var/lib/odoo/.local/share/Odoo
db_user = odoo
http_port = 8069
logfile = /var/log/odoo/odoo-server.log
longpolling_port = 8072

# Configuración de memoria y tiempo
limit_memory_hard = 2684354560
limit_memory_soft = 2147483648
limit_request = 8192
limit_time_cpu = 60
limit_time_real = 120
limit_time_real_cron = -1

# Configuración de workers para producción
workers = 17
limit_time_real = 1200
limit_time_cpu = 600

# Configuración de proxy
proxy_mode = True
xmlrpc_port = 8069
xmlrpc_interface = 127.0.0.1
netrpc_interface = 127.0.0.1

# Configuración de base de datos
db_maxconn = 64
db_sslmode = prefer
db_template = template0
list_db = True

# Configuración adicional
max_cron_threads = 2
server_wide_modules = base,web
without_demo = False
```

## 6. Gestión y Mantenimiento

### Herramientas de servicio
Comandos básicos para la gestión del servicio Odoo:
```bash
# Reiniciar el servicio
service odoo restart

# Ver logs en tiempo real
tail -f /var/log/odoo/odoo-server.log

# Ver logs de Nginx
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
```

### Actualizaciones
Actualizar todos los módulos de una base de datos:
```bash
/etc/init.d/odoo stop
su - odoo -s /bin/bash
odoo -d tu_base_de_datos -u all --stop-after-init --logfile=/dev/stdout
/etc/init.d/odoo start
```

### Shell de Odoo
Acceso al shell interactivo para operaciones avanzadas:
```bash
/etc/init.d/odoo stop
su - odoo -s /bin/bash
odoo shell -d tu_base_de_datos
```

#### Ejemplo de uso del shell
```python
# Buscar un módulo específico
mod = env['ir.module.module'].search([('name','=','purchase_request')])
print(mod)

# Modificar productos masivamente
products = env['product.template'].search([('type', '=', 'consu')])
for product in products:
    try:
        print(product.id)
        print('Modificando')
        product.type = 'product'
    except:
        print('error')

# Confirmar cambios
env.cr.commit()
```

Salir del shell y reiniciar el servicio:
```bash
/etc/init.d/odoo start
```

### Respaldos
La carpeta `/opt/backup` está configurada para almacenar respaldos de bases de datos y archivos.

## 7. Configuración de Desarrollo

### Integración con PyCharm
Configuración para desarrollo local con PyCharm:
```bash
# Ruta del ejecutable de Odoo
/Users/marlonfalcon/Documents/odoo/odoo-17/odoo/odoo-bin

# Parámetros de configuración
--config=/Users/marlonfalcon/Documents/odoo/odoo-17/odoo/debian/odoo-ee.conf -u base_bim_2 --i18n-overwrite
```

### Repositorios adicionales
Configurar repositorios oficiales de Odoo para desarrollo:
```bash
mkdir /opt/extra-addons-odoo
chown odoo: /opt/extra-addons-odoo/ -R
cd /opt/extra-addons-odoo/
git clone https://github.com/odoo/odoo.git
git checkout 17.0
```

## 8. Solución de Problemas

### Búsqueda de archivos
Localizar archivos de Odoo en el sistema:
```bash
find / -name "odoo"
```

### Análisis de logs
Ver los logs en tiempo real:
```bash
tail -f /var/log/odoo/odoo-server.log
```

Buscar errores específicos con contexto:
```bash
# Ejemplo: buscar un error específico con 50 líneas de contexto
grep "2021-04-28 06:55:52,330 43141" /var/log/odoo/odoo-server.log -B 50
```

### Instalación alternativa
Método alternativo usando script automatizado:
```bash
# Script de instalación automática
wget https://raw.githubusercontent.com/Yenthe666/InstallScript/17.0/odoo_install.sh
sudo chmod +x odoo_install.sh
sudo ./odoo_install.sh
```

**Nota:** Verifica la disponibilidad del script para la versión 17.0 antes de usar este método.

---

## 📝 Notas Adicionales

- **Seguridad:** Siempre cambia las contraseñas por defecto y configura un firewall apropiado.
- **Actualizaciones:** Mantén el sistema y Odoo actualizados regularmente.
- **Respaldos:** Implementa una estrategia de respaldos regular para tus bases de datos.
- **Monitoreo:** Configura monitoreo de logs para detectar problemas tempranamente.

## 🔗 Enlaces Útiles

- [Documentación oficial de Odoo](https://www.odoo.com/documentation/17.0/)
- [Repositorio oficial de Odoo en GitHub](https://github.com/odoo/odoo)
- [Configuración de Nginx para Odoo](https://github.com/falconsoft3d/ngix-para-odoo-erp)

---

**¿Problemas o mejoras?** Contacta al autor o crea un issue en el repositorio.
