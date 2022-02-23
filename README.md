# Star War API

### Installation 

Instale módulos npm en terminal (shell / sh).

```sh
npm install
```

### Setup Environment

Realice una copia del archivo de variables de entorno `(.env.example.yml)` y configure las variables correspondientes.
Considere *NODE_ENV* con el valor *"development"* `(Por defecto tendrá este valor)`.

```sh
cp .env.example.yml .env.yml
```
archivo yml estructura

```yaml
development:
  DB_USERNAME: "" 
  DB_PASSWORD: ""
  DB_DATABASE: ""
  DB_PORT: 3306
  DB_HOST: "localhost"
  DB_DIALECT: "mysql"
  SWAPI_URL: "https://swapi.py4e.com/api/"
```
Será necesario crear una base de datos.
si usas mac configurar el mysql 
en caso no lo tengas instalado:  brew install mysql y luego para levantar y cerrar
si deseas configurar user y password:  mysql -u -root
mysql.server start
mysqld stop / exit

```sql
CREATE DATABASE starwars;
```
### Migrations

Ejecute migraciones *.env.yml* `(DB_DATABASE)`

```sh
npx sequelize-cli db:migrate 
```

### Resources
Los endpoints desarrollados

- films
- people


| `api/films`      | Listado de peliculas. |
| `api/people`    | A People resource is an individual person or character within the Star Wars universe. |

metodos desarrollados en ambos endpoints: `POST`, `GET`, `PUT`, and `DELETE`.

#### Relational Model

La estructura de los recursos de [`SWAPI`](https://swapi.py4e.com/documentation), resulta ser más cercana a un modelo `NoSQL` que a uno relacional. Dado que la base de datos a utilizar es **mysql**, se normalizaron las entidades y sus relaciones. Sin embargo, esto no es transparente para un cliente que consuma el API, pues, se mantiene la estructura mostrada en [`SWAPI`](https://swapi.py4e.com/documentation), pero, solo a nivel de presentación.

Podemos agregar personas`(People)` al crear un recurso `Films`, especificando los **ids** o el **id** de persona(s) `(People)` existente. Esto para todos los hijos de Films `['people', 'planet', 'specie', 'starship', 'vehicle']`

Prueba para subir data en postman u otra herramienta

```bash
curl -X POST http://localhost:8082/development/api/films \
    -H "Content-Type: application/json" \
    -d
{
    "director": "Pepito el escamoso",
    "episode_id": 4,
    "opening_crawl": "It is a period of civil war...",
    "producer": "cr7",
    "release_date": "Now",
    "title": "El partido",
    "people": 1
}

curl -X POST http://localhost:8082/development/api/films \
    -H "Content-Type: application/json" \
    -d
{
    "director": "Gringo atrasador",
    "episode_id": 4,
    "opening_crawl": "It is a period of civil war...",
    "producer": "Joel",
    "release_date": "Now",
    "title": "el rap ",
    "people": [1, 2, 3]
}
```

##### Films

| Resource / HTTP method | Post             | Get         | Patch                  | Delete             |
| ---------------------- | ---------------- | ----------- | ---------------------- | ------------------ |
| `api/films`            | Create new film  | List films  | Error                  | Error              |
| `api/films/{id}`       | Error            | Get film    | Update user if exists  | Delete film        |

`api/films` acepta el parametro **lang**, especificado con el valor **es**, mapea los campos español, por defecto es ingles.

```bash
http://localhost:8082/development/api/films?lang=es
```

##### People

| Resource / HTTP method | Post             | Get         | Patch                  | Delete             |
| ---------------------- | ---------------- | ----------- | ---------------------- | ------------------ |
| `api/people`           | Create new pers  | List pers | Error                  | Error              |
| `api/people/{id}`      | Error            | Get pers  | Update pers if exists| Delete people      |


### Start Project 

Se iniciará un emulador de AWS λ en el puerto `8082`. El formato de los endpoint será el siguiente:

```sh
http://{host}:8082/{environment}/api/films
```

```sh
npm start
```

### Scripts & Deploy

Para generar los archivos *CloudFormation* ejecute:

```sh
npm run artifacts
```

Si se desea desplegar la API, configurar en terminal los accesos de AWS adecuadamente, y luego ejecutar el comando `sls deploy`
