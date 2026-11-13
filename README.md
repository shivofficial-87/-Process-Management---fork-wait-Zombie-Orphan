
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

void sortAscending(int a[], int n)
{
    int i, j, temp;

    for(i = 0; i < n-1; i++)
    {
        for(j = 0; j < n-i-1; j++)
        {
            if(a[j] > a[j+1])
            {
                temp = a[j];
                a[j] = a[j+1];
                a[j+1] = temp;
            }
        }
    }
}

void sortDescending(int a[], int n)
{
    int i, j, temp;

    for(i = 0; i < n-1; i++)
    {
        for(j = 0; j < n-i-1; j++)
        {
            if(a[j] < a[j+1])
            {
                temp = a[j];
                a[j] = a[j+1];
                a[j+1] = temp;
            }
        }
    }
}

void display(int a[], int n)
{
    int i;

    for(i = 0; i < n; i++)
        printf("%d ", a[i]);

    printf("\n");
}

int main()
{
    int a[100], n, i;
    pid_t pid;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    printf("Enter elements:\n");

    for(i = 0; i < n; i++)
        scanf("%d", &a[i]);

    pid = fork();

    if(pid < 0)
    {
        printf("Fork failed\n");
        return 1;
    }

    else if(pid == 0)
    {
        printf("\nChild Process\n");
        printf("Child PID: %d\n", getpid());

        sortDescending(a, n);

        printf("Descending Order: ");
        display(a, n);

        exit(0);
    }

    else
    {
        printf("\nParent Process\n");
        printf("Parent PID: %d\n", getpid());

        sortAscending(a, n);

        printf("Ascending Order: ");
        display(a, n);

        wait(NULL);

        printf("Child process completed.\n");
    }

    // Demonstration of Zombie Process
    printf("\n--- Zombie Process Demonstration ---\n");

    pid = fork();

    if(pid == 0)
    {
        printf("Zombie child process terminating.\n");
        exit(0);
    }
    else
    {
        printf("Parent sleeping for 10 seconds.\n");
        sleep(10);

        wait(NULL);

        printf("Zombie process cleared using wait().\n");
    }

    // Demonstration of Orphan Process
    printf("\n--- Orphan Process Demonstration ---\n");

    pid = fork();

    if(pid == 0)
    {
        printf("Child process started.\n");
        printf("Child PID: %d\n", getpid());
        printf("Parent PID: %d\n", getppid());

        sleep(5);

        printf("Child is now an orphan.\n");
        printf("New Parent PID: %d\n", getppid());
    }
    else
    {
        printf("Parent process terminating.\n");
        exit(0);
    }

    return 0;
}
