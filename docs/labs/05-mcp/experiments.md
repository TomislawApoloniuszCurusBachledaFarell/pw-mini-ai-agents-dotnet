# Tworzenie agentów AI w .NET

## Laboratorium 05 — Eksperymenty

### MCP

---

**Marcin Sulecki**  
marcin.sulecki@pw.edu.pl

Eksperymenty wykonujemy na przykładach z repozytorium [Shelly](https://github.com/sulmar/shelly).

Przed rozpoczęciem przygotuj środowisko zgodnie z [README.md](./README.md).

## Eksperymenty

| Eksperyment | Temat | Forma |
|---|---|---|
| EXP-00 | MCP — Inspector, serwer i klient | Zadanie programistyczne |
| EXP-01 | Dla chętnych — `Switch.Toggle` w MCP | Zadanie programistyczne |

---

## EXP-00 — MCP

**Cel:** poznanie serwera MCP, odkrywania narzędzi i ich wykorzystania przez klienta.

Wykorzystamy:

- `03-Shelly.Mcp` — serwer MCP,
- MCP Inspector — narzędzie do testowania serwerów,
- `04-Shelly.McpClient` — klient MCP.

### Krok 1 — MCP Inspector

Uruchom MCP Inspector:

```bash
npx @modelcontextprotocol/inspector
```

Połącz Inspector z serwerem `03-Shelly.Mcp`, używając transportu obsługiwanego przez serwer.

Sprawdź:

1. Jakie narzędzia udostępnia serwer?
2. Jakie argumenty przyjmują?
3. Jak wyglądają wyniki ich wywołania?

Wywołaj narzędzia włączania i wyłączania lampki.

**Pytanie:** czy do korzystania z MCP potrzebny jest model językowy?

### Krok 2 — Rozszerzenie serwera

### Zadanie

Dodaj do serwera MCP narzędzie umożliwiające odczyt aktualnej mocy lampki.

Następnie:

1. Uruchom ponownie serwer.
2. Połącz się przez MCP Inspector.
3. Sprawdź, czy nowe narzędzie jest widoczne.
4. Wywołaj je i zweryfikuj wynik.

**Pytanie:** skąd MCP Inspector wie, jakie narzędzia i argumenty udostępnia serwer?

### Krok 3 — Klient MCP

Uruchom aplikację `04-Shelly.McpClient`.

Wydaj polecenie:

> Jaką moc pobiera teraz lampka?

Sprawdź, czy klient automatycznie odkrywa dodane narzędzie, a model potrafi z niego skorzystać.

**Pytanie:** czy dodanie narzędzia do serwera MCP wymaga zmiany kodu klienta?

---

## EXP-01 — Dla chętnych: `Switch.Toggle` w MCP

Na [Laboratorium 04](../04-tool-calling/experiments.md#exp-02--dla-chętnych-rozszerzenie-możliwości-shelly)
dodaliśmy metodę `Switch.Toggle` do symulatora oraz do aplikacji Tool Calling.

**Zadanie:** udostępnij tę operację również przez MCP.

1. **MCP** — dodaj odpowiednie narzędzie do `03-Shelly.Mcp`.
   Sprawdź je najpierw przez MCP Inspector, a następnie
   przez `04-Shelly.McpClient`.

**Dokumentacja:**

- [Shelly Plug S Gen3](https://shelly-api-docs.shelly.cloud/gen2/Devices/Gen3/ShellyPlugSG3)
- [Switch API](https://shelly-api-docs.shelly.cloud/gen2/ComponentsAndServices/Switch)

### Pytania

1. Czym różni się udostępnienie nowej operacji przez Tool Calling i MCP?
2. Czy po dodaniu narzędzia do serwera MCP trzeba zmieniać kod klienta?

---

## Podsumowanie

Po wykonaniu eksperymentów powinieneś rozumieć:

- jak serwer MCP udostępnia narzędzia różnym klientom.

Na kolejnym laboratorium: [Laboratorium 06 — Skills](../06-skills/readme.md).

> **Nie pytamy tylko, czy model potrafi wykonać zadanie. Pytamy również, jakie możliwości musimy mu udostępnić, aby mógł je wykonać.**
