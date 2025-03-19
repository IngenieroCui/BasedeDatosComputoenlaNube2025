# Agregaciones

1. Cargar la data del json "productos.json"

Para lo anterior se usará MongoDBCompass debido a que es más rápido.

- Cuenta los productos de tipo “medio”, usando un método básico
```json
db.productos.countDocuments({tipo: "medio"})
25
```
- Indicar con un distinct, las empresas (fabricantes) que hay en la colección
```json
db.productos.distinct("fabricante");
[
  'A.O. Smith',
  'Alere',
  'American Tire Distributors Holdings',
  'Anthem',
  'Archrock',
  'Ascena Retail Group',
  'AutoNation',
  'Best Buy',
  'CIT Group',
  'Cabot',
  'Comcast',
  'Comerica',
  'Core-Mark Holding',
  'DST Systems',
  'Darling Ingredients',
  'Delta Air Lines',
  'Delta Tucker Holdings',
  "Dick's Sporting Goods",
  'First Solar',
  'HCA Holdings',
  'Hanesbrands',
  'Hartford Financial Services Group',
  'Hawaiian Holdings',
  'HealthSouth',
  'Hyatt Hotels',
  'Kar Auction Services',
  'Kelly Services',
  'Kemper',
  'Kimberly-Clark',
  'Lennar',
  'Mercury General',
  'Mondelez International',
  'Motorola Solutions',
  'Nasdaq OMX Group',
  'National Oilwell Varco',
  'Nordstrom',
  'OneMain Holdings',
  'Oneok',
  'Orbital ATK',
  'Pep Boys-Mann',
  'Pool',
  'Precision Castparts',
  'Primoris Services',
  'Raymond James Financial',
  'Seaboard',
  'Securian Financial Group',
  'Simon Property Group',
  'State Farm Insurance Cos.',
  'State Street Corp.',
  'SunPower',
  'TEGNA',
  'Telephone & Data Systems',
  'Total System Services',
  'Tractor Supply',
  'TransDigm Group',
  'Trinity Industries',
  'TrueBlue',
  'Universal American',
  'Universal Health Services',
  'WGL Holdings',
  "Wendy's",
  'Werner Enterprises',
  'WestRock',
  'Williams-Sonoma'
]
```
- Usando aggregate, visualizar los productos que tengan más de 80 unidades
```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        unidades: {
          $gt: 80
        }
      }
  }
]

db.productos.aggregate({$match: {unidades: {$gt: 80}}})
[
  {
    _id: ObjectId('67d9b752f15bd4107c14983a'),
    codigo: 0,
    nombre: 'Fantastic Wooden Fish',
    unidades: 95,
    precio: 291,
    fabricante: 'Kimberly-Clark',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId('67d9b752f15bd4107c14983c'),
    codigo: 2,
    nombre: 'Small Soft Fish',
    unidades: 96,
    precio: 189,
    fabricante: 'Primoris Services',
    tipo: 'medio'
  },
  {
    _id: ObjectId('67d9b752f15bd4107c149846'),
    codigo: 12,
    nombre: 'Refined Concrete Salad',
    unidades: 90,
    precio: 129,
    fabricante: 'Universal Health Services',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId('67d9b752f15bd4107c149858'),
    codigo: 30,
    nombre: 'Small Rubber Pants',
    unidades: 89,
    precio: 16,
    fabricante: 'Hanesbrands',
    tipo: 'basico'
  },
  {
    _id: ObjectId('67d9b752f15bd4107c14985b'),
    codigo: 33,
    nombre: 'Generic Concrete Hat',
    unidades: 82,
    precio: 70,
    fabricante: 'American Tire Distributors Holdings',
    tipo: 'basico'
  },
  {
    _id: ObjectId('67d9b752f15bd4107c14986f'),
    codigo: 53,
    nombre: 'Licensed Plastic Hat',
    unidades: 96,
    precio: 38,
    fabricante: 'Best Buy',
    tipo: 'medio'
  },
  {
    _id: ObjectId('67d9b752f15bd4107c149870'),
    codigo: 54,
    nombre: 'Generic Metal Sausages',
    unidades: 84,
    precio: 77,
    fabricante: 'DST Systems',
    tipo: 'medio'
  },
  {
    _id: ObjectId('67d9b752f15bd4107c149877'),
    codigo: 61,
    nombre: 'Sleek Rubber Keyboard',
    unidades: 82,
    precio: 33,
    fabricante: 'Alere',
    tipo: 'basico'
  },
  {
    _id: ObjectId('67d9b752f15bd4107c14987c'),
    codigo: 66,
    nombre: 'Incredible Concrete Fish',
    unidades: 96,
    precio: 336,
    fabricante: 'Darling Ingredients',
    tipo: 'medio'
  }
]
```
- Con $project visualizar solo el nombre, unidades y precio de los productos que tengan menos de 10 unidades
```json
[
  {
    $match:
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      {
        _id: 0,
        nombre: 1,
        unidades: 1,
        precio: 1
      }
  }
]

db.productos.aggregate([{$match: {unidades: {$lt: 10}}},{$project: {_id:0, nombre:1, precio:1, unidades:1}}])
[
  { nombre: 'Ergonomic Metal Ball', unidades: 5, precio: 246 },
  { nombre: 'Handmade Plastic Hat', unidades: 7, precio: 253 },
  { nombre: 'Ergonomic Metal Table', unidades: 0, precio: 94 },
  { nombre: 'Practical Frozen Chips', unidades: 0, precio: 305 },
  { nombre: 'Fantastic Metal Pants', unidades: 5, precio: 129 },
  { nombre: 'Intelligent Frozen Sausages', unidades: 3, precio: 111 },
  { nombre: 'Rustic Plastic Mouse', unidades: 5, precio: 24 }
]

```
- Con $project ponemos el fabricante pero le cambiamos el nombre por “empresa”. Usamos el mismo comando anterior
```json
[
  {
    $match:
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      {
        _id: 0,
        nombre: 1,
        unidades: 1,
        precio: 1,
        empresa: "$fabricante"
      }
  }
]

db.productos.aggregate([{$match: {unidades: {$lt: 10}}},{$project: {_id:0, nombre:1, precio:1, unidades:1, empresa: "$fabricante"}}])
[
  {
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard'
  },
  {
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods"
  },
  {
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services'
  },
  {
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines'
  },
  {
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings'
  },
  {
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith'
  },
  {
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK'
  }
]
```
- Añadir a la consulta anterior un campo calculado que se llame total y que multiplique precio por unidades.
```json
[
  {
    $match:
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      {
        _id: 0,
        nombre: 1,
        unidades: 1,
        precio: 1,
        empresa: "$fabricante",
        total: {
        	$multiply: ["$precio", "$unidades"]
        }
      }
  }
]

db.productos.aggregate([{$match: {unidades: {$lt: 10}}},{$project: {_id:0, nombre:1, precio:1, unidades:1, empresa: "$fabricante", total: {$multiply: ["$precio", "$unidades"]}}}])
[
  {
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard',
    total: 1230
  },
  {
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods",
    total: 1771
  },
  {
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services',
    total: 0
  },
  {
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines',
    total: 0
  },
  {
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings',
    total: 645
  },
  {
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith',
    total: 333
  },
  {
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK',
    total: 120
  }
]
```
- Hacer que el nombre salga en mayúsculas con el operador $toUpper
```json
{
  _id: 0,
  nombre: {
    $toUpper: "$nombre"
  },
  unidades: 1,
  precio: 1,
  empresa: "$fabricante",
  total: {
    $multiply: ["$precio", "$unidades"]
  }
}

db.productos.aggregate([{$match: {unidades: {$lt: 10}}},{$project: {_id:0, nombre: {$toUpper: "$nombre"}, precio:1, unidades:1, empresa: "$fabricante", total: {$multiply: ["$precio", "$unidades"]}}}])
[
  {
    unidades: 5,
    precio: 246,
    nombre: 'ERGONOMIC METAL BALL',
    empresa: 'Seaboard',
    total: 1230
  },
  {
    unidades: 7,
    precio: 253,
    nombre: 'HANDMADE PLASTIC HAT',
    empresa: "Dick's Sporting Goods",
    total: 1771
  },
  {
    unidades: 0,
    precio: 94,
    nombre: 'ERGONOMIC METAL TABLE',
    empresa: 'Kelly Services',
    total: 0
  },
  {
    unidades: 0,
    precio: 305,
    nombre: 'PRACTICAL FROZEN CHIPS',
    empresa: 'Delta Air Lines',
    total: 0
  },
  {
    unidades: 5,
    precio: 129,
    nombre: 'FANTASTIC METAL PANTS',
    empresa: 'OneMain Holdings',
    total: 645
  },
  {
    unidades: 3,
    precio: 111,
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    empresa: 'A.O. Smith',
    total: 333
  },
  {
    unidades: 5,
    precio: 24,
    nombre: 'RUSTIC PLASTIC MOUSE',
    empresa: 'Orbital ATK',
    total: 120
  }
]
```
- Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado con el operador $concat. Le llamamos al campo “completo”
```json
[
  {
    $match:
      {
        unidades: {
          $lt: 10
        }
      }
  },
  {
    $project:
      {
        _id: 0,
        nombre: {
          $toUpper: "$nombre"
        },
        unidades: 1,
        precio: 1,
        empresa: "$fabricante",
        total: {
          $multiply: ["$precio", "$unidades"]
        },
        completo: {
          $concat: ["$nombre", "$tipo"]
        }
      }
  }
]

db.productos.aggregate([{$match: {unidades: {$lt: 10}}},{$project: {_id:0, nombre: {$toUpper: "$nombre"}, precio:1, unidades:1, empresa: "$fabricante", total: {$multiply: ["$precio", "$unidades"]}, completo: {$concat: ["$nombre", "$tipo"]}}}])
[
  {
    unidades: 5,
    precio: 246,
    nombre: 'ERGONOMIC METAL BALL',
    empresa: 'Seaboard',
    total: 1230,
    completo: 'Ergonomic Metal Ballmedio'
  },
  {
    unidades: 7,
    precio: 253,
    nombre: 'HANDMADE PLASTIC HAT',
    empresa: "Dick's Sporting Goods",
    total: 1771,
    completo: 'Handmade Plastic Hatmedio'
  },
  {
    unidades: 0,
    precio: 94,
    nombre: 'ERGONOMIC METAL TABLE',
    empresa: 'Kelly Services',
    total: 0,
    completo: 'Ergonomic Metal Tableavanzado'
  },
  {
    unidades: 0,
    precio: 305,
    nombre: 'PRACTICAL FROZEN CHIPS',
    empresa: 'Delta Air Lines',
    total: 0,
    completo: 'Practical Frozen Chipsmedio'
  },
  {
    unidades: 5,
    precio: 129,
    nombre: 'FANTASTIC METAL PANTS',
    empresa: 'OneMain Holdings',
    total: 645,
    completo: 'Fantastic Metal Pantsbasico'
  },
  {
    unidades: 3,
    precio: 111,
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    empresa: 'A.O. Smith',
    total: 333,
    completo: 'Intelligent Frozen Sausagesbasico'
  },
  {
    unidades: 5,
    precio: 24,
    nombre: 'RUSTIC PLASTIC MOUSE',
    empresa: 'Orbital ATK',
    total: 120,
    completo: 'Rustic Plastic Mouseavanzado'
  }
]
```
- Ordena el resultado por el campo “total”
```json
[
  {
    $match: {
      unidades: {
        $lt: 10
      }
    }
  },
  {
    $project: {
      _id: 0,
      nombre: {
        $toUpper: "$nombre"
      },
      unidades: 1,
      precio: 1,
      empresa: "$fabricante",
      total: {
        $multiply: ["$precio", "$unidades"]
      },
      completo: {
        $concat: ["$nombre", "$tipo"]
      }
    }
  },
  {
    $sort:
      {
        total: 1
      }
  }
]

db.productos.aggregate([{$match: {unidades: {$lt: 10}}},{$project: {_id:0, nombre: {$toUpper: "$nombre"}, precio:1, unidades:1, empresa: "$fabricante", total: {$multiply: ["$precio", "$unidades"]}, completo: {$concat: ["$nombre", "$tipo"]}}}, {$sort: {total: 1}}])
[
  {
    unidades: 0,
    precio: 94,
    nombre: 'ERGONOMIC METAL TABLE',
    empresa: 'Kelly Services',
    total: 0,
    completo: 'Ergonomic Metal Tableavanzado'
  },
  {
    unidades: 0,
    precio: 305,
    nombre: 'PRACTICAL FROZEN CHIPS',
    empresa: 'Delta Air Lines',
    total: 0,
    completo: 'Practical Frozen Chipsmedio'
  },
  {
    unidades: 5,
    precio: 24,
    nombre: 'RUSTIC PLASTIC MOUSE',
    empresa: 'Orbital ATK',
    total: 120,
    completo: 'Rustic Plastic Mouseavanzado'
  },
  {
    unidades: 3,
    precio: 111,
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    empresa: 'A.O. Smith',
    total: 333,
    completo: 'Intelligent Frozen Sausagesbasico'
  },
  {
    unidades: 5,
    precio: 129,
    nombre: 'FANTASTIC METAL PANTS',
    empresa: 'OneMain Holdings',
    total: 645,
    completo: 'Fantastic Metal Pantsbasico'
  },
  {
    unidades: 5,
    precio: 246,
    nombre: 'ERGONOMIC METAL BALL',
    empresa: 'Seaboard',
    total: 1230,
    completo: 'Ergonomic Metal Ballmedio'
  },
  {
    unidades: 7,
    precio: 253,
    nombre: 'HANDMADE PLASTIC HAT',
    empresa: "Dick's Sporting Goods",
    total: 1771,
    completo: 'Handmade Plastic Hatmedio'
  }
]
```
- Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto
```json
[
  {
    $group: {
      _id: "$tipo",
      totalProducto: {
        $sum: 1
      }
    }
  }
]

db.productos.aggregate({$group: {_id: "$tipo", totalProducto: {$sum: 1}}})
[
  { _id: 'avanzado', totalProducto: 18 },
  { _id: 'medio', totalProducto: 25 },
  { _id: 'basico', totalProducto: 24 }
]
```
- Añadir el valor mayor y el menor
```json
[
  {
    $group: {
      _id: "$tipo",
      totalProducto: {
        $sum: 1
      },
      mayor: {
        $max: "$precio"
      },
      menor: {
        $min: "$precio"
      }
    }
  }
]

db.productos.aggregate({$group: {_id: "$tipo", totalProducto: {$sum: 1}, mayor: {$max: "$precio"}, menor: {$min: "$precio"}}})
[
  { _id: 'avanzado', totalProducto: 18, mayor: 331, menor: 18 },
  { _id: 'basico', totalProducto: 24, mayor: 285, menor: 16 },
  { _id: 'medio', totalProducto: 25, mayor: 337, menor: 16 }
]
```
- Añade el total de unidades por cada tipo
```json
[
  {
    $group: {
      _id: "$tipo",
      totalProducto: {
        $sum: 1
      },
      mayor: {
        $max: "$precio"
      },
      menor: {
        $min: "$precio"
      },
      totalUnidades: {
        $sum: "$unidades"
      }
    }
  }
]

db.productos.aggregate({$group: {_id: "$tipo", totalProducto: {$sum: 1}, mayor: {$max: "$precio"}, menor: {$min: "$precio"}, totalUnidades: {$sum: "$unidades"}}})
[
  {
    _id: 'medio',
    totalProducto: 25,
    mayor: 337,
    menor: 16,
    totalUnidades: 1224
  },
  {
    _id: 'basico',
    totalProducto: 24,
    mayor: 285,
    menor: 16,
    totalUnidades: 1067
  },
  {
    _id: 'avanzado',
    totalProducto: 18,
    mayor: 331,
    menor: 18,
    totalUnidades: 773
  }
]
```
- Con el operador $set y el operador “$substr” visualiza todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre.
```json
[
  {
    $match:
      {
        nombre: "Small Metal Tuna"
      }
  },
  {
    $set: {
      primeros5Char: {
        $substr: ["$nombre", 0, 5]
      }
    }
  }
]

db.productos.aggregate([{$match: {nombre: 'Small Metal Tuna'}},{$set: {primeros5Chars: {$substr: ["$nombre", 0, 5]}}}])
[
  {
    _id: ObjectId('67d9b752f15bd4107c149871'),
    codigo: 55,
    nombre: 'Small Metal Tuna',
    unidades: 46,
    precio: 43,
    fabricante: 'Raymond James Financial',
    tipo: 'medio',
    primeros5Chars: 'Small'
  }
]
```
- Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y lo guardamos en una colección denominada productos2
```json
[
  {
    $project:
      {
        _id: 0,
        nombre: 1,
        total: {
          $multiply: ["$precio", "$unidades"]
        }
      }
  },
  {
    $out: {
      db: "chambaGallardo",
      coll: "productos2"
    }
  }
]

db.productos.aggregate([{$project: {_id: 0, nombre: 1, total: {$multiply: ["$precio", "$unidades"]}}}, {$out: {db: "chambaGallardo", coll: "productos2"}}])
```
- Comprobamos que se ha creado
```json
show collections
personas
productos
productos2
```
- Hacemos un find para comprobar el resultado
```json
db.productos2.find()
[
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911b8'),
    nombre: 'Fantastic Wooden Fish',
    total: 27645
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911b9'),
    nombre: 'Rustic Concrete Pants',
    total: 18414
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911ba'),
    nombre: 'Small Soft Fish',
    total: 18144
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911bb'),
    nombre: 'Practical Soft Pants',
    total: 2747
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911bc'),
    nombre: 'Ergonomic Metal Ball',
    total: 1230
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911bd'),
    nombre: 'Small Steel Gloves',
    total: 1665
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911be'),
    nombre: 'Ergonomic Wooden Shirt',
    total: 14994
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911bf'),
    nombre: 'Handmade Steel Chair',
    total: 5392
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c0'),
    nombre: 'Handcrafted Soft Gloves',
    total: 4512
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c1'),
    nombre: 'Fantastic Concrete Salad',
    total: 12985
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c2'),
    nombre: 'Handmade Plastic Hat',
    total: 1771
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c3'),
    nombre: 'Refined Wooden Tuna',
    total: 8480
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c4'),
    nombre: 'Refined Concrete Salad',
    total: 11610
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c5'),
    nombre: 'Unbranded Soft Fish',
    total: 9434
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c6'),
    nombre: 'Small Concrete Fish',
    total: 5040
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c7'),
    nombre: 'Refined Concrete Bike',
    total: 2700
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c8'),
    nombre: 'Tasty Cotton Pants',
    total: 3536
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911c9'),
    nombre: 'Incredible Granite Gloves',
    total: 20590
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911ca'),
    nombre: 'Practical Metal Mouse',
    total: 6650
  },
  {
    _id: ObjectId('67d9f88ddc9f7016fb2911cb'),
    nombre: 'Handcrafted Steel Chicken',
    total: 6215
  }
]
```
- Usando $cond y $project vamos a visualizar el nombre del producto, el precio y un campo llamado valoración que ponga “barato” si el precio es menor de 250 y caro si es mayor o igual
```json
[
  {
    $project:
      {
        _id: 0,
        nombre: 1,
        precio: 1,
        valoracion: {
          $cond: [
            {
              $lt: ["$precio", 250]
            },
            "Barato",
            "Caro"
          ]
        }
      }
  }
]

db.productos.aggregate({$project: {_id: 0, nombre: 1, precio: 1, valoracion: {$cond: [{$lt: ["$precio", 250]}, "Barato", "Caro"]}}})
[
  { nombre: 'Fantastic Wooden Fish', precio: 291, valoracion: 'Caro' },
  { nombre: 'Rustic Concrete Pants', precio: 279, valoracion: 'Caro' },
  { nombre: 'Small Soft Fish', precio: 189, valoracion: 'Barato' },
  { nombre: 'Practical Soft Pants', precio: 67, valoracion: 'Barato' },
  { nombre: 'Ergonomic Metal Ball', precio: 246, valoracion: 'Barato' },
  { nombre: 'Small Steel Gloves', precio: 37, valoracion: 'Barato' },
  {
    nombre: 'Ergonomic Wooden Shirt',
    precio: 238,
    valoracion: 'Barato'
  },
  { nombre: 'Handmade Steel Chair', precio: 337, valoracion: 'Caro' },
  {
    nombre: 'Handcrafted Soft Gloves',
    precio: 282,
    valoracion: 'Caro'
  },
  {
    nombre: 'Fantastic Concrete Salad',
    precio: 265,
    valoracion: 'Caro'
  },
  { nombre: 'Handmade Plastic Hat', precio: 253, valoracion: 'Caro' },
  { nombre: 'Refined Wooden Tuna', precio: 212, valoracion: 'Barato' },
  {
    nombre: 'Refined Concrete Salad',
    precio: 129,
    valoracion: 'Barato'
  },
  { nombre: 'Unbranded Soft Fish', precio: 178, valoracion: 'Barato' },
  { nombre: 'Small Concrete Fish', precio: 126, valoracion: 'Barato' },
  {
    nombre: 'Refined Concrete Bike',
    precio: 180,
    valoracion: 'Barato'
  },
  { nombre: 'Tasty Cotton Pants', precio: 52, valoracion: 'Barato' },
  {
    nombre: 'Incredible Granite Gloves',
    precio: 290,
    valoracion: 'Caro'
  },
  {
    nombre: 'Practical Metal Mouse',
    precio: 190,
    valoracion: 'Barato'
  },
  {
    nombre: 'Handcrafted Steel Chicken',
    precio: 113,
    valoracion: 'Barato'
  }
]
Type "it" for more
chambaGallardo> it
[
  { nombre: 'Ergonomic Metal Table', precio: 94, valoracion: 'Barato' },
  { nombre: 'Tasty Wooden Ball', precio: 76, valoracion: 'Barato' },
  { nombre: 'Unbranded Soft Table', precio: 284, valoracion: 'Caro' },
  { nombre: 'Handcrafted Metal Tuna', precio: 320, valoracion: 'Caro' },
  {
    nombre: 'Incredible Frozen Shirt',
    precio: 173,
    valoracion: 'Barato'
  },
  {
    nombre: 'Licensed Fresh Chicken',
    precio: 233,
    valoracion: 'Barato'
  },
  {
    nombre: 'Intelligent Plastic Bike',
    precio: 241,
    valoracion: 'Barato'
  },
  { nombre: 'Sleek Steel Chicken', precio: 131, valoracion: 'Barato' },
  {
    nombre: 'Handcrafted Granite Cheese',
    precio: 304,
    valoracion: 'Caro'
  },
  {
    nombre: 'Unbranded Soft Computer',
    precio: 69,
    valoracion: 'Barato'
  },
  { nombre: 'Small Rubber Pants', precio: 16, valoracion: 'Barato' },
  {
    nombre: 'Generic Fresh Keyboard',
    precio: 16,
    valoracion: 'Barato'
  },
  { nombre: 'Sleek Soft Towels', precio: 68, valoracion: 'Barato' },
  { nombre: 'Generic Concrete Hat', precio: 70, valoracion: 'Barato' },
  {
    nombre: 'Practical Cotton Keyboard',
    precio: 25,
    valoracion: 'Barato'
  },
  { nombre: 'Sleek Frozen Hat', precio: 110, valoracion: 'Barato' },
  { nombre: 'Rustic Soft Table', precio: 331, valoracion: 'Caro' },
  { nombre: 'Awesome Soft Gloves', precio: 18, valoracion: 'Barato' },
  { nombre: 'Rustic Steel Shoes', precio: 257, valoracion: 'Caro' },
  { nombre: 'Handmade Soft Chips', precio: 208, valoracion: 'Barato' }
]
Type "it" for more
chambaGallardo> it
[
  { nombre: 'Handmade Frozen Salad', precio: 85, valoracion: 'Barato' },
  { nombre: 'Awesome Cotton Cheese', precio: 277, valoracion: 'Caro' },
  { nombre: 'Licensed Granite Ball', precio: 92, valoracion: 'Barato' },
  { nombre: 'Sleek Frozen Table', precio: 77, valoracion: 'Barato' },
  { nombre: 'Sleek Steel Ball', precio: 236, valoracion: 'Barato' },
  { nombre: 'Fantastic Soft Pants', precio: 103, valoracion: 'Barato' },
  {
    nombre: 'Intelligent Metal Bike',
    precio: 141,
    valoracion: 'Barato'
  },
  { nombre: 'Practical Frozen Chips', precio: 305, valoracion: 'Caro' },
  { nombre: 'Refined Plastic Hat', precio: 81, valoracion: 'Barato' },
  {
    nombre: 'Licensed Concrete Soap',
    precio: 68,
    valoracion: 'Barato'
  },
  { nombre: 'Awesome Frozen Shoes', precio: 125, valoracion: 'Barato' },
  {
    nombre: 'Fantastic Metal Pants',
    precio: 129,
    valoracion: 'Barato'
  },
  { nombre: 'Ergonomic Metal Hat', precio: 85, valoracion: 'Barato' },
  { nombre: 'Licensed Plastic Hat', precio: 38, valoracion: 'Barato' },
  {
    nombre: 'Generic Metal Sausages',
    precio: 77,
    valoracion: 'Barato'
  },
  { nombre: 'Small Metal Tuna', precio: 43, valoracion: 'Barato' },
  {
    nombre: 'Intelligent Frozen Sausages',
    precio: 111,
    valoracion: 'Barato'
  },
  { nombre: 'Practical Fresh Fish', precio: 331, valoracion: 'Caro' },
  {
    nombre: 'Refined Concrete Shirt',
    precio: 49,
    valoracion: 'Barato'
  },
  {
    nombre: 'Handcrafted Granite Chicken',
    precio: 164,
    valoracion: 'Barato'
  }
]
Type "it" for more
chambaGallardo> it
[
  { nombre: 'Practical Frozen Salad', precio: 250, valoracion: 'Caro' },
  { nombre: 'Sleek Rubber Keyboard', precio: 33, valoracion: 'Barato' },
  { nombre: 'Ergonomic Steel Fish', precio: 94, valoracion: 'Barato' },
  { nombre: 'Rustic Plastic Mouse', precio: 24, valoracion: 'Barato' },
  { nombre: 'Awesome Cotton Mouse', precio: 285, valoracion: 'Caro' },
  {
    nombre: 'Intelligent Soft Towels',
    precio: 93,
    valoracion: 'Barato'
  },
  {
    nombre: 'Incredible Concrete Fish',
    precio: 336,
    valoracion: 'Caro'
  }
]
```