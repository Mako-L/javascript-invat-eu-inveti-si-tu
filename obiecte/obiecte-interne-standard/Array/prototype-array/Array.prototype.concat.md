# Array.prototype.concat()

Nu modifică structura array-ului.

Realizează o *copie simplă* (în limba engleză numită *shallow copy*) formată din elementele array-ului asupra căruia se invocă metoda și elementele care se doresc a fi adăugate.

```javascript
var arr = [1, 2, 3];

arr.concat(4, 5, 6);    // returnează Array [ 1, 2, 3, 4, 5, 6 ]
arr.concat([4, 5, 6]);  // returnează Array [ 1, 2, 3, 4, 5, 6 ]
arr.concat([4, 5], 6);  // returnează Array [ 1, 2, 3, 4, 5, 6 ]
arr.concat([4, [5, 6]]);// returnează Array [ 1, 2, 3, 4, Array[2] ]
arr.concat([4, [5, 6], 7, 8, [9, 10]]); // returnează Array [ 1, 2, 3, 4, Array[2], 7, 8, Array[2] ]
```

Ceea ce trebuie remarcat este faptul că operațiunea `concat` realizează o *desfacere* a unui array și alipirea valorilor sale, dacă acesta este poziționat primul. Un array poziționat dincolo de prima valoare, va fi introdus ca array și valorile sale în mod direct.

Elementele array-urilor originale sunt copiate în noul array format respectându-se următoarele reguli:

- în cazul obiectelor sunt copiate referințe către acestea, nu obiectele în sine. Dacă un obiect referențiat este modificat, se va reflecta în array-ul original și cel nou format.
- în cazul *șirurilor de caractere* și a *numerelor*, acestea vor fi copiate în noul array. Modificarea valorilor din array-urile originale nu se vor răsfrânge în cel nou constituit.

La un array se pot adăuga elemente în mod direct folosindu-se `concat` prin pasarea unui al doilea argument.

```javascript
var arr = [1, 2, 3];
arr.concat("ceva");
console.log(arr);
// [ 1, 2, 3, "ceva" ]
```

![Array.prototype.concat()](ArrayConcat.svg)

Se poate chiar construi o funcție `concat` folosind `slice`, dar care să facă concat pe argumentele pasate.

```javascript
function concat () {
  return Array.prototype.slice.call(arguments).join(' ');
};
var sirNou = concat('ceva', 'text', 'pentru', 'a', 'fi', 'unit');
console.log(sirNou);
```

## Concat și spread

Metoda `concat` *sudează* elementele a două array-uri luând ca array receptor pe cel pe care se aplică, iar cel de-al doilea este pasat. Același lucru poate fi realizat prin folosirea operatorului trei puncte (*spread*).

```javascript
var arr1 = ['a', 'b', 'c'];
var arr2 = ['x', 'y', 'z'];
arr1 = [...arr1, ...arr2];
```
