[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-c66648af7eb3fe8bc4f294546bfd86ef473780cde1dea487d3c4ff354943c9ae.svg)](https://classroom.github.com/online_ide?assignment_repo_id=10506314&assignment_repo_type=AssignmentRepo)
# The Mysterious Realm of Lenia (jxkc2)

### Content Outline
1. Unveiling the Lenia Model
2. Basic Principles of Cellular Automata
3. The Classics of Cellular Automata
4. Bridging into Lenia from the Classics
5. Building our Variations of Lenia
6. Reflecting upon our Lenia Implementation and exploring new phenomena with Lenia
7. Conclusion

*Note: References are inserted in the form of hyperlinks for the specific words/phrases in this report write-up, instead of having a separate reference section*

## 1. Unveiling the Lenia Model

<img src="https://miro.medium.com/v2/resize:fit:1280/1*CwTP7ARiYE_BvsWOQzCkLA.gif" width="384" height="150" />

*Fig.1: Computer Simulation of Artificial Life in a 2D Grid*

A beautiful display of patterns, is it not? The above figure (or lenia) is a simulation describing the amazing yet complex changes in artificial patterns used to mimick the seemingly random changes in the states of living cells, which is in fact governed by a specific set of systematic rules. This is a characteristic of a discrete computational model called Cellular Automata, a complex system of several simple agents working together and exhibiting complex intelligent behaviour. 

In fact, the behaviour of lenia exhibited in Fig. 1 is an extension of the famous Conway's Way of Life; a continuous generalisation of the famous model. However, before ambitiously diving into the exploration of the lenia model and beyond in this report, we will start from leveraging the basic principles of the Cellular Automata model using simple classic examples and building this foundational framework up to study the lenia model, in addition to leaving some final room for applying our derived understanding of cellular automata models to other relevant applications of computational design in other fields.

## 2. Basic Principles of Cellular Automata

In [Cellular Automata][ducksearch] (CA), we need 3 specific aspects.
1. Grid of Cells/Entities 
2. The State of the Cells
3. Rules governing changes in State of the Cells

In order to simulate a [cellular automata][ducksearch] model, a spatial grid of cells, be it, 1D, 2D or 3D needs to be defined. In this grid, every cell can be thought of as an entity with a known state. For simplicity, we can assume this state to be of a binary type where the cell can exist in either state 0 or state 1 (white or black, etc.). The last aspect requires a set of defined rules that can model how these cell states change over time. This is determined by analysing neighbouring states. For instance, in the next moment in time, the state of cell can be calculated based on the neighborhood of one state, be it through summing neighboring states, averaging them, etc. Mathematically, we can express this relationship as:

$(cell \ state)_{t}$ $=$ $f(neighborhood \ of \ states)_{t-1}$

for any moment in time, $t$ and $(t-1)$ being the previous generation of time.

In the next section, we will use the above basic computational thinking of CA models to examine the classic examples of Wolfram's elementary CA and Conway's Way of Life, breaking down the design set-up of the rules influencing the change of states over time. From which, we will transition into the set-up of lenia and take a creative leap into modifying and extending the current lenia model implementation in the subsequent sections.

[ducksearch]: https://mathworld.wolfram.com/CellularAutomaton.html#:~:text=A%20cellular%20automaton%20is%20a,many%20time%20steps%20as%20desired.


## 3. The Classics of Cellular Automata
### 3.1 [Wolfram Elementary CA][duck]

Cellular Automata (CA) models have a rich history of work, dating back to John Von Neumann and Stanislaw Ulam. In this section however, we will instead start with a simple one-dimensional array of cells with state being either 0 or 1. The set-up of this is shown below with an arbitrary number of zeroes and a single 1 in the specified cell for simplicity.

<img src= https://i.postimg.cc/CKPp5Qq8/Computing-Project-Diagrams-page-1.png width="340" height="200">

*Fig.2: Schematic Diagram of How the [Wolfram CA][duck] runs*

The circled portion above represents the neighborhood of the specified cell of state 1 which we choose to focus on at time generation, $t=0$. In the next generation, $t=1$, the same highlighted section of the cell potentially has a different combination depending on the set of rules we impose on the system.

If we were to permute the entire list of possible combinations of the how this neighborhood (The 3 cells above) can be configured, they are:

$000$, $001$, $010$, $100$, $011$, $110$, $101$, $111$.

From this given list, we have the autonomy to assign each of the 8 combinations an outcome say 1 or 0 which we call this a rule set in wolfram elementary CA as shown below. This outcome is targeting what the middle cell of the neighborhood of 3 cells should be based on the rule set. 

<img src= https://i.postimg.cc/RCpXWzhG/Computing-Project-Diagrams-page-2.png width="325" height="300">

*Fig.3: Schematic Diagram of the Rule Set and Determination of State of Selected cell*

Because of the binary nature of the outcome, we essentially have $2^8 = 256$ possible rule sets in Wolfram elementary CA which are organised into 4 categories: Uniformity, Repetition, Random and Complexity. The first 2 categories provide predictable results in future generrations of time, where uniformity imples the system tends to all cells having a constant state 1 or 0 with time. Repetition implies an ordered pattern for instance, $01010101$ or $110110110$ etc. In contrast, random as the name suggests, provides non-repeating results in future generations of time. However, complexity is subtly different where it is not entirely random nor repetitive in nature, which is of interest in the study of Lenia.

[duck]: https://mathworld.wolfram.com/ElementaryCellularAutomaton.html

Now, with this knowledge in mind, we can easily simulate the elementary CA using NumPy library from Wolfram code.



It is important to note that as we are analysing the above results of [Wolfram elementary CA][New], observe that it is a 2D figure. This result is presented in this way because it essentially involves our original one dimensional array of cells stacked on changing generations of time from $t=0$ onwards and hence the 2D format. In other words, the $y$-axis is time while the $x$-axis represents the 1D array of cells of a given state.

In this case shown above, Rule 222 has essentially some cells having state 0 or 1 throughout as time passes, representing uniformity while rule 190 displays repetition of cells periodically changing from black to white down the vertical and vice-versa. In rule 30, it displays non-repeating or random patterns from a surpisingly simple rule and finally in rule 110, it represents the idea of complexity where it is more than just a simple sum of its parts, simple elements interacting locally with simple rules to give an ordered yet unpredictable pattern. Next, we shall extend this property of complexity to a 2D grid space.

[New]: https://mathworld.wolfram.com/ElementaryCellularAutomaton.html


### 3.2 [Conway's Game of Life][life]

In the eyes of John Horton Conway, his [Conway's Game of Life][life] (in the form of a checkerboard) was merely meant to be a thought experiment and in fact, a solitaire pasttime back before computers and processing were used. This pasttime piqued Conway's curiosity as he wondered whether a system exhibiting properties of biological reproduction can be simulated with simple rules in a game-like way. After much experimentation with just re-arrangement of squares on a 2D grid, he devised 3 fundamental criteria as quoted in which the system can be simulated to reproduce the behaviour of biological life:

1. There should be no explosive growth.
2. There should exist small initial patterns with chaotic, unpredictable outcomes.
3. There should be simple initial patterns that grow and change for a considerable period of time before coming to end in three possible ways: fading away completely (from overcrowding or becoming too sparse), settling into a stable configuration that remains unchanged therefter, or entering an oscillating phase in which they repeat an endless cycle of two or more periods.

These criteria are carefully chosen to ensure that the population behaviour is not predictable in any way. On this point, we will soon realise that there is a profound similarity between the classification of complexity in [Wolfram elementary CA][wolf] exhibited in rule 110 and criteria 1 of [Conway's Game of Life][life], whereby there is an unpredictable growth pattern born from just simple rules imposed above.

[life]: http://www.jstor.org/stable/24927642
[wolf]: https://mathworld.wolfram.com/ElementaryCellularAutomaton.html

With this idea of complexity in [Conway's Game of Life][life], we can easily see that this particular CA model set-up starts off with a 2D spatial grid of squares on which cells exist and its states are manifested again in a binary form of a black square being state 1 and a white square being state 0 as shown below for simplicity. Note that the grey squares refer to the neighborhood surrounding the cell with state 1 and not a different state.

<img src= https://i.postimg.cc/tTPD5mw4/Moore-neighbourhood-and-its-enumeration.jpg width="220" height="200">

*Fig. 4: 2D Checkerboard of cells with grey squares representing neighborhood of an individual cell in black*

Recalling from [Wolfram's elementary CA][wolf], we can see that the neighborhood of the center cell is no longer just left and right, but diagonal and vertical elements surrounding the cell as well. This is called [Moore's neighborhood][moore] and we will use this definition as it is the simplest one among other definitions of the neighborhood of the cell.

In this definition, we have a total of 9 cells including the center cell from Fig.4. This gives a total of $2^9 = 512$ combinations, significantly more than $2^3=8$ combinations for a 1D grid. We will soon notice that it is a cumbersome process to assign outcomes to each combination as we have done in the 1D case. Therefore, we instead propose generalities where if a neighborhood of a cell as a whole obeys a certain condition, we can propose how this state of the center cell behave in the next moment in time, $t$. This is the fundamental core of how [Conway's Game of Life][life] operates which we will find similar to how Lenia works, similar to the idea of population dynamics.

Therefore, we can elucidate 3 basic principles of how this state can change using criteria 3 from [Conway's Game of Life][life]. In short, they are:

1. Death
2. Birth 
3. Stasis

Death results in the living state of cell being 1 to change to 0 (cell is absent) when for example there is an overpopulation of the neighborhood with 4 or more living cells around a given cell or underpopulation with fewer than 2 living cells. Birth on the other hand, results in an absent cell state 0 to change to state 1 when there are exactly 3 living cells in the neighborhood region. Lastly, Stasis is where the cell state remains unchanged if there are 2 or 3 living cells in the neighborhood. As a result, with these simple rules, we therefore have a complex system that evolves with time in a seemingly random, intelligent way.

Armed with the knowledge in both the rules and 2D grid set up, we are ready to simulate this model of [Conway's Game of Life][life].

[life]: http://www.jstor.org/stable/24927642
[wolf]: https://mathworld.wolfram.com/ElementaryCellularAutomaton.html
[moore]: https://www.researchgate.net/figure/a-Von-Neumann-Neighborhood-bMoore-Neighborhood-number-of-neighboring-cells-shows_fig1_270451971



In the above implementation, the [pygame library][doc] is used to initialise the simulation in a separate command window. Motivation behind this is to allow users to effectively create their own initial pattern of their choice during program execution without having to constantly manipulate cumbersome arrays (changing ones and zeroes for states) to designate the type of initial patterns like gliders, glidergun, etc.

Therefore, if for example, a random static initial pattern is drawn manually like the first few seconds of the GIF file below, we will get Conway's algorithm coming into action after some time (once the spacebar is pressed).

<img src="https://i.postimg.cc/ZRhs1Srd/Random-Pattern-Conway.gif">

*Fig. 5: Result of Conway's Game of Life on a random initial pattern drawn from running Conway's Algorithm (Note: this result is produced from screen recording the algorithm in action when the random initial pattern is drawn in a separate window)*

[doc]: https://www.pygame.org/docs/

## 4. Bridging into Lenia from the Classics

Even before entering into the Lenia realm, we find that Conway's Game of life and Lenia simulations are not starkly different. This is because in the case of [lenia][Len], it is actually derived from Conway's game of life by making [space, time and state continuous while generalising the local rules][contin] of the algorithm as we have seen in the simulate function implementing Conway's rules without generalising unlike lenia. Thanks to [Bert Chan's work on Lenia][paper] in discovering potentially new biological life, there is a lot of room to explore different "creatures" from the [Lenia code][code], Bert Chan has posted online for others to experiment with. 

Before finding out how we can make the domains smooth similar to lenia, there are interesting new patterns we can explore with our current Conway's algorithm even with before considering how to make the boundary conditions smooth.

### 4.1 Intriguing Patterns for Thought 

Alan Hensel, like John Conway was also a life enthusiast in terms of collecting interesting patterns. Hensel, along with many others, compiled a brief but [extensive glossary of patterns][glos], some of which reveal interesting behaviour such as spaceships, methuselah, etc. In this report, we shall focus specifically on the spaceship, methuselah and as these patterns provide an interesting prelude to other algorithms  that have been made to understand these patterns deeper (though these other algorithms will not be implemented here), besides just our simple grid counting implementation with 2D arrays as we have done in section 3.

### 4.2 Methuselah

It is generally known that a [methuselah][met] is a pattern that requires several generations to stabilize (after a number of units of time during which the cellular automaton's rules are implemented) (known as its lifespan). At some point during its evolution, it also grows considerably bigger than its original configuration. The term "methuselah" is not typically used to describe patterns that stabilize in less than 100 generations, however it should be noted that there is no commonly accepted description for this pattern.



*Fig. 6: [R-Pentomino][r],  smallest and most well-known methuselah*

As seen in Fig. 6, the [R-pentomino][r] is an interesting pattern that begins with fewer than six cells; which does not stabilise until generation 1103, by which time it has a population of 116. Running Conway's algorithm we have developed allows us to see the bottom result where the R-pentomino quickly multiplies rapidly to other cells before stabilising to static live cells as shown in the video below.

[Len]: https://golden.com/wiki/Lenia#Table:-Further-Resources
[contin]: https://chakazul.github.io/lenia.html
[paper]: https://arxiv.org/pdf/1812.05433.pdf
[code]: https://colab.research.google.com/github/OpenLenia/Lenia-Tutorial/blob/main/Tutorial_From_Conway_to_Lenia.ipynb#scrollTo=21nh7RTSGVcW
[glos]: http://www.radicaleye.com/lifepage/picgloss/picgloss.html
[met]: https://en.wikipedia.org/wiki/Methuselah_(cellular_automaton)
[r]: https://conwaylife.com/wiki/R-pentomino#:~:text=The%20R%2Dpentomino%20is%20a,has%20a%20population%20of%20116.

Video 1 for Methuselah:

<video src= 'https://user-images.githubusercontent.com/73965521/230896555-aadc8ada-ea73-45b3-9f8d-0a95e290bc9d.mp4' controls>
</video>

From the result seen, it takes about a minute to see the entire R-Pentomino pattern to stabilise for 1103 generations. We can envision that this will be a problem in terms of computing power for both number of iterations made and the size of the spatial grid. Through [past experimentation with pentominoes by Laura Natalia][paper2], monitoring the evolution of pentominoes can be chaotic and such processes can stop intermittently during program execution due to overcrowding within a given defined space size, which affects the identification of configurations.

### 4.3 Solution to numerous generations: Hashlife?

This brings us to our first new algorithm called [Hashlife][hashlife] on the table for discussion. This clever algorithm, originally invented by Bill Gosper, was meant to speed up evaluation of the Game of Life for huge or very long running patterns using a memoized quadtree. At the algorithm's core, due to the fact that most CA patterns are extremely repetitious in both space and time, the quadtree in Hashlife enables memoizing of the recursion to cache the intermediate products, drastically reducing the amount of work needed. This lends itself an advantage in solving the computing power issue of complex pentominoes described above when running on even more complicated patterns beyond pentominoes that take numerous generations to stabilise. Thanks to the already current [code implementation of Hashlife in python by John H Williamson][hash], his implementation on the complicated [breeder 1 pattern][breed], as quoted, showed an astounding processing speed of only 1139 ms for 18,889,465,931,478,580,853,760 generations. Therefore, although this algorithm is not implemented here, we can see that it is very much designed to leverage on enormous amounts of spatial and temporal redundancy in most rules of Conway's Game of life. A full explanation of the quadtree can be found [here][exp].

However, like all algorithms, it has its drawbacks as well. Although Hashlife is quick in producing the final state of the pattern after several generations or at a specific instant of time that we are interested in, it is however not very efficient for processing chaotic patterns as it will rapidly run out of memory and stop the program execution. In addition, due to the algorithm being very complex in nature to implement, it is not very useful for interactive display, due to the constant removal of nodes from caches using a [garbage collector][cs].

[paper2]: http://www.comunidad.escom.ipn.mx/genaro/Papers/Thesis_files/reporteLauraNatalia.pdf
[hashlife]: https://en.wikipedia.org/wiki/Hashlife
[hash]: https://johnhw.github.io/hashlife/index.md.html
[breed]: https://conwaylife.com/wiki/Breeder_1
[exp]: https://www.drdobbs.com/jvm/an-algorithm-for-compressing-space-and-t/184406478
[cs]: https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)

### 4.4 Spaceships

In this separate illustration of a novel pattern called [spaceships][space], they are essentially patterns that, after a certain number of generations (known as their period), return to their original state but in a different location. This behavior closely resembles the typical glider pattern. As seen in Fig. 7 below, we have what we call a lightweight spaceship.

<img src= "https://i.postimg.cc/jS1b9VpR/Spaceship.png" width="120" height="100">

*Fig. 7: Light-weight spaceship Initial pattern*

It is useful to note that the amount of cells a pattern moves during the course of one period, divided by the period's duration, determines a spaceship's speed. This is conventionally stated in terms of $c$, or one cell each generation, which is a metaphor for the "speed of light." Again, running Conway's 2D array algorithm that we have, produces the evolution of the spaceship pattern according to Conway's rules. On the left is the evolution at $0.001s$ frame frate while on the bottom right has the evolution at frame rate $0.5s$ rate

Video 2:

<video src= 'https://user-images.githubusercontent.com/73965521/230896614-43cdda2b-3b68-4e00-81ad-bb707fcb5fbe.mp4' controls>
</video>


Video 3:

<video src= 'https://user-images.githubusercontent.com/73965521/230896638-262d6d98-12d7-45dc-8da4-8fb7c0caa42d.mp4' controls>
</video>

[space]: https://conwaylife.com/wiki/Spaceship



