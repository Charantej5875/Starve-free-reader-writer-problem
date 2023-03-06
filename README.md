# Starve-free-reader-writer-problem
# Problem description
 The reader-writer problem is a classic synchronization problem in computer science. It arises when multiple processes access a shared resource, where some processes only read the resource and others both read and write the resource. The problem is to ensure that concurrent access to the shared resource is managed in a way that maintains correctness and efficiency. 

In the reader-writer problem, readers can safely access the shared resource simultaneously without interfering with each other. However, when a writer wants to modify the resource, it must have exclusive access to the resource, preventing other readers and writers from accessing it until the write operation is complete. A starvation free solution to this concurrency issue is provided in the attached pseudocode. 

# Pseudo code
```C++ int rc=0;  //counts the number of readers reading shared data
binary semaphore mutex=1; //provides access to change rc
counting semaphore order=1; //To maintain queue of process which are willing to access shared data in FIFO order

binary semaphore data=1;  //To maintain access to shared data

//Reader Process
void reader(){
    //entry section
    wait(order); //wait in queue for its chance to get executed
    wait(mutex); //to maintain mutual exclusion
    rc++;
    if(rc==1) wait(data);  //wait process for access to shared data
    signal(order); //allowing next process in queue to get executed    
    signal(mutex); //

    //critical section
    
    //exit section
    wait(mutex);
    rc--;
    if(rc==0) signal(data); //release the access to shared data
    signal(mutex);
}```

//Writer Process
void writer(){
    //entry section
    wait(order);  //wait process in queue for its chance to get executed
    wait(data); //wait for access to shared data
    signal(order);  //allowing next process in queue to get executed

    //critical section
    
    //exit section
    signal(data); //release the access to shared data
}
```
# Pseudo code expalanation
A variable named “rc” is maintained to count number of readers in critical section. The counting semaphore "order"  maintains a queue for all processes (readers and writers) that are ready to be executed. The binary semaphore mutex is used to ensure that only one reader can access the “rc” variable at a time while binary semaphore data is maintained to give access to data in a mutually exclusive manner. If there are many sequential readers in the queue, all of them may access the shared resource at once to read.Each process will get a chance of progress because of FIFO implementation and not priority.In this way we can maintain mutual exclusion between process making sure that no process is prone to starvation. 
 
