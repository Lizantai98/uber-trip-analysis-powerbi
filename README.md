# Uber Trip Analysis | Power BI Dashboard
A four-page Power BI dashboard analysing 103,024 Ola and Uber ride bookings from July 2024. It answers four questions: how many bookings become rides, how demand moves through the month, what completed rides look like, and why bookings fail.

## Business questions
- How many bookings turn into completed rides?
- How does demand change across the month?
- What do completed rides look like (distance, value, payment, completion)?
- Why do bookings fail, and how much booking value is lost?

## Dataset
- Source: Ola & Uber: Ride Booking & Cancellation Data (Kaggle)
- Size and period: 103,024 bookings, 1 to 31 July 2024
- Fields used: date, time, booking status, vehicle type, pickup and drop locations, booking value, ride distance, payment method, ratings, and cancellation and incomplete-ride reasons

## Tools
Power BI Desktop: Power Query, DAX and data modelling.

## Data preparation
- Removed an empty column and an unusable "Vehicle Images" column
- Converted the text value "null" into real blanks and set the correct data types
- Added Trip_Period (Day = 6am to 6pm, otherwise Night) and Day Name
- Built a Calendar table linked to the booking date

## Data Model
One bookings table linked to a Calendar table on date, plus a small helper table that feeds the booking funnel.

## Selected DAX measures
Value and distance measures count completed ("Success") rides only, so cancelled bookings do not inflate them.

- Total Bookings = COUNTROWS('Uber_Analysis')

- Total Pickups = CALCULATE([Total Bookings], 'Uber_Analysis'[Booking_Status] = "Success")

- Total Booking Value = CALCULATE(SUM('Uber_Analysis'[Booking_Value]), 'Uber_Analysis'[Booking_Status] = "Success")

- Cancelled Bookings = [Total Bookings] - [Total Pickups] - [Driver Not Found]

- Cancellation Rate = DIVIDE([Cancelled Bookings], [Total Bookings])

- Value per Distance Unit = DIVIDE([Total Booking Value], [Total Trip Distance])

- Rolling 7-Day Avg = AVERAGEX(DATESINPERIOD(Calendar[Date], MAX(Calendar[Date]), -7, DAY), [Total Bookings])

## Key findings
### Overview

<img width="1439" height="808" alt="image" src="https://github.com/user-attachments/assets/58975963-9603-4ce3-b5ec-2612c373cf31" />

- Only 62.1% of bookings become rides (63,967 of 103,024). 28.1% are cancelled and 9.8% never get a driver.
- 58.3% of all bookings (60,041) finish with no incident.
- Completed bookings are worth 35.1M. Cash (19.3M) and UPI (14.2M) make up about 95% of it.
- Demand is spread evenly across the city: the top 10 pickup areas each have 2,100 to 2,201 bookings.

### Demand Trends

<img width="1447" height="805" alt="image" src="https://github.com/user-attachments/assets/f92aa5d1-81fd-4dc2-8854-29283e84d869" />

- Demand is steady at about 3,323 bookings a day, ranging from 3,072 to 3,432. The peak is 30 July and the lowest day is 31 July.
- Weekdays average 3,333 and weekends 3,294. There is a mild Sunday dip: three of the five quietest days are Sundays.
- Day and night look almost identical in volume (51,817 vs 51,207) and in success rate (61.9% vs 62.2%).

### Ride Insights

<img width="1440" height="805" alt="image" src="https://github.com/user-attachments/assets/33be944e-042f-4f83-afb3-527142e056a2" />

- 93.9% of confirmed rides complete. The 3,926 incomplete rides come from customer demand (1,601), vehicle breakdowns (1,591) and other issues (734).
- Autos run much shorter rides (about 10 distance units against about 25 for other vehicles) at a similar average booking value, so they earn 54.9 per distance unit against 21.6 to 22.3 for the rest.
- Ratings are uniform at 4.0. Cash accounts for the most trip distance (801,896), followed by UPI (590,318).

### Cancellations

<img width="1437" height="808" alt="image" src="https://github.com/user-attachments/assets/5630a92f-f494-48e9-b478-b229e779f922" />

- 38% of total booking value (21.5M of 56.5M) never becomes a ride: 10.2M from driver cancellations, 5.8M from customer cancellations and 5.5M from bookings with no driver found.
- Driver cancellations (17.9% of bookings) outnumber customer cancellations (10.2%). The top driver reason is personal and car-related issues (6,542).
- 56% of customer cancellations (5,845 of 10,499) cite driver behaviour: the driver not moving towards the pickup (3,175) or asking the customer to cancel (2,670).
- Cancellation rates are similar across vehicle types (about 27% to 28%) and across days (860 to 1,000 a day).

## Notes
- Currency and time units (driver acceptance and customer boarding times) are not specified in the source data, so values are shown without symbols.
- The data covers one month, so seasonal trends cannot be assessed.
- Demand by hour, vehicle mix and ratings are nearly uniform, which is typical of a generated practice dataset. No conclusions are drawn from them.

## Author
Elizabeth Antai, Data Analyst | www.linkedin.com/in/elizabeth-antai/ | github.com/Lizantai98
