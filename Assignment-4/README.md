# Assignment 4: So You Think Ants Can Dance

**Due: Monday, November 23, 11:59pm Central Time**

Animated characters are an important part of computer games and other interactive graphics. In this assignment, you will learn how to animate computer graphics characters using data from motion capture systems. You will be working with data from the [Carnegie Mellon motion capture database](http://mocap.cs.cmu.edu/), a great resource of free "mocap" data. The data will be formatted as text files, but with your programming and math skills, you will bring this data to life on the dance floor!

The CMU motion capture data is typical of all the skeleton-based motion capture data found in games and movies. So, gaining some experience with this type of animation is one of the most important goals of the assignment. There are several important learning goals, including:

In completing this assignment, your goal should be to learn:

- How transformations can be composed in a hierarchy to create a scene graph and/or, in this case, an animated character within a scene.
- How transformations can be used to scale, rotate, and translate basic shapes (unit cubes, spheres, cylinders, and cones) into more complex scenes and characters.
- How mocap data can be used and manipulated in multiple ways to create different types of animations. For example:
  - How to create a looping animation that smoothly interpolates between the beginning of the motion clip and the end to avoid any discontinuities.
  - How to overlay new motion clips onto a character at runtime, for example, making your character jump in a game when you press a button, or in our case, perform one of a series of cool ballet moves.
- How to read and extend some fairly sophisticated computer graphics code.

The amount of code you will need to write to complete this assignment is less than in the previous assignments. The support code provides quite a bit of infrastructure to deal with reading and playing back the mocap data. So, the key challenges will come in reading through and understanding the existing code as well as really thinking about the code that you do write. You will probably need to work out some of the structure on paper before sitting down at the keyboard to program.

You can try out the [instructor's implementation](https://csci-4611-fall-2026.github.io/Assignments/Assignment-4/dist) in the Builds repository on the course GitHub.

## Repository Setup

We are using GitHub for submission of programming assignments.

**Step 1:** Create your private repository using the following link: https://classroom50.org/CSCI-4611-Fall-2026/csci-4611/assignments/assignment-4/accept

**Step 2:**  The system will then create a new private repository with starter code that is only accessible by you, the instructor, and the TAs. You can then use `git` to check out the code to your local machine. If you prefer a GUI application instead of the command line, then I recommend using [GitHub Desktop](https://github.com/apps/desktop) or Visual Studio Code's integrated source control.

## Prerequisites

To work with this code, you will first need to install [Node.js 24.21.0 LTS](https://nodejs.org/en/download) (or newer) and [Visual Studio Code](https://code.visualstudio.com/). 

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

## Data

The CMU Mocap database contains 2,605 different motions, most recorded at 120Hz, but some recorded at 60Hz or other speeds. These motions range from the simple (person walking straight forward), to the complicated (directing traffic), to the silly (someone doing the "I'm a little teapot" dance).

The motions in the CMU database use skeletons specified in .asf files and separate motions specified in .amc files. The .asf files specify bone names, directions, lengths, and the skeleton hierarchy for one specific human subject who came to the mocap lab. That person likely performed several motions during the capture session, so there is typically one .asf file for multiple .amc files. The subjects are numbered (e.g., subject #50), and the skeleton files are named accordingly (e.g., 50.asf). Motion filenames start with the subject ID, then have an underscore, then the number of the motion (e.g., 50_01.amc is the first motion captured for subject 50). The support code comes with the data files we used in our solution to the assignment, but it can be fun to swap in other motions, and you are encouraged to experiment with this by downloading other .asf and corresponding .amc files from the [CMU database](http://mocap.cs.cmu.edu/).   If you are interested, you can also read more about the [Acclaim Motion Capture](http://graphics.cs.cmu.edu/nsp/course/cs229/info/ACCLAIMdef.html) data format used by the mocap files.

The animations used in this assignment are based on raw mocap data that contains noticeable glitches.  For example, you may notice the ballet dancer's limbs sometimes rapidly jump back and forth between incorrect positions, which also occurs in the instructor's implementation. These visual glitches often occur when the cameras could only capture a partial view as the person moves around. In a production pipeline, the raw mocap data would be manually cleaned by an animator before being integrated in an application. You **do not** need to worry about the mocap glitches during this assignment.

## Program Requirements

The starter code sets up two different "scenes" with animated dancing characters. Your assignment is to construct the hierarchical geometry for these characters and complete some of the code that controls animation playback.

In the *Ballet Studio* scene, we should see a single animated character that responds dynamically to the buttons in the GUI. This is a simplified version of what you could expect in a game where a character is controlled dynamically by a gamepad. The character should repeat a looping "idle" animation, waiting for the user to control them in some way. Then, the character should respond when we press the corresponding buttons in the GUI. The character should smoothly transition from the idle motion to the new motion, and when the new motion is done playing, they should transition back to the idle motion. In this way, the *Ballet Studio* is intended to illustrate how to work with mocap data in a dynamic context. This scene is also really useful for debugging because the GUI includes a dropdown list you can use to switch between characters. You are welcome to add more characters, but you need to implement at least the three shown in the dropdown. The characters you will create in `AxesCharacterGeometry` and `SkeletonCharacterGeometry` should be useful for debugging your code. For the third character, you are welcome to create an ant, like the instructors' implementation, or you can rename `AntCharacterGeometry` and make your own custom character (subject to the specific requirements in the rubric below).

The second scene is the *Salsa Class*. This scene should show two characters dancing together. This animation is not dynamically controlled. So, in a sense, it is a simpler situation than in the *Ballet Studio*, but it should look quite convincing to see the multiple characters dancing together.

For this assignment, we want to emphasize that **you do need to read the support code**. Reading through all of the code provided to learn the structure of the program is part of this assignment. Try to understand how the skeletal data are stored and how the motion data is stored in a `Keyframe`. Understand what happens when you ask the character's `AnimationController` to play and overlay an `Animation`. Understand how custom character geometry can be created by adding meshes as children of a specific `Bone` in the `Skeleton`.

Once you understand these aspects of the support code and animation system, we suggest proceeding with your implementation by following the steps described in the rubric below.

## Rubric

Graded out of 20 points.  Partial credit is possible for each step.

**Part 1: AxesCharacterGeometry.ts** (5 points total)

- Create a `createGeometryRecursive(bone: Bone)` method that draws coordinate axes at the bone’s local origin. (1 point)
- In `createGeometry()`, call this helper for each root-level `Bone` child of the skeleton. Within the helper, recurse through each child that is a `Bone` to traverse the entire skeleton. (2 points)
- Each `Bone` node’s local origin is located at the tip of that bone. The vector `bone.direction` points from the parent toward the tip, so the direction back toward the parent is `-bone.direction`. Geometry attached to a bone should use this convention, so you will need to rotate the axes mesh so that its blue (+Z) axis aligns with `-bone.direction`, as shown in the instructor's implementation. (2 points)

**Part 2: SkeletonCharacterGeometry.ts** (6 points total)

- Following the traversal pattern from Part 1, create a `createGeometryRecursive(bone: Bone)` method that adds a cylinder mesh for each bone, and initiate the traversal from `createGeometry()`. (1 point)
- Scale, rotate, and translate the cylinder so that one end is at the bone’s local origin and the other end extends toward its parent, along `-bone.direction`, with a length equal to `bone.length`. (3 points, 1 for each transformation)
- Lastly, compose the matrices in the correct order and apply the resulting matrix to the cylinder mesh using `setLocalToParentMatrix()`. (2 points)

If this part is correct, a stick figure should show up when you choose the Skeleton character, as shown in the instructor solution. There should not be gaps between bones, although you will see the end caps of the cylinder when the joint bends a lot.

**Part 3: AntCharacterGeometry.ts** (6 points total)

- **Part 3.1:** Create a custom humanoid character (it can be an ant like the example solution or some other character you design yourself) by adding geometries for specific bones in the skeleton. If the geometries are big, as in the ant in the instructor's solution, then you may not need to define a geometry for *every* bone; do what looks good for your character. At a minimum, create four mesh instances with distinct combinations of translation, rotation, and scale to form your custom character. Not every instance needs all three transformations, but your character must demonstrate all three. (3 points)

- **Part 3.2:** Create a face for your character under the "head" bone. Note, there are no bone placeholders for the eyes and other facial features, so this is a chance to show us you know how to compose matrices together to create a more sophisticated 3D model. Your character’s face should contain at least three mesh instances representing facial features, such as eyes, a mouth, or antennae. These may reuse the same primitive shape. At least one facial-feature mesh must use translation, rotation, and scaling. (3 points)

*Note that although the class is named AntCharacterGeometry, you are not required to create an ant.  If you have another creative idea, you may want to rename this class to better describe your implementation.*

**Part 4: App.ts**. (3 points total)

- **Part 4.1:** Load 4 additional animations from the CMU motion database. (1 point)

- **Part 4.2:** Trim each new animation so there is no “idle” time at the beginning and end of each clip. (1 point)

- **Part 4.3:** Add each animation to the ballet character's overlay queue when the appropriate GUI button is clicked. (1 point)

**Submission**

- To complete the submission, you will need to update your `README.md` file, build your project, and then commit and push both your source code **and** the contents of the `submission` folder to GitHub, as described below.  (-1 point deduction for skipping these steps)

## Wizard Bonus Challenge

All of the assignments in the course will include great opportunities for students to go beyond the requirements of the assignment and do cool extra work. On each assignment, you can earn **one bonus point** for implementing a meaningful new feature to your program. This should involve some original new programming, and should not just be something that can be quickly implemented by copying and slightly modifying existing code.  

There are great opportunities for extra work in this assignment. For example:

- You could browse through the [CMU mocap database](http://mocap.cs.cmu.edu/), download more skeleton (.asf) and motion clip (.amc) files, and then add additional characters to the scene.
- You might consider extending one of the characters with mouse and/or keyboard input.  For example, you could make a character that is animated using mocap data, but moves in the direction controlled by the user.
- You could create or download some 2D sprite animations and add them to the 3D scene, as discussed in class.
- You could extend the animation system with your own unique idea. Creativity is encouraged!

If you do any wizardly work, please describe it in your `README.md` file so we know what to look for!

## Assignment Submission

When you have finished the assignment, you should complete the missing information in your `README.md` file. Then, you will need to run the following command:

```
npm run submit
```

This compiles your TypeScript program into a JavaScript bundle that will be placed in the `submission` folder. **To complete your submission, you should commit your source code (`src` folder) and compiled build (`submission` folder), then push to GitHub.**

The push to GitHub will trigger a server-side workflow that will automatically deploy your build as a website on GitHub pages. Note that you may need to wait a couple minutes for the deployment to become active. Make sure to test everything by pointing your web browser at the GitHub pages URL for your repository:

```
https://csci-4611-fall-2026.github.io/your-repo-name-here
```

Note that the published JavaScript bundle code generated by the TypeScript compiler has been obfuscated so that it is not human-readable. So, you can feel free to send this link to other students, friends, and family to show off your work!

## Complete the Code Understanding Check

After you submit your code, you will need to complete the code understanding check. This will be posted on Canvas after the assignment due date.

## Acknowledgments

- Original assignment concept developed by Daniel Keefe and TAs 2012, 2014.

- Ported to MinGfx and additional refinements by Daniel Keefe and TAs 2018-2021.

- Ported to GopherGfx and additional refinements by Evan Suma Rosenberg 2022-2023.
- Significant revisions to emphasize use of transformation matrices by Daniel Keefe 2023.
- Significant revisions and animation controller classes by Evan Suma Rosenberg 2024-2026.

## License

Public distribution of assignment source code outside the class is **prohibited**. If you want to showcase your work to potential employers, then you may distribute the link to the compiled build on GitHub Pages or share the source code with them **privately**.
