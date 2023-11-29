* db design tool: https://dbdiagram.io/d/GolangBankBackend-65632b773be1495787bf3fa2

* Udemy course: https://www.udemy.com/course/backend-master-class-golang-postgresql-kubernetes/learn/lecture/25822274#questions

* source: https://github.com/techschool/simplebank

* wsl ubuntu account: anpc - anpcpw

    a. Install make: sudo apt update && sudo apt install make && make --version

    b. Install golang: sudo snap install go --classic

    c. Install sqlc: sudo snap install sqlc

*** INSTALL DOCKER ***

    a. On windows, don't install it on ProgramFiles folder

    b. Download it, and then inside that download folder, run powershell with admin right -> run: Start-Process "Docker Desktop Installer.exe" -Verb RunAs -Wait -ArgumentList "install --installation-dir=C:\Docker\"

* View docker logs: docker logs <container name or id>

* Install postgres on docker: docker pull postgres:12-alpine <light weight version>

    a. Read about postgres environment variable in Docker: https://hub.docker.com/_/postgres

    b. run docker container from pulled image: docker run --name postgres12 -p 5432:5432 -e POSTGRES_USER=root -e POSTGRES_PASSWORD=secret -d postgres:12-alpine

* Access Docker console to use postgres db

    a. docker exec -it postgres12 psql -U root

*** Table plus ***

* connection to postgresSQL:

    1. name: postgres12
    
    2. host: localhost - port 5432

    3. user: root - pw:secret - database: root
    
*** DB migration ***

* CLI docs: https://github.com/golang-migrate/migrate/tree/master/cmd/migrate

* using wsl as root: wsl -u root

* Install:

        $ curl -L https://packagecloud.io/golang-migrate/migrate/gpgkey | apt-key add -

        $ echo "deb https://packagecloud.io/golang-migrate/migrate/ubuntu/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/migrate.list

        $ apt-get update

        $ apt-get install -y migrate

* Create migration file to init db schema: migrate create -ext sql -dir db/migration -seq init_schema

* Create new db inside docker

    1. Enter the shell: docker exec -it postgres12 /bin/bash

    2. Tạo db: createdb --username=root --owner=root simple_bank

    3. Access db: psql simple_bank

    4. Drop db: dropdb simple_bank (Nhớ exit khỏi db)

    => docker exec -it postgres12 createdb --username=root --owner=root simple_bank (directed db creation from docker)

    => docker exec -it postgres12 psql -U root simple_bank (directly access db from docker)

* Remove docker container: docker rm <name or id>

* Tạo file make để automation:

    1. Chạy lệnh: make <tên alias được để trong file> - vd: make postgres

* Chạy migration đã có: migrate -path db/migration -database "postgresql://root:secret@localhost:5432/simple_bank?sslmode=disable" -verbose up

=> trong db được tạo có bảng schema_migrations chứa phiên bản migration gấn nhất

