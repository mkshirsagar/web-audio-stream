Web Audio API writable stream. Provides interface between Web Audio API and node streams. Send any PCM data to web-audio via write/pipe.

## Usage

[![npm install web-audio-stream](https://nodei.co/npm/web-audio-stream.png?mini=true)](https://npmjs.org/package/web-audio-stream/)

```js
var WAAStream = require('web-audio-stream');
var context = require('audio-context');

//create stream instance
var stream = WAAStream({
	context: context,
	channels: 2,
	sampleRate: 44100,

	//BUFFER_MODE, SCRIPT_MODE, WORKER_MODE (pending)
	mode: WAAStream.BUFFER_MODE,

	//disconnect node if input stream ends
	autoend: true
});


//use as AudioNode
stream.connect(context.destination);
stream.disconnect();


//or as stream
var chunk = new Float32Array(1024);
for (var i = 0; i < 1024; i++) {
	chunk[i] = Math.random();
}
stream.write(chunk);


var Generator = require('audio-generator');
var src = Generator(function (time) {
	return Math.sin(Math.PI * 2 * time * 440);
});
src.pipe(stream);


setTimeout(stream.end, 1000);
```

Stream is smart enough to recognize any type of data placed into it: audioBuffer, arrayBuffer, float32Array, buffer, array. Make sure only that passed buffer format complies with the one indicated in options.


## Related

* [audio-speaker](https://github.com/audio-lab/audio-speaker) — node/browser speaker stream.
* [audio-through](https://github.com/audio-lab/audio-speaker) — universal audio stream class.