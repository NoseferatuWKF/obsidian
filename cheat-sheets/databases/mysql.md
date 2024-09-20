connect to db
```bash
# use password from stdin
mysql -u <user> -p
```

case when column matches with any from another table
```SQL
select distinct t1.N,
case
    when t1.P is NULL then 'Root'
    when t2.P is not NULL then 'Inner'
    else 'Leaf'
end
from BST t1
left join BST t2 on t1.N = t2.P
order by t1.N;

-- another way
select t1.N
case
	when t1.P is NULL then 'Root'
	when exists (select 1 from BST t2 where t1.N = t2.P) then 'Inner'
	else 'Leaf'
from BST t1
order by t1.N;
```
