# Chess Algorithm Bishop
Given a chess board of 8 by 8 squares, alternating black and white which is represented by a one-dimensional list of length 64, this function determines whether a bishop in position x can take another piece in position y in one move.


## Solution

### Method 1

In this approach we do following :
- Validate the given positions
- The 8x8 chess board can be considered as a 2D coordinate system (0,0) to (7,7). We calculate the 2D coordinates of the two given positions.
- We need to calculate the slope between the 2 coordinates.
    The bishop can take another piece only if the other position is at 45 degree, 135 degree, 225 degree and 315 degree angles.
    The tangent of the one of these  angles must be equal to the slope between the points, i.e *slope = tan(angle)*
  
    We know,
  
        tan(45°) = 1 
        tan(135°) = -1  
        tan(225°) = 1 
        tan(315°) = -1
        
  Possible slope values for a bishop to take another pawn is 1 or -1. If the slope is any other value, the bishop cannot take the other position.
- ``True`` is returned if the slope between the two positions is -1 or 1, else ``False``.
- Order of magnitude - The order of magnitude of this function will remain the same given any valid chess board positions.

```python
def endangers(x: int, y: int) -> bool:
    if not ((0 <= x <= 63) and (0 <= y <= 63)):
        print("Enter valid numbers between 0 and 63!")
        return False
    elif x == y:
        print("Pawns cannot be at the same position!")
        return False
    else:
        # Calc the X and Y coordinate (or the rows and column location) of bishop at x
        xX = x % 8
        xY = x // 8  # Need to perform floor division 

        # Calc the X and Y coordinate (or the rows and column location) of another piece at y
        yX = y % 8
        yY = y // 8  # Need to perform floor division 

        # If the two coordinates are the same vertically or horizontally. Else will cause a division by 0 error.
        if xY == yY or xX == yX:
            return False

        # Take abs value of slope
        slope = abs((xY - yY) / (xX - yX))
        return slope == 1

    return False
```

### Method 2
In this approach we do following :
- Iteratively check all possibly location that the bishop can move to, to see if the other piece is present. 
- Locations are checked in the following order
    - All positions in the top left direction
    - All positions in the top right direction
    - All positions in the bottom left direction
    - All positions in the bottom right direction
    
- If ``checked_position``  is equal to the other piece's position, then ``True`` is returned, else `False`.
- Order of magnitude - The order of magnitude of the function will depend on the positions of the two pieces.
    - Since we are checking in the order of top left, top right, bottom left, and the bottom right, the worst case is if the other piece is in the bottom right direction to the bishop.
    - Since we are iteratively checking every point, the worst case situation would be if the other piece is in the bottom right direction at the max allowed distance from the bishop.
        Example:
        
            endangers(27, 63)
        This would take a total of 13 while loop runs to check if bishop can take the other piece's position. While,

            endangers(27, 18)
        would just take 1 while loop run to check if the bishop can take the other piece's position.
    
```python
def endangers(x: int, y: int):
    if not ((0 <= x <= 63) and (0 <= y <= 63)):
        print("Enter valid numbers between 0 and 63!")
        return False
    elif x == y:
        print("Pawns cannot be at the same position!")
        return False
    else:
        # top left. Start checking from bishop position.
        checked_position = x
        while checked_position >= 0:
            if checked_position % 8 == 0:
                if checked_position == y: return True
                break  # Max possible position in top left direction reached.
                # Stop checking further in this direction.
                
            elif 0 <= checked_position <= 7:
                if checked_position == y: return True
                break  # Max possible position in top left direction reached.
                # Stop checking further in this direction
            
            if checked_position == y: return True
            checked_position -= 9  # Calc next top left position

        # top right. Start checking from bishop position.
        checked_position = x
        while checked_position >= 0:
            if checked_position % 8 == 7:
                if checked_position == y: return True
                break  # Max possible position in top right direction reached.
                # Stop checking further in this direction.
            
            elif 0 <= checked_position <= 7:
                if checked_position == y: return True
                break  # Max possible position in top right direction reached.
                # Stop checking further in this direction.
            
            if checked_position == y: return True
            checked_position -= 7  # Calc next top right position

        # bottom left. Start checking from bishop position.
        checked_position = x
        while checked_position <= 63:
            if checked_position % 8 == 0:
                if checked_position == y: return True
                break  # Max possible position in bottom left direction reached.
                # Stop checking further in this direction.
            
            elif 56 <= checked_position <= 63:
                if checked_position == y: return True
                break  # Max possible position in bottom left direction reached.
                # Stop checking further in this direction.
            
            if checked_position == y: return True
            checked_position += 7  # Calc next bottom left position

        # bottom right. Start checking from bishop position.
        checked_position = x
        while checked_position <= 63:
            if checked_position % 8 == 7:
                if checked_position == y: return True
                break  # Max possible position in bottom right direction reached.
                # Stop checking further in this direction.
            
            elif 56 <= checked_position <= 63:
                if checked_position == y: return True
                break  # Max possible position in bottom right direction reached.
                # Stop checking further in this direction.
            
            if checked_position == y: return True
            checked_position += 9  # Calc next bottom right position

    return False
```
