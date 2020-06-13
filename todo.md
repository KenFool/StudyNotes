## 1. User Story
### 1.1 Picking
1. User aktiviert die Picking funktion
2. User klickt das Objekt das er auswählen möchte
3. Das geklickte Objekt wird farblich hervorgehoben und angezeigt

### 1.1 Picking - Line on surface for Trace
1. User aktiviert die Funktion
2. User klickt die Punkte auf der Objekt-Oberfläche, die er auswählen möchte
3. User öffnet ausgegebenes File um die Ausgabe zu sehen

### 1.2 Picking - Bohrung
1. User aktiviert die Funktion
2. User klickt 3 Punkte auf Oberfläche
3. User kann minimal den Kreis neuverschieben und anpassen (Radius, X,Y,Z-Ebene)
4. User bestimmt zunächst manuell über Gui-Element die Tiefe für den Zylinder (Benötige ich einen SceneGraph um ein Zylinder mit KegelStumpf zu rendern und zu verschieben?)
5. User bestätigt die Tiefe für die Bohrung
6. Ausgewählte Geometrie wird als User-benanntes Feature in die Feature-Liste des Bauteils eingetragen (Bohrung)
7. User erhält visuelles Feedback in der GUI über das Volumen des Features und geeigneter Kenngrößen (Bohrung: Durchmesser, Tiefe, usw.)

---

## 2. Programm-Ablauf
### 2.1 Picking
1. User wählt Funktion Picking aus
2. Picking-Objekt wird initialisiert
   1. Picking erhält:
      1. Klick-koordinaten (2D-Point)
      2. MVP-Matrizen
      3. Transformationen
   2. Das Picking-Objekt berechnet die Position des Klicks in den 3D-Raum (gluUnProject, glReadPixels)

```cpp
// get OpenGLPosition form Clickposition and return 3D-Coordinates in clip-space
CVector3 GetOGLPos(int x, int y)
{
    GLint viewport[4];              // [x,y,width,height]
    GLdouble modelview[16];         // model-view matrix -> primitive-vertices transformed to eye-coordinates
    GLdouble projection[16];        // projection        -> from eye to clip-coordinates
    GLfloat winX, winY, winZ;
    GLdouble posX, posY, posZ;

    glGetDoublev( GL_MODELVIEW_MATRIX, modelview );
    glGetDoublev( GL_PROJECTION_MATRIX, projection );
    glGetIntegerv( GL_VIEWPORT, viewport );

    winX = (float)x;
    winY = (float)viewport[3] - (float)y;
    glReadPixels( x, int(winY), 1, 1, GL_DEPTH_COMPONENT, GL_FLOAT, &winZ );

    gluUnProject( winX, winY, winZ, modelview, projection, viewport, &posX, &posY, &posZ);

    return CVector3(posX, posY, posZ);
}
```
   3. Die Pickpostion (3D-Point) wird mit den