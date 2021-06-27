# ucsd [Lecture 8: Transactions, ACID, 2PC, 2PL, Serializability](https://cseweb.ucsd.edu/classes/sp16/cse291-e/applications/ln/lecture8.html)

## ACID Transactions

Traditional database systems have relied upon bundling work into *transactions* that have the *ACID* properties. In so doing, they guarantee **consistency** at the expense of availability and/or partition tolerance. 

> NOTE: CAP theory

*ACID* is an acronym for:

1、Atomicity

2、Consistency (serializability)

3、Isolation

4、Durability



1、Acid - "All or nothing"

2、Consistency -- This implies two types of consistency. It implies that a single system is consistent and that there is consistency across systems. In other words, if $100 is moved from one bank account to another, not only is it subtracted from one and added to another on one host -- it appears this way everywhere. It is this property that allows one transaction to safely follow another.

3、Isolation - Regardless of the level of concurrency, transactions must yields the same results as if they were executed one at a time (but any one of perhaps several orderings).

4、Durability - permanance. Changes persist over crashes, &c.

## Transactions, Detail and Example

*Transactions* are sequences of actions such that **all** of the operations within the **transaction** succeed (on all recipients) and their effects are permanantly visible, or none of **none** of the operations suceed anywhere and they have no visible effects; this might be because of failure (unintentional) or an abort (intentional).

> NOTE: 简而言之: "All or nothing"

## Mommit point

Characterisitically, transactions have a *commit point*. This is the point of no return. Before this point, we can **undo** a transaction. After this point, all changes are permanant. If problems occur after the **commit point**, we can take compensating(补偿；修正；抵消) or corrective action, but we can't wave a magic wand(魔法棒) and **undo** it.

> NOTE: commit后是不能redo的

## Distributed Transactions and Atomic Commit Protocols

> NOTE: 放到了distributed computing章节中了