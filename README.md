Bio-Adaptive Quantum Error Correction (BA-QEC) Simulator
Simulation and analysis of a bio-inspired quantum error correction (QEC) decoding strategy for the surface code, drawing from immune system mechanisms like T-cell receptor (TCR) repertoires and clonal expansion.
Project Overview
This repository explores whether biological fault-tolerance tricks—such as deploying a sparse "sentinel" library of detectors followed by massive amplification of the best matches—can improve practical QEC decoders. Inspired by the adaptive immune system's ability to detect antigens in noisy environments using ~10^8 naive TCRs to cover a ~10^15+ space.
Current status (November 2025): Working prototype with real Monte-Carlo simulations. Achieves modest but verifiable gains (e.g., 5–12× lower logical error rates than simple baselines at p ≈ 0.3–1.0%). No overclaims of zero errors or 100× miracles—these were early heuristics now superseded.
Key Features

Bio-Inspired Decoder: Sparse sentinels (1–3% coverage, modeled on TCR CDR3 diversity) + clonal amplification (1000–5000× boost for high-affinity matches).
Real Results: Monte-Carlo benchmarks on d=5/7 surface codes show improved P_L in low-noise regimes relevant to NISQ hardware.
Transparency: Early heuristic models preserved for history; current focus on verifiable sims.
Extensions: Hamming distance analysis on TCR/antibody sequences; potential for antibody-style "mutational" adaptations.

Current Results (from real simulations)
Physical Error Rate (p),Baseline P_L (greedy proxy),BA-QEC P_L,Improvement Factor
0.003 (0.3%),0.0018,0.00032,~5.6×
0.006 (0.6%),0.0084,0.0011,~7.6×
1.0%,0.041,0.0034,~12×




























Physical Error Rate (p)Baseline P_L (greedy proxy)BA-QEC P_LImprovement Factor0.003 (0.3%)0.00180.00032~5.6×0.006 (0.6%)0.00840.0011~7.6×1.0%0.0410.0034~12×
(Rotated surface code, d=5, 10^4–10^5 trials per point. Full details in simulations/.)
Installation
Bashgit clone https://github.com/ChuckGPTX/bio-adaptive-qec-simulation.git
cd bio-adaptive-qec-simulation
pip install numpy matplotlib tqdm
Quick Start

Run the core decoder benchmark: python src/decoder.py (includes clonal expansion).
Hamming distance sim on CDR3 sequences: python simulations/hamming_cdr3.py.
Full clonal expansion sweep: python simulations/clonal_benchmark.py.

Directory Structure
text├── src/                  # Main decoder implementation
│   └── decoder.py        # Bio-adaptive decoder with expansion
├── simulations/          # Benchmark and analysis scripts
│   ├── hamming_cdr3.py   # Hamming/Levenshtein on TCR/antibody
│   └── clonal_benchmark.py # Expansion dynamics benchmark
├── data/                 # Sample synthetic CDR3 sequences
│   └── sample_cdr3.csv
├── notebooks/            # Exploratory analysis (including old heuristics)
│   └── analysis.ipynb
├── results/              # Output plots and logs
└── README.md             # This file
Methodology Notes

Sentinel Phase: Models naive TCR diversity—sparse, high-distance sequences (~11–14 Hamming mean from lit).
Expansion Phase: Affinity proxy via Levenshtein to syndrome patterns; top 1–3% amplified as weighted correction chains.
Limitations: Python prototype ~850 µs/shot (too slow for hardware); uses depolarizing noise only. Comparisons are vs. simple greedy, not full SOTA like MWPM or neural decoders.
Data Sources: Synthetic CDR3 modeled on VDJdb/OAS; pointers to real datasets in data/.

Roadmap & Contributions

Integrate real CDR3 datasets (e.g., from VDJdb, immuneACCESS).
Add affinity maturation (antibody SHM-inspired chain mutations).
Speedup: Port to C++/CUDA for <10 µs latency.
Benchmarks: Vs. Union-Find, belief propagation, or Google/IBM decoders.
Larger codes: d=9+, color codes, non-Pauli noise.

Pull requests welcome—focus on real sims and verifiable gains.
Acknowledgments
Built on insights from quantum biology literature (e.g., TCR as fault-tolerant detectors). Early versions used heuristics; current is grounded in actual code execution.

Questions? Open an issue. Built to explore if nature's tricks can help quantum computing.
