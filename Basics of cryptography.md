# Digital Signatures

From my understanding, digital signatures are not so much signature than a "proof of integrity" of the associated content on which the signature was put.
# Digital Certificates

From my understanding, Digital Certificates look more like a signature as they prove that the person (or the domain) is who he claims he is because he previously declared himself to a CA which signed his PIDs.

**Problematic**:

- **Alice** sends her public key to **Bob**
- **Alice** encrypts a msg with her private key and sends it to **Bob**
- **Bob** is able to decrypt the msg thanks to the public key
=> but how can we be sure there wasnt a Man in the Middle attack ?? How **Bob** be sure
- **Man in the Middle attack**:
	- Say **Tom** the bad guy is between **Alice** and **Bob** ("man in the middle"):
	- **Tom** intercepts **Alice**'s msg and has her public key
	- he decrypts **Alice**'s msg with her public key, re-encrypts a new msg by modifying the original msg and sends the newly re-encrypted msg along with his public key (and not **Alice**'s) to **Bob**
	- **Bob** is then "cheated" in thinking he is receiving encrypted msgs from **Alice** when in reality it was malicious **Tom**
**The fundamental problem is**: how can we be sure the public key decrypting the msg is **Alice**'s?

Enter ✨**Digital Certificates**✨ 
- **Alice** will send **Bob** her digital certificate 
- that digital certificate
	- associates **Alice**' public key and her identity 
	- it was issued by a CA (Certificate Authority)
- that digital certificate has 2 properties:
	- its proves **Alice** is who she claims to be
	- it is trusted because everybody trusts the CA who issued the certificate (think: like a government agency ; your web navigator has a list a trusted CAs, organizations add their private CA)

How to create a **digital certificate*:
- give a lot of (verifiable) PID data (Personal Identifier Data)
- give your public key
What happens when the CA creates the **digital certificate**
- they hash all the PID data using a hashing algorithm
- they encrypt this hash using the CA's 🗝️ private key => the CA adds a ✨**digital signature**✨ of the CA! this way he locks 🔒 the content of Alice's PIDs 
=> so in the end:
1. the end-receiver **Bob** receives the certificate with the key
2. Assuming he trusts the CA (in other words the CA was installed in the machine's certificates and so he trusts the CA's public key)
3. **Bob** uses the CA's public key to check the CA's signature on the certificate
4. he then uses Alice's public key to exchange with her in an encrypted way

How does the user validates the **digital certificate**?
- he uses the same hash algorithm to hash the PID data in the certificate
- he uses the CA's public to decrypt the CA's signature on the certificate
	- if he can decrypt the signature, he know's the certificate is indeed the CA's
	- if he can't, the certificate is not trusted
- he then
	- calculates the hash of the certificates PID's
	- and compares it with the decrypted certificates signature => if both hashes are the same: OK!

Learning resources:
- https://www.youtube.com/watch?v=c-O-uMxTaEw&list=PL7d8iOq_0_CWAfs_z4oQnCuVc6yr7W5Fp&index=3
