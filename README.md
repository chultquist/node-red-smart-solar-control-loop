# node-red-smart-solar-control-loop

A smart ESS control loop for Victron solar PV systems. This was born because I found
that none of the pre-packaged ESS algorithms worked for me, especially with LiFePo4
batteries where BatteryLife doesn't make as much sense. The BatteryLife algorithms are
also either: 1) slow to react (the optimized version only adjusts minimum SoC by 5%
per day, meaning that you cannot quickly change strategy for the odd sunny/cloudy day),
or 2) don't work well in the South African context where we have to deal with loadshedding
and so you may at times want to deliberately pull from the grid in order to charge the
battery so that the SoC is sufficient to see you through a period of the grid being
unavailable. I also found that the BatteryLife algorithms would do silly things like charge
my batteries from the grid at the start of the day (after running them low overnight),
even though there was plenty of solar PV forecast for the day.

## Important notes

This is a work in progress, and is not at all polished. I make no representations about
whether this code will work. You are advised to monitor yourself closely, especially
when the battery SoC nears 20% and make sure that the inverter starts (correctly) drawing
from the grid rather than fully depleting your battery. I take no responsibility for any
damage that may be caused as a result of the code contained herein.

## Pre-requisites

In order to use this package, you will need the following:

 - a Victron-based system where Node-RED is able to communicate with the Victron hardware.
   I think technically this requires a controller such as a Cerbo GX (which is what I
   have), but I'm not an expert and so not 100% sure

 - an API token from EskomSePush (you can get a free subscription at
   https://eskomsepush.gumroad.com/l/api), and the numerical area ID for your area in
   the EskomSePush database. You can use the API to work this out; if many people find
   this difficult to do I'll add instructions.

 - an API key for Solcast's rooftop PV offering (see https://solcast.com/free-rooftop-solar-forecasting
   to sign up for a free hobbyist account for your home. Make sure to accurately fill in
   all the details for your PV setup).

 - a VRM token to lookup your historical load data, and your VRM installation ID

## Usage

**NOTE:** this is still very much a work in progress. In time, the plan is to package this
properly into a NPM module for easy use (at which point in time the instructions below
should work). For now, if you want to use this you'll need to manually import the subflow
JSON and may need to do some tweaking to get it to work properly.

Once you have all the pre-requisites ready, simply add the Smart Control Loop node to one
of your flows and double-click to configure it. In addition to the tokens etc above, you will
need to provide the capacity of your battery in watts (I haven't found a reliable way of
querying for this yet).

The first output from the node is the key one ("setpoint"), and should be connected to a
"AC Power L1 setpoint (W)" node which you can create from a "VE Bus System" Victron node.
Because this seems to be dependent on exactly what type of inverter you have, I haven't
(yet) been able to automate this.

The other outputs are all informational & will be mostly helpful in providing me with data
should you encounter any bugs :)

