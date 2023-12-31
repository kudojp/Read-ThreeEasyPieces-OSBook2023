> Q1. Write a program that calls fork(). Before calling fork(), have the main process access a variable (e.g., x) and set its value to something (e.g., 100). What value is the variable in the child process? What happens to the variable when both the child and parent change the value of x?

Done in `hw_q1.c`. The stdout is as below.
```
Before fork, x=100
In a parent process (96144), x=100
In a parent process (96144), overwritten as x=1
In a child process (96145), x=100
In a child process (96145), overwritten as x=-1
Child process is finished now.
In a parent process (96144), it turns out that x=1
```

So, even if the child process changes x, it does not affect the parent process.

> Q2. Write a program that opens a file (with the open() system call) and then calls fork() to create a new process. Can both the child and parent access the file descriptor returned by open()?    
> What happens when they are writing to the file concurrently, i.e., at the same time?

For concurrent read, stdout of the first half of `hw_q2.c` is as below.
Thus, fd is shared between processes. It is noted  that the current offset of the file is shared as well.

```
Before fork, created a fd (3)
Read a file in a parent process: abc�ro�0
Read a file in a child process: def
```

For concurrent write, stdout of the last half is as below.
Thus, lines written from parent and those from child are mixed together.

```
Write from parent process
 Write from child process
 Write from child process
 Write from parent process
 Write from child process
 Write from parent process
 
```

> Q3. Write another program using fork(). The child process should print “hello”; the parent process should print “goodbye”. You should try to ensure that the child process always prints first; can you do this without calling wait() in the parent?

- with wait() -> Done in `hw_q3_with_wait.c`.
- without wait() -> Done in `hw_q3_without_wait.c`. I sent a signal from the child process to the parent.

> Q4. Write a program that calls fork() and then calls some form of exec() to run the program /bin/ls. See if you can try all of the variants of exec(), including (on Linux) execl(), execle(), execlp(), execv(), execvp(), and execvpe(). Why do you think there are so many variants of the same basic call?

Done in `hw_q4.c`.
- e -> You can specify environment variables.
- p -> You can specify PATH.
- v -> You can path command arguments in a form of vector.

> Q5. Now write a program that uses wait() to wait for the child process to finish in the parent. What does wait() return? What happens if you use wait() in the child?

- In a parent process, `wait()` returns id of the child process. (`hw_q5_wait_in_parent.c`)
- In a child process, `wait()` returns -1. (`hw_q5_wait_in_child.c`)

> Q6. Write a slight modification of the previous program, this time using waitpid() instead of wait(). When would waitpid() be useful?

Done in `hw_q6.c`.
`waitpid()` is useful when a parent process spawns multiple child processes.

> Q7. Write a program that creates a child process, and then in the child closes standard output (STDOUT FILENO). What happens if the child calls printf() to print some output after closing the descriptor?

In a child process, it cannot stdout anymore.
However in the parent process, even if STDOUT_FILENO is closed in the child process, it can stdout.

> Q8. Write a program that creates two children, and connects the standard output of one to the standard input of the other, using the pipe() system call.
