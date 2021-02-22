# micropython_debounced_input
Micropython debounced input (momentary button/switch) driver.

* Callback on input press and release
* Callback provides duration of press and duration since last press
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
TBD

