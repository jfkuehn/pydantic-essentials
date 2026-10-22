# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

Educational repository with Jupyter notebooks on Pydantic (v2). The material is organized as a progressive learning path in numbered chapter directories under `src/`; each chapter builds on the previous ones.

## Environment Setup

This project uses **uv** as the package manager (not pip or conda). Python >= 3.14 (see `.python-version` and `pyproject.toml`). Dependencies: `pydantic>=2.14`, `ipykernel`.

```bash
source .venv/bin/activate   # activate the virtual environment
uv sync                     # install dependencies
jupyter notebook            # run notebooks (alternativ: Kernel in PyCharm/VS Code)
```

## Repository Structure

Chapter directories in `src/` (Namen und Reihenfolge folgen dem Original-Kurs, `fbaptiste/pydantic-essentials` auf GitHub). Notebooks heißen `NN - Titel.ipynb`; die Nummerierung beginnt in jedem Kapitel bei `02`. Lokal vorhanden ist bisher das komplette Kapitel `02 - Basics` (Notebooks 02 bis 12) sowie aus `03 - Model Configuration` das Notebook `02 - Handling Extra Fields`; die übrigen Notebooks sind im Original-Kurs vorgesehen:

| Kapitel | Notebooks (Original-Kurs) |
|---|---|
| `02 - Basics` | 02 Creating a Pydantic Model, 03 Deserialization, 04 Serialization, 05 Type Coercion, 06 Required vs Optional Fields, 07 Nullable Fields, 08 Combining Nullable and Optional, 09 Inspecting Fields, 10 JSON Schema Generation, 11 Project, 12 Project Solution |
| `03 - Model Configuration` | 02 Handling Extra Fields, 03 Strict and Lax Type Coercion, 04 Validating Default Values, 05 Validating Assignments, 06 Mutability, 07 Coercing Numbers to Strings, 08 Standardizing Strings, 09 Handling Python Enums, 10 Project, 11 Project Solution |
| `04 - Field Aliasing, Serialization and Deserialization` | 02 Field Aliases and Default Values, 03 Alias Generator Functions, 04 Deserializing by Field Name or Alias, 05 Serialization Aliases, 06 Validation Aliases, 07 Custom Serializers, 08 Project, 09 Project Solution |
| `05 - Specialized Pydantic Types` | 01 Pydantic Types Documentation Links, 02 PositiveInt, 03 Constrained Lists, 04 UUID, 05 Date Related Types, 06 Network Types, 07 Project, 08 Project Solution |
| `06 - Additional Field Features` | 02 Numerical Constraints, 03 String Constraints, 04 Default Factories, 05 Additional Field Configurations, 06 Project, 07 Project Solution |
| `07 - Annotated Types` | 02 Pydantic and Annotated Types, 03 Annotated Types and Type Variables, 04 String Constraints, 05 Project, 06 Project Solution |
| `08 - Custom Validators` | 02 After Validators, 03 Before Validators, 04 Before and After Validators, 05 Custom Validators using Annotations, 06 Dependent Field Validations, 07 Project, 08 Project Solution |
| `09 - Properties and Computed Fields` | 02 Properties, 03 Computed Fields, 04 Project, 05 Project Solution |
| `10 - Custom Serializers using Annotated Types` | 02 Custom Serializers, 03 Project, 04 Project Solution |
| `11 - Complex Models` | 02 Model Composition, 03 Model Inheritance, 04 Project, 05 Project with Solution, 06 Project Final Solution |
| `12 - Applications` | 02 Consuming a REST API, 03 Ingesting a CSV File, 04 Validating Function Arguments, 05 Model Code Generators |

Verweise auf das nächste Notebook folgen dieser Reihenfolge (z. B. nach `03 - Deserialization` kommt `04 - Serialization`).

## Working with Notebooks

- Each notebook is self-contained and demonstrates concepts through executable examples.
  - Minimal external dependencies (standard library and Pydantic).
  - Examples progress from simple to complex; introspection (`model_fields`, `model_dump()`, `model_json_schema()`, `ValidationError.errors()`) is used to show Pydantic internals instead of just stating them.
  - Include both what works and what fails (`try`/`except ValidationError`).
  - Use the Pydantic v2 API only (`model_dump`, `model_validate`, `field_validator`, `ConfigDict` …), no deprecated v1 names (`.dict()`, `@validator`, `class Config`).
  - Access `model_fields` and `model_computed_fields` on the class, not on an instance (deprecated since Pydantic 2.11, removed in V3).
- Notebooks are committed **without** outputs: `nbstripout` (dev dependency) entfernt sie beim Commit als Git-Filter. Einmalig pro Klon einrichten mit `uv run nbstripout --install`; lokal bleiben die Outputs im Notebook erhalten.

## Styleguide für Lehr-Notebooks und Dokumentation (Deutsch)

### Sprache
- Erklärtexte, Markdown-Zellen, Kommentare und Ausgabetexte (`print`) sind auf Deutsch.
  - Code, Bezeichner und Inline-Code in Backticks bleiben unverändert.
  - Etablierte Fachbegriffe bleiben englisch (z. B. Model, Field, Validator, Serializer, Alias, Default-Wert, Annotated Type, Strict Mode, Coercion). Daraus wird in deutschen Texten kein Denglisch-Mix gebaut.

### Ansprache
- Keine direkte Anrede („Sie“, „du“, „Beachten Sie“, „Wie Sie sehen können“, „Lassen Sie uns“, „Vergessen Sie nicht“).
  - Stattdessen sachlich und neutral formulieren: Aussagesätze, Passiv oder „wir“ als gemeinsame Perspektive („Testen wir …“, „Wir prüfen …“).
  - Beispiel: „Wie Sie sehen können, schlägt die Validierung fehl.“ → „Die Validierung schlägt fehl.“
  - Hinweise beginnen mit „Hinweis:“ statt mit einem Imperativ.

### Erklärstil (nach Real Python, auf Deutsch)
Die Texte folgen dem Stil der Tutorials von Real Python: Sie führen Schritt für Schritt durch das Thema, statt Regeln nur aufzuzählen.
- **Motivation zuerst:** Ein Abschnitt beginnt mit der Frage oder dem Problem, das er löst („Wie verhindert Pydantic, dass ein `str` im `int`-Feld landet?“), danach folgt die Antwort.
  - **Warum nur, wo es hilft:** Ein Grund steht dort, wo ein Leser sonst etwas falsch versteht oder sich wundert, und er passt in einen Satz („Der Validator läuft nach der Typkonvertierung, weil er den bereits geparsten Wert prüfen soll“). Bei Selbstverständlichem entfällt er.
  - **Kleine Schritte:** Abläufe stehen als nummerierte Liste, jeweils mit einem Gedanken pro Schritt. Kurze Sätze, kurze Absätze.
  - **Konkret vor abstrakt:** Erst das Beispiel und die Beobachtung, dann der Fachbegriff. Bilder wie „Bauplan“ und „Behälter“ sind erlaubt, wenn sie das Verständnis erleichtern.
  - **Zwischenfazit:** Nach einem Gedankenschritt fasst ein Satz zusammen („Kurz gesagt: Das Model beschreibt die Daten, die Instanz hält sie.“).
  - **Gesprächiger, aber ohne direkte Anrede:** Der Ton ist freundlich und erklärend. Statt „du“ steht „wir“ oder eine sachliche Formulierung (siehe Ansprache).

### Aufbau eines Notebooks
Jedes Notebook ist knapp gerahmt, damit es nicht überladen wirkt.
- **Lernziele am Anfang:** Die einleitende Markdown-Zelle endet mit ein bis drei Stichpunkten „Danach ist klar, …“ und, falls nötig, einem Satz zu den Voraussetzungen („Voraussetzung: Kapitel 02 …“). Mehr nicht.
- **Praxisbeispiel in den Project-Notebooks:** Das realistische Beispiel (etwa eine API-Eingabe, eine Konfiguration oder ein Datensatz aus einer Datei) liegt in den Notebooks `Project` und `Project Solution` am Kapitelende. Einzelne Notebooks brauchen kein eigenes Praxisbeispiel.
  - Ausnahme: `02 - Creating a Pydantic Model` enthält ein kleines Praxisbeispiel am Ende, weil das `Project` erst am Kapitelende folgt.
- **Zusammenfassung am Ende:** Die letzte Markdown-Zelle fasst die wichtigsten Punkte in drei bis fünf Stichpunkten zusammen. Optional folgt ein Verweis auf das nächste Notebook oder auf die Pydantic-Dokumentation.

### Erklärungen zu Code
- Einfache Erklärungen stehen als **Inline-Kommentare** (`#`) direkt in der Code-Zelle, nicht in einer eigenen Markdown-Zelle. Das spart Platz.
  - Ein Kommentar steht über der Zeile oder am Zeilenende und umfasst ein bis zwei Sätze („`type(...)` bestätigt: Das Ergebnis ist ein gewöhnliches `dict`.“).
  - Er klingt wie ein kurzer Satz in einem Tutorial, nicht wie ein Etikett: Er sagt, was die Zeile zeigt oder bestätigt, und bindet das Warum bei Schlüsselstellen (Validierungsreihenfolge, Coercion, Fehlerverhalten, Rückgabewerte) in den Satz ein, etwa „`print` gibt echte Zeilenumbrüche aus, sonst stünde `\n` im Text.“
  - Starre Muster wie „Warum: …“, „Hinweis: …“ oder „Ergebnis: …“ am Kommentaranfang entfallen, ebenso das Präfix „Was der Code macht:“. Ein Kommentar wiederholt auch nicht bloß die Ausgabe.
  - Kommentare verraten keine Antwort auf eine Vorhersagefrage. Die Auflösung steht dann in der `<details>`-Zelle (siehe unten).
- Bei komplexen Abläufen (mehrere Schritte, Tabelle nötig), bei Auflösungen von Vorhersagefragen und bei Zwischenfazits („Kurz gesagt: …“) steht weiterhin eine Markdown-Zelle direkt **über** der Code-Zelle. Sie beginnt mit **„Was der Code macht:“** und steht eingeklappt in einem `<details>`-Block (`<summary>Erklärung anzeigen</summary>`, danach eine Leerzeile, dann der Text), damit zuerst selbst nachgedacht werden kann:

    ```
    <details>
    <summary>Erklärung anzeigen</summary>

    **Was der Code macht:** …
    </details>
    ```
- Bei Schlüsselstellen (z. B. Coercion vs. Strict Mode, Reihenfolge von `before`/`after`-Validatoren, Alias bei Input vs. Output, Mutation vs. Revalidierung, `model_dump` mit `exclude_unset`) steht vor der Code-Zelle eine kurze Vorhersagefrage („Was gibt das aus?“). Sie steht entweder am Ende der einleitenden Markdown-Zelle des Abschnitts (als **„Vorhersage:“**) oder in einer eigenen Markdown-Zelle.
- Der Fließtext in den Markdown-Zellen bleibt gesprächig und in zusammenhängender Prosa (siehe Erklärstil). Durch die Inline-Kommentare wird er nicht knapper: Motivationsfragen, Überleitungen und Zwischenfazits tragen weiter den Erzählfluss.

### Technik
- Nur den Text ändern, nicht die Struktur: Zell-IDs, Metadaten, Outputs und das JSON-Format (Einrückung, Zeilenumbrüche) bleiben erhalten, damit der Git-Diff minimal ist. Neue Zellen (Vorhersagefragen) bekommen eine eigene `id`.
  - Nach Änderungen prüfen, ob keine englischen Reste oder Anredeformen übrig sind (z. B. per `grep`).
  - Nicht committen, ohne zu fragen.
