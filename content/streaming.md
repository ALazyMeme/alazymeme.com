---
title: "Streaming Tips"
date: 2022-04-09T20:24:37+10:00
draft: false
type: page
---
<script>
    function calculate() {
        let width = Number(document.getElementById("width").value);
        let height = Number(document.getElementById("height").value);
        let fps = Number(document.getElementById("fps").value);
        let bpp = Number(document.getElementById("bpp").value);

        if (width < 1280 || width.length > 4) {
            return
        }

        if (height < 729 || height.length > 4) {
            return
        }

        if (fps < 30 || fps > 144) {
            return
        }

        if (bpp < 0.01 || bpp > 0.1) {
            return
        }

        let result = String((width*height*fps*bpp)/1000);
        let span = document.getElementById("result");
        span.innerText = result;
    }
</script>

## Rough Bitrate Calculator
This rough calculator is based on the so called 'bit per pixel' or 'bpp' value that allows one calculate the bitrate based upon this value. Where at the **0.1 bpp** is the highest value to be used, as anything above will result in diminishing return and basically wasting bandwidth. **0.05 bpp** should be considered the lowest value and rarely one should go below it for **gameplay**.

For variety casting, one does generally not want to go below **0.07 bpp** as this takes a wide variety of games into account that a variety caster can play and contains possible high motion gameplay, flashy vfx, foliage, or high detailed footage. Lower motion games, like HeartStone and simple platformers, can have a lower value to still look good, and creative casters can even go lower. If you see a lot of artifacting (blocky video), especially on movement, it means you have too little bitrate for the encode to look clean and should increase the bitrate.

Therefore, for variety casters, **900p60 @ 6000kbps** is popular as this results in the **0.07 bpp** value. Where **1080p60 @ 6000kbps** results in a **0.048 bpp** value. Of course this could work depending on the game. So personal testing is also required and fine tune on the game you play, do not blindly take over settings from a random guide!

(width x height x framerate x bpp) / 1000 = bitrate in kbps

<form>
    <label for="width">Width: </label><input type="number" id="width" name="width" placeholder="1600" maxlength="4" min="1280" required /><br>
    <label for="height">Height: </label><input type="number" id="height" name="height" placeholder="900" maxlength="4" min="720" required /><br>
    <label for="fps">FPS: </label><input type="fps" id="fps" name="fps" placeholder="60" maxlength="3" min="30" max="144" required /><br>
    <label for="bpp">BPP: </label><input type="number" id="bpp" name="bpp" placeholder="0.1" step="0.01" min="0.01" max="0.1" required /><br>
    <input type="submit" value="Calculate" onClick="calculate();" /><br>
    Result: <span id="result"></span> kbps
</form>

## How it Works
There is sometimes misconception with new streamers regarding what uses the GPU and CPU. In all cases, streaming software always uses both; regardless if you use hardware encoding (like NVENC, AMD, or QSV) or software encoding (x264). Hardware encoding uses dedicated hardware on the GPU or CPU to encode frames, that is seperate from normal GPU or CPU usage, not influencing the normal activity of those devices. Software encoding uses the normal CPU computational power to encode the received frames. So why does streaming software still need the "normal" CPU and GPU when using hardware encoding?

![Diagram of streaming software function](/img/streamingSoftware.png)

The above image is globally showing streaming software. Sources mainly run on CPU usage, especially browser sources which are known for sometimes causing high CPU usage. The composer takes these sources and creates the preview and composes frames for the encoder, and this happens entirely on the normal GPU and thus is shared with games and other applications. If a game is using the full bandwidth between CPU and GPU and/or GPU processing power, then the stream software will start lagging frames as it cannot properly prepare the frames for the encoder. The encoder takes these frames from the composer to encode them by the selected method. If one is using software encoding, and the CPU usage is peaking above 90% then you get dropped frames at encoding. Finally the encoder pushes the encoded frames on the network to the selected server.

## How can I keep my CPU/GPU usage "at bay"?
### Use Scenes as Sources
Ever wondered if you could group certain elements that belong together, and then move them around, transform and even apply filters to it? Well, that is exactly what you can do by using scenes as sources. After you created a specific "special" scene with only the elements required to be together, go to another scene and add a new source and select 'source', and lo-and-behold there is your group! For example, a webcam scene containing the webcam video capture, a frame, a webcam background and even a label with your name.

In addition to this, you are not duplicating or referencing, so a source (like browser or capture sources) are only used once. The same can be done for alerts, make a scene only containing various alerts, and add this scene as source in other scenes where you want to have alerts. No more hassle with double sounding alerts, or having to shut down sources when not active (which is only needed anyway when you duplicate instead of reference).

Finally, you need to make a change? That is amazingly easy as you only have the edit the "special" scene that is used by other scenes. So a quick edit to this fancy webcam frame, or adding top donator right under your webcam frame, it is all done with the ease of only editing one scene!

### Use Audio Sources
By default, audio devices are selected and setup within the settings of the application. These are considered global audio sources and will be always be active in any scene and using these will limit the control over your audio on a scene to scene basis.

Instead of using the global audio captures, **disable all** audio captures found in the audio section of the settings, and start using **Audio Input Capture** or **Audio Output Capture** sources. These sources are added per scene to scene so you exactly control which scene has what audio capture and eventually be pushed to the stream. For example, a break scene without the microphone audio input capture has autmatically a "muted" microphone when you swap to this break scene, no more hassle manually muting.

In addition you can control filters on the audio sources easier. Unlike video capture devices, some audio sources allows you to duplicate them so you can apply different filters per scene on the same actual audio input or output! You can combine this with tip #1 to have audio only in specific scenes that uses the scene that includes the audio capture. For example, a webcam scene add a microphone audio input capture to that scene, and any scene that uses this webcam scene will have an active microphone. Another (more) advanced application would be, to provide a virtual audio cable with background music (continously playing), that you add to your break scene. Then when you go to your break scene, your game audio (desktop audio) is faded out and the music is faded in by the transition.

### Record in 1080p, Stream in 720p
In case you were not aware OBS and Streamlabs have the ability to record on a higher resolution than the stream. This allows you to keep a copy of your stream in a higher resolution, and potentially better quality, for post-production purposes or archiving. This method might take a small hit on the encoding process, but it should be very minimimal and unnoticeable.

To start with, set your **Output (Scaled) Resolution** to the resolution you want to record at. This resolution will be fed to the encoders. The next step would be to lower the resolution of the stream by setting this up under **Advanced Output Mode**. In here you can select **Rescale Output** for Streaming and select the resolution you want to stream at. The recording one should be left unchecked, as that will take the resolution set with **Output (Scaled) Resolution**.

Of course you can do it the other way around to stream in a higher resolution than your recording, by using **Rescale Output** under recording instead of streaming. Do not use stream encoder as recording encoder with this setup, and I would suggest to always record with NVENC on a high bitrate (minimum 10000kbps for 1080p60) or CQP with a value of 18 to get good quality recordings.
