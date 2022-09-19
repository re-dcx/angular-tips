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
```ts
public form = this.fb.group({
    [CustomerFields.Name]: [null, [Validators.required]],
  });
```


```html
<ng-container [matColumnDef]="CustomerFields.Name">
    <th mat-header-cell *matHeaderCellDef mat-sort-header >NOMBRE</th>
    <td mat-cell *matCellDef="let row">
      {{ row.name }}
    </td>
</ng-container>
```