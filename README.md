# bubble-trouble


// Para instalar en local //

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

// Para compilar el proyecto //

### Compiles and minifies for production
```
npm run build
```

//

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).


// Comentarios //
Tareas hechas
 - Carga de las instancias (input y output)
 - Visualización de la instancia en el gráfico
 - Clic en la burbuja para cambiar de color. Seleccionar color de paleta de colores cargada con los colores totales del output.json, ya que no se debe usar más colores.
 - Mensaje de alerta si el color que intentas cambiar lo tiene algún bubble linkado
 - Visualización del número total de colores.


Tareas TO-DO
 - Selección de nivel y número de burbujas. Actualmente para cambiar el nivel deberemos mover el JSON a la carpeta public, sobreescribiendo el actual.
 - Modificar los cambios en el output.json
 - Ampliar, reducir el gráfico. Mejorar la vista (El nivel de 50 Bubbles se ve muy junto).
 - Scroll para movernos por el gráfico pudiendo acercar, alejar.
 - Optimización de métodos en el ciclo de vida de las vistas de VUE
 - Almacenar colores en constantes
 - Control de errores. El código no está absento de errores.
 - Refactorización de código


// Descripción de uso //
El navegador carga directamente el gráfico, con el número total de colores utilizados. 
En el gráfico hay un conjunto de burbujas que hay que colorear según un criterio. 
El criterio es que dos burbujas que están "vinculadas", no pueden compartir el mismo color. 
La idea es elegir el menor número de colores para colorear todas las burbujas. 