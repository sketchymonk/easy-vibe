# Dify-Einfuehrung und Wissensdatenbank-Integration

# Rueckblick auf die letzte Lektion

In den vorherigen Lektionen haben wir in Gruppen die Grundlagen von KI-Programmierung, Prompt-Engineering und KI-Bildgenerierung gelernt.

Um dir beim Rueckblick zu helfen, hier einige Fragen:

1. Was ist KI-Programmierung? Wie verwendet man KI-Programmierwerkzeuge (z. B. [z.ai](http://z.ai)), um eine Webseite zu erstellen?
2. Was sind grosse Sprachmodelle? Was ist Prompt-Engineering und Kontext-Engineering?
3. Wie unterscheiden sich die Modellfaehigkeiten in den drei Richtungen Text, KI-Coding und Bildgenerierung?
4. Was ist eine API? Wie verwendet man [z.ai](http://z.ai) fuer den Zugriff auf Drittanbieter-APIs?

In dieser Lektion gehen wir ueber einfache KI-Text- und Bild-Werkzeuge hinaus und betreten Workflow-Erstellungsplattformen, die naeher an realen Geschaefsablaeufen sind. Vom Chatbot zum KI-Agenten und KI-Workflow, und ueber die API wird er zu einer interaktiven "intelligenten" Roboterseite.

# Was du in dieser Lektion lernen wirst

1. Warum der Uebergang vom Chatbot zum Agenten und zur Workflow-Orchestrierung notwendig ist.
2. Was ist eine Agenten- und Workflow-Entwicklungsplattform und wie man KI-Faehigkeiten standardisiert und orchestrierbar macht.
3. Was ist Dify und wie man mit dieser Open-Source-Plattform fuer LLM-Anwendungen schnell Anwendungen erstellt, insbesondere Wissensdatenbank-Frage-Antwort-Roboter.
4. RAG-Implementierungsmethoden und Wert: Warum braucht man Retrieval-Augmented Generation?
5. Wie man von 0 auf 1 Dify und das KI IDE Trae erlernt.

# 1. Vom Dialog zum Agenten

In der vorherigen Phase haben wir gelernt, wie man Prompts verwendet, um grosse Modelle in Rollen zu versetzen. Aber wenn man genau darueber nachdenkt, stellt man fest: Ein Chatbot allein kann nicht handeln.

Er kann beschreiben, wie man Bestellungen prueft, aber nicht tatsaechlich in der Datenbank nachschlagen. Er kann beschreiben, was ein Wochenbericht enthalten sollte, aber nicht automatisch Projektdaten zusammenfassen und E-Mails senden.

Um die KI vom Chat-Partner zum digitalen Mitarbeiter zu machen, muessen wir ihr drei Kernfaehigkeiten geben:

1. **Exklusives Wissen** - Zugriff auf Produktdokumentation, Kundendaten, interne Richtlinien
2. **Werkzeugaufruf (Plugins)** - Datenbanken bedienen, APIs aufrufen
3. **Strukturierte Ausfuehrung** - Aufgaben nach vorgegebenem Ablaufplan erledigen

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image1.png)

## 1.1 Einfachster Agent: Frage-Antwort-Roboter basierend auf Wissensdatenbank

Die am weitesten verbreitete Form eines einfachen Agenten ist der Wissensdatenbank-Frage-Antwort-Roboter. Sein Durchbruch: Die Antworten des grossen Modells werden nicht mehr frei generiert, sondern basieren auf tatsaechlichen Quellen. Die Loesung dafuer ist RAG (Retrieval-Augmented Generation).

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image2.png)

Bildquelle: [https://www.datacamp.com/blog/what-is-retrieval-augmented-generation-rag](https://www.datacamp.com/blog/what-is-retrieval-augmented-generation-rag)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image3.png)

Bildquelle: [https://www.databricks.com/glossary/retrieval-augmented-generation-rag](https://www.databricks.com/glossary/retrieval-augmented-generation-rag)

## 1.2 Vom Dialog-Agenten zum Workflow

Selbst ein "verstaerkter Agent" mit Wissensdatenbank und Plugin-Aufruf reicht fuer komplexere Geschaefsprozesse nicht aus. Dies fuehrt zum hoeheren KI-Anwendungsparadigma: **KI-Workflow**.

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image4.png)

Ein Workflow zerlegt eine komplexe Aufgabe in mehrere geordnete, konfigurierbare und automatisch ausfuehrbare Teilschritte und orchestriert ihre logischen Beziehungen ueber visuelle oder codebasierte Methoden.

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image5.png)

## 1.3 Haeufige Agenten-/Workflow-Plattformen

| Plattform | Merkmale | Anwendungsbereich |
| --------- | --------- | ----------------- |
| Dify | Open-Source, RAG, LLM-Orchestrierung, API-Ausgabe | Unternehmens-Wissensdatenbank, massgeschneiderte Agenten |
| Coze (ByteDance) | In China verfuegbar, reichhaltige Plugins | Social-Bots, Mini-Programm-Integration |
| n8n | Universelle Automatisierung, API-Orchestrierung | Datensynchronisation, SaaS-Automatisierung |

### 1.3.1 Dify

Dify ist eine LLM-Anwendungsentwicklungs- und Betriebsplattform mit Fokus auf den gesamten Lebenszyklus von KI-Anwendungen.

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image6.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image7.png)

### 1.3.2 Coze (ByteDance)

Coze ist eine KI-Agenten-Entwicklungsplattform von ByteDance mit Fokus auf extreme Benutzerfreundlichkeit.

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image8.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image9.png)

### 1.3.3 n8n

n8n ist eine universelle programmierbare Workflow-Automatisierungsplattform.

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image10.png)

# 2. Dify im Detail

## 2.1 Was ist Dify?

Besuche [https://cloud.dify.ai/apps](https://cloud.dify.ai/apps) oder die Website https://dify.ai.

Dify ist eine Open-Source-Plattform zur Entwicklung von LLM-Anwendungen.

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image11.png)

### 2.1.1 Eigenes Dify bereitstellen (optional)

Du musst das Tutorial zur Web-Bereitstellung konsultieren: [Web-Anwendungen bereitstellen](/de-de/stage-2/backend/zeabur-deployment/)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image13.png)

## 2.2 Erste Dify-Chatbot-Anwendung erstellen

Besuche [https://cloud.dify.ai/apps](https://cloud.dify.ai/apps), registriere dich und melde dich an. Waehle "Studio" und klicke auf "CREATE APP" > "Create from Blank".

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image14.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image15.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image16.png)

Waehle "Chatbot" als App-Typ, gib Name und Beschreibung ein und klicke auf "Erstellen".

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image17.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image18.png)

Im mittleren Bereich "INSTRUCTIONS" kannst du System-Prompts eingeben. Rechts befindet sich das Debug-Fenster.

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image19.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image20.png)

## 2.3 Benutzerdefinierte Modellanbieter konfigurieren

1. Installiere die Plugins `OpenAI-API-compatible` und `SiliconFlow`:
   - https://marketplace.dify.ai/plugins/langgenius/openai_api_compatible
   - https://marketplace.dify.ai/plugins/langgenius/siliconflow

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image21.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image22.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image23.png)

2. Konfiguriere die Modelle:

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image24.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image25.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image26.png)

3. Ueberpruefe die Modellliste:

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image27.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image28.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image29.png)

## 2.4 Erste Dify-Wissensdatenbank erstellen

Klicke oben im Menue auf "Knowledge" und dann auf "Create Knowledge".

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image30.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image31.png)

Lade verschiedene Dateitypen hoch (PDF, TXT usw.), um die Wissensdatenbank zu erstellen.

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image32.png)

Konfiguriere die Embedding- und Rerank-Modelle:

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image33.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image34.png)

Klicke auf "Save & Process":

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image35.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image36.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image37.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image38.png)

Teste die Wissensdatenbank mit "Retrieval Testing":

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image39.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image40.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image41.png)

Verbinde die Wissensdatenbank mit deinem Agenten:

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image42.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image43.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image44.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image45.png)

## 2.5 Weitere Dify-Operationen

### 2.5.1 Workflow-Import und -Export

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image46.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image47.png)

### 2.5.2 Weitere Dify-Projekte ansehen

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image48.png)

## 2.6 Erste Dify-Workflow-Anwendung erstellen

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image49.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image50.png)

Waehle zwischen Chatflow (fuer fortlaufende Gespraeche) und Workflow (fuer Aufgabenautomatisierung).

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image51.png)

### 2.6.1 Haeufige Knoten

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image52.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image53.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image54.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image55.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image56.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image57.png)

### 2.6.2 Haeufige Werkzeuge

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image58.png)

### 2.6.3 Einfachen Intent-Klassifizierungs-Workflow erstellen

Wir erstellen einen Workflow fuer ein Restaurantszenario mit vier Intent-Kategorien:

- **Essen bestellen (buy_food)**: Benutzer moechte etwas bestellen
- **Beschwerde (complain)**: Benutzer aeussert Unzufriedenheit
- **Plaudern (chitchat)**: Allgemeine Fragen und Empfehlungen
- **Sonstiges (other)**: Nicht restaurantbezogene Themen

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image59.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image60.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image61.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image62.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image63.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image64.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image65.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image66.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image67.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image68.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image69.png)

## 2.7 Vorlagen-Workflow ausfuehren

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image70.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image71.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image72.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image73.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image74.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image75.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image76.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image77.png)

## 2.8 Dify als API-Anbieter nutzen

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image78.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image79.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image80.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image81.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image82.png)

## 2.9 Frontend-Chat-Anwendung mit Dify-API erstellen

Klicke auf "Publish" > "Publish Update" > "Access API Reference".

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image83.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image84.png)

Finde "Send Chat Message" und kopiere Request und Response Beispiele.

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image85.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image86.png)

Erstelle einen API-Key:

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image87.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image88.png)

```json
key:
app-zKdCHUXXXXXXXX

Please write me a front-end based on the following reference:

curl -X POST 'http://{DIFY_API_URL}/v1/chat-messages' \
--header 'Authorization: Bearer {api_key}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "inputs": {},
    "query": "What are the specs of the iPhone 13 Pro Max?",
    "response_mode": "streaming",
    "conversation_id": "",
    "user": "abc-123"
}'
```

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image89.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image90.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image91.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image92.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image93.png)

# 3. Weitere Geschaefsworkflow-Referenzen

## 3.1 Social-Media-Workflows

1. Plattformuebergreifende Content-Verbreitung (komplex)
2. Trending-Topic-Auswahl und Entwurfsgenerator (mittel)
3. Intelligente Kommentarklassifikation und Antwort-Assistent (komplex)
4. Kurzvideo-Skript- und Storyboard-Generator (komplex)
5. Live-Interaktion Q&A Echtzeit-Zusammenfassung (mittel)

## 3.2 Arbeitsplatz-Workflows

1. Intelligente Besprechungsprotokolle und automatische Aufgabenverteilung (komplex)
2. Lebenslauf-Stapelscreening und Erstbewertung (mittel)
3. Mehrsprachige E-Mail-Uebersetzung und Entwurfsantwort (einfach)
4. Wochen-/Monatsbericht-Datenaggregation (komplex)
5. Vertrag/Dokument intelligente Pruefung (mittel)

## 3.3 Lern- und Lebensworkflows

1. Akademische Arbeit Analyse und Notizgenerator (komplex)
2. Personalisierter Reiseplanungsassistent (mittel)
3. Fremdsprachen-Lernpraxispartner (einfach)
4. Persoenlicher Wissensdatenbank-Frage-Antwort- und Empfehlungssystem (komplex)
5. Fitness-/Ernaehrungsplan Tracking-Berater (mittel)

# 6. Einschraenkungen von Workflow-Plattformen

Workflow-Plattformen (Low-Code-Plattformen) sind keine Universalloesungen. "Low-Code" ist oft auch eine Art "High-Code" - Benutzer muessen die Konzepte, Regeln und Bedienlogik der Plattform verstehen.

Mit der schnellen Entwicklung von Vibe Coding kann das direkte Lesen oder Generieren von Code mit KI-Unterstützung manchmal effizienter sein.

# Hausaufgabe

## Dify-Grundoperationen meistern

1. Referenziere die Intent-Klassifizierungs-Workflow-Methode und lass die KI ein vollstaendig anderes Szenario vorschlagen.
2. Login-Workflow-Entschluesselungs-Challenge
3. Love-Loop-Workflow-Entschluesselungs-Challenge

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image94.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image95.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image96.png)

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image97.png)

## Dify-API-Aufruf implementieren

1. Stelle Dify bereit und erstelle eine einfache Wissensdatenbank.
2. Verwende Trae IDE, um ein Chat-Frontend zu erstellen, das mit der Dify-Wissensdatenbank ueber API interagiert.
3. Teste Mehrfach-Chat-Ergebnisse.

## Drittanbieter-Workflow ausprobieren

Finde einen Dify-Workflow auf Github, in WeChat oder anderen Plattformen und fuehre ihn erfolgreich aus.

# [Bug] Loesung fuer HTTP-Anfragefehler

![](/zh-cn/stage-2/ai-capabilities/dify-knowledge-base/images/image98.png)

Wenn du dieses Problem hast, verwende Zeabur als Netzwerk-Weiterleitungs-Gateway:

- Originaladresse: `http://{DIFY_API_URL}/v1/chat-messages`
- Neue Adresse: `https://{DIFY_NEW_API_URL}.zeabur.app/v1/chat-messages`

```python
from flask import Flask, request, Response
import requests

app = Flask(__name__)

TARGET_BASE_URL = "{DIFY_API_URL}"
LISTEN_PORT = 8080

@app.route('/', defaults={'path': ''}, methods=['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'OPTIONS', 'HEAD'])
@app.route('/<path:path>', methods=['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'OPTIONS', 'HEAD'])
def proxy_request(path):
    target_url = f"{TARGET_BASE_URL}/{path}"
    if request.query_string:
        target_url += f"?{request.query_string.decode('utf-8')}"

    headers = {key: value for key, value in request.headers if key.lower() not in ['host', 'connection', 'content-length', 'accept-encoding']}

    try:
        resp = requests.request(
            method=request.method,
            url=target_url,
            headers=headers,
            data=request.get_data(),
            cookies=request.cookies,
            allow_redirects=False,
            timeout=30
        )

        excluded_headers = ['content-encoding', 'content-length', 'transfer-encoding', 'connection']
        response_headers = [(name, value) for name, value in resp.raw.headers.items() if name.lower() not in excluded_headers]

        return Response(resp.content, resp.status_code, response_headers)

    except requests.exceptions.RequestException as e:
        print(f"Error forwarding request to {target_url}: {e}")
        return Response(f"Proxy Error: Could not reach target server or invalid response: {e}", status=502)
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
        return Response(f"Internal Proxy Error: {e}", status=500)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=LISTEN_PORT, debug=True)
```
