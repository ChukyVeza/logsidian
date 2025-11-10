```tasks 
not done 
sort by priority 
sort by due 
sort by scheduled 
sort by starts 
group by function task.happens.moment ? (task.due.moment?.isBefore(moment(), 'day') ? task.due.format("[%%0%% Overdue]") : (task.due.moment?.isBetween(moment(), moment(), 'day', '[]') ? task.due.format("[%%1%% Due Today]") : (task.happens.moment?.isBefore(moment().add(1, 'day'), 'day') ? task.happens.format("[%%2%% Scheduled Today]") : (task.happens.moment?.isBefore(moment().add(7, 'days'), 'days') ? task.happens.format("[%%3%% Upcoming]") : task.happens.format("[%%4%% Beyond Seven Days]"))))) : "No Date" 
group by function reverse task.scheduled.format("%%%") limit groups 3 
hide task count 
```
