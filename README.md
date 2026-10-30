# The Little Hands of Antwerp
## Unsupervised Writer Identification in a Visual Corpus of Vernacular Aldermen Deeds (ca. 1300-1350)

This repository holds the code supporting a scholarly publication, accepted as a long oral presentation at [CHR2027](https://2027.computational-humanities-research.org/) (Computational Humanities Research, Manchester) and to appear in the *Anthology of Computers and the Humanities*:

> Mike Kestemont, Jan Burgers, Bram Caers, Godfried Croenen, Els De Paermentier, Jeroen Deploige, Gernot A. Fink, Tim Raven, Remco Sleiderink, Caroline Vandyck, Malamatenia Vlachou Efstathiou, Georg Vogeler and Robin Waeytens. "The Little Hands of Antwerp. Unsupervised Writer Identification in a Visual Corpus of Vernacular Aldermen Deeds (ca. 1300-1350)." In *Proceedings of the Conference on Computational Humanities Research 2027 (CHR 2027)*, Manchester.

### Authors

| Author | Affiliation | ORCID |
| --- | --- | --- |
| Mike Kestemont | University of Antwerp | [0000-0003-3590-693X](https://orcid.org/0000-0003-3590-693X) |
| Jan Burgers | Huygens Institute | [0009-0002-5586-4079](https://orcid.org/0009-0002-5586-4079) |
| Bram Caers | Leiden University | [0000-0001-7482-3198](https://orcid.org/0000-0001-7482-3198) |
| Godfried Croenen | University of Antwerp & University of Liverpool | [0000-0001-8581-6727](https://orcid.org/0000-0001-8581-6727) |
| Els De Paermentier | Ghent University | [0000-0002-0070-0897](https://orcid.org/0000-0002-0070-0897) |
| Jeroen Deploige | Ghent University | [0000-0002-0143-7138](https://orcid.org/0000-0002-0143-7138) |
| Gernot A. Fink | TU Dortmund University | [0000-0002-7446-7813](https://orcid.org/0000-0002-7446-7813) |
| Tim Raven | TU Dortmund University | [0009-0002-1528-2744](https://orcid.org/0009-0002-1528-2744) |
| Remco Sleiderink | University of Antwerp | [0000-0003-0284-324X](https://orcid.org/0000-0003-0284-324X) |
| Caroline Vandyck | University of Antwerp & Research Foundation – Flanders (FWO) | [0009-0006-9995-1325](https://orcid.org/0009-0006-9995-1325) |
| Malamatenia Vlachou Efstathiou | IRHT-ENPC, Paris | [0000-0002-9397-356X](https://orcid.org/0000-0002-9397-356X) |
| Georg Vogeler | University of Graz | [0000-0002-1726-1712](https://orcid.org/0000-0002-1726-1712) |
| Robin Waeytens | Ghent University | [0009-0002-7576-178X](https://orcid.org/0009-0002-7576-178X) |

## Model training
The `attmask` folder contains the self-supervised pretraining code in this repository, adapted from Tim Raven's implementation, originally developed for the ICDAR2017 Historical Document Writer Identification (Historical-WI) competition. Only minimal modifications were made to run the code on a new dataset outside the original competition setting, without altering the underlying masking or training methodology. Raven's code [https://github.com/Traven16/SSL_ViT_WR] builds on the official AttMask ([https://github.com/gkakogeorgiou/attmask]) implementation, which introduces the attention-guided masked image modeling strategy. AttMask's codebase is itself built on top of two self-supervised learning frameworks:
- **DINO**: [https://github.com/facebookresearch/dino]
- **iBOT**: [https://github.com/bytedance/ibot]

Other files added:
- `extract_embeddings.py`, which we used to extract VLAD-encoded document embeddings from a trained ViT checkpoint for downstream processing. It processes images patch-wise, clusters patch descriptors with MiniBatchKMeans, and aggregates them into a fixed-length VLAD vector per document.
- `visualize_augmentations.py`, which visualizes the global and local crops produced by `DataAugmentationMorph` (as used during training in `main_attmask.py`), including the erosion/dilation morphological augmentations and Gaussian blur. Overlays a patch-grid on each crop to illustrate the effective patch tokenization at training resolution. Useful for sanity-checking augmentation strength and patch alignment before/after training.

 The folder contains an independent requirements file documenting the server-side environment in which we ran the code. If you use this code, please consider citing the original works below.

### Citations

```bibtex
@InProceedings{kakogeorgiou2022attmask,
  author    = {Kakogeorgiou, Ioannis and Gidaris, Spyros and Psomas, Bill and Avrithis, Yannis and Bursuc, Andrei and Karantzalos, Konstantinos and Komodakis, Nikos},
  title     = {What to Hide from Your Students: Attention-Guided Masked Image Modeling},
  booktitle = {Computer Vision -- ECCV 2022},
  year      = {2022},
  publisher = {Springer Nature Switzerland},
  pages     = {300--318},
  isbn      = {978-3-031-20056-4},
  url       = {https://link.springer.com/chapter/10.1007/978-3-031-20056-4_18},
  doi       = {10.1007/978-3-031-20056-4_18}
}

@inproceedings{caron2021emerging,
  title     = {Emerging Properties in Self-Supervised Vision Transformers},
  author    = {Caron, Mathilde and Touvron, Hugo and Misra, Ishan and J{\'e}gou, Herv{\'e} and Mairal, Julien and Bojanowski, Piotr and Joulin, Armand},
  booktitle = {Proceedings of the International Conference on Computer Vision (ICCV)},
  year      = {2021}
}

@article{zhou2021ibot,
  title   = {iBOT: Image BERT Pre-Training with Online Tokenizer},
  author  = {Zhou, Jinghao and Wei, Chen and Wang, Huiyu and Shen, Wei and Xie, Cihang and Yuille, Alan and Kong, Tao},
  journal = {International Conference on Learning Representations (ICLR)},
  year    = {2022}
}
```

## Repository structure

```
.
├── attmask/     # self-supervised pretraining (see "Model training" above)
├── analysis/    # notebooks reproducing the paper's figures/tables
├── data/        # corpus images, transcriptions, embeddings, clusterings
└── outputs/     # figures and interactive visualizations generated by analysis/
```

## Analysis notebooks

The `analysis` folder contains the notebooks used to produce the figures, tables, and clustering results reported in the paper. They are meant to be run in order, each reading its inputs from `data/` and writing figures to `outputs/`.

- `01-distribution.ipynb` — plots the distribution of deeds over time from the corpus filenames, reporting deeds sharing the same date. Writes `outputs/deeds_frequency.jpg`.
- `02-clustering.ipynb` — the core unsupervised writer-identification pipeline: loads the VLAD document embeddings (`data/embeddings/`), excludes documents listed in `data/exclude.txt`, reduces dimensionality (PCA → UMAP), clusters with HDBSCAN, and renders an interactive scatter plot with document thumbnails. Writes `outputs/thumbnail_cache.json` and `outputs/umap_hdbscan.html`.
- `03-graphetic.ipynb` — compares the paper's HDBSCAN clustering against human annotations and the RAVEN baseline (`data/misc-assets/clusterings.xlsx`), computing agreement statistics (ARI/NMI/Rand index) and temporal ranges per scribal hand. Writes `outputs/cluster_sizes.png`, `outputs/cluster_comp.png`, and the `*-ranges.png` figures.
- `04-linguistic.ipynb` — stylometric/linguistic analysis of the PAGE-XML transcriptions (`data/deeds-page`, extracted to `data/deeds-txt`): TF-IDF features over the (MUFI-glyph-aware) tokenized text, PCA/UMAP projection, and chi²-based distinctive-feature analysis per scribal group. Writes `outputs/stylo.png`.

These notebooks depend on packages not required by `attmask/` (e.g. `pandas`, `scikit-learn`, `scipy`, `umap-learn`, `bokeh`, `plotly`, `seaborn`, `matplotlib`, `Pillow`, `openpyxl`); see `analysis/requirements.txt` for the pinned versions, and install them in a separate, dedicated environment from the one used for model training:

```bash
conda create -n little-hands python=3.10.18
conda run -n little-hands pip install -r analysis/requirements.txt
```

Use a dedicated environment for this repository rather than reusing one shared with other projects. `02-clustering.ipynb`'s UMAP/HDBSCAN pipeline is sensitive to the exact numpy/numba/BLAS build in the environment: a later, unrelated package install (in another project sharing the same environment) shifted numpy's build and silently changed the clustering output, even with `random_state` fixed and the input data unchanged. The clustering reported in the paper is preserved as-is in `data/misc-assets/clusterings.xlsx`; a fresh run of `02-clustering.ipynb` is not guaranteed to reproduce it bit-for-bit.

## Data & model availability

The `data/` folder holds the corpus underlying the analysis:

- `deeds-img/`, `flanders-img/` — page/deed images from the Antwerp and Flanders collections.
- `deeds-page/`, `deeds-txt/` — PAGE-XML transcriptions and their extracted plain-text versions.
- `embeddings/` — VLAD document embeddings extracted from the trained ViT checkpoint.
- `exclude.txt` — IDs of documents excluded from clustering (e.g. damaged/illegible deeds).
- `misc-assets/` — supporting files: the master clustering spreadsheet (`clusterings.xlsx`), human annotations (`labels.csv`), and the trained model checkpoint.

The paleographic-description appendix is tracked in this repository at [`appendix/Appendix_paleographic_descriptions.pdf`](appendix/Appendix_paleographic_descriptions.pdf).

Large files (images, embeddings, model checkpoint, and the rendered `outputs/*.html`) are not tracked in this git repository due to size constraints. They are archived together with this code as a single combined deposit on Zenodo; the DOI will be added here upon publication.

To cite this work, see [`CITATION.cff`](CITATION.cff); Zenodo deposit metadata is in [`.zenodo.json`](.zenodo.json).

## License

This repository and the accompanying Zenodo deposit (code, data, model and appendix) are licensed under [CC BY 4.0](LICENSE), with two exceptions:

- **Images.** The deed images (`data/deeds-img/` and `data/flanders-img/`; `images-deeds.zip` and `images-flanders.zip` in the Zenodo deposit) are faithful reproductions of medieval documents (ca. 1300–1350) that are in the public domain. Under Article 14 of Directive (EU) 2019/790, such reproductions are not subject to copyright. CC BY 4.0 therefore does not apply to them; they are marked with the [Public Domain Mark 1.0](https://creativecommons.org/publicdomain/mark/1.0/).
- **Pretraining code.** The code in `attmask/` is a modified version of Apache-2.0 code (AttMask, DINO, iBOT) and remains under the [Apache License 2.0](attmask/LICENSE); see [`attmask/NOTICE`](attmask/NOTICE).

## The annotation event & acknowledgments

The scribal clustering was validated at a two-day community annotation event held at the University of Antwerp (21–22 April 2026), organized by Godfried Croenen, Mike Kestemont, Remco Sleiderink and Caroline Vandyck, with support from Silke Delcart.

We gratefully acknowledge the medievalists who volunteered as annotators: An Faems, Bram Caers, Chris De Wulf, Elisabeth de Bruijn, Els De Paermentier, Febe Thonissen, Frank Willaert, Georg Vogeler, Jan Burgers, Jeroen Deploige, Korneel Dewulf, Lisa Demets, Matenia Vlachou, Noé Leroy, Remco Sleiderink, Robin Moens, Robin Waeytens, Viorica Van der Roest and Ward Leloup.

The preliminary paleographic descriptions in the appendix were contributed by Jan Burgers, Godfried Croenen, Els De Paermentier and Robin Waeytens. We thank Chris De Wulf and Kamiel Temmerman for data donations, Dominique Stutzmann for literature suggestions, and former city archivist Jos Van den Nieuwenhuizen (1928–2026) and archivist Michel Oosterbosch, whose image collection and metadata the corpus builds on. The event was supported by the Special Research Fund (BOF) of the University of Antwerp; Caroline Vandyck and Mike Kestemont were supported by the Research Foundation – Flanders (FWO).

