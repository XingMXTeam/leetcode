# 003 Stack

## Content

顺序队列
链式队列

向线程池请求一个线程，但是线程池没有空闲线程。
处理策略：
1 拒绝
2 阻塞队列。也就是排队。
链表：可以无限排队 无界队列 数组：有界队列(响应时间敏感)
数据库连接池、分布式的消息队列（kafka）
大部分资源有限的场景，都可以通过队列实现请求排队。

队列有头尾指针。从头指针出，从尾指针进。
出队的时间复杂度是O(1)
入队可能出现队满的情况，需要做数据搬迁。平均时间复杂度是O(1)

循环队列：如何写好循环队列（确定好队空和队满的判定条件）（重点）
队空：tail=head
队满：(tail+1)%n=head。 tail没有存储东西。浪费一个数组的存储空间。
循环队列的长度设定需要对并发数据有一定的预测，否则会丢失太多请求。

阻塞队列:生产者-消费者模型
其实就是在队列基础上增加了阻塞操作。简单来说，就是在队列为空的时候，从队头取数据会被阻塞。因为此时还没有数据可取，直到队列中有了数据才能返回；如果队列已经满了，那么插入数据的操作就会被阻塞，直到队列中有空闲位置后再插入数据，然后再返回。

并发队列(线程安全的队列)：最简单直接的实现方式是直接在 enqueue()、dequeue() 方法上加锁，但是锁粒度大并发度会比较低，同一时刻仅允许一个存或者取操作。实际上，基于数组的循环队列，利用 CAS 原子操作，可以实现非常高效的并发队列。这也是循环队列比链式队列应用更加广泛的原因。在实战篇讲 Disruptor 的时候，我会再详细讲并发队列的应用。
无锁并发队列？
考虑使用CAS实现无锁队列，则在入队前，获取tail位置，入队时比较tail是否发生变化，如果否，则允许入队，反之，本次入队失败。出队则是获取head位置，进行cas。



## Question

1. 实现一个循环队列

