### **SHA-1 알고리즘의 취약성 (충돌 공격 가능)**
(참고용임)

SHA-1이 취약한 이유는 **"충돌 공격(Collision Attack)"**이 가능하기 때문이야.  

#### 1️⃣ **해시 함수의 기본 개념**  
해시 함수는 **임의의 입력값을 고정된 길이의 해시값으로 변환하는 함수**야.  
- 예를 들어, `"Hello"` → `SHA-1` → `f572d396fae9206628714fb2ce00f72e94f2258f`  
- `"Hello, world"` → `SHA-1` → `2ef7bde608ce5404e97d5f042f95f89f1c232871`  
- 같은 입력이면 항상 같은 해시값이 나와야 하고, **다른 입력이면 해시값도 달라야 함.**  

#### 2️⃣ **충돌 공격 (Collision Attack)이란?**  
**충돌(Collision)**이란 **서로 다른 두 개의 입력값이 같은 해시값을 가지는 경우**를 말해.  
- 보안이 강한 해시 함수라면 이런 충돌이 절대 발생하면 안 돼.  
- 그런데 **SHA-1은 충돌을 강제로 만들어낼 수 있음.**  
- 2017년 구글과 CWI 암스테르담 연구팀이 `SHAttered` 공격을 통해 SHA-1 해시 충돌을 성공함.  
  - 즉, 두 개의 **완전히 다른 파일**이 같은 SHA-1 해시값을 가질 수 있음.  
  - 이 말은 **공격자가 악성 코드가 포함된 파일을 원본 파일로 위장할 수 있다는 뜻.**  

#### 3️⃣ **SSL 인증서에서 SHA-1을 사용하면 문제되는 이유**  
- SSL 인증서는 **서버의 신뢰성을 증명**하는 역할을 함.  
- 만약 공격자가 가짜 인증서를 만들어서 **진짜 인증서와 같은 SHA-1 해시값을 가지게 만들면**,  
  - **중간자 공격(MITM, Man-in-the-Middle Attack)**이 가능해짐.  
  - 즉, 사용자는 악성 사이트에 접속했는데도 **"이 사이트는 안전함(🔒)"** 이라고 표시될 수 있음.  
- 그래서 **SHA-1 기반 루트 인증서는 더 이상 안전하지 않다고 판단됨.**  

---

### **UserTrust RSA Certification Authority로 변경 시 SSL 인증서 체인 구조**
AAA Certificate Services (SHA-1) 루트를 버리고, **UserTrust RSA Certification Authority**로 바꾸면 SSL 인증서 체인(Chain)이 달라짐.  

#### **1️⃣ 기존 (SHA-1 사용, 폐기된 체인)**
```
(루트 인증서)      AAA Certificate Services (SHA-1, 폐기됨)
   ↓
(중간 인증서)      Sectigo RSA Organization Validation Secure Server CA
   ↓
(서버 인증서)      example.com (웹사이트 SSL 인증서)
```
- AAA Certificate Services 루트가 **SHA-1** 기반이라 신뢰할 수 없음.  

#### **2️⃣ 변경 후 (SHA-256 기반, 안전한 체인)**
```
(루트 인증서)      UserTrust RSA Certification Authority (SHA-256, 신뢰 가능)
   ↓
(중간 인증서)      Sectigo RSA Organization Validation Secure Server CA
   ↓
(서버 인증서)      example.com (웹사이트 SSL 인증서)
```
- **루트 인증서를 `UserTrust RSA Certification Authority`로 변경**하면,  
  - 전체 체인이 **SHA-256 기반**으로 강화됨.  
  - 최신 브라우저(Firefox, Chrome, Edge 등)에서 정상적으로 SSL 인증서를 신뢰함.  

---

### **📌 요약**
1. **SHA-1은 충돌 공격이 가능**해서 보안성이 약함.  
2. **SHA-1 기반 루트 인증서(AAC Certificate Services)는 폐기됨.**  
3. **UserTrust RSA Certification Authority**를 루트로 사용해야 SSL 인증서가 정상 작동함.  
4. **SSL 인증서 체인을 변경해야 하며, 루트 인증서를 최신 SHA-256 기반으로 교체해야 함.**  

결론: **이제 SSL 인증서 체인을 `UserTrust RSA Certification Authority` 루트로 바꿔야 함.** 🔒
