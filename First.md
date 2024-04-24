```c
#include<stdio.h> 
#include<ctype.h> 
#include<string.h> 

// Function to calculate First sets
void findfirst(char, int, int); 

int count, n = 0; 
char calc_first[10][100];  // Array to store First sets
char production[10][10];   // Array to store productions
char first[10];   // Array to store First set of each non-terminal
int k; 

int main(int argc, char **argv) 
{ 
	int jm = 0; 
	int i; 
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
	
	int kay; 
	char done[count];   // Array to keep track of processed non-terminals
	int ptr = -1; 
	
	// Initializing calc_first array with '!'
	for(k = 0; k < count; k++) { 
		for(kay = 0; kay < 100; kay++) { 
			calc_first[k][kay] = '!'; 
		} 
	} 
	int point1 = 0, point2, xxx; 
	
	// Loop through each non-terminal in the productions
	for(k = 0; k < count; k++) { 
		char c = production[k][0];   // Current non-terminal
		point2 = 0; 
		xxx = 0; 
		
		// Check if First of c has already been calculated
		for(kay = 0; kay <= ptr; kay++) 
			if(c == done[kay]) 
				xxx = 1; 
				
		if (xxx == 1) 
			continue; 
		
		// Calculate First set for current non-terminal
		findfirst(c, 0, 0); 
		ptr += 1; 
		done[ptr] = c; 
		printf("\n First(%c) = { ", c); 
		calc_first[point1][point2++] = c; 
		
		// Print the First set for the current non-terminal
		for(i = 0 + jm; i < n; i++) { 
			int lark = 0, chk = 0; 
			for(lark = 0; lark < point2; lark++) { 
				if (first[i] == calc_first[point1][lark]) { 
					chk = 1; 
					break; 
				} 
			} 
			if(chk == 0) { 
				printf("%c, ", first[i]); 
				calc_first[point1][point2++] = first[i]; 
			} 
		} 
		printf("}\n"); 
		jm = n; 
		point1++; 
	} 
} 

// Function to calculate First set of a given non-terminal
void findfirst(char c, int q1, int q2) { 
	int j; 
	
	// If c is a terminal, add it to the First set
	if(!(isupper(c))) { 
		first[n++] = c; 
	} 
	// Loop through grammar productions
	for(j = 0; j < count; j++) { 
		if(production[j][0] == c) { 
			if(production[j][2] == '#') { 
				if(production[q1][q2] == '\0') 
					first[n++] = '#'; 
				else if(production[q1][q2] != '\0'
						&& (q1 != 0 || q2 != 0)) { 
					// Recursively calculate First set for next symbol
					findfirst(production[q1][q2], q1, (q2+1)); 
				} 
				else
					first[n++] = '#'; 
			} 
			else if(!isupper(production[j][2])) { 
				first[n++] = production[j][2]; 
			} 
			else { 
				// Recursively calculate First set for next non-terminal
				findfirst(production[j][2], j, 3); 
			} 
		} 
	} 
}
```

![image](https://github.com/PriyanshiNegi01/CD/assets/121029180/2f0873f5-af9d-488c-8518-6bfdbdfc2629)
