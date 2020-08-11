# Instrucțiunea try...catch

Această combinație este folosită pentru a testa prin evaluarea un fragment de cod. Dacă apar erori, acestea vor fi raportate prin `throw` (raportează ce erori au apărut).

În aceast enunț (ai să vezi prin alte materiale că `try...catch` mai este numită *construcția try...catch*), va exista o singură instrucțiune `try` urmată de una sau mai multe secvențe `catch` sau de `finally`.

-   `try {...} catch(eroare) {...}`,
-   `try {...} finally {...};`,
-   `try {...} catch(eroare) {...} finally {...};`.

Secvența **catch** cuprinde acțiunile care vor fi întreprinse în caz că din blocul `try` sunt *raportate* (în engleză se spune *thrown*) excepții. Catch primește un obiect, care reprezintă însăși eroarea/erorile care vor fi expuse. Dacă nu există excepții ridicate la evaluare codului, `catch` este ignorat.

```javascript
try {
  var x = false;
  if (x !== true) { throw 'Bre, e nasol!'}
} catch (e) {
  console.log(e);
} finally {
  console.log('Hai salut, am terminat');
};  // Bre, e nasol!  // Hai salut, am terminat
```

În momentul evaluării codului, atunci când apare o eroare, firul de execuție (*runtime*-ul) încearcă să găsească un block `catch`, unde să fie prelucrată eroarea. Dacă nu găsește în funcția din stivă care a generat eroarea, va căuta mai jos în stivă după un `catch`, iar dacă nu există, în cazul browserelor va fi executată funcția cu rol de callback pentru evenimentul `onerror`, iar pentru Node.js va fi callback-ul atașat lui `process.on('uncaughtException')`. 
Obiectul fundamental intern `Error` poate fi extins în caz că este necesar.

```javascript
function EroarePersonalizata(mesaj){
  this.mesaj = mesaj || 'Mesaj șablon';
  this.stiva = (new Error()).stack;
};
EroarePersonalizata.prototype = Object.create(Error.prototype);
EroarePersonalizata.constructor = EroarePersonalizata;

try {
  throw new EroarePersonalizata();
}catch(e){
  console.log(e.mesaj);
}; // Mesaj șablon

try {
  throw new EroarePersonalizata('Mesaj trimis');
}catch(e){
  console.log(e.mesaj);
}; // Mesaj trimis
```
