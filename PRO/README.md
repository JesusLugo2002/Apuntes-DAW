# Resumen

<div align=center>
<img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExMmswYzNmeWVtaDY2b3p6ajBoMXh6NHNtbWp0bGJncGhkNG13NzE1ayZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/lgaHEvyUjdHY4/giphy.gif"/>
<br>
Aquí mis apuntes de programación.
</div>

<div align=justify>

## Índice

1. [Enlaces importantes](#enlaces-importantes)
2. [Glosario](#glosario)
3. [Tipos de datos](#tipos-de-datos)
4. [Control de flujo](#control-de-flujo)
5. [Estructuras de datos](#estructuras-de-datos)
6. [Modularidad](#modularidad)

## Enlaces importantes

## Glosario

## Tipos de datos

## Control de flujo

## Estructuras de datos

## Modularidad

### Funciones

```py
# Definición de la función
def function_name(param1, param2, paramN):
    return values

# Llamada de la función
function_name(arg1, arg2, argN)
```
> [!NOTE]
> Los parámetros pueden ser __posicionales__ (se ingresan de forma normal, ejemplo: `say_name('Juan')`) o __nominales__ (se ingresan llamando al nombre del parametro antes, ejemplo: `say_name(name='Juan')`)

> [!NOTE]
> Una vez se empieza el ingreso de argumentos __nominales__, los siguientes deberán serlo también.

> [!NOTE]
> Para volver obligatorios que los argumentos sean posicionales o nominales, se debe colocar __/__ (esto dirá que los parametros __antes__ del símbolo serán siempre __posicionales__) o __*__ (los parámetros __después__ del símbolo serán siempre __nominales__)

> [!NOTE]
> Es preferible explicitar el `return None` siempre que se pueda.

> [!NOTE]
> Para darle un valor por defecto a algún parámetro, en la __definición de la función__ se le debe asignar con `param_name=default_value`.

> [!WARNING]
> NO es buena práctica modificar los valores ingresados en una función; se debe optar por generar un nuevo valor en base al ingresado.

> [!WARNING]
> Siempre que se trabaje con datos mutables, como listas, estás se deben inicializar dentro de la función como una variable y no como un parámetro, pues le puede dar un ictus y funcionar como le salga de la gana.

> [!NOTE]
> Se puede empaquetar/desempaquetar valores en tuplas o diccionarios añadiendo a los argumentos y/o parámetros un __*__ (de argumentos posicionales a tupla) o __**__ (de argumentos nominales a diccionarios).

> [!NOTE]
> Las funciones mismas también pueden ser argumentos de otras funciones.

</div>

</div>