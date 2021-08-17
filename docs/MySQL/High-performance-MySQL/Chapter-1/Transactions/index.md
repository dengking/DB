# Transactions



A banking application is the classic example of why transactions are necessary. Imagine a bank’s database with two tables: **checking** and **savings**. To move `$200` from Jane’s checking account to her savings account, you need to perform at least three steps:

1、Make sure her checking account balance is greater than `$200`.

2、Subtract `$200` from her checking account balance.

3、Add `$200` to her savings account balance.

The entire operation should be wrapped in a **transaction** so that if any one of the steps fails, any completed steps can be **rolled back**.

You start a transaction with the `START TRANSACTION` statement and then either make its changes permanent with `COMMIT` or discard the changes with `ROLLBACK`. So, the SQL for our sample transaction might look like this:

```sql
 START TRANSACTION;
 SELECT balance FROM checking WHERE customer_id = 10233276;
 UPDATE checking SET balance = balance - 200.00 WHERE customer_id = 10233276;
 UPDATE savings SET balance = balance + 200.00 WHERE customer_id = 10233276;
 COMMIT;
```

> NOTE: 
>
> checking account 支票帐户，你所开出去的支票会从这个账户提款．利息很低或无利息．
>
> savings account 储蓄帐户，利息比支票账户高

But transactions alone aren’t the whole story. What happens if the database server crashes while performing line 4? Who knows? The customer probably just lost `$200`. And what if another process comes along between lines 3 and 4 and removes the entire checking account balance? The bank has given the customer a `$​200` credit(贷款) without even knowing it.

Transactions aren’t enough unless the system passes the **ACID test**. ACID stands for **Atomicity**, **Consistency**, **Isolation**, and **Durability**. These are tightly related criteria that a well-behaved transaction processing system must meet:

## Atomicity



## Consistency



## Isolation



## Durability