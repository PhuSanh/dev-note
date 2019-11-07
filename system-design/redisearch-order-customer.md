# Redis search order customer

## 1. Context
There are 2 tables: order and customer
Order: id, customer_id,...
Customer: id, email, phone, last_name, first_name,...

## 2. Issue
We want to get (search) orders that have customer name, phone, email `LIKE` something

## 3. Normal approach
```
SELECT * 
FROM order AS o
JOIN customer AS c
ON o.customer_id = c.id
WHERE c.phone LIKE "%q%" OR c.email LIKE "%q%" OR c.last_name LIKE "%q%" OR c.first_name LIKE "%q%"
```
#### Pros:
- Easy to understand and code
- Do not need anything else except MySQL

#### Cons:
- Can not use index - because we use LIKE with the `%` in the beginning
- Maybe slow if there are too much records
- Should not use `JOIN` - performance issue too

## 4. Redis approach
We will store customer data in redis with this pattern
```
key: search|[last_name] [first_name]-[phone]-[email]|[customer_id]
value: ""
```
#### Pros (Improve performance)
- Not use JOIN, LIKE
- Search in redis (RAM)
- Do not need to create index on customer phone, email, name fields
- Reduce working in MySQL

#### Cons:
- Have to integrate with Redis
- Server cost
- Be careful with data consistency between redis and database

#### Note:
- SHOULD NOT use with `little data` and `search rarely`
- Whenever create or update customer, we must to update redis too
- Improve performance with redis persistence (store backup in storage - hard disk)
#### Action:
- q is data we want to search
- use command: KEYS \*q\*
- loop results, split by `|`, get last element -> customer_id
- use that customer_id(s) in `WHERE` clause
#### Example:
- There are 3 customers data in redis
```
search|Nguyen Van-098789879-nguyenvan@gmail.com|1
search|Tran Van-012345678-tranvan@gmail.com|2
search|Le An-0965435679-lean@gmail.com|3
```
- Receive search action with `q=van`
- `KEYS \*van\*` will return 2 customers
```
search|Nguyen Van-098789879-nguyenvan@gmail.com|1
search|Tran Van-012345678-tranvan@gmail.com|2
search|Le
```
- Loop and split (golang)
```
// should check length before continue

// declare string or int depend on your database
var ids []string

for _, v := range results {
    id := strings.Split("|")[2]
    ids = append(ids, id)
}
```
- Use ids in WHERE clause
```
idsString := "(" + strings.Join(ids, ",") + ")"
SELECT * FROM order WHERE customer_id IN idsString
```