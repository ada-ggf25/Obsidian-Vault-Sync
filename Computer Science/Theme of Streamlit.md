#computer_science #deploy #frontend 

## Summary

Streamlit’s theming system lets you fully customize the look and feel of your app via a simple `[theme]` block in `.streamlit/config.toml` [Streamlit Docs](https://docs.streamlit.io/develop/concepts/configuration/theming?utm_source=chatgpt.com). You can adjust accent colors (`primaryColor`), background shades (`backgroundColor` and `secondaryBackgroundColor`), text contrast (`textColor`), and even switch fonts (`font`) for an extra stylistic touch [Streamlit](https://blog.streamlit.io/introducing-theming/?utm_source=chatgpt.com). Community favorites—like the popular Dracula palette—demonstrate how deep purples and vibrant accents create a striking aesthetic [Dracula Theme](https://draculatheme.com/streamlit), and the official “Solarized Dark” showcase offers another inspiring example [GitHub](https://github.com/streamlit/theming-showcase-dark/blob/main/streamlit_app.py?utm_source=chatgpt.com). Below is a carefully curated dark theme combining these best practices for a truly elegant, modern look [Welcome to Kanaries Docs – Kanaries](https://docs.kanaries.net/topics/Streamlit/streamlit-theming?utm_source=chatgpt.com).

## Recommended Theme Config

Add the following to your `.streamlit/config.toml`:

```toml
toml
CopyEdit
[theme]
base                  = "dark"
primaryColor          = "#FF6E6C"
backgroundColor       = "#191724"
secondaryBackgroundColor = "#26233A"
textColor             = "#E0DEF4"
font                  = "serif"

```

## Theme Options Explanation

- **`base = "dark"`**
    
    Enforces dark mode on first load, preventing fallback to Streamlit’s light theme [Streamlit](https://discuss.streamlit.io/t/set-default-theme-on-load/13397?utm_source=chatgpt.com).
    
- **`primaryColor = "#FF6E6C"`**
    
    A warm coral accent that highlights interactive elements without harsh contrast [GitHub](https://github.com/dataprofessor/streamlit-custom-theme?utm_source=chatgpt.com).
    
- **`backgroundColor = "#191724"`**
    
    A deep “twilight” canvas that anchors all page content elegantly [Medium](https://medium.com/accredian/make-your-streamlit-web-app-look-better-14355c2db871?utm_source=chatgpt.com).
    
- **`secondaryBackgroundColor = "#26233A"`**
    
    Darker sidebar and widget background inspired by the Dracula theme’s secondary shade [Dracula Theme](https://draculatheme.com/streamlit).
    
- **`textColor = "#E0DEF4"`**
    
    A soft off-white that ensures readability while maintaining a gentle glow effect [Medium](https://medium.com/%40dataprojectswithMJ/customise-your-streamlit-web-app-theme-in-5-steps-bdc5b99314b1?utm_source=chatgpt.com).
    
- **`font = "serif"`**
    
    Switches to a serif typeface for headings and body text, adding a refined, classic feel [Streamlit](https://blog.streamlit.io/introducing-theming/?utm_source=chatgpt.com).
    

## Additional Tips

- **Clear cached theme settings**: If changes don’t appear immediately, remove the stored theme from your browser’s `localStorage` (e.g., `localStorage.removeItem('streamlit.internal.themeSettings')`) or re-select “Custom Theme” in the app menu [Stack Overflow](https://stackoverflow.com/questions/72818247/streamlit-config-toml-file-not-changing-the-theme-of-the-web-app?utm_source=chatgpt.com).
- **Combine with minimal menu**: To fully lock in your dark theme, hide the Streamlit hamburger menu via CSS or use `toolbarMode = "minimal"` in `[client]` [DEV Community](https://dev.to/buildandcodewithraman/how-to-configure-themes-in-streamlit-4bb8?utm_source=chatgpt.com).

With this configuration, your Streamlit app will load in a cohesive, high-contrast dark mode that feels both modern and aesthetically pleasing.

Sources

[](https://www.google.com/s2/favicons?domain=https://docs.kanaries.net&sz=32)

[](https://www.google.com/s2/favicons?domain=https://github.com&sz=32)

[](https://www.google.com/s2/favicons?domain=https://draculatheme.com&sz=32)

[](https://www.google.com/s2/favicons?domain=https://blog.streamlit.io&sz=32)

[](https://www.google.com/s2/favicons?domain=https://docs.streamlit.io&sz=32)