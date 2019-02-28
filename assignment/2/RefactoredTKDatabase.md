# Refactored Hockey Time Keeper's Database Program

This document outlines the design for a database used by a hockey timekeeper which can be used to store roster information, as well as individual player information, for both Skaters and Goalkeepers. The database can also monitor/update both Skater statistics (such as position, goals, assists, and points) and Goalie statistics (such as shots against and goals against).

## Public Interface

This program consists of six classes and two subclasses: the 'Player' class, the 'Skater' class (extends the 'Player' class), the 'Goalie' class (extends the 'Player' class), the 'PlayerList' class, the 'Main' class, the 'TableFactory' class, the 'TeamReader' class, and the 'TeamWriter' class. 
