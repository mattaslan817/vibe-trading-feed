# Vibe-Trading paper feed: `latest.json` schema (v1.0)

Paper trading only. Not investment advice. Backtests and paper results don't promise future returns.

One JSON file, regenerated from the paper ledgers after each scheduled check (pre-market, ~10:38, ~14:30, after the close; all ET). Prices are the last close or quote recorded in the ledgers, not live quotes. Missing values are `null`. Percentages are in percent units (0.708 means +0.708%). Money is USD.

| Field | Type | Meaning |
|---|---|---|
| `schema_version` | string | "1.0" |
| `generated_at` | string | ISO-8601 time with America/New_York offset, e.g. `2026-10-26T17:20:00-04:00` |
| `as_of_date` | string | Last trading day in the equity ledger (YYYY-MM-DD) |
| `disclaimer` | string | Show prominently |
| `paper_only` | bool | Always `true` |
| `rules_version` | string | Strategy rules version, e.g. `v0.5.1` |
| `account` | object | `start_date`, `start_value`, `value`, `return_pct`, `pnl`, `cash`, `open_positions`, `realized_pnl`, `valuation_note` |
| `benchmark` | object | SPY buy-and-hold from the same start: `name`, `start_date`, `start_price`, `method`, `value`, `return_pct`, `account_minus_spy_pct` |
| `equity_curve[]` | array | One row per trading day (oldest first): `date`, `total`, `spy_equivalent`, `total_return_pct`, `spy_return_pct`, `cash`, `open_positions`, `sleeves{<name>: {equity, return_pct, spy_equivalent, spy_return_pct}}`. The day-0 point is `account.start_value` for both lines on `account.start_date` before the open. |
| `sleeves[]` | array | Scoreboard: `sleeve`, `start_value`, `equity`, `return_pct`, `spy_return_pct`, `vs_spy_pct`, `invested_value`, `idle_spy_value`, `cash`, `open_positions`, `realized_pnl`, `base_weight`, `experimental`, `note` |
| `g1_gate` | object | Core market-trend gate: `state` ("IN (SPY)" or out), `invested`, `as_of_month`, `spx`, `spx_10m_avg`, `baa10y_month_avg`, `baa10y_12m_avg`, `rule` |
| `positions[]` | array | Open lots: `ticker`, `sleeve`, `lot_id`, `status` ("open" or "idle_spy"), `is_idle_cash_in_spy`, `entry_date`, `entry_time_et`, `shares`, `entry_price`, `last_price`, `cost_basis`, `market_value`, `unrealized_pnl`, `pnl_pct`, `stop_price`, `target_price`, `exit_by_date`, `report_date`, `signal_type`, `signal_link`, `reason` |
| `closed_trades[]` | array | Newest first: `ticker`, `sleeve`, `lot_id`, `entry_date`, `entry_price`, `exit_date`, `exit_price`, `shares`, `pnl`, `return_pct`, `fees`, `days_held`, `exit_reason`, `signal_type`, `signal_link` |
| `todays_fills[]` | array | Fills on `as_of_date`: `date`, `time_et`, `sleeve`, `ticker`, `side`, `shares`, `quote_price`, `fill_price`, `notional`, `fee`, `price_source`, `reason` |
| `recent_fills[]` | array | Last 100 fills, newest first, same fields |
| `last_fill_date` | string | Date of the newest fill |
| `pending_orders[]` | array | Queued paper orders: `trade_date`, `fill_at`, `sleeve`, `ticker`, `side`, `dollars`, `shares`, `reason` |
| `skips[]` | array | Rule-based skips, newest first: `date`, `logged_et`, `sleeve`, `ticker`, `rule`, `detail` |
| `signals[]` | array | Last 50 logged signals, newest first: `date`, `ticker`, `signal_type`, `source`, `sources[{url}]`, `description`, `sentiment`, `prior_close`, `ret_1d`, `ret_5d`, `ret_20d` |
| `shadow_tests[]` | array | Shadow (not-traded) setups: `setup`, `trades`, `open_or_pending`, `win_rate_pct`, `net_per_trade_pct`, `total_net_pct_sum`, `spy_same_windows_pct_sum`, `t_stat`, `weeks`, `weeks_positive`, `pass_bar`, `status` |
| `f50_shadow[]` | array | Test F50 shadow trades (no money; v0.5.5): `tier` (F50 scored, F50x evidence only), `ticker`, `signal_date`, `status`, `entry_date`, `exit_date`, `exit_reason`, `sources`, `entry_open`, `last_close`, `exit_price`, `stop`, `target`, `notional_usd`, `net_pct`, `spy_pct`, `excess_pct`, `pnl_usd`, `pct_below_50d`. F50 rows in `shadow_tests[]` also carry `start_date`, `pnl_usd`, `spy_pnl_usd`, `avg_excess_vs_spy_pct` |
| `alerts[]` | array | Last 150 alert lines from the last 5 alert logs, newest first: `date`, `text` |
| `latest_reflection` | object | `date`, `markdown` (the daily end-of-day reflection) |
