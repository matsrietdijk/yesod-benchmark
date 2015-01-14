# Database

```
echo " \
  create database \"yesod-benchmark\"; \
  create user \"yesod-benchmark\" with password 'yesod-benchmark'; \
  grant all on database \"yesod-benchmark\" to \"yesod-benchmark\";" \
  | psql
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
