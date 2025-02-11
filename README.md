# python_installer

### **Installing and Using PyInstaller**  

PyInstaller is a tool that packages Python scripts into standalone executables. Here's how you can install and use it.

---

### **1. Install PyInstaller**  
Make sure you have Python installed, then run:  
```sh
pip install pyinstaller
```

---

### **2. Create an Executable from a Python Script**  
#### **Basic Usage:**  
```sh
pyinstaller --onefile app.py
```
- `--onefile`: Packages everything into a single executable file.  

After running this, PyInstaller will create a `dist/` folder where your executable (`app.exe` on Windows or `app` on Linux/macOS) is located.

---

### **3. More Secure Options**  
```sh
pyinstaller --onefile --noconsole --hidden-import=secret_module app.py
```
- `--noconsole`: Hides the terminal window (useful for GUI apps).  
- `--hidden-import=secret_module`: Ensures hidden dependencies are included.  

If your app requires other hidden imports, you can add them like this:  
```sh
pyinstaller --onefile --noconsole --hidden-import=module1 --hidden-import=module2 app.py
```

---

### **4. Running the Executable**  
Once built, go to the `dist/` folder and run the generated file:  
```sh
cd dist
./app  # Linux/macOS
app.exe  # Windows
```

---

### **5. Extra Tips**  
- If your app uses external files, use `--add-data` to include them:  
  ```sh
  pyinstaller --onefile --add-data "config.json;." app.py
  ```
- If your app has a GUI (like Tkinter), use `--windowed` instead of `--noconsole`:  
  ```sh
  pyinstaller --onefile --windowed app.py
  ```
- To speed up builds, use:  
  ```sh
  pyinstaller --clean --onefile app.py
  ```

---

That's it! You now have a standalone executable from your Python script. 🚀
