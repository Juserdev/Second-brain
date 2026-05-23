---
Developer: Midudev
Language: React
Date: 05-05-2026
---
## useState
el useState devuelve un array
1. en la primera posicion devuelve el valor del estado
2. en la segunda posicion devuleve una funcion que nos va a permitir actualizar el estado para la nueva version
``` React
const state = useState(false)
const isFollowing = state[0]
const setIsFollowing = state[1]
```
Este es el ejemplo simple para poder entender lo que se hace, abajo pongo el ejemplo real por suerte en JS podemos hacer la desestructuracion

Entendiendo esto creamos  el useState
```React
const [isFollowing, setIsFollowing] = useState(false)
```

esto quiere decir que el primer estado del array seguien el ejemplo anterior es false, osea que ```isFollowing = fase```
ahora creamos una funcion para cambiar el estado:
```react
const handleClick = () => {
setIsFollowing(!isFollowing)
}
```
Esto quiere decirq ue cada vez que se haga click el valor inicial o sea el valor de isFollowing va a cambiar a su contrario, por ejemplo si comienza siendo **false** cambia a **True** y viceversa
ahora para que fucione lo ponemos directamente en el boton

```React
<button className={buttonclassName} onClick={handleClick}>
```
donde el onClick que lo quiere decir es que haga una funcion cuando se de click ejecute la funcion handleClick
