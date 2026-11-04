# Spoilerfri

YouTube player for esports highlights that never reveals how much is left of a video, so a highlight reel can't spoil the result.

Live: https://jelledenmark.github.io/spoilerfri/

## How it stays spoiler-free

- **No length.** No timeline and no total duration; only the time watched so far is shown.
- **Nothing from YouTube.** The title, pause suggestions and end screen are covered.
- **Series mode.** For a series (one video per game) the queue is hidden and you see one game at a time. "Næste game" is always there; only after the last game does it say "Serien er slut", just like watching live.
- **The link reveals nothing.** Video IDs are scrambled into the link and unused game slots are padded, so a 3-0 and a 3-2 produce links of the same length that look like random characters.

## Link formats

- Single videos / normal queue: `?v=ID1,ID2&n=Name1|Name2`
- Series mode: `?g=<code>&n=Matchup`, where `<code>` is built like this (Bo5 = 5 slots):

```python
import base64
def series_code(ids, max_games=5):
    raw = "".join(ids + ["." * 11] * (max_games - len(ids))).encode()
    x = bytes(b ^ ((i * 131 + 17) & 255) for i, b in enumerate(raw))
    return base64.urlsafe_b64encode(x).decode().rstrip("=")
```
