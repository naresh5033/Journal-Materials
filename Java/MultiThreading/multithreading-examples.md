# Multi Threading Examples and Solutions

### Thread Creation - MultiExecutor Solution

In this exercise we are going to implement a  MultiExecutor .

The client of this class will create a list of Runnable tasks and provide that list into MultiExecutor's constructor.

When the client runs the . executeAll(),  the MultiExecutor,  will execute all the given tasks.

To take full advantage of our multicore CPU, we would like the MultiExecutor to execute all the tasks in parallel, by passing each task to a different thread.

import java.util.ArrayList;
import java.util.List;

public class MultiExecutor {

    private final List<Runnable> tasks;
 
    /*
     * @param tasks to executed concurrently
     */
    public MultiExecutor(List<Runnable> tasks) {
        this.tasks = tasks;
    }
 
    /**
     * Executes all the tasks concurrently
     */
    public void executeAll() {
        List<Thread> threads = new ArrayList<>(tasks.size());
        
        for (Runnable task : tasks) {
            Thread thread = new Thread(task);
            threads.add(thread);
        }
        
        for(Thread thread : threads) {
            thread.start();
        }
    }
}

### Multithreaded Calculation - Solution

In this coding exercise you will use all the knowledge from the previous lectures.
Before taking the exercise make sure you review the following topics in particular:
1. Thread Creation - how to create and start a thread using the Thread class and the start() method.

2. Thread Join - how to wait for another thread using the Thread.join() method.



In this exercise we will efficiently calculate the following result = base1 ^ power1 + base2 ^ power2

Where a^b means: a raised to the power of b.

For example 10^2 = 100

We know that raising a number to a power is a complex computation, so we we like to execute:

result1 = x1 ^ y1

result2 = x2 ^ y2

In parallel.

and combine the result in the end : result = result1 + result2

This way we can speed up the entire calculation.



Note :

base1 >= 0, base2 >= 0, power1 >= 0, power2 >= 0



import java.math.BigInteger;

public class ComplexCalculation {
public BigInteger calculateResult(BigInteger base1,
BigInteger power1,
BigInteger base2,
BigInteger power2) {
BigInteger result;
PowerCalculatingThread thread1 = new PowerCalculatingThread(base1, power1);
PowerCalculatingThread thread2 = new PowerCalculatingThread(base2, power2);

        thread1.start();
        thread2.start();
 
        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
 
        result = thread1.getResult().add(thread2.getResult());
        return result;
    }
 
    private static class PowerCalculatingThread extends Thread {
        private BigInteger result = BigInteger.ONE;
        private BigInteger base;
        private BigInteger power;
 
        public PowerCalculatingThread(BigInteger base, BigInteger power) {
            this.base = base;
            this.power = power;
        }
 
        @Override
        public void run() {
            for(BigInteger i = BigInteger.ZERO;
                i.compareTo(power) !=0;
                i = i.add(BigInteger.ONE)) {
                result = result.multiply(base);
            }
        }
 
        public BigInteger getResult() {
            return result;
        }
    }
}

### Min - Max Metrics - Solution

In this exercise we are going to implement a class called MinMaxMetrics .

A single instance of this class will be passed to multiple threads in our application.

MinMaxMetrics is an analytics class and is used to keep track of the minimum and the maximum of a particular business or performance metric in our application.

Example:

A stock trading application that keeps track of the minimum and maximum price of the stock on a daily basis.



The class will have 3 methods:

addSample(..) - Takes a new sample.

getMin() - Returns the sample with the minimum value we have seen so far.

getMax() - Returns the sample with the maximum value we have seen so far.



Notes:

- Each of those methods can be called by any given number of threads concurrently, so the class needs to be thread safe.

- In addition, this class is used for analytics, so it needs to be as performant as possible as we don't want it to slow down our business logic threads too much.

- Threads that call getMin() or getMax() are interested in only one of the values and are never interested in both the min and the max in the same time

public class MinMaxMetrics {

    private volatile long minValue;
    private volatile long maxValue;
 
    /**
     * Initializes all member variables
     */
    public MinMaxMetrics() {
        this.maxValue = Long.MIN_VALUE;
        this.minValue = Long.MAX_VALUE;
    }
 
    /**
     * Adds a new sample to our metrics.
     */
    public void addSample(long newSample) {
        synchronized (this) {
            this.minValue = Math.min(newSample, this.minValue);
            this.maxValue = Math.max(newSample, this.maxValue);
        }
    }
 
    /**
     * Returns the smallest sample we've seen so far.
     */
    public long getMin() {
        return this.minValue;
    }
 
    /**
     * Returns the biggest sample we've seen so far.
     */
    public long getMax() {
        return this.maxValue;
    }
}

### Advanced locking, Product Reviews Service - Solution

After we learned about advanced locks and read-write locks, we told our friend Bob that we now know how to protect shared resources with special locks that keep our application thread-safe and efficient.

Bob just completed a mission-critical class for his online store that maintains the store products' reviews, and he asked us for help.

He already implemented all the business logic however not knowing which locks to use, he left us blank methods getLockForXXXX() where we can provide his business logic methods with the correct locks.



Please complete the class in the appropriate sections, to help Bob keep his class thread-safe and performant.

import java.util.*;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class ProductReviewsService {
private final HashMap<Integer, List<String>> productIdToReviews;

    // Create your member variables here
    ReentrantReadWriteLock reentrantReadWriteLock = new ReentrantReadWriteLock();
    Lock readLock = reentrantReadWriteLock.readLock();
    Lock writeLock = reentrantReadWriteLock.writeLock();
 
    /** DO NOT MODIFY THIS SECTION **/
 
    public ProductReviewsService() {
        this.productIdToReviews = new HashMap<>();
    }
 
    /**
     * Adds a product ID if not present
     */
    public void addProduct(int productId) {
        Lock lock = getLockForAddProduct();
 
        lock.lock();
 
        try {
            if (!productIdToReviews.containsKey(productId)) {
                productIdToReviews.put(productId, new ArrayList<>());
            }
        } finally {
            lock.unlock();
        }
    }
 
    /**
     * Removes a product by ID if present
     */
    public void removeProduct(int productId) {
        Lock lock = getLockForRemoveProduct();
 
        lock.lock();
 
        try {
            if (productIdToReviews.containsKey(productId)) {
                productIdToReviews.remove(productId);
            }
        } finally {
            lock.unlock();
        }
    }
 
    /**
     * Adds a new review to a product
     * @param productId - existing or new product ID
     * @param review - text containing the product review
     */
    public void addProductReview(int productId, String review) {
        Lock lock = getLockForAddProductReview();
        
        lock.lock();
        
        try {
            if (!productIdToReviews.containsKey(productId)) {
                productIdToReviews.put(productId, new ArrayList<>());               
            }
            productIdToReviews.get(productId).add(review);
        } finally {
            lock.unlock();
        }
    }
 
    /**
     * Returns all the reviews for a given product
     */
    public List<String> getAllProductReviews(int productId) {
        Lock lock = getLockForGetAllProductReviews();
 
        lock.lock();
 
        try {
            if (productIdToReviews.containsKey(productId)) {
                return Collections.unmodifiableList(productIdToReviews.get(productId));
            }
        } finally {
            lock.unlock();
        }
 
        return Collections.emptyList();
    }
 
    /**
     * Returns the latest review for a product by product ID
     */
    public Optional<String> getLatestReview(int productId) {
        Lock lock = getLockForGetLatestReview();
 
        lock.lock();
 
        try {
 
            if (productIdToReviews.containsKey(productId) && !productIdToReviews.get(productId).isEmpty()) {
                List<String> reviews = productIdToReviews.get(productId);
                return Optional.of(reviews.get(reviews.size() - 1));
            }
        } finally {
            lock.unlock();
        }
 
        return Optional.empty();
    }
 
    /**
     * Returns all the product IDs that contain reviews
     */
    public Set<Integer> getAllProductIdsWithReviews() {
        Lock lock = getLockForGetAllProductIdsWithReviews();
 
        lock.lock();
 
        try {
            Set<Integer> productsWithReviews = new HashSet<>();
            for (Map.Entry<Integer, List<String>> productEntry : productIdToReviews.entrySet()) {
                if (!productEntry.getValue().isEmpty()) {
                    productsWithReviews.add(productEntry.getKey());
                }
            }
            return productsWithReviews;
        } finally {
            lock.unlock();
        }
    }
 
    /** END OF UNMODIFIABLE SECTION **/
 
    Lock getLockForAddProduct() {
        // Add code here
        return writeLock;
    }
 
    Lock getLockForRemoveProduct() {
        // add code here
        return writeLock;
    }
 
    Lock getLockForAddProductReview() {
        // add code here
        return writeLock;
    }
 
    Lock getLockForGetAllProductReviews() {
        // add code here
        return readLock;
    }
 
    Lock getLockForGetLatestReview() {
        // add code here
        return readLock;
    }
 
    Lock getLockForGetAllProductIdsWithReviews() {
        // add code here
        return readLock;
    }
}

### Inter Thread communication, Simple CountDownLatch - Solution

As part of the java.util.concurrent package, the JDK contains many classes that help us coordinate between threads.
A few of those include the CountDownLatch, CyclicBarrier, Phaser, Exchanger, and others that may be added in future versions of the JDK.

However, all those classes are no more than higher-level wrappers built upon the same building blocks that we learned and already know very well. As such, if we never knew those classes existed, we wouldn't even need them, because we can implement their functionality with the tools we already have.

However it is still recommended to get somewhat familiar with at least some of them as they represent useful patterns.

In this exercise we are going to learn about the CountDownLatch , by implementing a subset of its functionality.



The SimpleCountDownLatch

To get familiar with CountDownLatch as well as gain confidence in our ability to implement it and other higher level classes we are going to implement a simple version of the CountDownLatch and implement the majority of the CountDownLatch's functionality.

The CountDownLatch and its simple version the SimpleCountDownLatch are initialized with a non-negative count.

The SimpleCountDownLatch allows one or more threads to wait until a set of operations being performed in other threads, completes.

The SimpleCountDownLatch has the following main operations:

countDown() - Decrements the count of the latch, releasing all waiting threads when the count reaches zero. If the current count already equals zero then nothing happens.

await() - Causes the current thread to wait until the latch has counted down to zero. If the current count is already zero then this method returns immediately.

The await method blocks until the current count reaches zero due to invocations of the countDown() method, after which all waiting threads are released and any subsequent invocations of await return immediately.

For more information and examples see CountDownLatch.

Hint : Consider using either version of the condition variables (Lock's condition variables or Object's wait/notify). For practice purposes you can try both.

Notes:

The SimpleCountDownLatch instance is single-use. In other words, once the count goes to zero, the SimpleCountDownLatch has no additional use.

The class has to be thread safe



Solution
public class SimpleCountDownLatch {
private int count;

    public SimpleCountDownLatch(int count) {
        this.count = count;
        if (count < 0) {
            throw new IllegalArgumentException("count cannot be negative");
        }
    }
 
    /**
     * Causes the current thread to wait until the latch has counted down to zero.
     * If the current count is already zero then this method returns immediately.
    */
    public void await() throws InterruptedException {
        synchronized (this) {
            while (count > 0) {
                this.wait();
            }
        }
    }
 
    /**
     *  Decrements the count of the latch, releasing all waiting threads when the count reaches zero.
     *  If the current count already equals zero then nothing happens.
     */
    public void countDown() {
        synchronized (this) {
            if (count > 0) {
                count--;
                
                if (count == 0) {
                    this.notifyAll();
                }
            }
        }
    }
 
    /**
     * Returns the current count.
    */
    public int getCount() {
        return this.count;
    }
}


