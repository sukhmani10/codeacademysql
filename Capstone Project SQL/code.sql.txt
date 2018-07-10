Capstone Project: Code 

SELECT DISTINCT utm_campaign as 'UTM_Campaign'
FROM page_visits;

SELECT DISTINCT utm_source as 'UTM_Source'
FROM page_visits;

SELECT DISTINCT utm_campaign as 'UTM_Campaign', utm_source as 'UTM_Source'
FROM page_visits;

SELECT DISTINCT page_name as 'Pages'
FROM page_visits;
https://gist.github.com/3df9dd1564d00344b721a63ccb4d04ff
WITH first_touch AS (
    SELECT user_id,
        MIN(timestamp) as first_touch_at
    FROM page_visits
    GROUP BY user_id)
SELECT 
    pv.utm_campaign, 
    COUNT (ft.first_touch_at) as Num_First_Touches
FROM first_touch ft
JOIN page_visits pv
    ON ft.user_id = pv.user_id
    AND ft.first_touch_at = pv.timestamp
GROUP BY utm_campaign
ORDER BY 2 desc;
https://gist.github.com/a4bc336151753accf26b843b1bbd0274
WITH last_touch AS (
    SELECT user_id,
        MAX (timestamp) as last_touch_at
    FROM page_visits
    GROUP BY user_id)
SELECT 
    pv.utm_campaign, 
    COUNT (lt.last_touch_at) as Num_Last_Touches
FROM last_touch lt
JOIN page_visits pv
    ON lt.user_id = pv.user_id
    AND lt.last_touch_at = pv.timestamp
GROUP BY utm_campaign
ORDER BY 2 desc;


SELECT DISTINCT COUNT (user_id) as Purchases
from page_visits
WHERE page_name is '4 - purchase';

WITH last_touch AS (
    SELECT user_id,
        MAX (timestamp) as last_touch_at
    FROM page_visits
    GROUP BY user_id)
SELECT 
    pv.utm_campaign, 
    COUNT (lt.last_touch_at) as Num_Last_Touches
FROM last_touch lt
JOIN page_visits pv
    ON lt.user_id = pv.user_id
    AND lt.last_touch_at = pv.timestamp
WHERE pv.page_name is '4 - purchase'
GROUP BY 1 
ORDER BY 2 desc; 

