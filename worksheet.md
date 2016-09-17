# Push Button Stop Motion

Elektronik und LEGO kombiniert? Einen Turmbau animieren? Mit Figuren eine Szene nachspielen? Oder sonst irgendetwas, das du vorstellen kannst? Wir zeigen dir, wie du mit Python und GPIO Zero deine eigene Stop-Motion-Animation mit einen Push Button Kontroller machen kannst. Also, lass deiner Fantasie freien Lauf! (Du musst natürlich nicht LEGO nehmen, sei einfach kreativ und lass dir was einfallen.)

## Die Kamera verbinden

Bevor du deinen Pi startest, musst du zuerst einmal die Kamera anschließen.

1. Suche auf dem Pi den Kamera Port. Er sollte sich neben dem Ethernet Port befinden. Dann musst du noch die Lasche an der Oberseite anheben.

2. Stecke das Kabel für die Kamera in den Port, dabei sollte die blaue Seite zum Ethernet Port zeigen. Während du das Kabel an seinem Platz hältst, musst die Lasche wieder runterdrücken.

3. Jetzt kannst du deinen Pi starten.

![Verbinde die Kamera](images/connect-camera.jpg)

## Die Kamera testen

1. Nachdem du die Kamera erfolgreich angeschlossen hast, kannst du sie nun testen. Dafür musst du zunächst das Terminalfenster öffnen und anschließend den folgenden Code eingeben:

    ```bash
    raspistill -k
    ```

2. Auf deinem Bildschirm sollte jetzt eine Vorschau erscheinen. Falls das Bild verkehrtherum ist, macht das erstmal nichts weiter. Du kannst das später ändern. Drücke `Strg + C`, um die Vorschau zu beenden.

3. Mit dem Befehl `ls` kannst du alle Dateien in deinem Home-Verzeichnis sehen. Dort findest du die Datei `image1.jpg`.

4. Wenn du auf das Symbol für den Dateimanager in der Taskleiste klickst, siehst du einige Ordner und Dateien. Um dir `image1.jpg` anzusehen, musst du es mit einem Doppelklick öffnen.

## Ein Foto mit Python aufnehmen

1. Öffne **Python 3 (IDLE)** im Hauptmenü:

    ![Öffne Python 3](images/python3-app-menu.png)
    
2. Wähle nun `File > New Window` im Menü aus, um einen Python Datei-Editor zu öffnen. 

3. Im nächsten Schritt gibst du folgenden Code in das Terminalfenster ein:

    ```python
    from picamera import PiCamera
    from time import sleep

    camera = PiCamera()

    camera.start_preview()
    sleep(3)
    camera.capture('/home/pi/Desktop/image.jpg')
    camera.stop_preview()
    ```

4. Wähle im Menü `File > Save` aus (oder drücke `Strg + S`und speichere die Datei als `animation.py`.

5. Wenn du `F5` drückst, startest du das Programm

6. Auf deinem Destop solltest du jetzt die Datei `image.jpg` sehen. Öffne das Bild mit einem Doppelklick.

7. Falls das Bild auf dem Kopf steht, kannst du entweder die Kamera drehen, in dem du eine Halterung dafür verwendest. Oder du lässt es so wie es ist und sagst Python, es soll das Bild um 180 Grad drehen. Um das zu machen, gibst du diesen Code

    ```python
    camera.rotation = 180
    ```

    hinter `camera = PiCamera()` ein, sodass daraus folgendes wird:

    ```python
    from picamera import PiCamera
    from time import sleep

    camera = PiCamera()

    camera.rotation = 180
    camera.start_preview()
    sleep(3)
    camera.capture('/home/pi/Desktop/image.jpg')
    camera.stop_preview()
    ```

8. Starte das Programm erneut und die Datei wird mit einem neuen Bild in der richtigen Ausrichtung überschrieben. Denke daran, dass du die Zeilen auch im nachfolgenden Code beibehältst, wenn du diesen in den näächsten Schritten veränderst.

## Schließe den Push Button an

1. Die Kamera ist angeschlossen, nun geht es an den Push Button. Dazu brauchst du ein Breadboard und mehrer Jumper Kabel. Wie du den Button an deinen Pi anschließt, siehst du in der Darstellung:

    ![](images/picamera-gpio-setup.png)
    
2. Bevor du so richtig loslegen kannst, musst du noch `Button` aus dem  `gpiozero` Modul importieren. Das machst du gleich am Anfang deines Codes. Erstelle `Button` und gib an, dass dieser zu Pin 17 am Pi verbunden ist. Außerdem solltest du `sleep` mit `button.wait_for_press` ersetzen. Das sieht dann so aus:

    ```python
    from picamera import PiCamera
    from time import sleep
    from gpiozero import Button

    button = Button(17)
    camera = PiCamera()

    camera.start_preview()
    button.wait_for_press()
    camera.capture('/home/pi/image3.jpg')
    camera.stop_preview()
    ```

3. Speichere und starte dein Programm

4. Sobald die Vorschau gestartet ist, drückst du den Button, den du an deinen Pi angeschlossen hast, um ein Foto aufzunehmen.

5. Gehe zurück zum Datei-Manager und du solltest eine Datei namens `image3.jpg` sehen. Und wieder, öffne das Foto mit einem Doppelklick, um es dir anzusehen.

## Ein Selfie machen

Wenn du von dir selbst mit der PiCamera ein Foto machen willst, musst du eine Verzögerungszeit hinzufügen, damit du dich selbst in Position bringen kannst. Dafür reicht eine kleine Änderung in deinem Code.

1. Füge deinem Code eine Zeile hinzu, um dem Programm zu sagen, dass es warten soll, bis das Foto aufgenommen wird:

    ```python
    camera.start_preview()
    button.wait_for_press()
    sleep(3)
    camera.capture('/home/pi/Desktop/image3.jpg')
    camera.stop_preview()
    ```

2. Programm speichern und starten.

3. Probiere einfach mal aus, ein Selfie zu machen, indem du Button drückst. Pass aber auf, dass due die Kamera nicht bewegst. Idealerweise hast du die Kamera fest in einer Position montiert.

4. Wie die Male zuvor auch, kannst du dir das Foto jetzt natürlich wieder im Datei-Manager ansehen. Starte das Programm anschließend neu, um ein neues Selfie aufzunehmen.

## Stop-Motion-Animation

Super! Jetzt, da du erfolgreich ein paar einzelne Fotos mit der PiCamera gemacht hast, ist es Zeit für den Versuch, mehrere Standbilder zu einer Stop-Motion-Animation zusammenzufügen.

1. **WICHTIG** Du musst einen neuen Ordner erstellen, umd deine Standbilder zu speichern. Gebe im Terminalfenster `mkdir animation` ein.

2. Verändere deinen Code, um eine Schleife hinzuzufügen, damit jedes Mal, wenn du den Button drückst, ein Foto gemacht wird:

    ```python
    camera.start_preview()
    frame = 1
    while True: 
        try:
            button.wait_for_press()
            camera.capture('/home/pi/animation/frame%03d.jpg' % frame)
            frame += 1
        except KeyboardInterrupt:
            camera.stop_preview()
            break
    ```

    *Da `while True` unendlich lange weiter laufen würde, musst du die Möglichkeit einbauen, das Programm manuell zu beenden. Indem du `try` und `except` nutzt, kann das Programm mit gesonderten Umständen umgehen - wenn du das Programm mit `Strg + C`zum Stoppen zwingst, wird die Kamera Vorschau geschlossen und die Schleife beendet.*

    *`frame%03d` heißt, dass die Datei mit dem Namen "frame" gefolgt von dreistelligen Zahl gespeichert wird. Es startet mit 001, 002, 003, usw. Diese Nummerierung ermöglicht eine übersichtliche Sortierung in der richtigen Reihenfolge für die Animation.*

3. Bevor wir mit der Kamera weiterarbeiten, musst du das Aufbauen, was animiert werden soll (zum Beipiel LEGO). Fertig? Gut, dann kann es losgehen.

4. Drücke den Button, um das erste Foto aufzunehmen. Stelle die Elemente zur Animation um und drücke den Button erneut. Elemente umstellen, Button drücken. Elemen... ach, du weißt schon, was du zu tun hast.

5. Sobald du alle Bilder aufgenommen hast, drückst du `Strg + C`, um das Programm zu beenden.

6. Öffne den Ordner `animation` im Datei-Manager, um alle Standbilder zu sehen.

## Die Animation erstellen

1. Prima! Du hast alle Fotos aufgenommen, jetzt geht es darum, das bewegte Bild zu erstellen. Du beginnst damit, indem du das Terminalfenster öffnest.

2. Führe den Befehl aus, um das Video zu renden (engl. für erzeugen):

    ```bash
    avconv -r 10 -qscale 2 -i animation/frame%03d.jpg animation.mp4
    ```

    *Beachte, dass du wieder `%03d` nutzt - das ist ein übliches Format, das sowohl von Python, als auch von `avconv` verstanden wird. Es bedeutet einfach nur, dass die Fotos in der richtigen Reihenfolge ins Video eingebunden werden.*

3. Du hast die Möglichkeit, die Framerate (also die Geschwindigkeit, mit der ein neues Bild auf dem Bildschirm erscheint) zu ändern, indem du den Befehl zum Renden veränderst. Probiere doch mal, `-r 10` (10 Bilder pro Sekunde) in eine andere Zahl zu ändern.

    ```bash
    omxplayer animation.mp4
    ```
4. Auch kannst du den Dateinamen des erzeugten Videos verändern, damit der vorherige Versuch nicht immer wieder überschrieben wird. Um das zu machen, musst du nur `animation.mp4` zu etwas anderem ändern.

## Was kommt als nächstes?

- Warum teilst du deine Animation nicht mit deinen Freunden? Lade es doch auf Youtube hoch!
- Du weißt jetzt, wie du mit einem Button Fotos mit der PiCamera aufnehmen kannst, wofür kannst du dieses Wissen noch verwenden?
- Kannst du etwas Ähnliches für ein Zeitraffer-Video machen?
- Was könntest du anstatt eines Button verwenden? Einen Bewegungssensor?
- Anstatt ein Video zu erstellen, was kannst du noch mit den aufgenommenen Fotos machen? Du könntest sie beispielsweise auf Twitter oder anderen Social-Media-Sites veröffentlichen!
