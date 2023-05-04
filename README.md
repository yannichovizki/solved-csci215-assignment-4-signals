Download Link: https://assignmentchef.com/product/solved-csci215-assignment-4-signals
<br>
<h2>Question 1 ­ Home brew of the sleep() call</h2>

The function unsigned int sleep (int sec) suspends the calling process until sec seconds have elapsed, or until a signal is delivered. The function returns 0 if the initial sleep time has fully elapsed, or the number of seconds remaining in the case of a delivered signal.

Program a function unsigned int mysleep(int sec) that uses the alarm system call to simulate the behavior of the sleep call.

<strong>NB:</strong> similarly to the POSIX specification of the sleep call, you can assume that the user will neither call alarm() nor use the SIGALRM signal in the same program.

<strong>NB2:</strong> if your function alters the default mask and / or behavior associated with a signal for a given period of time, you must restore the original mask and / or behavior afterwards.

Question 2 ­ Signal waves

We want to create a program waves that sends waves of signals along a lineage of processes. This program works as follows:

The parent process PP associated with the main program creates a line of N processes (parent creates a child, child creates a grandchild, and so on …)

Each process, with the exception of the last descendant LD (ie. the last created process) blocks in wait of a SIGUSR1. Process LD will start a wave of SIGUSR1 signals: it sends a SIGUSR1 to its parent, which transmits it to its own parent, and so on until PP delivers the SIGUSR1 received from its own child.

At the end of the first wave, PP initiates a second wave with SIGUSR2 signals. This second wave stops when it reaches LD.

Once it receives the SIGUSR2 signal of the second wave, LD initiates a third and last wave with SIGINT signals. This last wave is different from the previous ones: every child process terminates after emitting a SIGINT.

At the end of the third wave, PP displays the message “End of program” and terminates.

Question 3 ­ Signal­based barrier

The program below creates two child processes. All three processes carry out calc1() and then calc2() concurrently.

void calc1 () { int i;

for (i = 0; i &lt;1E8; i ++);

}

void calc2 () { int i;

for (i = 0; i &lt;1E8; i ++);

}

int main (int argc, char * argv []) { int i = 0;

pid_t pid_child [2];

while ((i &lt;2) &amp;&amp; ((pid_child [i] = fork ())! = 0)) i ++;

calc1 (); calc2 ();

printf ( “End Process %d 
”, i);

EXIT_SUCESS return;

}

We want to change the program by using signals (SIGUSR1 and / or SIGUSR2) to make sure that no process begins calc2 before the other two processes have also finished calc1.

Write the program barrier that implements this new specification.

<strong>Question 3.1</strong>: What is the minimum number of signal <em>emissions</em> required for this barrier? <strong>Question 3.2</strong>: Is it possible to implement this barrier with SIGUSR1 only?

Question 4 ­ Waiting for everyone without wait()

Consider the following program:

1:            #define N 4

2:            int i,j;

3:

4:            int main (int arg, char * argv []){

5:                  pid_t pid;

<sup>6:            </sup>     j=0;

<sup>7:            </sup>     for (i=0; i&lt;N &amp;&amp; (pid=fork())!=0; i++)

<sup>8:             </sup>     printf (“i:%d j:%d ⧵n”, i,j);

9:

<sup>10:          </sup>     if (((i%2)==1) &amp;&amp; (pid==0)) {

11:

while ((j&lt;i) &amp;&amp; ((pid=fork())==0)) {

12:

j++;

13:

printf (“i:%d j:%d ⧵n”, i,j);

14:

}

15:

}

16:

}

Modify this program so that the initial parent process will not terminate before any of its descendants has notified it that it is going to terminate too. Your code shall respect the following guidelines: