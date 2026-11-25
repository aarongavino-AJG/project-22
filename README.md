# Solvent Dependence of the HOMO–LUMO Gap and Aqueous Electron-Donating and Electron-Accepting Sites of (2*S*)-Hesperetin

Aaron Joseph P. Gavino · Principles of Quantum Chemistry Laboratory · LAB2-JL

## Project details

| Item | Value |
|---|---|
| Molecule | (2S)-Hesperetin, ID 22, Tier 2, C₁₆H₁₄O₆ |
| Series | Series I, C2=C3 saturation. Eriodictyol is the closest analogue and has a hydroxyl group in place of hesperetin’s 4′-methoxy group. |
| Isomeric SMILES used for embedding | `COC1=C(C=C(C=C1)[C@@H]2CC(=O)C3=C(C=C(C=C3O2)O)O)O` |
| Conformer search | RDKit 2026.03.6, ETKDGv3, random seed 232829, MMFF94 |
| Electronic structure | ORCA output header: `Program Version 6.1.0`. B3LYP/6-31+G(d,p). |
| Phase 1 batch submission date | N/A. Optimization, frequencies, and aqueous single points ran locally. |
| Phase 2 batch submission date | N/A. The gas-phase neutral single point ran locally. |

The water-optimized conformer had 102 positive internal vibrational frequencies, the lowest at 19.45 cm⁻¹. At the same geometry, removing CPCM water widened the HOMO–LUMO gap from 4.15 to 4.35 eV. In water, the largest electron-removal index was on the 3′-hydroxyl oxygen O21 (0.126), and the largest electron-addition index was on carbonyl carbon C10 (0.193). Atom labels use zero-based ORCA indices.

## Repository contents

| Path | Contents |
|---|---|
| `requirements.txt` | Pinned Python dependencies for the notebooks |
| `step0_conformers/` | `hesperetin_conformer_search.ipynb`, `hesperetin_ensemble.csv`, `hesperetin_survivors.csv`, and the MMFF94 starting geometry `conf01.xyz` |
| `cycle1_opt_freq/` | Input, output, and final optimized XYZ for the aqueous neutral optimization and frequency calculation |
| `cycle2_fukui/neutral/` | Aqueous neutral single-point input and output |
| `cycle2_fukui/cation/` | Aqueous radical-cation single-point input and output |
| `cycle2_fukui/anion/` | Aqueous radical-anion single-point input and output |
| `cycle2_extension/` | Gas-phase neutral single-point input and output at the water-optimized geometry |
| `analysis/` | `hesperetin_descriptors.ipynb` and `dataset_row_22_gavino_aaron.csv` |
| `analysis/figures/` | Numbered structure and condensed Fukui maps as publication PDFs |
| `paper/` | `final_project_22_gavino_aaron.pdf` |

## Reproduction from the SMILES

Run the conformer notebook in `step0_conformers/` using the SMILES and seed above. It embeds 50 conformers and minimizes them with MMFF94. The 0.30 Å RMSD filter yields 11 distinct minima, of which five lie within 12.6 kJ mol⁻¹ of the lowest. The lowest-energy conformer, RDKit ID 31 (`conf01`), supplies the starting geometry for the aqueous ORCA optimization and harmonic frequency calculation. Use the final optimized coordinates for the aqueous neutral, cation, and anion single points and for the gas-phase neutral extension. The aqueous jobs use CPCM water. The gas-phase single point uses the same functional and basis set with the continuum removed. To reproduce the reported analysis from the archived calculations, execute the descriptor notebook with the commands below.

### Clean-clone check on Windows

Clone this repository to a fresh directory and open PowerShell at its root. The analysis notebook reads one final `.out` from each of the five calculation folders. It also reads both conformer CSVs and the saved clustering output in `hesperetin_conformer_search.ipynb`.

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyter nbconvert --to notebook --execute analysis/hesperetin_descriptors.ipynb --output-dir "$env:TEMP"
Get-Content analysis/dataset_row_22_gavino_aaron.csv
```

The executed notebook copy goes to the temporary directory. The notebook writes the dataset CSV to `analysis/` and records the short Git hash available at execution. To regenerate the conformer CSVs first, run the conformer notebook including its CSV export cells and save it with the clustering output visible. The descriptor notebook uses that output to read the number of distinct minima.

## Sources of the paper tables and figures

Here `*.out` means the single final ORCA output in the specified folder. The first code cell of the descriptor notebook prints the filenames it actually reads.

| Paper item | Source and calculation |
|---|---|
| Table 1. MMFF94 survivors | `step0_conformers/hesperetin_survivors.csv`. The relative energy is the difference from conformer 31 in kcal/mol multiplied by 4.184. |
| Table 2. Electronic energies and SCF cycles | Final energy and convergence records in `cycle1_opt_freq/*.out`, `cycle2_fukui/neutral/*.out`, `cycle2_fukui/cation/*.out`, `cycle2_fukui/anion/*.out`, and `cycle2_extension/*.out`. |
| Table 3. Global descriptors | Frontier orbitals and dipole moments from `cycle2_fukui/neutral/*.out` and `cycle2_extension/*.out`. Changes are gas minus water. |
| Table 4 and Table C1. Fukui indices | Final Löwdin charges in the three `cycle2_fukui/` outputs. For each atom, f⁻ = q(cation) − q(neutral) and f⁺ = q(neutral) − q(anion). Table 4 ranks the sites. Table C1 includes all 36 atoms. |
| Appendix A. Worked calculations | Neutral orbital energies and charged-state energies from the single-point outputs. The Löwdin charge examples use the three aqueous charge states. |
| Figure 1. Numbered structure | `analysis/figures/figure_1_hesperetin_structure.pdf`. The atom indices follow the ORCA atom order. The 1.69 Å H34···O11 distance comes from the final Cartesian coordinates in `cycle1_opt_freq/*.out`. |
| Figure 2. Condensed Fukui maps | `analysis/figures/figure_2_fukui_maps.pdf`. Both panels use the heavy-atom indices and Löwdin charge differences underlying Table C1. |

The figure PDFs are archived artwork. The descriptor notebook checks the H34···O11 distance and the leading Fukui sites against the raw outputs. The report cites the published sources for the other molecules in Series I.
