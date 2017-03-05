---
layout: default
---

### Contenuti

- [Dichiara un doctype](#doctype)
- [Matematica del Box Model](#box-model-math)
- [Unità `rem` e Safari in iOS](#rems-mobile-safari)
- [Prima i Flottanti *Floats*](#floats-first)
- [*Floats* e la proprietà `clear`](#floats-clearing)
- [*Floats* e l'altezza computata](#floats-computed-height)
- [ I *Floats* si applicano a livello di blocco](#floats-block-level)
- [I margini verticali spesso collassano](#vertical-margins-collapse)
- [Formattazione visuale delle righe delle tabelle](#styling-table-rows)
- [Firefox e i pulsanti `<input>` ](#buttons-firefox)
- [Firefox e i bordi interni dei pulsanti](#buttons-firefox-outline)
- [Assegna sempre un `type` ai `<button>`](#buttons-type)
- [Limite di selettori in Internet Explorer](#ie-selector-limit)
- [Spiegando il posizionamento `position`](#position-explained)
- [Posizionamento `position` e larghezza `width`](#position-width)
- [Posizionamento `fixed` con `transform`](#position-transforms)


<a name="doctype"></a>
### Dichiara un doctype
Bisogna sempre dichiarare un doctype. Vi raccomando il semplice doctype di HTML5:

```html
<!DOCTYPE html>
```

[Tralasciare il doctype può causare problemi](http://quirks.spec.whatwg.org) come malformazione delle tabelle, dei tag `<input>`, e causare altri problemi nella gestione della pagina in `quirks mode`.


<a name="box-model-math"></a>
### Matematica del Box Model
Elementi con larghezza `width` assegnata diventano più ampi quando hanno pure il cuscinetto `padding` e/o un bordo con `border-width`.
Per evitare questi problemi, utilizza l'ormai comune [*reset* `box-sizing: border-box;`](http://www.paulirish.com/2012/box-sizing-border-box-ftw/).


<a name="rems-mobile-safari"></a>
### Unità `rem` e Safari per iOS
Mentre l'uso delle unità `rem` è integrato in Safari per dispositivi mobili in tutti i valori di proprietà, fa impazzire quando i `rem` sono usati nelle `media queries` facendo cambiare all'infinito il testo della pagina tra diverse dimensioni.

Per ora, meglio usare l'unità `em` al posto delle unità `rem`.

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

**Aiuto!** *Se avete un link per segnalare bug a Apple or WebKit, mi piacerebbe includerlo. Non sono sicuro dove segnalarlo dato che questo si applica soltanto alla versione di Safari per dispositivi mobili e non alla versione desktop*.


<a name="floats-first"></a>
### Prima i Flottanti *Floats*
Gli elementi flottanti `float` devono sempre essere i primi nell'ordine del documento. Questo perche gli elementi `float` devono avere qualcosa a cui avvolgersi attorno (un `wrapper`), altrimenti appaiono sotto il contenuto.

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
Se usi `float`, è probabilmente indispensabile l'utilizzo della proprietà `clear` in un elemento successivo ad uno reso `float`, così da impedire che questo subisca il `float`. Per usare la proprietà `clear`, puoi prendere spunto da uno di questi esempi.

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

Alternativamente, puoi assegnare `overflow`, con `auto` o `hidden` al padre.

```css
.parent {
  overflow: auto; /* clearfix */
}
.other-parent {
  overflow: hidden; /* clear fix */
}
```

È importante ricordarsi che `overflow` può causare effetti collaterali non voluti, tipicamente intorno agli elementi interni al padre.

**Pro-Tip!** Fai un piacere a te e a i tuoi colleghi usando un commento `/* clearfix */` quando usi la proprietà `clear` per `floats` dato che la proprietà può essere usata per altre ragioni.


<a name="floats-computed-height"></a>
### *Floats* e l'altezza computata
Un elemento padre che ha solo contenuto `float` avrà una l'altezza computata `height: 0;`. Applica un *clearfix* al padre per costringere il *browser* a calcolare l'altezza.


<a name="floats-block-level"></a>
### I *Floats* si applicano a livello di blocco
Gli elementi `float` diventano automaticamente visualizzati come blocco, ossia `display: block;`. Non è necessario impostare la proprietà `display` siccome è ignorata a meno che non abbia il valore `none`.

```css
.element {
  float: left;
  display: block; /* Non è necessario */
}
```

***Fun fact:*** *Anni fa, dovevamo impostare `display: inline;` per far funzionare `float` correttamente in IE6 evitando il [bug](http://www.positioniseverything.net/explorer/doubled-margin.html) di [margine raddoppiato](http://gabrieleromanato.altervista.org/css/pie/doubled-margin/index.html). Comunque, quei tempi sono ormai passati.*


<a name="vertical-margins-collapse"></a>
### I margini verticali adiacenti collassano
I margini superiori e inferiori in elementi adiacenti (uno dopo l'altro) possono collassare (*collapse*) in varie situazioni, pero mai negli elementi posizionati `float` o `absolute`. Puoi leggere [questo articolo di MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/margin_collapsing) o le specifiche CSS2 su come collassare i margini in [italiano](http://www.diodati.org/w3c/css2/box.html#collapsing-margins) o in [inglese](http://www.w3.org/TR/CSS2/box.html#collapsing-margins) per approfondire.

I margini orizzontali adiacenti **non collassano mai**.


<a name="styling-table-rows"></a>
### Formattazione visuale delle righe delle tabelle
Le righe di una tabella o `<tr>` non possono avere bordi a meno che non si usi il valore `border-collapse: collapse;` nel padre `<table>`.
Per di più, se il tag `<tr>` e suoi figli `<td>` o `<th>` hanno lo *stesso* `border-width`, le proprietà del bordo per le righe `<tr>` sono ignorate. [Puoi vedere un esempio in questo JS Bin](http://jsbin.com/yabek/2/)


<a name="buttons-firefox"></a>
### Firefox e i pulsanti `<input>`
Per ragioni sconosciute, Firefox applica un `line-height` al tag `<input>`, sia per i pulsanti "Invia" che per gli elementi di tipo `button`, ciò non può essere personalizzato usando i fogli di stile. A questo punto non si hanno che due alternative:

1. Usare solo i tag `<button>`
2. Non usare mai `line-height` nei pulsanti.

Se dovessi usare la prima alternativa (e mi raccomando questo comunque dato che i `<button>` sono ottimi), ecco cosa è necessario sapere:

```html
<!-- Così così -->
<input type="submit" value="Salva">
<input type="button" value="Annulla">

<!-- Bene dappertutto -->
<button type="submit">Salva</button>
<button type="button">Annulla</button>
```

Se dovessi preferire la seconda alternativa, basta non usare `line-height` e in cambio usare **solo** `padding` per allineare il testo del pulsante.
[Vedi questo esempio in JS Bin](http://jsbin.com/yabek/4/) in Firefox per vedere il problema e la soluzione.

**Buone notizie!** *Sembra che ci sarà [una correzione per questo](https://bugzilla.mozilla.org/show_bug.cgi?id=697451#c43) in Firefox 30. Queste sono buone notizie per il futuro, ma non cambieranno i problemi delle versioni precedenti.*


<a name="buttons-firefox-outline"></a>
### Firefox e i bordi interni dei pulsanti

Firefox [aggiunge bordi interni](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button#Notes) ai pulsanti `<input>` e `<button>`  per la proprietà `:focus`. Apparentemente è per ragioni di accessibilità, ma la sua collocazione sembra piuttosto strana. Usa questa soluzione in CSS per sovrascrivere questo comportamento:

```css
input::-moz-focus-inner,
button::-moz-focus-inner {
  padding: 0;
  border: 0;
}
```

Puoi vedere questa soluzione in azione nello stesso [esempio in JS Bin](http://jsbin.com/yabek/4/) menzionato nella sezione precedente.

**Pro-Tip!** *Non dimenticare di includere un stato `focus` ai link, `<botton>`, e `<input>`. Fornire un'accessibilità che sia conveniente è fondamentale, sia per gli utenti avanzati che per gli utenti con disabilità visive.*


<a name="buttons-type"></a>
### Assegna sempre un `type` ai `<button>`
Il valore iniziale è `submit`, ciò significa che qualsiasi pulsante in una *form* può effettuare un `submit`. Meglio usare `type="button"` per tutto ciò che non può effetturare il `submit` e `type="submit"` per i pulsanti che possono farlo.

```html
<button type="submit">Salva</button>
<button type="button">Annulla</button>
```

Per azioni che richiedono di un `<button>` e non sono in un form, usa `type="button"`.

```html
<button class="dismiss" type="button">x</button>
```

**Fun fact:** *Apparentemente, IE7 non supporta correttamente l'attributo `value` per i `<button>`. Invece di leggere i contenuti dell'attributo, accede a "innerHTML" (il contenuto tra l'apertura e chiusura di `<button>`). Comunque, questo non sembra un problema enorme per due motivi: l'uso di IE7 sta diminuendo, e sembra piuttosto raro impostare `value` e "innerHTML" a `<button>`*


<a name="ie-selector-limit"></a>
### Limite di selettori in Internet Explorer
Internet Explorer 9 le sue versioni precedenti hanno un massimo di 4,096 selettori per foglio di stile. C'è pure un limite complessivo di 31 fogli e tag `<style></style>` per pagina. Tutto ciò che supera tale soglia viene ignorato dal *browser*. Si deve sceglire tra dividere il CSS o avviare un *refactoring*. Io suggerirei quest'ultimo.

Come nota utile, ecco come i *browser* contano i selettori:

```css
/* Un selettore */
.element { }

/* Altri due selettori */
.element,
.other-element { }

/* Altri tre selettori */
input[type="text"],
.form-control,
.form-group > input { }
```


<a name="position-explained"></a>
### Spiegando il posizionamento `position`
Elementi `position: fixed;` sono posizionati rispetto alla finestra di visualizzazione *"viewport"* del browser. Elementi `position: absolute;` sono posizionati rispettivamente al più vicino blocco padre con posizionamento diverso da `static` (per esempio, `relative`, `absolute`, o `fixed`).


<a name="position-width"></a>
### Posizionamento `position` e larghezza `width`
Non specificare `width: 100%;` ad un elemento che ha `position: [absolute|fixed];`, `left`, e `right`. Utilizzare `width: 100%;` è come applicare congiuntamente `left: 0;` e `right: 0;`. Usa l'uno o l'altro, ma non entrambi.


<a name="position-transforms"></a>
### Posizionamento `fixed` con `transform`
Il *browser* interrompe `position: fixed;` quando al padre di un elemento viene applicata la proprietà `transform`. L'uso di `transform` crea un blocco con un nuovo contenitore, effettivamente costringendo il padre ad avere `position: relative;` in modo tale che l'elemento `fixed` sia reso `position: absolute;`.

[Qui c'è l'esempio](http://jsbin.com/yabek/1/), puoi leggere anche il [post di Eric Meyers su questa questione](http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/).
