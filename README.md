
# Skill Tree System for Unreal
This is an Unreal Engine plugin that contains a backend system with proper editor support for a skill tree system. A basic skill interface is in place to take advantage of Unreal Engine's built-in Gameplay Ability System. The purpose of this tool is to give designers an easy way to rapidly iterate on building skill trees for any game. While this project was originally developed for my own projects, I have tried to keep it generalized.

Any feedback, constructive criticism, or contributions are appreciated! This is currently a 1-person passion project with the hope that it might turn into a valuable open-sourced tool for Unreal Engine developers.

## General Overview
To use this plugin, you will first need to create **Skill Assets** which contain basic static data regarding the various abilities a player can learn. 

At runtime, a skill tree will provide all of the relational data between the various skills declared within the tree. It will also keep track of a player's progression within the three (i.e. which skills they have learned). This will be presented via a **Skill Tree Context** object. Long-term storage of a skill tree's state will be inside **Skill Tree Memory**. 

## Skill Tree
### Creation
Create a Skill Tree Asset with **Right Click -> Skill Tree System -> Skill Tree**. Skill trees are constructed within the graph editor via nodes and edges (aka ***Links***). Nodes represent a skill while links represent the requirements needed to learn the skill. 

**Note:** Nodes can also have requirements needed before they are available for a player to learn. For example, a link between *Skill 1* and *Skill 2* implicitly requires *Skill 1* to be learned *before* *Skill 2* can be learned. *Skill 2* might also have a custom requirement that states X number of skill points have to spent in the tree before it is available. 

### Node Types
#### Start Nodes
This is an implicit node type and is denoted by a node that has no links where it is the child. These nodes will be highlighted in green within the editor to more easily understand where players can enter the skill tree from. (By default, this system supports multiple start nodes).

#### End Nodes
This is another implicit node type and is denoted by a node that has no children. These nodes ill be highlighted in red within the editor to more easily understand where the tree ends.

#### Passive Skill Nodes
Passive nodes are used to identify skills that do not provide the player with an interactive ability. This could be things like stat upgrades, augmentations to existing abilities, etc.  While this is largely for visual purposes, data regarding the number of passive skills learned could be used as conditions for Links/Nodes. 

This node type can contain 1 or more ranks. Ranks allow the player to spend a set number of points more than once. Ranks are finite by default, and all ranks must be purchased before a link's default condition is satisfied. 

#### Active Skill Nodes
Active Skill nodes are used to identify skills within the tree that grant the player access to an interactive ability.  While this is largely for visual purposes, data regarding the number of active skills learned could be used as conditions for Links/Nodes. 

This node type can contain 1 or more ranks. Ranks allow the player to spend a set number of points more than once. Ranks are finite by default, and all ranks must be purchased before a link's default condition is satisfied. 

#### Choice Nodes
Choice nodes are unique selector nodes in which the player can choose from a selection of possible skills. Essentially, it is a group of other nodes where only one can be selected at a time. When a skill is chosen, the appropriate conditions are checked and - if successful - the corresponding event is fired off. 

**NOTE:** Reselecting a different choice will require a rebuild of the graph by default.

### Links
Links are the edges within the skill tree graph, and their main purpose is to represent relationships between different skills. In the editor, a link will be an arrow pointing from the parent node to the child node. 

Links can also hold conditions that govern whether or not a child skill node can be learned. The default link condition checks whether all ranks of the parent node are learned. 

### Building the Graph
#### Add Node
Right click in the grid and select the desired node type under the Skill Node category in the context menu. Then select the corresponding skill asset from the content browser. 

**Note:** The selected skill musts match the type of skill node added. For example, if a passive node is created, the corresponding skill *must* be a passive skill as well.

#### Add Link
Press and hold left mouse button on the border area of the parent node, then drag the mouse to the border of the child node. 

**Note:** If a circular dependency is detected from the newly created link, an error message will pop up and the operation will fail.

#### Retarget Edge
Hold Ctrl and drag the edge via left mouse button to change either the child or parent (depending on here the click and drag as initiated from). 

**Note:** If a circular dependency is detected from the newly created link, an error message will pop up and the operation will fail.

### Conditions
Edges and nodes can contain conditions to restrict the purchasing of a node. 
### Events
