# Tracking von Spielern


## 0_Benötigte Bibliotheken
Es wird ausschließlich Python verwendet. 
+ tensorflow-gpu==2.3.0rc0
+ opencv-python==4.5.2.54
+ numpy
+ lxml
+ tqdm
+ absl-py
+ matplotlib
+ easydict
+ pillow

## 1_Installation

Die Objekt Erkennung und Verfolgung erfolgt nach einer Vorlage von theAIGuysCode [GitHub](https://github.com/theAIGuysCode/yolov4-deepsort) 

```bash
# TensorFlow CPU
pip install -r requirements.txt

# TensorFlow GPU
pip install -r requirements-gpu.txt

```

## 2_Weights herunterladen
_Die vortrainierten Daten für TensorFlow [hier](https://drive.google.com/open?id=1cewMfusmPjYWbrnuJRuKhPMwRe_b9PaT) heruntergeladen werden. 
_Die '.weights' ins Verzeichtnis /data/ legen. 
_Anschließend die '.weights' in ein für TensorFlow lesbares Format umwandeln. 



```bash
# Convert darknet weights to tensorflow model
python save_model.py --model yolov4 

```

## 3_Leeres positions.json erstellen
_Vor dem ausführen erst eine leere JSON Datei im Verzeichtnis erstellen und mit folgendem Inhalt fülle:
```json
{"arr":[]}

```
## 4_Das Python Script ausführen
Anschließend Speichern nicht vergessen.
Das Python Script aus dem Terminal heraus ausführen.
+ --video = der Link des zu trackenden Videos
+ --output = der Link der Vorschau mit erkannten SpielerInnen
+ --json = das Json File aus Punkt 3.  in dem die Positionsdaten gespeichert werden 

![](outputs\soccer_ce.gif)

```bash
python3 object_tracker.py --video ./data/video/ce_topview.mov --output ./outputs/soccer_tracking.avi --model yolov4 --dont_show

```
## 5_Top View vorbereiten 
Die folgenden Schritte machen oder die Links für die Dateipfade so schreiben dass sie ins obere Verzeichtnis zeigen z.B. ( ..\positions.json)
+ Die JSON-Datei in das Verzeichtnis \drawTrackingPaths\ kopieren
+ Das getrackte Video ebenfalls in \drawTrackingPaths\ kopieren
+ Die Links zu den Dateien in der drawTrackingPaths\drawTrackingPaths.py Datei einfügen.

```python
#HIER DIE LINKS EINFÜGEN
json_file = '../positions_ce.json'
field_path = 'field.png'
video_path ='soccer_ce.avi'
output_path = './output4.avi'

```
## 6_Top View berechnen lassen
Die Python Datei aus dem Terminal heraus ausführen.
```bash
python3 drawTrackingPaths.py

```
![](drawTrackingPaths\output.gif)

## FAQ
+ Was muss ich beachten wenn ich eigene Videos tracken möchte? Dann müssen folgende Punkte geändert oder angepasst werden:
```python
#Die Homographie Punkte in Pixeln. Dazu markante Punkte in den Videos suchen die die gleichen Positionen haben müssen. zB. Eckfahnen. 
Die jeweils zugehörigen Punkte müssen dabei an der gleichen Stelle im Array aufgeführt werden. 
#Die Reihenfolge ist Links Unten -> Links Oben -> Rechts Oben -> Rechts Unten
#Die Punkte des auf dem 2D Feld
pts_dst = np.float32([[240,1055],[240,17],[1680,17],[1680,1055]])
#Die Punkte aus dem Video 
pts_src = np.float32([[-400,1220],[726,338],[2606,326],[3950,1220]])

```
+ Ich kann die Videos mit den Ergebnissen nicht ansehen?! - Unter MacOS lassen sich .avi Dateien nicht so einfach abspielen

+ Fehlermeldung: [ERROR:0] global /io/opencv/modules/videoio/src/cap.cpp (392) open VIDEOIO(CV_IMAGES): raised OpenCV exception - Der Pfad der Video Quelle stimmt wohl nicht.


## _Verbesserungs Möglichkeiten
+ Selbsttrainierte Bilder von Fußballspielern ergänzen und der .weights Datei hinzufügen.
+ Eine zweite Kamera von der Gegenüberliegenden Seite mit einbeziehen um das Problem zu umgehen dass YOLOv4 und DeepSORT Schwierigkeiten mit kleinen Objekten haben und die dritte Dimension der Höhe besser erkennen zu können (zB. beim Ball)
+ Eine höhere Auflösung bei den Videos verwenden um dann pro Frame in vier Abschnitten durch das Bild zu scannen. 



## Quellen
[theAIGuys](https://github.com/theAIGuysCode/yolov4-deepsort#readme)
