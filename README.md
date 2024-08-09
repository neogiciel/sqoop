## <h1>Hadoop : Logiciel Sqoop</h1>
***
<table><tr>
  <td><img src="https://github.com/user-attachments/assets/62ab5754-1b98-4a7f-9059-9f434548bea7" alt="drawing" height="240px"/></td>
</tr></table>

## Informations Générales
***
Le logiciel Sqoop fait partie de la Galaxie du BigData Hadoop.
Ce logiciel permet de pouvoir exporter et importer des données entre le système de fichier HDFS et n'importe quelle base de données

## Déploiement Docker
***
Image Docker:<br/>
```
docker pull ghcr.io/kasipavankumar/sqoop-docker:latest
```
Run Image Docker:<br/>
```
docker run -it ghcr.io/kasipavankumar/sqoop-docker:latest
```
Cette image permet de deployer :
* Une Infrastructure Apache Hadoop avec son système de fichier
* Une base de données Mysql ainsi que les données associées

## Import
***
Exemple d'exportation de la base de données Mysql vers HDFS de Hadoop
```
Exportation du fichier vers hdfs
sqoop import 
    --connect jdbc:mysql://localhost/employees 
    --table employees 
    --username bda 
    --password 123456
```
## Import avec création de Base de données
***
Exemple d'exportation de la base de données Mysql vers HDFS de Hadoop
```
#Creation des données dans la base
CREATE DATABASE IF NOT EXISTS test;

GRANT CREATE, ALTER, INDEX, LOCK TABLES, REFERENCES, UPDATE, DELETE, DROP, SELECT, INSERT ON `test`.* TO 'bda'@'localhost';
FLUSH PRIVILEGES;

CREATE TABLE client (
 id INT PRIMARY KEY NOT NULL AUTO_INCREMENT,
  nom VARCHAR(100),
  email VARCHAR(100)
);
INSERT INTO `client` (`nom`, `email`) VALUES ('Paul', 'paul@example.com');
INSERT INTO `client` (`nom`, `email`) VALUES ('Sandra', 'sandra@example.com');
```
importation vers hdfs
```
sqoop import --connect jdbc:mysql://localhost/test
	     --table client
	     --username bda
              --password 123456
```
Test de présence du fichier dans HDFS
```
hadoop fs -ls /user/root/client
```
Afficher les données
```
hadoop fs -cat /user/root/client/part-m-00000
1,Paul,paul@example.com
hadoop fs -cat /user/root/client/part-m-00001
2,Sandra,sandra@example.com
```
## Exemple d'Export
***
**Exportation d un fichier directement dans une base de données**
Copier le fichier sur le pod
```
docker cp people.txt agitated_goldstine:people.txt
```
Créer un dossier dans hdfs
```
hdfs dfs -mkdir -p /fichier
```
Pousser le fichier dans le cluster Hdfs
```
hdfs dfs -put people.txt /fichier/people.txt
```
Dans mysql créer la base de données ainsi que la table associé
```
CREATE DATABASE IF NOT EXISTS people;
GRANT CREATE, ALTER, INDEX, LOCK TABLES, REFERENCES, UPDATE, DELETE, DROP, SELECT, INSERT ON `people`.* TO 'bda'@'localhost';
CREATE TABLE people (
  prenom VARCHAR(100),
  age VARCHAR(100)
);
```
Faire un export du foicher vers la base de données
```
sqoop export --connect jdbc:mysql://localhost/people
	     --username bda
             --password 123456
             --table people
             --export-dir /fichier/people.txt
```
