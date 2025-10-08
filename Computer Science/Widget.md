#computer_science #frontend 

A **widget** is a little interactive UI control—like a slider, button, checkbox, or dropdown—that you can use to view or change values.

In **Jupyter notebooks**, “widgets” usually mean **ipywidgets**:

- They render UI elements right in a cell (slider, text box, toggle, etc.).
- When you interact with them, they **send events to Python** in the kernel.
- You can **bind** them to functions to update figures, recompute results, or filter data live.

Tiny example (conceptually):

```python
import ipywidgets as w
import matplotlib.pyplot as plt
import numpy as np

slider = w.IntSlider(min=1, max=10, value=3, description="freq")

def plot(freq):
    x = np.linspace(0, 2*np.pi, 400)
    y = np.sin(freq * x)
    plt.figure()
    plt.plot(x, y)
    plt.show()

w.interact(plot, freq=slider)   # moving the slider re-runs plot(freq)

```

So: a widget = an on-page control; in Jupyter it’s a bridge between your **UI** and **Python code**, great for demos, teaching, and quick interactive exploration.