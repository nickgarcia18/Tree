# The Tree 🌱 → 🌳

A growing collection of daily stock picks — one per day, never disappearing. Vote bull or bear, leave comments (up to 200 characters), and watch the tree grow.

**Live:** https://nickgarcia18.github.io/tree/

---

## Features

✅ **One daily pick** — Automatically selects stocks with market cap $50B+, trading 18–25% below 52-week high  
✅ **Growing tree visualization** — Left/right alternating branches that persist forever  
✅ **Bull/Bear voting** — Vote on each pick (🐂 or 🐻)  
✅ **Comments** — Up to 200 characters per pick  
✅ **Never disappears** — All picks and votes stored persistently  
✅ **Fully free** — GitHub Pages + Supabase free tier  

---

## Quick Start (5 minutes)

### 1. Enable GitHub Pages

1. Go to your repo: `https://github.com/nickgarcia18/tree`
2. Click **Settings** → **Pages**
3. Under "Build and deployment":
   - Source: **Deploy from a branch**
   - Branch: **main** → **/ (root)**
4. Click **Save**
5. Wait ~2 minutes, your site is live at: `https://nickgarcia18.github.io/tree/`

### 2. Set Up Supabase (Free)

1. Go to [supabase.com](https://supabase.com)
2. Sign up (use GitHub login for instant setup)
3. Create new project:
   - Name: `tree`
   - Region: closest to you
   - Database password: generate one
4. Wait 2 minutes for initialization

### 3. Create Database Tables

In your Supabase project:

1. Click **SQL Editor** (left sidebar)
2. Click **New Query**
3. Paste and run this:

```sql
-- Pick votes table
CREATE TABLE pick_votes (
  pick_id TEXT PRIMARY KEY,
  bull_count INTEGER DEFAULT 0,
  bear_count INTEGER DEFAULT 0
);

-- Pick comments table
CREATE TABLE pick_comments (
  id BIGSERIAL PRIMARY KEY,
  pick_id TEXT,
  text TEXT,
  created_at TIMESTAMP DEFAULT now()
);

-- Enable public access
ALTER TABLE pick_votes ENABLE ROW LEVEL SECURITY;
ALTER TABLE pick_comments ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Enable all access for pick_votes" ON pick_votes 
  FOR ALL USING (true) WITH CHECK (true);

CREATE POLICY "Enable all access for pick_comments" ON pick_comments 
  FOR ALL USING (true) WITH CHECK (true);
```

### 4. Get Your API Keys

1. In Supabase, click **Settings** → **API** (left sidebar)
2. Copy these:
   - **Project URL** → `SUPABASE_URL`
   - **anon public** → `SUPABASE_ANON_KEY`

### 5. Update index.html

1. In your GitHub repo, click on `index.html`
2. Click the **✏️ edit** button
3. Find these lines (~line 385):

```javascript
const SUPABASE_URL = "YOUR_SUPABASE_URL";
const SUPABASE_ANON_KEY = "YOUR_SUPABASE_ANON_KEY";
```

4. Replace with your actual keys from Step 4
5. Scroll down and click **Commit changes...**
6. Add a message: "Add Supabase keys"
7. Click **Commit**

**Done!** Your site is now live with voting and comments enabled. 🎉

---

## How It Works

### Daily Picks
- Runs every hour
- Checks 12 related stocks (Uber, Lyft, DoorDash, Spot, etc.)
- Filters by: Market cap $50B+ & trading 18–25% below 52-week high
- Calculates 30-day recovery probability using binomial lattice model
- Picks the one with highest probability
- Never picks the same stock twice

### Voting
- Click 🐂 (Bull) or 🐻 (Bear) to vote
- Votes are counted and persisted in Supabase
- All users see the same vote counts

### Comments
- Write up to 200 characters per pick
- Character counter shows progress
- Comments display newest first
- Stored in Supabase

### Storage
- **Picks**: Stored locally in browser (never expire)
- **Votes & Comments**: Stored in Supabase (synced across all users)

---

## Stock Selection Criteria

**Roots** (initial 4 seeds):
- UBER, WM, MCD, KO

**Related stocks** (auto-discovered daily):
- Rideshare/delivery: LYFT, DASH, SPOT
- Waste management: RSG, GFL, WSTE
- Restaurants/QSR: YUM, CMG, DRI
- Beverages/staples: PEP, MO, DEO

Each day, the algorithm:
1. Checks all related stocks
2. Filters for market cap > $50B
3. Filters for trading 18–25% below 52-week high
4. Calculates recovery probability (30-day model)
5. Picks the highest probability candidate

---

## Model Details

The recovery probability uses a **binomial lattice model**:
- **Volatility**: Fixed at 35% for all stocks (illustrative)
- **Drift**: 0% (neutral)
- **Target**: 95% of 52-week high
- **Timeframe**: 30 days

⚠️ **This is illustrative only, not a real forecast.** All stocks vary in actual volatility, and past patterns don't predict future results.

---

## Customization

### Change Root Stocks
Edit line ~378:
```javascript
const ROOT_TICKERS = ['UBER','WM','MCD','KO'];
```

### Add More Related Stocks
Edit line ~384:
```javascript
const INDUSTRY_MAPPING = {
  'UBER': ['LYFT', 'DASH', 'SPOT'],
  // Add more here
};
```

### Modify Comment Limit
Edit line ~373:
```javascript
const COMMENT_CHAR_LIMIT = 200;  // Change to desired limit
```

### Change Donation Links
Edit lines ~195–197 in the HTML (search for "buymeacoffee" and "paypal")

---

## Troubleshooting

### Voting/Comments Not Working
- Check that Supabase keys are correct in `index.html`
- Verify SQL tables were created successfully
- Check browser console for errors (F12 → Console)

### No Picks Showing Up
- Check that Finnhub API is working (it has free tier, included)
- Verify market conditions match criteria (need stocks $50B+ down 18–25%)
- Check browser console for fetch errors

### GitHub Pages Not Live
- Wait 2–3 minutes after enabling
- Hard refresh: `Ctrl+Shift+R` (or `Cmd+Shift+R` on Mac)
- Check repo Settings → Pages to confirm deployment status

---

## Free Tier Limits

| Service | Limit | Enough For |
|---------|-------|-----------|
| **GitHub Pages** | Unlimited | Your entire site |
| **Supabase** | 500K API calls/month | ~16K calls/day ✅ |
| **Finnhub** | 60 calls/minute | Daily picks ✅ |

---

## License

MIT — Feel free to fork, modify, and share.

---

## Support

- Report issues: [GitHub Issues](https://github.com/nickgarcia18/tree/issues)
- Questions: Check the code comments
- Ideas: Submit a PR or discussion

---

## Disclaimer

**For informational purposes only.** Not investment advice. Selection based on mechanical criteria (market cap & pullback %), not analysis of fundamentals, news, or future performance. The model is illustrative only and not a real forecast. Past patterns don't predict future results.

---

Built with ❤️ using GitHub Pages + Supabase + Finnhub API
