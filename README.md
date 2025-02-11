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

### **Using Cython Instead of PyInstaller** 🚀  
Cython can compile Python code into C for better performance and obfuscation, but it does **not** bundle dependencies into a single executable like PyInstaller. Instead, it generates compiled shared libraries (`.so` on Linux/macOS, `.pyd` on Windows).  

---

### **1. Install Cython**  
First, install Cython:  
```sh
pip install cython
```

---

### **2. Convert Python to C and Compile**  
#### **Step 1: Create a Python Script (e.g., `app.py`)**
```python
def greet():
    print("Hello from Cython!")

if __name__ == "__main__":
    greet()
```

#### **Step 2: Create a `setup.py` for Cython Compilation**
```python
from setuptools import setup
from Cython.Build import cythonize

setup(
    ext_modules=cythonize("app.py", compiler_directives={"language_level": "3"})
)
```

#### **Step 3: Compile to a Shared Library**
Run the following command:  
```sh
python setup.py build_ext --inplace
```
- This generates `app.c` (C code) and `app.cpython-xx-xx.so/.pyd` (compiled binary).  

---

### **3. Running the Compiled Module**  
Now, **rename** or **delete** `app.py` and use the compiled module:  
```python
import app
app.greet()
```

---

### **4. Creating an Executable Using Cython**  
Instead of generating a `.so`/`.pyd` file, you can turn your Python script into a standalone executable.

#### **Modify `setup.py` for an Executable**
```python
from setuptools import setup
from Cython.Build import cythonize
from setuptools.extension import Extension

ext_modules = [
    Extension("app", ["app.py"], extra_compile_args=["-O2"])
]

setup(
    name="app",
    ext_modules=cythonize(ext_modules, compiler_directives={"language_level": "3"})
)
```

#### **Compile It**
```sh
python setup.py build_ext --inplace
```

Now, your `app` is a compiled binary that can be executed. However, this method **still requires Python to be installed** to run the compiled `.so/.pyd` file.

---

### **5. Completely Standalone Executable (Optional)**
To make a **fully standalone** executable (without needing Python installed), use **Nuitka** instead of Cython:  
```sh
pip install nuitka
nuitka --onefile --standalone app.py
```
This works more like PyInstaller.

---

### **Cython vs PyInstaller**  
| Feature           | Cython 🟡 | PyInstaller 🟢 |
|------------------|----------|--------------|
| Performance Boost | ✅ | ❌ |
| Code Obfuscation | ✅ | ❌ |
| Generates Single `.exe` | ❌ | ✅ |
| Needs Python Installed | ✅ | ❌ |

If you need an **executable**, **PyInstaller or Nuitka** is better. If you just want **obfuscation and performance**, **Cython is good**. 💪
