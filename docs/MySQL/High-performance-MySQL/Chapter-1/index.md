# CHAPTER 1 MySQL Architecture and History

MySQL is very different from other database servers, and its architectural characteristics make it useful for a wide range of purposes as well as making it a poor choice for others.

> NOTE: 
>
> tradeoff
>
> 意图



To get the most from MySQL, you need to understand its design so that you can work with it, not against it. MySQL is flexible in many ways. For example, you can configure it to run well on a wide range of hardware, and it supports a variety of data types. However, MySQL’s most unusual and important feature is its **storage-engine architecture**, whose design separates **query processing** and other server tasks from **data storage and retrieval**. This **separation of concerns** lets you choose how your data is stored and what performance, features, and other characteristics you want.

