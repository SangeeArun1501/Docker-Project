FROM mysql:latest
# Add a database
ENV MYSQL_DATABASE=MYUsers
ENV MYSQL_ROOT_PASSWORD=root
EXPOSE 3306
# Add the content of the sql-scripts/ directory to your image
# All scripts in docker-entrypoint-initdb.d/ are automatically
# executed during container startup
COPY ./sql-scripts/ /docker-entrypoint-initdb.d/
