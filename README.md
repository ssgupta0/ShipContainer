## INSTRUCTIONS TO RUN APPLICATION

1)
  Clone repository
  
2)
  Navigate to cloned directory
  
3)
  Navigate to "/flask_shash" directory
  
4)
  command to run: `python3 container.py`
  
5)
  Open http://localhost:5000 in web browser
    or given URL from step (4) output 


## Project Report:

There are two main problems that we need to solve for Mr. Keogh
  Load/Unload
  Balance

#### Stuff that applies to both problems

Ships to be unloaded, loaded, or balanced have 1 cargo bay with the dimensions: 8 cargo containers x 12 cargo containers x1 cargo container
On average, there is one ship per day, but it is possible for multiple ships to come to port in a day, so there needs to be some way to distinguish between them.

 Our program should display the amount of moves to complete the balancing of the ship.
It should also keep a log file of all events with a timestamp that should only be accessible by the user via comments to logfile.

Containers weigh between 0 and 9999 kg

Storage of containers that are not picked up by the respective truck is not an issue since we have a button to call someone else to deal with it.

We only know the actual weight of the container once the crane lifts it.

#### Log File details

Once per year near Christmas, port is closed and log file is sent to Mr. Keogh

Logfile can not be edited directly, the employee makes a comment and the comment is pushed to the logfile.

If the container’s weight does not match the weight in the manifest, the employee makes a comment to the logfile.

For every new manifest uploaded to the program (one manifest per ship), a comment is made in the logfile stating “manifest for Ship X uploaded and will dock soon”.

Every time the user confirms a container is placed down, its new position is logged to the Logfile

Manifest is sent to the captain of the ship when done with the ship and then immediately deleted.

Format is [position in x,y], [weight in kg], [description: company, contents...etc]

Employees will have access to a cheap PC that has ethernet

Three shifts per day: 12am-8am, 8am-4pm, 4pm-12am

At start of each shift, the employee must sign in to the computer by inputting their name, which will get pushed to the log file along with the time

Buffer can hold the same amount of cargo as a class X2 ship

Before a ship can leave the port the buffer must be cleared

Set of trucks that offload a container and the set of trucks that onload a container are  disjoint.

Each object has its own version of the manifest.

Once they have used the crane to physically move the crate, they can click a button to show them the next animation of what the next move should be

Key elements of the Load/Unload problem

It is the employee’s responsibility to translate the transfer list.

To make the employee’s job easier, our software will only require them to select crates to be removed from the ship and enter names and weights of crates being loaded in a text box.

The employee is shown an animation of which container they should move next and where to move it to.

Once they have used the crane to physically move the crate, they can click the next button to show them the animation of the next move.

When the desired crate is accessible, a button will become available which summons the truck picking up that crate.

Similarly, when loading a crate, a button can be pressed which opens a text box allowing the operator to enter the name of the container. 

The goal of our software is to remove human error, finding the most efficient set of steps to load/unload a set of containers to/from a ship.


#### Key elements of the Balancing problem

A ship is required to be “balanced” by Maritime Law.

A ship is technically “balanced” when the total weight of the lighter side is within 10% of the total weight of the heavier side.

The side will be split so columns 1-6 and 7-12 will be considered the two sides

Formula: Balanced = max(weight(port),weight(starboard)) / min (weight(port),weight(starboard))

Containers have the mass noted on the manifest, where empty containers have a mass of 0kg.

If it is impossible to “balance” according to the above formula, we can legally do a SIFT operation.

Our algorithm will compute the best steps to reach a legally “balanced” state with minimal time loss.

The goal of our software is to remove the human error in finding the most efficient set of steps to unload a container from a ship.



When the program starts to run, we create a directory to hold the logfile. It is created at the directory of the program location.

At the start of a shift, an employee can log in by clicking on the login button and inputting their name. A log is created for each successful log in.

Once the manifest is there, a comment is made in the logfile stating “manifest X is uploaded and will dock soon”.

If the crane is not carrying anything, we can move it through containers to get to the one we want to pick up.

Each time a new manifest is given, we overwrite the old manifest copy.

We have two main pages, the home page and the containers page.

#### Input to our program

Employees log in using their name.

In load/unload, the employee chooses the containers to get picked up.

For Load/Unload, the employee manually types in the weight and the name of the container to be loaded.

#### Load/Unload Heuristic

Costs and branching factors development process

Pick up the containers in top layer list left to right

to choose which column to pick from first, …?

First containers that are moved are the ones that are on the top layer of the ship.

This top layer is stored in a list.

The cost to move the crane to the truck is included when trying to choose the best move to make in the heuristic.

Once the container has reached the top pink box, the set cost to reach the truck is added.

Heuristic should consider
-distance to top of each column from crane location
-pick up from a column that has a container to be moved that is trapped

At some point during the brainstorming process, we thought about having each state include both which container is chosen to be picked up and where to drop it off. This partly survived in the structure of the heuristic.

##### The overall structure is 
After each step in the heuristic, the original index and the new index is stored.

At the end of the heuristic, the final index is stored.

This is also used as the data to be stored in the final manifest

The cost to move the crane to the truck is included when trying to choose the best move to make in the heuristic.

Once the container has reached the top pink box, the set cost to reach the truck is added.

#### Final heuristic
Length of the load and unload lists.

Final Cost and branching

1st: pick up containers in top layer list

2nd: unload containers that need to be unloaded

If there are no containers in the top layer, pick up a container from a column that needs to be unloaded.

#### Balancing Heuristic

Heuristic for choosing a col to be picked up from

Heuristic is calculated by counting how many columns away is the next non-full column is

If we are picking up from the lighter side, our heuristic becomes slightly worse than the worst case of picking from the heavy side

After placing chosen container in each column, we calculate the heuristic for each of those states

Take the sum of all weights of the containers and divide by two. Subtract the lighter side from this avg. Find the containers that best fit the 
Potential of getting balanced

We then add our heuristic for picking the container and heuristic after it has been placed for our final heuristic of the state

Balanced state is saved separately and not expanded

During the early coding process, we realized we did not have all the pieces, so we had to modify the structure a bit to make it easier overall.
We decided to make a copy of the logfile.
The heuristic had to be modified from picking from the top layer first to instead mainly considering the distance from the crane current location to 
To make a move, lift C up max height and move to proper col, then move down the number of spots until one above the top container of that col.
Finalized the heuristics for both Load/Unload and Balancing as detailed above.
TeamAI-aboard_Testing
Our program accepts ship cases with any name but we kept the same format for simplicity, ShipCase#.txt
To test the sorting module, we will test:
It can sort 10 objects in under 3 seconds. We will test this with:
A list that is already sorted. 			Balancing	shipCase6
A list that has many duplicates.			Both		shipCase7
A list that is all duplicates.			Both		shipCase8
We will test the special cases of
A list of size zero, of size one, or size two.		Both		shipCase9
A list that has extreme variance (uses SIFT)	Balancing	shipCase10


Acceptance testing
Since the original plan of getting three test cases from Mr. Keogh and our program being tested on the spot was changed, we used the given test cases to test our program.
Test case 5 took 25 seconds to balance.
On the same input, our program can produce an erroneous output but after hours of debugging and the error only occurring sometimes, we could not find the bug. If the program is run on the same case again, we find the program breaks.
For test case 3, this happens everytime so far.

A Maintenance Plan
In the future, we plan on adding 
an animation to highlight the most optimal route, instead we highlighted the before and after positions in different colors for each step.
implementation of the buffer similar to the bay of the ship.
access to the Desktop folder by the employee so they can select the final manifest to send to the ship’s captain. 
saving the log file and manifest to the Desktop would help with not losing current state in the problem during a power outage.


YouTube video demo link: https://youtu.be/bBkDjadsykA
