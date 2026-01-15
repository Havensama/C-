Write a program that:
1.	simulate the CPU scheduling algorithm round-robin.

SOURCE CODE USING C:
#include <stdio.h>

int main() {
    int n, timeQuantum;
    int bt[20], rt[20], wt[20], tat[20];
    int time = 0, remain, i = 0;
    float avgWT = 0, avgTAT = 0;

    printf("Enter number of processes: ");
    scanf("%d", &n);

    remain = n;

    printf("Enter burst time for each process:\n");
    for (i = 0; i < n; i++) {
        printf("Process %d: ", i + 1);
        scanf("%d", &bt[i]);
        rt[i] = bt[i];   // remaining time
        wt[i] = 0;
    }

    printf("Enter time quantum: ");
    scanf("%d", &timeQuantum);

    printf("\nProcess\tBurst Time\tWaiting Time\tTurnaround Time\n");

    i = 0;
    while (remain > 0) {
        if (rt[i] > 0) {
            if (rt[i] <= timeQuantum) {
                time += rt[i];
                rt[i] = 0;
                remain--;

                tat[i] = time;
                wt[i] = tat[i] - bt[i];

                avgWT += wt[i];
                avgTAT += tat[i];

                printf("P%d\t\t%d\t\t%d\t\t%d\n",
                       i + 1, bt[i], wt[i], tat[i]);
            } else {
                rt[i] -= timeQuantum;
                time += timeQuantum;
            }
        }
        i = (i + 1) % n;
    }

    avgWT /= n;
    avgTAT /= n;

    printf("\nAverage Waiting Time = %.2f", avgWT);
    printf("\nAverage Turnaround Time = %.2f\n", avgTAT);

    return 0;
}

2.	simulate the CPU scheduling algorithm non-preemptive priority.

SOURCE CODE USING C:
#include <stdio.h>

int main() {
    int n;
    int bt[20], wt[20], tat[20], pr[20], pid[20];
    int i, j, temp;
    float avgWT = 0, avgTAT = 0;

    printf("Enter number of processes: ");
    scanf("%d", &n);

    for (i = 0; i < n; i++) {
        pid[i] = i + 1;
        printf("\nProcess %d\n", pid[i]);
        printf("Burst Time: ");
        scanf("%d", &bt[i]);
        printf("Priority (lower number = higher priority): ");
        scanf("%d", &pr[i]);
    }

    /* Sort processes by priority */
    for (i = 0; i < n - 1; i++) {
        for (j = i + 1; j < n; j++) {
            if (pr[i] > pr[j]) {
                temp = pr[i]; pr[i] = pr[j]; pr[j] = temp;
                temp = bt[i]; bt[i] = bt[j]; bt[j] = temp;
                temp = pid[i]; pid[i] = pid[j]; pid[j] = temp;
            }
        }
    }

    wt[0] = 0;
    for (i = 1; i < n; i++) {
        wt[i] = wt[i - 1] + bt[i - 1];
    }

    for (i = 0; i < n; i++) {
        tat[i] = wt[i] + bt[i];
        avgWT += wt[i];
        avgTAT += tat[i];
    }

    avgWT /= n;
    avgTAT /= n;

    printf("\nProcess\tPriority\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for (i = 0; i < n; i++) {
        printf("P%d\t\t%d\t\t%d\t\t%d\t\t%d\n",
               pid[i], pr[i], bt[i], wt[i], tat[i]);
    }

    printf("\nAverage Waiting Time = %.2f", avgWT);
    printf("\nAverage Turnaround Time = %.2f\n", avgTAT);

    return 0;
}



1. EXPERIMENT 1
using System;
using System.Diagnostics;

class number1
{
    static void Main(string[] args)
    {
        try
        {
            Process process = new Process();
            process.StartInfo.FileName = "whoami";
            process.StartInfo.UseShellExecute = false;

            process.Start();

            Console.WriteLine("Process started successfully");
            Console.WriteLine("Process ID is: " + process.Id);

            process.WaitForExit();
        }
        catch (Exception)
        {
            Console.WriteLine("Process failed");
        }
    }
}


2. EXPERIMENT 1.2

   #include <iostream>
#include <unistd.h>
#include <sys/wait.h>
#include <cstdlib>

using namespace std;

class Process {
public:
    void run() {
        int pid;
        pid = fork();

        if (pid < 0) {
            cout << "Fork failed" << endl;
            exit(1);
        }
        else if (pid == 0) {
            // child process
            execlp("whoami", "whoami", NULL);
            exit(0);
        }
        else {
            // parent process
            cout << "Process ID is: " << getpid() << endl;
            wait(NULL);
        }
    }
};

int main() {
    Process p;
    p.run();
    return 0;
}

