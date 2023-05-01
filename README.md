Download Link: https://assignmentchef.com/product/solved-b561-assignment-4-the-primary-key-constraints
<br>
In this assignment, you will be required to use PostgreSQL. Your solutions should include the PostgreSQL statements for solving the problems, as well as the results of running these statements. Turn in a file .sql with your solutions (where necessary, include comments explaining your solutions). Turn in a file .txt which illustrates how your solutions work. Consider a database with the following relations:

<table width="0">

 <tbody>

  <tr>

   <td width="268">Student(sid int, sname text, major text)</td>

   <td width="355">sid is primary key</td>

  </tr>

  <tr>

   <td width="268">Course(cno int, cname text, total int, max int)</td>

   <td width="355">cno is primary key total is the number of students enrolled in course cno max is the maximum permitted enrollment for course cno</td>

  </tr>

  <tr>

   <td width="268">Prerequisite(cno int, prereq int)</td>

   <td width="355"><strong>meaning</strong>: course cno has as a prerequisite course prereq cno is foreign key referencing Course prereq is foreign key referencing Course</td>

  </tr>

  <tr>

   <td width="268">HasTaken(sid int,cno int)</td>

   <td width="355"><strong>meaning</strong>: student sid has taken course cno in the past sid foreign key referencing Student cno foreign key referencing Course</td>

  </tr>

  <tr>

   <td width="268">Enroll(sid int,cno int)</td>

   <td width="355"><strong>meaning</strong>: student sid is currently enrolled in course cno sid foreign key referencing Student cno foreign key referencing Course</td>

  </tr>

  <tr>

   <td width="268">Waitlist(sid int,cno int, position int)</td>

   <td width="355"><strong>meaning</strong>: student sid is on the waitlist to enroll in cno, where pos is the relative position of student sid on the waitlist to enroll in course cno sid foreign key referencing Student cno foreign key referencing Course</td>

  </tr>

 </tbody>

</table>

<ol>

 <li>(a) Notice the primary key constraints specified for these relations. Write insert triggers to enforce these primary constraints.

  <ul>

   <li>Notice the foreign key constraints specified for these relations. Writeinsert and delete triggers on the appropriate relations to enforce these foreign key constraints. Furthermore, write delete triggers on the Student and Course relations that have cascading deletion effects on the relations that hold foreign keys that reference the primary keys of these relations.</li>

   <li>The HasTaken relation is special in the sense that only inserts are permitted on it. HasTaken serves as a historical record of courses that students have taken in the past. Furthermore, we require that no new inserts are permitted on HasTaken from the moment that students start getting enrolled in the Enroll Write trigger on HasTaken to enforce these requirements.</li>

   <li>The Course and Prerequisite relations are special in the sense that only inserts are permitted on them. In addition, we assume that Prerequisite and Course can no longer change from the moment that enrollment records get inserted or deleted in the Enroll In particular, no new courses and/or no new prerequisites for courses can be added or updated from that point on. The idea behind these requirements is that course-data is very rigid temporarily relative to students enrolling in such courses. Write triggers to enforce these requirements.</li>

  </ul></li>

 <li>Furthermore (1) inserts and deletes in the Enroll relation and (2) insertand deletes in the Waitlist are governed by the following rules:

  <ul>

   <li>A student can only enroll in a course if he or she has taken all the prerequisites for that course. If the enrollment succeeds, the total enrollment for that course needs to be incremented by 1.</li>

   <li>A student can only enroll in a course if his or her enrollment does not exceed the maximum enrollment for that course. However, the student must then be placed at the next available position on the waitlist for that course.</li>

   <li>A student can drop a course, either by removing him or herself from the Waitlist relation or from the Enroll relation. When the latter happens and if there are students on the waitlist for that course, then the student who is at the first position for that course on the waitlist gets enrolled in that course and removed from the waitlist. If there are no students on the waitlist, then the total enrollment for that course needs to decrease by 1.</li>

  </ul></li>

</ol>

Write appropriate triggers to enforce these rules.

<ol start="3">

 <li>Consider a situation wherein we repeatedly, and randomly, throw 3 dice<em>d</em><sub>1</sub>, <em>d</em><sub>2</sub>, <em>d</em><sub>3</sub>. Clearly, the possible value for each such dice <em>d<sub>i </sub></em>is between 1 and 6.</li>

</ol>

Define the random variable <em>T </em>which, when applied to a throw of 3 dice <em>d</em><sub>1</sub>, <em>d</em><sub>2</sub>, and <em>d</em><sub>3</sub>, returns the sum of the 3 dice values. I.e.,

<em>T</em>(<em>d</em><sub>1</sub><em>,d</em><sub>2</sub><em>,d</em><sub>3</sub>) = <em>d</em><sub>1 </sub>+ <em>d</em><sub>2 </sub>+ <em>d</em><sub>3</sub><em>.</em>

Notice that the range of values for <em>T </em>is [3<em>,</em>18]. For example <em>T</em>(2<em>,</em>4<em>,</em>5) = <em>T</em>(3<em>,</em>2<em>,</em>6) = 11, <em>T</em>(1<em>,</em>1<em>,</em>6) = 1 + 1 + 6 = 8, <em>T</em>(1<em>,</em>1<em>,</em>1) = 3, etc.

The objective of this question is to compute, in an <strong>incremental way</strong>, an approximation of the <em>expected value </em><strong>E</strong>(<em>T</em>) of the random variable <em>T</em>, as well as an approximation of the <em>standard deviation </em><strong>V</strong>(<em>T</em>) of the random variable <em>T</em>. By definition, the exact values for <strong>E</strong>(<em>T</em>) and <strong>V</strong>(<em>T</em>) are as follows:

<strong>E</strong>

and

<strong>V</strong>

where <em>p</em>(<em>T </em>= <em>v</em>) is the probability that the outcome of <em>T </em>is the value <em>v</em>. Incidentally, the theoretically correct value for√    <strong>E</strong>(<em>T</em>) is 10<em>.</em>5 and that for

<strong>V</strong>(<em>T</em>) is        8<em>.</em>75 = 2<em>.</em>95803···.

Your solution requires the use of a trigger which maintains an approximation of <strong>E</strong>(<em>T</em>) and <strong>V</strong>(<em>T</em>). More specifically, each time a throw of 3 dice is made, these approximations need to be refined in accordance with the outcome of <em>T </em>for this throw. Furthermore, and this needs to be a core part of your solution, the approximations are required to be maintained in an incremental fashion. In particular, you can not store the set of all throws and from that set find the approximations of <strong>E</strong>(<em>T</em>) and <strong>V</strong>(<em>T</em>). To obtain a random value for a single dice, you can use the function call

floor(random()*6)+1

This works since the function call random() produces a random value in the range [0<em>,</em>1). Therefore a throw of three dices is a tuple of the form

(floor(random()*6)+1<em>,</em>floor(random()*6)+1<em>,</em>floor(random()*6)+1)<em>.</em>

You need to experiment with different numbers of throws. To do this, use the following function template:

create or replace function runExperiment(n int) returns record as $$ declare i int;

begin

— code to initialize the experiment;

for i in 1..n loop

— code to call a trigger on

— throw (floor(random()*6)+1, floor(random()*6)+1, floor(random()*6)+1);

end loop;

return — (approximation for E(T), approximation for V(T)); end;

$$ language plpgsql; Then execute for example

select runExperiment(1000);

This will be an experiment with n = 1000 throws of 3 dice each.

The following is a table with some values obtained for <strong>E</strong>(<em>T</em>) and <strong>V</strong>(<em>T</em>) for different values of n:

<table width="0">

 <tbody>

  <tr>

   <td width="58">n</td>

   <td width="54"><strong>E(T)</strong></td>

   <td width="61"><strong>V(T)</strong></td>

  </tr>

  <tr>

   <td width="58">10</td>

   <td width="54">10.6</td>

   <td width="61">3.16859</td>

  </tr>

  <tr>

   <td width="58">100</td>

   <td width="54">10.24</td>

   <td width="61">2.80756</td>

  </tr>

  <tr>

   <td width="58">1000</td>

   <td width="54">10.419</td>

   <td width="61">3.06356</td>

  </tr>

  <tr>

   <td width="58">10000</td>

   <td width="54">10.505</td>

   <td width="61">2.94929</td>

  </tr>

  <tr>

   <td width="58">100000</td>

   <td width="54">10.490</td>

   <td width="61">2.95849</td>

  </tr>

 </tbody>

</table>


