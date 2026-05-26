# CLI KI-Programmierwerkzeuge

In diesem Tutorial stellen wir KI-Programmier-Agenten vor, die direkt in der Kommandozeile ausgefuehrt werden. Im Gegensatz zu den Agenten in Trae oder Cursor koennen CLI KI-Programmierwerkzeuge nur im Terminal verwendet werden. Im Vergleich zu IDE-integrierten Agenten bieten sie in der Regel ein laengeres Kontextfenster, schnellere Tool-Aufrufe und kompatiblere Modelle.

## Von der CLI ausgehend

CLI (Command Line Interface) bedeutet die Bedienung von Softwareanwendungen ueber reine Textbefehle im Terminal, anstatt sich auf grafische Benutzeroberflaechen (GUI) zu verlassen.

![](/zh-cn/stage-2/backend/modern-cli/images/image1.png)![](/zh-cn/stage-2/backend/modern-cli/images/image2.png)

CLI eignet sich von Natur aus fuer Textbefehlsoperationen und ist bei einem Teil der Power-User sogar beliebter als GUI.

![](/zh-cn/stage-2/backend/modern-cli/images/image3.png)

Keine Sorge, wenn du keine CLI-Erfahrung hast. Genau wie wir Trae frueher fuer grundlegende Operationen genutzt haben, koennen wir hier CLI-Programmierwerkzeuge alle CLI-Operationen fuer uns ausfuehren lassen.

## Unterschiede zu KI-IDEs

Man kann CLI KI-Programmierwerkzeuge mit z.ai oder Trae vergleichen. Sie benoetigen nur einen einfachen Dialogueingang und fuehren automatisch alle erforderlichen Operationen durch.

![](/zh-cn/stage-2/backend/modern-cli/images/image4.png)![](/zh-cn/stage-2/backend/modern-cli/images/image5.png)

| Funktionsmerkmal         | Claude Code    | Cursor          | Besserer      |
| ------------------------ | -------------- | --------------- | ------------- |
| Automatische Aufgabenausfuehrung | Sehr stark | Begrenzt | Claude Code |
| IDE-Integration          | Nur Kommandozeile | Natives VS Code | Cursor |
| Echtzeit-Codevervollstaendigung | Nein | Hervorragend | Cursor |
| Mehrdateioperationen     | Sehr stark     | Gut             | Claude Code |
| GitHub-Integration       | Direkte Commits | Manuelle Operation | Claude Code |
| Lernkurve                | Mittel         | Einfach         | Cursor |
| Kontextlaenge            | Sehr lang      | Gut             | Claude Code |
| Debugging-Unterstützung  | Automatisiert  | Viel manuell    | Claude Code |

Kurz gesagt koennen CLI KI-Programmierwerkzeuge:
- Laengere fortlaufende Gespraeche unterstuetzen
- Laengere Kontextfenster bieten
- Schneller reagieren

## Haeufige CLI KI-Programmierwerkzeuge

Wir empfehlen zwei Hauptkategorien:

- **Codex** nutzt GPT-5 und ist insgesamt leistungsstaerker
- **Claude Code** bietet ueber GLM 4.6 eine API-Weiterleitung mit einer Erfahrung nahe Claude 4, aber guenstiger
- **OpenCode** ermoeglicht freien Modellwechsel und bietet kostenlose Modelle

### Claude Code

Claude Code ist ein von Anthropic entwickeltes KI-Programmierwerkzeug basierend auf dem Claude-Grosssprachenmodell. Seine Hauptinteraktion findet im Terminal statt, unterstuetzt aber auch die Nutzung als VS Code-Plugin.

![](/zh-cn/stage-2/backend/modern-cli/images/image6.png)

**Vorteile**: Sehr langes Kontextfenster, kann unklare Anforderungen klaeren, automatische Aufgabenplanung und tiefes Verstaendnis der gesamten Codebasis.

![](/zh-cn/stage-2/backend/modern-cli/images/image7.png)![](/zh-cn/stage-2/backend/modern-cli/images/image8.png)

![](/zh-cn/stage-2/backend/modern-cli/images/image9.png)![](/zh-cn/stage-2/backend/modern-cli/images/image10.png)

Kurs zum systematischen Lernen von Claude Code: <https://www.bilibili.com/video/BV176t2zSEpr>

![](/zh-cn/stage-2/backend/modern-cli/images/image11.png)

#### GLM als Backend verwenden (Empfohlen)

GLM (General Language Model) ist eine von zhipu AI entwickelte Serie von Grosssprachenmodellen. GLM-4.6 ist die neueste Version mit herausragender Codefaehigkeit.

![](/zh-cn/stage-2/backend/modern-cli/images/image12.png)

![](/zh-cn/stage-2/backend/modern-cli/images/image13.png)

Installation:

```python
# Claude Code installieren
npm install -g @anthropic-ai/claude-code

# In dein Projekt wechseln
cd your-awesome-project

# Claude Code starten
claude
```

API-Key abrufen:
- Inlands-Version: <https://bigmodel.cn/usercenter/proj-mgmt/apikeys>
- International: <https://z.ai/manage-apikey/apikey-list>

**Inlands-Version GLM** konfigurieren:

```python
setx ANTHROPIC_AUTH_TOKEN your_zhipu_api_key
setx ANTHROPIC_BASE_URL https://open.bigmodel.cn/api/anthropic
```

**International GLM** konfigurieren:

```python
setx ANTHROPIC_AUTH_TOKEN your_zai_api_key
setx ANTHROPIC_BASE_URL https://api.z.ai/api/anthropic
```

![](/zh-cn/stage-2/backend/modern-cli/images/image14.png)

#### Kimi K2 als Backend verwenden (Empfohlen)

Kimi K2 ist ein neues Grosssprachenmodell von Moonshot AI mit hervorragender Codeverstaendnis- und Generierungsfaehigkeit. Es unterstuetzt ein Ultra-Lang-Kontextfenster von bis zu 200K Tokens.

API-Key: <https://platform.moonshot.cn/console/account>

```bash
export ANTHROPIC_BASE_URL=https://api.moonshot.cn/anthropic
export ANTHROPIC_AUTH_TOKEN=sk-YOURKEY
```

#### Minimax als Backend verwenden (Empfohlen)

Minimax ist ein Grosssprachenmodell von MiniMax mit hervorragender Reasoning-Faehigkeit und Codegenerierungsqualitaet.

API-Key: <https://platform.minimax.io/>

```bash
export ANTHROPIC_BASE_URL=https://api.minimax.io/anthropic
export ANTHROPIC_AUTH_TOKEN=YOUR_MINIMAX_API_KEY
export ANTHROPIC_MODEL=MiniMax-M2.7
```

#### DeepSeek als Backend verwenden (Empfohlen)

DeepSeek ist ein Open-Source-Grosssprachenmodell mit herausragender Codefaehigkeit und hohem Preis-Leistungs-Verhaeltnis.

API-Key: <https://platform.deepseek.com/usage>

```bash
export ANTHROPIC_BASE_URL=https://api.deepseek.com/anthropic
export ANTHROPIC_AUTH_TOKEN=YOU_DEEPSEEK_API_KEY
export API_TIMEOUT_MS=600000
export ANTHROPIC_MODEL=deepseek-chat
export ANTHROPIC_SMALL_FAST_MODEL=deepseek-chat
export CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1
```

#### Volcengine Coding Plan als Backend verwenden (Empfohlen)

Volcengine (Volcano Engine) ist die Cloud-Service-Plattform von ByteDance.

API-Key: <https://console.volcengine.com/ark/region:ark+cn-beijing/apiKey>

```bash
export ANTHROPIC_BASE_URL=https://ark.volces.com/api/anthropic
export ANTHROPIC_AUTH_TOKEN=YOUR_VOLCANO_API_KEY
export ANTHROPIC_MODEL=doubao-pro-32k
```

#### Claude Code erweiterte Nutzung

| Befehl               | Funktion                                            | Beispiel                                     |
| -------------------- | --------------------------------------------------- | -------------------------------------------- |
| claude               | Interaktiven Modus starten                          | `claude`                                     |
| claude "query"       | Einmalige Aufgabe ausfuehren                        | `claude "explain this project"`              |
| claude -p "query"    | Einmalige Frage mit automatischem Beenden           | `claude -p "explain this function xxxx"`     |
| claude -c            | Letzte Sitzung fortsetzen                           | `claude -c`                                  |
| /init                | Projektbeschreibung mit CLAUDE.md initialisieren   | `/init`                                      |
| /clear               | Aktuelle Sitzung leeren                             | `/clear`                                     |
| /compact             | Sitzungsverlauf komprimieren                        | `/compact`                                   |
| /cost                | Aktuelle Kosten anzeigen                           | `/cost`                                      |
| exit oder Ctrl+C     | Claude Code beenden                                 | `exit` oder `Ctrl+C`                         |

**CLAUDE.md**

`CLAUDE.md` ist eine spezielle Datei, die Claude beim Start automatisch liest. Sie eignet sich fuer:
- Haeufige Bash-Befehle
- Kerndateien und Hilfsfunktionen
- Code-Stilvereinbarungen
- Testmethoden
- Repository-Zusammenarbeitsregeln

```
# Bash commands
- npm run build: Build the project
- npm run typecheck: Run the typechecker

# Code style
- Use ES modules (import/export) syntax, not CommonJS (require)
- Destructure imports when possible

# Workflow
- Be sure to typecheck when you're done making changes
```

#### Interne Funktionsweise von Claude Code

![](/zh-cn/stage-2/backend/modern-cli/images/image25.png)

Claude Code zerlegt Programmieraufgaben in einen fortlaufenden "Wahrnehmen-Denken-Handeln-Verifizieren"-Zyklus. Es ahmt den Workflow menschlicher Entwickler nach: "Code schreiben > ausfuehren > Ergebnisse pruefen > verbessern".

### Codex

![](/zh-cn/stage-2/backend/modern-cli/images/image26.png)

![](/zh-cn/stage-2/backend/modern-cli/images/image27.png)

Codex ist ein von OpenAI entwickeltes KI-Kooperations-Programmierwerkzeug. Sein groesster Vorteil ist die effiziente Anpassung an GPT-5.

Installation:

```
npm i -g @openai/codex
```

#### Offizielle OpenAI API als Backend

![](/zh-cn/stage-2/backend/modern-cli/images/image28.png)

![](/zh-cn/stage-2/backend/modern-cli/images/image29.png)

#### Weitergeleitete OpenAI API als Backend

Konfigurationsbeispiel:

````bash
My API key is: [Paste your obtained sk-xxxxx key here]

Please help me complete the following configuration tasks:

1. Create a `.codex` folder under my user directory
2. Create `config.toml` in the `.codex` directory
3. Write the following content:
   ```toml
   preferred_auth_method = "apikey"

   [model_providers.myrelay]
   name = "My Relay Station"
   base_url = "https://api.zyai.online/v1"
   env_key = "MYRELAY_API_KEY"
   wire_api = "responses"
   ```

4. Set system environment variable MYRELAY_API_KEY
````

Nach der Konfiguration kannst du Codex mit `codex --profile myrelay` starten.

### OpenCode

![](/zh-cn/stage-2/backend/modern-cli/images/image32.png)

OpenCode ist eine Open-Source KI Coding Agent-Plattform, positioniert als "Multi-Modell-Version von Claude Code".

Installation:

```bash
# Linux / Unix
curl -fsSL https://opencode.ai/install | bash

# Windows
npm i -g opencode-ai
```

#### Kostenlose Modelle in OpenCode verwenden

Starte OpenCode mit `opencode` und gib `/models` ein, um nach "free" zu suchen.

![](/zh-cn/stage-2/backend/modern-cli/images/image35.png)

#### Drittanbieter-Modelle verwenden

Gib `/connect` ein, waehle den ersten Eintrag und folge der Authentifizierung.

![](/zh-cn/stage-2/backend/modern-cli/images/image36.png)

![](/zh-cn/stage-2/backend/modern-cli/images/image37.png)

![](/zh-cn/stage-2/backend/modern-cli/images/image38.png)

#### Oh My OpenAgent Plugin installieren

Oh-My-OpenAgent ist ein "Multi-Agent KI-Programmier-Kommandosystem", das auf OpenCode laeuft.

```text
Install and configure oh-my-openagent by following the instructions here:
https://raw.githubusercontent.com/code-yeongyu/oh-my-openagent/refs/heads/dev/docs/guide/installation.md
```

## Weitere Verwendungsmoeglichkeiten von CLI KI-Programmierwerkzeugen

### KI fuer Anforderungen-Dokumente nutzen

Abstrakte Anforderungen muessen fuer grosse Sprachmodelle "konkretisiert" werden. Lass die KI helfen, deine Anforderungen zu detaillieren:

`Based on my needs, please elaborate and provide a more detailed Product Requirement Document for reference.`

### Ordner verwalten

`Please help me organize the contents of the current folder.`

### Neue Projekte entwickeln

Genau wie bei z.ai oder Trae koennen CLI-Tools verwendet werden, um Projekte von Grund auf zu entwickeln.

### Open-Source-Projekte bereitstellen

`I want to deploy this GitHub project https://github.com/langgenius/dify. Please help me clone the project and run it.`

![](/zh-cn/stage-2/backend/modern-cli/images/image31.png)

### Code erklaeren und Dokumentation schreiben

- "Bitte erklaere mir dieses Projekt: Wie startet man es, wie verwendet man es?"
- "Bitte schreibe eine vollstaendige Dokumentation fuer dieses Projekt."

### Weitere Anwendungsmoeglichkeiten

- Lokale Dateien verwalten und organisieren
- Tagebuch schreiben, Zusammenfassungen erstellen
- Systemfehler analysieren und beheben
- Wiederkehrende Kommandozeilen-Aufgaben ausfuehren
