# Build Docker Image
cd /mnt/c/Users/User/Desktop/Create.with.Code/Kubernetes/gowebapp
docker build -t gowebapp:v1 .

# Build MySQL Docker Image
docker build -t gowebapp-mysql:v1 .

# Build Network
docker network create gowebapp

# Run Systems
docker run --net gowebapp --name gowebapp-mysql --hostname gowebapp-mysql -d -e MYSQL_ROOT_PASSWORD=mypassword gowebapp-mysql:v1

sleep 20

docker run -p 8080:8080 --net gowebapp -d --name gowebapp --hostname gowebapp gowebapp:v1

# Find URL
echo "http://$SESSION_NAME-gowebapp-docker.$INGRESS_DOMAIN"

# Login to MySQL
docker exec -it gowebapp-mysql mysql -u root -pmypassword gowebapp

# MySQL commands
SHOW DATABASES;
USE gowebapp;
SHOW TABLES;
SELECT * FROM <table_name>;
exit;

# Clean Up
docker rm -f gowebapp gowebapp-mysql

# Tag Images
docker tag gowebapp:v1 matthiasf1271/gowebapp:v1
docker tag gowebapp-mysql:v1 matthiasf1271/gowebapp-mysql:v1

# Publish Images to the registry
docker push matthiasf1271/gowebapp:v1
docker push matthiasf1271/gowebapp-mysql:v1