---
title: "Breaking Codes"
description: "Cryptanalysis techniques and strategies for code-breaking"
---

## 🔨 The Art of Cryptanalysis

<div class="classified-stamp">FOR YOUR EYES ONLY</div>

**Cryptanalysis** is the science of analyzing and breaking cryptographic systems. While encryption is about hiding information, cryptanalysis is about revealing it. This guide will teach you the fundamental techniques used by codebreakers from ancient times to Bletchley Park.

<div class="typewriter">
"The difference between a puzzle and a cipher is that one is meant to be solved, the other is not. Your job is to solve them anyway."
</div>

<div class="morse-divider">-.. --. </div>

## 🎯 General Approach to Breaking Ciphers

### The Codebreaker's Scientific Method

1. **Observe** - Examine the ciphertext carefully
2. **Analyze** - Gather statistics and patterns
3. **Hypothesize** - Form theories about the cipher type
4. **Test** - Try your theories with code
5. **Iterate** - Refine based on results
6. **Verify** - Confirm your solution makes sense

### First Questions to Ask

When you encounter an encrypted message:

```
□ What is the length of the ciphertext?
□ What characters are present? (letters only? numbers? symbols?)
□ Are there patterns or repeated sequences?
□ Does the structure suggest anything? (grouping, spacing)
□ What's the distribution of characters?
□ Does frequency analysis reveal anything?
```

<div class="morse-divider">-.. --. </div>

## 📊 Fundamental Technique: Frequency Analysis

### Why It Works

Natural languages have characteristic letter distributions. No matter how complex the cipher, these patterns often leak through.

### English Letter Frequencies

**Most Common:**
```
E - 12.7%    T - 9.1%     A - 8.2%
O - 7.5%     I - 7.0%     N - 6.7%
```

**Memory Aid:** "ETAOIN SHRDLU" (descending order)

**Least Common:**
```
Z - 0.07%    Q - 0.10%    X - 0.15%
J - 0.15%    K - 0.77%
```

### Implementation

```python
def frequency_analysis(text):
    """Analyze letter frequency in text"""
    from collections import Counter
    
    # Extract only letters
    letters = [c.upper() for c in text if c.isalpha()]
    total = len(letters)
    
    if total == 0:
        return {}
    
    # Count frequencies
    counter = Counter(letters)
    
    # Convert to percentages
    frequencies = {
        letter: (count / total) * 100 
        for letter, count in counter.items()
    }
    
    return dict(sorted(frequencies.items(), 
                      key=lambda x: x[1], 
                      reverse=True))

def visualize_frequency(frequencies):
    """Display frequency analysis visually"""
    print("Letter Frequency Analysis:")
    print("-" * 50)
    for letter, freq in list(frequencies.items())[:10]:
        bar = "█" * int(freq * 2)
        print(f"{letter}: {freq:5.2f}% {bar}")
```

### Common Digraphs & Trigraphs

**Digraphs (2-letter combinations):**
- Most common: TH, HE, IN, ER, AN, RE, ED

**Trigraphs (3-letter combinations):**
- Most common: THE, AND, THA, ENT, ION, FOR

**Doubled Letters:**
- Common: LL, EE, SS, OO, TT, FF, RR, NN, PP, CC

<div class="alert-box">
💡 INSIGHT: In substitution ciphers, if you see a doubled letter in ciphertext, it likely represents a commonly doubled letter in plaintext (LL, EE, SS, etc.)
</div>

<div class="morse-divider">-.. --. </div>

## 🔢 Statistical Measures

### Index of Coincidence (IC)

Measures how likely two random letters from text are the same.

**Formula:** 
$$IC = \frac{\sum_{i=1}^{26} f_i(f_i - 1)}{N(N-1)}$$

Where $f_i$ is frequency of letter $i$, and $N$ is total letters.

**Interpretation:**
- English text: IC ≈ 0.065
- Random text: IC ≈ 0.038
- Monoalphabetic cipher: IC ≈ 0.065 (preserves distribution)
- Polyalphabetic cipher: IC ≈ 0.038-0.065 (flattens distribution)

```python
def index_of_coincidence(text):
    """Calculate Index of Coincidence"""
    from collections import Counter
    
    letters = [c.upper() for c in text if c.isalpha()]
    n = len(letters)
    
    if n <= 1:
        return 0.0
    
    freq = Counter(letters)
    
    # Sum of f_i * (f_i - 1)
    numerator = sum(f * (f - 1) for f in freq.values())
    denominator = n * (n - 1)
    
    return numerator / denominator

# Usage
ic = index_of_coincidence(ciphertext)
print(f"IC: {ic:.4f}")

if ic > 0.060:
    print("→ Likely monoalphabetic (substitution)")
elif ic < 0.045:
    print("→ Likely polyalphabetic (Vigenère)")
else:
    print("→ Mixed or short text")
```

### Chi-Squared Test

Measures how well observed frequencies match expected frequencies.

```python
def chi_squared_test(observed_freq, expected_freq):
    """
    Compare observed vs expected frequencies
    Lower value = better match
    """
    chi_squared = 0
    
    for letter in 'ABCDEFGHIJKLMNOPQRSTUVWXYZ':
        observed = observed_freq.get(letter, 0)
        expected = expected_freq.get(letter, 0)
        
        if expected > 0:
            chi_squared += ((observed - expected) ** 2) / expected
    
    return chi_squared

# English letter percentages
ENGLISH_FREQ = {
    'A': 8.2, 'B': 1.5, 'C': 2.8, 'D': 4.3, 'E': 12.7,
    'F': 2.2, 'G': 2.0, 'H': 6.1, 'I': 7.0, 'J': 0.15,
    'K': 0.77, 'L': 4.0, 'M': 2.4, 'N': 6.7, 'O': 7.5,
    'P': 1.9, 'Q': 0.10, 'R': 6.0, 'S': 6.3, 'T': 9.1,
    'U': 2.8, 'V': 0.98, 'W': 2.4, 'X': 0.15, 'Y': 2.0,
    'Z': 0.07
}
```

<div class="morse-divider">-.. --. </div>

## 🎨 Pattern Recognition Techniques

### Word Patterns

Identify structural patterns in words:

```python
def get_word_pattern(word):
    """
    Convert word to pattern based on letter repetition
    Example: HELLO → 0.1.2.2.3
    """
    word = word.upper()
    pattern = []
    mapping = {}
    next_num = 0
    
    for char in word:
        if char not in mapping:
            mapping[char] = next_num
            next_num += 1
        pattern.append(str(mapping[char]))
    
    return '.'.join(pattern)

# Examples:
print(get_word_pattern("HELLO"))    # 0.1.2.2.3
print(get_word_pattern("LETTER"))   # 0.1.2.2.1.3
print(get_word_pattern("SUCCESS"))  # 0.1.2.2.1.0.0
```

Match cipher patterns to dictionary words:

```python
def find_pattern_matches(cipher_word, dictionary):
    """Find all dictionary words matching cipher pattern"""
    target_pattern = get_word_pattern(cipher_word)
    matches = []
    
    for word in dictionary:
        if len(word) == len(cipher_word):
            if get_word_pattern(word) == target_pattern:
                matches.append(word)
    
    return matches

# Example:
# If cipher word is "XYZZY" (pattern: 0.1.2.2.1)
# Matches: "LEVEL", "REFER", "MAMMA", etc.
```

### Common Word Recognition

**Single-letter words:** A, I  
**Two-letter words:** OF, TO, IN, IT, IS, BE, AS, AT, SO, WE, HE, BY, OR, ON, DO, IF, ME, MY  
**Three-letter words:** THE, AND, FOR, ARE, BUT, NOT, YOU, ALL, CAN, HER, WAS, ONE, OUR, OUT

<div class="alert-box">
🎯 STRATEGY: Start with short, common words. Each correct mapping makes the next guess easier!
</div>

<div class="morse-divider">-.. --. </div>

## 🛠️ Attack Methods by Cipher Type

### For Caesar Cipher

**Method: Brute Force (25 attempts)**

```python
def break_caesar(ciphertext):
    """Try all possible shifts"""
    for shift in range(26):
        decrypted = ""
        for char in ciphertext.upper():
            if char.isalpha():
                decrypted += chr((ord(char) - 65 - shift) % 26 + 65)
            else:
                decrypted += char
        print(f"Shift {shift:2d}: {decrypted}")
```

**Optimization: Frequency-based**

```python
def break_caesar_smart(ciphertext):
    """Use frequency to guess shift"""
    from collections import Counter
    
    letters = [c for c in ciphertext.upper() if c.isalpha()]
    most_common = Counter(letters).most_common(1)[0][0]
    
    # Assume most common is 'E'
    shift = (ord(most_common) - ord('E')) % 26
    
    return shift
```

### For Vigenère Cipher

**Step 1: Find Key Length**

Use Kasiski Examination or IC method (see [Vigenère page](/codes/vigenere/)).

**Step 2: Break into Caesar Ciphers**

```python
def break_vigenere(ciphertext, key_length):
    """Break Vigenère given key length"""
    # Split into groups
    groups = [''] * key_length
    
    for i, char in enumerate(ciphertext.upper()):
        if char.isalpha():
            groups[i % key_length] += char
    
    # Break each group as Caesar cipher
    key = ''
    for group in groups:
        shift = break_caesar_smart(group)
        key += chr(shift + ord('A'))
    
    return key
```

### For Substitution Cipher

**Iterative Frequency + Pattern Analysis**

```python
def break_substitution_interactive(ciphertext):
    """Interactive substitution breaking"""
    mapping = {}  # cipher_char -> plain_char
    
    # Start with frequency analysis
    cipher_freq = frequency_analysis(ciphertext)
    english_order = "ETAOINSHRDLCUMWFGYPBVKJXQZ"
    
    print("Suggested initial mappings:")
    for i, (cipher_char, _) in enumerate(list(cipher_freq.items())[:10]):
        plain_char = english_order[i]
        print(f"  {cipher_char} → {plain_char}?")
    
    # Apply and show partial decryption
    def apply_mapping(text, mapping):
        result = ""
        for char in text.upper():
            if char in mapping:
                result += mapping[char]
            elif char.isalpha():
                result += char.lower()
            else:
                result += char
        return result
    
    # Interactive refinement loop
    while True:
        partial = apply_mapping(ciphertext, mapping)
        print(f"\nCurrent decryption:\n{partial}\n")
        
        # Get user input for mappings
        cmd = input("Enter mapping (e.g., 'X=E') or 'done': ")
        if cmd.lower() == 'done':
            break
        
        if '=' in cmd:
            cipher_char, plain_char = cmd.upper().split('=')
            mapping[cipher_char.strip()] = plain_char.strip()
```

### For Morse Code

**Method: Dictionary Lookup**

Morse code is straightforward encoding, not encryption. The challenge comes in various forms:

```python
# Standard Morse decoder
MORSE_DECODE = {
    '.-': 'A', '-...': 'B', '-.-.': 'C', '-..': 'D', '.': 'E',
    '..-.': 'F', '--.': 'G', '....': 'H', '..': 'I', '.---': 'J',
    '-.-': 'K', '.-..': 'L', '--': 'M', '-.': 'N', '---': 'O',
    '.--.': 'P', '--.-': 'Q', '.-.': 'R', '...': 'S', '-': 'T',
    '..-': 'U', '...-': 'V', '.--': 'W', '-..-': 'X', '-.--': 'Y',
    '--..': 'Z',
    '-----': '0', '.----': '1', '..---': '2', '...--': '3',
    '....-': '4', '.....': '5', '-....': '6', '--...': '7',
    '---..': '8', '----.': '9',
    '/': ' '  # Word separator
}

def decode_morse(morse_string):
    """Decode properly spaced Morse"""
    words = morse_string.split(' / ')
    result = []
    for word in words:
        decoded_word = ''
        for letter in word.split(' '):
            if letter in MORSE_DECODE:
                decoded_word += MORSE_DECODE[letter]
        result.append(decoded_word)
    return ' '.join(result)
```

**Challenge Variations:**

1. **No Letter Spacing** - Harder! Requires word dictionary matching
2. **Audio Files** - Detect tone patterns and timing
3. **Visual Morse** - Analyze blink/flash patterns

```python
def decode_ambiguous_morse(morse_no_spaces, word_dict):
    """Decode Morse without letter boundaries"""
    # Build reverse lookup
    text_to_morse = {v: k for k, v in MORSE_DECODE.items() if v != ' '}
    morse_to_text = {v.replace(' ', ''): k 
                     for k, v in text_to_morse.items()}
    
    # Dynamic programming approach
    def find_words(position, current_solution):
        if position == len(morse_no_spaces):
            return [current_solution]
        
        solutions = []
        for word in word_dict:
            morse_word = ''.join(text_to_morse.get(c, '') 
                                for c in word.upper())
            if morse_no_spaces[position:].startswith(morse_word):
                solutions.extend(
                    find_words(position + len(morse_word),
                             current_solution + [word])
                )
        return solutions
    
    return find_words(0, [])

# Audio analysis for Morse
import numpy as np
from scipy.io import wavfile

def decode_morse_audio(audio_file):
    """Extract Morse from audio file"""
    sample_rate, data = wavfile.read(audio_file)
    
    # Convert to mono
    if len(data.shape) > 1:
        data = data.mean(axis=1)
    
    # Normalize and threshold
    data = np.abs(data) / np.max(np.abs(data))
    threshold = 0.1
    is_tone = data > threshold
    
    # Measure segments
    segments = []
    current = is_tone[0]
    duration = 1
    
    for i in range(1, len(is_tone)):
        if is_tone[i] == current:
            duration += 1
        else:
            segments.append((current, duration))
            current = is_tone[i]
            duration = 1
    
    # Estimate dot duration (shortest tone)
    tone_durations = [d for tone, d in segments if tone]
    if not tone_durations:
        return ""
    
    dot_duration = min(tone_durations)
    
    # Convert to dots and dashes
    morse = []
    for is_tone, duration in segments:
        if is_tone:
            units = round(duration / dot_duration)
            morse.append('.' if units <= 1.5 else '-')
        else:
            units = round(duration / dot_duration)
            if units >= 5:
                morse.append(' / ')
            elif units >= 2:
                morse.append(' ')
    
    morse_string = ''.join(morse)
    return decode_morse(morse_string)
```

<div class="alert-box">
🎯 TIP: For Morse, pattern recognition is key. Look for SOS (... --- ...) and common words to establish boundaries!
</div>

<div class="morse-divider">-.. --. </div>

## 🧪 Advanced Techniques

### Kasiski Examination

Find repeated sequences to determine key length:

```python
def kasiski_examination(ciphertext, min_length=3):
    """Find repeated sequences and their distances"""
    text = ''.join(c for c in ciphertext.upper() if c.isalpha())
    sequences = {}
    
    # Find all sequences of min_length or more
    for length in range(min_length, min(20, len(text)//4)):
        for i in range(len(text) - length):
            seq = text[i:i+length]
            positions = []
            
            # Find all occurrences
            for j in range(len(text) - length):
                if text[j:j+length] == seq:
                    positions.append(j)
            
            if len(positions) > 1:
                sequences[seq] = positions
    
    # Calculate distances
    print("Repeated sequences and distances:")
    for seq, positions in sorted(sequences.items(), 
                                 key=lambda x: len(x[0]), 
                                 reverse=True)[:10]:
        distances = [positions[i+1] - positions[i] 
                    for i in range(len(positions)-1)]
        print(f"{seq}: {distances}")
    
    # Find GCD of distances to suggest key length
    from math import gcd
    from functools import reduce
    
    all_distances = []
    for positions in sequences.values():
        all_distances.extend([positions[i+1] - positions[i] 
                             for i in range(len(positions)-1)])
    
    if all_distances:
        common_factor = reduce(gcd, all_distances)
        print(f"\nSuggested key length: {common_factor}")
```

### Dictionary Attack

Use a word list to test possible plaintexts:

```python
def dictionary_attack(ciphertext, wordlist):
    """
    Try decrypting with each word as key
    Useful for Vigenère or keyword-based ciphers
    """
    for word in wordlist:
        # Try word as Vigenère key
        decrypted = vigenere_decrypt(ciphertext, word)
        
        # Check if result contains common English words
        if contains_english_words(decrypted):
            print(f"Possible key: {word}")
            print(f"Decrypted: {decrypted}\n")

def contains_english_words(text, threshold=0.5):
    """Check if text contains enough English words"""
    words = text.split()
    valid_count = sum(1 for w in words if is_english_word(w))
    return (valid_count / len(words)) > threshold if words else False
```

<div class="morse-divider">-.. --. </div>

## 🎓 The Bletchley Park Approach

### Team Organization

At Bletchley Park, success came from:

1. **Specialization** - Different teams for different cipher types
2. **Collaboration** - Sharing insights and techniques
3. **Persistence** - Trying many approaches systematically
4. **Innovation** - Creating new methods when old ones failed

### Apply to Code-A-Thon

**Suggested team roles:**

- **Analyst** - Performs frequency and statistical analysis
- **Coder** - Implements breaking algorithms quickly
- **Researcher** - Consults references and documentation
- **Tester** - Validates partial solutions and results

<div class="typewriter">
"No single person broke Enigma. It took thousands of people, each contributing their expertise. Your team is your greatest asset."
</div>

<div class="morse-divider">-.. --. </div>

## 📦 Building Your Toolkit

### Essential Functions

Create a reusable library with these functions:

```python
# cryptanalysis_toolkit.py

def clean_text(text):
    """Remove non-letters and convert to uppercase"""
    return ''.join(c for c in text.upper() if c.isalpha())

def frequency_analysis(text):
    """Return letter frequency dictionary"""
    # Implementation above
    pass

def index_of_coincidence(text):
    """Calculate IC"""
    # Implementation above
    pass

def caesar_decrypt(text, shift):
    """Decrypt Caesar cipher"""
    pass

def vigenere_decrypt(text, key):
    """Decrypt Vigenère cipher"""
    pass

def substitution_decrypt(text, key):
    """Decrypt substitution cipher"""
    pass

def get_word_pattern(word):
    """Get pattern of word"""
    pass

def auto_break_caesar(ciphertext):
    """Automatically break Caesar"""
    pass

def find_vigenere_key_length(ciphertext):
    """Find Vigenère key length using IC"""
    pass
```

### Quick Analysis Script

```python
def quick_analysis(ciphertext):
    """Run all basic analyses"""
    print("=" * 60)
    print("CIPHER ANALYSIS")
    print("=" * 60)
    
    print(f"\nLength: {len(ciphertext)}")
    
    # Frequency
    freq = frequency_analysis(ciphertext)
    print("\nTop 5 letters:")
    for letter, pct in list(freq.items())[:5]:
        print(f"  {letter}: {pct:.2f}%")
    
    # IC
    ic = index_of_coincidence(ciphertext)
    print(f"\nIndex of Coincidence: {ic:.4f}")
    if ic > 0.060:
        print("  → Likely monoalphabetic")
    elif ic < 0.045:
        print("  → Likely polyalphabetic")
    
    # Patterns
    words = ciphertext.split()
    single = [w for w in words if len(w) == 1]
    if single:
        print(f"\nSingle letters: {set(single)}")
        print("  → Likely A or I")
    
    # Try Caesar brute force
    print("\n" + "=" * 60)
    print("TRYING CAESAR CIPHER:")
    print("=" * 60)
    # Show first few shifts...
```

<div class="morse-divider">-.. --. </div>

## 🏆 Competition Strategies

### Time Management

- **First 15 minutes:** Analyze all ciphers, identify types
- **Next 3-4 hours:** Focus on solvable ciphers first
- **Last hour:** Attempt difficult ciphers or refine solutions

### Point Optimization

1. **Easy wins first** - Caesar is quick points
2. **Parallel work** - Split team if possible
3. **Document methods** - May earn bonus points
4. **Verify solutions** - Wrong answer wastes time

### When You're Stuck

1. **Take a break** - Fresh eyes help
2. **Try a different approach** - Don't fixate
3. **Ask "what if?"** - Challenge assumptions
4. **Look for partial patterns** - Piece together solution

<div class="alert-box">
⚠️ REMEMBER: Speed matters, but accuracy matters more. A correct slow solution beats a fast wrong answer!
</div>

<div class="morse-divider">-.. --. </div>

## 📚 Further Resources

### On This Site

- [Caesar Cipher Details →](/codes/caesar/)
- [Vigenère Cipher Details →](/codes/vigenere/)
- [Substitution Cipher Details →](/codes/substitution/)
- [Mystery Cipher →](/codes/mystery/)
- [Event Rules →](/rules/)

### External Resources

If you are looking for more to do in this area, we invite you to check out the below projects.

- **CyberChef** - Online crypto tool for quick tests
  - [https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/)
- **Cryptopals** - Advanced cryptography challenges
  - [https://cryptopals.com/](https://cryptopals.com/)
- **dcode.fr** - Cipher identification and breaking tools
  - [https://www.dcode.fr/en](https://www.dcode.fr/en)

<div class="typewriter">
"The codebreaker's greatest tools are not computers or algorithms, but curiosity, persistence, and the willingness to try again."
</div>

---

<div class="morse-divider">···− −−− −−− −·· ·−·· ··− −·−· −·−</div>
