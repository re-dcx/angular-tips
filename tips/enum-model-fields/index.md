# Enums de campos de un dto

Cuando se defina una interfaz de dto, se debe definir también un enum con los campos del mismo. Por ejemplo el siguient dto:

```ts
export interface Customer {
  id: number;
  name: string;
  customer_image_path: string;
  customer_type: CustomerTypeEnum;
}
```

irá acompañado de este enum:
```ts
export enum CustomerFields {
  Id = 'id',
  Name = 'name',
  CustomerImagePath = 'customer_image_path',
  CustomerType = 'customer_type',
}
```

Este enum puede ser útil en los siguientes contextos:

## En un parametro `RequestParam`
Podemos darles valores a un `RequestParam` sin miedo a cometer erratas:
```ts
// Antes:
// param = new RequestParam({
//     query: {
//       fields: ['name'],
//       value: '',
//     },   
//     order: [{ field: 'name', type: 'asc' } as RequestOrder],


// Ahora
param = new RequestParam({
    query: {
      fields: [CustomerFields.Name],
      value: '',
    },   
    order: [{ field: CustomerFields.Name, type: 'asc' } as RequestOrder],
    //...
  });
```

## En formularios
Podemos utilizar enums para incializar nuestros formularios de igual forma sin temor a erratas. Hay que destacar que es necesario encerrar `CustomerFields.Name` entre corchetes por que de otra forma el compilador de typescript nos devolverá un error.

```ts

// Antes
// public form = this.fb.group({
//     'name': [null, [Validators.required]],
//   });

// Ahora
public form = this.fb.group({
    [CustomerFields.Name]: [null, [Validators.required]],
  });
```


## En la plantilla de un componente
Y también podemos utilizarlos enums desde la plantilla de un componente

```html
<!-- Antes -->
<!-- <ng-container [matColumnDef]="'name'">
    <th mat-header-cell *matHeaderCellDef mat-sort-header >NOMBRE</th>
    <td mat-cell *matCellDef="let row">
      {{ row.name }}
    </td>
</ng-container> -->

<!-- Ahora -->
<ng-container [matColumnDef]="CustomerFields.Name">
    <th mat-header-cell *matHeaderCellDef mat-sort-header >NOMBRE</th>
    <td mat-cell *matCellDef="let row">
      {{ row[CustomerFields.Name] }}
    </td>
</ng-container>

```

En este caso será necesario hacer un pequeño cambio el `ts` para que el enum que quede accesible desde el `html`. Puedes echarle un vistazo al [consejo para usar enums desde una plantilla](../how-to-use-enums-from-template/index.md)