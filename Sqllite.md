# SQLite 데이터 삽입 예제

이 프로젝트는 SQLite를 사용하여 간단히 데이터를 삽입하는 예제 코드입니다. 

## 코드 설명

### 주요 단계
1. **데이터베이스 연결**: `sqlite3.connect()`를 사용하여 데이터베이스에 연결합니다. 데이터베이스 파일이 존재하지 않으면 새로 생성됩니다.
2. **커서 생성**: `connection.cursor()`를 호출하여 데이터베이스 작업을 수행할 커서를 생성합니다.
3. **SQL 실행**: `cursor.execute()`를 사용하여 SQL 쿼리를 실행합니다. 이 예제에서는 `users` 테이블에 데이터를 삽입합니다.
4. **변경 내용 저장 (커밋)**: `connection.commit()`을 호출하여 변경 내용을 저장합니다.
5. **오류 처리**: 오류가 발생하면 `connection.rollback()`을 호출하여 변경 내용을 롤백합니다.
6. **연결 닫기**: `connection.close()`를 호출하여 데이터베이스 연결을 종료합니다.

### 코드
```python
import sqlite3

# 1. 데이터베이스 연결
connection = sqlite3.connect("example.db")

try:
    # 2. 커서 생성
    cursor = connection.cursor()

    # 3. SQL 실행
    cursor.execute("INSERT INTO users (name, age) VALUES (?, ?)", ("Alice", 30))

    # 4. 변경 내용 저장 (커밋)
    connection.commit()
    print("데이터 삽입 성공!")

except sqlite3.Error as e:
    # 오류 발생 시 롤백
    connection.rollback()
    print(f"오류 발생: {e}")

finally:
    # 5. 연결 닫기
    connection.close()
