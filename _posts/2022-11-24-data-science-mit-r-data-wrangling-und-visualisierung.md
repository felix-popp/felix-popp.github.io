---
layout: post
title: Data Science mit R - Data Wrangling und Visualisierung
author: Felix
comments: false
tags: []
excerpt_separator: "<!--more-->"
sticky: false
hidden: true
featureImage: ''

---
METABRIC
================

# Zusätzliche Funktionalität laden

Fast jede Software hat Erweiterungen irgendeiner Art. Die Bezeichnungen
variieren von extensions über plug-ins bis zur library. Unterschiedliche
Terminologie für dasselbe Prinzip: Mehr Features durch Erweiterungen,
die andere Leute bereitstellen. <!--more--> R hat sowas natürlich auch, als populäre
open-source Software. Hier heißen die Erweiterungen packages und
bestehen aus Funktionen, die andere Leute für bestimmte Anwendungsfälle
geschrieben haben. Durch ein zentrales Verteilungssystem werden die
Erweiterungen verfügbar gemacht, sodass wir alle sie benutzen können.
Das gute an R ist, egal was du dir ausdenkst oder brauchst, irgendwer
hat es schon gemacht und stellt es frei zur Verfügung. You name it.

Die Erweiterungen musst duzuerst mit dem Befehl
`install.packages("tidyverse")` installieren.

Mit dem Befehl `library` lädst du für jedes Projekt genau die
Funktionalität, die du brauchst.

``` r
library(tidyverse)
library(lubridate)
library(ggplot2)
library(ggpubr)
library(readxl)
library(khroma)
library(gtsummary)
library(survival)
library(survminer)
```

Am wichtigsten ist die Erweiterung ´tidyverse´. Das Tidyverse ist eine
Sammlung von Erweiterungen für die Statistiksoftware R, die von Hadley
Wickham und seinem Team eingeführt wurden und „eine zugrunde liegende
Designphilosophie, Grammatik und Datenstrukturen“ teilen.

# Data Wrangling

Data Wrangling, auch Datenaufbereitung genannt, bezeichnet den Prozess
der Strukturierung, Bereinigung und Transformation der Rohdaten in ein
Format, das sich inhaltlich auswerten und zur Modellierung sowie
Visualisierung einsetzen lässt. Die Datenaufbereitung ist essenziell und
erfordert ca. 50-80% der Zeit für eine Datenanalyse. In der Regel liegen
die Rohdaten in einer Excel-Tabelle oder als CSV-Datei vor (steht für
‚Comma-separated values‘ und beschreibt den Aufbau einer Textdatei zur
Speicherung einfach strukturierter Daten), die in einen R-Data Frame
(tibble) eingelesen werden. Für den Kurs wird der öffentlich zugängliche
Datensatz des Breast Cancer International Consortiums (METABRIC)
verwendet. Diese Datenbank eines kanadisch-britischen Projekts enthält
klinische und Sequenzierungsdaten von 1904 Patientinnen mit Brustkrebs.
Der Datensatz wurde von Professor Carlos Caldas vom Cambridge Research
Institute und Professor Sam Aparicio vom British Columbia Cancer Centre
in Kanada gesammelt und in Nature Communications veröffentlicht. Danach
werden die Daten bereinigt, so dass jede Spalte einer Variable und jede
Zeile einer Beobachtung entspricht. Der METABRIC Datensatz ist bereits
bereinigt und kann einfach eingelesen werden:

``` r
metabric <- read_csv("../METABRIC_RNA_Mutation.csv")
```

Sobald die Daten bereinigt sind, ist ein üblicher erster Schritt, sie zu
transformieren. Die Transformation umfasst die Eingrenzung von
Beobachtungen, die von Interesse sind (z.B. alle Patientinnen, die eine
Mastektomie erhalten haben), die Erstellung neuer Variablen, die
Funktionen vorhandener Variablen sind (z.B. die Berechnung des Alters
bei Therapiebeginn aus dem Geburtsdatum und dem OP-Datum), und die
Berechnung einer Reihe von zusammenfassenden Statistiken (z.B. Zählungen
oder Mittelwerte). Gerade Zählungen geben wichtige Rückschlüsse auf die
Datenqualität. Wenn alle Patientinnen beispielsweise sehr alt sind, ist
der Datensatz bezüglich des Alters unbalanciert. Auswertungen für Junge
Patientinnen können dann nicht durchgeführt werden. Das Bereinigen und
Transformieren der Daten wird als „Wrangling“ bezeichnet, weil es sich
oft wie ein Kampf anfühlt, die Daten in eine Form zu bringen, mit der
man gut arbeiten kann.

# Visualisierung

Besser noch als Zählen ist eine Visualisierung. Ein Bild sagt mehr als
1000 Worte. Unter Datenvisualisierung versteht man die Übertragung von
Informationen in einen visuellen Kontext, z.B. eine Karte oder ein
Diagramm, um Daten für das menschliche Gehirn leichter verständlich zu
machen und Erkenntnisse daraus zu ziehen. Das Hauptziel der
Datenvisualisierung besteht darin, Muster, Trends und Ausreißer in
Datensätzen zu erkennen. Eine gute Visualisierung kann unerwartete
Aspekte zeigen und neue Fragen zu den Daten aufwerfen. Eine gute
Visualisierung kann auch darauf hinweisen, dass die falsche Frage
gestellt wurde, oder dass andere Daten erhoben werden müssen.  
Das Statisitk-System R verfügt über mehrere Systeme zur Erstellung von
Diagrammen, aber ggplot2 ist eines der elegantesten und vielseitigsten.
ggplot2 implementiert die „Grammatik der Grafik“, ein kohärentes System
zur Beschreibung und Erstellung von Diagrammen. Die Funktionen im
ggplot2-Paket bauen ein Diagramm in Ebenen auf. Ein komplexes Diagramm
entsteht, indem mit einem einfachen Diagramm begonnen wird und nach und
nach zusätzliche Elemente (Ebenen) hinzugefügt werden. Um ein Diagramm
zu erstellen, teilen wir `ggplot` mit, dass unsere Daten `metabric`
sind, und geben an, dass die x-Achse die Variable `age_at_diagnosis`
darstellt. Dann weisen wir `ggplot` an, dies als Dichteplot
darzustellen, indem wir die Option `geom_density()` hinzufügen.

``` r
p <- ggplot(metabric, aes(x=age_at_diagnosis)) + geom_density()
p
```

![](../assets/Data_Science_mit_R_METABRIC_files/figure-gfm/unnamed-chunk-6-1.png)

Man sieht eine annähernde Normalverteilung des Patientenalters, und das
mit nur einer Zeile!

Geometrische Objekte oder `geoms` sind die eigentlichen Markierungen,
die wir auf einer Fläche anbringen. Beispiele hierfür sind:

Punkte (`geom_point`, für Streudiagramme, Punktdiagramme, usw.) Linien
(`geom_line`, für Zeitreihen, Trendlinien, usw.) Boxplot
(`geom_boxplot`, für, nun ja, Boxplots!)

Ein Diagramm sollte mindestens ein `geom` haben, aber es gibt keine
Obergrenze. Mit dem Operator + kannst Du ein `geom` zu einem Plot
hinzufügen. In `ggplot2` bedeutet ästhetisch “etwas, das man sehen
kann”. Jede Ästhetik `aes` ist eine Zuordnung zwischen einem visuellen
Merkmal und einer Variablen. Beispiele sind:

Position (d. h. auf der x- und y-Achse) Farbe (“äußere” Farbe) Füllung
(“innere” Farbe) Form (der Punkte) Linientyp Größe

Nun können wir einfach eine Ästhetik zu unserer Visualisierung
hinzufügen. Farblich können wir die chirurgische Therapie darstellen.

``` r
p <- ggplot(metabric, aes(x=age_at_diagnosis, color=type_of_breast_surgery)) + geom_density()
p
```

![](../assets/Data_Science_mit_R_METABRIC_files/figure-gfm/unnamed-chunk-8-1.png)

Tatsächlich ist der Datensatz bezüglich der chirurgischen Therapie
unvollständig. Bei einigen Patientinnen ist die Therapie nicht angegeben
oder sie haben vielleicht keine erhalten. Wir wissen das nicht. Wir
können aber zählen wie vollständig der Datensatz ist:

``` r
metabric %>% count(type_of_breast_surgery)
```

    # A tibble: 3 x 2
      type_of_breast_surgery     n
      <chr>                  <int>
    1 BREAST CONSERVING        755
    2 MASTECTOMY              1127
    3 <NA>                      22

Für nur 22 Fälle ist die chirurgische Therapie nicht bekannt, das ist
wirklich gut. In der Grafik hätten wir das nicht so leicht ablesen
können. Zählen lohnt sich als immer mal wieder.

Hier ist `%>%` der sogenannte “Pipe”-Operator. Dieser Operator leitet
einen Wert oder das Ergebnis eines Ausdrucks an den nächsten
Funktionsaufruf/Ausdruck weiter. Eine Funktion zum Filtern von Daten
kann zum Beispiel wie folgt geschrieben werden:

`filter(daten, variable == 42)`

oder

`daten %>% filter(variable == 42)`

Beide Funktionen erfüllen die gleiche Aufgabe, und der Vorteil der
Verwendung von `%>%` ist vielleicht nicht sofort ersichtlich; wenn Du
jedoch mehrere Funktionen ausführen möchtest, wird der Vorteil
offensichtlich.

Für eine Dissertation benötigen wir noch schöne Tabellen aus der
fantastischen library `gtsummary`.

``` r
tbl_summary(metabric %>% select(type_of_breast_surgery))
```

<div>
<div id="hmevqpvait" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#hmevqpvait .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#hmevqpvait .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#hmevqpvait .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#hmevqpvait .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#hmevqpvait .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#hmevqpvait .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#hmevqpvait .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#hmevqpvait .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#hmevqpvait .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#hmevqpvait .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#hmevqpvait .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#hmevqpvait .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#hmevqpvait .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#hmevqpvait .gt_from_md > :first-child {
  margin-top: 0;
}

#hmevqpvait .gt_from_md > :last-child {
  margin-bottom: 0;
}

#hmevqpvait .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#hmevqpvait .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#hmevqpvait .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#hmevqpvait .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#hmevqpvait .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#hmevqpvait .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#hmevqpvait .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#hmevqpvait .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#hmevqpvait .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#hmevqpvait .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#hmevqpvait .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#hmevqpvait .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#hmevqpvait .gt_left {
  text-align: left;
}

#hmevqpvait .gt_center {
  text-align: center;
}

#hmevqpvait .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#hmevqpvait .gt_font_normal {
  font-weight: normal;
}

#hmevqpvait .gt_font_bold {
  font-weight: bold;
}

#hmevqpvait .gt_font_italic {
  font-style: italic;
}

#hmevqpvait .gt_super {
  font-size: 65%;
}

#hmevqpvait .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1"><strong>Characteristic</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1"><strong>N = 1,904</strong><sup class="gt_footnote_marks">1</sup></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">type_of_breast_surgery</td>
<td class="gt_row gt_center"></td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">BREAST CONSERVING</td>
<td class="gt_row gt_center">755 (40%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">MASTECTOMY</td>
<td class="gt_row gt_center">1,127 (60%)</td></tr>
    <tr><td class="gt_row gt_left" style="text-align: left; text-indent: 10px;">Unknown</td>
<td class="gt_row gt_center">22</td></tr>
  </tbody>
  
  <tfoot>
    <tr class="gt_footnotes">
      <td colspan="2">
        <p class="gt_footnote">
          <sup class="gt_footnote_marks">
            <em>1</em>
          </sup>
           
          n (%)
          <br />
        </p>
      </td>
    </tr>
  </tfoot>
</table>
</div>
</div>


Jetzt entfernen wir die ungültigen Werte

``` r
 df <- metabric %>% drop_na(type_of_breast_surgery)
 df %>% count(type_of_breast_surgery)
```

    # A tibble: 2 x 2
      type_of_breast_surgery     n
      <chr>                  <int>
    1 BREAST CONSERVING        755
    2 MASTECTOMY              1127

und visualisieren den Datensatz erneut:

``` r
p <- ggplot(df, aes(x=age_at_diagnosis, color=type_of_breast_surgery)) + geom_density()
p
```

![](../assets/Data_Science_mit_R_METABRIC_files/figure-gfm/unnamed-chunk-16-1.png)

Patientinnen, die eine Matektomie erhalten haben, sind offensichtlich
älter.