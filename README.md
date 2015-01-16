# Setup

## Create db

```
echo " \
  create database \"yesod-benchmark\"; \
  create user \"yesod-benchmark\" with password 'yesod-benchmark'; \
  grant all on database \"yesod-benchmark\" to \"yesod-benchmark\";" \
  | psql
```

## Install yesod app

run the following command:

```fish
cabal install -j --enable-tests --max-backjumps=-1 --reorder-goals; and yesod devel
```

## Generate dummy content

```fish
for l in 'en' 'nl'; \
    set lorem (curl http://www.loripsum.net/api/4); \
    for i in (seq 20); \
        # psql 'yesod-benchmark' -c "insert into employee (ident, title, content, firstname, lastname, lang) \
        #values ('$i', 'Employee $i', '$lorem', 'Voornaam', 'Achternaam', '$l');"; \
        echo "insert into yesod_benchmark.employee (ident, title, content, firstname, lastname, lang) \
        values ('$i', 'Employee $i', '$lorem', 'Voornaam', 'Achternaam', '$l');" \
        | mysql -uyesod_benchmark -pyesod_benchmark; \
    end; \
    set lorem (curl http://www.loripsum.net/api/6); \
    for i in (seq 15); \
        #psql 'yesod-benchmark' -c "insert into project (ident, title, content, url, customer, lang) \
        #values ('$i', 'Project $i', '$lorem', 'http://dummy.url', 'Klant', '$l');"; \
        echo "insert into yesod_benchmark.project (ident, title, content, url, customer, lang) \
        values ('$i', 'Project $i', '$lorem', 'http://dummy.url', 'Klant', '$l');" \
        | mysql -uyesod_benchmark -pyesod_benchmark; \
    end; \
end
```
