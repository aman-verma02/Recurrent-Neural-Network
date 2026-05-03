## Recurrent Neural Network



### Embedding - converting the text into vectors

---

# Step 0: Our example sentences

1. "I love AI"
2. "AI loves me"
3. "I enjoy learning"

---

# Step 1: Tokenization

Split sentences into words:

```
["I", "love", "AI"]
["AI", "loves", "me"]
["I", "enjoy", "learning"]
```

---

# Step 2: Build vocabulary

Collect all unique words:

```
["I", "love", "AI", "loves", "me", "enjoy", "learning"]
```

Now assign each word an index:

```
"I"        → 0  
"love"     → 1  
"AI"       → 2  
"loves"    → 3  
"me"       → 4  
"enjoy"    → 5  
"learning" → 6  
```

---

# Step 3: Convert sentences → index sequences

Now replace each word with its index:

```
"I love AI"        → [0, 1, 2]  
"AI loves me"      → [2, 3, 4]  
"I enjoy learning" → [0, 5, 6]
```

At this point:
👉 Words are now **numbers**, but still not useful for learning.

---

# Step 4: One-hot encoding (intermediate idea)

Each word becomes a vector of size = vocab size (7):

Example:

"I" (index 0):

```
[1, 0, 0, 0, 0, 0, 0]
```

"love" (index 1):

```
[0, 1, 0, 0, 0, 0, 0]
```

Problems:

* Very sparse
* No meaning (all words equally distant)

---

# Step 5: Embedding matrix (the key step)

We create a matrix:

* vocab size = 7
* embedding size = 3 (just for example)

```
E (7 × 3):

"I"        → [0.2,  0.1,  0.4]
"love"     → [0.9,  0.3,  0.5]
"AI"       → [0.8,  0.7,  0.6]
"loves"    → [0.85, 0.25, 0.45]
"me"       → [0.1,  0.9,  0.3]
"enjoy"    → [0.88, 0.35, 0.55]
"learning" → [0.75, 0.65, 0.8]
```

---

# Step 6: Convert indices → vectors (embedding lookup)

Now replace each index with its vector.

---

### Sentence 1: "I love AI"

```
[0, 1, 2]
→
[
 [0.2, 0.1, 0.4],   # I
 [0.9, 0.3, 0.5],   # love
 [0.8, 0.7, 0.6]    # AI
]
```

---

### Sentence 2: "AI loves me"

```
[2, 3, 4]
→
[
 [0.8,  0.7,  0.6],   # AI
 [0.85, 0.25, 0.45],  # loves
 [0.1,  0.9,  0.3]    # me
]
```

---

### Sentence 3: "I enjoy learning"

```
[0, 5, 6]
→
[
 [0.2,  0.1, 0.4],   # I
 [0.88, 0.35, 0.55], # enjoy
 [0.75, 0.65, 0.8]   # learning
]
```

---

# Step 7: This is what RNN actually receives

At each time step, the RNN gets:

* a vector (not a word)
* e.g. for sentence 1:

```
t=1 → [0.2, 0.1, 0.4]
t=2 → [0.9, 0.3, 0.5]
t=3 → [0.8, 0.7, 0.6]
```

---

# Step 8: Important insight

Initially:

* These vectors are **random**

During training:

* If "love" and "enjoy" behave similarly → vectors become similar
* If "AI" appears in similar contexts → it learns meaningful position

---

# Final mental picture

```
Text → Tokens → Indices → Embedding Matrix → Dense Vectors → RNN
```

---

