# Manual de instalacion de la aplicacion web

"INTRO"

## Decisiones de proyecto

|Elemento|Decisión|Versión|Justificación|
|--------|-------|--------|-------------|
|Servidor Web|Apache|2.4|Sencillo de usar|
|Base de Datos|MySQL|8|Se usó el año pasado|
|Lenguaje Servidor|Python|3|Uso extendido|
|FrameWork|Flask|3|Sencillo de usar, pensado para webs (formularios, sesiones)|
|Control de versiones|Git|2|Sencillo de usar, popular|
|Documentación|Markdown|-|Muy utilizado por Git|

## Proceso de instalación / puesta en marcha

1. Actualizar el sistema
`sudo apt update`
`sudo apt upgrade`
2. Instalar y configurar Git
`sudo apt git`
`git init`
`git add .`
`git commit -m "Commit incial con readme y pagina principa formulario web"`
3. Instalar VSC y Plugins:
    - Markdown all in one
4. Instalar Apache2
`sudo apt install apache2`
5. Cambio de propietario y permisos de carpeta
`sudo chown -R $USER:$USER /var/www/html`
`sudo chmod -R u=rwX,go=rX /var/www/html`
6. Creacion de nueva carpeta de "Incidencias", y cambio de propietario de carpeta
`sudo mkdir -p /var/www/incidencias.ies.teis`
`sudo chown -R $USER:$USER /var/www/incidencias.ies.teis`
7. Creacion de Documento "incidencias.ies.teis.conf"
`sudo nano /etc/apache2/sites-available/incidencias.ies.teis.conf`
<VirtualHost *:80>
        ServerName incidencias.ies.teis
        DocumentRoot /var/www/incidencias.ies.teis
        <Directory /var/www/incidencias.ies.teis>
                AllowOverride All
                Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/incidencias-error.log
        CustomLog ${APACHE_LOG_DIR}/incidencias-error.log combined
</VirtualHost>

8. Instalar MySQL.
``` bash
sudo apt install mysql-server
```

9. Configuracion de MySQL.
``` bash
sudo mysql

create database incidencias;
create user 'incidencias'@'localhost' identified by 'incidencias';
grant all privileges on incidencias.* to 'incidencias'@'localhost';
flush privileges;
```

10. Creacion de tabla y insercion de datos en ella.
``` bash
create table registro( 
        id int auto_increment primary key, 
        aula varchar(30), 
        descripcion text, 
        usuario varchar(20), 
        estado varchar(30) 
        );

insert into registro (aula, descripcion, usuario, estado) values ('Taller1', 'PC 24 no arranca', 'Pedro', 'ABIERTA'), ('Taller1', 'Proyector no se ve nitido', 'Pedro', 'ABIERTA');
```

## Configuracion de git/github

1. Crear repositorio local, añadir archivos y commit
```bash
git init
git add.
git commit -m "comentario"
```
2. Crear cuenta github, crear repositorio en github

3. Sincronizacion entre github y equipo local
```bash
git remote add origin https://github.com/CLOUD-29/incidencias.ies.teis.git
git branch -M main
git push -u origin main
```

## Instalación de Python y componentes relacionados

1. Instalacion de PYHTON
```bash
sudo apt install python3 python3-pip python3-venv -y
```

1. Creacion de entorno virtual
```bash
python3 -m venv venv
source venv/bin/activate
```

3. Instalar flask, conector de bases de datos, comprobar y guardar las dependencias
```bash
pip install flask
pip install mysql-connector-python
pip list
pip freeze > requirements.txt
```


## Rutina trabajo flask

Al empezar:
```bash
cd /var/www/incindencias.ies.teis
source venv/bin/activate
python app.py # Lanzar app
```

Al terminar:
```bash
    Ctrl+C para salir del entorno
    deactivate
```


## Primera aplicacion Python/Flask

1. Creamos un fichero app.py en la carpeta
```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def inicio():
    return "<h1>Incidencias IES Teis</h1>"

if __name__ == "__main__":
    app.run(debug=True)
```

- Tras iniciar comando revisar con navegador que la pagina muestra lo escrito en el comando "<h1>Incidencias IES Teis</h1>"
  
Ir a navegador y introducir la direccion asignada a la pagina:
```bash
http://localhost:5000 / http://127.0.0.1:5000
```

## Migracion del formulario a Python/Flask

1. Creamos una carpeta tempaltes y movmos ahí nuestro index.html
2. Modificaos app.py:
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def inicio():
    return render_template("index.html")

if __name__ == "__main__":
    app.run(debug=True)
```
3. Comprobamos accediendo a http://localhost:5000 (ó la direccion añadida en /etc/hosts).Hemos conseguido que ahora el formulario lo devuelva Flask


## Recibir los datos del formulario

1. 
```python
from flask import Flask, render_template, request
```

2. Añadimos una ruta en app.py para recibir los datos del formulario:
```python
@app.route("/incidencia", methods=["POST"])
def crear_incidencia():

    Aula = request.from["aula"]
    Usuario = request.form["usuario"]
    Descripcion = request.form["dsecripcion"]

    return "Incidencia recibida"
```

## Introducir datos en la BD
```python
import mysql.connector

def crear_incidencia():
    conexion = mysql.connector.connect(
        host="localhost".
        user="incidencias".
        password="incidencias".
        database="incidencias".
    )

    cursor = conexion.cursor()

    sql = """
        INSERT INTO registro
        (aula, usuario, descripcion, estado)
        VALUES(%5, %5, %5, %5)
    """

    valores = (
        aula,
        usuario,
        descripcion,
        "Abierta"
    )

    cursor.execute(sql, valores)

    conexion.execute(sql, valores)

    cursor.commit()

    cursor.close()
    conexion.close()

    return "incidencia recibida"
```


show databases;
select user from mysql.user;

- Despues de meter README en incidencias.ies.teis
`git init`
`git add .`
`git commit -m "Commit incial con readme y pagina principa formulario web"`

- Primera vez que descargamos datos de GitHub
``` bash
git clone https://github.com/CLOUD-29/incidencias.ies.teis.git
```

- Actualizar documentos tras haber hecho instalacion para actualizar cambios de GitHub
```bash
git pull