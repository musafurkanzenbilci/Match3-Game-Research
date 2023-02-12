# Match3 Game Research

This repository contains a collection of various problems and possible solutions to them while building a Match3 Game like Candy Crush, Royal Match, etc.

## Story
I am passionate about games since my childhood and always wanted to be part of the creation of these games. Because sometimes I admire the design like minesweeper and sometimes I only wanted to meet(!) those guys who made the game like League of Legends.

So, I am developing games in Unity since last summer as I have time. At first, my idea was to build a Match3 Game and maybe create a twist on the gameplay. But the more I plan how to do it, the more I had to think. I did not expect to see these kinds of complex problems in Match3 games, especially in the initialization of the board. 

## Initialization of the Game Board

For simplicity, I will mention places in the grid as **Node** and the signs I put in them as **Piece**.

So, we need to assign pieces to each node on the game board to start the game. But what are the constraints?

- The game must include no matches
- The game must include possible matches
- The game must ensure there will be enough possible matches for the goal of the level

The no-match constraint seems familiar to me. It is like coloring a map without coloring two neighbors with the same color. 

<img width="415" alt="Screenshot 2023-02-11 at 18 59 44" src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b1/Map_of_United_States_accessible_colors_shown.svg/2880px-Map_of_United_States_accessible_colors_shown.svg.png"><img width="415" alt="Screenshot 2023-02-11 at 18 59 44" src="https://cdn.vox-cdn.com/thumbor/bzrCTxtnJViyIsGZBlztQFPQZSM=/0x0:960x540/1600x900/cdn.vox-cdn.com/uploads/chorus_image/image/29156199/candy-crush-saga-screenshot_960.0.jpg">



Yeah! It is a **_CSP_**([_Constraint Satisfaction Problem_](https://en.wikipedia.org/wiki/Constraint_satisfaction_problem#:~:text=Constraint%20satisfaction%20problems%20(CSPs)%20are,solved%20by%20constraint%20satisfaction%20methods.)). I studied CSPs in my AI course previous semester. So how do we solve them?

CSPs are defined by three factors in our case:
1. **Variables** - Defined set of piece values
2. **Domain** - A set {x1,..., xd} representing all possible piece values that a Node can take on.
3. **Constraints** - Constraints define restrictions on the values of variables, potentially with regard to other
variables.

Based on these factors, I will create a graph structure to represent the game board, will traverse each node, and assign pieces to them from their domain. At each assignment, we will **exclude the assigned piece value from the domain** of the neighbors.

Also to optimize solving CSPs, I also will use **Least Constraining Value** principle. Briefly, when selecting which piece value to assign next, a good policy to implement is to select the value that **prunes the fewest values from the domains** of the neighbor nodes.

Here is the result with 2 neighbourhood style:

| 4 Neighbours (up,down,right,left) | 8 neighbours(+ diagonals) |
| ------------- | ------------- |
| <img left="10" width="405" height="600" alt="Screenshot 2023-02-12 at 11 11 40" src="https://user-images.githubusercontent.com/123447782/218300204-999f7480-4ccc-47d0-a096-cd7d0638d018.png"> | <img left="10" width="405" height="600" alt="Screenshot 2023-02-11 at 18 59 44" src="https://user-images.githubusercontent.com/123447782/218268125-fde4fd20-0f16-4bb7-9058-2559d17bd022.png">  |




&nbsp; 
&nbsp; 

As you can see, there is no match for both of them. Constraint SATISFIED!!

But there is something wrong. Boards are unplayable and each column is a pattern. We need possible matches on the board and an arbitrary layout each time we instantiate it also making sure there are no matches on the initial game board.

I must have studied the AI course really well because it inspires me again for the solution. The keyword was **exploration**. Briefly, exploration is making arbitrary or non-arbitrary moves to maximize knowledge over an environment. So with a specified randomness threshold, if the **generated random value is bigger than the specified randomness threshold, I will assign a random piece** to the current node instead of using CSP solution piece. I am aware that random moves may create matches but we need the randomness for the game sense.

Here are the boards with different randomness thresholds:


| 0.75 threshold - 2 matches - 34 random nodes - 3 potential matches | 0.5 threshold - 4 matches - 63 random nodes - 16 potential matches |
| ------------- | ------------- |
| <img width="414" alt="34r075" src="https://user-images.githubusercontent.com/123447782/218270070-7b633f40-8b79-4b13-9d67-6062ae3de5bb.png">  | <img width="411" alt="63r050" src="https://user-images.githubusercontent.com/123447782/218270072-b0f04ba7-3fba-4dec-8fa3-af01b0f5ce0d.png">  |

| 0.25 threshold - 6 match - 100 random node - 15 potential matches | 0 threshold - 7 match - All Nodes are Random - 24 potential matches|
| ------------- | ------------- |
|<img width="411" alt="100r025" src="https://user-images.githubusercontent.com/123447782/218270073-7cb666b4-902d-4e36-8598-d022292d37f1.png"> | <img width="414" alt="126r0" src="https://user-images.githubusercontent.com/123447782/218270075-0baff387-d836-4b68-986d-d5726f0bd081.png"> |

&nbsp; 
&nbsp; 

**What threshold value we should choose?**

Our constraints briefly imply that we should minimize the matches while keeping enough match possibilities. When we look for match numbers and potential matches;

- I definitely eliminated **0.75 threshold**. It creates unplayable game boards.
- For **All Random Case**, at first sight, it made me think why did I bother this much **rather than simply instantiating a random board and fixing matches** :D. But it is too random and we **cannot balance the number of potential matches with the goal or difficulty of the level**.
- **SOLUTION:** I see the balance in **0.5** and **0.25** cases. Maybe we can stretch the range to 0.6 and 0.2 and choose a value between this range based on the difficulty of the level.

# TO BE CONTINUED WITH
- How to ensure there will be enough possible matches for the goal of the level
- How to test the game
- What would be the strategy for solution if I want to create an AI agent


## Getting Started

To run the projects, you need to have the following tools and libraries installed on your computer:

- Unity Editor

To run a project, simply navigate to its directory and follow the instructions in the project's readme.

## Contributing

1. Fork the repository
2. Create a new branch for your changes (`git checkout -b new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin new-feature`)
5. Create a new Pull Request

## License

This repository is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.


&nbsp; 
&nbsp; 



