# Battery-Buffered DC Fast Charging — Site Simulator

An open, interactive tool for modeling EV charging sites where a **battery buffers a limited grid connection** — for any charger. Pick a market and a vehicle (real charge curves), set the traffic, and watch the buffer drain, the grid refill it, and the chargers **throttle** when it empties.

**▶ Live:** https://brandenflasch.github.io/bess-dcfc-simulator/

It's a single self-contained `index.html` — no build step, no dependencies, fully offline. Open the file or host it anywhere static.

**Made by [Branden Flasch](https://github.com/brandenflasch).**

---

## What it does

- **Battery-buffered site model** — grid + battery + solar feed multi-port chargers; the battery covers demand the grid can't and recharges from surplus.
- **Multiport power-sharing** — each charger cabinet shares its power across its ports (e.g. a 400 kW dual-port unit: one car gets 400 kW, two split it).
- **Real charge curves** from [EVKX.net](https://evkx.net) — Tesla Model Y/3, Hyundai Ioniq 5, Kia EV6, VW ID.4, Ford Mustang Mach-E, Chevrolet Equinox EV, Volvo EX30, BYD Atto 3, Porsche Taycan, GMC Hummer EV, plus a Denza Z9 GT flash curve and a synthetic heavy-duty truck.
- **NA / EU markets** with a **sales-weighted "Mixed fleet"** — a random model per arriving car, weighted by ~2024–25 BEV sales share, so popular models dominate and overlap looks like a real site.
- **Traffic** — realistic Poisson arrivals (cars/day) or a back-to-back worst case; optional time-of-day profile with morning/evening rush peaks and a quiet overnight trough.
- **Longevity & duty-cycle** readouts — the "tank drains faster than it refills" test: how long the buffer lasts at the average load, and the max utilization it can sustain before draining.
- **Monte Carlo** — sweeps many random days for the P50/P95/worst distribution of coincident peak and demand charge, so you can size to a confidence level instead of one lucky run.
- **Battery-sizing sweep** — plots a P95 metric vs battery size to find the diminishing-returns knee.
- **Lab tab** — round-trip efficiency, reserve floor, solar, and demand-charge / time-of-use economics.

## Presets

- **BYD Flash 2.0** — 2 MW (2× 1 MW ports), ~500 kVA grid, 400 kWh / 10C buffer; loads the Denza Z9 GT (10→97% in ~9 min).
- **4× 400 kW** and **2× 200 kW** — dual-port units on a small grid + buffer.
- **Low utilization** — the quiet-site case where battery-buffering shines.
- **Solar + BESS** and **Heavy-duty fleet** (Lab) — daily solar cycle, and the truck-depot failure case.

## Data & method notes

- **Charge curves:** DC charging curves (SoC→kW) from **[EVKX.net](https://evkx.net)**, downsampled to 5% SoC steps. All credit to EVKX for the underlying data. The Denza Z9 GT curve is calibrated to a measured real-world flash session; the heavy-duty-truck and any synthetic curves are illustrations.
- **Sales weights** are approximate 2024–25 BEV registrations, used only to weight the random vehicle mix. The "~3–10 sessions per port/day" utilization anchor is a rough rule of thumb, not a cited figure. Refine both to taste.
- This is a **teaching model**, not a vendor controller emulation: adaptive 5–60 s timestep; supply order solar→grid→battery; per-cabinet sharing then site-level throttle; recharge at the charge C-rate × round-trip efficiency. Solar, economics, and the longevity/duty-cycle figures are first-order.

**Data last updated:** 2026-06-01

## Run locally

Just open `index.html` in a browser. To publish on GitHub Pages: push to a repo, then **Settings → Pages → Deploy from branch → `main` / root**.

## Credits & license

Made by **Branden Flasch** · [github.com/brandenflasch](https://github.com/brandenflasch). Charge-curve data © their respective source ([EVKX.net](https://evkx.net)). Code released under the MIT License (see `LICENSE`).
