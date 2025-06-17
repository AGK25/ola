# 🚖 Ola Ride Booking Analysis — Power BI Project

## 📌 Project Overview
This Power BI project visualizes and analyzes Ola ride booking data for **July 2024**. It aims to identify trends in ride volumes, cancellations, revenue generation, and customer behavior, helping stakeholders make data-driven decisions.

---

## 🛠️ Tools & Technologies
- 📊 **Power BI** – Interactive dashboard creation
- 🗃️ **PostgreSQL** – SQL queries for data cleaning and transformation
- 📂 **Microsoft Excel** – Preprocessing and validation of input data

---

## 🗂️ Dashboard Navigation & Insights

### ✅ 1. Overall Summary
- 📆 **Date Filter**: Interactive slicer for July 2024
- 🔢 **Total Bookings (Sample)**: 103,024
- 📈 **Estimated Monthly Bookings**: 35 Million (scaled projection)
- 🥧 **Booking Status Breakdown**:
  - ✅ Success: 62%
  - ❌ Canceled by Driver: ~18%
  - ❌ Canceled by Customer: ~10%
  - 🚫 Driver Not Found: ~9%
- 📉 **Ride Volume Over Time**: Line chart showing daily booking activity

### 🚗 2. Vehicle Type
- Analysis by vehicle types (Auto, Sedan, Mini, Prime, etc.)
- Usage distribution across date range

### 💰 3. Revenue
- 📊 **Revenue by Payment Method**:
  - Cash leads, followed by UPI, Credit & Debit Cards
- 🧾 **Top Customers by Spend**: Table showing top 5 high-value riders
- 📆 **Revenue Trend by Day**: Daily revenue visualization

### ❌ 4. Cancellation Analysis
- Breakdown of ride cancellations with filters
- Identification of peak cancellation days and responsible parties

### ⭐ 5. Ratings (Assumed)
- Placeholder page for customer satisfaction and feedback trends

---

## 🧾 SQL Queries Used

```sql
-- 1. Clean and filter July data
SELECT 
  booking_id, ride_date, customer_id,
  booking_status, payment_method,
  vehicle_type, revenue_value, ride_distance_km
FROM raw_bookings
WHERE ride_date BETWEEN '2024-07-01' AND '2024-07-31';

-- 2. Calculate total successful bookings
SELECT COUNT(*) AS total_bookings
FROM bookings_clean
WHERE booking_status = 'Success';

-- 3. Revenue grouped by payment method
SELECT payment_method, SUM(revenue_value) AS total_revenue
FROM bookings_clean
GROUP BY payment_method;

-- 4. Top 5 customers by revenue
SELECT customer_id, SUM(revenue_value) AS total_spent
FROM bookings_clean
GROUP BY customer_id
ORDER BY total_spent DESC
LIMIT 5;

--5. Get the number of rides cancelled by drivers due to personal and car-related issues:
Create View Rides_cancelled_by_Drivers_P_C_Issues As
SELECT COUNT(*) FROM bookings
WHERE canceled_rides_by_driver  = 'Personal & Car related issue';


--6. Find the maximum and minimum driver ratings for Prime Sedan bookings:
Create View Max_Min_Driver_Rating As
SELECT MAX(Driver_Ratings) as max_rating,
MIN(Driver_Ratings) as min_rating
FROM bookings WHERE Vehicle_Type = 'Prime Sedan';



--7. Retrieve all rides where payment was made using UPI:
Create View UPI_Payment As
SELECT * FROM bookings
WHERE Payment_Method = 'UPI';





--8. Find the average customer rating per vehicle type:

Create View AVG_Cust_Rating As
SELECT Vehicle_Type, AVG(Customer_Rating) as avg_customer_rating
FROM bookings
GROUP BY Vehicle_Type;

--9. Calculate the total booking value of rides completed successfully:
Create View total_successful_ride_value As
SELECT SUM(Booking_Value) as total_successful_ride_value
FROM bookings
WHERE Booking_Status = 'Success';




--10. List all incomplete rides along with the reason:
Create View Incomplete_Rides_Reason As
SELECT Booking_ID, Incomplete_Rides_Reason
FROM bookings
WHERE Incomplete_Rides = 'Yes';
```

---

## 📌 Key Insights
- ✅ A strong majority of rides are completed successfully.
- ❌ Driver cancellations are a major reason for service disruption.
- 💵 Cash continues to be the most preferred payment mode.
- 📊 Ride and revenue trends show stable daily patterns with minor fluctuations.

---

## 📁 Project Files Included
- `ola project.pbix` – The full Power BI dashboard


---

## 👤 Author
**Garv Kukreja**  
🎓 Aspiring Data Analyst | 📈 Passionate about Data Visualization and Urban Mobility Analytics  
📬 Feel free to connect for collaboration or feedback!

---

✅ *This project can be extended by integrating user rating data, geospatial ride mapping, or time-to-completion metrics for a deeper operational insight.*
