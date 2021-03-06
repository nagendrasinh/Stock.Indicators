﻿# Moving Average Convergence/Divergence (MACD)

MACD is a simple oscillator view of two converging/diverging exponential moving averages.
[More info ...](https://school.stockcharts.com/doku.php?id=technical_indicators:moving_average_convergence_divergence_macd)

![image](chart.png)

```csharp
// usage
IEnumerable<MacdResult> results = Indicator.GetMacd(history, fastPeriod, slowPeriod, signalPeriod);  
```

## Parameters

| name | type | notes
| -- |-- |--
| `history` | IEnumerable\<[Quote](../../docs/GUIDE.md#quote)\> | Historical Quotes data should be at any consistent frequency (day, hour, minute, etc).  You must supply at least 2 × slow period + signal period worth of `history`.  Since this uses a smoothing technique, we recommend you use at least 250 data points prior to the intended usage date for maximum precision.
| `fastPeriod` | int | Number of periods (`F`) for the faster moving average.  Must be greater than 0.  Default is 12.
| `slowPeriod` | int | Number of periods (`S`) for the slower moving average.  Must be greater than 0 and greater than `fastPeriod`.  Default is 26.
| `signalPeriod` | int | Number of periods (`P`) for the moving average of MACD.  Must be greater than or equal to 0.  Default is 9.

## Response

```csharp
IEnumerable<MacdResult>
```

The first `S-1` slow periods + signal period will have `null` values since there's not enough data to calculate.  We always return the same number of elements as there are in the historical quotes.

### MacdResult

| name | type | notes
| -- |-- |--
| `Date` | DateTime | Date
| `Macd` | decimal | The MACD line is the difference between slow and fast moving averages
| `Signal` | decimal | Moving average of the `MACD` line
| `Histogram` | decimal | Gap between of the `MACD` and `Signal` line

## Example

```csharp
// fetch historical quotes from your favorite feed, in Quote format
IEnumerable<Quote> history = GetHistoryFromFeed("SPY");

// calculate MACD(12,26,9)
IEnumerable<MacdResult> results = Indicator.GetMacd(history,12,26,9);

// use results as needed
DateTime evalDate = DateTime.Parse("12/31/2018");
MacdResult result = results.Where(x=>x.Date==evalDate).FirstOrDefault();
Console.WriteLine("MACD on {0} was ${1}", result.Date, result.Macd);
```

```bash
MACD on 12/31/2018 was -6.22
```
