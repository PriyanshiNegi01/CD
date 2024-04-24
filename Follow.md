```c
#include<stdio.h> 
#include<ctype.h> 
#include<string.h> 

// Function prototypes
void follow(char c); 
void followfirst(char c, int c1, int c2); 

// Global variables
int count, m = 0; 
char calc_follow[10][100]; 
char production[10][10]; 
char f[10]; 
char ck; 
int e; 

int main(int argc, char **argv) { 
    int km = 0; 
    count = 8; 
    
    // Given grammar productions
    strcpy(production[0], "E=TR"); 
    strcpy(production[1], "R=+TR"); 
    strcpy(production[2], "R=#"); 
    strcpy(production[3], "T=FY"); 
    strcpy(production[4], "Y=*FY"); 
    strcpy(production[5], "Y=#"); 
    strcpy(production[6], "F=(E)"); 
    strcpy(production[7], "F=i"); 
    
    char donee[count]; 
    int ptr = -1; 
    
    // Initialize calc_follow array with '!'
    for(int k = 0; k < count; k++) { 
        for(int kay = 0; kay < 100; kay++) { 
            calc_follow[k][kay] = '!'; 
        } 
    } 
    int point1 = 0; 
    
    // Loop through each non-terminal in the productions
    for(e = 0; e < count; e++) { 
        ck = production[e][0]; 
        int point2 = 0; 
        int xxx = 0; 
        
        // Check if Follow of ck has already been calculated
        for(int kay = 0; kay <= ptr; kay++) 
            if(ck == donee[kay]) 
                xxx = 1; 
                
        if (xxx == 1) 
            continue; 
        
        // Calculate Follow set for current non-terminal
        follow(ck); 
        ptr += 1; 
        donee[ptr] = ck; 
        printf(" Follow(%c) = { ", ck); 
        calc_follow[point1][point2++] = ck; 
        
        // Print the Follow set for the current non-terminal
        for(int i = 0 + km; i < m; i++) { 
            int lark = 0, chk = 0; 
            for(lark = 0; lark < point2; lark++) { 
                if (f[i] == calc_follow[point1][lark]) { 
                    chk = 1; 
                    break; 
                } 
            } 
            if(chk == 0) { 
                printf("%c, ", f[i]); 
                calc_follow[point1][point2++] = f[i]; 
            } 
        } 
        printf(" }\n\n"); 
        km = m; 
        point1++; 
    } 
} 

// Function to calculate Follow set of a given non-terminal
void follow(char c) { 
    int i, j; 
    
    // If c is the start symbol, add '$' to its Follow set
    if(production[0][0] == c) { 
        f[m++] = '$'; 
    } 
    // Loop through each production
    for(i = 0; i < 10; i++) { 
        for(j = 2;j < 10; j++) { 
            // If c is found in a production
            if(production[i][j] == c) { 
                // Calculate First set of the symbol after c
                if(production[i][j+1] != '\0') { 
                    followfirst(production[i][j+1], i, (j+2)); 
                } 
                // If c is at the end of a production and not the starting symbol
                if(production[i][j+1]=='\0' && c!=production[i][0]) { 
                    follow(production[i][0]); 
                } 
            } 
        } 
    } 
} 

// Function to calculate First set of a given symbol
void followfirst(char c, int c1, int c2) { 
    int k; 
    
    // If c is a terminal, add it to the Follow set
    if(!(isupper(c))) 
        f[m++] = c; 
    else { 
        // If c is a non-terminal, add its First set to the Follow set
        int i = 0, j = 1; 
        for(i = 0; i < count; i++) { 
            if(calc_follow[i][0] == c) 
                break; 
        } 
        
        // Include the First set of c in the Follow set
        while(calc_follow[i][j] != '!') { 
            if(calc_follow[i][j] != '#') { 
                f[m++] = calc_follow[i][j]; 
            } 
            else { 
                // If '#' is in the First set of c, recursively calculate Follow set
                if(production[c1][c2] == '\0') { 
                    follow(production[c1][0]); 
                } 
                else { 
                    followfirst(production[c1][c2], c1, c2+1); 
                } 
            } 
            j++; 
        } 
    } 
}
```
![image](https://github.com/PriyanshiNegi01/CD/assets/121029180/13bc5021-8489-4650-abcf-31a0bf5771da)
