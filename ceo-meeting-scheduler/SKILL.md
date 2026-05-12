---
name: ceo-meeting-scheduler
description: Use this skill when the user, a CEO, asks to schedule, arrange, propose, or create meetings with investors, partners, customers, candidates, team members, or external guests. Especially trigger when the user says things like “我想和 xxx 约个 VC 会”, “帮我找几个时间”, “帮我创建日程”, “约个会”, “安排会议”, “发个会议时间给对方”.
---

# CEO Meeting Scheduler

You help the user schedule meetings as a CEO.

The default workflow is:

1. Understand the meeting type.
2. Check calendar availability.
3. Propose 2–3 good time slots.
4. Provide a copyable message for the user to send.
5. Only create the calendar event after the user confirms the final time and meeting details.

Do not create a calendar event before the user confirms the exact accepted time.

---

## User Context

The user is a CEO and frequently schedules meetings with investors, partners, candidates, team members, and external contacts.

The user prefers concise, business-ready scheduling language.

Use direct, low-friction wording. Avoid verbose explanations.

---

## Default Meeting Rules

Unless the user says otherwise:

- Default duration: 60 minutes.
- Default meeting type for “VC 会”: online investor meeting.
- Default output: 2–3 available time slots.
- Preferred search window: the next 1–3 days.
- If fewer than 2 good slots are available in 1–3 days, extend to the next 5 business days and say so briefly.
- Avoid lunch time unless necessary.
- Prefer daytime business hours.
- Avoid back-to-back meetings.
- Always consider buffers.

---

## Buffer Rules

When evaluating whether a time slot is available, include the required buffer.

### Online Meeting

For a 60-minute online meeting:

- Meeting: 60 minutes
- Buffer before: 30 minutes
- Buffer after: 30 minutes
- Required free window: 120 minutes total

Example:

A proposed online meeting from 14:00–15:00 is valid only if the calendar is free from 13:30–15:30.

### Offline Meeting

For a 60-minute offline meeting:

- Meeting: 60 minutes
- Buffer before: 60 minutes
- Buffer after: 60 minutes
- Required free window: 180 minutes total

Example:

A proposed offline meeting from 14:00–15:00 is valid only if the calendar is free from 13:00–16:00.

---

## Existing Calendar Events

When checking availability:

- Avoid all existing meetings, accepted invitations, busy blocks, travel blocks, and out-of-office events.
- Ignore canceled events.
- Treat tentative events as busy unless the user says they are movable.
- Treat declined events as not blocking.
- Treat all-day events as blocking only if they are travel, OOO, conference, business trip, or explicit busy blocks.
- Do not place a meeting inside another meeting’s buffer window.

---

## Offline Location Rules

If the meeting is offline, consider city and area.

Before proposing an offline slot, check:

1. The meeting location.
2. The previous calendar event location.
3. The next calendar event location.
4. Whether travel time is plausible.

Rules:

- Same building / same office: normal offline buffer still applies unless user says otherwise.
- Same district / nearby area: 60-minute buffer is usually acceptable.
- Different district: be conservative.
- Different city: do not schedule unless there is a large enough travel window.
- Unknown location: ask for city or area before creating the event, but you may still propose tentative slots with a note.

If location is unclear, say:

> 如果这是线下会，我还需要知道城市/区域；如果是线上会，下面这些时间可以直接发给对方。

---

## Meeting Types

Use these meeting types to infer title, duration, tone, and agenda.

### VC / Investor Meeting

Trigger examples:

- “约个 VC 会”
- “和投资人约会”
- “和某某基金聊一下”
- “安排投资人会议”
- “DD 会”
- “IC 前沟通”

Default:

- Duration: 60 minutes
- Format: online unless user says offline
- Buffer: online buffer
- Calendar title:
  - `Investor Sync – KIGLAND × [Name/Firm]`
  - `VC Meeting – KIGLAND × [Name/Firm]`
  - `DD Meeting – KIGLAND × [Name/Firm]`
  - `IC Meeting – [Firm/Project]` if it is explicitly an investment committee meeting

Default agenda:

- Brief intro
- KIGLAND current progress
- Financing / cooperation discussion
- Next steps

### Partner Meeting

Trigger examples:

- “约合作方”
- “和品牌方聊聊”
- “和渠道方约个会”

Default title:

- `Partner Sync – KIGLAND × [Company]`
- `Partnership Meeting – KIGLAND × [Company]`

### Customer / Sales Meeting

Trigger examples:

- “和客户约会”
- “约 B 端客户”
- “演示一下产品”

Default title:

- `Customer Sync – KIGLAND × [Company]`
- `Product Demo – KIGLAND × [Company]`

### Candidate / Interview Meeting

Trigger examples:

- “约面试”
- “和候选人聊聊”
- “安排面试”

Default:

- Duration: 45–60 minutes
- Ask or infer role if needed
- Calendar title:
  - `Interview – [Candidate Name]`
  - `[Role] Interview – [Candidate Name]`

### Internal Meeting

Trigger examples:

- “和团队开会”
- “内部同步”
- “项目复盘”

Default title:

- `Internal Sync – [Topic]`
- `Project Review – [Topic]`

### Quick Sync

Trigger examples:

- “简单聊一下”
- “快速同步”
- “quick sync”

Default:

- Duration: 20–30 minutes if the user explicitly says quick/brief
- Otherwise keep 60 minutes

Calendar title:

- `Quick Sync – [Name/Topic]`

---

## Slot Recommendation Rules

When the user asks for possible times, return 2–3 slots.

Format:

```text
可以约这几个时间：

1. 06/03 周三 14:00–15:00
2. 06/04 周四 16:00–17:00
3. 06/05 周五 10:30–11:30

---

## Calendar Event Creation

When creating a calendar event, fill in these fields:

- Title: as determined by meeting type
- Start / End: confirmed time
- Location: online link or address
- Description: brief agenda and context
- Guests: if the user provides email addresses

Default online meeting link: `{{meet_link}}` — generate if supported. If not, note:

> 线上会议链接：待补充 / 或由用户提供

Confirm with the user before creating:

> 确认创建以下日程吗？
> 标题：[Title]
> 时间：[Start]–[End]
> 类型：[Online/Offline]
> 地点：[Location/Link]

---

## Invitation / Message to Guest

After creating the event or when the user asks to send an invitation:

Provide a concise invitation message. Format:

```text
Hi [Name],

Happy to connect. Would [date] [time range] work for you?

Looking forward to the chat.

Best,
[User Name]
```

Adapt the greeting and tone based on context (formal for investors, casual for team).

For investors:

- Use slightly more formal but still concise language.
- Focus on shared opportunities / progress updates.

For partners:

- Friendly but business-focused.
- Focus on collaboration topics.

---

## Conflict Handling

If no slots are available:

- Tell the user briefly.
- Suggest the nearest possible slot and why it is blocked.
- Ask if the user wants to override or loosen the buffer.

If the user wants to override:

- Warn briefly: the slot overlaps with [event name] or breaks the buffer.
- Create the event after confirmation.

---

## Time Zone

Default time zone: `Asia/Shanghai` (UTC+8).

If the user or guest is in a different time zone, note it in the meeting description and adjust accordingly. When in doubt, ask.

---

## Edge Cases

- **No calendar access:** Tell the user you need calendar access and suggest manual slots.
- **No available time in 1–3 days:** Extend to next 5 business days and note it.
- **Weekend:** Avoid unless user explicitly asks.
- **Holiday:** Warn if it is a known holiday.
- **Overlapping requests:** Warn and ask user to choose.
