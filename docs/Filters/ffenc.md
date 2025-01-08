<!-- automatically generated - do not edit, patch gpac/applications/gpac/gpac.c -->

# FFmpeg encoder  
  
Register name used to load filter: __ffenc__  
This filter may be automatically loaded during graph resolution.  
  
This filter encodes audio and video streams using FFmpeg.  
See FFmpeg documentation (https://ffmpeg.org/documentation.html) for more details.  
To list all supported encoders for your GPAC build, use `gpac -h ffenc:*`.  
  
The filter will try to resolve the codec name in [c](#c) against a libavcodec codec name (e.g. `libx264`) and use it if found.  
If not found, it will consider the name to be a GPAC codec name and find a codec for it. In that case, if no pixel format is given, codecs will be enumerated to find a matching pixel format.  
  
Options can be passed from prompt using `--OPT=VAL` (global options) or appending `::OPT=VAL` to the desired encoder filter.  
Encoder flags can be passed directly as `:FLAGNAME`.  
  
Note  
Setting the `:low_delay` flag will set by default `profile=baseline`, `preset=ultrafast` and `tune=zerolatency` options as well as `x264-params` for AVC|H264. If one or more of these options are set as filter arguments, no defaulting is used for all these options.  
  
The filter will look for property `TargetRate` on input PID to set the desired bitrate per PID.  
  
The filter will force a closed gop boundary:  

- at each packet with a `FileNumber` property set or a `CueStart` property set to true.  
- if [fintra](#fintra) and [rc](#rc) is set.  

  
When forcing a closed GOP boundary, the filter will flush, destroy and recreate the encoder to make sure a clean context is used, as currently many encoders in libavcodec do not support clean reset when forcing picture types.  
If [fintra](#fintra) is not set and the output of the encoder is a DASH session in live profile without segment timeline, [fintra](#fintra) will be set to the target segment duration and [rc](#rc) will be set.  
  
The filter will look for property `logpass` on input PID to set 2-pass log filename, otherwise defaults to `ffenc2pass-PID.log`.  
  
Arguments may be updated at runtime. If [rld](#rld) is set, the encoder will be flushed then reloaded with new options.  
If codec is video and [fintra](#fintra) is set, reload will happen at next forced intra; otherwise, reload happens at next encode.  
The [rld](#rld) option is usually needed for dynamic updates of rate control parameters, since most encoders in ffmpeg do not support it.  
  

# Options  {.no-collapse}  
  
<div markdown class="option">  
<a id="c" data-level="basic">__c__</a> (str): codec identifier. Can be any supported GPAC codec name or ffmpeg codec name - updated to ffmpeg codec name after initialization  
</div>  
<div markdown class="option">  
<a id="pfmt" data-level="basic">__pfmt__</a> (pfmt, default: _none_): pixel format for input video. When not set, input format is used  
</div>  
<div markdown class="option">  
<a id="fintra" data-level="basic">__fintra__</a> (frac, default: _-1/1_): force intra / IDR frames at the given period in sec, e.g. `fintra=2` will force an intra every 2 seconds and `fintra=1001/1000` will force an intra every 30 frames on 30000/1001=29.97 fps video; ignored for audio  
</div>  
<div markdown class="option">  
<a id="all_intra">__all_intra__</a> (bool, default: _false_, updatable): only produce intra frames  
</div>  
<div markdown class="option">  
<a id="ls">__ls__</a> (bool, default: _false_): log stats  
</div>  
<div markdown class="option">  
<a id="rc">__rc__</a> (bool, default: _false_): reset encoder when forcing intra frame (some encoders might not support intra frame forcing)  
</div>  
<div markdown class="option">  
<a id="rld">__rld__</a> (bool, default: _false_, updatable): force reloading of encoder when arguments are updated  
</div>  
<div markdown class="option">  
<a id="round">__round__</a> (sint, default: _1_, updatable): round video up or down  

- 0: no rounding  
- 1: round up to match codec YUF format requirements  
- -1: round down to match codec YUF format requirements  
- other: round to lower (negative value) or higher (positive value), for example CTU size  
</div>  
  
<div markdown class="option">  
<a id="*" data-level="basic">__*__</a> (str): any possible options defined for AVCodecContext and sub-classes. see `gpac -hx ffenc` and `gpac -hx ffenc:*`  
</div>  
  
