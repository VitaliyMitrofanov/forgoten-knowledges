# Clear zabiix database before migration

````
DELETE FROM alerts WHERE age(to_timestamp(alerts.clock)) > (:7 * INTERVAL '1 day');
 
DELETE FROM acknowledges WHERE age(to_timestamp(acknowledges.clock)) > (14 * INTERVAL '1 day');
 
DELETE FROM events WHERE age(to_timestamp(events.clock)) > (14 * INTERVAL '1 day');
 
DELETE FROM history WHERE age(to_timestamp(history.clock)) > (14 * INTERVAL '1 day');
DELETE FROM history_uint WHERE age(to_timestamp(history_uint.clock)) > (14 * INTERVAL '1 day') ;
DELETE FROM history_str WHERE age(to_timestamp(history_str.clock)) > (14 * INTERVAL '1 day') ;
DELETE FROM history_text WHERE age(to_timestamp(history_text.clock)) > (14 * INTERVAL '1 day') ;
DELETE FROM history_log WHERE age(to_timestamp(history_log.clock)) > (14 * INTERVAL '1 day') ;
 
DELETE FROM trends WHERE age(to_timestamp(trends.clock)) > (180 * INTERVAL '1 day');
DELETE FROM trends_uint WHERE age(to_timestamp(trends_uint.clock)) > (180 * INTERVAL '1 day') ;
````
