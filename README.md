
# Exemple de mappage de relation de clé étrangère JPA One-To-One avec Spring Boot, Spring Data JPA et MySQL
---
## 1.	Objectif

Créer un exemple de mappage d'une relation de clé étrangère one-to-one  avec Spring Boot, Spring Data JPA et MySQL.

### Outils :

•	JDK 1.8 or later

•	Maven 3 ou plus

•	MySQL 5.6 ou plus

## 2.	Structure du projet

![44](https://user-images.githubusercontent.com/28655112/41090331-ded9f8dc-6a3b-11e8-8d28-10c3b68ddf6d.PNG)

## 3.	Configuration de projet

### 3.1.	Dépendances de projet
```bash
<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```

### 3.2.	Propriétés de l'application

 application.properties :
 
 ```bash
spring.datasource.url=jdbc:mysql://localhost/jpa_onetoone_foreignkey
spring.datasource.username=
spring.datasource.password=
spring.datasource.driver-class-name=com.mysql.jdbc.Driver

spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
```

## 4.	Relation One-To-One:

Les tables book et book_detail ont une relation one-to-one via book.book_detail_id et book_detail.id.
 
### Définir les entités JPA:

L'entité JPA est définie avec l’annotation @Entity, représente une table dans la base de données. Elle signifie que la classe doit être gérée par la couche de persistance.

Dans la classe Book, on met le code suivant :
```bash
@OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "book_detail_id")
    public BookDetail getBookDetail() {
        return bookDetail;
    }
 ```
Dans la classe BookDetail, on met le code suivant :
```bash
@OneToOne(mappedBy = "bookDetail")
    public Book getBook() {
        return book;
    }
 ```
### Description des annotations :

•	@Table désigne la table utilisée en base pour représenter cet objet. Dans notre cas, cette annotation est facultative car par défaut la table prend le nom de la classe.

•	@Id indique que le champ est la clé primaire de la table.

•	@Column crée le lien entre la représentation du champ en base et sa déclaration dans le code. Plusieurs arguments, tous optionnels, sont paramétrables à travers cette annotation comme: name, length, nullable, unique…

•	@GeneratedValue avec son argument « strategy » apporte des informations sur la manière de gérer la clé primaire. Ici, elle est gérée de manière automatique par le SGBDR. L’attribut strategy peut prendre plusieurs valeurs :

  	Strategy = GenerationType.AUTO : La génération de la clé primaire est laissée à l’implémentation.  
  
  	Strategy = GenerationType.IDENTITY : La génération de la clé primaire se fera à partir d’une Identité propre au SGBD. Il utilise un type de colonne spéciale à la base de données. Exemple pour MySQL, il s’agit d’un AUTO_INCREMENT.
  
  	Strategy = GenerationType.TABLE : La génération de la clé primaire se fera en utilisant une table dédiée hibernate_sequence qui stocke les noms et les valeurs des séquences.
  
  	Strategy = GenerationType.SEQUENCE : La génération de la clé primaire se fera par une séquence définie dans le SGBD, auquel on ajoutera l’attribut generator. Cette stratégie doit être utilisée avec une autre annotation qui est @SequenceGenerator.

•	@OneToOne : Dans l’exemple, l’annotation « @OneToOne » est utilisée pour modéliser la relation entre la table « book » et la table associée « bookDetail » (un book peut avoir un et un seul detailBook « 1-1 »).
Dans notre exemple, on  a utilisé dans l’annotation «@OneToOne»  l’attribut «Cascade». La valeur « CascadeType.ALL » indique à l’ORM que toutes les opérations (PERSIST, MERGE, REMOVE, REFRESH) effectuées sur l’objet Book sont implicitement effectuées sur les entités qui lui sont associées.. Ainsi, lors de la suppression d’un book, l’ORM supprimera en cascade toutes les commandes rattachées à ce dernier.

•	@JoinColumn indique que l'entité est le propriétaire de la relation: la table correspondante a une colonne avec une clé étrangère à la table référencée. 

mappedBy represente la relation inverse.

## 5.	 Spring Data JPA Repository

Spring Data JPA contient certains intégré Repository mis en œuvre certaines fonctions communes à travailler avec la base de données: findOne, findAll, save…

``` public interface BookRepository extends JpaRepository<Book, Integer>{ } ```

## 6.	Exécuter l'application

 ![11](https://user-images.githubusercontent.com/28655112/41090618-980280b8-6a3c-11e8-9842-cfc0ee9657ef.PNG)
 
On obtient les tables suivantes :
   
![22](https://user-images.githubusercontent.com/28655112/41090664-ad60f7dc-6a3c-11e8-959b-1c47be67fee4.PNG)
![33](https://user-images.githubusercontent.com/28655112/41090670-b1790148-6a3c-11e8-8905-a36655198aa4.PNG)
