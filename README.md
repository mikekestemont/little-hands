# The Little Hands of Antwerp
## Unsupervised Writer Identification in a Visual Corpus of Vernacular Aldermen Deeds (ca. 1300-1350)

This repository holds the code supporting a scholarly publication, currently under double-blinded peer review for the CHR conference:

> "The Little Hands of Antwerp. Unsupervised Writer Identification in a Visual Corpus of Vernacular Aldermen Deeds (ca. 1300-1350)"

## Model training
The `attmask` folder contains the self-supervised pretraining code in this repository, adapted from Tim Raven's implementation, originally developed for the ICDAR2017 Historical Document Writer Identification (Historical-WI) competition. Only minimal modifications were made to run the code on a new dataset outside the original competition setting, without altering the underlying masking or training methodology. Raven's code builds on the official AttMask ([repo](https://github.com/gkakogeorgiou/attmask)) implementation, which introduces the attention-guided masked image modeling strategy. AttMask's codebase is itself built on top of two self-supervised learning frameworks:
- **DINO**: [https://github.com/facebookresearch/dino]
- **iBOT**: [https://github.com/bytedance/ibot]

The only novel file added is `extract_embeddings.py`, which we used to extract VLAD-encoded document embeddings from a trained ViT checkpoint for downstream processing. Processes images patch-wise, clusters patch descriptors with MiniBatchKMeans, and aggregates them into a fixed-length VLAD vector per document. The folder contains an independent requirements file documenting the server-side environment in which we ran the code. If you use this code, please consider citing the original works below.

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

