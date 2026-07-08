# Dida Hotel MCP — Hotel Search & Booking

[![Version](https://img.shields.io/badge/Version-1.0.0-blue.svg)](https://github.com/DIDA-AI/dida_hotel_mcp_global/releases)
[![ModelScope](https://img.shields.io/badge/ModelScope-Rank%237-brightgreen.svg)](https://modelscope.cn/)
[![MCP Version](https://img.shields.io/badge/MCP-1.0.0-blue.svg)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Smithery Badge](https://smithery.ai/badge/@dida-ai/dida-hotel-mcp-global)](https://smithery.ai/server/@dida-ai/dida-hotel-mcp-global)

DIDA Hotel MCP Server Global is designed specifically for **global users and overseas travel scenarios**.

> 💡 **For Chinese users** or workflows primarily targeting the mainland China market and local payment systems, please refer to the localized version:
> 👉 [**Dida-hotel-MCP-CN**](https://github.com/DIDA-AI/Dida-hotel-MCP-CN)

An official Model Context Protocol (MCP) server that empowers AI Agents to search, compare, and book **over 2 Million hotels globally**. Powered by DIDA (Asia's #1 and world's #3 B2B travel platform), this server bridges the gap between AI travel recommendations and real-world bookings.

🏠 [Partner Center](https://global.rollinggo.store/) · 🚀 [Quick Start](#-quick-start) · 🔧 [Available Tools](#-available-tools) · 📚 [Config Examples](#-mcp-client-configuration) · 💬 [Support](#-support)

---

## 🌟 Why DIDA Hotel MCP Server Global?

Traditional AI agents can only recommend hotels based on static training datasets. The **DIDA Hotel MCP Server Global** equips your LLM agent with direct, real-time transactional capabilities:

* 🏨 **2M+ Global Hotels**: Deep coverage of major destinations worldwide, returning multilingual room details.
* ⚡ **110K+ Direct Partner Rates**: Live inventory checks, rate locks, and cancellation policy checks, ensuring the agent recommendations are bookable in real-time.
* 💰 **Monetization Ready**: Configure your own markups in the DIDA Partner Center. When users book via your AI assistant, **you earn commissions directly**.
* 🔌 **Client-Ready**: Instant integration with Cursor, Claude Desktop, Windsurf, ChatGPT, and other MCP-compliant applications.

---

## 🎯 Target Use Cases

* **AI Travel Planners**: Integrate hotel searching directly into natural language itineraries.
* **Corporate Travel Assistants**: Allow employees to query, compare, and book business travel within Slack, Teams, or custom chat interfaces.
* **Transactional Demos**: Validate end-to-end AI agent commerce and payment flows without heavy backend integration.

---

## 🚀 Quick Start

Integrate global hotel search and booking into your AI assistant in under **5 minutes** with no coding required.

### Step 1: Get Your Developer API Key

1. Go to the [DIDA Partner Center](https://global.rollinggo.store/apply) and sign up for a free key.
2. Fill in your basic information. Approval is automated and takes 1–3 minutes. You will receive an email containing:
   - Your `mcp_` prefixed API Key.
   - Credentials for your B2B Partner Center dashboard (to monitor orders, configure markup, and track earnings).

---

### Step 2: Running the Server

Choose one of the methods below to run the server.

#### Method A: Run via `uv` (Recommended - Zero Config Setup)
If you have [uv](https://github.com/astral-sh/uv) installed, run the server instantly:
```bash
uv run --with-requirements requirements.txt server.py
```

#### Method B: Standard Python Setup
```bash
# Clone the repository
git clone https://github.com/DIDA-AI/dida_hotel_mcp_global.git
cd dida_hotel_mcp_global

# Setup virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies and run
pip install -r requirements.txt
python server.py
```
The local server will run on `http://localhost:8000/mcp`, automatically forwarding requests to the secure DIDA global API nodes.

---

## ⚙️ MCP Client Configuration

### 1. Claude Desktop (Stdio / Local Subprocess)
Best for local desktop workflows. Add this to your Claude Desktop configuration file (typically `~/Library/Application Support/Claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "dida-hotel-mcp": {
      "command": "python",
      "args": ["/absolute/path/to/dida_hotel_mcp_global/server.py"],
      "env": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```
*(Make sure to replace `/absolute/path/to/...` and `YOUR_API_KEY` with your actual local path and key).*

### 2. Cursor / Windsurf (SSE / HTTP Transport)
Add the local server URL under Cursor's MCP Settings page:
* **Name**: Dida-Hotel-MCP
* **Type**: `SSE`
* **URL**: `http://localhost:8000/mcp`
* **Headers (JSON)**:
  ```json
  {
    "Authorization": "Bearer YOUR_API_KEY",
    "Accept-Language": "en_US"
  }
  ```

---

## 🔧 Available Tools

The server registers 3 core tools to handle the complete search-to-book lifecycle:

### 1. `searchHotels`
Find hotels by location, date, price, star rating, and tags.
* **Arguments**:
  - `originQuery` (string, **required**): User's raw text request (e.g., "Find boutique hotels in Tokyo under $200").
  - `place` (string, **required**): Specific destination, attraction, or airport name.
  - `placeType` (string, **required**): Location type (`city`, `airport`, `point_of_interest`, `hotel`, etc.).
  - `checkInParam` (object, optional): `checkInDate` (YYYY-MM-DD), `stayNights`, and `adultCount`.
  - `filterOptions` (object, optional): `starRatings` (array of numbers), `distanceInMeter` (radius limit from POI).
  - `hotelTags` (object, optional): `requiredTags`, `preferredBrands`, `maxPricePerNight`.

### 2. `getHotelDetail`
Fetch real-time room types, dynamic pricing, inventory, and cancellation policies for a selected hotel.
* **Arguments**:
  - `hotelId` (number) or `name` (string) — to identify the hotel.
  - `dateParam` (object, optional): `checkInDate` and `checkOutDate`.
  - `occupancyParam` (object, optional): `roomCount`, `adultCount`, `childCount`.
  - `localeParam` (object, optional): `currency` (e.g., `USD`, `EUR`) and `countryCode`.

### 3. `getHotelSearchTags`
Retrieve metadata containing all filterable tag names (e.g., "Free WiFi", "Gym", "Kid-Friendly") to refine search filtering.

---

## 🔑 Security & Headers
* The local server forwards requests to the secure DIDA global API.
* Always supply your API Key in the headers. Keys must start with `mcp_`.
* **Required header**: `Authorization: Bearer mcp_your_key_here`

---

## 💬 Support
* 📧 **Email**: [york.lu@dida.com](mailto:york.lu@dida.com)
* 🐛 **Issues**: Submit issues or feature requests on [GitHub Issues](https://github.com/DIDA-AI/dida_hotel_mcp_global/issues).
* 👥 **WeChat Community**: Scan the QR code below for instant technical assistance (primarily Chinese-speaking developers).

![Support WeChat](support-wehcat%20group.png)

---
**Made with ❤️ by the DIDA Team**
