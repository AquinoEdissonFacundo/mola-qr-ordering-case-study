# Mola Cafetería — Real-time QR Ordering Platform

**A QR-based, real-time table-ordering system with a session model that keeps ordering tied to a physically present, staff-opened table. Productized as *QR Mesa*.**

🔗 Demo: [molaa-cafeteria.vercel.app](https://molaa-cafeteria.vercel.app) · Platform: qrmesa.com · Built by [Edisson Toloza](https://www.edissontoloza.com/)

> Note: this is a technical case study of a system I designed and built. The linked build is a private demo/pilot.

---

## The problem

A café or bar that lets customers order from their phone faces a subtle but important control problem: **who is allowed to place an order, and when?** A naïve "scan the QR, order anything" flow breaks down immediately — people can bookmark the link and order from home, tables get orders they never made, and the bar has no way to know if a table is even occupied.

Mola needed table-side ordering that stays anchored to a real, currently-seated table.

## The solution

A real-time ordering platform with two surfaces:

1. **Customer menu** — a digital menu (food, drinks and merchandising) that also serves as the venue's public showcase of what they offer.
2. **Admin panel** — where staff create and manage the physical QRs, open and close tables, and receive incoming orders live at the bar.

The core idea is a **two-layer table session model**.

## How the session model works

- The venue has a fixed set of physical QRs — one per table (e.g. 20 tables, 20 printed QR codes). Each table carries a **persistent table token** that identifies it.
- **QRs are inactive by default.** A scan on a closed table cannot place orders.
- When guests sit down, **staff activate that table**, which opens an **ordering session** for it. Only while the session is open can that table send orders to the bar.
- When the guests leave — or when the bar closes the table at the end of service — the **session is closed**, and any further attempts to order from that QR are rejected.

This is what stops the "ordering from home" problem: ordering isn't tied to *having the link*, it's tied to an **active session that only staff can open**, on a table that's physically in use. The persistent table token identifies *which* table; the session layer governs *whether ordering is allowed right now*.

## Real-time flow

Orders placed at a table appear **live at the bar** the moment they're submitted, using a realtime subscription (Supabase Realtime) rather than manual refreshing. Staff see the queue update instantly and work straight from it.

## Admin capabilities

- Generate and manage QR codes per table
- Open / close table sessions
- Receive and track orders in real time
- Manage the menu (food, drinks, merchandising)
- Multi-venue administration (the platform is built to run more than one location — the basis of the *QR Mesa* product)

## Key technical decisions

| Decision | Why |
|---|---|
| **Separate persistent table token from ephemeral session** | Identity ("which table") and authorization ("can it order now") are different concerns; splitting them makes remote/stale ordering impossible while keeping stable physical QRs. |
| **Staff-controlled activation** | Ordering can only start from inside the venue, on staff action — no self-service abuse. |
| **Realtime subscriptions over polling** | The bar needs orders the instant they happen; a live subscription is simpler and faster than refresh loops. |
| **Menu-as-showcase** | The same catalog powers both ordering and the venue's public presence, so there's a single source of truth. |

## Tech stack

**Frontend:** React · responsive, mobile-first (customers order from their phones)
**Backend / realtime:** Supabase · Supabase Realtime
**Model:** Per-table QR · persistent table token + session-scoped ordering · admin order dashboard

## Role

Designed and built the platform end to end — the customer ordering experience, the session/authorization model, the realtime bar dashboard and the admin tooling.

---

*Owner: Edisson Toloza — Full Stack Engineer · [Portfolio](https://www.edissontoloza.com/) · [GitHub](https://github.com/AquinoEdissonFacundo) · [LinkedIn](https://www.linkedin.com/in/facundo-toloza-desarrollador-web/)*
