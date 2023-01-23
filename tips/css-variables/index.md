# Budgets de la aplicación y variables css

En la construcción de una app angular se hace una serie de comprobaciones relativas al tamaño total de la aplicación y del tamaño individual de los estilos componentes. Esto tamaños están fijados en el fichero `angular.json` y podrían tener unos valores como sigue
```json
...
  "budgets": [
    {
      "type": "initial",
      "maximumWarning": "2mb",
      "maximumError": "5mb"
    },
    {
      "type": "anyComponentStyle",
      "maximumWarning": "10kb",
      "maximumError": "20kb"
    }
  ]  
```
    
`initial` es para definir los budgets de la construcción total de la aplicación y `anyComponentStyle` se refiere al tamaño individual de los estilos de los componentes.

Para evitar que nos aparezcan warnings y errores cuando construimos la aplicación es buena idea evitar utilizar `@import` en los estilos de los componentes. 

Si se necesita hacer uso de alguna función que requiera de `@import` podemos llevarnos esa lógica a `main.scss` y dejarla en una [variable css](https://www.w3schools.com/css/css3_variables.asp)

Por ejemplo si en un componente tuviésemos el siguiente código en el estilo de un componente
```scss
  // foo.component.scss

  @use "@angular/material" as mat;

  .green-chip {
    background-color: mat.get-color-from-palette($mat-green, 500);
  }
```

Podríamos añadir el siguiente código a `main.scss` el siguiente código:
- En `main.scss`:
```scss
  @import "/src/main.scss";
  // ...
  :root {
    // ...
    --mat-green-500: #{mat.get-color-from-palette($mat-green, 500)};
  }
```

- En `foo.scss`:
```scss
  .green-chip {
    background-color: var(--mat-green-500) !important;
  }
```

Tal como se puede intuir es necesario definir estas variables css dentro de un bloque `:root` para que puedan ser accesibles desde cualquier scss de nuestra aplicación.

Haciendo uso de variables css y evitando usar `@import` en los estilos de los componentes podremos evitar que el tamaño de la aplicación crezca con cada componente que añadamos y evitaremos problemas con los budgets en los despliegues