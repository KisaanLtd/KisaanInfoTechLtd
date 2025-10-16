# KeltnerTrend Indicator - Analysis Values Reference

## 1. Daily Spatial Position Analysis (`calculateDailySpatialPosition()`)

| Variable | Type | Description | Value Range |
|----------|------|-------------|-------------|
| `dailyRangePosition` | Float | Current daily close position within previous day's range (%) | 0-100% |
| `isAbovePrevHigh` | Boolean | Daily close is above previous day's high | true/false |
| `isBelowPrevLow` | Boolean | Daily close is below previous day's low | true/false |
| `isWithinPrevRange` | Boolean | Daily close is within previous day's range | true/false |
| `isAbovePrevClose` | Boolean | Daily close is above previous day's close | true/false |
| `isBelowPrevClose` | Boolean | Daily close is below previous day's close | true/false |
| `isAtPrevClose` | Boolean | Daily close equals previous day's close | true/false |
| `distFromPrevClose` | Float | Distance from previous close (% of prev range) | -∞ to +∞ |
| `dailyPositionZone` | Integer | Categorized position zone | 1-5 |
| `hasBullishMomentum` | Boolean | Daily close > previous day's close | true/false |
| `hasBearishMomentum` | Boolean | Daily close < previous day's close | true/false |

### Position Zone Categories

| Zone | Value | Description |
|------|-------|-------------|
| Gap Up | 1 | Above previous high |
| Upper Range | 2 | Between prev close and prev high (>75% range) |
| Middle Range | 3 | Around 50% of range (25-75%) |
| Lower Range | 4 | Between prev low and prev close (<25% range) |
| Gap Down | 5 | Below previous low |

---

## 2. Previous Day Zone Boundaries

| Variable | Type | Description | Calculation |
|----------|------|-------------|-------------|
| `pdZoneMax` | Float | Upper boundary of prev day zone | `math.max(prevSessionVwap, dailyClose)` |
| `pdZoneMin` | Float | Lower boundary of prev day zone | `math.min(prevSessionVwap, dailyClose)` |

### Previous Day Zone vs SuperTrend Confluence

| Variable | Type | Description | Condition |
|----------|------|-------------|-----------|
| `st_InsidepdZone` | Boolean | SuperTrend contained within PD zone | `pdZoneMin < st_min AND pdZoneMax > st_max` |
| `pdZone_Inside_st` | Boolean | PD zone contained within SuperTrend | `st_max > pdZoneMax AND st_min < pdZoneMin` |
| `pdZone_Outside_aboveST` | Boolean | PD zone completely above SuperTrend | `st_max < pdZoneMin` |
| `pdZone_Outside_belowST` | Boolean | PD zone completely below SuperTrend | `st_min > pdZoneMax` |
| `pdZone_Outside` | Boolean | PD zone outside SuperTrend (either side) | `pdZone_Outside_aboveST OR pdZone_Outside_belowST` |
| `st_max_Inside_pdZone` | Boolean | ST max line inside PD zone | `st_max > pdZoneMin AND st_min < pdZoneMin` |
| `st_min_Inside_pdZone` | Boolean | ST min line inside PD zone | `st_min < pdZoneMax AND st_max > pdZoneMax` |

---

## 3. Band Confluence Analysis

### Keltner Channel vs SuperTrend Relationships

| Variable | Type | Description | Condition |
|----------|------|-------------|-----------|
| `kcInside_st` | Boolean | Keltner Channel inside SuperTrend bands | `kclower > st_min AND kcupper < st_max` |
| `stOutside_above_kc` | Boolean | SuperTrend completely above Keltner | `kclower > st_max` |
| `stOutside_below_kc` | Boolean | SuperTrend completely below Keltner | `kcupper < st_min` |
| `kcOutside` | Boolean | Keltner outside SuperTrend (either side) | `stOutside_below_kc OR stOutside_above_kc` |
| `stmin_inside_kc` | Boolean | ST min line inside Keltner Channel | `st_min > kclower AND st_max > kcupper` |
| `stmax_inside_kc` | Boolean | ST max line inside Keltner Channel | `st_max < kcupper AND st_min < kclower` |
| `bandConfluence` | Boolean | Any band confluence detected | `kcInside_st OR kcOutside OR stmin_inside_kc OR stmax_inside_kc` |

---

## Usage Notes

- **Daily Spatial Position**: Use to understand momentum and gap behavior relative to previous day
- **Previous Day Zone**: Identifies key support/resistance zones from previous session
- **Band Confluence**: Determines relationship between volatility bands for trend confirmation
- All boolean values return `true` when condition is met, `false` otherwise
- Percentage values are calculated relative to previous day's range
- Position zones help categorize market structure quickly