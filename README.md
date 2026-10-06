<div align="center">
  <img src="https://cdn.crstian.me/teslamate-achievements-logo.png" alt="TeslaMate Achievements" width="200"/>




# TeslaMate Achievements Dashboard 🏆

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![Grafana](https://img.shields.io/badge/Grafana-12.1.1+-orange?style=for-the-badge&logo=grafana)](https://grafana.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16+-blue?style=for-the-badge&logo=postgresql)](https://www.postgresql.org/)
[![TeslaMate](https://img.shields.io/badge/TeslaMate-Compatible-green?style=for-the-badge)](https://github.com/teslamate-org/teslamate)

*A gamification dashboard for TeslaMate that tracks and displays various driving achievements based on your Tesla vehicle data.*

[Quick Start](#-quick-start) • [Configuration](#️-configuration) • [Achievements](#-achievement-categories) • [Troubleshooting](#-troubleshooting)


  <img src="https://cdn.crstian.me/achievements-dashboard.png" alt="Achievements Dashboard" width="800"/>
</div>

---

## 📖 Overview

This dashboard transforms your Tesla driving experience into an engaging achievement system, motivating you to explore new driving habits, improve efficiency, and reach exciting milestones while tracking your environmental impact.

### ✨ Key Features

- **50+ Unique Achievements** across 10 diverse categories
- **Real-time Progress Tracking** based on your TeslaMate data
- **Visual Progress Indicators** with emoji-based status display
- **Comprehensive Categories**: Efficiency, Exploration, Performance, Ecology, and more
- **Customizable Metrics** to match your driving habits and preferences
- **Environment-focused Tracking** with CO₂ savings calculations

---

## 🚀 Quick Start

### Prerequisites

Before you begin, ensure you have:

- ✅ **TeslaMate**: Fully functional instance with PostgreSQL database
- ✅ **Grafana**: Version 12.1.1 or higher with table panel support

### Installation

**Edit your Teslamate "docker-compose.yml" file and add these two new lines at the end of the "volumes" section of the grafana container:**

```yaml
services:
  grafana:
    volumes:
      - teslamate-grafana-data:/var/lib/grafana
      - ~/teslamate-achievements/dashboard.yml:/etc/grafana/provisioning/dashboards/dashboard.yml
      - ~/teslamate-achievements/dashboards:/TeslaMateAchievements
```

1. **Clone this repository** (if you haven't already):
   ```bash
   git clone https://github.com/your-username/teslamate-achievements.git ~/teslamate-achievements
   ```

2. **Save your docker-compose.yml file**

3. **Recreate Grafana container**:
   ```bash
   docker compose up -d
   ```

4. **Browse the Grafana Dashboards** from the Web and you should have a new "TeslaMate Achievements" folder

---



## ⚙️ Configuration


### Custom Car ID

Update all SQL queries in the dashboard:

```sql
-- Replace car_id = 1 with your specific car ID
WHERE car_id = 2  -- Example for second car
```

### Home Location Setup

The dashboard searches for geofences containing specific patterns:

```sql
-- Edit home location patterns in relevant queries
WHERE (g.name ILIKE '%Home%' OR g.name ILIKE '%Casa%' OR g.name ILIKE '%YourHomeName%')
```

### Energy Cost Calculations

Customize your savings calculations:

```sql
-- Modify the gas savings multiplier
(sum(distance) * 0.15) - (SELECT sum(coalesce(cost,0)) FROM charging_processes WHERE car_id=1)
--                   ^^^^ Adjust this value based on local gas prices
```

---

## 🏅 Achievement Categories

### 🌿 Efficiency & Energy
| Achievement | Imperial Requirement | Metric Requirement |
|-------------|----------------------|--------------------|
| **🏅 Hypermiler** | Drive >30 mi with >100% efficiency vs rated | Drive >50 km with >100% efficiency vs rated |
| **⚡ Supercharger Junkie** | Fast charge at speeds over 120 kW | Fast charge at speeds over 120 kW |
| **⚠️ Living on the Edge** | Finish a drive with less than 5% battery | Finish a drive with less than 5% battery |
| **🍃 Efficiency Master** | Efficiency >100% vs Rated (7-day streak) | Efficiency >100% vs Rated (7-day streak) |
| **🌿 Efficiency Expert** | Efficiency >110% vs Rated (7-day streak) | Efficiency >110% vs Rated (7-day streak) |
| **🧘 Efficiency Guru** | Efficiency >120% vs Rated (7-day streak) | Efficiency >120% vs Rated (7-day streak) |

### 🌍 Exploration & Geography
| Achievement | Imperial Requirement | Metric Requirement |
|-------------|----------------------|--------------------|
| **🏙️ Local Explorer** | Visit 5 different cities | Visit 5 different cities |
| **🗺️ Regional Explorer** | Visit 10 different regions/counties | Visit 10 different regions/counties |
| **🇪🇸 National Explorer** | Visit 15 different States/Provinces | Visit 15 different States/Provinces |
| **🏕️ Day Tripper** | Travel more than 50 mi from home | Travel more than 80 km from home |
| **🎒 Adventurer** | Travel more than 150 mi from home | Travel more than 250 km from home |
| **🌍 Globetrotter** | Travel more than 300 mi from home | Travel more than 500 km from home |

### ⏱️ Time & Weather
| Achievement | Imperial Requirement | Metric Requirement |
|-------------|----------------------|--------------------|
| **❄️ Arctic Charge** | Charge while outside temp is below 32°F | Charge while outside temp is below 0°C |
| **🦇 Night Rider** | Complete a drive between 02:00 AM and 05:00 AM | Complete a drive between 02:00 AM and 05:00 AM |
| **🌙 Night Owl** | Accumulate >60 mi at night (00:00-06:00) | Accumulate >100 km at night (00:00-06:00) |
| **🦉 Midnight Warrior** | Accumulate >300 mi at night (00:00-06:00) | Accumulate >500 km at night (00:00-06:00) |
| **🌅 Early Bird** | Start 10 charging sessions before 7:00 AM | Start 10 charging sessions before 7:00 AM |
| **🐓 Rise and Shine** | Start 50 charging sessions before 7:00 AM | Start 50 charging sessions before 7:00 AM |
| **📅 Veteran** | Data collected for more than 1 year | Data collected for more than 1 year |

### 🏎️ Endurance & Performance (Expedition Series)
| Achievement | Imperial Requirement | Metric Requirement |
|-------------|----------------------|--------------------|
| **🧭 Wayfarer** | Drive >100 mi in a single trip | Drive >160 km in a single trip |
| **🍑 Iron Butt** | Drive >250 mi in a single trip | Drive >400 km in a single trip |
| **🧭 Grand Tourer** | Accumulate >6,000 mi total distance | Accumulate >10,000 km total distance |
| **🧭 Pathfinder** | Drive >100 mi in a single day | Drive >160 km in a single day |
| **🗺️ Voyager** | Drive >200 mi in a single day | Drive >320 km in a single day |
| **🧭 Trailblazer** | Drive >300 mi in a single day | Drive >500 km in a single day |
| **⚡ Odyssey** | Drive >500 mi in a single day | Drive >800 km in a single day |
| **🚀 Speedster** | Reach a top speed >110 mph | Reach a top speed >180 km/h |
| **🧘 Smooth Operator** | 600 mi streak without exceeding 75 mph | 1,000 km streak without exceeding 120 km/h |

### 💰 Savings & Charging Habits
| Achievement | Imperial Requirement | Metric Requirement |
|-------------|----------------------|--------------------|
| **🔓 First Spark** | First time data recorded | First time data recorded |
| **🆓 Bounty Hunter** | Charge 100 kWh for free ($0 cost) | Charge 100 kWh for free (0€ cost) |
| **🏴‍☠️ Freeloader** | Charge 1,000 kWh for free | Charge 1,000 kWh for free |
| **👑 Solar Baron** | Charge 2,500 kWh for free | Charge 2,500 kWh for free |
| **☀️ Zero-Cost Mogul** | Charge 5,000 kWh for free | Charge 5,000 kWh for free |
| **🌙 Night Planner** | Complete 50 charging sessions off-peak (00:00-08:00) | Complete 50 charging sessions off-peak (00:00-08:00) |
| **🦉 Grid Harmonizer** | Complete 150 charging sessions off-peak (00:00-08:00) | Complete 150 charging sessions off-peak (00:00-08:00) |
| **🪙 Piggy Bank** | Save estimated >$250 vs Gas | Save estimated >250€ vs Gas |
| **🐖 Thrifty** | Save estimated >$500 vs Gas | Save estimated >500€ vs Gas |
| **🤑 Green Tycoon** | Save estimated >$1,000 vs Gas | Save estimated >1,000€ vs Gas |
| **💰 Vault Keeper** | Save estimated >$2,500 vs Gas | Save estimated >2,500€ vs Gas |
| **💎 Electric Mogul** | Save estimated >$10,000 vs Gas | Save estimated >10,000€ vs Gas |
| **🚗 A New Car** | Save estimated >$50,000 vs Gas (Paid for itself!) | Save estimated >50,000€ vs Gas (Paid for itself!) |

### 🔋 Battery Tech & Strategy
| Achievement | Imperial Requirement | Metric Requirement |
|-------------|----------------------|--------------------|
| **🧘 Patient Lvl 1** | Spend 10 hours charging total | Spend 10 hours charging total |
| **🧘 Patient Lvl 2** | Spend 50 hours charging total | Spend 50 hours charging total |
| **🧘 Patient Lvl 3** | Spend 100 hours charging total | Spend 100 hours charging total |
| **🧘 Patient Lvl 4: Zen Master** | Spend 250 hours charging total | Spend 250 hours charging total |
| **🧘 Patient Lvl 5: The Monk** | Spend 500 hours charging total | Spend 500 hours charging total |
| **🧘 Patient Lvl 6: Eternal Charge** | Spend 1,000 hours charging total | Spend 1,000 hours charging total |
| **🔌 AC Devotee** | 80% of charging sessions are AC (Slow) | 80% of charging sessions are AC (Slow) |
| **⚡ Fast Charge Pro** | Complete 50 Supercharger sessions (>50kW) | Complete 50 Supercharger sessions (>50kW) |
| **⚡ Supercharger Elite** | Complete 150 Supercharger sessions (>50kW) | Complete 150 Supercharger sessions (>50kW) |
| **🔋 Full Cycle** | Charge from <10% to >95% | Charge from <10% to >95% |
| **📐 The Optimizer** | Finish 100 charges between 80-90% SoC | Finish 100 charges between 80-90% SoC |
| **🧲 Mega-Regen** | Regenerative braking capture exceeds 75 kW | Regenerative braking capture exceeds 75 kW |

### 🌿 Ecology & Impact
| Achievement | Imperial Requirement | Metric Requirement |
|-------------|----------------------|--------------------|
| **🌱 Eco Sprout** | Save 250 kg of CO2 vs ICE car | Save 250 kg of CO2 vs ICE car |
| **🛡️ Planet Guardian** | Save 1,000 kg of CO2 vs ICE car | Save 1,000 kg of CO2 vs ICE car |
| **🦸 Climate Hero** | Save 2,500 kg of CO2 vs ICE car | Save 2,500 kg of CO2 vs ICE car |
| **🌍 Atmosphere Defender** | Save 5,000 kg of CO2 vs ICE car | Save 5,000 kg of CO2 vs ICE car |
| **🪐 Biosphere Titan** | Save 10,000 kg of CO2 vs ICE car | Save 10,000 kg of CO2 vs ICE car |
| **🌟 Earth Champion** | Save 25,000 kg of CO2 vs ICE car | Save 25,000 kg of CO2 vs ICE car |
| **🌱 Sapling Planter** | Save CO2 equivalent to planting 10 trees | Save CO2 equivalent to planting 10 trees |
| **🌳 Personal Forest** | Save CO2 equivalent to planting 50 trees | Save CO2 equivalent to planting 50 trees |
| **🌲 Woodland Grove** | Save CO2 equivalent to planting 100 trees | Save CO2 equivalent to planting 100 trees |
| **🦜 Amazon Rainforest** | Save CO2 equivalent to planting 200 trees | Save CO2 equivalent to planting 200 trees |
| **🏞️ National Park** | Save CO2 equivalent to planting 500 trees | Save CO2 equivalent to planting 500 trees |
| **🛢️ Oil Barrel Saver** | Displace 500 gallons of gasoline | Displace 2,000 liters of gasoline |

### 🚀 Space & Time Continuum
| Achievement | Imperial Requirement | Metric Requirement |
|-------------|----------------------|--------------------|
| **🌐 Through the Core** | Drive 7,917 mi (Earth Diameter) | Drive 12,742 km (Earth Diameter) |
| **🧱 Great Wall of China** | Drive 13,171 mi (Length of Great Wall) | Drive 21,196 km (Length of Great Wall) |
| **🌍 Around the World** | Drive 24,901 mi (Earth Circumference) | Drive 40,075 km (Earth Circumference) |
| **🛰️ ISS Orbiter** | Drive 26,400 mi (One ISS Orbit distance) | Drive 42,500 km (One ISS Orbit distance) |
| **🌌 Geostationary Belt** | Drive 44,400 mi (Geostationary Orbit distance) | Drive 71,500 km (Geostationary Orbit distance) |
| **🛸 Lagrange Point L1** | Drive 120,000 mi (Halfway to the Moon) | Drive 190,000 km (Halfway to the Moon) |
| **🚀 To the Moon** | Drive 238,855 mi (Earth to Moon distance) | Drive 384,400 km (Earth to Moon distance) |
| **🏎️ 24h Le Mans** | Spend 24 total hours driving | Spend 24 total hours driving |
| **📸 Road Tourist** | Spend 100 total hours driving | Spend 100 total hours driving |
| **📅 A Week on the Road** | Spend 168 total hours (1 week) driving | Spend 168 total hours (1 week) driving |
| **🛋️ The 500-Hour Club** | Spend 500 total hours driving | Spend 500 total hours driving |
| **⏳ The 1,000-Hour Master** | Spend 1,000 total hours driving | Spend 1,000 total hours driving |

### 🏔️ Habits & Extremes
| Achievement | Imperial Requirement | Metric Requirement |
|-------------|----------------------|--------------------|
| **🔥 Daily Driver (7 Days)** | Drive 7 consecutive days | Drive 7 consecutive days |
| **🔥 Daily Driver (30 Days)** | Drive 30 consecutive days | Drive 30 consecutive days |
| **🔥 Daily Driver (100 Days)** | Drive 100 consecutive days | Drive 100 consecutive days |
| **🏖️ Weekender** | 60% of driving done on weekends | 60% of driving done on weekends |
| **💼 Workaholic** | 60% of driving done on weekdays (Mon-Fri) | 60% of driving done on weekdays (Mon-Fri) |
| **🔥 Inferno Rider** | Drive in temperatures > 104°F | Drive in temperatures > 40°C |
| **❄️ Ice Road Trucker** | Drive in temperatures < 14°F | Drive in temperatures < -10°C |
| **⛰️ Mountaineer** | Climb 30,000 ft cumulative elevation | Climb 10,000 m cumulative elevation |
| **🧗 Alpinist** | Climb 150,000 ft cumulative elevation | Climb 50,000 m cumulative elevation |
| **🏔️ The Sherpa** | Climb 300,000 ft cumulative elevation | Climb 100,000 m cumulative elevation |
| **🌊 Mariana Trench** | Descend 36,000 ft cumulative elevation | Descend 11,000 m cumulative elevation |

---



## 🐛 Troubleshooting

<details>
<summary><strong>No Achievements Showing</strong></summary>

**Symptoms**: All achievements show as ❌ or no data appears

**Solutions**:
1. Verify PostgreSQL datasource connection in Grafana
2. Check that `car_id` matches your actual vehicle ID
3. Ensure you have drives/charges recorded in TeslaMate
4. Validate database permissions for the Grafana user
5. Check datasource UID matches dashboard configuration

```sql
-- Test query to verify data exists
SELECT count(*) FROM drives WHERE car_id = 1;
SELECT count(*) FROM charging_processes WHERE car_id = 1;
```

</details>

<details>
<summary><strong>Incorrect Geofence Calculations</strong></summary>

**Symptoms**: Distance from home achievements don't work correctly

**Solutions**:
1. Verify your home geofence exists in TeslaMate
2. Check geofence name matches search patterns ("Home" or "Casa")
3. Ensure GPS coordinates are accurate in your drives data
4. Update geofence patterns in SQL queries

```sql
-- Check available geofences
SELECT name, latitude, longitude FROM geofences;
```

</details>

---

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### 🎯 Contribution Areas

- **New Achievement Ideas**: Suggest creative and motivating achievements
- **Bug Reports**: Report issues with detailed descriptions

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Credits

This project builds upon amazing technologies and communities:

- **[TeslaMate](https://github.com/teslamate-org/teslamate)** - Core data logging system
- **[Grafana](https://grafana.com/)** - Powerful visualization platform
- **[PostgreSQL](https://www.postgresql.org/)** - Reliable database backend


Special thanks to the TeslaMate community for creating such a comprehensive data ecosystem.

---


## 💝 Donate & Support

If you find this project useful and want to support its development:

### PayPal Donation
[![Donate](https://img.shields.io/badge/Donate-PayPal-blue?style=for-the-badge&logo=paypal&logoColor=white)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=MUW2XFMQB2782)

### Tesla Referral
Other way to support me is to use my referral link to purchase a Tesla product and get Credits you can redeem for exclusive awards like Supercharging miles, merchandise, and accessories.

<div align="center">
  <img src="https://cdn.crstian.me/tesla-wide.png" alt="Tesla Logo" width="100"/>
</div>

**[Use my Tesla referral link](https://ts.la/cristian354389)**

You can see my Tesla gear here: **[teslagear.crstian.me](https://teslagear.crstian.me/)**

Your support helps keep this project maintained and improved! 🙏


---

<div align="center">

**🌟 Star this project if you find it helpful!**

*Made with ❤️ by the Tesla community*

---

**Disclaimer**: This dashboard is for entertainment purposes only. Drive safely and follow all traffic laws. Achievement calculations are estimates and may not reflect all driving conditions.

</div>