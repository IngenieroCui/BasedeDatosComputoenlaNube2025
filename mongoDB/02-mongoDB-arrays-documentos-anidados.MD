# Busquedas en arrays y documentos anidados

Buscar desde un documento anidado

```json
    db.ciudades.find({"alcalde.nombre": "Rita"})
    db.ciudades.find({alcalde: {nombre:"Rita", apellidos: "Macias", edad: 57}})
```

1. Seleccionar las ciudades donde los alcaldes tengan mas de 70 anios

```json
    db.ciudades.find({"alcalde.edad": {$gt: 70}})
```

2. Realizar la consulta anterior pero realizando una proyeccion

```json
db.ciudades.find({"alcalde.edad": {$gt: 70}}, {_id:0, nombre: 1, "alcalde.edad": 1})
```

3. Seleccionar las ciudades donde el alcalde tenga 76 anios o 41 anios

```json
db.ciudades.find({$or: [{"alcalde.edad": 76}, {"alcalde.edad": 41}]})
```

## Consultas con arrays

1. Crear una colección cazas

db.createCollection("cazas")

2. Cargar los documentos de la coleccion cazas a la db

## Ejemplos con consulta con Arrays

1. Buscar los documentos de la coleccion cazas cuyo pais sea USA

```json
db.cazas.find({paises: "usa"})
```

2. Buscar los documentos donde las dimensiones de los cazas sea mayor a 5

```json
db.cazas.find({dimensiones: {$gt:5 }})
```

3. Buscar los documentos donde las dimensiones del avion por ancho que es la posicion 1
que sea superior a 14
```json
db.cazas.find({"dimensiones.1": {$gt: 14}}, {_id:0, modelo: 1, dimensiones: 1})
```

### Arrays. Operador $all

1. Buscar los aviones que estan sirviendo en egipto e israel

```json
db.cazas.find({paises: {$all: ["egipto", "israel"]}})

db.cazas.find({$and: [{paises: "egipto"}, {paises: "israel"}]})
```

2. Seleccionar los cazas que sirven en inglaterra y españa 

```json
db.cazas.find({paises: {$all: ["inglaterra", "españa"]}})

db.cazas.find({$and: [{paises: "inglaterra"}, {paises: "españa"}]})
```
### Arrays operador $elementMatch -> Permite hacer busquedas dentro de arrays, este busca la lista de condiciones que estan dentro del operador.

1. Buscar que aviones tienen uso dentro de inglaterra

```json
db.cazas.find({paises: {$elemMatch: {$eq: "inglaterra"}}})
```

2. Buscar los aviones donde las dimensiones sean mayores a 20 o mayores  15

```json
db.cazas.find({dimensiones: {$elemMatch: {$lt:20, $gt: 15}}})
```

### Buscar en arrays de documentos anidados 

1. utilizar la coleccion ciudades.
2. Buscar los consejeros de nombre Jeri Flowers de edad 78
```json
db.ciudades.find({identificador:1, nombre: Jeri FLowers, edad:78})
```
3. Buscar un consejero donde tengan una edad mayor a 70 
```json
db.ciudades.find({"consejeros.edad": {$gt: 70}})
```

4. Buscar en consejeros en la posicion 2 que esten entre 70 y 80 años
```json
db.ciudades.find({"consejeros.2.edad": {$lt:80, $gt: 70}})
```

5. Buscar los consejeros de la posicion 0 que sean mayores a 70
```json
db.ciudades.find({"consejeros.0.edad": {$gt: 70}}, {_id: 0,"consejeros.identificador": 1, "consejeros.nombre": 1, "consejeros.edad": 1})
```
6. Buscar los alcalde mayores a 70
```json
db.ciudades.find({"alcalde.edad": {$gt: 70}}, {_id: 0, "alcalde.nombre": 1, "alcalde.edad": 1})
```