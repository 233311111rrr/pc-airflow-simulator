# 🌬️ PC Case Airflow Simulator

[繁體中文](README.md) | **English**

A browser-based simulator that shows **airflow direction, air speed and temperature distribution** inside a PC case, so you can compare positive pressure, negative pressure, radiators and fan layouts.

A single HTML file with **zero dependencies and nothing to install**. It works on phones, tablets and desktops.

> ⚠️ This is a **simplified 2D side-view fluid simulation**. It is meant for **comparing layouts against each other**, not for accurate CFD. CPU and GPU temperatures are rough estimates.

---

## ✨ Features

### Simulation and display
- Side-view cross-section (left = front, right = rear, plus top and bottom) with a temperature heatmap (blue = room temperature → red = hot)
- White arrows show airflow direction and speed; switch between **Temperature** and **Air speed** views
- Live readout: intake / exhaust CFM, net difference, **positive / negative / balanced** pressure, average air temperature inside the case, and estimated CPU / GPU temperatures
- The simulation stays pinned at the top of the page while the settings below scroll independently

### Case and environment
- Custom case depth (front to rear), height, and thickness (side panel to side panel) in cm
- Side panel: tempered glass / solid steel / mesh (affects heat loss)
- Optional bottom PSU shroud
- Room temperature and intake filter clogging

### Fans
- Unlimited fans. Set the mounting side (front / rear / top / bottom), position (%), **12 cm or 14 cm** size, and intake or exhaust
- **Enter maximum airflow (CFM) and maximum RPM by hand.** Adjust speed with a slider (0–100%) or type an RPM directly
- Built-in presets: positive pressure, negative pressure, balanced, and a liquid-cooling example

### CPU / GPU
- **Drag them directly on the canvas** to move them, or type left / top / width / height (%)
- Air cooled or liquid cooled, with an adjustable load %
- The graphics card has its own **fan count, diameter, per-fan CFM, max RPM and speed**. The GPU temperature estimate responds to total fan airflow
- **Model database**: pick a model and the wattage, dimensions and fan data are applied for you
  - GPUs: NVIDIA RTX 50 / 40 / 30 / 20 and GTX, AMD RX 9000 / 7000 / 6000 / 5000, Intel Arc
  - CPUs: Intel Core Ultra 200S and 12th–14th gen, AMD Ryzen 5000 / 7000 / 9000, Threadripper
  - Coolers: common tower air coolers and AIO pump blocks, with real dimensions

### Liquid cooling
- Radiators (120 / 140 / 240 / 280 / 360 / 420) with a configurable side, position, thickness and target (CPU, GPU or both)
- A liquid-cooled component's heat goes **100% to the radiator**. Air passing through the radiator meets resistance and is heated
- If a liquid-cooled component has no radiator assigned, it falls back to air cooling

### More
- **Passive vents** (filters / mesh)
- **Thermometers**: unlimited, draggable, or placed by position %. Each one reads the air temperature around its point
- Responsive layout for any screen size, with dark and light themes and touch support


---

## 🎮 Controls

| Action | How |
|---|---|
| Move CPU / GPU / thermometers | Drag on the canvas, or enter positions in the matching tab |
| Add or delete fans, radiators, vents, thermometers | The "＋ Add" and ✕ buttons in each tab |
| Apply a model | The dropdowns in the CPU/GPU tab |
| Switch layout | The Positive / Negative / Balanced / Liquid example buttons under the canvas |
| Pause / reset airflow | The buttons under the canvas |

---

## 🧠 How it works

- Uses Jos Stam's **Stable Fluids** method on a grid of about 110 × N cells to solve incompressible airflow (advection and pressure projection)
- Fans act as fixed-velocity boundaries that inject or extract air. Air speed is computed as `airflow ÷ fan area`
- Vents are pressure outlets, and radiators are modeled as damped porous regions
- Heat sources warm nearby air, hot air has buoyancy, and temperature is carried by the flow and relaxes toward room temperature depending on the side panel
- CPU / GPU temperature = nearby air temperature + power × an equivalent thermal resistance (the GPU's resistance is scaled by total fan airflow)

---

## 📚 About the database

The marks in the dropdowns show how far each entry has been checked:

| Mark | Meaning |
|---|---|
| ✓ | Checked online (GPU: wattage and dimensions; CPU: maximum power, PPT / PL2) |
| ✓W | Only the wattage was checked (no official reference dimensions) |
| None | Typical or reference-design values, **not individually verified** |

Notes:
- Add-in-board GPU dimensions vary a lot (the same model can differ by dozens of mm in length and by 1–2 slots in thickness), so **please correct them to match your actual card**
- Fan diameters and airflow are typical values estimated from fan size, since most vendors do not publish them
- The database may be missing recently released models. PRs are welcome

The data lives in three objects inside `airflow.html`: `GD` (GPUs), `CD` (CPUs) and `KD` (coolers). Each is a comma-separated string you can edit to add entries.

---

## ⚠️ Limitations

- 2D side view only, so 3D effects such as a graphics card's sideways exhaust, cables and motherboard blockage are not modeled
- Heat output and thermal resistance are simplified, so temperatures are only useful for comparing layouts
- There is no loop or pump model. Liquid cooling is simplified as "heat moves to the radiator"



## 🤝 Contributing

Issues and PRs are welcome, especially:
- Adding or correcting GPU, CPU and cooler data (please include an official source link)
- Model improvements and performance optimizations

## 📄 License

Add a license that suits you (for example, MIT).
