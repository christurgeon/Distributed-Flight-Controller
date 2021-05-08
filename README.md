# Distributed Airplanes
A distrubuted way of calculating information on airplanes using the Actor model and SALSA language.

The program calculates nearest neighbor via nautical distance, DCPA which is the minimum distance at the closest point of arrival, and TCPA which is the time from the initial position to the closest distance. Formula for calculating these values was taken from this NASA Paper found [here](https://ntrs.nasa.gov/api/citations/20150000558/downloads/20150000558.pdf). 

Each file contains a flight ID, latitude, longitude, trak, horizontal speed, and altitude.

## Authors
Chris Turgeon
Daniel Tabin 

## Running the Program
<ol>
<li> Ensure your terminal's working directory is the directory above the "src" directory</li>
<li> Compile salsa files</li>
<li> Compile java files</li>
<li> Execute the program with command line arguments </li>
</ol>
```
java -cp salsa1.1.5.jar;. salsac.SalsaCompiler src/*.salsa
javac -cp salsa1.1.5.jar;. src/*.java
java -cp salsa1.1.5.jar;. src/Main [input.txt] [theaters.txt] [nameserver]
```

The optional command like arguments are the input file, the file listing the locations of theaters, and the ip address of the name server.  While the program runs with zero, one, two, or three command line args, the order matters.  If just one argument is given, it must be the input.txt, the second must be theaters.txt and the third argument must be the nameserver.


## Additional Instructions for Running with Theaters

You will need to open (```number_of_theaters + 1```) terminals, which all are in the directory above the "src" directory.

On your first terminal, type in:
```
java -cp salsa1.1.5.jar;. wwc.naming.WWCNamingServer
```

On three different terminals [assuming you are using our theaters.txt file], type in:
```
java -cp salsa1.1.5.jar;. wwc.messaging.Theater
```
```
java -cp salsa1.1.5.jar;. wwc.messaging.Theater 4041 
```
```
java -cp salsa1.1.5.jar;. wwc.messaging.Theater 4042
```

These correspond to the theaters you have in your theaters.txt file, which should  again be in the directory above the "src" directory.

Finally run as outlined above.

All of the program output showed up in this main terminal, and all the process information showed up in the first terminal that we opened.

## Other Notes:
Our program creates an actor for every plane, called a "PairEngine" given us \~n actors where n is the number of airplanes.  Beforehand, we wanted to make an actor for every pair of Airplanes, thus giving us ```n(n-1)``` total actors.  While we feel this is a good theoretical distributed solution allowing for maximal parallelization, this does not run very efficiently for large n on a single laptop.  This is due to the way SALSA treats each actor as a java thread.  In a real world situation, the RAM of a single computer would not be enough to run the n^2 solution for large n.  Additionally, large inputs such as the 1500 nautical mile input run very slowly.

Additionally, our program expects well-formed input files. The given number of actors are allocated based on the integer in the first line of the file denoting the number of flights. If there is a dupliate flight or empty line then that line is skipped and an actor is deallocated (under the assumption that the malformed line _should_ hold data for a unique flight). Furthermore, the program expects that two or more flights exist in the file. 
