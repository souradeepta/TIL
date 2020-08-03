# Difference Between MD5 and SHA1

The MD5 and SHA1 are the hashing algorithms where MD5 is better than SHA in terms of speed. However, SHA1 is more secure as compared to MD5. The concept behind these hashing algorithms is that these are used to generate a unique digital fingerprint of data or message which is known as a hash or digest.

The essential features of hash algorithms are:

- These functions cannot be reversed.
- Size of the digest or hash is always fixed and does not depend on the size of the data.
- It is always unique, no two distinct data set are able to produce a similar hash.

Hash algorithm primary purpose is the verification of the files instead of encryption of the file or message. I should not be used for storing the information or securing it.



## Content: MD5 Vs SHA1

1. 1. [Comparison Chart](https://techdifferences.com/difference-between-md5-and-sha1.html#ComparisonChart)
   2. [Definition](https://techdifferences.com/difference-between-md5-and-sha1.html#Definition)
   3. [Key Differences](https://techdifferences.com/difference-between-md5-and-sha1.html#KeyDifferences)
   4. [Conclusion](https://techdifferences.com/difference-between-md5-and-sha1.html#Conclusion)



### Comparison Chart



|                    BASIS FOR COMPARISON                     |              MD5               |            SHA1             |
| :---------------------------------------------------------: | :----------------------------: | :-------------------------: |
|                         Stands for                          |         Message Digest         |    Secure Hash Algorithm    |
|                  Length of Message Digest                   |            128 bits            |          160 bits           |
|        Discerning of original message would require         |        2128 operations         |       2160 operations       |
| For finding two messages generating the same message digest | 264 operations would be needed | 280 operations are required |
|                          Security                           |              Poor              |          Moderate           |
|                            Speed                            |              Fast              |            Slow             |





### Definition of MD5

The **MD5** is a hashing algorithm known as a **message digest algorithm** introduced by Ron Rivest. However, there are several version of MD is created among which the first was the MD (message digest algorithm) followed by MD2, MD3, MD4 and at last MD5. These versions of MD was improving one after the other, the newer version was better than the old one.

MD5 was the fastest algorithm produced in that era and was able to protect itself from collisions. At present, it may not assure its security as the hacker and technology discovered the way to create collisions. It creates 128-bit messages where input text is processed in 512-bit blocks which are further separated into 16 32-bit sub-blocks. The result of the MD5 algorithm built a set of four 32-bit blocks which creates a 128-bit message digest.

#### MD5 Working

Now let’s understand the whole process step by step:

- **Padding** – It is the first step of MD5 in which padding bits are added to the original message to make the original message equal to a value divisible by 512.

- **Append length** – It is used for calculating the original message length (i.e. excluding padding bits) expressed in 64-bit value then adding that to the end of the original message along with padding bits.

- **Division of input** – In this step, the input message is divided into 512-bit blocks.

- **Initialize chaining variables** – Now, we initialize the four 32-bit chaining variable named as A, B, C and D.

- Process block

   

  – In this step, the actual algorithm starts working. It’s not simple to explain the process block at a span, so we are diving this step into substep.

  - **a:** The four chaining variables are copied into four corresponding variables a,b,c, and d. Therefore, we have a=A, b=B, c=C and d=D. The combination of a,b,c and d is viewed as a single register which is capable of containing the intermediate and final results.
  - **b:** This step performs the division over the 512-bit block to make 16 sub-blocks of 32-bit each.
  - **c:** Now, all the 16 sub-blocks are processed in four rounds. Each round has 16 sub-blocks input accompanied by the variables a,b,c,d and some constants. A single round is explained in the diagram given below.![MD5 operation](https://techdifferences.com/wp-content/uploads/2019/04/MD5.jpg)

These rounds vary in one major manner: step 1 of the four rounds could have different processing while the other steps in all four rounds are the same.

Each round is consist of 16 input sub-blocks named as M[0], M[1],…., M[15] or commonly, M[i], where i ranges from 0-15. Additionally, there is an array of constants are given as input denoted by t, t[1], t[2],…, t[64] or in general as t[k] where k varies from 1 to 64 but only 16 are used out of the 64 values of t.

##### A single MD5 operation is expressed by an equation:

**a = b + ((a + Process P (b, c, d)+ M[i] + T[k])<<<s)**

Where,
a, b, c, d = Chaining variables
Process P = A non-linear operation
M[i] = M[q x 16 + i], which is the ith 32-bit word in the qth 512-bit block of the message
t[k] = A constant
<<<s = Circular-left shift by s bits

### Definition of SHA1

SHA is a hash algorithm developed and published by the collaboration of NIST and NSA in 1993 as a Federal Information Processing Standard (FIPS PUB 180). **SHA1** was the revised version of SHA published in 1995 FIPS PUB 180-1. However, SHA1 is relatable to MD5 as it is based on MD5.

The SHA 1 can take any arbitrary message as an input which is 264 bits in length and produce 160-bit long message digest. SHA stands for **Secure Hash Algorithm** where secure signifies the one-way (pre-image resistance or collision resistance) property and inability to produce a similar message from two messages. Here, one-way means that the one can not obtain the original message with the help of the message digest of that message.

#### SHA1 Working

As we know that SHA1 is modelled after MD5 and most of its steps resemble MD5. So, here we will explain the SHA1 steps briefly.

- **Padding** – This step adds the padding to the end of the genuine message in such a manner that the length of the message is 64 bits less of the multiple of 512.
- **Append** – The length of the message is computed devoiding the padding bits and appended to the end of the padding as a 64-bit block.
- **Division of input** – Divide the input into 512-bit blocks
- **Initialize chaining variable** – Here 5 chaining variables are initialized A, B, C, D, and E each of 32-bits total of 160-bits.
- **Process blocks** – This step includes copying of chaining variables, division of 512 bit-block into 16 sub-blocks, and processing 4 rounds of 20 step each.

However, in SHA1 there are four rounds and each round consist of 20 steps where each round takes the current 512-bit block, the register abcde and constant K[t] (where t=0 to 79) as the three inputs.![single SHA-1 operation](https://techdifferences.com/wp-content/uploads/2019/04/SHA1.jpg)It makes 80 iterations, which can be mathematically expressed by:

**abcde = (e + Process P + s5(a) +W [t] +K [t]), a, s30 (b),c,d**

Where,
abcde = The register constructed of five variables a, b, c, d and e
Process P = The logical operation,
St = Circular-left shift of the 32-bit sub-block by t bits.
W[t] = A 32-bit derived from the current 32-bit sub-block
K[t] = On of the five additive constants



## Key Differences Between MD5 and SHA1

1. MD5 can create 128 bits long message digest while SHA1 generates 160 bits long message digest.
2. To discern the original message the attacker would need 2128 operations while using the MD5 algorithm. On the other hand, in SHA1 it will be 2160 which makes it quite difficult to find.
3. If the attacker wants to find the two messages having the same message digest, he would require 264 operations for MD5 whereas 280 for SHA1.
4. When it comes to security by the above-given fact SHA1 hold more points relative to MD5.
5. MD5 is faster than SHA1, but SHA1 is more complex as compared to MD5.



### Conclusion

The crucial difference between MD5 and SHA1 is that MD5 was priorly developed and had several vulnerabilities where one can create the collisions for message digest. On the other hand, SHA1 brought a lot of improvement in hashing and is better than MD5. Although, there are still some issues in SHA1 which got resolved in SHA 256 and SHA 512.