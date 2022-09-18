## Uso del método `loadData()`

Siempre que tengamos un componente que requiera cargar datos haciendo una llamada a un endpoint debemos encapsular la lógica en un método privado llamado `_loadData():void` 

Si necesitaramos recargar los datos desde la plantilla, por ejemplo desde un botón `refresh` no podremos llamar de forma directa a este método, se debe crear un método público llamado `onRefresh()`
