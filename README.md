# ESTELA Exam Builder

**Build multiple unique physics exam versions from isomorphic problem banks — export to LaTeX or Word in one click.**

ESTELA is a desktop application for physics instructors. It lets you browse a curated bank of isomorphic physics problems, assemble an exam cart, and generate multiple exam versions where every version tests the same concepts but with different problems. Powered by [Tauri](https://tauri.app) and supported by NSF Award #2421299 and the Gates Foundation.

**[⬇ Download for Windows / macOS / Linux](https://jeebuscrossaint.github.io/estela#download)**

---

## What are isomorphic problems?

Isomorphic problems share the same physics concept and difficulty level but differ in surface details — different numbers, scenarios, or physical setups — so that any two problems in a bank can be used interchangeably as exam questions without giving an unfair advantage to students who have seen a previous version.

---

## Features

- **Browse & search** — Filter problem banks by course, unit, topic, or free-text keyword. Preview individual questions with fully rendered equations (KaTeX).
- **Multiple exam versions** — Generate up to 10 unique versions from a single cart. Version A gets question 1 from each bank, Version B gets question 2, and so on. Multiple-choice answer order is also shuffled per version.
- **Flexible export**
  - LaTeX (`.tex`) using the `exam` document class
  - Word (`.docx`) with native OMML equations via bundled Pandoc
  - Browser preview (KaTeX-rendered HTML)
  - Bundle ZIP — all versions + answer keys + figures in one archive
- **Answer keys** — generated automatically alongside every exam version
- **Built-in bank sync** — download individual courses directly from within the app; no manual cloning needed
- **Light / dark mode**

---

## Download

Pre-built installers are available for all platforms at:

**[jeebuscrossaint.github.io/estela](https://jeebuscrossaint.github.io/estela#download)**

| Platform | Format |
|----------|--------|
| Windows 10/11 | `.exe` installer |
| macOS Apple Silicon (M1–M4) | `.app` bundle (zip) |
| macOS Intel | `.app` bundle (zip) |
| Linux | AppImage + `.deb` |

Installers are built automatically by CI on every update. No LaTeX distribution or Python install required — Pandoc is bundled.

---

## Getting started

### 1. Get problem banks

Either clone this repository:

```bash
git clone https://github.com/jeebuscrossaint/estela.git
```

Or use the **Get Problem Banks** panel inside the app to download individual courses directly from the sidebar.

### 2. Load the repository

Enter the folder path in the **Repository** field in the sidebar and click **Load**. All `.yaml` / `.yml` problem bank files are scanned recursively.

### 3. Build your exam

1. Browse by course and topic, or use the search box.
2. Click **Preview** on any card to see questions with rendered equations.
3. Click **+ Add** to add a bank to the cart.
4. Set an exam title and number of versions.
5. Click an export button to download.

---

## Repository structure

```
estela/
├── docs/                         # GitHub Pages site (download page + one-pager)
├── frontend/
│   └── index.html                # Full app UI (HTML + CSS + JS, single file)
├── src-tauri/
│   ├── src/main.rs               # All backend logic (Tauri commands)
│   ├── tauri.conf.json           # App config
│   └── binaries/                 # Bundled Pandoc binary (gitignored, downloaded by CI)
├── PHY I Mechanics/              # Problem bank folders, organized by unit
│   ├── 0_Vector and Math/
│   ├── 1_1D Motion/
│   └── ...
├── Templates/
│   ├── Problem-bank-template.yaml
│   ├── YAML_formatting_prompt.md
│   └── YAML_problem_types.md
└── .github/workflows/build.yml   # CI — builds all platforms, publishes to releases
```

---

## Problem bank format

Problem banks are YAML files following the ESTELA schema. See [`Templates/Problem-bank-template.yaml`](./Templates/Problem-bank-template.yaml) for the full format. A minimal example:

```yaml
bank_info:
  title: "Newton's Second Law — Horizontal Surface"
  bank_id: PHY1-F-N2L-091525
  date_created: 09-15-2025
  status: ready
  authors:
    - Your Name

questions:
  - numerical:
      id: q1
      title: Block on a surface
      text: "A <latex>5\\,\\text{kg}</latex> block is pushed with a net force of <latex>20\\,\\text{N}</latex>. Find the acceleration."
      answer:
        value: 4
        tolerance: 0.1

  - multiple_choice:
      id: q2
      title: Direction of friction
      text: "A block slides to the right. Kinetic friction acts..."
      answers:
        - answer: {text: "to the left", correct: true}
        - answer: {text: "to the right", correct: false}
        - answer: {text: "downward", correct: false}
```

**Naming convention:** `Subject-Unit-Abbreviation-mmddyy.yaml`
Example: `PHY1-F-N2L-091525` = Physics 1 / Forces / Newton's 2nd Law / Sep 15 2025

### Supported question types

`numerical` · `multiple_choice` · `true_false` · `multiple_answers` · `essay` · `formula` · `fill_in_multiple_blanks` · `ordering` · `categorization` · `hot_spot` · `file_upload`

> **Note:** `categorization` and `hot_spot` are Canvas-only and are flagged as unsupported for paper export.

---

## Creating new problem banks

1. Copy [`Templates/Problem-bank-template.yaml`](./Templates/Problem-bank-template.yaml) into the appropriate course/unit folder.
2. Use the AI prompts in [`Templates/YAML_formatting_prompt.md`](./Templates/YAML_formatting_prompt.md) to generate and format problems.
3. Name the file following the convention above.
4. Load the folder in the app to verify rendering.
5. (Optional) Upload to Canvas via the [UCF YAML-to-QTI converter](https://canvas-lti.cdl.ucf.edu/yaml-to-qti/).

---

## Building from source

**Prerequisites:**

- [Node.js 18+](https://nodejs.org)
- [Rust toolchain](https://rustup.rs)
- Pandoc binary in `src-tauri/binaries/` (see below)

**Get Pandoc (Windows):**

```powershell
$ver = (Invoke-RestMethod "https://api.github.com/repos/jgm/pandoc/releases/latest").tag_name
Invoke-WebRequest "https://github.com/jgm/pandoc/releases/download/$ver/pandoc-$ver-windows-x86_64.zip" -OutFile pandoc.zip
Expand-Archive pandoc.zip -DestinationPath pandoc-tmp -Force
Copy-Item "pandoc-tmp/pandoc-$ver/pandoc.exe" "src-tauri/binaries/pandoc-x86_64-pc-windows-msvc.exe"
```

**Get Pandoc (macOS/Linux):**

```bash
# macOS
brew install pandoc
cp "$(brew --prefix)/bin/pandoc" src-tauri/binaries/pandoc-aarch64-apple-darwin  # or x86_64-apple-darwin

# Linux
apt install pandoc   # then copy binary to src-tauri/binaries/pandoc-x86_64-unknown-linux-gnu
```

**Run:**

```bash
npm install
npm run tauri dev     # development
npm run tauri build   # production build
```

---

## Canvas integration

ESTELA exports to LaTeX and Word for print/paper delivery. For Canvas online delivery, upload the QTI zip produced by the [UCF YAML-to-QTI converter](https://canvas-lti.cdl.ucf.edu/yaml-to-qti/) to a Canvas New Quiz item bank.

---

## License

[CC BY-NC 4.0](./LICENSE) — free for educational and non-commercial use.

---

## Funding & acknowledgements

- NSF Award [#2421299](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2421299) — HSI Implementation and Evaluation Project
- Bill & Melinda Gates Foundation INV-076932
- UCF Department of Physics
