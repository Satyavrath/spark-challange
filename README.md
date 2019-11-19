# spark-challenge

## Links

- [Repo](https://github.com/Satyavrath/spark-challange)

## Prerequisites

- VS Code
- VS Code Extension: Maven for Java
- VS Code Extension: Java Extension Pack

## Instructions 1 - Start New Maven Project

1. Create a new project using Maven (you do not need to fork this repo).
1. There should be a folder on your laptop where you keep your git projects. Go to this folder, e.g. 44517.
1. Open this parent folder in VS Code.
1. Click the VS Code Extensions icon. Verify you have the two required extensions. If not, install them.
1. Click the VS Code Explorer icon. From the menu, select:
1. View / Command Palette / Maven: Create Maven Project / archetype-quickstart-jdk8 / most recent version.
1. When the folder window opens, click your parent folder up at the top, click "Select Destination Folder".

## Instructions 2 - Interactive Mode

```Bash
groupId: edu.nwmissouri.injamuri
artifactId: spark-challenge
version: HIT ENTER
package: HIT ENTER
Y: HIT ENTER
```

You will now have a spark-challenge project folder. Exit VS Code.


## Instructions 3 - Add to POM.xml

Add POM.xml 

## Prepare the Code

```PowerShell
mvn clean
mvn compile
mvn assembly:single
```

## Execute

```Bash
java -cp target/spark-challenge-1.0.0-jar-with-dependencies.jar edu.nwmissouri.injamuri.App "data.txt"
```


```Java
import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import scala.Tuple2;
import java.util.Arrays;
import java.nio.file.FileSystems;
import java.nio.file.Path;
import java.util.Comparator;
import org.apache.commons.io.FileUtils;
```

## Challenges - create a process method
 // setMaster to local and setAppName to Challenge
    SparkConf sparkConf = new SparkConf().setMaster("local").setAppName("spark-challange");

// define a new JavaSparkContext, pass in the spark configuration object
JavaSparkContext sparkContext = new JavaSparkContext(sparkConf);

// use Java API to flatMap text into an RDD of strings (words)
// In 2.0, FlatMapFunction.call() returns an Iterator rather than Iterable.
// replace what must this be with the correct variable name
    
JavaRDD<String> wordsFromFile = inputFile.flatMap( content -> Arrays.asList(content.split(" ")).iterator());


// use wordsFromFile and mapToPair to create a new Tuple2 for each word
// the tuple is in the form (word, 1)
// Replace each T below to create a JavaPairRDD with the associated types

JavaPairRDD<String, Integer> countData = wordsFromFile.mapToPair(word -> new Tuple2(word, 1))
    .reduceByKey((x, y) -> (int) x + (int) y);

// create new JavaPairRDD that reverses the tuple to (n, word)
// use countData.mapToPair and provide a simple function that
// takes each pair p and outputs a new Tuple2 with p._2 first, then p._1.
// then call .sortByKey() to sort in reverse order,
// pass in Comparator.reverseOrder() to the sortByKey method
JavaPairRDD<Integer, String> output = countData.mapToPair(p -> new Tuple2(p._2, p._1)).sortByKey (Comparator.reverseOrder());
// save results to a folder (RDDs are complex) - provide a simple string value.
String outputFolder = "output.txt";
// get the path to your outputFolder
Path path = FileSystems.getDefault().getPath(outputFolder);

// use FileUtils to delete them quietly
// It does not work.
// google and find the import statement required. Add it to this file.
FileUtils.deleteQuietly(path.toFile());

// use call saveAsTextFile on your output JavaPairRDD to save your results.
// Pass in the variable holding your simple string

output.saveAsTextFile(outputFolder);
// close your spark context
sparkContext.close();
}

/**
* Says hello to the world.
* @param args The arguments of the program.
*/

public static void main(String[] args) {
    // System.out.println("Hello World!");
    if (args.length == 0) {
        System.out.println("No files provided.");
        System.exit(0);
    }
    sortWord(args[0]); 
}
}
  
