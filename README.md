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
