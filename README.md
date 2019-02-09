Advanced Offshore Streaming 2019
Created for a requirement to have decode realtime h265 nal streams. 

Live streaming can now be acheived by pushing data directly into the decoder after calling 	player.startStreamingDecoder(decoder);



## Live streaming example :

<!DOCTYPE html>
<html>
<head>
<meta charset='utf-8'>
<title>H265 Live Player</title>

<body>
    <h1>libde265.js</h1>
    <div>


    <button id="play" onClick="JavaScript:playback()">Play</button> <span id="status"></span>
	</div>

		<canvas id="video" width="640" height="480"></canvas>
		</div>

		<script src="assets/plugins/jquery/jquery-3.3.1.min.js"></script>
		<script src="assets/plugins/jquery-ui/jquery-ui.min.js"></script>
		<script type="text/javascript" src="assets/Player/libde265.js"></script>
		<script src="/socket.io/socket.io.js"></script>
		<script src="/assets/Player/Canvas/YUVCanvas.js"></script>

		<script>
		<!--
			// Socket IO
			var socket= io();

			var decoder={};
			var video;
			var status;
			var player = null;

		$(document).ready(function() {

			// Give the canvas to the video player (decoder)
			var video = document.getElementById("video");
		  var status = document.getElementById("status");

			player = new libde265.RawPlayer(video,null);


			player.set_status_callback(function(msg, fps) {
					switch (msg) {
					case "loading":
							status.innerHTML = "Loading movie...";
							break;
					case "initializing":
							status.innerHTML = "Initializing...";
							break;
					case "playing":
							status.innerHTML = "Playing...";
							break;
					case "stopped":
							status.innerHTML = "";
							break;
					case "fps":
							status.innerHTML = Number(fps).toFixed(2) + " fps";
							break;
					default:
							status.innerHTML = msg;
					}



			});

		// get a new Decoder

			player.startStreamingDecoder(decoder);

		});

		var running=false;
		function playback(){
			if (running==false){
			socket.emit('f',{function:'getStream',feed:'1'})
			running=true;
		}
		else running=false;
		}
			//on socket.io connection success



			socket.on('stream', function (data) {

				if(running==true){
			var data= new Uint8Array(data);
				decoder.decoder.push_data(data);
			}
			});




			-->

		</script>


</body>
</html>


From the original developers : 
# libde265.js

Pure JavaScript HEVC/H.265 video decoding library using libde265.

Compiled from libde265 using Emscripten. Should run in all current
browsers like Google Chrome 33+, Firefox 28+, IE 11+, Opera 20+ and
Safari 7+ on OSX Mavericks. Older versions might work, but this is
mostly untested.

NOTE: This is a very early preview which needs more testing and lots of
optimizations!

## Building

(currently only tested on Linux)

- Install [Emscripten][1] and put into your `PATH`
- Execute the `build.sh`, this will download and compile libde265 using
  Emscripten and will generate the `libde265.js` file.
- If the version of your default LLVM is below 3.2, you might need to
  install the package `llvm-3.2` (or newer) and set the environment
  variable `LLVM_ADD_VERSION` to `3.2` (or whatever you installed).

## Examples

A small example can be found in the `demo` folder and on
https://strukturag.github.io/libde265.js/.

## Known issues

- More code from libde265 should be made asm.js aware
- Decoding should be made asynchronous through WebWorkers where available

[1]: http://emscripten.org

Copyright (c) 2014 struktur AG
