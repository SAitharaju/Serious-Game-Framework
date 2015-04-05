Serious Game Framework
======================

A framework targeted at university professors and other educators to create simple Serious
Games and let them be played by their students and pupils.

## Table of Contents
- [Features](#)
    - [Game](#)
    - [Roles](#)
        - [Game designers](#)
            - [Learning analytics](#)
        - [Players](#)
            - [Learning analytics](#)
                - [Session Summary](#)
            - [Motivation](#)
                - [Highscore](#)
                - [Badges](#)
                - [Experience](#)
        - [Administrator](#)
- [Installation guide](#)
- [Development](#)
- [Planned Features](#)
- [Contributors](#)
- [License](#)

## Features

We want to give educators the possibility to create Serious Games including course
relevant material for their students, to allow for a refreshing, new teaching and learning
experience in the context of the new medium internet.

The application consists of three components:

The Serious Game Framework frontend to create and play games, as well as to view your
own profile as a player, game designer or administrator.

The second component is a heavily modified version of [GLEANER: Game LEarning ANalytics for Educational Research](http://e-ucm.github.io/gleaner/) created by [Ángel Serrano-Laguna](https://github.com/anserran/gleaner-frontend) and maintained by the [e-ucm eLearning group](http://www.e-ucm.es/). GLEANER provides us with an interface to interact with an underlying MongoDB.

As we award certain user actions with badges, we make use of [OpenBadges](http://openbadges.org/) to allow players to push their earned badges to their OpenBadges Backpack.

## Game

The concept of the games is rather simple. When entering a game, the player will find four
galleries, each filled with tiles containing a keyword of the game's topic. The player has
to drag and drop one tile of each gallery into a determined slot in the middle of the
screen. Tiles can only be put into their predetermined slot. Upon entering a tile into
each slot it is revealed to the player if his answer was correct or not. The inserted
tiles are then discarded and the user can continue with the rest of the tiles.

## Roles

### Game designers

Even though not feature complete, we plan to give game designers full control over the
creation of the games in the context of dragging and dropping tiles into predetermined
slots. We started in the context of Medical Education, however the basic concepts of the
game could be applied to any topic given the proper tiles and connections.

#### Learning analytics

After a user has designed a game, he can view statistics to these games on his profile. At
the current point of time, statistics can be viewed in pie and bar charts, whereas the
created game can be selected through a dropdown menu. In addition, if some levels were
solved exceptionally bad (more than 50% wrong answers with more than one answer total),
the designer will see a notice on the profile of up to the 10 worst levels. The gives the
educator more information about which topics might not have been conveyed as good and
require more attention in class.

As the feature of creating games has not been completed as off yet, the corresponding tab
on the profile page is for demonstration purposes only.

### Players

A user can login via the OpenID Connect button on the top left of the main page. This leads the user to the [Learning Layers OIDC page](https://api.learning-layers.eu/o/oauth2/login). From here a user can either login or create a new account.

After a game has been created, users can access the game via the main page. If the player
is logged in each completed level will save one record to the database along with the
result: *correct* or *wrong*. The player can then access these statistics and more via his
profile page, which is only available if the user is logged in.

#### Learning analytics

A player can view his saved statistics via the Profile tab on the main page. Statistics
are shown per game, so if a player has participated in two games he can set the viewed
game via a Dropdown menu. The statistics are shown in a Pie chart by default and can be
toggled between the Pie chart and a Bar chart view. Additionally, the top ten (at most)
worst levels are shown to the player to reveal learning deficits. A worst level is
determined by more than 50% wrong answers with more than one answer in total.

##### Session Summary

Throughout one's session the statistics are also saved. So when a player wants to close
the application window an alert will pop up to show the number of correct and wrong
answers for this play session.

#### Motivation

We have added multiple mechanisms to keep players motivated.

##### Highscore

The highscore is determined by the user's correct and wrong answers; a correct answer will
net the player 5 points whereas a wrong answer will subtract 2 points from the highscore.

In addition, a player will see the next two (at most) players above him to have an
additional motivation, and the two (at most) players beneath him, to see what other
players have achieved. To keep the Highscore ladder anonymous (as the players should
could know each other from the actual lectures) only the name of the player looking at his
profile will be shown. The other positions will be shown as (from top to bottom): _"Your
next Challenge"_, _"Your next Milestone"_, _"You've beaten this guy"_, _"...and this
guy"_.

##### Badges

Certain actions can award the player with a Badge. At the moment these actions include:

+ A Badge for completing each of the two pre existing games
    + _Tutorial_
    + _Hormones_
+ _Basic Elearner_, for clicking the elearning link three times
+ _Correctemundo_, for answering ten levels correctly
+ _Please Study_, for answering a game with more wrong than correct answers

Upon achieving a badge for the first time a sound will be played to support the player's
accomplishment. In addition, the user has the possibility to add the Badge to his Mozilla
Backpack as we are using the OpenBadges. However, the process of pushing the badge to
the backpack is only activated once, namely after earning a badge for the first time.
Nevertheless, badges can be _earned_ multiple times, with the exact number appearing next
to the badge on the profile.

##### Experience

In addition to the badge system, we have added an experience system to the application.
Through his actions each user can earn experience points. The experience is made up as
follows:

+ 150 points per created game
+ 10 points per earned badge
+ Highscore
    + 5 points per correct answer
    + -2 points per wrong answer
+ 1 point per elearning link clicked
+ 0.5 points per login

With his experience a player can rise in his experience level: Each user starts as a **Total Noob** at level 0, and climbs from **Beginner** over **Experienced Elearner**, **Professional** and **Expert** to **Master**. Each level requires more and more experience points and therefore more and more actions from the user. It starts with 100 points to reach Level 1, and then 150 additional points for the next, then 250, then 500 and finally 1000 points for Level 5 **Master**. Each level is accompanied by an image which gets more and more impressive as the user climbs in level. Additionally an experience bar indicating the users progress can be found on his profile.

### Administrator

An administrator (for example a developer) has access to all statistics of all games to
see how the application is used.

## Installation guide

+ Install NodeJS
    + We recommend running v0.10.30, as there are often compatibility problems with
      node_modules.
+ Install MongoDB
+ Go to folder gleaner-frontend within lib folder
    + Install dependencies with ```npm install```
    + Make sure that the _node\_modules_ as well as _bower\_components_ are installed
+ [Create new user in gleaner-frontend ```node bin/install <username\> <password\>```]
+ Start gleaner-frontend with ```node app/app.js```
+ [Login]
+ Navigate to localhost:3000, where the node application was started
    + Create a new Game
    + Create a new Version for this game
+ Copy the Tracking Code to _Serious-Game-Framework/lib/gleaner-tracker.js_
    + The code line is high up in the file, a comment will mark the correct line
+ Copy the first 24 characters of Tracking Code to _gleaner\_data/lib/traces\_.js_
    + A comment will mark the correct line
+ After changing the configuration, restart the gleaner-frontend server
    + ```CTRL+C``` to terminate the server, ```node app/app.js``` to restart

## Development

The main files of this project are the _serious\_game\_framework.js_, which contains the game logic as well as functions to interract with the Gleaner API and build the user profile, and _collector.js_ of the gleaner-frontend module gleaner-data, which contains all functions to calculate the statistics, the highscore and the experience level.

## Planned Features

We are looking to...

+ finish the game designer view to allow educators to create and publish games
+ add an optional division per day for all statistics.
+ add more usage statistics for administrators.
+ add a possibility to re-add badges to the Mozilla Backpack.
+ add a Feedback form to the application.
+ add more (meaningful) badges.


## Contributors

Simon Grubert, Serious Game Framework Base  
[Marko Kajzer](mailto:marko.kajzer@rwth-aachen.de), Learning Analytics and Motivation  
Marc Treiber, Badge Design and Realization

Special thanks to the team behind [GLEANER](http://e-ucm.github.io/gleaner/).

## License

Copyright (c) 2014 Simon Grubert & Marko Kajzer, Advanced Community Information
Systems (ACIS) Group, Chair of Computer Science 5 (Databases & Information Systems),
RWTH Aachen University, Germany All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are
permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of
conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice, this list of
conditions and the following disclaimer in the documentation and/or other materials
provided with the distribution.

* Neither the name of the {organization} nor the names of its contributors may be used to
endorse or promote products derived from this software without specific prior written
permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
"AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED
TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A
PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED
TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
