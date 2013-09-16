Suppose a cigarette requires three ingredients, tobacco, paper and match. There are three chain smokers. 
Each of them has only one ingredient with infinite supply. 
There is an agent who has infinite supply of all three ingredients. 
To make a cigarette, the smoker has tobacco (resp., paper and match) must have the other two ingredients paper and match 
(resp., tobacco and match, and tobacco and paper). 
The agent and smokers share a table. 
The agent randomly generates two ingredients and notifies the smoker who needs these two ingredients. 
Once the ingredients are taken from the table, the agent supplies another two. 
On the other hand, each smoker waits for the agent's notification. 
Once it is notified, the smoker picks up the ingredients, makes a cigarette, smokes for a while, 
and goes back to the table waiting for his next ingredients.


We use red squares and green circles for semaphore waits and signals, respectively. 
The table is a shared object, because the agent puts ingredients on the table and smokers take ingredients from the table.
Therefore, some protection is necessary to avoid race condition. 
A mutex lock simply does not work, because with a lock the owner must unlock the lock which is not the case here. 
The agent waits for the table to become available, and then puts ingredients on the table. 
If we use a mutex lock, the agent must unlock the table, and if the agent is a fast runner he may come back 
and lock the table again to make another set of ingredient before the previous ones are taken. 
Thus, we have a race condition. 
Thus, we use a semaphore with which the thread that executes a wait to lock the semaphore and the thread that 
executes a signal to unlock the semaphore do not have to be the same.


When the table is available, the agent puts ingredient on the table. 
Of course, the agent knows what ingredients are on the table, and, hence, signals the smoker that needs this 
set of ingredients. On the other hand, a smoker cannot continue if he has no ingredients. 
To this end, we need another semaphore to block a smoker. In fact, we need three semaphores, one for each smoker. 
For example, if the agent puts paper and match on the table, he most inform the smoker who needs paper and match to 
continue. Therefore, the smoker who needs paper and match must wait on this semaphore.


Thus, the agent prepares for the ingredients, waits on the table, makes the ingredients available on the table when it 
becomes available, signals the proper smoker to take the ingredient, and then goes back for another round. 
For a smoker, he waits for the ingredients he needs, takes them when they are available, informs the agent that the 
table is free, makes a cigarette and smokes, and goes back for another round.
