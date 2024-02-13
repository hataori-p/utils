# utils

Utilities for working with vocal files for Windows command line.

>2 vendors of Virustotal.com flag the files as malicious - don't worry, please check the checksums

YouTube video tutorial https://youtu.be/Yw698k9BysE

## fmid.exe - Middle/center from stereo signal extractor
<sup>167424 bytes, CRC32: 6F33AFB0, CRC64: 5EC1B4514AA28191, SHA256: 10ED1422D2827A441A9AB3E52097068EAF583268BFE6FE2B880944FC46784377</sup>

```
fmid <stereo.wav> <out_middle.wav>

input file: RIFF wave PCM, 2 channels, 16 bit/ch
```
* mono and side M/S signals are computed from stereo LR: M = L + R, S = L - R
* then side is spectrally subtracted from mono using FFT, only spectral magnitudes are subtracted, phase of the side is set to zero

Output should be middle/center signal without everything panned to sides (eg vocal harmonies)

## fsub.exe - Signal spectral subtractor
<sup>171008 bytes, CRC32: 5D82FC90, CRC64: 2BE91A796D756F7A, SHA256: 20CF5DCF9707D9000B9BFD4139528AC0BB85ED3537B7F2D782EBB61D601B4288</sup>

```
fsub <source.wav> <to_subtract.wav> <difference_output.wav> [<gain,def=1.0>] [<delay,def=0 samples>]

input files:  RIFF wave PCM, 1 or 2 channels, 16 bit/ch
if to_subtract is stereo, will be converted to mono when source is mono
output file channels same as source
gain of the subtracted file, uses def 1.41 when stereo-mono
delay in samples applied to subtracted file, can be negative
```

Spectral subtraction of 2 signals, magnitudes are subtracted, first file retains phase, phase of the second file is set to zero

When subtracting mono signal from stereo, it is important to set gain to 1.41 (square root of 2) to compensate

You can use higher gain but for the price of distortion.

Subtract the midd/center mono vocal (from fmid.exe see above) from all vocals (got from stem separation sw) - you receive only side signal

## foctave.exe - One octave up voice converter
<sup>52736 bytes, CRC32: 361A8E06, CRC64: 4F5AB043A23B87B5, SHA256: B3504AE4C6289AD8A6EDE372B864503F790ACB0F359663F238760C8A78F4EB67</sup>

```
foctave <pitch.txt> <input.wav> <output.wav>
```
* input pitch: Praat's short text pitch object format containing analyzed pitch of the input wav
* input wav file: RIFF wave PCM, 1 channel (mono), 16 bit/ch, singing voice or speech
* output file will contain only even harmonics of the input signal, effectively shifting it up one octave with speaker's vocal characteristics retained
