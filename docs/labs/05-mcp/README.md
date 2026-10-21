# Tworzenie agentów AI w .NET

## Laboratorium 05 — MCP

### Jak udostępniać narzędzia niezależnie od konkretnej aplikacji?

---

**Marcin Sulecki**  
marcin.sulecki@pw.edu.pl

Na tym laboratorium poznamy:

1. **Serwer MCP** — jak udostępniać narzędzia przez protokół MCP?
2. **MCP Inspector** — jak odkrywać i testować narzędzia serwera?
3. **Klient MCP** — jak odkryte narzędzia udostępnić modelowi językowemu?

Na [Laboratorium 04](../04-tool-calling/README.md) model korzystał z narzędzi zdefiniowanych
bezpośrednio w aplikacji. Teraz udostępnimy te same operacje na urządzeniu Shelly przez **Model Context Protocol (MCP)**.

### Scenariusz

Ponownie wykorzystamy inteligentne gniazdko **Shelly Plug S Gen3** z podłączoną lampką
oraz jego API HTTP poznane na [Laboratorium 04](../04-tool-calling/README.md#dla-zainteresowanych).

Operacje sterowania lampką udostępni serwer MCP. Sprawdzimy je najpierw przez MCP Inspector,
a następnie przez klienta MCP, z którego korzysta model językowy:

> Jaką moc pobiera teraz lampka?

Do wykonania ćwiczeń nie jest wymagane fizyczne urządzenie Shelly — można korzystać z symulatora.

### Przykłady

Kod źródłowy znajduje się w osobnym repozytorium:

**[Shelly — od HTTP do agentów AI](https://github.com/sulmar/shelly)**

| Projekt | Temat | Opis |
|---|---|---|
| [00-Shelly.Simulator](https://github.com/sulmar/shelly/tree/main/00-Shelly.Simulator) | Symulator | Symuluje urządzenie Shelly i udostępnia jego API HTTP. |
| [03-Shelly.Mcp](https://github.com/sulmar/shelly/tree/main/03-Shelly.Mcp) | MCP Server | Udostępnia narzędzia przez protokół MCP. |
| [04-Shelly.McpClient](https://github.com/sulmar/shelly/tree/main/04-Shelly.McpClient) | MCP Client | Odkrywa narzędzia serwera MCP i udostępnia je modelowi. |

### Eksperymenty

| Eksperyment | Temat | Forma |
|---|---|---|
| EXP-00 | MCP — Inspector, serwer i klient | Zadanie programistyczne |
| EXP-01 | Dla chętnych — `Switch.Toggle` w MCP | Zadanie programistyczne |

**[Instrukcje eksperymentów →](./experiments.md)**

### Przygotowanie środowiska

Do wykonania ćwiczeń potrzebne są:

- .NET SDK
- [Ollama](https://ollama.com/download) z modelem obsługującym Tool Calling (np. `qwen3:8b`)
- Node.js i npm — do uruchomienia MCP Inspector
- repozytorium [Shelly](https://github.com/sulmar/shelly) i uruchomiony symulator

#### 1. Przygotuj model, repozytorium i symulator

Model, repozytorium i symulator przygotuj zgodnie z
[Laboratorium 04 — Przygotowanie środowiska](../04-tool-calling/README.md#przygotowanie-środowiska).

#### 2. Uruchom MCP Inspector

```bash
npx @modelcontextprotocol/inspector
```

Połącz Inspector z serwerem `03-Shelly.Mcp`, używając transportu obsługiwanego przez serwer.

#### 3. Uruchom serwer i klienta MCP

Przykłady `03-Shelly.Mcp` i `04-Shelly.McpClient` uruchamiaj zgodnie z instrukcjami w ich katalogach.

### Najważniejsze pojęcia

| Pojęcie | Znaczenie |
|---|---|
| MCP | Protokół udostępniania narzędzi i innych zasobów klientom |
| MCP Server | Udostępnia narzędzia przez protokół MCP |
| MCP Client | Odkrywa narzędzia serwera MCP i udostępnia je modelowi |
| MCP Inspector | Narzędzie do testowania serwerów MCP |

> **Model wybiera narzędzie, ale to aplikacja odpowiada za jego wykonanie.**

### Wykład

Teoria: [Wykład 03 — Tool Calling i MCP](../../lectures/03-tool-calling/README.md)
