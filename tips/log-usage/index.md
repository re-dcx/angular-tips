# Uso recomendado de `log`

Si estamos utilizando un proyecto generado a partir de [ngx-rocket](https://github.com/ngx-rocket/generator-ngx-rocket) posiblemente estemos utilizando logs de la siguiente forma:

```ts
//...
import { Logger } from '@app/@core';

const log = new Logger('TemplatesConfigurationComponent');

@Component({
  selector: 'app-templates-configuration',
  templateUrl: './templates-configuration.component.html',
  styleUrls: ['./templates-configuration.component.scss']
})
export class TemplatesConfigurationComponent {
//...
```

Esto tiene el inconveniente de que si renombramos nuestro componente se nos puede olvidar cambiar el texto dentro de `Logger('TemplatesConfigurationComponent')`. 

Esto puede llegar a ser confuso si, por ejemplo, creamos un componente a partir de otro copiando su código y renombrando las clases. En tal caso puede suceder que los logs recibidos parezcan llegar del componente equivocado.

Para evitar este problema es mejor utilizar este sistema para inicializar el log:
```ts
//...
import { Logger } from '@app/@core';

@Component({
  selector: 'app-templates-configuration',
  templateUrl: './templates-configuration.component.html',
  styleUrls: ['./templates-configuration.component.scss']
})
export class TemplatesConfigurationComponent {
  //...
  log = new Logger(TemplatesConfigurationComponent.name)
  //...
```

Siguiendo este sistema, los logs siempre mostrará el nombre de la clase desde donde se está llamando.

Hay que tener en cuenta que al usar este sistema, ahora se debe que poner `this.` antes de `log`. Osea si antes se ponía:


```ts 
log.info('template loaded successful');
```

Ahora habrá que poner algo como:

```ts 
this.log.info('template loaded successful');
```
