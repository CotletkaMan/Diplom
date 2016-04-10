Knowledge Pipeline Installation for UNIX & WNT


In order to use the Knowledge Pipeline on UNIX you will
need to setup a few environment variables and other details.
The Pipeline system is using the TAO CORBA implementation,
and includes the appropriate shared libraries in order to execute
the server.  I have not included much for other CORBA tools
or utilities.

Notes:  Check out the doc directory for a preliminary version
of the Pipeline "Head Start Guide" in the file kpl.html

Assumptions:

  It is assumed that Java 1.2 and Java3d are installed on
  the system in order to run the demo programs.  It assumes 
  that the install location for Java 1.2 is "/opt/java1.2" for
  UNIX and c:\jdk1.2.2 on WNT.  This will also work with Java
  1.3.

  It assumes that the path for installing the kpl directory
  is /usr/local/kpl on UNIX and c:\kpl on WNT.  
  If this is changed then some of the 
  details below will also need to change.  

  It assumes that UG V18 is installed on the machine.  The
  server does use various shared libraries from UG.  It assumes that
  the path environment variable is set to include the UG 
  library directory in the search path.


Initial Configuration
  Step 1:
    Add the following environment variable to the UNIX system or
    the equivalent command in WNT using set or environment settings.

    CORBA_NAMING_IOR_FILE=/usr/local/kpl/naming_ior.txt

    This is somewhat optional.  If you don't want to create an
    environment variable you can just type the command into the
    shell just before starting the server.  For example:

    export CORBA_NAMING_IOR_FILE=/usr/local/kpl/naming_ior.txt
    /usr/local/kpl/bin/ug_server Fusion


  Step 2:
    Change the "PATH" environment variable in UNIX or WNT to include
  
    /usr/local/kpl/bin/hpp          - for UNIX
    c:\kpl\bin\wnti;c:\kpl\lib\wnti                 - for WNT

    This is also somewhat optional especially on WNT since you
    could put the dll files in some other location that is currently
    in the search path.  On WNT you are adding paths for both 
    the libraries and for the executable.

   
  Step 3:  for UNIX
    Change the SHLIB_PATH variable to include /usr/local/kpl/lib.

    export SHLIB_PATH=/usr/ugv170/ugii/libs:/usr/local/kpl/lib/hpp

    Without a command like this, or moving the shared libraries 
    from the /usr/local/kpl/lib/hpp directory to somewhere that is
    visible to the system, the server will not run.


Basic Steps to start the Knowledge Pipeline server.

  1) Start the name server (tnameserv.exe) which is a 
     java program found under /opt/java1.2/bin or in the bin
     directory on WNT for jdk1.2.2
     This program can be started in the background.
     A unix command might be:

       tnameserv -ORBInitialPort 1050 >/usr/local/kpl/naming_ior.txt

     On WNT
  
       tnameserv -ORBInitialPort 1050 >c:\kpl\naming_ior.txt

     When the program starts it will print out the IOR
     object string for the name service and store that into
     a file called "naming_ior.txt".  This IOR needs
     to be captured in an environment variable before
     running the ug_server.  The output in the file will 
     look something like this:

     Initial Naming Context
     IOR:00000000000002849444c3af6d672e6f727674f4354f74e672616d900f1000018ded
     af0000d00af98000000081749937780000ef999837832810cf000000018afabcafe00000
     002ac448004f000000000000
     TransientNameServer: setting port for initial object references to: 1050

     Note:  The IOR is actually one long string, but broken here into several
     lines for readablity.

     The block starting with IOR: through the hex number values is the string
     version of IOR.  Note that the name of the output file in the tnameserv
     command line is the same as the name used with the environment variable 
     CORBA_NAMING_IOR_FILE.  If you change one you need to change the other.

     The pipeline server uses the IOR to register with the name service.
     A Java application can then use the name service to find the Pipeline
     server.

     You are not restricted to just using tnameserv as the naming server.
     I tend to like it because it is easy for Java 1.2 to find and interface
     with tnameserv.

  
  2) Run the pipeline server (ug_server).  This program is also
     executed from a command prompt.  The server takes
     one parameter for the name of the server.  For the demo cases
     the server name is assumed to be "Fusion".  This is also the
     default if the parameter is not included.

       /usr/local/kpl/bin/ug_server Fusion

     The name of the server is used with the naming service in order
     to locate a particular server on the network.

     The server will read the IOR from the CORBA_NAMING_IOR_FILE and 
     then register its name with the naming service.

     After running the server you will get a message that the KF
     Pipeline server is running on a particular host.  Once
     you get this message you can begin to use the server.


Running Demos:

  There is a demo directory containing a Java application that
  will use the Server.  To run the demo VDE application:

   cd /usr/local/kpl/demo
   java VDE -host <name_server_host>

      where <name_server_host> is either the name or the IP
      address where tnameserv was started.


   The application allows you to browse the knowledge tree for
   a UG part.   You can use "File->Open" to open a UG part file
   on the server - and the VDE should be roughly the same as
   the tree in interactive UG.
