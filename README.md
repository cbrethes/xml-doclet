A doclet to output javadoc as XML Thats Works with JDK > 1.8 
============================================================

> This library is a fork from [MarkusBernhardt Xml-Doclet Github Project](https://github.com/MarkusBernhardt/xml-doclet) that doesn't work with jdk > 1.8
> I just made a dirty quick fix to make it works again and renamed package to be able to publish it on Maven Central (asap)
> Tested with jdk 11 only

Until its not available on Maven Central, make this maven plugin works locally to you :
1. checkout [git repo](https://github.com/cbrethes/xml-doclet.git) 
2. go into the project and generate source with `mvn jaxb2:xjc`
3. finally install it to your local maven repo with `mvn clean install`
4. follow the usage below (basically replace `markusbernhardt` by `cbrethes` in plugin groupId)

This library provides a doclet to output the javadoc comments from Java source code to a XML document.

The name, some ideas and most unit tests were shamelessly stolen from the
[xml-doclet](http://code.google.com/p/xml-doclet) library by Seth Call.

Usage
-----

If you are using maven you can use this library by adding the following report to your pom.xml:

    <project>
    	...
    			<plugin>
    				<groupId>org.apache.maven.plugins</groupId>
    				<artifactId>maven-javadoc-plugin</artifactId>
    				<executions>
    					<execution>
    						<id>xml-doclet</id>
						<phase>prepare-package</phase>
    						<goals>
    							<goal>javadoc</goal>
    						</goals>
    						<configuration>
    							<doclet>com.github.cbrethes.xmldoclet.XmlDoclet</doclet>
    							<additionalparam>-d ${project.build.directory} -filename ${project.artifactId}-${project.version}-javadoc.xml</additionalparam>
    							<useStandardDocletOptions>false</useStandardDocletOptions>
    							<docletArtifact>
    								<groupId>com.github.cbrethes</groupId>
    								<artifactId>xml-doclet</artifactId>
    								<version>1.0.5</version>
    							</docletArtifact>
    						</configuration>
						</execution>
    				</executions>
    			</plugin>
    	...
    </project>
    
Use 'mvn package' with maven.
If you are not using maven, you can use the [jar-with-dependencies](http://search.maven.org/remotecontent?filepath=com/github/cbrethes/xml-doclet/1.0.5/xml-doclet-1.0.5-jar-with-dependencies.jar), which contains all required libraries.

    javadoc -doclet com.github.cbrethes.xmldoclet.XmlDoclet \
    -docletpath xml-doclet-1.0.5-jar-with-dependencies.jar \
    [Javadoc- and XmlDoclet-Options]

A Makefile target to generate xml from both the production and test code:


    javadoc:
	mkdir -p target/production target/test
	CLASSPATH=$$(echo $$(find ~/.m2/repository/ -name '*.jar'|grep -v jdk14 )|sed 's/ /:/g')\
     javadoc -doclet com.github.cbrethes.xmldoclet.XmlDoclet -sourcepath src/main/java -d target/production org.rulez.demokracia.PDEngine
	CLASSPATH=$$(echo $$(find ~/.m2/repository/ -name '*.jar'|grep -v jdk14 )|sed 's/ /:/g')\
     javadoc -doclet com.github.cbrethes.xmldoclet.XmlDoclet -sourcepath src/test/java -d target/test org.rulez.demokracia.PDEngine

If you want more control and feel adventurous you could you use this [jar](http://search.maven.org/remotecontent?filepath=com/github/cbrethes/xml-doclet/1.0.5/xml-doclet-1.0.5.jar) and provide all required libraries from this [list](DEPENDENCIES.md) on your own.

Options
-------

    -d <directory>            Destination directory for output file.
                              Default: .
                              
    -docencoding <encoding>   Encoding of the output file.
                              Default: UTF8
                              
    -dryrun                   Parse javadoc, but don't write output file.
                              Default: false
                              
    -filename <filename>      Name of the output file.
                              Default: javadoc.xml

