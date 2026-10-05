<p align="center">
  <img src="assets/logo.svg" alt="Voting Diary logo" width="96" height="96">
</p>

<h1 align="center">Voting Diary</h1>

<p align="center">
  <strong>English</strong> | <a href="README.pt-BR.md">Português (Brasil)</a>
</p>

A private, browser-only diary for your votes in Brazilian elections. Log each vote you cast, then see how your choices spread across the political spectrum, parties, gender and race over the years, and check who actually won.

It is a static HTML/JS app (no framework, no build step, no backend, no account, no tracking). **Your data never leaves your browser.**

> **Heads-up:** the interface is currently in Brazilian Portuguese and the built-in knowledge is about Brazilian elections. See [Limitations](#limitations).

## Features

- **Vote log:** year, round, office, place, candidate number, name, party, gender, and optional LGBT flag and self-declared color/race.
- **Dashboard** with a shareable report:
  - where you sit on the political spectrum (overall and per year);
  - party orientation per year and a party-by-year table;
  - share of votes for women, LGBT people, Black people (black and brown), Indigenous people, and a color/race breakdown;
  - who received more than one vote from you;
  - your votes versus who won, per election;
  - elections you did not vote in (absences), kept out of the vote statistics.
- **Toggle** to include or ignore second-round votes.
- **Elections tab:** register winners to compare with your votes. Built-in results are included (see below).
- **Parties tab:** edit the left-to-right orientation of each party, so the spectrum reflects your own criteria.
- **Export** the report as PDF or PNG, and your data as JSON. **Import** a JSON back at any time.

## Quick start

No installation is required.

1. Download or clone the repository.
2. Open `index.html` in a modern browser.

Or serve it locally:

```bash
git clone https://github.com/vicleme/voting-diary.git
cd voting-diary
python3 -m http.server 8000
# open http://localhost:8000
```

To publish it, enable **GitHub Pages** (Settings > Pages > Deploy from a branch > `main` / root).

## Privacy

- Everything is stored in your browser's `localStorage` (key `diario_votos_v1`). Clearing site data erases it, so export a JSON backup now and then.
- The app makes no network requests at runtime: the libraries and fonts it needs are bundled in [`vendor/`](vendor).
- Exported JSON, PDF and PNG files contain your voting history. The included `.gitignore` skips the default export names so you do not commit them by accident.

## How results are determined

For each vote, the app decides "elected" or "not elected" in this order:

1. If you set **"Foi eleito(a)?"** on the vote, that answer wins.
2. Otherwise it looks for winners registered for the same year, office and place (President ignores the place).
3. If the candidate matches a winner, it counts as elected.
4. If not, it counts as not elected when the office has a single seat (President, Governor, Mayor) or when the winners were registered as a **complete list** ("Lista completa de eleitos"). Otherwise the result stays as "no data".

### Built-in results

These are available out of the box, and your own registrations are merged with them without duplicates:

| Office | Place | Years |
| --- | --- | --- |
| President | Brazil | 1989 to 2022 |
| Governor | SP | 2018, 2022, 2026 |
| Senator | SP | 2018, 2022, 2026 |
| Mayor | Mongaguá (SP) | 2020, 2024, 2025 (supplementary) |

Please double-check them against the official source ([TSE](https://www.tse.jus.br/)) before relying on them, and open an issue if you find a mistake.

## Data format

Exports look like this (field names are in Portuguese for compatibility with existing files):

```json
{
  "app": "diario-de-votos",
  "versao": 3,
  "nome": "Your name (optional)",
  "votes": [
    {
      "id": 1, "ano": 2022, "turno": 1, "cargo": "Presidente", "local": "",
      "num": "13", "nome": "Candidate", "partido": "PT", "gen": "M",
      "res": "", "lgbt": false, "negra": false, "raca": ""
    }
  ],
  "winners": [
    { "id": 1, "ano": 2022, "cargo": "Governador", "local": "SP",
      "nome": "Candidate", "partido": "PARTY", "full": true }
  ],
  "parties": { "PT": 0 },
  "absences": [
    { "id": 1, "ano": 2025, "turno": 1, "local": "City", "desc": "", "just": "sim" }
  ]
}
```

| Field | Values |
| --- | --- |
| `gen` | `F`, `M`, `O` |
| `res` | `""` (automatic), `"sim"`, `"nao"` |
| `raca` | `""`, `branca`, `preta`, `parda`, `amarela`, `indigena` |
| `parties` | `0` left, `1` center-left, `2` center, `3` center-right, `4` right, `""` unclassified |
| `just` | `sim`, `nao`, `pendente` |

## Limitations

- **Language:** the UI is in Brazilian Portuguese only. Translations are welcome.
- **Scope:** offices, parties and built-in results are specific to Brazil.
- **The spectrum is a simplification.** It averages the orientation you assign to each party. It says nothing about the individual candidates. The default classification is only a starting point and is meant to be edited.
- **Gender, LGBT and color/race are entered by you.** Use the candidates' public self-declaration (for color/race, the candidacy registration at the TSE).

## Project structure

```
.
├── index.html          # the whole app (HTML, CSS and JS)
├── assets/             # logo and favicon
├── vendor/             # bundled third-party code and fonts, with licenses
├── LICENSE
├── README.md
└── README.pt-BR.md
```

## Contributing

Issues and pull requests are welcome, especially for translations, fixes to the built-in results, and support for more offices and places. Keep it dependency-free: plain HTML, CSS and JS.

## License

[MIT](LICENSE). Bundled third-party software keeps its own license:

- [html2canvas](https://github.com/niklasvh/html2canvas) 1.4.1 (MIT)
- [jsPDF](https://github.com/parallax/jsPDF) 2.5.1 (MIT)
- [Bricolage Grotesque](https://github.com/ateliertriay/bricolage) and [Public Sans](https://github.com/uswds/public-sans) via [Fontsource](https://fontsource.org/) (SIL Open Font License 1.1)

Full texts are in [`vendor/licenses/`](vendor/licenses).
