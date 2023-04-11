[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-c66648af7eb3fe8bc4f294546bfd86ef473780cde1dea487d3c4ff354943c9ae.svg)](https://classroom.github.com/online_ide?assignment_repo_id=10506314&assignment_repo_type=AssignmentRepo)
# The Mysterious Realm of Lenia (jxkc2)

### Content Outline
1. Unveiling the Lenia Model
2. Basic Principles of Cellular Automata
3. The Classics of Cellular Automata
4. Bridging into Lenia from the Classics
5. Building our Variations of Lenia
6. Reflecting upon our Lenia Implementations and exploring new phenomena with Lenia
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

$$(cell \space state)_{t} = f(neighborhood \space of \space states)_{t-1}$$

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

*The Implementation of Wolfram elementary CA can be found in lenia (jxkc2).ipynb Section 1*

It is important to note that as we are analysing the results of [Wolfram elementary CA][New] found in *lenia (jxkc2).ipynb Section 1*, observe that it is a 2D figure for each rule set. This result is presented in this way because it essentially involves our original one dimensional array of cells stacked on evolving generations of time from $t=0$ onwards and hence the 2D format. In other words, the $y$-axis is time while the $x$-axis represents the 1D array of cells of a given state.

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


In the implementation of the 2D array Conway's algorithm outlined in *lenia (jxkc2).ipynb section 2*, the [pygame library][doc] is used to initialise the simulation in a separate command window. Motivation behind this is to allow users to effectively create their own initial pattern of their choice during program execution without having to constantly manipulate cumbersome arrays (changing ones and zeroes for states) to designate the type of initial simple or complex patterns.

Therefore, if for example, a random static initial pattern is drawn manually like the first few seconds of the GIF file below, we will get Conway's algorithm coming into action after some time (once the spacebar is pressed).

<img src="https://i.postimg.cc/ZRhs1Srd/Random-Pattern-Conway.gif">

*Fig. 5: Result of Conway's Game of Life on a random initial pattern drawn from running Conway's Algorithm (Note: this result is produced from screen recording the algorithm in action when the random initial pattern is drawn in a separate window)*

[doc]: https://www.pygame.org/docs/

## 4. Bridging into Lenia from the Classics

Even before entering into the Lenia realm, we find that Conway's Game of life and Lenia simulations are not starkly different. This is because in the case of [lenia][Len], it is actually derived from Conway's game of life by making [space, time and state continuous while generalising the local rules][contin] of the algorithm as we have seen in the simulate function implementing Conway's rules but without generalising in the lenia case. Thanks to [Bert Chan's work on Lenia][paper] in discovering potentially new biological life, there is a lot of room to explore different "creatures" from the [Lenia code][code] Bert Chan has posted online for others to experiment with. 

However, before finding out how we can make the domains smooth similar to lenia, there are interesting new patterns that we can explore with our current Conway's algorithm even without making the boundary conditions smooth.

### 4.1 Intriguing Patterns for Thought 

Alan Hensel, like John Conway was also a life enthusiast in terms of collecting interesting patterns. Hensel, along with many others, compiled a brief but [extensive glossary of patterns][glos], some of which reveal interesting behaviour such as gliders, puffer trains, etc. In this report, we shall focus specifically on the spaceship, methuselah and as these patterns provide an interesting prelude to other algorithms that have been made to understand these patterns deeper (though these other algorithms will not be implemented here), besides just our simple grid counting implementation with 2D arrays as we have done in section 2 of *lenia (jxkc2).ipynb*.

### 4.2 Methuselah

It is generally known that a [methuselah][met] is a pattern that requires several generations to stabilize (after a number of units of time during which the cellular automaton's rules are implemented) (known as its lifespan). At some point during its evolution, it also grows considerably bigger than its original configuration. The term "methuselah" is not typically used to describe patterns that stabilize in less than 100 generations, however it should be noted that there is no commonly accepted description for this pattern.

<img src= "https://i.postimg.cc/CxDQK2v4/R-pentomino.png">

*Fig. 6: [R-Pentomino][r],  smallest and most well-known methuselah*

As seen in Fig. 6, the [R-pentomino][r] is an interesting pattern that begins with fewer than six cells; which does not stabilise until generation 1103, by which time it has a population of 116. Running Conway's algorithm we have developed allows us to see the bottom result where the R-pentomino quickly multiplies rapidly to other cells before stabilising to static live cells as shown in the video below.

[Len]: https://golden.com/wiki/Lenia#Table:-Further-Resources
[contin]: https://chakazul.github.io/lenia.html
[paper]: https://arxiv.org/pdf/1812.05433.pdf
[code]: https://colab.research.google.com/github/OpenLenia/Lenia-Tutorial/blob/main/Tutorial_From_Conway_to_Lenia.ipynb#scrollTo=21nh7RTSGVcW
[glos]: http://www.radicaleye.com/lifepage/picgloss/picgloss.html
[met]: https://en.wikipedia.org/wiki/Methuselah_(cellular_automaton)
[r]: https://conwaylife.com/wiki/R-pentomino#:~:text=The%20R%2Dpentomino%20is%20a,has%20a%20population%20of%20116.


<video src= 'https://user-images.githubusercontent.com/73965521/230896555-aadc8ada-ea73-45b3-9f8d-0a95e290bc9d.mp4' controls>
</video>

*Fig. 7: Animated Evolution of the R-pentomino Methuselah*

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

*Fig. 8: Light-weight spaceship Initial pattern*

It is useful to note that the amount of cells a pattern moves during the course of one period, divided by the period's duration, determines a spaceship's speed. This is conventionally stated in terms of $c$, or one cell each generation, which is a metaphor for the "speed of light." Again, running Conway's 2D array algorithm that we have, produces the evolution of the spaceship pattern according to Conway's rules. On the left is the evolution at $0.001s$ frame frate while on the bottom right has the evolution at frame rate $0.5s$ rate

<video src= 'https://user-images.githubusercontent.com/73965521/230896614-43cdda2b-3b68-4e00-81ad-bb707fcb5fbe.mp4' controls>
</video>
<video src= 'https://user-images.githubusercontent.com/73965521/230896638-262d6d98-12d7-45dc-8da4-8fb7c0caa42d.mp4' controls>
</video>

*Fig. 9: Animated Evolution of the Spaceship pattern. Left Video represents the fast frame rate for the animation and right video represents the slow frame rate for the animation*

[space]: https://conwaylife.com/wiki/Spaceship

### 4.5 Deploying Neural Networks and Machine Learning Methods

Outlined in [Qitian Liao's research on "Automatic Detection of Interesting Cellular Automata" focused on spaceships][NN], the aim of his research on spaceships was to narrow down the search for rule sets (beyond Conway's standard ruleset) that are able to produce interesting glider generations from spaceships as seen in Fig. 10 below. His [research methodology][NN] was based on the assumption of the Moore's neighborhood which we have used in our simple 2D array Conway's algorithm implementation and that random initial congurations will and must eventually stabilize.

<img src= "https://i.postimg.cc/FFGRWrQ2/Liqiaotian.png" width="200" height="200">

*Fig. 10: Four examples of discovered glider generators with dierent
rules. They are “Brian’s Brain” with rule “/2/3” (top left), “Burst” with rule
“0,2,3,5,6,7,8/3,4,6,8/9” (top right), “Brain6” with rule “6/2,4,6/3” (bottom left), and “Star Wars” with rule “3,4,5/2/4” (bottom right) ([Taken from Qitian Liao's research][NN])*

The challenge of this research aim was that most rule sets produced unappealing results for the spaceship patterns, some of which showed unstable, explosive growth of the patterns while other configurations showed the patterns dying out quickly. In Qitian Liao's research, he looked at the implementation of his data generation algorithm and added data augmentation to a few known rules, from which, he created the dataset from scratch. To identify intriguing principles that produce spaceships, Qitian Liao used recurrent neural networks (RNN), convolutional neural networks (CNN), feature extraction and other trained machine learning algorithms in discovering several new rules. 

From his results, he was able to discover a whole family of spacecraft ranging from Frankenstein spacecraft, the New Life, to several more intriguing outcomes detailed in his [paper][NN]. Although these deep neural networks seem promising, it is worth noting that he only considered a limited number of assumptions, some of which include limiting to Moore's neighborhood, [“Instant birth, gradual death, no recovery” Model][NN], spaceships and not other patterns like oscillators. In addition, performance of the neural networks as Qitian Liao had tested, increasing kernel size and filters in the convolutional layers did not improve performance of the CNN model. In summary, while deep neural learning is not all perfect, there is still much potential in what these neural networks can offer in expanding the knowledge bank of new patterns or creatures.


### 4.6 Final thoughts on other algorithms and patterns out there

Overall, while we have seen from our above examples of methuselah and spaceships, both motivating the discovery of Hashlife and neural network algorithms respectively, there are also other work on new algorithms focused on different aims that include improving speed of rendering the animated evolution or simply finding new creatures as we will see in lenia. 

Other ambitious algorithms in the works include [Accelerated GPU-based algorithm][gpu] or [Callahan's algorithm][cal] in improving the processing speed of simulating Conway's Game of life. The performance of these side algorithms along with other numerous interesting unexplored patterns in [Alan's extensive glossary][glos], beyond the methuselah and spaceships, are left for the reader to explore further.


[NN]: https://www2.eecs.berkeley.edu/Pubs/TechRpts/2021/EECS-2021-150.pdf
[gpu]: http://www.marekfiser.com/Projects/Conways-Game-of-Life-on-GPU-using-CUDA
[cal]: https://github.com/johnhw/glgol
[glos]: http://www.radicaleye.com/lifepage/picgloss/picgloss.html


## 5. Building our Variations of Lenia

Now, the fun begins in embarking our journey into the renowned lenia concept developed by Bert Chan. To make this seamless transition from Conway's game of life to the lenia variation, we would need to first consider how to generalise Conway's algorithm into one that has a continuous domain. In this section, we will be exploring 2 research articles and attempt to work on the implementation of these models towards a similar concept of lenia, they are the [Gray Scott Model][gray] and the [Smoothlife model][smooth]

[gray]: https://www.lanevol.org/resources/gray-scott

### 5.1 First Variation of Lenia: The Smoothlife model

In the Smoothlife model as outlined in the Rafler's research paper [here][smooth], we will explain in this sub-section how the mathematics of the Smoothlife model is able to qualitatively generate the continuous model similar to lenia. As for The full explanation of how the mathematical steps in the paper are implemented in code, it can be found in lenia (jxkc2).ipynb repository.

From Rafler's paper, the mathematical aspects or features that we need are essentially effective grids, smooth transition functions and time-steps added with some speedups of the algorithm.

#### 5.1.1 Continuous Grids

We have seen that in Conway's simple 2D array grid counting algorithm implemented in section 2 of lenia (jxkc2).ipynb repository, we have constructed a discrete grid of infinitesimal cells. In order to smooth the domain, it is first necessary to swap out the discrete grid for a time-dependent continuous real scalar field on the plane. In other words, we make the length of the cells small but finite. The radius of the tiny, individual cell, $r_i$, is therefore defined as a positive real value. The filling of the cell or the average of the field over the radius $h$ neighborhood around the cell at any given moment is easily described by the integral variable $M$ used in the smoothlife model article $x$. 

The number of cells in each cell's neighborhood should also be counted using this approach of averaging. As a result, we swap out the Moore's neighborhood, which we recalled to be a rectangular border in the original game of Life, with an annulus that is centered at the same random point $x$. This outer radius is believed to be $3r_i$ as deduced from Rafler's research paper [Smoothlife][smooth] under eqn. $(2)$ using $r_a$ definition. Visually, these mathematical integrals can be represented in fig. 11 below.



*Fig. 11: $M$ represents the inner cell area and $N$ to represent the outer cell area*

#### 5.1.2 Smooth transition functions

Recall that in Conway's game of life, the rules for the game of life involved counting the number of living and dead cells in the Moore's neighborhood discretely. Therefore, in the case of Smoothlife, we would need to generalize the rules for the game of life that apply to continuous values. The equation explaining Conway's rules may be obtained if we define our original field $f$ to be a discrete grid and $N$, $M$ gives the state of each cell and the quantity of live cells in the Moore neighborhood, respectively:

$$ f(x,t+1) = S(N(x,t),M(x,t)) $$

Where $S$ represents the transition function of the game of life. This function $S$ is, in other words, the set of rules governing the evolution of life in Conway's Game of life. In inequality form:

$$
S(N,M)=
\begin{cases}
1 & \quad \text{if $3-M \leq N $ and $ N \leq 3$}\\ 
0 & \quad \text{otherwise}
\end{cases}
$$

With this in mind, we need to smooth out the function $S$ or create a smooth approximation to $S$. To achieve this, following [Rafler's paper][smooth], we begin with the logistic sigmoid in the study by using the sigmoid functions to express the transition state and thresholding..

$$ \sigma = \frac{1}{1+e^{-\frac{4}{\alpha}(x-a)}} $$

Plotting this function in lenia (jxkc2).ipynb produces the following plot for the logistic sigmoid for $a=0$ and $\alpha = 4$:

<img src= "https://i.postimg.cc/WbvCPxqG/Graph1.png">

*Fig. 12: Plot of the logistic sigmoid $\sigma$ from $x = -6$ to $6$*


We see that the sigmoid functions can be explained to go smoothly to 0 when $x < a$, and goes to 1 when $x > a$. On the other hand, the parameter $\alpha$ tells us how fast the sigmoid goes from 0 to 1 with $a$ being the parameter that shifts the sigmoid. Therefore, we can impose that any effective field values greater than 0.5 to be alive while those below 0.5 to be dead. Hence, using Rafler's eqn. $(5)$, the $\sigma_m$ function selects between $x$ and $y$ depending on whether the cell is alive or dead respectively. 

In addition to the above, it is also necessary to smoothly threshold the interval which can be done using eqn. $(4)$. Combining this with eqn. $(5)$ gives us the state transition function in eqn.$(6)$ where $b_1$, $b_2$, $d_1$ and $d_2$ represents the limits for the life/death intervals. As for the width of the step, $\alpha$, we have the $\sigma_n$ and $\sigma_m$ variables which therefore necessitate 2 different values of $\alpha_n$ and $\alpha_m$. In total, these are the 6 parameters for building the CA for smoothlife.

#### 5.1.3 Time steps

Having made the state and space continuous, we are now left with the time domain. To smooth the discretised time-steps, we can introduce an infinitesimal time $dt$ and reinterpret the transition function as a rate of change of the function $f(\overrightarrow{x}, t)$ instead of the new function value as quoted by Rafler in his paper. Hence, this gives the function as:

$$ f(\overrightarrow{x}, t+dt) = f(\overrightarrow{x}, t) + dtS[s(n,m)]f(\overrightarrow{x}, t) $$

With this eqn. above, we use $s(n,m) = 2s_d(n,m)-1$ to smooth the time step.

#### 5.1.4 Miscellaneous Enhancements


#### 5.1.5 Exploring New Patterns with Varied Enhancements


### 5.2 The Gray Scott Model 



[smooth]: https://arxiv.org/abs/1111.1567



