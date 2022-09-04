# SnowyHype's Notes/ Guide on Making Engines with AngeTheGreat's Engine Simulator
*With information from multiple creators, including (but not limited to) [JCKF](jckf.no)*

## Where Do I Start Using The Simulator?

First and foremost, thank you very much for reading this and a special thank you for using the simulator\
Please before demanding support, read the [faq](https://github.com/ange-yaghi/engine-sim/wiki/Frequently-Asked-Questions).

Adding on to this, nobody is *required* to help you. Help is all voluntary.

[**Download New Engines**](https://catalog.engine-sim.parts/)


## Finding Cam and Crank Orders
Firing order is the order of which cylinders fire at what time.

### Example for firing order `1-4-2-3`:

```
Lobe 1 to go 1st. 1 - 1 = 0
Lobe 2 to go 3rd. 3 - 1 = 2
Lobe 3 to go 4th. 4 - 1 = 3
Lobe 4 to go 2nd. 2 - 1 = 1
```
## Finding Firing Intervals

Take 1 cycle (720°), and divide it by the amount of cylinders.

### Example for a `10` cylinder engine:
```
720 / 10 = 72
72 = 72° Firing interval
```

Your firing interval should be set as the angles between `Cam Lobes`, `Rod Journals`, and `Ignition Firing`.

## Finding Crank TDC (Top Dead Center)

What the heck is a TDC? 
Many people find them selves asking this, others asking why the heck this is needed? Well in the simulator, TDC effects ***everything*** related to timing your engine correctly. Crank TDC is the number of degrees you have to rotate your crank in order to bring piston number 1 to Top Dead Center of its compression stroke. 

`You can calculate it by finding the difference between the crank's spawn angle at -90°, and cylinder number 1's bank angle.`

### Example for an inline engine with your bank angle at `0°`:
Say cylinder #1 spawns facing all the way to the right, that means that your cylinder 1 is at 0° or any interval of 360° 
(... 360 , 720, ...) In that case, your bank is at 0° and that puts your cylinder 1 crank angle at -90°. That leaves 90° inbetween  the two values. which should be your TDC value.

Bank angle = 0
Cylinder 1 spawn angle = -90

`0 - -90 = 90`


## What is a main.mr??

`main.mr` is the file that the engine sim looks at to load engines into the simulator. The original file looks something like this,
```
import "engine_sim.mr"
import "themes/default.mr"
import "engines/kohler/kohler_ch750.mr"

use_default_theme()
set_engine(
    kohler_ch750()
)
```

But the original file runs an engine that wasn't built to move a 6000lb pickup. Changing the engine can be a little tricky, but here are some tricks to understanding `main.mr`!

First, lets break it down. In `main.mr`, you see a couple import lines. These lines import important files such as the engine sim library, the loaded theme, and an engine folder and file. Importing another engine it isn't very hard. 

### In depth explanation:

`main.mr` can be found in the assets folder.

Say your engine is found in: `assets > engines > mycustomfolder`
You'd have to import your engine as shown here: `import "engines/mycustomfolder/mycustomengine.mr`

But wait thats not all! Unless `mycustomengine.mr` has a public node containing the action to set your engine you'll have to put a little more effort into this file.

If your custom engine file has a public node containing the `set_engine` action, you just need to put `mycustomnode()` under everything.

Here's an example of this:
```
import "engine_sim.mr"
import "themes/default.mr"
import "engines/mycustomfolder/mycustomengine.mr"

use_default_theme()
mycustomnode()
```

If your engine file does not have a public node containing the `set_engine` action, you'll need to add that into your `main.mr` file and find the correct node name for the engine node. 

Here's an example of this:
```
import "engine_sim.mr"
import "themes/default.mr"
import "engines/mycustomfolder/mycustomengine.mr"

use_default_theme()
set_engine(mycustomenginenode())
```

If your engine file has a vehicle or a transmission node within it, it can be enabled by either adding it into `mycustomnode()` or by adding it into `main.mr` just like you did your engine but instead with, `set_transmission` and `set_vehicle`.

Here's an example of what `mycustomnode` should look like
```
public node main {
    set_engine(i4())
    set_vehicle(a_vehicle())
    set_transmission(a_transmission())
}
```
`main` is the node name\
`i4` is the engine node name\
`a_vehicle` is the vehicle node name\
`a_transmission` is the transmission node name\


## The Simulator Crashes Every Time I Open It!! Help?!

No need to worry, this just means that you made a code error. 

An error is made up of a couple different parts, to read an error you need to understand what those different parts mean.

Heres a quick example of what an error looks like:
```
main(3): error IO0010: Could not open file
main(7): error R0010: Undefined node type
```
 
In this error you see `main(3)` and `main(7)`. This indicates the file and what line of said file is giving you trouble.\
Next you see `error IO0010:` and `error R0010`. This indicates which error your code threw.\
Lastly you see `Could not open file` and `Undefined node type`. This translates that error code into english.

Please [refer to the wiki](https://github.com/ange-yaghi/engine-sim/wiki/How-to-read-the-error-log) to get more information on the different error types.

