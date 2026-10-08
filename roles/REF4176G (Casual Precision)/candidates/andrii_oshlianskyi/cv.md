# Andrii Oshlianskyi — CV (parsed)

**Title:** Senior Machine Learning Engineer — Computer Vision, OCR, NLP
**Location:** Kyiv, Ukraine (Remote)
**Contact:** andronex@gmail.com · +380 66 920 5888 · linkedin: andrii-oshlianskyi
**CV last updated:** January 2026

## Professional Experience

**Machine Learning Engineer / Researcher — Samsung R&D Institute Ukraine** (Kyiv, UA) — *May 2024 – Jan 2026*
- Prepared/processed multilingual handwritten datasets; special handling for CJK scripts; synthetic data generation.
- Trained OCR models for multiple languages/scripts using image and stroke-based (PLT) data.
- Developed language/text-type classifiers with custom architectures (transformer-based, attention-based).
- Built text autocorrection and predictive text models for OCR outputs, leveraging sequence models and LLMs.
- Researched PDF reading order and document structure reconstruction using graph-based approaches (PyTorch Geometric).

**Machine Learning Engineer — Preste** (Paris, FR, Remote) — *Oct 2022 – May 2024*
- End-to-end production computer vision pipelines.
- RGB + thermal (FLIR) image preprocessing/alignment; defect detection (YOLOv8); ROI segmentation (SAM + CLIP).
- X-ray defect localization (two-stage segmentation+classification, and one-stage UNet); classical CV postprocessing.
- OCR pipelines for document processing/extraction/classification; backend integration.
- POC models for tiny-object and fake detection.
- Inference optimization (ONNX, quantization, TensorRT); deployment on NVIDIA Jetson edge devices with Docker.
- Interactive demos via Gradio.

**Machine Learning Engineer / Data Scientist — EJaw** (Kyiv, UA) — *Aug 2022 – Apr 2023*
- End-to-end medical/biological image classification pipelines (cervical and blood cell analysis).
- Multi-stage segmentation/classification; sequence modeling (CRNN, BLSTM, Attention).
- Model explainability/validation (Grad-CAM, saliency maps).
- R&D on drone-based 3D reconstruction, object recognition, autonomous navigation.
- Generative modeling (Stable Diffusion + ControlNet).

**Machine Learning Engineer — DeepX** (London, UK, Remote) — *Oct 2021 – Jul 2022*
- Object detection/tracking for airport security video (YOLOv5 + classical CV tracking).
- Multi-camera pipelines for vending machines — synchronization, low-FPS analysis, distortion correction.
- Pose estimation/event recognition combining deep learning + classical CV.
- Full pipeline development with decision logic, optimization, deployment.

**Research internships:** GSI Helmholtz Centre for Heavy Ion Research (Monte Carlo simulations, 2018); Computational Physics Dept., University of Oslo (numerical simulations, 2018).

## Education

- MSc Physics and Astronomy — Taras Shevchenko National University of Kyiv, 2019–2021 (Nuclear/High Energy/Particle Physics), Diploma with Honour
- BSc Physics — same institution, 2015–2019, Diploma with Honour
- 2 peer-reviewed publications (Astrophysical Journal; Proceedings of Science / ICRC 2021) in neutrino astrophysics

## Skills

- **Languages:** English (C1), Ukrainian (native), Russian (fluent)
- **Programming:** Python, C++, Bash
- **Frameworks/libraries:** PyTorch, Lightning, Transformers, Hydra, Optuna, TensorFlow, Keras, YOLO, SAM, CLIP, GradCAM, NLTK, spaCy, tokenizers, BERT, SentenceTransformers, OpenCV, Albumentations, NumPy, Pandas, SciPy, scikit-image, matplotlib, seaborn, TensorBoard, TorchMetrics, PyTorch Geometric, WandB, MLflow, Poetry
- **MLOps/Deployment:** Docker, Singularity, DVC, ONNX, TensorRT, NVIDIA Jetson, JetPack SDK, Gradio, Flask (base), AWS (base), Azure (base)

## Notable for this role (REF4176G)

- Strong, direct fit: OCR, document structure recognition (PDF reading order reconstruction), layout understanding — very close to the Postlog/Prelog recognition problem space.
- Some LLM exposure (autocorrection/predictive text leveraging LLMs) but not clearly production LLM-pipeline ownership (prompt versioning, evaluation, grounding) at the depth the JD describes.
- AWS listed only as "base" level — JD requires "Strong (3/4)" hands-on with S3/IAM/CloudWatch/orchestration; worth probing directly.
- No SQL explicitly listed anywhere in skills or experience — also worth probing directly, since JD requires "Strong (3/4)."
- Strong Python/ML engineering fundamentals and inference optimization background (ONNX/TensorRT/quantization) — relevant to cost/latency concerns even if not LLM-specific.
