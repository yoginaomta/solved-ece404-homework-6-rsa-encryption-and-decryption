Download Link: https://assignmentchef.com/product/solved-ece404-homework-6-rsa-encryption-and-decryption
<br>
The goal of this homework is to give you a deeper understanding of RSA encryption and decryption, its underlying principles and standard representation.

Before starting this assignment, make sure that you understand the relationship between the modulus and the block size for RSA cipher and how RSA is made practically possible by the fact that modular exponentiation possesses a fast implementation. Also, before starting to write your own code for RSA, play with the script PrimeGenerator.py that is discussed in Lecture 12. You can download the script from the lecture notes web site.

<h2>Part 1: RSA Encryption and Decryption</h2>

Write a Python/Perl script to implement a 256-bit RSA algorithm for encryption and decryption. You can use the text in the Homework 6 section of the website. Your data block from the text will be of 128-bits. For the reasons explained in section 4 of lecture 12 (12.4), prepend it with 128 zeroes on the left to make it a 256-bit block. For this assignment, if the overall plaintext length is not a multiple of 128 bits, append an appropriate number of zero bytes to it so that it is (then pad again to make it a 256-bit block as mentioned previously).

Regarding key generation, note the following:

<ol>

 <li>The priority in RSA is to select a particular value of e and choose p and q accordingly. For this assignment, use e = 65537.</li>

 <li>Use the PrimeGenerator.py script mentioned above (you can import it to your script) to generate values of p and q. Both p and q must satisfy the following conditions:

  <ul>

   <li>The two left-most bits of both p and q must be set.</li>

   <li>p and q should not be equal.</li>

   <li>(<em>p </em>− 1) and (<em>q </em>− 1) should be co-prime to e. Hence, <em>gcd</em>((<em>p </em>− 1)<em>,e</em>) and <em>gcd</em>((<em>q </em>− 1)<em>,e</em>) should be 1. Use Euclids algorithm to compute the gcd.</li>

  </ul></li>

</ol>

If any of the above condition is not satisfied, repeat step:2.

<ol start="3">

 <li>Compute d. You may use the <em>multiplicative inverse </em>function from Python’s BitVector class.</li>

 <li>To compute the modular exponentiation for decryption, use the Chinese Remainder Theorem (CRT). Implementation details are in section 12.5 of the lecture notes.</li>

 <li>Use the script in the lecture notes to compute general modular exponentiation. Note that calculation of <em>V<sub>p </sub></em>and <em>V<sub>q </sub></em>for the CRT during decryption as well as the encryption will make use of this script.</li>

 <li>After decryption, remove the padded 128 zeroes from each block to make the plaintext printable in ASCII form.</li>

</ol>

<h2>Program Requirements</h2>

Your script for should have the following command-line syntax:

python rsa.py -g p.txt q.txt python rsa.py -e message.txt p.txt q.txt encrypted.txt python rsa.py -d encrypted.txt p.txt q.txt decrypted.txt

An explanation of this syntax is as follows:

For key generation (indicated with -g) the generated values of <em>p </em>and <em>q </em>will be written to <strong>p.txt </strong>and <strong>q.txt</strong>, respectively. The .txt files should contain the number as an integer represented in ASCII. So, for example, if <em>p </em>= 7, the corresponding text file will display 7 when opened in a text editor (example files can be found in the Homework section of the course webpage).

For encryption (-e), it should read the input text from a file called <strong>message.txt </strong>(or whatever the name of the command-line argument after -e is) and use the <em>p </em>and <em>q </em>values found in the command-line arguments <strong>p.txt </strong>and <strong>q.txt </strong>for encryption. The encrypted output should be saved in <strong>hexstring </strong>format to a file with the name of the final argument, in this case a file called

<strong>encrypted.txt</strong>.

For decryption (indicated with the -d argument), the input hex file is specified with argument after -d, in this case <strong>encrypted.txt</strong>. As with encryption, use the <em>p </em>and <em>q </em>values found in the command-line arguments <strong>p.txt </strong>and <strong>q.txt </strong>to decrypt the ciphertext. The decrypted output should be saved to a file with the name specified by the last argument, in this case <strong>decrypted.txt</strong>.

<h2>Part 2: Breaking RSA Encryption for small values of <em>e</em></h2>

Section 12.3.2 in Lecture 12 describes a method for breaking RSA encryption for small values of <em>e</em>, like 3. In this scenario, a sender <em>A </em>sends the same message M to 3 different receivers using their respective public keys. All of the public keys have the same value of <em>e</em>, but different values of <em>n</em>. An attacker can intercept the three cipher texts and use the Chinese Remainder Theorem to calculate the value of <em>M</em><sup>3 </sup>mod <em>N</em>, where <em>N </em>is the product of the values of <em>n</em>. The attacker can then solve the cube-root to get the plaintext message <em>M</em>. Write a script that does the following:

<ol>

 <li>Generates three sets of public and private keys with <em>e </em>= 3</li>

 <li>Encrypts the given plaintext with each of the three public keys</li>

 <li>Takes the three encrypted files and the public keys, and outputs the decrypted file as cracked.txt. Because Python’s pow() function will not provide enough precision to solve the cube-root, we have provided code on the course website (in the homework section) that should have the necessary precision. You will need to install the numpy library to use this code. <strong>If you are using Python 3, switch the calls to long(…) to calls to int(…) in solve pRoot.py.</strong></li>

</ol>

<h2>Program Requirements</h2>

Your script should have the following call syntax:

python breakRSA.py -e message.txt enc1.txt enc2.txt enc3.txt n_1_2_3.txt #Steps 1 and 2 python breakRSA.py -c enc1.txt enc2.txt enc3.txt n_1_2_3.txt cracked.txt #Step 3

An explanation of this syntax is as follows:

For encryption (-e), the program should read in the plaintext file (in this case <strong>message.txt</strong>), generate the three different public and private keys, encrypts the plaintext with each of the three public keys (<em>n</em><sub>1</sub><em>,n</em><sub>2</sub><em>,n</em><sub>3</sub>), and write each ciphertext to <strong>enc1.txt</strong>, <strong>enc2.txt</strong>, and <strong>enc3.txt </strong>,respectively. Then the program should write each of the public keys (<em>n</em><sub>1</sub><em>,n</em><sub>2</sub><em>,n</em><sub>3</sub>) to <strong>n 1 2 3.txt</strong>, with each key separated with a newline character, (an example is given on the ECE404 Homework page).

For cracking the encryption (-c), the program should read each of the different encrypted files (in this case, from <strong>enc1.txt</strong>, <strong>enc2.txt</strong>, and <strong>enc3.txt</strong>) and the public keys (in this case, from <strong>n 1 2 3.txt</strong>), use this information to crack the encryption, and then write the recovered plaintext to a file (in this case, <strong>cracked.txt</strong>).