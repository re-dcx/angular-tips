# Comparación de arrays desordenados con `lodash`

Cómo ya sabréis, para comparar dos arrays (por ejemplo con ids de usuarios) podemos usar la función `isEqual` de lodash.

```ts
import * as _ from 'lodash';
// ...
const listOne = [1, 2, 3];
const listTwo = [1, 2, 3];

const isEqual = _.isEqual(listOne, listTwo) // true
```

El problema es que si los arrays no están en el mismo orden  `_.isEqual` nos devolverá `false` aunque tengan los mismos elementos. Por ejemplo 
```ts
import * as _ from 'lodash';
// ...
const listOne = [1, 2, 3];
const listTwo = [3, 2, 1];

const isEqual = _.isEqual(listOne, listTwo); // false
```

Una solución podría es ordenar los arrays antes de compararlos.
```ts
import * as _ from 'lodash';
// ...
const listOne = [1, 2, 3];
const listTwo = [3, 2, 1];

const result = _.isEqual(_.sortBy(listOne), _.sortBy(listTwo)); // true
```

Pero habría una forma más compacta de compararlos usando la función `_.xor`

```ts
import * as _ from 'lodash';
// ...
const listOne = [1, 2, 3];
const listTwo = [3, 2, 1];

const result = _.xor(listOne, listTwo).length===0; // true
```
La función `_.xor` devuelve los elementos en los que se diferencian dos arrays por ejemplo  `_.xor([1,2], [2,3])` nos devolvería `[1,3]`. Por lo tanto, tal como podréis intuir, si la cantidad de elementos devueltos por `_.xor` es cero significa que las dos listas son iguales.

