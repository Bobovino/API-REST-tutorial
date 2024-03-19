Redis EC2
redis-cli -h cache-5head-001.roqodk.0001.euw3.cache.amazonaws.com -p 6379

keys *

ELIMINAR TODAS LAS CLAVES CON CLAVE 'region' redis-cli -h cache-5head-001.roqodk.0001.euw3.cache.amazonaws.com -p 6379 --scan --pattern 'match:*' | xargs -I {} redis-cli -h cache-5head-001.roqodk.0001.euw3.cache.amazonaws.com -p 6379 del {}

RDS EC2 Psql
psql -h db-5head-instance.cfgp2upiw5hg.eu-west-3.rds.amazonaws.com -U postgres -d db_5head \c "db_5head"

psql --host=db-5head-instance.cfgp2upiw5hg.eu-west-3.rds.amazonaws.com --port=5432 --username=postgres --password --dbname=db_5head

ECR private
aws ecr get-login-password --region eu-west-3 | docker login --username AWS --password-stdin 430180859042.dkr.ecr.eu-west-3.amazonaws.com/5head

docker build -t 430180859042.dkr.ecr.eu-west-3.amazonaws.com/5head:latest . docker push 430180859042.dkr.ecr.eu-west-3.amazonaws.com/5head:latest

docker run --name schedule-container --network="host" --env-file .env 430180859042.dkr.ecr.eu-west-3.amazonaws.com/5head:latest docker run --env-file .env --platform linux/amd64 -p 9000:8080 430180859042.dkr.ecr.eu-west-3.amazonaws.com/5head:latest --network="host" docker ps docker logs a8b297dfa40f

local redis
redis-server redis-cli

nohup redis-server &

Parar: redis-cli shutdown

Local Psql
sudo service postgresql start sudo service postgresql status

psql -U postgres -d db-5head-local -h localhost -W CREATE DATABASE "db-5head-local";

\l listar bases de datos \c "db-5head-local" \dt listar tablas

SELECT * FROM MatchSchedule ORDER BY matchid ASC ;

sudo -u postgres psql sudo -u user_name psql db_name psql -d database_name -U user_name -h host_name -p port_number ALTER USER user_name WITH PASSWORD 'new_password';

DELETE FROM Tournaments; TRUNCATE TABLE my_table;

CREATE TABLE regions ( regionlong VARCHAR(255) PRIMARY KEY , regionmedium VARCHAR(255), IsCurrent boolean );

CREATE TABLE leagues ( event VARCHAR(255) PRIMARY KEY , OverviewPage VARCHAR(255) *FK );

CREATE TABLE Tournaments ( Name VARCHAR(255) PRIMARY KEY, DateStart TIMESTAMP WITHOUT TIME ZONE, Date TIMESTAMP WITHOUT TIME ZONE, EventType VARCHAR(255), TournamentLevel VARCHAR(255), OverviewPage VARCHAR(255), *PK Rulebook TEXT, Region VARCHAR(255) *FK );

CREATE TABLE TournamentRosters ( Team VARCHAR(255) PRIMARY KEY, Roles TEXT, RosterLinks TEXT, OverviewPage VARCHAR(255) *FK );

CREATE TABLE MatchSchedule ( MatchId VARCHAR(255) PRIMARY KEY, DateTime_UTC TIMESTAMP WITHOUT TIME ZONE, Team1 VARCHAR(255), Team2 VARCHAR(255), Round VARCHAR(255), Phase VARCHAR(255), BestOf INTEGER, Patch VARCHAR(255), DisabledChampions TEXT, OverviewPage TEXT, *FK IsTiebreaker Boolean, is_new Boolean );

CREATE TABLE MatchScheduleGame ( OverviewPage VARCHAR(255), GameId VARCHAR(255) PRIMARY KEY, Selection TEXT, HasSelection Boolean, Blue VARCHAR(255), Red VARCHAR(255), Winner VARCHAR(255), MatchId VARCHAR(255), *FK IsRemake Boolean , WinnerLoggedAt TIMESTAMP WITHOUT TIME ZONE );

CREATE TABLE PicksAndBansS7 ( GameId VARCHAR(255) PRIMARY KEY, Team1 VARCHAR(255), Team2 VARCHAR(255), Winner VARCHAR(255), Team1Ban1 VARCHAR(255), Team1Ban2 VARCHAR(255), Team1Ban3 VARCHAR(255), Team1Ban4 VARCHAR(255), Team1Ban5 VARCHAR(255), Team1Pick1 VARCHAR(255), Team1Pick2 VARCHAR(255), Team1Pick3 VARCHAR(255), Team1Pick4 VARCHAR(255), Team1Pick5 VARCHAR(255), Team2Ban1 VARCHAR(255), Team2Ban2 VARCHAR(255), Team2Ban3 VARCHAR(255), Team2Ban4 VARCHAR(255), Team2Ban5 VARCHAR(255), Team2Pick1 VARCHAR(255), Team2Pick2 VARCHAR(255), Team2Pick3 VARCHAR(255), Team2Pick4 VARCHAR(255), Team2Pick5 VARCHAR(255), Team1PicksByRoleOrder TEXT, Team2PicksByRoleOrder TEXT, MatchId VARCHAR(255), *FK OverviewPage VARCHAR(255) *FK );

-- Assuming you're adding the Region column to reference regions.regionlong ALTER TABLE matchschedule ADD COLUMN istiebreaker VARCHAR(255);

ALTER TABLE Tournaments ADD CONSTRAINT fk_region FOREIGN KEY (Region) REFERENCES regions(regionlong);

{"targetLambda": "functionName"}

Tagging resources
aws lambda tag-resource --resource arn:aws:lambda:eu-west-3:430180859042:function:Regions --tags '{"Group":"poll_esports"}'

Changing AWS Profiles
export AWS_PROFILE=aws1

ECR public
aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/m0h2p7a3 docker build -t 5head . docker tag 5head:latest public.ecr.aws/m0h2p7a3/5head:latest docker push public.ecr.aws/m0h2p7a3/5head:latest

Build function
sam init sam build

Invoke function
sam local invoke -e events/event.json --env-vars env.json

Deploy function
sam deploy

Packaging the layer
pip install -r requirements.txt -t ./python/ cd python $ zip -r ../polling-layer.zip .