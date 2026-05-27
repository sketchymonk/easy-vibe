# Dify Einführung und Wissensdatenbank-Integration

# Rückblick auf die letzte Lektion

In den vergangenen Lektionen haben wir in Gruppen die Grundlagen von KI-Programmierung, Prompt-Engineering und KI-Bildgenerierung kennengelernt. Diese Inhalte haben uns geholfen, ein erstes Verständnis für die Grenzen und Fähigkeiten verschiedener Großer Sprachmodelle (LLMs, Large Language Models) bzw. generativer Modelle zu entwickeln.

Um dir bei der Wiederholung des letzten Unterrichts zu helfen, sind hier einige Fragen zum Nachdenken:

1. Was ist KI-Programmierung? Wie verwendet man KI-Programmierwerkzeuge (z. B. [z.ai](http://z.ai)), um eine Webseite zu erstellen?
2. Was ist ein Großes Sprachmodell? Was sind Prompt-Engineering und Context-Engineering? Wie schreibst du einen komplexen Prompt?
3. In Bezug auf die drei verschiedenen Richtungen Text, KI-Coding und Bildgenerierung – wo siehst du jeweils die Stärken und Schwächen der Modelle?
4. Was ist eine API? Wie verwendet man [z.ai](http://z.ai), um auf Drittanbieter-APIs zuzugreifen?

Wenn du bei einer dieser Fragen noch Unsicherheit hast, kannst du die Unterlagen der letzten Lektion noch einmal durchsehen oder einfach in der WeChat-Gruppe fragen.

In dieser Lektion werden wir von einfachen KI-Text- und Bild-Tools zu einer Workflow-Plattform übergehen, die näher an realen Unternehmensanwendungen ist. Vom Chatbot hin zum KI-Agenten und KI-Workflow – und über eine API wird daraus eine interaktive „intelligente" Bot-Seite.

Falls du bei einzelnen Schritten Schwierigkeiten hast, mach dir keine Sorgen. Es empfiehlt sich, jederzeit einen Screenshot der aktuellen Oberfläche zu machen und diesen einem Großen Sprachmodell zur Analyse vorzulegen. Aktuelle Modelle können die meisten häufigen Fragen bereits beantworten.

Wenn auch nach dem Fragen keine Lösung gefunden wird, scheue dich nicht, einfach etwas auszuprobieren. Keine Angst vor Fehlern – jeder Versuch ist eine Gelegenheit zu lernen und Fortschritte zu machen. Mit zunehmender Praxis wirst du immer sicherer und die Bedienung immer flüssiger!

# Was du in dieser Lektion lernen wirst

1. Warum der Übergang vom Chatbot zum Agenten und zur Workflow-Orchestrierung notwendig ist.
2. Was Agenten- und Workflow-Entwicklungsplattformen sind und wie man KI-Fähigkeiten als standardisierte, orchestrierbare Prozesse (SOPs) aufbaut.
3. Was Dify ist und wie man mit dieser Open-Source-Plattform für LLM-Anwendungen schnell Anwendungen erstellt, insbesondere Wissensdatenbank-Frage-Antwort-Bots.
4. Die Implementierungsmethoden und den Wert von RAG – warum Retrieval-Augmented Generation benötigt wird.
5. Wie du von Grund auf Dify und die KI IDE Trae erlernst (`Extra Knowledge 4 - What is AI IDE and Trae`), einschließlich der Erstellung von Agenten, Workflows und der Entwicklung einer Frontend-Chatbot-Webanwendung basierend auf der Dify API.

- Grundlegende Verwendung von Dify sowie Methoden zur Erstellung von Agenten und Workflows, API-Aufrufmethoden.
- Verwendung einer KI IDE und wie man mit einer KI IDE programmiert.
- Eine interaktive Frontend-Webanwendung als Agenten-Programm.

# 1. Vom Dialog zum Agenten

In der vorherigen Phase haben wir gelernt, wie man mit Prompts große Modelle dazu bringt, Rollen zu spielen, Texte zu generieren oder einfachen Code zu schreiben. Wenn du jedoch genauer nachdenkst, erkennst du ein Problem: Ein Chatbot allein kann keine echten Aufgaben erledigen.

Er kann die Frage „Wie prüfe ich eine Bestellung?" beantworten, aber nicht wirklich in einer Datenbank nach den entsprechenden Daten suchen; er kann beschreiben, was ein Wochenbericht enthalten sollte, aber nicht automatisch deine Projektdaten zusammenfassen und eine E-Mail senden. Diese Einschränkung des „nur Redens, kein Handelns" macht es für reine dialogbasierte KI schwierig, in echte Geschäftsprozesse integriert zu werden.

Um KI vom Chat-Partner zum digitalen Mitarbeiter zu machen, müssen wir ihr drei Kernfähigkeiten verleihen:

1. Exklusives Wissen – die Fähigkeit, Produktdokumentation, Kundendaten und interne Richtlinien zu lesen und zu verstehen;
2. Werkzeugnutzung (oder Plugins) – die Fähigkeit, Datenbanken zu bedienen und APIs aufzurufen;
3. Strukturierte Ausführung – die Fähigkeit, Aufgaben nach vordefinierter Logik Schritt für Schritt zu erledigen statt frei zu improvisieren.

Dies ist der Grundentwurf eines KI-Agenten (AI Agent): eine automatisierte Einheit mit Ziel, Wissen, Werkzeugen und Ausführungspfad.

![](images/image1.png)

> Hinweis: Was in der aktuellen Branche als einfache Version eines „Agenten" bezeichnet wird, meist meist eine erweiterte Anwendung, die auf der Kombination aus LLM + Werkzeugen + Wissensdatenbank basiert, und keinen selbstständig planenden Agenten im eigentlichen Sinne. Einfache Agenten verfügen zwar nicht über echte Schlussfolgerungs- und Langzeitplanungsfähigkeiten, reichen jedoch aus, um eine Vielzahl von Automatisierungsszenarien im Unternehmensumfeld zu unterstützen. Wir werden in späteren Kapiteln ausführlich auf echte, selbstständig planende und handelnde Agenten eingehen.

## 1.1 Der einfachste Agent: Frage-Antwort-Bot auf Basis einer Wissensdatenbank

Nachdem die vielfältigen Kernfähigkeiten eines Agenten definiert sind, stellt sich eine interessante Frage: Kann man allein durch die Umsetzung einer einzigen, einfachsten Funktion einen tatsächlich nutzbaren Basis-Agenten erstellen? Die Antwort lautet: Ja.

Tatsächlich ist das Kernanliegen der Nutzer in vielen realen Geschäftsszenarien nicht, dass KI automatisch komplexe Operationen ausführt (wie API-Aufrufe oder die Koordination von Aufgaben über Systeme hinweg), sondern dass sie auf der Grundlage unternehmenseigener Dokumente präzise und zuverlässige Fragen beantwortet. Dies entspricht genau der ersten der drei Kernfähigkeiten eines Agenten: der Fähigkeit zur Bereitstellung von exklusivem Wissen. Daraus ergibt sich die einfachste und am weitesten verbreitete Form eines Agenten: der Frage-Antwort-Bot auf Basis einer Wissensdatenbank.

Obwohl er noch nicht über Werkzeugnutzung oder autonome Planungsfähigkeiten verfügt, liegt der entscheidende Durchbruch darin, dass die Antworten des großen Modells nicht mehr frei erfunden werden, sondern auf belegbaren Grundlagen beruhen. Wie wird das erreicht? Die Lösung des Kernproblems lautet: Wenn ein Unternehmen über Tausende von Dokumentseiten verfügt, wie findet das Modell bei jedem Gesprächsdialog schnell die Inhalte, die für die aktuelle Frage am relevantesten sind?

Eine Lösung hierfür ist: Retrieval-Augmented Generation (RAG).

Der Grundgedanke von RAG ist: Wenn ein Nutzer eine Frage stellt, sucht das System zunächst in der Unternehmens-Wissensdatenbank nach den Textfragmenten, die semantisch am stärksten mit der Frage verwandt sind (z. B. ein Abschnitt aus dem Produkthandbuch oder eine Klausel aus den HR-Richtlinien). Diese Fragmente werden dann als Kontext in die Eingabe des großen Modells „eingefügt", um es dazu zu bringen, die Antwort auf der Grundlage realer Dokumente zu generieren.

![](images/image2.png)

Bildquelle: [https://www.datacamp.com/blog/what-is-retrieval-augmented-generation-rag](https://www.datacamp.com/blog/what-is-retrieval-augmented-generation-rag)

So beruht die Antwort des Modells nicht mehr auf dem verallgemeinerten Wissen seiner Trainingsdaten, sondern wird auf den von dem Unternehmen bereitgestellten verlässlichen Informationen verankert. Das Ziel von RAG ist es, durch diese dynamische Einbindung externen Wissens die Authentizität, Genauigkeit und Konsistenz der Antworten deutlich zu steigern – sogar dahingehend, dass Antworten „zur Rolle passen", beispielsweise im Kundenservice-Jargon oder im Stil einer technischen Dokumentation.

In der geschäftlichen Praxis ist diese Technologie besonders wichtig, da große Modelle häufig „Halluzinationen" erzeugen. Wenn du beispielsweise als CFO oder Berater nach konkreten Daten eines bestimmten Zeitraums fragst, könnte das Modell Datumsangaben und Ereignisse erfinden. Durch die Einführung von RAG werden die Kontrollierbarkeit und Zuverlässigkeit der Antworten deutlich verbessert.

![](images/image3.png)

Bildquelle: [https://www.databricks.com/glossary/retrieval-augmented-generation-rag](https://www.databricks.com/glossary/retrieval-augmented-generation-rag)

Im praktischen Teil dieser Lektion werden wir die beliebte KI-Workflow-Plattform Dify verwenden, um Schritt für Schritt einen Frage-Antwort-Bot auf Basis einer Wissensdatenbank zu erstellen. Du kannst problemlos verschiedene Arten von exklusivem Material in eine Wissensdatenbank umwandeln, wie Produkthandbücher, Unternehmensrichtlinien, Projektdokumentation, Forschungsarbeiten, Knowledge-Base-Artikel oder sogar persönliche Notizensammlungen.

Nach Abschluss der Einrichtung kannst du verschiedene Fragen stellen, um die Fähigkeiten zu testen, zum Beispiel:

- „Welche wesentlichen Funktions-Upgrades hat die neueste Version unseres Produkts A?"
- „Bitte erkläre mir anhand des Mitarbeiterhandbuchs, wie die Urlaubsregelungen für dieses Jahr festgelegt sind."
- „Wie wurde die technische Herausforderung ‚XXX', auf die wir im Projekt XX gestoßen sind, gelöst?"
- „Welche Kernforschungsmethode wird in dieser Arbeit beschrieben?"

Du wirst aus erster Hand erleben, wie die RAG-Technologie statische, verteilte Dokumentationen in eine präzise, intelligente Wissensdatenbank verwandelt, die hochpräzise Frage-Antwort-Unterstützung für unterschiedlichste Szenarien bietet.

## 1.2 Vom dialogbasierten Agenten zum Workflow

Selbst ein „erweiterter Agent" mit Wissensdatenbank und Plugin-Aufruffähigkeiten stößt bei komplexeren Geschäftsprozessen noch an seine Grenzen.

Stell dir folgende Nutzeranfrage vor: „Welche Funktionsupdates hat unser neues SaaS-Produkt kürzlich erhalten? Kannst du mir eine Kundenübersicht zusammenstellen?"

Diese Anfrage wirkt einfach, erfordert jedoch im Hintergrund mehrere koordinierte Schritte: Zunächst müssen die Funktionsveröffentlichungen des letzten Monats aus der internen Produktdokumentation oder der Notion-Wissensdatenbank abgerufen werden; dann die kundenrelevanten Kernfunktionen herausgefiltert werden; anschließend ein großes Modell die technischen Beschreibungen in kundenfreundliche Sprache übersetzt; und schließlich der generierte Inhalt an das Marketing-Team per E-Mail gesendet oder in einer Google Docs-Vorlage gespeichert wird.

Wenn man sich nur auf die freie Schlussfolgerung eines großen Sprachmodells verlässt, ist nicht einmal sicher, ob ein einziger Dialog alle Schritte abschließen kann. Selbst wenn, werden leicht wichtige Informationen übersehen, interne Begriffe mit Kundensprache verwechselt oder die Ausgabe ist nicht strukturiert. Noch wichtiger ist: Unternehmen benötigen überprüfbare, wiederverwendbare und überwachbare standardisierte Ausführungspfade – nicht die improvisierte Leistung eines Modells bei jedem Durchlauf. Überwachbarkeit und Reproduzierbarkeit sind für Unternehmen von entscheidender Bedeutung; unerwartete Ergebnisse können zu unvorhergesehenen und schwerwiegenden Verlusten führen.

Dies führt zu einem fortgeschritteneren KI-Anwendungsparadigma: dem KI-Workflow (AI Workflow).

![](images/image4.png)

Ein Workflow bedeutet, eine komplexe Aufgabe in mehrere geordnete, konfigurierbare und automatisch ausführbare Teilschritte zu zerlegen und ihre logischen Beziehungen – wie Bedingungsprüfungen, Schleifen oder Parallelausführung – visuell oder per Code zu orchestrieren. Die Standardisierung von KI-Fähigkeiten als SOP (Standard Operating Procedure) bedeutet, die Erfahrung, wie eine Aufgabe mit KI erledigt wird, als wiederverwendbare Vorlage zu fixieren.

Diese Vorgehensweise bringt vielfältigen Mehrwert: Nicht-Techniker (wie Produktmanager oder Marketing-Mitarbeiter) können durch Drag-and-Drop-Komponenten schnell KI-Anwendungen erstellen; Entwickler können RAG-Retrieval, LLM-Aufrufe, API-Tools usw. als Standardknoten verpacken und in verschiedenen Geschäftsszenarien wiederverwenden; der gesamte Prozess kann vollständig nachverfolgt, debuggt und kontinuierlich optimiert werden, was den Anforderungen von Unternehmen an Stabilität und Compliance gerecht wird.

KI-Workflows richten sich an eine breite Nutzerschaft. Produktmanager können ohne Programmierung komplette Nutzerinteraktionspfade entwerfen; Marketing-Mitarbeiter können schnell Kundenservice-Bots, Content-Generatoren oder Benachrichtigungssysteme aufbauen; Entwickler und Algorithmus-Ingenieure können Kernfähigkeiten modularisieren und für Frontend-Aufrufe bereitstellen; Gründer oder Solo-Entwickler können mit minimalen Kosten ein KI-Produkt-MVP validieren und innerhalb weniger Tage einen vollständigen Prototyp mit Datenabfrage, Inhaltserstellung und Aktionsausführung online stellen.

Darüber hinaus ist erwähnenswert, dass KI-Workflows in der Regel durch eine Zwischendarstellung (Intermediate Representation) beschrieben werden können. Die konkrete Ausdrucksweise variiert zwar zwischen den verschiedenen Workflow-Plattformen, aber die meisten verwenden strukturierte Dateien (wie JSON, YAML usw.), um Knotentypen, Ein- und Ausgaben sowie Ausführungslogik zu definieren, ähnlich der folgenden Darstellung:

![](images/image5.png)

Kurz gesagt: Wenn Agenten die KI vom „Können-Reden" zum „Können-Handeln" bringen, dann lassen Workflows die KI vom „gelegentlich eine Sache schaffen" zum „stabilen, zuverlässigen und skalierbaren Erledigen einer ganzen Aufgabengattung" übergehen. In der kommenden Praxis werden wir die Dify-Plattform nutzen, um selbst einen vollständigen KI-Workflow zu erstellen und den gesamten Prozess von der Idee bis zur lauffähigen Anwendung zu erleben.

## 1.3 Gängige Agenten-/Workflow-Plattformen

Mit der rasanten Entwicklung der generativen KI-Technologie sind eine Reihe von Low-Code- und sogar No-Code-Agenten- und Workflow-Plattformen entstanden, die Entwicklern und Fachanwendern helfen sollen, schnell Agenten und automatisierte Prozesse zu erstellen, ohne sich in die Komplexität der Programmierung vertiefen zu müssen.

Zunächst ist zu klären: Low-Code-Plattformen sind Entwicklungswerkzeuge, die durch visuelles Ziehen von Komponenten, vordefinierte Geschäftslogik-Vorlagen und grafische Regelkonfiguration den manuellen Programmieraufwand deutlich reduzieren. Ihr Kern besteht darin, visuelle Konfiguration und knotenbasiertes Drag-and-Drop-Programmieren als Ersatz für direktes Code-Schreiben zu nutzen. Dies befreit Entwickler mit technischen Fähigkeiten von repetitiver Arbeit und ermöglicht es gleichzeitig Nicht-Technikern mit Geschäftslogik-Kenntnissen, an der Anwendungserstellung teilzunehmen. Im Wesentlichen bilden sie eine Brücke zwischen Entwicklungseffizienz und Szenario-Flexibilität.

Der herausragende Wert solcher Low-/No-Code-Agentenplattformen liegt darin, die Einstiegshürde für KI-Anwendungen erheblich zu senken. Früher benötigte ein Team Wochen kooperativer Arbeit – von der Anforderungsanalyse über Code-Entwicklung bis hin zu Test und Bereitstellung –, um einen KI-Agenten (wie einen Kundenservice-QA-Bot oder einen Datenverarbeitungsassistenten) fertigzustellen. Mit den visuellen Werkzeugen der Plattformen lässt sich die Zeitspanne „von der Idee bis zum Live-Betrieb" auf wenige Stunden verkürzen.

Die derzeit gängigsten Low-Code-KI-Workflow-Plattformen auf dem Markt umfassen:

| Plattform                                    | Merkmale                                          | Anwendungsbereiche                     |
| -------------------------------------------- | ------------------------------------------------- | --------------------------------------- |
| Dify                                         | Open Source, Wissensdatenbank-RAG, LLM-Orchestrierung, API-Ausgabe, chinesischfreundlich | Unternehmens-Wissensdatenbank-QA, maßgeschneiderte Agenten, API-Dienste |
| Coze (ByteDance)                             | In China verfügbar, Integration des Douyin/Feishu-Ökosystems, umfangreiche Plugins | Social-Bots, Integration chinesischer Mini-Programme |
| n8n                                          | Universelles Automatisierungstool, KI-Knoten-Unterstützung, Fokus auf API-Orchestrierung | Systemübergreifende Datensynchronisation, KI + traditionelle SaaS-Automatisierung |
| Baidu Qianfan AppBuilder / Alibaba Bailian / Tencent HunYuan | Cloud-native Lösungen großer Anbieter, Integration eigener Modelle | Unternehmensweite Bereitstellung, Szenarien mit hohen Compliance-Anforderungen |

Die Auswahl an Low-Code-KI-Workflow-Plattformen auf dem Markt ist reichhaltig. Obwohl Mainstream-Cloud-Anbieter wie AWS, Azure und Alibaba Cloud entsprechende KI-Workflow-Lösungen anbieten, zeichnen sich Dify, Coze und n8n durch die folgenden drei Kernvorteile als die derzeit am weitesten verbreiteten Vertreter aus:

1. Höchste Benutzerfreundlichkeit. Die Plattformen verwenden visuelle Drag-and-Drop-Oberflächen, sodass Nutzer ohne tiefes technisches Verständnis schnell starten können.
2. Hohe Flexibilität. Unterstützung für benutzerdefinierte Komponenten und erweiterbare API-Schnittstellen, die sowohl für leichtgewichtige Szenarien wie Lehrdemonstrationen und MVP-Validierungen als auch für agile Iterationen kleiner und mittlerer Teams geeignet sind.
3. Ausgereiftes Ökosystem. Nicht nur die offizielle Dokumentation ist detailliert und die Reaktionszeiten kurz, sondern auch die aktive Nutzergemeinschaft erleichtert den schnellen Zugriff auf vorgefertigte Lösungen unterschiedlicher Nutzer.

Alle drei Plattformen unterstützen die Ausgabe der erstellten KI-Agenten als standardisierte API-Schnittstellen, die nahtlos in Frontend-Webanwendungen, unternehmensinterne ERP-Systeme oder mobile Apps integriert werden können, was die technische Einstiegshürde für die praktische Umsetzung von KI-Fähigkeiten weiter senkt.

### 1.3.1 Dify: Unternehmensweite LLMOps- und Anwendungslebenszyklus-Management-Plattform

Dify positioniert sich als LLM-Anwendungsentwicklungs- und betriebsplattform mit dem Ziel, das gesamte Lebenszyklusmanagement von KI-Anwendungen – von der Konzeption über die Bereitstellung bis zur Optimierung – anzubieten. Der Kern ist eine Low-Code-Plattform, die Entwicklern und innovativen Nicht-Technikern helfen soll, schnell produktionsreife KI-Anwendungen zu erstellen.

![](images/image6.png)

Funktional deckt Dify visuelles Workflow-Design, Agenten-Erstellung, Wissensdatenbank-Verwaltung und Multi-Modell-Unterstützung ab. Die Plattform ermöglicht die Gestaltung komplexer Aufgabenabläufe durch Drag-and-Drop-Knoten und unterstützt die Erstellung intentionbasierter Agenten. Die Wissensdatenbank-Funktionalität sticht besonders hervor: Sie kann Dokumente in verschiedenen Formaten verarbeiten und eine effiziente Vektor-Suche durchführen. Gleichzeitig ist Dify mit zahlreichen LLMs kompatibel, darunter GPT, Claude und verschiedene Open-Source-Modelle. Die erstellten Anwendungen können per Klick als Standard-API veröffentlicht und einfach integriert werden.

![](images/image7.png)

Hinsichtlich der technischen Architektur zeichnet sich Dify durch Open Source und die Möglichkeit der privaten Bereitstellung aus, mit Fokus auf Flexibilität, Skalierbarkeit und Unternehmens-Compliance. Die Zielnutzer umfassen Entwicklerteams und Business-Innovatoren; typische Anwendungsszenarien reichen von Unternehmens-Wissensdatenbanken und intelligentem Kundenservice über automatisierte Inhaltserstellung bis hin zu branchenspezifischen KI-Assistenten und unternehmensweiten KI-Plattformen.

### 1.3.2 Coze (ByteDance): Wegbereiter der No-Code-KI-Agenten-Erstellung

Coze ist eine von ByteDance herausgegebene KI-Agenten-Entwicklungsplattform, die mit maximaler Benutzerfreundlichkeit als Kern auch Nutzer ohne Programmiererfahrung in die Lage versetzt, funktionsreiche KI-Chatbots einfach zu erstellen, zu testen und zu veröffentlichen.

![](images/image8.png)

Ihr Kernprinzip ist die Vereinfachung der Bot-Erstellung zu einer Art Baukasten-Prinzip. Nutzer können über die Oberfläche mühelos Rollen und Wissensdatenbanken konfigurieren und die umfangreiche integrierte Plugin-Bibliothek nutzen, um dem Bot externe Fähigkeiten wie Nachrichten, Reisen oder Bildgenerierung hinzuzufügen. Der erstellte Bot kann per Klick schnell auf Plattformen wie Doubao, Feishu und WeChat-öffentliche Konten veröffentlicht werden.

![](images/image9.png)

Die technische Architektur ist vollständig auf niedrige Einstiegshürden ausgerichtet; das Backend integriert Bytedances eigene Modelle und kapselt komplexe Abläufe, wobei multimodales Verständnis und Echtzeit-Antworten im Vordergrund stehen. Als primär als Cloud-Angebot betriebene Plattform sind die Möglichkeiten zur privaten Bereitstellung relativ begrenzt. Typische Anwendungsszenarien umfassen persönliche Assistenten und Unterhaltungs-Bots, intelligenten Kundenservice und QA-Systeme, Online-Bildungsassistenten und schnelle Prototyp-Validierung.

### 1.3.2 n8n: Programmierbare Backend-Workflow-Automatisierungs-Engine

n8n ist eine universelle, programmierbare Workflow-Automatisierungsplattform, deren Kernpositionierung darin besteht, verschiedene Anwendungen, Datenbanken und APIs miteinander zu verbinden und Datenflüsse sowie automatisierte Aufgabenausführung zu realisieren.

Über eine umfangreiche Bibliothek von Integrationsknoten unterstützt sie Hunderte von SaaS-Diensten, Datenbanken und Protokollen und verbindet visuelle und codebasierte Ansätze: Nutzer können Knoten auf einer Leinwand ziehen und gleichzeitig JavaScript- oder Python-Code einfügen, um benutzerdefinierte Logik zu schreiben. n8n ist besonders stark bei datenintensiven Backend-Aufgaben wie Datensynchronisation, ETL-Prozessen und API-Orchestrierung.

![](images/image10.png)

Ein Schlüsselmerkmal ist „quelloffen" und „selbst hostbar": Nutzer können die Plattform privat bereitstellen und volle Kontrolle über Daten und Umgebung behalten, was sie für Branchen mit hohen Datensicherheitsanforderungen besonders attraktiv macht. Die Hauptzielgruppe sind Entwickler, technisch versierte Betriebsmitarbeiter und Datenanalysten. Der größte Vorteil von n8n liegt in seinem äußerst starken Community-Ökosystem. Im Internet findet man eine Fülle von n8n-Tutorials und Erfahrungsberichten, die Nutzern praktische Lernreferenzen und Inspiration bieten. Gleichzeitig unterstützt es die Verbindung mit zahlreichen globalen Plattform-Ökosystemen wie YouTube und Instagram und hilft Nutzern dabei, plattformübergreifende Daten- und Servicebarrieren mühelos zu überwinden und automatisierte Multi-Ökosystem-Workflows zu realisieren.

### 1.3.3 Weitere Workflow-Plattformen

Neben den oben genannten bekanntesten Plattformen haben die großen Technologieunternehmen in China jeweils ihre eigenen integrierten KI-Entwicklungsplattformen auf den Markt gebracht: Baidu Qianfan AppBuilder bietet durchgehende Unterstützung von der Modellauswahl über den RAG-Aufbau bis zur Agenten-Veröffentlichung, mit tiefer Integration des ERNIE-Modells; Alibaba Cloud Bailian basiert auf der Tongyi Qianwen-Modellreihe mit Fokus auf Unternehmenssicherheit und private Bereitstellung; Tencent Cloud TI Platform konzentriert sich auf Branchenszenarien wie Finanzwesen und Gesundheitswesen und bietet umfangreiche vorkonfigurierte Lösungsvorlagen. Diese Plattformen sind in der Regel tief in ihr jeweiliges Cloud-Ökosystem integriert und eignen sich für Unternehmen, die bereits im entsprechenden Technologieumfeld tätig sind.

In Bezug auf Universalität, Offenheit und Community-Ökosystem bleiben Dify und Coze jedoch aufgrund ihrer herausragenden Benutzerfreundlichkeit, breiten Modellunterstützung und aktiven Entwickler-Communitys die derzeit am weitesten verbreiteten Wahlmöglichkeiten.

Obwohl sich die Plattformen in ihrer Positionierung und ihrem Ökosystem unterscheiden, ist ihre Kernlogik stets die visuelle Orchestrierung und Verbindung verschiedener Fähigkeitsmodule. Wer daher das Designkonzept und die Bedienung einer dieser Plattformen beherrscht, verfügt über die Grundlage, um schnell zu ähnlichen Werkzeugen zu wechseln. In der folgenden Praxis werden wir Dify als konkretes Beispiel verwenden.

# 2. Dify im Detail

## 2.1 Was ist Dify

Wir haben bereits grundlegende Informationen zu Dify kennengelernt. Für weitere Details kannst du über [https://cloud.dify.ai/apps](https://cloud.dify.ai/apps) auf die Dify-Plattform zugreifen. Wenn du mehr erfahren möchtest, besuche die offizielle Website unter https://dify.ai.

Dify ist eine Open-Source-Plattform zur Entwicklung von LLM-Anwendungen. Sie bietet eine intuitive Oberfläche, die Agenten-Workflows, RAG-Pipelines, Werkzeugfähigkeiten, Modellverwaltung, Observabilität und weitere Funktionen vereint und dir hilft, schnell vom Prototyp in die Produktionsumgebung zu gelangen.

![](images/image11.png)

In Dify kannst du Große Sprachmodelle und verschiedene Werkzeuge nutzen, um „Workflows" zu erstellen. Ein Workflow verknüpft Schritte, die du sonst manuell ausführen müsstest – wie Datenabfrage, LLM-Aufrufe, Websuche, Ergebnisfilterung, Formatierung usw. – gemäß einer Geschäftslogik zu einem automatisierten, wiederverwendbaren Prozess. Ohne Workflows müsstest du bei jedem Durchlauf dieselben Inhalte per Copy & Paste an das große Modell übergeben – ineffizient, fehleranfällig und in realen Geschäftsabläufen kaum reproduzierbar.

Die Erstellung eines Workflows ist wie das Zusammenbauen von Bausteinen oder Puzzleteilen. Du verbindest „LLM-Knoten" (für das Verstehen und Generieren), verschiedene „Werkzeugknoten" (für konkrete Aktionen wie Datenbankabfragen, E-Mail-Versand, Textübersetzung usw.) sowie „Datenknoten" (für das Lesen und Speichern von Informationen) wie Bausteine miteinander. Sie arbeiten automatisch nach der von dir vorgegebenen Logik zusammen, ohne dass du bei jedem Durchlauf manuell eingreifen musst. Du kannst es dir auch als eine Art „Low-Code-Programm" vorstellen: Du musst lediglich per Drag-and-Drop die Ein- und Ausgabepfade konfigurieren, um relativ komplexe Geschäftslogik umzusetzen.

Ein Beispiel: Wenn du ein Amazon- oder Douyin-E-Commerce-Shopbesitzer bist und ein KI-Kundenservice-System aufbauen möchtest, kannst du einen Workflow nach der folgenden Struktur entwerfen:

1. Trigger-Knoten (ähnlich START): Empfängt die Kundenanfrage, z. B. „Wie lange ist die Garantiezeit für dieses Produkt?".
2. Fragenklassifikations-Knoten (ähnlich QUESTION CLASSIFIER): Verwendet ein Modell (z. B. GPT), um die Nutzerfrage zu klassifizieren und zu bestimmen, ob es sich um After-Sales (z. B. Garantie), Nutzungshinweise oder eine andere Kategorie handelt.
3. Wissensabfrage-Knoten (ähnlich KNOWLEDGE RETRIEVAL): Greift basierend auf der Klassifikation automatisch auf die entsprechende Wissensdatenbank zu. Wenn es sich um eine Garantiefrage handelt, werden die präzisen Informationen zum Thema „Garantie" aus der After-Sales-SOP-Wissensdatenbank abgerufen.
4. LLM-Knoten (LLM Node): Sendet die Nutzerfrage zusammen mit den abgerufenen Wissensdatenbank-Inhalten an das Große Sprachmodell (z. B. GPT), das eine nutzerfreundliche Antwort generiert (ohne zu technischen Jargon).
5. Bedingungsknoten: Prüft, ob die vom großen Modell generierte Antwort eine klare Garantiezeit (z. B. „1 Jahr", „3 Jahre") enthält. Wenn ja, wird der nächste Schritt ausgeführt; wenn nicht, wird „Bitte geben Sie die Produktmodellnummer an" ausgegeben.
6. Ausgabeknoten (ähnlich ANSWER): Gibt die endgültige Antwort an den Nutzer zurück und protokolliert automatisch die Beratungsaufzeichnung in einer Tabelle.

![](images/image12.png)

Während des gesamten Prozesses musst du nicht manuell die Wissensdatenbank durchsuchen, die Modellantworten wiederholt anpassen oder Daten separat erfassen – der Workflow verknüpft diese Schritte und „läuft automatisch ab". Und er ist sehr flexibel: Wenn du beispielsweise später eine neue Regel hinzufügen möchtest („Wenn der Nutzer nach dem Garantieumfang fragt, eine andere Wissensdatenbank abfragen"), fügst du einfach einen weiteren Bedingungsknoten im Workflow hinzu, ohne das gesamte System neu zu strukturieren.

Dies ist ein relativ einfaches Workflow-Beispiel, aber diese Fähigkeiten vollständig zu beherrschen, kann für dich momentan noch eine Herausforderung darstellen. Deshalb beginnen wir in dieser Lektion mit einem grundlegenderen Wissensdatenbank-Agenten und werden später schrittweise komplexere Workflow-Techniken erlernen.

### 2.1.1 Bereitstellung einer eigenen Dify-Instanz (optional)

Dieser Inhalt war ursprünglich für eine spätere Lektion geplant. Da jedoch einige Lernende möglicherweise aufgrund von Netzwerkbeschränkungen vorübergehend nicht auf die offizielle Dify-Website oder den Cloud-Dienst zugreifen können, bieten wir diesen optionalen Lernpfad vorab an, damit du deinen Lernfortschritt reibungslos fortsetzen kannst.

Du musst dieses Tutorial als Einführung in die grundlegende Verwendung von Web-Bereitstellungsplattformen heranziehen: [Wie man Web-Anwendungen bereitstellt](/de-de/stage-2/backend/zeabur-deployment/)

![](images/image13.png)

Du lernst, wie man eine eigene Dify-Instanz auf Zeabur bereitstellt. Nach der Bereitstellung registrierst und meldest du dich unter dem entsprechenden Link an und folgst dann dem nachfolgenden Tutorial.

Beachte, dass verschiedene Dify-Versionen leichte Unterschiede in der Bedienung und der Benutzeroberfläche aufweisen können, die Gesamtstruktur jedoch ähnlich bleibt. Wenn du Unterschiede feststellst, keine Panik – suche einfach nach den entsprechenden Schnittstellen und Einstiegspunkten, um fortzufahren.

## 2.2 Erstellen der ersten Dify-Chatbot-Anwendung

Nach dem Aufruf der Dify-Startseite [https://cloud.dify.ai/apps](https://cloud.dify.ai/apps) und der Registrierung/Anmeldung wählst du Studio und siehst die folgende Oberfläche:

![](images/image14.png)

Finde den Bereich `CREATE APP` auf der linken Seite und klicke auf `Create from Blank`.

![](images/image15.png)

![](images/image16.png)

Finde unter APP Type den Eintrag Chatbot (falls er anfangs nicht sichtbar ist, klicke auf die Schaltfläche „Weitere Typen anzeigen" und suche in der vollständigen Liste). Wähle Chatbot aus, gib darunter den Namen und die Beschreibung der Anwendung ein und klicke abschließend auf Erstellen.

![](images/image17.png)

Nach der Erstellung siehst du eine Oberfläche wie die folgende.

![](images/image18.png)

Der Bereich „INSTRUCTIONS" in der Mitte bezeichnet die integrierten Anweisungen, die du als Standard-Prompt oder System-Prompt verstehen kannst.

Darunter befindet sich der Bereich „Knowledge" – hier werden wir später unsere Wissensdatenbank hochladen.

Rechts befindet sich das Debug-Fenster, in dem du nach dem Anpassen der Prompts mit dem Agenten sprechen und die Ergebnisse in Echtzeit überprüfen kannst.

Du kannst im INSTRUCTIONS-Bereich frei Rollen-Prompts eingeben und die Auswirkungen auf den Dialog beobachten; oder du klickst auf Generate, um das große Modell automatisch einen Prompt für dich erstellen zu lassen.

![](images/image19.png)

Beachte, dass oben rechts verschiedene Modelloptionen angezeigt werden. Du kannst zwischen verschiedenen Dialogmodellen wechseln und ihre Unterschiede in Tonalität, logischer Schlussfolgerung und Langtextverarbeitung vergleichen, um das für deine Anforderungen passende Modell zu finden.

![](images/image20.png)

## 2.3 Unterstützung für benutzerdefinierte Modellanbieter

Um die Flexibilität von Dify voll auszuschöpfen und den unterschiedlichen Schwierigkeiten beim Modellzugriff in verschiedenen Regionen Rechnung zu tragen, sowie um spezifische Geschäftsanforderungen, Kostenkontrolle oder Datenschutzvorgaben zu erfüllen, müssen wir häufig benutzerdefinierte Modelle einbinden. Dify unterstützt die Konfiguration von drei Kernmodelltypen: Große Sprachmodelle (LLMs), Embedding-Modelle und Rerank-Modelle. Dieser Abschnitt führt dich schrittweise durch diese benutzerdefinierten Konfigurationen.

Dify kann flexibel Modelle von Mainstream-Anbietern wie OpenAI, Azure und Anthropic einbinden und ist gleichzeitig vollständig kompatibel mit jedem selbst gehosteten oder Drittanbieter-Modell, das der OpenAI-API-Schnittstellenspezifikation entspricht. Du kannst dies durch die Installation des integrierten OpenAI-Compatible-Plugins sowie plattformspezifischer Plugins für die großen Modellplattformen erreichen.

Die detaillierten Schritte sind wie folgt. Zuerst müssen wir die entsprechenden Plugins installieren:

1. Wir müssen die Plugins `OpenAI-API-compatible` und `SiliconFlow` installieren, um Unterstützung für die meisten großen Modelle und Embedding-Modelle zu erhalten. Ersteres bietet Unterstützung für OpenAI-kompatible Schnittstellen, letzteres ist ein Dienst, der die meisten gängigen und leistungsstarken Open-Source-Modelle bereitstellt. Du kannst die Installation über die folgenden Webseiten durchführen:
   1. https://marketplace.dify.ai/plugins/langgenius/openai_api_compatible
   2. https://marketplace.dify.ai/plugins/langgenius/siliconflow
2. Wenn du Dify selbst bereitgestellt hast, kannst du über die entsprechenden Systemeinstellungen in den Plugin-Markt navigieren und die Operationen dort durchführen.

![](images/image21.png)

![](images/image22.png)

Nach dem Aufrufen des Plugin-Markts suche einfach nach dem entsprechenden Plugin-Namen.

![](images/image23.png)

3. Nach Abschluss der Installation können wir neue Modellanbieter konfigurieren. Im Bereich der Modellanbieter in den Einstellungen sehen wir alle aktuell unterstützten Modellanbieter:
   ![](images/image24.png)
4. Vor der ersten Verwendung muss die Modellkonfiguration abgeschlossen werden. Für das OpenAI-API-compatible-Plugin kannst du auf „Add Model" klicken, um ein beliebiges Modell hinzuzufügen und zu konfigurieren. Unter „Model Type" wählst du aus, ob es sich um ein LLM oder ein Embedding-Modell handelt. Du musst sicherstellen, dass der Modelltyp korrekt konfiguriert ist.
   Du musst den genauen Modellnamen, die Modell-Endpunkt-URL und den API-Schlüssel eingeben, um die Modellaktivierung sicherzustellen. Wenn dir die Konfiguration dieser Parameter zunächst zu aufwändig erscheint, kannst du direkt zur SiliconFlow-Plattform-Konfiguration springen oder das OpenRouter-Plugin eines Drittanbieters für eine vereinfachte Modellkonfiguration installieren. (Stelle sicher, dass beim Anbieter nutzbares Guthaben vorhanden ist.)

   ![](images/image25.png)

   Für das `SiliconFlow`-Plugin genügt es, nach dem Klick auf Setup den Schlüssel zu konfigurieren, und du kannst Embedding- und Rerank-Modelle für Tests verwenden. Du kannst auf „Get your API Key from SiliconFlow" klicken, um den Authentifizierungsschlüssel zu erhalten.

   ![](images/image26.png)

5. Nach Abschluss der Konfiguration kannst du auf die Modellliste klicken, um zu sehen, wie viele Modelle aktuell unterstützt werden. Damit ist die Grundkonfiguration aller Basis-Modelle abgeschlossen.
   ![](images/image27.png)

   Darunter werden die meisten gängigen Embedding- und Rerank-Modelle unterstützt:

   ![](images/image28.png)

   Wenn du die Standardmodellkonfiguration von Dify ändern möchtest, kannst du auch auf die Schaltfläche „System Model Settings" klicken, um alle Standardmodelle zu bearbeiten.

   ![](images/image29.png)

## 2.4 Erstellen der ersten Dify-Wissensdatenbank

Damit haben wir die Erstellung des einfachsten Agenten abgeschlossen, aber es fehlt noch eine Wissensdatenbank. Klicke nun im oberen Menü auf `Knowledge`, um zur Seite zur Erstellung einer Wissensdatenbank zu gelangen.

![](images/image30.png)

Klicke dann auf `Create Knowledge` auf der linken Seite, um deine erste Wissensdatenbank zu erstellen.

![](images/image31.png)

In dieser Oberfläche kannst du verschiedene Dateitypen (z. B. PDF, TXT usw.) hochladen, um die Wissensdatenbank aufzubauen. Du kannst lange Texte hochladen oder Inhalte von Wikipedia kopieren und als TXT-Datei speichern. In diesem Beispiel laden wir eine TXT-Datei über Elon Musk aus Wikipedia hoch.

Nach dem Klick auf Next gelangst du zur Seite „Knowledge Base Settings" (Wissensdatenbank-Einstellungen). Hier gibt es viele Optionen, die wir uns Schritt für Schritt ansehen.

Zuerst im Bereich **General**: Du kannst dir diesen Bereich als die Einstellungen für die „Textsegmentierungsregeln" vorstellen. Da wir lange Texte in kleine Stücke aufteilen müssen, müssen zunächst die Segmentierungsregeln definiert werden. In der Einführungsphase musst du nur auf **maximum chunk length (maximale Segmentlänge)** achten. Probiere Werte wie 512, 2048 oder 4096 aus und klicke auf **Preview Chunk**, um die Auswirkungen der verschiedenen Einstellungen zu überprüfen.

Du kannst auch die Option **Chunk overlap (Segmentüberlappung)** anpassen. Sie bestimmt, ob zwischen benachbarten Fragmenten ein gewisser überlappender Inhalt beibehalten wird. Eine angemessene Überlappung hilft zu verhindern, dass wichtige Informationen auf verschiedene Fragmente aufgeteilt werden und dadurch schwer verständlich sind.

![](images/image32.png)

In den Einstellungen gibt es auch eine Option namens **Chunk using Q&A format in English**. Wenn aktiviert, verwendet das System ein großes Sprachmodell, um einen Teil des Wissensdatenbank-Inhalts in ein Frage-Antwort-Format umzuwandeln und zu speichern, was in bestimmten Szenarien die Suchergebnisse deutlich verbessern kann.

In realen Geschäftsanwendungen trägt die Auswahl einer geeigneten Segmentierungsstrategie je nach Szenario dazu bei, die Suchergebnisse zu optimieren und sicherzustellen, dass Abfragen die erwarteten Informationen zurückgeben.

Scrolle weiter nach unten, um die Einstellungen zum Embedding-Modell zu sehen.

Kurz erklärt: Die Kernfunktion eines Embedding-Modells besteht darin, unstrukturierte Daten (z. B. Texte, Bilder usw.) in „numerische Vektoren" (Embedding-Vektoren) umzuwandeln, die ein Computer verarbeiten kann. Durch diese Umwandlung kann das Modell schnell die Ähnlichkeit zwischen verschiedenen Daten berechnen und so semantisch ähnliche Inhalte matchen – beispielsweise anhand eines Benutzersatzes das Dokument, Bild oder Produkt finden, das semantisch am nächsten ist.

Die Wahl des Embedding-Modells hat erheblichen Einfluss auf das Endergebnis der Suche (z. B. Matching-Genauigkeit, Antwortgeschwindigkeit usw.). Wir empfehlen hier vorrangig das Qwen 0.6B Embedding-Modell; du kannst auch auf die 4B- oder 8B-Version wechseln, um die Unterschiede in der Suchqualität bei verschiedenen Parametergrößen direkt zu vergleichen.

![](images/image33.png)

Hier siehst du auch eine weitere Modelleinstellung namens **Rerank model**, deren Standardwert **Jina-rerank-m0** ist. (Wenn du kein Campus-Student bist, wird möglicherweise ein Fehler wegen eines fehlenden Rerank-Modells angezeigt. Du musst dann ein Rerank-Modell konfigurieren, bevor du es hier verwenden kannst.)

Die Hauptfunktion des Rerank-Modells besteht darin, eine zweite, feinere Sortierung der „anfänglich ausgewählten Kandidatenergebnisse" durchzuführen, sodass die Ergebnisse, die am besten zu den Nutzeranforderungen passen, weiter oben erscheinen, was die Relevanz der Endergebnisse und die Nutzererfahrung deutlich verbessert.

Einfach ausgedrückt: Das Rerank-Modell löst das Problem, dass die „erste Auswahl nicht fein genug ist". Eine Suchmaschine könnte beispielsweise zunächst mit einfacheren Regeln 1000 potenziell relevante Webseiten ermitteln und dann durch das Rerank-Modell die 10 relevantesten auswählen und auf der ersten Seite anzeigen.

Gleiches gilt für Empfehlungssysteme: Sie finden möglicherweise zunächst 500 „möglicherweise passende" Produkte und sortieren diese dann mit dem Rerank-Modell so, dass die Produkte, die du am wahrscheinlichsten kaufen wirst, ganz oben in der Liste stehen.

![](images/image34.png)

Wenn alle Einstellungen abgeschlossen sind, klicke auf **Save & Process**. Das System tritt in die Vektorisierungsphase der Wissensdatenbank ein. In dieser Phase wandelt das Embedding-Modell die segmentierten Texte in Vektordarstellungen um.

![](images/image35.png)

Nach Abschluss der Verarbeitung klicke auf **Go to document**, um die bereits verarbeiteten und gespeicherten Inhalte der Wissensdatenbank anzuzeigen.

![](images/image36.png)

Klicke direkt auf den Namen der Wissensdatenbank, um den konkreten Inhalt jedes Segments anzuzeigen.

![](images/image37.png)

Hier kannst du jedes unpassende Textfragment präzise bearbeiten oder löschen.

![](images/image38.png)

In der linken Seitenleiste wähle **Retrieval Testing**, um einen Abruftest der Wissensdatenbank durchzuführen und zu prüfen, ob die Suche ordnungsgemäß funktioniert. Jeder Test gibt mehrere Segmente mit der höchsten Ähnlichkeit zurück.

![](images/image39.png)

Wenn du mehr Segmente sehen möchtest, klicke auf die Einstellungen unter `VECTOR SEARCH`:

![](images/image40.png)

![](images/image41.png)

Top K bezeichnet die Anzahl der Textsegmente, die bei der Vektorsuche zurückgegeben werden und dem Abfragevektor am ähnlichsten sind. Bei der aktuellen Einstellung von 3 werden die 3 ähnlichsten Textsegmente zurückgegeben.

Score Threshold ist ein „Schwellenwert": Nur Textfragmente mit einem Ähnlichkeitsergebnis größer oder gleich diesem Schwellenwert (im Beispiel 0,5) werden zurückgegeben. Dadurch werden weniger relevante Inhalte herausgefiltert und die Ergebnisse werden präziser.

Damit ist die Wissensdatenbank vollständig vorbereitet. Klicke nun in der oberen Menüleiste auf „Studio", finde den zuvor erstellten Agenten und verknüpfe ihn mit der konfigurierten Wissensdatenbank.

![](images/image42.png)

![](images/image43.png)

In jedem Dialog siehst du nun in den Antworten die Quellen der getroffenen Wissensdatenbankeinträge. Klicke auf den entsprechenden Eintrag, um das konkret abgerufene Textfragment anzuzeigen.

![](images/image44.png)

![](images/image45.png)

## 2.5 Weitere gängige Dify-Operationen

Nachdem wir die Grundlagen der Chatbot- und Wissensdatenbank-Erstellung beherrschen, können wir uns mit weiteren Verwendungsweisen von Dify vertraut machen.

### 2.5.1 Import und Export von Workflows

Erinnerst du dich an die zuvor erwähnte Zwischendarstellung von Workflows? Dify unterstützt den Import und Export von Workflows im DSL-Format (Domain Specific Language). DSL ist ein auf JSON basierendes Standardformat, das die Knotenstruktur, Verbindungsbeziehungen und Konfigurationsparameter eines Workflows vollständig abbildet. Du kannst DSL-Dateien einfach importieren und exportieren, Workflows mit anderen teilen oder fremde Workflows als Referenz importieren. Konkret finden wir die Import-Schaltfläche des Workflows leicht auf der Arbeitsflächen-Seite:

![](images/image46.png)

Für den Export eines Workflows klicken wir einfach auf die rechte untere Ecke des einzelnen Workflow-Blocks, um die Export-Schaltfläche zu finden:

![](images/image47.png)

Mit DSL-Dateien kannst du komplexe Workflow-Designs problemlos zwischen verschiedenen Dify-Instanzen migrieren oder teilen.

### 2.5.2 Weitere Dify-Projekte entdecken

Wenn du findest, dass deine selbst erstellten Workflows oder Agenten zu einfach sind, bietet die Dify-Plattform umfangreiche Beispielprojekte, die dir helfen, schnell zu verstehen, wie man komplexe Anwendungen erstellt. Diese Beispielprojekte decken verschiedene Geschäftsszenarien ab. Du kannst auf Explore klicken, um die von anderen erstellten Workflows zu sehen und daraus zu lernen.