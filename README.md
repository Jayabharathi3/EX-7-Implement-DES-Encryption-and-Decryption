# EX-7 : Implement Data Encryption Standard (DES) Algorithm

## AIM:
  To write a program to implement Data Encryption Standard (DES).
  
## ALGORITHM:
 ### STEP-1: 
 Read the 64-bit plain text.
 ### STEP-2: 
Split it into two 32-bit blocks and store it in two different arrays.
 ### STEP-3:
Perform XOR operation between these two arrays.
 ### STEP-4:
 The output obtained is stored as the second 32-bit sequence and the original second 32-bit sequence forms the first part.
 ### STEP-5: 
 Thus the encrypted 64-bit cipher text is obtained in this way. Repeat the same process for the remaining plain text characters.

## PROGRAM:
```C
#include <stdio.h>
#include <string.h>
#include <stdint.h>

uint64_t stringToBinary(const char *str)
{
    uint64_t binary = 0;
    for (int i = 0; i < 8 && str[i] != '\0'; ++i)
    {
        binary <<= 8;
        binary |= (uint64_t)(unsigned char)str[i]; 
    }
    return binary;
}

uint32_t XOR(uint32_t a, uint32_t b)
{
    return a ^ b;
}

uint64_t encryptDES(uint64_t plainText)
{
    uint32_t left = (plainText >> 32) & 0xFFFFFFFF;
    uint32_t right = plainText & 0xFFFFFFFF;
    uint32_t xorResult = XOR(left, right);
    uint64_t cipherText = 0;
    cipherText = ((uint64_t)right << 32) | xorResult;
    return cipherText;
}

uint64_t decryptDES(uint64_t cipherText)
{
    uint32_t left = (cipherText >> 32) & 0xFFFFFFFF;
    uint32_t right = cipherText & 0xFFFFFFFF;
    uint32_t xorResult = XOR(left, right);
    uint64_t plainText = 0;
    plainText = ((uint64_t)xorResult << 32) | right; 
    return plainText;
}

void binaryToString(uint64_t binary, char *str)
{
    for (int i = 0; i < 8; i++)
    {
        str[i] = (binary >> (56 - i * 8)) & 0xFF; 
    }
    str[8] = '\0'; 
}

int main()
{
    char plainText[9];  
    printf("Enter an 8-character plaintext: ");
    fgets(plainText, sizeof(plainText), stdin);
    plainText[strcspn(plainText, "\n")] = 0;  

    // Ensure the input is exactly 8 characters
    if (strlen(plainText) != 8) {
        printf("Please enter exactly 8 characters.\n");
        return 1;
    }

    uint64_t binaryPlainText = stringToBinary(plainText);
    uint64_t cipherText = encryptDES(binaryPlainText);
    printf("Encrypted Cipher Text (in hex): %016llX\n", cipherText);

    uint64_t decryptedText = decryptDES(cipherText);
    char decryptedString[9];
    binaryToString(decryptedText, decryptedString);
    
    printf("Decrypted String: %s\n", decryptedString);
    return 0;
}


```

## OUTPUT:
![image](https://github.com/user-attachments/assets/ba9326d0-3535-4597-bd67-4256353f7056)


## RESULT:
  Thus the data encryption standard algorithm had been implemented successfully.
