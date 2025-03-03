# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values,m

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
PROGRAM:
CaearCipher.
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
void main()
{
 char plain[10], cipher[10];
 int key,i,length;
 int result;
 printf("\n Enter the plain text:");
 scanf("%s", plain);
 printf("\n Enter the key value:");
 scanf("%d", &key);
 printf("\n \n \t PLAIN TEXt: %s",plain);
 printf("\n \n \t ENCRYPTED TEXT: ");
 for(i = 0, length = strlen(plain); i < length; i++)
 {
 cipher[i]=plain[i] + key;
if (isupper(plain[i]) && (cipher[i] > 'Z'))
 cipher[i] = cipher[i] - 26;
 if (islower(plain[i]) && (cipher[i] > 'z'))
 cipher[i] = cipher[i] - 26;
 printf("%c", cipher[i]);
 }
 printf("\n \n \t AFTER DECRYPTION : ");
 for(i=0;i<length;i++)
 {
 plain[i]=cipher[i]-key;
 if(isupper(cipher[i])&&(plain[i]<'A'))
 plain[i]=plain[i]+26;
 if(islower(cipher[i])&&(plain[i]<'a'))
 plain[i]=plain[i]+26;
 printf("%c",plain[i]);
 }
}
```
## OUTPUT:
OUTPUT:

<img width="406" alt="cry 1" src="https://github.com/user-attachments/assets/4cf76945-7aeb-4b50-a85b-4b1e737cc353" />

## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void generateKeyTable(char key[], int keyLength, char keyTable[SIZE][SIZE]) {
    int used[26] = {0};  // To track used characters
    int i, j, k = 0;

    for (i = 0; i < keyLength; i++) {
        if (key[i] != 'j') {
            if (used[key[i] - 'a'] == 0) {
                used[key[i] - 'a'] = 1;
                keyTable[k / SIZE][k % SIZE] = key[i];
                k++;
            }
        }
    }

    for (i = 0; i < 26; i++) {
        if (i + 'a' != 'j' && used[i] == 0) {
            keyTable[k / SIZE][k % SIZE] = i + 'a';
            k++;
        }
    }
}

void search(char keyTable[SIZE][SIZE], char a, char b, int *row1, int *col1, int *row2, int *col2) {
    int i, j;

    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            if (keyTable[i][j] == a) {
                *row1 = i;
                *col1 = j;
            } else if (keyTable[i][j] == b) {
                *row2 = i;
                *col2 = j;
            }
        }
    }
}

void encryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
    int row1, col1, row2, col2;

    search(keyTable, a, b, &row1, &col1, &row2, &col2);

    if (row1 == row2) {
        *x = keyTable[row1][(col1 + 1) % SIZE];
        *y = keyTable[row2][(col2 + 1) % SIZE];
    } else if (col1 == col2) {
        *x = keyTable[(row1 + 1) % SIZE][col1];
        *y = keyTable[(row2 + 1) % SIZE][col2];
    } else {
        *x = keyTable[row1][col2];
        *y = keyTable[row2][col1];
    }
}

void decryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
    int row1, col1, row2, col2;

    search(keyTable, a, b, &row1, &col1, &row2, &col2);

    if (row1 == row2) {
        *x = keyTable[row1][(col1 + SIZE - 1) % SIZE];
        *y = keyTable[row2][(col2 + SIZE - 1) % SIZE];
    } else if (col1 == col2) {
        *x = keyTable[(row1 + SIZE - 1) % SIZE][col1];
        *y = keyTable[(row2 + SIZE - 1) % SIZE][col2];
    } else {
        *x = keyTable[row1][col2];
        *y = keyTable[row2][col1];
    }
}

void encrypt(char keyTable[SIZE][SIZE], char plainText[], char cipherText[]) {
    int i, j = 0;
    char a, b;

    for (i = 0; plainText[i] != '\0'; i += 2) {
        a = plainText[i];
        if (plainText[i + 1] != '\0') {
            b = plainText[i + 1];
        } else {
            b = 'x';
        }

        if (a == b) {
            b = 'x';
            i--;
        }

        encryptPair(keyTable, a, b, &cipherText[j], &cipherText[j + 1]);
        j += 2;
    }
    cipherText[j] = '\0';
}

void decrypt(char keyTable[SIZE][SIZE], char cipherText[], char plainText[]) {
    int i, j = 0;
    char a, b;

    for (i = 0; cipherText[i] != '\0'; i += 2) {
        a = cipherText[i];
        b = cipherText[i + 1];

        decryptPair(keyTable, a, b, &plainText[j], &plainText[j + 1]);
        j += 2;
    }
    plainText[j] = '\0';
}

int main() {
    char key[100], plainText[100], cipherText[100], decryptedText[100];
    char keyTable[SIZE][SIZE];
    int i, keyLength;

    printf("Enter the key: ");
    scanf("%s", key);

    for (i = 0; key[i] != '\0'; i++) {
        key[i] = tolower(key[i]);
    }

    keyLength = strlen(key);
    generateKeyTable(key, keyLength, keyTable);

    printf("Enter the plaintext: ");
    scanf("%s", plainText);

    for (i = 0; plainText[i] != '\0'; i++) {
        plainText[i] = tolower(plainText[i]);
    }

    encrypt(keyTable, plainText, cipherText);
    printf("Encrypted Text: %s\n", cipherText);

    decrypt(keyTable, cipherText, decryptedText);
    printf("Decrypted Text: %s\n", decryptedText);

    return 0;
}
```




## OUTPUT:
Output:
![377319144-68073362-617b-4698-aa44-9f8e4ad02ed6](https://github.com/user-attachments/assets/81c6c520-3294-44ea-86d6-264f901594d1)


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
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:
PROGRAM:
```
#include <stdio.h>

int main() 
{
    unsigned int key[3][3] = {{6, 24, 1}, {13, 16, 10}, {20, 17, 15}};
    unsigned int inverseKey[3][3] = {{8, 5, 10}, {21, 8, 21}, {21, 12, 8}};

    char msg[4];
    unsigned int enc[3] = {0}, dec[3] = {0};

    printf("Enter plain text: ");
    scanf("%3s", msg);

    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            enc[i] += key[i][j] * (msg[j] - 'A') % 26;

    printf("Encrypted Cipher Text: %c%c%c\n", enc[0] % 26 + 'A', enc[1] % 26 + 'A', enc[2] % 26 + 'A');

    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            dec[i] += inverseKey[i][j] * enc[j] % 26;

    printf("Decrypted Cipher Text: %c%c%c\n", dec[0] % 26 + 'A', dec[1] % 26 + 'A', dec[2] % 26 + 'A');

    return 0;
}
```
## OUTPUT:
OUTPUT:
<img width="218" alt="02 cry" src="https://github.com/user-attachments/assets/92d3e4f8-ecd3-4783-a965-65d5c1c9ca66" />



Input Message : SecurityLaboratory
Padded Message : SECURITYLABORATORY Encrypted Message : EACSDKLCAEFQDUKSXU Decrypted Message : SECURITYLABORATORY
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
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:
PROGRAM:
#include<stdio.h> #include<string.h>
//FunctiontoperformVigenereencryption voidvigenereEncrypt(char*text,constchar*key){ inttextLen= strlen(text);
intkeyLen=strlen(key); for(inti =0;i< textLen;i++){ charc =text[i]; if(c>='A'&&c<='Z'){
//Encryptuppercaseletters
text[i]=((c-'A'+key[i%keyLen]-'A')%26)+'A';
}else if(c>='a'&&c<='z'){
//Encryptlowercaseletters
text[i]=((c-'a'+key[i%keyLen]-'A')%26)+'a';
}
}
}
//FunctiontoperformVigeneredecryption voidvigenereDecrypt(char*text,constchar*key){ inttextLen= strlen(text);
intkeyLen=strlen(key);

for(inti =0;i< textLen;i++){ charc =text[i]; if(c>='A'&&c<='Z'){
//Decryptuppercaseletters
 
text[i]=((c-'A'-(key[i% keyLen]-'A') +26) %26)+ 'A';
}else if(c>='a'&&c<='z'){
//Decryptlowercaseletters
text[i]=((c-'a'-(key[i% keyLen]-'A') +26) %26)+ 'a';
}
}
}
intmain(){
constchar *key="KEY";//Replacewithyourdesired key
char message[]= "Thisisasecretmessage.";//Replace withyourmessage
//Encrypt themessage vigenereEncrypt(message,key); printf("EncryptedMessage:%s\n",message);
//Decrypt themessage backtotheoriginal vigenereDecrypt(message,key); printf("DecryptedMessage:%s\n",message); Return 0;

## OUTPUT:
OUTPUT :

Simulating Vigenere Cipher


Input Message : SecurityLaboratory
Encrypted Message : NMIYEMKCNIQVVROWXC Decrypted Message : SECURITYLABORATORY
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
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:

PROGRAM:
#include<stdio.h> #include<string.h> #include<stdlib.h> main()
{
int i,j,len,rails,count,code[100][1000]; char str[1000];
printf("Enter a Secret Message\n"); gets(str);
len=strlen(str);
printf("Enter number of rails\n"); scanf("%d",&rails); for(i=0;i<rails;i++)
{
for(j=0;j<len;j++)
{
code[i][j]=0;
}
}
count=0; j=0;
while(j<len)
{
if(count%2==0)
{
for(i=0;i<rails;i++)
{
//strcpy(code[i][j],str[j]);
code[i][j]=(int)str[j]; j++;
}

}
else
{
 
for(i=rails-2;i>0;i--)
{
code[i][j]=(int)str[j]; j++;
}
}

count++;
}

for(i=0;i<rails;i++)
{
for(j=0;j<len;j++)
{
if(code[i][j]!=0) printf("%c",code[i][j]);
}
}
printf("\n");
}
## OUTPUT:
OUTPUT:
Enter a Secret Message wearediscovered
Enter number of rails 2
waeicvrderdsoee
## RESULT:
The program is executed successfully
