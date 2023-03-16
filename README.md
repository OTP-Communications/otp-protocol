# otp-protocol version 0.0.1

Describe the api, format, &amp; syntax of the protocol used in our otp encrypted communications

## Alice and Bob

Alice can be represented and addressed by a random UUID consisting of an 8 digit long hexadecimal number. This corresponds to a 64 bit integer. Same goes for Bob. This space should be large enough to make the possibility of collisions acceptable. 


## Storing One-Time-Pads
One time pads are saved in a file with a `.otp` suffix. This should be a UTF-8 encoded file. A `.otp` file may consist of any number of lines. 
Each line is composed of has 128 characters of hexadecimal as shown below(except that the hexadecimal characters will be randomized in actual usage). Lines should be CRLF terminated. 

Any line not consisting solely of 128 hexadecimal characters will be considered metadata, and can safely be ignored in version 0.0.1. 
This is specified in hopes of making some level of backwards/forwards compatibility possible. 

<img width="1188" alt="image" src="https://user-images.githubusercontent.com/24924799/225484177-ea5c9d98-d92d-4a81-810c-5e03ab639c9d.png">

It is suggested that the file be named following a convention similar to `{UUID for Alice}_{UUID for Bob}.v{protocol version}.otp` though this may not prove neccessary. 

For convenience, the complementary file `{UUID for Bob}_{UUID for Alice}.v{protocol version}.otp` may be created with the lines order reversed. 

## Reuse of One-Time-Pads considered harmful
Using the same section of a otp to encrypt and send different messages is in direct violation of the assumptions that make a otp unbreakable and future safe. 
To send distinct messages encrypted using the same(or overlapping) portion of the pad is considered subversive behavior. Clients should distrust any other client that does so. 

To avoid having to coordinate between Alice and Bob, Alice will send messages encrypted from the start of the pad, and Bob will send messages encrypted from the end of the pad. They may continue sending messages until meeting in the middle. Using a duplicate file with the line order reversed for decryption may be helpful. 
