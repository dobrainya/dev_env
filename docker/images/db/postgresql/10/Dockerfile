# Для начала указываем исходный образ, он будет использован как основа
FROM postgres:10.6

RUN apt-get update && apt-get install -y \
        nano \
        mc

EXPOSE 5432
CMD ["postgres"]