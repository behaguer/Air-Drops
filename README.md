![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/head.png)

# DCS Air Drops for the C130j
This is a DCS mission .lua script that allows c130 Air Drops to be called in from the radio menu. It also allows the player to drop CDS crates and use map markers to "manufacture" ground vehicles from dropped CDS supply crates.

This script includes the spawning of c130's, which will carry 2 weapons platforms for deployment. It has 3 variations, Tank, Apc and Hummer. Once the radio command is issued it will give you a map marker code so you can create a mission map marker in the f10 menu with that code. 

If you would like to troubleshoot in the lua script you can turn debuging on which will output messages.

# Instructions
1. Open the example mission file `Air Drop.miz` in the mission editor.
3. Click the Set Rules for trigger icon on the left hand nav menu. (3 Down from the text "MIS")
4. Note the ONCE trigger is set. Click on it.
5. Note the Time More is set to trigger on load.
6. Click the DO SCRIPT FILE and make sure it is linked to the Air `Air Drop.lua` script.
7. Load the mission as a multiplayer mission, then in-game:
    Use the radio menu to call in drops place map markers named `dp-alpha`, `dp-bravo`, etc. for drop zones (these are generated dynamically) or Spawn CDS cargo containers using the C-130J mod and fly to the drop zone and drop them. Once they have landed use `make tank`, `make apc`, or `make humvee` map markers to spawn vehicles from nearby landed crates. Each spawn requires 2 CDS crates. If you are wanting to build a farp use 4 CDS containers and the map command `make farp`
8. Enjoy.

![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/cds.png)
![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/cds2.png)
![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/farp.png)
![DCS Airdrop Script for Eagle Dynamics Digital Combat Simulator.](/assets/images/farp1.png)