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