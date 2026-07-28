# The Bodyweight Tax: What Your Joints Are Actually Carrying

two people run the same pace, same shoes, same surface through a marathon block.
one ends up with cooked shins and calves, the other feels nothing. everyone says
the heavier one is "just out of shape." they're not. the physics are literally
different at different bodyweights, and the gap compounds every single step.

so i ported the joint-load model behind the [Yunvo calculator](https://tools.yunvo.fit/joint-load.html)
into python, ran the numbers, and checked them against the in-vivo biomechanics
literature.

## what this is

a deterministic biomechanics model of peak and cumulative joint force during
walking and running. peak force through any joint is:

```
F = bodyweight × base[activity][joint] × speedMod × cadenceMod × strikeMod
```

cumulative load over a distance is just `F × steps`. everything in the notebook
is that one equation, swept across bodyweight, pace, cadence, foot strike, and
distance. no dataset, no fitting — pure physics, fully reproducible.

## findings

- **the bodyweight tax is perfectly linear, and inescapable.** every joint force
  scales exactly with bodyweight. a 150 / 200 / 250 lb runner all sit at 7.0×BW
  at the ankle — but that's 1,050 vs 1,400 vs 1,750 lb of real force per step.
- **running vs walking is a cliff, not a step.** for a 200 lb runner the ankle
  jumps 5.8× going from walking to running. the ankle is the hidden villain
  (7×BW, higher than any other joint), while everyone worries about their knees.
- **a marathon is astronomical.** ~54 million lb through one ankle over 26.2 mi —
  roughly 4,200 elephants, or **~7 blue whales per mile**.
- **you can't out-form a bodyweight problem.** cadence −9%, forefoot −14%, but
  +30 lb of bodyweight is +15%, erasing both fixes combined.
- **forefoot striking robs Peter to pay Paul.** tibia −14%, ankle/Achilles +18%.
- **same pace, different body:** over a 16-week / 640-mile block, the 210 lb shin
  absorbs **182 million lb more** than the 150 lb shin.

## scientific validation

the coefficients aren't invented. peak per-joint multiples were set to fall
within — and mostly below — the ranges measured in the biomechanics literature,
including in-vivo studies that instrumented real hip implants and Achilles
tendons (see `07_validation.png`).

| Joint (running) | Model | Measured range | Source |
|---|---|---|---|
| Ankle / Achilles | 7.0× BW | 6–12× BW | Komi 1990 — in-vivo tendon transducer |
| Knee (patellofemoral) | 5.5× BW | 4.0–6.4× BW (5.2 ± 1.2) | Ho et al. 2022 — meta-analysis |
| Hip (contact) | 5.5× BW | 5.0–5.5× BW jogging | Bergmann et al. 1993 — implant telemetry |
| Tibia | 3.2× BW | impact/GRF portion* | Scott & Winter 1990 |

**\*Honest caveat on the tibia.** measured *total* tibial contact force reaches
10–14× bodyweight (Scott & Winter 1990), but ~80% of that is muscle contraction,
not impact. this model's shin figure is a simplified **impact-reaching-the-bone**
proxy — do not present it on the same slide as the 10–14× contact-force number,
or you're comparing two different quantities.

for context: measured ground reaction force is ~2.5× BW running / ~1.2× walking —
the joints above the ground see far more than the ground itself does.

## methodology

reference peak-force multiples and the pace / cadence / strike modifiers trace to
published work:

- ground reaction force scales ~linearly with running speed, ~+8%/mph
  (Nilsson & Thorstensson 1989)
- ~0.6% GRF reduction per step/min above 170 spm (Heiderscheit et al. 2011)
- walking cadence / speed load relationships (Menz et al. 2003; Keller et al. 1996)
- foot-strike pattern shifts load between tibia and ankle/Achilles
  (Daoud et al. 2012; Kulmala et al. 2013; Han et al. 2025)

reference forces vary ±15–20% with anatomy, footwear, and surface. this models how
the levers move, not a diagnosis for any one person. **not medical advice.**

## references

- **Bergmann et al. 1993** — Hip joint loading during walking and running, measured
  in two patients. J Biomech. [link](https://www.sciencedirect.com/science/article/abs/pii/002192909390058M) · [OrthoLoad](https://orthoload.com/publications/hip/)
- **Komi 1990** — in-vivo Achilles tendon force in running, up to ~12× BW. [context](https://www.nature.com/articles/s41598-021-84847-w)
- **Ho et al. 2022** — "May the force be with you": patellofemoral joint force
  meta-analysis (~5.2× BW running). [summary](https://www.physiotutors.com/research/patellofemoral-joint-reaction-forces/)
- **Scott & Winter 1990** — distal tibia contact force 10–14× BW, ~80% muscle-driven. [related](https://www.sciencedirect.com/science/article/abs/pii/S0021929007002576)
- **Heiderscheit et al. 2011** — Effects of step rate manipulation on joint mechanics.
  MSSE 43(2):296–302. +10% cadence → ~34% less knee energy absorption. [link](https://journals.lww.com/acsm-msse/fulltext/2011/02000/effects_of_step_rate_manipulation_on_joint.14.aspx)
- **Foot-strike** — [systematic review](https://www.sciencedirect.com/science/article/pii/S2950273X24000729) · [Han et al. 2025](https://onlinelibrary.wiley.com/doi/10.1111/sms.70066)

## setup

```bash
pip install -r requirements.txt
jupyter notebook joint_load_model.ipynb   # model, findings, validation, references
```

## figures generated

regenerated by running the notebook:

| file | description |
|------|-------------|
| 01_bodyweight_tax.png | per-step force vs bodyweight, all joints |
| 02_walk_vs_run_cliff.png | walking vs running peak force, with ratios |
| 03_cumulative_distance.png | cumulative load building to a marathon |
| 04_cant_outform.png | every form lever vs +30 lb of bodyweight |
| 05_strike_tradeoff.png | forefoot lowers the shin by loading the ankle |
| 06_same_pace_diff_body.png | 150 vs 210 lb over a 16-week training block |
| 07_validation.png | model coefficients vs measured in-vivo ranges |
