# Wie man eine einfache Android-App erstellt - Native Compose-Entwicklung

# 1 Was Android-Apps und Android-Entwicklung sind

In diesem Tutorial werden wir einen vollstaendigen Prozess durchlaufen: **von einer Idee in deinem Kopf bis zu einer echten App, die erfolgreich auf einem Android-Telefon installiert und ausgefuehrt werden kann.**

Fuer dieses Tutorial solltest du mindestens Folgendes haben:

- Einen Computer mit angemessener Leistung (Windows oder Mac)
- Ein Android-Telefon (optional; wenn du keins hast, verwenden wir einen Emulator)
- Android Studio installiert (fuer das Build)
- Trae installiert und registriert (fuer AI-Coding)

## 1.1 Definition einer Android-App

Eine Android-App ist eine native Anwendung, die auf dem Android-Betriebssystem laeuft. Anders als Mini-Programme ist sie nicht von einem Host wie WeChat abhaengig. Sie laeuft direkt auf Systemebene, hat ein eigenes Symbol auf dem Startbildschirm, startet schnell, fuehlt sich fluessig an und kann tief auf Systemfunktionen wie Bluetooth, Sensoren und Hintergrunddienste zugreifen.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image1.png)

## 1.2 Android-App-Entwicklung

Android-Entwicklung bezeichnet den gesamten Prozess der Erstellung solcher Anwendungen. Im in diesem Tutorial verwendeten Vibe-Coding-Entwicklungsmodus mit **KI-gestuetzter Programmierung** wandelt sich die Rolle des Entwicklers vom "Code-Schreiber" zum "Produktarchitekten":

1. **Du (Architekt / PM)**: verantwortlich fuer Geschaeftslogik-Design, Prompt-Erstellung und die endgueltige Abnahme des Ergebnisses.
2. **Trae (KI-Ingenieur)**: verantwortlich fuer die Ausfuehrung von Anweisungen, die Umwandlung natuerlicher Sprache in standardmaessigen Kotlin-Code und Jetpack-Compose-Layouts sowie die Behandlung von Syntaxfehlern und Logikdetails.
3. **Android Studio (Build-Fabrik)**: verantwortlich fuer die Bereitstellung der Kompilierungsumgebung, die Paketierung von Code in eine ausfuehrbare App und Emulator-Vorschauen.

## 1.3 Gängige Methoden zur Erstellung von Android-Apps

In der echten Entwicklung gibt es mehr als einen Weg, Android-Apps zu erstellen. Wir werden hier nicht tief einsteigen, sondern nur einen grundlegenden Ueberblick geben.

**Der erste Weg: Native Entwicklung**
Dies ist der offiziell von Google empfohlene Weg. Du verwendest direkt **Kotlin** und **Jetpack Compose** zur Entwicklung. Der Vorteil ist die beste Performance und der volle Zugriff auf die Telefon-Hardware.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image2.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image3.png)

**Der zweite Weg: Cross-Plattform-Entwicklung**
Zum Beispiel Flutter oder React Native. Die Grundidee ist "ein Codebase schreiben und sowohl Android- als auch iOS-Apps generieren".

**Der dritte Weg: Hybrid-Entwicklung**
Im Wesentlichen wird hierbei eine Webseite in eine App-Huelle verpackt. Dies ist schnell entwickelt, aber das Erlebnis und die Fluessigkeit sind in der Regel nicht so gut wie bei einer nativen App, und es ist schwierig, ein ausgereiftes, immersives kleines Tool zu erstellen.

**Die Wahl dieses Tutorials: native Entwicklung (** **Kotlin + Compose)** kombiniert mit KI-Tools fuer das Coding.
Der Grund ist einfach: nativer Jetpack-Compose-Code hat eine sehr klare Struktur und ist hochgradig dafuer geeignet, von KI verstanden und generiert zu werden. Wir muessen den Code nicht von Grund auf selbst schreiben. Stattdessen leiten wir Trae mit natuerlicher Sprache an, um qualitativ hochwertigen nativen Code zu generieren.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image4.png)

## 1.4 In diesem Tutorial behandelte Android-App-Entwicklungsschritte

Um den Lernprozess interessant zu gestalten, dreht sich dieses Tutorial um einen entspannenden, aber technisch repraesentativen Fall - das **elektronische Holz-Bon-Gong**. Wir kombinieren Traes Vibe-Coding-Modus mit einer Route, die du wiederholt verwenden kannst:

1. **Verstaendnis und Umgebung aufbauen**: verstehen, was Android-Apps sind, Android Studio und Trae installieren und China-freundliche Mirrors konfigurieren, damit die Toolchain reibungslos funktioniert.
2. **Projektgeruest erstellen**: ein leeres Android-Projekt erstellen, das erfolgreich im Emulator ausgefuehrt werden kann.
3. **KI-iterative Entwicklung**: das Projekt in Trae oeffnen und dann durch Gespraeche mit der KI schrittweise das Bon-Gong-Bild, Tipp-Animationen, Soundeffekte, schwebenden Text und mehr implementieren.
4. **Geraete-Debugging und Feinschliff**: den Emulator verlassen, die App auf deinem echten Telefon installieren, echtes Vibrations-Feedback erleben und die KI bei der Fehlersuche helfen lassen.
5. **Paketierung und Veroeffentlichung**: ein formelles APK generieren und verstehen, wie man es teilt oder veroeffentlicht.

Dieser Abschnitt zeigt nur das Gesamtbild und klappt noch nicht alle Befehle aus. Merke dir vorerst die Hauptlinie: **Umgebung einrichten -> Geruest erstellen -> KI-Beschreibung und Generierung -> Geraete-Politur -> Paketierung und Auslieferung**. In den naechsten Kapiteln werden wir dich Schritt fuer Schritt durch jeden Schritt fuehren.

# 2 Entwicklungsumgebung einrichten

## 2.1 In diesem Tutorial verwendete Tools

Waehrend des gesamten Entwicklungsprozesses verwenden wir drei Tools gemeinsam, die die Rollen von "Design", "Konstruktion" und "Abnahme" spielen.

- **Trae**: dies ist dein **KI-Coding-Partner**. Im Vibe-Coding-Modus muessen wir nicht mehr Zeile fuer Zeile Code eingeben. Stattdessen sagen wir der KI hauptsaechlich in natuerlicher Sprache, was wir wollen, und sie kuemmert sich um Codegenerierung und -aenderung.
- **Android Studio**: dies ist Googles offizielle **App-Build-Fabrik**. Obwohl es viele Knoepfe hat, verwenden wir es in diesem Tutorial hauptsaechlich, um das Projektgeruest zu erstellen und den von Trae generierten Code in etwas Installierbares auf einem Telefon zu kompilieren.
- **Ein Android-Geraet**: dies dient als **Testendgeraet** zur Begutachtung des Ergebnisses. Du kannst es mit deinem Computer verbinden, um echtes Vibrations-Feedback zu erleben. Wenn du keins hast, kann der in Android Studio integrierte **Emulator** ein virtuelles Telefon perfekt simulieren, was fuer die fruehe Entwicklung ausreicht.

## 2.2 Trae herunterladen

Trae ist unser Hauptschauplatz fuer **Vibe Coding**. Du kannst es dir einfach als einen **"KI-gestuetzten Code-Editor"** vorstellen.

Besuche die offizielle Website [https://www.trae.cn](https://www.trae.cn), lade die Version herunter, die zu deinem Computersystem passt (Windows oder Mac), und installiere sie wie gewoehnliche Software, indem du auf das Installationsprogramm doppelklickst und den Anweisungen folgst. Sobald dieses Tool bereit ist, werden wir in der spaeteren Praxis aufhoeren, langweilige Code-Fenster anzustarren und stattdessen das Projekt hier oeffnen und der KI in natuerlicher Sprache sagen, was erstellt werden soll.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image5.png)

## 2.3 Android Studio herunterladen

Wir benoetigen Android Studio, um das Android SDK und den Emulator bereitzustellen, die zum Ausfuehren der App erforderlich sind. Besuche die offizielle Download-Seite [https://developer.android.com/studio?hl=zh-cn](https://developer.android.com/studio?hl=zh-cn) und lade das Paket fuer dein Betriebssystem herunter (dieses Tutorial basiert auf **2025.2.3**). Nach dem Download installiere es wie normale Software, wobei du die Standardoptionen beibehaeltst.

**Besonderer Hinweis fuer Anfaenger:**

Obwohl moderne Versionen von Android Studio die Konfiguration stark vereinfacht haben, haengt es im Hintergrund weiterhin vom **JDK (Java Development Kit)** ab. Wenn du zum ersten Mal entwickelst oder waehrend der Installation Fehler im Zusammenhang mit Umgebungsvariablen oder SDK-Konfiguration auftreten, keine Panik. Du kannst diese detaillierte Einrichtungsanleitung konsultieren: [Android Studio 2024 Setup: SDK und Gradle-Konfiguration](https://blog.csdn.net/keiraee/article/details/142321644?ops_request_misc=elastic_search_misc&request_id=a2b858d1f665095c53afa9114ad8864d&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-142321644-null-null.142^v102^pc_search_result_base4&utm_term=android%20studio%E5%AE%89%E8%A3%85%E5%8F%8A%E9%85%8D%E7%BD%AE&spm=1018.2226.3001.4187)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image6.png)

## 2.4 Ein neues Projekt erstellen

Oeffne Android Studio und klicke auf dem Willkommensbildschirm auf **New Project**.

**Schritt 1: Vorlage auswaehlen**

Waehle in der Vorlagenliste **Empty Activity** (beachte das Jetpack-Compose-Symbol darauf).

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image7.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image8.png)

**Schritt 2: Projektkonfiguration ausfuellen**

Dann siehst du ein Konfigurationsformular. Fuelle es ungefaehr wie folgt aus und behalte den Rest bei den Standardeinstellungen:

| **Feld** | **Empfohlener Wert** | **Erklaerung** |
| ----------------- | -------------------------------------------------- | ---------------------------------------- |
| **Name** | My Application 1 | App-Name, der auf dem Telefon-Startbildschirm angezeigt wird |
| **Package name** | com.example.myapplication1 | Eindeutiger App-Identifikator |
| **Save location** | Benutzerdefinierter Pfad (z. B. `E:\AndroidProjects\Myapplication1`) | Projektspeicherort; nicht empfohlen auf Laufwerk C |
| **Minimum SDK** | API 30 | Abdeckt ueber 90% der aktiven Geraete und bietet eine gute Balance zwischen Kompatibilitaet und Funktionen |
| **Language** | Kotlin (empfohlen) | Kotlin ist Googles offiziell empfohlene Sprache, sauberer und sicherer |

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image9.png)

**Schritt 3: Auf Projekterstellung warten**

Klicke auf **Finish**. Android Studio wird automatisch Abhaengigkeiten herunterladen und das Projekt erstellen (du siehst einen Fortschrittsbalken in der unteren rechten Ecke).

- _Hinweis: Die erste Projekterstellung kann mehrere Minuten dauern. Warte geduldig, bis der Fortschritt unten abgeschlossen ist und der Projektdateibaum links vollstaendig geladen ist._

## 2.5 Abhaengigkeitskonfiguration: Gradle-Download und Gradle-Repository-Mirrors

> Dies ist einer der wenigen Schritte im Vibe-Coding-Workflow, bei denen **manuelle Bedienung** empfohlen wird. Obwohl die KI auch bei der Aenderung der Konfiguration helfen kann, betrifft die Umgebungskonfiguration niedrige Dateien, sodass manuelle Aenderungen zuverlaessiger sind.

Warum muessen wir die Konfiguration aendern?

Standardmaessig verbindet sich Android Studio mit Servern im Ausland, sodass das Herunterladen von Build-Tools und Abhaengigkeiten eine Stunde dauern oder sogar fehlschlagen kann. Nach dem Wechsel zu inlaendischen Mirrors ist es oft innerhalb weniger Minuten erledigt. **Dies ist eine einmalige Aufgabe, die sich fuer immer auszahlt.**

1. **Vorbereitung**

Wenn die Statusleiste unten rechts in Android Studio derzeit einen Fortschrittsbalken wie `Gradle Building...` anzeigt, pausiere den laufenden Abhaengigkeits-Download zunaechst, um Dateikonflikte zu vermeiden.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image10.png)

2. **Gradle-Download beschleunigen**

Erweitere im Projektdateibaum links `gradle` -> `wrapper` und oeffne dann `gradle-wrapper.properties`. Aendere die Download-Quelle auf den Tencent-Mirror:

```text
distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-8.7-bin.zip
```

Achtung: Du musst nur `services.gradle.org/distributions` durch `mirrors.cloud.tencent.com/gradle` ersetzen. Aendere nichts anderes.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image11.png)

3. **Abhaengigkeits-Repository-Download beschleunigen**

Oeffne dann `settings.gradle.kts` im Projektstamm und ersetze den Inhalt innerhalb des `repositories`-Blocks durch Folgendes:

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image12.png)

Ersetze den markierten Abschnitt durch diesen Code (aktuelle Quellenliste Stand 2025-02-21):

```json
        // Aliyun Mirrors (deckt Maven Central, Google, JCenter usw. ab)
        maven { setUrl("https://maven.aliyun.com/repository/public/") }
        maven { setUrl("https://maven.aliyun.com/repository/google/") }
        maven { setUrl("https://maven.aliyun.com/repository/jcenter/") }
        maven { setUrl("https://maven.aliyun.com/repository/gradle-plugin/") }
        // Huawei Cloud Mirror
        maven { setUrl("https://repo.huaweicloud.com/repository/maven/") }
        // Tencent Cloud Mirror
        maven { setUrl("https://mirrors.cloud.tencent.com/nexus/repository/maven-public/") }
        // NetEase Mirror
        maven { setUrl("https://mirrors.163.com/maven/repository/maven-public/") }
```

Es sollte dann wie im folgenden Screenshot aussehen:

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image13.png)

4. **Speichern und Aenderungen anwenden**

Speichere nun die Datei und klicke auf `Try Again` in der oberen rechten Ecke. Android Studio wird den Download erneut starten. Warte einige Minuten. Wenn die Konsole `BUILD SUCCESSFUL` anzeigt, ist die Umgebungseinrichtung vollstaendig abgeschlossen und wir koennen mit dem Coden beginnen.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image14.png)

## 2.6 Die Projektstruktur verstehen

Nach erfolgreicher Projekterstellung erscheint das **Projekt**-Panel links. Wechsle zur **Android**-Ansicht (Standard), und du wirst folgende Schluesselverzeichnisse sehen:

```text
app/
├── manifests/
│   └── AndroidManifest.xml            <- App-"Ausweis", deklariert App-Namen und Einstiegs-Activity (MainActivity)
│
├── java/
│   └── com.example.myapplication1/
│       ├── MainActivity.kt            <- App-Einstiegspunkt, erstellt UI mit Jetpack Compose
│       │
│       └── ui/                        <- steuert das Gesamt-UI-Design (Farben, Schriftarten)
├── res/
│   ├── drawable/                      <- Bildressourcen (z. B. ic_launcher.png)
│   ├── mipmap/                        <- App-Symbol
│   ├── values/                        <- Text-, Farb- und Theme-Stile
│   │   ├── colors.xml
│   │   ├── strings.xml
│   │   └── themes.xml
│   └── xml/                           <- systembezogene Konfigurationsdateien (nicht UI)
└── build.gradle (Module: app)         <- App-Build-Konfiguration (auf Anfaengerebene normalerweise unberuehrt)
```

Als Anfaenger muessen wir uns normalerweise nur auf drei Dateien konzentrieren:

- `MainActivity.kt`: steuert das Verhalten und entscheidet, "was auf dem Bildschirm erscheint"
- `AndroidManifest.xml`: registriert Komponenten und entscheidet, "wo die App startet"
- `Theme.kt`: definiert das visuelle Erscheinungsbild

# 3 Android-App-Entwicklung

In den ersten beiden Kapiteln haben wir bereits verstanden, was Android-Apps sind, und die beiden wichtigsten Werkzeuge geschaerft: Trae und Android Studio. Ab diesem Abschnitt verlassen wir die Theorie und betreten die echte Praxis. Wir werden den Vibe-Coding-Modus anwenden, um von Grund auf eine sehr beliebte Stressabbau-App zu erstellen - das **elektronische Holz-Bon-Gong**. Es passt gut zum "Vibe"-Thema (einfach und entspannend) und deckt gleichzeitig drei Kernbereiche der Android-Entwicklung ab: **UI-Interaktion (Tippen), Datenspeicherung (Verdienstzaehler) und Multimedia (Soundeffekte)**.

Jetzt folge den Anweisungen und sende den ersten Befehl an die KI.

## 3.1 Der erste "Master-Prompt": Von Null auf Eins

Im Vibe-Coding-Modus muessen wir nicht wie in der traditionellen Entwicklung zunaechst Layout-Dateien erstellen und dann Logik-Code schreiben. Was wir tun muessen, ist **die Anforderungen auf einmal klar beschreiben und die KI den ersten ausfuehrbaren Prototyp generieren lassen**.

Oeffne das Projektverzeichnis, das wir gerade erstellt haben, in Trae und gib im Chat-Panel rechts den folgenden Prompt ein:

```text
Du bist ein erfahrener Android-Entwicklungsexperte. Bitte schreibe die aktuelle MainActivity.kt um und verwandle sie in eine "Elektronisches Holz-Bon-Gong"-App. Anforderungen:
1. Der Bildschirmhintergrund ist schwarz.
2. Zeige eine Bon-Gong-Grafik in der Mitte des Bildschirms, moderat gross, in Weiss.
3. Zeige eine Zeile weissen Text darueber: "Verdienst: 0".
4. Wenn das Bon-Gong in der Mitte getippt wird, erhoet sich die Zahl um 1 und es erscheint ein einfacher Skalierungs-Animationseffekt (simuliert das Gefuehl des Schlagens).
5. Verwende Jetpack Compose.
```

Nach dem Senden wird Trae beginnen, deine Projektstruktur zu analysieren. Wenige Sekunden spaeter wird es den vollstaendigen Code fuer `MainActivity.kt` direkt generieren.

1. Aus der Antwort koennen wir seine Schlussfolgerungslogik und Interaktionslogik sehen
2. Wir koennen direkt sehen, welche Teile des Codes neu geschrieben wurden
3. Wenn wir mit dem Ergebnis nicht zufrieden sind, koennen wir auf die vorherige Version zurueckkehren

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image15.png)

## 3.2 Ausfuehren und Vorschau (Emulator-Debugging)

An diesem Punkt hat die KI die erste Runde der Entwicklung abgeschlossen. Aber denk daran: Was wir in Trae sehen, sind nur Code-"Blaupausen", keine echte interaktive App. Trae selbst kann Android-Apps nicht direkt ausfuehren, daher muessen wir uns auf das **virtuelle Geraet (Emulator)** von Android Studio verlassen. Es ist, als wuerden wir deinen Computerbildschirm in ein virtuelles Android-Telefon verwandeln, das es uns ermoeglicht, den Code sofort zu installieren und das echte Ergebnis zu sehen.

Lass uns nun dieses "virtuelle Telefon" konfigurieren.

**Schritt 1: Den Emulator erstellen**

Gehe zurueck zu Android Studio, finde und klicke auf **Device Manager** in der rechten Symbolleiste. Wenn du ihn nicht finden kannst, oeffne ihn ueber `View -> Tool Windows -> Device Manager`.

Klicke im Panel auf **Add a new device** und waehle dann **Create Virtual Device**, um in das Geraeteauswahlfenster zu gelangen.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image16.png)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image17.png)

Waehle im Hardware-Auswahlfenster **Phone** und dann **Smart Phone** (mittlere Bildschirmgroesse) oder ein anderes Geraeteprofil wie Pixel, und klicke auf **Next**.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image18.png)

**Schritt 2: Das Systemabbild konfigurieren**

Waehle im Dialog **System Image** die Option **API 36.1**. Wenn es noch nicht heruntergeladen wurde, klicke zunaechst auf **Download**, waehle es nach Abschluss des Downloads aus und klicke auf **Finish**.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image19.png)

**Schritt 3: Den Emulator starten**

Nach erfolgreicher Erstellung erscheint dein neues Telefon in der Geraetemanager-Liste. Klicke auf den **Dreieck-Abspielknopf** rechts. Nach kurzer Wartezeit erscheint ein telefonfoermiges Fenster - das ist dein Android-Emulator.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image20.png)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image21.png)

**Schritt 4: Die App ausfuehren**

Jetzt kommt der magische Moment. Stelle sicher, dass der Emulator gestartet ist und den Startbildschirm anzeigt, und klicke dann auf das markante **gruene Ausfuehrungs-Dreieck** in der oberen Symbolleiste von Android Studio (oder verwende die Tastenkombination `Shift + F10`). Android Studio kompiliert automatisch den von Trae geschriebenen Code, verpackt ihn als App und installiert ihn im Emulator.

Innerhalb von Sekunden solltest du sehen, wie der Emulator-Bildschirm aufleuchtet und eine weisse Bon-Gong-Grafik in der Mitte mit dem Text "Verdienst: 0" darueber anzeigt. Versuche darauf zu tippen und pruefe, ob sich die Zahl erhoelt und die Animation funktioniert. Das ist deine erste Android-App.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image22.png)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image23.png)

## 3.3 Optimierungsiteration (Assets und Sound hinzufuegen)

In dieser Phase hat unsere App bereits eine grundlegende Form: Tippen erhoet die Zahl. Aber es ist immer noch nur eine "stumme" weisse geometrische Form, die an Unterhaltung vermissen laesst. Als Naechstes werden wir das elektronische Bon-Gong durch ein echtes Bild und einen Schlag-Soundeffekt viel immersiver machen.

**Genau das ist der attraktivste Teil des Vibe-Coding-Modus.** In der traditionellen Entwicklung ist das Hinzufuegen von Soundeffekten und komplexeren Animationen oft ein Albtraum fuer Anfaenger. Du musst das Laden und Freigeben von `MediaPlayer`-Ressourcen verwalten (sonst koennen Speicherlecks auftreten) und auch Animationskurven berechnen. Im Vibe-Coding-Modus musst du dich um diese Details auf niedriger Ebuee ueberhaupt nicht kuemmern. Du musst der KI nur wie ein Regisseur sagen: "Aendere das Requisit und fuege einen Soundeffekt beim Tippen hinzu", und die Implementierung erscheint sofort.

**Schritt 1: Assets vorbereiten**
Du benoetigst ein Bon-Gong-Bild (`png`) und einen Schlag-Soundeffekt (`mp3`).

- **Bild-Asset**: Kopiere das vorbereitete `white_muyu.png` in `app/src/main/res/drawable`
- **Audio-Asset**: Klicke in Android Studio mit der rechten Maustaste auf den Ordner `res` im linken Projekt-Panel, waehle `New -> Android Resource Directory`, waehle **raw** als Ressourcentyp, klicke auf OK und kopiere dann `voice.mp3` in den neuen Ordner `res/raw`. _(Hinweis: Wenn du eine kommerzielle Veroeffentlichung planst, stelle sicher, dass du die gesetzlichen Rechte an allen Assets hast.)_

Hier sind die Bild- und Sound-Assets, die ich fuer dich gefunden habe. Wenn es dir unangenehm ist, selbst danach zu suchen, kannst du sie direkt verwenden.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image24.png)

Schlag-Sound-Download-Link: https://www.aigei.com/s?q=%E6%9C%A8%E9%B1%BC&type=sound
Waehle den ersten 1-Sekunden-Soundeffekt.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image25.png)

**Schritt 2: Die Iterationsanweisung senden**

Nachdem die Assets bereit sind, gehe zurueck zu Trae. Trae wird den Code erneut aendern und das Audio-Laden und die Animationslogik fuer dich behandeln. Du musst ihm nur sagen, welche Assets verwendet werden sollen. Gib diesen Prompt ein:

```text
Ich habe die Assets hinzugefuegt. Der Bildpfad ist res/drawable/white_muyu.png und der Soundeffekt-Pfad ist res/raw/voice.mp3. Bitte aktualisiere den Code:
1. Ersetze das Bon-Gong-Symbol in der Mitte durch mein Bild.
2. Spiele jedes Mal, wenn das Bon-Gong getippt wird, den Schlag-Sound ab.
3. Zeige beim Tippen einen voruebergehenden "+1"-Text ueber dem Bon-Gong an und lass ihn dann nach oben schweben und verschwinden (wie schwebende Punktzahlen in Spielen).
```

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image26.png)

**Schritt 3: Das Ergebnis ueberpruefen**

Nachdem Trae die Codeaenderungen abgeschlossen hat, gehe zurueck zu Android Studio und klicke erneut auf den gruene Ausfuehrungs-Knopf (Neu ausfuehren), um den Emulator neu zu starten. Zu diesem Zeitpunkt wird sich deine App verwandelt fuehlen. Versuche kontinuierlich zu tippen - du solltest ein knackiges "Tok-Tok"-Geraeusch hoeren und den schwebenden "Verdienst +1"-Text aufploppen sehen. Dies vollendet den Uebergang von "Demo" zu "Produkt".

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image27.png)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image28.png)

## 3.4 Was tun, wenn Bugs auftreten? (Debugging-Schleife mit KI)

KI-generierter Code ist nicht garantiert beim ersten Versuch perfekt, genau wie auch Top-Ingenieure nicht versprechen koennen, fehlerfreien Code in einem Durchgang zu liefern. Aber im Vibe-Coding-Modus sind Bugs keine Mauer, die dich blockiert; sie werden zu Trittsteinen in deiner Zusammenarbeit mit der KI.

**Fall 1: Die App stuerzt ab**

Angenommen, die App stuerzt sofort nach dem Klick auf Ausfuehren ab oder das Tippen auf das Bon-Gong spielt keinen Sound. Traditionell muesstest du nach dem Fehlercode suchen, Dutzende technischer Foren durchsuchen und viele schwer verstaendliche englische Beitraege lesen. Im Vibe-Coding-Modus musst du nur eine Sache tun - **Bote sein**.

**Schritte:**

1. **Log oeffnen**: Finde das **Logcat**-Panel am unteren Rand von Android Studio (das kleine Katzen-Symbol).
2. **Fehler lokalisieren**: Du siehst scrollende Logs, und die **roten Zeilen** sind normalerweise die Schluesselfehler.
3. **Kopieren und Einfuegen**: Waehle den roten englischen Fehlertext aus, kopiere ihn und fuege ihn in Trae ein: "Ich habe diesen Fehler beim Ausfuehren bekommen. Bitte hilf mir, ihn zu beheben."
4. Die KI kann dir sofort etwas sagen wie: "Das ist passiert, weil die Vibrationsberechtigung in `AndroidManifest.xml` nicht deklariert wurde", und gibt dir dann den korrigierten Code. Du klickst einfach auf Uebernehmen und machst weiter.

**Fall 2: Die App laeuft, aber das Erlebnis fuehlt sich schlecht an**

Manchmal stuerzt die App nicht ab, fuehlt sich aber trotzdem unbefriedigend an. Wenn du beispielsweise sehr schnell auf das Bon-Gong tippst, faellt dir vielleicht auf, dass neue "+1"-Animationen erst erscheinen, nachdem die vorherige "+1" vollstaendig verschwunden ist. Das macht das Feedback traeg und unbefriedigend. Du musst dich nicht selbst mit Multi-Threading oder Animations-Warteschlangen befassen. Du musst nur dieses Unbehagen der KI klar beschreiben.

Sende diese "erweiterte Anweisung" an Trae:

```text
Bitte aendere die aktuelle Animationslogik, um das Problem "schnelles Tippen loest nicht aus" zu loesen.
Aktuelles Problem: Es scheint nur einen Animationszustand zu geben, sodass ich warten muss, bis die vorherige "+1" vollstaendig verschwunden ist, bevor ein weiterer Klick reagiert.
Anforderungen:
1. Ersetze den einzelnen Animationszustand durch eine mutableStateListOf-basierte Liste.
2. Jedes Mal, wenn das Bon-Gong getippt wird, sofort eine neue "+1"-Instanz zur Liste hinzufuegen (mit eigener ID und Anfangsposition), unabhaengig davon, ob die vorherige Animation beendet ist.
3. Iteriere in der UI durch diese Liste, sodass jedes "+1" seine eigene Aufwaerts-schwebende + Ausblende-Animation unabhaengig ausfuehrt.
4. Nachdem eine "+1"-Animation beendet ist, automatisch aus der Liste entfernen, um Speicherlecks zu verhindern.
Bitte stelle den aktualisierten MainActivity.kt-Code direkt bereit.
```

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image29.png)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image30.png)

## 3.5 Demonstration des Endergebnisses

In den vorherigen Schritten haben wir bereits ein elektronisches Bon-Gong erstellt, das man sehen und hoeren kann. Um es naeher an einer veroeffentlichungsfertigen App zu bringen, werden wir in einer letzten Iteration **Beruehrungs-Feedback** und **Anpassung** hinzufuegen. Wir werden zwei Kernfunktionen implementieren: erstens **Vibrations-Feedback**, sodass jeder Tipp eine physische Reaktion vom Telefon-Motor erhaelt und die Immersion stark verbessert; zweitens **angepasster Text**, der es Benutzern ermoeglicht, den Text auf dem Bildschirm zu aendern, beispielsweise "Verdienst +1" in "Gehalt +1" oder "Aergernis -1" umzuwandeln.

Sende den folgenden sorgfaeltig gestalteten Prompt an Trae. Es wird die Dialog-Logik, den Zustandswechsel und die Hardware-Interaktion in einem Durchgang behandeln:

```text
Rolle: Du bist ein Android Jetpack Compose Experte.
Aufgabe: Bitte fuege "angepassten Text" und "Vibrations-Feedback" zur bestehenden elektronischen Bon-Gong-App hinzu.
Anforderungen:
1. Haptisches Feedback
Wenn der Benutzer das Bon-Gong tippt, rufe zusaetzlich zu Sound und Animation das haptische Feedback des Telefons auf (verwende LocalHapticFeedback.current), um eine leichte taktile Reaktion zu geben.
2. Angepasste Textfunktion (UI und Interaktion)
Einstieg: Fuege ein kleines Bearbeitungssymbol neben dem oberen Text wie "Verdienst +1" hinzu (verwende Icons.Default.Edit).
Dialog-Logik: Wenn das Symbol getippt wird, zeige einen Dialog (Dialog/AlertDialog).
    Dialog-Titel: "Inhalt aendern"
    Eingabe: Erlaube dem Benutzer, den Text einzugeben, den er ansammeln moechte (Standard ist "Verdienst")
    Wertauswahl: Unter der Eingabe zwei Optionen anbieten (z. B. RadioButton oder Umschalter), damit der Benutzer "+1" oder "-1" waehlen kann
    Speichern-Knopf: Nach dem Klicken auf Speichern den Dialog schliessen und die neuen Einstellungen auf den Startbildschirm anwenden
    Datenaktualisierung: Wenn der Benutzer den Inhalt aktualisiert, den oberen Zaehler auf 0 zuruecksetzen und von vorne zaehlen
3. Effekt-Aktualisierung
Nach dem Speichern sollten sowohl der obere Zaehler-Text als auch der schwebende Animationstext beim Tippen des Bon-Gongs das benutzerdefinierte Format anzeigen.
    Die Groesse des schwebenden Textes sollte die Groesse des oberen Zaehler-Textes nicht ueberschreiten
    Beispiel: Wenn der Benutzer "Gehalt" eingibt und "+1" waehlt, wird die obere Zaehlerlogik +1 und der schwebende Text wird "Gehalt+1"
    Wenn der Benutzer "Aergernis" eingibt und "-1" waehlt, wird die obere Zaehlerlogik -1 und der schwebende Text wird "Aergernis-1"
4. Technische Anforderungen:
Stelle sicher, dass der neue Zustand (Text und Zahl) die Animation korrekt beeinflusst.
Bitte stelle den vollstaendig aktualisierten MainActivity.kt bereit, waehrend die vorherige Sound- und Animationslogik unveraendert bleibt.
```

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image31.png)

# 4 Geraete-Debugging und Feinschliff

Der Emulator ist praktisch, kann aber keine echte Telefon-Vibration simulieren oder die echte Beruehrungsverzoegerung vollstaendig widerspiegeln. Um das genaueste "Gefuehl" zu bekommen, muessen wir die App auf einem echten Android-Telefon installieren. Im Folgenden sind zwei Verbindungsmethoden aufgefuehrt, aus denen du waehlen kannst:

1. **Drahtloses Debugging (Wi-Fi)**: kein Datenkabel erforderlich, praktisch fuer taegliche Ueberpruefungen. Aber dein Computer und Telefon muessen sich im **gleichen Wi-Fi-Netzwerk** befinden.
2. **USB-Kabel-Debugging**: stabiler und weniger verbindungsanfaellig, geeignet wenn das Netzwerk schlecht ist oder die Ersteinrichtung fehlschlaegt.

## 4.1 Drahtloses Debugging

Dies ist die praktischste Methode unter Android 11 und hoeher.

**Schritt 1: Das Telefon vorbereiten**

1. Stelle sicher, dass sich das Telefon und der Computer im **gleichen Wi-Fi** befinden.
2. Oeffne **Entwickleroptionen** und aktiviere **Drahtloses Debugging**.
3. Tippe auf **Drahtloses Debugging**, um in die Details zu gelangen, und waehle dann **Geraet mit QR-Code koppeln**. Dein Telefon oeffnet eine Scanner-Ansicht.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image32.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image33.png)

**Schritt 2: Am Computer koppeln**

1. Gehe zurueck zu Android Studio und klicke auf die Geraeteauswahl in der oberen Symbolleiste.
2. Waehle **Geraete ueber Wi-Fi koppeln** aus dem Dropdown.
3. Ein QR-Code wird auf dem Bildschirm angezeigt.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image34.png)

**Schritt 3: Scannen und verbinden**

1. Verwende dein Telefon, um den QR-Code auf deinem Computerbildschirm zu scannen.
2. Sowohl das Telefon als auch der Computer sollten "Kopplung erfolgreich" anzeigen.
3. Zu diesem Zeitpunkt zeigt die obere Geraeteleiste von Android Studio automatisch dein Telefonmodell an (z. B. `Google Pixel 8`).

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image35.png)

4. Fuehre die App aus, indem du auf ▶️ Ausfuehren klickst

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image36.png)

## 4.2 USB-Kabel-Debugging

Wenn die drahtlose Verbindung instabil ist oder dein Netzwerk kompliziert ist, ist das Anschliessen mit einem Kabel immer die zuverlaessigste Loesung. Obwohl es weniger bequem ist, bietet es die schnellste Uebertragungsgeschwindigkeit und bricht fast nie ab.

### 4.2.1 USB-Treiber in Android Studio vorbereiten (nur Windows)

Mac-Benutzer koennen diesen Schritt ueberspringen, da macOS das Telefon normalerweise direkt erkennt. Windows-Benutzer muessen sicherstellen, dass der Computer das Android-Telefon erkennen kann, was normalerweise die Installation des Google-USB-Treibers bedeutet:

1. Klicke in Android Studio auf `Tools -> SDK Manager` (oder finde es unter `Settings -> Languages & Frameworks -> Android SDK`)
2. Wechsle zum Tab **SDK Tools**
3. Aktiviere **Google USB Driver** und klicke auf **Apply**, um ihn herunterzuladen und zu installieren

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image37.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image38.png)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image39.png)

### 4.2.2 Dieselbe SDK-Version wie dein echtes Geraet herunterladen

**Schritt 1: Android-Version des Telefons pruefen**

Am Beispiel eines OPPO-Telefons: Oeffne Einstellungen -> Ueber das Telefon -> pruefe die Android-Version (im Beispiel ist es Android 12).

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image40.png)

**Schritt 2: Diese Android-Plattformversion in Android Studio herunterladen**

1. Klicke in Android Studio auf `Tools -> SDK Manager`
2. Bleibe im Standard-Tab **SDK Platforms**
3. Waehle Android 12.0 und klicke auf Apply, um es herunterzuladen

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image41.png)

### 4.2.3 Entwicklermodus auf dem Telefon aktivieren

Oeffne deineTelefoneinstellungen, gehe zu den Entwickleroptionen und aktiviere **USB-Debugging**.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image42.png)

### 4.2.4 USB-Treiber-Autorisierung auf dem Telefon installieren

Nimm nun dein Telefon. Es sollte einen wichtigen Sicherheitsdialog anzeigen: "USB-Debugging erlauben?" Stelle sicher, dass du **Immer erlauben** aktivierst und dann auf **Erlauben** oder **OK** tippst. Dies ist die Schluesselautorisierung, die dem Computer die Kontrolle fuer das Debugging gibt.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image43.png)

### 4.2.5 Die App auf dem Telefon ausfuehren

1. In der oberen Geraeteauswahl von Android Studio solltest du nun dein Telefonmodell sehen (z. B. `OPPO-PDKM00`)
2. Klicke auf ▶️ Ausfuehren. Dein Telefon zeigt den Dialog "USB-Debugging erlauben?" an; aktiviere "Immer erlauben" und bestaetige
3. Die App wird automatisch installiert und gestartet

Jetzt versuche, das Bon-Gong auf deinem Telefon zu tippen und spuere die echte Vibrationsmotor-Reaktion. Das ist das vollstaendige Vibe-Coding-Erlebnis.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image44.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image45.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image46.png)

# 5 Die App als APK paketieren

Der Code ist fertig und der Geraete-Test funktioniert ebenfalls. Jetzt muessen wir die App aus Android Studio "herausholen" und in eine Datei umwandeln, die du an Freunde zum Installieren schicken kannst. Dieser Prozess wird **Paketierung** genannt. In der Android-Entwicklung gibt es zwei vollstaendig verschiedene Paketierungsmodi, und wir waehlen basierend auf dem Verwendungsszenario.

## 5.1 Die Debug-Version paketieren (fuer schnelles Teilen)

Wenn du die App nur mit Freunden fuer einen schnellen Test teilen oder an Testtelefone zur Ueberpruefung senden moechtest, ist die **Debug-Version** die schnellste Option. Sie ist wie ein "Entwurf" - voll funktionsfaehig, aber nicht formal signiert, daher kann sie nicht an App-Stores eingereicht werden.

**Die Schritte sind sehr einfach:** Finde im oberen Menue von Android Studio `Build`, fahre ueber `Generate App Bundles or APKs` und klicke im Untermenue auf `Generate APKs`.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image47.png)

Warte etwa 5 Sekunden, je nach Projektgroesse. Im Konsolenbereich unten rechts in Android Studio erscheint eine Benachrichtigung. Klicke auf den blauen `locate`-Link und der Ausgabeordner oeffnet sich automatisch. Die Datei namens `app-debug.apk` ist das Paket, das wir wollen.

Du kannst es direkt ueber WeChat oder QQ an jedes Android-Telefon senden, und der Empfaenger kann es installieren und verwenden. Beachte, dass die Debug-Version keine Release-Version ist.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image48.png)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image49.png)

## 5.2 Die Release-Version paketieren

Wenn du die App in einem App-Store veroeffentlichen moechtest (wie Google Play oder Huawei AppGallery) oder die Warnung "Unsichere App" waehrend der Installation vermeiden willst, musst du eine **Release-Version** paketieren. Diese Version erfordert eine eindeutige **digitale Signatur**, die wie ein Anti-Faelschungs-Siegel ist und beweist, dass du diese App entwickelt hast und dass sie nicht manipuliert wurde.

> Kernzweck der Signierung
>
> - Die Identitaet des Herausgebers bestimmen: da eine App mit demselben Paketnamen ein installiertes Programm ersetzen kann, verhindert die Signierung, dass dies missbraeucht wird
> - Die Integritaet der App gewaehrleisten: der Signierungsprozess deckt jede Datei im Paket ab und stellt sicher, dass sie danach nicht ersetzt werden

Die Android-App-Signierung ist wie das Anbringen eines Siegels. Nachdem das Siegel angebracht ist, sind die App und der Entwickler miteinander verknuepft: Die App ist deine, und du bist dafuer verantwortlich. Andere koennen dich nicht imitieren, und du kannst andere nicht imitieren.

**Schritt 1: Den Signierungs-Assistenten starten**

Waehle im oberen Menue `Build` und klicke dann auf `Generate Signed Bundle / APK`. Im Popup-Fenster hast du zwei Auswahlmoeglichkeiten:

- Android App Bundle (`.aab`): von Google Play erforderlich, kleiner in der Groesse, aber kann nicht direkt auf einem Telefon installiert werden
- APK: Standard-Installationspaket, kann direkt installiert werden
_Zur Demonstration waehlen wir zunaechst APK und klicken auf Next._

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image50.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image51.png)

**Schritt 2: Einen digitalen Schluessel (KeyStore) erstellen**

Hier bleiben Anfaenger am haeufigsten stecken. Da dies deine erste Release-Paketierung ist, musst du einen neuen **Keystore** erstellen. Klicke unterhalb von `Key store path` auf **Create new**.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image52.png)

Fuelle im Popup die erforderlichen Informationen aus, aehnlich wie bei der Registrierung eines Kontos. Wir empfehlen dringend, dass das Keystore-Passwort und das Schluessel-Alias-Passwort **identisch** sind und dass du sie **sorgfaeltig aufschreibst**. Wenn du dieses Passwort verlierst, kann deine App in Zukunft nie wieder aktualisiert werden.

Klicke nach Abschluss auf OK. Du kehrst zum vorherigen Bildschirm zurueck, und die von dir eingegebenen Schluesselinformationen wurden automatisch ausgefuellt.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image53.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image54.png)

**Schritt 3: Das formelle Paket generieren**

Klicke auf Next, waehle **release** unter Build Variants und klicke schliesslich auf **Create**.

Nach kurzer Wartezeit zeigt Android Studio erneut eine "Generate Signed APK"-Erfolgsmeldung in der unteren rechten Ecke an. Klicke auf **locate** und dieses Mal siehst du das digital signierte formelle Paket im Ordner (normalerweise `app-release.apk` genannt). Diese Datei ist das Endprodukt, das du als Entwickler lieferst.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image55.png)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image56.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image57.png)

# 6 Offizielle Veroeffentlichung in App-Stores / Maerkten

Wenn deine App-Entwicklung abgeschlossen ist und das Release-Paket bereit ist, ist der naechste Schritt die Veroeffentlichung, damit mehr Menschen sie herunterladen und nutzen koennen. Aktuell sind die wichtigsten Vertriebskanaele in zwei Kategorien unterteilt: **inlaendische Android-App-Stores** und **auslaendische App-Stores (Google Play)**.

## 6.1 In inlaendischen Maerkten veroeffentlichen

Das Android-Oekosystem in Festlandchina ist besonders. Es gibt keinen einzigen offiziellen Store (da Google Play nicht direkt zugaenglich ist). Stattdessen ist der Markt zwischen **Hersteller-App-Stores** und **Drittanbieter-Plattformen** aufgeteilt. Die wichtigsten **Hersteller-Stores** umfassen Huawei, Xiaomi, OPPO, vivo, Meizu, Samsung usw. Da sie auf Geraeten vorinstalliert sind, haben sie den groessten Traffic. Die wichtigsten **Drittanbieter-Plattformen** umfassen Tencent MyApp und 360 Mobile Assistant.

### 6.1.1 Die Kernschwierigkeit: Das "Hindernis" fuer Einzelentwickler

Bevor du ein Konto registrierst, gibt es eine sehr wichtige Sache, die du wissen musst: **Inlaendische App-Maerkte sind sehr streng mit Einzelentwicklern.**

Derzeit fordern fast alle grossen inlaendischen App-Stores (Huawei, Xiaomi, OV, MyApp usw.) ein *Software-Copyright-Registrierungszertifikat* fuer die Einreichung.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image58.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image59.png)

- **Was ist das?** Es ist ein rechtliches Dokument, das beweist, dass die App dir gehoert.
- **Kosten fuer die Beschaffung**: Du musst ueber das Urheberrechtsamt beantragen. Selbststaendig dauert es normalerweise 2-3 Monate; ueber eine Agentur fuer schnellere Bearbeitung koennen die Kosten von mehreren hundert bis ueber tausend RMB betragen.
- **Aktuelle Realitaet**: Ohne dieses Zertifikat wird deine App die Pruefung sehr wahrscheinlich nicht bestehen, oder du kannst den App-Eintrag moeglicherweise gar nicht erstellen. Zusaetzlich koennen Kategorien wie Nachrichten, Finanzen und Gesundheitsversorgung auch ICP-Einreichungen oder andere Qualifikationen erfordern.

Wenn deine App also nur ein persoenliches Uebungsprojekt oder kleines Tool ist und du keine Zeit und kein Geld fuer die Beantragung dieses Zertifikats ausgeben moechtest, empfehle ich, direkt zu Abschnitt 6.2 zu springen und Google Play in Betracht zu ziehen oder die APK-Datei einfach direkt mit Freunden zu teilen.

### 6.1.2 Ein Entwicklerkonto registrieren

Wenn du die erforderlichen Qualifikationen bereits vorbereitet hast oder dich entschieden hast, in inlaendischen Maerkten zu veroeffentlichen, ist der erste Schritt die Kontoregistrierung. Der Prozess ist bei den wichtigsten Plattformen aehnlich und erfordert normalerweise eine Ausweisverifizierung fuer Einzelpersonen oder eine Geschaeftslizenzverifizierung fuer Unternehmen.

Im Folgenden sind die Entwicklerplattform-URLs der wichtigsten App-Maerkte aufgefuehrt:

Tencent Open Platform: https://open.tencent.com/

360 Open Platform: http://dev.360.cn

Baidu Developer Platform: http://app.baidu.com

Xiaomi Open Platform: https://dev.mi.com

Huawei Developer Alliance: http://developer.huawei.com/consumer/cn

Alibaba Developer Platform: http://open.uc.cn
Die Alibaba-Vertrieb integriert Wandoujia, Ali Jiuyou, PP Assistant, UC App Store, Shenma Search und YunOS App Store. Du musst nur ein einziges Alibaba-Entwicklerkonto registrieren.

Samsung Developer Platform: http://support-cn.samsung.com/App/DeveloperChina/Home/Index

OPPO Developer Alliance: http://open.oppomobile.com

vivo Developer Alliance: https://dev.vivo.com.cn

Lenovo Open Platform: http://open.lenovo.com

Meizu Developer Alliance: http://open.flyme.cn

Gionee Developer Alliance: https://open.appgionee.com

**Am Beispiel von Tencent MyApp:** Besuche die Tencent Open Platform und klicke auf Registrieren. Es wird empfohlen, sich direkt mit einem QQ-Konto anzumelden. Beachte, dass ein QQ-Konto, sobald es gebunden ist, schwer wieder zu entbinden ist, daher ist es besser, ein dediziertes Arbeits-QQ-Konto zu verwenden. Folge den Anweisungen, waehle "Einzelentwickler" oder "Unternehmensentwickler", lade deine Ausweisfotos hoch und schliesse die Gesichtsverifizierung ab. Nach bestandener Verifizierung klicke auf **App erstellen**, um zu beginnen.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image60.png)![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image61.png)

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image62.png)

### 6.1.3 Einreichungsablauf und erforderliche Unterlagen

Nach Genehmigung der Kontoueberpruefung kannst du die App erstellen und zur Pruefung einreichen. Du musst das folgende "Vierer-Set" vorbereiten:

1. **Installationspaket**: das in Kapitel 5 paketierte **Release-APK**
2. **Textinformationen**:
3. **App-Name**: darf keine sensiblen Woerter enthalten
4. **Einzeilen-Einfuehrung**: innerhalb von 20 chinesischen Zeichen, einfach und direkt (z. B.: "Eine entspannende elektronische Bon-Gong-App")
5. **Detaillierte Beschreibung**: 200+ chinesische Zeichen, die die Funktionen und Verwendungsszenarien der App vorstellen
6. **Visuelle Materialien**:
7. **App-Symbol**: hochaufgeloestes PNG, normalerweise 512x512
8. **App-Screenshots**: bereite 4-5 klare Screenshots der App in Verwendung vor, idealerweise die Hauptseiten abdeckend, normalerweise in einheitlicher Groesse wie 1080x1920
9. **Qualifikationsdokument**: lade einen Scan deines Software-Copyright-Registrierungszertifikats hoch

**Einreichung und Pruefung:** Nachdem alle Informationen ausgefuellt und das APK hochgeladen wurde, klicke auf **Zur Pruefung einreichen**. Der Pruefzyklus betraegt normalerweise 1-3 Werktage. Achte waehrend dieser Zeit auf E-Mail oder SMS. Pruefer koennen die Einreichung ablehnen, weil Screenshots unklar sind, Beschreibungen nicht standardisiert sind oder erforderliche Qualifikationen fehlen. In diesem Fall ueberarbeite entsprechend dem Feedback und reiche erneut ein.

## 6.2 Im auslaendischen Markt veroeffentlichen (Google Play)

Wenn du dich nicht mit der Komplexitaet von Software-Copyright-Zertifikaten und Einreichungen in inlaendischen App-Stores befassen moechtest oder deine Zielgruppe global ist, ist Google Play die beste Wahl fuer Einzelentwickler.

### 6.2.1 Vorbereitung

- **Google-Konto**: ein normales Gmail-Konto reicht aus
- **25 $ Registrierungsgebuhr**: dies ist eine **einmalige lebenslange Gebuehr** und erfordert eine Kreditkarte, die USD-Zahlungen unterstuetzt (Visa / Mastercard)
- **Zuverlaessiger Netzwerkzugang**: Du musst reibungslos auf die Google Play Console zugreifen koennen
- **Formelles Installationspaket**: Beachte, dass Google Play das **.aab**-Format (Android App Bundle) erfordert, nicht APK. Waehle in Android Studio waehrend der Paketierung Android App Bundle. Die Schritte sind fast identisch mit der APK-Paketierung.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image63.png)

### 6.2.2 Google Play Console Veroeffentlichungsprozess (Ueberblick)

Da die Google Play-Registrierung und -Zahlung noch einige Einstiegshuerden haben (wie die Notwendigkeit einer auslaendischen Kreditkarte), bietet dieses Tutorial derzeit keine Schritt-fuer-Schritt-Screenshots. Hier ist jedoch der uebliche Vier-Schritte-Prozess:

**Schritt 1: Eine App erstellen und die Konsole aufrufen**

Klicke auf `Create app`, fuelle den App-Namen aus (`Electronic Wooden Fish`), waehle Englisch als Sprache, waehle App und Kostenlos als App-Typ und aktiviere die Zustimmung. Danach hast du Zugang zum Backend.

**Schritt 2: Die Store-Seite gestalten**

Dies ist der erste Eindruck des Benutzers. Du musst das vorbereitete App-**Symbol** (512x512) und eine **Feature-Grafik** (1024x500) hochladen. Was die englische Beschreibung betrifft, kannst du einfach Trae fragen: **"Bitte hilf mir, eine englische Beschreibung fuer die Veroeffentlichung des Electronic Wooden Fish auf Google Play zu schreiben, in einem leichten und entspannten Ton."** Die KI schreibt es normalerweise natuerlicher als eine direkte Uebersetzung.

**Schritt 3: Datenschutz und Inhaltseinstufung**

- Datenschutzrichtlinie: Suche nach "App Privacy Policy Generator" und generiere einen kostenlosen Link zum Einfuegen
- Inhaltseinstufung: fuelle einen einfachen Fragebogen aus (z. B. ob es Gewalt oder Gluecksspiel gibt). Electronic Wooden Fish erhaelt normalerweise eine allgemeine 3+-Einstufung.

**Schritt 4: Hochladen und veroeffentlichen**

Klicke im Menue `Production` auf `Create new release`, lade deine `.aab`-Datei hoch, speichere und reiche zur Pruefung ein. Die Google Play-Pruefung ist normalerweise schnell (1-3 Tage). Sobald genehmigt, kann deine App weltweit heruntergeladen werden.

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image64.png)

_Wenn du die Entwicklerkontoregistrierung bereits abgeschlossen hast, kann dich dieses Video-Tutorial durch den restlichen Prozess fuehren:_ [Vollstaendiger Workflow: Hochladen einer Android-App auf Google Play](https://www.bilibili.com/video/BV16REQzGEnk/?share_source=weixin&vd_source=b42f227a4f2d413fbde18499d83227cf)

# 7 Abschliessende Bemerkungen

Damit sind wir am Ende des Tutorials angelangt. Wenn du das elektronische Bon-Gong auf deinem Telefon betrachtest, das du persoenlich erstellt hast, frage ich mich, wie du dich jetzt fuehlst.

Als jemand mit einer Ausbildung in Softwaretechnik bin ich in der heutigen schnell entwickelnden KI-Aera tatsaechlich ziemlich emotional. Frueher haben wir uns durch dicke Programmierbuecher gearbeitet, komplexe Syntax gelernt, mit der Umgebungseinrichtung gekaempft und den halben Tag damit verbracht, rote Fehlermeldungen zu bekampfen. Aber die Zeiten haben sich geaendert, und jetzt lernen wir zunehmend, wie man KI leitet.

Durch diese Vibe-Coding-Praxis hast du bereits den vollstaendigen Android-App-Entwicklungsprozess erlebt. Die technische Barriere sinkt tatsaechlich. Wir muessen nicht mehr staendig trockenen Code durcharbeiten und koennen mehr Energie darauf verwenden, zu entscheiden, **was erstellt werden soll**. Aber egal wie stark die Tools sind, sie bleiben nur Tools. Lass diese App nicht auf deinem Telefon Staub ansetzen. Bastel weiter daran, zerstoere sie und repariere sie wieder. Erst wenn du anfaengst, eigene Ideen zu haben und sie zum Leben zu erwecken, hast du wirklich die Schwelle ueberschritten.

Wenn dir dieses Tutorial geholfen hat, zu spueren, dass "eine App zu erstellen tatsaechlich nicht so schwer ist", dann bin ich geehrt, einen weiteren Builder der neuen Generation in die Entwicklungswelt gebracht zu haben.

Ich freue mich sehr auf dein naechstes Werk. Mach weiter so!

![](../../../../zh-cn/stage-3/cross-platform/android-app/images/image65.png)

**_Viel Spass in der Welt der Android-Entwicklung!_**

# Referenzen

CSDN: [Wie man ein Android Studio Projekt paketiert/erstellt (2024-03-04)](https://blog.csdn.net/GenuineMonster/article/details/136443130?ops_request_misc=&request_id=&biz_id=102&utm_term=android%20studio%20%E6%89%93%E5%8C%85%20APK%20%E5%B9%B6%E5%88%86%E4%BA%AB&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-136443130.142^v102^pc_search_result_base4&spm=1018.2226.3001.4187)

CSDN: [Android Studio Installation und Konfiguration](https://blog.csdn.net/Changersh/article/details/149838228?ops_request_misc=&request_id=&biz_id=102&utm_term=android%20studio%E5%AE%89%E8%A3%85%E5%8F%8A%E9%85%8D%E7%BD%AE&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-149838228.142^v102^pc_search_result_base4&spm=1018.2226.3001.4187)
