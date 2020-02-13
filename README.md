### [Gist Code](https://gist.github.com/rodriggochaves/9e63b7cb98b724916633643ff4652cf2)


### 1) It is good to have attributes defined when begin the class. That way we know which attributes the class have. 

**Example: It is cool to put all the atributes the class have like below** 

```rb
require 'game_of_life/cell'

module GameOfLife
  class Board
    # Declare the attributes here, for more clarity
    attr_reader :cells :board_limit 
    
    def ...
 ```
  
  
### 2) Initialize attritutes in the constructor class and make it clear how they are going to be initialized when an instance is created.


First thing I notice, the class attributes was lost in the code,
what I mean is, one variable was inside a private function and was hard to find out the properties of the class, bacause that was no declaration of the **@board_limit** attribute.

On **def initialize cell_quantity** is good to have all the class attributes initialized when the instance is 
created.

**Before:**

```rb
   def initialize cell_quantity
      set_board_limit(cell_quantity)
      cells = (0..cell_quantity - 1).to_a.map do |x|
        (0..cell_quantity - 1).to_a.map do |y|
          Cell.new(x, y)
        end
      end
      @cells = cells.flatten
    end
```

- if the class attributes is defined, you could divide the attributes assignments on the constructor using privates methods if some kind of code needs to be abstracted, or you can assignment directly like below. 

**After:**

```rb
    def initialize cell_quantity
      # No function needed for this atribution, is simple and better just make the atribuition inside the method initialize
      
      @board_limit = cell_quantity - 1

      cells = (0..cell_quantity - 1).to_a.map do |x|
        (0..cell_quantity - 1).to_a.map do |y|
          Cell.new(x, y)
        end
      end

      @cells = cells.flatten
    end

```
 
- What could make this confuse in this code is that some attributes are lost inside of private classes, what make this confuse when atributes are located in diferentes places on the code. Cause is hard to find where are the variable was set on the fisrt place.
 
- This is a example of code that have a variable seted outside of the constructor, and makes kind hard to connect to the class, cause this variable don't even is declared on the begin of the class.

- Code:

 ```rb
    private def set_board_limit cell_quantity
      @board_limit = cell_quantity - 1
    end
 ```
 
### 3) The **def neighbors** could be a constant or a global variable

- The neighbors information is something that doesn't change, so, this part is lost cause it's look like a properties of the class somehow, or part of the solution but not necessarily something that I have to calculate or change. 
I have to access the neighbors to calculate somenthig else, so, I think makes more sense if we make this a *constant* that will be accessed for any method that needed.

- Putting **neighbors** on the middle of the code makes the programmer thing, Is this a function? But why is a function if is just returning a value? 
We don't need to call a function every time we need to search for the **neighbors**, so let's put this on the constant, easy to reach and understand.

**Code: **
```rb
  def neighbors
    [
      { x: - 1, y: - 1 },
      { x: + 0, y: - 1 },
      { x: + 1, y: - 1 },
      { x: - 1, y: + 0 },
      { x: + 1, y: + 0 },
      { x: - 1, y: + 1 },
      { x: + 0, y: + 1 },
      { x: + 1, y: + 1 },
    ]
  end
  ```
  
  **Sugestion:**
  ```rb
  NEIGHBORS =  [
      { x: - 1, y: - 1 },
      { x: + 0, y: - 1 },
      { x: + 1, y: - 1 },
      { x: - 1, y: + 0 },
      { x: + 1, y: + 0 },
      { x: - 1, y: + 1 },
      { x: + 0, y: + 1 },
      { x: + 1, y: + 1 },
    ]
    ```
 

