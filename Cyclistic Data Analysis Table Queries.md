/* Analysis Tables */

/* Table 1
Count of member & casual memberships
*/

SELECT member_casual, count (member_casual)
FROM  organized_cyclistic_data
GROUP BY
member_casual

/* Table 2
Monthly count/ Membership type
*/

SELECT member_casual, start_month, count (*)
FROM organized_cyclistic_data
GROUP BY
member_casual, start_month
ORDER BY
member_casual ASC

/* Table 3
Start day/ Membership type
*/

SELECT member_casual, start_day, count(*)
FROM organized_cyclistic_data
GROUP BY member_casual, start_day
ORDER BY member_casual


/* Table 4
Calculation of Average duration in minutes.
*/

SELECT member_casual, start_day, count(*), round(Avg(duration_in_min),2)
FROM organized_cyclistic_data
GROUP BY member_casual, start_day
ORDER BY member_casual





