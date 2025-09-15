# Data-analysis-SQL-Aggregation-Data-with-View-
Aggregation Data with View in SQL

/*create view `data-analytics-mate.Students.aggregation_data_Brovko1` as
Select es.id_account,
date_trunc(date_add(s.date, interval es.sent_date day), month) as sent_month,
min(date_add(s.date, interval es.sent_date day)) over (partition by account_id, date_trunc(date_add(s.date, interval es.sent_date day), month)) as first_sent_date,
max(date_add(s.date, interval es.sent_date day)) over (partition by account_id, date_trunc(date_add(s.date, interval es.sent_date day), month)) as last_sent_date
from DA.email_sent es
join DA.account_session acs
on es.id_account = acs.account_id
join DA.session s
on acs.ga_session_id = s.ga_session_id*/

/*create view `data-analytics-mate.Students.aggregation_data_Brovko2` as
select sent_month,
id_account,
count(*) as sent_msg_from_month,
first_sent_date,
last_sent_date
from `data-analytics-mate.Students.aggregation_data_Brovko1`
group by sent_month, id_account, first_sent_date, last_sent_date*/

/*create view `data-analytics-mate.Students.aggregation_data_Brovko3` as
select sent_month,
id_account,
count(*) as sent_msg_from_month,
first_sent_date,
last_sent_date
from `data-analytics-mate.Students.aggregation_data_Brovko2`
group by sent_month, id_account, first_sent_date,
last_sent_date*/

/*create view `data-analytics-mate.Students.aggregation_data_Brovko4` as
select sent_month,
id_account,
sent_msg_from_month,
sum(sent_msg_from_month) over (partition by sent_month) as total_sent_msg_from_month,
first_sent_date,
last_sent_date
from `data-analytics-mate.Students.aggregation_data_Brovko3`*/

select sent_month,
id_account,
round(sent_msg_from_month *100/ total_sent_msg_from_month, 2)  as sent_msg_percent_from_this_month,
first_sent_date,
last_sent_date
from `data-analytics-mate.Students.aggregation_data_Brovko4`
order by sent_month desc
