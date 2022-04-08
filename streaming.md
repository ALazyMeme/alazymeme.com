# Streaming Tips

## Rough Bitrate Calculator
This rough calculator is based on the so called 'bit per pixel' or 'bpp' value that allows one calculate the bitrate based upon this value. Where at the **0.1 bpp** is the highest value to be used, as anything above will result in diminishing return and basically wasting bandwidth. **0.05 bpp**> should be considered the lowest value and rarely one should go below it for **gameplay**.

For variety casting, one does generally not want to go below **0.07 bpp** as this takes a wide variety of games into account that a variety caster can play and contains possible high motion gameplay, flashy vfx, foliage, or high detailed footage. Lower motion games, like HeartStone and simple platformers, can have a lower value to still look good, and creative casters can even go lower. If you see a lot of artifacting (blocky video), especially on movement, it means you have too little bitrate for the encode to look clean and should increase the bitrate.

Therefore, for variety casters, **900p60 @ 6000kbps** is popular as this results in the **0.07 bpp** value. Where **1080p60 @ 6000kbps** results in a **0.048 bpp** value. Of course this could work depending on the game. So personal testing is also required and fine tune on the game you play, do not blindly take over settings from a random guide!

(width x height x framerate x bpp) / 1000 = bitrate in kbps

<form id="bitrateCalculator">
    <label for="width">Width:</label><input type="number" id="width" name="width" placeholder="1600" minlength="3" maxlength="4" required="">  
    <label for="height">Height:</label><input type="number" id="height" name="height" placeholder="900" minlength="3" maxlength="4" required="">  
    <label for="fps">FPS:</label><input type="fps" id="fps" name="fps" placeholder="60" minlength="2" maxlength="2" required="">  
    <label for="bpp">BPP:</label><input type="number" id="bpp" name="bpp" placeholder="0.1" required="">  
    <input type="submit" value="Calculate">  
    <input type="text" id="result" name="result" placeholder="Calculated Bitrate" disabled=""> mbps
</form>
