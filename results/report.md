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

Full configuration (attack parameters, label smoothing, seed) is recorded in `results/results.json`.

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

2. The strongest gains occurred under PGD, which is consistent with the idea that the improvement is not narrowly overfit to a single attack method — though this is based on a single seed and two attack types, and has not yet been tested against attacks the model wasn't trained against.

3. Clean accuracy remained unchanged, indicating robustness gains were achieved without sacrificing standard performance.

4. Evaluation methodology matters. Early results suggesting perfect robustness were traced to gradient saturation rather than genuine model resilience.

---

## Reproducibility

All results in this report can be regenerated from a single run of the notebook:

1. Open `notebook/AudioGuard.ipynb` in Colab with a CUDA GPU runtime (T4 or better).
2. Run all cells top to bottom (**Runtime → Run all**). The dataset is generated programmatically; no external downloads are required.
3. Section 6b is a built-in sanity check: it verifies training loss and gradient magnitude are in a healthy range before any attack is run. If this check warns, the attack results that follow should not be trusted.
4. Section 9 (baseline) and Section 10 (hardened) save results to `results/results_<RUN_ID>.json`, matching the schema in `results/results.json`.

Random seed is fixed in the config cell (Section 2) for reproducibility within a single environment; results may vary slightly across different GPU types or PyTorch versions.

---

## Limitations

* Synthetic dataset rather than real-world audio.
* Single model architecture.
* Limited attack budget.
* Single-seed evaluation — the reported improvements have not yet been confirmed to hold across multiple random seeds, so the exact percentage-point gains should be treated as a first estimate rather than a stable effect size.
* No evaluation against attacks the hardened model wasn't trained on (e.g. PGD with a larger ε, or a different attack algorithm entirely), which would be a stronger test of generalized robustness rather than robustness to the specific attack used during adversarial training.

---

## Future Work

* Evaluate on real-world audio datasets.
* Run multi-seed experiments and report confidence intervals on the robustness gains, rather than a single point estimate per condition.
* Compare additional adversarial training methods.
* Evaluate transfer attacks (attacks crafted on one model and applied to another) and held-out attack types not used during hardening.
* Test robustness of voice-cloning detection systems specifically.

---

## Conclusion

Adversarial training improved robustness against both FGSM and PGD attacks while maintaining identical clean accuracy, with the largest gain observed under the stronger PGD attack (+10.6 percentage points). These results are based on a single training seed and a limited attack budget, so the precise magnitude of improvement should be confirmed with multi-seed runs before being treated as a stable effect. The project also highlights the importance of validating evaluation methodology — gradient saturation, not genuine robustness, was the original explanation for an early, misleadingly perfect result — before drawing conclusions about model robustness.