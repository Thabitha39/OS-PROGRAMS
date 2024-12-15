# OS-PROGRAMS

**LRU**
#include <stdio.h>
int n,pg[30], fr[10];
void lru();
int main()
{
int i;
printf("Enter total number of pages:\n"); 
scanf("%d",&n);
printf("Enter page sequence:\n");
for (i = 0; i < n; i++)
scanf("%d",&pg[i]);
lru();
return 0;
}
void lru()
{
int count[10],i,j,fault=0, flag,f, temp,min,m, x;
printf("Enter the frame size:\n"); 
scanf("%d",&f);
for (i = 0; i < f; i++)
{
fr[i] = -1;
count[i]=-1;
}
for(i =0;i <n; i++)
{
flag = 0; temp=pg[i];
for (j = 0; j < f; j++)
{
if(fr[j]==temp)
{
flag=1;
count[j] = i;
}
} 
if (flag == 0)
{
if (fault < f)
{
fr[fault]= temp;
count[fault]=i;
fault++;
}
else
{
min = 0;
for(m=1;m<f; m++)
{
if(count[m]<count[min])
{
min=m;
}
}
fr[min] = temp;
count[min] = i;
fault++;
}
}
printf("\n Page frames after accessing page %d:\t",temp); 
for (x = 0; x < f; x++)
{
if(fr[x]!=-1)
printf("%d\t",fr[x]);
else
printf("-\t");
}
}
printf("\n \n Total number of page faults =%d\n", fault);
}

**OPTIMAL**
#include <stdio.h>
#include <stdbool.h>
bool contains(int frame[], int frameSize, int page)
{
 for (int i = 0; i < frameSize; i++) 
{
 if (frame[i] == page)
{
 return true;
 }
 }
 return false;
}
int findOptimalPage(int pages[], int n, int frame[], int frameSize, int currentIndex) 
{
 int res = -1, farthest = currentIndex;
 for (int i = 0; i < frameSize; i++)
{
 int j;
 for (j = currentIndex; j < n; j++)
{
 if (frame[i] == pages[j])
{
 if (j > farthest) 
{
farthest = j;
 res = i;
 }
 break;
 }
 }
 if (j == n)
 return i;
 }
 return (res == -1) ? 0 : res;
}
void optimalPageReplacement(int pages[], int n, int frameSize) 
{
 int frame[frameSize];
 bool isPageFault[n];
 int pageFaults = 0;
 for (int i = 0; i < frameSize; i++)
 frame[i] = -1;
 for (int i = 0; i < n; i++)
 isPageFault[i] = false;
 for (int i = 0; i < n; i++)
{
 if (!contains(frame, frameSize, pages[i])) 
{
 int replaceIndex;
 if (i < frameSize) 
{
 replaceIndex = i;
 }
else
{
 replaceIndex = findOptimalPage(pages, n, frame, frameSize, i + 1);
 }
 frame[replaceIndex] = pages[i];
 isPageFault[i] = true;
 pageFaults++;
 printf("Frame state after inserting page %d: ", pages[i]);
 for (int j = 0; j < frameSize; j++)
{
 if (frame[j] != -1)
 printf("%d ", frame[j]);
 else
 printf("- ");
}
printf("\n");
 }
 }
 printf("Total Page Faults: %d\n", pageFaults);
}
int main() 
{
 int pages[50], n, frameSize, i;
 printf("Enter the number of pages: ");
 scanf("%d", &n);
 printf("Enter the page sequence of %d pages: ", n);
 for (i = 0; i < n; i++)
 scanf("%d", &pages[i]);
 printf("Enter the frame size: ");
 scanf("%d", &frameSize);
 optimalPageReplacement(pages, n, frameSize);
return 0;
}
