# Cryptography---19CS412-classical-techqniques


# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To develop a simple C program to implement Caeser Cipher.

## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
#include <stdio.h>
#include <string.h>

// Function to perform Caesar cipher encryption
void encrypt(char message[], int key) {
    int i;
    char ch;
    
    for(i = 0; message[i] != '\0'; ++i) {
        ch = message[i];
        
        if(ch >= 'a' && ch <= 'z') {
            ch = ch + key;
            
            if(ch > 'z') {
                ch = ch - 'z' + 'a' - 1;
            }
            
            message[i] = ch;
        }
        else if(ch >= 'A' && ch <= 'Z') {
            ch = ch + key;
            
            if(ch > 'Z') {
                ch = ch - 'Z' + 'A' - 1;
            }
            
            message[i] = ch;
        }
    }
}

// Function to perform Caesar cipher decryption
void decrypt(char message[], int key) {
    int i;
    char ch;
    
    for(i = 0; message[i] != '\0'; ++i) {
        ch = message[i];
        
        if(ch >= 'a' && ch <= 'z') {
            ch = ch - key;
            
            if(ch < 'a') {	
                ch = ch + 'z' - 'a' + 1;
            }
            
            message[i] = ch;
        }
        else if(ch >= 'A' && ch <= 'Z') {
            ch = ch - key;
            
            if(ch < 'A') {
                ch = ch + 'Z' - 'A' + 1;
            }
            
            message[i] = ch;
        }
    }
}

int main() {
    char message[100];
    int key;
    
    printf("Enter a message: ");
    gets(message); // Input message
    
    printf("Enter key: ");
    scanf("%d", &key); // Input key
    
    // Encrypt the message
    encrypt(message, key);
    printf("Encrypted message: %s\n", message);
    
    // Decrypt the message
    decrypt(message, key);
    printf("Decrypted message: %s\n", message);
    
    return 0;
}

## OUTPUT:
Enter a message: pavithra
Enter key: 3
Encrypted message: sdylwkud
Decrypted message: pavithra 
 
## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To develop a simple C program to implement PlayFair Cipher.

## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Function to generate the Playfair cipher key table
void generateKeyTable(char key[], char keyTable[][5]) {
    int i, j, k, len;
    int flag = 0, *dicty;
    
    // Initialize the dictionary array
    dicty = (int *)calloc(26, sizeof(int));
    
    len = strlen(key);
    
    // Mark all characters in the key as not present in dictionary
    for (i = 0; i < len; ++i) {
        dicty[key[i] - 97] = 2;
    }
    
    // Inserting the key
    i = 0;
    j = 0;
    
    for (k = 0; k < len; ++k) {
        if (dicty[key[k] - 97] == 2) {
            dicty[key[k] - 97] -= 1;
            keyTable[i][j++] = key[k];
            
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }
    
    // Fill the remaining elements of the table with the remaining characters
    char start = 'a';
    
    for (k = 0; k < 26; ++k) {
        if (dicty[k] == 0) {
            keyTable[i][j++] = start++;
            
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }
}

// Function to search for the characters in the key table and return their positions
void search(char keyTable[][5], char a, char b, int arr[]) {
    int i, j;
    
    if (a == 'j')
        a = 'i';
    else if (b == 'j')
        b = 'i';
    
    for (i = 0; i < 5; ++i) {
        for (j = 0; j < 5; ++j) {
            if (keyTable[i][j] == a) {
                arr[0] = i;
                arr[1] = j;
            }
            else if (keyTable[i][j] == b) {
                arr[2] = i;
                arr[3] = j;
            }
        }
    }
}

// Function to encrypt the message using the Playfair cipher
void encrypt(char str[], char keyTable[][5]) {
    int i, a[4];
    char a1, a2;
    
    for (i = 0; i < strlen(str); i += 2) {
        a1 = str[i];
        a2 = str[i + 1];
        
        if (a1 == a2)
            a2 = 'x';
        
        search(keyTable, a1, a2, a);
        
        // Case 1: When both the characters are in the same row
        if (a[0] == a[2]) {
            str[i] = keyTable[a[0]][(a[1] + 1) % 5];
            str[i + 1] = keyTable[a[0]][(a[3] + 1) % 5];
        }
        // Case 2: When both the characters are in the same column
        else if (a[1] == a[3]) {
            str[i] = keyTable[(a[0] + 1) % 5][a[1]];
            str[i + 1] = keyTable[(a[2] + 1) % 5][a[1]];
        }
        // Case 3: When both the characters are in different rows and columns
        else {
            str[i] = keyTable[a[0]][a[3]];
            str[i + 1] = keyTable[a[2]][a[1]];
        }
    }
}

// Function to decrypt the message using the Playfair cipher
void decrypt(char str[], char keyTable[][5]) {
    int i, a[4];
    char a1, a2;
    
    for (i = 0; i < strlen(str); i += 2) {
        a1 = str[i];
        a2 = str[i + 1];
        
        search(keyTable, a1, a2, a);
        
        // Case 1: When both the characters are in the same row
        if (a[0] == a[2]) {
            str[i] = keyTable[a[0]][(a[1] - 1 + 5) % 5];
            str[i + 1] = keyTable[a[0]][(a[3] - 1 + 5) % 5];
        }
        // Case 2: When both the characters are in the same column
        else if (a[1] == a[3]) {
            str[i] = keyTable[(a[0] - 1 + 5) % 5][a[1]];
            str[i + 1] = keyTable[(a[2] - 1 + 5) % 5][a[1]];
        }
        // Case 3: When both the characters are in different rows and columns
        else {
            str[i] = keyTable[a[0]][a[3]];
            str[i + 1] = keyTable[a[2]][a[1]];
        }
    }
}

int main() {
    char key[25], keyTable[5][5], str[100];
    int i, j, k, len;
    
    printf("Enter key: ");
    scanf("%s", key);
    
    printf("Enter a message: ");
    scanf("%s", str);
    
    // Convert key to lowercase
    len = strlen(key);
    for (i = 0; i < len; ++i) {
        key[i] = tolower(key[i]);
    }
    
    // Generate key table
    generateKeyTable(key, keyTable);
    
    // Convert message to lowercase
    len = strlen(str);
    for (i = 0; i < len; ++i) {
        str[i] = tolower(str[i]);
    }
    
    // Encrypt the message
    encrypt(str, keyTable);
    printf("Encrypted message: %s\n", str);
    
    // Decrypt the message
    decrypt(str, keyTable);
    printf("Decrypted message: %s\n", str);
    
    return 0;
}
## OUTPUT:
Enter key: 3
Enter a message: pavithra
Encrypted message: ufxgwepc
Decrypted message: pavithra
## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
#include<stdio.h>
#include<string.h>
int main() {
    unsigned int a[3][3] = { { 6, 24, 1 }, { 13, 16, 10 }, { 20, 17, 15 } };
    unsigned int b[3][3] = { { 8, 5, 10 }, { 21, 8, 21 }, { 21, 12, 8 } };
    int i, j;
    unsigned int c[20], d[20];
    char msg[20];
    int determinant = 0, t = 0;
    ;
    printf("Enter plain text\n ");
    scanf("%s", msg);
    for (i = 0; i < 3; i++) {
        c[i] = msg[i] - 65;
        printf("%d ", c[i]);
    }
    for (i = 0; i < 3; i++) {
        t = 0;
        for (j = 0; j < 3; j++) {
            t = t + (a[i][j] * c[j]);
        }
        d[i] = t % 26;
    }
    printf("\nEncrypted Cipher Text :");
    for (i = 0; i < 3; i++)
        printf(" %c", d[i] + 65);
    for (i = 0; i < 3; i++) {
        t = 0;
        for (j = 0; j < 3; j++) {
            t = t + (b[i][j] * d[j]);
        }
        c[i] = t % 26;
    }
    printf("\nDecrypted Cipher Text :");
    for (i = 0; i < 3; i++)
        printf(" %c", c[i] + 65);
    return 0;
}

## OUTPUT:
Enter plain text
 pavithra
47 32 53 
Encrypted Cipher Text : L P R
Decrypted Cipher Text : V G B

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
#include<stdio.h>
#include<string.h>

// Function to perform Vigenere encryption
void vigenereEncrypt(char* text, const char* key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);
    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Encrypt uppercase letters
            text[i] = ((c - 'A' + key[i % keyLen] - 'A') % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Encrypt lowercase letters
            text[i] = ((c - 'a' + key[i % keyLen] - 'A') % 26) + 'a';
        }
    }
}

// Function to perform Vigenere decryption
void vigenereDecrypt(char* text, const char* key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);
    for (int i = 0; i < textLen; i++) {
        char c = text[i];
        if (c >= 'A' && c <= 'Z') {
            // Decrypt uppercase letters
            text[i] = ((c - 'A' - (key[i % keyLen] - 'A') + 26) % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Decrypt lowercase letters
            text[i] = ((c - 'a' - (key[i % keyLen] - 'A') + 26) % 26) + 'a';
        }
    }
}

int main() {
    const char *key = "KEY"; // Replace with your desired key
    char message[] = "This is a secret message."; // Replace with your message
    // Encrypt the message
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);
    // Decrypt the message back to the original
    vigenereDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);
    return 0;
}

## OUTPUT:
Encrypted Message: zetsxfbe
Decrypted Message: pavithra
 
 
## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

## PROGRAM:
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

// Function to perform rail fence encryption
void encrypt(char str[], int rails) {
    int i, j, len, count = 0;
    int code[100][1000];
    len = strlen(str);
    
    for(i = 0; i < rails; i++) {
        for(j = 0; j < len; j++) {
            code[i][j] = 0;
        }
    }
    
    j = 0;
    while(j < len) {
        if(count % 2 == 0) {
            for(i = 0; i < rails; i++) {
                code[i][j] = (int)str[j];
                j++;
            }
        } else {
            for(i = rails - 2; i > 0; i--) {
                code[i][j] = (int)str[j];
                j++;
            }
        }
        count++;
    }
    
    printf("Encrypted message: ");
    for(i = 0; i < rails; i++) {
        for(j = 0; j < len; j++) {
            if(code[i][j] != 0)
                printf("%c", (char)code[i][j]);
        }
    }
    printf("\n");
}

// Function to perform rail fence decryption
void decrypt(char str[], int rails) {
    int i, j, len, count = 0;
    int code[100][1000];
    len = strlen(str);
    
    for(i = 0; i < rails; i++) {
        for(j = 0; j < len; j++) {
            code[i][j] = 0;
        }
    }
    
    j = 0;
    while(j < len) {
        if(count % 2 == 0) {
            for(i = 0; i < rails; i++) {
                code[i][j] = 1; // Indicator for filled cell
                j++;
            }
        } else {
            for(i = rails - 2; i > 0; i--) {
                code[i][j] = 1; // Indicator for filled cell
                j++;
            }
        }
        count++;
    }
    
    j = 0;
    for(i = 0; i < rails; i++) {
        for(j = 0; j < len; j++) {
            if(code[i][j] == 1)
                code[i][j] = (int)str[j];
        }
    }
    
    printf("Decrypted message: ");
    for(i = 0; i < rails; i++) {
        for(j = 0; j < len; j++) {
            if(code[i][j] != 0)
                printf("%c", (char)code[i][j]);
        }
    }
    printf("\n");
}

int main() {
    char str[1000];
    int rails;
    
    printf("Enter a Secret Message\n");
    gets(str);
    
    printf("Enter number of rails\n");
    scanf("%d", &rails);
    
    // Encrypt the message
    encrypt(str, rails);
    
    // Decrypt the message
    decrypt(str, rails);
    
    return 0;
}

## OUTPUT:
 Enter a Secret Message
pavi
Enter number of rails
3
Encrypted message: paiv
Decrypted message: p-a-v-i-
 

## RESULT:
The program is executed successfully
