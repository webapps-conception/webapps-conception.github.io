---
layout: post
robots: index,follow
published: true
title: "Comment développer une API Rest en Python ?"
date: 2024-09-28 12:30:00 +0002
tags:
- python
- flask
---

# Comment développer une API Rest en Python ?
## Architecture REST

![Architecture REST](/assets/images/architecture-rest.webp)

REST (Representational State Transfer) ou RESTful est un style d’architecture permettant de construire des applications (Web, Intranet, Web Service). Il s’agit d’un ensemble de conventions et de bonnes pratiques à respecter et non d’une technologie à part entière. L’architecture REST utilise les spécifications originelles du protocole HTTP.

- L’URI est utilisé comme identifiant de ressources.
- Les protocoles HTTP (GET, POST, PUT, DELETE) sont utilisés comme identifiant des opérations.
- Les réponses HTTP (HTML, XML, CSV, JSON) sont utilisées comme représentation des ressources.
- Les liens sont utilisés comme relation entre les ressources d'une application.
- L'entête HEADER est utilisée comme jeton d'authentification (token).

## Flask-RESTful
[Flask-RESTful](https://flask-restful.readthedocs.io/en/latest/index.html "Documentation Flask-RESTful"){:target="_blank"} est une extension pour Flask qui ajoute la prise en charge de la création rapide d'API REST. Il s'agit d'une abstraction légère qui fonctionne avec vos ORM/bibliothèques existantes. Flask-RESTful encourage les meilleures pratiques avec une configuration minimale. Si vous connaissez Flask, Flask-RESTful devrait être facile à maîtriser.

#### Installation
``` bash
pip install flask-restful
```

#### Exemple d'API
Dans ce post, nous n'allons pas nous attarder sur la documentation technique du module [Flask-RESTful](https://flask-restful.readthedocs.io/en/latest/index.html "Documentation Flask-RESTful"){:target="_blank"}, mais plutôt aller à l'essentiel par un exemple.

``` python
from flask import Flask
from flask_restful import reqparse, abort, Api, Resource

app = Flask(__name__)
api = Api(app)

TODOS = {
    'todo1': {'task': 'build an API'},
    'todo2': {'task': '?????'},
    'todo3': {'task': 'profit!'},
}


def abort_if_todo_doesnt_exist(todo_id):
    if todo_id not in TODOS:
        abort(404, message="Todo {} doesn't exist".format(todo_id))

parser = reqparse.RequestParser()
parser.add_argument('task')


# Todo
# shows a single todo item and lets you delete a todo item
class Todo(Resource):
    def get(self, todo_id):
        abort_if_todo_doesnt_exist(todo_id)
        return TODOS[todo_id]

    def delete(self, todo_id):
        abort_if_todo_doesnt_exist(todo_id)
        del TODOS[todo_id]
        return '', 204

    def put(self, todo_id):
        args = parser.parse_args()
        task = {'task': args['task']}
        TODOS[todo_id] = task
        return task, 201


# TodoList
# shows a list of all todos, and lets you POST to add new tasks
class TodoList(Resource):
    def get(self):
        return TODOS

    def post(self):
        args = parser.parse_args()
        todo_id = int(max(TODOS.keys()).lstrip('todo')) + 1
        todo_id = 'todo%i' % todo_id
        TODOS[todo_id] = {'task': args['task']}
        return TODOS[todo_id], 201

##
## Actually setup the Api resource routing here
##
api.add_resource(TodoList, '/todos')
api.add_resource(Todo, '/todos/<todo_id>')


if __name__ == '__main__':
    app.run(debug=True)
```

>💡 Le code de cet exemple est disponible sur [Github](https://github.com/webapps-conception/flask-restful-python-example){:target="_blank"}.

#### Démarrage du service
``` bash
$ python api.py
 * Serving Flask app 'api'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 105-130-942
```

#### Obtenir la liste (GET)
``` bash
$ curl http://localhost:5000/todos
{
    "todo1": {
        "task": "build an API"
    },
    "todo2": {
        "task": "?????"
    },
    "todo3": {
        "task": "profit!"
    }
}
```

#### Obtenir une seule tâche (GET)
``` bash
$ curl http://localhost:5000/todos/todo3
{
    "task": "profit!"
}
```

#### Suppression d'une tâche (DELETE)
``` bash
$ curl http://localhost:5000/todos/todo2 -X DELETE -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> DELETE /todos/todo2 HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> 
< HTTP/1.1 204 NO CONTENT
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 11:30:03 GMT
< Content-Type: application/json
< Connection: close
< 
* Closing connection 0
```

#### Ajout d'une nouvelle tâche
``` bash
$ curl http://localhost:5000/todos -d '{ "task":"something new" }' -X POST -H "Content-Type: application/json" -v
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> POST /todos HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Content-Type: application/json
> Content-Length: 26
> 
* upload completely sent off: 26 out of 26 bytes
< HTTP/1.1 201 CREATED
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 11:37:29 GMT
< Content-Type: application/json
< Content-Length: 32
< Connection: close
< 
{
    "task": "something new"
}
* Closing connection 0
```

#### Mise à jour d'une tâche (PUT)
``` bash
$ curl http://localhost:5000/todos/todo3 -d '{ "task":"something different" }' -X PUT -H "Content-Type: application/json" -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> PUT /todos/todo3 HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Content-Type: application/json
> Content-Length: 32
> 
* upload completely sent off: 32 out of 32 bytes
< HTTP/1.1 201 CREATED
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 11:40:06 GMT
< Content-Type: application/json
< Content-Length: 38
< Connection: close
< 
{
    "task": "something different"
}
* Closing connection 0
```

#### Affichage du résultat
``` bash
$ curl http://localhost:5000/todos
{
    "todo1": {
        "task": "build an API"
    },
    "todo3": {
        "task": "something different"
    },
    "todo4": {
        "task": "something new"
    }
}
```

## Flask-JWT-Extended
Afin de sécuriser votre application Flask, il est nécessaire de comprendre le fonctionnement d'une authentification basic JWT (JSON Web Token), ce qui permet l'échange sécurisé de jetons entre le serveur et les clients. Cette sécurité de l’échange se traduit par la vérification de l'intégrité et de l'authenticité des données. Elle s’effectue par l'algorithme HMAC ou RSA.

Le site [JWT.io](https://jwt.io/){:target="_blank"} permet de décoder, vérifier et générer JWT.

Flask-JWT-Extended ajoute non seulement la prise en charge de l'utilisation des jetons Web JSON (JWT) à Flask pour protéger les vues, mais également de nombreuses fonctionnalités utiles (et facultatives) intégrées pour faciliter l'utilisation des jetons Web JSON. Ceux-ci incluent :

- Prise en charge de l'ajout de revendications personnalisées aux jetons Web JSON
- Validation des revendications personnalisées sur les jetons reçus
- Création de jetons à partir d'objets complexes ou d'objets complexes à partir de jetons reçus
- Actualiser les jetons
- Fraîcheur des jetons et décorateurs de vue séparés pour autoriser uniquement les nouveaux jetons
- Révocation/mise sur liste noire de jetons
- Stockage des jetons dans les cookies et protection CSRF

#### Installation

``` bash
pip install flask-jwt-extended
```

#### Usage basic
Dans ce post, nous n'allons pas nous attarder sur la documentation technique du module [Flask-JWT-Extended](https://flask-jwt-extended.readthedocs.io/en/stable/ "Flask-JWT-Extended’s Documentation"){:target="_blank"}, mais plutôt aller à l'essentiel par un exemple.

``` python
from flask import Flask
from flask import jsonify
from flask import request

from flask_jwt_extended import create_access_token
from flask_jwt_extended import get_jwt_identity
from flask_jwt_extended import jwt_required
from flask_jwt_extended import JWTManager

app = Flask(__name__)

# Setup the Flask-JWT-Extended extension
app.config["JWT_SECRET_KEY"] = "super-secret"  # Change this!
jwt = JWTManager(app)


# Create a route to authenticate your users and return JWTs. The
# create_access_token() function is used to actually generate the JWT.
@app.route("/login", methods=["POST"])
def login():
    username = request.json.get("username", None)
    password = request.json.get("password", None)
    if username != "test" or password != "test":
        return jsonify({"msg": "Bad username or password"}), 401

    access_token = create_access_token(identity=username)
    return jsonify(access_token=access_token)


# Protect a route with jwt_required, which will kick out requests
# without a valid JWT present.
@app.route("/protected", methods=["GET"])
@jwt_required()
def protected():
    # Access the identity of the current user with get_jwt_identity
    current_user = get_jwt_identity()
    return jsonify(logged_in_as=current_user), 200


if __name__ == "__main__":
    app.run()
```

>💡 Le code de cet exemple est disponible sur [Github](https://github.com/webapps-conception/flask-jwt-extended-python-example){:target="_blank"}.

#### Démarrage du service
``` bash
$ python api.py
 * Serving Flask app 'api'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
```

#### Test d'acccès à une route protégée
``` bash
$ curl http://localhost:5000/protected -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> GET /protected HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> 
< HTTP/1.1 401 UNAUTHORIZED
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 13:39:15 GMT
< Content-Type: application/json
< Content-Length: 39
< Connection: close
< 
{"msg":"Missing Authorization Header"}
* Closing connection 0
```

Pour accéder à une vue protégée jwt_required, vous devez envoyer le JWT avec chaque demande.

``` bash
$ curl http://localhost:5000/login -d '{ "username":"test", "password":"test" }' -X POST -H "Content-Type: application/json" -v
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> POST /login HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Content-Type: application/json
> Content-Length: 40
> 
* upload completely sent off: 40 out of 40 bytes
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 13:40:18 GMT
< Content-Type: application/json
< Content-Length: 349
< Connection: close
< 
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYxNzIxOCwianRpIjoiZDE5OGVjZWYtYTNkMC00YzAzLWI3OGEtMDQ5NDBkODEzNmEyIiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MTcyMTgsImNzcmYiOiIyZjdhOGNkOC1mNzMwLTQ5MjEtYjE1NC03MzBiZDk1Zjg5NWMiLCJleHAiOjE3Mjc2MTgxMTh9.dhMvJ9Osu2ddAxXQerDzqIzO7madi887txr8ybstDZ0"}
* Closing connection 0
```

Par défaut, cela se fait avec une entête d'autorisation qui ressemble à : `Authorization: Bearer <access_token>`

``` bash
$ export JWT="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYxNzIxOCwianRpIjoiZDE5OGVjZWYtYTNkMC00YzAzLWI3OGEtMDQ5NDBkODEzNmEyIiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MTcyMTgsImNzcmYiOiIyZjdhOGNkOC1mNzMwLTQ5MjEtYjE1NC03MzBiZDk1Zjg5NWMiLCJleHAiOjE3Mjc2MTgxMTh9.dhMvJ9Osu2ddAxXQerDzqIzO7madi887txr8ybstDZ0"

$ curl http://localhost:5000/protected -H "Authorization: Bearer $JWT" -H "Content-Type: application/json" -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> GET /protected HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYxNzIxOCwianRpIjoiZDE5OGVjZWYtYTNkMC00YzAzLWI3OGEtMDQ5NDBkODEzNmEyIiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MTcyMTgsImNzcmYiOiIyZjdhOGNkOC1mNzMwLTQ5MjEtYjE1NC03MzBiZDk1Zjg5NWMiLCJleHAiOjE3Mjc2MTgxMTh9.dhMvJ9Osu2ddAxXQerDzqIzO7madi887txr8ybstDZ0
> Content-Type: application/json
> 
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 13:47:25 GMT
< Content-Type: application/json
< Content-Length: 24
< Connection: close
< 
{"logged_in_as":"test"}
* Closing connection 0
```

## Sécurisation JWT d'un module Flask-RESTful
Afin de sécuriser notre API Flask-RESTful, appliquons la sécurisation JWT à notre exemple :

``` python
from flask import Flask
from flask import jsonify
from flask import request
from flask_restful import reqparse, abort, Api, Resource

from flask_jwt_extended import create_access_token
from flask_jwt_extended import get_jwt_identity
from flask_jwt_extended import jwt_required
from flask_jwt_extended import JWTManager

app = Flask(__name__)

# Setup the Flask-JWT-Extended extension
app.config["JWT_SECRET_KEY"] = "super-secret"  # Change this!
jwt = JWTManager(app)

api = Api(app)

TODOS = {
    'todo1': {'task': 'build an API'},
    'todo2': {'task': '?????'},
    'todo3': {'task': 'profit!'},
}


def abort_if_todo_doesnt_exist(todo_id):
    if todo_id not in TODOS:
        abort(404, message="Todo {} doesn't exist".format(todo_id))

parser = reqparse.RequestParser()
parser.add_argument('task')


# Todo
# shows a single todo item and lets you delete a todo item
class Todo(Resource):

    @jwt_required()
    def get(self, todo_id):
        abort_if_todo_doesnt_exist(todo_id)
        return TODOS[todo_id]

    @jwt_required()
    def delete(self, todo_id):
        abort_if_todo_doesnt_exist(todo_id)
        del TODOS[todo_id]
        return '', 204

    @jwt_required()
    def put(self, todo_id):
        args = parser.parse_args()
        task = {'task': args['task']}
        TODOS[todo_id] = task
        return task, 201


# TodoList
# shows a list of all todos, and lets you POST to add new tasks
class TodoList(Resource):

    @jwt_required()
    def get(self):
        return TODOS

    @jwt_required()
    def post(self):
        args = parser.parse_args()
        todo_id = int(max(TODOS.keys()).lstrip('todo')) + 1
        todo_id = 'todo%i' % todo_id
        TODOS[todo_id] = {'task': args['task']}
        return TODOS[todo_id], 201

##
## Actually setup the Api resource routing here
##
api.add_resource(TodoList, '/todos')
api.add_resource(Todo, '/todos/<todo_id>')

# Create a route to authenticate your users and return JWTs. The
# create_access_token() function is used to actually generate the JWT.
@app.route("/login", methods=["POST"])
def login():
    username = request.json.get("username", None)
    password = request.json.get("password", None)
    if username != "test" or password != "test":
        return jsonify({"msg": "Bad username or password"}), 401

    access_token = create_access_token(identity=username)
    return jsonify(access_token=access_token)


# Protect a route with jwt_required, which will kick out requests
# without a valid JWT present.
@app.route("/protected", methods=["GET"])
@jwt_required()
def protected():
    # Access the identity of the current user with get_jwt_identity
    current_user = get_jwt_identity()
    return jsonify(logged_in_as=current_user), 200

if __name__ == '__main__':
    app.run(debug=True)
```

>💡 Le code de cet exemple est disponible sur [Github](https://github.com/webapps-conception/flask-restful-python-example){:target="_blank"}.

#### Démarrage du service
``` bash
$ python api-jwt.py
 * Serving Flask app 'api-jwt'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 105-130-942
```

#### Obtenir la liste (GET)
``` bash
$ curl http://localhost:5000/todos
{
  "msg": "Missing Authorization Header"
}
```

>📝 Nous pouvons constater que nous n'avons plus accès directement à la route `/todos`.

Pour accéder à une vue protégée jwt_required, vous devez envoyer le JWT avec chaque demande :

``` bash
$ curl http://localhost:5000/login -d '{ "username":"test", "password":"test" }' -X POST -H "Content-Type: application/json" -v
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> POST /login HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Content-Type: application/json
> Content-Length: 40
> 
* upload completely sent off: 40 out of 40 bytes
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 14:37:44 GMT
< Content-Type: application/json
< Content-Length: 354
< Connection: close
< 
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYyMDY2NCwianRpIjoiMDVkNzMxOGEtZjdmMC00MzQ5LTk5ZjgtNjk3NTAxMjMwMmE3IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MjA2NjQsImNzcmYiOiIyYmJjY2JjMi1kZTk0LTQ0OGQtYTEyYS0wZDM4MzQxYjJjNDIiLCJleHAiOjE3Mjc2MjE1NjR9.UBOgw3I_lm48xcnO2hO1CA3XT6D6HDOWxX0knG8bDHU"
}
* Closing connection 0
```

Avec l'intégration de la Token dans l'entête d'autorisation, vous pouvez accéder à la ressource demandée :


``` bash
$ export JWT="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYyMDY2NCwianRpIjoiMDVkNzMxOGEtZjdmMC00MzQ5LTk5ZjgtNjk3NTAxMjMwMmE3IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MjA2NjQsImNzcmYiOiIyYmJjY2JjMi1kZTk0LTQ0OGQtYTEyYS0wZDM4MzQxYjJjNDIiLCJleHAiOjE3Mjc2MjE1NjR9.UBOgw3I_lm48xcnO2hO1CA3XT6D6HDOWxX0knG8bDHU"
$ curl http://localhost:5000/todos -H "Authorization: Bearer $JWT" -H "Content-Type: application/json" -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> GET /todos HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYyMDY2NCwianRpIjoiMDVkNzMxOGEtZjdmMC00MzQ5LTk5ZjgtNjk3NTAxMjMwMmE3IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MjA2NjQsImNzcmYiOiIyYmJjY2JjMi1kZTk0LTQ0OGQtYTEyYS0wZDM4MzQxYjJjNDIiLCJleHAiOjE3Mjc2MjE1NjR9.UBOgw3I_lm48xcnO2hO1CA3XT6D6HDOWxX0knG8bDHU
> Content-Type: application/json
> 
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 14:39:22 GMT
< Content-Type: application/json
< Content-Length: 150
< Connection: close
< 
{
    "todo1": {
        "task": "build an API"
    },
    "todo2": {
        "task": "?????"
    },
    "todo3": {
        "task": "profit!"
    }
}
* Closing connection 0
```
