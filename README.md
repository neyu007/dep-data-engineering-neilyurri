# **What problem you are solving**
I want to check whether More Power(Iloilo City Power provider) pricing is reasonable base on the the spot market and grid data events. Also I want to track Spot Market Volatility and Utility Rate Impacts

# **Where your data comes from**

To build a robust historical tracking dashboard that correlates grid alerts with local utility rates in Iloilo City, I will be extracting and processing data across three different velocity tiers. Here is a breakdown of my data sources and exactly how I plan to ingest them.

## 1. High-Velocity Market Data (The Spot Market)
For my high-velocity market data, my primary source for wholesale electricity pricing and generation volume will be the Independent Electricity Market Operator of the Philippines (IEMOP), which operates the Wholesale Electricity Spot Market (WESM).

_What I will extract:_ I will pull Real-Time Dispatch (RTD) Market Clearing Prices (to capture volatile spot prices), Load Weighted Average Prices (to capture the actual prices utilities pay), and Regional Summaries specifically for the Visayas Grid.

_Where I will access it:_ I will access public market logs directly through the IEMOP Market Data Portal. For the massive historical datasets required for my pipeline, I plan to formally request access to their Market Information Databank (MIND) and Knowledge Suites, as they are known to accommodate academic and cohort-based projects.

## 2. Event-Driven Grid Data (The Outages & Alerts)

To correlate why these spot market prices suddenly spiked, I need to track when the Visayas grid was experiencing actual failures or supply deficits. I will source this event-driven grid data from the National Grid Corporation of the Philippines (NGCP) and the Department of Energy (DOE).

_What I will extract:_ My specific extraction targets are timestamps of Yellow and Red grid alerts, logs of specific power plant outages, and the daily Available Capacity versus Peak Demand in the Visayas Grid.

_Where I will access it:_ Since the NGCP publishes highly structured daily grid status updates on their website and social media—rather than through a clean open API—I plan to write a Python web scraper using BeautifulSoup or Selenium to automatically parse these text alerts into tabular database rows. I will supplement this by downloading historical "System Peak Demand" Excel datasets from the DOE.

## 3. Low-Velocity Consumer Data (The Billing Impact)

To complete the story, I need to show exactly how these high-frequency IEMOP market spikes translated into the monthly bills of end consumers right here in Iloilo City. My low-velocity consumer data will come directly from our local distribution utility, MORE Electric and Power Corporation, as well as the Energy Regulatory Commission (ERC).

_What I will extract:_ I will extract the monthly blended generation charges and the total residential electricity rates (per kWh).

_Where I will access it:_ MORE Power publishes highly detailed monthly rate advisories that explicitly break down cost components and state the exact percentage of power sourced from WESM that month. I will scrape this data from their official website and corporate announcements to serve as the 'final outcome' metric in my database.

**My Data Architecture Strategy**

From a data modeling perspective, I will configure my ingestion scripts to pull the massive volume of IEMOP time-series data into a large, partitioned fact table within my relational database (e.g., PostgreSQL / Supabase). I will then treat the scraped NGCP alert logs and MORE Power monthly rate advisories as dimension tables, allowing me to join them cleanly against my main fact table to power the final historical dashboard.
