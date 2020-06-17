#CppConference 2019 - Test-Driven Development [https://www.youtube.com/watch?v=RoYljVOj2H8]
---
## Test-Driven Development-Prozess
- Sehr kurze Entwicklungsphasen
  - 1. Schreibe Tests für die neuen Features
  - 2. Lass die Tests laufen und erwarte, dass sie scheitern
  - 3. Schreibe neuen Code der die Tests besteht
  - 4. Lass die Tests erneut laufen und bestätige, dass sie erfolgreich sind (sonst 3.)
  - 5. Refaktorisiere und Iteriere den geschriebenen Code
  - 6. Lass die Tests erneut laufen und bestätige, dass sie erfolgreich sind (-> 1.)
    - dass das Refaktorisieren die Code-Funktionalität beibehalten hat (sonst 5.)
---
## Wann soll man Tests schreiben
1. Bevor Code geschrieben wurde
   1. Code wird nicht compilen  __[-]__
   2. Tests beschreiben, wie der Code aussehen soll __[+]__
   3. Beschreiben die Erwartungen(Requirements) an die(das) Schnittstelle(Interface) in C++ __[+]__
2. Nachdem die Schnittstellen(Interfaces) deklariert wurden
   1. Code wird kompilieren aber nicht linken. __[+|-]__
3. Nach dem Schreiben einer Dummy Implementierung
   1. Minimum um Tests zu kompilieren und laufen zu lassen  __[+]__
   2. hat den Vorteil, dass die Tests die Anwesenheit des Codes feststellen können __[+]__
4. Schnittstellen müssen nach Spezifikation geschrieben werden (no Client Code MockUp)
---
## Video Beispiel Sudoku-Solver
1. Constructor Test
2. Set Test
3. Get Test
4. Functionality Test
5. Throw Tests Beispiele:
   1. EXPECT_THROW(S.get(9,0), std::logic_error);   // out of Bounce for a 8x8 sudoku grid
   2. EXPECT_THROW(S.set(2,1), std::logic_error);   // value bigger than 9 for sudoku
   3. EXPECT_THROW(S.get(0,0,6), std::logic_error); // failure to override const value
---
## Wieviele Tests sind genug - Entscheidungsfrage
1. Wenige Tests für "Normale/Allgemeine Fälle"
2. 1 Test für jeden "Spezial Fall"(Special Cases, Corner Cases, Edge Cases, Bugs)
3. "Whitebox Testing" ich als Entwickler schreibe die Tests und weiß welche 2 Testcases gleich/ähnlich sind und welche davon abweichen
4. Fehlerbehandlungen sind logisch getrennte Funktionen und sollten seperat in einem eigenen Schritt getestet werden [vor|nach "normal cases"]
---
## Wann ist der Code gut genug
1. Refaktorisierung für Lesbarkeit und Wartbarkeit("claritiy and maintainability")
   1. solange bis es als "Gut genug" empfunden wird, liegt im Auge des Programmierers
2. Code-Verbesserungen beim Refaktorisieren können bereits Optimierungen sein
   1. Wenn Performance-Ziele klar definiert wurden und getestet werden zum Verifizieren
3. Code-Re-Organisation kann Notwendig werden für die nächsten Iterationen des Codes
   1. Neue Interfaces und Optionen, Paramete, etc.
   2. Auf keine Fall: Änderungen und Neu-Entwicklungen verwechseln
4. Code-Verbesserungen selber erfordern das Schreiben neuer Tests
   1. Hier werden mehr|neue Tests hinzugefügt
---
## Was _kann_ und was _soll_ getestet werden
1. public API
2. In Klassen-Hierarchien restricted API(protected)
3. Was noch:
   1. Container können Probleme darstellen, weil man nicht richtig genau die Out-of-bounce für zum Beispiel Arrays[8] testen kann
---
## Sollen Implementationen getestet werden?
1. Ja:
   1. Viel Fehler werden nicht durch die _public API_ beobachtet
2. Nein:
   1. Die Implementation kann sich ständig ändern und die Tests sind dann anfällig
   2. _Einer der häufigsten Gründe warum TestSuites vernachlässigt werden, ist das man ständig die Tests updaten muss_
3. __Testen Tests der Implementiereung die Implementierung?__
   1. Das Problem: _out of Bound_-Zugriff auf Container(class data member arrray, method-call on a data-member)
   2. Die Wurzel des Problems ist die inkorrekte Anwendung der API
   3. Wir haben das Interface der Eineit getestet