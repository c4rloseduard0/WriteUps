# Desafio EY
A Ernst & Young (EY) é uma empresa que presta serviços em diversas áreas e está entre as quatros maiores empresas de serviços profissionais do mundo. Recentemente ela abriu um processo seletivo para preencher vagas de trainee. A seleção era um CTF com 9 desafios de várias áreas como reverse, forense, web, programação, etc e aqui segue minha resolução dos desafios.

## Tools usadas
* Dirb
* Python
* Fcrackzip
* Ltrace
* Aircrack-ng
* Wireshark

## Lista de Desafios
**[Misc](#misc)**
**[Criptografia](#criptografia)**
**[NET](#net)**
## Misc
### Misc30
O primeiro desafio de misc era um arquivo zip com senha, eu fiz um ataque de força bruta usando fcrackzip e a rockyou como wordlist

```bash
$fcrackzip -v -u -D -p rockyou.txt misc30.zip
found file 'flag.txt', (size cp/uc     45/    33, flags 9, chk 60c8)


PASSWORD FOUND!!!!: pw == sakura10
```
Depois era só descompactar o arquivo e ver a flag que estava encriptada em base64
```bash
$base64 -d flag.txt
EY{BRUT3_F0RC3_R0CK_Y0U}
```

### Misc50
Esse era o desafio mais complicadinho, uma pasta com várias imagens que juntas formam algo que leva à flag, pra juntar essas imagens eu fiz um código em python usando a biblioteca pillow e executei ele no diretório onde estavam as imagens
```python
from PIL import Image

new_image = Image.new("RGB", (250, 250))

w, h = 0, 0

for i in range(1, 26):
    for j in range(1, 26):
        im = Image.open(str(i)+"-"+str(j)+".png")
        new_image.paste(im,(w,h))
        w += (im.size)[0]
    h += (im.size)[1]
    w = 0

new_image.save("out.jpg", "JPEG")
```
A imagem de saída era um QRcode, era só colocar no leitor qualquer e capturar a flag, eu usei o zbarimg por questões de praticidade
```bash
$zbarimg out.jpg
QR-Code:EY{1M_G0NN4_N33D_M0R3_GLU3}
scanned 1 barcode symbols from 1 images in 0.06 seconds
```

## Criptografia
O desafio de cripto era um simples texto em base32, só decodificar e pegar a flag

```bash
$echo "IVMXWVCIGE2V6MJVL43TAMC7GM2DKWL5BI======" | base32 -d
EY{TH15_15_700_345Y}
```

## NET
### NET30
Nesse desafio era dado um um arquivo .pcap com um tráfego telnet, eram poucos pacotes então era usar alguma ferramenta como wireshark e ir procurando
![Foto do pacote com a flag](misc30.png)
