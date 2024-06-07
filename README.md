// SJF scheduling program in c
#include<string.h>
#include<stdio.h> // Add this line

int main()
{
    int bt[20],at[10],n,i,j,temp,st[10],ft[10],wt[10],ta[10];
    int totwt=0,totta=0;
    double awt,ata;
    char pn[10][10],t[10];
    //clrscr();
    printf("Enter the number of process:");
    scanf("%d",&n);
    for(i=0; i<n; i++)
    {
        printf("Enter process name, arrival time& burst time:");
        scanf("%s%d%d",pn[i],&at[i],&bt[i]);
    }
    for(i=0; i<n; i++)
        for(j=0; j<n; j++)
        {
            if(bt[i]<bt[j])
            {
                temp=at[i];
                at[i]=at[j];
                at[j]=temp;
                temp=bt[i];
                bt[i]=bt[j];
                bt[j]=temp;
                strcpy(t,pn[i]);
                strcpy(pn[i],pn[j]);
                strcpy(pn[j],t);
            }
        }
    for(i=0; i<n; i++)
    {
        if(i==0)
            st[i]=at[i];
        else
            st[i]=ft[i-1];
        wt[i]=st[i]-at[i];
        ft[i]=st[i]+bt[i];
        ta[i]=ft[i]-at[i];
        totwt+=wt[i];
        totta+=ta[i];
    }
    awt=(double)totwt/n;
    ata=(double)totta/n;
    printf("\nProcessname\tarrivaltime\tbursttime\twaitingtime\tturnaroundtime");
    for(i=0; i<n; i++)
    {
        printf("\n%s\t%5d\t\t%5d\t\t%5d\t\t%5d",pn[i],at[i],bt[i],wt[i],ta[i]);
    }
    printf("\nAverage waiting time: %f",awt);
    printf("\nAverage turnaroundtime: %f",ata);
    return 0;
}



Priority scheduling problem in c


 
#include <stdio.h>
 
//Function to swap two variables
void swap(int *a,int *b)
{
    int temp=*a;
    *a=*b;
    *b=temp;
}
int main()
{
    int n;
    printf("Enter Number of Processes: ");
    scanf("%d",&n);
 
    // b is array for burst time, p for priority and index for process id
    int b[n],p[n],index[n];
    for(int i=0;i<n;i++)
    {
        printf("Enter Burst Time and Priority Value for Process %d: ",i+1);
        scanf("%d %d",&b[i],&p[i]);
        index[i]=i+1;
    }
    for(int i=0;i<n;i++)
    {
        int a=p[i],m=i;
 
        //Finding out highest priority element and placing it at its desired position
        for(int j=i;j<n;j++)
        {
            if(p[j] > a)
            {
                a=p[j];
                m=j;
            }
        }
 
        //Swapping processes
        swap(&p[i], &p[m]);
        swap(&b[i], &b[m]);
        swap(&index[i],&index[m]);
    }
 
    // T stores the starting time of process
    int t=0;
 
    //Printing scheduled process
    printf("Order of process Execution is\n");
    for(int i=0;i<n;i++)
    {
        printf("P%d is executed from %d to %d\n",index[i],t,t+b[i]);
        t+=b[i];
    }
    printf("\n");
    printf("Process Id     Burst Time   Wait Time    TurnAround Time\n");
    int wait_time=0;
    for(int i=0;i<n;i++)
    {
        printf("P%d          %d          %d          %d\n",index[i],b[i],wait_time,wait_time + b[i]);
        wait_time += b[i];
    }
    return 0;
}


BEST FIT ALGORITHM

#include <stdio.h>

void implimentBestFit(int blockSize[], int blocks, int processSize[], int proccesses)
{
    // This will store the block id of the allocated block to a process
    int allocation[proccesses];
    int occupied[blocks];
    
    // initially assigning -1 to all allocation indexes
    // means nothing is allocated currently
    for(int i = 0; i < proccesses; i++){
        allocation[i] = -1;
    }
    
    for(int i = 0; i < blocks; i++){
        occupied[i] = 0;
    }
 
    // pick each process and find suitable blocks
    // according to its size ad assign to it
    for (int i = 0; i < proccesses; i++)
    {
        
        int indexPlaced = -1;
        for (int j = 0; j < blocks; j++) { 
            if (blockSize[j] >= processSize[i] && !occupied[j])
            {
                // place it at the first block fit to accomodate process
                if (indexPlaced == -1)
                    indexPlaced = j;
                    
                // if any future block is smalller than the current block where
                // process is placed, change the block and thus indexPlaced
		// this reduces the wastage thus best fit
                else if (blockSize[j] < blockSize[indexPlaced])
                    indexPlaced = j;
            }
        }
 
        // If we were successfully able to find block for the process
        if (indexPlaced != -1)
        {
            // allocate this block j to process p[i]
            allocation[i] = indexPlaced;
            
            // make the status of the block as occupied
            occupied[indexPlaced] = 1;
        }
    }
 
    printf("\nProcess No.\tProcess Size\tBlock no.\n");
    for (int i = 0; i < proccesses; i++)
    {
        printf("%d \t\t\t %d \t\t\t", i+1, processSize[i]);
        if (allocation[i] != -1)
            printf("%d\n",allocation[i] + 1);
        else
            printf("Not Allocated\n");
    }
}
 
// Driver code
int main()
{
    int blockSize[] = {100, 50, 30, 120, 35};
    int processSize[] = {40, 10, 30, 60};
    int blocks = sizeof(blockSize)/sizeof(blockSize[0]);
    int proccesses = sizeof(processSize)/sizeof(processSize[0]);
 
    implimentBestFit(blockSize, blocks, processSize, proccesses);
 
    return 0 ;
}



FIRST FIT ALGORITHM


#include <stdio.h>

void implimentFirstFit(int blockSize[], int blocks, int processSize[], int processes)
{
    // This will store the block id of the allocated block to a process
    int allocate[processes];
    int occupied[blocks];

    // initially assigning -1 to all allocation indexes
    // means nothing is allocated currently
    for(int i = 0; i < processes; i++)
	{
		allocate[i] = -1;
	}
	
	for(int i = 0; i < blocks; i++){
        occupied[i] = 0;
    }
	
    // take each process one by one and find
    // first block that can accomodate it
    for (int i = 0; i < processes; i++)
    {
        for (int j = 0; j < blocks; j++) 
        { 
        if (!occupied[j] && blockSize[j] >= processSize[i])
            {
                // allocate block j to p[i] process
                allocate[i] = j;
                occupied[j] = 1;
 
                break;
            }
        }
    }

    printf("\nProcess No.\tProcess Size\tBlock no.\n");
    for (int i = 0; i < processes; i++)
    {
        printf("%d \t\t\t %d \t\t\t", i+1, processSize[i]);
        if (allocate[i] != -1)
            printf("%d\n",allocate[i] + 1);
        else
            printf("Not Allocated\n");
    }
}

void main()
{
    int blockSize[] = {30, 5, 10};
    int processSize[] = {10, 6, 9};
    int m = sizeof(blockSize)/sizeof(blockSize[0]);
    int n = sizeof(processSize)/sizeof(processSize[0]);
    
    implimentFirstFit(blockSize, m, processSize, n);
}


AVERAGE PAY OF EMPLOYEES

BEGIN {
    FS = ", "
    sum = 0
    count = 0
}

{
    if ($2 > 6000 && $3 > 4) {
        sum = sum + $2  # assuming $2 is the salary column
        count++
    }
}

END {
    if (count > 0) {  # changed from count > 1 to count > 0
        avg_salary = sum / count
        printf "The average salary of employees is %.2f\n", avg_salary
    } else {
        printf "No employee matches the criteria\n"
    }
}

save this content in vi emp.awk
sva the data in vi employees1.csv
to execute awk -f emp.awk employees1.csv


EGIN{PRINT "RESULT"}
{
count=0
if($2>45 && $3>45)
{
printf "The students is pass"
count++
}
else
{
c++
}
}
END{
printf "The total pass is :"count
printf "The total fail isn :"c
}
 store it in marks.awk
 name,mark1,mark2,mark3,mark4,mark5
bala,73,66,79,90,67
bhu,45,88,96,45,66

store it in marks.csv
awk -f marks.awk marks.csv to execute(do not add %d here remember it please)



SIGNAL CATCHING

#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
and write the program in it


FIFO ALGORITHM

#include < stdio.h >  
int main()  
{  
    int incomingStream[] = {4 , 1 , 2 , 4 , 5};  
    int pageFaults = 0;  
    int frames = 3;  
    int m, n, s, pages;   
    pages = sizeof(incomingStream)/sizeof(incomingStream[0]);   
    printf(" Incoming \ t Frame 1 \ t Frame 2 \ t Frame 3 ");  
    int temp[ frames ];  
    for(m = 0; m < frames; m++)  
    {  
        temp[m] = -1;  
    }  
    for(m = 0; m < pages; m++)  
    {  
        s = 0;   
        for(n = 0; n < frames; n++)  
        {  
            if(incomingStream[m] == temp[n])  
            {  
                s++;  
                pageFaults--;  
            }  
        }  
        pageFaults++;  
        if((pageFaults <= frames) && (s == 0))  
        {  
            temp[m] = incomingStream[m];  
        }  
        else if(s == 0)  
        {  
            temp[(pageFaults - 1) % frames] = incomingStream[m];  
        }  
        printf("\n");  
        printf("%d\t\t\t",incomingStream[m]);  
        for(n = 0; n < frames; n++)  
        {  
            if(temp[n] != -1)  
                printf(" %d\t\t\t", temp[n]);  
            else  
                printf(" - \t\t\t");  
        }  
    }  
    printf("\nTotal Page Faults:\t%d\n", pageFaults);  
    return 0;  
} 

Page:

A page is a unit of memory that is requested by a process or program.
In the program, the incomingStream array represents a sequence of page requests.
Each element of the incomingStream array is a page number, which is a unique identifier for a page.
Pages are the units of memory that are being managed by the program.
Frame:

A frame is a slot in physical memory that can hold a page.
In the program, the temp array represents the frames in physical memory.
Each element of the temp array corresponds to a frame, and its value represents the page number that is currently stored in that frame.
Frames are the physical memory locations where pages are stored.

DEADLOCK AVOIDANCE ALGORITHM

#include<stdio.h>  
int main()  
{  
    // P0 , P1 , P2 , P3 , P4 are the Process names here  
    int n , m , i , j , k;  
    n = 5; // Number of processes  
    m = 3; // Number of resources  
    int alloc[ 5 ] [ 3 ] = { { 0 , 1 , 0 }, // P0 // Allocation Matrix  
                        { 2 , 0 , 0 } , // P1  
                        { 3 , 0 , 2 } , // P2  
                        { 2 , 1 , 1 } , // P3  
                        { 0 , 0 , 2 } } ; // P4  
    int max[ 5 ] [ 3 ] = { { 7 , 5 , 3 } , // P0 // MAX Matrix  
                    { 3 , 2 , 2 } , // P1  
                    { 9 , 0 , 2 } , // P2  
                    { 2 , 2 , 2 } , // P3  
                    { 4 , 3 , 3 } } ; // P4  
    int avail[3] = { 3 , 3 , 2 } ; // Available Resources  
    int f[n] , ans[n] , ind = 0 ;  
    for (k = 0; k < n; k++) {  
        f[k] = 0;  
    }  
    int need[n][m];  
    for (i = 0; i < n; i++) {  
        for (j = 0; j < m; j++)  
            need[i][j] = max[i][j] - alloc[i][j] ;  
    }  
    int y = 0;  
    for (k = 0; k < 5; k++){  
        for (i = 0; i < n; i++){  
            if (f[i] == 0){  
                int flag = 0;  
                for (j = 0; j < m; j++) {  
                    if(need[i][j] > avail[j]){  
                        flag = 1;  
                        break;  
                    }  
                }  
                if ( flag == 0 ) {  
                    ans[ind++] = i;  
                    for (y = 0; y < m; y++)  
                        avail[y] += alloc[i][y] ;  
                    f[i] = 1;  
                }  
            }  
        }  
    }  
    int flag = 1;   
    for(int i=0;i<n;i++)  
    {  
    if(f[i] == 0)  
    {  
        flag = 0;  
        printf(" The following system is not safe ");  
        break;  
    }  
    }  
    if (flag == 1)  
    {  
    printf(" Following is the SAFE Sequence \ n ");  
    for (i = 0; i < n - 1; i++)  
        printf(" P%d -> " , ans[i]);  
    printf(" P%d ", ans[n - 1]);  
    }  
    return(0);  
}  
Output

Following is the SAFE Sequence
 P1 -> P3 -> P4 -> P0 -> P2


 FCFS ALGORITHM
 #include<stdio.h>
 
 int main()
 
{
    int n,bt[30],wait_t[30],turn_ar_t[30],av_wt_t=0,avturn_ar_t=0,i,j;
    printf("Please enter the total number of processes(maximum 30):");  // the maximum process that be used to calculate is specified.
    scanf("%d",&n);
 
    printf("\nEnter The Process Burst Timen");
    for(i=0;i<n;i++)  // burst time for every process will be taken as input
    {
        printf("P[%d]:",i+1);
        scanf("%d",&bt[i]);
    }
 
    wait_t[0]=0;   
 
    for(i=1;i<n;i++)
    {
        wait_t[i]=0;
        for(j=0;j<i;j++)
            wait_t[i]+=bt[j];
    }
 
    printf("\nProcess\t\tBurst Time\tWaiting Time\tTurnaround Time");
 
    for(i=0;i<n;i++)
    {
        turn_ar_t[i]=bt[i]+wait_t[i];
        av_wt_t+=wait_t[i];
        avturn_ar_t+=turn_ar_t[i];
        printf("\nP[%d]\t\t%d\t\t\t%d\t\t\t\t%d",i+1,bt[i],wait_t[i],turn_ar_t[i]);
    }
 
    av_wt_t/=i;
    avturn_ar_t/=i;  // average calculation is done here
    printf("\nAverage Waiting Time:%d",av_wt_t);
    printf("\nAverage Turnaround Time:%d",avturn_ar_t);
 
    return 0;
}

n the context of process scheduling, burst time refers to the amount of time a process requires to complete its execution. It is the time required by a process to execute its CPU-bound instructions. In other words, it is the time spent by a process in the running state, executing its instructions.

Burst time is an important parameter in process scheduling, as it determines how long a process will occupy the CPU. A process with a shorter burst time will complete its execution quickly, while a process with a longer burst time will take more time to complete.

Turnaround Time

Turnaround time is the total time taken by a process to complete its execution, from the time it arrives in the ready queue to the time it finishes its execution. It includes the time spent waiting in the ready queue, the time spent executing on the CPU, and any other overheads.

Turnaround time is an important performance metric in process scheduling, as it measures the responsiveness of the system. A shorter turnaround time indicates that the system is responsive and can complete processes quickly, while a longer turnaround time indicates that the system is slow and may be experiencing congestion.

The turnaround time can be calculated using the following formula:

Turnaround Time = Completion Time - Arrival Time

where:

Completion Time is the time at which the process finishes its execution
Arrival Time is the time at which the process arrives in the ready queue
In the code snippet I explained earlier, the turnaround time is calculated as the sum of the burst time and waiting time: turn_ar_t[i] = bt[i] + wait_t[i];. This is because the waiting time is the time spent waiting in the ready queue, and the burst time is the time spent executing on the CPU.


BEST FIT
The algorithm maintains a list of free memory blocks of various sizes.
When a process requests memory, the algorithm searches for the smallest free block that is large enough to satisfy the request.
If such a block is found, it is allocated to the process.
If no such block is found, the process is not allocated memory.

FIRST FIT

The OS maintains a list of free memory blocks of various sizes.
When a process requests memory, the OS searches for the smallest free block that is large enough to satisfy the request.
If such a block is found, it is allocated to the process.
If no such block is found, the process is not allocated memory.


