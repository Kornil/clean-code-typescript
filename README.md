# clean-code-typescript [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Clean%20Code%20Typescript&url=https://github.com/labs42io/clean-code-typescript)

Concetti di Clean Code per TypeScript.
Ispirato da [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript).

## Indice

  1. [Introduzione](#introduzione)
  2. [Variabili](#variabili)
  3. [Funzioni](#funzioni)
  4. [Oggetti e Strutture Dati](#oggetti-e-strutture-dati)
  5. [Classi](#classi)
  6. [SOLID](#solid)
  7. [Testing](#testing)
  8. [Concorrenza](#concorrenza)
  9. [Gestione degli errori](#gestione-degli-errori)
  10. [Formattazione](#formattazione)
  11. [Commenti](#commenti)
  12. [Traduzioni](#traduzioni)

## Introduzione

![Immagine divertente riguardo la qualità del software in relazione a quante parolacce esclami quando lo leggi](https://www.osnews.com/images/comics/wtfm.jpg)

Software engineering principles, di Robert C. Martin's book
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adattato per TypeScript. questa non è una style guide, è una guida per scrivere software
[leggibile, riutilizzabile, e rifattorizzabile](https://github.com/ryanmcdermott/3rs-of-software-architecture) in TypeScript.

Non tutti i principi di questa guida devono essere seguiti alla lettera e pochi sono universalmente riconosciuti.
Queste sono linee guida non ufficiali ma codificate da anni di esperienza collettivi dagli autori di *Clean Code*.

Il lavoro di software engineering ha poco più di 50 anni, e stiamo ancora imparando molto. Quando l'architettura software sarà vecchia quanto l'architettura, forse allora avremo rigide regole da seguire.
Per adesso, usa queste linee guida per valutare la qualità del codice TypeScript che tu e il tuo team scrivete. 

Un chiarimento: conoscere queste linee guida non ti renderà immediatamente un miglior software developer, lavorarci per molti anni non significa non farai errori.
Ogni pezzo di codice comincia da una prima bozza, come l'argilla viene modellata nella sua forma finale, si possono rimuovere le imperfezioni dal codice quando lo revisioniamo con i nostri pari.
Non abbatterti se le tue prime bozze necessitano migliorie, abbatti il codice!


**[⬆ torna all'inizio](#indice)**

## Variabili

### Usa nomi esplicativi

Distingui i nomi in modo che il lettore capisca la differenza tra loro.

**Sbagliato:**

```ts
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}

```

**Corretto:**

```ts
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```

**[⬆ torna all'inizio](#indice)**

### Usa nomi pronunciabili

Se non puoi pronunciarla, non puoi parlarne senza sembrare un idiota.

**Sbagliato:**

```ts
type DtaRcrd102 = {
  genymdhms: Date;
  modymdhms: Date;
  pszqint: number;
}
```

**Corretto:**

```ts
type Customer = {
  generationTimestamp: Date;
  modificationTimestamp: Date;
  recordId: number;
}
```

**[⬆ torna all'inizio](#indice)**

### Usa lo stesso vocabolo per variabili duplicate

**Sbagliato:**

```ts
function getUserInfo(): User;
function getUserDetails(): User;
function getUserData(): User;
```

**Corretto:**

```ts
function getUser(): User;
```

**[⬆ torna all'inizio](#indice)**

### Usa nomi ritrovabili

Leggeremo più codice di quanto ne scriveremo. È importante che il codice che scriviamo sia legibile e possa essere ritrovato. Se *non* nominiamo le variabili in modo esplicativo per capire il programma, rechiamo torto ai nostri lettori. Usa nomi che possono essere trovati. Tools come [ESLint](https://typescript-eslint.io/) possono aiutare a identificare costanti senza nome (conosciute come stringhe e numeri magici).

**Sbagliato:**

```ts
// What the heck is 86400000 for?
setTimeout(restart, 86400000);
```

**Corretto:**

```ts
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 24 * 60 * 60 * 1000; // 86400000

setTimeout(restart, MILLISECONDS_PER_DAY);
```

**[⬆ torna all'inizio](#indice)**

### Usa variabili che possono essere spiegate

**Sbagliato:**

```ts
declare const users: Map<string, User>;

for (const keyValue of users) {
  // iterate through users map
}
```

**Corretto:**

```ts
declare const users: Map<string, User>;

for (const [id, user] of users) {
  // iterate through users map
}
```

**[⬆ torna all'inizio](#indice)**

### Evita mappe mentali

Esplicito è meglio di implicito.
*La chiarezza è sovrana.*

**Sbagliato:**

```ts
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**Corretto:**

```ts
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

**[⬆ torna all'inizio](#indice)**

### Non aggiungere contesto inutile

Se il/la tuo/a classe/tipo/oggetto dice qualcosa, non ripeterlo nel nome della variabile.
If your class/type/object name tells you something, don't repeat that in your variable name.

**Sbagliato:**

```ts
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
}

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**Corretto:**

```ts
type Car = {
  make: string;
  model: string;
  color: string;
}

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

**[⬆ torna all'inizio](#indice)**


### Usa gli argomenti predefiniti invece di short circuiting o condizionali.

Gli argomenti predefiniti sono spesso preferibili a short circuiting. are often cleaner than short circuiting.

**Sbagliato:**

```ts
function loadPages(count?: number) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**Corretto:**

```ts
function loadPages(count: number = 10) {
  // ...
}
```

**[⬆ torna all'inizio](#indice)**

### Usa enum per documentare l'intento

Enum può aiutare a documentare l'intento del codice. Per esempio quando i valori possono essere diversi dai valori reali. 

**Sbagliato:**

```ts
const GENRE = {
  ROMANTIC: 'romantic',
  DRAMA: 'drama',
  COMEDY: 'comedy',
  DOCUMENTARY: 'documentary',
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // some logic to be executed 
    }
  }
}
```

**Corretto:**

```ts
enum GENRE {
  ROMANTIC,
  DRAMA,
  COMEDY,
  DOCUMENTARY,
}

projector.configureFilm(GENRE.COMEDY);

class Projector {
  // declaration of Projector
  configureFilm(genre) {
    switch (genre) {
      case GENRE.ROMANTIC:
        // some logic to be executed 
    }
  }
}
```

**[⬆ torna all'inizio](#indice)**

## Funzioni

### Argomenti delle funzioni (Idealmente 2 o meno)

Limitare il numero di parametri è estremamente importante in quanto facilita testare le funzioni stesse.
Aver troppi parametri può portare a un'esplosione di combinazioni e casi differenti con argomenti diversi tra loro.

Uno o due argomenti è l'ideale, tre dovrebbe essere evitato se possibile. Altri oltre quelli dovrebbero essere consolidati.
Di solito, se hai più di due argomenti la tua funzione sta cercando di fare troppo.
Se questo non è il caso, la maggior parte delle volte un oggetto di alto livello può essere usato come argomento.

Object literals possono essere usati nel caso ci sia bisogno di molti argomenti.

Per rendere più chiaro quali proprietà una funzione aspetta, questa sintassi può essere usata  [destructuring](https://basarat.gitbook.io/typescript/future-javascript/destructuring).
Questo porta un po' di vantaggi:

1. Guardando la signature della funzione, è chiaro quale proprietà vengano usate.

2. Può essere usata per simulare parametri con nome.

3. Destructuring clona i valori primitivi degli argomenti oggetti passati alla funzione. Questo può aiutare a prevenire effetti indesiderati (side effects). Nota: oggetti e array destrutturati dagli argomenti dell'oggetto NON sono clonati.

4. TypeScript warns you about unused properties, which would be impossible without destructuring.
4. TypeScript avvisa per le proprietà inutilizzate, che non sarebbe possibile senza destructuring.

**Sbagliato:**

```ts
function createMenu(title: string, body: string, buttonText: string, cancellable: boolean) {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Corretto:**

```ts
function createMenu(options: { title: string, body: string, buttonText: string, cancellable: boolean }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

La leggibilità può essere incrementata usando [type aliases](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases):

```ts

type MenuOptions = { title: string, body: string, buttonText: string, cancellable: boolean };

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

**[⬆ torna all'inizio](#indice)**

### Le funzioni dovrebbero avere un singolo scopo

Questa è probabilmente la regola più importante nell'ingegneria del software. Quando una funzione ha più di uno scopo, è difficile da comporre, testare e ragionare. Isolando le funzioni in singoli scopi rende il codice più semplice da rifattorizzare e sarà più semplice da leggere. Se non impari nient'altro da questa guida oltre a questo, sarai comunque avanti rispetto a molti programmatori.

**Sbagliato:**

```ts
function emailActiveClients(clients: Client[]) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Corretto:**

```ts
function emailActiveClients(clients: Client[]) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ torna all'inizio](#indice)**

### I nomi delle funzioni dovrebbero specificare cosa fanno

**Sbagliato:**

```ts
function addToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Corretto:**

```ts
function addMonthToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();
addMonthToDate(date, 1);
```

**[⬆ torna all'inizio](#indice)**

### Le funzioni dovrebbero avere un solo livello di astrazione

Quando una funzione ha più livelli di astrazione, di solito sta facendo troppo. Separando la funzione aumenta il livello di riusabilità e rende più facile testarla.

**Sbagliato:**

```ts
function parseCode(code: string) {
  const REGEXES = [ /* ... */ ];
  const statements = code.split(' ');
  const tokens = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Corretto:**

```ts
const REGEXES = [ /* ... */ ];

function parseCode(code: string) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);

  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code: string): Token[] {
  const statements = code.split(' ');
  const tokens: Token[] = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function parse(tokens: Token[]): SyntaxTree {
  const syntaxTree: SyntaxTree[] = [];
  tokens.forEach((token) => {
    syntaxTree.push( /* ... */ );
  });

  return syntaxTree;
}
```

**[⬆ torna all'inizio](#indice)**

### Rimuovi il codice duplicato

Fai del tuo meglio per rimuovere codice duplicato.
Duplicare il codice è sbagliato perché se aumenta il codice da alterare quando la logica viene cambiata.

Immagina di gestire un ristorante e tener traccia dell'inventario: tutti i pomodori, cipolle, aglio, spezie etc.
Se ci sono più liste che tracciano questo, tutte dovranno essere aggiornate quando servirai un piatto che contiene pomodori.
Se c'è una singola lista, c'è solo quella da aggiornare!

Spesso il codice viene duplicato perché sono presenti diverse variazioni, che hanno molto in comune, ma le cui differenze forzano ad avere funzioni diverse che si occupano dello stesso scopo.
Rimuovere il codice duplicato significa creare astrazioni che possono gestire queste differenze con una sola funzione/modulo/classe.

Usare la giusta astrazione è critico, per questo bisognerebbe seguire i principi [SOLID](#solid). Attenzione! Astrazioni sbagliate possono peggiorare le duplicazioni di codice. Nonostante quest, se puoi scrivere una buona astrazione, fallo! Non ripetere o ti ritroverai a dover aggiornare più codice in posti diversi ogni volta che vorrai cambiare qualcosa.

**Sbagliato:**

```ts
function showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Corretto:**

```ts
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    }
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio,
    }
  }
}

function showEmployeeList(employee: (Developer | Manager)[]) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra,
    };

    render(data);
  });
}
```

Considera un union type o una classe madre comune per la tua astrazione.
```ts
class Developer {
  // ...
}

class Manager {
  // ...
}

type Employee = Developer | Manager

function showEmployeeList(employee: Employee[]) {
  // ...
  });
}

```

Esercita criticità riguardo alle duplicazioni di codice. Spesso c'è un compromesso tra duplicare codice e introdurre complessità aggiungendo astrazioni non necessarie. Quando due implementazioni simili tra due moduli diversi sono presenti in domini diversi, la duplicazione potrebbe essere accettabile e preferibile all'estrarre il codice comune. L'estrazione del codice comune, in questo caso, introduce una dipendenza indiretta tra i due moduli.

**[⬆ torna all'inizio](#indice)**

### Imposta oggetti predefiniti con Object.assign o destructuring

**Sbagliato:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: 'Bar' });
```

**Corretto:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  const menuConfig = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // ...
}

createMenu({ body: 'Bar' });
```

O in alternativa lo spread operator:

```ts
function createMenu(config: MenuConfig) {
  const menuConfig = {
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true,
    ...config,
  };

  // ...
}
```
Lo spread operator e `Object.assign()` sono molto simili.
La differenza principale è che spreading definisce nuove proprietà mentre `Object.assign()` le imposta (set). Le differenze sono spiegate in maggior dettaglio in [questo](https://stackoverflow.com/questions/32925460/object-spread-vs-object-assign) thread.

In alternativa, puoi usare la destrutturazione con valori predefiniti:

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu({ title = 'Foo', body = 'Bar', buttonText = 'Baz', cancellable = true }: MenuConfig) {
  // ...
}

createMenu({ body: 'Bar' });
```

Per evitare effetti collaterali e comportamenti imprevisti passando esplicitamente valori `undefined` o `null`, il compilatore di TypeScript può disabilitarlo.
Vedi l'opzione [`--strictNullChecks`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#--strictnullchecks) su TypeScript.

**[⬆ torna all'inizio](#indice)**

### Non usare flags come parametri di funzione

Le flags indicano che una funzione ha più di uno scopo.
Le funzioni dovrebbero avere uno scopo. Dividi le funzioni se seguono percorsi diversi in base a booleani.

**Sbagliato:**

```ts
function createFile(name: string, temp: boolean) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Corretto:**

```ts
function createTempFile(name: string) {
  createFile(`./temp/${name}`);
}

function createFile(name: string) {
  fs.create(name);
}
```

**[⬆ torna all'inizio](#indice)**

### Evita gli effetti collaterali (parte 1)

Una funzione produce un effetto collaterale se fa qualcosa oltre a prendere un valore e restituirne un altro o altri.
Un effetto collaterale potrebbe essere sovrascrivere un file, modificare una variabile globale o mandare per errore tutti i tuoi soldi a uno sconosciuto.

Ogni tanto degli effetti collaterali in un programma sono necessari. Nell'esempio precedente, sovrascrivere un file potrebbe essere un'azione legittima.
Il comportamento corretto è centralizzare dove queste interazioni avvengono. Non avere funzioni e classi diverse che sovrascrivono lo stesso file.
Un servizio dovrebbe essere responsabile. Solo uno.

Il punto principale è di evitare errori comuni come condividere lo stato tra oggetti senza alcuna struttura, usare data types mytabili che possono essere sovrascritti e non centralizzare dove gli effetti collaterali avvengono. Se puoi fare questo sarai molto più felice della maggior parte degli altri programmatori.

**Sbagliato:**

```ts
// Global variable referenced by following function.
let name = 'Robert C. Martin';

function toBase64() {
  name = btoa(name);
}

toBase64();
// If we had another function that used this name, now it'd be a Base64 value

console.log(name); // expected to print 'Robert C. Martin' but instead 'Um9iZXJ0IEMuIE1hcnRpbg=='
```

**Corretto:**

```ts
const name = 'Robert C. Martin';

function toBase64(text: string): string {
  return btoa(text);
}

const encodedName = toBase64(name);
console.log(name);
```

**[⬆ torna all'inizio](#indice)**

### Evita gli effetti collaterali (parte 2)

I browser e Node.js processano soltanto JavaScript, il codice TypeScript deve essere compilato prima di essere eseguito o debuggato. Su JavaScript, alcuni valori non possono essere cambiati (immutabili) mentre altri possono (mutabili). Oggetti e arrays sono due esempi di valori mutabili quindi è importante prestare attenzione quando vengono passati come parametri a una funzione. Una funzione JavaScript può cambiare le proprietà di un offetto o alterare il contenuto di un array, che potrebbe facilmente causare bugs.

Immagina una funzione che accetta un array come parametro per rappresentare un carrello. Se la funzione cambia il contenuto nel carrello array, aggiungendo un oggetto da comprare per esempio,  tutte le altre funzioni che usano lo stesso carrello saranno affette da questo cambiamento. Questo potrebbe essere buono o no. Di seguito un esempio di un errore:

L'user clicca il pulsante "compra" che chiama la funzione `purchase`  che genera una richiesta di rete e manda l'array `cart` al server. Per colpa di una cattiva connessione di rete la funzione `purchase` deve ritentare la richiesta.
Nel frattempo cosa succederebbe se l'user cliccasse accidentalmente il bottone "Aggiungi al Carrello" di un oggetto che non volevano prima della richiesta di rete? Se questo accade prima che la richiesta di rete cominci, la funzione manderà accidentalmente il nuovo oggetto perchè l'array `cart` è stato modificato.

Una soluzione sarebbe di clonare `cart` dentro la funzione `addItemToCart`, modificarlo e ritornare il clone. In questo modo la funzione che sta ancora usando il vecchio carrello non sarebbe affetta dal cambiamento.

Due avvertenze da menzionare per questo approccio:

1. Ci sono casi in cui modificare l'oggetto è il comportamento voluto, ma adottando questa pratica di programmazione troverai che questi casi sono abbastanza rari. La maggior parte del codice può essere rifattorizzato per rimuoevere tutti gli effetti collaterali! (Vedi [funzione pure](https://en.wikipedia.org/wiki/Pure_function))

2. Clonare grandi oggetti può diventare molto costoso in termini di performance. Fortunatamente questo non è un grande problema in quanto sono presenti [librerie](https://github.com/immutable-js/immutable-js) che consentono questo tipo di approccio di programmazione in modo veloce senza usare troppa memoria come sarebbe clonando manualmente oggetti e arrays.

**Sbagliato:**

```ts
function addItemToCart(cart: CartItem[], item: Item): void {
  cart.push({ item, date: Date.now() });
};
```

**Corretto:**

```ts
function addItemToCart(cart: CartItem[], item: Item): CartItem[] {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ torna all'inizio](#indice)**

### Non modificare le funzioni globali

Inquinare i globali è una pratica sbagliata su Javascript perchè potrebbe creare conflitti con altre librerie, e l'user potrebbe non esserne a conoscenza fino a che un eccezzione appare in produzione.
Un esempio: immagina di voler estendere il metodo nativo dell'array per aggiungere un metodo `diff` per mostrare le differenze tra due array. La funzione potrebbe essere scritta dentro `Array.prototype`, ma potrebbe creare un conflitto con una libreria che cerca di sovrascrivere lo stesso metodo. Se l'altra libreria usava `diff` per vedere le differenze tra il primo e l'ultimo elemento dell'array? Per questo motivo è preferibile usare una classe e semplicemente estendere il globale  `Array`.

**Sbagliato:**

```ts
declare global {
  interface Array<T> {
    diff(other: T[]): Array<T>;
  }
}

if (!Array.prototype.diff) {
  Array.prototype.diff = function <T>(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter(elem => !hash.has(elem));
  };
}
```

**Corretto:**

```ts
class MyArray<T> extends Array<T> {
  diff(other: T[]): T[] {
    const hash = new Set(other);
    return this.filter(elem => !hash.has(elem));
  };
}
```

**[⬆ torna all'inizio](#indice)**

### Preferisci functional programming all'imperative programming

Usa questo stile di programmazione dove possibile.

**Sbagliato:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < contributions.length; i++) {
  totalOutput += contributions[i].linesOfCode;
}
```

**Corretto:**

```ts
const contributions = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const totalOutput = contributions
  .reduce((totalLines, output) => totalLines + output.linesOfCode, 0);
```

**[⬆ torna all'inizio](#indice)**

### Incapsula i condizionali

**Sbagliato:**

```ts
if (subscription.isTrial || account.balance > 0) {
  // ...
}
```

**Corretto:**

```ts
function canActivateService(subscription: Subscription, account: Account) {
  return subscription.isTrial || account.balance > 0;
}

if (canActivateService(subscription, account)) {
  // ...
}
```

**[⬆ torna all'inizio](#indice)**

### Evita i condizionali negativi

**Sbagliato:**

```ts
function isEmailNotUsed(email: string): boolean {
  // ...
}

if (isEmailNotUsed(email)) {
  // ...
}
```

**Corretto:**

```ts
function isEmailUsed(email: string): boolean {
  // ...
}

if (!isEmailUsed(email)) {
  // ...
}
```

**[⬆ torna all'inizio](#indice)**

### Evita i condizionali

Questo sembra impossibile. Sentendo questa frase la maggior parte delle persone chiede "come posso fare qualcosa senza gli `if` statement?" La risposta è che il polimorfismo può raggiungere lo stesso obbiettivo in molti casi. La seconda domanda è spesso "Fantastico, ma perché dovrei farlo?" La risposta è un concetto di clean code che abbiamo già imparato: una funzione dovrebbe avere un singolo scopo. QUando classi e funzioni hanno `if` statements, stai dicendo all'user che le tue funzioni fanno più di una cosa. Ricorda, un singolo scopo.

**Sbagliato:**

```ts
class Airplane {
  private type: string;
  // ...

  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
      default:
        throw new Error('Unknown airplane type.');
    }
  }

  private getMaxAltitude(): number {
    // ...
  }
}
```

**Corretto:**

```ts
abstract class Airplane {
  protected getMaxAltitude(): number {
    // shared logic with subclasses ...
  }

  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ torna all'inizio](#indice)**

### Evita type checking

TypeScript è un superset sintattico rigoroso di JavaScript e aggiunge opzionalmente static type checking al linguaggio.
Preferisci sempre di speficicare i tipi delle variabili, parametri e dei valori di ritorno per sfruttare appieno le funzionalità di TypeScript.
Rende il refactoring più facile.

**Sbagliato:**

```ts
function travelToTexas(vehicle: Bicycle | Car) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(currentLocation, new Location('texas'));
  }
}
```

**Corretto:**

```ts
type Vehicle = Bicycle | Car;

function travelToTexas(vehicle: Vehicle) {
  vehicle.move(currentLocation, new Location('texas'));
}
```

**[⬆ torna all'inizio](#indice)**

### Non ottimizzare troppo

I browser moderni eseguono molte ottimizzazioni dietro le quinte durante il runtime. Spesso, se stai ottimizzando stai perdendo tempo. Ci sono ottime [risorse](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) per vedere dove l'ottimizzazione è carente. 
Modern browsers do a lot of optimization under-the-hood at runtime. A lot of times, if you are optimizing then you are just wasting your time. There are good [resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) for seeing where optimization is lacking. Prendi di mira quellie nel frattempo, finché non verranno risolte, se possono esserlo.

**Sbagliato:**

```ts
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Corretto:**

```ts
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ torna all'inizio](#indice)**

### Rimuovi codice morto

Il codice morto è brutto come il codice duplicato. Non c'è ragione per lasciarlo nel codebase.
Se non è chiamato, rimuovilo! Resterà comunque salvato nella version history se servirà di nuovo.

**Sbagliato:**

```ts
function oldRequestModule(url: string) {
  // ...
}

function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**Corretto:**

```ts
function requestModule(url: string) {
  // ...
}

const req = requestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**[⬆ torna all'inizio](#indice)**

### Usa iteratori e generatori

Usa generatori e iterabili per lavorare con collezioni di dati in stream.
Alcune ragioni:

- separa l'implementazione del "calee", il calee decide quanti a quanti oggetti accedere
- lazy execution, gli oggetti vengono inviati su richiesta
- Supporto per iterare oggetti tramite la sintassi  `for-of`
- Gli iterabili permettono di implementare modelli di iteratori ottimizzati

**Sbagliato:**

```ts
function fibonacci(n: number): number[] {
  if (n === 1) return [0];
  if (n === 2) return [0, 1];

  const items: number[] = [0, 1];
  while (items.length < n) {
    items.push(items[items.length - 2] + items[items.length - 1]);
  }

  return items;
}

function print(n: number) {
  fibonacci(n).forEach(fib => console.log(fib));
}

// Print first 10 Fibonacci numbers.
print(10);
```

**Corretto:**

```ts
// Generates an infinite stream of Fibonacci numbers.
// The generator doesn't keep the array of all numbers.
function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];

  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

function print(n: number) {
  let i = 0;
  for (const fib of fibonacci()) {
    if (i++ === n) break;  
    console.log(fib);
  }  
}

// Print first 10 Fibonacci numbers.
print(10);
```

Sono presenti librerie per facilitare lavorare con iterabili similarmente ad array nativi, concatenando metodi come `map`, `slice`, `forEach` etc.
Vedi [itiriri](https://www.npmjs.com/package/itiriri) per un esempio di manipolazione avanzata con iterabili o [itiriri-async](https://www.npmjs.com/package/itiriri-async) per manipolazione di iterabili asincroni.

```ts
import itiriri from 'itiriri';

function* fibonacci(): IterableIterator<number> {
  let [a, b] = [0, 1];
 
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

itiriri(fibonacci())
  .take(10)
  .forEach(fib => console.log(fib));
```

**[⬆ torna all'inizio](#indice)**

## Oggetti e Strutture Dati

### Usa getters e setters

TypeScript supporta la sintassi getter/setter.
Usare getters e setters per accedere a dati in oggetti che incapsulano il loro comportamento potrebbe essere più semplice che cercare la proprietà di un oggetto.
"Perché?" potresti chiederti. Qui una lista di ragioni:

- Quando c'è bisogno di più che accedere alla proprietà di un oggetto, non devi cercare e cambiare tutti gli accessors nel codebase.
- Semplifica la validazione con un semplice `set`.
- Incapsula la rappresentazione interna.
- Facile aggiungere registrazione e gestione di errori.
- Lazy load le proprietà di un oggetto, per esempio ricevendolo da un server.

**Sbagliato:**

```ts
type BankAccount = {
  balance: number;
  // ...
}

const value = 100;
const account: BankAccount = {
  balance: 0,
  // ...
};

if (value < 0) {
  throw new Error('Cannot set negative balance.');
}

account.balance = value;
```

**Corretto:**

```ts
class BankAccount {
  private accountBalance: number = 0;

  get balance(): number {
    return this.accountBalance;
  }

  set balance(value: number) {
    if (value < 0) {
      throw new Error('Cannot set negative balance.');
    }

    this.accountBalance = value;
  }

  // ...
}

// Ora `BankAccount` incapsula la logica di validazione.
// Se un giorno le specificazioni cambiano, e c'è bisogno di validazione extra,
// bisognerà solo cambiare l'implementazione del  `setter`,
// lasciando immutato tutto il codice dipendente.
const account = new BankAccount();
account.balance = 100;
```

**[⬆ torna all'inizio](#indice)**

### Usa membri privati/protetti negli oggetti

TypeScript supporta `public` *(predefinito)*, `protected` and `private` accessori per i membri delle classi.  

**Sbagliato:**

```ts
class Circle {
  radius: number;
  
  constructor(radius: number) {
    this.radius = radius;
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**Corretto:**

```ts
class Circle {
  constructor(private readonly radius: number) {
  }

  perimeter() {
    return 2 * Math.PI * this.radius;
  }

  surface() {
    return Math.PI * this.radius * this.radius;
  }
}
```

**[⬆ torna all'inizio](#indice)**

### Preferisci l'immutabilità

Il sistema dei tipi di TypeScript consente di segnare singole proprietà di interfacce/classi come *readonly*. Questo consente di lavorare in maniera funzionale (prevenendo mutazioni inaspettate).  
Per scenari più avanzati, il tipo `Readonly` che prende un tipo `T` rende tutte le proprietà readonly usando mapped types (vedi [mapped types](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)).

**Sbagliato:**

```ts
interface Config {
  host: string;
  port: string;
  db: string;
}
```

**Corretto:**

```ts
interface Config {
  readonly host: string;
  readonly port: string;
  readonly db: string;
}
```

Per gli array, `ReadonlyArray<T>` può essere usato per crearli readonly.
Questo non consente modifiche come `push()` e `fill()`, ma consente funzionalità come `concat()` e `slice()` che non cambiano il valore dell'array.

**Sbagliato:**

```ts
const array: number[] = [ 1, 3, 5 ];
array = []; // error
array.push(100); // array will be updated
```

**Corretto:**

```ts
const array: ReadonlyArray<number> = [ 1, 3, 5 ];
array = []; // error
array.push(100); // error
```

Dichiarare argomenti read-only in [TypeScript 3.4 è più facile](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#improvements-for-readonlyarray-and-readonly-tuples).

```ts
function hoge(args: readonly string[]) {
  args.push(1); // error
}
```

Preferisci [const assertions](https://github.com/microsoft/TypeScript/wiki/What's-new-in-TypeScript#const-assertions) per valori letterali.

**Sbagliato:**

```ts
const config = {
  hello: 'world'
};
config.hello = 'world'; // value is changed

const array  = [ 1, 3, 5 ];
array[0] = 10; // value is changed

// writable objects is returned
function readonlyData(value: number) {
  return { value };
}

const result = readonlyData(100);
result.value = 200; // value is changed
```

**Corretto:**

```ts
// read-only object
const config = {
  hello: 'world'
} as const;
config.hello = 'world'; // error

// read-only array
const array  = [ 1, 3, 5 ] as const;
array[0] = 10; // error

// You can return read-only objects
function readonlyData(value: number) {
  return { value } as const;
}

const result = readonlyData(100);
result.value = 200; // error
```

**[⬆ torna all'inizio](#indice)**

### tipo vs. interfaccia

Usa un tipo quando avresti bisogno di una unione o intersezione. Usa un'interfaccia quando vuoi usare `extends` or `implements`. Non ci sono regole precise, usa quello che preferisci.
Per una spiegazione più dettagliata, vedi questa [risposta](https://stackoverflow.com/questions/37233735/typescript-interfaces-vs-types/54101543#54101543) riguardo le differenze tra `type` e `interface` su TypeScript.

**Sbagliato:**

```ts
interface EmailConfig {
  // ...
}

interface DbConfig {
  // ...
}

interface Config {
  // ...
}

//...

type Shape = {
  // ...
}
```

**Corretto:**

```ts

type EmailConfig = {
  // ...
}

type DbConfig = {
  // ...
}

type Config  = EmailConfig | DbConfig;

// ...

interface Shape {
  // ...
}

class Circle implements Shape {
  // ...
}

class Square implements Shape {
  // ...
}
```

**[⬆ torna all'inizio](#indice)**

## Classi

### Le classi dovrebbero essere piccole

La grandezza di una classe si misura dalle sue responsabilità. Seguendo il *principio di singola responsabilità*, le classi saranno piccole.

**Sbagliato:**

```ts
class Dashboard {
  getLanguage(): string { /* ... */ }
  setLanguage(language: string): void { /* ... */ }
  showProgress(): void { /* ... */ }
  hideProgress(): void { /* ... */ }
  isDirty(): boolean { /* ... */ }
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  addSubscription(subscription: Subscription): void { /* ... */ }
  removeSubscription(subscription: Subscription): void { /* ... */ }
  addUser(user: User): void { /* ... */ }
  removeUser(user: User): void { /* ... */ }
  goToHomePage(): void { /* ... */ }
  updateProfile(details: UserDetails): void { /* ... */ }
  getVersion(): string { /* ... */ }
  // ...
}

```

**Corretto:**

```ts
class Dashboard {
  disable(): void { /* ... */ }
  enable(): void { /* ... */ }
  getVersion(): string { /* ... */ }
}

// split the responsibilities by moving the remaining methods to other classes
// ...
```

**[⬆ torna all'inizio](#indice)**

### Alta coesione e basso accoppiamento

La coesione definisce il grado in cui i membri di una classe sono relazionati tra loro. Idealmente, tutti i campi di una classe dovrebbero essere usati da ogni metodo.
Possiamo allora dire che una classe è *coesiva al massimo*. In pratica questo non è sempre possibile, o consigliabile. La coesione dovrebbe comunque essere alta.

Accoppiamento si riferisce a quanto dipendenti sono due classi tra loro. Le classi hanno un basso grado di accoppiamneto se non si influenzano tra loro. 

Buon software design ha **alta coesione** e **basso grado di accoppiamento**.

**Sbagliato:**

```ts
class UserManager {
  // Bad: each private variable is used by one or another group of methods.
  // It makes clear evidence that the class is holding more than a single responsibility.
  // If I need only to create the service to get the transactions for a user,
  // I'm still forced to pass and instance of `emailSender`.
  constructor(
    private readonly db: Database,
    private readonly emailSender: EmailSender) {
  }

  async getUser(id: number): Promise<User> {
    return await db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await db.transactions.find({ userId });
  }

  async sendGreeting(): Promise<void> {
    await emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**Corretto:**

```ts
class UserService {
  constructor(private readonly db: Database) {
  }

  async getUser(id: number): Promise<User> {
    return await this.db.users.findOne({ id });
  }

  async getTransactions(userId: number): Promise<Transaction[]> {
    return await this.db.transactions.find({ userId });
  }
}

class UserNotifier {
  constructor(private readonly emailSender: EmailSender) {
  }

  async sendGreeting(): Promise<void> {
    await this.emailSender.send('Welcome!');
  }

  async sendNotification(text: string): Promise<void> {
    await this.emailSender.send(text);
  }

  async sendNewsletter(): Promise<void> {
    // ...
  }
}
```

**[⬆ torna all'inizio](#indice)**

### Preferisci composizione a editarietà 

Come dichiarato famosamente su [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) da Gang of Four, dovresti *preferire composizione all'ereditarietà* dove possibile. Ci sono molte buone ragioni per usare l'ereditarietà e molte per usare la composizione. Il punto principale di questa massima è che se pensi istintivamente a ereditarietà, prova a pensare se la composizione potrebbe modellare meglio il problema. In alcuni casi può.

Potresti pensare allora, "quando dovrei usare l'ereditarità?" Dipende dal problema, ma questa è una lista dove ha più senso della composizione:

1. L'ereditarietà rappresenta una relazione "is-a" e non "has-a". (Umani->Animali vs. User->UserDetails).

2. Puoi riutilizzare codice da una della classi di base. (Gli umani possono muoversi come tutti gli animali).

3. Vuoi applicare cambiamenti globali la classi derivate utilizzando una classe base. (Cambiare il consumo calorico di tutti gli animali quando si muovono).

**Sbagliato:**

```ts
class Employee {
  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(
    name: string,
    email: string,
    private readonly ssn: string,
    private readonly salary: number) {
    super(name, email);
  }

  // ...
}
```

**Corretto:**

```ts
class Employee {
  private taxData: EmployeeTaxData;

  constructor(
    private readonly name: string,
    private readonly email: string) {
  }

  setTaxData(ssn: string, salary: number): Employee {
    this.taxData = new EmployeeTaxData(ssn, salary);
    return this;
  }

  // ...
}

class EmployeeTaxData {
  constructor(
    public readonly ssn: string,
    public readonly salary: number) {
  }

  // ...
}
```

**[⬆ torna all'inizio](#indice)**

### Preferisci concatenare metodi

Questo pattern è molto utile e usato comunemente da molte librerie. 
This pattern is very useful and commonly used in many libraries. Consente al tuo codice di essere espressivo e meno verboso. Per questa ragione, concatena metodi e osserva quanto pulito sarà il tuo codice.

**Sbagliato:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): void {
    this.collection = collection;
  }

  page(number: number, itemsPerPage: number = 100): void {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
  }

  orderBy(...fields: string[]): void {
    this.orderByFields = fields;
  }

  build(): Query {
    // ...
  }
}

// ...

const queryBuilder = new QueryBuilder();
queryBuilder.from('users');
queryBuilder.page(1, 100);
queryBuilder.orderBy('firstName', 'lastName');

const query = queryBuilder.build();
```

**Corretto:**

```ts
class QueryBuilder {
  private collection: string;
  private pageNumber: number = 1;
  private itemsPerPage: number = 100;
  private orderByFields: string[] = [];

  from(collection: string): this {
    this.collection = collection;
    return this;
  }

  page(number: number, itemsPerPage: number = 100): this {
    this.pageNumber = number;
    this.itemsPerPage = itemsPerPage;
    return this;
  }

  orderBy(...fields: string[]): this {
    this.orderByFields = fields;
    return this;
  }

  build(): Query {
    // ...
  }
}

// ...

const query = new QueryBuilder()
  .from('users')
  .page(1, 100)
  .orderBy('firstName', 'lastName')
  .build();
```

**[⬆ torna all'inizio](#indice)**

## SOLID

### Principio di singola responsabilità (SRP)

Come indicato su Clean Code, "Non dovrebbe mai esserci più di una ragione per cambiare una classe". È allettante riempire una classe con molte funzionalità, come quando puoi portare solo una valigia sul tuo volo. Il problema è che una classe non sarà mai concettualmente coesiva e avrà sempre ragioni per cambiare. Minimizzare il tempo necessario per cambiare una classe è importante, perché avendo troppa funzionalità quando ci sarà bisogno di cambiarne un pezzo, sarà difficile capire quali saranno gli effetti su altri moduli dipendenti nel codebase. 

**Sbagliato:**

```ts
class UserSettings {
  constructor(private readonly user: User) {
  }

  changeSettings(settings: UserSettings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Corretto:**

```ts
class UserAuth {
  constructor(private readonly user: User) {
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  private readonly auth: UserAuth;

  constructor(private readonly user: User) {
    this.auth = new UserAuth(user);
  }

  changeSettings(settings: UserSettings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ torna all'inizio](#indice)**

### Principio aperto/chiuso (OCP)

Come dichiarato da Bertrand Meyer, "Entità software (classi, moduli, funzioni, etc.) dovrebbero essere aperte per estensioni, ma chiuse per modifiche. Cosa significa? Questo principio afferma fondamentalmente che una entità può aggiungere nuove funzionalità finchè la funzionalità esistente non viene alterata.

**Sbagliato:**

```ts
class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    if (this.adapter instanceof AjaxAdapter) {
      const response = await makeAjaxCall<T>(url);
      // transform response and return
    } else if (this.adapter instanceof NodeAdapter) {
      const response = await makeHttpCall<T>(url);
      // transform response and return
    }
  }
}

function makeAjaxCall<T>(url: string): Promise<T> {
  // request and return promise
}

function makeHttpCall<T>(url: string): Promise<T> {
  // request and return promise
}
```

**Corretto:**

```ts
abstract class Adapter {
  abstract async request<T>(url: string): Promise<T>;

  // code shared to subclasses ...
}

class AjaxAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // request and return promise
  }

  // ...
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
  }

  async request<T>(url: string): Promise<T>{
    // request and return promise
  }

  // ...
}

class HttpRequester {
  constructor(private readonly adapter: Adapter) {
  }

  async fetch<T>(url: string): Promise<T> {
    const response = await this.adapter.request<T>(url);
    // transform response and return
  }
}
```

**[⬆ torna all'inizio](#indice)**

### Principio di sostituzione di Liskov (LSP)

Un termine spaventoso per un concetto semplice. Formalmente definito come "Se S è un subtipo di T, oggetti di tipo T possono essere rimpiazzati con oggetti di tipo S (cioè oggetti di tipo S possono sostituire oggetti di tipo T) senza alterare nessuna delle proprietà desiderabili del programma (correttezza, compiti eseguiti, etc.)". Una definizione ancora più spaventosa.

La miglior spiegazione per questo, avendo una classe madre e una figlia, madre e figlia possono essere usate intercambiabilmente senza risultati scorretti. Questo potrebbe portare confusione, consideriamo il classico esempio Quadrato-Rettangolo. Matematicamente, un quadrato è un rettangolo, ma modellandolo usando una relazione "is-a" tramite ereditarietà, può portare problemi. 

**Sbagliato:**

```ts
class Rectangle {
  constructor(
    protected width: number = 0,
    protected height: number = 0) {

  }

  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  setWidth(width: number): this {
    this.width = width;
    return this;
  }

  setHeight(height: number): this {
    this.height = height;
    return this;
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width: number): this {
    this.width = width;
    this.height = width;
    return this;
  }

  setHeight(height: number): this {
    this.width = height;
    this.height = height;
    return this;
  }
}

function renderLargeRectangles(rectangles: Rectangle[]) {
  rectangles.forEach((rectangle) => {
    const area = rectangle
      .setWidth(4)
      .setHeight(5)
      .getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Corretto:**

```ts
abstract class Shape {
  setColor(color: string): this {
    // ...
  }

  render(area: number) {
    // ...
  }

  abstract getArea(): number;
}

class Rectangle extends Shape {
  constructor(
    private readonly width = 0,
    private readonly height = 0) {
    super();
  }

  getArea(): number {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(private readonly length: number) {
    super();
  }

  getArea(): number {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes: Shape[]) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ torna all'inizio](#indice)**

### Principio di segregazione delle interfacce (ISP)

L'ISP afferma che "I clienti non dovrebbero essere costretti a dipendere da interfacce che non utilizzano.". Questo principio è correlato al principio di singola responsabilità.
Questo significa che le astrazioni dovrebbero sempre essere progettate in modo che i client che ne utilizzano i metodi esposti non ricevano l'intera torta.Questo include imporre ai client la responsabilità di implementare medoti di cui hanno veramente bisogno.

**Sbagliato:**

```ts
interface SmartPrinter {
  print();
  fax();
  scan();
}

class AllInOnePrinter implements SmartPrinter {
  print() {
    // ...
  }  
  
  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements SmartPrinter {
  print() {
    // ...
  }  
  
  fax() {
    throw new Error('Fax not supported.');
  }

  scan() {
    throw new Error('Scan not supported.');
  }
}
```

**Corretto:**

```ts
interface Printer {
  print();
}

interface Fax {
  fax();
}

interface Scanner {
  scan();
}

class AllInOnePrinter implements Printer, Fax, Scanner {
  print() {
    // ...
  }  
  
  fax() {
    // ...
  }

  scan() {
    // ...
  }
}

class EconomicPrinter implements Printer {
  print() {
    // ...
  }
}
```

**[⬆ torna all'inizio](#indice)**

### Principio di inversione delle dipendenze (DIP)

Questo principio 
This principle afferma due cose essenziali:

1. I moduli di alto livello non dovrebbero dipendere dai moduli di basso livello. Entrambi dovrebbero dipendere da astrazioni.

2. Le astrazioni non dovrebbero dipendere dai dettagli.I dettagli dovrebbero dipendere da astrazioni.

Questo può essere inzialmente difficile da capire, ma avendo lavorato con Angular, si può vedere l'implementazione di questo principio nella forma di Dependency Injection (DI).
Anche non essendo concetti identici, il DIP previene moduli di alto livello dal conoscere i dettagli dei moduli di basso livello e di configurarli. Questo può essere fatto attraverso DI. Un grande beneficio è la riduzione dell'accoppiamento tra moduli. L'accoppiamento (coupling) è un pessimo pattern di sviluppo in quanto rende il codice più difficile modificare.

Il DIP si ottiene solitamente utilizzando un contenitore a inversione di controllo (IoC). Un esempio di un robusto IoC per TypeScript è [InversifyJs](https://www.npmjs.com/package/inversify).

**Sbagliato:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

class XmlFormatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}

class ReportReader {

  // BAD: We have created a dependency on a specific request implementation.
  // We should just have ReportReader depend on a parse method: `parse`
  private readonly formatter = new XmlFormatter();

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader();
const report = await reader.read('report.xml');
```

**Corretto:**

```ts
import { readFile as readFileCb } from 'fs';
import { promisify } from 'util';

const readFile = promisify(readFileCb);

type ReportData = {
  // ..
}

interface Formatter {
  parse<T>(content: string): T;
}

class XmlFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts an XML string to an object T
  }
}


class JsonFormatter implements Formatter {
  parse<T>(content: string): T {
    // Converts a JSON string to an object T
  }
}

class ReportReader {
  constructor(private readonly formatter: Formatter) {
  }

  async read(path: string): Promise<ReportData> {
    const text = await readFile(path, 'UTF8');
    return this.formatter.parse<ReportData>(text);
  }
}

// ...
const reader = new ReportReader(new XmlFormatter());
const report = await reader.read('report.xml');

// or if we had to read a json report
const reader = new ReportReader(new JsonFormatter());
const report = await reader.read('report.json');
```

**[⬆ torna all'inizio](#indice)**

## Testing

Il testing è più importante che rilasciare. Senza test o con pochi e inadeguati, ogni volta che il codice viene rilasciato c'è il rischio di rompere qualcosa.

Decidere quanto è l'ammontare di testing adeguato spetta a tutto il team, ma avere una copertura del 100% (tutti gli statements e branches) è l'unico modo per ottenere un alto livello di confidenza e tranquillità nello sviluppo. Questo significa che oltre ad avere un buon framework di testing, vi è anche bisogno di un buon [coverage tool](https://github.com/gotwarlost/istanbul).

Non c'è scusa per non scrivere test. Sono presenti [parecchi ottimi frameworks per testing in JS](http://jstherightway.org/#testing-tools) con supporto per Typecript, ogni team ha ampia scelta.
Una volta trovato uno che funziona per tutto il team, l'obbiettivo sarà scrivere test per ogni nuovo modulo e funzionalità. Se il tuo modo preferito è il Test Driven Development (TDD), ottimo, ma il punto principale è raggiungere gli obbiettivi di copertura prima di rilasciare qualisiasi funzionalità, o modificarne una già esistente.

### Le tre regole del TDD

1. Non ti è consentito scrivere alcun codice a meno che non serva per far passare un unit test fallito.

2. Non ti è consentito scrivere più di un unit test di quanto sia necessario a farlo fallire, e fallimenti di compilazione sono fallimenti.

3. Non ti è consentito scrivere più codice di quanto sia sufficiente a far passare un unit test fallito.

**[⬆ torna all'inizio](#indice)**

### Regole F.I.R.S.T.

Buon codice dovrebbe seguire queste regole:

- **Fast (Veloce)** I test dovrebbero essere veloci per essere eseguiti frequentemente.

- **Independent (Indipendenti)** I test non dovrebbero dipendere tra loro. Dovrebbero ritornare lo stesso output sia se eseguiti da soli o insieme in qualsiasi ordine.

- **Repeatable (Ripetibili)** I test dovrebbero essere ripetibili in qualsiasi ambiente e non dovrebbero esserci scuse per il loro fallimento.

- **Self-Validating (Auto-validanti)** I test dovrebbero rispondere con *Passato* o *Fallito*. Non dovrebbe essere necessario comparare log files per vedere se un test è passato.

- **Timely (Puntuale)** I test dovrebbero essere scritti prima del codice. Se scrivi un test dopo aver scritto il relativo codice, i test potrebbero essere troppo complicati.

**[⬆ torna all'inizio](#indice)**

### Un singolo concetto per ogni test

I test dovrebbero seguire il *Principio di singola responsabilità*. Un singolo assert per unit test.

**Sbagliato:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles date boundaries', () => {
    let date: AwesomeDate;

    date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));

    date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));

    date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**Corretto:**

```ts
import { assert } from 'chai';

describe('AwesomeDate', () => {
  it('handles 30-day months', () => {
    const date = new AwesomeDate('1/1/2015');
    assert.equal('1/31/2015', date.addDays(30));
  });

  it('handles leap year', () => {
    const date = new AwesomeDate('2/1/2016');
    assert.equal('2/29/2016', date.addDays(28));
  });

  it('handles non-leap year', () => {
    const date = new AwesomeDate('2/1/2015');
    assert.equal('3/1/2015', date.addDays(28));
  });
});
```

**[⬆ torna all'inizio](#indice)**

### Il nome di un test dovrebbe rivelare il suo intento

Quando un test fallisce, il suo nome dovrebbe indicare cosa è andato storto.

**Sbagliato:**

```ts
describe('Calendar', () => {
  it('2/29/2020', () => {
    // ...
  });

  it('throws', () => {
    // ...
  });
});
```

**Corretto:**

```ts
describe('Calendar', () => {
  it('should handle leap year', () => {
    // ...
  });

  it('should throw when format is invalid', () => {
    // ...
  });
});
```

**[⬆ torna all'inizio](#indice)**

## Concorrenza

### Preferisci promesse a callbacks

I callbacks non sono puliti e causano nesting eccessivo *(the callback hell)*.
Sono presenti tools che trasformano funzioni esistenti che usano callbacks a versioni che ritornano promesse (per Node.js vedi [`util.promisify`](https://nodejs.org/dist/latest-v8.x/docs/api/util.html#util_util_promisify_original), per scopi generali vedi [pify](https://www.npmjs.com/package/pify), [es6-promisify](https://www.npmjs.com/package/es6-promisify))

**Sbagliato:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';

function downloadPage(url: string, saveTo: string, callback: (error: Error, content?: string) => void) {
  get(url, (error, response) => {
    if (error) {
      callback(error);
    } else {
      writeFile(saveTo, response.body, (error) => {
        if (error) {
          callback(error);
        } else {
          callback(null, response.body);
        }
      });
    }
  });
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html', (error, content) => {
  if (error) {
    console.error(error);
  } else {
    console.log(content);
  }
});
```

**Corretto:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url)
    .then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));  
```

Le promesse supportano dei metodi helpers per rendere il loro codice più conciso:

| Modello                  | Descrizione                                  |
| ------------------------ | -------------------------------------------- |
| `Promise.resolve(value)` | Converte un valore a una promessa risolta.   |
| `Promise.reject(error)`  | Converte un valore a una promessa rifiutata. |
| `Promise.all(promises)`  | Restituisce una nuova promessa che viene risolta quando tutte le promesse nell'array vengono risolte o che viene rifiutata appena una delle promesse nell'array viene rifiutata. |
| `Promise.race(promises)`| Restituire una nuova promessa che viene risolta/rifiutata con il risultato/errore della prima promessa che viene risolta. |

`Promise.all` è particolarmente utile quando è necessario eseguire task in parallelo. `Promise.race` rende più facile implementare cose come timeouts per promesse.

**[⬆ torna all'inizio](#indice)**

### Async/Await è più pulito delle Promesse

Con la sintassi di `async`/`await` è possibile scrivere codice più pulito e comprensivile di promesse concatenate. Dentro una funzione con prefisso `async`, è possibile indicare al runtime JavaScript di mettere in pausa l'esecuzione del codice in corrispondenza della parola `await` (quando usato in una promessa).

**Sbagliato:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = util.promisify(writeFile);

function downloadPage(url: string, saveTo: string): Promise<string> {
  return get(url).then(response => write(saveTo, response));
}

downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', 'article.html')
  .then(content => console.log(content))
  .catch(error => console.error(error));  
```

**Corretto:**

```ts
import { get } from 'request';
import { writeFile } from 'fs';
import { promisify } from 'util';

const write = promisify(writeFile);

async function downloadPage(url: string): Promise<string> {
  const response = await get(url);
  return response;
}

// somewhere in an async function
try {
  const content = await downloadPage('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
  await write('article.html', content);
  console.log(content);
} catch (error) {
  console.error(error);
}
```

**[⬆ torna all'inizio](#indice)**

## Gestione degli errori

Generare errori è una buona cosa! Significa che il runtime ha identificato con successo che qualcosa nel programma è andato storto e lo comunica interrompendo l'esecuzione della funzione sullo stack corrente, terminando il processo (su Node), e notificando nella console della traccia nello stack. 

### Usa sempre errori per rigettare o rifiutare (throw e reject)

JavaScript e TypeScript consentono di usare `throw` su qualsiasi oggetto. Anche una promessa può essere rifiutata usando un oggetto.
È consigliato usare sempre la sintassi `throw` con il tipo `Error`. Questo perchè un errore potrebbe essere intercettato a un livello più alto tramite la sintassi `catch`.
Sarebbe molto confusionario catturare una stringa e renderebbe il [debugging più difficile](https://basarat.gitbook.io/typescript/type-system/exceptions#always-use-error).

Per le stesse ragioni, le promesse dovrebbero essere rifiutate usando il tipo `Error`.

**Sbagliato:**

```ts
function calculateTotal(items: Item[]): number {
  throw 'Not implemented.';
}

function get(): Promise<Item[]> {
  return Promise.reject('Not implemented.');
}
```

**Corretto:**

```ts
function calculateTotal(items: Item[]): number {
  throw new Error('Not implemented.');
}

function get(): Promise<Item[]> {
  return Promise.reject(new Error('Not implemented.'));
}

// or equivalent to:

async function get(): Promise<Item[]> {
  throw new Error('Not implemented.');
}
```

Il beneficio di usare il tipi `Error` è il supporto della sintassi `try/catch/finally` e che implicitamente tutti gli errori hanno la proprietà `stack` molto utile per debugging.
Sono presenti alternative a usare la sintassi `throw` ritornando invece oggetti errore personalizzati. TypeScript rende questo molto facile.
Considera il seguente esempio:

```ts
type Result<R> = { isError: false, value: R };
type Failure<E> = { isError: true, error: E };
type Failable<R, E> = Result<R> | Failure<E>;

function calculateTotal(items: Item[]): Failable<number, 'empty'> {
  if (items.length === 0) {
    return { isError: true, error: 'empty' };
  }

  // ...
  return { isError: false, value: 42 };
}
```

Per una spiegazione dettagliata di questa idea far riferimento al [post originale](https://medium.com/@dhruvrajvanshi/making-exceptions-type-safe-in-typescript-c4d200ee78e9).

**[⬆ torna all'inizio](#indice)**

### Non ignorare gli errori catturati

Ignorando un errore, non avrai la possibilità di correggerlo o reagire. Registrare l'errore sulla console (`console.log`) non è molto meglio in quanto può essere perso tra lo spam stampato nella console. Avvolgere codice in un `try/catch` significa che un errore potrebbe essere generato e serve quindi un piano o un percorso nel codice deve essere creato per quando questo succede.

**Sbagliato:**

```ts
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}

// or even worse

try {
  functionThatMightThrow();
} catch (error) {
  // ignore error
}
```

**Corretto:**

```ts
import { logger } from './logging'

try {
  functionThatMightThrow();
} catch (error) {
  logger.log(error);
}
```

**[⬆ torna all'inizio](#indice)**

### Non ignorare promesse rifiutate

Per le stesse ragioni, non dovresti ignorare errori catturati in un `try/catch`.

**Sbagliato:**

```ts
getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    console.log(error);
  });
```

**Corretto:**

```ts
import { logger } from './logging'

getUser()
  .then((user: User) => {
    return sendEmail(user.email, 'Welcome!');
  })
  .catch((error) => {
    logger.log(error);
  });

// or using the async/await syntax:

try {
  const user = await getUser();
  await sendEmail(user.email, 'Welcome!');
} catch (error) {
  logger.log(error);
}
```

**[⬆ torna all'inizio](#indice)**

## Formattazione

La formattazione è soggettiva. Come molte regole qui scritte, non esistono regole rigide e veloci da seguire. Il punto principale è di *NON LITIGARE* per la formattazione. Esistono molti strumenti per autonomatizzarlo. Molto tempo e soldi vengono sprecati in discussioni sulla formattazione. In generale la regola da seguire è *suguire regole di formattazione consistenti*.

Per TypeScript, esite uno strumento molto potente chiamato [ESLint](https://typescript-eslint.io/). È uno strumento di analisi statico che aiuta a migliorare drammaticamente la leggibilità e capacità di mantenere codice. Esistono configurazioni già pronte per ESLint che possono essere usate come referenze:

- [ESLint Config Airbnb](https://www.npmjs.com/package/eslint-config-airbnb-typescript) - Airbnb style guide

- [ESLint Base Style Config](https://www.npmjs.com/package/eslint-plugin-base-style-config) - set di regole essenziali per JS, TS e React

- [ESLint + Prettier](https://www.npmjs.com/package/eslint-config-prettier) - regole di linting per [Prettier](https://github.com/prettier/prettier) code formatter

Fare riferimento inoltre a questa guida [TypeScript StyleGuide and Coding Conventions](https://basarat.gitbook.io/typescript/styleguide).

### Migrare da TSLint a ESLint

Se serve aiuto per migrare da TSLint a ESLint, fai riferimento a questo progetto: <https://github.com/typescript-eslint/tslint-to-eslint-config>

### Usa la capitalizzazione consistente

La capitalizzazione dice molto sulle variabili, funzioni, etc. Queste regole sono soggettive, quindi ogni team può scegliere come vuole. Il punto è di *essere consistenti*.

**Sbagliato:**

```ts
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

type animal = { /* ... */ }
type Container = { /* ... */ }
```

**Corretto:**

```ts
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

const discography = getArtistDiscography('ACDC');
const beatlesSongs = SONGS.filter((song) => isBeatlesSong(song));

function eraseDatabase() {}
function restoreDatabase() {}

type Animal = { /* ... */ }
type Container = { /* ... */ }
```

Preferire `PascalCase` per classi, interfacce, tipi e nomi di namespaces.
Preferire `camelCase` per variabili, funzioni e membri di classi.
Preferire `SNAKE_CASE` maiuscolo per costanti.

**[⬆ torna all'inizio](#indice)**

### I chiamanti e i chiamati delle funzioni dovrebbero essere vicini

Se una funzione ne chiama un'altra, queste funzioni dovrebbero essere verticalmente vicine nel loro file. Idealmente, il chiamante dovrebbe essere sopra il chiamato.
Tendiamo a leggere codice dall'alto verso il basso, come un libro. Per questa ragione, il codice dobrebbe seguire lo stesso principio.

**Sbagliato:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**Corretto:**

```ts
class PerformanceReview {
  constructor(private readonly employee: Employee) {
  }

  review() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();

    // ...
  }

  private getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  private lookupPeers() {
    return db.lookup(this.employee.id, 'peers');
  }

  private getManagerReview() {
    const manager = this.lookupManager();
  }

  private lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  private getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.review();
```

**[⬆ torna all'inizio](#indice)**

### Organizzare gli import statements

Con import statements puliti e facili da leggere è più veloce vedere le dipendenze del codice. È importante applicare le seguenti pratiche per `import` statements:

- Import statements dovrebbero essere alfabetizzati e raggruppati.
- Import inutilizzati dovrebbero essere rimossi.
- Named imports devono essere alfabetizzati (ex. `import {A, B, C} from 'foo';`).
- Fonti di import devono essere alfabetizzati nei rispettivi gruppi, ex: `import * as foo from 'a'; import * as bar from 'b';`.
- Preferisi `import type` invece di `import` per import di soli tipi per prevenire cicli di dipendenze, dato che questi import vengono cancellati durante il runtime.
- Gruppi di imports devono essere delineati da righe vuote.
- I gruppi devono rispettare il seguente ordine:
  - Polyfills (ex. `import 'reflect-metadata';`)
  - Moduli builtin di Node (ex. `import fs from 'fs';`)
  - Moduli esterni (ex. `import { query } from 'itiriri';`)
  - moduli interni (i.e `import { UserService } from 'src/services/userService';`)
  - Moduli da una directory madre (ex. `import foo from '../foo'; import qux from '../../foo/qux';`)
  - Moduli dalla stessa directory o una allo stesso livello (ex. `import bar from './bar'; import baz from './bar/baz';`)

**Sbagliato:**

```ts
import { TypeDefinition } from '../types/typeDefinition';
import { AttributeTypes } from '../model/attribute';
import { Customer, Credentials } from '../model/types';
import { ApiCredentials, Adapters } from './common/api/authorization';
import fs from 'fs';
import { ConfigPlugin } from './plugins/config/configPlugin';
import { BindingScopeEnum, Container } from 'inversify';
import 'reflect-metadata';
```

**Corretto:**

```ts
import 'reflect-metadata';

import fs from 'fs';
import { BindingScopeEnum, Container } from 'inversify';

import { AttributeTypes } from '../model/attribute';
import { TypeDefinition } from '../types/typeDefinition';
import type { Customer, Credentials } from '../model/types';

import { ApiCredentials, Adapters } from './common/api/authorization';
import { ConfigPlugin } from './plugins/config/configPlugin';
```

**[⬆ torna all'inizio](#indice)**

### Usare gli alias di typescript

Crea import migliori definendo le proprietà path e baseUrl nella sezione compilerOptions nel file `tsconfig.json`.

Questo eviterà lunghi percorsi relativi negli import.

**Sbagliato:**

```ts
import { UserService } from '../../../services/UserService';
```

**Corretto:**

```ts
import { UserService } from '@services/UserService';
```

```js
// tsconfig.json
...
  "compilerOptions": {
    ...
    "baseUrl": "src",
    "paths": {
      "@services": ["services/*"]
    }
    ...
  }
...
```

**[⬆ torna all'inizio](#indice)**

## Commenti

L'uso dei commenti è un indicazione del fallimento di esprimersi senza. Il codice dovrebbe essere l'unica fonte di verità.
  
> Non commentare codice sbagliato: riscrivilo.
> — *Brian W. Kernighan and P. J. Plaugher*

### Preferire codice esplicativo invece di commenti

I commenti sono una scusa, non un requisito. Il buon codice *di solito* si documenta da solo.

**Sbagliato:**

```ts
// Check if subscription is active.
if (subscription.endDate > Date.now) {  }
```

**Corretto:**

```ts
const isSubscriptionActive = subscription.endDate > Date.now;
if (isSubscriptionActive) { /* ... */ }
```

**[⬆ torna all'inizio](#indice)**

### Non lasciare codice commentato nel codice

Il version control esiste per un motivo. Lascia il vecchio codice nella cronologia.

**Sbagliato:**

```ts
type User = {
  name: string;
  email: string;
  // age: number;
  // jobPosition: string;
}
```

**Corretto:**

```ts
type User = {
  name: string;
  email: string;
}
```

**[⬆ torna all'inizio](#indice)**

### Non usare commenti da diario

Ricorda il version control! Non c'è bisogno di codice morto, codice commentato e specialmente commenti da diario. Usa `git log` per aver la cronologia!

**Sbagliato:**

```ts
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Added type-checking (LI)
 * 2015-03-14: Implemented combine (JR)
 */
function combine(a: number, b: number): number {
  return a + b;
}
```

**Corretto:**

```ts
function combine(a: number, b: number): number {
  return a + b;
}
```

**[⬆ torna all'inizio](#indice)**

### Evitare i marcatori posizionali

Aggiungono solo rumore. I nomi delle funzioni e variabili, insieme all'indentazione e formattazione danno struttura al codice.
La maggior parte degli IDE supporta la funzione di ripiegamento del codice che permette di aprire/chiudere un blocco di codice (vedi Visual Studio Code [folding regions](https://code.visualstudio.com/updates/v1_17#_folding-regions)).

**Sbagliato:**

```ts
////////////////////////////////////////////////////////////////////////////////
// Client class
////////////////////////////////////////////////////////////////////////////////
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  ////////////////////////////////////////////////////////////////////////////////
  // public methods
  ////////////////////////////////////////////////////////////////////////////////
  public describe(): string {
    // ...
  }

  ////////////////////////////////////////////////////////////////////////////////
  // private methods
  ////////////////////////////////////////////////////////////////////////////////
  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**Corretto:**

```ts
class Client {
  id: number;
  name: string;
  address: Address;
  contact: Contact;

  public describe(): string {
    // ...
  }

  private describeAddress(): string {
    // ...
  }

  private describeContact(): string {
    // ...
  }
};
```

**[⬆ torna all'inizio](#indice)**

### Commenti TODO

Quando è necessario lasciare note nel codice per miglioramenti successivi, usa i commenti `// TODO`. La maggior parte degli IDE hanno un supporto speciale per questi commenti in modo da poterli scorrere rapidamente.

Ricorda inoltre che un commento *TODO* non è una scusa per scrivere codice sbagliato.

**Sbagliato:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**Corretto:**

```ts
function getActiveSubscriptions(): Promise<Subscription[]> {
  // TODO: ensure `dueDate` is indexed.
  return db.subscriptions.find({ dueDate: { $lte: new Date() } });
}
```

**[⬆ torna all'inizio](#indice)**

## Traduzioni

Questa guida è inoltre disponibile in altre lingue:

- ![us](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/United-States.png) Originale in **Inglese**: [labs42io/clean-code-typescript](https://github.com/labs42io/clean-code-typescript)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portoghese Brasiliano**: [vitorfreitas/clean-code-typescript](https://github.com/vitorfreitas/clean-code-typescript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Cinese**: 
  - [beginor/clean-code-typescript](https://github.com/beginor/clean-code-typescript)
  - [pipiliang/clean-code-typescript](https://github.com/pipiliang/clean-code-typescript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **Francese**: [ralflorent/clean-code-typescript](https://github.com/ralflorent/clean-code-typescript)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **Tedesco**: [mheob/clean-code-typescript](https://github.com/mheob/clean-code-typescript)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Giapponese**: [MSakamaki/clean-code-typescript](https://github.com/MSakamaki/clean-code-typescript)
- ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Koreano**: [738/clean-code-typescript](https://github.com/738/clean-code-typescript)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russo**: [Real001/clean-code-typescript](https://github.com/Real001/clean-code-typescript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spagnolo**: [3xp1o1t/clean-code-typescript](https://github.com/3xp1o1t/clean-code-typescript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turco**: [ozanhonamlioglu/clean-code-typescript](https://github.com/ozanhonamlioglu/clean-code-typescript)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ucraino**: [KirillPd/clean-code-typescript](https://github.com/KirillPd/clean-code-typescript)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hoangsetup/clean-code-typescript](https://github.com/hoangsetup/clean-code-typescript)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italiano**: [Kornil/clean-code-typescript](https://github.com/kornil/clean-code-typescript)

Le referenze vengono aggiunge quando le traduzioni sono complete.
Vedi questa [discussione](https://github.com/labs42io/clean-code-typescript/issues/15) per maggiori dettagli e progresso.
Puoi fornire un contributo indispensabile alla comunità di *Clean Code* traducendo nella tua lingua.
