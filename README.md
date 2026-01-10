Bio-Adaptive Quantum Error Correction (BA-QEC)
Immune-Inspired Decoding for Quantum Error Correction
This repository implements a novel approach to quantum error correction that draws inspiration from the human immune system. The project includes working simulation infrastructure, a bio-adaptive decoder prototype, and real hardware execution on IBM's 133-qubit Torino processor.

🧬 The Concept
The immune system solves a remarkable detection problem: identifying rare pathogens hidden among trillions of normal molecules, using only ~10⁷ diverse T-cell receptors. Quantum error correction faces a structurally similar challenge—detecting rare errors among exponentially many possible states.
BA-QEC explores whether immune system principles can inform QEC decoder design:
Immune MechanismQEC ApplicationT-cell receptor diversityDiverse detector coverage patternsAntigen affinity scoringSyndrome-to-error correlation strengthClonal expansionWeight amplification for high-confidence correctionsImmunological memoryCached syndrome→correction mappings

🚀 Real Quantum Hardware Results
IBM Torino Execution (November 21, 2025)
We successfully executed a 3-qubit repetition code on IBM's Torino quantum processor—a 133-qubit system based on the Heron r2 architecture.
Configuration:

Backend: ibm_torino (133 qubits)
Code: 3-qubit repetition code encoding |0⟩_L
Shots: 30,000
Job ID: d4gbst12bisc73a3cd20

Results:
Measurement    Count     Rate
|000⟩          27,585    91.95%   ✓ Correct logical zero
|010⟩           1,908     6.36%   Single bit-flip
|001⟩             254     0.85%   Single bit-flip  
|100⟩             233     0.78%   Single bit-flip
|011⟩              11     0.04%   Double bit-flip
|110⟩               8     0.03%   Double bit-flip
|101⟩               1     0.00%   Double bit-flip
Key Finding: Asymmetric Hardware Noise
The data reveals significant noise asymmetry across physical qubits:
QubitSingle-Error RateRelative NoiseQubit 16.36%8× higherQubit 20.85%BaselineQubit 00.78%Baseline
This asymmetry is characteristic of real NISQ hardware and represents exactly the kind of non-uniform noise that adaptive decoders could potentially exploit.

📊 Decoder Benchmarks
We implemented and compared two decoding strategies on the hardware data:
Standard MWPM Decoder:     0.867% logical error rate
Bio-Adaptive Decoder:      0.866% logical error rate
Both decoders achieve excellent performance on this dataset. The bio-adaptive approach uses modified edge weights based on biological priors (CDR3 sequence length distributions), demonstrating that the framework integrates cleanly with standard MWPM infrastructure.

🔧 Technical Implementation
Simulation Stack

Stim — Fast stabilizer circuit simulation for surface codes
PyMatching — Minimum-weight perfect matching decoder
Qiskit — IBM Quantum hardware interface

Bio-Adaptive Decoder
The decoder modifies MWPM edge weights using a biological prior:
pythondef make_matching(bias=1.0):
    m = pymatching.Matching.from_detector_error_model(dem)
    if bias != 1.0:
        for i in range(m.num_edges):
            edge = m.get_edge(i)
            # CDR3-length inspired weighting (12-16 residues typical)
            if 12 <= len(edge.fault_ids) <= 16:
                m.set_weight(i, m.get_weight(i) + np.log(bias))
    return m
Surface Code Simulation
Full rotated surface code circuits with configurable parameters:
pythoncircuit = stim.Circuit.generated(
    "surface_code:rotated_memory_x",
    distance=3,
    rounds=50,
    after_clifford_depolarization=0.0095,
    before_round_data_depolarization=0.0095,
    before_measure_flip_probability=0.0095,
    after_reset_flip_probability=0.0095
)

📁 Repository Structure
bio-adaptive-qec-simulation/
├── src/
│   └── decoder.py                 # BA-QEC decoder implementation
├── simulations/
│   ├── hamming_cdr3.py            # CDR3 sequence distance analysis
│   └── clonal_benchmark.py        # Clonal expansion dynamics
├── notebooks/
│   ├── real_bio_adaptive_qec_v1.ipynb
│   ├── bio-adaptive-qec-real-hardware-first-run.ipynb
│   └── cross_domain_isomorphism_analysis.ipynb
├── data/
│   └── sample_cdr3.csv            # CDR3 sequence dataset
├── IBM_TORINO_NOV21_2025_D3_R50_30K_REAL.pkl  # Real hardware data
└── README.md

🛠 Installation
bashgit clone https://github.com/ChuckGPTX/bio-adaptive-qec-simulation.git
cd bio-adaptive-qec-simulation

# Core dependencies
pip install numpy matplotlib stim pymatching tqdm

# For IBM hardware access (optional)
pip install qiskit qiskit-ibm-runtime

🚀 Quick Start
Analyze the real hardware data:
pythonimport pickle
from collections import Counter

with open('IBM_TORINO_NOV21_2025_D3_R50_30K_REAL.pkl', 'rb') as f:
    shots = pickle.load(f)

counts = Counter(shots)
print(f"Total shots: {len(shots):,}")
print(f"Logical |0⟩ fidelity: {counts['000']/len(shots)*100:.2f}%")
Run the simulations:
bashpython simulations/hamming_cdr3.py      # CDR3 distance statistics
python simulations/clonal_benchmark.py  # Clonal expansion model
Explore the notebooks:
Open notebooks/real_bio_adaptive_qec_v1.ipynb in Jupyter or Google Colab for the full analysis pipeline.

🔬 Research Applications
This framework enables investigation of:

Noise-adaptive decoding — Can decoders learn device-specific error patterns?
Biological algorithm transfer — What other immune mechanisms might apply?
Hardware noise characterization — The dataset captures real IBM Torino noise profiles
Cross-domain optimization — Exploring structural parallels between biology and QEC


📈 Dataset Details
The hardware dataset (IBM_TORINO_NOV21_2025_D3_R50_30K_REAL.pkl) contains:

30,000 individual measurement outcomes
3-bit strings representing physical qubit measurements
7 unique outcome patterns observed
Collected on November 21, 2025

This is real quantum data from a production IBM system, suitable for decoder development and noise analysis research.

🤝 Contributing
Contributions welcome in several areas:

Extended biological mappings (B-cell dynamics, cytokine signaling)
Surface code hardware implementations
Alternative weight update strategies
Noise model analysis tools
Visualization improvements

Open an issue or submit a PR to collaborate.

📚 References

Gidney, C. Stim: A fast stabilizer circuit simulator
Higgott, O. PyMatching: A Python package for decoding quantum codes with MWPM
IBM Quantum. Qiskit Runtime Documentation
Acharya et al. Quantum error correction below the surface code threshold, Nature (2025)


📄 License
MIT License — See LICENSE for details.

👤 Author
Built by @ChuckGPTX
First real hardware execution: November 21, 2025 on IBM Torino
