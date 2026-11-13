# Neural Network Activities

An interactive, browser-based set of hands-on activities for a neural networks course (ISTA 457 / INFO 557). Each activity turns a specific concept from the course into a small, guided exercise — fill in a computation, drag a slider, wire up a graph, answer a check-for-understanding question — with live feedback instead of a static worksheet.

## Purpose

Lecture and textbook material on things like backprop, regularization, or attention can be hard to internalize from equations alone. These activities let a student:

- Work through a concrete numeric example (e.g. compute an actual gradient table by hand, then check it)
- Manipulate a parameter and watch the consequence immediately (e.g. drag a stop-epoch slider and watch train/val loss curves)
- Get instant right/wrong feedback with a hint, instead of finding out at grading time
- Get a practice-completion badge in the open page; it resets on reload and does not send progress or grades to D2L

## The activities

38 activities, grouped by course chapter:

**Prerequisite**
- 0.0 · Neuron Explorer — weighted sum + ReLU

**Chapter 6 — Feedforward Networks & Training**
- 6.1 · Bias Absorption — merge W and c into one matrix multiply
- 6.2.1 · Cross-Entropy — J(W) matrix view
- 6.2.2 · Sigmoid CE Cost — cost curve explorer
- 6.2.3 · Sigmoid vs Softmax — independent vs. competing outputs
- 6.3 · Maxout Units — simulate ReLU and \|x\|
- 6.4 · Linear Regions — counting regions of a deep ReLU network
- 6.5.1 · Computational Graph — build a GRU-like DAG
- 6.5.2 · Backprop Table — `build_grad` step-by-step

**Chapter 7 — Regularization**
- 7.1 · Regularization Race — L1 vs L2
- 7.8 · Early Stopping — when to stop training
- 7.12 · Input Dropout — beyond hidden layers
- 7.KD · KD Regularizer — taxonomy-aware regularization

**Chapter 8 — Optimization**
- 8.1 · Surrogate Loss — when the true loss isn't trainable
- 8.2 · Gradient Norm — reading the loss landscape
- 8.3 · Momentum SGD — velocity accumulation
- 8.4 · Transfer Learning — reusing pre-trained features
- 8.5 · Optimizer Race — SGD vs AdaGrad vs Adam
- 8.7.1 · Batch Norm — controlling column means

**Chapter 9 — Convolutional Networks**
- 9.1 · Convolution — slide a pattern detector over an image
- 9.3 · Adaptive Pooling — fixed output size for any input
- 9.GC.1 · GCN Mechanics — graph, adjacency matrix, message passing
- 9.GC.2 · Conv vs GCN — can graph convolution replace image convolution?

**Chapter 10 — Sequence Models**
- 10.1.1 · Conv Sentiment — window size & negation
- 10.1.2 · RNN Forward — step-by-step h(t) trace
- 10.2.1 · RNN Designer — word to sequence
- 10.2.2 · Tanh Saturation — vanishing gradients in BPTT
- 10.3 · Bi-RNN Equations — forward, backward & output
- 10.4 · NER as Seq2Seq — token classification vs. generation
- 10.6 · Recursive NNs — beyond language trees
- 10.7 · Linear RNN Stability — unrolling, w vs u
- 10.10 · GRU vs LSTM — gates as learned memory
- 10.TF · Transformer Positional Encoding — beyond sinusoids

**Chapter 11 — Evaluation & Interpretability**
- 11.1 · Cancer Registrar NLP — metrics for high-stakes classification
- 11.3 · Learning Curves — bias vs. variance diagnosis
- 11.4 · Hyperband — bracket search budget
- 11.LS · LIME for EEG — neighborhoods in time-series
- 11.GA · Gradient Attribution — saliency, occlusion & Shapley values

On GitHub Pages, each activity is its own page: `docs/activities/<number>.html`, e.g. `docs/activities/6.4.html` opens Linear Regions directly. See [Publishing to GitHub Pages](#publishing-to-github-pages) below.


## Published INFO 557 activities

- [Weekly learning route](https://info-557-az-online.github.io/neural-network-activities/course.html): 13 selected activities for the 7.5-week course.
- [All 38 activities](https://info-557-az-online.github.io/neural-network-activities/).
- [Example: early stopping](https://info-557-az-online.github.io/neural-network-activities/activities/7.8.html).

The weekly route is optional and ungraded. Choose one short practice check within
the existing study time; no screenshot, extra experiment or submission is needed.
D2L remains authoritative for assessment instructions and submissions. Week 7.5
adds no new activity. Edit `course/weekly-activities.json` to maintain the route.

This deployment loads no analytics, requests no student identifier and sends no
activity responses or progress to a collection service. Badges are practice
feedback in the current page, not a persistent instructor dashboard. See
[privacy and progress](ANALYTICS.md) for hosting and math-renderer dependencies.

## Running locally

**Recommended static practice (Python 3, no packages):**

```sh
python3 -m http.server 8001 --bind 127.0.0.1 --directory docs
```

Open <http://127.0.0.1:8001/>. This is the same backend-free site used by Pages.

**Legacy development backend:** Python 3. It includes a CSV submission endpoint;
it is not part of Pages and is not configured as a student-data service.

**Windows (PowerShell):**
```
python -m venv myenv
.\myenv\Scripts\Activate.ps1
pip install -r backend/requirements.txt
.\start.bat
```
(PowerShell requires the `.\` prefix to run a script from the current directory.)

**Linux / macOS:**
```
python3 -m venv myenv
source myenv/bin/activate
pip install -r backend/requirements.txt
cd backend && python -m uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Open **http://localhost:8000** in a browser — this shows the activity list. Click a card, or go directly to `http://localhost:8000/app#<activity-number>` (e.g. `#7.1`).

## Editing and checking activities

The maintained student app is `backend/static/index.html`. GitHub Pages files in
`docs/` are generated from it; edit the shared source, then rebuild:

```sh
python3 scripts/build_pages.py
python3 scripts/check_pages.py
node --test tests/activities.test.cjs
```

The calculation and regression checks require Node.js 18 or newer and no npm
packages. `check_pages.py` checks generated files in a temporary directory and
reports any difference from the committed site.

For the browser checks, serve the generated site:

```sh
python3 -m http.server 8001 --bind 127.0.0.1 --directory docs
```

In another terminal, with Playwright CLI installed:

```sh
python3 scripts/check_browser.py --url http://127.0.0.1:8001/
```

Use `--cli PATH` for a CLI wrapper, and `--session NAME --reuse` to reuse an
already open browser session. Results are saved under `output/playwright/`.
The browser checks use the activity handlers and answer keys to exercise a
successful path through all 38 activities; the separate calculation checks
provide independent numerical checks for selected examples. These checks do
not replace instructor review of instructional goals or student usability.

The `frontend/` directory is an earlier three-module React prototype. It is not
served by the documented Python app or used to build the 38 GitHub Pages activities.

## Publishing to GitHub Pages

Run the build and checks above, and commit both the shared source and regenerated
`docs/` files. Configure GitHub Pages to publish the `docs/` folder on `main`.
An individual activity is then available at `activities/<id>.html` below the site URL.


The course repository publishes from **main /docs**. `.nojekyll` preserves the
static output. The `Validate activities` workflow checks generated-source parity,
the weekly route, privacy boundaries, and numerical regressions on pushes and PRs.
Pages deployment is managed by GitHub's branch publishing workflow. Check the
Pages build/deployment result before claiming a new version is live.
