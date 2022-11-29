# Formularios en angular 14

Angular 14 incorpora [un nuevo sistema para poder asignar tipos en los formularios.](https://angular.io/guide/typed-forms)

Hasta ahora, cuando se creaba un formulario

```ts

// without FormBuilder
form = new FormGroup({
    [InvitationCreationParamFields.phone_number]: new FormControl(null, [Validators.required]),
    [InvitationCreationParamFields.role]: new FormControl(null, [Validators.required]),
    [InvitationCreationParamFields.patients_ids]: new FormControl(<number[]>[], [])
});

// with FormBuilder
form = this.formBuilder({
    [InvitationCreationParamFields.phone_number]: [null, [Validators.required]],
    [InvitationCreationParamFields.role]: [null, [Validators.required]],
    [InvitationCreationParamFields.patients_ids]: [[], []],
  });

```



El nuevo sistema en angular 14, sería algo como:
```ts
// without FormBuilder
form = new FormGroup({
    [InvitationCreationParamFields.phone_number]: new FormControl<string>(null, [Validators.required]),
    [InvitationCreationParamFields.role]: new FormControl<UserGroupEnum>(null, [Validators.required]),
    [InvitationCreationParamFields.patients_ids]: new FormControl<number[]>(<number[]>[], []),
  });

// with FormBuilder
form = this.formBuilder.group<{
  [InvitationCreationParamFields.phone_number]: FormControl<string>,
  [InvitationCreationParamFields.role]: FormControl<UserRoleEnum>,
  [InvitationCreationParamFields.patients_ids]: FormControl<number[]>,
}>({
    [InvitationCreationParamFields.phone_number]: [null, [Validators.required]],
    [InvitationCreationParamFields.role]: [null, [Validators.required]],
    [InvitationCreationParamFields.patients_ids]: [<number[]>[], []],
  });
```

Como se puede observar las únicas diferencias es que se han añadido los tipos que tiene cada control, y que la formula con FormBuilder es demasiado larga (Por lo cual, de momento, lo recomendable será usar la versión sin FormBuilder). A parte como podréis adivinar ahora se puede realizar las siguientes cosas;

- `form.value` devuelve una valor tipado en lugar de `any`. Lo que nos permitirá poder acceder por ejemplo a `form.value.phone_number` directamente sin necesidad de hacer un cast de hacer ningún cast.

- Podemos acceder a un control de un form usando por ejemplo `form.controls.phone_number`, ya no será necesario hacer cosas del estilo de `form.controls[InvitationCreationParamFields.phone_number]` en este contexto.

- VSCode es capaz de sugerir las propiedades de `form.value` y `form.controls`. Aunque de momento parece que tiene algún bug, por que el primer valor siempre lo omite si se usa un enum para definir los índices de los controles.

Por último comentar que [la migración de angular 13 a angular 14](https://update.angular.io/?v=13.0-14.0) es relativamente poco dolorosa ya que de forma automática se modifica nuestro código para que nuestros formularios sigan funcionando con versiones no tipadas, por ejemplo los `FormGroup` convierte a `UntypedFormGroup`, permitiendo que podamos ir migrando poco a poco nuestros formularios a versiones tipadas.

