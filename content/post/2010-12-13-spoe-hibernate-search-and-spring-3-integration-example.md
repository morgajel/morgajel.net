---
author: Jesse Morgan
categories:
- Projects
- SPoE
date: "2010-12-13T20:15:14Z"
guid: http://morgajel.net/?p=1009
id: 1009
title: 'SPoE: Hibernate Search and Spring 3 Integration Example'
url: /2010/12/13/1009
views:
- "323"
---

You’d think that documentation on creating a search engine for a Spring 3 MVC website would be plentiful- search boxes are on most sites these days, and it should be trivial to implement some type of search functionality in Spring MVC.Â Unfortunately, there is very little direct documentation on the subject,Â so I thought I’d write up what I did so others can benefit (and hopefully I hit enough keyword combos to have it be found).

Prerequisites:

- You want to implement a search box that scans a given model looking for a term.
- You have a decent grasp on java, Spring 3 and are using Hiberate Search 3.1.
- You have all your classpaths loaded, and a functional, if not skeletal MVC site.

## Step 1: Grok the basics of Hibernate Search

Start off by reading a little bit of the [documentation](http://docs.jboss.org/hibernate/search/3.1/reference/en/html_single/) for Hibernate Search, mainly ***“ExampleÂ 1.5.Â Example entities after adding Hibernate Search annotations.”*** Go ahead and annotate your model as well as you can using their example as a guide- it doesn’t have to be perfect, just make sure you include a field you’d like to search.

## Step 2: Add Hibernate Search Configuration

Next you have to add some hibernate properties. You can do this in your persistence.xml, hibernate properties, or in your \*-servlet.xml where you define your Hibernate Session Factory (which is what I did). Here’s an example of the props I used- note that my indexBase is in Tomcat’s /tmp/ directory- that may not be the best place for it, but it works for the time being:

```
<bean id="mySessionFactory" class="org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean">
...
Â Â Â  <property name="hibernateProperties">
Â Â Â Â Â  <props>
Â Â Â Â Â Â Â  ...
Â  Â Â  Â Â  <prop key="hibernate.transaction.factory_class">org.hibernate.transaction.JDBCTransactionFactory</prop>
Â Â Â Â Â Â Â  <prop key="hibernate.search.default.directory_provider">org.hibernate.search.store.FSDirectoryProvider</prop>
Â  Â Â  Â Â  <prop key="hibernate.search.default.indexBase">${catalina.base}/tmp/indexes</prop>
Â Â Â Â Â  </props>
Â Â Â  </property>
</bean>

```

## Step 3: Pass SessionFactory to Controller

After you add these properties, you’ll need to autowire your SessionFactory bean into your controller:

```
<bean id="searchController">
Â Â Â  <property name="sessionFactory" ref="mySessionFactory" />
</bean>
```

You’ll also need to add the following to your SearchController to get the sessionFactory autowired:

```
@Autowired
public SessionFactory getSessionFactory() {
Â Â Â  return sessionFactory;
}
public void setSessionFactory(SessionFactory pSessionFactory) {
Â Â Â  this.sessionFactory = pSessionFactory;
}
```

## Step 4: Implement Search

Now for the final step- to implement the actual search.Â I’ll be searching the title and content of my Snippet Class. In my SearchController I’ll add the following:

```
@RequestMapping
public ModelAndView quickSearch(@RequestParam("q") final String searchQuery) {
...
Â Â Â  //FIXME need to sanitize user input
Â Â Â  //FIXME make sure to handle Exceptions
Â Â Â  ModelAndView mav = new ModelAndView();

Â Â Â  //Open a Hibernate Session since none exist. This was the cause of much grief.
Â Â Â  Session session = sessionFactory.openSession();
Â Â Â  //Then open a Hibernate Search session
Â Â Â  FullTextSession fullTextSession = Search.getFullTextSession(session);
Â Â Â  //Begin your search transaction
Â Â Â  Transaction tx = fullTextSession.beginTransaction();

Â Â Â  //Create a list of fields you want to search
Â Â Â  String[] fields = new String[]{"title", "content", "lastModifiedDate"};
Â Â Â  //Define what type of parser and Analyzer you want to use
Â Â Â  MultiFieldQueryParser parser = new MultiFieldQueryParser(fields, new StandardAnalyzer());
Â Â Â  // Create a low-level Lucene Query from our parser using our searchQuery passed in from the user
Â Â Â  org.apache.lucene.search.Query query = parser.parse(searchQuery);
Â Â Â  // Run a Hibernate Search query using the lucene query against our Snippet class
Â Â Â  org.hibernate.Query hibQuery = fullTextSession.createFullTextQuery(query, Snippet.class);

Â Â Â  // List the results
Â Â Â  List results = hibQuery.list();
Â Â Â  // Commit the transaction
Â Â Â  tx.commit();
Â Â Â  // Close your hibernate session
Â Â Â  session.close();
Â Â Â  // pass your result set to your ModelAndView to be used.
Â Â Â  mav.addObject("results", results);
```

And just like that, a simple search in your Spring 3 MVC application using Hibernate Search.Â I’m still a little fuzzy about why you need a transaction for a read-only search, but hey, it’s working, I can fine tune later. It goes without saying that there is no exception handling here- it’s not critical to the example.

The only downside to this implementation is that it doesn’t index existing content, only new content. You CAN do a full index by running:

```
Â Â Â  FullTextSession fullTextSession = Search.getFullTextSession(session);
Â Â Â  Transaction tx = fullTextSession.beginTransaction();Â 
Â Â Â  List snippets = session.createQuery("from Snippet as snippet").list();
Â Â Â Â Â  for (Snippet snippet : snippets) {Â Â Â Â 
Â  Â Â     <strong>fullTextSession.index(snippet);</strong>
Â Â  Â Â  }
Â Â Â  tx.commit(); //index is written at commit time
```

But I’d suggest making an administrative tool/button/one-time-event out of it rather than implementing it here, because I sure there’s a slight performance hit in indexing thousands of lines of text.

I hope this ends up being of value to someone- if it does, please leave a comment letting me know, and if I have any mistakes above, please let me know that as well 😀