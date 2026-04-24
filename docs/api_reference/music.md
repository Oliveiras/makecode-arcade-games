# MakeCode Arcade / API Reference Guide / Game

MakeCode Arcade is a platform to build Retro Arcade games for the browser and handheld consoles.

This document contains the API reference for the **Music** module.

This module can play songs, melodies, sounds, and tones.

<style>
    h1,h2,h3 {
        margin-top: 40px !important;
    }
    h3 {
        color: purple;
    }
</style>


## Play music

Use the play block to play a song you compose, a short melody or a built-in sound.

### play

Play a song, melody, tone, or a sound effect from a playable music source.

```js
function music.play(toPlay: music.Playable, playbackMode: music.PlaybackMode): void;
```

Music is played for a simple tone, a melody, or a song. Each of these music sources is called a playble object. The play block can take any of these playable objects and play them as sound output for your game.

The simpliest music source is a tone, one note play for a duration of time.

Then, there is the melody which is a series of notes played at a certain speed, or tempo. You can create your own melody of choose a built-in one to play.

The most complex playabe object is a song. Songs are composed in the Song Editor using many notes from different instruments.

**_Parameters_**

- **toPlay:** the playable object, or music source, to play.
- **playbackMode:** the playback mode for continuing the program:
  - **play until done:** play the music source in toPlay but wait to run the next part of the program until music play is done.
  - **in background:** play the music source in toPlay but continue with the rest of the program before music play is done.
  - **in background looping:** play the music source in toPlay but continue with the rest of the program before music play is done. The music will remain playing, returning to the first note of the music after its duration.

> ℹ️ Stop the music!
>
> You can stop any music currently playing with the stop all sounds block. This is useful if playbackMode is set to in background looping and you wish to stop the music for a scene change or respond to an event with a different sound.

**_Examples_**

**1. Play a melody**

Play a short melody created in the Melody Editor.


```js
music.play(music.stringPlayable("D F E A E A C B ", 120), music.PlaybackMode.UntilDone)
```

**2. Different music sources, one block to play them all**

Put 4 different playable music sources in an array. Play one after the other.

```js
let playables = [
music.tonePlayable(262, music.beat(BeatFraction.Whole)),
music.stringPlayable("D F E A E A C B ", 120),
music.melodyPlayable(music.baDing),
music.createSong(hex`0078000408020200001c00010a006400f40164000004000000000000000000000000000500000430000400080001220c001000012514001800011e1c00200001222400280001252c003000012934003800012c3c004000011e03001c0001dc00690000045e010004000000000000000000000564000104000330000400080001290c001000011e1400180001251c002000012924002800011b2c003000012234003800011e3c0040000129`)
]
for (let someMusic of playables) {
    music.play(someMusic, music.PlaybackMode.UntilDone)
    pause(500)
}
```

**3. Looping music play**

Play a simple song in the background while a monkey moves around the screen. When the monkey hits the bubble in the middle of the screen, stop the song and play a bursting sound.

```js
let bubble = sprites.create(img`
    . . . . . b b b b b b . . . . . 
    . . . b b 9 9 9 9 9 9 b b . . . 
    . . b b 9 9 9 9 9 9 9 9 b b . . 
    . b b 9 d 9 9 9 9 9 9 9 9 b b . 
    . b 9 d 9 9 9 9 9 1 1 1 9 9 b . 
    b 9 d d 9 9 9 9 9 1 1 1 9 9 9 b 
    b 9 d 9 9 9 9 9 9 1 1 1 9 9 9 b 
    b 9 3 9 9 9 9 9 9 9 9 9 1 9 9 b 
    b 5 3 d 9 9 9 9 9 9 9 9 9 9 9 b 
    b 5 3 3 9 9 9 9 9 9 9 9 9 d 9 b 
    b 5 d 3 3 9 9 9 9 9 9 9 d d 9 b 
    . b 5 3 3 3 d 9 9 9 9 d d 5 b . 
    . b d 5 3 3 3 3 3 3 3 d 5 b b . 
    . . b d 5 d 3 3 3 3 5 5 b b . . 
    . . . b b 5 5 5 5 5 5 b b . . . 
    . . . . . b b b b b b . . . . . 
    `, SpriteKind.Player)
let monkey = sprites.create(img`
    . . . . f f f f f . . . . . . . 
    . . . f e e e e e f . . . . . . 
    . . f d d d d e e e f . . . . . 
    . c d f d d f d e e f f . . . . 
    . c d f d d f d e e d d f . . . 
    c d e e d d d d e e b d c . . . 
    c d d d d c d d e e b d c . f f 
    c c c c c d d d e e f c . f e f 
    . f d d d d d e e f f . . f e f 
    . . f f f f f e e e e f . f e f 
    . . . . f e e e e e e e f f e f 
    . . . f e f f e f e e e e f f . 
    . . . f e f f e f e e e e f . . 
    . . . f d b f d b f f e f . . . 
    . . . f d d c d d b b d f . . . 
    . . . . f f f f f f f f f . . . 
    `, SpriteKind.Enemy)
monkey.setBounceOnWall(true)
monkey.x = scene.screenWidth()
monkey.setVelocity(50, 40)
music.play(music.stringPlayable("C5 A B G A F A C5 ", 120), music.PlaybackMode.LoopingInBackground)
sprites.onOverlap(SpriteKind.Player, SpriteKind.Enemy, function (sprite, otherSprite) {
    music.stopAllSounds()
    sprites.destroy(sprite, effects.blizzard, 500)
    music.play(music.melodyPlayable(music.zapped), music.PlaybackMode.UntilDone)
})
```

**4. Play a sound effect**

Play a sine wave sound effect for 5 seconds.

```js
music.play(music.createSoundEffect(WaveShape.Sine, 5000, 0, 255, 0, 500, SoundExpressionEffect.None, InterpolationCurve.Linear), music.PlaybackMode.UntilDone)
```


## Sound effects

### createSoundEffect

Create a sound expression string for a sound effect.

```js
function music.createSoundEffect(waveShape: WaveShape, startFrequency: number, endFrequency: number, startVolume: number, endVolume: number, duration: number, effect: SoundExpressionEffect, interpolation: InterpolationCurve): music.SoundEffect;
```

A sound expression is set of parameters that describe a sound effect that will last for some amount of time. These parameters specify a base waveform, frequency range, sound volume, and effects. Sound data is created as a sound effect object and can then be played to the speaker, headphones, or at an output pin.

**_Parameters_**

- **waveShape:** the primary shape of the waveform:
  - **sine:** sine wave shape
  - **sawtooth:** sawtooth wave shape
  - **triangle:** triangle wave shape
  - **square:** square wave shape
  - **noise:** random noise generated wave shape
- **startFrequency:** a number that is the frequency of the waveform when the sound expression starts.
- **endFrequency:** a number that is the frequency of the waveform when the sound expression stops.
- **startVolume:** a number the initial volume of the sound expression.
- **endVolume:** a number the ending volume of the sound expression.
- **duration:** a number the duration in milliseconds of the sound expression.
- **effect:** an effect to add to the waveform. These are:
  - **tremolo:** add slight changes in volume of the sound expression.
  - **vibrato:** add slight changes in frequency to the sound expression.
  - **warble:** similar to vibrato but with faster variations in the frequency changes.
- **interpolation:** controls the rate of frequency change in the sound expression.
  - **linear:** the change in frequency is constant for the duration of the sound.
  - **curve:** the change in frequency is faster at the beginning of the sound and slows toward the end.
  - **logarithmic:** the change in frequency is rapid during the very first part of the sound.

**_Returns_**

a soundEffect object with the the desired sound effect parameters.

**_Examples_**

**1. Sine wave sound**

Create a sound expression string and assign it to a variable. Play the sound for the sound expression.

```js
let mySound = music.createSoundEffect(WaveShape.Sine, 2000, 0, 1023, 0, 500, SoundExpressionEffect.None, InterpolationCurve.Linear)
music.playSoundEffect(mySound, SoundExpressionPlayMode.UntilDone)
```

**2. Complex waveform sound**

Create a triangle wave sound expression with vibrato and a curve interpolation. Play the sound until it finishes.

```js
let mySound = music.createSoundEffect(
    WaveShape.Triangle,
    1000,
    2700,
    255,
    255,
    500,
    SoundExpressionEffect.Vibrato,
    InterpolationCurve.Curve
    )
music.playSoundEffect(mySound, SoundExpressionPlayMode.UntilDone)
```

### randomizeSound

Make a new sound similar to the original one but with some variations.

```js
function music.randomizeSound(sound: music.SoundEffect): music.SoundEffect;
```

The resulting sound effect will randomize some of the parameters of the original sound effect to create differences from the original sound.

**_Parameters_**

- **sound:** the original sound-effect.

**_Returns_**

a new sound-effect with some differences from the oringal sound.

**_Examples_**

**1. Play random sound effect.**

Randomize and play a sine wave sound effect.

```js
music.play(music.randomizeSound(music.createSoundEffect(WaveShape.Sine, 5000, 0, 255, 0, 500, SoundExpressionEffect.None, InterpolationCurve.Linear)), music.PlaybackMode.UntilDone)
```


## Sound

### ringTone

Play a musical tone on the speaker. The tone has a pitch (frequency) as high or low as you say. The tone will keep playing until tell it to stop.

```js
function music.ringTone(frequency: number): void;
```

> ℹ️ Simulator
>
> ring tone works on the Arcade. It might not work in the simulator on every browser.

The tone will keep playing until you stop it with stop all sounds.

**_Parameters_**

- **frequency:** is a number that says how high-pitched or low-pitched the tone is. This number is in Hz (Hertz), which is a measurement of frequency (pitch).

**_Example_**

**1. Play tone and change frequency.**

Play a tone with a base frequency of 440 but change it by 20 Hertz steps both up and down.

```js
let offset = 0
offset = -100
while (true) {
    while (offset < 100) {
        offset += 20
        music.ringTone(440 + offset)
        pause(300)
    }
    while (offset > -100) {
        offset += -20
        music.ringTone(440 + offset)
        pause(300)
    }
}
```

### rest

Give the speaker a period of time to not play any sound.

```js
function music.rest(ms: number): void;
```

> ℹ️ Simulator
>
> rest works on the Arcade. It might not work in the simulator on every browser.

**_Parameters_**

- **ms:** is a number saying how many milliseconds the Arcade should rest. One second is 1000 milliseconds.

**_Examples_**

**1. Play tone and rest.**

Play a ‘C’ note for one second and then rest for one second. Do it again, and again…

```js
let frequency = music.noteFrequency(Note.C)
while (true) {
    music.playTone(frequency, 1000)
    music.rest(1000)
}
```

### noteFrequency

Get the frequency of a musical note.

```js
function music.noteFrequency(name: Note): number;
```

**_Parameters_**

- **name:** is the name of the Note you want a frequency value for.

**_Returns_**

a number that is the frequency (in Hertz) of a note you chose.

**_Examples_**

**1. Play note and rest.**

Play a ‘C’ note for one second, rest for one second, and then play an ‘A’ note for one second.

```js
music.playTone(music.noteFrequency(Note.C), 1000)
music.rest(1000)
music.playTone(music.noteFrequency(Note.A), 1000)
```

### beat

Get the length of time for a musical beat.

```js
function music.beat(fraction: BeatFraction): number;
```

**_Parameters_**

- **fraction:** means fraction of a beat (BeatFraction.Whole, BeatFraction.Sixteenth, etc.)

**_Returns_**

a number that is the amount of time in milliseconds (one-thousandth of a second) for the beat fraction.

**_Examples_**

**1. Play tone for a quarter of a beat**

```js
music.playTone(Note.C, music.beat(BeatFraction.Quarter))
```

### tempo

Find the tempo (speed) of the music that is playing right now.

```js
function music.tempo(): number;
```

**_Returns_**

a number that means the count of beats per minute (number of beats in a minute of the music that the Arcade is playing right now).

### changeTempoBy

Makes the tempo (speed of a piece of music) faster or slower by the amount you say.

```js
function music.changeTempoBy(bpm: number): void;
```

> ℹ️ Simulator
>
> This function only works on the Arcade and in some browsers.

**_Parameters_**

- **bpm:** is a number that says how much to change the bpm (beats per minute, or number of beats in a minute of the music that the Arcade is playing).

### setTempo

Make the tempo (speed) of the music playing go faster or slower.

```js
function music.setTempo(bpm: number): void;
```

> ℹ️ Simulator
>
> set tempo works on the Arcade. It might not work in the simulator on every browser.

**_Parameters_**

- **bpm:** is a number that means the amount beats per minute you want. This is how fast you want Arcade to play music.

### setVolume

Set the volume for the sound synthesizer.

The synthesizer volume controls the level of the sound playing on current sound output of the board (speaker or pin).

```js
function music.setVolume(volume: number): void;
```

> ℹ️ Simulator
>
> set volume works on the Arcade. It might not work in the simulator on every browser.

**_Parameters_**

- **volume:** the volume of of the sounds played to the sound output. The volume number can be between 0 for silent and 255 for the loudest sound.

### stopAllSounds

Stop all the sounds that are playing right now and any others waiting to play.

```js
function music.stopAllSounds(): void;
```

If you play sounds or sound effects more than once, the sounds you asked to play later have to wait until the sounds played earlier finish. You can stop the sound that is playing now and all the sounds waiting to play with stop all sounds.

> ℹ️ Simulator
>
> stop all sounds works on the Arcade. It might not work in the simulator on every browser.
