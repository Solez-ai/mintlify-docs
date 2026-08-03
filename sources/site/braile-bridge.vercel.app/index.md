# Source: https://braile-bridge.vercel.app/

Science & Technology Innovation Competition

# Every student writes. 
_Every teacher reads._

**BrailleBridge** is a low-cost, bilingual Braille writing system that lets blind and visually impaired students compose answers independently — on braille chords — while teachers see ordinary digital text appear instantly, in real time.

[Try the live demo](https://braile-bridge.vercel.app/#demo) [Explore the system](https://braile-bridge.vercel.app/#problem)

0/device

Total hardware cost

0

Languages — English & Bangla

0ms

Chord detection window

0

Braille dots, one device

ESP32 · Bluetooth HID

1 4 2 5 3 6

SPACE SHIFT

EN a

Two outputs · one chord ~$12 bill of materials

Scroll

BrailleBridge does not replace Braille. It protects Braille as the student’s natural writing method — while removing the barrier between braille input and teacher-readable output.

01 — The Problem

## Silent answers are _still_ answers

For many blind students, writing during class means dictating every word to a sighted writer — sacrificing privacy, independence, speed, and accuracy.

### Loss of privacy

Answers must be spoken aloud for a writer to record — nothing stays private.

### Loss of independence

Someone else controls the final written record — and how it is interpreted.

### Reduced speed control

The student waits, repeats, and slows to the writer’s pace — not their own.

### Transcription errors

Technical terms, names, and Bangla phonetics are easily misheard or misspelled.

### Teacher accessibility gap

Embossed braille is unreadable to most sighted teachers — so work goes unchecked.

### Financial barriers

Mechanical braillers cost hundreds to thousands of dollars and are hard to repair locally.

The old way

Student dictates → writer types → meaning is lost

With BrailleBridge

Student writes braille chords → digital text appears for the teacher

02 — The Solution

## One chord. _Two outputs._

A six-dot braille keyboard, an ESP32 translator, and a web-based teacher console — a complete, affordable classroom system.

### Blind student

Presses familiar braille chords on tactile mechanical keys

6 dots + Space + Shift

### ESP32 translator

Reads the chord within 180 ms, encodes a 6-bit value, looks up the dictionary

English · Bangla · Shift

Bluetooth HID

### Student’s own device

Works with any phone, tablet, or PC — no proprietary receiver needed.

USB Serial · 115200

### Teacher software

Readable digital output in real time — monitor, review, save, and assess.

1

**Press.** The student presses a braille chord on the six-dot keyboard.

2

**Translate.** The ESP32 collects keys within 180 ms, encodes the chord as a 6-bit value, and looks it up in the active dictionary.

3

**Send.** The character is transmitted over Bluetooth (student device) and USB Serial (teacher software) simultaneously.

4

**Read.** The teacher software displays, timestamps, saves, and organises every answer.

03 — Innovation & Novelty

## The first _combination_ of its kind

BrailleBridge fuses a physical braille keyboard, bilingual translation, Bluetooth output, and web-based teacher software — within a single affordable system. No existing product delivers this combination.

#### Six-dot tactile input

Physical Outemu switches with true mechanical feel — braille as it was meant to be written.

#### English & Bangla, same keys

One hardware device, two dictionaries — with Shift-based modifiers for vowel signs, numbers, punctuation, and math.

#### Bluetooth HID keyboard

Pairs with any modern device — no proprietary software or receiver required.

#### USB serial to teacher software

Feeds a professional HTML/CSS/JS console via the Web Serial API — no installs.

### ESP32 architecture

~$5 microcontroller, ~$0.38 switches, and a custom 3D-printed enclosure.

#### Designed for classrooms

Built for exams and tests: silent input, real-time monitoring, timestamped records.

Comparison with existing solutions
| Solution | Strength | Limitation |
| --- | --- | --- |
| Human writer | No equipment needed | Privacy loss, errors |
| Slate & stylus | Affordable, tactile | Slow; teacher cannot read |
| Perkins Brailler | Familiar method | Expensive, heavy, embossed only |
| Voice typing | Fast in quiet | Fails in noisy classrooms |
| Commercial digital | Advanced accessibility | Very expensive, hard to repair |
| Ordinary keyboard | Cheap, ubiquitous | No six-dot chording |
| **BrailleBridge** | **Bilingual + teacher software** | _Prototype stage_ |

04 — Specifications & Hardware

## Built from parts you can _replace_

Every component is standard, affordable, and locally replaceable — from the ESP32 to the mechanical switches to the 3D-printed enclosure.

Microcontroller | ESP32 Dev Module
Braille keys | 6 × Outemu Red mechanical switches
Additional keys | Space, Shift
Chording window | 180 ms
Output channels | Bluetooth HID + USB Serial
Languages | English, Bangla (Bharati Braille)
Shift functions | Caps, numbers, punctuation, math
LED indicators | Red (English) · Green (Bangla)
Enclosure | Fusion 360 CAD, FDM 3D-printed
Teacher software | HTML / CSS / JS (Web Serial API)

GPIO mapping
| Function | GPIO | Notes |
| --- | --- | --- |
| Dot 1 | 14 | Top-left |
| Dot 2 | 12 | Middle-left |
| Dot 3 | 13 | Bottom-left |
| Dot 4 | 27 | Top-right |
| Dot 5 | 26 | Middle-right |
| Dot 6 | 25 | Bottom-right |
| Space | 33 | Dedicated space key |
| Shift | 32 | Modifier |
| Red LED | 18 | English indicator |
| Green LED | 19 | Bangla indicator |

**LED language cues:** Red ON / Green OFF = English · Green ON / Red OFF = Bangla · Both ON for 100 ms = valid character.

05 — Firmware Architecture

## Chord → character, _reliably_

Written in C++ on the Arduino framework, the firmware turns six mechanical keys into a compact 6-bit value — then looks it up in a structured dictionary.

chord-read.ino C++

```
uint8_t readChord() {
  uint8_t chord = 0;
  if (digitalRead(PIN_DOT1) == LOW) chord |= (1 << 0);
  if (digitalRead(PIN_DOT2) == LOW) chord |= (1 << 1);
  if (digitalRead(PIN_DOT3) == LOW) chord |= (1 << 2);
  if (digitalRead(PIN_DOT4) == LOW) chord |= (1 << 3);
  if (digitalRead(PIN_DOT5) == LOW) chord |= (1 << 4);
  if (digitalRead(PIN_DOT6) == LOW) chord |= (1 << 5);
  return chord;
}
```

1

#### Bitmask encoding

Each dot owns one bit in an 8-bit integer. A chord becomes a single 6-bit value — compact, constant-time comparison.

2

#### 180 ms chording window

Fingers rarely land together. The firmware ORs every key pressed within 180 ms into one chord — so one intent is never read as many.

3

#### Dictionary-driven

Translations live in a `BrailleEntry` array. Adding a language? Append entries — no control-flow changes.

4

#### Software-defined language

Hold dots 4+5+6 for 500 ms to toggle English ↔ Bangla. No hardware switch — just a boolean.

5

#### Modifier architecture

Shift expands the range — uppercase, Bangla vowel signs, numbers 1–0, punctuation, and math symbols — without adding keys.

Chord pressed GPIO read 180 ms window 6-bit mask Dictionary lookup Output character

06 — Try It Yourself

## Compose a chord _right here_

Click the six dots like the physical keys, then read the translation. This is the exact chord-to-character pipeline running in your browser.

1 4 2 5 3 6

English বাংলা

Shift Space Clear

⠀

Translation —

Chord value 0b000000

Active dots None

Mode English

Output appears here…

**Pro tip:** toggle **Shift** and press dot 1 (A) to type the digit **1** — Shift + A–J gives 1–0. In বাংলা mode, Shift turns vowels into vowel signs (কার) — just like the real firmware.

07 — The Dictionaries

## English, Bangla — _same six dots_

Because Bharati Braille maps cells to _sounds_, most Bangla chords coincide with their English counterparts. Only the active dictionary changes.

English A–Z Bangla (Bharati) Numbers & Symbols

First decade · A–J
| Letter | Chord | Dots |
| --- | --- | --- |
| A | 0b000001 | 1 |
| B | 0b000011 | 1,2 |
| C | 0b001001 | 1,4 |
| D | 0b011001 | 1,4,5 |
| E | 0b010001 | 1,5 |
| F | 0b001011 | 1,2,4 |
| G | 0b011011 | 1,2,4,5 |
| H | 0b010011 | 1,2,5 |
| I | 0b001010 | 2,4 |
| J | 0b011010 | 2,4,5 |

Second decade · K–T (first + dot 3)
| Letter | Chord | Dots |
| --- | --- | --- |
| K | 0b000101 | 1,3 |
| L | 0b000111 | 1,2,3 |
| M | 0b001101 | 1,3,4 |
| N | 0b011101 | 1,3,4,5 |
| O | 0b010101 | 1,3,5 |
| P | 0b001111 | 1,2,3,4 |
| Q | 0b011111 | 1–5 |
| R | 0b010111 | 1,2,3,5 |
| S | 0b001110 | 2,3,4 |
| T | 0b011110 | 2,3,4,5 |

Third decade · U–Z (first + dots 3 & 6)
| Letter | Chord | Dots |
| --- | --- | --- |
| U | 0b100101 | 1,3,6 |
| V | 0b100111 | 1,2,3,6 |
| W | 0b111010 | 2,4,5,6 · exception |
| X | 0b101101 | 1,3,4,6 |
| Y | 0b111101 | 1,3,4,5,6 |
| Z | 0b101001 | 1,3,5,6 |

Vowels · স্বরবর্ণ
| Vowel | English | Dots | Binary |
| --- | --- | --- | --- |
| অ | O | 1 | 0b000001 |
| আ | — | 3,4,5 | 0b011100 |
| ই | I | 2,4 | 0b001010 |
| ঈ | — | 3,5 | 0b010100 |
| উ | U | 1,3,6 | 0b100101 |
| ঊ | — | 1,2,5,6 | 0b110011 |
| এ | E | 1,5 | 0b010001 |
| ঐ | — | 3,4 | 0b001100 |
| ও | O | 1,3,5 | 0b010101 |
| ঔ | — | 2,4,6 | 0b101010 |

ক-বর্গ · Velar
| Letter | Dots | Binary |
| --- | --- | --- |
| ক | 1,3 | 0b000101 |
| খ | 4,6 | 0b101000 |
| গ | 1,2,4,5 | 0b011011 |
| ঘ | 1,2,6 | 0b100011 |
| ঙ | 3,4,6 | 0b101100 |

চ-বর্গ · Palatal
| Letter | Dots | Binary |
| --- | --- | --- |
| চ | 1,4 | 0b001001 |
| ছ | 1,6 | 0b100001 |
| জ | 2,4,5 | 0b011010 |
| ঝ | 3,5,6 | 0b110100 |
| ঞ | 2,5 | 0b010010 |

ট-বর্গ · Retroflex
| Letter | Dots | Binary |
| --- | --- | --- |
| ট | 2,3,4,5,6 | 0b111110 |
| ঠ | 2,4,5,6 | 0b110110 |
| ড | 1,2,4,6 | 0b101011 |
| ঢ | All 6 | 0b111111 |
| ণ | 3,4,5,6 | 0b111100 |

ত-বর্গ · Dental
| Letter | Dots | Binary |
| --- | --- | --- |
| ত | 2,3,4,5 | 0b011110 |
| থ | 1,4,5,6 | 0b111001 |
| দ | 1,4,5 | 0b011001 |
| ধ | 2,3,4,6 | 0b101110 |
| ন | 1,3,4,5 | 0b011101 |

প-বর্গ · Labial
| Letter | Dots | Binary |
| --- | --- | --- |
| প | 1,2,3,4 | 0b001111 |
| ফ | 2,3,5 | 0b010110 |
| ব | 1,2 | 0b000011 |
| ভ | 4,5 | 0b011000 |
| ম | 1,3,4 | 0b001101 |

Semi-vowels & others
| Letter | Dots | Binary |
| --- | --- | --- |
| য | 1,3,4,5,6 | 0b111101 |
| র | 1,2,3,5 | 0b010111 |
| ল | 1,2,3 | 0b000111 |
| শ | 1,4,6 | 0b101001 |
| ষ | 1,2,3,4,6 | 0b101111 |
| স | 2,3,4 | 0b001110 |
| হ | 1,2,5 | 0b010011 |

Special marks
| Mark | Name | Dots | Binary |
| --- | --- | --- | --- |
| ্ | Halant (virama) | 4 | 0b001000 |
| ং | Anusvara | 5,6 | 0b110000 |
| ঃ | Visarga | 6 | 0b100000 |
| ঁ | Candrabindu | 5 | 0b010000 |

Numbers · Shift + A–J
| Shift + | Number | Dots |
| --- | --- | --- |
| A | 1 | 1 |
| B | 2 | 1,2 |
| C | 3 | 1,4 |
| D | 4 | 1,4,5 |
| E | 5 | 1,5 |
| F | 6 | 1,2,4 |
| G | 7 | 1,2,4,5 |
| H | 8 | 1,2,5 |
| I | 9 | 2,4 |
| J | 0 | 2,4,5 |

Punctuation · Shift + chord
| Symbol | Dots |
| --- | --- |
| , | 2 |
| ; | 2,3 |
| : | 2,5 |
| . | 2,5,6 |
| ? | 2,3,6 |
| ! | 2,3,5 |

Mathematics · Shift + chord
| Symbol | Dots |
| --- | --- |
| + | 2,3,5 |
| − | 3,6 |
| × | 2,3,6 |
| ÷ | 3,4 |
| \= | 2,3,5,6 |

08 — Teacher Software

## A console any teacher can _read_

Built with HTML, CSS, and JavaScript using the Web Serial API. No Python, no Qt, no database — just a browser. Runs in Chrome and Edge.

- Multi-student roster with COM ports & baud rates
- Live serial reading — a dedicated reader loop per student
- Automatic language detection from `LANG:en` / `LANG:bn`
- Real-time serif display with auto-scroll & blinking cursor
- Chord visualization — 6 mini-dots animate per input
- Exam mode: every student side-by-side on one screen
- Timestamped export — `[HH:MM:SS]` per character
- Light/dark theme, persistent roster via localStorage

Teacher Console — live

SA

**Sarah Ahmed** USB · 115200 · EN

The mitochondria is the powerhouse of the cell.

RK

**Rahim Karim** Bluetooth · BN

আমার সোনার বাংলা, আমি তোমায় ভালোবাসি।

MI

**Maya Islam** USB · 115200 · EN

x² + y² = r² · 180 ms per chord ✓

3 students connected [Read further docs](https://braille.mintlify.site)

serial-connect.js JavaScript

```
async function connectStudentSerial(student) {
  const port = await navigator.serial.requestPort();
  await port.open({ baudRate: student.baud });
  student.serialPort = port; student.connected = true;

  const decoder = new TextDecoderStream();
  port.readable.pipeTo(decoder.writable);
  const reader = decoder.readable.getReader();

  async function readLoop() {
    while (student.readLoopActive) {
      const { value, done } = await reader.read();
      if (done) break;
      for (const ch of value) handleStudentChar(student, ch);
    }
  }
  readLoop();
}
```

09 — Engineering Innovations

## Small details, _big difference_

01

#### Chord buffering

A 180 ms window absorbs fingers landing milliseconds apart.

02

#### Bitmask encoding

Six keys collapse into one 6-bit integer — compact, constant-time comparison.

03

#### Dictionary-driven translation

A new language is just new array entries — no control-flow changes.

04

#### Dual communication

Bluetooth for the student, USB Serial for the teacher — simultaneous and independent.

05

#### Software-defined language

A boolean variable switches English and Bangla — no hardware switch.

06

#### Modifier architecture

Shift expands caps, numbers, and symbols without adding physical keys.

07

#### Modular construction

Switches, ESP32, enclosure, and software repair or upgrade independently.

08

#### Web-based teacher software

Nothing to install — it runs in the browser and updates on refresh.

09

#### Failsafe refresh

A 500 ms interval rebuilds every feed from stored data — no character is ever lost.

10 — Affordability

## Under _$15_, fully built

A fraction of mechanical braillers or commercial digital systems — with every component replaceable for a few dollars.

Bill of materials · per device
| Component | Qty | Cost (USD) |
| --- | --- | --- |
| ESP32 Dev Module | 1 | $5.00 |
| Outemu Red switch | 8 | $3.00 |
| Keycaps (3D-printed) | 8 | $1.00 |
| LED (3mm) + resistor | 2+2 | $0.30 |
| Perfboard | 1 | $0.80 |
| PLA filament (enclosure) | 30 g | $1.20 |
| M3 screws, wire, solder | — | $0.70 |
| **Total** | | **~$12.00** |

0

a complete device, including tools & overhead

Perkins Brailler**$600 – $1,000+**

Commercial digital**$1,000 – $5,000+**

BrailleBridge**<$15**

11 — Conclusion

## When a blind student writes her own exam answer — without a writer, without speaking aloud, and without the teacher needing braille — _the project has succeeded._

BrailleBridge bridges four gaps: between tactile writing and readable text, between English and Bangla, between independent composition and teacher assessment, and between the high cost of assistive technology and limited school budgets.

[Write your first chord](https://braile-bridge.vercel.app/#demo)