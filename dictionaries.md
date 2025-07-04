# 📖0627_dictionaries

- 파이썬의 **딕셔너리**는 데이터를 **key와 value**의 쌍으로 저장하는 자료형입니다. 실제 사전(dictionary)처럼, 특정 단어(키)를 통해 그 뜻(값)을 바로 찾을 수 있는 구조입니다.


### ✅1. 딕셔너리의 특징
- **키와 값 쌍**으로 데이터를 저장합니다. 예시: {'name': 'Alice', 'age': 25}
- **키를 통해 값에 빠르게 접근**할 수 있습니다. 순서가 아니라 "이름표"로 찾는 방식입니다.
- **키는 중복될 수 없고**, **고유**해야 합니다. 같은 키를 두 번 넣으면 마지막 값으로 덮어씁니다.
- **값은 어떤 자료형이든** 올 수 있습니다(숫자, 문자열, 리스트, 딕셔너리 등).
**변경 가능**하며, 값을 자유롭게 추가·수정·삭제할 수 있습니다.
  
### ✅2. 딕셔너리 생성 방법
```
# 가장 기본적인 방법
person = {'name': 'Alice', 'age': 25, 'city': 'Seoul'}

# 빈 딕셔너리 만들기
empty = {}
empty2 = dict()

# 다양한 생성 방법
d1 = dict(name='Tom', age=20)
d2 = dict([('name', 'Tom'), ('age', 20)])
```

### ✅3. 딕셔너리 요소 접근 및 추가/수정/삭제
```
# 값 접근
print(person['name'])      # 'Alice'
print(person.get('age'))   # 25

# 값 추가/수정
person['job'] = 'Engineer'     # 추가
person['age'] = 26             # 수정

# 값 삭제
del person['city']             # 'city' 키와 값 삭제
person.pop('job')              # 'job' 키와 값 삭제
```

### ✅4. 주요 메서드
<img width="737" alt="스크린샷 2025-07-04 15 14 55" src="https://github.com/user-attachments/assets/e11bacc8-dbaa-4726-95e6-11d359ce5b0a" />


### ✅5. 딕셔너리의 키와 값
- 키: 불변(immutable) 타입만 가능. (숫자, 문자열, 튜플 등)
- 값: 모든 자료형 가능 (리스트, 딕셔너리, 숫자, 문자열 등).
```
# 키로 리스트, 집합(set)은 불가
# dic = {[1,2]: 'hi'}  # 오류 발생
```

### ✅6. 반복문과 딕셔너리
```
for key in person:
    print(key, person[key])

for k, v in person.items():
    print(f"{k}: {v}")
```

### ✅7. 활용 예시
```
score = {'math': 90, 'english': 85, 'science': 95}
print(score['math'])  # 90

# 값 추가
score['history'] = 88

# 값 수정
score['english'] = 90

# 값 삭제
del score['science']
```

### ✅8. 딕셔너리의 장점
- **빠른 조회**: 리스트처럼 순차적으로 찾지 않고, 키로 바로 접근(O(1) 속도).
- **구조적 데이터 관리**: 이름-값, ID-정보 등 구조화된 데이터에 적합.
- 정리: 파이썬 딕셔너리는 키-값 쌍으로 데이터를 저장하고, 키를 통해 값을 빠르고 효율적으로 관리할 수 있는 매우 강력한 자료형입니다. 데이터의 추가, 수정, 삭제, 조회가 모두 쉽고, 다양한 메서드를 활용할 수 있습니다.
