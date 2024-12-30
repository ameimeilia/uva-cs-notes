---
layout: default
title: Hibernate
parent: Final Notes
nav_order: 9
---
Example Repository: [https://github.com/sde-coursepack/HibernateExample](https://github.com/sde-coursepack/HibernateExample)
### Entity Classes
- classes whose data we want to store in our database.  
- An entity class should look something like [State.java](https://github.com/sde-coursepack/HibernateExamples/blob/master/src/main/java/edu/virginia/cs/State.java) :

```Java
@Entity
@Table(name = "STATES")
public class State {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column (name = "ID")
    private int id;

    @Column(name="STATE_NAME", unique = true, nullable = false)
    private String name;

    @Column(name = "POPULATION", nullable = false)
    private int population;

    @Column(name = "CAPITOL_CITY", nullable = false)
    private String capitolCity;    
    
    ...
```

- Each field has a column name
- primary key has the `@Id` annotation and `@GenerateValue` annotation (similar to `AUTO_INCREMENT` in SQLite)
- class also has:
	1. Getters and setters for every field that you want to store in the table
	2. A zero-argument constructor
#### Hibernate Configuration
- entity class is connected to hibernate in the [hibernate.cfg.xml](https://github.com/sde-coursepack/HibernateExamples/blob/master/src/main/resources/hibernate.cfg.xml) file in `src/main/resources`

```Java
<hibernate-configuration>
    <session-factory>
        <property name="show_sql">false</property>
        <property name="format_sql">false</property>
        <property name="dialect">org.hibernate.community.dialect.SQLiteDialect</property>
        <property name="connection.driver_class">org.sqlite.JDBC</property>
        // important!
        <property name="connection.url">jdbc:sqlite:my_db.sqlite3</property>

        <property name="hibernate.hbm2ddl.auto" >update</property>

		// important!
        <mapping class="edu.virginia.sde.hibernate.State" />
        <mapping class="edu.virginia.sde.hibernate.Client" />
        <mapping class="edu.virginia.sde.hibernate.Account" />
        <mapping class="edu.virginia.sde.hibernate.Transaction" />
    </session-factory>
</hibernate-configuration>
```
1. `<property name="connection.url">jdbc:sqlite:my_db.sqlite3</property>`
	- specifies where the database is
2. mapping portion
	- list all classes that are **entity** classes
#### Simple Persist Example
- [State.java](https://github.com/sde-coursepack/HibernateExamples/blob/master/src/main/java/edu/virginia/cs/State.java) is tied to the Database Table STATES:

```Java
@Entity
@Table(name = "STATES")
public class State { … }
```

- simple example of adding a State to the STATES table: [StateDemo.java](https://github.com/sde-coursepack/HibernateExamples/blob/master/src/main/java/edu/virginia/cs/StateDemo.java)

**Getting and using a hibernate session**

```Java
var session = HibernateUtil.getSessionFactory().openSession();
session.beginTransaction();

State virginia = new State("Virginia", 8700000, "Richmond");
session.persist(virginia);
session.getTransaction().commit();
```

- Session is similar to JDBC's Connection class
- use Session object to perform queries on the database

- the following code:

```Java
State virginia = new State("Virginia", 8700000, "Richmond");
session.persist(virginia);
session.getTransaction().commit();
```

1. Creates the object for the State of Virginia
2. Adds that object to the database (persist)
3. Commits to transaction (saves permanently)
	- Unlike JDBC, in Hibernate auto-commit is false by default
	- Be aware that if you disconnect without committing, no changes will be made

- add objects to the database without any SQL
- code is no longer tied to one specific database implementation → can change the database/type of database in the configuration without changing any code
#### Single Get Example
- Getting a single object from its ID/primary key - typically an int:

```Java
// returns null if object not found -> do null check
State maryland = session.get(State.class, 1);
System.out.println(maryland);
```

#### Get All Example
- Getting all objects of a particular class from the database:

```Java
String hql = "from State"; //class name, not Table name!
Query<State> stateQuery = session.createQuery(hql);
List<State> stateList = stateQuery.getResultList();
```

#### HQL Query Example
- HQL stands for Hibernate Query Language:

```Java
String hql = "SELECT e FROM State e WHERE e.capitolCity = :name";
 TypedQuery<State> capitolQuery = StateDemo.session.createQuery(hql, State.class);
 capitolQuery.setParameter("name", capitolCity);
 State state = capitolQuery.getSingleResult();
```

- HQL uses Class and Field names instead of Table and Column names.
- `:name` is an example of a query parameter: specify, at runtime, the value of the query
- allow us to define a constant as a query, but then set the parameters at runtime.
- Another advantage of HQL is that Hibernate will translate it into the correct SQL dialect for your app
#### Update Example
- Hibernate will determine when to use Update or Insert, so in the query you simply use persist()
- Example from [GetExample.java](https://github.com/sde-coursepack/HibernateExamples/blob/master/src/main/java/edu/virginia/cs/GetExample.java):

```Java
private static void deposit100ToAccount(Account account) {
	account.deposit(100);
	session.persist(account); //Updates the account balance on the database
	session.getTransaction().commit(); //save the change
}
```

#### Criteria Query
- using appropriate queries with the correct criteria will be far faster and more memory efficient than search/sorting/filtering
- limitation: HQL only works in Hibernate
- there are many other JPA implementations
- Example from [GetExample.java](https://github.com/sde-coursepack/HibernateExamples/blob/master/src/main/java/edu/virginia/cs/GetExample.java) :

```Java
private static Account getPrimaryCheckingAccountForClient(Client client) {
	// used to create our query that uses "where" criteria
	CriteriaBuilder builder = session.getCriteriaBuilder(); 

	CriteriaQuery<Account> criteria = builder.createQuery(Account.class);
	Root<Account> root = criteria.from(Account.class);

	//Predicates are individual criteria for a select statement

	//checking accounts only         
	Predicate checkingAccounts = builder.equal(root.get("accountType"), AccountType.CHECKING); 
	
	//accountHolder is Bob's ID

	Predicate clientAccountId = builder.equal(root.get("accountHolder"), client.getId());
	
	//Add the predicates to the criteria with an AND
	criteria.select(root).where(builder.and(checkingAccounts, bobAccountId));

	//Generate a query from the criteria
	Query<Account> this_query = session.createQuery(criteria);

	//Get the list of accounts returned by the criteria
	List<Account> accountList = this_query.getResultList();

	//Print account list
	System.out.println(accountList);

	return accountList.get(0);//returns first checking account

}
```

- two Predicates (criteria)
	1. one for ensuring we only get Checking Accounts
	2. one for ensuring we are only getting accounts belonging to a particular client
#### For the Exam 
- be familiar broadly with the idea of Predicates enough to interpret them
- given a small table and a CriteriaQuery with one or more predicates, you should be able to predict what data the query returns

**On the exam, you should be familiar with:**
1. How to write an Entity class, including the following annotations: `@Entry`, `@Table`, `@Id` and `@GeneratedValue(strategy = GenerationType.Auto)`, `@Column`, `@OneToMany(mappedby = )`, `@ManyToOne` and `@JoinColumn ``
	- Additionally, what an entity class requires
		1. Zero Argument Constructor
		2. Getters and Setters for all `@Column` and `@JoinField` fields
2. Basic Hibernate operations:
	- Opening a new session:  
	    `session = HibernateUtil.getSessionFactory().openSession();`
	- Using `persist(object)` and `get(MyClass.class, id)` to store and retrieve individual objects.
	- Using simple HQL queries with Query objects
		1. `FROM ClassName` to get all of the data from a particular table
		2. Using `Where` clause to filter data
    - Effect of OneToMany and ManyToOne relationships, and how to persist them correctly
	- Be able to read and understand Predicates in a CriteriaQuery code snippet
    
**You will not need to memorize:**
1. You will not be asked anything about the hibernate.cfg.xml file
	- That said, you should be familiar with what the following mean:
		1. `<property name="connection.url">jdbc:sqlite:my_db.sqlite3</property>`
		2. `<mapping class="edu.virginia.sde.hibernate.State" />`
2. Nothing related to the HibernateUtils.java class other than how to create a new session using the HibernateUtils from class (see point 2a above)
3. No advanced HQL queries (i.e., joins or complex criteria)
4. What libraries to import
5. CriteriaQuery Syntax