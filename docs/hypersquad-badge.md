# SCRIPT GET HYPERSQUAD BADGE
![Preview](https://support.discord.com/hc/article_attachments/360009331331)

## HIW
This JavaScript script demonstrates client-side manipulation by injecting code into Discord's internal Webpack modules. 

It functions by scanning for HTTPUtils to hijack the app's private API handler, allowing it to send a direct POST request to the /hypesquad/online endpoint. This process effectively bypasses the standard UI (the personality quiz) to manually update a user's profile badge directly on the server.

---

```
let wreq = webpackChunkdiscord_app.push([[Symbol()],{},r=>r]);
webpackChunkdiscord_app.pop();
const chunks = Object.entries(wreq.m)
const findChunkByCode = (...codes) => {
    for (let i = 0; i < chunks.length; i++) {
        const [id,func] = chunks[i]
        const chunkCode = func.toString()

        if (codes.every(code=>chunkCode.includes(code))) return wreq(id)
    }
}

const api = Object.values(findChunkByCode("HTTPUtils")).find(e=>e?.get)

api.post({url: "/hypesquad/online",body:{house_id: 1}})

```
---

### Tutorial
https://www.youtube.com/watch?v=dB4wCL3HGHM

### House Selection: The house_id determines the badge:
- [1] House of Bravery (Purple).
- [2] House of Brilliance (Orange).
- [3] House of Balance (Green).

example: `api.post({url: "/hypesquad/online",body:{house_id: 2}})` (2, for get orange)

### Warning: Running scripts in the console can violate Discord's Terms of Service and poses a security risk if the code is from an untrusted source.
