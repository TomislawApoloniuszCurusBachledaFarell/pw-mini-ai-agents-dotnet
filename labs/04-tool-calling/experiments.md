# Tworzenie agentów AI w .NET

## Laboratorium 04 — Eksperymenty

### Od HTTP API do agentów AI

---

**Marcin Sulecki**  
marcin.sulecki@sulmar.pl

Eksperymenty wykonujemy na przykładach z repozytorium [Shelly](https://github.com/sulmar/shelly).

Przed rozpoczęciem przygotuj środowisko zgodnie z [README.md](./README.md).

## Eksperymenty

| Eksperyment | Temat | Forma |
|---|---|---|
| EXP-00 | HTTP API | Ćwiczenie |
| EXP-01 | Tool Calling — odczyt stanu lampki | Zadanie programistyczne |
| EXP-02 | MCP — Inspector, serwer i klient | Zadanie programistyczne |
| EXP-03 | Agent AI — włączenie lampki na określony czas | Zadanie problemowe i programistyczne |
| EXP-04 | Code Execution — obliczenie kosztu energii | Demonstracja prowadzącego |

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

## EXP-03 — Agent AI

**Cel:** zrozumienie, jak możliwości agenta zależą od dostępnych narzędzi.

Uruchom aplikację `05-Shelly.Agent`.

Sprawdź działanie polecenia:

> Upewnij się, że lampka jest włączona.

Zaobserwuj, czy agent sprawdza aktualny stan urządzenia i weryfikuje rezultat działania.

### Zadanie

Użytkownik chce wydać polecenie:

> Włącz lampkę na 5 sekund, a następnie ją wyłącz.

**Rozszerz aplikację tak, aby agent potrafił samodzielnie zrealizować takie polecenie.**

Nie implementuj na stałe sekwencji włączania i wyłączania lampki.

Agent powinien samodzielnie zdecydować, jakie działania wykonać i w jakiej kolejności.

**Wymagania:**

- Zidentyfikuj ograniczenia obecnego rozwiązania.
- Zaproponuj sposób rozszerzenia możliwości agenta.
- Zaimplementuj rozwiązanie.
- Sprawdź polecenia z różnymi czasami.
- Zweryfikuj końcowy stan lampki.

**Pytanie:** czy do rozwiązania problemu potrzebny jest lepszy model, zmiana promptu czy dodatkowe narzędzie?

---

## EXP-04 — Code Execution (demonstracja)

**Cel:** poznanie mechanizmu generowania i wykonywania kodu przez agenta.

Uruchomimy aplikację `06-Shelly.CodeExecution`.

Użytkownik pyta:

> Ile kosztuje świecenie mojej lampki przez 500 godzin przy cenie 4,50 zł/kWh?

Agent może:

1. Odczytać aktualną moc lampki.
2. Wygenerować kod Pythona obliczający zużycie energii i koszt.
3. Uruchomić kod w kontenerze Docker.
4. Odczytać wynik wykonania.
5. Przedstawić odpowiedź użytkownikowi.

### Pytania do dyskusji

- Dlaczego model zdecydował się wygenerować kod?
- Czy te obliczenia można wykonać bez Pythona?
- Co się stanie, jeśli wygenerowany kod zawiera błąd?
- Czy model potrafi poprawić kod na podstawie komunikatu błędu?
- Jakie zagrożenia wiążą się z wykonywaniem kodu wygenerowanego przez LLM?

> **Wygenerowanie kodu nie oznacza jego wykonania, a poprawne wykonanie nie gwarantuje poprawności wyniku.**

---

## ## EXP-05 Dla chętnych — Rozszerzenie możliwości Shelly

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
- jak serwer MCP udostępnia narzędzia różnym klientom,
- dlaczego agent potrzebuje odpowiednio zaprojektowanego środowiska,
- jakie możliwości i zagrożenia wprowadza Code Execution.

> **Nie pytamy tylko, czy model potrafi wykonać zadanie. Pytamy również, jakie możliwości musimy mu udostępnić, aby mógł je wykonać.**