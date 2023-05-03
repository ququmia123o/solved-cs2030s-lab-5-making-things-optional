Download Link: https://assignmentchef.com/product/solved-cs2030s-lab-5-making-things-optional
<br>
<h2>Review Topics</h2>

<ul>

 <li>Inheritance</li>

 <li>Overriding</li>

 <li>Interface</li>

 <li>Generic class</li>

</ul>

<h2>Topic Coverage</h2>

<ul>

 <li>Bounded Type Parameter</li>

 <li>HashMap</li>

 <li>Optional</li>

 <li>Functional Interfaces</li>

</ul>

<h2>Problem Description</h2>

In this lab, we are going to see how we can use <tt>Optional</tt> to simplify our tt, allowing us to focus on the core application logic instead of worrying about edge cases. This lab also involves using the <tt>HashMap</tt> class from Java Collections and more functional interfaces from Java: <tt>Consumer</tt>, <tt>Supplier</tt>, and <tt>Runnable</tt>.

<h2>Task</h2>

Your task is to read in a roster of students, the modules they take, the assessments they have completed, and the grade for each assessment. Then, given a query consisting of a triplet: a student, a module, and an assessment, retrieve the corresponding grade.

For instance, if the input is:

<pre>Steve CS1010 Lab3 ASteve CS1231 Test A+Bruce CS2030 Lab1 C</pre>

and the query is <tt>Steve CS1231 Test</tt>, the program should print <tt>A+</tt>.




In our scenario, a roster has zero or more students; a student takes zero or more modules, a module has zero or more assessments, and each assessment has exactly one grade. Each of these entities can be uniquely identified by a string.




<h2>Preamble</h2>

<h2><tt>Map</tt></h2>

<tt>Map&lt;K,V&gt;</tt> is a generic interface from the Java Collection Framework, the implementation of which is useful for storing a collection of items and retrieving an item. It maintains a map (aka dictionary) between keys (of type <tt>K</tt>) and values (of type <tt>V</tt>). The two core methods are (i) <tt>put</tt>, which stores a (key, value) pair into the map, and (ii) <tt>get</tt>, which returns the value associated with a given key if the key is found or returns <tt>null</tt> otherwise.

The following examples show how the <tt>Map&lt;K,V&gt;</tt> interface and its implementation <tt>HashMap&lt;K,V&gt;</tt> can be used.

<pre><b>jshell&gt; Map&lt;String,Integer&gt; map = new HashMap&lt;&gt;();</b><b>jshell&gt; map.put("one", 1);</b>$.. ==&gt; null<b>jshell&gt; map.put("two", 2);</b>$.. ==&gt; null<b>jshell&gt; map.put("three", 3);</b>$.. ==&gt; null<b>jshell&gt; map.get("one")</b>$.. ==&gt; 1<b>jshell&gt; map.get("four")</b>$.. ==&gt; null<b>jshell&gt; map.entrySet()</b>$.. ==&gt; [one=1, two=2, three=3]<b>jshell&gt; for (Map.Entry e: map.entrySet()) {</b><b>   ...&gt;  System.out.println(e.getKey() + ":" + e.getValue());</b><b>   ...&gt; }</b>one:1two:2three:3</pre>

<h2>Level 1</h2>

<h2><tt>Assessment</tt> class and <tt>Keyable</tt> interface</h2>

We shall start by writing the <tt>Assessment</tt> class that implements the following <tt>Keyable</tt> interface.

<pre>interface Keyable {    String getKey();}</pre>

Include a <tt>getGrade</tt> method that returns the grade of an assessment.

<pre><b>jshell&gt; new Assessment("Lab1", "B")</b>$.. ==&gt; {Lab1: B}<b>jshell&gt; new Assessment("Lab1", "B").getGrade()</b>$.. ==&gt; "B"<b>jshell&gt; new Assessment("Lab1", "B").getKey()</b>$.. ==&gt; "Lab1"<b>jshell&gt; /exit</b></pre>

Check the format correctness of the output by running the following on the command line:

<pre>$ javac -Xlint:rawtypes *.java$ jshell -q your_java_files_in_bottom-up_dependency_order &lt; test1.jsh</pre>

Check your styling by issuing the following

<pre>$ checkstyle *.java</pre>

<h2>Level 2</h2>

<h2><tt>Module</tt> class</h2>

Write the <tt>Module</tt> class to store (via the <tt>put</tt> method) the assessments of a module in a map for easy retrieval as part of answering queries. A module can have zero or more assessments, with each assessment having a title as a key — a unique identifier.

<pre><b>jshell&gt; new Module("CS2040")</b>$.. ==&gt; CS2040: {}<b>jshell&gt; new Module("CS2040").getKey();</b>$.. ==&gt; "CS2040"<b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B"))</b>$.. ==&gt; CS2040: {{Lab1: B}}<b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).put(new Assessment("Lab2","A+"))</b>$.. ==&gt; CS2040: {{Lab1: B}, {Lab2: A+}}<b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).put(new Assessment("Lab2","A+")).get("Lab1")</b>$.. ==&gt; {Lab1: B}<b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).put(new Assessment("Lab2","A+")).get("Lab2")</b>$.. ==&gt; {Lab2: A+}<b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).put(new Assessment("Lab2","A+")).get("Lab3")</b>$.. ==&gt; null<b>jshell&gt; /exit</b></pre>

Check the format correctness of the output by running the following on the command line:

<pre>$ javac -Xlint:rawtypes *.java$ jshell -q your_java_files_in_bottom-up_dependency_order &lt; test2.jsh</pre>

Check your styling by issuing the following

<pre>$ checkstyle *.java</pre>

<h2>Level 3</h2>

<h2><tt>Student</tt> class and generic class <tt>KeyableMap</tt></h2>

Write a <tt>Student</tt> class that stores the modules he/she reads in a map via the <tt>put</tt> method. A student can read zero or more modules, with each module having a unique module code as its key. You will notice that the implementations of the <tt>Student</tt> and <tt>Module</tt> classes are very similar. Hence, by applying the <i>abstraction principle</i>, write a generic class <tt>KeyableMap</tt> to reduce the duplication.

<i>Hint:</i> <tt>KeyableMap&lt;V&gt;</tt> is a generic class that wraps around a <tt>String</tt> key (i.e. implements <tt>Keyable</tt>) and a <tt>Map&lt;String,V&gt;</tt>. <tt>KeyableMap</tt> models an entity that contains a map, but is also itself contained in another container (e.g. a student contains a map of modules but could be contained in a roster). The parameter type <tt>V</tt> is the type of the value of items stored in the map; <tt>V</tt> must be a subtype of <tt>Keyable</tt>.

The class <tt>KeyableMap</tt> is a mutable class — we made this decision since the <tt>Map</tt> implementation in Java Collection Framework is mutable. <tt>KeyableMap</tt> provides two core methods:

<ul>

 <li><tt>get(String key)</tt> which returns the item with the given <tt>key</tt>;</li>

 <li><tt>put(V item)</tt> which adds the key-value pair (<tt>item.getKey()</tt>,<tt>item</tt>) into the map. The <tt>put</tt> method returns a <tt>KeyableMap</tt>. To avoid type mismatch when chaining <tt>put</tt> method calls together, each sub-class of <tt>KeyableMap</tt> should override the <tt>put</tt> method from <tt>KeyableMap</tt> and change the return type to the corresponding sub-classes. E.g., <tt>Student</tt> should override <tt>put</tt> to return a <tt>Student</tt> through a (guaranteed to be safe) type casting.</li>

</ul>

<pre><b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).get("Lab1")</b>$.. ==&gt; {Lab1: B}<b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).get("Lab1").getGrade()</b>$.. ==&gt; "B"<b>jshell&gt; new Student("Tony").put(new Module("CS2040").put(new Assessment("Lab1", "B")))</b>$.. ==&gt; Tony: {CS2040: {{Lab1: B}}}<b>jshell&gt; new Student("Tony").put(new Module("CS2040").put(new Assessment("Lab1", "B"))).get("CS2040")</b>$.. ==&gt; CS2040: {{Lab1: B}}<b>jshell&gt; Student natasha = new Student("Natasha");</b><b>jshell&gt; natasha.put(new Module("CS2040").put(new Assessment("Lab1", "B")))</b>$.. ==&gt; Natasha: {CS2040: {{Lab1: B}}}<b>jshell&gt; natasha.put(new Module("CS2030").put(new Assessment("PE", "A+")).put(new Assessment("Lab2", "C")))</b>$.. ==&gt; Natasha: {CS2030: {{Lab2: C}, {PE: A+}}, CS2040: {{Lab1: B}}}<b>jshell&gt; Student tony = new Student("Tony");</b><b>jshell&gt; tony.put(new Module("CS1231").put(new Assessment("Test", "A-")))</b>$.. ==&gt; Tony: {CS1231: {{Test: A-}}}<b>jshell&gt; tony.put(new Module("CS2100").put(new Assessment("Test", "B")).put(new Assessment("Lab1", "F")))</b>$.. ==&gt; Tony: {CS1231: {{Test: A-}}, CS2100: {{Lab1: F}, {Test: B}}}<b>jshell&gt; /exit</b></pre>

Check the format correctness of the output by running the following on the command line:

<pre>$ javac -Xlint:rawtypes *.java$ jshell -q your_java_files_in_bottom-up_dependency_order &lt; test3.jsh</pre>

Check your styling by issuing the following

<pre>$ checkstyle *.java</pre>

<h2>Level 4</h2>

<h2>Avoiding <tt>null</tt> with <tt>Optional</tt></h2>

So far we have been avoiding invalid retrievals such as

<pre>new Module("CS2040").put(new Assessment("Lab1", "B")).get("Lab2")</pre>

which would result in a <tt>null</tt> value. Furthermore, chaining methods like

<pre>new Student("Tony").put(new Module("CS2040").put(new Assessment("Lab1", "B"))).get("CS2030" ).get("Lab1")</pre>

would result in a <tt>NullPointerException</tt> being thrown due to calling <tt>get("Lab1")</tt> on a <tt>null</tt> value. Let’s deal with the generation of <tt>null</tt> values first, and worry about chaining <tt>get</tt> methods in the next level.

Take a look at the <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Optional.html">Java documentation of Optional</a> to familiarize yourself with the APIs available. Modify the <tt>KeyableMap</tt> generic class such that each call to <tt>get</tt> returns an <tt>Optional&lt;V&gt;</tt> where <tt>V</tt> is a subtype of <tt>Keyable</tt>.

<pre><b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).get("Lab1")</b>$.. ==&gt; Optional[{Lab1: B}]<b>jshell&gt; new Student("Tony").put(new Module("CS2040").put(new Assessment("Lab1", "B")))</b>$.. ==&gt; Tony: {CS2040: {{Lab1: B}}}<b>jshell&gt; new Student("Tony").put(new Module("CS2040").put(new Assessment("Lab1", "B"))).get("CS2040")</b>$.. ==&gt; Optional[CS2040: {{Lab1: B}}]<b>jshell&gt; Student natasha = new Student("Natasha");</b><b>jshell&gt; natasha.put(new Module("CS2040").put(new Assessment("Lab1", "B")))</b>$.. ==&gt; Natasha: {CS2040: {{Lab1: B}}}<b>jshell&gt; natasha.put(new Module("CS2030").put(new Assessment("PE", "A+")).put(new Assessment("Lab2", "C")))</b>$.. ==&gt; Natasha: {CS2030: {{Lab2: C}, {PE: A+}}, CS2040: {{Lab1: B}}}<b>jshell&gt; Student tony = new Student("Tony");</b><b>jshell&gt; tony.put(new Module("CS1231").put(new Assessment("Test", "A-")))</b>$.. ==&gt; Tony: {CS1231: {{Test: A-}}}<b>jshell&gt; tony.put(new Module("CS2100").put(new Assessment("Test", "B")).put(new Assessment("Lab1", "F")))</b>$.. ==&gt; Tony: {CS1231: {{Test: A-}}, CS2100: {{Lab1: F}, {Test: B}}}<b>jshell&gt; /exit</b></pre>

As you can see, the only difference is that each value returned from an invocation of the <tt>get</tt> method is wrapped in an <tt>Optional</tt>.

Check the format correctness of the output by running the following on the command line:

<pre>$ javac -Xlint:rawtypes *.java$ jshell -q your_java_files_in_bottom-up_dependency_order &lt; test4.jsh</pre>

Check your styling by issuing the following

<pre>$ checkstyle *.java</pre>

<h2>Level 5</h2>

<h2>“Chaining” <tt>Optionals</tt></h2>

Chaining <tt>Optionals</tt> together as such

<pre>new Student("Tony").put(new Module("CS2040").put(new Assessment("Lab1", "B"))).get("CS2040" ).get("Lab1")</pre>

will now result in a compilation error instead. This is because the <tt>Optional</tt> class does not have a <tt>get(String)</tt> method defined (<i>although it does define a <tt>get()</tt> method which, other than for debugging purposes, should typically be avoided</i>).

Rather than chaining in the usual way, we do it through a <tt>map</tt> or <tt>flatMap</tt>. Let’s start with <tt>map</tt>.

<pre><b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).get("Lab1")</b>$.. ==&gt; Optional[{Lab1: B}]<b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).get("Lab1").getGrade()</b>|  Error:|  cannot find symbol|    symbol:   method getGrade()|  new Module("CS2040").put(new Assessment("Lab1", "B")).get("Lab1").getGrade()|  ^------------------------------------------------------------------------^<b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).get("Lab1").map(x -&gt; x.getGrade())</b>$.. ==&gt; Optional[B]<b>jshell&gt; new Module("CS2040").put(new Assessment("Lab1", "B")).get("Lab1").map(Assessment::getGrade)</b>$.. ==&gt; Optional[B]</pre>

As expected, invoking <tt>getGrade()</tt> on an <tt>Optional</tt> results in a compilation error. However, we can perform a similar chaining effect by passing in the functionality of <tt>getGrade</tt> (either in the form of a lambda or method reference) to <tt>Optional</tt>‘s <tt>map</tt> method. Notice the return value is actually wrapped in another <tt>Optional</tt>. When using <tt>map</tt>, you can think of the operation as “taking the value out of the <tt>Optional</tt> box, transforming it via the function passed to map, and wrap the transformed value back in another <tt>Optional</tt>“.

Now this is where things start to get interesting! Look at the following:

<pre><b>jshell&gt; new Student("Tony").put(new Module("CS2040").put(new Assessment("Lab1", "B"))).get("CS2040" ).map(x -&gt; x.get("Lab1"))</b>$.. ==&gt; Optional[Optional[{Lab1: B}]]</pre>

Observe that the return value is an <tt>Optional</tt> wrapped around another <tt>Optional</tt> that wraps around the desired value! Why is this so? The difference lies in the return type of <tt>Assessment::getGrade</tt> (read <tt>getGrade</tt> method of the <tt>Assessment</tt> class) and <tt>Module::get</tt>. The former returns a <tt>String</tt>, while the latter returns an <tt>Optional</tt>.

In <tt>x -&gt; x.getGrade()</tt> (or <tt>Assessment::getGrade</tt>), the transformed value is simply the grade, and this is wrapped in an Optional. However, passing <tt>x -&gt; x.get("Lab1")</tt> in the above code snippet results in a transformed value of <tt>Optional</tt>. And this transformed value is wrapped around another <tt>Optional</tt> via the <tt>map</tt> operation!

As such, we use the <tt>flatMap</tt> method instead. You may think of <tt>flatMap</tt> as flattening the <tt>Optional</tt>s into a single one.

<pre><b>jshell&gt; new Student("Tony").put(new Module("CS2040").put(new Assessment("Lab1", "B"))).get("CS2040" ).flatMap(x -&gt; x.get("Lab1"))</b>$.. ==&gt; Optional[{Lab1: B}]<b>jshell&gt; new Student("Tony").put(new Module("CS2040").put(new Assessment("Lab1", "B"))).get("CS2040" ).flatMap(x -&gt; x.get("Lab1")).map(Assessment::getGrade)</b>$.. ==&gt; Optional[B]</pre>

Now you are ready to create a roster. Define a <tt>Roster</tt> class that stores the students in a map via the <tt>put</tt> method. A roster can have zero or more students, with each student having a unique id as its key. Once again, notice the similarities between <tt>Roster</tt>, <tt>Student</tt> and <tt>Module</tt>.

Define a method called <tt>getGrade</tt> in <tt>Roster</tt> to answer the query from the user. The method takes in three <tt>String</tt> parameters, corresponds to the student id, the module code, and the assessment title, and returns the corresponding grade.

In cases where there are no such student, or the student does not read the given module, or the module does not have the corresponding assessment, then output <tt>No such record</tt> followed by details of the query. Here, you might find <tt>Optional::orElse</tt> useful.




<pre><b>jshell&gt; Student natasha = new Student("Natasha");</b><b>jshell&gt; natasha.put(new Module("CS2040").put(new Assessment("Lab1", "B")))</b>$.. ==&gt; Natasha: {CS2040: {{Lab1: B}}}<b>jshell&gt; natasha.put(new Module("CS2030").put(new Assessment("PE", "A+")).put(new Assessment("Lab2", "C")))</b>$.. ==&gt; Natasha: {CS2030: {{Lab2: C}, {PE: A+}}, CS2040: {{Lab1: B}}}<b>jshell&gt; Student tony = new Student("Tony");</b><b>jshell&gt; tony.put(new Module("CS1231").put(new Assessment("Test", "A-")))</b>$.. ==&gt; Tony: {CS1231: {{Test: A-}}}<b>jshell&gt; tony.put(new Module("CS2100").put(new Assessment("Test", "B")).put(new Assessment("Lab1", "F")))</b>$.. ==&gt; Tony: {CS1231: {{Test: A-}}, CS2100: {{Lab1: F}, {Test: B}}}<b>jshell&gt; Roster roster = new Roster("AY1920").put(natasha).put(tony)</b><b>jshell&gt; roster</b>roster ==&gt; AY1920: {Natasha: {CS2030: {{Lab2: C}, {PE: A+}}, CS2040: {{Lab1: B}}}, Tony: {CS1231: {{Test: A-}}, CS2100: {{Lab1: F}, {Test: B}}}}<b>jshell&gt; roster.get("Tony").flatMap(x -&gt; x.get("CS1231")).flatMap(x -&gt; x.get("Test")).map(Assessment::getGrade)</b>$.. ==&gt; Optional[A-]<b>jshell&gt; roster.get("Natasha").flatMap(x -&gt; x.get("CS2040")).flatMap(x -&gt; x.get("Lab1")).map(Assessment::getGrade)</b>$.. ==&gt; Optional[B]<b>jshell&gt; roster.get("Tony").flatMap(x -&gt; x.get("CS1231")).flatMap(x -&gt; x.get("Exam")).map(Assessment::getGrade)</b>$.. ==&gt; Optional.empty<b>jshell&gt; roster.get("Steve").flatMap(x -&gt; x.get("CS1010")).flatMap(x -&gt; x.get("Lab1")).map(Assessment::getGrade)</b>$.. ==&gt; Optional.empty<b>jshell&gt; roster.getGrade("Tony","CS1231","Test")</b>$.. ==&gt; "A-"<b>jshell&gt; roster.getGrade("Natasha","CS2040","Lab1")</b>$.. ==&gt; "B"<b>jshell&gt; roster.getGrade("Tony","CS1231","Exam");</b>$.. ==&gt; "No such record: Tony CS1231 Exam"<b>jshell&gt; roster.getGrade("Steve","CS1010","Lab1");</b>$.. ==&gt; "No such record: Steve CS1010 Lab1"<b>jshell&gt; /exit</b></pre>

Check the format correctness of the output by running the following on the command line:

<pre>$ javac -Xlint:rawtypes *.java$ jshell -q your_java_files_in_bottom-up_dependency_order &lt; test5.jsh</pre>

Check your styling by issuing the following

<pre>$ checkstyle *.java</pre>

<h2>Level 6</h2>

<h2>The <tt>Main</tt> class</h2>

Now use the classes that you have built and write a <tt>Main</tt> class to solve the following:

Read the following from the standard input:

<ul>

 <li>The first token read is an integer <i>N</i>, indicating the number of records to be read.</li>

 <li>The subsequent inputs consist of <i>N</i> records, each record consists of four words, separated by one or more spaces. The four words correspond to the student id, the module code, the assessment title, and the grade, respectively.</li>

 <li>The subsequent inputs consist of zero or more queries. Each query consists of three words, separated by one or more spaces. The three words correspond to the student id, the module code, and the assessment title.</li>

</ul>

For each query, if a match in the input is found, print the corresponding grade to the standard output. Otherwise, print “No Such Record:” followed by the three words given in the query, separated by exactly one space.




See sample input and output below. Inputs are underlined.

<pre>$ java Main<u>12Jack   CS2040 Lab4    BJack   CS2040 Lab6    CJane   CS1010 Lab1    AJane   CS2030 Lab1    A+Janice CS2040 Lab1    A+Janice CS2040 Lab4    A+Jim    CS1010 Lab9    A+Jim    CS2010 Lab1    CJim    CS2010 Lab2    BJim    CS2010 Lab8    A+Joel   CS2030 Lab3    CJoel   CS2030 Midterm AJack   CS2040 Lab4Jack   CS2040 Lab6Janice CS2040 Lab1Janice CS2040 Lab4Joel   CS2030 MidtermJason  CS1010 Lab1Jack   CS2040 Lab5Joel   CS2040 Lab3</u>BCA+A+ANo such record: Jason CS1010 Lab1No such record: Jack CS2040 Lab5No such record: Joel CS2040 Lab3</pre>