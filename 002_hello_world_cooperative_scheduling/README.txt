Now, lets use cooperative scheduling - prj - 002_hello_world_cooperative_scheduling

When task1 finishes sending its message, it should give up the p. using taskYIELD() API of FreeRTOS in task1_handler. A task willlingly giving up the p. is called yielding. This will force a context switch.
Now, the scheduler schedules task2. 
Use taskYIELD() in task2_handler also.

There is no preemption here. There is just switching of tasks willingly.

Now, the output is printed correctly onto the SWV ITM data console. 