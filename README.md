create pod with using podman 

$ sudo podman pod create --name my-pod -p 8080:80

now write connect.php file for testing the connectivity and put this file in dir to mount with the Dockerfile in phpmyadmin container to access it.

<?php
$servername = "127.0.0.1" ;
$username = "root";
$password = "";

// Create connection
$conn = mysqli_connect($servername, $username, $password);

// Check connection
if (!$conn) {
  die("Connection failed: " . mysqli_connect_error());
}
echo "Connected successfully";
?>


now run mysql container with using Pod and give environment vairables.

$ sudo podman run -d --restart=always --pod=my-pod -e MYSQL_DATABASE="net_banking"  -e MYSQL_ALLOW_EMPTY_PASSWORD=yes --name=con-db mysql:5.7

use Dockerfile to build the image for phpmyadmin with copy connect.php file which is above created.

$ sudo podman build -t con-web .

now run container with using image build by Dockerfile

$ sudo podman run -d --restart=always --pod=my-pod   -e PMA_HOST="127.0.0.1" --name con-web localhost/con-web


now access it from localhost with 8080 port.



""" for help view above video pod.mp4 """"

task/ ------|
            |-->Dockerfile
            |-->connect.php




