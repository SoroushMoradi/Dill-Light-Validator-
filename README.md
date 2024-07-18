# Dill-Light-Validator

The Most Scalable  
Data Availability Network    
Dill is the first modular network fully compatible with the Danksharding roadmap.  



![Dill Light Validator (1)](https://github.com/user-attachments/assets/24d66592-713e-4616-92d5-0b555a2ed2d8)



**Download the light validator binary package from:**
```
curl -O https://dill-release.s3.ap-southeast-1.amazonaws.com/linux/dill.tar.gz
```

**Extract the package:**
```
tar -xzvf dill.tar.gz && cd dill
```

Sample(چنین چیزی باید ببینید):
```
ubuntu@ip-xxxx:~$ curl -O https://dill-release.s3.ap-southeast-1.amazonaws.com/linux/dill.tar.gz
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 64.5M  100 64.5M    0     0  6655k      0  0:00:09  0:00:09 --:--:-- 8072k
ubuntu@ip-xxxx:~$ tar -xzvf dill.tar.gz && cd dill
dill/
dill/validators.json
dill/genesis.ssz
dill/dill_validators_gen
dill/start_light.sh
dill/dill-node
ubuntu@ip-xxxx:~/dill$
```
**Generate validator keys using the key generation tool:  
ساخت کیف پول جدید**
__save phrases__
```
./dill_validators_gen new-mnemonic --num_validators=1 --chain=andes --folder=./
```
**sample:  
چیزی که نهایتا باید ببینید**
```
ubuntu@ip-xxxx:~/dill$ ./dill_validators_gen new-mnemonic --num_validators=1 --chain=andes --folder=./

***Using the tool on an offline and secure device is highly recommended to keep your mnemonic safe.***

Please choose your language ['1. العربية', '2. ελληνικά', '3. English', '4. Français', '5. Bahasa melayu', '6. Italiano', '7. 日本語', '8. 한국어', '9. Português do Brasil', '10. român', '11. Türkçe', '12. 简体中文']:  [English]: 3
Please choose the language of the mnemonic word list ['1. 简体中文', '2. 繁體中文', '3. čeština', '4. English', '5. Italiano', '6. 한국어', '7. Português', '8. Español']:  [english]: 4
Create a password that secures your validator keystore(s). You will need to re-enter this to decrypt them when you setup your Dill validators.:
Repeat your keystore password for confirmation:
The amount of DILL token to be deposited(2500 by default). [2500]:
This is your mnemonic (seed phrase). Write it down and store it safely. It is the ONLY way to retrieve your deposit.


Creating your keys.
Creating your keystores:	  [####################################]  1/1
Verifying your keystores:	  [####################################]  1/1
Verifying your deposits:	  [####################################]  1/1

Success!
Your keys can be found at: ./validator_keys


Press any key.
ubuntu@ip-xxxx:~/dill$
```
**این ولیدیتور کی هارو میسازه و داخل فایل ./validator_keys ذخیرش میکنه:**
```
ls -ltr ./validator_keys
```
Sample:
```
total 8
-r--r----- 1 ubuntu ubuntu 710 Jul 13 08:06 keystore-m_12381_3600_0_0_0-xxxxxx.json
-r--r----- 1 ubuntu ubuntu 706 Jul 13 08:06 deposit_data-xxxx.json
ubuntu@ip-xxxxx:~/dill$
```
**Import your keys to your keystore:**  
اینجا پسوورد هایی که میدین و pubkey رو ذخیره کنید.
```
./dill-node accounts import --andes --wallet-dir ./keystore --keys-dir validator_keys/ --accept-terms-of-use
```
sample:
```

ubuntu@ip-xxxxx:~/dill$ ./dill-node accounts import --andes --wallet-dir ./keystore --keys-dir validator_keys/ --accept-terms-of-use
[2024-07-13 08:08:34]  INFO flags: Running on the Andes Beacon Chain Testnet
Password requirements: at least 8 characters
New wallet password:
Confirm password:
[2024-07-13 08:08:39]  INFO wallet: Successfully created new wallet walletPath=/home/ubuntu/dill/keystore
[2024-07-13 08:08:39]  WARN client: You are using an insecure gRPC connection. If you are running your beacon node and validator on the same machines, you can ignore this message. If you want to know how to enable secure connections, see: https://docs.prylabs.network/docs/prysm-usage/secure-grpc
[2024-07-13 08:08:39]  INFO accounts: importing validator keystores...
[2024-07-13 08:08:39]  INFO accounts: checking directory for keystores: /home/ubuntu/dill/validator_keys
Enter the password for your imported accounts:
Importing accounts, this may take a while...
Importing accounts... 100% [===================================================================================]  [1s:0s]
[2024-07-13 08:08:44]  INFO local-keymanager: Reloaded validator keys into keymanager
[2024-07-13 08:08:44]  INFO local-keymanager: Successfully imported validator key(s) pubkeys=0xxxxx
[2024-07-13 08:08:44]  INFO accounts: Imported accounts [xxxxxx], view all of them by running `accounts list`
ubuntu@ip-xxxxx:~/dill$
```

**Write the password you configured in the previous step into a file:  
پسووردی که قبلا انتخاب کردینو اینجا جایگزین کنید:**
```
echo {my-password} > walletPw.txt
```

**start the light validator node:  
استارت نود:**
```
./start_light.sh -p walletPw.txt
```

sample:
```
ubuntu@xxxxx:~/dill$ ./start_light.sh -p walletPw.txt
Option --pwdfile, argument 'walletPw.txt'
Remaining arguments:
using password file at walletPw.txt
start light node
start light node done
ubuntu@xxxxx:~/dill$ nohup: redirecting stderr to stdout

```
**check the node ruuning  
بررسی سلامت نود:**
```
ps -ef | grep dill
```
sample:
```
ubuntu      1981       1 86 08:09 pts/0    00:00:43 /home/ubuntu/dill/dill-node --light --embedded-geth --datadir /home/ubuntu/dill/light_node/data/beacondata --genesis-state /home/ubuntu/dill/genesis.ssz --grpc-gateway-host 0.0.0.0 --initial-validators /home/ubuntu/dill/validators.j
...
```
Done.


