## Taks 3, Using OpenSSL from the command line interface

Now, our goal is to create a text file that can be encrypted in three different types. For this, I have to create a directory for this project and access this directory. So, I used my terminal and went to my desire folder, and type

```
mkdir Project 2 – openssl
cd Project 2 - openssl
```

Then for the text file created, we used nano editor and named as “textFile”

```
nano textFile
```

Here we typed our text, “Hi everyone, This is a group project about OpenSSL. Our goal is to encrypt and decrypt using OpenSSL.”
Then press ctrl + X, then y, and press enter to save this text. 

![Screen Shot 2022-08-06 at 10 02 38 AM](https://user-images.githubusercontent.com/93491482/185805226-b9339fce-4dc6-4dfa-b56e-14f0804ba90b.png)

## a.	Create a text file with some input and encrypt it using
## I.	AES-128 CBC
## II.	AES-256 CTR
## III.	DES


Now, we are going to encrypt with three following different types of encryption methods.

i.	AES-128 CBC

ii.	AES-256 CTR

iii.	DES

To do this encryption, In the terminal, we used three commands that are similar to each other and just changed the encryption method. 

i.	For AES-128 CBC
```
openssl enc -aes-128-cbc -base64 -in textFile -out text_aes_128_cbc
```
Here are “textFile” will encrypted and it will save as “text_aes_128_cbc”. Besides terminal is required for the password for extra security. For the password we used “password.” We also need to verify our password with our given password.

![Screen Shot 2022-08-06 at 10 11 21 AM](https://user-images.githubusercontent.com/93491482/185804520-980a2330-5e20-450b-8052-192ecfbda5e4.png)

ii.	For AES-256 CTR
```
openssl enc -aes-256-ctr -base64 -in textFile -out text_aes_128_cbc
```
Here “textFile” will be encrypted and it will save as “text_aes_256_ctr” and password we used “password”. We also need to verify our password with our given password.

![Screen Shot 2022-08-06 at 10 14 39 AM](https://user-images.githubusercontent.com/93491482/185804560-422d75d7-f36b-436a-9a23-da3882299358.png)


iii.	For DES
```
openssl enc -des -base64 -in textFile -out text_des
```
Here “textFile” will be encrypted and it will save as “text_des” and password we used “password”. We also need to verify our password with our given password.


![Screen Shot 2022-08-06 at 10 18 15 AM](https://user-images.githubusercontent.com/93491482/185804587-ee74c9bb-3093-4532-8a3c-2d9ac3492206.png)

Now, we need to verify we will have 3 different encrypted file and one original files. 
```
ls
```

![Screen Shot 2022-08-06 at 10 27 02 AM](https://user-images.githubusercontent.com/93491482/185804608-11e47906-3317-49b3-9c97-181739ee1a81.png)


## b. Create a 2048 bit RSA public and private key.
After we verify there are three different encrypted files, and one original file are this directory now we will create a 2048-bit RSA public and private key. For this, we need to create directories for two people where they will have each other’s public key and they can share encrypted text file. After getting an encrypted file they will decrypt it with their own private key. Assume one is Alice and the other one is Bob. They both need private and public keys for RSA encryption. We can follow the following commands:

For Alice,
```
mkdir A
ls
cd A
openssl genrsa -out keypairA.pem 2048
```
For Bob
```
mkdir B
ls
cd B
openssl genrsa -out keypairB.pem 2048
```
Here, for the last command genrsa is generate an RSA key and it will save as “keypairA” and “keypairB”, which will contain private and public keys. 

![Screen Shot 2022-08-06 at 10 38 03 AM](https://user-images.githubusercontent.com/93491482/185804646-ea0feea8-f1fb-4423-baa9-214d8d76dd20.png)

Now, we are going to separate our public key from keypairA and keypairB.

For Alice,
```
openssl rsa -in keypairA.pem -pubout -out publicA.pem
cat publicA.pem
```
**Notice that after OpenSSL we do not generate the key, we are using this RSA key to separate the file. So we used RSA after OpenSSL.
For Bob,
```
openssl rsa -in keypairB.pem -pubout -out publicB.pem
cat publicB.pem
```

![Screen Shot 2022-08-06 at 10 40 43 AM](https://user-images.githubusercontent.com/93491482/185804682-8dbc8a85-9b2f-4c8f-a9a1-e19db876b080.png)

## Task4, Exchange of encrypted data. 
### a. Encrypt a file (e.g., a text file) with an algorithm and key length of your choice.

To exchange data, we will need to create a link between Alice and Bob to access each other’s public keys by

For Alice,
```
ln -s /Users/muhfatalam/Downloads/Summer\ 2022/CSCI\ 360/Project\ 2\ -\ openssl/B/publicB.pem
ls
```

For Bob,
```
ln -s /Users/muhfatalam/Downloads/Summer\ 2022/CSCI\ 360/Project\ 2\ -\ openssl/A/publicA.pem
ls
```

![Screen Shot 2022-08-06 at 10 56 32 AM](https://user-images.githubusercontent.com/93491482/185804709-9409f1ed-c1cb-41fe-87f1-a8af9314b0d2.png)

Now for demonstration purposes, we will make an encrypted text file with bob’s public key in the Alice directory. In the text file, we typed “Hi, My account number is 1234567890. And we all going to John Jay” and saved it as “textFile”
```
nano textFile
cat textFile
```
Now we are going to encrypt our “textFile” with Bob’s public key “public.pem” and save it as “encryptedFile”.

```
openssl rsautl -encrypt -in textFile -out encrptedFile -inkey publicB.pem -pubin
ls
cat encryptedFile
```

![Screen Shot 2022-08-06 at 11 03 20 AM](https://user-images.githubusercontent.com/93491482/185804754-14edd810-875e-484b-af72-8e6021344a03.png)

### b.	Exchange the file and the necessary credentials for decryption (i.e., algorithm, key) with your neighbor.

Now, Bob, it is going to copy this encrypted file from Alice and save his directory with the name “received.”

```
cp /Users/muhfatalam/Downloads/Summer\ 2022/CSCI\ 360/Proiect\ 2\ -\openssl/A/encrypte
dFile received
```


### c.	Decrypt the secret of your neighbor.

For decryption, Bob will use his own private key and save it as “decryptedFile.”

```
openssl rsautl -decrypt -in received -out decryptedFile -inkey keypairB.pem
ls
cat decryptedFile
```

![Screen Shot 2022-08-06 at 11 14 49 AM](https://user-images.githubusercontent.com/93491482/185804793-1140d1a1-c725-4dc5-a64b-fb143e5c2451.png)

For security purposes, before Alice encrypts the file with Bob’s public key, we can separate our private key with any kind of encryption methods like AES or DES from “keypairA.pem” to save it with “privateA.pem.” Here we are using “AES-256-CBC”. For passwords, we are using “password.”

```
openssl rsa -in keypairA.pem -aes-256-cbc -out privateA.pem
```

After verifying the password, we will sign our “textFile”, and save it as a “signed” name using the inkey “privateA.pem.”

```
openssl rsautl -sign -in textFile -out signed -inkey privateA.pem
```
From now every time, we used our private key we need to use password. 

![Screen Shot 2022-08-06 at 11 35 51 AM](https://user-images.githubusercontent.com/93491482/185805007-d70d40db-a14e-4356-9381-de42ae261078.png)

Now Bob needs to copy this file and saved it as a “singed” name.

```
cp /Users/muhfatalam/Downloads/Summer\ 2022/CSCI\ 360/Project\ 2\ -\ openssl/A/signed signed
ls
```
After copying the file Bob needs to verify this signed file and save it as “signedFile” using the public key of Alice “publicA.pem.”

```
openssl rsautl -verify -in signed -out signedFile -inkey publicA.pem -pubin
```

Now we can open this file:

```
ls
cat signedFile
```

![Screen Shot 2022-08-06 at 11 38 50 AM](https://user-images.githubusercontent.com/93491482/185805025-ddef3c5e-25ad-4ea9-b2bf-03dc9e6fa438.png)

When we used any encryption method for RSA it will become more secure besides the public and private keys.

