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
<p>
oberwähntem Tage mancher sorgliche Gedanke auf,
&amp; wir seufzten öfters zum Heiland, daß Er uns
vor allem Schaden, der uns etwa in der folgen-
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

- Schritt 2: Textermittlung
    + **Übergangsahrscheinlichkeiten** zwischen Vektoren
    + Wahrscheinlichkeitsverteilung über mgl. Zeichen und Zeichentrenner

<center>
<img src="img/nbg_grid.png" width="400px" />
</center>
<center>
<p>↓</p>
</center>
<center>
<p>
oberwähntem Tage mancher sorgliche Gedanke auf,
&amp; wir seufzten öfters zum Heiland, daß Er uns
vor allem Schaden, der uns etwa in der folgen-
den Nacht begegnen könnte, in Gnaden bewahren
</p>
</center>

---

class: part-slide

# Vielen Dank für Ihre Aufmerksamkeit

<center>
<a href="https://wrznr.github.io/oberseminar-germanistik-2021">wrznr.github.io/oberseminar-germanistik-2021</a>
</center>
