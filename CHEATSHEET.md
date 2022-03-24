docker create network -d bridge openhack


docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=_YourStrong@Passw0rd" \
   -p 1433:1433 --name sql1 --hostname sql1 \
   --network openhack \
   -d \
   mcr.microsoft.com/mssql/server:2017-latest

sudo docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd \
-S localhost -U SA \
-P _YourStrong@Passw0rd \
-Q "CREATE DATABASE mydrivingDB"


docker run --network openhack -e SQLFQDN=sql1 -e SQLUSER=sa -e SQLPASS=_YourStrong@Passw0rd -e SQLDB=mydrivingDB registryuxy0369.azurecr.io/dataload:1.0

docker run --network openhack -ti -e ASPNETCORE_ENVIRONMENT=Local -e SQL_PASSWORD=_YourStrong@Passw0rd -e SQL_USER=SA -e SQL_SERVER=sql1 -p 8080:80 openhack-poi:local 