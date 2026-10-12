# VyaparSetu AI — Team CodeBrain

An assisted digital trade prototype for farmers, FPOs, MSMEs and buyers. This upgrade is additive: the existing sign-in/sign-up, farmer crop listings, buyer browsing and ordering, order history, market-price samples, irrigation guidance and Trade Mitr assistant remain available.

## Added in this upgrade

- **Language selector:** English, Hindi, Punjabi, Bengali and Marathi. English/Hindi have the broadest translation coverage; some secondary helper text in Punjabi/Bengali/Marathi still falls back to English. The selected language is remembered in the browser. Trade Mitr demo replies support all five languages; configured AI responses are prompted to use the selected language.
- **Buyer demand requests:** buyers can publish the crop, quantity, unit, quality, delivery location and optional budget they need.
- **Demand-first supply matching:** producers can compare open buyer requests with listed crop supply. The current prototype uses transparent rule-based matching, not a trained ML model.
- **Supply aggregation estimate:** matching listings are totalled after converting kg, quintal and tonne to a common unit. This indicates whether the listed quantity could cover a request; it does not reserve stock or create a combined order.
- **Landed-price estimate:** combines the listed price with an illustrative transport estimate. Distance and rate are entered by the user; transport is divided across the listing quantity. It is not a logistics-provider quote and excludes any unknown charges.
- **Trade trust request:** users can submit business/contact details for a *demo review request*. The app does not perform KYC, GSTIN checks, phone OTP verification, or mark anyone as genuinely verified.
- **Demo order tracking:** buyer or seller can move an order through confirmed, packed, dispatched, delivered or issue-reported stages. This is manual prototype tracking, not live GPS or courier integration.
- **Demand signals:** a summary of currently listed crops and quantities is shown. This is not a predictive demand forecast.

## Requirements

- Windows 10/11
- Node.js 20+ (npm included)
- Python 3.11+ recommended

## 1. Start the backend

Open PowerShell in the project folder:

```powershell
cd backend
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
Copy-Item .env.example .env
```

If PowerShell blocks activation, run this in the same terminal, then activate again:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

Run the API:

```powershell
uvicorn app.main:app --reload --port 8000
```

API docs: http://127.0.0.1:8000/docs

## 2. Start the frontend

Open a **second** PowerShell terminal at the project root:

```powershell
cd frontend
npm install
npm run dev
```

Open http://localhost:5173

## 3. Test the original features

1. Create an account and choose **Farmer**. Publish a crop listing.
2. Sign out and create a second account as **Buyer**.
3. Open **Buy crops**, search the listing, click **Buy now**, set quantity/location and place the prototype order.
4. Open **My orders** to review the order and try the manual tracking selector.

## 4. Test the new trade-intelligence features

1. Sign in as a **Buyer** and open **Trade intelligence**.
2. Publish a demand request (crop, amount, quality, delivery location and optional budget).
3. View the match results and supply aggregation estimate. Sign in as a **Farmer** to compare available buyer demand against your listings.
4. Enter a distance to estimate a listing's landed price.
5. Submit a demo trust-review request to see the status change from `unverified` to `pending review (demo)`.
6. Change the language selector to English, Hindi, Punjabi, Bengali or Marathi.

## Configure AI for Trade Mitr

Edit `backend\.env`:

```dotenv
AI_PROVIDER=openai
OPENAI_API_KEY=your_actual_api_key
OPENAI_MODEL=gpt-4o-mini
FRONTEND_ORIGIN=http://localhost:5173
```

Restart the backend after editing `.env`. API use may incur charges. Never put the API key in frontend code or commit `.env`. Without a key, Trade Mitr uses its labelled demo assistant.

## Data and prototype limitations

- Accounts, password hashes, session tokens, crop listings, orders, demands and verification requests are held in backend memory. They reset when the backend process restarts. A database is needed for persistent storage.
- Passwords are hashed, but this is not production-grade authentication; there is no email verification, OTP, password recovery, rate limiting, or deployment hardening.
- **Market prices and weather are synthetic demo values**, not live mandi prices or forecasts. Irrigation guidance is generic and is not connected to live weather or soil sensors.
- **No payment is collected.** Orders are prototype records and do not legally confirm a sale, create escrow, book transport or provide real shipment tracking.
- Matching is rule-based and uses only entered listings; match percentages are heuristic scores, not validated AI confidence. Supply aggregation is an estimate and does not reserve inventory across sellers.
- Landed-price estimates depend on user-entered distance/rate and are not a carrier quote. The demand summary is not a trained forecast.
- Verification submissions are not real verification. Do not treat a profile as verified based on this prototype.
