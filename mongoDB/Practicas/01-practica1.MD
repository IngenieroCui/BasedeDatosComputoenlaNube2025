# Practica 1: Base de datos, colecciones e inserts

1. Conectarnos con mongosh a MongoDB.
2. Crear una base de datos llamada curso.
3. Comprobar que la base de datos no exista.
4. Crear una coleccion que se llame facturas y mostrarla.

```json
db.createCollection("Facturas") 
show collections
```

5. Insertar un documento con los siguientes datos:

| Codigo     | Valor    |
|---------------|---------------|
| Cod_factura | 10 |
| Cliente | Frutas Ramirez |
| Total| 223 |

| Codigo     | Valor    |
|---------------|---------------|
| Cod_factura | 20 |
| Cliente | Ferreteria Juan |
| Total| 140 |

```json
db.Facturas.insertOne( 
{
    cod_Factura: 10, 
    cliente: "Frutas Ramirez", 
    total: 223
})

db.Facturas.insertOne( 
{
    cod_Factura: 20, 
    cliente: "Ferreteria Juan", 
    total: 140
})
```

6. Crear una nueva coleccion pero usando directamente el insertOne.
Insertar un documento en la coleccion productos con los siguientes datos.

| Codigo     | Valor    |
|---------------|---------------|
| Cod_producto | 1 |
| Nombre | Tornillo x 1" |
| Precio | 2 |
| Unidades | 1500 |

```json
db.productos.insertOne( 
{ 
    cod_producto: 1, 
    nombre: "Tornillo x 1'' ", 
    precio: 2, 
    unidades: 1500 
} )
```

7. Crear un nuevo documento de producto que contenga un array con los siguientes datos:

| Codigo     | Valor    |
|---------------|---------------|
| Cod_producto | 2 |
| Nombre | Martillo |
| Precio | 20 |
| Unidades | 50 |
| Fabricantes | fab1, fab2, fab3, fab4 |

```json
db.productos.insertOne({ 
    cod_producto: 2, 
    nombre:'Martillo', 
    precio: 20, 
    unidades: 50, 
    fabricantes: ["fab1","fab2","fab3","fab4"]})
```

8. Borrar la coleccion Facturas y comprobar que se borro.
```json
db.Facturas.drop()
show collections
```

9. Insertar un documento en una coleccion denominada **fabricantes** para probar los subdocumentos y la clave _id personalizada.

| Codigo     | Valor    |
|---------------|---------------|
| id | 1 |
| Nombre | fab1 |
| Localidad | ciudad: Buenos aires, pais: Argentina, Calle: Calle pez 27, Cod_postal: 2900 |

```json
db.fabricantes.insertOne({
  _id: 1,
  Nombre: "fab1",
  Localidad: {
    ciudad: "Buenos Aires",
    pais: "Argentina",
    Calle: "Calle Pez 27",
    Cod_postal: 2900
  }
})
```

9. insertMany


| Codigo     | Valor    |
|---------------|---------------|
| Cod_producto | 3 |
| Nombre | Alicates |
| Precio | 10 |
| Unidades | 25 |
| Fabricantes | fab1, fab2, fab5 |

| Codigo     | Valor    |
|---------------|---------------|
| Cod_producto | 4 |
| Nombre | Arandela |
| Precio | 1 |
| Unidades | 500 |
| Fabricantes | fab2, fab3, fab4 |

