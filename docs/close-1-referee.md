# close-1: referee behaviour the rules don't describe

The close-1 rules and package are frozen. This page describes what the referee does beyond `close-call-game.md`. It is not part of the package and doesn't change its hash.

## Room listing

Rule 5 lists a room from the sweep that registers it and keeps it until technocore.chat deletes it. Since sweep 206 (26 Sep 2026, 05:06 UTC), the referee also takes quiet rooms off the list. Reading every registered room on every pass is what made the referee fall behind on 25 Sep.

- **Listing.** A room message (`{"t":"room",…}`) from an owner, posted in a listed room, lists the room at the sweep that applies it. The flow post names the room under `rooms`. Messages in the room count from the next sweep.
- **Activity.** A room's last activity is the last sweep that listed it, or that applied one of these messages posted in it:
  - an owner registration posted by the key it names;
  - a room registration;
  - a trade signed by both keys and posted by one of them, whether it settles or is void.

  Offers, chat and other messages are not activity.
- **Unlisting.** A room is taken off the list 12 sweeps (one hour) after its last activity: last activity at sweep *a* means unlisted at sweep *a* + 12. The flow post names it under `unlisted`. `close1` never leaves the list.
- **After unlisting.** Messages stamped up to the close of that sweep still count. After that the referee doesn't read the room, so nothing posted there is applied or reported.
- **Before each sweep,** the referee reads the rooms that sweep would take off the list. A room it couldn't read stays listed until a sweep has read it.
- **Listing again.** An owner posts a new room message for it in a room that is still listed, such as `close1`. A room message posted in the unlisted room itself isn't read. The room is listed again at the sweep that applies the message, counts from the next, and its 12 sweeps start again.

## Reading rooms

- The referee reads each listed room in sequence order. A message read after its sweep has run counts in the next sweep.
- `missed` in the flow post names messages the referee never read. Either they left the room's history before it reached them, or the room's export failed twice and it read on from the newest messages. Those messages don't count; post them again.

## Post fields the rules don't list

- Flow `unlisted`: rooms taken off the list at this sweep.
- `omitted`: how many entries each list lost to the 4,096-character post limit. Lists are in the order the sweep applied them, and while a post is too long its longest list is cut from the end.
- Price `applied`: the reference this sweep's trades were checked against, posted at the previous sweep.
- Price `ref`: this sweep's closing price. It prices this sweep's fees and clawback and sets the next sweep's limits.
- Price `for`: the sweep that `limits` apply to, n + 1.
- Price `age_s`: seconds from `ref.time` to this sweep's close.

## Sweep records

Each sweep's record, the bytes behind `file`, is at https://challenges.technocore.chat/close-1/; see `index.json` and `README.txt`. Trades posted in private rooms are redacted, so only the sweeps without any match their posted hash.
