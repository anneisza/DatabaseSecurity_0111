Masih bisa diinjeksi (file "web01_terinjeksi.py")
1. Tautology Attack
Login tanpa password valid, input di form login:
- Username : ' OR '1'='1' --
- Password : _(dikosongkan)_
  <img width="1919" height="1019" alt="image" src="https://github.com/user-attachments/assets/f9733e8d-3968-4908-a657-69eaa75cd6c0" />
  <img width="1919" height="1020" alt="image" src="https://github.com/user-attachments/assets/aea77c28-b79a-4fb2-99f9-28e7d2c29128" />

2. Piggyback Attack
akses URL search dengan payload ini: http://localhost:5000/search?keyword=%25' UNION SELECT id, username, password FROM user--
 <img width="1919" height="1020" alt="image" src="https://github.com/user-attachments/assets/1fc75ba0-42a1-41f4-b09e-21153944febf" />


---

Perubahan Code untuk Mencegah SQL Injection

1. fungsi authenticate()

   Sebelum:
```python
   query = ('SELECT id, username FROM user WHERE username=\'%s\' AND password=\'%s\'' % (username, password))
   cur.execute(query)
```
   Sesudah:
```python
   query = "SELECT id, username FROM user WHERE username= ? AND password= ?"
   cur.execute(query, (username, password))
```

2. fungsi delete_time_line()

   Sebelum:
```python
   query = f"DELETE FROM time_line WHERE user_id={uid} AND id={tid}"
   cur.execute(query)
```
   Sesudah:
```python
   query = "DELETE FROM time_line WHERE user_id=? AND id=?"
   cur.execute(query, (uid, tid))
```

3. fungsi search()

   Sebelum:
```python
   query = f"SELECT id, user_id, content FROM time_line WHERE content LIKE '%{keyword}%'"
   cur.execute(query)
```
   Sesudah:
```python
   query = "SELECT id, user_id, content FROM time_line WHERE content LIKE ?"
   cur.execute(query, (f"%{keyword}%",))
```

---
Setelah diganti agar mencegah dari injeksi (file "web01.py")
1. Tautology Attack
Login tanpa password valid, input di form login:
- Username : ' OR '1'='1' --
- Password : _(dikosongkan)_\
  <img width="1919" height="1020" alt="image" src="https://github.com/user-attachments/assets/4ee4ecf6-018a-4405-918a-edc007fdc816" />
  <img width="1919" height="1020" alt="image" src="https://github.com/user-attachments/assets/331b2066-962b-4532-9af9-d564d3439fd8" />

2. Piggyback Attack
akses URL search dengan payload ini: http://localhost:5000/search?keyword=%25' UNION SELECT id, username, password FROM user--
 <img width="1919" height="1015" alt="image" src="https://github.com/user-attachments/assets/ff92b912-acc6-4526-b810-ea12a93d69ab" />
