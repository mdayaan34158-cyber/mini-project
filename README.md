# Machine Learning-Based Bearing Fault Classification in Induction Motors
## RaceGuard v5.1 — cross-checked native Streamlit release

**Current entry point: `streamlit_app.py`.** Read **docs/USER_MANUAL.pdf** (exactly two pages), then **START_HERE_DEPLOYMENT.md**. This is a native Streamlit app, **not Gradio, a Flask adapter, or an embedded dashboard**. The active runtime requirements contain no Gradio or Flask. Validated numerical modules and trained weights are retained; services are called directly from src/services.py.

## Current cross-check update
This rebuilt release fixes duplicate chart IDs when the current analysis is also displayed from history, strict JSON export of nonfinite unused CSV metadata, and empty-file upload precedence. Full export validation runs before publishing a successful result. Additional UI/history/session and validation regressions are included. Read **docs/RECHECK_REPORT.md** for findings and current test evidence. Selected signal samples are still strictly rejected if nonfinite; they are never silently corrected.

## Run locally — Python 3.13
Extract the ZIP, open the inner `bearing-streamlit` folder, and run RUN_WINDOWS.bat or `sh RUN_MAC_LINUX.sh`. These install dependencies and start the app. Manual alternative:
```sh
python3.13 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m streamlit run streamlit_app.py
```
Windows Command Prompt:
```bat
py -3.13 -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt
.venv\Scripts\python.exe -m streamlit run streamlit_app.py
```
Open **http://localhost:8501/**. Keep the terminal open; Ctrl+C stops the app. Do not run the file with plain `python streamlit_app.py`. Do not follow old Gradio/launch.py instructions inside archive/. Runtime configuration is in .streamlit/config.toml (bind0.0.0.0, port8501, upload20MB, CORS/XSRF protection enabled, anonymous usage telemetry disabled).

## Deploy
Recommended: **Streamlit Community Cloud**. Put extracted project contents in a repository accessible to your Cloud account; choose the repository/branch, **streamlit_app.py** entry point, and Python3.13 in Advanced settings. Keep requirements.txt and .streamlit/config.toml at project root and retain models/, src/, data/ and docs/. No GPU, app secret, model download or separate backend server is needed for bundled examples. Review DATA_RIGHTS.md before publishing data excerpts. Read START_HERE_DEPLOYMENT.md for full steps and an optional container/Hugging Face Docker route. **No Gradio SDK** is used.

Actual remote account publication was not performed here. Verify the real URL assigned by your host; a local URL or Arena preview is not a permanent public deployment. The supplied Dockerfile is prepared but a Docker build was not executed. Windows/macOS instructions are supplied, not host-tested here.

## Workspaces / preserved calculations
### Vibration
- CWRU benchmark examples; CSV preview and explicit signal-column/sample-rate selection; pasted measured samples; synthetic examples and numeric builder; optional actual bearing geometry.
- Model compatibility is explicitly required:48kHz drive-end acceleration in g,4,096-sample windows. Other rates/arbitrary signals use signal mode. No hidden resampling, missing-row removal or imputation.
- Actual waveform/FFT, per-window features, complete all-window formula ledger, source/domain/review notices and optional bearing-frequency markers.
- Synthetic inputs report **Signal analysis complete** with intentional nonclassification—not a fabricated bearing fault or engineering risk.
- Last5 successful runs retained in session, with an explicitly historical viewer. JSON/features CSV/HTML report and pasted/generated waveform downloads. Input edits hide stale current results until rerun; failures remove current outputs/exports.

### RUL & life planning
- Declared accumulated operating age / nominal total life with optional paired min/max service scenarios, hours/days/cycles, warnings and proportional budget consumption.
- Or chronological age,indicator CSV/paste with at least6 complete strictly increasing operating ages and a justified higher-is-worse indicator/reference/limit.
- Centered OLS, threshold crossing, supported conditional crossing interval, R²/residual and holdout diagnostics, observed/fit/limit graph and prefix-fit history. Demo is labelled synthetic. JSON/CSV exports.
- No RUL comes from the classifier/a single RMS value. Budget ranges are assumptions; fitted-crossing intervals are not individual bearing survival/lifetime guarantees. R² is fit quality, not reliability. No real maximum life is fabricated.

### Sound — completed recordings, not live telemetry
- Labelled digital-audio simulation, WAV upload, **native Streamlit microphone recording**, and externally supplied level/duration logs.
- **Record → Stop → Analyse sound.** This release does **not** continuously stream a live microphone meter. That former Gradio path is intentionally not claimed as a Streamlit capability. No fake live fallback.
- Correct RMS dBFS, interval min/max/current, sample-energy/duration-weighted average, sample peak/clipping, fixed display scale, project alerts, proportional meter and trend. Out-of-range numbers remain visible even when plot/meter position clips. Silence is no finite dB, not0dB.
- Default digital scale−90..0 and alerts−20/−6dBFS are illustrative project rules, not occupational/safety limits. External SPL logs remain unverified; default85/100 log alerts also are not standards. User-reference offset requires a note and is never certified SPL. Live weighting/exposure compliance is not invented.
- WAV bounds20MB/300seconds/1–8channels/1–192kHz/10million selected samples; explicit channel0=first. Log schema level_db,duration_sec (positive durations, up to10,000rows/24hours). Exports include raw and displayed conventions.

### Evidence & guide
Unchanged model evaluation, data-rights notice, current PDF and explicit limitations. The narrow same-rig CWRU benchmark is not independent industrial validation. Neither synthetic inputs nor framework conversion create new accuracy/life/calibration evidence.

## Microphone / data privacy
Native Streamlit uploads completed recordings to the server. Browser/device gain, encoding/resampling/preprocessing can alter amplitude. Raw digital **dBFS is not calibrated SPL/dBA/dBC**. A reference offset cannot establish frequency response or certification. No A/C/IEC time weighting, exposure compliance or certified acoustic accuracy. Physical acoustic calibration and real run-to-failure accuracy were not validated.

Permission and HTTPS/localhost are required. Embedded host policy may block a microphone; try a direct app URL or WAV upload. Use Stop before analysis/navigation; close the tab or revoke site permission if needed to fully release hardware. Do not record sensitive audio on public/untrusted hosts. Uploads/results live in server/session memory and downloads; this is not secure persistent telemetry storage. Export before closing. No raw audio is deliberately written to project files by our app.

## Project layout
- streamlit_app.py: complete native Streamlit interface, rerun/session handling and plots/downloads.
- src/services.py: direct framework-independent validation/dispatch/exports (no Flask/Gradio imports).
- src/inference.py, processing.py, feature_extraction.py, formulas.py, inputs.py, rul.py, sound.py: retained numerical kernels; trained model/evaluation unchanged.
- models/, data/manifest.json, data/sample_data/: actual artifacts and reproducible examples.
- scripts/train.py / download_data.py: original offline training/data-acquisition source; training is not a web action. Full raw dataset is not bundled; review provenance/rights.
- .streamlit/config.toml, requirements*.txt, RUN scripts, Dockerfile: current runtime/deployment configuration.
- tests/: current numerical/service/Streamlit AppTest/browser checks. Historical Gradio tests are not part of the active suite.
- docs/: two-page current user guide, current verification evidence and screenshots.
- **archive/previous_v4_source.zip**: complete previous project/source preserved exactly as delivered, including older dashboards/reports/configuration. It is archival only; do not unpack it over this Streamlit root or install its requirements. It is not imported/served by the app and may be omitted from production upload if historical source is unnecessary there.

## Testing and scope
Read **docs/TEST_REPORT.md** for executed results and limitations. Tests include native AppTest sessions and real Chromium workflows, not screenshots alone. Virtual microphone software testing is not calibrated physical hardware validation. We do not promise zero errors for every browser/OS/host; actual failures should display a clear input/computation error, never a healthy-equipment result.
```sh
python -m pip install -r requirements-dev.txt
python -m playwright install --with-deps chromium
python -m pytest -q
python -m streamlit run streamlit_app.py
# Another terminal:
python tests/browser_check.py
```
The browser test accepts RACEGUARD_URL for another test port. Fake-device flags belong to tests only; normal launch never creates a fake microphone. Finish numeric edits with Enter/blur and wait for uploads to finish before Analyse. PDF regeneration: install requirements-docs.txt, then `python scripts/build_user_manual.py`.

SHA256SUMS.txt covers all final packaged regular files except itself. On compatible systems: `sha256sum -c SHA256SUMS.txt` from the extracted root. See PACKAGE_VERIFICATION.md for extraction/run checks.
