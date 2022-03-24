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


## Secrets

```sh

# poi-db-user-pass
kubectl -n api create secret generic poi-db-user-pass \
  --from-literal=username=sqladminuXy0369 \
  --from-literal=password='_YourStrong@Passw0rd'

# userprofile-db-user-pass
kubectl -n api create secret generic userprofile-db-user-pass \
  --from-literal=username=sqladminuXy0369 \
  --from-literal=password='_YourStrong@Passw0rd'

# user-java-db-user-pass
kubectl -n api create secret generic user-java-db-user-pass \
  --from-literal=username=sqladminuXy0369 \
  --from-literal=password='_YourStrong@Passw0rd'

# tripviewer-db-user-pass
kubectl -n web create secret generic tripviewer-db-user-pass \
  --from-literal=username=sqladminuXy0369 \
  --from-literal=password='_YourStrong@Passw0rd'

# trips-db-user-pass
kubectl -n api create secret generic trips-db-user-pass \
  --from-literal=username=sqladminuXy0369 \
  --from-literal=password='_YourStrong@Passw0rd'

kubectl get secret poi-db-user-pass -o jsonpath='{.data}'
kubectl get secret userprofile-db-user-pass -o jsonpath='{.data}'
kubectl get secret user-java-db-user-pass -o jsonpath='{.data}'
kubectl get secret tripviewer-db-user-pass -o jsonpath='{.data}'
kubectl get secret trips-db-user-pass -o jsonpath='{.data}'


kubectl delete secret poi-db-user-pass userprofile-db-user-pass user-java-db-user-pass tripviewer-db-user-pass trips-db-user-pass

```


# ingresss
kubectl create ns ingress
helm -n ingress install ingress-nginx ingress-nginx/ingress-nginx 
