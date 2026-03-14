<div align="center">

# eｊｅｒｃｉｃｉｏ
### Alumnos

(𝙱á𝚜𝚒𝚌𝚘 - POST/GET estudiantes)  


<img src="https://i.pinimg.com/1200x/62/92/87/6292878db61d36c92e7f9bc6262e039b.jpg" width="400">

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
