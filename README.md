## <h1>Hadoop : Logiciel Sqoop</h1>
***
<table><tr>
  <td><img src="https://github.com/user-attachments/assets/62ab5754-1b98-4a7f-9059-9f434548bea7" alt="drawing" height="240px"/></td>
</tr></table>

## Informations Générales
***
Le logiciel Sqoop fait partie de la Galaxie du BigData Hadoop.
Ce logiciel permet de pouvoir exporter et importer des données entre le système de fichier HDFS et n'importe quelle base de données

## Docker
***
Image Docker:<br/>
*docker pull ghcr.io/kasipavankumar/sqoop-docker:latest*<br/>

Run Image Docker:<br/>
*docker run -it ghcr.io/kasipavankumar/sqoop-docker:lates*<br/>

Cette image permet de deployer :
* Une Infrastructure Apache Hadoop avec son système de fichier
* Une base de données Mysql ainsi que ses données préremplit

* Java 17
## Instalation
***
Lancement de l'application Quarkus<br>
```
$ mvn  clean
$ mvn quarkus:dev
```
Le service est accessible sur http://localhost:8080

## FAQs
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



