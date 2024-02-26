# RSA

## 키만들기

### 개인키 만들기

- openssl genrsa -out file_name.key 2048(키사이즈)

#### 키정보 확인

- oepnssl rsa -in file_name.key -text

#### 키에 패스워드 추가

- openssl rsa -in file_name.key -out file_name_key.key -aes256

#### 처음부터 패스워드를 부여하여 키 생성

- openssl genrsa -aes256 -out file_name.key 2048

#### 패스워드 제거

- openssl rsa -in key_file_name.key -out file_name.key

### 공개키 만들기

- openssl rsa -in privatekey.key -pubout publickey.key

### CSR 파일 만들기

- openssl req -new -key privatekey.key -out csr.csr

### CRT 파일 만들기

- openssl x509 -req -days <유효기간 없어도됨> -in crt파일명 -signkey **private.key** -out crt.crt
- 유효기간을 안넣으면 한달로 된다.

### CRT 파일 확인

- openssl x509 -in crt.crt -noout -text

### CSR없이 CRT 파일 생성

- openssl req -new -x509 -days <유효기간 없어도됨> -nodes -keyout **private.key** -out crt.crt
  > nodes는 키파일 패스워드 제거를 위하여 입력함

### CRT파일에서 공개키 만들기

- openssl x509 -inform PEM -in certificate.crt -pubkey -noout > public_key.pem
