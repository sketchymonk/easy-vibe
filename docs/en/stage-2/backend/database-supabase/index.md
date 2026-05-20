---
title: 'Supabase Database (Postgres) Basics'
description: 'A practical guide to using Supabase Postgres: schema design, SQL editor basics, auth concepts, storage URLs, realtime, and a repeatable integration workflow.'
---

<script setup>
const duration = 'Approx. <strong>6 hours</strong>'
</script>

# Supabase Database (Postgres) Basics

<ChapterIntroduction :duration="duration" :tags="['Supabase', 'Postgres', 'SQL', 'Backend']" coreOutput="A working Supabase-backed app skeleton" expectedOutput="A repeatable database integration workflow">

This chapter focuses on the database part of Supabase (Postgres). You will learn how to design a minimal schema, use the SQL Editor safely, and integrate a Supabase database into an application in a standardized way.

</ChapterIntroduction>

## What You Will Build

By the end, you should be able to:

1. Create tables, indexes, and constraints for an app.
2. Seed data and run queries in the SQL Editor.
3. Integrate the database into an app (schema -> SQL -> code changes).
4. Understand the basics of Auth / Realtime / Storage at a conceptual level.

---

## 1. SQL Editor Basics And Core Operations

### 1.1 `CREATE`: Define Schema

Example (a minimal `todos` table):

```sql
create table if not exists public.todos (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null,
  title text not null,
  is_done boolean not null default false,
  created_at timestamptz not null default now()
);
```

### 1.2 `INSERT`: Seed Data

```sql
insert into public.todos (user_id, title, is_done)
values
  ('00000000-0000-0000-0000-000000000000', 'First todo', false);
```

### 1.3 `SELECT`: Query Data

```sql
select id, title, is_done, created_at
from public.todos
order by created_at desc
limit 20;
```

### 1.4 `UPDATE`: Modify Rows

```sql
update public.todos
set is_done = true
where id = '00000000-0000-0000-0000-000000000000';
```

### 1.5 `DELETE`: Remove Rows

```sql
delete from public.todos
where id = '00000000-0000-0000-0000-000000000000';
```

::: warning Safety Tips
Always run `SELECT` first with the same `WHERE` clause you plan to use for `UPDATE`/`DELETE`.
:::

---

## 2. Standardized Workflow: Integrate Supabase Into Any App

Use this repeatable flow:

1. Analyze the project and identify data needs (entities + relationships).
2. Produce an `init.sql` (schema + seed) you can run end-to-end.
3. Refactor app code to use Supabase (data access layer + env vars).
4. Validate: create/read/update/delete + edge cases.

---

## 3. Auth (Conceptual)

Supabase Auth provides common flows such as:

- Email/password
- OAuth providers (e.g. Google, GitHub)
- Password reset

Your app typically needs:

1. A login UI / flow.
2. A session store.
3. Server-side verification for protected actions.

---

## 4. Realtime (Conceptual)

Supabase Realtime can help with:

- Postgres changes (subscribe to row changes)
- Broadcast & Presence (sync ephemeral state)

Use it when realtime creates user value (collaboration, live dashboards, chat), not "because it exists".

---

## 5. Storage (Conceptual)

Storage is for files (images, documents). Two URL patterns:

1. Public URL: stable link if bucket/object is public.
2. Signed URL: time-limited link for private content.

---

## Exercises

1. Design a small schema for your app (2-3 tables).
2. Write `init.sql` that can be executed from scratch.
3. Implement CRUD in your app and validate the behavior.

