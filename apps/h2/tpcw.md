INSTALLATION:

The process of setting up TPC-W takes the following steps:

1. Populate the database
   a. Set the dbName, and driverName variables appropriately in the file
      TPCW_Populate.java
   b. Compile the TPCW_Populate program, using the Makefile in the populate
      directory
   c. Run the TPCW_Populate program.

2. Build the TPC provided image creation program, which resides in the
   ImgGen directory
   a. The only other file needed from ../gd-1.7.2/ is gdfontg.c
   b. The Makefile must be edited for proper CFLAGS and libraries. The
      linker needs the Math-Library
   c. Running make should generate the executable tpcwIMG in the current
      directory

3. Run the image population script, which creates an image for every item
   in the database.
   a. Make sure perl is installed
   b. Change the variables $NUM_ITEMS and $DEST_DIR in the file 
      populate/populate_images sctript for your specific installation 
   c. Run the populate/populate_images script (perl populate/populate_images)

4. Copy all of the image files from the images directory into the
   same directory where the image population script created all of the
   images.

5. Install Tomcat
   a. # apt-get install tomcat7
   b. # service tomcat7 restart
   c. Look into your "/etc/init.d/tomcat7" file to find the right path of
      your CATALINA_HOME.
   d. First, create a context (or web application) called tpcw for our web
      service in Tomcat, by creating a sub-directory called tpcw under
      $CATALINA_HOME\webapps. Also create a subdirectory "WEB-INF" and
      sub-sub-directory "WEB-INF\classes" under the context root directory.

6. Build the java servlets
   a. Edit the file servlets/TPCW_Database.java, modifying the variables 
      "driverName" and "jdbcPath" for your particular DB installation.
   b. In all servlets, did replacing of ";$sessionid$" to ";jsessionid="
   c. Build the servlets, using servlets/Makefile
   d. compiled and copied servlets + classes into "WEB-INF\classes".

7. Build the RBE
   a. in rbe.java, changed ";$sessionid$" to ";jsessionid="
   b. javac rbe/* from the root tpcw directory. Make sure the . is in your
      CLASSPATH.

8. Run the RBE, see the runtpcw file for examples

java -mx512M rbe.RBE -EB rbe.EBTPCW2Factory 30 -OUT run1.m -RU 100 -MI 1000 -RD 100 -WWW http://localhost:8080/ -CUST 144000 -ITEM 10000 -TT 0.0