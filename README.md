**Overview**

This project simulates a facilities operations reporting system for a company running Manufacturing, Warehouse, Office, and Data Center sites across Nigeria, the UK, Germany, Kenya, the USA, and Singapore. It covers a full fiscal year (2025) of daily operational data.

The goal was to go beyond a static chart dump and build something that actually behaves like a production dashboard — live conditional formatting, cross-filtering slicers, and DAX measures that hold up under different filter contexts.

**Features**

6 color-coded KPI cards Total Energy Cost, Total CO2, Avg Occupancy, Avg Utilization, Uptime, and Safety Incidents, each with rule-based conditional formatting (red/amber/green) that updates live as slicers change

Sparkline trendlines on every KPI card showing the 3-month rolling trend

Energy Cost Trend by Facility Type a multi-series line chart colored by facility type

Downtime heatmap day-of-week × month matrix highlighting downtime patterns

Occupancy vs. Utilization scatter chart flags facilities that are busy but inefficient (or vice versa)

Efficiency & Impact Ratio chart normalized metrics (cost per sqm, cost per occupant, CO2 per kWh) so facilities of different sizes are compared fairly, not just by raw totals

Facility scorecard table  every facility's core metrics with data bars for at-a-glance scanning, plus rank columns functioning as a leaderboard

Slicers for facility type, city, facility name, and month — every visual on the page responds to all of them

**Tech stack** 
Power BI Desktop — report, visuals, and modeling
DAX — 20+ custom measures (see DAX_Measures.md)

**Dataset**

facility_operations_dataset.csv  2,555 rows of daily operational data (7 facilities × 365 days), including:

**Category**                 	**Fields**
Identity      facility_id, facility_name, city, country, facility_type, latitude, longitude
Energy       	                energy_consumption_kwh, energy_cost_usd
Cost & Maintenance           	maintenance_cost_usd
Reliability	                  downtime_hours
Occupancy                    	occupancy_count, occupancy_pct, capacity
Efficiency                  	equipment_utilization_pct, size_sqm
Environment                  	water_usage_liters, waste_generated_kg, co2_emissions_kg
Conditions	                  temperature_c, humidity_pct
Safety	                      safety_incidents
Time	                         date, day_of_week, month, is_weekend

Data is synthetic but built with realistic seasonal, weekday/weekend, and facility-type-specific patterns rather than random noise — so the trends in the dashboard are meaningful, not just decorative.

**DAX measure library**

Full measure list in DAX_Measures.md, grouped by category:

Energy (cost, per sqm, per occupant, MoM %, rolling averages)
Cost & Maintenance
Downtime & Reliability (uptime %, ranking)
Occupancy & Utilization
Environmental (CO2 per kWh, waste per occupant)
Safety (incident rate, days since last incident)
Ranking & comparison helpers (RANKX, TOPN)

**What I learned building this**
How Power BI's conditional formatting rules actually evaluate against filter context, and why direction matters (high = bad for cost/emissions, low = bad for occupancy/utilization)
Debugging a RANKX measure that returned "rank 1" for every row — traced to a city column sitting in the table that was silently narrowing the filter context RANKX was comparing against, even though the measure's ALL() scope was written correctly
The difference between "converting an existing visual" and "creating a new one" in the visual gallery, and why clicking empty canvas space before picking a chart type matters
Building sparklines as manually stripped-down line charts when the native Card visual's sparkline field wasn't behaving as expected

**Files in this repo**
facility_operations_dataset.csv.csv   	 # Source dataset
 DAX_Measures.md                  		  # Full DAX measure library
 Facilities_Dashboard.pbix                        # Power BI report file
 README.md                        		  # This file
screenshots/                     		  # Dashboard screenshots

**Contact**

Feel free to reach out or open an issue if you spot something worth improving — this was a learning project and I'm always open to feedback.
