# Intro to Web Development

NYU Tandon · Integrated Digital Media · **DM-UY 2193** · Fall 2026
Professor Katie Adee · kaa328@nyu.edu

**DM-UY 2193 · Section A** · Tuesday & Thursday, 10:00–11:50 AM · 370 Jay St,
Room 307, Brooklyn Campus · September 2 – December 14, 2026

The full syllabus is in [SYLLABUS.md](SYLLABUS.md).

---

## Attendance — read this now, not in week 10

Attendance is **mandatory**, and **three unexcused absences drop your grade by 5%**.

- Email me **ahead of time** if you are going to miss a class. Illness, a
  conference, a job interview, a family emergency — all fine, but request the
  accommodation through the proper channels so the absence is recorded as
  excused. No email ahead of time means the absence is marked **unexcused**.
- This class is taught **in person**. If a session moves to Zoom I will tell you
  well in advance; Zoom is not an option during in-person instruction.
- If you miss a class, it is **your responsibility** to catch up from the notes
  and examples in this repository. That is what they are here for.
- Office hours are 20-minute slots, for questions **after** you have attempted
  something yourself. **Email me to set up a time**, and include the specific
  problem you are stuck on plus a link to the relevant files in your repo.
  Office hours are not a private re-teaching of a class you missed.

Full policy in [SYLLABUS.md](SYLLABUS.md).

---

## Start here

| | |
|---|---|
| **[SETUP.md](SETUP.md)** | Software, terminal, Git, GitHub, tokens, and how to hand in work. Do this first. |
| **[TROUBLESHOOTING.md](TROUBLESHOOTING.md)** | When something breaks. Check here before emailing me. |
| **[GitHub_Cheatsheet](GitHub_Cheatsheet/README.md)** | The submit-your-homework workflow, written out in full. |

## How this repo is organised

```
WeekNN/
├── README.md              ← index for the week: what we cover, links to everything
├── HW.md                  ← the homework for that week
└── WeekN_1/  WeekN_2/     ← one folder per class day
    └── Topic/
        ├── README.md         ← the class notes for that topic
        ├── StarterFiles/     ← copy this to follow along (not every lesson has one)
        └── InClassExercise/  ← empty until I teach the class. Afterwards it holds
                                 exactly what we built together.
```

Three rules, everywhere in this repo:

- **Homework is always `HW.md`**, at the top of the week folder.
- **Class notes are always `README.md`**, in the folder for that topic.
- **`WeekNN/README.md` is just an index** — it links to the notes and the homework.
- **`InClassExercise/` is empty until that class has happened.** I push what we
  built together right after the session, so if you fell behind or missed a day,
  that folder is the exact code from the room. Check back after class, not
  before.
- **`StarterFiles/` only exists where a lesson needs one.** Some topics start
  from a blank file.

Some sessions also have:

- **`WALKTHROUGH.md`** — the class rebuilt step by step, showing exactly what
  changed at each stage. Use it if you miss a class or fall behind mid-session.
- **`HTML_Basics.pptx`** and other slides used in the lecture.

## Working with this repo

**Do not edit files inside this repo.** Copy what you need into your own
homework repo and work on the copy. If you edit files here, `git pull` will start
failing and you will spend a class fighting merge conflicts instead of building
websites.

```bash
git pull                                  # get my latest notes and examples
cp -R WeekNN/SomeTopic/StarterFiles ~/your-hw-repo/WeekNN/
```

## Weeks

| | |
|---|---|
| 01 | Getting set up — Git, GitHub, markdown |
| 02 | HTML: structure, tags, lists, images, links, forms |
| 03 | Information architecture, UX, links & directories, intro to CSS, the box model |
| 04 | CSS layout: positioning, flexbox, transforms, media queries |
| 05 | Midterm proposal presentations |
| 06 | Flexbox continued, responsive design |
| 07 | Portfolio and midterm presentations |
| 08 | CSS variables, contact forms, Photoshop |
| 09 | Intro to JavaScript: math, arrays, CSS grid |
| 10 | JavaScript objects, JSON, async data and the DOM |
| 11 | User input, local storage, geolocation |
| 12 | Bootstrap grid, Leaflet & OpenStreetMap |
| 13 | Bootstrap galleries, GSAP animation |
| 14 | Bootstrap components, final projects |
