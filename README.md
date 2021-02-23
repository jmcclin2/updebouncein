# micropython_debounced_input
Micropython debounced input (momentary button/switch) driver.

* Callback on input press and release
* Callback provides input state, duration of press, and duration since last press
* Adjustable debounce period
* Configurable for logic high or low press  

## Usage
Simple example of single momentary button on pin 28; external button is normally open with one contact tied to 3.3V and the other to pin 28.

```python
from machine import Pin
from debounced_input import DebouncedInput

# Define button press/release callback
def callback(pin, pressed, duration_ms):
    if (pressed):
        print("Pin-", pin, " Pressed:", duration_ms, "ms since last press")
    else:
        print("Pin-", pin, " Released:", duration_ms, "ms long press")
    
button = DebouncedInput(28, callback, pin_pull=Pin.PULL_DOWN)
```

## Callback
The first callback parameter is the pin number associated with the event; this allows a single callback function to be used with several buttons which may simplify input handling in the application.  The second parameter is the state of the input; ```True``` for pressed/active, ```False``` for released/in-active.  The definition of pressed/released depends on how the input is wired and the state of the constructor parameter ```pin_logic_pressed``` (see corresponding section on configurable logic state for high or low press).  The thrid parameter ```duration_ms``` is either the duration since the last button press (if ```pressed``` is ```True```), or the duration of the button press (if ```pressed``` is ```False```).

## Debounce
The constructor provides an optional named argument ```debounce_ms```.  The default is 100ms.  The driver installs an interrupt for the specified pin which fires on both the rising and falling edge.  In handling the interrupt resulting from either edge, the driver disables the interrupts for the specified pin, and starts the debounce timer. Once the debounce timer expires, the driver samples the level of the pin, compares it to the expected state, updates the duration timer and calls the user's callback function.

## Configurable Logic (high/low) For Button Press
The constructor provides an optional named argument ```pin_logic_pressed``` which is ```True``` by default.  This parameter informs the driver what the logic state of the pin should be when the button is pressed.  If ```pin_logic_pressed``` is ```True`` the driver checks the actual pin input for a high level to associate with the pressed event.  If ```pin_logic_pressed``` is ```False`` a low level is associated with the pressed event.  The constructor also provides an optional named argument ```pin_pull``` which is ```None``` by default.  The parameter can be set to ```Pin.PULL_DOWN``` or ```Pin.PULL_UP``` as required.


