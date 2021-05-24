# Для начала указываем исходный образ, он будет использован как основа
FROM postgres:10.6

RUN apt-get update && apt-get install -y \
        nano \
        mc
COPY init-db.sql docker-entrypoint-initdb.d/

#EXPOSE 5432

CMD ["postgres"]
