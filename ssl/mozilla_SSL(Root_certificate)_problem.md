### mozilla SSL(Root 인증서) 문제
(참고용임)

Mozilla가 **AAA Certificate Services (SHA-1)** 루트 인증서를 신뢰하지 않는 이유는 다음과 같다.  

### 1. **SHA-1의 보안 취약성**  
SHA-1 알고리즘은 **충돌 공격**이 가능한 취약한 해시 함수다.  
- 2017년 구글과 CWI 암스테르담 연구팀이 **"SHAttered" 공격**을 통해 실제 충돌을 입증함.
- 이후 주요 브라우저(Chrome, Firefox, Edge 등)는 **SHA-1 기반 인증서를 차단**하기 시작.
- Mozilla도 보안 강화를 위해 **SHA-1 루트 인증서를 더 이상 신뢰하지 않음.**  

### 2. **Sectigo(구 Comodo) 인증서 구조 변경**  
AAA Certificate Services 루트 인증서는 **Sectigo(구 Comodo)**에서 운영하는 루트 중 하나였음.  
- 하지만 **SHA-1 루트는 더 이상 사용되지 않으며**, Mozilla는 이를 신뢰 목록에서 제거함.  
- 대신 **SHA-256 기반의 최신 루트 인증서를 신뢰하도록 전환함.**  

---

### **그럼, UserTrust 인증서를 루트로 저장하면 되나?**  
맞음. **UserTrust RSA Certification Authority**가 Sectigo의 현재 신뢰할 수 있는 루트 인증서다.  
- Mozilla의 신뢰할 수 있는 루트 인증서 목록에는 **UserTrust RSA Certification Authority**가 포함되어 있음.  
- 따라서 **Sectigo SSL 인증서 체인을 구성할 때, AAA Certificate Services 대신 UserTrust RSA를 루트로 설정해야 함.**  

#### **📌 해결 방법**
1. **Sectigo SSL 인증서를 사용할 경우**  
   - 인증서 체인의 최상위 루트가 `UserTrust RSA Certification Authority`인지 확인.
   - 기존의 `AAA Certificate Services (SHA-1)` 루트 인증서를 제거.  

2. **Mozilla Firefox 등에서 SSL 오류가 발생할 경우**  
   - 최신 루트 인증서 체인으로 갱신.
   - `UserTrust RSA Certification Authority`를 신뢰하도록 설정.  

즉, **AAA Certificate Services (SHA-1)는 더 이상 사용하지 않고, UserTrust RSA를 루트로 저장하는 게 맞다.**
