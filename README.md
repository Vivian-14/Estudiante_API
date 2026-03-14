<div align="center">

# Resolución API
### Alumnos
(𝙱á𝚜𝚒𝚌𝚘 - POST/GET estudiantes)  


<img src="https://i.pinimg.com/1200x/62/92/87/6292878db61d36c92e7f9bc6262e039b.jpg" width="400">

### Introducción

En esta actividad se desarrolló una API utilizando Python y el framework Flask con el objetivo de registrar y consultar información de estudiantes dentro de una base de datos. La API permite almacenar datos como el nombre del estudiante, la carrera que cursa y el semestre en el que se encuentra.

Para cumplir con el objetivo del ejercicio, se implementaron dos endpoints principales. El primero es un endpoint POST que permite agregar nuevos estudiantes a la base de datos, y el segundo es un endpoint GET que permite consultar todos los registros almacenados.

Este proyecto tiene como finalidad practicar la creación de APIs, la conexión de una aplicación web con una base de datos y la realización de operaciones básicas de almacenamiento y consulta de información utilizando Flask.


<br><br>

<img src="https://github.com/Vivian-14/Estudiante_API/blob/main/PRUEBAS/3.png" width="700">
<img src="https://github.com/Vivian-14/Estudiante_API/blob/main/PRUEBAS/1.png" width="700">
<img src="https://github.com/Vivian-14/Estudiante_API/blob/main/PRUEBAS/2.png" width="700">

</div>

###  Codigo Utilizado

```python
from flask import Flask, request, jsonify, render_template
from database import db
from models import Estudiante

# primero se crea la app
app = Flask(__name__)

# configuración base de datos
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///estudiantes.db"
app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False

db.init_app(app)

# crear la base de datos
with app.app_context():
    db.create_all()

# ruta para el HTML
@app.route("/")
def inicio():
    return render_template("index.html")


# POST estudiantes
@app.route("/estudiantes", methods=["POST"])
def agregar_estudiante():

    data = request.json

    nuevo = Estudiante(
        nombre=data["nombre"],
        carrera=data["carrera"],
        semestre=data["semestre"]
    )

    db.session.add(nuevo)
    db.session.commit()

    return jsonify({"mensaje": "Estudiante registrado"})


# GET estudiantes
@app.route("/estudiantes", methods=["GET"])
def obtener_estudiantes():

    estudiantes = Estudiante.query.all()

    return jsonify([e.to_dict() for e in estudiantes])


if __name__ == "__main__":
    app.run(debug=True)


```
