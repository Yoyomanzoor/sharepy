# sharepy

Want to share your overly complicated conda environment? Or want to delete all those old conda environments but appease your neuroticism and save a backup? Use this shell script to easily save conda environments!

Requires `conda >= 4.8.0`. If script does not work, try updating conda.

Tested in `x86_64-pc-linux-gnu (64-bit)` and `x86_64-w64-mingw32/x64 (64-bit)` distributions.

## Install and run by adding script to path

1. Download the `sharepy` bash script.

```
sudo wget https://gist.githubusercontent.com/Yoyomanzoor/0ae9008abd4e72384e21098580a9d75b/raw/c7bea25d5b8f57fa94a18e7218604e3b48fc2c8f/sharepy.sh -O /usr/local/bin/sharepy
sudo chmod +x /usr/local/bin/sharepy
```

2. Run `sharepy -e <environment name> -o <output directory>`

## Install and run without adding script to path

This method requires no administrator privileges (although it is still possible to add to path without admin privileges) 

1. Download the `sharepy` bash script via `wget https://gist.githubusercontent.com/Yoyomanzoor/0ae9008abd4e72384e21098580a9d75b/raw/c7bea25d5b8f57fa94a18e7218604e3b48fc2c8f/sharepy.sh`.
2. Run `path/to/sharepy/sharepy.sh -e <environment name> -o <output directory>`

## Install and run by adding to `~/.bashrc`

In your `~/.bashrc` file, you can add this function:

```bash
# sharepy by Yoyomanzoor https://github.com/Yoyomanzoor/sharepy
function sharepy() {
    conda activate $1
    mkdir -p $1
    cd $1
    conda list > $1.txt
    conda list --explicit > $1_explicit.txt 
    conda env export > $1.yml
    conda env export --from-history > $1_cp.yml
    cd ..
}
```

To use this version, run `sharepy <environment name>` in the directory you would like the output.

That's it!

## Output files

Say we have a conda environment called `myenv`.  
There are 4 output files:

File | contents | re-install with conda
-|-|-
myenv/myenv.txt | human-readable table with environment info |
myenv/myenv_explicit.txt | OS-specific list of tar files for all items in the environment | `$ conda create --name <env> --file <this file>`
myenv/myenv.yml | OS-specific yaml file of environment | `$ conda env create -f <this file>`
myenv/myenv_crossplatform.yml | Cross platform yaml file of environment. Be careful! There are known issues sometimes with this, such as not including pip dependencies. | `$ conda env create -f <this file>`