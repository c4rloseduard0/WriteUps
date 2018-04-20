# 5º Hacking N Roll - Script Rules

Challenge can be found in: https://shellterlabs.com/pt/questions/5-hacking-n-roll/script-rules/

In the statement of the challenge says: "God bless this mess. Resolve this mess, please!" and give us many flies with strange names, I analyzed the names, and saw that they all had 40 characteres, similar to sha1. After some tests I conclude that really was sha1 and when decrypted, they formed a sequence of numbers from 0 to 663, so I was a python code for renames the files, and get the content of theses files. The content was in hex so I included in script a function for decrypt.

KABUM, there it was the flag in middle of text, now just score:

Here is the script:
```python
#!/usr/bin/python
import os
import hashlib

enc_flag = ''

#Rename all files ----------------------------------
listaArquivos = os.listdir()
for arq in listaArquivos:
    for i in range(len(listaArquivos)):
        h = hashlib.sha1(bytes(str(i), 'utf-8')).hexdigest()
        if h == arq:
            os.rename(arq, str(i))
# --------------------------------------------------

#Get flag encrypted in hex -------------------------
for i in range(len(os.listdir()) - 1):
    arq = open(str(i), 'r').read()
    enc_flag = enc_flag + arq.strip()
# --------------------------------------------------

#Decode and show flag-------------------------------
flag = bytearray.fromhex(enc_flag).decode()
print(flag)
# --------------------------------------------------
```

See more Write-ups in: https://www.youtube.com/watch?v=WCw8RETGDLY&list=PLlq-hlhs91wqtRtIgQRxmqvgqTDZn6Qhu


