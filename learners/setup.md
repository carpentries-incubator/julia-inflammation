---
title: Setup
---

## Overview

This lesson is designed to be run on a personal computer.
All of the software and data used in this lesson are freely available online,
and instructions on how to obtain them are provided below.

## Install Julia  

To get started, download and install [Julia](https://julialang.org/downloads/) for your operating system.  

## Launch Julia  

To start programming in Julia, choose one of the following options:  

### **Option A: Jupyter Notebook**  

1. Open a terminal (or Command Prompt on Windows).
2. Open **Julia**(Type `julia` and press **Enter**) in your terminal and type:  

    ```julia
    using IJulia
    notebook()
    ```  

2. A browser window will open. Click **New** → **Julia** to start coding.  

---

### **Option B: VS Code / Codium**  

1. Install [VS Code](https://code.visualstudio.com/) or [VSCodium](https://vscodium.com/).  
2. Open VS Code and install the **Julia extension** (search for "Julia" in the Extensions panel).  
3. Open a new file, save it as `script.jl`, and start writing Julia code.  

---

### **Option C: Terminal / REPL**  

1. Open a terminal (or Command Prompt on Windows).  
2. Type `julia` and press **Enter** to open Julia’s interactive mode.  
3. Start typing commands, or run a script with:  

    ```bash
    julia myscript.jl
    ```  


## Download

You also need to download some files to follow this lesson:

1. Download [juliainflammation-data.zip](data/juliainflammation-data.zip).
2. Create a folder called `julia-novice-inflammation` on your Desktop.
3. Move downloaded files to `julia-novice-inflammation`.
4. Unzip the files.
