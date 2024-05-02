This application has 2 tasks: task1 and task2. Each task prints a string onto the SWV ITM data console. Both of them have same priority - 2.
We have the systick timer interrupt (RTOS ticks) generated for every 1 ms (since configTICK_RATE_HZ 1000. This macro is used to control RTOS tick frequency).
In this application, configUSE_PREEMPTION is set to 1 --> kernel is premptive. That's why at every interrupt from the systick timer, it launches the scheduler to preempt the task. (Scheduler runs for every 1ms)
It doesn't matter whether task1 completed its execution or not. Scheduler preempts task1 and runs task2 on CPU. Task1 enters READY state, task2 enters RUNNING state.
At the next RTOS tick, task2 returns to READY state and task1 enters RUNNING state. Now, task1 continues at the place where it stoppped excuting in the previous preemption (starts printing where it previously stopped).
... and so on.
ITM in the p. is a resource now which is shared between 2 tasks. 
This is the reason prints appear corrupted.

This is called resource contention - multiple tasks racing to use a common resource.

To solve this issue, we can make task1 to lock this resource and not to relaese it until it finishes printing its full message. This is done using semaphore/mutex - kind of lock.
Or we can also config the kernel to use cooperative scheduling.