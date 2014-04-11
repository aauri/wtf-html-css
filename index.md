---
layout: default
---

### Contenuti

- [Dichiara un doctype](#doctype)
- [Matematica del Box Model](#box-model-math)
- [Unità `rem` e Safari in iOS](#rems-mobile-safari)
- [Flottanti "*Floats*" prima](#floats-first)
- [*Floats* e la proprietà `clear`](#floats-clearing)
- [*Floats* e l'altezza computata](#floats-computed-height)
- [*Floats* sono a blocco (*block level*)](#floats-block-level)
- [Margini verticali collassano spesso](#vertical-margins-collapse)
- [Formattazione visuale di righe di tabelle](#styling-table-rows)
- [Firefox e i pulsanti `<input>` ](#buttons-firefox)
- [Firefox e i bordi interni dei pulsanti](#buttons-firefox-outline)
- [Assegna sempre un `type` per `<button>`](#buttons-type)
- [Limite di selettori in Internet Explorer](#ie-selector-limit)
- [Posizionamento `position` chiarito](#position-explained)
- [Posizionamento `position` e larghezza `width`](#position-width)
- [Posizionamento `fixed` con `transform`](#position-transforms)


<a name="doctype"></a>
### Dichiara un doctype
Bisogna sempre dichiarare un doctype. Vi raccomando il semplice doctype di HTML5:

```html
<!DOCTYPE html>
```

[Tralasciare il doctype può causare problemi](http://quirks.spec.whatwg.org) con malformazione di tabelle, forme `<input>`, ed altri problemi nella gestione della pagina.


<a name="box-model-math"></a>
### Matematica del Box Model
Elementi con larghezza `width` assegnata diventano più ampi quando hanno pure il cuscinetto `padding` e/o un bordo con `border-width`.
Per evitare questi problemi, utilizza l'ormai comune [*reset* `box-sizing: border-box;`](http://www.paulirish.com/2012/box-sizing-border-box-ftw/).


<a name="rems-mobile-safari"></a>
### Unità `rem` e Safari per iOS
Mentre l'uso di unità `rem` e integrato in Safari per dispositivi mobile in tutti i valori di proprietà, fa da impazzire quando `rem` sono usati in `media queries` e infinitamente lampeggiano il testo della pagina in diverse dimensioni.

Per ora, meglio usare l'unità `em` in cambio delle unità `rem`.

```css
html {
  font-size: 16px;
}

/* Causa problemi in Safari per iOS */
@media (min-width: 40rem) {
  html {
    font-size: 20px;
  }
}

/* Funziona bene in Safari per iOS */
@media (min-width: 40em) {
  html {
    font-size: 20px;
  }
}
```

**Aiuto!** *Se avete un link per segnalazione di bug per Apple or WebKit, mi piacerebbe includerlo qui. Questo problema solo succede nella versione di Safari per dispositivi mobile e non nella versione per desktop, quindi non siamo sicuri dove segnalare questo bug*.


<a name="floats-first"></a>
###Flottanti "*Floats*" prima
Elementi flottanti `float` devono sempre venire prima nell'ordine del documento. Questo è perche elementi `float` devono avere qualcosa da avvolgersi intorno, sennò sono resi con difetti di spaziatura. 

```html
<div class="parent">
  <div class="float">Float</div>
  <div class="content">
    <!-- … -->
  </div>
</div>
```


<a name="floats-clearing"></a>
### *Floats* e la proprietà `clear`
Se usi `float`, è probabilmente indispensabile usare la proprietà `clear` in un elemento successivo ad uno reso `float`, così impedisce che questo subisca il `float`. Per usare la proprietà `clear`, puoi usare uno di questi esempi.

Ecco il [micro clearfix](http://nicolasgallagher.com/micro-clearfix-hack/) che usa `clear` con una classe separata.

```css
.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}
```

Oppure, assegna `overflow`, con `auto` or `hidden` nel progenitore.

```css
.parent {
  overflow: auto; /* clearfix */
}
.other-parent {
  overflow: hidden; /* clear fix */
}
```

È importante ricordarsi che `overflow` può causare effetti collaterali non voluti, tipicamente intorno elementi all'interno del progenitore.

**Pro-Tip!** Fai un piacere a te e a i tuoi colleghi usando un commento `/* clearfix */` quando usi la proprietà `clear` per `floats` siccome la proprietà può essere usata per altre ragioni.


<a name="floats-computed-height"></a>
### *Floats* e l'altezza computata
Un elemento progenitore che solo ha contenuto `float` avrà una l'altezza computata `height: 0;`. Usa un *clearfix* nel progenitore per costringere il *browser* a calcolare l'altezza.


<a name="floats-block-level"></a>
### *Floats* sono a blocco (*block level*)
Elementi `float` sono automaticamente a blocco o `display: block;`. Non è necessario impostare la proprietà `display` siccome è ignorata a meno che non abbia il valore `none`.

```css
.element {
  float: left;
  display: block; /* Non è necessario */
}
```

***Fun fact:*** *Anni fa, dovevamo impostare `display: inline;` per far funzionare `float` correttamente in IE6 evitando il [bug](http://www.positioniseverything.net/explorer/doubled-margin.html) di [margine raddoppiato](http://gabrieleromanato.altervista.org/css/pie/doubled-margin/index.html). Comunque, quei tempi sono ormai passati.*


<a name="vertical-margins-collapse"></a>
### Margini verticali adiacenti collassano
Margini superiori e inferiori in elementi adiacenti (uno dopo l'altro) possono collassare (*collapse*) in varie situazioni, pero mai nei elementi posizionati `float` o `absolute`. Puoi leggere [questo articolo di MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/margin_collapsing) o le specifiche CSS2 su collassare i margini in [italiano](http://www.diodati.org/w3c/css2/box.html#collapsing-margins) o in [inglese](http://www.w3.org/TR/CSS2/box.html#collapsing-margins) per approfondimento.

I margini orizzontali **non collassano mai**.


<a name="styling-table-rows"></a>
### Formattazione visuale di righe di tabelle
Le righe di una tabella o `<tr>` non possono avere bordi a meno che usi il valore `border-collapse: collapse;` nel progenitore `<table>`. 
Per di più, se il `<tr>` e suoi `<td>` o `<th>` hanno lo *stesso* `border-width`, le proprietà del bordo per le righe `<tr>` sono ignorate. [Puoi vedere un esempio in questo JS Bin](http://jsbin.com/yabek/2/)


<a name="buttons-firefox"></a>
### Firefox e i pulsanti `<input>`
Per ragioni sconosciute, Firefox applica un `line-height` a i pulsanti "Invia" e bottoni di `<input>` e CSS non può cambiare questo comportamento. A questo punto hai due alternative:

1. Usa solo elementi `<button>`
2. Non usare mai `line-height` nei tuoi pulsanti.

Se usi la prima alternativa (e mi raccomando questo comunque  perché i `<button>` sono ottimi), ecco cosa è necessario sapere:

```html
<!-- Così così -->
<input type="submit" value="Salva">
<input type="button" value="Annulla">

<!-- Bene dappertutto -->
<button type="submit">Salva</button>
<button type="button">Annulla</button>
```

Se preferisci usare la seconda alternativa, basta non usare `line-height` e in cambio usare **solo** `padding` per allineare il testo del pulsante.
[Vedi questo esempio in JS Bin](http://jsbin.com/yabek/4/) in Firefox per vedere il problema e la soluzione.

**Buone notizie!** *Sembra che ci sarà [una correzione per questo](https://bugzilla.mozilla.org/show_bug.cgi?id=697451#c43) in Firefox 30. Queste sono buone notizie per noi nel futuro, pero non cambia i problemi nelle versioni precedenti.*


<a name="buttons-firefox-outline"></a>
### Firefox e i bordi interni di pulsanti

Firefox [aggiunge bordi interni](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button#Notes) a i pulsanti `<input>` e `<button>` nella proprietà `:focus`. Apparentemente è per l'accessibilità, ma la sua collocazione sembra piuttosto strana. Usa questa soluzione in CSS per ignorare questi bordi:

```css
input::-moz-focus-inner,
button::-moz-focus-inner {
  padding: 0;
  border: 0;
}
```

Puoi vedere questa soluzione in azione nello stesso [esempio in JS Bin](http://jsbin.com/yabek/4/) menzionato nella sezione anteriore.

**Pro-Tip!** *Non dimenticare di includere un stato `focus` a i link, `<botton>`, e `<input>`. Fornire usabilità per l'accessibilità è fondamentale, sia per gli utenti Pro o utenti con disabilità visive.*


<a name="buttons-type"></a>
### Assegna sempre un `type` per `<button>`
Il valore iniziale è `submit`, cioè, qualsiasi pulsante in una *form* può inviare la forma. Meglio usare `type="button"` per tutto ciò che non invia la forma e `type="submit"` per i pulsanti per inviare.

```html
<button type="submit">Salva</button>
<button type="button">Annulla</button>
```

Per azioni che richiedono di un `<button>` e non sono forme, usa `type="button"`.

```html
<button class="dismiss" type="button">x</button>
```

**Fun fact:** *Apparentemente, IE7 non supporta correttamente l'attributo `value` nel `<button>`. Invece di rendere i contenuti dell'attributo, rende dal "innerHTML" (il contenuto tra l'apertura e chiusura di `<button>`). Comunque, questo non sembra un problema enorme per due motivi: l'uso di IE7 sta abbassando, e sembra piuttosto raro impostare `value` e "innerHTML" a `<button>`*


<a name="ie-selector-limit"></a>
### Limite di selettori in Internet Explorer
Internet Explorer 9 e versioni precedenti hanno un massimo di 4,096 selettori per *stylesheet*. C'è pure un limite di 31 *stylesheet* e `<style></style>` inclusi insieme per pagina. Tutto ciò sopra di tale limite viene ignorato dal *browser*. Si deve sceglire tra dividere il CSS o il *refactoring*. Io suggerirei quest'ultimo.

Come nota utile, ecco come i *browser* contano i selettori:

```css
/* Un selettore */
.element { }

/* Più altri due selettori */
.element,
.other-element { }

/* Più altri tre selettori */
input[type="text"],
.form-control,
.form-group > input { }
```


<a name="position-explained"></a>
### Posizionamento `position` chiarito
Elementi `position: fixed;` sono posizionati rispetto alla finestra di visualizzazione *"viewport"* del browser. Elementi `position: absolute;` sono posizionati rispetto al blocco congenitore posizionato oltre a `static` (per esempio, `relative`, `absolute`, o `fixed`).


<a name="position-width"></a>
### Posizionamento `position` e larghezza `width`
Non specificare `width: 100%;` a un elemento che ha `position: [absolute|fixed];`, `left`, e `right`. L'uso di `width: 100%;` è lo stesso al uso congiunto di `left: 0;` e `right: 0;`. Usa uno o l'altro, ma non tutti e due.


<a name="position-transforms"></a>
### Posizionamento `fixed` con `transform`
Il *browser* interrompe `position: fixed;` quando il progenitore di un elemento ha la proprietà `transform`. L'uso di `transform` crea un blocco contenitore nuovo, effettivamente costringendo al progenitore a `position: relative;` e l'elemento `fixed` è reso `position: absolute;`.

[Qui c'è l'esempio](http://jsbin.com/yabek/1/) e leggi pure il [post di Eric Meyers su questo punto](http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/).
