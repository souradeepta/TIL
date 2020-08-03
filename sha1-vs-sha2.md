# SHA1 vs SHA256

[ref](https://www.keycdn.com/support/sha1-vs-sha256)



![SHA1 vs SHA256](https://www.keycdn.com/img/support/sha1-vs-sha256.png)



## What is SHA?[#](https://www.keycdn.com/support/sha1-vs-sha256#what-is-sha)

SHA, which stands for secure hash algorithm, is a cryptographic hashing  algorithm used to determine the integrity of a particular piece of data. Variations of this algorithm are often used by SSL certificate  authorities to sign certificates. This algorithm help ensures that your  website's data is not modified or tampered with. It does so by  generating **unique hash values** from any particular file / variation of a file. Based on these hash values, it can be determined  whether or not the file has been altered by comparing the **expected hash value to the hash value received**.

As computers become more powerful, the SHA hash sizes are increasing to  help better improve security and make it harder for attackers to decrypt hashes. The secure hash algorithm originally started out as SHA0 (a  160-bit hash published in 1993). As of when this article was published,  there is currently a much more powerful SHA known as SHA3 (a 1600-bit  hash).



## SHA1 vs SHA256[#](https://www.keycdn.com/support/sha1-vs-sha256#sha1-vs-sha256)

This article will focus mainly on the differences that exist between SHA1 vs SHA256. SHA2 is the successor of SHA1 and is commonly used by many SSL  certificate authorities. There are currently six different SHA2 variants including:

- SHA-224
- SHA-256
- SHA-384
- SHA-512
- SHA-512/224
- SHA-512/256

These variations differ in terms of output size, internal state size, block  size, message size, and rounds. To compare the differences that exist  between the SHA1 vs SHA256 algorithms, consider the following SHA  comparison information from [Wikipedia](https://en.wikipedia.org/wiki/Secure_Hash_Algorithm).

| Algorithm | Output size (bits) | Internal state size (bits) | Block size (bits) | Max message size (bits) | Rounds |
| --------- | ------------------ | -------------------------- | ----------------- | ----------------------- | ------ |
| SHA1      | 160 (5 × 32)       | 160                        | 512               | 264 − 1                 | 80     |
| SHA256    | 256 (8 × 32)       | 256                        | 512               | 264 − 1                 | 64     |

Due to SHA1's smaller bit size, it has become **more susceptible to attacks** which therefore led to its deprecation from SSL certificate issuers in  January 2016. An example of the difference in size between SHA1 vs  SHA256 can be seen in the following example hashes:

- **SHA1** - `da39a3ee5e6b4b0d3255bfef95601890afd80709`
- **SHA256** - `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855`

With our online hash generator tool, you can quickly generate an [SHA256 hash](https://tools.keycdn.com/sha256-online-generator) for any string or input value. Simply enter a string value into the input box and select Generate. The tool will then **generate a unique 64-digit hash** for the value you specified.

![keycdn sha256 generator tool]()



## Hash collision attacks[#](https://www.keycdn.com/support/sha1-vs-sha256#hash-collision-attacks)

A hash attack, known as a collision attack, can used to compromise the  security that hash algorithms intend to put in place. These attacks  occur when **two different files produce the same hash**. In this case, it is possible to substitute one file for another, therefore potentially resulting in a major security breach.

The SHA0 algorithm was unable to protect against these types of attacks  which is why it is no longer used. Additionally, SHA1 has also been  deemed quite vulnerable to collision attacks which is why all browsers  will be removing support for certificates signed with SHA1 by January  2017. SHA256 however, is currently much more resistant to collision  attacks as it is able to generate a longer hash which is harder to  break.



## SHA1 check tools[#](https://www.keycdn.com/support/sha1-vs-sha256#sha1-check-tools)

As SHA1 has been deprecated due to its security vulnerabilities, it is important to ensure you are **no longer using an SSL certificate which is signed using SHA1**. All major SSL certificate issuers now use SHA256 which is more secure  and trustworthy. The following tools can be used to check if your domain is still using SHA1.

- ssllabs

   \- SSLLabs is a great tool that displays a variety of information  regarding your domain's SSL certificate. You can use this tool to verify the signature algorithm used, which is located near the top of the  report. It should look similar to the following.

  ![ssllabs signature algorithm]()

- shaaaaaaaaaaaaa

   \- Alternatively, if you want to simply check your domain solely for  certificates using the weak SHA1 signature you can use this tool. It  will let you know which algorithm is currently in use on the domain you  have specified.

  ![sha1 checker tool]()

Ensuring that your website is not using an outdated signature algorithm is essential for maintaining the **proper security measures for your website**. If by chance you run one of the SHA checks above and see that the SHA1  algorithm is being used, you may want to strongly consider purchasing a  new SSL certificate that uses SHA256.





