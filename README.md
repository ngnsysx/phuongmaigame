# Game Phương Mai

**A browser-based campus exploration game that helps Fulbright University Vietnam students discover campus events.**

Students walk around a pixel-art version of campus, meet familiar characters, and play short mini-games. Event announcements are placed inside those interactions, so students come across them while exploring instead of having to dig them out of an inbox.

> Final project for **ENG201: Product Development**, Fulbright University Vietnam (Fall 2025), taught by Prof. Le Quan. Built by Group 5.

<p align="center">
  <a href="https://ngnsysx.github.io/phuongmaigame/"><b>▶ Play the game</b></a> &nbsp;·&nbsp;
  <a href="reports/final-report.pdf"><b>📄 Final report</b></a> &nbsp;·&nbsp;
  <a href="reports/presentation-slides.pdf"><b>📊 Presentation slides</b></a>
</p>

---

## Table of Contents

- [The Problem](#the-problem)
- [The Solution](#the-solution)
- [Features](#features)
- [Getting Started](#getting-started)
- [How to Play](#how-to-play)
- [Analytics & Data](#analytics--data)
- [Results](#results)
- [Repository Structure](#repository-structure)
- [Tech Stack](#tech-stack)
- [Team](#team)

---

## The Problem

Fulbright students often miss campus events even when they would have wanted to go. Our user research found that:

- **70%** of surveyed students had missed an event they were interested in.
- **100%** said they miss event emails because they receive too many emails every day.
- Only **20%** regularly check the OneStop events calendar, and only **40%** regularly notice posters.
- **80%** feel there are so many events that it is hard to tell which ones are relevant.

The problem is not that students don't care about events. The current channels (Outlook, OneStop, posters) are scattered and easy to ignore, so event information doesn't reach students at the right moment.

## The Solution

Game Phương Mai turns event discovery into a short, low-effort activity. Players explore a campus that looks like their own, and event information shows up at checkpoints as posters, newsletters, PDFs, TV screens, and mini-challenges with a clear **Register / Learn more** call to action.

Every checkpoint follows the same interaction pattern:

```
approach → prompt → interact → outcome / call to action
```

## Features

- **Explorable campus map.** A Common Area hub that connects to **Classroom 1** and the **Makerspace**, with collision detection and animated character movement.
- **Event checkpoints.** Newsletters, a TV poster, and an embedded event PDF (Convocation 2025 – Hearty Plant), plus a registration link for the Innovation Bootcamp.
- **9+ mini-games** tied to characters and events:

  | Location     | Mini-games                                                                  |
  |--------------|-----------------------------------------------------------------------------|
  | Classroom 1  | Keyboard Shortcuts Challenge, Kera Candy Balance, Cam Internship Shooter    |
  | Makerspace   | Color Wiring Map, DST Puzzle, Table Wipe Rush, Find the SD Card, Sink or Swim |

- **NPC stories and cutscenes.** Conversations with characters and a multi-scene story sequence.
- **Background music and sound effects**, with a music toggle.
- **Built-in feedback.** An in-game button that opens a Google Form.
- **Analytics.** Every meaningful interaction is tracked through Google Analytics 4 and GoatCounter (see [Analytics & Data](#analytics--data)).

## Getting Started

**Play online:** <https://ngnsysx.github.io/phuongmaigame/>

The game is plain HTML, CSS, and JavaScript. There is no build step and nothing to install.

### Run locally

The pages load images, audio, and a PDF, so serve them over HTTP rather than opening the files directly:

```bash
git clone https://github.com/ngnsysx/phuongmaigame.git
cd phuongmaigame
python3 -m http.server 8000
```

Then open <http://localhost:8000> in your browser.

If you use VS Code, the **Live Server** extension also works.

### Deploy

The live version is hosted on **GitHub Pages**. Because the game is fully static, the repository can also be hosted as-is on Netlify, Vercel, or any static file host. `index.html` is the entry point.

## How to Play

| Key                  | Action                                         |
|----------------------|------------------------------------------------|
| `W` `A` `S` `D` / Arrow keys | Move your character                    |
| `C`                  | Enter Classroom 1 (at the Common Area door)    |
| `M`                  | Enter the Makerspace (at the Common Area door) |
| `E`                  | Read the newsletter / open or close a window   |
| `Q` / `E` / `R`      | Start a Classroom 1 mini-game                  |
| `Space`              | Main in-game action (shoot, stabilize, start)  |
| `T`                  | Retry a mini-game                              |
| `Esc`                | Close a Makerspace mini-game                   |

Other keys (`H`, `K`, `L`, `P`, `V`, …) trigger context-specific interactions. An on-screen prompt tells you which key to press whenever you are near a checkpoint. Use **← Return to Common Area** to go back to the hub.

## Analytics & Data

The game uses two analytics tools:

- **Google Analytics 4** ([`js/ga4.js`](js/ga4.js)) sends a standardized event for each interaction:

  | Event prefix   | Example                          |
  |----------------|----------------------------------|
  | `minigame_`    | `minigame_Kera_Candy_Balance`    |
  | `checkpoint_`  | `checkpoint_TV`                  |
  | `document_`    | `document_Hearty_Plant_PDF`      |
  | `newsletter_`  | `newsletter_Innovation_Bootcamp` |
  | `scene_`       | `scene_TA_Drama`                 |

  It also records one `user_first_visit` or `user_returned` event per browser, using `localStorage`, to measure return rate.

- **GoatCounter** is a lightweight, privacy-friendly tracker used for page views and simple counters such as newsletter opens.

### Retention analysis

[`reports/user_behavior_dt.csv`](reports/user_behavior_dt.csv) holds per-user binary features (whether each user played each mini-game) and a `user_returned` label. We trained a decision-tree classifier on this data to see which mini-games were associated with players coming back. The tree is in [`reports/decision-tree.png`](reports/decision-tree.png). In this sample, playing **Cam Internship Shooter** and then **Keyboard Shortcuts** was the strongest path to a return visit.

## Results

Across two public test rounds shortly after release:

| Metric                                         | Result                        |
|------------------------------------------------|-------------------------------|
| Active users (both tests combined, first 3 days) | **148**                     |
| Page views, test 2 (first 24 hours)            | **306** from 63 unique users  |
| Return-to-play rate                            | **51.7%** (30 of 58 users)    |
| Event information opens (TV + PDF)             | **86**                        |
| Most popular mini-game                         | Kera Candy Balance: 192 plays, about 12.8 per player |

**A/B test on event delivery.** We compared a view-only event checkpoint (Common Area) with an interactive one that had a mini-game and a Register button (Makerspace). **6 of 9** interviewed players (about 67%) noticed the interactive checkpoint more and remembered more of its details.

### Documents

- 📄 [Final report (PDF)](reports/final-report.pdf): the full user research, A/B test design, MVP scope, iterations, and data analysis
- 📊 [Presentation slides (PDF)](reports/presentation-slides.pdf): the final product presentation

## Repository Structure

The game files live at the repository root so GitHub Pages can serve `index.html` directly.

```
.
├── index.html            # Common Area: entry point and hub
├── classroom1.html       # Classroom 1 map and its mini-games
├── mks.html              # Makerspace map and its mini-games
├── js/ga4.js             # Google Analytics 4 event tracking
├── asset/                # NPC sprites, posters, UI images
├── background/           # Map backgrounds
├── character/            # Player walking-animation frames
├── game/                 # Mini-game sprites (memory, puzzle, race)
├── scenes/               # Story cutscene frames
├── sound/                # Music and sound effects
├── docs/                 # Event documents shown in-game
└── reports/              # Project deliverables (not used by the game)
    ├── final-report.pdf
    ├── presentation-slides.pdf
    ├── decision-tree.png
    └── user_behavior_dt.csv
```

## Tech Stack

- **HTML5 Canvas** and **vanilla JavaScript** for rendering, movement, collisions, and mini-games
- **CSS** for overlays and UI
- **Google Analytics 4** and **GoatCounter** for analytics
- **Google Forms** for player feedback
- **Python (scikit-learn)** for the decision-tree retention analysis

## Team

**Group 5**, Fulbright University Vietnam

| Name                  | Student ID |
|-----------------------|------------|
| Hoàng Ánh Dương       | 240076     |
| Nguyễn Thị Phượng     | 230153     |
| Lê Ngô Mai Uyên       | 230134     |
| Nguyễn Huỳnh Đức      | 220080     |
| Lê Thị Kiều Thoa      | 240110     |

Instructor: **Prof. Le Quan**

---

<sub>This is an academic project. Characters and locations are inspired by the Fulbright University Vietnam community and are used for educational purposes.</sub>
