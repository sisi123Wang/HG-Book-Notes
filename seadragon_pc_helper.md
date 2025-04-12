## 💻 Using Seadragon on a PC: A Guide for Windows Users

Most of my classmates use MacBooks, but as a PC user, I’ve had to troubleshoot my way through several challenges while accessing and working on **Seadragon**, our HPC cluster. Here's a collection of problems I encountered—and how I fixed (or worked around) them.

---

### 0. Copying the RStudio `.lsf` Script

**Problem:**\
When copying the RStudio job script, I couldn’t just copy and paste the command into my PC terminal. This made it really easy to forget small details.

Command:

```bash
cp /risapps/singularity/repo/RStudio/4.4.1/rserver_4.4.1.lsf $HOME/
```

**Common Pitfalls for PC users:**

- There’s a **space after** `cp` **and before** `$HOME/` – don’t forget it!
- If you get an error like `No such file or directory`, it may be because:
  - You **forgot the ****************************************************************************************`$`**************************************************************************************** sign** before `HOME`
  - You **accidentally removed or changed the trailing space** after the filename

Use keyboard shortcuts like `Ctrl+Shift+C/V` in your terminal to reduce errors.

---

### 1. VPN

**Problem:**\
Frequent disconnections due to VPN timeouts, personal Wi-Fi instability, and unstable connections during large job submissions or downloads.

**Fixes:**

- Monitor VPN session limits; re-login before timeout during long jobs.
- Make sure your home or dorm Wi-Fi is stable; try restarting the router or switching to Ethernet if possible.

---

###  2. RStudio Server Access on Seadragon

**Problem:**
Couldn’t connect to RStudio Server from a browser (localhost:8787 didn't work, or got "job not found" errors with `bpeek` and `bjobs`).

Sometimes the issue is caused by **Windows-style carriage returns** in `.lsf` files created or edited on a PC. To check this, run:

```bash
file rserver_4.4.1.lsf
```

If the result includes `CRLF line terminators`, you’ll need to convert the file to Unix format:

```bash
dos2unix *.lsf
```

Run this in your `$HOME` directory where the `.lsf` files are stored. Then re-submit your RStudio job.

This method also converts the file to Unix format if you you see errors like `ix: Failed to change the owner` or `ix: problems converting file`, it could indicate permission issues with the file or directory.

You can manually remove the CRLF line endings using Vim:

```bash
vim rserver_4.4.1.lsf
```

Once inside Vim:

1. Press `i` to enter **insert mode**.
2. Type `:set ff=unix` and press `Enter`.
3. Press `Esc` to leave insert mode.
4. Type `:wq` and press `Enter` to write and quit.

If the problem of the job ending really fast still persists, here are some troubleshooting tips shared by Rong Yao from our HPC department:

1. **Check if X11 is enabled when logging into Seadragon.** Before submitting the job, on the login node, run:

   ```bash
   echo $DISPLAY
   ```

   You should see something like:

   ```
   localhost:16.0
   ```

   If not, X11 might not be enabled properly. You can refer to the official documentation for more help: [RStudio Container Documentation](https://hpcweb.mdanderson.edu/applications.dir/r.dir/RStudio_Container.pdf)

2. **Clean up your RStudio logs:**
   Navigate to your RStudio log directory:

   ```bash
   cd $HOME/logs/rstudio/lib
   ```

   Then clean up the contents. At the time this worked for me, I only had one file in that directory.

**Fixes:**

- Make sure you submit with the correct `.lsf` job script.
- If `bpeek` returns "Job not found", check if the job finished quickly or failed before you peeked. Use `bjobs -a` to see finished jobs.
- Use the right log path: check `/home/username/logs/rstudio/your_output_file.log`.

---

### 3. R Package Installation Issues

 **Hint:** Sometimes, simply removing the problematic package and reinstalling it works surprisingly well on a PC. Use `remove.packages("packageName")` followed by `install.packages("packageName")` to try this quick fix.

**Problem 1:**
You see a message like:

> *A version of this package for your version of R might be available elsewhere...*

**Solution:**
This means your version of R may not be compatible with the latest CRAN version. Always **check the compatibility of your RStudio version with the package**. You may need to:

- Use an archived version of the package
- Switch to Bioconductor
- Update or downgrade your R version if possible

**Problem 2:**
Installing packages like `gdsfmt` failed because the package is no longer on CRAN or due to permission problems.

**Solution:**
Use Bioconductor for `gdsfmt`:

```R
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("gdsfmt")
```

**Problem 3:**

> *ERROR: lazy loading failed for packages*

**Solution:**
Try installing the package without the test load using the `INSTALL_opts` parameter:

```R
install.packages("YourPackageHere", 
                 repos = NULL, 
                 type = "source", 
                 lib = "/home/swang25/R/ubuntu/4.4.1",
                 INSTALL_opts = c("--no-multiarch", "--no-test-load"))
```

**Problem 4:**
Permission errors like `00LOCK-*`

**Solution:**
Remove the lock file manually:

```bash
rm -rf ~/R/ubuntu/4.4.1/00LOCK-*
```

---

### 4. Job Submission & Debugging

**Problem:**\
Submitted jobs didn’t run or threw errors like “Job not found” or “permission denied.”

**Fixes:**

- Double-check your `.lsf` file: use the right partition (`#BSUB -q short`, etc.)
- Look at error and output logs in the `logs/` directory.
- Use `ps -ef | grep rserver` to see if the RStudio server is running.

---

### 5. Rscript Command Not Found

**Problem:**\
In your `.err` file, If you see:

```
Rscript: command not found
```

This means that when your LSF job runs, it cannot find the `Rscript` command. This typically happens if your `PATH` isn’t set correctly on the compute node.

**Solution:**\
On your login node (e.g., using PuTTY), run:

```bash
which Rscript
```

If it returns something like:

```bash
/risapps/rhel8/R/4.4.1/bin/Rscript
```

Then you need to update your `.lsf` script to use the full path:

```bash
/risapps/rhel8/R/4.4.1/bin/Rscript /path/to/your/script.R
```

- Double-check your `.lsf` file: use the right partition (`#BSUB -q short`, etc.)
- Look at error and output logs in the `logs/` directory.
- Use `ps -ef | grep rserver` to see if the RStudio server is running.

---

###  6. Working with R inside the Cluster

**Problem 1:**\
Conflicts between Seadragon’s RedHat-based R environment and packages built for Ubuntu.

**Solution:**\
Install packages into your local R lib (e.g., `~/R/ubuntu/4.4.1`). Also, specify the library path when installing to avoid installing to system directories(modift it in your lsf file): export R\_LIBS\_USER="\~/R/library"

**Problem 2:**\
GLIBC incompatibility issues due to outdated system libraries.

**Solution:**\
You can solve the GLIBC incompatibility issues by providing an environment with newer system libraries. Modify your LSF script (or run interactively) to use the Singularity image.

**Problem 3:**\
Singularity module not loaded by default.

**Solution:**\
In your `.lsf` file, add:

```bash
module load singularity
```

---

### 7. Shortcuts & Terminal Tips for PC Users

- **Copy/paste in terminal:** Use `Ctrl+Shift+C` and `Ctrl+Shift+V`.
- **Find your login node:** Run `hostname` or look for the prompt in your terminal.
- **Check current directory:** Use `pwd` (print working directory).
- **View file contents:** Use `cat filename`, `less filename`, or `head filename`.

---

### Final Thoughts

It’s totally doable to work on Seadragon using a PC—you just have to be ready to deal with a few quirks. I hope this saves you some time and frustration!

**Need more help?**\
If you run into problems not listed here, I highly recommend submitting a ticket to the **HPC team through the 4info website**. They’re super helpful and usually respond quickly. In my experience, they replied within a day!
...

