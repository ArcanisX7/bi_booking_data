# Business Analytics Task

Analysis of a booking data dump from a travel agency. The task was to build a short management overview, show the most important dimensions in charts, and answer four business questions.

## Data

The source file is `Excel_test__Business_Analytics.xlsx`, sheet `raw_data` (about 37,000 rows). It is synthetic data with pre-aggregated bookings.

Dimensions: Market (CZ, HU, PL, SK), Channel (Online, Offline), Departure Season (Summer, Winter), Departure Year, Booking Month, Passenger Group (7 groups), Destination Type (4 types).
Metrics: Bookings, AOV (average order value), GBV (gross booking value).

## Structure

```
notebook.ipynb        full pipeline: cleaning, analysis, charts
Excel_test__Business_Analytics.xlsx
index.html            overview tables
dimensions.html       charts
qa.html               the four questions and answers
```

## Method

**Cleaning.** The file contains subtotal rows. All rows with `Total`, empty values, or zero bookings are removed, so only the lowest level of detail is left.

**AOV.** AOV is always computed as GBV divided by Bookings at each level. It is never averaged, because an average of averages gives wrong numbers on subtotals.

**First Minute.** The data has no booking date, only the booking month and the departure year, so the flag is built from the gap between them:

- Summer: booked in an earlier calendar year, or in January to March of the departure year
- Winter: booked two or more years earlier, or in August to November of the previous year

**Incomplete data.** The file ends in November 2025, and that month is not complete. Every year-on-year comparison therefore uses the same window in both years (January to October, or October alone). Full calendar years are not compared, because November and December carry the highest First Minute share and would make 2025 look weaker than it is. The years 2022 and 2023 are excluded as well, because the business was still starting up.

**Choosing the important dimensions.** Eta-squared shows how much of the variation in AOV each dimension explains. Passenger Group (39 %) and Destination Type (30 %) are far ahead of Market (4 %), Channel (3 %) and Departure Season (0 %).

## Main findings

1. **AOV** is driven by who travels and where they go. Passenger Group and Destination Type explain most of the variation. Part of the passenger effect is order size, not price, because AOV is value per booking and the data has no passenger counts.
2. **GBV** follows volume, not price. Czechia and Poland together make about 74 % of GBV, and Families alone 48 %. Order value shifts the mix only at the margins.
3. **First Minute** is stable, not falling. On the same January to October window it holds 24.1 % of bookings in 2025 against 24.2 % in 2024. Its value is higher than the rest, €2,419 against €2,009, and that premium does not change year on year.
4. **Growth potential** is highest in Poland. It has the lowest market penetration together with the largest population, and it already grows above the company average. Its weak points are the lowest AOV in the portfolio and growth that comes mostly from the offline channel.

## Note

Population figures used for the market penetration comparison are not part of the dataset. They come from public statistics and are only a rough proxy for market size.
