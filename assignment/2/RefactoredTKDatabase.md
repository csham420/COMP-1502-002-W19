# Refactored Hockey Time Keeper's Database Program

This document outlines the design for a database used by a hockey timekeeper which can be used to store roster information, as well as individual player information, for both Skaters and Goalkeepers. The database can also monitor/update both Skater statistics (such as position, goals, assists, and points) and Goalie statistics (such as shots against and goals against).

## Public Interface

This program consists of several classes and subclasses: the `Player` class, the `Skater` class (extends the `Player` class), the `Goalie` class (extends the `Player` class), the `PlayerList` class, the `Main` class, the `TableFactory` class, the `RosterTable` class (extends the `TableFactory` class), the `SkaterStatTable` class (extends the `TableFactory` class), the `GoalieStatTable` class (extends the `TableFactory` class), the `TeamReader` class, and the `TeamWriter` class. Each of these classes and their respective subclasses will be explained in further detail below, starting with the `Player` class.

### The Player Class
The Player Class is an abstract class that specifies the general roster information that a new `Player` object will have. The roster information includes the player's:
 * Name
 * Date of Birth
 * Hometown
 * Weight
 * Height
 * and Jersey Number

The Player Class contains one constructor method which accepts the player's roster information as parameters for a new `Player` object. This constructor method is abstract and is only meant to be used by the subclasses of the `Player` class, the `Skater` class and the `Goalie` class, in order to store common roster data that both types of `Player` objects share.

The `Player` class also contains the following methods:
 * `getName` - returns the name of the Player
 * `getDoB` - returns the Player's date of birth
 * `getTown` - returns the Player's Hometown
 * `getWeight` - returns the Player's weight
 * `getHeight` - returns the Player's height
 * `getNumber` - returns the Player's jersey number

### The Skater SubClass
The `Skater` subclass extends the `Player` class and stores both position information and play information for each Skater.
  
The information stored by the `Player` class includes: 
* position (left wing (LW), right wing (RW), centre (C), or defense (D)
* the Skater's goals
* assists
* points (goals + assists)
* shots
* shooting percentage
* power play points, shots, and assists.
  
The `Skater` class contains three constructors:

* `roster`- serves to add a new Skater to the team list using only their roster information through the use of the super class's constructor. All play information will be set to zero (i.e. if a new player has signed to the team during the pre-season)
* `recreation`- serves to add a new Skater, this time including both roster and play information using the super class's constructor and additional play information passed as parameters. (i.e. if a player gets traded to a new team mid-season, they will keep their pre-existing stats)
* `copy`- copies an existing `Skater` object and its pre-existing values into a new, identical object

The `Skater` class contains the following assessors:

* `getPoints` - returns the total number (int) of points the skater has accumulated (goals + assists)
* `getAssists` - returns the total number (int) of assists the skater has made
* `getGoals` - returns the total number (int) of goals the skater has scored
* `getPPPoints` - returns the total number (int) of Power Play points the skater has accumulated (Power play goals + power play assists)
* `getPPAssists` - returns the total number (int) of Power Play assists the skater has made
* `getPPGoals` - returns the total number (int) of Power Play goals the skater has scored
* `getTotalShots` - returns the total number (int) of shots taken by the skater
* `getShootPercent` - returns the skater's overall shooting percentage (double)

The `Skater` class also contains the following methods:

* `recordGoal` - the selected Skater's goals, points, and total shots increase (through the `recordShot()` method) by one, the user is also prompted to record assists for up to two players through the `recordAssist()` method
* `recordAssist` - allows the user to record assists for up to two skaters, the selected player's assists and points increase by one
* `recordPPGoal` - the selected skater's Power Play goals, Power Play points, and total shots (through the `recordShot()` method) increase by one, the user is also prompted to record Power Play assists for up to two players through the `recordPPAssist` method
* `recordPPAssist` - allows the user to record Power Play assists for up to two skaters, the selected player's Power Point assists and Power Play points increase by one
* `recordShot` - increases the skater's total shots value by one
* `calcShootPercent` - this method calculates the skater's shooting percentage by dividing the sum of goals and Power Play goals by the total number of shots taken
* `rosterToString` - used to print out the object's roster information
* `playToString` - used to print out the object's roster and play information

### The Goalie SubClass
The `Goalie` subclass extends the `Player` class and stores both position information and play information for each Goalie.

The information stored by the `Goalie` includes:
 * Position (Goalie)
 * Shots Against
 * Goals Against
 * Shutouts
 * Games Played
 * Minutes Played
 * Goals against average
 * Save Percentage

The `Goalie` class contains two constructors:

 * `roster` serves to add a new Goalie to the team list using only their roster information through the use of the super class's constructor. All play information will be set to zero
 * `recreation` serves to add a new Goalie, this time including both roster and play information/statistics using the super class's constructor and additional play information passed as parameters.

The `Goalie` class contains the following accessor methods:

 * `getShotsA`- returns the (int) number of shots taken against the goalie
 * `getGoalsA` - returns the (int) number of goals scored against the goalie
 * `getShutouts` - returns the (int) number of shutout games the goalie has
 * `getMinutesPlayed` - returns total minutes (int) played by the goalie
 * `getGGA` - returns the goals against average (double) statistic of the goalie
 * `getSV` - returns the save percentage (double) statistic of the goalie

The `Goalie` class also contains the following methods:
 * `recordShotA` - records a shot taken against the goalie by incrementing the Shots Against value by one
 * `recordGoalA` - records a goal scored against the goalie, as well as a shot against the Goalie, by incrementing the Goals Against  and Shots Against values by one
 * `recordMinutes` - records the total number of minutes played by the goalie in one game and adds it to the total Minutes Played value and increments the Games Played value by one
 * `recordSOMinutes` - records the total number of minutes played by the goalie in a shootout game and adds it to the total Minutes Played value and increments both the Games Played value and the Shutout value by one
 * `rosterToString` - used to print out the object's roster information
 * `playToString` - used to print out the object's roster and play information
 
Note: The Values of Goals against average and Save percentage will be derived from the values of the other statistics.
 * Goals against average - will be derived by multiplying Goals Against by 60 Minutes and dividing the product by the total minutes played by the goalie
 * Save percentage - will be derived by dividing goals against by goals saved, and subtracting the result from 1

### The Player List Class
The `PlayerList` class will contain an Array List that will be populated with `Player` objects in order to access all player information simultaneously and can be used to add new `Players` (`Goalie` and/or `Skater` objects) to the list, as well as update individual player statistics. Player's jersey numbers will be used to search unique players individually.

The `PlayerList` class contains the following methods:

* `addSkater`- allows the user to add a new `Skater` object to the existing array list and gives the user the option to use one of two constructors in the `Skater` class by allowing the user to choose if they wish to: 1) add a player with no pre-existing stats or 2) add a player with existing stats
* `addGoalie`- allows the user to add a new `Goalie` object to the existing array list and gives the user the option to use one of two constructors in the `Goalie` class by allowing the user to choose if they wish to: 1) add a player with no pre-existing stats or 2) add a player with existing stats
* `addGoal` - passes the Skater's jersey number as a parameter and uses the `recordGoal` method from the `Skater` class to record a goal as well as make an addition to the Skater's total shots, and prompts the user to add assists for up to two Skater using the `recordAssist` method from the `Skater` class
* `addPPGoal` - passes the Skater's jersey number as a parameter and uses the `recordPPGoal` method from the `Skater` class to record a Power Play goal as well as make an addition to the Skater's total shots, and prompts the user to add Power Play assists for up to two Skaters using the `recordPPAssist` method from the `Player` class
* `addShotA` - passes the Goalie's jersey number as a parameter and uses the `recordShotA` method from the `Goalie` class to record a shot against the Goalie
* `addGoalA` - passes the Goalie's jersey number as a parameter and uses the `recordGoalA` method from the `Goalie` class to record a shot and a goal against the Goalie
* `addMinutes` - passes the Goalie's jersey number as a parameter and uses the `recordMinutes` method from the `Goalie` class to record the total amount of minutes a Goalie has played during one game, and increases the value of the Games Played value by one
* `addSOMinutes` - passes the Goalie's jersey number as a parameter and uses the `recordSOMinutes` method from the `Goalie` class to record the total amount of minutes a Goalie has played during one shutout game, and increases the value of the Games Played value and the Shutout value by one 
* `searchPlayer` - a search method used to find a player's roster info and stats using their jersey number
* `listSkaters` - a method used to print out a list of every `Skater` object stored in the `PlayerList` object along with their roster and play information 
* `listGoalies` - a method used to print out a list of every `Goalie` object stored in the `PlayerList` object along with their roster and play information
* `listPlayers` - a method used to print out a list of all `Player` objects stored in the `PlayerList` object along with their roster and play information (includes both `Skater` and `Goalie` objects)

### The Menu Class
The `Menu` class is a simple class that allows the user to interact with the database program.

The `Menu` class will prompt the user to do the following:

* List all of the skaters' roster information (not including statistics)
* List all of the goalies' roster information (not including statistics)
* List all of the skaters' statistics information
* List all of the goalies' statistics information
* Add a new Skater
* Add a new Goalie
* Record a shot
* Record a goal by a Skater (with an option to add up to two assists)
* Record a Power Play goal by a Skater (with an option to add up to two Power Play assists)
* Record a game played and total minutes played in the game by a goalie
* Record a shutout in a game, as well as total minutes played during the shutout by a goalie

### The TableFactory Class
The role of the `TableFactory` class is to read data from a specified `PlayerList` object and return a String containing the data in one of several formats:
 * A roster information list for all `Player` objects (both `Goalie` and `Skater` objects included) in the `PlayerList`
 * A list of all `Skater` objects in the `PlayerList` and their play information
 * A list of all `Goalie` objects in the `PlayerList` and their play information

### The RosterTable Class
The `RosterTable` is a subclass of the `TableFactory` class. The purpose of the `RosterTable` class is to accept a `PlayerList` object and return a formatted `String` of roster information for all the `Player` objects in the specified `PlayerList`.

The `RosterTable` class contains the following methods:
* `RosterTable` - constructor method that will accept a `PlayerList` object as a parameter
* `createRosterString` - this method will read the roster info from the `PlayerList` and return a table containing the roster information as a `String`

### The SkaterStatTable Class
The `SkaterStatTable` is a subclass of the `TableFactory` class. The purpose of the `SkaterStatTable` class is to accept a `PlayerList` object and return a formatted `String` of `Skater` statistic/play information for all the `Skater` objects in the specified `PlayerList`.

The `SkaterStatTable` class contains the following methods:
* `SkaterStatTable` - constructor method that will accept a `PlayerList` object as a parameter
* `createSkaterString` - this method will read the statistics info from the `PlayerList` and return a table containing the play information as a `String`

### The GoalieStatTable Class
The `GoalieStatTable` is a subclass of the `TableFactory` class. The purpose of the `GoalieStatTable` class is to accept a `PlayerList` object and return a formatted `String` of `Goalie` statistic/play information for all the `Goalie` objects in the specified `PlayerList`.

The `GoalieStatTable` class contains the following methods:
* `GoalieStatTable` - constructor method that will accept a `PlayerList` object as a parameter
* `createGoalieString` - this method will read the statistics info from the `PlayerList` and return a table containing the play information as a `String`

### The TeamReader Class
The `TeamReader` class is a simple class that uses the data presented in a specified file to return a `PlayerList` object containing the data found in the file. This is done by the class' only method, the `read` method, which is a public-static helper method.

### The TeamWriter Class
The `TeamWriter` class is another simple class that accepts a specified output file and a `PlayerList` object and writes the data contained in the passed `PlayerList` to the output file. This is done by the class' only method, the `write` method, which is a public-static helper method.

## Private Implementation

### The Player Class

The `Player` class needs to store the following roster information:
* name - String
* date of birth - String
* home town - String
* weight - String
* height - String
* jersey number - String
* position - enum {LW, RW, C, D}

#### Important Methods
 * `toString` - will return a `String` containing the Player's roster information in a specific format
 
There are also accessor methods (`get()`) for each variable included in the roster information.

### The Skater SubClass

The `Skater` subclass will inherit the roster information from the `Player` superclass and encapsulate/record all of the Skater's statistic/play information 

The `Skater` subclass will store the following play information:
* number of goals scored by the player - double
* number of assists made by the player - int
* number of Power Play goals the player has registered - int 
* number of Power Play assists the player has registered - int
* number of shots the player has registered - double

There are also accessor methods (`get()`) for each variable included in the play information.

Each skater's points can be directly derived from the goals and assists of each skater (including the Power Play counterparts of each statistic). I also chose to store values of the goals and shots as doubles, as they will later be used to calculate the skater's shooting percentage, which would be more accurately recording in the form as a `double` as opposed to an `int`. When it comes to printing out or displaying the goals or shots individually, they can be casted into the `int` form in order to display a cleaner looking stat sheet.

When creating a new `Skater`, you have the option of providing only the roster information (through the use of the super constructor) or the roster information and play information if the `Skater` has pre-existing stats.

#### Important Methods
* `recordGoal` - a void method that should increase the skater's goal stat by one, increase the skater's total shots taken using the `recordShot` method, and prompt the `recordAssist` method which allows for up to two skaters to have their assist stat increased by one
* `recordPPGoal` - a void method identical to the `recordGoal` method, but increases the Power Play variables of the goals and assists (prompts `recordPPAssist` method) stats as opposed to the standard versions (the normal `recordShot` method is still called)
* `recordShot` - a void method that increases the skater's total shots value by one

### The Goalie SubClass

The `Goalie` subclass will inherit the roster information from the `Player` superclass and encapsulate/record all of the Goalie's statistic/play information.

The `Goalie` subclass will store the following play information:
 * the number of shots taken against the goalie - double
 * the number of goals scored against the goalie - double
 * the number of shutout games the goalie has played - int
 * the number of minutes the goalie has played - int
 * the goalie's goals against average statistic - double
 * the goalie's save percentage statistic - double 

Similar as to in the `Skater` class, the values of Shots Against and Goals Against are stored as doubles because they will later be used in calculations to derive the values of Goals Against Average and Save Percentage and storing them as doubles would help preserve accuracy.

When creating a new `Goalie`, you have the option of providing only the roster information (through the use of the super constructor) or the roster information and play information if the `Goalie` has pre-existing stats.

#### Important Methods
 * `recordShotA` - a void method that should increase the goalie's Shots Against (double) statistic by one
 * `recordGoalA` - a void method that should increase the goalie's Goals Against (double) and Shots Against (double) statistic by one
 * `recordMinutes` - a void method that should add the inputted amount of minutes played in a game to a goalie's total minutes played (int), and increments the games played (int) statistic by one
 * `recordSOMinutes` - a void method that should add the inputted amount of minutes played in a shutout game to a goalie's total minutes played (int), and increments the games played (int) and shutout games played (int) statistic by one

### The PlayerList Class

### The Menu Class

#### Important Methods
 * `listSkaters` - a method used to print out a list of every `Skater` object stored in the `PlayerList` object along with their roster and play information 
* `listGoalies` - a method used to print out a list of every `Goalie` object stored in the `PlayerList` object along with their roster and play information
* `listPlayers` - a method used to print out a list of all `Player` objects stored in the `PlayerList` object along with their roster and play information (includes both `Skater` and `Goalie` objects)
 * `addSkater` - this method adds a new `Skater` to the last index of the `PlayerList` ArrayList
 * `addGoalie` - this method adds a new `Goalie` to the last index of the `PlayerList` ArrayList
 * `addShot` this method records a shot to a specified skater
 * `addGoal` this method records a goal for a specified skater and up to two assists for two other specified skaters
 * `addPPGoal` - this method records a Powerplay goal for a specified skater and up to two power play assists for two other specified skaters
 * `addGoalA` - this method records a shot and a goal against the goalie for a specified goalie
 * `addShotsA` - this method records a shot against the goalie for a specified goalie
 * `addSOMinutes` - this method records a shutout and minutes played during the shutout for a specified goalie, as well as a game played
 * `addMinutes` - this method records minutes played for a specified goalie, as well as a game played

### The TableFactory Class

#### Important Methods
* `listAllPlayersRoster` - this method will return all the Players' roster information in a table as a `String`
* `listAllSkatersStat` - this method will return all the Skaters' statistic/play information in a table as a `String`
* `listAllGoaliesStats` - this method will return all the Goalies' statistic/play information in a table as a `String`

### The RosterTable Class

#### Important Methods
* `rosterTable` - constructor that will pass in the `PlayerList`
* `createTableString` - this method will return a `String` containing a table listing the roster information of the `Player` objects in the `PlayerList` 

### The SkaterStatTable Class

#### Important Methods
 * `SkaterStatTable` - constructor that will pass in the PlayerList
 * `createTableString` - this method will return a `String` containing a table listing the statistic/play information of the `Skater` objects in the `PlayerList`

### The GoalieStatTable Class

#### Important Methods
 * `GoalieStatTable` - constructor that will pass in the PlayerList
 * `createTableString` - this method will return a `String` containing a table listing the statistic/play information of the `Goalie` objects in the `PlayerList`

### The TeamReader Class
This class will pass in a file and get back a `PlayerList` containing Player data read in from the file

#### Important Methods
 * `loadFile` - this method will read all the data from the passed file and create the appropriate `Player` objects to populate the `PlayerClass` ArrayList

### The TeamWriter Class
This class will pass in a file and the data in the `PlayerList` will be read and written to that file 

#### Important Methods
 * `saveFile` - this method will save all the information in the `PlayerList` to an output file
