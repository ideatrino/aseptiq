# Aseptiq

**An offline, zero-dependency optimizer that designs the leanest aseptic protocol for a target sterility level — while accounting for the correlated barrier failures that naive models miss.**

Most infection-control math multiplies barrier failure probabilities as if every barrier were independent: `P(contamination) = q₁ × q₂ × …`. Reality is messier. One careless operator, one power cut, or one bad lot of disinfectant can breach several "independent" barriers at the same time. Aseptiq prices that in, refuses to over-sterilize past the level you actually need, and hands back the shortest protocol that hits your target.

It is one self-contained HTML file. No install, no server, no build step, no dependencies, no tracking, no cost. Open it in a browser and it works — including fully offline.

---

## What it does

- **Models correlated (common-cause) failure** with a coupling factor β. Set β = 0 and you get the old, over-optimistic independence assumption back; raise it and the tool shows you how much of your "defense in depth" is an illusion.
- **Stops over-treatment.** Kill steps that push past your target sterility level get an escalating degradation cost, because real sterilization damages tissue, materials, and equipment.
- **Optimizes against a single dial, μ**, the exchange rate between protection and friction. Low μ stacks everything for maximum safety; high μ keeps the protocol lean and convenient.
- **Diagnoses weak points.** It flags groups where adding more barriers is wasted effort and tells you to diversify the failure mode instead.

## The model

A control's *survival factor* — how much contaminant slips past it:

| Control type | Survival factor `s` |
|---|---|
| Barrier (exclusion) | `q` (probability a contaminant gets past) |
| Kill step (reduction) | `10^(-log-kill)` (surviving fraction) |

Residual contamination probability blends two worlds:

```
S_independent = product of every control's s          (the naive world)
S_commoncause = product over GROUPS of the group's weakest link
S             = (1 − β) · S_independent  +  β · S_commoncause
```

The common-cause world keeps only each group's weakest link, because a shared failure event defeats the rest of the group at once. That single line is why stacking barriers inside one failure group gives sharply diminishing returns — and why diversification beats redundancy past a point.

The optimizer is a greedy, μ-parameterized search: it repeatedly adds the control with the highest *(log-protection gained ÷ friction cost)* and stops when nothing left clears μ.

## Use it

1. Open `index.html` in any browser, or visit the hosted version (see below).
2. Edit the list of controls — name, type, breach probability or log-kill, friction cost, and a **failure group** (controls that share a failure mode get the same group name).
3. Set μ, β, and your target sterility level.
4. Press **Optimize protocol**.
5. Read the recommended protocol and the diagnostics. Export your setup as JSON to save or share it.



## Disclaimer

Aseptiq is a planning and teaching aid, **not** a validated medical device or a substitute for regulated sterility assurance. Its numbers are model estimates from the inputs you provide. For real clinical, laboratory, or manufacturing use, follow the applicable pharmacopeia / ISO standards and validate empirically.

## License

See [`LICENSE`](LICENSE). Source is publicly visible for reference and evaluation; commercial and other use requires written permission. For licensing inquiries, contact the author.

---

Created by **Ideatrino** · 2026 · ideatrino@proton.me
