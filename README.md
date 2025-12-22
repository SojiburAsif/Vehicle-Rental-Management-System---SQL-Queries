-- Query 1: JOIN
select
  booking_id,
  name as customer_name,
  vehicle_name,
  start_date,
  end_date,
  booking_status as status
from
  bookings b
  inner join users u on b.user_id = u.user_id
  inner join vehicles v on b.vehicle_id = v.vehicle_id;

-- Query 2: EXISTS
SELECT *
FROM vehicles v
WHERE NOT EXISTS (
  SELECT 1
  FROM bookings b
  WHERE b.vehicle_id = v.vehicle_id
);
-- Query 3: WHERE

select * from vehicles 
where type = 'car' and status = 'available'



  
-- Query 4: GROUP BY and HAVING
SELECT 
  v.vehicle_name,
  COUNT(b.booking_id) AS total_bookings
FROM bookings b
JOIN vehicles v 
  ON b.vehicle_id = v.vehicle_id
GROUP BY v.vehicle_name
HAVING COUNT(b.booking_id) > 2;
