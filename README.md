# small-jsfuck
Generate small jsf code - [RUN it HERE](https://kamil-kielczewski.github.io/small-jsfuck)

## Goal

The idea of converting JS code to only 6 characters `[]()!+` was implementend in [JSFuck](https://github.com/aemkei/jsfuck) for theoretical reasons. This project was created to make jsfuck more practical by decrease size of generated code.

## Algorithm

* Get input source-code string and convert each character code to four base4 (not confuse with base64!) numbers (each can have value 0,1,2 or 3) and write it as 4-digit string e.g 
```js 
"c".charCodeAt(0).toString(4).padStart(4,0)  --> "1203"
```
* Convert each digit to value which have smallest JSfuck representation using following map 
```js
0 --> 0       jsf:   (+[])
1 --> 1       jsf: (+!![])
2 --> false   jsf:    +![]
3 --> true    jsf:   +!![]
``` 
e.g. `"c" --> "1203" --> "1false0true"`

* Encode such mapped values to jsf by add `[]` to beginning and join by `+` e.g. 
```js
"c" --> "1203" --> "1false0true" --> "[]+(+!![])+![]+(+[])+!![]". 
```
Steps 2 and 3 are done by following code which produce `MIDDLE_STEP_CODE= []+(+!![])+![]+(+[])+!![]`  
```js
"[]"+ "1203".replace(/0/g,"+(+[])").replace(/1/g,"+(+!![])").replace(/2/g,"+![]").replace(/3/g,"+!![]");
```
(whe add `"[]"` at the beginnig only on first character in whole code string)

* Include additional code (STUB) to make inversion of MIDDLE_STEP_CODE and run it:
```js
Function((MIDDLE_STEP_CODE).replace(/true/g,3).replace(/false/g, 2).match(/..../g).map(x=>String.fromCharCode(parseInt(x,4))).join(""))()
```
  after (jsf) optimalization it looks like follows
```js
[]["flat"]["constructor"](MIDDLE_STEP_CODE)["split"](true)["join"](3)["split"](false)["join"](2)["match"]([]["flat"]["constructor"]("return/..../g")())["map"]([]["flat"]["constructor"]("return f=>String.fromCharCode(parseInt(f,4))")())["join"]([]))()
```
  in above code finally we need to convert all strings (like "flat", "constructor",...) to jsf, true/fale and nubers, and paste MIDDLE_STEP_CODE to get final result. Above run and inverse conversion code gives ~23kB overhead.

If in above long line of code we remove code before `(MIDDLE_STEP_CODE)` and also remove last `)()` (at the end of line) - then after run this we get deconverted string (which will be not executed)

## Benefits

In this way each character change to 4 digits, each digit change to 4-8 jsf characters. So in worst case each character is converted to `4*8=32` jsf characters, in best case it is converted to `4*4=16` characters. So 1kB of code will grow to 16-32kB after conversion to jsf. It is quite efficient if we compare this to direct jsf conversion.

## Credits

Thanks to [aemkei](https://github.com/aemkei/jsfuck) for creating [jsfuck](http://www.jsfuck.com/), and for [hazzik](https://github.com/hazzik) who kindly aswer my questions in issues in jsfuck repo (like [this](https://github.com/aemkei/jsfuck/issues/100#issuecomment-679378602))

Big thanks for below StackOverflow comunity members who help me solve jsfuck problems during deconverter writing

* [trincot](https://stackoverflow.com/users/5459839/trincot) for answers: [1](https://stackoverflow.com/a/63603113/860099), [2](https://stackoverflow.com/a/63604570/860099), [3](https://stackoverflow.com/a/63605950/860099) and [4](https://stackoverflow.com/a/63636251/860099)
* [Siguza](https://stackoverflow.com/users/2302862/siguza) for answer [1](https://stackoverflow.com/a/63675158/860099)
* [Jonas Wilms](https://stackoverflow.com/users/5260024/jonas-wilms) for answer [1](https://stackoverflow.com/a/63618378/860099)


