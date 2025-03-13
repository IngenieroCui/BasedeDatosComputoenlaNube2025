# Agregaciones

## Metodos para realizar agregaciones simples

- distinct(): Devuelve valores no duplicados.
- countDocuments(): Cuenta los documentos en una coleccion (parecido al size).
- estimatedDocumentCount(): Cuenta de manera aproximada durante un periodo de tiempo.

## Una Aggregation pipeline consta de una o mas etapas (stages) que procesan documentos

1. Cada etapa realiza una operacion en los documentos de entrada. Por ejemplo, una fase puede filtrar documentos, agrupar documentos y calcular valores.

2. Los documentos que se generan en una fase pasan a la siguiente fase. 

3. Pueden devolver resultados para grupos de documentos como totales, maximo, minimo, etc. 

## Se utiliza la clausula "aggregate" 

- Existen una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen distintos tipos de etapa, de comparacion, booleanos, aritmeticos, de cadena, etc.

## Metodos simples: countDocuments() y distinct() 

1. Contar los documentos de la coleccion libros 
```json
db.libros.countDocuments()
```

2. Contar los documentos de la coleccion Terra
```json
db.libros.countDocuments({editorial: "Terra"})
db.libros.countDocuments({editorial: {$eq: "Terra"}})
```

3. Mostrar todos los libros mostrando solamente la editorial
```json
db.libros.find({}, {_id: 0, editorial: 1})
```

4. Mostrar todas las distintas editoriales 
```json
db.libros.distinct({}, {_id: 0, editorial: 1})
```

[Documentacion de agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Una pipeline basica
# tienen funciones de etapa
```json
db.libros.aggregate({$match: (editorial: "Terra")})
```

## $project. Incluir y renombrar campos

```json 
db.libros.aggregate(
    {
        $match: {editorial: "Terra"}
    },
    {
        $project: {_id : 0, titulo: 1, precio: 1, NombreEditorial: "$editorial"}
    }
)
```

```json
[
  {
    $match:
      {
        editorial: "Planeta"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        NombreEditorial: "Editorial"
      }
  }
]
```

## $sort

```json

```

## $group.agrupaciones
## Cuantos documentos hay por cada una de las editoriales

```json
[
  {
    $group:
      {
        _id: "$editorial",
        NumeroDocumentos: {
          $count: {}
        }
      }
  }
]
```

Cuantos documentos hay por cada una de las editoriales por numero de documentos de manera descendentes

```json
[
  {
    $group:
      {
        _id: "$editorial",
        NumeroDocumentos: {
          $count: {}
        }
      }
  },
  {
    $sort:
      {
        NumeroDocumentos: -1
      }
  }
]
```

## agrupar por tipo de propiedad mostrando el numero de propiedades y el promedio de sus precios

```json
[
  {
    $group:
      {
        _id: "property.type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  }
]
```

Operador $set

```json
[
  {
    $group:
      {
        _id: "property.type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  }
]
```

### operador $unset

Para quitar campos ya hechos

```json
[
  {
    $group:
      {
        _id: "property.type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      "Media"
  }
]
```
# Creando nuevas colecciones utilizando el operador $out
Nota: Debe ser el ultimo en la agregacion 

```json
[
  {
    $group: {
      _id: "property.type",
      Numero: {
        $count: {}
      },
      Media: {
        $avg: "$price"
      }
    }
  },
  {
    $set: {
      Media_Total: {
        $trunc: "$Media"
      }
    }
  },
  {
    $unset: "Media"
  },
  {
    $out:
      {
        db: "sample_airbnb",
        coll: "listReviewAggregation
      }
  }
]
```

## Ejemplos con operadores de comparacion y logica

```json
[
  {
    $project:
      {
        _id: 0,
        price: 1,
        caro: {
          $gt: ["$price", 300]
        },
        medio: {
          $and: [
            {
              $gt: ["$price", 100]
            },
            {
              $lte: ["$price", 300]
            }
          ]
        },
        barato: {
          $lt: ["$price", 100]
        }
      }
  }
]
```

$cond. Devuelve valores segun una condicion (Es parecido al operador ternario condicion ? b : c)