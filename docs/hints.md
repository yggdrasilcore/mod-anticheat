# Author notes (ground truth)

The prose is generated from `src/` and `conf/`. The following context is not
expressible from the source alone; treat it as authoritative.

## What this module is

A server-side, passive anticheat. It observes the movement and packet data the
client already sends (move flags, opcodes, positions) and raises a *report* when
a check trips. It never guesses from memory scanning; every detection is derived
from inbound movement/opcode traffic, so it can produce false positives on
legitimate unusual movement.

## Report model

- Each player accumulates an in-memory report counter per report type
  (Speed, Fly, Jump, WaterWalk, TeleportToPlane, Climb, Teleport, IgnoreControl,
  ZAxis, AntiSwim, Gravity, Antiknockback, NoFallDamage, OpAck, CounterMeasure).
- Counters are flushed to the characters database every
  `Anticheat.SaveReportsTime` seconds into `players_reports_status`, and the
  per-day totals into `daily_players_reports`.
- `Anticheat.ReportInChatThreshold.Min.<type>` / `.Max.<type>` bound when GMs get
  a chat alert for that type: alerts start at the Min count and stop at Max. The
  Max must be >= Min or the alert is never shown.
- `Anticheat.AlertFrequency.<type>` throttles alerts: an alert is only sent when
  the running count is a multiple of the frequency.

## Automatic moderation

Independent of alerts, `Anticheat.KickPlayer` / `BanPlayer` / `JailPlayer` with
their `ReportsFor*` thresholds act once total reports reach the configured value.

## Counter measures

`Anticheat.CM.*` are active responses, not detections:

- `TIMEMANIPULATION` hardens against a crash-inducing time-manipulation exploit.
- `Teleport` returns the player to their last GPS position.
- `FLYHACK`, `JUMPHACK`, `ADVJUMPHACK`, `IGNOREZ` put the player back on the ground.
- `SPEEDHACK` clamps movement to the server rate.
- `ALERTSCREEN` / `ALERTCHAT` notify online GMs when a measure fires.

`Anticheat.BG.StartAreaTeleport` returns a player to the battleground start spot
if they leave it before the match begins.

## False-positive tuning

`Anticheat.SpeedLimitTolerance` is a whole-percent allowance added to the allowed
speed before a speed report is raised (a large enough tolerance suppresses
marginal flags). `Anticheat.StricterFlyHackCheck` and
`Anticheat.StricterDetectJumpHack` enable stricter move-flag checks that are more
sensitive and therefore more prone to false positives.

## Operator surface

Config lives in `conf/Anticheat.conf.dist`. In-game GM commands:

| Command | Purpose | Security |
| --- | --- | --- |
| `.anticheat global` | server-wide report statistics | GM |
| `.anticheat player` | reports for one player | GM |
| `.anticheat warn` | warn a reported player | GM |
| `.anticheat jail` | jail a reported player | GM |
| `.anticheat parole` | release a jailed player | Admin |
| `.anticheat delete` | delete a player's reports | Admin |
| `.anticheat purge` | purge old reports | Admin |
