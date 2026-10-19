# Fun-Course

A web app for scheduling student presentations. Teachers open time slots in university halls. Students register a presentation into a slot, mark which topics interest them, and see who is attending and what others have commented.

Built as a team project by three students, with a PHP backend and a plain JavaScript frontend. No frameworks on either side.

## Stack

| Part | Tech |
|---|---|
| Backend | PHP 8 (strict types), PDO |
| Database | MySQL, schema and seed scripts in `database/` |
| Frontend | Vanilla JavaScript (ES modules), HTML, CSS |
| Auth | Server-side sessions with a one hour lifetime, role-based access for teachers and students |

## Features

**Students**
- Register, log in and log out
- Browse scheduled presentations and open each one's details
- Register a presentation into a free slot
- Mark interest in presentations and set attendance preferences
- Read and add comments on a presentation

**Teachers**
- Separate teacher panel, protected by role
- Create presentation slots by date, faculty and hall
- View all slots and the presentations booked into each one

## How it is built

The backend has its own small router that maps each method and path to a controller. Controllers handle HTTP, and services hold the queries and business rules. Errors go through one global handler, so every failure comes back to the client in the same format.

```
server/
├── index.php              entry point, CORS, session setup, route table
└── src/
    ├── router.php         method and path dispatch
    ├── controllers/       auth, events, slots, interests, faculties, halls
    ├── services/          database queries and business rules
    ├── errors/            HTTP exception types
    └── global_error_handler.php
client/
├── src/
│   ├── pages/             login, register, home, event pages
│   ├── teacher-panel/     teacher-only pages
│   ├── outlets/           route guards for logged-in users and teachers
│   ├── components/        button, card, input, modal, table, dropdown
│   └── services/          API clients
└── styles/                design tokens, component and page styles
database/
├── schema.sql             roles, users, faculties, halls, slots, events, interests, attendings, comments
└── seed.sql               roles, faculties, halls and sample users
```

The frontend is a single-page app with client-side routing and route guards, written without a framework. It uses reusable components and a shared set of CSS design tokens.

## Running locally

You need PHP 8.1+, MySQL 8 and Node (only for the static file server).

```bash
# 1. Create the database
mysql -u root -p -e "CREATE DATABASE schedule_db"
mysql -u root -p schedule_db < database/schema.sql
mysql -u root -p schedule_db < database/seed.sql

# 2. Configure
cp .env.example .env              # then fill in DB_USER and DB_PASS

# 3. Start the API on port 3001 (run from the server folder)
cd server
php -S localhost:3001 index.php

# 4. In a second terminal, serve the frontend on port 3000
cd client
npm run dev
```

Open `http://localhost:3000`.

## Team

Built by [Dimitar Stoilov](https://github.com/15Mitko15), [mila-z](https://github.com/mila-z) and [Martin Nikolov](https://github.com/marto3000).
