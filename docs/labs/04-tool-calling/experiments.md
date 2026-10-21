# Tworzenie agentów AI w .NET

## Laboratorium 04 — Eksperymenty

### Tool Calling i MCP

---

**Marcin Sulecki**  
marcin.sulecki@pw.edu.pl

Eksperymenty wykonujemy na przykładach z repozytorium [Shelly](https://github.com/sulmar/shelly).

Przed rozpoczęciem przygotuj środowisko zgodnie z [README.md](./README.md).

## Eksperymenty

| Eksperyment | Temat | Forma |
|---|---|---|
| EXP-00 | HTTP API | Ćwiczenie |
| EXP-01 | Tool Calling — odczyt stanu lampki | Zadanie programistyczne |
| EXP-02 | MCP — Inspector, serwer i klient | Zadanie programistyczne |
| EXP-03 | Dla chętnych — `Switch.Toggle` | Zadanie programistyczne |

---

## EXP-00 — HTTP API

**Cel:** poznanie komunikacji z urządzeniem bez udziału modelu językowego.

Uruchom symulator Shelly oraz aplikację `01-Shelly.Http`.

Sprawdź, w jaki sposób aplikacja wykorzystuje `HttpClient` do sterowania urządzeniem.

### Zadanie

Wykonaj następujące operacje:

1. Włącz lampkę.
2. Odczytaj jej stan.
3. Wyłącz lampkę.

**Pytanie:** co zmieni się w aplikacji, kiedy zamiast użytkownika wybierającego operację decyzję będzie podejmował model językowy?

---

## EXP-01 — Tool Calling

**Cel:** rozszerzenie możliwości modelu przez dodanie nowego narzędzia.

Uruchom aplikację `02-Shelly.ToolCalling`.

Sprawdź działanie poleceń:

> Włącz lampkę.

> Wyłącz lampkę.

### Zadanie

Rozszerz aplikację tak, aby model potrafił odpowiedzieć na pytania:

> Czy lampka jest teraz włączona?

> Ile watów pobiera aktualnie lampka?

**Wymagania:**

- Dodaj narzędzie odczytujące stan urządzenia.
- Udostępnij je modelowi przez Tool Calling.
- Sprawdź, czy model korzysta z narzędzia, gdy potrzebuje aktualnych danych.
- Zweryfikuj odpowiedź modelu na podstawie rzeczywistego stanu urządzenia.

**Pytanie:** dlaczego model nie powinien odpowiadać na pytania o aktualny stan urządzenia bez jego odczytania?

---

## EXP-02 — MCP

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

## EXP-03 — Dla chętnych: rozszerzenie możliwości Shelly

W [dokumentacji Shelly](https://shelly-api-docs.shelly.cloud/gen2/ComponentsAndServices/Switch/#switchtoggle) znajdziesz metodę `Switch.Toggle`, która
przełącza aktualny stan gniazdka.

Przykładowe wywołanie:

http://192.168.33.1/rpc/Switch.Toggle?id=0

**Zadanie:** dodaj obsługę tej operacji do kolejnych aplikacji.

1. **HTTP API** — rozszerz `00-Shelly.Simulator` o metodę
   `Switch.Toggle` i sprawdź jej działanie.

2. **Tool Calling** — rozszerz `02-Shelly.ToolCalling`, aby model
   mógł wykonać polecenie:

   > Przełącz lampkę.

3. **MCP** — dodaj odpowiednie narzędzie do `03-Shelly.Mcp`.
   Sprawdź je najpierw przez MCP Inspector, a następnie
   przez `04-Shelly.McpClient`.

**Dokumentacja:**

- [Shelly Plug S Gen3](https://shelly-api-docs.shelly.cloud/gen2/Devices/Gen3/ShellyPlugSG3)
- [Switch API](https://shelly-api-docs.shelly.cloud/gen2/ComponentsAndServices/Switch)

### Pytania

1. Czy dodanie metody do API automatycznie udostępnia ją modelowi?
2. Czym różni się udostępnienie nowej operacji przez Tool Calling i MCP?
3. Czy po dodaniu narzędzia do serwera MCP trzeba zmieniać kod klienta?
4. Czy model może zrealizować polecenie „Przełącz lampkę” bez
   dedykowanego narzędzia `Switch.Toggle`?

---

## Podsumowanie

Po wykonaniu eksperymentów powinieneś rozumieć:

- jak model korzysta z narzędzi udostępnionych przez aplikację,
- jak rozszerzać możliwości modelu przez Tool Calling,
- jak serwer MCP udostępnia narzędzia różnym klientom.

Na kolejnym laboratorium: [Laboratorium 06 — Skills](../06-skills/readme.md).

> **Nie pytamy tylko, czy model potrafi wykonać zadanie. Pytamy również, jakie możliwości musimy mu udostępnić, aby mógł je wykonać.**