## EX NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
## AIM:
To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




## Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define MX 5

void encryptPair(char ch1, char ch2, char key[MX][MX])
{
    int i, j, row1 = -1, col1 = -1, row2 = -1, col2 = -1;
    for (i = 0; i < MX; i++)
    {
        for (j = 0; j < MX; j++)
        {
            if (key[i][j] == ch1)
            {
                row1 = i;
                col1 = j;
            }
            else if (key[i][j] == ch2)
            {
                row2 = i;
                col2 = j;
            }
        }
    }
    if (row1 == row2)
        printf("%c%c", key[row1][(col1 + 1) % 5], key[row2][(col2 + 1) % 5]);
    else if (col1 == col2)
        printf("%c%c", key[(row1 + 1) % 5][col1], key[(row2 + 1) % 5][col2]);
    else
        printf("%c%c", key[row1][col2], key[row2][col1]);
}

void decryptPair(char ch1, char ch2, char key[MX][MX])
{
    int i, j, row1 = -1, col1 = -1, row2 = -1, col2 = -1;
    for (i = 0; i < MX; i++)
    {
        for (j = 0; j < MX; j++)
        {
            if (key[i][j] == ch1)
            {
                row1 = i;
                col1 = j;
            }
            else if (key[i][j] == ch2)
            {
                row2 = i;
                col2 = j;
            }
        }
    }
    if (row1 == row2)
        printf("%c%c", key[row1][(col1 + 4) % 5], key[row2][(col2 + 4) % 5]);
    else if (col1 == col2)
        printf("%c%c", key[(row1 + 4) % 5][col1], key[(row2 + 4) % 5][col2]);
    else
        printf("%c%c", key[row1][col2], key[row2][col1]);
}

int main()
{
    int i, j, k = 0, m = 0;
    char key[MX][MX], keyminus[25], keystr[25], plaintext[100];
    char alpha[26] = "ABCDEFGHIKLMNOPQRSTUVWXYZ";

    printf("Simulating Playfair Cipher\n");
    printf("Key text: ");
    fgets(keystr, sizeof(keystr), stdin);
    keystr[strcspn(keystr, "\n")] = 0;

    printf("Plain text: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = 0;

    for (i = 0; i < strlen(keystr); i++)
    {
        if (tolower(keystr[i]) == 'j')
            keystr[i] = 'I';
        keystr[i] = toupper(keystr[i]);
    }

    for (i = 0; i < strlen(plaintext); i++)
    {
        if (tolower(plaintext[i]) == 'j')
            plaintext[i] = 'I';
        plaintext[i] = toupper(plaintext[i]);
    }

    int n = strlen(keystr), found;
    for (i = 0; i < 25; i++)
    {
        found = 0;
        for (j = 0; j < n; j++)
        {
            if (keystr[j] == alpha[i])
            {
                found = 1;
                break;
            }
        }
        if (!found)
            keyminus[m++] = alpha[i];
    }

    k = 0;
    m = 0;
    for (i = 0; i < MX; i++)
    {
        for (j = 0; j < MX; j++)
        {
            if (k < n)
                key[i][j] = keystr[k++];
            else
                key[i][j] = keyminus[m++];
        }
    }

    printf("Cipher text: ");
    for (i = 0; i < strlen(plaintext); i++)
    {
        if (plaintext[i + 1] == '\0')
        {
            encryptPair(plaintext[i], 'X', key);
        }
        else if (plaintext[i] == plaintext[i + 1])
        {
            encryptPair(plaintext[i], 'X', key);
        }
        else
        {
            encryptPair(plaintext[i], plaintext[i + 1], key);
            i++;
        }
    }

    printf("\nDecrypted text: %s\n", plaintext);
    return 0;
}

```

## Output:
![image](https://github.com/user-attachments/assets/b809b84f-f57c-41be-9de0-b5db54294ede)

## RESULT:
The program is executed successfully.
