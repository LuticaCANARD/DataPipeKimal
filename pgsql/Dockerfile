FROM postgres:latest
VOLUME [ "./pgsql/init/:/docker-entrypoint-initdb.d/" ]
EXPOSE 5432

CMD ["postgres"]