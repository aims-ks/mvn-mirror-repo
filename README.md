How to add this repo to your project
------------------------------------

Add this to your project .pom file:

```
    <repositories>
        <!-- AIMS ks maven repository on GitHub -->
        <repository>
            <id>aims-ks.mvn-mirror-repo</id>
            <name>AIMS Knowledge System Maven Mirror repository</name>
            <url>https://raw.githubusercontent.com/aims-ks/mvn-mirror-repo/master/</url>
        </repository>
    </repositories>
```

Add a jar dependency to your project

```
    <dependencies>
        <dependency>
            <groupId>edu.ucar</groupId>
            <artifactId>netcdf4</artifactId>
            <version>5.0.0-alpha3</version>
        </dependency>
    </dependencies>
```

How to deploy jar to the mvn-mirror-repo
----------------------------------------

1. Update the `mvn-mirror-repo` project
    ```
    cd /path/to/mvn-mirror-repo/
    git pull
    ```

2. Deploy the jar in this project

    Replace `/path/to` with the path to the `mvn-mirror-repo` directory on your local computer.

    Replace `[GROUPID]`, `[LIBRARY]` and `[VERSION]` with the names and version of the library you want to add to the repository.

    ```
    mvn deploy:deploy-file -Durl=file:///path/to/mvn-mirror-repo/ -DpomFile=pom.xml -Dfile=[LIBRARY]-[VERSION].jar -DgroupId=[GROUPID] -DartifactId=[LIBRARY] -Dpackaging=jar -Dversion=[VERSION]
    ```

    Since most of this info is already available in the POM file, it can be simplified to:

    ```
    mvn deploy:deploy-file -Durl=file:///path/to/mvn-mirror-repo/ -DpomFile=/path/to/pom.xml -Dfile=/path/to/[LIBRARY]-[VERSION].jar
    ```

    NOTE: Paths must be absolute.

    You can install files copied from any source as long as you have the **JAR** and the **POM** file.
    It's tempting to simply grab the files directly from your local maven repository `~/.m2/repository`
    but the mvn command refuse to deploy jar from there.
    Fortunately, you can easily trick it using a symbolic link:
    ```
    cd ~
    ln -s .m2 m2
    ```

    Examples (using the symbolic link shown above):
    ```
    mvn deploy:deploy-file -Durl=file:///home/username/projects/mvn-mirror-repo/ -DpomFile=/home/username/m2/repository/edu/ucar/netcdf4/5.0.0-alpha3/netcdf4-5.0.0-alpha3.pom -Dfile=/home/username/m2/repository/edu/ucar/netcdf4/5.0.0-alpha3/netcdf4-5.0.0-alpha3.jar
    mvn deploy:deploy-file -Durl=file:///home/username/projects/mvn-mirror-repo/ -DpomFile=/home/username/m2/repository/edu/ucar/netcdf4/4.6.10/netcdf4-4.6.10.pom -Dfile=/home/username/m2/repository/edu/ucar/netcdf4/4.6.10/netcdf4-4.6.10.jar
    ```

4. Push to GitHub
    ```
    cd /path/to/mvn-mirror-repo/
    git add -A
    git status
    git commit -m "Commit message"
    git push -u origin master
    ```
