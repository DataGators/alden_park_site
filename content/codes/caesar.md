---
title: "Caesar Cipher"
description: "The ancient Roman shift cipher"
weight: 1
---

## 🏛️ Caesar Cipher: The Ancient Shift

<div class="classified-stamp">DECLASSIFIED</div>

The **Caesar Cipher** is one of the oldest known encryption techniques, dating back to Julius Caesar (100-44 BC), who used it to protect military messages. It's a type of substitution cipher where each letter is shifted by a fixed number of positions in the alphabet.

<div class="morse-divider">- ·· · · ·</div>

## 📜 Historical Background

### Julius Caesar's Method

Roman historian Suetonius documented Caesar's use of this cipher:

<div class="typewriter">
"If he had anything confidential to say, he wrote it in cipher, that is, by so changing the order of the letters of the alphabet, that not a word could be made out."
</div>

Caesar typically used a shift of 3, meaning:
- A → D
- B → E
- C → F
- ...and so on

### Military Applications

The cipher was effective in Caesar's time because:
1. Most people were illiterate
2. Those who could read didn't know the system
3. Messengers couldn't betray information they couldn't read

## 🔧 How It Works

### The Mechanism

The Caesar Cipher shifts each letter by a fixed number (the **key**). With a shift of 3:

```
Plain Alphabet:  A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
Cipher Alphabet: D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
```

### Encryption Example

**Message:** "ATTACK AT DAWN"  
**Shift:** 3  
**Process:**
```
A → D
T → W
T → W
A → D
C → F
K → N
...
```
**Result:** "DWWDFN DW GDZQ"

### Mathematical Formula

For a shift of `k`:
- **Encryption:** $E(x) = (x + k) \mod 26$
- **Decryption:** $D(x) = (x - k) \mod 26$

Where `x` is the letter's position (A=0, B=1, ..., Z=25)

<div class="morse-divider">- ·· · · ·</div>

## 💻 Implementation Examples

### Python

```python
def caesar_encrypt(plaintext, shift):
    result = ""
    for char in plaintext.upper():
        if char.isalpha():
            shifted = chr((ord(char) - 65 + shift) % 26 + 65)
            result += shifted
        else:
            result += char
    return result

def caesar_decrypt(ciphertext, shift):
    return caesar_encrypt(ciphertext, -shift)

# Example usage
message = "HELLO WORLD"
encrypted = caesar_encrypt(message, 3)
print(f"Encrypted: {encrypted}")  # KHOOR ZRUOG

decrypted = caesar_decrypt(encrypted, 3)
print(f"Decrypted: {decrypted}")  # HELLO WORLD
```

### JavaScript

```javascript
function caesarCipher(text, shift, decode = false) {
    if (decode) shift = -shift;
    
    return text.toUpperCase().split('').map(char => {
        if (char.match(/[A-Z]/)) {
            const code = char.charCodeAt(0);
            let shifted = ((code - 65 + shift) % 26);
            if (shifted < 0) shifted += 26;
            return String.fromCharCode(shifted + 65);
        }
        return char;
    }).join('');
}

// Example
console.log(caesarCipher("HELLO", 3));        // KHOOR
console.log(caesarCipher("KHOOR", 3, true));  // HELLO
```

### Java

```java
public class CaesarCipher {
    public static String encrypt(String text, int shift) {
        StringBuilder result = new StringBuilder();
        
        for (char c : text.toUpperCase().toCharArray()) {
            if (Character.isLetter(c)) {
                int shifted = ((c - 'A' + shift) % 26 + 26) % 26;
                result.append((char) (shifted + 'A'));
            } else {
                result.append(c);
            }
        }
        return result.toString();
    }
    
    public static String decrypt(String text, int shift) {
        return encrypt(text, -shift);
    }
}
```

<div class="morse-divider">- ·· · · ·</div>

## 🔓 Weaknesses & Breaking Techniques

### Primary Weakness: Limited Key Space

The Caesar Cipher has only **25 possible keys** (shifts 1-25). This makes it vulnerable to **brute force** attacks.

### Breaking Method 1: Brute Force

Simply try all 25 possible shifts:

```python
def break_caesar_brute_force(ciphertext):
    print("Trying all possible shifts:\n")
    for shift in range(26):
        decrypted = caesar_decrypt(ciphertext, shift)
        print(f"Shift {shift:2d}: {decrypted}")
    
# Most plaintext will be recognizable to human eye
```

### Breaking Method 2: Frequency Analysis

English letter frequencies:
- Most common: E, T, A, O, I, N
- Least common: Z, Q, X, J, K

**Technique:**
1. Find the most frequent letter in ciphertext
2. Assume it represents 'E' (most common in English)
3. Calculate the shift
4. Decrypt and verify

```python
from collections import Counter

def break_caesar_frequency(ciphertext):
    # Remove spaces and count letters
    letters = [c for c in ciphertext.upper() if c.isalpha()]
    frequency = Counter(letters)
    
    # Most common letter likely represents 'E'
    most_common = frequency.most_common(1)[0][0]
    
    # Calculate shift (E is position 4)
    shift = (ord(most_common) - ord('E')) % 26
    
    return caesar_decrypt(ciphertext, shift), shift
```

### Breaking Method 3: Dictionary Attack

1. Try each possible shift
2. Check if result contains valid English words
3. Score based on word dictionary matches

<div class="alert-box">
⚠️ STRATEGY TIP: For short messages, brute force is fastest. For longer messages, frequency analysis is more reliable.
</div>

<div class="morse-divider">- ·· · · ·</div>

## 🎯 Practice Challenges

Try breaking these Caesar-encrypted messages:

### Challenge 1 (Easy)
```
KHOOR ZRUOG
```
<details>
<summary>Hint</summary>
Shift = 3
</details>

### Challenge 2 (Medium)
```
WKH TXLFN EURZQ IRA MXPSV RYHU WKH ODCB GRJ
```
<details>
<summary>Hint</summary>
Use frequency analysis or brute force
</details>

### Challenge 3 (Hard)
```
ESP QTDPC TD Z QTDPC, LYO ESPCP ZCP YZ XCPNPDEPO DPNCPED
```
<details>
<summary>Hint</summary>
Look for short common words like "THE" or "AND"
</details>

<div class="morse-divider">- ·· · · ·</div>

## 🚀 Advanced Topics

### ROT13 - A Special Case

ROT13 uses a shift of 13, which is its own inverse:
```
ROT13(ROT13(x)) = x
```

This makes it useful for hiding spoilers or puzzle solutions, as the same operation encrypts and decrypts.

### Handling Non-English Text

Different languages have different letter frequencies:
- **Spanish:** E, A, O are most common
- **German:** E, N, I are most common
- **French:** E, A, S are most common

### Character Set Expansion

The Caesar Cipher can be extended to:
- Include lowercase letters
- Include numbers and symbols
- Use different alphabets (Cyrillic, Greek, etc.)

<div class="morse-divider">- ·· · · ·</div>

## 📚 Further Reading

- [Learn more about breaking codes →](/breaking/)
- [Try the Vigenère Cipher →](/codes/vigenere/)
- [Event Rules & FAQ →](/rules/)

<div class="typewriter">
"The simplest ciphers often reveal the deepest truths about cryptanalysis. Master the Caesar, and you're ready for greater challenges."
</div>

---

<div class="morse-divider">···− −−− −−− −·· ·−·· ··− −·−· −·−</div>
