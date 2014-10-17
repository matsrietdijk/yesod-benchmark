# Database

```
echo " \
  create database yesod_benchmark; \
  grant all privileges on yesod_benchmark.* to \
  yesod_benchmark@localhost identified by 'yesod_benchmark'; " \
|  mysql -u root -p
```

## Test data

```
lorem=$(curl http://www.loripsum.net/api/4)
for i in {1..20}; do \
  echo "\
    insert into yesod_benchmark.employee (title, content, firstname, lastname) \
    values ('Employee $i', '$lorem', 'Voornaam', 'Achternaam');"
  done | mysql -uyesod_benchmark -p

lorem=$(curl http://www.loripsum.net/api/6)
for i in {1..15}; do \
  echo "\
    insert into yesod_benchmark.project (title, content, url, customer) \
    values ('Project $i', '$lorem', 'http://dummy.url', 'Klant');"
  done | mysql -uyesod_benchmark -p
```
