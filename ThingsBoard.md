# Documentation for ThingsBoard

0. Pre-requisites

   - [Set up Ubuntu Environment](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04#installing-the-oracle-jdk)

     ```bash
     sudo add-apt-repository ppa:webupd8team/java
     sudo apt-get update
     sudo apt-get install oracle-java8-installer
     ```

     

   - Set up PostgreSQL

     ```bash
     sudo apt-get update
     sudo apt-get install postgresql postgresql-contrib
     sudo service postgresql enable
     sudo service postgresql start
     ```

   - Add ThingsBoard DB to PostgreSQL

     Run this command in Terminal: `psql -U postgres -d postgres -h 127.0.0.1 -W`

     And then,

     ```sql
     CREATE DATABASE thingsboard;
     \q
     ```

   - [Also, change the username and password of the default Postgres user](https://blog.2ndquadrant.com/how-to-safely-change-the-postgres-user-password-via-psql/)

1. Download ThingsBoard

   - Run this command in Terminal :

     ```bash
     wget https://github.com/thingsboard/thingsboard/releases/download/v2.0.3/thingsboard-2.0.3.deb
     ```

2. Install ThingsBoard

   ```bash
   sudo dpkg -i thingsboard*.deb
   ```

3. Configure database in Thingsboard

   - First, enter in terminal:

   ```bash
   sudo gedit /etc/thingsboard/conf/thingsboard.yml
   ```

   - Find the block of code which says `HSQLDB DAO Configuration`

     It will look like this.

   ```yaml
   # HSQLDB DAO Configuration
   #spring:
   #  data:
   #    jpa:
   #      repositories:
   #        enabled: "true"
   #  jpa:
   #    hibernate:
   #      ddl-auto: "validate"
   #    database-platform: "org.hibernate.dialect.HSQLDialect"
   #  datasource:
   #    driverClassName: "${SPRING_DRIVER_CLASS_NAME:org.hsqldb.jdbc.JDBCDriver}"
   #    url: "${SPRING_DATASOURCE_URL:jdbc:hsqldb:file:${SQL_DATA_FOLDER:/tmp}/thingsboardDb;sql.enforce_size=false}"
   #    username: "${SPRING_DATASOURCE_USERNAME:sa}"
   #    password: "${SPRING_DATASOURCE_PASSWORD:}"
   ```

   - Make sure that it is commented by placing a # character before each line

     This ensures that the inbuilt HSQL instance is disabled.

   - Now we need to enable the PostgreSQL instance. For this, find `PostgreSQL DAO Configuration block`

     Uncomment all the lines so that it looks similar to this

     ```yaml
     # PostgreSQL DAO Configuration
     spring:
       data:
         jpa:
           repositories:
             enabled: "true"
       jpa:
         hibernate:
           ddl-auto: "validate"
         database-platform: "org.hibernate.dialect.PostgreSQLDialect"
       datasource:
         driverClassName: "${SPRING_DRIVER_CLASS_NAME:org.postgresql.Driver}"
         url: "${SPRING_DATASOURCE_URL:jdbc:postgresql://localhost:5432/thingsboard}"
         username: "${SPRING_DATASOURCE_USERNAME:postgres}"
         password: "${SPRING_DATASOURCE_PASSWORD:postgres}"
     ```

     ### NOTE Remember to change the password of the `postgres` user to the one that you changed to in Step 0

4. Finally, we are ready to Install thingsboard:

   ```bash
   # --loadDemo option will load demo data: users, devices, assets, rules, widgets.
   sudo /usr/share/thingsboard/bin/install/install.sh --loadDemo
   ```

   ##### Disable the loadDemo option if you do not want the demo data!

5. It is ready! Start the ThingsBoard service!

   ```bash
   sudo service thingsboard start
   ```

   ##### If you want ThingsBoard to start automatically upon a reboot, you must enable it:

   ```bash
   sudo service thingsboard enable
   ```





Congratulations! You have successfully set up the ThingsBoard server. You can now see the web ui on (within 5 minutes)

`http://localhost:8080`