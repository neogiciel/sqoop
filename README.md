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
**Manager d'appel à la Base de données**

L'appel au requetes se fait directement via EntityManager.
Nous avons centralisé les requêtes dans un Manager qui permets de gérer toutes les table de notre base de données.
Ce choix délibéré de ne pas eclater les différents appels dans différentes classe s'inspire directement de mon experience s'avere bien plus productif à long terme.

Cette solution est un véritable atout en terme d ajout, de modification, d'optimisation des requêtes SQL. 
Sachant que dans un micro service les base de données sont souvent éclatés et le nombre de table assez réduite.

```
public interface BddManager {
    
     /*
     * Table Personne
     */
    public List<Personne> getListPersonnes();
    public List<Personne> getListPersonnesSQL();
    public Long addPersonne(Personne personne);
    public Personne getPersonneFromId(int Id);
    public void deletePersonne(int id);
	  public void updatePersonne(Personne personne);
	
    /*
     * Table Service
     */
    public List<Service> getListServices();
    public Service getServiceFromId(int Id);
    public Long addService(Service service);
    public void deleteService(int id);
    public void updateService(Service service);

    /*
     * Table Service Persoone
     */
    public List<ServicePersonne> getListPersonneFromServices(int id);

}
```



