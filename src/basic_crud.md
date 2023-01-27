1. Conectarse a MongoDB
```sh
mongo <URI Atlas>
```


2. Usar la base de datos creada de platzi
```sh
use <databse>
```

*El campo _id si no es agregado por nosotros de forma explícita, MongoDB lo agrega por nosotros como un ObjectId*
*El campo _id es obligatorio en todos los documentos*


# Create
We are gonna name our collection as "inventory"
```sh
db.inventory.insertOne(
   { item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
)
```

```sh
db.inventory.insertMany( [
   { item: "canvas", qty: 100, size: { h: 28, w: 35.5, uom: "cm" }, status: "A" },
   { item: "journal", qty: 25, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "mat", qty: 85, size: { h: 27.9, w: 35.5, uom: "cm" }, status: "A" },
   { item: "mousepad", qty: 25, size: { h: 19, w: 22.85, uom: "cm" }, status: "P" },
   { item: "notebook", qty: 50, size: { h: 8.5, w: 11, uom: "in" }, status: "P" },
   { item: "paper", qty: 100, size: { h: 8.5, w: 11, uom: "in" }, status: "D" },
   { item: "planner", qty: 75, size: { h: 22.85, w: 30, uom: "cm" }, status: "D" },
   { item: "postcard", qty: 45, size: { h: 10, w: 15.25, uom: "cm" }, status: "A" },
   { item: "sketchbook", qty: 80, size: { h: 14, w: 21, uom: "cm" }, status: "A" },
   { item: "sketch pad", qty: 95, size: { h: 22.85, w: 30.5, uom: "cm" }, status: "A" }
] )
```

```sh
db.inventory.find( { item: "canvas" } )
```

```sh
db.inventory.insertOne(
   { _id: "one", item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
)
```

*Atomicidad, todas las operaciones de escritura en MongoDB con atómicas a nivel de documentos*


# Read
```sh
db.inventory.find( {} )
```

## Igualdad 
```sh
db.inventory.find( { status: "D" } )
```

## Operadores
```sh
db.inventory.find( { qty: { $gt: 30 } } )
```

## Condición AND
```sh
db.inventory.find( { status: "A", qty: { $lt: 30 } } )
```

## Condición OR con operador
```sh
db.inventory.find( { $or: [ { status: "A" }, { qty: { $lt: 30 } } ] } )
```

## Trae el primer documento que cumpla con el filtro de acuerdo al orden natural en que los documentos se encuentren guardados en disco
```sh
db.inventory.findOne( { status: "A", qty: { $lt: 30 } } )
```

# Update
## Update One
```sh
db.inventory.updateOne(
   { item: "paper" },
   {
     $set: { "size.uom": "cm", status: "P" },
     $currentDate: { lastModified: true }
   })
```

## Update Many
```sh
db.inventory.find({qty: {$lt: 50}})
```
```sh
db.inventory.updateMany(
   { "qty": { $lt: 50 } },
   {
     $set: { "size.uom": "in", status: "P" },
     $currentDate: { lastModified: true }
   }
)
```
```sh
db.inventory.find({qty: {$lt: 50}})
```

## Reemplazar un documento y conservar su _id
```sh
db.inventory.find({item: "paper"})
```
```sh
db.inventory.replaceOne(
   { item: "paper" },
   { item: "paper", instock: [ { warehouse: "A", qty: 60 }, { warehouse: "B", qty: 40 } ] }
)
```
```sh
db.inventory.find({item: "paper"})
```
# Delete
```sh
db.inventory.find({status: "A"})
```
## Borrar muchos documentos de acuerdo a un filtro
```sh
db.inventory.deleteMany({ status : "A" })
```
```sh
db.inventory.find({status: "D"})
```

## Borrar un documento
```sh
db.inventory.deleteOne( { status: "D" } )
```

## Borrar todos los documentos de una base datos
```sh
db.inventory.deleteMany({})
```