# java-tp6

` Package DAO : `
```java
Code de la classe DAO :
package DAO;
import java.sql.Connection;
public abstract class DAO <T> {
protected Connection cnn = null;
public DAO(Connection cn) {
this.cnn = cn;
}
public abstract boolean create(T entity);
public abstract boolean delete(T entity);
public abstract boolean update(T entity);
public abstract T find(String id);
}
```

` Package DAO : `
```java
Code de la classe DepartementDAO :
package DAO;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import entity.Etudiant;
import entity.Departement;
public class DepartementDAO extends DAO<Departement> {
public DepartementDAO(Connection cn) {
super(cn);
}
public boolean create(Departement entity) {
return false;
}
public boolean delete(Departement entity) {
return false;}
public boolean update(Departement entity) {
return false;
}
public Departement find(String id) {
return null;
}
}
```


` Code de la classe EtudiantDAO : `
```java
package DAO;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import entity.Etudiant;
import entity.Departement;
public class EtudiantDAO extends DAO<Etudiant> {
public EtudiantDAO(Connection cn) {
super(cn);
}
public boolean create (Etudiant e)
{
boolean ok= true;
try {
String reqInsert = "insert into
Etudiant(Matricule,Nom,Adresse,NDept,age) values (?,?,?,?,?)";
System.out.print(e.getMatricule());
PreparedStatement pstmt = cnn.prepareStatement(reqInsert);
pstmt.setString(1, e.getMatricule());
pstmt.setString(2, e.getNom());
pstmt.setString(3, e.getAdresse());
pstmt.setString(4, e.getNDept());
pstmt.setString(5, e.getAge());
pstmt.executeUpdate();
}
catch(SQLException ex){ok = false;
ex.printStackTrace();
System.out.println("problem inserttion " +ex.getMessage());
}
return ok;
}
public Etudiant find(String id) {
Etudiant e = new Etudiant();
try {
String select = "select * from Etudiant where Matricule = ?";
PreparedStatement pstmt;
pstmt = ((Connection) this.cnn).prepareStatement(select,
ResultSet.TYPE_SCROLL_SENSITIVE);
pstmt.setString(1, id);
ResultSet rs = pstmt.executeQuery();
if (rs.next()) {
e.setMatricule(id);
e.setNom(rs.getString(2));
DepartementDAO deptdao = new DepartementDAO(this.cnn);
e.setNDept(deptdao.find(rs.getString("NDept")).getNdept());
}
}
catch(SQLException e1) {
e1.printStackTrace();
}
return e;
}
@Override
public boolean delete(Etudiant entity) {
// TODO Auto-generated method stub
return false;
}
@Override
public boolean update(Etudiant entity) {
// TODO Auto-generated method stub
return false;}
}
```

# Package Entity :

` Code de la classe Departement :`
```java 
package entity;
import java.util.*;
public class Departement {
private String NDept,Nomdept;
private Set<Etudiant> etudiants = new HashSet<Etudiant>();
public Departement() {};
public String getNdept() {return NDept;}
public String getNomdept() {return Nomdept;}
public void setNDept(String NDept) {this.NDept= NDept;}
public void setNomdept(String Nomdept) {this.Nomdept=
Nomdept;}
public void addEtudiant(Etudiant e) {
if (e.getNDept()==null) etudiants.add(e);
}
public void removeEtudiant(Etudiant e) {etudiants.remove(e);}
}
```

`Code de la classe Etudiant : `

```java 
package entity;
public class Etudiant {
private String Matricule,Nom,Adresse,NDept,Age ;
public Etudiant(String Matricule, String Nom, String Adresse,
String
NDept , String Age){
this.Matricule=Matricule;
this.Nom=Nom;
this.Adresse=Adresse;
this.NDept=NDept;this.Age=Age;
};
public Etudiant(){};
public String getMatricule() {return Matricule;}
public String getNom() {return Nom;}
public String getAdresse() {return Adresse;}
public String getNDept() {return NDept;}
public String getAge() {return Age;}
public void setMatricule(String Matricule) {this.Matricule=
Matricule;}
public void setNom(String Nom) {this.Nom= Nom;}
public void setAdresse(String Adresse) {this.Adresse= Adresse;}
public void setNDept(String NDept) {this.NDept= NDept;}
public void setAge(String Age) {this.Age= Age;}
}
```


# Package Metier :

` Code de la classe Test :`

```java
package Metier;
import DAO.EtudiantDAO;
import entity.Etudiant;
import java.sql.Connection;
import Utils.SqlConnection;
public class Test {
public static void main(String[] args) {
Connection cn=SqlConnection.getInstanc();
Etudiant e1= new Etudiant("111","Eya","Tunis","Dept1","24");
EtudiantDAO eDAO =new EtudiantDAO(cn);
eDAO.create(e1);
}
}
```

# Package Utils : 
`Code de la classe SqlConnection:`

```java
package Utils;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
public class SqlConnection {
private static String url = "jdbc:mysql://localhost/Etudiant";
private static String user = "root";
private static String password = "";
private static Connection cn;
private static String name = "localhost/Etudiant";
private SqlConnection (){
try {
Class.forName("com.mysql.jdbc.Driver");
cn = DriverManager.getConnection(url, user, password);
}
catch (ClassNotFoundException ex) {
System.out.println("Impossible de charger le driver "
+ex.getMessage());
}
catch (SQLException ex) {
System.out.println("Erreur de connexion\n" +ex.getMessage());
)
}
public static Connection getInstanc(){
if(cn==null){
new SqlConnection();
System.out.println("Instanciation de la connection à "+ name);
}
else
System.out.println("Connection à "+ name + " existante");
return cn;
}
public static String getName() {return name;}
}
```

