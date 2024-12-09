# SQLite3에서 `sqlite3.Row` 사용 방법

`sqlite3.Row`를 사용하면 데이터베이스 조회 결과를 컬럼 이름 기반으로 접근할 수 있어 코드 가독성과 유연성이 향상됩니다.

---

## 예제 코드: `sqlite3.Row` 활용



```python
import sqlite3

# 데이터베이스 연결
connection = sqlite3.connect("example.db")

# Row 형식 설정
connection.row_factory = sqlite3.Row

try:
    # 커서 생성
    cursor = connection.cursor()

    # 테이블 생성 (예제용)
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS users (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            age INTEGER NOT NULL
        )
    """)

    # 데이터 삽입
    cursor.execute("INSERT INTO users (name, age) VALUES (?, ?)", ("Alice", 30))
    connection.commit()

    # 데이터 조회
    cursor.execute("SELECT * FROM users")
    rows = cursor.fetchall()

    # Row 객체를 딕셔너리처럼 출력
    for row in rows:
        print(dict(row))  # {'id': 1, 'name': 'Alice', 'age': 30}

except sqlite3.Error as e:
    print(f"오류 발생: {e}")

finally:
    connection.close()

```



## 주요 설정: connection.row_factory
* connection.row_factory = sqlite3.Row:
    * 이 설정을 추가하면 fetchone()이나 fetchall()로 반환되는 결과가 sqlite3.Row 객체로 변환됩니다.
    * sqlite3.Row는 딕셔너리와 유사한 방식으로 데이터를 다룰 수 있습니다.
    * row['column_name'] 형태로 컬럼에 접근할 수 있습니다.


출력 예시
위 코드를 실행하면 다음과 같이 출력됩니다
{'id': 1, 'name': 'Alice', 'age': 30}


## 장점
1. 가독성 증가:
    * 인덱스 번호 대신 컬럼 이름으로 데이터에 접근하므로 코드 가독성이 좋아집니다.
2. 유연성 제공:
    * 컬럼 순서가 바뀌어도 코드를 수정할 필요가 없습니다.

---

## 만약 Row 설정 없이 데이터를 딕셔너리 형태로 변환하려면, 
## 다음과 같이 fetchall() 결과를 수동으로 변환할 수도 있습니다

rows = cursor.fetchall()
columns = [desc[0] for desc in cursor.description]
result = [dict(zip(columns, row)) for row in rows]
