# SCRIPT GET HYPERSQUAD BADGE
![Preview](https://support.discord.com/hc/article_attachments/39481135346583)

## HIW
This script hooks into Discord’s internal Webpack system to access hidden modules that aren’t exposed publicly. It scans all loaded modules to find functions tied to activity events by matching internal identifiers, then extracts handlers for starting and completing activities—essentially bypassing normal APIs and directly calling Discord’s internal logic.

After that, it automates activity execution using infinite async loops: each loop repeatedly starts an activity, waits a set duration, completes it, then pauses for a cooldown before repeating. Multiple loops run concurrently (gathering, crafting, combat), effectively simulating continuous activity progression without user input, which relies on internal behavior and can break or trigger restrictions.

---

```
let wpRequire = window.webpackChunkdiscord_app.push([[Symbol()], {}, r => r]);
const findModule = (filter) => {
    for (let i in wpRequire.c) {
        let m = wpRequire.c[i].exports;
        if (!m) continue;
        if (filter(m)) return m;
        if (m.default && filter(m.default)) return m.default;
        for (let j in m) {
            if (m[j] && filter(m[j])) return m[j];
        }
    }
};

const startActivity = findModule(m => typeof m === 'function' && m.toString().includes('"GORILLA_START_ACTIVITY_SUCCESS"'));
const completeActivity = findModule(m => typeof m === 'function' && m.toString().includes('"GORILLA_COMPLETE_ACTIVITY_SUCCESS"'));
const sleep = async ms => await new Promise(r => setTimeout(r, ms));

async function runActivityLoop(name, duration, cooldown) {
  while (true) {
    startActivity({ activity: name });
    await sleep(duration);
    completeActivity({ activity: name });
    await sleep(cooldown);
  }
}

runActivityLoop("gathering", 3000, 1000);
await sleep(500);
runActivityLoop("crafting", 5000, 128_000);
await sleep(500);
runActivityLoop("combat", 5000, 188_000);
```
---

### Tutorial
https://www.youtube.com/watch?v=8b-l5ABA_Hc

### Warning: Running scripts in the console can violate Discord's Terms of Service and poses a security risk if the code is from an untrusted source.
