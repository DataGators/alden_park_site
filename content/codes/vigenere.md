---
title: "Vigenère Cipher"
description: "The polyalphabetic 'unbreakable' cipher"
weight: 2
---

## 🇫🇷 Vigenère Cipher: Le Chiffre Indéchiffrable

<div class="classified-stamp">SECRET</div>

The **Vigenère Cipher** was long considered unbreakable—earning the nickname "le chiffre indéchiffrable" (the indecipherable cipher). It uses a keyword to apply multiple Caesar shifts, making simple frequency analysis ineffective.

<div class="morse-divider">-.. --. </div>

## 📜 Historical Background

### Origins

- **1467:** First described by Leon Battista Alberti
- **1553:** Refined by Giovan Battista Bellaso
- **1586:** Misattributed to Blaise de Vigenère (name stuck)
- **1863:** Finally broken by Friedrich Kasiski

### Famous Uses

The Vigenère cipher was used by:
- **Confederate forces** during the American Civil War
- **The Zodiac Killer** in some of his messages (1960s-70s)
- **Various resistance movements** during WWII

<div class="typewriter">
"For three centuries, cryptographers believed the Vigenère cipher was unbreakable. They were wrong."
</div>

<div class="morse-divider">-.. --. </div>

## 🔧 How It Works

### The Tabula Recta

The Vigenère cipher uses a table called the **tabula recta** (Vigenère square):

```
    A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
A   A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
B   B C D E F G H I J K L M N O P Q R S T U V W X Y Z A
C   C D E F G H I J K L M N O P Q R S T U V W X Y Z A B
D   D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
E   E F G H I J K L M N O P Q R S T U V W X Y Z A B C D
...
```

### Encryption Process

1. **Choose a keyword:** e.g., "LEMON"
2. **Repeat the keyword** to match plaintext length
3. **For each letter:**
   - Find plaintext letter in first row
   - Find keyword letter in first column
   - Cipher letter is at their intersection

### Step-by-Step Example

**Plaintext:** ATTACKATDAWN  
**Keyword:** LEMON (repeated: LEMONLEMONLE)

```
Plaintext:  A T T A C K A T D A W N
Key:        L E M O N L E M O N L E
Ciphertext: L X F O P V E F R N H R
```

**Breakdown:**
- A + L = L (shift 11)
- T + E = X (shift 4)
- T + M = F (shift 12)
- A + O = O (shift 14)
- C + N = P (shift 13)
- ...and so on

### Mathematical Formula

For plaintext letter $P$, key letter $K$:

**Encryption:** $C = (P + K) \mod 26$  
**Decryption:** $P = (C - K) \mod 26$

<div class="morse-divider">-.. --. </div>

## 💻 Implementation Examples

### Python

```python
def vigenere_encrypt(plaintext, key):
    result = []
    key = key.upper()
    key_index = 0
    
    for char in plaintext.upper():
        if char.isalpha():
            # Get shift from current key letter
            shift = ord(key[key_index % len(key)]) - ord('A')
            # Encrypt character
            encrypted = chr((ord(char) - ord('A') + shift) % 26 + ord('A'))
            result.append(encrypted)
            key_index += 1
        else:
            result.append(char)
    
    return ''.join(result)

def vigenere_decrypt(ciphertext, key):
    result = []
    key = key.upper()
    key_index = 0
    
    for char in ciphertext.upper():
        if char.isalpha():
            shift = ord(key[key_index % len(key)]) - ord('A')
            decrypted = chr((ord(char) - ord('A') - shift + 26) % 26 + ord('A'))
            result.append(decrypted)
            key_index += 1
        else:
            result.append(char)
    
    return ''.join(result)

# Example
plaintext = "ATTACK AT DAWN"
key = "LEMON"
encrypted = vigenere_encrypt(plaintext, key)
print(f"Encrypted: {encrypted}")  # LXFOPV EF RNHR

decrypted = vigenere_decrypt(encrypted, key)
print(f"Decrypted: {decrypted}")  # ATTACK AT DAWN
```

### JavaScript

```javascript
function vigenere(text, key, decrypt = false) {
    const keyUpper = key.toUpperCase();
    let keyIndex = 0;
    
    return text.toUpperCase().split('').map(char => {
        if (char.match(/[A-Z]/)) {
            const charCode = char.charCodeAt(0) - 65;
            const keyChar = keyUpper.charCodeAt(keyIndex % keyUpper.length) - 65;
            keyIndex++;
            
            let result;
            if (decrypt) {
                result = (charCode - keyChar + 26) % 26;
            } else {
                result = (charCode + keyChar) % 26;
            }
            
            return String.fromCharCode(result + 65);
        }
        return char;
    }).join('');
}

// Example
const encrypted = vigenere("ATTACK AT DAWN", "LEMON");
console.log(encrypted);  // LXFOPV EF RNHR

const decrypted = vigenere(encrypted, "LEMON", true);
console.log(decrypted);  // ATTACK AT DAWN
```

### Java

```java
public class VigenereCipher {
    public static String encrypt(String plaintext, String key) {
        StringBuilder result = new StringBuilder();
        String keyUpper = key.toUpperCase();
        int keyIndex = 0;
        
        for (char c : plaintext.toUpperCase().toCharArray()) {
            if (Character.isLetter(c)) {
                int shift = keyUpper.charAt(keyIndex % keyUpper.length()) - 'A';
                char encrypted = (char) (((c - 'A' + shift) % 26) + 'A');
                result.append(encrypted);
                keyIndex++;
            } else {
                result.append(c);
            }
        }
        return result.toString();
    }
    
    public static String decrypt(String ciphertext, String key) {
        StringBuilder result = new StringBuilder();
        String keyUpper = key.toUpperCase();
        int keyIndex = 0;
        
        for (char c : ciphertext.toUpperCase().toCharArray()) {
            if (Character.isLetter(c)) {
                int shift = keyUpper.charAt(keyIndex % keyUpper.length()) - 'A';
                char decrypted = (char) (((c - 'A' - shift + 26) % 26) + 'A');
                result.append(decrypted);
                keyIndex++;
            } else {
                result.append(c);
            }
        }
        return result.toString();
    }
}
```

<div class="morse-divider">-.. --. </div>

## 🔓 Weaknesses & Breaking Techniques

### Why It Was Considered Unbreakable

Unlike Caesar cipher:
- **No single shift** - Each letter potentially uses a different shift
- **Frequency analysis fails** - Letter frequencies are flattened
- **Multiple possible plaintexts** - Without knowing key length, many solutions seem valid

### The Breakthrough: Kasiski Examination (1863)

**Key Insight:** Repeated words in plaintext create repeated sequences in ciphertext when they align with the same key letters.

**Method:**
1. **Find repeated sequences** in ciphertext (3+ letters)
2. **Measure distances** between repetitions
3. **Find GCD** of all distances → likely key length
4. **Break into Caesar ciphers** - Each position uses same shift

### Example of Kasiski Method

```
Ciphertext: LXFOPVEFRNHRLXFOPV...
            ^^^^^^        ^^^^^^
            LXFOPV repeats after 12 letters
```

If sequences repeat at distances: 12, 24, 36, 48...  
GCD = 12, so key length is likely 3, 4, 6, or 12.

### Breaking Method 1: Known Key Length

```python
def break_vigenere_known_length(ciphertext, key_length):
    # Separate into groups by key position
    groups = [''] * key_length
    for i, char in enumerate(ciphertext):
        if char.isalpha():
            groups[i % key_length] += char
    
    # Each group is a Caesar cipher
    key = ''
    for group in groups:
        # Use frequency analysis on each group
        shift = find_caesar_shift(group)  # See Caesar page
        key += chr(shift + ord('A'))
    
    return key
```

### Breaking Method 2: Index of Coincidence

The **Index of Coincidence (IC)** measures how letter frequencies in text compare to random vs. natural language:
- **Random text:** IC ≈ 0.038
- **English text:** IC ≈ 0.065

**Process:**
1. Try different key lengths (1-20)
2. For each length, split ciphertext into groups
3. Calculate IC for each group
4. Key length with IC closest to 0.065 is likely correct

```python
def index_of_coincidence(text):
    text = ''.join(c for c in text.upper() if c.isalpha())
    n = len(text)
    if n <= 1:
        return 0
    
    frequency = {}
    for char in text:
        frequency[char] = frequency.get(char, 0) + 1
    
    sum_freq = sum(f * (f - 1) for f in frequency.values())
    return sum_freq / (n * (n - 1))

def find_key_length(ciphertext, max_length=20):
    best_length = 1
    best_avg_ic = 0
    
    for length in range(1, max_length + 1):
        groups = [''] * length
        for i, char in enumerate(ciphertext.upper()):
            if char.isalpha():
                groups[i % length] += char
        
        avg_ic = sum(index_of_coincidence(g) for g in groups) / length
        
        if avg_ic > best_avg_ic:
            best_avg_ic = avg_ic
            best_length = length
    
    return best_length
```

<div class="alert-box">
⚠️ STRATEGY TIP: For Vigenère, finding the key length is 90% of the battle. Once you know the length, it's just multiple Caesar ciphers!
</div>

<div class="morse-divider">-.. --. </div>

## 🎯 Practice Challenges

### Challenge 1 (Easy - Key Known)
```
Key: CRYPTO
Ciphertext: EMGLDOAYFC NPODOPWHVV RW VCRYEMOW
```

### Challenge 2 (Medium - Key Length Known)
```
Key Length: 5
Ciphertext: RIJVS UYVJN BLDOK KDKKL PSXOP RUJIV SMJSE FFPOD KTQCY
```
<details>
<summary>Hint</summary>
Use frequency analysis on each of the 5 Caesar groups
</details>

### Challenge 3 (Hard - Unknown Key)
```
KSMEH ZBBLK SOYEG QGZPL RXVNV BPVNR LEJSS GQJSM 
EHMEF ZTIKM EFZBM JXMG DLXRX BKZMV WAWZQ VEJHS 
RJQXS YVNZJ VYMJW XSQJX SMEHM EFZRJ NMGFP
```
<details>
<summary>Hint</summary>
Use Kasiski or IC method to find key length first
</details>

<div class="morse-divider">-.. --. </div>

## 🚀 Advanced Topics

### Autokey Cipher

A variation where the key is the plaintext itself (after an initial primer):

```
Plaintext:  ATTACK AT DAWN
Key:        KEYWOR DA TTAC
Ciphertext: KTTWXR DT DFNP
```

### Running Key Cipher

Uses a very long key (often from a book):
```
Key: Text from "War and Peace", page 42...
```

This makes Kasiski useless but requires key synchronization.

### One-Time Pad (Unbreakable)

If the key is:
- Truly random
- At least as long as the message
- Used only once
- Kept perfectly secret

Then the cipher is **information-theoretically secure** (unbreakable even with infinite computing power).

<div class="morse-divider">-.. --. </div>

## 📊 Comparison: Caesar vs. Vigenère

| Feature | Caesar | Vigenère |
|---------|---------|-----------|
| Key Space | 25 | $26^n$ (n = key length) |
| Breakability | Trivial | Moderate |
| Frequency Analysis | Direct | Requires key length |
| Brute Force | Always works | Impractical for long keys |
| Historical Period | Ancient | Renaissance to Modern |

<div class="morse-divider">-.. --. </div>

## 📚 Further Reading

- [Learn general breaking techniques →](/breaking/)
- [Try the Substitution Cipher →](/codes/substitution/)
- [Back to Codes Overview →](/codes/)

<div class="typewriter">
"The Vigenère cipher teaches us that complexity is not security. Even sophisticated systems have patterns—you just need to know where to look."
</div>

---

<div class="morse-divider">···− −−− −−− −·· ·−·· ··− −·−· −·−</div>
