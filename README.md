# TP Macowins

## Pseudocódigo


### Prenda

La prenda tiene un precio base y una condición (estado), que reduce o mantiene su precio.
Cada clase hija de Prenda es un tipo específico de prenda (pantalón, saco, camisa, etc). Si se agregan nuevas prendas solo deben heredar de Prenda.

```
clase abstracta Prenda{

    var precioBase
    var estado

    constructor(precioBase, estado)

    method precio(){
        return estado.modificaPrecio(precioBase)
    }

    abstract String tipo()

}

```

```
clase Pantalón hereda de Prenda  {

    method tipo(){
        return "PANTALÓN"
    }
}


clase Saco hereda de Prenda  {

    method tipo(){
        return "Saco"
    }
}


clase Camisa hereda de Prenda  {

    method tipo(){
        return "Camisa"
    }
}
```

```
interfaz Estado{
    method modificaPrecio(precioBase)
}

```

```

clase Nuevo implementa Estado{

    method modificaPrecio(precioBase){
        return precioBase
    }

}

clase Promoción implementa Estado{

    var descuento

    constructor(descuento)

    method modificaPrecio(precioBase){
        return precioBase - descuento
    }

}

clase Liquidación implementa Estado{

    method modificaPrecio(precioBase){
        return precioBase * 50%
    }

}

```

### Ganancias de un día.

Es necesario saber las ganancias de un día, eso depende de las ventas que se hayan hecho, que tienen una lista de prendas y su respectiva cantidad (Sería un `item` en la FACTURA), una fecha y la forma de pago 

```

clase Ventas{

    var ventas = []

    method gananciasDeUnDía(unaFecha){

        return ventas.filter(venta -> esDe(unaFecha))
                     .map(precioFacturado)
                     .sum()
    }
}

clase Venta{

    var items = []
    var fecha
    var formaDePago

    constructor(items, fecha formaDePago)

    method precioFacturado(){
        return formaDePago.precioFinal(items)
    }

}

clase Item{

    var prenda
    var cantidad

    constructor(prenda, cantidad)

    method total(){
        return prenda.precio() * cantidad
    }
}

```

## Formas de pago

Existen 2 formas de pago, efectivo o tarjeta.

```
interfaz FormaDePago{

    method precioFinal(items[])

}

clase Efectivo implementa FormaDePago{

    method precioFinal(items[]){
        return items.map(total)
                    .sum()
    }
}

clase Tarjeta implementa FormaDePago{

    var cuotas
    var recargo

    constructor(cuotas, recargo)

    method precioFinal(items[]){
        return items.map(calculaPrecioConIntereses).sum()
    }

    method calculaPrecioConIntereses(unItem){
        return unItem.total() + cuotas * recargo + 
               0.01 * unItem.precioDeLaPrenda()
    }
}

```

## Dudas

- ¿Es realmente necesario hacer la clase abstracta Prenda?, entiendo que cada prenda puede tener su comportamiento, y para aprovechar el polimorfismo lo ideal es que cada prenda herede de la clase asbtracta Prenda (o quizá una interfaz si fuesen solo mensajes que deben entender todos). Ahora bien, dados los requerimientos, podría haber una clase `Prenda`, con un atributo `tipo` y el método `precio()` sin más y funcionaría correctamente.


- Las clases que son una forma de pago, reciben en su método una lista de items, ya que lo necesita para poder saber el valor de cada prenda, si cambia la definicion de `Venta`, `FormaDePago` y sus clases que la implementa dejan de funcionar. ¿Esto tiene solucion o se convive con el problema?

- Crear la clase `Venta`, `Ventas` e `Item` fueron necesarias para resolver el requerimiento de las ganancias de un dia. ¿Esto incluye implementar la lógica de agregar una nueva venta?, el hecho de que sean las ganancias de un DÍA, ¿Implica que tengo registradas ventas de otros días?



