# Cyclic Redundancy Code (CRC)
```c
#include <stdio.h>
#define DEGREE 16
int mod2add(int,int);
int getch(int array[],int pos);
int result[30];
void calc_CRC(int length)
{
    int ccitt[]={1,0,0,0,1,0,0,0,0,0,0,1,0,0,0,0,1};
    int i=0,pos=0,newpos;
    while(pos<length-DEGREE)
    {
        for(i=pos;i<pos+DEGREE;++i)
            result[i]=mod2add(result[i],ccitt[i-pos]);
        newpos=getnext(result,pos);

        if(newpos>pos+1)
            pos=newpos-1;
        ++pos;
    }
}
int getch(int array[],int pos)
{
    int i=pos;
    while(array[i]==0)
        ++i;
    return i;
}
int mod2add(int x,int y)
{
    return(x==y?0:1);
}
void main()
{
    int array[30],ch;
    int length,i=0;
    clrscr();

    printf("enter the data stream:");
    while((ch=getch())!='\r')
        array[i++]=ch-'0';
    length=i;
    for(i=0;i<DEGREE;++i)
        array[i+length]=0;
    length=length+DEGREE;
    for(i=0;i<length;i++)
        result[i]=array[i];
    calc_CRC(length);
    printf("\n the transmitted frame is:");
    for(i=0;i<length-DEGREE;++i)
        printf("%d",array[i]);
    for(i=length-DEGREE;i<length;++i)
        printf("%d",result[i]);
    printf("\n Enter the stream for which CRC has to be checked:");
    i=0;
    while((ch=getch())!='\r')
        array[i++]=ch-'0';
    length=i;
    for(i=0;i<length;i++)
        result[i]=array[i];
    calc_CRC(length);
    printf("\n Checksum:");
    for(i=length-DEGREE;i<length;++i)
        printf("%d",result[i]);
    getch();
}
```

## Expected Output
```
Enter the Data Stream: 10011
The transmitted frame is: 100110010001001010010
Enter the stream for which CRC has to be checked: 100110010001001010010
Checksum: 000000000000000
```
