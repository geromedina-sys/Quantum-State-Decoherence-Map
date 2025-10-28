# **The Decoherence Death Map: Dynamic Fingerprinting for Robust Quantum State Classification**

**Author:** Geronimo Marco Medina
**Affiliation:** Independent Researcher, La Plata, Buenos Aires, Argentina
**Date:** October 28, 2025

---

## **Abstract**

This project introduces a **dynamic method** for classifying and diagnosing multi-qubit entangled states (GHZ, W, Dicke classes) by leveraging their decoherence behavior, overcoming critical limitations of static approaches. We demonstrate that while certain quantum states can be indistinguishable based on static physical metrics (e.g., W vs. Dicke(k=2) states for N=3 qubits sharing identical initial fingerprints), their **evolutionary trajectory under decoherence** provides a unique and robust signature. We construct a 3D **"Decoherence Death Map"** utilizing physically meaningful features ($H_Z$ - Shannon entropy, $E_X$ - global X-correlation, $H_q$ - average partial entropy). This map visualizes the distinct "death paths" followed by different entanglement families, enabling unambiguous classification even for problematic cases (like W vs. Dicke at N=3, or GHZ vs. its phase-flipped version) and under various noise channels (Amplitude Damping T1, Phase Flip T2, and mixtures thereof). Furthermore, the analysis reveals that the **type of noise** also leaves a distinct signature on the trajectory shape and endpoint. This dynamic fingerprinting method holds significant potential as a fast, resource-efficient diagnostic tool for real-time state characterization and error analysis in experimental quantum computing platforms. ⚛️🔬🗺️

---

## **1. Introduction: The Challenge of Quantum State Classification**

Accurately characterizing multi-qubit quantum states is paramount for advancing quantum computation and information processing. However, the standard method, **Quantum State Tomography (QST)**, scales exponentially in resources (measurements and classical post-processing time) with the number of qubits ($N$), quickly becoming intractable for larger systems.  This necessitates the development of efficient "shortcuts": identifying a small set of easily measurable **physical metrics (features)** that serve as effective "fingerprints" to classify the state's entanglement class.

Initial explorations focused on **static features**, calculated from the state at a single point in time ($t=0$). Metrics like the **Shannon entropy of computational basis probabilities ($H_Z$)**, **global correlation expectation values ($E_X = \langle X^{\otimes N} \rangle$)**, and **average partial entropy ($H_q$)** showed promise in distinguishing canonical families like GHZ, W, and Dicke states.

However, this static "snapshot" approach suffers from **critical limitations**:
1.  **Static Degeneracy:** We discovered that for $N=3$ qubits, the W ($k=1$) and Dicke ($k=2$) states possess **identical static feature vectors** ($H_Z = \log_2(3)$, $E_X = 0$, $H_q \approx 0.918$). They are "static twins," indistinguishable by these metrics alone!
2.  **Blindness to Relative Phase:** Probability-based metrics ($H_Z, H_q$) cannot differentiate states differing only by a relative phase, such as the standard GHZ state ($|0...0\rangle + |1...1\rangle$) and its phase-flipped "Impostor" ($|0...0\rangle - |1...1\rangle$). While incorporating $E_X$ resolved this specific case, a more general approach was sought.

These challenges led us to hypothesize: If the static "photo" is insufficient, could the **"movie" – the dynamics of how the state decoheres and "dies" over time –** provide a complete and unambiguous fingerprint? 🤔

---

## **2. Method: Simulating Quantum "Death" and Mapping Fingerprints**

To test our hypothesis, we implemented the following computational framework:

1.  **Quantum States:** We focused on $N=3$ and $N=5$ qubit systems, studying four representative multi-qubit entanglement families:
    * **W ($k=1$):** Uniform superposition of single-excitation states.
    * **GHZ:** Equal superposition of $|0...0\rangle$ and $|1...1\rangle$.
    * **Dicke ($k=2$):** Uniform superposition of double-excitation states.
    * **Impostor:** GHZ state with a negative relative phase.
2.  **Feature Space:** We selected three key physical metrics, calculated from the density matrix $\rho(t)$ at each time step $t$:
    * **$H_Z(t)$:** Shannon entropy of the diagonal elements of $\rho(t)$ in the computational basis. Measures disorder/mixedness in this basis.
    * **$E_X(t)$:** Expectation value of the global correlation operator $X^{\otimes N}$, $Tr[\rho(t) X^{\otimes N}]$. Sensitive to global phase coherence.
    * **$H_q(t)$:** Average von Neumann entropy of the single-qubit reduced density matrices. Quantifies average bipartite entanglement.
3.  **Decoherence Simulation:** We modeled the system's interaction with its environment using the **Quantum Channel formalism**, specifically via **Kraus Operators** ($K_i$). We simulated the time evolution of the density matrix $\rho(t) = \sum_i K_i(\gamma(t)) \rho(0) K_i^\dagger(\gamma(t))$, where $\gamma(t)$ is a time-dependent parameter quantifying the noise strength (from $\gamma=0$, pure state, to $\gamma=1$, fully decohered state). We implemented two fundamental noise models and their mixture:
    * **Amplitude Damping (T1 Noise):** Models energy relaxation ($|1\rangle \to |0\rangle$).
    * **Phase Flip (T2 Noise):** Models loss of phase coherence (Z errors).
    * **Mixed Noise:** Sequential application of T1 and T2 channels with varying relative strengths (e.g., 50/50 or 80/20) to mimic more realistic environments.
4.  **Trajectory Generation:** For each initial pure state $\rho(0)$ and each noise model, we simulated the evolution over $N_{STEPS}=50$, calculating the feature vector $[H_Z(t_i), E_X(t_i), H_q(t_i)]$ at each step $t_i$ (corresponding to $\gamma_i$).
5.  **Visualization - The "Decoherence Death Map":** We plotted these sequences of feature vectors as **continuous trajectories** in a 3D space spanned by the $H_Z$, $E_X$, and $H_q$ axes. 🗺️

---

## **3. Results: Unveiling the Dynamic Fingerprints**

Our simulations strikingly confirmed the hypothesis that decoherence dynamics provide unique state signatures.

### **3.1 Static Method Failure (N=3)**

The initial simulation for $N=3$ pure states ($t=0$) clearly showed the degeneracy problem: W (blue) and Dicke(k=2) (green) states occupy the **exact same point** in the feature space, rendering them indistinguishable statically.

<img width="931" height="630" alt="seccion31" src="https://github.com/user-attachments/assets/9edc348d-a7eb-42f6-9a1f-8363e644cdb5" />

### **3.2 The "Death Map" Solves the Mystery (N=3, T1 Noise)**

Simulating decoherence under pure Amplitude Damping (T1) revealed the dynamic solution:

<img width="718" height="743" alt="decoherencia" src="https://github.com/user-attachments/assets/d9034cca-2e08-4f15-851c-c0ba3dacf8b8" />

**Key Observations (corroborated by the text log):**
* **Dynamic Separation:** While W (blue) and Dicke (green) **start at the same point ('X')**, their trajectories diverge *immediately*, following clearly distinct paths towards the final state.
* **Phase Symmetry:** GHZ (red) and Impostor (black) trajectories start at opposite poles of the $E_X$ axis and trace mirror-symmetric paths.
* **Common Death (T1):** All trajectories converge to the **same final point** (Cyan Star), the fully relaxed state $|000\rangle$, as expected for pure T1 noise ($H_Z=0, E_X=0, H_q=0$).

**Discovery:** The **dynamic trajectory**, the "way of dying," **unambiguously distinguishes** the initially identical W and Dicke(k=2) states for N=3. ✅

### **3.3 Forensic Analysis: Identifying the "Cause of Death" (N=3, T1 vs T2)**

We compared how key metrics evolve under T1 (energy loss) versus T2 (phase loss) noise, revealing unique statistical signatures for each noise type.

<img width="1390" height="1787" alt="muerte" src="https://github.com/user-attachments/assets/8da4ad2a-d311-490b-9cfc-8a1553ecdbda" />

**Key Observations:**
* **Fidelity Decay:** The decay profile of Fidelity ($F(t) = \langle \psi_{pure} | \rho(t) | \psi_{pure} \rangle$) strongly depends on both the initial state AND the noise type. Notably, W and Dicke states exhibit a surprising "revival" (F $\to 1$) under pure T2 (Phase Flip) noise, while GHZ/Impostor decay fully (F $\to 0$). Under T1, GHZ/Impostor decay only to F=0.5, while W/Dicke decay fully to F=0.
* **Entanglement ($H_q$) Decay:** $H_q$ decays towards 0 under T1 noise (with state-specific curve shapes - Dicke even shows an initial increase!), but remains **constant** under pure T2 noise, indicating that phase errors alone do not destroy entanglement as measured by $H_q$.
* **Phase Correlation ($E_X$) Decay:** $E_X$ decays towards 0 under T1 noise, but is **inverted** ($+1 \leftrightarrow -1$) under pure T2 noise for GHZ/Impostor states, confirming its sensitivity to phase coherence.

**Discovery:** The **dominant noise type** leaves a **characteristic temporal signature** on the evolution of $F$, $H_q$, and $E_X$. 🕵️‍♂️

### **3.4 Final Robustness Test (N=5, Mixed Noise 80/20)**

We scaled the system to $N=5$ qubits and applied a more realistic mixed noise channel (80% T1 + 20% T2) to test the method's robustness.

<img width="819" height="739" alt="robustez" src="https://github.com/user-attachments/assets/b0ad5d40-78fb-4e07-a218-0bbec7fa255c" />

**Key Observations:**
* **Separation Maintained:** The **four trajectories remain clearly distinguishable** in the 3D map, despite the increased system size and complex noise.
* **Distinct Start Points (N=5):** Unlike N=3, for N=5, all four pure states already possess distinct static fingerprints.
* **Informative Endpoints:** The trajectories **do not converge** to the pure T1 death point ($|0...0\rangle$). Instead, they end at **different mixed states**, indicating that the final decohered state under mixed noise still retains information about the initial state. The endpoint's location depends on the T1/T2 mixture.

**Main Discovery:** The "Decoherence Death Map" method is **robust, scalable (to N=5), and generalizable** to mixed noise. The entire trajectory, including the endpoint, serves as an information-rich fingerprint. 💪

---

## **4. Discussion: The Diagnostic Power of Quantum Dynamics**

Our results strongly suggest that **decoherence dynamics offer a far richer source of information** for state characterization than static properties alone. The specific way a quantum state loses its coherence ("dies") is fundamentally linked to its entanglement structure and the nature of its interaction with the environment.

* We resolved the **static degeneracy problem** (e.g., W vs Dicke at N=3) by demonstrating the uniqueness of their dynamic trajectories.
* We confirmed the necessity of **phase-sensitive metrics ($E_X$)** for distinguishing states like GHZ and its Impostor.
* We established that the **noise type (T1 vs T2)** leaves a clear **temporal signature** on the evolution of key features ($H_q, E_X$, Fidelity).
* We showed that under **mixed noise**, even the **final decohered state** provides information about the initial state.

The 3D "Decoherence Death Map" ($H_Z, E_X, H_q$) functions as a **quantum diagnostic space**. By experimentally measuring these three features over time, one could potentially:
1.  **Identify** the prepared quantum state by matching its measured trajectory to pre-calculated canonical paths (red, blue, green, black).
2.  **Diagnose** the dominant noise channel(s) by analyzing the *shape* of the trajectory and its *endpoint*.
3.  **Estimate** the remaining quantum coherence or lifetime of the state based on its current position along its canonical path.

---

## **5. Conclusion**

In conclusion, we have developed and computationally validated a **robust dynamic method for the classification and diagnosis of entangled quantum states**. By utilizing a minimal set of physically meaningful metrics ($H_Z, E_X, H_q$) and tracking their evolution under simulated decoherence, we constructed a "Decoherence Death Map" that reveals unique fingerprints for different entanglement families (GHZ, W, Dicke) and noise types (T1, T2, Mixed). This dynamic approach overcomes the limitations of static methods and offers a potentially valuable, resource-efficient tool for **real-time quantum state characterization, verification, and validation (QCVV)** in the NISQ (Noisy Intermediate-Scale Quantum) era and beyond. 🚀

---

## **6. Code Repository**

The Python (NumPy/Matplotlib) code used to generate these simulations and plots is available in this repository:
[*](https://github.com/geromedina-sys/Quantum-State-Decoherence-Map/blob/main/Codes.ipynb)
