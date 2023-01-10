---
title: "[MySQL] sql 파일을 import, export하기"

categories:
  - data_and_ai
tags:
  - [데이터, SQL, MySQL, import, export]
---

Export로 인해 만들어진 *.sql 파일을 다른 PC에서 Import 하는 방법에 대해 알아보자.   
여러 가지 방법이 있겠지만, 내가 사용한 방법은 아래와 같다.   

# Import

```sql
mysql -u[아이디] -p [데이터베이스명] < [SQL 파일 경로]
```

# Export

```sql
mysqldump -u root -p [database name] > ../../../[name.sql]
```