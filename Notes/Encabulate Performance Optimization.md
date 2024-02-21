---
related-to:
  - "[[Retro Encabulator]]"
tags:
  - project
---
The [[Retro Encabulator]]'s `Encabulate` RPC needs to respond to requests in under 100ms 99% of the time (p99 < 100ms). It's current p99 time is 350ms. We believe that there are a number of things we can do to try and reduce the runtime of this RPC.

- Reduce unnecessary `.clone()` calls
- Shift the currently synchronous ambifacient calls to a background worker, ensuring to leave the necessary panendermic request verification stays in the synchronous path
# Issues
```dataview
list from #issue and -#issue/solved where related-to = this.file.link or contains(related-to, this.file.link)
```
## Solved Issues
```dataview
list from #issue/solved where related-to = this.file.link or contains(related-to, this.file.link)
```
