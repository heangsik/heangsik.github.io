# Node 최신 버젼 설치

## 설치법

1. curl 설치
   ```
   sudo apt-get install curl
   ```
2. nvm 설치
   ```
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
   ```
3. nvm 설치 확인
   ```
   command -v nvm
   ```
   - 만약 vnm이 출력이 되지 않으면 창을 새로 열어서 확인한다.
4. nvm으로 설치 내역 확인
   ```
   nvm -ls
   ```
   ![nvm_list](../image/nvm%20install%20list.jpg)
5. nvm 으로 node 설치
   ```
   nvm install --lts
   ```
   ![nvm_install](../image/nvm%20installing.jpg)
6. 설치 내역 확인
   ```
   nvm -ls
   ```
   ![nvm_installed](../image/nvm%20installed.jpg)
