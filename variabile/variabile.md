# Variabile

## Introducere

Vom aduce în prim plan câteva lucruri interesante despre activitatea cercetătorului Christopher Strachey, care a jucat un rol foarte important în fixarea semanticii limbajelor de programare. Este cunoscut prin notele sale de curs intitulate *Concepte fundamentale în limbajele de programare* (*Fundamental Concepts in Programming Languages*). Christopher Strachey este figura proeminentă a echipei care a creat limbajului de programare CPL (Combined Programming Language) în anii 60. Acest limbaj de programare este strămoșul limbajului de programare C influențat de ALGOL60.  Vă mai aduceți aminte din introducere, când am povestit despre partea stângă (**Left Hand Side**) și partea din dreapta (**Right Hand Side**) a unei expresii? Strachey le numește **L-value** și **R-value**.
În notele de curs, pe lângă sintaxa conform BNF, apare și o expresie care atribuie o valoare unui identificator: `let p = 3.5`. Am menționat aceste detalii pentru a vedea rădăcinile JavaScript, care rezonează cu trecutul programării atunci când privim noile cuvinte cheie `let` și `const`.

Christopher Strachey indică faptul că există termeni concurenți pentru *identificator*, care a fost introdus de ALGOL60. Aceștia sunt *nume* (în lb. engleză `name`) și *referință* (în lb. engleză `reference`), care era folosit de alte limbaje de programare. Tot din lucrarea sa vom găsi clarificarea că variabilele sunt *legate* de valori (**bound variable**). Acest lucru este important pentru a înțelege natura variabilelor, care, în esență, este o legătură între un identificator și o valoare. Variabilele care încă nu au atribuită o valoare sunt *variabile libere*. În JavaScript, variabilele libere primesc automat valoarea `undefined`.

```javascript
let ceva; // undefined
```

## Natura variabilelor

O variabilă este ceea ce spune însuși cuvântul: o zonă rezervată de care este nevoie pentru a *memora* o valoare. Această valoare poate să se modifice pe măsură ce codul se execută și de aici și denumirea de *variabilă*. Pentru a înțelege cât mai bine variabilele, amintiți-vă de identificatori și rolul acestora în *spațiul* creat de mediul lexical. După cum bine v-ați amintit, identificatorii sunt etichetele necesare variabilelor pentru a identifica **zona rezervată** din memoria computerului. E ca o etichetă pe care o punem pe un borcan în care vom pune ceva valoros, i.e. o valoare.

## Declararea variabilelor

Pentru a declara o variabilă se va scrie `let variabila = 'ceva';`. În cazul în care sunt declarate mai multe variabile, se vor înșirui folosindu-se operatorul virgulă, fără a mai specifica cuvântul cheie rezervat limbajului.

```javascript
var ceva; // undefined
var a, b = 'ceva', c = true; // declararea una după alta
var $ceva    = 'Hanna',
    _altceva = 10,
    un_nume  = 'George'; // DA, poți face asta! :D
let asteptValoarea01 = 100;
const ᚠ = 'o rună'; // se poate pentru că folosim UTF16
```

Magia atribuirii valorii identificatorului o face operatorul `=`. Ceea ce se întâmplă dincolo de cortină este alocarea unui spațiu în memoria computerului pentru a *reține* valoarea atribuită. Atribuirea valorii se poate face dintr-un singur pas (`var x = 1;`), declarând și atribuind imediat, sau mai întâi poți declara numele variabilei și abia la momentul oportun vei face atribuirea valorii. Când spunem **atribuire** ar trebui să ne gândim la realizarea *legăturii* dintre identificator și valoarea pe care o individualizează.

```javascript
var x;  // în acest moment are valoarea undefined
x = 10; // poți verifica dacă s-a făcut atribuirea:
x;      // este returnat 10
```

Ce se întâmplă atunci când un identificator devine valoarea unui altuia? În acel moment este copiată valoarea de la primul identificator. Modificarea valorii primului nu va avea drept efect modificarea celui de-al doilea.

```javascript
let identificator1 = 10;
let identificator2 = identificator1;
identificator1 += 1;
console.log(identificator2);
```

Acest comportament nu mai este aplicat și *valorilor complexe* (*obiecte compozite* în alte lucrări) așa cum sunt obiectele. Obiectele complexe sunt stocate prin crearea unei adrese în memorie unde pot fi găsite. Aceste adrese sunt similare celor din lumea fizică, unde pentru a ajunge la un prieten vei introduce în GPS o adresă din oraș. Ceea ce este important de a înțelege privitor la valorile complexe este că atunci când dorești copierea lor, se va face o copie la adresă, nicidecum nu se va duplica obiectul. Spunem că facem o copiere prin referință, nu prin valoare așa cum am făcut mai sus.

```javascript
let obi = {a: '1'};
let obi2 = obi;
console.log(obi2);
```

Chiar dacă am atribuit o valoare de un anumit tip unei variable, poți în orice moment să-i atribui o altă valoare de un alt tip.

```javascript
let a = 1;
a = 'ceva';
console.log(a);
// "ceva"
```

În JavaScript, abia la momentul constituirii mediului lexical se fac și legăturile dintre numele identificatorilor și valorile lor. Atunci când sunt folosite numele identificatorilor în expresii, acestea sunt evaluate prin returnarea valorii lor.

```javascript
let a = {0: 10},
    b = a;
a === b; // true
```

De fapt, ceea ce am realizat este o copiere a variabilei `a` prin referință. Spunem că noua variabilă `b` face o trimitere către variabila deja existentă `a`, care este legată la o valoare. În cazul nostru vorbim despre un obiect. Da, structura `{0: 10}` este un obiect foarte, foarte simplu.

Aceeași demonstrație se poate realiza prin pasarea valorii ca argument al unei funcții. Pentru exemplificare vom folosi un șablon des întâlnit în JavaScript, care se aplică pentru a executa imediat o funcție fără a o apela după identificatorul său. Vom introduce funcția în prima pereche de paranteze din expresia `()();`, ceea ce va conduce la evaluarea funcției instant. În limbaj tehnic, aceast șablon de lucru se numește IIFE (Imediately Invoked Function Expression) - **expresie de funcție invocată imediat**. Și mai folosim ceva foarte util: metoda `log()` a obiectului `console` dare este disponibil din oficiu. Dar mai întâi, vă invit să priviți la exemplu pentru a crea ceva context și cu siguranță nedumerire.

```javascript
(function () {
  let x = 10;
})();
console.log(x); // ReferenceError
```

Poți trage cu ochiul repede la funcții să afli mai multe, dar pentru moment este îndeajuns cât să ne putem descurca.

## Variabile și mediu lexical

Ce-i *mediul lexical*? Pornesc prin a-ți reaminti faptul că este crucial modul în care redactezi codul sursă. Felul în care declari variabilele va determina modul în care funcționează programul. Un mediu lexical este o zonă a codului în care găsim un set de legături dintre identificatori și valori. Acesta este pus la dispoziția motorului JavaScript pentru a evalua codul zonei. Nu este greșit să gândești codul sursă ca o poveste a mediilor lexicale. În alte lucrări din limba engleză i se mai spune și *scope*. Funcțiile au o proprietate extraordinară: creează propriul **mediu lexical**.

Funcțiile creează aceste zone numite medii lexicale, dar începând cu ES6, simpla declarare a blocului de cod prin acolade `{}`, creează un mediu lexical (*scope*). Este foarte importantă această regulă pentru că de ea ține modul în care izolăm, combinăm și punem execuția unei funcții într-un anumit context.

```javascript
let a = {0: 10};
(function (b) {
  console.log(b === a);
})(a); // true
```

JavaScript permite modificarea valorii preexistente a unei variabile. Această posibilitate se numește *umbrire* (*shadowing*).

```javascript
let valoare = 10;
valoare = 15;
console.log(valoare); // 15
```

Ceea ce se întâmplă este o mutarea unei legături la o altă valoare. Merită investigat ceea ce se întâmplă și în cazul mediilor lexicale generate de funcții.

```javascript
(function x () {
  let ceva = 10;
  (function y () {
    let ceva = 15;  // declarare
  })();
  return ceva;
})(); // 10
```

Funcția `x` creează un mediul lexical (un *scope*) în care se va afla valoarea `10` legată de numele `x`. În interiorul funcției `x`, este evaluată funcția `y`, care creează propriul mediu lexical în care este declarată o variabilă proprie cu numele `ceva`. Pentru că se face o declarare, chiar dacă variabila poartă același nume cu variabila din mediul lexical (*scope*) extern la care funcția internă are acces, variabila din mediul intern, nu o va *umbri* (suprascrie) pe cea din mediul lexical extern. Situația se va schimba dacă nu se face declarare, ci atribuirea unei valori.

```javascript
(function x () {
  let ceva = 10;
  (function y () {
    ceva = 15;  // atribuire
  })();
  return ceva;
})(); // 15
```

**Moment ZEN**: Variabilele sunt legături ale identificatorilor la valorile lor.

Nu folosiți niciodată cuvintele rezervate ale limbajului drept nume pentru variabile. Va apărea o eroare. Reține și faptul că JavaScript face diferența dintre majuscule și minuscule. Astfel, `oVariabila` nu este echivalentul lui `ovariabila`. Sunt două variabile diferite.

JavaScript folosește trei cuvinte cheie specifice limbajului: `var`, `let` și `const`. Declararea variabilelor cu `let` și `const` a fost adăugată de versiunea ES6 a standardului și sunt legate strict de blocul de cod delimitat prin acolade.

```javascript
let ceva = 'test';
if (true) {
  let ceva = 'TEST';
  console.log(ceva);  // TEST
};
console.log(ceva);    // test
```

În cazul în care variabila `ceva` ar fi fost declarată cu `var`, cea de-a doua declarație ar fi fost o redeclarare a aceluiași identificator. Acest lucru nu se petrece în cazul lui `let`. Chiar vorbim de două declarații distincte din două blocuri distincte de cod.
Cele trei moduri de a declara variabile au efect și asupra mediului lexical.

## Declarații folosind var

Declararea unei variabile folosind cuvântul cheie `var`, va avea ca efect declararea variabilei în *global scope* sau într-o funcție. Nu este localizată la nivel de bloc de cod simplu - `{}`. De exemplu, pentru o iterare cu `for`, variabila definită drept condiție este o variabilă *înregistrată* în scope-ul funcției care găzduiește `for`, nu în cel al lui `for`. Din nevoia de a localiza variabilele la nivel de bloc delimitat de acolade, au fost introduse de ES6 declarațiile cu `let` și `const`.

O declarație `var` în cadrul unei funcții declanșează aplicarea următoarelor comportamente:

- partea declarativă (`var x`) este supusă `hoisting`-ului ajungând automat la `suprafața funcției`, având valoarea `undefined`, apoi
- atribuirea valorii se face acolo unde se menționează în cod operatorul `=` în momentul în care motorul a ajuns la acel fragment în procesul de evaluare.

Am menționat deja mecanismul de *hoisting*. Este regula prin care variabilele declarate cu `var` sunt puse la dispoziția întregului cod de la bun început, nu pe măsură ce evaluarea avansează. Spunem că identificatorii acestor declarații sunt omniprezenți din start chiar dacă sunt inițializați cu valoarea `undefined`. Pentru a înțelege cu adevărat hoisting-ul, ar fi necesar să vezi cum face JavaScript compilarea codului.

```javascript
function ex () {
  var x = undefined; // nu se vede, lucrează hoistingul.
  //...
  x = 10;
};
```

Buna practică spune ca atunci când folosești `var` pentru a declara variabile, pune-le pe toate imediat ce ai deschis blocul de cod. Astfel, le vei face *omniprezente* pentru acea zonă de cod și va fi mai ușor de operat cu ele. Un alt sfat foarte bun spune că, pentru a ușura comunicarea cu ceilalți programatori care vor inspecta codul tău mai târziu, faci declararea variabilelor cât mai aproape de locul în care vor fi utilizate. Uneori acest lucru necesită chiar redeclararea (shadowing) în locul în care se cere. Acesta este cazul în care avem de-a face cu fragmente imense de cod (500 - 1000 de linii de cod). De ce? Pentru a nu-l fugări pe colegul tău din viitor pe tot codul înapoi pentru a face muncă de detectiv. Bineînțeles că vei face și un mic comentariu în care explici de ce ai ales redeclararea. Atenție, acest lucru nu este posibil atunci când folosești `let`. Acest lucru nu este permis în cazul lui `let` pentru că `let` permite declararea o singură dată într-un loc fix. Variabilele declarate cu `let` nu sunt afectate de *hoisting*.

```javascript
function facCeva() {
  if (conditie > 0) {
    var a = 0;
    var b = true;
  } else if ( conditie < 100) {
    var a = 10;
    var b = false;
  }
};
```

### Standardul spune

> Un enunț `var` declară variabile care sunt în *aria de efect* al contextului de execuție curent. Variabilele `var` sunt create atunci când este instanțiat mediul lexical care le conține, dar la momentul în care sunt create, li se atribuie valoarea `undefined`. Atribuirea valorii se face la momentul execuției, nu la momentul creării. Valoarea este atribuită la momentul execuției codului, nu la momentul declarării.

## `let` și `const`

Cuvântul cheie `let` îl putem traduce *fie*, precum în: *fie `x` un număr cu valoarea `1`*. Este folosit pentru a defini variabile în cel mai apropiat *mediu lexical* (*scope*), care poate fi global scope, un bloc de cod delimitat prin acolade `{}` sau o buclă precum `for`. Rolul lor este de a localiza la nivel de înregistrare *în scope* în următoarele medii: `global`, `function` și bloc `{}`.

### Standardul spune

> Declarațiile `let` și `const` definesc variabilele care aparțin mediului lexical al contextului de execuție curent. Variabilele sunt create atunci când mediul lexical este instanțiat, dar nu vor fi accesate nicicum până când nu sunt evaluate legăturile la valori (**lexical binding**). Valoarea este atribuită atunci când este evaluat acest **lexical binding**, nu la momentul declarării lor. Dacă o declarație cu `let` nu are o valoare de inițializare, este atribuit `undefined` la momentul în care este evaluat, adică la momentul când se face `lexical binding` și se completează așa-numitul **Registru de mediu**.

## Mantre

-   La momentul creării, variabilele vor fi inițializate automat cu valoarea `undefined`.
-   Identificatorii variabilelor și ai funcțiilor sunt recoltați prin mecanismul motorului JavaScript numit *identifier lookup*.
-   Dacă declari o variabilă cu `var` în corpul unei declarații `if` sau `for`, această variabilă va fi disponibilă și în afara blocului funcțional, fie că blocul a fost executat sau nu.
-   *Scope*-ul unei variabile poate fi înțeles mediul unde este găsit un identificator.
-   Variabilele locale sunt reconectate la valori de câte ori funcția este invocată.
-   Variabilele locale sunt reatribuite cu valori de fiecare dată când o funcție este invocată.
-   Parametrii unei funcții sunt la rândul lor variabile locale.
-   Declarațiile de variabile se află *în scope* de la momentul în care au fost declarate, până la închiderea blocului funcției în care au fost declarate indiferent de imbricarea altor blocuri `{}`.

## Căutarea în scope chain

Am menționat faptul că variabilele locale sunt introduse în ***scope***, care poate fi perceput ca un obiect la al cărui proprietăți ai access. Atunci când se execută un fragment de cod, interpretorul caută identificatorii în obiectul scope curent. Dacă nu o găsește, atunci interpretorul va căuta mai sus în obiectul scope părinte și tot așa până când nu mai există un alt obiect părinte. Această înlănțuire de scope-uri, se numește **scope chain**. Scope-ul (**mediul lexical**) se formează la momentul declarării variabilelor și funcțiilor, nu la momentul execuției. Doar funcțiile atunci când sunt executate creează pe loc mediul lexical.

## Atribuirea valorilor cu operatorul SAU

Declararea unei variabile și atribuirea acesteia cu o valoare, poate fi rafinată prin opțiunea pentru una din două valori. Pentru a realiza acest minicircuit, se va folosi operatorul logic SAU (`||`). După cum știm deja acesta evaluează expresia din partea stângă și dacă se reduce valoarea la adevăr, atunci nu va mai evalua expresia din dreapta.

```javascript
var expresia1 = 0, expresia2 = {a: 10};
var x = expresia1 || expresia2;
```

S-a aplicat mecanismul de transformare, iar valoarea 0 a fost evaluată a fi `false`. Astfel, identificatorul *x* a fost legat la obiect.
Ceea ce se întâmplă când expresia din stânga este evaluată la false, dar și cea din partea dreaptă la fel, este că identificatorul *x* va fi legat la valoarea expresiei din dreapta.

## Stări confuze

Se poate întâmpla ca la momentul în care citești codul scris de altcineva sau codul scris de tine în trecut să nu mai poți aprecia cu claritate ce este disponibil, cui. Reține faptul că variabile pot fi pasate ca argumente.

Când există o stare de confuzie, cel mai bine este să arunci o privire unde este definită funcția (nu unde este apelată) și identifică acolo ce este vizibil.

## Bune practici

Folosește `const` pentru toate valorile pentru care nu se așteaptă nicio modificare și `let` în cazul în care știi clar că vor surveni modificări.
Folosește `var` când ai nevoie de mecanismul de hoisting (omniprezența în cod), dar și pentru momentele când ai nevoie să o redeclari. Cu alte cuvinte, folosește `var` atunci când ai nevoie de o valoare în mai multe medii lexicale diferite, care eventual sunt înlănțuite (*scope chaining*).
Declară toate variabilele necesare în capul codului/funcției în ordinea necesităților de accesare a valorilor. În cazul folosirii blocurilor de cod, care sunt permise odată cu ES6, pune toate declarațiile de variabile cu `let` chiar de la bun început pentru a evita TDZ - Temporal Dead Zone.

```javascript
function facCeva() {
  // cod util
  { let ceva = 10, altceva = ++ceva;
    // cod înscris unui
    // bloc dedicat
  }
};
```

În bucle folosește întotdeauna `let` ori `const` pentru a crea pentru fiecare iterație propria legătură identificator - valoare. În cazul lui `var`, vei avea acces mereu doar la ultima valoare dată de ultima iterare (se face shadowing).

## Testarea unei variabile pentru null sau undefined

În situațiile în care ai o variabilă care trebuie să primească valoarea în urma unei evaluări sau ca răspuns al unei operațiuni asicrone, ai nevoie să testezi dacă a fost atribuită o valoare sau valorile `null` ori `undefined` care indică o problemă.

```javascript
let ceva = 10;

if (typeof ceva === 'undefined') {
    console.log('Variabilă undefined');
} else if (typeof ceva === 'null') {
    console.log('Valoarea este null');
} else {
    console.log(ceva);
}
```

Poți testa după ambele dacă acest lucru este necesar.

```javascript
let undeva;
if (undeva === null || undeva === undefined) {
  console.log('Valaorea este null sau undefined');
}
```

## Resurse

- [Wikipedia, Name binding](https://en.wikipedia.org/wiki/Name_binding)
- [](https://stackabuse.com/javascript-check-if-variable-is-a-undefined-or-null/)
