```c
#include<stdio.h>
#include<string.h>
#define SIZE 10

int main () {
    char non_terminal;
    char beta[SIZE], alpha[SIZE];
    int num;
    char production[10][SIZE];
    int index=3; /* starting of the string following "->" */
    printf("Enter the number of Productions: ");
    scanf("%d",&num);
    printf("Enter the grammar as E->E-A :\n");
    for(int i=0;i<num;i++){
        scanf("%s",production[i]);
    }
    for(int i=0;i<num;i++){
        printf("\nGRAMMAR: %s",production[i]);
        non_terminal=production[i][0];
        if(non_terminal==production[i][index]) {
            int j = 0, k = 0;
            index += 2; // Skip non-terminal and "->"
            while (production[i][index] != '|' && production[i][index] != '\0') {
                beta[j++] = production[i][index++];
            }
            beta[j] = '\0'; // Null-terminate beta
            if (production[i][index] == '|') {
                index++; // Skip '|'
                while (production[i][index] != '\0') {
                    alpha[k++] = production[i][index++];
                }
            }
            alpha[k] = '\0'; // Null-terminate alpha
            printf(" is left recursive.\n");
            printf("Grammar without left recursion:\n");
            printf("%c->%s%c\'\n", non_terminal, beta, non_terminal);
            printf("%c\'->%s%c\'|E\n", non_terminal, alpha, non_terminal);
        }
        else
            printf(" is not left recursive.\n");
    }
    return 0;
}
```
OUTPUT:
<br/>
Enter the number of Productions: 1
<br/>
Enter the grammar as E->E-A :
<br/>
E->E+T|T
<br/>

GRAMMAR: E->E+T|T is left recursive.
<br/>
Grammar without left recursion:
<br/>
E->TE'
<br/>
E'->TE'|E
<br/>
