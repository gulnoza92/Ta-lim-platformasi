# Milliy-ed (EduAdapt)

**An AI-assisted adaptive learning platform — a single environment for teachers, students, parents and school administrators.**

🇺🇿 [O'zbekcha](README.md) · 🇷🇺 [Русский](README.ru.md) · 🇬🇧 **English**

---

## The problem

In most schools a single teacher delivers the same material to 30+ students.
It is hard to spot who is falling behind while there is still time to act, and
parents often learn how their child is doing only at the end of the term.

## The solution

EduAdapt tracks each student's level in real time and adapts the questions to it.
AI assists the teacher but **never replaces them** — the final grade is always
set by the teacher.

## Core features

| Module | What it does |
|---|---|
| **Adaptive testing** | Questions get harder after a correct answer, easier after a mistake |
| **AI content generation** | The teacher types a topic — AI proposes an exercise |
| **Human-in-the-loop grading** | AI suggests a score; the teacher approves or edits it |
| **Early warning** | Students who drop below 45% are flagged automatically |
| **Attendance and parent contact** | Daily attendance plus direct teacher–parent messaging |
| **Home-based learning** | Individual plans and accommodations for students who cannot attend |
| **Engagement hub** | Mobile-lab scheduling and near-peer mentors for remote districts |
| **Professional development** | International courses and credential badges for teachers |
| **Admin dashboard** | Aggregate indicators across a school or district |

## Security

This system handles children's data, so security was designed in from the start:

- **Every student has their own account.** The teacher issues a one-time code;
  the student then sets a personal password. Impersonating another student is not possible.
- **All passwords are bcrypt-hashed** and never sent to the browser.
- **Quiz answers stay on the server** — the browser receives only the question and
  options, and the server marks the response.
- **Mastery is computed server-side** — a client cannot award itself a score.
- **Parents see only their own child** (a separate code per family).
- **The AI key lives only on the server**, behind a per-user request limit.
- **Automatic backups** every 24 hours, retained for 14 days.

## Technology

**Frontend:** React 18, Vite, Tailwind CSS, Recharts
**Backend:** Node.js, Express, SQLite, JWT, bcrypt
**AI:** Anthropic Claude API (proxied server-side)

## Project structure

```
src/                 React frontend
  lib/api.js         backend communication
  views/             screens by role
server/              Express backend
  src/db.js          SQLite schema
  src/auth.js        JWT and role checks
  src/routes/        API endpoints
  src/backup.js      automatic backups
```

## Getting started

```bash
# Backend
cd server
cp .env.example .env      # fill in JWT_SECRET and ADMIN_PASSWORD
npm install
npm run dev               # http://localhost:8787

# Frontend (in a second terminal)
cp .env.example .env
npm install
npm run dev               # http://localhost:5173
```

For `JWT_SECRET`: `openssl rand -hex 32`

Add `ANTHROPIC_API_KEY` to `server/.env` for the AI features. Everything else
works without a key.

## How each role signs in

| Role | What is needed |
|---|---|
| Teacher | Class code + class password |
| Student (first time) | Class code + name + one-time code, then sets own password |
| Student (afterwards) | Class code + name + own password |
| Parent | Class code + child's name + parent code |
| Admin | Name + admin password |

A one-time code is cleared once used. If a student forgets their password, the
teacher issues a new code with the "Reset password" button.

## Backups and export

The server writes a backup to `server/data/backups/` at startup and every 24 hours,
keeping the last 14. Teachers can download three CSV files: roster with access
codes, grades, and attendance. The files open correctly in Excel.

## Status

A working MVP. The backend has been tested end to end: role boundaries, class
isolation, password security and data leakage.
