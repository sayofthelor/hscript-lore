# hscript-lore
### Version of hscript modified for Lore Engine to expand upon its capabilities.

## Differences (for now)
- Types and metadata allowed by default
- Class importing works properly
- Package keyword at the front of a script is ignored
- Built-in method for calling functions

## Example Code
```haxe
var script:String = '
package testing.scripts.hscript; // gets automatically ignored

import Math;

function doAngles():Float {
  var sum:Float = 0;
  for (a in angles) {
    sum += Math.cos(a);
  }
  return sum;
}

function doAnglesWithInput(otherAngles:Array<Float>):Float {
  var sum:Float = 0;
  for (a in otherAngles) {
    sum += Math.cos(a);
  }
  return sum;
}
';
var parser = new hscript.Parser();
var expr = parser.parseString(script);
var interp = new hscript.Interp();
interp.variables.set('angles', [0, 1, 2, 3]);
interp.execute(expr);
trace(interp.callMethod('doAngles'));
trace(interp.callMethod('doAnglesWithInput', [[0, 1, 2, 3]]));
```

## Funny Example Code (hscript inside hscript)
```haxe
var script:String = '
import hscript.Interp;
import hscript.Parser;

var interp:Interp;
var innerExpr;
var parser:Parser;

var innerScript:String = \'
function doThing() {
  trace("...and hello from the inner script!");
}
\';

function init() { // technically could just be in body of script but i like cleanliness
  interp = new Interp();
  parser = new Parser();
  innerExpr = parser.parseString(innerScript);
  interp.execute(innerExpr);
}

function doHelloTest() {
  trace("Hello from the outer script...");
  interp.callMethod("doThing");
}
';
var interp = new hscript.Interp();
var parser = new hscript.Parser();
var expr = parser.parseString(script);
interp.execute(expr);
interp.callMethod('init');
interp.callMethod('doHelloTest');
/*
  output should be:
  hscript:23: Hello from the outer script...
  hscript:3: ...and hello from the inner script!
*/
```