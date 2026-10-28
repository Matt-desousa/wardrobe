# Decisions

Short record of what was decided and why. Update it as decisions change.

## Decided

1. **Single-label classification, extensible later.** First task is one label per item
   (for example "t-shirt", "jeans"). More attributes (sleeve length, pattern, season) come
   later as new heads, without retraining the core model.
2. **Frozen pretrained backbone plus small swappable heads.** MobileNetV3 (or V2) pretrained
   on ImageNet stays frozen; EfficientNet-B0 is a comparison candidate. Heads train on stored
   embeddings, so new capabilities never touch the backbone. Embeddings are stored per item
   to allow new heads, similar-item search and duplicate detection without re-running images.
   Backbone fine-tuning only if real closet photos show clearly insufficient accuracy; that
   would force regenerating all stored embeddings, so the tradeoff must be flagged first.
3. **On-device inference in the browser.** Backbone and head exported to ONNX, run with
   ONNX Runtime Web. Photos do not leave the device in the MVP.
4. **PWA, frontend only.** Static hosting on GitHub Pages, no backend required. Codespaces
   is a dev environment only, never production. Camera access needs HTTPS, which both give.
5. **Training data.** Mainly the developer's own photos, supplemented by open datasets for
   rare classes (check licenses; some are research-only). Public sets are studio or street
   shots and are not the main source.
6. **Frontend tooling: Vite + React + TypeScript.** Chosen by the developer for the UI that
   will follow the proof of concept.
7. **First embedding notebook uses a torchvision Fashion-MNIST sample.** Permissive and easy,
   but low-res grayscale, so it only checks the plumbing and cannot pick the backbone. The
   real comparison reruns on closet photos.

## Data and labeling rules

- Store the finest label available per photo, even if the first model trains on a coarser class.
- Keep original photos. Standardize the photo setup early.
- Aim for about 50 or more images per class, with augmentation.
- Hold out a test set of real closet photos, never used for training. Report per-class
  precision and recall.

## Open (propose options and ask before deciding)

- Label taxonomy: flat list or hierarchy. Most important, discuss next.
- Image preprocessing: background removal or garment cropping.
- Color and pattern handling: classical CV versus ML.
- Low-confidence flow and whether user corrections become training data.
- Local storage details: IndexedDB schema, export and import.
- Model versioning and shipping updated models to the client.
- Later features: outfit recommendations, similarity search, wear history, sync and accounts.
