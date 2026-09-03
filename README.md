# The Aster Hotel — AI Reservation & Concierge Voice Agent

An AI-powered hotel reservation and concierge voice-agent prototype built with **xAI Playground** and an **n8n mock backend**.

## Overview

The agent acts as a warm, professional front-desk voice assistant for **The Aster Hotel**. It is designed to handle:

- Room availability checks and bookings
- Hotel FAQs
- Housekeeping requests
- Concierge service scheduling
- SMS confirmation messages
- Out-of-scope questions with safe escalation to the front desk

The backend is intentionally mocked for the assessment; no live Property Management System (PMS) is required.

## Architecture

```text
Caller
  ↓
xAI Voice Agent
  ├── check_room_availability ──→ n8n webhook
  ├── book_room ────────────────→ n8n webhook
  ├── request_housekeeping ────→ n8n webhook
  ├── schedule_concierge_service → n8n webhook
  └── aster_send_confirmation ─→ n8n webhook
                                      ↓
                              Mock JSON response
```

## Agent Configuration

- **Platform:** xAI Playground
- **Agent:** The Aster Hotel Reservations & Concierge Agent
- **Voice:** Eve
- **Turn detection:** Server-side VAD / default voice-agent turn detection
- **Deployment:** Live
- **Persona:** Warm, polished, efficient hotel front-of-house assistant

## Embedded Hotel FAQ

The agent answers these questions directly from its embedded knowledge base without a tool call:

| Topic | Answer |
|---|---|
| Check-in | 3:00 PM |
| Check-out | 11:00 AM |
| Breakfast | Complimentary, daily 7:00 AM–10:00 AM |
| Valet parking | $25 per night |
| Pool & gym | 2nd floor, daily 6:00 AM–10:00 PM |

For information outside the knowledge base, the agent does not invent an answer and offers the front desk as the escalation path.

## Tools

| Tool | Purpose |
|---|---|
| `check_room_availability` | Checks room availability and returns rate/alternatives |
| `book_room` | Creates a mock room booking |
| `request_housekeeping` | Logs a housekeeping request |
| `schedule_concierge_service` | Schedules a mock concierge service |
| `aster_send_confirmation` | Sends a mock SMS confirmation |

> **Naming note:** The assessment calls the final tool `send_confirmation`. Because a team-level tool with that name already existed, this project uses the unique name `aster_send_confirmation` to avoid affecting another project. Its function and payload are unchanged.

## Mock Backend Responses

### `check_room_availability`

```json
{
  "available": true,
  "room_type": "Deluxe King",
  "rate_per_night": 189,
  "alt_options": ["Standard Queen - $149/night"]
}
```

### `book_room`

```json
{
  "status": "confirmed",
  "confirmation_code": "HTL-2291"
}
```

### `request_housekeeping`

```json
{
  "status": "logged",
  "eta_minutes": 20
}
```

### `schedule_concierge_service`

```json
{
  "status": "scheduled",
  "service_id": "CNC-8817"
}
```

### `aster_send_confirmation`

```json
{
  "status": "sent",
  "channel": "sms"
}
```

## Demonstration Scenarios

1. **Room booking** — availability → rate confirmation → booking → confirmation code → SMS confirmation
2. **Unavailable room** — unavailable result → alternative room/rate offered
3. **Hotel FAQ** — breakfast question answered from embedded knowledge base without a tool call
4. **Housekeeping** — room number + request → housekeeping tool → ETA → confirmation
5. **Concierge** — service details + time → concierge tool → service ID → confirmation
6. **Out of scope** — unsupported policy question → transparent escalation to front desk

## n8n Mock Backend

The mock backend contains five webhook workflows:

- `check_room_availability` → `Return Availability`
- `book_room` → `Return Booking`
- `request_housekeeping` → `Return Housekeeping`
- `schedule_concierge_service` → `Return Concierge`
- `send_confirmation` → `Return Confirmation`

The n8n workflow named `send_confirmation` corresponds to the xAI tool named `aster_send_confirmation`.

## Demo

**Loom:** _Add final Loom share link here_

The Loom walkthrough demonstrates the xAI configuration, voice settings, tools, n8n mock backend, mock responses, and supported test flows.

## Limitations

- The backend is mocked and does not connect to a real hotel PMS, payment system, inventory system, or messaging provider.
- Live voice execution depends on available xAI voice-agent credits.
- Availability, booking, housekeeping, concierge, and confirmation results shown by the mock backend are fixed assessment/demo responses.

## Project Status

Configuration and mock backend are prepared for demonstration and assessment submission. Live voice-test results should only be reported when the corresponding calls were actually executed.

## Assessment Evidence

The submission package should include screenshots/exports of:

- Agent configuration and system instructions
- Voice settings
- Tool configurations
- n8n mock workflows and response payloads
- Loom demonstration
- Final LMS submission details
