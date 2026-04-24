# KIET_AI_DebugChallenge

# Bugs
Bug 2 → Crash immediately: vocabulary is empty since all text tokens are letters, never digits

Bug 3 + 4 → Training runs but loss never decreases; accuracy stays near random (model gets zero-vector inputs and wrong gradients)

Bug 5 → Training looks successful, but during chat the bot always says "I don't understand" because threshold 0.95 is almost never reached

Bug 1 → Even after fixing bugs 2–5, the bot still refuses every response — students must find the config value

The bugs are ordered by discovery difficulty, forcing students to debug in layers: crash → training failure → inference failure.

SOLUTIONS OF BUGS:
Bug 1 | _one_hot()        : vec[idx] was set to 0.0 → fixed to 1.0

Bug 2 | preprocess()      : tok.isdigit() → tok.isalpha() (kept digits, dropped letters)

Bug 3 | backward()        : tanh derivative used (1 + h²) → fixed to (1 - h²)

Bug 4 | _softmax()        : missing numerical stabilisation → added (x - x.max())

Bug 5 | CONFIDENCE_THRESHOLD : was 0.95 (almost never reached) → lowered to 0.70
