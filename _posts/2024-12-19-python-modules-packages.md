---
title: "[Python] 파이썬 모듈과 패키지 정리"
excerpt: "파이썬 모듈과 패키지는 기본 중의 기본이나 꼭 쓰려고 하면 하나씩 헷갈려서 이번 기회에 정리해보았다. 정리하고 보니, 지피티한테 그간 질문했던 것의 모음집인 것 같다."

categories:
  - Development
tags:
  - python
  - module
  - package
  - import

permalink: /development/python-modules-packages/

toc: true
toc_sticky: true

date: 2025-06-26
last_modified_at: 2025-06-26
---

파이썬 모듈과 패키지는 기본 중의 기본이나 꼭 쓰려고 하면 하나씩 헷갈려서 이번 기회에 정리해보았다. 정리하고 보니, 지피티한테 그간 질문했던 것의 모음집인 것 같다.

---

## 1. 모듈이란?

모듈은 `.py` 확장자를 가진 파이썬 파일이다. 안에 함수, 변수, 클래스 등 필요한 코드들을 넣어두고 다른 파일에서 불러와 쓸 수 있다. 그냥 기능별로 잘라서 저장해놓는 느낌.

### 왜 쓰냐면

* 코드 길어지면 관리가 너무 힘드니까
* 똑같은 기능 여러 번 만들기 귀찮으니까

### 예시

```python
# greet_module.py
def greet(name):
    return f"안녕하세요, {name}님!"
```

---

## 2. 모듈 가져오기 (import)

모듈은 `import`로 불러온다. 방식은 다양하다.

```python
# 기본
import greet_module
print(greet_module.greet("홍길동"))

# 함수만 가져오기
from greet_module import greet
print(greet("홍길동"))

# 이름 길면 짧게 별칭 주기
import greet_module as gm
print(gm.greet("홍길동"))
```

---

## 3. `__name__`의 정체

파이썬에는 `__name__`이라는 속성이 있는데, 이게 뭐냐면...

* 직접 실행하면 `__name__ == "__main__"`
* import 당하면 `__name__ == 모듈 이름`

그래서 아래처럼 쓰면 테스트 코드가 import될 때는 실행되지 않음.

```python
# greet_module.py
def greet(name):
    return f"안녕하세요, {name}님!"

if __name__ == "__main__":
    print(greet("테스트"))
```

---

## 4. 파이썬 기본 모듈들

설치 안 해도 되는 내장 모듈들 중 자주 쓰는 거 몇 개만 정리.

```python
# math - 수학 계산
import math
print(math.sqrt(25))

# random - 랜덤값 뽑기
import random
print(random.randint(1, 6))

# datetime - 시간 정보
from datetime import datetime
print(datetime.now())
```

---

## 5. 내가 만든 모듈도 가능

```python
# mymath.py
def add(a, b):
    return a + b
```

```python
# main.py
import mymath
print(mymath.add(3, 5))
```

---

## 6. 패키지란?

여러 모듈을 폴더에 모아놓은 것. `__init__.py` 파일이 있으면 파이썬이 "이거 패키지구나" 인식함.

```
mypackage/
├── __init__.py
├── math_operations.py
├── string_operations.py
```

```python
# math_operations.py
def add(a, b):
    return a + b

# string_operations.py
def to_uppercase(s):
    return s.upper()
```

```python
# 사용
from mypackage.math_operations import add
from mypackage.string_operations import to_uppercase

print(add(2, 3))
print(to_uppercase("hello"))
```

---

## 7. 상대 경로 import

같은 패키지 내부에서 다른 모듈을 불러올 땐 `from .` 붙여서 상대 경로로 쓴다.

```python
# math_operations.py
from .string_operations import to_uppercase

def add_and_upper(a, b):
    return to_uppercase(str(a + b))
```

---

## 8. 모듈 검색 경로 확인

파이썬은 모듈을 어디서부터 찾는지 `sys.path`를 참고한다.
필요하면 경로를 추가할 수도 있다.

```python
import sys
print(sys.path)

sys.path.append("/path/to/module")
```

---

## 9. 실습 예제: 학생 관리 시스템

모듈과 패키지를 써보는 연습으로 만들어본 간단한 예제.

```
school/
├── __init__.py
├── student_management.py
├── course_management.py
main.py
```

```python
# student_management.py
students = []

def add_student(name, age):
    students.append({"name": name, "age": age})

def list_students():
    return students
```

```python
# course_management.py
courses = []

def add_course(title, instructor):
    courses.append({"title": title, "instructor": instructor})

def list_courses():
    return courses
```

```python
# main.py
from school import add_student, list_students, add_course, list_courses

add_student("홍길동", 20)
add_student("김영희", 22)

add_course("파이썬", "홍길동")
add_course("자료구조", "김영희")

print(list_students())
print(list_courses())
```

---

## 마무리

모듈이랑 패키지 개념은 알 것 같다가도 실전에서 자꾸 까먹는다. 이렇게 정리해두면 나중에 다시 봐도 좀 덜 헷갈릴 듯. 앞으로 프로젝트 짤 때 이 구조로 자연스럽게 나눌 수 있도록 익숙해져야겠다.

---

> 참고: [공식 Python 튜토리얼 - Modules](https://docs.python.org/3/tutorial/modules.html) 