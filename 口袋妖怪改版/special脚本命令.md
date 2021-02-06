# special脚本命令



```
25 special
Arguments:
word function

Calls a special function; that is, a piece of ASM code designed for use by scripts and listed in a table of pointers.
调用特殊函数;也就是说一段 ASM 代码，该代码设计为供脚本使用，并列在指针表中。

The special table is located at 0x0815FD60

特殊表在rom 中的地址15FD60。
```





```
Special 0x0 = Cura a tu equipo (Sage)
Special 0x2 = Efecto de un warp, pantalla a negro (Sage)
Special 0x20 = Inicia batalla con otro jugador, falla por si mismo (Sage)
Special 0x21 = Inicia un intercambio con otro jugador, falla por si mismo (Sage)
Special 0x22 =Salta la confirmación/espera e inicia el intercambio (Sage)
Special 0x23 = Pregunta si deseas guardar la partida (Sage)
Special 0x3C = Abre el menu del PC (Sage)
Special 0x5F = Sirve para editar perfiles y depende de la variable 0x8004 0x"valor del 1 al 5" (Sage)
Special 0x60 = Te muestra tu perfil (el que editaste con special 0x5f), depende de los mismos valores que el anterior
(Sage)
Special 0x29 = Te da a elegir 3 Pokémon de tu equipo (Sage)
Special 0x9D = Tutorial del viejo (El que te enseña a capturar pokémon) (Sage)
Special 0x9F = Escoge un Pokémon y guarda su posición en la var 0x8004 (Sage)
Special 0x9E = Abre el menu de nombrar Pokémon, está ligado al Special 0x9F, o a la variable 0x8004 0x"slot del equipo" (Sage)
Special 0xC8 = Pantallazo de cuando pierdes (Sage)
Special 0xFB = Abre el mapa (Sage)
Special 0x7B = Comprueba si el Pokémon es recibido o no en intercambio (Sage)
Special 0x7C = Almacena el nombre del Pokémon en la variable 0x8004 (Sage)
Special 0x16F = Activa la pokedex nacional en Fire Red (Wold)
Special 0x136 = Efecto de un terrremoto en Fire Red (Wold)
Special 0x20 = Inicia batalla con otro jugador, falla por si mismo (Sage)
Special 0x21 = Inicia un intercambio con otro jugador, falla por si mismo (Sage)
Special 0x22 =Salta la confirmación/espera e inicia el intercambio (Sage)
Special 0x7B = Comprueba si el Pokémon es recibido o no en intercambio (Sage)
Special 0x7C = Almacena el nombre del Pokémon en la variable 0x8004 (Sage)
```

```
Special Description *other data*
000 Heals pokemon
001 clears the variable given in special2 or all the usual variables if in special 1

Warp Commands
002 door warp script (fade black until finished warping)
003 same screen effect as 02. Peculiar behaviour, warps to last used warp, or to center-map if last change was made by a screen transition, or trapped in Battle link if used after game start

Link Commands
020 battle trough link. Stored in the same location as all other battle data
021 link start (trading)
022 faster link start (overwrites other commands)
023 save game popup
027 updates a copy of your party at address stored at 03005008 +0x38. the number of pokemon in that party copy is stored 4 bytes before the start
028 replaces your party with the one stored with the code above. If you never used special 0x27, the party is that of the last time you loaded the game
029 although it lets you select three pokemon, it shows not where they are stored, being nowhere on the usual variables(8000-800f)
02A Nice crash sound…
032 checks the data for the Enigma Berry, stored at (03005008) + 0x30ec. if the old, not E-reader one, returns 1, else 0

Trainer Commands
034 prints a buffered message, buffered by the trainerBattle command.
035 prints a buffered message, buffered somewhere by a different command from 34. Dpad-Sensitive.
036 returns the number of times the buffered trainer was fought. If bigger than 0, usually is followed by na end signal
038 plays buffered trainer music
039 used vs-seeker. placed in given variable if it was fought before or not
03B activates battle with buffered trainer

03C activates pokestorage menu

Double battle commands
03d checks for a single viable pokemon, that is, pokemon capable to fight. returns 0x1 to given variable if only one pokemon capable of fight is present, 0 otherwise
045 checks for all viable pokemon. result stored in given variable

Profile commands
05D save game popup
05F edit profiles. Which depend on 0x8004 value *Key: 0x0 = profile 0x1 = battle quote 0x2 = uppon wining 0x3 = uppon losing quote 0x4 = nine word message 0x5 and up, nothing*
060 displays profile, same key as 5f

Pokemon Size "mini-game"
077 Buffers Heracross in buffer 0x0 and its recorded size in buffer 0x1
078 checks Heracross size. returns to given variable 0x1 if there was no heracross, 0x2 if it was smaller, 0x3 if bigger and 0x4 if the same size. Updates record automatically
079 Buffers Magicarp in buffer 0x0 and its recorded size in buffer 0x1
07A checks Magicarp size. returns to given variable 0x1 if there was no Magicarp, 0x2 if it was smaller, 0x3 if bigger and 0x4 if the same size. Updates record automatically

Name Rater commands
07B checks pokemon nickname, buffers it and put 01 in given variable if it was never nicknamed
07C buffers pokemon name indicated at 0x8004 (nickname)
07D obtained in a trade checker

080 flushes random words into buffer 2

083 counts the number of pokemon in your party and places it on a given variable
084 same as 83
085 gives the number to the last pokemon on your party, to the variable passed

Map Commands
08D displays last processed message
08e Forces map refresh, that is, puts into effect all changes made by setmaptile command
08f Returns the player map position, the X to 0x8004 and the y to 0x8005. Coordinates are the same as the ones for the map in AdvanceMap.

094 Buffers a word based on gender, big guy if boy, big girl if girl
095 Buffers other words based on gender, daughter if boy, son if girl. These two commands may be harmless leftovers from R/S

09D old man battle
Name rater commands 2

09E nickname pokemon in party indicated by 0x8004
09F chooses a pokemon for a purpose and stores its position at 0x8004. works even with eggs

Trainer House Commands
0A3 checks the Trainer Card achievements (color and stars). Place the achievement number from 0 to 7 in 0x8004, and returned in a given variable 1 if completed
0A4 Places how many achievements you have in a given variable
0a5 Buffers the rival achievement trainer name to the 0x0 buffer. Mostly, your rival name, but LT Surge, Koga and Lance also appear.

Special Battle commands
0AB makes a random battle based on the Tree wild values
0B4 after battle, ceratin variables are set. b4 reads them and places on a given variable the status at battle end. *key 0x1 = fainted, 0x7 = captured, 0x4 = escaped*

Breeding center commands
0B5 Buffers the Daycare Pokemon name to the buffers 0x0 and 0x1
0B6 daycare status *key: 0x1 = one egg, 0x2 = one pokemon, 0x3 = two pokemon*
0b7 Clear egg flag and reset timer for another egg
0b8 Creates and gives away egg
0b9 Corrects, in your party, the egg given by 0xb8, adding all learnable attacks, including egg moves
0BA seems to register a pokemon number after reading from the party(with other special) and puts it in a variable. Returns 277 for Treeko, so real, in game number.
0BB removes pokemon stored at 0x8004 from party. Places it on the daycare center address (dynamic)
0BC A selection screen for pokemon with store instead of select, identical in use to 9f
0BD two slot selection screen that allows you to choose one of two pokemon on the daycare center. If no pokemon is stored there, blank name, lv0 male will appear.
0BE checks number of levels they grew in daycare and places it on given variable as well as in buffer2.
0BF calculates price on pokemon growth and places it on buffer2
0C0 gets pokemon back from daycare center (taken with bb). If 0x8004 is different from the slot number the pokemon is in, a bad egg is formed.
0c1 a "silent hatch" code, hatches an egg in your party indicated by 0x8004, even if not an egg (reset level to 5, changes met location, clears EVs)
0C2 hatches a pokemon in the 0x8004 given position, even if not an egg
0C4 shows battle records and time scores. varies with value on ox8004 *key:0x0 = battle records, else = time board*
0C5 checks if you have enough money to pay for pokemon return. If true, return 1 to given variable
0C6 charges the money calculated bf

Whiteout command
0C8 whiteout screen and carried to a pokecenter. Variables are cleaned, money is partially lost

Safari mode command
0CD start safari game. Upon end, teleported back to default map, even though you never went near it
0CE ends safari game. This end, not being the default call to script, lets you remain where you are

Pokedex evaluation command
0D4 seems to place on 0x8005 the seen pokemon and on 0x8006 the caught ones. Also places on 0x800c a number that might be pokemon the pokedex doesn't detect but were caught. if 0x8004 = 0, print kanto's values, 0x8004 != 0x0, national dex values
0D5 prints the pokedex evaluation. Number of pokemon caught is stored on 0x8004

Pc animation commands
0D6 flashes a tile that is one tile above the script. Used in pc scripts
0D7 switches a tile above script. Another animation

move deleter\reminder commands
0DB selects a pokemon, but no variable is updated
0DC shows moves from pokemon in 0x8004 and allows you to choose one, position placed on 0x8005
0DD deletes move that was suposedly chosen with dc
0DF places on last-result the number of attacks on a pokemon defined by 0x8004
0E0 opens up a menu to teach previously known attacks to pokemon (move reminder) pokemon used is stored at 0x8004
0E6 returns the slot of the first available attack. If all occupied, returns 3 to given variable

Wierd Commands
0e9 prepares the message "previously on your quest", as well as others saved from last playthrough
0ea & eb continue the previous command, but have some glitches
0EC depending on 0x8004, may crash (0x0), have a wild battle with the buffered wild pokemon (0x1) or start an item-forbidden Trainer match (0x2), all other values crash.
0EF clears party, erasing all pokemon. And this definitely erases them, as the code clearly only fills the places with 0's
0F1 after clearing all waiting data, shuts down the system internally with the Stop Bios command. Also, disables all interuptions, killing the game right there.
0F5 asks to select three pokemon, and places them in the given order(removing the remaining) - pokemon tower. Choose cancel to get all your pokemon back.
0F6 appears to return if a pokemon is able to enter the event
0F8 really wierd command, deletes part of your party except for some of those you chose in f5

Item storage
0F9 pc item storage menu with pc animation (your room)
0FA same as f9 but without pc animation
0FB town map

Trade commands
0FC checks the trade in 0x8004 and buffers the name of the wanted pokemon(0x0) and the given pokemon(0x1)
0FD gets the pokemon to trade and places it in 0x0202402c. Is in it's party form, that is, fully decrypted, 100 bytes.
0FE activates trade, gets pokemon from previous memory address Incomplete pokemon(80 byte form) crash when seen. If deposited and withdrawn, it's fixed
0FF checks pokemon in 0x8004's number and places it in given variable

pc menu
106 opens pc menu, no animations, returns 01 to given variable and ff to lastresult

Finishing commands
107 shows hall of fame thens returns you to pc menu
108 shows diploma for finishing kanto dex. Finishes script.
10F this is the function that is called by f1 to do the shuting down thing. Therefore, this crashes everything
110 saves hall of fame and plays ending

camera control commands
111 elevator scene + small animation
113 freeze screen, but only on scripts, not on signposts or people. Call again to unfreeze
114 unfreeze screen\camera. Works on all surfaces
11F returns to a variable the facing you had when activating the event

Pokemon check commands
126 Check first pokemon added EV's. if over 510, returns 0x1
12b Checks party for a grass pokemon. returns 0x1 if there is one.
12e Checks party for your kanto starter. 0x1 if you have it
130 See if there is room for pokemon in a box. returns 0x1 if there is.

132 shows current floor
136 use strenght sound

Wild battle commands
137 starts wild battle on ice. Uses everything the normal wild battle use
138 starts wild battle on normal terrain. Uses everything the normal wild battle use.
139 same as 138

Cave commands
13D flashes screen.
13E warps to last used warp
13F falls to last used warp
143 perfectly normal wild battle

147 checks your pokemon in position referenced by 0x8004 and returns to the given variable its pokemon number. returns 0x19c if an egg.
14C fades sound until it turns off. Only some soundeffects remain. Loses effect after leaving map.
153 checks your party for pokemon equipped with e-card berries. 1 to variable if it happens
156 ghost battle. If you have a shilp scope, it becomes the lv 30, uncatchable marowak. If not, the ghost will have the cry of the pokemon that was buffered.
157 activates bicycle
158 opens several different multichoice boxes depending on 0x8004 *key: 0x0 = badge talk; 0x1 = silph-co elevator; 0x2 = rocket elevator; 0x3 = celadon department store elevator; 0x4 = link options; 0x5 = pharmacy options?; 0x6 other elevator; 0x7 and up - return 0x7f to lastresult*
15C crashes game after executing whole script
15d buffers to buffer 0x0 the name of the last pokemon you encountered
15e checks if values 0n 0x8004 and 0x8006 add up to more than 9999 decimal.
15f checks pokemon number in party and places the function for the correct pokecenter animation. returns to given variable that number.

161 activates surf sprite
162 places the number of the starter you chose in a given variable
163 sees pokemon number 0x8004 in the pokedex
166 lets you nickname the pokemon in the box in 0x800f, slot 0x8010. Gives it the buffered name.

Wireless link mini game commands
16A checks for wireless connector, 1 if there, 0 otherwise. Places value on given variable
16B tries to link for a game of pokemon jump the rope
16C tries to link for a game of dodrio berry-picking
16D linking for union room
16E wireless status

16F activates national dex
171 makes it impossible to step through scripts with the D-pad. only works on Scripts (green S on Advance Map)

Fame checker commands
173 activates the info at the slot 0x8005, for person 0x8004
174 sets the pesron picture status. 0x8004 = pesron number, 0x8005 = set value. 0x1 = only shadow. 0x2 = correct picture

(cerulean) Daycare commands
176 Remove pokemon from party and store it in daycare.
177 buffers daycare pokemon name in buffer 0x0 and the money owed for it in buffer 0x1
178 checks if a pokemon is at the daycare. returns 0x1 if there is.
179 returns the levels grown and buffers the same data as 0x177
17a returns the pokemon kept there to the trainer. returns to variable the pokemon number

17B travels in the boat for vermillion
17c checks for a pokemon species in the party. pokemon number wanted is stored in 0x8004, returns to given variable 0x1 if there is one.
183 checks if someone is playing through wireless and buffers that player name. if none, the empty string is buffered
187 places value on given variable. Seems to be a error checker of some kind, for every time it returns 0x2, a script is called to end.

Fossil commands
18B shows fossil picture. Only works if 0x8004 is 0x8d or 0x8e. Position is stored in 0x8005(x) and 0x8006(y)
18C unshows picture

Move tutor commands
18D Accesses move tutor data and teaches that move to an allowed pokemon. Tutor placed at 0x8005
18E a menu to choose pokemon for something

191 SS Anne departure scene. With no boat, your sprite will follow it and disappear, making you invisible.
192 checks if you have any of the needed pokemon to play Jump the Rope minigame
193 checks if the national pokedex is active.
194 if used without 0x8004, clears all warps and reproduces buffered video all over the map. Entering any menu will blank the video, crashing the game on exit
195 pokemon jump records
196 buffers attacks to 0x0 if 0x121< 0x8004 <0x15a. attacks buffered are stored in a table. returns 0x1 if it buffered anything

Berry powder commands
19b checks if you have the berry pouch and at least a berry. returns 0x1 if true to given variable and to 0x800d
19C shows powder counter
19D hides powder counter
19e checks if powder is more or equal to the amount asked for in 0x8004. returns 0x1 to given variable if its true.
19f decreases the amount in 0x8004 to the powder counter. returns 0x1 to given variable if it didn't go negative.
1A2 shows berry crushing records

Old lady tutor commands
1a3 buffers the ultimate attack for the starters and prepares the right tutor. return 0x0 if no pokemon in your party can learn it.
1a4 after teaching, checks the value of the tutor and sets the respective flag to the attack. if all three attacks were taught, returns 1 to given variable

1A5 plays credits
1A6 berry-picking records
1A7 multichoice for the islands, varies with what's on 0x8004
1AB sound effect for deoxys triangles
1AE checks for illegal pokemon for the union room and places one if found on a given variable
1B0 checks if pokedex (national) is complete. 0x1 if true, placed on a given variable
1B2 places a red arrow at the pixel coordinates indicated by 0x8004(X) and 0x8005(Y). 0x8006 = 0 means turn on, or create a new one, 0x8006 != 0 means shut last arrow down

1B5 creates a tile animaiton one block left two-four up the player
1b6 checks if you have a dodrio in your party.
1B7 creates a tile animation two-six blocks right from the top-left corner of the screen
1bb Create a full, party ready pokemon, at the trade slot. 0x8004 = pokemon number, 0x8005 = pokemon level, 0x8006 = item held
```

