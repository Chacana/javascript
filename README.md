# FRONT-END

*Este articulo tiene como objetivo explicar algunos conceptos, funcionalidades y utilidades que les pueda servir para su desarrollo.*

## Contenido


1. [Loading automatico en Ajax](#loading)


### Loading en peticiones Ajax jQuery

Es probable que en tus desarrollos te hayas encontrado con la necesidad de implementar una animación de carga cada vez que realizamos una petición Ajax de jQuery. 

Algo como esto:

```javascript

function getUsers() {

  mostrarLoading();
  
  $.ajax({ 
    url: "get_user.txt", 
    success: function(result) {
      $("#div").html(result);
      ocultarLoading();
    }
  });
}

```

Si analizamos el código anterior veremos que la función getUsers() nos mostrará una animación de loading (mostrarLoading()), luego ejecutará una petición Ajax y este se ocultará cuando termine (ocultarLoading()). 

Este código funcionará correctamente pero tendríamos que repetir el llamado de mostrarLoading() y ocultarLoading() cada vez que tengamos una petición Ajax haciendo que nuestro código tenga muchas líneas duplicadas. Es por eso que les quiero enseñar la siguiente solución:

```javascript
      $.ajaxSetup({
        beforeSend: () => mostrarLoading(),
        complete: () => ocultarLoading()
    });
```

Con ajaxSetup le indicamos a Ajax un comportamiento por defecto que tendrá cada vez que se ejecute y complete una petición. En este ejemplo llamaremos a la función mostrarLoading() antes de ejecutar la petición y una vez que complete satisfactoriamente (success) se llamara la otra función de ocultarLoading().

```javascript

 $.ajaxSetup({
   beforeSend: () => mostrarLoading(),
   complete: () => ocultarLoading()
});

function getUsers() {
  $.ajax({ 
    url: "get_user.txt", 
    success: function(result) {
      $("#div").html(result);
    }
  });
}

```

Esto disparará ambas funciones de forma automatica cuando utilicemos $.ajax.

