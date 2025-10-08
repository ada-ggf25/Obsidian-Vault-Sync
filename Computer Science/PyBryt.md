#computer_science #python 

PyBryt is an open-source Python **auto-assessment** library used in teaching to check students’ code and give targeted feedback. In short, it lets instructors define “reference solutions” and behavioral checkpoints; student notebooks are then analyzed against those references to verify results, intermediate values, and reasoning steps—not just final outputs. ([microsoft.github.io](https://microsoft.github.io/pybryt/?utm_source=chatgpt.com))

### What it’s for

- Auto-grading programming/Computational Thinking assignments (e.g., in Jupyter).
- Providing **rich, formative feedback** (e.g., “you computed the right intermediate array but never normalized it”), not only pass/fail tests. ([arXiv](https://arxiv.org/abs/2112.02144?utm_source=chatgpt.com))

### How it works (nutshell)

- Instructors create a **reference notebook** with annotated values/checkpoints.
- PyBryt runs a student’s code, **collects values**, and compares them to the references with tolerance rules and patterns; it can also check time/space complexity characteristics.
- Works alongside other graders like **Otter Grader, OKPy, or Autolab**. ([GitHub](https://github.com/microsoft/pybryt?utm_source=chatgpt.com))

### Learn more / docs

- Official docs & examples. ([microsoft.github.io](https://microsoft.github.io/pybryt/?utm_source=chatgpt.com))
- GitHub repo. ([GitHub](https://github.com/microsoft/pybryt?utm_source=chatgpt.com))
- Microsoft Learn intro & advanced modules. ([Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/introduction-pybryt/?utm_source=chatgpt.com))

If you tell me your use case (course size, LMS, JupyterHub/Colab, etc.), I can sketch a minimal setup and example checkpoints.