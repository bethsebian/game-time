# Infinite Loop

This is a clone of the popular mobile game of the same name, which is itself a remake of the flash game Loops of Zen.

### Authors

[Penney Garrett](https://github.com/PenneyGadget)  
[Beth Sebian](https://github.com/bethsebian)  


## The Game
[Click here to play!](http://penneygadget.github.io/game-time/)

![infinite-loop](http://g.recordit.co/D8vOI0HKrw.gif)

## Running Locally
1. Install the dependencies: `$ npm install`
2. Fire up a development server: `$ npm start`
3. Once the server is running, visit the site at `http://localhost:8080/webpack-dev-server/`
4. Once the server is running, visit the test suite at `http://localhost:8080/webpack-dev-server/test.html`
5. To build the static files: `$ npm run build`
6. To run tests in Node: `$ npm test`

## Highlights
The Pattern Generator and Tadpole classes allow us to automatically generate patterns. Our approach had several steps:
  1. Selecting a canvas size based on the user's level.
  2. Before we pick shapes for all the tiles on the canvas, we create an object for every tile that stores constraints about what type of shape that tile could become. We initialize these objects with constraints based on where they're located on the canvas (for example, the top left tile on the canvas cannot be a shape that needs to connect to another shape either above or below it).
  3. We set shapes for every tile one-by-one, starting with the top left tile.
    * Create a lists of all the shape-orientation combinations that would work given that tile's constraints.
    * Randomly select a shape and orientation from that list of viable combinations.
    * Once that tiles shape is picked, that tile talks to its four neighbors and updates their constraints. For example, if a tile's sets its shape-orientation as a spoke with the connection pointing to the right (`0-`), the tile to the _right_ of that tile would now be required to be a shape that has a connection to its _left_.
    * Then the next tile, with updated constraints, sets its shape-orientation combination.
    * That loop repeats until all the tiles have picked a shape and orientation. This flow ensures that there are no unattached points in the pattern.

## Areas for Improvement
* Refactoring is needed in many files to simplify functions.
  * Extract large methods into smaller methods, since that would make it easier to unit test these functions. For example, see the [tadpole.js](https://github.com/bethsebian/game-time/blob/master/lib/scripts/tadpole.js) file.
  * Extract variables that serve as references for several methods into a separate reference file. For example, the [tilesByCoordinates](https://github.com/bethsebian/game-time/blob/master/lib/scripts/grid.js#L35-49#L53-L162) and [locationsIndex](https://github.com/bethsebian/game-time/blob/master/lib/scripts/grid.js#L35-49#L53-L162)
* Our test suite is pretty light right now. There are several features, such as the automated creation of patterns, that are not adequately tested.
* Convert shapes to SVG, so user's click causes them to visibly rotate, instead of snap to their new position.
* Bug fix: rewrite the method that evaluates if a user's click restores the canvas to its winning pattern. Our current code will only acknowledge a win if their arrangement exactly matches the pattern that the pattern generator created. In some scenarios, there are multiple arrangements given the tiles presented that will look to the user like winning combinations but will not trigger the user's advancement to the next level.




