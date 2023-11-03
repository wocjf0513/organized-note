
* CASE - WHEN- THEN -END AS '별명'

* DATE_FORMAT(필드 명,"%Y-%m-&d")

* IF(조건문, 참일때 값, 거짓일 때 값) AS '별명'

[[MYSQL] 📚 테이블 조인(JOIN) - 그림으로 알기 쉽게 정리](https://inpa.tistory.com/entry/MYSQL-%F0%9F%93%9A-JOIN-%EC%A1%B0%EC%9D%B8-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EA%B8%B0%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC)

INNER JOIN 

(LEFT JOIN 같은 경우는 LEFT에 있는 테이블을 다 가져온다. 그 행 중 ON조건에 맞지 않는 컬럼들은 NULL로 채워진다.

MSSQL은 FULL OUTER 를 지원X 해서 LEFT , RIGHT UNION을 이용한다.)

SET @var= 쿼리문 (변수 선언)

GROUP BY ( ) HAVING 조건

## 기본

* INSERT INTO dbo.취미 (나이, 반정보, 이름, 취미) VALUES (16, 'A반', '홍길동', '등산')

* UPDATE Reservation
  
  SET RoomNum = 2002
  
  WHERE Name = '홍길동';

* DELETE FROM 테이블이름
  
  WHERE 필드이름=데이터값

* 문자열 함수

LENGTH()  문자열의 길이를 반환

CONCAT('Oracle', NULL, 'Corporation') : 문자열 합치기

LOCATE('abc', 'ababcDEFabc') : 위치 검색

LEFT('MySQL PHP HTML Java', 5), => MySQL

LOWER('MySQL') => mysql

UPPER

REPLACE('MySQL', 'My', 'MS ') => MSSQL

TRIM('Hello world') => Helloworld

* 그룹 함수 

MIN, MAX

AVG

UNION : 자동으로 중복제거

UNION ALL : 중복 제거 없이 합친 모든 행을 반환 
