![](https://velog.velcdn.com/images/ekdeon/post/947023a1-1f43-4b82-a0ea-cfbdc3ea702c/image.png)

---

### 1. LEFT JOIN (= LEFT OUTER JOIN)Permalink

![](https://velog.velcdn.com/images/ekdeon/post/78727011-cd83-4724-924d-031bb75725dc/image.png)
- LEFT OUTER JOIN 이라고도 함.

```sql
SELECT * FROM TableA A LEFT JOIN TableB B ON A.col1 = B.col1;
```
![](https://velog.velcdn.com/images/ekdeon/post/644d8c0e-72d6-4931-b79c-2b861e934910/image.png)

---

### 2. RIGHT JOIN (= RIGHT OUTER JOIN)Permalink

![](https://velog.velcdn.com/images/ekdeon/post/3539dcdd-405e-4928-b268-526d75c81779/image.png)


```sql
SELECT * FROM TableA A RIGHT JOIN TableB B ON A.col1 = B.col1;
```
![](https://velog.velcdn.com/images/ekdeon/post/69c435a3-c20a-4ab4-a3c7-cf1613fb91fd/image.png)

---

### 3. LEFT JOIN (순수 A만 구할 때)Permalink

![](https://velog.velcdn.com/images/ekdeon/post/2f2bca9e-c9db-4a63-8b4e-3709340565d1/image.png)

```sql
SELECT * FROM TableA A LEFT JOIN TableB B ON A.col1 = B.col1 WHERE B.col1 IS NULL;
```

![](https://velog.velcdn.com/images/ekdeon/post/c85f18b4-5f20-4d3b-a22d-5094749aae63/image.png)


---

### 4. RIGHT JOIN (순수 B만 구할 때)Permalink

![](https://velog.velcdn.com/images/ekdeon/post/578c63ec-b4ca-4a48-bfa4-92ded50cf5c7/image.png)

```sql
SELECT * FROM TableA A RIGHT JOIN TableB B ON A.col1 = B.col1 WHERE A.col1 IS NULL;
```

![](https://velog.velcdn.com/images/ekdeon/post/c8890964-0b60-4aa5-b9f4-c9acc25a8aa3/image.png)

---

### 5. INNER JOIN (A와 B의 교집합)Permalink

![](https://velog.velcdn.com/images/ekdeon/post/ee763062-d210-447d-b489-7875274e3a73/image.png)

```sql
SELECT * FROM TableA A INNER JOIN TableB B ON A.col1 = B.col1;
```

![](https://velog.velcdn.com/images/ekdeon/post/7342f40e-c998-4512-bf30-97f32d92f523/image.png)

---

### 6. FULL OUTER JOINPermalink

![](https://velog.velcdn.com/images/ekdeon/post/a2c0422a-f91b-4a69-81db-56baf9279813/image.png)


```sql
SELECT * FROM TableA A FULL OUTER JOIN TableB B ON A.col1 = B.col1;
```
 
MySQL은 FULL OUTER JOIN을 지원하지 않으므로, LEFT JOIN과 RIGHT JOIN을 UNION해서 사용해야한다.

```sql
SELECT * FROM TableA A LEFT JOIN TableB B ON A.col1 = B.col1
UNION
SELECT * FROM TableA A RIGHT JOIN TableB B ON A.col1 = B.col1;
```

![](https://velog.velcdn.com/images/ekdeon/post/c8dc4ae0-dc99-4129-985d-1c6746eb9df6/image.png)

---

### 7. FULL OUTER JOIN (교집합 제외)Permalink

![](https://velog.velcdn.com/images/ekdeon/post/00229f6a-ee5f-47a5-992d-5e1ee9dafea0/image.png)


```sql
SELECT * FROM TableA A LEFT JOIN TableB B ON A.col1 = B.col1 WHERE B.col1 IS NULL
UNION
SELECT * FROM TableA A RIGHT JOIN TableB B ON A.col1 = B.col1 WHERE A.col1 IS NULL;
```

![](https://velog.velcdn.com/images/ekdeon/post/74140918-dbd0-44b5-8e49-e2cea55f15ec/image.png)
