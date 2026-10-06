# TECHNO jupyter notebook

Simulated JetBot robot (teleop with ipywidgets + matplotlib) for kids. Runs in
the browser — no login, no install.

## Links for the class

| Mode | Link |
|---|---|
| **App (Voilà, code hidden)** | [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/SirErico/TECHNO_sim/main?urlpath=voila%2Frender%2FTECHNO_symulacja.ipynb) `https://mybinder.org/v2/gh/SirErico/TECHNO_sim/main?urlpath=voila%2Frender%2FTECHNO_symulacja.ipynb` |
| **Notebook (code visible)** | [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/SirErico/TECHNO_sim/main?labpath=TECHNO_symulacja.ipynb) `https://mybinder.org/v2/gh/SirErico/TECHNO_sim/main?labpath=TECHNO_symulacja.ipynb` |

## Teacher notes

* **Pre-warm before class.** Open the link yourself 10–15 minutes before the
  lesson. The first build after a new commit takes 1–5 minutes; after that,
  launches are fast.
* Voilà runs every cell when the page opens, including the demo moves that
  call `time.sleep`, so the page takes about 15–20 s to finish loading.
* Binder sessions **time out after ~10 minutes idle** and nothing is saved.
  If a student's page stops responding, they reopen the link.
* 30 students launching at the same moment may be slow — let them open the
  link in small groups.
* **Offline fallback** (no internet / Binder down): on one laptop on the
  classroom network:

  ```bash
  pip install -r requirements.txt
  jupyter notebook --ip=0.0.0.0          # notebook, code visible
  voila --Voila.ip=0.0.0.0 TECHNO_symulacja.ipynb   # app, code hidden
  ```

  Students open `http://<laptop-ip>:8888` (notebook, use the token printed in
  the terminal) or `http://<laptop-ip>:8866` (Voilà).

## Implementation notes

* The live teleop views refresh from an `asyncio` task rather than a thread.
  Updating widgets from a plain thread raises `LookupError: shell_parent` on
  ipykernel 7.
* Plots are rendered to PNG and pushed into a `widgets.Image`, only when the
  robot pose changes. This avoids the flashing that `Output` +
  `clear_output` causes.
* Don't use `%matplotlib widget` / ipympl here, because it conflicts with
  the redraw loop. JupyterLite isn't supported either, since the robot
  physics runs in background threads.
