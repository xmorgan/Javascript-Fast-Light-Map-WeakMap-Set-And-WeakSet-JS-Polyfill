# Fast & Light Map & WeakMap Polyfill
This is a fast, lightweight (~1.2kb uncompressed, and only 509 bytes gziped) Javascript Map and Weakmap polyfill. If you don't want to download the script, then you can always just use the script hosted @ dropbox by inserting the following into your `<head>`.

```HTML
<script src="https://www.dropbox.com/s/rrsp5c8kmctklrj/mapPolyfill.min.js?dl=1" type="text/javascript"></script>
```

# When To & When Not To Use This Polyfill
This polyfill differs from the other polyfills in that it reasonably sacrafices standard-conformity for lightweightness and fastness. For example, this polyfill uses regular maps with a `length` getter added as a proxy to the `size` property as a polyfill to weakmaps. This may break some scripts which heavily probe the inner workings of Weakmaps. Also, this polyfill does not do Symbols, which should be an okay thing for the majority of production code.

# Browser Support
This polyfill should bring Map and WeakMap support to IE9.

# Sorta Important Memory-Leak Bug
A potential problem with this polyfill, or any other Map polyfills, are 'memory leaks'. For example, if you set an object key in a map to a value, then unreference that object key, then the object will still be in memory because of the reference to it inside this polyfills internal look-up table. For example, in the native implementation of javascipt maps, this should not consume much memory, but in the polyfill it will:

```Javascript
    var myMap = new WeakMap(), longString = 'some random strang ', i=100e+6;
    
    while (i--)
        myMap.set({}, longString += i)
    
    console.log(longString.length * myMap.length)
```

The unavoidable, unfixable reason for this is because there currently is no way other than WeakMaps to assign weak references/arrays to objects in javascript, which would allow the transpiler to know that its safe to ignore specific references to something.

# The <i>SingleWeakMapMinimal.src.js</i> file
This is some code I wrote for my own project to create an absolute bare minimalistic way to get a single weak map for referencing DOM elements. I thought someone else might find it useful/helpful, so I posted it up here. Enjoy.
