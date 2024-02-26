### PostgreSql
```
brew services start postgresql@15
```
Скачиваем PostgreSql, открываем shell от sql и создаем юзера, создаем бд. Делаем владельцем бд нашего юзера:
```sql
postgres=# CREATE USER books_user WITH PASSWORD ‘password’
postgres=# CREATE DATABASE books_db WITH OWNER books_user
```
Зайти с напрямую с консоли 
```bash
sudo su - postgres
```
