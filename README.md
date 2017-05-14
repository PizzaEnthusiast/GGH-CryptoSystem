# GGH-CryptoSystem
An amazing cryptosystem based on lattices
A project developed by students:

* Timothy Indrieri
* Edward Morkunas

under the supervision of Michael Soltys

## Plan 
The plan for this project is to create an implementation of the GGH cryptosystem within the real integer field. 
As well as try to extend the GGH cryptosystem to a finite field for the floating problem.
The GGH cryptosystem uses idea of the closest vector problem for the encrytpion of a message.

## Motivation
Educational understanding of how a lattice cryptosystem operates. Computer Security is a large and ever growing field. This project allows us to explore some new options that may be taken in the future. Also as quantum computer come around a new methodology may need to be taken in order to secure the new age of computers. 

## Contributors 
The book referenced here showed us the foundation of the GGH encryption scheme.
<a href="http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.182.9999&rep=rep1&type=pdf">[The algorithm is shown here.]</a>
The following wikipedia page for the GGH cryptosystem helped us understand how to decrpyt messages in the system. This decryption method was followed closely in our implementation. 
<a href="https://en.wikipedia.org/wiki/GGH_encryption_scheme">[GGH wiki.]</a>
 
## How to Run the Code
 Input: Enter in a security parameter and the text file that you would like to encrypt.
 In order for the program to complete its run, the input must be as long as the security parameter.
 
 Output: The program will output several items:
*Private Key
*Public Key
*Encoded Text ( Base-64)
*Cypher Vector
*Decrypted Vector
*Your decrypted message

## The Method
In order for our system to be able to send messages to users, it must be able to generate different keys each time a message is sent. This would improve security greatly, and remove any threats of analysis. In order to generate a private basis, the system must abide by one guideline for the GGH cryptosystem. The private basis for the system must be very close to being or completely orthogonal. In the following code provided, there is a commented out section(lines 7-15) of what was an attempt to achieve a perfectly orthogonal basis. The code will generate a random nxn matrix using the Numpy module and compute its Hadamard ratio. If the ratio found is fairly large, then the lattice that was generated was a "good basis”. 

There had proven to be difficulties when taking this approach to producing our basis.  The smaller range we allowed the ratio to fit in, the longer the program would run. Using this method also allowed for future computation errors because the basis was not perfectly orthogonal, and led to floating point computation errors. As a result, the approach taken was to use a basis that is  known as a completely orthogonal basis, the identity matrix of the given dimension n.

In order to successfully calculate a public basis for our system,  we need to generate a transformation matrix that would result in a nearly parallel public key. Multiple attempts were created in order to produce such a matrix. The first attempt was to randomly generate a matrix, and to calculate its determinant. This method proved to hinder performance of the system because this was an expensive operation, and was limited to a special case of matrices. The code never seemed to produce such a matrix. An additional attempt at creating a unimodular matrix was to integrate the SAGE mathematics library into Python. SAGE has a library that will produce such a matrix, however it was found that SAGE was not portable to all systems and as a result another attempt was produced. 

Once the keys were generated, the system would be ready to encrypt messages. In our implementation, the encrypt command takes the file name and public basis in order to create a cipher text. It will read in the file and encode each letter into base 64. It will store these into a list and then create a list of the binary encodings for the base64 text. Once the text is in a binary format, the system will multiply each byte with a vector of the public basis. Additionally the code will generate a random 1xN matrix that may be added to the cypher to create noise. Encrypt will then send the cypher text back. 
The system must avoid floating point types when performing this operation because the cipher may be rounded or vulnerable to analysis. Floating points are also undesired because of future difficulties when trying to decrypt the message.

An issue that arose while implementing the encryption feature was keeping the encodings uniform. The system had to be sure that it was multiplying the formats correctly, and that it was not losing any data. This became clear when the system would multiply code that was in ASCII format. The cipher text would result in the same values. 

We used the inverse of our private basis which was the identity. Than after that we multiplied this by our cipher text. Finally we took that result from the multiplication and multiplied this by the inverse of the unimodular which gave us back the message. The process required us to try and round the numbers because of floating point numbers that appeared due to the inverse of the matrix. We realized that the higher the dimensions the less precision we had (found that dimension 11 and higher did not work). Another way to solve this would be by Babai’s algorithm that could be implemented.

## Acknowledgments
A huge thanks to Michael Soltys for his guidance and inspiration. A thank you also goes out to the Computer Science program at CSUCI that made this study possible. 
