# Refactored Hockey Time Keeper's Database Program

This document outlines the design for a database used by a hockey timekeeper which can be used to store roster information, as well as individual player information, for both Skaters and Goalkeepers. The database can also monitor/update both Skater statistics (such as position, goals, assists, and points) and Goalie statistics (such as shots against and goals against).

## Public Interface

This program consists of six classes and two subclasses: the `Player` class, the `Skater` class (extends the `Player` class), the `Goalie` class (extends the `Player` class), the `PlayerList` class, the `Main` class, the `TableFactory` class, the `TeamReader` class, and the `TeamWriter` class. Each of these classes and their respective subclasses will be explained in further detail below, starting with the `Player` class.

### The Player Class

The Player Class is an abstract class that specifies the general roster information that a new `Player` object will have. The roster information includes the player's:
 * Name
 * Date of Birth
 * Hometown
 * Weight
 * Height
 * and Jersey Number

The Player Class contains one consstrcutor method which accepts the player's roster information as parameters for a new `Player` object. This constrcutor method is abstract and is only meant to be used by the subclasses of the `Player` class, the `Skater` class and the `Goalie` class, in order to store common roster data that both types of `Player` objects share.

### The Skater Class

The `Skater` class extends the `Player` class and stores both position information and play information for each Skater.
  
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
* `recreation`- serves to add a new Skater, this time including both roster and play information using the super class's constructor and additonal play information passed as parameters. (i.e. if a player gets traded to a new team mid-season, they will keep their pre-existing stats)
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

### The Goalie Class

The `Goalie` class extends the `Player` class and stores both position information and play information for each Goalie.

The information stored by the `Goalie` includes:
 * Poition (Goalie)
 * Shots Against
 * Goals Against
 * Shutouts
 * Games Played
 * Minutes Played
 * Goals against average
 * Save Percentage

The `Goalie` class contains two constructors:

 * `roster` serves to add a new Goalie to the team list using only their roster information through the use of the super class's constructor. All play information will be set to zero
 * `recreation` serves to add a new Goalie, this time including both roster and play information/statistics using the super class's constructor and additonal play information passed as parameters.

The `Goalie` class contains the following accessor methods:

 * `getShotsA`- returns the (int) number of shots taken against the goalie
 * `getGoalsA` - returns the (int) number of goals scored against the goalie
 * `getShutouts` - returns the (int) number of shutout games the goalie has
 * `getMinutesPlayed` - returns total minutes (int) played by the goalie
 * `getGGA` - returns the goals against average (double) statistic of the goalie
 * `getSV` - returns the save percentage (double) statistic of the goalie

The `Goalie` class also contains the following methods:
 * `recordShotA` - records a shot taken against the goalie by incrementing the Shots Against value by one
 * `recordGoalA` - records a goal scored against the goalie by incrementing the Goals Against value by one
 * `recordMinutes` - records the total number of minutes played by the goalie in one game and adds it to the total Minutes Played value and increments the Games Played value by one
 * `recordSOMinutes` - records the total number of minutes played by the goalie in a shoutout game and adds it to the total Minutes Played value and increments both the Games Played value and the Shutout value by one
 
Note: The Values of Goals against average and Save percentage will be derived from the values of the other statistics.
 * Goals against average - will be derived by multiplying Goals Against by 60 Minutes and dividing the product by the total minutes played by the goalie
 * Save percentage - will be derived by dividing goals against by goals saved, and subtracting the result from 1
