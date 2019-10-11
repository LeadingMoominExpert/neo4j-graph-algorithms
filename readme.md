# Installation guide for ubuntu 18.04

## Install Java
You need Java 1.8 (JDK & JRE)

`sudo apt install openjdk-8-jdk openjdk-8-jre`

## Installing and configuring Neo4j

Install Neo4j with the following commands
```
sudo su
wget --no-check-certificate -O - https://debian.neo4j.org/neotechnology.gpg.key | sudo apt-key add -
echo 'deb http://debian.neo4j.org/repo stable/' > /etc/apt/sources.list.d/neo4j.list
apt update
apt install neo4j
exit
```

Check if Neo4j is running after installation by running

`service neo4j status`

You can now access Neo4j portal at `http://localhost:7474/browser/`. Login using the default Neo4j password ‘neo4j’ and then you will be prompted to set a new password.

Move your custom database directory to `/var/lib/neo4j/data/databases` and change the owner to neo4j by running `sudo chown -R neo4j:adm graph_newname.db` (or whatever name you chose for your custom database)

The configurations file should be located at `/etc/neo4j`. Stop Neo4j by `service neo4j stop` and change the following line
`#dbms.active_database=graph.db` to `dbms.active_database=graph_newname.db` (notice removing the comment tag). Enable algo by adding the line `dbms.security.procedures.unrestricted=algo.*` to the configurations file. Then restart the service with `service neo4j start`.

## Installing Java IDE and getting the source code

Install IntelliJ IDEA Community, since it's decent and the following instructions are for it.

`sudo snap install intellij-idea-community --classic`

Get the source code to your computer

`git clone git@github.com:LeadingMoominExpert/neo4j-graph-algorithms.git`

- When opening up IntelliJ IDEA for the first time, select import project and locate the `pom.xml`-file in the previously cloned repository and click OK
- Then from `JDK for importer` select the previously installed 1.8 JDK and click next
- From profiles select `Benchmark`
- From Maven projects to import select `org.neo4j:graph-algorithms-parent:3.5.4.1
- Keep on clicking next, until you finish the import

Then just let Maven work it's magic and install all the dependencies.

## Creating executable JAR-file
You still need to create the excecutable JAR for your Neo4j to be able to used the forked functions. Use the following steps in IntelliJ IDEA

- From the main menu, select File | Project Structure `Ctrl+Shift+Alt+S` and click Artifacts.
- Click the Add button, point to JAR and select From modules with dependencies.
- For the module select `graph-algorithms-algo`
- Select the destination folder of the JAR-file to be `/var/lib/neo4j/plugins/`
- IntelliJ IDEA creates the artifact configuration and shows its settings in the right-hand part of the Project Structure dialog.
- Apply the changes and close the dialog.
- From Build select Build Artifacts and IntelliJ builds your target JAR-file to be used as a Neo4j plugin

In case of a FileNotFoundException (Permission denied), make sure your user has the rights to read and write to the target directory.

Restart Neo4j by running `service neo4j restart`. Test if you successfully imported algo to Neo4j by calling `CALL algo.list()` in the browser UI. Feel free to shut down Neo4j if you're not using it `service neo4j stop`.
