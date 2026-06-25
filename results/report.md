# AudioGuard Experiment Report

## Research Question

Can adversarial training improve the robustness of an audio classification model against FGSM and PGD attacks without sacrificing clean accuracy?

---

## Experimental Setup

### Dataset

Synthetic UrbanSound8K-style dataset with:

* 10 audio classes
* Frequency jitter
* Noise augmentation
* Deliberate class overlap

### Model

* 4-layer CNN
* Batch Normalization
* Dropout
* Label smoothing (0.1)

### Attacks

#### FGSM

* ε = 0.03

#### PGD

* ε = 0.03
* 10 attack steps

### Hardware

* NVIDIA T4 GPU
* Google Colab

---

## Methodological Validation

Initial experiments reported nearly perfect robustness under FGSM and PGD attacks.

Further investigation revealed that:

* training loss had collapsed
* gradients had become extremely small
* the synthetic dataset was overly separable

The benchmark was revised using:

* frequency jitter
* stronger noise augmentation
* class overlap
* label smoothing

to maintain meaningful gradients and ensure attacks were evaluating genuine model vulnerability.

---

## Results

| Model    | Clean | FGSM  | PGD   |
| -------- | ----- | ----- | ----- |
| Baseline | 81.9% | 68.8% | 64.4% |
| Hardened | 81.9% | 75.6% | 75.0% |

---

## Robustness Improvements

### FGSM

```text
68.8% → 75.6%
```

Improvement:

```text
+6.8 percentage points
```

### PGD

```text
64.4% → 75.0%
```

Improvement:

```text
+10.6 percentage points
```

---

## Key Findings

1. Adversarial training improved robustness against both FGSM and PGD attacks.

2. The strongest gains occurred under PGD, suggesting improvements generalized beyond a single attack method.

3. Clean accuracy remained unchanged, indicating robustness gains were achieved without sacrificing standard performance.

4. Evaluation methodology matters. Early results suggesting perfect robustness were traced to gradient saturation rather than genuine model resilience.

---

## Limitations

* Synthetic dataset rather than real-world audio.
* Single model architecture.
* Limited attack budget.
* Single-seed evaluation.

---

## Future Work

* Evaluate on real-world audio datasets.
* Compare additional adversarial training methods.
* Evaluate transfer attacks.
* Test robustness of voice-cloning detection systems.
* Run multi-seed experiments and confidence interval analysis.

---

## Conclusion

Adversarial training improved robustness against both FGSM and PGD attacks while maintaining identical clean accuracy. The largest improvement was observed under the stronger PGD attack, increasing robustness by 10.6 percentage points. The project also highlights the importance of validating evaluation methodology before drawing conclusions about model robustness.
