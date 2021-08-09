# Crypto: Xoro
![date](https://img.shields.io/badge/date-07.08.2021-brightgreen.svg)
![category](https://img.shields.io/badge/category-crypto-lightgrey.svg)
![score](https://img.shields.io/badge/score-388/500-blue.svg)
<!-- Score of the question / Max Score possible in any question -->

## Description

Xoro was a Crypto challenge where a netcat command was given to be connected. The following python code was running on it, when attempted to connect.
```py
#!/usr/bin/env python3

import  os

FLAG = open('flag.txt','rb').read()
def  xor(a, b):
	return  bytes([i^j  for  i,j  in  zip(a,b)])

def  pad(text, size):
	return  text*(size//len(text)) + text[:size%len(text)]

def  encrypt(data, key):
	keystream = pad(key, len(data))
	encrypted = xor(keystream, data)
	return  encrypted.hex()

if  __name__ == "__main__":
	print("\n===== WELCOME TO OUR ENCRYPTION SERVICE =====\n")
	try:
		key = os.urandom(32)
		pt = input('[plaintext (hex)]> ').strip()
		ct = encrypt(bytes.fromhex(pt) + FLAG, key)
		print("[ciphertext (hex)]>", ct)
		print("See ya ;)")
	except  Exception  as  e:
		print(":( Oops!", e)
		print("Terminating Session!")
```
## Summary
The user input+flag was XORed with a randomly generated key and the hex was provided. When the output was XORed with the input, we got the key, and through that, the flag.
## Detailed solution
First, without having a proper understanding of what the code does, we tried to make a decrypt function from the encrypt function. But it was a random key generated, so we thought we had to try all the key combinations and guessed one would make it.
Trying bruteforce, we gave a loop for a flag length from 9 to 80 and kept on trying with generating a random function, but it was pretty clear we're not gonna make it. We didn't know the length of the flag, but it was clear that when no input was given, a 78 character hex was returned. Therefore the length of the flag was 39. So we made the following decode function, to figure out if any output was there that made sense.
```py
def  decrypt(data):
	byte = bytes.fromhex(data)
	len_flag = 39
	for  i  in  range(10000000):
		rand = os.urandom(32)
		keystream = pad(rand, len_flag)
		encrypted = xor(keystream, byte)
		try:
			print(encrypted.decode("utf-8"))
		except  UnicodeDecodeError:
			pass
```
But then, no use was found. So we tried to understand the code more(duh). 
* The randomly generated key is always of the size 32. 
* The `pad()` function basically finds the length of the flag, and repeates the randomly generated key(of size 32) till the length, so that both the flag and the key have equal length.
* The `encrypt()` function just xors the padded key with the flag and returns the hex of it from the bytes version.

But the key point we had to realize was that, the input taken is also added to the flag for encryption. So if we had given a hex that we made of 32 length, then we would have the key XORed with our own hex. So if we slice the first 32 characters of the encrypted hex, and XORed with our known hex, we would get the key back!
And if we got the key, then we can create the padded key, and XOR it with our encrypted string to get the Flag!

A small gist of what i went through: 
![Screenshot](https://media.discordapp.net/attachments/856580866175139890/873627481360441384/Screenshot_2021-08-07_at_11.33.28_PM.png)

## Flag
```
BSNoida{how_can_you_break_THE_XOR_?!?!}
```
