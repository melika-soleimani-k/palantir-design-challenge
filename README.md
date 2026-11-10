# palantir-design-challenge
# Palantir Design Challenge | Lens B Track
## STRATEGIC PRODUCT DOSSIER & TECHNICAL DEFENSE
**Project:** Interchange Discrepancy Console (Truth Transit Architecture)  
**Candidate:** Melika Soleimani Khoshdoni  
**Role:** Corridor Dispatcher (Human-in-the-Loop Decision Support)  
**Viewport:** Locked to 1440×900 Desktop Environment (Zero-Scroll Layout)

---

## 1. Executive Product Charter

### Real-World Operational Breakdown
At intermodal transit hubs, the primary driver of rider dissatisfaction is **feeder-to-rail connection breakdowns**. When a city bus experiences delays, passengers missing the regional train face long wait times (often over an hour)[cite: 16]. Control room dispatchers operate under severe informational asymmetry:
- **Static Timetable:** Claims the feeder bus arrived on time (18:22:00)[cite: 16, 19].
- **Consumer App ETA:** Displays an optimistic 18:23:30 arrival (assuming free-flow traffic)[cite: 16, 19].
- **Hardware GPS Ping:** Telemetry models actual arrival at 18:27:15 due to bottlenecking[cite: 16, 19].

Under a **105-second operational window** before train departure, dispatchers frequently issue "blind" 3-minute holds that fail physically while penalizing rail corridor performance[cite: 16].

### Core Mechanism
The **Interchange Discrepancy Console** is a desktop decision-support interface[cite: 16]. Instead of averaging conflicting feeds, it de-conflicts static GTFS, consumer app ETAs, and sensor telemetry on a **Single Unified 540-Second Temporal Axis**[cite: 16]. It evaluates station physical walk constraints to determine if a hold option is physically viable[cite: 16].

---

## 2. Mathematical & Telemetry Foundation (Snapshot 18:22:15)

### Tri-Stream Telemetry Discrepancy
- **Static GTFS Baseline:** Target `18:22:00` [0.00% Axis Pos][cite: 19].
- **Consumer App ETA:** Uncalibrated free-flow model `18:23:30` (+90s error) [16.67% Axis Pos][cite: 19].
- **Scheduled Train Departure (R-104):** Target `18:24:00` [22.22% Axis Pos][cite: 19].
- **Sensor Model ETA:** Hardware GPS ping from 18:22:03 (1.2km away) predicts physical dock arrival at `18:27:15` (+05m 15s delay) [58.33% Axis Pos][cite: 19].
- **Est. Transfer Complete:** Passenger ready time at platform `18:30:19` [92.41% Axis Pos][cite: 19].

### Physical Transfer Dwell Model
$$T_{\text{walk}} = \frac{185\text{m}}{1.2\text{ m/s}} = 154.17\text{s} \approx 02\text{m } 34\text{s}$$[cite: 19]
$$T_{\text{transfer}} = 154\text{s (walk)} + 30\text{s (buffer)} = 184\text{s} \approx 03\text{m } 04\text{s}$$[cite: 19]
$$T_{\text{ready}} = 18:27:15 + 03:04 = \mathbf{18:30:19}$$[cite: 19]

---

## 3. Dispatcher Decision Matrix

| Option | Departure Time | Pax Ready | Margin / Deficit | Rail Delay | Outcome Assessment |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **0s Hold (No Go)** | `18:24:00` | `18:30:19` | -06m 19s (-379s Deficit)[cite: 20] | 0m | Connection Broken. 23 Pax Missed. 58m 41s wait[cite: 20]. |
| **180s Hold (Poor)** | `18:27:00` | `18:30:19` | -03m 19s (-199s Deficit)[cite: 20] | +3m | **Ineffective Hold.** Penalizes rail; pax still miss train[cite: 20]. |
| **420s Hold (Feasible)** | `18:31:00` | `18:30:19` | +41s Positive Margin[cite: 20] | +7m | Connection Protected. Requires Line Controller Clearance[cite: 20]. |

### Headway & Passenger Wait Time
- **Next Service Departure:** `19:29:00` (65-minute headway from 18:24:00)[cite: 20].
- **Passenger Wait Time if Missed:** $19:29:00 - 18:30:19 = \mathbf{58\text{m } 41\text{s}}$[cite: 20].

---

## 4. UI Architecture & Functional Palette

- **Canvas Background:** `#080C14` / `#0E1218` (Industrial ultra-dark workspace)[cite: 21].
- **Static GTFS Data:** Slate Neutral (`#8896A6` / `text-slate-400`)[cite: 21].
- **Consumer App ETA:** Amber Warning (`#E5A93C` / `text-amber-400`)[cite: 21].
- **Sensor Model ETA / Deficit:** Critical Rose (`#FF5C57` / `text-rose-400`)[cite: 21].
- **Pax Walk Corridor:** Transfer Purple (`#A371F7` / `text-purple-400`)[cite: 21].
- **Active Rail / Decision Laser:** Action Cyan (`#4DA3FF` / `text-sky-400`)[cite: 21].
- **Feasible State / Protected Margin:** Feasible Emerald (`text-emerald-400`)[cite: 20].

---

## 5. Non-Goals & Boundaries

- **Not a B2C App:** Designed strictly for control-room dispatchers, not mobile riders[cite: 17].
- **Human-in-the-Loop:** Does not transmit automated interlock commands directly; all options preview consequences prior to manual execution[cite: 17, 21].
- **Node-Focused Scope:** Optimizes terminal interchange connection points rather than national network modeling[cite: 17].
