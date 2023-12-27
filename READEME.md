services:
 postgres;
   image:postgres:16.1
   environment:
     POSTGRES_USER: airflow
     POSTGRES_PASSWORD: airflow
     POSTGRES_DB: airflow
   volume:
     - postgres-db-volume:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "airflow"]
      interval: 5s
      retries; 5
    restart: always


    winpty docker run -it \
   -e POSTGRES_USER="root" \
   -e POSTGRES_PASSWORD="root" \
   -e POSTGRES_DB="ny_taxi" \
   -v /"$(pwd)"/ny_taxi_postgres_data:/var/lib/postgresql/data \
   -p 5431:5432 \
postgres:16.1


https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page



winpty docker run -it \
   -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
   -e PGADMIN_DEFAULT_PASSWORD="root" \
   -p 8080:80 \
  dpage/pgadmin4



  # Network
  docker network create pg-network


    winpty docker run -it \
   -e POSTGRES_USER="root" \
   -e POSTGRES_PASSWORD="root" \
   -e POSTGRES_DB="ny_taxi" \
   -v /"$(pwd)"/ny_taxi_postgres_data:/var/lib/postgresql/data \
   -p 5431:5432 \
   --network=pg-network \
   --name pg-database \
postgres:16.1


winpty docker run -it \
   -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
   -e PGADMIN_DEFAULT_PASSWORD="root" \
   -p 8080:80 \
   --network=pg-network \
   --name pgadmin \
  dpage/pgadmin4




# FOR URL


URL="https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-05.csv.gz"

python ingest_data.py \
  --user=root \
  --password=root \
  --host=localhost \
  --port=5432 \
  --db=ny_taxi \
  --table_name=yellow_taxi_trips \
  --url=${URL}



# FOR LOCAL PATH


docker build -t taxi_ingest:v001 . 



python ingest_data.py \
  --user=root \
  --password=root \
  --host=localhost \
  --port=5431 \
  --db=ny_taxi \ 
  --table_name=yellow_taxi_trips 

docker build -t taxi_ingest:v001 . 

  
winpty docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pg-database \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips 
    








