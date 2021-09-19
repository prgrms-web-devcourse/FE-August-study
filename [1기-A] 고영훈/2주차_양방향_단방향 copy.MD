암호화가 되지 않은 자료를 평문(plain text)이라고 한다.. 평문을 암호 알고리즘을 통해 암호문(cypher text)으로 만드는 과정을 암호화(encryption)라고 한다.

반대로, 암호문을 주어진 암호 알고리즘을 가지고 평문으로 돌이키는 과정을 복호화(decryption)라고 한다..

우선 암호의 기본적인 3가지 특성을 짚고 넘어가고자 한다..

- **기밀성(Confidentiality)**: 암호화된 내용이 무엇인지 알 수 없어야 한다는 특성
- **무결성(Integrity)**: 원본과 확실한 데이터라는 것이라는 특성
- **인증(Authentication)**: 권한이 있는 사람만 접근할 수 있는 특성

우선, 가장먼저 분류되는 방식은 양방향, 단방향 알고리즘이다. 간단하게 말해서

**양방향 알고리즘은 암호화된 암호문을 복호화 할 수 있는 알고리즘을 의미**

한다. 반대로 **단방향 알고리즘은 암호화는 수행하지만 절대로 복호화가 불가능한 알고리즘**

을 말한다. 양방향 알고리즘은 대표적으로 대칭키(비공개키) 방식과 비대칭키(공개키) 방식으로 나눠지며, 단방향은 Hash방식이 대표적이다.

## 단방향 암호

단방향 암호화는 암호화는 가능하지만, 복호화가 불가능한 특징을 가지고 있습니다. 복호화가 불가능하기 때문에 무결성에 초점을 두고 있다.

즉, 원본 데이터가 변경되었거나 훼손되지 않았는지 확인하는 데 초점을 둔다. 예를 들어 비밀번호를 저장할 때 단방향으로 암호화하여 암호문을 저장해두고 사용자가 비밀번호를 다시 입력했을 때 저장된 암호문과 비교해서 일치하면 인증을 시켜준다.

평문을 암호화화하는건 가능하지만 복호화는 불가능.

![img](https://lh5.googleusercontent.com/g4tGXv9wVk24vD4FPSjz1WnX476HsmicRBetXcR_DdfIAlC6buRAKbKsRKVFgcdWgEvDdt9BhGgGra01ZhJey4KEmI4bRAjvQNjEDdpTOPfRfDKUxtd3AIXHDmZcSqDxSsdKhnKdYdQ)

그러나 레인보우 테이블을 이용해서 하나하나 대칭해가면서 비교하면 찾을 수 있다.

![img](https://lh4.googleusercontent.com/Xc3b2l4flmhC_vAmiWRqiaglHiamo1YmW9YtLnQa1OCYjeQKnvCvugjz_0L4WoBOh3JnvfsuWEpfqwRFOZ8C-pHBXtnjtc_lMRmUJC4WtESLG45LH-P9tY6na8jForG8tSz57A08fMI)

이걸 방지하기 위해 salt라는 기법을 사용한다. salt는 랜덤한 string인데 이것을 원본 암호에 부착하여 해시함수에 통과시킨다.

![img](https://lh6.googleusercontent.com/cidu7_xMVNc0PP2aCEKszatSVFifh_WDzm4eYvMb7HDgoDOQPA8WxL7ukYlRH34lGfzWR8n3yYAr2HJ0Ea37DXRftrYMxjMzg7YstZAKrvW93cC6afUzXBso2rqy6z8KFDgTvs7dtVk)

그래서 암호가 저장된 DB가 털려도 안전하다. 그럼 복호화가 불가능한데 어떻게 암호소유자를 검증하느냐?

사용자로부터 받은 암호를 똑같은 방식으로 암호화해서 암호화된 패스워드끼리 비교하면 된다. 해시 함수가 사용된다.

파이썬에서 관련 라이브러리가 있으니 한번 실습해보자. 'Hermione' 라는 암호를 입력하고 salt한다음에 암호와 salt를 print해보았다.

그리고 input1,input2에 암호를 입력해서 이미 salt를 칠해놓고 저장되어있는 원본암호와 비교해보았다.

```python
import bcrypt

original_pass = 'Hermione'.encode('utf-8')
salt = bcrypt.gensalt()

hashed_password = bcrypt.hashpw(original_pass, salt)
print('hashed_pass:',hashed_password)
print('salt:',salt)

input1_pass = 'Hermi'
print('input1_pass:',bcrypt.checkpw(input1_pass.encode('utf-8'), hashed_password)) # return True

input2_pass = 'Harry'
print('input2_pass:',bcrypt.checkpw(input2_pass.encode('utf-8'), hashed_password)) # return False

#hashed_pass: b'$2b$12$HOGelYJgRZWYcJZdHNvPJ.320S7fYYsZb8vIa4VbhoVMAKlusgYb.'
#salt: b'$2b$12$HOGelYJgRZWYcJZdHNvPJ.'
#input1_pass: False
#input2_pass: False
```

## 양방향 암호

### 비대칭키(공개키): 암호화 복호화 서로 다른 방식

예를 들어 Alice 와 Bob이 서로 비대칭키를 이용해 정보를 암호화시켜 주고받는다고 하자.

우선 각자의 공개키를 교환한다. 그리고 문서를 각자가 받은 공개키(public key)로 암호화 시켜 전자서명의 형태로 문서에 부착한다. 그리고 그 문서를 전달하면 자신이 가지고 있는 비밀키(private key)로 문서를 열람할 수 있다.

우편함에 비유를 하자. 내가 가진 우편함의 입구를 여는 키는 모두가 가지고 있다. 그래서 그 키를 이용해서 열 수 있다. 하지만 우편함에 있는 봉투를 꺼내기 위해 열어야 하는 출구열쇠는 나만 열 수 있다고 보면 된다. 입구를 열기 위한 키는 공개키이고 출구를 열기위한 키는 비밀키이다.

비대칭키는 아래서 실습이 가능하니 실습해보자.

https://www.devglan.com/online-tools/rsa-encryption-decryption

### 대칭키(비공개키): 암호화 복호화 동일한 방식

대칭키랑은 다르게 암호화할때 사용되는 키랑 복호화할때 사용되는키랑 같다는 특징이 있다.

https://encode-decode.com/aes256-encrypt-online/

### 비교

|             대칭키             |           비대칭키           |
| :----------------------------: | :--------------------------: |
|           **빠르다**           |          **느리다**          |
| **데이터 용량이 큰 곳에 사용** | 데이터 용량이 작은곳에 사용  |
|  **보안에 취약-같은 키 사용**  | **보안이 철저-다른 키 사용** |

출처: ([자바공작소)[https://javaplant.tistory.com/26](https://javaplant.tistory.com/26)
