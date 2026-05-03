
// UUH! IlMarcatoreTM 20:13
//
//

const ratchet = register('ratchet', (pat) => pat.sometimes(ply(2)))

samples({
  'myvox':[
    '0.wav', '1.wav', '2.wav', '3.wav', '4.wav', 
    '5.wav', '6.wav', '7.wav', '8.wav', '9.wav', '10.wav', '11.wav', '12.wav'
  ]
}, 'https://cdn.jsdelivr.net/gh/italianloverboy/uuh@main/samples/');

await initAudioOnFirstClick()

setcpm(126/4)

const bPatt = "<[g1[~ g2] ~ g1 [f1 d1] g1 ~ g1][g1[~ bb1] ~ g1[c2 d2] c2 ~ f1][g1[~ g2] ~ g1[f1 d1] g1 ~ g1] [g1[~ f1] ~ eb1[d1 c1] bb0 ~ d1]>";
const sPatt = "<[g3[~ c3] ~ e3[c3 d3] e3 ~ g3][bb2[~ f3] ~ d3[eb3 f3] d3 ~ f2][g3[~ c3] ~ e3[c3 d3] e3 ~ g3][eb3[~ bb2] ~ d3[c3 bb2] eb3 ~ d3]>";

const hookPatt = "<~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~ ~[~ c4 d4 bb3 ~ e3 g3 e3][~ bb3 c4 d4 f4 ~ d4 c4][g3 f3 g3 bb3 ~ c4 d4 bb3] ~>";
const chordRhythm = "<[~ x ~ ~] [~ ~ x ~][~ x ~ x][~ ~ ~ x]>";

 

 
const modWeak   = rand.range(0.01, 0.1);  
const modMid    = rand.range(0.1,  0.7);  
const modStrong = rand.range(0.7,  1.5);   
 
const modActive = "<0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1>";

 
const wMod = (base) => choose(modWeak, modMid, modStrong).segment(1).slow(10).mul(modActive).add(base);

// =====================================================================

stack(
  // 1. Kick:  
  
  s("[bd*4, ~[~ mt] ~ [~ lt]]").bank("RolandTR909").room(wMod(0.2))
    .gain("<0 0 0 0 1 1 1 1 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1>"), 
  
  // 2. HI-HATS:  
  
  s("[~ oh]*4").bank("RolandTR909").decay(wMod(0.1))
    .gain("<0 0 0.3 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0.6 0 0 0 0 0 0 0.3 0.6 0.6 0.6 0.4 0.6 0.4 0.6 0.4 0.6 0.4 0.6 0 0 0 0>"),

  // --- CLAP GESU
  stack(
    s("~ cp ~ cp").bank("RolandTR909").room(wMod(0.3)),  
    s("[~ rd]*4").bank("RolandTR909").decay(wMod(0.3))   
  ).gain("<0.25 0 0 0 0 0 0 0 0 0 0 0 0.25 0.35 0.25 0.35>"),
  
  // 3. BASS:
  note(bPatt).s("saw").lpq(45).decay(wMod(0.32)).sustain(2)  
    .gain("<0.7 0 0.7 0 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0 0.7 0 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0.7 0 0 0 0>"),
  
  // 4. SUB-SYNTH
    note(sPatt).s("saw").lpf(220).lpq(10).adsr(0.4, 1.3, wMod(0.8), 1.3) 
    .gain("<0.8 0 0 0.8 0 0 0 0.8 0.8 0.8 0.8 0.8 0.8 0.8 0.8 0>"),

  // 5. VOCE PRINCIPALE
  s("myvox").n(choose(0, 10, 11, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11,0, 10, 11, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12))
    .speed(choose(0.6, 0.55, 0.65, 0.5, 0.75, 0.8, 1.3, 1.4))
    .crush(choose(5.5, 9, 8, 6, 8, 9, 8, 5)) 
    .pan(rand).gain(0.4), 

  // 6. CONTROCORO
  s("myvox").n(choose(0, 10, 11, 10, 11, 10, 11, 12, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 10, 11, 10, 11, 10, 11, 1, 2, 3, 4, 5, 6, 7, 8, 9 ))
    .struct("~ ~ ~ x").speed(choose(0.55, 0.6, 0.65, 0.8, 1.3)) 
    .pan(sine.slow(4)) 
    .gain("<0.25 0 0 0 0.45 0.35 0.45 0.35 0.55 0.25 0.45 0.35 0.45 0.35 0.45 0>"),

 // 7. color
  note(`
    <
      [[~ g4 ~ d4] [~ f4 ~ c4][~ g4 ~ bb3] [~ c4 d4 ~]]!16 
      [[g3 bb3 d4 f4] [g4 d4 bb3 g3] [c4 ~ d4 ~][~ f4 d4 c4]]!16 
      [[~ d4 c4 ~][f4 ~ g4 ~] [~ bb4 a4 ~] [g4 f4 d4 ~]]!16 
      [[g4 [~ f4] d4 ~] [~ c4 [bb3 d4] ~] [g4 ~ f4 d4] [~ c4 ~ ~]]!16 
      [[~ g4 f4 d4] [~ ~ c4 d4] [f4 ~ d4 ~][~ bb3 c4 ~]]!16 
      [[g4 a4 bb4 ~][c5 ~ d5 ~] [c5 bb4 g4 f4] [d4 ~ ~ ~]]!16 
      [[g4 ~ g4 ~] [~ g4 d4 ~][g4 ~ f4 ~] [c4 d4 ~ ~]]!16 
      [[d4 d4 ~ f4] [d4 d4 ~ c4] [d4 d4 ~ g4][f4 d4 c4 ~]]!16 
      [[g4 ~ ~ ~][~ d4 ~ ~] [f4 ~ ~ ~] [~ c4 ~ ~]]!16 
      [[g4 ~ bb4 ~] [c5 ~ d5 ~] [f4 ~ d4 ~][~ c4 g3 ~]]!16 
      [[d5 ~ ~ ~][~ a4 ~ ~] [c5 ~ ~ ~] [~ g4 ~ ~]]!16 
      [[g3 g4 ~ f4] [~ d4 c4 bb3][g3 ~ d4 ~] [f4 g4 ~ ~]]!16
    >
  `)
 .s(choose("gm_sitar", "saw", "fm", "pulse", "gm_telephone", "casio", "clash2", "cowbell", "siren", "tubularbells2", "gm_pan_flute").segment(1).slow(16))
.gain("<0 0 0 0 0.20 0.20 0 0 0.20 0.20 0.20 0.20 0.20 0.20 0.25 0.25>"),
  
// 8. CHORD STAB (Complesso, varia ogni 16 cicli, strumento fisso ogni 8)
note(`
    <
    [[g3m9 ~ ~ g3m9] [~ bb3maj7 ~ ~] [c4maj7 ~ ~ f3m] [~ ~ d3m7 ~]]!16
    [[bb3maj7 ~ ~ ~] [~ c4m7 ~ ~] [eb4maj7 ~ ~ ~] [~ ~ ~ d4sus4]]!16
    [[f37 ~ ~ ~] [~ g3m9 ~ ~] [~ ~ c4m7 ~] [~ d37 ~ ~]]!16
    [[eb3maj7 ~ g3m7 ~] [~ f37 ~ ~] [bb3maj7 ~ ~ ~] [~ ~ ~ ~]]!16
    >
`)
.s(choose("saw", "square", "triangle", "juno", "fm", "pulse", "casio").segment(1).slow(8))
.lpf(sine.range(400, 2800).fast(0.5)).lpq(15)
.delay(wMod(0.6)).delayt(0.375).delayfb(wMod(0.4)).room(wMod(0.4))
.gain("<0 0 0 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35 0.35>"),

// 9. EARWORM (Potenziato: più range, effetti dinamici, strumento fisso ogni 8)
note(hookPatt)
  .s(choose("saw", "square", "fm", "juno", "supersaw", "triangle").segment(1).slow(8)) 
  .sometimes(x => x.ply("<2 4 3>")) // Variazione ritmica casuale
  .lpf(sine.range(100, 4500).slow(4)) // Range filtro molto più ampio
  .lpq(rand.range(10, 40)) // Risonanza variabile
  .crush(choose(0, 4, 8).segment(1).slow(4)) // Grana bitcrush che varia
  .pan(sine.slow(10)) // Movimento stereo lento
  .delay(0.4).delayt(0.65).delayfb(wMod(0.5))
  .room(0.4)
  .gain("<0 0 0.55 0 0 0.55 0 0 0.55 0 0 0 0.55 0.55 0.55 0>")

) 
