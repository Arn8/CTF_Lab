# CTF_Lab
Practice lab: locate hidden directories, crack stego artefacts - all flags are encrypted

Custom CTF created for locating hidden directories, using steganography to reveal hidden flag in image and decrypting all the encrypeted flags.

Home page
Instructions:Capture 2 flags to complete the challenge.  
Flags follow format: `WATCHDOGS{...}`

Challenge 1 — Directory Traversal
Description: Small directory traversal / discovery challenge.

- `/uploads` — contains 1 text and 5 folders; one folder must be discovered by enumeration.  
- `/New_folder` — contains the flag and a riddle/clue to the next directory.  
- Flag encryption: ROT47 (the flag inside is ROT47-encrypted).

Code to decrypt:
python3 - <<'PY'
s = "ROT47_TEXT"
print(''.join(chr(33+((ord(c)-33+47)%94)) if 33 <= ord(c) <= 126 else c for c in s))
PY

Challenge 2 — Steganography
Description: An image contains an embedded `flag.txt` that requires a passkey to extract.

- `/final` — contains 1 hint and 2 folders.  
- `/final/Image` — contains an image with an embedded `flag.txt` (the extracted flag is encrypted with a Vigenère cipher).  
- `/final/Passkey` — contains a file whose content is the passkey; the passkey itself is Base64-encoded 6 times.

Code to decrypt:
1. For passkey
s="BASE64_ENCODED_STRING_HERE"
for i in {1..6}; do
  s=$(echo -n "$s" | base64 -d)
done
echo "$s"

2. For vigenère cipher
# vigenere_decrypt.py
def decrypt_vigenere(ct, key):
    pt = []
    ki = 0
    for c in ct:
        if c.isalpha():
            a = 'A' if c.isupper() else 'a'
            pt.append(chr((ord(c) - ord(a) - (ord(key[ki % len(key)].lower()) - ord('a')) ) % 26 + ord(a)))
            ki += 1
        else:
            pt.append(c)
    return ''.join(pt)

cipher = "CIPHERTEXT_HERE"
key = "KEY_HERE"
print(decrypt_vigenere(cipher, key))

Note: You can use online tools to decrypt the flags, websites like Cyberchef, dcode...
