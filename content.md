layout: true
  
<div class="my-header"></div>

<div class="my-footer">
  <table>
    <tr>
      <td style="text-align:right">Sächsische Landesbibliothek – Staats- und Universitätsbibliothek</td>
      <td>4. Mai 2021</td>
      <td style="text-align:right"><a href="https://www.slub-dresden.de/">www.slub-dresden.de</a></td>
    </tr>
    <tr>
      <td style="text-align:right">Referate 4.3 &amp; 2.5</td>
      <td />
    </tr>
  </table>
</div>

<div class="my-title-footer">
  <table>
    <tr>
      <td style="text-align:left"><b>Kay-Michael Würzner</b></td>
      <td style="text-align:left"><b>Robert Sachunsky</b></td>
    </tr>
    <tr>
      <td style="text-align:left">Referat Open Science</td>
      <td style="text-align:left">Referat Digitale Objekte</td>
    </tr>
    <tr/>
    <tr/>
    <tr/>
    <tr/>
    <tr>
      <td style="font-size:8pt"><b>4. Mai 2021</b></td>
    </tr>
    <tr>
      <td style="font-size:8pt">Oberseminar Germanistik, TU Dresden</td>
    </tr>
  </table>
</div>

---

class: title-slide
count: false

# Grundlagen der automatischen Vervolltextung handschriftlicher Materialien 
## am Beispiel der Nachrichten aus der Brüdergemeine

---

# Überblick

- Handschriftenerkennung: Wie funktioniert's?
- Training und *Ground Truth*
- Struktur- und Texterfassung mit Larex

---

class: part-slide
count: false

# Handschriftenerkennung: Wie funktioniert's?

---

# Wie funktioniert's?

- Ziel: Transformation von Bilddaten in maschinenlesbaren Volltext
    + schrittweise Verarbeitung

<center>
<img src="img/nbg_region.png" width="400px" />
</center>
<center>
<p>↓</p>
</center>
<center>
<p style="display: inline-block; text-align: left;">
oberwähntem Tage mancher sorgliche Gedanke auf,<br/>
&amp; wir seufzten öfters zum Heiland, daß Er uns<br/>
vor allem Schaden, der uns etwa in der folgen-<br/>
den Nacht begegnen könnte, in Gnaden bewahren
</p>
</center>

---

# Wie funktioniert's?

- Schritt 1: Zeilenerkennung
    + **regelbasierte** (Bildmorphologie) oder
    + **datengetriebene** Verfahren (e.g. Pixelklassifikation)

<center>
<img src="img/nbg_region.png" width="400px" />
</center>
<center>
<p>↓</p>
</center>
<center>
<img src="img/nbg_lines.png" width="400px" />
</center>

---

# Wie funktioniert's?

- Schritt 2: Vektorisierung
    + **Skalierung** auf einheitliche Höhe
    + **Unterteilung** in 1pixel-breite Streifen

<center>
<img src="img/nbg_lines.png" width="400px" />
</center>
<center>
<p>↓</p>
</center>
<center>
<img src="img/nbg_grid.png" width="400px" />
</center>

---

# Wie funktioniert's?

- Schritt 3: Textermittlung
    + **Übergangswahrscheinlichkeiten** zwischen Vektoren
    + Rückgriff auf (offline) trainiertes **Modell**

<center>
<img src="img/nbg_grid.png" width="400px" />
</center>
<center>
<p>↓</p>
</center>
<center>
<p style="display: inline-block; text-align: left; font-size: small; font-style: italic;">
oberwähntem Tage mancher sorgliche Gedanke auf,<br/>
&amp; wir seufzten öfters zum Heiland, daß Er uns<br/>
vor allem Schaden, der uns etwa in der folgen-<br/>
den Nacht begegnen könnte, in Gnaden bewahren
</p>
</center>

---

# Wie funktioniert's?

.cols[
.fourty[
- Schritt 3: Zeichenerkennung (Prinzip)
]
.sixty[
<p style="margin-top:-20px">
<img src="https://files.gitter.im/5b97ae51d73408ce4fa7b1ee/LYSx/lstm-fraktur-arrows.png" width="550px" style="transform:rotate(90deg);"/>
</p>
]
]

---

# Wie funktioniert's?

- Software
    + [**Tesseract**](https://github.com/tesseract-ocr/tesseract): komplettes Open-Source-Paket
        * regelbasierte Bildvorverarbeitung und Layouterkennung
        * datengetriebene Texterkennung (unterstützt > 100 Sprachen)
        * Ease-of-Use-Training eigener Modelle
        * für OCR und **HTR** verwendbar
    + [**OCRopy**](https://github.com/ocropus/ocropy): umfangreiches Open-Source-Paket
        * regelbasierte Bildvorverarbeitung und Layouterkennung
        * datengetriebene Texterkennung (nur sehr wenige Modelle vorhanden)
        * für OCR und **HTR** verwendbar
        * prominente Ableger: [**kraken**](https://kraken.re) und [**Calamari**](https://github.com/Calamari-OCR/calamari)
    + [**OCR-D**](https://ocr-d.de/): Workflow-Engine
        * Orchestrierung verschiedener Open-Source-Pakete zu stabilen Workflows
        * gleichzeitig DFG-Förderprogramm zur Verbesserung von OCR für historische Drucke
        * SLUB als maßgebliche Entwicklungseinrichtung

---

class: part-slide
count: false

# Training und *Ground Truth*

---

# Training und *Ground Truth*

- Basis der Erkennung: **trainierte** Modelle
    + anhand manuell erstellter, fehlerfreier Trainingsdaten
- für Text:
    + Paare von Zeilenbildern und deren Text
- für Struktur (Regionen und Zeilen):
    + Seitenbilder und Koordinaten der Struktureinheiten
- Algorithmus lernt Zuordnung
    + Wahrscheinlichkeitsverteilung über mgl. Zeichen und Zeichentrenner
- bei ausreichender Menge und **Repräsentativität** der Daten → verbesserte Erkennung

---

# Training und *Ground Truth*: Beispiel

- [Matthäi-Edition](https://digital.slub-dresden.de/werkansicht/dlf/429482/) des [Codex Boernerianus](https://digital.slub-dresden.de/werkansicht/dlf/2966/1/)

<center>
<img src="img/boernerianus_region.png" width="400px" />
</center>

- Texterkennung mit Tesseract:

<center>
<p style="display: inline-block; text-align: left; font-size:14pt">
legis cuftodiat nonne<br/>
vouov. ζυλασση Ovi<br/>
circumcifionem repu<br/>
περιτομὴν Λογεισ<br/>
legem confummans te<br/>
νομον πτελουσα σε
</p>
</center>

---

# Training und *Ground Truth*: Beispiel

- Ergebnis auf den ersten Blick nicht schlecht, jedoch

<center>
<div style="display: inline-block; text-align: left; font-size:16pt">
<table><colgroup><col /><col /></colgroup>
<tbody>
<tr>
<th colspan="1">Ground Truth</th>
<th colspan="1">OCR Latin+Greek</th></tr>
<tr>
<td>legis cu<span class="cdiff8 diff" style="color: rgb(0,128,0);" title="">ſ</span>todiat nonne</td>
<td>legis cu<span class="cdiff8 diff" style="color: rgb(255,0,0);" title="">f</span>todiat nonne</td></tr>
<tr>
<td><span style="color: rgb(0,128,0);"><span class="cdiff25 diff" title="">&nu;</span><span class="cdiff26 diff" title="">&omicron;</span><span class="cdiff27 diff" title="">&mu;</span><span class="cdiff28 diff" title="">&omicron;</span><span class="cdiff29 diff" title="">&upsilon;</span></span>. <span class="cdiff32 diff" style="color: rgb(0,128,0);" title="">&phi;</span>&upsilon;&lambda;&alpha;&sigma;&sigma;&eta; <span style="color: rgb(0,128,0);"><span class="cdiff40 diff" title="">&Omicron;</span><span class="cdiff41 diff" title="">&upsilon;</span><span class="cdiff42 diff" title="">&chi;</span><span class="cdiff43 diff" title="">&epsilon;</span><span class="cdiff44 diff" title="">&iota;</span></span></td>
<td><span style="color: rgb(255,0,0);"><span class="cdiff25 diff" title="">v</span><span class="cdiff26 diff" title="">o</span><span class="cdiff27 diff" title="">u</span><span class="cdiff28 diff" title="">o</span><span class="cdiff29 diff" title="">v</span></span>. <span class="cdiff32 diff" style="color: rgb(255,0,0);" title="">&zeta;</span>&upsilon;&lambda;&alpha;&sigma;&sigma;&eta; <span style="color: rgb(255,0,0);"><span class="cdiff40 diff" title="">O</span><span class="cdiff41 diff" title="">v</span><span class="cdiff42 diff" title="">i</span><span class="cdiff43 diff ellipsis">&middot;</span><span class="cdiff44 diff ellipsis">&middot;</span></span></td></tr>
<tr>
<td>circumci<span class="cdiff57 diff" style="color: rgb(0,128,0);" title="">ſ</span>ionem repu</td>
<td>circumci<span class="cdiff57 diff" style="color: rgb(255,0,0);" title="">f</span>ionem repu</td></tr>
<tr>
<td>&pi;&epsilon;&rho;&iota;&tau;&omicron;&mu;<span class="cdiff79 diff" style="color: rgb(0,128,0);" title="">&eta;</span>&nu; &Lambda;&omicron;&gamma;&epsilon;&iota;&sigma;</td>
<td>&pi;&epsilon;&rho;&iota;&tau;&omicron;&mu;<span class="cdiff79 diff" style="color: rgb(255,0,0);" title="">ὴ</span>&nu; &Lambda;&omicron;&gamma;&epsilon;&iota;&sigma;</td></tr>
<tr>
<td>legem con<span class="cdiff101 diff" style="color: rgb(0,128,0);" title="">ſ</span>ummans te</td>
<td>legem con<span class="cdiff101 diff" style="color: rgb(255,0,0);" title="">f</span>ummans te</td></tr>
<tr>
<td>&nu;&omicron;&mu;&omicron;&nu; <span class="cdiff121 diff ellipsis" style="color: rgb(0,128,0);">&middot;</span><span style="color: rgb(0,0,0);">&tau;</span>&epsilon;&lambda;&omicron;&upsilon;&sigma;&alpha; &sigma;&epsilon;</td>
<td>&nu;&omicron;&mu;&omicron;&nu; <span class="cdiff121 diff" style="color: rgb(255,0,0);" title="">&pi;</span>&tau;&epsilon;&lambda;&omicron;&upsilon;&sigma;&alpha; &sigma;&epsilon;</td></tr></tbody></table></div>
</center>

- Anteil fehlerhaft erkannter Zeichen bei 12 %
    + Mischung Latein und Altgriechisch
    + langes s in Antiqua

---

# Training und *Ground Truth*: Beispiel

- nach Transkription von 973 Zeilen durch Juan Garcés und
- 30 000 Trainingsschritten

<center>
<div style="display: inline-block; text-align: left; font-size:16pt">
<table><colgroup><col /><col /></colgroup>
<tbody>
<tr>
<th colspan="1">Ground Truth</th>
<th colspan="1">OCR GT-Finetuning</th></tr>
<tr>
<td>legis cu<span class="cdiff8 diff" style="color: rgb(0,0,0);" title="">ſ</span>todiat nonne</td>
<td>legis cu<span class="cdiff8 diff" style="color: rgb(0,0,0);" title="">ſ</span>todiat nonne</td></tr>
<tr>
<td><span style="color: rgb(0,0,0);"><span class="cdiff25 diff" title="">&nu;</span><span class="cdiff26 diff" title="">&omicron;</span><span class="cdiff27 diff" title="">&mu;</span><span class="cdiff28 diff" title="">&omicron;</span><span class="cdiff29 diff" title="">&upsilon;</span>. <span class="cdiff32 diff" title="">&phi;</span>&upsilon;&lambda;&alpha;&sigma;&sigma;&eta; <span class="cdiff40 diff" title="">&Omicron;</span><span class="cdiff41 diff" title="">&upsilon;</span><span class="cdiff42 diff" title="">&chi;</span><span class="cdiff43 diff" title="">&epsilon;</span><span class="cdiff44 diff" title="">&iota;</span></span></td>
<td><span style="color: rgb(0,0,0);"><span class="cdiff25 diff" title="">&nu;</span><span class="cdiff26 diff" title="">&omicron;</span><span class="cdiff27 diff" title="">&mu;</span><span class="cdiff28 diff" title="">&omicron;</span><span class="cdiff29 diff" title="">&upsilon;</span>. <span class="cdiff32 diff" title="">&phi;</span>&upsilon;&lambda;&alpha;&sigma;&sigma;&eta; <span class="cdiff40 diff" title="">&Omicron;</span><span class="cdiff41 diff" title="">&upsilon;</span><span class="cdiff42 diff" title="">&chi;</span><span class="cdiff43 diff" title="">&epsilon;</span><span class="cdiff44 diff" title="">&iota;</span></span></td></tr>
<tr>
<td><span style="color: rgb(0,0,0);">circumci<span class="cdiff57 diff" title="">ſ</span>ionem repu</span></td>
<td><span style="color: rgb(0,0,0);">circumci<span class="cdiff57 diff" title="">ſ</span>ionem repu</span></td></tr>
<tr>
<td><span style="color: rgb(0,0,0);">&pi;&epsilon;&rho;&iota;&tau;&omicron;&mu;<span class="cdiff79 diff" title="">&eta;</span>&nu; <span style="color: rgb(0,128,0);">&Lambda;</span>&omicron;&gamma;&epsilon;&iota;&sigma;</span></td>
<td><span style="color: rgb(0,0,0);">&pi;&epsilon;&rho;&iota;&tau;&omicron;&mu;<span class="cdiff79 diff" title="">&eta;</span>&nu; <span class="cdiff82 diff" style="color: rgb(255,0,0);" title="">&Alpha;</span>&omicron;&gamma;&epsilon;&iota;&sigma;</span></td></tr>
<tr>
<td><span style="color: rgb(0,0,0);">legem con<span class="cdiff101 diff" title="">ſ</span>ummans te</span></td>
<td><span style="color: rgb(0,0,0);">legem con<span class="cdiff101 diff" title="">ſ</span>ummans te</span></td></tr>
<tr>
<td>&nu;&omicron;&mu;&omicron;&nu; <span style="color: rgb(0,0,0);">&tau;</span>&epsilon;&lambda;&omicron;&upsilon;&sigma;&alpha; <span style="color: rgb(0,128,0);">&sigma;</span>&epsilon;</td>
<td>&nu;&omicron;&mu;&omicron;&nu; <span style="color: rgb(0,0,0);">&tau;</span>&epsilon;&lambda;&omicron;&upsilon;&sigma;&alpha; <span style="color: rgb(255,0,0);">&omicron;</span>&epsilon;</td></tr></tbody></table>
</div></center>

- Reduktion der Fehler auf zwei Zeichen
    + saubere Trennung zwischen Latein und Altgriechisch
    + langes s in Antiqua

---

# Training und *Ground Truth*: Optimierungen

- Ergebnis des Experiments: **hochspezifisches** Modell
    + Aufwand im Allgemeinen nicht leistbar
- Optionen
    + **Transfer-Learning**
        * Anpassung eines existierenden, ähnlichen Modells mit wenigen Zeilen GT auf spezifische Vorlage
    + **generische Modelle**
        * auf Basis großer Mengen diversen GTs trainierte Modelle mit Allgemeinheitsanspruch 
        * schwierig im Bereich HTR
    + **synthetisches Training**
        * mit Hilfe großer Textmengen und verschiedener Computerschriftarten automatisch erzeugter GT
        * Erzeugung einer realistischeren Optik durch
    + **Augmentierung**
        * größere Modellrobustheit und Variantenstabilität durch gezieltes Verrauschen
        * Schrägstellung, Spiegelung, Störpixel, horizontale und vertikale Verzerrung etc.

---

# Training und *Ground Truth*: HTR

<center>
<img src="https://camo.githubusercontent.com/06493331adfcac6c297a8cd048fcb77742088085c31cf7c5046c4c17c06d4bbc/68747470733a2f2f66696c65732e6769747465722e696d2f77727a6e722f744153492f4f43522d442d494d472d4445535045434b5f303030355f72315f72316c32362e706e67" width="600px"/>
</center>

- z.B. [Konzilsprotokolle Universitätsarchiv Greifswald](https://zenodo.org/record/215383#.YJFuPHVfjDs)
    + 8 770 Zeilen GT
    + einzeln segmentiert
- Training und Test mit Tesseract

```
$ make training MODEL_NAME=htr PSM=13 MAX_ITERATIONS=30000
$ tesseract tline.png - -l htr --psm 13 --dpi 300 2>/dev/null
sowohl, als auch eb bei der Schrift etwas zu er-
```

---

class: part-slide

# Struktur- und Texterfassung mit Larex

---

# Struktur- und Texterfassung mit Larex

- Webapplikation für bequeme/schnelle Transkription
    + Struktur- und Textebene
- bereits am Institut für Germanistik im Einsatz

![](https://github.com/OCR4all/LAREX/raw/master/documentation/larex.gif)

---

class: part-slide

# Diskussion und lose Enden




---

class: part-slide

# Vielen Dank für Ihre Aufmerksamkeit

<center>
<a href="https://wrznr.github.io/oberseminar-germanistik-2021">wrznr.github.io/oberseminar-germanistik-2021</a>
</center>
