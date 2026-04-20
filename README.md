This challenge asks us to solve a cryptographic problem. Once we start the machine, we use the IP address and port given to us, and we establish the connection by involving "netcat".

![ImageAlt](https://github.com/ZeynepBasaran09/ctf-writeups/blob/c844c59394b3e6aa128f2bca570ead9cda204aef/Ekran%20g%C3%B6r%C3%BCnt%C3%BCs%C3%BC%202026-04-20%20180902.png)

After we connect, we can access our first flag but it's encoded in XOR format. We are returning to the file we downloaded in the beginning of the challenge.

![ImageAlt](https://github.com/ZeynepBasaran09/ctf-writeups/blob/55e9996f6462009420bf33c80002bbd9f6537518/Ekran%20g%C3%B6r%C3%BCnt%C3%BCs%C3%BC%202026-04-20%20181609.png)

In the source code, we have 2 great hint for the decryption. First one is that we can see the plain text is encoded with the key. And the second one is, it says that key is randomly generated but length of the key is fixed which is 5. After finding this, we can create the descryption script:


    def find_xor_key_and_decode(encoded_text, known_start, known_end, key_length=5):

    encoded_bytes = bytes.fromhex(encoded_text)
    
    known_start_bytes = known_start.encode()
    
    known_end_byte = known_end.encode()
    
    key_start = bytes([encoded_bytes[i] ^ known_start_bytes[i] for i in range(len(known_start_bytes))])
    
    key_end = encoded_bytes[-1] ^ known_end_byte[0]
    
    key = key_start + bytes([key_end])
    
    key = key[:key_length]
    
    decoded_message = bytes([encoded_bytes[i] ^ key[i % key_length] for i in range(len(encoded_bytes))]).decode('latin1')
    
    return key, decoded_message
    

When we use this code and run it, we can access our first flag: THM{p1alntExtAtt4ckcAnr3alLyhUrty0urxOr}

After submitting our first flag, key, first 4 known letters and the last known letter, the challenge gives us the second flag:THM{BrUt3_ForC1nG_XOR_cAn_B3_FuN_nO?}

So this was how to finish the challenge named "W1seGuy". Keys and encoded flag might be different in every try, so i didn't put mine's in here just in case.
