[WAV - Waveform Audio File Format](https://docs.fileformat.com/audio/wav/)

WAV files are a common file format for representing audio.
* begin with a 44-byte *header*
   information that about the file itself, like the size of the file, the number of samples per second, the size of each sample, etc.
* after the *header*, it's the *sequence of samples*
   each a single 2-byte integer representing the audio signal at a particular point in time

Scaling each sample value can change the volume of the audio.
* multiply by 2 = double the volume
* multiply by 0.5 = cutting the volume in half