# Assignment 1: Space Minesweeper

**Due: Friday, October 2, 11:59pm CT**

This assignment involves creating a simple, fun game using 2D graphics. You will learn to:

- Use TypeScript and GopherGfx for some serious programming
- Work with 2D graphics coordinate systems and vectors
- Implement game update loops and event handlers
- Animate computer graphics based on user input

In general, assignments in this class are intended to be implemented in a step-by-step manner.  The rubric below lists all of the features you should implement, starting with the basic functionality and then working up to more advanced features.

You can try a [finished version of the game](https://csci-4611-fall-2026.github.io/Assignments/Assignment-1/dist) on the course GitHub Pages. This is only a representative example. You do **not** need to make your game look or play exactly the same as the instructor's version, so long as it satisfies the requirements specified in the rubric.

## Repository Setup

We are using GitHub for submission of programming assignments. You will need to create a GitHub account if you do not already have one.

**Step 1:** You will receive an email invitation to join the CSCI-4611-Fall-2026 organization. You must first accept that invitation, which will create an association within the [course organization](https://github.com/CSCI-4611-Fall-2026) between your University email and your GitHub account.

**Step 2: ** Create your private repository using the following link: TO BE ADDED

**Step 3:**  The system will then create a new private repository with starter code that is only accessible by you, the instructor, and the TAs. You can then use `git` to check out the code to your local machine. If you prefer a GUI application instead of the command line, then I recommend using [GitHub Desktop](https://github.com/apps/desktop) or Visual Studio Code's integrated source control.

## Prerequisites

To work with this code, you will first need to install [Node.js 24.21.0 LTS](https://nodejs.org/en/download) (or newer) and [Visual Studio Code](https://code.visualstudio.com/). 

I also recommend you install the following useful VS Code extensions:

- [JavaScript Debugger](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug) (essential for real-time debugging)
- [WebGL GLSL Editor](https://marketplace.visualstudio.com/items?itemName=raczzalan.webgl-glsl-editor) (used for programming shaders later in the course)

## Getting Started

The starter code implements the general structure that we reviewed in lecture.  After cloning your repository, you will need to set up the initial project by pulling the dependencies from the node package manager with:

```
npm install
```

This will create a `node_modules` folder in your directory and download all the dependencies needed to run the project.  Note that this folder is listed in the `.gitignore` file, so it will not be committed to your repository.

You can compile and run a server with:

```
npm run start
```

Your program should open in a web browser automatically.  If not, you can run it by pointing your browser at `http://localhost:5173`.

## Rubric

Functional requirements are graded out of 20 points. Additionally, your submission will be evaluated for code quality (2 points). After your program is submitted, you will also schedule an in-person meeting with a graduate TA for a code understanding check (4 points). The overall assignment grade is therefore worth a total of 26 points.

This assignment is divided into seven parts.  The provided starter code implements the complete structure of the game and has extensive comments throughout.  The locations to add your code for each of the requirements listed below is marked with `ADD YOUR CODE HERE`.  Note that you do **not** need to make any changes to the existing code, nor do you need to add any code outside of the areas marked in the comments.  However, you should feel free to change the existing code if you want to do something fun or go beyond the requirements of the assignment.

#### Part 1: Star Movement (4 points)

- The starter code includes a ship that rotates to point towards the mouse cursor.  In class, we made the ship move towards the position of the mouse.  However, this time we want the ship to always be at the center of the screen.  Therefore, instead of moving the ship, you should move the stars in the opposite direction, thereby creating the *illusion* of ship movement. (2)
- When a star moves outside the boundaries of the scene, reset its position so that it reenters the scene at the opposite side. Recall that the normalized device coordinates used in 2D scenes go from (-1,-1) to (1,1).  Thus, if a star is exiting to the right side with an x coordinate of 1.01, then we can subtract 2 so that it is entering at the left side at -0.99. (1)
- To create a cool effect, we can make the velocity of each star dependent on its size, so that smaller stars appear to move slower than bigger stars.  This creates a depth illusion is known as [parallax](https://en.wikipedia.org/wiki/Parallax).  You can achieve this by considering the size of each star when computing its velocity. (1)

#### Part 2: Mine Movement (3 points)

- To complete the illusion of the ship flying through through the scene, you will also need to additionally move each mine in the opposite of the ship's direction, similar to the way you moved the stars. (1)
- Next, you should add some additional movement to each mine so that they appear to "home in" on the ship. (1)
- Finally, add some slow rotation to each mine so they appear to spin. (1)

#### Part 3: Laser Spawning (3 points)

- When the user clicks the mouse, a new instance of the laser object should be added to the scene. (2)
  
- When the laser is created, it should be rotated to point towards the mouse cursor, similar to the way the ship was pointed in that direction. (1)

#### Part 4: Laser Movement (3 points)

- In the update method, each laser instance should be translated forward so that it appears to be shooting out from the ship. (2)
- When a laser moves outside the boundary of the window, it should be removed from the scene.  We don't want an infinite number of lasers that can slow down our game! (1)

#### Part 5: Mine Collisions (3 points)

- Complete the code in the `checkForMineCollisions()` method to test for mine-to-mine collisions. (2)  
- When two mines intersect, they should be removed from the scene. (1)

#### Part 6: Laser Collisions (3 points)

- Complete the code in the `checkForLaserCollisions()` method to test for laser-to-mine collisions (2). 
- When an intersection occurs, the mine and the laser should be removed from the scene. (1)

#### Part 7: Documentation and Submission (1 point)

- To submit the assignment for grading, you will need to update your `README.md` file and then deploy to GitHub Pages, as described below. (1)

## Wizard Bonus Challenge

All of the assignments in the course will include great opportunities for students to go beyond the requirements of the assignment and do cool extra work. On each assignment, you can earn **1 bonus point** for implementing a meaningful new feature to your program. This should involve some **original new programming**, and should not just be something that can be quickly implemented by copying and slightly modifying existing code.

Here are a few examples:

- You could implement the logic to turn this into a fully functional game. This could involve making the mines capable of destroying the player's ship or providing an objective to "win" the game.
- You could program a new type of enemy or weapon, possibly by creating or importing new assets. [Kenney](https://www.kenney.nl/assets) is a great source for free 2D assets to use in game development.
- You could program an animation when mines are destroyed instead of making them instantly disappear from the scene.

A single point may not sound like a lot, but keep in mind that on a 20-point scale, this is equivalent to a 5% bonus! Make sure to describe your wizard functionality in your `README.md` file, so that the TAs know what to look for when they grade your program.

The wizard bonus challenge also offers you a chance to show off your skills and creativity!  While grading the assignments the TAs will identify some of the best examples of people doing cool stuff with computer graphics. We call these students our **wizards**, and after each assignment, the students selected as wizards will have their programs demonstrated to the class.

## Assignment Submission

When you have finished the assignment, you should complete the missing information in your `README.md` file. Then, will need to run the following command:

```
npm run submit
```

This compiles your TypeScript program into a JavaScript bundle that will be placed in the `submission` folder. **To complete your submission, you should commit the contents of the `submission` folder and then push to GitHub.**

The push to GitHub will trigger a server-side workflow that will automatically deploy your build as a website on GitHub pages. Note that you may need to wait a couple minutes for the deployment to become active. Make sure to test everything by pointing your web browser at the GitHub pages URL for your repository:

```
https://csci-4611-fall-2026.github.io/your-repo-name-here
```

Note that the published JavaScript bundle code generated by the TypeScript compiler has been obfuscated so that it is not human-readable. So, you can feel free to send this link to other students, friends, and family to show off your work!

## Schedule a Code Understanding Check

After you submit your code, you can schedule a meeting with a graduate TA for the code understanding check. Instructions for how to prepare and schedule this meeting will be posted soon.

## Acknowledgments

The ship graphics were from the Kenney [Space Shooter Remastered](https://kenney.nl/assets/space-shooter-remastered) and [Simple Space](https://www.kenney.nl/assets/simple-space) asset packages. The laser sound effect was obtained from [ZapSplat](https://www.zapsplat.com/).

## License

Public distribution of assignment source code outside the class is **prohibited**. If you want to showcase your work to potential employers, then you may distribute the link to the compiled build on GitHub Pages or share the source code with them **privately**.

