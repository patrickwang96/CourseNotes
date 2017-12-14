# Source Code

emp_dept.pig
```
emp = load 'emp_dept/emp.csv' as (empno:int, ename:chararray, job:chararray, mgr:int, hiredate:datetime, sal:float, deptno:int);
dept = load 'emp_dept/dept.csv' as (deptno:int, dname:chararray, loc:chararray);
salgrade = load 'emp_dept/salgrade.csv' as (grade:int, losal:int, hisal:int);

smith_record = filter emp by ename matches 'SMITH';
-- (1980-12-17T00:00:00.000+08:00)
smith_date_question1 = foreach smith_record generate hiredate as date;

ford_record = filter emp by ename matches 'FORD';
-- (ANALYST)
ford_jobtitle_question2 = foreach ford_record generate job as jobtitle;


emp_ordered = order emp by hiredate asc;
-- (7369,SMITH,CLERK,7902,1980-12-17T00:00:00.000+08:00,800.0,20)
first_employee_question3 = limit emp_ordered 1;

emp_by_dept = group emp by deptno;
--(10,3)
--(20,5)
--(30,6)
number_of_emp_by_dept_question4 = foreach emp_by_dept generate group as deptno, COUNT(emp) as number;

emp_with_loc = join emp by deptno left, dept by deptno;
emp_with_loc_with_schema = foreach emp_with_loc generate emp::empno as empno, dept::loc as loc;
emp_by_loc = group emp_with_loc_with_schema by loc;
--(DALLAS,5)
--(CHICAGO,6)
--(NEW YORK,3)
number_of_emp_by_loc_question5 = foreach emp_by_loc generate group as loc, COUNT(emp_with_loc_with_schema) as number;

-- use previous question's emp_with_loc table
emp_with_loc_and_sal_schema= foreach emp_with_loc generate emp::empno as empno, dept::loc as loc, emp::sal as sal;
emp_by_loc_with_sal = group emp_with_loc_and_sal_schema by loc;
--(DALLAS,2175.0)
--(CHICAGO,1566.6666666666667)
--(NEW YORK,2916.6666666666665)
avg_sal_by_loc_question6 = foreach emp_by_loc_with_sal generate group as loc, AVG(emp_with_loc_and_sal_schema.sal) as avg_sal;


-- use the emp_by_dept table in question4
max_sal_by_dept = foreach emp_by_dept generate group as deptno, MAX(emp.sal) as max_sal;
emp_with_max_sal = join emp by deptno left, max_sal_by_dept by deptno;
emp_with_highest_sal_by_dept = filter emp_with_max_sal by max_sal_by_dept::max_sal == emp::sal;
--(7839,KING,5000.0)
--(7788,SCOTT,3000.0)
--(7902,FORD,3000.0)
--(7698,BLAKE,2850.0)
highest_paid_emp_by_dept_question7 = foreach emp_with_highest_sal_by_dept generate emp::empno as empno, emp::ename as ename, emp::sal as sal;


emp2 = load 'emp_dept/emp.csv' as (empno:int, ename:chararray, job:chararray, mgr:int, hiredate:datetime, sal:float, deptno:int);
emp_with_subordinate = join emp by empno, emp2 by mgr;
requested_mgr_no = foreach emp_with_subordinate generate emp::mgr as empno;
requested_mgr_no_distinct = distinct requested_mgr_no;
requested_mgr = join requested_mgr_no_distinct by empno, emp by empno;
--(7655,JONES,MANAGER,7839,1981-04-02T00:00:00.000+08:00,2975.0,20)
--(7839,KING,PRESIDENT,,1981-11-12T00:00:00.000+08:00,5000.0,10)
managers_question8 = foreach requested_mgr generate requested_mgr_no_distinct::empno as empno, emp::ename as ename, emp::job as job, emp::mgr as mgr, emp::hiredate as hiredate, emp::sal as sal, emp::deptno as deptno;


emp_by_year = group emp by GetYear(hiredate);
--(1980,1)
--(1981,10)
--(1987,2)
--(1991,1)
emp_number_by_year_question9 = foreach emp_by_year generate group as year, COUNT(emp) as number;


emp_with_grade = cross emp, salgrade;
emp_filtered_with_grade = filter emp_with_grade by emp::sal >= (float)salgrade::losal and emp::sal < (float)salgrade::hisal;
--(7934,MILLER,2)
--(7900,JAMES,1)
--(7876,ADAMS,1)
--(7844,TURNER,3)
--(7839,KING,5)
--(7782,CLARK,4)
--(7698,BLAKE,4)
--(7654,MARTIN,2)
--(7655,JONES,4)
--(7521,WARD,2)
--(7499,ALLEN,3)
--(7369,SMITH,1)
emp_with_selfgrade_question10 = foreach emp_filtered_with_grade generate emp::empno as empno, emp::ename as ename, salgrade::grade as paygrade;


```

emp_dept.sql

```

-- Question1
select hiredate from emp where ename = 'SMITH';

-- Question2 
select job from emp where ename = 'FORD';

-- Question3
select * from emp order by hiredate asc limit 1;

-- Question4
select COUNT(*) as number, deptno from emp group by deptno;

-- Queestion5
select COUNT(*) as number, loc from emp, dept where dept.deptno = emp.deptno group by loc;

-- Question6
select AVG(emp.sal) as avg_sal, loc from emp, dept where dept.deptno = emp.deptno group by loc;

-- Question7
select emp.* from emp, (
	select MAX(emp.sal) as max_sal, deptno from emp group by deptno) as tmp 
	where emp.deptno = tmp.deptno and emp.sal = tmp.max_sal;

-- Question8
select distinct e.*  from emp as e, (
	select b.mgr as mgrno from emp as a, emp as b where a.mgr = b.empno) as tmp
	where e.empno = tmp.mgrno;

-- Question9
select COUNT(*), yr from emp group by strftime('%Y', hiredate) as yr;

select COUNT(*) as number, tmp.year as year from (
	select strftime('%Y', hiredate) as year, emp.* from emp) as tmp
	group by tmp.year;

-- Question10
select e.*, s.grade from emp as e, salgrade as s where e.sal < s.hisal and e.sal >= s.losal;

```

graph.pig

```

-- SID: 54378942
-- NAME: WANG Ruochen

-- loading them into A.
-- each record is nodeA\tnodeB
A = load 'ex_data/roadnet/roadNet-CA.txt' as (nodeA: chararray, nodeB: chararray);

-- groupby A
B_out = group A by nodeA;

-- groupby nodeB
B_in = group A by nodeB;

-- group wise counting
-- C -> (outdegree, node)
C_out = foreach B_out generate COUNT(A) as freq, group as node;

C_in = foreach B_in generate COUNT(A) as freq, group as node;

-- why totalCount = join C_in by C_in.node full, C_out by C_out.node; failed?
totalCount = join C_in by node full, C_out by node;

-- D: {nodeName: chararray,inDegree: long,outDegree: long}
-- the in degree and out degree of each node
D = foreach totalCount generate C_in::node as nodeName, (C_in::freq is null ? (long)0 : C_in::freq) as inDegree , (C_out::freq is null ? (long)0 : C_out::freq) as outDegree;
inDegreeGrouped = group D by inDegree;
outDegreeGrouped = group D by outDegree;

-- the frequency of indegree value
--(1,179)
--(2,92)
--(3,117)
--(4,69)
--(5,2)
inDegreeFreq = foreach inDegreeGrouped generate group as degree, COUNT(D) as freq;
-- the frequency of outdegree value
--(0,141)
--(1,30)
--(2,18)
--(3,153)
--(4,111)
--(5,5)
--(6,1)
outDegreeFreq = foreach outDegreeGrouped generate group as degree, COUNT(D) as freq;


inNodeGroup = group inDegreeFreq all;
outNodeGroup = group outDegreeFreq all;

-- (459)
nodeCount = foreach inNodeGroup generate SUM(inDegreeFreq.freq) as count;

outDegreePercent = foreach outDegreeFreq generate degree, (double)freq/nodeCount.count as percent;

--(0,0.30718954248366015)
deadEndNode = filter outDegreePercent by degree == 0;

inDegreeSum = foreach inDegreeFreq generate degree * freq as sum;
outDegreeSum = foreach outDegreeFreq generate degree * freq as sum;

inDegreeGrouped = group inDegreeSum all;
outDegreeGrouped = group outDegreeSum all;

-- (2.178649237472767)
avgInDegree = foreach inDegreeGrouped generate (double)SUM(inDegreeSum.sum)/nodeCount.count as average;
-- (2.178649237472767)
avgOutDegree = foreach outDegreeGrouped generate (double)SUM(outDegreeSum.sum)/nodeCount.count as average;

```

kmeans.scala

```

// SID: 54378942
// NAME: WANG Ruochen

import org.apache.spark.SparkContext
import org.apache.spark.SparkContext._
import org.apache.spark._

object KmeansModel{

  def distance(p: Vector[Double], q: Vector[Double]): Double = {
    math.sqrt(p.zip(q).map(pair => math.pow(pair._1 - pair._2, 2)).sum)
  }

  def clostestpoint(q: Vector[Double], candidates: Array[Vector[Double]]): Vector[Double] = {
    candidates.minBy(p => distance(p, q))
  }

  def add_vec(v1: Vector[Double], v2: Vector[Double]): Vector[Double] = {
    v1.zip(v2).map(pair => pair._1 + pair._2)
  }

  def average(cluster: Iterable[Vector[Double]]): Vector[Double] = {
    cluster.reduce(add_vec).map(vec => vec / cluster.size)
  }

  def compare_centroids(a1: Iterable[Vector[Double]], a2: Iterable[Vector[Double]]): Boolean = {
    a1.zip(a2).map(pair => pair._1.equals(pair._2)).reduce(_ && _)
  }

  def main(args: Array[String]): Unit ={

//    val k = int(args(0))
//    for dev, just fix k value to 3 -. -
    val k = 3
    val sc = new SparkContext()
    val filePath = "clustering_dataset.txt"
    val input = sc.textFile(filePath)

    val pointsMap = input.map(_.split("\t")).map(_.map(_.toDouble).to[Vector])

    var curCentroids = pointsMap.takeSample(false, k, 1L) // fix rand seeds to 1L in order to be reproduceable

    var nextCentroids = curCentroids.clone()

    do{
      curCentroids = nextCentroids.clone()
      nextCentroids = pointsMap.map(vec => (clostestpoint(vec, curCentroids), vec)).groupByKey().map(pair => average(pair._2)).collect()
    }while (compare_centroids(curCentroids, nextCentroids) != true)

    val dump_str = curCentroids.map(_.toString()).toString()
    System.out.println(dump_str)
    // dump_str:
    // res6: Array[Vector[Double]] = Array(Vector(12.972570894104999, 19.430958569806002),
    // Vector(6.676557700004402, 13.166919895269002),
    // Vector(6.8945984549098975, 2.11317553301257))

  }
}
```

topkwordcount.java

```
import java.io.IOException;
import java.util.*;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class TopKWordCount {

  public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
      }
    }
  }

  public static class IntSumReducer
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
    }
  }

  static <K,V extends Comparable<? super V>>
  SortedSet<Map.Entry<K,V>> entriesSortedByValues(Map<K,V> map) {
    SortedSet<Map.Entry<K,V>> sortedEntries = new TreeSet<Map.Entry<K,V>>(
      new Comparator<Map.Entry<K,V>>() {
        @Override public int compare(Map.Entry<K,V> e1, Map.Entry<K,V> e2) {
          int res = e2.getValue().compareTo(e1.getValue());
          return res != 0 ? res : 1;
        }
      }
    );
    sortedEntries.addAll(map.entrySet());
    return sortedEntries;
  }

  // Create your 2nd mapper here


  public static class IdMapper
       extends Mapper<Object, Text, Text, Text>{

    private Text word_freq = new Text();
    private Text k    = new Text("-");

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      // Complete your 2nd mapper
        //System.out.println("IdMapper: key: " + key + " value: " + value);
        context.write(k,value);
    }
  }

  // Create your 2nd reducer here

  public static class IntMaxReducer
       extends Reducer<Text,Text,Text,Text> {
    private Text result = new Text();
    private String max_word;

    public void reduce(Text key, Iterable<Text> values,
                       Context context
                       ) throws IOException, InterruptedException {
      // Complete your 2nd reducer
        Map<Text, IntWritable> collection = new HashMap<Text, IntWritable>();

        for (Text val: values){
            StringTokenizer tokenizer = new StringTokenizer(val.toString(),"\t");
            Text word = new Text(tokenizer.nextToken());
            IntWritable count = new IntWritable(Integer.parseInt(tokenizer.nextToken()));
   //         System.out.println("IntMaxReducer: Key: " + word.toString() + " value: " + count.toString());
     //       if (count.compareTo(maxCount) == 1){
     //           maxWord = word;
     //           maxCount = count;
     //       }
            collection.put(word,count);
        }

        // SortedSet<Map.Entry<Text,IntWritable>> sorted_collection = entriesSortedByValues(collection);

        int k = Integer.parseInt(context.getConfiguration().get("numResults"));
        int count = 0;

        for (Map.Entry<Text, IntWritable> e: entriesSortedByValues(collection)){
            context.write(key,new Text(e.getKey().toString() + "\t" + e.getValue().toString()));
            count++;
            if (count >= k) break;
        }
    }
  }


  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(TopKWordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);

    FileInputFormat.addInputPath(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    job.waitForCompletion(true);


    Configuration conf2 = new Configuration();
    conf2.set("numResults", args[3]);
    Job job2 = Job.getInstance(conf2, "word count");

    // Insert your Code Here

    job2.setJarByClass(TopKWordCount.class);
    job2.setMapperClass(IdMapper.class);
    job2.setReducerClass(IntMaxReducer.class);
    job2.setOutputKeyClass(Text.class);
    job2.setOutputValueClass(Text.class);

    FileInputFormat.addInputPath(job2, new Path(args[1]));
    FileOutputFormat.setOutputPath(job2, new Path(args[2]));
    System.exit(job2.waitForCompletion(true) ? 0 : 1);

  }
}
```


