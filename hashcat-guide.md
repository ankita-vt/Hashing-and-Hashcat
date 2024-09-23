Detailed Walkthrough of Hashcat

Welcome to our comprehensive guide on Hashcat, the sophisticated password recovery tool that employs advanced hashing algorithms.
In a world where data breaches and cyber threats are all too common, it’s vital for anyone in cybersecurity to understand how to secure and recover passwords.
This project is designed to instruct on the effective utilization of Hashcat, making it a vital resource for both security professionals and enthusiasts.
We’ve crafted this guide to be both comprehensive and engaging, covering the basics of cryptographic hashes and real-life applications. As you dive into this material, you’ll discover different hashing algorithms, the advantages of using GPUs for cracking, and various attack methods like dictionary and brute-force attacks.
Whether you’re just starting out and want to get a handle on password security or you’re a seasoned pro looking to sharpen your skills, this guide is packed with basics.
So, let’s jump into the fascinating world of Hashcat together and equip yourself with essential cybersecurity tools!

1.	Understanding the Role of Hashing and Hashcat

What is a Hash?
A hash is defined as a fixed-length string that is generated through the application of a cryptographic hash function to a given set of input data. This process results in a unique representation, often referred to as a digital fingerprint, of the original data. 

The primary purpose of hashing in our context:
Secure Password Storage: Hashes play a critical role in the secure storage of passwords. Rather than storing passwords in plaintext, systems often store the hash values, which adds a layer of security by making it difficult for attackers to recover the original passwords from the stored hashes.

Common Hash Types
There are several widely recognized cryptographic hash algorithms, each with distinct characteristics:
1.	MD4:
Description: An early hash function that is known for its speed in processing. However, it has significant security vulnerabilities and is not recommended for secure applications.
Example: 94e3cb0fa9aa7a5ee3db74b3712c8d78 (Hello world!)


2.	MD5:
Description: A widely used hashing algorithm that produces a 128-bit hash value. While it is fast and suitable for non-cryptographic purposes, MD5 is vulnerable to collision attacks, where two different inputs produce the same hash output.
Example: fc3ff98e8c6a0d3087d515c0473f8677 (Hello, world!)
3.	SHA-1:
Description: This hash function generates a 160-bit hash value and is considered more secure than MD5. However, SHA-1 has also been found to have vulnerabilities, making it less suitable for high-security applications.
Example: 2ef7bde608ce5404e97d5f042f95f89f1c232871 (Hello, world!)
4.	SHA-256:
Description: Part of the SHA-2 family of hashing algorithms, SHA-256 produces a 256-bit hash value. It is widely regarded as secure and is recommended for cryptographic applications due to its resistance to collision and pre-image attacks.
Example: a591a6d40bf420404a011733cfb7b190d62c65bf0bcda32b53ed49f456f7f229 (Hello, world!)

 
2. Why Hashcat?
Two of the most widely used password recovery tools in the field are John the Ripper and Hashcat. While there are several other options available, such as Hydra, they tend to be more niche in their applications.
In this project, we will focus on Hashcat due to its exceptional GPU support and compatibility with a wide variety of hash types.
 
3. Why Use GPU?
Speed Advantage
•	The utilization of Graphics Processing Units (GPUs) in Hashcat provides a substantial speed advantage over traditional Central Processing Units (CPUs). GPUs are specifically designed for parallel processing, allowing them to execute multiple calculations simultaneously. This architectural feature enables Hashcat to conduct password cracking operations at a significantly accelerated pace.
•	In practical terms, this means that tasks which may take hours or days on a CPU can often be completed in a fraction of the time when leveraging the parallel processing capabilities of a GPU. This enhanced speed is particularly beneficial when attempting to crack large datasets or complex password hashes, making GPUs an invaluable asset in password recovery efforts.
Automatic Detection
•	Hashcat is equipped with a built-in mechanism for automatic detection of installed GPUs, specifically those from NVIDIA and AMD. 
•	Upon launching Hashcat, the tool assesses the system’s hardware and automatically allocates resources for optimal performance. This capability not only simplifies the setup process for users but also ensures that the available computational power is utilized effectively, maximizing the efficiency of the password cracking process.
 
4. Why Use GPU?
•	Hashcat commands are constructed to require a minimum of four key arguments. These arguments are essential for the tool to execute password recovery processes effectively and include the following components:
 

Arguments: 
1. Hash Mode (-m): Specifies the hashing algorithm being targeted (e.g., MD5, SHA-256). This is critical as it directs Hashcat on how to process the input hashes.
“Tip: If you don’t know what’s the hash type/ mode number you could use “https://hashes.com/en/tools/hash_identifier” to identify the hash type. Then in Linux could use grep hashtype to find the mode number. 
Ex. Hashcat –help | grep sha256”
Common hash modes: 
	MD5: Mode 0
	SHA-1: Mode 100
	SHA-256: Mode 1400

1.	Attack Mode (-a): Defines the strategy that Hashcat will employ to attempt to crack the passwords (e.g., dictionary attack, brute-force attack). This influences the efficiency and effectiveness of the password recovery process.

a.	Straight / Dictionary Attack: (-a 0) Use Dictionary file (or Wordlist)
b.	Combination Attack: (-a 1) Append Dictionary files
c.	Mask / Brute-Force Attack: (-a 3)

3.  Hash File (Relative Path): The input txt file containing one or multiple hashes to be cracked, with each hash listed on a separate line. 
4. Wordlist (Relative Path) or Mask: Depending on the attack mode selected, this argument represents either a list of potential passwords (in the case of dictionary attacks) or a defined character mask for brute-force attempts (More on this in the later section).
 
5. Setting Up Hashcat
Prepared to crack or recover some passwords?
For Windows: 
1. Download Hashcat
•	Go to the official Hashcat website and download the latest version of Hashcat for Windows.
•	The download will be a .7z (7-Zip) archive file. Extract the Archive using 7-Zip or WinRAR
2. Install CUDA or OpenCL Drivers (Optional)
If you plan to use Hashcat with a GPU, you need the appropriate drivers:
o	NVIDIA GPUs: Install CUDA drivers from the NVIDIA website.
o	AMD GPUs: Install OpenCL drivers from the AMD website.
4. Using Command Prompt navigate to the Hashcat Folder
  		cd C:\path\to\hashcat
5. Run the following command to verify the installation:  
               hashcat -I

For Linux: 
1. Install Dependencies
sudo apt install build-essential ocl-icd-libopencl1 git
2. Download Hashcat
sudo apt install Hashcat
3. Install OpenCL Drivers
For NVIDIA GPUs (CUDA):
Install the NVIDIA drivers and CUDA toolkit:
sudo apt install nvidia-driver-<version>
sudo apt install nvidia-cuda-toolkit
For AMD GPUs (ROCm):
Follow the instructions on the ROCm website to install OpenCL drivers
 
6. Attack Modes
a.	Dictionary Attack (-a 0 ):
Utilizing a precompiled list of potential passwords, known as wordlists (e.g., rockyou.txt), can significantly enhance the efficiency of password cracking. This approach offers several advantages, including faster processing times, particularly for common passwords. 
For example, the command hashcat -m 0 -a 0 hash.txt rockyou.txt demonstrates how to implement this method using Hashcat, where hash.txt contains the hashes to be cracked and rockyou.txt serves as the wordlist.
b.	Brute force/ Mask Attack (-a 3): 
While a brute-force attack tests all possible combinations, a mask attack narrows the search to specific patterns based on known insights about the password. If you have information about the password structure, you can optimize the cracking process using a mask attack. 
For instance, if you know the password starts with a capital letter, ends with a special character, and is 5 characters long, you could use the command: hashcat -m 1000 -a 3.
 
 
 
Where only the password length is known, brute force can be used as a last resort. 
For instance, the command hashcat -m 0 -a 3 hash.txt ?a?a?a?a?a would be applied if the password length is 5 characters.
c.	Combinator attack (-a 1): 
The attack merges the words from each file in a specified format, such as appending one word to another. For example, if you have a list of first names and a list of last names, a combinator attack could generate combinations like "JohnDoe" or "JaneSmith." This approach is useful for cracking passwords that may be formed by concatenating familiar words or phrases.
 
 
7. Custom Rules (Advanced)
Rules modify dictionary words to generate variations, thereby increasing the likelihood of finding a match. Key capabilities include:
1.	Case Modification: Change to lowercase or uppercase (e.g., password becomes PASSWORD).
2.	Appending/Prepending: Add numbers or characters to the end or beginning (e.g., password becomes password1).
3.	Inversion: Change the order of characters.
4.	Reversal: Reverse the entire string (e.g., password becomes drowssap).
5.	Duplication: Repeat characters or the entire password.
6.	Character Insertion: Insert characters at specific positions.
7.	Condition-Based Rejection: Exclude passwords based on specific criteria.
8.	And More: Additional rules for further customization.

How to Apply:
Use the -r option followed by the path to your rules file, typically found in a subfolder named "rules" within the Hashcat directory.
Example Command: hashcat -m 0 -a 0 -r rules/best64.rule hash.txt rockyou.txt

Additional rules that can be utilized include:
•	best64.rule 
•	rockyou-30000.rule
•	T0XIC.rule

 
8. Practical Scenarios
•	Scenario 1: 
o	Hash Type: SHA256
o	Command: hashcat -m 1400 -a 3 hash.txt ?l?l?l?l?l?d?d
  
•	Scenario 2: 
o	Hash Type: NTLM
o	Command: hashcat -m 1000 -a 3 hash.txt ?u?l?l?d?d?s
   

•	Scenario 3: 
o	Hash Type: MD5
o	Command: hashcat -m 0 -a 3 hash.txt ?l?l?l?l?d?d?d
   
•	Scenario 4: 
o	Hash Type: Linux (SHA 512)
o	Command: hashcat -m 1800 -a 3 hash.txt ?l?l?l?l

•	Challenge Mode:
Experiment with more complex hashes for hands-on practice.

10. Drawbacks of Hashcat
•	Resource Intensity: Hashcat needs a lot of computing power, especially when working with large datasets. This can be a problem for devices with limited hardware. For example, cracking complex passwords with many characters requires strong GPUs and lots of memory, making it tough to use on lower-end machines. 
•	Complex Setup: Setting up the necessary drivers and ensuring compatibility with various operating systems can be a hurdle for beginners, making it difficult for them to get started.

•	Learning Curve: The command-line interface of Hashcat can be daunting for users accustomed to graphical user interfaces (GUIs). For example, users may struggle to remember specific command syntax or option.

•	Limited Compatibility: Hashcat does not support all hash types, particularly some of the newer or proprietary algorithms. For instance, while it supports widely used hashes like MD5 and SHA-256, it may not be able to crack certain advanced hashes (example bcrypt or Argon2).

11. Discussion Questions
•	Usability: How does the lack of a GUI affect the usability of Hashcat for beginners?
•	Hashing Algorithms: What are the implications of using Yescrypt or other memory-hard hashing algorithms in Hashcat?
•	Ethical Considerations: What do you think are the ethical implications of using Hashcat for password recovery compared to its potential for malicious use?

12. References
•	Hashcat direct download (Version 6.2.6) [Software] https://hashcat.net/files/hashcat-6.2.6.7z
•	Hashcat [Software]. GitHub https://github.com/hashcat/hashcat
•	Hashcat [Wiki] https://hashcat.net/wiki/
•	Cryptographic hash function [Wiki]  https://en.wikipedia.org/wiki/Cryptographic_hash_function
•	NVIDIA CUDA Toolkit [Software] https://developer.nvidia.com/cuda-toolkit
•	AMD OpenCL SDK [Software].  https://github.com/GPUOpen-LibrariesAndSDKs/OCL-SDK/releases
•	NotSoSecure Password cracking rules [Software].  https://github.com/NotSoSecure/password_cracking_rules
•	S3Curiosity. (2020, January 15). The power of Hashcat.   https://medium.com/@S3Curiosity/power-of-hashcat-5c0b3ccd9898

