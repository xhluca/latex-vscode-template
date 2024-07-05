Instructions by ChatGPT below:

---

### Prerequisites
1. **Install Visual Studio Code**: Download and install VS Code from [here](https://code.visualstudio.com/).
2. **Install Git**: Download and install Git from [here](https://git-scm.com/).
3. **Install LaTeX Distribution**: Install a LaTeX distribution like TeX Live (for Linux/Mac) or MikTeX (for Windows).
4. **Install latexmk**:
   - Open a terminal and run:
     ```sh
     sudo apt install latexmk
     ```

### Step 1: Install Necessary Extensions in VS Code
1. **LaTeX Workshop**: This extension provides rich support for LaTeX in VS Code.
   - Open VS Code.
   - Go to Extensions (`Ctrl+Shift+X`).
   - Search for `LaTeX Workshop` and install it.

2. **GitLens**: This extension enhances Git capabilities in VS Code.
   - In the Extensions pane, search for `GitLens` and install it.

3. **Live Share** (Optional): For real-time collaboration.
   - In the Extensions pane, search for `Live Share` and install it.

### Step 2: Clone Your Repository
1. **Open VS Code**.
2. **Open Terminal** in VS Code (`Ctrl+`).
3. **Clone Your Repository**:
   ```sh
   git clone https://github.com/your-username/your-repo.git
   ```
4. **Open the Cloned Folder**:
   - Go to `File > Open Folder...` and select the cloned repository folder.

### Step 3: Set Up LaTeX Workshop
1. **Configure LaTeX Workshop**:
   - Open the command palette (`Ctrl+Shift+P`).
   - Search for and select `Preferences: Open Settings (JSON)`.
   - Add the following configuration for LaTeX Workshop:
     ```json
     {
       "latex-workshop.latex.tools": [
         {
           "name": "pdflatex",
           "command": "pdflatex",
           "args": [
             "-synctex=1",
             "-interaction=nonstopmode",
             "-file-line-error",
             "%DOC%"
           ]
         },
         {
           "name": "bibtex",
           "command": "bibtex",
           "args": [
             "%DOCFILE%"
           ]
         },
         {
           "name": "latexmk",
           "command": "latexmk",
           "args": [
             "-synctex=1",
             "-interaction=nonstopmode",
             "-file-line-error",
             "-pdf",
             "-outdir=build",
             "%DOC%"
           ]
         }
       ],
       "latex-workshop.latex.recipes": [
         {
           "name": "latexmk",
           "tools": [
             "latexmk"
           ]
         },
         {
           "name": "pdflatex -> bibtex -> pdflatex*2",
           "tools": [
             "pdflatex",
             "bibtex",
             "pdflatex",
             "pdflatex"
           ]
         }
       ],
       "latex-workshop.view.pdf.viewer": "tab",
       "latex-workshop.latex.autoBuild.run": "onFileChange",
       "latex-workshop.latex.clean.enabled": true,
       "latex-workshop.latex.outDir": "build",
       "latex-workshop.view.pdf.external.viewer.command": "xdg-open",
       "latex-workshop.view.pdf.external.viewer.args": [
         "%PDF%"
       ],
       "latex-workshop.view.pdf.external.viewer.enabled": true
     }
     ```

### Step 4: Create and Edit LaTeX Documents
1. **Create a New LaTeX File**:
   - Go to `File > New File` and save it with a `.tex` extension (e.g., `main.tex`).
   - Write your LaTeX code in this file.

2. **Build the LaTeX Document**:
   - Save your `.tex` file (`Ctrl+S`).
   - LaTeX Workshop will automatically build the document if `autoBuild.run` is set to `onFileChange`.
   - To manually build, open the command palette (`Ctrl+Shift+P`), search for `LaTeX Workshop: Build with recipe`, and select your desired recipe.

3. **View the PDF**:
   - LaTeX Workshop should now open the `build/main.pdf` file in a new tab within VS Code. If you want to open it with an external viewer, ensure that the `external.viewer.enabled` setting is true.

### Step 5: Use Git for Version Control
1. **Stage Changes**:
   - Open the Source Control pane (`Ctrl+Shift+G`).
   - Stage your changes by clicking the `+` icon next to the file.

2. **Commit Changes**:
   - Enter a commit message in the message box and click the `✔` icon to commit.

3. **Push Changes**:
   - Push your changes to the remote repository using the push button in the Source Control pane.

### Step 6: Collaborate with Others
1. **Pull Changes**:
   - To update your local repository with changes from the remote repository, use the pull button in the Source Control pane.

2. **Handle Merge Conflicts**:
   - If there are merge conflicts, VS Code will highlight them, allowing you to resolve them directly.

3. **Real-time Collaboration (Optional)**:
   - Use the Live Share extension to collaborate in real-time with others.

### Step 7: Automate with CI/CD (Optional)
1. **Set Up Continuous Integration**:
   - Use a service like GitHub Actions to automatically build your LaTeX documents when changes are pushed to the repository.
   - Create a `.github/workflows/latex.yml` file in your repository with the following content:
     ```yaml
     name: Build LaTeX document

     on: [push]

     jobs:
       build:

         runs-on: ubuntu-latest

         steps:
         - uses: actions/checkout@v2
         - name: Set up TeX Live
           uses: xu-cheng/latex-action@v2
         - name: Compile LaTeX document
           run: latexmk -pdf -file-line-error -interaction=nonstopmode -outdir=build main.tex
     ```

By following these steps, your LaTeX build should output the PDF to the `/build` directory, and LaTeX Workshop should link to `build/main.pdf` correctly. If you still encounter issues, please ensure that the directory structure and file paths are correct and that there are no typos in the configuration.

---

# FAQ

> I changed to iclr template and got this error: `! LaTeX Error: File eso-pic.sty' not found.`

The error message indicates that the `eso-pic.sty` package is missing from your LaTeX installation. This package is required by the ICLR template you're using.

To resolve this, you need to install the missing package. On a Debian-based system (such as Ubuntu), you can install the necessary LaTeX package using the following command:

```sh
sudo apt-get install texlive-latex-extra
```

This package includes `eso-pic.sty` and many other common LaTeX packages that are often required.

### Steps to Install the Missing Package and Compile Again

1. **Install the Required Package**:
   Open a terminal and run:
   ```sh
   sudo apt-get install texlive-latex-extra
   ```

2. **Ensure Your LaTeX Setup is Correct**:
   Make sure your LaTeX Workshop configuration in VS Code is set to build the documents correctly and that the output directory is specified as `/build`.

3. **Retry Compiling Your Document**:
   Open your LaTeX file in VS Code and try building it again.

### Additional Configuration in `settings.json`
Ensure that your VS Code settings are still correctly configured as mentioned before:

```json
{
  "latex-workshop.latex.tools": [
    {
      "name": "pdflatex",
      "command": "pdflatex",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "%DOC%"
      ]
    },
    {
      "name": "bibtex",
      "command": "bibtex",
      "args": [
        "%DOCFILE%"
      ]
    },
    {
      "name": "latexmk",
      "command": "latexmk",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "-pdf",
        "-outdir=build",
        "%DOC%"
      ]
    }
  ],
  "latex-workshop.latex.recipes": [
    {
      "name": "latexmk",
      "tools": [
        "latexmk"
      ]
    },
    {
      "name": "pdflatex -> bibtex -> pdflatex*2",
      "tools": [
        "pdflatex",
        "bibtex",
        "pdflatex",
        "pdflatex"
      ]
    }
  ],
  "latex-workshop.view.pdf.viewer": "tab",
  "latex-workshop.latex.autoBuild.run": "onFileChange",
  "latex-workshop.latex.clean.enabled": true,
  "latex-workshop.latex.outDir": "build",
  "latex-workshop.view.pdf.external.viewer.command": "xdg-open",
  "latex-workshop.view.pdf.external.viewer.args": [
    "%PDF%"
  ],
  "latex-workshop.view.pdf.external.viewer.enabled": true
}
```

After installing the `texlive-latex-extra` package, try building your LaTeX document again. This should resolve the `eso-pic.sty` not found error and allow you to generate the PDF output in the `/build` directory.