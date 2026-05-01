https://tinyurl.com/ilmarcatoreTM

// Prebake script
//
// This is code that is loaded before your pattern is run.
// You can use it to define custom functions to use in any pattern.
// 
// This is an initial example script. You can edit it to add 
// your own funtions.
//
// To use a script shared by some other user you can use
// the import-button or paste the script in this editor.

const ratchet = register('ratchet', (pat) => pat.sometimes(ply(2)))

samples({
  'myvox':[
    '0.wav', '1.wav', '2.wav', '3.wav', '4.wav', 
    '5.wav', '6.wav', '7.wav', '8.wav', '9.wav'
  ]
}, 'https://cdn.jsdelivr.net/gh/italianloverboy/uuh@main/samples/');

await initAudioOnFirstClick()

setcpm(126/4)

const bPatt = "<[g1[~ g2] ~ g1 [f1 d1] g1 ~ g1][g1[~ bb1] ~ g1[c2 d2] c2 ~ f1] [g1[~ g2] ~ g1[f1 d1] g1 ~ g1] [g1[~ f1] ~ eb1[d1 c1] bb0 ~ d1]>";
const sPatt = "<[g3[~ c3] ~ e3[c3 d3] e3 ~ g3][bb2[~ f3] ~ d3[eb3 f3] d3 ~ f2][g3[~ c3] ~ e3[c3 d3] e3 ~ g3][eb3[~ bb2] ~ d3[c3 bb2] eb3 ~ d3]>";

 
const hookPatt = "<~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~[~ c4 d4 bb3 ~ e3 g3 e3][~ bb3 c4 d4 f4 ~ d4 c4][g3 f3 g3 bb3 ~ c4 d4 bb3] ~>";
const chordRhythm = "<[~ x ~ ~] [~ ~ x ~][~ x ~ x][~ ~ ~ x]>";

stack(
  // 1. Kick:  
  s("[bd*4, ~[~ mt] ~ [~ lt]]").bank("RolandTR909").room(0.2)
    .gain("<0 0 1 1 1 1 1 0 1 1 1 1 1 1 1 0>"), 
  
  // 2. HI-HATS:  
  s("[~ oh]*4").bank("RolandTR909").decay(0.1)
    .gain("<0 0 0 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0>"),

  // --- CLAP  
  stack(
    s("~ cp ~ cp").bank("RolandTR909").room(0.3),
    s("[~ rd]*4").bank("RolandTR909").decay(0.3)
  ).gain("<0 0 0 0 0 0 0 0 0 0 0 0 0.25 0.35 0.25 0>"),
  
  // 3. BASS:
  note(bPatt).s("saw").lpq(45).decay(0.22).sustain(2)
    .gain("<0 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0>"),
  
  // 4. SUB-SYNTH
  note(sPatt).s("saw").lpf(220).lpq(10).adsr(0.4, 1.3, 0.8, 1.3)
    .gain("<0 0 0.8 0.8 0.8 0.8 0.8 0.8 0.8 0.8 0.8 0.8 0.8 0.8 0.8 0>"),

  // 5. VOCE PRINCIPALE
  s("myvox").n(choose(0, 0, 1, 2, 2, 3, 4, 4, 5, 6, 6, 7, 8, 8, 9))
    .speed(choose(0.6, 0.5, 0.6, 0.5, 0.6, 0.8, 1.3, 1.6))
    .crush(choose(5.2,9,8,4.5,8,9,8)) 
    .pan(rand).gain(0.4), 

  // 6. CONTROCORO
  s("myvox").n(choose(1, 1, 2, 3, 3, 4, 5, 5, 6, 7, 7, 9, 9))
    .struct("~ ~ ~ x").speed(choose(0.5, 0.6, 0.6, 0.6, 1.3)) 
    .pan(sine.slow(4)) 
    .gain("<0 0 0 0 0.35 0.25 0.35 0.25 0.35 0.25 0.35 0.25 0.35 0.25 0.35 0>"),

  // 7. BRASS - 
  note("<~ ~ d4> <~ f4 ~> <~ d4 ~> <~ g4 ~ c4>") 
    .s(choose("superbrass", "saw", "fm", "pulse").slow(16))  
    .gain("<0 0 0 0 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0>"),

  // 8.   CHORD STAB -  
  note("g4m9").struct(chordRhythm)
    .s(choose("square", "saw", "triangle").slow(16)) 
    .lpf(sine.range(400, 2800).fast(0.5)).lpq(15) 
    .delay(0.6).delayt(0.375).delayfb(0.4).room(0.4) 
    .gain("<0 0 0 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0>"),

  // 9. EARWORM  
  note(hookPatt)
    .s(choose("saw", "square", "fm", "juno").slow(16)) 
    .lpf(sine.range(200, 1800).slow(4)) 
    .delay(0.4).delayt(0.65).delayfb(0.5) 
    .gain("<0 0 0 0 0 0 0 0 0 0 0 0 0.55 0.55 0.55 0>") 
)
