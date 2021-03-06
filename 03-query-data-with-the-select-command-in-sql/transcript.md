With data inner table the quickest way to pull it all out is to write `select * from Users;`. As you can see, we have two rows of data instead of our `Users` table. 

```sql 
select * from Users; 
  create_date |             user_handle              | last_name | first _name 
--------------+--------------------------------------+-----------+-------------
  2018-06-06  | a0eebc99-9c0b-42f8-bb6d-6bb9bd380a11 | clark     | tyler  
  2018-06-06  | a0eebc99-9c0b-42f8-bb6d-6bb9bd380a11 | jones     | debbie  
(2 rows )

```

One of the most common tasks when working with SQL databases is to query the data from the tables using this `select` statement. The select statement is very powerful and has lots of clauses and functions that can be used with it. At a minimum, all that is needed is what columns we want to work with and from which table these columns exist on. The `*` states that we want all the columns within the defined table. We can also `select` out specific columns by comma separating them.

```sql 
select first_name, last_name from Users;
  first_name  | last_name 
  ------------+-----------
  tyler       | clark
  debbie      | jones
(2 rows) 

```

If we ask for a column that does not exist from our table, we're going to see an error. If we ask for a `middle_name` from our user's table we'll see that the column `middle_name` doesn't exist in our table. 

```sql 
select first_name, last_name, middle_name from Users;
ERROR: column "middle_name" does not exist 
LINE 1: select first_name, last_name, middle_name from Users;


```

However, SQL databases such as SQLite, MySQL, and Postgres have open functions. We can use one of these functions within the query and our database will treat it as a column and input a value within each row. Even though `current_time` is not a column within the user's table, it is a built-in function within Postgres so it's handling it for us.

```sql 
select first_name, last_name, current_time from Users;
  first_name  | last_name |     timetz
  ------------+-----------+------------------
  tyler       | clark     | 22:41:42.936252-07
  debbie      | jones     | 22:41:42.936252-07
(2 rows) 

```

We can also alias our columns within the query. If we add, say, `select first_name as FirstName, last_name as LastName, current_time as time from Users;`, when we run this query we'll see that we've changed the way our column names have rendered. 

```sql 
select first_name as FirstName, last_name as LastName, current_time as time from Users;
  firstname   | lastname  |     time
  ------------+-----------+------------------
  tyler       | clark     | 22:41:42.936252-07
  debbie      | jones     | 22:41:42.936252-07
(2 rows) 

```

This is critical to know when working with packages within languages like C#. We need column names to match properties of classes. One common function you'll probably use a lot when working with SQL is the `count()` function which simply returns the number of rows within our table. 

```sql 
select count(*) from Users;
  count 
--------
      2
(1 row)
select count(first_name) from Users;
  count 
--------
      2
(1 row)

```

It doesn't matter if you use the `*` or any combination of columns as long as they exist within the table it's going to give us a count of rows. Finally, we have the ability to do some filtering of data when pulling it out at the column level. Let's `insert` another set of data where the `first_name` and `last_name` matches data that already exists. For `(first_name, last_name)`, we're going to insert `('tyler', 'clark')`.

```sql 
insert into Users (first_name, last_name) values ('tyler', 'clark');
INSERT 0 1
$ postgres #
```

If we `select * from Users;` table we'll find that we have two rows that have the matching first name and last name of Tyler Clark. 
 
```sql 
select * from Users; 
  create_date |             user_handle              | last_name | first _name 
--------------+--------------------------------------+-----------+-------------
  2018-06-06  | a0eebc99-9c0b-42f8-bb6d-6bb9bd380a11 | clark     | tyler  
  2018-06-06  | a0eebc99-9c0b-42f8-bb6d-6bb9bd380a11 | jones     | debbie  
              |                                      | clark     | tyler  
(3 rows )

```

If we use the built-in `select distinct` function on the `(first_name)` column of the user's table we'll see that we only get two rows back. It's removing the duplicate, Tyler, from the table. 

```sql 
select distinct (first_name) from Users;
  first_name 
--------
 tyler 
 debbie
(2 row)

```
The same thing can be done for `(last_name)`. 

```sql 
select distinct (last_name) from Users;
  first_name 
--------
 clark 
 jones
(2 row)

```

What's great is SQL gives us the ability to combine functions together so we can get a count of all the distinct `last_names` within our user's table, which is two and compare that against all the rows within our user's table, which is three.

```sql 
select count (distinct(last_name)) from Users;
  count 
--------
     2
 (1 row)
select count (last_name) from Users;
  count 
--------
     3
 (1 row)
```